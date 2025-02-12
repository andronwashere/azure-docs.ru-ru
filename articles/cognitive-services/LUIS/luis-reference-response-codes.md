---
title: Коды HTTP-ответов API — LUIS
titleSuffix: Azure Cognitive Services
description: Сведения о том, какие коды ответов HTTP возвращаются API-интерфейсами разработки и конечных точек LUIS
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: article
ms.date: 03/04/2019
ms.author: diberry
ms.openlocfilehash: c92bcf55b571c37efa308d9121ee4aee714e684a
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/26/2019
ms.locfileid: "68560085"
---
# <a name="common-api-response-codes-and-their-meaning"></a>Стандартные коды ответа API и их значение

API-интерфейсы [разработки](https://go.microsoft.com/fwlink/?linkid=2092087) и [конечных точек](https://go.microsoft.com/fwlink/?linkid=2092356) возвращают коды ответов HTTP. В то время как ответные сообщения содержат сведения, относящиеся к запросу, код состояния ответа HTTP имеет общий характер. 

## <a name="common-status-codes"></a>Распространенные коды состояний
В следующей таблице перечислены некоторые наиболее распространенные коды состояний ответов HTTP для API-интерфейсов [разработки](https://go.microsoft.com/fwlink/?linkid=2092087) и [конечных точек](https://go.microsoft.com/fwlink/?linkid=2092356).

|Код|API|Объяснение|
|:--|--|--|
|400|Разработки, конечных точек|Параметры запроса указаны неправильно. Это означает, что требуемые параметры отсутствуют, имеют неправильный формат или слишком большой размер|
|400|Разработки, конечных точек|Текст запроса указан неправильно. Это означает, что JSON отсутствует, имеет неправильный формат или слишком большой размер|
|401|Разработка|Используется ключ подписки для конечной точки вместо ключа разработки|
|401|Разработки, конечных точек|Недопустимый, неправильно форматированный или пустой ключ|
|401|Разработки, конечных точек| Ключ не соответствует региону|
|401|Разработка|Вы не являетесь владельцем или участником совместной работы|
|401|Разработка|Недопустимый порядок вызовов API|
|403|Разработки, конечных точек|Превышена месячная квота для ключей|
|409|Конечная точка|Приложение все еще загружается|
|410|Конечная точка|Приложение необходимо повторно обучить и опубликовать заново|
|414|Конечная точка|В запросе превышено максимально допустимое количество символов|
|429|Разработки, конечных точек|Превышено ограничение скорости (запросов в секунду)|

## <a name="next-steps"></a>Следующие шаги

* Документация по [разработке](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c2f) и [конечной точке](https://westus.dev.cognitive.microsoft.com/docs/services/5819c76f40a6350ce09de1ac/operations/5819c77140a63516d81aee78) REST API
