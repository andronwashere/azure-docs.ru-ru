---
title: Схема событий журнала действий Azure
description: Общие сведения о схеме событий для данных, генерируемых в журнал действий
author: johnkemnetz
services: azure-monitor
ms.service: azure-monitor
ms.topic: reference
ms.date: 1/16/2019
ms.author: dukek
ms.subservice: logs
ms.openlocfilehash: ba5e0f696f54f46fb14086b542dc3b2e64155975
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "66244937"
---
# <a name="azure-activity-log-event-schema"></a>Схема событий журнала действий Azure
**Журнал действий Azure** — это журнал с подробными сведениями о событиях на уровне подписки, которые произошли в Azure. В этой статье описывается схема событий по категориям данных. Схема данных зависит от того, считываются ли данные через портал, PowerShell, CLI или напрямую через REST API и [передачу потока данных в хранилище или Центры событий с помощью профиля журнала](activity-log-export.md). В примерах ниже приведена схема, доступная на портале, в PowerShell, CLI и REST API. Сопоставление этих свойств со [схемой журналов системы диагностики Azure](diagnostic-logs-schema.md) приведено в конце статьи.

## <a name="administrative"></a>Administrative
Эта категория содержит записи всех операций создания, обновления, удаления и других действий, которые выполняются через Resource Manager. В качестве примеров типов событий, которые относятся к этой категории, можно назвать "создание виртуальной машины" или "удаление группы безопасности сети". Каждое действие, выполняемое пользователем или приложением с помощью Resource Manager, моделируется как операция с определенным типом ресурсов. Если тип операции — "запись", "удаление" или "действие", то любые сведения об этой операции (о ее запуске, успешном выполнении или сбое) записываются в категорию "Административная". Категория "Административная" также включает в себя любые изменения в управлении доступом на основе ролей, которые происходят в подписке.

### <a name="sample-event"></a>Пример события
```json
{
    "authorization": {
        "action": "Microsoft.Network/networkSecurityGroups/write",
        "scope": "/subscriptions/<subscription ID>/resourcegroups/myResourceGroup/providers/Microsoft.Network/networkSecurityGroups/myNSG"
    },
    "caller": "rob@contoso.com",
    "channels": "Operation",
    "claims": {
        "aud": "https://management.core.windows.net/",
        "iss": "https://sts.windows.net/1114444b-7467-4144-a616-e3a5d63e147b/",
        "iat": "1234567890",
        "nbf": "1234567890",
        "exp": "1234567890",
        "_claim_names": "{\"groups\":\"src1\"}",
        "_claim_sources": "{\"src1\":{\"endpoint\":\"https://graph.windows.net/1114444b-7467-4144-a616-e3a5d63e147b/users/f409edeb-4d29-44b5-9763-ee9348ad91bb/getMemberObjects\"}}",
        "http://schemas.microsoft.com/claims/authnclassreference": "1",
        "aio": "A3GgTJdwK4vy7Fa7l6DgJC2mI0GX44tML385OpU1Q+z+jaPnFMwB",
        "http://schemas.microsoft.com/claims/authnmethodsreferences": "rsa,mfa",
        "appid": "355249ed-15d9-460d-8481-84026b065942",
        "appidacr": "2",
        "http://schemas.microsoft.com/2012/01/devicecontext/claims/identifier": "10845a4d-ffa4-4b61-a3b4-e57b9b31cdb5",
        "e_exp": "262800",
        "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname": "Robertson",
        "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname": "Rob",
        "ipaddr": "111.111.1.111",
        "name": "Rob Robertson",
        "http://schemas.microsoft.com/identity/claims/objectidentifier": "f409edeb-4d29-44b5-9763-ee9348ad91bb",
        "onprem_sid": "S-1-5-21-4837261184-168309720-1886587427-18514304",
        "puid": "18247BBD84827C6D",
        "http://schemas.microsoft.com/identity/claims/scope": "user_impersonation",
        "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier": "b-24Jf94A3FH2sHWVIFqO3-RSJEiv24Jnif3gj7s",
        "http://schemas.microsoft.com/identity/claims/tenantid": "1114444b-7467-4144-a616-e3a5d63e147b",
        "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name": "rob@contoso.com",
        "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn": "rob@contoso.com",
        "uti": "IdP3SUJGtkGlt7dDQVRPAA",
        "ver": "1.0"
    },
    "correlationId": "b5768deb-836b-41cc-803e-3f4de2f9e40b",
    "eventDataId": "d0d36f97-b29c-4cd9-9d3d-ea2b92af3e9d",
    "eventName": {
        "value": "EndRequest",
        "localizedValue": "End request"
    },
    "category": {
        "value": "Administrative",
        "localizedValue": "Administrative"
    },
    "eventTimestamp": "2018-01-29T20:42:31.3810679Z",
    "id": "/subscriptions/<subscription ID>/resourcegroups/myResourceGroup/providers/Microsoft.Network/networkSecurityGroups/myNSG/events/d0d36f97-b29c-4cd9-9d3d-ea2b92af3e9d/ticks/636528553513810679",
    "level": "Informational",
    "operationId": "04e575f8-48d0-4c43-a8b3-78c4eb01d287",
    "operationName": {
        "value": "Microsoft.Network/networkSecurityGroups/write",
        "localizedValue": "Microsoft.Network/networkSecurityGroups/write"
    },
    "resourceGroupName": "myResourceGroup",
    "resourceProviderName": {
        "value": "Microsoft.Network",
        "localizedValue": "Microsoft.Network"
    },
    "resourceType": {
        "value": "Microsoft.Network/networkSecurityGroups",
        "localizedValue": "Microsoft.Network/networkSecurityGroups"
    },
    "resourceId": "/subscriptions/<subscription ID>/resourcegroups/myResourceGroup/providers/Microsoft.Network/networkSecurityGroups/myNSG",
    "status": {
        "value": "Succeeded",
        "localizedValue": "Succeeded"
    },
    "subStatus": {
        "value": "",
        "localizedValue": ""
    },
    "submissionTimestamp": "2018-01-29T20:42:50.0724829Z",
    "subscriptionId": "<subscription ID>",
    "properties": {
        "statusCode": "Created",
        "serviceRequestId": "a4c11dbd-697e-47c5-9663-12362307157d",
        "responseBody": "",
        "requestbody": ""
    },
    "relatedEvents": []
}

```

