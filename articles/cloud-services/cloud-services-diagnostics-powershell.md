---
title: Включение диагностики в облачных службах Azure с использованием PowerShell | Документация Майкрософт
description: Узнайте, как включить диагностику для облачных служб с помощью PowerShell.
services: cloud-services
documentationcenter: .net
author: georgewallace
ms.service: cloud-services
ms.devlang: dotnet
ms.topic: article
ms.date: 09/06/2016
ms.author: gwallace
ms.openlocfilehash: 4beed4dd874c23c36e125b5855e2e8380859ef83
ms.sourcegitcommit: 4b647be06d677151eb9db7dccc2bd7a8379e5871
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/19/2019
ms.locfileid: "68359162"
---
# <a name="enable-diagnostics-in-azure-cloud-services-using-powershell"></a>Включение диагностики в облачных службах Azure с помощью PowerShell
Для сбора диагностических данных, таких как журналы приложений, счетчики производительности и т. д., из облачной службы можно использовать расширение системы диагностики Azure. В этой статье описывается включение расширения диагностики Azure для облачной службы с помощью PowerShell.  Сведения о компонентах, которые потребуются для выполнения инструкций в этой статье, см. в статье [Установка и настройка Azure PowerShell](/powershell/azure/overview).

## <a name="enable-diagnostics-extension-as-part-of-deploying-a-cloud-service"></a>Включение расширения диагностики как части развертывания облачной службы
Этот подход подходит для сценариев непрерывной интеграции, в которых в ходе развертывания облачной службы можно включить расширение системы диагностики. При создании нового развертывания облачной службы можно включить расширение диагностики, передав параметр *екстенсионконфигуратион* в командлет [New-AzureDeployment](/powershell/module/servicemanagement/azure/new-azuredeployment?view=azuresmps-3.7.0) . Параметр *ExtensionConfiguration* принимает массив конфигураций диагностики, которые могут быть созданы с помощью командлета [New-AzureServiceDiagnosticsExtensionConfig](/powershell/module/servicemanagement/azure/new-azureservicediagnosticsextensionconfig?view=azuresmps-3.7.0) .

В следующем примере показано включение диагностики для облачной службы с веб-ролью и рабочей ролью, каждая из которых имеет свою конфигурацию диагностики.

```powershell
$service_name = "MyService"
$service_package = "CloudService.cspkg"
$service_config = "ServiceConfiguration.Cloud.cscfg"
$webrole_diagconfigpath = "MyService.WebRole.PubConfig.xml"
$workerrole_diagconfigpath = "MyService.WorkerRole.PubConfig.xml"

$webrole_diagconfig = New-AzureServiceDiagnosticsExtensionConfig -Role "WebRole" -DiagnosticsConfigurationPath $webrole_diagconfigpath
$workerrole_diagconfig = New-AzureServiceDiagnosticsExtensionConfig -Role "WorkerRole" -DiagnosticsConfigurationPath $workerrole_diagconfigpath

New-AzureDeployment -ServiceName $service_name -Slot Production -Package $service_package -Configuration $service_config -ExtensionConfiguration @($webrole_diagconfig,$workerrole_diagconfig)
```

Если файл конфигурации диагностики содержит элемент `StorageAccount` с именем учетной записи хранения, то командлет `New-AzureServiceDiagnosticsExtensionConfig` автоматически использует эту учетную запись хранения. Для этого учетная запись хранения должна входить в ту же подписку, что и развертываемая облачная служба.

В пакете SDK Azure 2.6 и более поздних версий файлы конфигурации расширения, создаваемые выходной целью публикации MSBuild, будут включать имя учетной записи хранения на основе строки конфигурации диагностики, указанной в файле конфигурации службы (.cscfg). Приведенный ниже сценарий показывает, как анализировать файлы конфигурации расширения из выходной цели публикации и настраивать расширение системы диагностики для каждой роли при развертывании облачной службы.

```powershell
$service_name = "MyService"
$service_package = "C:\build\output\CloudService.cspkg"
$service_config = "C:\build\output\ServiceConfiguration.Cloud.cscfg"

#Find the Extensions path based on service configuration file
$extensionsSearchPath = Join-Path -Path (Split-Path -Parent $service_config) -ChildPath "Extensions"

$diagnosticsExtensions = Get-ChildItem -Path $extensionsSearchPath -Filter "PaaSDiagnostics.*.PubConfig.xml"
$diagnosticsConfigurations = @()
foreach ($extPath in $diagnosticsExtensions)
{
    #Find the RoleName based on file naming convention PaaSDiagnostics.<RoleName>.PubConfig.xml
    $roleName = ""
    $roles = $extPath -split ".",0,"simplematch"
    if ($roles -is [system.array] -and $roles.Length -gt 1)
    {
        $roleName = $roles[1]
        $x = 2
        while ($x -le $roles.Length)
            {
               if ($roles[$x] -ne "PubConfig")
                {
                    $roleName = $roleName + "." + $roles[$x]
                }
                else
                {
                    break
                }
                $x++
            }
        $fullExtPath = Join-Path -path $extensionsSearchPath -ChildPath $extPath
        $diagnosticsconfig = New-AzureServiceDiagnosticsExtensionConfig -Role $roleName -DiagnosticsConfigurationPath $fullExtPath
        $diagnosticsConfigurations += $diagnosticsconfig
    }
}
New-AzureDeployment -ServiceName $service_name -Slot Production -Package $service_package -Configuration $service_config -ExtensionConfiguration $diagnosticsConfigurations
```

