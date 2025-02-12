---
title: Запустите экземпляры контейнеров Azure
titleSuffix: Azure Cognitive Services
description: Развертывание контейнера LUIS в экземпляре контейнера Azure и протестировать его в веб-браузере.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 7/5/2019
ms.author: dapine
ms.openlocfilehash: 1d19d80bbc334a376f77eb285349fb1a87a91a54
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/09/2019
ms.locfileid: "67711574"
---
# <a name="deploy-the-language-understanding-luis-container-to-azure-container-instances"></a>Развертывание контейнера Language Understanding (LUIS) в экземплярах контейнеров Azure

Дополнительные сведения о развертывании Cognitive Services [LUIS](luis-container-howto.md) контейнера в Azure [экземпляры контейнеров](https://docs.microsoft.com/azure/container-instances/). Эта процедура демонстрирует создание ресурса обнаружения аномалий. Затем мы обсудим, получение образа связанного контейнера. Наконец рассматривается возможность выполнять оркестрацию двух из браузера. С помощью контейнеров можно сместить внимание разработчиков от управления инфраструктурой, чтобы вместо этого сосредоточиться на разработке приложений.

[!INCLUDE [Prerequisites](../containers/includes/container-prerequisites.md)]

[!INCLUDE [Create LUIS resource](includes/create-luis-resource.md)]

[!INCLUDE [Create LUIS Container Instance resource](../containers/includes/create-container-instances-resource.md)]

[!INCLUDE [API documentation](../../../includes/cognitive-services-containers-api-documentation.md)]

[!INCLUDE [Next steps](../containers/includes/containers-next-steps.md)]
