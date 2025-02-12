---
title: Управление потреблением и затраты для журналов Azure Monitor | Документация Майкрософт
description: Узнайте, как изменить ценовой план, а также управлять политикой объем и срок хранения данных для рабочей области Log Analytics в Azure Monitor.
services: azure-monitor
documentationcenter: azure-monitor
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: azure-monitor
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 06/06/2019
ms.author: magoedte
ms.subservice: ''
ms.openlocfilehash: bcfefc9698f7f251e99531750e19e7c06395e064
ms.sourcegitcommit: cf438e4b4e351b64fd0320bf17cc02489e61406a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/08/2019
ms.locfileid: "67655702"
---
# <a name="manage-usage-and-costs-with-azure-monitor-logs"></a>Управление потреблением и расходов с помощью журналов Azure Monitor

> [!NOTE]
> В этой статье описывается, как контролировать затраты в Azure Monitor, задав срок хранения данных для рабочей области Log Analytics.  См. в следующей статье, Дополнительные сведения.
> - В статье [Мониторинг использования и ожидаемых затрат](usage-estimated-costs.md) описано, как можно просмотреть сведения об использовании и оценить затраты по нескольким функциям мониторинга Azure для разных моделей ценообразования. Там также описано, как можно изменить модель ценообразования.

Журналы Azure монитор предназначен для масштабирования и поддержки сбора, индексирования и хранения больших объемов данных в день из любого источника в вашей организации или развернутых в Azure.  Хотя это может быть определяющим фактором для вашей организации, в конечном счете основной является экономическая эффективность. С этой целью важно понимать, что стоимость рабочей области Log Analytics не основаны только на объем данных, собираемых, это также зависит выбранный план и как долго вы решили хранить данные, созданные из подключенных источников.  

В этой статье мы рассмотрим, как можно в упреждающем режиме отслеживать увеличение объема хранимых данных и определять ограничения для контроля этих связанных затрат. 

Стоимость данных может быть значительной в зависимости от следующих факторов: 

- объем данных, создаваемых и попадающих в рабочую область; 
    - число включенных решений по управлению;
    - число отслеживаемых систем;
    - тип данных, собираемых из каждого отслеживаемого ресурса; 
- выбранная продолжительность хранения данных. 

## <a name="understand-your-workspaces-usage-and-estimated-cost"></a>Понимание использования и ожидаемых затрат для рабочей области

Azure упрощает мониторинг журналов, легко разобраться, что затраты, скорее всего, основываться на последних шаблонов использования. Чтобы сделать это, используйте **использование службы анализа журналов и оценка затрат** для просмотра и анализа данных об использовании. Здесь вы узнаете, какой объем данных был собран каждым решением, какой объем данных сохраняется, и получите оценку затрат на основании объема полученных данных и дополнительных объемов хранения сверх включенных в тариф.

![Использование и ожидаемые затраты](media/manage-cost-storage/usage-estimated-cost-dashboard-01.png)

Чтобы подробнее изучить данные, щелкните значок в верхней правой части любой диаграммы на странице **Использование и ожидаемые затраты**. Теперь вы можете доработать этот запрос, чтобы получить дополнительные сведения о потреблении.  

![Представление журналов](media/manage-cost-storage/logs.png)

На странице **Использование и оценка затрат** можно просмотреть сведения о томах данных за месяц. Сюда входят все данные, полученные и сохраненные в рабочей области Log Analytics.  Нажмите кнопку **сведения об использовании** в верхней части страницы, чтобы просмотреть панель мониторинга использования с информацией о тенденциях объема данных по источникам, компьютерам и предложения. Чтобы просмотреть и установить ежедневное ограничение или изменить срок хранения, щелкните **Управление объемом данных**.
 
