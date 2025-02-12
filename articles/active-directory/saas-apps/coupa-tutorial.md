---
title: Руководство по Интеграция Azure Active Directory с Coupa | Документация Майкрософт
description: Узнайте, как настроить единый вход между Azure Active Directory и Coupa.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 47f27746-9057-4b9c-991e-3abf77710f73
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 01/25/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: f7b4db749d1191179b3f4f1d9bb13d77925697f2
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "67104547"
---
# <a name="tutorial-azure-active-directory-integration-with-coupa"></a>Руководство по Интеграция Azure Active Directory с Coupa

В этом руководстве описано, как интегрировать Coupa с Azure Active Directory (Azure AD).
Интеграция Azure AD с приложением Coupa обеспечивает следующие преимущества.

* С помощью Azure AD вы можете контролировать доступ к Coupa.
* Вы можете включить автоматический вход пользователей в Coupa (единый вход) с помощью учетной записи Azure AD.
* Вы можете управлять учетными записями централизованно на портале Azure.

Дополнительные сведения об интеграции приложений SaaS с Azure AD см. в статье [Единый вход в приложениях в Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Если у вас еще нет подписки Azure, [создайте бесплатную учетную запись Azure](https://azure.microsoft.com/free/), прежде чем начинать работу.

## <a name="prerequisites"></a>Предварительные требования

Чтобы настроить интеграцию Azure AD с Coupa, вам потребуется:

* подписка Azure AD (если у вас нет среды Azure AD, вы можете получить пробную версию на один месяц по [этой ссылке](https://azure.microsoft.com/pricing/free-trial/));
* подписка с поддержкой единого входа Coupa.

## <a name="scenario-description"></a>Описание сценария

В рамках этого руководства вы настроите и проверите единый вход Azure AD в тестовой среде.

* Coupa поддерживает единый вход, инициированный **поставщиком услуг**.

## <a name="adding-coupa-from-the-gallery"></a>Добавление Coupa из коллекции

Чтобы настроить интеграцию Coupa с Azure AD, необходимо добавить Coupa из коллекции в список управляемых приложений SaaS.

**Чтобы добавить Coupa из коллекции, сделайте следующее:**

1. На **[портале Azure](https://portal.azure.com)** в области навигации слева щелкните значок **Azure Active Directory**.

    ![Кнопка Azure Active Directory](common/select-azuread.png)

2. Перейдите в колонку **Корпоративные приложения** и выберите **Все приложения**.

    ![Колонка "Корпоративные приложения"](common/enterprise-applications.png)

3. Чтобы добавить новое приложение, в верхней части диалогового окна нажмите кнопку **Создать приложение**.

    ![Кнопка "Создать приложение"](common/add-new-app.png)

4. В поле поиска введите **Coupa**, выберите **Coupa** на панели результатов и нажмите кнопку **Добавить**, чтобы добавить это приложение.

     ![Coupa в списке результатов](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Настройка и проверка единого входа в Azure AD

В этом разделе описана настройка и проверка единого входа Azure AD в Coupa с использованием тестового пользователя **Britta Simon**.
Для обеспечения работы единого входа необходимо установить связь между пользователем Azure AD и соответствующим пользователем в Coupa.

Чтобы настроить и проверить единый вход Azure AD в Coupa, вам потребуется выполнить действия в следующих стандартных блоках.

1. **[Настройка единого входа Azure AD](#configure-azure-ad-single-sign-on)** необходима, чтобы пользователи могли использовать эту функцию.
2. **[Настройка единого входа в Coupa](#configure-coupa-single-sign-on)** необходима, чтобы настроить параметры единого входа на стороне приложения.
3. **[Создание тестового пользователя Azure AD](#create-an-azure-ad-test-user)** требуется для проверки работы единого входа Azure AD от имени пользователя Britta Simon.
4. **[Назначение тестового пользователя Azure AD](#assign-the-azure-ad-test-user)** необходимо, чтобы разрешить пользователю Britta Simon использовать единый вход Azure AD.
5. **[Создание тестового пользователя Coupa](#create-coupa-test-user)** требуется, чтобы в Coupa существовал пользователь Britta Simon, связанный с одноименным пользователем в Azure AD.
6. **[Проверка единого входа](#test-single-sign-on)** необходима, чтобы проверить работу конфигурации.

### <a name="configure-azure-ad-single-sign-on"></a>Настройка единого входа Azure AD

В этом разделе описано включение единого входа Azure AD на портале Azure.

Чтобы настроить единый вход Azure AD в Coupa, сделайте следующее.

1. На [портале Azure](https://portal.azure.com/) на странице интеграции с приложением **Coupa** щелкните **Единый вход**.

    ![Ссылка "Настройка единого входа"](common/select-sso.png)

2. В диалоговом окне **Выбрать метод единого входа** выберите режим **SAML/WS-Fed**, чтобы включить единый вход.

    ![Режим выбора единого входа](common/select-saml-option.png)

3. На странице **Настройка единого входа с помощью SAML** щелкните **Изменить**, чтобы открыть диалоговое окно **Базовая конфигурация SAML**.

    ![Правка базовой конфигурации SAML](common/edit-urls.png)

4. В разделе **Базовая конфигурация SAML** выполните приведенные ниже действия.

    ![Сведения о домене и URL-адресах единого входа для приложения Coupa](common/sp-identifier-reply.png)

    a. В текстовое поле **URL-адрес для входа** введите URL-адрес в следующем формате: `https://<companyname>.coupahost.com`.

    > [!NOTE]
    > Значение URL-адреса входа приведено для примера. Вместо него необходимо указать фактический URL-адрес входа. Чтобы получить это значение, обратитесь в [службу поддержки клиентов Coupa](https://success.coupa.com/Support/Contact_Us?).

    b. В поле **Идентификатор** введите URL-адрес:

    | Среда  | URL-адрес |
    |:-------------|----|
    | Песочница | `sso-stg1.coupahost.com`|
    | Производство | `sso-prd1.coupahost.com`|
    | | |

    c. В текстовом поле **URL-адрес ответа** введите URL-адрес:

    | Среда | URL-адрес |
    |------------- |----|
    | Песочница | `https://sso-stg1.coupahost.com/sp/ACS.saml2`|
    | Производство | `https://sso-prd1.coupahost.com/sp/ACS.saml2`|
    | | |

4. На странице **Настройка единого входа с помощью SAML** в разделе **Сертификат подписи SAML** щелкните **Скачать**, чтобы скачать нужный вам **XML метаданных федерации**, и сохраните его на компьютере.

    ![Ссылка для скачивания сертификата](common/metadataxml.png)

6. Требуемый URL-адрес можно скопировать из раздела **Настройка Coupa**.

    ![Копирование URL-адресов настройки](common/copy-configuration-urls.png)

    а) URL-адрес входа.

    б) Идентификатор Azure AD.

    в) URL-адрес выхода.

### <a name="configure-coupa-single-sign-on"></a>Настройка единого входа в Coupa

1. Выполните вход на веб-сайт компании Coupa в качестве администратора.

2. Последовательно выберите **Setup \> Security Control** (Настройка > Управление безопасностью).

    ![Security Controls](./media/coupa-tutorial/ic791900.png "Security Controls") (Средства управления безопасностью)

3. В разделе **Вход с использованием учетных данных Coupa** выполните следующие действия.

    ![Метаданные Coupa SP](./media/coupa-tutorial/ic791901.png "Метаданные Coupa SP")

    a. Выберите **Вход с помощью SAML**.

    b. Нажмите кнопку **Обзор**, чтобы отправить метаданные, загруженные с портала Azure.

    c. Выберите команду **Сохранить**.

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

В этом разделе описано, как предоставить пользователю Britta Simon доступ к Coupa, чтобы он мог использовать единый вход Azure.

1. На портале Azure выберите **Корпоративные приложения**, **Все приложения**, а затем — **Coupa**.

    ![Колонка "Корпоративные приложения"](common/enterprise-applications.png)

2. В списке приложений выберите **Coupa**.

    ![Ссылка на Coupa в списке "Приложения"](common/all-applications.png)

3. В меню слева выберите **Пользователи и группы**.

    ![Ссылка "Пользователи и группы"](common/users-groups-blade.png)

4. Нажмите кнопку **Добавить пользователя**, а затем в диалоговом окне **Добавление назначения** выберите **Пользователи и группы**.

    ![Область "Добавление назначения"](common/add-assign-user.png)

5. В диалоговом окне **Пользователи и группы** из списка пользователей выберите **Britta Simon**, а затем в верхней части экрана нажмите кнопку **Выбрать**.

6. Если ожидается, что в утверждении SAML будет получено какое-либо значение роли, то в диалоговом окне **Выбор ролей** нужно выбрать соответствующую роль для пользователя из списка и затем нажать кнопку **Выбрать**, расположенную в нижней части экрана.

7. В диалоговом окне **Добавление назначения** нажмите кнопку **Назначить**.

### <a name="create-coupa-test-user"></a>Создание тестового пользователя Coupa

Чтобы пользователи Azure AD могли выполнять вход в Coupa, они должны быть подготовлены для Coupa.  

* В случае с Coupa подготовка выполняется вручную.

**Чтобы настроить подготовку учетных записей пользователей, выполните следующие действия.**

1. Выполните вход на веб-сайт компании **Coupa** в качестве администратора.

2. В меню вверху щелкните **Setup** (Настройка) и выберите **Users** (Пользователи).

    ![Пользователи](./media/coupa-tutorial/ic791908.png "Пользователи")

3. Нажмите кнопку **Создать**.

    ![Создание пользователей](./media/coupa-tutorial/ic791909.png "Создание пользователей")

4. В разделе **Создание пользователя** выполните следующие действия.

    ![Сведения о пользователе](./media/coupa-tutorial/ic791910.png "Сведения о пользователе")

    a. В соответствующие текстовые поля введите атрибуты **Login** (Имя для входа), **First name** (Имя), **Last Name** (Фамилия), **Single Sign-On ID** (Идентификатор единого входа) и **Email** (Адрес электронной почты) действующей учетной записи Azure Active Directory, которую вы хотите подготовить.

    b. Нажмите кнопку **Создать**.

    >[!NOTE]
    >Владелец учетной записи Azure Active Directory получит электронное сообщение со ссылкой для подтверждения учетной записи перед ее активацией.
    >

>[!NOTE]
>Вы можете использовать любые другие средства создания учетной записи пользователя Coupa или API, предоставляемые Coupa для подготовки учетных записей пользователя AAD.

### <a name="test-single-sign-on"></a>Проверка единого входа 

В этом разделе описано, как проверить конфигурацию единого входа Azure AD с помощью панели доступа.

Щелкнув плитку Coupa на панели доступа, вы автоматически войдете в приложение Coupa, для которого настроили единый вход. См. дополнительные сведения о [панели доступа](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)

## <a name="additional-resources"></a>Дополнительные ресурсы

- [Список учебников по интеграции приложений SaaS с Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Что такое условный доступ в Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

