---
title: Выходные данные Azure Stream Analytics в Cosmos DB
description: Из этой статьи вы узнаете, как с помощью Azure Stream Analytics сохранять выходные данные в Azure Cosmos DB в формате JSON, что позволяет архивировать данные и уменьшать задержки запросов в отношении неструктурированных данных JSON.
services: stream-analytics
author: mamccrea
ms.author: mamccrea
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 01/11/2019
ms.custom: seodec18
ms.openlocfilehash: de5febaeecd176a8718364720132d3fa4433c57f
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/28/2019
ms.locfileid: "67443623"
---
# <a name="azure-stream-analytics-output-to-azure-cosmos-db"></a>Выходные данные Azure Stream Analytics в Azure Cosmos DB  
Stream Analytics позволяет направлять данные из [Azure Cosmos DB](https://azure.microsoft.com/services/documentdb/) в формат JSON, позволяя архивировать данные и уменьшать задержки запросов в отношении неструктурированных данных JSON. В этом документе представлены некоторые рекомендации по реализации данной конфигурации.

Тем, кто не знаком с Cosmos DB, мы рекомендуем просмотреть [схему обучения работе с Azure Cosmos DB](https://azure.microsoft.com/documentation/learning-paths/documentdb/). 

> [!Note]
> В настоящее время Azure Stream Analytics поддерживает соединение только с Azure Cosmos DB при помощи **SQL API**.
> Другие API Azure Cosmos DB в данный момент не поддерживаются. Если указать модулю Azure Stream Analytics учетные записи Azure Cosmos DB, созданные при помощи других API, это может привести к неправильному сохранению данных. 

## <a name="basics-of-cosmos-db-as-an-output-target"></a>Основные сведения о Cosmos DB как объекте вывода данных
Выходные данные Azure Cosmos DB в Stream Analytics позволяет записывать результаты обработки как выходные данные JSON в вашей Cosmos DB контейнеры потока. Stream Analytics не создает контейнеры в базе данных, вместо необходимости создать их заранее. Таким образом, чтобы вы управляются выставление счетов контейнеров Cosmos DB, и того, вы можете регулировать производительность, согласованность и емкость напрямую с помощью контейнеров [интерфейсов API Cosmos DB](https://msdn.microsoft.com/library/azure/dn781481.aspx).

> [!Note]
> Необходимо добавить 0.0.0.0 в список разрешенных IP-адресов брандмауэра Azure Cosmos DB.

Ниже приведены некоторые из параметров контейнер Cosmos DB.

## <a name="tune-consistency-availability-and-latency"></a>Настройка согласованности, доступности и задержки
Для соответствия требованиям вашего приложения, Azure Cosmos DB позволяет точно настроить базы данных и контейнеров и отрегулировать уровень согласованности, доступности, задержки и пропускной способности. В зависимости от того, какие уровни согласованности чтения потребуются вашему сценарию для чтения и записи задержки, вы можете выбирать уровень согласованности в своей учетной записи базы данных. Пропускную способность можно повысить, увеличив масштаб Units(RUs) запроса для контейнера. Также по умолчанию Azure Cosmos DB активирует синхронное индексирование для каждой операции CRUD в контейнер. Это еще один полезный параметр для контроля производительности операций чтения и записи в Azure Cosmos DB. Дополнительные сведения см. в статье об [изменении уровней согласованности в для базы данных и запросов](../cosmos-db/consistency-levels.md).

## <a name="upserts-from-stream-analytics"></a>Вставка и обновление Upsert в Stream Analytics
Интеграция Stream Analytics с Azure Cosmos DB позволяет вставлять или обновлять записи в контейнере на основе заданного столбца Идентификаторов документов. Этот процесс называется также *Upsert*.

Stream Analytics использует оптимистичный подход Upsert, при котором обновления выполняются только тогда, когда операция вставки завершается ошибкой из-за конфликта с идентификатором документа. С помощью 1.0 уровень совместимости это обновление выполняется как исправление, поэтому оно допускает частичное обновление к документу, то есть, добавление новых свойств или замена существующего свойства выполняется постепенно. Тем не менее изменение значений свойств массива в документе JSON приводит к перезаписи всего массива, то есть массивы не объединяются. С версии 1.2 поведение upsert изменяется для вставки или замены документа. Это описано далее в разделе 1.2 уровень совместимости ниже.

Если входящий документ JSON содержит существующее поле идентификатора, это поле автоматически используется в качестве столбца идентификаторов документов в Cosmos DB и все последующие операции записи обрабатываются следующим образом:
- для уникальных идентификаторов выполняется вставка;
- если для повторяющихся идентификаторов и параметра Document ID задано значение "ID", выполняется операция upsert;
- если для повторяющихся идентификаторов и параметра Document ID не задано значение, после первого документа возникает ошибка.

Если вы хотите сохранить <i>все</i> документы, включая документы с повторяющимся идентификатором, переименуйте поле идентификатора в запросе (с ключевым словом AS) и разрешите Cosmos DB автоматически создавать поле идентификатора или заменять идентификатор значением из другого столбца (с помощью ключевого слова AS или с помощью параметра Document ID).

## <a name="data-partitioning-in-cosmos-db"></a>Секционирование данных в Cosmos DB
[Неограниченные](../cosmos-db/partition-data.md) контейнеры для Azure Cosmos DB — это рекомендуемый способ секционирования данных, так как Azure Cosmos DB автоматически масштабирует разделы на основе рабочей нагрузки. При записи в неограниченные контейнеры Stream Analytics используется столько же параллельных модулей записи, сколько и на предыдущем шаге запроса или в схеме разбиения входных данных.
> [!NOTE]
> В настоящее время Azure Stream Analytics поддерживает только контейнеров с неограниченным хранилищем с помощью ключей разделов верхнего уровня. Например, `/region` поддерживается. Вложенные ключи разделов (например, `/region/name`) не поддерживаются. 

В зависимости от выбранный ключ секции может появиться это _предупреждение_:

`CosmosDB Output contains multiple rows and just one row per partition key. If the output latency is higher than expected, consider choosing a partition key that contains at least several hundred records per partition key.`

Важно выбрать свойство ключа секции, имеет ряд уникальных значений, который позволяет равномерно распределять рабочую нагрузку по этим значениям. В качестве артефакта естественным секционирования запросы, связанные с одинаковым ключом секции ограничиваются максимальная пропускная способность одной секции. Кроме того размер хранилища для документов, принадлежащий тому же ключу секции ограничено до 10 ГБ. Идеальный ключ секции — это тот, который часто отображается в качестве фильтра в запросах и обеспечивает достаточную кратность, чтобы гарантировать масштабируемость решения.

Ключ секции — это также граница для транзакций в хранимых процедур и триггеров в DocumentDB. Таким образом, документы, которые возникают вместе в транзакциях тем же значением ключа секции, необходимо выбрать ключ секции. Статья [секционирование и масштабирование в Cosmos DB](../cosmos-db/partitioning-overview.md) позволяет получить дополнительную информацию о выборе ключа секции.

Для основных коллекций Azure Cosmos DB Stream Analytics позволяет невозможно увеличить или уменьшить, когда в них полно. Верхний предел их пропускной способности: 10 ГБ и 10 000 ЕЗ/с.  Чтобы перенести данные из контейнера фиксированного размера в контейнер неограниченного размера (например, с ключом секции и пропускной способностью не менее 1000 ЕЗ/с), вам нужно использовать [средство миграции данных](../cosmos-db/import-data.md) или [библиотеку канала изменений](../cosmos-db/change-feed.md).

Права на запись в несколько основных контейнеров рекомендуется и не рекомендуется для масштабирования к заданию Stream Analytics.

## <a name="improved-throughput-with-compatibility-level-12"></a>Повышенная пропускная способность с 1.2 уровень совместимости
При уровне совместимости 1.2 естественная интеграция Stream Analytics поддерживает массовое записи в Cosmos DB. Это позволяет эффективно запись Cosmos DB с помощью максимальное увеличение пропускной способности и эффективно запросы на регулирование дескриптор. Улучшенная записи механизм доступен в разделе на новый уровень совместимости, из-за разницы поведения upsert.  Прежде чем 1.2 поведение upsert — Вставка или слияние документа. С версии 1.2 поведение операции Upsert изменяется для вставки или замены документа. 

Прежде чем 1.2 использует пользовательскую хранимую процедуру для массовой вставки-обновления документов на ключ секции в Cosmos DB, куда будут записываться пакет как транзакцию. Даже в том случае, когда единственную запись достигает временная ошибка (регулирования), должна быть повторена весь пакет. Это затрудняло сценариев с помощью даже разумным регулирования относительно медленнее. Выполнив сравнение показано, как будет вести себя такие задания с версии 1.2.

В следующем примере показано две идентичные задания Stream Analytics, чтение же входных данных концентратора событий. Оба задания Stream Analytics, [полностью секционированный](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-parallelization#embarrassingly-parallel-jobs) запрос к серверу и записи для идентичных контейнеров, в CosmosDB. Метрики в левой части можно использовать из задания, настроенного с уровнем совместимости 1.0 и тех, которые в правой части должны быть настроены 1.2. Ключ секции контейнер Cosmos DB — уникальный идентификатор GUID, поступающих из входного события.

![Сравнение метрик Stream analytics](media/stream-analytics-documentdb-output/stream-analytics-documentdb-output-3.png)

Интенсивность поступления событий в концентраторе событий имеет два раза выше, чем контейнеров Cosmos DB (20 тысяч RUs) настроены для принимать, поэтому регулирования ожидается в Cosmos DB. Тем не менее задание с версии 1.2, постоянно производит запись в более высокую пропускную способность (выходные данные событий в минуту), используя ниже среднего использования единиц потоковой Передачи-%. В вашей среде эта разница будет зависеть от несколько дополнительных факторов, таких как выбор формат событий, размер входного события и сообщения, ключи секции, запроса и т.д.

![Сравнение метрик Cosmos db](media/stream-analytics-documentdb-output/stream-analytics-documentdb-output-2.png)

С версии 1.2 Stream Analytics более разумно использовать 100% доступная пропускная способность в Cosmos DB с очень мало resubmissions из ограничения регулирования частоты. Это обеспечивает удобство работы для других рабочих нагрузок, таких как запросы, выполняемые в контейнере, в то же время. Если вам нужно опробовать как ASA масштабируется с помощью Cosmos DB как приемник для 1-10 тысяч сообщений/сек, вот [проект azure образцов](https://github.com/Azure-Samples/streaming-at-scale/tree/master/eventhubs-streamanalytics-cosmosdb) позволяет сделать.
Обратите внимание на то, что пропускная способность вывода Cosmos DB идентичен 1.0 и 1.1. Поскольку 1.2 в настоящее время не используется по умолчанию, вы можете [установить уровень совместимости](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-compatibility-level) для задания Stream Analytics с помощью портала или с помощью [создания вызова REST API задания](https://docs.microsoft.com/rest/api/streamanalytics/stream-analytics-job). Он имеет *настоятельно рекомендуется* использование 1.2 уровень совместимости в ASA с помощью Cosmos DB. 



## <a name="cosmos-db-settings-for-json-output"></a>Параметры Cosmos DB для выходных данных JSON

При создании Cosmos DB как средства обработки выходных данных в Stream Analytics создается запрос информации, показанный ниже. В этом разделе объясняются определения свойств.

![экран выходных данных documentdb stream analytics](media/stream-analytics-documentdb-output/stream-analytics-documentdb-output-1.png)

|Поле           | Описание|
|-------------   | -------------|
|Псевдоним выходных данных    | Псевдоним для ссылки на эти выходные данные в запросе ASA.|
|Подписка    | Выберите подписку Azure.|
|Идентификатор учетной записи      | Имя или универсальный код ресурса (URI) конечной точки учетной записи Azure Cosmos DB.|
|Ключ учетной записи     | Общедоступный ключ доступа к учетной записи Azure Cosmos DB.|
|База данных        | Имя базы данных Azure Cosmos DB.|
|Имя контейнера | Имя контейнера для использования. `MyContainer` — Это пример допустимых входных данных — один контейнер с именем `MyContainer` должен существовать.  |
|Идентификатор документа     | Необязательный элемент. Имя столбца в выходных событиях используется как уникальный ключ, на котором должны основываться операции вставки или обновления. Если оставить это поле пустым, все события будут вставлены, без возможности обновления.|
