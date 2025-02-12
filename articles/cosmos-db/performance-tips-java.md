---
title: Советы по повышению производительности Azure Cosmos DB и Java
description: Узнайте, как повысить производительность базы данных Azure Cosmos DB с помощью параметров конфигурации клиента
author: SnehaGunda
ms.service: cosmos-db
ms.devlang: java
ms.topic: conceptual
ms.date: 05/23/2019
ms.author: sngun
ms.openlocfilehash: 7d6427db7090b3419fbe67535baeb4a5df6a5d65
ms.sourcegitcommit: a6873b710ca07eb956d45596d4ec2c1d5dc57353
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/16/2019
ms.locfileid: "68261312"
---
# <a name="performance-tips-for-azure-cosmos-db-and-java"></a>Советы по повышению производительности для Java и Azure Cosmos DB

> [!div class="op_single_selector"]
> * [Async Java](performance-tips-async-java.md)
> * [Java](performance-tips-java.md)
> * [.NET](performance-tips.md)
> 

Azure Cosmos DB — быстрая и гибкая распределенная база данных, которая легко масштабируется с гарантированной задержкой и пропускной способностью. Для масштабирования базы данных с помощью Azure Cosmos DB не нужно вносить в архитектуру существенные изменения или писать сложный код. Для увеличения или уменьшения масштаба достаточно выполнить один вызов API. Дополнительные сведения см. в разделах [о подготовке пропускной способности контейнера](how-to-provision-container-throughput.md) и [о подготовке пропускной способности базы данных](how-to-provision-database-throughput.md). Но так как для доступа к Azure Cosmos DB выполняются сетевые вызовы, некоторые оптимизации на стороне клиента помогут повысить производительность при работе с [пакетом SDK для Java и SQL](documentdb-sdk-java.md).

Чтобы повысить производительность базы данных, рассмотрите следующие варианты.

## <a name="networking"></a>Сеть
<a id="direct-connection"></a>

1. **Режим подключения: используйте DirectHttps**

    Режим подключения к Azure Cosmos DB серьезно влияет на производительность, в частности на задержку на стороне клиента. Важнейший параметр для настройки политики подключения ([ConnectionPolicy](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb.connectionpolicy)) на стороне клиента — это [ConnectionMode](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb.connectionmode).  Этот параметр может принимать одно из двух значений:

   1. [Gateway](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb.connectionmode) ("Шлюз", используется по умолчанию);
   2. [DirectHttps](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb.connectionmode) ("Прямое подключение через HTTPS").

      Режим шлюза поддерживается на всех платформах SDK, поэтому он установлен по умолчанию.  Так как в режиме шлюза используется стандартный порт HTTPS и одна конечная точка, он будет правильным выбором для приложений, запущенных в корпоративных сетях со строгими ограничениями брандмауэра. Но режим шлюза не обеспечивает максимальную производительность, поскольку задействует дополнительный сетевой узел при каждой операции чтения или записи данных в Azure Cosmos DB. Режим прямого подключения позволит повысить производительность. 

      Пакет SDK для Java использует транспортный протокол HTTPS. HTTPS использует SSL для первоначальной аутентификации и шифрования трафика. При работе с пакетом SDK для Java нужно открыть только стандартный порт HTTPS 443. 

      Режим подключения настраивается с помощью параметра ConnectionPolicy во время создания экземпляра DocumentClient. 

      ```Java
      public ConnectionPolicy getConnectionPolicy() {
        ConnectionPolicy policy = new ConnectionPolicy();
        policy.setConnectionMode(ConnectionMode.DirectHttps);
        policy.setMaxPoolSize(1000);
        return policy;
      }
        
      ConnectionPolicy connectionPolicy = new ConnectionPolicy();
      DocumentClient client = new DocumentClient(HOST, MASTER_KEY, connectionPolicy, null);
      ```

      ![Пример политики подключения Azure Cosmos DB](./media/performance-tips-java/connection-policy.png)

   <a id="same-region"></a>
