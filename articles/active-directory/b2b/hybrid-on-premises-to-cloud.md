---
title: Синхронизировать учетные записи локальных партнеров в облако в качестве пользователей B2B — Azure Active Directory | Документация Майкрософт
description: Предоставление локально управляемым внешним партнерам доступа к локальным и облачным ресурсам на основе одного комплекта учетных данных с помощью службы совместной работы Azure Active Directory B2B.
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: conceptual
ms.date: 04/24/2018
ms.author: mimart
author: msmimart
manager: celestedg
ms.reviewer: mal
ms.custom: it-pro, seo-update-azuread-jan
ms.collection: M365-identity-device-management
ms.openlocfilehash: 2ae75311ab61449f37ccea15a0bcb88fed80c3ed
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "65767340"
---
# <a name="grant-locally-managed-partner-accounts-access-to-cloud-resources-using-azure-ad-b2b-collaboration"></a>Предоставление локально управляемым партнерским учетным записям доступа к облачным ресурсам через службу совместной работы Azure Active Directory B2B

До появления Azure Active Directory (AAD) организации, которые использовали локальные системы управления удостоверениями, как правило, включали управляемые учетные записи для всех партнеров в локальный каталог. При переносе приложений из такой среды в AAD необходимо сохранить доступ партнеров ко всем используемым ими ресурсам. Расположение этих ресурсов (локально или в облаке) не должно иметь никакого значения. Также будет удобно, если партнеры смогут использовать одинаковые учетные данные для доступа к локальным ресурсам и к AAD. 

Если вы создали в локальном каталоге учетные записи для внешних партнеров (например, учетную запись wmoran для внешнего пользователя Wendy Moran в домене partners.contoso.com), вы сможете теперь синхронизировать такие учетные записи в облако. В частности, вы можете использовать службу Azure AD Connect для синхронизации партнерских учетных записей с облаком в качестве пользователей Azure AD B2B, у которых атрибут UserType имеет значение Guest. Так вы разрешите партнерам обращаться к облачным ресурсам с теми же учетными данными, которые они используют локально, предоставляя им только требуемые для этого права доступа. 

## <a name="identify-unique-attributes-for-usertype"></a>Определение уникальных атрибутов для UserType

Перед включением синхронизации атрибута UserType необходимо решить, каким образом этот атрибут будет получен из локальной службы Active Directory. Другими словами, какие параметры в локальной среде являются уникальными для внешних участников совместной работы? Определите параметр, который позволяет отличить этих участников от членов вашей организации.

Для этого существуют два распространенных подхода.

- Назначьте неиспользуемый локальный атрибут Active Directory (например, extensionAttribute1) в качестве исходного атрибута. 
- Кроме того, можно получить значение атрибута UserType из других свойств. Например, требуется синхронизировать всех пользователей в качестве гостевых, если их локальный атрибут Active Directory UserPrincipalName заканчивается доменном  *\@partners.contoso.com*.
 
Подробные требования к атрибутам приведены в разделе [Включение синхронизации атрибута UserType](../hybrid/how-to-connect-sync-change-the-configuration.md#enable-synchronization-of-usertype). 

## <a name="configure-azure-ad-connect-to-sync-users-to-the-cloud"></a>Настройка Azure AD Connect для синхронизации пользователей с облаком

После определения уникального атрибута можно настроить Azure AD Connect для синхронизации этих пользователей с облаком в качестве пользователей Azure AD B2B (то есть пользователей, у которых атрибут UserType имеет значение Guest). С точки зрения процесса авторизации эти пользователи будут неотличимы от пользователей B2B, созданных с помощью процесса приглашения в службу совместной работы Azure AD B2B.

Инструкции по реализации приведены в разделе [Включение синхронизации атрибута UserType](../hybrid/how-to-connect-sync-change-the-configuration.md#enable-synchronization-of-usertype).

## <a name="next-steps"></a>Дальнейшие действия

- [Служба совместной работы Azure Active Directory B2B для гибридных организаций](hybrid-organizations.md)
- [Предоставление пользователям B2B в Azure AD доступа к локальным приложениям](hybrid-cloud-to-on-premises.md)
- Общие сведения об Azure AD Connect см. в статье [Интеграция локальных каталогов с Azure Active Directory](../hybrid/whatis-hybrid-identity.md).

