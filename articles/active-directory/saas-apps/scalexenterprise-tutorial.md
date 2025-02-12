---
title: Руководство. Интеграция Azure Active Directory со ScaleX Enterprise | Документация Майкрософт
description: Узнайте, как настроить единый вход Azure Active Directory в ScaleX Enterprise.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: c2379a8d-a659-45f1-87db-9ba156d83183
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/20/2017
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 64edf2aa47211c1d2a598417a7b2edc00f260075
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "60321389"
---
# <a name="tutorial-azure-active-directory-integration-with-scalex-enterprise"></a>Руководство. Интеграция Azure Active Directory со ScaleX Enterprise

В этом руководстве описано, как интегрировать ScaleX Enterprise с Azure Active Directory (Azure AD).

Интеграция ScaleX Enterprise с Azure AD обеспечивает следующие преимущества:

- С помощью Azure AD вы можете контролировать, у кого есть доступ к приложению ScaleX Enterprise.
- Вы можете включить автоматический вход пользователей в ScaleX Enterprise (единый вход) с использованием учетной записи Azure AD.
- Вы можете управлять учетными записями централизованно — через портал Azure.

Подробнее узнать об интеграции приложений SaaS с Azure AD можно в статье [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

## <a name="prerequisites"></a>Технические условия

Чтобы настроить интеграцию Azure AD с ScaleX Enterprise, вам потребуется:

- подписка Azure AD;
- подписка ScaleX Enterprise с поддержкой единого входа.

> [!NOTE]
> Мы не рекомендуем использовать рабочую среду для проверки действий в этом учебнике.

При проверке действий в этом учебнике соблюдайте следующие рекомендации:

- Не используйте рабочую среду без необходимости.
- Если у вас нет пробной среды Azure AD, вы можете получить пробную версию на один месяц по [этой ссылке](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Описание сценария
В рамках этого руководства проводится проверка единого входа Azure AD в тестовой среде. Сценарий, описанный в этом учебнике, состоит из двух стандартных блоков.

1. Добавление ScaleX Enterprise из коллекции
1. настройка и проверка единого входа в Azure AD.

## <a name="adding-scalex-enterprise-from-the-gallery"></a>Добавление ScaleX Enterprise из коллекции
Чтобы настроить интеграцию приложения ScaleX Enterprise с Azure AD, вам нужно добавить это приложение из коллекции в свой список управляемых приложений SaaS.

**Добавление приложения ScaleX Enterprise из коллекции**

1. На **[портале Azure](https://portal.azure.com)** в области навигации слева щелкните значок **Azure Active Directory**. 

    ![Active Directory][1]

1. Перейдите к разделу **Корпоративные приложения**. Затем выберите **Все приложения**.

    ![ПРИЛОЖЕНИЯ][2]
    
1. Нажмите кнопку **Добавить** в верхней части диалогового окна.

    ![ПРИЛОЖЕНИЯ][3]

1. В поле поиска введите **ScaleX Enterprise**.

    ![Создание тестового пользователя Azure AD](./media/scalexenterprise-tutorial/tutorial_scalexenterprise_search.png)

1. На панели результатов выберите **ScaleX Enterprise** и нажмите кнопку **Добавить**, чтобы добавить приложение.

    ![Создание тестового пользователя Azure AD](./media/scalexenterprise-tutorial/tutorial_scalexenterprise_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>настройка и проверка единого входа в Azure AD.
В этом разделе описана настройка и проверка единого входа Azure AD в ScaleX Enterprise с использованием тестового пользователя Britta Simon.

Для работы единого входа службе Azure AD нужно знать, какой пользователь в ScaleX Enterprise соответствует пользователю в Azure AD. Иными словами, нужно установить связь между пользователем Azure AD и соответствующим пользователем в ScaleX Enterprise.

Чтобы установить эту связь, следует назначить **имя пользователя** в Azure AD в качестве значения **имени пользователя** в ScaleX Enterprise.

Чтобы настроить и проверить единый вход Azure AD в ScaleX Enterprise, выполните следующие действия:

1. **[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** необходима, чтобы пользователи могли использовать эту функцию.
1. **[Создание тестового пользователя Azure AD](#creating-an-azure-ad-test-user)** требуется для проверки работы единого входа в Azure AD от имени пользователя Britta Simon.
1. **[Создание тестового пользователя ScaleX Enterprise](#creating-a-scalex-enterprise-test-user)** требуется для создания пользователя Britta Simon в ScaleX Enterprise, связанного с соответствующим пользователем в Azure AD.
1. **[Назначение тестового пользователя Azure AD](#assigning-the-azure-ad-test-user)** необходимо, чтобы позволить Britta Simon использовать единый вход Azure AD;
1. **[Проверка единого входа](#testing-single-sign-on)** необходима, чтобы убедиться в корректной работе конфигурации.

### <a name="configuring-azure-ad-single-sign-on"></a>Настройка единого входа в Azure AD

В этом разделе мы включим на портале Azure единый вход Azure AD и настроим его в приложении ScaleX Enterprise.

**Настройка единого входа Azure AD в ScaleX Enterprise**

1. На портале Azure на странице интеграции с приложением **ScaleX Enterprise** щелкните **Единый вход**.

    ![Настройка единого входа][4]

1. В диалоговом окне **Единый вход** в разделе **Режим** выберите **Вход на основе SAML**, чтобы включить функцию единого входа.
 
    ![Настройка единого входа](./media/scalexenterprise-tutorial/tutorial_scalexenterprise_samlbase.png)

1. Если вы хотите настроить приложение в режиме, инициированном **поставщиком удостоверений**, то в разделе **Домены и URL-адреса приложения ScaleX Enterprise** сделайте следующее:

    ![Настройка единого входа](./media/scalexenterprise-tutorial/tutorial_scalexenterprise_url1.png)

    a. В текстовом поле **Идентификатор** введите значение в следующем формате: `https://platform.rescale.com/saml2/<company id>/`.

    2\. В текстовом поле **URL-адрес ответа** введите URL-адрес в следующем формате: `https://platform.rescale.com/saml2/<company id>/acs/`.

1. Установите флажок **Показать дополнительные параметры URL-адресов**, если вы хотите настроить приложение для работы в режиме, инициируемом **поставщиком услуг**.

    ![Настройка единого входа](./media/scalexenterprise-tutorial/tutorial_scalexenterprise_url2.png)

    В текстовом поле **URL-адрес для входа** введите значение в следующем формате: `https://platform.rescale.com/saml2/<company id>/sso/`.
     
    > [!NOTE] 
    > Значения, указанные выше, приведены в качестве примера. Измените их на фактические значения идентификатора, URL-адреса ответа или URL-адреса входа. Чтобы получить эти значения, обратитесь в [службу поддержки клиентов ScaleX Enterprise](https://info.rescale.com/contact_sales). 

1. Приложение ScaleX ожидает проверочные утверждения SAML в определенном формате, который требует изменить настраиваемые сопоставления атрибутов для конфигурации атрибутов токена SAML. Установите флажок **Просмотреть и изменить все другие атрибуты пользователей**, чтобы открыть пользовательские параметры атрибутов.

    ![Настройка единого входа](./media/scalexenterprise-tutorial/scalex_attributes.png)
    
    a. Щелкните правой кнопкой мыши атрибут **name** и щелкните "Удалить".

    ![Настройка единого входа](./media/scalexenterprise-tutorial/delete_attribute_name.png)

    2\. Щелкните атрибут **emailaddress**, чтобы открыть окно "Изменить атрибут". Измените его значение с **user.mail** на **user.userprincipalname** и нажмите кнопку "ОК".

    ![Настройка единого входа](./media/scalexenterprise-tutorial/edit_email_attribute.png) 
    
1. В разделе **Сертификат подписи SAML** щелкните **Сертификат (Base64)** , а затем сохраните файл сертификата на компьютере.

    ![Настройка единого входа](./media/scalexenterprise-tutorial/tutorial_scalexenterprise_certificate.png) 

1. Нажмите кнопку **Сохранить** .

    ![Настройка единого входа](./media/scalexenterprise-tutorial/tutorial_general_400.png)
    
1. В разделе **Настройка ScaleX Enterprise** щелкните **Настроить ScaleX Enterprise**, чтобы открыть окно **Настройка единого входа**. Скопируйте **идентификатор сущности SAML** и **URL-адрес службы единого входа SAML** из раздела **Краткий справочник**.

    ![Настройка единого входа](./media/scalexenterprise-tutorial/tutorial_scalexenterprise_configure.png) 

1. Чтобы настроить единый вход на стороне **ScaleX Enterprise**, войдите на веб-сайт компании ScaleX Enterprise в качестве администратора.

1. В правом верхнем углу щелкните меню и выберите **Contoso Administration** (Администрирование Contoso).

    > [!NOTE] 
    > Contoso является лишь примером. Используйте фактическое название компании. 

    ![Настройка единого входа](./media/scalexenterprise-tutorial/Test_Admin.png) 

1. В верхнем меню выберите **Integrations** (Интеграции) и щелкните **Single Sign-On** (Единый вход).

    ![Настройка единого входа](./media/scalexenterprise-tutorial/admin_sso.png) 

1. Заполните форму следующим образом.

    ![Настройка единого входа](./media/scalexenterprise-tutorial/scalex_admin_save.png) 
    
    a. Выберите **Create any user who can authenticate with SSO** (Создание любого пользователя, который может выполнить проверку подлинности с помощью единого входа).

    2\. **Service Provider SAML** (Поставщик услуг SAML). Вставьте значение ***urn:oasis:names:tc:SAML:2.0:nameid-format:persistent***.

    c. **Name of Identity Provider email field in ACS response** (Имя поля электронного адреса поставщика удостоверений в ответе ACS). Вставьте значение `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`.

    d. **Identity Provider EntityDescriptor Entity ID** (Идентификатор сущности EntityDescriptor поставщика удостоверений). Вставьте значение **идентификатора сущности SAML**, скопированное на портале Azure.

    д. **Identity Provider SingleSignOnService URL** (URL-адрес SingleSignOnService поставщика удостоверений). Вставьте значение **URL-адреса службы единого входа SAML**, скопированное на портале Azure.

    Е. **Identity Provider public X509 certificate** (Общий сертификат X509 поставщика удостоверений). Откройте сертификат X509, скачанный на портале Azure, в Блокноте и вставьте содержимое в это поле. Убедитесь в отсутствии разрыва строк в середине содержимого сертификата.
    
    ж. Установите следующие флажки: **Enabled, Encrypt NameID, Sign AuthnRequests** ("Включено", "Шифрование идентификатора имени", "Подпись AuthnRequests").

    h. Нажмите кнопку **Update SSO Settings** (Обновить параметры единого входа), чтобы сохранить настройки.

> [!TIP]
> Краткую версию этих инструкций теперь можно также прочитать на [портале Azure](https://portal.azure.com) во время настройки приложения.  После добавления этого приложения из раздела **Active Directory > Корпоративные приложения** просто выберите вкладку **Единый вход** и откройте встроенную документацию через раздел **Настройка** в нижней части страницы. Дополнительные сведения о встроенной документации см. в статье [Руководство. Настройка единого входа на основе SAML для приложения в Azure Active Directory]( https://go.microsoft.com/fwlink/?linkid=845985).
> 

### <a name="creating-an-azure-ad-test-user"></a>Создание тестового пользователя Azure AD
Цель этого раздела — создать на портале Azure тестового пользователя с именем Britta Simon.

![Создание пользователя Azure AD][100]

**Чтобы создать тестового пользователя в Azure AD, выполните следующие действия:**

1. На **портале Azure** в области навигации слева щелкните значок **Azure Active Directory**.

    ![Создание тестового пользователя Azure AD](./media/scalexenterprise-tutorial/create_aaduser_01.png) 

1. Перейдите в раздел **Пользователи и группы** и щелкните **Все пользователи**, чтобы отобразить список пользователей.
    
    ![Создание тестового пользователя Azure AD](./media/scalexenterprise-tutorial/create_aaduser_02.png) 

1. В верхней части диалогового окна щелкните **Добавить**, чтобы открыть диалоговое окно **Пользователь**.
 
    ![Создание тестового пользователя Azure AD](./media/scalexenterprise-tutorial/create_aaduser_03.png) 

1. На странице диалогового окна **Пользователь** выполните следующие действия.
 
    ![Создание тестового пользователя Azure AD](./media/scalexenterprise-tutorial/create_aaduser_04.png) 

    a. В текстовом поле **Имя** введите **BrittaSimon**.

    2\. В текстовом поле **Имя пользователя** введите **адрес электронной почты** учетной записи BrittaSimon.

    c. Выберите **Показать пароль** и запишите значение поля **Пароль**.

    d. Нажмите кнопку **Создать**.
 
### <a name="creating-a-scalex-enterprise-test-user"></a>Создание тестового пользователя ScaleX Enterprise

Чтобы пользователи Azure AD могли входить в ScaleX Enterprise, их нужно подготовить для ScaleX Enterprise. В случае с ScaleX Enterprise подготовка пользователей осуществляется автоматически, при этом никакие ручные действия не требуются. Для всех пользователей, которые успешно выполнят проверку подлинности с помощью учетных данных единого входа, будет автоматически выполняться подготовка на стороне ScaleX.

### <a name="assigning-the-azure-ad-test-user"></a>Назначение тестового пользователя Azure AD

В этом разделе мы разрешим пользователю Britta Simon использовать единый вход Azure, предоставив этому пользователю доступ к ScaleX Enterprise.

![Назначение пользователя][200] 

**Назначение пользователя Britta Simon приложению ScaleX Enterprise**

1. На портале Azure откройте представление приложений, перейдите к представлению каталога, а затем выберите **Корпоративные приложения** и щелкните **Все приложения**.

    ![Назначение пользователя][201] 

1. В списке приложений выберите **ScaleX Enterprise**.

    ![Настройка единого входа](./media/scalexenterprise-tutorial/tutorial_scalexenterprise_app.png) 

1. В меню слева выберите **Пользователи и группы**.

    ![Назначение пользователя][202] 

1. Нажмите кнопку **Добавить**. Затем в диалоговом окне **Добавление назначения** выберите **Пользователи и группы**.

    ![Назначение пользователя][203]

1. В диалоговом окне **Пользователи и группы** в списке пользователей выберите **Britta Simon**.

1. В диалоговом окне **Пользователи и группы** нажмите кнопку **Выбрать**.

1. В диалоговом окне **Добавление назначения** нажмите кнопку **Назначить**.

### <a name="testing-single-sign-on"></a>Проверка единого входа

В этом разделе описано, как проверить конфигурацию единого входа Azure AD с помощью панели доступа.

Щелкнув плитку ScaleX Enterprise на панели доступа, вы автоматически войдете в приложение ScaleX Enterprise. См. дополнительные сведения о [панели доступа](../user-help/active-directory-saas-access-panel-introduction.md)


## <a name="additional-resources"></a>Дополнительные ресурсы

* [Список учебников по интеграции приложений SaaS с Azure Active Directory](tutorial-list.md)
* [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/scalexenterprise-tutorial/tutorial_general_01.png
[2]: ./media/scalexenterprise-tutorial/tutorial_general_02.png
[3]: ./media/scalexenterprise-tutorial/tutorial_general_03.png
[4]: ./media/scalexenterprise-tutorial/tutorial_general_04.png

[100]: ./media/scalexenterprise-tutorial/tutorial_general_100.png

[200]: ./media/scalexenterprise-tutorial/tutorial_general_200.png
[201]: ./media/scalexenterprise-tutorial/tutorial_general_201.png
[202]: ./media/scalexenterprise-tutorial/tutorial_general_202.png
[203]: ./media/scalexenterprise-tutorial/tutorial_general_203.png

