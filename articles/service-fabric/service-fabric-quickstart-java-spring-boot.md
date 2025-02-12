---
title: Создание приложения Spring Boot в Service Fabric в Azure | Документы Майкрософт
description: В этом кратком руководстве вы развернете приложение Spring Boot для Azure Service Fabric, используя пример приложения Spring Boot.
services: service-fabric
documentationcenter: java
author: suhuruli
manager: msfussell
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: java
ms.topic: quickstart
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 01/29/2019
ms.author: suhuruli
ms.custom: mvc, devcenter
ms.openlocfilehash: f7cf3f4cc0ceba89c031f5c36e90bbd6ef3dd20a
ms.sourcegitcommit: 4b431e86e47b6feb8ac6b61487f910c17a55d121
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/18/2019
ms.locfileid: "68327160"
---
# <a name="quickstart-deploy-a-java-spring-boot-application-to-service-fabric"></a>Краткое руководство. Развертывание приложения Java Spring Boot в Service Fabric

Azure Service Fabric — это платформа распределенных систем для развертывания микрослужб и контейнеров и управления ими.

В этом руководстве объясняется, как развернуть приложение Spring Boot в Service Fabric. Это краткое руководство использует пример [Начало работы](https://spring.io/guides/gs/spring-boot/) с веб-сайта Spring Boot. В этом кратком руководстве приведены пошаговые инструкции по развертыванию Spring Boot в виде приложения Service Fabric с использованием знакомых средств командной строки. Когда вы завершите работу с руководством, у вас будет пример Spring Boot, работающий на Service Fabric.

![Снимок экрана приложения](./media/service-fabric-quickstart-java-spring-boot/springbootsflocalhost.png)

Из этого краткого руководства вы узнаете, как выполнять следующие задачи:

* Развертывание приложения Spring Boot в Service Fabric
* Развертывание приложения в локальном кластере.
* Масштабирование приложения на несколько узлов
* Отработка отказа службы без ущерба для ее доступности

## <a name="prerequisites"></a>Предварительные требования

Для работы с этим кратким руководством сделайте следующее:

1. Установите пакет SDK и интерфейс командной строки Service Fabric:

    a. [Mac](https://docs.microsoft.com/azure/service-fabric/service-fabric-cli#cli-mac)
    
    b. [Linux](https://docs.microsoft.com/azure/service-fabric/service-fabric-get-started-linux#installation-methods)

1. [установите Git](https://git-scm.com/);
1. установка Yeoman;

    a. [Mac](https://docs.microsoft.com/azure/service-fabric/service-fabric-get-started-mac#create-your-application-on-your-mac-by-using-yeoman)

    b. [Linux](https://docs.microsoft.com/azure/service-fabric/service-fabric-get-started-linux#set-up-yeoman-generators-for-containers-and-guest-executables)
1. Настройте среду Java:

    a. [Mac](https://docs.microsoft.com/azure/service-fabric/service-fabric-get-started-mac#create-your-application-on-your-mac-by-using-yeoman)
    
    b.  [Linux](https://docs.microsoft.com/azure/service-fabric/service-fabric-get-started-linux#set-up-java-development)

## <a name="download-the-sample"></a>Скачивание примера приложения

В окне терминала выполните следующую команду, чтобы клонировать пример приложения Spring Boot Getting Started на локальный компьютер.

```bash
git clone https://github.com/spring-guides/gs-spring-boot.git
```

## <a name="build-the-spring-boot-application"></a>Создание приложения Spring Boot 
1. Чтобы создать приложение, перейдите в каталог `gs-spring-boot/complete` и выполните следующую команду: 

    ```bash
    ./gradlew build
    ``` 

## <a name="package-the-spring-boot-application"></a>Упаковка приложения Spring Boot 
1. В клонированном экземпляре каталога `gs-spring-boot` выполните команду `yo azuresfguest`. 

1. Введите следующие сведения для каждого запроса.

    ![Записи Yeoman](./media/service-fabric-quickstart-java-spring-boot/yeomanspringboot.png)

1. В папке `SpringServiceFabric/SpringServiceFabric/SpringGettingStartedPkg/code` создайте файл с именем `entryPoint.sh`. Добавьте в файл `entryPoint.sh` следующий код. 

    ```bash
    #!/bin/bash
    BASEDIR=$(dirname $0)
    cd $BASEDIR
    java -jar gs-spring-boot-0.1.0.jar
    ```

1. Добавьте ресурс **Endpoints** в файл *gs-spring-boot/SpringServiceFabric/SpringServiceFabric/SpringGettingStartedPkg/ServiceManifest.xml*.

    ```xml 
        <Resources>
          <Endpoints>
            <Endpoint Name="WebEndpoint" Protocol="http" Port="8080" />
          </Endpoints>
       </Resources>
    ```

    Файл **ServiceManifest.xml** теперь выглядит следующим образом: 

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <ServiceManifest Name="SpringGettingStartedPkg" Version="1.0.0"
                     xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsd="https://www.w3.org/2001/XMLSchema" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance" >

       <ServiceTypes>
          <StatelessServiceType ServiceTypeName="SpringGettingStartedType" UseImplicitHost="true">
       </StatelessServiceType>
       </ServiceTypes>

       <CodePackage Name="code" Version="1.0.0">
          <EntryPoint>
             <ExeHost>
                <Program>entryPoint.sh</Program>
                <Arguments></Arguments>
                <WorkingFolder>CodePackage</WorkingFolder>
             </ExeHost>
          </EntryPoint>
       </CodePackage>
        <Resources>
          <Endpoints>
            <Endpoint Name="WebEndpoint" Protocol="http" Port="8080" />
          </Endpoints>
       </Resources>
     </ServiceManifest>
    ```

Итак, на данный момент у вас есть приложение Service Fabric, созданное на основе примера "Начало работы" для Spring Boot, которое теперь можно развернуть в Service Fabric.

## <a name="run-the-application-locally"></a>Локальный запуск приложения

1. Запустите локальный кластер на компьютерах Ubuntu, выполнив следующую команду:

    ```bash
    sudo /opt/microsoft/sdk/servicefabric/common/clustersetup/devclustersetup.sh
    ```

    Если вы пользуетесь компьютером Mac, запустите локальный кластер из образа Docker (при условии, что выполнены [предварительные требования](https://docs.microsoft.com/azure/service-fabric/service-fabric-get-started-mac#create-a-local-container-and-set-up-service-fabric) по настройке локального кластера для Mac). 

    ```bash
    docker run --name sftestcluster -d -p 19080:19080 -p 19000:19000 -p 25100-25200:25100-25200 -p 8080:8080 mysfcluster
    ```

    Запуск локального кластера занимает некоторое время. Чтобы убедиться, что кластер является рабочим, откройте Service Fabric Explorer по адресу **http://localhost:19080** . Наличие пяти работоспособных узлов означает, что локальный кластер запущен и работает. 
    
    ![Работоспособное состояние локального кластера](./media/service-fabric-quickstart-java-spring-boot/sfxlocalhost.png)

1. Перейдите в папку `gs-spring-boot/SpringServiceFabric`.
1. Выполните следующую команду, чтобы подключиться к локальному кластеру.

    ```bash
    sfctl cluster select --endpoint http://localhost:19080
    ```
1. Выполните скрипт `install.sh`.

    ```bash
    ./install.sh
    ```

1. Откройте любой веб-браузер и перейдите к приложению по адресу `http://localhost:8080`.

    ![Локальная клиентская часть приложения](./media/service-fabric-quickstart-java-spring-boot/springbootsflocalhost.png)

Теперь вы можете получить доступ к приложению Spring Boot, которое развернуто в кластере Service Fabric.

## <a name="scale-applications-and-services-in-a-cluster"></a>Масштабирование приложений и служб в кластере

Службы могут легко масштабироваться в кластере с учетом изменения нагрузки на них. Масштабирование службы осуществляется путем изменения числа экземпляров, запущенных в кластере. Существует множество способов масштабирования служб. Например, можно использовать скрипты или команды интерфейса командной строки Service Fabric (sfctl). В следующих шагах используется Service Fabric Explorer.

Средство Service Fabric Explorer работает во всех кластерах Service Fabric. Чтобы его открыть, укажите адрес кластера и порт управления HTTP (19080) для кластера в адресной строке браузера, например `http://localhost:19080`.

Для масштабирования службы веб-интерфейса сделайте следующее:

1. Откройте Service Fabric Explorer в своем кластере (например, по ссылке `http://localhost:19080`).
1. Щелкните многоточие рядом с узлом **fabric:/SpringServiceFabric/SpringGettingStarted** в представлении в виде дерева и выберите **Масштабировать службу**.

    ![Масштабирование службы в Service Fabric Explorer](./media/service-fabric-quickstart-java-spring-boot/sfxscaleservicehowto.png)

    Теперь вы можете выбрать нужное количество экземпляров службы.

1. Укажите здесь число **3** и щелкните **Масштабировать службу**.

    Масштабирование службы можно настроить и с помощью командной строки, как описано ниже.

    ```bash
    # Connect to your local cluster
    sfctl cluster select --endpoint https://<ConnectionIPOrURL>:19080 --pem <path_to_certificate> --no-verify

    # Run Bash command to scale instance count for your service
    sfctl service update --service-id 'SpringServiceFabric~SpringGettingStarted' --instance-count 3 --stateless 
    ``` 

1. Щелкните узел **fabric:/SpringServiceFabric/SpringGettingStarted** в представлении в виде дерева и разверните узел раздела (здесь отображается его идентификатор GUID).

    ![Масштабирование службы в Service Fabric Explorer завершено](./media/service-fabric-quickstart-java-spring-boot/sfxscaledservice.png)

    У службы есть три экземпляра. Представление в виде дерева демонстрирует, на каких узлах они запущены.

С помощью этой простой задачи управления вы удвоили количество ресурсов для обработки пользовательской нагрузки для службы веб-интерфейса. Важно понимать, что для надежной работы службы не требуется запускать несколько экземпляров службы. При сбое в работе службы Service Fabric запускает новый экземпляр службы в кластере.

## <a name="fail-over-services-in-a-cluster"></a>Службы отработки отказа в кластере

Чтобы продемонстрировать отработку отказа службы, с помощью Service Fabric Explorer моделируется перезагрузка узла. Убедитесь, что запущен только один экземпляр службы.

1. Откройте Service Fabric Explorer в своем кластере (например, по ссылке `http://localhost:19080`).
1. Нажмите кнопку с многоточием рядом с узлом, на котором запущен экземпляр этой службы, и выберите команду перезапуска узла.

    ![Service Fabric Explorer с командой перезапуска узла](./media/service-fabric-quickstart-java-spring-boot/sfxhowtofailover.png)
1. Этот экземпляр службы перемещается в другой узел без простоев в работе приложения.

    ![Service Fabric Explorer после успешного перезапуска узла](./media/service-fabric-quickstart-java-spring-boot/sfxfailedover.png)

## <a name="next-steps"></a>Дополнительная информация

Из этого руководства вы узнали, как выполнить следующие действия:

* Развертывание приложения Spring Boot в Service Fabric
* Развертывание приложения в локальном кластере.
* Масштабирование приложения на несколько узлов
* Отработка отказа службы без ущерба для ее доступности

Дополнительные сведения о работе с приложениями Java в Service Fabric см. в руководстве по приложениям Java.

> [!div class="nextstepaction"]
> [Развертывание приложения Java](./service-fabric-tutorial-create-java-app.md)
