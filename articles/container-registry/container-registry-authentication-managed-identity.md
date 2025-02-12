---
title: Аутентификация в Реестре контейнеров Azure с использованием управляемого удостоверения
description: Предоставление доступа к образам в закрытом реестре контейнеров с помощью управляемого удостоверения Azure, назначаемого пользователем или системой.
services: container-registry
author: dlepow
manager: gwallace
ms.service: container-registry
ms.topic: article
ms.date: 01/16/2019
ms.author: danlep
ms.openlocfilehash: 0672fb71ba4f56d0faf332df029100cb48741c8b
ms.sourcegitcommit: f5075cffb60128360a9e2e0a538a29652b409af9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/18/2019
ms.locfileid: "68309889"
---
# <a name="use-an-azure-managed-identity-to-authenticate-to-an-azure-container-registry"></a>Аутентификация в реестре контейнеров Azure с помощью управляемого удостоверения Azure 

Проходите аутентификацию в реестре контейнеров Azure из другого ресурса Azure с помощью [управляемого удостоверения для ресурсов Azure](../active-directory/managed-identities-azure-resources/overview.md), не предоставляя учетные данные реестра и не управляя ими. Например, настройте управляемое удостоверение, назначаемое пользователем или системой, на виртуальной машине Linux, чтобы получать доступ к образам контейнера из реестра контейнеров так же просто, как при использовании общедоступного реестра.

В этой статье вы ознакомитесь с управляемыми удостоверениями и узнаете, как выполнять следующие действия:

> [!div class="checklist"]
> * применение на виртуальной машине Azure удостоверения, назначаемого пользователем или системой;
> * предоставление удостоверению доступа к реестру контейнеров Azure;
> * получение доступа к реестру и извлечение образа контейнера с помощью управляемого удостоверения. 

Для создания ресурсов Azure с помощью инструкций из этой статьи требуется Azure CLI версии 2.0.55 или более поздней версии. Чтобы узнать версию, выполните команду `az --version`. Если вам необходимо выполнить установку или обновление, см. статью [Установка Azure CLI 2.0][azure-cli].

Чтобы настроить реестр контейнеров и отправить образ контейнера в него, необходимо также локально установить модуль Docker. DOCKER предоставляет пакеты, которые легко настраивают DOCKER в любой [macOS][docker-mac], [Windows][docker-windows]или системе [Linux][DOCKER-Linux] .

## <a name="why-use-a-managed-identity"></a>Для чего нужны управляемые удостоверения?

Функция управляемого удостоверения для ресурсов Azure предоставляет службам Azure автоматически управляемое удостоверение в Azure Active Directory (Azure AD). Управляемое удостоверение можно настроить для [определенных ресурсов Azure](../active-directory/managed-identities-azure-resources/services-support-managed-identities.md), включая виртуальные машины. Затем с использованием удостоверения вы можете получить доступ к другим ресурсам Azure, не предоставляя учетные данные в коде или сценариях.

Управляемые удостоверения бывают двух типов:

* *Назначаемые пользователем удостоверения*, которые можно назначить нескольким ресурсам и хранить столько, сколько необходимо. Назначаемые пользователем удостоверения сейчас доступны в предварительной версии.

* *Удостоверение, управляемое системой*, является уникальным для определенного ресурса, например отдельной виртуальной машины, и актуально в течение времени существования этого ресурса.

После настройки для ресурса Azure управляемого удостоверения предоставьте последнему необходимый доступ к другому ресурсу, как и любому субъекту безопасности. Например, назначьте управляемому удостоверению роль с разрешениями на извлечение, отправку и извлечение или другими разрешениями в частном реестре в Azure. (Полный список ролей реестра см. в статье [Роли и разрешения реестра контейнеров Azure](container-registry-roles.md).) Удостоверению можно предоставить доступ к одному или нескольким ресурсам.

