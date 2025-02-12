---
title: Управление аутентификацией в Azure Maps | Документация Майкрософт
description: Вы можете управлять аутентификацией в Azure Maps с помощью портала Azure.
author: walsehgal
ms.author: v-musehg
ms.date: 07/11/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.openlocfilehash: 1dc63a2c2350ad4f1d02d0c1b22050293d7b866c
ms.sourcegitcommit: 64798b4f722623ea2bb53b374fb95e8d2b679318
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/11/2019
ms.locfileid: "67837816"
---
# <a name="manage-authentication-in-azure-maps"></a>Управление аутентификацией в Azure Maps

После создания учетной записи "карты Azure", идентификатор клиента и ключи создаются для поддержки проверки подлинности Shared Key или Azure Active Directory (Azure AD).

## <a name="view-authentication-details"></a>Просмотр сведений об аутентификации

Проверки подлинности можно просмотреть на портале Azure. Перейдите к своей учетной записи и выберите **проверки подлинности** на **параметры** меню.

![Подробные сведения о проверке подлинности](./media/how-to-manage-authentication/how-to-view-auth.png)

 Дополнительные сведения см. в разделе [проверки подлинности с помощью Azure Maps](https://aka.ms/amauth).


## <a name="set-up-azure-ad-app-registration"></a>Настройка регистрации приложения Azure AD

После создания учетной записи службы карт Azure, необходимо установить связь между клиентом Azure AD и ресурсов службы карт Azure.

1. Перейдите к колонке Azure AD и создать регистрацию. Укажите имя для регистрации. В **URL-адрес входа** укажите на домашнюю страницу веб-приложение или API (например, https:\//localhost/). Если у вас уже есть зарегистрированное приложение, перейдите к шагу 2.

    ![Регистрация приложения](./media/how-to-manage-authentication/app-registration.png)

    ![Сведения о регистрации приложения](./media/how-to-manage-authentication/app-create.png)

2. Чтобы назначить делегированные разрешения API для службы карт Azure, перейдите в приложение в разделе **регистрация приложений**, а затем выберите **параметры**.  Выберите **необходимые разрешения**, а затем выберите **добавить**. Найдите и выберите **"карты Azure"** под **Выбор API**, а затем выберите **выберите** кнопки.

    ![Разрешения приложения API](./media/how-to-manage-authentication/app-permissions.png)

3. В разделе **Выбор разрешений**выберите **доступа службы карт Azure**и выберите **выберите** кнопки.

    ![Выберите разрешения для приложения API](./media/how-to-manage-authentication/select-app-permissions.png)

4. Выполните шаг или b, в зависимости от метода проверки подлинности.

    1. Если приложение использует проверку подлинности токена пользователя с SDK веб-карты Azure, включите `oauthEnableImplicitFlow` присвоив ему значение true в разделе манифеста странице сведений регистрации приложения.
    
       ![Манифест приложения](./media/how-to-manage-authentication/app-manifest.png)

    2. Если приложение использует проверку подлинности сервера и приложения, перейдите на страницу **ключи** колонке в регистрации приложения и создайте пароль или отправить сертификат открытого ключа для регистрации приложения. Если создать пароль, после выбора **Сохранить**, скопируйте пароль для использования в будущем и сохраните в безопасном месте. Этот пароль будет использоваться для получения маркеров из Azure AD.

       ![Ключи приложения](./media/how-to-manage-authentication/app-keys.png)


## <a name="grant-rbac-to-azure-maps"></a>Предоставление RBAC для Azure Maps

После учетная запись Azure Maps связывается с клиентом Azure AD, можно предоставить доступ, назначив пользователя, группы или приложения один или несколько ролей управления доступом "карты Azure".

1. Перейдите к **управление доступом (IAM)** выберите **назначения ролей**, а затем выберите **добавить назначение роли**.

    ![Предоставление RBAC](./media/how-to-manage-authentication/how-to-grant-rbac.png)

2. В **добавить назначение роли** окна в разделе **роли**выберите **Azure Maps дату чтения (Предварительная версия)** . В поле **Назначение доступа к** выберите **Пользователь, группа или субъект-служба Azure AD**. В разделе **выберите**, выберите пользователя или приложения. Щелкните **Сохранить**.

    ![Добавление назначения роли](./media/how-to-manage-authentication/add-role-assignment.png)

## <a name="view-available-azure-maps-rbac-roles"></a>Просмотр доступных ролей RBAC Azure Maps

Чтобы просмотреть роли управления ДОСТУПОМ доступа на основе ролей, доступных для службы карт Azure, перейдите к **управление доступом (IAM)** выберите **ролей**, и затем поиск ролей начиная с **"картыAzure"** . Это роли, которые можно предоставить доступ.

![Просмотр доступных ролей](./media/how-to-manage-authentication/how-to-view-avail-roles.png)


## <a name="view-azure-maps-rbac"></a>Просмотр карты Azure RBAC

RBAC предоставляет детальный контроль доступа.

Для просмотра пользователей и приложений, которые были предоставлены RBAC для службы карт Azure, перейдите на **управление доступом (IAM)** выберите **назначения ролей**, а затем отфильтровать по **"карты Azure"** .

![Просмотр пользователей и приложений предоставлены RBAC](./media/how-to-manage-authentication/how-to-view-amrbac.png)


## <a name="request-tokens-for-azure-maps"></a>Запрос маркеров для Azure Maps

После регистрации приложения и связали ее с помощью Azure Maps, вы можете запросить маркеры доступа.

* Если приложение использует проверку подлинности токена пользователя с SDK веб-карты Azure, необходимо настроить страницу HTML с Идентификатором клиента "карты Azure" и идентификатор приложения Azure AD.

* Если приложение использует проверку подлинности сервера и приложения, необходимо запросить маркер из конечной точки входа Azure AD `https://login.microsoftonline.com` с Идентификатором ресурса Azure AD `https://atlas.microsoft.com/`, идентификатор клиента "карты Azure", идентификатор приложения Azure AD и регистрации приложения Azure AD пароль или сертификат.

Дополнительные сведения о запросе маркеров доступа из Azure AD для пользователей и субъекты-службы, см. в разделе [сценарии аутентификации в Azure AD](https://docs.microsoft.com/azure/active-directory/develop/authentication-scenarios).


## <a name="next-steps"></a>Следующие шаги

Дополнительные сведения об аутентификации Azure AD и веб-пакете SDK Azure Maps см. в статье [Использование библиотеки Map Control в службе Azure Maps](https://docs.microsoft.com/azure/azure-maps/how-to-use-map-control).

Узнайте, как просматривать метрики использования API для учетной записи Azure Maps:
> [!div class="nextstepaction"] 
> [Просмотр метрик использования](how-to-view-api-usage.md)