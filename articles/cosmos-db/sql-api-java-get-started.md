---
title: Руководство по NoSQL. Пакет SDK для создания приложения Java с помощью API-интерфейса SQL для Azure Cosmos DB
description: Руководство по NoSQL, в котором создается оперативная база данных и консольное приложение Java с помощью API-интерфейса SQL для Azure Cosmos DB. Azure SQL — это база данных NoSQL для JSON.
author: SnehaGunda
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.devlang: java
ms.topic: tutorial
ms.date: 12/22/2018
ms.author: sngun
ms.openlocfilehash: 55bdcf9847f2194f269b92aed830a66c79c4d337
ms.sourcegitcommit: 6b41522dae07961f141b0a6a5d46fd1a0c43e6b2
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/15/2019
ms.locfileid: "67985664"
---
# <a name="nosql-tutorial-build-a-sql-api-java-console-application"></a>Руководство по NoSQL. Создание консольного приложения Java с помощью API-интерфейса SQL

> [!div class="op_single_selector"]
> * [.NET](sql-api-get-started.md)
> * [Java](sql-api-java-get-started.md)
> * [Async Java](sql-api-async-java-get-started.md)
> * [Node.js](sql-api-nodejs-get-started.md)
> 

Приветствуем вас в разделе руководства по NoSQL, посвященного пакету SDK для создания консольного приложения Java с помощью API-интерфейса SQL для Azure Cosmos DB. После изучения этого руководства у вас будет консольное приложение, которое создает ресурсы Azure Cosmos DB и отправляет запросы к ним.

Мы рассмотрим:

* создание учетной записи Azure Cosmos DB и подключение к ней;
* настройка решения Visual Studio;
* Создание оперативной базы данных
* создание коллекции;
* создание документов JSON;
* выполнение запросов к коллекции;
* создание документов JSON;
* выполнение запросов к коллекции;
* замена документа;
* удаление документа;
* удаление базы данных.

А теперь приступим к работе!

## <a name="prerequisites"></a>Предварительные требования
Вам потребуются:

