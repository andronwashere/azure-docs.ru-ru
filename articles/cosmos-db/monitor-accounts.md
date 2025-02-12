---
title: Мониторинг метрик производительности и хранения в Azure Cosmos DB
description: Узнайте, как отслеживать метрики производительности, например запросы и ошибки сервера, и метрики использования, такие как использованный объем хранилища, для учетной записи Azure Cosmos DB.
author: SnehaGunda
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/23/2019
ms.author: sngun
ms.custom: seodec18
ms.openlocfilehash: 1c12007e04ff8cd08ddb7afdf9a3c2add9638de3
ms.sourcegitcommit: 9a699d7408023d3736961745c753ca3cec708f23
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/16/2019
ms.locfileid: "68277431"
---
# <a name="monitor-performance-and-storage-metrics-in-azure-cosmos-db"></a>Мониторинг метрик производительности и хранения в Azure Cosmos DB

Вы можете отслеживать свои учетные записи Azure DocumentDB на [портале Azure](https://portal.azure.com/). Для каждой учетной записи Azure Cosmos DB доступен полный набор метрик для мониторинга пропускной способности, хранилища, доступности, задержки и согласованности.

Метрики можно просмотреть на странице учетной записи, на странице метрик или в Azure Monitor.

## <a name="view-performance-metrics-on-the-metrics-page"></a>Просмотр метрик производительности на странице "Метрики"
1. На [портале Azure](https://portal.azure.com/) щелкните **Все службы**, прокрутите до пункта **Базы данных**, щелкните **Azure Cosmos DB**, а затем выберите имя учетной записи Azure Cosmos DB, для которой требуется просмотреть метрики производительности.
2. Когда новая страница загрузится, в меню ресурсов в разделе **Мониторинг** щелкните **Метрики**.
3. После открытия страницы "Метрики" выберите коллекцию для проверки в раскрывающемся списке **Коллекции**.

   Портал Azure отображает набор доступных метрик коллекции. Обратите внимание, что метрики пропускной способности, хранилища, доступности, задержки и согласованности предоставляются на отдельных вкладках. Чтобы получить дополнительную информацию о предоставляемых метриках, щелкните двустороннюю стрелку в правом верхнем углу каждой области метрики.

   ![Снимок экрана с объективом мониторинга, отображающим набор метрик](./media/monitor-accounts/metrics-suite.png)

## <a name="view-performance-metrics-by-using-azure-monitoring"></a>Просмотр метрик производительности с помощью Azure Monitor
1. На левой панели [портала Azure](https://portal.azure.com/) щелкните **Монитор**.
2. В меню ресурсов выберите **Метрики**.
3. В окне **Монитор — метрики** в раскрывающемся меню **Группа ресурсов** выберите группу ресурсов, связанную с учетной записью Azure Cosmos DB, которую нужно отслеживать. 
4. В раскрывающемся меню **Ресурсы** выберите учетную запись базы данных для отслеживания.
5. В списке **доступных метрик** выберите метрики, которые нужно отобразить. Используйте клавишу CTRL, чтобы выбрать несколько элементов. 

## <a name="view-performance-metrics-on-the-account-page"></a>Просмотр метрик производительности на странице учетной записи
1. На [портале Azure](https://portal.azure.com/) щелкните **Все службы**, прокрутите до пункта **Базы данных**, щелкните **Azure Cosmos DB**, а затем выберите имя учетной записи Azure Cosmos DB, для которой требуется просмотреть метрики производительности.
2. В группе связанных элементов **Мониторинг** по умолчанию отображаются следующие элементы:
   
   * общее число запросов для текущего дня;
   * Используемое хранилище.
   
   ![Снимок экрана с объективом мониторинга, отображающий запросы и использование хранилища](./media/monitor-accounts/documentdb-total-requests-and-usage.png)
3. Чтобы открыть страницу подробных сведений о **метрике**, щелкните двустороннюю стрелку в правом верхнем углу плитки **Запросы**.
4. На странице **Метрика** отображаются подробные сведения обо всех запросах. 

## <a name="set-up-alerts-in-the-portal"></a>Настройка оповещений на портале

> [!NOTE]
> Классические оповещения в Azure Monitor будут прекращены 31 августа 2019. Портал Azure предлагает [средство миграции](../azure-monitor/platform/alerts-using-migration-tool.md) для переноса классических правил генерации оповещений. Однако не все классические оповещения о метриках Azure Cosmos DB можно перенести, но есть некоторые исключения. список классических оповещений, которые невозможно перенести, см. в статье [Azure Monitor](../azure-monitor/platform/alerts-understand-migration.md#cosmos-db-metrics) . 

1. На [портале Azure](https://portal.azure.com/) щелкните **Все службы**, **Azure Cosmos DB**, а затем выберите имя учетной записи Azure Cosmos DB, для которой требуется настроить оповещения метрик производительности.
2. В меню ресурсов щелкните **Правила оповещения**, чтобы открыть соответствующую страницу.  
   ![Снимок экрана с выбранной частью правил генерации оповещений](./media/monitor-accounts/madocdb10.5.png)
3. На странице **Правила оповещений** щелкните **Добавить оповещение**.  
   ![Снимок экрана: страница "Правила оповещений" с выделенной кнопкой "Добавить оповещение"](./media/monitor-accounts/madocdb11.png)
4. На странице **Добавление правила оповещения** укажите следующее:
   
   * имя настраиваемого правила оповещения;
   * описание нового правила оповещения;
   * метрику для правила оповещения;
   * условие, пороговое значение и период для активации оповещения (например, более 5 ошибок сервера за последние 15 минут);
   * следует ли оповещать администратора службы и соадминистраторов по электронной почте при срабатывании оповещения;
   * дополнительные адреса для оповещений электронной почты.  
     ![Снимок экрана со страницей "Добавление правила оповещения"](./media/monitor-accounts/madocdb12.png)

## <a name="monitor-azure-cosmos-db-programmatically"></a>Программный мониторинг Azure Cosmos DB
Доступные на портале метрики уровня учетной записи, такие как данные об использовании хранилища учетной записи и общее число запросов, недоступны через API-интерфейсы SQL. Тем не менее, можно получить данные об использовании на уровне коллекции с помощью API SQL. Чтобы получить данных на уровне коллекции, выполните следующие действия:

* Чтобы использовать REST API, [выполните запрос GET к коллекции](https://msdn.microsoft.com/library/mt489073.aspx). Квота и данные об использовании коллекции возвращаются в заголовке ответа x-ms-resource-quota и x-ms-resource-usage headers.
* Чтобы использовать пакет SDK для .NET, воспользуйтесь методом [DocumentClient.ReadDocumentCollectionAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.readdocumentcollectionasync.aspx), возвращающим объект [ResourceResponse](https://msdn.microsoft.com/library/dn799209.aspx), который содержит ряд свойств использования, например **CollectionSizeUsage**, **DatabaseUsage**, **DocumentUsage** и прочие.

Чтобы получить дополнительные метрики, используйте [пакет SDK для Azure Monitor](https://www.nuget.org/packages/Microsoft.Azure.Insights). Доступные определения метрик можно получить с помощью следующего вызова.

    https://management.azure.com/subscriptions/{SubscriptionId}/resourceGroups/{ResourceGroup}/providers/Microsoft.DocumentDb/databaseAccounts/{DocumentDBAccountName}/metricDefinitions?api-version=2015-04-08

В запросах для извлечения отдельных метрик используется следующий формат.

    https://management.azure.com/subscriptions/{SubscriptionId}/resourceGroups/{ResourceGroup}/providers/Microsoft.DocumentDb/databaseAccounts/{DocumentDBAccountName}/metrics?api-version=2015-04-08&$filter=%28name.value%20eq%20%27Total%20Requests%27%29%20and%20timeGrain%20eq%20duration%27PT5M%27%20and%20startTime%20eq%202016-06-03T03%3A26%3A00.0000000Z%20and%20endTime%20eq%202016-06-10T03%3A26%3A00.0000000Z

Дополнительные сведения см. в записи блога [Retrieving Resource Metrics via the Azure Insights API](https://blogs.msdn.microsoft.com/cloud_solution_architect/2016/02/23/retrieving-resource-metrics-via-the-azure-insights-api/) (Получение метрик ресурсов через API Azure Insights). Обратите внимание, что служба Azure Insights была переименована в Azure Monitor.  В этой записи блога используется устаревшее название.

## <a name="next-steps"></a>Следующие шаги
Дополнительные сведения о планировании ресурсов DocumentDB см. на [странице калькулятора ресурсов Azure Cosmos DB](https://www.documentdb.com/capacityplanner).

