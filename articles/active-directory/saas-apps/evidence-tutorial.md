---
title: Руководство по Интеграция Azure Active Directory с Evidence.com | Документация Майкрософт
description: Узнайте, как настроить единый вход Azure Active Directory в приложении Evidence.com.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: f9a7cb7c-ff67-40dc-872c-1fa35f9dd03b
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 02/07/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: ffee3b8fa1d152ce94513353c5c178adb5e3cc77
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "67102955"
---
# <a name="tutorial-azure-active-directory-integration-with-evidencecom"></a>Руководство по Интеграция Azure Active Directory с Evidence.com

В этом руководстве описано, как интегрировать Evidence.com с Azure Active Directory (Azure AD).
Интеграция Azure AD с приложением Evidence.com обеспечивает следующие преимущества:

* С помощью Azure AD вы можете контролировать доступ к Evidence.com.
* Вы можете включить автоматический вход пользователей в Evidence.com (единый вход) с помощью учетной записи Azure AD.
* Вы можете управлять учетными записями централизованно на портале Azure.

Дополнительные сведения об интеграции приложений SaaS с Azure AD см. в статье [Единый вход в приложениях в Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Если у вас еще нет подписки Azure, [создайте бесплатную учетную запись Azure](https://azure.microsoft.com/free/), прежде чем начинать работу.

## <a name="prerequisites"></a>Предварительные требования

Чтобы настроить интеграцию Azure AD с Evidence.com, вам потребуется:

* подписка Azure AD (если у вас нет среды Azure AD, вы можете получить пробную версию на один месяц по [этой ссылке](https://azure.microsoft.com/pricing/free-trial/));
* подписка Evidence.com с поддержкой единого входа.

## <a name="scenario-description"></a>Описание сценария

В рамках этого руководства вы настроите и проверите единый вход Azure AD в тестовой среде.

* Evidence.com поддерживает единый вход инициированного **пакета обновления**.

## <a name="adding-evidencecom-from-the-gallery"></a>Добавление Evidence.com из коллекции

Чтобы настроить интеграцию Evidence.com с Azure AD, необходимо добавить Evidence.com из коллекции в список управляемых приложений SaaS.

**Чтобы добавить Evidence.com из коллекции, сделайте следующее:**

1. На **[портале Azure](https://portal.azure.com)** в области навигации слева щелкните значок **Azure Active Directory**.

    ![Кнопка Azure Active Directory](common/select-azuread.png)

2. Перейдите в колонку **Корпоративные приложения** и выберите **Все приложения**.

    ![Колонка "Корпоративные приложения"](common/enterprise-applications.png)

3. Чтобы добавить новое приложение, в верхней части диалогового окна нажмите кнопку **Создать приложение**.

    ![Кнопка "Создать приложение"](common/add-new-app.png)

4. В поле поиска введите **Evidence.com**, на панели результатов выберите **Evidence.com** и нажмите кнопку **Добавить**, чтобы добавить это приложение.

     ![Evidence.com в списке результатов](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Настройка и проверка единого входа в Azure AD

В этом разделе описана настройка и проверка единого входа Azure AD в Evidence.com с использованием тестового пользователя **Britta Simon**.
Для работы единого входа необходимо установить связь между пользователем Azure AD и одноименным пользователем в Evidence.com.

Чтобы настроить и проверить единый вход Azure AD в Evidence.com, необходимо выполнить действия в следующих стандартных блоках.

1. **[Настройка единого входа Azure AD](#configure-azure-ad-single-sign-on)** необходима, чтобы пользователи могли использовать эту функцию.
2. **[Настройка единого входа в Evidence.com](#configure-evidencecom-single-sign-on)** необходима, чтобы настроить параметры единого входа на стороне приложения.
3. **[Создание тестового пользователя Azure AD](#create-an-azure-ad-test-user)** требуется для проверки работы единого входа Azure AD от имени пользователя Britta Simon.
4. **[Назначение тестового пользователя Azure AD](#assign-the-azure-ad-test-user)** необходимо, чтобы разрешить пользователю Britta Simon использовать единый вход Azure AD.
5. **[Создание тестового пользователя Evidence.com](#create-evidencecom-test-user)** требуется для того, чтобы в Evidence.com существовал пользователь Britta Simon, связанный с одноименным пользователем в Azure AD.
6. **[Проверка единого входа](#test-single-sign-on)** необходима, чтобы проверить работу конфигурации.

### <a name="configure-azure-ad-single-sign-on"></a>Настройка единого входа Azure AD

В этом разделе описано включение единого входа Azure AD на портале Azure.

Чтобы настроить единый вход Azure AD в Evidence.com, сделайте следующее:

1. На [портале Azure ](https://portal.azure.com/)на странице интеграции с приложением **Evidence.com** выберите **Единый вход**.

    ![Ссылка "Настройка единого входа"](common/select-sso.png)

2. В диалоговом окне **Выбрать метод единого входа** выберите режим **SAML/WS-Fed**, чтобы включить единый вход.

    ![Режим выбора единого входа](common/select-saml-option.png)

3. На странице **Настройка единого входа с помощью SAML** щелкните **Изменить**, чтобы открыть диалоговое окно **Базовая конфигурация SAML**.

    ![Правка базовой конфигурации SAML](common/edit-urls.png)

4. В разделе **Базовая конфигурация SAML** выполните приведенные ниже действия.

    ![Сведения о домене и URL-адресах единого входа для приложения Evidence.com](common/sp-identifier.png)

    a. В текстовом поле **URL-адрес для входа** введите URL-адрес в следующем формате: `https://<yourtenant>.evidence.com`.

    b. В текстовом поле **Идентификатор (сущности)** введите URL-адрес в следующем формате: `https://<yourtenant>.evidence.com`.

    > [!NOTE]
    > Эти значения приведены для примера. Необходимо обновить эти значения действующим URL-адресом для входа и идентификатором. Чтобы получить их, обратитесь в [службу поддержки клиентов Evidence.com](https://communities.taser.com/support/SupportContactUs?typ=LE). Можно также посмотреть шаблоны в разделе **Базовая конфигурация SAML** на портале Azure.

5. На странице **Настройка единого входа с помощью SAML** в разделе **Сертификат подписи SAML** щелкните **Загрузить**, чтобы загрузить требуемый **сертификат (Base64)** из предложенных вариантов, и сохраните его на компьютере.

    ![Ссылка для скачивания сертификата](common/certificatebase64.png)

6. Требуемый URL-адрес можно скопировать из раздела **настройки Evidence.com**.

    ![Копирование URL-адресов настройки](common/copy-configuration-urls.png)

    а) URL-адрес входа.

    b. Идентификатор Azure AD

    c. URL-адрес выхода.

### <a name="configure-evidencecom-single-sign-on"></a>Настройка единого входа в Evidence.com

1. В отдельном окне браузера войдите в клиент Evidence.com с правами администратора и перейдите к вкладке **Admin** (Администрирование).

2. Щелкните **Agency Single Sign On**

3. Выберите параметр **SAML Based Single Sign On**

4. Скопируйте **идентификатор Azure AD**, **URL-адрес входа** и **URL-адрес выхода**, отображаемые на портале Azure, и добавьте их в соответствующие поля в приложении Evidence.com.

5. Откройте скачанный файл сертификата в кодировке Base64 в Блокноте, скопируйте его содержимое в буфер обмена, а затем вставьте его в поле **Security Certificate** (Сертификат безопасности). 

6. Сохраните конфигурацию в Evidence.com.

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

В этом разделе описано, как разрешить пользователю Britta Simon использовать единый вход Azure, предоставив этому пользователю доступ к Evidence.com.

1. На портале Azure выберите **Корпоративные приложения**, **Все приложения**, а затем — **Evidence.com**.

    ![Колонка "Корпоративные приложения"](common/enterprise-applications.png)

2. В списке приложений выберите **Evidence.com**.

    ![Ссылка Evidence.com в списке приложений](common/all-applications.png)

3. В меню слева выберите **Пользователи и группы**.

    ![Ссылка "Пользователи и группы"](common/users-groups-blade.png)

4. Нажмите кнопку **Добавить пользователя**, а затем в диалоговом окне **Добавление назначения** выберите **Пользователи и группы**.

    ![Область "Добавление назначения"](common/add-assign-user.png)

5. В диалоговом окне **Пользователи и группы** из списка пользователей выберите **Britta Simon**, а затем в верхней части экрана нажмите кнопку **Выбрать**.

6. Если ожидается, что в утверждении SAML будет получено какое-либо значение роли, то в диалоговом окне **Выбор ролей** нужно выбрать соответствующую роль для пользователя из списка и затем нажать кнопку **Выбрать**, расположенную в нижней части экрана.

7. В диалоговом окне **Добавление назначения** нажмите кнопку **Назначить**.

### <a name="create-evidencecom-test-user"></a>Создание тестового пользователя Evidence.com

Чтобы пользователи Azure AD могли выполнять вход, нужно подготовить их для доступа в приложении Evidence.com. В этом разделе описано, как создавать учетные записи пользователей Azure AD в Evidence.com.

**Чтобы подготовить учетную запись пользователя в Evidence.com, выполните следующие действия.**

1. В окне веб-браузера войдите на корпоративный веб-сайт Evidence.com в качестве администратора.

2. Перейдите к вкладке **Admin** (Администрирование).

3. Щелкните **Add User**(Добавить пользователя).

4. Нажмите кнопку **Add** (Добавить).

5. Параметр **Email Address** (Адрес электронной почты) для нового пользователя должен совпадать с именем пользователя в Azure AD, которому вы хотите предоставить доступ. Возможно, в вашей организации в качестве имени пользователя не используется адрес электронной почты. Тогда зайдите на портале Azure в раздел **Evidence.com > Атрибуты > Единый вход** и измените параметр nameidentifier (идентификатор имени, отправляемый приложению Evidence.com) так, чтобы передавался именно адрес электронной почты.

### <a name="test-single-sign-on"></a>Проверка единого входа 

В этом разделе описано, как проверить конфигурацию единого входа Azure AD с помощью панели доступа.

Щелкните плитку Evidence.com на панели доступа, чтобы автоматически войти в приложение Evidence.com, для которого вы настроили единый вход. См. дополнительные сведения о [панели доступа](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)

## <a name="additional-resources"></a>Дополнительные ресурсы

- [Список учебников по интеграции приложений SaaS с Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Что такое условный доступ в Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

