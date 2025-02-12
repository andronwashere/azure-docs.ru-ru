---
title: Субъект-службы для служб Azure Kubernetes (AKS)
description: Создание субъект-службы Azure Active Directory для кластера в службе Azure Kubernetes (AKS) и управление ей
services: container-service
author: mlearned
ms.service: container-service
ms.topic: conceptual
ms.date: 04/25/2019
ms.author: mlearned
ms.openlocfilehash: 304b9dae9f3a1e134809d8959a96dc4e3ec0edd3
ms.sourcegitcommit: 6a42dd4b746f3e6de69f7ad0107cc7ad654e39ae
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/07/2019
ms.locfileid: "67615109"
---
# <a name="service-principals-with-azure-kubernetes-service-aks"></a>Субъекты-службы со службой Azure Kubernetes

Для взаимодействия с API-интерфейсами Azure кластеру AKS требуется [субъекта-службы Azure Active Directory (AD)][aad-service-principal]. Субъект-служба необходима для динамического создания и управления другими ресурсами Azure, например балансировщиком нагрузки Azure или реестром контейнеров (ACR).

В этой статье показано как создать и использовать субъект-службу для кластеров AKS.

## <a name="before-you-begin"></a>Перед началом работы

Чтобы создать субъект-службу в Azure AD, вы должны иметь права на регистрацию приложения в клиенте Azure AD и назначение приложению роли в подписке Azure. Если у вас нет необходимых разрешений, может потребоваться попросить администратора Azure AD или администратора подписки предоставить их или предварительно создать субъект-службу для использования с кластером AKS.

Если вы используете субъект-службу из Azure AD, отличным клиентом, доступны дополнительные рекомендации, связанные с разрешениями при развертывании кластера. Возможно, у вас нет соответствующих разрешений для чтения и записи данных каталога. Дополнительные сведения см. в разделе [Каковы разрешения пользователя по умолчанию в Azure Active Directory?][azure-ad-permissions]

Вам также понадобится Azure CLI версии 2.0.59 или более поздней версии установлен и настроен. Чтобы узнать версию, выполните команду  `az --version`. Если требуется выполнить установку или обновление, см. в разделе [установить Azure CLI][install-azure-cli].

## <a name="automatically-create-and-use-a-service-principal"></a>Автоматическое создание и использование субъект-службы

При создании кластера AKS в Azure, портала или с использованием [создать az aks][az-aks-create] команды Azure может автоматически создать субъект-службу.

В следующем примере Azure CLI субъект-служба не указана. В этом сценарии субъект-службу в кластере AKS создает Azure CLI. Для успешного завершения операции в учетной записи Azure должны быть соответствующие права на создание субъект-службы.

```azurecli
az aks create --name myAKSCluster --resource-group myResourceGroup
```

## <a name="manually-create-a-service-principal"></a>Создание субъект-службы вручную

Чтобы вручную создать субъект-службу с помощью Azure CLI, используйте [az ad sp create-for-rbac][az-ad-sp-create] команды. В следующем примере параметр `--skip-assignment` запрещает присваивание любых дополнительных назначений по умолчанию.

```azurecli-interactive
az ad sp create-for-rbac --skip-assignment
```

Результат будет похож на следующий пример. Запишите свои `appId` и `password`. Эти значения используются при создании кластера AKS в следующем разделе.

```json
{
  "appId": "559513bd-0c19-4c1a-87cd-851a26afd5fc",
  "displayName": "azure-cli-2019-03-04-21-35-28",
  "name": "http://azure-cli-2019-03-04-21-35-28",
  "password": "e763725a-5eee-40e8-a466-dc88d980f415",
  "tenant": "72f988bf-86f1-41af-91ab-2d7cd011db48"
}
```

## <a name="specify-a-service-principal-for-an-aks-cluster"></a>Указание субъект-службы для кластера AKS

Чтобы использовать существующий субъект-службу при создании кластера AKS с помощью [создать az aks][az-aks-create] команды, используйте `--service-principal` и `--client-secret` параметры, указывающие `appId` и `password` из выходных данных [az ad sp create-for-rbac][az-ad-sp-create] команды:

```azurecli-interactive
az aks create \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --service-principal <appId> \
    --client-secret <password>
```

