---
title: Руководство по Настройка ThousandEyes для автоматической подготовки пользователей с помощью Azure Active Directory | Документация Майкрософт
description: Узнайте, как настроить Azure Active Directory для автоматической подготовки и отзыва учетных записей пользователей в ThousandEyes.
services: active-directory
documentationcenter: ''
author: ArvindHarinder1
manager: CelesteDG
ms.assetid: d4ca2365-6729-48f7-bb7f-c0f5ffe740a3
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/28/2019
ms.author: arvinh
ms.collection: M365-identity-device-management
ms.openlocfilehash: 7f8325737a62bba71364c02a234636999b0b1b9c
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "65964132"
---
# <a name="tutorial-configure-thousandeyes-for-automatic-user-provisioning"></a>Руководство по Настройка ThousandEyes для автоматической подготовки пользователей

Цель этого руководства — показать, как в ThousandEyes и Azure AD настроить автоматическую подготовку и отзыв учетных записей пользователей из Azure AD в ThousandEyes. 

## <a name="prerequisites"></a>Технические условия

Сценарий, описанный в этом учебнике, предполагает, что у вас уже имеется:

* клиент Azure Active Directory;
* клиент ThousandEyes с [планом Standard](https://www.thousandeyes.com/pricing) или выше; 
* учетная запись пользователя ThousandEyes с разрешениями администратора. 

> [!NOTE]
> Интеграция подготовки в Azure AD зависит от [API SCIM ThousandEyes](https://success.thousandeyes.com/PublicArticlePage?articleIdParam=kA044000000CnWrCAK), доступного для команд ThousandEyes, использующих план Standard или выше.

## <a name="assigning-users-to-thousandeyes"></a>Назначение пользователей в ThousandEyes

В Azure Active Directory для определения того, какие пользователи должны получать доступ к выбранным приложениям, используется концепция, называемая "назначение". В контексте автоматической подготовки учетной записи будут синхронизированы только те записи и группы, которые были "назначены" приложению в Azure AD. 

Перед настройкой и включением службы подготовки необходимо решить, какие пользователи или группы в Azure AD представляют пользователей, которым требуется доступ к приложению ThousandEyes. Приняв это решение, можно будет назначить этих пользователей для приложения ThousandEyes, выполнив следующие действия.

[Назначение корпоративному приложению пользователя или группы](../manage-apps/assign-user-or-group-access-portal.md)

### <a name="important-tips-for-assigning-users-to-thousandeyes"></a>Важные советы по назначению пользователей в ThousandEyes

* Рекомендуется назначить одного пользователя Azure AD в ThousandEyes для тестирования конфигурации подготовки. Дополнительные пользователи и/или группы можно назначить позднее.

* При назначении пользователя в ThousandEyes в диалоговом окне назначения необходимо выбрать роль **пользователя** или другую действительную роль для конкретного приложения (если доступно). Роль **Доступ по умолчанию** не подходит для подготовки, и эти пользователи пропускаются.

## <a name="configuring-user-provisioning-to-thousandeyes"></a>Настройка подготовки учетных записей пользователей в ThousandEyes 

В этом разделе описывается подключение вашего каталога Azure AD к API подготовки учетных записей пользователей в ThousandEyes и настройка службы подготовки для создания, обновления и отключения назначенных учетных записей пользователей в ThousandEyes на основе назначения пользователей и групп в Azure AD.

> [!TIP]
> Для ThousandEyes можно также включить единый вход на основе SAML. Для этого следуйте инструкциям на [портале Azure](https://portal.azure.com). Единый вход можно настроить независимо от автоматической подготовки, хотя эти две возможности дополняют друг друга.

### <a name="configure-automatic-user-account-provisioning-to-thousandeyes-in-azure-ad"></a>Настройка автоматической подготовки учетных записей пользователей для ThousandEyes в Azure AD

1. На [портале Azure](https://portal.azure.com) перейдите в раздел **Azure Active Directory > Корпоративные приложения > Все приложения**.

2. Если в ThousandEyes уже настроен единый вход, найдите свой экземпляр ThousandEyes с помощью поля поиска. В противном случае щелкните **Добавить** и выполните поиск **ThousandEyes** в коллекции приложений. Выберите ThousandEyes в результатах поиска и добавьте в список приложений.

3. Выберите экземпляр ThousandEyes, а затем перейдите на вкладку **Подготовка**.

4. Для параметра **Режим подготовки к работе** выберите значение **Automatic** (Автоматически).

    ![Подготовка ThousandEyes](./media/thousandeyes-provisioning-tutorial/ThousandEyes1.png)

5. В разделе **Учетные данные администратора** введите **Токен носителя OAuth**, созданный вашей учетной записью ThousandEyes (его можно найти и (или) сгенерировать в своей учетной записи ThousandEyes в разделе **Профиль**).

    ![Подготовка ThousandEyes](./media/thousandeyes-provisioning-tutorial/ThousandEyes2.png)

6. На портале Azure щелкните **Проверить подключение**, чтобы убедиться, что Azure AD может подключиться к приложению ThousandEyes. Если установить подключение не удалось, убедитесь, что у учетной записи ThousandEyes есть разрешения администратора, и повторите шаг 5.

7. В поле **Почтовое уведомление** введите адрес электронной почты пользователя или группы, которые должны получать уведомления об ошибках подготовки, а также установите флажок "Отправить уведомление по электронной почте при сбое".

8. Выберите команду **Сохранить**.

9. В разделе "Сопоставления" выберите **Synchronize Azure Active Directory Users to ThousandEyes** (Синхронизировать пользователей Azure Active Directory с ThousandEyes).

10. В разделе **Сопоставления атрибутов** просмотрите пользовательские атрибуты, которые синхронизированы из Azure AD в ThousandEyes. Атрибуты, выбранные как свойства **Matching**, используются для сопоставления учетных записей пользователей в ThousandEyes для операций обновления. Нажмите кнопку "Сохранить", чтобы подтвердить все изменения.

11. Чтобы включить службу подготовки Azure AD для ThousandEyes, измените значение параметра **Состояние подготовки** на **Включено** в разделе **Параметры**.

12. Выберите команду **Сохранить**.

После этого начнется начальная синхронизация всех пользователей и (или) групп, назначенных в ThousandEyes в разделе "Пользователи и группы". Начальная синхронизация занимает больше времени, чем последующие операции синхронизации. Если служба запущена, они выполняются примерно каждые 40 минут. В разделе **Сведения о синхронизации** можно отслеживать ход выполнения и переходить по ссылкам для просмотра журналов действий по подготовке, в которых зафиксированы все действия, выполняемые службой подготовки.

Дополнительные сведения о чтении журналов подготовки Azure AD см. в руководстве по [отчетам об автоматической подготовке учетных записей](../manage-apps/check-status-user-account-provisioning.md).

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Управление подготовкой учетных записей пользователей для корпоративных приложений](../manage-apps/configure-automatic-user-provisioning-portal.md)
* [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Дальнейшие действия

* [Сведения о просмотре журналов и получении отчетов о действиях по подготовке](../manage-apps/check-status-user-account-provisioning.md)
