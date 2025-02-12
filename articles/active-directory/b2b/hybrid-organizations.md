---
title: Служба совместной работы B2B для гибридных организаций в Azure Active Directory | Документация Майкрософт
description: Предоставьте партнерам доступ к локальным и облачным ресурсам с помощью службы совместной работы AAD B2B.
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: conceptual
ms.date: 04/26/2018
ms.author: mimart
author: msmimart
manager: celestedg
ms.reviewer: mal
ms.collection: M365-identity-device-management
ms.openlocfilehash: b30e9c14863f67d28203eac916606e8786101e3b
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "65768285"
---
# <a name="azure-active-directory-b2b-collaboration-for-hybrid-organizations"></a>Служба совместной работы Azure Active Directory B2B для гибридных организаций

Служба совместной работы Azure Active Directory (AAD) B2B позволяет легко предоставить внешним партнерам доступ к приложениям и ресурсам вашей организации. Она применима даже в гибридной конфигурации, когда одновременно используются локальные и облачные ресурсы. При этом не имеет значения, как вы управляете учетными записями внешних партнеров: в локальной системе управления удостоверениями или в качестве пользователей AAD B2B в облаке. Теперь вы можете предоставить этим пользователям доступ к ресурсам в любом расположении, а им потребуется только один набор учетных данных для входа в обе среды.

## <a name="grant-b2b-users-in-azure-ad-access-to-your-on-premises-apps"></a>Предоставление пользователям AAD B2B доступа к локальным приложениям

Если ваша организация использует возможности совместной работы AAD B2B для приглашения гостевых пользователей из партнерских организаций в Azure AD, вы теперь сможете предоставить таким пользователям B2B доступ к приложениям в локальной среде.

Для приложений, использующих аутентификацию на основе SAML, вы можете предоставлять пользователям B2B доступ к приложениям через портал Azure, настроив AD Application Proxy для проверки подлинности.

В приложениях, которые используют встроенную проверку подлинности Windows (IWA) с ограниченным делегированием Kerberos (KCD), следует применять для аутентификации AAD Proxy. Но авторизация в этой схеме требует наличия объекта пользователя в локальном каталоге Active Directory на Windows Server. Создать локальные объекты пользователей, которые представляют гостевых пользователей B2B, можно двумя способами.

- Вы можете использовать Microsoft Identity Manager (MIM) 2016 SP1 и агент управления MIM для Microsoft Graph.
- Вы можете использовать скрипт PowerShell. (Для этого решения не требуется MIM.)

Дополнительные сведения о реализации этих решений см. в статье [Предоставление пользователям B2B в Azure AD доступа к локальным приложениям](hybrid-cloud-to-on-premises.md).

## <a name="grant-locally-managed-partner-accounts-access-to-cloud-resources"></a>Предоставление локально управляемым партнерским учетным записям доступа к облачным ресурсам

До появления AAD организации, которые использовали локальные системы управления удостоверениями, традиционно включали в локальный каталог управляемые учетные записи для всех партнеров. Если вы работаете в такой организации, вы наверняка хотите сохранить доступ партнеров к приложениям и другим ресурсам, которые вы перемещаете в облако. И лучше всего, чтобы эти пользователи использовали один набор учетных данных для доступа к локальным и облачным ресурсам. 

Теперь мы предоставили возможность синхронизировать локальные учетные записи с помощью AAD Connect с облаком, где они будут считаться "гостевыми пользователями" и использоваться точно так же, как обычные пользователи AAD B2B.

Чтобы защитить данные компании, вы можете строго регулировать доступ к нужным ресурсам и настроить политики авторизации, которые различают гостевых пользователей и сотрудников организации.

Подробнее см. статью [Предоставление локально управляемым партнерским учетным записям доступа к облачным ресурсам через службу совместной работы AAD B2B](hybrid-on-premises-to-cloud.md).
 
## <a name="next-steps"></a>Дальнейшие действия

- [Предоставление пользователям B2B в Azure AD доступа к локальным приложениям](hybrid-cloud-to-on-premises.md)
- [Предоставление локально управляемым партнерским учетным записям доступа к облачным ресурсам через службу совместной работы AAD B2B](hybrid-on-premises-to-cloud.md)


