---
title: Лицензии требования по использованию Azure Active Directory для управления привилегированными пользователями — | Документация Майкрософт
description: Сведения о требованиях к лицензиям для использования Azure AD Privileged Identity Management (PIM).
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: markwahl-msft
ms.assetid: 34367721-8b42-4fab-a443-a2e55cdbf33d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: pim
ms.date: 01/16/2019
ms.author: rolyon
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: 40e16209a185623b6e15650f70141edd6394e337
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "60437462"
---
# <a name="license-requirements-to-use-pim"></a>Требования к лицензиям для использования PIM

Чтобы использовать управление привилегированными пользователями (PIM) в Azure Active Directory (Azure AD), у каталога должна быть допустимая лицензия. Кроме того, лицензии должны быть назначены администраторам и соответствующим пользователям. В этой статье описываются требования к лицензиям для использования PIM.

## <a name="prerequisites"></a>Технические условия

Чтобы использовать PIM, в вашем каталоге должна быть одна из следующих платных или пробных лицензий:

- Azure AD Premium P2
- Enterprise Mobility + Security (EMS) E5.
- Microsoft 365 M5

Дополнительные сведения см. в статье [Что такое Microsoft Azure Active Directory](../fundamentals/active-directory-whatis.md).

## <a name="which-users-must-have-licenses"></a>У каких пользователей должны быть лицензии?

Лицензия должна быть у каждого администратора или пользователя, который получает преимущества от PIM или применяет эту технологию. Примеры приведены ниже.

- Администраторы с ролями Azure AD, управляемыми с помощью PIM.
- Администраторы с ролями ресурсов Azure, управляемыми с помощью PIM.
- Администраторы, которым назначена привилегированная роль администратора.
- Пользователи, назначенные право ролей Azure AD управлять с помощью PIM
- Пользователи, которые могут утверждать или отклонять запросы в PIM.
- Пользователи, которым назначена роль ресурса Azure с использованием JIT-назначений или прямых назначений (на основе времени).  
- Пользователи, которые назначены для проверки доступа.
- Пользователи, которые выполняют проверки доступа.

Сведения о том, как назначить лицензии вашим пользователям, см. в статье [Назначение или удаление лицензий с помощью портала Azure Active Directory](../fundamentals/license-users-groups.md).

## <a name="what-happens-when-a-license-expires"></a>Что происходит, когда срок действия лицензии истекает?

Если срок действия лицензии Azure AD Premium P2, EMS E5 или пробной лицензии истекает, функции PIM больше не будут доступны в вашем каталоге:

- назначенные постоянные роли Azure AD сохраняются без изменений;
- Служба PIM на портале Azure, а также командлеты API Graph и PIM-интерфейсы PowerShell больше не позволяют пользователям активировать привилегированные роли, управлять привилегированным доступом или выполнять проверки доступа для привилегированных ролей.
- удаляются назначения для ролей Azure AD, так как пользователи уже не могут активировать привилегированные роли;
- Завершаются все действующие проверки доступа для ролей Azure AD, и удаляются параметры конфигурации PIM.
- PIM больше не будет отправлять сообщения об изменении назначений ролей.

## <a name="next-steps"></a>Дальнейшие действия

- [Развертывание PIM](pim-deployment-plan.md)
- [Начало работы с PIM](pim-getting-started.md)
- [Роли, которыми вы не можете управлять в PIM](pim-roles.md)