При развертывании кластера AKS с помощью портала Azure на странице *Проверка подлинности* диалогового окна **Create Kubernetes cluster** (Создание кластера Kubernetes) выберите **Настроить субъект-службу**. Выберите **Использовать существующую**и укажите следующие значения:

- **Service principal client ID** (Идентификатор клиента для субъект-службы) — это *appId*;
- **Service principal client secret** (Секрет клиента субъект-службы) — это значение *password*.

![Изображение перехода к приложению Azure для голосования](media/kubernetes-service-principal/portal-configure-service-principal.png)

## <a name="delegate-access-to-other-azure-resources"></a>Делегирование прав доступа другим ресурсам Azure

Субъект-службу для кластера AKS можно использовать для доступа к другим ресурсам. Например если вы хотите развернуть этот кластер AKS в имеющейся подсетью виртуальной сети Azure или подключение для реестра контейнеров Azure (ACR), необходимо делегировать доступ к этим ресурсам субъекту-службе.

Чтобы делегировать разрешения, создать назначение роли с помощью [Создание назначений ролей az][az-role-assignment-create] команды. Назначить `appId` к определенной области, например группу ресурсов или ресурс виртуальной сети. Затем роль определяет разрешения субъекта-службы по отношению к ресурсу, как показано в следующем примере:

```azurecli
az role assignment create --assignee <appId> --scope <resourceScope> --role Contributor
```

Для `--scope` ресурса нужно указать полный идентификатор ресурса, например */subscriptions/\<GUID\>/resourceGroups/myResourceGroup* или */subscriptions/\<GUID\>/resourceGroups/myResourceGroupVnet/providers/Microsoft.Network/virtualNetworks/myVnet*

В следующих разделах описываются распространенные делегирования, которые вам могут потребоваться.

### <a name="azure-container-registry"></a>Реестр контейнеров Azure

Если вы используете Реестр контейнеров Azure (ACR) как хранилище образов контейнера, необходимо предоставить кластеру AKS разрешения на чтение и извлечение образов. Субъекту-службе кластера AKS необходимо делегировать роль *Читатель* в реестре. Подробные инструкции см. в разделе [предоставление AKS доступа к ACR][aks-to-acr].

### <a name="networking"></a>Сеть

Вы можете использовать расширенное сетевое взаимодействие, где виртуальная сеть и подсеть или общедоступные IP-адреса находятся в другой группе ресурсов. Назначьте одно разрешение из следующего набора разрешений роли:

- Создание [пользовательской роли][rbac-custom-role] и определите следующие разрешения роли:
  - *Microsoft.Network/virtualNetworks/subnets/join/action*
  - *Microsoft.Network/virtualNetworks/subnets/read*
  - *Microsoft.Network/virtualNetworks/subnets/write*
  - *Microsoft.Network/publicIPAddresses/join/action*
  - *Microsoft.Network/publicIPAddresses/read*
  - *Microsoft.Network/publicIPAddresses/write*
- Также можно предоставить [участник сетей][rbac-network-contributor] встроенной роли в подсети в виртуальной сети

### <a name="storage"></a>Хранилище

Вам может потребоваться доступ к существующим дисковым ресурсам в другой группе ресурсов. Назначьте одно разрешение из следующего набора разрешений роли:

- Создание [пользовательской роли][rbac-custom-role] и определите следующие разрешения роли:
  - *Microsoft.Compute/disks/read*
  - *Microsoft.Compute/disks/write*
- Также можно предоставить [участник учетной записи хранения][rbac-storage-contributor] встроенные роли для группы ресурсов

### <a name="azure-container-instances"></a>Экземпляры контейнеров Azure

Если вы используете интеграцию Virtual Kubelet и AKS и решили запустить службу "Экземпляры контейнеров Azure" (ACI) в группе ресурсов, отличной от группы для кластера AKS, предоставьте субъекту-службе AKS разрешения *Участник* на доступ к группе ресурсов ACI.

## <a name="additional-considerations"></a>Дополнительные замечания

При использовании AKS и субъект-служб Azure AD учитывайте приведенные ниже аспекты.

