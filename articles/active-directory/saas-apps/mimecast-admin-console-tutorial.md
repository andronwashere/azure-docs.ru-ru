---
title: Руководство по Интеграция Azure Active Directory с консолью администрирования Mimecast | Документация Майкрософт
description: Узнайте, как настроить единый вход Azure Active Directory в консоль администрирования Mimecast.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: 81c50614-f49b-4bbc-97d5-3cf77154305f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 12/27/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 0bbbd73d1856ba5d3dc19873c56fce622b272939
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "67097346"
---
# <a name="tutorial-azure-active-directory-integration-with-mimecast-admin-console"></a>Руководство по Интеграция Azure Active Directory с консолью администрирования Mimecast

В этом руководстве описано, как интегрировать консоль администрирования Mimecast с Azure Active Directory (Azure AD).
Интеграция Azure AD с консолью администрирования Mimecast обеспечивает следующие преимущества:

* С помощью Azure AD вы можете контролировать доступ к консоли администрирования Mimecast.
* Вы можете включить автоматический вход пользователей в консоль администрирования Mimecast (единый вход) с использованием учетных записей Azure AD.
* Вы можете управлять учетными записями централизованно на портале Azure.

Дополнительные сведения об интеграции приложений SaaS с Azure AD см. в статье [Единый вход в приложениях в Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Если у вас еще нет подписки Azure, [создайте бесплатную учетную запись Azure](https://azure.microsoft.com/free/), прежде чем начинать работу.

## <a name="prerequisites"></a>Предварительные требования

Чтобы настроить интеграцию Azure AD с консолью администрирования Mimecast, вам потребуется:

* подписка Azure AD (если у вас нет среды Azure AD, вы можете получить пробную версию на один месяц по [этой ссылке](https://azure.microsoft.com/pricing/free-trial/));
* Подписка на консоль администрирования Mimecast с поддержкой единого входа

## <a name="scenario-description"></a>Описание сценария

В рамках этого руководства вы настроите и проверите единый вход Azure AD в тестовой среде.

* Консоль администрирования Mimecast поддерживает единый вход инициированного **пакета обновления**.

## <a name="adding-mimecast-admin-console-from-the-gallery"></a>Добавление консоли администрирования Mimecast из коллекции

Чтобы настроить интеграцию консоли администрирования Mimecast с Azure AD, необходимо добавить консоль администрирования Mimecast из коллекции в список управляемых приложений SaaS.

**Чтобы добавить консоль администрирования Mimecast из коллекции, сделайте следующее.**

1. На **[портале Azure](https://portal.azure.com)** в области навигации слева щелкните значок **Azure Active Directory**.

    ![Кнопка Azure Active Directory](common/select-azuread.png)

2. Перейдите в колонку **Корпоративные приложения** и выберите **Все приложения**.

    ![Колонка "Корпоративные приложения"](common/enterprise-applications.png)

3. Чтобы добавить новое приложение, в верхней части диалогового окна нажмите кнопку **Создать приложение**.

    ![Кнопка "Создать приложение"](common/add-new-app.png)

4. В поле поиска введите **Консоль администрирования Mimecast**, выберите **Консоль администрирования Mimecast** на панели результатов и нажмите кнопку **Добавить**, чтобы добавить это приложение.

     ![Консоль администрирования Mimecast в списке результатов](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Настройка и проверка единого входа в Azure AD

В этом разделе описана настройка и проверка единого входа Azure AD с помощью консоли администрирования Mimecast с использованием тестового пользователя **Britta Simon**.
Для обеспечения работы единого входа необходимо установить связь между пользователем Azure AD и одноименным пользователем в консоли администрирования Mimecast.

Чтобы настроить и проверить единый вход Azure AD в консоли администрирования Mimecast, вам потребуется выполнить действия в следующих стандартных блоках.

1. **[Настройка единого входа Azure AD](#configure-azure-ad-single-sign-on)** необходима, чтобы пользователи могли использовать эту функцию.
2. **[Настройка единого входа в консоль администрирования Mimecast](#configure-mimecast-admin-console-single-sign-on)** необходима, чтобы настроить параметры единого входа на стороне приложения.
3. **[Создание тестового пользователя Azure AD](#create-an-azure-ad-test-user)** требуется для проверки работы единого входа Azure AD от имени пользователя Britta Simon.
4. **[Назначение тестового пользователя Azure AD](#assign-the-azure-ad-test-user)** необходимо, чтобы разрешить пользователю Britta Simon использовать единый вход Azure AD.
5. **[Создание тестового пользователя консоли администрирования Mimecast](#create-mimecast-admin-console-test-user)** требуется для того, чтобы в консоли администрирования Mimecast существовал пользователь Britta Simon, связанный с представлением этого же пользователя в Azure AD.
6. **[Проверка единого входа](#test-single-sign-on)** необходима, чтобы проверить работу конфигурации.

### <a name="configure-azure-ad-single-sign-on"></a>Настройка единого входа Azure AD

В этом разделе описано включение единого входа Azure AD на портале Azure.

Чтобы настроить единый вход Azure AD в консоли администрирования Mimecast, сделайте следующее.

1. На [портале Azure](https://portal.azure.com/) на странице интеграции с приложением **Консоль администрирования Mimecast** выберите **Единый вход**.

    ![Ссылка "Настройка единого входа"](common/select-sso.png)

2. В диалоговом окне **Выбрать метод единого входа** выберите режим **SAML/WS-Fed**, чтобы включить единый вход.

    ![Режим выбора единого входа](common/select-saml-option.png)

3. На странице **Настройка единого входа с помощью SAML** щелкните **Изменить**, чтобы открыть диалоговое окно **Базовая конфигурация SAML**.

    ![Правка базовой конфигурации SAML](common/edit-urls.png)

4. В разделе **Базовая конфигурация SAML** выполните приведенные ниже действия.

    ![Сведения о домене и URL-адресах единого входа консоли администрирования Mimecast](common/sp-signonurl.png)

    В текстовом поле **URL-адрес для входа** введите URL-адрес:
    
    | |
    | -- |
    | `https://webmail-uk.mimecast.com`|
    | `https://webmail-us.mimecast.com`|

    > [!NOTE] 
    > URL-адрес для входа зависит от региона.

4. На странице **Настройка единого входа с помощью SAML** в разделе **Сертификат подписи SAML** щелкните **Загрузить**, чтобы загрузить требуемый **сертификат (Base64)** из предложенных вариантов, и сохраните его на компьютере.

    ![Ссылка для скачивания сертификата](common/certificatebase64.png)

6. Требуемый URL-адрес можно скопировать из раздела **Set up Mimecast Admin Console** (Настройка консоли администрирования Mimecast).

    ![Копирование URL-адресов настройки](common/copy-configuration-urls.png)

    а) URL-адрес входа.

    б) Идентификатор Azure AD.

    в) URL-адрес выхода.

### <a name="configure-mimecast-admin-console-single-sign-on"></a>Настройка консоли администрирования Mimecast с поддержкой единого входа

1. В другом окне веб-браузера войдите в консоль администрирования Mimecast в качестве администратора.

2. Выберите **Services \> Application** (Службы > Приложение).

    ![Services](./media/mimecast-admin-console-tutorial/ic794998.png "Services") (Службы)

3. Щелкните **Профили проверки подлинности**.

    ![Authentication Profiles](./media/mimecast-admin-console-tutorial/ic794999.png "Authentication Profiles") (Профили аутентификации)
    
4. Щелкните **Новый профиль проверки подлинности**.

    ![New Authentication Profile](./media/mimecast-admin-console-tutorial/ic795000.png "New Authentication Profile") (Создать профиль аутентификации)

5. В разделе **Профиль проверки подлинности** сделайте следующее:

    ![Authentication Profile](./media/mimecast-admin-console-tutorial/ic795015.png "Authentication Profile") (Профиль аутентификации)
    
    a. В текстовом поле **Описание** введите имя конфигурации.
    
    b. Выберите **Обязательное использование проверки подлинности SAML для консоли администрирования Mimecast**.
    
    c. В поле **Provider** (Поставщик) выберите **Azure Active Directory**.
    
    d. В текстовое поле **URL-адрес издателя** вставьте значение **Идентификатор Azure AD**, скопированное с портала Azure.
    
    д. В текстовое поле **URL-адрес входа** вставьте **URL-адрес входа**, скопированный с портала Azure.

    Е. В текстовое поле **URL-адрес выхода** вставьте значение **URL-адрес входа**, скопированное с портала Azure.
    
    >[!NOTE]
    >Значения URL-адреса входа в систему и URL-адреса выхода из системы для консоли администрирования Mimecast одинаковы.
    
    ж. Откройте в Блокноте скачанный на портале Azure сертификат в кодировке Base-64, удалите первую строку (" *--* ") и последнюю строку (" *--* "), скопируйте остальное содержимое в буфер обмена и вставьте его в текстовое поле **Identity Provider Certificate (Metadata)** (Сертификат поставщика удостоверений (метаданные)).
    
    h. Установите флажок **Разрешить единый вход**.
    
    i. Выберите команду **Сохранить**.

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

В этом разделе описано, как разрешить пользователю Britta Simon использовать единый вход Azure, предоставив этому пользователю доступ к консоли администрирования Mimecast.

1. На портале Azure выберите **Корпоративные приложения**, **Все приложения**, а затем — **Mimecast Admin Console** (Консоль администрирования Mimecast).

    ![Колонка "Корпоративные приложения"](common/enterprise-applications.png)

2. Из списка приложений введите и выберите **Mimecast Admin Console** (Консоль администрирования Mimecast).

    ![Ссылка на консоль администрирования Mimecast в списке "Приложения"](common/all-applications.png)

3. В меню слева выберите **Пользователи и группы**.

    ![Ссылка "Пользователи и группы"](common/users-groups-blade.png)

4. Нажмите кнопку **Добавить пользователя**, а затем в диалоговом окне **Добавление назначения** выберите **Пользователи и группы**.

    ![Область "Добавление назначения"](common/add-assign-user.png)

5. В диалоговом окне **Пользователи и группы** из списка пользователей выберите **Britta Simon**, а затем в верхней части экрана нажмите кнопку **Выбрать**.

6. Если ожидается, что в утверждении SAML будет получено какое-либо значение роли, то в диалоговом окне **Выбор ролей** нужно выбрать соответствующую роль для пользователя из списка и затем нажать кнопку **Выбрать**, расположенную в нижней части экрана.

7. В диалоговом окне **Добавление назначения** нажмите кнопку **Назначить**.

### <a name="create-mimecast-admin-console-test-user"></a>Создание тестового пользователя консоли администрирования Mimecast

Чтобы разрешить пользователям Azure AD вход в консоль администрирования Mimecast, они должны быть подготовлены для консоли администрирования Mimecast. В случае с консолью администрирования Mimecast подготовка выполняется вручную.

* Перед созданием пользователей необходимо зарегистрировать домен.

**Чтобы настроить подготовку учетных записей пользователей, выполните следующие действия.**

1. Войдите в **консоль администрирования Mimecast** в качестве администратора.

2. Выберите **Directories \> Internal** (Каталоги > Внутренние).
   
    ![Directories](./media/mimecast-admin-console-tutorial/ic795003.png "Directories") (Каталоги)

3. Щелкните **Зарегистрировать новый домен**.
   
    ![Register New Domain](./media/mimecast-admin-console-tutorial/ic795004.png "Register New Domain") (Зарегистрировать новый домен)

4. После создания нового домена щелкните **Новый адрес**.
   
    ![New Address](./media/mimecast-admin-console-tutorial/ic795005.png "New Address") (Новый адрес)

5. В окне нового адреса выполните следующие действия.
   
    ![Сохранить](./media/mimecast-admin-console-tutorial/ic795006.png "Сохранить")
   
    a. В соответствующие текстовые поля введите атрибуты **Email Address** (Адрес электронной почты), **Global Name** (Глобальное имя), **Password** (Пароль) и **Confirm Password** (Подтверждение пароля) действительной учетной записи Azure AD, которую вы хотите подготовить.

    b. Выберите команду **Сохранить**.

>[!NOTE]
>Вы можете использовать любые другие средства создания учетных записей консоли администрирования Mimecast или API, предоставляемые консолью администрирования Mimecast для подготовки учетных записей пользователей Azure AD. 

### <a name="test-single-sign-on"></a>Проверка единого входа 

В этом разделе описано, как проверить конфигурацию единого входа Azure AD с помощью панели доступа.

Щелкнув плитку "Mimecast Admin Console" (Консоль администрирования Mimecast) на панели доступа, вы автоматически войдете в приложение консоли администрирования Mimecast, для которого настроили единый вход. См. дополнительные сведения о [панели доступа](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)

## <a name="additional-resources"></a>Дополнительные ресурсы

- [Список учебников по интеграции приложений SaaS с Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Что такое условный доступ в Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

