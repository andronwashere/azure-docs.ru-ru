---
title: Создание пользовательских ролей для ресурсов Azure с помощью Azure PowerShell | Документация Майкрософт
description: Узнайте, как создавать пользовательские роли с управлением доступом на основе ролей (RBAC) для ресурсов Azure с помощью Azure PowerShell. Здесь приводится описание процедур получения списка, создания, обновления и удаления пользовательских ролей.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.assetid: 9e225dba-9044-4b13-b573-2f30d77925a9
ms.service: role-based-access-control
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/20/2019
ms.author: rolyon
ms.reviewer: bagovind
ms.openlocfilehash: ad1185cab2b2bd2d0fea10f21b7859fd9ab1339f
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "66158450"
---
# <a name="create-custom-roles-for-azure-resources-using-azure-powershell"></a>Создание пользовательских ролей для ресурсов Azure с помощью Azure PowerShell

Если [встроенные роли для ресурсов Azure](built-in-roles.md) не соответствуют потребностям вашей организации, вы можете создать собственные пользовательские роли. В этой статье описывается создание пользовательских ролей и управление ими с помощью Azure PowerShell.

Пошаговые инструкции см. в статье [Руководство. Создание настраиваемой роли для ресурсов Azure с помощью Azure PowerShell](tutorial-custom-role-powershell.md).

