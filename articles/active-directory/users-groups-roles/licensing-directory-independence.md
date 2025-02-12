---
title: Характеристики взаимодействия нескольких клиентов в Azure Active Directory | Документация Майкрософт
description: Управление клиентами Azure Active Directory как полностью независимыми ресурсами.
services: active-tenant
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: article
ms.workload: identity
ms.subservice: users-groups-roles
ms.date: 01/31/2019
ms.author: curtand
ms.custom: it-pro
ms.reviewer: sumitp
ms.collection: M365-identity-device-management
ms.openlocfilehash: 45f48b6d8ef29d14606f18d4ccee77bd742a670a
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "60470078"
---
# <a name="understand-how-multiple-azure-active-directory-tenants-interact"></a>Сведения о взаимодействии нескольких клиентов Azure Active Directory

В Azure Active Directory (Azure AD) каждый клиент является полностью независимым ресурсом: равноправным, полнофункциональным и логически независимым от других клиентов, которыми вы управляете. Между клиентами нет связи типа "родительский объект — дочерний объект". Такая независимость клиентов подразумевает и независимость ресурсов, административную независимость и независимость синхронизации.

## <a name="resource-independence"></a>Независимость ресурсов
* Если создать или удалить ресурс в одном клиенте, это не повлияет на какой-либо ресурс в другом клиенте, за исключением внешних пользователей. 
* Если какое-либо из доменных имен используется для одного клиента, то оно не может использоваться для любого другого клиента.

## <a name="administrative-independence"></a>Административная независимость
Если пользователь без прав администратора клиента Contoso создает тестовый клиент Test, то:

* По умолчанию пользователь, создающий новый клиент, добавляется в него в качестве внешнего пользователя, и ему назначается роль глобального администратора этого клиента.
* У администраторов клиента Contoso нет прямых прав администратора для клиента Test, пока администратор Test явно не предоставит им эти привилегии. Тем не менее администраторы Contoso могут контролировать доступ к клиенту Test, если они управляют учетной записью пользователя, создавшего клиент Test.
* Если добавить или удалить роль администратора для пользователя в одном клиенте, это изменение не повлияет на роли администратора, которые назначены пользователю в другом клиенте.

## <a name="synchronization-independence"></a>Независимость синхронизации
Вы можете настроить каждый клиент Azure AD независимо, чтобы синхронизировать данные из одного из следующих экземпляров:

* инструмент Azure AD Connect для синхронизации данных с одним лесом AD;
* соединитель Azure Active Directory для Forefront Identity Manager для синхронизации данных с одним или несколькими локальными лесами либо источниками данных за пределами Azure AD.

## <a name="add-an-azure-ad-tenant"></a>Добавление клиента Azure AD
Чтобы добавить клиент Azure AD на портале Azure, войдите на [портал Azure](https://portal.azure.com) под учетной записью глобального администратора Azure AD, а затем щелкните слева **Создать**.

> [!NOTE]
> В отличие от других ресурсов Azure, клиенты не являются дочерними ресурсами подписки Azure. Если ваша подписка отменена, или истек срок действия, по-прежнему доступны данные клиента, с помощью Azure PowerShell, Azure Graph API или в центре администрирования Microsoft 365. Можно также [связать другую подписку с клиентом](../fundamentals/active-directory-how-subscriptions-associated-directory.md).
>

## <a name="next-steps"></a>Дальнейшие действия
Общие сведения о лицензировании Azure AD и рекомендации по работе с этой службой см. в статье [Основы группового лицензирования в Azure Active Directory](../fundamentals/active-directory-licensing-whatis-azure-portal.md).
