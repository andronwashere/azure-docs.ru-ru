---
title: Использование хранилища BLOB-объектов для IIS и хранилища таблиц для событий в Azure Monitor | Документация Майкрософт
description: Azure Monitor может считывать журналы служб Azure, которые записывают диагностические данные в хранилище таблиц, или журналы IIS, записанные в хранилище BLOB-объектов.
services: log-analytics
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: bf444752-ecc1-4306-9489-c29cb37d6045
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 04/12/2017
ms.author: magoedte
ms.openlocfilehash: 901544886e0a0c90c29e83fc71f7a7a25ffc6862
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "66244888"
---
# <a name="collect-azure-diagnostic-logs-from-azure-storage"></a>Сбор журналов диагностики Azure из службы хранилища Azure

Azure Monitor можно считывать журналы следующих служб, которые записывают диагностические данные в хранилище таблиц, или журналы IIS, записанные в хранилище BLOB-объектов:

* кластеры Service Fabric (предварительная версия);
* Виртуальные машины
* Веб-роль или рабочая роль.

Прежде чем Azure Monitor может собирать данные в рабочую область Log Analytics для этих ресурсов, необходимо включить систему диагностики Azure.

После включения диагностики, можно использовать портал Azure или PowerShell, чтобы настроить рабочую область для сбора журналов.

Диагностика Azure — это расширение Azure, которое позволяет собирать диагностические данные из рабочей роли, веб-роли или виртуальной машины, выполняющейся в Azure. Данные хранятся в учетной записи хранения Azure и затем могут собираться с Azure Monitor.

Для Azure Monitor для сбора этих журналов диагностики Azure журналы должны находиться в следующих расположениях:

| Тип журнала | Тип ресурса | Расположение |
| --- | --- | --- |
| Журналы IIS |Виртуальные машины <br> Веб-роли <br> Рабочие роли |wad-iis-logfiles (хранилище BLOB-объектов) |
| syslog |Виртуальные машины |LinuxsyslogVer2v0 (хранилище таблиц) |
| Операционные события Service Fabric |Узлы Service Fabric |WADServiceFabricSystemEventTable |
| События субъектов Service Fabric Reliable Actors |Узлы Service Fabric |WADServiceFabricReliableActorEventTable |
| События надежных служб Service Fabric |Узлы Service Fabric |WADServiceFabricReliableServiceEventTable |
| Журналы событий Windows |Узлы Service Fabric <br> Виртуальные машины <br> Веб-роли <br> Рабочие роли |WADWindowsEventLogsTable (хранилище таблиц) |
| Журналы трассировки событий Windows |Узлы Service Fabric <br> Виртуальные машины <br> Веб-роли <br> Рабочие роли |WADETWEventTable (хранилище таблиц) |

> [!NOTE]
> Журналы IIS веб-сайтов Azure в настоящее время не поддерживаются.
>
>

При работе с виртуальными машинами вы можете установить [агент Log Analytics](../../azure-monitor/learn/quick-collect-azurevm.md) на виртуальную машину, чтобы включить дополнительные функции анализа. Это позволит, помимо анализа журналов IIS и журналов событий, выполнять дополнительные виды анализа, включая отслеживание изменений конфигурации, оценку SQL и оценку обновлений.

## <a name="enable-azure-diagnostics-in-a-virtual-machine-for-event-log-and-iis-log-collection"></a>Включение диагностики Azure в виртуальной машине для сбора журналов событий и журналов IIS

Следующая процедура используется для включения диагностики Azure на виртуальной машине для сбора журналов событий и журналов IIS с помощью портала Microsoft Azure.

### <a name="to-enable-azure-diagnostics-in-a-virtual-machine-with-the-azure-portal"></a>Включение диагностики Azure на виртуальной машине с помощью портала Azure

1. При создании виртуальной машины установите агент ВМ. Если виртуальная машина уже существует, проверьте, установлен ли агент.

   * На портале Azure перейдите к виртуальной машине, выберите **Необязательная настройка**, а затем **Диагностика** и задайте для параметра **Состояние** значение **Вкл.**

     По завершении этой процедуры на виртуальной машине будет установлено и запущено расширение системы диагностики Azure. Это расширение отвечает за сбор диагностических данных.
