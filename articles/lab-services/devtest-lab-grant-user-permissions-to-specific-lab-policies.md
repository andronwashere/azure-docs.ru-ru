---
title: Предоставление пользователю разрешений для определенных политик лаборатории | Документация Майкрософт
description: Узнайте, как предоставить пользователю разрешения для определенных политик лаборатории в DevTest Labs исходя из его потребностей.
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
editor: ''
ms.assetid: 5ca829f0-eb69-40a1-ae26-03a629db1d7e
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/17/2018
ms.author: spelluru
ms.openlocfilehash: 70469a9e8737a9df18628951a061c97081c74080
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "62127384"
---
# <a name="grant-user-permissions-to-specific-lab-policies"></a>Предоставление пользователю разрешений для определенных политик лаборатории
## <a name="overview"></a>Обзор
В этой статье рассказывается, как с помощью PowerShell предоставить пользователям разрешения для определенной политики лаборатории. Разрешения могут предоставляться в зависимости от потребностей каждого пользователя. Например, определенному пользователю можно разрешить изменить параметры политики виртуальной машины, но не политики затрат.

## <a name="policies-as-resources"></a>Политики как ресурсы
Как уже обсуждалось в одноименной статье, [управление доступом на основе ролей (RBAC)](../role-based-access-control/role-assignments-portal.md) обеспечивает точное управление доступом для Azure. С помощью RBAC вы можете распределить обязанности внутри команды разработчиков и предоставить пользователям доступ на том уровне, который им необходим для выполнения поставленных задач.

В DevTest Labs политика — это тип ресурса, который включает действие RBAC **Microsoft.DevTestLab/labs/policySets/policies/** . Каждая политика лаборатории представляет собой ресурс типа "Политика" и может быть назначена в качестве области действия для роли RBAC.

Например, чтобы предоставить пользователям разрешения на чтение/запись **размеры виртуальных Машин разрешен** политики, необходимо создать пользовательскую роль, которая работает с **Microsoft.DevTestLab/labs/policySets/policies/** действия , а затем назначить пользователей этой роли в рамках **Microsoft.DevTestLab/labs/policySets/policies/AllowedVmSizesInLab**.

Дополнительные сведения о пользовательских ролях в RBAC см. в статье [Пользовательские роли в Azure RBAC](../role-based-access-control/custom-roles.md).

## <a name="creating-a-lab-custom-role-using-powershell"></a>Создание пользовательской роли лаборатории с помощью PowerShell
Чтобы приступить к работе, вам потребуется [установите Azure PowerShell](/powershell/azure/install-az-ps). 

Настроив командлеты Azure PowerShell, вы сможете выполнять следующие задачи:

* Получать список всех операций и действий по тому или иному поставщику ресурсов.
* Получать список действий по определенной роли:
* Создание настраиваемой роли

Примеры выполнения этих задач демонстрирует следующий сценарий PowerShell:

    ‘List all the operations/actions for a resource provider.
    Get-AzProviderOperation -OperationSearchString "Microsoft.DevTestLab/*"

    ‘List actions in a particular role.
    (Get-AzRoleDefinition "DevTest Labs User").Actions

    ‘Create custom role.
    $policyRoleDef = (Get-AzRoleDefinition "DevTest Labs User")
    $policyRoleDef.Id = $null
    $policyRoleDef.Name = "Policy Contributor"
    $policyRoleDef.IsCustom = $true
    $policyRoleDef.AssignableScopes.Clear()
    $policyRoleDef.AssignableScopes.Add("/subscriptions/<SubscriptionID> ")
    $policyRoleDef.Actions.Add("Microsoft.DevTestLab/labs/policySets/policies/*")
    $policyRoleDef = (New-AzRoleDefinition -Role $policyRoleDef)

## <a name="assigning-permissions-to-a-user-for-a-specific-policy-using-custom-roles"></a>Предоставление пользователю разрешений в отношении определенной политики с помощью пользовательских ролей
Определив пользовательские роли, можно назначить их пользователям. Чтобы назначить пользовательскую роль, необходимо получить **ObjectId** соответствующего пользователя. Чтобы сделать это, используйте **Get AzADUser** командлета.

В следующем примере **ObjectId** пользователя *SomeUser* имеет значение 05DEFF7B-0AC3-4ABF-B74D-6A72CD5BF3F3.

    PS C:\>Get-AzADUser -SearchString "SomeUser"

    DisplayName                    Type                           ObjectId
    -----------                    ----                           --------
    someuser@hotmail.com                                          05DEFF7B-0AC3-4ABF-B74D-6A72CD5BF3F3

При наличии **ObjectId** для пользователя и имя пользовательской роли, можно назначить эту роль пользователю с **New AzRoleAssignment** командлета:

    PS C:\>New-AzRoleAssignment -ObjectId 05DEFF7B-0AC3-4ABF-B74D-6A72CD5BF3F3 -RoleDefinitionName "Policy Contributor" -Scope /subscriptions/<SubscriptionID>/resourceGroups/<ResourceGroupName>/providers/Microsoft.DevTestLab/labs/<LabName>/policySets/default/policies/AllowedVmSizesInLab

В предыдущем примере использовалась политика **AllowedVmSizesInLab** . Также можно использовать любую из следующих политик:

* MaxVmsAllowedPerUser
* MaxVmsAllowedPerLab
* AllowedVmSizesInLab
* LabVmsShutdown

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="next-steps"></a>Дальнейшие действия
После того как пользователю будут предоставлены разрешения для определенных политик лаборатории, можно выполнить следующие действия.

* [Безопасный доступ к лаборатории](devtest-lab-add-devtest-user.md)
* [Определение политик лаборатории](devtest-lab-set-lab-policy.md)
* [Создание шаблона лаборатории](devtest-lab-create-template.md)
* [Создание пользовательских артефактов для виртуальных машин](devtest-lab-artifact-author.md)
* [Добавление виртуальной машины в лабораторию](devtest-lab-add-vm.md)

