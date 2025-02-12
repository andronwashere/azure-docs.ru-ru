---
title: Настройка CoreDNS для службы Azure Kubernetes (AKS)
description: Дополнительные сведения о настройке CoreDNS Добавление поддоменов или расширить пользовательские конечные точки DNS, с помощью службы Azure Kubernetes (AKS)
services: container-service
author: jnoller
ms.service: container-service
ms.topic: article
ms.date: 03/15/2019
ms.author: jenoller
ms.openlocfilehash: 247665f58dd064565f0e9aebc9859e97ce0ab0c0
ms.sourcegitcommit: 64798b4f722623ea2bb53b374fb95e8d2b679318
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/11/2019
ms.locfileid: "67836981"
---
# <a name="customize-coredns-with-azure-kubernetes-service"></a>Настройка CoreDNS со службой Azure Kubernetes

Служба Azure Kubernetes (AKS) использует [CoreDNS][coredns] проекта для управления DNS кластера и разрешение со всеми *1.12.x* и более поздней версии кластеров. Ранее был использован проекта kube-dns. Этот проект kube-dns теперь является устаревшим. Дополнительные сведения о настройке CoreDNS и Kubernetes, см. в разделе [официальной документации вышестоящего][corednsk8s].

А AKS — это управляемая служба, нельзя изменить основной конфигурации для CoreDNS ( *CoreFile*). Вместо этого использовать Kubernetes *ConfigMap* переопределять параметры по умолчанию. Чтобы просмотреть AKS CoreDNS ConfigMaps по умолчанию, используйте `kubectl get configmaps coredns -o yaml` команды.

В этой статье показано, как использовать ConfigMaps базовой настройки параметров CoreDNS в AKS.

> [!NOTE]
> `kube-dns` предлагаются различные [параметры настройки][kubednsblog] через карту конфигурации Kubernetes. — CoreDNS **не** обратную совместимость с kube-dns. Необходимо обновить все настройки, который использовался ранее для использования с CoreDNS.

## <a name="before-you-begin"></a>Перед началом работы

В этой статье предполагается, что у вас есть кластер AKS. Если вам нужен кластер AKS, см. в этом кратком руководстве AKS [с помощью Azure CLI][aks-quickstart-cli] or [using the Azure portal][aks-quickstart-portal].

## <a name="what-is-supportedunsupported"></a>Новые возможности, поддерживаемые и неподдерживаемые

Поддерживаются все встроенные подключаемые модули CoreDNS. Поддерживаются не add-on/сторонние подключаемые модули. 

## <a name="rewrite-dns"></a>Перепишите DNS

Один сценарий, к которым у вас есть — для выполнения операции перезаписи имя DNS в режиме реального времени. В следующем примере замените `<domain to be written>` собственным полным доменным именем. Создайте файл с именем `corednsms.yaml` и вставьте следующий пример конфигурации:

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: coredns-custom
  namespace: kube-system
data:
  test.server: |
    <domain to be rewritten>.com:53 {
        errors
        cache 30
        rewrite name substring <domain to be rewritten>.com default.svc.cluster.local
        proxy .  /etc/resolv.conf # you can redirect this to a specific DNS server such as 10.0.0.10
    }
```

Создается при помощи ConfigMap [kubectl применить configmap][kubectl-apply] команду и укажите имя yaml-ФАЙЛ манифеста:

```console
kubectl apply -f corednsms.yaml
```

Чтобы проверить, были применены настройки, используйте [kubectl get configmaps][kubectl-get] и укажите ваш *coredns-custom* ConfigMap:

```
kubectl get configmaps --namespace=kube-system coredns-custom -o yaml
```

Теперь принудительно CoreDNS перезагрузить ConfigMap. [Kubectl удалить pod][kubectl delete] команда не уничтожения данных и не вызывает простоя. `kube-dns` Модулей будут удалены, а планировщик Kubernetes заново их. Эти новые модули содержат изменение значение срока ЖИЗНИ.

```console
kubectl delete pod --namespace kube-system -l k8s-app=kube-dns
```

> [!Note]
> Приведенная выше команда является правильным. Хотя мы Меняем `coredns`, выполняется развертывание **kube-dns** имя.

## <a name="custom-proxy-server"></a>Пользовательский прокси-сервер

Если вам нужно указать прокси-сервер для сетевого трафика, можно создать ConfigMap для настройки DNS. В следующем примере обновите `proxy` имя и адрес со значениями для конкретной среды. Создайте файл с именем `corednsms.yaml` и вставьте следующий пример конфигурации:

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: coredns-custom
  namespace: kube-system
data:
  test.server: | # you may select any name here, but it must end with the .server file extension
    <domain to be rewritten>.com:53 {
        proxy foo.com 1.1.1.1
    }
```

