---
title: Аутентификация с помощью реестра контейнеров Azure из службы "Экземпляры контейнеров Azure"
description: Узнайте, как обеспечить доступ к образам в закрытом реестре контейнеров из службы "Экземпляры контейнеров Azure" с помощью субъекта-службы Azure Active Directory.
services: container-registry
author: dlepow
manager: gwallace
ms.service: container-registry
ms.topic: article
ms.date: 04/23/2018
ms.author: danlep
ms.openlocfilehash: d2099de9ad909b23b79a92a831d7730b1cf126e3
ms.sourcegitcommit: f5075cffb60128360a9e2e0a538a29652b409af9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/18/2019
ms.locfileid: "68311639"
---
# <a name="authenticate-with-azure-container-registry-from-azure-container-instances"></a>Аутентификация с помощью реестра контейнеров Azure из службы "Экземпляры контейнеров Azure"

Вы можете обеспечить доступ к закрытым реестрам контейнеров в реестре контейнеров Azure с помощью субъекта-службы Azure Active Directory (Azure AD).

Из этой статьи вы узнаете, как создать и настроить субъект-службу Azure AD с разрешениями на *извлечение данных* в реестре. Затем вы запустите контейнер в службе "Экземпляры контейнеров Azure" (ACI), которая извлекает образ контейнера из закрытого реестра, с помощью субъекта-службы для аутентификации.

## <a name="when-to-use-a-service-principal"></a>Когда следует использовать субъект-службу

Субъект-службу следует использовать для аутентификации из ACI в **сценариях автоматического входа**, например в приложениях или службах, которые создают экземпляры контейнеров автоматически или другим способом без участия пользователя.

Например, для автоматического скрипта, который выполняется ночью и создает [экземпляр контейнера на основе задач](../container-instances/container-instances-restart-policy.md) для обработки некоторых данных, можно использовать субъект-службу с разрешениями только на извлечение для аутентификации в реестре. Затем можно сменить учетные данные субъекта-службы или полностью отменить его доступ, что никак не отразится на других службах и приложениях.

Субъекты-службы также следует использовать, если учетная запись [администратора](container-registry-authentication.md#admin-account) реестра отключена.

[!INCLUDE [container-registry-service-principal](../../includes/container-registry-service-principal.md)]

## <a name="authenticate-using-the-service-principal"></a>Аутентификация с помощью субъекта-службы

Чтобы запустить контейнер в службе "Экземпляры контейнеров Azure", используя субъект-службу, укажите идентификатор субъекта-службы для параметра `--registry-username` и пароль для параметра `--registry-password`.

```azurecli-interactive
az container create \
    --resource-group myResourceGroup \
    --name mycontainer \
    --image mycontainerregistry.azurecr.io/myimage:v1 \
    --registry-login-server mycontainerregistry.azurecr.io \
    --registry-username <service-principal-ID> \
    --registry-password <service-principal-password>
```

## <a name="sample-scripts"></a>Примеры сценариев

На GitHub можно найти предыдущие примеры сценариев для Azure CLI, а также версии для Azure PowerShell:

* [Интерфейс командной строки Azure][acr-scripts-cli]
* [Azure PowerShell][acr-scripts-psh]

## <a name="next-steps"></a>Следующие шаги

В следующих статьях приводятся дополнительные сведения о работе с субъектами-службами и реестром контейнеров Azure.

* [Аутентификация в реестре контейнеров Azure с помощью субъектов-служб](container-registry-auth-service-principal.md)
* [Аутентификация с помощью Реестра контейнеров Azure из службы Azure Kubernetes](container-registry-auth-aks.md)

<!-- IMAGES -->

<!-- LINKS - External -->
[acr-scripts-cli]: https://github.com/Azure/azure-docs-cli-python-samples/tree/master/container-registry
[acr-scripts-psh]: https://github.com/Azure/azure-docs-powershell-samples/tree/master/container-registry

<!-- LINKS - Internal -->
