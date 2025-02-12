---
title: Руководство по Интеграция Azure Active Directory с FreshDesk | Документация Майкрософт
description: Узнайте, как настроить единый вход между Azure Active Directory и FreshDesk.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: c2a3e5aa-7b5a-4fe4-9285-45dbe6e8efcc
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 12/21/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 3ed903e71a019d0bdb1b62503913bd696e9d8a8d
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "67102047"
---
# <a name="tutorial-azure-active-directory-integration-with-freshdesk"></a>Руководство по Интеграция Azure Active Directory с FreshDesk

В этом руководстве описано, как интегрировать приложение FreshDesk с Azure Active Directory (Azure AD).
Интеграция Azure AD с FreshDesk обеспечивает следующие преимущества:

* В Azure AD можно контролировать, у кого есть доступ к FreshDesk.
* Вы можете включить автоматический вход пользователей во FreshDesk (единый вход) с помощью учетных записей Azure AD.
* Вы можете управлять учетными записями централизованно на портале Azure.

Дополнительные сведения об интеграции приложений SaaS с Azure AD см. в статье [Единый вход в приложениях в Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Если у вас еще нет подписки Azure, [создайте бесплатную учетную запись Azure](https://azure.microsoft.com/free/), прежде чем начинать работу.

## <a name="prerequisites"></a>Предварительные требования

Чтобы настроить интеграцию Azure AD с FreshDesk, вам потребуется:

* подписка Azure AD (если у вас нет среды Azure AD, вы можете получить пробную версию на один месяц по [этой ссылке](https://azure.microsoft.com/pricing/free-trial/));
* Подписка FreshDesk с поддержкой единого входа.

## <a name="scenario-description"></a>Описание сценария

В рамках этого руководства вы настроите и проверите единый вход Azure AD в тестовой среде.

* FreshDesk поддерживает единый вход инициированного **пакета обновления**.

## <a name="adding-freshdesk-from-the-gallery"></a>Добавление FreshDesk из коллекции

Чтобы настроить интеграцию FreshDesk с Azure AD, необходимо добавить FreshDesk из коллекции в список управляемых приложений SaaS.

**Чтобы добавить FreshDesk из коллекции, выполните следующие действия:**

1. На **[портале Azure](https://portal.azure.com)** в области навигации слева щелкните значок **Azure Active Directory**.

    ![Кнопка Azure Active Directory](common/select-azuread.png)

2. Перейдите в колонку **Корпоративные приложения** и выберите **Все приложения**.

    ![Колонка "Корпоративные приложения"](common/enterprise-applications.png)

3. Чтобы добавить новое приложение, в верхней части диалогового окна нажмите кнопку **Создать приложение**.

    ![Кнопка "Создать приложение"](common/add-new-app.png)

4. В поле поиска введите **FreshDesk**, выберите **FreshDesk** на панели результатов и нажмите кнопку **Добавить**, чтобы добавить это приложение.

     ![FreshDesk в списке результатов](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Настройка и проверка единого входа в Azure AD

В этом разделе описана настройка и проверка единого входа Azure AD в приложение FreshDesk с использованием тестового пользователя **Britta Simon**.
Для обеспечения работы единого входа необходимо установить связь между пользователем Azure AD и соответствующим пользователем во FreshDesk.

Чтобы настроить и проверить единый вход Azure AD в FreshDesk, вам потребуется выполнить действия в следующих стандартных блоках:

1. **[Настройка единого входа Azure AD](#configure-azure-ad-single-sign-on)** необходима, чтобы пользователи могли использовать эту функцию.
2. **[Настройка единого входа во FreshDesk](#configure-freshdesk-single-sign-on)** необходима, чтобы настроить параметры единого входа на стороне приложения.
3. **[Создание тестового пользователя Azure AD](#create-an-azure-ad-test-user)** требуется для проверки работы единого входа Azure AD от имени пользователя Britta Simon.
4. **[Назначение тестового пользователя Azure AD](#assign-the-azure-ad-test-user)** необходимо, чтобы разрешить пользователю Britta Simon использовать единый вход Azure AD.
5. **[Создание тестового пользователя FreshDesk](#create-freshdesk-test-user)** требуется для того, чтобы во FreshDesk существовал пользователь Britta Simon, связанный с представлением этого же пользователя в Azure AD.
6. **[Проверка единого входа](#test-single-sign-on)** необходима, чтобы проверить работу конфигурации.

### <a name="configure-azure-ad-single-sign-on"></a>Настройка единого входа Azure AD

В этом разделе описано включение единого входа Azure AD на портале Azure.

Чтобы настроить единый вход Azure AD во FreshDesk, выполните следующие действия.

1. На [портале Azure](https://portal.azure.com/) на странице интеграции с приложением **FreshDesk** выберите **Единый вход**.

    ![Ссылка "Настройка единого входа"](common/select-sso.png)

2. В диалоговом окне **Выбрать метод единого входа** выберите режим **SAML/WS-Fed**, чтобы включить единый вход.

    ![Режим выбора единого входа](common/select-saml-option.png)

3. На странице **Настройка единого входа с помощью SAML** щелкните **Изменить**, чтобы открыть диалоговое окно **Базовая конфигурация SAML**.

    ![Правка базовой конфигурации SAML](common/edit-urls.png)

4. В разделе **Базовая конфигурация SAML** выполните приведенные ниже действия.

    ![Сведения о домене и URL-адресах единого входа FreshDesk](common/sp-identifier.png)

    a. В текстовом поле **URL-адрес для входа** введите URL-адрес в формате `https://<tenant-name>.freshdesk.com` или любом другом, поддерживаемом Freshdesk.

    b. В текстовом поле **Идентификатор (сущности)** введите URL-адрес в формате `https://<tenant-name>.freshdesk.com` или любом другом, поддерживаемом Freshdesk.

    > [!NOTE]
    > Эти значения приведены для примера. Необходимо обновить эти значения действующим URL-адресом для входа и идентификатором. Для получения этих значений обратитесь к [группе поддержки клиентов FreshDesk](https://freshdesk.com/helpdesk-software?utm_source=Google-AdWords&utm_medium=Search-IND-Brand&utm_campaign=Search-IND-Brand&utm_term=freshdesk&device=c&gclid=COSH2_LH7NICFVUDvAodBPgBZg). Можно также посмотреть шаблоны в разделе **Базовая конфигурация SAML** на портале Azure.

5. Приложение FreshDesk ожидает проверочные утверждения SAML в определенном формате, который требует добавить настраиваемые сопоставления атрибутов в конфигурацию атрибутов токена SAML. На следующем снимке экрана приведен пример. По умолчанию **Уникальный идентификатор пользователя** имеет значение **user.userprincipalname**, но для FreshDesk требуется сопоставить это значение с адресом электронной почты пользователя. Для этого можно использовать атрибут **user.mail** из списка или соответствующее значение атрибута, основанное на конфигурации организации. 

    ![image](common/edit-attribute.png)

6. В разделе **Утверждения пользователя** диалогового окна **Атрибуты пользователя** измените утверждения, щелкнув значок **Изменить**, или добавьте утверждение, нажав кнопку **Добавить новое утверждение**, чтобы настроить атрибут токена SAML, как показано на рисунке выше, и выполните следующие действия.
    
    | ИМЯ | Исходный атрибут |
    | ---------------| --------------- |
    | Уникальный идентификатор пользователя | user.mail |

    a. Щелкните **Добавить новое утверждение**, чтобы открыть диалоговое окно **Управление утверждениями пользователя**.

    ![image](common/new-save-attribute.png)

    ![image](common/new-attribute-details.png)

    b. В текстовом поле **Имя** введите имя атрибута, отображаемое для этой строки.

    c. Оставьте пустым поле **Пространство имен**.

    d. В качестве источника выберите **Атрибут**.

    д. В списке **Атрибут источника** введите значение атрибута, отображаемое для этой строки.

    Е. Нажмите кнопку **ОК**.

    ж. Выберите команду **Сохранить**.

7. На странице **Настройка единого входа с помощью SAML** в разделе **Сертификат подписи SAML** щелкните **Загрузить**, чтобы загрузить требуемый **сертификат (Base64)** из предложенных вариантов, и сохраните его на компьютере.

    ![Ссылка для скачивания сертификата](common/certificatebase64.png)

8. Откройте **командную строку** и выполните следующие команды:

    a. Введите в командной строке значение `certutil.exe -dump FreshDesk.cer`.

    > [!NOTE]
    > В нашем случае **FreshDesk.cer** — это сертификат, загруженный с портала Azure.

    b. Скопируйте значение **Cert Hash(sha256)** и вставьте его в Блокнот. 

9. Требуемый URL-адрес можно скопировать из раздела **Set up FreshDesk** (Настройка FreshDesk).

    ![Копирование URL-адресов настройки](common/copy-configuration-urls.png)

    а) URL-адрес входа.

    б) Идентификатор Azure AD.

    в) URL-адрес выхода.

### <a name="configure-freshdesk-single-sign-on"></a>Настройка FreshDesk с поддержкой единого входа

1. В другом окне браузера войдите на свой корпоративный веб-сайт Freshdesk в качестве администратора.

2. Щелкните значок **Настройки** и в разделе **Безопасность** выполните следующие действия:

    ![Единый вход](./media/freshdesk-tutorial/IC776770.png "Единый вход")
  
    a. Выберите для параметра **Single Sign On (SSO)** (Единый вход) значение **On** (Включено).

    b. Выберите **Единый вход SAML**.

    c. В текстовое поле **SAML Login URL** (URL-адрес SAML) вставьте значение **URL-адрес входа**, скопированное на портале Azure.

    d. В текстовое поле **Logout URL** (URL-адрес выхода) вставьте значение **URL-адрес выхода**, скопированное на портале Azure.

    д. В текстовое поле **Security Certificate Fingerprint** (Отпечаток сертификата безопасности) вставьте значение **Cert Hash(sha256)** , полученное ранее.
  
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

В этом разделе описано, как разрешить пользователю Britta Simon использовать единый вход Azure, предоставив доступ к FreshDesk.

1. На портале Azure выберите **Корпоративные приложения**, **Все приложения**, а затем — **FreshDesk**.

    ![Колонка "Корпоративные приложения"](common/enterprise-applications.png)

2. В списке приложений введите и выберите **FreshDesk**.

    ![Ссылка на FreshDesk в списке приложений](common/all-applications.png)

3. В меню слева выберите **Пользователи и группы**.

    ![Ссылка "Пользователи и группы"](common/users-groups-blade.png)

4. Нажмите кнопку **Добавить пользователя**, а затем в диалоговом окне **Добавление назначения** выберите **Пользователи и группы**.

    ![Область "Добавление назначения"](common/add-assign-user.png)

5. В диалоговом окне **Пользователи и группы** из списка пользователей выберите **Britta Simon**, а затем в верхней части экрана нажмите кнопку **Выбрать**.

6. Если ожидается, что в утверждении SAML будет получено какое-либо значение роли, то в диалоговом окне **Выбор ролей** нужно выбрать соответствующую роль для пользователя из списка и затем нажать кнопку **Выбрать**, расположенную в нижней части экрана.

7. В диалоговом окне **Добавление назначения** нажмите кнопку **Назначить**.

### <a name="create-freshdesk-test-user"></a>Создание тестового пользователя FreshDesk

Чтобы пользователи Azure AD могли входить во FreshDesk, их необходимо подготовить во FreshDesk.  
В случае с FreshDesk подготовка выполняется вручную.

**Чтобы подготовить учетные записи пользователей, выполните следующие действия.**

1. Выполните вход в клиент **Freshdesk** .

2. В верхнем меню щелкните **Администратор**.

    ![Администратор](./media/freshdesk-tutorial/IC776772.png "Администратор")

3. На вкладке **General Settings** (Общие параметры) выберите **Agents** (Агенты).
  
    ![Агенты](./media/freshdesk-tutorial/IC776773.png "Агенты")

4. Нажмите **Создать агента**.

    ![Создание агента](./media/freshdesk-tutorial/IC776774.png "Создание агента")

5. В диалоговом окне "Сведения об агенте " выполните следующие действия.

    ![Сведения об агенте](./media/freshdesk-tutorial/IC776775.png "Сведения об агенте")

    a. В текстовое поле **Электронная почта** введите адрес электронной почты той учетной записи Azure AD, которую вы хотите подготовить.

    b. В текстовое поле **Полное имя** введите имя учетной записи Azure AD, которую желаете подготовить.

    c. В текстовое поле **Название** введите название учетной записи Azure AD, которую желаете подготовить.

    d. Выберите команду **Сохранить**.

    >[!NOTE]
    >Владелец учетной записи Azure AD получит по электронной почте сообщение со ссылкой для активации учетной записи.
    >
    >[!NOTE]
    >Чтобы подготовить учетные записи пользователей AAD к работе с FreshDesk, можно использовать любые другие средства создания учетной записи пользователя Freshdesk или API, предоставляемые Freshdesk.

### <a name="test-single-sign-on"></a>Проверка единого входа 

В этом разделе описано, как проверить конфигурацию единого входа Azure AD с помощью панели доступа.

Щелкнув плитку "FreshDesk" на панели доступа, вы автоматически войдете во FreshDesk, для которого настроили единый вход. См. дополнительные сведения о [панели доступа](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)

## <a name="additional-resources"></a>Дополнительные ресурсы

- [Список учебников по интеграции приложений SaaS с Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Что такое условный доступ в Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

