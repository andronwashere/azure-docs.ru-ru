---
title: Руководство по интеграции Azure Active Directory с консолью Oracle Cloud Infrastructure Console | Документация Майкрософт
description: Узнайте, как настроить единый вход между Azure Active Directory и Oracle Cloud Infrastructure Console.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: celested
ms.assetid: f045fe19-11f8-4ccf-a3eb-8495fdc8716f
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 06/10/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 456c984e577e3427ce8cd62d6f63987118f2c8ed
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/17/2019
ms.locfileid: "67164154"
---
# <a name="tutorial-integrate-oracle-cloud-infrastructure-console-with-azure-active-directory"></a>Руководство по интеграции консоли Oracle Cloud Infrastructure Console с Azure Active Directory

В этом учебнике описано, как интегрировать консоль Oracle Cloud Infrastructure Console с Azure Active Directory (Azure AD). Интеграция консоли Oracle Cloud Infrastructure Console с Azure AD обеспечивает следующие преимущества:

* С помощью Azure AD вы можете контролировать доступ к Oracle Cloud Infrastructure Console.
* Вы можете включить автоматический вход пользователей в консоль Oracle Cloud Infrastructure Console с использованием учетных записей Azure AD.
* Централизованное управление учетными записями через портал Azure.

Чтобы узнать больше об интеграции приложений SaaS с Azure AD, прочитайте статью [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

## <a name="prerequisites"></a>Предварительные требования

Чтобы приступить к работе, потребуется следующее:

* подписка Azure AD Если у вас нет подписки, вы можете получить [бесплатную учетную запись](https://azure.microsoft.com/free/).
* Подписка Oracle Cloud Infrastructure Console с поддержкой единого входа.

## <a name="scenario-description"></a>Описание сценария

В рамках этого руководства вы настроите и проверите единый вход Azure AD в тестовой среде. Консоль Oracle Cloud Infrastructure Console поддерживает единый вход, инициированный **поставщиком услуг**.

## <a name="adding-oracle-cloud-infrastructure-console-from-the-gallery"></a>Добавление консоли Oracle Cloud Infrastructure Console из коллекции

Чтобы настроить интеграцию консоли Oracle Cloud Infrastructure Console с Azure AD, необходимо добавить консоль Oracle Cloud Infrastructure Console из коллекции в список управляемых приложений SaaS.

1. Войдите на [портал Azure](https://portal.azure.com) с помощью личной учетной записи Майкрософт либо рабочей или учебной учетной записи.
1. В области навигации слева выберите службу **Azure Active Directory**.
1. Перейдите в колонку **Корпоративные приложения** и выберите **Все приложения**.
1. Чтобы добавить новое приложение, выберите **Новое приложение**.
1. В разделе **Добавление из коллекции** введите **Oracle Cloud Infrastructure Console** в поле поиска.
1. Выберите **Oracle Cloud Infrastructure Console** на панели результатов, а затем добавьте приложение. Подождите несколько секунд, пока приложение не будет добавлено в ваш клиент.

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Настройка и проверка единого входа в Azure AD

Настройте и проверьте единый вход Azure AD в Oracle Cloud Infrastructure Console с использованием тестового пользователя **B. Simon**. Для обеспечения работы единого входа необходимо установить связь между пользователем Azure AD и соответствующим пользователем в Oracle Cloud Infrastructure Console.

Чтобы настроить и проверить единый вход Azure AD в Oracle Cloud Infrastructure Console, вам потребуется выполнить действия в следующих стандартных блоках:

1. **[Настройка единого входа Azure AD](#configure-azure-ad-sso)** необходима, чтобы пользователи могли использовать эту функцию.
1. **[Настройка Oracle Cloud Infrastructure Console](#configure-oracle-cloud-infrastructure-console)** требуется, чтобы настроить параметры единого входа на стороне приложения.
1. **[Создание тестового пользователя Azure AD](#create-an-azure-ad-test-user)** требуется для проверки работы единого входа Azure AD от имени пользователя B. Simon.
1. **[Назначение тестового пользователя Azure AD](#assign-the-azure-ad-test-user)** необходимо, чтобы позволить B. Simon использовать единый вход Azure AD.
1. **[Создание тестового пользователя Оracle Cloud Infrastructure Console](#create-oracle-cloud-infrastructure-console-test-user)** требуется для того, чтобы в консоли Oracle Cloud Infrastructure Console существовал пользователь Britta Simon, связанный с представлением этого же пользователя в Azure AD.
1. **[Проверка единого входа](#test-sso)** необходима, чтобы убедиться в корректной работе конфигурации.

### <a name="configure-azure-ad-sso"></a>Настройка единого входа Azure AD

Выполните следующие действия, чтобы включить единый вход Azure AD на портале Azure.

1. На [портале Azure](https://portal.azure.com/) на странице интеграции с приложением **Oracle Cloud Infrastructure Console** найдите раздел **Управление** и выберите **Единый вход**.
1. На странице **Выбрать метод единого входа** выберите **SAML**.
1. На странице **Настройка единого входа с помощью SAML** щелкните значок "Изменить" (значок пера), чтобы открыть диалоговое окно **Базовая конфигурация SAML** и изменить параметры.

   ![Правка базовой конфигурации SAML](common/edit-urls.png)

1. На странице **Базовая конфигурация SAML** введите значения для следующих полей.

   > [!NOTE]
   > Вы получите файл метаданных поставщика служб, выполнив инструкции, указанные в разделе о **настройке единого входа в Oracle Cloud Infrastructure Console** в этом учебнике.
    
   1. Щелкните **Отправить файл метаданных**.

   1. Щелкните **значок папки**, выберите файл метаданных и нажмите кнопку **Отправить**.

   1. После успешной передачи файла метаданных значения **Идентификатор** и **URL-адрес ответа** в текстовом поле раздела **Базовая конфигурация SAML** заполнятся автоматически.
    
      > [!NOTE]
      > Если поля **Идентификатор** и **URL-адрес ответа** автоматически не заполняются, введите эти значения вручную в соответствии с поставленной задачей.

      В текстовое поле **URL-адрес для входа** введите URL-адрес в следующем формате: `https://console.<REGIONNAME>.oraclecloud.com/`.

      > [!NOTE]
      > Это значение приведено для примера. Вместо него необходимо указать фактический URL-адрес входа. Чтобы получить значение, обратитесь в [службу поддержки клиентов Oracle Cloud Infrastructure Console](https://www.oracle.com/support/advanced-customer-support/products/cloud.html). Можно также посмотреть шаблоны в разделе **Базовая конфигурация SAML** на портале Azure.

1. На странице **Настройка единого входа с помощью SAML** в разделе **Сертификат подписи SAML** найдите элемент **XML метаданных федерации** и нажмите кнопку **Скачать**, чтобы скачать сертификат и сохранить его на компьютере.

   ![Ссылка для скачивания сертификата](common/metadataxml.png)

1. Приложение Oracle Cloud Infrastructure Console ожидает проверочные утверждения SAML в определенном формате, который требует добавить сопоставления настраиваемых атрибутов в вашу конфигурацию атрибутов токена SAML. На следующем снимке экрана показан список атрибутов по умолчанию. Щелкните значок **Изменить** , чтобы открыть диалоговое окно "Атрибуты пользователя".

   ![image](common/edit-attribute.png)

1. В дополнение к описанному выше приложение Oracle Cloud Infrastructure Console ожидает несколько дополнительных атрибутов в ответе SAML. В разделе **Утверждения и атрибуты пользователя** в диалоговом окне **Утверждения групп (предварительная версия)**  выполните следующие действия:

   1. Щелкните значок **пера** рядом со **значением идентификатора имени**.

   1. Щелкните **Постоянный** в списке **Выбор формата идентификатора имени**.
 
   1. Выберите команду **Сохранить**.

      ![image](./media/oracle-cloud-tutorial/config07.png)
    
      ![image](./media/oracle-cloud-tutorial/config11.png)

   1. Щелкните **значок ручки** рядом с параметром **Группы, возвращенные в утверждении**.

   1. Установите переключатель **Группы безопасности** в списке.

   1. Выберите **исходный атрибут** **идентификатора группы**.

   1. Установите флажок **Изменение имени утверждения группы**.

   1. В текстовом поле **Имя** введите **groupName**.

   1. В текстовом поле **Пространство имен (необязательно)** введите `https://auth.oraclecloud.com/saml/claims`.

   1. Выберите команду **Сохранить**.

      ![image](./media/oracle-cloud-tutorial/config08.png)

1. Скопируйте требуемый URL-адрес из раздела **Настройка Oracle Cloud Infrastructure Console**.

   ![Копирование URL-адресов настройки](common/copy-configuration-urls.png)

### <a name="configure-oracle-cloud-infrastructure-console"></a>Настройка Oracle Cloud Infrastructure Console

1. В другом окне веб-браузера войдите в консоль Oracle Cloud Infrastructure Console от имени администратора.

1. Щелкните левую часть меню и выберите **Identity** (Идентификатор), а затем перейдите к разделу **Federation** (Федерация).

   ![Конфигурация](./media/oracle-cloud-tutorial/config01.png)

1. Сохраните файл метаданных **поставщика услуг**, щелкнув ссылку **Download this document** (Скачать этот документ) и передайте его в раздел **Базовая конфигурация SAML** на портале Azure, а затем щелкните **Add Identity Provider** (Добавить поставщик удостоверений).

   ![Конфигурация](./media/oracle-cloud-tutorial/config02.png)

1. Во всплывающем окне **Add Identity Provider** (Добавление поставщика удостоверений) выполните следующие действия:

   ![Конфигурация](./media/oracle-cloud-tutorial/config03.png)

   1. В текстовом поле **Name** (Имя) введите имя.

   1. В текстовом поле **Description** (Описание) введите описание.

   1. В качестве **типа** выберите **Мicrosoft Active Directory federation service (ADFS) or SAML 2.0 compliant identity provider** (Служба федерации Active Directory Microsoft (ADFS) или поставщик удостоверений, совместимый с SAML 2.0).

   1. Затем щелкните **Browse** (Обзор), чтобы передать XML-файл метаданных федерации, скачанный с портала Azure.

   1. Нажмите кнопку **Продолжить** и в разделе **Edit Identity Provider** (Изменение поставщика удостоверений) выполните следующие действия:

      ![Конфигурация](./media/oracle-cloud-tutorial/config09.png)

   1. В полях раздела **Identity provider group** (Группа поставщиков удостоверений) введите имя и идентификатор группы, настроенные на портале Azure. Группу необходимо сопоставить с соответствующей группой в поле **Oci group** (Группа OCI).

   1. Вы можете сопоставить несколько групп в соответствии с настройками на портале Azure и потребностями вашей организации. Щелкните **+ Add mapping** (+ Добавить сопоставление), чтобы добавить столько групп, сколько вам требуется.

   1. Нажмите кнопку **Submit**(Отправить).

### <a name="create-an-azure-ad-test-user"></a>Создание тестового пользователя Azure AD

В этом разделе описано, как на портале Azure создать тестового пользователя с именем B. Simon.

1. На портале Azure в области слева выберите **Azure Active Directory**, **Пользователи**, а затем — **Все пользователи**.
1. В верхней части экрана выберите **Новый пользователь**.
1. В разделе **Свойства пользователя** выполните следующие действия.
   1. В поле **Имя** введите `B. Simon`.  
   1. В поле **Имя пользователя** введите username@companydomain.extension. Например, `B. Simon@contoso.com`.
   1. Установите флажок **Показать пароль** и запишите значение, которое отображается в поле **Пароль**.
   1. Нажмите кнопку **Создать**.

### <a name="assign-the-azure-ad-test-user"></a>Назначение тестового пользователя Azure AD

В этом разделе описано, как включить единый вход Azure для пользователя B. Simon и как предоставить такому пользователю доступ к Oracle Cloud Infrastructure Console.

1. На портале Azure выберите **Корпоративные приложения**, а затем —**Все приложения**.
1. В списке приложений выберите **Oracle Cloud Infrastructure Console**.
1. На странице "Обзор" приложения найдите раздел **Управление** и выберите **Пользователи и группы**.

   ![Ссылка "Пользователи и группы"](common/users-groups-blade.png)

1. Выберите **Добавить пользователя**, а в диалоговом окне **Добавление назначения** выберите **Пользователи и группы**.

   ![Ссылка "Добавить пользователя"](common/add-assign-user.png)

1. В диалоговом окне **Пользователи и группы** выберите **B. Simon** из списка пользователей, а затем в нижней части экрана нажмите кнопку **Выбрать**.
1. Если ожидается, что в утверждении SAML будет получено какое-либо значение роли, то в диалоговом окне **Выбор роли** нужно выбрать соответствующую роль для пользователя из списка и затем нажать кнопку **Выбрать**, расположенную в нижней части экрана.
1. В диалоговом окне **Добавление назначения** нажмите кнопку **Назначить**.

### <a name="create-oracle-cloud-infrastructure-console-test-user"></a>Создание тестового пользователя в Oracle Cloud Infrastructure Console

 Oracle Cloud Infrastructure Console поддерживает JIT-подготовку. Эта функция включена по умолчанию. В этом разделе никакие действия с вашей стороны не требуются. Новый пользователь не создается при попытке доступа, и вам не нужно создавать пользователя.

### <a name="test-sso"></a>Проверка единого входа

Выбрав плитку Oracle Cloud Infrastructure Console на Панели доступа, вы будете перенаправлены на страницу входа в Oracle Cloud Infrastructure Console. Щелкните **Identity provider** (Поставщик удостоверений) в раскрывающемся меню и нажмите кнопку **Continue** (Продолжить), чтобы войти в систему, как показано ниже. См. дополнительные сведения о [панели доступа](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)

![Конфигурация](./media/oracle-cloud-tutorial/config10.png)

## <a name="additional-resources"></a>Дополнительные ресурсы

- [Список учебников по интеграции приложений SaaS с Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Что представляет собой условный доступ в Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
