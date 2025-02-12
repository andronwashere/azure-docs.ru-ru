---
title: Управление доступом на основе ролей в службе автоматизации Azure
description: Контроль доступа на основе ролей (RBAC) Azure обеспечивает управление доступом к ресурсам Azure. В этой статье описывается настройка RBAC в службе автоматизации Azure.
keywords: автоматизация RBAC, контроль доступа на основе ролей, RBAC Azure
services: automation
ms.service: automation
ms.subservice: shared-capabilities
author: bobbytreed
ms.author: robreed
ms.date: 05/17/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 9b2bcdf3d74c6946b8c9f0dacaeabf28d9c76f94
ms.sourcegitcommit: f811238c0d732deb1f0892fe7a20a26c993bc4fc
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/29/2019
ms.locfileid: "67477732"
---
# <a name="role-based-access-control-in-azure-automation"></a>Управление доступом на основе ролей в службе автоматизации Azure

Контроль доступа на основе ролей (RBAC) Azure обеспечивает управление доступом к ресурсам Azure. С помощью [RBAC](../role-based-access-control/overview.md) вы сможете распределить обязанности внутри своей команды и предоставить доступ пользователям, группам и приложениям на том уровне, который им необходим для выполнения поставленных задач. Для предоставления доступа на основе ролей можно использовать портал Azure, программы командной строки Azure и API управления Azure.

## <a name="roles-in-automation-accounts"></a>Роли в учетных записях службы автоматизации

В службе автоматизации Azure доступ предоставляется путем назначения соответствующей роли RBAC пользователям, группам и приложениям в области учетной записи автоматизации. Ниже перечислены встроенные роли, поддерживаемые учетной записью автоматизации.

| **Роль** | **Описание** |
|:--- |:--- |
| Владелец. |Роль владельца обеспечивает доступ ко всем ресурсам и действиям в учетной записи службы автоматизации, включая предоставление доступа на управление учетной записью службы автоматизации другим пользователям, группам и приложениям. |
| участник; |Роль участника позволяет управлять всем, кроме изменения разрешений других пользователей на доступ к учетной записи службы автоматизации. |
| Читатель |Роль читателя позволяет просматривать все ресурсы в учетной записи службы автоматизации, но не дает возможность вносить какие-либо изменения. |
| Оператор службы автоматизации |Роль оператора службы автоматизации позволяет просматривать имя и свойства модуля runbook, а также создавать задания и управлять ими для всех модулей runbook в учетной записи службы автоматизации. Эта роль пригодится, если вам нужно защитить ресурсы учетной службы автоматизации, такие как учетные данные, активы и модули Runbook, от просмотра и изменения, и в то же время разрешить членам вашей организации выполнять эти модули Runbook. |
|Оператор заданий службы автоматизации|Роль оператора заданий службы автоматизации позволяет создавать задания и управлять ими для всех модулей runbook в учетной записи службы автоматизации.|
|Оператор Runbook службы автоматизации|Роль оператора runbook службы автоматизации позволяет просматривать имя и свойства модуля runbook.|
| участник Log Analytics. | Участник Log Analytics может считывать все данные мониторинга и изменять его параметры. Изменение параметров мониторинга подразумевает добавление расширений в виртуальные машины, чтение ключей учетной записи хранения для настройки коллекции журналов в службе хранилища Microsoft Azure, создание и настройку учетных записей службы автоматизации, добавление решений и настройку диагностики Azure во всех ресурсах Azure.|
| читатель Log Analytics; | Читатель Log Analytics может просматривать параметры мониторинга, выполнять поиск всех данных мониторинга и просматривать эти данные. Читатель также может просматривать конфигурации диагностики Azure на всех ресурсах Azure. |
| Monitoring Contributor | Участник мониторинга может считывать все данные мониторинга и изменять его параметры.|
| Роль Monitoring Reader | Читатель мониторинга может считывать все данные мониторинга. |
| Администратор доступа пользователей |Роль администратора доступа пользователей позволяет управлять доступом пользователей к учетным записям службы автоматизации Azure. |

## <a name="role-permissions"></a>Разрешения ролей

В следующих таблицах описываются разрешения, предоставленные каждой роли. Это могут быть свойства Actions, которые предоставляют разрешения, и свойства NotActions, которые их ограничивают.

### <a name="owner"></a>Владелец.

