---
title: Создание шлюза приложений Azure — шаблоны
description: В этой статье приводятся инструкции по созданию шлюза приложений Azure с помощью шаблона Azure Resource Manager
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: conceptual
ms.date: 6/26/2019
ms.author: victorh
ms.openlocfilehash: a762e8c9ed1981173f3729837456ac2cfea081b8
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/28/2019
ms.locfileid: "67449540"
---
# <a name="create-an-application-gateway-using-the-azure-resource-manager-template"></a>Создание шлюза приложений с помощью шаблона Azure Resource Manager

Шлюз приложений — это балансировщик нагрузки уровня 7. Он отвечает за отработку отказов и эффективную маршрутизацию HTTP-запросов между разными серверами (облачными и локальными). Шлюз приложений выполняет многие функции контроллера доставки приложений (ADC), включая балансировку нагрузки HTTP, определение сходства сеансов на основе файлов cookie, разгрузку SSL, выполнение пользовательской проверки работоспособности, поддержку нескольких сайтов и т. д. Полный список поддерживаемых функций представлен в [обзоре шлюза приложений](application-gateway-introduction.md).

В этой статье описывается скачивание и изменения существующего [шаблона Azure Resource Manager](../azure-resource-manager/resource-group-authoring-templates.md) из GitHub, а также развернуть шаблон из GitHub, Azure PowerShell и Azure CLI.

Если вы просто развертываете шаблон непосредственно из GitHub без изменений, перейдите к развертыванию шаблона из GitHub.

## <a name="scenario"></a>Сценарий

Из этой статьи вы узнаете:

* как создать шлюз приложений с брандмауэром веб-приложения;
* как создать виртуальную сеть с именем VirtualNetwork1 и зарезервированным блоком CIDR (10.0.0.0/16);
* как создать подсеть с именем Appgatewaysubnet и блоком CIDR (10.0.0.0/28);
* как задать два ранее настроенных внутренних IP-адреса для веб-серверов, которые будут балансировать трафик. В этом примере в качестве внутренних IP-адресов используются адреса 10.0.1.10 и 10.0.1.11.

> [!NOTE]
> Приведенные значения представляют в этом шаблоне параметры. Чтобы настроить шаблон, можно изменить правила, прослушиватель, SSL и другие параметры в файле azuredeploy.json.

![Сценарий](./media/application-gateway-create-gateway-arm-template/scenario.png)

## <a name="download-and-understand-the-azure-resource-manager-template"></a>Скачивание и использование шаблона диспетчера ресурсов Azure

Вы можете скачать из GitHub уже существующий шаблон диспетчера ресурсов Azure (ARM), чтобы создать виртуальную сеть и две подсети, а также внести в него нужные изменения и применить. Для этого выполните следующие действия.

