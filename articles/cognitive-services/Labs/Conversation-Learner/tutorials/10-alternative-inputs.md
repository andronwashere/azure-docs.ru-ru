---
title: Как использовать альтернативные входные данные в Conversation Learner (Microsoft Cognitive Services) | Документация Майкрософт
titleSuffix: Azure
description: Сведения об использовании альтернативных входных данных в Conversation Learner.
services: cognitive-services
author: nitinme
manager: nolachar
ms.service: cognitive-services
ms.subservice: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: nitinme
ms.openlocfilehash: 383c063e1ba7a29986e4b1c48024072799234414
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "66387932"
---
# <a name="how-to-use-alternative-inputs"></a>Как использовать альтернативные входные данные

В этом учебнике показано, как использовать поле альтернативных входных данных для речевых фрагментов, вводимых пользователем в интерфейсе обучения.

## <a name="video"></a>Видео

[![Предварительная версия учебника об альтернативных входных данных](https://aka.ms/cl_Tutorial_v3_AlternativeInputs_Preview)](https://aka.ms/cl_Tutorial_v3_AlternativeInputs)

## <a name="requirements"></a>Требования
Для работы с этим руководством требуется запущенный бот обучения общего назначения.

    npm run tutorial-general

## <a name="details"></a>Сведения
Альтернативные входные данные — это другие, семантически эквивалентные речевые фрагменты, которые пользователь может произносить в определенной точке диалога обучения. Такие данные позволяют более компактно описать варианты речевых фрагментов, не используя каждый вариант в отдельном обучающем диалоге.

## <a name="steps"></a>Действия

### <a name="create-the-model"></a>Создание модели

1. В пользовательском веб-интерфейсе щелкните "Создать модель".
2. В поле "Имя" введите AlternativeInputs и нажмите клавишу ВВОД.
3. Нажмите кнопку "Создать".

### <a name="entity-creation"></a>Создание сущности

1. На левой панели щелкните "Сущности", а затем нажмите кнопку "Создать сущность".
2. Выберите "Пользовательская обучаемая" в поле "Тип сущности".
3. В поле "Имя сущности" укажите city (город).
4. Нажмите кнопку "Создать".

![](../media/T10_actions.png)

Теперь создадим три действия.

### <a name="create-the-first-action"></a>Создание первого действия

1. На левой панели щелкните "Действия", а затем нажмите кнопку "Создать действие".
2. В поле "Ответ бота..." введите Which city? (Какой город?).
3. В поле "Ожидаемая сущность в ответе пользователя..." введите сity (город).
4. В поле "Запрещенные сущности" введите city (город).
5. Нажмите кнопку "Создать".

![](../media/T10_action_create_1.png)

### <a name="create-the-second-action"></a>Создание второго действия

1. На левой панели щелкните "Действия", а затем нажмите кнопку "Создать действие".
2. В поле "Ответ бота..." введите The weather in the $city is probably sunny (Погода в городе $city, скорее всего, солнечная).
3. Нажмите кнопку "Создать".

![](../media/T10_action_create_2.png)

### <a name="create-the-third-action"></a>Создание третьего действия

1. На левой панели щелкните "Действия", а затем нажмите кнопку "Создать действие".
2. В поле "Ответ бота..." введите Try asking for the weather (Попробуйте спросить о погоде).
3. В поле "Запрещенные сущности" введите city (город).
4. Нажмите кнопку "Создать".

![](../media/T10_action_create_3.png)

Теперь у вас есть три действия.

![](../media/T10_actions.png)

### <a name="train-the-model"></a>Обучение модели

1. На левой панели щелкните "Обучающие диалоги" и нажмите кнопку "Создать обучающий диалог".
2. На панели чата, на которой отображается надпись "Введите текст сообщения...", введите текст What's the weather? (Какая сейчас погода?).
3. Нажмите кнопку "Оценить действия".
4. Выберите ответ Which city? (Какой город?).
5. На панели чата, на которой отображается надпись "Введите текст сообщения...", введите текст Denver (Денвер).
6. Нажмите кнопку "Оценить действия".
7. Выберите ответ The weather in Denver is probably sunny (Погода в Денвере, скорее всего, солнечная).
8. Нажмите кнопку Submit Changes (Отправить изменения).

![](../media/T10_training_1.png)

Давайте обучим модель дальше, создав еще один обучающий диалог.

### <a name="second-model-train-dialog"></a>Второй обучающий диалог модели

1. На левой панели щелкните "Обучающие диалоги" и нажмите кнопку "Создать обучающий диалог".
2. На панели чата, на которой отображается надпись "Введите текст сообщения...", введите текст What can you do? (Что ты умеешь делать?).
3. Нажмите кнопку "Оценить действия".
4. Выберите ответ: Try asking for weather (Попробуйте спросить о погоде).
5. На панели чата, на которой отображается надпись "Введите текст сообщения...", введите текст What's the weather in Seattle? (Какая сейчас погода в Сиэтле?).
6. В списке сущностей выберите Seattle (Сиэтл), затем — city (город).
7. Нажмите кнопку "Оценить действия".
8. Выберите ответ The weather in Seattle is probably sunny (Погода в Сиэтле, скорее всего, солнечная).
9. Нажмите кнопку Submit Changes (Отправить изменения).

![](../media/T10_training_2.png)

### <a name="third-model-train-dialog-using-alternative-input"></a>Третий обучающий диалог модели с использованием альтернативных входных данных

1. На левой панели щелкните "Обучающие диалоги" и нажмите кнопку "Создать обучающий диалог".
2. На панели чата, на которой отображается надпись "Введите текст сообщения...", введите текст help (помощь).
3. Нажмите кнопку "Оценить действия".
    - Модель не знает, что лучше выбрать, и по умолчанию выберет самый высокий процентиль.
4. Нажмите кнопку "Отказаться от обучения", а затем кнопку "Подтвердить".

![](../media/T10_training_3.png)

Давайте точнее настроим систему, используя альтернативные входные данные. Можно добавить альтернативные входные данные во время обучения или позже.

1. На левой панели щелкните "Обучающие диалоги" и выберите What can you do? (Что ты умеешь делать?) из списка обучающих диалогов.
1. Щелкните фразу What can you do? (Что ты умеешь делать?) на панели чата.
1. В поле "Добавить альтернативный ввод…" введите help (помощь) и нажмите ВВОД.
1. Нажмите кнопку "Сохранить изменения".

![](../media/T10_training_4.png)

Давайте добавим другие альтернативные входные данные для обработки Хьюстона.

1. Выберите фразу What's the weather in Seattle? (Какая погода в Сиэтле?) на панели чата.
1. В поле "Добавить альтернативный ввод..." введите forecast for Houston (прогноз для Хьюстона) и нажмите ВВОД.
   - Сообщение об ошибке показывает, что альтернативные входные данные должны быть семантически эквивалентны и содержать те же сущности, что и исходный речевой фрагмент, а не просто те же значения сущностей. Наличие одинаковых сущностей является обязательным.
1. Щелкните Houston (Хьюстон) и выберите city (город) в списке сущностей.
1. В поле "Добавить альтернативный ввод..." введите forecast for Seattle (прогноз для Сиэтла) и нажмите ВВОД.
1. Щелкните Seattle (Сиэтл) и выберите city (город) в списке сущностей.
1. Нажмите кнопку "Сохранить изменения".
1. Нажмите кнопку "Сохранить правки".

![](../media/T10_training_5.png)

### <a name="testing-the-model"></a>Тестирование модели

1. На левой панели щелкните "Записываемые диалоги" и нажмите "Создать записываемый диалог".
2. На панели чата, на которой отображается надпись "Введите текст сообщения...", введите текст help me (помоги).
3. На панели чата, на которой отображается надпись "Введите текст сообщения...", введите текст forecast for Denver (прогноз для Денвера).

![](../media/T10_logdialog.png)

## <a name="next-steps"></a>Дальнейшие действия

> [!div class="nextstepaction"]
> [Запись диалогов](./11-log-dialogs.md)
