---
title: Работа с поддержкой веб-канала изменений в Azure Cosmos DB
description: Используйте поддержку веб-канала изменений Azure Cosmos DB, чтобы отслеживать изменения в документах и выполнять обработку на основе событий, например триггеров, и поддерживать кэши и аналитические системы в обновленном состоянии.
author: rimman
ms.author: rimman
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 07/23/2019
ms.reviewer: sngun
ms.custom: seodec18
ms.openlocfilehash: 40caea40637c57aedb6315ff6fc032898ff07af7
ms.sourcegitcommit: c72ddb56b5657b2adeb3c4608c3d4c56e3421f2c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/24/2019
ms.locfileid: "68467948"
---
# <a name="change-feed-in-azure-cosmos-db---overview"></a>Канал изменений в Azure Cosmos DB. Обзор

Канал изменений, поддерживаемый в Azure Cosmos DB, прослушивает изменения в контейнере Azure Cosmos DB. Затем он выводит отсортированный список документов в порядке, в котором они были изменены. Изменения сохраняются и обрабатываются асинхронно и пошагово, а выходные данные могут распределяться в один или несколько объектов-получателей для параллельной обработки. 

Azure Cosmos DB используется для приложений Интернета вещей, розничной торговли, игр и приложений для ведения журнала операций. Для таких приложений часто используется одинаковый конструктивный шаблон, в котором изменения в данных активируют дополнительные действия, например:

* активирование уведомления или вызова API при вставке или изменении элемента;
* потоковая обработка в режиме реального времени (для Интернета вещей) или аналитическая обработка в режиме реального времени по рабочим данным;
* дополнительное перемещение данных путем их синхронизации с кэшем, поисковой системой или хранилищем данных либо архивирование данных в холодное хранилище.

Канал изменений в Azure Cosmos DB позволяет создавать эффективные и масштабируемые решения для каждого из этих шаблонов, как показано на следующем рисунке:

![Использование веб-канала изменений Azure Cosmos DB для аналитики в реальном времени и вычислений на основе событий](./media/change-feed/changefeedoverview.png)

## <a name="supported-apis-and-client-sdks"></a>Поддерживаемые API и клиентские пакеты SDK

Этот компонент сейчас поддерживается следующими API Cosmos DB и клиентскими пакетами SDK:

| **Клиентские драйверы**; | **Интерфейс командной строки Azure** | **API SQL**; | **API Cassandra**; | **API Azure Cosmos DB для MongoDB** | **API Gremlin**;|**API таблицы**; |
| --- | --- | --- | --- | --- | --- | --- |
| .NET | Н/Д | Да | Нет | Нет | Да | Нет |
|Java|Н/Д|Да|Нет|Нет|Да|Нет|
|Python|Н/Д|Да|Нет|Нет|Да|Нет|
|Node/JS|Н/Д|Да|Нет|Нет|Да|Нет|

## <a name="change-feed-and-different-operations"></a>Канал изменений и разные операции

Сейчас в канале изменений отображаются все операции. Пока недоступны функции, позволяющие управлять каналом изменений для выбора конкретных операций, например включать обновления и исключать вставки. Вы можете помечать элементы данных об обновлениях специальным маркером, чтобы фильтровать по нему элементы при обработке канала изменений. Сейчас в канале изменений не записываются операции удаления. Как и в предыдущем примере, вы можете добавить маркер для удаляемых элементов, например создать в элементе специальный атрибут deleted, присвоить ему значение TRUE и задать срок жизни, чтобы реализовать автоматическое удаление элементов. По каналу изменений можно получать элементы за предыдущие периоды, например, все добавленные пять лет назад элементы. Если нужный элемент не был удален, данные из канала изменений можно получать с самого момента создания контейнера.

### <a name="sort-order-of-items-in-change-feed"></a>Порядок сортировки элементов в канале изменений

Элементы в канале изменений упорядочены по времени их изменения. Этот порядок сортировки обеспечивается только в пределах ключа логического раздела.

### <a name="change-feed-in-multi-region-azure-cosmos-accounts"></a>Канал изменений для учетных записей Azure Cosmos с поддержкой нескольких регионов

В учетной записи Azure Cosmos с поддержкой нескольких регионов действует следующее правило: если для региона записи выполнена отработка отказа, канал изменений учитывает отработку отказа вручную и сохраняет непрерывность.

### <a name="change-feed-and-time-to-live-ttl"></a>Канал изменений и срок жизни (TTL)

Если для элемента свойство TTL (Срок жизни) имеет значение -1, канал изменений будет сохраняться без ограничения по времени. Если данные не удаляются, они будут храниться в канале изменений.  

### <a name="change-feed-and-etag-lsn-or-ts"></a>Канал изменений и теги _etag, _lsn или _ts

Формат _etag считается внутренним, и его не следует применять для важных элементов, так как он может измениться в любое время. Тег _ts содержит метку времени последнего изменения или создания. Этот идентификатор можно использовать для сравнения в хронологическом порядке. _lsn — это идентификатор пакета, который добавляется только для веб-канала изменений; Он представляет идентификатор транзакции. Значение _lsn будет одинаковым для многих элементов. ETag для FeedResponse отличается от eTag для элементов. Тег_etag — это внутренний идентификатор, который используется для управления параллелизмом как метка актуальной версии элемента, тогда как ETag используется для упорядочивания канала.

## <a name="change-feed-use-cases-and-scenarios"></a>Варианты использования и сценарии для канала изменений

