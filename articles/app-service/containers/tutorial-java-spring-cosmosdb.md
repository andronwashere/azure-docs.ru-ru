---
title: Создание веб-приложения Java в Службе приложений Azure в Linux
description: Создание, развертывание и масштабирование веб-приложений Java Spring Boot с помощью Службы приложений Azure в Linux и Azure Cosmos DB.
author: rloutlaw
ms.author: routlaw
manager: angerobe
ms.service: app-service-web
ms.devlang: java
ms.topic: tutorial
ms.date: 12/10/2018
ms.custom: seodec18, seo-java-july2019
ms.openlocfilehash: 625e40b4dcb8ac558831508b292fd5cbcfb90b5b
ms.sourcegitcommit: bafb70af41ad1326adf3b7f8db50493e20a64926
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/25/2019
ms.locfileid: "68489458"
---
# <a name="tutorial-build-a-java-web-app-using-spring-and-azure-cosmos-db"></a>Руководство. Создание веб-приложения Java с использованием Spring и Azure Cosmos DB

В данном руководстве описывается процесс создания, настройки, развертывания и масштабирования веб-приложений Java в Azure. Когда вы выполните инструкции руководства, у вас будет приложение [Spring Boot](https://projects.spring.io/spring-boot/), данные которого хранятся в [Azure Cosmos DB](/azure/cosmos-db), которая работает в [Службе приложений Azure на Linux](/azure/app-service/containers).

![Приложение Java, работающее в службе приложений Azure](./media/tutorial-java-spring-cosmosdb/spring-todo-app-running-locally.jpg)

Из этого руководства вы узнаете, как выполнять следующие задачи:

> [!div class="checklist"]
> * Создание базы данных Cosmos DB
> * Подключение примера приложения к базе данных и его локальное тестирование
> * Развертывание примера приложения в Azure
> * Потоковая передача журналов диагностики из службы приложений
> * Добавление дополнительных экземпляров для масштабирования примера приложения

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Предварительные требования

* [Azure CLI](https://docs.microsoft.com/cli/azure/overview), установленный на компьютере. 
* [Git](https://git-scm.com/)
* [Java JDK](https://aka.ms/azure-jdks)
* [Maven](https://maven.apache.org)

## <a name="clone-the-sample-todo-app-and-prepare-the-repo"></a>Клонирование примера приложения со списком задач и подготовка репозитория

В этом руководстве используется пример приложения со списком задач с веб-интерфейсом, который вызывает REST API Spring, поддерживаемый [Azure Cosmos DB Spring Data](https://github.com/Microsoft/spring-data-cosmosdb). Код для приложения можно найти на сайте [GitHub](https://github.com/Microsoft/spring-todo-app). Дополнительные сведения о создании приложений Java с помощью Spring и Cosmos DB см. в статье [Использование начального приложения Spring Boot с API SQL Azure Cosmos DB](https://docs.microsoft.com/java/azure/spring-framework/configure-spring-boot-starter-java-app-with-cosmos-db ) и [Spring Data for Azure Cosmos DB](https://github.com/Microsoft/spring-data-cosmosdb#quick-start) (Spring Data для Azure Cosmos DB).


Выполните следующие команды в терминале, чтобы клонировать репозиторий и настроить среду примера приложения.

```bash
git clone --recurse-submodules https://github.com/Azure-Samples/e2e-java-experience-in-app-service-linux-part-2.git
cd e2e-java-experience-in-app-service-linux-part-2
yes | cp -rf .prep/* .
```

## <a name="create-an-azure-cosmos-db"></a>Создание Azure Cosmos DB

Чтобы создать базу данных Azure Cosmos DB в подписке, выполните следующие действия. Приложение списка задач подключится к этой базе данных и сохранит данные при запуске, сохраняя состояние приложения независимо от того, где вы его запускаете.

1. Войдите в Azure CLI и при необходимости настройте подписку, если у вас есть несколько подключенных к учетным данным для входа.

    ```bash
    az login
    az account set -s <your-subscription-id>
    ```   

2. Создайте группу ресурсов Azure, указав ее имя.

    ```bash
    az group create -n <your-azure-group-name> \
        -l <your-resource-group-region>
    ```

3. Создайте Azure Cosmos DB с видом `GlobalDocumentDB`. В имени Cosmos DB нужно использовать только строчные буквы. Запишите поле `documentEndpoint` в ответе от команды.

    ```bash
    az cosmosdb create --kind GlobalDocumentDB \
        -g <your-azure-group-name> \
        -n <your-azure-COSMOS-DB-name-in-lower-case-letters>
    ```

4. Получите ключ Azure Cosmos DB, чтобы подключиться к приложению. Сохраните `primaryMasterKey` и `documentEndpoint` поблизости, так как они потребуются на следующем шаге.

    ```bash
    az cosmosdb list-keys -g <your-azure-group-name> -n <your-azure-COSMOSDB-name>
    ```

## <a name="configure-the-todo-app-properties"></a>Настройка свойств приложения со списком задач

Откройте окно терминала на компьютере. Скопируйте пример файла сценария в клонированный репозиторий, чтобы его можно было настроить для только что созданной базы данных Cosmos DB.

```bash
cd initial/spring-todo-app
cp set-env-variables-template.sh .scripts/set-env-variables.sh
```
 
Отредактируйте `.scripts/set-env-variables.sh` в любом удобном редакторе и укажите сведения о подключении Azure Cosmos DB. Для конфигурации службы приложений на платформе Linux используйте тот же регион, что и раньше (`your-resource-group-region`), и группу ресурсов (`your-azure-group-name`), используемую при создании базы данных Cosmos DB. Выберите уникальное имя WEBAPP_NAME, так как оно не должно совпадать с любым именем веб-приложения в любом развертывании Azure.

```bash
export COSMOSDB_URI=<put-your-COSMOS-DB-documentEndpoint-URI-here>
export COSMOSDB_KEY=<put-your-COSMOS-DB-primaryMasterKey-here>
export COSMOSDB_DBNAME=<put-your-COSMOS-DB-name-here>

# App Service Linux Configuration
export RESOURCEGROUP_NAME=<put-your-resource-group-name-here>
export WEBAPP_NAME=<put-your-Webapp-name-here>
export REGION=<put-your-REGION-here>
```

Затем выполните скрипт:

```bash
source .scripts/set-env-variables.sh
```
   
Эти переменные среды используются в `application.properties` в приложении со списком задач. Поля в файле свойств задают конфигурацию репозиторию по умолчанию для Spring Data:

```properties
azure.cosmosdb.uri=${COSMOSDB_URI}
azure.cosmosdb.key=${COSMOSDB_KEY}
azure.cosmosdb.database=${COSMOSDB_DBNAME}
```

```java
@Repository
public interface TodoItemRepository extends DocumentDbRepository<TodoItem, String> {
}
```

Затем пример приложения использует заметку `@Document`, импортированную из `com.microsoft.azure.spring.data.cosmosdb.core.mapping.Document`, чтобы настроить тип сущности, который будет хранить и контролировать Cosmos DB:

```java
@Document
public class TodoItem {
    private String id;
    private String description;
    private String owner;
    private boolean finished;
```

## <a name="run-the-sample-app"></a>Запуск примера приложения

Используйте Maven, чтобы запустить пример.

```bash
mvn package spring-boot:run
```

Полученный результат должен выглядеть примерно так:

```bash
bash-3.2$ mvn package spring-boot:run
[INFO] Scanning for projects...
[INFO] 
[INFO] ------------------------------------------------------------------------
[INFO] Building spring-todo-app 2.0-SNAPSHOT
[INFO] ------------------------------------------------------------------------
[INFO] 


[INFO] SimpleUrlHandlerMapping - Mapped URL path [/webjars/**] onto handler of type [class org.springframework.web.servlet.resource.ResourceHttpRequestHandler]
[INFO] SimpleUrlHandlerMapping - Mapped URL path [/**] onto handler of type [class org.springframework.web.servlet.resource.ResourceHttpRequestHandler]
[INFO] WelcomePageHandlerMapping - Adding welcome page: class path resource [static/index.html]
2018-10-28 15:04:32.101  INFO 7673 --- [           main] c.m.azure.documentdb.DocumentClient      : Initializing DocumentClient with serviceEndpoint [https://sample-cosmos-db-westus.documents.azure.com:443/], ConnectionPolicy [ConnectionPolicy [requestTimeout=60, mediaRequestTimeout=300, connectionMode=Gateway, mediaReadMode=Buffered, maxPoolSize=800, idleConnectionTimeout=60, userAgentSuffix=;spring-data/2.0.6;098063be661ab767976bd5a2ec350e978faba99348207e8627375e8033277cb2, retryOptions=com.microsoft.azure.documentdb.RetryOptions@6b9fb84d, enableEndpointDiscovery=true, preferredLocations=null]], ConsistencyLevel [null]
[INFO] AnnotationMBeanExporter - Registering beans for JMX exposure on startup
[INFO] TomcatWebServer - Tomcat started on port(s): 8080 (http) with context path ''
[INFO] TodoApplication - Started TodoApplication in 45.573 seconds (JVM running for 76.534)
```

После запуска приложения со списком задач Spring к нему можно получить доступ локально с помощью этой ссылки: [http://localhost:8080/](http://localhost:8080/).

 ![Приложение Java, работающее в службе приложений Azure](./media/tutorial-java-spring-cosmosdb/spring-todo-app-running-locally.jpg)

Если вместо сообщения "Приложение со списком задач запущено" отображаются исключения, убедитесь, что скрипт `bash` на предыдущем шаге правильно экспортировал переменные среды и что значения для созданной базы данных Azure Cosmos DB верны.

## <a name="configure-azure-deployment"></a>Настройка развертывания в Azure

Откройте файл `pom.xml` в каталоге `initial/spring-boot-todo` и добавьте конфигурацию [модуля Maven для Службы приложений Azure](https://github.com/Microsoft/azure-maven-plugins/blob/develop/azure-webapp-maven-plugin/README.md).

```xml    
<plugins> 

    <!--*************************************************-->
    <!-- Deploy to Java SE in App Service Linux           -->
    <!--*************************************************-->
       
    <plugin>
        <groupId>com.microsoft.azure</groupId>
            <artifactId>azure-webapp-maven-plugin</artifactId>
            <version>1.4.0</version>
            <configuration>
            <deploymentType>jar</deploymentType>
            
            <!-- Web App information -->
            <resourceGroup>${RESOURCEGROUP_NAME}</resourceGroup>
            <appName>${WEBAPP_NAME}</appName>
            <region>${REGION}</region>
            
            <!-- Java Runtime Stack for Web App on Linux-->
            <linuxRuntime>jre8</linuxRuntime>
            
            <appSettings>
                <property>
                    <name>COSMOSDB_URI</name>
                    <value>${COSMOSDB_URI}</value>
                </property>
                <property>
                    <name>COSMOSDB_KEY</name>
                    <value>${COSMOSDB_KEY}</value>
                </property>
                <property>
                    <name>COSMOSDB_DBNAME</name>
                    <value>${COSMOSDB_DBNAME}</value>
                </property>
                <property>
                    <name>JAVA_OPTS</name>
                    <value>-Dserver.port=80</value>
                </property>
            </appSettings>
            
        </configuration>
    </plugin>            
    ...
</plugins>
```

## <a name="deploy-to-app-service-on-linux"></a>Развертывание в cлужбу приложений на платформе Linux

Используйте целевой объект Maven `azure-webapp:deploy`, чтобы развернуть приложение со списком задач в Службу приложений Azure на платформе Linux.

```bash

# Deploy
bash-3.2$ mvn azure-webapp:deploy
[INFO] Scanning for projects...
[INFO] 
[INFO] ------------------------------------------------------------------------
[INFO] Building spring-todo-app 2.0-SNAPSHOT
[INFO] ------------------------------------------------------------------------
[INFO] 
[INFO] --- azure-webapp-maven-plugin:1.4.0:deploy (default-cli) @ spring-todo-app ---
[INFO] Authenticate with Azure CLI 2.0
[INFO] Target Web App doesn't exist. Creating a new one...
[INFO] Creating App Service Plan 'ServicePlanb6ba8178-5bbb-49e7'...
[INFO] Successfully created App Service Plan.
[INFO] Successfully created Web App.
[INFO] Trying to deploy artifact to spring-todo-app...
[INFO] Successfully deployed the artifact to https://spring-todo-app.azurewebsites.net
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time: 02:19 min
[INFO] Finished at: 2018-10-28T15:32:03-07:00
[INFO] Final Memory: 50M/574M
[INFO] ------------------------------------------------------------------------
```

Выходные данные содержат URL-адрес развернутого приложения (в этом примере — `https://spring-todo-app.azurewebsites.net`). Этот URL-адрес можно скопировать в веб-браузер или выполнить следующую команду в окне терминала, чтобы загрузить приложение.

```bash
open https://spring-todo-app.azurewebsites.net
```

Вы должны увидеть приложение, которое выполняется с удаленным URL-адресом, в адресной строке:

 ![Приложение Java, работающее в службе приложений Azure](./media/tutorial-java-spring-cosmosdb/spring-todo-app-running-in-app-service.jpg)

## <a name="stream-diagnostic-logs"></a>Потоковая передача журналов диагностики

[!INCLUDE [Access diagnostic logs](../../../includes/app-service-web-logs-access-no-h.md)]


## <a name="scale-out-the-todo-app"></a>Масштабирование приложения со списком задач

Выполните масштабирование приложения, добавив другую рабочую роль:

```bash
az appservice plan update --number-of-workers 2 \
   --name ${WEBAPP_PLAN_NAME} \
   --resource-group <your-azure-group-name>
```

## <a name="clean-up-resources"></a>Очистка ресурсов

Если эти ресурсы не требуются для изучения другого руководства (см. раздел [Дальнейшие действия](#next)), их можно удалить, выполнив следующие команды в Cloud Shell: 
  
```bash
az group delete --name <your-azure-group-name>
```

<a name="next"></a>

## <a name="next-steps"></a>Дополнительная информация

[Azure для разработчиков Java](/java/azure/)
[Spring Boot](https://spring.io/projects/spring-boot), [Использование начального приложения Spring Boot с API SQL Azure Cosmos DB](/java/azure/spring-framework/configure-spring-boot-starter-java-app-with-cosmos-db?view=azure-java-stable), [Добро пожаловать в базу данных Azure Cosmos DB](/azure/cosmos-db/sql-api-introduction), [Вводные сведения о службе приложений Azure на платформе Linux](app-service-linux-intro.md).

Дополнительные сведения о запуске приложений Java в Службе приложений Azure в Linux см. в статье

> [!div class="nextstepaction"] 
> [Руководство разработчика для Java для службы приложений в Linux](configure-language-java.md).