Затем это удостоверение можно использовать для аутентификации в любой [службе, которая поддерживает аутентификацию Azure AD](../active-directory/managed-identities-azure-resources/services-support-managed-identities.md#azure-services-that-support-azure-ad-authentication), не храня какие-либо учетные данные в коде. Чтобы получить доступ к реестру контейнеров Azure с виртуальной машины, используя удостоверение, выполните аутентификацию с помощью Azure Resource Manager. Выберите метод проверки подлинности с помощью управляемого удостоверения в зависимости от сценария:

* [получение маркера доступа Azure AD](../active-directory/managed-identities-azure-resources/how-to-use-vm-token.md) программным способом с помощью вызовов HTTP и REST;

* использование [пакетов SDK Azure](../active-directory/managed-identities-azure-resources/how-to-use-vm-sdk.md);

* [вход в Azure CLI или PowerShell](../active-directory/managed-identities-azure-resources/how-to-use-vm-sign-in.md) с удостоверением. 

## <a name="create-a-container-registry"></a>Создание реестра контейнеров

Если у вас еще нет реестра контейнеров Azure, создайте его и отправьте в него пример образа контейнера. Действия описаны в [кратком руководстве по созданию закрытого реестра контейнеров с помощью Azure CLI](container-registry-get-started-azure-cli.md).

В этой статье предполагается, что у вас есть образ контейнера `aci-helloworld:v1`, хранящийся в реестре. В примерах используется реестр с именем *myContainerRegistry*. На последующих шагах замените эти значения именами вашего реестра и образа.

## <a name="create-a-docker-enabled-vm"></a>Создание виртуальной машины с поддержкой Docker

Создайте виртуальную машину Ubuntu с поддержкой Docker. Необходимо также установить [Azure CLI](/cli/azure/install-azure-cli?view=azure-cli-latest) на виртуальной машине. Если у вас уже есть виртуальная машина Azure, пропустите этот шаг создания виртуальной машины.

Разверните виртуальную машину Ubuntu Azure по умолчанию с помощью команды [AZ VM Create][az-vm-create]. В следующем примере создается виртуальная машина с именем *myDockerVM* в имеющейся группе ресурсов *myResourceGroup*.

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myDockerVM \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys
```

Создание виртуальной машины может занять несколько минут. После выполнения команды запишите значение `publicIpAddress`, отображаемое Azure CLI. Этот адрес используется для установления SSH-подключений к виртуальной машине.

### <a name="install-docker-on-the-vm"></a>Установка Docker на виртуальной машине

После запуска виртуальной машины установите SSH-подключение к ней. Замените *publicIpAddress* общедоступным IP-адресом виртуальной машины.

```bash
ssh azureuser@publicIpAddress
```

Выполните следующую команду, чтобы установить Docker на виртуальную машину:

```bash
sudo apt install docker.io -y
```

После установки выполните следующую команду, чтобы проверить правильность работы Docker на виртуальной машине:

```bash
sudo docker run -it hello-world
```

Выходные данные:

```
Hello from Docker!
This message shows that your installation appears to be working correctly.
[...]
```

### <a name="install-the-azure-cli"></a>Установка Azure CLI

Выполните действия, описанные в статье [Установка Azure CLI с помощью apt](/cli/azure/install-azure-cli-apt?view=azure-cli-latest), чтобы установить Azure CLI на виртуальной машине Ubuntu. Для этой статьи установите версию 2.0.55 или более позднюю версию.

Выйдите из сеанса SSH.

## <a name="example-1-access-with-a-user-assigned-identity"></a>Пример 1 Доступ с помощью назначаемого пользователем удостоверения

### <a name="create-an-identity"></a>Создание удостоверения

Создайте в подписке удостоверение, используя команду [az identity create](/cli/azure/identity?view=azure-cli-latest#az-identity-create). Вы можете использовать ту же группу ресурсов, что и ранее для создания реестра контейнеров или виртуальной машины, либо другую.

```azurecli-interactive
az identity create --resource-group myResourceGroup --name myACRId
```

Чтобы настроить удостоверение в следующих шагах, используйте команду [AZ Identity][az-identity-show] -use, чтобы сохранить идентификатор ресурса удостоверения и идентификатор субъекта-службы в переменных.

```azurecli
# Get resource ID of the user-assigned identity
userID=$(az identity show --resource-group myResourceGroup --name myACRId --query id --output tsv)

# Get service principal ID of the user-assigned identity
spID=$(az identity show --resource-group myResourceGroup --name myACRId --query principalId --output tsv)
```

Так как идентификатор удостоверения потребуется позже при входе в интерфейс командной строки с виртуальной машины, отобразится значение:

```bash
echo $userID
```

Идентификатор выглядит следующим образом:

```
/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxxx/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/myACRId
```

### <a name="configure-the-vm-with-the-identity"></a>Настройка виртуальной машины с удостоверением

Следующая команда [AZ VM Identity Assign][az-vm-identity-assign] НАстраивает виртуальную машину DOCKER с назначенным пользователем удостоверением:

```azurecli
az vm identity assign --resource-group myResourceGroup --name myDockerVM --identities $userID
```

### <a name="grant-identity-access-to-the-container-registry"></a>Предоставление удостоверению доступа к реестру контейнеров

Теперь настройте для удостоверения доступ к реестру контейнеров. Сначала используйте команду [AZ запись контроля][az-acr-show] доступа, чтобы получить идентификатор ресурса реестра:

```azurecli
resourceID=$(az acr show --resource-group myResourceGroup --name myContainerRegistry --query id --output tsv)
```

Чтобы назначить роль Акрпулл для реестра, используйте команду [AZ Role назначение Create][az-role-assignment-create] . Эта роль предоставляет реестру [разрешения на извлечение](container-registry-roles.md). Чтобы предоставить разрешения и на извлечение, и на отправку, назначьте роль ACRPush.

```azurecli
az role assignment create --assignee $spID --scope $resourceID --role acrpull
```

### <a name="use-the-identity-to-access-the-registry"></a>Использование удостоверения для доступа к реестру

Установите SSH-подключение к виртуальной машине Docker, настроенной с удостоверением. Выполните следующие команды в Azure CLI, установленном на виртуальной машине.

Сначала Проверяйте подлинность Azure CLI с помощью команды [AZ login][az-login], используя удостоверение, настроенное на виртуальной машине. Для `<userID>` замените идентификатор удостоверения идентификатором, полученным на предыдущем шаге. 

```azurecli
az login --identity --username <userID>
```

Затем выполните аутентификацию в реестре с помощью команды [AZ контроля учетных записей][az-acr-login]. При использовании этой команды CLI применяет маркер Active Directory, созданный при выполнении команды `az login`, чтобы эффективно выполнить аутентификацию вашего сеанса в реестре контейнеров. (В зависимости от настройки виртуальной машины может потребоваться выполнить эту команду и команды Docker с помощью `sudo`.)

```azurecli
az acr login --name myContainerRegistry
```

Отобразится сообщение `Login succeeded`. Затем вы можете выполнить команды `docker`, не предоставляя учетные данные. Например, запустите [DOCKER Pull][docker-pull] , чтобы извлечь `aci-helloworld:v1` образ, указав имя сервера входа в реестре. Имя сервера входа состоит из имени реестра контейнеров (в нижнем регистре), за которым следует `.azurecr.io`, например `mycontainerregistry.azurecr.io`.

```
docker pull mycontainerregistry.azurecr.io/aci-helloworld:v1
```

## <a name="example-2-access-with-a-system-assigned-identity"></a>Пример 2 Доступ с помощью назначаемого системой удостоверения

### <a name="configure-the-vm-with-a-system-managed-identity"></a>Настройка виртуальной машины с управляемым системой удостоверением

Следующая команда [AZ VM Identity Assign][az-vm-identity-assign] НАстраивает виртуальную машину DOCKER с назначенным системой удостоверением:

```azurecli
az vm identity assign --resource-group myResourceGroup --name myDockerVM 
```

Используйте команду [AZ VM][az-vm-show] -Command, чтобы задать для переменной значение `principalId` (идентификатор субъекта-службы) удостоверения виртуальной машины, которое будет использоваться в последующих шагах.

```azurecli-interactive
spID=$(az vm show --resource-group myResourceGroup --name myDockerVM --query identity.principalId --out tsv)
```

### <a name="grant-identity-access-to-the-container-registry"></a>Предоставление удостоверению доступа к реестру контейнеров

Теперь настройте для удостоверения доступ к реестру контейнеров. Сначала используйте команду [AZ запись контроля][az-acr-show] доступа, чтобы получить идентификатор ресурса реестра:

```azurecli
resourceID=$(az acr show --resource-group myResourceGroup --name myContainerRegistry --query id --output tsv)
```

Чтобы назначить роль Акрпулл для удостоверения, используйте команду [AZ Role назначение Create][az-role-assignment-create] . Эта роль предоставляет реестру [разрешения на извлечение](container-registry-roles.md). Чтобы предоставить разрешения и на извлечение, и на отправку, назначьте роль ACRPush.

```azurecli
az role assignment create --assignee $spID --scope $resourceID --role acrpull
```

### <a name="use-the-identity-to-access-the-registry"></a>Использование удостоверения для доступа к реестру

Установите SSH-подключение к виртуальной машине Docker, настроенной с удостоверением. Выполните следующие команды в Azure CLI, установленном на виртуальной машине.

Сначала Проверяйте подлинность Azure CLI с помощью команды [AZ login][az-login], используя назначенное системой удостоверение на виртуальной машине.

```azurecli
az login --identity
```

Затем выполните аутентификацию в реестре с помощью команды [AZ контроля учетных записей][az-acr-login]. При использовании этой команды CLI применяет маркер Active Directory, созданный при выполнении команды `az login`, чтобы эффективно выполнить аутентификацию вашего сеанса в реестре контейнеров. (В зависимости от настройки виртуальной машины может потребоваться выполнить эту команду и команды Docker с помощью `sudo`.)

```azurecli
az acr login --name myContainerRegistry
```

Отобразится сообщение `Login succeeded`. Затем вы можете выполнить команды `docker`, не предоставляя учетные данные. Например, запустите [DOCKER Pull][docker-pull] , чтобы извлечь `aci-helloworld:v1` образ, указав имя сервера входа в реестре. Имя сервера входа состоит из имени реестра контейнеров (в нижнем регистре), за которым следует `.azurecr.io`, например `mycontainerregistry.azurecr.io`.

```
docker pull mycontainerregistry.azurecr.io/aci-helloworld:v1
```

## <a name="next-steps"></a>Следующие шаги

Из этой статьи вы узнали, как использовать управляемые удостоверения в службе "Реестр контейнеров Azure" и выполнять следующие действия:

> [!div class="checklist"]
> * применение на виртуальной машине Azure удостоверения, назначаемого пользователем или системой;
> * предоставление удостоверению доступа к реестру контейнеров Azure;
> * получение доступа к реестру и извлечение образа контейнера с помощью управляемого удостоверения.

* Дополнительные сведения см. в статье [Использование управляемых удостоверений в Службе приложений и Функциях Azure](/azure/active-directory/managed-identities-azure-resources/).


<!-- LINKS - external -->
[docker-linux]: https://docs.docker.com/engine/installation/#supported-platforms
[docker-login]: https://docs.docker.com/engine/reference/commandline/login/
[docker-mac]: https://docs.docker.com/docker-for-mac/
[docker-pull]: https://docs.docker.com/engine/reference/commandline/pull/
[docker-windows]: https://docs.docker.com/docker-for-windows/

<!-- LINKS - Internal -->
[az-login]: /cli/azure/reference-index#az-login
[az-acr-login]: /cli/azure/acr#az-acr-login
[az-acr-show]: /cli/azure/acr#az-acr-show
[az-vm-create]: /cli/azure/vm#az-vm-create
[az-vm-show]: /cli/azure/vm#az-vm-show
[az-vm-identity-assign]: /cli/azure/vm/identity#az-vm-identity-assign
[az-role-assignment-create]: /cli/azure/role/assignment#az-role-assignment-create
[az-acr-login]: /cli/azure/acr#az-acr-login
[az-identity-show]: /cli/azure/identity#az-identity-show
[azure-cli]: /cli/azure/install-azure-cli