1. Перейдите к статье [Create Application Gateway with web application firewall enabled](https://github.com/Azure/azure-quickstart-templates/tree/master/101-application-gateway-waf) (Создание шлюза приложения с включенным брандмауэром веб-приложений).
1. Щелкните **azuredeploy.json** и нажмите кнопку **RAW**.
1. Сохраните файл в локальную папку на своем компьютере.
1. Если вы знакомы с шаблонами Azure Resource Manager, перейдите к шагу 7.
2. Откройте только что сохраненный файл и просмотрите содержимое раздела **parameters** в строке
3. В параметрах шаблона ARM есть заполнитель для значений, которые могут подставляться во время развертывания.

   | Параметр | Описание |
   | --- | --- |
   | **subnetPrefix** |Блок CIDR для подсети шлюза приложений. |
   | **applicationGatewaySize** | Размер шлюза приложений.  WAF позволяет только средний и крупный. |
   | **backendIpaddress1** |IP-адрес первого веб-сервера. |
   | **backendIpaddress2** |IP-адрес второго веб-сервера. |
   | **wafEnabled** | Параметр, определяющий, включен ли WAF.|
   | **wafMode** | Режим брандмауэра веб-приложения.  Доступные варианты: **Предотвращение** или **Защита**.|
   | **wafRuleSetType** | Тип набора правил для WAF.  В настоящее время поддерживается только OWASP. |
   | **wafRuleSetVersion** |Версия набора правил. Поддерживаются только OWASP CRS 2.2.9 и 3.0. |

1. Проверьте содержимое раздела **resources** и обратите внимание на следующие свойства.

   * **type**. Тип ресурса, созданного на основе шаблона. В этом случае используется тип `Microsoft.Network/applicationGateways`, представляющий шлюз приложений.
   * **name**. Имя ресурса. Обратите внимание на применение `[parameters('applicationGatewayName')]`. Эта строка кода означает, что имя предоставляется пользователем или извлекается из файла параметров при развертывании.
   * **properties**. Список свойств для ресурса. Во время создания шлюза приложений этот шаблон использует виртуальную сеть и общедоступный IP-адрес.

1. Вернитесь к [https://github.com/Azure/azure-quickstart-templates/blob/master/101-application-gateway-waf/](https://github.com/Azure/azure-quickstart-templates/blob/master/101-application-gateway-waf).
1. Щелкните **azuredeploy-parameters.json** и нажмите кнопку **RAW**.
1. Сохраните файл в локальную папку на своем компьютере.
1. Откройте сохраненный файл и измените значения параметров. Чтобы развернуть шлюз приложений в соответствии с задачами этого руководства, используйте следующие значения.

     ```json
     {
         "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
         "contentVersion": "1.0.0.0",
         "parameters": {
             "addressPrefix": {
             "value": "10.0.0.0/16"
             },
             "subnetPrefix": {
             "value": "10.0.0.0/28"
             },
             "applicationGatewaySize": {
             "value": "WAF_Medium"
             },
             "capacity": {
             "value": 2
             },
             "backendIpAddress1": {
             "value": "10.0.1.10"
             },
             "backendIpAddress2": {
             "value": "10.0.1.11"
             },
             "wafEnabled": {
             "value": true
             },
             "wafMode": {
             "value": "Detection"
             },
             "wafRuleSetType": {
             "value": "OWASP"
             },
             "wafRuleSetVersion": {
             "value": "3.0"
             }
         }
     }
     ```

1. Сохраните файл. Можно проверить шаблон JSON и параметра шаблона с помощью инструментов проверки JSON как [JSlint.com](https://www.jslint.com/).

## <a name="deploy-the-azure-resource-manager-template-using-azure-powershell"></a>Развертывание шаблона Azure Resource Manager, с помощью Azure PowerShell

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Если вы никогда не использовали Azure PowerShell, см. в разделе: [установке и настройке этой среды](/powershell/azure/overview). Следуйте инструкциям в статье, чтобы войти в Azure и выбрать подписку.

1. Подключение к Azure

    ```azurepowershell
    Connect-AzAccount
    ```

1. Просмотрите подписки учетной записи.

    ```azurepowershell
    Get-AzSubscription
    ```

    Вам будет предложено указать свои учетные данные для проверки подлинности.

1. Выберите, какие подписки Azure будут использоваться.

    ```azurepowershell
    Select-AzSubscription -Subscriptionid "GUID of subscription"
    ```

1. При необходимости создайте группу ресурсов с помощью **New-AzureResourceGroup** командлета. В примере ниже создается группа ресурсов с именем AppgatewayRG, расположенная в восточной части США.

    ```azurepowershell
    New-AzResourceGroup -Name AppgatewayRG -Location "West US"
    ```

1. Запустите **New AzResourceGroupDeployment** командлет, чтобы развернуть новую виртуальную сеть, с помощью шаблона и параметров файлов вы скачали и изменили.
    
    ```azurepowershell
    New-AzResourceGroupDeployment -Name TestAppgatewayDeployment -ResourceGroupName AppgatewayRG `
    -TemplateFile C:\ARM\azuredeploy.json -TemplateParameterFile C:\ARM\azuredeploy-parameters.json
    ```

## <a name="deploy-the-azure-resource-manager-template-using-the-azure-cli"></a>Развертывание шаблона Azure Resource Manager, с помощью Azure CLI

Чтобы развернуть шаблон Azure Resource Manager, загруженный с помощью Azure CLI, выполните следующие действия.

1. Если вы еще не использовали Azure CLI, ознакомьтесь со статьей [Установка и настройка CLI Azure](/cli/azure/install-azure-cli) и следуйте инструкциям вплоть до выбора учетной записи Azure и подписки.

1. При необходимости выполните команду `az group create`, как показано во фрагменте кода ниже, чтобы создать группу ресурсов. Обратите внимание на результат выполнения команды. В списке, который откроется после выполнения команды, будут указаны используемые параметры. Дополнительные сведения о группах ресурсов см. в статье [Общие сведения о диспетчере ресурсов Azure](../azure-resource-manager/resource-group-overview.md).

    ```azurecli
    az group create --location westus --name appgatewayRG
    ```
    
    **-n (или --name)** . Имя для новой группы ресурсов. В нашем примере это *appgatewayRG*.
    
    **-l (или --location)** . Регион Azure, в котором создается группа ресурсов. В нашем примере это *westus*.

1. Запустите `az group deployment create` командлет, чтобы развернуть новую виртуальную сеть, с помощью шаблона и параметров файлов вы скачали и изменили на предыдущем шаге. В списке, который откроется после выполнения команды, будут указаны используемые параметры.

    ```azurecli
    az group deployment create --resource-group appgatewayRG --name TestAppgatewayDeployment --template-file azuredeploy.json --parameters @azuredeploy-parameters.json
    ```

## <a name="deploy-the-azure-resource-manager-template-using-click-to-deploy"></a>Развертывание шаблона Azure Resource Manager, с помощью нажмите кнопку развертывания

Развертывание с помощью кнопки развертывания — еще один способ использования шаблонов ARM. Он позволяет быстро и удобно работать с шаблонами на портале Azure.

1. Перейдите к статье о [создании шлюза приложений с брандмауэром веб-приложения](https://azure.microsoft.com/documentation/templates/101-application-gateway-waf/).

1. Нажмите кнопку **Развернуть в Azure**.

    ![Развертывание в Azure](./media/application-gateway-create-gateway-arm-template/deploytoazure.png)
    
1. На портале укажите параметры шаблона развертывания и нажмите кнопку **OК**.

    ![Параметры](./media/application-gateway-create-gateway-arm-template/ibiza1.png)
    
1. Установите флажок **Я принимаю указанные выше условия** и нажмите кнопку **Приобрести**.

1. На странице развертывания пользовательских щелкните **создать**.

## <a name="providing-certificate-data-to-resource-manager-templates"></a>Добавление данных сертификата в шаблоны Resource Manager

При использовании протокола SSL с шаблоном сертификат необходимо указать в виде строки в формате base64, а не передать его. Для преобразования PFX- или CER-файла в строку base64, используйте одну из следующих команд. Следующие команды преобразуют сертификат в строку base64, которую можно указать для шаблона. Ожидаемым результатом является строка, которую можно сохранить в переменной и вставить в шаблон.

### <a name="macos"></a>macOS

```bash
cert=$( base64 <certificate path and name>.pfx )
echo $cert
```

### <a name="windows"></a>Windows
```powershell
[System.Convert]::ToBase64String([System.IO.File]::ReadAllBytes("<certificate path and name>.pfx"))
```

## <a name="delete-all-resources"></a>Удаление всех ресурсов

Чтобы удалить все ресурсы, созданные в этой статье, выполните одно из следующих действий:

### <a name="azure-powershell"></a>Azure PowerShell

```azurepowershell
Remove-AzResourceGroup -Name appgatewayRG
```

### <a name="azure-cli"></a>Инфраструктура CLI Azure

```azurecli
az group delete --name appgatewayRG
```

## <a name="next-steps"></a>Дальнейшие действия

Если вы хотите настроить разгрузку SSL, см. в разделе: [Настройка шлюза приложений для разгрузки SSL с помощью классической модели развертывания](application-gateway-ssl.md).

Если вы хотите настроить шлюз приложений для использования с внутренним балансировщиком нагрузки, см. в разделе: [Создание шлюза приложений с помощью шаблона диспетчера ресурсов Azure](application-gateway-ilb.md).

Дополнительные сведения о параметрах балансировки нагрузки в целом см. в статьях:

* [Подсистема балансировщика нагрузки Azure](https://azure.microsoft.com/documentation/services/load-balancer/)
* [Azure Traffic Manager](https://azure.microsoft.com/documentation/services/traffic-manager/)