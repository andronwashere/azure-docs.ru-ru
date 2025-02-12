---
title: Конфигурация клиентского приложения (Библиотека проверки подлинности Майкрософт) | Службы
description: Сведения о параметрах конфигурации для общедоступных клиентов и конфиденциальных клиентских приложений в библиотеке проверки подлинности Майкрософт (MSAL).
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
ms.author: ryanwi
ms.reviewer: saeeda
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 767f7362a6c46d864ba17f23f6506bf6cdb71414
ms.sourcegitcommit: 770b060438122f090ab90d81e3ff2f023455213b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/17/2019
ms.locfileid: "68304729"
---
# <a name="application-configuration-options"></a>Параметры конфигурации приложения

В коде вы Инициализирует новое общедоступное или конфиденциальное клиентское приложение (или пользовательский агент для MSAL. js) для проверки подлинности и получения маркеров. При инициализации клиентского приложения в библиотеке проверки подлинности Майкрософт (MSAL) можно задать ряд параметров конфигурации. Эти параметры делятся на две группы:

- Параметры регистрации, включая:
    - [Центр сертификации](#authority) (состоящее из [экземпляра](#cloud-instance) поставщика удостоверений и [аудитории](#application-audience) для входа в приложение и, возможно, идентификатора клиента).
    - [Идентификатор клиента](#client-id).
    - [URI перенаправления](#redirect-uri).
    - [Секрет клиента](#client-secret) (для конфиденциальных клиентских приложений).
- [Параметры ведения журнала](#logging), в том числе уровень ведения журнала, Управление личными данными и имя компонента с помощью библиотеки.

## <a name="authority"></a>Authority
Центр — это URL-адрес, указывающий каталог, из которого MSAL может запрашивать маркеры. Общие центры:

- HTTPS\://Login.microsoftonline.com/\<клиент\>/, где &lt;клиент&gt; — это идентификатор клиента Azure Active Directory (Azure AD) или домен, связанный с этим клиентом Azure AD. Используется только для входа пользователей определенной организации.
- HTTPS\://Login.microsoftonline.com/Common/. Используется для входа пользователей с рабочими и учебными учетными записями или личными учетными записями Майкрософт.
- HTTPS\://Login.microsoftonline.com/organizations/. Используется для входа пользователей с рабочими и учебными учетными записями.
- HTTPS\://Login.microsoftonline.com/Consumers/. Используется для входа пользователей только с личными учетными записями Майкрософт (ранее известными как учетные записи Windows Live ID).

Параметр центра должен соответствовать требованиям, объявленным на портале регистрации приложений.

URL-адрес центра состоит из экземпляра и аудитории.

Центр может иметь следующие права:
- Облачный центр Azure AD.
- Центр Azure AD B2C. См. раздел [особенности B2C](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki/AAD-B2C-specifics).
- Центр службы федерации Active Directory (AD FS) (AD FS). См. раздел [поддержка AD FS](https://aka.ms/msal-net-adfs-support).

Облачные центры Azure AD состоит из двух частей:
- *Экземпляр* поставщика удостоверений
- Аудитория входа для  приложения

Экземпляр и аудиторию можно объединить и предоставить в качестве URL-адреса центра сертификации. В версиях MSAL.NET, предшествующих MSAL 3. *x*, вам пришлось самостоятельно составлять центр, основываясь на облаке, которому вы хотите ориентироваться, и аудитории для входа.  На этой схеме показано, как состоит URL-адрес центра:

![Как состоит URL-адрес центра](media/msal-client-application-configuration/authority.png)

## <a name="cloud-instance"></a>Облачный экземпляр
*Экземпляр* используется для указания того, подписывает ли приложение пользователей из общедоступного облака Azure или из национальных облаков. Используя MSAL в коде, вы можете задать экземпляр облака Azure с помощью перечисления или передать URL-адрес в [Национальный облачный экземпляр](authentication-national-cloud.md#azure-ad-authentication-endpoints) в качестве `Instance` члена (если это известно).

MSAL.NET выдаст явное исключение, если `Instance` указаны `AzureCloudInstance` и, и.

Если экземпляр не указан, приложение будет ориентироваться на экземпляр общедоступного облака Azure (экземпляр URL-адреса `https://login.onmicrosoftonline.com`).

## <a name="application-audience"></a>Аудитория приложения

Аудитория входа зависит от бизнес-потребностей вашего приложения:
- Если вы являетесь бизнес-разработчиком (LOB), возможно, вы создадите приложение с одним клиентом, которое будет использоваться только в вашей организации. В этом случае необходимо указать организацию по ее ИДЕНТИФИКАТОРу клиента (ИДЕНТИФИКАТОРу экземпляра Azure AD) или доменному имени, связанному с экземпляром Azure AD.
- Если вы являетесь независимым поставщиком программного обеспечения, вам может потребоваться войти в систему пользователей с помощью рабочих и учебных учетных записей в любой организации или в некоторых организациях (многоклиентское приложение). Но вам также может потребоваться войти в систему с помощью личных учетных записей Майкрософт.

### <a name="how-to-specify-the-audience-in-your-codeconfiguration"></a>Как указать аудиторию в коде или конфигурации
Используя MSAL в коде, вы указываете аудиторию, используя одно из следующих значений:
- Перечисление аудитории центра Azure AD
- Идентификатор клиента, который может быть следующим:
  - Идентификатор GUID (идентификатор вашего экземпляра Azure AD) для приложений с одним клиентом.
  - Доменное имя, связанное с экземпляром Azure AD (также для приложений с одним клиентом).
- Один из этих заполнителей в качестве идентификатора клиента вместо перечисления аудитории центра Azure AD:
    - `organizations`для многоклиентского приложения
    - `consumers`Вход пользователей только с помощью личных учетных записей
    - `common`Вход пользователей с помощью рабочих и учебных учетных записей или их личных учетных записей Майкрософт

MSAL выдаст осмысленное исключение, если указать как аудиторию центра Azure AD, так и идентификатор клиента.

Если вы не укажете аудиторию, приложение будет ориентироваться на Azure AD и личные учетные записи Майкрософт в качестве аудитории. (То есть он будет вести себя так, `common` как будто были указаны.)

### <a name="effective-audience"></a>Эффективная аудитория
Действующая аудитория для вашего приложения будет минимальной (если есть пересечение) аудитории, заданной в приложении, и аудиторией, указанной в регистрации приложения. Фактически [Регистрация приложений](https://aka.ms/appregistrations) возможности позволяют указать аудиторию (поддерживаемые типы учетных записей) для приложения. Дополнительные сведения см. в [кратком руководстве Регистрация приложения на платформе](quickstart-register-app.md)Microsoft Identity.

В настоящее время для входа в приложение только для пользователей с личными учетными записями Майкрософт достаточно настроить оба этих параметра:
- Задайте для `Work and school accounts and personal accounts`аудитории регистрации приложений значение.
- Задайте для `AadAuthorityAudience.PersonalMicrosoftAccount` аудитории в коде или конфигурации значение (или `TenantID` = "потребители").

## <a name="client-id"></a>Идентификатор клиента
Идентификатор клиента — уникальный идентификатор приложения (клиента), назначенный приложению Azure AD при регистрации приложения.

## <a name="redirect-uri"></a>URI перенаправления
URI перенаправления — это универсальный код ресурса (URI), по которому поставщик удостоверений отправляет маркеры безопасности обратно.

### <a name="redirect-uri-for-public-client-apps"></a>URI перенаправления для общедоступных клиентских приложений
Если вы являетесь общедоступным клиентским приложением-разработчиком, использующим MSAL:
- Вы хотите использовать `.WithDefaultRedirectUri()` в приложениях для настольных компьютеров или UWP (MSAL.NET 4.1 +). Этот метод задаст свойству URI перенаправления общедоступного клиентского приложения значение рекомендуемого URI перенаправления по умолчанию для общедоступных клиентских приложений. 

  Платформа  | URI перенаправления  
  ---------  | --------------
  Классическое приложение (FW .NET) | `https://login.microsoftonline.com/common/oauth2/nativeclient` 
  UWP | `WebAuthenticationBroker.GetCurrentApplicationCallbackUri()`значение. Это позволяет осуществлять единый вход с браузером, присвоив значение результату Вебаусентикатионброкер. Жеткуррентаппликатионкаллбаккури (), который необходимо зарегистрировать.
  .NET Core | `https://localhost`. Это позволяет пользователю использовать системный браузер для интерактивной проверки подлинности, так как .NET Core в данный момент не имеет пользовательского интерфейса для встроенного веб-представления.

- Вам не нужно добавлять URI перенаправления при создании приложения Xamarin Android и iOS, которое не поддерживает брокер (универсальный код ресурса (URI) перенаправления автоматически устанавливается `msal{ClientId}://auth` в для Xamarin Android и iOS

- URI перенаправления необходимо настроить в [Регистрация приложений](https://aka.ms/appregistrations):

   ![URI перенаправления в Регистрация приложений](media/msal-client-application-configuration/redirect-uri.png)

URI перенаправления можно переопределить с помощью `RedirectUri` свойства (например, при использовании брокеров). Ниже приведены некоторые примеры URI перенаправления для этого сценария.

- `RedirectUriOnAndroid`= "msauth-5a434691-CCB2-4fd1-b97b-b64bcfbc03fc://ком.Микрософт.идентити.клиент.сампле";
- `RedirectUriOnIos`= $ "msauth. {Пучок. ID}://АУС ";

Дополнительные сведения см. в [документации по Android и iOS](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki/Leveraging-the-broker-on-iOS).

### <a name="redirect-uri-for-confidential-client-apps"></a>URI перенаправления для конфиденциальных клиентских приложений
Для веб-приложений URI перенаправления (или URI ответа) — это универсальный код ресурса (URI), который Azure AD будет использовать для отправки маркера обратно в приложение. Этот универсальный код ресурса (URI) может быть URL веб-приложения или веб-API, если конфиденциальное приложение является одним из указанных. URI перенаправления должен быть зарегистрирован в регистрации приложения. Эта регистрация особенно важна при развертывании приложения, изначально протестированного локально. Затем необходимо добавить URL-адрес ответа развернутого приложения на портале регистрации приложений.

Для приложений управляющей программы не нужно указывать URI перенаправления.

## <a name="client-secret"></a>Секрет клиента
Этот параметр указывает секрет клиента для конфиденциального клиентского приложения. Этот секрет (пароль приложения) предоставляется порталом регистрации приложений или предоставляется Azure AD во время регистрации приложения с помощью PowerShell AzureAD, PowerShell AzureRM или Azure CLI.

## <a name="logging"></a>Ведение журнала
Другие параметры конфигурации включают ведение журнала и устранение неполадок. Дополнительные сведения об их использовании см. в статье [ведение журнала](msal-logging.md) .

## <a name="next-steps"></a>Следующие шаги
Дополнительные сведения о [создании экземпляров клиентских приложений с помощью MSAL.NET](msal-net-initializing-client-applications.md).

Дополнительные сведения о [создании экземпляров клиентских приложений с помощью MSAL. js](msal-js-initializing-client-applications.md).
