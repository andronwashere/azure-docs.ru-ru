---
title: Краткое руководство. Использование API Gremlin с Python в Azure Cosmos DB
description: В этом руководстве показано, как использовать API Gremlin в Azure Cosmos DB для создания консольного приложения с помощью портала Azure и Python
author: luisbosquez
ms.service: cosmos-db
ms.subservice: cosmosdb-graph
ms.devlang: python
ms.topic: quickstart
ms.date: 01/22/2019
ms.author: lbosq
ms.openlocfilehash: 18f54e64498c7809e662ceb71d1e497a0f275ad6
ms.sourcegitcommit: cababb51721f6ab6b61dda6d18345514f074fb2e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/04/2019
ms.locfileid: "66480447"
---
# <a name="quickstart-create-a-graph-database-in-azure-cosmos-db-using-python-and-the-azure-portal"></a>Краткое руководство. Создание графовой базы данных в Azure Cosmos DB с помощью Python и портала Azure

> [!div class="op_single_selector"]
> * [Консоль Gremlin](create-graph-gremlin-console.md)
> * [.NET](create-graph-dotnet.md)
> * [Java](create-graph-java.md)
> * [Node.js](create-graph-nodejs.md)
> * [Python](create-graph-python.md)
> * [PHP](create-graph-php.md)
>  

В этом руководстве показано, как использовать Python и [API Gremlin](graph-introduction.md) в Azure Cosmos DB для сборки консольного приложения путем клонирования примера из репозитория GitHub. Кроме того, здесь показано, как создать учетную запись Azure Cosmos DB с помощью веб-портала Azure.   

Azure Cosmos DB — это глобально распределенная многомодельная служба базы данных Майкрософт. Вы можете быстро создавать и запрашивать документы, таблицы, пары "ключ — значение" и базы данных графов, используя возможности глобального распределения и горизонтального масштабирования Azure Cosmos DB.  

> [!NOTE]
> Для работы с этим кратким руководством требуется учетная запись базы данных графа, созданной после 20 декабря 2017 г. Существующие учетные записи будут поддерживать Python после переноса в общедоступную версию.