### <a name="property-descriptions"></a>Описания свойств
| Имя элемента | Описание |
| --- | --- |
| authorization |BLOB-объект со свойствами RBAC события. Обычно включает следующие свойства: action, role и scope. |
| caller |Адрес электронной почты пользователя, который выполнил операцию, утверждение имени субъекта-службы или имени участника-пользователя в зависимости от доступности. |
| каналов |Одно из следующих значений: Admin, Operation |
| claims |Токен JWT, который используется Active Directory для аутентификации пользователя или приложения при выполнении этой операции в Resource Manager. |
| correlationId |Обычно GUID в строковом формате. События, которые совместно используют идентификатор correlationId, принадлежат к одному общему действию. |
| Description (Описание) |Статическое описание события в текстовом виде. |
| eventDataId |Уникальный идентификатор события. |
| eventName | Понятное имя административные события. |
| category | Всегда «административная» |
| httpRequest |Большой двоичный объект, описывающий HTTP-запрос. Обычно включает clientRequestId, clientIpAddress и method (метод HTTP, например PUT). |
| level |Уровень события. Одно из следующих значений: Critical, Error, Warning или Informational |
| resourceGroupName |Имя группы ресурсов для затронутого ресурса. |
| resourceProviderName |Имя поставщика ресурса для затронутого ресурса. |
| тип_ресурса | Тип ресурса, на которое повлияла административного события. |
| resourceId |Идентификатор ресурса для затронутого ресурса. |
| operationId |События, относящиеся к одной операции, совместно используют один GUID. |
| operationName |Имя операции. |
| properties |Набор пар `<Key, Value>` (например, Dictionary) c подробным описанием события. |
| status |Строка, описывающая состояние операции. Некоторые обычные значения: Started (Запущен), In Progress (Выполняется), Succeeded (Успешно), Failed (Сбой), Active (Активно), Resolved (Разрешено). |
| subStatus |Обычно это код состояния HTTP соответствующего вызова REST, но может также включать другие строки, описывающие подсостояние, например: OK (код состояния HTTP: 200), Created (Создано) (код состояния HTTP: 201), Accepted (Принято) (код состояния HTTP: 202), No Content (Нет содержимого) (код состояния HTTP: 204), Bad Request (Недопустимый запрос) (код состояния HTTP: 400), Not Found (Не найдено) (код состояния HTTP: 404), Conflict (Конфликт) (код состояния HTTP: 409), Internal Server Error (Внутренняя ошибка сервера) (код состояния HTTP: 500), Service Unavailable (Служба недоступна) (код состояния HTTP: 503), Gateway Timeout (Истекло время ожидания шлюза) (код состояния HTTP: 504). |
| eventTimestamp |Метка времени, когда служба Azure создала событие при обработке соответствующего этому событию запроса. |
| submissionTimestamp |Метка времени, когда событие стало доступно для запросов. |
| subscriptionId |Идентификатор подписки Azure. |

## <a name="service-health"></a>Работоспособность службы
Эта категория содержит записи всех инцидентов, связанных с работоспособностью службы, которые произошли в Azure. В качестве примера типа события из этой категории можно назвать следующее: "В работе SQL Azure в восточной части США наблюдаются простои". Существует шесть разновидностей событий работоспособности служб: Action Required (Требуется действие), Assisted Recovery (Помощь при восстановлении), Incident (Инцидент), Maintenance (Обслуживание), Information (Информация) и Security (Безопасность). Они отображаются, только если в подписке есть ресурс, на который повлияет данное событие.

### <a name="sample-event"></a>Пример события
```json
{
  "channels": "Admin",
  "correlationId": "c550176b-8f52-4380-bdc5-36c1b59d3a44",
  "description": "Active: Network Infrastructure - UK South",
  "eventDataId": "c5bc4514-6642-2be3-453e-c6a67841b073",
  "eventName": {
      "value": null
  },
  "category": {
      "value": "ServiceHealth",
      "localizedValue": "Service Health"
  },
  "eventTimestamp": "2017-07-20T23:30:14.8022297Z",
  "id": "/subscriptions/<subscription ID>/events/c5bc4514-6642-2be3-453e-c6a67841b073/ticks/636361902148022297",
  "level": "Warning",
  "operationName": {
      "value": "Microsoft.ServiceHealth/incident/action",
      "localizedValue": "Microsoft.ServiceHealth/incident/action"
  },
  "resourceProviderName": {
      "value": null
  },
  "resourceType": {
      "value": null,
      "localizedValue": ""
  },
  "resourceId": "/subscriptions/<subscription ID>",
  "status": {
      "value": "Active",
      "localizedValue": "Active"
  },
  "subStatus": {
      "value": null
  },
  "submissionTimestamp": "2017-07-20T23:30:34.7431946Z",
  "subscriptionId": "<subscription ID>",
  "properties": {
    "title": "Network Infrastructure - UK South",
    "service": "Service Fabric",
    "region": "UK South",
    "communication": "Starting at approximately 21:41 UTC on 20 Jul 2017, a subset of customers in UK South may experience degraded performance, connectivity drops or timeouts when accessing their Azure resources hosted in this region. Engineers are investigating underlying Network Infrastructure issues in this region. Impacted services may include, but are not limited to App Services, Automation, Service Bus, Log Analytics, Key Vault, SQL Database, Service Fabric, Event Hubs, Stream Analytics, Azure Data Movement, API Management, and Azure Search. Multiple engineering teams are engaged in multiple workflows to mitigate the impact. The next update will be provided in 60 minutes, or as events warrant.",
    "incidentType": "Incident",
    "trackingId": "NA0F-BJG",
    "impactStartTime": "2017-07-20T21:41:00.0000000Z",
    "impactedServices": "[{\"ImpactedRegions\":[{\"RegionName\":\"UK South\"}],\"ServiceName\":\"Service Fabric\"}]",
    "defaultLanguageTitle": "Network Infrastructure - UK South",
    "defaultLanguageContent": "Starting at approximately 21:41 UTC on 20 Jul 2017, a subset of customers in UK South may experience degraded performance, connectivity drops or timeouts when accessing their Azure resources hosted in this region. Engineers are investigating underlying Network Infrastructure issues in this region. Impacted services may include, but are not limited to App Services, Automation, Service Bus, Log Analytics, Key Vault, SQL Database, Service Fabric, Event Hubs, Stream Analytics, Azure Data Movement, API Management, and Azure Search. Multiple engineering teams are engaged in multiple workflows to mitigate the impact. The next update will be provided in 60 minutes, or as events warrant.",
    "stage": "Active",
    "communicationId": "636361902146035247",
    "version": "0.1.1"
  }
}
```
Дополнительные сведения о значениях в свойствах см. в статье об [уведомлениях о работоспособности службы](./../../azure-monitor/platform/service-notifications.md).

