---
title: Руководство. Создание геореплицированного реестра Docker в Azure
description: Создайте реестр контейнеров Azure, настройте георепликацию, подготовьте образ Docker и разверните его в реестре. Первая часть руководства из трех частей.
services: container-registry
author: dlepow
manager: gwallace
ms.service: container-registry
ms.topic: tutorial
ms.date: 04/30/2017
ms.author: danlep
ms.custom: seodec18, mvc
ms.openlocfilehash: 87746bd39e624699612bf5221258ad757cd462b3
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/26/2019
ms.locfileid: "68309583"
---
# <a name="tutorial-prepare-a-geo-replicated-azure-container-registry"></a>Руководство. Подготовка геореплицированного реестра контейнеров Azure

Реестр контейнеров Azure — это частный реестр Docker, развернутый в Azure, который можно хранить недалеко от развертываний. Из этого набора из трех руководств вы узнаете, как с помощью георепликации развернуть веб-приложение ASP.NET Core, запущенное в контейнере Linux, на двух экземплярах [веб-приложений для контейнеров](../app-service/containers/index.yml). Вы узнаете, как автоматически развернуть образы для каждого экземпляра веб-приложения из ближайшего геореплицированного репозитория с помощью Azure.

В этой первой части руководства из трех частей вы узнаете, как:

> [!div class="checklist"]
> * создавать геореплицированный реестр контейнеров Azure;
> * клонировать исходный код приложения из GitHub;
> * создавать образ контейнера Docker из источника приложения;
> * отправлять образ контейнера в реестр.

В последующих руководствах вы развернете контейнер из частного реестра в веб-приложении, запущенном в двух регионах Azure. Затем вы обновите код в приложении, а также оба экземпляра веб-приложения с помощью одной команды `docker push` в реестре.

## <a name="before-you-begin"></a>Перед началом работы

Для работы с этим руководством нужна локальная установка Azure CLI версии 2.0.31 или более поздней. Чтобы узнать версию, выполните команду `az --version`. Если вам необходимо выполнить установку или обновление, см. статью [Установка Azure CLI 2.0]( /cli/azure/install-azure-cli).

