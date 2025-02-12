---
title: Проверка качества данных для службы Пользовательское распознавание речи-Speech
titleSuffix: Azure Cognitive Services
description: Пользовательское распознавание речи предоставляет средства, позволяющие визуально проверять качество распознавания модели путем сравнения звуковых данных с соответствующими результатами распознавания. На портале Пользовательское распознавание речи можно воспроизвести отправленный звук и определить, верно ли предоставленный результат распознавания.  Это средство позволяет быстро проверять качество базовой модели преобразования речи в текст или обученную пользовательскую модель без необходимости транскрипция никаких звуковых данных.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 07/05/2019
ms.author: erhopf
ms.openlocfilehash: b58f9c17995128091b5c4badd228356dbacc6ae9
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/26/2019
ms.locfileid: "68562843"
---
# <a name="inspect-custom-speech-data"></a>Проверка данных Пользовательское распознавание речи

> [!NOTE]
> На этой странице предполагается, что вы прочитали [подготовленные тестовые данные для пользовательское распознавание речи](how-to-custom-speech-test-data.md) и отправили набор данных для проверки.

Пользовательское распознавание речи предоставляет средства, позволяющие визуально проверять качество распознавания модели путем сравнения звуковых данных с соответствующими результатами распознавания. На портале Пользовательское распознавание речи можно воспроизвести отправленный звук и определить, верно ли предоставленный результат распознавания. Это средство позволяет быстро оценить качество базовой модели преобразования речи в текст или обученную пользовательскую модель, не транскрипция никаких звуковых данных.

В этом документе вы узнаете, как визуально проверить качество модели с помощью предварительно отправленных обучающих данных.

На этой странице вы узнаете, как визуально проверить качество встроенной в Майкрософт модели преобразования речи в текст и (или) настраиваемой модели. Вы будете использовать данные, отправленные на вкладку **данные** для тестирования.

## <a name="create-a-test"></a>Создание теста

Чтобы создать тест, выполните следующие инструкции:

1. Перейдите в раздел **> преобразования речи в текст Пользовательское распознавание речи > тестирования**.
2. Нажмите кнопку **добавить тест**.
3. Выберите **проверить качество (только звуковые данные)** . Присвойте тесту имя, описание и выберите свой набор данных для звука.
4. Выберите до двух моделей, которые вы хотите протестировать.
5. Нажмите кнопку **Создать**.

После успешного создания теста можно сравнить модели параллельно.

## <a name="side-by-side-model-comparisons"></a>Сравнение параллельных моделей

После *успешной*проверки состояния теста щелкните имя тестового элемента, чтобы просмотреть подробные сведения о нем. На этой странице подробностей перечислены все фразы продолжительностью в наборе данных, указывающие результаты распознавания двух моделей вместе с прознаком из отправленного набора данных.

Чтобы проверить параллельное сравнение, можно переключить различные типы ошибок, включая вставку, удаление и подстановку. Прослушивая звук и сравнивая результаты распознавания в каждом столбце (показывая транскрипцию с отметкой и результаты двух моделей преобразования речи в текст), вы можете решить, какая модель соответствует вашим потребностям и какие улучшения требуются.

Проверка качества помогает проверить, достаточно ли для приложения качество конечной точки распознавания речи.  Для целевой меры точности, которая требует расшифрованной Audio, следуйте инструкциям, приведенным в статье [точность оценки](how-to-custom-speech-evaluate-data.md).

## <a name="next-steps"></a>Следующие шаги

* [Оценка данных](how-to-custom-speech-evaluate-data.md)
* [Обучение модели](how-to-custom-speech-train-model.md)
* [Развертывание модели](how-to-custom-speech-deploy-model.md)

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Подготовка тестовых данных для Пользовательское распознавание речи](how-to-custom-speech-test-data.md)
