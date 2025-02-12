---
title: Руководство разработчика по Azure Active Directory условному доступу
description: Руководство разработчика и сценарии для условного доступа Azure AD
services: active-directory
keywords: ''
author: rwike77
manager: CelesteDG
ms.author: ryanwi
ms.reviewer: jmprieur, saeeda
ms.date: 02/28/2019
ms.service: active-directory
ms.subservice: develop
ms.custom: aaddev
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.collection: M365-identity-device-management
ms.openlocfilehash: dc93a7de824aeaf173e7179de0b0233b73488feb
ms.sourcegitcommit: 4b431e86e47b6feb8ac6b61487f910c17a55d121
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/18/2019
ms.locfileid: "68321155"
---
# <a name="developer-guidance-for-azure-active-directory-conditional-access"></a>Руководство разработчика по Azure Active Directory условному доступу

Функция условного доступа в Azure Active Directory (Azure AD) предлагает один из нескольких способов, которые можно использовать для защиты приложения и защиты службы. Условный доступ позволяет разработчикам и корпоративным клиентам защищать службы множеством способов, включая:

* Многофакторная Идентификация
* разрешение доступа к определенным службам только устройствам, зарегистрированным в Intune;
* ограничение расположения пользователей и диапазонов IP-адресов.

Дополнительные сведения о всех возможностях условного доступа см. [в разделе условный доступ в Azure Active Directory](../active-directory-conditional-access-azure-portal.md).

Для разработчиков, создающих приложения для Azure AD, в этой статье показано, как можно использовать условный доступ, и вы также узнаете о влиянии доступа к ресурсам, к которым у вас нет контроля условного доступа. В статье также рассматриваются последствия условного доступа в потоке "от имени", веб-приложениях, обращении к Microsoft Graph и вызове API.

Предполагается знание [однотенантных](quickstart-v1-integrate-apps-with-azure-ad.md) и [мультитенантных](howto-convert-app-to-be-multi-tenant.md) приложений, а также [распространенных шаблонов аутентификации](authentication-scenarios.md).

## <a name="how-does-conditional-access-impact-an-app"></a>Как условный доступ влияет на приложение?

### <a name="app-types-impacted"></a>Затронутые типы приложений

В большинстве случаев условный доступ не изменяет поведение приложения или требует внесения изменений от разработчика. Только в некоторых случаях, когда приложение косвенно или автоматически запрашивает маркер для службы, приложению требуются изменения кода для обработки "проблем условного доступа". Это может быть так же просто, как выполнение запроса на интерактивный вход.

В частности, в следующих сценариях требуется, чтобы код обрабатывал проблемы условного доступа:

* приложения, выполняющие поток On-Behalf-Of;
* приложения, получающие доступ к нескольким службам или ресурсам;
* одностраничные приложения, использующие ADAL.js.
* веб-приложения, которые вызывают ресурс.

Политики условного доступа могут применяться к приложению, но также могут применяться к веб-API, к которому обращается приложение. Дополнительные сведения о настройке политики условного доступа см. в разделе [краткое руководство. Требовать MFA для конкретных приложений с Azure Active Directory условным](../conditional-access/app-based-mfa.md)доступом.

В зависимости от сценария корпоративный клиент может в любое время применять и удалять политики условного доступа. Чтобы ваше приложение продолжало функционировать при применении новой политики, необходимо реализовать обработку запросов. В следующих примерах показана обработка запроса.

### <a name="conditional-access-examples"></a>Примеры условного доступа

В некоторых сценариях для управления условным доступом требуются изменения кода, тогда как другие работают как есть. Ниже приведено несколько сценариев, использующих условный доступ для многофакторной проверки подлинности, которая дает некоторую информацию о разнице.