Владелец может управлять всем, включая доступ. В следующей таблице показаны разрешения, предоставленные для этой роли.

|Действия|ОПИСАНИЕ|
|---|---|
|Microsoft.Automation/automationAccounts/|Создание ресурсов всех типов и управление ими.|

### <a name="contributor"></a>участник;

Участник может управлять всем, кроме доступа. В следующей таблице показаны разрешения, которые предоставлены и запрещены для этой роли.

|**Действия**  |**Описание**  |
|---------|---------|
|Microsoft.Automation/automationAccounts/|Создание ресурсов всех типов и управление ими|
|**Запрещенные действия**||
|Microsoft.Authorization/*/Delete| Удаление и назначение ролей.       |
|Microsoft.Authorization/*/Write     |  Создание и назначение ролей.       |
|Microsoft.Authorization/elevateAccess/Action    | Запрет создания администратора доступа пользователей.       |

### <a name="reader"></a>Читатель

Читатель может просматривать все ресурсы в учетной записи службы автоматизации, но не может вносить какие-либо изменения.

|**Действия**  |**Описание**  |
|---------|---------|
|Microsoft.Automation/automationAccounts/read|Просмотр всех ресурсов в учетной записи службы автоматизации. |

### <a name="automation-operator"></a>Оператор службы автоматизации

Оператор службы автоматизации может создавать задания и управлять ими, а также просматривать имена и свойства всех модулей runbook в учетной записи службы автоматизации.  Примечание. Если вы хотите контролировать доступ операторов к отдельным модулям runbook, не используйте эту роль, а вместо нее назначьте сочетание ролей "Оператор задания автоматизации" и "Оператор Runbook автоматизации". В следующей таблице показаны разрешения, предоставленные для этой роли.

