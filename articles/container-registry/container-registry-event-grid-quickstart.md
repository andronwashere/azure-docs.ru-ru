---
title: Краткое руководство. Отправка событий Реестра контейнеров Azure в Сетку событий
description: В этом кратком руководстве описано, как включить события Сетки событий для реестра контейнеров, передать образ контейнера, отправить события в пример приложения и удалить их из него.
services: container-registry
author: dlepow
manager: gwallace
ms.service: container-registry
ms.topic: article
ms.date: 08/23/2018
ms.author: danlep
ms.custom: seodec18
ms.openlocfilehash: 49ee9a7f12601b0d93e320ab797be4a1ada41c04
ms.sourcegitcommit: f5075cffb60128360a9e2e0a538a29652b409af9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/18/2019
ms.locfileid: "68309803"
---
# <a name="quickstart-send-events-from-private-container-registry-to-event-grid"></a>Краткое руководство. Отправка событий из частного реестра контейнеров в службу "Сетка событий"

Сетка событий Azure — это полностью управляемая служба маршрутизации событий, которая обеспечивает равномерное потребление событий с помощью модели "публикация — подписка". В этом кратком руководстве вы создадите реестр контейнеров, подпишитесь на события реестра, а затем развернете пример веб-приложения для получения событий, используя Azure CLI. Наконец, вы запускаете образ контейнера `push` и `delete` и просматриваете полезную нагрузку события в примере приложения.

Когда вы выполните шаги в этой статье, события, отправленные из вашего реестра контейнеров в Сетку событий, будут отображаться в примере веб-приложения:

![Веб-браузер, где отображается пример веб-приложения с тремя полученными событиями][sample-app-01]

Если у вас еще нет подписки Azure, [создайте бесплатную учетную запись Azure][azure-account], прежде чем начинать работу.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Команды Azure CLI в этой статье отформатированы для выполнения в оболочке **Bash**. Если вы используете другую оболочку, такую ​​как PowerShell или командную строку, вам может потребоваться соответственно откорректировать символы продолжения строки или строки назначения переменных. В этой статье используются переменные, чтобы минимизировать требуемое изменение команд.

## <a name="create-a-resource-group"></a>Создание группы ресурсов

Группа ресурсов Azure — это логический контейнер, в котором происходит развертывание ресурсов Azure и управление ими. Следующая команда [AZ Group Create][az-group-create] создает группу ресурсов с именем *myResourceGroup* в регионе *eastus* . Если вы хотите использовать другое имя для своей группы ресурсов, установите другое значение для `RESOURCE_GROUP_NAME`.

```azurecli-interactive
RESOURCE_GROUP_NAME=myResourceGroup

az group create --name $RESOURCE_GROUP_NAME --location eastus
```

## <a name="create-a-container-registry"></a>Создание реестра контейнеров

Затем разверните реестр контейнеров в группе ресурсов с помощью следующих команд. Перед выполнением команды [AZ запись контроля][az-acr-create] доступа укажите `ACR_NAME` имя реестра. Имя реестра должно быть уникальным в пределах Azure и содержать от 5 до 50 буквенно-цифровых символов.

```azurecli-interactive
ACR_NAME=<acrName>

az acr create --resource-group $RESOURCE_GROUP_NAME --name $ACR_NAME --sku Basic
```

После создания реестра Azure CLI выводит данные, подобные следующим.

```json
{
  "adminUserEnabled": false,
  "creationDate": "2018-08-16T20:02:46.569509+00:00",
  "id": "/subscriptions/<Subscription ID>/resourceGroups/myResourceGroup/providers/Microsoft.ContainerRegistry/registries/myregistry",
  "location": "eastus",
  "loginServer": "myregistry.azurecr.io",
  "name": "myregistry",
  "provisioningState": "Succeeded",
  "resourceGroup": "myResourceGroup",
  "sku": {
    "name": "Basic",
    "tier": "Basic"
  },
  "status": null,
  "storageAccount": null,
  "tags": {},
  "type": "Microsoft.ContainerRegistry/registries"
}

```

## <a name="create-an-event-endpoint"></a>Создание конечной точки событий

В этом разделе вы используете шаблон Resource Manager, расположенный в репозитории GitHub, для развертывания готового примера веб-приложения в Службе приложений Azure. Позже вы подпишитесь на события Сетки событий вашего реестра и укажите это приложение как конечную точку, в которую отправляются события.

Чтобы развернуть пример приложения, задайте для `SITE_NAME` уникальное имя своего веб-приложения и выполните следующие команды. Имя сайта должно быть уникальным в пределах Azure, так как оно является частью полного доменного имени (FQDN) веб-приложения. В следующем разделе вы перейдете к полному доменному имени приложения в веб-браузере, чтобы просмотреть события реестра.

