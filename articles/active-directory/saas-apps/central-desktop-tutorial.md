---
title: Руководство по Интеграция Azure Active Directory с Central Desktop | Документация Майкрософт
description: Сведения о настройке единого входа между Azure Active Directory и Central Desktop.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: b805d485-93db-49b4-807a-18d446c7090e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 02/12/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 58fcd21c7e6e3f2336d3e809584451dc38f6276b
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "67105742"
---
# <a name="tutorial-azure-active-directory-integration-with-central-desktop"></a>Руководство по Интеграция Azure Active Directory с Central Desktop

В этом руководстве описано, как интегрировать Central Desktop с Azure Active Directory (Azure AD).
Интеграция Azure AD с приложением Central Desktop обеспечивает следующие преимущества:

* С помощью Azure AD вы можете контролировать доступ к Central Desktop.
* Вы можете включить автоматический вход пользователей в Central Desktop (единый вход) с помощью учетных записей Azure AD.
* Вы можете управлять учетными записями централизованно на портале Azure.

Дополнительные сведения об интеграции приложений SaaS с Azure AD см. в статье [Единый вход в приложениях в Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Если у вас еще нет подписки Azure, [создайте бесплатную учетную запись Azure](https://azure.microsoft.com/free/), прежде чем начинать работу.

## <a name="prerequisites"></a>Предварительные требования

Чтобы настроить интеграцию Azure AD с Central Desktop, вам потребуется:

* подписка Azure AD (если у вас нет среды Azure AD, вы можете получить пробную версию на один месяц по [этой ссылке](https://azure.microsoft.com/pricing/free-trial/));
* подписка на Central Desktop с поддержкой единого входа.

## <a name="scenario-description"></a>Описание сценария

В рамках этого руководства вы настроите и проверите единый вход Azure AD в тестовой среде.

* Central Desktop поддерживает единый вход, инициированный **поставщиком услуг**.

## <a name="adding-central-desktop-from-the-gallery"></a>Добавление Central Desktop из коллекции.

Чтобы настроить интеграцию Central Desktop с Azure AD, необходимо добавить Central Desktop из коллекции в список управляемых приложений SaaS.

**Чтобы добавить Central Desktop из коллекции, выполните следующие действия:**

1. На **[портале Azure](https://portal.azure.com)** в области навигации слева щелкните значок **Azure Active Directory**.

    ![Кнопка Azure Active Directory](common/select-azuread.png)

2. Перейдите в колонку **Корпоративные приложения** и выберите **Все приложения**.

    ![Колонка "Корпоративные приложения"](common/enterprise-applications.png)

3. Чтобы добавить новое приложение, в верхней части диалогового окна нажмите кнопку **Создать приложение**.

    ![Кнопка "Создать приложение"](common/add-new-app.png)

4. В поле поиска введите **Central Desktop**, выберите **Central Desktop** на панели результатов и нажмите кнопку **Добавить**, чтобы добавить это приложение.

     ![Central Desktop в списке результатов](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Настройка и проверка единого входа в Azure AD

В этом разделе описана настройка и проверка единого входа Azure AD в Central Desktop с использованием тестового пользователя **Britta Simon**.
Чтобы обеспечить работу единого входа, нужно установить связь между пользователем Azure AD и соответствующим пользователем в Central Desktop.

Чтобы настроить и проверить единый вход Azure AD в Central Desktop, вам потребуется выполнить действия в следующих стандартных блоках:

1. **[Настройка единого входа Azure AD](#configure-azure-ad-single-sign-on)** необходима, чтобы пользователи могли использовать эту функцию.
2. **[Настройка единого входа в Central Desktop](#configure-central-desktop-single-sign-on)** требуется для того, чтобы настроить параметры единого входа на стороне приложения.
3. **[Создание тестового пользователя Azure AD](#create-an-azure-ad-test-user)** требуется для проверки работы единого входа Azure AD от имени пользователя Britta Simon.
4. **[Назначение тестового пользователя Azure AD](#assign-the-azure-ad-test-user)** необходимо, чтобы разрешить пользователю Britta Simon использовать единый вход Azure AD.
5. **[Создание тестового пользователя приложения Central Desktop](#create-central-desktop-test-user)** требуется для того, чтобы в Central Desktop существовал пользователь Britta Simon, связанный с одноименным пользователем в Azure AD.
6. **[Проверка единого входа](#test-single-sign-on)** необходима, чтобы проверить работу конфигурации.

### <a name="configure-azure-ad-single-sign-on"></a>Настройка единого входа Azure AD

В этом разделе описано включение единого входа Azure AD на портале Azure.

Чтобы настроить единый вход Azure AD в Central Desktop, выполните следующие действия:

1. На [портале Azure](https://portal.azure.com/) на странице интеграции с приложением **Central Desktop** выберите **Единый вход**.

    ![Ссылка "Настройка единого входа"](common/select-sso.png)

2. В диалоговом окне **Выбрать метод единого входа** выберите режим **SAML/WS-Fed**, чтобы включить единый вход.

    ![Режим выбора единого входа](common/select-saml-option.png)

3. На странице **Настройка единого входа с помощью SAML** щелкните **Изменить**, чтобы открыть диалоговое окно **Базовая конфигурация SAML**.

    ![Правка базовой конфигурации SAML](common/edit-urls.png)

4. В разделе **Базовая конфигурация SAML** выполните приведенные ниже действия.

    ![Сведения о домене и URL-адресах единого входа для приложения Central Desktop](common/sp-identifier-reply.png)

    a. В текстовое поле **URL-адрес для входа** введите URL-адрес в следующем формате: `https://<companyname>.centraldesktop.com`.

    b. В поле **Идентификатор** введите URL-адрес в следующем формате:
    
    | |
    |--|
    | `https://<companyname>.centraldesktop.com/saml2-metadata.php`|
    | `https://<companyname>.imeetcentral.com/saml2-metadata.php`|
    | |

    c. В текстовом поле **URL-адрес ответа** введите URL-адрес в формате `https://<companyname>.centraldesktop.com/saml2-assertion.php`.

    > [!NOTE]
    > Эти значения приведены для примера. Укажите вместо них фактические значения URL-адреса для входа, идентификатора и URL-адреса ответа. Чтобы получить их, обратитесь в [службу поддержки клиентов Central Desktop](https://imeetcentral.com/contact-us). Можно также посмотреть шаблоны в разделе **Базовая конфигурация SAML** на портале Azure.

5. На странице **Настройка единого входа с помощью SAML** в разделе **Сертификат подписи SAML** щелкните **Загрузить**, чтобы загрузить требуемый **сертификат (необработанный)** из предложенных вариантов и сохранить его на компьютере.

    ![Ссылка для скачивания сертификата](common/certificateraw.png)

6. В разделе **Set up Central Desktop** (Настройка Central Desktop) скопируйте требуемый URL-адрес.

    ![Копирование URL-адресов настройки](common/copy-configuration-urls.png)

    а) URL-адрес входа.

    б) Идентификатор Azure AD.

    в) URL-адрес выхода.

### <a name="configure-central-desktop-single-sign-on"></a>Настройка единого входа в Central Desktop

1. Войдите в клиент **Central Desktop**.

2. Перейдите в меню **Параметры**. Выберите **Advanced** (Дополнительно), а затем **Single Sign On** (Единый вход).

    ![Настройка — Дополнительно](./media/central-desktop-tutorial/ic769563.png "Настройка — Дополнительно")

3. На странице **Single Sign On Settings** (Параметры единого входа) сделайте следующее:

    ![Параметры единого входа](./media/central-desktop-tutorial/ic769564.png "Параметры единого входа")

    a. Установите флажок **Разрешить единый вход SAML версии 2**.

    b. В поле **SSO URL** (URL-адрес единого входа) вставьте значение **идентификатора Azure AD**, скопированное на портале Azure.

    c. В поле **SSO Login URL** (URL-адрес для единого входа) вставьте **URL-адрес входа**, скопированный на портале Azure.

    d. В поле **SSO Logout URL** (URL-адрес для единого выхода) вставьте **URL-адрес выхода**, скопированный на портале Azure.

4. В разделе **Message Signature Verification Method** (Метод проверки подписей в сообщениях) сделайте следующее:

    ![Метод проверки подписей в сообщениях](./media/central-desktop-tutorial/ic769565.png "Message Signature Verification Method")
    
    a. Выберите **Сертификат**.

    b. В списке **SSO Certificate** (Сертификат единого входа) выберите значение **RSH SHA256**.

    c. Откройте скачанный сертификат в Блокноте. Затем скопируйте содержимое сертификата и вставьте его в поле **SSO Certificate** (Сертификат единого входа).

    d. Установите флажок **Отображать ссылку на страницу входа SAML версии 2**.

    д. Нажмите кнопку **Обновить**.

### <a name="create-an-azure-ad-test-user"></a>Создание тестового пользователя Azure AD

Цель этого раздела — создать на портале Azure тестового пользователя с именем Britta Simon.

1. На портале Azure в области слева выберите **Azure Active Directory**, **Пользователи**, а затем — **Все пользователи**.

    ![Ссылки "Пользователи и группы" и "Все пользователи"](common/users.png)

2. В верхней части экрана выберите **Новый пользователь**.

    ![Кнопка "Новый пользователь"](common/new-user.png)

3. В разделе свойств пользователя сделайте следующее:

    ![Диалоговое окно "Пользователь"](common/user-properties.png)

    а. В поле **Имя** введите **BrittaSimon**.
  
    b. В поле **Имя пользователя** введите **brittasimon\@домен_вашей_компании.доменная_зона**.  
    Например BrittaSimon@contoso.com.

    c. Установите флажок **Показать пароль** и запишите значение, которое отображается в поле "Пароль".

    d. Нажмите кнопку **Создать**.

### <a name="assign-the-azure-ad-test-user"></a>Назначение тестового пользователя Azure AD

В этом разделе описано, как разрешить пользователю Britta Simon использовать единый вход Azure, предоставив этому пользователю доступ к Central Desktop.

1. На портале Azure выберите **Корпоративные приложения**, **Все приложения** и **Central Desktop**.

    ![Колонка "Корпоративные приложения"](common/enterprise-applications.png)

2. В списке приложений выберите **Central Desktop**.

    ![Ссылка на Central Desktop в списке приложений](common/all-applications.png)

3. В меню слева выберите **Пользователи и группы**.

    ![Ссылка "Пользователи и группы"](common/users-groups-blade.png)

4. Нажмите кнопку **Добавить пользователя**, а затем в диалоговом окне **Добавление назначения** выберите **Пользователи и группы**.

    ![Область "Добавление назначения"](common/add-assign-user.png)

5. В диалоговом окне **Пользователи и группы** из списка пользователей выберите **Britta Simon**, а затем в верхней части экрана нажмите кнопку **Выбрать**.

6. Если ожидается, что в утверждении SAML будет получено какое-либо значение роли, то в диалоговом окне **Выбор ролей** нужно выбрать соответствующую роль для пользователя из списка и затем нажать кнопку **Выбрать**, расположенную в нижней части экрана.

7. В диалоговом окне **Добавление назначения** нажмите кнопку **Назначить**.

### <a name="create-central-desktop-test-user"></a>Создание тестового пользователя приложения Central Desktop

Чтобы пользователи Azure AD могли входить систему, их необходимо подготовить в Central Desktop. В этом разделе описывается порядок создания учетных записей пользователей Azure AD в Central Desktop.

> [!NOTE]
> Для подготовки учетных записей пользователя Azure AD вы можете использовать любые другие средства создания учетной записи пользователя Central Desktop или API, предоставляемые Central Desktop.

**Чтобы подготовить учетные записи пользователей для Central Desktop, сделайте следующее:**

1. Войдите в клиент Central Desktop.

2. Выберите **People** (Люди) и **Add Internal Members** (Добавить внутренних участников).

    ![Люди](./media/central-desktop-tutorial/ic781051.png "Люди")

3. В поле **Email Address of New Members** (Адреса электронной почты новых участников) введите учетную запись Azure AD, которую вы хотите подготовить, а затем нажмите кнопку **Далее**.

    ![Адреса электронной почты новых участников](./media/central-desktop-tutorial/ic781052.png "Адреса электронной почты новых участников")

4. Выберите **Add Internal members** (Добавить внутренних участников).

    ![Добавление внутренних участников](./media/central-desktop-tutorial/ic781053.png "Добавление внутренних участников")
  
   > [!NOTE]
   > Пользователи, которые были добавлены, получат сообщение электронной почты, содержащее ссылку для активации учетной записи.

### <a name="test-single-sign-on"></a>Проверка единого входа

В этом разделе описано, как проверить конфигурацию единого входа Azure AD с помощью панели доступа.

Щелкнув плитку Central Desktop на Панели доступа, вы автоматически войдете в приложение Central Desktop, для которого настроили единый вход. См. дополнительные сведения о [панели доступа](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)

## <a name="additional-resources"></a>Дополнительные ресурсы

- [Список учебников по интеграции приложений SaaS с Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Что такое условный доступ в Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
