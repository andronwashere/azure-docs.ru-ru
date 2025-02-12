---
title: Руководство по Интеграция Azure Active Directory с Nuclino | Документация Майкрософт
description: Узнайте, как настроить единый вход между Azure Active Directory и Nuclino.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 74bbab82-5581-4dcf-8806-78f77c746968
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/05/2019
ms.author: jeedes
ms.openlocfilehash: 010055d994a5cdc4dd5540fd80d6dd81c44a1e3c
ms.sourcegitcommit: 6a42dd4b746f3e6de69f7ad0107cc7ad654e39ae
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/07/2019
ms.locfileid: "67612819"
---
# <a name="tutorial-azure-active-directory-integration-with-nuclino"></a>Руководство по Интеграция Azure Active Directory с Nuclino

В этом руководстве описано, как интегрировать Nuclino с Azure Active Directory (Azure AD).
Интеграция Azure AD с приложением Nuclino обеспечивает следующие преимущества:

* С помощью Azure AD вы можете контролировать доступ к Nuclino.
* Вы можете включить автоматический вход для пользователей в Nuclino (единый вход) с помощью учетной записи Azure AD.
* Вы можете управлять учетными записями централизованно на портале Azure.

Дополнительные сведения об интеграции приложений SaaS с Azure AD см. в статье [Единый вход в приложениях в Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Если у вас еще нет подписки Azure, [создайте бесплатную учетную запись Azure](https://azure.microsoft.com/free/), прежде чем начинать работу.

## <a name="prerequisites"></a>Предварительные требования

Чтобы настроить интеграцию Azure AD с Nuclino, вам потребуется:

* подписка Azure AD (если у вас нет среды Azure AD, вы можете получить пробную версию на один месяц по [этой ссылке](https://azure.microsoft.com/pricing/free-trial/));
* подписка Nuclino с поддержкой единого входа.

## <a name="scenario-description"></a>Описание сценария

В рамках этого руководства вы настроите и проверите единый вход Azure AD в тестовой среде.

* Nuclino поддерживает единый вход, инициируемый **поставщиком услуг** и **поставщиком удостоверений**.

* Nuclino поддерживает **JIT**-подготовку пользователей.

## <a name="adding-nuclino-from-the-gallery"></a>добавление Nuclino из коллекции;

Чтобы настроить интеграцию Nuclino с Azure AD, необходимо добавить Nuclino из коллекции в список управляемых приложений SaaS.

**Чтобы добавить Nuclino из коллекции, выполните следующие действия:**

1. На **[портале Azure](https://portal.azure.com)** в области навигации слева щелкните значок **Azure Active Directory**.

    ![Кнопка Azure Active Directory](common/select-azuread.png)

2. Перейдите в колонку **Корпоративные приложения** и выберите **Все приложения**.

    ![Колонка "Корпоративные приложения"](common/enterprise-applications.png)

3. Чтобы добавить новое приложение, в верхней части диалогового окна нажмите кнопку **Создать приложение**.

    ![Кнопка "Создать приложение"](common/add-new-app.png)

4. В поле поиска введите **Nuclino**, на панели результатов выберите **Nuclino** и нажмите кнопку **Добавить**, чтобы добавить это приложение.

     ![Nuclino в списке результатов](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Настройка и проверка единого входа в Azure AD

В этом разделе описана настройка и проверка единого входа Azure AD в Nuclino с использованием тестового пользователя **Britta Simon**.
Для обеспечения работы единого входа необходимо установить связь между пользователем Azure AD и соответствующим пользователем в Nuclino.

Чтобы настроить и проверить единый вход Azure AD в Nuclino, вам потребуется выполнить действия в следующих стандартных блоках:

1. **[Настройка единого входа Azure AD](#configure-azure-ad-single-sign-on)** необходима, чтобы пользователи могли использовать эту функцию.
2. **[Настройка единого входа в Nuclino](#configure-nuclino-single-sign-on)** необходима, чтобы настроить параметры единого входа на стороне приложения.
3. **[Создание тестового пользователя Azure AD](#create-an-azure-ad-test-user)** требуется для проверки работы единого входа Azure AD от имени пользователя Britta Simon.
4. **[Назначение тестового пользователя Azure AD](#assign-the-azure-ad-test-user)** необходимо, чтобы разрешить пользователю Britta Simon использовать единый вход Azure AD.
5. **[Создание тестового пользователя приложения Nuclino](#create-nuclino-test-user)** требуется для того, чтобы в Nuclino существовал пользователь Britta Simon, связанный с одноименным пользователем в Azure AD.
6. **[Проверка единого входа](#test-single-sign-on)** необходима, чтобы проверить работу конфигурации.

### <a name="configure-azure-ad-single-sign-on"></a>Настройка единого входа Azure AD

В этом разделе описано включение единого входа Azure AD на портале Azure.

Чтобы настроить единый вход Azure AD в Nuclino, выполните следующие действия.

1. На [портале Azure](https://portal.azure.com/) на странице интеграции с приложением **Nuclino** щелкните **Единый вход**.

    ![Ссылка "Настройка единого входа"](common/select-sso.png)

2. В диалоговом окне **Выбрать метод единого входа** выберите режим **SAML/WS-Fed**, чтобы включить единый вход.

    ![Режим выбора единого входа](common/select-saml-option.png)

3. На странице **Настройка единого входа с помощью SAML** щелкните **Изменить**, чтобы открыть диалоговое окно **Базовая конфигурация SAML**.

    ![Правка базовой конфигурации SAML](common/edit-urls.png)

4. Если вы хотите настроить приложение в режиме, инициируемом **поставщиком удостоверений**, в разделе **Базовая конфигурация SAML** выполните следующие действия.

    ![Сведения о домене и URL-адресах единого входа для приложения Nuclino](common/idp-intiated.png)

    a. В текстовом поле **Идентификатор** введите URL-адрес в формате `https://api.nuclino.com/api/sso/<UNIQUE-ID>/metadata`.

    б) В текстовом поле **URL-адрес ответа** введите URL-адрес в формате `https://api.nuclino.com/api/sso/<UNIQUE-ID>/acs`.

    > [!NOTE]
    > Эти значения приведены для примера. Замените их фактическими значениями идентификатора и URL-адреса ответа из раздела **Authentication** (Проверка подлинности), который описывается далее в этом руководстве.

5. Чтобы настроить приложение для работы в режиме, инициируемом **поставщиком услуг**, щелкните **Задать дополнительные URL-адреса** и выполните следующие действия.

    ![Сведения о домене и URL-адресах единого входа для приложения Nuclino](common/metadata-upload-additional-signon.png)

    В текстовом поле **URL-адрес входа** введите URL-адрес в формате `https://app.nuclino.com/<UNIQUE-ID>/login`.

    > [!NOTE]
    > Это значение приведено для справки. Вместо него необходимо указать фактический URL-адрес входа. Для получения этого значения обратитесь в [группу поддержки клиентов Nuclino](mailto:contact@nuclino.com). Можно также посмотреть шаблоны в разделе **Базовая конфигурация SAML** на портале Azure.

6. Приложение Nuclino ожидает проверочные утверждения SAML в определенном формате, который требует добавить настраиваемые сопоставления атрибутов в конфигурацию атрибутов токена SAML. На следующем снимке экрана показан список атрибутов по умолчанию. Щелкните значок **Изменить**, чтобы открыть диалоговое окно  **Атрибуты пользователя** .

    ![image](common/edit-attribute.png)

7. В дополнение к описанному выше приложение Nuclino ожидает несколько дополнительных атрибутов в ответе SAML. В разделе **Утверждения пользователя** диалогового окна **Атрибуты пользователя** выполните следующие действия, чтобы добавить атрибут токена SAML, как показано в приведенной ниже таблице.

    | ИМЯ |  Исходный атрибут|
    | ---------------| --------- |
    | first_name | user.givenname |
    | last_name | user.surname |

    a. Щелкните **Добавить новое утверждение**, чтобы открыть диалоговое окно **Управление утверждениями пользователя**.

    ![image](common/new-save-attribute.png)

    ![image](common/new-attribute-details.png)

    b. В текстовом поле **Имя** введите имя атрибута, отображаемое для этой строки.

    c. Оставьте пустым поле **Пространство имен**.

    d. В качестве источника выберите **Атрибут**.

    д. В списке **Атрибут источника** введите значение атрибута, отображаемое для этой строки.

    Е. Нажмите кнопку **ОК**.

    ж. Выберите команду **Сохранить**.

8. На странице **Настройка единого входа с помощью SAML** в разделе **Сертификат подписи SAML** щелкните **Загрузить**, чтобы загрузить требуемый **сертификат (Base64)** из предложенных вариантов, и сохраните его на компьютере.

    ![Ссылка для скачивания сертификата](common/certificatebase64.png)

9. Требуемые URL-адреса вы можете скопировать из раздела **Настройка Nuclino**.

    ![Копирование URL-адресов настройки](common/copy-configuration-urls.png)

    а) URL-адрес входа.

    b. Идентификатор Azure AD

    c. URL-адрес выхода.

### <a name="configure-nuclino-single-sign-on"></a>Настройка единого входа в Nuclino

1. В другом окне веб-браузера войдите на сайт компании Nuclino в качестве администратора.

2. Щелкните **значок меню**.

    ![Настройка Nuclino](./media/nuclino-tutorial/configure1.png)

3. Щелкните **Azure AD SSO** (Единый вход в Azure AD) и выберите **Team settings** (Параметры команды) из раскрывающегося списка.

    ![Настройка Nuclino](./media/nuclino-tutorial/configure2.png)

4. Выберите **Authentication** (Проверка подлинности) на левой панели навигации.

    ![Настройка Nuclino](./media/nuclino-tutorial/configure3.png)

5. В разделе **Authentication** (Проверка подлинности) сделайте следующее:

    ![Настройка Nuclino](./media/nuclino-tutorial/configure4.png)

    a. Выберите **SAML-based single sign-on (SSO)** (Единый вход на основе SAML).

    b. Скопируйте значение **ACS URL (You need to copy and paste this to your SSO provider)** (URL-адрес ACS (вам нужно скопировать и вставить его в свой поставщик SSO)) и вставьте его в текстовое поле **URL-адрес ответа** в разделе **Базовая конфигурация SAML** на портале Azure.

    c. Скопируйте значение **Entity ID (You need to copy and paste this to your SSO provider)** (Идентификатор сущности (вам нужно скопировать и вставить его в свой поставщик SSO)) и вставьте его в текстовое поле **Идентификатор** в разделе **Базовая конфигурация SAML** на портале Azure.

    d. В текстовое поле **SSO URL** (URL-адрес единого входа) вставьте значение **URL-адрес входа**, скопированное на портале Azure.

    д. В текстовое поле **Идентификатор сущности** вставьте значение **Идентификатор Azure AD**, скопированное на портале Azure.

    Е. Откройте скачанный файл **сертификата (Base64)** в Блокноте. Скопируйте его содержимое в буфер обмена и вставьте в текстовое поле **Public certificate** (Общедоступный сертификат).

    ж. Нажмите кнопку **Сохранить изменения**.

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

В этом разделе описано, как предоставить пользователю Britta Simon доступ к Nuclino, чтобы он мог использовать единый вход Azure.

1. На портале Azure выберите **Корпоративные приложения**, **Все приложения**, а затем — **Nuclino**.

    ![Колонка "Корпоративные приложения"](common/enterprise-applications.png)

2. В списке приложений выберите **Nuclino**.

    ![Ссылка на Nuclino в списке приложений](common/all-applications.png)

3. В меню слева выберите **Пользователи и группы**.

    ![Ссылка "Пользователи и группы"](common/users-groups-blade.png)

4. Нажмите кнопку **Добавить пользователя**, а затем в диалоговом окне **Добавление назначения** выберите **Пользователи и группы**.

    ![Область "Добавление назначения"](common/add-assign-user.png)

5. В диалоговом окне **Пользователи и группы** из списка пользователей выберите **Britta Simon**, а затем в верхней части экрана нажмите кнопку **Выбрать**.

6. Если ожидается, что в утверждении SAML будет получено какое-либо значение роли, то в диалоговом окне **Выбор ролей** нужно выбрать соответствующую роль для пользователя из списка и затем нажать кнопку **Выбрать**, расположенную в нижней части экрана.

7. В диалоговом окне **Добавление назначения** нажмите кнопку **Назначить**.

### <a name="create-nuclino-test-user"></a>Создание тестового пользователя Nuclino

В этом разделе вы создадите в Nuclino пользователя с именем Britta Simon. Приложение Nuclino поддерживает JIT-подготовку пользователей, которая включена по умолчанию. В этом разделе никакие действия с вашей стороны не требуются. Если пользователь еще не существует в Nuclino, он создается после выполнения аутентификации.

> [!Note]
> Чтобы создать пользователя вручную, обратитесь к [группе поддержки Nuclino](mailto:contact@nuclino.com).

### <a name="test-single-sign-on"></a>Проверка единого входа 

В этом разделе описано, как проверить конфигурацию единого входа Azure AD с помощью панели доступа.

Щелкнув элемент "Nuclino" на Панели доступа, вы автоматически войдете в приложение Nuclino, для которого настроили единый вход. См. дополнительные сведения о [панели доступа](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)

## <a name="additional-resources"></a>Дополнительные ресурсы

- [Список учебников по интеграции приложений SaaS с Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Что такое условный доступ в Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

