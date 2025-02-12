---
title: Руководство по Интеграция Azure Active Directory с TOPdesk — Public | Документация Майкрософт
description: Узнайте, как настроить единый вход между Azure Active Directory и TOPdesk - Public.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 0873299f-ce70-457b-addc-e57c5801275f
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 05/02/2019
ms.author: jeedes
ms.openlocfilehash: a2a0ffd670a03aeaaa262b83127a385be9efc978
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "67088478"
---
# <a name="tutorial-azure-active-directory-integration-with-topdesk---public"></a>Руководство по Интеграция Azure Active Directory с TOPdesk — Public

В этом руководстве описано, как интегрировать TOPdesk - Public с Azure Active Directory (Azure AD).
Интеграция Azure AD с приложением TOPdesk - Public обеспечивает следующие преимущества.

* C помощью Azure AD вы можете контролировать доступ к TOPdesk - Public.
* Вы можете включить автоматический вход пользователей в TOPdesk — Public (единый вход) с помощью учетных записей Azure AD.
* Вы можете управлять учетными записями централизованно на портале Azure.

Дополнительные сведения об интеграции приложений SaaS с Azure AD см. в статье [Единый вход в приложениях в Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Если у вас еще нет подписки Azure, [создайте бесплатную учетную запись Azure](https://azure.microsoft.com/free/), прежде чем начинать работу.

## <a name="prerequisites"></a>Предварительные требования

Чтобы настроить интеграцию Azure AD с TOPdesk - Public, вам потребуется следующее:

* подписка Azure AD (если у вас нет среды Azure AD, вы можете получить пробную версию на один месяц по [этой ссылке](https://azure.microsoft.com/pricing/free-trial/));
* подписка TOPdesk — Public с поддержкой единого входа.

## <a name="scenario-description"></a>Описание сценария

В рамках этого руководства вы настроите и проверите единый вход Azure AD в тестовой среде.

* TOPdesk — Public поддерживает единый вход, инициированный **поставщиком службы**.

## <a name="adding-topdesk---public-from-the-gallery"></a>Добавление TOPdesk - Public из коллекции

Чтобы настроить интеграцию TOPdesk - Public с Azure AD, необходимо добавить TOPdesk - Public из коллекции в список управляемых приложений SaaS.

**Чтобы добавить TOPdesk - Public из коллекции, выполните следующие действия.**

1. На **[портале Azure](https://portal.azure.com)** в области навигации слева щелкните значок **Azure Active Directory**.

    ![Кнопка Azure Active Directory](common/select-azuread.png)

2. Перейдите в колонку **Корпоративные приложения** и выберите **Все приложения**.

    ![Колонка "Корпоративные приложения"](common/enterprise-applications.png)

3. Чтобы добавить новое приложение, в верхней части диалогового окна нажмите кнопку **Создать приложение**.

    ![Кнопка "Создать приложение"](common/add-new-app.png)

4. В поле поиска введите **TOPdesk - Public**, выберите **TOPdesk - Public** на панели результатов и нажмите кнопку **Добавить**, чтобы добавить это приложение.

     ![TOPdesk - Public в списке результатов](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Настройка и проверка единого входа в Azure AD

В этом разделе описана настройка и проверка единого входа Azure AD в TOPdesk — Public с использованием тестового пользователя **Britta Simon**.
Для обеспечения работы единого входа необходимо установить связь между пользователем Azure AD и соответствующим пользователем в TOPdesk — Public.

Чтобы настроить и проверить единый вход Azure AD в TOPdesk - Public, вам потребуется выполнить действия в следующих стандартных блоках.

1. **[Настройка единого входа Azure AD](#configure-azure-ad-single-sign-on)** необходима, чтобы пользователи могли использовать эту функцию.
2. **[Настройка единого входа в TOPdesk — Public](#configure-topdesk---public-single-sign-on)** необходима, чтобы настроить параметры единого входа на стороне приложения.
3. **[Создание тестового пользователя Azure AD](#create-an-azure-ad-test-user)** требуется для проверки работы единого входа Azure AD от имени пользователя Britta Simon.
4. **[Назначение тестового пользователя Azure AD](#assign-the-azure-ad-test-user)** необходимо, чтобы разрешить пользователю Britta Simon использовать единый вход Azure AD.
5. **[Создание тестового пользователя TOPdesk — Public](#create-topdesk---public-test-user)** требуется для того, чтобы в TOPdesk — Public существовал пользователь Britta Simon, связанный с одноименным пользователем в Azure AD.
6. **[Проверка единого входа](#test-single-sign-on)** необходима, чтобы проверить работу конфигурации.

### <a name="configure-azure-ad-single-sign-on"></a>Настройка единого входа Azure AD

В этом разделе описано включение единого входа Azure AD на портале Azure.

Чтобы настроить единый вход Azure AD в TOPdesk — Public, выполните следующие действия.

1. На [портале Azure](https://portal.azure.com/) на странице интеграции с приложением **TOPdesk — Public** щелкните **Единый вход**.

    ![Ссылка "Настройка единого входа"](common/select-sso.png)

2. В диалоговом окне **Выбрать метод единого входа** выберите режим **SAML/WS-Fed**, чтобы включить единый вход.

    ![Режим выбора единого входа](common/select-saml-option.png)

3. На странице **Настройка единого входа с помощью SAML** щелкните **Изменить**, чтобы открыть диалоговое окно **Базовая конфигурация SAML**.

    ![Правка базовой конфигурации SAML](common/edit-urls.png)

4.  Если у вас есть **файл метаданных поставщика служб**, выполните следующие действия в разделе **Базовая конфигурация SAML**:

    >[!NOTE]
    >Вы получите **файл метаданных поставщика службы**, описанный далее в разделе **Настройка единого входа в TOPdesk — Public**.

    a. Щелкните **Отправить файл метаданных**.
    
    ![Передача файла метаданных](common/upload-metadata.png)

    b. Щелкните **значок папки**, выберите файл метаданных и нажмите кнопку **Отправить**.

    ![Выбор файла метаданных](common/browse-upload-metadata.png)

    c. После успешной передачи файла метаданных значения **Идентификатор** и **URL-адрес ответа** в разделе "Базовая конфигурация SAML" заполнятся автоматически.

    ![Сведения о домене и URL-адресах единого входа для приложения TOPdesk - Public](common/sp-identifier-reply.png)

    d. В текстовое поле **URL-адрес для входа** введите URL-адрес в следующем формате: `https://<companyname>.topdesk.net`.

    д. В текстовом поле **Идентификатор** введите URL-адрес в следующем формате: `https://<companyname>.topdesk.net/tas/public/login/verify`

    > [!NOTE] 
    > Если поля **Идентификатор** и **URL-адрес ответа** автоматически не заполняются, введите эти значения вручную. Следуйте описанному выше шаблону и вы получите нужные значения, как описано далее в разделе **Настройка единого входа в TOPdesk — Public**. Значение **URL-адреса входа** не является реальным, поэтому введите фактический URL-адрес входа. Чтобы получить эти данные, обратитесь в [службу поддержки клиентов TOPdesk — Public](https://help.topdesk.com/saas/enterprise/user/). Можно также посмотреть шаблоны в разделе **Базовая конфигурация SAML** на портале Azure.

5. На странице **Настройка единого входа с помощью SAML** в разделе **Сертификат подписи SAML** щелкните **Скачать**, чтобы скачать нужный вам **XML метаданных федерации**, и сохраните его на компьютере.

    ![Ссылка для скачивания сертификата](common/metadataxml.png)

6. Скопируйте нужный URL-адрес из раздела **Настройка TOPdesk — Public**.

    ![Копирование URL-адресов настройки](common/copy-configuration-urls.png)

    а) URL-адрес входа.

    b. Идентификатор Azure AD

    c. URL-адрес выхода.

### <a name="configure-topdesk---public-single-sign-on"></a>Настройка единого входа в TOPdesk — Public

1. Выполните вход на веб-сайт **TOPdesk — Public** компании в качестве администратора.

2. В меню **TOPdesk** выберите пункт **Settings** (Параметры).
   
    ![Параметры](./media/topdesk-public-tutorial/ic790598.png "Параметры")

3. Щелкните **Параметры входа**.
   
    ![Параметры входа](./media/topdesk-public-tutorial/ic790599.png "Параметры входа")

4. Разверните меню **Login Settings** (Параметры входа), а затем выберите **General** (Общие).
   
    ![Общие](./media/topdesk-public-tutorial/ic790600.png "Общие")

5. В разделе **Public** (Общедоступные) для конфигурации **SAML login** (Вход SAML) выполните следующие действия.
   
    ![Технические параметры](./media/topdesk-public-tutorial/ic790601.png "Технические параметры")
   
    a. Нажмите кнопку **Скачать** , чтобы скачать общедоступный файл метаданных, а затем сохраните его локально на компьютере.
   
    b. Откройте скачанный файл метаданных и определите местоположение узла **AssertionConsumerService**.

    ![AssertionConsumerService](./media/topdesk-public-tutorial/ic790619.png "AssertionConsumerService")
   
    c. Скопируйте значение **AssertionConsumerService** и вставьте это значение в текстовое поле **URL-адрес ответа** в разделе **Базовая конфигурация SAML**.      
   
6. Чтобы создать файл сертификата, выполните следующие действия:
    
    ![Сертификат](./media/topdesk-public-tutorial/ic790606.png "Сертификат")
    
    a. Откройте скачанный файл метаданных на портале Azure.
    
    b. Разверните узел **RoleDescriptor**, содержащий **xsi:type** со значением **fed:ApplicationServiceType**.
    
    c. Скопируйте значение узла **X509Certificate** .
    
    d. Сохраните скопированное значение узла **X509Certificate** локально в файл на компьютере.

7. В разделе **Public** (Общедоступные) щелкните **Add** (Добавить).
    
    ![Вход SAML](./media/topdesk-public-tutorial/ic790625.png "Вход SAML")

8. На странице диалогового окна **Помощник настройки SAML** сделайте следующее:
    
    ![Помощник по настройке SAML](./media/topdesk-public-tutorial/ic790608.png "Помощник по настройке SAML")
    
    a. Чтобы отправить скачанный файл метаданных через портал Azure, напротив пункта **Federation Metadata** (Метаданные федерации) нажмите кнопку **Browse** (Обзор).

    b. Чтобы отправить файл сертификата, напротив пункта **Certificate (RSA)** (Сертификат (RSA)) нажмите кнопку **Browse** (Обзор).

    c. Чтобы отправить файл с логотипом, полученный от службы поддержки TOPdesk, напротив пункта **Logo icon** (Значок логотипа) нажмите кнопку **Browse** (Обзор).

    d. В текстовое поле **Атрибут имени пользователя** введите значение `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`.

    д. В текстовом поле **Отображаемое имя** введите имя конфигурации.

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

В этом разделе описано, как разрешить пользователю Britta Simon использовать единый вход Azure, предоставив этому пользователю доступ к TOPdesk - Public.

1. На портале Azure выберите **Корпоративные приложения**, **Все приложения**, а затем — **TOPdesk — Public**.

    ![Колонка "Корпоративные приложения"](common/enterprise-applications.png)

2. В списке приложений выберите **TOPdesk - Public**.

    ![Ссылка на TOPdesk - Public в списке "Приложения"](common/all-applications.png)

3. В меню слева выберите **Пользователи и группы**.

    ![Ссылка "Пользователи и группы"](common/users-groups-blade.png)

4. Нажмите кнопку **Добавить пользователя**, а затем в диалоговом окне **Добавление назначения** выберите **Пользователи и группы**.

    ![Область "Добавление назначения"](common/add-assign-user.png)

5. В диалоговом окне **Пользователи и группы** из списка пользователей выберите **Britta Simon**, а затем в верхней части экрана нажмите кнопку **Выбрать**.

6. Если ожидается, что в утверждении SAML будет получено какое-либо значение роли, то в диалоговом окне **Выбор ролей** нужно выбрать соответствующую роль для пользователя из списка и затем нажать кнопку **Выбрать**, расположенную в нижней части экрана.

7. В диалоговом окне **Добавление назначения** нажмите кнопку **Назначить**.

### <a name="create-topdesk---public-test-user"></a>Создание тестового пользователя TOPdesk — Public

Чтобы пользователи Azure AD могли входить в TOPdesk — Public, они должны быть подготовлены в приложении. В случае TOPdesk — Public подготовка пользователей осуществляется вручную.

### <a name="to-configure-user-provisioning-perform-the-following-steps"></a>Чтобы настроить подготовку учетных записей пользователей, выполните следующие действия.

1. Выполните вход на веб-сайт **TOPdesk — Public** своей компании в качестве администратора.

2. В меню в верхней части страницы щелкните **TOPdesk \> New \> Support Files \> Person** ("TOPdesk" > "Создать" > "Файлы поддержки" > "Пользователь").
   
    ![Пользователь](./media/topdesk-public-tutorial/ic790628.png "Пользователь")

3. В диалоговом окне "Новый пользователь" выполните следующие действия:
   
    ![Новый пользователь](./media/topdesk-public-tutorial/ic790629.png "Новый пользователь")
   
    a. Перейдите на вкладку "Общее".

    b. В текстовое поле **Last Name** (Фамилия) введите фамилию пользователя, например Simon.
 
    c. Выберите **Веб-сайт** для этой учетной записи.
 
    d. Выберите команду **Сохранить**.

> [!NOTE]
> Для подготовки учетных записей Azure AD вы можете использовать любые другие инструменты или API-интерфейсы создания пользователей, предоставляемые TOPdesk — Public.

### <a name="test-single-sign-on"></a>Проверка единого входа 

В этом разделе описано, как проверить конфигурацию единого входа Azure AD с помощью панели доступа.

Щелкнув плитку TOPdesk — Public на панели доступа, вы автоматически войдете в приложение TOPdesk — Public, для которого настроили единый вход. См. дополнительные сведения о [панели доступа](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)

## <a name="additional-resources"></a>Дополнительные ресурсы

- [Список учебников по интеграции приложений SaaS с Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Что такое условный доступ в Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