Как и в предыдущих примерах создается при помощи ConfigMap [kubectl применить configmap][kubectl-apply] command and specify the name of your YAML manifest. Then, force CoreDNS to reload the ConfigMap using the [kubectl delete pod][kubectl delete] планировщик Kubernetes должен повторно создать их:

```console
kubectl apply -f corednsms.yaml
kubectl delete pod --namespace kube-system --label k8s-app=kube-dns
```

## <a name="use-custom-domains"></a>Использовать личные домены

Может потребоваться настройка личных доменов, которые можно разрешить только внутренним образом. Например, может потребоваться разрешить пользовательский домен *puglife.local*, который не является допустимым доменом верхнего уровня. Без личного домена ConfigMap в кластере AKS не удается разрешить адрес.

В следующем примере обновление личного домена и IP-адрес передавать трафик со значениями для конкретной среды. Создайте файл с именем `corednsms.yaml` и вставьте следующий пример конфигурации:

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: coredns-custom
  namespace: kube-system
data:
  puglife.server: |
    puglife.local:53 {
        errors
        cache 30
        proxy . 192.11.0.1  # this is my test/dev DNS server
    }
```

Как и в предыдущих примерах создается при помощи ConfigMap [kubectl применить configmap][kubectl-apply] command and specify the name of your YAML manifest. Then, force CoreDNS to reload the ConfigMap using the [kubectl delete pod][kubectl delete] планировщик Kubernetes должен повторно создать их:

```console
kubectl apply -f corednsms.yaml
kubectl delete pod --namespace kube-system --label k8s-app=kube-dns
```

## <a name="stub-domains"></a>Заглушки доменов

CoreDNS также может использоваться для настройки доменов заглушки. В следующем примере обновление пользовательских доменов и IP-адресов со значениями для конкретной среды. Создайте файл с именем `corednsms.yaml` и вставьте следующий пример конфигурации:

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: coredns-custom
  namespace: kube-system
data:
  test.server: |
    abc.com:53 {
        errors
        cache 30
        proxy . 1.2.3.4
    }
    my.cluster.local:53 {
        errors
        cache 30
        proxy . 2.3.4.5
    }

```

Как и в предыдущих примерах создается при помощи ConfigMap [kubectl применить configmap][kubectl-apply] command and specify the name of your YAML manifest. Then, force CoreDNS to reload the ConfigMap using the [kubectl delete pod][kubectl delete] планировщик Kubernetes должен повторно создать их:

```console
kubectl apply -f corednsms.yaml
kubectl delete pod --namespace kube-system --label k8s-app=kube-dns
```

## <a name="hosts-plugin"></a>Подключаемый модуль узлов

Поддерживаются все встроенные подключаемые модули это означает, что CoreDNS [узлы][coredns hosts] подключаемый модуль доступен для настройки также:

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: coredns-custom # this is the name of the configmap you can overwrite with your changes
  namespace: kube-system
data:
    test.override: |
          hosts example.hosts example.org { # example.hosts must be a file
              10.0.0.1 example.org
              fallthrough
          }
```

## <a name="next-steps"></a>Следующие шаги

В этой статье показано несколько примеров сценариев для настройки CoreDNS. Сведения о проекте CoreDNS, см. в разделе [страницы вышестоящего проекта CoreDNS][coredns].

Дополнительные сведения об основных концепциях сети, см. в разделе [сети основные понятия для приложений в AKS][concepts-network].

<!-- LINKS - external -->
[kubednsblog]: https://www.danielstechblog.io/using-custom-dns-server-for-domain-specific-name-resolution-with-azure-kubernetes-service/
[coredns]: https://coredns.io/
[corednsk8s]: https://kubernetes.io/docs/tasks/administer-cluster/dns-custom-nameservers/#coredns
[dnscache]: https://coredns.io/plugins/cache/
[kubectl-apply]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#apply
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[kubectl delete]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#delete
[coredns hosts]: https://coredns.io/plugins/hosts/

<!-- LINKS - internal -->
[concepts-network]: concepts-network.md
[aks-quickstart-cli]: kubernetes-walkthrough.md
[aks-quickstart-portal]: kubernetes-walkthrough-portal.md
