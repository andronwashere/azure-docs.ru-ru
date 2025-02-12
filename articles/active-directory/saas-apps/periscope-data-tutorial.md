---
title: Руководство по Интеграция Azure Active Directory с Periscope Data | Документация Майкрософт
description: Узнайте, как настроить единый вход Azure Active Directory в Periscope Data.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 3f378edb-9ac9-494d-a84a-03357b923ee1
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 02/21/2019
ms.author: jeedes
ms.openlocfilehash: efde1f1dafc62576398c5225ad1c652438fc0c31
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "67094502"
---
# <a name="tutorial-azure-active-directory-integration-with-periscope-data"></a>Руководство по Интеграция Azure Active Directory с Periscope Data

В этом руководстве описано, как интегрировать Periscope Data с Azure Active Directory (Azure AD).
Интеграция Periscope Data с Azure AD обеспечивает следующие преимущества:

* С помощью Azure AD вы можете контролировать доступ к Periscope Data.
* Вы можете включить автоматический вход пользователей в Periscope Data (единый вход) с помощью их учетных записей Azure AD.
* Вы можете управлять учетными записями централизованно на портале Azure.

Дополнительные сведения об интеграции приложений SaaS с Azure AD см. в статье [Единый вход в приложениях в Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Если у вас еще нет подписки Azure, [создайте бесплатную учетную запись Azure](https://azure.microsoft.com/free/), прежде чем начинать работу.

## <a name="prerequisites"></a>Предварительные требования

Чтобы настроить интеграцию Azure AD с Periscope Data, вам потребуется:

* подписка Azure AD (если у вас нет среды Azure AD, вы можете получить пробную версию на один месяц по [этой ссылке](https://azure.microsoft.com/pricing/free-trial/));
* подписка Periscope Data с поддержкой единого входа.

## <a name="scenario-description"></a>Описание сценария

В рамках этого руководства вы настроите и проверите единый вход Azure AD в тестовой среде.

* Periscope Data поддерживает единый вход, инициированный **поставщиком услуг**.

## <a name="adding-periscope-data-from-the-gallery"></a>Добавление Periscope Data из коллекции.

Чтобы настроить интеграцию Periscope Data с Azure AD, необходимо добавить Periscope Data из коллекции в список управляемых приложений SaaS.

**Чтобы добавить Periscope Data из коллекции, выполните следующие действия**:

1. На **[портале Azure](https://portal.azure.com)** в области навигации слева щелкните значок **Azure Active Directory**.

    ![Кнопка Azure Active Directory](common/select-azuread.png)

2. Перейдите в колонку **Корпоративные приложения** и выберите **Все приложения**.

    ![Колонка "Корпоративные приложения"](common/enterprise-applications.png)

3. Чтобы добавить новое приложение, в верхней части диалогового окна нажмите кнопку **Создать приложение**.

    ![Кнопка "Создать приложение"](common/add-new-app.png)

4. В поле поиска введите **Periscope Data**, выберите **Periscope Data** на панели результатов и щелкните **Добавить**, чтобы добавить это приложение.

     ![Periscope Data в списке результатов](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Настройка и проверка единого входа в Azure AD

В этом разделе описана настройка и проверка единого входа AAD в Periscope Data с использованием тестового пользователя **Britta Simon**.
Для обеспечения работы единого входа необходимо установить связь между пользователем AAD и соответствующим пользователем в Periscope Data.

Чтобы настроить и проверить единый вход Azure AD в Periscope Data, вам потребуется выполнить действия в следующих стандартных блоках.

1. **[Настройка единого входа Azure AD](#configure-azure-ad-single-sign-on)** необходима, чтобы пользователи могли использовать эту функцию.
2. **[Настройка единого входа в Periscope Data](#configure-periscope-data-single-sign-on)** необходима, чтобы настроить параметры единого входа на стороне приложения.
3. **[Создание тестового пользователя Azure AD](#create-an-azure-ad-test-user)** требуется для проверки работы единого входа Azure AD от имени пользователя Britta Simon.
4. **[Назначение тестового пользователя Azure AD](#assign-the-azure-ad-test-user)** необходимо, чтобы разрешить пользователю Britta Simon использовать единый вход Azure AD.
5. **[Создание тестового пользователя Periscope Data](#create-periscope-data-test-user)** требуется для того, чтобы в Periscope Data существовал пользователь Britta Simon, связанный с одноименным пользователем в AAD.
6. **[Проверка единого входа](#test-single-sign-on)** необходима, чтобы проверить работу конфигурации.

### <a name="configure-azure-ad-single-sign-on"></a>Настройка единого входа Azure AD

В этом разделе описано включение единого входа Azure AD на портале Azure.

Чтобы настроить единый вход Azure AD в Periscope Data, выполните следующие действия:

1. На [портале Azure](https://portal.azure.com/) на странице интеграции с приложением **Periscope Data** выберите **Единый вход**.

    ![Ссылка "Настройка единого входа"](common/select-sso.png)

2. В диалоговом окне **Выбрать метод единого входа** выберите режим **SAML/WS-Fed**, чтобы включить единый вход.

    ![Режим выбора единого входа](common/select-saml-option.png)

3. На странице **Настройка единого входа с помощью SAML** щелкните **Изменить**, чтобы открыть диалоговое окно **Базовая конфигурация SAML**.

    ![Правка базовой конфигурации SAML](common/edit-urls.png)

4. В разделе **Базовая конфигурация SAML** выполните приведенные ниже действия.

    ![Сведения о домене и URL-адресах единого входа для приложения Periscope Data](common/sp-identifier.png)

    a. В текстовом поле **URL-адрес для входа** введите любой из следующих URL-адресов.
    
    | |
    |--|
    | `https://app.periscopedata.com/` |
    | `https://app.periscopedata.com/app/<SITENAME>` |

    b. В текстовом поле **Идентификатор (сущности)** введите URL-адрес в следующем формате: `https://app.periscopedata.com/<SITENAME>/sso`.

    > [!NOTE]
    > Значение URL-адреса входа приведено только для примера. Замените эти значения фактическим значением URL-адреса входа. Обратитесь в [службу поддержки Periscope Data](mailto:support@periscopedata.com), чтобы получить это значение и значение идентификатора, которое упоминается в разделе **Настройка единого входа в Periscope Data** далее в этом руководстве. Можно также посмотреть шаблоны в разделе **Базовая конфигурация SAML** на портале Azure.

5. На странице **Настройка единого входа с помощью SAML** в разделе **Сертификат подписи SAML** нажмите кнопку "Копировать", чтобы копировать **URL-адрес метаданных федерации приложений** и сохранить его на компьютере.

    ![Ссылка для скачивания сертификата](common/copy-metadataurl.png)

### <a name="configure-periscope-data-single-sign-on"></a>Настройка единого входа в Periscope Data

1. В другом окне веб-браузера войдите в Periscope Data с правами администратора.

2. Откройте меню кнопкой с шестеренкой в нижнем левом углу, затем выберите пункты меню **Billing** (Выставление счетов)  > **Security** (Безопасность) и выполните следующие действия. Только администраторы имеют доступ к этим параметрам.

    ![Сведения о настройке Periscope Data](./media/periscope-data-tutorial/configure01.png)

    a. Скопируйте **URL-адрес метаданных федерации приложений** из **сертификата подписи SAML**, полученного на шаге 5, и откройте этот адрес в браузере. Откроется XML-документ.

    b. В текстовом поле **Single Sign-On** (Единый вход) выберите **Azure Active Directory**.

    c. Найдите тег **SingleSignOnService** и вставьте сюда значение **Location** (Расположение) в текстовом поле **SSO URL** (URL-адрес единого входа).

    d. Найдите тег **SingleLogoutService** и вставьте значение **Location** (Расположение) в текстовом поле **SLO URL** (URL-адрес единого выхода).

    д. Скопируйте значение **Identifier** (Идентификатор) для своего экземпляра и вставьте его в текстовое поле **Идентификатор (сущности)** в разделе**Базовая конфигурация SAML** на портале Azure.

    Е. Перейдите к первому тегу в XML-файле, скопируйте значение **entityID** и вставьте его в текстовое поле **Issuer** (Издатель).

    ж. Найдите тег **IDPSSODescriptor** с описанием протокола SAML. В этом разделе найдите тег **KeyDescriptor**, для которого указано **use=signing**. Скопируйте значение **X509Certificate** и вставьте его в текстовое поле **Certificate** (Сертификат).

    h. Для сайтов с несколькими пространствами можно выбрать пространство по умолчанию с помощью раскрывающегося списка **Default Space** (Пространство по умолчанию). Именно в это пространство будут добавляться новые пользователи, которые впервые входят в Periscope Data и подготавливаются через единый вход Active Directory.

    i. Наконец, нажмите кнопку **Save** (Сохранить) и **подтвердите** изменение параметров единого входа, введя команду **Logout**.

    ![Сведения о настройке Periscope Data](./media/periscope-data-tutorial/configure02.png)

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

В этом разделе описано, как разрешить пользователю Britta Simon использовать единый вход Azure, предоставив этому пользователю доступ к Periscope Data.

1. На портале Azure выберите **Корпоративные приложения**, **Все приложения**, а затем — **Periscope Data**.

    ![Колонка "Корпоративные приложения"](common/enterprise-applications.png)

2. В списке приложений выберите **Periscope Data**.

    ![Ссылка на Periscope Data в списке "Приложения"](common/all-applications.png)

3. В меню слева выберите **Пользователи и группы**.

    ![Ссылка "Пользователи и группы"](common/users-groups-blade.png)

4. Нажмите кнопку **Добавить пользователя**, а затем в диалоговом окне **Добавление назначения** выберите **Пользователи и группы**.

    ![Область "Добавление назначения"](common/add-assign-user.png)

5. В диалоговом окне **Пользователи и группы** из списка пользователей выберите **Britta Simon**, а затем в верхней части экрана нажмите кнопку **Выбрать**.

6. Если ожидается, что в утверждении SAML будет получено какое-либо значение роли, то в диалоговом окне **Выбор ролей** нужно выбрать соответствующую роль для пользователя из списка и затем нажать кнопку **Выбрать**, расположенную в нижней части экрана.

7. В диалоговом окне **Добавление назначения** нажмите кнопку **Назначить**.

### <a name="create-periscope-data-test-user"></a>Создание тестового пользователя Periscope Data

Чтобы пользователи AAD могли выполнять вход в Periscope Data, они должны быть подготовлены в Periscope Data. В Periscope Data подготовка выполняется вручную.

**Чтобы подготовить учетную запись пользователя, сделайте следующее:**

1. Войдите в Periscope Data с правами администратора.

2. Щелкните значок **Параметры** в левой нижней части меню и перейдите к пункту **Permissions** (Разрешения).

    ![Сведения о настройке Periscope Data](./media/periscope-data-tutorial/configure03.png)

3. Щелкните **ADD USER** (Добавить пользователя) выполните следующие действия.

      ![Сведения о настройке Periscope Data](./media/periscope-data-tutorial/configure04.png)

    a. В текстовое поле **First Name** (Имя) введите имя пользователя, например **Britta**.

    b. В текстовое поле **Last Name** (Фамилия) введите фамилию пользователя, например **Simon**.

    c. В текстовом поле **Email** (Адрес электронной почты) введите адрес электронной почты пользователя, например **brittasimon\@contoso.com**.

    d. Щелкните **ADD** (Добавить).

### <a name="test-single-sign-on"></a>Проверка единого входа 

В этом разделе описано, как проверить конфигурацию единого входа Azure AD с помощью панели доступа.

Щелкнув плитку Periscope Data на панели доступа, вы автоматически войдете в приложение Periscope Data, для которого настроили единый вход. См. дополнительные сведения о [панели доступа](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)

## <a name="additional-resources"></a>Дополнительные ресурсы

- [Список учебников по интеграции приложений SaaS с Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Что такое условный доступ в Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

