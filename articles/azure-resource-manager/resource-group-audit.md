---
title: Просмотр журналов действий Azure для наблюдения за ресурсами | Документация Майкрософт
description: Просмотр действий пользователя и ошибок с помощью журналов действий. Показывает портала Azure PowerShell, Azure CLI и REST.
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: conceptual
ms.date: 05/13/2019
ms.author: tomfitz
ms.openlocfilehash: 7ff45be4eea5c6e8ab83093847164ede0e94579a
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "65606578"
---
# <a name="view-activity-logs-to-monitor-actions-on-resources"></a>Просмотр журналов действий для отслеживания действий с ресурсами

С помощью журналов действий можно определить:

* какие операции выполнялись с ресурсами в вашей подписке;
* кто запустил операцию;
* когда была выполнена операция;
* состояние операции;
* значения других свойств, которые могут помочь в изучении операции.

Журнал действий содержит все операции записи (PUT, POST, DELETE) для ресурсов. Он не содержит операции чтения (GET). Список действий ресурсов см. в статье [Операции поставщиков ресурсов Azure Resource Manager](../role-based-access-control/resource-provider-operations.md). Журналы действий можно использовать для поиска ошибки при устранении неполадок, а также для наблюдения за тем, как пользователь в вашей организации изменяет ресурс.

Журналы действий хранятся в течение 90 дней. Вы можете запросить любой диапазон дат, при условии, что дата начала не старше 90 дней в прошлом.

