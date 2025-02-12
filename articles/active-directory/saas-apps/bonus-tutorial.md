---
title: Руководство по Интеграция Azure Active Directory с Bonusly | Документация Майкрософт
description: Узнайте, как настроить единый вход между Azure Active Directory и Bonusly.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 29fea32a-fa20-47b2-9e24-26feb47b0ae6
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 04/14/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: f605398896c93d15b7475c05c79361fdcefcfcab
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/28/2019
ms.locfileid: "67442773"
---
# <a name="tutorial-azure-active-directory-integration-with-bonusly"></a>Руководство по Интеграция Azure Active Directory с Bonusly

В этом руководстве описано, как интегрировать Bonusly с Azure Active Directory (Azure AD).
Интеграция Azure AD с приложением Bonusly обеспечивает следующие преимущества:

* С помощью Azure AD вы можете контролировать доступ к приложению Bonusly.
* Можно включить автоматический вход пользователей в Bonusly (единый вход) с помощью учетных записей Azure AD.
* Вы можете управлять учетными записями централизованно на портале Azure.

Дополнительные сведения об интеграции приложений SaaS с Azure AD см. в статье [Единый вход в приложениях в Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Если у вас еще нет подписки Azure, [создайте бесплатную учетную запись Azure](https://azure.microsoft.com/free/), прежде чем начинать работу.

## <a name="prerequisites"></a>Предварительные требования

Чтобы настроить интеграцию Azure AD с приложением Bonusly, вам потребуется:

* подписка Azure AD (если у вас нет среды Azure AD, вы можете получить [бесплатную учетную запись](https://azure.microsoft.com/free/));
* подписка Bonusly с поддержкой единого входа.

## <a name="scenario-description"></a>Описание сценария

В рамках этого руководства вы настроите и проверите единый вход Azure AD в тестовой среде.

* Bonusly поддерживает единый вход, инициируемый **поставщиком удостоверений**.

## <a name="adding-bonusly-from-the-gallery"></a>Добавление Bonusly из коллекции

Чтобы настроить интеграцию Bonusly с Azure AD, необходимо добавить Bonusly из коллекции в список управляемых приложений SaaS.

**Чтобы добавить Bonusly из коллекции, сделайте следующее:**

1. На **[портале Azure](https://portal.azure.com)** в области навигации слева щелкните значок **Azure Active Directory**.

    ![Кнопка Azure Active Directory](common/select-azuread.png)

2. Перейдите в колонку **Корпоративные приложения** и выберите **Все приложения**.

    ![Колонка "Корпоративные приложения"](common/enterprise-applications.png)

3. Чтобы добавить новое приложение, в верхней части диалогового окна нажмите кнопку **Создать приложение**.

    ![Кнопка "Создать приложение"](common/add-new-app.png)

4. В поле поиска введите **Bonusly**, выберите **Bonusly** на панели результатов и нажмите кнопку **Добавить**, чтобы добавить это приложение.

    ![Bonusly в списке результатов](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Настройка и проверка единого входа в Azure AD

В этом разделе описана настройка и проверка единого входа Azure AD в Bonusly с использованием тестового пользователя **Britta Simon**.
Для обеспечения работы единого входа необходимо установить связь между пользователем Azure AD и соответствующим пользователем в Bonusly.

Чтобы настроить и проверить единый вход Azure AD в Bonusly, выполните следующие стандартные действия.

1. **[Настройка единого входа Azure AD](#configure-azure-ad-single-sign-on)** необходима, чтобы пользователи могли использовать эту функцию.
2. **[Настройка единого входа в Bonusly](#configure-bonusly-single-sign-on)** необходима, чтобы настроить параметры единого входа на стороне приложения.
3. **[Создание тестового пользователя Azure AD](#create-an-azure-ad-test-user)** требуется для проверки работы единого входа Azure AD от имени пользователя Britta Simon.
4. **[Назначение тестового пользователя Azure AD](#assign-the-azure-ad-test-user)** необходимо, чтобы разрешить пользователю Britta Simon использовать единый вход Azure AD.
5. **[Создание тестового пользователя Bonusly](#create-bonusly-test-user)** требуется для того, чтобы в Bonusly существовал пользователь Britta Simon, связанный с одноименным пользователем в Azure AD.
6. **[Проверка единого входа](#test-single-sign-on)** необходима, чтобы проверить работу конфигурации.

### <a name="configure-azure-ad-single-sign-on"></a>Настройка единого входа Azure AD

В этом разделе описано включение единого входа Azure AD на портале Azure.

Чтобы настроить единый вход Azure AD в Bonusly, сделайте следующее.

1. На [портале Azure](https://portal.azure.com/) на странице интеграции с приложением **Bonusly** выберите **Единый вход**.

    ![Ссылка "Настройка единого входа"](common/select-sso.png)

2. В диалоговом окне **Выбрать метод единого входа** выберите режим **SAML/WS-Fed**, чтобы включить единый вход.

    ![Режим выбора единого входа](common/select-saml-option.png)

3. На странице **Настройка единого входа с помощью SAML** щелкните **Изменить**, чтобы открыть диалоговое окно **Базовая конфигурация SAML**.

    ![Правка базовой конфигурации SAML](common/edit-urls.png)

4. В разделе **Базовая конфигурация SAML** выполните приведенные ниже действия.

    ![Сведения о домене и URL-адресах для единого входа в приложение Bonusly](common/idp-reply.png)

    В текстовом поле **URL-адрес ответа** введите URL-адрес в следующем формате: `https://Bonus.ly/saml/<tenant-name>`.

    > [!NOTE]
    > Это значение приведено для примера. Вместо него нужно указать фактический URL-адрес ответа. Чтобы получить это значение, обратитесь к [группе поддержки клиентов Bonusly](https://bonus.ly/contact). Можно также посмотреть шаблоны в разделе **Базовая конфигурация SAML** на портале Azure.

5. В разделе **Сертификат подписи SAML** щелкните кнопку **Правка**, чтобы открыть диалоговое окно **Сертификат подписи SAML**.

    ![Изменение сертификата подписи SAML](common/edit-certificate.png)

6. В разделе **Сертификат подписи SAML** скопируйте значение **Отпечаток** и сохраните его на компьютере.

    ![Копирование значения "Отпечаток"](common/copy-thumbprint.png)

7. Требуемые URL-адреса вы можете скопировать из раздела **Настройка Bonusly**.

    ![Копирование URL-адресов настройки](common/copy-configuration-urls.png)

    а) URL-адрес входа.

    b. Идентификатор Azure AD

    c. URL-адрес выхода.

### <a name="configure-bonusly-single-sign-on"></a>Настройка единого входа в Bonusly

1. В другом окне браузера войдите в клиент **Bonusly**.

1. На панели инструментов в верхней части экрана щелкните **Settings** (Параметры) и выберите **Integrations and apps** (Интеграции и приложения).

    ![Социальный раздел Bonusly](./media/bonus-tutorial/ic773686.png "Bonusly")
1. В разделе **Single Sign-On** (Единый вход) выберите **SAML**.

1. На диалоговой странице **SAML** выполните следующие действия.

    ![Диалоговое окно SAML в Bonusly](./media/bonus-tutorial/ic773687.png "Bonusly")

    a. В текстовое поле **IdP SSO target URL** (Целевой URL-адрес единого входа IdP) вставьте значение **URL-адрес входа**, скопированное на портале Azure.

    b. В текстовое поле **IdP Login URL** (URL-адрес для входа IdP) вставьте значение **URL-адрес входа**, скопированное на портале Azure.

    c. В текстовое поле **IdP Issuer** (Издатель IdP) вставьте значение **Идентификатор Azure AD**, скопированное на портале Azure.
    
    d. Вставьте значение **Отпечаток** с портала Azure в текстовое поле **Cert Fingerprint** (Отпечаток сертификата).
    
    д. Выберите команду **Сохранить**.

### <a name="create-an-azure-ad-test-user"></a>Создание тестового пользователя Azure AD

Цель этого раздела — создать на портале Azure тестового пользователя с именем Britta Simon.

1. На портале Azure в области слева выберите **Azure Active Directory**, **Пользователи**, а затем — **Все пользователи**.

    ![Ссылки "Пользователи и группы" и "Все пользователи"](common/users.png)

2. В верхней части экрана выберите **Новый пользователь**.

    ![Кнопка "Новый пользователь"](common/new-user.png)

3. В разделе свойств пользователя сделайте следующее:

    ![Диалоговое окно "Пользователь"](common/user-properties.png)

    а. В поле **Имя** введите **BrittaSimon**.
  
    b. В поле **Имя пользователя** введите `brittasimon@yourcompanydomain.extension`. Например BrittaSimon@contoso.com.

    c. Установите флажок **Показать пароль** и запишите значение, которое отображается в поле "Пароль".

    d. Нажмите кнопку **Создать**.

### <a name="assign-the-azure-ad-test-user"></a>Назначение тестового пользователя Azure AD

В этом разделе мы разрешим пользователю Britta Simon использовать единый вход Azure, предоставив этому пользователю доступ к Bonusly.

1. На портале Azure выберите **Корпоративные приложения**, **Все приложения**, а затем — **Bonusly**.

    ![Колонка "Корпоративные приложения"](common/enterprise-applications.png)

2. В списке приложений выберите **Bonusly**.

    ![Ссылка на Bonusly в списке "Приложения"](common/all-applications.png)

3. В меню слева выберите **Пользователи и группы**.

    ![Ссылка "Пользователи и группы"](common/users-groups-blade.png)

4. Нажмите кнопку **Добавить пользователя**, а затем в диалоговом окне **Добавление назначения** выберите **Пользователи и группы**.

    ![Область "Добавление назначения"](common/add-assign-user.png)

5. В диалоговом окне **Пользователи и группы** из списка пользователей выберите **Britta Simon**, а затем в верхней части экрана нажмите кнопку **Выбрать**.

6. Если ожидается, что в утверждении SAML будет получено какое-либо значение роли, то в диалоговом окне **Выбор ролей** нужно выбрать соответствующую роль для пользователя из списка и затем нажать кнопку **Выбрать**, расположенную в нижней части экрана.

7. В диалоговом окне **Добавление назначения** нажмите кнопку **Назначить**.

### <a name="create-bonusly-test-user"></a>Создание тестового пользователя приложения Bonusly

Чтобы пользователи Azure AD могли входить в приложение Bonusly, они должны быть подготовлены в этом приложении. В случае с Bonusly подготовка выполняется вручную.

> [!NOTE]
> Для подготовки учетных записей пользователей AAD можно использовать любые другие средства создания учетных записей пользователей Bonusly или API, предоставляемые разработчиками Bonusly. 

**Чтобы настроить подготовку учетных записей пользователей, выполните следующие действия.**

1. В окне веб-браузера войдите в клиент Bonusly.

1. Щелкните **Параметры**.

    ![Параметры](./media/bonus-tutorial/ic781041.png "Параметры")

1. Откройте вкладку **Users and bonuses** (Пользователи и бонусы).

    ![Пользователи и бонусы](./media/bonus-tutorial/ic781042.png "Пользователи и бонусы")

1. Выберите **Manage Users**(Управление пользователями).

    ![Управление пользователями](./media/bonus-tutorial/ic781043.png "Управление пользователями")

1. Нажмите кнопку **Add User**(Добавить пользователя).

    ![Добавление пользователя](./media/bonus-tutorial/ic781044.png "Добавление пользователя")

1. На странице **Добавление пользователя** выполните следующие действия.

    ![Добавление пользователя](./media/bonus-tutorial/ic781045.png "Добавление пользователя")  

    a. В текстовое поле **First name** (Имя) введите имя пользователя, например **Britta**.

    b. В текстовое поле **Last name** (Фамилия) введите фамилию пользователя, например **Simon**.

    c. В текстовое поле **Email** (Электронная почта) введите адрес электронной почты пользователя, например `brittasimon\@contoso.com`.

    d. Выберите команду **Сохранить**.

    > [!NOTE]
    > Владелец учетной записи Azure AD получит сообщение электронной почты со ссылкой для подтверждения учетной записи перед ее активацией.  

### <a name="test-single-sign-on"></a>Проверка единого входа

В этом разделе описано, как проверить конфигурацию единого входа Azure AD с помощью панели доступа.

Щелкнув элемент "Bonusly" на Панели доступа, вы автоматически войдете в приложение Bonusly, для которого настроили единый вход. См. дополнительные сведения о [панели доступа](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)

## <a name="additional-resources"></a>Дополнительные ресурсы

- [Список учебников по интеграции приложений SaaS с Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Что такое условный доступ в Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
