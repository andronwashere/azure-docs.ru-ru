---
title: Руководство. Интеграция Azure Active Directory с InstaVR Viewer | Документация Майкрософт
description: Узнайте, как настроить единый вход для Azure Active Directory и InstaVR Viewer.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: 13ffa29f-d0a5-4b21-b296-cfd76f380940
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 01/17/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 0e005d794fa3c6c70edf3d3466b0f076e2007352
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "67099974"
---
# <a name="tutorial-azure-active-directory-integration-with-instavr-viewer"></a>Руководство. Интеграция Azure Active Directory с InstaVR Viewer

В этом руководстве описано, как интегрировать InstaVR Viewer с Azure Active Directory (Azure AD).
Интеграция InstaVR Viewer с Azure Active Directory обеспечивает следующие преимущества:

* С помощью Azure Active Directory вы можете контролировать доступ к InstaVR Viewer.
* Вы можете включить автоматический вход пользователей в InstaVR Viewer (единый вход) через учетную запись Azure Active Directory.
* Вы можете управлять учетными записями централизованно на портале Azure.

Дополнительные сведения об интеграции приложений SaaS с Azure AD см. в статье [Единый вход в приложениях в Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Если у вас еще нет подписки Azure, [создайте бесплатную учетную запись Azure](https://azure.microsoft.com/free/), прежде чем начинать работу.

## <a name="prerequisites"></a>Предварительные требования

Чтобы настроить интеграцию Azure Active Directory с InstaVR Viewer, вам потребуется:

* подписка Azure AD (если у вас нет среды Azure AD, вы можете получить пробную версию на один месяц по [этой ссылке](https://azure.microsoft.com/pricing/free-trial/));
* подписка с поддержкой единого входа в InstaVR Viewer.

## <a name="scenario-description"></a>Описание сценария

В рамках этого руководства вы настроите и проверите единый вход Azure AD в тестовой среде.

* InstaVR Viewer поддерживает единый вход, инициированный **SP**
* InstaVR Viewer поддерживает **JIT**-подготовку пользователей

## <a name="adding-instavr-viewer-from-the-gallery"></a>Добавление InstaVR Viewer из коллекции

Чтобы настроить интеграцию InstaVR Viewer с Azure Active Directory, необходимо добавить его из коллекции в список управляемых приложений SaaS.

**Чтобы добавить InstaVR Viewer из коллекции, выполните следующие действия.**

1. На **[портале Azure](https://portal.azure.com)** в области навигации слева щелкните значок **Azure Active Directory**.

    ![Кнопка Azure Active Directory](common/select-azuread.png)

2. Перейдите в колонку **Корпоративные приложения** и выберите **Все приложения**.

    ![Колонка "Корпоративные приложения"](common/enterprise-applications.png)

3. Чтобы добавить новое приложение, в верхней части диалогового окна нажмите кнопку **Создать приложение**.

    ![Кнопка "Создать приложение"](common/add-new-app.png)

4. В поле поиска введите **InstaVR Viewer**, выберите **InstaVR Viewer** на панели результатов и нажмите кнопку **Добавить**, чтобы добавить это приложение.

     ![InstaVR Viewer в списке результатов](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Настройка и проверка единого входа в Azure AD

В этом разделе описана настройка и проверка единого входа в InstaVR Viewer через Azure Active Directory с использованием тестового пользователя **Britta Simon**.
Для обеспечения работы единого входа необходимо установить связь между пользователем Azure Active Directory и соответствующим пользователем InstaVR Viewer.

Чтобы настроить и проверить единый вход в InstaVR Viewer через Azure Active Directory, вам потребуется выполнить действия в следующих блоках.

1. **[Настройка единого входа Azure AD](#configure-azure-ad-single-sign-on)** необходима, чтобы пользователи могли использовать эту функцию.
2. **[Настройка единого входа в InstaVR Viewer](#configure-instavr-viewer-single-sign-on)** необходима, чтобы настроить параметры единого входа на стороне приложения.
3. **[Создание тестового пользователя Azure AD](#create-an-azure-ad-test-user)** требуется для проверки работы единого входа Azure AD от имени пользователя Britta Simon.
4. **[Назначение тестового пользователя Azure AD](#assign-the-azure-ad-test-user)** необходимо, чтобы разрешить пользователю Britta Simon использовать единый вход Azure AD.
5. **[Создание тестового пользователя InstaVR Viewer](#create-instavr-viewer-test-user)** требуется для того, чтобы в InstaVR Viewer существовал пользователь Britta Simon, связанный с одноименным пользователем в Azure Active Directory.
6. **[Проверка единого входа](#test-single-sign-on)** необходима, чтобы проверить работу конфигурации.

### <a name="configure-azure-ad-single-sign-on"></a>Настройка единого входа Azure AD

В этом разделе описано включение единого входа Azure AD на портале Azure.

Чтобы настроить единый вход в InstaVR Viewer через Azure Active Directory, выполните следующие действия.

1. На [портале Azure](https://portal.azure.com/) на странице интеграции с приложением **InstaVR Viewer** выберите **Единый вход**.

    ![Ссылка "Настройка единого входа"](common/select-sso.png)

2. В диалоговом окне **Выбрать метод единого входа** выберите режим **SAML/WS-Fed**, чтобы включить единый вход.

    ![Режим выбора единого входа](common/select-saml-option.png)

3. На странице **Настройка единого входа с помощью SAML** щелкните **Изменить**, чтобы открыть диалоговое окно **Базовая конфигурация SAML**.

    ![Правка базовой конфигурации SAML](common/edit-urls.png)

4. В разделе **Базовая конфигурация SAML** выполните приведенные ниже действия.

    ![Сведения о домене и URL-адресах единого входа для приложения InstaVR Viewer](common/sp-identifier.png)

    a. В текстовом поле **URL-адрес для входа** введите URL-адрес в следующем формате: `https://console.instavr.co/auth/saml/login/<WEBPackagedURL>`.

    > [!NOTE]
    > Не существует единого шаблона для URL-адреса входа. Он создается, когда пользователи InstaVR Viewer выполняют веб-подготовку приложений к развертыванию. Он уникален для каждого пользователя и пакета. Чтобы получить точный URL-адрес для входа, необходимо войти в экземпляр InstaVR Viewer и выполнить веб-подготовку к развертыванию.

    b. В текстовом поле **Идентификатор (сущности)** введите URL-адрес в следующем формате: `https://console.instavr.co/auth/saml/sp/<WEBPackagedURL>`.

    > [!NOTE]
    > Значение идентификатора приведено для примера и не является реальным. Замените его на фактический идентификатор, как описано далее в этом руководстве.

5. На странице **Настройка единого входа с помощью SAML** в разделе **Сертификат подписи SAML** щелкните **Загрузить**, чтобы загрузить требуемый **сертификат (Base64)** и **файл метаданных федерации** из предложенных вариантов, и сохраните их на компьютере.

    ![Ссылка для скачивания сертификата](common/metadata-certificatebase64.png)

6. Скопируйте требуемый URL-адрес из раздела **Настройка InstaVR Viewer**.

    ![Копирование URL-адресов настройки](common/copy-configuration-urls.png)

    а) URL-адрес входа.

    б) Идентификатор Azure AD.

    в) URL-адрес выхода.

### <a name="configure-instavr-viewer-single-sign-on"></a>Настройка единого входа InstaVR Viewer

1. В новом окне веб-браузера откройте веб-сайт компании InstaVR Viewer и войдите в качестве администратора.

2. Щелкните **Значок пользователя** и выберите **Учетная запись**.

    ![Панель конфигурации InstaVR Viewer](media/instavr-viewer-tutorial/tutorial-instavr-viewer-account.png)

3. Прокрутите вниз до раздела **SAML Auth** (Проверка подлинности SAML) и выполните следующие шаги.

    ![Панель конфигурации InstaVR Viewer](media/instavr-viewer-tutorial/tutorial-instavr-viewer-configure.png)

    a. В текстовое поле **SSO URL** (URL-адрес единого входа) вставьте **URL-адрес входа**, скопированный на портале Azure.

    b. В текстовое поле **URL-адрес выхода** вставьте значение **URL-адреса выхода**, скопированное на портале Azure.

    c. В текстовое поле **Идентификатор сущности** вставьте значение **идентификатора Azure AD**, скопированное на портале Azure.

    d. Щелкните **Обновить**, чтобы отправить скачанный файл сертификата.

    д. Щелкните **Обновить**, чтобы отправить скачанный файл метаданных федерации.

    Е. Скопируйте **идентификатор сущности** и вставьте его в текстовое поле **Идентификатор (сущности)** в разделе **Базовая конфигурация SAML** на портале Azure.

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

В этом разделе описано, как разрешить пользователю Britta Simon использовать единый вход Azure, предоставив этому пользователю доступ к InstaVR Viewer.

1. На портале Azure выберите **Корпоративные приложения**, **Все приложения**, а затем **InstaVR Viewer**.

    ![Колонка "Корпоративные приложения"](common/enterprise-applications.png)

2. В списке приложений введите и выберите **InstaVR Viewer**.

    ![Ссылка на InstaVR Viewer в списке приложений](common/all-applications.png)

3. В меню слева выберите **Пользователи и группы**.

    ![Ссылка "Пользователи и группы"](common/users-groups-blade.png)

4. Нажмите кнопку **Добавить пользователя**, а затем в диалоговом окне **Добавление назначения** выберите **Пользователи и группы**.

    ![Область "Добавление назначения"](common/add-assign-user.png)

5. В диалоговом окне **Пользователи и группы** из списка пользователей выберите **Britta Simon**, а затем в верхней части экрана нажмите кнопку **Выбрать**.

6. Если ожидается, что в утверждении SAML будет получено какое-либо значение роли, то в диалоговом окне **Выбор ролей** нужно выбрать соответствующую роль для пользователя из списка и затем нажать кнопку **Выбрать**, расположенную в нижней части экрана.

7. В диалоговом окне **Добавление назначения** нажмите кнопку **Назначить**.

### <a name="create-instavr-viewer-test-user"></a>Создание тестового пользователя InstaVR Viewer

В этом разделе вы создадите в InstaVR Viewer пользователя с именем Britta Simon. Приложение InstaVR Viewer поддерживает JIT-подготовку пользователей, которая включена по умолчанию. В этом разделе никакие действия с вашей стороны не требуются. Если пользователь еще не существует в InstaVR Viewer, он создается после проверки подлинности. При возникновении проблем обратитесь в [службу поддержки InstaVR Viewer](mailto:contact@instavr.co).

### <a name="test-single-sign-on"></a>Проверка единого входа

1. В новом окне веб-браузера откройте веб-сайт компании InstaVR Viewer и войдите в качестве администратора.

2. На левой навигационной панели выберите **Пакет**, а затем — **Make package for Web** (Создание пакета для сети).

    ![Панель конфигурации InstaVR Viewer](media/instavr-viewer-tutorial/tutorial-instavr-viewer-testing1.png)

3. Выберите **Скачать**.

    ![Панель конфигурации InstaVR Viewer](media/instavr-viewer-tutorial/tutorial-instavr-viewer-testing2.png)

4. Выберите **Open Hosted Page** (Открыть страницу размещения) и вы будете перенаправлены на страницу входа в Azure Active Directory.

    ![Панель конфигурации InstaVR Viewer](media/instavr-viewer-tutorial/tutorial-instavr-viewer-testing3.png)

5. Введите свои учетные данные Azure Active Directory, чтобы войти в Azure Active Directory с помощью единого входа.

## <a name="additional-resources"></a>Дополнительные ресурсы

- [Список учебников по интеграции приложений SaaS с Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Что такое условный доступ в Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
