---
title: Настройка Application Insights в Azure с помощью PowerShell | Документация Майкрософт
description: Автоматизируйте настройку системы диагностики Azure для передачи данных в Application Insights.
services: application-insights
documentationcenter: .net
author: mrbullwinkle
manager: carmonm
ms.assetid: 4ac803a8-f424-4c0c-b18f-4b9c189a64a5
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 11/17/2015
ms.author: mbullwin
ms.openlocfilehash: 3c0decaa89b4ecc503157a32fcb1e5b4d249ccfb
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "60254634"
---
# <a name="using-powershell-to-set-up-application-insights-for-an-azure-web-app"></a>Настройка Application Insights для веб-приложения Azure с помощью PowerShell

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

[Microsoft Azure](https://azure.com) можно [настроить для отправки данных системы диагностики Azure](../../azure-monitor/platform/diagnostics-extension-to-application-insights.md) в [Azure Application Insights](../../azure-monitor/app/app-insights-overview.md). Данные диагностики связаны с облачными службами Azure и виртуальными машинами Azure. Они дополняют данные телеметрии, отправляемые из приложения с помощью пакета SDK Application Insights. В рамках автоматизации создания новых ресурсов в Azure вы можете настроить диагностику с помощью PowerShell.

## <a name="azure-template"></a>Шаблон Azure
Если веб-приложение работает в Azure и вы создаете ресурсы с помощью шаблона Azure Resource Manager, можно настроить Application Insights, добавив следующий код в узел ресурсов:

    {
      resources: [
        /* Create Application Insights resource */
        {
          "apiVersion": "2015-05-01",
          "type": "microsoft.insights/components",
          "name": "nameOfAIAppResource",
          "location": "centralus",
          "kind": "web",
          "properties": { "ApplicationId": "nameOfAIAppResource" },
          "dependsOn": [
            "[concat('Microsoft.Web/sites/', myWebAppName)]"
          ]
        }
       ]
     } 

* `nameOfAIAppResource` — имя ресурса Application Insights.
* `myWebAppName` — идентификатор веб-приложения.

## <a name="enable-diagnostics-extension-as-part-of-deploying-a-cloud-service"></a>Включение расширения диагностики как части развертывания облачной службы
Параметр `ExtensionConfiguration` командлета `New-AzureDeployment` принимает массив значений для конфигурации диагностики. Их можно создать с помощью командлета `New-AzureServiceDiagnosticsExtensionConfig` . Пример:

```ps

    $service_package = "CloudService.cspkg"
    $service_config = "ServiceConfiguration.Cloud.cscfg"
    $diagnostics_storagename = "myservicediagnostics"
    $webrole_diagconfigpath = "MyService.WebRole.PubConfig.xml" 
    $workerrole_diagconfigpath = "MyService.WorkerRole.PubConfig.xml"

    $primary_storagekey = (Get-AzStorageKey `
     -StorageAccountName "$diagnostics_storagename").Primary
    $storage_context = New-AzStorageContext `
       -StorageAccountName $diagnostics_storagename `
       -StorageAccountKey $primary_storagekey

    $webrole_diagconfig = `
     New-AzureServiceDiagnosticsExtensionConfig `
      -Role "WebRole" -Storage_context $storageContext `
      -DiagnosticsConfigurationPath $webrole_diagconfigpath
    $workerrole_diagconfig = `
     New-AzureServiceDiagnosticsExtensionConfig `
      -Role "WorkerRole" `
      -StorageContext $storage_context `
      -DiagnosticsConfigurationPath $workerrole_diagconfigpath

    New-AzureDeployment `
      -ServiceName $service_name `
      -Slot Production `
      -Package $service_package `
      -Configuration $service_config `
      -ExtensionConfiguration @($webrole_diagconfig,$workerrole_diagconfig)

``` 

## <a name="enable-diagnostics-extension-on-an-existing-cloud-service"></a>Включение расширения диагностики в существующей облачной службе
Для уже существующей службы используйте командлет `Set-AzureServiceDiagnosticsExtension`.

```ps

    $service_name = "MyService"
    $diagnostics_storagename = "myservicediagnostics"
    $webrole_diagconfigpath = "MyService.WebRole.PubConfig.xml" 
    $workerrole_diagconfigpath = "MyService.WorkerRole.PubConfig.xml"
    $primary_storagekey = (Get-AzStorageKey `
         -StorageAccountName "$diagnostics_storagename").Primary
    $storage_context = New-AzStorageContext `
        -StorageAccountName $diagnostics_storagename `
        -StorageAccountKey $primary_storagekey

    Set-AzureServiceDiagnosticsExtension `
        -StorageContext $storage_context `
        -DiagnosticsConfigurationPath $webrole_diagconfigpath `
        -ServiceName $service_name `
        -Slot Production `
        -Role "WebRole" 
    Set-AzureServiceDiagnosticsExtension `
        -StorageContext $storage_context `
        -DiagnosticsConfigurationPath $workerrole_diagconfigpath `
        -ServiceName $service_name `
        -Slot Production `
        -Role "WorkerRole"
```

## <a name="get-current-diagnostics-extension-configuration"></a>Получение текущей конфигурации расширения диагностики
```ps

    Get-AzureServiceDiagnosticsExtension -ServiceName "MyService"
```


## <a name="remove-diagnostics-extension"></a>Удаление расширения диагностики
```ps

    Remove-AzureServiceDiagnosticsExtension -ServiceName "MyService"
```

Если расширение диагностики было включено с помощью командлета `Set-AzureServiceDiagnosticsExtension` или `New-AzureServiceDiagnosticsExtensionConfig` без параметра Role, вы можете удалить расширение с помощью командлета `Remove-AzureServiceDiagnosticsExtension` без параметра Role. Если параметр Role использовался при включении расширения, он также должен применяться и при его удалении.

Удаление расширения диагностики из каждой отдельной роли

```ps

    Remove-AzureServiceDiagnosticsExtension -ServiceName "MyService" -Role "WebRole"
```


## <a name="see-also"></a>См. также
* [Мониторинг приложений облачных служб Azure с помощью Application Insights](../../azure-monitor/app/cloudservices.md)
* [Отправка данных системы диагностики Azure в Application Insights](../../azure-monitor/platform/diagnostics-extension-to-application-insights.md)
* [Use PowerShell to set alerts in Application Insights](powershell-alerts.md)

