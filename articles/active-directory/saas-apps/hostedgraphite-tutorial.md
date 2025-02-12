---
title: Руководство по Интеграция Azure Active Directory с Hosted Graphite | Документация Майкрософт
description: Узнайте, как настроить единый вход между Azure Active Directory и Hosted Graphite.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: a1ac4d7f-d079-4f3c-b6da-0f520d427ceb
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 02/15/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 79a83ec34d8e9d577e6bde8421a11b4c7bf6b241
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "67100907"
---
# <a name="tutorial-azure-active-directory-integration-with-hosted-graphite"></a>Руководство по Интеграция Azure Active Directory с Hosted Graphite

В этом руководстве описано, как интегрировать Hosted Graphite с Azure Active Directory (Azure AD).
Интеграция Azure AD с приложением Hosted Graphite обеспечивает следующие преимущества.

* С помощью Azure AD вы можете контролировать доступ к Hosted Graphite.
* Вы можете включить автоматический вход пользователей в Hosted Graphite (единый вход) с помощью учетных записей Azure AD.
* Вы можете управлять учетными записями централизованно на портале Azure.

Дополнительные сведения об интеграции приложений SaaS с Azure AD см. в статье [Единый вход в приложениях в Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Если у вас еще нет подписки Azure, [создайте бесплатную учетную запись Azure](https://azure.microsoft.com/free/), прежде чем начинать работу.

## <a name="prerequisites"></a>Предварительные требования

Чтобы настроить интеграцию Azure AD с Hosted Graphite, вам потребуется:

* подписка Azure AD (если у вас нет среды Azure AD, вы можете получить пробную версию на один месяц по [этой ссылке](https://azure.microsoft.com/pricing/free-trial/));
* подписка Hosted Graphite с поддержкой единого входа.

## <a name="scenario-description"></a>Описание сценария

В рамках этого руководства вы настроите и проверите единый вход Azure AD в тестовой среде.

* Hosted Graphite поддерживает инициированный единый вход **пакета обновлений и выдающей точки распространения**.
* Hosted Graphite поддерживает **JIT**-подготовку пользователей.

## <a name="adding-hosted-graphite-from-the-gallery"></a>Добавление Hosted Graphite из коллекции

Чтобы настроить интеграцию Hosted Graphite с Azure AD, необходимо добавить Hosted Graphite из коллекции в список управляемых приложений SaaS.

**Чтобы добавить Hosted Graphite из коллекции, выполните следующие действия.**

1. На **[портале Azure](https://portal.azure.com)** в области навигации слева щелкните значок **Azure Active Directory**.

    ![Кнопка Azure Active Directory](common/select-azuread.png)

2. Перейдите в колонку **Корпоративные приложения** и выберите **Все приложения**.

    ![Колонка "Корпоративные приложения"](common/enterprise-applications.png)

3. Чтобы добавить новое приложение, в верхней части диалогового окна нажмите кнопку **Создать приложение**.

    ![Кнопка "Создать приложение"](common/add-new-app.png)

4. В поле поиска введите **Hosted Graphite**, выберите **Hosted Graphite** на панели результатов и нажмите кнопку **Добавить**, чтобы добавить это приложение.

     ![Hosted Graphite в списке результатов](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Настройка и проверка единого входа в Azure AD

В этом разделе описана настройка и проверка единого входа Azure AD в приложение Hosted Graphite для тестового пользователя **Britta Simon**.
Для обеспечения работы единого входа необходимо установить связь между пользователем Azure AD и соответствующим пользователем в Hosted Graphite.

Чтобы настроить и проверить единый вход Azure AD в Hosted Graphite, вам потребуется выполнить действия в указанных далее стандартных блоках.

1. **[Настройка единого входа Azure AD](#configure-azure-ad-single-sign-on)** необходима, чтобы пользователи могли использовать эту функцию.
2. **[Настройка единого входа в Hosted Graphite](#configure-hosted-graphite-single-sign-on)** необходима, чтобы настроить параметры единого входа на стороне приложения.
3. **[Создание тестового пользователя Azure AD](#create-an-azure-ad-test-user)** требуется для проверки работы единого входа Azure AD от имени пользователя Britta Simon.
4. **[Назначение тестового пользователя Azure AD](#assign-the-azure-ad-test-user)** необходимо, чтобы разрешить пользователю Britta Simon использовать единый вход Azure AD.
5. **[Создание тестового пользователя Hosted Graphite](#create-hosted-graphite-test-user)** требуется для того, чтобы в Hosted Graphite существовал пользователь Britta Simon, связанный с представлением этого же пользователя в Azure AD.
6. **[Проверка единого входа](#test-single-sign-on)** необходима, чтобы проверить работу конфигурации.

### <a name="configure-azure-ad-single-sign-on"></a>Настройка единого входа Azure AD

В этом разделе описано включение единого входа Azure AD на портале Azure.

Чтобы настроить единый вход Azure AD в Hosted Graphite, выполните следующие действия:

1. На [портале Azure](https://portal.azure.com/) на странице интеграции с приложением **Hosted Graphite** выберите **Единый вход**.

    ![Ссылка "Настройка единого входа"](common/select-sso.png)

2. В диалоговом окне **Выбрать метод единого входа** выберите режим **SAML/WS-Fed**, чтобы включить единый вход.

    ![Режим выбора единого входа](common/select-saml-option.png)

3. На странице **Настройка единого входа с помощью SAML** щелкните **Изменить**, чтобы открыть диалоговое окно **Базовая конфигурация SAML**.

    ![Правка базовой конфигурации SAML](common/edit-urls.png)

4. Если вы хотите настроить приложение в режиме, инициируемом **поставщиком удостоверений**, в разделе **Базовая конфигурация SAML** выполните следующие действия.

    ![Сведения о домене и URL-адресах единого входа приложения Hosted Graphite](common/idp-intiated.png)

    a. В текстовом поле **Идентификатор** введите URL-адрес в формате `https://www.hostedgraphite.com/metadata/<user id>`.

    б) В текстовом поле **URL-адрес ответа** введите URL-адрес в формате `https://www.hostedgraphite.com/complete/saml/<user id>`.

5. Чтобы настроить приложение для работы в режиме, инициируемом **поставщиком услуг**, щелкните **Задать дополнительные URL-адреса** и выполните следующие действия.

    ![Сведения о домене и URL-адресах единого входа приложения Hosted Graphite](common/metadata-upload-additional-signon.png)

    В текстовом поле **URL-адрес входа** введите URL-адрес в формате `https://www.hostedgraphite.com/login/saml/<user id>/`.

    > [!NOTE]
    > Обратите внимание, что значения, указанные выше, используются в качестве примера. Необходимо указать фактические значения идентификатора, URL-адреса ответа и URL-адреса входа. Чтобы получить эти значения, на стороне приложения перейдите в раздел "Доступ" -> "Настройка SAML" или обратитесь в [службу поддержки Hosted Graphite](mailto:help@hostedgraphite.com).

6. На странице **Настройка единого входа с помощью SAML** в разделе **Сертификат подписи SAML** щелкните **Загрузить**, чтобы загрузить требуемый **сертификат (Base64)** из предложенных вариантов, и сохраните его на компьютере.

    ![Ссылка для скачивания сертификата](common/certificatebase64.png)

7. Скопируйте соответствующий вашим требованиям URL-адрес из раздела **Настройка Hosted Graphite**.

    ![Копирование URL-адресов настройки](common/copy-configuration-urls.png)

    а) URL-адрес входа.

    б) Идентификатор Azure AD.

    в) URL-адрес выхода.

### <a name="configure-hosted-graphite-single-sign-on"></a>Настройка единого входа Hosted Graphite

1. Войдите в клиент Hosted Graphite с правами администратора.

2. На боковой панели выберите **SAML Setup page** (Страница настройки SAML), щелкнув **Access -> SAML Setup** (Доступ -> Настройка SAML).

    ![Настройка единого входа на стороне приложения](./media/hostedgraphite-tutorial/tutorial_hostedgraphite_000.png)

3. Проверьте, что эти URL-адреса соответствуют конфигурации в разделе **Базовая конфигурация SAML** на портале Azure.

    ![Настройка единого входа на стороне приложения](./media/hostedgraphite-tutorial/tutorial_hostedgraphite_001.png)

4. В текстовые поля **Entity or Issuer ID** (Идентификатор сущности или издателя) и **SSO Login URL** (URL-адрес единого входа) вставьте значения **идентификатора Azure AD** и **URL-адреса входа**, скопированные на портале Azure.

    ![Настройка единого входа на стороне приложения](./media/hostedgraphite-tutorial/tutorial_hostedgraphite_002.png)

5. Для параметра **Default User Role** (Роль пользователя по умолчанию) выберите значение **Read-only** (Только для чтения).

    ![Настройка единого входа на стороне приложения](./media/hostedgraphite-tutorial/tutorial_hostedgraphite_004.png)

6. Откройте в блокноте сертификат в кодировке Base-64, скачанный с портала Azure, скопируйте его в буфер обмена и вставьте в текстовое поле **Сертификат X.509**.

    ![Настройка единого входа на стороне приложения](./media/hostedgraphite-tutorial/tutorial_hostedgraphite_005.png)

7. Нажмите кнопку **Сохранить** .

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

В этом разделе описано, как включить единый вход Azure для пользователя Britta Simon, предоставив этому пользователю доступ к Hosted Graphite.

1. На портале Azure выберите **Корпоративные приложения**, **Все приложения**, а затем — **Hosted Graphite**.

    ![Колонка "Корпоративные приложения"](common/enterprise-applications.png)

2. В списке приложений выберите **Hosted Graphite**.

    ![Ссылка на Hosted Graphite в списке "Приложения"](common/all-applications.png)

3. В меню слева выберите **Пользователи и группы**.

    ![Ссылка "Пользователи и группы"](common/users-groups-blade.png)

4. Нажмите кнопку **Добавить пользователя**, а затем в диалоговом окне **Добавление назначения** выберите **Пользователи и группы**.

    ![Область "Добавление назначения"](common/add-assign-user.png)

5. В диалоговом окне **Пользователи и группы** из списка пользователей выберите **Britta Simon**, а затем в верхней части экрана нажмите кнопку **Выбрать**.

6. Если ожидается, что в утверждении SAML будет получено какое-либо значение роли, то в диалоговом окне **Выбор ролей** нужно выбрать соответствующую роль для пользователя из списка и затем нажать кнопку **Выбрать**, расположенную в нижней части экрана.

7. В диалоговом окне **Добавление назначения** нажмите кнопку **Назначить**.

### <a name="create-hosted-graphite-test-user"></a>Создание тестового пользователя Hosted Graphite

В этом разделе вы создадите в Hosted Graphite пользователя Britta Simon. Приложение Hosted Graphite поддерживает JIT-подготовку пользователей, которая включена по умолчанию. В этом разделе никакие действия с вашей стороны не требуются. Если пользователь еще не существует в Hosted Graphite, он создается после проверки подлинности.

> [!NOTE]
> Чтобы создать пользователя вручную, обратитесь в службу поддержки Hosted Graphite, отправив сообщение по адресу <mailto:help@hostedgraphite.com>.

### <a name="test-single-sign-on"></a>Проверка единого входа

В этом разделе описано, как проверить конфигурацию единого входа Azure AD с помощью панели доступа.

Щелкнув плитку "Hosted Graphite" на панели доступа, вы автоматически войдете в приложение Hosted Graphite, для которого настроили единый вход. См. дополнительные сведения о [панели доступа](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)

## <a name="additional-resources"></a>Дополнительные ресурсы

- [Список учебников по интеграции приложений SaaS с Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Что такое условный доступ в Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

