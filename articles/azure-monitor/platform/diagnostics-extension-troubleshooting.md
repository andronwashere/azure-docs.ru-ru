---
title: Устранение неполадок расширения для Системы диагностики Azure
description: Устранение неполадок при использовании системы диагностики Azure на виртуальных машинах, в Service Fabric или в облачных службах.
services: azure-monitor
author: rboucher
ms.service: azure-monitor
ms.subservice: diagnostic-extension
ms.topic: conceptual
ms.date: 05/08/2019
ms.author: robb
ms.openlocfilehash: 99ac4ffc288773e52183d371ef2c20f6153bc0f3
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "65471781"
---
# <a name="azure-diagnostics-troubleshooting"></a>Устранение неполадок с помощью системы диагностики Azure
Данная статья содержит сведения об устранении неполадок, относящихся к средствами диагностики Azure. Дополнительные сведения о системе диагностики Azure см. в [обзоре системы диагностики Azure](diagnostics-extension-overview.md).

## <a name="logical-components"></a>Логические компоненты
**Средство запуска подключаемого модуля системы диагностики (DiagnosticsPluginLauncher.exe).** Запускает расширение системы диагностики Azure. Используется в качестве процесса точки входа.

**Подключаемый модуль системы диагностики (DiagnosticsPlugin.exe).** Настраивает и запускает агент мониторинга, а также управляет временем его существования. Это основной процесс, который запускает программа запуска.

**Агент мониторинга (процессы MonAgent\*.exe).** Отслеживает, собирает и передает данные диагностики.  

## <a name="logartifact-paths"></a>Пути к журналам и артефактам
В таблицах ниже приведены расположения некоторых важных журналов и артефактов. Эти сведения используются в остальной части документа.

### <a name="azure-cloud-services"></a>Облачные службы Azure
| Артефакт | Путь |
| --- | --- |
| **Файл конфигурации системы диагностики Azure** | %SystemDrive%\Packages\Plugins\Microsoft.Azure.Diagnostics.PaaSDiagnostics\<версия>\Config.txt |
| **Файлы журналов** | C:\Logs\Plugins\Microsoft.Azure.Diagnostics.PaaSDiagnostics\<версия>\ |
| **Локальное хранилище данных диагностики** | C:\Resources\Directory\<CloudServiceDeploymentID>.\<RoleName>.DiagnosticStore\WAD0107\Tables |
| **Файл конфигурации агента мониторинга** | C:\Resources\Directory\<CloudServiceDeploymentID>.\<RoleName>.DiagnosticStore\WAD0107\Configuration\MaConfig.xml |
| **Пакет расширений системы диагностики Azure** | %SystemDrive%\Packages\Plugins\Microsoft.Azure.Diagnostics.PaaSDiagnostics\<версия> |
| **Путь к служебной программе сбора журналов** | %SystemDrive%\Packages\GuestAgent\ |
| **Файл журнала MonAgentHost** | C:\Resources\Directory\<ИД_разверт._облачн._службы>.\<имя_роли>.DiagnosticStore\WAD0107\Configuration\MonAgentHost.<текущ._номер>.log |

### <a name="virtual-machines"></a>Виртуальные машины
| Артефакт | Путь |
| --- | --- |
| **Файл конфигурации системы диагностики Azure** | C:\Packages\Plugins\Microsoft.Azure.Diagnostics.IaaSDiagnostics\<версия>\RuntimeSettings |
| **Файлы журналов** | C:\WindowsAzure\Logs\Plugins\Microsoft.Azure.Diagnostics.IaaSDiagnostics\<DiagnosticsVersion>\ |
| **Локальное хранилище данных диагностики** | C:\WindowsAzure\Logs\Plugins\Microsoft.Azure.Diagnostics.IaaSDiagnostics\<версия_системы_диагностики>\WAD0107\Tables |
| **Файл конфигурации агента мониторинга** | C:\WindowsAzure\Logs\Plugins\Microsoft.Azure.Diagnostics.IaaSDiagnostics\<версия_системы_диагностики>\WAD0107\Configuration\MaConfig.xml |
| **Состояние файла** | C:\Packages\Plugins\Microsoft.Azure.Diagnostics.IaaSDiagnostics\<версия>\Status |
| **Пакет расширений системы диагностики Azure** | C:\Packages\Plugins\Microsoft.Azure.Diagnostics.IaaSDiagnostics\<версия_системы_диагностики>|
| **Путь к служебной программе сбора журналов** | C:\WindowsAzure\Logs\WaAppAgent.log |
| **Файл журнала MonAgentHost** | C:\WindowsAzure\Logs\Plugins\Microsoft.Azure.Diagnostics.IaaSDiagnostics\<версия_системы_диагностики>\WAD0107\Configuration\MonAgentHost.<текущ._номер>.log |

