---
title: Azure Cosmos DB — API, пакет SDK и ресурсы для SQL Java
description: Сведения о пакете SDK и API-интерфейсе SQL Java, включая даты выхода и прекращения использования, а также изменения, внесенные в каждую версию пакета SDK SQL Java для Azure Cosmos DB.
author: SnehaGunda
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.devlang: java
ms.topic: reference
ms.date: 03/13/2019
ms.author: sngun
ms.openlocfilehash: 7af43d24b03a4c548925bdca4189fd0524c701a4
ms.sourcegitcommit: a12b2c2599134e32a910921861d4805e21320159
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/24/2019
ms.locfileid: "67341636"
---
# <a name="azure-cosmos-db-java-sdk-for-sql-api-release-notes-and-resources"></a>Azure Cosmos DB: заметки о выпуске и материалы
> [!div class="op_single_selector"]
> * [.NET](sql-api-sdk-dotnet.md)
> * [Веб-канал изменений в .NET](sql-api-sdk-dotnet-changefeed.md)
> * [.NET Core](sql-api-sdk-dotnet-core.md)
> * [Node.js](sql-api-sdk-node.md)
> * [Async Java](sql-api-sdk-async-java.md)
> * [Java](sql-api-sdk-java.md)
> * [Python](sql-api-sdk-python.md)
> * [REST](https://docs.microsoft.com/rest/api/cosmos-db/)
> * [Поставщик ресурсов REST](https://docs.microsoft.com/rest/api/cosmos-db-resource-provider/)
> * [SQL](sql-api-query-reference.md)
> * [BulkExecutor — .NET](sql-api-sdk-bulk-executor-dot-net.md)
> * [BulkExecutor — Java](sql-api-sdk-bulk-executor-java.md)

Пакет SDK Java для API-интерфейса SQL поддерживает синхронные операции. Для асинхронных операций используйте [пакет SDK Java Async для API-интерфейса SQL](sql-api-sdk-async-java.md). 

| |  |
|---|---|
|**Скачивание пакета SDK**|[Maven](https://search.maven.org/#search%7Cgav%7C1%7Cg%3A%22com.microsoft.azure%22%20AND%20a%3A%22azure-documentdb%22)|
|**Документация по API**|[Справочная документация по API Java](/java/api/com.microsoft.azure.documentdb)|
|**Участие в разработке пакета SDK**|[GitHub](https://github.com/Azure/azure-documentdb-java/)|
|**Начало работы**|[Приступая к работе с пакетом SDK для Java](sql-api-java-get-started.md)|
|**Учебник по веб-приложениям**|[Руководство по ASP.NET MVC. Разработка веб-приложений в Azure Cosmos DB](sql-api-java-application.md)|
|**Минимальная поддерживаемая среда выполнения**|[Пакет средств разработки Java (JDK) 7+](https://aka.ms/azure-jdks)|

## <a name="release-notes"></a>Заметки о выпуске

### <a name="a-name213213"></a><a name="2.1.3"/>2.1.3
* Исправлена ошибка в PartitionKey для хэша V2.

### <a name="a-name212212"></a><a name="2.1.2"/>2.1.2
* Добавлена поддержка составных индексах.
* Исправлена ошибка в диспетчере глобальной конечной точки для принудительного обновления.
* Исправлена ошибка, для операции Upsert с предварительными условиями в режиме прямого подключения.

### <a name="a-name211211"></a><a name="2.1.1"/>2.1.1
* Исправлена ошибка в кэше адрес шлюза.

### <a name="a-name210210"></a><a name="2.1.0"/>2.1.0
* Добавлена поддержка операций записи в нескольких регионах для режима прямого подключения.
* Добавлена поддержка обработки исключений IOException, генерируемых как исключения ServiceUnavailable из прокси-сервера.
* Исправлена ошибка в политике повтора обнаружения конечных точек.
* Исправлена ошибка, приводившая к возникновению исключения пустого указателя в BaseDatabaseAccountConfigurationProvider.
* Исправлена ошибка, чтобы убедиться, что QueryIterator не возвращает значения NULL.
* Исправлена ошибка, не разрешающая использование большого ключа секции.

### <a name="a-name200200"></a><a name="2.0.0"/>2.0.0
* Добавлена поддержка операций записи в нескольких регионах для режима шлюза.

### <a name="a-name11641164"></a><a name="1.16.4"/>1.16.4
* Исправлена ошибка в чтении диапазонов ключей секций для запроса.

### <a name="a-name11631163"></a><a name="1.16.3"/>1.16.3
* Исправлена ошибка в настройке размера заголовка маркера продолжения в режиме DirectHttps.

### <a name="a-name11621162"></a><a name="1.16.2"/>1.16.2
* Добавлена поддержка отработки отказа потоковой передачи.
* Добавлена поддержка для пользовательских метаданных.
* Улучшена логика обработки сеанса.
* Исправлена ошибка в кэше диапазона ключей секций.
* Исправлена ошибка с NPE в режиме прямого подключения.

### <a name="a-name11611161"></a><a name="1.16.1"/>1.16.1
* Добавлена поддержка уникальных индексов.
* Добавлена поддержка ограничения размера маркера продолжения в параметрах веб-канала.
* Исправлена ошибка сериализации JSON (метка времени).
* Исправлена ошибка сериализации JSON (перечисление).
* Зависимость от com.fasterxml.jackson.core:jackson-databind обновлена до версии 2.9.5.

### <a name="a-name11601160"></a><a name="1.16.0"/>1.16.0
* Улучшен пул подключений для режима прямого подключения.
* Улучшена предварительная выборка для запросов между секциями, отличных от orderBy.
* Улучшено создание UUID.
* Улучшена логика согласованности сеанса.
* Добавлена поддержка MultiPolygon.
* Добавлена поддержка статистики диапазона ключей секции для коллекции.
* Исправлена ошибка, возникающая при поддержке нескольких регионов.

### <a name="a-name11501150"></a><a name="1.15.0"/>1.15.0
* Повышенная производительность сериализации JSON.
* Для этой версии пакета SDK требуется последняя версия эмулятора Azure Cosmos DB. Ее можно скачать по адресу https://aka.ms/cosmosdb-emulator.

### <a name="a-name11401140"></a><a name="1.14.0"/>1.14.0
* Внутренние изменения в библиотеках партнеров корпорации Майкрософт.

### <a name="a-name11301130"></a><a name="1.13.0"/>1.13.0
* Устранена проблема при считывании диапазонов односекционных ключей.
* Устранена проблема с синтаксическим анализом в ResourceID, которая затрагивала базы данных с сокращенными именами.
* Исправлена проблема, вызванная кодированием ключа секции.

### <a name="a-name11201120"></a><a name="1.12.0"/>1.12.0
* Важные исправления для обработки запросов во время разбиения на секции.
* Исправлена проблема с уровнями согласованности Strong и BoundedStaleness.

### <a name="a-name11101110"></a><a name="1.11.0"/>1.11.0
* Добавлена поддержка нового уровня согласованности с именем ConsistentPrefix.
* Исправлена ошибка чтения коллекции в режиме сеанса.

### <a name="a-name11001100"></a><a name="1.10.0"/>1.10.0
* Включена поддержка секционированных коллекций с производительностью 2500 ЕЗ/с, а также масштабирование с шагом в 100 ЕЗ/с.
* Исправлена ошибка в машинной сборке, которая могла вызывать в некоторых запросах исключение NullRef.

### <a name="a-name196196"></a><a name="1.9.6"/>1.9.6
* Исправлена ошибка в конфигурации обработчика запросов, которая могла вызывать исключения для запросов в режиме шлюза.
* Исправлены некоторые ошибки в контейнере сеансов, которые могли вызывать исключение Owner resource not found (Ресурс владельца не найден) для запросов сразу после создания коллекции.

### <a name="a-name195195"></a><a name="1.9.5"/>1.9.5
* Добавлена поддержка статистических запросов (COUNT, MIN, MAX, SUM и AVG). Дополнительные сведения см. в разделе [Статистические функции](sql-query-aggregates.md).
* Добавлена поддержка веб-канала изменений.
* Добавлена поддержка сведений о квотах коллекций посредством RequestOptions.setPopulateQuotaInfo.
* Добавлена поддержка ведения журнала сценариев хранимых процедур посредством RequestOptions.setScriptLoggingEnabled.
* Исправлена ошибка, в которой запрос в режиме DirectHttps может перестать отвечать на запросы при обнаружении ошибок регулирования.
* Исправлена ошибка в режиме согласованности сеанса.
* Исправлена ошибка, которая могла приводить к порождению исключения NullReferenceException в HttpContext при высокой частоте запросов.
* Повышена производительность режима DirectHttps.

### <a name="a-name194194"></a><a name="1.9.4"/>1.9.4
* Добавлена поддержка прокси-сервера на основе экземпляра простого клиента с помощью API ConnectionPolicy.setProxy().
* Добавлен API DocumentClient.close() для правильного завершения работы экземпляра DocumentClient.
* Повышена производительность запросов в режиме прямого подключения за счет получения плана запроса от машинной сборки, а не шлюза.
* Задан параметр FAIL_ON_UNKNOWN_PROPERTIES = false, чтобы пользователям не нужно было определять JsonIgnoreProperties в POJO.
* Выполнен рефакторинг ведения журнала для использования SLF4J.
* Исправлено несколько ошибок в модуле чтения данных согласованности.

### <a name="a-name193193"></a><a name="1.9.3"/>1.9.3
* Исправлена ошибка в управлении подключениями для предотвращения утечек в режиме прямого подключения.
* Исправлена ошибка в запросе TOP, которая могла порождать исключение NullReference.
* Повышена производительности за счет сокращения числа сетевых вызовов к внутренним кэшам.
* В DocumentClientException добавлены код состояния, ActivityID и универсальный код ресурса (URI) для более удобного устранения неполадок.

### <a name="a-name192192"></a><a name="1.9.2"/>1.9.2
* Исправлена проблема в управлении подключениями для повышения стабильности.

### <a name="a-name191191"></a><a name="1.9.1"/>1.9.1
* Добавлена поддержка уровня согласованности BoundedStaleness.
* Добавлена поддержка прямого подключения для операций CRUD с секционированными коллекциями.
* Исправлена ошибка, возникающая при выполнении запросов к базе данных с помощью SQL.
* Исправлена ошибка в кэше сеанса, приводившая к неправильному заданию маркера сеанса.

### <a name="a-name190190"></a><a name="1.9.0"/>1.9.0
* Добавлена поддержка параллельных запросов между секциями.
* Добавлена поддержка запросов TOP и ORDER BY для секционированных коллекций.
* Добавлена поддержка строгой согласованности.
* Добавлена поддержка запросов по имени при использовании прямого подключения.
* Внесено исправление, обеспечивающее согласованность ActivityId для всех попыток выполнения запроса.
* Исправлена ошибка, связанная с кэшем сеанса и воссозданием коллекции с тем же именем.
* Добавлены типы данных Polygon и LineString и задана политика индексирования коллекций для запросов пространственных геозон.
* Устранены проблемы с JavaDoc для Java 1.8.

### <a name="a-name181181"></a><a name="1.8.1"/>1.8.1
* Исправлена ошибка в PartitionKeyDefinitionMap, из-за которой осуществлялось кэширование коллекций одной секции и не выполнялись дополнительные запросы на получение ключа секции.
* Исправлена ошибка, из-за которой при предоставлении неправильного ключа секции не предпринималась повторная попытка.

### <a name="a-name180180"></a><a name="1.8.0"/>1.8.0
* Добавлена поддержка учетных записей базы данных в нескольких регионах.
* Добавлена поддержка автоматического повтора на отрегулированных запросов с параметрами для настройки максимального количества повторов и максимального время между повторами.  Ознакомьтесь с RetryOptions и ConnectionPolicy.getRetryOptions().
* Не рекомендуется использовать IPartitionResolver на основе пользовательского кода секционирования. Используйте секционированные коллекции, чтобы увеличить возможности хранилища и пропускную способность.

### <a name="a-name171171"></a><a name="1.7.1"/>1.7.1
* Добавлена поддержка политики повтора для ограничения скорости.  

### <a name="a-name170170"></a><a name="1.7.0"/>1.7.0
* Добавлена поддержка срока жизни для документов.

### <a name="a-name160160"></a><a name="1.6.0"/>1.6.0
* Реализованы [секционированные коллекции](partition-data.md) и [определяемые пользователем уровни производительности](performance-levels.md).

### <a name="a-name151151"></a><a name="1.5.1"/>1.5.1
* Исправлена ошибка в HashPartitionResolver, чтобы значения создавались с прямым порядком байтов для согласования с другими пакетами SDK.

### <a name="a-name150150"></a><a name="1.5.0"/>1.5.0
* Добавьте сопоставители разделов "Хэш" и "Диапазон" для сегментирования приложений на разделы.

### <a name="a-name140140"></a><a name="1.4.0"/>1.4.0
* Реализована операция Upsert. Для поддержки функции Upsert добавлены новые методы upsertXXX.
* Реализована маршрутизация на основе идентификатора. Отсутствуют изменения общего API-интерфейса. Все изменения касаются внутреннего интерфейса.

### <a name="a-name130130"></a><a name="1.3.0"/>1.3.0
* Пропущен номер выпуска для согласованности номера версии с другими пакетами SDK.

### <a name="a-name120120"></a><a name="1.2.0"/>1.2.0
* Поддержка геопространственного индекса
* Проверка свойств идентификатора для всех ресурсов. Идентификаторы ресурсов не могут содержать знаки ?, /, #, \, или заканчиваться пробелом.
* Добавлен новый заголовок "ход выполнения преобразования индекса" в ResourceResponse.

### <a name="a-name110110"></a><a name="1.1.0"/>1.1.0
* Реализована политика индексации версии 2.

### <a name="a-name100100"></a><a name="1.0.0"/>1.0.0
* Пакет SDK общей доступности.

## <a name="release-and-retirement-dates"></a>Даты выпуска и выбытия
Корпорация Майкрософт отправит уведомление минимум за **12 месяцев** до вывода пакета SDK из эксплуатации, чтобы обеспечить более плавный переход на новую или поддерживаемую версию.

Новые функции, возможности и оптимизации добавляются только в текущую версию пакета SDK, поэтому рекомендуется как можно раньше обновлять пакет SDK до последней версии.

Любые запросы к Cosmos DB с помощью выведенного из эксплуатации пакета SDK будут отклонены службой.

> [!WARNING]
> Все версии **1.x** пакета SDK SQL для Java будет прекращена **30 мая 2020**.
> 
>

> [!WARNING]
> Поддержка всех версий пакета SDK SQL для Java версии ниже **1.0.0** была прекращена **29 февраля 2016 года**.
> 
> 

<br/>

| Version | Дата выпуска | Дата вывода |
| --- | --- | --- |
| [2.1.3](#2.1.3) |13 марта 2018 г. |--- |
| [2.1.2](#2.1.2) |09 марта 2018 г. |--- |
| [2.1.1](#2.1.1) |13 декабря 2018 г. |--- |
| [2.1.0](#2.1.0) |20 ноября 2018 г. |--- |
| [2.0.0](#2.0.0) |21 сентября 2018 г. |--- |
| [1.16.4](#1.16.4) |10 сентября 2018 г. |30 мая 2020 г. |
| [1.16.3](#1.16.3) |9 сентября 2018 г. |30 мая 2020 г. |
| [1.16.2](#1.16.2) |29 июня 2018 г. |30 мая 2020 г. |
| [1.16.1](#1.16.1) |16 мая 2018 г. |30 мая 2020 г. |
| [1.16.0](#1.16.0) |15 марта 2018 г. |30 мая 2020 г. |
| [1.15.0](#1.15.0) |14 ноября 2017 г. |30 мая 2020 г. |
| [1.14.0](#1.14.0) |28 октября 2017 г. |30 мая 2020 г. |
| [1.13.0](#1.13.0) |25 августа 2017 г. |30 мая 2020 г. |
| [1.12.0](#1.12.0) |11 июля 2017 г. |30 мая 2020 г. |
| [1.11.0](#1.11.0) |10 мая 2017 г. |30 мая 2020 г. |
| [1.10.0](#1.10.0) |11 марта 2017 г. |30 мая 2020 г. |
| [1.9.6](#1.9.6) |21 февраля 2017 г. |30 мая 2020 г. |
| [1.9.5](#1.9.5) |31 января 2017 г. |30 мая 2020 г. |
| [1.9.4](#1.9.4) |24 ноября 2016 г. |30 мая 2020 г. |
| [1.9.3](#1.9.3) |30 октября 2016 г. |30 мая 2020 г. |
| [1.9.2](#1.9.2) |28 октября 2016 г. |30 мая 2020 г. |
| [1.9.1](#1.9.1) |26 октября 2016 г. |30 мая 2020 г. |
| [1.9.0](#1.9.0) |3 октября 2016 г. |30 мая 2020 г. |
| [1.8.1](#1.8.1) |30 июня 2016 г. |30 мая 2020 г. |
| [1.8.0](#1.8.0) |14 июня 2016 г. |30 мая 2020 г. |
| [1.7.1](#1.7.1) |30 апреля 2016 г. |30 мая 2020 г. |
| [1.7.0](#1.7.0) |27 апреля 2016 г. |30 мая 2020 г. |
| [1.6.0](#1.6.0) |29 марта 2016 г. |30 мая 2020 г. |
| [1.5.1](#1.5.1) |31 декабря 2015 г. |30 мая 2020 г. |
| [1.5.0](#1.5.0) |4 декабря 2015 г. |30 мая 2020 г. |
| [1.4.0](#1.4.0) |5 октября 2015 г. |30 мая 2020 г. |
| [1.3.0](#1.3.0) |5 октября 2015 г. |30 мая 2020 г. |
| [1.2.0](#1.2.0) |5 августа 2015 г. |30 мая 2020 г. |
| [1.1.0](#1.1.0) |9 июля 2015 г. |30 мая 2020 г. |
| 1.0.1 |12 мая 2015 г. |30 мая 2020 г. |
| [1.0.0](#1.0.0) |7 апреля 2015 г. |30 мая 2020 г. |
| 0.9.5-prelease |9 марта 2015 г. |29 февраля 2016 г. |
| 0.9.4-prelease |17 февраля 2015 г. |29 февраля 2016 г. |
| 0.9.3-prelease |13 января 2015 г. |29 февраля 2016 г. |
| 0.9.2-prelease |19 декабря 2014 г. |29 февраля 2016 г. |
| 0.9.1-prelease |19 декабря 2014 г. |29 февраля 2016 г. |
| 0.9.0-prelease |10 декабря 2014 г. |29 февраля 2016 г. |

## <a name="faq"></a>Часто задаваемые вопросы
[!INCLUDE [cosmos-db-sdk-faq](../../includes/cosmos-db-sdk-faq.md)]

## <a name="see-also"></a>См. также
Дополнительные сведения о Cosmos DB см. на странице службы [Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/).

