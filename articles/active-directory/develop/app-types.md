---
title: Типы приложений в версии 1.0 | Azure
description: В этой статье описываются типы приложений и сценарии, поддерживаемые конечной точкой Azure Active Directory версии 2.0.
services: active-directory
documentationcenter: ''
author: rwike77
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 09/24/2018
ms.author: ryanwi
ms.reviewer: saeeda, jmprieur, andret
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: efed9e35aed729c9efa39b0772b681d8c53ba7b8
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "65540664"
---
# <a name="application-types-in-v10"></a>Типы приложений в версии 1.0

Azure Active Directory (Azure AD) поддерживает проверку подлинности для различных современных архитектур приложений, основанных на стандартных отраслевых протоколах OAuth 2.0 или OpenID Connect.

На следующей схеме показаны эти сценарии и типы приложений, а также способы добавления различных компонентов:

![Типы приложений и сценарии](./media/authentication-scenarios/application_types_and_scenarios.png)

Ниже приведены пять основных сценариев приложений, которые поддерживает система Azure AD:

- **[Одностраничные приложения (SPA).](single-page-application.md)** Пользователю нужно войти в одностраничное приложение, защиту которого обеспечивает Azure Active Directory.
- **[Веб-браузер — веб-приложение.](web-app.md)** Пользователю нужно войти в веб-приложение, защиту которого обеспечивает Azure Active Directory.
- **[Собственное приложение — веб-API.](native-app.md)** Собственному приложению, которое выполняется на телефоне, планшете или компьютере, необходимо выполнить аутентификацию пользователя для получения ресурсов из веб-API, защиту которого обеспечивает Azure Active Directory.
- **[Веб-приложение — веб-API.](web-api.md)** Веб-приложению требуется получать ресурсы из веб-API, защиту которого обеспечивает Azure Active Directory.
- **[Управляющая программа или серверное приложение — веб-API.](service-to-service.md)** Управляющей программе или серверному приложению, в котором нет веб-интерфейса пользователя, требуется получить ресурсы из веб-API, защиту которого обеспечивает Azure Active Directory.

Прежде чем начать работу с кодом, воспользуйтесь этими ссылками, чтобы подробнее изучить каждый тип приложения и ознакомиться с общими сценариями. Вы также узнаете о различиях, которые нужно учитывать при написании конкретного приложения, работающего с конечной точкой версии 1.0 или 2.0.

> [!NOTE]
> Конечная точка версии 2.0 поддерживает не все сценарии и возможности Azure AD. Чтобы определить, стоит ли вам использовать конечную точку версии 2.0, ознакомьтесь с [ограничениями версии 2.0](active-directory-v2-limitations.md).