## <a name="metric-data-doesnt-appear-in-the-azure-portal"></a>Данные метрик не отображается на портале Azure
Система диагностики Azure предоставляет данные метрик, которые могут отображаться на портале Azure. Если возникли проблемы с просмотром этих данных на портале, необходимо проверить таблицу WADMetrics\* в учетной записи хранения системы диагностики Azure, чтобы узнать, имеются ли соответствующие записи метрики.

Здесь **PartitionKey** таблицы — это идентификатор ресурса, виртуальной машины или масштабируемого набора виртуальных машин. **RowKey** — это имя метрики (также известное как имя счетчика производительности).

Если идентификатор ресурса неверный, необходимо проверить правильность его значения. Для этого выберите **Diagnostics** **Configuration** (Конфигурация диагностики)  > **Метрики** > **ResourceId**.

Если нет данных для конкретной метрики, необходимо проверить, включена ли метрика (счетчик производительности). Для этого выберите **Diagnostics Configuration** (Конфигурация диагностики)  > **PerformanceCounter**. Следующие счетчики включены по умолчанию.
- \Процессор (_общий объем ресурсов)\% загруженности процессора
- \Память\доступные байты
- \ASP.NET Applications(__Total__)\Requests/Sec
- \ASP.NET Applications(__Total__)\Errors Total/Sec
- \ASP.NET\Requests Queued
- \ASP.NET\Requests Rejected
- \Processor(w3wp)\% Processor Time
- \Process(w3wp)\Private Bytes
- \Process(WaIISHost)\% Processor Time
- \Process(WaIISHost)\Private Bytes
- \Process(WaWorkerHost)\% Processor Time
- \Process(WaWorkerHost)\Private Bytes
- \Memory\Page Faults/sec
- \.NET CLR Memory(_Global_)\% Time in GC
- \LogicalDisk(C:)\Disk Write Bytes/sec
- \LogicalDisk(C:)\Disk Read Bytes/sec
- \LogicalDisk(D:)\Disk Write Bytes/sec
- \LogicalDisk(D:)\Disk Read Bytes/sec

Если конфигурация настроена правильно, но данные метрики все равно не отображаются, следуйте инструкциям ниже для устранения ошибок.


## <a name="azure-diagnostics-is-not-starting"></a>Система диагностики Azure не запускается
Сведения о том, почему система диагностики Azure не запускается, см. в файлах **DiagnosticsPluginLauncher.log** и **DiagnosticsPlugin.log** в расположение файлов журнала, указанном выше.

Если эти журналы указывают `Monitoring Agent not reporting success after launch`, это означает, что произошел сбой запуска MonAgentHost.exe. Просмотрите журналы в расположении, указанном для `MonAgentHost log file` в предыдущем разделе.

В последней строке файлов журнала указан код выхода.  