* Активная учетная запись Azure. Если у вас ее нет, зарегистрируйте [бесплатную учетную запись](https://azure.microsoft.com/free/). 

  [!INCLUDE [cosmos-db-emulator-docdb-api](../../includes/cosmos-db-emulator-docdb-api.md)]

* [Git](https://git-scm.com/downloads).
* [Комплект разработчика Java (JDK 7 +)](https://aka.ms/azure-jdks).
* [Maven](https://maven.apache.org/download.cgi).

## <a name="step-1-create-an-azure-cosmos-db-account"></a>Шаг 1. создание учетной записи Azure Cosmos DB;
Давайте создадим учетную запись Azure Cosmos DB. Если у вас уже есть учетная запись, которую вы собираетесь использовать, можно перейти к [клонированию проекта GitHub](#GitClone). При использовании эмулятора Azure Cosmos DB выполните действия, описанные в [этой статье](local-emulator.md), чтобы настроить эмулятор и сразу перейти к [клонированию проекта GitHub](#GitClone).

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

## <a id="GitClone"></a>Шаг 2. Клонирование проекта GitHub
[Чтобы приступить к работе с Azure Cosmos DB и Java](https://github.com/Azure-Samples/documentdb-java-getting-started), сначала клонируйте репозиторий GitHub. Например, чтобы получить образец проекта в локальной среде, в локальном каталоге выполните следующую команду.

    git clone git@github.com:Azure-Samples/azure-cosmos-db-documentdb-java-getting-started.git

    cd azure-cosmos-db-documentdb-java-getting-started

В этом каталоге находится файл проекта `pom.xml` и папка с исходным кодом Java, `src`. Кроме того, здесь также находится файл `Program.java` с инструкциями по выполнению простых операций с Azure Cosmos DB, таких как создание документов и запрос данных в коллекции. В файле `pom.xml` содержится зависимость от [пакета Java SDK для Azure Cosmos DB в Maven](https://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb).

    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-documentdb</artifactId>
        <version>LATEST</version>
    </dependency>

## <a id="Connect"></a>Шаг 3. Подключение к учетной записи Azure Cosmos DB
Далее вернитесь на [портал Azure](https://portal.azure.com), чтобы получить конечную точку и первичный главный ключ. Конечная точка и первичный ключ Azure Cosmos DB позволяют приложению предоставлять данные о расположении, в котором будет устанавливаться подключение, делая подключение вашего приложения доверенным для Azure Cosmos DB.

На портале Azure перейдите к учетной записи Azure Cosmos DB, а затем щелкните **Ключи**. Скопируйте универсальный код ресурса (URI) с портала и вставьте его в параметр `https://FILLME.documents.azure.com` в файле Program.java. Затем скопируйте на портале значение поля "Первичный ключ" и вставьте его в параметр `FILLME`.

    this.client = new DocumentClient(
        "https://FILLME.documents.azure.com",
        "FILLME"
        , new ConnectionPolicy(),
        ConsistencyLevel.Session);

![Снимок экрана: использование портала Azure для создания консольного приложения Java согласно руководству по NoSQL Отображается учетная запись Azure Cosmos DB со следующими выделенными элементами: активный концентратор, кнопка "Ключи" в колонке учетной записи Azure Cosmos DB, а также значение универсального кода ресурса (URI), первичный и вторичный ключи в колонке "Ключи".][keys]

## <a name="step-4-create-a-database"></a>Шаг 4. Создание базы данных
[Базу данных](databases-containers-items.md#azure-cosmos-databases) Azure Cosmos DB можно создать с помощью метода [createDatabase](/java/api/com.microsoft.azure.documentdb.documentclient.createdatabase) класса **DocumentClient**. База данных представляет собой логический контейнер для хранения документов JSON, разделенных между коллекциями.

    Database database = new Database();
    database.setId("familydb");
    this.client.createDatabase(database, null);

## <a id="CreateColl"></a>Шаг 5. Создание коллекции
> [!WARNING]
> Элемент **createCollection** создает коллекцию с зарезервированной пропускной способностью и соответствующей ценой. Дополнительные сведения см. на [странице цен](https://azure.microsoft.com/pricing/details/cosmos-db/).
> 
> 

Вы можете создать коллекцию с помощью метода [createCollection](/java/api/com.microsoft.azure.documentdb.documentclient.createcollection), который относится к классу **DocumentClient**. Коллекция представляет собой контейнер документов JSON и связанную с ними логику в виде приложения JavaScript.


    DocumentCollection collectionInfo = new DocumentCollection();
    collectionInfo.setId("familycoll");

    // Azure Cosmos DB collections can be reserved with throughput specified in request units/second. 
    // Here we create a collection with 400 RU/s.
    RequestOptions requestOptions = new RequestOptions();
    requestOptions.setOfferThroughput(400);

    this.client.createCollection("/dbs/familydb", collectionInfo, requestOptions);

## <a id="CreateDoc"></a>Шаг 6. создание документов JSON;
Вы можете создать документ с помощью метода [createDocument](/java/api/com.microsoft.azure.documentdb.documentclient.createdocument), который относится к классу **DocumentClient**. Документы относятся к пользовательскому (произвольному) содержимому JSON. Теперь мы можем добавить один или несколько документов. Если у вас уже есть данные, которые вы хотите хранить в базе данных, вы можете использовать [средство миграции данных](import-data.md) Azure Cosmos DB, чтобы импортировать эти данные в базу данных.

    // Insert your Java objects as documents 
    Family andersenFamily = new Family();
    andersenFamily.setId("Andersen.1");
    andersenFamily.setLastName("Andersen")

    // More initialization skipped for brevity. You can have nested references
    andersenFamily.setParents(new Parent[] { parent1, parent2 });
    andersenFamily.setDistrict("WA5");
    Address address = new Address();
    address.setCity("Seattle");
    address.setCounty("King");
    address.setState("WA");

    andersenFamily.setAddress(address);
    andersenFamily.setRegistered(true);

    this.client.createDocument("/dbs/familydb/colls/familycoll", family, new RequestOptions(), true);

![На схеме представлены иерархические отношения между учетной записью, оперативной базой данных, коллекцией и документами, используемыми в руководстве по NoSQL при создании консольного приложения Java.](./media/sql-api-get-started/nosql-tutorial-account-database.png)

## <a id="Query"></a>Шаг 7. Запрашивание ресурсов Azure Cosmos DB
Azure Cosmos DB поддерживает [полнофункциональные запросы](how-to-sql-query.md) к документам JSON, хранящимся в каждой коллекции.  Ниже приведен образец кода, который позволяет запросить документы в Azure Cosmos DB, используя синтаксис SQL с методом [queryDocuments](/java/api/com.microsoft.azure.documentdb.documentclient.querydocuments).

    FeedResponse<Document> queryResults = this.client.queryDocuments(
        "/dbs/familydb/colls/familycoll",
        "SELECT * FROM Family WHERE Family.lastName = 'Andersen'", 
        null);

    System.out.println("Running SQL query...");
    for (Document family : queryResults.getQueryIterable()) {
        System.out.println(String.format("\tRead %s", family));
    }

## <a id="ReplaceDocument"></a>Шаг 8. Замена документа JSON
Azure Cosmos DB поддерживает обновление документов JSON с помощью метода [replaceDocument](/java/api/com.microsoft.azure.documentdb.documentclient.replacedocument).

    // Update a property
    andersenFamily.Children[0].Grade = 6;

    this.client.replaceDocument(
        "/dbs/familydb/colls/familycoll/docs/Andersen.1", 
        andersenFamily,
        null);

## <a id="DeleteDocument"></a>Шаг 9. Удаление документа JSON
Аналогичным образом Azure Cosmos DB поддерживает удаление документов JSON с помощью метода [deleteDocument](/java/api/com.microsoft.azure.documentdb.documentclient.deletedocument).  

    this.client.delete("/dbs/familydb/colls/familycoll/docs/Andersen.1", null);

## <a id="DeleteDatabase"></a>Шаг 10. Удаление базы данных
Удаление созданной базы данных влечет удаление всех ее дочерних ресурсов (коллекций, документов и т. д.).

    this.client.deleteDatabase("/dbs/familydb", null);

## <a id="Run"></a>Шаг 11. Запуск консольного приложения Java
Чтобы запустить приложение из консоли, перейдите в папку проекта и выполните компиляцию с помощью Maven:
    
    mvn package

Команда `mvn package` позволяет скачать последнюю версию библиотеки Azure Cosmos DB из Maven и возвращает `GetStarted-0.0.1-SNAPSHOT.jar`. Затем запустите приложение, выполнив следующую команду:

    mvn exec:java -D exec.mainClass=GetStarted.Program

Поздравляем! Вы завершили работу с руководством по NoSQL и создали работающее консольное приложение Java.

## <a name="next-steps"></a>Дополнительная информация
* Дополнительные сведения о создании веб-приложения Java См. сведения в статье о [создании веб-приложения Java с использованием Azure Cosmos DB](sql-api-java-application.md).
* Узнайте, как выполнять [мониторинг учетной записи Azure Cosmos DB](monitor-accounts.md).
* Отправьте запросы образцу набора данных в [Площадке для запросов](https://www.documentdb.com/sql/demo).

[keys]: media/sql-api-get-started/nosql-tutorial-keys.png
