---
title: Проверка работоспособности реестра в реестре контейнеров Azure
description: Узнайте, как выполнить команду быстрой диагностики для выявления распространенных проблем при использовании реестра контейнеров Azure, включая настройку локального DOCKER и подключение к реестру.
services: container-registry
author: dlepow
manager: gwallace
ms.service: container-registry
ms.topic: article
ms.date: 07/02/2019
ms.author: danlep
ms.openlocfilehash: 3511655d220ee85ce6b5744612e5d6fddafbe877
ms.sourcegitcommit: f5075cffb60128360a9e2e0a538a29652b409af9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/18/2019
ms.locfileid: "68309731"
---
# <a name="check-the-health-of-an-azure-container-registry"></a>Проверка работоспособности реестра контейнеров Azure

При использовании реестра контейнеров Azure иногда могут возникать проблемы. Например, вы не можете извлечь образ контейнера из-за проблемы с DOCKER в локальной среде. Или сетевая ошибка может препятствовать подключению к реестру. 

В качестве первого шага диагностики выполните команду [AZ контролем учетных записей проверки работоспособности][az-acr-check-health] command to get information about the health of the environment and optionally access to a target registry. This command is available in Azure CLI version 2.0.67 or later. If you need to install or upgrade, see [Install Azure CLI][azure-cli].

## <a name="run-az-acr-check-health"></a>Выполните команду AZ контроля доступа (проверка работоспособности)

В следующих примерах показаны различные способы выполнения `az acr check-health` команды.

> [!NOTE]
> При выполнении команды в Azure Cloud Shell локальная среда не проверяется. Однако можно проверить доступ к целевому реестру.

### <a name="check-the-environment-only"></a>Проверка только окружения

Чтобы проверить локальную управляющую программу DOCKER, версию CLI и конфигурацию клиента Helm, выполните команду без дополнительных параметров:

```azurecli
az acr check-health
```

### <a name="check-the-environment-and-a-target-registry"></a>Проверка среды и целевого реестра

Чтобы проверить доступ к реестру, а также выполнить проверку локальной среды, передайте имя целевого реестра. Пример:

```azurecli
az acr check-health --name myregistry
```

## <a name="error-reporting"></a>Отчеты об ошибках

Команда записывает данные в стандартный вывод. При обнаружении проблемы она предоставляет код и описание ошибки. Дополнительные сведения о кодах и возможных решениях см. в справочнике по [ошибкам](container-registry-health-error-reference.md).

По умолчанию команда останавливается при обнаружении ошибки. Можно также выполнить команду, чтобы она выпускала выходные данные для всех проверок работоспособности, даже если обнаружены ошибки. `--ignore-errors` Добавьте параметр, как показано в следующих примерах:

```azurecli
# Check environment only
az acr check-health --ignore-errors

# Check environment and target registry
az acr check-health --name myregistry --ignore-errors
```      

Пример выходных данных:

```console
$ az acr check-health --name myregistry --ignore-errors --yes

Docker daemon status: available
Docker version: Docker version 18.09.2, build 6247962
Docker pull of 'mcr.microsoft.com/mcr/hello-world:latest' : OK
ACR CLI version: 2.2.9
Helm version:
Client: &version.Version{SemVer:"v2.14.1", GitCommit:"5270352a09c7e8b6e8c9593002a73535276507c0", GitTreeState:"clean"}
DNS lookup to myregistry.azurecr.io at IP 40.xxx.xxx.162 : OK
Challenge endpoint https://myregistry.azurecr.io/v2/ : OK
Fetch refresh token for registry 'myregistry.azurecr.io' : OK
Fetch access token for registry 'myregistry.azurecr.io' : OK
```  



## <a name="next-steps"></a>Следующие шаги

Дополнительные сведения о кодах ошибок, возвращаемых командой [AZ контроля доступа проверки][az-acr-check-health] работоспособности, см [. в](container-registry-health-error-reference.md)этой статье.

Часто задаваемые вопросы и другие известные проблемы реестра контейнеров Azure см. в [часто](container-registry-faq.md) задаваемых вопросах.





<!-- LINKS - internal -->
[azure-cli]: /cli/azure/install-azure-cli
[az-acr-check-health]: /cli/azure/acr#az-acr-check-health
