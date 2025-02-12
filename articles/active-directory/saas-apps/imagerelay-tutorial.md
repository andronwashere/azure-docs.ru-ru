---
title: Руководство по Интеграция Azure Active Directory с Image Relay | Документация Майкрософт
description: Сведения о настройке единого входа Azure Active Directory в Image Relay.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 65bb5990-07ef-4244-9f41-cd28fc2cb5a2
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 02/20/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 37b7fe05f306f08d83de0832c5b1dd814f5560ed
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "67100546"
---
# <a name="tutorial-azure-active-directory-integration-with-image-relay"></a>Руководство по Интеграция Azure Active Directory с Image Relay

В этом учебнике описано, как интегрировать Image Relay с Azure Active Directory (Azure AD).
Интеграция Image Relay с Azure AD обеспечивает следующие преимущества.

* С помощью Azure AD вы можете контролировать доступ к Image Relay.
* Вы можете включить автоматический вход пользователей (единый вход) в Image Relay с помощью учетной записи Azure AD.
* Вы можете управлять учетными записями централизованно на портале Azure.

Дополнительные сведения об интеграции приложений SaaS с Azure AD см. в статье [Единый вход в приложениях в Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Если у вас еще нет подписки Azure, [создайте бесплатную учетную запись Azure](https://azure.microsoft.com/free/), прежде чем начинать работу.

## <a name="prerequisites"></a>Предварительные требования

Чтобы настроить интеграцию Azure AD с Image Relay, вам потребуется:

* подписка Azure AD (если у вас нет среды Azure AD, вы можете получить пробную версию на один месяц по [этой ссылке](https://azure.microsoft.com/pricing/free-trial/));
* подписка Image Relay с поддержкой единого входа.

## <a name="scenario-description"></a>Описание сценария

В рамках этого руководства вы настроите и проверите единый вход Azure AD в тестовой среде.

* Image Relay поддерживает единый вход, инициированный **поставщиком услуг**.

## <a name="adding-image-relay-from-the-gallery"></a>Добавление Image Relay из коллекции.

Чтобы настроить интеграцию Image Relay с Azure AD, необходимо добавить Image Relay из коллекции в список управляемых приложений SaaS.

**Чтобы добавить Image Relay из коллекции, выполните следующие действия:**

1. На **[портале Azure](https://portal.azure.com)** в области навигации слева щелкните значок **Azure Active Directory**.

    ![Кнопка Azure Active Directory](common/select-azuread.png)

2. Перейдите в колонку **Корпоративные приложения** и выберите **Все приложения**.

    ![Колонка "Корпоративные приложения"](common/enterprise-applications.png)

3. Чтобы добавить новое приложение, в верхней части диалогового окна нажмите кнопку **Создать приложение**.

    ![Кнопка "Создать приложение"](common/add-new-app.png)

4. В поле поиска введите **Image Relay**, выберите **Image Relay** на панели результатов и нажмите кнопку **Добавить**, чтобы добавить это приложение.

    ![Image Relay в списке результатов поиска](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Настройка и проверка единого входа в Azure AD

В этом разделе описана настройка и проверка единого входа Azure AD в приложение Image Relay для тестового пользователя **Britta Simon**.
Для обеспечения работы единого входа необходимо установить связь между пользователем Azure AD и соответствующим пользователем в Image Relay.

Чтобы настроить и проверить единый вход Azure AD в Image Relay, вам потребуется выполнить действия в следующих стандартных блоках.

1. **[Настройка единого входа Azure AD](#configure-azure-ad-single-sign-on)** необходима, чтобы пользователи могли использовать эту функцию.
2. **[Настройка единого входа в Image Relay](#configure-image-relay-single-sign-on)** необходима, чтобы настроить параметры единого входа на стороне приложения.
3. **[Создание тестового пользователя Azure AD](#create-an-azure-ad-test-user)** требуется для проверки работы единого входа Azure AD от имени пользователя Britta Simon.
4. **[Назначение тестового пользователя Azure AD](#assign-the-azure-ad-test-user)** необходимо, чтобы разрешить пользователю Britta Simon использовать единый вход Azure AD.
5. **[Создание тестового пользователя Image Relay](#create-image-relay-test-user)** требуется, чтобы в Image Relay существовал пользователь Britta Simon, связанный с представлением этого пользователя в Azure AD.
6. **[Проверка единого входа](#test-single-sign-on)** необходима, чтобы проверить работу конфигурации.

### <a name="configure-azure-ad-single-sign-on"></a>Настройка единого входа Azure AD

В этом разделе описано включение единого входа Azure AD на портале Azure.

Чтобы настроить единый вход Azure AD в Image Relay, выполните следующие действия:

1. На [портале Azure](https://portal.azure.com/) на странице интеграции с приложением **Image Relay** выберите **Единый вход**.

    ![Ссылка "Настройка единого входа"](common/select-sso.png)

2. В диалоговом окне **Выбрать метод единого входа** выберите режим **SAML/WS-Fed**, чтобы включить единый вход.

    ![Режим выбора единого входа](common/select-saml-option.png)

3. На странице **Настройка единого входа с помощью SAML** щелкните **Изменить**, чтобы открыть диалоговое окно **Базовая конфигурация SAML**.

    ![Правка базовой конфигурации SAML](common/edit-urls.png)

4. В разделе **Базовая конфигурация SAML** выполните приведенные ниже действия.

    ![Сведения о домене и URL-адресах единого входа для приложения Image Relay](common/sp-identifier.png)

    a. В текстовом поле **URL-адрес для входа** введите URL-адрес в следующем формате: `https://<companyname>.imagerelay.com/`.

    b. В текстовом поле **Идентификатор (сущности)** введите URL-адрес в следующем формате: `https://<companyname>.imagerelay.com/sso/metadata`.

    > [!NOTE]
    > Эти значения приведены для примера. Необходимо обновить эти значения действующим URL-адресом для входа и идентификатором. Чтобы получить эти значения, обратитесь в [службу поддержки клиентов Image Relay](http://support.imagerelay.com/). Можно также посмотреть шаблоны в разделе **Базовая конфигурация SAML** на портале Azure.

4. На странице **Настройка единого входа с помощью SAML** в разделе **Сертификат подписи SAML** щелкните **Загрузить**, чтобы загрузить требуемый **сертификат (Base64)** из предложенных вариантов, и сохраните его на компьютере.

    ![Ссылка для скачивания сертификата](common/certificatebase64.png)

6. Скопируйте требуемый URL-адрес из раздела **Настройка Image Relay**.

    ![Копирование URL-адресов настройки](common/copy-configuration-urls.png)

    а) URL-адрес входа.

    б) Идентификатор Azure AD.

    в) URL-адрес выхода.

### <a name="configure-image-relay-single-sign-on"></a>Настройка единого входа в Image Relay

1. В другом окне браузера войдите на сайт компании Image Relay с правами администратора.

2. На панели инструментов вверху экрана выберите рабочую нагрузку **Users & Permissions** (Пользователи и разрешения).

    ![Настройка единого входа](./media/imagerelay-tutorial/tutorial_imagerelay_06.png) 

3. Щелкните **Создать новое разрешение**.

    ![Настройка единого входа](./media/imagerelay-tutorial/tutorial_imagerelay_08.png)

4. В рабочей нагрузке **Single Sign On Settings** (Параметры единого входа) установите флажок **This Group can only sign-in via Single Sign On** (Эта группа может входить только через единый вход) и нажмите кнопку **Сохранить**.

    ![Настройка единого входа](./media/imagerelay-tutorial/tutorial_imagerelay_09.png) 

5. Откройте **Параметры учетной записи**.

    ![Настройка единого входа](./media/imagerelay-tutorial/tutorial_imagerelay_10.png) 

6. Выберите рабочую нагрузку **Параметры единого входа** .

    ![Настройка единого входа](./media/imagerelay-tutorial/tutorial_imagerelay_11.png)

7. В диалоговом окне **SAML Settings** (Параметры SAML) выполните следующие действия.

    ![Настройка единого входа](./media/imagerelay-tutorial/tutorial_imagerelay_12.png)

    a. В текстовое поле **Login URL** (URL-адрес входа) вставьте значение **URL-адреса входа**, скопированное с портала Azure.

    b. В текстовое поле **Logout URL** (URL-адрес выхода) вставьте значение **URL-адреса выхода**, скопированное с портала Azure.

    c. В поле **Name Id Format** (Формат ИД имени) выберите **urn:oasis:names:tc:SAML:1.1:nameid-format:emailAddress**.

    d. В разделе **Binding Options for Requests from the Service Provider (Image Relay)** (Обязательные параметры для запросов от поставщика услуг (Image Relay)) выберите **POST Binding** (Привязка POST).

    д. В разделе **Сертификат X.509** щелкните **Обновить сертификат**.

    ![Настройка единого входа](./media/imagerelay-tutorial/tutorial_imagerelay_17.png)

    Е. Откройте скачанный сертификат в блокноте, а затем скопируйте и вставьте его содержимое в текстовое поле **x.509 Certificate** (Сертификат X.509).

    ![Настройка единого входа](./media/imagerelay-tutorial/tutorial_imagerelay_18.png)

    ж. В разделе **Just-In-Time User Provisioning** (JIT-подготовка пользователей) выберите **Enable Just-In-Time User Provisioning** (Включить JIT-подготовку пользователей).

    ![Настройка единого входа](./media/imagerelay-tutorial/tutorial_imagerelay_19.png)

    h. Выберите группу разрешений (например, **SSO Basic**(Базовый единый вход)), которой будет разрешено выполнять вход только через службу единого входа.

    ![Настройка единого входа](./media/imagerelay-tutorial/tutorial_imagerelay_20.png)

    i. Выберите команду **Сохранить**.

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

В этом разделе описано, как включить единый вход Azure для пользователя Britta Simon, предоставив этому пользователю доступ к Image Relay.

1. На портале Azure выберите **Корпоративные приложения**, **Все приложения**, а затем — **Image Relay**.

    ![Колонка "Корпоративные приложения"](common/enterprise-applications.png)

2. В списке приложений выберите **Image Relay**.

    ![Ссылка на Image Relay в списке "Приложения"](common/all-applications.png)

3. В меню слева выберите **Пользователи и группы**.

    ![Ссылка "Пользователи и группы"](common/users-groups-blade.png)

4. Нажмите кнопку **Добавить пользователя**, а затем в диалоговом окне **Добавление назначения** выберите **Пользователи и группы**.

    ![Область "Добавление назначения"](common/add-assign-user.png)

5. В диалоговом окне **Пользователи и группы** из списка пользователей выберите **Britta Simon**, а затем в верхней части экрана нажмите кнопку **Выбрать**.

6. Если ожидается, что в утверждении SAML будет получено какое-либо значение роли, то в диалоговом окне **Выбор ролей** нужно выбрать соответствующую роль для пользователя из списка и затем нажать кнопку **Выбрать**, расположенную в нижней части экрана.

7. В диалоговом окне **Добавление назначения** нажмите кнопку **Назначить**.

### <a name="create-image-relay-test-user"></a>Создание тестового пользователя в Image Relay

Цель этого раздела — создать пользователя с именем Britta Simon в Image Relay.

**Чтобы создать пользователя с именем Britta Simon в Image Relay, выполните следующие действия:**

1. Войдите на cайт компании Image Relay в качестве администратора.

2. Откройте раздел **Users & Permissions** (Пользователи и разрешения) и выберите параметр **Create SSO User** (Создать пользователя единого входа).

    ![Настройка единого входа](./media/imagerelay-tutorial/tutorial_imagerelay_21.png) 

3. Введите **Адрес электронной почты**, **Имя**, **Фамилию** и **Организацию** пользователя, которого нужно подготовить, и выберите группу разрешений (например, "Базовый единый вход"). Эта группа сможет выполнять вход только с помощью единого входа.

    ![Настройка единого входа](./media/imagerelay-tutorial/tutorial_imagerelay_22.png)

4. Нажмите кнопку **Создать**.

### <a name="test-single-sign-on"></a>Проверка единого входа

В этом разделе описано, как проверить конфигурацию единого входа Azure AD с помощью панели доступа.

Щелкнув плитку Image Relay на Панели доступа, вы автоматически войдете в приложение Image Relay, для которого настроили единый вход. См. дополнительные сведения о [панели доступа](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)

## <a name="additional-resources"></a>Дополнительные ресурсы

- [Список учебников по интеграции приложений SaaS с Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Что такое условный доступ в Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)