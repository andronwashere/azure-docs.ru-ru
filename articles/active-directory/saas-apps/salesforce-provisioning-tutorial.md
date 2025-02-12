---
title: Руководство по Настройка Salesforce для автоматической подготовки пользователей с помощью Azure Active Directory | Документация Майкрософт
description: Узнайте, как настроить единый вход Azure Active Directory в приложении Salesforce.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: 49384b8b-3836-4eb1-b438-1c46bb9baf6f
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/08/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 712cc5ce62225987f8cc3ea13b5e4fd10a7d5eaf
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "60515766"
---
# <a name="tutorial-configure-salesforce-for-automatic-user-provisioning"></a>Руководство по Настройка Salesforce для автоматической подготовки пользователей

Цель этого учебника — показать, как в Salesforce и Azure AD необходимо выполнять автоматическую подготовку и отмену подготовки учетных записей пользователей из Azure AD в Salesforce.

## <a name="prerequisites"></a>Технические условия

Сценарий, описанный в этом учебнике, предполагает, что у вас уже имеется:

* клиент Azure Active Directory;
* клиент Salesforce.com.

> [!IMPORTANT]
> Если вы используете пробную учетную запись Salesforce.com, то не сможете настроить автоматическую подготовку пользователей. У пробных учетных записей нет необходимых прав доступа к API. Вы можете обойти это ограничение, используя для выполнения заданий данного руководства [бесплатную учетную запись разработчика](https://developer.salesforce.com/signup).

Если вы используете изолированную среду Salesforce, ознакомьтесь с [Учебником по интеграции с изолированной средой Salesforce](https://go.microsoft.com/fwLink/?LinkID=521879).

## <a name="assigning-users-to-salesforce"></a>Назначение пользователей в Salesforce

В Azure Active Directory для определения того, какие пользователи должны получать доступ к выбранным приложениям, используется концепция, называемая "назначение". В контексте автоматической подготовки учетной записи будут синхронизированы только те записи и группы, которые были "назначены" приложению в Azure AD.

Перед настройкой и включением службы подготовки необходимо решить, каким пользователям или группам в Azure AD требуется доступ к приложению Salesforce. После принятия решения можно назначить этих пользователей приложению Salesforce, следуя инструкциям в статье [Назначение пользователя или группы корпоративному приложению в Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal).

### <a name="important-tips-for-assigning-users-to-salesforce"></a>Важные рекомендации по назначению пользователей в Salesforce

* Рекомендуется назначить одного пользователя Azure AD в Salesforce для тестирования конфигурации подготовки. Дополнительные пользователи и/или группы можно назначить позднее.

* При назначении пользователя в Salesforce необходимо выбрать допустимую роль пользователя. Роль "Доступ по умолчанию" не работает для подготовки.

    > [!NOTE]
    > Это приложение импортирует профили из Salesforce в рамках процесса подготовки, который клиент может выбрать при назначении пользователей Azure AD. Обратите внимание, что профили, импортированные из Salesforce, появляются в Azure AD в качестве роли.

## <a name="enable-automated-user-provisioning"></a>Включение автоматической подготовки пользователей

В этом разделе описывается подключение к API подготовки учетной записи пользователя Azure AD в Salesforce и настройка подготовки службы для создания, обновления и отмены назначения учетных записей пользователей в Salesforce на основе назначения пользователей и групп в Azure AD.

> [!Tip]
> Для Salesforce также можно включить единый вход на базе управления лицензиями. Для этого следуйте инструкциям, указанным на [портале Azure](https://portal.azure.com). Единый вход можно настроить независимо от автоматической подготовки, хотя эти две возможности дополняют друг друга.

### <a name="configure-automatic-user-account-provisioning"></a>Настройка автоматической подготовки учетных записей пользователей

В этом разделе показано, как включить подготовку учетных записей пользователей Active Directory для Salesforce.

1. На [портале Azure](https://portal.azure.com) перейдите в раздел **Azure Active Directory > Корпоративные приложения > Все приложения**.

2. Если в Salesforce уже настроен единый вход, найдите свой экземпляр Salesforce с помощью поля поиска. В противном случае щелкните **Добавить** и выполните поиск **Salesforce** в коллекции приложений. Выберите Salesforce в результатах поиска и добавьте его в список приложений.

3. Выберите экземпляр Salesforce, а затем перейдите на вкладку **Подготовка**.

4. Для параметра **Режим подготовки к работе** выберите значение **Automatic** (Автоматически).

    ![Подготовка](./media/salesforce-provisioning-tutorial/provisioning.png)

5. В разделе **Учетные данные администратора** укажите следующие параметры конфигурации.

    a. В текстовом поле **Имя администратора** введите имя учетной записи Salesforce с профилем **системного администратора** в Salesforce.com.

    2\. В текстовом поле **Пароль администратора** введите пароль для этой учетной записи.

6. Для получения маркера безопасности Salesforce откройте новую вкладку и выполните вход с этой учетной записью администратора Salesforce. В правом верхнем углу страницы щелкните свое имя и выберите **Параметры**.

    ![Включение автоматической подготовки пользователей](./media/salesforce-provisioning-tutorial/sf-my-settings.png "Включение автоматической подготовки пользователей")

7. В области навигации слева щелкните **My Personal Information** (Моя личная информация), чтобы развернуть соответствующий раздел, и щелкните **Reset My Security Token** (Сбросить мой маркер безопасности).
  
    ![Включение автоматической подготовки пользователей](./media/salesforce-provisioning-tutorial/sf-personal-reset.png "Включение автоматической подготовки пользователей")

8. На странице **Reset Security Token** (Сброс маркера безопасности) нажмите кнопку **Reset Security Token** (Сбросить маркер безопасности).

    ![Включение автоматической подготовки пользователей](./media/salesforce-provisioning-tutorial/sf-reset-token.png "Включение автоматической подготовки пользователей")

9. Проверьте входящие сообщения в почтовом ящике, связанном с этой учетной записью администратора. Найдите сообщение от Salesforce.com с новым маркером безопасности.

10. Скопируйте маркер, перейдите к окну Azure AD и вставьте его в поле **Секретный токен**.

11. **URL-адрес клиента** нужно вводить, если экземпляр Salesforce находится в облаке для государственных организаций Salesforce. В других случаях это необязательный параметр. Введите URL-адрес клиента в формате https://\<ваш_экземпляр\>.my.salesforce.com, заменив \<ваш_экземпляр\> именем экземпляра Salesforce.

12. На портале Azure щелкните **Проверить подключение**, чтобы убедиться, что Azure AD может подключиться к приложению Salesforce.

13. В поле **Почтовое уведомление** введите адрес электронной почты пользователя или группы, которые должны получать уведомления об ошибках подготовки, а также установите флажок ниже.

14. Нажмите кнопку **Сохранить**.  

15. В разделе сопоставления выберите **Synchronize Azure Active Directory Users to Salesforce** (Синхронизировать пользователей Azure Active Directory с Salesforce).

16. В разделе **Attribute Mappings** (Сопоставление атрибутов) просмотрите пользовательские атрибуты, которые синхронизированы из Azure AD в Salesforce. Обратите внимание, что атрибуты, которые выбраны в качестве свойств **Matching** (Сопоставление), будут использоваться для сопоставления учетных записей пользователей в Salesforce для операций обновления. Нажмите кнопку "Сохранить", чтобы подтвердить все изменения.

17. Чтобы включить службу подготовки Azure AD для Salesforce, измените значение параметра **Provisioning Status** (Статус подготовки) на **On** (Включено) в разделе "Настройки".

18. Нажмите кнопку **Сохранить**.

После этого начнется начальная синхронизация всех пользователей и групп, назначенная в Salesforce в разделе "Пользователи и группы". Обратите внимание, что начальная синхронизация занимает больше времени, чем последующие операции синхронизации. Если служба запущена, они выполняются примерно каждые 40 минут. В разделе **Сведения о синхронизации** можно отслеживать ход выполнения и переходить по ссылкам для просмотра журналов действий, в которых зафиксированы все действия, выполняемые в приложении Salesforce службой подготовки.

Дополнительные сведения о чтении журналов подготовки Azure AD см. в руководстве по [отчетам об автоматической подготовке учетных записей](../manage-apps/check-status-user-account-provisioning.md).

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Управление подготовкой учетных записей пользователей для корпоративных приложений](tutorial-list.md)
* [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)
* [Настройка единого входа](https://docs.microsoft.com/azure/active-directory/active-directory-saas-salesforce-tutorial)