## <a name="resource-health"></a>Работоспособность ресурса
Эта категория содержит записи всех событий, связанных с работоспособностью службы, которые произошли с ресурсами Azure. Пример события такого типа из этой категории: Virtual Machine health status changed to unavailable (Состояние работоспособности виртуальной машины изменилось на "Недоступно"). Существует четыре вида событий работоспособности ресурсов: Available (Доступно), Unavailable (Недоступно), Degraded (Понижено) и Unknown (Неизвестно). Кроме того, события работоспособности ресурсов можно классифицировать по свойствам PlatformInitiated (Инициировано платформой) и UserInitiated (Инициировано пользователем).

### <a name="sample-event"></a>Пример события

```json
{
    "channels": "Admin, Operation",
    "correlationId": "28f1bfae-56d3-7urb-bff4-194d261248e9",
    "description": "",
    "eventDataId": "a80024e1-883d-37ur-8b01-7591a1befccb",
    "eventName": {
        "value": "",
        "localizedValue": ""
    },
    "category": {
        "value": "ResourceHealth",
        "localizedValue": "Resource Health"
    },
    "eventTimestamp": "2018-09-04T15:33:43.65Z",
    "id": "/subscriptions/<subscription ID>/resourceGroups/<resource group>/providers/Microsoft.Compute/virtualMachines/<resource name>/events/a80024e1-883d-42a5-8b01-7591a1befccb/ticks/636716720236500000",
    "level": "Critical",
    "operationId": "",
    "operationName": {
        "value": "Microsoft.Resourcehealth/healthevent/Activated/action",
        "localizedValue": "Health Event Activated"
    },
    "resourceGroupName": "<resource group>",
    "resourceProviderName": {
        "value": "Microsoft.Resourcehealth/healthevent/action",
        "localizedValue": "Microsoft.Resourcehealth/healthevent/action"
    },
    "resourceType": {
        "value": "Microsoft.Compute/virtualMachines",
        "localizedValue": "Microsoft.Compute/virtualMachines"
    },
    "resourceId": "/subscriptions/<subscription ID>/resourceGroups/<resource group>/providers/Microsoft.Compute/virtualMachines/<resource name>",
    "status": {
        "value": "Active",
        "localizedValue": "Active"
    },
    "subStatus": {
        "value": "",
        "localizedValue": ""
    },
    "submissionTimestamp": "2018-09-04T15:36:24.2240867Z",
    "subscriptionId": "<subscription ID>",
    "properties": {
        "stage": "Active",
        "title": "Virtual Machine health status changed to unavailable",
        "details": "Virtual machine has experienced an unexpected event",
        "healthStatus": "Unavailable",
        "healthEventType": "Downtime",
        "healthEventCause": "PlatformInitiated",
        "healthEventCategory": "Unplanned"
    },
    "relatedEvents": []
}
```

### <a name="property-descriptions"></a>Описания свойств
| Имя элемента | Описание |
| --- | --- |
| каналов | Всегда имеет значение "Admin, Operation". |
| correlationId | Глобальный уникальный идентификатор (GUID) в строковом формате. |
| description |Статическое текстовое описание события оповещения. |
| eventDataId |Уникальный идентификатор события оповещения. |
| category | Всегда ResourceHealth. |
| eventTimestamp |Метка времени, когда служба Azure создала событие при обработке соответствующего этому событию запроса. |
| level |Уровень события. Одно из следующих значений: Critical (Критическое), Error (Ошибка), Warning (Предупреждение), Informational (Информационное) или Verbose (Подробные сведения) |
| operationId |События, относящиеся к одной операции, совместно используют один GUID. |
| operationName |Имя операции. |
| resourceGroupName |Имя группы ресурсов, к которой относится ресурс. |
| resourceProviderName |Всегда Microsoft.Resourcehealth/healthevent/action. |
| тип_ресурса | Тип ресурса, на который повлияло событие работоспособности ресурса. |
| resourceId | Имя идентификатора затронутого ресурса. |
| status |Строка, описывающая состояние события работоспособности. Значения могут быть такими: Active (Активно), Resolved (Разрешено), InProgress (Выполняется), Updated (Обновлено). |
| subStatus | Обычно для оповещений имеет значение null. |
| submissionTimestamp |Метка времени, когда событие стало доступно для запросов. |
| subscriptionId |Идентификатор подписки Azure. |
| properties |Набор пар `<Key, Value>` (например, Dictionary) c подробным описанием события.|
| properties.title | Понятная для пользователя строка с описанием состояния работоспособности ресурса. |
| properties.details | Понятная для пользователя строка с описанием подробностей события. |
| properties.currentHealthStatus | Текущее состояние работоспособности ресурса. Одно из следующих значений: Available (Доступно), Unavailable (Недоступно), Degraded (Понижено) и Unknown (Неизвестно). |
| properties.previousHealthStatus | Предыдущее состояние работоспособности ресурса. Одно из следующих значений: Available (Доступно), Unavailable (Недоступно), Degraded (Понижено) и Unknown (Неизвестно). |
| properties.type | Описание типа события работоспособности ресурса. |
| properties.cause | Описание причины события работоспособности ресурса: UserInitiated либо PlatformInitiated. |


## <a name="alert"></a>Предупреждение
Эта категория содержит записи всех активаций оповещений Azure. В качестве примера типа события из этой категории можно назвать следующее: "Процент использования ЦП на виртуальной машине myVM за последние 5 минут превышал 80 %". Различные системы Azure работают по принципу оповещений, то есть вы можете определить какое-то правило, и система будет направлять вам уведомление, когда условия этого правила выполняются. Каждый раз, когда "активируется" поддерживаемый тип оповещения Azure или выполняются условия для создания уведомления, в эту категорию журнала активности также передается запись об активации.

### <a name="sample-event"></a>Пример события