Сведения из журналов действий можно получить с помощью портала, PowerShell, интерфейса командной строки Azure, API REST Insights или с помощью [библиотеки .NET для Insights](https://www.nuget.org/packages/Microsoft.Azure.Insights/).

## <a name="azure-portal"></a>Портал Azure

1. Чтобы просмотреть журналы действий на портале, выберите **Монитор**.

    ![Выбор элемента "Монитор"](./media/resource-group-audit/select-monitor.png)

1. Выберите **Журнал действий**.

    ![Выбор журнала действий](./media/resource-group-audit/select-activity-log.png)

1. Отобразится сводка последних операций. Набор фильтров по умолчанию применяется к операциям. Обратите внимание, что информация сводки включает в том, кто запустил действие и когда это произошло.

    ![Просмотр сводки последних операций](./media/resource-group-audit/audit-summary.png)

1. Чтобы быстро выполнить предварительно определенный набор фильтров, выберите **быстрый анализ данных**.

    ![Выберите быстрый анализ данных](./media/resource-group-audit/select-quick-insights.png)

1. Выберите один из способов. Например, выберите **неудачных развертываний:** еще ошибки от развертываний.

    ![Выберите неудачных развертываний](./media/resource-group-audit/select-failed-deployments.png)

1. Обратите внимание, что фильтры были изменены для выделения ошибок при развертывании за последние 24 часа. Отображаются только те операции, которые соответствуют данным фильтрам.

    ![Фильтры представлений](./media/resource-group-audit/view-filters.png)

1. Чтобы сосредоточиться на конкретных операциях, измените фильтры или примените новые. Например, на следующем рисунке показано новое значение для параметра **Временной диапазон**, а для параметра **Тип ресурса** заданы учетные записи хранения. 

    ![Установка параметров фильтра](./media/resource-group-audit/set-filter.png)

1. Если вы хотите повторить тот же запрос позже, выберите **Закрепить текущие фильтры**.

    ![Закрепление фильтров](./media/resource-group-audit/pin-filters.png)

1. Задайте имя для фильтра.

    ![Присвоение имени фильтру](./media/resource-group-audit/name-filters.png)

1. Фильтр доступен на панели мониторинга.

    ![Отображение фильтра на панели мониторинга](./media/resource-group-audit/show-dashboard.png)

1. На портале можно просмотреть изменения к ресурсу. Перейдите по умолчанию в монитор и выберите операцию, которая участвует в изменении ресурса.

    ![Выберите операцию](./media/resource-group-audit/select-operation.png)

1. Выберите **журнал (Предварительная версия) изменений** и выбрать одну из доступных операций.

    ![Выберите журнал изменений](./media/resource-group-audit/select-change-history.png)

1. Отображаются изменения в ресурсе.

    ![Показать изменения](./media/resource-group-audit/show-changes.png)

Дополнительные сведения о журнале изменений, см. в разделе [получить изменения ресурсов](../governance/resource-graph/how-to/get-resource-changes.md).

## <a name="powershell"></a>PowerShell

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Чтобы получить записи журнала, выполните команду **Get-AzLog**. Укажите дополнительные параметры, чтобы отфильтровать список записей. Если не указать время начала и окончания, возвращаются записи за последние семь дней.

```azurepowershell-interactive
Get-AzLog -ResourceGroup ExampleGroup
```

В следующем примере показано, как использовать журнал действий для анализа действий, выполненных в течение указанного времени. Даты начала и окончания указывайте в формате даты.

```azurepowershell-interactive
Get-AzLog -ResourceGroup ExampleGroup -StartTime 2019-05-05T06:00 -EndTime 2019-05-09T06:00
```

Диапазон дат, например последние 14 дней, также можно указать с помощью функций даты.

```azurepowershell-interactive
Get-AzLog -ResourceGroup ExampleGroup -StartTime (Get-Date).AddDays(-14)
```

Можно просмотреть действия, выполненные конкретным пользователем.

```azurepowershell-interactive
Get-AzLog -ResourceGroup ExampleGroup -StartTime (Get-Date).AddDays(-14) -Caller someone@contoso.com
```

Можно применить фильтр для невыполненных операций.

```azurepowershell-interactive
Get-AzLog -ResourceGroup ExampleGroup -Status Failed
```

Можно получить сведения об одной ошибке, просмотрев сообщение о состоянии для ее записи.

```azurepowershell-interactive
(Get-AzLog -ResourceGroup ExampleGroup -Status Failed).Properties.Content.statusMessage | ConvertFrom-Json
```

Вы можете выбрать определенные значения, чтобы ограничить возвращаемые данные.

```azurepowershell-interactive
Get-AzLog -ResourceGroupName ExampleGroup | Format-table EventTimeStamp, Caller, @{n='Operation'; e={$_.OperationName.value}}, @{n='Status'; e={$_.Status.value}}, @{n='SubStatus'; e={$_.SubStatus.LocalizedValue}}
```

В зависимости от указанного времени начала приведенные выше команды могут вернуть длинный список операций для группы ресурсов. Чтобы найти в результатах нужную информацию, отфильтруйте их, используя условия поиска. Например, вы можете отфильтровать их по типу операции.

```azurepowershell-interactive
Get-AzLog -ResourceGroup ExampleGroup | Where-Object {$_.OperationName.value -eq "Microsoft.Resources/deployments/write"}
```

График ресурсов можно использовать, чтобы просмотреть журнал изменений для ресурса. Дополнительные сведения см. в разделе [получить изменения ресурсов](../governance/resource-graph/how-to/get-resource-changes.md).

## <a name="azure-cli"></a>Инфраструктура CLI Azure

Чтобы получить записи журнала, выполните команду [az monitor activity-log list](/cli/azure/monitor/activity-log#az-monitor-activity-log-list) со смещением, чтобы указать на диапазон времени.

```azurecli-interactive
az monitor activity-log list --resource-group ExampleGroup --offset 7d
```

В следующем примере показано, как использовать журнал действий для анализа действий, выполненных в течение указанного времени. Даты начала и окончания указывайте в формате даты.

```azurecli-interactive
az monitor activity-log list -g ExampleGroup --start-time 2019-05-01 --end-time 2019-05-15
```

Можно найти действия, выполненные конкретным пользователем, даже для группы ресурсов, которой больше не существует.

```azurecli-interactive
az monitor activity-log list -g ExampleGroup --caller someone@contoso.com --offset 5d
```

Можно применить фильтр для невыполненных операций.

```azurecli-interactive
az monitor activity-log list -g ExampleGroup --status Failed --offset 1d
```

Можно получить сведения об одной ошибке, просмотрев сообщение о состоянии для ее записи.

```azurecli-interactive
az monitor activity-log list -g ExampleGroup --status Failed --offset 1d --query [].properties.statusMessage
```

Вы можете выбрать определенные значения, чтобы ограничить возвращаемые данные.

```azurecli-interactive
az monitor activity-log list -g ExampleGroup --offset 1d --query '[].{Operation: operationName.value, Status: status.value, SubStatus: subStatus.localizedValue}'
```

В зависимости от указанного времени начала приведенные выше команды могут вернуть длинный список операций для группы ресурсов. Чтобы найти в результатах нужную информацию, отфильтруйте их, используя условия поиска. Например, вы можете отфильтровать их по типу операции.

```azurecli-interactive
az monitor activity-log list -g ExampleGroup --offset 1d --query "[?operationName.value=='Microsoft.Storage/storageAccounts/write']"
```

График ресурсов можно использовать, чтобы просмотреть журнал изменений для ресурса. Дополнительные сведения см. в разделе [получить изменения ресурсов](../governance/resource-graph/how-to/get-resource-changes.md).

## <a name="rest-api"></a>REST API

Операции REST для работы с журналом действий включены в интерфейс [REST API Insights](/rest/api/monitor/). Получение событий журнала действий описано в статье [Список событий управления в подписке](/rest/api/monitor/activitylogs).

## <a name="next-steps"></a>Дальнейшие действия

* Чтобы получить больше информации о действиях в вашей подписке, можно использовать журналы аудита Azure совместно с Power BI. Дополнительные сведения см. в записи блога [View and analyze Azure Audit Logs in Power BI and more](https://azure.microsoft.com/blog/analyze-azure-audit-logs-in-powerbi-more/) (Журналы аудита Azure в Power BI: просмотр, анализ и другие возможности).
* Дополнительные сведения о настройке политик безопасности см. в статье о [контроле доступа на основе ролей Azure](../role-based-access-control/role-assignments-portal.md).
* Чтобы узнать о командах для просмотра операций развертывания, ознакомьтесь с разделом [View deployment operations with Azure Resource Manager](resource-manager-deployment-operations.md) (Просмотр операций развертывания с помощью Azure Resource Manager).
* Вы можете запретить всем пользователям операции удаления для определенного ресурса, как описано в статье [Блокировка ресурсов с помощью Azure Resource Manager](resource-group-lock-resources.md).
* Список операций, доступных для каждого поставщика Microsoft Azure Resource Manager, см. в статье [Операции поставщиков ресурсов Azure Resource Manager](../role-based-access-control/resource-provider-operations.md).
