---
title: Удаление ресурсов образов в Реестре контейнеров Azure
description: Дополнительные сведения об эффективном управлении размером реестра путем удаления данных образа контейнера.
services: container-registry
author: dlepow
ms.service: container-registry
ms.topic: article
ms.date: 06/17/2019
ms.author: danlep
ms.openlocfilehash: c603afa61499a615a0882cef06f14fd3d080a9ef
ms.sourcegitcommit: 66237bcd9b08359a6cce8d671f846b0c93ee6a82
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/11/2019
ms.locfileid: "67797767"
---
# <a name="delete-container-images-in-azure-container-registry"></a>Удаление образов контейнеров в службе "Реестр контейнеров Azure"

Чтобы управлять размером Реестра контейнеров Azure, необходимо периодически удалять данные устаревших образов. Хотя некоторые образы контейнеров, развернутые в рабочей среде, могут потребовать долговременного хранения, другие, как правило, могут быть удалены быстрее. Например, в сценарии автоматизированной сборки и тестирования реестр может быть быстро заполнен изображениями, которые никогда не придется развертывать, и после того, как завершится этап построения и тестирования, они незамедлительно могут быть удалены.

Так как данные образов можно удалить несколькими способами, важно понимать, как каждая из операций удаления влияет на использование хранилища. В этой статье рассматриваются несколько методов для удаления данных изображения:

* Удаление [репозитория](#delete-repository). Это действие удаляет из репозитория все образы и все уникальные слои.
* Удаление [тегов](#delete-by-tag). Это действие удаляет образ, тег и все уникальные слои, связанные с образом, и все другие теги, связанные с образом.
* Удаление [дайджеста манифеста](#delete-by-manifest-digest). Удаление образа, всех уникальных слоев в образе и всех тегов, связанных с образом.

Примеры сценариев предоставляются для автоматизации операций удаления.

Введение в концепции, см. в разделе [о реестрах, репозитории и образы](container-registry-concepts.md).

## <a name="delete-repository"></a>Удаление репозитория

При удалении репозитория удаляются все образы хранилища, включая все теги, уникальные слои и манифесты. При удалении репозитория, то восстановить дисковое пространство, используемое с образов, которые ссылаются на уникальный слои в нем.

Следующая команда Azure CLI удаляет репозиторий acr-helloworld и все теги и манифесты репозитория. Если слои, ссылается на удаленные манифесты не содержит ссылок на другие образы в реестре, также будут удалены их данные слоя, в результате восстановления дискового пространства.

```azurecli
 az acr repository delete --name myregistry --repository acr-helloworld
```

## <a name="delete-by-tag"></a>Удаление по тегу

С помощью операции удаления можно удалить отдельные образы из репозитория по указанным именам репозиториев и тегов. При удалении по тегу восстанавливается дисковое пространство, используемое любыми уникальными слоями в образе (слои, которые не используются совместно с другими образами в реестре).

Чтобы удалить тег, используйте [az acr репозитория delete][az-acr-repository-delete] и укажите имя образа в `--image` параметра. Удаляются все уникальные слои образа и все другие теги, связанные с удаляемым образом.

Пример удаления образа acr-helloworld:latest из реестра myregistry.

```azurecli
$ az acr repository delete --name myregistry --image acr-helloworld:latest
This operation will delete the manifest 'sha256:0a2e01852872580b2c2fea9380ff8d7b637d3928783c55beb3f21a6e58d5d108' and all the following images: 'acr-helloworld:latest', 'acr-helloworld:v3'.
Are you sure you want to continue? (y/n): y
```

> [!TIP]
> Удаление *по тегу* не следует путать с удалением тега (обестегивание). Вы можете удалить тег с помощью команды Azure CLI [az acr репозитория удалить тег][az-acr-repository-untag]. Отсутствует пространство освобождается в том случае, когда тег изображения, так как его [манифеста](container-registry-concepts.md#manifest) и уровень данных остаются в реестре. Удаляется только ссылка на тег.

## <a name="delete-by-manifest-digest"></a>Удаление с помощью дайджеста манифеста

[Дайджест манифеста](container-registry-concepts.md#manifest-digest) может быть связан с одним, несколькими тегами или не иметь связей с тегами вообще. При удалении с помощью дайджеста все теги, связанные манифестом, удаляются, как и данные слоя для любых слоев, уникальных для образа. Данные общего слоя не удаляются.

Для удаления с помощью дайджеста сначала перечислите дайджесты манифеста для репозитория, содержащего изображения, которые необходимо удалить. Пример:

```console
$ az acr repository show-manifests --name myregistry --repository acr-helloworld
[
  {
    "digest": "sha256:0a2e01852872580b2c2fea9380ff8d7b637d3928783c55beb3f21a6e58d5d108",
    "tags": [
      "latest",
      "v3"
    ],
    "timestamp": "2018-07-12T15:52:00.2075864Z"
  },
  {
    "digest": "sha256:3168a21b98836dda7eb7a846b3d735286e09a32b0aa2401773da518e7eba3b57",
    "tags": [
      "v2"
    ],
    "timestamp": "2018-07-12T15:50:53.5372468Z"
  }
]
```

Затем укажите хэш-кода, необходимо удалить в [az acr репозитория delete][az-acr-repository-delete] команды. Команда имеет следующий формат.

```azurecli
az acr repository delete --name <acrName> --image <repositoryName>@<digest>
```

Пример удаления последнего манифеста из списка выходных данных (с тегом "v2").

```console
$ az acr repository delete --name myregistry --image acr-helloworld@sha256:3168a21b98836dda7eb7a846b3d735286e09a32b0aa2401773da518e7eba3b57
This operation will delete the manifest 'sha256:3168a21b98836dda7eb7a846b3d735286e09a32b0aa2401773da518e7eba3b57' and all the following images: 'acr-helloworld:v2'.
Are you sure you want to continue? (y/n): y
```

`acr-helloworld:v2` Образ будет удален из реестра, так как все данные слоя, уникальные для этого образа. Если манифест связан с несколькими тегами, все связанные теги также удаляются.

## <a name="delete-digests-by-timestamp"></a>Удалить дайджестов по метке времени

Чтобы поддерживался размер репозитория или реестра, может потребоваться периодически удалять манифеста дайджестов старше определенной даты.

Следующая команда Azure CLI выводит все манифеста хэш-кода в репозитории, старше указанной отметке времени, в порядке возрастания. Замените `<acrName>` и `<repositoryName>` значениями, уместными для вашей среды. Метка времени может быть выражение с даты времени или даты, как в следующем примере.

```azurecli
az acr repository show-manifests --name <acrName> --repository <repositoryName> \
--orderby time_asc -o tsv --query "[?timestamp < '2019-04-05'].[digest, timestamp]"
```

После определения устаревших манифеста дайджестов, можно запустить следующий сценарий Bash, чтобы удалить манифеста дайджестов старше указанной отметке времени. Для работы этого сценария требуется Azure CLI и **xargs**. Сценарий не выполняет удаление по умолчанию. Чтобы включить удаление образа, измените значение `ENABLE_DELETE` на `true`.

> [!WARNING]
> Используйте приведенный ниже пример сценария с осторожностью--данные удаленных изображения — неисправимая. Если у вас есть системы, которые извлекают образы с манифеста хэш-кода (в отличие от имени образа), не следует запускать эти скрипты. Если удалить манифеста дайджестов не этих систем извлекать образы из реестра. Вместо извлечения манифестом, попробуйте внедрить *уникального разметка* схема [рекомендация][tagging-best-practices]. 

```bash
#!/bin/bash

# WARNING! This script deletes data!
# Run only if you do not have systems
# that pull images via manifest digest.

# Change to 'true' to enable image delete
ENABLE_DELETE=false

# Modify for your environment
# TIMESTAMP can be a date-time string such as 2019-03-15T17:55:00.
REGISTRY=myregistry
REPOSITORY=myrepository
TIMESTAMP=2019-04-05  

# Delete all images older than specified timestamp.

if [ "$ENABLE_DELETE" = true ]
then
    az acr repository show-manifests --name $REGISTRY --repository $REPOSITORY \
    --orderby time_asc --query "[?timestamp < '$TIMESTAMP'].digest" -o tsv \
    | xargs -I% az acr repository delete --name $REGISTRY --image $REPOSITORY@% --yes
else
    echo "No data deleted."
    echo "Set ENABLE_DELETE=true to enable deletion of these images in $REPOSITORY:"
    az acr repository show-manifests --name $REGISTRY --repository $REPOSITORY \
   --orderby time_asc --query "[?timestamp < '$TIMESTAMP'].[digest, timestamp]" -o tsv
fi
```

## <a name="delete-untagged-images"></a>Удаление образов без тегов

Как было упомянуто в разделе [Дайджест манифеста](container-registry-concepts.md#manifest-digest), в результате отправки измененных образов с помощью существующего тега **untags** ранее отправленный образ стал потерянным (или "несвязанным"). Манифест образа, который был отправлен ранее, и его данные слоя остаются в реестре. Рассмотрим следующую последовательность событий.

1. Отправка образа *acr-helloworld* с тегом **latest**. `docker push myregistry.azurecr.io/acr-helloworld:latest`
1. Проверка манифеста для репозитория *acr-helloworld*.

   ```console
   $ az acr repository show-manifests --name myregistry --repository acr-helloworld
   [
     {
       "digest": "sha256:d2bdc0c22d78cde155f53b4092111d7e13fe28ebf87a945f94b19c248000ceec",
       "tags": [
         "latest"
       ],
       "timestamp": "2018-07-11T21:32:21.1400513Z"
     }
   ]
   ```

1. Изменение Dockerfile репозитория *acr-helloworld*.
1. Отправка образа *acr-helloworld* с тегом **latest**. `docker push myregistry.azurecr.io/acr-helloworld:latest`
1. Проверка манифеста для репозитория *acr-helloworld*.

   ```console
   $ az acr repository show-manifests --name myregistry --repository acr-helloworld
   [
     {
       "digest": "sha256:7ca0e0ae50c95155dbb0e380f37d7471e98d2232ed9e31eece9f9fb9078f2728",
       "tags": [
         "latest"
       ],
       "timestamp": "2018-07-11T21:38:35.9170967Z"
     },
     {
       "digest": "sha256:d2bdc0c22d78cde155f53b4092111d7e13fe28ebf87a945f94b19c248000ceec",
       "tags": [],
       "timestamp": "2018-07-11T21:32:21.1400513Z"
     }
   ]
   ```

Как показано в выходных данных последнего шага в последовательности, теперь установлена потерянной манифеста, `"tags"` свойство является пустым списком. Этот манифест существует в реестре вместе с уникальными данными слоя, на которые он ссылается. **Чтобы удалить потерянные образы и их данные слоя, необходимо удалить дайджест манифеста**.

## <a name="delete-all-untagged-images"></a>Удаление всех образов без тегов

С помощью следующей команды Azure CLI можно вывести список всех образов репозитория без тегов. Замените `<acrName>` и `<repositoryName>` значениями, уместными для вашей среды.

```azurecli
az acr repository show-manifests --name <acrName> --repository <repositoryName> --query "[?tags[0]==null].digest"
```

С помощью этой команды в сценарии, можно удалить все образы без тегов в репозитории.

> [!WARNING]
> С осторожностью используйте следующие примеры сценариев — данные удаленных изображений восстановлению не подлежат. Если у вас есть системы, которые извлекают образы с манифеста хэш-кода (в отличие от имени образа), не следует запускать эти скрипты. Удаление образов без тегов не позволит этим системам получать образы из реестра. Вместо извлечения манифестом, попробуйте внедрить *уникального разметка* схема [рекомендация][tagging-best-practices].

**Azure CLI в Bash**

Следующий сценарий Bash удаляет из репозитория все образы, которые не были помечены тегом. Для работы этого сценария требуется Azure CLI и **xargs**. Сценарий не выполняет удаление по умолчанию. Чтобы включить удаление образа, измените значение `ENABLE_DELETE` на `true`.

```bash
#!/bin/bash

# WARNING! This script deletes data!
# Run only if you do not have systems
# that pull images via manifest digest.

# Change to 'true' to enable image delete
ENABLE_DELETE=false

# Modify for your environment
REGISTRY=myregistry
REPOSITORY=myrepository

# Delete all untagged (orphaned) images
if [ "$ENABLE_DELETE" = true ]
then
    az acr repository show-manifests --name $REGISTRY --repository $REPOSITORY  --query "[?tags[0]==null].digest" -o tsv \
    | xargs -I% az acr repository delete --name $REGISTRY --image $REPOSITORY@% --yes
else
    else
    echo "No data deleted."
    echo "Set ENABLE_DELETE=true to enable image deletion of these images in $REPOSITORY:"
    az acr repository show-manifests --name $REGISTRY --repository $REPOSITORY --query "[?tags[0]==null]" -o tsv
fi
```

**Azure CLI в PowerShell**

Следующий сценарий PowerShell удаляет из репозитория все образы, которые не были помечены тегом. Для него требуется PowerShell и Azure CLI. Сценарий не выполняет удаление по умолчанию. Чтобы включить удаление образа, измените значение `$enableDelete` на `$TRUE`.

```powershell
# WARNING! This script deletes data!
# Run only if you do not have systems
# that pull images via manifest digest.

# Change to '$TRUE' to enable image delete
$enableDelete = $FALSE

# Modify for your environment
$registry = "myregistry"
$repository = "myrepository"

if ($enableDelete) {
    az acr repository show-manifests --name $registry --repository $repository --query "[?tags[0]==null].digest" -o tsv `
    | %{ az acr repository delete --name $registry --image $repository@$_ --yes }
} else {
    Write-Host "No data deleted."
    Write-Host "Set `$enableDelete = `$TRUE to enable image deletion."
    az acr repository show-manifests --name $registry --repository $repository --query "[?tags[0]==null]" -o tsv
}
```

## <a name="next-steps"></a>Следующие шаги

Дополнительные сведения об образе хранилища в реестре контейнеров Azure см. в статье [Хранение образа контейнера в реестре контейнеров Azure](container-registry-storage.md).

<!-- IMAGES -->
[manifest-digest]: ./media/container-registry-delete/01-manifest-digest.png

<!-- LINKS - External -->
[docker-manifest-inspect]: https://docs.docker.com/edge/engine/reference/commandline/manifest/#manifest-inspect
[portal]: https://portal.azure.com
[tagging-best-practices]: https://stevelasker.blog/2018/03/01/docker-tagging-best-practices-for-tagging-and-versioning-docker-images/

<!-- LINKS - Internal -->
[az-acr-repository-delete]: /cli/azure/acr/repository#az-acr-repository-delete
[az-acr-repository-show-manifests]: /cli/azure/acr/repository#az-acr-repository-show-manifests
[az-acr-repository-untag]: /cli/azure/acr/repository#az-acr-repository-untag
