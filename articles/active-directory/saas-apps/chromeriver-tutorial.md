---
title: Руководство по Интеграция Azure Active Directory с Chromeriver | Документация Майкрософт
description: Узнайте, как настроить единый вход Azure Active Directory в Chromeriver.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 445c5600-e340-4724-a9cb-3cfaf5770b70
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 02/14/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 60f1c5862fd152beaff135c442108b3694613d52
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "67105844"
---
# <a name="tutorial-azure-active-directory-integration-with-chromeriver"></a>Руководство по Интеграция Azure Active Directory с Chromeriver

В этом руководстве описано, как интегрировать Chromeriver с Azure Active Directory (Azure AD).
Интеграция Chromeriver с Azure AD обеспечивает следующие преимущества:

* С помощью Azure AD вы можете контролировать доступ к Chromeriver.
* Учетные записи Azure AD позволяют включить автоматический вход пользователей в Chromeriver (единый вход).
* Вы можете управлять учетными записями централизованно на портале Azure.

Дополнительные сведения об интеграции приложений SaaS с Azure AD см. в статье [Единый вход в приложениях в Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Если у вас еще нет подписки Azure, [создайте бесплатную учетную запись Azure](https://azure.microsoft.com/free/), прежде чем начинать работу.

## <a name="prerequisites"></a>Предварительные требования

Чтобы настроить интеграцию Azure AD с Chromeriver, вам потребуется:

* подписка Azure AD (если у вас нет среды Azure AD, вы можете получить пробную версию на один месяц по [этой ссылке](https://azure.microsoft.com/pricing/free-trial/));
* подписка Chromeriver с поддержкой единого входа.

## <a name="scenario-description"></a>Описание сценария

В рамках этого руководства вы настроите и проверите единый вход Azure AD в тестовой среде.

* В Chromeriver поддерживается единый вход, инициированный **поставщиком удостоверений**.

## <a name="adding-chromeriver-from-the-gallery"></a>Добавление Chromeriver из коллекции

Чтобы настроить интеграцию Chromeriver с Azure AD, необходимо добавить Chromeriver из коллекции в список управляемых приложений SaaS.

**Чтобы добавить Chromeriver из коллекции, сделайте следующее.**

1. На **[портале Azure](https://portal.azure.com)** в области навигации слева щелкните значок **Azure Active Directory**.

    ![Кнопка Azure Active Directory](common/select-azuread.png)

2. Перейдите в колонку **Корпоративные приложения** и выберите **Все приложения**.

    ![Колонка "Корпоративные приложения"](common/enterprise-applications.png)

3. Чтобы добавить новое приложение, в верхней части диалогового окна нажмите кнопку **Создать приложение**.

    ![Кнопка "Создать приложение"](common/add-new-app.png)

4. Чтобы добавить это приложение, в поле поиска введите **Chromeriver**, выберите **Chromeriver** на панели результатов и нажмите кнопку **Добавить**.

    ![Chromeriver в списке результатов](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Настройка и проверка единого входа в Azure AD

В рамках этого раздела вы настроите и проверите единый вход Azure AD в Chromeriver с использованием тестового пользователя **Britta Simon**.
Чтобы обеспечить единый вход, свяжите пользователя Azure AD с соответствующим пользователем в Chromeriver.

Чтобы настроить и проверить единый вход Azure AD в Chromeriver, вам потребуется выполнить действия в следующих стандартных блоках.

1. **[Настройка единого входа Azure AD](#configure-azure-ad-single-sign-on)** необходима, чтобы пользователи могли использовать эту функцию.
2. **[Настройка единого входа в Chromeriver](#configure-chromeriver-single-sign-on)** требуется, чтобы определить параметры единого входа на стороне приложения.
3. **[Создание тестового пользователя Azure AD](#create-an-azure-ad-test-user)** требуется для проверки работы единого входа Azure AD от имени пользователя Britta Simon.
4. **[Назначение тестового пользователя Azure AD](#assign-the-azure-ad-test-user)** необходимо, чтобы разрешить пользователю Britta Simon использовать единый вход Azure AD.
5. **[Создание тестового пользователя Chromeriver](#create-chromeriver-test-user)** требуется, чтобы в Chromeriver также существовал пользователь Britta Simon, связанный с одноименным пользователем в Azure AD.
6. **[Проверка единого входа](#test-single-sign-on)** необходима, чтобы проверить работу конфигурации.

### <a name="configure-azure-ad-single-sign-on"></a>Настройка единого входа Azure AD

В этом разделе описано включение единого входа Azure AD на портале Azure.

Чтобы настроить единый вход Azure AD в Chromeriver, сделайте следующее:

1. На [портале Azure](https://portal.azure.com/) на странице интеграции с приложением **Chromeriver** щелкните **Единый вход**.

    ![Ссылка "Настройка единого входа"](common/select-sso.png)

2. В диалоговом окне **Выбрать метод единого входа** выберите режим **SAML/WS-Fed**, чтобы включить единый вход.

    ![Режим выбора единого входа](common/select-saml-option.png)

3. На странице **Настройка единого входа с помощью SAML** щелкните **Изменить**, чтобы открыть диалоговое окно **Базовая конфигурация SAML**.

    ![Правка базовой конфигурации SAML](common/edit-urls.png)

4. На странице **Настройка единого входа с помощью SAML** выполните следующие действия.

    ![Сведения о домене и URL-адресах единого входа для приложения Chromeriver](common/idp-intiated.png)

    a. В текстовом поле **Идентификатор** введите URL-адрес в формате `https://<subdomain>.chromeriver.com`.

    б) В текстовом поле **URL-адрес ответа** введите URL-адрес в формате `https://<subdomain>.chromeriver.com/login/sso/saml/consume?customerId=<uniqueid>`.

    > [!NOTE]
    > Эти значения приведены для примера. Измените их на фактические значения идентификатора и URL-адреса ответа. Чтобы получить эти значения, обратитесь в [службу технической поддержки Chromeriver](https://www.chromeriver.com/services/support). Можно также посмотреть шаблоны в разделе **Базовая конфигурация SAML** на портале Azure.

5. На странице **Настройка единого входа с помощью SAML** в разделе **Сертификат подписи SAML** щелкните **Скачать**, чтобы скачать нужный вам **XML метаданных федерации**, и сохраните его на компьютере.

    ![Ссылка для скачивания сертификата](common/metadataxml.png)

6. Требуемые URL-адреса можно скопировать из раздела **настройки Chromeriver**.

    ![Копирование URL-адресов настройки](common/copy-configuration-urls.png)

    а) URL-адрес входа.

    б) Идентификатор Azure AD.

    в) URL-адрес выхода.

### <a name="configure-chromeriver-single-sign-on"></a>Настройка единого входа в Chromeriver

Чтобы настроить единый вход на стороне **Chromeriver**, отправьте скачанный **XML-файл метаданных федерации** и URL-адреса, скопированные на портале Azure, в [службу технической поддержки Chromeriver](https://www.chromeriver.com/services/support). Специалисты службы поддержки настроят подключение единого входа SAML на обеих сторонах.

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

В этом разделе описано, как позволить пользователю Britta Simon использовать единый вход Azure, предоставив ему доступ к Chromeriver.

1. На портале Azure последовательно выберите **Корпоративные приложения**, **Все приложения** и **Chromeriver**.

    ![Колонка "Корпоративные приложения"](common/enterprise-applications.png)

2. Из списка приложений выберите **Chromeriver**.

    ![Ссылка на Chromeriver в списке "Приложения"](common/all-applications.png)

3. В меню слева выберите **Пользователи и группы**.

    ![Ссылка "Пользователи и группы"](common/users-groups-blade.png)

4. Нажмите кнопку **Добавить пользователя**, а затем в диалоговом окне **Добавление назначения** выберите **Пользователи и группы**.

    ![Область "Добавление назначения"](common/add-assign-user.png)

5. В диалоговом окне **Пользователи и группы** из списка пользователей выберите **Britta Simon**, а затем в верхней части экрана нажмите кнопку **Выбрать**.

6. Если ожидается, что в утверждении SAML будет получено какое-либо значение роли, то в диалоговом окне **Выбор ролей** нужно выбрать соответствующую роль для пользователя из списка и затем нажать кнопку **Выбрать**, расположенную в нижней части экрана.

7. В диалоговом окне **Добавление назначения** нажмите кнопку **Назначить**.

### <a name="create-chromeriver-test-user"></a>Создание тестового пользователя Chromeriver

Чтобы пользователи Azure AD могли выполнить вход в Chromeriver, они должны быть подготовлены в Chromeriver. В случае Chromeriver учетные записи пользователей должны быть созданы [группой поддержки Chromeriver](https://www.chromeriver.com/services/support).

> [!NOTE]
> Вы можете использовать любые другие инструменты создания учетных записей пользователя Chromeriver или API, предоставляемые Chromeriver для подготовки учетных записей пользователя Azure Active Directory.

### <a name="test-single-sign-on"></a>Проверка единого входа

В этом разделе описано, как проверить конфигурацию единого входа Azure AD с помощью панели доступа.

Щелкнув плитку Chromeriver на Панели доступа, вы автоматически войдете в приложение Chromeriver, для которого настроили единый вход. См. дополнительные сведения о [панели доступа](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)

## <a name="additional-resources"></a>Дополнительные ресурсы

- [Список учебников по интеграции приложений SaaS с Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Что такое условный доступ в Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

