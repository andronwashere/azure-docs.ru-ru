---
title: Руководство по Интеграция Azure Active Directory с GitHub | Документация Майкрософт
description: Узнайте, как настроить единый вход между Azure Active Directory и GitHub.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: 8761f5ca-c57c-4a7e-bf14-ac0421bd3b5e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 02/11/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: f0730dd89a61b046f73e7e39c8226e66700b3494
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "67101641"
---
# <a name="tutorial-azure-active-directory-integration-with-github"></a>Руководство по Интеграция Azure Active Directory с GitHub

В этом руководстве описано, как интегрировать GitHub с Azure Active Directory (Azure AD).
Интеграция Azure AD с GitHub обеспечивает следующие преимущества.

* С помощью Azure AD вы можете контролировать доступ к GitHub.
* Вы можете включить автоматический вход пользователей в GitHub (единый вход) с помощью учетных записей Azure AD.
* Вы можете управлять учетными записями централизованно на портале Azure.

Дополнительные сведения об интеграции приложений SaaS с Azure AD см. в статье [Единый вход в приложениях в Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Если у вас еще нет подписки Azure, [создайте бесплатную учетную запись Azure](https://azure.microsoft.com/free/), прежде чем начинать работу.

## <a name="prerequisites"></a>Предварительные требования

Чтобы настроить интеграцию Azure AD с GitHub, вам потребуется:

* подписка Azure AD (если у вас нет среды Azure AD, вы можете получить пробную версию на один месяц по [этой ссылке](https://azure.microsoft.com/pricing/free-trial/));
* Организация GitHub, созданная в облаке [GitHub Enterprise](https://help.github.com/articles/github-s-products/#github-enterprise), которой требуется [план выставления счетов GitHub Enterprise](https://help.github.com/articles/github-s-billing-plans/#billing-plans-for-organizations).

## <a name="scenario-description"></a>Описание сценария

В рамках этого руководства вы настроите и проверите единый вход Azure AD в тестовой среде.

* GitHub поддерживает единый вход инициированного **пакета обновления**.

* GitHub поддерживает [**автоматическую** подготовку пользователей](github-provisioning-tutorial.md).

## <a name="adding-github-from-the-gallery"></a>Добавление GitHub из коллекции

Чтобы настроить интеграцию GitHub с Azure AD, необходимо добавить GitHub из коллекции в список управляемых приложений SaaS.

**Чтобы добавить приложение GitHub из коллекции, сделайте следующее.**

1. На **[портале Azure](https://portal.azure.com)** в области навигации слева щелкните значок **Azure Active Directory**.

    ![Кнопка Azure Active Directory](common/select-azuread.png)

2. Перейдите в колонку **Корпоративные приложения** и выберите **Все приложения**.

    ![Колонка "Корпоративные приложения"](common/enterprise-applications.png)

3. Чтобы добавить новое приложение, в верхней части диалогового окна нажмите кнопку **Создать приложение**.

    ![Кнопка "Создать приложение"](common/add-new-app.png)

4. В поле поиска введите **GitHub**, выберите **GitHub.com** на панели результатов и нажмите кнопку **Добавить**, чтобы добавить это приложение.

     ![GitHub в списке результатов](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Настройка и проверка единого входа в Azure AD

В этом разделе описано, как настроить и проверить единый вход Azure AD с помощью GitHub с использованием тестового пользователя **Britta Simon**.
Для обеспечения работы единого входа необходимо установить связь между пользователем Azure AD и соответствующим пользователем в GitHub.

Чтобы настроить и проверить единый вход Azure AD в GitHub, потребуется выполнить действия в следующих стандартных блоках.

1. **[Настройка единого входа Azure AD](#configure-azure-ad-single-sign-on)** необходима, чтобы пользователи могли использовать эту функцию.
2. **[Настройка единого входа в GitHub](#configure-github-single-sign-on)** необходима, чтобы настроить параметры единого входа на стороне приложения.
3. **[Создание тестового пользователя Azure AD](#create-an-azure-ad-test-user)** требуется для проверки работы единого входа Azure AD от имени пользователя Britta Simon.
4. **[Назначение тестового пользователя Azure AD](#assign-the-azure-ad-test-user)** необходимо, чтобы разрешить пользователю Britta Simon использовать единый вход Azure AD.
5. **[Создание тестового пользователя GitHub](#create-github-test-user)** нужно для того, чтобы в GitHub также существовал пользователь Britta Simon, связанный с представлением пользователя в Azure AD.
6. **[Проверка единого входа](#test-single-sign-on)** необходима, чтобы проверить работу конфигурации.

### <a name="configure-azure-ad-single-sign-on"></a>Настройка единого входа Azure AD

В этом разделе описано включение единого входа Azure AD на портале Azure.

Чтобы настроить единый вход Azure AD в GitHub, сделайте следующее.

1. На [портале Azure](https://portal.azure.com/) на странице интеграции с приложением **GitHub** щелкните **Единый вход**.

    ![Ссылка "Настройка единого входа"](common/select-sso.png)

2. В диалоговом окне **Выбрать метод единого входа** выберите режим **SAML/WS-Fed**, чтобы включить единый вход.

    ![Режим выбора единого входа](common/select-saml-option.png)

3. На странице **Настройка единого входа с помощью SAML** щелкните **Изменить**, чтобы открыть диалоговое окно **Базовая конфигурация SAML**.

    ![Правка базовой конфигурации SAML](common/edit-urls.png)

4. В разделе **Базовая конфигурация SAML** выполните приведенные ниже действия.

    ![Сведения о домене и URL-адресах единого входа приложения GitHub](common/sp-identifier.png)

    a. В текстовом поле **URL-адрес для входа** введите URL-адрес в следующем формате: `https://github.com/orgs/<entity-id>/sso`.

    b. В текстовом поле **Идентификатор (сущности)** введите URL-адрес в следующем формате: `https://github.com/orgs/<entity-id>`.

    > [!NOTE]
    > Обратите внимание, что значения, указанные выше, используются в качестве примера. Необходимо заменить эти значения фактическим URL-адресом для входа и идентификатором. Мы рекомендуем использовать уникальное значение строки идентификатора. Перейдите в раздел администрирования GitHub, чтобы получить эти значения.

5. Приложение GitHub ожидает утверждения SAML в определенном формате, поэтому следует добавить настраиваемые сопоставления атрибутов в вашу конфигурацию атрибутов токена SAML. На следующем снимке экрана показан список атрибутов по умолчанию, когда **nameidentifier** сопоставляется с **user.userprincipalname**. Приложение GitHub ожидает, что **nameidentifier** будет сопоставляться с **user.mail**, поэтому необходимо изменить сопоставление атрибутов, щелкнув значок **Edit** (Изменить).

    ![image](common/edit-attribute.png)

6. На странице **Настройка единого входа с помощью SAML** в разделе **Сертификат подписи SAML** щелкните **Загрузить**, чтобы загрузить требуемый **сертификат (Base64)** из предложенных вариантов, и сохраните его на компьютере.

    ![Ссылка для скачивания сертификата](common/certificatebase64.png)

7. Требуемый URL-адрес можно скопировать из раздела **Настройка GitHub**.

    ![Копирование URL-адресов настройки](common/copy-configuration-urls.png)

    а) URL-адрес входа.

    б) Идентификатор Azure AD.

    в) URL-адрес выхода.

### <a name="configure-github-single-sign-on"></a>Настройка единого входа в GitHub

1. В другом окне веб-браузера войдите на свой корпоративный сайт GitHub в качестве администратора.

2. Перейдите к разделу **Параметры** и щелкните **Безопасность**.

    ![Параметры](./media/github-tutorial/tutorial_github_config_github_03.png)

3. Установите флажок **Enable SAML authentication** (Включить проверку подлинности SAML). После этого появятся поля конфигурации единого входа. Затем с помощью значения URL-адреса единого входа обновите URL-адрес единого входа в конфигурации Azure AD.

    ![Параметры](./media/github-tutorial/tutorial_github_config_github_13.png)

4. Задайте значения в следующих полях:

    ![Параметры](./media/github-tutorial/tutorial_github_config_github_051.png)

    a. В текстовое поле **Sign on URL** (URL-адрес входа) вставьте значение **URL-адрес входа**, скопированное на портале Azure.

    b. В текстовое поле **Issuer** (Издатель) вставьте значение **Идентификатор Azure AD**, скопированное на портале Azure.

    c. Откройте в Блокноте сертификат, скачанный с портала Azure, и вставьте его содержимое в текстовое поле **Public Certificate** (Общедоступный сертификат).

    d. Щелкните значок **Изменить**, чтобы изменить **метод подписи** и **метод выборки** с **RSA-SHA1** и **SHA1** на **RSA-SHA256** и **SHA256**, как показано ниже.

    ![image](./media/github-tutorial/tutorial_github_sha.png)

5. Щелкните **Test SAML configuration** (Проверка конфигурации SAML), чтобы убедиться в отсутствии сбоев проверки при выполнении единого входа.

    ![Параметры](./media/github-tutorial/tutorial_github_config_github_06.png)

6. Нажмите кнопку **Сохранить**

> [!NOTE]
> Технология единого входа в GitHub позволяет выполнять аутентификацию определенных организаций и не заменяет метод аутентификации самого репозитория. Однако если сеанс github.com истекает, пользователи могут получить запрос на вход с использованием идентификатора и пароля GitHub во время процесса единого входа.

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

В этом разделе объясняется, как разрешить пользователю Britta Simon использовать единый вход Azure, предоставив этому пользователю доступ к GitHub.

1. На портале Azure выберите **Корпоративные приложения**, **Все приложения**, а затем — **GitHub**.

    ![Колонка "Корпоративные приложения"](common/enterprise-applications.png)

2. В списке приложений выберите **GitHub**.

    ![Ссылка на GitHub в списке "Приложения"](common/all-applications.png)

3. В меню слева выберите **Пользователи и группы**.

    ![Ссылка "Пользователи и группы"](common/users-groups-blade.png)

4. Нажмите кнопку **Добавить пользователя**, а затем в диалоговом окне **Добавление назначения** выберите **Пользователи и группы**.

    ![Область "Добавление назначения"](common/add-assign-user.png)

5. В диалоговом окне **Пользователи и группы** из списка пользователей выберите **Britta Simon**, а затем в верхней части экрана нажмите кнопку **Выбрать**.

6. Если ожидается, что в утверждении SAML будет получено какое-либо значение роли, то в диалоговом окне **Выбор ролей** нужно выбрать соответствующую роль для пользователя из списка и затем нажать кнопку **Выбрать**, расположенную в нижней части экрана.

7. В диалоговом окне **Добавление назначения** нажмите кнопку **Назначить**.

### <a name="create-github-test-user"></a>Создание тестового пользователя GitHub

В рамках этого раздела создается пользователь с именем Britta Simon в GitHub. GitHub поддерживает автоматическую подготовку пользователей, которая по умолчанию включена. Дополнительные сведения о настройке автоматической подготовки пользователей можно найти [здесь](github-provisioning-tutorial.md).

**Если необходимо создать пользователя вручную, выполните следующие действия:**

1. Выполните вход на свой корпоративный сайт Github в качестве администратора.

2. Выберите параметр **Пользователи**.

    ![Люди](./media/github-tutorial/tutorial_github_config_github_08.png "Люди")

3. Нажмите **Invite member** (Пригласить участника).

    ![Приглашение пользователей](./media/github-tutorial/tutorial_github_config_github_09.png "приглашение пользователей")

4. На странице диалогового окна **Invite member** (Приглашение участников) сделайте следующее.

    a. В текстовом поле **Электронная почта** введите адрес электронной почты учетной записи Britta Simon.

    ![Приглашение участников](./media/github-tutorial/tutorial_github_config_github_10.png "приглашение участников")

    b. Щелкните **Send Invitation** (Отправить приглашение).

    ![Приглашение участников](./media/github-tutorial/tutorial_github_config_github_11.png "приглашение участников")

    > [!NOTE]
    > Владелец учетной записи Azure Active Directory получит по электронной почте сообщение со ссылкой для активации учетной записи.

### <a name="test-single-sign-on"></a>Проверка единого входа 

В этом разделе описано, как проверить конфигурацию единого входа Azure AD с помощью панели доступа.

Щелкнув плитку "GitHub" на панели доступа, вы автоматически войдете в приложение GitHub, для которого настроили единый вход. См. дополнительные сведения о [панели доступа](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)

## <a name="additional-resources"></a>Дополнительные ресурсы

- [Список учебников по интеграции приложений SaaS с Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Что такое условный доступ в Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