2. На существующей виртуальной машине включите мониторинг и настройте ведение журнала событий. Диагностику можно включить на уровне ВМ. Чтобы включить диагностику, а затем настроить ведение журнала событий, выполните указанные ниже действия.

   1. Выберите виртуальную машину.
   2. Щелкните **Мониторинг**.
   3. Щелкните **Диагностика**.
   4. Установите для параметра **Состояние** значение **ВКЛ**.
   5. Выберите все нужные журналы диагностики.
   6. Последовательно выберите **ОК**.

## <a name="enable-azure-diagnostics-in-a-web-role-for-iis-log-and-event-collection"></a>Включение Диагностики Azure в веб-роли для сбора журналов IIS и событий

Порядок действий для включения системы диагностики Azure описан [здесь](../../cloud-services/cloud-services-dotnet-diagnostics.md). Эта информация используется в приведенных ниже инструкциях по использованию системы диагностики в Log Analytics.

При включении диагностики Azure происходит следующее.

* Журналы IIS по умолчанию сохраняются, а их данные передаются с интервалом, заданным с помощью параметра scheduledTransferPeriod/
* Журналы событий Windows по умолчанию не передаются.

### <a name="to-enable-diagnostics"></a>Включение диагностики

Чтобы включить журналы событий Windows или изменить значение scheduledTransferPeriod, настройте Диагностику Azure с помощью XML-файла конфигурации (diagnostics.wadcfg), как описывается в разделе [Шаг 4. Создание файла конфигурации системы диагностики и установка расширения](../../cloud-services/cloud-services-dotnet-diagnostics.md).

В следующем примере файл конфигурации собирает все журналы IIS и все события из журналов приложения и системы:

```xml
    <?xml version="1.0" encoding="utf-8" ?>
    <DiagnosticMonitorConfiguration xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration"
          configurationChangePollInterval="PT1M"
          overallQuotaInMB="4096">

      <Directories bufferQuotaInMB="0"
         scheduledTransferPeriod="PT10M">  
        <!-- IISLogs are only relevant to Web roles -->
        <IISLogs container="wad-iis" directoryQuotaInMB="0" />
      </Directories>

      <WindowsEventLog bufferQuotaInMB="0"
         scheduledTransferLogLevelFilter="Verbose"
         scheduledTransferPeriod="PT10M">
        <DataSource name="Application!*" />
        <DataSource name="System!*" />
      </WindowsEventLog>

    </DiagnosticMonitorConfiguration>
```

Убедитесь, что в параметре ConfigurationSettings указана учетная запись хранения, как в следующем примере:

```xml
    <ConfigurationSettings>
       <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" value="DefaultEndpointsProtocol=https;AccountName=<AccountName>;AccountKey=<AccountKey>"/>
    </ConfigurationSettings>
```

Значения **AccountName** и **AccountKey** можно найти на портале Azure. Для этого откройте раздел "Управление ключами доступа" на панели мониторинга учетной записи хранения. Для строки подключения должен использоваться протокол **https**.

После того как обновленная конфигурация диагностики применяется к облачной службе и диагностические данные начнут записываться в хранилище Azure, вы будете готовы настроить рабочую область Log Analytics.

## <a name="use-the-azure-portal-to-collect-logs-from-azure-storage"></a>Сбор журналов из хранилища Azure с помощью портала Azure

Чтобы настроить рабочую область Log Analytics в Azure Monitor для сбора журналов для следующих служб Azure можно использовать портал Azure:

* Кластеры Service Fabric.
* Виртуальные машины
* Веб-роль или рабочая роль.

На портале Azure перейдите к рабочей области Log Analytics и сделайте следующее:

1. Щелкните *Storage accounts logs* (Журналы учетных записей хранения).
2. Щелкните задачу *Добавить*.
3. Выберите учетную запись, содержащую журналы диагностики.
   * Это может быть классическая учетная запись хранения или учетная запись хранения Azure Resource Manager.
4. Выберите тип данных, по которому требуется собирать журналы.
   * Доступны следующие варианты: журналы IIS, журналы событий, системный журнал (Linux), журналы трассировки событий Windows, события Service Fabric.
