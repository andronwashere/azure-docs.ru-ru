---
title: Руководство по Интеграция Azure Active Directory с Adobe Sign | Документация Майкрософт
description: Узнайте, как настроить единый вход Azure Active Directory в приложении Adobe Sign.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: f9385723-8fe7-4340-8afb-1508dac3e92b
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 12/19/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: bcb27e24e9b53b734a24304a63c8fd91d5e94f5d
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "67107328"
---
# <a name="tutorial-azure-active-directory-integration-with-adobe-sign"></a>Руководство по Интеграция Azure Active Directory с Adobe Sign

В этом руководстве описано, как интегрировать Adobe Sign с Azure Active Directory (Azure AD).
Интеграция Azure AD с Adobe Sign обеспечивает следующие преимущества:

* С помощью Azure AD вы можете контролировать доступ к Adobe Sign.
* Вы можете включить автоматический вход пользователей в Adobe Sign (единый вход) с помощью учетной записи Azure AD.
* Вы можете управлять учетными записями централизованно на портале Azure.

Дополнительные сведения об интеграции приложений SaaS с Azure AD см. в статье [Единый вход в приложениях в Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Если у вас еще нет подписки Azure, [создайте бесплатную учетную запись Azure](https://azure.microsoft.com/free/), прежде чем начинать работу.

## <a name="prerequisites"></a>Предварительные требования

Чтобы настроить интеграцию Azure AD с Adobe Sign, вам потребуется:

* подписка Azure AD (если у вас нет среды Azure AD, вы можете получить пробную версию на один месяц по [этой ссылке](https://azure.microsoft.com/pricing/free-trial/));
* Подписка Adobe Sign с поддержкой единого входа.

## <a name="scenario-description"></a>Описание сценария

В рамках этого руководства вы настроите и проверите единый вход Azure AD в тестовой среде.

* Adobe Sign поддерживает единый вход, инициированный **поставщиком услуг**.

## <a name="adding-adobe-sign-from-the-gallery"></a>Добавление Adobe Sign из коллекции

Чтобы настроить интеграцию Adobe Sign с Azure AD, необходимо добавить Adobe Sign из коллекции в список управляемых приложений SaaS.

**Чтобы добавить Adobe Sign из коллекции, сделайте следующее.**

1. На **[портале Azure](https://portal.azure.com)** в области навигации слева щелкните значок **Azure Active Directory**.

    ![Кнопка Azure Active Directory](common/select-azuread.png)

2. Перейдите в колонку **Корпоративные приложения** и выберите **Все приложения**.

    ![Колонка "Корпоративные приложения"](common/enterprise-applications.png)

3. Чтобы добавить новое приложение, в верхней части диалогового окна нажмите кнопку **Создать приложение**.

    ![Кнопка "Создать приложение"](common/add-new-app.png)

4. В поле поиска введите **Adobe Sign**, выберите **Adobe Sign** на панели результатов и нажмите кнопку **Добавить**, чтобы добавить это приложение.

     ![Adobe Sign в списке результатов](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Настройка и проверка единого входа в Azure AD

В этом разделе описана настройка и проверка единого входа Azure AD в Adobe Sign с использованием тестового пользователя **Britta Simon**.
Для обеспечения работы единого входа необходимо установить связь между пользователем Azure AD и соответствующим пользователем в Adobe Sign.

Чтобы настроить и проверить единый вход Azure AD в Adobe Sign, вам потребуется выполнить действия в следующих стандартных блоках.

1. **[Настройка единого входа Azure AD](#configure-azure-ad-single-sign-on)** необходима, чтобы пользователи могли использовать эту функцию.
2. **[Настройка единого входа в Adobe Sign](#configure-adobe-sign-single-sign-on)** необходима, чтобы настроить параметры единого входа на стороне приложения.
3. **[Создание тестового пользователя Azure AD](#create-an-azure-ad-test-user)** требуется для проверки работы единого входа Azure AD от имени пользователя Britta Simon.
4. **[Назначение тестового пользователя Azure AD](#assign-the-azure-ad-test-user)** необходимо, чтобы разрешить пользователю Britta Simon использовать единый вход Azure AD.
5. **[Создание тестового пользователя Adobe Sign](#create-adobe-sign-test-user)** нужно, чтобы в Adobe Sign также существовал пользователь Britta Simon, связанный с одноименным пользователем в Azure AD.
6. **[Проверка единого входа](#test-single-sign-on)** необходима, чтобы проверить работу конфигурации.

### <a name="configure-azure-ad-single-sign-on"></a>Настройка единого входа Azure AD

В этом разделе описано включение единого входа Azure AD на портале Azure.

Чтобы настроить единый вход Azure AD в Adobe Sign, сделайте следующее.

1. На [портале Azure](https://portal.azure.com/) на странице интеграции с приложением **Adobe Sign** щелкните **Единый вход**.

    ![Ссылка "Настройка единого входа"](common/select-sso.png)

2. В диалоговом окне **Выбрать метод единого входа** выберите режим **SAML/WS-Fed**, чтобы включить единый вход.

    ![Режим выбора единого входа](common/select-saml-option.png)

3. На странице **Настройка единого входа с помощью SAML** щелкните **Изменить**, чтобы открыть диалоговое окно **Базовая конфигурация SAML**.

    ![Правка базовой конфигурации SAML](common/edit-urls.png)

4. В разделе **Базовая конфигурация SAML** выполните приведенные ниже действия.

    ![Сведения о домене и URL-адресах для единого входа в приложении Adobe Sign](common/sp-identifier.png)

    a. В текстовом поле **URL-адрес для входа** введите URL-адрес в следующем формате: `https://<companyname>.echosign.com/`.

    b. В текстовом поле **Идентификатор (сущности)** введите URL-адрес в следующем формате: `https://<companyname>.echosign.com`.

    > [!NOTE]
    > Эти значения приведены для примера. Необходимо обновить эти значения действующим URL-адресом для входа и идентификатором. Чтобы получить эти значения, обратитесь к [группе поддержки Adobe Sign](https://helpx.adobe.com/in/contact/support.html). Можно также посмотреть шаблоны в разделе **Базовая конфигурация SAML** на портале Azure.

4. На странице **Настройка единого входа с помощью SAML** в разделе **Сертификат подписи SAML** щелкните **Загрузить**, чтобы загрузить требуемый **сертификат (Base64)** из предложенных вариантов, и сохраните его на компьютере.

    ![Ссылка для скачивания сертификата](common/certificatebase64.png)

6. Требуемый URL-адрес вы можете скопировать из раздела **Настройка Adobe Sign**.

    ![Копирование URL-адресов настройки](common/copy-configuration-urls.png)

    а) URL-адрес входа.

    б) Идентификатор Azure AD.

    в) URL-адрес выхода.

### <a name="configure-adobe-sign-single-sign-on"></a>Настройка единого входа в Adobe Sign

1. Перед настройкой необходимо обратиться к [группе поддержки клиентов Adobe Sign](https://helpx.adobe.com/in/contact/support.html), чтобы добавить свой домен в список разрешений Adobe Sign. Домен можно добавить следующим образом:

    a. [Группа поддержки клиентов Adobe Sign](https://helpx.adobe.com/in/contact/support.html) отправит случайно сгенерированный токен. Для вашего домена токен будет выглядеть примерно так: **adobe-sign-verification= xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx**

    b. Опубликуйте токен проверки в текстовой записи DNS и отправьте уведомление [группе поддержки клиентов Adobe Sign](https://helpx.adobe.com/in/contact/support.html).
    
    > [!NOTE]
    > Это может занять несколько дней или больше. Обратите внимание, что задержки распространения DNS означают, что значение, опубликованное в DNS, может не отображаться в течение часа или более. ИТ-администратор должен быть осведомлен о том, как публиковать этот токен в текстовой записи DNS.
    
    c. После уведомления [группы поддержки клиентов Adobe Sign](https://helpx.adobe.com/in/contact/support.html) с помощью запроса в службу поддержки, а также после публикации токена домен проверят и добавят его к учетной записи.
    
    d. В целом, чтобы опубликовать токен в записи DNS, сделайте следующее:

    * Войдите в учетную запись.
    * Найдите страницу для обновления записи DNS. Эта страница может называться "Управление DNS", "Управление именами сервера" или "Дополнительные параметры".
    * Найдите текстовые записи для домена.
    * Добавьте запись типа TXT с полным значением токена, предоставленным Adobe.
    * Сохраните изменения.

1. В другом окне веб-браузера войдите на свой корпоративный веб-сайт Adobe Sign в качестве администратора.

1. В меню SAML выберите **Параметры учетной записи** > **Параметры SAML**.
   
    ![Снимок экрана страницы параметров SAML на веб-сайте Adobe Sign](./media/adobe-echosign-tutorial/ic789520.png "Учетная запись")

1. В разделе **SAML Settings** (Параметры SAML) выполните следующие действия.
  
   ![Снимок экрана параметров SAML](./media/adobe-echosign-tutorial/ic789521.png "Параметры SAML")
   
   ![Снимок экрана параметров SAML](./media/adobe-echosign-tutorial/ic789522.png "Параметры SAML")

   a. В разделе **SAML Mode** (Режим SAML) выберите параметр **SAML Mandatory** (SAML обязательно).
   
   b. Установите флажок **Allow EchoSign Account Administrators to log in using their EchoSign Credentials** (Разрешить администраторам учетных записей EchoSign вход с использованием учетных данных EchoSign).
   
   c. В разделе **Создание пользователя** установите флажок **Automatically add users authenticated through SAML** (Автоматически добавлять пользователей, прошедших проверку подлинности с использованием SAML).

   d. Вставьте **идентификатор Azure AD**, скопированный на портале Azure, в текстовое поле **Idp Entity ID** (Идентификатор сущности поставщика удостоверений).
    
   д. Вставьте **URL-адрес входа**, скопированный на портале Azure, в текстовое поле **Idp Login URL** (URL-адрес входа поставщика удостоверений).
   
   Е. Вставьте **URL-адрес выхода**, скопированный на портале Azure, в текстовое поле **Idp Logout URL** (URL-адрес выхода поставщика удостоверений).

   ж. Откройте скачанный файл **сертификата (Base64)** в Блокноте. Скопируйте его содержимое в буфер обмена и вставьте в текстовое поле **сертификата поставщика удостоверений**.

   h. Щелкните **Save changes** (Сохранить изменения).

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

В этом разделе описано, как разрешить пользователю Britta Simon использовать единый вход Azure, предоставив этому пользователю доступ к Adobe Sign.

1. На портале Azure выберите **Корпоративные приложения**, **Все приложения**, а затем — **Adobe Sign**.

    ![Колонка "Корпоративные приложения"](common/enterprise-applications.png)

2. В списке приложений введите и выберите **Adobe Sign**.

    ![Ссылка на Adobe Sign в списке приложений](common/all-applications.png)

3. В меню слева выберите **Пользователи и группы**.

    ![Ссылка "Пользователи и группы"](common/users-groups-blade.png)

4. Нажмите кнопку **Добавить пользователя**, а затем в диалоговом окне **Добавление назначения** выберите **Пользователи и группы**.

    ![Область "Добавление назначения"](common/add-assign-user.png)

5. В диалоговом окне **Пользователи и группы** из списка пользователей выберите **Britta Simon**, а затем в верхней части экрана нажмите кнопку **Выбрать**.

6. Если ожидается, что в утверждении SAML будет получено какое-либо значение роли, то в диалоговом окне **Выбор ролей** нужно выбрать соответствующую роль для пользователя из списка и затем нажать кнопку **Выбрать**, расположенную в нижней части экрана.

7. В диалоговом окне **Добавление назначения** нажмите кнопку **Назначить**.

### <a name="create-adobe-sign-test-user"></a>Создание тестового пользователя Adobe Sign

Чтобы пользователи Azure AD могли выполнять вход в Adobe Sign, они должны быть подготовлены в Adobe Sign. Этот процесс выполняется в ручную.

>[!NOTE]
>Вы можете использовать любые другие инструменты создания учетной записи пользователя Adobe Sign или API, предоставляемые Adobe Sign для подготовки учетных записей пользователя Azure AD. 

1. Выполните вход на свой корпоративный веб-сайт **Adobe Sign** в качестве администратора.

2. В верхней области меню выберите **Учетная запись**. Затем в левой области выберите **Пользователи и группы** > **Создать нового пользователя**.
   
    ![Снимок экрана корпоративного веб-сайта Adobe Sign с выделенными параметрами "Учетная запись", "Пользователи и группы" и "Создать нового пользователя"](./media/adobe-echosign-tutorial/ic789524.png "Учетная запись")
   
3. В разделе **Создание нового пользователя** выполните следующие действия.
   
    ![Снимок экрана раздела "Создание нового пользователя"](./media/adobe-echosign-tutorial/ic789525.png "Создать пользователя")
   
    a. Введите в текстовые поля **Адрес электронной почты**, **Имя** и **Фамилия** соответствующие данные действующей учетной записи Azure AD, которую нужно подготовить.
   
    b. Нажмите кнопку **Создать пользователя**.

>[!NOTE]
>Владелец учетной записи Azure Active Directory получит электронное сообщение со ссылкой для подтверждения учетной записи перед ее активацией. 

### <a name="test-single-sign-on"></a>Проверка единого входа 

В этом разделе описано, как проверить конфигурацию единого входа Azure AD с помощью панели доступа.

Щелкнув плитку Adobe Sign на панели доступа, вы автоматически войдете в приложение Adobe Sign, для которого настроили единый вход. См. дополнительные сведения о [панели доступа](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)

## <a name="additional-resources"></a>Дополнительные ресурсы

- [Список учебников по интеграции приложений SaaS с Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Что такое условный доступ в Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

