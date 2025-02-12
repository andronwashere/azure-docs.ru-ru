---
title: Советы по повышению производительности для Azure Cosmos DB и Async Java
description: Узнайте, как повысить производительность базы данных Azure Cosmos DB с помощью параметров конфигурации клиента
author: SnehaGunda
ms.service: cosmos-db
ms.devlang: java
ms.topic: conceptual
ms.date: 05/23/2019
ms.author: sngun
ms.openlocfilehash: 2c2d776012e702469be4fd3217fb89be0ad419bf
ms.sourcegitcommit: a6873b710ca07eb956d45596d4ec2c1d5dc57353
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/16/2019
ms.locfileid: "68261619"
---
# <a name="performance-tips-for-azure-cosmos-db-and-async-java"></a>Советы по повышению производительности для Azure Cosmos DB и Async Java

> [!div class="op_single_selector"]
> * [Async Java](performance-tips-async-java.md)
> * [Java](performance-tips-java.md)
> * [.NET](performance-tips.md)
> 

Azure Cosmos DB — быстрая и гибкая распределенная база данных, которая легко масштабируется с гарантированной задержкой и пропускной способностью. Для масштабирования базы данных с помощью Azure Cosmos DB не нужно вносить в архитектуру существенные изменения или писать сложный код. Для увеличения или уменьшения масштаба достаточно вызвать один метод интерфейса API или пакета SDK. Но так как для доступа к Azure Cosmos DB выполняются сетевые вызовы, некоторая оптимизация на стороне клиента поможет повысить производительность при работе с [пакетом SDK для Async Java и SQL](sql-api-sdk-async-java.md).

Чтобы повысить производительность базы данных, рассмотрите следующие варианты.

## <a name="networking"></a>Сеть
   <a id="same-region"></a>
