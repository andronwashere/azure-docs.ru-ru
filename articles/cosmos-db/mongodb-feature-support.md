---
title: Использование API Azure Cosmos DB для поддержки возможностей MongoDB
description: Сведения о возможностях MongoDB 3.4, которые предоставляет API Azure Cosmos DB для MongoDB.
ms.service: cosmos-db
ms.subservice: cosmosdb-mongo
ms.topic: overview
ms.date: 05/21/2019
author: sivethe
ms.author: sivethe
ms.openlocfilehash: 09d12c817e9e8678cf96c63c0bff623923f7074d
ms.sourcegitcommit: 59fd8dc19fab17e846db5b9e262a25e1530e96f3
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/21/2019
ms.locfileid: "65978837"
---
# <a name="azure-cosmos-dbs-api-for-mongodb-supported-features-and-syntax"></a>API Azure Cosmos DB для MongoDB — поддержка возможностей и синтаксиса

Azure Cosmos DB — это глобально распределенная многомодельная служба базы данных Майкрософт. С API Azure Cosmos DB для MongoDB можно взаимодействовать, используя любой из [драйверов](https://docs.mongodb.org/ecosystem/drivers) с открытым кодом для клиента MongoDB. API Azure Cosmos DB для MongoDB позволяет использовать имеющиеся драйверы клиента благодаря [сетевому протоколу](https://docs.mongodb.org/manual/reference/mongodb-wire-protocol) MongoDB.

С помощью API Azure Cosmos DB для MongoDB вы можете пользоваться всеми функциями MongoDB, к которым вы привыкли, и возможностями Cosmos DB для организаций: [глобальное распространение](distribute-data-globally.md), [автоматическое сегментирование](partition-data.md), гарантии по уровням доступности и задержкам, автоматическое индексирование каждого поля, шифрование хранимых данных, резервное копирование и многое другое.

## <a name="protocol-support"></a>Поддержка протоколов

API Azure Cosmos DB для MongoDB совместим с сервером MongoDB версии **3.2** по умолчанию. Ниже перечислены поддерживаемые операторы, а также ограничения и исключения. Функции или операторы запросов, добавленные в MongoDB версии **3.4**, сейчас доступны в режиме предварительной версии. Любой драйвер клиента, который распознает эти протоколы, должен иметь возможность подключения к API Azure Cosmos DB для MongoDB.

[Конвейер агрегирования MongoDB](#aggregation-pipeline) сейчас доступен как отдельный компонент в режиме предварительной версии.

## <a name="query-language-support"></a>Поддержка языка запросов

API Azure Cosmos DB для MongoDB предоставляет полную поддержку всех конструкций языка запросов MongoDB. Ниже приводится подробный список поддерживаемых операций, операторов, этапов, команд и параметров.

## <a name="database-commands"></a>Команды базы данных

API Azure Cosmos DB для MongoDB поддерживает приведенные ниже команды базы данных.

### <a name="query-and-write-operation-commands"></a>Команды для запросов и записи
- удалить
- поиск
- findAndModify
- getLastError
- getMore
- insert
- обновить

### <a name="authentication-commands"></a>Команды для аутентификации
- logout
- authenticate
- getnonce

### <a name="administration-commands"></a>Команды для администрирования
- dropDatabase
- listCollections
- drop
- create
- filemd5
- createIndexes
- listIndexes
- dropIndexes
- connectionStatus
- reIndex

### <a name="diagnostics-commands"></a>Команды для диагностики
- buildInfo
- collStats
- dbStats
- hostInfo
- listDatabases
- whatsmyuri

<a name="aggregation-pipeline"/>

## <a name="aggregation-pipelinea"></a>Конвейер статистической обработки</a>

Cosmos DB поддерживает конвейер статистической обработки в режиме общедоступной предварительной версии. В [блоге Azure](https://aka.ms/mongodb-aggregation) вы найдете инструкции по освоению этой общедоступной предварительной версии.

### <a name="aggregation-commands"></a>Команды статистической обработки
- aggregate
- count
- distinct

### <a name="aggregation-stages"></a>Этапы статистической обработки
- $project
- $match
- $limit
- $skip
- $unwind
- $group
- $sample
- $sort
- $lookup
- $out
- $count
- $addFields

### <a name="aggregation-expressions"></a>Статистические выражения

#### <a name="boolean-expressions"></a>Логические выражения
- $and
- $or
- $not

#### <a name="set-expressions"></a>Выражения для наборов
- $setEquals
- $setIntersection
- $setUnion
- $setDifference
- $setIsSubset
- $anyElementTrue
- $allElementsTrue

#### <a name="comparison-expressions"></a>Выражения сравнения
- $cmp
- $eq
- $gt
- $gte
- $lt
- $lte
- $ne

#### <a name="arithmetic-expressions"></a>Арифметические выражения
- $abs
- $add
- $ceil
- $divide
- $exp
- $floor
- $ln
- $log
- $log10
- $mod
- $multiply
- $pow
- $sqrt
- $subtract
- $trunc

#### <a name="string-expressions"></a>Строковые выражения
- $concat
- $indexOfBytes
- $indexOfCP
- $split
- $strLenBytes
- $strLenCP
- $strcasecmp
- $substr
- $substrBytes
- $substrCP
- $toLower
- $toUpper

#### <a name="array-expressions"></a>Выражения для массивов
- $arrayElemAt
- $concatArrays
- $filter
- $indexOfArray
- $isArray
- $range
- $reverseArray
- $size
- $slice
- $in

#### <a name="date-expressions"></a>Выражения для дат
- $dayOfYear
- $dayOfMonth
- $dayOfWeek
- $year
- $month
- $week
- $hour
- $minute
- $second
- $millisecond
- $isoDayOfWeek
- $isoWeek

#### <a name="conditional-expressions"></a>Условные выражения
- $cond
- $ifNull

## <a name="aggregation-accumulators"></a>Операторы накопления статистической обработки
- $sum
- $avg
- $first
- $last
- $max
- $min
- $push
- $addToSet

## <a name="operators"></a>Операторы

Ниже представлен список поддерживаемых операторов с примерами их использования. Изучите пример документа, используемый в приведенных ниже запросах.

```json
{
  "Volcano Name": "Rainier",
  "Country": "United States",
  "Region": "US-Washington",
  "Location": {
    "type": "Point",
    "coordinates": [
      -121.758,
      46.87
    ]
  },
  "Elevation": 4392,
  "Type": "Stratovolcano",
  "Status": "Dendrochronology",
  "Last Known Eruption": "Last known eruption from 1800-1899, inclusive"
}
```

Оператор | Пример |
--- | --- |
$eq | `{ "Volcano Name": { $eq: "Rainier" } }` |  | -
$gt | `{ "Elevation": { $gt: 4000 } }` |  | -
$gte | `{ "Elevation": { $gte: 4392 } }` |  | -
$lt | `{ "Elevation": { $lt: 5000 } }` |  | -
$lte | `{ "Elevation": { $lte: 5000 } }` | | -
$ne | `{ "Elevation": { $ne: 1 } }` |  | -
$in | `{ "Volcano Name": { $in: ["St. Helens", "Rainier", "Glacier Peak"] } }` |  | -
$nin | `{ "Volcano Name": { $nin: ["Lassen Peak", "Hood", "Baker"] } }` | | -
$or | `{ $or: [ { Elevation: { $lt: 4000 } }, { "Volcano Name": "Rainier" } ] }` |  | -
$and | `{ $and: [ { Elevation: { $gt: 4000 } }, { "Volcano Name": "Rainier" } ] }` |  | -
$not | `{ "Elevation": { $not: { $gt: 5000 } } }`|  | -
$nor | `{ $nor: [ { "Elevation": { $lt: 4000 } }, { "Volcano Name": "Baker" } ] }` |  | -
$exists | `{ "Status": { $exists: true } }`|  | -
$type | `{ "Status": { $type: "string" } }`|  | -
$mod | `{ "Elevation": { $mod: [ 4, 0 ] } }` |  | -
$regex | `{ "Volcano Name": { $regex: "^Rain"} }`|  | -

### <a name="notes"></a>Примечания

В запросах $regex выражения, привязанные слева, поддерживают поиск по индексу. Но если добавить модификатор i (отключение учета регистра) и m (многостроковое выражение), коллекция будет проверяться во всех выражениях.
Если есть необходимость включить $ или |, желательно создать два (или более) запроса regex. Например, исходный запрос вида ```find({x:{$regex: /^abc$/})``` следует изменить следующим образом: ```find({x:{$regex: /^abc/, x:{$regex:/^abc$/}})```.
Тогда первая часть будет использовать индекс, чтобы ограничить поиск только теми документами, имя которых начинается со стороки ^abc, а вторая часть будет проверять соответствие строк. Оператор вертикальной черты | действует как функция or. Например, запрос ```find({x:{$regex: /^abc|^def/})``` отбирает документы, в которых значение поля x начинается со строк abc или def. Чтобы использовать индекс, мы рекомендуем разбивать запрос на два разных запроса, соединенных оператором $or, вот так: ```find( {$or : [{x: $regex: /^abc/}, {$regex: /^def/}] })```.

### <a name="update-operators"></a>Обновление операторов

#### <a name="field-update-operators"></a>Операторы обновления полей
- $inc
- $mul
- $rename
- $setOnInsert
- $set
- $ не задано
- $min
- $max
- $currentDate

#### <a name="array-update-operators"></a>Операторы обновления массивов
- $addToSet
- $pop
- $pullAll
- $pull (примечание: $pull с условием не поддерживается)
- $pushAll
- $push
- $each
- $slice
- $sort
- $position

#### <a name="bitwise-update-operator"></a>Оператор побитового обновления
- $bit

### <a name="geospatial-operators"></a>Геопространственные операторы

Оператор | Пример | |
--- | --- | --- |
$geoWithin | ```{ "Location.coordinates": { $geoWithin: { $centerSphere: [ [ -121, 46 ], 5 ] } } }``` | Yes |
$geoIntersects |  ```{ "Location.coordinates": { $geoIntersects: { $geometry: { type: "Polygon", coordinates: [ [ [ -121.9, 46.7 ], [ -121.5, 46.7 ], [ -121.5, 46.9 ], [ -121.9, 46.9 ], [ -121.9, 46.7 ] ] ] } } } }``` | Yes |
$near | ```{ "Location.coordinates": { $near: { $geometry: { type: "Polygon", coordinates: [ [ [ -121.9, 46.7 ], [ -121.5, 46.7 ], [ -121.5, 46.9 ], [ -121.9, 46.9 ], [ -121.9, 46.7 ] ] ] } } } }``` | Yes |
$nearSphere | ```{ "Location.coordinates": { $nearSphere : [ -121, 46  ], $maxDistance: 0.50 } }``` | Yes |
$geometry | ```{ "Location.coordinates": { $geoWithin: { $geometry: { type: "Polygon", coordinates: [ [ [ -121.9, 46.7 ], [ -121.5, 46.7 ], [ -121.5, 46.9 ], [ -121.9, 46.9 ], [ -121.9, 46.7 ] ] ] } } } }``` | Yes |
$minDistance | ```{ "Location.coordinates": { $nearSphere : { $geometry: {type: "Point", coordinates: [ -121, 46 ]}, $minDistance: 1000, $maxDistance: 1000000 } } }``` | Yes |
$maxDistance | ```{ "Location.coordinates": { $nearSphere : [ -121, 46  ], $maxDistance: 0.50 } }``` | Yes |
$center | ```{ "Location.coordinates": { $geoWithin: { $center: [ [-121, 46], 1 ] } } }``` | Yes |
$centerSphere | ```{ "Location.coordinates": { $geoWithin: { $centerSphere: [ [ -121, 46 ], 5 ] } } }``` | Yes |
$box | ```{ "Location.coordinates": { $geoWithin: { $box:  [ [ 0, 0 ], [ -122, 47 ] ] } } }``` | Yes |
$polygon | ```{ "Location.coordinates": { $near: { $geometry: { type: "Polygon", coordinates: [ [ [ -121.9, 46.7 ], [ -121.5, 46.7 ], [ -121.5, 46.9 ], [ -121.9, 46.9 ], [ -121.9, 46.7 ] ] ] } } } }``` | Yes |

## <a name="sort-operations"></a>Сортировать операции
При использовании операции `findOneAndUpdate` поддерживается сортировка операций по одному полю, а по нескольким полям — нет.

## <a name="additional-operators"></a>Дополнительные операторы

Оператор | Пример | Примечания 
--- | --- | --- |
$all | ```{ "Location.coordinates": { $all: [-121.758, 46.87] } }``` | 
$elemMatch | ```{ "Location.coordinates": { $elemMatch: {  $lt: 0 } } }``` |  
$size | ```{ "Location.coordinates": { $size: 2 } }``` | 
$comment |  ```{ "Location.coordinates": { $elemMatch: {  $lt: 0 } }, $comment: "Negative values"}``` | 
$text |  | Не поддерживается. Вместо этого используйте $regex

## <a name="unsupported-operators"></a>Неподдерживаемые операторы

Операторы ```$where``` и ```$eval``` не поддерживаются в Azure Cosmos DB.

### <a name="methods"></a>Методы

Поддерживаются следующие методы:

#### <a name="cursor-methods"></a>Методы курсора

Метод | Пример | Примечания 
--- | --- | --- |
cursor.sort() | ```cursor.sort({ "Elevation": -1 })``` | Документы без ключа сортировки не возвращаются

## <a name="unique-indexes"></a>Уникальные индексы

По умолчанию Cosmos DB индексирует каждое поле в документах, сохраняемых в базе данных. Уникальные индексы гарантируют, что среди всех документов в коллекции не будет повторяющихся значений определенного поля, то есть для него сохраняется уникальность, как для ключа по умолчанию _id. Вы можете создать в Cosmos DB пользовательские индексы, используя команду createIndex, которая поддерживает ограничение уникальности (unique).

Уникальные индексы доступны для всех учетных записей Cosmos с помощью API Azure Cosmos DB для MongoDB.

## <a name="time-to-live-ttl"></a>Срок жизни (TTL)

Cosmos DB поддерживает срок жизни (TTL), определяемый по метке времени для документа. Срок жизни можно включить для коллекций, перейдя на [портал Azure](https://portal.azure.com).

## <a name="user-and-role-management"></a>Управление пользователями и ролями

Cosmos DB пока не поддерживает концепцию пользователей и ролей. Тем не менее Cosmos DB поддерживает управление доступом на основе ролей (RBAC), а также пароли (ключи) для чтения и записи или только для чтения, которые можно получить с помощью [портала Azure](https://portal.azure.com) (на странице "Строка подключения").

## <a name="replication"></a>Репликация

Cosmos DB поддерживает собственный механизм автоматической репликации на самых нижних уровнях. Этот механизм применяется и для организации глобальной репликации с низкой задержкой. Cosmos DB не поддерживает запуск или настройку репликации вручную.

## <a name="write-concern"></a>Проблемы с записью

Некоторые приложения используют [проблемы с записью](https://docs.mongodb.com/manual/reference/write-concern/), указывая на число ответов, требуемых во время выполнения операции записи. Так как Cosmos DB выполняет репликацию в фоновом режиме, все операции записи, автоматически происходят в режиме кворума по умолчанию. Любые проблемы с записью, определенные в клиентском коде, игнорируются. Дополнительные сведения см. в статье [Настраиваемые уровни согласованности данных в DocumentDB](consistency-levels.md).

## <a name="sharding"></a>Сегментирование

Cosmos DB поддерживает автоматическое сегментирование на стороне сервера. Cosmos DB не поддерживает запуск или настройку сегментирования вручную.

## <a name="next-steps"></a>Дополнительная информация

- Узнайте, как [использовать Studio 3T](mongodb-mongochef.md) с API Azure Cosmos DB для MongoDB.
- Узнайте, как [использовать Robo 3T](mongodb-robomongo.md) с API Azure Cosmos DB для MongoDB.
- Ознакомьтесь с [примерами](mongodb-samples.md) MongoDB с API Azure Cosmos DB для MongoDB.

<sup>Примечание. В этой статье описывается функция Azure Cosmos DB, обеспечивающая совместимость сетевого протокола с базами данных MongoDB. Корпорация Майкрософт не использует базы данных MongoDB для предоставления этой службы. Azure Cosmos DB не связана с MongoDB, Inc.</sup>