## <a name="prerequisites"></a>Предварительные требования

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)] Кроме того, можно воспользоваться [бесплатной пробной версией Azure Cosmos DB](https://azure.microsoft.com/try/cosmosdb/) без подписки Azure, оплаты и каких-либо обязательств.

Кроме того, сделайте следующее:
* [Python](https://www.python.org/downloads/) версии 3.5 или более поздней
* [Диспетчер пакетов pip](https://pip.pypa.io/en/stable/installing/)
* [Git](https://git-scm.com/)
* [Драйвер Python для Gremlin](https://github.com/apache/tinkerpop/tree/master/gremlin-python)

## <a name="create-a-database-account"></a>Создание учетной записи базы данных

Перед созданием графовой базы данных необходимо создать учетную запись графовой базы данных Gremlin с Azure Cosmos DB.

[!INCLUDE [cosmos-db-create-dbaccount-graph](../../includes/cosmos-db-create-dbaccount-graph.md)]

## <a name="add-a-graph"></a>Добавление графа

[!INCLUDE [cosmos-db-create-graph](../../includes/cosmos-db-create-graph.md)]

## <a name="clone-the-sample-application"></a>Клонирование примера приложения

Теперь перейдем к работе с кодом. Мы клонируем приложение API Gremlin из GitHub, зададим строку подключения и запустим приложение. Вы узнаете, как можно упростить работу с данными программным способом.  

1. Откройте командную строку, создайте папку git-samples, а затем закройте окно командной строки.

    ```bash
    md "C:\git-samples"
    ```

2. Откройте окно терминала git, например git bash, и выполните команду `cd`, чтобы перейти в папку для установки примера приложения.  

    ```bash
    cd "C:\git-samples"
    ```

3. Выполните команду ниже, чтобы клонировать репозиторий с примером. Эта команда создает копию примера приложения на локальном компьютере. 

    ```bash
    git clone https://github.com/Azure-Samples/azure-cosmos-db-graph-python-getting-started.git
    ```

## <a name="review-the-code"></a>Просмотр кода

Этот шаг не является обязательным. Если вы хотите узнать, как создать в коде ресурсы базы данных, изучите приведенные ниже фрагменты кода. Они взяты из файла connect.py, который расположен в папке C:\git-samples\azure-cosmos-db-graph-python-getting-started\. Если вас это не интересует, можете сразу переходить к разделу [Обновление строки подключения](#update-your-connection-information). 

* Gremlin `client` инициализируется в строке 104 файла `connect.py`:

    ```python
    ...
    client = client.Client('wss://<YOUR_ENDPOINT>.gremlin.cosmosdb.azure.com:443/','g', 
        username="/dbs/<YOUR_DATABASE>/colls/<YOUR_COLLECTION_OR_GRAPH>", 
        password="<YOUR_PASSWORD>")
    ...
    ```

* Последовательность шагов Gremlin объявляется в начале файла `connect.py`. Затем они выполняются с помощью метода `client.submitAsync()`:

    ```python
    client.submitAsync(_gremlin_cleanup_graph)
    ```

## <a name="update-your-connection-information"></a>Обновление информации о подключении

Теперь вернитесь на портал Azure, чтобы получить данные для подключения. Скопируйте эти данные в приложение. Эти настройки обеспечат обмен данными между вашим приложением и размещенной базой данных.

1. На [портале Azure](https://portal.azure.com/) щелкните **Ключи**. 

    Скопируйте первую часть значения URI.

    ![Просмотр и копирование ключа доступа на портале Azure, страница "Ключи"](./media/create-graph-python/keys.png)

2. Откройте файл connect.py и в строке 104 вставьте значение URI вместо `<YOUR_ENDPOINT>`:

    ```python
    client = client.Client('wss://<YOUR_ENDPOINT>.gremlin.cosmosdb.azure.com:443/','g', 
        username="/dbs/<YOUR_DATABASE>/colls/<YOUR_COLLECTION_OR_GRAPH>", 
        password="<YOUR_PASSWORD>")
    ```

    Часть URI в клиентском объекте теперь должна выглядеть следующим образом:

    ```python
    client = client.Client('wss://test.gremlin.cosmosdb.azure.com:443/','g', 
        username="/dbs/<YOUR_DATABASE>/colls/<YOUR_COLLECTION_OR_GRAPH>", 
        password="<YOUR_PASSWORD>")
    ```

3. Измените второй параметр объекта `client`, чтобы заменить строки `<YOUR_DATABASE>` и `<YOUR_COLLECTION_OR_GRAPH>`. Если вы использовали предложенные значения, параметр должен выглядеть следующим образом:

    `username="/dbs/sample-database/colls/sample-graph"`

    Весь объект `client` теперь должен выглядеть следующим образом:

    ```python
    client = client.Client('wss://test.gremlin.cosmosdb.azure.com:443/','g', 
        username="/dbs/sample-database/colls/sample-graph", 
        password="<YOUR_PASSWORD>")
    ```

4. На портале Azure с помощью кнопки "Копировать" скопируйте первичный ключ и вставьте его вместо `<YOUR_PASSWORD>` в параметр `password=<YOUR_PASSWORD>`.

    Теперь все определение объекта `client` должно выглядеть следующим образом:
    ```python
    client = client.Client('wss://test.gremlin.cosmosdb.azure.com:443/','g', 
        username="/dbs/sample-database/colls/sample-graph", 
        password="asdb13Fadsf14FASc22Ggkr662ifxz2Mg==")
    ```

6. Сохраните файл `connect.py`.

## <a name="run-the-console-app"></a>Запуск консольного приложения

1. В окне терминала Git перейдите в папку azure-cosmos-db-graph-python-getting-started folder с помощью команды `cd`.

    ```git
    cd "C:\git-samples\azure-cosmos-db-graph-python-getting-started"
    ```

2. В окне терминала Git введите следующую команду, чтобы установить необходимые пакеты Python.

   ```
   pip install -r requirements.txt
   ```

3. В окне терминала Git выполните следующую команду, чтобы запустить приложение Python.
    
    ```
    python connect.py
    ```

    В окне терминала появятся вершины и ребра, добавляемые в граф. 
    
    Если возникают ошибки времени ожидания, проверьте, правильно ли вы [указали сведения о подключении](#update-your-connection-information), и попробуйте еще раз выполнить последнюю команду. 
    
    Когда программа завершит работу, нажмите клавишу ВВОД и переключитесь на окно веб-браузера с порталом Azure.

<a id="add-sample-data"></a>
## <a name="review-and-add-sample-data"></a>Просмотр и добавление демонстрационных данных

Теперь можно вернуться в обозреватель данных, чтобы просмотреть вершины, добавленные в граф, и добавить дополнительные точки данных.

1. Щелкните **Обозреватель данных**, затем разверните **sample-graph**, щелкните **Граф** и нажмите кнопку **Применить фильтр**. 

   ![Создание документов в обозревателе данных на портале Azure](./media/create-graph-python/azure-cosmosdb-data-explorer-expanded.png)

2. В списке **результатов** обратите внимание на новых пользователей, добавленных в граф. Выберите пользователя **ben**. Вы увидите, что он связан с пользователем robin. Здесь можно перетаскивать вершины мышью, увеличивать или уменьшать масштаб колесиком мыши, а также увеличивать размер графа с помощью двойной стрелки. 

   ![Новые вершины в графе в обозревателе данных на портале Azure](./media/create-graph-python/azure-cosmosdb-graph-explorer-new.png)

3. Давайте добавим несколько новых пользователей. Нажмите кнопку **New Vertex** (Создать вершину), чтобы добавить данные в граф.

   ![Создание документов в обозревателе данных на портале Azure](./media/create-graph-python/azure-cosmosdb-data-explorer-new-vertex.png)

4. Введите метку *person*.

5. Щелкните **Добавить свойство**, чтобы поочередно добавить каждое из указанных ниже свойств. Обратите внимание, что вы можете создать уникальные свойства для каждого пользователя в графе. Требуется только ключ идентификатора.

    key|value|Примечания
    ----|----|----
    id|ashley|Уникальный идентификатор вершины. Если не указать идентификатор, он создастся автоматически.
    gender|Женский| 
    Технология | java | 

    > [!NOTE]
    > В этом кратком руководстве создается несекционированная коллекция. Но если вы создаете секционированную коллекцию, указав ключ раздела во время создания коллекции, включите ключ раздела в качестве ключа каждой новой вершины. 

6. Последовательно выберите **ОК**. Чтобы увидеть кнопку **ОК** в нижней части экрана, может потребоваться развернуть экран.

7. Еще раз нажмите кнопку **New Vertex** (Создать вершину), чтобы добавить нового пользователя. 

8. Введите метку *person*.

9. Щелкните **Добавить свойство**, чтобы поочередно добавить следующие свойства:

    key|value|Примечания
    ----|----|----
    id|rakesh|Уникальный идентификатор вершины. Если не указать идентификатор, он создастся автоматически.
    gender|Мужской| 
    Учебное заведение|MIT| 

10. Последовательно выберите **ОК**. 

11. Нажмите кнопку **Применить фильтр** со стандартным значением фильтра `g.V()`, чтобы отобразить все значения графа. Все пользователи теперь отображаются в списке **Результаты**. 

    По мере добавления новых данных используйте фильтры для ограничения результатов. По умолчанию обозреватель данных использует `g.V()` для получения всех вершин в графе. Вы можете задать для него другой [запрос графа](tutorial-query-graph.md), например `g.V().count()`, который возвращает число всех вершин графа в формате JSON. Если вы изменили фильтр, для возвращения к полному списку результатов снова установите фильтр `g.V()` и щелкните **Применить фильтр**.

12. Теперь можно будет соединить пользователей rakesh и ashley. Выберите в списке **Результаты** пользователя **ashley**, а затем нажмите кнопку редактирования рядом с разделом **Целевые объекты** в нижнем правом углу. Чтобы отобразить область **Свойства**, может потребоваться развернуть окно.

    ![Изменение целевого объекта вершины в графе](./media/create-graph-python/azure-cosmosdb-data-explorer-edit-target.png)

13. В поле **Целевой объект** введите *rakesh*, затем в поле **Граничная метка** введите слово *знает* и щелкните значок галочки.

    ![Создание связи между пользователями ashley и rakesh в обозревателе данных](./media/create-graph-python/azure-cosmosdb-data-explorer-set-target.png)

14. Теперь выберите пользователя **rakesh** в списке результатов. Вы увидите, что пользователи ashley и rakesh связаны. 

    ![Две вершины, связанные в обозревателе данных](./media/create-graph-python/azure-cosmosdb-graph-explorer.png)

    На этом часть руководства, посвященная созданию ресурсов, завершена. Вы можете дополнить граф новыми вершинами, а также изменить существующие вершины или запросы. Теперь давайте изучим метрики, которые предоставляет Azure Cosmos DB, а затем очистим все ресурсы. 

## <a name="review-slas-in-the-azure-portal"></a>Просмотр соглашений об уровне обслуживания на портале Azure

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a>Очистка ресурсов

[!INCLUDE [cosmosdb-delete-resource-group](../../includes/cosmos-db-delete-resource-group.md)]

## <a name="next-steps"></a>Дополнительная информация

В этом кратком руководстве вы узнали, как создать учетную запись Azure Cosmos DB, граф с помощью обозревателя данных, а также как запустить приложение. Теперь вы можете создавать более сложные запросы и внедрять эффективную логику обхода графа с помощью Gremlin. 

> [!div class="nextstepaction"]
> [Как выполнять запросы к данным в базе данных Azure Cosmos DB с помощью API Graph (предварительная версия)](tutorial-query-graph.md)

