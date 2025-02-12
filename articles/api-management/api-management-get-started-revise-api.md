---
title: Безопасное внесение некритических изменений с помощью редакций в службе управления API Azure | Документация Майкрософт
description: Следуйте инструкциям этого руководства, чтобы научиться вносить некритические изменения с помощью редакций в службе управления API.
services: api-management
documentationcenter: ''
author: vladvino
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.custom: mvc
ms.topic: tutorial
ms.date: 06/15/2018
ms.author: apimpm
ms.openlocfilehash: 757c710ddca72ec6a1383b51a8b536d196e6cb8c
ms.sourcegitcommit: b7a44709a0f82974578126f25abee27399f0887f
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/18/2019
ms.locfileid: "67203592"
---
# <a name="use-revisions-to-make-non-breaking-changes-safely"></a>Безопасное внесение некритических изменений с помощью редакций
Когда готовый к работе API начинают использовать разработчики, следует осторожно вносить изменения в этот API, чтобы не нарушить работу вызывающих его сторон. Полезно также сообщать разработчикам о внесенных изменениях. Для этого можно использовать **редакции** в службе управления API Azure. Дополнительные сведения см. в статьях [Versions & revisions](https://blogs.msdn.microsoft.com/apimanagement/2017/09/14/versions-revisions/) (Версии и редакции) и [API Versioning with Azure API Management](https://blogs.msdn.microsoft.com/apimanagement/2017/09/13/api-versioning-with-azure-api-management/) (Управление версиями API с помощью службы управления API Azure).

Из этого руководства вы узнаете, как выполнять следующие задачи:

> [!div class="checklist"]
> * Добавление новой редакции
> * Внесение некритических изменений в редакцию
> * Назначение редакции текущей и добавление записи журнала изменений
> * Обзор портала разработчика для просмотра изменений и журнала изменений

![Журнал изменений на портале разработчика](media/api-management-getstarted-revise-api/azure_portal.PNG)

## <a name="prerequisites"></a>Предварительные требования

+ Ознакомьтесь с [терминологией службы управления API в Azure](api-management-terminology.md).
+ Выполните инструкции из краткого руководства — [Создание экземпляра службы управления API Azure](get-started-create-service-instance.md).
+ Также выполните инструкции из руководства [Импорт и публикация первого API](import-and-publish.md).

## <a name="add-a-new-revision"></a>Добавление новой редакции

![Добавление версии API](media/api-management-getstarted-revise-api/07-AddRevisions-01-AddNewRevision.png)

1. Выберите страницу **Интерфейсы API**.
2. Выберите **Demo Conference API** в списке API (или другой API, в который нужно внести исправления).
3. Выберите вкладку **Исправления** в меню вверху страницы.
4. Выберите **+ Add Revision** (+ Добавить редакцию).

    > [!TIP]
    > Также можно выбрать команду **Добавить редакцию** в контекстном меню ( **…** ) API.

5. Введите описание вашей новой редакции, чтобы напомнить, для чего она будет использоваться.
6. Нажмите кнопку **Создать**
7. Новая редакция создана.

    > [!NOTE]
    > Исходный API остается в редакции **Revision 1**. Именно эту редакцию по-прежнему будут вызывать пользователи, пока вы не выберете текущей другую редакцию.

## <a name="make-non-breaking-changes-to-your-revision"></a>Внесение некритических изменений в редакцию

![Изменение версии](media/api-management-getstarted-revise-api/07-AddRevisions-02-MakeChanges.png)

1. В списке API выберите **Demo Conference API**.
2. Выберите вкладку **Конструктор** в верхней части экрана.
3. Обратите внимание, что **селектор исправлений** (непосредственно над вкладкой "Конструктор") показывает текущую версию **Revision 2**.

    > [!TIP]
    > Используйте селектор редакции для переключения между редакциям, с которыми вы хотите работать.

4. Щелкните **+ Add Operation** (+ Добавить операцию).
5. Укажите, что создается операция **POST**, а затем введите имя, отображаемое имя и URL-адрес операции — **test**.
6. **Сохраните** новую операцию.
7. Теперь мы внесли изменение в редакцию **Revision 2**. Используйте **селектор редакции** в верхней части страницы, чтобы вернуться к редакции **Revision 1**.
8. Обратите внимание, что новая операция не отображается в редакции **Revision 1**. 

## <a name="make-your-revision-current-and-add-a-change-log-entry"></a>Назначение редакции текущей и добавление записи журнала изменений

1. Выберите вкладку **Исправления** в меню в верхней части страницы.

    ![Меню "Исправления" на экране редакции](media/api-management-getstarted-revise-api/RevisionsMenu.PNG)

2. Откройте контекстное меню ( **…** ) для редакции **Revision 2**.
3. Выберите **Сделать текущим**.
4. Установите флажок **Post to Public Change log for this API** (Записать в общедоступный журнал изменений для этого API), если нужно опубликовать примечания об этом изменении. Введите описание внесенного изменения, которое будет отображаться для разработчиков, например: **Тестирование исправлений. Добавлена новая операция test.**
5. Теперь редакция **Revision 2** является текущей.

## <a name="browse-the-developer-portal-to-see-changes-and-change-log"></a>Обзор портала разработчика для просмотра изменений и журнала изменений

1. На портале Azure выберите **API**.
2. Выберите **Портал разработчика** в верхнем меню.
3. Выберите **API**, а затем — **Demo Conference API**.
4. Обратите внимание на то, что теперь доступна новая операция **test**.
5. Выберите **API Change History** (Журнал изменений API) под именем API.
6. Обратите внимание на запись журнала изменений в этом списке.

    ![Developer portal](media/api-management-getstarted-revise-api/developer_portal.PNG)

## <a name="next-steps"></a>Дополнительная информация

Из этого руководства вы узнали, как выполнить следующие задачи:

> [!div class="checklist"]
> * Добавление новой редакции
> * Внесение некритических изменений в редакцию
> * Назначение редакции текущей и добавление записи журнала изменений
> * Обзор портала разработчика для просмотра изменений и журнала изменений

Перейдите к следующему руководству:

> [!div class="nextstepaction"]
> [Публикация нескольких версий API](api-management-get-started-publish-versions.md)
