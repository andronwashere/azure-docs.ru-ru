---
title: Руководство. Интеграция Azure Active Directory с SpringCM | Документация Майкрософт
description: Узнайте, как настроить единый вход между Azure Active Directory и SpringCM.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 4a42f797-ac58-4aca-a8e6-53bfe5529083
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 04/08/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 5e1d1973dd51068e6f3e0746ee988a51f375899f
ms.sourcegitcommit: ccb9a7b7da48473362266f20950af190ae88c09b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/05/2019
ms.locfileid: "67588019"
---
# <a name="tutorial-azure-active-directory-integration-with-springcm"></a>Руководство. Интеграция Azure Active Directory с SpringCM

В этом руководстве описано, как интегрировать SpringCM с Azure Active Directory (Azure AD).
Интеграция Azure AD с приложением SpringCM обеспечивает следующие преимущества.

* С помощью Azure AD вы можете контролировать доступ к SpringCM.
* Вы можете включить автоматический вход пользователей в SpringCM (единый вход) с помощью учетных записей Azure AD.
* Вы можете управлять учетными записями централизованно на портале Azure.

Дополнительные сведения об интеграции приложений SaaS с Azure AD см. в статье [Единый вход в приложениях в Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Если у вас еще нет подписки Azure, [создайте бесплатную учетную запись Azure](https://azure.microsoft.com/free/), прежде чем начинать работу.

## <a name="prerequisites"></a>Предварительные требования

Чтобы настроить интеграцию Azure AD с приложением SpringCM, вам потребуется:

* подписка Azure AD (если у вас нет среды Azure AD, вы можете получить [бесплатную учетную запись](https://azure.microsoft.com/free/));
* подписка SpringCM с поддержкой единого входа.

## <a name="scenario-description"></a>Описание сценария

В рамках этого руководства вы настроите и проверите единый вход Azure AD в тестовой среде.

* SpringCM поддерживает единый вход, инициируемый **поставщиком услуг**.

## <a name="adding-springcm-from-the-gallery"></a>Добавление SpringCM из коллекции.

Чтобы настроить интеграцию SpringCM с Azure AD, необходимо добавить SpringCM из коллекции в список управляемых приложений SaaS.

**Чтобы добавить SpringCM из коллекции, выполните следующие действия.**

1. На **[портале Azure](https://portal.azure.com)** в области навигации слева щелкните значок **Azure Active Directory**.

    ![Кнопка Azure Active Directory](common/select-azuread.png)

2. Перейдите в колонку **Корпоративные приложения** и выберите **Все приложения**.

    ![Колонка "Корпоративные приложения"](common/enterprise-applications.png)

3. Чтобы добавить новое приложение, в верхней части диалогового окна нажмите кнопку **Новое приложение**.

    ![Кнопка "Создать приложение"](common/add-new-app.png)

4. В поле поиска введите **SpringCM**, выберите **SpringCM** на панели результатов и нажмите кнопку **Добавить**, чтобы добавить это приложение.

    ![SpringCM в списке результатов](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Настройка и проверка единого входа в Azure AD

В этом разделе описана настройка и проверка единого входа Azure AD в SpringCM с использованием тестового пользователя **Britta Simon**.
Для обеспечения работы единого входа необходимо установить связь между пользователем Azure AD и соответствующим пользователем в SpringCM.

Чтобы настроить и проверить единый вход Azure AD в SpringCM, вам потребуется выполнить действия в следующих стандартных блоках.

1. **[Настройка единого входа Azure AD](#configure-azure-ad-single-sign-on)** необходима, чтобы пользователи могли использовать эту функцию.
2. **[Настройка единого входа в SpringCM](#configure-springcm-single-sign-on)** необходима, чтобы настроить параметры единого входа на стороне приложения.
3. **[Создание тестового пользователя Azure AD](#create-an-azure-ad-test-user)** требуется для проверки работы единого входа Azure AD от имени пользователя Britta Simon.
4. **[Назначение тестового пользователя Azure AD](#assign-the-azure-ad-test-user)** необходимо, чтобы разрешить пользователю Britta Simon использовать единый вход Azure AD.
5. **[Создание тестового пользователя SpringCM](#create-springcm-test-user)** требуется для создания пользователя Britta Simon в SpringCM, связанного с одноименным пользователем в Azure AD.
6. **[Проверка единого входа](#test-single-sign-on)** необходима, чтобы проверить работу конфигурации.

### <a name="configure-azure-ad-single-sign-on"></a>Настройка единого входа Azure AD

В этом разделе описано включение единого входа Azure AD на портале Azure.

Чтобы настроить единый вход Azure AD в SpringCM, выполните следующие действия.

1. На [портале Azure](https://portal.azure.com/) на странице интеграции с приложением **SpringCM** щелкните **Единый вход**.

    ![Ссылка "Настройка единого входа"](common/select-sso.png)

2. В диалоговом окне **Выбрать метод единого входа** выберите режим **SAML/WS-Fed**, чтобы включить единый вход.

    ![Режим выбора единого входа](common/select-saml-option.png)

3. На странице **Настройка единого входа с помощью SAML** щелкните **Изменить**, чтобы открыть диалоговое окно **Базовая конфигурация SAML**.

    ![Правка базовой конфигурации SAML](common/edit-urls.png)

4. В разделе **Базовая конфигурация SAML** выполните приведенные ниже действия.

    ![Сведения о домене и URL-адресах единого входа для приложения SpringCM](common/sp-signonurl.png)

    В текстовом поле **URL-адрес входа** введите URL-адрес в формате `https://na11.springcm.com/atlas/SSO/SSOEndpoint.ashx?aid=<identifier>`.

    > [!NOTE]
    > Это значение приведено для примера. Вместо него необходимо указать фактический URL-адрес входа. Для получения этого значения обратитесь к [группе поддержки клиентов SpringCM](https://knowledge.springcm.com/support). Можно также посмотреть шаблоны в разделе **Базовая конфигурация SAML** на портале Azure.

4. На странице **Настройка единого входа с помощью SAML** в разделе **Сертификат подписи SAML** щелкните **Загрузить**, чтобы загрузить требуемый **сертификат (необработанный)** из предложенных вариантов и сохранить его на компьютере.

    ![Ссылка для скачивания сертификата](common/certificateraw.png)

6. Требуемые URL-адреса можно скопировать из раздела **Настройка SpringCM**.

    ![Копирование URL-адресов настройки](common/copy-configuration-urls.png)

    а) URL-адрес входа.

    b. Идентификатор Azure AD

    c. URL-адрес выхода.

### <a name="configure-springcm-single-sign-on"></a>Настройка единого входа в SpringCM

1. В другом окне веб-браузера войдите на сайт **SpringCM** компании в качестве администратора.

1. В меню вверху выберите **GO TO** (Перейти к), щелкните **Preferences** (Параметры), а затем в разделе **Account Preferences** (Параметры учетной записи) щелкните **SAML SSO** (Единый вход SAML).
   
    ![Единый вход SAML](./media/spring-cm-tutorial/ic797051.png "Единый вход SAML")

1. В разделе «Конфигурация поставщика удостоверений» выполните следующие действия:
   
    ![Конфигурация поставщика удостоверений](./media/spring-cm-tutorial/ic797052.png "Конфигурация поставщика удостоверений")
    
    a. Чтобы отправить загруженный сертификат Azure Active Directory, щелкните **Select Issuer Certificate** (Выбрать сертификат издателя) или **Change Issuer Certificate** (Изменить сертификат издателя).
    
    b. В текстовое поле **Issuer** (Издатель) вставьте значение **Идентификатор Azure AD**, скопированное на портале Azure.
    
    c. В текстовое поле **Service Provider (SP) Initiated Endpoint** (Конечная точка, инициированная поставщиком услуг) вставьте значение **URL-адрес входа**, скопированное на портале Azure.
            
    d. Для параметра **SAML Enabled** (SAML включен) установите значение **Enabled** (Включено).

    д. Выберите команду **Сохранить**.

### <a name="create-an-azure-ad-test-user"></a>Создание тестового пользователя Azure AD 

Цель этого раздела — создать на портале Azure тестового пользователя с именем Britta Simon.

1. На портале Azure в области слева выберите **Azure Active Directory**, **Пользователи**, а затем — **Все пользователи**.

    ![Ссылки "Пользователи и группы" и "Все пользователи"](common/users.png)

2. В верхней части экрана выберите **Новый пользователь**.

    ![Кнопка "Новый пользователь"](common/new-user.png)

3. В разделе свойств пользователя сделайте следующее:

    ![Диалоговое окно "Пользователь"](common/user-properties.png)

    а. В поле **Имя** введите **BrittaSimon**.
  
    b. В поле **Имя пользователя** введите `brittasimon@yourcompanydomain.extension`. Например BrittaSimon@contoso.com.

    c. Установите флажок **Показать пароль** и запишите значение, которое отображается в поле "Пароль".

    d. Нажмите кнопку **Создать**.

### <a name="assign-the-azure-ad-test-user"></a>Назначение тестового пользователя Azure AD

В этом разделе описано, как разрешить пользователю Britta Simon использовать единый вход Azure путем предоставления доступа к SpringCM.

1. На портале Azure выберите **Корпоративные приложения**, **Все приложения**, а затем — **SpringCM**.

    ![Колонка "Корпоративные приложения"](common/enterprise-applications.png)

2. В списке приложений выберите **SpringCM**.

    ![Ссылка на SpringCM в списке "Приложения"](common/all-applications.png)

3. В меню слева выберите **Пользователи и группы**.

    ![Ссылка "Пользователи и группы"](common/users-groups-blade.png)

4. Нажмите кнопку **Добавить пользователя**, а затем в диалоговом окне **Добавление назначения** выберите **Пользователи и группы**.

    ![Область "Добавление назначения"](common/add-assign-user.png)

5. В диалоговом окне **Пользователи и группы** из списка пользователей выберите **Britta Simon**, а затем в верхней части экрана нажмите кнопку **Выбрать**.

6. Если ожидается, что в утверждении SAML будет получено какое-либо значение роли, то в диалоговом окне **Выбор ролей** нужно выбрать соответствующую роль для пользователя из списка и затем нажать кнопку **Выбрать**, расположенную в нижней части экрана.

7. В диалоговом окне **Добавление назначения** нажмите кнопку **Назначить**.

### <a name="create-springcm-test-user"></a>Создание тестового пользователя SpringCM

Чтобы пользователи Azure Active Directory могли входить в SpringCM, они должны быть подготовлены в SpringCM. В случае SpringCM подготовка выполняется вручную.

> [!NOTE]
> Дополнительные сведения см. в статье [Create and Edit a SpringCM User](https://knowledge.springcm.com/create-and-edit-a-springcm-user) (Создание и изменение пользователя SpringCM). 

**Чтобы подготовить учетную запись пользователя в SpringCM, выполните следующее.**

1. Выполните вход на свой корпоративный сайт **SpringCM** в качестве администратора.

1. Щелкните **GO TO** (Перейти), а затем — **ADDRESS BOOK** (Адресная книга).
   
    ![Создание пользователя](./media/spring-cm-tutorial/ic797054.png "Создание пользователя")

1. Нажмите кнопку **Создать пользователя**.

1. Выберите **роль пользователя**.

1. Установите флажок **Отправить сообщение для активации**.

1. В соответствующие текстовые поля введите имя, фамилию и электронный адрес пользователя действующей учетной записи Azure Active Directory, которую вы хотите подготовить.

1. Добавьте пользователя в **группу безопасности**.

1. Выберите команду **Сохранить**.

   > [!NOTE]
   > Вы можете использовать любые другие инструменты создания учетных записей пользователей SpringCM или API, предоставляемые SpringCM для подготовки учетных записей пользователей AAD.

### <a name="test-single-sign-on"></a>Проверка единого входа 

В этом разделе описано, как проверить конфигурацию единого входа Azure AD с помощью панели доступа.

Щелкнув элемент "SpringCM" на Панели доступа, вы автоматически войдете в приложение SpringCM, для которого настроили единый вход. См. дополнительные сведения о [панели доступа](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)

## <a name="additional-resources"></a>Дополнительные ресурсы

- [Список учебников по интеграции приложений SaaS с Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Что такое условный доступ в Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

