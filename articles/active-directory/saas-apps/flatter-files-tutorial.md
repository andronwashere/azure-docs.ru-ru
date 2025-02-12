---
title: Руководство по Интеграция Azure Active Directory с Flatter Files | Документация Майкрософт
description: Узнайте, как настроить единый вход между Azure Active Directory и Flatter Files.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: f86fe5e3-0e91-40d6-869c-3df6912d27ea
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 02/15/2019
ms.author: jeedes
ms.openlocfilehash: 9e4ba987393628af07f8a8a507f635047eb18cc5
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "67102557"
---
# <a name="tutorial-azure-active-directory-integration-with-flatter-files"></a>Руководство по Интеграция Azure Active Directory с Flatter Files

В этом руководстве описано, как интегрировать приложение Flatter Files с Azure Active Directory (Azure AD).
Интеграция Azure AD с Flatter Files обеспечивает следующие преимущества.

* С помощью Azure AD вы можете контролировать доступ к Flatter Files.
* Учетные записи Azure AD позволяют включить автоматический вход для пользователей в приложение Flatter Files (единый вход).
* Вы можете управлять учетными записями централизованно на портале Azure.

Дополнительные сведения об интеграции приложений SaaS с Azure AD см. в статье [Единый вход в приложениях в Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Если у вас еще нет подписки Azure, [создайте бесплатную учетную запись Azure](https://azure.microsoft.com/free/), прежде чем начинать работу.

## <a name="prerequisites"></a>Предварительные требования

Чтобы настроить интеграцию Azure AD с Flatter Files, вам потребуется:

* подписка Azure AD (если у вас нет среды Azure AD, вы можете получить пробную версию на один месяц по [этой ссылке](https://azure.microsoft.com/pricing/free-trial/));
* подписка Flatter Files с поддержкой единого входа.

## <a name="scenario-description"></a>Описание сценария

В рамках этого руководства вы настроите и проверите единый вход Azure AD в тестовой среде.

* В приложении Flatter Files поддерживается единый вход, инициированный **поставщиком удостоверений**.

## <a name="adding-flatter-files-from-the-gallery"></a>Добавление Flatter Files из коллекции

Чтобы настроить интеграцию Flatter Files с Azure AD, необходимо добавить Flatter Files из коллекции в список управляемых приложений SaaS.

**Как добавить Flatter Files из коллекции**

1. На **[портале Azure](https://portal.azure.com)** в области навигации слева щелкните значок **Azure Active Directory**.

    ![Кнопка Azure Active Directory](common/select-azuread.png)

2. Перейдите в колонку **Корпоративные приложения** и выберите **Все приложения**.

    ![Колонка "Корпоративные приложения"](common/enterprise-applications.png)

3. Чтобы добавить новое приложение, в верхней части диалогового окна нажмите кнопку **Создать приложение**.

    ![Кнопка "Создать приложение"](common/add-new-app.png)

4. В поле поиска введите **Flatter Files**, выберите **Flatter Files** на панели результатов и нажмите кнопку **Добавить**, чтобы добавить это приложение.

     ![Flatter Files в списке результатов](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Настройка и проверка единого входа в Azure AD

В рамках этого раздела вы настроите и проверите единый вход Azure AD в Flatter Files с использованием тестового пользователя **Britta Simon**.
Чтобы обеспечить единый вход, свяжите пользователя Azure AD с соответствующим пользователем в приложении Flatter Files.

Чтобы настроить и проверить единый вход Azure AD в Flatter Files, вам потребуется выполнить следующие действия.

1. **[Настройка единого входа Azure AD](#configure-azure-ad-single-sign-on)** необходима, чтобы пользователи могли использовать эту функцию.
2. **[Настройка единого входа в приложение Flatter Files](#configure-flatter-files-single-sign-on)** требуется, чтобы определить параметры единого входа на стороне приложения.
3. **[Создание тестового пользователя Azure AD](#create-an-azure-ad-test-user)** требуется для проверки работы единого входа Azure AD от имени пользователя Britta Simon.
4. **[Назначение тестового пользователя Azure AD](#assign-the-azure-ad-test-user)** необходимо, чтобы разрешить пользователю Britta Simon использовать единый вход Azure AD.
5. **[Создание тестового пользователя Flatter Files](#create-flatter-files-test-user)** требуется, чтобы в приложении Flatter Files существовал пользователь Britta Simon, связанный с одноименным пользователем в Azure AD.
6. **[Проверка единого входа](#test-single-sign-on)** необходима, чтобы проверить работу конфигурации.

### <a name="configure-azure-ad-single-sign-on"></a>Настройка единого входа Azure AD

В этом разделе описано включение единого входа Azure AD на портале Azure.

Чтобы настроить единый вход Azure AD в приложение Flatter Files, сделайте следующее:

1. На [портале Azure](https://portal.azure.com/) на странице интеграции с приложением **Flatter Files** щелкните **Единый вход**.

    ![Ссылка "Настройка единого входа"](common/select-sso.png)

2. В диалоговом окне **Выбрать метод единого входа** выберите режим **SAML/WS-Fed**, чтобы включить единый вход.

    ![Режим выбора единого входа](common/select-saml-option.png)

3. На странице **Настройка единого входа с помощью SAML** щелкните **Изменить**, чтобы открыть диалоговое окно **Базовая конфигурация SAML**.

    ![Правка базовой конфигурации SAML](common/edit-urls.png)

4. В разделе **Базовая конфигурация SAML** не нужно выполнять никаких действий, так как приложение уже предварительно интегрировано с Azure.

    ![Сведения о домене и URL-адресах единого входа для приложения Flatter Files](common/preintegrated.png)

5. На странице **Настройка единого входа с помощью SAML** в разделе **Сертификат подписи SAML** щелкните **Загрузить**, чтобы загрузить требуемый **сертификат (Base64)** из предложенных вариантов, и сохраните его на компьютере.

    ![Ссылка для скачивания сертификата](common/certificatebase64.png)

6. Требуемые URL-адреса можно скопировать из раздела **настройки Flatter Files**.

    ![Копирование URL-адресов настройки](common/copy-configuration-urls.png)

    а) URL-адрес входа.

    б) Идентификатор Azure AD.

    в) URL-адрес выхода.

### <a name="configure-flatter-files-single-sign-on"></a>Настройка единого входа в приложении Flatter Files

1. Войдите в приложение Flatter Files с правами администратора.

2. Щелкните **Панель мониторинга**. 
   
    ![Настройка единого входа](./media/flatter-files-tutorial/tutorial_flatter_files_05.png)  

3. Выберите элемент **Settings** (Параметры), а затем выполните следующие действия на вкладке **Company** (Компания). 
   
    ![Настройка единого входа](./media/flatter-files-tutorial/tutorial_flatter_files_06.png)  
    
    a. Установите флажок **Use SAML 2.0 For Authentication**(Использовать SAML 2.0 для проверки подлинности).
    
    b. Нажмите кнопку **Configure SAML** (Настроить SAML).

4. В диалоговом окне **SAML Configuration** (Настройка SAML) выполните следующие действия. 
   
    ![Настройка единого входа](./media/flatter-files-tutorial/tutorial_flatter_files_08.png)  
   
    a. В поле **Домен** укажите свой зарегистрированный домен.
   
   > [!NOTE]
   > Если у вас нет зарегистрированного домена, обратитесь в службу поддержки Flatter Files по адресу [support@flatterfiles.com](mailto:support@flatterfiles.com). 
    
    b. В текстовое поле **Identity Provider URL** (URL-адрес поставщика удостоверений) вставьте **URL-адрес для входа**, скопированный на портале Azure.
   
    c.  Откройте сертификат в кодировке Base-64 в Блокноте, скопируйте его содержимое в буфер обмена, а затем вставьте его в текстовое поле **Сертификат поставщика удостоверений**.

    d. Нажмите кнопку **Обновить**.

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

В этом разделе описано, как разрешить пользователю Britta Simon использовать единый вход Azure путем предоставления доступа к Flatter Files.

1. На портале Azure последовательно выберите **Корпоративные приложения**, **Все приложения** и **Flatter Files**.

    ![Колонка "Корпоративные приложения"](common/enterprise-applications.png)

2. В списке приложений выберите **Flatter Files**.

    ![Ссылка на Flatter Files в списке приложений](common/all-applications.png)

3. В меню слева выберите **Пользователи и группы**.

    ![Ссылка "Пользователи и группы"](common/users-groups-blade.png)

4. Нажмите кнопку **Добавить пользователя**, а затем в диалоговом окне **Добавление назначения** выберите **Пользователи и группы**.

    ![Область "Добавление назначения"](common/add-assign-user.png)

5. В диалоговом окне **Пользователи и группы** из списка пользователей выберите **Britta Simon**, а затем в верхней части экрана нажмите кнопку **Выбрать**.

6. Если ожидается, что в утверждении SAML будет получено какое-либо значение роли, то в диалоговом окне **Выбор ролей** нужно выбрать соответствующую роль для пользователя из списка и затем нажать кнопку **Выбрать**, расположенную в нижней части экрана.

7. В диалоговом окне **Добавление назначения** нажмите кнопку **Назначить**.

### <a name="create-flatter-files-test-user"></a>Создание тестового пользователя Flatter Files

Цель этого раздела — создать пользователя с именем Britta Simon в Flatter Files.

**Как создать пользователя с именем Britta Simon в Flatter Files**

1. Войдите на сайт своей компании в службе **Flatter Files** с правами администратора.

2. В области навигации слева щелкните **Settings** (Параметры), а затем выберите вкладку **Users** (Пользователи).
   
    ![Создание пользователя Flatter Files](./media/flatter-files-tutorial/tutorial_flatter_files_09.png)

3. Нажмите кнопку **Add User**(Добавить пользователя). 

4. На странице **Добавление пользователя** выполните следующие действия.
   
    ![Создание пользователя Flatter Files](./media/flatter-files-tutorial/tutorial_flatter_files_10.png)

    a. В текстовом поле **Имя** введите **Britta**.
   
    b. В текстовом поле **Фамилия** введите **Simon**. 
   
    c. В текстовом поле **Email Address** (Адрес электронной почты) введите адрес электронной почты пользователя Britta Simon на портале Azure.
   
    d. Нажмите кнопку **Submit**(Отправить).   


### <a name="test-single-sign-on"></a>Проверка единого входа 

В этом разделе описано, как проверить конфигурацию единого входа Azure AD с помощью панели доступа.

Щелкнув плитку Flatter Files на Панели доступа, вы автоматически войдете в приложение Flatter Files, для которого настроили единый вход. См. дополнительные сведения о [панели доступа](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)

## <a name="additional-resources"></a>Дополнительные ресурсы

- [Список учебников по интеграции приложений SaaS с Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Что такое условный доступ в Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