```azurecli-interactive
SITE_NAME=<your-site-name>

az group deployment create \
    --resource-group $RESOURCE_GROUP_NAME \
    --template-uri "https://raw.githubusercontent.com/Azure-Samples/azure-event-grid-viewer/master/azuredeploy.json" \
    --parameters siteName=$SITE_NAME hostingPlanName=$SITE_NAME-plan
```

Как только развертывание завершится (это может занять несколько минут), откройте браузер и перейдите в свое веб-приложение, чтобы убедиться, что оно запущено:

`http://<your-site-name>.azurewebsites.net`

Вы должны увидеть пример приложения, в котором не отображаются сообщения о событиях:

![Веб-браузер с примером веб-приложения, в котором не отображаются события][sample-app-02]

[!INCLUDE [event-grid-register-provider-cli.md](../../includes/event-grid-register-provider-cli.md)]

## <a name="subscribe-to-registry-events"></a>Подписка на события реестра

Подпишитесь на *раздел*, чтобы определить в Сетке событий Azure, какие события необходимо отслеживать и куда их отправлять. Приведенная ниже команда [AZ eventgrid Event-Subscription создает][az-eventgrid-event-subscription-create] подписку на созданный реестр контейнеров и указывает URL-адрес приложения в качестве конечной точки, в которую он должен отсылать события. Переменные среды, которые вы заполнили в предыдущих разделах, используются здесь повторно, поэтому никаких изменений не требуется.

```azurecli-interactive
ACR_REGISTRY_ID=$(az acr show --name $ACR_NAME --query id --output tsv)
APP_ENDPOINT=https://$SITE_NAME.azurewebsites.net/api/updates

az eventgrid event-subscription create \
    --name event-sub-acr \
    --source-resource-id $ACR_REGISTRY_ID \
    --endpoint $APP_ENDPOINT
```

После создания подписки вы увидите выходные данные, аналогичные следующим:

```JSON
{
  "destination": {
    "endpointBaseUrl": "https://eventgridviewer.azurewebsites.net/api/updates",
    "endpointType": "WebHook",
    "endpointUrl": null
  },
  "filter": {
    "includedEventTypes": [
      "All"
    ],
    "isSubjectCaseSensitive": null,
    "subjectBeginsWith": "",
    "subjectEndsWith": ""
  },
  "id": "/subscriptions/<Subscription ID>/resourceGroups/myResourceGroup/providers/Microsoft.ContainerRegistry/registries/myregistry/providers/Microsoft.EventGrid/eventSubscriptions/event-sub-acr",
  "labels": null,
  "name": "event-sub-acr",
  "provisioningState": "Succeeded",
  "resourceGroup": "myResourceGroup",
  "topic": "/subscriptions/<Subscription ID>/resourceGroups/myresourcegroup/providers/microsoft.containerregistry/registries/myregistry",
  "type": "Microsoft.EventGrid/eventSubscriptions"
}
```

## <a name="trigger-registry-events"></a>Активация событий реестра

Теперь, когда пример приложения запущен и вы подписались на свой реестр с помощью Сетки событий, вы можете создать события. В этом разделе вы используете задачи ACR для создания и перемещения образа контейнера в реестр. Задачи ACR — это функция Реестра контейнеров Azure, которая позволяет создавать образы контейнеров в облаке без необходимости установки подсистемы Docker на вашем локальном компьютере.

### <a name="build-and-push-image"></a>Создание и отправка образа

Выполните следующую команду Azure CLI, чтобы создать образ контейнера из содержимого репозитория GitHub. По умолчанию задачи ACR автоматически передает успешно созданный образ в ваш реестр, который генерирует событие `ImagePushed`.

```azurecli-interactive
az acr build --registry $ACR_NAME --image myimage:v1 -f Dockerfile https://github.com/Azure-Samples/acr-build-helloworld-node.git
```

Вы должны увидеть выходные данные, похожие на следующие, когда задачи ACR будут создавать, а затем передавать ваш образ. Следующий пример выходных данных усечен для краткости.

```console
$ az acr build -r $ACR_NAME --image myimage:v1 -f Dockerfile https://github.com/Azure-Samples/acr-build-helloworld-node.git
Sending build context to ACR...
Queued a build with build ID: aa2
Waiting for build agent...
2018/08/16 22:19:38 Using acb_vol_27a2afa6-27dc-4ae4-9e52-6d6c8b7455b2 as the home volume
2018/08/16 22:19:38 Setting up Docker configuration...
2018/08/16 22:19:39 Successfully set up Docker configuration
2018/08/16 22:19:39 Logging in to registry: myregistry.azurecr.io
2018/08/16 22:19:55 Successfully logged in
Sending build context to Docker daemon  94.72kB
Step 1/5 : FROM node:9-alpine
...
```

