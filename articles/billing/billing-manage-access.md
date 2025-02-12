---
title: Управление доступом к счетам Azure | Документация Майкрософт
description: Узнайте, как предоставить доступ членами вашей команды к сведениям о выставлении счетов в Azure
services: ''
documentationcenter: ''
author: vikramdesai01
manager: amberb
editor: ''
tags: billing
ms.assetid: e4c4d136-2826-4938-868f-a7e67ff6b025
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 11/02/2018
ms.author: banders
ms.custom: seodec18
ms.openlocfilehash: 6726c876d0895f9488aa2ae5c225a6b2ac19e69f
ms.sourcegitcommit: ac1cfe497341429cf62eb934e87f3b5f3c79948e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/01/2019
ms.locfileid: "67491042"
---
# <a name="manage-access-to-billing-information-for-azure"></a>Управление доступом к данным для выставления счетов в Azure

Вы может предоставить другим людям доступ к сведениям о счетах для вашей учетной записи на портале Azure. Тип выставления счетов ролей и инструкции для предоставления доступа к сведениям о счетах зависят от типа учетной записи выставления счетов. Чтобы определить тип учетной записи выставления счетов, см. в разделе [Проверьте тип учетной записи выставления счетов](#check-the-type-of-your-billing-account).

Статья относится к клиентам с учетными записями Microsoft Online Service программы. Если вы являетесь клиентом Azure с соглашением Enterprise (EA) и администратора предприятия, можно предоставить администраторам отдела и владельца учетной записи разрешения на корпоративном портале. Дополнительные сведения см. в описании [административных ролей в Azure при использовании Соглашения Azure Enterprise](billing-understand-ea-roles.md). Если вы являетесь клиентом соглашения клиентов Microsoft, см. в разделе, [понять соглашении клиента Microsoft административные роли в Azure](billing-understand-mca-roles.md). 

## <a name="account-administrators-for-microsoft-online-service-program-accounts"></a>Администраторы учетных записей для учетных записей Microsoft Online Service программы

Учетная запись администратора является единственным владельцем учетной записи выставления счетов программы Microsoft Online Service. Для пользователей, которые зарегистрировались в Azure назначается роль. Администраторы учетных записей могут выполнять различные задачи выставления счетов, как создавать подписки, Просмотр счетов или менять тарифный план для подписки.

## <a name="give-others-access-to-view-billing-information"></a>Предоставить общий доступ для просмотра сведений о выставлении счетов

Администратор учетной записи можно предоставлять другим пользователям доступ к Azure для выставления счетов, назначив один из следующих ролей в подписке в учетной записи.

- Администратор служб
- Соадминистратор
- Владелец.
- участник;
- Читатель
- Читатель счетов

Эти роли имеют доступ к сведениям о счетах в [портала Azure](https://portal.azure.com/). Пользователей, которые назначены эти роли можно также использовать [API выставления счетов](billing-usage-rate-card-overview.md) для программного получения счета-фактуры и сведения об использовании.

Сведения о назначении ролей см. в статье [Управление доступом с помощью RBAC и портала Azure](../role-based-access-control/role-assignments-portal.md).

** Если вы являетесь клиентом EA, владелец учетной записи можно назначить выше роли другим пользователям своей группы. Но эти пользователи могли просматривать сведения об оплате, администратор предприятия должен разрешить отключил Возможность просмотра затрат на корпоративном портале.


### <a name="opt-in"></a> Предоставление пользователям разрешений на скачивание счетов

После администратор учетной записи, назначенной соответствующие роли другим пользователям, их необходимо включить доступ для скачивания счетов на портале Azure. Все счета, созданные до декабря 2016 года, доступны только администратору учетной записи.

1. Войдите в [портала Azure](https://portal.azure.com/), как администратор учетной записи

1. Выполните поиск по **Управление затратами + выставление счетов**.

    ![Снимок экрана, показывающий поиск на портале Azure](./media/billing-manage-access/billing-search-cost-management-billing.png)
 
1. Выберите **подписок** в левой области. В зависимости от доступа, может потребоваться выбрать область, выставления счетов, а затем выберите **подписок**.
 
    ![Снимок экрана: Выбор подписок](./media/billing-manage-access/billing-select-subscriptions.png)

1. Выберите **счета-фактуры** и затем **доступа к счету**.

    ![На снимке экрана показано, как делегировать доступ к счетам](./media/billing-manage-access/AA-optin.png)

1. Выберите **On** (Включено) и сохраните изменения.

    ![На снимке экрана показано, как включать и выключать делегирование доступа к счету](./media/billing-manage-access/AA-optinAllow.png)

Администратор учетной записи также может настроить отправку счетов по электронной почте. Дополнительные сведения см. в статье [Получение счета по электронной почте](billing-download-azure-invoice-daily-usage-date.md).

## <a name="give-read-only-access-to-billing"></a>Предоставление доступа только для чтения к выставлению счетов

Если пользователю нужен доступ к сведениям о счетах в подписке, но не требуется управлять службами Azure, назначьте ему роль читателя счетов. Эта роль хорошо подходит для сотрудников организации, которые отвечают за управление бюджетами и затратами для подписок Azure.

Функция чтения счетов находится в предварительной версии и пока не поддерживает неглобальные облака.

1. Войдите в [портала Azure](https://portal.azure.com/), как администратор учетной записи

1. Выполните поиск по **Управление затратами + выставление счетов**.

    ![Снимок экрана, показывающий поиск на портале Azure](./media/billing-manage-access/billing-search-cost-management-billing.png)

1. Выберите **подписок** в левой области. В зависимости от доступа, может потребоваться выбрать область, выставления счетов, а затем выберите **подписок**.
 
    ![Снимок экрана: Выбор подписок](./media/billing-manage-access/billing-select-subscriptions.png)

1. Выберите **Управление доступом (IAM)** .
1. Выберите **добавить** в верхней части страницы.

    ![Снимок экрана, показывающий щелкните Добавить назначение роли](./media/billing-manage-access/billing-click-add-role-assignment.png)

1. В раскрывающемся списке **Роль** выберите **Читатель счетов**.
1. В текстовом поле **Выбрать** введите имя или адрес электронной почты пользователя, которого вы хотите добавить.
1. Выберите пользователя.
1. Щелкните **Сохранить**.
    ![Снимок экрана, показывающий щелкните Добавить назначение роли](./media/billing-manage-access/billing-save-role-assignment.png)

1. Через несколько секунд пользователю назначается роль читателя счетов для подписки.

** Если вы являетесь клиентом EA, владельца учетной записи или администратор отдела можно назначить роль читателя счетов участникам команды. Но для того, чтобы читатель счетов получил доступ к сведениям для выставления счетов по отделу или всей учетной записи, корпоративный администратор предприятия должен включить политики **Возможность просмотра затрат для владельца учетной записи включена** или **Возможность просмотра затрат для администратора отдела включена** на корпоративном портале.

## <a name="check-the-type-of-your-billing-account"></a>Проверьте тип учетной записи выставления счетов
[!INCLUDE [billing-check-account-type](../../includes/billing-check-account-type.md)]

## <a name="next-steps"></a>Дальнейшие действия

- Пользователи с другими ролями, такими как владелец или участник, получают доступ не только к счетам, но и к службам Azure. Сведения об управлении этими ролями см. в статье [Управление доступом с помощью RBAC и портала Azure](../role-based-access-control/role-assignments-portal.md).
- Дополнительные сведения о ролях см. в статье [Встроенные роли для ресурсов Azure](../role-based-access-control/built-in-roles.md).

## <a name="need-help-contact-us"></a>Требуется помощь? Свяжитесь с нами.

Если у вас есть вопросы или нужна помощь, [создать запрос в службу поддержки](https://go.microsoft.com/fwlink/?linkid=2083458).