Visual Studio Online использует аналогичный подход для автоматических развертываний облачных служб с расширением системы диагностики. Полный пример см. в файле [Publish-AzureCloudDeployment.ps1](https://github.com/Microsoft/azure-pipelines-tasks/blob/master/Tasks/AzureCloudPowerShellDeploymentV1/Publish-AzureCloudDeployment.ps1).

Если в конфигурации диагностики нет элемента `StorageAccount`, то в командлет необходимо передать параметр *StorageAccountName*. Если параметр *StorageAccountName* указан, командлет использует учетную запись хранения, указанную в этом параметре, а не в файле конфигурации диагностики.

Если учетная запись хранения для диагностики и облачная служба относятся к разным подпискам, то в командлет необходимо явным образом передать параметры *StorageAccountName* и *StorageAccountKey*. Параметр *StorageAccountKey* не требуется, если учетная запись хранения диагностики входит в ту же подписку, так как при включении расширения диагностики командлет автоматически запрашивает и устанавливает значение ключа. Если же учетная запись хранения диагностики входит в другую подписку, командлет не сможет получить ключ автоматически, а значит, его необходимо явно указать с помощью параметра *StorageAccountKey*.

```powershell
$webrole_diagconfig = New-AzureServiceDiagnosticsExtensionConfig -Role "WebRole" -DiagnosticsConfigurationPath $webrole_diagconfigpath -StorageAccountName $diagnosticsstorage_name -StorageAccountKey $diagnosticsstorage_key
$workerrole_diagconfig = New-AzureServiceDiagnosticsExtensionConfig -Role "WorkerRole" -DiagnosticsConfigurationPath $workerrole_diagconfigpath -StorageAccountName $diagnosticsstorage_name -StorageAccountKey $diagnosticsstorage_key
```

## <a name="enable-diagnostics-extension-on-an-existing-cloud-service"></a>Включение расширения диагностики в существующей облачной службе
Для включения или обновления конфигурации диагностики в уже работающей облачной службе можно использовать командлет [Set-AzureServiceDiagnosticsExtension](/powershell/module/servicemanagement/azure/set-azureservicediagnosticsextension?view=azuresmps-3.7.0) .

[!INCLUDE [cloud-services-wad-warning](../../includes/cloud-services-wad-warning.md)]

```powershell
$service_name = "MyService"
$webrole_diagconfigpath = "MyService.WebRole.PubConfig.xml"
$workerrole_diagconfigpath = "MyService.WorkerRole.PubConfig.xml"

$webrole_diagconfig = New-AzureServiceDiagnosticsExtensionConfig -Role "WebRole" -DiagnosticsConfigurationPath $webrole_diagconfigpath
$workerrole_diagconfig = New-AzureServiceDiagnosticsExtensionConfig -Role "WorkerRole" -DiagnosticsConfigurationPath $workerrole_diagconfigpath

Set-AzureServiceDiagnosticsExtension -DiagnosticsConfiguration @($webrole_diagconfig,$workerrole_diagconfig) -ServiceName $service_name
```

## <a name="get-current-diagnostics-extension-configuration"></a>Получение текущей конфигурации расширения диагностики
Получите текущую конфигурацию диагностики для облачной службы с помощью командлета [Get-AzureServiceDiagnosticsExtension](/powershell/module/servicemanagement/azure/get-azureservicediagnosticsextension?view=azuresmps-3.7.0) .

```powershell
Get-AzureServiceDiagnosticsExtension -ServiceName "MyService"
```

## <a name="remove-diagnostics-extension"></a>Удаление расширения диагностики
Чтобы отключить диагностику в облачной службе, можно использовать командлет [Remove-AzureServiceDiagnosticsExtension](/powershell/module/servicemanagement/azure/remove-azureservicediagnosticsextension?view=azuresmps-3.7.0) .

```powershell
Remove-AzureServiceDiagnosticsExtension -ServiceName "MyService"
```

Если расширение системы диагностики включено с помощью командлета *Set-AzureServiceDiagnosticsExtension* или командлета *New-AzureServiceDiagnosticsExtensionConfig* без параметра *Role* , то расширение можно удалить с помощью  *Remove-AzureServiceDiagnosticsExtension* без параметра *Role* . Если параметр *Role* использовался при включении расширения, он также должен использоваться при удалении расширения.

Удаление расширения диагностики из каждой отдельной роли

```powershell
Remove-AzureServiceDiagnosticsExtension -ServiceName "MyService" -Role "WebRole"
```

## <a name="next-steps"></a>Следующие шаги
* Дополнительные рекомендации по использованию диагностики Azure и других методов для устранения неполадок см. в статье [Включение диагностики в облачных службах и виртуальных машинах Azure](cloud-services-dotnet-diagnostics.md).
* В статье [Схема конфигурации диагностики](/azure/azure-monitor/platform/diagnostics-extension-schema-1dot2) поясняются различные параметры XML-конфигураций для расширения диагностики.
* Сведения о включении расширения диагностики для виртуальных машин см. в статье [Создание виртуальной машины Windows с мониторингом и диагностикой с использованием шаблона Azure Resource Manager](../virtual-machines/windows/extensions-diagnostics-template.md).