- Субъект-служба для Kubernetes входит в конфигурацию кластера. Тем не менее не используйте идентификатор для развертывания кластера.
- По умолчанию учетные данные субъекта-службы действительны в течение одного года. Вы можете [обновления или смены учетных данных субъекта-службы][update-credentials] в любое время.
- Все субъекты-службы связаны с определенными приложениями Azure AD. Субъект-служба для кластера Kubernetes может быть связана с любым допустимым именем приложения Azure AD (например *https://www.contoso.org/example* ). URL-адрес приложения не обязательно должен быть реальной конечной точкой.
- При указании **идентификатора клиента** субъект-службы используйте значение `appId`.
- На узле агент виртуальных машин в кластере Kubernetes учетные данные субъекта-службы хранятся в файле `/etc/kubernetes/azure.json`
- При использовании [создать az aks][az-aks-create] команду, чтобы создать субъект-службу автоматически, службы, учетные данные записываются в файл `~/.azure/aksServicePrincipal.json` на компьютере, используемый для выполнения команды.
- При удалении кластера AKS, разработанный Доном Саймом [создать az aks][az-aks-create], субъекта-службы, который был создан автоматически не удаляется.
    - Чтобы удалить субъект-службу, выполните запрос для кластера *servicePrincipalProfile.clientId* , а затем удалить с [az ad app delete][az-ad-app-delete]. Замените имена группы ресурсов и кластера собственными значениями.

        ```azurecli
        az ad sp delete --id $(az aks show -g myResourceGroup -n myAKSCluster --query servicePrincipalProfile.clientId -o tsv)
        ```

## <a name="troubleshoot"></a>Устранение неполадок

Учетные данные субъекта-службы для кластера AKS, кэшируются Azure CLI. Если истек срок действия этих учетных данных, возникают ошибки при развертывании кластеров в AKS. Сообщение об ошибке при выполнении [создать az aks][az-aks-create] может указывать на проблему с учетными данными субъекта-службы кэширования:

```console
Operation failed with status: 'Bad Request'.
Details: The credentials in ServicePrincipalProfile were invalid. Please see https://aka.ms/aks-sp-help for more details.
(Details: adal: Refresh request failed. Status Code = '401'.
```

Проверьте срок действия файла учетных данных, используя следующую команду:

```console
ls -la $HOME/.azure/aksServicePrincipal.json
```

Срок действия учетных данных субъекта-службы по умолчанию — один год. Если ваш *aksServicePrincipal.json* файл старше одного года, удалите его и попробуйте снова развернуть кластер службы контейнеров AZURE.

## <a name="next-steps"></a>Следующие шаги

Дополнительные сведения о субъекте-службе Azure Active Directory см. в разделе [объекты приложения и службы субъекта][service-principal].

Сведения о том, как обновить учетные данные, см. в разделе [обновления или сменить учетные данные для субъекта-службы в AKS][update-credentials].

<!-- LINKS - internal -->
[aad-service-principal]:../active-directory/develop/app-objects-and-service-principals.md
[acr-intro]: ../container-registry/container-registry-intro.md
[az-ad-sp-create]: /cli/azure/ad/sp#az-ad-sp-create-for-rbac
[azure-load-balancer-overview]: ../load-balancer/load-balancer-overview.md
[install-azure-cli]: /cli/azure/install-azure-cli
[service-principal]:../active-directory/develop/app-objects-and-service-principals.md
[user-defined-routes]: ../load-balancer/load-balancer-overview.md
[az-ad-app-list]: /cli/azure/ad/app#az-ad-app-list
[az-ad-app-delete]: /cli/azure/ad/app#az-ad-app-delete
[az-aks-create]: /cli/azure/aks#az-aks-create
[rbac-network-contributor]: ../role-based-access-control/built-in-roles.md#network-contributor
[rbac-custom-role]: ../role-based-access-control/custom-roles.md
[rbac-storage-contributor]: ../role-based-access-control/built-in-roles.md#storage-account-contributor
[az-role-assignment-create]: /cli/azure/role/assignment#az-role-assignment-create
[aks-to-acr]: ../container-registry/container-registry-auth-aks.md?toc=%2fazure%2faks%2ftoc.json#grant-aks-access-to-acr
[update-credentials]: update-credentials.md
[azure-ad-permissions]: ../active-directory/fundamentals/users-default-permissions.md
