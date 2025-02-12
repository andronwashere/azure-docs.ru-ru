---
title: Мониторинг пакетной службы Azure | Документация Майкрософт
description: Сведения о службах, метриках, журналах диагностики и других функциях мониторинга для пакетной службы Azure.
services: batch
author: laurenhughes
manager: gwallace
ms.assetid: ''
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.workload: na
ms.date: 04/05/2018
ms.author: lahugh
ms.openlocfilehash: 77fcfed125104588e27e6a0e80cdb4a92a744083
ms.sourcegitcommit: 4b431e86e47b6feb8ac6b61487f910c17a55d121
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/18/2019
ms.locfileid: "68322451"
---
# <a name="monitor-batch-solutions"></a>Мониторинг решений пакетной службы

Azure и пакетная служба предоставляют широкий набор служб, инструментов и API-интерфейсов для мониторинга решений пакетной службы. Эта обзорная статья поможет вам выбрать правильный подход к мониторингу в соответствии с вашими требованиями.

Обзор компонентов и служб Azure, доступных для мониторинга ресурсов Azure, см. в статье [Мониторинг приложений и ресурсов в Azure](../monitoring-and-diagnostics/monitoring-overview.md).

## <a name="subscription-level-monitoring"></a>Мониторинг на уровне подписки

На уровне подписки, в том числе для учетных записей пакетной службы, [журнал действий Azure](../azure-monitor/platform/activity-logs-overview.md) позволяет собирать операционные данные о событиях по [нескольким категориям](../azure-monitor/platform/activity-logs-overview.md#categories-in-the-activity-log).

В частности, для учетных записей пакетной службы в журнал действий собираются данные о создании и удалении учетных записей, а также об управлении ключами.

Для получения событий из журнала действий можно использовать портал Azure. На портале щелкните **Все службы** > **Журнал действий**. Также можно создать запрос сведений о событиях с помощью Azure CLI, командлетов PowerShell или REST API Azure Monitor. Есть возможность экспортировать журнал действий или настроить [оповещения журнала действий](../monitoring-and-diagnostics/monitoring-activity-log-alerts-new-experience.md).

## <a name="batch-account-level-monitoring"></a>Мониторинг на уровне учетной записи пакетной службы

Для мониторинга отдельных учетных записей пакетной службы можно применить [Azure Monitor](../azure-monitor/overview.md). Azure Monitor собирает [метрики](../azure-monitor/platform/data-platform-metrics.md) и (если настроено) [журналы диагностики](../azure-monitor/platform/diagnostic-logs-overview.md) для ресурсов, относящихся к уровню учетной записи пакетной службы, таких как пулы, задания и задачи. Эти данные можно собирать и использовать вручную или программными средствами, чтобы отслеживать действия в учетной записи пакетной службы и диагностировать проблемы. Дополнительные сведения см. в статье о [метриках, оповещениях и журналах для диагностики и мониторинга решений пакетной службы](batch-diagnostics.md).
 
> [!NOTE]
> По умолчанию метрики доступны в учетной записи пакетной службы за 30-дневный период со скользящей ротацией. Для сбора данных об учетной записи пакетной службы необходимо включить ведение журнала диагностики, хранение и (или) обработка которого может повлечь некоторые расходы на использование ресурсов. 

## <a name="batch-resource-monitoring"></a>Мониторинг ресурсов пакетной службы

В приложениях пакетной службы вы можете применить API-интерфейсы пакетной службы для отслеживания и запроса состояний ресурсов, таких как задания, задачи, узлы и пулы. Пример:

* [получение количества задач и вычислительных узлов с учетом состояния](batch-get-resource-counts.md);
* [эффективное создание запросов на вывод списка ресурсов пакетной службы](batch-efficient-list-queries.md);
* [создание зависимостей задач](batch-task-dependencies.md);
* использование [задач диспетчера заданий](/rest/api/batchservice/job/add#jobmanagertask);
* мониторинг [состояния задач](/rest/api/batchservice/task/list#taskstate);
* мониторинг [состояния узлов](/rest/api/batchservice/computenode/list#computenodestate);
* мониторинг [состояния пулов](/rest/api/batchservice/pool/get#poolstate);
* мониторинг [использования пулов в учетной записи](/rest/api/batchservice/pool/listusagemetrics);
* [подсчет количества узлов с определенным состоянием в пуле](/rest/api/batchservice/account/listpoolnodecounts).

## <a name="vm-performance-counters-and-application-monitoring"></a>Мониторинг счетчиков производительности и приложений для виртуальной машины

* Службу [Application Insights](../azure-monitor/app/app-insights-overview.md) можно использовать в Azure, чтобы программными средствами отслеживать доступность, производительность и потребление для заданий и задач пакетной службы. Вы можете легко получить счетчики производительности из вычислительных узлов (виртуальных машин) и произвольные данные о задачах на этих виртуальных машинах. 

  Примеры можно найти в статье [Monitor and debug an Azure Batch .NET application with Application Insights](monitor-application-insights.md) (Мониторинг и отладка приложения .NET пакетной службы с помощью Application Insights) и в [примерах кода](https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/ApplicationInsights) для нее.

  > [!NOTE]
  > Использование Application Insights может повлечь дополнительные затраты. Ознакомьтесь с [информацией о тарифных планах](https://azure.microsoft.com/pricing/details/application-insights/). 
  >

* [Batch Explorer](https://github.com/Azure/BatchExplorer) — это бесплатный автономный клиентский инструмент с множеством функций для создания, отладки и мониторинга приложений пакетной службы Azure. Скачайте [пакет установки](https://azure.github.io/BatchExplorer/) для Mac, Linux или Windows. (Необязательно.) Настройте в решении пакетной службы [отображение данных из Application Insights](https://github.com/Azure/batch-insights), таких как счетчики производительности виртуальных машин в Batch Explorer.


## <a name="next-steps"></a>Следующие шаги

* См. дополнительные сведения об [API-интерфейсах и средствах пакетной службы](batch-apis-tools.md) для сборки решений пакетной службы.
* Узнайте больше о [ведении журналов диагностики](batch-diagnostics.md) для пакетной службы.