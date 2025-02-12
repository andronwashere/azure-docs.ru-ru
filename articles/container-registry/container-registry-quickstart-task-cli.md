---
title: Краткое руководство. Сборка и запуск образа контейнера в Реестре контейнеров Azure
description: Быстрый запуск задач с помощью Реестра контейнеров Azure для создания и запуска в облаке образов контейнеров по требованию.
services: container-registry
author: dlepow
manager: gwallace
ms.service: container-registry
ms.topic: quickstart
ms.date: 04/02/2019
ms.author: danlep
ms.custom: ''
ms.openlocfilehash: e5e02d8194f9164a03bb27d932df45d91486c518
ms.sourcegitcommit: f5075cffb60128360a9e2e0a538a29652b409af9
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/18/2019
ms.locfileid: "68310632"
---
# <a name="quickstart-build-and-run-a-container-image-using-azure-container-registry-tasks"></a>Краткое руководство. Сборка и запуск образа контейнера с помощью Задач Реестра контейнеров Azure

В этом кратком руководстве показано, как с помощью команд Задач Реестра контейнеров Azure можно быстро создавать, отправлять и запускать образ контейнера Docker в Azure, а также продемонстрирована выгрузка "внутреннего цикла" разработки в облако. [Задачи Реестра контейнеров Azure][container-registry-tasks-overview] — это набор функций в Реестре контейнеров Azure, которые призваны упростить управление образами контейнеров и их изменение в течение всего жизненного цикла контейнеров. 

После знакомства с этим кратким руководством ознакомьтесь с более сложными функциями Задач Реестра контейнеров Azure. Задачи Реестра контейнеров Azure позволяют автоматизировать создание образов на основе фиксированного кода или обновления базовых образов или проверять несколько контейнеров параллельно, а также использовать другие сценарии. 

Если у вас еще нет подписки Azure, [создайте бесплатную учетную запись Azure][azure-account], прежде чем начинать работу.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Для выполнения инструкций этого краткого руководства можно использовать Azure Cloud Shell или локальный экземпляр Azure CLI. Если вы хотите использовать его локально, рекомендуется использовать версию 2.0.58 или более позднюю. Чтобы узнать версию, выполните команду `az --version`. Если вам необходимо выполнить установку или обновление, см. статью [Установка Azure CLI 2.0][azure-cli-install].

## <a name="create-a-resource-group"></a>Создание группы ресурсов

Если у вас еще нет реестра контейнеров, сначала создайте группу ресурсов с помощью команды [az group create][az-group-create]. Группа ресурсов Azure является логическим контейнером, в котором происходит развертывание ресурсов Azure и управление ими.

В следующем примере создается группа ресурсов с именем *myResourceGroup* в расположении *eastus*.

```azurecli-interactive
az group create --name myResourceGroup --location eastus
```

## <a name="create-a-container-registry"></a>Создание реестра контейнеров

Создайте реестр контейнеров с помощью команды [az acr create][az-acr-create]. Имя реестра должно быть уникальным в пределах Azure и содержать от 5 до 50 буквенно-цифровых символов. В примере ниже используется имя *myContainerRegistry008*. Замените его уникальным значением.

```azurecli-interactive
az acr create --resource-group myResourceGroup --name myContainerRegistry008 --sku Basic
```

В этом примере создается реестр ценовой категории *Базовый*; это экономный вариант для разработчиков, изучающих Реестр контейнеров Azure. Дополнительные сведения об уровнях служб см. в статье [Номера SKU реестра контейнеров][container-registry-skus].

## <a name="build-an-image-from-a-dockerfile"></a>Создание образа с помощью Dockerfile

Теперь используйте Реестр контейнеров Azure для создания образа. Во-первых, создайте рабочий каталог, а затем создайте Dockerfile с именем *Dockerfile* и следующим содержимым. Это простой пример создания образа контейнера Linux, но вы можете создавать собственные стандартные Dockerfile и создавать образы для других платформ.

```bash
echo FROM hello-world > Dockerfile
```