```json
{
  "caller": "Microsoft.Insights/alertRules",
  "channels": "Admin, Operation",
  "claims": {
    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/spn": "Microsoft.Insights/alertRules"
  },
  "correlationId": "/subscriptions/<subscription ID>/resourceGroups/myResourceGroup/providers/microsoft.insights/alertrules/myalert/incidents/L3N1YnNjcmlwdGlvbnMvZGY2MDJjOWMtN2FhMC00MDdkLWE2ZmItZWIyMGM4YmQxMTkyL3Jlc291cmNlR3JvdXBzL0NzbUV2ZW50RE9HRk9PRC1XZXN0VVMvcHJvdmlkZXJzL21pY3Jvc29mdC5pbnNpZ2h0cy9hbGVydHJ1bGVzL215YWxlcnQwNjM2MzYyMjU4NTM1MjIxOTIw",
  "description": "'Disk read LessThan 100000 ([Count]) in the last 5 minutes' has been resolved for CloudService: myResourceGroup/Production/Event.BackgroundJobsWorker.razzle (myResourceGroup)",
  "eventDataId": "149d4baf-53dc-4cf4-9e29-17de37405cd9",
  "eventName": {
    "value": "Alert",
    "localizedValue": "Alert"
  },
  "category": {
    "value": "Alert",
    "localizedValue": "Alert"
  },
  "id": "/subscriptions/<subscription ID>/resourceGroups/myResourceGroup/providers/Microsoft.ClassicCompute/domainNames/myResourceGroup/slots/Production/roles/Event.BackgroundJobsWorker.razzle/events/149d4baf-53dc-4cf4-9e29-17de37405cd9/ticks/636362258535221920",
  "level": "Informational",
  "resourceGroupName": "myResourceGroup",
  "resourceProviderName": {
    "value": "Microsoft.ClassicCompute",
    "localizedValue": "Microsoft.ClassicCompute"
  },
  "resourceId": "/subscriptions/<subscription ID>/resourceGroups/myResourceGroup/providers/Microsoft.ClassicCompute/domainNames/myResourceGroup/slots/Production/roles/Event.BackgroundJobsWorker.razzle",
  "resourceType": {
    "value": "Microsoft.ClassicCompute/domainNames/slots/roles",
    "localizedValue": "Microsoft.ClassicCompute/domainNames/slots/roles"
  },
  "operationId": "/subscriptions/<subscription ID>/resourceGroups/myResourceGroup/providers/microsoft.insights/alertrules/myalert/incidents/L3N1YnNjcmlwdGlvbnMvZGY2MDJjOWMtN2FhMC00MDdkLWE2ZmItZWIyMGM4YmQxMTkyL3Jlc291cmNlR3JvdXBzL0NzbUV2ZW50RE9HRk9PRC1XZXN0VVMvcHJvdmlkZXJzL21pY3Jvc29mdC5pbnNpZ2h0cy9hbGVydHJ1bGVzL215YWxlcnQwNjM2MzYyMjU4NTM1MjIxOTIw",
  "operationName": {
    "value": "Microsoft.Insights/AlertRules/Resolved/Action",
    "localizedValue": "Microsoft.Insights/AlertRules/Resolved/Action"
  },
  "properties": {
    "RuleUri": "/subscriptions/<subscription ID>/resourceGroups/myResourceGroup/providers/microsoft.insights/alertrules/myalert",
    "RuleName": "myalert",
    "RuleDescription": "",
    "Threshold": "100000",
    "WindowSizeInMinutes": "5",
    "Aggregation": "Average",
    "Operator": "LessThan",
    "MetricName": "Disk read",
    "MetricUnit": "Count"
  },
  "status": {
    "value": "Resolved",
    "localizedValue": "Resolved"
  },
  "subStatus": {
    "value": null
  },
  "eventTimestamp": "2017-07-21T09:24:13.522192Z",
  "submissionTimestamp": "2017-07-21T09:24:15.6578651Z",
  "subscriptionId": "<subscription ID>"
}
```

### <a name="property-descriptions"></a>Описания свойств
| Имя элемента | Описание |
| --- | --- |
| caller | Всегда имеет значение Microsoft.Insights/alertRules. |
| каналов | Всегда имеет значение "Admin, Operation". |
| claims | Большой двоичный объект JSON с именем субъекта-службы (SPN) или типом ресурса обработчика предупреждений. |
| correlationId | Глобальный уникальный идентификатор (GUID) в строковом формате. |
| description |Статическое текстовое описание события оповещения. |
| eventDataId |Уникальный идентификатор события оповещения. |
| category | Всегда «предупреждения» |
| level |Уровень события. Одно из следующих значений: Critical, Error, Warning или Informational |
| resourceGroupName |Имя группы ресурсов для затронутого ресурса, если это оповещение метрики. Для других типов оповещений это имя группы ресурсов, содержащей само оповещение. |
| resourceProviderName |Имя поставщика ресурсов для затронутого ресурса, если это оповещение метрики. Для других типов оповещений это имя поставщика ресурсов для самого оповещения. |
| resourceId | Имя идентификатора ресурса для затронутого ресурса, если это оповещение метрики. Для других типов оповещений это идентификатор ресурса самого ресурса оповещения. |
| operationId |События, относящиеся к одной операции, совместно используют один GUID. |
| operationName |Имя операции. |
| properties |Набор пар `<Key, Value>` (например, Dictionary) c подробным описанием события. |
| status |Строка, описывающая состояние операции. Некоторые обычные значения: Started (Запущен), In Progress (Выполняется), Succeeded (Успешно), Failed (Сбой), Active (Активно), Resolved (Разрешено). |
| subStatus | Обычно для оповещений имеет значение null. |
| eventTimestamp |Метка времени, когда служба Azure создала событие при обработке соответствующего этому событию запроса. |
| submissionTimestamp |Метка времени, когда событие стало доступно для запросов. |
| subscriptionId |Идентификатор подписки Azure. |

### <a name="properties-field-per-alert-type"></a>Поле свойств по типу оповещения
Поле свойств будет содержать разные значения в зависимости от источника события оповещения. Два распространенных поставщика событий оповещений — оповещения журнала действий и оповещения метрик.

#### <a name="properties-for-activity-log-alerts"></a>Свойства оповещений журнала действий
| Имя элемента | Описание |
| --- | --- |
| properties.subscriptionId | Идентификатор подписки из события журнала действий, которое вызвало активацию данного правила оповещения журнала действий. |
| properties.eventDataId | Идентификатор данных события из события журнала действий, которое вызвало активацию данного правила оповещения журнала действий. |
| properties.resourceGroup | Идентификатор группы ресурсов из события журнала действий, которое вызвало активацию данного правила оповещения журнала действий. |
| properties.resourceId | Идентификатор ресурса из события журнала действий, которое вызвало активацию данного правила оповещения журнала действий. |
| properties.eventTimestamp | Метка времени события из события журнала действий, которое вызвало активацию данного правила оповещения журнала действий. |
| properties.operationName | Имя операции из события журнала действий, которое вызвало активацию данного правила оповещения журнала действий. |
| properties.status | Состояние из события журнала действий, которое вызвало активацию данного правила оповещения журнала действий.|

