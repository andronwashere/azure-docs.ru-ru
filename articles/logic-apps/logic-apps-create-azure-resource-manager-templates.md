---
title: Создание шаблонов приложений логики для развертывания Azure Logic Apps
description: Узнайте, как создавать шаблоны Azure Resource Manager для автоматизации развертывания в Azure Logic Apps
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
ms.topic: article
ms.date: 07/26/2019
ms.openlocfilehash: 7d216a3706c13a5fff312850e244a521ab22ae9e
ms.sourcegitcommit: 04ec7b5fa7a92a4eb72fca6c6cb617be35d30d0c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/22/2019
ms.locfileid: "68386542"
---
# <a name="create-azure-resource-manager-templates-to-automate-deployment-for-azure-logic-apps"></a>Создание шаблонов Azure Resource Manager для автоматизации развертывания Azure Logic Apps

Чтобы помочь вам автоматизировать создание и развертывание приложения логики, в этой статье описываются способы создания [шаблона Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) для приложения логики. Общие сведения о структуре и синтаксисе шаблона, содержащего определение рабочего процесса и другие ресурсы, необходимые для развертывания, см. в [разделе Обзор: Автоматизируйте развертывание приложений логики с помощью](logic-apps-azure-resource-manager-templates-overview.md)шаблонов Azure Resource Manager.

Azure Logic Apps предоставляет предварительно [созданные приложения логики Azure Resource Manager шаблона](https://github.com/Azure/azure-quickstart-templates/blob/master/101-logic-app-create/azuredeploy.json) , которые можно повторно использовать, не только для создания приложений логики, но и для определения ресурсов и параметров, используемых для развертывания. Вы можете использовать этот шаблон для собственных бизнес-сценариев или настроить его в соответствии со своими требованиями. Дополнительные сведения о шаблонах Azure Resource Manager см. в следующих разделах:

* [Структура и синтаксис шаблона Azure Resource Manager](../azure-resource-manager/resource-group-authoring-templates.md)
* [Шаблоны диспетчера ресурсов Azure](../azure-resource-manager/resource-group-authoring-templates.md)
* [Разработка шаблонов Azure Resource Manager для обеспечения согласованности с облаком](../azure-resource-manager/templates-cloud-consistency.md)

> [!IMPORTANT]
> Подключения в шаблоне должны использовать ту же группу ресурсов и расположение Azure, что и приложение логики.

<a name="visual-studio"></a>

## <a name="create-templates-with-visual-studio"></a>Создание шаблонов с помощью Visual Studio

Чтобы проще всего создать допустимые параметризованные шаблоны приложений логики, которые в основном готовы к развертыванию, используйте Visual Studio (бесплатный выпуск Community Edition или более позднюю версию) и средства Azure Logic Apps для Visual Studio. Затем можно либо [создать приложение логики в Visual Studio](../logic-apps/quickstart-create-logic-apps-with-visual-studio.md) , либо [найти и скачать существующее приложение логики из портал Azure в Visual Studio](../logic-apps/manage-logic-apps-with-visual-studio.md).

Загрузив приложение логики, вы получаете шаблон, который включает определения для приложения логики и другие ресурсы, такие как подключения. Этот шаблон также позволяет *параметризовать*или определять параметры для значений, используемых для развертывания приложения логики и других ресурсов. Значения этих параметров можно указать в отдельном файле параметров. Таким образом, вы сможете легко изменять эти значения в зависимости от потребностей развертывания. Дополнительные сведения см. в следующих статьях:

* [Создание приложений логики с помощью Visual Studio](../logic-apps/quickstart-create-logic-apps-with-visual-studio.md)
* [Управление приложениями логики в Visual Studio](../logic-apps/manage-logic-apps-with-visual-studio.md)

<a name="azure-powershell"></a>

## <a name="create-templates-with-azure-powershell"></a>Создание шаблонов с помощью Azure PowerShell

Шаблоны диспетчер ресурсов можно создать с помощью Azure PowerShell с модулем [логикапптемплате](https://github.com/jeffhollan/LogicAppTemplateCreator). Этот модуль с открытым исходным кодом сначала оценивает приложение логики и все подключения, используемые приложением логики. Затем модуль создает ресурсы шаблона с необходимыми параметрами для развертывания.

Например, предположим, что у вас есть приложение логики, которое получает сообщение из очереди служебной шины Azure и отправляет данные в базу данных SQL Azure. Модуль сохраняет всю логику оркестрации и параметризовать строки подключения SQL и служебной шины, чтобы вы могли предоставлять и изменять эти значения в зависимости от потребностей развертывания.

### <a name="install-powershell-modules"></a>Установка модулей PowerShell

1. Установите [Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-az-ps), если вы еще этого не сделали.

1. Чтобы самый простой способ установить модуль Логикапптемплате из [коллекция PowerShell](https://www.powershellgallery.com/packages/LogicAppTemplate), выполните следующую команду:

   ```text
   PS> Install-Module -Name LogicAppTemplate
   ```

   Чтобы выполнить обновление до последней версии, выполните следующую команду:

   ```text
   PS> Update-Module -Name LogicAppTemplate
   ```

Или для установки вручную выполните действия, описанные в GitHub для [создателя шаблона приложения логики](https://github.com/jeffhollan/LogicAppTemplateCreator).

### <a name="install-azure-resource-manager-client"></a>Установка клиента Azure Resource Manager

Чтобы модуль Логикапптемплате работал с любым маркером доступа клиента и подписки Azure, установите [средство клиента Azure Resource Manager](https://github.com/projectkudu/ARMClient), которое является простым средством командной строки, которое вызывает API Azure Resource Manager.

При выполнении `Get-LogicAppTemplate` команды с помощью этого средства команда сначала получает маркер доступа через средство ARMClient, передает маркер в сценарий PowerShell и создает его в виде JSON-файла. Дополнительные сведения о средстве см. в этой [статье о средстве клиента Azure Resource Manager](https://blog.davidebbo.com/2015/01/azure-resource-manager-client.html).

### <a name="generate-template-with-powershell"></a>Создание шаблона с помощью PowerShell

Чтобы создать шаблон после установки модуля Логикапптемплате, выполните следующую команду PowerShell:

```text
PS> Get-LogicAppTemplate
```

Чтобы следовать рекомендациям по конвейеру в маркере из [средства клиента Azure Resource Manager](https://github.com/projectkudu/ARMClient), выполните следующую команду, где `$SubscriptionId` — это идентификатор подписки Azure:

```text
PS> armclient token $SubscriptionId | Get-LogicAppTemplate -LogicApp <logic-app-name> -ResourceGroup <Azure-resource-group-name> -SubscriptionId $SubscriptionId -Verbose | Out-File C:\template.json
```

После извлечения можно создать файл параметров из шаблона, выполнив следующую команду:

```text
PS> Get-ParameterTemplate -TemplateFile $filename | Out-File '<parameters-file-name>.json'
```

Для извлечения со ссылками Azure Key Vault (только статические) выполните следующую команду:

```text
PS> Get-ParameterTemplate -TemplateFile $filename -KeyVault Static | Out-File $fileNameParameter
```

| Параметры | Обязательное значение | Описание |
|------------|----------|-------------|
| TemplateFile | Да | Путь к файлу шаблона |
| Хранилище ключей | Нет | Перечисление, описывающее способ управления возможными значениями хранилища ключей. Значение по умолчанию — `None`. |
||||

## <a name="next-steps"></a>Следующие шаги

> [!div class="nextstepaction"]
> [Развертывание шаблонов приложений логики](../logic-apps/logic-apps-deploy-azure-resource-manager-templates.md)