5. Значение параметра "Источник" заполняется автоматически на основе типа данных, и его нельзя изменить.
6. Нажмите кнопку "ОК", чтобы сохранить конфигурацию.

Повторите шаги 2 – 6 для дополнительных учетных записей хранения и типы данных, которые требуется собирать в рабочую область.

Приблизительно через 30 минут имеется возможность просмотреть данные из учетной записи хранения в рабочей области Log Analytics. Отобразятся только данные, записанные в хранилище после применения конфигурации. Рабочая область не считывает уже существующие данные из учетной записи хранения.

> [!NOTE]
> Портал не проверяет существование источника, а также то, записываются ли новые данные.
>
>

## <a name="enable-azure-diagnostics-in-a-virtual-machine-for-event-log-and-iis-log-collection-using-powershell"></a>Включение диагностики Azure на виртуальной машине для сбора журналов событий и журналов IIS с помощью PowerShell

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Следуйте инструкциям в [Настройка Azure Monitor для индексации системы диагностики Azure](powershell-workspace-configuration.md#configuring-log-analytics-workspace-to-collect-azure-diagnostics-from-storage) использовать PowerShell для чтения из службы диагностики Azure, записанных в хранилище таблиц.

С помощью Azure PowerShell можно более точно указать, какие события должны записываться в хранилище.
Дополнительную информацию см. в статье [Включение диагностики на виртуальных машинах Azure](/azure/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines).

Включить и обновить систему диагностики Azure можно с помощью приведенного ниже сценария PowerShell.
Его также можно использовать с пользовательской конфигурацией ведения журналов.
Измените сценарий, указав учетную запись хранения, имя службы и имя виртуальной машины.
Сценарий использует командлеты для классических виртуальных машин.

Изучите приведенный ниже образец скрипта, скопируйте, измените нужным образом и сохраните как файл скрипта PowerShell, а затем выполните скрипт.

```powershell
    #Connect to Azure
    Add-AzureAccount

    # settings to change:
    $wad_storage_account_name = "myStorageAccount"
    $service_name = "myService"
    $vm_name = "myVM"

    #Construct Azure Diagnostics public config and convert to config format

    # Collect just system error events:
    $wad_xml_config = "<WadCfg><DiagnosticMonitorConfiguration><WindowsEventLog scheduledTransferPeriod=""PT1M""><DataSource name=""System!* "" /></WindowsEventLog></DiagnosticMonitorConfiguration></WadCfg>"

    $wad_b64_config = [System.Convert]::ToBase64String([System.Text.Encoding]::UTF8.GetBytes($wad_xml_config))
    $wad_public_config = [string]::Format("{{""xmlCfg"":""{0}""}}",$wad_b64_config)

    #Construct Azure diagnostics private config

    $wad_storage_account_key = (Get-AzStorageKey $wad_storage_account_name).Primary
    $wad_private_config = [string]::Format("{{""storageAccountName"":""{0}"",""storageAccountKey"":""{1}""}}",$wad_storage_account_name,$wad_storage_account_key)

    #Enable Diagnostics Extension for Virtual Machine

    $wad_extension_name = "IaaSDiagnostics"
    $wad_publisher = "Microsoft.Azure.Diagnostics"
    $wad_version = (Get-AzureVMAvailableExtension -Publisher $wad_publisher -ExtensionName $wad_extension_name).Version # Gets latest version of the extension

    (Get-AzureVM -ServiceName $service_name -Name $vm_name) | Set-AzureVMExtension -ExtensionName $wad_extension_name -Publisher $wad_publisher -PublicConfiguration $wad_public_config -PrivateConfiguration $wad_private_config -Version $wad_version | Update-AzureVM
```


## <a name="next-steps"></a>Дальнейшие действия

* [Сбор журналов и метрик для поддерживаемых служб Azure](collect-azure-metrics-logs.md).
* [Включите решения](../../azure-monitor/insights/solutions.md) , чтобы обеспечить глубокое понимание данных.
* [Воспользуйтесь запросами поиска](../../azure-monitor/log-query/log-query-overview.md) для анализа данных.