#### <a name="properties-for-metric-alerts"></a>Свойства оповещений метрик
| Имя элемента | Описание |
| --- | --- |
| properties.RuleUri | Идентификатор ресурса самого правила оповещения метрики. |
| properties.RuleName | Имя правила оповещения метрики. |
| properties.RuleDescription | Описание правила оповещения метрики (как указано в правиле оповещения). |
| properties.Threshold | Пороговое значение, используемое для оценки правила оповещения метрики. |
| properties.WindowSizeInMinutes | Размер окна, используемый для оценки правила оповещения метрики. |
| properties.Aggregation | Тип агрегата, определенный в правиле оповещения метрики. |
| properties.Operator | Условный оператор, используемый для оценки правила оповещения метрики. |
| properties.MetricName | Имя метрики, используемой для оценки правила оповещения метрики. |
| properties.MetricUnit | Единица измерения метрики, используемая для оценки правила оповещения метрики. |

## <a name="autoscale"></a>Автомасштабирование
Эта категория содержит записи всех событий, связанных с операциями системы автоматического масштабирования, в основе которой лежат параметры автомасштабирования, определенные в подписке. В качестве примера типа события из этой категории можно назвать следующее: "Не удалось выполнить автоматическое увеличение масштаба". С помощью функции автомасштабирования можно автоматически увеличивать или уменьшать число экземпляров в поддерживаемом типе ресурсов на основе времени суток и (или) загружать данные (метрики), используя параметр автомасштабирования. Когда выполняются условия для увеличения или уменьшения масштаба, в эту категорию записываются соответствующие события (запуск, успешное выполнение или сбой).

### <a name="sample-event"></a>Пример события
```json
{
  "caller": "Microsoft.Insights/autoscaleSettings",
  "channels": "Admin, Operation",
  "claims": {
    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/spn": "Microsoft.Insights/autoscaleSettings"
  },
  "correlationId": "fc6a7ff5-ff68-4bb7-81b4-3629212d03d0",
  "description": "The autoscale engine attempting to scale resource '/subscriptions/<subscription ID>/resourceGroups/myResourceGroup/providers/Microsoft.ClassicCompute/domainNames/myResourceGroup/slots/Production/roles/myResource' from 3 instances count to 2 instances count.",
  "eventDataId": "a5b92075-1de9-42f1-b52e-6f3e4945a7c7",
  "eventName": {
    "value": "AutoscaleAction",
    "localizedValue": "AutoscaleAction"
  },
  "category": {
    "value": "Autoscale",
    "localizedValue": "Autoscale"
  },
  "id": "/subscriptions/<subscription ID>/resourceGroups/myResourceGroup/providers/microsoft.insights/autoscalesettings/myResourceGroup-Production-myResource-myResourceGroup/events/a5b92075-1de9-42f1-b52e-6f3e4945a7c7/ticks/636361956518681572",
  "level": "Informational",
  "resourceGroupName": "myResourceGroup",
  "resourceProviderName": {
    "value": "microsoft.insights",
    "localizedValue": "microsoft.insights"
  },
  "resourceId": "/subscriptions/<subscription ID>/resourceGroups/myResourceGroup/providers/microsoft.insights/autoscalesettings/myResourceGroup-Production-myResource-myResourceGroup",
  "resourceType": {
    "value": "microsoft.insights/autoscalesettings",
    "localizedValue": "microsoft.insights/autoscalesettings"
  },
  "operationId": "fc6a7ff5-ff68-4bb7-81b4-3629212d03d0",
  "operationName": {
    "value": "Microsoft.Insights/AutoscaleSettings/Scaledown/Action",
    "localizedValue": "Microsoft.Insights/AutoscaleSettings/Scaledown/Action"
  },
  "properties": {
    "Description": "The autoscale engine attempting to scale resource '/subscriptions/<subscription ID>/resourceGroups/myResourceGroup/providers/Microsoft.ClassicCompute/domainNames/myResourceGroup/slots/Production/roles/myResource' from 3 instances count to 2 instances count.",
    "ResourceName": "/subscriptions/<subscription ID>/resourceGroups/myResourceGroup/providers/Microsoft.ClassicCompute/domainNames/myResourceGroup/slots/Production/roles/myResource",
    "OldInstancesCount": "3",
    "NewInstancesCount": "2",
    "LastScaleActionTime": "Fri, 21 Jul 2017 01:00:51 GMT"
  },
  "status": {
    "value": "Succeeded",
    "localizedValue": "Succeeded"
  },
  "subStatus": {
    "value": null
  },
  "eventTimestamp": "2017-07-21T01:00:51.8681572Z",
  "submissionTimestamp": "2017-07-21T01:00:52.3008754Z",
  "subscriptionId": "<subscription ID>"
}

```

### <a name="property-descriptions"></a>Описания свойств
| Имя элемента | Описание |
| --- | --- |
| caller | Всегда имеет значение Microsoft.Insights/autoscaleSettings. |
| каналов | Всегда имеет значение "Admin, Operation". |
| claims | Большой двоичный объект JSON с именем субъекта-службы (SPN) или типом ресурса системы автомасштабирования. |
| correlationId | Глобальный уникальный идентификатор (GUID) в строковом формате. |
| description |Статическое текстовое описание события автомасштабирования. |
| eventDataId |Уникальный идентификатор события автомасштабирования. |
| level |Уровень события. Одно из следующих значений: Critical, Error, Warning или Informational |
| resourceGroupName |Имя группы ресурсов для параметра автомасштабирования. |
| resourceProviderName |Имя поставщика ресурсов для параметра автомасштабирования. |
| resourceId |Идентификатор ресурса параметра автомасштабирования. |
| operationId |События, относящиеся к одной операции, совместно используют один GUID. |
| operationName |Имя операции. |
| properties |Набор пар `<Key, Value>` (например, Dictionary) c подробным описанием события. |
| properties.Description | Подробное описание действий, выполненных системой автоматического масштабирования. |
| properties.ResourceName | Идентификатор ресурса для затронутого ресурса (ресурса, в отношении которого выполнялось действие масштабирования). |
| properties.OldInstancesCount | Число экземпляров до выполнения действия автомасштабирования. |
| properties.NewInstancesCount | Число экземпляров после выполнения действия автомасштабирования. |
| properties.LastScaleActionTime | Метка времени, когда произошло действие автоматического масштабирования. |
| status |Строка, описывающая состояние операции. Некоторые обычные значения: Started (Запущен), In Progress (Выполняется), Succeeded (Успешно), Failed (Сбой), Active (Активно), Resolved (Разрешено). |
| subStatus | Обычно для автоматического масштабирования имеет значение null. |
| eventTimestamp |Метка времени, когда служба Azure создала событие при обработке соответствующего этому событию запроса. |
| submissionTimestamp |Метка времени, когда событие стало доступно для запросов. |
| subscriptionId |Идентификатор подписки Azure. |

