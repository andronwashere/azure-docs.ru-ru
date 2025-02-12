---
title: Уведомление с помощью веб-перехватчика и классического оповещения метрики для системы за пределами Azure
description: Узнайте, как перенаправлять оповещения метрик Azure в другие системы за пределами Azure.
author: snehithm
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 04/03/2017
ms.author: snmuvva
ms.subservice: alerts
ms.openlocfilehash: 264f3eb042a3c29523ed93df93dfa6d45c00ae87
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "60345794"
---
# <a name="have-a-classic-metric-alert-notify-a-non-azure-system-using-a-webhook"></a>Уведомление с помощью веб-перехватчика и классического оповещения метрики для системы за пределами Azure
С помощью веб-перехватчика можно направлять уведомления об оповещениях Azure в другие системы для постобработки или выполнения настраиваемых действий. Веб-перехватчик можно использовать, чтобы направить оповещение к службам, которые отправляют SMS-сообщения, ведут журналы об ошибках, уведомляют членов команды в чате или службах обмена сообщениями либо выполняют другие действия. 

В этой статье описываются способы настройки веб-перехватчика для оповещения метрики Azure. В ней также показывается, как выглядят полезные данные HTTP POST для веб-перехватчика. Дополнительные сведения о настройке и схеме для оповещений журнала действий Azure см. в статье [Вызов веб-перехватчика для оповещений журнала действий Azure](alerts-log-webhook.md).

Оповещения Azure с помощью запроса HTTP POST помещают содержимое оповещения в формате JSON в универсальный код ресурса (URI) веб-перехватчика, указанный при создании оповещения. Схема определена далее в этой статье. Этот URI должен быть допустимой конечной точкой HTTP или HTTPS. При активации оповещений Azure размещает одну запись для каждого запроса.