Проверить наличие созданного образа в вашем реестре можно, выполнив следующую команду, чтобы просмотреть теги в репозитории myimage:

```azurecli-interactive
az acr repository show-tags --name $ACR_NAME --repository myimage
```

Тег v1 созданного вами образа должен появиться в выходных данных аналогично следующему:

```console
$ az acr repository show-tags --name $ACR_NAME --repository myimage
[
  "v1"
]
```

### <a name="delete-the-image"></a>Удаление образа

Теперь создайте `ImageDeleted` событие, удалив образ с помощью команды [AZ контроля репозитория Delete][az-acr-repository-delete] :

```azurecli-interactive
az acr repository delete --name $ACR_NAME --image myimage:v1
```

Вы должны увидеть выходные данные, похожие на следующие, в которых запрашивается подтверждение для удаления манифеста и связанных с ним образов:

```console
$ az acr repository delete --name $ACR_NAME --image myimage:v1
This operation will delete the manifest 'sha256:f15fa9d0a69081ba93eee308b0e475a54fac9c682196721e294b2bc20ab23a1b' and all the following images: 'myimage:v1'.
Are you sure you want to continue? (y/n): y
```

## <a name="view-registry-events"></a>Просмотр событий реестра

Вы передали образ в свой реестр, а затем удалили его. Перейдите к веб-приложению средства просмотра Сетки событий, и вы увидите события `ImageDeleted` и `ImagePushed`. Вы также можете увидеть событие проверки подписки, сгенерированное при выполнении команды в разделе [Подписка на события реестра](#subscribe-to-registry-events).

На следующем снимке экрана показан пример приложения с тремя событиями, а событие `ImageDeleted` расширено, чтобы показать его подробности.

![Веб-браузер, где показан пример приложения с событиями ImagePushed и ImageDeleted][sample-app-03]

Поздравляем! Если вы видите события `ImagePushed` и `ImageDeleted`, ваш реестр отправляет события в Сетку событий, а она отправляет эти события в конечную точку веб-приложения.

## <a name="clean-up-resources"></a>Очистка ресурсов

Когда вы закончите работу с ресурсами, созданными в этом руководстве, вы можете удалить их с помощью следующей команды Azure CLI. При удалении группы ресурсов все входящие в нее ресурсы удаляются безвозвратно.

**Внимание!** Это действие необратимо. Прежде чем выполнять команду, убедитесь, что вам больше не нужны какие-либо ресурсы в группе.

```azurecli-interactive
az group delete --name $RESOURCE_GROUP_NAME
```

## <a name="event-grid-event-schema"></a>Схема событий службы "Сетка событий Azure"

Справочник по схеме сообщения событий для Реестра контейнеров Azure можно найти в документации по Сетке событий:

[Схема событий службы "Сетка событий Azure" для Реестра контейнеров](../event-grid/event-schema-container-registry.md)

## <a name="next-steps"></a>Следующие шаги

В этом кратком руководстве вы развернули реестр контейнеров, создали образ с помощью задач ACR, удалили его и использовали события реестра из Сетки событий с помощью примера приложения. Теперь перейдите к руководству по задачам ACR, чтобы узнать больше о создании образов контейнеров в облаке, включая автоматическую сборку при обновлении базового образа:

> [!div class="nextstepaction"]
> [Руководство. Создание образов контейнера в облаке с помощью службы "Задачи Реестра контейнеров Azure"](container-registry-tutorial-quick-task.md)

<!-- IMAGES -->
[sample-app-01]: ./media/container-registry-event-grid-quickstart/sample-app-01.png
[sample-app-02]: ./media/container-registry-event-grid-quickstart/sample-app-02-no-events.png
[sample-app-03]: ./media/container-registry-event-grid-quickstart/sample-app-03-with-events.png

<!-- LINKS - External -->
[azure-account]: https://azure.microsoft.com/free/?WT.mc_id=A261C142F
[sample-app]: https://github.com/dbarkol/azure-event-grid-viewer

<!-- LINKS - Internal -->
[az-acr-create]: /cli/azure/acr/repository
[az-acr-repository-delete]: /cli/azure/acr/repository#az-acr-repository-delete
[az-eventgrid-event-subscription-create]: /cli/azure/eventgrid/event-subscription#az-eventgrid-event-subscription-create
[az-group-create]: /cli/azure/group#az-group-create
