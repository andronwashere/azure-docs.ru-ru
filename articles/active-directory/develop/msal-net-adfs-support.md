---
title: Поддержка AD FS в библиотеке проверки подлинности Майкрософт для .NET | Службы
description: Сведения о поддержке службы федерации Active Directory (AD FS) (AD FS) в библиотеке проверки подлинности Майкрософт для .NET (MSAL.NET).
services: active-directory
documentationcenter: dev-center-name
author: rwike77
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/16/2019
ms.author: jmprieur
ms.reviewer: saeeda
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 13e1f80f3a0a10466ead60d828d28aa9916fe26b
ms.sourcegitcommit: af58483a9c574a10edc546f2737939a93af87b73
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/17/2019
ms.locfileid: "68302689"
---
# <a name="active-directory-federation-services-support-in-msalnet"></a>Поддержка службы федерации Active Directory (AD FS) в MSAL.NET
Службы федерации Active Directory (AD FS) (AD FS) в Windows Server позволяет добавлять аутентификацию и авторизацию на основе OpenID Connect Connect и OAuth 2,0 для разрабатываемых приложений. Эти приложения могут выполнять проверку подлинности пользователей непосредственно в AD FS. Дополнительные сведения см. в статье [сценарии AD FS для разработчиков](/windows-server/identity/ad-fs/overview/ad-fs-scenarios-for-developers).

Библиотека проверки подлинности Microsoft для .NET (MSAL.NET) поддерживает два сценария проверки подлинности в AD FS.

- MSAL.NET взаимодействует с Azure Active Directory, который сам является *федеративным* с AD FS.
- MSAL.NET **напрямую** обращается к центру ADFS. Это поддерживается только в AD FS 2019 и более поздних версиях. В одном из сценариев это относится [Azure Stack](https://azure.microsoft.com/overview/azure-stack/) поддержки.


## <a name="msal-connects-to-azure-ad-which-is-federated-with-ad-fs"></a>MSAL подключается к Azure AD, который является федеративным с AD FS
MSAL.NET поддерживает подключение к Azure AD, которое подписывает управляемых пользователей (пользователей, управляемых в Azure AD) или федеративных пользователей (пользователей, управляемых другим поставщиком удостоверений, например AD FS). MSAL.NET не знает о том, что пользователи являются федеративными. По мере того, как это касается, он взаимодействует с Azure AD.

В этом случае в качестве [центра](msal-client-application-configuration.md#authority) используется обычный центр (имя узла центра сертификации + клиент, общие или организации).

### <a name="acquiring-a-token-interactively"></a>Интерактивное получение маркера
При вызове `AcquireTokenInteractive` метода взаимодействие с пользователем обычно выполняется следующим образом:

1. Пользователь вводит идентификатор своей учетной записи.
2. В Azure AD отображается краткое сообщение "идет переход на страницу вашей организации".
3. Пользователь перенаправляется на страницу входа поставщика удостоверений. Страница входа обычно настраивается с помощью логотипа Организации.

Поддерживаемые AD FS версии в этом объединенном сценарии: AD FS v2, AD FS v3 (Windows Server 2012 R2) и AD FS V4 (AD FS 2016).

### <a name="acquiring-a-token-using-acquiretokenbyintegratedauthentication-or-acquiretokenbyusernamepassword"></a>Получение маркера с помощью Аккуиретокенбинтегратедаусентикатион или Аккуиретокенбюсернамепассворд
При получении маркера с помощью `AcquireTokenByIntegratedAuthentication` методов или `AcquireTokenByUsernamePassword` MSAL.NET получает поставщик удостоверений для связи на основе имени пользователя.  MSAL.NET получает [токен SAML 1,1](reference-saml-tokens.md) после обращения к поставщику удостоверений.  Затем MSAL.NET предоставляет маркер SAML для Azure AD в качестве пользовательского утверждения (аналогично потоку "от [имени](msal-authentication-flows.md#on-behalf-of)") для возврата JWT.

## <a name="msal-connects-directly-to-ad-fs"></a>MSAL напрямую подключается к AD FS
MSAL.NET поддерживает подключение к AD FS 2019, что соответствует требованиям Open ID Connect и понимает PKCE и области. Для этой поддержки требуется, чтобы к Windows Server был применен пакет обновления [KB 4490481](https://support.microsoft.com/en-us/help/4490481/windows-10-update-kb4490481) . При прямом подключении к AD FS центр, который вы хотите использовать для создания приложения, аналогичен `https://mysite.contoso.com/adfs/`.

В настоящее время нет планов для поддержки прямого подключения к:

- AD FS 16, так как он не поддерживает PKCE и по-прежнему использует ресурсы, а не область
- AD FS версии 2, которая не соответствует требованиям OIDC.

 Если требуется поддержка сценариев, требующих прямого подключения к AD FS 2016, используйте последнюю версию [библиотеки проверки Подлинности Azure Active Directory](active-directory-authentication-libraries.md#microsoft-supported-client-libraries). После обновления локальной системы до AD FS 2019 вы сможете использовать MSAL.NET.

## <a name="see-also"></a>См. также

Сведения об объединенном варианте см. в статье [Настройка поведения при входе Azure Active Directory для приложения с помощью политики обнаружения домашней области](https://docs.microsoft.com/azure/active-directory/manage-apps/configure-authentication-for-federated-users-portal) .
