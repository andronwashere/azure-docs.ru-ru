---
title: Использование действий с ожиданием и без ожидания в модели Conversation Learner в Microsoft Cognitive Services | Документация Майкрософт
titleSuffix: Azure
description: Сведения об использовании действий с ожиданием и без ожидания в модели Conversation Learner.
services: cognitive-services
author: nitinme
manager: nolachar
ms.service: cognitive-services
ms.subservice: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: nitinme
ms.openlocfilehash: 57a58b008792130c82a86ff4a5801f4bb39aabad
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "66387908"
---
# <a name="wait-and-non-wait-actions"></a>Действия с ожиданием и без ожидания

В этом руководстве показано различие между действиями с ожиданием и действиями без ожидания в приложении Conversation Learner.

## <a name="video"></a>Видео

[![Предварительная версия учебника о действиях с ожиданием и без ожидания](https://aka.ms/cl_Tutorial_v3_WaitnonWait_Preview)](https://aka.ms/cl_Tutorial_v3_WaitnonWait)

## <a name="requirements"></a>Требования
Для работы с этим руководством требуется запущенный бот обучения общего назначения.

    npm run tutorial-general

## <a name="details"></a>Сведения

- Действие с ожиданием В этом режиме система останавливает выполнение действий и ожидает ввода от пользователя.
- Действие без ожидания В этом режиме система сразу же выбирает следующее действие (не дожидаясь ввода от пользователя).

## <a name="steps"></a>Действия

### <a name="create-a-new-model"></a>Создание модели

1. В пользовательском веб-интерфейсе щелкните "Новая модель".
2. В поле "Имя" введите Wait Non-Wait, нажмите клавишу ВВОД или кнопку "Создать".

### <a name="create-the-first-two-wait-actions"></a>Создание первых двух действий с ожиданием

1. На левой панели щелкните "Действия", а затем нажмите кнопку "Создать действие".
2. В поле "Ответ бота..." введите What pizza would you like? (Какую пиццу Вы хотели бы заказать?).
    - Это действие с ожиданием, поэтому не снимайте флажок "Ожидать ответа".
3. Нажмите кнопку "Создать".
4. Повторите эти шаги, создав другое действие и указав Pizza on the way! (Пицца отправлена!) в качестве ответа бота.

### <a name="train-using-those-wait-actions"></a>Обучение с помощью действий с ожиданием

1. На левой панели щелкните "Обучающие диалоги" и нажмите кнопку "Создать обучающий диалог".
2. На панели чата, на которой отображается надпись "Введите текст сообщения...", введите Hi (Привет). 
    - Эта процедура имитирует реплику пользователя.
3. Нажмите кнопку "Оценить действия".
4. Выберите ответ What pizza would you like? (Какую пиццу Вы хотели бы заказать?).
5. Ответьте за пользователя: "Margherita" (Маргарита).
6. Нажмите кнопку "Оценить действия".
7. Выберите ответ Pizza on the way! (Пицца отправлена!).
8. Нажмите кнопку "Сохранить".

### <a name="create-a-non-wait-action-while-training"></a>Создание действия без ожидания во время обучения
Хотя действие без ожидания можно создать точно так же, как вы делали это раньше, это действие также можно создать в сеансе обучения.
1. Нажмите кнопку "Создать обучающий диалог".
2. От имени пользователя введите Hello (Привет).
3. Нажмите кнопку "Оценить действия".
4. Нажмите кнопку "+ Действие". 
    - Откроется знакомое диалоговое окно "Создать действие".
5. В поле "Ответ бота..." введите Welcome to Pizza Bot! (Приветствую Вас! Я бот для заказа пиццы).
6. Снимите флажок "Ожидать ответа".
7. Нажмите кнопку "Создать".
    - Обратите внимание на то, что бот немедленно отвечает Welcome to Pizza Bot! (Приветствую Вас! Я бот для заказа пиццы). и у вас сразу же запрашивается следующий ответ бота. Это вызвано тем, что ответ бота представляет собой действие без ожидания, которое мы только что создали.
9. Выберите ответ What pizza would you like? (Какую пиццу Вы хотели бы заказать?).
10. Ответьте за пользователя: "Margherita" (Маргарита).
11. Нажмите кнопку "Оценить действия".
12. Выберите ответ Pizza on the way! (Пицца отправлена!).
13. Нажмите кнопку "Сохранить".

> [!NOTE]
> Последовательность ответов бота при выполнении действий с ожиданием и без ожидания.

## <a name="next-steps"></a>Дальнейшие действия

> [!div class="nextstepaction"]
> [Introduction to entities](./04-introduction-to-entities.md) (Основные сведения о сущностях)
