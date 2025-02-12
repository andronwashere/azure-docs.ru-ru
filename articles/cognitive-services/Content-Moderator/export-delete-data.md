---
title: Экспорт или удаление данных пользователя Content Moderator
titleSuffix: Azure Cognitive Services
description: Сведения о том, как экспортировать и удалять данные в Content Moderator.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: conceptual
ms.date: 02/07/2019
ms.author: pafarley
ms.openlocfilehash: b18bc2907c00ed1424db440f09c914faff6a9863
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/26/2019
ms.locfileid: "68565558"
---
# <a name="export-or-delete-user-data-in-content-moderator"></a>Экспорт и удаление данных пользователя в Content Moderator

Content Moderator собирает данные пользователей, необходимые для работы службы, но пользователи могут полностью контролировать просмотр, экспорт и удаление своих данных с помощью [средства проверки](https://contentmoderator.cognitive.microsoft.com/), а также [API-интерфейсов модерации и проверки](https://docs.microsoft.com/azure/cognitive-services/content-moderator/api-reference).

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-intro-sentence.md)]

Дополнительные сведения о том, как экспортировать и удалять данные пользователей в Content Moderator, см. в следующей таблице.

| Data | Операция экспорта | Операция удаления |
| ---- | ---------------- | ---------------- |
| Сведения об учетной записи (ключи подписки) | Н/Д | Удаление с помощью портала Azure (подписки Azure). Также можно воспользоваться кнопкой **Удалить команду** на странице "Параметры команды" [пользовательского интерфейса проверки](https://contentmoderator.cognitive.microsoft.com/). |
| Изображения для настраиваемого сопоставления | Вызовите [API получения идентификаторов изображений](https://westus.dev.cognitive.microsoft.com/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f676). Изображения хранятся в формате одностороннего защищаемого хэширования, и способа извлечь фактические изображения не существует. | Вызовите [API удаления всех изображений](https://westus.dev.cognitive.microsoft.com/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f686). Или удаление ресурса Content Moderator с помощью портала Azure. |
| Условия для настраиваемого сопоставления | Вызовите [API получения всех терминов](https://westus.dev.cognitive.microsoft.com/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f67e). | Вызовите [API удаления всех терминов](https://westus.dev.cognitive.microsoft.com/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f67d). Или удаление ресурса Content Moderator с помощью портала Azure. |
| Tags | Н/Д | Можно использовать значок **Удалить**, доступный для каждого тега на странице Tag settings (Параметры тегов) пользовательского интерфейса проверки. Также можно воспользоваться кнопкой **Удалить команду** на странице "Параметры команды" [пользовательского интерфейса проверки](https://contentmoderator.cognitive.microsoft.com/). |
| Отзывы | Вызовите [API создания проверки](https://westus.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/580519483f9b0709fc47f9c2). | Воспользуйтесь кнопкой **Удалить команду** на странице "Параметры команды" [пользовательского интерфейса проверки](https://contentmoderator.cognitive.microsoft.com/).
| Пользователи | Н/Д | Воспользуйтесь значком **Удалить**, доступным для каждого пользователя на странице "Параметры команды" [пользовательского интерфейса проверки](https://contentmoderator.cognitive.microsoft.com/). Также можно воспользоваться кнопкой **Удалить команду** на странице "Параметры команды" [пользовательского интерфейса проверки](https://contentmoderator.cognitive.microsoft.com/). |

