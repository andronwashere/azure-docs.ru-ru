---
title: Руководство. Развертывание выделенных устройств HSM Azure в существующей виртуальной сети с помощью PowerShell | Документация Майкрософт
description: В этом руководстве описано развертывание выделенного устройства HSM с помощью PowerShell в существующей виртуальной сети
services: dedicated-hsm
documentationcenter: na
author: barclayn
manager: barbkess
editor: ''
ms.service: key-vault
ms.topic: tutorial
ms.custom: mvc, seodec18
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/07/2018
ms.author: barclayn
ms.openlocfilehash: 581ce6d75df8f42bb72bbfc93e85684d97620e3a
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/23/2019
ms.locfileid: "66158986"
---
# <a name="tutorial--deploying-hsms-into-an-existing-virtual-network-using-powershell"></a>Руководство. Развертывание выделенных устройств HSM в существующей виртуальной сети с помощью PowerShell

Служба выделенных устройств HSM Azure предоставляет физическое устройство в исключительное пользование клиента со всеми возможностями административного контроля и под полную ответственность за управление устройством. Так как в этом случае предоставляется физическое оборудование, корпорация Майкрософт должна контролировать выделение этих устройств, чтобы обеспечить эффективное управление емкостью. Поэтому в обычной подписке Azure служба выделенных устройств HSM недоступна для подготовки ресурсов. Клиентам Azure, которым требуется доступ к службе выделенных устройств HSM, сначала следует связаться со своим менеджером Майкрософт по работе с клиентами и запросить регистрацию для использования службы выделенных устройств HSM. Только после этого можно будет подготовить службу к работе.
В этом руководстве демонстрируется стандартный процесс подготовки к работе. При этом подразумевается следующее:

- у клиента есть виртуальная сеть;
- у клиента есть виртуальная машина;
- клиенту нужно добавить ресурсы HSM в существующую среду.

Стандартная высокодоступная архитектура с развертыванием в нескольких регионах может выглядеть следующим образом:

![Развертывание в нескольких регионах](media/tutorial-deploy-hsm-powershell/high-availability.png)

В этом руководстве описана интеграция пары устройств HSM и требуемого шлюза ExpressRoute (см. Subnet 1 на предыдущем рисунке) в существующую виртуальную сеть (см. VNET 1 на предыдущем рисунке).  Все другие ресурсы — это стандартные ресурсы Azure. Аналогичный процесс интеграции можно использовать для устройств HSM в подсети 4 виртуальной сети 3 (см. представленный выше рисунок).


