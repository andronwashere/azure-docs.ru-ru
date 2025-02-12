---
title: Управление доступом к ресурсам Azure для внешних пользователей с использованием RBAC | Документация Майкрософт
description: Узнайте, как с помощью управления доступом на основе ролей (RBAC) управлять доступом к ресурсам Azure для внешних пользователей организации.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.assetid: ''
ms.service: role-based-access-control
ms.devlang: ''
ms.topic: conceptual
ms.tgt_pltfrm: ''
ms.workload: identity
ms.date: 03/20/2018
ms.author: rolyon
ms.reviewer: skwan
ms.custom: it-pro
ms.openlocfilehash: d919453816436366c00dde506210a2ed38cc69b7
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "65952212"
---
# <a name="manage-access-to-azure-resources-for-external-users-using-rbac"></a>Управление доступом к ресурсам Azure для внешних пользователей с использованием RBAC

Управление доступом на основе ролей (RBAC) предоставляет расширенные возможности управления безопасностью для больших организаций и SMB, работающих с внешними сотрудниками, поставщиками или фрилансерами, которым требуется доступ к определенным ресурсам в вашей среде, а не к целой инфраструктуре или любым областям, связанным с выставлением счетов. Кроме того, RBAC обеспечивает гибкость владения подпиской Azure, управляемой с использованием учетной записи администратора (роль администратора службы на уровне подписки), и позволяет нескольким пользователям работать в рамках одной подписки, но без прав администратора.

> [!NOTE]
> Подписки Office 365 или лицензии Azure Active Directory (например, Доступ к Azure Active Directory) подготовленные Microsoft 365, Центр администрирования не поддерживают RBAC.

## <a name="assign-rbac-roles-at-the-subscription-scope"></a>Назначение ролей RBAC в области действия подписки

Существует два распространенных примера использования RBAC (но это еще не все):

* При работе с внешними пользователями организации, которые не являются частью клиента Azure Active Directory администратора и которым предоставляется доступ на управление определенными ресурсами или целой подпиской.
* При работе с пользователями в организации, которые являются частью клиента Azure Active Directory пользователя, но входят в разные команды или группы и которым требуется доступ к целой подписке или определенным группам ресурсов (областям действия ресурсов) в среде.

## <a name="grant-access-at-a-subscription-level-for-a-user-outside-of-azure-active-directory"></a>Предоставление доступа на уровне подписки внешним пользователям Azure Active Directory

Роли RBAC можно получить только от **владельца** подписки. Таким образом, администратор должен войти в качестве пользователя, у которого эта роль предопределена или который уже создал подписку Azure.

Войдите на портал Azure как администратор. Выберите "Подписки", а затем выберите необходимую подписку.
![Колонка подписки на портале Azure](./media/role-assignments-external-users/0.png) По умолчанию администратору, который приобрел подписку Azure, назначается роль **администратора учетной записи**. Дополнительные сведения о ролях подписки Azure см. в статье [Добавление или изменение администраторов подписки Azure](../billing/billing-add-change-azure-subscription-administrator.md).

В этом примере пользователь alflanigan@outlook.com является **владельцем** бесплатной пробной версии подписки в клиенте AAD (каталог Default tenant Azure (Клиент Azure по умолчанию)). Так как этот пользователь создал подписку Azure, используя начальную учетную запись Microsoft Outlook, после добавления других пользователей в этот клиент к их учетной записи будет добавляться следующее доменное имя (по умолчанию): **"\@alflaniganuoutlook.onmicrosoft.com"** . Новое доменное имя состоит из имени пользователя и доменного имени пользователя, создавшего клиент, а также расширения **.onmicrosoft.com**.
Кроме того, пользователи могут войти в систему, используя имя личного домена в клиенте. Для этого сначала необходимо добавить это имя в новый клиент, а затем проверить. Дополнительные сведения о проверке имени личного домена в клиенте Azure Active Directory см. в [этой статье](../active-directory/fundamentals/add-custom-domain.md).

В этом примере в каталоге Default tenant Azure (Клиент Azure по умолчанию) содержатся только пользователи с доменным именем "\@alflanigan.onmicrosoft.com".

После выбора подписки администратор должен щелкнуть **Управление доступом (IAM)** и выбрать **Добавить новую роль**.