|**Действия**  |**Описание**  |
|---------|---------|
|Microsoft.Authorization/*/read|Авторизация на чтение.|
|Microsoft.Automation/automationAccounts/hybridRunbookWorkerGroups/read|Считывает ресурсы гибридной рабочей роли Runbook.|
|Microsoft.Automation/automationAccounts/jobs/read|Вывод списка заданий Runbook.|
|Microsoft.Automation/automationAccounts/jobs/resume/action|Возобновление приостановленного задания.|
|Microsoft.Automation/automationAccounts/jobs/stop/action|Отмена выполняющегося задания.|
|Microsoft.Automation/automationAccounts/jobs/streams/read|Чтение потоков и выходных данных задания.|
|Microsoft.Automation/automationAccounts/jobs/output/read|Возвращает выходные данные задания.|
|Microsoft.Automation/automationAccounts/jobs/suspend/action|Остановка выполняющегося задания.|
|Microsoft.Automation/automationAccounts/jobs/write|Создание заданий.|
|Microsoft.Automation/automationAccounts/jobSchedules/read|Возвращает расписание заданий службы автоматизации Azure.|
|Microsoft.Automation/automationAccounts/jobSchedules/write|Создает расписание заданий службы автоматизации Azure.|
|Microsoft.Automation/automationAccounts/linkedWorkspace/read|Возвращает рабочую область, связанную с учетной записью службы автоматизации.|
|Microsoft.Automation/automationAccounts/read|Возвращает учетную запись службы автоматизации Azure.|
|Microsoft.Automation/automationAccounts/runbooks/read|Возвращает runbook службы автоматизации Azure.|
|Microsoft.Automation/automationAccounts/schedules/read|Возвращает ресурс расписания службы автоматизации Azure.|
|Microsoft.Automation/automationAccounts/schedules/write|Создает или обновляет ресурс расписания службы автоматизации Azure.|
|Microsoft.Resources/subscriptions/resourceGroups/read      |Чтение ролей и их назначений.         |
|Microsoft.Resources/deployments/*      |Создание развертываний группы ресурсов и управление ими.         |
|Microsoft.Insights/alertRules/*      | Создание правил оповещения и управление ими.        |
|Microsoft.Support/* |Создание запросов в службу поддержки и управление ими.|

### <a name="automation-job-operator"></a>Оператор заданий службы автоматизации

Роль оператора заданий службы автоматизации предоставляется на уровне учетной записи службы автоматизации. Она предоставляет разрешения на создание заданий и управление ими для всех модулей runbook в учетной записи. В следующей таблице показаны разрешения, предоставленные для этой роли.

|**Действия**  |**Описание**  |
|---------|---------|
|Microsoft.Authorization/*/read|Авторизация на чтение.|
|Microsoft.Automation/automationAccounts/jobs/read|Вывод списка заданий Runbook.|
|Microsoft.Automation/automationAccounts/jobs/resume/action|Возобновление приостановленного задания.|
|Microsoft.Automation/automationAccounts/jobs/stop/action|Отмена выполняющегося задания.|
|Microsoft.Automation/automationAccounts/jobs/streams/read|Чтение потоков и выходных данных задания.|
|Microsoft.Automation/automationAccounts/jobs/suspend/action|Остановка выполняющегося задания.|
|Microsoft.Automation/automationAccounts/jobs/write|Создание заданий.|
|Microsoft.Resources/subscriptions/resourceGroups/read      |  Чтение ролей и их назначений.       |
|Microsoft.Resources/deployments/*      |Создание развертываний группы ресурсов и управление ими.         |
|Microsoft.Insights/alertRules/*      | Создание правил оповещения и управление ими.        |
|Microsoft.Support/* |Создание запросов в службу поддержки и управление ими.|

### <a name="automation-runbook-operator"></a>Оператор Runbook службы автоматизации

Роль оператора Runbook службы автоматизации предоставляется в области Runbook. Оператор runbook службы автоматизации может просматривать имя и свойства модуля runbook.  Эта роль в сочетании с ролью "Оператор заданий службы автоматизации" позволяет создавать задания и управлять ими для модуля runbook. В следующей таблице показаны разрешения, предоставленные для этой роли.

|**Действия**  |**Описание**  |
|---------|---------|
|Microsoft.Automation/automationAccounts/runbooks/read     | Вывод списка Runbook.        |
|Microsoft.Authorization/*/read      | Авторизация на чтение.        |
|Microsoft.Resources/subscriptions/resourceGroups/read      |Чтение ролей и их назначений.         |
|Microsoft.Resources/deployments/*      | Создание развертываний группы ресурсов и управление ими.         |
|Microsoft.Insights/alertRules/*      | Создание правил оповещения и управление ими.        |
|Microsoft.Support/*      | Создание запросов в службу поддержки и управление ими.        |

### <a name="log-analytics-contributor"></a>участник Log Analytics.

Участник Log Analytics может читать все данные мониторинга и изменять его параметры. Изменение параметров мониторинга подразумевает добавление расширений в виртуальные машины, чтение ключей учетной записи хранения для настройки коллекции журналов в службе хранилища Azure, создание и настройку учетных записей службы автоматизации, добавление решений и настройку диагностики Azure во всех ресурсах Azure. В следующей таблице показаны разрешения, предоставленные для этой роли.

|**Действия**  |**Описание**  |
|---------|---------|
|*/чтение|Чтение ресурсов всех типов, кроме секретов.|
|Microsoft.Automation/automationAccounts/*|Управление учетными записями службы автоматизации.|
|Microsoft.ClassicCompute/virtualMachines/extensions/*|Создание расширений виртуальных машин и управление ими.|
|Microsoft.ClassicStorage/storageAccounts/listKeys/action|Вывод списка ключей классической учетной записи хранения.|
|Microsoft.Compute/virtualMachines/extensions/*|Создание расширений классических виртуальных машин и управление ими.|
|Microsoft.Insights/alertRules/*|Чтение, запись и удаление правила генерации оповещений.|
|Microsoft.Insights/diagnosticSettings/*|Чтение, запись и удаление параметров диагностики.|
|Microsoft.OperationalInsights/*|Управление журналами Azure Monitor.|
|Microsoft.OperationsManagement/*|Управление решениями в рабочих пространствах.|
|Microsoft.Resources/deployments/*|Создание развертываний группы ресурсов и управление ими.|
|Microsoft.Resources/subscriptions/resourcegroups/deployments/*|Создание развертываний группы ресурсов и управление ими.|
|Microsoft.Storage/storageAccounts/listKeys/action|Составление списка ключей учетной записи хранения.|
|Microsoft.Support/*|Создание запросов в службу поддержки и управление ими.|

### <a name="log-analytics-reader"></a>читатель Log Analytics;

Читатель Log Analytics может просматривать и искать все данные мониторинга, а также просматривать его параметры, в том числе конфигурацию Диагностики Azure во всех ресурсах Azure. В следующей таблице показаны разрешения, предоставленные или запрещенные для этой роли.

|**Действия**  |**Описание**  |
|---------|---------|
|*/чтение|Чтение ресурсов всех типов, кроме секретов.|
|Microsoft.OperationalInsights/workspaces/analytics/query/action|Управление запросами в журналах Azure Monitor.|
|Microsoft.OperationalInsights/workspaces/search/action|Поиск журналов данных Azure Monitor.|
|Microsoft.Support/*|Создание запросов в службу поддержки и управление ими.|
|**Запрещенные действия**| |
|Microsoft.OperationalInsights/workspaces/sharedKeys/read|Чтение ключей общего доступа запрещено.|

### <a name="monitoring-contributor"></a>Monitoring Contributor

Участник мониторинга может читать все данные мониторинга и изменять его параметры. В следующей таблице показаны разрешения, предоставленные для этой роли.

|**Действия**  |**Описание**  |
|---------|---------|
|*/чтение|Чтение ресурсов всех типов, кроме секретов.|
|Microsoft.AlertsManagement/alerts/*|Управление оповещениями.|
|Microsoft.AlertsManagement/alertsSummary/*|Управление панелью мониторинга оповещений.|
|Microsoft.Insights/AlertRules/*|Управление правилами генерации оповещений.|
|Microsoft.Insights/components/*|Управление компонентами Application Insights.|
|Microsoft.Insights/DiagnosticSettings/*|Управление параметрами диагностики.|
|Microsoft.Insights/eventtypes/*|Вывод списка событий журнала действий (событий управления) в подписке. Это разрешение применяется для доступа к журналу действий посредством кода или портала.|
|Microsoft.Insights/LogDefinitions/*|Это разрешение необходимо пользователям, которым требуется доступ к журналам действия на портале. Получение списка категорий журнала в журнале действий.|
|Microsoft.Insights/MetricDefinitions/*|Чтение определений метрик (вывод списка доступных типов метрик для ресурса).|
|Microsoft.Insights/Metrics/*|Чтение метрик для ресурса.|
|Microsoft.Insights/Register/Action|Регистрация поставщика Microsoft.Insights.|
|Microsoft.Insights/webtests/*|Управление веб-тестами Application Insights.|
|Microsoft.OperationalInsights/workspaces/intelligencepacks/*|Управление пакетами решение журналы Azure Monitor.|
|Microsoft.OperationalInsights/workspaces/savedSearches/*|Управление поисками сохраняться журналы Azure Monitor.|
|Microsoft.OperationalInsights/workspaces/search/action|Поиск в рабочих областях Log Analytics.|
|Microsoft.OperationalInsights/workspaces/sharedKeys/action|Получение списка ключей для рабочей области Log Analytics.|
|Microsoft.OperationalInsights/workspaces/storageinsightconfigs/*|Управление конфигурациями подробных данных хранилища журналов Azure Monitor.|
|Microsoft.Support/*|Создание запросов в службу поддержки и управление ими.|
|Microsoft.WorkloadMonitor/workloads/*|Управление рабочими нагрузками.|

### <a name="monitoring-reader"></a>Роль Monitoring Reader

Читатель мониторинга может читать все данные мониторинга. В следующей таблице показаны разрешения, предоставленные для этой роли.

|**Действия**  |**Описание**  |
|---------|---------|
|*/чтение|Чтение ресурсов всех типов, кроме секретов.|
|Microsoft.OperationalInsights/workspaces/search/action|Поиск в рабочих областях Log Analytics.|
|Microsoft.Support/*|Создание запросов в службу поддержки и управление ими|

### <a name="user-access-administrator"></a>Администратор доступа пользователей

Администратор доступа пользователей может управлять доступом пользователей к ресурсам Azure. В следующей таблице показаны разрешения, предоставленные для этой роли.

|**Действия**  |**Описание**  |
|---------|---------|
|*/чтение|Чтение всех ресурсов|
|Microsoft.Authorization/*|Управление авторизацией|
|Microsoft.Support/*|Создание запросов в службу поддержки и управление ими|

## <a name="onboarding"></a>Переход

В следующих таблицах показаны минимальные требуемые разрешения, необходимые для подключения виртуальных машин к решениям по отслеживанию изменений или управлению обновлениями.

### <a name="onboarding-from-a-virtual-machine"></a>Подключение из виртуальной машины

|**Действие**  |**Разрешение**  |**Минимальная область**  |
|---------|---------|---------|
|Запись нового развертывания      | Microsoft.Resources/deployments/*          |Подписка          |
|Запись новой группы ресурсов      | Microsoft.Resources/subscriptions/resourceGroups/write        | Подписка          |
|Создание рабочего пространства по умолчанию      | Microsoft.OperationalInsights/workspaces/write         | Группа ресурсов         |
|Создание учетной записи      |  Microsoft.Automation/automationAccounts/write        |Группа ресурсов         |
|Связывание рабочей области и учетной записи      |Microsoft.OperationalInsights/workspaces/write</br>Microsoft.Automation/automationAccounts/read|Рабочая область</br>Учетная запись службы автоматизации
|Создание решения      | Microsoft.OperationalInsights/workspaces/intelligencepacks/write |Группа ресурсов          |
|Создание расширения MMA      | Microsoft.Compute/virtualMachines/write         | Виртуальная машина         |
|Создание сохраненного поискового запроса      | Microsoft.OperationalInsights/workspaces/write          | Рабочая область         |
|Создание конфигурации области      | Microsoft.OperationalInsights/workspaces/write          | Рабочая область         |
|Подключение решения к конфигурации области      | Microsoft.OperationalInsights/workspaces/intelligencepacks/write         | Решение         |
|Проверка состояния подключения — чтение рабочей области      | Microsoft.OperationalInsights/workspaces/read         | Рабочая область         |
|Проверка состояния подключения — чтение свойств связанной рабочей области учетной записи     | Microsoft.Automation/automationAccounts/read      | Учетная запись службы автоматизации        |
|Проверка состояния подключения — чтение решения      | Microsoft.OperationalInsights/workspaces/intelligencepacks/read          | Решение         |
|Проверка состояния подключения — чтение виртуальной машины      | Microsoft.Compute/virtualMachines/read         | Виртуальная машина         |
|Проверка состояния подключения — чтение учетной записи      | Microsoft.Automation/automationAccounts/read  |  Учетная запись службы автоматизации   |
| Проверка подключения рабочей области для виртуальной Машины<sup>1</sup>       | Microsoft.OperationalInsights/workspaces/read         | Подписка         |
| Регистрация поставщика Log Analytics |Microsoft.Insights/register/action | Подписка|

<sup>1</sup> это разрешение требуется для подключения через интерфейс портала для виртуальной Машины.

### <a name="onboarding-from-automation-account"></a>Подключение из учетной записи службы автоматизации

|**Действие**  |**Разрешение** |**Минимальная область**  |
|---------|---------|---------|
|Создание развертывания     | Microsoft.Resources/deployments/*        | Подписка         |
|Создание группы ресурсов     | Microsoft.Resources/subscriptions/resourceGroups/write         | Подписка        |
|Колонка AutomationOnboarding — создание рабочей области     |Microsoft.OperationalInsights/workspaces/write           | Группа ресурсов        |
|Колонка AutomationOnboarding — чтение связанной рабочей области     | Microsoft.Automation/automationAccounts/read        | Учетная запись службы автоматизации       |
|Колонка AutomationOnboarding — чтение решения     | Microsoft.OperationalInsights/workspaces/intelligencepacks/read         | Решение        |
|Колонка AutomationOnboarding — чтение рабочей области     | Microsoft.OperationalInsights/workspaces/intelligencepacks/read        | Рабочая область        |
|Создание связи для рабочей области и учетной записи     | Microsoft.OperationalInsights/workspaces/write        | Рабочая область        |
|Запись учетной записи для Shoebox      | Microsoft.Automation/automationAccounts/write        | Учетная запись.        |
|Создание решения      | Microsoft.OperationalInsights/workspaces/intelligencepacks/write        | Группа ресурсов         |
|Создание или изменение сохраненного поискового запроса     | Microsoft.OperationalInsights/workspaces/write        | Рабочая область        |
|Создание или изменение конфигурации области     | Microsoft.OperationalInsights/workspaces/write        | Рабочая область        |
|Подключение решения к конфигурации области      | Microsoft.OperationalInsights/workspaces/intelligencepacks/write         | Решение         |
| Регистрация поставщика Log Analytics |Microsoft.Insights/register/action | Подписка|
|**Шаг 2. Подключение нескольких виртуальных машин**     |         |         |
|Колонка VMOnboarding — создание расширения MMA     | Microsoft.Compute/virtualMachines/write           | Виртуальная машина        |
|Создание или изменение сохраненного поискового запроса     | Microsoft.OperationalInsights/workspaces/write           | Рабочая область        |
|Создание или изменение конфигурации области  | Microsoft.OperationalInsights/workspaces/write   | Рабочая область|

## <a name="update-management"></a>Управление обновлениями

Управление обновлениями действует между несколькими службами для обеспечения их обслуживания. В следующей таблице показаны разрешения, необходимые для развертываний управления обновлениями.

|**Ресурс**  |**Роль**  |**Область**  |
|---------|---------|---------|
|Учетная запись службы автоматизации     | участник Log Analytics.       | Учетная запись службы автоматизации        |
|Учетная запись службы автоматизации    | Участник виртуальной машины        | Группа ресурсов для учетной записи        |
|Рабочая область Log Analytics     | участник Log Analytics.| Рабочая область Log Analytics        |
|Рабочая область Log Analytics |читатель Log Analytics;| Подписка|
|Решение     |участник Log Analytics.         | Решение|
|Виртуальная машина     | Участник виртуальной машины        | Виртуальная машина        |

## <a name="configure-rbac-for-your-automation-account"></a>Настройки RBAC для учетной записи службы автоматизации

В следующем разделе показано, как настроить RBAC в учетной записи службы автоматизации с помощью [портала](#configure-rbac-using-the-azure-portal) и [PowerShell](#configure-rbac-using-powershell).

### <a name="configure-rbac-using-the-azure-portal"></a>Настройка RBAC с помощью портала Azure

1. Войдите на [портал Azure](https://portal.azure.com/) и откройте учетную запись службы автоматизации на странице "Учетные записи автоматизации".
2. Щелкните элемент управления **Управление доступом (IAM)** в левом верхнем углу экрана. Откроется страница **Управление доступом (IAM)** , на которую можно добавить новых пользователей, группы и приложения, чтобы управлять учетной записью службы автоматизации и просматривать имеющиеся роли, настраиваемые для этой учетной записи.
3. Перейдите на вкладку **Назначения ролей**.

   ![Кнопка доступа](media/automation-role-based-access-control/automation-01-access-button.png)

#### <a name="add-a-new-user-and-assign-a-role"></a>Добавление нового пользователя и назначение роли

1. На странице **Управление доступом (IAM)** щелкните **+ Добавить назначение ролей**, чтобы открыть страницу **Добавить назначение ролей**, на которой можно добавить пользователя, группу или приложение, а также назначить им роль.

2. Выберите роль из списка доступных ролей. Вы можете выбрать любую из доступных встроенных ролей, поддерживаемых учетной записью службы автоматизации, или пользовательскую роль, которую вы создали сами.

3. Введите имя пользователя, которому необходимо предоставить разрешения, в поле **Выбрать**. Выберите пользователя из списка и щелкните **Сохранить**.

   ![Добавление пользователей](media/automation-role-based-access-control/automation-04-add-users.png)

   Пользователь будет добавлен на страницу **Пользователи** и ему будет назначена выбранная роль.

   ![Список пользователей](media/automation-role-based-access-control/automation-05-list-users.png)

   Кроме того, роль можно назначить пользователю на странице **Роли**.
4. На странице **Управление доступом (IAM)** щелкните **Роли**, чтобы открыть страницу **Роли**. Здесь отображаются имена ролей и число пользователей и групп, которым она назначена.

    ![Назначение роли из странице пользователей](media/automation-role-based-access-control/automation-06-assign-role-from-users-blade.png)

   > [!NOTE]
   > Управление доступом на основе ролей настраивается только в области учетной записи службы автоматизации и не может настраиваться в ресурсах более низкого уровня.

#### <a name="remove-a-user"></a>Удаление пользователя

Разрешение на доступ для пользователя, который не управляет учетной записью службы автоматизации или прекращает работу в организации, можно удалить. Чтобы удалить пользователя, выполните описанные ниже действия.

1. На странице **Управление доступом (IAM)** выберите пользователя, которого необходимо удалить, и щелкните **Удалить**.
2. Нажмите кнопку **Удалить** в области сведений о назначении.
3. Нажмите кнопку **Да** для подтверждения удаления.

   ![Удаление пользователей](media/automation-role-based-access-control/automation-08-remove-users.png)

### <a name="configure-rbac-using-powershell"></a>Настройка RBAC с помощью PowerShell

Доступ к учетной записи службы автоматизации на основе ролей можно также настроить с помощью указанных ниже [командлетов Azure PowerShell](../role-based-access-control/role-assignments-powershell.md).

Командлет [Get-AzureRmRoleDefinition](/previous-versions/azure/mt603792(v=azure.100)) выводит список всех ролей RBAC, доступных в Azure Active Directory. При использовании со свойством **Name** он позволяет получить список всех действий, которые может выполнить определенная роль.

```azurepowershell-interactive
Get-AzureRmRoleDefinition -Name 'Automation Operator'
```

Ниже приведен пример выходных данных.

```azurepowershell
Name             : Automation Operator
Id               : d3881f73-407a-4167-8283-e981cbba0404
IsCustom         : False
Description      : Automation Operators are able to start, stop, suspend, and resume jobs
Actions          : {Microsoft.Authorization/*/read, Microsoft.Automation/automationAccounts/jobs/read, Microsoft.Automation/automationAccounts/jobs/resume/action,
                   Microsoft.Automation/automationAccounts/jobs/stop/action...}
