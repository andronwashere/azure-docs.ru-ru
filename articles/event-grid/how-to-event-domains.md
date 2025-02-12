---
title: Управление большими наборами разделов в Сетке событий Azure с использованием доменов событий
description: Сведения об управлении большими наборами разделов в Сетке событий Azure и публикация событий в этих разделах с помощью доменов событий.
services: event-grid
author: banisadr
ms.service: event-grid
ms.author: babanisa
ms.topic: conceptual
ms.date: 01/17/2019
ms.openlocfilehash: 0042b0bd8c6ed9e9d253c44151dcf0588c742b48
ms.sourcegitcommit: e5dcf12763af358f24e73b9f89ff4088ac63c6cb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/14/2019
ms.locfileid: "67137842"
---
# <a name="manage-topics-and-publish-events-using-event-domains"></a>Управление разделами и публикация событий с помощью доменов событий

В этой статье показано, как:

* создать домен в Сетке событий;
* подписаться на разделы службы "Сетка событий";
* получение списка ключей;
* опубликовать события в домен.

Общие сведения о доменах событий см. в статье [Общие сведения о доменах событий, используемых для управления разделами службы "Сетка событий Azure"](event-domains.md).

[!INCLUDE [requires-azurerm](../../includes/requires-azurerm.md)]

## <a name="create-an-event-domain"></a>Создание Домена событий

Чтобы управлять большими наборами разделов, создайте домен событий.

Для интерфейса командной строки Azure:

```azurecli-interactive
az eventgrid domain create \
  -g <my-resource-group> \
  --name <my-domain-name> \
  -l <location>
```

Для PowerShell используйте команду:

```azurepowershell-interactive
New-AzureRmEventGridDomain `
  -ResourceGroupName <my-resource-group> `
  -Name <my-domain-name> `
  -Location <location>
```

После успешного создания возвращаются следующие значения:

```json
{
  "endpoint": "https://<my-domain-name>.westus2-1.eventgrid.azure.net/api/events",
  "id": "/subscriptions/<sub-id>/resourceGroups/<my-resource-group>/providers/Microsoft.EventGrid/domains/<my-domain-name>",
  "inputSchema": "EventGridSchema",
  "inputSchemaMapping": null,
  "location": "westus2",
  "name": "<my-domain-name>",
  "provisioningState": "Succeeded",
  "resourceGroup": "<my-resource-group>",
  "tags": null,
  "type": "Microsoft.EventGrid/domains"
}
```

Запишите значения `endpoint` и `id`, так как они необходимы для управления доменом и публикации событий.

## <a name="manage-access-to-topics"></a>Управление доступом к разделам

Управление доступом к разделам выполняется через [назначение ролей](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-cli). Назначение ролей использует управление доступом на основе ролей, чтобы ограничить операции с ресурсами Azure для авторизованных пользователей в определенной области.

Сетка событий имеет две встроенные роли, которые можно использовать для назначения определенным пользователям доступа к различным разделам в пределах домена. Это роль `EventGrid EventSubscription Contributor (Preview)`, которая позволяет создавать и удалять подписки, и роль `EventGrid EventSubscription Reader (Preview)`, которая позволяет только перечислять подписки на события.

Следующая команда интерфейса командной строки Azure ограничивает `alice@contoso.com` созданием и удалением подписок на события только в разделе `demotopic1`:

```azurecli-interactive
az role assignment create \
  --assignee alice@contoso.com \
  --role "EventGrid EventSubscription Contributor (Preview)" \
  --scope /subscriptions/<sub-id>/resourceGroups/<my-resource-group>/providers/Microsoft.EventGrid/domains/<my-domain-name>/topics/demotopic1
```

Следующая команда PowerShell ограничивает `alice@contoso.com` созданием и удалением подписок на события только в разделе `demotopic1`:

```azurepowershell-interactive
New-AzureRmRoleAssignment `
  -SignInName alice@contoso.com `
  -RoleDefinitionName "EventGrid EventSubscription Contributor (Preview)" `
  -Scope /subscriptions/<sub-id>/resourceGroups/<my-resource-group>/providers/Microsoft.EventGrid/domains/<my-domain-name>/topics/demotopic1
```

Дополнительные сведения об управлении доступом для операций службы "Сетка событий" см. в статье [Сетка событий: безопасность и проверка подлинности](./security-authentication.md).

## <a name="create-topics-and-subscriptions"></a>Создание разделов и подписок

Служба "Сетка событий" автоматически создает соответствующий раздел в домене и управляет им на основе вызова для создания подписки на событие для раздела домена. Отдельного шага по созданию раздела в домене нет. Аналогичным образом, когда последняя подписка на событие для раздела удаляется, сам раздел также удаляется.

Подписка на раздел в домене совпадает с подпиской на любой другой ресурс Azure. Для идентификатора исходного ресурса укажите идентификатор домена событий, возвращенный при создании домена ранее. Чтобы указать раздел, на который вы хотите подписаться, добавьте `/topics/<my-topic>` в конец идентификатора исходного ресурса. Чтобы создать подписку на события в области домена, которая принимает все события в домене, укажите идентификатор домена событий без указания каких-либо разделов.

