---
title: Как использовать теги версии с помощью модели диалога ученика — Azure Cognitive Services | Документация Майкрософт
titleSuffix: Azure
description: Узнайте, как использовать управление версиями и расстановку тегов в приложении Conversation Learner.
services: cognitive-services
author: nitinme
manager: nolachar
ms.service: cognitive-services
ms.subservice: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: nitinme
ms.openlocfilehash: 4067c7fb43cc200b8f49dbc14151c69a188e4e8e
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "66475703"
---
# <a name="how-to-use-version-tagging"></a>Как использовать теги версии

В этом учебнике показано, как помечать тегами версии модели Conversation Learner и указывать, какая из версий является активной.  

## <a name="requirements"></a>Требования
Для этого руководства требуется Bot Framework Emulator для создания записанных диалогов, а не пользовательский веб-интерфейс записи диалогов.  

Для работы с этим учебником требуется запущенный бот обучения общего назначения.

    npm run tutorial-general

## <a name="details"></a>Сведения

Помеченные версии модели являются статическими; их нельзя изменить. При изменении модели вы всегда редактируете главную версию. Когда вы добавляете новый тег, Conversation Learner записывает моментальный снимок модели в определенный момент времени. 

Бот будет использовать версию модели, выбранную в качестве активной, но все имеющиеся диалоги можно будет просматривать, только если для тега редактирования установлена главная версия. Если для свойства "Тег редактирования" модели установлена другая версия (кроме главной), вы сможете просмотреть моментальный снимок модели, но не сможете его изменить.

## <a name="steps"></a>Действия

### <a name="install-the-bot-framework-emulator"></a>Установка Bot Framework Emulator

1. Перейдите к [https://github.com/Microsoft/BotFramework-Emulator](https://github.com/Microsoft/BotFramework-Emulator).
2. Скачайте и установите эмулятор.

### <a name="create-a-model"></a>Создание модели

1. На домашней странице списка моделей нажмите кнопку `New Model`.
2. В поле `Name` введите Tutorial-18-Versioning и нажмите клавишу ВВОД.
4. На панели слева щелкните "Параметры".
5. Скопируйте содержимое поля CONVERSATION_LEARNER_MODEL_ID в буфер обмена.

### <a name="configure-the-emulator"></a>Настройка эмулятора

1. Откройте ENV-файл в корневой папке Conversation Learner.
2. Добавьте строку в ENV-файл следующим образом.
    - `CONVERSATION_LEARNER_MODEL_ID=[paste-model-id-from-clipboard-here]`
3. Перезапустите службу Conversation Learner, выйдя из командной строки и повторно выполнив приведенную ниже команду.
    - `npm run tutorial-general`
4. В Bot Framework Emulator создайте новую конфигурацию бота и установите в качестве URL-адреса конечной точки `http://localhost:3978/api/messages`

### <a name="version-1"></a>версия 1

Мы создадим одно действие для версии 1.

1. На левой панели веб-интерфейса пользователя щелкните "Действия", а затем нажмите кнопку `New Action`.
2. В поле "Ответ бота" введите hi there (version 1) (Привет [версия 1]).
3. Нажмите кнопку `Save`.

Теперь мы пометим это как "Версия 1" модели.

1. На левой панели щелкните "Параметры" и нажмите на значок ![](../media/tutorial18_version_tags.PNG)"Теги версии". Появится кнопка `New Tag`. Нажмите на нее.
    - Введите имя "Версия 1"
1. В раскрывающемся списке "Активный тег" выберите "Версия 1".  
    - Теперь каналы, использующие этот бот, будут использовать версию модели "Версия 1".
    - Сущности, действия и обучающие диалоги версии 1 модели больше нельзя изменить.
    - Если вы выберете версию 1 в качестве тега редактирования, вы сможете ТОЛЬКО просматривать модель, но не изменять ее.
    - Оставьте главную версию в качестве тега редактирования — единственная версия модели, которую можно редактировать.

Теперь вы увидите "Версия 1" в таблице "Теги версии".

### <a name="version-2"></a>версия 2

Теперь изменим нашу модель, чтобы она отличалась от версии 1.

1. На панели слева щелкните "Действия".
2. В таблице действий щелкните hi there (version 1) (Привет [версия 1]).
3. Измените поле "Ответ бота" на hi there (version 2) (Привет [версия 2]).
4. Нажмите кнопку `Save`.
5. Нажмите кнопку `New Action`.
6. В поле "Ответ бота" введите bye bye (version 2) (пока [версия 2]).

### <a name="confirm-bot-framework-emulator-is-using-version-1"></a>Убедитесь, что Bot Framework Emulator использует версию 1.

1. В Bot Framework Emulator введите сообщение Hey there (Привет).
2. Бот ответит: hi there (version 1) (Привет [версия 1]).
    - Это подтверждает, что версия 1 активна.

### <a name="view-the-conversation-logs-in-conversation-learner-web-ui"></a>Просмотр журналов диалога в веб-интерфейсе пользователя Conversation Learner

1. На панели слева щелкните "Записанные диалоги"
    - Если диалогов нет, нажмите кнопку "Обновить".
2. Обратите внимание на тег "Версия 1" в таблице.
3. В таблице щелкните hi there (version 1) (Привет [версия 1]).

> [!NOTE]
> Мы можем внести исправления, выбирая из всех доступных функций Conversation Learner, но эти правки будут внесены в главную версию, а не в версию 1.

Теперь вы знаете, как работает управление версиями и как можно взаимодействовать с ботом с помощью Bot Framework Emulator.

## <a name="next-steps"></a>Дальнейшие действия

> [!div class="nextstepaction"]
> [Перечисление сущностей и укажите сущности действия](./tutorial-enum-set-entity.md)
