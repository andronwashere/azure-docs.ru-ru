---
title: Привязки Azure Cosmos DB для службы "Функции" версии 1.х
description: Узнайте, как использовать триггеры и привязки Azure Cosmos DB в службе "Функции Azure".
services: functions
author: craigshoemaker
ms.author: cshoe
manager: gwallace
keywords: функции azure, функции, обработка событий, динамические вычисления, независимая архитектура
ms.service: azure-functions
ms.topic: reference
ms.date: 11/21/2017
ms.custom: seodec18
ms.openlocfilehash: 0e6782c48543723438ee332313de268117dee3e9
ms.sourcegitcommit: 9b80d1e560b02f74d2237489fa1c6eb7eca5ee10
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/01/2019
ms.locfileid: "67480723"
---
# <a name="azure-cosmos-db-bindings-for-azure-functions-1x"></a>Привязки Azure Cosmos DB для службы "Функции Azure" версии 1.х

> [!div class="op_single_selector" title1="Выберите версию среды выполнения функций Azure, которую вы используете: "]
> * [Версия 1](functions-bindings-cosmosdb.md)
> * [Версия 2](functions-bindings-cosmosdb-v2.md)

В этой статье описывается использование привязок [Azure Cosmos DB](../cosmos-db/serverless-computing-database.md) в службе "Функции Azure". Служба "Функции Azure" поддерживает привязки триггера, а также входные и выходные привязки для Azure Cosmos DB.

> [!NOTE]
> Эта статья предназначена для службы "Функции Azure" версии 1.x. Сведения об использовании этих привязок в Функциях 2.x см. [здесь](functions-bindings-cosmosdb-v2.md).
>
>Эта привязка называлась DocumentDB. В Функциях версии 1.x только триггер был переименован в Cosmos DB. Входная и выходная привязка, а также пакет NuGet сохраняют имя DocumentDB.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

> [!NOTE]
> Привязки Azure Cosmos DB поддерживаются только для использования с API SQL. Для всех других API Azure Cosmos DB доступ к базе данных из функции должен осуществляться с использованием статического клиента для API, включая [API базы данны Azure Cosmos для MongoDB](../cosmos-db/mongodb-introduction.md), [API Cassandra](../cosmos-db/cassandra-introduction.md), [API Gremlin](../cosmos-db/graph-introduction.md) и [API таблиц](../cosmos-db/table-introduction.md).

## <a name="packages---functions-1x"></a>Пакеты – Функции 1.x

