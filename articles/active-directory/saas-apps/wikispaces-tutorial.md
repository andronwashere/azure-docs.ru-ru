---
title: Руководство по Интеграция Azure Active Directory с Wikispaces | Документация Майкрософт
description: Узнайте, как настроить единый вход Azure Active Directory в Wikispaces.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 665b95aa-f7f5-4406-9e2a-6fc299a1599c
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/28/2019
ms.author: jeedes
ms.openlocfilehash: d2c03143eca9f335c440dfcaee766e205b5c0730
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "67087151"
---
# <a name="tutorial-azure-active-directory-integration-with-wikispaces"></a>Руководство по Интеграция Azure Active Directory с Wikispaces

В этом руководстве описано, как интегрировать приложение Wikispaces с Azure Active Directory (Azure AD).
Интеграция Azure AD с приложением Wikispaces обеспечивает следующие преимущества:

* С помощью Azure Active Directory вы можете контролировать доступ к Wikispaces.
* Вы можете включить автоматический вход пользователей в Wikispaces (единый вход) с помощью учетных записей Azure Active Directory.
* Вы можете управлять учетными записями централизованно на портале Azure.

Дополнительные сведения об интеграции приложений SaaS с Azure AD см. в статье [Единый вход в приложениях в Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Если у вас еще нет подписки Azure, [создайте бесплатную учетную запись Azure](https://azure.microsoft.com/free/), прежде чем начинать работу.

## <a name="prerequisites"></a>Предварительные требования

Чтобы настроить интеграцию Azure AD с Wikispaces, вам потребуется:

* подписка Azure AD (если у вас нет среды Azure AD, вы можете получить [бесплатную учетную запись](https://azure.microsoft.com/free/));
* подписка Wikispaces с поддержкой единого входа.

## <a name="scenario-description"></a>Описание сценария

В рамках этого руководства вы настроите и проверите единый вход Azure AD в тестовой среде.

* Приложение Wikispaces поддерживает единый вход, инициированный **поставщиком услуг**.

## <a name="adding-wikispaces-from-the-gallery"></a>Добавление Wikispaces из коллекции

Чтобы настроить интеграцию Wikispaces с Azure AD, необходимо добавить Wikispaces из коллекции в список управляемых приложений SaaS.

**Чтобы добавить Wikispaces из коллекции, сделайте следующее.**

1. На **[портале Azure](https://portal.azure.com)** в области навигации слева щелкните значок **Azure Active Directory**.

    ![Кнопка Azure Active Directory](common/select-azuread.png)

2. Перейдите в колонку **Корпоративные приложения** и выберите **Все приложения**.

    ![Колонка "Корпоративные приложения"](common/enterprise-applications.png)

3. Чтобы добавить новое приложение, в верхней части диалогового окна нажмите кнопку **Создать приложение**.

    ![Кнопка "Создать приложение"](common/add-new-app.png)

4. В поле поиска введите **Wikispaces**, выберите **Wikispaces** на панели результатов и нажмите кнопку **Добавить**, чтобы добавить это приложение.

     ![Wikispaces в списке результатов](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Настройка и проверка единого входа в Azure AD

В этом разделе описана настройка и проверка единого входа Azure Active Directory в Wikispaces с использованием тестового пользователя **Britta Simon**.
Для обеспечения работы единого входа необходимо установить связь между пользователем Azure Active Directory и соответствующим пользователем в Wikispaces.

Чтобы настроить и проверить единый вход Azure AD в Wikispaces, вам потребуется выполнить действия в следующих стандартных блоках.

1. **[Настройка единого входа Azure AD](#configure-azure-ad-single-sign-on)** необходима, чтобы пользователи могли использовать эту функцию.
2. **[Настройка единого входа в Wikispaces](#configure-wikispaces-single-sign-on)** необходима, чтобы настроить параметры единого входа на стороне приложения.
3. **[Создание тестового пользователя Azure AD](#create-an-azure-ad-test-user)** требуется для проверки работы единого входа Azure AD от имени пользователя Britta Simon.
4. **[Назначение тестового пользователя Azure AD](#assign-the-azure-ad-test-user)** необходимо, чтобы разрешить пользователю Britta Simon использовать единый вход Azure AD.
5. **[Создание тестового пользователя Wikispaces](#create-wikispaces-test-user)** требуется для того, чтобы в приложении Wikispaces существовал пользователь Britta Simon, связанный с одноименным пользователем в Azure Active Directory.
6. **[Проверка единого входа](#test-single-sign-on)** необходима, чтобы проверить работу конфигурации.

### <a name="configure-azure-ad-single-sign-on"></a>Настройка единого входа Azure AD

В этом разделе описано включение единого входа Azure AD на портале Azure.

Чтобы настроить единый вход Azure Active Directory в Wikispaces, сделайте следующее.

1. На [портале Azure](https://portal.azure.com/) на странице интеграции с приложением **Wikispaces** выберите **Единый вход**.

    ![Ссылка "Настройка единого входа"](common/select-sso.png)

2. В диалоговом окне **Выбрать метод единого входа** выберите режим **SAML/WS-Fed**, чтобы включить единый вход.

    ![Режим выбора единого входа](common/select-saml-option.png)

3. На странице **Настройка единого входа с помощью SAML** щелкните **Изменить**, чтобы открыть диалоговое окно **Базовая конфигурация SAML**.

    ![Правка базовой конфигурации SAML](common/edit-urls.png)

4. В разделе **Базовая конфигурация SAML** выполните приведенные ниже действия.

    ![Сведения о домене и URL-адресах единого входа для приложения Wikispaces](common/sp-identifier.png)

    a. В текстовом поле **URL-адрес для входа** введите URL-адрес в следующем формате: `https://<companyname>.wikispaces.net`.

    b. В текстовом поле **Идентификатор (сущности)** введите URL-адрес в следующем формате: `https://session.wikispaces.net/<instancename>`.

    > [!NOTE]
    > Эти значения приведены для примера. Необходимо обновить эти значения действующим URL-адресом для входа и идентификатором. Чтобы получить эти значения, обратитесь к [группе поддержки клиентов Wikispaces](https://www.wikispaces.com/site/help). Можно также посмотреть шаблоны в разделе **Базовая конфигурация SAML** на портале Azure.

5. На странице **Настройка единого входа с помощью SAML** в разделе **Сертификат подписи SAML** щелкните **Скачать**, чтобы скачать нужный вам **XML метаданных федерации**, и сохраните его на компьютере.

    ![Ссылка для скачивания сертификата](common/metadataxml.png)

6. Скопируйте требуемый URL-адрес из раздела **Настройка Wikispaces**.

    ![Копирование URL-адресов настройки](common/copy-configuration-urls.png)

    а) URL-адрес входа.

    b. Идентификатор Azure AD

    c. URL-адрес выхода.

### <a name="configure-wikispaces-single-sign-on"></a>Настройка единого входа в Wikispaces

Чтобы настроить единый вход на стороне **Wikispaces**, нужно отправить скачанный файл **XML метаданных федерации** и соответствующие URL-адреса, скопированные на портале Azure, в [группу поддержки Wikispaces](https://www.wikispaces.com/site/help). Специалисты службы поддержки настроят подключение единого входа SAML на обеих сторонах.

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

В этом разделе описано, как разрешить пользователю Britta Simon использовать единый вход Azure, предоставив этому пользователю доступ к Wikispaces.

1. На портале Azure выберите **Корпоративные приложения**, **Все приложения**, а затем — **Wikispaces**.

    ![Колонка "Корпоративные приложения"](common/enterprise-applications.png)

2. Из списка приложений выберите **Wikispaces**.

    ![Ссылка на Wikispaces в списке приложений](common/all-applications.png)

3. В меню слева выберите **Пользователи и группы**.

    ![Ссылка "Пользователи и группы"](common/users-groups-blade.png)

4. Нажмите кнопку **Добавить пользователя**, а затем в диалоговом окне **Добавление назначения** выберите **Пользователи и группы**.

    ![Область "Добавление назначения"](common/add-assign-user.png)

5. В диалоговом окне **Пользователи и группы** из списка пользователей выберите **Britta Simon**, а затем в верхней части экрана нажмите кнопку **Выбрать**.

6. Если ожидается, что в утверждении SAML будет получено какое-либо значение роли, то в диалоговом окне **Выбор ролей** нужно выбрать соответствующую роль для пользователя из списка и затем нажать кнопку **Выбрать**, расположенную в нижней части экрана.

7. В диалоговом окне **Добавление назначения** нажмите кнопку **Назначить**.

### <a name="create-wikispaces-test-user"></a>Создание тестового пользователя Wikispaces

Чтобы пользователи Azure Active Directory могли выполнять вход в Wikispaces, они должны быть подготовлены в Wikispaces. В случае использования Wikispaces подготовка выполняется вручную.

### <a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Чтобы подготовить учетные записи пользователей, выполните следующие действия.

1. Выполните вход на сайт компании **Wikispaces** в качестве администратора.

2. Перейдите в раздел **Участники**.
   
    ![Участники](./media/wikispaces-tutorial/ic787193.png "Участники")

3. Щелкните **Пригласить пользователей**.
   
    ![Приглашение участников](./media/wikispaces-tutorial/ic787194.png "приглашение участников")

4. В разделе **Пригласить пользователей** выполните следующие действия.
   
    ![Приглашение участников](./media/wikispaces-tutorial/ic787208.png "приглашение участников")
   
    a. Введите **имя пользователя или электронный адрес** для действующей учетной записи AAD, которую вы хотите подготовить, в соответствующие текстовые поля.
   
    b. Нажмите кнопку **Отправить**.  
      
    > [!NOTE]
    > Владелец учетной записи Azure Active Directory получит электронное сообщение со ссылкой для подтверждения учетной записи перед ее активацией.
    
> [!NOTE]
> Вы можете использовать любые другие средства создания учетной записи пользователя Wikispaces или API-интерфейсы, предоставляемые Wikispaces, для подготовки учетных записей пользователей AAD.

### <a name="test-single-sign-on"></a>Проверка единого входа 

В этом разделе описано, как проверить конфигурацию единого входа Azure AD с помощью панели доступа.

Щелкнув плитку Wikispaces на Панели доступа, вы автоматически войдете в приложение Wikispaces, для которого настроили единый вход. См. дополнительные сведения о [панели доступа](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)

## <a name="additional-resources"></a>Дополнительные ресурсы

- [Список учебников по интеграции приложений SaaS с Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Что такое условный доступ в Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