* Вы создаете приложение iOS с одним клиентом и примените политику условного доступа. В приложении выполняется вход пользователя и оно не запрашивает доступ к API. При входе пользователя в систему политика автоматически вызывается и пользователю необходимо выполнить многофакторную проверку подлинности (MFA). 
* Вы создаете собственное приложение, в котором используется служба среднего уровня для доступа к нисходящему API. Корпоративный клиент в организации, использующей это приложение, применяет политику к нисходящему API. Когда пользователь входит в систему, собственное приложение запрашивает доступ к среднему уровню и отправляет маркер. Средний уровень выполняет поток On-Behalf-Of для запроса доступа к нисходящему API. На этом этапе запрос утверждений передается на средний уровень. Средний уровень отправляет запрос обратно в собственное приложение, которое должно соответствовать политике условного доступа.

#### <a name="microsoft-graph"></a>Microsoft Graph

Microsoft Graph имеет особые соображения при создании приложений в средах условного доступа. Как правило, механизм условного доступа работает одинаково, но политики, которые пользователи видят, будут основываться на базовых данных, запрашиваемых приложением из графа. 

В частности, все области Microsoft Graph представляют некоторый набор данных, к которому можно применить политики по отдельности. Так как политикам условного доступа назначаются определенные наборы данных, Azure AD будет применять политики условного доступа на основе диаграммы, а не самих графов.

Например, если приложение запрашивает следующие области Microsoft Graph,

```
scopes="Bookings.Read.All Mail.Read"
```

Приложение может предполагать, что пользователи будут выполнять все политики, заданные в книгах и Exchange. Некоторые области могут сопоставляться с несколькими наборами данных, если они предоставляют доступ. 

### <a name="complying-with-a-conditional-access-policy"></a>Соблюдение политики условного доступа

Для нескольких различных топологий приложений при установке сеанса оценивается политика условного доступа. Так как политика условного доступа работает с гранулярностью приложений и служб, точка, в которой она вызывается, в значительной степени зависит от сценария, который вы пытаетесь выполнить.

Когда приложение пытается получить доступ к службе с помощью политики условного доступа, может возникнуть проблема условного доступа. Эта задача кодируется в `claims` параметре, который поступает в ответе от Azure AD. Ниже приведен пример этого параметра запроса: 

```
claims={"access_token":{"polids":{"essential":true,"Values":["<GUID>"]}}}
```

Разработчики могут воспользоваться этим запросом и добавить его в новый запрос к Azure AD. При передаче этого состояния пользователю предлагается выполнить любое действие, необходимое для соблюдения политики условного доступа. В следующих сценариях описываются особенности ошибок и представлены сведения о том, как извлечь параметр.

## <a name="scenarios"></a>Сценарии

### <a name="prerequisites"></a>предварительные требования