Оплата за использование Log Analytics добавляется в счет Azure. Дополнительную информацию о счете за подписку на Azure можно просмотреть в разделе выставления счетов портала Azure или на [Портале управления счетами и подписками Azure](https://account.windowsazure.com/Subscriptions).  

## <a name="daily-cap"></a>Ежедневное ограничение

Вы можете настроить ежедневное ограничение и ограничить ежедневный прием данных для своей рабочей области, но будьте осторожны, так как ваша цель не должна превышать дневной лимит.  В противном случае в этот момент вы потеряете данные за оставшуюся часть дня, что может повлиять на доступность в рабочей области служб и решений Azure, функциональные возможности которых зависят от актуальных данных.  В результате вы не сможете наблюдать за условиями работоспособности ресурсов, поддерживающих ИТ-службы, и получать соответствующие уведомления.  Ежедневное ограничение предназначено для использования в качестве способа управления неожиданным увеличением объема данных из управляемых ресурсов и всегда оставаться в пределах лимита, или для ограничения незапланированных платежей за рабочую область.  

По достижении ежедневного ограничения сбор платных типов данных прекращается до конца дня. В верхней части страницы для выбранной рабочей области Log Analytics появится предупреждающий баннер и событие операции будет отправлено в таблицу *Операция* в категории **LogManagement**. Сбор данных возобновится по наступлении времени сброса, определенного в разделе *Daily limit will be set at* (Ежедневное ограничение будет установлено в). Рекомендуем определить правило генерации оповещений на основе этого события операции, настроенного для уведомления о достижении ежедневного ограничения по сбору данных. 

> [!NOTE]
> Ежедневное ограничение не останавливает сбор данных из центра безопасности Azure.

### <a name="identify-what-daily-data-limit-to-define"></a>Определение ежедневного ограничения по сбору данных

Сведения о тенденциях приема данных и определении ежедневного ограничения для объема см. в статье [Анализ использования данных в службе Log Analytics](usage-estimated-costs.md). Их необходимо тщательно рассмотреть, так как вы не сможете контролировать свои ресурсы после достижения предела. 

### <a name="manage-the-maximum-daily-data-volume"></a>Управление максимальным ежедневным объемом данных

Следующие шаги описывают, как настроить ограничения для управления объемом данных, рабочая область Log Analytics будет принимать в день.  

1. В рабочей области на панели слева выберите пункт **Usage and estimated costs** (Использование и ожидаемые затраты).
2. В верхней части страницы **Usage and estimated costs** (Использование и ожидаемые затраты) для выбранной рабочей области щелкните **Управление объемом данных**. 
3. По умолчанию для ежедневного ограничения установлено значение **Выкл**. Щелкните **Вкл.** , чтобы включить его, а затем настройте лимит для объема данных (ГБ/день).

    ![Log Analytics Настройка ограничения сбора данных](media/manage-cost-storage/set-daily-volume-cap-01.png)

### <a name="alert-when-daily-cap-reached"></a>Оповещение при достижении ежедневного ограничения

Когда достигнуто ограничение сбора данных, на портале Azure отображается соответствующее сообщение. Однако, возможно, вы управляете операционными проблемами, требующими немедленного внимания, иным образом.  Чтобы получать оповещение, вы можете создать новое правило генерации оповещений в Azure Monitor.  Дополнительные сведения см. в разделе [создание, просмотр и Управление оповещениями](alerts-metric.md).

Перед началом работы просмотрите рекомендуемые настройки для оповещения:

- Цель: выбор ресурса Log Analytics.
- Критерии: 
   - "Название сигнала" — "Поиск по пользовательским журналам";
   - "Поисковый запрос" — Operation | where Detail has 'OverQuota'
   - "На основе" — Количество результатов
   - "Условие" — Больше
   - "Пороговое значение" — 0
   - "Период" — 5 (минут);
   - "Частота" — 5 (минут);
- "Имя правила генерации оповещений" — "Достигнут ежедневный предел сбора данных";
- "Уровень серьезности" — "Предупреждение (серьезность 1)"

Как только будет определено оповещение и достигнут предел, активируется предупреждение и будет выполнен ответ, определенный в группе действий. Ваша команда может получать сообщение по электронной почте и текстовое сообщение, могут быть автоматизированы действия с помощью веб-перехватчиков, модулей Runbook службы автоматизации или же выполнена [интеграция с внешним решением ITSM](itsmc-overview.md#create-itsm-work-items-from-azure-alerts). 

## <a name="change-the-data-retention-period"></a>Изменение срока хранения данных

В следующих шагах описана настройка периода хранения данных журнала в рабочей области.
 
1. В рабочей области на панели слева выберите пункт **Usage and estimated costs** (Использование и ожидаемые затраты).
2. В верхней части страницы **Usage and estimated costs** (Использование и ожидаемые затраты) щелкните **Управление объемом данных**.
3. В области воспользуйтесь ползунком, чтобы увеличить или уменьшить количество дней, а затем нажмите кнопку **ОК**.  Если вы используете уровень *Бесплатный*, то вы не сможете изменить срок хранения данных. Необходимо перейти на платный уровень, чтобы управлять этим параметром.

    ![Изменение параметров хранения данных рабочей области](media/manage-cost-storage/manage-cost-change-retention-01.png)
    
Срок хранения может также быть [задаются ARM](https://docs.microsoft.com/azure/azure-monitor/platform/template-workspace-configuration#configure-a-log-analytics-workspace) с помощью `dataRetention` параметра. Кроме того, если задать срок хранения данных до 30 дней, вы можете активировать немедленное Очистка старых данных с помощью `immediatePurgeDataOn30Days` параметр, который может быть полезно для сценариев, связанные с соответствием требованиям. Эта функция выполняется только через ARM. 

## <a name="legacy-pricing-tiers"></a>Устаревшие ценовые категории

Подписки, которые имели рабочей области Log Analytics или ресурс Application Insights в нем до 2 апреля 2018 г., или связаны с соглашением Enterprise Agreement, начатые до 1 февраля 2019 г., продолжат получать доступ к прежних версий, ценовые категории: **Бесплатный**, **автономного (за ГБ)** и **за каждый узел (OMS)** .  Рабочие области в ценовой категории Free будет иметь ежедневный прием данных с ограничением на 500 МБ (за исключением безопасности типов данных, собранных с помощью центра безопасности Azure) и хранение данных ограничено до 7 дней. Ценовой категории Free предназначен только для ознакомительных целей. Рабочие области в автономном или на каждом узле ценовые категории имеют пользовательское хранение до 2 лет. Рабочие области, созданные до апреля 2016 г. также имеют доступ исходный **стандартный** и **уровня "премиум"** ценовые категории. Дополнительные сведения о ценах ограничения уровня доступны [здесь](https://docs.microsoft.com/azure/azure-subscription-service-limits#log-analytics-workspaces).

> [!NOTE]
> Чтобы использовать права, полученные при покупке подписки OMS E1, OMS E2 или настройки OMS для System Center, выберите ценовой уровень Log Analytics *за узле*.

Ранние последователи Log Analytics также имеют доступ к исходной ценовые категории **стандартный** и **уровня "премиум"** , который устранили хранение данных от 30 до 365 дней соответственно. 

## <a name="changing-pricing-tier"></a>Изменение ценовой категории.

Если ваша рабочая область Log Analytics имеет доступ к устаревшим уровням ценообразования, для переключения между устаревшими уровнями ценообразования выполните следующее.

1. На портале Azure в области подписок Log Analytics выберите рабочую область.

2. На панели рабочей области в разделе **Общие** щелкните **Ценовая категория**.  

3. В разделе **Ценовая категория** выберите ценовую категорию и щелкните **Выбрать**.  
    ![Выбранный ценовой план](media/manage-cost-storage/workspace-pricing-tier-info.png)

Вы также можете [установить ценовую через ARM](https://docs.microsoft.com/azure/azure-monitor/platform/template-workspace-configuration#configure-a-log-analytics-workspace) с помощью `ServiceTier` параметра. 

## <a name="troubleshooting-why-log-analytics-is-no-longer-collecting-data"></a>Почему Log Analytics больше не собирает данные

Если вы используете устаревшую бесплатную ценовую категорию и отправили больше 500 МБ данных за день, сбор данных останавливается до конца дня. Достижение ежедневного ограничения является распространенной причиной, по которой Log Analytics прекращает сбор данных или по которой данные отсутствуют.  Log Analytics создает событие типа "Операция", когда сбор данных начинается и останавливается. Выполните следующий запрос в поле поиска, чтобы проверить, достигнут ли лимит и отсутствуют ли данные: 

```kusto
Operation | where OperationCategory == 'Data Collection Status'
```

При остановке сбора данных — OperationStatus **предупреждение**. При запуске сбора данных — OperationStatus **успешно**. В следующей таблице описаны причины, по которым сбор данных останавливается, и приведены рекомендуемые действия, чтобы его возобновить:  

|Причина прекращения сбора| Решение| 
|-----------------------|---------|
|Достигнут ежедневный предел устаревшей бесплатной ценовой категории. |Дождитесь следующего дня для автоматического перезапуска сбора или перейдите на платную ценовую категорию.|
|Достигнуто ежедневное ограничение рабочей области.|Дождитесь автоматического перезапуска сбора или увеличьте предел ежедневного объема собираемых данных, как описано в разделе управления максимальным ежедневным объемом данных. Время сброса ежедневного ограничения отображается на странице **Управление объемом данных**. |
|Подписка Azure находится в состоянии "Приостановлено" по причине:<br> Период бесплатной пробной версии завершен<br> Истек срок действия Azure Pass<br> Достигнут лимит ежемесячной суммы расходов (например, на подписку MSDN или Visual Studio)|Измените подписку на платную<br> Удалите ограничение или подождите, пока оно сбросится|

Чтобы получать уведомления при остановке сбора данных, выполните действия, описанные в *Создание ежедневного ограничения данных* оповещение, чтобы получить уведомление о прекращении сбора данных. Выполните действия, описанные в [создать группу действий](action-groups.md) настроить действия электронной почты, веб-перехватчика или runbook для правила генерации оповещений. 

## <a name="troubleshooting-why-usage-is-higher-than-expected"></a>Превышенный объем данных: причины и устранение

Превышенное использование вызывается одной (или двумя) причинами:
- Больше узлов, чем ожидалось, отправляет данные в рабочую область Log Analytics
- Дополнительные данные, чем ожидалось, отправляемые в рабочей области Log Analytics

## <a name="understanding-nodes-sending-data"></a>Основные сведения об узлах, отправка данных

Чтобы определить количество компьютеров, сообщающих о периодических сигналов каждый день за последний месяц, с помощью

```kusto
Heartbeat | where TimeGenerated > startofday(ago(31d))
| summarize dcount(Computer) by bin(TimeGenerated, 1d)    
| render timechart
```

Чтобы получить список компьютеров, на которых будет оплачиваться как узлы, если рабочая область находится узел устаревших версий в ценовой категории, найдите узлы, отправляющие **выставлен счет типы данных** (некоторые типы данных предоставляются бесплатно). Чтобы сделать это, используйте `_IsBillable` [свойство](log-standard-properties.md#_isbillable) и использовать самым левым полем полное доменное имя. Эта команда возвращает список компьютеров с данными по документу:

```kusto
union withsource = tt * 
| where _IsBillable == true 
| extend computerName = tolower(tostring(split(Computer, '.')[0]))
| where computerName != ""
| summarize TotalVolumeBytes=sum(_BilledSize) by computerName
```

Можно оценить количество оплачиваемых узлы, отмеченные как: 

```kusto
union withsource = tt * 
| where _IsBillable == true 
| extend computerName = tolower(tostring(split(Computer, '.')[0]))
| where computerName != ""
| billableNodes=dcount(computerName)
```

> [!NOTE]
> Используйте эти запросы `union withsource = tt *` только в случае необходимости, так как сканирование по типам данных требует больших затрат на выполнение. Следующий запрос заменяет старый способ запроса на получение сведений для компьютера с типом данных об использовании.  

Более точные вычисления, выставляется фактически является для получения количества компьютеров в час, которые отправляют типы данных выставляется счет. (Для рабочих областей в устаревших ценовую категорию на каждом узле, Log Analytics подсчитывает количество узлов, которые должны выставляться на почасовой основе). 

```kusto
union withsource = tt * 
| where _IsBillable == true 
| extend computerName = tolower(tostring(split(Computer, '.')[0]))
| where computerName != ""
| summarize billableNodes=dcount(computerName) by bin(TimeGenerated, 1h) | sort by TimeGenerated asc
```

## <a name="understanding-ingested-data-volume"></a>Основные сведения о принимаемых данных тома

На странице **Использование и ожидаемые затраты** есть диаграмма *Data ingestion per solution* (Прием данных по решениям), которая позволяет отобразить объем отправленных данных в целом и для каждого решения отдельно. Это позволяет выявить некоторые тенденции, например оценить изменения общего объема используемых данных (или по определенному решению). Для создания этой диаграммы применяется такой запрос:

```kusto
Usage | where TimeGenerated > startofday(ago(31d))| where IsBillable == true
| summarize TotalVolumeGB = sum(Quantity) / 1024 by bin(TimeGenerated, 1d), Solution| render barchart
```

Обратите внимание, что предложение "where IsBillable = true" отсеивает выбранные типы данных из определенных решений, для которых не взимается плата за прием данных. 

Вы можете изучить данные еще подробнее, чтобы найти тенденции для определенных типов данных, например для данных из журналов IIS:

```kusto
Usage | where TimeGenerated > startofday(ago(31d))| where IsBillable == true
| where DataType == "W3CIISLog"
| summarize TotalVolumeGB = sum(Quantity) / 1024 by bin(TimeGenerated, 1d), Solution| render barchart
```

### <a name="data-volume-by-computer"></a>Объем данных в зависимости от компьютера

Чтобы увидеть **размер** оплачиваемых событий, обрабатываемых на каждом компьютере, используйте `_BilledSize` [свойство](log-standard-properties.md#_billedsize), предоставляющий размер в байтах:

```kusto
union withsource = tt * 
| where _IsBillable == true 
| extend computerName = tolower(tostring(split(Computer, '.')[0]))
| summarize Bytes=sum(_BilledSize) by  computerName | sort by Bytes nulls last
```

`_IsBillable` [Свойство](log-standard-properties.md#_isbillable) указывает, будет ли данные, полученные взиматься плата.

Чтобы просмотреть число **оплачиваемых** события, принятые на каждом компьютере, используйте 

```kusto
union withsource = tt * 
| where _IsBillable == true 
| extend computerName = tolower(tostring(split(Computer, '.')[0]))
| summarize eventCount=count() by computerName  | sort by count_ nulls last
```

Чтобы счетчики оплачиваемых типов данных отправляли данные на конкретный компьютер, выполните такой запрос:

```kusto
union withsource = tt *
| where Computer == "computer name"
| where _IsBillable == true 
| summarize count() by tt | sort by count_ nulls last
```

### <a name="data-volume-by-azure-resource-resource-group-or-subscription"></a>Объем данных в зависимости от ресурсов Azure, группу ресурсов или подписку

Для данных из узлов, размещенных в Azure можно получить **размер** оплачиваемых событий, которые принимаются __на каждом компьютере__, использовать _ResourceId [свойство](log-standard-properties.md#_resourceid), который предоставляет полный путь к ресурс:

```kusto
union withsource = tt * 
| where _IsBillable == true 
| summarize Bytes=sum(_BilledSize) by _ResourceId | sort by Bytes nulls last
```

Для данных из узлов, размещенных в Azure можно получить **размер** оплачиваемых событий, которые принимаются __каждой подписки Azure__, синтаксический анализ `_ResourceId` свойство как:

```kusto
union withsource = tt * 
| where _IsBillable == true 
| parse tolower(_ResourceId) with "/subscriptions/" subscriptionId "/resourcegroups/" 
    resourceGroup "/providers/" provider "/" resourceType "/" resourceName   
| summarize Bytes=sum(_BilledSize) by subscriptionId | sort by Bytes nulls last
```

Изменение `subscriptionId` для `resourceGroup` покажет том оплачиваемые данные, полученные по группе ресурсов Azure. 


> [!NOTE]
> Некоторые поля с типом данных Usage (Потребление) уже устарели и данные в них не заполняются, хотя они пока сохраняются в схеме. Например, сюда относятся поля **Computer** и ряд данных о приеме данных (**TotalBatches**, **BatchesWithinSla**, **BatchesOutsideSla**, **BatchesCapped** и **AverageProcessingTimeMs**).

### <a name="querying-for-common-data-types"></a>Запросы для общих типов данных

Чтобы получить более подробную информацию об источнике данных по определенному типу данных, воспользуйтесь приведенными ниже примерами запросов.

+ Решение по **безопасности**
  - `SecurityEvent | summarize AggregatedValue = count() by EventID`
+ Решение для **управления журналами**
  - `Usage | where Solution == "LogManagement" and iff(isnotnull(toint(IsBillable)), IsBillable == true, IsBillable == "true") == true | summarize AggregatedValue = count() by DataType`
+ Тип данных **Perf**
  - `Perf | summarize AggregatedValue = count() by CounterPath`
  - `Perf | summarize AggregatedValue = count() by CounterName`
+ Тип данных **Event**
  - `Event | summarize AggregatedValue = count() by EventID`
  - `Event | summarize AggregatedValue = count() by EventLog, EventLevelName`
+ Тип данных **Syslog**
  - `Syslog | summarize AggregatedValue = count() by Facility, SeverityLevel`
  - `Syslog | summarize AggregatedValue = count() by ProcessName`
+ Тип данных **AzureDiagnostics**
  - `AzureDiagnostics | summarize AggregatedValue = count() by ResourceProvider, ResourceId`

### <a name="tips-for-reducing-data-volume"></a>Советы по снижению объемов данных

Вот несколько рекомендаций, которые помогут снизить объем собираемых журналов.

| Источник превышенного объема данных | Как сократить объем данных |
| -------------------------- | ------------------------- |
| События безопасности            | Выберите [события со стандартным или минимальным уровнем безопасности](https://docs.microsoft.com/azure/security-center/security-center-enable-data-collection#data-collection-tier). <br> Измените политику аудита безопасности таким образом, чтобы собирать только необходимые события. В частности проверьте необходимость сбора следующих событий: <br> - [аудит платформы фильтрации](https://technet.microsoft.com/library/dd772749(WS.10).aspx); <br> - [аудит реестра](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd941614(v%3dws.10));<br> - [аудит файловой системы](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd772661(v%3dws.10));<br> - [аудит объектов ядра](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd941615(v%3dws.10));<br> - [аудит работы с дескрипторами](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd772626(v%3dws.10));<br> — аудит съемных носителей. |
| Счетчики производительности       | Измените [конфигурацию счетчика производительности](data-sources-performance-counters.md), чтобы <br> уменьшить частоту сбора или <br> сократить число счетчиков производительности. |
| Журналы событий                 | Измените [конфигурацию журнала событий](data-sources-windows-events.md), чтобы <br> сократить число собранных журналов событий или <br> выполнять сбор только необходимых уровней событий. Например, не выполняйте сбор событий уровня *сведений*. |
| Системный журнал                     | Измените [конфигурацию системного журнала](data-sources-syslog.md), чтобы <br> сократить число собранных объектов или <br> выполнять сбор только необходимых уровней событий. Например, не выполняйте сбор событий уровня *сведений* и *отладки*. |
| AzureDiagnostics           | Измените коллекцию журнала ресурсов, чтобы: <br> Уменьшить число ресурсов, отправляющих журналы в Log Analytics. <br> Выполнять сбор только необходимых журналов. |
| Данные решений с компьютеров, которым не требуется решение | Используйте [нацеливание решений](../insights/solution-targeting.md), чтобы выполнять сбор данных только в нужных группах компьютеров. |

### <a name="getting-security-and-automation-node-counts"></a>Получение счетчиков узел безопасности и автоматизации

Если вы используете ценовую категорию "За узел (OMS)", плата взимается в зависимости от количества используемых узлов и решений. Количество узлов для предложения "Аналитика", для которых выставляются счета, отображается в таблице на странице **Использование и предполагаемые затраты**.  

Чтобы просмотреть количество отдельных узлов безопасности, выполните такой запрос:

```kusto
union
(
    Heartbeat
    | where (Solutions has 'security' or Solutions has 'antimalware' or Solutions has 'securitycenter')
    | project Computer
),
(
    ProtectionStatus
    | where Computer !in~
    (
        (
            Heartbeat
            | project Computer
        )
    )
    | project Computer
)
| distinct Computer
| project lowComputer = tolower(Computer)
| distinct lowComputer
| count
```

Чтобы просмотреть количество отдельных узлов службы автоматизации, выполните такой запрос:

```kusto
 ConfigurationData 
 | where (ConfigDataType == "WindowsServices" or ConfigDataType == "Software" or ConfigDataType =="Daemons") 
 | extend lowComputer = tolower(Computer) | summarize by lowComputer 
 | join (
     Heartbeat 
       | where SCAgentChannel == "Direct"
       | extend lowComputer = tolower(Computer) | summarize by lowComputer, ComputerEnvironment
 ) on lowComputer
 | summarize count() by ComputerEnvironment | sort by ComputerEnvironment asc
```

## <a name="create-an-alert-when-data-collection-is-high"></a>Создать оповещение, когда сбор данных

В этом разделе описывается создание оповещения для следующих случаев:
- объем данных превышает заданный объем;
- объем данных превысит заданный объем.

Служба "Оповещения Azure" поддерживает [оповещения журналов](alerts-unified-log.md), использующие поисковые запросы. 

Следующий запрос содержит результат, когда за сутки собрано более 100 ГБ данных:

```kusto
union withsource = $table Usage 
| where QuantityUnit == "MBytes" and iff(isnotnull(toint(IsBillable)), IsBillable == true, IsBillable == "true") == true 
| extend Type = $table | summarize DataGB = sum((Quantity / 1024)) by Type 
| where DataGB > 100
```

Следующий запрос использует простую формулу для прогноза, что в течение 24 часов будет отправлено более 100 ГБ данных: 

```kusto
union withsource = $table Usage 
| where QuantityUnit == "MBytes" and iff(isnotnull(toint(IsBillable)), IsBillable == true, IsBillable == "true") == true 
| extend Type = $table 
| summarize EstimatedGB = sum(((Quantity * 8) / 1024)) by Type 
| where EstimatedGB > 100
```

Чтобы создать оповещения для различных объемов данных, измените значение "100" в запросах на число ГБ, для которых вы хотите создать оповещения.

Выполните действия, описанные в разделе о [создании оповещений журналов](alerts-metric.md), чтобы получать уведомления при превышении ожидаемого объема собираемых данных.

При создании оповещения для первого запроса, когда имеется объем данных, превышающий 100 ГБ в сутки, задайте параметрам следующие значения:  

- Для параметра **Определение условия оповещения** укажите вашу рабочую область Log Analytics в качестве целевого ресурса.
- Для **критериев оповещения** укажите следующее:
   - для **названия сигнала** выберите значение **Пользовательский поиск по журналам**;
   - **поисковому запросу** — `union withsource = $table Usage | where QuantityUnit == "MBytes" and iff(isnotnull(toint(IsBillable)), IsBillable == true, IsBillable == "true") == true | extend Type = $table | summarize DataGB = sum((Quantity / 1024)) by Type | where DataGB > 100`;
   - **логика оповещений** должна быть **основана на** *числе результатов*, а значение **условия** должно быть *больше* **порогового значения** *0*;
   - укажите **период времени** *1440* минут, а для **частоты оповещений** задайте каждые *60* минут, так как данные об использовании обновляются раз в час.
- В разделе **Определение сведений об оповещении** задайте такие значения:
   - **имени** — *объем данных, превышающий 100 ГБ в сутки*;
   - **серьезности** — *предупреждение*;

Укажите существующую или создайте новую [группу действий](action-groups.md), чтобы получать соответствующее уведомление, когда оповещение журнала соответствует заданным критериям.

При создании оповещения для второго запроса, когда спрогнозировано, что суточный объем данных превысит 100 ГБ, задайте параметрам следующие значения:

- Для параметра **Определение условия оповещения** укажите вашу рабочую область Log Analytics в качестве целевого ресурса.
- Для **критериев оповещения** укажите следующее:
   - для **названия сигнала** выберите значение **Пользовательский поиск по журналам**;
   - **поисковому запросу** — `union withsource = $table Usage | where QuantityUnit == "MBytes" and iff(isnotnull(toint(IsBillable)), IsBillable == true, IsBillable == "true") == true | extend Type = $table | summarize EstimatedGB = sum(((Quantity * 8) / 1024)) by Type | where EstimatedGB > 100`;
   - **логика оповещений** должна быть **основана на** *числе результатов*, а значение **условия** должно быть *больше* **порогового значения** *0*;
   - укажите **период времени** *180* минут, а для **частоты оповещений** задайте каждые *60* минут, так как данные об использовании обновляются один раз в час.
- В разделе **Определение сведений об оповещении** задайте такие значения:
   - **имени** — *объем данных, превышающий 100 ГБ в сутки*;
   - **серьезности** — *предупреждение*;

Укажите существующую или создайте новую [группу действий](action-groups.md), чтобы получать соответствующее уведомление, когда оповещение журнала соответствует заданным критериям.

Когда вы получите оповещение, выполните действия, описанные в следующем разделе, чтобы определить причину использования превышенного объема данных.

## <a name="limits-summary"></a>Сводная таблица ограничений

Существуют некоторые дополнительные ограничения Log Analytics, некоторые из которых зависят от Log Analytics, Ценовая категория. Все они описаны [здесь](https://docs.microsoft.com/azure/azure-subscription-service-limits#log-analytics-workspaces).


## <a name="next-steps"></a>Следующие шаги

- См. в разделе [при поиске по журналам в Azure Monitor журналы](../log-query/log-query-overview.md) вы научитесь использовать язык поиска. Вы можете использовать поисковые запросы, чтобы выполнить дополнительный анализ данных об использовании.
- Выполните действия, описанные в разделе о [создании оповещений журналов](alerts-metric.md), чтобы получать уведомления при выполнении условий поиска.
- Используйте [нацеливание решений](../insights/solution-targeting.md), чтобы выполнять сбор данных только в нужных группах компьютеров.
- Сведения о настройке эффективной политики сбора событий см. в статье [Сбор данных в Центре безопасности Azure](../../security-center/security-center-enable-data-collection.md).
- Измените [конфигурацию счетчика производительности](data-sources-performance-counters.md).
- Сведения об изменении параметров сбора событий см. в статье [Источники данных для журнала событий Windows в Log Analytics](data-sources-windows-events.md).
- Сведения об изменении конфигурации системного журнала см. в статье [Источники данных для журнала событий Windows в Log Analytics](data-sources-syslog.md).