Канал изменений позволяет эффективно обрабатывать большие наборы данных с большим объемом операций записи. Также канал изменений позволяет обойтись без запросов ко всему набору данных, когда нужно найти только изменившиеся данные.

### <a name="use-cases"></a>Варианты использования

С помощью канала изменений можно эффективно выполнять следующие задачи:

* обновление кэша, индекса поиска или хранилища данных, используя хранимые в Azure Cosmos DB данные;

* реализация распределения по уровням и архивирования данных на уровне приложений, например хранение "горячих" данных в Azure Cosmos DB и перенос устаревших "холодных" данных в другое хранилище, например [хранилище BLOB-объектов Azure](../storage/common/storage-introduction.md);

* выполнение миграции без простоя в другую учетную запись Azure Cosmos или другой контейнер Azure Cosmos с другим ключом логического раздела;

* реализация на основе Azure Cosmos DB [лямбда-архитектуры](https://blogs.technet.microsoft.com/msuspartner/2016/01/27/azure-partner-community-big-data-advanced-analytics-and-lambda-architecture/), в рамках которой Azure Cosmos DB поддерживает уровни обработки в реальном времени, пакетной обработки и обслуживания запросов. Это позволяет создать лямбда-архитектуру с низкой совокупной стоимостью владения;

* получение и хранение данных о событиях от устройств, датчиков, инфраструктуры и приложений и обработка этих событий в реальном времени, например с помощью [Spark](../hdinsight/spark/apache-spark-overview.md).  Ниже показано, как реализовать лямбда-архитектуру на основе Azure Cosmos DB с помощью канала изменений:

![Лямбда-конвейер для приема и запроса на основе Azure Cosmos DB](./media/change-feed/lambda.png)

### <a name="scenarios"></a>Сценарии

Ниже описаны несколько сценариев, которые можно легко реализовать с помощью канала изменений.

* В [бессерверных](https://azure.microsoft.com/solutions/serverless/) веб-приложениях и мобильных приложениях вы можете отслеживать события, такие как изменения профиля, параметров или расположения, а также активировать определенные действия, например отправлять push-уведомления на соответствующие устройства с помощью [Функций Azure](change-feed-functions.md).

* При использовании Azure Cosmos DB для создания игр веб-канал изменений можно использовать, например, для создания списков лидеров в режиме реального времени на основе очков в завершенных играх.


## <a name="working-with-change-feed"></a>Работа с каналом изменений

Для работы с каналом изменений у вас есть следующие варианты:

* [Как использовать канал изменений Azure Cosmos DB с Функциями Azure](change-feed-functions.md)
* [Использование канала изменений с обработчиком веб-канала изменений](change-feed-processor.md) 

Канал изменений доступен для каждого ключа логических разделов в контейнере, и его можно распределить для одного или нескольких потребителей для параллельной обработки, как показано на следующем рисунке.

![Распределенная обработка веб-канала изменений Azure Cosmos DB](./media/change-feed/changefeedvisual.png)

## <a name="features-of-change-feed"></a>Функции канала изменений

* Канал изменений по умолчанию включен для всех учетных записей Azure Cosmos.

* Вы можете использовать свою [подготовленную пропускную способность](request-units.md) для чтения из канала изменений, как и для любой другой операции Azure Cosmos DB, в любом регионе, настроенном для базы данных Azure Cosmos.

* Канал изменений поддерживает операции вставки и обновления, выполняемые по отношению к элементам в контейнере. Вы можете отслеживать операции удаления, используя вместо прямого удаления флаг обратимого удаления в элементах (например, документах). Или же можно установить ограниченный срок жизни элементов, используя настройки [срока жизни](time-to-live.md). Например, установите значение 24 часа и отслеживайте удаления по значению этого свойства. Эта возможность позволяет обрабатывать изменения быстрее, чем с использованием срока жизни. 

* Каждое изменение в документе отображается в канале изменений ровно один раз, а логикой контрольных точек управляют клиенты. Если вы хотите избежать сложности управления контрольными точками, обработчик веб-канала изменений обеспечивает автоматическую контрольную точку и "хотя бы один раз" семантику. См. раздел [Использование веб-канала изменений с обработчиком веб-канала изменений](change-feed-processor.md).

* В журнал изменений вносится только самое последнее изменение в конкретном элементе. Промежуточные изменения могут быть недоступны.

* Канал изменений сортируется по времени изменения в пределах каждого значения ключа логических разделов. Для значений ключа разделов не четкий порядок не предусмотрен.

* Изменения можно синхронизировать в любой момент времени. Это означает, что фиксированный период хранения данных, в пределах которого доступны изменения, отсутствует.

* Изменения поступают параллельно для всех ключей логических разделов из контейнера Azure Cosmos. Это позволяет нескольким потребителям параллельно обрабатывать изменения из больших контейнеров.

* Приложения могут одновременно запрашивать несколько каналов изменения в одном контейнере. Свойство ChangeFeedOptions.StartTime позволяет определить начальную точку. Например, так можно найти токен продолжения за определенный период времени. Если значение ContinuationToken указано, оно имеет приоритет перед значениями StartTime и StartFromBeginning. Точность ChangeFeedOptions.StartTime равняется приблизительно 5 секундам. 

## <a name="next-steps"></a>Следующие шаги

Вы можете продолжить знакомство с каналом изменений, перейдя к следующим статьям:

* [Reading Azure Cosmos DB change feed](read-change-feed.md) (Чтение канала изменений Azure Cosmos DB)
* [Как использовать канал изменений Azure Cosmos DB с Функциями Azure](change-feed-functions.md)
* [Использование обработчика веб-канала изменений](change-feed-processor.md)
