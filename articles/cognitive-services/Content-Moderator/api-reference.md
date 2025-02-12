---
title: Справочник по API — Content Moderator
titleSuffix: Azure Cognitive Services
description: Узнайте о модерации различного контента и интерфейсах API проверки для Content Moderator.
services: cognitive-services
author: sanjeev3
manager: nitinme
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: reference
ms.date: 05/29/2019
ms.author: sajagtap
ms.openlocfilehash: 3ad911a95dbe6209fcf55adcac3cf2937b06d1ff
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/26/2019
ms.locfileid: "68565612"
---
# <a name="content-moderator-api-reference"></a>Справочник по API Content Moderator

Вы можете приступить к работе с API Content Moderator Azure следующими способами:

- В портал Azure Подпишитесь [на Content MODERATOR API](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesContentModerator).
- См. раздел [пробное Content Moderator в Интернете](quick-start.md) для регистрации с помощью [средства проверки Content Moderator](https://contentmoderator.cognitive.microsoft.com/).

## <a name="moderation-apis"></a>API модерации

Ниже перечислены интерфейсы API Content Moderator, которые можно использовать для настройки рабочих процессов модерации.

| Описание | Ссылка |
| -------------------- |-------------|
| **API модерации изображений**<br /><br />Позволяет проверить изображения на наличие содержимого для взрослых и непристойного характера с использованием тегов, оценки достоверности и других сведений, извлекаемых из содержимого. <br /><br />Используйте эти сведения для публикации, отклонения или проверки содержимого в рабочем процессе после модерации. <br /><br />| [Справочник по API модерации изображений](https://westus.dev.cognitive.microsoft.com/docs/services/57cf753a3f9b070c105bd2c1/operations/57cf753a3f9b070868a1f66c "Image Moderation API reference")   |
| **API модерации текста**<br /><br />Позволяет проверить содержимое текста. Возвращаются термины ненормативной лексики и персональные данные. <br /><br />Используйте эти сведения для публикации, отклонения или проверки содержимого в рабочем процессе после модерации.<br /><br /> | [Справочник по API модерации текста](https://westus.dev.cognitive.microsoft.com/docs/services/57cf753a3f9b070c105bd2c1/operations/57cf753a3f9b070868a1f66f "Text Moderation API reference")   |
| **API модерации видео**<br /><br />Позволяет проверять видео на наличие содержимого для взрослых и содержимого непристойного характера. <br /><br />Используйте эти сведения для публикации, отклонения или проверки содержимого в рабочем процессе после модерации.<br /><br /> | [Справочник по API модерации видео](video-moderation-api.md "Video Moderation API overview")   |
| **API управления списками**<br /><br />Позволяет создать и настроить пользовательские списки исключения или включения для текста и изображений. Если эта функция включена, то операции консолей **Image - Match** (Изображение — сопоставление) и **Text - Screen** (Текст — проверка) выполняют нечеткое сопоставление отправленного содержимого с вашими пользовательскими списками. <br /><br />Для повышения эффективности можно пропустить шаг модерации на основе машинного обучения.<br /><br /> | [Справочник по API управления списками](https://westus.dev.cognitive.microsoft.com/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f675 "List Management API reference")   |

## <a name="review-apis"></a>Просмотр API-интерфейсов

API-интерфейсы проверки имеют следующие компоненты:

| Описание | Ссылка |
| -------------------- |-------------|
| **Задания**<br /><br /> Запуск рабочих процессов модерации "поиск и проверка"для содержимого изображений и текста. Задание модерации проверяет содержимое с помощью API модерации изображений и API модерации текста. Задания модерации используют определенные рабочие процессы и рабочие процессы по умолчанию для создания проверок. <br /><br />После того, как модератор-пользователь проверит автоматически назначенные теги и данные прогноза и отправит решение по модерации содержимого, API проверки отправляет все сведения на конечную точку API.<br /><br /> | [Справочник по заданиям](https://westus.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/580519483f9b0709fc47f9c5 "Job reference")   |
| **Проверки**<br /><br />Используйте инструмент проверки для непосредственного создания проверок изображений или текста для модераторов-пользователей.<br /><br /> После того, как модератор-пользователь проверит автоматически назначенные теги и данные прогноза и отправит решение по модерации содержимого, API проверки отправляет все сведения на конечную точку API.<br /><br /> | [Справочник по проверкам](https://westus.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/580519483f9b0709fc47f9c4 "Review reference")   |
| **Рабочие процессы**<br /><br />Создание, обновление пользовательских рабочих процессов вашей команды и получение сведений о них. Для определения рабочих процессов используется инструмент проверки. <br /> <br />Рабочие процессы обычно используются в Content Moderator, но можно также использовать некоторые интерфейсы API, которые доступны в виде соединителей в инструменте проверки.<br /><br /> | [Справочник по рабочим процессам](https://westus.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/5813b46b3f9b0711b43c4c59 "Workflow reference")   |