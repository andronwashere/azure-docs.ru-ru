---
title: Надстройка маршрутизации приложений HTTP в Службе Azure Kubernetes (AKS)
description: Использование надстройки маршрутизации приложений HTTP в Службе Azure Kubernetes (AKS).
services: container-service
author: lachie83
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 04/25/2018
ms.author: laevenso
ms.openlocfilehash: d6e1cc033416c90e27b5caf4bba310400e55b3a5
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "60466332"
---
# <a name="http-application-routing"></a>Маршрутизация приложений HTTP

Решение маршрутизации приложений HTTP упрощает доступ к приложениям, развернутым в кластере Службы Azure Kubernetes (AKS). Если решение включено, то оно настраивает контроллер входящего трафика в кластере службы AKS. Когда приложения развернуты, решение также создает общедоступные DNS-имена для конечных точек приложений.

При включении этой надстройки в подписке создается зона DNS. Дополнительные сведения о стоимости DNS см. [на этой странице][dns-pricing].

> [!CAUTION]
> Надстройка маршрутизации приложения HTTP предназначена для быстрого создания входного контроллера и доступа к вашим приложениям. Мы не рекомендуем ее использовать для работы. Дополнительные сведения о готовых к развертыванию приложений, включающих несколько реплик и поддержку TLS, см. раздел [Создание входного контроллера HTTPS](https://docs.microsoft.com/azure/aks/ingress-tls).

## <a name="http-routing-solution-overview"></a>Обзор решения маршрутизации HTTP-трафика

Надстройка развертывает два компонента: [контроллер входящего трафика Службы Azure Kubernetes][ingress] и контроллер [внешних DNS][external-dns].

- **Контроллер входящего трафика**. Доступ к контроллеру входящего трафика в Интернете предоставляется с помощью службы Azure Kubernetes типа LoadBalancer. Контроллер входящего трафика отслеживает и реализует [ресурсы входящего трафика Службы Azure Kubernetes][ingress-resource], которые создают маршруты к конечным точкам приложения.
- **Контроллер внешних DNS**. Отслеживает ресурсы входящего трафика службы Azure Kubernetes и создает DNS-записи A в зоне DNS определенного кластера.

## <a name="deploy-http-routing-cli"></a>Развертывание маршрутизации HTTP-трафика Интерфейс командной строки

Надстройку для маршрутизации приложений HTTP можно активировать с помощью Azure CLI при развертывании кластера AKS. Для этого используйте команду [az aks create][az-aks-create] с аргументом `--enable-addons`.

```azurecli
az aks create --resource-group myResourceGroup --name myAKSCluster --enable-addons http_application_routing
```

> [!TIP]
> Если вы хотите включить несколько надстроек, укажите их в виде списка через запятую. Например, чтобы включить маршрутизацию и мониторинг приложений HTTP, используйте формат `--enable-addons http_application_routing,monitoring`.

Можно также включить маршрутизацию HTTP для существующего кластера AKS с помощью команды [az aks enable-addons][az-aks-enable-addons]. Чтобы включить маршрутизацию HTTP в существующем кластере, добавьте параметр `--addons` и укажите *http_application_routing*, как показано в следующем примере.

```azurecli
az aks enable-addons --resource-group myResourceGroup --name myAKSCluster --addons http_application_routing
```

После развертывания или обновления кластера получите имя зоны DNS с помощью команды [az aks show][az-aks-show]. Оно потребуется, чтобы развертывать приложения в кластер службы AKS.

```azurecli
$ az aks show --resource-group myResourceGroup --name myAKSCluster --query addonProfiles.httpApplicationRouting.config.HTTPApplicationRoutingZoneName -o table

Result
-----------------------------------------------------
9f9c1fe7-21a1-416d-99cd-3543bb92e4c3.eastus.aksapp.io
```

## <a name="deploy-http-routing-portal"></a>Развертывание маршрутизации HTTP-трафика Портал

При развертывании кластера службы AKS надстройку для маршрутизации приложений HTTP можно активировать на портале Azure.

![Включение функции маршрутизации HTTP-трафика](media/http-routing/create.png)

После развертывания кластера перейдите к автоматически созданной группе ресурсов службы AKS и выберите зону DNS. Запишите имя зоны DNS. Оно потребуется, чтобы развертывать приложения в кластер службы AKS.

![Получение имени зоны DNS](media/http-routing/dns.png)

## <a name="use-http-routing"></a>Использование маршрутизации HTTP-трафика

Решение маршрутизации приложений HTTP можно активировать только в ресурсах входящего трафика помеченных следующим образом.

```yaml
annotations:
  kubernetes.io/ingress.class: addon-http-application-routing
```

Создайте файл с именем **samples-http-application-routing.yaml** и скопируйте его в следующий YAML. В строке 43 обновите `<CLUSTER_SPECIFIC_DNS_ZONE>`, указав имя зоны DNS, полученное на предыдущем шаге этой статьи.


```yaml
apiVersion: extensions/v1beta1
kind: Deployment
metadata:
  name: party-clippy
spec:
  template:
    metadata:
      labels:
        app: party-clippy
    spec:
      containers:
      - image: r.j3ss.co/party-clippy
        name: party-clippy
        resources:
          requests:
            cpu: 100m
            memory: 128Mi
          limits:
            cpu: 250m
            memory: 256Mi
        tty: true
        command: ["party-clippy"]
        ports:
        - containerPort: 8080
---
apiVersion: v1
kind: Service
metadata:
  name: party-clippy
spec:
  ports:
  - port: 80
    protocol: TCP
    targetPort: 8080
  selector:
    app: party-clippy
  type: ClusterIP
---
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: party-clippy
  annotations:
    kubernetes.io/ingress.class: addon-http-application-routing
spec:
  rules:
  - host: party-clippy.<CLUSTER_SPECIFIC_DNS_ZONE>
    http:
      paths:
      - backend:
          serviceName: party-clippy
          servicePort: 80
        path: /
```

Выполните команду [kubectl apply][kubectl-apply], чтобы создать ресурсы.

```bash
$ kubectl apply -f samples-http-application-routing.yaml

deployment "party-clippy" created
service "party-clippy" created
ingress "party-clippy" created
```

Используйте c URL или браузер для перехода к имени узла, указанному в разделе узле файла samples-http-application-routing.yaml. Прежде чем приложение станет доступным через Интернет, может пройти около одной минуты.

```bash
$ curl party-clippy.471756a6-e744-4aa0-aa01-89c4d162a7a7.canadaeast.aksapp.io

 _________________________________
/ It looks like you're building a \
\ microservice.                   /
 ---------------------------------
 \
  \
     __
    /  \
    |  |
    @  @
    |  |
    || |/
    || ||
    |\_/|
    \___/

```

## <a name="remove-http-routing"></a>Удаление маршрутизации HTTP

Решение для маршрутизации HTTP можно удалить с помощью Azure CLI. Для этого выполните следующую команду, заменив свой кластер AKS и имя группы ресурсов.

```azurecli
az aks disable-addons --addons http_application_routing --name myAKSCluster --resource-group myResourceGroup --no-wait
```

При отключении надстройки маршрутизации приложений HTTP некоторые ресурсы Kubernetes могут оставаться в кластере. Эти ресурсы включают объекты *configMap* и *секреты* и создаются в пространстве имен *kube-system*. Чтобы поддерживать чистоту кластера, можно удалить эти ресурсы.

Найдите ресурсы *addon-http-application-routing* с помощью следующих команд [kubectl get][kubectl-get].

```console
kubectl get deployments --namespace kube-system
kubectl get services --namespace kube-system
kubectl get configmaps --namespace kube-system
kubectl get secrets --namespace kube-system
```

В следующем примере выходных данных показаны объекты configMap, подлежащие удалению.

```
$ kubectl get configmaps --namespace kube-system

NAMESPACE     NAME                                                       DATA   AGE
kube-system   addon-http-application-routing-nginx-configuration         0      9m7s
kube-system   addon-http-application-routing-tcp-services                0      9m7s
kube-system   addon-http-application-routing-udp-services                0      9m7s
```

Чтобы удалить ресурсы, используйте команду [kubectl delete][kubectl-delete]. Укажите тип ресурса, имя ресурса и пространство имен. В следующем примере удаляется один из предыдущих объектов configmap.

```console
kubectl delete configmaps addon-http-application-routing-nginx-configuration --namespace kube-system
```

Повторите предыдущий шаг `kubectl delete` для всех ресурсов *addon-http-application-routing*, которые остаются в кластере.

## <a name="troubleshoot"></a>Устранение неполадок

Выполните команду [kubectl logs][kubectl-logs], чтобы просмотреть журналы приложений для внешнего приложения DNS. В журналах должно быть указано, что записи A и TXT DNS успешно созданы.

```
$ kubectl logs -f deploy/addon-http-application-routing-external-dns -n kube-system

time="2018-04-26T20:36:19Z" level=info msg="Updating A record named 'party-clippy' to '52.242.28.189' for Azure DNS zone '471756a6-e744-4aa0-aa01-89c4d162a7a7.canadaeast.aksapp.io'."
time="2018-04-26T20:36:21Z" level=info msg="Updating TXT record named 'party-clippy' to '"heritage=external-dns,external-dns/owner=default"' for Azure DNS zone '471756a6-e744-4aa0-aa01-89c4d162a7a7.canadaeast.aksapp.io'."
```

Эти записи также можно увидеть в ресурсе зоны DNS на портале Azure.

![Получение записей DNS](media/http-routing/clippy.png)

Выполните команду [kubectl logs][kubectl-logs], чтобы просмотреть журналы приложений для контроллера входящего трафика NGINX. В журналах должно быть указано, что `CREATE` ресурса входящего трафика создан и контроллер перезагружен. Все действия по протоколу HTTP регистрируются в журнале.

```bash
$ kubectl logs -f deploy/addon-http-application-routing-nginx-ingress-controller -n kube-system

-------------------------------------------------------------------------------
NGINX Ingress controller
  Release:    0.13.0
  Build:      git-4bc943a
  Repository: https://github.com/kubernetes/ingress-nginx
-------------------------------------------------------------------------------

I0426 20:30:12.212936       9 flags.go:162] Watching for ingress class: addon-http-application-routing
W0426 20:30:12.213041       9 flags.go:165] only Ingress with class "addon-http-application-routing" will be processed by this ingress controller
W0426 20:30:12.213505       9 client_config.go:533] Neither --kubeconfig nor --master was specified.  Using the inClusterConfig.  This might not work.
I0426 20:30:12.213752       9 main.go:181] Creating API client for https://10.0.0.1:443
I0426 20:30:12.287928       9 main.go:225] Running in Kubernetes Cluster version v1.8 (v1.8.11) - git (clean) commit 1df6a8381669a6c753f79cb31ca2e3d57ee7c8a3 - platform linux/amd64
I0426 20:30:12.290988       9 main.go:84] validated kube-system/addon-http-application-routing-default-http-backend as the default backend
I0426 20:30:12.294314       9 main.go:105] service kube-system/addon-http-application-routing-nginx-ingress validated as source of Ingress status
I0426 20:30:12.426443       9 stat_collector.go:77] starting new nginx stats collector for Ingress controller running in namespace  (class addon-http-application-routing)
I0426 20:30:12.426509       9 stat_collector.go:78] collector extracting information from port 18080
I0426 20:30:12.448779       9 nginx.go:281] starting Ingress controller
I0426 20:30:12.463585       9 event.go:218] Event(v1.ObjectReference{Kind:"ConfigMap", Namespace:"kube-system", Name:"addon-http-application-routing-nginx-configuration", UID:"2588536c-4990-11e8-a5e1-0a58ac1f0ef2", APIVersion:"v1", ResourceVersion:"559", FieldPath:""}): type: 'Normal' reason: 'CREATE' ConfigMap kube-system/addon-http-application-routing-nginx-configuration
I0426 20:30:12.466945       9 event.go:218] Event(v1.ObjectReference{Kind:"ConfigMap", Namespace:"kube-system", Name:"addon-http-application-routing-tcp-services", UID:"258ca065-4990-11e8-a5e1-0a58ac1f0ef2", APIVersion:"v1", ResourceVersion:"561", FieldPath:""}): type: 'Normal' reason: 'CREATE' ConfigMap kube-system/addon-http-application-routing-tcp-services
I0426 20:30:12.467053       9 event.go:218] Event(v1.ObjectReference{Kind:"ConfigMap", Namespace:"kube-system", Name:"addon-http-application-routing-udp-services", UID:"259023bc-4990-11e8-a5e1-0a58ac1f0ef2", APIVersion:"v1", ResourceVersion:"562", FieldPath:""}): type: 'Normal' reason: 'CREATE' ConfigMap kube-system/addon-http-application-routing-udp-services
I0426 20:30:13.649195       9 nginx.go:302] starting NGINX process...
I0426 20:30:13.649347       9 leaderelection.go:175] attempting to acquire leader lease  kube-system/ingress-controller-leader-addon-http-application-routing...
I0426 20:30:13.649776       9 controller.go:170] backend reload required
I0426 20:30:13.649800       9 stat_collector.go:34] changing prometheus collector from  to default
I0426 20:30:13.662191       9 leaderelection.go:184] successfully acquired lease kube-system/ingress-controller-leader-addon-http-application-routing
I0426 20:30:13.662292       9 status.go:196] new leader elected: addon-http-application-routing-nginx-ingress-controller-5cxntd6
I0426 20:30:13.763362       9 controller.go:179] ingress backend successfully reloaded...
I0426 21:51:55.249327       9 event.go:218] Event(v1.ObjectReference{Kind:"Ingress", Namespace:"default", Name:"party-clippy", UID:"092c9599-499c-11e8-a5e1-0a58ac1f0ef2", APIVersion:"extensions", ResourceVersion:"7346", FieldPath:""}): type: 'Normal' reason: 'CREATE' Ingress default/party-clippy
W0426 21:51:57.908771       9 controller.go:775] service default/party-clippy does not have any active endpoints
I0426 21:51:57.908951       9 controller.go:170] backend reload required
I0426 21:51:58.042932       9 controller.go:179] ingress backend successfully reloaded...
167.220.24.46 - [167.220.24.46] - - [26/Apr/2018:21:53:20 +0000] "GET / HTTP/1.1" 200 234 "" "Mozilla/5.0 (compatible; MSIE 9.0; Windows NT 6.1; Trident/5.0)" 197 0.001 [default-party-clippy-80] 10.244.0.13:8080 234 0.004 200
```

## <a name="clean-up"></a>Очистка

Удалите связанные объекты Kubernetes, созданные в этой статье.

```bash
$ kubectl delete -f samples-http-application-routing.yaml

deployment "party-clippy" deleted
service "party-clippy" deleted
ingress "party-clippy" deleted
```

## <a name="next-steps"></a>Дальнейшие действия

Сведения об установке контроллера защищенного входящего HTTPS-трафика в AKS см. в статье [Входящий трафик HTTPS в Службе Azure Kubernetes (AKS)][ingress-https].

<!-- LINKS - internal -->
[az-aks-create]: /cli/azure/aks?view=azure-cli-latest#az-aks-create
[az-aks-show]: /cli/azure/aks?view=azure-cli-latest#az-aks-show
[ingress-https]: ./ingress-tls.md
[az-aks-enable-addons]: /cli/azure/aks#az-aks-enable-addons


<!-- LINKS - external -->
[dns-pricing]: https://azure.microsoft.com/pricing/details/dns/
[external-dns]: https://github.com/kubernetes-incubator/external-dns
[kubectl-apply]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#apply
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[kubectl-delete]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#delete
[kubectl-logs]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#logs
[ingress]: https://kubernetes.io/docs/concepts/services-networking/ingress/
[ingress-resource]: https://kubernetes.io/docs/concepts/services-networking/ingress/#the-ingress-resource