2. **Повышение производительности за счет размещения клиентов в одном регионе Azure**

    Если это возможно, размещайте приложения, выполняющие вызовы к Azure Cosmos DB, в том же регионе, в котором находится база данных Azure Cosmos DB. Для приблизительного сравнения: вызовы к Azure Cosmos DB в пределах региона выполняются в течение 1–2 мс, но задержка между восточным и западным побережьем США превышает 50 мс. Значение задержки может отличаться в зависимости от выбранного маршрута при передаче запроса от клиента к границе центра обработки данных Azure. Минимальная возможная задержка достигается при размещении клиентского приложения в том же регионе Azure, в котором предоставляется конечная точка Azure Cosmos DB. Список доступных регионов см. на странице [Регионы Azure](https://azure.microsoft.com/regions/#services).

    ![Пример политики подключения Azure Cosmos DB](./media/performance-tips/same-region.png)
   
## <a name="sdk-usage"></a>Использование пакета SDK
1. **Установка последней версии пакета SDK**

    Пакеты SDK для Azure Cosmos DB постоянно улучшаются, чтобы обеспечивать самую высокую производительность. Сведения об улучшениях в последней версии пакета SDK см. в статье о [пакете SDK для Azure Cosmos DB](documentdb-sdk-java.md).
2. **Используйте один и тот же клиент Azure Cosmos DB в течение всего жизненного цикла приложения.**

    Каждый экземпляр [DocumentClient](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb.documentclient) в режиме прямого подключения является потокобезопасным, эффективно управляет подключениями и кэширует адреса. Чтобы реализовать возможность управления подключениями и оптимизировать производительность, рекомендуется использовать один экземпляр DocumentClient для каждого домена в течение жизненного цикла приложения.

   <a id="max-connection"></a>
3. **При использовании режима шлюза увеличьте размер пула (MaxPoolSize) для каждого узла.**

    В режиме шлюза Azure Cosmos DB выполняет запросы через HTTPS или REST, а значит на них распространяется стандартное ограничение на количество подключений к одному имени узла или IP-адресу. Возможно, стоит увеличить это значение (до 200–1000), чтобы клиентская библиотека могла использовать несколько одновременных подключений к Azure Cosmos DB. В пакете SDK для Java для параметра [ConnectionPolicy.getMaxPoolSize](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb.connectionpolicy.getmaxpoolsize) по умолчанию установлено значение 100. Используйте [setMaxPoolSize]( https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb.connectionpolicy.setmaxpoolsize), чтобы изменить это значение.

4. **Настройка параллельных запросов для секционированных коллекций**

    В пакете SDK для Java и Azure Cosmos DB SQL версии 1.9.0 и выше поддерживаются параллельные запросы, позволяющие обращаться к секционированным коллекциям в параллельном режиме. Дополнительные сведения см. в [примерах кода](https://github.com/Azure/azure-documentdb-java/tree/master/documentdb-examples/src/test/java/com/microsoft/azure/documentdb/examples) для работы с пакетами SDK. Параллельные запросы предназначены для сокращения задержки при обработке запросов и улучшения пропускной способности посредством их последовательных аналогов.

    (а) ***Применение setMaxDegreeOfParallelism.\:*** Параллельный режим запросов позволяет одновременно обращаться к нескольким секциям. Однако данные из каждой секционированной коллекции извлекаются в рамках запроса последовательно. С помощью [setMaxDegreeOfParallelism](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb.feedoptions.setmaxdegreeofparallelism) установите значение, соответствующее количеству секций, что обеспечит максимальную вероятность высокой производительности запроса при сохранении всех остальных параметров системы. Если вы не знаете количество секций, просто используйте высокое значение для setMaxDegreeOfParallelism. Система автоматически выберет минимальное из двух значений: количество секций или число, указанное пользователем. 

    Следует отметить, что параллельные запросы обеспечивают больше преимуществ, если данные равномерно распределены во всех секциях по отношению к запросу. Если коллекция секционируется таким образом, что все или большинство данных, возвращаемых запросом, содержатся в нескольких секциях (в худшем случае в одной), это негативно скажется на производительности запроса.

    (б) ***Применение setMaxBufferedItemCount.\:*** В параллельном режиме запрос выполняет предварительную выборку результатов, пока клиент обрабатывает уже полученный пакет данных. Предварительная выборка способствует общему уменьшению задержки при обработке запроса. Значение setMaxBufferedItemCount ограничивает количество предварительно выбираемых результатов. Указав для [setMaxBufferedItemCount](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb.feedoptions.setmaxbuffereditemcount) ожидаемое количество возвращаемых результатов (или еще более высокое значение), вы обеспечите максимальное влияние предварительной выборки на производительность запросов.

    Предварительная выборка работает одинаково при любом значении параметра MaxDegreeOfParallelism. Для данных из всех секций применяется один буфер.  

5. **Применение интервала задержки getRetryAfterInMilliseconds.**

    Во время проверки производительности следует увеличивать нагрузку до тех пор, пока регулирование не будет выполняться при небольшой частоте запросов. При регулировании клиентское приложение должно реализовать интервал частоты повтора для частоты повтора сервера. Это гарантирует, что время ожидания между повторными попытками будет минимальным. Политика повторов реализована в версии 1.8.0 и более поздних версиях [пакета SDK для Java](documentdb-sdk-java.md). Дополнительные сведения см. в статье о методе [getRetryAfterInMilliseconds](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb.documentclientexception.getretryafterinmilliseconds).

6. **Развертывание рабочей нагрузки клиента**

    При проверке с высокой пропускной способностью (более 50 000 единиц запроса в секунду) клиентское приложение может стать узким местом из-за того, что на компьютере будут достигнуты максимальные уровни использования ЦП и сети. Если вы достигли этой точки, то можете повысить производительность Azure Cosmos DB, развернув клиентские приложения на нескольких серверах.

7. **Адресация на основе имен**

    Используйте адресацию на основе имен, то есть ссылки в формате `dbs/MyDatabaseId/colls/MyCollectionId/docs/MyDocumentId` вместо самоссылающегося (\_self) формата `dbs/<database_rid>/colls/<collection_rid>/docs/<document_rid>`. Это позволит избежать получения идентификаторов для всех ресурсов, требуемых для создания ссылки. Кэширование этих данных не увеличит производительность, поскольку ресурсы могут создаваться заново (иногда с теми же именами).

   <a id="tune-page-size"></a>
8. **Оптимизация производительности посредством настройки размера страницы для запросов и каналов чтения**

    Если при массовом чтении документов с помощью функции чтения канала (например [readDocuments](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb.documentclient.readdocuments)) или обработке запроса SQL будет получен слишком большой набор результатов, такие результаты возвращаются частями. По умолчанию результаты возвращаются в пакетах (не более 100 элементов и не более 1 МБ в каждом пакете).

    Чтобы снизить количество сетевых взаимодействий, необходимых для получения всех нужных результатов, попробуйте увеличить размер страницы до 1000 с помощью заголовка запроса [x-ms-max-item-count](https://docs.microsoft.com/rest/api/cosmos-db/common-cosmosdb-rest-request-headers). Чтобы отобразить только некоторые результаты, например, когда пользовательский интерфейс или приложение API возвращает только десять результатов за раз, размер страницы можно уменьшить до 10. Это позволит снизить пропускную способность, используемую на операции чтения и на выполнение запросов.

    Размер страницы также можно изменить с помощью [метода setPageSize](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb.feedoptionsbase.setpagesize).

## <a name="indexing-policy"></a>Политика индексации
 
1. **Увеличение скорости выполнения операций записи посредством исключения неиспользуемых путей из индексирования**

    Политика индексирования Azure Cosmos DB позволяет добавлять к индексированию или исключать из индексирования определенные пути к документам, используя механизм Indexing Paths ([IndexingPolicy.IncludedPaths](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb.indexingpolicy.setincludedpaths) и [IndexingPolicy.ExcludedPaths](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb.indexingpolicy.setexcludedpaths)). Возможность управления путями индексирования позволяет оптимизировать производительность записи и снизить затраты на хранение индекса для сценариев с заранее определенными шаблонами запросов. Это связано с тем, что затраты на индексирование непосредственно зависят от количества уникальных путей индексирования.  Например, в приведенном ниже коде показано, как исключить из индексации целый раздел документов (так называемое поддерево) с помощью подстановочного знака "*".

    ```Java
    Index numberIndex = Index.Range(DataType.Number);
    numberIndex.set("precision", -1);
    indexes.add(numberIndex);
    includedPath.setIndexes(indexes);
    includedPaths.add(includedPath);
    indexingPolicy.setIncludedPaths(includedPaths);
    collectionDefinition.setIndexingPolicy(indexingPolicy);
    ```

    Дополнительные сведения см. в статье [Политики индексации Azure Cosmos DB](indexing-policies.md).

## <a name="throughput"></a>Пропускная способность
<a id="measure-rus"></a>

1. **Измерение и настройка расхода единиц запроса/повторного использования**

    Azure Cosmos DB предоставляет обширный набор операций с документами в коллекции базы данных, в том числе реляционные и иерархические запросы с использованием UDF, хранимых процедур и триггеров. Затраты, связанные с каждой из этих операций, зависят от типа процессора, операций ввода-вывода и памяти, необходимой для завершения операции. Вместо того чтобы думать о закупке и управлении аппаратными ресурсами, вы можете думать о единице запроса (RU) как единой меры для ресурсов, необходимых для выполнения различных операций с базами данных и обслуживания запросов приложений.

    Пропускная способность выделяется на основе количества [единиц запроса](request-units.md), заданного для каждого контейнера. Удельный расход единиц запросов оценивается в расчете на одну секунду. Частота запросов для приложений, у которых она превышает подготовленные единицы запросов для контейнера, будет ограничена, пока она не упадет ниже зарезервированного для контейнера уровня. Если приложению требуется более высокий уровень пропускной способности, можно увеличить ее путем выделения дополнительных единиц запросов. 

    Сложность запроса влияет на количество единиц запроса, потребляемых операцией. Количество предикатов и их характер, количество определяемых пользователем функций и размер набора исходных данных — все это влияет на плату за операции запроса.

    Чтобы измерить издержки любой операции (создания, обновления или удаления), просмотрите заголовок [x-MS-Request-расход](https://docs.microsoft.com/rest/api/cosmos-db/common-cosmosdb-rest-response-headers) (или эквивалентное свойство RequestCharge в [\<ResourceResponse t >](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb.resourceresponse) или [FeedResponse\<t > ](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb.feedresponse)для измерения количества единиц запросов, потребляемых этими операциями.

    ```Java
    ResourceResponse<Document> response = client.createDocument(collectionLink, documentDefinition, null, false);

    response.getRequestCharge();
    ```             

    Стоимость запроса, указанная в этом заголовке, учитывается как часть подготовленной пропускной способности. Например, если вы предоставили 2000 единиц запроса в секунду, а приведенный выше запрос возвращает 1000 документов размером по 1 КБ каждый, затраты на операцию составят 1000 единиц. Таким образом, перед ограничением частоты выполнения последующих запросов сервер за одну секунду выполняет только два таких запроса. Чтобы узнать больше, ознакомьтесь с [единицами запроса](request-units.md) и [калькулятором единиц запроса](https://www.documentdb.com/capacityplanner).
   <a id="429"></a>
1. **Обработка ограничения скорости / слишком высокая частота запросов**

    Выполнение запроса, который превышает лимит зарезервированной пропускной способности для учетной записи, не приводит к снижению производительности сервера, так как пользователь не сможет превысить это зарезервированное значение. Сервер заранее завершит запрос с ошибкой RequestRateTooLarge (код состояния HTTP: 429) и вернет в заголовке [x-x-ms-retry-after-ms](https://docs.microsoft.com/rest/api/cosmos-db/common-cosmosdb-rest-response-headers) время (в миллисекундах), спустя которое можно повторно выполнить этот запрос.

        HTTP Status 429,
        Status Line: RequestRateTooLarge
        x-ms-retry-after-ms :100

    Пакеты SDK перехватят этот ответ, обработают заголовок retry-after, указанный сервером, и отправят запрос повторно. Если к вашей учетной записи параллельно имеет доступ только один клиент, следующая попытка будет успешной.

    По умолчанию количество повторных попыток отправки запроса составляет 9 (это значение задается клиентом). Если к вашей учетной записи имеют доступ несколько клиентов и они выполняют запросы одновременно, этого значения может быть недостаточно. В этом случае клиент выдаст для приложения исключение [DocumentClientException](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb.documentclientexception) с кодом состояния 429. Число повторных попыток по умолчанию можно переопределить в свойстве [RetryOptions](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb.connectionpolicy.setretryoptions) экземпляра [ConnectionPolicy](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb.connectionpolicy). По умолчанию в случае превышения заданного счетчика повторов исключение DocumentClientException с кодом состояния 429 возвращается через 30 секунд (совокупное время ожидания). Это происходит, даже если текущее значение количества повторных попыток (по умолчанию (9) или определенное пользователем) меньше максимального значения.

    Хотя автоматическая процедура отправки повторного запроса позволяет улучшить устойчивость приложений и повысить удобство работы с ними, она может снизить производительность, что, в свою очередь, станет причиной появления более длительных задержек.  Если настройка производительности повлияла на регулирование сервера и стала причиной автоматической отправки запросов пакетом SDK, это может стать причиной появления пиков задержек на стороне клиента. Чтобы избежать пиков задержек во время настройки производительности, проверьте расход ресурсов на каждую операцию и убедитесь, что значение частоты запросов не превышено. Дополнительные сведения см. в статье [Единицы запросов в DocumentDB](request-units.md).
3. **Использование меньших документов для более высокой пропускной способности**

    Стоимость запроса (плата за обработку запроса) для каждой операции напрямую зависит от размера документа. За операции с большими документами взимается больше единиц запроса, чем за операции с мелкими документами.

## <a name="next-steps"></a>Дальнейшие действия
Дополнительные сведения о создании приложения с высокой масштабируемостью и производительностью см. в статье [Partitioning and scaling in Azure Cosmos DB](partition-data.md) (Секционирование и масштабирование в Azure Cosmos DB).