Вы можете разрабатывать любые приложения и сценарии, описанные здесь, используя различные языки и платформы. Все сценарии снабжены полными примерами кода, доступными в соответствующих руководствах: [примеры кода для сценариев с использованием версии 1.0](sample-v1-code.md) и [примеры кода для сценариев с использованием версии 2.0](sample-v2-code.md). Примеры кода также можно скачать напрямую из соответствующих [репозиториев с примерами на сайте GitHub](https://github.com/Azure-Samples?q=active-directory).

Кроме того, если вашему приложению требуется конкретная часть или сегмент комплексного сценария, то в большинстве случаев эти функции можно добавлять независимым образом. Например, если имеется нативное приложение, которое вызывает веб-интерфейс API, можно легко добавить веб-приложение, которое также будет вызывать такой интерфейс.

## <a name="app-registration"></a>Регистрация приложения

### <a name="registering-an-app-that-uses-the-azure-ad-v10-endpoint"></a>Регистрация приложения, в котором используется конечная точку Azure AD версии 1.0

Любое приложение, передающее функцию проверку подлинности системе Azure AD, должно быть зарегистрировано в каталоге. Этот шаг подразумевает предоставление системе Azure AD информации об этом приложении, включая URL-адрес доступа к приложению, URL-адрес для отправки ответов после проверки подлинности, URI для идентификации приложения и многое другое. Эта информация необходима по нескольким основным причинам:

* Службе Azure AD необходимо взаимодействовать с приложением при выполнении входа в систему или при обмене маркерами. Данные, передаваемые между Azure AD и приложением, включают в себя следующее:
  
  * **URI идентификатора приложения** — идентификатор приложения. Это значение отправляется в Azure AD во время проверки подлинности и указывает, для какого приложения вызывающая сторона желает получить маркер. Кроме того, это значение включается в маркер, чтобы приложение знало, что именно это значение запрашивалось.
  * **URL-адрес ответа** и **URI перенаправления** — для веб-API или веб-приложения URL-адрес ответа указывает то место, куда Azure AD будет отправлять ответ после аутентификации, в том числе маркер в случае успешной проверки подлинности. Для собственного приложения URI перенаправление — это уникальный идентификатор, на который Azure AD будет перенаправлять агент пользователя при запросе OAuth 2.0.
  * **Идентификатор приложения** — идентификатор для приложения, который создается при регистрации приложения в Azure AD. При запросе кода авторизации или маркера идентификатор приложения и ключ отправляются в Azure AD во время проверки подлинности.
  * **Ключ** — ключ, который отправляется вместе с идентификатором приложения при проверке подлинности в Azure AD для вызова веб-API.
* Службе Azure AD необходимо убедиться, что приложение содержит необходимые разрешения для доступа к данным вашего каталога, другим приложениям в вашей организации и т. д.

Дополнительные сведения см. в статье о [регистрации приложения](quickstart-register-app.md).

## <a name="single-tenant-and-multi-tenant-apps"></a>Однотенантные и мультитенантные приложения

Процесс подготовки станет яснее, когда вы поймете, что существует две категории приложений, которые можно разрабатывать и интегрировать с Azure AD:

* **Однотенантное приложение** —такое приложение (для одного клиента) предназначено для использования в одной организации. Обычно это бизнес-приложения (LoB), которые создает разработчик компании. Однотенантное приложение предоставляет пользователям доступ только в одном каталоге. В результате, его достаточно подготовить в одном каталоге. Такие приложения обычно регистрируются разработчиком в организации.
* **Мультитенантное приложение** — такое приложение (для нескольких клиентов) предназначено для использования в нескольких организациях. Как правило, это приложения категории "программное обеспечение как услуга" (SaaS), которые создают независимые поставщики программных продуктов (ISV). Мультитенантные приложения необходимо подготавливать в каждом каталоге, где они будут использоваться. Следовательно, для их регистрации требуется предоставление разрешений пользователем или администратором. Процесс предоставление разрешений начинается, когда приложение регистрируется в каталоге и получает доступ к API-интерфейсу Graph или, возможно, к другому веб-интерфейсу API. Когда пользователь или администратор из другой организации регистрируется для использования приложения, для них появляется диалоговое окно, в котором отображаются разрешения, необходимые для приложения. Пользователь или администратор может предоставить приложению требуемые разрешения, что дает приложению доступ к необходимым данным и регистрирует приложение в их каталоге. Дополнительные сведения см. в разделе [Обзор платформы согласия](consent-framework.md).

### <a name="additional-considerations-when-developing-single-tenant-or-multi-tenant-apps"></a>Дополнительные рекомендации при разработке однотенантных или мультитенантных приложений

При разработке мультитенантного приложения вместо однотенантного необходимо учесть некоторые дополнительные соображения. Например, если вы делаете свое приложение доступным для пользователей в нескольких каталогах, требуется механизм, чтобы определять, членами какого клиента (тенанта) они являются. Однотенантное приложение должно присутствовать только в собственном каталоге для данного пользователя, тогда как мультитенантное приложение должно идентифицировать конкретного пользователя из любого каталога в Azure AD. Для выполнения этой задачи система Azure AD предоставляет общую конечную точку проверки подлинности, в которую мультитенантное приложение может направлять запросы на вход, вместо конечной точки для конкретного клиента. Для всех каталогов в Azure AD эта конечная точка — https://login.microsoftonline.com/common, тогда как конечной точкой для конкретного клиента может быть https://login.microsoftonline.com/contoso.onmicrosoft.com. Общую конечную точку особенно важно учитывать при разработке своего приложения, так как вам будет нужна специальная логика для обработки нескольких клиентов во время входа, выхода и проверки маркеров.

Если вы разрабатываете однотенантное приложение, но хотите сделать его доступным для многих организаций, вы можете легко внести изменения в само приложение и его конфигурацию в системе Azure AD и переделать его на мультитенантное. Кроме того, Azure AD использует один и тот же ключ подписи для всех маркеров во всех каталогах, независимо от того, выполняется ли проверка подлинности в однотенантном или мультитенантном приложении.

Каждый сценарий, рассматриваемый в этом документе, содержит подраздел, где описываются соответствующие требования по подготовке. Подробнее о подготовке приложения в Azure AD, а также о различиях между однотенантными и мультитенантными приложениями можно узнать в статье [Интеграция приложений с Azure Active Directory](quickstart-v1-integrate-apps-with-azure-ad.md). Продолжим рассмотрение типичных сценариев работы приложений в Azure AD.

## <a name="next-steps"></a>Дальнейшие действия

- Ознакомьтесь с [основными сведениями о проверке подлинности](authentication-scenarios.md) в Azure AD.
