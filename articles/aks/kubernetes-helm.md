---
title: Развертывание контейнеров с помощью Helm в Kubernetes в Azure
description: Узнайте, как использовать средства упаковки Helm для развертывания контейнеров в кластере Azure Kubernetes Service (AKS)
services: container-service
author: zr-msft
ms.service: container-service
ms.topic: article
ms.date: 05/23/2019
ms.author: zarhoads
ms.openlocfilehash: 76a5391cbe142851d9b1f60ea9346af2e7a35d6a
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "66392140"
---
# <a name="install-applications-with-helm-in-azure-kubernetes-service-aks"></a>Установка приложения с помощью Helm в службе Azure Kubernetes

[Helm][helm] — это средство упаковки с открытым кодом, которое помогает установить приложения Kubernetes и управлять их жизненным циклом. Аналогично диспетчерам пакетов Linux, таких как *APT* и *Yum*, Helm используется для управления чартами Kubernetes, представляющими собой пакеты предварительно настроенных ресурсов Kubernetes.

В этой статье показано, как настроить и использовать Helm в кластере Kubernetes в AKS.

## <a name="before-you-begin"></a>Перед началом работы

В этой статье предполагается, что у вас есть кластер AKS. Если вам нужен кластер AKS, обратитесь к этому краткому руководству по работе с AKS [с помощью Azure CLI][aks-quickstart-cli] или [портала Azure][aks-quickstart-portal].

Вам также потребуется интерфейс командной строки Helm установлен, который является клиент, который запускается в системе разработки. Он позволяет запуск, остановка и управление приложениями с помощью Helm. Если вы используете Azure Cloud Shell, интерфейс командной строки Helm уже установлен. Инструкции по установке для вашей локальной платформы см. в разделе [Установка Helm][helm-install].

> [!IMPORTANT]
> Helm должен работать на узлах Linux. При наличии узлов Windows Server в кластере, необходимо убедиться, что Helm модулей только планируются для запуска на узлах Linux. Также необходимо убедиться, что любой чарты Helm, устанавливаемым также планируются для запуска на правильные узлы. Команды в этой статье используется [узел селекторы] [ k8s-node-selector] чтобы убедиться в том, модулей планируется правильные узлы, но не всех диаграмм Helm могут предоставлять имя узла. Также можно использовать другие параметры в кластере, такие как [taints][taints].

## <a name="create-a-service-account"></a>Создание учетной записи службы

Перед развертыванием Helm в кластере AKS с поддержкой RBAC необходимо создать учетную запись службы и привязку роли для службы Tiller. Дополнительные сведения о защите Helm и Tiller в кластере с поддержкой RBAC см. в руководстве по [использованию Tiller, пространств имен и RBAC][tiller-rbac]. Если в кластере AKS не включена поддержка RBAC, пропустите этот шаг.

Создайте файл `helm-rbac.yaml` и скопируйте в него следующий код YAML:

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

Создайте учетную запись службы и привязку роли с помощью команды `kubectl apply`:

```console
kubectl apply -f helm-rbac.yaml
```

## <a name="secure-tiller-and-helm"></a>Защита Tiller и Helm

Клиент Helm и служба Tiller проверяют подлинность и взаимодействуют друг с другом с помощью протокола TLS/SSL. Этот метод проверки подлинности позволяет обеспечить безопасность кластера Kubernetes и развертываемых служб. Для повышения безопасности можно создавать собственные подписанные сертификаты. Каждый пользователь Helm получит собственный сертификат клиента, и инициализация Tiller в кластере Kubernetes будет выполняться с помощью сертификатов. Дополнительные сведения см. в статье [Using TLS/SSL between Helm and Tiller][helm-ssl].

Благодаря кластеру Kubernetes с поддержкой RBAC вы можете контролировать уровень доступа Tiller в кластере. Можно определить пространство имен Kubernetes, в котором развертывается Tiller, и ограничить пространства имен, в которых Tiller может развертывать ресурсы. Такой подход позволяет создавать экземпляры Tiller в разных пространствах имен и определять границы развертывания, а также ограничивать область пользователей клиента Helm определенными пространствами имен. Дополнительные сведения см. в статье об [управлении доступом на основе ролей Helm][helm-rbac].

## <a name="configure-helm"></a>Настройка Helm

