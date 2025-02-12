---
title: Модерация текста — Content Moderator
titleSuffix: Azure Cognitive Services
description: Модерация текста используется для обнаружения нежелательных текстов, персональных данных и совпадений со списком терминов.
services: cognitive-services
author: sanjeev3
manager: nitinme
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: conceptual
ms.date: 01/10/2019
ms.author: sajagtap
ms.openlocfilehash: e1d5224d8dc86c82624613b0d2a984ceef3ae5bf
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/26/2019
ms.locfileid: "68564376"
---
# <a name="learn-text-moderation-concepts"></a>Изучение концепций модерации текста

В Content Moderator предусмотрены возможности модерации текста на основе машинных алгоритмов и [пользовательских проверок](Review-Tool-User-Guide/human-in-the-loop.md).

Можно блокировать, утверждать или проверять содержимое на основе политик и порогов. Используйте их, чтобы дополнить пользовательскую модерацию сред, в которых партнеры, сотрудники и потребители создают текстовое содержимое. Это могут быть чаты, доски обсуждений, чат-боты, каталоги электронной торговли и документы. 

Ответ службы будет содержать следующие данные:

- Ненормативная лексика: сопоставление по терминам с использованием встроенного списка ненормативных слов на разных языках.
- Классификация: распределение по трем категориям с применением машинных алгоритмов.
- Персональные данные
- Автоматически исправленный текст.
- Исходный текст.
- Язык

## <a name="profanity"></a>Ненормативная лексика

Если API обнаруживает ненормативную лексику на любом из [поддерживаемых языков](Text-Moderation-API-Languages.md), эти термины включаются в ответ. В ответе также указывается их расположение (`Index`) в исходном тексте. Объект `ListId` в следующем примере JSON обозначает обнаруженные термины из [пользовательских списков терминов](try-terms-list-api.md) (при наличии):

    "Terms": [
    {
        "Index": 118,
        "OriginalIndex": 118,
        "ListId": 0,
        "Term": "crap"
    }

> [!NOTE]
> Для параметра **language** (язык) задайте значение `eng` или оставьте это поле пустым, чтобы в ответе отобразился результат **classification** (классификация), которая сейчас выполняется в режиме предварительной версии. **Эта функция поддерживает только английский язык**.
>
> Для обнаружения **ненормативной лексики** укажите [код ISO 639-3](http://www-01.sil.org/iso639-3/codes.asp) для поддерживаемых языков, которые перечислены в этой статье, или оставьте это поле пустым.

## <a name="classification"></a>Классификация

**Функция классификации текста** Content Moderator с машинными алгоритмами поддерживает **только английский язык** и используется для обнаружения потенциально нежелательного содержимого. С ее помощью оценивается содержимое, которое может считаться недопустимым в определенном контексте. Она выдает вероятность для каждой категории и может рекомендовать пользовательскую проверку. Она использует обученную модель для выявления лексики оскорбительного, неуважительного или дискриминационного характера. Сюда входят сленговые и сокращенные выражения, оскорбления и специально искаженные слова, которые нужно проверять. 

Ниже приводится пример ответа в формате JSON:

    "Classification": {
        "ReviewRecommended": true,
        "Category1": {
            "Score": 1.5113095059859916E-06
            },
        "Category2": {
            "Score": 0.12747249007225037
            },
        "Category3": {
            "Score": 0.98799997568130493
        }
    }

### <a name="explanation"></a>Объяснение

- `Category1` обозначает потенциальное наличие лексики, которая в некоторых обстоятельствах может считаться сексуально откровенной или предназначенной только для взрослых.
- `Category2` обозначает потенциальное наличие лексики, которая в некоторых обстоятельствах может считаться сексуально окрашенной или не предназначенной для детей.
- `Category3` обозначает потенциальное наличие лексики, которая в определенных обстоятельствах может считаться оскорбительной.
- `Score` имеет значение в диапазоне от 0 до 1. Чем выше оценка, тем более подходящей модель считает соответствующую категорию. Эта возможность использует статистическую модель прогнозирования, а не оценки, кодированные вручную. Корпорация Майкрософт рекомендует протестировать ее на своих данных, чтобы проверить применимость анализа по каждой категории.
- `ReviewRecommended` принимает значения true или false в зависимости от внутренних порогов оценки. Клиенты могут на выбор использовать значения по умолчанию или настраивать собственные пороги в соответствии с действующими политиками.

## <a name="personal-data"></a>Персональные данные

Функция персональных данных обнаруживает потенциальные наличие таких сведений:

- Адрес электронной почты
- почтовый адрес в США;
- IP-адрес
- номер телефона США;
- номер телефона Великобритании;
- номер социального страхования.

Ниже показан пример результатов:

    "PII": {
        "Email": [{
            "Detected": "abcdef@abcd.com",
            "SubType": "Regular",
            "Text": "abcdef@abcd.com",
            "Index": 32
            }],
        "IPA": [{
            "SubType": "IPV4",
            "Text": "255.255.255.255",
            "Index": 72
            }],
        "Phone": [{
            "CountryCode": "US",
            "Text": "6657789887",
            "Index": 56
            }, {
            "CountryCode": "US",
            "Text": "870 608 4000",
            "Index": 212
            }, {
            "CountryCode": "UK",
            "Text": "+44 870 608 4000",
            "Index": 208
            }, {
            "CountryCode": "UK",
            "Text": "0344 800 2400",
            "Index": 228
            }, {
            "CountryCode": "UK",
            "Text": "0800 820 3300",
            "Index": 245
            }],
        "Address": [{
            "Text": "1 Microsoft Way, Redmond, WA 98052",
            "Index": 89
            }],
        "SSN": [{
            "Text": "999999999",
            "Index": 56
            }, {
            "Text": "999-99-9999",
            "Index": 267
            }]
        }

## <a name="auto-correction"></a>Автоматическое исправление

Предположим, мы получили следующий текст (слова lzay и f0x намеренно искажены):

    The qu!ck brown f0x jumps over the lzay dog.

Если передать его на автоматическое исправление, ответ будет содержать правильную версию текста:

    The quick brown fox jumps over the lazy dog.

## <a name="creating-and-managing-your-custom-lists-of-terms"></a>Создание пользовательских списков терминов и управление ими

Используемый по умолчанию глобальный список терминов отлично подходит для большинства случаев, но в некоторых ситуациях нужно учесть дополнительные термины, характерные для вашего бизнеса. Например, вы можете фильтровать бренды ваших конкурентов в сообщениях пользователей.

> [!NOTE]
> Существует максимальное ограничение в **5 списков терминов**, каждый из которых может содержать **не более 10 000 терминов**.
>

В следующем примере представлен идентификатор совпадения по списку:

    "Terms": [
    {
        "Index": 118,
        "OriginalIndex": 118,
        "ListId": 231.
        "Term": "crap"
    }

Content Moderator предоставляет [API списка терминов](https://westus.dev.cognitive.microsoft.com/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f67f) с операциями для управления пользовательскими списками терминов. Начните работу с изучения [консоли API для списков терминов](try-terms-list-api.md) и примеров кода для REST API. Также изучите [краткое руководство по спискам терминов для .NET](term-lists-quickstart-dotnet.md), если вы уже знакомы с C# и Visual Studio.

## <a name="next-steps"></a>Следующие шаги

Проверьте в работе [консоль API для списков терминов](try-text-api.md) и примеры кода для REST API. Также изучите [краткое руководство по модерации текста для .NET](text-moderation-quickstart-dotnet.md), если вы уже знакомы с C# и Visual Studio.