NotActions       : {}
AssignableScopes : {/}
```

Командлет [Get-AzureRmRoleAssignment](/previous-versions/azure/mt619413(v=azure.100)) выводит список назначений ролей RBAC Azure AD в указанной области. Без параметров эта команда возвращает все назначения ролей, существующие в подписке. С помощью параметра **ExpandPrincipalGroups** можно получить список назначений доступа для указанного пользователя, а также для групп, в состав которых он входит.
    **Пример.** Следующая команда выводит список всех пользователей в учетной записи службы автоматизации с указанием их ролей.

```azurepowershell-interactive
Get-AzureRMRoleAssignment -scope '/subscriptions/<SubscriptionID>/resourcegroups/<Resource Group Name>/Providers/Microsoft.Automation/automationAccounts/<Automation account name>'
```

Ниже приведен пример выходных данных.

```powershell
RoleAssignmentId   : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Automation/automationAccounts/myAutomationAccount/provid
                     ers/Microsoft.Authorization/roleAssignments/cc594d39-ac10-46c4-9505-f182a355c41f
Scope              : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Automation/automationAccounts/myAutomationAccount
DisplayName        : admin@contoso.com
SignInName         : admin@contoso.com
RoleDefinitionName : Automation Operator
RoleDefinitionId   : d3881f73-407a-4167-8283-e981cbba0404
ObjectId           : 15f26a47-812d-489a-8197-3d4853558347
ObjectType         : User
```

Командлет [New-AzureRmRoleAssignment](/previous-versions/azure/mt603580(v=azure.100)) предоставляет доступ пользователям, группам и приложениям к определенной области.
    **Пример.** Следующая команда назначает роль оператора службы автоматизации пользователю в области учетной записи службы автоматизации.

```azurepowershell-interactive
New-AzureRmRoleAssignment -SignInName <sign-in Id of a user you wish to grant access> -RoleDefinitionName 'Automation operator' -Scope '/subscriptions/<SubscriptionID>/resourcegroups/<Resource Group Name>/Providers/Microsoft.Automation/automationAccounts/<Automation account name>'
```

Ниже приведен пример выходных данных.

```azurepowershell
RoleAssignmentId   : /subscriptions/00000000-0000-0000-0000-000000000000/resourcegroups/myResourceGroup/Providers/Microsoft.Automation/automationAccounts/myAutomationAccount/provid
                     ers/Microsoft.Authorization/roleAssignments/25377770-561e-4496-8b4f-7cba1d6fa346
