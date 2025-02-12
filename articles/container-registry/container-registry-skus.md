---
title: Номера SKU реестра контейнеров Azure
description: Сравнение разных уровней служб, доступных в Реестре контейнеров Azure.
services: container-registry
author: dlepow
ms.service: container-registry
ms.topic: article
ms.date: 05/06/2019
ms.author: danlep
ms.openlocfilehash: f36b206ff015511dea7369617febe9220282bbe5
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "65069044"
---
# <a name="azure-container-registry-skus"></a>Номера SKU реестра контейнеров Azure

Реестр контейнеров Azure (ACR) доступен на нескольких уровнях служб, известных как номера SKU. Эти номера SKU предоставляют прогнозируемые цены и несколько вариантов, относящихся к согласованию с емкостью и шаблонами потребления частного реестра Docker в Azure.

| SKU | Управляемые | ОПИСАНИЕ |
| --- | :-------: | ----------- |
| **базовая;** | Yes | Недорогой начальный уровень для разработчиков, изучающих реестр контейнеров Azure. Основные реестры имеют те же программные возможности, как "стандартный" и "премиум" (таких как Azure Active Directory [интеграции проверки подлинности](container-registry-authentication.md#individual-login-with-azure-ad), [изображение удаления][container-registry-delete], и [веб-перехватчики][container-registry-webhook]). Тем не менее поставляемые хранилище и пропускная способность образов подходят для сценариев с меньшим коэффициентом использования. |
| **Стандартный** | Yes | Реестры уровня "Стандартный" предоставляют те же возможности, что и реестры уровня "Базовый", но больший размер хранилища и более высокую пропускную способность образов. Реестры уровня "Стандартный" подходят для большинства рабочих сценариев. |
| **Премиальный** | Yes | В реестрах уровня "Премиум" обеспечивается наивысший объем хранилища и количество параллельных операций, что делает возможными сценарии с большим объемом данных. В дополнение к более высокую пропускную способность образа уровня "премиум" добавляет возможности, включая [георепликации] [ container-registry-geo-replication] для управления одним реестром в нескольких регионах [содержимого доверия](container-registry-content-trust.md) для образа тег подписи, и [брандмауэры и виртуальные сети (Предварительная версия)](container-registry-vnet.md) для ограничения доступа к реестру. |
|  Классический (*недоступно после апреля 2019*) | Нет | Этот номер SKU включил первоначальный выпуск службы "Реестр контейнеров Azure" в Azure. Реестры уровня "Классический" поддерживаются учетной записью хранения, которую создает Azure в подписке, из-за чего ACR предоставляет меньше возможностей более высокого уровня, таких как повышенная пропускная способность и георепликация. |

> [!IMPORTANT]
> Классического реестра, SKU, **устаревшим**и будут недоступны после **апреля 2019**. Мы рекомендуем использовать Basic, Standard или Premium для всех новых реестров. Все существующие классические реестры должны обновляться до апреля 2019 г. Сведения об обновлении см. в разделе [обновление классического реестра][container-registry-upgrade].

Basic, Standard и Premium номеров SKU (Собирательно вызываемой *управляемых реестров*) обеспечивают одинаковые программные возможности. Они также все преимущества [хранилище изображений] [ container-registry-storage] полностью управляется Azure. Выбрав номер SKU более высокого уровня, можно получить больший уровень производительности и масштабируемости. При наличии нескольких уровней служб можно начать работу с "Базового" уровня служб, а затем перейти к уровню "Стандартный" и "Премиум" по мере увеличения потребления реестра.

## <a name="sku-feature-matrix"></a>Матрица возможностей номеров SKU

В следующей таблице приведены сведения о возможностях и ограничениях уровней служб "Базовый", "Стандартный" и "Премиум".

[!INCLUDE [container-instances-limits](../../includes/container-registry-limits.md)]

## <a name="changing-skus"></a>Изменение номеров SKU

Номер SKU реестра можно изменить с помощью Azure CLI или на портале Azure. Вы можете свободно перемещаться между управляемыми номерами SKU, пока в них имеется необходимая максимальная емкость. При переключении в один из управляемых номеров SKU из классической модели развертывания, вы не сможете вернуться на этот реестр — это одностороннее преобразование.

### <a name="azure-cli"></a>Инфраструктура CLI Azure

Для перемещения между номерами SKU в Azure CLI используйте команду [az acr update][az-acr-update]. Например, чтобы перейти на уровень обслуживания "Премиум":

```azurecli
az acr update --name myregistry --sku Premium
```

### <a name="azure-portal"></a>Портал Azure

В колонке **Обзор** реестра контейнеров на портале Azure выберите **Обновить**, а затем выберите новый **номер SKU** из раскрывающегося списка номеров SKU.

![Обновление номера SKU реестра контейнеров на портале Azure][update-registry-sku]

При наличии классического реестра на портале Azure нельзя выбрать управляемый номер SKU. Вместо этого необходимо сначала [обновление] [ container-registry-upgrade] до управляемого реестра.

## <a name="pricing"></a>Цены

Сведения о ценах на каждый номер SKU реестра контейнеров Azure см. на странице [цен на реестр контейнеров][container-registry-pricing].

Дополнительные сведения о стоимости передачи данных см. на [странице цен на пропускную способность](https://azure.microsoft.com/pricing/details/bandwidth/). 

## <a name="next-steps"></a>Дальнейшие действия

**Схема развития реестра контейнеров Azure**

Ознакомьтесь со [схемой развития ACR][acr-roadmap] на GitHub, чтобы найти сведения о новых возможностях в службе.

**UserVoice реестра контейнеров Azure**

Отправляйте свои предложения и голосуйте за предложения функций на [форуме UserVoice для ACR][container-registry-uservoice].

<!-- IMAGES -->
[update-registry-sku]: ./media/container-registry-skus/update-registry-sku.png

<!-- LINKS - External -->
[acr-roadmap]: https://aka.ms/acr/roadmap
[container-registry-pricing]: https://azure.microsoft.com/pricing/details/container-registry/
[container-registry-uservoice]: https://feedback.azure.com/forums/903958-azure-container-registry

<!-- LINKS - Internal -->
[az-acr-update]: /cli/azure/acr#az-acr-update
[container-registry-geo-replication]: container-registry-geo-replication.md
[container-registry-upgrade]: container-registry-upgrade.md
[container-registry-storage]: container-registry-storage.md
[container-registry-delete]: container-registry-delete.md
[container-registry-webhook]: container-registry-webhook.md
