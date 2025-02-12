---
title: Повышение прав доступа для управления всеми подписками Azure и группами управления | Документация Майкрософт
description: Сведения о том, как с помощью портала Azure или REST API повысить права доступа глобального администратора для управления всеми подписками и группами управления в Azure Active Directory.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: bagovind
ms.assetid: b547c5a5-2da2-4372-9938-481cb962d2d6
ms.service: role-based-access-control
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/02/2019
ms.author: rolyon
ms.reviewer: bagovind
ms.openlocfilehash: ede7037aabc85739ee47636f1390c15e0b0d1639
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "66158421"
---
# <a name="elevate-access-to-manage-all-azure-subscriptions-and-management-groups"></a>Повышение прав доступа для управления всеми подписками Azure и группами управления

Даже если вы являетесь глобальным администратором в Azure Active Directory (Azure AD), у вас может не быть права доступа ко всем подпискам и группам управления в вашем каталоге. В этой статье описываются способы, с помощью которых вы можете повысить права доступа для всех подписок и групп управления.

[!INCLUDE [gdpr-dsr-and-stp-note](../../includes/gdpr-dsr-and-stp-note.md)]

## <a name="why-would-you-need-to-elevate-your-access"></a>Для чего нужно повышение прав доступа

Если вы являетесь глобальным администратором, иногда могут возникать ситуации, требующие выполнения следующих задач:

- восстановление доступа к подписке Azure или группе управления, если пользователь потерял доступ;
- предоставление другому пользователю или себе доступа к подписке Azure или группе управления;
- просмотр всех подписок Azure или групп управления в организации;
- предоставление приложению автоматизации (например, приложению для выставления счетов или аудита) доступа ко всем подпискам Azure или группам управления.

## <a name="how-does-elevate-access-work"></a>Как работает повышение прав доступа