Чтобы развернуть базовую службу Tiller в кластере AKS, используйте команду [helm init][helm-init]. Если в кластере не включена поддержка RBAC, удалите значение и аргумент `--service-account`. Если вы настроили TLS/SSL для Tiller и Helm, пропустите эту процедуру базовой инициализации и укажите необходимый `--tiller-tls-`, как показано в следующем примере.

```console
helm init --service-account tiller --node-selectors "beta.kubernetes.io/os"="linux"
```

Если вы настроили подключение TLS/SSL между Helm и Tiller, укажите параметры `--tiller-tls-*` и имена собственных сертификатов, как показано в следующем примере:

```console
helm init \
    --tiller-tls \
    --tiller-tls-cert tiller.cert.pem \
    --tiller-tls-key tiller.key.pem \
    --tiller-tls-verify \
    --tls-ca-cert ca.cert.pem \
    --service-account tiller \
    --node-selectors "beta.kubernetes.io/os"="linux"
```

## <a name="find-helm-charts"></a>Поиск чартов Helm

Чарты Helm используются для развертывания приложений в кластере Kubernetes. Чтобы найти предварительно созданные чарты Helm, используйте команду [helm search][helm-search].

```console
helm search
```

В следующем кратком примере выходные данные содержат некоторые из доступных для использования чартов Helm:

```
$ helm search

NAME                           CHART VERSION    APP VERSION  DESCRIPTION
stable/aerospike               0.1.7            v3.14.1.2    A Helm chart for Aerospike in Kubernetes
stable/anchore-engine          0.1.7            0.1.10       Anchore container analysis and policy evaluatio...
stable/apm-server              0.1.0            6.2.4        The server receives data from the Elastic APM a...
stable/ark                     1.0.1            0.8.2        A Helm chart for ark
stable/artifactory             7.2.1            6.0.0        Universal Repository Manager supporting all maj...
stable/artifactory-ha          0.2.1            6.0.0        Universal Repository Manager supporting all maj...
stable/auditbeat               0.1.0            6.2.4        A lightweight shipper to audit the activities o...
stable/aws-cluster-autoscaler  0.3.3                         Scales worker nodes within autoscaling groups.
stable/bitcoind                0.1.3            0.15.1       Bitcoin is an innovative payment network and a ...
stable/buildkite               0.2.3            3            Agent for Buildkite
stable/burrow                  0.4.4            0.17.1       Burrow is a permissionable smart contract machine
stable/centrifugo              2.0.1            1.7.3        Centrifugo is a real-time messaging server.
stable/cerebro                 0.1.0            0.7.3        A Helm chart for Cerebro - a web admin tool tha...
stable/cert-manager            v0.3.3           v0.3.1       A Helm chart for cert-manager
stable/chaoskube               0.7.0            0.8.0        Chaoskube periodically kills random pods in you...
stable/chartmuseum             1.5.0            0.7.0        Helm Chart Repository with support for Amazon S...
stable/chronograf              0.4.5            1.3          Open-source web application written in Go and R...
stable/cluster-autoscaler      0.6.4            1.2.2        Scales worker nodes within autoscaling groups.
stable/cockroachdb             1.1.1            2.0.0        CockroachDB is a scalable, survivable, strongly...
stable/concourse               1.10.1           3.14.1       Concourse is a simple and scalable CI system.
stable/consul                  3.2.0            1.0.0        Highly available and distributed service discov...
stable/coredns                 0.9.0            1.0.6        CoreDNS is a DNS server that chains plugins and...
stable/coscale                 0.2.1            3.9.1        CoScale Agent
stable/dask                    1.0.4            0.17.4       Distributed computation in Python with task sch...
stable/dask-distributed        2.0.2                         DEPRECATED: Distributed computation in Python
stable/datadog                 0.18.0           6.3.0        DataDog Agent
...
```

Чтобы обновить список чартов, используйте команду [helm repo update][helm-repo-update]. Ниже приведен пример успешного обновления репозитория:

```console
$ helm repo update

Hold tight while we grab the latest from your chart repositories...
...Skip local chart repository
...Successfully got an update from the "stable" chart repository
Update Complete. ⎈ Happy Helming!⎈
```

## <a name="run-helm-charts"></a>Выполнение чартов Helm

Чтобы установить чарты с помощью Helm, используйте команду [helm install][helm-install] и укажите имя чарта для установки. Чтобы увидеть, установка чарт Helm в действии, давайте установим развертывание основных nginx с помощью чарта Helm. Если вы настроили TLS/SSL, добавьте параметр `--tls`, чтобы использовать сертификат клиента Helm.

