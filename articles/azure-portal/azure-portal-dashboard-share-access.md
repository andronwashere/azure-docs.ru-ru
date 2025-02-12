---
title: Предоставление общего доступа к панелям мониторинга на портале Azure с помощью управления доступом на основе ролей | Документация Майкрософт
description: В этой статье описывается, как предоставить общий доступ к панели мониторинга на портале Azure с помощью управления доступом на основе ролей.
services: azure-portal
documentationcenter: ''
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 8908a6ce-ae0c-4f60-a0c9-b3acfe823365
ms.service: azure-portal
ms.devlang: NA
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 08/01/2016
ms.author: tomfitz
ms.openlocfilehash: fbbc8a4f636a95d18baa0dc5de541279ce36789b
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "60551994"
---
# <a name="share-azure-dashboards-by-using-role-based-access-control"></a>Предоставление общего доступа к панелям мониторинга Azure с помощью управления доступом на основе ролей
После настройки панели мониторинга ее можно опубликовать и использовать совместно с другими пользователями в организации. Вы можете предоставить другим пользователям доступ к панели мониторинга Azure с помощью [управления доступом на основе ролей](../role-based-access-control/role-assignments-portal.md). Роль назначается пользователю или группе пользователей. Она определяет, можно ли пользователям просматривать или изменять опубликованную панель мониторинга. 

Все опубликованные панели мониторинга реализуются как ресурсы Azure, то есть они существуют как управляемые элементы в рамках подписки и содержатся в группе ресурсов.  С точки зрения контроля доступа панели мониторинга ничем не отличаются от других ресурсов, например виртуальной машины и учетной записи хранения.

> [!TIP]
> Отдельные элементы на панели мониторинга принудительно применяют собственные требования к контролю доступа (в зависимости от отображаемых ими ресурсов).  Таким образом, можно создать панель мониторинга с более широкими возможностями совместного использования, по-прежнему обеспечивая защиту данных в отдельных элементах.
> 
> 

## <a name="understanding-access-control-for-dashboards"></a>Общие сведения о контроле доступа для панелей мониторинга
С помощью управления доступом на основе ролей пользователям можно назначать роли на трех разных уровнях:

* подписка
* resource group
* resource

Разрешения, которые вы назначаете, наследуются от подписки до ресурса. Опубликованная панель мониторинга — это ресурс. Значит, пользователям уже могут быть назначены роли для подписки, которая также относится к опубликованной панели мониторинга. 

Ниже приведен пример.  Допустим, у вас есть подписка Azure, и нескольким членам вашей группы назначены роли **владельца**, **участника** или **читателя** этой подписки. Пользователи, являющиеся владельцами или участниками, могут просматривать, создавать, изменять и удалять панели мониторинга в подписке, а также получать их списки.  Пользователи, являющиеся читателями, могут просматривать панели мониторинга и получать их списки, но не могут изменять или удалять их.  Пользователи с доступом для чтения могут вносить локальные изменения в опубликованную панель мониторинга (например, при устранении неполадок), но не могут публиковать эти изменения на сервере.  Они получат возможность создать закрытую копию панели мониторинга для собственных потребностей.

Однако можно назначать разрешения для группы ресурсов, содержащей несколько панелей мониторинга, или для отдельной панели мониторинга. Например, вы можете решить, что группа пользователей должна располагать ограниченными разрешениями в подписке, но иметь доступ более высокого уровня к определенной панели мониторинга. Таким образом, пользователям необходимо назначить роль для этой панели мониторинга. 

## <a name="publish-dashboard"></a>Публикация панели мониторинга
Предположим, что после настройки панели мониторинга вам необходимо предоставить группе пользователей в вашей подписке доступ к этой панели. В следующих шагах описывается настроенная группа, которая называется "Менеджеры хранилищ" (вы можете присвоить группе любое другое имя). Сведения о создании группы Active Directory и добавлении в нее пользователей см. в статье [Управление группами в Azure Active Directory](../active-directory/fundamentals/active-directory-groups-create-azure-portal.md).

1. На панели мониторинга выберите **Общий доступ**.
   
     ![выбрать "Общий доступ"](./media/azure-portal-dashboard-share-access/select-share.png)
2. Перед назначением доступа необходимо опубликовать панель мониторинга. По умолчанию панель мониторинга будет опубликована в группе ресурсов с именем **Панели мониторинга**. Нажмите кнопку **Опубликовать**.
   
     ![Опубликовать](./media/azure-portal-dashboard-share-access/publish.png)

Панель мониторинга опубликована. Если разрешения, унаследованные от подписки, подходят, больше ничего не нужно делать. Другие пользователи в организации смогут получать доступ к панели мониторинга и изменять ее на основе роли уровня подписки. Однако в целях этого руководства давайте назначим группе пользователей роль для этой панели мониторинга.

## <a name="assign-access-to-a-dashboard"></a>Назначение доступа к панели мониторинга
1. После публикации панели мониторинга щелкните **Управление пользователями**.
   
     ![Управление пользователями](./media/azure-portal-dashboard-share-access/manage-users.png)
2. Появится список имеющихся пользователей, которым уже назначена роль для этой панели мониторинга. Ваш список имеющихся пользователей будет отличаться от приведенного на рисунке ниже. Скорее всего, назначения будут унаследованы от подписки. Щелкните **Добавить**, чтобы добавить нового пользователя или группу.
   
     ![добавить пользователя](./media/azure-portal-dashboard-share-access/existing-users.png)
3. Выберите роль, соответствующую разрешениям, которые нужно предоставить. В этом примере щелкните **Участник**.
   
     ![выбрать роль](./media/azure-portal-dashboard-share-access/select-role.png)
4. Выберите пользователя или группу, которым нужно назначить роль. Если необходимых пользователя или группы нет в списке, используйте поле поиска. В списке доступных групп будут отображаться группы, созданные в Active Directory.
   
     ![выбрать пользователя](./media/azure-portal-dashboard-share-access/select-user.png) 
5. По завершении добавления пользователей или групп нажмите кнопку **ОК**. 
6. Новое назначение будет добавлено в список пользователей. Обратите внимание, что для **доступа** задано состояние **Назначенный**, а не **Унаследованный**.
   
     ![назначенные роли](./media/azure-portal-dashboard-share-access/assigned-roles.png)

## <a name="next-steps"></a>Дальнейшие действия
* Список ролей, см. в разделе [RBAC: для управления доступом на основе ролей в Azure](../role-based-access-control/built-in-roles.md).
* Сведения об управлении ресурсами см. в статье [Manage Azure resources through portal](resource-group-portal.md) (Управление ресурсами Azure через портал).

