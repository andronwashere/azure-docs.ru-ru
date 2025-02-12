---
title: Управление удостоверениями — Azure Active Directory | Документация Майкрософт
description: Azure Active Directory Identity Governance позволяет сбалансировать потребности Организации в обеспечении безопасности и производительности сотрудников с помощью правильных процессов и видимости.
services: active-directory
documentationcenter: ''
author: msaburnley
manager: daveba
editor: markwahl-msft
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: compliance
ms.date: 04/29/2019
ms.author: ajburnle
ms.reviewer: markwahl-msft
ms.collection: M365-identity-device-management
ms.openlocfilehash: 9f09a8c2f8caae3cbb182cf2dc4621deb95f7e5c
ms.sourcegitcommit: a0b37e18b8823025e64427c26fae9fb7a3fe355a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/25/2019
ms.locfileid: "68499664"
---
# <a name="what-is-azure-ad-identity-governance"></a>Что такое Azure AD Identity Governance?

Управление удостоверениями Azure Active Directory (Azure AD) позволяет сбалансировать потребности Организации в обеспечении безопасности и производительности сотрудников с помощью правильных процессов и видимости. Благодаря ей всем нужным пользователям будут предоставлены нужные права доступа к ресурсам. Кроме того, она включает функции защиты, мониторинга и аудита доступа к критически важным ресурсам, не влияя при этом на производительность сотрудников.  

Система управления идентификацией предоставляет организациям возможность выполнять следующие задачи для сотрудников, бизнес-партнеров и поставщиков, а также служб и приложений.

- управление жизненным циклом удостоверений;
- управление жизненным циклом доступа;
- защищенное администрирование.

В частности, эта служба помогает организациям контролировать четыре ключевых аспекта:

- Какие пользователи и к каким ресурсам должны иметь доступ?
- Как эти пользователи применяют этот доступ?
- Есть ли эффективные средства управления доступом на корпоративном уровне?
- Могут ли аудиторы убедиться, что эти средства работают правильно?

## <a name="identity-lifecycle"></a>Жизненный цикл удостоверений

Управление удостоверениями помогает организациям достичь баланса  между производительностью — как быстро можно получить доступ к необходимым ресурсам, например, когда они присоединяются к моей организации? и *безопасности* (динамика изменения уровня доступа со временем, например при изменении роли пользователя в организации).  Управление жизненным циклом удостоверений является основой для управления удостоверениями, а эффективное управление в масштабе требует модернизации инфраструктуры управления жизненным циклом удостоверений для приложений.

![Жизненный цикл удостоверений](./media/identity-governance-overview/identity-lifecycle.png)

Во многих организациях жизненный цикл удостоверений для сотрудников связан с представлением этого пользователя в системе управления человеческими ресурсами.  Azure AD Premium автоматически управляет удостоверениями пользователей, которые представлены в Workday одновременно в Active Directory и Azure Active Directory (см. [руководство по настройке Workday для автоматической подготовки пользователей (предварительная версия))](../saas-apps/workday-inbound-tutorial.md).  Azure AD Premium также включает [Microsoft Identity Manager](/microsoft-identity-manager/), который может импортировать записи из локальных систем управления человеческими ресурсами, например SAP, Oracle eBusiness или Oracle PeopleSoft.

Также следует учитывать постоянно растущую потребность в сотрудничестве с пользователями, не входящими в вашу организацию. Функции совместной работы [Azure AD B2B](/azure/active-directory/b2b/) позволяют безопасно предоставлять доступ к корпоративным приложениям и службам гостевым пользователям и внешним партнерам из любой организации, сохраняя полный контроль над корпоративными данными.

## <a name="access-lifecycle"></a>Жизненный цикл доступа

Организациям требуется использовать определенный процесс для управления доступом после первоначальной подготовки при создании удостоверения пользователя.  Более того, им важно иметь возможность эффективного масштабирования для разработки и применения политик доступа и элементов управления в непрерывном цикле.

![Жизненный цикл доступа](./media/identity-governance-overview/access-lifecycle.png)