## <a name="security"></a>Безопасность
Эта категория содержит запись любых предупреждений, созданных центром безопасности Azure. Примером типа события в этой категории является "Выполнен подозрительный файл с двойным расширением".

### <a name="sample-event"></a>Пример события
```json
{
    "channels": "Operation",
    "correlationId": "965d6c6a-a790-4a7e-8e9a-41771b3fbc38",
    "description": "Suspicious double extension file executed. Machine logs indicate an execution of a process with a suspicious double extension.\r\nThis extension may trick users into thinking files are safe to be opened and might indicate the presence of malware on the system.",
    "eventDataId": "965d6c6a-a790-4a7e-8e9a-41771b3fbc38",
    "eventName": {
        "value": "Suspicious double extension file executed",
        "localizedValue": "Suspicious double extension file executed"
    },
    "category": {
        "value": "Security",
        "localizedValue": "Security"
    },
    "eventTimestamp": "2017-10-18T06:02:18.6179339Z",
    "id": "/subscriptions/<subscription ID>/providers/Microsoft.Security/locations/centralus/alerts/965d6c6a-a790-4a7e-8e9a-41771b3fbc38/events/965d6c6a-a790-4a7e-8e9a-41771b3fbc38/ticks/636439033386179339",
    "level": "Informational",
    "operationId": "965d6c6a-a790-4a7e-8e9a-41771b3fbc38",
    "operationName": {
        "value": "Microsoft.Security/locations/alerts/activate/action",
        "localizedValue": "Microsoft.Security/locations/alerts/activate/action"
    },
    "resourceGroupName": "myResourceGroup",
    "resourceProviderName": {
        "value": "Microsoft.Security",
        "localizedValue": "Microsoft.Security"
    },
    "resourceType": {
        "value": "Microsoft.Security/locations/alerts",
        "localizedValue": "Microsoft.Security/locations/alerts"
    },
    "resourceId": "/subscriptions/<subscription ID>/providers/Microsoft.Security/locations/centralus/alerts/2518939942613820660_a48f8653-3fc6-4166-9f19-914f030a13d3",
    "status": {
        "value": "Active",
        "localizedValue": "Active"
    },
    "subStatus": {
        "value": null
    },
    "submissionTimestamp": "2017-10-18T06:02:52.2176969Z",
    "subscriptionId": "<subscription ID>",
    "properties": {
        "accountLogonId": "0x2r4",
        "commandLine": "c:\\mydirectory\\doubleetension.pdf.exe",
        "domainName": "hpc",
        "parentProcess": "unknown",
        "parentProcess id": "0",
        "processId": "6988",
        "processName": "c:\\mydirectory\\doubleetension.pdf.exe",
        "userName": "myUser",
        "UserSID": "S-3-2-12",
        "ActionTaken": "Detected",
        "Severity": "High"
    },
    "relatedEvents": []
}

```

### <a name="property-descriptions"></a>Описания свойств
| Имя элемента | Описание |
| --- | --- |
| каналов | Всегда значение Operation. |
| correlationId | Глобальный уникальный идентификатор (GUID) в строковом формате. |
| description |Статическое текстовое описание события безопасности. |
| eventDataId |Уникальный идентификатор события безопасности. |
| eventName |Понятное имя события безопасности. |
| category | Всегда «безопасность» |
| id |Уникальный идентификатор ресурса события безопасности. |
| level |Уровень события. Одно из следующих значений: Critical (Критический), Error (Ошибка), Warning (Предупреждение) или Informational (Информационное) |
| resourceGroupName |Имя группы ресурсов для ресурса. |
| resourceProviderName |Имя поставщика ресурсов для центра безопасности Azure. Всегда значение Microsoft.Security. |
| тип_ресурса |Тип ресурса, создавшего событие безопасности, например Microsoft.Security/locations/alerts. |
| resourceId |Идентификатор ресурса оповещения системы безопасности. |
| operationId |События, относящиеся к одной операции, совместно используют один GUID. |
| operationName |Имя операции. |
| properties |Набор пар `<Key, Value>` (например, Dictionary) c подробным описанием события. Эти свойства будут различаться в зависимости от типа оповещения системы безопасности. [Здесь](../../security-center/security-center-alerts-type.md) описаны типы предупреждений, поступающих из центра безопасности. |
| properties.Severity |Уровень серьезности. Возможные значения: High, Medium или Low. |
| status |Строка, описывающая состояние операции. Некоторые обычные значения: Started (Запущен), In Progress (Выполняется), Succeeded (Успешно), Failed (Сбой), Active (Активно), Resolved (Разрешено). |
| subStatus | Обычно имеет значение null для событий безопасности. |
| eventTimestamp |Метка времени, когда служба Azure создала событие при обработке соответствующего этому событию запроса. |
| submissionTimestamp |Метка времени, когда событие стало доступно для запросов. |
| subscriptionId |Идентификатор подписки Azure. |

## <a name="recommendation"></a>Рекомендации
Эта категория содержит запись о любых новых рекомендациях, которые создаются для служб. Примером рекомендации будет "использование группы доступности для повышения отказоустойчивости". Существует четыре типа событий уровня рекомендаций, которые могут быть сгенерированы: High Availability (Высокий уровень доступности), Performance (Производительность), Security (Безопасность) и Cost Optimization (Оптимизация затрат). 

