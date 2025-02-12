---
title: 'Устранение неполадок: предупреждения журнала в Azure Monitor | Документация Майкрософт'
description: Распространенные проблемы, ошибки и способы их устранения для правил генерации оповещений журнала в Azure.
author: msvijayn
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 10/29/2018
ms.author: vinagara
ms.subservice: alerts
ms.openlocfilehash: 03a6ea45577b4a4bf57501b1834f91438feb4e2b
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "66477859"
---
# <a name="troubleshoot-log-alerts-in-azure-monitor"></a>Устранение неполадок: предупреждения журнала в Azure Monitor  

В этой статье показано, как устранить распространенные неполадки, которые могут возникнуть при настройке оповещений журнала в Azure Monitor. Он также предоставляет способы решения распространенных проблем с помощью функции или настройки оповещений журнала. 

Термин *оповещения журнала* описываются правила, что огня на основе журнала запроса в [рабочей области Azure Log Analytics](../learn/tutorial-viewdata.md) или в [Azure Application Insights](../../azure-monitor/app/analytics.md). Дополнительные сведения о функциональные возможности, терминология и типы в [оповещениях журналов в Azure Monitor](../platform/alerts-unified-log.md).

> [!NOTE]
> В этой статье не считает случаях, когда на портале Azure отображаются активации правила оповещения и уведомления не выполняется, соответствующей группе действий. См. сведения в таких случаях [Создание групп действий на портале Azure и управление ими](../platform/action-groups.md).

## <a name="log-alert-didnt-fire"></a>Оповещение журналов не срабатывает

Ниже приведены некоторые распространенные причины, почему состояние настроенного [правила оповещения журнала в Azure Monitor](../platform/alerts-log.md) не показывает [как *инициируется* предполагаемое время](../platform/alerts-managing-alert-states.md). 

### <a name="data-ingestion-time-for-logs"></a>Время приема данных для журналов

Оповещения журнала периодически запускает запрос на основе [Log Analytics](../learn/tutorial-viewdata.md) или [Application Insights](../../azure-monitor/app/analytics.md). Так как Azure Monitor обрабатывает многие терабайты данных от тысяч клиентов из различных источников по всему миру, служба делает возможной различными временными задержками. Дополнительные сведения см. в разделе [время приема данных в журналах Azure Monitor](../platform/data-ingestion-time.md).

Чтобы избежать задержек, система ожидает и повторяет попытку запроса на оповещение несколько раз, если он обнаруживает, что необходимые данные еще не принимаются. Система имеет экспоненциально возрастающее время ожидания. Оповещения журнала только после данных доступна, так что задержка может быть вызвано медленным приема данных журнала. 

### <a name="incorrect-time-period-configured"></a>Неправильно настроен период времени