[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Предварительные требования

Сейчас служба выделенных устройств Azure недоступна на портале Azure, поэтому все операции с ней осуществляются в командной строке или с помощью PowerShell. В этом руководстве используется PowerShell в Azure Cloud Shell. Если вы не знакомы с PowerShell, см. статью [Get started with Azure PowerShell](https://docs.microsoft.com/powershell/azure/get-started-azureps) (Приступая к работе с Azure PowerShell)

Предполагается, что:

- Вы зарегистрировались в службе выделенных устройств HSM Azure и получили одобрение на ее использование. Если это не так, свяжитесь со своим менеджером Майкрософт по работе с клиентами и запросите у него соответствующие сведения. 
- Вы создали группу ресурсов, которая включает нужные ресурсы. Новые ресурсы, развернутые в рамках этого руководства, будут добавлены в эту группу.
- Вы создали необходимую виртуальную сеть, подсеть и виртуальные машины, как показано на схеме выше, и теперь хотите интегрировать два устройства HSM в эту среду.

Для выполнения всех приведенных ниже инструкций требуется, чтобы вы открыли портал Azure и запустили Cloud Shell (щелкните значок "\>\_" в правом верхнем углу портала).

## <a name="provisioning-a-dedicated-hsm"></a>Подготовка выделенного устройства HSM к работе

Проверка выделенных устройств HSM и их интеграция с существующей сетью посредством шлюза ExpressRoute будет осуществляться с помощью программы командной строки SSH. Это позволит убедиться, что устройство HSM доступно по сети и готово к дальнейшей настройке. В приведенных ниже командах используется шаблон Resource Manager, чтобы создать ресурсы HSM и связанные с ними сетевые ресурсы.

### <a name="validating-feature-registration"></a>Проверка регистрации функции

Как указано выше, перед подготовкой к работе необходимо зарегистрировать службу выделенных устройств HSM в своей подписке. Чтобы проверить это, выполните следующую команду в Cloud Shell на портале Azure. 

```powershell
Get-AzProviderFeature -ProviderNamespace Microsoft.HardwareSecurityModules -FeatureName AzureDedicatedHsm
```

Следующая команда позволяет проверить сетевые возможности, необходимые для службы выделенных устройств HSM.

```powershell
Get-AzProviderFeature -ProviderNamespace Microsoft.Network -FeatureName AllowBaremetalServers
```

Обе команды должны вернуть состояние Registered (как показано ниже), чтобы вы могли продолжить работу.  Если вам нужно зарегистрироваться для использования службы, свяжитесь со своим менеджером Майкрософт по работе с клиентами.

![Состояние подписки](media/tutorial-deploy-hsm-powershell/subscription-status.png)

### <a name="creating-hsm-resources"></a>Создание ресурсов HSM

Устройство HSM подготавливается к работе в виртуальной сети клиента. При этом подразумевается использование подсети. Для использования HSM требуется шлюз ExpressRoute, который обеспечивает обмен данными между виртуальной сетью и физическим устройством. Также необходима виртуальная машина, которая осуществляет доступ к устройству HSM с помощью клиентского ПО Gemalto. Чтобы упростить использование, эти ресурсы собраны в файл шаблона и связанный с ним файл параметров. Эти файлы можно получить, написав в корпорацию Майкрософт по адресу HSMrequest@Microsoft.com.

Когда вы получите эти файлы, измените файл параметров, чтобы задать любые имена ресурсов. Измените строки, в которых ключи "value" равны "".

- `namingInfix` — префикс для имен ресурсов HSM.
- `ExistingVirtualNetworkName` — имя виртуальной сети, используемой устройствами HSM.
- `DedicatedHsmResourceName1` — имя ресурса HSM в метке 1 центра обработки данных.
- `DedicatedHsmResourceName2` — имя ресурса HSM в метке 2 центра обработки данных.
- `hsmSubnetRange` — диапазон IP-адресов в подсети для устройств HSM.
- `ERSubnetRange` — диапазон IP-адресов в подсети для шлюза виртуальной сети.

Пример указанных настроек:

```json
{
"$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "namingInfix": {
      "value": "MyHSM"
    },
    "ExistingVirtualNetworkName": {
      "value": "MyHSM-vnet"
    },
    "DedicatedHsmResourceName1": {
      "value": "HSM1"
    },
    "DedicatedHsmResourceName2": {
      "value": "HSM2"
    },
    "hsmSubnetRange": {
      "value": "10.0.2.0/24"
    },
    "ERSubnetRange": {
      "value": "10.0.255.0/26"
    },
  }
}
```

На основе соответствующего файла шаблона Resource Manager и указанных сведений будет создано 6 ресурсов:

- подсеть для устройств HSM в указанной виртуальной сети;
- подсеть для шлюза виртуальной сети; 
- шлюз виртуальной сети, который связывает виртуальную сеть с устройствами HSM;
- общедоступный IP-адрес шлюза;
- устройство HSM в метке 1;
- устройство HSM в метке 2.

Когда значения параметров будут заданы, эти файлы нужно отправить в файловый ресурс Cloud Shell на портале Azure. На портале Azure щелкните символ Cloud Shell "\>\_" в правом верхнем углу, чтобы открыть окно командной оболочки в нижней части экрана. В качестве командной оболочки можно использовать BASH и PowerShell. Нужно выбрать BASH, если вы не сделали этого ранее.

В этой командной оболочке есть команда отправки и загрузки файлов на панели инструментов. Воспользуйтесь этой командой, чтобы отправить файл шаблона и файл параметров в свой файловый ресурс:

![Файловый ресурс](media/tutorial-deploy-hsm-powershell/file-share.png)

После отправки файлов можно создать ресурсы.
Прежде чем создавать ресурсы HSM, нужно создать другие необходимые ресурсы. Нужно создать виртуальную сеть с диапазонами подсетей для вычислительных ресурсов, устройств HSM и шлюза. Ниже приведены примеры команд по созданию такой виртуальной сети.

```powershell
$compute = New-AzVirtualNetworkSubnetConfig `
  -Name compute `
  -AddressPrefix 10.2.0.0/24
```

```powershell
$delegation = New-AzDelegation `
  -Name "myDelegation" `
  -ServiceName "Microsoft.HardwareSecurityModules/dedicatedHSMs"

```

```powershell
$hsmsubnet = New-AzVirtualNetworkSubnetConfig ` 
  -Name hsmsubnet ` 
  -AddressPrefix 10.2.1.0/24 ` 
  -Delegation $delegation 

```

```powershell

$gwsubnet= New-AzVirtualNetworkSubnetConfig `
  -Name GatewaySubnet `
  -AddressPrefix 10.2.255.0/26

```

```powershell

New-AzVirtualNetwork `
  -Name myHSM-vnet `
  -ResourceGroupName myRG `
  -Location westus `
  -AddressPrefix 10.2.0.0/16 `
  -Subnet $compute, $hsmsubnet, $gwsubnet

```

>[!NOTE]
>Самым важным моментом при создании виртуальной сети, на который нужно обратить внимание, является то, что параметр delegations для подсети устройства HSM должен иметь значение Microsoft.HardwareSecurityModules/dedicatedHSMs.  Иначе подготовка устройств HSM к работе будет невозможна.

После того как все предварительные условия будут выполнены, запустите следующую команду для использования шаблона Resource Manager, убедившись, что всем параметрам присвоены уникальные значения (по крайней мере, имя группы ресурсов):

```powershell

New-AzResourceGroupDeployment -ResourceGroupName myRG `
     -TemplateFile .\Deploy-2HSM-toVNET-Template.json `
     -TemplateParameterFile .\Deploy-2HSM-toVNET-Params.json `
     -Name HSMdeploy -Verbose

```

Выполнение этой команды занимает примерно 20 минут. Параметр -verbose обеспечивает непрерывное отображение состояния.

![Состояние подготовки](media/tutorial-deploy-hsm-powershell/progress-status.png)

После успешного завершения отобразится сообщение "provisioningState": "Succeeded". Теперь можно подключиться к существующей виртуальной машине и проверить доступность устройства HSM с помощью SSH.

## <a name="verifying-the-deployment"></a>Проверка развертывания

Чтобы убедиться, что устройства подготовлены к работе, и просмотреть их параметры, запустите указанный ниже набор команд. Убедитесь, что группа ресурсов настроена соответствующим образом и имя ресурса совпадает с указанным в файле параметров.

```powershell

$subid = (Get-AzContext).Subscription.Id
$resourceGroupName = "myRG"
$resourceName = "HSM1"  
Get-AzResource -Resourceid /subscriptions/$subId/resourceGroups/$resourceGroupName/providers/Microsoft.HardwareSecurityModules/dedicatedHSMs/$resourceName

```

![Состояние подготовки к работе](media/tutorial-deploy-hsm-powershell/progress-status2.png)

Теперь вы можете просмотреть ресурсы с помощью [обозревателя ресурсов Azure](https://resources.azure.com/).   В обозревателе ресурсов разверните раздел "Подписки" слева, подписку для службы выделенных устройств HSM, раздел "Группы ресурсов", используемую группу ресурсов и выберите элемент "Ресурсы".

## <a name="testing-the-deployment"></a>Проверка развертывания

Чтобы проверить развертывание, подключитесь к виртуальной машине, которая осуществляет доступ к устройству HSM, а затем подключитесь непосредственно к устройству HSM. Это позволит убедиться, что устройство HSM доступно.
Для подключения к виртуальной машине используется средство SSH. Используйте приведенную ниже команду, заменив имя администратора и имя DNS значениями, указанными в вашем файле параметров.

`ssh adminuser@hsmlinuxvm.westus.cloudapp.azure.com`

Используйте пароль, указанный в файле параметров.
После входа на виртуальную машину Linux вы можете войти в устройство HSM, воспользовавшись частным IP-адресом, который указан на портале для ресурса \<префикс>hsm_vnic.

```powershell

(Get-AzResource -ResourceGroupName myRG -Name HSMdeploy -ExpandProperties).Properties.networkProfile.networkInterfaces.privateIpAddress

```
Узнав нужный IP-адрес, выполните следующую команду:

`ssh tenantadmin@<ip address of HSM>`

При успешном выполнении отобразится запрос пароля. Пароль по умолчанию — PASSWORD. По запросу устройства HSM нужно будет изменить пароль на более надежный и воспользоваться механизмом для хранения пароля и предотвращения его утечки, используемым в вашей организации.  

>[!IMPORTANT]
>Если вы забудете пароль, настройки устройства HSM нужно будет сбросить, что приведет к потере ключей.

Когда вы подключитесь к устройству HSM с помощью SSH, запустите приведенную ниже команду, чтобы проверить работоспособность устройства.

`hsm show`

Выходные данные должны быть аналогичны показанным на снимке экрана ниже.

![Состояние подготовки к работе](media/tutorial-deploy-hsm-powershell/output.png)

На этом этапе вы выделили все ресурсы для высокодоступного развертывания двух устройств HSM, а также убедились, что они доступны и работают. Дальнейшие операции по конфигурации или проверке требуют взаимодействия с самим устройством HSM. Для этого выполните инструкции, изложенные в разделе 7 руководства администратора Gemalto Luna Network HSM 7, где описаны действия по инициализации устройства HSM и созданию разделов. Вся документация и ПО доступны непосредственно на веб-сайте Gemalto после регистрации на портале поддержки клиентов Gemalto и получения идентификатора клиента. Скачайте клиентское программное обеспечение версии 7.2, чтобы получить все необходимые компоненты.

## <a name="delete-or-clean-up-resources"></a>Удаление или очистка ресурсов

После того как вы завершили работу с устройством HSM, его можно удалить как ресурс и вернуть в пул свободных устройств. Очевидная проблема при этом — возможное наличие конфиденциальных данных клиента на устройстве. Чтобы удалить конфиденциальные данные клиента с устройства, нужно сбросить настройки устройства до заводских с помощью клиентского ПО Gemalto. Ознакомьтесь с руководством администратора для устройства Gemalto SafeNet Network Luna 7 и выполните приведенные ниже команды в указанном порядке.

1. `hsm factoryReset -f`
2. `sysconf config factoryReset -f -service all`
3. `network interface delete -device eth0`
4. `network interface delete -device eth1`
5. `network interface delete -device eth2`
6. `network interface delete -device eth3`
7. `my file clear -f`
8. `my public-key clear -f`
9. `syslog rotate`


> [!NOTE]
> При возникновении проблем с конфигурацией устройств Gemalto обратитесь в [службу поддержки клиентов Gemalto](https://safenet.gemalto.com/technical-support/).

Если вы закончили работу с ресурсами в этой группе ресурсов, их можно удалить с помощью приведенной ниже команды.

```powershell

$subid = (Get-AzContext).Subscription.Id
$resourceGroupName = "myRG" 
$resourceName = "HSMdeploy"  
Remove-AzResource -Resourceid /subscriptions/$subId/resourceGroups/$resourceGroupName/providers/Microsoft.HardwareSecurityModules/dedicatedHSMs/$resourceName 

```

## <a name="next-steps"></a>Дополнительная информация

После выполнения действий, описанных в этом руководстве, ресурсы выделенных устройств HSM будут готовы к работе и доступны в вашей виртуальной сети. Теперь вы можете добавить в это развертывание дополнительные ресурсы в соответствии с требованиями вашей архитектуры развертывания. Дополнительные сведения о планировании развертывания см. в базовой документации. Рекомендуем использовать схему с двумя устройствами HSM в основном регионе, чтобы обеспечить доступность на уровне стойки, и двумя устройствами HSM в другом регионе, чтобы обеспечить доступность на уровне региона. Файл шаблона, который использовался в этом руководстве, может служить образцом для развертывания двух устройств HSM, но его параметры нужно изменить в соответствии с вашими требованиями.

* [Высокая доступность](high-availability.md)
* [Физическая безопасность](physical-security.md)
* [Сеть](networking.md)
* [Мониторинг](monitoring.md)
* [Возможности поддержки](supportability.md)