Условный доступ Azure AD — это компонент, включенный в [Azure AD Premium](https://docs.microsoft.com/azure/active-directory/active-directory-whatis). Дополнительные сведения о требованиях к лицензиям см. в статье [Отчет о нелицензированном использовании](../active-directory-conditional-access-unlicensed-usage-report.md). Разработчики могут присоединиться к сети разработчиков [Microsoft Developer Network](https://msdn.microsoft.com/dn308572.aspx), которая включает бесплатную подписку на Enterprise Mobility Suite, содержащую Azure AD Premium.

### <a name="considerations-for-specific-scenarios"></a>Рекомендации для конкретных сценариев

Следующие сведения применимы только в следующих сценариях условного доступа:

* приложения, выполняющие поток On-Behalf-Of;
* приложения, получающие доступ к нескольким службам или ресурсам;
* одностраничные приложения, использующие ADAL.js.

В следующих разделах рассматриваются более сложные сценарии. Основной принцип работы — это политики условного доступа, которые оцениваются во время запроса маркера для службы, к которой применена политика условного доступа.

## <a name="scenario-app-performing-the-on-behalf-of-flow"></a>Сценарий. приложение, выполняющее поток On-Behalf-Of

В этом сценарии рассматривается вариант, при котором собственное приложение вызывает веб-службу или API. В свою очередь, эта служба выполняет поток "от имени", чтобы вызвать подчиненную службу. В нашем случае мы применили политику условного доступа к подчиненной службе (веб-API 2) и используете собственное приложение, а не приложение серверной или управляющей программы. 

![Схема приложения, выполняющего поток On-Behalf-Of](./media/conditional-access-dev-guide/app-performing-on-behalf-of-scenario.png)

В изначальном запросе маркера для веб-API 1 не отображается запрос на многофакторную проверку подлинности пользователя, так как веб-API 1 может не всегда получить доступ к подчиненному API. Когда веб-API 1 пытается запросить маркер от имени пользователя для веб-API 2, запрос завершается ошибкой, так как пользователь не выполнил вход и не прошел многофакторную проверку подлинности.

Azure AD возвращает ответ HTTP с некоторыми полезными данными:

> [!NOTE]
> В этом экземпляре это описание ошибки многофакторной проверки подлинности, но существует широкий спектр `interaction_required` возможных относящихся к условному доступу.

```
HTTP 400; Bad Request
error=interaction_required
error_description=AADSTS50076: Due to a configuration change made by your administrator, or because you moved to a new location, you must use multi-factor authentication to access '<Web API 2 App/Client ID>'.
claims={"access_token":{"polids":{"essential":true,"Values":["<GUID>"]}}}
```

В веб-API 1 перехватывается ошибка `error=interaction_required` и отправляется запрос `claims` в классическое приложение. На этом этапе классическое приложение может сделать новый вызов `acquireToken()` и добавить запрос `claims` в качестве дополнительного параметра строки запроса. Новый запрос требует от пользователя выполнить многофакторную проверку подлинности, отправить новый маркер в веб-API 1 и завершить поток On-Behalf-Of.

Чтобы опробовать этот сценарий, см. [пример кода .NET](https://github.com/Azure-Samples/active-directory-dotnet-webapi-onbehalfof-ca). Он демонстрирует передачу запроса утверждений из веб-API 1 в собственное приложение и создание запроса в клиентском приложении.

## <a name="scenario-app-accessing-multiple-services"></a>Сценарий: приложение, обращающееся к нескольким службам

В этом сценарии мы рассмотрим ситуацию, в которой веб-приложение обращается к двум службам, к которым назначена политика условного доступа. В зависимости от логики приложения ему может не потребоваться доступ к обеим веб-службам. В этом сценарии порядок запроса маркера играет важную роль при взаимодействии с пользователем.

Предположим, что у нас есть веб-служба A и B, а для веб-службы б применена политика условного доступа. Хотя первоначальный запрос на интерактивную проверку подлинности требует согласия для обеих служб, во всех случаях политика условного доступа не требуется. Если приложение запрашивает маркер для веб-службы B, тогда вызывается политика и последующие запросы к веб-службе A также выполняются следующим образом.

![Схема приложения, получающего доступ к нескольким службам](./media/conditional-access-dev-guide/app-accessing-multiple-services-scenario.png)

Кроме того, если приложение изначально запрашивает маркер для веб-службы A, конечный пользователь не вызывает политику условного доступа. Это позволяет разработчику приложений управлять интерфейсом пользователя и не вызывать принудительную политику условного доступа во всех случаях. Несложная ситуация заключается в том, что приложение в дальнейшем запрашивает маркер для веб-службы B. На этом этапе конечный пользователь должен соответствовать политике условного доступа. Когда приложение пытается выполнить операцию `acquireToken`, может произойти следующая ошибка (как показано на схеме ниже):

```
HTTP 400; Bad Request
error=interaction_required
error_description=AADSTS50076: Due to a configuration change made by your administrator, or because you moved to a new location, you must use multi-factor authentication to access '<Web API App/Client ID>'.
claims={"access_token":{"polids":{"essential":true,"Values":["<GUID>"]}}}
```

![Приложение, получающее доступ к нескольким службам, запрашивающим новый маркер](./media/conditional-access-dev-guide/app-accessing-multiple-services-new-token.png)

Если приложение использует библиотеку ADAL, при сбое получения маркера всегда осуществляется повторная попытка в интерактивном режиме. При выполнении этого интерактивного запроса у конечного пользователя есть возможность соответствовать условному доступу. Это справедливо, если запрос не является, `AcquireTokenSilentAsync` или `PromptBehavior.Never` в этом случае приложению требуется выполнить интерактивный ```AcquireToken``` запрос, чтобы предоставить конечному пользователю возможность соответствовать политике.

## <a name="scenario-single-page-app-spa-using-adaljs"></a>Сценарий. одностраничное приложение (SPA), использующее ADAL.js

В этом сценарии мы рассмотрим ситуацию, когда у нас есть одностраничное приложение (SPA), использующее файл ADAL. js для вызова веб-API, защищенного с помощью условного доступа. Это простая архитектура, но с некоторыми нюансами, которые необходимо учитывать при разработке с помощью условного доступа.

В ADAL.js есть несколько функций получения маркеров: `login()`, `acquireToken(...)`, `acquireTokenPopup(…)` и `acquireTokenRedirect(…)`.

* `login()`Получает маркер идентификации через интерактивный запрос на вход, но не получает маркеры доступа для любой службы (включая защищенный от условного доступа веб-API).
* `acquireToken(…)` может использоваться для автоматического получения маркера доступа. Это означает, что он никогда не будет отображаться на пользовательском интерфейсе.
* `acquireTokenPopup(…)` и `acquireTokenRedirect(…)` используются для автоматического запроса маркера для ресурса. Это означает, что они всегда отображаются на пользовательском интерфейсе входа.

Если приложению требуется маркер доступа для вызова веб-API, оно попробует выполнить функцию `acquireToken(…)`. Если срок действия сеанса маркера истек или необходимо соблюдать политику условного доступа, функция *acquireToken* завершается сбоем и приложение использует `acquireTokenPopup()` или. `acquireTokenRedirect()`

![Схема одностраничного приложения, использующего поток ADAL](./media/conditional-access-dev-guide/spa-using-adal-scenario.png)

Давайте рассмотрим пример с нашим сценарием условного доступа. Пользователь только что зашел на веб-сайт и не имеет сеанса. Мы выполним вызов `login()` и получим идентификатор маркера без многофакторной проверки подлинности. Затем пользователь нажимает кнопку, которая требует от приложения запросить данные из веб-API. Приложение пытается выполнить вызов, `acquireToken()` но не выполняется, так как пользователь еще не выполнил многофакторную проверку подлинности и должен соответствовать политике условного доступа.

Azure AD отправляет следующий ответ HTTP:

```
HTTP 400; Bad Request
error=interaction_required
error_description=AADSTS50076: Due to a configuration change made by your administrator, or because you moved to a new location, you must use multi-factor authentication to access '<Web API App/Client ID>'.
```

Приложению необходимо перехватить `error=interaction_required`. Приложение может использовать `acquireTokenPopup()` или `acquireTokenRedirect()` для того же ресурса. Пользователь вынужден пройти многофакторную проверку подлинности. После того как пользователь завершит многофакторную проверку подлинности, приложению выдается новый маркер доступа для запрашиваемого ресурса.

Чтобы проверить этот сценарий, см. [пример кода On-Behalf-Of JS SPA](https://github.com/Azure-Samples/active-directory-dotnet-webapi-onbehalfof-ca). В этом примере кода для демонстрации этого сценария используется политика условного доступа и веб-API, зарегистрированные ранее с помощью JS SPA. По примеру можно узнать, как правильно обрабатывать запросы утверждений и получить маркер доступа, который может использоваться для веб-API. Кроме того ознакомьтесь с общим [примером кода Angular.js](https://github.com/Azure-Samples/active-directory-angularjs-singlepageapp) для получения руководства по Angular SPA.

## <a name="see-also"></a>См. также

* Дополнительные сведения о возможностях условного доступа см. в статье об [условном доступе в Azure Active Directory](../active-directory-conditional-access-azure-portal.md).
* Дополнительные примеры кода Azure AD доступны в [репозитории примеров кода GitHub](https://github.com/azure-samples?utf8=%E2%9C%93&q=active-directory).
* Дополнительные сведения о пакете SDK ADAL и доступу к справочной документации см. в статье [Библиотеки проверки подлинности Azure Active Directory](active-directory-authentication-libraries.md).
* Дополнительные сведения о сценариях с несколькими клиентами см. в статье [Как реализовать вход любого пользователя Azure Active Directory (AD) с помощью шаблона мультитенантного приложения](howto-convert-app-to-be-multi-tenant.md).
