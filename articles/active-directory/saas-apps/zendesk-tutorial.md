---
title: Руководство по Интеграция Azure Active Directory с Zendesk | Документация Майкрософт
description: Узнайте, как настроить единый вход между Azure Active Directory и Zendesk.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: 9d7c91e5-78f5-4016-862f-0f3242b00680
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 02/14/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: c4d161295f236c711e8efbe23deb3c8ba5cad985
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "67086309"
---
# <a name="tutorial-azure-active-directory-integration-with-zendesk"></a>Руководство по Интеграция Azure Active Directory с Zendesk

В этом руководстве описано, как интегрировать Zendesk с Azure Active Directory (Azure AD).
Интеграция Azure AD с приложением Zendesk обеспечивает следующие преимущества.

* С помощью Azure AD вы можете управлять доступом к Zendesk.
* Вы можете включить автоматический вход пользователей в Zendesk (единый вход) с помощью учетных записей Azure AD.
* Вы можете управлять учетными записями централизованно на портале Azure.

Дополнительные сведения об интеграции приложений SaaS с Azure AD см. в статье [Единый вход в приложениях в Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Если у вас еще нет подписки Azure, [создайте бесплатную учетную запись Azure](https://azure.microsoft.com/free/), прежде чем начинать работу.

## <a name="prerequisites"></a>Предварительные требования

Чтобы настроить интеграцию Azure AD с Zendesk, вам потребуется:

* подписка Azure AD (если у вас нет среды Azure AD, вы можете получить пробную версию на один месяц по [этой ссылке](https://azure.microsoft.com/pricing/free-trial/));
* Подписка Zendesk с поддержкой единого входа.

## <a name="scenario-description"></a>Описание сценария

В рамках этого руководства вы настроите и проверите единый вход Azure AD в тестовой среде.

* Zendesk поддерживает единый вход инициированного **пакета обновления**.

* Zendesk поддерживает [**автоматическую** подготовку пользователей](zendesk-provisioning-tutorial.md).

## <a name="adding-zendesk-from-the-gallery"></a>Добавление Zendesk из коллекции

Чтобы настроить интеграцию Zendesk с Azure AD, необходимо добавить Zendesk из коллекции в список управляемых приложений SaaS.

**Чтобы добавить Zendesk из коллекции, выполните следующие действия.**

1. На **[портале Azure](https://portal.azure.com)** в области навигации слева щелкните значок **Azure Active Directory**.

    ![Кнопка Azure Active Directory](common/select-azuread.png)

2. Перейдите в колонку **Корпоративные приложения** и выберите **Все приложения**.

    ![Колонка "Корпоративные приложения"](common/enterprise-applications.png)

3. Чтобы добавить новое приложение, в верхней части диалогового окна нажмите кнопку **Создать приложение**.

    ![Кнопка "Создать приложение"](common/add-new-app.png)

4. В поле поиска введите **Zendesk**, выберите **Zendesk** на панели результатов и нажмите кнопку **Добавить**, чтобы добавить это приложение.

     ![Zendesk в списке результатов](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Настройка и проверка единого входа в Azure AD

В этом разделе описана настройка и проверка единого входа Azure AD с приложением Zendesk с использованием тестового пользователя **Britta Simon**.
Для обеспечения работы единого входа необходимо установить связь между пользователем Azure AD и соответствующим пользователем в Zendesk.

Чтобы настроить и проверить единый вход Azure AD в Zendesk, вам потребуется выполнить действия в следующих стандартных блоках:

1. **[Настройка единого входа Azure AD](#configure-azure-ad-single-sign-on)** необходима, чтобы пользователи могли использовать эту функцию.
2. **[Настройка единого входа в Zendesk](#configure-zendesk-single-sign-on)** необходима, чтобы настроить параметры единого входа на стороне приложения.
3. **[Создание тестового пользователя Azure AD](#create-an-azure-ad-test-user)** требуется для проверки работы единого входа Azure AD от имени пользователя Britta Simon.
4. **[Назначение тестового пользователя Azure AD](#assign-the-azure-ad-test-user)** необходимо, чтобы разрешить пользователю Britta Simon использовать единый вход Azure AD.
5. **[Создание тестового пользователя Zendesk](#create-zendesk-test-user)** требуется для создания в Zendesk пользователя Britta Simon, связанного с представлением этого же пользователя в Azure AD.
6. **[Проверка единого входа](#test-single-sign-on)** необходима, чтобы проверить работу конфигурации.

### <a name="configure-azure-ad-single-sign-on"></a>Настройка единого входа Azure AD

В этом разделе описано включение единого входа Azure AD на портале Azure.

Чтобы настроить единый вход Azure AD в Zendesk, сделайте следующее:

1. На [портале Azure](https://portal.azure.com/) на странице интеграции с приложением **Zendesk** выберите **Единый вход**.

    ![Ссылка "Настройка единого входа"](common/select-sso.png)

2. В диалоговом окне **Выбрать метод единого входа** выберите режим **SAML/WS-Fed**, чтобы включить единый вход.

    ![Режим выбора единого входа](common/select-saml-option.png)

3. На странице **Настройка единого входа с помощью SAML** щелкните **Изменить**, чтобы открыть диалоговое окно **Базовая конфигурация SAML**.

    ![Правка базовой конфигурации SAML](common/edit-urls.png)

4. В разделе **Базовая конфигурация SAML** выполните приведенные ниже действия.

    ![Сведения о домене и URL-адресах единого входа для приложения Zendesk](common/sp-identifier.png)

    a. В текстовом поле **URL-адрес для входа** введите URL-адрес в следующем формате: `https://<subdomain>.zendesk.com`.

   b. В текстовом поле **Идентификатор (сущности)** введите URL-адрес в следующем формате: `https://<subdomain>.zendesk.com`.

    > [!NOTE]
    > Эти значения приведены для примера. Необходимо обновить эти значения действующим URL-адресом для входа и идентификатором. Чтобы получить эти значения, обратитесь в [службу поддержки клиентов Zendesk](https://support.zendesk.com/hc/en-us/articles/203663676-Using-SAML-for-single-sign-on-Professional-and-Enterprise). Можно также посмотреть шаблоны в разделе **Базовая конфигурация SAML** на портале Azure.

5. Приложение Zendesk ожидает утверждения SAML в определенном формате. Обязательные атрибуты SAML отсутствуют, но при необходимости вы можете управлять ими из раздела **Атрибуты пользователя** на странице интеграции приложения. На странице **Настройка единого входа с помощью SAML** нажмите кнопку **Изменить**, чтобы открыть диалоговое окно **Атрибуты пользователя**.

    ![image](common/edit-attribute.png)

6. В разделе **Утверждения пользователя** диалогового окна **Атрибуты пользователя** настройте атрибут токена SAML, как показано на рисунке выше, и выполните следующие действия.

    a. Щелкните **Добавить новое утверждение**, чтобы открыть диалоговое окно **Управление утверждениями пользователя**.

    ![image](common/new-save-attribute.png)

    ![image](common/new-attribute-details.png)

    b. В текстовом поле **Имя** введите имя атрибута, отображаемое для этой строки.

    c. Оставьте пустым поле **Пространство имен**.

    d. В качестве источника выберите **Атрибут**.

    д. Из списка **Атрибут источника** выберите значение соответствующего атрибута.

    Е. Нажмите кнопку **ОК**.

    ж. Выберите команду **Сохранить**.

    > [!NOTE]
    > Используйте атрибуты расширения для добавления атрибутов, которые отсутствуют в Azure AD по умолчанию. Щелкните [User attributes that can be set in SAML](https://support.zendesk.com/hc/articles/203663676-Using-SAML-for-single-sign-on-Professional-and-Enterprise-) (Атрибуты пользователя, которые можно настроить в SAML), чтобы получить полный список атрибутов SAML, которые принимает **Zendesk**.

7. На странице **Настройка единого входа с помощью SAML** откройте раздел **Сертификат подписи SAML** и скопируйте **Отпечаток**, а затем сохраните его на компьютере.

    ![Копирование значения "Отпечаток"](common/copy-certificatethumbprint.png)

8. Требуемый URL-адрес вы можете скопировать из раздела **Настройка Zendesk**.

    ![Копирование URL-адресов настройки](common/copy-configuration-urls.png)

    а) URL-адрес входа.

    б) Идентификатор Azure AD.

    в) URL-адрес выхода.

9. Существует два способа настройки Zendesk: автоматический и настройка вручную.
  
10. Для автоматизации настройки в Zendesk необходимо установить **Расширение браузера "Безопасный вход в мои приложения"** , щелкнув **Установить расширение**.

    ![image](./media/zendesk-tutorial/install_extension.png)

11. Чтобы перейти к приложению Zendesk после добавления расширения в браузере, щелкните **Setup Zendesk** (Установка Zendesk). После этого укажите учетные данные администратора для входа в Zendesk. Расширение браузера автоматически настроит приложение и автоматизирует его в разделе **Настройка единого входа Zendesk**.

    ![image](./media/zendesk-tutorial/d2_saml.png)

### <a name="configure-zendesk-single-sign-on"></a>Настройка единого входа Zendesk

1. Если необходимо вручную настроить Zendesk, откройте новое окно веб-браузера, зайдите на сайт компании Zendesk в роли администратора и выполните следующие шаги.

2. Щелкните **Администратор**.

3. В области навигации слева щелкните **Settings** (Параметры), а затем щелкните **Security** (Безопасность).

4. На странице **Безопасность** сделайте следующее:

    ![Безопасность](././media/zendesk-tutorial/ic773089.png "Безопасность")

    ![Единый вход](././media/zendesk-tutorial/ic773090.png "Единый вход")

    a. Щелкните вкладку **Admin & Agents** (Администраторы и агенты).

    b. Выберите **Single sign-on (SSO) and SAML** (Единый вход и SAML), а затем щелкните **SAML**.

    c. В текстовое поле **URL-адрес единого входа SAML** вставьте **URL-адрес входа**, скопированный на портале Azure.

    d. В текстовое поле **URL-адрес удаленного выхода** вставьте **URL-адрес выхода**, скопированный на портале Azure.

    д. В текстовое поле **Certificate Fingerprint** (Отпечаток сертификата) вставьте значение **Отпечаток**, которое вы скопировали на портале Azure.

    Е. Выберите команду **Сохранить**.

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

В этом разделе описано, как предоставить пользователю Britta Simon доступ к Zendesk, чтобы он мог использовать единый вход Azure.

1. На портале Azure выберите **Корпоративные приложения**, **Все приложения**, а затем **Zendesk**.

    ![Колонка "Корпоративные приложения"](common/enterprise-applications.png)

2. В списке приложений введите и выберите **Zendesk**.

    ![Ссылка на Zendesk в списке приложений](common/all-applications.png)

3. В меню слева выберите **Пользователи и группы**.

    ![Ссылка "Пользователи и группы"](common/users-groups-blade.png)

4. Нажмите кнопку **Добавить пользователя**, а затем в диалоговом окне **Добавление назначения** выберите **Пользователи и группы**.

    ![Область "Добавление назначения"](common/add-assign-user.png)

5. В диалоговом окне **Пользователи и группы** из списка пользователей выберите **Britta Simon**, а затем в верхней части экрана нажмите кнопку **Выбрать**.

6. Если ожидается, что в утверждении SAML будет получено какое-либо значение роли, то в диалоговом окне **Выбор ролей** нужно выбрать соответствующую роль для пользователя из списка и затем нажать кнопку **Выбрать**, расположенную в нижней части экрана.

7. В диалоговом окне **Добавление назначения** нажмите кнопку **Назначить**.

### <a name="create-zendesk-test-user"></a>Создание тестового пользователя Zendesk

Цель этого раздела — создать пользователя с именем Britta Simon в Zendesk. Zendesk поддерживает автоматическую подготовку пользователей, которая по умолчанию включена. Дополнительные сведения о настройке автоматической подготовки пользователей можно найти [здесь](Zendesk-provisioning-tutorial.md).

**Если необходимо создать пользователя вручную, выполните следующие действия:**

> [!NOTE]
> Учетные записи с ролью **Конечный пользователь** подготавливаются автоматически при входе. Учетные записи с ролями **Агент** и **Администратор** необходимо вручную подготовить в **Zendesk** перед выполнением входа.

1. Войдите в клиент **Zendesk** .

2. Откройте вкладку **Список клиентов** .

3. Выберите вкладку **User** (Пользователь) и нажмите кнопку **Add** (Добавить).

    ![Добавление пользователя](././media/zendesk-tutorial/ic773632.png "Добавление пользователя")
4. Введите **имя** и **электронный адрес** существующей учетной записи Azure AD, которую нужно подготовить, а затем нажмите кнопку **Сохранить**.

    ![Новый пользователь](././media/zendesk-tutorial/ic773633.png "Новый пользователь")

> [!NOTE]
> Вы можете использовать любые другие средства создания учетной записи пользователя Zendesk или API, предоставляемые Zendesk, для подготовки учетных записей пользователей AAD.

### <a name="test-single-sign-on"></a>Проверка единого входа 

В этом разделе описано, как проверить конфигурацию единого входа Azure AD с помощью панели доступа.

Щелкнув плитку "Zendesk" на панели доступа, вы автоматически войдете в приложение Zendesk, для которого настроили единый вход. См. дополнительные сведения о [панели доступа](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)

## <a name="additional-resources"></a>Дополнительные ресурсы

- [Список учебников по интеграции приложений SaaS с Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Что такое условный доступ в Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Руководство по настройке Google Apps для автоматической подготовки пользователей](zendesk-provisioning-tutorial.md)