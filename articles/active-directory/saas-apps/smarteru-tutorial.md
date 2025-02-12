---
title: Руководство по Интеграция Azure Active Directory со SmarterU | Документация Майкрософт
description: Узнайте, как настроить единый вход между Azure Active Directory и SmarterU.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 95fe3212-d052-4ac8-87eb-ac5305227e85
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/19/2019
ms.author: jeedes
ms.openlocfilehash: 07c7a15ea713bb6f73e1174a681dcccb5e79e289
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "67090516"
---
# <a name="tutorial-azure-active-directory-integration-with-smarteru"></a>Руководство по Интеграция Azure Active Directory со SmarterU

В этом учебнике описано, как интегрировать SmarterU с Azure Active Directory (Azure AD).
Интеграция Azure AD с приложением SmarterU обеспечивает следующие преимущества.

* С помощью Azure AD можно управлять доступом к SmarterU.
* Вы можете включить автоматический вход пользователей в SmarterU (единый вход) с помощью учетной записи Azure AD.
* Вы можете управлять учетными записями централизованно на портале Azure.

Дополнительные сведения об интеграции приложений SaaS с Azure AD см. в статье [Единый вход в приложениях в Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Если у вас еще нет подписки Azure, [создайте бесплатную учетную запись Azure](https://azure.microsoft.com/free/), прежде чем начинать работу.

## <a name="prerequisites"></a>Предварительные требования

Чтобы настроить интеграцию Azure AD с SmarterU, вам потребуется:

* подписка Azure AD (если у вас нет среды Azure AD, вы можете получить пробную версию на один месяц по [этой ссылке](https://azure.microsoft.com/pricing/free-trial/));
* подписка SmarterU с поддержкой единого входа.

## <a name="scenario-description"></a>Описание сценария

В рамках этого руководства вы настроите и проверите единый вход Azure AD в тестовой среде.

* SmarterU поддерживает единый вход, инициированный **поставщиком удостоверений**.

## <a name="adding-smarteru-from-the-gallery"></a>Добавление SmarterU из коллекции

Чтобы настроить интеграцию SmarterU с Azure AD, необходимо добавить SmarterU из коллекции в список управляемых приложений SaaS.

**Чтобы добавить SmarterU из коллекции, выполните следующие действия.**

1. На **[портале Azure](https://portal.azure.com)** в области навигации слева щелкните значок **Azure Active Directory**.

    ![Кнопка Azure Active Directory](common/select-azuread.png)

2. Перейдите в колонку **Корпоративные приложения** и выберите **Все приложения**.

    ![Колонка "Корпоративные приложения"](common/enterprise-applications.png)

3. Чтобы добавить новое приложение, в верхней части диалогового окна нажмите кнопку **Создать приложение**.

    ![Кнопка "Создать приложение"](common/add-new-app.png)

4. В поле поиска введите **SmarterU**, выберите **SmarterU** на панели результатов и нажмите кнопку **Добавить**, чтобы добавить это приложение.

     ![SmarterU в списке результатов](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Настройка и проверка единого входа в Azure AD

В этом разделе описана настройка и проверка единого входа Azure AD в приложение SmarterU с использованием тестового пользователя **Britta Simon**.
Для обеспечения работы единого входа необходимо установить связь между пользователем Azure AD и соответствующим пользователем в SmarterU.

Чтобы настроить и проверить единый вход Azure AD в SmarterU, вам потребуется выполнить действия в следующих стандартных блоках.

1. **[Настройка единого входа Azure AD](#configure-azure-ad-single-sign-on)** необходима, чтобы пользователи могли использовать эту функцию.
2. **[Настройка единого входа в SmarterU](#configure-smarteru-single-sign-on)** необходима, чтобы настроить параметры единого входа на стороне приложения.
3. **[Создание тестового пользователя Azure AD](#create-an-azure-ad-test-user)** требуется для проверки работы единого входа Azure AD от имени пользователя Britta Simon.
4. **[Назначение тестового пользователя Azure AD](#assign-the-azure-ad-test-user)** необходимо, чтобы разрешить пользователю Britta Simon использовать единый вход Azure AD.
5. **[Создание тестового пользователя SmarterU](#create-smarteru-test-user)** требуется для создания в SmarterU пользователя Britta Simon, связанного с представлением этого пользователя в Azure AD.
6. **[Проверка единого входа](#test-single-sign-on)** необходима, чтобы проверить работу конфигурации.

### <a name="configure-azure-ad-single-sign-on"></a>Настройка единого входа Azure AD

В этом разделе описано включение единого входа Azure AD на портале Azure.

Чтобы настроить единый вход Azure AD в SmarterU, выполните следующие действия:

1. На [портале Azure](https://portal.azure.com/) на странице интеграции с приложением **SmarterU** выберите **Единый вход**.

    ![Ссылка "Настройка единого входа"](common/select-sso.png)

2. В диалоговом окне **Выбрать метод единого входа** выберите режим **SAML/WS-Fed**, чтобы включить единый вход.

    ![Режим выбора единого входа](common/select-saml-option.png)

3. На странице **Настройка единого входа с помощью SAML** щелкните **Изменить**, чтобы открыть диалоговое окно **Базовая конфигурация SAML**.

    ![Правка базовой конфигурации SAML](common/edit-urls.png)

4. В разделе **Базовая конфигурация SAML** выполните приведенные ниже действия.

    ![Сведения о домене и URL-адресах единого входа приложения SmarterU](common/idp-identifier.png)

    В текстовом поле **Идентификатор** введите URL-адрес: `https://www.smarteru.com/`.

5. На странице **Настройка единого входа с помощью SAML** в разделе **Сертификат подписи SAML** щелкните **Скачать**, чтобы скачать нужный вам **XML метаданных федерации**, и сохраните его на компьютере.

    ![Ссылка для скачивания сертификата](common/metadataxml.png)

6. Требуемые URL-адреса можно скопировать из раздела **Настройка SmarterU**.

    ![Копирование URL-адресов настройки](common/copy-configuration-urls.png)

    а) URL-адрес входа.

    b. Идентификатор Azure AD

    c. URL-адрес выхода.

### <a name="configure-smarteru-single-sign-on"></a>Настройка единого входа в SmarterU

1. В другом окне веб-браузера войдите на сайт SmarterU своей компании в качестве администратора.

1. На панели инструментов вверху щелкните **Параметры учетной записи**.

    ![Параметры учетной записи](./media/smarteru-tutorial/accountsettings.png)

1. На странице конфигурации учетной записи выполните следующие действия.

    ![Внешняя авторизация](./media/smarteru-tutorial/externalauthorizationconfiguration.png) 

    a. Установите флажок **Включить внешнюю авторизацию**.
  
    b. В разделе **Master Login Control** (Управление универсальным именем для входа) щелкните вкладку **SmarterU**.
  
    c. В разделе **User Default Login** (Имя для входа пользователей по умолчанию) щелкните вкладку **SmarterU**.
  
    d. Выберите **Включить SAML**.
  
    д. Скопируйте содержимое скачанного файла метаданных и вставьте его в текстовое поле **IdP Metadata** (Метаданные IdP).

    Е. Выберите **Identifier Attribute/Claim** (Атрибут или утверждение удостоверения).
  
    ж. Выберите команду **Сохранить**.

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

В этом разделе описано, как разрешить пользователю Britta Simon использовать единый вход Azure, предоставив этому пользователю доступ к SmarterU.

1. На портале Azure последовательно выберите **Корпоративные приложения**, **Все приложения**, а затем — **SmarterU**.

    ![Колонка "Корпоративные приложения"](common/enterprise-applications.png)

2. В списке приложений выберите **SmarterU**.

    ![Ссылка на SmarterU в списке приложений](common/all-applications.png)

3. В меню слева выберите **Пользователи и группы**.

    ![Ссылка "Пользователи и группы"](common/users-groups-blade.png)

4. Нажмите кнопку **Добавить пользователя**, а затем в диалоговом окне **Добавление назначения** выберите **Пользователи и группы**.

    ![Область "Добавление назначения"](common/add-assign-user.png)

5. В диалоговом окне **Пользователи и группы** из списка пользователей выберите **Britta Simon**, а затем в верхней части экрана нажмите кнопку **Выбрать**.

6. Если ожидается, что в утверждении SAML будет получено какое-либо значение роли, то в диалоговом окне **Выбор ролей** нужно выбрать соответствующую роль для пользователя из списка и затем нажать кнопку **Выбрать**, расположенную в нижней части экрана.

7. В диалоговом окне **Добавление назначения** нажмите кнопку **Назначить**.

### <a name="create-smarteru-test-user"></a>Создание тестового пользователя SmarterU

Чтобы пользователи Azure AD могли входить в SmarterU, их необходимо подготовить для SmarterU. В случае SmarterU подготовка выполняется вручную.

**Чтобы подготовить учетную запись пользователя, сделайте следующее:**

1. Выполните вход в клиент **SmarterU**.

1. Перейдите в раздел **Пользователи**.

1. В разделе «Пользователи» выполните следующие действия:

    ![Новый пользователь](./media/smarteru-tutorial/adduser.png)  

    a. Щелкните **+ Пользователь**.

    b. Введите значения атрибутов, связанные с учетной записью пользователя Azure Active Directory, в следующие текстовые поля: **Primary Email** (Основной адрес электронной почты), **Employee ID** (Идентификатор сотрудника), **Password** (Пароль), **Verify Password** (Проверьте пароль), **Given Name** (Заданное имя), **Surname** (Фамилия).

    c. Нажмите **Активный**.

    d. Выберите команду **Сохранить**.

> [!NOTE]
> Вы можете использовать любые другие инструменты создания учетных записей пользователей SmarterU или API, предоставляемые SmarterU для подготовки учетных записей пользователей AAD.

### <a name="test-single-sign-on"></a>Проверка единого входа

В этом разделе описано, как проверить конфигурацию единого входа Azure AD с помощью панели доступа.

Щелкнув плитку SmarterU на Панели доступа, вы автоматически войдете в приложение SmarterU, для которого настроили единый вход. См. дополнительные сведения о [панели доступа](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)

## <a name="additional-resources"></a>Дополнительные ресурсы

- [Список учебников по интеграции приложений SaaS с Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Что такое условный доступ в Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