Привязки Azure Cosmos DB для Функций 1.x доступны в пакете NuGet [Microsoft.Azure.WebJobs.Extensions.DocumentDB](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.DocumentDB) версии 1.х. Исходный код для привязок находится в репозитории GitHub [azure-webjobs-sdk-extensions](https://github.com/Azure/azure-webjobs-sdk-extensions/tree/v2.x/src/WebJobs.Extensions.DocumentDB).

[!INCLUDE [functions-package](../../includes/functions-package.md)]

## <a name="trigger"></a>Триггер

Триггер Azure Cosmos DB использует [канал изменений Azure Cosmos DB](../cosmos-db/change-feed.md) для ожидания вставок и обновлений в разных разделах. На канале изменений публикуются вставки и обновления, а не удаления.

## <a name="trigger---example"></a>Пример триггера

Языковой пример см. в разделах:

* [C#](#trigger---c-example)
* [Скрипт C# (CSX)](#trigger---c-script-example)
* [JavaScript](#trigger---javascript-example)

[Пропустить примеры триггеров](#trigger---attributes)

### <a name="trigger---c-example"></a>Пример C# в триггере

В следующем примере показана [функция C#](functions-dotnet-class-library.md), вызываемая при вставке или обновлении в указанной базе данных и коллекции.

```cs
using Microsoft.Azure.Documents;
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Host;
using System.Collections.Generic;

namespace CosmosDBSamplesV1
{
    public static class CosmosTrigger
    {
        [FunctionName("CosmosTrigger")]
        public static void Run([CosmosDBTrigger(
            databaseName: "ToDoItems",
            collectionName: "Items",
            ConnectionStringSetting = "CosmosDBConnection",
            LeaseCollectionName = "leases",
            CreateLeaseCollectionIfNotExists = true)]IReadOnlyList<Document> documents,
            TraceWriter log)
        {
            if (documents != null && documents.Count > 0)
            {
                log.Info($"Documents modified: {documents.Count}");
                log.Info($"First document Id: {documents[0].Id}");
            }
        }
    }
}
```

[Пропустить примеры триггеров](#trigger---attributes)

### <a name="trigger---c-script-example"></a>Пример скрипта C# в триггере

В следующем примере показаны привязка триггера Cosmos DB в файле *function.json* и [функция сценария C#](functions-reference-csharp.md), которая использует эту привязку. При изменении записей в Cosmos DB функция записывает сообщения в журнале.

Данные привязки в файле *function.json*:

```json
{
    "type": "cosmosDBTrigger",
    "name": "documents",
    "direction": "in",
    "leaseCollectionName": "leases",
    "connectionStringSetting": "<connection-app-setting>",
    "databaseName": "Tasks",
    "collectionName": "Items",
    "createLeaseCollectionIfNotExists": true
}
```

Ниже приведен код скрипта C#.

```cs
    #r "Microsoft.Azure.Documents.Client"
    
    using System;
    using Microsoft.Azure.Documents;
    using System.Collections.Generic;
    

    public static void Run(IReadOnlyList<Document> documents, TraceWriter log)
    {
        log.Info("Documents modified " + documents.Count);
        log.Info("First document Id " + documents[0].Id);
    }
```

[Пропустить примеры триггеров](#trigger---attributes)

### <a name="trigger---javascript-example"></a>Пример JavaScript в триггере

В следующем примере показаны привязка триггера Cosmos DB в файле *function.json* и [функция JavaScript](functions-reference-node.md), которая использует эту привязку. При изменении записей в Cosmos DB функция записывает сообщения в журнале.

Данные привязки в файле *function.json*:

```json
{
    "type": "cosmosDBTrigger",
    "name": "documents",
    "direction": "in",
    "leaseCollectionName": "leases",
    "connectionStringSetting": "<connection-app-setting>",
    "databaseName": "Tasks",
    "collectionName": "Items",
    "createLeaseCollectionIfNotExists": true
}
```

Ниже показан код JavaScript.

```javascript
    module.exports = function (context, documents) {
        context.log('First document Id modified : ', documents[0].id);

        context.done();
    }
```

## <a name="trigger---attributes"></a>Атрибуты триггера

В [библиотеках классов C#](functions-dotnet-class-library.md) используйте атрибут [CosmosDBTrigger](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.CosmosDB/Trigger/CosmosDBTriggerAttribute.cs).

Конструктор атрибута принимает имя базы данных и имя коллекции. Дополнительные сведения о параметрах и других свойствах, которые можно настроить, см. в разделе [Конфигурация триггера](#trigger---configuration). Ниже приведен пример атрибута `CosmosDBTrigger` в сигнатуре метода:

```csharp
    [FunctionName("DocumentUpdates")]
    public static void Run(
        [CosmosDBTrigger("database", "collection", ConnectionStringSetting = "myCosmosDB")]
    IReadOnlyList<Document> documents,
        TraceWriter log)
    {
        ...
    }
```

Полный пример см. в разделе [Пример C# в триггере](#trigger---c-example).

## <a name="trigger---configuration"></a>Конфигурация триггера

В следующей таблице описываются свойства конфигурации привязки, которые задаются в файле *function.json* и атрибуте `CosmosDBTrigger`.

|свойство function.json | Свойство атрибута |Описание|
|---------|---------|----------------------|
|**type** || Нужно задать значение `cosmosDBTrigger`. |
|**direction** || Нужно задать значение `in`. Этот параметр задается автоматически при создании триггера на портале Azure. |
|**name** || Имя переменной, используемое в коде функции, представляющей список документов с изменениями. |
|**connectionStringSetting**|**ConnectionStringSetting** | Имя параметра приложения, содержащего строку подключения, используемую для подключения к отслеживаемой учетной записи Azure Cosmos DB. |
|**databaseName**|**DatabaseName**  | Имя базы данных Azure Cosmos DB с отслеживаемой коллекцией. |
|**collectionName** |**CollectionName** | Имя отслеживаемой коллекции. |
|**leaseConnectionStringSetting** | **LeaseConnectionStringSetting** | (Необязательно.) Имя параметра приложения, содержащее строку подключения для службы, содержащей коллекцию аренды. Если значение не задано, используется значение `connectionStringSetting`. Этот параметр задается автоматически при создании привязки на портале. Строка подключения для коллекции аренд должна иметь разрешения на запись.|
|**leaseDatabaseName** |**LeaseDatabaseName** | (Необязательно.) Имя базы данных, в которой содержится коллекция, используемая для хранения аренд. Если значение не задано, используется значение параметра `databaseName`. Этот параметр задается автоматически при создании привязки на портале. |
|**leaseCollectionName** | **LeaseCollectionName** | (Необязательно.) Имя коллекции, используемой для хранения аренд. Если значение не задано, используется значение `leases`. |
|**createLeaseCollectionIfNotExists** | **CreateLeaseCollectionIfNotExists** | (Необязательно.) Если задано значение `true`, коллекция аренд создается автоматически, если она не создана. По умолчанию используется значение `false`. |
|**leasesCollectionThroughput**| **LeasesCollectionThroughput**| (Необязательно.) Определяет количество единиц запросов для назначения при создании коллекции аренд. Этот параметр используется, только если для `createLeaseCollectionIfNotExists` задано значение `true`. Этот параметр задается автоматически при создании привязки с помощью портала.
|**leaseCollectionPrefix**| **LeaseCollectionPrefix**| (Дополнительно) Если параметр задан, то он добавляет префикс к созданной аренде в коллекции аренды для этой функции, позволяя двум разным функциям Azure совместно эффективно использовать коллекцию аренд с помощью различных префиксов.
|**feedPollDelay**| **FeedPollDelay**| (Дополнительно) Если параметр задан, то он определяет задержку между опросами секции на наличие новых изменений в веб-канале, после того как все текущие изменения будут утеряны (в миллисекундах). По умолчанию это 5000 (5 секунд).
|**leaseAcquireInterval**| **LeaseAcquireInterval**| (Дополнительно) Если параметр задан, то он определяет интервал для запуска задачи вычисления при условии равномерности распределения секций между известными экземплярами узла (в миллисекундах). По умолчанию это 13000 (13 секунд).
|**leaseExpirationInterval**| **LeaseExpirationInterval**| (Дополнительно) Если параметр задан, то он определяет интервал, за который берется аренда, представляющая секцию (в миллисекундах). Если аренда не будет обновлена в течение этого интервала, это приведет к ее истечению и владение секцией перейдет к другому экземпляру. По умолчанию это 60000 (60 секунд).
|**leaseRenewInterval**| **LeaseRenewInterval**| (Дополнительно) Если параметр задан, то он определяет интервал продления для всех аренд в разделах, которые на данный момент занятые экземплярами (в миллисекундах). По умолчанию это 17000 (17 секунд).
|**checkpointFrequency**| **CheckpointFrequency**| (Дополнительно) Если параметр задан, то он определяет интервал между контрольными точками аренды (в миллисекундах). После каждого вызова функции всегда используется значение по умолчанию.
|**maxItemsPerInvocation**| **MaxItemsPerInvocation**| (Дополнительно) Если параметр задан, то он устанавливает максимально получаемое количество элементов за вызов функции.
|**startFromBeginning**| **StartFromBeginning**| (Необязательно.) Если параметр задан, значит, триггер может начать чтение изменений с начала журнала коллекции, а не с текущего момента времени. Это возможно только при первом запуске триггера, так как при последующих запусках контрольные точки уже будут сохранены. Если установить значение `true` для этого параметра при уже созданных арендах, этот параметр не будет иметь никакого эффекта.

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="trigger---usage"></a>Использование триггера

Триггеру требуется вторая коллекция, которая используется для хранения _аренды_ в разделах. Отслеживаемая коллекция и та, в которой содержатся аренды, должны быть доступны для успешной работы триггера.

>[!IMPORTANT]
> Если несколько функций настроены на использование триггера Cosmos DB для одной и той же коллекции, то каждая из функций должна использовать выделенную коллекцию аренды или указывать на другой префикс `LeaseCollectionPrefix` для каждой функции. В противном случае активируется только одна из функций. Сведения о префиксе см. в разделе с [конфигурацией](#trigger---configuration).

Триггер не указывает, был ли документ обновлен или вставлен. Вместо этого он представляет сам документ. Если операции обновления и вставки необходимо обрабатывать по-разному, это можно сделать, внедрив поля меток времени для операций обновления и вставки.

## <a name="input"></a>Вход

Входная привязка Azure Cosmos DB извлекает один или несколько документов из Azure Cosmos DB и передает их входному параметру функции через API SQL. Идентификатор документа или параметры запроса можно определить по триггеру, который вызывает функцию.

## <a name="input---examples"></a>Примеры входных данных

См. примеры (для разных языков), в которых для считывания одного документа указывается значение идентификатора:

* [C#](#input---c-examples)
* [Скрипт C# (CSX)](#input---c-script-examples)
* [JavaScript](#input---javascript-examples)
* [F#](#input---f-examples)

[Пропустить примеры входных данных](#input---attributes)

### <a name="input---c-examples"></a>Примеры входных данных C#

В этом разделе содержатся следующие примеры:

* [Триггер очереди, поисковый идентификатор из JSON](#queue-trigger-look-up-id-from-json-c)
* [Триггер HTTP, поисковый идентификатор из строки запроса](#http-trigger-look-up-id-from-query-string-c)
* [Триггер HTTP, поисковый идентификатор из данных маршрута](#http-trigger-look-up-id-from-route-data-c)
* [Триггер HTTP, поисковый идентификатор из данных маршрута, используется SqlQuery](#http-trigger-look-up-id-from-route-data-using-sqlquery-c)
* [Триггер HTTP, получение нескольких документов, используется SqlQuery](#http-trigger-get-multiple-docs-using-sqlquery-c)
* [Триггер HTTP, получение нескольких документов, используется DocumentClient](#http-trigger-get-multiple-docs-using-documentclient-c)

В примерах используется `ToDoItem` простого типа:

```cs
namespace CosmosDBSamplesV1
{
    public class ToDoItem
    {
        public string Id { get; set; }
        public string Description { get; set; }
    }
}
```

[Пропустить примеры входных данных](#input---attributes)

#### <a name="queue-trigger-look-up-id-from-json-c"></a>Триггер очереди, поисковый идентификатор из JSON (C#)

В следующем примере показана [функция C#](functions-dotnet-class-library.md), которая получает один документ. Функция активируется сообщением из очереди, который содержит объект JSON. Очередь триггера анализирует JSON в объект с именем `ToDoItemLookup`, который содержит идентификатор для поиска. Этот идентификатор используется для получения `ToDoItem` документа из указанной базы данных и коллекции.

```cs
namespace CosmosDBSamplesV1
{
    public class ToDoItemLookup
    {
        public string ToDoItemId { get; set; }
    }
}
```

```cs
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Host;

namespace CosmosDBSamplesV1
{
    public static class DocByIdFromJSON
    {
        [FunctionName("DocByIdFromJSON")]
        public static void Run(
            [QueueTrigger("todoqueueforlookup")] ToDoItemLookup toDoItemLookup,
            [DocumentDB(
                databaseName: "ToDoItems",
                collectionName: "Items",
                ConnectionStringSetting = "CosmosDBConnection",
                Id = "{ToDoItemId}")]ToDoItem toDoItem,
            TraceWriter log)
        {
            log.Info($"C# Queue trigger function processed Id={toDoItemLookup?.ToDoItemId}");

            if (toDoItem == null)
            {
                log.Info($"ToDo item not found");
            }
            else
            {
                log.Info($"Found ToDo item, Description={toDoItem.Description}");
            }
        }
    }
}
```

[Пропустить примеры входных данных](#input---attributes)

#### <a name="http-trigger-look-up-id-from-query-string-c"></a>Триггер HTTP, поисковый идентификатор из строки запроса (C#)

В следующем примере показана [функция C#](functions-dotnet-class-library.md), которая получает один документ. Функция инициируется HTTP-запросом, который в строке запроса указывает идентификатор для поиска. Этот идентификатор используется для получения `ToDoItem` документа из указанной базы данных и коллекции.

```cs
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Extensions.Http;
using Microsoft.Azure.WebJobs.Host;
using System.Net;
using System.Net.Http;

namespace CosmosDBSamplesV1
{
    public static class DocByIdFromQueryString
    {
        [FunctionName("DocByIdFromQueryString")]
        public static HttpResponseMessage Run(
            [HttpTrigger(AuthorizationLevel.Anonymous, "get", "post", Route = null)]HttpRequestMessage req,
            [DocumentDB(
                databaseName: "ToDoItems",
                collectionName: "Items",
                ConnectionStringSetting = "CosmosDBConnection",
                Id = "{Query.id}")] ToDoItem toDoItem,
            TraceWriter log)
        {
            log.Info("C# HTTP trigger function processed a request.");
            if (toDoItem == null)
            {
                log.Info($"ToDo item not found");
            }
            else
            {
                log.Info($"Found ToDo item, Description={toDoItem.Description}");
            }
            return req.CreateResponse(HttpStatusCode.OK);
        }
    }
}
```

[Пропустить примеры входных данных](#input---attributes)

#### <a name="http-trigger-look-up-id-from-route-data-c"></a>Триггер HTTP, поисковый идентификатор из данных маршрута (C#)

В следующем примере показана [функция C#](functions-dotnet-class-library.md), которая получает один документ. Функция инициируется HTTP-запросом, в котором с помощью данных маршрута указывается идентификатор для поиска. Этот идентификатор используется для получения `ToDoItem` документа из указанной базы данных и коллекции.

```cs
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Extensions.Http;
using Microsoft.Azure.WebJobs.Host;
using System.Net;
using System.Net.Http;

namespace CosmosDBSamplesV1
{
    public static class DocByIdFromRouteData
    {
        [FunctionName("DocByIdFromRouteData")]
        public static HttpResponseMessage Run(
            [HttpTrigger(
                AuthorizationLevel.Anonymous, "get", "post",
                Route = "todoitems/{id}")]HttpRequestMessage req,
            [DocumentDB(
                databaseName: "ToDoItems",
                collectionName: "Items",
                ConnectionStringSetting = "CosmosDBConnection",
                Id = "{id}")] ToDoItem toDoItem,
            TraceWriter log)
        {
            log.Info("C# HTTP trigger function processed a request.");

            if (toDoItem == null)
            {
                log.Info($"ToDo item not found");
            }
            else
            {
                log.Info($"Found ToDo item, Description={toDoItem.Description}");
            }
            return req.CreateResponse(HttpStatusCode.OK);
        }
    }
}
```

[Пропустить примеры входных данных](#input---attributes)

#### <a name="http-trigger-look-up-id-from-route-data-using-sqlquery-c"></a>Триггер HTTP, поисковый идентификатор из данных маршрута, используется SqlQuery (C#)

В следующем примере показана [функция C#](functions-dotnet-class-library.md), которая получает один документ. Функция инициируется HTTP-запросом, в котором с помощью данных маршрута указывается идентификатор для поиска. Этот идентификатор используется для получения `ToDoItem` документа из указанной базы данных и коллекции.

```cs
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Extensions.Http;
using Microsoft.Azure.WebJobs.Host;
using System.Collections.Generic;
using System.Net;
using System.Net.Http;

namespace CosmosDBSamplesV1
{
    public static class DocByIdFromRouteDataUsingSqlQuery
    {
        [FunctionName("DocByIdFromRouteDataUsingSqlQuery")]
        public static HttpResponseMessage Run(
            [HttpTrigger(AuthorizationLevel.Anonymous, "get", "post",
                Route = "todoitems2/{id}")]HttpRequestMessage req,
            [DocumentDB(
                databaseName: "ToDoItems",
                collectionName: "Items",
                ConnectionStringSetting = "CosmosDBConnection",
                SqlQuery = "select * from ToDoItems r where r.id = {id}")] IEnumerable<ToDoItem> toDoItems,
            TraceWriter log)
        {
            log.Info("C# HTTP trigger function processed a request.");
            foreach (ToDoItem toDoItem in toDoItems)
            {
                log.Info(toDoItem.Description);
            }
            return req.CreateResponse(HttpStatusCode.OK);
        }
    }
}
```

[Пропустить примеры входных данных](#input---attributes)

#### <a name="http-trigger-get-multiple-docs-using-sqlquery-c"></a>Триггер HTTP, получение нескольких документов, используется SqlQuery (C#)

В следующем примере показана [функция C#](functions-dotnet-class-library.md), которая получает список документов. Функция активируется с помощью HTTP-запроса. Запрос указывается в свойстве атрибута `SqlQuery`.

```cs
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Extensions.Http;
using Microsoft.Azure.WebJobs.Host;
using System.Collections.Generic;
using System.Net;
using System.Net.Http;

namespace CosmosDBSamplesV1
{
    public static class DocsBySqlQuery
    {
        [FunctionName("DocsBySqlQuery")]
        public static HttpResponseMessage Run(
            [HttpTrigger(AuthorizationLevel.Anonymous, "get", "post", Route = null)]
                HttpRequestMessage req,
            [DocumentDB(
                databaseName: "ToDoItems",
                collectionName: "Items",
                ConnectionStringSetting = "CosmosDBConnection",
                SqlQuery = "SELECT top 2 * FROM c order by c._ts desc")]
                IEnumerable<ToDoItem> toDoItems,
            TraceWriter log)
        {
            log.Info("C# HTTP trigger function processed a request.");
            foreach (ToDoItem toDoItem in toDoItems)
            {
                log.Info(toDoItem.Description);
            }
            return req.CreateResponse(HttpStatusCode.OK);
        }
    }
}
```

[Пропустить примеры входных данных](#input---attributes)

#### <a name="http-trigger-get-multiple-docs-using-documentclient-c"></a>Триггер HTTP, получение нескольких документов, используется DocumentClient (C#)

В следующем примере показана [функция C#](functions-dotnet-class-library.md), которая получает список документов. Функция активируется с помощью HTTP-запроса. Код использует экземпляр `DocumentClient`, предоставленный привязкой Azure Cosmos DB для считывания списка документов. Экземпляр `DocumentClient` может также использоваться для операций записи.

```cs
using Microsoft.Azure.Documents.Client;
using Microsoft.Azure.Documents.Linq;
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Extensions.Http;
using Microsoft.Azure.WebJobs.Host;
using System;
using System.Linq;
using System.Net;
using System.Net.Http;
using System.Threading.Tasks;

namespace CosmosDBSamplesV1
{
    public static class DocsByUsingDocumentClient
    {
        [FunctionName("DocsByUsingDocumentClient")]
        public static async Task<HttpResponseMessage> Run(
            [HttpTrigger(AuthorizationLevel.Anonymous, "get", "post", Route = null)]HttpRequestMessage req,
            [DocumentDB(
                databaseName: "ToDoItems",
                collectionName: "Items",
                ConnectionStringSetting = "CosmosDBConnection")] DocumentClient client,
            TraceWriter log)
        {
            log.Info("C# HTTP trigger function processed a request.");

            Uri collectionUri = UriFactory.CreateDocumentCollectionUri("ToDoItems", "Items");
            string searchterm = req.GetQueryNameValuePairs()
                .FirstOrDefault(q => string.Compare(q.Key, "searchterm", true) == 0)
                .Value;

            if (searchterm == null)
            {
                return req.CreateResponse(HttpStatusCode.NotFound);
            }

            log.Info($"Searching for word: {searchterm} using Uri: {collectionUri.ToString()}");
            IDocumentQuery<ToDoItem> query = client.CreateDocumentQuery<ToDoItem>(collectionUri)
                .Where(p => p.Description.Contains(searchterm))
                .AsDocumentQuery();

            while (query.HasMoreResults)
            {
                foreach (ToDoItem result in await query.ExecuteNextAsync())
                {
                    log.Info(result.Description);
                }
            }
            return req.CreateResponse(HttpStatusCode.OK);
        }
    }
}
```

[Пропустить примеры входных данных](#input---attributes)

### <a name="input---c-script-examples"></a>Примеры входных данных скрипта C#

В этом разделе содержатся следующие примеры:

* [Триггер очереди, поисковый идентификатор из строки](#queue-trigger-look-up-id-from-string-c-script)
* [Триггер очереди, получение нескольких документов, используется SqlQuery](#queue-trigger-get-multiple-docs-using-sqlquery-c-script)
* [Триггер HTTP, поисковый идентификатор из строки запроса](#http-trigger-look-up-id-from-query-string-c-script)
* [Триггер HTTP, поисковый идентификатор из данных маршрута](#http-trigger-look-up-id-from-route-data-c-script)
* [Триггер HTTP, получение нескольких документов, используется SqlQuery](#http-trigger-get-multiple-docs-using-sqlquery-c-script)
* [Триггер HTTP, получение нескольких документов, используется DocumentClient](#http-trigger-get-multiple-docs-using-documentclient-c-script)

В примерах триггера HTTP используется `ToDoItem` простого типа:

```cs
namespace CosmosDBSamplesV1
{
    public class ToDoItem
    {
        public string Id { get; set; }
        public string Description { get; set; }
    }
}
```

[Пропустить примеры входных данных](#input---attributes)

#### <a name="queue-trigger-look-up-id-from-string-c-script"></a>Триггер очереди, поисковый идентификатор из строки (скрипт C#)

В следующем примере показаны входная привязка Cosmos DB в файле *function.json* и [функция сценария C#](functions-reference-csharp.md), которая использует эту привязку. Функция считывает один документ и обновляет текстовое значение в документе.

Данные привязки в файле *function.json*:

```json
{
    "name": "inputDocument",
    "type": "documentDB",
    "databaseName": "MyDatabase",
    "collectionName": "MyCollection",
    "id" : "{queueTrigger}",
    "partitionKey": "{partition key value}",
    "connection": "MyAccount_COSMOSDB",
    "direction": "in"
}
```
В разделе [Конфигурация](#input---configuration) описываются эти свойства.

Ниже приведен код скрипта C#.

```cs
    using System;

    // Change input document contents using Azure Cosmos DB input binding
    public static void Run(string myQueueItem, dynamic inputDocument)
    {
        inputDocument.text = "This has changed.";
    }
```

[Пропустить примеры входных данных](#input---attributes)

#### <a name="queue-trigger-get-multiple-docs-using-sqlquery-c-script"></a>Триггер очереди, получение нескольких документов, используется SqlQuery (скрипт C#)

В следующем примере показана входная привязка Azure Cosmos DB в файле *function.json* и [функция сценария C#](functions-reference-csharp.md), которая использует эту привязку. Функция извлекает несколько документов, указанных SQL-запросом, используя триггер очереди для настройки параметров запроса.

Триггер очереди предоставляет параметр `departmentId`. Сообщение из очереди `{ "departmentId" : "Finance" }` возвратит все записи для финансового отдела.

Данные привязки в файле *function.json*:

```json
{
    "name": "documents",
    "type": "documentdb",
    "direction": "in",
    "databaseName": "MyDb",
    "collectionName": "MyCollection",
    "sqlQuery": "SELECT * from c where c.departmentId = {departmentId}",
    "connection": "CosmosDBConnection"
}
```

В разделе [Конфигурация](#input---configuration) описываются эти свойства.

Ниже приведен код скрипта C#.

```csharp
    public static void Run(QueuePayload myQueueItem, IEnumerable<dynamic> documents)
    {
        foreach (var doc in documents)
        {
            // operate on each document
        }
    }

    public class QueuePayload
    {
        public string departmentId { get; set; }
    }
```

[Пропустить примеры входных данных](#input---attributes)

#### <a name="http-trigger-look-up-id-from-query-string-c-script"></a>Триггер HTTP, поисковый идентификатор из строки запроса (скрипт C#)

В следующем примере показана [функция скрипта C#](functions-reference-csharp.md), которая получает один документ. Функция инициируется HTTP-запросом, который в строке запроса указывает идентификатор для поиска. Этот идентификатор используется для получения `ToDoItem` документа из указанной базы данных и коллекции.

Ниже показан файл *function.json*.

```json
{
  "bindings": [
    {
      "authLevel": "anonymous",
      "name": "req",
      "type": "httpTrigger",
      "direction": "in",
      "methods": [
        "get",
        "post"
      ]
    },
    {
      "name": "$return",
      "type": "http",
      "direction": "out"
    },
    {
      "type": "documentDB",
      "name": "toDoItem",
      "databaseName": "ToDoItems",
      "collectionName": "Items",
      "connection": "CosmosDBConnection",
      "direction": "in",
      "Id": "{Query.id}"
    }
  ],
  "disabled": true
}
```

Ниже приведен код скрипта C#.

```cs
using System.Net;

public static HttpResponseMessage Run(HttpRequestMessage req, ToDoItem toDoItem, TraceWriter log)
{
    log.Info("C# HTTP trigger function processed a request.");

    if (toDoItem == null)
    {
        log.Info($"ToDo item not found");
    }
    else
    {
        log.Info($"Found ToDo item, Description={toDoItem.Description}");
    }
    return req.CreateResponse(HttpStatusCode.OK);
}
```

[Пропустить примеры входных данных](#input---attributes)

#### <a name="http-trigger-look-up-id-from-route-data-c-script"></a>Триггер HTTP, поисковый идентификатор из данных маршрута (скрипт C#)

В следующем примере показана [функция скрипта C#](functions-reference-csharp.md), которая получает один документ. Функция инициируется HTTP-запросом, в котором с помощью данных маршрута указывается идентификатор для поиска. Этот идентификатор используется для получения `ToDoItem` документа из указанной базы данных и коллекции.

Ниже показан файл *function.json*.

```json
{
  "bindings": [
    {
      "authLevel": "anonymous",
      "name": "req",
      "type": "httpTrigger",
      "direction": "in",
      "methods": [
        "get",
        "post"
      ],
      "route":"todoitems/{id}"
    },
    {
      "name": "$return",
      "type": "http",
      "direction": "out"
    },
    {
      "type": "documentDB",
      "name": "toDoItem",
      "databaseName": "ToDoItems",
      "collectionName": "Items",
      "connection": "CosmosDBConnection",
      "direction": "in",
      "Id": "{id}"
    }
  ],
  "disabled": false
}
```

Ниже приведен код скрипта C#.

```cs
using System.Net;

public static HttpResponseMessage Run(HttpRequestMessage req, ToDoItem toDoItem, TraceWriter log)
{
    log.Info("C# HTTP trigger function processed a request.");

    if (toDoItem == null)
    {
        log.Info($"ToDo item not found");
    }
    else
    {
        log.Info($"Found ToDo item, Description={toDoItem.Description}");
    }
    return req.CreateResponse(HttpStatusCode.OK);
}
```

[Пропустить примеры входных данных](#input---attributes)

#### <a name="http-trigger-get-multiple-docs-using-sqlquery-c-script"></a>Триггер HTTP, получение нескольких документов, используется SqlQuery (скрипт C#)

В следующем примере показана [функция скрипта C#](functions-reference-csharp.md), которая получает список документов. Функция активируется с помощью HTTP-запроса. Запрос указывается в свойстве атрибута `SqlQuery`.

Ниже показан файл *function.json*.

```json
{
  "bindings": [
    {
      "authLevel": "anonymous",
      "name": "req",
      "type": "httpTrigger",
      "direction": "in",
      "methods": [
        "get",
        "post"
      ]
    },
    {
      "name": "$return",
      "type": "http",
      "direction": "out"
    },
    {
      "type": "documentDB",
      "name": "toDoItems",
      "databaseName": "ToDoItems",
      "collectionName": "Items",
      "connection": "CosmosDBConnection",
      "direction": "in",
      "sqlQuery": "SELECT top 2 * FROM c order by c._ts desc"
    }
  ],
  "disabled": false
}
```

Ниже приведен код скрипта C#.

```cs
using System.Net;

public static HttpResponseMessage Run(HttpRequestMessage req, IEnumerable<ToDoItem> toDoItems, TraceWriter log)
{
    log.Info("C# HTTP trigger function processed a request.");

    foreach (ToDoItem toDoItem in toDoItems)
    {
        log.Info(toDoItem.Description);
    }
    return req.CreateResponse(HttpStatusCode.OK);
}
```

[Пропустить примеры входных данных](#input---attributes)

#### <a name="http-trigger-get-multiple-docs-using-documentclient-c-script"></a>Триггер HTTP, получение нескольких документов, используется DocumentClient (скрипт C#)

В следующем примере показана [функция скрипта C#](functions-reference-csharp.md), которая получает список документов. Функция активируется с помощью HTTP-запроса. Код использует экземпляр `DocumentClient`, предоставленный привязкой Azure Cosmos DB для считывания списка документов. Экземпляр `DocumentClient` может также использоваться для операций записи.

Ниже показан файл *function.json*.

```json
{
  "bindings": [
    {
      "authLevel": "anonymous",
      "name": "req",
      "type": "httpTrigger",
      "direction": "in",
      "methods": [
        "get",
        "post"
      ]
    },
    {
      "name": "$return",
      "type": "http",
      "direction": "out"
    },
    {
      "type": "documentDB",
      "name": "client",
      "databaseName": "ToDoItems",
      "collectionName": "Items",
      "connection": "CosmosDBConnection",
      "direction": "inout"
    }
  ],
  "disabled": false
}
```

Ниже приведен код скрипта C#.

```cs
#r "Microsoft.Azure.Documents.Client"

using System.Net;
using Microsoft.Azure.Documents.Client;
using Microsoft.Azure.Documents.Linq;

public static async Task<HttpResponseMessage> Run(HttpRequestMessage req, DocumentClient client, TraceWriter log)
{
    log.Info("C# HTTP trigger function processed a request.");

    Uri collectionUri = UriFactory.CreateDocumentCollectionUri("ToDoItems", "Items");
    string searchterm = req.GetQueryNameValuePairs()
        .FirstOrDefault(q => string.Compare(q.Key, "searchterm", true) == 0)
        .Value;

    if (searchterm == null)
    {
        return req.CreateResponse(HttpStatusCode.NotFound);
    }

    log.Info($"Searching for word: {searchterm} using Uri: {collectionUri.ToString()}");
    IDocumentQuery<ToDoItem> query = client.CreateDocumentQuery<ToDoItem>(collectionUri)
        .Where(p => p.Description.Contains(searchterm))
        .AsDocumentQuery();

    while (query.HasMoreResults)
    {
        foreach (ToDoItem result in await query.ExecuteNextAsync())
        {
            log.Info(result.Description);
        }
    }
    return req.CreateResponse(HttpStatusCode.OK);
}
```

[Пропустить примеры входных данных](#input---attributes)

### <a name="input---javascript-examples"></a>Примеры входных данных JavaScript

В этом разделе содержатся следующие примеры:

* [Триггер очереди, поисковый идентификатор из JSON](#queue-trigger-look-up-id-from-json-javascript)
* [Триггер HTTP, поисковый идентификатор из строки запроса](#http-trigger-look-up-id-from-query-string-javascript)
* [Триггер HTTP, поисковый идентификатор из данных маршрута](#http-trigger-look-up-id-from-route-data-javascript)
* [Триггер очереди, получение нескольких документов, используется SqlQuery](#queue-trigger-get-multiple-docs-using-sqlquery-javascript)

[Пропустить примеры входных данных](#input---attributes)

#### <a name="queue-trigger-look-up-id-from-json-javascript"></a>Триггер очереди, поисковый идентификатор из JSON (JavaScript)

В следующем примере показана входная привязка Cosmos DB в файле *function.json* и функция [JavaScript](functions-reference-node.md), которая использует привязку. Функция считывает один документ и обновляет текстовое значение в документе.

Данные привязки в файле *function.json*:

```json
{
    "name": "inputDocumentIn",
    "type": "documentDB",
    "databaseName": "MyDatabase",
    "collectionName": "MyCollection",
    "id" : "{queueTrigger_payload_property}",
    "partitionKey": "{queueTrigger_payload_property}",
    "connection": "MyAccount_COSMOSDB",
    "direction": "in"
},
{
    "name": "inputDocumentOut",
    "type": "documentDB",
    "databaseName": "MyDatabase",
    "collectionName": "MyCollection",
    "createIfNotExists": false,
    "partitionKey": "{queueTrigger_payload_property}",
    "connection": "MyAccount_COSMOSDB",
    "direction": "out"
}
```
В разделе [Конфигурация](#input---configuration) описываются эти свойства.

Ниже показан код JavaScript.

```javascript
    // Change input document contents using Azure Cosmos DB input binding, using context.bindings.inputDocumentOut
    module.exports = function (context) {
    context.bindings.inputDocumentOut = context.bindings.inputDocumentIn;
    context.bindings.inputDocumentOut.text = "This was updated!";
    context.done();
    };
```

[Пропустить примеры входных данных](#input---attributes)

#### <a name="http-trigger-look-up-id-from-query-string-javascript"></a>Триггер HTTP, поисковый идентификатор из строки запроса (JavaScript)

В следующем примере показана [функция JavaScript](functions-reference-node.md), которая получает один документ. Функция инициируется HTTP-запросом, который в строке запроса указывает идентификатор для поиска. Этот идентификатор используется для получения `ToDoItem` документа из указанной базы данных и коллекции.

Ниже показан файл *function.json*.

```json
{
  "bindings": [
    {
      "authLevel": "anonymous",
      "name": "req",
      "type": "httpTrigger",
      "direction": "in",
      "methods": [
        "get",
        "post"
      ]
    },
    {
      "name": "$return",
      "type": "http",
      "direction": "out"
    },
    {
      "type": "documentDB",
      "name": "toDoItem",
      "databaseName": "ToDoItems",
      "collectionName": "Items",
      "connection": "CosmosDBConnection",
      "direction": "in",
      "Id": "{Query.id}"
    }
  ],
  "disabled": true
}
```

Ниже показан код JavaScript.

```javascript
module.exports = function (context, req, toDoItem) {
    context.log('JavaScript queue trigger function processed work item');
    if (!toDoItem)
    {
        context.log("ToDo item not found");
    }
    else
    {
        context.log("Found ToDo item, Description=" + toDoItem.Description);
    }

    context.done();
};
```

[Пропустить примеры входных данных](#input---attributes)

#### <a name="http-trigger-look-up-id-from-route-data-javascript"></a>Триггер HTTP, поисковый идентификатор из данных маршрута (JavaScript)

В следующем примере показана [функция JavaScript](functions-reference-node.md), которая получает один документ. Функция инициируется HTTP-запросом, который в строке запроса указывает идентификатор для поиска. Этот идентификатор используется для получения `ToDoItem` документа из указанной базы данных и коллекции.

Ниже показан файл *function.json*.

```json
{
  "bindings": [
    {
      "authLevel": "anonymous",
      "name": "req",
      "type": "httpTrigger",
      "direction": "in",
      "methods": [
        "get",
        "post"
      ],
      "route":"todoitems/{id}"
    },
    {
      "name": "$return",
      "type": "http",
      "direction": "out"
    },
    {
      "type": "documentDB",
      "name": "toDoItem",
      "databaseName": "ToDoItems",
      "collectionName": "Items",
      "connection": "CosmosDBConnection",
      "direction": "in",
      "Id": "{id}"
    }
  ],
  "disabled": false
}
```

Ниже показан код JavaScript.

```javascript
module.exports = function (context, req, toDoItem) {
    context.log('JavaScript queue trigger function processed work item');
    if (!toDoItem)
    {
        context.log("ToDo item not found");
    }
    else
    {
        context.log("Found ToDo item, Description=" + toDoItem.Description);
    }

    context.done();
};
```

[Пропустить примеры входных данных](#input---attributes)



#### <a name="queue-trigger-get-multiple-docs-using-sqlquery-javascript"></a>Триггер очереди, получение нескольких документов, используется SqlQuery (JavaScript)

В следующем примере показана входная привязка Azure Cosmos DB в файле *function.json* и [функция JavaScript](functions-reference-node.md), которая использует привязку. Функция извлекает несколько документов, указанных SQL-запросом, используя триггер очереди для настройки параметров запроса.

Триггер очереди предоставляет параметр `departmentId`. Сообщение из очереди `{ "departmentId" : "Finance" }` возвратит все записи для финансового отдела.

Данные привязки в файле *function.json*:

```json
{
    "name": "documents",
    "type": "documentdb",
    "direction": "in",
    "databaseName": "MyDb",
    "collectionName": "MyCollection",
    "sqlQuery": "SELECT * from c where c.departmentId = {departmentId}",
    "connection": "CosmosDBConnection"
}
```

В разделе [Конфигурация](#input---configuration) описываются эти свойства.

Ниже показан код JavaScript.

```javascript
    module.exports = function (context, input) {
        var documents = context.bindings.documents;
        for (var i = 0; i < documents.length; i++) {
            var document = documents[i];
            // operate on each document
        }
        context.done();
    };
```

[Пропустить примеры входных данных](#input---attributes)

<a name="infsharp"></a>

### <a name="input---f-examples"></a>Примеры входных данных F#

В следующем примере показаны привязка Cosmos DB в файле *function.json* и [функция F#](functions-reference-fsharp.md), которая использует эту привязку. Функция считывает один документ и обновляет текстовое значение в документе.

Данные привязки в файле *function.json*:

```json
{
    "name": "inputDocument",
    "type": "documentDB",
    "databaseName": "MyDatabase",
    "collectionName": "MyCollection",
    "id" : "{queueTrigger}",
    "connection": "MyAccount_COSMOSDB",
    "direction": "in"
}
```

В разделе [Конфигурация](#input---configuration) описываются эти свойства.

Ниже показан код F#.

```fsharp
    (* Change input document contents using Azure Cosmos DB input binding *)
    open FSharp.Interop.Dynamic
    let Run(myQueueItem: string, inputDocument: obj) =
    inputDocument?text <- "This has changed."
```

Для этого примера нужен файл `project.json`, указывающий зависимости NuGet `FSharp.Interop.Dynamic` и `Dynamitey`:

```json
{
    "frameworks": {
        "net46": {
            "dependencies": {
                "Dynamitey": "1.0.2",
                "FSharp.Interop.Dynamic": "3.0.0"
            }
        }
    }
}
```

Чтобы добавить файл `project.json`, см. раздел [об управлении пакетом F#](functions-reference-fsharp.md#package).

## <a name="input---attributes"></a>Входные атрибуты

В [библиотеках классов C#](functions-dotnet-class-library.md) используйте атрибут [DocumentDB](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/v2.x/src/WebJobs.Extensions.DocumentDB/DocumentDBAttribute.cs).

Конструктор атрибута принимает имя базы данных и имя коллекции. Дополнительные сведения о параметрах и других свойствах, которые можно настроить, см. в следующем разделе о [настройке](#input---configuration).

## <a name="input---configuration"></a>Входная конфигурация

В следующей таблице описываются свойства конфигурации привязки, которые задаются в файле *function.json* и атрибуте `DocumentDB`.

|свойство function.json | Свойство атрибута |Описание|
|---------|---------|----------------------|
|**type**     || Нужно задать значение `documentdb`.        |
|**direction**     || Нужно задать значение `in`.         |
|**name**     || Имя параметра привязки, представляющего документ в функции.  |
|**databaseName** |**DatabaseName** |База данных, содержащая документ.        |
|**collectionName** |**CollectionName** | Имя коллекции, содержащей документ. |
|**id**    | **Id** | Идентификатор документа, который нужно получить. Это свойство поддерживает [выражения привязок](./functions-bindings-expressions-patterns.md). Не задавайте свойства **id** или **sqlQuery** одновременно. Если не задать ни одного из них, извлекается вся коллекция. |
|**sqlQuery**  |**SqlQuery**  | SQL-запрос к Azure Cosmos DB, используемый для извлечения нескольких документов. Свойство поддерживает привязки времени выполнения, как показано в примере: `SELECT * FROM c where c.departmentId = {departmentId}`. Не задавайте свойства **id** или **sqlQuery** одновременно. Если не задать ни одного из них, извлекается вся коллекция.|
|**подключение**     |**ConnectionStringSetting**|Имя параметра приложения, содержащего строку подключения к Azure Cosmos DB.        |
|**partitionKey**|**PartitionKey**|Задает значение ключа секции для поиска. Может включать параметры привязки.|

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="input---usage"></a>Использование входной привязки

В функциях C# и F# любые изменения, внесенные во входной документ посредством именованных входных параметров, сохраняются автоматически после успешного выхода из функции.

В функциях JavaScript изменения не обрабатываются автоматически при выходе из функции. Для внесения изменений используйте `context.bindings.<documentName>In` и `context.bindings.<documentName>Out`. Ознакомьтесь с [примером на языке JavaScript](#input---javascript-examples).

## <a name="output"></a>Output

Выходная привязка Azure Cosmos DB позволяет записать новый документ в базу данных Azure Cosmos DB с помощью API SQL.

## <a name="output---examples"></a>Примеры выходных данных

Языковой пример см. в разделах:

* [C#](#output---c-examples)
* [Скрипт C# (CSX)](#output---c-script-examples)
* [JavaScript](#output---javascript-examples)
* [F#](#output---f-examples)

См. также [пример входных данных](#input---c-examples), в котором используется `DocumentClient`.

[Пропустить примеры вывода](#output---attributes)

### <a name="output---c-examples"></a>Примеры выходных данных C#

В этом разделе содержатся следующие примеры:

* Триггер очереди, запись одного документа
* Триггер очереди, запись документов при помощи IAsyncCollector

В примерах используется `ToDoItem` простого типа:

```cs
namespace CosmosDBSamplesV1
{
    public class ToDoItem
    {
        public string Id { get; set; }
        public string Description { get; set; }
    }
}
```

[Пропустить примеры вывода](#output---attributes)

#### <a name="queue-trigger-write-one-doc-c"></a>Триггер очереди, запись одного документа (C#)

В следующем примере показана [функция C#](functions-dotnet-class-library.md), которая добавляет документ в базу данных, используя данные, полученные из сообщения хранилища очередей.

```cs
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Host;
using System;

namespace CosmosDBSamplesV1
{
    public static class WriteOneDoc
    {
        [FunctionName("WriteOneDoc")]
        public static void Run(
            [QueueTrigger("todoqueueforwrite")] string queueMessage,
            [DocumentDB(
                databaseName: "ToDoItems",
                collectionName: "Items",
                ConnectionStringSetting = "CosmosDBConnection")]out dynamic document,
            TraceWriter log)
        {
            document = new { Description = queueMessage, id = Guid.NewGuid() };

            log.Info($"C# Queue trigger function inserted one row");
            log.Info($"Description={queueMessage}");
        }
    }
}
```

[Пропустить примеры вывода](#output---attributes)

#### <a name="queue-trigger-write-docs-using-iasynccollector-c"></a>Триггер очереди, запись документов с помощью IAsyncCollector (C#)

В следующем примере показана [функция C#](functions-dotnet-class-library.md), которая добавляет коллекцию документов в базу данных с помощью данных, полученных из JSON сообщения очереди.

```cs
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Host;
using System.Threading.Tasks;

namespace CosmosDBSamplesV1
{
    public static class WriteDocsIAsyncCollector
    {
        [FunctionName("WriteDocsIAsyncCollector")]
        public static async Task Run(
            [QueueTrigger("todoqueueforwritemulti")] ToDoItem[] toDoItemsIn,
            [DocumentDB(
                databaseName: "ToDoItems",
                collectionName: "Items",
                ConnectionStringSetting = "CosmosDBConnection")]
                IAsyncCollector<ToDoItem> toDoItemsOut,
            TraceWriter log)
        {
            log.Info($"C# Queue trigger function processed {toDoItemsIn?.Length} items");

            foreach (ToDoItem toDoItem in toDoItemsIn)
            {
                log.Info($"Description={toDoItem.Description}");
                await toDoItemsOut.AddAsync(toDoItem);
            }
        }
    }
}
```

[Пропустить примеры вывода](#output---attributes)

### <a name="output---c-script-examples"></a>Примеры выходных данных скрипта C#

В этом разделе содержатся следующие примеры:

* Триггер очереди, запись одного документа
* Триггер очереди, запись документов при помощи IAsyncCollector

[Пропустить примеры вывода](#output---attributes)

#### <a name="queue-trigger-write-one-doc-c-script"></a>Триггер очереди, запись одного документа (скрипт C#)

В следующем примере показаны выходная привязка Azure Cosmos DB в файле *function.json* и [функция сценария C#](functions-reference-csharp.md), которая использует эту привязку. Функция использует входную привязку очереди для очереди, которую получает JSON в следующем формате:

```json
{
    "name": "John Henry",
    "employeeId": "123456",
    "address": "A town nearby"
}
```

Функция создает документы Azure Cosmos DB для каждой записи в следующем формате:

```json
{
    "id": "John Henry-123456",
    "name": "John Henry",
    "employeeId": "123456",
    "address": "A town nearby"
}
```

Данные привязки в файле *function.json*:

```json
{
    "name": "employeeDocument",
    "type": "documentDB",
    "databaseName": "MyDatabase",
    "collectionName": "MyCollection",
    "createIfNotExists": true,
    "connection": "MyAccount_COSMOSDB",
    "direction": "out"
}
```

В разделе [Конфигурация](#output---configuration) описываются эти свойства.

Ниже приведен код скрипта C#.

```cs
    #r "Newtonsoft.Json"

    using Microsoft.Azure.WebJobs.Host;
    using Newtonsoft.Json.Linq;

    public static void Run(string myQueueItem, out object employeeDocument, TraceWriter log)
    {
        log.Info($"C# Queue trigger function processed: {myQueueItem}");

        dynamic employee = JObject.Parse(myQueueItem);

        employeeDocument = new {
            id = employee.name + "-" + employee.employeeId,
            name = employee.name,
            employeeId = employee.employeeId,
            address = employee.address
        };
    }
```

#### <a name="queue-trigger-write-docs-using-iasynccollector"></a>Триггер очереди, запись документов при помощи IAsyncCollector

Для создания нескольких документов можно выполнить привязку к `ICollector<T>` или к `IAsyncCollector<T>`, где `T` — любой из поддерживаемых типов.

Этот пример относится к простому типу `ToDoItem`:

```cs
namespace CosmosDBSamplesV1
{
    public class ToDoItem
    {
        public string Id { get; set; }
        public string Description { get; set; }
    }
}
```

Ниже показан файл function.json:

```json
{
  "bindings": [
    {
      "name": "toDoItemsIn",
      "type": "queueTrigger",
      "direction": "in",
      "queueName": "todoqueueforwritemulti",
      "connection": "AzureWebJobsStorage"
    },
    {
      "type": "documentDB",
      "name": "toDoItemsOut",
      "databaseName": "ToDoItems",
      "collectionName": "Items",
      "connection": "CosmosDBConnection",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

Ниже приведен код скрипта C#.

```cs
using System;

public static async Task Run(ToDoItem[] toDoItemsIn, IAsyncCollector<ToDoItem> toDoItemsOut, TraceWriter log)
{
    log.Info($"C# Queue trigger function processed {toDoItemsIn?.Length} items");

    foreach (ToDoItem toDoItem in toDoItemsIn)
    {
        log.Info($"Description={toDoItem.Description}");
        await toDoItemsOut.AddAsync(toDoItem);
    }
}
```

[Пропустить примеры вывода](#output---attributes)

### <a name="output---javascript-examples"></a>Примеры выходных данных JavaScript

В следующем примере показана выходная привязка Azure Cosmos DB в файле *function.json* и [функция JavaScript](functions-reference-node.md), которая использует эту привязку. Функция использует входную привязку очереди для очереди, которую получает JSON в следующем формате:

```json
{
    "name": "John Henry",
    "employeeId": "123456",
    "address": "A town nearby"
}
```

Функция создает документы Azure Cosmos DB для каждой записи в следующем формате:

```json
{
    "id": "John Henry-123456",
    "name": "John Henry",
    "employeeId": "123456",
    "address": "A town nearby"
}
```

Данные привязки в файле *function.json*:

```json
{
    "name": "employeeDocument",
    "type": "documentDB",
    "databaseName": "MyDatabase",
    "collectionName": "MyCollection",
    "createIfNotExists": true,
    "connection": "MyAccount_COSMOSDB",
    "direction": "out"
}
```

В разделе [Конфигурация](#output---configuration) описываются эти свойства.

Ниже показан код JavaScript.

```javascript
    module.exports = function (context) {

      context.bindings.employeeDocument = JSON.stringify({
        id: context.bindings.myQueueItem.name + "-" + context.bindings.myQueueItem.employeeId,
        name: context.bindings.myQueueItem.name,
        employeeId: context.bindings.myQueueItem.employeeId,
        address: context.bindings.myQueueItem.address
      });

      context.done();
    };
```

[Пропустить примеры вывода](#output---attributes)

### <a name="output---f-examples"></a>Примеры выходных данных F#

В следующем примере показаны выходная привязка Azure Cosmos DB в файле *function.json* и [функция F#](functions-reference-fsharp.md), которая использует эту привязку. Функция использует входную привязку очереди для очереди, которую получает JSON в следующем формате:

```json
{
    "name": "John Henry",
    "employeeId": "123456",
    "address": "A town nearby"
}
```

Функция создает документы Azure Cosmos DB для каждой записи в следующем формате:

```json
{
    "id": "John Henry-123456",
    "name": "John Henry",
    "employeeId": "123456",
    "address": "A town nearby"
}
```

Данные привязки в файле *function.json*:

```json
{
    "name": "employeeDocument",
    "type": "documentDB",
    "databaseName": "MyDatabase",
    "collectionName": "MyCollection",
    "createIfNotExists": true,
    "connection": "MyAccount_COSMOSDB",
    "direction": "out"
}
```
В разделе [Конфигурация](#output---configuration) описываются эти свойства.

Ниже показан код F#.

```fsharp
    open FSharp.Interop.Dynamic
    open Newtonsoft.Json

    type Employee = {
      id: string
      name: string
      employeeId: string
      address: string
    }

    let Run(myQueueItem: string, employeeDocument: byref<obj>, log: TraceWriter) =
      log.Info(sprintf "F# Queue trigger function processed: %s" myQueueItem)
      let employee = JObject.Parse(myQueueItem)
      employeeDocument <-
        { id = sprintf "%s-%s" employee?name employee?employeeId
          name = employee?name
          employeeId = employee?employeeId
          address = employee?address }
```

Для этого примера нужен файл `project.json`, указывающий зависимости NuGet `FSharp.Interop.Dynamic` и `Dynamitey`:

```json
{
    "frameworks": {
        "net46": {
          "dependencies": {
            "Dynamitey": "1.0.2",
            "FSharp.Interop.Dynamic": "3.0.0"
           }
        }
    }
}
```

Чтобы добавить файл `project.json`, см. раздел [об управлении пакетом F#](functions-reference-fsharp.md#package).

## <a name="output---attributes"></a>Выходные атрибуты

В [библиотеках классов C#](functions-dotnet-class-library.md) используйте атрибут [DocumentDB](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/v2.x/src/WebJobs.Extensions.DocumentDB/DocumentDBAttribute.cs).

Конструктор атрибута принимает имя базы данных и имя коллекции. Дополнительные сведения о параметрах и других свойствах, которые можно настроить, см. в разделе [Выходная конфигурация](#output---configuration). Ниже приведен пример атрибута `DocumentDB` в сигнатуре метода:

```csharp
    [FunctionName("QueueToDocDB")]
    public static void Run(
        [QueueTrigger("myqueue-items", Connection = "AzureWebJobsStorage")] string myQueueItem,
        [DocumentDB("ToDoList", "Items", Id = "id", ConnectionStringSetting = "myCosmosDB")] out dynamic document)
    {
        ...
    }
```

Полный пример см. в разделе [Пример выходных данных C#](#output---c-examples).

## <a name="output---configuration"></a>Выходная конфигурация

В следующей таблице описываются свойства конфигурации привязки, которые задаются в файле *function.json* и атрибуте `DocumentDB`.

|свойство function.json | Свойство атрибута |Описание|
|---------|---------|----------------------|
|**type**     || Нужно задать значение `documentdb`.        |
|**direction**     || Нужно задать значение `out`.         |
|**name**     || Имя параметра привязки, представляющего документ в функции.  |
|**databaseName** | **DatabaseName**|База данных, содержащая коллекцию, в которой создается документ.     |
|**collectionName** |**CollectionName**  | Имя коллекции, в которой создается документ. |
|**createIfNotExists**  |**CreateIfNotExists**    | Логическое значение, указывающее, будет ли создана коллекция при ее отсутствии. Значение по умолчанию — *false*, так как коллекции создаются с использованием зарезервированной пропускной способности, с которой связаны ценовые требования. Дополнительные сведения см. на [странице с расценками](https://azure.microsoft.com/pricing/details/documentdb/).  |
|**partitionKey**|**PartitionKey** |Если для `CreateIfNotExists` задано значение true, определяется путь к ключу раздела для созданной коллекции.|
|**collectionThroughput**|**collectionThroughput**| Если для `CreateIfNotExists` задано значение true, определяется [пропускная способность](../cosmos-db/set-throughput.md) созданной коллекции.|
|**подключение**    |**ConnectionStringSetting** |Имя параметра приложения, содержащего строку подключения к Azure Cosmos DB.        |

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="output---usage"></a>Использование выходной привязки

При записи в параметре вывода функции в базе данных по умолчанию создается документ, для которого автоматически создается GUID в качестве идентификатора документа. Идентификатор выходного документа можно определить, указав свойство `id` в объекте JSON, передаваемом в параметр вывода.

> [!Note]
> Если указать идентификатор существующего документа, он перезаписывается новым выходным документом.

## <a name="exceptions-and-return-codes"></a>Исключения и коды возврата

| Привязка | Справочные материалы |
|---|---|
| Cosmos DB | [Коды ошибок Cosmos DB](https://docs.microsoft.com/rest/api/cosmos-db/http-status-codes-for-cosmosdb) |

## <a name="next-steps"></a>Дальнейшие действия

* [Azure Cosmos DB: обработка данных бессерверных баз данных с помощью службы "Функции Azure"](../cosmos-db/serverless-computing-database.md)
* [Основные понятия триггеров и привязок в Функциях Azure](functions-triggers-bindings.md)

<!---
> [!div class="nextstepaction"]
> [Go to a quickstart that uses a Cosmos DB trigger](functions-create-cosmos-db-triggered-function.md)
--->