Выполните команду [az acr build][az-acr-build], чтобы создать образ. После успешной сборки образ помещается в реестр. В примере ниже показан перенос образа `sample/hello-world:v1`. Символ `.` в конце команды задает расположение Dockerfile, в данном случае это текущий каталог.

```azurecli-interactive
az acr build --image sample/hello-world:v1 --registry myContainerRegistry008 --file Dockerfile . 
```

Результат успешной сборки и перенос образа может иметь следующий вид:

```console
Packing source code into tar to upload...
Uploading archived source code from '/tmp/build_archive_b0bc1e5d361b44f0833xxxx41b78c24e.tar.gz'...
Sending context (1.856 KiB) to registry: mycontainerregistry008...
Queued a build with ID: ca8
Waiting for agent...
2019/03/18 21:56:57 Using acb_vol_4c7ffa31-c862-4be3-xxxx-ab8e615c55c4 as the home volume
2019/03/18 21:56:57 Setting up Docker configuration...
2019/03/18 21:56:58 Successfully set up Docker configuration
2019/03/18 21:56:58 Logging in to registry: mycontainerregistry008.azurecr.io
2019/03/18 21:56:59 Successfully logged into mycontainerregistry008.azurecr.io
2019/03/18 21:56:59 Executing step ID: build. Working directory: '', Network: ''
2019/03/18 21:56:59 Obtaining source code and scanning for dependencies...
2019/03/18 21:57:00 Successfully obtained source code and scanned for dependencies
2019/03/18 21:57:00 Launching container with name: build
Sending build context to Docker daemon  13.82kB
Step 1/1 : FROM hello-world
latest: Pulling from library/hello-world
Digest: sha256:2557e3c07ed1e38f26e389462d03ed943586fxxxx21577a99efb77324b0fe535
Successfully built fce289e99eb9
Successfully tagged mycontainerregistry008.azurecr.io/sample/hello-world:v1
2019/03/18 21:57:01 Successfully executed container: build
2019/03/18 21:57:01 Executing step ID: push. Working directory: '', Network: ''
2019/03/18 21:57:01 Pushing image: mycontainerregistry008.azurecr.io/sample/hello-world:v1, attempt 1
The push refers to repository [mycontainerregistry008.azurecr.io/sample/hello-world]
af0b15c8625b: Preparing
af0b15c8625b: Layer already exists
v1: digest: sha256:92c7f9c92844bbbb5d0a101b22f7c2a7949e40f8ea90c8b3bc396879d95e899a size: 524
2019/03/18 21:57:03 Successfully pushed image: mycontainerregistry008.azurecr.io/sample/hello-world:v1
2019/03/18 21:57:03 Step ID: build marked as successful (elapsed time in seconds: 2.543040)
2019/03/18 21:57:03 Populating digests for step ID: build...
2019/03/18 21:57:05 Successfully populated digests for step ID: build
2019/03/18 21:57:05 Step ID: push marked as successful (elapsed time in seconds: 1.473581)
2019/03/18 21:57:05 The following dependencies were found:
2019/03/18 21:57:05
- image:
    registry: mycontainerregistry008.azurecr.io
    repository: sample/hello-world
    tag: v1
    digest: sha256:92c7f9c92844bbbb5d0a101b22f7c2a7949e40f8ea90c8b3bc396879d95e899a
  runtime-dependency:
    registry: registry.hub.docker.com
    repository: library/hello-world
    tag: v1
    digest: sha256:2557e3c07ed1e38f26e389462d03ed943586f744621577a99efb77324b0fe535
  git: {}

Run ID: ca8 was successful after 10s
```

## <a name="run-the-image"></a>Запуск образа

Теперь быстро запустите созданный и переданный в реестр образ. В рабочем процессе разработки контейнера можно реализовать этап проверки перед развертыванием образа.

Создайте в локальном рабочем каталоге файл *quickrun.yaml* со следующим содержимым для отдельного этапа. Замените имя сервера входа вашего реестра на *\<acrLoginServer\>* . Имя сервера входа имеет формат *\<имя_реестра\>.azurecr.io* (в нижнем регистре), например *mycontainerregistry008.azurecr.io*. В этом примере предполагается, что в предыдущем разделе вы создали и отправили в реестр образ `sample/hello-world:v1`:

```yml
steps:
  - cmd: <acrLoginServer>/sample/hello-world:v1
```

Этап `cmd` в этом примере охватывает запуск контейнера в конфигурации по умолчанию, но `cmd` поддерживает дополнительные параметры `docker run` или другие команды `docker`.

Запустите контейнер с помощью следующей команды:

```azurecli-interactive
az acr run --registry myContainerRegistry008 --file quickrun.yaml .
```

Результат аналогичен приведенному ниже:

```console
Packing source code into tar to upload...
Uploading archived source code from '/tmp/run_archive_ebf74da7fcb04683867b129e2ccad5e1.tar.gz'...
Sending context (1.855 KiB) to registry: mycontainerre...
Queued a run with ID: cab
Waiting for an agent...
2019/03/19 19:01:53 Using acb_vol_60e9a538-b466-475f-9565-80c5b93eaa15 as the home volume
2019/03/19 19:01:53 Creating Docker network: acb_default_network, driver: 'bridge'
2019/03/19 19:01:53 Successfully set up Docker network: acb_default_network
2019/03/19 19:01:53 Setting up Docker configuration...
2019/03/19 19:01:54 Successfully set up Docker configuration
2019/03/19 19:01:54 Logging in to registry: mycontainerregistry008.azurecr.io
2019/03/19 19:01:55 Successfully logged into mycontainerregistry008.azurecr.io
2019/03/19 19:01:55 Executing step ID: acb_step_0. Working directory: '', Network: 'acb_default_network'
2019/03/19 19:01:55 Launching container with name: acb_step_0

Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (amd64)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker ID:
 https://hub.docker.com/

For more examples and ideas, visit:
 https://docs.docker.com/get-started/

2019/03/19 19:01:56 Successfully executed container: acb_step_0
2019/03/19 19:01:56 Step ID: acb_step_0 marked as successful (elapsed time in seconds: 0.843801)

Run ID: cab was successful after 6s
```

## <a name="clean-up-resources"></a>Очистка ресурсов

Ненужные группу ресурсов, реестр контейнеров и все образы контейнеров можно удалить с помощью команды [az group delete][az-group-delete].

```azurecli
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Дополнительная информация

В этом кратком руководстве вы использовали функции Задач Реестра контейнеров Azure, чтобы быстро создавать, передавать и запускать образы контейнеров Docker непосредственно в Azure. Чтобы узнать больше об использовании Задач Реестра контейнеров Azure для автоматизации создания и обновления образов, ознакомьтесь с руководствами по Задачам Реестра контейнеров Azure.

> [!div class="nextstepaction"]
> [Руководства по использованию Реестра контейнеров Azure][container-registry-tutorial-quick-task]

<!-- LINKS - external -->
[docker-linux]: https://docs.docker.com/engine/installation/#supported-platforms
[docker-mac]: https://docs.docker.com/docker-for-mac/
[docker-push]: https://docs.docker.com/engine/reference/commandline/push/
[docker-pull]: https://docs.docker.com/engine/reference/commandline/pull/
[docker-rmi]: https://docs.docker.com/engine/reference/commandline/rmi/
[docker-run]: https://docs.docker.com/engine/reference/commandline/run/
[docker-tag]: https://docs.docker.com/engine/reference/commandline/tag/
[docker-windows]: https://docs.docker.com/docker-for-windows/
[azure-account]: https://azure.microsoft.com/free/

<!-- LINKS - internal -->
[az-acr-create]: /cli/azure/acr#az-acr-create
[az-acr-build]: /cli/azure/acr#az-acr-build
[az-group-create]: /cli/azure/group#az-group-create
[az-group-delete]: /cli/azure/group#az-group-delete
[azure-cli]: /cli/azure/install-azure-cli
[container-registry-tasks-overview]: container-registry-tasks-overview.md
[container-registry-tutorial-quick-task]: container-registry-tutorial-quick-task.md
[container-registry-skus]: container-registry-skus.md
[azure-cli-install]: /cli/azure/install-azure-cli
