---
title: Руководство по Интеграция Azure Active Directory с SumoLogic | Документация Майкрософт
description: Узнайте, как настроить единый вход Azure Active Directory в SumoLogic.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: fbb76765-92d7-4801-9833-573b11b4d910
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/07/2019
ms.author: jeedes
ms.openlocfilehash: 07874988c97044ede59c18b8f85a425483679431
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "67089528"
---
# <a name="tutorial-azure-active-directory-integration-with-sumologic"></a>Руководство по Интеграция Azure Active Directory с SumoLogic

В этом руководстве описано, как интегрировать SumoLogic с Azure Active Directory (Azure AD).
Интеграция Azure AD с приложением SumoLogic обеспечивает следующие преимущества.

* С помощью Azure AD вы можете контролировать доступ к SumoLogic.
* Вы можете включить автоматический вход пользователей в SumoLogic (единый вход) с помощью учетной записи Azure AD.
* Вы можете управлять учетными записями централизованно на портале Azure.

Дополнительные сведения об интеграции приложений SaaS с Azure AD см. в статье [Единый вход в приложениях в Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Если у вас еще нет подписки Azure, [создайте бесплатную учетную запись Azure](https://azure.microsoft.com/free/), прежде чем начинать работу.

## <a name="prerequisites"></a>Предварительные требования

Чтобы настроить интеграцию Azure AD с приложением SumoLogic, вам потребуется:

* подписка Azure AD (если у вас нет среды Azure AD, вы можете получить пробную версию на один месяц по [этой ссылке](https://azure.microsoft.com/pricing/free-trial/));
* подписка SumoLogic с поддержкой единого входа.

## <a name="scenario-description"></a>Описание сценария

В рамках этого руководства вы настроите и проверите единый вход Azure AD в тестовой среде.

* SumoLogic поддерживает единый вход, инициированный **поставщиком услуг**.

## <a name="adding-sumologic-from-the-gallery"></a>Добавление SumoLogic из коллекции

Чтобы настроить интеграцию SumoLogic с Azure AD, необходимо добавить SumoLogic из коллекции в список управляемых приложений SaaS.

**Чтобы добавить SumoLogic из коллекции, сделайте следующее:**

1. На **[портале Azure](https://portal.azure.com)** в области навигации слева щелкните значок **Azure Active Directory**.

    ![Кнопка Azure Active Directory](common/select-azuread.png)

2. Перейдите в колонку **Корпоративные приложения** и выберите **Все приложения**.

    ![Колонка "Корпоративные приложения"](common/enterprise-applications.png)

3. Чтобы добавить новое приложение, в верхней части диалогового окна нажмите кнопку **Создать приложение**.

    ![Кнопка "Создать приложение"](common/add-new-app.png)

4. В поле поиска введите **SumoLogic**, на панели результатов выберите **SumoLogic** и нажмите кнопку **Добавить**, чтобы добавить это приложение.

     ![SumoLogic в списке результатов](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Настройка и проверка единого входа в Azure AD

В этом разделе описана настройка и проверка единого входа Azure AD в SumoLogic с использованием тестового пользователя **Britta Simon**.
Для обеспечения работы единого входа необходимо установить связь между пользователем Azure AD и соответствующим пользователем в SumoLogic.

Чтобы настроить и проверить единый вход Azure AD в SumoLogic, выполните действия в следующих стандартных блоках:

1. **[Настройка единого входа Azure AD](#configure-azure-ad-single-sign-on)** необходима, чтобы пользователи могли использовать эту функцию.
2. **[Настройка единого входа в SumoLogic](#configure-sumologic-single-sign-on)** необходима, чтобы настроить параметры единого входа на стороне приложения.
3. **[Создание тестового пользователя Azure AD](#create-an-azure-ad-test-user)** требуется для проверки работы единого входа Azure AD от имени пользователя Britta Simon.
4. **[Назначение тестового пользователя Azure AD](#assign-the-azure-ad-test-user)** необходимо, чтобы разрешить пользователю Britta Simon использовать единый вход Azure AD.
5. **[Создание тестового пользователя SumoLogic](#create-sumologic-test-user)** требуется для того, чтобы в SumoLogic существовал пользователь Britta Simon, связанный с одноименным пользователем в Azure AD.
6. **[Проверка единого входа](#test-single-sign-on)** необходима, чтобы проверить работу конфигурации.

### <a name="configure-azure-ad-single-sign-on"></a>Настройка единого входа Azure AD

В этом разделе описано включение единого входа Azure AD на портале Azure.

Чтобы настроить единый вход Azure AD в SumoLogic, выполните следующие действия:

1. На [портале Azure](https://portal.azure.com/) на странице интеграции с приложением **SumoLogic** выберите **Единый вход**.

    ![Ссылка "Настройка единого входа"](common/select-sso.png)

2. В диалоговом окне **Выбрать метод единого входа** выберите режим **SAML/WS-Fed**, чтобы включить единый вход.

    ![Режим выбора единого входа](common/select-saml-option.png)

3. На странице **Настройка единого входа с помощью SAML** щелкните **Изменить**, чтобы открыть диалоговое окно **Базовая конфигурация SAML**.

    ![Правка базовой конфигурации SAML](common/edit-urls.png)

4. В разделе **Базовая конфигурация SAML** выполните приведенные ниже действия.

    ![Сведения о домене и URL-адресах единого входа приложения SumoLogic](common/sp-identifier.png)

    a. В текстовом поле **URL-адрес для входа** введите URL-адрес в следующем формате: `https://<tenantname>.SumoLogic.com`.

   b. В текстовом поле **Идентификатор (сущности)** введите URL-адрес в следующем формате:

    | |
    |--|
    | `https://<tenantname>.us2.sumologic.com` |
    | `https://<tenantname>.sumologic.com` |
    | `https://<tenantname>.us4.sumologic.com` |
    | `https://<tenantname>.eu.sumologic.com` |
    | `https://<tenantname>.au.sumologic.com` |

    > [!NOTE]
    > Эти значения приведены для примера. Необходимо обновить эти значения действующим URL-адресом для входа и идентификатором. Чтобы получить эти значения, обратитесь в [службу поддержки клиентов SumoLogic](https://www.sumologic.com/contact-us/). Можно также посмотреть шаблоны в разделе **Базовая конфигурация SAML** на портале Azure.

5. На странице **Настройка единого входа с помощью SAML** в разделе **Сертификат подписи SAML** щелкните **Загрузить**, чтобы загрузить требуемый **сертификат (Base64)** из предложенных вариантов, и сохраните его на компьютере.

    ![Ссылка для скачивания сертификата](common/certificatebase64.png)

6. Требуемые URL-адреса можно скопировать из раздела **Настройка SumoLogic**.

    ![Копирование URL-адресов настройки](common/copy-configuration-urls.png)

    а) URL-адрес входа.

    b. Идентификатор Azure AD

    c. URL-адрес выхода.

### <a name="configure-sumologic-single-sign-on"></a>Настройка единого входа в SumoLogic

1. В другом окне веб-браузера войдите на корпоративный веб-сайт SumoLogic в качестве администратора.

1. Выберите **Manage (Управление) \> Security (Безопасность)** .

    ![Управление](./media/sumologic-tutorial/ic778556.png "Управление")

1. Нажмите кнопку **SAML**.

    ![Глобальные параметры безопасности](./media/sumologic-tutorial/ic778557.png "Глобальные параметры безопасности")

1. В списке **Select a configuration or create a new one** (Выберите настройку или создайте новую) выберите **Azure AD**, а затем щелкните **Configure** (Настройка).

    ![Настройка SAML 2.0](./media/sumologic-tutorial/ic778558.png "Настройка SAML 2.0")

1. В диалоговом окне **Настройка SAML 2.0** сделайте следующее:

    ![Настройка SAML 2.0](./media/sumologic-tutorial/ic778559.png "Настройка SAML 2.0")

    a. В текстовом поле **Configuration Name** (Имя конфигурации) введите **Azure AD**.

    b. Выберите **Режим отладки**.

    c. В текстовое поле **Issuer** (Издатель) вставьте значение **идентификатора Azure AD**, скопированное на портале Azure.

    d. В текстовое поле **Authn Request URL** (URL-адрес для запроса аутентификации) вставьте **URL-адрес входа**, скопированный на портале Azure.

    д. Откройте сертификат в кодировке Base-64 в Блокноте, скопируйте его содержимое в буфер обмена и вставьте весь сертификат в текстовое поле **Сертификат X.509** .

    Е. В поле **Email Attribute** (Атрибут электронной почты) задайте значение **Use SAML subject** (Использовать субъект SAML).  

    ж. Выберите пункт **Конфигурация входа, инициируемая поставщиком услуг**.

    h. В текстовом поле **Login Path** (Путь входа) введите **Azure** и нажмите кнопку **Save** (Сохранить).

### <a name="create-an-azure-ad-test-user"></a>Создание тестового пользователя Azure AD

Цель этого раздела — создать на портале Azure тестового пользователя с именем Britta Simon.

1. На портале Azure в области слева выберите **Azure Active Directory**, **Пользователи**, а затем — **Все пользователи**.

    ![Ссылки "Пользователи и группы" и "Все пользователи"](common/users.png)

2. В верхней части экрана выберите **Новый пользователь**.

    ![Кнопка "Новый пользователь"](common/new-user.png)

3. В разделе свойств пользователя сделайте следующее:

    ![Диалоговое окно "Пользователь"](common/user-properties.png)

    а. В поле **Имя** введите **BrittaSimon**.
  
    b. В поле **Имя пользователя** введите **brittasimon@yourcompanydomain.extension** .  
    Например BrittaSimon@contoso.com.

    c. Установите флажок **Показать пароль** и запишите значение, которое отображается в поле "Пароль".

    d. Нажмите кнопку **Создать**.

### <a name="assign-the-azure-ad-test-user"></a>Назначение тестового пользователя Azure AD

В этом разделе описано, как разрешить пользователю Britta Simon использовать единый вход Azure, предоставив доступ к SumoLogic.

1. На портале Azure последовательно выберите **Корпоративные приложения**, **Все приложения**, а затем — **SumoLogic**.

    ![Колонка "Корпоративные приложения"](common/enterprise-applications.png)

2. В списке приложений выберите **SumoLogic**.

    ![Ссылка на SumoLogic в списке приложений](common/all-applications.png)

3. В меню слева выберите **Пользователи и группы**.

    ![Ссылка "Пользователи и группы"](common/users-groups-blade.png)

4. Нажмите кнопку **Добавить пользователя**, а затем в диалоговом окне **Добавление назначения** выберите **Пользователи и группы**.

    ![Область "Добавление назначения"](common/add-assign-user.png)

5. В диалоговом окне **Пользователи и группы** из списка пользователей выберите **Britta Simon**, а затем в верхней части экрана нажмите кнопку **Выбрать**.

6. Если ожидается, что в утверждении SAML будет получено какое-либо значение роли, то в диалоговом окне **Выбор ролей** нужно выбрать соответствующую роль для пользователя из списка и затем нажать кнопку **Выбрать**, расположенную в нижней части экрана.

7. В диалоговом окне **Добавление назначения** нажмите кнопку **Назначить**.

### <a name="create-sumologic-test-user"></a>Создание тестового пользователя SumoLogic

Чтобы пользователи Azure AD могли входить в SumoLogic, их необходимо подготовить для SumoLogic. В случае SumoLogic подготовка пользователей осуществляется вручную.

**Чтобы подготовить учетную запись пользователя, сделайте следующее:**

1. Войдите в клиент **SumoLogic**.

1. Выберите **Manage \> Users** (Управление > Пользователи).

    ![Пользователи](./media/sumologic-tutorial/ic778561.png "Пользователи")

1. Щелкните **Добавить**.

    ![Пользователи](./media/sumologic-tutorial/ic778562.png "Пользователи")

1. В диалоговом окне **Новый пользователь** сделайте следующее:

    ![Новый пользователь](./media/sumologic-tutorial/ic778563.png "Новый пользователь") 

    a. Введите сведения об учетной записи Azure AD, которую необходимо подготовить, в текстовые поля **First Name** (Имя), **Last Name** (Фамилия) и **Email** (Адрес электронной почты).
  
    b. Выберите роль.
  
    c. Для параметра **Status** (Состояние) выберите значение **Active** (Активно).
  
    d. Выберите команду **Сохранить**.

> [!NOTE]
> Вы можете использовать любые другие средства создания учетной записи пользователя SumoLogic или API-интерфейсы, предоставляемые SumoLogic для подготовки учетных записей пользователя AAD.

### <a name="test-single-sign-on"></a>Проверка единого входа

В этом разделе описано, как проверить конфигурацию единого входа Azure AD с помощью панели доступа.

Щелкнув плитку SumoLogic на Панели доступа, вы автоматически войдете в приложение SumoLogic, для которого настроили единый вход. См. дополнительные сведения о [панели доступа](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)

## <a name="additional-resources"></a>Дополнительные ресурсы

- [Список учебников по интеграции приложений SaaS с Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Что такое условный доступ в Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

