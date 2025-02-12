---
title: Развертывание приложения Service Fabric в кластер в Azure | Документы Майкрософт
description: Сведения о развертывании приложения в кластере из Visual Studio.
services: service-fabric
documentationcenter: .net
author: athinanthny
manager: msfussell
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: tutorial
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 07/22/2019
ms.author: mikhegn
ms.custom: mvc
ms.openlocfilehash: d637b01eb9e4d1664def9eb537b810603eea18ea
ms.sourcegitcommit: fe6b91c5f287078e4b4c7356e0fa597e78361abe
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/29/2019
ms.locfileid: "68598810"
---
# <a name="tutorial-deploy-a-service-fabric-application-to-a-cluster-in-azure"></a>Руководство по Развертывание приложения Service Fabric в кластер в Azure

Это руководство представляет собой вторую часть цикла. Здесь показано, как развернуть приложение Azure Service Fabric в новый кластер в Azure.

Из этого руководства вы узнаете, как выполнять следующие задачи:
> [!div class="checklist"]
> * Создание кластера.
> * Развертывание приложения на удаленный кластер с помощью Visual Studio.

Из этой серии руководств вы узнаете, как выполнить следующие задачи:
> [!div class="checklist"]
> * [Создание приложения .NET Service Fabric](service-fabric-tutorial-create-dotnet-app.md).
> * Развертывание приложения в удаленном кластере.
> * [Добавление конечной точки HTTPS к интерфейсной службе ASP.NET Core](service-fabric-tutorial-dotnet-app-enable-https-endpoint.md).
> * [Настройка CI/CD с помощью Azure Pipelines](service-fabric-tutorial-deploy-app-with-cicd-vsts.md).
> * [Настройка мониторинга и диагностики приложения](service-fabric-tutorial-monitoring-aspnet.md).

## <a name="prerequisites"></a>Предварительные требования

Перед началом работы с этим руководством выполните следующие действия:

* Если у вас еще нет подписки Azure, создайте [бесплатную учетную запись Azure](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
* [Установите Visual Studio 2019](https://www.visualstudio.com/), а также рабочие нагрузки **Разработка для Azure** и **ASP.NET и разработка веб-приложений**.
* [Установите пакет SDK для Service Fabric](service-fabric-get-started.md).

## <a name="download-the-voting-sample-application"></a>Скачивание примера приложения для голосования

Если вы не создавали пример приложения для голосования [в первой части этой серии руководств](service-fabric-tutorial-create-dotnet-app.md), вы можете скачать его. В командном окне выполните следующий код, чтобы клонировать репозиторий с примером приложения на локальный компьютер.

```git
git clone https://github.com/Azure-Samples/service-fabric-dotnet-quickstart 
```

Откройте приложение в Visual Studio с правами администратора и выполните сборку приложения.

## <a name="create-a-cluster"></a>Создание кластера

Теперь, когда приложение готово, создайте кластер Service Fabric, а затем разверните в него приложение. [Кластер Service Fabric](https://docs.microsoft.com/azure/service-fabric/service-fabric-deploy-anywhere) — это подключенный к сети набор виртуальных машин или физических компьютеров, в котором вы развертываете микрослужбы и управляете ими.

В этом руководстве вы создадите новый тестовый кластер из трех узлов в интегрированной среде разработки Visual Studio, а затем опубликуете приложение в этом кластере. Дополнительные сведения о создании производственного кластера см. в статье [Руководство. Развертывание кластера Service Fabric на платформе Windows в виртуальной сети Azure](service-fabric-tutorial-create-vnet-and-windows-cluster.md). Можно также развернуть приложение в существующий кластер (созданный ранее с помощью [портала Azure](https://portal.azure.com)) с использованием [PowerShell](./scripts/service-fabric-powershell-create-secure-cluster-cert.md) или скриптов [Azure CLI](./scripts/cli-create-cluster.md) или из [Шаблона Azure Resource Manager](service-fabric-tutorial-create-vnet-and-windows-cluster.md).

> [!NOTE]
> Для связи между службами приложение для голосования и многие другие приложения используют обратный прокси-сервер Service Fabric. У кластеров, созданных из Visual Studio, есть обратный прокси-сервер, включенный по умолчанию. Для работы приложения для голосования при развертывании в существующий кластер вы должны [включить обратный прокси-сервер в кластере](service-fabric-reverseproxy-setup.md).


### <a name="find-the-votingweb-service-endpoint"></a>Поиск конечной точки службы VotingWeb

Интерфейсная веб-служба приложения для голосования прослушивает определенный порт (8080, если вы выполнили действия, описанные в [первой части этого руководства](service-fabric-tutorial-create-dotnet-app.md). Если приложение развертывается в кластере в Azure, то приложение и кластер запускаются под контролем Azure Load Balancer. Порт приложения должен открываться в подсистеме балансировки нагрузки Azure для кластера с помощью правила. Правило отправляет входящий трафик подсистемы балансировки нагрузки в веб-службу. Порт указан в файле **VotingWeb/PackageRoot/ServiceManifest.xml** в элементе **Endpoint**. 

```xml
<Endpoint Protocol="http" Name="ServiceEndpoint" Type="Input" Port="8080" />
```

Запишите конечную точку службы, которая необходима на следующем этапе.  Если вы развертываете в существующий кластер, откройте этот порт, создав правило и пробу в подсистеме балансировки нагрузки Azure с помощью [скрипта PowerShell](./scripts/service-fabric-powershell-open-port-in-load-balancer.md) или подсистеме балансировки нагрузки для этого кластера на [портале Azure](https://portal.azure.com).

### <a name="create-a-test-cluster-in-azure"></a>Создание тестового кластера в Azure
Щелкните правой кнопкой мыши проект в **приложении для голосования** и выберите пункт **Опубликовать**.

В **Конечной точке подключения** выберите **Создать новый кластер**.  Если вы развертываете в существующий кластер, выберите конечную точку кластера из списка.  Откроется диалоговое окно "Создание кластера Service Fabric".

На вкладке **Кластер** введите **имя кластера** (например, "mytestcluster"), выберите свою подписку, регион для кластера (например, центрально-южная часть США), введите количество узлов кластера (для тестового кластера мы рекомендуем три узла) и введите группу ресурсов (например, "mytestclustergroup"). Щелкните **Далее**.

![Создание кластера](./media/service-fabric-tutorial-deploy-app-to-party-cluster/create-cluster.png)

Введите пароль и путь вывода для сертификата кластера на вкладке **Сертификат**. Самозаверяющий сертификат создается в виде PFX-файла и сохраняется по указанному пути вывода.  Он используется для обеспечения безопасности как между узлами, так и между клиентами.  Не используйте самозаверяющий сертификат для кластеров в рабочей среде.  Этот сертификат используется в Visual Studio для проверки подлинности кластера и развертывания приложений. Выберите **Импорт сертификата**, чтобы установить PFX в хранилище сертификатов CurrentUser\My вашего компьютера.  Щелкните **Далее**.

![Создание кластера](./media/service-fabric-tutorial-deploy-app-to-party-cluster/certificate.png)

На вкладке **Сведения о виртуальной машине** введите **Имя пользователя** и **Пароль** для учетной записи администратора кластера.  Выберите **Образ виртуальной машины** для узлов кластера и **Размер виртуальной машины** для каждого узла кластера.  Выберите вкладку **Advanced** (Дополнительно).

![Создание кластера](./media/service-fabric-tutorial-deploy-app-to-party-cluster/vm-detail.png)

В разделе **Порты** введите конечную точку службы VotingWeb из предыдущего шага (например, 8080).  При создании кластера эти порты приложения открываются в подсистеме балансировки нагрузки Azure для переадресации трафика в кластер.  Чтобы создать кластер, щелкните **Создать**. Это займет всего несколько минут.

![Создание кластера](./media/service-fabric-tutorial-deploy-app-to-party-cluster/advanced.png)

## <a name="publish-the-application-to-the-cluster"></a>Публикация приложения в кластере

Теперь, когда новый кластер готов, вы можете развернуть приложение для голосования напрямую из Visual Studio.

Щелкните правой кнопкой мыши проект в **приложении для голосования** и выберите пункт **Опубликовать**. Откроется диалоговое окно **Publish** (Опубликовать).

В поле **Конечная точка подключения** выберите конечную точку для кластера, созданного на предыдущем шаге.  Например "mytestcluster.southcentral.cloudapp.azure.com:19000". Если вы выберете **Расширенные параметры подключения**, информация о сертификате должна быть заполнена автоматически.  
![Публикация приложения Service Fabric](./media/service-fabric-tutorial-deploy-app-to-party-cluster/publish-app.png)

Нажмите кнопку **Опубликовать**.

После развертывания приложения откройте браузер и введите адрес кластера с последующим **:8080**. Или укажите другой порт, если он настроен. Например, `http://mytestcluster.southcentral.cloudapp.azure.com:8080`. Вы увидите приложения, выполняющиеся в кластере Azure. На веб-странице для голосования попробуйте добавить и удалить параметры голосования, а также проголосовать за один или несколько из этих параметров.

![Пример для голосования Service Fabric](./media/service-fabric-tutorial-deploy-app-to-party-cluster/application-screenshot-new-azure.png)


## <a name="next-steps"></a>Дополнительная информация
В этой части руководства вы узнали, как выполнить следующие действия:

> [!div class="checklist"]
> * Создание кластера.
> * Развертывание приложения на удаленный кластер с помощью Visual Studio.

Перейдите к следующему руководству:
> [!div class="nextstepaction"]
> [Включение HTTPS](service-fabric-tutorial-dotnet-app-enable-https-endpoint.md)