Azure AD и ресурсы Azure защищены независимо друг от друга. То есть назначения ролей Azure AD не предоставляют доступ к ресурсам Azure, а назначения ролей Azure не предоставляют доступ к Azure AD. Однако [глобальный администратор](../active-directory/users-groups-roles/directory-assign-admin-roles.md#company-administrator) в Azure AD может назначить себе права доступа для управления всеми подписками Azure и группами управления в вашем каталоге. Эту возможность следует использовать, если у вас нет доступа к ресурсам в подписке Azure, например к виртуальным машинам или учетным записям хранения, и вы хотите использовать свои привилегии глобального администратора для получения доступа к этим ресурсам.

При повышении прав доступа вам будет назначена роль [администратора доступа пользователей](built-in-roles.md#user-access-administrator) в Azure, охватывающая корень каталога (`/`). Это позволяет просматривать все ресурсы и назначать доступ в любой подписке или группе управления в каталоге. Назначения роли администратора доступа пользователей можно удалить с помощью PowerShell.

После внесения изменений, которые необходимо выполнить на уровне корня, следует удалить этот повышенный уровень доступа.

![Повышение прав доступа](./media/elevate-access-global-admin/elevate-access.png)

## <a name="azure-portal"></a>Портал Azure

Выполните следующие действия, чтобы повысить права доступа глобального администратора с помощью портала Azure.

1. Войдите на [портал Azure](https://portal.azure.com) или [центр администрирования Azure Active Directory](https://aad.portal.azure.com) как глобальный администратор.

1. В списке навигации выберите **Azure Active Directory**, а затем нажмите кнопку **Свойства**.

   ![Снимок экрана: свойства Azure AD](./media/elevate-access-global-admin/aad-properties.png)

1. В разделе **Управление доступом для ресурсов Azure** установите значение **Да**.

   ![Снимок экрана: управление доступом для ресурсов Azure](./media/elevate-access-global-admin/aad-properties-global-admin-setting.png)

   Когда вы задаете для переключателя значение **Да**, вам назначается роль администратора доступа пользователей в Azure RBAC, охватывающая корень каталога (/). Это предоставляет разрешение на назначение ролей во всех подписках и группах управления Azure, связанных с данным каталогом Azure AD. Этот переключатель доступен только пользователям, которым назначена роль глобального администратора в Azure AD.

   Если для переключателя задать значение **Нет**, роль администратора доступа пользователей в Azure RBAC будет удалена из вашей учетной записи. Вы больше не сможете назначать роли во всех подписках и группах управления Azure, связанных с данным каталогом Azure AD. Вы сможете просматривать (и управлять ими) только подписки и группы управления Azure, к которым вам был предоставлен доступ.

1. Нажмите кнопку **Сохранить**, чтобы сохранить настройки.

   Этот параметр не является глобальным свойством и применяется только к текущему выполнившему вход пользователю. Вы не можете повысить права доступа для всех членов роли глобального администратора.

1. Выйдите из системы и войдите снова, чтобы обновить доступ.

    Теперь у вас есть доступ ко всем подпискам и группам управления в своем каталоге. Вы заметите, что вам назначена роль администратора доступа пользователей в корневой области.

   ![Назначения ролей подписки с помощью корневой области. Снимок экрана](./media/elevate-access-global-admin/iam-root.png)

1. Выполните изменения, для которых требуются повышенные права доступа.

    Сведения о том, как назначить роли, см. в статье, посвященной [управлению доступом с помощью RBAC и портала Azure](role-assignments-portal.md). Если вы используете Azure AD Privileged Identity Management (PIM), ознакомьтесь со статьей [Обнаружение ресурсов Azure и управление ими с помощью управления привилегированными пользователями](../active-directory/privileged-identity-management/pim-resource-roles-discover-resources.md) и [Назначение ролей ресурсам Azure в PIM](../active-directory/privileged-identity-management/pim-resource-roles-assign-roles.md).

1. Когда вы закончите, установите переключатель **Управление доступом для ресурсов Azure** в положение **Нет**. Так как это параметр для отдельного пользователя, необходимо войти как тот же пользователь, который использовался для повышения доступа.

## <a name="azure-powershell"></a>Azure PowerShell

[!INCLUDE [az-powershell-update](../../includes/updated-for-az.md)]

### <a name="list-role-assignment-at-the-root-scope-"></a>Получение списка назначения ролей в корневой области (/)

Чтобы получить список назначения ролей администратора доступа пользователей для пользователя в области root (`/`), выполните команду [Get-AzRoleAssignment](/powershell/module/az.resources/get-azroleassignment).

```azurepowershell
Get-AzRoleAssignment | where {$_.RoleDefinitionName -eq "User Access Administrator" `
  -and $_.SignInName -eq "<username@example.com>" -and $_.Scope -eq "/"}
```

```Example
RoleAssignmentId   : /providers/Microsoft.Authorization/roleAssignments/098d572e-c1e5-43ee-84ce-8dc459c7e1f0
Scope              : /
DisplayName        : username
SignInName         : username@example.com
RoleDefinitionName : User Access Administrator
RoleDefinitionId   : 18d7d88d-d35e-4fb5-a5c3-7773c20a72d9
ObjectId           : d65fd0e9-c185-472c-8f26-1dafa01f72cc
ObjectType         : User
CanDelegate        : False
```

### <a name="remove-a-role-assignment-at-the-root-scope-"></a>Удаление назначения ролей в корневой области (/)

Чтобы удалить назначение роли администратора доступа пользователей для пользователя в области root (`/`), выполните следующие шаги:

1. Войдите в систему как пользователь, который может удалить повышенный уровень доступа. Это может быть тот же пользователь, который использовался для повышения прав доступа, или другой глобальный администратор с повышенными правами доступа в корневой области.


1. Чтобы удалить назначение роли администратора доступа пользователей, воспользуйтесь командой [Remove-AzRoleAssignment](/powershell/module/az.resources/remove-azroleassignment).

    ```azurepowershell
    Remove-AzRoleAssignment -SignInName <username@example.com> `
      -RoleDefinitionName "User Access Administrator" -Scope "/"
    ```

## <a name="rest-api"></a>REST API

### <a name="elevate-access-for-a-global-administrator"></a>Повышение прав доступа глобального администратора

Чтобы повысить права доступа глобального администратора с помощью REST API, выполните приведенные ниже основные действия.

1. С помощью REST вызовите действие `elevateAccess`, которое предоставит вам роль администратора доступа пользователей в области root (`/`).

   ```http
   POST https://management.azure.com/providers/Microsoft.Authorization/elevateAccess?api-version=2016-07-01
   ```

1. Создание [назначение роли](/rest/api/authorization/roleassignments), чтобы назначить любую роль в любой области. В следующем примере показаны свойства для назначения роли {roleDefinitionID} в области root (`/`).

   ```json
   { 
     "properties": {
       "roleDefinitionId": "providers/Microsoft.Authorization/roleDefinitions/{roleDefinitionID}",
       "principalId": "{objectID}",
       "scope": "/"
     },
     "id": "providers/Microsoft.Authorization/roleAssignments/64736CA0-56D7-4A94-A551-973C2FE7888B",
     "type": "Microsoft.Authorization/roleAssignments",
     "name": "64736CA0-56D7-4A94-A551-973C2FE7888B"
   }
   ```

1. Администратор доступа пользователей может также удалять назначения ролей в корневой области (`/`).

1. Удалите привилегии администратора доступа пользователей, пока они не понадобятся вновь.

### <a name="list-role-assignments-at-the-root-scope-"></a>Получение списка назначений ролей в корневой области (/)

Вы можете перечислить все назначения ролей для пользователя в корневой области (`/`).

- Вызовите [GET roleAssignments](/rest/api/authorization/roleassignments/listforscope), где `{objectIdOfUser}` — идентификатор объекта пользователя, чьи назначения роли вы хотите получить.

   ```http
   GET https://management.azure.com/providers/Microsoft.Authorization/roleAssignments?api-version=2015-07-01&$filter=principalId+eq+'{objectIdOfUser}'
   ```

### <a name="list-deny-assignments-at-the-root-scope-"></a>Вывод списка запретов назначений в корневой области (/)

Вы можете перечислить все запреты назначений для пользователя в корневой области (`/`).

- Вызовите GET denyAssignments, где `{objectIdOfUser}` — идентификатор объекта пользователя, запреты назначений которого вы хотите получить.

   ```http
   GET https://management.azure.com/providers/Microsoft.Authorization/denyAssignments?api-version=2018-07-01-preview&$filter=gdprExportPrincipalId+eq+'{objectIdOfUser}'
   ```

### <a name="remove-elevated-access"></a>Удаление повышенного права доступа

При вызове `elevateAccess` для вас создается назначение роли. Поэтому, чтобы отозвать эти привилегии, необходимо удалить назначение.

1. Вызовите [GET roleDefinitions](/rest/api/authorization/roledefinitions/get) (где `roleName` — это администратор доступа пользователей), чтобы определить GUID имени роли этого администратора.

    ```http
    GET https://management.azure.com/providers/Microsoft.Authorization/roleDefinitions?api-version=2015-07-01&$filter=roleName+eq+'User Access Administrator'
    ```

    ```json
    {
      "value": [
        {
          "properties": {
        "roleName": "User Access Administrator",
        "type": "BuiltInRole",
        "description": "Lets you manage user access to Azure resources.",
        "assignableScopes": [
          "/"
        ],
        "permissions": [
          {
            "actions": [
              "*/read",
              "Microsoft.Authorization/*",
              "Microsoft.Support/*"
            ],
            "notActions": []
          }
        ],
        "createdOn": "0001-01-01T08:00:00.0000000Z",
        "updatedOn": "2016-05-31T23:14:04.6964687Z",
        "createdBy": null,
        "updatedBy": null
          },
          "id": "/providers/Microsoft.Authorization/roleDefinitions/18d7d88d-d35e-4fb5-a5c3-7773c20a72d9",
          "type": "Microsoft.Authorization/roleDefinitions",
          "name": "18d7d88d-d35e-4fb5-a5c3-7773c20a72d9"
        }
      ],
      "nextLink": null
    }
    ```

    Сохраните ИД из параметра `name`. В этом случае — `18d7d88d-d35e-4fb5-a5c3-7773c20a72d9`.

2. Также необходимо вывести назначений ролей для администратора каталога в области каталога. Выведите список всех назначений в области каталога для `principalId` администратора каталога, совершившего вызов для повышения прав доступа. При этом будут перечислены все назначения в каталоге для objectid.

    ```http
    GET https://management.azure.com/providers/Microsoft.Authorization/roleAssignments?api-version=2015-07-01&$filter=principalId+eq+'{objectid}'
    ```
    
    >[!NOTE] 
    >У администратора каталога не должно быть много назначений. Если предыдущий запрос возвращает слишком много назначений, можно запросить все назначения только на уровне области каталога, а затем отфильтровать результаты: `GET https://management.azure.com/providers/Microsoft.Authorization/roleAssignments?api-version=2015-07-01&$filter=atScope()`.
        
   1. Предыдущие вызовы возвращают список назначения ролей. Найдите назначение роли, у которого задана область `"/"` и `roleDefinitionId` заканчивается значением ИД имени роли, найденным на шаге 1, а `principalId` совпадает со значением objectId администратора каталога. 
    
      Пример назначения ролей:

       ```json
       {
         "value": [
           {
             "properties": {
               "roleDefinitionId": "/providers/Microsoft.Authorization/roleDefinitions/18d7d88d-d35e-4fb5-a5c3-7773c20a72d9",
               "principalId": "{objectID}",
               "scope": "/",
               "createdOn": "2016-08-17T19:21:16.3422480Z",
               "updatedOn": "2016-08-17T19:21:16.3422480Z",
               "createdBy": "93ce6722-3638-4222-b582-78b75c5c6d65",
               "updatedBy": "93ce6722-3638-4222-b582-78b75c5c6d65"
             },
             "id": "/providers/Microsoft.Authorization/roleAssignments/e7dd75bc-06f6-4e71-9014-ee96a929d099",
             "type": "Microsoft.Authorization/roleAssignments",
             "name": "e7dd75bc-06f6-4e71-9014-ee96a929d099"
           }
         ],
         "nextLink": null
       }
       ```
        
      Опять же, сохраните ИД из параметра `name`. В данном случае это e7dd75bc-06f6-4e71-9014-ee96a929d099.

   1. Наконец, используйте ИД назначения роли, чтобы удалить назначение, добавленное `elevateAccess`:

      ```http
      DELETE https://management.azure.com/providers/Microsoft.Authorization/roleAssignments/e7dd75bc-06f6-4e71-9014-ee96a929d099?api-version=2015-07-01
      ```

## <a name="next-steps"></a>Дальнейшие действия

- [Сведения о различных ролях в Azure](rbac-and-directory-admin-roles.md)
- [Управление доступом к ресурсам Azure с помощью RBAC и REST API](role-assignments-rest.md)