Как правило, пользователь, которому вы предоставили доступ в предыдущем разделе, создаст подписку. Для упрощения этой статьи создайте подписку. 

Для интерфейса командной строки Azure:

```azurecli-interactive
az eventgrid event-subscription create \
  --name <event-subscription> \
  --source-resource-id "/subscriptions/<sub-id>/resourceGroups/<my-resource-group>/providers/Microsoft.EventGrid/domains/<my-domain-name>/topics/demotopic1" \
  --endpoint https://contoso.azurewebsites.net/api/updates
```

Для PowerShell используйте команду:

```azurepowershell-interactive
New-AzureRmEventGridSubscription `
  -ResourceId "/subscriptions/<sub-id>/resourceGroups/<my-resource-group>/providers/Microsoft.EventGrid/domains/<my-domain-name>/topics/demotopic1" `
  -EventSubscriptionName <event-subscription> `
  -Endpoint https://contoso.azurewebsites.net/api/updates
```

Если вам нужна тестовая конечная точка, куда будут отправляться события, вы всегда можете развернуть [предварительно готовое веб-приложение](https://github.com/Azure-Samples/azure-event-grid-viewer), отображающее входящие события. Вы можете отправлять события на веб-сайт тестирования по адресу `https://<your-site-name>.azurewebsites.net/api/updates`.

<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure-Samples%2Fazure-event-grid-viewer%2Fmaster%2Fazuredeploy.json" target="_blank"><img src="https://azuredeploy.net/deploybutton.png"/></a>

Разрешения, заданные для раздела, хранятся в Azure Active Directory и должны быть явно удалены. Удаление подписки на событие не отменяет доступ пользователей к созданию подписок на события, если у них есть доступ на запись в разделе.


## <a name="publish-events-to-an-event-grid-domain"></a>Публикация событий в Домен службы "Сетка событий"

Публикация событий в домен совпадает с публикацией [в настраиваемый раздел](./post-to-custom-topic.md). Тем не менее вместо публикации в пользовательском разделе публикуйте все события на конечную точку домена. В данных события JSON укажите раздел, к которому вы хотите добавить события. Следующий массив событий приведет к публикации события с идентификатором `"id": "1111"` в раздел `demotopic1`, а событие с идентификатором `"id": "2222"` будет отправлено в раздел `demotopic2`:

```json
[{
  "topic": "demotopic1",
  "id": "1111",
  "eventType": "maintenanceRequested",
  "subject": "myapp/vehicles/diggers",
  "eventTime": "2018-10-30T21:03:07+00:00",
  "data": {
    "make": "Contoso",
    "model": "Small Digger"
  },
  "dataVersion": "1.0"
},
{
  "topic": "demotopic2",
  "id": "2222",
  "eventType": "maintenanceCompleted",
  "subject": "myapp/vehicles/tractors",
  "eventTime": "2018-10-30T21:04:12+00:00",
  "data": {
    "make": "Contoso",
    "model": "Big Tractor"
  },
  "dataVersion": "1.0"
}]
```

Чтобы получить конечную точку домена с помощью Azure CLI, выполните следующую команду:

```azurecli-interactive
az eventgrid domain show \
  -g <my-resource-group> \
  -n <my-domain>
```

Чтобы получить ключи, которые будет использовать домен, выполните команду ниже:

```azurecli-interactive
az eventgrid domain key list \
  -g <my-resource-group> \
  -n <my-domain>
```

Чтобы получить конечную точку домена с помощью PowerShell, выполните следующую команду:

```azurepowershell-interactive
Get-AzureRmEventGridDomain `
  -ResourceGroupName <my-resource-group> `
  -Name <my-domain>
```

Чтобы получить ключи, которые будет использовать домен, выполните команду ниже:

```azurepowershell-interactive
Get-AzureRmEventGridDomainKey `
  -ResourceGroupName <my-resource-group> `
  -Name <my-domain>
```

Затем используйте предпочтительный метод создания запроса HTTP POST для публикации событий в домен Сетки событий.

## <a name="search-lists-of-topics-or-subscriptions"></a>Поиск в списках разделов или подписок

Чтобы сделать поиск и управление большим количеством разделов или подписок, API-интерфейсы сетки событий поддерживать список и разбиение на страницы.

### <a name="using-cli"></a>Использование синтаксиса командной строки

Для использования его убедитесь в том, что вы используете версию расширения Azure CLI "Сетка событий" 0.4.1 или более поздней версии.

```azurecli-interactive
# If you haven't already installed the extension, do it now.
# This extension is required for preview features.
az extension add --name eventgrid

az eventgrid topic list \
    --odata-query "contains(name, 'my-test-filter')"
```

## <a name="next-steps"></a>Дальнейшие действия

* Общие понятия, касающиеся доменов событий, и сведения об их эффективности см. в [этой статье](event-domains.md).
