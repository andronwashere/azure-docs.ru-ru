---
title: Руководство по Интеграция Azure Active Directory с Fidelity NetBenefits | Документация Майкрософт
description: Узнайте, как настроить единый вход Azure Active Directory в приложении Fidelity NetBenefits.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 77dc8a98-c0e7-4129-ab88-28e7643e432a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 02/12/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 2ce37aea9e700907ebfda9aa181b7f0eb638af35
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "67102732"
---
# <a name="tutorial-azure-active-directory-integration-with-fidelity-netbenefits"></a>Руководство по Интеграция Azure Active Directory с Fidelity NetBenefits

В этом руководстве описано, как интегрировать Fidelity NetBenefits с Azure Active Directory (Azure AD).
Интеграция Azure AD с приложением Fidelity NetBenefits обеспечивает следующие преимущества.

* С помощью Azure AD вы можете контролировать доступ к Fidelity NetBenefits.
* Вы можете включить автоматический вход пользователей в Fidelity NetBenefits (единый вход) с применением учетных записей Azure AD.
* Вы можете управлять учетными записями централизованно на портале Azure.

Дополнительные сведения об интеграции приложений SaaS с Azure AD см. в статье [Единый вход в приложениях в Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Если у вас еще нет подписки Azure, [создайте бесплатную учетную запись Azure](https://azure.microsoft.com/free/), прежде чем начинать работу.

## <a name="prerequisites"></a>Предварительные требования

Чтобы настроить интеграцию Azure AD с Fidelity NetBenefits, вам потребуется:

* подписка Azure AD (если у вас нет среды Azure AD, вы можете получить пробную версию на один месяц по [этой ссылке](https://azure.microsoft.com/pricing/free-trial/));
* подписка Fidelity NetBenefits с поддержкой единого входа.

## <a name="scenario-description"></a>Описание сценария

В рамках этого руководства вы настроите и проверите единый вход Azure AD в тестовой среде.

* Fidelity NetBenefits поддерживает единый вход, инициированный **поставщиком удостоверений**

* Fidelity NetBenefits поддерживает **JIT**-подготовку пользователей

## <a name="adding-fidelity-netbenefits-from-the-gallery"></a>Добавление Fidelity NetBenefits из коллекции

Чтобы настроить интеграцию Fidelity NetBenefits с Azure AD, необходимо добавить Fidelity NetBenefits из коллекции в список управляемых приложений SaaS.

**Чтобы добавить Fidelity NetBenefits из коллекции, выполните следующие действия.**

1. На **[портале Azure](https://portal.azure.com)** в области навигации слева щелкните значок **Azure Active Directory**.

    ![Кнопка Azure Active Directory](common/select-azuread.png)

2. Перейдите в колонку **Корпоративные приложения** и выберите **Все приложения**.

    ![Колонка "Корпоративные приложения"](common/enterprise-applications.png)

3. Чтобы добавить новое приложение, в верхней части диалогового окна нажмите кнопку **Создать приложение**.

    ![Кнопка "Создать приложение"](common/add-new-app.png)

4. В поле поиска введите **Fidelity NetBenefits**, выберите **Fidelity NetBenefits** на панели результатов и нажмите кнопку **Добавить**, чтобы добавить это приложение.

     ![Fidelity NetBenefits в списке результатов](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Настройка и проверка единого входа в Azure AD

В этом разделе описана настройка и проверка единого входа Azure AD в Fidelity NetBenefits с использованием тестового пользователя **Britta Simon**.
Для обеспечения работы единого входа необходимо установить связь между пользователем Azure AD и соответствующим пользователем в Fidelity NetBenefits.

Чтобы настроить и проверить единый вход Azure AD в Fidelity NetBenefits, вам потребуется выполнить действия в следующих стандартных блоках.

1. **[Настройка единого входа Azure AD](#configure-azure-ad-single-sign-on)** необходима, чтобы пользователи могли использовать эту функцию.
2. **[Настройка единого входа в Fidelity NetBenefits](#configure-fidelity-netbenefits-single-sign-on)** необходима, чтобы настроить параметры единого входа на стороне приложения.
3. **[Создание тестового пользователя Azure AD](#create-an-azure-ad-test-user)** требуется для проверки работы единого входа Azure AD от имени пользователя Britta Simon.
4. **[Назначение тестового пользователя Azure AD](#assign-the-azure-ad-test-user)** необходимо, чтобы разрешить пользователю Britta Simon использовать единый вход Azure AD.
5. **[Создание тестового пользователя Fidelity NetBenefits](#create-fidelity-netbenefits-test-user)** нужно для того, чтобы в Fidelity NetBenefits также существовал пользователь Britta Simon, связанный с одноименным пользователем в Azure AD.
6. **[Проверка единого входа](#test-single-sign-on)** необходима, чтобы проверить работу конфигурации.

### <a name="configure-azure-ad-single-sign-on"></a>Настройка единого входа Azure AD

В этом разделе описано включение единого входа Azure AD на портале Azure.

Чтобы настроить единый вход Azure AD в Fidelity NetBenefits, выполните следующие действия:

1. На [портале Azure ](https://portal.azure.com/)на странице**интеграции с приложением**Fidelity NetBenefits выберите **Единый вход**.

    ![Ссылка "Настройка единого входа"](common/select-sso.png)

2. В диалоговом окне **Выбрать метод единого входа** выберите режим **SAML/WS-Fed**, чтобы включить единый вход.

    ![Режим выбора единого входа](common/select-saml-option.png)

3. На странице **Настройка единого входа с помощью SAML** щелкните **Изменить**, чтобы открыть диалоговое окно **Базовая конфигурация SAML**.

    ![Правка базовой конфигурации SAML](common/edit-urls.png)

4. На странице **Настройка единого входа с помощью SAML** выполните следующие действия.

    ![Сведения о домене и URL-адресах единого входа для приложения Fidelity NetBenefits](common/idp-intiated.png)

    a. В текстовом поле **Идентификатор** введите URL-адрес в таком формате:

    для тестовой среды: `urn:sp:fidelity:geninbndnbparts20:uat:xq1`

    для рабочей среды: `urn:sp:fidelity:geninbndnbparts20`

    b. В текстовом поле **URL-адрес ответа** введите URL-адрес, предоставленный Fidelity во время реализации, или обратитесь к менеджеру по обслуживанию клиентов Fidelity.

5. Приложение NetBenefits ожидает проверочные утверждения SAML в определенном формате, который требует добавить настраиваемые сопоставления атрибутов в вашу конфигурацию атрибутов токена SAML. На следующем снимке экрана показан список атрибутов по умолчанию, когда **nameidentifier** сопоставляется с **user.userprincipalname**. Приложение Fidelity NetBenefits ожидает сопоставления **nameidentifier**с **employeeid** или любым другим утверждением, применимым к организации в качестве **nameidentifier**, поэтому необходимо изменить сопоставление атрибутов, щелкнув значок **Изменить** и изменив сопоставление атрибутов.

    ![image](common/edit-attribute.png)

    >[!Note]
    >Fidelity NetBenefits поддерживает статическую и динамическую федерацию. Статическая федерация означает, что не будет использоваться JIT-подготовка пользователей на основе SAML, а динамическая федерация означает, что такая JIT-подготовка пользователей поддерживается. Для использования JIT-подготовки клиентам нужно добавить несколько дополнительных утверждений в Azure AD, таких как дата рождения пользователя и т. д. Эти сведения предоставляются назначенным **менеджером по обслуживанию клиентов Fidelity** и должны включать динамическую федерацию для вашего экземпляра.

6. На странице **Настройка единого входа с помощью SAML** в разделе **Сертификат подписи SAML** щелкните **Скачать**, чтобы скачать нужный вам **XML метаданных федерации**, и сохраните его на компьютере.

    ![Ссылка для скачивания сертификата](common/metadataxml.png)

7. Скопируйте требуемый URL-адрес из раздела **Настройка Fidelity NetBenefits**.

    ![Копирование URL-адресов настройки](common/copy-configuration-urls.png)

    а) URL-адрес входа.

    б) Идентификатор Azure AD.

    в) URL-адрес выхода.

### <a name="configure-fidelity-netbenefits-single-sign-on"></a>Настройка единого входа Fidelity NetBenefits

Чтобы настроить единый вход на стороне **Fidelity NetBenefits**, нужно отправить скачанный файл **XML метаданных федерации** и соответствующие URL-адреса, скопированные на портале Azure, [группе поддержки Fidelity NetBenefits](mailto:SSOMaintenance@fmr.com). Специалисты службы поддержки настроят подключение единого входа SAML на обеих сторонах.

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

В этом разделе описано, как разрешить пользователю Britta Simon использовать единый вход Azure, предоставив этому пользователю доступ к Fidelity NetBenefits.

1. На портале Azure выберите **Корпоративные приложения**, **Все приложения**, а затем — **Fidelity NetBenefits**.

    ![Колонка "Корпоративные приложения"](common/enterprise-applications.png)

2. Из списка приложений выберите **Fidelity NetBenefits**.

    ![Ссылка на Fidelity NetBenefits в списке приложений](common/all-applications.png)

3. В меню слева выберите **Пользователи и группы**.

    ![Ссылка "Пользователи и группы"](common/users-groups-blade.png)

4. Нажмите кнопку **Добавить пользователя**, а затем в диалоговом окне **Добавление назначения** выберите **Пользователи и группы**.

    ![Область "Добавление назначения"](common/add-assign-user.png)

5. В диалоговом окне **Пользователи и группы** из списка пользователей выберите **Britta Simon**, а затем в верхней части экрана нажмите кнопку **Выбрать**.

6. Если ожидается, что в утверждении SAML будет получено какое-либо значение роли, то в диалоговом окне **Выбор ролей** нужно выбрать соответствующую роль для пользователя из списка и затем нажать кнопку **Выбрать**, расположенную в нижней части экрана.

7. В диалоговом окне **Добавление назначения** нажмите кнопку **Назначить**.

### <a name="create-fidelity-netbenefits-test-user"></a>Создание тестового пользователя Fidelity NetBenefits

В этом разделе описано, как создать пользователя Britta Simon в Fidelity NetBenefits. В случае, если вы создаете статическую федерацию, обратитесь к своему назначенному **менеджеру по обслуживанию клиентов Fidelity**, чтобы создать пользователей на платформе Fidelity NetBenefits. Перед использованием единого входа необходимо создать и активировать этих пользователей.

Для динамической федерации пользователи создаются с помощью JIT-подготовки. Для использования JIT-подготовки клиентам нужно добавить несколько дополнительных утверждений в Azure AD, таких как дата рождения пользователя и т. д. Эти сведения предоставляются назначенным **менеджером по обслуживанию клиентов Fidelity** и должны включать динамическую федерацию для вашего экземпляра.

### <a name="test-single-sign-on"></a>Проверка единого входа 

В этом разделе описано, как проверить конфигурацию единого входа Azure AD с помощью панели доступа.

Щелкнув плитку Fidelity NetBenefits на панели доступа, вы автоматически войдете в приложение Fidelity NetBenefits, для которого настроили единый вход. См. дополнительные сведения о [панели доступа](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)

## <a name="additional-resources"></a>Дополнительные ресурсы

- [Список учебников по интеграции приложений SaaS с Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Что такое условный доступ в Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

