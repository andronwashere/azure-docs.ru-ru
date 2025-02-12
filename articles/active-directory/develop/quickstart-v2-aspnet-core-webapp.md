---
title: Краткое руководство. Использование веб-приложения ASP.NET Core с платформой удостоверений Майкрософт | Azure
description: Узнайте, как реализовать единый вход Майкрософт в веб-приложении ASP.NET Core с помощью OpenID Connect.
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
editor: ''
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/11/2019
ms.author: jmprieur
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4723b224d61b2ccc2b563150befa5ea2d33453ad
ms.sourcegitcommit: e9c866e9dad4588f3a361ca6e2888aeef208fc35
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/19/2019
ms.locfileid: "68335615"
---
# <a name="quickstart-add-sign-in-with-microsoft-to-an-aspnet-core-web-app"></a>Краткое руководство. Добавление возможности входа в веб-приложение ASP.NET Core с помощью учетной записи Майкрософт

[!INCLUDE [active-directory-develop-applies-v2](../../../includes/active-directory-develop-applies-v2.md)]

В этом кратком руководстве вы узнаете, как веб-приложение ASP.NET Core позволяет войти в личные учетные записи (hotmail.com, outlook.com и т. д.), а также рабочие и учебные учетные записи из любого экземпляра Azure Active Directory (Azure AD).

![Схема работы приложения, создаваемого в этом кратком руководстве](media/quickstart-v2-aspnet-core-webapp/aspnetcorewebapp-intro.svg)

