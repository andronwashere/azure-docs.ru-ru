---
title: Руководство по Настройка Cerner Central для автоматической подготовки пользователей с помощью Azure Active Directory | Документация Майкрософт
description: Узнайте, как настроить Azure Active Directory для автоматической подготовки пользователей и их добавления в список в Cerner Central.
services: active-directory
documentationcenter: ''
author: ArvindHarinder1
manager: CelesteDG
ms.assetid: d4ca2365-6729-48f7-bb7f-c0f5ffe740a3
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/27/2019
ms.author: arvinh
ms.collection: M365-identity-device-management
ms.openlocfilehash: 61e88e0fe7e6eec5b3cdfd03755a186744b77b47
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "65964201"
---
# <a name="tutorial-configure-cerner-central-for-automatic-user-provisioning"></a>Руководство по Настройка Cerner Central для автоматической подготовки пользователей

Цель этого руководства — показать, как настроить автоматическую подготовку и отзыв учетных записей пользователей Azure AD в списке пользователей в Cerner Central.

## <a name="prerequisites"></a>Технические условия

Сценарий, описанный в этом учебнике, предполагает, что у вас уже имеется:

* Клиент Azure Active Directory.
* Клиент Cerner Central.

> [!NOTE]
> Для интеграции Azure Active Directory с Cerner Central используется протокол [SCIM](http://www.simplecloud.info/).

## <a name="assigning-users-to-cerner-central"></a>Добавление пользователей в Cerner Central

В Azure Active Directory для определения того, какие пользователи должны получать доступ к выбранным приложениям, используется концепция, называемая "назначение". В контексте автоматической подготовки учетной записи синхронизированы будут только те пользователи и группы, которые были "назначены" приложению в Azure AD. 

Перед настройкой и включением службы подготовки необходимо решить, какие пользователи и (или) группы в Azure AD представляют пользователей, которым нужен доступ к Cerner Central. Когда этот вопрос будет решен, этих пользователей можно будет назначить приложению Cerner Central, следуя приведенным ниже указаниям.

[Назначение корпоративному приложению пользователя или группы](../manage-apps/assign-user-or-group-access-portal.md)

### <a name="important-tips-for-assigning-users-to-cerner-central"></a>Важные рекомендации по назначению пользователей приложению Cerner Central

* Рекомендуем сначала назначить приложению Cerner Central одного пользователя Azure AD и с его помощью протестировать конфигурацию подготовки. Дополнительные пользователи и/или группы можно назначить позднее.

* По завершении первоначального тестирования отдельного пользователя Cerner Central порекомендует назначить целый список пользователей для доступа к любому решению Cerner (не только Cerner Central), которых следует подготовить и добавить в список пользователей Cerner Central.  Другие решения Cerner используют этот перечень пользователей из списка пользователей.

* Назначая пользователя приложению Cerner Central, в диалоговом окне назначения необходимо выбрать роль **Пользователь**. Пользователи с ролью "Default Access" исключаются из подготовки.

## <a name="configuring-user-provisioning-to-cerner-central"></a>Настройка подготовки пользователей в Cerner Central

В этом разделе мы подключим Azure AD к списку пользователей Cerner Central с помощью API подготовки учетных записей Cerner (через протокол SCIM), а также настроим службу подготовки для создания, изменения и отключения назначенных учетных записей пользователей в Cerner Central на основе назначения пользователей и групп в Azure AD.

> [!TIP]
> Кроме того, можно включить на основе SAML единого входа, для Cerner Central, следуйте инструкциям, указанным в [портала Azure](https://portal.azure.com). Единый вход можно настроить независимо от автоматической подготовки, хотя две эти функции дополняют друг друга. Дополнительные сведения см. в [руководстве по настройке единого входа в Cerner Central](cernercentral-tutorial.md).

### <a name="to-configure-automatic-user-account-provisioning-to-cerner-central-in-azure-ad"></a>Настройка автоматической подготовки учетных записей пользователей Azure AD в Cerner Central

Для подготовки учетных записей пользователей в Cerner Central потребуется запросить у Cerner системную учетную запись Cerner Central и создать токен носителя OAuth, который Azure AD сможет использовать для подключения к конечной точке SCIM Cerner. Также перед развертыванием в рабочей среде рекомендуется выполнить интеграцию в песочнице Cerner.

1. Сначала убедитесь, что у пользователей, управляющих интеграцией Cerner и Azure AD, есть учетная запись CernerCare. Она требуется для доступа к документации, необходимой для выполнения инструкций. При необходимости воспользуйтесь указанным ниже URL-адресом, чтобы создавать учетные записи CernerCare в любой применимой среде.

   * Песочница: https://sandboxcernercare.com/accounts/create

   * Производство: https://cernercare.com/accounts/create  

2. Далее создайте для Azure AD системную учетную запись. Выполните инструкции ниже, чтобы отправить запрос на получение системной учетной записи для песочницы и рабочей среды.

   * Указания: https://wiki.ucern.com/display/CernerCentral/Requesting+A+System+Account

   * Песочница: https://sandboxcernercentral.com/system-accounts/

   * Производство: https://cernercentral.com/system-accounts/

3. Далее создайте токен носителя OAuth для каждой системной учетной записи. Для этого выполните приведенные ниже инструкции.

   * Указания: https://wiki.ucern.com/display/public/reference/Accessing+Cerner%27s+Web+Services+Using+A+System+Account+Bearer+Token

   * Песочница: https://sandboxcernercentral.com/system-accounts/

   * Производство: https://cernercentral.com/system-accounts/

4. Наконец, необходимо получить идентификаторы области списка пользователей для песочницы и рабочей среды в Cerner, чтобы завершить настройку. Дополнительные сведения о том, как их получить, см. в статье https://wiki.ucern.com/display/public/reference/Publishing+Identity+Data+Using+SCIM. 

5. Теперь можно настроить Azure AD для подготовки учетных записей пользователей в Cerner. Войдите на [портал Azure](https://portal.azure.com) и перейдите в раздел **Azure Active Directory > Корпоративные приложения > Все приложения**.

6. Если в Cerner Central уже настроен единый вход, найдите свой экземпляр Cerner Central с помощью поля поиска. В противном случае щелкните **Добавить** и выполните поиск **Cerner Central** в коллекции приложений. Выберите Cerner Central в результатах поиска и добавьте его в свой список приложений.

7. Выберите экземпляр Cerner Central и перейдите на вкладку **Подготовка**.

8. Для параметра **Режим подготовки к работе** выберите значение **Automatic** (Автоматически).

   ![Подготовка Cerner Central](./media/cernercentral-provisioning-tutorial/Cerner.PNG)

9. Заполните следующие поля в разделе **Учетные данные администратора**.

   * В поле **URL-адрес клиента** введите URL-адрес в формате, указанном ниже, заменив User-Roster-Realm-ID идентификатором области, полученным на шаге 4.

    > Песочница: https://user-roster-api.sandboxcernercentral.com/scim/v1/Realms/User-Roster-Realm-ID/ 
    > 
    > Производство: https://user-roster-api.cernercentral.com/scim/v1/Realms/User-Roster-Realm-ID/ 

   * В поле **Секретный токен** введите токен носителя OAuth, созданный на шаге 3, и нажмите кнопку **Проверить подключение**.

   * В правом верхнем углу портала должно отобразиться оповещение об успешной проверке подключения.

1. В поле **Почтовое уведомление** введите адрес электронной почты пользователя или группы, которые должны получать уведомления об ошибках подготовки, а также установите флажок ниже.

1. Выберите команду **Сохранить**.

1. В разделе **Сопоставления атрибутов** просмотрите атрибуты пользователей и групп Azure AD, которые будут синхронизированы с Cerner Central. Атрибуты, которые выбраны в качестве свойств **Matching**, используются для сопоставления учетных записей пользователей и групп в Cerner Central для операций обновления. Нажмите кнопку "Сохранить", чтобы подтвердить все изменения.

1. Чтобы включить службу подготовки Azure AD для Cerner Central, в разделе **Параметры** установите переключатель **Состояние подготовки** в положение **Включено**.

1. Выберите команду **Сохранить**.

После этого будет запущена начальная синхронизация всех пользователей и (или) групп, назначенных приложению Cerner Central в разделе "Пользователи и группы". Начальная синхронизация занимает больше времени, чем последующие операции синхронизации. Если служба запущена, они выполняются примерно каждые 40 минут. В разделе **Сведения о синхронизации** можно отслеживать ход синхронизации и с помощью ссылок просматривать журналы действий по подготовке, в которых описаны все действия, выполняемые службой подготовки в отношении приложения Cerner Central.

Дополнительные сведения о чтении журналов подготовки Azure AD см. в руководстве по [отчетам об автоматической подготовке учетных записей](../manage-apps/check-status-user-account-provisioning.md).

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Cerner Central: Publishing identity data using Azure AD](https://wiki.ucern.com/display/public/reference/Publishing+Identity+Data+Using+Azure+AD) (Cerner Central: публикация данных удостоверений с помощью Azure Active Directory)
* [Учебник. Интеграция Azure Active Directory с Cerner Central](cernercentral-tutorial.md)
* [Управление подготовкой учетных записей пользователей для корпоративных приложений](../manage-apps/configure-automatic-user-provisioning-portal.md)
* [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Дальнейшие действия

* [Сведения о просмотре журналов и получении отчетов о действиях по подготовке](https://docs.microsoft.com/azure/active-directory/active-directory-saas-provisioning-reporting).