1. **Повышение производительности за счет размещения клиентов в одном регионе Azure**

    Если это возможно, размещайте приложения, выполняющие вызовы к Azure Cosmos DB, в том же регионе, в котором находится база данных Azure Cosmos DB. Для приблизительного сравнения: вызовы к Azure Cosmos DB в пределах региона выполняются в течение 1–2 мс, но задержка между восточным и западным побережьем США превышает 50 мс. Значение задержки может отличаться в зависимости от выбранного маршрута при передаче запроса от клиента к границе центра обработки данных Azure. Минимальная возможная задержка достигается при размещении клиентского приложения в том же регионе Azure, в котором предоставляется конечная точка Azure Cosmos DB. Список доступных регионов см. на странице [Регионы Azure](https://azure.microsoft.com/regions/#services).

    ![Пример политики подключения Azure Cosmos DB](./media/performance-tips/same-region.png)

## <a name="sdk-usage"></a>Использование пакета SDK
1. **Установка последней версии пакета SDK**

    Пакеты SDK для Azure Cosmos DB постоянно улучшаются, чтобы обеспечивать самую высокую производительность. Сведения об улучшениях в последней версии пакета SDK см. в статье о [пакете SDK для Azure Cosmos DB](sql-api-sdk-async-java.md).
2. **Использование отдельного клиента Azure Cosmos DB в течение всего жизненного цикла приложения.**

    Каждый экземпляр AsyncDocumentClient является потокобезопасным, а также эффективно управляет подключениями и кэширует адреса. Чтобы реализовать возможность управления подключениями и оптимизировать производительность, рекомендуется использовать один экземпляр AsyncDocumentClient для каждого домена в течение жизненного цикла приложения.

   <a id="max-connection"></a>

3. **Настройка ConnectionPolicy**

    При использовании пакета SDK для Async Java служба Azure Cosmos DB выполняет запросы через HTTPS или REST, а значит на них распространяется максимальный стандартный размер пула подключений (1000). В большинстве случаев это значение по умолчанию подходит идеально. Но если у вас большая коллекция, в которую входит много секций, можно использовать параметр setMaxPoolSize, чтобы установить большее число (например, 1500) для максимального размера пула подключений.

4. **Настройка параллельных запросов для секционированных коллекций**

    В пакете SDK Async Java для Azure Cosmos DB SQL поддерживаются параллельные запросы, позволяющие обращаться к секционированным коллекциям в параллельном режиме. Дополнительные сведения см. в [примерах кода](https://github.com/Azure/azure-cosmosdb-java/tree/master/examples/src/test/java/com/microsoft/azure/cosmosdb/rx/examples) для работы с пакетами SDK. Параллельные запросы предназначены для сокращения задержки при обработке запросов и улучшения пропускной способности посредством их последовательных аналогов.

    (а) ***Применение setMaxDegreeOfParallelism.\:*** Параллельный режим запросов позволяет одновременно обращаться к нескольким секциям. При этом данные из каждой секционированной коллекции извлекаются в рамках запроса последовательно. С помощью параметра setMaxDegreeOfParallelism установите значение, соответствующее числу секций, что обеспечит максимальную вероятность высокой производительности запроса при сохранении всех остальных параметров системы. Если вы не знаете число секций, просто используйте высокое значение для параметра setMaxDegreeOfParallelism. Система автоматически выберет минимальное из двух значений: число секций или число, указанное пользователем.

    Следует отметить, что параллельные запросы обеспечивают больше преимуществ, если данные равномерно распределены во всех секциях по отношению к запросу. Если коллекция секционируется таким образом, что все или большинство данных, возвращаемых запросом, содержатся в нескольких секциях (в худшем случае в одной), это негативно скажется на производительности запроса.

    (б) ***Применение setMaxBufferedItemCount.\:*** В параллельном режиме запрос выполняет предварительную выборку результатов, пока клиент обрабатывает уже полученный пакет данных. Предварительная выборка способствует общему уменьшению задержки при обработке запроса. Значение setMaxBufferedItemCount ограничивает число предварительно выбираемых результатов. Указав для параметра setMaxBufferedItemCount ожидаемое число возвращаемых результатов (или еще более высокое значение), вы обеспечите максимальное влияние предварительной выборки на производительность запросов.

    Предварительная выборка работает одинаково при любом значении параметра MaxDegreeOfParallelism. Для данных из всех секций применяется один буфер.

5. **Применение интервала задержки getRetryAfterInMilliseconds.**

    Во время проверки производительности следует увеличивать нагрузку до тех пор, пока регулирование не будет выполняться при небольшой частоте запросов. При регулировании клиентское приложение должно реализовать интервал частоты повтора для частоты повтора сервера. Это гарантирует, что время ожидания между повторными попытками будет минимальным.
6. **Развертывание рабочей нагрузки клиента**

    При проверке с высокой пропускной способностью (более 50 000 единиц запроса в секунду) клиентское приложение может стать узким местом из-за того, что на компьютере будут достигнуты максимальные уровни использования ЦП и сети. Если вы достигли этой точки, то можете повысить производительность Azure Cosmos DB, развернув клиентские приложения на нескольких серверах.

7. **Адресация на основе имен**

    Используйте адресацию на основе имен, то есть ссылки в формате `dbs/MyDatabaseId/colls/MyCollectionId/docs/MyDocumentId` вместо самоссылающегося (\_self) формата `dbs/<database_rid>/colls/<collection_rid>/docs/<document_rid>`. Это позволит избежать получения идентификаторов для всех ресурсов, требуемых для создания ссылки. Кроме того, кэширование этих данных не увеличит производительность, так как ресурсы могут создаваться заново (иногда с теми же именами).

   <a id="tune-page-size"></a>
8. **Повышение производительности путем настройки размера страницы для запросов и каналов чтения**

    Если при массовом чтении документов с помощью функции чтения канала (например, readDocuments) или обработке запроса SQL будет получен слишком большой набор результатов, такие результаты возвращаются частями. По умолчанию результаты возвращаются в пакетах (не более 100 элементов и не более 1 МБ в каждом пакете).

    Чтобы снизить количество сетевых взаимодействий, необходимых для получения всех нужных результатов, попробуйте увеличить размер страницы до 1000 с помощью заголовка запроса [x-ms-max-item-count](https://docs.microsoft.com/rest/api/cosmos-db/common-cosmosdb-rest-request-headers). Чтобы отобразить только некоторые результаты, например, когда пользовательский интерфейс или приложение API возвращает только десять результатов за раз, размер страницы можно уменьшить до 10. Это позволит снизить пропускную способность, используемую на операции чтения и на выполнение запросов.

    Размер страницы также можно изменить с помощью метода setMaxItemCount.

9. **Использование надлежащего планировщика (избегайте перехвата потоков цикла обработки событий ввода-вывода netty)**

    Пакет SDK для Async Java использует [netty](https://netty.io/) для неблокирующихся операций ввода-вывода. В пакете SDK используется фиксированное число потоков цикла обработки событий ввода-вывода netty (это число соответствует числу ядер ЦП на компьютере) для выполнения операций ввода-вывода. Объект Observable, возвращаемый API, выводит результат в один из общих потоков цикла обработки событий ввода-вывода netty. Поэтому важно не блокировать общие потоки цикла обработки событий ввода-вывода netty. Интенсивное использование ЦП или блокировка операции в потоке цикла обработки событий ввода-вывода netty может вызвать взаимоблокировку или значительно уменьшить пропускную способность пакета SDK.

    Например, следующий код выполняет рабочую нагрузку с интенсивной нагрузкой на ЦП в потоке цикла обработки событий ввода-вывода netty.

    ```java
    Observable<ResourceResponse<Document>> createDocObs = asyncDocumentClient.createDocument(
      collectionLink, document, null, true);

    createDocObs.subscribe(
      resourceResponse -> {
        //this is executed on eventloop IO netty thread.
        //the eventloop thread is shared and is meant to return back quickly.
        //
        // DON'T do this on eventloop IO netty thread.
        veryCpuIntensiveWork();
      });
    ```

    Если необходимо выполнить обработку полученного результата с интенсивной нагрузкой на ЦП, этого нежелательно делать в потоке цикла обработки событий ввода-вывода netty. Вместо этого можно указать собственный планировщик, чтобы выполнить рабочую нагрузку, используя собственный поток.

    ```java
    import rx.schedulers;

    Observable<ResourceResponse<Document>> createDocObs = asyncDocumentClient.createDocument(
      collectionLink, document, null, true);

    createDocObs.subscribeOn(Schedulers.computation())
    subscribe(
      resourceResponse -> {
        // this is executed on threads provided by Scheduler.computation()
        // Schedulers.computation() should be used only when:
        //   1. The work is cpu intensive 
        //   2. You are not doing blocking IO, thread sleep, etc. in this thread against other resources.
        veryCpuIntensiveWork();
      });
    ```

    Следует использовать подходящий планировщик RxJava в зависимости от типа рабочей нагрузки. Дополнительные сведения см. здесь: [``Schedulers``](http://reactivex.io/RxJava/1.x/javadoc/rx/schedulers/Schedulers.html).

    Дополнительные сведения о пакете SDK для Async Java см. на этой [странице GitHub](https://github.com/Azure/azure-cosmosdb-java).

10. **Отключение ведения журнала netty** Журналы библиотеки netty загружают ЦП, и их необходимо отключить (отключения журнала в конфигурации может быть недостаточно). Если вы не используете режим отладки, полностью отключите ведение журналов netty. Поэтому, если вы используете log4j для сокращения дополнительных затрат на ЦП в связи с ``org.apache.log4j.Category.callAppenders()`` из netty, добавьте следующую строку в базу кода:

    ```java
    org.apache.log4j.Logger.getLogger("io.netty").setLevel(org.apache.log4j.Level.OFF);
    ```

11. **Лимит на ресурсы открытых файлов ОС**  Некоторые дистрибутивы Linux (например, RedHat) ограничивают максимальное число открытых файлов и общее число подключений. Чтобы узнать текущие ограничения, выполните следующую команду:

    ```bash
    ulimit -a
    ```

    Число открытых файлов (nofile) должно быть достаточно большим, чтобы было достаточно места для настроенного размера пула подключений и других открытых файлов в ОС. Это число можно изменить для включения поддержки пула подключений большего размера.

    Откройте файл limits.conf:

    ```bash
    vim /etc/security/limits.conf
    ```
    
    Добавьте или измените следующие строки:

    ```
    * - nofile 100000
    ```

12. **Использование собственной реализация SSL для netty**  Netty может напрямую использовать OpenSSL, чтобы повысить производительность стека реализации SSL. Если такая конфигурация отсутствует, netty вернется к реализации SSL по умолчанию в Java.

    Для Ubuntu:
    ```bash
    sudo apt-get install openssl
    sudo apt-get install libapr1
    ```

    Также добавьте в проект следующие зависимости maven:
    ```xml
    <dependency>
      <groupId>io.netty</groupId>
      <artifactId>netty-tcnative</artifactId>
      <version>2.0.20.Final</version>
      <classifier>linux-x86_64</classifier>
    </dependency>
    ```

Для других платформ (например, RedHat, Windows, Mac и т. д.) воспользуйтесь следующими инструкциями: https://netty.io/wiki/forked-tomcat-native.html

## <a name="indexing-policy"></a>Политика индексации
 
1. **Увеличение скорости выполнения операций записи путем исключения неиспользуемых путей из индексирования**

    Политика индексирования Azure Cosmos DB позволяет добавлять к индексированию или исключать из индексирования определенные пути к документам, используя механизм Indexing Paths (setIncludedPaths и setExcludedPaths). Возможность управления путями индексирования позволяет оптимизировать производительность записи и снизить затраты на хранение индекса для сценариев с заранее определенными шаблонами запросов. Это связано с тем, что затраты на индексирование непосредственно зависят от числа индексируемых уникальных путей. Например, в приведенном ниже коде показано, как исключить из индексации целый раздел документов (так называемое поддерево) с помощью подстановочного знака "*".

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

    Сложность запроса влияет на количество единиц запроса, потребляемых операцией. Число предикатов и их характер, количество определяемых пользователем функций и размер набора исходных данных — все это влияет на плату за операции запроса.

    Чтобы оценить расходы на любую операцию (создание, обновление или удаление), проверьте значение заголовка [x-ms-request-charge](https://docs.microsoft.com/rest/api/cosmos-db/common-cosmosdb-rest-request-headers). Это значение содержит число единиц запроса, потребляемых соответствующей операцией. Вы также можете просмотреть эквивалентное свойство RequestCharge в ResourceResponse\<t > или FeedResponse\<t >.

    ```Java
    ResourceResponse<Document> response = asyncClient.createDocument(collectionLink, documentDefinition, null,
                                                     false).toBlocking.single();
    response.getRequestCharge();
    ```

    Стоимость запроса, указанная в этом заголовке, учитывается как часть подготовленной пропускной способности. Например, если вы предоставили 2000 единиц запроса в секунду, а приведенный выше запрос возвращает 1000 документов размером по 1 КБ каждый, затраты на операцию составят 1000 единиц. Таким образом, перед ограничением частоты выполнения последующих запросов сервер за одну секунду выполняет только два таких запроса. Чтобы узнать больше, ознакомьтесь с [единицами запроса](request-units.md) и [калькулятором единиц запроса](https://www.documentdb.com/capacityplanner).
<a id="429"></a>
2. **Обработка ограничения скорости / слишком высокая частота запросов**

    Выполнение запроса, который превышает лимит зарезервированной пропускной способности для учетной записи, не приводит к снижению производительности сервера, так как пользователь не сможет превысить это зарезервированное значение. Сервер заранее завершит запрос с ошибкой RequestRateTooLarge (код состояния HTTP: 429) и вернет в заголовке [x-x-ms-retry-after-ms](https://docs.microsoft.com/rest/api/cosmos-db/common-cosmosdb-rest-request-headers) время (в миллисекундах), спустя которое можно повторно выполнить этот запрос.

        HTTP Status 429,
        Status Line: RequestRateTooLarge
        x-ms-retry-after-ms :100

    Пакеты SDK перехватят этот ответ, обработают заголовок retry-after, указанный сервером, и отправят запрос повторно. Если к вашей учетной записи параллельно имеет доступ только один клиент, следующая попытка будет успешной.

    По умолчанию число повторных попыток отправки запроса составляет девять (это значение задается клиентом). Если к вашей учетной записи имеют доступ несколько клиентов и они выполняют запросы одновременно, этого может быть недостаточно. В этом случае клиент выдаст для приложения исключение DocumentClientException с кодом состояния 429. Число повторных попыток по умолчанию можно переопределить в свойстве setRetryOptions экземпляра ConnectionPolicy. По умолчанию при превышении заданного счетчика повторов исключение DocumentClientException с кодом состояния 429 возвращается через 30 секунд (общее время ожидания). Это происходит, даже если текущее значение количества повторных попыток (по умолчанию (9) или определенное пользователем) меньше максимального значения.

    Хотя автоматическая процедура отправки повторного запроса позволяет улучшить устойчивость приложений и повысить удобство работы с ними, она может снизить производительность, что, в свою очередь, станет причиной появления более длительных задержек. Если настройка производительности повлияла на регулирование сервера и стала причиной автоматической отправки запросов пакетом SDK, это может стать причиной появления пиков задержек на стороне клиента. Чтобы избежать пиков задержек во время настройки производительности, проверьте расход ресурсов на каждую операцию и убедитесь, что значение частоты запросов не превышено. Дополнительные сведения см. в статье [Единицы запросов в DocumentDB](request-units.md).
3. **Использование меньших документов для более высокой пропускной способности**

    Стоимость запроса (плата за обработку запроса) для каждой операции напрямую зависит от размера документа. За операции с большими документами взимается больше единиц запроса, чем за операции с мелкими документами.

## <a name="next-steps"></a>Дальнейшие действия
Дополнительные сведения о создании приложения с высокой масштабируемостью и производительностью см. в статье [Partitioning and scaling in Azure Cosmos DB](partition-data.md) (Секционирование и масштабирование в Azure Cosmos DB).
