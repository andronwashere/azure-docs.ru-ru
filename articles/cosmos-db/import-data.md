---
title: Средство миграции базы данных для Azure Cosmos DB
description: Из этой статьи вы узнаете, как использовать открытые средства миграции данных Azure Cosmos DB для импорта данных в Azure Cosmos DB из различных источников, включая MongoDB, SQL Server, хранилище таблиц, Amazon DynamoDB, файлы CSV и JSON. Преобразование CSV в JSON.
author: deborahc
ms.service: cosmos-db
ms.topic: tutorial
ms.date: 05/20/2019
ms.author: dech
ms.openlocfilehash: 792dca41a052930bf2c853846cdd0c09661c5cd3
ms.sourcegitcommit: 24fd3f9de6c73b01b0cee3bcd587c267898cbbee
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/20/2019
ms.locfileid: "65954497"
---
# <a name="use-data-migration-tool-to-migrate-your-data-to-azure-cosmos-db"></a>Использование средства переноса данных для переноса данных в Azure Cosmos DB

В этом руководстве показано, как использовать средство миграции данных Azure Cosmos DB, с помощью которого можно импортировать данные из различных источников в коллекции и таблицы Azure Cosmos DB. Данные можно импортировать из JSON-файлов, CSV-файлов, с сервера SQL, из MongoDB, хранилища таблиц Azure, Amazon DynamoDB и даже из коллекций API SQL для базы данных Azure Cosmos DB. Эти данные переносятся в коллекции и таблицы для использования в Azure Cosmos DB. Средство миграции данных можно также использовать при миграции из односекционной коллекции в многосекционную для API SQL.

Какой программный интерфейс вы собираетесь использовать с помощью Azure Cosmos DB?

* **[API SQL](documentdb-introduction.md)** . Вы можете импортировать данные, используя любые варианты источников средства переноса данных.
* **[API таблицы](table-introduction.md)** . Для импорта данных вы можете использовать средство миграции данных или AzCopy. Дополнительные сведения см. в статье [Import data for use with the Azure Cosmos DB Table API](table-import.md) (Импорт данных для использования с помощью API таблицы Azure DB Cosmos).
* **[API Azure Cosmos DB для MongoDB](mongodb-introduction.md)** . Сейчас средство переноса данных не поддерживает API Azure Cosmos DB для MongoDB в качестве источника или целевого объекта. Инструкции по переносу данных в коллекции Azure Cosmos DB или из них см. в статье [Перенос данных MongoDB в Azure Cosmos DB](mongodb-migrate.md). Вы по-прежнему можете экспортировать данные из MongoDB в коллекции API SQL в Azure Cosmos DB, чтобы использовать их с этим интерфейсом, с помощью средства переноса данных.
* **[API Gremlin](graph-introduction.md)** . Сейчас средство переноса данных не поддерживает импорт учетных записей API Gremlin.

В рамках этого руководства рассматриваются следующие задачи:

> [!div class="checklist"]
> * установка средства миграции данных;
> * импорт данных из разных источников данных;
> * экспорт данных из Azure Cosmos DB в JSON.

## <a id="Prerequisites"></a>Предварительные требования

Прежде чем выполнять инструкции, изложенные в этой статье, выполните следующие действия:

