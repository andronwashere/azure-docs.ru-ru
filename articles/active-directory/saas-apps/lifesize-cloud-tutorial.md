---
title: Руководство по Интеграция Azure Active Directory с Lifesize Cloud | Документация Майкрософт
description: Узнайте, как настроить единый вход между Azure Active Directory и Lifesize Cloud.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: 75fab335-fdcd-4066-b42c-cc738fcb6513
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 1/4/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: f751eb9dfb8ef65bea80993ddbd3a682b1d47111
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "67098058"
---
# <a name="tutorial-azure-active-directory-integration-with-lifesize-cloud"></a>Руководство по Интеграция Azure Active Directory с Lifesize Cloud

В этом учебнике описано, как интегрировать приложение Lifesize Cloud с Azure Active Directory (Azure AD).
Интеграция Azure AD с приложением Lifesize Cloud обеспечивает следующие преимущества:

* С помощью Azure AD вы можете контролировать доступ к Lifesize Cloud.
* Вы можете включить автоматический вход для пользователей в Lifesize Cloud (единый вход) с использованием учетных записей Azure AD.
* Вы можете управлять учетными записями централизованно на портале Azure.

Дополнительные сведения об интеграции приложений SaaS с Azure AD см. в статье [Единый вход в приложениях в Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Если у вас еще нет подписки Azure, [создайте бесплатную учетную запись Azure](https://azure.microsoft.com/free/), прежде чем начинать работу.

## <a name="prerequisites"></a>Предварительные требования

Чтобы настроить интеграцию Azure AD с приложением Lifesize Cloud, вам потребуется:

* подписка Azure AD (если у вас нет среды Azure AD, вы можете получить пробную версию на один месяц по [этой ссылке](https://azure.microsoft.com/pricing/free-trial/));
* Подписка Lifesize Cloud с поддержкой единого входа.

## <a name="scenario-description"></a>Описание сценария

В рамках этого руководства вы настроите и проверите единый вход Azure AD в тестовой среде.

* Lifesize Cloud поддерживает единый вход инициированного **пакета обновления**.

* Lifesize Cloud поддерживает **автоматическую** подготовку пользователей.

## <a name="adding-lifesize-cloud-from-the-gallery"></a>Добавление Lifesize Cloud из коллекции

Чтобы настроить интеграцию Lifesize Cloud с Azure AD, необходимо добавить Lifesize Cloud из коллекции в список управляемых приложений SaaS.

**Чтобы добавить Lifesize Cloud из коллекции, выполните следующие действия:**

1. На **[портале Azure](https://portal.azure.com)** в области навигации слева щелкните значок **Azure Active Directory**.

    ![Кнопка Azure Active Directory](common/select-azuread.png)

2. Перейдите в колонку **Корпоративные приложения** и выберите **Все приложения**.

    ![Колонка "Корпоративные приложения"](common/enterprise-applications.png)

3. Чтобы добавить новое приложение, в верхней части диалогового окна нажмите кнопку **Создать приложение**.

    ![Кнопка "Создать приложение"](common/add-new-app.png)

4. В поле поиска введите **Lifesize Cloud**, выберите **Lifesize Cloud** на панели результатов и нажмите кнопку **Добавить**, чтобы добавить это приложение.

     ![Lifesize Cloud в списке результатов](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Настройка и проверка единого входа в Azure AD

В этом разделе описана настройка и проверка единого входа Azure AD с Lifesize Cloud с использованием тестового пользователя **Britta Simon**.
Для обеспечения работы единого входа необходимо установить связь между пользователем Azure AD и соответствующим пользователем в Lifesize Cloud.

Чтобы настроить и проверить единый вход Azure AD в Lifesize Cloud, выполните действия в следующих стандартных блоках:

1. **[Настройка единого входа Azure AD](#configure-azure-ad-single-sign-on)** необходима, чтобы пользователи могли использовать эту функцию.
2. **[Настройка единого входа в Lifesize Cloud](#configure-lifesize-cloud-single-sign-on)** необходима, чтобы настроить параметры единого входа на стороне приложения.
3. **[Создание тестового пользователя Azure AD](#create-an-azure-ad-test-user)** требуется для проверки работы единого входа Azure AD от имени пользователя Britta Simon.
4. **[Назначение тестового пользователя Azure AD](#assign-the-azure-ad-test-user)** необходимо, чтобы разрешить пользователю Britta Simon использовать единый вход Azure AD.
5. **[Создание тестового пользователя Lifesize Cloud](#create-lifesize-cloud-test-user)** требуется для создания в Lifesize Cloud пользователя Britta Simon, связанного с соответствующим пользователем в Azure AD.
6. **[Проверка единого входа](#test-single-sign-on)** необходима, чтобы проверить работу конфигурации.

### <a name="configure-azure-ad-single-sign-on"></a>Настройка единого входа Azure AD

В этом разделе описано включение единого входа Azure AD на портале Azure.

Чтобы настроить единый вход Azure AD в Lifesize Cloud, выполните следующие действия:

1. На [портале Azure](https://portal.azure.com/) на странице интеграции с приложением **Lifesize Cloud** выберите **Единый вход**.

    ![Ссылка "Настройка единого входа"](common/select-sso.png)

2. В диалоговом окне **Выбрать метод единого входа** выберите режим **SAML/WS-Fed**, чтобы включить единый вход.

    ![Режим выбора единого входа](common/select-saml-option.png)

3. На странице **Настройка единого входа с помощью SAML** щелкните **Изменить**, чтобы открыть диалоговое окно **Базовая конфигурация SAML**.

    ![Правка базовой конфигурации SAML](common/edit-urls.png)

4. В разделе **Базовая конфигурация SAML** выполните приведенные ниже действия.

    ![Сведения о домене и URL-адресах единого входа Lifesize Cloud](common/sp-identifier-relay.png)

    a. В текстовое поле **URL-адрес для входа** введите URL-адрес в следующем формате: `https://login.lifesizecloud.com/ls/?acs`.

    b. В текстовом поле **Идентификатор** введите URL-адрес в формате `https://login.lifesizecloud.com/<companyname>`.

    c. Щелкните **Задать дополнительные URL-адреса**.

    d. В текстовом поле **Состояние ретранслятора** введите URL-адрес в формате `https://webapp.lifesizecloud.com/?ent=<identifier>`.

    > [!NOTE]
    > Эти значения приведены для примера. Замените эти значения фактическим URL-адресом входа, идентификатором и состоянием ретранслятора. Обратитесь в [группу поддержки клиентов Lifesize Cloud](https://www.lifesize.com/en/support) для получения URL-адреса входа и значений идентификаторов, а также значения состояния ретрансляции из конфигурации единого входа, которое описывается далее в этом руководстве. Можно также посмотреть шаблоны в разделе **Базовая конфигурация SAML** на портале Azure.

5. На странице **Настройка единого входа с помощью SAML** в разделе **Сертификат подписи SAML** щелкните **Загрузить**, чтобы загрузить требуемый **сертификат (Base64)** из предложенных вариантов, и сохраните его на компьютере.

    ![Ссылка для скачивания сертификата](common/certificatebase64.png)

6. Скопируйте требуемый URL-адрес из раздела **Настройка Lifesize Cloud**.

    ![Копирование URL-адресов настройки](common/copy-configuration-urls.png)

    а) URL-адрес входа.

    б) Идентификатор Azure AD.

    в) URL-адрес выхода.

### <a name="configure-lifesize-cloud-single-sign-on"></a>Настройка единого входа в Lifesize Cloud

1. Для настройки единого входа в вашем приложении необходимо войти в приложение Lifesize Cloud с правами администратора.

2. В правом верхнем углу страницы щелкните свое имя и выберите **Advance Settings** (Дополнительные параметры).

    ![Настройка единого входа](./media/lifesize-cloud-tutorial/tutorial_lifesizecloud_06.png)

3. В меню Advance Settings (Дополнительные параметры) щелкните ссылку **SSO Configuration** (Конфигурация единого входа). Откроется страница "SSO Configuration" (Конфигурация единого входа) для вашего экземпляра.

    ![Настройка единого входа](./media/lifesize-cloud-tutorial/tutorial_lifesizecloud_07.png)

4. Теперь настройте следующие значения в пользовательском интерфейсе единого входа.

    ![Настройка единого входа](./media/lifesize-cloud-tutorial/tutorial_lifesizecloud_08.png)

    a. В текстовое поле **Издатель поставщика удостоверений** вставьте значение **идентификатора Azure AD**, скопированное на портале Azure.

    b.  В текстовое поле **Login URL** (URL-адрес входа) вставьте значение **URL-адреса входа**, скопированное с портала Azure.

    c. Откройте в блокноте сертификат в кодировке Base-64, скачанный с портала Azure, скопируйте его в буфер обмена и вставьте в текстовое поле **Сертификат X.509**.
  
    d. В разделе "SAML Attribute Mappings" (Сопоставления атрибутов SAML) в текстовом поле "First Name" (Имя) введите значение в формате `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname`

    д. В разделе "SAML Attribute Mappings" (Сопоставления атрибутов SAML) в текстовом поле **Last Name** (Фамилия) введите значение в формате `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname`

    Е. В разделе "SAML Attribute Mappings" (Сопоставления атрибутов SAML) в текстовом поле **Адрес электронной почты** введите значение в формате `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`

5. Чтобы проверить конфигурацию, можно нажать кнопку **Test** (Проверить).

    >[!NOTE]
    >Чтобы проверка прошла успешно, необходимо завершить работу мастера настройки в Azure AD, а также предоставить доступ пользователям или группам, которые будут выполнять эту проверку.

6. Чтобы включить единый вход, установите флажок **Enable SSO** (Включить единый вход).

7. Затем нажмите кнопку **Update** (Обновить), чтобы сохранить все параметры. При этом будет создано значение RelayState. Скопируйте значение RelayState, созданное в текстовом поле, и вставьте его в поле **Состояние ретранслятора** в разделе **URL-адреса и домены Lifesize Cloud**.

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

В этом разделе описано, как разрешить пользователю Britta Simon использовать единый вход Azure путем предоставления доступа к Lifesize Cloud.

1. На портале Azure выберите **Корпоративные приложения**, **Все приложения**, а затем — **Lifesize Cloud**.

    ![Колонка "Корпоративные приложения"](common/enterprise-applications.png)

2. В списке приложений выберите **Lifesize Cloud**.

    ![Ссылка на Lifesize Cloud в списке приложений](common/all-applications.png)

3. В меню слева выберите **Пользователи и группы**.

    ![Ссылка "Пользователи и группы"](common/users-groups-blade.png)

4. Нажмите кнопку **Добавить пользователя**, а затем в диалоговом окне **Добавление назначения** выберите **Пользователи и группы**.

    ![Область "Добавление назначения"](common/add-assign-user.png)

5. В диалоговом окне **Пользователи и группы** из списка пользователей выберите **Britta Simon**, а затем в верхней части экрана нажмите кнопку **Выбрать**.

6. Если ожидается, что в утверждении SAML будет получено какое-либо значение роли, то в диалоговом окне **Выбор ролей** нужно выбрать соответствующую роль для пользователя из списка и затем нажать кнопку **Выбрать**, расположенную в нижней части экрана.

7. В диалоговом окне **Добавление назначения** нажмите кнопку **Назначить**.

### <a name="create-lifesize-cloud-test-user"></a>Создание тестового пользователя Lifesize Cloud

В этом разделе описано, как создать пользователя Britta Simon в приложении Lifesize Cloud. Lifesize Cloud поддерживает автоматическую подготовку пользователей. После успешной аутентификации в Azure AD для пользователя будет автоматически выполняться подготовка в приложении.

### <a name="test-single-sign-on"></a>Проверка единого входа

В этом разделе описано, как проверить конфигурацию единого входа Azure AD с помощью панели доступа.

После щелчка элемента "Lifesize Cloud" на панели доступа откроется страница входа в приложение Lifesize Cloud. Здесь введите свое имя пользователя, после этого вы будете перенаправлены на домашнюю страницу приложения.

См. дополнительные сведения о [панели доступа](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)

## <a name="additional-resources"></a>Дополнительные ресурсы

- [Список учебников по интеграции приложений SaaS с Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Что такое условный доступ в Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
