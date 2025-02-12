---
title: Руководство по Интеграция Azure Active Directory с TINFOIL SECURITY | Документация Майкрософт
description: Узнайте, как настроить единый вход Azure Active Directory в TINFOIL SECURITY.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: da02da92-e3b0-4c09-ad6c-180882b0f9f8
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/27/2019
ms.author: jeedes
ms.openlocfilehash: 627a324c580661220712a0598a996844fac0c169
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "67088605"
---
# <a name="tutorial-azure-active-directory-integration-with-tinfoil-security"></a>Руководство по Интеграция Azure Active Directory с TINFOIL SECURITY

В этом руководстве описано, как интегрировать TINFOIL SECURITY с Azure Active Directory (Azure AD).
Интеграция TINFOIL SECURITY с Azure AD имеет следующие преимущества:

* С помощью Azure AD вы можете контролировать, у кого есть доступ к приложению TINFOIL SECURITY.
* Вы можете включить автоматический вход для пользователей (единый вход) в TINFOIL SECURITY с помощью их учетных записей Azure AD.
* Вы можете управлять учетными записями централизованно на портале Azure.

Дополнительные сведения об интеграции приложений SaaS с Azure AD см. в статье [Единый вход в приложениях в Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Если у вас еще нет подписки Azure, [создайте бесплатную учетную запись Azure](https://azure.microsoft.com/free/), прежде чем начинать работу.

## <a name="prerequisites"></a>Предварительные требования

Чтобы настроить интеграцию Azure AD с приложением TINFOIL SECURITY, вам потребуется:

* подписка Azure AD (если у вас нет среды Azure AD, вы можете получить пробную версию на один месяц по [этой ссылке](https://azure.microsoft.com/pricing/free-trial/));
* подписка TINFOIL SECURITY с поддержкой единого входа.

## <a name="scenario-description"></a>Описание сценария

В рамках этого руководства вы настроите и проверите единый вход Azure AD в тестовой среде.

* TINFOIL SECURITY поддерживает единый вход, инициированный **поставщиком удостоверений**.

## <a name="adding-tinfoil-security-from-the-gallery"></a>Добавление TINFOIL SECURITY из коллекции

Чтобы настроить интеграцию TINFOIL SECURITY с Azure AD, вам нужно добавить TINFOIL SECURITY из коллекции в список управляемых приложений SaaS.

**Чтобы добавить TINFOIL SECURITY из коллекции, сделайте следующее.**

1. На **[портале Azure](https://portal.azure.com)** в области навигации слева щелкните значок **Azure Active Directory**.

    ![Кнопка Azure Active Directory](common/select-azuread.png)

2. Перейдите в колонку **Корпоративные приложения** и выберите **Все приложения**.

    ![Колонка "Корпоративные приложения"](common/enterprise-applications.png)

3. Чтобы добавить новое приложение, в верхней части диалогового окна нажмите кнопку **Создать приложение**.

    ![Кнопка "Создать приложение"](common/add-new-app.png)

4. В поле поиска введите **TINFOIL SECURITY**, выберите **TINFOIL SECURITY** на панели результатов и нажмите кнопку **Добавить**, чтобы добавить это приложение.

     ![TINFOIL SECURITY в списке результатов](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Настройка и проверка единого входа в Azure AD

В этом разделе описана настройка и проверка единого входа Azure AD в TINFOIL SECURITY с использованием тестового пользователя **Britta Simon**.
Для обеспечения работы единого входа необходимо установить связь между пользователем Azure AD и соответствующим пользователем в TINFOIL SECURITY.

Чтобы настроить и проверить единый вход Azure AD в TINFOIL SECURITY, выполните следующие действия.

1. **[Настройка единого входа Azure AD](#configure-azure-ad-single-sign-on)** необходима, чтобы пользователи могли использовать эту функцию.
2. **[Настройка единого входа в TINFOIL SECURITY](#configure-tinfoil-security-single-sign-on)** необходима, чтобы настроить параметры единого входа на стороне приложения.
3. **[Создание тестового пользователя Azure AD](#create-an-azure-ad-test-user)** требуется для проверки работы единого входа Azure AD от имени пользователя Britta Simon.
4. **[Назначение тестового пользователя Azure AD](#assign-the-azure-ad-test-user)** необходимо, чтобы разрешить пользователю Britta Simon использовать единый вход Azure AD.
5. **[Создание тестового пользователя TINFOIL SECURITY](#create-tinfoil-security-test-user)** требуется для того, чтобы в TINFOIL SECURITY существовал пользователь Britta Simon, связанный с одноименным пользователем в Azure AD.
6. **[Проверка единого входа](#test-single-sign-on)** необходима, чтобы проверить работу конфигурации.

### <a name="configure-azure-ad-single-sign-on"></a>Настройка единого входа Azure AD

В этом разделе описано включение единого входа Azure AD на портале Azure.

Чтобы настроить единый вход Azure AD в TINFOIL SECURITY, сделайте следующее.

1. На [портале Azure](https://portal.azure.com/) на странице интеграции с приложением **TINFOIL SECURITY** выберите **Единый вход**.

    ![Ссылка "Настройка единого входа"](common/select-sso.png)

2. В диалоговом окне **Выбрать метод единого входа** выберите режим **SAML/WS-Fed**, чтобы включить единый вход.

    ![Режим выбора единого входа](common/select-saml-option.png)

3. На странице **Настройка единого входа с помощью SAML** щелкните **Изменить**, чтобы открыть диалоговое окно **Базовая конфигурация SAML**.

    ![Правка базовой конфигурации SAML](common/edit-urls.png)

4. В разделе **Базовая конфигурация SAML** не нужно выполнять никаких действий, так как приложение уже предварительно интегрировано с Azure.

    ![Сведения о домене и URL-адресах единого входа для приложения TINFOIL SECURITY](common/preintegrated.png)

5. Приложение TINFOIL SECURITY предусматривает использование проверочных утверждений SAML в определенном формате, что предполагает добавление настраиваемых сопоставлений атрибутов в конфигурацию атрибутов токена SAML. На следующем снимке экрана показан список атрибутов по умолчанию. Нажмите кнопку **Изменить**, чтобы открыть диалоговое окно **Атрибуты пользователя**.

        ![image](common/edit-attribute.png)

6. В дополнение к описанному выше приложение TINFOIL SECURITY ожидает несколько дополнительных атрибутов в ответе SAML. В разделе **Утверждения пользователя** диалогового окна **Атрибуты пользователя** выполните следующие действия, чтобы добавить атрибут токена SAML, как показано в приведенной ниже таблице.

    | ИМЯ | Исходный атрибут |
    | ------------------- | -------------|
    | accountid | UXXXXXXXXXXXXX |

    a. Щелкните **Добавить новое утверждение**, чтобы открыть диалоговое окно **Управление утверждениями пользователя**.

    ![image](common/new-save-attribute.png)

    ![image](common/new-attribute-details.png)

    b. В текстовом поле **Имя** введите имя атрибута, отображаемое для этой строки.

    c. Оставьте пустым поле **Пространство имен**.

    d. В качестве источника выберите **Атрибут**.

    д. В текстовое поле **Атрибут источника** вставьте идентификатор учетной записи, который вы получите позже при работе с этим руководством.

    Е. Нажмите кнопку **ОК**.

    ж. Выберите команду **Сохранить**.

7. В разделе **Сертификат подписи SAML** щелкните кнопку **Правка**, чтобы открыть диалоговое окно **Сертификат подписи SAML**.

    ![Изменить сертификат подписи SAML](common/edit-certificate.png)

8. В разделе **Сертификат подписи SAML** скопируйте значение **Отпечаток** и сохраните его на компьютере.

    ![Копирование значения "Отпечаток"](common/copy-thumbprint.png)

9. Скопируйте требуемый URL-адрес из раздела **Настройка TINFOIL SECURITY**.

    ![Копирование URL-адресов настройки](common/copy-configuration-urls.png)

    а) URL-адрес входа.

    b. Идентификатор Azure AD

    c. URL-адрес выхода.

### <a name="configure-tinfoil-security-single-sign-on"></a>Настройка единого входа в TINFOIL SECURITY

1. В другом окне веб-браузера войдите на свой корпоративный веб-сайт TINFOIL SECURITY в качестве администратора.

2. На панели инструментов в верхней части экрана щелкните **Моя учетная запись**.
   
    ![Панель мониторинга](./media/tinfoil-security-tutorial/ic798971.png "Панель мониторинга")

3. Щелкните **Security**(Безопасность).
   
    ![Безопасность](./media/tinfoil-security-tutorial/ic798972.png "Безопасность")

4. На странице настроек **Единый вход** выполните следующие действия.
   
    ![Единый вход](./media/tinfoil-security-tutorial/ic798973.png "Единый вход")
   
    a. Выберите **Включить SAML**.
   
    b. Щелкните **Настроить вручную**.
   
    c. В текстовое поле **SAML Post URL** (URL-адрес POST SAML) вставьте **URL-адрес входа**, скопированный на портале Azure.
   
    d. В текстовое поле **SAML Certificate Fingerprint** (Отпечаток сертификата SAML) вставьте значение **Отпечаток**, скопированное в разделе **Сертификат подписи SAML**.
  
    д. Скопируйте значение **Your Account ID** (Ваш идентификатор учетной записи) и вставьте его в текстовое поле **Значение атрибута** в разделе **Добавление атрибута** на портале Azure.
   
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
  
    b. В поле **Имя пользователя** введите brittasimon@yourcompanydomain.extension. Например BrittaSimon@contoso.com.

    c. Установите флажок **Показать пароль** и запишите значение, которое отображается в поле "Пароль".

    d. Нажмите кнопку **Создать**.

### <a name="assign-the-azure-ad-test-user"></a>Назначение тестового пользователя Azure AD

В этом разделе описано, как разрешить пользователю Britta Simon использовать единый вход Azure, предоставив этому пользователю доступ к TINFOIL SECURITY.

1. На портале Azure выберите **Корпоративные приложения**, **Все приложения**, а затем — **TINFOIL SECURITY**.

    ![Колонка "Корпоративные приложения"](common/enterprise-applications.png)

2. Из списка приложений выберите **TINFOIL SECURITY**.

    ![Ссылка на TINFOIL SECURITY в списке "Приложения"](common/all-applications.png)

3. В меню слева выберите **Пользователи и группы**.

    ![Ссылка "Пользователи и группы"](common/users-groups-blade.png)

4. Нажмите кнопку **Добавить пользователя**, а затем в диалоговом окне **Добавление назначения** выберите **Пользователи и группы**.

    ![Область "Добавление назначения"](common/add-assign-user.png)

5. В диалоговом окне **Пользователи и группы** из списка пользователей выберите **Britta Simon**, а затем в верхней части экрана нажмите кнопку **Выбрать**.

6. Если ожидается, что в утверждении SAML будет получено какое-либо значение роли, то в диалоговом окне **Выбор ролей** нужно выбрать соответствующую роль для пользователя из списка и затем нажать кнопку **Выбрать**, расположенную в нижней части экрана.

7. В диалоговом окне **Добавление назначения** нажмите кнопку **Назначить**.

### <a name="create-tinfoil-security-test-user"></a>Создание тестового пользователя в TINFOIL SECURITY

Чтобы пользователи Azure AD могли выполнить вход в TINFOIL SECURITY, они должны быть подготовлены для TINFOIL SECURITY. В случае TINFOIL SECURITY подготовка выполняется вручную.

**Чтобы подготовить пользователя, выполните следующие действия.**

1. Если пользователь является частью учетной записи Enterprise, то для создания учетной записи пользователя необходимо обратиться к [группе поддержки TINFOIL SECURITY](https://www.tinfoilsecurity.com/contact).

1. Если пользователь является обычным пользователем SaaS TINFOIL SECURITY, то он может добавить участника совместной работы на любой из своих сайтов. Это активирует процесс, который отправит приглашение по указанному электронному адресу для создания учетной записи пользователя TINFOIL SECURITY.

> [!NOTE]
> Вы можете использовать любые другие инструменты создания учетных записей пользователя TINFOIL SECURITY или API, предоставляемые TINFOIL SECURITY для подготовки учетных записей пользователя Azure Active Directory.
> 

### <a name="test-single-sign-on"></a>Проверка единого входа 

В этом разделе описано, как проверить конфигурацию единого входа Azure AD с помощью панели доступа.

Щелкнув плитку TINFOIL SECURITY на Панели доступа, вы автоматически войдете в приложение TINFOIL SECURITY, для которого настроили единый вход. См. дополнительные сведения о [панели доступа](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)

## <a name="additional-resources"></a>Дополнительные ресурсы

- [Список учебников по интеграции приложений SaaS с Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Что такое условный доступ в Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