> [!div renderon="docs"]
> ## <a name="register-and-download-your-quickstart-app"></a>Регистрация и скачивание приложения, используемого в этом кратком руководстве
> У вас есть два варианта запуска приложения, используемого в этом кратком руководстве:
> * [Экспресс-способ] [Вариант 1. Регистрация и автоматическая настройка приложения, а затем скачивание примера кода](#option-1-register-and-auto-configure-your-app-and-then-download-your-code-sample)
> * [Вручную] [Вариант 2. Регистрация и настройка приложения и примера кода вручную](#option-2-register-and-manually-configure-your-application-and-code-sample)
>
> ### <a name="option-1-register-and-auto-configure-your-app-and-then-download-your-code-sample"></a>Вариант 1. Регистрация и автоматическая настройка приложения, а затем скачивание примера кода
>
> 1. Откройте [Регистрация приложений](https://aka.ms/aspnetcore2-1-aad-quickstart-v2) на портале Azure.
> 1. Введите имя приложения и нажмите кнопку **Зарегистрировать**.
> 1. Следуйте инструкциям для загрузки и автоматической настройки нового приложения одним щелчком мыши.
>
> ### <a name="option-2-register-and-manually-configure-your-application-and-code-sample"></a>Вариант 2. Регистрация и настройка приложения и примера кода вручную
>
> #### <a name="step-1-register-your-application"></a>Шаг 1. Регистрация приложения
> Чтобы зарегистрировать приложение и добавить сведения о его регистрации в решение вручную, сделайте следующее:
>
> 1. Войдите на [портал Azure](https://portal.azure.com) с помощью личной учетной записи Майкрософт либо рабочей или учебной учетной записи.
> 1. Если учетная запись предоставляет доступ нескольким клиентам, выберите свою учетную запись в правом верхнем углу и нужный клиент Azure AD для этого сеанса портала.
> 1. Перейдите на страницу [Регистрация приложений](https://go.microsoft.com/fwlink/?linkid=2083908) Платформы удостоверений Майкрософт для разработчиков.
> 1. Выберите **Новая регистрация**.
> 1. После появления страницы **Регистрация приложения** введите сведения о регистрации приложения:
>    - В разделе **Имя** введите понятное имя приложения, которое будет отображаться пользователям приложения, например `AspNetCore-Quickstart`.
>    - В поле **URI перенаправления** добавьте `https://localhost:44321/` и выберите **Зарегистрировать**.
> 1. Выберите меню **Проверка подлинности**, а затем добавьте следующие сведения:
>    - В поле **URI перенаправления** добавьте `https://localhost:44321/signin-oidc` и выберите **Сохранить**.
>    - В разделе **Дополнительные параметры** задайте для параметра **URL-адрес выхода** значение `https://localhost:44321/signout-oidc`.
>    - В разделе **Неявное предоставление** установите флажок **Токен идентификатора**.
>    - Щелкните **Сохранить**.

> [!div class="sxs-lookup" renderon="portal"]
> #### <a name="step-1-configure-your-application-in-the-azure-portal"></a>Шаг 1. Настройка приложения на портале Azure
> Чтобы пример кода для этого краткого руководства работал, необходимо добавить URL-адреса ответа `https://localhost:44321/` и `https://localhost:44321/signin-oidc`, URL-адрес выхода `https://localhost:44321/signout-oidc` и запросить выдачу токенов идентификации конечной точкой авторизации.
> > [!div renderon="portal" id="makechanges" class="nextstepaction"]
> > [Внести это изменение для меня]()
>
> > [!div id="appconfigured" class="alert alert-info"]
> > ![Уже настроено](media/quickstart-v2-aspnet-webapp/green-check.png). Ваше приложение настроено с помощью этих атрибутов.

#### <a name="step-2-download-your-aspnet-core-project"></a>Шаг 2. Скачивание проекта ASP.NET Core

- [Скачайте решение Visual Studio 2019](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/archive/aspnetcore2-2.zip)

#### <a name="step-3-configure-your-visual-studio-project"></a>Шаг 3. Настройка проекта Visual Studio

1. Извлеките ZIP-файл в локальную папку, расположенную в корневой папке (например, **C:\Azure-Samples**).
1. Если вы используете Visual Studio 2019, откройте решение в Visual Studio (необязательно).
1. Измените файл **appsettings.json**. Найти `ClientId` и измените значение `ClientId` на **идентификатор зарегистрированного приложения (клиента)** . 

    ```json
    "ClientId": "Enter_the_Application_Id_here"
    "TenantId": "Enter_the_Tenant_Info_Here"
    ```

> [!div class="sxs-lookup" renderon="portal"]
> > [!NOTE]
> > В этом кратком руководстве поддерживается Enter_the_Supported_Account_Info_Here.

> [!div renderon="docs"]
> Описание
> - `Enter_the_Application_Id_here` — **идентификатор приложения (клиента)** , зарегистрированного на портале Azure. **Идентификатор приложения (клиента)** можно найти на странице **Обзор** приложения.
> - `Enter_the_Tenant_Info_Here` — один из следующих вариантов:
>   - Если приложение поддерживает **учетные записи только в этом каталоге организации**, замените это значение **идентификатором клиента** или **именем клиента** (например, contoso.microsoft.com).
>   - Если ваше приложение поддерживает вариант **Учетные записи в любом каталоге организации**, замените это значение на `organizations`.
>   - Если приложение поддерживает вариант **Все пользователи с учетными записями Майкрософт**, замените это значение на `common`.
>
> > [!TIP]
> > Чтобы найти значения параметров **Идентификатор приложения (клиента)** , **Идентификатор каталога (клиента)** и **Поддерживаемые типы учетных записей**, на портале Azure перейдите на страницу **Обзор**.

## <a name="more-information"></a>Дополнительные сведения

В этом разделе представлен код, используемый для выполнения входа пользователей. Это может быть полезно для рассмотрения принципов работы кода и основных аргументов. Также вы поймете, нужно ли добавлять функцию входа в существующее приложение ASP.NET Core.

### <a name="startup-class"></a>Класс Startup

ПО промежуточного слоя *Microsoft.AspNetCore.Authentication* использует класс Startup, который выполняется при инициализации хост-процесса:

```csharp
public void ConfigureServices(IServiceCollection services)
{
  services.Configure<CookiePolicyOptions>(options =>
  {
    // This lambda determines whether user consent for non-essential cookies is needed for a given request.
    options.CheckConsentNeeded = context => true;
    options.MinimumSameSitePolicy = SameSiteMode.None;
  });

  services.AddAuthentication(AzureADDefaults.AuthenticationScheme)
          .AddAzureAD(options => Configuration.Bind("AzureAd", options));

  services.Configure<OpenIdConnectOptions>(AzureADDefaults.OpenIdScheme, options =>
  {
    options.Authority = options.Authority + "/v2.0/";         // Microsoft identity platform

    options.TokenValidationParameters.ValidateIssuer = false; // accept several tenants (here simplified)
  });

  services.AddMvc(options =>
  {
     var policy = new AuthorizationPolicyBuilder()
                     .RequireAuthenticatedUser()
                     .Build();
     options.Filters.Add(new AuthorizeFilter(policy));
  })
  .SetCompatibilityVersion(CompatibilityVersion.Version_2_1);
}
```

Метод `AddAuthentication` настраивает службу для добавления аутентификации на основе файлов cookie, которая используется при работе с браузером, и определения запроса для OpenID Connect. 

Строка, содержащая `.AddAzureAd`, добавляет в ваше приложение функцию аутентификации платформы удостоверений Майкрософт. Затем выполняется настройка для входа с помощью конечной точки платформы удостоверений Майкрософт.

> |Where  |  |
> |---------|---------|
> | ClientId  | Идентификатор приложения (клиента), зарегистрированного на портале Azure. |
> | Authority | Конечная точка службы токенов безопасности для проверки подлинности пользователей. Обычно это <https://login.microsoftonline.com/{tenant}/v2.0> для общедоступного облака, где {tenant} — имя вашего клиента, идентификатор клиента или *common* для ссылки на общую конечную точку (используется для мультитенантных приложений). |
> | TokenValidationParameters | Список параметров для проверки маркеров. В этом случае для `ValidateIssuer` задано значение `false`, чтобы указать, что приложение может принимать операции входа с любых типов личных, рабочих или учебных учетных записей. |


> [!NOTE]
> Параметр `ValidateIssuer = false` приводится в этом кратком руководстве для упрощения. В реальных приложениях необходимо проверить издателя.
> См. примеры кода, чтобы узнать, как это сделать.

### <a name="protect-a-controller-or-a-controllers-method"></a>Защита контроллера или метода контроллера

Контроллер или его методы можно защитить с помощью атрибута `[Authorize]`. Этот атрибут разрешает доступ к контроллеру или методам только аутентифицированным пользователям. Это значит, что для доступа к контроллеру можно создавать запрос на аутентификацию, если пользователь не прошел аутентификацию.

[!INCLUDE [Help and support](../../../includes/active-directory-develop-help-support-include.md)]

## <a name="next-steps"></a>Дополнительная информация

Извлеките репозиторий GitHub для работы с этим руководством по ASP.NET Core. Вы узнаете, как добавлять аутентификацию для нового веб-приложения ASP.NET Core, вызывать API Microsoft Graph и другие API Майкрософт, вызывать собственные API, добавлять авторизацию, реализовать вход пользователей в национальных облаках или с использованием удостоверений социальных сетей и многое другое:

> [!div class="nextstepaction"]
> [Руководство. Веб-приложение ASP.NET Core](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/)

Помогите нам улучшить платформу Microsoft Identity. Поделитесь своим мнением, ответив на два вопроса.

> [!div class="nextstepaction"]
> [Опрос по платформе удостоверений Майкрософт](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbRyKrNDMV_xBIiPGgSvnbQZdUQjFIUUFGUE1SMEVFTkdaVU5YT0EyOEtJVi4u)