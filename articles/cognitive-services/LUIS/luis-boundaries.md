---
title: Ограничения — LUIS
titleSuffix: Azure Cognitive Services
description: Это статья об известных ограничениях Интеллектуальной службы распознавания речи Azure (LUIS). У LUIS есть несколько областей границ. Граница модели управляет намерениями, сущностями и возможностями в LUIS. Предел квот основывается на типе ключа. Сочетание клавиш управляет веб-сайтом LUIS.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: article
ms.date: 07/29/2019
ms.author: diberry
ms.custom: seodec18
ms.openlocfilehash: 6d4991a0a05bbdd7143987bfa227cc40732cda35
ms.sourcegitcommit: 3877b77e7daae26a5b367a5097b19934eb136350
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/30/2019
ms.locfileid: "68639250"
---
# <a name="boundaries-for-your-luis-model-and-keys"></a>Границы модели LUIS и ключи
У LUIS есть несколько областей границ. Первая — [граница модели](#model-boundaries), которая управляет намерениями, сущностями и возможностями в LUIS. Вторая область — [пределы квот](#key-limits) на основе типа ключа. Третья область границ — [сочетания клавиш](#keyboard-controls) для управления веб-сайтом LUIS. Четвертая область — [сопоставление региона мира](luis-reference-regions.md) между веб-сайтом разработки LUIS и API-интерфейсами [конечной точки](luis-glossary.md#endpoint) LUIS. 


## <a name="model-boundaries"></a>Границы модели

Если размеры вашего приложения превышают границы модели LUIS, попробуйте использовать приложение [диспетчера LUIS](luis-concept-enterprise.md#dispatch-tool-and-model) или [контейнер LUIS](luis-container-howto.md). 

|Область|Ограничение|
|--|:--|
| [Имя приложения][luis-get-started-create-app] | * Макс. кол-во символов по умолчанию |
| [Пакетное тестирование][batch-testing]| 10 наборов данных, 1000 высказываний для каждого набора данных|
| Явный список | 50 для каждого приложения|
| Внешние сущности | без ограничений |
| [Intents][intents]|500 для каждого приложения: 499 пользовательских целей, а также обязательное намерение _None_.<br>Приложение [на основе диспетчеризации](https://aka.ms/dispatch-tool) имеет 500 соответствующих источников диспетчеризации.|
| [Сущности списка](./luis-concept-entity-types.md) | Родительский объект: 50, дочерний объект: 20 000. Каноническое имя — *макс. кол-во символов по умолчанию. Значения синонимов не имеют ограничений по длине. |
| [Сущности и роли, полученные от компьютера](./luis-concept-entity-types.md):<br> составного<br>простого<br>роль сущности|Ограничение в 100 родительских сущностей или 330 сущностей, в зависимости от того, какое ограничение будет достигнуто первым. Роль считается сущностью с целью этой границы. Примером является составной объект с простой сущностью, имеющей 2 роли: 1 составной + 1 простой + 2 роли = 4 из 330 сущностей.|
| [Предварительный просмотр — сущности динамического списка](https://aka.ms/luis-api-v3-doc#dynamic-lists-passed-in-at-prediction-time)|2 списка запросов к конечной точке прогноза ~ 1000 на запрос|
| [Шаблоны](luis-concept-patterns.md)|500 шаблонов для каждого приложения.<br>Максимальная длина шаблона — 400 символов.<br>3 сущности Pattern.any для каждого шаблона<br>Максимум 2 вложенных необязательных текста в шаблоне|
| [Pattern.Any](./luis-concept-entity-types.md)|100 для каждого приложения, 3 сущности pattern.any для каждого шаблона |
| [Список фраз][phrase-list]|10 списков фраз, 5000 элементов для каждого списка|
| [Предварительно созданные сущности](./luis-prebuilt-entities.md) | без ограничений|
| [Сущности регулярного выражения](./luis-concept-entity-types.md)|20 сущностей<br>макс. 500 символов для каждого шаблона сущности регулярного выражения|
| [Роли](luis-concept-roles.md)|300 ролей для каждого приложения, 10 ролей для каждой сущности|
| [Utterance][utterances] | 500 символов|
| [Фразы продолжительностью][utterances] | 15 000 на приложение-нет ограничений на количество фразы продолжительностью на каждую цель|
| [Версии](luis-concept-version.md)| без ограничений |
| [Имя версии][luis-how-to-manage-versions] | 10 символов — только буквенно-цифровые и точка (.) |

* Макс. кол-во символов по умолчанию — 50. 

<a name="intent-and-entity-naming"></a>

## <a name="object-naming"></a>Именование объектов

Не используйте следующие символы в следующих именах.

|Object|Исключить символы|
|--|--|
|Назначение, сущность и имена ролей|`:`<br>`$`|
|Имя версии|`\`<br> `/`<br> `:`<br> `?`<br> `&`<br> `=`<br> `*`<br> `+`<br> `(`<br> `)`<br> `%`<br> `@`<br> `$`<br> `~`<br> `!`<br> `#`|

## <a name="key-usage"></a>Использование ключа

Распознавание речи имеет отдельные ключи: один тип ключей для разработки, другой — для запроса конечной точки прогнозирования. Дополнительные сведения о различиях между основными типами ключей см. в статье [Ключи разработки и запрашивания конечной точки прогнозирования в LUIS](luis-concept-keys.md).

## <a name="key-limits"></a>Ограничения ключа

Ключ разработки имеет разные ограничения для разработки и конечной точки. Ключ конечной точки службы LUIS является допустимым только для запросов конечной точки.


|Ключ|Разработка|Конечная точка|Цель|
|--|--|--|--|
|Распознавание речи, разработка / начальный этап|1 млн/месяц, 5/с|1 тыс./месяц, 5/с|Разработка приложения LUIS|
|Language Understanding [Subscription][pricing] -F0 — уровень Free |недопустимо|10 тыс./месяц, 5/с|Запрос конечной точки LUIS|
|Language Understanding [Subscription][pricing] -S0 — базовый уровень|недопустимо|50/с|Запрос конечной точки LUIS|
|[Подписка][pricing] на автообслуживание — S0 — уровень Standard|недопустимо|50/с|Запрос конечной точки LUIS|
|[Интеграция анализа тональности](luis-how-to-publish-app.md#enable-sentiment-analysis)|недопустимо|бесплатно|Добавление сведений о тональности, включая извлечение данных ключевой фразы |
|[Интеграция с речью](../speech-service/how-to-recognize-intents-from-speech-csharp.md)|недопустимо|5,5 долл. США/1 тыс. запросов конечной точки|Преобразование голосового высказывания в текстовое и возвращение результатов LUIS|

## <a name="keyboard-controls"></a>Элементы управления клавиатуры

|Ввод с клавиатуры | Описание | 
|--|--|
|CTRL+E|Переключение между токенами и сущностями в списке высказываний|

## <a name="website-sign-in-time-period"></a>Период времени входа веб-сайта

Выполнять вход можно в течение **60 минут**. По истечении этого времени возникнет ошибка. Необходимо снова выполнить вход.

[luis-get-started-create-app]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-get-started-create-app
[batch-testing]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-concept-test#batch-testing
[intents]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-concept-intent
[phrase-list]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-concept-feature
[utterances]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-concept-utterance
[luis-how-to-manage-versions]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-how-to-manage-versions
[pricing]: https://azure.microsoft.com/pricing/details/cognitive-services/language-understanding-intelligent-services/
<!-- TBD: fix this link -->
[speech-to-intent-pricing]: https://azure.microsoft.com/pricing/details/cognitive-services/language-understanding-intelligent-services/
