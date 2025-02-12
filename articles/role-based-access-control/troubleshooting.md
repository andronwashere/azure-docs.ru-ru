---
title: Устранение неполадок RBAC для ресурсов Azure | Документация Майкрософт
description: Устранение неполадок с управлением доступом на основе ролей (RBAC) для ресурсов Azure.
services: azure-portal
documentationcenter: na
author: rolyon
manager: mtillman
ms.assetid: df42cca2-02d6-4f3c-9d56-260e1eb7dc44
ms.service: role-based-access-control
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 06/12/2019
ms.author: rolyon
ms.reviewer: bagovind
ms.custom: seohack1
ms.openlocfilehash: 59ece9c37a563efba6329a30c06c1b596b1a5d57
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "67058159"
---
# <a name="troubleshoot-rbac-for-azure-resources"></a>Устранение неполадок RBAC в Azure

В этой статье содержатся ответы на часто задаваемые вопросы об управлении доступом на основе ролей (RBAC) для ресурсов Azure. Вы узнаете, какие ситуации возможны при использовании ролей на портале Azure, а также сможете устранить неполадки, связанные с доступом.

## <a name="problems-with-rbac-role-assignments"></a>Проблемы с назначениями ролей в RBAC

- Если вы не сможете добавить назначение роли на портале Azure на **управление доступом (IAM)** поскольку **добавить** > **добавить назначение роли** может быть отключена или Поскольку возникает ошибка разрешения «клиент с идентификатором объекта не авторизован для выполнения действия», проверьте, что вы уже вошли учетную запись пользователя, которой назначены роли, который имеет `Microsoft.Authorization/roleAssignments/write` разрешения, такие как [владельца](built-in-roles.md#owner) или [администратор доступа пользователей](built-in-roles.md#user-access-administrator) в области видимости, вы пытаетесь назначить роль.
- Если вы получаете сообщение об ошибке «можно создать дополнительные назначения ролей (код: RoleAssignmentLimitExceeded)", попробуйте уменьшить число назначений ролей, назначив роли группам. Azure поддерживает до **2000** назначений ролей на подписку.

## <a name="problems-with-custom-roles"></a>Проблемы с пользовательскими ролями

- Шаги по созданию пользовательской роли, см. в статье в учебниках пользовательской роли с помощью [Azure PowerShell](tutorial-custom-role-powershell.md) или [Azure CLI](tutorial-custom-role-cli.md).
- Если не удается обновить существующую пользовательскую роль, проверьте, что вы уже вошли учетную запись пользователя, которой назначены роли, который имеет `Microsoft.Authorization/roleDefinition/write` разрешения, такие как [владельца](built-in-roles.md#owner) или [администратор доступа пользователей](built-in-roles.md#user-access-administrator).
- Если не удается удалить пользовательскую роль и отображается сообщение об ошибке "Существуют назначения ролей, которые ссылаются на роль (код: RoleDefinitionHasAssignments)", это означает, что все еще есть назначения этой пользовательской роли. Удалите эти назначения и снова попытайтесь удалить пользовательскую роль.
- Если при попытке создать пользовательскую роль отображается сообщение об ошибке "Превышено ограничение на определение ролей. Можно создать дополнительные определения ролей (код: RoleDefinitionLimitExceeded)» при попытке создать новую настраиваемую роль, удалить все пользовательские роли, которые не используются. Azure поддерживает до **5000** пользовательские роли в клиенте. (Специализированные облачные службы, такие как Azure для государственных организаций, Azure для Германии и Azure для Китая от 21Vianet поддерживают до 2000 пользовательских ролей.)
- Если отобразится сообщение об ошибке аналогичную «у клиента есть разрешение на выполнение действия «Microsoft.Authorization/roleDefinitions/write» в области «/ subscriptions / {subscriptionid}», однако связанная подписка не найден» при попытке обновить пользовательскую роль, проверка является ли один или несколько [назначаемых областей](role-definitions.md#assignablescopes) были удалены в клиенте. Если область удалена, создайте запрос в службу поддержки, так как способа самостоятельного решения этой проблемы пока не существует.

## <a name="recover-rbac-when-subscriptions-are-moved-across-tenants"></a>Восстановление RBAC при перемещении подписок между клиентами

- Если вам требуется способ передать подписку в другой Azure AD клиента, см. в разделе [передача прав владения подпиской Azure другой учетной записи](../billing/billing-subscription-transfer.md).
- Когда вы переносите подписку в другой клиент Azure AD, все назначения ролей удаляются из исходного клиента Azure AD без возможности восстановления и не переносятся в целевой клиент Azure AD. Необходимо повторно создать назначения ролей в целевом клиенте. Вам также потребуется вручную создавать управляемые удостоверения для ресурсов Azure. Дополнительные сведения см. в разделе [часто задаваемые вопросы и известные проблемы с управляемые удостоверения](../active-directory/managed-identities-azure-resources/known-issues.md).
- При наличии Azure AD глобального администратора и вы не имеют доступа к подписке, после его перемещения между клиентами, используйте **управление доступом для ресурсов Azure** переключатель, чтобы временно [повышение прав доступа](elevate-access-global-admin.md) для получения доступа к подписке.

## <a name="issues-with-service-admins-or-co-admins"></a>Проблемы с администраторами или соадминистраторами служб

- Если возникают проблемы с администратором служб или соадминистраторы, см. в разделе [Добавление или изменение администраторов подписки Azure](../billing/billing-add-change-azure-subscription-administrator.md) и [роли администратора классической подписки, роли Azure RBAC и Azure AD роли администратора](rbac-and-directory-admin-roles.md).

## <a name="access-denied-or-permission-errors"></a>Отказано в доступе или разрешение ошибок

- Если при попытке создать ресурс появляется сообщение об ошибке разрешения "Клиент с идентификатором объекта не авторизован для выполнения действия с областью (код: AuthorizationFailed)", убедитесь, что вы вошли как пользователь, которому назначена роль с разрешением на запись для ресурса в выбранной области. Например, для управления виртуальными машинами в группе ресурсов вам должна быть назначена роль [Участник виртуальных машин](built-in-roles.md#virtual-machine-contributor) для этой группы ресурсов (или вышестоящего уровня). Список разрешений для каждой встроенной роли см. в статье [Built-in roles for Azure resources](built-in-roles.md) (Встроенные роли для ресурсов Azure).
- Если возникает ошибка разрешения «У вас нет разрешения на создание запроса на поддержку» при попытке создать или обновить запрос в службу поддержки, проверьте, что вы уже вошли учетную запись пользователя, которой назначены роли, который имеет `Microsoft.Support/supportTickets/write` разрешение, например [Поддерживают участник-отправитель запроса](built-in-roles.md#support-request-contributor).

## <a name="role-assignments-without-a-security-principal"></a>Назначения ролей без субъект безопасности

Если вывести список назначений ролей с помощью Azure PowerShell, может появиться назначений с пустым `DisplayName` и `ObjectType` неизвестно. Например [Get AzRoleAssignment](/powershell/module/az.resources/get-azroleassignment) возвращает назначение ролей, аналогичную следующей:

```azurepowershell
RoleAssignmentId   : /subscriptions/11111111-1111-1111-1111-111111111111/providers/Microsoft.Authorization/roleAssignments/22222222-2222-2222-2222-222222222222
Scope              : /subscriptions/11111111-1111-1111-1111-111111111111
DisplayName        :
SignInName         :
RoleDefinitionName : Storage Blob Data Contributor
RoleDefinitionId   : ba92f5b4-2d11-453d-a403-e96b0029c9fe
ObjectId           : 33333333-3333-3333-3333-333333333333
ObjectType         : Unknown
CanDelegate        : False
```

Аналогичным образом, если вывести список назначений ролей с помощью Azure CLI, может появиться назначений с пустым `principalName`. Например [список назначений ролей az](/cli/azure/role/assignment#az-role-assignment-list) возвращает назначение ролей, аналогичную следующей:

```azurecli
{
    "canDelegate": null,
    "id": "/subscriptions/11111111-1111-1111-1111-111111111111/providers/Microsoft.Authorization/roleAssignments/22222222-2222-2222-2222-222222222222",
    "name": "22222222-2222-2222-2222-222222222222",
    "principalId": "33333333-3333-3333-3333-333333333333",
    "principalName": "",
    "roleDefinitionId": "/subscriptions/11111111-1111-1111-1111-111111111111/providers/Microsoft.Authorization/roleDefinitions/ba92f5b4-2d11-453d-a403-e96b0029c9fe",
    "roleDefinitionName": "Storage Blob Data Contributor",
    "scope": "/subscriptions/11111111-1111-1111-1111-111111111111",
    "type": "Microsoft.Authorization/roleAssignments"
}
```

Эти назначения ролей возникают, если назначение роли субъекту безопасности (пользователя, группы, субъекта-службы или управляемого удостоверения) и затем удалить этого участника безопасности. Эти назначения ролей не отображаются на портале Azure, и он не является проблемой, оставить их. Однако при желании можно удалить эти назначения ролей.

Чтобы удалить эти назначения ролей, используйте [Remove-AzRoleAssignment](/powershell/module/az.resources/remove-azroleassignment) или [удалить назначение роли az](/cli/azure/role/assignment#az-role-assignment-delete) команды.

В PowerShell при попытке удалить назначения ролей, используя идентификатор объекта и имя определения роли и более чем одно назначение ролей сопоставляет параметры, вы получите сообщение об ошибке: «Предоставленной информации не сопоставляется назначения ролей». Ниже приведен пример сообщения об ошибке:

```Example
PS C:\> Remove-AzRoleAssignment -ObjectId 33333333-3333-3333-3333-333333333333 -RoleDefinitionName "Storage Blob Data Contributor"

Remove-AzRoleAssignment : The provided information does not map to a role assignment.
At line:1 char:1
+ Remove-AzRoleAssignment -ObjectId 33333333-3333-3333-3333-333333333333 ...
+ ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
+ CategoryInfo          : CloseError: (:) [Remove-AzRoleAssignment], KeyNotFoundException
+ FullyQualifiedErrorId : Microsoft.Azure.Commands.Resources.RemoveAzureRoleAssignmentCommand
```

Если вы получаете это сообщение об ошибке, убедитесь, что можно также указать `-Scope` или `-ResourceGroupName` параметров.

```Example
PS C:\> Remove-AzRoleAssignment -ObjectId 33333333-3333-3333-3333-333333333333 -RoleDefinitionName "Storage Blob Data Contributor" - Scope /subscriptions/11111111-1111-1111-1111-111111111111
```

## <a name="rbac-changes-are-not-being-detected"></a>Не удается обнаружить изменения RBAC

Azure Resource Manager иногда кэширует конфигурации и данные для повышения производительности. При создании или удалении назначения ролей вступление изменений в силу может занять до 30 минут. Если вы используете портал Azure, Azure PowerShell или Azure CLI, можно принудительно обновить изменения назначения ролей, выйдя из учетной записи и повторно войдя в нее. Если вы вносите изменения в назначения ролей с помощью вызовов REST API, можно принудительно обновить изменения, обновив маркер доступа.

## <a name="web-app-features-that-require-write-access"></a>Компоненты веб-приложений, требующие доступа для записи

Если предоставить пользователю доступ только для чтения к одному веб-приложению, некоторые компоненты могут неожиданно отключиться. Перечисленные ниже возможности управления требуют доступа к веб-приложению с правом **записи** (роль участника или владельца), поэтому они недоступны при наличии прав только на чтение.

* Команды (такие как запуск, остановка и т. д.).
* Изменение параметров, таких как общие параметры конфигурации, параметры масштабирования, резервного копирования и мониторинга.
* Доступ к учетным данным для публикации и другим секретным сведениям, например к параметрам приложений и строкам подключения.
* Журналы потоковой передачи.
* Конфигурация журналов диагностики.
* Консоль (командная строка).
* Активные и недавние развертывания (для локального непрерывного развертывания Git).
* Приблизительные затраты.
* Веб-тесты
* Виртуальная сеть (видна читателю только в том случае, если ранее была настроена пользователем с доступом на запись).

Если у вас нет доступа ни к одной из этих плиток, попросите администратора предоставить вам доступ к веб-приложению с правами участника.

## <a name="web-app-resources-that-require-write-access"></a>Ресурсы веб-приложений, требующие доступа для записи

Работа с веб-приложениями осложняется наличием нескольких взаимосвязанных ресурсов. Вот типичная группа ресурсов, связанная с несколькими веб-сайтами:

![Группа ресурсов веб-приложений](./media/troubleshooting/website-resource-model.png)

В результате, если вы предоставите кому-либо доступ только к веб-приложению, многие функции в колонке веб-сайта на портале Azure будут отключены.

Для этих элементов требуется доступ на **запись** к **плану службы приложений**, который соответствует вашему веб-сайту:  

* просмотр ценовой категории веб-приложения (например, "Бесплатный" или "Стандартный");  
* параметры масштабирования (число экземпляров, размер виртуальной машины, настройки автоматического масштабирования);  
* квоты (квоты хранилища, пропускной способности, ресурсов ЦП).  

Элементы, требующие доступа на **запись** ко всей **группе ресурсов**, которая содержит веб-сайт:  

* SSL-сертификаты и привязки (SSL-сертификаты могут совместно использоваться сайтами, относящимися к одной группе ресурсов и находящимися в одном географическом расположении);  
* правила оповещений.  
* параметры автоматического масштабирования;  
* компоненты Application Insights;  
* Веб-тесты  

## <a name="virtual-machine-features-that-require-write-access"></a>Компоненты виртуальных машин, требующие доступа для записи

Как и в случае с веб-приложениями, для некоторых функций в колонке виртуальной машины нужен доступ к виртуальной машине или другим ресурсам в группе ресурсов с правами на запись.

Виртуальные машины связаны с доменными именами, виртуальными сетями, учетными записями хранения и правилами оповещений.

Элементы, требующие доступа к **виртуальной** машине с правом **записи**:

* Конечные точки  
* IP-адреса  
* диски;  
* Расширения  

Элементы, требующие доступа на **запись** как к **виртуальной машине**, так и к **группе ресурсов**, в которой она находится (а также к доменному имени):  

* Группа доступности  
* набор балансировки нагрузки;  
* правила оповещений.  

Если у вас нет доступа ни к одному из этих элементов, попросите администратора предоставить вам права участника для группы ресурсов.

## <a name="azure-functions-and-write-access"></a>Функции Azure и доступ для записи

Для некоторых компонентов решения [Функции Azure](../azure-functions/functions-overview.md) требуется доступ на запись. Например, если пользователю назначена роль читателя, он не сможет просматривать функции в пределах приложения-функции. На портале отобразится текст **(Нет доступа)** .

![Сообщение об отсутствии доступа в приложении-функции](./media/troubleshooting/functionapps-noaccess.png)

Пользователь с ролью читателя может щелкнуть вкладку **Функции платформы** и выбрать **Все параметры**, чтобы просмотреть некоторые параметры, связанные с приложением-функцией (так же, как для веб-приложения), но не может изменить эти параметры.

## <a name="next-steps"></a>Дальнейшие действия
* [Управление доступом к ресурсам Azure с помощью RBAC и портала Azure](role-assignments-portal.md)
* [Просмотр журналов действий для изменений RBAC в ресурсах Azure](change-history-report.md)

