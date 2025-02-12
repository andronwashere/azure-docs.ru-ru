---
title: Azure Cosmos DB — API, пакет SDK и ресурсы для SQL .NET Core
description: Сведения об API-интерфейсе и пакете SDK для SQL .NET Core, в том числе даты выхода, даты прекращения использования и внесенные изменения по каждой версии пакета SDK .NET Core для Azure Cosmos DB.
author: SnehaGunda
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.devlang: dotnet
ms.topic: reference
ms.date: 03/22/2018
ms.author: sngun
ms.openlocfilehash: de712d63fa2315a85c6c0eaf392d9c17758e4999
ms.sourcegitcommit: 920ad23613a9504212aac2bfbd24a7c3de15d549
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/15/2019
ms.locfileid: "68226180"
---
# <a name="azure-cosmos-db-net-core-sdk-for-sql-api-release-notes-and-resources"></a>Azure Cosmos DB: Заметки о выпуске и материалы
> [!div class="op_single_selector"]
> * [.NET Core](sql-api-sdk-dotnet-core.md)
> * [.NET Standard](sql-api-sdk-dotnet-standard.md)
> * [.NET](sql-api-sdk-dotnet.md)
> * [Веб-канал изменений в .NET](sql-api-sdk-dotnet-changefeed.md)
> * [Node.js](sql-api-sdk-node.md)
> * [Async Java](sql-api-sdk-async-java.md)
> * [Java](sql-api-sdk-java.md)
> * [Python](sql-api-sdk-python.md)
> * [REST](https://docs.microsoft.com/rest/api/cosmos-db/)
> * [Поставщик ресурсов REST](https://docs.microsoft.com/rest/api/cosmos-db-resource-provider/)
> * [SQL](sql-api-query-reference.md)
> * [BulkExecutor — .NET](sql-api-sdk-bulk-executor-dot-net.md)
> * [BulkExecutor — Java](sql-api-sdk-bulk-executor-java.md)

| |  |
|---|---|
|**Скачивание пакета SDK**| [NuGet](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB.Core/)|
|**Документация по API**|[Справочная документация по API .NET](/dotnet/api/overview/azure/cosmosdb?view=azure-dotnet)|
|**Примеры**|[Примеры кода для .NET](sql-api-dotnet-samples.md)|
|**Начало работы**|[Приступая к работе с Azure Cosmos DB .NET](sql-api-sdk-dotnet.md)|
|**Учебник по веб-приложениям**|[Руководство по ASP.NET MVC. Разработка веб-приложений в Azure Cosmos DB](sql-api-dotnet-application.md)|
|**Текущая поддерживаемая платформа**|[.NET Standard 1.6 и .NET Standard 1.5](https://www.nuget.org/packages/NETStandard.Library)|

## <a name="release-notes"></a>Заметки о выпуске

> [!NOTE]
> Если вы используете .NET Core, ознакомьтесь с последней версией [пакета SDK для .NET](sql-api-sdk-dotnet-standard.md)версии 3. x, которая предназначена для .NET Standard. 

### <a name="a-name241241"></a><a name="2.4.1"/>2.4.1

* Исправляет состояние отслеживания состояния гонки для запросов, вызвавших пустые страницы

### <a name="a-name240240"></a><a name="2.4.0"/>2.4.0

* Версия System .NET. HTTP пакета SDK соответствует тому, что определено в пакете NuGet.
* Увеличенный размер десятичной точности для запросов LINQ.
* Добавлены новые классы Компоситепас, Компоситепассортордер, Спатиалспек, Спатиалтипе и Партитионкэйдефинитионверсион
* Добавлен Тиметоливепропертипас в коллекции documentcollection
* Добавлены Компоситеиндексес и Спатиалиндексес в Индексполици
* Добавлена версия в Партитионкэйдефинитион
* В PartitionKey добавлено значение None

### <a name="a-name230230"></a><a name="2.3.0"/>2.3.0

 * Добавлены Идлеткпконнектионтимеаут, Опенткпконнектионтимеаут, Максрекуестсперткпконнектион и MaxTcpConnectionsPerEndpoint в ConnectionPolicy.
 
### <a name="a-name223223"></a><a name="2.2.3"/>2.2.3

* Улучшения диагностики

### <a name="a-name222222"></a><a name="2.2.2"/>2.2.2

* Добавлена настройка переменной среды POCOSerializationOnly.

* Удален файл DocumentDB.Spatial.Sql.dll, который теперь содержится в Microsoft.Azure.Documents.ServiceInterop.dll

### <a name="a-name221221"></a><a name="2.2.1"/>2.2.1

* Улучшения в логике повторных попыток во время отработки отказа для выполнения вызовов StoredProcedure.

* Создана отдельная база данных DocumentClientEventSource. 

* Исправлена ошибка со временем ожидания GatewayAddressCache, при которой не учитывалось ConnectionPolicy RequestTimeout.

### <a name="a-name220220"></a><a name="2.2.0"/>2.2.0

* Для прямой транспортной диагностики и транспортной диагностики по протоколу TCP добавлено исключение TransportException — внутренний тип исключений пакета SDK. Если этот тип присутствует в сообщениях об исключении, он выводит дополнительную информацию, которая помогает при устранении неполадок с проблемами подключения клиентов.

* Добавлена новая перегрузка конструктора, которая принимает HttpMessageHandler (стек обработчика HTTP-данных) для использования при отправке запросов HttpClient (например HttpClientHandler).

* Исправлена ошибка, при которой заголовок со значениями NULL не обрабатывался должным образом.

* Улучшенная проверка кэша коллекции.

### <a name="a-name213213"></a><a name="2.1.3"/>2.1.3

* Обновленные System.Net.Security до версии 4.3.2.

### <a name="a-name212212"></a><a name="2.1.2"/>2.1.2

* Усовершенствования диагностической трассировки.

### <a name="a-name211211"></a><a name="2.1.1"/>2.1.1

* Добавлена дополнительная устойчивость к кратковременным сбоям 	запросов в нескольких регионах.

### <a name="a-name210210"></a><a name="2.1.0"/>2.1.0

* Добавлена поддержка для операций записи в нескольких регионах.
* Повышение производительности запросов перекрестных разделов с помощью TOP и MaxBufferedItemCount.

### <a name="a-name200200"></a><a name="2.0.0"/>2.0.0

* Добавлена поддержка отмены запроса.
* Для ConnectionPolicy добавлено свойство SetCurrentLocation, которое автоматически заполняет предпочтительные расположения в зависимости от региона.
* В запросах между секциями исправлена ошибка, связанная с минимальным и максимальным значениями и фильтром, который не находил соответствия документов в отдельной секции.
* Методы DocumentClient теперь соответствуют IDocumentClient.
* Обновлен прямой транспортный стек TCP для сокращения числа установленных подключений.
* Добавлена поддержка TCP в режиме прямого подключения для клиентов, отличных от Windows.

### <a name="a-name200-preview2200-preview2"></a><a name="2.0.0-preview2"/>2.0.0-preview2

* Добавлена поддержка отмены запроса.
* Для ConnectionPolicy добавлено свойство SetCurrentLocation, которое автоматически заполняет предпочтительные расположения в зависимости от региона.
* В запросах между секциями исправлена ошибка, связанная с минимальным и максимальным значениями и фильтром, который не находил соответствия документов в отдельной секции.

### <a name="a-name200-preview200-preview"></a><a name="2.0.0-preview"/>2.0.0-preview

* Методы DocumentClient теперь соответствуют IDocumentClient.
* Обновлен прямой транспортный стек TCP для сокращения числа установленных подключений.
* Добавлена поддержка TCP в режиме прямого подключения для клиентов, отличных от Windows.

### <a name="a-name11001100"></a><a name="1.10.0"/>1.10.0

* Добавлено свойство ConsistencyLevel в FeedOptions.
* Добавлен класс JsonSerializerSettings в RequestOptions и FeedOptions.
* Добавлено свойство EnableReadRequestsFallback для ConnectionPolicy.

### <a name="a-name191191"></a><a name="1.9.1"/>1.9.1

* Исправлено исключение KeyNotFoundException в сложных случаях для запросов ORDER BY между секциями.
* Исправлена ошибка, из-за которой не учитывался атрибут JsonProperty в выбранном предложении запросов LINQ.

### <a name="a-name182182"></a><a name="1.8.2"/>1.8.2

* Исправлена ошибка, которая возникала в состоянии гонки и вызывала периодические ошибки "Microsoft.Azure.Documents.NotFoundException: The read session is not available for the input session token" (Microsoft.Azure.Documents.NotFoundException. Сеанс чтения для входного маркера сеанса недоступен) при использовании уровня согласованности сеанса.

### <a name="a-name181181"></a><a name="1.8.1"/>1.8.1

* Исправлена регрессия, где FeedOptions.MaxItemCount = -1 вызвало исключение System.ArithmeticException: размер страницы является отрицательным значением.
* Добавлена новая функция ToString() в QueryMetrics.
* Предоставлена статистика по секциям для считывания коллекций.
* Добавлено свойство PartitionKey в ChangeFeedOptions.
* Исправлены незначительные ошибки.

### <a name="a-name171171"></a><a name="1.7.1"/>1.7.1
 
 * Добавлена возможность указывать уникальные индексы для документов с помощью применения свойства UniqueKeyPolicy к коллекции DocumentCollection.
 * Исправлена ошибка, когда пользовательские параметры JsonSerializer не учитывались при выполнении некоторых запросов и хранимых процедур.

### <a name="a-name170170"></a><a name="1.7.0"/>1.7.0
 
 * Изменена фирменная символика с Azure DocumentDB на Azure Cosmos DB в справочной документации по API, сведениях о метаданных в сборках и пакете NuGet.
 * Предоставлены данные диагностики и сведения о задержке ответов на запросы, отправленные в режиме прямого соединения. Имена свойств: RequestDiagnosticsString и RequestLatency в классе ResourceResponse.
 * Для этой версии пакета SDK требуется последняя версия эмулятора Azure Cosmos DB. Ее можно скачать по адресу https://aka.ms/cosmosdb-emulator.
 
### <a name="a-name160160"></a><a name="1.6.0"/>1.6.0

* Добавлено несколько исправлений и улучшений для повышения надежности.

### <a name="a-name151151"></a><a name="1.5.1"/>1.5.1 

* Внутренние изменения, связанные с поддержкой [Microsoft.Azure.Graphs](https://docs.microsoft.com/azure/cosmos-db/graph-sdk-dotnet)

### <a name="a-name150150"></a><a name="1.5.0"/>1.5.0 

* Добавлена поддержка PartitionKeyRangeId как FeedOption для ограничения области результатов запроса определенным диапазоном ключа секции.
* Добавлена поддержка StartTime как ChangeFeedOption для поиска изменений после указанного периода.

### <a name="a-name141141"></a><a name="1.4.1"/>1.4.1

*   Устранена проблема в классе JsonSerializable, которая могла порождать исключение переполнения стека.

### <a name="a-name140140"></a><a name="1.4.0"/>1.4.0

*   Добавлена поддержка для указания пользовательских параметров JsonSerializerSettings при создании экземпляра [DocumentClient](/dotnet/api/microsoft.azure.documents.client.documentclient?view=azure-dotnet).

### <a name="a-name132132"></a><a name="1.3.2"/>1.3.2

*   Поддержка .NET Standard 1.5 в качестве одной из требуемых версий .NET Framework.

### <a name="a-name131131"></a><a name="1.3.1"/>1.3.1

*   Устранена проблема, возникающая на компьютерах под управлением 64-разрядной ОС без поддержки инструкций SSE4, которая приводила к исключению SEHException при выполнении запросов Azure Cosmos DB.

### <a name="a-name130130"></a><a name="1.3.0"/>1.3.0

*   Добавлена поддержка нового уровня согласованности с именем ConsistentPrefix.
*   Добавлена поддержка запроса метрик отдельных секций.
*   Добавлена поддержка ограничения размера маркера продолжения запросов.
*   Добавлена поддержка более подробной трассировки невыполненных запросов.
*   Внесены некоторые улучшения производительности в пакет SDK.

### <a name="a-name122122"></a><a name="1.2.2"/>1.2.2

* Устранена проблема, когда не учитывалось значение PartitionKey в FeedOptions для статистических запросов.
* Устранена проблема, связанная с прозрачной обработкой управления при промежуточном выполнении запроса Order By между разделами.

### <a name="a-name121121"></a><a name="1.2.1"/>1.2.1

* Исправлена проблема, вызывавшая взаимоблокировки в некоторых асинхронных интерфейсах API при использовании в контексте ASP.NET.

### <a name="a-name120120"></a><a name="1.2.0"/>1.2.0

* Исправления для повышения устойчивости пакета SDK к автоматической отработке отказа при определенных условиях.

### <a name="a-name112112"></a><a name="1.1.2"/>1.1.2

* Исправлена проблема, время от времени вызывающая исключение "WebException: удаленное имя не удалось разрешить".
* Добавлена поддержка непосредственного считывания типизированного документа путем добавления новых перегрузок в API ReadDocumentAsync.

### <a name="a-name111111"></a><a name="1.1.1"/>1.1.1

* Добавлена поддержка LINQ для статистических запросов (COUNT, MIN, MAX, SUM и AVG).
* Исправление ошибки утечки памяти для объекта ConnectionPolicy, вызываемой использованием обработчика событий.
* Исправление ошибки, из-за которой метод UpsertAttachmentAsync не работал, когда использовался тег ETag.
* Исправление ошибки, из-за которой продолжение запросов orderBy между секциями не работало при сортировке по полю строки.

### <a name="a-name110110"></a><a name="1.1.0"/>1.1.0

* Добавлена поддержка статистических запросов (COUNT, MIN, MAX, SUM и AVG). Дополнительные сведения см. в разделе [Статистические функции](sql-query-aggregates.md).
* Минимальная пропускная способность секционированных коллекций снижена с 10 100 ЕЗ/с до 2500 ЕЗ/с.

### <a name="a-name100100"></a><a name="1.0.0"/>1.0.0

Пакет SDK .NET Core для Azure Cosmos DB позволяет создавать быстрые кроссплатформенные приложения [ASP.NET Core](https://www.asp.net/core) и [.NET Core](https://www.microsoft.com/net/core#windows) для Windows, Mac и Linux. Последний выпуск пакета SDK .NET Core для Azure Cosmos DB полностью совместим с [Хamarin](https://www.xamarin.com) и пригоден для создания приложений для iOS, Android и Mono (Linux).  

### <a name="a-name010-preview010-preview"></a><a name="0.1.0-preview"/>0.1.0-preview

Пакет SDK .NET Core для Azure Cosmos DB (предварительная версия) позволяет создавать быстрые кроссплатформенные приложения [ASP.NET Core](https://www.asp.net/core) и [.NET Core](https://www.microsoft.com/net/core#windows) для Windows, Mac и Linux.

Пакет SDK .NET Core для Azure Cosmos DB (предварительная версия) функционально полностью эквивалентен последней версии [пакета SDK .NET для Azure Cosmos DB](sql-api-sdk-dotnet.md) и поддерживает следующие возможности:
* все [режимы подключения](performance-tips.md#networking): режим шлюза, прямые TCP- и HTTP-подключения;
* все [уровни согласованности](consistency-levels.md): строгая, ограниченное устаревание, согласованность сеанса, окончательная;
* [секционированные коллекции](partition-data.md);
* [георепликация данных и межрегиональные учетные записи баз данных](distribute-data-globally.md).

Если у вас возникли вопросы об этом пакете SDK, опубликуйте их на форуме сайта [StackOverflow](https://stackoverflow.com/questions/tagged/azure-documentdb) или сообщите о проблеме в [репозитории GitHub](https://github.com/Azure/azure-documentdb-dotnet/issues).

## <a name="release--retirement-dates"></a>Даты выпуска и выбытия

| Version | Дата выпуска | Дата вывода |
| --- | --- | --- |
| [2.4.1](#2.4.1) |20 июня 2019 г. |--- |
| [2.4.0](#2.4.0) |05 мая 2005 г., 2019 |--- |
| [2.3.0](#2.3.0) |04, 2019 |--- |
| [2.2.3](#2.2.3) |11 марта 2019 г. |--- |
| [2.2.2](#2.2.2) |06 февраля, 2019 |--- |
| [2.2.1](#2.2.1) |24 декабря 2018 г. |--- |
| [2.2.0](#2.2.0) |7 декабря 2018 г. |--- |
| [2.1.3](#2.1.3) |15 октября 2018 г. |--- |
| [2.1.2](#2.1.2) |04 октября 2018 г. |--- |
| [2.1.1](#2.1.1) |27 сентября 2018 г. |--- |
| [2.1.0](#2.1.0) |21 сентября 2018 г. |--- |
| [2.0.0](#2.0.0) |07 сентября 2018 г. |--- |
| [1.9.1](#1.9.1) |9 марта 2018 г. |--- |
| [1.8.2](#1.8.2) |21 февраля 2018 г. |--- |
| [1.8.1](#1.8.1) |5 февраля 2018 г. |--- |
| [1.7.1](#1.7.1) |16 ноября 2017 г. |--- |
| [1.7.0](#1.7.0) |10 ноября 2017 г. |--- |
| [1.6.0](#1.6.0) |17 октября 2017 г. |--- |
| [1.5.1](#1.5.1) |2 октября 2017 г. |--- |
| [1.5.0](#1.5.0) |10 августа 2017 г. |--- | 
| [1.4.1](#1.4.1) |7 августа 2017 г. |--- |
| [1.4.0](#1.4.0) |2 августа 2017 г. |--- |
| [1.3.2](#1.3.2) |12 июня 2017 г. |--- |
| [1.3.1](#1.3.1) |23 мая 2017 г. |--- |
| [1.3.0](#1.3.0) |10 мая 2017 г. |--- |
| [1.2.2](#1.2.2) |19 апреля 2017 г. |--- |
| [1.2.1](#1.2.1) |29 марта 2017 г. |--- |
| [1.2.0](#1.2.0) |25 марта 2017 г. |--- |
| [1.1.2](#1.1.2) |20 марта 2017 г. |--- |
| [1.1.1](#1.1.1) |14 марта 2017 г. |--- |
| [1.1.0](#1.1.0) |16 февраля 2017 г. |--- |
| [1.0.0](#1.0.0) |21 декабря 2016 г. |--- |
| [0.1.0-preview](#0.1.0-preview) |15 ноября 2016 г. |31 декабря 2016 г. |

## <a name="see-also"></a>См. также
Дополнительные сведения о Cosmos DB см. на странице службы [Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/).