Обычно ИТ-подразделения передают решения по утверждению доступа ответственным лицам в организации.  Также в этом процессе могут участвовать сами пользователи.  Например, пользователям, которые работают с конфиденциальными данными клиентов в маркетинговом приложении для Европы, нужна информация о соответствующих корпоративных политиках. Гостевые пользователи могут ничего не знать о внутренних требованиях к обработке данных, которые действуют в пригласившей их организации.

Организации могут автоматизировать процессы управления жизненным циклом доступа, применив такие технологии, как [динамические группы](../users-groups-roles/groups-dynamic-membership.md) и подготовка пользователей в [приложениях SaaS](../saas-apps/tutorial-list.md) или [приложениях, интегрированных с SCIM](../manage-apps/use-scim-to-provision-users-and-groups.md).  Организации также могут выбирать, [какие гостевые пользователи получат доступ к локальным приложениям](../b2b/hybrid-cloud-to-on-premises.md).  Также эти права доступа можно регулярно проверять с помощью [проверок доступа Azure Active Directory](access-reviews-overview.md).

Когда пользователь пытается получить доступ к приложениям, Azure AD применяет политики [условного доступа](/azure/active-directory/conditional-access/) . Например, политики условного доступа могут включать отображение [условий использования](../conditional-access/terms-of-use.md) и гарантируют [, что пользователь согласен с этими условиями](../conditional-access/require-tou.md) , прежде чем будет иметь доступ к приложению.

## <a name="privileged-access-lifecycle"></a>Жизненный цикл привилегированного доступа

Исторически привилегированный доступ был описан другими поставщиками как отдельная возможность в управлении удостоверениями. Однако в корпорации Майкрософт мы думаем, что Управление привилегированным доступом является ключевой частью управления идентификацией, в частности, при наличии потенциального нарушения, связанного с этими правами администратора, может стать причиной Организации. Контроль должен распространяться на всех сотрудников, поставщиков и подрядчиков, которые получают права администратора.

![Жизненный цикл привилегированного доступа](./media/identity-governance-overview/privileged-access-lifecycle.png)

Azure AD Privileged Identity Management (PIM) предоставляет целый ряд средств управления, предназначенных для защиты прав доступа к ресурсам в AAD, Azure и других службах Microsoft Online Services.  В дополнение к многофакторной проверке подлинности и условному доступу предоставляется исчерпывающий набор элементов управления для обеспечения безопасности ресурсов компании (каталога), а также возможности создания предупреждений о JIT-доступе и изменения ролей, предоставляемые Azure AD PIM. Office 365 и роли ресурсов Azure. Как и для других видов доступа, с помощью проверок доступа можно настроить регулярные сертификации прав доступа для всех пользователей с ролями администратора.

## <a name="getting-started"></a>Приступая к работе

Хотя не существует идеального решения или рекомендаций для каждого клиента, следующие конфигурации предоставляют рекомендации по базовым политикам, которые корпорация Майкрософт рекомендует для обеспечения более безопасных и производительных сотрудников.

- [Конфигурации доступа для удостоверений и устройств](/microsoft-365/enterprise/microsoft-365-policies-configurations)
- [Защита привилегированного доступа](../users-groups-roles/directory-admin-roles-secure.md)

Вы также можете перейти на вкладку Приступая к работе  в статье Управление удостоверениями в портал Azure, чтобы начать использовать управление ресурсами, проверки доступа, управление привилегированными пользователями и условия использования.

![Приступая к работе с системой управления удостоверениями](./media/identity-governance-overview/getting-started.png)

## <a name="next-steps"></a>Следующие шаги

- [Что такое управление запасами в Azure AD? (предварительная версия)](entitlement-management-overview.md)
- [Общие сведения о службе проверок доступа Azure AD](access-reviews-overview.md)
- [Что такое Azure AD Privileged Identity Management?](../privileged-identity-management/pim-configure.md)
- [Для чего нужна функции "Условия использования"](active-directory-tou.md)