Как описано в статье на [терминология для оповещений журнала](../platform/alerts-unified-log.md#log-search-alert-rule---definition-and-types), период времени, указанному в конфигурации указывает диапазон времени для запроса. Запрос возвращает только записи, которые были созданы в течение этого периода. 

Период времени ограничивает данные, для запроса к журналу избежать злонамеренного использования, и он обходит любые команды времени (например **Назад**) используется в запросе к журналу. Например, если задан период 60 минут, а запрос выполняется в 13:15, то для запроса журнала используются только записи, созданные между 12:15 и 13:15. Если в запросе журнала используется время команду **назад (1d)** , в запросе только по-прежнему используется данных между 12:15 и 13:15:00, так как период времени имеет значение этого интервала.

Период времени в конфигурации проверьте, соответствует ли ваш запрос. Для примера, показанного ранее Если в запросе журнала используется **назад (1d)** с зеленой маркером, период времени должно быть присвоено 24 часа или 1440 минут (обозначается красным цветом). Этот параметр гарантирует, что запрос выполняется должным образом.

![Период времени](media/alert-log-troubleshoot/LogAlertTimePeriod.png)

### <a name="suppress-alerts-option-is-set"></a>Настроена отмена вывода оповещений

Как описано в шаге 8 статьи на [Создание правила оповещения журнала на портале Azure](../platform/alerts-log.md#managing-log-alerts-from-the-azure-portal), оповещения журнала содержат **отменить вывод оповещений** параметр, чтобы запретить действия активации и уведомления в течение времени. Таким образом можно подумать, что оповещение не срабатывают. По сути срабатывают, но было подавлено.  

![Отменить вывод оповещений](media/alert-log-troubleshoot/LogAlertSuppress.png)

### <a name="metric-measurement-alert-rule-is-incorrect"></a>Неправильно настроено правило генерации оповещений "Измерение метрик"

*Оповещения об измерении метрики журнала* — подтипов оповещений журнала, которые имеют специальные возможности и синтаксис ограниченных запроса на оповещение. Правило для оповещения об измерении метрики журнала требуется выходных данных быть метрик временных рядов запроса. То есть выходные данные — это таблица с distinct, одинакового по размеру временных периодов вместе с соответствующей агрегированные значения. 

Вы можете иметь дополнительные переменные в таблице наряду с **AggregatedValue**. Эти переменные могут использоваться для сортировки таблицы. 

Например предположим, что правило для оповещения об измерении метрики журнала был настроен в качестве:

- Запрос `search *| summarize AggregatedValue = count() by $table, bin(timestamp, 1h)`  
- период времени в 6 часов
- Пороговое значение 50
- логику оповещений три последовательных нарушений
- **Агрегатные при** выбрана в качестве **$table**

Поскольку команда содержит **суммировать... по** и предоставляет две переменные (**timestamp** и **$table**), система автоматически выберет **$table** для **Aggregate при** . Система сортирует таблицу результатов с помощью **$table** поле, как показано на следующем снимке экрана. Затем он смотрит, кратное **AggregatedValue** экземпляров для каждого типа таблицы (например **availabilityResults**) для просмотра, если бы три или более последовательные бреши.

![Измерение метрик выполнения запроса с несколькими значениями](media/alert-log-troubleshoot/LogMMQuery.png)

Так как **Aggregate при** определен на **$table**, данные сортируются по **$table** столбца (обозначается красным цветом). Затем мы группе и поиска типов **Aggregate при** поля. 

Например, для **$table**, значений в параметре **availabilityResults** будет считаться как один построения на сущность (о чем оранжевым цветом). В этом построения значение или сущности службы оповещений проверяет три последовательные бреши (обозначается зеленым цветом). Нарушений активировать оповещение для табличного значения **availabilityResults**. 

Аналогичным образом Если три последовательные бреши произойти для любых других значений **$table**, то же самое запускается другой уведомления о предупреждении. Службы оповещений автоматически сортирует значения в одной построения/сущности (о чем оранжевым цветом) по времени.

Теперь предположим, что изменения правила для оповещения об измерении метрики журнала и запрос был `search *| summarize AggregatedValue = count() by bin(timestamp, 1h)`. Остальной части конфигурации остается таким же, как раньше, включая логику оповещений для трех последовательных нарушений. **Aggregate при** параметр в данном случае является **timestamp** по умолчанию. Только одно значение предоставляется в запросе для **суммировать... по** (то есть **timestamp**). Как и в предыдущем примере выходные данные в конце выполнения бы, как показано ниже.

   ![Измерение метрик. выполнение запросов с сингулярная](media/alert-log-troubleshoot/LogMMtimestamp.png)

Так как **Aggregate при** определен на **timestamp**, данные сортируются на **timestamp** столбца (обозначается красным цветом). Затем выполняется группирование по **timestamp**. Например, значения для `2018-10-17T06:00:00Z` будет считаться как один построения на сущность (о чем оранжевым цветом). В этом построения значение или сущности, службы оповещений содержатся не последовательные бреши (так как каждый **timestamp** значение имеет только одну запись). Поэтому никогда не срабатывает. В этом случае необходимо либо:

- Добавить фиктивный или существующую переменную (как **$table**) для правильной сортировки с помощью **Aggregate при** поля.
- Перенастройте правила генерации оповещений, чтобы использовать логику оповещений на основе **всего нарушения** вместо этого.

## <a name="log-alert-fired-unnecessarily"></a>Оповещение журналов срабатывает без необходимости

Настроенное [правила оповещения журнала в Azure Monitor](../platform/alerts-log.md) может активироваться неожиданно, при просмотре в [оповещений Azure](../platform/alerts-managing-alert-states.md). В следующих разделах описаны некоторые распространенные причины.

### <a name="alert-triggered-by-partial-data"></a>Предупреждение активируется по частичным данным

Log Analytics и Application Insights могут быть задержки приема и обработки. При выполнении запроса на оповещение log, возможно, данные недоступны или только некоторые данные недоступны. Дополнительные сведения см. в разделе [время приема данных журнала в Azure Monitor](../platform/data-ingestion-time.md).

В зависимости от того, как вы настроили правило генерации оповещений за неверного вызова может произойти, если нет данных или часть данных в журналах во время выполнения предупреждений. В таких случаях рекомендуется изменить запрос на оповещение или конфигурации. 

Например если вы настраиваете правила оповещения журнала сработать, если число результатов из запроса аналитики — меньше 5, оповещение инициируется, если нет данных (запись ноль) или частичные результаты (одну запись). Но после задержки приема данных, один и тот же запрос с полным набором данных может предоставить результат 10 записей.

### <a name="alert-query-output-is-misunderstood"></a>Понимается неправильно — выходные данные запроса на оповещение

Вы предоставляете логику для оповещений журнала в аналитическом запросе. Запрос аналитики можно использовать различные больших данных и математические функции. Службы оповещений запущен запрос с интервалами, указанными с данными в течение указанного времени. Службы оповещений делает незначительные изменения в запрос, в зависимости от типа оповещений. Можно просмотреть это изменение в **выполняемый запрос** разделе **настройка логики сигнала** экрана:

![Выполняемый запрос](media/alert-log-troubleshoot/LogAlertPreview.png)

**Выполняемый запрос** поле — запускает службы оповещений журнала. Если вы хотите понять, какой вывод запроса на оповещение может быть, прежде чем создавать оповещения, можно запустить указанного запроса и период времени с помощью [портал аналитики](../log-query/portals.md) или [API аналитики](https://docs.microsoft.com/rest/api/loganalytics/).

## <a name="log-alert-was-disabled"></a>Оповещение журнала был отключен

В следующих разделах перечислены некоторые причины, почему Azure Monitor может отключить [правило генерации оповещений журнала](../platform/alerts-log.md).

### <a name="resource-where-the-alert-was-created-no-longer-exists"></a>Ресурс, где создания предупреждения больше не существует

Правила генерации оповещений журнала, созданные в Azure Monitor предназначенных для конкретного ресурса, например рабочей областью Azure Log Analytics, приложения Azure Application Insights и ресурс Azure. Служба оповещений журнала выполнит запрос analytics, предоставляемых в правило для указанного целевого объекта. Но после создания правила, пользователи часто работают для удаления из Azure--или перемещения внутри Azure — целевой объект правила генерации оповещений журнала. Так как целевой объект правила оповещения больше не является допустимым, произойдет сбой выполнения правила.

В таких случаях Azure Monitor отключает оповещения журнала и гарантирует, что плата не взимается без необходимости при правило не может выполняться непрерывно по расширяемой периода (например, за неделю). Чтобы узнать точное время, когда Azure Monitor отключена оповещения журнала через [журнал действий Azure](../../azure-resource-manager/resource-group-audit.md). В журнале действий Azure когда Azure Monitor отключает правило генерации оповещений журнала добавляется событие.

Ниже примере события в журнале действий Azure — для правило генерации оповещений, которая была отключена из-за сбоя последовательности.

```json
{
    "caller": "Microsoft.Insights/ScheduledQueryRules",
    "channels": "Operation",
    "claims": {
        "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/spn": "Microsoft.Insights/ScheduledQueryRules"
    },
    "correlationId": "abcdefg-4d12-1234-4256-21233554aff",
    "description": "Alert: test-bad-alerts is disabled by the System due to : Alert has been failing consistently with the same exception for the past week",
    "eventDataId": "f123e07-bf45-1234-4565-123a123455b",
    "eventName": {
        "value": "",
        "localizedValue": ""
    },
    "category": {
        "value": "Administrative",
        "localizedValue": "Administrative"
    },
    "eventTimestamp": "2019-03-22T04:18:22.8569543Z",
    "id": "/SUBSCRIPTIONS/<subscriptionId>/RESOURCEGROUPS/<ResourceGroup>/PROVIDERS/MICROSOFT.INSIGHTS/SCHEDULEDQUERYRULES/TEST-BAD-ALERTS",
    "level": "Informational",
    "operationId": "",
    "operationName": {
        "value": "Microsoft.Insights/ScheduledQueryRules/disable/action",
        "localizedValue": "Microsoft.Insights/ScheduledQueryRules/disable/action"
    },
    "resourceGroupName": "<Resource Group>",
    "resourceProviderName": {
        "value": "MICROSOFT.INSIGHTS",
        "localizedValue": "Microsoft Insights"
    },
    "resourceType": {
        "value": "MICROSOFT.INSIGHTS/scheduledqueryrules",
        "localizedValue": "MICROSOFT.INSIGHTS/scheduledqueryrules"
    },
    "resourceId": "/SUBSCRIPTIONS/<subscriptionId>/RESOURCEGROUPS/<ResourceGroup>/PROVIDERS/MICROSOFT.INSIGHTS/SCHEDULEDQUERYRULES/TEST-BAD-ALERTS",
    "status": {
        "value": "Succeeded",
        "localizedValue": "Succeeded"
    },
    "subStatus": {
        "value": "",
        "localizedValue": ""
    },
    "submissionTimestamp": "2019-03-22T04:18:22.8569543Z",
    "subscriptionId": "<SubscriptionId>",
    "properties": {
        "resourceId": "/SUBSCRIPTIONS/<subscriptionId>/RESOURCEGROUPS/<ResourceGroup>/PROVIDERS/MICROSOFT.INSIGHTS/SCHEDULEDQUERYRULES/TEST-BAD-ALERTS",
        "subscriptionId": "<SubscriptionId>",
        "resourceGroup": "<ResourceGroup>",
        "eventDataId": "12e12345-12dd-1234-8e3e-12345b7a1234",
        "eventTimeStamp": "03/22/2019 04:18:22",
        "issueStartTime": "03/22/2019 04:18:22",
        "operationName": "Microsoft.Insights/ScheduledQueryRules/disable/action",
        "status": "Succeeded",
        "reason": "Alert has been failing consistently with the same exception for the past week"
    },
    "relatedEvents": []
}
```

### <a name="query-used-in-a-log-alert-is-not-valid"></a>Недопустимый запрос, используемый в оповещении журнала

Каждого правила генерации оповещений журнала, созданные в Azure Monitor в процессе его настройки необходимо указать запрос analytics, периодически срабатывающий службы оповещений. Запрос аналитики может иметь правильный синтаксис во время создания правила или обновления. Но в некоторых случаях за период времени, указанный запрос в правило генерации оповещений журнала можно разрабатывать проблем синтаксиса и привести к сбою выполнения правила. Ниже приведены некоторые распространенные причины, почему запрос analytics, в правило генерации оповещений журнала можно разработать ошибки.

- Запрос записывается в [выполнения по нескольким ресурсам](../log-query/cross-workspace-query.md). И один или несколько указанных ресурсов больше не существует.
- [оповещения об измерении метрики тип журнала](../../azure-monitor/platform/alerts-unified-log.md#metric-measurement-alert-rules) настроен есть оповещение нормы синтаксис запроса не соответствует
- Произошел, нет потоков данных на платформу аналитики. [Выполнение запроса приводит к возникновению ошибки](https://dev.loganalytics.io/documentation/Using-the-API/Errors) , так как нет данных для предоставленного запроса.
- Изменения в [язык запросов](https://docs.microsoft.com/azure/kusto/query/) включают измененный формат для команд и функций. Поэтому запрос, приведенный ранее в правило генерации оповещений больше не является допустимым.

[Помощник по Azure](../../advisor/advisor-overview.md) предупреждает об этом поведении. Добавлен рекомендацию для определенного журнала правило генерации оповещений в помощнике по Azure, в категории с высоким уровнем доступности с со средним уровнем влияния и описание «Восстановить правила генерации оповещений журнала для обеспечения мониторинга». Если запрос предупреждения в правило генерации оповещений журнала не исправлено после рекомендацию в течение семи дней предоставленными помощником по Azure, Azure Monitor отключит оповещения журнала и убедитесь, что плата не взимается без необходимости при правило не может выполняться непрерывно для расширяемой периода ( как и за неделю).

Можно найти точное время, когда Azure Monitor отключить правило генерации оповещений журнала, просматривая события в [журнал действий Azure](../../azure-resource-manager/resource-group-audit.md).

## <a name="next-steps"></a>Дальнейшие действия

- Ознакомьтесь со сведениями об [оповещениях журналов в Azure](../platform/alerts-unified-log.md).
- Дополнительные сведения об [Application Insights](../../azure-monitor/app/analytics.md).
- Дополнительные сведения о [журнал запросов](../log-query/log-query-overview.md).
