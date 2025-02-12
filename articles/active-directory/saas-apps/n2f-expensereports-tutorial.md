---
title: Руководство по Интеграция Azure Active Directory с N2F - Expense reports | Документация Майкрософт
description: Узнайте, как настроить единый вход между Azure Active Directory и приложением N2F - Expense reports.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: f56d53d7-5a08-490a-bfb9-78fefc2751ec
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/01/2019
ms.author: jeedes
ms.openlocfilehash: 0a7b38f26261625c6db8acb6653b3cd9353fdcc2
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "67096502"
---
# <a name="tutorial-azure-active-directory-integration-with-n2f---expense-reports"></a>Руководство по Интеграция Azure Active Directory с N2F - Expense reports

В этом руководстве описано, как интегрировать приложение N2F - Expense reports с Azure Active Directory (Azure AD).
Интеграция приложение N2F - Expense reports с Azure AD обеспечивают следующие преимущества.

* C помощью Azure AD вы можете контролировать доступ к N2F - Expense reports.
* Вы можете включить автоматический вход пользователей (единый вход) в N2F — Expense reports с учетной записью Azure AD.
* Вы можете управлять учетными записями централизованно на портале Azure.

Дополнительные сведения об интеграции приложений SaaS с Azure AD см. в статье [Единый вход в приложениях в Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Если у вас еще нет подписки Azure, [создайте бесплатную учетную запись Azure](https://azure.microsoft.com/free/), прежде чем начинать работу.

## <a name="prerequisites"></a>Предварительные требования

Чтобы настроить интеграцию Azure AD с N2F - Expense reports, вам потребуется:

* подписка Azure AD (если у вас нет среды Azure AD, вы можете получить пробную версию на один месяц по [этой ссылке](https://azure.microsoft.com/pricing/free-trial/));
* подписка N2F — Expense reports с поддержкой единого входа.

## <a name="scenario-description"></a>Описание сценария

В рамках этого руководства вы настроите и проверите единый вход Azure AD в тестовой среде.

* N2F — Expense reports поддерживает единый вход, инициируемый **поставщиком услуг** и **поставщиком удостоверений**.

## <a name="adding-n2f---expense-reports-from-the-gallery"></a>Добавление N2F - Expense reports из коллекции

Чтобы настроить интеграцию N2F - Expense reports с Azure AD, необходимо добавить N2F - Expense reports из коллекции в список управляемых приложений SaaS.

**Чтобы добавить N2F - Expense reports из коллекции, выполните следующие действия.**

1. На **[портале Azure](https://portal.azure.com)** в области навигации слева щелкните значок **Azure Active Directory**.

    ![Кнопка Azure Active Directory](common/select-azuread.png)

2. Перейдите в колонку **Корпоративные приложения** и выберите **Все приложения**.

    ![Колонка "Корпоративные приложения"](common/enterprise-applications.png)

3. Чтобы добавить новое приложение, в верхней части диалогового окна нажмите кнопку **Создать приложение**.

    ![Кнопка "Создать приложение"](common/add-new-app.png)

4. В поле поиска введите **N2F - Expense reports**, на панели результатов выберите **N2F - Expense reports** и нажмите кнопку **Добавить**, чтобы добавить это приложение.

     ![N2F - Expense reports в списке результатов](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Настройка и проверка единого входа в Azure AD

В этом разделе описана настройка и проверка единого входа Azure AD в N2F — Expense reports с использованием тестового пользователя **Britta Simon**.
Для обеспечения работы единого входа необходимо установить связь между пользователем Azure AD и соответствующим пользователем в N2F — Expense reports.

Чтобы настроить и проверить единый вход Azure AD в N2F - Expense reports, вам потребуется выполнить следующие действия.

1. **[Настройка единого входа Azure AD](#configure-azure-ad-single-sign-on)** необходима, чтобы пользователи могли использовать эту функцию.
2. **[Настройка единого входа в N2F — Expense reports](#configure-n2f---expense-reports-single-sign-on)** необходима, чтобы настроить параметры единого входа на стороне приложения.
3. **[Создание тестового пользователя Azure AD](#create-an-azure-ad-test-user)** требуется для проверки работы единого входа Azure AD от имени пользователя Britta Simon.
4. **[Назначение тестового пользователя Azure AD](#assign-the-azure-ad-test-user)** необходимо, чтобы разрешить пользователю Britta Simon использовать единый вход Azure AD.
5. **[Создание тестового пользователя N2F — Expense reports](#create-n2f---expense-reports-test-user)** требуется для создания в N2F — Expense reports пользователя Britta Simon, связанного с одноименным пользователем в Azure AD.
6. **[Проверка единого входа](#test-single-sign-on)** необходима, чтобы проверить работу конфигурации.

### <a name="configure-azure-ad-single-sign-on"></a>Настройка единого входа Azure AD

В этом разделе описано включение единого входа Azure AD на портале Azure.

Чтобы настроить единый вход Azure AD в N2F — Expense reports, выполните следующие действия.

1. На [портале Azure](https://portal.azure.com/) на странице интеграции с приложением **N2F — Expense reports** выберите **Единый вход**.

    ![Ссылка "Настройка единого входа"](common/select-sso.png)

2. В диалоговом окне **Выбрать метод единого входа** выберите режим **SAML/WS-Fed**, чтобы включить единый вход.

    ![Режим выбора единого входа](common/select-saml-option.png)

3. На странице **Настройка единого входа с помощью SAML** щелкните **Изменить**, чтобы открыть диалоговое окно **Базовая конфигурация SAML**.

    ![Правка базовой конфигурации SAML](common/edit-urls.png)

4. Если нужно настроить приложение в режиме, инициированном **поставщиком удостоверений**, в разделе **Базовые настройки SAML** пользователю для этого не нужно ничего делать, так как это приложение уже предварительно интегрировано в Azure.

    ![Сведения о домене и URL-адресах единого входа для приложения N2F - Expense reports](common/preintegrated.png)

5. Чтобы настроить приложение для работы в режиме, инициируемом **поставщиком услуг**, щелкните **Задать дополнительные URL-адреса** и выполните следующие действия.

    ![Сведения о домене и URL-адресах единого входа для приложения N2F - Expense reports](common/metadata-upload-additional-signon.png)

    В текстовом поле **URL-адрес входа** введите URL-адрес: `https://www.n2f.com/app/`.

6. На странице **Настройка единого входа с помощью SAML** в разделе **Сертификат подписи SAML** нажмите кнопку "Копировать", чтобы копировать **URL-адрес метаданных федерации приложений** и сохранить его на компьютере.

    ![Ссылка для скачивания сертификата](common/copy-metadataurl.png)

7. Скопируйте требуемый URL-адрес из раздела **Настройка myPolicies**.

    ![Копирование URL-адресов настройки](common/copy-configuration-urls.png)

    а) URL-адрес входа.

    b. Идентификатор Azure AD

    c. URL-адрес выхода.

### <a name="configure-n2f---expense-reports-single-sign-on"></a>Настройка единого входа в N2F — Expense reports

1. В другом окне веб-браузера войдите на корпоративный сайт N2F - Expense reports в качестве администратора.

2. Щелкните **Settings** (Параметры), а затем выберите **Advance Settings** (Дополнительные параметры) из раскрывающегося списка.

    ![Конфигурация N2F - Expense reports](./media/n2f-expensereports-tutorial/configure1.png)

3. Выберите вкладку **Account settings** (Параметры учетной записи).

    ![Конфигурация N2F - Expense reports](./media/n2f-expensereports-tutorial/configure2.png)

4. Выберите **Authentication** (Проверки подлинности), а затем выберите вкладку **+ Add an authentication method** (Добавить метод проверки подлинности).

    ![Конфигурация N2F - Expense reports](./media/n2f-expensereports-tutorial/configure3.png)

5. Выберите **SAML Microsoft Office 365** как метод проверки подлинности.

    ![Конфигурация N2F - Expense reports](./media/n2f-expensereports-tutorial/configure4.png)

6. В разделе **Authentication method** (Метод проверки подлинности) выполните следующие действия.

    ![Конфигурация N2F - Expense reports](./media/n2f-expensereports-tutorial/configure5.png)

    a. В текстовое поле **Entity ID** (Идентификатор сущности) вставьте значение **идентификатора Azure AD**, скопированное на портале Azure.

    b. В текстовое поле **Metadata URL** (URL-адрес метаданных) вставьте значение **URL-адреса метаданных федерации приложения**, скопированное на портале Azure.

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

В этом разделе описано, как разрешить пользователю Britta Simon использовать единый вход Azure, предоставив этому пользователю доступ к N2F - Expense reports.

1. На портале Azure выберите **Корпоративные приложения**, **Все приложения**, а затем — **N2F — Expense reports**.

    ![Колонка "Корпоративные приложения"](common/enterprise-applications.png)

2. В списке приложений выберите **N2F - Expense reports**.

    ![Ссылка на N2F - Expense reports в списке приложений](common/all-applications.png)

3. В меню слева выберите **Пользователи и группы**.

    ![Ссылка "Пользователи и группы"](common/users-groups-blade.png)

4. Нажмите кнопку **Добавить пользователя**, а затем в диалоговом окне **Добавление назначения** выберите **Пользователи и группы**.

    ![Область "Добавление назначения"](common/add-assign-user.png)

5. В диалоговом окне **Пользователи и группы** из списка пользователей выберите **Britta Simon**, а затем в верхней части экрана нажмите кнопку **Выбрать**.

6. Если ожидается, что в утверждении SAML будет получено какое-либо значение роли, то в диалоговом окне **Выбор ролей** нужно выбрать соответствующую роль для пользователя из списка и затем нажать кнопку **Выбрать**, расположенную в нижней части экрана.

7. В диалоговом окне **Добавление назначения** нажмите кнопку **Назначить**.

### <a name="create-n2f---expense-reports-test-user"></a>Создание тестового пользователя в N2F — Expense reports

Чтобы разрешить пользователям Azure AD вход в N2F - Expense reports, их необходимо подготовить для N2F - Expense reports. В случае с N2F - Expense reports подготовка выполняется вручную.

**Чтобы подготовить учетную запись пользователя, сделайте следующее:**

1. Выполните вход на сайт компании N2F - Expense reports в качестве администратора.

2. Щелкните **Settings** (Параметры), а затем выберите **Advance Settings** (Дополнительные параметры) из раскрывающегося списка.

    ![Добавление пользователя в N2F — Expense](./media/n2f-expensereports-tutorial/configure1.png)

3. Выберите вкладку **Users** (Пользователи) на панели навигации слева.

    ![Конфигурация N2F - Expense reports](./media/n2f-expensereports-tutorial/user1.png)

4. Щелкните **+ New user** (+ Новый пользователь).

    ![Конфигурация N2F - Expense reports](./media/n2f-expensereports-tutorial/user2.png)

5. В разделе **User** (Пользователь) выполните следующие действия.

    ![Конфигурация N2F - Expense reports](./media/n2f-expensereports-tutorial/user3.png)

    a. В текстовом поле **Email address** (Адрес электронной почты) введите адрес электронной почты пользователя, например **brittasimon\@contoso.com**.

    b. В текстовое поле **First name** (Имя) введите имя пользователя, например **Britta**.

    c. В текстовое поле **Name** (Имя) введите имя, например **BrittaSimon**.

    d. Выберите **роль, непосредственного руководителя (N + 1)** и **отделение** в соответствии с требованиями своей организации.

    д. Нажмите кнопку **Validate and send invitation** (Проверить и отправить приглашение).

    > [!NOTE]
    > Если во время добавления пользователя возникают проблемы, обратитесь в [службу поддержки N2F - Expense reports](mailto:support@n2f.com)

### <a name="test-single-sign-on"></a>Проверка единого входа 

В этом разделе описано, как проверить конфигурацию единого входа Azure AD с помощью панели доступа.

Щелкнув плитку N2F — Expense reports на Панели доступа, вы автоматически войдете в приложение N2F — Expense reports, для которого настроили единый вход. См. дополнительные сведения о [панели доступа](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)

## <a name="additional-resources"></a>Дополнительные ресурсы

- [Список учебников по интеграции приложений SaaS с Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Что такое условный доступ в Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