[!INCLUDE [az-powershell-update](../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Технические условия

Для создания пользовательских ролей в Azure требуются:

- разрешения на создание пользовательских ролей, такие как [Владелец](built-in-roles.md#owner) или [Администратор доступа пользователя](built-in-roles.md#user-access-administrator);
- [Azure Cloud Shell](../cloud-shell/overview.md) или [Azure PowerShell](/powershell/azure/install-az-ps).

## <a name="list-custom-roles"></a>Вывод списка настраиваемых ролей

Чтобы получить список ролей, доступных для назначения в области, используется команда [Get-AzRoleDefinition](/powershell/module/az.resources/get-azroledefinition). В следующем примере перечисляются все роли, доступные для назначения в выбранной подписке.

```azurepowershell
Get-AzRoleDefinition | FT Name, IsCustom
```

```Example
Name                                              IsCustom
----                                              --------
Virtual Machine Operator                              True
AcrImageSigner                                       False
AcrQuarantineReader                                  False
AcrQuarantineWriter                                  False
API Management Service Contributor                   False
...
```

В следующем примере перечислены только пользовательские роли, доступные для назначения в выбранной подписке.

```azurepowershell
Get-AzRoleDefinition | ? {$_.IsCustom -eq $true} | FT Name, IsCustom
```

```Example
Name                     IsCustom
----                     --------
Virtual Machine Operator     True
```

Если выбранная подписка не указана в `AssignableScopes` роли, пользовательская роль не будет отображаться.

## <a name="list-a-custom-role-definition"></a>Получение списка определений пользовательской роли

Чтобы получить список определение пользовательской роли, используйте [Get-AzRoleDefinition](/powershell/module/az.resources/get-azroledefinition). Это тот же командлет, как использовать встроенные роли.

```azurepowershell
Get-AzRoleDefinition <role name> | ConvertTo-Json
```

```Example
PS C:\> Get-AzRoleDefinition "Virtual Machine Operator" | ConvertTo-Json

{
  "Name": "Virtual Machine Operator",
  "Id": "00000000-0000-0000-0000-000000000000",
  "IsCustom": true,
  "Description": "Can monitor and restart virtual machines.",
  "Actions": [
    "Microsoft.Storage/*/read",
    "Microsoft.Network/*/read",
    "Microsoft.Compute/*/read",
    "Microsoft.Compute/virtualMachines/start/action",
    "Microsoft.Compute/virtualMachines/restart/action",
    "Microsoft.Authorization/*/read",
    "Microsoft.ResourceHealth/availabilityStatuses/read",
    "Microsoft.Resources/subscriptions/resourceGroups/read",
    "Microsoft.Insights/alertRules/*",
    "Microsoft.Support/*"
  ],
  "NotActions": [],
  "DataActions": [],
  "NotDataActions": [],
  "AssignableScopes": [
    "/subscriptions/11111111-1111-1111-1111-111111111111"
  ]
}
```

В следующем примере перечисляются только действия роли:

```azurepowershell
(Get-AzRoleDefinition <role name>).Actions
```

```Example
PS C:\> (Get-AzRoleDefinition "Virtual Machine Operator").Actions

"Microsoft.Storage/*/read",
"Microsoft.Network/*/read",
"Microsoft.Compute/*/read",
"Microsoft.Compute/virtualMachines/start/action",
"Microsoft.Compute/virtualMachines/restart/action",
"Microsoft.Authorization/*/read",
"Microsoft.ResourceHealth/availabilityStatuses/read",
"Microsoft.Resources/subscriptions/resourceGroups/read",
"Microsoft.Insights/alertRules/*",
"Microsoft.Insights/diagnosticSettings/*",
"Microsoft.Support/*"
```

## <a name="create-a-custom-role"></a>Создание настраиваемой роли

Чтобы создать пользовательскую роль, используется команда [New-AzRoleDefinition](/powershell/module/az.resources/new-azroledefinition). Существует два способа структурирования роли: с помощью объекта `PSRoleDefinition` или шаблона JSON. 

### <a name="get-operations-for-a-resource-provider"></a>Получение перечня операций, доступных поставщику ресурсов

При создании пользовательских ролей важно знать все возможные операции, которые могут осуществлять поставщики ресурсов.
Вы можете просмотреть список [операций, доступных поставщику ресурсов](resource-provider-operations.md), или воспользоваться командой [Get-AzProviderOperation](/powershell/module/az.resources/get-azprovideroperation) для получения этой информации.
Например, если нужно проверить все доступные операции для виртуальных машин, используйте следующую команду.

```azurepowershell
Get-AzProviderOperation <operation> | FT OperationName, Operation, Description -AutoSize
```

```Example
PS C:\> Get-AzProviderOperation "Microsoft.Compute/virtualMachines/*" | FT OperationName, Operation, Description -AutoSize

OperationName                                  Operation                                                      Description
-------------                                  ---------                                                      -----------
Get Virtual Machine                            Microsoft.Compute/virtualMachines/read                         Get the propertie...
Create or Update Virtual Machine               Microsoft.Compute/virtualMachines/write                        Creates a new vir...
Delete Virtual Machine                         Microsoft.Compute/virtualMachines/delete                       Deletes the virtu...
Start Virtual Machine                          Microsoft.Compute/virtualMachines/start/action                 Starts the virtua...
...
```

### <a name="create-a-custom-role-with-the-psroledefinition-object"></a>Создание пользовательской роли с помощью объекта PSRoleDefinition

При использовании PowerShell для создания пользовательской роли можно использовать одну из [встроенных ролей](built-in-roles.md) в качестве отправной точки, а можно начать и с нуля. Первый пример в этом разделе начинается со встроенной роли, и затем производится ее настройка для добавления дополнительных разрешений. Измените атрибуты для добавления нужных параметров (`Actions`, `NotActions` или `AssignableScopes`), а затем сохраните изменения в качестве новой роли.

В следующем примере сначала используется встроенная роль [Участник виртуальной машины](built-in-roles.md#virtual-machine-contributor) для создания пользовательской роли под названием *Оператор виртуальной машины*. Новая роль предоставляет доступ ко всем операциям чтения поставщиков ресурсов *Microsoft.Compute*, *Microsoft.Storage* и *Microsoft.Network*, а также доступ для запуска, перезапуска и мониторинга виртуальных машин. Настраиваемую роль можно использовать в двух подписках.

```azurepowershell
$role = Get-AzRoleDefinition "Virtual Machine Contributor"
$role.Id = $null
$role.Name = "Virtual Machine Operator"
$role.Description = "Can monitor and restart virtual machines."
$role.Actions.Clear()
$role.Actions.Add("Microsoft.Storage/*/read")
$role.Actions.Add("Microsoft.Network/*/read")
$role.Actions.Add("Microsoft.Compute/*/read")
$role.Actions.Add("Microsoft.Compute/virtualMachines/start/action")
$role.Actions.Add("Microsoft.Compute/virtualMachines/restart/action")
$role.Actions.Add("Microsoft.Authorization/*/read")
$role.Actions.Add("Microsoft.ResourceHealth/availabilityStatuses/read")
$role.Actions.Add("Microsoft.Resources/subscriptions/resourceGroups/read")
$role.Actions.Add("Microsoft.Insights/alertRules/*")
$role.Actions.Add("Microsoft.Support/*")
$role.AssignableScopes.Clear()
$role.AssignableScopes.Add("/subscriptions/00000000-0000-0000-0000-000000000000")
$role.AssignableScopes.Add("/subscriptions/11111111-1111-1111-1111-111111111111")
New-AzRoleDefinition -Role $role
```

В следующем примере показан другой способ создания пользовательской роли *Оператор виртуальной машины*. Сначала создается новый объект `PSRoleDefinition`. Действия операции указаны в переменной `perms`, для которой задано свойство `Actions`. Свойство `NotActions` устанавливается путем считывания `NotActions` из встроенной роли [Оператор виртуальной машины](built-in-roles.md#virtual-machine-contributor). Так как в роли [Оператор виртуальной машины](built-in-roles.md#virtual-machine-contributor) нет `NotActions`, эта строка необязательна, но в ней показано, как можно получить сведения из другой роли.

```azurepowershell
$role = [Microsoft.Azure.Commands.Resources.Models.Authorization.PSRoleDefinition]::new()
$role.Name = 'Virtual Machine Operator 2'
$role.Description = 'Can monitor and restart virtual machines.'
$role.IsCustom = $true
$perms = 'Microsoft.Storage/*/read','Microsoft.Network/*/read','Microsoft.Compute/*/read'
$perms += 'Microsoft.Compute/virtualMachines/start/action','Microsoft.Compute/virtualMachines/restart/action'
$perms += 'Microsoft.Authorization/*/read'
$perms += 'Microsoft.ResourceHealth/availabilityStatuses/read'
$perms += 'Microsoft.Resources/subscriptions/resourceGroups/read'
$perms += 'Microsoft.Insights/alertRules/*','Microsoft.Support/*'
$role.Actions = $perms
$role.NotActions = (Get-AzRoleDefinition -Name 'Virtual Machine Contributor').NotActions
$subs = '/subscriptions/00000000-0000-0000-0000-000000000000','/subscriptions/11111111-1111-1111-1111-111111111111'
$role.AssignableScopes = $subs
New-AzRoleDefinition -Role $role
```

### <a name="create-a-custom-role-with-json-template"></a>Создание пользовательской роли с помощью шаблона JSON

Шаблон JSON может использоваться в качестве определения источника для пользовательской роли. В следующем примере создается пользовательская роль, которая разрешает доступ на чтение к хранилищу и вычислительным ресурсам, доступ для поддержки, и добавляет эту роль к двум подпискам. Создайте файл `C:\CustomRoles\customrole1.json` с приведенным ниже содержимым. При первичном создании роли идентификатору должно быть присвоено значение `null`, так как новый идентификатор создается автоматически. 

```json
{
  "Name": "Custom Role 1",
  "Id": null,
  "IsCustom": true,
  "Description": "Allows for read access to Azure storage and compute resources and access to support",
  "Actions": [
    "Microsoft.Compute/*/read",
    "Microsoft.Storage/*/read",
    "Microsoft.Support/*"
  ],
  "NotActions": [],
  "AssignableScopes": [
    "/subscriptions/00000000-0000-0000-0000-000000000000",
    "/subscriptions/11111111-1111-1111-1111-111111111111"
  ]
}
```

Чтобы добавить роль к подпискам, выполните следующую команду PowerShell.

```azurepowershell
New-AzRoleDefinition -InputFile "C:\CustomRoles\customrole1.json"
```

## <a name="update-a-custom-role"></a>Обновление пользовательской роли

Аналогично созданию пользовательской роли можно изменить имеющуюся пользовательскую роль с помощью либо объекта `PSRoleDefinition`, либо шаблона JSON.

### <a name="update-a-custom-role-with-the-psroledefinition-object"></a>Обновление пользовательской роли с помощью объекта PSRoleDefinition

Чтобы изменить пользовательскую роль, сначала используется команда [Get-AzRoleDefinition](/powershell/module/az.resources/get-azroledefinition) для получения определения роли. Затем внесите необходимые изменения в определение роли. Наконец, с помощью команды [Set-AzRoleDefinition](/powershell/module/az.resources/set-azroledefinition) сохраняется измененное определение роли.

В следующем примере показано добавление операции `Microsoft.Insights/diagnosticSettings/*` к настраиваемой роли *Оператор виртуальной машины* .

```azurepowershell
$role = Get-AzRoleDefinition "Virtual Machine Operator"
$role.Actions.Add("Microsoft.Insights/diagnosticSettings/*")
Set-AzRoleDefinition -Role $role
```

```Example
PS C:\> $role = Get-AzRoleDefinition "Virtual Machine Operator"
PS C:\> $role.Actions.Add("Microsoft.Insights/diagnosticSettings/*")
PS C:\> Set-AzRoleDefinition -Role $role

Name             : Virtual Machine Operator
Id               : 88888888-8888-8888-8888-888888888888
IsCustom         : True
Description      : Can monitor and restart virtual machines.
Actions          : {Microsoft.Storage/*/read, Microsoft.Network/*/read, Microsoft.Compute/*/read,
                   Microsoft.Compute/virtualMachines/start/action...}
NotActions       : {}
AssignableScopes : {/subscriptions/00000000-0000-0000-0000-000000000000,
                   /subscriptions/11111111-1111-1111-1111-111111111111}
```

В следующем примере показано добавление подписки Azure в назначаемые области настраиваемой роли *Оператор виртуальной машины* .

```azurepowershell
Get-AzSubscription -SubscriptionName Production3

$role = Get-AzRoleDefinition "Virtual Machine Operator"
$role.AssignableScopes.Add("/subscriptions/22222222-2222-2222-2222-222222222222")
Set-AzRoleDefinition -Role $role
```

```Example
PS C:\> Get-AzSubscription -SubscriptionName Production3

Name     : Production3
Id       : 22222222-2222-2222-2222-222222222222
TenantId : 99999999-9999-9999-9999-999999999999
State    : Enabled

PS C:\> $role = Get-AzRoleDefinition "Virtual Machine Operator"
PS C:\> $role.AssignableScopes.Add("/subscriptions/22222222-2222-2222-2222-222222222222")
PS C:\> Set-AzRoleDefinition -Role $role

Name             : Virtual Machine Operator
Id               : 88888888-8888-8888-8888-888888888888
IsCustom         : True
Description      : Can monitor and restart virtual machines.
Actions          : {Microsoft.Storage/*/read, Microsoft.Network/*/read, Microsoft.Compute/*/read,
                   Microsoft.Compute/virtualMachines/start/action...}
NotActions       : {}
AssignableScopes : {/subscriptions/00000000-0000-0000-0000-000000000000,
                   /subscriptions/11111111-1111-1111-1111-111111111111,
                   /subscriptions/22222222-2222-2222-2222-222222222222}
```

### <a name="update-a-custom-role-with-a-json-template"></a>Обновление пользовательской роли с помощью шаблона JSON

С помощью предыдущего шаблона JSON можно легко изменить существующую пользовательскую роль, чтобы добавить или удалить действия. Обновите шаблон JSON и добавьте действие чтения для сетевых подключений, как показано в следующем примере. Определения, перечисленные в шаблоне, не применяются все вместе к существующему определению. Это означает, что роль отображается так, как указано в шаблоне. Кроме того, необходимо обновить поле идентификатора с помощью идентификатора роли. Если вы не уверены, каким является это значение, для получения этой информации можно использовать командлет [Get-AzRoleDefinition](/powershell/module/az.resources/get-azroledefinition).

```json
{
  "Name": "Custom Role 1",
  "Id": "acce7ded-2559-449d-bcd5-e9604e50bad1",
  "IsCustom": true,
  "Description": "Allows for read access to Azure storage and compute resources and access to support",
  "Actions": [
    "Microsoft.Compute/*/read",
    "Microsoft.Storage/*/read",
    "Microsoft.Network/*/read",
    "Microsoft.Support/*"
  ],
  "NotActions": [],
  "AssignableScopes": [
    "/subscriptions/00000000-0000-0000-0000-000000000000",
    "/subscriptions/11111111-1111-1111-1111-111111111111"
  ]
}
```

Чтобы обновить существующую роль, выполните следующую команду PowerShell.

```azurepowershell
Set-AzRoleDefinition -InputFile "C:\CustomRoles\customrole1.json"
```

## <a name="delete-a-custom-role"></a>Удаление настраиваемой роли

Чтобы удалить пользовательскую роль, используется команда [Remove-AzRoleDefinition](/powershell/module/az.resources/remove-azroledefinition).

В следующем примере показано удаление настраиваемой роли *Оператор виртуальной машины* .

```azurepowershell
Get-AzRoleDefinition "Virtual Machine Operator"
Get-AzRoleDefinition "Virtual Machine Operator" | Remove-AzRoleDefinition
```

```Example
PS C:\> Get-AzRoleDefinition "Virtual Machine Operator"

Name             : Virtual Machine Operator
Id               : 88888888-8888-8888-8888-888888888888
IsCustom         : True
Description      : Can monitor and restart virtual machines.
Actions          : {Microsoft.Storage/*/read, Microsoft.Network/*/read, Microsoft.Compute/*/read,
                   Microsoft.Compute/virtualMachines/start/action...}
NotActions       : {}
AssignableScopes : {/subscriptions/00000000-0000-0000-0000-000000000000,
                   /subscriptions/11111111-1111-1111-1111-111111111111}

PS C:\> Get-AzRoleDefinition "Virtual Machine Operator" | Remove-AzRoleDefinition

Confirm
Are you sure you want to remove role definition with name 'Virtual Machine Operator'.
[Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"): Y
```

## <a name="next-steps"></a>Дальнейшие действия

- [Учебник. Создание пользовательской роли для ресурсов Azure с помощью Azure PowerShell](tutorial-custom-role-powershell.md)
- [Пользовательские роли для ресурсов Azure](custom-roles.md)
- [Операции поставщиков ресурсов Azure Resource Manager](resource-provider-operations.md)