Scope              : /subscriptions/00000000-0000-0000-0000-000000000000/resourcegroups/myResourceGroup/Providers/Microsoft.Automation/automationAccounts/myAutomationAccount
DisplayName        : admin@contoso.com
SignInName         : admin@contoso.com
RoleDefinitionName : Automation Operator
RoleDefinitionId   : d3881f73-407a-4167-8283-e981cbba0404
ObjectId           : f5ecbe87-1181-43d2-88d5-a8f5e9d8014e
ObjectType         : User
```

Командлет [Remove-AzureRmRoleAssignment](/previous-versions/azure/mt603781(v=azure.100)) удаляет доступ указанного пользователя, группы или приложения к определенной области.
    **Пример.** Следующая команда удаляет пользователя из роли оператора службы автоматизации в области учетной записи службы автоматизации.

```azurepowershell-interactive
Remove-AzureRmRoleAssignment -SignInName <sign-in Id of a user you wish to remove> -RoleDefinitionName 'Automation Operator' -Scope '/subscriptions/<SubscriptionID>/resourcegroups/<Resource Group Name>/Providers/Microsoft.Automation/automationAccounts/<Automation account name>'
```

В предыдущих примерах замените **идентификатор для входа**, **идентификатор подписки**, **имя группы ресурсов** и **имя учетной записи службы автоматизации** данными своей учетной записи. Прежде чем продолжить удаление назначений ролей пользователей, выберите **Да** при появлении запроса на подтверждение.

### <a name="user-experience-for-automation-operator-role---automation-account"></a>Возможности для пользователя с ролью оператора службы автоматизации. Учетная запись службы автоматизации

Когда пользователь с ролью оператора службы автоматизации в учетной записи службы автоматизации просматривает учетную запись службы автоматизации, в которой он был назначен, отображаются только список модулей runbook, задания runbook и расписания, созданные в учетной записи службы автоматизации, но не отображаются их определения. Такой пользователь может запускать, останавливать, приостанавливать, возобновлять и планировать задания Runbook. У него нет доступа к другим ресурсам службы автоматизации, включая конфигурации, гибридные рабочие роли и узлы DSC.

![Нет доступа к ресурсам](media/automation-role-based-access-control/automation-10-no-access-to-resources.png)

## <a name="configure-rbac-for-runbooks"></a>Настройка RBAC для модулей runbook

Служба автоматизации Azure позволяет назначать роли RBAC для определенных модулей runbook. Для этого выполните следующий сценарий, чтобы добавить пользователя к определенному модулю runbook. Следующий сценарий может запустить администратор учетной записи службы автоматизации или администратор клиента.

```azurepowershell-interactive
$rgName = "<Resource Group Name>" # Resource Group name for the Automation Account
$automationAccountName ="<Automation Account Name>" # Name of the Automation Account
$rbName = "<Name of Runbook>" # Name of the runbook
$userId = "<User ObjectId>" # Azure Active Directory (AAD) user's ObjectId from the directory