### <a name="sample-event"></a>Пример события
```json
{
    "channels": "Operation",
    "correlationId": "92481dfd-c5bf-4752-b0d6-0ecddaa64776",
    "description": "The action was successful.",
    "eventDataId": "06cb0e44-111b-47c7-a4f2-aa3ee320c9c5",
    "eventName": {
        "value": "",
        "localizedValue": ""
    },
    "category": {
        "value": "Recommendation",
        "localizedValue": "Recommendation"
    },
    "eventTimestamp": "2018-06-07T21:30:42.976919Z",
    "id": "/SUBSCRIPTIONS/<Subscription ID>/RESOURCEGROUPS/MYRESOURCEGROUP/PROVIDERS/MICROSOFT.COMPUTE/VIRTUALMACHINES/MYVM/events/06cb0e44-111b-47c7-a4f2-aa3ee320c9c5/ticks/636640038429769190",
    "level": "Informational",
    "operationId": "",
    "operationName": {
        "value": "Microsoft.Advisor/generateRecommendations/action",
        "localizedValue": "Microsoft.Advisor/generateRecommendations/action"
    },
    "resourceGroupName": "MYRESOURCEGROUP",
    "resourceProviderName": {
        "value": "MICROSOFT.COMPUTE",
        "localizedValue": "MICROSOFT.COMPUTE"
    },
    "resourceType": {
        "value": "MICROSOFT.COMPUTE/virtualmachines",
        "localizedValue": "MICROSOFT.COMPUTE/virtualmachines"
    },
    "resourceId": "/SUBSCRIPTIONS/<Subscription ID>/RESOURCEGROUPS/MYRESOURCEGROUP/PROVIDERS/MICROSOFT.COMPUTE/VIRTUALMACHINES/MYVM",
    "status": {
        "value": "Active",
        "localizedValue": "Active"
    },
    "subStatus": {
        "value": "",
        "localizedValue": ""
    },
    "submissionTimestamp": "2018-06-07T21:30:42.976919Z",
    "subscriptionId": "<Subscription ID>",
    "properties": {
        "recommendationSchemaVersion": "1.0",
        "recommendationCategory": "Security",
        "recommendationImpact": "High",
        "recommendationRisk": "None"
    },
    "relatedEvents": []
}

```
### <a name="property-descriptions"></a>Описания свойств
| Имя элемента | Описание |
| --- | --- |
| каналов | Всегда значение Operation. |
| correlationId | Глобальный уникальный идентификатор (GUID) в строковом формате. |
| description |Статическое текстовое описание события рекомендации |
| eventDataId | Уникальный идентификатор события рекомендации. |
| category | Всегда "Рекомендация" |
| id |Уникальный идентификатор ресурса события рекомендации. |
| level |Уровень события. Одно из следующих значений: Critical (Критический), Error (Ошибка), Warning (Предупреждение) или Informational (Информационное) |
| operationName |Имя операции.  Всегда "Microsoft.Advisor/generateRecommendations/action"|
| resourceGroupName |Имя группы ресурсов для ресурса. |
| resourceProviderName |Имя поставщика ресурсов для ресурса, к которому применяется данная рекомендация, например "MICROSOFT.COMPUTE" |
| тип_ресурса |Имя типа ресурсов для ресурса, к которому применяется данная рекомендация, например "MICROSOFT.COMPUTE/virtualmachines" |
| resourceId |Идентификатор ресурса для ресурса, к которому применяется рекомендация |
| status | Всегда "Active" |
| submissionTimestamp |Метка времени, когда событие стало доступно для запросов. |
| subscriptionId |Идентификатор подписки Azure. |
| properties |Набор пар `<Key, Value>` (например, Dictionary) c подробным описанием рекомендации.|
| properties.recommendationSchemaVersion| Версия схемы свойств рекомендации, опубликована в записи журнала действий |
| properties.recommendationCategory | Категория рекомендации. Возможные значения: "Высокая доступность", "Производительность", "Безопасность" и "Стоимость" |
| properties.recommendationImpact| Влияние рекомендации Возможные значения: High, Medium или Low |
| properties.recommendationRisk| Риск рекомендации. Возможные значения: Error, Warning, None |

## <a name="policy"></a>Политика

Эта категория содержит записи всех операций действия эффекта, выполняемых [Политикой Azure](../../governance/policy/overview.md). Примеры типов событий, которые отобразятся в этой категории, включают _Audit_ и _Deny_. Каждое действие, выполняемое Политикой, моделируется как операция для ресурса.

### <a name="sample-policy-event"></a>Пример события Политики

```json
{
    "authorization": {
        "action": "Microsoft.Resources/checkPolicyCompliance/read",
        "scope": "/subscriptions/<subscriptionID>"
    },
    "caller": "33a68b9d-63ce-484c-a97e-94aef4c89648",
    "channels": "Operation",
    "claims": {
        "aud": "https://management.azure.com/",
        "iss": "https://sts.windows.net/1114444b-7467-4144-a616-e3a5d63e147b/",
        "iat": "1234567890",
        "nbf": "1234567890",
        "exp": "1234567890",
        "aio": "A3GgTJdwK4vy7Fa7l6DgJC2mI0GX44tML385OpU1Q+z+jaPnFMwB",
        "appid": "1d78a85d-813d-46f0-b496-dd72f50a3ec0",
        "appidacr": "2",
        "http://schemas.microsoft.com/identity/claims/identityprovider": "https://sts.windows.net/1114444b-7467-4144-a616-e3a5d63e147b/",
        "http://schemas.microsoft.com/identity/claims/objectidentifier": "f409edeb-4d29-44b5-9763-ee9348ad91bb",
        "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier": "b-24Jf94A3FH2sHWVIFqO3-RSJEiv24Jnif3gj7s",
        "http://schemas.microsoft.com/identity/claims/tenantid": "1114444b-7467-4144-a616-e3a5d63e147b",
        "uti": "IdP3SUJGtkGlt7dDQVRPAA",
        "ver": "1.0"
    },
    "correlationId": "b5768deb-836b-41cc-803e-3f4de2f9e40b",
    "description": "",
    "eventDataId": "d0d36f97-b29c-4cd9-9d3d-ea2b92af3e9d",
    "eventName": {
        "value": "EndRequest",
        "localizedValue": "End request"
    },
    "category": {
        "value": "Policy",
        "localizedValue": "Policy"
    },
    "eventTimestamp": "2019-01-15T13:19:56.1227642Z",
    "id": "/subscriptions/<subscriptionID>/resourceGroups/myResourceGroup/providers/Microsoft.Sql/servers/contososqlpolicy/events/13bbf75f-36d5-4e66-b693-725267ff21ce/ticks/636831551961227642",
    "level": "Warning",
    "operationId": "04e575f8-48d0-4c43-a8b3-78c4eb01d287",
    "operationName": {
        "value": "Microsoft.Authorization/policies/audit/action",
        "localizedValue": "Microsoft.Authorization/policies/audit/action"
    },
    "resourceGroupName": "myResourceGroup",
    "resourceProviderName": {
        "value": "Microsoft.Sql",
        "localizedValue": "Microsoft SQL"
    },
    "resourceType": {
        "value": "Microsoft.Resources/checkPolicyCompliance",
        "localizedValue": "Microsoft.Resources/checkPolicyCompliance"
    },
    "resourceId": "/subscriptions/<subscriptionID>/resourceGroups/myResourceGroup/providers/Microsoft.Sql/servers/contososqlpolicy",
    "status": {
        "value": "Succeeded",
        "localizedValue": "Succeeded"
    },
    "subStatus": {
        "value": "",
        "localizedValue": ""
    },
    "submissionTimestamp": "2019-01-15T13:20:17.1077672Z",
    "subscriptionId": "<subscriptionID>",
    "properties": {
        "isComplianceCheck": "True",
        "resourceLocation": "westus2",
        "ancestors": "72f988bf-86f1-41af-91ab-2d7cd011db47",
        "policies": "[{\"policyDefinitionId\":\"/subscriptions/<subscriptionID>/providers/Microsoft.
            Authorization/policyDefinitions/5775cdd5-d3d3-47bf-bc55-bb8b61746506/\",\"policyDefiniti
            onName\":\"5775cdd5-d3d3-47bf-bc55-bb8b61746506\",\"policyDefinitionEffect\":\"Deny\",\"
            policyAssignmentId\":\"/subscriptions/<subscriptionID>/providers/Microsoft.Authorization
            /policyAssignments/991a69402a6c484cb0f9b673/\",\"policyAssignmentName\":\"991a69402a6c48
            4cb0f9b673\",\"policyAssignmentScope\":\"/subscriptions/<subscriptionID>\",\"policyAssig
            nmentParameters\":{}}]"
    },
    "relatedEvents": []
}
```

