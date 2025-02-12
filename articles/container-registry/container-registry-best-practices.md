---
title: Рекомендации по использованию реестра контейнеров Azure
description: Узнайте, как эффективно использовать реестр контейнеров Azure, следуя приведенным ниже рекомендациям.
services: container-registry
author: dlepow
ms.service: container-registry
ms.topic: article
ms.date: 09/27/2018
ms.author: danlep
ms.openlocfilehash: 2cf64c7c4f99a57c4a4a6cf03e68e8af803ceca9
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "60787364"
---
# <a name="best-practices-for-azure-container-registry"></a>Рекомендации по использованию реестра контейнеров Azure

Следуя этим рекомендациям, вы сможете обеспечить максимальную производительность и экономичное использование частного реестра Docker в Azure.

## <a name="network-close-deployment"></a>Развертывание в ближайшей сети

Создайте реестр контейнеров в том же регионе Azure, в котором выполняется развертывание контейнеров. Размещение реестра в сети региона, расположенной близко к узлам контейнеров, поможет снизить как задержки, так и затраты.

Развертывание в ближайшей сети — одна из основных причин использования частного реестра контейнеров. Образы Docker обладают эффективной [многоуровневой конструкцией](https://docs.docker.com/engine/userguide/storagedriver/imagesandcontainers/), позволяющей выполнять добавочное развертывание. Тем не менее новым узлам требуется извлекать все уровни, необходимые для заданного образа. Эта начальная команда `docker pull` может быстро добавить в развертывание до нескольких гигабайтов. Размещение частного реестра близко к развертыванию сводит к минимуму задержку сети.
Кроме того, исходящий сетевой трафик во всех общедоступных облаках, включая Azure, является платным. Извлечение образов из одного центра обработки данных в другой увеличивает затраты на исходящий сетевой трафик, как и задержку.

## <a name="geo-replicate-multi-region-deployments"></a>Георепликация развертываний в нескольких регионах

Используйте функцию [георепликации](container-registry-geo-replication.md) реестра контейнеров Azure, если вы развертываете контейнеры в нескольких регионах. Будь то обслуживание глобальных клиентов из локальных центров обработки данных или совместная работа команды разработчиков, находящихся в разных расположениях, можно упростить управление реестром и свести к минимуму задержку, выполняя георепликацию реестра. Георепликация доступна только при наличии реестров уровня [Премиум](container-registry-skus.md).

Чтобы узнать, как использовать георепликацию, ознакомьтесь с руководством, состоящим из трех частей, [Подготовка геореплицированного реестра контейнеров Azure](container-registry-tutorial-prepare-registry.md).

## <a name="repository-namespaces"></a>Пространства имен репозитория

За счет использования пространств имен репозитория можно обеспечить общий доступ к отдельному реестру для нескольких групп в вашей организации. Реестры могут совместно использоваться в различных развертываниях и разными командами. Реестр контейнеров Azure поддерживает вложенные пространства имен, включая изоляцию групп.

Например, рассмотрим следующие теги образа контейнера. Образы, используемые во всей в организации, например `aspnetcore`, помещаются в корневое пространство имен, тогда как образы контейнеров, принадлежащие группе по эксплуатации и обслуживанию (Production) и группе по маркетингу (Marketing), используют собственные пространства имен.

```
contoso.azurecr.io/aspnetcore:2.0
contoso.azurecr.io/products/widget/web:1
contoso.azurecr.io/products/bettermousetrap/refundapi:12.3
contoso.azurecr.io/marketing/2017-fall/concertpromotions/campaign:218.42
```

## <a name="dedicated-resource-group"></a>Выделенная группа ресурсов

Так как реестры контейнеров — это ресурсы, используемые на нескольких узлах контейнера, реестр должен находиться в собственной группе ресурсов.

Хотя вы можете поэкспериментировать с определенным типом узла, например с экземплярами контейнеров Azure, скорее всего вы решите удалить экземпляр контейнера после завершения работы. Однако вы можете также хранить коллекцию образов, отправленных в реестр контейнеров Azure. Поместив реестр в собственную группу ресурсов, можно свести к минимуму риск случайного удаления коллекции образов из реестра при удалении группы ресурсов экземпляра контейнера.

## <a name="authentication"></a>Authentication

При аутентификации с помощью реестра контейнеров Azure существуют два основных сценария: отдельная аутентификация и аутентификация службы (или "автономная"). В таблице ниже приводится краткий обзор этих сценариев и рекомендуемый метод аутентификации для каждого из них.

| type | Пример сценария | Рекомендуемый метод |
|---|---|---|
| Отдельное удостоверение | Разработчик извлекает образы на свой компьютер для разработки или отправляет образы с него. | [az acr login](/cli/azure/acr?view=azure-cli-latest#az-acr-login) |
| Удостоверение службы или автономное удостоверение | Конвейеры сборки и развертывания, в которых пользователь не участвует напрямую. | [Субъект-служба](container-registry-authentication.md#service-principal) |

Более подробные сведения об аутентификации с помощью реестра контейнеров Azure см. в разделе [Аутентификация с помощью частного реестра контейнеров Docker](container-registry-authentication.md).

## <a name="manage-registry-size"></a>Управление размером реестра

Ограничения хранилища для каждого [номера SKU реестра контейнеров][container-registry-skus] предназначены для согласования с типичным сценарием: **Базовый** — для начала работы, **Стандартный** — для большинства рабочих приложений, **Премиум** — для гипермасштабируемой производительности и [георепликации][container-registry-geo-replication]. На протяжении жизненного цикла вашего реестра вам необходимо управлять его размером, периодически удаляя неиспользуемое содержимое.

Используйте команду Azure CLI [az acr show-usage][az-acr-show-usage] для отображения текущего размера реестра.

```console
$ az acr show-usage --resource-group myResourceGroup --name myregistry --output table
NAME      LIMIT         CURRENT VALUE    UNIT
--------  ------------  ---------------  ------
Size      536870912000  185444288        Bytes
Webhooks  100                            Count
```

Также можно найти текущее хранилище, используемое в **обзоре** реестра на портале Azure.

![Сведения об использовании реестра на портале Azure][registry-overview-quotas]

### <a name="delete-image-data"></a>Удаление данных образа

Реестр контейнеров Azure поддерживает несколько методов для удаления данных образа из реестра контейнеров. Можно удалить образы с использованием тега или манифеста хэш-кода или удалить весь репозиторий.

Дополнительные сведения об удалении данных образа из реестра, включая немаркированные (иногда называемые "висячие" или "потерянные") образы, см. [здесь](container-registry-delete.md).

## <a name="next-steps"></a>Дальнейшие действия

Реестр контейнеров Azure доступен на нескольких уровнях, называемых номерами SKU, каждый из которых обеспечивает разные возможности. Дополнительные сведения о доступных номерах SKU см. в разделе [Номера SKU реестра контейнеров Azure](container-registry-skus.md).

<!-- IMAGES -->
[delete-repository-portal]: ./media/container-registry-best-practices/delete-repository-portal.png
[registry-overview-quotas]: ./media/container-registry-best-practices/registry-overview-quotas.png

<!-- LINKS - Internal -->
[az-acr-repository-delete]: /cli/azure/acr/repository#az-acr-repository-delete
[az-acr-show-usage]: /cli/azure/acr#az-acr-show-usage
[azure-cli]: /cli/azure
[azure-portal]: https://portal.azure.com
[container-registry-geo-replication]: container-registry-geo-replication.md
[container-registry-skus]: container-registry-skus.md
