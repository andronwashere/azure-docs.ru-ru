---
title: Сохраненные поисковые запросы в решениях по управлению | Документация Майкрософт
description: Решения по управлению обычно включают в себя возможность сохранения поисковых запросов в Log Analytics для анализа данных, собранных этим решением. Они могут также определить оповещения для уведомления пользователя или автоматического выполнения действия в ответ на критическую ошибку. В этой статье описывается, как определить сохраненные поиски в шаблоне Resource Manager, поэтому они могут быть включены в решения по управлению Log Analytics.
services: monitoring
documentationcenter: ''
author: bwren
manager: carmonm
editor: tysonn
ms.service: azure-monitor
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/27/2019
ms.author: bwren
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 0975b23a8f96da6fc2dfcc8bd9ad046847a68aa9
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "62104836"
---
# <a name="adding-log-analytics-saved-searches-and-alerts-to-management-solution-preview"></a>Добавление сохраненных поисковых запросов и оповещений Log Analytics в решение по управлению (предварительная версия)

> [!IMPORTANT]
> Сведения о создании оповещений с помощью шаблона Resource Manager устарелые, поскольку [оповещения Log Analytics были расширены к Azure Monitor](../platform/alerts-extend.md). Дополнительные сведения о создании оповещений журнала с помощью шаблона Resource Manager см. в разделе [Управление оповещениями журнала с помощью шаблона ресурсов Azure](../platform/alerts-log.md#managing-log-alerts-using-azure-resource-template).

> [!NOTE]
> Это предварительная версия документации по созданию решений по управлению, доступных в режиме предварительной версии. Любые схемы, приведенные ниже, могут измениться.

[Решения по управлению](solutions.md) обычно включают в себя возможность [сохранения поисковых запросов](../../azure-monitor/log-query/log-query-overview.md) в Log Analytics для анализа данных, собранных этим решением. Они могут также определять [оповещения](../../azure-monitor/platform/alerts-overview.md) для уведомления пользователя или автоматического выполнения действия в ответ на критическую ошибку. В этой статье описывается, как определить сохраненные поиски и оповещения Log Analytics в [шаблоне Resource Manager](../../azure-resource-manager/resource-manager-quickstart-create-templates-use-the-portal.md), чтобы их можно было добавлять в [решения для управления](solutions-creating.md).

> [!NOTE]
> В примерах этой статьи используются обязательные или общие параметры и переменные для решений по управлению, описанные в статье [Проектирование и сборка решения по управлению в Azure (предварительная версия)](solutions-creating.md).

## <a name="prerequisites"></a>Технические условия
В этой статье предполагается, что вы уже знаете, как [создать решение по управлению](solutions-creating.md), а также знакомы со структурой [шаблона ARM](../../azure-resource-manager/resource-group-authoring-templates.md) и файла решения.


## <a name="log-analytics-workspace"></a>Рабочая область Log Analytics
Все ресурсы в Log Analytics размещаются в [рабочей области](../../azure-monitor/platform/manage-access.md). Как описано в разделе [Рабочая область Log Analytics и учетная запись службы автоматизации](solutions.md#log-analytics-workspace-and-automation-account), рабочая область не включена в решение по управлению и ее нужно создать до его установки. Если она недоступна, установка решения завершается сбоем.

Имя рабочей области указывается в имени каждого ресурса Log Analytics. Это сделано в решении с параметром **workspace**, как в следующем примере ресурса SavedSearch.

    "name": "[concat(parameters('workspaceName'), '/', variables('SavedSearchId'))]"

## <a name="log-analytics-api-version"></a>Версия API Log Analytics
Все ресурсы Log Analytics, определенные в шаблоне Resource Manager, имеют свойство **apiVersion**. В нем указывается версия API, которую должен использовать этот ресурс.

В следующей таблице перечислены версии API для ресурсов, используемых в этом примере.

| Тип ресурса | Версия API | Запрос |
|:---|:---|:---|
| savedSearches | 2017-03-15-preview | Event &#124; where EventLevelName == "Error"  |


## <a name="saved-searches"></a>Сохраненные поиски
Добавьте в решение функцию [сохраненных поисков](../../azure-monitor/log-query/log-query-overview.md), чтобы пользователи могли запрашивать данные, собранные решением. Сохраненные поисковые запросы отображаются в разделе **Сохраненные условия поиска** на портале Azure. Сохраненный поиск также является обязательным для каждого оповещения.

Ресурсы [сохраненных поисков Log Analytics](../../azure-monitor/log-query/log-query-overview.md) имеют тип `Microsoft.OperationalInsights/workspaces/savedSearches`. Их структура приведена ниже. Далее представлены общие переменные и параметры, чтобы этот фрагмент кода можно было скопировать и вставить в файл решения и изменить имена параметров.

    {
        "name": "[concat(parameters('workspaceName'), '/', variables('SavedSearch').Name)]",
        "type": "Microsoft.OperationalInsights/workspaces/savedSearches",
        "apiVersion": "[variables('LogAnalyticsApiVersion')]",
        "dependsOn": [
        ],
        "tags": { },
        "properties": {
            "etag": "*",
            "query": "[variables('SavedSearch').Query]",
            "displayName": "[variables('SavedSearch').DisplayName]",
            "category": "[variables('SavedSearch').Category]"
        }
    }

В следующей таблице описаны все свойства сохраненного условия поиска.

| Свойство | description |
|:--- |:--- |
| category | Категория для сохраненного поиска.  Сохраненные поиски в одном решении часто будут принадлежать одной категории, поэтому они группируются в консоли. |
| displayName | Имя, отображаемое для сохраненного поиска на портале. |
| query | Запрос для выполнения. |

> [!NOTE]
> В запросе необходимо использовать escape-символы, если он содержит знаки, которые можно интерпретировать как JSON. Например, запрос **AzureActivity | OperationName:"Microsoft.Compute/virtualMachines/write"** в файл решения должен быть записан как **AzureActivity | OperationName:/\"Microsoft.Compute/virtualMachines/write\"** .

## <a name="alerts"></a>Оповещения
[Оповещения журналов Azure](../../azure-monitor/platform/alerts-unified-log.md) создаются правилами оповещений Azure, выполняющими указанные запросы журналов с регулярными интервалами. Если результаты запроса соответствуют указанным условиям, то создается запись оповещения и выполняются одно или несколько действий с помощью [групп действий](../../azure-monitor/platform/action-groups.md).

> [!NOTE]
> Начиная с 14 мая 2018 г., все оповещения в экземпляре публичного облака Azure рабочей области Log Analytics начали распространяться на Azure. Дополнительные сведения см. в статье об [отображении оповещений в Azure](../../azure-monitor/platform/alerts-extend.md). Теперь действия пользователей, которые отображают оповещения в Azure, контролируются в группах действий Azure. Если рабочая область и ее оповещения перенесены в Azure, можно извлечь или добавить действия с помощью [группы действий (шаблон Azure Resource Manager)](../../azure-monitor/platform/action-groups-create-resource-manager-template.md).
Правила генерации оповещений в решении для управления состоят из трех различных ресурсов.

- **Сохраненный поиск.** Определяет выполняемый поиск по журналам. Один сохраненный поиск может использоваться несколькими правилами генерации оповещений.
- **Расписание.** Определяет, как часто будет выполняться поиск по журналам. У каждого правила генерации оповещений есть только одно расписание.
- **Действие оповещения.** У каждого правила генерации оповещений есть один ресурс группы действий или ресурс действий (устарел) типа **Alert**, который определяет параметры оповещения, например условия создания записи оповещения и его серьезность. Для ресурса [Группа действий](../../azure-monitor/platform/action-groups.md) может быть настроена группа действий, выполняемых при возникновении оповещения, например голосовой звонок, отправка SMS или сообщения по электронной почте, срабатывание веб-перехватчика, инструмента ITSM, runbook службы автоматизации, приложения логики и т. д.

Ресурс действия (устарел) может также определять электронную почту и Runbook для ответа.
- **Действие веб-перехватчика (устарело).** Если правило генерации оповещений вызывает веб-перехватчик, ему требуется дополнительный ресурс действия с типом **Webhook**.

Ресурсы сохраненных поисков описаны выше. Другие ресурсы описываются ниже.

### <a name="schedule-resource"></a>Ресурс расписания

У сохраненного поиска может быть одно или несколько расписаний, представляющих отдельное правило генерации оповещений. Расписание определяет, как часто выполняется поиск, а также интервал времени для извлечения данных. Ресурсы расписания имеют тип `Microsoft.OperationalInsights/workspaces/savedSearches/schedules/`. Их структура приведена ниже. Далее представлены общие переменные и параметры, чтобы этот фрагмент кода можно было скопировать и вставить в файл решения и изменить имена параметров.

    {
        "name": "[concat(parameters('workspaceName'), '/', variables('SavedSearch').Name, '/', variables('Schedule').Name)]",
        "type": "Microsoft.OperationalInsights/workspaces/savedSearches/schedules/",
        "apiVersion": "[variables('LogAnalyticsApiVersion')]",
        "dependsOn": [
            "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'), '/savedSearches/', variables('SavedSearch').Name)]"
        ],
        "properties": {
            "etag": "*",
            "interval": "[variables('Schedule').Interval]",
            "queryTimeSpan": "[variables('Schedule').TimeSpan]",
            "enabled": "[variables('Schedule').Enabled]"
        }
    }
В следующей таблице описаны свойства ресурсов расписания.

| Имя элемента | Обязательно для заполнения | description |
|:--|:--|:--|
| enabled       | Да | Указывает, включено ли оповещение при его создании. |
| interval      | Да | Как часто выполняется запрос (в минутах). |
| queryTimeSpan | Да | Интервал времени в минутах, на котором следует оценивать результаты. |

Ресурс расписания будет зависеть от сохраненного поиска, поэтому он должен быть создан перед расписанием.
> [!NOTE]
> В любой рабочей области имя расписания должно быть уникальным. Нельзя использовать один и тот же идентификатор для двух расписаний, даже если они связаны с разными сохраненными поисковыми запросами. Кроме того, имена всех сохраненных поисковых запросов, расписаний и действий, созданных при помощи API Log Analytics, должны писаться строчными буквами.

### <a name="actions"></a>Действия
Расписание может включать несколько действий. Действие может определять один выполняемый процесс или несколько (например, отправку почты или запуск модуля Runbook) или задавать пороговое значение, определяющее, когда результаты поиска отвечают некоторым условиям. Некоторые действия будут определять оба элемента так, чтобы процессы выполнялись при достижении порогового значения.
Действия можно определить при помощи ресурса [группы действий] или ресурса действия.
> [!NOTE]
> Начиная с 14 мая 2018 г., все оповещения в экземпляре публичного облака Azure рабочей области Log Analytics начали автоматически распространяться на Azure. Дополнительные сведения см. в статье об [отображении оповещений в Azure](../../azure-monitor/platform/alerts-extend.md). Теперь действия пользователей, которые отображают оповещения в Azure, контролируются в группах действий Azure. Если рабочая область и ее оповещения перенесены в Azure, можно извлечь или добавить действия с помощью [группы действий (шаблон Azure Resource Manager)](../../azure-monitor/platform/action-groups-create-resource-manager-template.md).
Существуют два типа ресурса действия, которые задает свойство **Type**. Для расписания требуется одно действие **Alert**, которое определяет сведения о правиле генерации оповещений и действия, предпринимаемые при создании оповещения. Ресурсы действия имеют тип `Microsoft.OperationalInsights/workspaces/savedSearches/schedules/actions`.

Структура действий оповещений приведена ниже. Далее представлены общие переменные и параметры, чтобы этот фрагмент кода можно было скопировать и вставить в файл решения и изменить имена параметров.

```json
{
    "name": "[concat(parameters('workspaceName'), '/', variables('SavedSearch').Name, '/', variables('Schedule').Name, '/', variables('Alert').Name)]",
    "type": "Microsoft.OperationalInsights/workspaces/savedSearches/schedules/actions",
    "apiVersion": "[variables('LogAnalyticsApiVersion')]",
    "dependsOn": [
        "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'), '/savedSearches/', variables('SavedSearch').Name, '/schedules/', variables('Schedule').Name)]"
    ],
    "properties": {
        "etag": "*",
        "type": "Alert",
        "name": "[variables('Alert').Name]",
        "description": "[variables('Alert').Description]",
        "severity": "[variables('Alert').Severity]",
        "threshold": {
            "operator": "[variables('Alert').Threshold.Operator]",
            "value": "[variables('Alert').Threshold.Value]",
            "metricsTrigger": {
                "triggerCondition": "[variables('Alert').Threshold.Trigger.Condition]",
                "operator": "[variables('Alert').Trigger.Operator]",
                "value": "[variables('Alert').Trigger.Value]"
            },
        },
        "AzNsNotification": {
            "GroupIds": "[variables('MyAlert').AzNsNotification.GroupIds]",
            "CustomEmailSubject": "[variables('MyAlert').AzNsNotification.CustomEmailSubject]",
            "CustomWebhookPayload": "[variables('MyAlert').AzNsNotification.CustomWebhookPayload]"
        }
    }
}
```

В следующей таблице описаны свойства ресурсов действия оповещения.

| Имя элемента | Обязательно для заполнения | description |
|:--|:--|:--|
| type | Да | Тип действия.  Для действий оповещения это тип **Alert**. |
| name | Да | Отображаемое имя оповещения.  Это имя, которое отображается в консоли для правила генерации оповещений. |
| description | Нет | Необязательное описание оповещения. |
| severity | Да | Серьезность записи оповещения состоит из следующих значений.<br><br> **критический**<br>**предупреждение**<br>**информационный**


#### <a name="threshold"></a>Threshold (Пороговое значение)
Это обязательный раздел. Он определяет свойства порога оповещения.

| Имя элемента | Обязательно для заполнения | description |
|:--|:--|:--|
| Operator | Да | Оператор сравнения может содержать следующие значения:<br><br>**gt — больше;<br>lt — меньше.** |
| Value | Да | Значение для сравнения с результатами. |

##### <a name="metricstrigger"></a>MetricsTrigger
Это необязательный раздел. Он добавляется для оповещения об измерении метрики.

> [!NOTE]
> Оповещения об измерении метрики в настоящее время находятся на этапе общедоступной предварительной версии.

| Имя элемента | Обязательно для заполнения | description |
|:--|:--|:--|
| TriggerCondition | Да | Указывает, задает ли порог общее число нарушений или число последовательных нарушений посредством следующих значений:<br><br>**Total;<br>Consecutive.** |
| Operator | Да | Оператор сравнения может содержать следующие значения:<br><br>**gt — больше;<br>lt — меньше.** |
| Value | Да | Определяет, сколько раз должны быть соблюдены условия для активации оповещения. |


#### <a name="throttling"></a>Регулирование
Это необязательный раздел. Добавьте этот раздел, если некоторое время после создания оповещения тем же правилом его не нужно отображать.

| Имя элемента | Обязательно для заполнения | description |
|:--|:--|:--|
| DurationInMinutes | Значение Yes, если добавлен элемент Throttling. | Количество минут, в течение которых оповещение не отображается после его создания тем же правилом генерации оповещений. |

#### <a name="azure-action-group"></a>Группа действий Azure
Все оповещения в Azure используют группу действий в качестве механизма обработки действий по умолчанию. С помощью группы действий можно указать действия один раз, а затем связать группу действий для несколькими оповещениями в Azure. Это избавляет от необходимости повторно объявить одни и те же действия снова и снова. Группы действий поддерживает несколько действий, включая сообщения электронной почты, текстовые сообщения, голосовые вызовы, подключения ITSM, runbook службы автоматизации, универсальный код ресурса (URI) веб-перехватчика и многое другое.

Теперь для пользователей, перенесших оповещения в Azure, расписание должно передавать сведения о группе действий вместе с пороговым значением, чтобы обеспечить создание оповещения. Сведения о сообщениях электронной почты, URL-адреса веб-перехватчиков, данные runbook службы автоматизации и другие действия следует определить в группе действий перед созданием оповещения. Вы можете создать [группу действий в Azure Monitor](../../azure-monitor/platform/action-groups.md) на портале или использовать [группу действий (шаблон ресурса)](../../azure-monitor/platform/action-groups-create-resource-manager-template.md).

| Имя элемента | Обязательно для заполнения | description |
|:--|:--|:--|
| AzNsNotification | Да | Идентификатор ресурса группы действий Azure, который необходимо связать с оповещением для выполнения необходимых действий при удовлетворении условий генерации оповещения. |
| CustomEmailSubject | Нет | Пользовательская строка темы в почтовых сообщениях, отправляемых на все адреса, указанные в соответствующей группе действий. |
| CustomWebhookPayload | Нет | Настраиваемые полезные данные для отправки всем конечным точкам веб-перехватчика, указанным в соответствующей группе действий. Формат зависит от того, что веб-перехватчик ожидает получить, и должен представлять собой правильно сформированный сериализованный JSON. |

#### <a name="actions-for-oms-legacy"></a>Действия для OMS (для прежних версий)

У каждого расписания есть одно действие **Alert**. Оно определяет данные оповещения и, при необходимости, уведомление и действия для исправления Уведомление позволяет отправить электронное сообщение на один или несколько адресов. Исправление позволяет запустить Runbook в службе автоматизации Azure, чтобы попытаться устранить обнаруженную проблему.

> [!NOTE]
> Начиная с 14 мая 2018 г., все оповещения в экземпляре публичного облака Azure рабочей области Log Analytics начали автоматически распространяться на Azure. Дополнительные сведения см. в статье об [отображении оповещений в Azure](../../azure-monitor/platform/alerts-extend.md). Теперь действия пользователей, которые отображают оповещения в Azure, контролируются в группах действий Azure. Если рабочая область и ее оповещения перенесены в Azure, можно извлечь или добавить действия с помощью [группы действий (шаблон Azure Resource Manager)](../../azure-monitor/platform/action-groups-create-resource-manager-template.md).

##### <a name="emailnotification"></a>EmailNotification
 Этот раздел является необязательным. Добавьте его, если требуется, чтобы оповещение отправляло электронное сообщение одному или нескольким получателям.

| Имя элемента | Обязательно для заполнения | description |
|:--|:--|:--|
| Recipients | Да | Разделенный запятыми список электронных адресов, на которые отправляются уведомления при создании оповещения, как показано в приведенном ниже примере.<br><br>**[«recipient1\@contoso.com», «получатель2\@contoso.com»]** |
| Subject | Да | Строка темы электронного сообщения. |
| Attachment | Нет | Вложения в настоящее время не поддерживаются. Если этот элемент добавлен, он должен иметь значение **None**. |

##### <a name="remediation"></a>Исправление
Этот раздел является необязательным. Добавьте его, если требуется, чтобы в ответ на оповещение запускался Runbook. 

| Имя элемента | Обязательно для заполнения | description |
|:--|:--|:--|
| RunbookName | Да | Имя запускаемого модуля Runbook. |
| WebhookUri | Да | Универсальный код ресурса (URI) webhook для Runbook. |
| Expiry | Нет | Дата и время истечения срока действия исправления. |

##### <a name="webhook-actions"></a>Действия webhook

Действия webhook запускают процесс путем вызова URL-адреса и, при необходимости, предоставления полезных данных для отправки. Они похожи на действия исправления, но предназначены для объектов webhook, которые могут вызывать процессы, отличные от модулей Runbook службы автоматизации Azure. Они также включают дополнительную возможность предоставления полезных данных, доставляемых в удаленный процесс.

Если оповещение будет вызывать webhook, то помимо ресурса действия **Alert** ему требуется ресурс действия типа **Webhook**.

    {
      "name": "name": "[concat(parameters('workspaceName'), '/', variables('SavedSearch').Name, '/', variables('Schedule').Name, '/', variables('Webhook').Name)]",
      "type": "Microsoft.OperationalInsights/workspaces/savedSearches/schedules/actions/",
      "apiVersion": "[variables('LogAnalyticsApiVersion')]",
      "dependsOn": [
        "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'), '/savedSearches/', variables('SavedSearch').Name, '/schedules/', variables('Schedule').Name)]"
      ],
      "properties": {
        "etag": "*",
        "type": "[variables('Alert').Webhook.Type]",
        "name": "[variables('Alert').Webhook.Name]",
        "webhookUri": "[variables('Alert').Webhook.webhookUri]",
        "customPayload": "[variables('Alert').Webhook.CustomPayLoad]"
      }
    }
В следующей таблице описаны свойства ресурсов действия webhook.

| Имя элемента | Обязательно для заполнения | description |
|:--|:--|:--|
| тип | Да | Тип действия. Объект **Webhook** для действий веб-перехватчика. |
| name | Да | Отображаемое имя действия. Оно не отображается в консоли. |
| webhookUri | Да | Универсальный код ресурса (URI) webhook. |
| CustomPayload | Нет | Пользовательские полезные данные, отправляемые в объект webhook. Формат зависит от того, какие данные ожидает веб-перехватчик. |

## <a name="sample"></a>Образец

Ниже приведен пример решения со следующими ресурсами:

- Сохраненный поиск
- Расписание
- Группа действий

В примере используются переменные [стандартных параметров решения]( solutions-solution-file.md#parameters), что является общепринятой практикой для решений, в отличие от жестко программируемых значений в определениях ресурсов.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0",
    "parameters": {
        "workspaceName": {
            "type": "string",
            "metadata": {
                "Description": "Name of Log Analytics workspace"
            }
        },
        "workspaceregionId": {
            "type": "string",
            "metadata": {
                "Description": "Region of Log Analytics workspace"
            }
        },
        "actiongroup": {
            "type": "string",
            "metadata": {
                "Description": "List of action groups for alert actions separated by semicolon"
            }
        }
    },
    "variables": {
        "SolutionName": "MySolution",
        "SolutionVersion": "1.0",
        "SolutionPublisher": "Contoso",
        "ProductName": "SampleSolution",
        "LogAnalyticsApiVersion-Search": "2017-03-15-preview",
        "LogAnalyticsApiVersion-Solution": "2015-11-01-preview",
        "MySearch": {
            "displayName": "Error records by hour",
            "query": "MyRecord_CL | summarize AggregatedValue = avg(Rating_d) by Instance_s, bin(TimeGenerated, 60m)",
            "category": "Samples",
            "name": "Samples-Count of data"
        },
        "MyAlert": {
            "Name": "[toLower(concat('myalert-',uniqueString(resourceGroup().id, deployment().name)))]",
            "DisplayName": "My alert rule",
            "Description": "Sample alert. Fires when 3 error records found over hour interval.",
            "Severity": "critical",
            "ThresholdOperator": "gt",
            "ThresholdValue": 3,
            "Schedule": {
                "Name": "[toLower(concat('myschedule-',uniqueString(resourceGroup().id, deployment().name)))]",
                "Interval": 15,
                "TimeSpan": 60
            },
            "MetricsTrigger": {
                "TriggerCondition": "Consecutive",
                "Operator": "gt",
                "Value": 3
            },
            "ThrottleMinutes": 60,
            "AzNsNotification": {
                "GroupIds": [
                    "[parameters('actiongroup')]"
                ],
                "CustomEmailSubject": "Sample alert"
            }
        }
    },
    "resources": [
        {
            "name": "[concat(variables('SolutionName'), '[' ,parameters('workspacename'), ']')]",
            "location": "[parameters('workspaceRegionId')]",
            "tags": { },
            "type": "Microsoft.OperationsManagement/solutions",
            "apiVersion": "[variables('LogAnalyticsApiVersion-Solution')]",
            "dependsOn": [
                "[resourceId('Microsoft.OperationalInsights/workspaces/savedSearches', parameters('workspacename'), variables('MySearch').Name)]",
                "[resourceId('Microsoft.OperationalInsights/workspaces/savedSearches/schedules', parameters('workspacename'), variables('MySearch').Name, variables('MyAlert').Schedule.Name)]",
                "[resourceId('Microsoft.OperationalInsights/workspaces/savedSearches/schedules/actions', parameters('workspacename'), variables('MySearch').Name, variables('MyAlert').Schedule.Name, variables('MyAlert').Name)]"
            ],
            "properties": {
                "workspaceResourceId": "[resourceId('Microsoft.OperationalInsights/workspaces', parameters('workspacename'))]",
                "referencedResources": [
                ],
                "containedResources": [
                    "[resourceId('Microsoft.OperationalInsights/workspaces/savedSearches', parameters('workspacename'), variables('MySearch').Name)]",
                    "[resourceId('Microsoft.OperationalInsights/workspaces/savedSearches/schedules', parameters('workspacename'), variables('MySearch').Name, variables('MyAlert').Schedule.Name)]",
                    "[resourceId('Microsoft.OperationalInsights/workspaces/savedSearches/schedules/actions', parameters('workspacename'), variables('MySearch').Name, variables('MyAlert').Schedule.Name, variables('MyAlert').Name)]"
                ]
            },
            "plan": {
                "name": "[concat(variables('SolutionName'), '[' ,parameters('workspaceName'), ']')]",
                "Version": "[variables('SolutionVersion')]",
                "product": "[variables('ProductName')]",
                "publisher": "[variables('SolutionPublisher')]",
                "promotionCode": ""
            }
        },
        {
            "name": "[concat(parameters('workspaceName'), '/', variables('MySearch').Name)]",
            "type": "Microsoft.OperationalInsights/workspaces/savedSearches",
            "apiVersion": "[variables('LogAnalyticsApiVersion-Search')]",
            "dependsOn": [ ],
            "tags": { },
            "properties": {
                "etag": "*",
                "query": "[variables('MySearch').query]",
                "displayName": "[variables('MySearch').displayName]",
                "category": "[variables('MySearch').category]"
            }
        },
        {
            "name": "[concat(parameters('workspaceName'), '/', variables('MySearch').Name, '/', variables('MyAlert').Schedule.Name)]",
            "type": "Microsoft.OperationalInsights/workspaces/savedSearches/schedules/",
            "apiVersion": "[variables('LogAnalyticsApiVersion-Search')]",
            "dependsOn": [
                "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'), '/savedSearches/', variables('MySearch').Name)]"
            ],
            "properties": {
                "etag": "*",
                "interval": "[variables('MyAlert').Schedule.Interval]",
                "queryTimeSpan": "[variables('MyAlert').Schedule.TimeSpan]",
                "enabled": true
            }
        },
        {
            "name": "[concat(parameters('workspaceName'), '/', variables('MySearch').Name, '/', variables('MyAlert').Schedule.Name, '/', variables('MyAlert').Name)]",
            "type": "Microsoft.OperationalInsights/workspaces/savedSearches/schedules/actions",
            "apiVersion": "[variables('LogAnalyticsApiVersion-Search')]",
            "dependsOn": [
                "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'), '/savedSearches/', variables('MySearch').Name, '/schedules/', variables('MyAlert').Schedule.Name)]"
            ],
            "properties": {
                "etag": "*",
                "Type": "Alert",
                "Name": "[variables('MyAlert').DisplayName]",
                "Description": "[variables('MyAlert').Description]",
                "Severity": "[variables('MyAlert').Severity]",
                "Threshold": {
                    "Operator": "[variables('MyAlert').ThresholdOperator]",
                    "Value": "[variables('MyAlert').ThresholdValue]",
                    "MetricsTrigger": {
                        "TriggerCondition": "[variables('MyAlert').MetricsTrigger.TriggerCondition]",
                        "Operator": "[variables('MyAlert').MetricsTrigger.Operator]",
                        "Value": "[variables('MyAlert').MetricsTrigger.Value]"
                    }
                },
                "Throttling": {
                    "DurationInMinutes": "[variables('MyAlert').ThrottleMinutes]"
                },
                "AzNsNotification": {
                    "GroupIds": "[variables('MyAlert').AzNsNotification.GroupIds]",
                    "CustomEmailSubject": "[variables('MyAlert').AzNsNotification.CustomEmailSubject]"
                }
            }
        }
    ]
}
```

Следующий файл параметров содержит примеры значений для такого решения.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "workspacename": {
            "value": "myWorkspace"
        },
        "accountName": {
            "value": "myAccount"
        },
        "workspaceregionId": {
            "value": "East US"
        },
        "regionId": {
            "value": "East US 2"
        },
        "pricingTier": {
            "value": "Free"
        },
        "actiongroup": {
            "value": "/subscriptions/3b540246-808d-4331-99aa-917b808a9166/resourcegroups/myTestGroup/providers/microsoft.insights/actiongroups/sample"
        }
    }
}
```

## <a name="next-steps"></a>Дальнейшие действия
* [Добавьте представления](solutions-resources-views.md) в решение для управления.
* [Добавьте модули Runbook и другие ресурсы службы автоматизации](solutions-resources-automation.md) в решение для управления.