```
DiagnosticsPluginLauncher.exe Information: 0 : [4/16/2016 6:24:15 AM] DiagnosticPlugin exited with code 0
```
Если код выхода имеет **отрицательное значение**, ознакомьтесь с [таблицей кодов выхода](#azure-diagnostics-plugin-exit-codes) в разделе [Рекомендации](#references).

## <a name="diagnostics-data-is-not-logged-to-azure-storage"></a>Журнал данных диагностики не записывается в службу хранилища Azure
Определите, как отображаются данные (данные не отображаются вообще или они отображаются частично).

### <a name="diagnostics-infrastructure-logs"></a>Журналы инфраструктуры диагностики
Система диагностики регистрирует все ошибки в журналы инфраструктуры системы диагностики. Убедитесь, что вы включили [запись журналов инфраструктуры системы диагностики в своей конфигурации](#how-to-check-diagnostics-extension-configuration). Затем можно быстро найти все соответствующие ошибки, отображаемые в таблице `DiagnosticInfrastructureLogsTable`, в настроенной учетной записи хранения.

### <a name="no-data-is-appearing"></a>Данные не отображаются
Самая распространенная причина того, что данные о событии вообще не отображается — неправильно заданные сведения об учетной записи хранения.

Решение. Исправьте файл конфигурации системы диагностики и переустановите систему диагностики.

Если учетная запись хранения настроена правильно, удаленный доступ к компьютеру и убедитесь, что *DiagnosticsPlugin.exe* и *MonAgentCore.exe* запущены. Если они не выполняются, выполните действия в разделе [Система диагностики Azure не запускается](#azure-diagnostics-is-not-starting).

Если процессы выполняются, перейдите к разделу [Сохраняются ли собранные данные локально?](#is-data-getting-captured-locally) и следуйте инструкциям в нем.

Если это не решает проблему, попробуйте:

1. Удаление агента
2. Удалить каталог C:\WindowsAzure\Logs\Plugins\Microsoft.Azure.Diagnostics.IaaSDiagnostics
3. Установить агент еще раз


### <a name="part-of-the-data-is-missing"></a>Отсутствует часть данных
Вы получаете не все данные, это означает, что конвейер сбора и передачи данных настроен правильно. Следуйте указаниям из подразделов ниже, чтобы сузить список возможных проблем.

#### <a name="is-the-collection-configured"></a>Настроен ли сбор?
Сбор определенных типов данных указывается в конфигурации системы диагностики. [Проверьте конфигурацию](#how-to-check-diagnostics-extension-configuration), чтобы убедиться, что вы ищете только те данные, для которых настроен сбор.

#### <a name="is-the-host-generating-data"></a>Создает ли узел данные?
- **Счетчики производительности.** Откройте системный монитор и проверьте значение счетчика.

- **Журналы трассировки.**  Удаленно подключитесь к виртуальной машине и добавьте экземпляр TextWriterTraceListener в файл конфигурации приложения.  Сведения о настройке прослушивателя см. по ссылке https://msdn.microsoft.com/library/sk36c28t.aspx.  Проверьте, имеет ли элемент `<trace>` значение `<trace autoflush="true">`.<br />
Если журналы трассировки не создаются, дополнительные сведения см. в разделе "Подробные сведения об отсутствии журналов трассировки".

- **Трассировка событий Windows.** Удаленно подключитесь к виртуальной машине и установите PerfView.  Откройте средство PerfView и выберите **File** (Файл)  > **User Command** (Пользовательская команда)  > **Ожидать передачи данных** > **etwprovider2**, указав при этом необходимых поставщиков ETW (etwprovder1, etwprovider2 и т. д.). Команда **Ожидать передачи данных** учитывает регистр. Поставщики трассировки событий Windows следует указывать через запятые без пробелов. Если выполнение этой команды завершилось сбоем, нажмите кнопку **Журнал** в правом нижнем углу средства Perfview, чтобы просмотреть попытки и результаты запуска.  Если все входные данные верны, через несколько секунд откроется новое окно с событиями трассировки событий Windows.

- **Журналы событий.** Удаленно подключитесь к виртуальной машине. Откройте `Event Viewer`, а затем убедитесь, что события существуют.

#### <a name="is-data-getting-captured-locally"></a>Сохраняются ли данные локально?
Проверьте, сохраняются ли данные локально.
По умолчанию эти данные хранятся в файлах `*.tsf` в локальном хранилище данных диагностики. Каждый тип журнала записывается в разные файлы `.tsf`. Их имена напоминают имена таблиц в службе хранилища Azure.

Например, показатели `Performance Counters` записываются в файл `PerformanceCountersTable.tsf`, а данные журналов событий — в файл `WindowsEventLogsTable.tsf`. Откройте локальный набор файлов, используя указания в разделе [Извлечение локального журнала](#local-log-extraction), и проверьте, записываются ли они на диск.

Если локальные журналы не сохраняются локально и узел создает данные, скорее всего проблема связана с конфигурацией. Тщательно проверьте настройки конфигурации.

Кроме того, просмотрите файл конфигурации агента наблюдения (MaConfig.xml). и проверьте, задан ли раздел с описанием соответствующего исходного журнала и не возникают ли сложности с переводом между конфигурацией системы диагностики Azure и конфигурацией агента наблюдения.

#### <a name="is-data-getting-transferred"></a>Передаются ли данные?
Если данные сохраняются локально, но по-прежнему не отображаются в учетной записи хранения, сделайте следующее:

- Проверьте, указали ли вы правильную учетную запись хранения и не отзывали ли вы ее ключи. Иногда пользователи облачных служб Azure не обновляют раздел `useDevelopmentStorage=true`.

- Проверьте правильность указанной учетной записи хранения. Убедитесь, что у вас нет ограничений сети, запрещающих доступ компонентов к конечным точкам общедоступного хранилища. Это можно сделать, удаленно подключившись к виртуальной машине и создав запись в той же учетной записи хранения.

- Наконец можно просмотреть сбои, которые выводятся агентом наблюдения. Агент наблюдения записывает свои журналы в файл `maeventtable.tsf` в локальном хранилище данных диагностики. Чтобы открыть этот файл, следуйте инструкциям из раздела [Извлечения локального журнала](#local-log-extraction). Попытайтесь определить наличие `errors`, указывающих на сбои при чтении в локальных файлах и записи в хранилище.

### <a name="capturing-and-archiving-logs"></a>Архивация и запись журналов
Если вы собираетесь обратиться в службу поддержки, первое, что вас могут попросить — это собрать журналы с компьютера. Вы можете сэкономить время, сделав это самостоятельно. Запустите служебную программу `CollectGuestLogs.exe`, расположенную в папке "Служебный путь коллекции журналов". В той же папке она создаст ZIP-файл со всеми соответствующими журналами Azure.

## <a name="diagnostics-data-tables-not-found"></a>Таблицы данных диагностики не найдены
Таблицы в службе хранилища Azure, содержащие события трассировки событий Windows, именуются с помощью следующего кода:

```csharp
        if (String.IsNullOrEmpty(eventDestination)) {
            if (e == "DefaultEvents")
                tableName = "WADDefault" + MD5(provider);
            else
                tableName = "WADEvent" + MD5(provider) + eventId;
        }
        else
            tableName = "WAD" + eventDestination;
```

Вот пример:

```XML
        <EtwEventSourceProviderConfiguration provider="prov1">
          <Event id="1" />
          <Event id="2" eventDestination="dest1" />
          <DefaultEvents />
        </EtwEventSourceProviderConfiguration>
        <EtwEventSourceProviderConfiguration provider="prov2">
          <DefaultEvents eventDestination="dest2" />
        </EtwEventSourceProviderConfiguration>
```
```JSON
"EtwEventSourceProviderConfiguration": [
    {
        "provider": "prov1",
        "Event": [
            {
                "id": 1
            },
            {
                "id": 2,
                "eventDestination": "dest1"
            }
        ],
        "DefaultEvents": {
            "eventDestination": "DefaultEventDestination",
            "sinks": ""
        }
    },
    {
        "provider": "prov2",
        "DefaultEvents": {
            "eventDestination": "dest2"
        }
    }
]
```
Этот код создает четыре таблицы:

| Событие | Имя таблицы |
| --- | --- |
| provider=”prov1” &lt;Event id=”1” /&gt; |WADEvent+MD5(“prov1”)+”1” |
| provider=”prov1” &lt;Event id=”2” eventDestination=”dest1” /&gt; |WADdest1 |
| provider=”prov1” &lt;DefaultEvents /&gt; |WADDefault+MD5(“prov1”) |
| provider=”prov2” &lt;DefaultEvents eventDestination=”dest2” /&gt; |WADdest2 |

## <a name="references"></a>Справочники

### <a name="how-to-check-diagnostics-extension-configuration"></a>Как проверить конфигурацию расширения системы диагностики
Самый простой способ проверить конфигурацию расширения — перейти в [обозреватель ресурсов Azure](http://resources.azure.com) и выбрать виртуальную машину или облачную службу, где выполняется расширение системы диагностики Azure (IaaSDiagnostics или PaaDiagnostics).

Кроме того, вы можете подключиться к удаленному рабочему столу виртуальной машины и просмотреть файл конфигурации системы диагностики Azure, как описано в разделе "Пути к артефактам".

В любом случае выполните поиск **Microsoft.Azure.Diagnostics**, а затем найдите поле **xmlCfg** или **WadCfg**.

Если поиск выполняется на виртуальной машине и вы видите поле **WadCfg**, это означает, что файл конфигурации имеет формат JSON. Если вы видите поле **xmlCfg**, то это означает, что файл конфигурации имеет формат XML и кодировку Base64. Его необходимо [декодировать](https://www.bing.com/search?q=base64+decoder), чтобы увидеть XML-файл, который был загружен системой диагностики.

Если вы проверяете расширение в облачной службе и открыли файл конфигурации на диске, данные в нем будут в кодировке base64, поэтому необходимо [раскодировать их](https://www.bing.com/search?q=base64+decoder), чтобы просмотреть данные XML, загруженные системой диагностики.

### <a name="azure-diagnostics-plugin-exit-codes"></a>Коды выхода подключаемого модуля системы диагностики Azure
Подключаемый модуль возвращает следующие коды выхода:

| Код выхода | Описание |
| --- | --- |
| 0 |Успешно. |
| -1 |Общая ошибка. |
| -2 |Невозможно загрузить RCF-файл.<p>Эта внутренняя ошибка возникает только тогда, когда средство запуска подключаемого модуля гостевого агента вызвано вручную, неправильно и на виртуальной машине. |
| -3 |Не удалось загрузить файл конфигурации системы диагностики.<p><p>Решение. Это вызвано тем, что файл конфигурации не прошел проверку схемы. Следует предоставить файл конфигурации, соответствующий схеме. |
| -4 |Другой экземпляр системы диагностики агента мониторинга уже использует локальный каталог ресурсов.<p><p>Решение. Укажите другое значение для **LocalResourceDirectory**. |
| -6 |Попытка средства запуска подключаемого модуля гостевого агента запустить систему диагностики с помощью неправильно составленной команды.<p><p>Эта внутренняя ошибка возникает только тогда, когда средство запуска подключаемого модуля гостевого агента вызвано вручную, неправильно и на виртуальной машине. |
| -10 |Подключаемый модуль системы диагностики был завершен с необработанным исключением. |
| -11 |Гостевому агенту не удалось создать процесс, ответственный за запуск и мониторинг агента мониторинга.<p><p>Решение. Убедитесь, что доступных системных ресурсов достаточно для запуска новых процессов.<p> |
| -101 |Недопустимые аргументы при вызове подключаемого модуля системы диагностики.<p><p>Эта внутренняя ошибка возникает только тогда, когда средство запуска подключаемого модуля гостевого агента вызвано вручную, неправильно и на виртуальной машине. |
| -102 |Процесс подключаемого модуля не может инициализироваться.<p><p>Решение. Убедитесь, что доступных системных ресурсов достаточно для запуска новых процессов. |
| -103 |Процесс подключаемого модуля не может инициализироваться. А именно: не удается создать объект средства ведения журнала.<p><p>Решение. Убедитесь, что доступных системных ресурсов достаточно для запуска новых процессов. |
| -104 |Не удалось загрузить RCF-файл, предоставленный гостевым агентом.<p><p>Эта внутренняя ошибка возникает только тогда, когда средство запуска подключаемого модуля гостевого агента вызвано вручную, неправильно и на виртуальной машине. |
| -105 |Подключаемому модулю системы диагностики не удается открыть файл конфигурации системы диагностики.<p><p>Эта внутренняя ошибка возникает только тогда, когда подключаемый модуль системы диагностики вызван вручную, неправильно и на виртуальной машине. |
| -106 |Не удалось прочитать файл конфигурации системы диагностики.<p><p>Это вызвано тем, что файл конфигурации не прошел проверку схемы. <br><br>Решение. Следует предоставить файл конфигурации, соответствующий схеме. Дополнительные сведения см. в разделе [Как проверить конфигурацию расширения системы диагностики](#how-to-check-diagnostics-extension-configuration). |
| -107 |Недопустимая передача каталога ресурсов в агент мониторинга.<p><p>Эта внутренняя ошибка может возникать, только если агент мониторинга вызван вручную, неправильно и на виртуальной машине.</p> |
| -108 |Не удалось преобразовать файл конфигурации системы диагностики в файл конфигурации агента мониторинга.<p><p>Эта внутренняя ошибка может возникать, только если подключаемый модуль системы диагностики вызван с помощью недопустимого файла конфигурации. |
| -110 |Общая ошибка конфигурации системы диагностики.<p><p>Эта внутренняя ошибка может возникать, только если подключаемый модуль системы диагностики вызван с помощью недопустимого файла конфигурации. |
| -111 |Не удалось запустить агент мониторинга.<p><p>Решение. Убедитесь, что доступен достаточный объем системных ресурсов. |
| -112 |Общая ошибка |

### <a name="local-log-extraction"></a>Извлечение локального журнала
Агент мониторинга сохраняет данные журналов и артефактов как файлы с расширением `.tsf`. Файл TSF (`.tsf`) недоступен для чтения, но его можно преобразовать в формат CSV (`.csv`) с помощью следующей служебной программы:

```
<Azure diagnostics extension package>\Monitor\x64\table2csv.exe <relevantLogFile>.tsf
```
После этого создается файл `<relevantLogFile>.csv` в том же расположении, что и соответствующий файл TSF (`.tsf`).

>[!NOTE]
> С помощью этой служебной программы нужно преобразовать только основной файл TSF (например, PerformanceCountersTable.tsf). Все сопутствующие файлы (например, PerformanceCountersTables_\*\*001.tsf, PerformanceCountersTables_\*\*002.tsf и т. д.) обрабатываются автоматически.

### <a name="more-about-missing-trace-logs"></a>Дополнительные сведения об отсутствии журналов трассировки

>[!NOTE]
> Следующие сведения главным образом относятся к облачным службам Azure, если вы не настроили DiagnosticsMonitorTraceListener в приложении, выполняющемся на виртуальной машине IaaS.

- Проверьте, включен ли класс **DiagnosticMonitorTraceListener** в файле web.config или app.config.  Этот класс по умолчанию настроен в проектах облачных служб, но некоторые клиенты могут закомментировать его, в результате чего система диагностики не собирает трассировочные операторы.

- Если журналы не записываются из метода **OnStart** или **Run** проверьте наличие класса **DiagnosticMonitorTraceListener** в файле app.config.  По умолчанию он находится в файле web.config, но применяется только к коду, выполненному в средстве w3wp.exe. Поэтому этот класс также нужно определить в файле app.config, чтобы собирать трассировки, выполненные в средстве WaIISHost.exe.

- Убедитесь, что вы используете **Diagnostics.Trace.TraceXXX**, а не **Diagnostics.Debug.WriteXXX**. Операторы отладки удаляются из конечной сборки.

- Проверьте, имеет ли скомпилированный код строки **Diagnostics.Trace** (для проверки используйте Reflector, ildasm или ILSpy). Команды **Diagnostics.Trace** удаляются из скомпилированного двоичного файла, если не используется символ условной компиляции трассировки. Если для сборки проекта используется msbuild, это обычная проблема.   

## <a name="known-issues-and-mitigations"></a>Известные проблемы и устранения рисков
Ниже приведен список известных проблем и способы их устранения.

**1. Зависимость от .NET 4.5.**

Система диагностики Azure зависит от среды выполнения на платформе .NET 4.5 или более поздней версии. На момент написания статьи на всех компьютерах, подготовленных для облачных служб Azure, а также во всех официальных образах на базе виртуальных машин Azure, установлена платформа .NET версии 4.5 или более поздней.

Однако бывает, что вы пытаетесь запустить расширение Диагностики Azure для Windows на компьютере без .NET версии 4.5 или более поздней. Например, если создаете виртуальную машину на основе старого образа (моментального снимка) или с помощью собственного диска.

В этом случае при запуске **DiagnosticsPluginLauncher.exe** возвращается код выхода **255**. Сбой происходит из-за следующего необработанного исключения:
```
System.IO.FileLoadException: Could not load file or assembly 'System.Threading.Tasks, Version=1.5.11.0, Culture=neutral, PublicKeyToken=b03f5f7f11d50a3a' or one of its dependencies
```

**Устранение.** Установите на виртуальной машине платформу .NET версии 4.5 или более поздней.

**2. Данные счетчиков производительности доступные в хранилище, но не отображаются на портале**

На портале виртуальных машин данные определенных счетчиков производительности отображаются по умолчанию. Если счетчики производительности не отображаются, но известно, что данные создаются, так как они доступна в хранилище, проверьте следующее:

- Указаны ли имена счетчиков в хранилище на английском. В противном случае диаграмма метрик на портале не сможет распознать их. **Устранение.** Измените язык системных учетных записей виртуальной машины на английский. Для этого выберите **Панель управления** > **Регион** > **Административный** > **Настройка копирования**. Далее отмените выделение параметра **Экран приветствия и системные учетные записи**, чтобы пользовательский язык не применялся к системной учетной записи.

- Если вы используете подстановочные знаки (\*) в именах счетчиков производительности, портал не сможет отслеживать настроенные и собранные показатели счетчика, отправляемые в приемник службы хранилища Azure. **Устранение.** Чтобы убедиться в том, что вы можете использовать подстановочные знаки и что на портале развернут узел (\*), перенаправьте данные счетчиков производительности в [приемник Azure Monitor](diagnostics-extension-schema.md#diagnostics-extension-111).