* **Установите** [Microsoft .NET Framework 4.51](https://www.microsoft.com/download/developer-tools.aspx) или более поздней версии.

* **Увеличьте пропускную способность**. Продолжительность переноса данных зависит от пропускной способности, настроенной для отдельной коллекции или набора коллекций. Увеличьте пропускную способность для крупных миграций. После переноса уменьшите пропускную способность для экономии расходов. Дополнительные сведения об увеличении пропускной способности на портале Azure см. в статьях об [уровнях производительности](performance-levels.md) и [ценовых категориях](https://azure.microsoft.com/pricing/details/cosmos-db/) в Azure Cosmos DB.

* **Создайте ресурсы Azure Cosmos DB**. Перед началом переноса данных заранее создайте коллекции на портале Azure. Для перехода на учетную запись Azure Cosmos DB с пропускной способностью уровня базы данных при создании коллекций Azure Cosmos DB укажите ключ раздела.

## <a id="Overviewl"></a>Обзор

Средство миграции данных — это решение с открытым исходным кодом, которое импортирует данные в Azure Cosmos DB из различных источников, таких как:

* файлы JSON;
* MongoDB
* SQL Server
* СЫМ-файлы;
* табличное хранилище Azure;
* Amazon DynamoDB
* HBase
* коллекции Azure Cosmos DB.

Хотя средство миграции предоставляет графический интерфейс пользователя (dtui.exe), им также можно управлять из командной строки (dt.exe). К слову, существует возможность просмотреть соответствующую команду после настройки импорта в пользовательском интерфейсе. Можно преобразовать исходные данные в табличном формате, например данные SQL Server или CSV-файлы, чтобы создать иерархические связи (вложенные документы) во время импорта. Чтобы узнать о доступных источниках, примерах команд для импорта из каждого источника, возможных целевых объектах и просмотре результатов импорта, читайте дальше.

## <a id="Install"></a>Установка

Исходный код средства миграции можно найти в [этом репозитории](https://github.com/azure/azure-documentdb-datamigrationtool) GitHub. Вы можете скачать решение и скомпилировать его локально, или [скачать предварительно скомпилированный двоичный файл](https://aka.ms/csdmtool), а затем запустить одну из версий:

* **Dtui.exe**. Версия средства с графическим интерфейсом.
* **Dt.exe**. Версия средства с командной строкой.

## <a name="select-data-source"></a>Выберите источник данных

После установки средства вы можете импортировать данные. Поддерживаются такие типы данных и виды импорта:

* [файлы JSON](#JSON);
* [MongoDB](#MongoDB)
* [файлы экспорта MongoDB](#MongoDBExport);
* [SQL Server](#SQL)
* [CSV-файлы](#CSV);
* [Хранилище таблиц Azure](#AzureTableSource)
* [Amazon DynamoDB](#DynamoDBSource);
* [Большой двоичный объект](#BlobImport)
* [коллекции Azure Cosmos DB](#SQLSource);
* [HBase](#HBaseSource)
* [массовый импорт Azure Cosmos DB](#SQLBulkTarget);
* [последовательный импорт записей Azure Cosmos DB](#SQLSeqTarget).

## <a id="JSON"></a>Импорт файлов JSON

Функция импорта из исходных JSON-файлов позволяет импортировать один или несколько JSON-файлов, каждый из которых содержит один документ либо массив документов JSON. При добавлении папок, содержащих JSON-файлы для импорта, вы можете выполнить рекурсивный поиск файлов во вложенных папках.

![Снимок экрана: параметры исходного файла JSON — средства миграции базы данных](./media/import-data/jsonsource.png)

Строка подключения в следующем формате:

`AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>`

* `<CosmosDB Endpoint>` — это URI конечной точки. Это значение можно получить на портале Azure. Перейдите к своей учетной записи Azure Cosmos. Откройте область **Обзор** и скопируйте значение**URI**.
* `<AccountKey>` имеет значение пароля или **первичного ключа**. Это значение можно получить на портале Azure. Перейдите к своей учетной записи Azure Cosmos. Откройте область **Строки подключения** или **Ключи** и скопируйте значение пароля или **первичного ключа**.
* `<CosmosDB Database>` является именем базы данных CosmosDB.

Пример: `AccountEndpoint=https://myCosmosDBName.documents.azure.com:443/;AccountKey=wJmFRYna6ttQ79ATmrTMKql8vPri84QBiHTt6oinFkZRvoe7Vv81x9sn6zlVlBY10bEPMgGM982wfYXpWXWB9w==;Database=myDatabaseName`

> [!NOTE]
> Используйте команду Verify, чтобы проверить доступ к учетной записи Azure Cosmos DB, указанному в строке подключения.

Ниже приведены некоторые примеры команд для импорта файлов JSON.

```console
#Import a single JSON file
dt.exe /s:JsonFile /s.Files:.\Sessions.json /t:DocumentDBBulk /t.ConnectionString:"AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:Sessions /t.CollectionThroughput:2500

#Import a directory of JSON files
dt.exe /s:JsonFile /s.Files:C:\TESessions\*.json /t:DocumentDBBulk /t.ConnectionString:" AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:Sessions /t.CollectionThroughput:2500

#Import a directory (including sub-directories) of JSON files
dt.exe /s:JsonFile /s.Files:C:\LastFMMusic\**\*.json /t:DocumentDBBulk /t.ConnectionString:" AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:Music /t.CollectionThroughput:2500

#Import a directory (single), directory (recursive), and individual JSON files
dt.exe /s:JsonFile /s.Files:C:\Tweets\*.*;C:\LargeDocs\**\*.*;C:\TESessions\Session48172.json;C:\TESessions\Session48173.json;C:\TESessions\Session48174.json;C:\TESessions\Session48175.json;C:\TESessions\Session48177.json /t:DocumentDBBulk /t.ConnectionString:"AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:subs /t.CollectionThroughput:2500

#Import a single JSON file and partition the data across 4 collections
dt.exe /s:JsonFile /s.Files:D:\\CompanyData\\Companies.json /t:DocumentDBBulk /t.ConnectionString:"AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:comp[1-4] /t.PartitionKey:name /t.CollectionThroughput:2500
```

## <a id="MongoDB"></a>Импорт из MongoDB

> [!IMPORTANT]
> Чтобы выполнить импорт в учетную запись Cosmos, настраиваемую с помощью API Azure Cosmos DB для MongoDB, следуйте этим [инструкциям](mongodb-migrate.md).

Функция импорта из исходной базы данных MongoDB позволяет импортировать одну коллекцию MongoDB. При этом дополнительно можно отфильтровать документы с помощью запроса и изменить структуру документа с использованием проекции.  

![Снимок экрана параметров источника MongoDB](./media/import-data/mongodbsource.png)

Строка подключения представляется в стандартном формате MongoDB:

`mongodb://<dbuser>:<dbpassword>@<host>:<port>/<database>`

> [!NOTE]
> Используйте команду Verify, чтобы проверить доступ к экземпляру MongoDB, указанному в строке подключения.

Введите имя коллекции, из которой будут импортированы данные. Дополнительно можно указать файл для запроса, например `{pop: {$gt:5000}}`, или для проекции, например `{loc:0}`, чтобы отфильтровать и сформировать импортируемые данные.

Ниже приведены некоторые примеры команд для импорта данных из MongoDB.

```console
#Import all documents from a MongoDB collection
dt.exe /s:MongoDB /s.ConnectionString:mongodb://<dbuser>:<dbpassword>@<host>:<port>/<database> /s.Collection:zips /t:DocumentDBBulk /t.ConnectionString:"AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:BulkZips /t.IdField:_id /t.CollectionThroughput:2500

#Import documents from a MongoDB collection which match the query and exclude the loc field
dt.exe /s:MongoDB /s.ConnectionString:mongodb://<dbuser>:<dbpassword>@<host>:<port>/<database> /s.Collection:zips /s.Query:{pop:{$gt:50000}} /s.Projection:{loc:0} /t:DocumentDBBulk /t.ConnectionString:"AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:BulkZipsTransform /t.IdField:_id/t.CollectionThroughput:2500
```

## <a id="MongoDBExport"></a>Импортировать файлы экспорта MongoDB

> [!IMPORTANT]
> При импорте в учетную запись Azure Cosmos DB с поддержкой MongoDB выполните эти [инструкции](mongodb-migrate.md).

Параметр импорта JSON-файлов экспорта MongoDB позволяет импортировать файлы JSON, созданные с помощью служебной программы mongoexport.  

![Снимок экрана параметров экспорта MongoDB](./media/import-data/mongodbexportsource.png)

При добавлении для импорта папок, содержащих JSON-файлы экспорта MongoDB, вы можете выполнить рекурсивный поиск файлов во вложенных папках.

Ниже приведен пример команды для импорта данных из JSON-файлов экспорта MongoDB.

```console
dt.exe /s:MongoDBExport /s.Files:D:\mongoemployees.json /t:DocumentDBBulk /t.ConnectionString:"AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:employees /t.IdField:_id /t.Dates:Epoch /t.CollectionThroughput:2500
```

## <a id="SQL"></a>Импорт из SQL Server

Параметр импорта из источника SQL позволяет импортировать данные из отдельной базы данных SQL Server и фильтровать записи для импорта с помощью запроса. Кроме того, можно изменить структуру документа, указав разделитель вложения (подробнее об этом чуть позже).  

![Снимок экрана: параметры источника SQL — средства миграции базы данных](./media/import-data/sqlexportsource.png)

Формат строки подключения — это стандартный формат строки подключения SQL.

> [!NOTE]
> Используйте команду Verify, чтобы проверить доступ к экземпляру SQL Server, указанному в строке подключения.

Свойство разделителя вложения используется для создания иерархических связей (вложенных документов) во время импорта. Рассмотрим следующий запрос SQL:

`select CAST(BusinessEntityID AS varchar) as Id, Name, AddressType as [Address.AddressType], AddressLine1 as [Address.AddressLine1], City as [Address.Location.City], StateProvinceName as [Address.Location.StateProvinceName], PostalCode as [Address.PostalCode], CountryRegionName as [Address.CountryRegionName] from Sales.vStoreWithAddresses WHERE AddressType='Main Office'`

Он возвращает следующие результаты (показаны частичные результаты):

![Снимок экрана: результаты запроса SQL](./media/import-data/sqlqueryresults.png)

Обратите внимание на псевдонимы, например Address.AddressType и Address.Location.StateProvinceName. Если указать разделитель вложения ., средство импорта создаст вложенные документы Address и Address.Location во время импорта. Ниже приведен пример полученного документа в Azure Cosmos DB.

*{ "id": "956", "Name": "Finer Sales and Service", "Address": { "AddressType": "Main Office", "AddressLine1": "#500-75 O'Connor Street", "Location": { "City": "Ottawa", "StateProvinceName": "Ontario" }, "PostalCode": "K4B 1S2", "CountryRegionName": "Canada" } }*

Ниже приведены некоторые примеры команд для импорта данных из SQL Server:

```console
#Import records from SQL which match a query
dt.exe /s:SQL /s.ConnectionString:"Data Source=<server>;Initial Catalog=AdventureWorks;User Id=advworks;Password=<password>;" /s.Query:"select CAST(BusinessEntityID AS varchar) as Id, * from Sales.vStoreWithAddresses WHERE AddressType='Main Office'" /t:DocumentDBBulk /t.ConnectionString:" AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:Stores /t.IdField:Id /t.CollectionThroughput:2500

#Import records from sql which match a query and create hierarchical relationships
dt.exe /s:SQL /s.ConnectionString:"Data Source=<server>;Initial Catalog=AdventureWorks;User Id=advworks;Password=<password>;" /s.Query:"select CAST(BusinessEntityID AS varchar) as Id, Name, AddressType as [Address.AddressType], AddressLine1 as [Address.AddressLine1], City as [Address.Location.City], StateProvinceName as [Address.Location.StateProvinceName], PostalCode as [Address.PostalCode], CountryRegionName as [Address.CountryRegionName] from Sales.vStoreWithAddresses WHERE AddressType='Main Office'" /s.NestingSeparator:. /t:DocumentDBBulk /t.ConnectionString:" AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:StoresSub /t.IdField:Id /t.CollectionThroughput:2500
```

## <a id="CSV"></a>Импорт CSV-файлов и преобразование CSV в JSON

Параметр импорта из CSV-файла позволяет импортировать один или несколько CSV-файлов. При добавлении для импорта папок, содержащих CSV-файлы, вы можете выполнить рекурсивный поиск файлов во вложенных папках.

![Снимок экрана: параметры источника CSV — преобразование CSV в JSON](media/import-data/csvsource.png)

Как и для источника SQL, свойство разделителя вложения можно использовать для создания иерархических связей (вложенных документов) во время импорта. Рассмотрим следующую строку заголовков и строки данных CSV:

![Снимок экрана: примеры записей CSV — преобразование CSV в JSON](./media/import-data/csvsample.png)

Обратите внимание на псевдонимы, например DomainInfo.Domain_Name и RedirectInfo.Redirecting. Если указать разделитель вложения ., средство импорта создаст вложенные документы DomainInfo и RedirectInfo во время импорта. Ниже приведен пример полученного документа в Azure Cosmos DB.

*{ "DomainInfo": { "Domain_Name": "ACUS.GOV", "Domain_Name_Address": "https:\//www.ACUS.GOV" }, "Federal Agency": "Administrative Conference of the United States", "RedirectInfo": { "Redirecting": "0", "Redirect_Destination": "" }, "id": "9cc565c5-ebcd-1c03-ebd3-cc3e2ecd814d" }*

При импорте из CSV-файлов средство импорта попытается определить сведения о типе для значений, которые не заключены в кавычки (значения, заключенные в кавычки, считаются строками).  Типы определяются в таком порядке: номер, дата и время, логическое значение.  

Следует отметить еще кое-что об импорте CSV-файлов.

1. По умолчанию значения, не заключенные в кавычки, очищаются от табуляции и пробелов, а значения, заключенные в кавычки, остаются в нетронутом виде. Вы можете переопределить это поведение, установив флажок "Обрезать значения, заключенные в кавычки" или воспользовавшись параметром командной строки /s.TrimQuoted.
2. По умолчанию объект null, не заключенный в кавычки, считается значением null. Вы можете переопределить это поведение (то есть сделать так, чтобы объект null, не заключенный в кавычки, считался строкой null). Для этого установите флажок "Считать объект NULL, не заключенный в кавычки, строкой" или настройте параметр командной строки /s.NoUnquotedNulls.

Далее показан пример команды для импорта CSV:

```console
dt.exe /s:CsvFile /s.Files:.\Employees.csv /t:DocumentDBBulk /t.ConnectionString:"AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:Employees /t.IdField:EntityID /t.CollectionThroughput:2500
```

## <a id="AzureTableSource"></a>Импорт из табличного хранилища Azure

Вариант импорта из источника хранилища таблиц Azure позволяет импортировать данные из отдельной таблицы хранилища Azure. При необходимости можно отфильтровать сущности таблицы для импорта.

Можно вывести данные, импортированные из Хранилища таблиц Azure, в таблицы и сущности Azure Cosmos DB для использования с API таблиц. Импортированные данные также можно вывести в коллекции и документы для использования с API SQL. Но API таблиц доступен в качестве целевого объекта только в служебной программе командной строки. Пользовательский интерфейс средства переноса данных не позволяет выполнять экспорт в API таблиц. Дополнительные сведения см. в статье [Import data for use with the Azure Cosmos DB Table API](table-import.md) (Импорт данных для использования с помощью API таблицы Azure DB Cosmos).

![Снимок экрана: параметры источника табличного хранилища Azure](./media/import-data/azuretablesource.png)

Для строки подключения табличного хранилища Azure используется следующий формат:

`DefaultEndpointsProtocol=<protocol>;AccountName=<Account Name>;AccountKey=<Account Key>;`

> [!NOTE]
> Используйте команду Verify, чтобы проверить доступ к экземпляру табличного хранилища Azure, указанному в строке подключения.

Введите имя таблицы Azure, из которой будут импортированы данные. При необходимости можно указать [фильтра](../vs-azure-tools-table-designer-construct-filter-strings.md).

Параметр импорта из табличного хранилища Azure предоставляет следующие дополнительные параметры:

1. Включить внутренние поля
   1. All — включить все внутренние поля (PartitionKey, RowKey и Timestamp)
   2. None — исключить все внутренние поля
   3. RowKey — включить только поле RowKey
2. Выбор столбцов
   1. Фильтры Хранилища таблиц Azure не поддерживают проекции. Если вы хотите импортировать только определенные свойства сущности таблицы Azure, их необходимо добавить в список "Выбор столбцов". Остальные свойства сущностей будут игнорироваться.

Ниже приведен пример команды для импорта данных из хранилища таблиц Azure:

```console
dt.exe /s:AzureTable /s.ConnectionString:"DefaultEndpointsProtocol=https;AccountName=<Account Name>;AccountKey=<Account Key>" /s.Table:metrics /s.InternalFields:All /s.Filter:"PartitionKey eq 'Partition1' and RowKey gt '00001'" /s.Projection:ObjectCount;ObjectSize  /t:DocumentDBBulk /t.ConnectionString:" AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:metrics /t.CollectionThroughput:2500
```

## <a id="DynamoDBSource"></a>Импорт из Amazon DynamoDB

Функция импорта из Amazon DynamoDB позволяет выполнять импорт из отдельной таблицы Amazon DynamoDB. Дополнительно можно отфильтровать импортируемые сущности. Чтобы максимально упростить настройку импорта, представлено несколько шаблонов.

![Снимок экрана: параметры источника DynamoDB Amazon — средства миграции базы данных](./media/import-data/dynamodbsource1.png)

![Снимок экрана: параметры источника DynamoDB Amazon — средства миграции базы данных](./media/import-data/dynamodbsource2.png)

Формат строки подключения Amazon DynamoDB выглядит следующим образом:

`ServiceURL=<Service Address>;AccessKey=<Access Key>;SecretKey=<Secret Key>;`

> [!NOTE]
> Используйте команду Verify, чтобы проверить доступ к экземпляру Amazon DynamoDB, указанному в строке подключения.

Ниже приведен пример команды для импорта данных из Amazon DynamoDB:

```console
dt.exe /s:DynamoDB /s.ConnectionString:ServiceURL=https://dynamodb.us-east-1.amazonaws.com;AccessKey=<accessKey>;SecretKey=<secretKey> /s.Request:"{   """TableName""": """ProductCatalog""" }" /t:DocumentDBBulk /t.ConnectionString:"AccountEndpoint=<Azure Cosmos DB Endpoint>;AccountKey=<Azure Cosmos DB Key>;Database=<Azure Cosmos DB Database>;" /t.Collection:catalogCollection /t.CollectionThroughput:2500
```

## <a id="BlobImport"></a>Импорт из хранилища больших двоичных объектов Azure

JSON-файл, файл экспорта MongoDB и параметры импорта источника файла CSV позволяют импортировать из хранилища больших двоичных объектов Azure один или несколько файлов. Чтобы выбрать файлы для импорта, предоставьте регулярное выражение после указания URL-адреса или ключа учетной записи для контейнера больших двоичных объектов.

![Снимок экрана: параметры исходного файла больших двоичных объектов](./media/import-data/blobsource.png)

Ниже приведен пример команды для импорта JSON-файлов из хранилища больших двоичных объектов Azure:

```console
dt.exe /s:JsonFile /s.Files:"blobs://<account key>@account.blob.core.windows.net:443/importcontainer/.*" /t:DocumentDBBulk /t.ConnectionString:"AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:doctest
```

## <a id="SQLSource"></a>Импорт из коллекции API SQL

Параметр импортера источников Azure Cosmos DB позволяет импортировать данные из коллекций Azure Cosmos DB и, если нужно, фильтровать документы с помощью запроса.  

![Снимок экрана: параметры источника Azure Cosmos DB](./media/import-data/documentdbsource.png)

Для строки подключения Azure Cosmos DB используется следующий формат:

`AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;`

Сроку подключения к учетной записи Azure Cosmos DB можно найти на странице "Ключи" портала Azure, как описано в [статье об управлении учетной записью Azure Cosmos DB](manage-account.md). Имя учетной записи необходимо добавить к строке подключения в следующем формате:

`Database=<CosmosDB Database>;`

> [!NOTE]
> Используйте команду Verify, чтобы проверить доступ к экземпляру Azure Cosmos DB, указанному в строке подключения.

Чтобы импортировать данные из одной коллекции Azure Cosmos DB, введите ее имя. Чтобы выполнить импорт из нескольких коллекций Azure Cosmos DB, введите регулярное выражение для поиска имени одной коллекции либо нескольких коллекций (например, collection01 | collection02 | collection03). Дополнительно можно указать или предоставить файл запроса для фильтрации и формирования импортируемых данных.

> [!NOTE]
> Так как в поле коллекции можно вводить регулярные выражения, то при импорте данных из одной коллекции, имя которой содержит символы регулярного выражения, перед этими символами нужно использовать escape-символы.

Параметр импорта Azure Cosmos DB предоставляет следующие дополнительные параметры:

1. Include Internal Fields (Включить внутренние поля). Указывает, следует ли включать системные свойства документа Azure Cosmos DB в экспорт (например, _rid, _ts).
2. Number of Retries on Failure (Число повторных попыток в случае сбоя). Указывает количество повторных попыток подключения к Azure Cosmos DB при временном сбое (например, прерывание сетевого подключения).
3. Retry Interval (Интервал повтора). Указывает время ожидания между повторными попытками подключения к Azure Cosmos DB при временном сбое (например, прерывание сетевого подключения).
4. Connection Mode (Режим подключения). Указывает режим подключения для Azure Cosmos DB. Доступны варианты: DirectTcp, DirectHttps и Gateway. Режимы прямого подключения быстрее, а режим шлюза более удобен для брандмауэра, так как использует только порт 443.

![Снимок экрана: дополнительные параметры источника Azure Cosmos DB](./media/import-data/documentdbsourceoptions.png)

> [!TIP]
> Средство импорта по умолчанию использует режим подключения DirectTcp. При возникновении проблем с брандмауэром перейдите на режим шлюза, так как он использует только порт 443.

Ниже приведены некоторые примеры команд для импорта данных из Azure Cosmos DB:

```console
#Migrate data from one Azure Cosmos DB collection to another Azure Cosmos DB collections
dt.exe /s:DocumentDB /s.ConnectionString:"AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /s.Collection:TEColl /t:DocumentDBBulk /t.ConnectionString:" AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:TESessions /t.CollectionThroughput:2500

#Migrate data from more than one Azure Cosmos DB collection to a single Azure Cosmos DB collection
dt.exe /s:DocumentDB /s.ConnectionString:"AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /s.Collection:comp1|comp2|comp3|comp4 /t:DocumentDBBulk /t.ConnectionString:"AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:singleCollection /t.CollectionThroughput:2500

#Export an Azure Cosmos DB collection to a JSON file
dt.exe /s:DocumentDB /s.ConnectionString:"AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /s.Collection:StoresSub /t:JsonFile /t.File:StoresExport.json /t.Overwrite /t.CollectionThroughput:2500
```

> [!TIP]
> Средство импорта данных Azure Cosmos DB также поддерживает импорт данных из [эмулятора Azure Cosmos DB](local-emulator.md). При импорте данных из локального эмулятора задайте следующую конечную точку: `https://localhost:<port>`.

## <a id="HBaseSource"></a>Импорт из HBase

Параметр импортера источника HBase позволяет импортировать данные из таблицы HBase и фильтровать данные при необходимости. Чтобы максимально упростить настройку импорта, представлено несколько шаблонов.

![Снимок экрана: параметры источника HBase](./media/import-data/hbasesource1.png)

![Снимок экрана: параметры источника HBase](./media/import-data/hbasesource2.png)

Формат строки подключения HBase Stargate выглядит следующим образом:

`ServiceURL=<server-address>;Username=<username>;Password=<password>`

> [!NOTE]
> Используйте команду Verify, чтобы проверить доступ к экземпляру HBase, указанному в строке подключения.

Ниже приведен пример команды для импорта данных из HBase:

```console
dt.exe /s:HBase /s.ConnectionString:ServiceURL=<server-address>;Username=<username>;Password=<password> /s.Table:Contacts /t:DocumentDBBulk /t.ConnectionString:"AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:hbaseimport
```

## <a id="SQLBulkTarget"></a>Импорт в API SQL (массовый импорт)

Средство массового импорта Azure Cosmos DB позволяет импортировать данные из любого доступного источника, используя хранимую процедуру Azure Cosmos DB для повышения эффективности. Средство поддерживает импорт в одну коллекцию Azure Cosmos DB, содержащую один раздел. Оно также поддерживает сегментированный импорт, когда данные распределяются между несколькими коллекциями Azure Cosmos DB, каждая из которых содержит один раздел. Дополнительные сведения о секционировании данных см. в статье о [секционировании и масштабировании в Azure Cosmos DB](partition-data.md). Кроме того, это средство создает, выполняет и удаляет хранимую процедуру из целевых коллекций.  

![Снимок экрана: параметры массового импорта Azure Cosmos DB](./media/import-data/documentdbbulk.png)

Для строки подключения Azure Cosmos DB используется следующий формат:

`AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;`

Строку подключения учетной записи Azure Cosmos DB можно получить со страницы "Ключи" на портале Azure, как описано в статье об [управлении учетной записью Azure Cosmos DB](manage-account.md), однако в строку подключения необходимо добавить имя базы данных в следующем формате:

`Database=<CosmosDB Database>;`

> [!NOTE]
> Используйте команду Verify, чтобы проверить доступ к экземпляру Azure Cosmos DB, указанному в строке подключения.

Чтобы импортировать данные в одну коллекцию, укажите имя этой коллекции и нажмите кнопку "Добавить". Чтобы импортировать несколько коллекций, нужно либо ввести имя каждой коллекции по отдельности, либо использовать следующий синтаксис для указания нескольких коллекций: *префикс_коллекции*[начальный индекс - конечный индекс]. При указании нескольких коллекций с помощью указанного выше синтаксиса следует учитывать следующие рекомендации:

1. Поддерживаются только шаблоны имени диапазона целых чисел. Например, если указать [0–3], будут созданы такие коллекции: collection0, collection1, collection2, collection3.
2. Вы можете использовать сокращенный синтаксис: если ввести collection[3], будет создан тот же набор коллекций, что и на этапе 1.
3. Вы можете указать несколько подстановок. Например, коллекция [0–1] [0–9] создаст 20 имен коллекций с нулем в начале (collection01… 02… 03).

Указав имена коллекций, выберите нужную пропускную способность коллекций (от 400 до 10 000 ЕЗ). Для повышения производительности выберите большее значение. Дополнительные сведения об уровнях производительности в Azure Cosmos DB см. в [этой статье](performance-levels.md).

> [!NOTE]
> Параметр пропускной способности применяется только для создания коллекции. Если указанная коллекция уже существует, ее пропускная способность не меняется.

При импорте нескольких коллекций средство импорта поддерживает сегментирование на основе хэшей. В этом сценарии укажите свойство документа, которое будет использоваться в качестве ключа раздела. (Если не указать ключ раздела, документы будут сегментированы между целевыми коллекциями случайным образом.)

Дополнительно можно указать, какое поле в источнике импорта следует использовать в качестве свойства идентификатора документа во время импорта в Azure Cosmos DB. Если документы не содержат такое свойство, средство импорта создаст GUID в качестве значения идентификатора.

Во время импорта доступны ряд дополнительных параметров. Во-первых, хотя средство и предоставляет хранимую процедуру массового импорта по умолчанию (BulkInsert.js), вы можете указать собственную хранимую процедуру:

 ![Снимок экрана: параметр хранимой процедуры массового импорта Azure Cosmos DB](./media/import-data/bulkinsertsp.png)

Кроме того, при импорте типов даты (например, из SQL Server или MongoDB) можно выбрать три параметра импорта:

 ![Снимок экрана: параметры импорта даты и времени импорта Azure Cosmos DB](./media/import-data/datetimeoptions.png)

* Строка. Сохраняется как строковое значение.
* Эпоха. Сохраняется как числовое значение эпохи.
* Оба. Сохраняется строковое значение и числовое значение эпохи. Этот параметр создает вложенный документ, например "date_joined": { "Value": "2013-10-21T21:17:25.2410000Z", "Epoch": 1382390245 }

Средство массового импорта Azure Cosmos DB предоставляет следующие дополнительные параметры:

1. Размер пакета. По умолчанию средство использует размер пакета 50.  При импорте больших документов рекомендуется уменьшить размер пакета. И наоборот, при импорте небольших документов рекомендуется увеличить размер пакета.
2. Максимальный размер скрипта (в байтах). По умолчанию средство использует максимальный размер скрипта, равный 512 КБ.
3. Disable Automatic Id Generation (Отключить автоматическое создание идентификатора). Если каждый импортируемый документ содержит поле идентификатора, то выбор этого параметра может повысить производительность. Документы, у которых отсутствует поле уникального идентификатора, не будут импортированы.
4. Update Existing Documents (Обновить существующие документы). По умолчанию средство не заменяет существующие документы с конфликтами идентификаторов. При выборе этого параметра существующие документы с совпадающими идентификаторами будут перезаписываться. Эта функция пригодится для плановых миграций, которые используются для обновления существующих документов.
5. Number of Retries on Failure (Число повторных попыток в случае сбоя). Указывает частоту повторных попыток подключения к Azure Cosmos DB при временных сбоях (например, при прерывании сетевого подключения).
6. Retry Interval (Интервал повтора). Указывает время ожидания между повторными попытками подключения к Azure Cosmos DB при временном сбое (например, прерывание сетевого подключения).
7. Connection Mode (Режим подключения). Указывает режим подключения для Azure Cosmos DB. Доступны варианты: DirectTcp, DirectHttps и Gateway. Режимы прямого подключения быстрее, а режим шлюза более удобен для брандмауэра, так как использует только порт 443.

![Снимок экрана: дополнительные параметры массового импорта Azure Cosmos DB](./media/import-data/docdbbulkoptions.png)

> [!TIP]
> Средство импорта по умолчанию использует режим подключения DirectTcp. При возникновении проблем с брандмауэром перейдите на режим шлюза, так как он использует только порт 443.

## <a id="SQLSeqTarget"></a>Импорт в API SQL (последовательный импорт записей)

Средство последовательного импорта записей Azure Cosmos DB позволяет импортировать данные из доступного источника по одной записи. Этот параметр можно выбрать при импорте в существующую коллекцию, для которой достигнута квота хранимых процедур. Средство поддерживает импорт в одну коллекцию Azure Cosmos DB (состоящую из одного или нескольких разделов). Оно также поддерживает сегментированный импорт, когда данные распределяются между несколькими коллекциями Azure Cosmos DB, каждая из которых содержит один или несколько разделов. Дополнительные сведения о секционировании данных см. в статье о [секционировании и масштабировании в Azure Cosmos DB](partition-data.md).

![Снимок экрана: параметры последовательного импорта записей Azure Cosmos DB](./media/import-data/documentdbsequential.png)

Для строки подключения Azure Cosmos DB используется следующий формат:

`AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;`

Строку подключения для учетной записи Azure Cosmos DB можно найти на странице "Ключи" портала Azure, как описано в [статье об управлении учетной записью Azure Cosmos DB](manage-account.md). Имя учетной записи необходимо добавить к строке подключения в следующем формате:

`Database=<Azure Cosmos DB Database>;`

> [!NOTE]
> Используйте команду Verify, чтобы проверить доступ к экземпляру Azure Cosmos DB, указанному в строке подключения.

Чтобы импортировать отдельную коллекцию, введите имя коллекции, в которую импортируются данные, и нажмите кнопку "Добавить". Чтобы импортировать несколько коллекций, введите имя каждой коллекции по отдельности. Чтобы указать несколько коллекций, можно использовать следующий синтаксис: *префикс_коллекции*[начальный индекс - конечный индекс]. При указании нескольких коллекций с помощью указанного выше синтаксиса следует учитывать следующие рекомендации:

1. Поддерживаются только шаблоны имени диапазона целых чисел. Например, если указать [0–3], будут созданы такие коллекции: collection0, collection1, collection2, collection3.
2. Вы можете использовать сокращенный синтаксис: если ввести collection[3], будет создан тот же набор коллекций, что и на этапе 1.
3. Вы можете указать несколько подстановок. Например, коллекция [0–1] [0–9] создает 20 имен коллекций с нулем в начале (collection01… 02… 03).

Указав имена коллекций, выберите нужную пропускную способность коллекций (от 400 до 250 000 ЕЗ). Для повышения производительности выберите большее значение. Дополнительные сведения об уровнях производительности в Azure Cosmos DB см. в [этой статье](performance-levels.md). Для любой операции импорта в коллекции с пропускной способностью выше 10 000 ЕЗ потребуется ключ секции. Если требуется пропускная способность более 250 000 ЕЗ, вам нужно будет отправить запрос на портал, чтобы увеличить ограничения учетной записи.

> [!NOTE]
> Параметр пропускной способности применяется только для создания коллекции или базы данных. Если указанная коллекция уже существует, ее пропускная способность не меняется.

При импорте нескольких коллекций средство импорта поддерживает сегментирование на основе хэшей. В этом сценарии укажите свойство документа, которое будет использоваться в качестве ключа раздела. (Если не указать ключ раздела, документы будут сегментированы между целевыми коллекциями случайным образом.)

Дополнительно можно указать, какое поле в источнике импорта следует использовать в качестве свойства идентификатора документа во время импорта в Azure Cosmos DB. (Если документы не содержат такое свойство, средство импорта создаст GUID в качестве значения идентификатора.)

Во время импорта доступны ряд дополнительных параметров. При импорте типов даты (например, из SQL Server или MongoDB) можно выбрать три параметра импорта:

 ![Снимок экрана: параметры импорта даты и времени импорта Azure Cosmos DB](./media/import-data/datetimeoptions.png)

* Строка. Сохраняется как строковое значение.
* Эпоха. Сохраняется как числовое значение эпохи.
* Оба. Сохраняется строковое значение и числовое значение эпохи. Этот параметр создает вложенный документ, например "date_joined": { "Value": "2013-10-21T21:17:25.2410000Z", "Epoch": 1382390245 }

Средство последовательного импорта записей Azure Cosmos DB предоставляет следующие дополнительные параметры:

1. Количество параллельных запросов. По умолчанию средство использует два параллельных запроса. При импорте небольших документов рекомендуется увеличить число параллельных запросов. Если задать слишком большое число, при импорте может происходить ограничение скорости.
2. Disable Automatic Id Generation (Отключить автоматическое создание идентификатора). Если каждый импортируемый документ содержит поле идентификатора, то выбор этого параметра может повысить производительность. Документы, у которых отсутствует поле уникального идентификатора, не будут импортированы.
3. Update Existing Documents (Обновить существующие документы). По умолчанию средство не заменяет существующие документы с конфликтами идентификаторов. При выборе этого параметра существующие документы с совпадающими идентификаторами будут перезаписываться. Эта функция пригодится для плановых миграций, которые используются для обновления существующих документов.
4. Number of Retries on Failure (Число повторных попыток в случае сбоя). Указывает частоту повторных попыток подключения к Azure Cosmos DB при временных сбоях (например, при прерывании сетевого подключения).
5. Retry Interval (Интервал повтора). Указывает время ожидания между повторными попытками подключения к Azure Cosmos DB при временных сбоях (например, при прерывании подключения).
6. Connection Mode (Режим подключения). Указывает режим подключения для Azure Cosmos DB. Доступны варианты: DirectTcp, DirectHttps и Gateway. Режимы прямого подключения быстрее, а режим шлюза более удобен для брандмауэра, так как использует только порт 443.

![Снимок экрана: дополнительные параметры последовательного импорта записей Azure Cosmos DB](./media/import-data/documentdbsequentialoptions.png)

> [!TIP]
> Средство импорта по умолчанию использует режим подключения DirectTcp. При возникновении проблем с брандмауэром перейдите на режим шлюза, так как он использует только порт 443.

## <a id="IndexingPolicy"></a>Настройка политики индексации

Если вы разрешили средству переноса создавать коллекции API SQL в Azure Cosmos DB во время импорта, можно указать политику индексирования коллекций. В разделе дополнительных параметров массового импорта Azure Cosmos DB и параметров последовательной записи Azure Cosmos DB перейдите в раздел "Политика индексации".

![Снимок экрана: дополнительные параметры политики индексации Azure Cosmos DB](./media/import-data/indexingpolicy1.png)

С помощью дополнительного параметра политики индексации можно выбрать файл политики индексации, вручную ввести политику индексации или выбрать из набора шаблонов по умолчанию (щелкнув правой кнопкой в текстовом поле политики индексации).

Шаблоны политик, предоставляемые средством:

* По умолчанию. Эта политика лучше всего подходит для запросов с проверкой равенства строк. Она также подходит при использовании выражения ORDER BY, диапазона и запросов с проверкой равенства чисел. Эта политика имеет более низкий индекс служебных данных хранилища, чем "Диапазон".
* Диапазон. Эта политика лучше всего подходит при использовании выражения ORDER BY, диапазона и запросов с проверкой равенства чисел и строк. Эта политика имеет более высокий индекс служебных данных хранилища, чем "По умолчанию" или "Хэш".

![Снимок экрана: дополнительные параметры политики индексации Azure Cosmos DB](./media/import-data/indexingpolicy2.png)

> [!NOTE]
> Если не указать политику индексации, будет применена политика по умолчанию. Дополнительные сведения о политиках индексации Azure Cosmos DB см. в [этой статье](index-policy.md).

## <a name="export-to-json-file"></a>Экспорт в файл JSON

Средство экспорта JSON Azure Cosmos DB позволяет экспортировать любые доступные источники в JSON-файл, содержащий массив документов JSON. Средство выполняет экспорт автоматически. Также можно просмотреть полученную команду миграции и выполнить ее самостоятельно. Результирующий JSON-файл может храниться локально или в хранилище больших двоичных объектов Azure.

![Снимок экрана: параметры экспорта в локальный файл Azure Cosmos DB JSON](./media/import-data/jsontarget.png)

![Снимок экрана: параметр экспорта в хранилище BLOB-объектов Azure Cosmos DB JSON](./media/import-data/jsontarget2.png)

Дополнительно полученный объект JSON можно отформатировать для чтения. Эта операция увеличивает размер полученного документа, но делает его более удобным для чтения.

* Стандартный экспорт JSON

  ```JSON
  [{"id":"Sample","Title":"About Paris","Language":{"Name":"English"},"Author":{"Name":"Don","Location":{"City":"Paris","Country":"France"}},"Content":"Don's document in Azure Cosmos DB is a valid JSON document as defined by the JSON spec.","PageViews":10000,"Topics":[{"Title":"History of Paris"},{"Title":"Places to see in Paris"}]}]
  ```

* Экспорт JSON, отформатированный для чтения

  ```JSON
    [
     {
    "id": "Sample",
    "Title": "About Paris",
    "Language": {
      "Name": "English"
    },
    "Author": {
      "Name": "Don",
      "Location": {
        "City": "Paris",
        "Country": "France"
      }
    },
    "Content": "Don's document in Azure Cosmos DB is a valid JSON document as defined by the JSON spec.",
    "PageViews": 10000,
    "Topics": [
      {
        "Title": "History of Paris"
      },
      {
        "Title": "Places to see in Paris"
      }
    ]
    }]
  ```

Ниже приведен пример команды для экспорта JSON-файлов в хранилище BLOB-объектов Azure:

```console
dt.exe /ErrorDetails:All /s:DocumentDB /s.ConnectionString:"AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB database_name>" /s.Collection:<CosmosDB collection_name>
/t:JsonFile /t.File:"blobs://<Storage account key>@<Storage account name>.blob.core.windows.net:443/<Container_name>/<Blob_name>"
/t.Overwrite
```

## <a name="advanced-configuration"></a>Расширенная конфигурация

На экране расширенной конфигурации укажите расположение файла журнала, в который нужно записывать сообщения об ошибках. К этой странице применяются такие правила.

1. Если имя файла не указано, все сообщения об ошибках возвращаются на страницу результатов.
2. Если указано имя файла, но не указан каталог, то файл будет создан (или перезаписан) в текущем каталоге среды.
3. Если выбрать существующий файл, то этот файл будет перезаписан. Добавление в конец файла не поддерживается.
4. Затем укажите, какие сообщение об ошибках необходимо регистрировать в журнале — все, критические или никакие. И наконец, решите, как часто будет обновляться сообщение о переносе данных на экране.

   ![Снимок экрана: расширенная конфигурация экрана](./media/import-data/AdvancedConfiguration.png)

## <a name="confirm-import-settings-and-view-command-line"></a>Подтверждение параметров импорта и просмотр командной строки

1. Указав сведения об источнике данных и целевом объекте миграции, а также дополнительные параметры, просмотрите сводку миграции или (при необходимости) скопируйте полученную команду миграции. (Копирование команды может понадобиться, если вы выполняете автоматизацию операций импорта.)

    ![Снимок экрана: окно сводки](./media/import-data/summary.png)

    ![Снимок экрана: окно сводки](./media/import-data/summarycommand.png)

2. После проверки источника и назначения нажмите кнопку **Импорт**. Затраченное время, число передаваемых объектов и сведения об ошибках (если вы не указали имя файла при настройке расширенной конфигурации) обновляются в процессе импорта. После завершения вы можете экспортировать результаты (например, для обработки ошибок импорта).

    ![Снимок экрана: параметры экспорта в Azure Cosmos DB JSON](./media/import-data/viewresults.png)

3. Также можно начать новую операцию импорта, сбросив все значения или сохранив существующие параметры. (Например, можно сохранить сведения о строке подключения, источнике данных, целевом объекте и т. п.)

    ![Снимок экрана: параметры экспорта в Azure Cosmos DB JSON](./media/import-data/newimport.png)

## <a name="next-steps"></a>Дополнительная информация

В этом руководстве вы выполнили следующие задачи:

> [!div class="checklist"]
> * установка средства миграции данных;
> * импорт данных из разных источников данных;
> * экспорт данных из Azure Cosmos DB в JSON.

Теперь вы можете перейти к следующему руководству, из которого вы узнаете, как запрашивать данные с помощью Azure Cosmos DB.

> [!div class="nextstepaction"]
>[Как выполнять запросы с помощью SQL в базе данных Azure Cosmos DB](../cosmos-db/tutorial-query-sql-api.md)