![Функция "Управление доступом (IAM)" на портале Azure](./media/role-assignments-external-users/1.png)

![Параметр "Добавить новую роль" в области "Управление доступом (IAM)" на портале Azure](./media/role-assignments-external-users/2.png)

Далее необходимо выбрать роль RBAC и пользователя, которому ее нужно назначить. В раскрывающемся меню **Роль** для администратора отображаются только встроенные роли RBAC, доступные в Azure. Подробное описание каждой роли и их назначаемых областей см. в статье [Встроенные роли для ресурсов Azure](built-in-roles.md).

Затем необходимо добавить адрес электронной почты внешнего пользователя. Этот внешний пользователь не должен отображаться в имеющемся клиенте. После внешний пользователь приглашен, они будут отображаться в разделе **подписки > Управление доступом (IAM)** с всех текущих пользователей, назначенных роли RBAC в области подписки.

![Добавить разрешения в новую роль RBAC](./media/role-assignments-external-users/3.png)

![Список ролей RBAC на уровне подписки](./media/role-assignments-external-users/4.png)

Пользователя chessercarlton@gmail.com пригласили стать **владельцем** бесплатной пробной версии подписки. После отправки приглашения внешний пользователь получит письмо со ссылкой активации.
![Приглашение по электронной почте стать участником роли RBAC](./media/role-assignments-external-users/5.png)

Новые внешние пользователи организации не имеют атрибутов в каталоге Default tenant Azure (Клиент Azure по умолчанию). Они будут созданы после предоставления этому пользователю разрешений на запись в каталог, связанный с подпиской назначенные им роли.

![Сообщение с приглашением стать участником роли RBAC](./media/role-assignments-external-users/6.png)

В дальнейшем этот пользователь отображается в клиенте Azure Active Directory как внешний. Это можно посмотреть на портале Azure.

![Колонка пользователя Azure Active Directory на портале Azure](./media/role-assignments-external-users/7.png)

В представлении **Пользователи** внешних пользователей можно распознать по разным типам значков на портале Azure.

Тем не менее внешний пользователь с ролью **Владелец** или **Участник** в области действия **подписки** не имеет доступа к каталогу администратора, если это разрешение ему не предоставит **глобальный администратор**. В свойствах пользователя можно определить параметр **Тип пользователя**, который имеет два значения: **Участник** и **Гость**. Участник — это зарегистрированный в каталоге пользователь, а гость — это пользователь, приглашенный в каталог из внешнего источника. Дополнительные сведения см. в статье [Как администраторы Azure Active Directory могут добавить пользователей службы совместной работы B2B?](../active-directory/active-directory-b2b-admin-add-users.md).

> [!NOTE]
> Убедитесь, что после ввода учетных данных на портале внешний пользователь входит в нужный каталог. Тот же пользователь может иметь доступ к нескольким каталогам и может войти в любой из них, щелкнув имя пользователя в верхнем правом углу на портале Azure и выбрав в раскрывающемся списке нужный каталог.

Будучи гостем, внешний пользователь может управлять всеми ресурсами в подписке Azure, но не имеет доступа к каталогу.

![Доступ к каталогу Azure Active Directory на портале Azure ограничен](./media/role-assignments-external-users/9.png)

Azure Active Directory и подписка Azure не имеют дочерних и родительских отношений, в отличие от других ресурсов Azure (например, виртуальные машины, виртуальные сети, веб-приложения, хранилище и т. д.), которые связаны с подпиской Azure. Создание этих ресурсов, выставление счетов за использование и управление ими выполняется в рамках подписки Azure, а с помощью самой подписки контролируется доступ к каталогу Azure. Дополнительные сведения см. в статье [Как связать или добавить подписку Azure в Azure Active Directory](../active-directory/fundamentals/active-directory-how-subscriptions-associated-directory.md).

Из всех встроенных ролей RBAC только роли **Владелец** и **Участник** предлагают возможности полного доступа к управлению всеми ресурсами в среде. Разница между ними заключается в том, что участник не может создавать и удалять роли RBAC. Другие встроенные роли, например **Участник виртуальных машин**, обеспечивают доступ к управлению только теми ресурсами, которые указаны в названиях этих ролей, вне зависимости от **группы ресурсов**, используемой при их создании.

