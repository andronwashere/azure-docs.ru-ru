---
title: Запуск Virtual Kubelet в кластере службы Azure Kubernetes
description: Узнайте, как использовать Virtual Kubelet со службой Azure Kubernetes (AKS) для запуска контейнеров Linux и Windows в службе "Экземпляры контейнеров Azure".
services: container-service
author: mlearned
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 05/31/2019
ms.author: mlearned
ms.openlocfilehash: f18992be353d2d6cc739412d98ccd97d5e78d4c7
ms.sourcegitcommit: 6a42dd4b746f3e6de69f7ad0107cc7ad654e39ae
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/07/2019
ms.locfileid: "67613860"
---
# <a name="use-virtual-kubelet-with-azure-kubernetes-service-aks"></a>Использование Virtual Kubelet со службой Azure Kubernetes (AKS)

Экземпляры контейнеров Azure предоставляют размещенную среду для запуска контейнеров в Azure. При использовании экземпляров контейнеров Azure вам не требуется управлять базовой вычислительной инфраструктурой, это делает Azure. При запуске контейнеров в Экземплярах контейнеров Azure взимается плата за каждый второй запущенный контейнер.

При использовании поставщика Virtual Kubelet для Экземпляров контейнеров Azure вы можете назначить контейнеры Linux и Windows на экземпляр контейнера, как если бы это был стандартный узел Kubernetes. Эта конфигурация позволяет воспользоваться преимуществами Kubernetes и возможностью управления стоимостью и затратами экземпляров контейнеров.

> [!NOTE]
> Теперь в AKS есть встроенная поддержка планирования контейнеров в ACI (*виртуальные узлы*). Сейчас эти виртуальные узлы поддерживают экземпляры контейнеров Linux. Если вам нужно запланировать экземпляры контейнеров Windows, вы можете продолжить работу с Virtual Kubelet. В противном случае следует использовать виртуальные узлы и не выполнять вручную инструкции Virtual Kubelet, указанные в данной статье. Вы можете начать работу с виртуальными узлами с помощью [Azure CLI][virtual-nodes-cli] or [Azure portal][virtual-nodes-portal].
>
> Virtual Kubelet представляет собой экспериментальный открытый проект и должен использоваться таким образом. Для участия, проблемы с файлов и узнайте больше о виртуальных kubelet, см. в разделе [проекта GitHub виртуального Kubelet][vk-github].

## <a name="before-you-begin"></a>Перед началом работы

Для выполнения этих инструкций у вас должен быть кластер AKS. Если вам нужен кластер AKS, см. в разделе [быстрого запуска Azure Kubernetes Service (AKS)][aks-quick-start].

Вам понадобится Azure CLI версии **2.0.65** или более поздней версии. Чтобы узнать версию, выполните команду `az --version`. Если вам необходимо выполнить установку или обновление, см. статью [Установка Azure CLI 2.0](/cli/azure/install-azure-cli).

