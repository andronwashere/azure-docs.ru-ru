---
title: Изменения, вносимые в проект WebAPI при подключении к Azure AD
description: В этой статье описывается, что происходит с проектом WebAPI при подключении к Azure AD с помощью Visual Studio.
services: active-directory
author: ghogen
manager: douge
ms.service: active-directory
ms.subservice: develop
ms.assetid: 57630aee-26a2-4326-9dbb-ea2a66daa8b0
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.workload: azure-vs
ms.topic: conceptual
ms.date: 03/12/2018
ms.author: ghogen
ms.custom: aaddev, vs-azure
ms.collection: M365-identity-device-management
ms.openlocfilehash: 19a36ca4b194d8031b4a263a092ecb52be74cdd0
ms.sourcegitcommit: 4b431e86e47b6feb8ac6b61487f910c17a55d121
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/18/2019
ms.locfileid: "68324274"
---
# <a name="what-happened-to-my-webapi-project-visual-studio-azure-active-directory-connected-service"></a>Что произошло с моим проектом WebApi в подключенной службе Visual Studio Azure Active Directory

> [!div class="op_single_selector"]
> - [Начало работы](vs-active-directory-webapi-getting-started.md)
> - [Что произошло?](vs-active-directory-webapi-what-happened.md)

В этой статье описаны конкретные изменения, которые вносятся в проекты ASP.NET WebAPI, одностраничного приложения ASP.NET и API-интерфейса Azure ASP.NET при добавлении [подключенной службы Azure Active Directory с помощью Visual Studio](vs-active-directory-add-connected-service.md). Это также касается и проектов мобильной службы Azure ASP.NET в Visual Studio 2015.

Сведения о работе с подключенной службой см. в статье [Начало работы с Azure Active Directory и подключенными службами Visual Studio (проекты WebApi)](vs-active-directory-webapi-getting-started.md).

## <a name="added-references"></a>Добавленные ссылки

Это изменение влияет на файлы проекта (ссылки \*.NET) и `packages.config` (ссылки NuGet).

| Тип | Ссылка |
| --- | --- |
| .NET; NuGet | Microsoft.Owin |
| .NET; NuGet | Microsoft.Owin.Host.SystemWeb |
| .NET; NuGet | Microsoft.Owin.Security |
| .NET; NuGet | Microsoft.Owin.Security.ActiveDirectory |
| .NET; NuGet | Microsoft.Owin.Security.Jwt |
| .NET; NuGet | Microsoft.Owin.Security.OAuth |
| .NET; NuGet | Owin |
| .NET; NuGet | System.IdentityModel.Tokens.Jwt |

Дополнительные ссылки, если выбран параметр **Чтение данных каталога**

| Тип | Ссылка |
| --- | --- |
| .NET; NuGet | EntityFramework |
| .NET        | EntityFramework.SqlServer (только для Visual Studio 2015) |
| .NET; NuGet | Microsoft.Azure.ActiveDirectory.GraphClient |
| .NET; NuGet | Microsoft.Data.Edm |
| .NET; NuGet | Microsoft.Data.OData |
| .NET; NuGet | Microsoft.Data.Services.Client |
| .NET; NuGet | Microsoft.IdentityModel.Clients.ActiveDirectory; |
| .NET        | Microsoft.IdentityModel.Clients.ActiveDirectory.WindowsForms<br>(только для Visual Studio 2015) |
| .NET; NuGet | System.Spatial |

Следующие ссылки удалены (только для проектов ASP.NET 4, например в Visual Studio 2015):

| Type | Ссылка |
| --- | --- |
| .NET; NuGet | Microsoft.AspNet.Identity.Core |
| .NET; NuGet | Microsoft.AspNet.Identity.EntityFramework |
| .NET; NuGet | Microsoft.AspNet.Identity.Owin |

## <a name="project-file-changes"></a>Изменения в файле проекта

- Для свойства `IISExpressSSLPort` установлено конкретное число.
- Для свойства `WebProject_DirectoryAccessLevelKey` установлено значение 0 (или 1, если выбран параметр **Чтение данных каталога**).
- Для свойства `IISUrl` установлено значение `https://localhost:<port>/`, где `<port>` соответствует значению `IISExpressSSLPort`.

## <a name="webconfig-or-appconfig-changes"></a>Изменения в файлах web.config или app.config

- Добавлены следующие записи конфигурации:

    ```xml
    <appSettings>
        <add key="ida:ClientId" value="<ClientId from the new Azure AD app>" />
        <add key="ida:Tenant" value="<your selected Azure domain>" />
        <add key="ida:Audience" value="<your selected domain + / + project name>" />
    </appSettings>
    ```

- Только для Visual Studio 2017. В разделе `<appSettings>` добавлена следующая запись:

    ```xml
    <add key="ida:MetadataAddress" value="<domain URL + /federationmetadata/2007-06/federationmetadata.xml>" />
    ```

- Добавлены элементы `<dependentAssembly>` в узел `<runtime><assemblyBinding>` для `System.IdentityModel.Tokens.Jwt`.

- Если вы выбрали параметр **Чтение данных каталога**, в разделе `<appSettings>` добавлена следующая запись конфигурации:

    ```xml
    <add key="ida:Password" value="<Your Azure AD app's new password>" />
    ```

## <a name="code-changes-and-additions"></a>Изменения и дополнения в коде

- Добавлен атрибут `[Authorize]` в `Controllers/ValueController.cs` и все остальные существующие контроллеры.

- Добавлен или изменен класс запуска `App_Start/Startup.Auth.cs` для аутентификации, который содержит логику запуска для аутентификации Azure AD. Если вы выбрали параметр **Чтение данных каталога**, этот файл содержит еще и код для получения кода OAuth и его обмена на маркер доступа.

- (Только для Visual Studio 2015 с приложением ASP.NET 4) Удален класс `App_Start/IdentityConfig.cs` и добавлены `Controllers/AccountController.cs`, `Models/IdentityModel.cs` и `Providers/ApplicationAuthProvider.cs`.

- Добавлен файл `Connected Services/AzureAD/ConnectedService.json` (для Visual Studio 2017) или `Service References/Azure AD/ConnectedService.json` (для Visual Studio 2015), который содержит сведения, позволяющие Visual Studio отслеживать добавление подключенной службы.

### <a name="file-backup-visual-studio-2015"></a>Резервные копии файлов (для Visual Studio 2015)

При добавлении подключенной службы Visual Studio 2015 создает резервные копии всех измененных и удаленных файлов. Все эти файлы сохраняются в папке `Backup/AzureAD`. Visual Studio 2017 не создает резервные копии.

- `Startup.cs`
- `App_Start\IdentityConfig.cs`
- `App_Start\Startup.Auth.cs`
- `Controllers\AccountController.cs`
- `Controllers\ManageController.cs`
- `Models\IdentityModels.cs`
- `Models\ApplicationOAuthProvider.cs`

## <a name="changes-on-azure"></a>Изменения в Azure

- Создано приложение Azure AD в домене, который вы выбрали при добавлении подключенной службы.
- В приложение добавлены разрешения на **чтение данных каталога**, если вы выбрали соответствующий параметр.

[Подробнее об Azure Active Directory](https://azure.microsoft.com/services/active-directory/).

## <a name="next-steps"></a>Следующие шаги

- [Сценарии аутентификации в Azure Active Directory](authentication-scenarios.md)
- [Добавление возможности входа в веб-приложение ASP.NET с помощью учетной записи Майкрософт](quickstart-v1-aspnet-webapp.md)
