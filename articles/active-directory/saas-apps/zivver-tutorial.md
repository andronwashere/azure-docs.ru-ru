---
title: Руководство по Интеграция Azure Active Directory с ZIVVER | Документация Майкрософт
description: Узнайте, как настроить единый вход между Azure Active Directory и ZIVVER.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 64cb7ea0-df6c-4963-84d8-6f435980e2de
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 04/22/2019
ms.author: jeedes
ms.openlocfilehash: e18c5dfe3649f74a9f84c605785ca99748298bea
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "67086132"
---
# <a name="tutorial-azure-active-directory-integration-with-zivver"></a>Руководство по Интеграция Azure Active Directory с ZIVVER

В этом руководстве описано, как интегрировать ZIVVER с Azure Active Directory (Azure AD).
Интеграция Azure AD с приложением ZIVVER обеспечивает следующие преимущества:

* С помощью Azure AD вы можете контролировать доступ к ZIVVER.
* Вы можете включить автоматический вход пользователей в ZIVVER (единый вход) с помощью учетных записей Azure AD.
* Вы можете управлять учетными записями централизованно на портале Azure.

Дополнительные сведения об интеграции приложений SaaS с Azure AD см. в статье [Единый вход в приложениях в Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Если у вас еще нет подписки Azure, [создайте бесплатную учетную запись Azure](https://azure.microsoft.com/free/), прежде чем начинать работу.

## <a name="prerequisites"></a>Предварительные требования

Чтобы настроить интеграцию Azure AD с ZIVVER, вам потребуется:

* подписка Azure AD (если у вас нет среды Azure AD, вы можете получить [бесплатную учетную запись](https://azure.microsoft.com/free/));
* подписка ZIVVER с поддержкой единого входа.

## <a name="scenario-description"></a>Описание сценария

В рамках этого руководства вы настроите и проверите единый вход Azure AD в тестовой среде.

* ZIVVER поддерживает единый вход, инициируемый **поставщиком удостоверений**.

## <a name="adding-zivver-from-the-gallery"></a>Добавление ZIVVER из коллекции

Чтобы настроить интеграцию ZIVVER с Azure AD, необходимо добавить ZIVVER из коллекции в список управляемых приложений SaaS.

**Чтобы добавить ZIVVER из коллекции, сделайте следующее.**

1. На **[портале Azure](https://portal.azure.com)** в области навигации слева щелкните значок **Azure Active Directory**.

    ![Кнопка Azure Active Directory](common/select-azuread.png)

2. Перейдите в колонку **Корпоративные приложения** и выберите **Все приложения**.

    ![Колонка "Корпоративные приложения"](common/enterprise-applications.png)

3. Чтобы добавить новое приложение, в верхней части диалогового окна нажмите кнопку **Создать приложение**.

    ![Кнопка "Создать приложение"](common/add-new-app.png)

4. В поле поиска введите **ZIVVER**, выберите **ZIVVER** на панели результатов и нажмите кнопку **Добавить**, чтобы добавить это приложение.

     ![ZIVVER в списке результатов](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Настройка и проверка единого входа в Azure AD

В этом разделе описана настройка и проверка единого входа Azure AD в приложение ZIVVER с использованием тестового пользователя **Britta Simon**.
Для обеспечения работы единого входа необходимо установить связь между пользователем Azure AD и соответствующим пользователем в ZIVVER.

Чтобы настроить и проверить единый вход Azure AD в ZIVVER, выполните действия в следующих стандартных блоках:

1. **[Настройка единого входа Azure AD](#configure-azure-ad-single-sign-on)** необходима, чтобы пользователи могли использовать эту функцию.
2. **[Настройка единого входа в ZIVVER](#configure-zivver-single-sign-on)** необходима, чтобы настроить параметры единого входа на стороне приложения.
3. **[Создание тестового пользователя Azure AD](#create-an-azure-ad-test-user)** требуется для проверки работы единого входа Azure AD от имени пользователя Britta Simon.
4. **[Назначение тестового пользователя Azure AD](#assign-the-azure-ad-test-user)** необходимо, чтобы разрешить пользователю Britta Simon использовать единый вход Azure AD.
5. **[Создание тестового пользователя ZIVVER](#create-zivver-test-user)** требуется для того, чтобы в ZIVVER существовал пользователь Britta Simon, связанный с одноименным пользователем в Azure AD.
6. **[Проверка единого входа](#test-single-sign-on)** необходима, чтобы проверить работу конфигурации.

### <a name="configure-azure-ad-single-sign-on"></a>Настройка единого входа Azure AD

В этом разделе описано включение единого входа Azure AD на портале Azure.

Чтобы настроить единый вход Azure AD в ZIVVER, сделайте следующее.

1. На [портале Azure](https://portal.azure.com/) на странице интеграции с приложением **ZIVVER** щелкните **Единый вход**.

    ![Ссылка "Настройка единого входа"](common/select-sso.png)

2. В диалоговом окне **Выбрать метод единого входа** выберите режим **SAML/WS-Fed**, чтобы включить единый вход.

    ![Режим выбора единого входа](common/select-saml-option.png)

3. На странице **Настройка единого входа с помощью SAML** щелкните **Изменить**, чтобы открыть диалоговое окно **Базовая конфигурация SAML**.

    ![Правка базовой конфигурации SAML](common/edit-urls.png)

4. В разделе **Базовая конфигурация SAML** выполните приведенные ниже действия.

    ![Сведения о домене и URL-адресах единого входа приложения ZIVVER](common/idp-identifier.png)

    В текстовом поле **Идентификатор** введите URL-адрес: `https://app.zivver.com/SAML/Zivver`

5. Приложение ZIVVER ожидает проверочные утверждения SAML в определенном формате, что требует добавить настраиваемые сопоставления атрибутов в вашу конфигурацию атрибутов токена SAML. На следующем снимке экрана показан список атрибутов по умолчанию, когда  **nameidentifier**  сопоставляется с  **user.userprincipalname**. Приложение ZIVVER ожидает, что  **nameidentifier** будет сопоставляться с  **user.mail**, поэтому необходимо изменить сопоставление атрибутов, щелкнув значок  **Изменить** .

    ![image](common/edit-attribute.png)

6. В дополнение к описанному выше приложение ZIVVER ожидает несколько дополнительных атрибутов в ответе SAML. В разделе **Утверждения пользователя** диалогового окна **Атрибуты пользователя** выполните следующие действия, чтобы добавить атрибут токена SAML, как показано в приведенной ниже таблице.

    | ИМЯ | Пространство имен | Исходный атрибут|
    | ---------------| --------------- |
    | ZivverAccountKey | https:\//zivver.com/SAML/Attributes | user.objectid |

    >[!NOTE]
    >Если вы используете гибридную конфигурацию с локальной средой Active Directory и инструментом Azure AD Connect, значением должно быть `user.objectGUID`.

    a. Щелкните **Добавить новое утверждение**, чтобы открыть диалоговое окно **Управление утверждениями пользователя**.

    ![image](common/new-save-attribute.png)

    ![image](common/new-attribute-details.png)

    b. В текстовом поле **Имя** введите имя атрибута, отображаемое для этой строки.

    c. Оставьте пустым поле **Пространство имен**.

    d. В качестве источника выберите **Атрибут**.

    д. В списке **Атрибут источника** введите значение атрибута, отображаемое для этой строки.

    Е. Выберите команду **Сохранить**.

7. На странице **Настройка единого входа с помощью SAML** в разделе **Сертификат подписи SAML** щелкните **Скачать**, чтобы скачать **XML-файл метаданных федерации**, а затем щелкните значок**Копировать**, чтобы скопировать **URL-адрес метаданных федерации приложений** из предложенных вариантов, и сохраните его на своем компьютере.

    ![Ссылка для скачивания сертификата](./media/zivver-tutorial/metadataxmlurl.png)

8. Требуемые URL-адреса можно скопировать из раздела **Настройка ZIVVER**.

    ![Копирование URL-адресов настройки](common/copy-configuration-urls.png)

    а) URL-адрес входа.

    b. Идентификатор Azure AD

    c. URL-адрес выхода.

### <a name="configure-zivver-single-sign-on"></a>Настройка единого входа в ZIVVER

1. В другом окне веб-браузера войдите на свой корпоративный [сайт](https://app.zivver.com/login) ZIVVER в качестве администратора.

2. Щелкните значок **Organization settings** (Параметры организации) в нижней левой части окна браузера.

3. Выберите **Single sign-on** (Единый вход).

4. Откройте XML-файл метаданных федерации, скачанный с портала Azure.

5. В текстовое поле **Identity Provider metadata URL** (URL-адрес метаданных поставщика удостоверений) вставьте значение **URL-адрес метаданных федерации приложений**, который вы скопировали на портале Azure.

6. Установите флажок **Turn on SSO** (Включить единый вход).

7. Щелкните **СОХРАНИТЬ**.

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

В этом разделе описано, как разрешить пользователю Britta Simon использовать единый вход Azure, предоставив доступ к ZIVVER.

1. На портале Azure выберите **Корпоративные приложения**, **Все приложения**, а затем — **ZIVVER**.

    ![Колонка "Корпоративные приложения"](common/enterprise-applications.png)

2. В списке приложений выберите **ZIVVER**.

    ![Ссылка на ZIVVER в списке "Приложения"](common/all-applications.png)

3. В меню слева выберите **Пользователи и группы**.

    ![Ссылка "Пользователи и группы"](common/users-groups-blade.png)

4. Нажмите кнопку **Добавить пользователя**, а затем в диалоговом окне **Добавление назначения** выберите **Пользователи и группы**.

    ![Область "Добавление назначения"](common/add-assign-user.png)

5. В диалоговом окне **Пользователи и группы** из списка пользователей выберите **Britta Simon**, а затем в верхней части экрана нажмите кнопку **Выбрать**.

6. Если ожидается, что в утверждении SAML будет получено какое-либо значение роли, то в диалоговом окне **Выбор ролей** нужно выбрать соответствующую роль для пользователя из списка и затем нажать кнопку **Выбрать**, расположенную в нижней части экрана.

7. В диалоговом окне **Добавление назначения** нажмите кнопку **Назначить**.

### <a name="create-zivver-test-user"></a>Создание тестового пользователя ZIVVER

В этом разделе описано, как создать пользователя Britta Simon в приложении ZIVVER. Обратитесь к  [группе поддержки ZIVVER](https://support.zivver.com/), чтобы добавить пользователей на платформу ZIVVER. Перед использованием единого входа необходимо создать и активировать пользователей.

### <a name="test-single-sign-on"></a>Проверка единого входа 

В этом разделе описано, как проверить конфигурацию единого входа Azure AD с помощью панели доступа.

Щелкнув элемент "ZIVVER" на Панели доступа, вы автоматически войдете в приложение ZIVVER, для которого настроили единый вход. См. дополнительные сведения о [панели доступа](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)

## <a name="additional-resources"></a>Дополнительные ресурсы

- [Список учебников по интеграции приложений SaaS с Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Что такое условный доступ в Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