### <a name="policy-event-property-descriptions"></a>Описания свойств события Политики

| Имя элемента | Описание |
| --- | --- |
| authorization | Массив со свойствами RBAC события. Для новых ресурсов — это действие и область запроса, вызвавшего вычисление. Для имеющихся ресурсов действием является "Microsoft.Resources/checkPolicyCompliance/read". |
| caller | Для новых ресурсов — это удостоверение, которое запустило развертывание. Для имеющихся ресурсов — глобальный уникальный идентификатор поставщика ресурсов Microsoft Azure Policy Insights. |
| каналов | События политики используют только канал "Operation". |
| claims | Токен JWT, который используется Active Directory для аутентификации пользователя или приложения при выполнении этой операции в Resource Manager. |
| correlationId | Обычно GUID в строковом формате. События, которые совместно используют идентификатор correlationId, принадлежат к одному общему действию. |
| description | Это поле будет пустым для событий Политики. |
| eventDataId | Уникальный идентификатор события. |
| eventName | "BeginRequest" или "EndRequest". "BeginRequest" используется для отложенной оценки auditIfNotExists и deployIfNotExists, а также при запуске эффектом deployIfNotExists развертывания шаблона. Все остальные операции возвращают "EndRequest". |
| category | Объявляет события журнала действий как относящиеся к "Policy". |
| eventTimestamp | Метка времени, когда служба Azure создала событие при обработке соответствующего этому событию запроса. |
| id | Уникальный идентификатор события для конкретного ресурса. |
| level | Уровень события. Audit использует значение "Warning", а Deny — "Error". Ошибка auditIfNotExists или deployIfNotExists может создать значения "Warning" и "Error" в зависимости от серьезности. Все события Политики используют значение "Informational". |
| operationId | События, относящиеся к одной операции, совместно используют один GUID. |
| operationName | Имя операции непосредственно коррелируется c эффектом Политики. |
| resourceGroupName | Имя группы ресурсов для оцениваемого ресурса. |
| resourceProviderName | Имя поставщика ресурсов для оцениваемого ресурса. |
| тип_ресурса | Для новых ресурсов — это оцениваемый тип. Для имеющихся ресурсов — возвращает "Microsoft.Resources/checkPolicyCompliance/read". |
| resourceId | Идентификатор оцениваемого ресурса. |
| status | Строка, описывающая состояние результата оценки Политики. Большинство оценок Политики возвращают значение "Succeeded", но эффект Deny возвращает значение "Failed". Ошибки в auditIfNotExists или deployIfNotExists также возвращают значение "Failed". |
| subStatus | Поле будет пустым для событий Политики. |
| submissionTimestamp | Метка времени, когда событие стало доступно для запросов. |
| subscriptionId | Идентификатор подписки Azure. |
| properties.isComplianceCheck | Возвращает значение "False" при развертывании нового ресурса или обновлении свойств Azure Resource Manager имеющегося ресурса. Все остальные [триггеры оценки](../../governance/policy/how-to/get-compliance-data.md#evaluation-triggers) приводят к значению "True". |
| properties.resourceLocation | Регион Azure оцениваемого ресурса. |
| properties.ancestors | Разделенный запятыми список родительских групп управления, упорядоченный от непосредственно родительской к самой дальней родительской. |
| properties.policies | Содержит сведения об определении политики, назначении, влиянии и параметрах, результатом которых является эта оценка Политики. |
| relatedEvents | Это поле будет пустым для событий Политики. |

## <a name="mapping-to-diagnostic-logs-schema"></a>Сопоставление со схемой журналов диагностики

При потоковой передаче журнала действий Azure в учетную запись хранения или пространство имен Центров событий данные соответствуют [схеме журналов системы диагностики Azure](./diagnostic-logs-schema.md). Ниже приведено сопоставление свойств приведенной выше схемы и схемы журналов диагностики:

| Свойство схемы журналов диагностики | Свойство схемы журнала действий REST API | Примечания |
| --- | --- | --- |
| time | eventTimestamp |  |
| resourceId | resourceId | Параметры subscriptionId, resourceType и resourceGroupName получаются из параметра resourceId. |
| operationName | operationName.value |  |
| category | Часть имени операции | Разделитель типа операции — "Запись", "Удаление" или "Действие" |
| resultType | status.value | |
| resultSignature | substatus.value | |
| resultDescription | description |  |
| durationMs | Н/Д | Всегда 0 |
| callerIpAddress | httpRequest.clientIpAddress |  |
| correlationId | correlationId |  |
| identity | свойства удостоверений и авторизации |  |
| Уровень | Уровень |  |
| location | Н/Д | Расположение, где было обработано событие. *Это не расположение ресурса, а место, где событие было обработано. Это свойство будет удалено в будущем обновлении.* |
| properties | properties.eventProperties |  |
| properties.eventCategory | category | Если свойство properties.eventCategory не задано, параметр category равен "Административная" |
| properties.eventName | eventName |  |
| properties.operationId | operationId |  |
| properties.eventProperties | properties |  |


## <a name="next-steps"></a>Дальнейшие действия
* [См. дополнительные сведения о журнале действий](activity-logs-overview.md)
* [Экспорт журнала действий в службе хранилища Azure или концентраторы событий](activity-log-export.md)

