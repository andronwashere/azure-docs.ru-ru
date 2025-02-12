---
title: Руководство по Интеграция Azure Active Directory с MyWorkDrive | Документация Майкрософт
description: Сведения о том, как настроить единый вход между Azure Active Directory и MyWorkDrive.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: celested
ms.assetid: 4d049778-3c7b-46c0-92a4-f2633a32334b
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 05/20/2019
ms.author: jeedes
ms.openlocfilehash: 0c3eb72abc90347c8e18a2f56a5d4756ecd80f3a
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "67096335"
---
# <a name="tutorial-integrate-myworkdrive-with-azure-active-directory"></a>Руководство по Интеграция MyWorkDrive с Azure Active Directory

В этом руководстве описано, как интегрировать MyWorkDrive с Azure Active Directory (Azure AD). Интеграция MyWorkDrive с Azure AD обеспечивает следующие возможности.

* C помощью Azure AD вы можете контролировать доступ к MyWorkDrive.
* Вы можете включить автоматический вход пользователей в MyWorkDrive с помощью учетных записей Azure AD.
* Централизованное управление учетными записями через портал Azure.

Чтобы узнать больше об интеграции приложений SaaS с Azure AD, прочитайте статью [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

## <a name="prerequisites"></a>Предварительные требования

Чтобы приступить к работе, потребуется следующее:

* подписка Azure AD Если у вас нет подписки, [здесь](https://azure.microsoft.com/pricing/free-trial/) вы можете получить бесплатную пробную версию сроком на один месяц.
* подписка MyWorkDrive с поддержкой единого входа.

## <a name="scenario-description"></a>Описание сценария

В рамках этого руководства вы настроите и проверите единый вход Azure AD в тестовой среде. MyWorkDrive поддерживает единый вход, инициируемый **поставщиком услуг** и **поставщиком удостоверений**.

## <a name="adding-myworkdrive-from-the-gallery"></a>Добавление MyWorkDrive из коллекции

Чтобы настроить интеграцию MyWorkDrive с Azure AD, вам потребуется добавить MyWorkDrive из коллекции в список управляемых приложений SaaS.

1. Войдите на [портал Azure](https://portal.azure.com) с помощью личной учетной записи Майкрософт либо рабочей или учебной учетной записи.
1. В области навигации слева выберите службу **Azure Active Directory**.
1. Перейдите в колонку **Корпоративные приложения** и выберите **Все приложения**.
1. Чтобы добавить новое приложение, выберите **Новое приложение**.
1. В разделе **Добавление из коллекции** в поле поиска введите **MyWorkDrive**.
1. Выберите **MyWorkDrive** в области результатов и добавьте это приложение. Подождите несколько секунд, пока приложение не будет добавлено в ваш клиент.

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Настройка и проверка единого входа в Azure AD

Настройте и проверьте единый вход Azure AD в MyWorkDrive с использованием тестового пользователя **Britta Simon**. Для обеспечения работы единого входа необходимо установить связь между пользователем Azure AD и соответствующим пользователем в MyWorkDrive.

Чтобы настроить и проверить единый вход Azure AD в MyWorkDrive, выполните действия в следующих стандартных блоках.

1. **[Настройка единого входа Azure AD](#configure-azure-ad-sso)** необходима, чтобы пользователи могли использовать эту функцию.
2. **[Настройка единого входа в MyWorkDrive](#configure-myworkdrive-sso)** необходима, чтобы настроить параметры единого входа на стороне приложения.
3. **[Создание тестового пользователя Azure AD](#create-an-azure-ad-test-user)** требуется для проверки работы единого входа Azure AD от имени пользователя Britta Simon.
4. **[Назначение тестового пользователя Azure AD](#assign-the-azure-ad-test-user)** необходимо, чтобы разрешить пользователю Britta Simon использовать единый вход Azure AD.
5. **[Создание тестового пользователя приложения MyWorkDrive](#create-myworkdrive-test-user)** требуется для того, чтобы в MyWorkDrive существовал пользователь Britta Simon, связанный с одноименным пользователем в Azure AD.
6. **[Проверка единого входа](#test-sso)** необходима, чтобы убедиться в корректной работе конфигурации.

### <a name="configure-azure-ad-sso"></a>Настройка единого входа Azure AD

Выполните следующие действия, чтобы включить единый вход Azure AD на портале Azure.

1. На [портале Azure](https://portal.azure.com/) на странице интеграции с приложением **MyWorkDrive** найдите раздел **Управление** и выберите **Единый вход**.
1. На странице **Выбрать метод единого входа** выберите **SAML**.
1. На странице **Настройка единого входа с помощью SAML** щелкните значок "Изменить" (значок пера), чтобы открыть диалоговое окно **Базовая конфигурация SAML** и изменить параметры.

   ![Правка базовой конфигурации SAML](common/edit-urls.png)

1. Если вы хотите настроить приложение в режиме, инициируемом **поставщиком удостоверений**, на странице **Базовая конфигурация SAML** введите значения следующих полей.

    В текстовом поле **URL-адрес ответа** введите URL-адрес в следующем формате: `https://<SERVER.DOMAIN.COM>/SAML/AssertionConsumerService.aspx`.

1. Чтобы настроить приложение для работы в режиме, инициируемом **поставщиком услуг**, щелкните **Задать дополнительные URL-адреса** и выполните следующие действия.

    В текстовом поле **URL-адрес входа** введите URL-адрес в формате `https://<SERVER.DOMAIN.COM>/Account/Login-saml`.

    > [!NOTE]
    > Эти значения приведены для примера. Измените их на фактические значения URL-адреса ответа и URL-адреса входа. Введите имя узла корпоративного сервера MyWorkDrive, например так:
    > 
    > URL-адрес ответа: `https://yourserver.yourdomain.com/SAML/AssertionConsumerService.aspx`.
    > 
    > URL-адрес для входа: `https://yourserver.yourdomain.com/Account/Login-saml`.
    > 
    > Если вы не знаете, как указать в этих параметрах имя узла и SSL-сертификат, обратитесь к [группе поддержки MyWorkDrive](mailto:support@myworkdrive.com).

1. На странице **Настройка единого входа с помощью SAML** в разделе **Сертификат подписи SAML** нажмите кнопку "Копировать", чтобы скопировать **URL-адрес метаданных федерации приложений** в буфер обмена.

    ![Ссылка для скачивания сертификата](common/copy-metadataurl.png)

### <a name="configure-myworkdrive-sso"></a>Настройка единого входа в MyWorkDrive

1. В другом окне веб-браузера войдите в MyWorkDrive с правами администратора безопасности.

2. На сервере MyWorkDrive на панели администрирования щелкните **ENTERPRISE** (Корпоративное приложение) и выполните следующие действия:

    ![Администратор](./media/myworkdrive-tutorial/tutorial_myworkdrive_admin.png)

    a. Включите **SAML/ADFS SSO** (единый вход в SAML и распределенную файловую систему Azure).

    b. Выберите **SAML – Azure AD**

    c. В текстовое поле **Azure App Federation Metadata Url** (URL-адрес метаданных федерации приложения Azure) вставьте **URL-адрес метаданных федерации приложения**, скопированный на портале Azure.

    d. Нажмите кнопку **Сохранить**

    > [!NOTE]
    > Дополнительные сведения см. в справочной статье об [интеграции MyWorkDrive с Azure AD](https://www.myworkdrive.com/support/saml-single-sign-on-azure-ad/).

### <a name="create-an-azure-ad-test-user"></a>Создание тестового пользователя Azure AD

В этом разделе описано, как создать на портале Azure тестового пользователя с именем Britta Simon.

1. На портале Azure в области слева выберите **Azure Active Directory**, **Пользователи**, а затем — **Все пользователи**.
1. В верхней части экрана выберите **Новый пользователь**.
1. В разделе **Свойства пользователя** выполните следующие действия.
   1. В поле **Имя** введите `Britta Simon`.  
   1. В поле **Имя пользователя** введите username@companydomain.extension. Например, `BrittaSimon@contoso.com`.
   1. Установите флажок **Показать пароль** и запишите значение, которое отображается в поле **Пароль**.
   1. Нажмите кнопку **Создать**.

### <a name="assign-the-azure-ad-test-user"></a>Назначение тестового пользователя Azure AD

В этом разделе описано, как разрешить пользователю Britta Simon использовать единый вход Azure, предоставив этому пользователю доступ к MyWorkDrive.

1. На портале Azure выберите **Корпоративные приложения**, а затем —**Все приложения**.
1. В списке приложений выберите **MyWorkDrive**.
1. На странице "Обзор" приложения найдите раздел **Управление** и выберите **Пользователи и группы**.

   ![Ссылка "Пользователи и группы"](common/users-groups-blade.png)

1. Выберите **Добавить пользователя**, а в диалоговом окне **Добавление назначения** выберите **Пользователи и группы**.

    ![Ссылка "Добавить пользователя"](common/add-assign-user.png)

1. В диалоговом окне **Пользователи и группы** выберите **Britta Simon** из списка пользователей, а затем в нижней части экрана нажмите кнопку **Выбрать**.
1. Если ожидается, что в утверждении SAML будет получено какое-либо значение роли, то в диалоговом окне **Выбор роли** нужно выбрать соответствующую роль для пользователя из списка и затем нажать кнопку **Выбрать**, расположенную в нижней части экрана.
1. В диалоговом окне **Добавление назначения** нажмите кнопку **Назначить**.

### <a name="create-myworkdrive-test-user"></a>Создание тестового пользователя MyWorkDrive

В этом разделе описано, как создать пользователя Britta Simon в приложении MyWorkDrive. Обратитесь в [службу поддержки MyWorkDrive](mailto:support@myworkdrive.com), чтобы добавить пользователей на платформу MyWorkDrive. Перед использованием единого входа необходимо создать и активировать пользователей.

### <a name="test-sso"></a>Проверка единого входа

Щелкнув элемент "MyWorkDrive" на Панели доступа, вы автоматически войдете в приложение MyWorkDrive, для которого настроили единый вход. См. дополнительные сведения о [панели доступа](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)

## <a name="additional-resources"></a>Дополнительные ресурсы

- [Список учебников по интеграции приложений SaaS с Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Что такое условный доступ в Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)