К пользователю с ролью **Участник виртуальных машин**, назначенной на уровне подписки, применяются следующие разрешения и ограничения:

* просматривать все виртуальные машины независимо от даты их развертывания и группы ресурсов, к которой они принадлежат;
* полный доступ к управлению виртуальными машинами в подписке;
* не может просматривать другие типы ресурсов в подписке;
* не может изменять параметры выставления счетов.

## <a name="assign-a-built-in-rbac-role-to-an-external-user"></a>Назначение встроенной роли RBAC внешним пользователям

Для оценки разных сценариев внешнему пользователю alflanigan@gmail.com назначили роль **Участник виртуальных машин**.

![Встроенная роль "Участник виртуальных машин"](./media/role-assignments-external-users/11.png)

Внешний пользователь с этой встроенной ролью может просматривать только виртуальные машины и ресурсы Resource Manager, необходимые во время развертывания, а также управлять ими. Эти роли с ограниченными правами обеспечивают доступ только к соответствующим ресурсам, созданным на портале Azure.

![Обзор роли "Участник виртуальных машин" на портале Azure](./media/role-assignments-external-users/12.png)

## <a name="grant-access-at-a-subscription-level-for-a-user-in-the-same-directory"></a>Предоставление доступа на уровне подписки пользователям в том же каталоге

Этот процесс аналогичен добавлению внешнего пользователя и в отношении администратора, предоставляющего роль RBAC, и пользователя, которому эта роль назначается. Отличие заключается в том, что приглашенный пользователь не получит приглашение по электронной почте, так как после входа все области действия ресурсов в подписке будут доступны на панели мониторинга.

## <a name="assign-rbac-roles-at-the-resource-group-scope"></a>Назначение ролей RBAC в области действия группы ресурсов

Процесс назначения роли RBAC внешним и внутренним пользователям (в том же каталоге) в области действия **группы ресурсов** и на уровне подписки аналогичен. Пользователи с ролью RBAC могут просматривать в среде только группу ресурсов, к которой им предоставлен доступ, щелкнув значок **Группы ресурсов** на портале Azure.

## <a name="assign-rbac-roles-at-the-resource-scope"></a>Назначение ролей RBAC в области действия ресурсов

Процесс назначения роли RBAC в области действия ресурсов, на уровне подписки и на уровне группы ресурсов аналогичен. В обоих сценариях используется тот же рабочий процесс. Опять же, пользователи с ролью RBAC могут просматривать только те элементы, к которым им предоставлен доступ. Эти ресурсы отображаются на вкладке **Все ресурсы** или непосредственно на панели мониторинга.

Важный аспект, который следует учитывать при назначении роли RBAC на уровне группы ресурсов или на уровне ресурсов, — это то, что пользователи должны войти в нужный каталог.

![Вход в каталог на портале Azure](./media/role-assignments-external-users/13.png)

## <a name="assign-rbac-roles-for-an-azure-active-directory-group"></a>Назначение ролей RBAC группе Azure Active Directory

Все сценарии с использованием ролей RBAC на трех различных уровнях в Azure обеспечивают права на развертывание, администрирование различных ресурсов и управление ими, не управляя личной подпиской. Независимо от того, назначена ли роль RBAC на уровне подписки, группы ресурсов или ресурса, все созданные пользователями ресурсы оплачиваются в рамках подписки Azure, к которой они имеют доступ. Таким образом, пользователи с правами администратора выставления счетов всей подписки Azure имеют полное представление о потреблении ресурсов, независимо от того, кто ими управляет.

В более крупных организациях роли RBAC можно таким же образом применять к группам Azure Active Directory, учитывая то, что администраторы хотят предоставлять доступ командам или целым отделам, а не отдельно каждому пользователю. Это позволяет эффективно использовать время, а также предоставляет полезные возможности управления. Чтобы проиллюстрировать этот момент, мы назначили одной из групп в клиенте роль **Участник** на уровне подписки.

![Добавление роли RBAC группе AAD](./media/role-assignments-external-users/14.png)

Подготовка этих групп безопасности и управление ими выполняется только в Azure Active Directory.

