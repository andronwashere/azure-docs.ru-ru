---
title: Управление группами Azure Resource Manager с помощью портала Azure | Документация Майкрософт
description: Используйте портал Azure для управления группами Azure Resource Manager.
services: azure-resource-manager,azure-portal
documentationcenter: ''
author: mumian
ms.service: azure-resource-manager
ms.topic: conceptual
ms.date: 03/26/2019
ms.author: jgao
ms.openlocfilehash: bc3c1a05c64edea260bd177dd7eaefc003db5310
ms.sourcegitcommit: 2d3b1d7653c6c585e9423cf41658de0c68d883fa
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/20/2019
ms.locfileid: "67296295"
---
# <a name="manage-azure-resource-manager-resource-groups-by-using-the-azure-portal"></a>Управление группами ресурсов Azure Resource Manager с помощью портала Azure

Сведения об использовании [портала Azure](https://portal.azure.com) с [Azure Resource Manager](resource-group-overview.md) для управления группами ресурсов Azure. Для управления ресурсами Azure, см. в разделе [управление ресурсами Azure с помощью портала Azure](./manage-resources-portal.md).

Другие статьи об управлении группами ресурсов:

- [Управление группами ресурсов Azure с помощью Azure CLI](./manage-resources-cli.md)
- [Управление группами ресурсов Azure с помощью Azure PowerShell](./manage-resources-powershell.md)

[!INCLUDE [Handle personal data](../../includes/gdpr-intro-sentence.md)]

## <a name="what-is-a-resource-group"></a>Что такое группа ресурсов

Группа ресурсов — это контейнер, содержащий связанные ресурсы для решения Azure. В группу ресурсов могут входить все ресурсы приложения или только те, которыми необходимо управлять совместно. Кроме того, пользователи могут выбрать оптимальный для своей организации способ распределения ресурсов в группах ресурсов. Как правило, ресурсы с общим жизненным циклом добавляют в одну и ту же группу ресурсов, чтобы их можно было легко развертывать, обновлять и удалять в виде группы.

В группе ресурсов хранятся метаданные о ресурсах. Исходя из этого, при указании расположения группы ресурсов вы определяете расположение метаданных. В целях обеспечения соответствия необходимо убедиться, что данные хранятся в определенном регионе.

В группе ресурсов хранятся метаданные о ресурсах. При указании расположения группы ресурсов, вы указываете, где хранятся эти метаданные.

## <a name="create-resource-groups"></a>Создание группы ресурсов

1. Войдите на [портале Azure](https://portal.azure.com).
2. Выберите **групп ресурсов**

    ![добавление группы ресурсов](./media/manage-resource-groups-portal/manage-resource-groups-add-group.png)
3. Выберите **Добавить**.
4. Введите следующие значения.

   - **Подписка**: Выберите подписку Azure. 
   - **Группа ресурсов.** Введите имя новой группы ресурсов. 
   - **Регион**. Выберите расположение Azure, такие как **Центральная часть США**.

     ![Создать группу ресурсов](./media/manage-resource-groups-portal/manage-resource-groups-create-group.png)
5. Выберите **Просмотр и создание**
6. Нажмите кнопку **Создать**. Занимает несколько секунд, чтобы создать группу ресурсов.
7. Выберите **обновить** в верхнем меню, чтобы обновить список групп ресурсов и затем выберите группу вновь созданного ресурса, чтобы открыть его. Или выберите **уведомления**(значок колокольчика) от верхней, а затем выберите **перейти к группе ресурсов** открыть созданной группе ресурсов.

    ![Перейти к группе ресурсов](./media/manage-resource-groups-portal/manage-resource-groups-add-group-go-to-resource-group.png)

## <a name="list-resource-groups"></a>Перечислить группы ресурсов

1. Войдите на [портале Azure](https://portal.azure.com).
2. Чтобы получить список групп ресурсов, выберите **групп ресурсов**

    ![просмотр групп ресурсов](./media/manage-resource-groups-portal/manage-resource-groups-list-groups.png)

3. Чтобы настроить данные, отображаемые для группы ресурсов, выберите **изменить столбцы**. На следующем рисунке показаны столбцы сложения, можно добавить на экран:

## <a name="open-resource-groups"></a>Открыть ресурс группы

1. Войдите на [портале Azure](https://portal.azure.com).
2. Выберите **Группы ресурсов**.
3. Выберите группу ресурсов, которую требуется открыть.

## <a name="delete-resource-groups"></a>Удаление группы ресурсов

1. Откройте группу ресурсов, которую требуется удалить.  См. в разделе [откройте групп ресурсов](#open-resource-groups).
2. Выберите **Удалить группу ресурсов**.

    ![delete azure resource group](./media/manage-resource-groups-portal/delete-group.png)

Дополнительные сведения о как упорядочивает удаление ресурсов в Azure Resource Manager, см. в разделе [удаление группы ресурсов Azure Resource Manager](./resource-group-delete.md).

## <a name="deploy-resources-to-a-resource-group"></a>Развертывание ресурсов в группе ресурсов

После создания шаблона Resource Manager, можно использовать портал Azure для развертывания ресурсов Azure. Создание шаблона, см. в разделе [краткое руководство: по созданию и развертыванию шаблонов Azure Resource Manager с помощью портала Azure](./resource-manager-quickstart-create-templates-use-the-portal.md). Развертывание шаблона с помощью портала, см. в разделе [развертывание ресурсов с использованием шаблонов Resource Manager и портала Azure](resource-group-template-deploy-portal.md).

## <a name="move-to-another-resource-group-or-subscription"></a>Переместить в другую группу ресурсов или подписку

Ресурсы в группе можно переместить в другую группу ресурсов. Дополнительные сведения см. в статье [Перемещение ресурсов в новую группу ресурсов или подписку](resource-group-move-resources.md).

## <a name="lock-resource-groups"></a>Блокировки группы ресурсов

Блокировка запрещает другим пользователям в организации случайно удалить или изменить критически важные ресурсы, такие как подписки Azure, группу ресурсов или ресурс. 

1. Откройте группу ресурсов, которую требуется удалить.  См. в разделе [откройте групп ресурсов](#open-resource-groups).
2. В левой области выберите **блокирует**.
3. Чтобы добавить блокировку к группе ресурсов, выберите **добавить**.
4. Введите **имя блокировки**, **тип блокировки**, и **заметки**. Следующие типы блокировки **только для чтения**, и **удалить**.

    ![блокировки группы ресурсов azure](./media/manage-resource-groups-portal/manage-resource-groups-add-lock.png)

Дополнительные сведения см. в разделе [блокировка ресурсов для предотвращения непредвиденных изменений](./resource-group-lock-resources.md).

## <a name="tag-resource-groups"></a>Тег группы ресурсов

К ресурсам и их группам можно добавлять теги. Это позволяет логически их упорядочивать. Сведения см. в разделе [использование тегов для организации ресурсов в Azure](./resource-group-using-tags.md#portal).

## <a name="export-resource-groups-to-templates"></a>Экспорт групп ресурсов в шаблонах

Сведения об экспорте шаблонов см. в разделе [экспорта одной или нескольких ресурсов в шаблон - портал](export-template-portal.md).

## <a name="manage-access-to-resource-groups"></a>Управление доступом к группам ресурсов

[Управление доступом на основе ролей (RBAC)](../role-based-access-control/overview.md) — это способ управления доступом к ресурсам в Azure. Дополнительные сведения см. в статье [Управление доступом с помощью RBAC и портала Azure](../role-based-access-control/role-assignments-portal.md).

## <a name="next-steps"></a>Дальнейшие действия

- Чтобы узнать, Azure Resource Manager, см. в разделе [обзоре Azure Resource Manager](./resource-group-overview.md).
- Чтобы узнать синтаксис шаблона Resource Manager, см. в разделе [описание структуры и синтаксиса шаблонов Azure Resource Manager](./resource-group-authoring-templates.md).
- Чтобы научиться разрабатывать шаблоны, см. в разделе [пошаговые учебники](/azure/azure-resource-manager/).
- Чтобы просмотреть схемы шаблонов Azure Resource Manager, см. в разделе [Справочник по шаблонам](/azure/templates/).