# Gets the Automation Account resource
$aa = Get-AzureRmResource -ResourceGroupName $rgName -ResourceType "Microsoft.Automation/automationAccounts" -ResourceName $automationAccountName

# Get the Runbook resource
$rb = Get-AzureRmResource -ResourceGroupName $rgName -ResourceType "Microsoft.Automation/automationAccounts/runbooks" -ResourceName "$automationAccountName/$rbName"

# The Automation Job Operator role only needs to be ran once per user.
New-AzureRmRoleAssignment -ObjectId $userId -RoleDefinitionName "Automation Job Operator" -Scope $aa.ResourceId

# Adds the user to the Automation Runbook Operator role to the Runbook scope
New-AzureRmRoleAssignment -ObjectId $userId -RoleDefinitionName "Automation Runbook Operator" -Scope $rb.ResourceId
```

Когда сценарий будет выполнен, пользователь должен войти на портал Azure и просмотреть **все ресурсы**. Он увидите модули runbook, для которых он был назначен **оператором runbook службы автоматизации**.

![Роли RBAC для runbook на портале](./media/automation-role-based-access-control/runbook-rbac.png)

### <a name="user-experience-for-automation-operator-role---runbook"></a>Возможности для пользователя с ролью оператора службы автоматизации. Модуль runbook

Когда пользователь, которому назначена роль оператора службы автоматизации модуля runbook, просматривает назначенный ему модуль, он может только запускать его и просматривать его задания.

![Может только запускать](media/automation-role-based-access-control/automation-only-start.png)

## <a name="next-steps"></a>Дальнейшие действия

* Сведения о различных способах настройки RBAC для службы автоматизации Azure см. в статье [Управление доступом на основе ролей с помощью Azure PowerShell](../role-based-access-control/role-assignments-powershell.md).
* Дополнительные сведения о различных способах запуска модуля Runbook см. в статье [Запуск модуля Runbook в службе автоматизации Azure](automation-starting-a-runbook.md).
* Сведения о различных типах модуля Runbook см. в статье [Типы модулей Runbook в службе автоматизации Azure](automation-runbook-types.md).

