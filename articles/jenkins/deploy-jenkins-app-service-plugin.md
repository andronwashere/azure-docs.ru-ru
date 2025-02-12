---
title: Развертывание в службе приложений Azure с помощью подключаемого модуля Jenkins
description: Узнайте, как развернуть веб-приложение Java в Azure в Jenkins с использованием подключаемого модуля Jenkins службы приложений Azure.
ms.service: jenkins
keywords: jenkins, azure, devops, app service
author: tomarchermsft
manager: jeconnoc
ms.author: tarcher
ms.topic: tutorial
ms.date: 07/31/2018
ms.openlocfilehash: 29a842f7dfcf720f29fcff80d2e736893c824f5a
ms.sourcegitcommit: 24fd3f9de6c73b01b0cee3bcd587c267898cbbee
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/20/2019
ms.locfileid: "65949554"
---
# <a name="deploy-to-azure-app-service-by-using-the-jenkins-plugin"></a>Развертывание в службе приложений Azure с помощью подключаемого модуля Jenkins 

Для развертывания веб-приложения Java в Azure можно использовать Azure CLI в [конвейере Jenkins](/azure/jenkins/execute-cli-jenkins-pipeline) или [подключаемый модуль Jenkins службы приложений Azure](https://plugins.jenkins.io/azure-app-service). Подключаемый модуль Jenkins версии 1.0 поддерживает непрерывное развертывание с помощью функции "Веб-приложения" службы приложений Azure, используя следующие инструменты:
* Отправка файлов.
* Docker для веб-приложений в Linux.

Из этого руководства вы узнаете, как выполнять следующие задачи:
> [!div class="checklist"]
> * Настройка Jenkins для развертывания веб-приложений с помощью отправки файлов.
> * Настройка Jenkins для развертывания веб-приложений для контейнеров.

## <a name="create-and-configure-a-jenkins-instance"></a>Создание и настройка экземпляра Jenkins

Если у вас нет главного экземпляра Jenkins, воспользуйтесь [шаблоном решений](install-jenkins-solution-template.md), который содержит комплект разработчиков Java (JDK) версии 8 и следующие необходимые подключаемые модули Jenkins:

* [подключаемый модуль Jenkins клиента Git](https://plugins.jenkins.io/git-client) версии 2.4.6; 
* [подключаемый модуль Docker Commons](https://plugins.jenkins.io/docker-commons) версии 1.4.0;
* [учетные данные Azure](https://plugins.jenkins.io/azure-credentials) версии 1.2;
* [службу приложений Azure](https://plugins.jenkins.io/azure-app-service) версии 0.1.

Подключаемый модуль Jenkins можно использовать для развертывания веб-приложений на всех языках, поддерживаемых веб-приложениями, например C#, PHP, Java и Node.js. В этом руководстве мы используем [простое веб-приложение Java для Azure](https://github.com/azure-devops/javawebappsample). Чтобы создать разветвление репозитория для своей учетной записи GitHub, нажмите кнопку **Fork** (Разветвление) в правом верхнем углу интерфейса GitHub.  

> [!NOTE]
> Для создания проекта Java требуются Java JDK и Maven. Установите эти компоненты в главном экземпляре Jenkins или агенте виртуальной машины при его использовании для обеспечения непрерывной интеграции. Если вы развертываете приложение Java SE, на сервере сборки также потребуется развертывание ZIP.
>

Чтобы установить эти компоненты, войдите в экземпляр Jenkins с помощью SSH-подключения и выполните следующие команды:

```bash
sudo apt-get install -y openjdk-7-jdk
sudo apt-get install -y maven
```

Чтобы развернуть веб-приложения для контейнеров, установите Docker в главном экземпляре Jenkins или агенте виртуальной машины, который используется для сборки. Инструкции см. в статье [Choose Docker CE or Docker EE on Ubuntu](https://docs.docker.com/engine/installation/linux/ubuntu/) (Выбор Docker CE и Docker EE в Ubuntu).

## <a name="service-principal"></a> Добавление субъекта-службы Azure в учетные данные Jenkins

Для развертывания в Azure требуется субъект-служба Azure. 


1. Для создания субъекта-службы Azure используйте [Azure CLI](/cli/azure/create-an-azure-service-principal-azure-cli?toc=%2fazure%2fazure-resource-manager%2ftoc.json)  или [портал Azure](/azure/azure-resource-manager/resource-group-create-service-principal-portal).
2. На панели мониторинга Jenkins выберите **Credentials** (Учетные данные) > **System** (Система). Затем выберите **Global credentials (unrestricted)** (Глобальные учетные данные (неограниченные)).
3. Чтобы добавить субъект-службу Microsoft Azure, выберите **Add Credentials** (Добавить учетные данные). Укажите значения в полях **идентификатора подписки**, **идентификатора клиента**, **секрета клиента** и **конечной точки маркера OAuth 2.0**. Задайте **mySp** в качестве значения поля **идентификатора**. Мы будем использовать этот идентификатор в последующих шагах этой статьи.


## <a name="configure-jenkins-to-deploy-web-apps-by-uploading-files"></a>Настройка Jenkins для развертывания веб-приложений путем отправки файлов

Чтобы развернуть проект в веб-приложении, можно отправить артефакты сборки с помощью отправки файлов. Служба приложений поддерживает разные способы развертывания. Подключаемый модуль Jenkins Службы приложений Azure упрощает работу, наследуя параметр развертывания на основе типа файла. 

* Для приложений Java EE используется [развертывание WAR](/azure/app-service/deploy-zip#deploy-war-file).
* Для приложений Java SE используется [развертывание ZIP](/azure/app-service/deploy-zip#deploy-zip-file).
* Для других языков используется [развертывание Git](/azure/app-service/deploy-local-git).

Перед настройкой задания в Jenkins требуется план службы приложений Azure и веб-приложение для запуска приложения Java.


1. Создайте план службы приложений Azure с ценовой категорией **Бесплатный** с помощью [команды Azure CLI](/cli/azure/appservice/plan#az-appservice-plan-create) `az appservice plan create`. От плана службы приложений зависят физические ресурсы, используемые для размещения приложений. Все приложения, назначенные плану службы приложений, совместно используют эти ресурсы. Эти ресурсы позволяют сэкономить при размещении нескольких приложений.
2. Создайте веб-приложение. Воспользуйтесь [порталом Azure](/azure/app-service/configure-common) или выполните следующую команду `az` Azure CLI:
    ```azurecli-interactive 
    az webapp create --name <myAppName> --resource-group <myResourceGroup> --plan <myAppServicePlan>
    ```
    
3. Настройте конфигурацию среды выполнения Java, необходимую вашему приложению. Следующая команда Azure CLI настраивает веб-приложение для запуска в JDK 8 и [Apache Tomcat](https://tomcat.apache.org/) версии 8.0.
    ```azurecli-interactive
    az webapp config set \
    --name <myAppName> \
    --resource-group <myResourceGroup> \
    --java-version 1.8 \
    --java-container Tomcat \
    --java-container-version 8.0
    ```

### <a name="set-up-the-jenkins-job"></a>Настройка задания Jenkins

1. Создайте **универсальный** проект на панели мониторинга Jenkins.
2. Настройте поле **управления исходным кодом** для использования локальной вилки [простого веб-приложения Java для Azure](https://github.com/azure-devops/javawebappsample). Укажите значение **URL-адреса репозитория**. Например, http:\//github.com/&lt;your_ID>/javawebappsample.
3. Добавьте шаг для создания проекта с помощью Maven. Для этого добавьте команду **оболочки выполнения**. В этом примере требуется дополнительная команда для переименования \*WAR-файла в целевой папке на **ROOT.war**.   
    ```bash
    mvn clean package
    mv target/*.war target/ROOT.war
    ```

4. Добавьте действие после сборки, выбрав **Publish an Azure Web App** (Публикация веб-приложения Azure).
5. Укажите **mySp** в качестве субъекта-службы Azure. Этот субъект был сохранен в качестве [учетных данных Azure](#service-principal) на предыдущем шаге.
6. В разделе **конфигурации приложения** выберите группу ресурсов и веб-приложение в своей подписке. Подключаемый модуль Jenkins автоматически определяет платформу веб-приложения (Windows или Linux). Для веб-приложения Windows появляется параметр **Publish Files** (Публикация файлов).
7. Заполните файлы, которые нужно развернуть. Например, укажите пакет в формате WAR, если вы используете Java. Используйте дополнительные параметры **исходного** и **целевого каталогов**, чтобы указать исходные и целевые папки для отправки файла. Веб-приложение Java в Azure выполняется на сервере Tomcat. Поэтому для Java необходимо отправить пакет в формате WAR в папку веб-приложений. В этом примере задайте для **исходного каталога** значение **target**, а для **целевого каталога** — **webapps**.
8. Если вы хотите выполнить развертывание в слот, отличный от рабочего, можно также задать имя **слота**.
9. Сохраните проект и создайте его. Веб-приложение развертывается в Azure после сборки.

### <a name="deploy-web-apps-by-uploading-files-using-jenkins-pipeline"></a>Развертывание веб-приложений путем отправки файлов с помощью конвейера Jenkins

Подключаемый модуль Jenkins службы приложений Azure готов к использованию в конвейере. Следующий пример доступен в репозитории GitHub.

1. В интерфейсе GitHub откройте файл **Jenkinsfile_ftp_plugin**. Чтобы изменить файл, щелкните значок карандаша. Обновите определения **resourceGroup** и **webAppName** для веб-приложения в строках 11 и 12 соответственно:
    ```java
    def resourceGroup = '<myResourceGroup>'
    def webAppName = '<myAppName>'
    ```

2. Задайте определение **withCredentials** в строке 14 для идентификатора учетных данных в вашем экземпляре Jenkins.
    ```java
    withCredentials([azureServicePrincipal('<mySp>')]) {
    ```

### <a name="create-a-jenkins-pipeline"></a>Создание конвейера Jenkins

1. Откройте Jenkins в веб-браузере. Выберите **New Item** (Создать элемент).
2. Укажите имя задания и выберите **Pipeline** (Конвейер). Нажмите кнопку **ОК**.
3. Откройте вкладку **Pipeline** (Конвейер).
4. Для значения **Definition** (Определение) выберите значение **Pipeline script from SCM** (Сценарий конвейера из SCM).
5. Для значения **SCM** выберите **Git**. Введите URL-адрес GitHub для разветвленного репозитория. Например: https://&lt;ваш_разветвленный_репозиторий>.git.
6. Измените значение **пути к сценарию** на **Jenkinsfile_ftp_plugin**.
7. Нажмите кнопку **Сохранить** (Сохранить) и запустите задание.

## <a name="configure-jenkins-to-deploy-web-app-for-containers"></a>Настройка Jenkins для развертывания веб-приложений для контейнеров

Веб-приложения в Linux поддерживают развертывание с помощью Docker. Чтобы развернуть веб-приложение с помощью Docker, необходимо предоставить файл Dockerfile, который упаковывает веб-приложение со средой выполнения службы в образ Docker. Затем подключаемый модуль Jenkins создаст образ, отправит его в реестр Docker и развернет в веб-приложении.

Веб-приложения на платформе Linux также поддерживают традиционные методы развертывания, такие как Git и отправка файлов, но только для встроенных языков (.NET Core, Node.js, PHP и Ruby). Для других языков необходимо объединить код приложения и среду выполнения службы в образ Docker и использовать Docker для развертывания.

Перед настройкой задания в Jenkins вам понадобится веб-приложение в Linux, а также реестр контейнеров для хранения частных образов контейнера Docker и управления ими. DockerHub можно использовать для создания реестра контейнеров. В этом примере используется реестр контейнеров Azure.

* Создайте веб-приложение в Linux, используя сведения [этой статьи](../app-service/containers/quickstart-nodejs.md).
* Реестр контейнеров Azure — это управляемая служба [реестра Docker](https://docs.docker.com/registry/) на базе реестра Docker версии 2.0 с открытым кодом. Создайте реестр контейнеров Azure, используя сведения [этой статьи](/azure/container-registry/container-registry-get-started-azure-cli). Кроме того, можно использовать DockerHub.

### <a name="set-up-the-jenkins-job-for-docker"></a>Настройка задания Jenkins для Docker

1. Создайте **универсальный** проект на панели мониторинга Jenkins.
2. Настройте поле **управления исходным кодом** для использования локальной вилки [простого веб-приложения Java для Azure](https://github.com/azure-devops/javawebappsample). Укажите значение **URL-адреса репозитория**. Например, http:\//github.com/&lt;your_ID>/javawebappsample.
3. Добавьте шаг для создания проекта с помощью Maven. Для этого добавьте команду **оболочки выполнения**. Добавьте следующую строку в команду:
    ```bash
    mvn clean package
    ```

4. Добавьте действие после сборки, выбрав **Publish an Azure Web App** (Публикация веб-приложения Azure).
5. Укажите **mySp** в качестве субъекта-службы Azure. Этот субъект был сохранен в качестве [учетных данных Azure](#service-principal) на предыдущем шаге.
6. В разделе **конфигурации приложения** выберите группу ресурсов и веб-приложение Linux в своей подписке.
7. Выберите **Publish via Docker** (Публикация через Docker).
8. Заполните значение пути **Dockerfile**. Вы можете сохранить значение по умолчанию /Dockerfile.
При использовании реестра контейнеров Azure для значения **URL-адреса реестра Docker** укажите URL-адрес в формате https://&lt;ваш_реестр>.azurecr.io. Если вы используете DockerHub, оставьте это значение пустым.
9. Для значения **учетных данных реестра** добавьте учетные данные для реестра контейнеров. Идентификатор пользователя и пароль можно получить, выполнив следующие команды в Azure CLI. Первая команда включает учетную запись администратора.
    ```azurecli-interactive
    az acr update -n <yourRegistry> --admin-enabled true
    az acr credential show -n <yourRegistry>
    ```

10. Имя образа Docker и значение тега на вкладке **Дополнительно** являются необязательными. По умолчанию значение имени образа можно получить из имени образа, настроенного на портале Azure (в параметрах **контейнера Docker**). Тег создается на основе $BUILD_NUMBER.
    > [!NOTE]
    > Обязательно задайте имя образа на портале Azure или задайте значение **образа Docker** на вкладке **Дополнительно**. В этом примере задайте &lt;ваш_реестр>.azurecr.io/calculator в качестве значения **образа Docker** и не указывайте значение **тега образа Docker**.

11. Развертывание завершается сбоем, если использовать параметр встроенного образа Docker. Настройте конфигурацию Docker для использования пользовательского образа в настройках **контейнера Docker** на портале Azure. Для развертывания встроенного изображения используйте подход с передачей файла.
12. Кроме того, можно выбрать имя **слота**, отличное от **рабочего**. Этот способ аналогичный способу передачи файла.
13. Сохраните и создайте проект. После этого образ контейнера помещается в реестр, а веб-приложение развертывается.

### <a name="deploy-web-app-for-containers-by-using-jenkins-pipeline"></a>Развертывание веб-приложения для контейнеров с помощью конвейера Jenkins

1. В интерфейсе GitHub откройте файл **Jenkinsfile_container_plugin**. Чтобы изменить файл, щелкните значок карандаша. Обновите определения **resourceGroup** и **webAppName** для веб-приложения в строках 11 и 12 соответственно:
    ```java
    def resourceGroup = '<myResourceGroup>'
    def webAppName = '<myAppName>'
    ```

2. Измените строку 13 на сервер контейнера реестра:
    ```java
    def registryServer = '<registryURL>'
    ```

3. Измените строку 16 для использования идентификатора учетных данных в вашем экземпляре Jenkins:
    ```java
    azureWebAppPublish azureCredentialsId: '<mySp>', publishType: 'docker', resourceGroup: resourceGroup, appName: webAppName, dockerImageName: imageName, dockerImageTag: imageTag, dockerRegistryEndpoint: [credentialsId: 'acr', url: "http://$registryServer"]
    ```

### <a name="create-a-jenkins-pipeline"></a>Создание конвейера Jenkins    

1. Откройте Jenkins в веб-браузере. Выберите **New Item** (Создать элемент).
2. Укажите имя задания и выберите **Pipeline** (Конвейер). Нажмите кнопку **ОК**.
3. Откройте вкладку **Pipeline** (Конвейер).
4. Для значения **Definition** (Определение) выберите значение **Pipeline script from SCM** (Сценарий конвейера из SCM).
5. Для значения **SCM** выберите **Git**. Введите URL-адрес GitHub для разветвленного репозитория. Например: https://&lt;ваш_разветвленный_репозиторий>.git.
7. Измените значение **пути к сценарию** на **Jenkinsfile_container_plugin**.
8. Нажмите кнопку **Сохранить** (Сохранить) и запустите задание.

## <a name="verify-your-web-app"></a>Проверка веб-приложения

1. Чтобы проверить успешное развертывание WAR-файла в веб-приложении, откройте веб-браузер.
2. Перейдите по адресу: http://&lt;имя_вашего_приложения>.azurewebsites.net/api/calculator/ping. Замените значение &lt;имя_вашего_приложения> на имя вашего веб-приложения. Отобразится сообщение:
    ```
    Welcome to Java Web App!!! This is updated!
    Sun Jun 17 16:39:10 UTC 2017
    ```

3. Перейдите по адресу: http://&lt;имя_вашего_приложения>.azurewebsites.net/api/calculator/add?x=&lt;x>&y=&lt;y>. Замените &lt;x> и &lt;y> любыми числами для получения сумы x + y. Калькулятор отображает сумму: ![Калькулятор: сложение](./media/execute-cli-jenkins-pipeline/calculator-add.png)

### <a name="for-azure-app-service-on-linux"></a>Служба приложений Azure под управлением Linux

1. Чтобы проверить веб-приложение, выполните следующую команду в Azure CLI:
    ```CLI
    az acr repository list -n <myRegistry> -o json
    ```
    Отобразится следующее сообщение:
    ```CLI
    ["calculator"]
    ```
    
2. Перейдите по адресу: http://&lt;имя_вашего_приложения>.azurewebsites.net/api/calculator/ping. Замените значение &lt;имя_вашего_приложения> на имя вашего веб-приложения. Отобразится сообщение: 
    ```
    Welcome to Java Web App!!! This is updated!
    Sun Jul 09 16:39:10 UTC 2017
    ```

3. Перейдите по адресу: http://&lt;имя_вашего_приложения>.azurewebsites.net/api/calculator/add?x=&lt;x>&y=&lt;y>. Замените &lt;x> и &lt;y> любыми числами для получения сумы x + y.
    
## <a name="troubleshooting-the-jenkins-plugin"></a>Устранение неполадок подключаемого модуля Jenkins

Если вы столкнулись с ошибками, которые касаются подключаемых модулей Jenkins, сообщите о проблеме конкретного компонента в [JENKS JIRA](https://issues.jenkins-ci.org/).

## <a name="next-steps"></a>Дополнительная информация

В этом руководстве вы выполнили развертывание в Azure с помощью подключаемого модуля Jenkins службы приложений Azure.

Вы научились выполнять следующие задачи:

> [!div class="checklist"]
> * Настройка Jenkins для развертывания Службы приложений Azure с помощью отправки файлов 
> * Настаивать Jenkins для развертывания веб-приложений для контейнеров. 