Вам потребуется понимание базовых понятий Docker, таких как контейнеры, образы контейнеров и основные команды CLI Docker. [Руководство по началу работы с Docker]( https://docs.docker.com/get-started/) содержит базовые сведения о контейнерах.

Для работы с этим руководством потребуется локальная установка Docker. На сайте Docker предоставляются инструкции по установке для систем [macOS](https://docs.docker.com/docker-for-mac/), [Windows](https://docs.docker.com/docker-for-windows/) и [Linux](https://docs.docker.com/engine/installation/#supported-platforms).

Azure Cloud Shell не включает в себя компоненты Docker, необходимые для выполнения каждого шага этого руководства. Таким образом, мы рекомендуем выполнить локальную установку среды разработки Azure CLI и Docker.

## <a name="create-a-container-registry"></a>Создание реестра контейнеров

Войдите на [портале Azure](https://portal.azure.com).

Последовательно выберите **Создать ресурс** > **Контейнеры** > **Реестр контейнеров Azure**.

![Создание реестра контейнеров на портале Azure][tut-portal-01]

Настройте свой новый реестр, используя следующие параметры.

* **Имя реестра**. Создайте глобально уникальное имя реестра в Azure, которое содержит от 5 до 50 буквенно-цифровых символов.
* **Группа ресурсов**. **Создать** > `myResourceGroup`.
* **Расположение**`West US`.
* **Пользователь-администратор.** `Enable` (необходимо для веб-приложения для контейнеров, чтобы извлекать образы).
* **SKU.** `Premium` (необходимо для георепликации).

Выберите **Создать**, чтобы развернуть экземпляр записи контроля доступа (ACR).

![Создание реестра контейнеров на портале Azure][tut-portal-02]

В этом руководстве будет использоваться имя `<acrName>` как заполнитель выбранного **имени реестра контейнеров**.

> [!TIP]
> Так как обычно реестры контейнеров Azure — это долговременные ресурсы, которые используются на нескольких узлах контейнера, мы рекомендуем создать реестр в его собственной группе ресурсов. После настройки геореплицированных реестров и веб-перехватчиков эти дополнительные ресурсы помещаются в одну и ту же группу ресурсов.
>

## <a name="configure-geo-replication"></a>Настройка георепликации

Теперь, когда у вас есть реестр уровня "Премиум", можно настроить георепликацию. Веб-приложение, которое вы настроите в следующем руководстве для запуска в двух регионах, может извлекать образы контейнера из ближайшего реестра.

Перейдите в новый реестр контейнеров на портале Azure и в разделе **Службы** выберите **Replications** (Репликации).

![Репликации в реестре контейнера пользовательского интерфейса на портале Azure][tut-portal-03]

Отобразится карта, на которой показаны зеленые шестиугольники, отображающие регионы Azure, доступные для георепликации:

 ![Карта регионов на портале Azure][tut-map-01]

Реплицируйте свой реестр в регион "Восточная часть США". Для этого выберите его зеленый шестиугольник, а затем в разделе **Create replication** (Создать репликацию) нажмите кнопку **Создать**.

 ![Создание пользовательского интерфейса репликации на портале Azure][tut-portal-04]

По завершении репликации на портале отобразится состояние *Готово* для обоих регионов. С помощью кнопки **Обновить** можно обновить состояние репликации. Чтобы создать и синхронизировать реплики, может потребоваться несколько минут.

![Пользовательский интерфейс состояния репликации на портале Azure][tut-portal-05]

## <a name="container-registry-login"></a>Вход в реестр контейнеров

Теперь, после настройки георепликации, создайте образ контейнера и отправьте его в свой реестр. Сначала войдите в свой экземпляр ACR, прежде чем отправлять в него образы.

Выполните команду [az acr login](https://docs.microsoft.com/cli/azure/acr#az-acr-login), чтобы аутентифицировать и кэшировать учетные данные для вашего реестра. Замените `<acrName>` именем реестра, созданного ранее.

```azurecli
az acr login --name <acrName>
```

По завершении команда возвращает `Login Succeeded`.

## <a name="get-application-code"></a>Получение кода приложения

Пример в этом руководстве включает небольшое веб-приложение, созданное с помощью [ASP.NET Core][aspnet-core]. Приложение обслуживает страницу HTML, отображающую регион, из которого был развернут образ с помощью реестра контейнеров Azure.

![Приложение из руководства, отображающееся в браузере][tut-app-01]

Выполните указанную ниже команду git, чтобы загрузить пример в локальный каталог. Или выполните команду `cd`, чтобы загрузить пример в обычный каталог.

```bash
git clone https://github.com/Azure-Samples/acr-helloworld.git
cd acr-helloworld
```

Если у вас не установлен `git`, вы можете [скачать ZIP-архив][acr-helloworld-zip] непосредственно с сайта GitHub.

## <a name="update-dockerfile"></a>Обновление Dockerfile

Файл Dockerfile в примере репозитория демонстрирует, как создается контейнер. Он запускается из официального образа [aspnetcore][dockerhub-aspnetcore], копирует файлы приложения в контейнер, устанавливает зависимости, компилирует выходные данные с помощью официального образа [aspnetcore-build][dockerhub-aspnetcore-build] и, наконец, оптимизирует образ aspnetcore.

[Dockerfile][dockerfile] расположен в каталоге `./AcrHelloworld/Dockerfile` клонированного источника.

```Dockerfile
FROM microsoft/aspnetcore:2.0 AS base
# Update <acrName> with the name of your registry
# Example: uniqueregistryname.azurecr.io
ENV DOCKER_REGISTRY <acrName>.azurecr.io
WORKDIR /app
EXPOSE 80

FROM microsoft/aspnetcore-build:2.0 AS build
WORKDIR /src
COPY *.sln ./
COPY AcrHelloworld/AcrHelloworld.csproj AcrHelloworld/
RUN dotnet restore
COPY . .
WORKDIR /src/AcrHelloworld
RUN dotnet build -c Release -o /app

FROM build AS publish
RUN dotnet publish -c Release -o /app

FROM base AS production
WORKDIR /app
COPY --from=publish /app .
ENTRYPOINT ["dotnet", "AcrHelloworld.dll"]
```

Приложение в образе *acr-helloworld* пытается определить регион, из которого был развернут его контейнер, запросив информацию о сервере входа в реестр у DNS. Необходимо указать полное доменное имя сервера входа в реестр для переменной среды `DOCKER_REGISTRY` в Dockerfile.

Сначала получите URL-адрес сервера входа в реестр, выполнив команду `az acr show`. Замените `<acrName>` именем реестра, созданного на предыдущих шагах.

```azurecli
az acr show --name <acrName> --query "{acrLoginServer:loginServer}" --output table
```

Выходные данные:

```bash
AcrLoginServer
-----------------------------
uniqueregistryname.azurecr.io
```

Затем замените строку `ENV DOCKER_REGISTRY` полным доменным именем сервера входа в реестр. В этом примере используется имя реестра *uniqueregistryname*:

```Dockerfile
ENV DOCKER_REGISTRY uniqueregistryname.azurecr.io
```

## <a name="build-container-image"></a>Создание образа контейнера

Теперь, когда вы включили в файл Dockerfile полное доменное имя сервера входа в реестр, можно создать образ контейнера с помощью команды `docker build`. Выполните указанную ниже команду, чтобы создать образ и пометить его URL-адресом частного реестра. Снова замените `<acrName>` именем своего реестра.

```bash
docker build . -f ./AcrHelloworld/Dockerfile -t <acrName>.azurecr.io/acr-helloworld:v1
```

Несколько строк выходных данных отображаются во время создания образа Docker (ниже показан усеченный пример).

```bash
Sending build context to Docker daemon  523.8kB
Step 1/18 : FROM microsoft/aspnetcore:2.0 AS base
2.0: Pulling from microsoft/aspnetcore
3e17c6eae66c: Pulling fs layer

[...]

Step 18/18 : ENTRYPOINT dotnet AcrHelloworld.dll
 ---> Running in 6906d98c47a1
 ---> c9ca1763cfb1
Removing intermediate container 6906d98c47a1
Successfully built c9ca1763cfb1
Successfully tagged uniqueregistryname.azurecr.io/acr-helloworld:v1
```

Используйте `docker images`, чтобы просмотреть готовый образ с тегами:

```console
$ docker images
REPOSITORY                                      TAG    IMAGE ID        CREATED               SIZE
uniqueregistryname.azurecr.io/acr-helloworld    v1     01ac48d5c8cf    About a minute ago    284MB
[...]
```

## <a name="push-image-to-azure-container-registry"></a>Передача образа в реестр контейнеров Azure

Теперь выполните команду `docker push`, чтобы отправить в реестр образ *acr-helloworld*. Замените `<acrName>` именем реестра.

```bash
docker push <acrName>.azurecr.io/acr-helloworld:v1
```

Так как реестр настроен для георепликации, ваш образ будет автоматически реплицирован в регион *западной части США* и *восточной части США*. Для этого необходимо выполнить отдельную команду `docker push`.

```console
$ docker push uniqueregistryname.azurecr.io/acr-helloworld:v1
The push refers to a repository [uniqueregistryname.azurecr.io/acr-helloworld]
cd54739c444b: Pushed
d6803756744a: Pushed
b7b1f3a15779: Pushed
a89567dff12d: Pushed
59c7b561ff56: Pushed
9a2f9413d9e4: Pushed
a75caa09eb1f: Pushed
v1: digest: sha256:0799014f91384bda5b87591170b1242bcd719f07a03d1f9a1ddbae72b3543970 size: 1792
```

## <a name="next-steps"></a>Дополнительная информация

В этом руководстве вы создали частный, геореплицированный реестр контейнеров, образ контейнера, а затем отправили образ в реестр.

Перейдите к следующему руководству, из которого вы узнаете, как развернуть созданный контейнер в нескольких экземплярах службы "Веб-приложение для контейнеров" с использованием георепликации для локального обслуживания образов.

> [!div class="nextstepaction"]
> [Развертывание веб-приложения из реестра контейнеров Azure](container-registry-tutorial-deploy-app.md)

<!-- IMAGES -->
[tut-portal-01]: ./media/container-registry-tutorial-prepare-registry/tut-portal-01.png
[tut-portal-02]: ./media/container-registry-tutorial-prepare-registry/tut-portal-02.png
[tut-portal-03]: ./media/container-registry-tutorial-prepare-registry/tut-portal-03.png
[tut-portal-04]: ./media/container-registry-tutorial-prepare-registry/tut-portal-04.png
[tut-portal-05]: ./media/container-registry-tutorial-prepare-registry/tut-portal-05.png
[tut-app-01]: ./media/container-registry-tutorial-prepare-registry/tut-app-01.png
[tut-map-01]: ./media/container-registry-tutorial-prepare-registry/tut-map-01.png

<!-- LINKS - External -->
[acr-helloworld-zip]: https://github.com/Azure-Samples/acr-helloworld/archive/master.zip
[aspnet-core]: https://dot.net
[dockerhub-aspnetcore]: https://hub.docker.com/r/microsoft/aspnetcore/
[dockerhub-aspnetcore-build]: https://store.docker.com/community/images/microsoft/aspnetcore-build
[dockerfile]: https://github.com/Azure-Samples/acr-helloworld/blob/master/AcrHelloworld/Dockerfile