## <a name="configure-webhooks-via-the-azure-portal"></a>Настройка веб-перехватчика на портале Azure
Чтобы добавить или обновить универсальный код ресурса (URI) веб-перехватчика на [портале Azure](https://portal.azure.com/), перейдите в раздел **создания и обновления оповещений**.

![Добавление панели правил генерации оповещений](./media/alerts-webhooks/Alertwebhook.png)

Вы также можете настроить оповещение так, чтобы при его активации в URI веб-перехватчика добавлялись соответствующие сведения. Для этого можно использовать [командлеты Azure PowerShell](../../azure-monitor/platform/powershell-quickstart-samples.md#create-metric-alerts), [кроссплатформенный интерфейс командной строки](../../azure-monitor/platform/cli-samples.md#work-with-alerts) или [REST API Azure Monitor](https://msdn.microsoft.com/library/azure/dn933805.aspx).

## <a name="authenticate-the-webhook"></a>Аутентификация веб-перехватчика
Для веб-перехватчика может использоваться аутентификация на основе маркеров. Универсальный код ресурса (URI) веб-перехватчика сохраняется вместе с идентификатором маркера. Например: `https://mysamplealert/webcallback?tokenid=sometokenid&someparameter=somevalue`

## <a name="payload-schema"></a>Схема полезных данных
Операция POST содержит следующие полезные данные и схему JSON для всех оповещений, связанных с метриками.

```JSON
{
    "status": "Activated",
    "context": {
        "timestamp": "2015-08-14T22:26:41.9975398Z",
        "id": "/subscriptions/s1/resourceGroups/useast/providers/microsoft.insights/alertrules/ruleName1",
        "name": "ruleName1",
        "description": "some description",
        "conditionType": "Metric",
        "condition": {
            "metricName": "Requests",
            "metricUnit": "Count",
            "metricValue": "10",
            "threshold": "10",
            "windowSize": "15",
            "timeAggregation": "Average",
            "operator": "GreaterThanOrEqual"
        },
        "subscriptionId": "s1",
        "resourceGroupName": "useast",
        "resourceName": "mysite1",
        "resourceType": "microsoft.foo/sites",
        "resourceId": "/subscriptions/s1/resourceGroups/useast/providers/microsoft.foo/sites/mysite1",
        "resourceRegion": "centralus",
        "portalLink": "https://portal.azure.com/#resource/subscriptions/s1/resourceGroups/useast/providers/microsoft.foo/sites/mysite1"
    },
    "properties": {
        "key1": "value1",
        "key2": "value2"
    }
}
```


| Поле | Обязательно | Фиксированный набор значений | Примечания |
|:--- |:--- |:--- |:--- |
| status |Да |Activated, Resolved |Состояние оповещения на основе заданных условий. |
| context |Да | |Контекст оповещения. |
| timestamp |Да | |Время активации оповещения |
| id |Да | |Каждое правило оповещения имеет уникальный идентификатор. |
| name |Да | |Имя оповещения. |
| description |Да | |Описание оповещения. |
| conditionType |Да |Metric, Event |Поддерживаются два типа оповещений: метрики и события. Оповещения метрики основаны на условии метрики. Оповещения событий основаны на событиях в журнале действий. Таким образом, это значение позволяет узнать тип оповещения — метрики или события. |
| condition |Да | |Специальные поля для проверки с учетом параметра **conditionType**. |
| metricName |Для оповещений на основе метрик | |Имя метрики, определяющей, что контролирует метрика. |
| metricUnit |Для оповещений на основе метрик |Bytes, BytesPerSecond, Count, CountPerSecond, Percent, Seconds |Допустимая метрикой единица Список допустимых значений см. См. [допустимые значения](https://msdn.microsoft.com/library/microsoft.azure.insights.models.unit.aspx). |
| metricValue |Для оповещений на основе метрик | |Фактическое значение метрики, которая вызвала оповещение. |
| threshold |Для оповещений на основе метрик | |Пороговое значение, при котором активируется оповещение. |
| windowSize |Для оповещений на основе метрик | |Период времени, используемый для отслеживания активности оповещения по пороговому значению. Значение должно составлять от 5 минут до 24 часов. Значение должно быть в формате длительности ISO 8601. |
| timeAggregation |Для оповещений на основе метрик |Average, Last, Maximum, Minimum, None, Total |Порядок объединения данных, собранных за определенный период. Значение по умолчанию — Average (Среднее). См. [допустимые значения](https://msdn.microsoft.com/library/microsoft.azure.insights.models.aggregationtype.aspx). |
| operator |Для оповещений на основе метрик | |Оператор, используемый для сравнения текущих данных метрик с заданным пороговым значением. |
| subscriptionId |Да | |Идентификатор подписки Azure. |
| resourceGroupName |Да | |Имя группы ресурсов для затронутого ресурса. |
| resourceName |Да | |Имя затронутого ресурса. |
| resourceType |Да | |Тип ресурса затронутого ресурса. |
| resourceId |Да | |Идентификатор ресурса затронутого ресурса. |
| resourceRegion |Да | |Регион или расположение затронутого ресурса. |
| portalLink |Да | |Прямая ссылка на страницу сводки по ресурсу на портале. |
| properties |Нет |Необязательно |Набор пар "ключ — значение", содержащих подробные сведения о событии. Например, `Dictionary<String, String>`. Поле свойства не является обязательным. В настраиваемом пользовательском интерфейсе или в рабочем процессе на основе приложения логики пользователи могут вводить пары "ключ — значение" для передачи в виде полезных сведений. Еще один способ передачи пользовательских свойств в веб-перехватчик — через сам универсальный код ресурса (URI) веб-перехватчика (в виде параметров запросов). |

> [!NOTE]
> Значение в поле **свойства** можно задать только с помощью [REST API Azure Monitor](https://msdn.microsoft.com/library/azure/dn933805.aspx).
>
>

## <a name="next-steps"></a>Дальнейшие действия
* Дополнительные сведения об оповещениях Azure и веб-перехватчиках см. в видео, посвященном [интеграции оповещений Azure с PagerDuty](https://go.microsoft.com/fwlink/?LinkId=627080).
* Узнайте больше о [выполнении скриптов службы автоматизации Azure для оповещений Azure](https://go.microsoft.com/fwlink/?LinkId=627081).
* Узнайте, как [использовать приложение логики для отправки сообщения SMS с помощью Twilio из оповещения Azure](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-text-message-with-logic-app).
* Узнайте, как [использовать приложение логики для отправки сообщения Slack из оповещения Azure](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-slack-with-logic-app).
* Узнайте, как [использовать приложение логики для отправки сообщения в очередь Azure из оповещения Azure](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-queue-with-logic-app).

