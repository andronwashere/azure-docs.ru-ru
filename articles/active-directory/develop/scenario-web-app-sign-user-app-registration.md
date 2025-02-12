---
title: Веб-приложение, которое поддерживает вход пользователей (регистрации приложения) — платформе Microsoft identity
description: Узнайте, как создать веб-приложение, которое поддерживает вход пользователей (регистрации приложения)
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/07/2019
ms.author: jmprieur
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 0ae638f8cbef29c5d167a3ab59188169cbd934ef
ms.sourcegitcommit: 6e6813f8e5fa1f6f4661a640a49dc4c864f8a6cb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/17/2019
ms.locfileid: "67150228"
---
# <a name="web-app-that-signs-in-users---app-registration"></a>Веб-приложение, которое поддерживает вход пользователей — Регистрация приложения

На этой странице объясняется особенности регистрации приложения для веб-приложения, где пользователи, впервые входящий в систему.

Чтобы зарегистрировать приложение, можно использовать:

- [Веб-приложение краткие руководства](#register-an-app-using-the-quickstarts) -помимо отличный первый опыт работы с создания приложения, краткие руководства на портале Azure содержат кнопки с именем **внесения этого изменения для меня**. Эта кнопка используется для задания свойств, которые требуется, даже для существующего приложения. Вам потребуется адаптировать значения этих свойств в собственных регистр. В частности web API URL-адрес для вашего приложения, вероятно, будет отличаться от предложенное по умолчанию, это отразится выхода URI.
- Портал Azure, чтобы [вручную зарегистрировать приложение](#register-an-app-using-azure-portal)
- PowerShell и средства командной строки

## <a name="register-an-app-using-the-quickstarts"></a>Регистрация приложения с помощью кратких руководств

При переходе на эту ссылку, можно создать загрузочный Создание веб-приложения:

- [ASP.NET Core](https://aka.ms/aspnetcore2-1-aad-quickstart-v2)
- [ASP.NET](https://ms.portal.azure.com/#blade/Microsoft_AAD_RegisteredApps/applicationsListBlade/quickStartType/AspNetWebAppQuickstartPage/sourceType/docs)

### <a name="register-an-app-using-azure-portal"></a>Регистрация приложения с помощью портала Azure

> [!NOTE]
> портал для использования другой в зависимости от того, если приложение выполняется в общедоступном облаке Microsoft Azure или в облачной службе национальные или независимых. Дополнительные сведения см. в разделе [национальных облаков](./authentication-national-cloud.md#app-registration-endpoints)

1. Войдите на [портал Azure](https://portal.azure.com) с помощью личной учетной записи Майкрософт либо рабочей или учебной учетной записи. Кроме того войдите в портал Azure по выбору в национальном облаке.
1. Если учетная запись предоставляет доступ к более чем одного клиента, выберите свою учетную запись в правом верхнем углу и задать сеанса работы с порталом для нужного Azure AD клиента.
1. В области навигации слева выберите **Azure Active Directory** службы, а затем выберите **регистрация приложений** > **Регистрация нового**.
1. После появления страницы **Регистрация приложения** введите сведения о регистрации приложения:
   1. Выберите типы поддерживаемых учетной записи для приложения (см. в разделе [типы счетов, поддерживаемые](./v2-supported-account-types.md))
   1. В разделе **Имя** введите понятное имя приложения, которое будет отображаться пользователям приложения, например `AspNetCore-WebApp`.
   1. В **URI перенаправления**добавьте тип приложения и URI назначения, которая будет принимать возвращаемых ответов маркеров после успешной проверки подлинности. Например, `https://localhost:44321/`.  Выберите **Зарегистрировать**.
1. Выберите меню **Проверка подлинности**, а затем добавьте следующие сведения:
   1. В **URL-адрес ответа**, добавьте `https://localhost:44321/signin-oidc`.
   1. В разделе **Дополнительные параметры** задайте для параметра **URL-адрес выхода** значение `https://localhost:44321/signout-oidc`.
   1. В разделе **Неявное предоставление** установите флажок **Токен идентификатора**.
   1. Щелкните **Сохранить**.

### <a name="register-an-app-using-powershell"></a>Регистрация приложения с помощью PowerShell

> [!NOTE]
> В настоящее время Azure AD PowerShell только создает приложения со следующими типами поддерживаемой учетной записи:
>
> - MyOrg (учетные записи в каталоге организации только)
> - AnyOrg (учетные записи в любой каталог организации).
>
> Если вы хотите создать приложение, выполняет вход пользователей с их личных учетных записей Майкрософт (например Скайп, XBox, Outlook.com), вы можете сначала создать приложение с несколькими владельцами (поддерживаемые типы учетных записей = учетные записи в любой каталог организации), а затем измените `signInAudience` свойства в манифесте приложения на портале Azure. Это объясняется в сведениях о на шаге [1.3](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/tree/master/1-WebApp-OIDC/1-3-AnyOrgOrPersonal#step-1-register-the-sample-with-your-azure-ad-tenant) учебника по ASP.NET Core (и можно обобщить до веб-приложений на любом языке).

## <a name="next-steps"></a>Дальнейшие действия

> [!div class="nextstepaction"]
> [Конфигурации кода приложения](scenario-web-app-sign-user-app-configuration.md)