Чтобы установить Virtual Kubelet, установите и настройте [Helm][aks-helm] в кластере AKS. Убедитесь, что ваш Tiller [настроен для использования с Kubernetes RBAC](#for-rbac-enabled-clusters), при необходимости.

### <a name="register-container-instances-feature-provider"></a>Зарегистрировать поставщик функций экземпляры контейнеров

Если вы не использовали ранее службы экземпляра контейнера Azure (ACI), необходимо зарегистрируйте поставщик служб с вашей подпиской. Можно проверить состояние регистрации поставщика ACI с помощью [список поставщиков az][az-provider-list] команды, как показано в следующем примере:

```azurecli-interactive
az provider list --query "[?contains(namespace,'Microsoft.ContainerInstance')]" -o table
```

Поставщик *Microsoft.ContainerInstance* должен иметь состояние *Registered*, как показано в следующем примере выходных данных.

```console
Namespace                    RegistrationState
---------------------------  -------------------
Microsoft.ContainerInstance  Registered
```

Если поставщик отображается в том, что *NotRegistered*, зарегистрируйте поставщик с помощью [az provider register][az-provider-register] как показано в следующем примере:

```azurecli-interactive
az provider register --namespace Microsoft.ContainerInstance
```

### <a name="for-rbac-enabled-clusters"></a>Кластеры с поддержкой RBAC

Если используется кластер AKS с поддержкой RBAC, необходимо создать учетную запись службы и привязку роли для использования с Tiller. Дополнительные сведения см. в разделе [управление доступом на основе ролей Helm][helm-rbac]. Чтобы создать учетную запись службы и привязку роли, создайте файл с именем *rbac-virtual-kubelet.yaml* и вставьте следующее определение.

```yaml
apiVersion: v1
kind: ServiceAccount
metadata:
  name: tiller
  namespace: kube-system
---
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: tiller
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: cluster-admin
subjects:
  - kind: ServiceAccount
    name: tiller
    namespace: kube-system
```

Применение учетной записи службы и привязка с [применить kubectl][kubectl-apply] и укажите ваш *rbac виртуальной kubelet.yaml* файл, как показано в следующем примере:

```console
$ kubectl apply -f rbac-virtual-kubelet.yaml

clusterrolebinding.rbac.authorization.k8s.io/tiller created
```

Настройка Helm для использования учетной записи службы Tiller.

```console
helm init --service-account tiller
```

Теперь можно продолжить установку Virtual Kubelet в кластере AKS.

## <a name="installation"></a>Установка

Используйте [az aks install соединитель][aks-install-connector] команду, чтобы установить Virtual Kubelet. В следующем примере разворачиваются соединители как для Linux, так и для Windows.

```azurecli-interactive
az aks install-connector \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --connector-name virtual-kubelet \
    --os-type Both
```

Эти аргументы доступны для [az aks install соединитель][aks-install-connector] команды.

| Аргумент: | Описание | Обязательно для заполнения |
|---|---|:---:|
| `--connector-name` | Имя соединителя ACI.| Да |
| `--name` `-n` | Имя управляемого кластера. | Да |
| `--resource-group` `-g` | Имя группы ресурсов. | Да |
| `--os-type` | Тип операционной системы экземпляров контейнера. Допустимые значения: Оба, Linux, Windows. По умолчанию: Linux. | Нет |
| `--aci-resource-group` | Группа ресурсов, в которой будут созданы группы контейнеров ACI. | Нет |
| `--location` `-l` | Расположение для создания групп контейнеров ACI. | Нет |
| `--service-principal` | Субъект-служба, используемый для проверки подлинности в API-интерфейсах Azure. | Нет |
| `--client-secret` | Секрет, связанный с субъектом-службой. | Нет |
| `--chart-url` | URL-адрес диаграммы Helm для установки соединителя ACI. | Нет |
| `--image-tag` | Тег образа контейнера Virtual Kubelet. | Нет |

## <a name="validate-virtual-kubelet"></a>Проверка Virtual Kubelet

Чтобы проверить, что был установлен Virtual Kubelet, получения списка узлов Kubernetes с помощью [kubectl получить узлы][kubectl-get] команды:

```console
$ kubectl get nodes

NAME                                             STATUS   ROLES   AGE   VERSION
aks-nodepool1-56577038-0                         Ready    agent   11m   v1.12.8
virtual-kubelet-virtual-kubelet-linux-eastus     Ready    agent   39s   v1.13.1-vk-v0.9.0-1-g7b92d1ee-dev
virtual-kubelet-virtual-kubelet-windows-eastus   Ready    agent   37s   v1.13.1-vk-v0.9.0-1-g7b92d1ee-dev
```

## <a name="run-linux-container"></a>Запуск контейнера Linux

Создайте файл `virtual-kubelet-linux.yaml` и скопируйте в него следующий код YAML. Обратите внимание, [nodeSelector][node-selector] and [toleration][toleration] используются для планирования контейнеров на узле.

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: aci-helloworld
spec:
  replicas: 1
  selector:
    matchLabels:
      app: aci-helloworld
  template:
    metadata:
      labels:
        app: aci-helloworld
    spec:
      containers:
      - name: aci-helloworld
        image: microsoft/aci-helloworld
        ports:
        - containerPort: 80
      nodeSelector:
        beta.kubernetes.io/os: linux
        kubernetes.io/role: agent
        type: virtual-kubelet
      tolerations:
      - key: virtual-kubelet.io/provider
        operator: Equal
        value: azure
        effect: NoSchedule
```

Запускает приложение с [kubectl создать][kubectl-create] команды.

```console
kubectl create -f virtual-kubelet-linux.yaml
```

Используйте [kubectl get pods][kubectl-get] с `-o wide` аргумент для вывода списка модулей с узлом запланированного. Обратите внимание, что pod `aci-helloworld` был назначен на узел `virtual-kubelet-virtual-kubelet-linux`.

```console
$ kubectl get pods -o wide

NAME                              READY   STATUS    RESTARTS   AGE     IP               NODE
aci-helloworld-7b9ffbf946-rx87g   1/1     Running   0          22s     52.224.147.210   virtual-kubelet-virtual-kubelet-linux-eastus
```

## <a name="run-windows-container"></a>Запуск контейнера Windows

Создайте файл `virtual-kubelet-windows.yaml` и скопируйте в него следующий код YAML. Обратите внимание, [nodeSelector][node-selector] and [toleration][toleration] используются для планирования контейнеров на узле.

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nanoserver-iis
spec:
  replicas: 1
  selector:
    matchLabels:
      app: nanoserver-iis
  template:
    metadata:
      labels:
        app: nanoserver-iis
    spec:
      containers:
      - name: nanoserver-iis
        image: microsoft/iis:nanoserver
        ports:
        - containerPort: 80
      nodeSelector:
        beta.kubernetes.io/os: windows
        kubernetes.io/role: agent
        type: virtual-kubelet
      tolerations:
      - key: virtual-kubelet.io/provider
        operator: Equal
        value: azure
        effect: NoSchedule
```

Запускает приложение с [kubectl создать][kubectl-create] команды.

```console
kubectl create -f virtual-kubelet-windows.yaml
```

Используйте [kubectl get pods][kubectl-get] с `-o wide` аргумент для вывода списка модулей с узлом запланированного. Обратите внимание, что pod `nanoserver-iis` был назначен на узел `virtual-kubelet-virtual-kubelet-windows`.

```console
$ kubectl get pods -o wide

NAME                              READY   STATUS    RESTARTS   AGE     IP               NODE
nanoserver-iis-5d999b87d7-6h8s9   1/1     Running   0          47s     52.224.143.39    virtual-kubelet-virtual-kubelet-windows-eastus
```

## <a name="remove-virtual-kubelet"></a>Удаление Virtual Kubelet

Используйте [az aks remove-connector][aks-remove-connector] команду, чтобы удалить Virtual Kubelet. Замените значения аргументов на имя соединителя, кластера AKS и группы ресурсов кластера AKS.

```azurecli-interactive
az aks remove-connector \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --connector-name virtual-kubelet \
    --os-type Both
```

> [!NOTE]
> Если возникают ошибки при удалении соединителей обоих ОС или вы хотите удалить только соединитель Windows или Linux, тип операционной системы можно указать вручную. Добавьте параметр `--os-type` к предыдущей команде `az aks remove-connector` и укажите ОС: `Windows` или `Linux`.

## <a name="next-steps"></a>Следующие шаги

Возможные причины этой проблемы с Virtual Kubelet, см. в разделе [известные совместимости и обходные пути][vk-troubleshooting]. To report problems with the Virtual Kubelet, [open a GitHub issue][vk-issues].

Дополнительные сведения о Virtual Kubelet в [проекта GitHub виртуального Kubelet][vk-github].

<!-- LINKS - internal -->
[aks-quick-start]: ./kubernetes-walkthrough.md
[aks-remove-connector]: /cli/azure/aks#az-aks-remove-connector
[az-container-list]: /cli/azure/aks#az-aks-list
[aks-install-connector]: /cli/azure/aks#az-aks-install-connector
[virtual-nodes-cli]: virtual-nodes-cli.md
[virtual-nodes-portal]: virtual-nodes-portal.md
[aks-helm]: kubernetes-helm.md
[az-provider-list]: /cli/azure/provider#az-provider-list
[az-provider-register]: /cli/azure/provider#az-provider-register

<!-- LINKS - external -->
[kubectl-create]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#create
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[node-selector]:https://kubernetes.io/docs/concepts/configuration/assign-pod-node/
[toleration]: https://kubernetes.io/docs/concepts/configuration/taint-and-toleration/
[vk-github]: https://github.com/virtual-kubelet/virtual-kubelet
[helm-rbac]: https://docs.helm.sh/using_helm/#role-based-access-control
[kubectl-apply]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#apply
[vk-troubleshooting]: https://github.com/virtual-kubelet/virtual-kubelet#known-quirks-and-workarounds
[vk-issues]: https://github.com/virtual-kubelet/virtual-kubelet/issues
