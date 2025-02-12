---
title: 'Azure Backup: Создание политик архивации с помощью REST API'
description: Управление политиками архивации (планирование и хранение) с помощью REST API
author: pvrk
manager: shivamg
keywords: REST API; Azure VM backup; Azure VM restore;
ms.service: backup
ms.topic: conceptual
ms.date: 08/21/2018
ms.author: pullabhk
ms.assetid: 5ffc4115-0ae5-4b85-a18c-8a942f6d4870
ms.openlocfilehash: f0729a49c3dc72a28431d711e6783abda96d2ce3
ms.sourcegitcommit: c72ddb56b5657b2adeb3c4608c3d4c56e3421f2c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/24/2019
ms.locfileid: "68466817"
---
# <a name="create-azure-recovery-services-backup-policies-using-rest-api"></a>Создание политик резервного копирования Служб восстановления Azure с помощью REST API

Шаги по созданию политики резервного копирования для хранилища Служб восстановления Azure описаны в [документе о политике REST API](https://docs.microsoft.com/rest/api/backup/protectionpolicies/createorupdate). Мы используем этот документ в качестве ссылки для создания политики для резервной копии виртуальной машины Azure.

## <a name="backup-policy-essentials"></a>Основные компоненты политики резервного копирования

- Политика резервного копирования создается в каждом хранилище.
- Политику резервного копирования можно создать для резервного копирования следующих рабочих нагрузок
  - Azure
  - SQL на виртуальной машине Azure
  - Общая папка Azure
- Политики могут назначаться нескольким ресурсам. Политику резервного копирования виртуальной машины Azure можно использовать для защиты нескольких виртуальных машин Azure.
- Политика состоит из двух компонентов
  - Расписание Время выполнения резервного копирования
  - Хранения Время хранения каждой резервной копии.
- Расписание может определяться как ежедневное или еженедельное с настройкой определенного момента времени.
- Период хранения можно определить для ежедневных, еженедельных, ежемесячных и ежегодных точек резервного копирования.
- "Еженедельно" относиться к резервному копированию в определенный день недели, "Ежемесячно" — к резервному копированию в определенный день месяца, а "Ежегодно" — к резервному копированию в определенный день года.
- Период хранения для ежемесячных и ежегодных точек резервного копирования называется LongTermRetention.
- При создании хранилища также создается политика для резервного копирования виртуальных машин Azure с именем DefaultPolicy, которую можно использовать для резервного копирования виртуальных машин Azure.

Для создания или обновления политики Azure Backup используйте следующую операцию *PUT*.

```http
PUT https://management.azure.com/Subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.RecoveryServices/vaults/{vaultName}/backupPolicies/{policyName}?api-version=2016-12-01
```

`{policyName}` и `{vaultName}` предоставляются в URI. Дополнительные сведения предоставляются в тексте запроса.

## <a name="create-the-request-body"></a>Создание текста запроса

Например, чтобы создать политику для восстановления диска из резервной копии виртуальной машины Azure, выполните компоненты текста запроса.

|Имя  |Обязательное значение  |Тип  |Описание  |
|---------|---------|---------|---------|
|свойства     |   True      |  ProtectionPolicy:[AzureIaaSVMProtectionPolicy](https://docs.microsoft.com/rest/api/backup/protectionpolicies/createorupdate#azureiaasvmprotectionpolicy)      | Свойства ProtectionPolicyResource        |
|теги     |         | Object        |  Теги ресурсов       |

Полный список определений в тексте запроса см. в [документе REST API о политике резервного копирования](https://docs.microsoft.com/rest/api/backup/protectionpolicies/createorupdate).

### <a name="example-request-body"></a>Примеры текста запроса

Следующий текст запроса определяет политику резервного копирования для резервных копий виртуальных машин Azure.

Политика включает:

- создание еженедельной резервной копии каждый понедельник, среду, четверг в 10:00 по тихоокеанскому времени США (зима);
- хранение резервных копий, создающихся каждый понедельник, среду, четверг, на одну неделю;
- хранение резервных копий, создающихся каждую первую среду и третий четверг месяца в течение двух месяцев (перезаписывает предыдущие условия сохранения, если такие есть);
- хранение резервных копий, создающихся в четвертый понедельник и четвертый четверг февраля и ноября в течение четырех лет (перезаписывает предыдущие условия сохранения, если такие есть).

```json
{
  "properties": {
    "backupManagementType": "AzureIaasVM",
    "timeZone": "Pacific Standard Time",
    "schedulePolicy": {
      "schedulePolicyType": "SimpleSchedulePolicy",
      "scheduleRunFrequency": "Weekly",
      "scheduleRunTimes": [
        "2018-01-24T10:00:00Z"
      ],
      "scheduleRunDays": [
        "Monday",
        "Wednesday",
        "Thursday"
      ]
    },
    "retentionPolicy": {
      "retentionPolicyType": "LongTermRetentionPolicy",
      "weeklySchedule": {
        "daysOfTheWeek": [
          "Monday",
          "Wednesday",
          "Thursday"
        ],
        "retentionTimes": [
          "2018-01-24T10:00:00Z"
        ],
        "retentionDuration": {
          "count": 1,
          "durationType": "Weeks"
        }
      },
      "monthlySchedule": {
        "retentionScheduleFormatType": "Weekly",
        "retentionScheduleWeekly": {
          "daysOfTheWeek": [
            "Wednesday",
            "Thursday"
          ],
          "weeksOfTheMonth": [
            "First",
            "Third"
          ]
        },
        "retentionTimes": [
          "2018-01-24T10:00:00Z"
        ],
        "retentionDuration": {
          "count": 2,
          "durationType": "Months"
        }
      },
      "yearlySchedule": {
        "retentionScheduleFormatType": "Weekly",
        "monthsOfYear": [
          "February",
          "November"
        ],
        "retentionScheduleWeekly": {
          "daysOfTheWeek": [
            "Monday",
            "Thursday"
          ],
          "weeksOfTheMonth": [
            "Fourth"
          ]
        },
        "retentionTimes": [
          "2018-01-24T10:00:00Z"
        ],
        "retentionDuration": {
          "count": 4,
          "durationType": "Years"
        }
      }
    }
  }
}
```

> [!IMPORTANT]
> Форматы времени для расписания и хранения поддерживают только DateTime. Они не поддерживают только формат времени.

## <a name="responses"></a>Responses

Создание и обновление политики резервного копирования является [асинхронной операцией](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-async-operations). Это означает, что такая операция создает другую операцию, которая должна отслеживаться отдельно.

Она возвращает два ответа: 202 (принято), когда создается другая операция, и 200 (ОК), когда эта операция завершается.

|Название  |Тип  |Описание  |
|---------|---------|---------|
|200 ОК     |    [ProtectionPolicyResource](https://docs.microsoft.com/rest/api/backup/protectionpolicies/createorupdate#protectionpolicyresource)     |  OK       |
|202 — принято     |         |     Принято    |

### <a name="example-responses"></a>Примеры ответов

После отправки запроса *PUT* для создания или обновления политики будет получен первоначальный ответ 202 (принято) с заголовком расположения или асинхронным заголовком Azure.

```http
HTTP/1.1 202 Accepted
Pragma: no-cache
Retry-After: 60
Azure-AsyncOperation: https://management.azure.com/Subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/SwaggerTestRg/providers/Microsoft.RecoveryServices/vaults/testVault/backupPolicies/testPolicy1/operations/00000000-0000-0000-0000-000000000000?api-version=2016-06-01
X-Content-Type-Options: nosniff
x-ms-request-id: db785be0-bb20-4598-bc9f-70c9428b170b
x-ms-client-request-id: e1f94eef-9b2d-45c4-85b8-151e12b07d03; e1f94eef-9b2d-45c4-85b8-151e12b07d03
Strict-Transport-Security: max-age=31536000; includeSubDomains
x-ms-ratelimit-remaining-subscription-writes: 1199
x-ms-correlation-request-id: db785be0-bb20-4598-bc9f-70c9428b170b
x-ms-routing-request-id: SOUTHINDIA:20180521T073907Z:db785be0-bb20-4598-bc9f-70c9428b170b
Cache-Control: no-cache
Date: Mon, 21 May 2018 07:39:06 GMT
Location: https://management.azure.com/Subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/SwaggerTestRg/providers/Microsoft.RecoveryServices/vaults/testVault/backupPolicies/testPolicy1/operationResults/00000000-0000-0000-0000-000000000000?api-version=2016-06-01
X-Powered-By: ASP.NET
```

Затем отследите итоговую операцию, используя заголовок location или Azure-AsyncOperation с помощью простой команды *GET*.

```http
GET https://management.azure.com/Subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/SwaggerTestRg/providers/Microsoft.RecoveryServices/vaults/testVault/backupPolicies/testPolicy1/operationResults/00000000-0000-0000-0000-000000000000?api-version=2016-06-01
```

После завершения операции он возвращает код 200 (ОК) с содержимым политики в тексте ответа.

```json
{
  "id": "/Subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/SwaggerTestRg/providers/Microsoft.RecoveryServices/vaults/testVault/backupPolicies/testPolicy1",
  "name": "testPolicy1",
  "type": "Microsoft.RecoveryServices/vaults/backupPolicies",
  "properties": {
    "backupManagementType": "AzureIaasVM",
    "schedulePolicy": {
      "schedulePolicyType": "SimpleSchedulePolicy",
      "scheduleRunFrequency": "Weekly",
      "scheduleRunDays": [
        "Monday",
        "Wednesday",
        "Thursday"
      ],
      "scheduleRunTimes": [
        "2018-01-24T10:00:00Z"
      ],
      "scheduleWeeklyFrequency": 0
    },
    "retentionPolicy": {
      "retentionPolicyType": "LongTermRetentionPolicy",
      "weeklySchedule": {
        "daysOfTheWeek": [
          "Monday",
          "Wednesday",
          "Thursday"
        ],
        "retentionTimes": [
          "2018-01-24T10:00:00Z"
        ],
        "retentionDuration": {
          "count": 1,
          "durationType": "Weeks"
        }
      },
      "monthlySchedule": {
        "retentionScheduleFormatType": "Weekly",
        "retentionScheduleWeekly": {
          "daysOfTheWeek": [
            "Wednesday",
            "Thursday"
          ],
          "weeksOfTheMonth": [
            "First",
            "Third"
          ]
        },
        "retentionTimes": [
          "2018-01-24T10:00:00Z"
        ],
        "retentionDuration": {
          "count": 2,
          "durationType": "Months"
        }
      },
      "yearlySchedule": {
        "retentionScheduleFormatType": "Weekly",
        "monthsOfYear": [
          "February",
          "November"
        ],
        "retentionScheduleWeekly": {
          "daysOfTheWeek": [
            "Monday",
            "Thursday"
          ],
          "weeksOfTheMonth": [
            "Fourth"
          ]
        },
        "retentionTimes": [
          "2018-01-24T10:00:00Z"
        ],
        "retentionDuration": {
          "count": 4,
          "durationType": "Years"
        }
      }
    },
    "timeZone": "Pacific Standard Time",
    "protectedItemsCount": 0
  }
}
```

Если политика уже используется для защиты элемента, любое обновление в политике приведет к [изменению защиты](backup-azure-arm-userestapi-backupazurevms.md#changing-the-policy-of-protection) всех связанных элементов.

## <a name="next-steps"></a>Следующие шаги

[Включите защиту для незащищенной виртуальной машины Azure](backup-azure-arm-userestapi-backupazurevms.md).

Дополнительные сведения о REST API Azure Backup с использованием REST API см. в следующих документах:

- [Recovery Services](/rest/api/recoveryservices/) (Службы восстановления)
- [Начало работы с Azure REST API](/rest/api/azure/)