```console
helm install stable/nginx-ingress \
    --set controller.nodeSelector."beta\.kubernetes\.io/os"=linux \
    --set defaultBackend.nodeSelector."beta\.kubernetes\.io/os"=linux
```

В следующем кратком примере выходные данные содержат состояние развертывания ресурсов Kubernetes, созданное чартом Helm:

```
$ helm install stable/nginx-ingress --set controller.nodeSelector."beta\.kubernetes\.io/os"=linux --set defaultBackend.nodeSelector."beta\.kubernetes\.io/os"=linux

NAME:   flailing-alpaca
LAST DEPLOYED: Thu May 23 12:55:21 2019
NAMESPACE: default
STATUS: DEPLOYED

RESOURCES:
==> v1/ConfigMap
NAME                                      DATA  AGE
flailing-alpaca-nginx-ingress-controller  1     0s

==> v1/Pod(related)
NAME                                                            READY  STATUS             RESTARTS  AGE
flailing-alpaca-nginx-ingress-controller-56666dfd9f-bq4cl       0/1    ContainerCreating  0         0s
flailing-alpaca-nginx-ingress-default-backend-66bc89dc44-m87bp  0/1    ContainerCreating  0         0s

==> v1/Service
NAME                                           TYPE          CLUSTER-IP  EXTERNAL-IP  PORT(S)                     AGE
flailing-alpaca-nginx-ingress-controller       LoadBalancer  10.0.109.7  <pending>    80:31219/TCP,443:32421/TCP  0s
flailing-alpaca-nginx-ingress-default-backend  ClusterIP     10.0.44.97  <none>       80/TCP                      0s
...
```

Занимает одну-две для минуты *EXTERNAL-IP* адрес службы контроллера входящего трафика nginx заполнены и позволяют получить доступ к веб-браузера.

## <a name="list-helm-releases"></a>Список выпусков Helm

Чтобы просмотреть список выпусков, установленных в кластере, используйте команду [helm list][helm-list]. В следующем примере показано nginx ingress выпуск, развернутый на предыдущем шаге. Если вы настроили TLS/SSL, добавьте параметр `--tls`, чтобы использовать сертификат клиента Helm.

```console
$ helm list

NAME                REVISION    UPDATED                     STATUS      CHART                 APP VERSION   NAMESPACE
flailing-alpaca   1         Thu May 23 12:55:21 2019    DEPLOYED    nginx-ingress-1.6.13    0.24.1      default
```

## <a name="clean-up-resources"></a>Очистка ресурсов

При развертывании диаграммы Helm создается несколько ресурсов Kubernetes. К ним относятся элементы pod, развертывания и службы. Для очистки этих ресурсов используйте команду `helm delete` и укажите имя выпуска, как описано в предыдущей команде `helm list`. В следующем примере удаляется выпуска с именем *flailing alpaca*:

```console
$ helm delete flailing-alpaca

release "flailing-alpaca" deleted
```

## <a name="next-steps"></a>Дальнейшие действия

Дополнительные сведения об управлении развертываниями приложений Kubernetes с помощью Helm см. в документации по Helm.

> [!div class="nextstepaction"]
> [Документация по Helm][helm-documentation]

<!-- LINKS - external -->
[helm]: https://github.com/kubernetes/helm/
[helm-documentation]: https://docs.helm.sh/
[helm-init]: https://docs.helm.sh/helm/#helm-init
[helm-install]: https://docs.helm.sh/using_helm/#installing-helm
[helm-install-options]: https://github.com/kubernetes/helm/blob/master/docs/install.md
[helm-list]: https://docs.helm.sh/helm/#helm-list
[helm-rbac]: https://docs.helm.sh/using_helm/#role-based-access-control
[helm-repo-update]: https://docs.helm.sh/helm/#helm-repo-update
[helm-search]: https://docs.helm.sh/helm/#helm-search
[tiller-rbac]: https://docs.helm.sh/using_helm/#tiller-namespaces-and-rbac
[helm-ssl]: https://docs.helm.sh/using_helm/#using-ssl-between-helm-and-tiller

<!-- LINKS - internal -->
[aks-quickstart-cli]: kubernetes-walkthrough.md
[aks-quickstart-portal]: kubernetes-walkthrough-portal.md
[install-azure-cli]: /cli/azure/install-azure-cli
[k8s-node-selector]: concepts-clusters-workloads.md#node-selectors
[taints]: operator-best-practices-advanced-scheduler.md