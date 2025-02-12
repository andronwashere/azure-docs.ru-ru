---
title: Руководство по Интеграция Azure Active Directory с Riskware | Документация Майкрософт
description: Узнайте, как настроить единый вход между Azure Active Directory и Riskware.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 81866167-b163-4695-8978-fd29a25dac7a
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/25/2019
ms.author: jeedes
ms.openlocfilehash: b2bfbed33433521fd086d474ea4b754f5435f5e6
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "67092924"
---
# <a name="tutorial-azure-active-directory-integration-with-riskware"></a>Руководство по Интеграция Azure Active Directory с Riskware

В этом руководстве описано, как интегрировать Riskware с Azure Active Directory (Azure AD).
Интеграция Azure AD с приложением Riskware обеспечивает следующие преимущества:

* С помощью Azure AD вы можете контролировать доступ к Riskware.
* Вы можете включить автоматический вход пользователей в Riskware (единый вход) с помощью учетных записей Azure AD.
* Вы можете управлять учетными записями централизованно на портале Azure.

Дополнительные сведения об интеграции приложений SaaS с Azure AD см. в статье [Единый вход в приложениях в Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Если у вас еще нет подписки Azure, [создайте бесплатную учетную запись Azure](https://azure.microsoft.com/free/), прежде чем начинать работу.

## <a name="prerequisites"></a>Предварительные требования

Чтобы настроить интеграцию Azure AD с Riskware, вам потребуется следующее:

* подписка Azure AD (если у вас нет среды Azure AD, вы можете получить [бесплатную учетную запись](https://azure.microsoft.com/free/));
* подписка Riskware с поддержкой единого входа.

## <a name="scenario-description"></a>Описание сценария

В рамках этого руководства вы настроите и проверите единый вход Azure AD в тестовой среде.

* Riskware поддерживает единый вход, инициируемый **поставщиком услуг**.

## <a name="adding-riskware-from-the-gallery"></a>Добавление Riskware из коллекции

Чтобы настроить интеграцию Riskware с Azure AD, необходимо добавить Riskware из коллекции в список управляемых приложений SaaS.

**Чтобы добавить Riskware из коллекции, выполните следующие действия:**

1. На **[портале Azure](https://portal.azure.com)** в области навигации слева щелкните значок **Azure Active Directory**.

    ![Кнопка Azure Active Directory](common/select-azuread.png)

2. Перейдите в колонку **Корпоративные приложения** и выберите **Все приложения**.

    ![Колонка "Корпоративные приложения"](common/enterprise-applications.png)

3. Чтобы добавить новое приложение, в верхней части диалогового окна нажмите кнопку **Создать приложение**.

    ![Кнопка "Создать приложение"](common/add-new-app.png)

4. В поле поиска введите **Riskware**, выберите **Riskware** на панели результатов и нажмите кнопку **Добавить**, чтобы добавить это приложение.

    ![Riskware в списке результатов](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Настройка и проверка единого входа в Azure AD

В этом разделе описана настройка и проверка единого входа Azure AD в Riskware с использованием тестового пользователя **Britta Simon**.
Для обеспечения работы единого входа необходимо установить связь между пользователем Azure AD и соответствующим пользователем в Riskware.

Чтобы настроить и проверить единый вход Azure AD в Riskware, вам потребуется выполнить действия в следующих стандартных блоках.

1. **[Настройка единого входа Azure AD](#configure-azure-ad-single-sign-on)** необходима, чтобы пользователи могли использовать эту функцию.
2. **[Настройка единого входа в Riskware](#configure-riskware-single-sign-on)** необходима, чтобы настроить параметры единого входа на стороне приложения.
3. **[Создание тестового пользователя Azure AD](#create-an-azure-ad-test-user)** требуется для проверки работы единого входа Azure AD от имени пользователя Britta Simon.
4. **[Назначение тестового пользователя Azure AD](#assign-the-azure-ad-test-user)** необходимо, чтобы разрешить пользователю Britta Simon использовать единый вход Azure AD.
5. **[Создание тестового пользователя приложения Riskware](#create-riskware-test-user)** требуется для того, чтобы в Riskware существовал пользователь Britta Simon, связанный с одноименным пользователем в Azure AD.
6. **[Проверка единого входа](#test-single-sign-on)** необходима, чтобы проверить работу конфигурации.

### <a name="configure-azure-ad-single-sign-on"></a>Настройка единого входа Azure AD

В этом разделе описано включение единого входа Azure AD на портале Azure.

Чтобы настроить единый вход Azure AD в Riskware, выполните следующие действия.

1. На [портале Azure](https://portal.azure.com/) на странице интеграции с приложением **Riskware** щелкните **Единый вход**.

    ![Ссылка "Настройка единого входа"](common/select-sso.png)

2. В диалоговом окне **Выбрать метод единого входа** выберите режим **SAML/WS-Fed**, чтобы включить единый вход.

    ![Режим выбора единого входа](common/select-saml-option.png)

3. На странице **Настройка единого входа с помощью SAML** щелкните **Изменить**, чтобы открыть диалоговое окно **Базовая конфигурация SAML**.

    ![Правка базовой конфигурации SAML](common/edit-urls.png)

4. В разделе **Базовая конфигурация SAML** выполните приведенные ниже действия.

    ![Сведения о домене и URL-адресах единого входа для приложения Riskware](common/sp-identifier.png)

    a. В текстовом поле **URL-адрес для входа** введите URL-адрес в следующем формате:
    
    | Среда| Шаблон URL-адреса|
    |--|--|
    | UAT|  `https://riskcloud.net/uat?ccode=<COMPANYCODE>` |
    | PROD| `https://riskcloud.net/prod?ccode=<COMPANYCODE>` |
    | DEMO| `https://riskcloud.net/demo?ccode=<COMPANYCODE>` |
    |||

    b. В текстовое поле **Идентификатор (сущности)** введите URL-адрес:
    
    | Среда| Шаблон URL-адреса|
    |--|--|
    | UAT| `https://riskcloud.net/uat` |
    | PROD| `https://riskcloud.net/prod` |
    | DEMO| `https://riskcloud.net/demo` |
    |||

    > [!NOTE]
    > Значение URL-адреса входа приведено только для примера. Вместо него необходимо указать фактический URL-адрес входа. Чтобы получить это значение, обратитесь к [группе поддержки Riskware](mailto:support@pansoftware.com.au). Можно также посмотреть шаблоны в разделе **Базовая конфигурация SAML** на портале Azure.

5. На странице **Настройка единого входа с помощью SAML** в разделе **Сертификат подписи SAML** щелкните **Скачать**, чтобы скачать нужный вам **XML метаданных федерации**, и сохраните его на компьютере.

    ![Ссылка для скачивания сертификата](common/metadataxml.png)

6. Требуемые URL-адреса можно скопировать из раздела **Настройка Riskware**.

    ![Копирование URL-адресов настройки](common/copy-configuration-urls.png)

    а) URL-адрес входа.

    b. Идентификатор Azure AD

    c. URL-адрес выхода.

### <a name="configure-riskware-single-sign-on"></a>Настройка единого входа в Riskware

1. В другом окне веб-браузера войдите на корпоративный сайт Riskware в качестве администратора.

1. В правом верхнем углу щелкните **Maintenance** (Обслуживание), чтобы открыть страницу обслуживания.

    ![Настройка Riskware, раздел "Обслуживание"](./media/riskware-tutorial/tutorial_riskware_maintain.png)

1. На странице обслуживания щелкните **Authentication** (Аутентификация).

    ![Настройка Riskware, аутентификация](./media/riskware-tutorial/tutorial_riskware_authen.png)

1. На странице **Authentication Configuration** (Настройка аутентификации) выполните следующие действия:

    ![Настройка Riskware, настройка аутентификации](./media/riskware-tutorial/tutorial_riskware_config.png)

    a. Выберите для параметра аутентификации **Type** (Тип) значение **SAML**.

    b. В текстовом поле **Code** (Код) введите код, например AZURE_UAT.

    c. В текстовом поле **Description** (Описание) введите описание, например "Конфигурация AZURE для единого входа".

    d. В текстовое поле **Single Sign On Page** (Страница единого входа) вставьте значение **URL-адрес входа**, скопированное на портале Azure.

    д. В текстовое поле **Sign out Page** (Страница выхода) вставьте значение **URL-адрес выхода**, скопированное на портале Azure.

    Е. В текстовое поле **Post Form Field** (Поле формы POST) введите имя поля из ответа POST, которое содержит значение SAML, например SAMLResponse.

    ж. В текстовом поле **XML Identity Tag Name** (Имя тега идентификатора XML) введите имя атрибута ответа SAML, который содержит уникальный идентификатор, например NameID.

    h. Откройте в Блокноте скачанный с портала Azure  **XML-файл метаданных** и скопируйте из этого файла сертификат безопасности. Затем вставьте его в текстовое поле **Certificate** (Сертификат).

    i. В текстовом поле **Consumer URL** (URL-адрес объекта-получателя) вставьте значение **URL-адреса ответа**, которое вы получили от службы поддержки.

    j. В текстовом поле **Issuer** (Издатель) вставьте значение **идентификатора**, которое вы получили от службы поддержки.

    > [!Note]
    > Чтобы получить это значения, обратитесь к [службе поддержки клиентов Riskware](mailto:support@pansoftware.com.au).

    k. Установите флажок **Use POST** (Использовать POST).

    l. Установите флажок **Use SAML Request** (Использовать запрос SAML).

    m. Выберите команду **Сохранить**.

### <a name="create-an-azure-ad-test-user"></a>Создание тестового пользователя Azure AD

Цель этого раздела — создать на портале Azure тестового пользователя с именем Britta Simon.

1. На портале Azure в области слева выберите **Azure Active Directory**, **Пользователи**, а затем — **Все пользователи**.

    ![Ссылки "Пользователи и группы" и "Все пользователи"](common/users.png)

2. В верхней части экрана выберите **Новый пользователь**.

    ![Кнопка "Новый пользователь"](common/new-user.png)

3. В разделе свойств пользователя сделайте следующее:

    ![Диалоговое окно "Пользователь"](common/user-properties.png)

    а. В поле **Имя** введите **BrittaSimon**.
  
    b. В поле **Имя пользователя** введите `brittasimon@yourcompanydomain.extension`.  
    Например BrittaSimon@contoso.com.

    c. Установите флажок **Показать пароль** и запишите значение, которое отображается в поле "Пароль".

    d. Нажмите кнопку **Создать**.

### <a name="assign-the-azure-ad-test-user"></a>Назначение тестового пользователя Azure AD

В этом разделе описано, как разрешить пользователю Britta Simon использовать единый вход Azure, предоставив этому пользователю доступ к Riskware.

1. На портале Azure выберите **Корпоративные приложения**, **Все приложения**, а затем — **Riskware**.

    ![Колонка "Корпоративные приложения"](common/enterprise-applications.png)

2. Из списка приложений выберите **Riskware**.

    ![Ссылка на Riskware в списке "Приложения"](common/all-applications.png)

3. В меню слева выберите **Пользователи и группы**.

    ![Ссылка "Пользователи и группы"](common/users-groups-blade.png)

4. Нажмите кнопку **Добавить пользователя**, а затем в диалоговом окне **Добавление назначения** выберите **Пользователи и группы**.

    ![Область "Добавление назначения"](common/add-assign-user.png)

5. В диалоговом окне **Пользователи и группы** из списка пользователей выберите **Britta Simon**, а затем в верхней части экрана нажмите кнопку **Выбрать**.

6. Если ожидается, что в утверждении SAML будет получено какое-либо значение роли, то в диалоговом окне **Выбор ролей** нужно выбрать соответствующую роль для пользователя из списка и затем нажать кнопку **Выбрать**, расположенную в нижней части экрана.

7. В диалоговом окне **Добавление назначения** нажмите кнопку **Назначить**.

### <a name="create-riskware-test-user"></a>Создание тестового пользователя Riskware

Чтобы пользователи Azure AD могли выполнять вход в Riskware, они должны быть подготовлены для Riskware. В Riskware подготовка выполняется вручную.

**Чтобы подготовить учетную запись пользователя, сделайте следующее:**

1. Войдите в Riskware с правами администратора безопасности.

1. В правом верхнем углу щелкните **Maintenance** (Обслуживание), чтобы открыть страницу обслуживания. 

    ![Настройка Riskware, обслуживание](./media/riskware-tutorial/tutorial_riskware_maintain.png)

1. На странице обслуживания щелкните **People** (Пользователи).

    ![Настройка Riskware, пользователи](./media/riskware-tutorial/tutorial_riskware_people.png)

1. На вкладке **Details** (Подробные сведения) сделайте следующее:

    ![Настройка Riskware, подробные сведения](./media/riskware-tutorial/tutorial_riskware_details.png)

    a. Выберите **Person Type** (Тип роли), например Employee (Сотрудник).

    b. В текстовое поле **First Name** (Имя) введите имя, например **Britta**.

    c. В текстовое поле **Surname** (Фамилия) введите фамилию, например **Simon**.

1. На вкладке **Безопасность** сделайте следующее:

    ![Настройка Riskware, безопасность](./media/riskware-tutorial/tutorial_riskware_security.png)

    a. В разделе **Authentication** (Аутентификация) выберите настроенный ранее **режим аутентификации**, например "Конфигурация AZURE для единого входа".

    b. В разделе **Logon Details** (Данные для входа) введите в текстовое поле **User ID** (Идентификатор пользователя) адрес электронной почты пользователя, например `brittasimon@contoso.com`.

    c. В текстовом поле **Password** (Пароль) введите пароль этого пользователя.

1. На вкладке **Organisation** (Организация) сделайте следующее:

    ![Настройка Riskware, организация](./media/riskware-tutorial/tutorial_riskware_org.png)

    a. Выберите параметр от имени организации **Level1**.

    b. В разделе **Person's Primary Workplace** (Основное рабочее место) введите описание расположения в текстовое поле **Location** (Расположение).

    c. В разделе **Employee** (Сотрудник) выберите в поле **Employee Status** (Статус сотрудника) значение Casual (Обычный).

    d. Выберите команду **Сохранить**.

### <a name="test-single-sign-on"></a>Проверка единого входа

В этом разделе описано, как проверить конфигурацию единого входа Azure AD с помощью панели доступа.

Щелкнув элемент "Riskware" на Панели доступа, вы автоматически войдете в приложение Riskware, для которого настроили единый вход. См. дополнительные сведения о [панели доступа](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)

## <a name="additional-resources"></a>Дополнительные ресурсы

- [Список учебников по интеграции приложений SaaS с Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Что такое условный доступ в Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
