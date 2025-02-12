---
title: Определение содержимого для взрослых и содержимого непристойного характера. Компьютерное зрение
titleSuffix: Azure Cognitive Services
description: Понятия, связанные с определением содержимого для взрослых и содержимого непристойного характера на изображениях с помощью API компьютерного зрения.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: conceptual
ms.date: 02/08/2019
ms.author: pafarley
ms.custom: seodec18
ms.openlocfilehash: 69a4c136e9c210dd40e004b8d5e1c1a2a8fceaa7
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "60368357"
---
# <a name="detect-adult-and-racy-content"></a>Обнаружение содержимого для взрослых и содержимого непристойного характера

Компьютерное зрение может обнаруживать содержимое для взрослых среди изображений, чтобы разработчики могли ограничить отображение таких изображений в их программном обеспечении. Флаги содержимого применяются с рейтингом от единицы до нуля, чтобы разработчики могли интерпретировать результаты в зависимости от их собственных предпочтений. 

> [!NOTE]
> Такую функцию также предлагает служба [Azure Content Moderator](https://docs.microsoft.com/azure/cognitive-services/content-moderator/overview). Ознакомьтесь также с другими решениями для более тщательной модерации контента, например с модерацией текста или рабочими процессами пользовательской проверки.

## <a name="content-flag-definitions"></a>Определения флага содержимого

Под изображениями **для взрослых** понимаются изображения, имеющие порнографический характер, на которых часто показаны обнаженные тела и половые акты. 

Под изображениями **непристойного характера** понимаются изображения, имеющие неприличный характер, но зачастую менее откровенные, чем изображения категории **Для взрослых**. 

## <a name="identify-adult-and-racy-content"></a>Определение содержимого для взрослых и содержимого непристойного характера

API [анализа](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa).

Метод анализа изображений возвращает в ответе JSON два логических свойства, `isAdultContent` и `isRacyContent`, которые обозначают содержимое для взрослых и содержимое непристойного характера соответственно. Метод также возвращает два свойства, `adultScore` и `racyScore`, которые представляют степень достоверности при определении содержимого для взрослых и содержимого непристойного характера соответственно.

## <a name="next-steps"></a>Дальнейшие действия

Ознакомьтесь с принципами [обнаружения содержимого, связанного с определенными предметными областями](concept-detecting-domain-content.md), и [определения лиц](concept-detecting-faces.md).
