---
title: Руководство по Интеграция Azure Active Directory с Communifire | Документация Майкрософт
description: Узнайте, как настроить единый вход между Azure Active Directory и Communifire.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: de2a164d-2115-43e7-a9ed-e54f483f4aeb
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 02/13/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: afa8c451790331e81a96485c7965110c968fe249
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "67104992"
---
# <a name="tutorial-azure-active-directory-integration-with-communifire"></a>Руководство по Интеграция Azure Active Directory с Communifire

В этом руководстве описано, как интегрировать Communifire с Azure Active Directory (Azure AD).
Интеграция Azure AD с приложением Communifire обеспечивает следующие преимущества.

* С помощью Azure AD вы можете контролировать доступ к Communifire.
* Можно включить автоматический вход пользователей (единый вход) в Communifire с помощью учетной записи Azure AD.
* Вы можете управлять учетными записями централизованно на портале Azure.

Дополнительные сведения об интеграции приложений SaaS с Azure AD см. в статье [Единый вход в приложениях в Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Если у вас еще нет подписки Azure, [создайте бесплатную учетную запись Azure](https://azure.microsoft.com/free/), прежде чем начинать работу.

## <a name="prerequisites"></a>Предварительные требования

Чтобы настроить интеграцию Azure AD с приложением Communifire, вам потребуется:

* подписка Azure AD (если у вас нет среды Azure AD, вы можете получить пробную версию на один месяц по [этой ссылке](https://azure.microsoft.com/pricing/free-trial/));
* подписка Communifire с поддержкой единого входа.

## <a name="scenario-description"></a>Описание сценария

В рамках этого руководства вы настроите и проверите единый вход Azure AD в тестовой среде.

* Communifire поддерживает единый вход, инициированный **поставщиком услуг и поставщиком удостоверений**.

* Communifire поддерживает **JIT**-подготовку пользователей.

## <a name="adding-communifire-from-the-gallery"></a>Добавление Communifire из коллекции

Чтобы настроить интеграцию Communifire с Azure AD, вам потребуется добавить Communifire из коллекции в список управляемых приложений SaaS.

**Чтобы добавить Communifire из коллекции, выполните следующие действия.**

1. На **[портале Azure](https://portal.azure.com)** в области навигации слева щелкните значок **Azure Active Directory**.

    ![Кнопка Azure Active Directory](common/select-azuread.png)

2. Перейдите в колонку **Корпоративные приложения** и выберите **Все приложения**.

    ![Колонка "Корпоративные приложения"](common/enterprise-applications.png)

3. Чтобы добавить новое приложение, в верхней части диалогового окна нажмите кнопку **Создать приложение**.

    ![Кнопка "Создать приложение"](common/add-new-app.png)

4. В поле поиска введите **Communifire**, выберите **Communifire** на панели результатов и нажмите кнопку **Добавить**, чтобы добавить это приложение.

     ![Communifire в списке результатов поиска](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Настройка и проверка единого входа в Azure AD

В этом разделе описана настройка и проверка единого входа Azure AD в Communifire с использованием тестового пользователя **Britta Simon**.
Для обеспечения работы единого входа необходимо установить связь между пользователем Azure AD и соответствующим пользователем в Communifire.

Чтобы настроить и проверить единый вход Azure AD в Communifire, вам потребуется выполнить действия в следующих стандартных блоках.

1. **[Настройка единого входа Azure AD](#configure-azure-ad-single-sign-on)** необходима, чтобы пользователи могли использовать эту функцию.
2. **[Настройка единого входа в Communifire](#configure-communifire-single-sign-on)** необходима, чтобы настроить параметры единого входа на стороне приложения.
3. **[Создание тестового пользователя Azure AD](#create-an-azure-ad-test-user)** требуется для проверки работы единого входа Azure AD от имени пользователя Britta Simon.
4. **[Назначение тестового пользователя Azure AD](#assign-the-azure-ad-test-user)** необходимо, чтобы разрешить пользователю Britta Simon использовать единый вход Azure AD.
5. **[Создание тестового пользователя приложения Communifire](#create-communifire-test-user)** требуется, чтобы в приложении Communifire существовал пользователь Britta Simon, связанный с одноименным пользователем в Azure AD.
6. **[Проверка единого входа](#test-single-sign-on)** необходима, чтобы проверить работу конфигурации.

### <a name="configure-azure-ad-single-sign-on"></a>Настройка единого входа Azure AD

В этом разделе описано включение единого входа Azure AD на портале Azure.

Чтобы настроить единый вход Azure AD в Communifire, выполните следующие действия.

1. На [портале Azure](https://portal.azure.com/) на странице интеграции с приложением **Communifire** выберите **Единый вход**.

    ![Ссылка "Настройка единого входа"](common/select-sso.png)

2. В диалоговом окне **Выбрать метод единого входа** выберите режим **SAML/WS-Fed**, чтобы включить единый вход.

    ![Режим выбора единого входа](common/select-saml-option.png)

3. На странице **Настройка единого входа с помощью SAML** щелкните **Изменить**, чтобы открыть диалоговое окно **Базовая конфигурация SAML**.

    ![Правка базовой конфигурации SAML](common/edit-urls.png)

4. Если вы хотите настроить приложение в режиме, инициируемом **поставщиком удостоверений**, в разделе **Базовая конфигурация SAML** выполните следующие действия.

    ![Сведения о домене и URL-адресах единого входа приложения Communifire](common/idp-intiated.png)

    a. В текстовом поле **Идентификатор** введите URL-адрес в формате `https://<subdomain>.communifire.com`.

    б) В текстовом поле **URL-адрес ответа** введите URL-адрес в формате `https://<subdomain>.communifire.com/SAML/AssertionConsumerService.aspx`.

5. Чтобы настроить приложение для работы в режиме, инициируемом **поставщиком услуг**, щелкните **Задать дополнительные URL-адреса** и выполните следующие действия.

    ![Сведения о домене и URL-адресах единого входа приложения Communifire](common/metadata-upload-additional-signon.png)

    В текстовом поле **URL-адрес входа** введите URL-адрес в формате `https://<subdomain>.communifire.com/login`.

    > [!NOTE]
    > Эти значения приведены для примера. Замените их фактическими значениями идентификатора, URL-адреса ответа и URL-адреса входа. Чтобы получить их, обратитесь в [службу поддержки клиентов Communifire](mailto:support@axerosolutions.com). Можно также посмотреть шаблоны в разделе **Базовая конфигурация SAML** на портале Azure.

6. На странице **Настройка единого входа с помощью SAML** в разделе **Сертификат подписи SAML** щелкните **Скачать**, чтобы скачать нужный вам **XML метаданных федерации**, и сохраните его на компьютере.

    ![Ссылка для скачивания сертификата](common/metadataxml.png)

7. В разделе **Сертификат подписи SAML** щелкните кнопку **Изменить**, чтобы открыть диалоговое окно **Сертификат подписи SAML** и выполнить следующие действия.

    ![Изменение сертификата подписи SAML](common/edit-certificate.png)

    a. Для параметра **Вариант подписывания** выберите значение **Ответ знака SAML и утверждение**.
    
    b. Нажмите кнопку **Сохранить**

    ![Вариант подписывания Communifire](./media/communifire-tutorial/tutorial-communifire-signing-option.png)
 
8. Требуемый URL-адрес можно скопировать из раздела **Настройка Communifire**.

    ![Копирование URL-адресов настройки](common/copy-configuration-urls.png)

    а) URL-адрес входа.

    б) Идентификатор Azure AD.

    в) URL-адрес выхода.

### <a name="configure-communifire-single-sign-on"></a>Настройка единого входа в Communifire

Чтобы настроить единый вход на стороне **Communifire**, нужно отправить скачанный **XML-файл метаданных федерации** и соответствующие URL-адреса, скопированные на портале Azure, [группе поддержки Communifire](mailto:support@axerosolutions.com). Специалисты службы поддержки настроят подключение единого входа SAML на обеих сторонах.

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

В этом разделе описано, как разрешить пользователю Britta Simon использовать единый вход Azure, предоставив этому пользователю доступ к Communifire.

1. На портале Azure выберите **Корпоративные приложения**, **Все приложения**, а затем — **Communifire**.

    ![Колонка "Корпоративные приложения"](common/enterprise-applications.png)

2. В списке приложений выберите **Communifire**.

    ![Ссылка на Communifire в списке "Приложения"](common/all-applications.png)

3. В меню слева выберите **Пользователи и группы**.

    ![Ссылка "Пользователи и группы"](common/users-groups-blade.png)

4. Нажмите кнопку **Добавить пользователя**, а затем в диалоговом окне **Добавление назначения** выберите **Пользователи и группы**.

    ![Область "Добавление назначения"](common/add-assign-user.png)

5. В диалоговом окне **Пользователи и группы** из списка пользователей выберите **Britta Simon**, а затем в верхней части экрана нажмите кнопку **Выбрать**.

6. Если ожидается, что в утверждении SAML будет получено какое-либо значение роли, то в диалоговом окне **Выбор ролей** нужно выбрать соответствующую роль для пользователя из списка и затем нажать кнопку **Выбрать**, расположенную в нижней части экрана.

7. В диалоговом окне **Добавление назначения** нажмите кнопку **Назначить**.

### <a name="create-communifire-test-user"></a>Создание тестового пользователя в приложении Communifire

Цель этого раздела — создать пользователя с именем Britta Simon в Communifire. Приложение Communifire поддерживает **JIT**-подготовку. Эта функция включена по умолчанию. Новый пользователь создается путем сохранения сведений о профиле при первой попытке доступа к Communifire, если такой пользователь еще не существует.

>[!Note]
>Если вы хотите создать пользователя вручную, обратитесь к [группе поддержки Communifire](mailto:support@axerosolutions.com).

### <a name="test-single-sign-on"></a>Проверка единого входа 

В этом разделе описано, как проверить конфигурацию единого входа Azure AD с помощью панели доступа.

Щелкнув плитку Communifire на Панели доступа, вы автоматически войдете в приложение Communifire, для которого настроили единый вход. См. дополнительные сведения о [панели доступа](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)

## <a name="additional-resources"></a>Дополнительные ресурсы

- [Список учебников по интеграции приложений SaaS с Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Что такое условный доступ в Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

