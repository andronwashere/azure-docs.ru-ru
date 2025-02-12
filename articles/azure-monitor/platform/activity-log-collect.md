---
title: Сбор и анализ журналов действий Azure в рабочую область Log Analytics | Документация Майкрософт
description: Сбор журнала действий Azure в журналах Azure Monitor и использовать решение мониторинга для анализа и поиска журнала действий Azure во всех подписках Azure.
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: ''
ms.assetid: dbac4c73-0058-4191-a906-e59aca8e2ee0
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 05/19/2019
ms.author: bwren
ms.openlocfilehash: 5839fd40a128097e400f13acbe4fb6ef90c656b7
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "66248134"
---
# <a name="collect-and-analyze-azure-activity-logs-in-log-analytics-workspace-in-azure-monitor"></a>Сбор и анализ журналов действий Azure в рабочую область Log Analytics в Azure Monitor
[Журнал действий Azure](activity-logs-overview.md) предоставляет сведения о событиях уровня подписки, которые произошли в вашей подписке Azure. В этой статье описывается, как собирать журнал действий в рабочем пространстве Log Analytics и как использовать анализ журнала действий [решение для мониторинга](../insights/solutions.md), который предоставляет журнал запросов и представлений для анализа таких данных. 

Подключение журнала действий в рабочую область Log Analytics предоставляет следующие преимущества:

- Консолидация журнала действий из нескольких подписок Azure в одном месте для анализа.
- Store записи журнала действий дольше 90 дней.
- Сопоставляйте данные журнала действий с другими данными мониторинга, собранные Azure Monitor.
- Используйте [журнал запросов](../log-query/log-query-overview.md) для выполнения сложного анализа и получения подробных сведений о записи журнала действий.

## <a name="connect-to-log-analytics-workspace"></a>Подключиться к рабочей области Log Analytics
Журнал действий можно подключить к одной рабочей области, но одной рабочей области можно подключить к журналу действий для нескольких подписок в одном клиенте Azure. Коллекции нескольких клиентов, см. в разделе [сбора журналов действий Azure в рабочую область Log Analytics в подписках в другой Azure Active Directory клиентов](activity-log-collect-tenants.md).

Используйте следующую процедуру для подключения к рабочей области Log Analytics журнала действий:

1. Из **рабочих областей Log Analytics** меню на портале Azure выберите рабочую область для сбора в журнале действий.
1. В **источники данных рабочей области** меню рабочей области, выберите **журнал действий Azure**.
1. Щелкните подписку, которую вы хотите подключиться.

    ![Рабочие области](media/activity-log-export/workspaces.png)

1. Нажмите кнопку **Connect** для подключения к выбранной рабочей области журнала действий в подписке. Если подписка уже подключен к другой рабочей области, нажмите кнопку **Disconnect** во-первых, чтобы отключить его.

    ![Подключение рабочих областей](media/activity-log-export/connect-workspace.png)

## <a name="analyze-in-log-analytics-workspace"></a>Анализ в рабочей области Log Analytics
При подключении журнала действий в рабочую область Log Analytics, записей будет записываться в рабочую область, в таблицу с именем **AzureActivity** , которые можно получить с помощью [запроса журнала](../log-query/log-query-overview.md). Структура этой таблицы изменяется в зависимости от [категории записи журнала](activity-logs-overview.md#categories-in-the-activity-log). См. в разделе [схема событий журнала действий Azure](activity-log-schema.md) описание каждой категории.

## <a name="activity-logs-analytics-monitoring-solution"></a>Решение для мониторинга Analytics журналы действий
В решении Azure Log Analytics для мониторинга содержит несколько запросов по журналам и представлений для анализа записей журнала действий в рабочей области Log Analytics.

### <a name="install-the-solution"></a>Установка решения
Используйте процедуру, описанную в [установки решения для мониторинга](../insights/solutions.md#install-a-monitoring-solution) установка **анализ журнала действий** решения. Нет каких-либо дополнительных настроек.

### <a name="use-the-solution"></a>Использование решения
Решения для мониторинга осуществляется из **монитор** меню на портале Azure. Выберите **дополнительные** в **Insights** раздел, чтобы открыть **Обзор** страницы с помощью плитки решения. **Журналов действий Azure** плитке отображается количество число **AzureActivity** записей в рабочей области.

![Плитка "Журналы действий Azure"](media/collect-activity-logs/azure-activity-logs-tile.png)


Нажмите кнопку **журналов действий Azure** плитку, чтобы открыть **журналов действий Azure** представления. Представление состоит из частей визуализации в следующей таблице. Каждая часть содержится до 10 элементов, соответствующих критериям, частей для указанного интервала времени. Запуском запроса журнала, который возвращает все совпадающие записи, нажав кнопку **см. в разделе, все** в нижней части.

![Панель мониторинга "Журналы действий Azure"](media/collect-activity-logs/activity-log-dash.png)

| Визуализации | Описание |
| --- | --- |
| Записи журнала действий Azure | Линейчатая диаграмма верхнюю запись журнала действий Azure, показывает итоги по записи для выбранного диапазона дат и отображается список 10 основных инициаторов действий. Щелкните линейчатую диаграмму, чтобы выполнить поиск `AzureActivity`. Щелкните элемент вызывающего объекта, чтобы выполнить поиск журналов, возвращающий все записи журнала действий для этого элемента. |
| Журналы действий по состоянию | Здесь отображается кольцевая диаграмма состояния выбранного диапазона дат и список первых десяти состояние записей журнала действий Azure. Щелкните диаграмму, чтобы выполнить запрос по журналу для `AzureActivity | summarize AggregatedValue = count() by ActivityStatus`. Щелкните состояние, чтобы выполнить поиск журналов, возвращающий все записи журнала действий для этой записи о состоянии. |
| Журналы действий по ресурсу | Показывает общее число ресурсов с журналами действий и список из десяти основных ресурсов с записью подсчитывает для каждого ресурса. Щелкните область итогов, чтобы запустить поиск `AzureActivity | summarize AggregatedValue = count() by Resource` по журналам, который возвратит все доступные для решения ресурсы Azure. Щелкните ресурс, для выполнения запроса к журналу, возвращающий все записи действий для этого ресурса. |
| Журналы действий по поставщику ресурсов | Показывает общее число поставщиков ресурсов, которые создают журналы действий, а также список первые десять. Щелкните область итогов, чтобы выполнить запрос журнала для `AzureActivity | summarize AggregatedValue = count() by ResourceProvider`, который возвратит все поставщики ресурсов Azure. Щелкните поставщик ресурсов для выполнения запроса к журналу, возвращающий все записи действий для поставщика. |

## <a name="next-steps"></a>Дальнейшие действия

- Дополнительные сведения о [журнал действий](activity-logs-overview.md).
- Дополнительные сведения о [платформы данных Azure Monitor](data-platform.md).
- Используйте [журнал запросов](../log-query/log-query-overview.md) для просмотра подробных сведений из журнала действий.
