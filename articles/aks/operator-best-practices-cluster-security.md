---
title: Рекомендации для операторов. Безопасность кластера в службах Azure Kubernetes (AKS)
description: Ознакомьтесь с рекомендациями по управлению безопасностью и обновлениями для кластеров в службе Azure Kubernetes (AKS) для операторов кластеров.
services: container-service
author: mlearned
ms.service: container-service
ms.topic: conceptual
ms.date: 12/06/2018
ms.author: mlearned
ms.openlocfilehash: d4a77fc1756b0fa9decb6d3a84760beb1e700863
ms.sourcegitcommit: 6a42dd4b746f3e6de69f7ad0107cc7ad654e39ae
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/07/2019
ms.locfileid: "67614894"
---
# <a name="best-practices-for-cluster-security-and-upgrades-in-azure-kubernetes-service-aks"></a>Рекомендации по безопасности и обновлениям кластера в службах Azure Kubernetes (AKS)

При управлении кластерами в Azure Kubernetes Service (AKS) безопасность ваших рабочих нагрузок и данных является ключевым фактором. Необходимо обеспечить безопасный доступ к ресурсам и рабочим нагрузкам, особенно при запуске мультитенантных кластеров с использованием логической изоляции. Чтобы свести к минимуму риск атаки, вы также должны убедиться, что применяете последние обновления безопасности Kubernetes и ОС узла.

Эта статья посвящается вопросам, связанным с безопасностью кластера AKS. Вы узнаете, как выполнять следующие задачи:

> [!div class="checklist"]
> * использовать Azure Active Directory и средства управления доступом на основе ролей для безопасного доступа к серверу API;
> * обезопасить доступ контейнера к ресурсам узла;
> * обновить кластер AKS до последней версии Kubernetes;
> * обновить узлы и автоматически применить исправления безопасности.

Можно также прочитать рекомендации по [управлении образами контейнеров][best-practices-container-image-management] and for [pod security][best-practices-pod-security].

## <a name="secure-access-to-the-api-server-and-cluster-nodes"></a>Безопасный доступ к серверу API и узлам кластера

**Рекомендации**. Обеспечение безопасного доступа к серверу API Kubernetes является одним из самых важных факторов для защиты кластера. Интегрируйте управление доступом на основе ролей Kubernetes (RBAC) с Azure Active Directory для управления доступом к серверу API. Эти элементы управления позволяют защитить AKS так же, как и доступ к подпискам Azure.

Сервер API Kubernetes предоставляет единую точку подключения для запросов на выполнение действий в кластере. Для защиты и аудита доступа к серверу API ограничьте доступ и предоставьте право доступа с наименьшими привилегиями. Этот подход не уникален для Kubernetes, но особенно важен, когда кластер AKS логически изолирован для использования несколькими клиентами.

Azure Active Directory (AD) предоставляет решение для управления удостоверениями корпоративного уровня и интегрируется с кластерами AKS. Так как Kubernetes не предоставляет решение для управления удостоверениями, обеспечить детализированный способ ограничения доступа к серверу API может быть трудно. Благодаря кластерам с интегрированным Azure AD в AKS вы можете использовать существующие учетные записи пользователей и групп для проверки подлинности пользователей на сервере API.

![Интеграция Azure Active Directory для кластеров AKS](media/operator-best-practices-cluster-security/aad-integration.png)

Используйте интеграцию с Kubernetes RBAC и Azure AD для защиты сервера API и предоставляйте наименьшее количество разрешений, требуемых для набора ресурсов с заданной областью, например единое пространство имен. Различным пользователям или группам пользователей в Azure AD могут предоставляться разные роли RBAC. Эти детализированные разрешения позволяют ограничивать доступ к серверу API и предоставляют четкий журнал аудита выполненных действий.

Для предоставления доступа к файлам и папкам рекомендуется использовать группы, а не отдельные идентификаторы, для привязки пользователей к ролям RBAC рекомендуется использовать членство в *группах* Azure AD, а не отдельных *пользователей*. При изменении членства в группах пользователей их права доступа в кластере AKS будут меняться соответственно. Если вы связываете пользователя непосредственно с ролью, его функциональные обязанности могут измениться. Членство в группах Azure AD будет обновлено, но разрешения для кластера AKS не будут отражать это. В таком случае пользователю предоставляется больше разрешений, чем ему требуется.

Дополнительные сведения об интеграции Azure AD и RBAC см. в разделе [советы и рекомендации для проверки подлинности и авторизации в AKS][aks-best-practices-identity].

## <a name="secure-container-access-to-resources"></a>Обеспечение безопасного доступа контейнера к ресурсам

**Рекомендации**. Ограничьте доступ к действиям, которые могут выполнять контейнеры. Укажите наименьшее количество разрешений и избегайте использования корневой или привилегированной эскалации.

Подобно предоставлению пользователям или группам наименьшего количества требуемых привилегий, контейнеры должны быть ограничены только необходимыми действиями и процессами. Чтобы свести к минимуму риск атак, не настраивайте приложения и контейнеры, которые требуют повышенных привилегий или корневого доступа. Например, задайте `allowPrivilegeEscalation: false` в манифесте pod. Эти *контексты безопасности pod* встроены в Kubernetes и позволяют определить дополнительные разрешения, например, какого пользователя или группу запускать или какие возможности Linux предоставлять. Дополнительные советы и рекомендации, см. в разделе [pod безопасный доступ к ресурсам][pod-security-contexts].

Для более детального управления действиями контейнера можно также использовать встроенные функции безопасности Linux, такие как *AppArmor* и *seccomp*. Эти функции определяются на уровне узла, а затем реализуются с помощью манифеста pod. Встроенные функции безопасности Linux доступны только на узлах Linux и модулей POD.

> [!NOTE]
> Среды Kubernetes в AKS или в другой службе сейчас не полностью безопасны для использования в неблагоприятных мультитенантных средах. Дополнительные функциональные возможности безопасности, такие как *AppArmor*, *seccomp*, *политики безопасности pod* или более точные элементы управления доступом на основе ролей (RBAC) для узлов усложняют эксплойты. Однако для обеспечения полноценной безопасности при выполнении неблагоприятных мультитенантных рабочих нагрузок гипервизор является единственным уровнем безопасности, которому следует доверять. Домен безопасности для Kubernetes становится целым кластером, а не отдельным узлом. Для таких типов неблагоприятных мультитенантных рабочих нагрузок следует использовать физически изолированные кластеры.

### <a name="app-armor"></a>AppArmor

Чтобы ограничить действия, которые могут выполнять контейнеры, можно использовать [AppArmor][k8s-apparmor] модуль безопасности ядра Linux. AppArmor является частью базовой операционной системы узла AKS и включен по умолчанию. Вы создаете профили AppArmor, которые ограничивают действия, такие как чтение, запись или выполнение, или системные функции, такие как подключение файловых систем. Профили AppArmor по умолчанию ограничивают доступ к различным расположениям `/proc` и `/sys` и предоставляют средства для логической изоляции контейнеров от базового узла. AppArmor работает с любыми приложениями под управлением Linux, а не только с группами pod Kubernetes.

![Профили AppArmor, использующиеся в кластере AKS для ограничения действий контейнера](media/operator-best-practices-container-security/apparmor.png)

Чтобы увидеть AppArmor в действии, в следующем примере создается профиль, который запрещает запись в файлы. [SSH][aks-ssh] к узлу AKS, создайте файл с именем *запретить write.profile* и вставьте следующее содержимое:

```
#include <tunables/global>
profile k8s-apparmor-example-deny-write flags=(attach_disconnected) {
  #include <abstractions/base>
  
  file,
  # Deny all file writes.
  deny /** w,
}
```

Профили AppArmor добавляются с помощью команды `apparmor_parser`. Добавьте профиль в AppArmor и укажите имя для профиля, созданного на предыдущем шаге.

```console
sudo apparmor_parser deny-write.profile
```

Если профиль правильно анализируется и применяется к AppArmor, выходные данные не возвращаются. Вы вернулись в командную строку.

Теперь на локальном компьютере необходимо создать манифест pod с именем *aks-apparmor.yaml* и вставить следующее содержимое. Этот манифест определяет заметку для `container.apparmor.security.beta.kubernetes` и ссылается на профиль *deny-write*, созданный на предыдущих шагах.

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: hello-apparmor
  annotations:
    container.apparmor.security.beta.kubernetes.io/hello: localhost/k8s-apparmor-example-deny-write
spec:
  containers:
  - name: hello
    image: busybox
    command: [ "sh", "-c", "echo 'Hello AppArmor!' && sleep 1h" ]
```

Развертывание примера pod с помощью [kubectl применить][kubectl-apply] команды:

```console
kubectl apply -f aks-apparmor.yaml
```

Модуль развертывания, использовать [kubectl exec][kubectl-exec] команду, чтобы записать в файл. В следующем примере выходных данных показано, что команда невыполнима.

```
$ kubectl exec hello-apparmor touch /tmp/test

touch: /tmp/test: Permission denied
command terminated with exit code 1
```

Дополнительные сведения о AppArmor, см. в разделе [AppArmor профилей в Kubernetes][k8s-apparmor].

### <a name="secure-computing"></a>Безопасные вычисления

Хотя AppArmor работает для любого приложения Linux [seccomp (*сек*рисунок *comp*экономическая)][seccomp] работает на уровне процесса. Seccomp представляет собой модуль безопасности ядра Linux, а также изначально поддерживается средой выполнения Docker, используемой узлами AKS. С помощью seccomp ограничиваются вызовы процессов, которые могут выполнять контейнеры. Можно создать фильтры, которые определяют, какие действия разрешить или запретить, а потом использовать заметки в манифесте YAML pod для сопоставления с фильтром seccomp. Это соответствует совету о предоставлении контейнеру только минимальных разрешений, необходимых для запуска, и не более.

Чтобы увидеть seccomp в действии, создайте фильтр, который предотвращает изменение разрешений для файла. [SSH][aks-ssh] к узлу AKS, создайте фильтр с именем seccomp */var/lib/kubelet/seccomp/prevent-chmod* и вставьте следующее содержимое:

```
{
  "defaultAction": "SCMP_ACT_ALLOW",
  "syscalls": [
    {
      "name": "chmod",
      "action": "SCMP_ACT_ERRNO"
    }
  ]
}
```

Теперь на локальном компьютере необходимо создать манифест pod с именем *aks-seccomp.yaml* и вставить следующее содержимое. Этот манифест определяет заметку для `seccomp.security.alpha.kubernetes.io` и ссылается на фильтр *prevent-chmod*, созданный на предыдущем шаге.

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: chmod-prevented
  annotations:
    seccomp.security.alpha.kubernetes.io/pod: localhost/prevent-chmod
spec:
  containers:
  - name: chmod
    image: busybox
    command:
      - "chmod"
    args:
     - "777"
     - /etc/hostname
  restartPolicy: Never
```

Развертывание примера pod с помощью [kubectl применить][kubectl-apply] команды:

```console
kubectl apply -f ./aks-seccomp.yaml
```

Просмотр состояния модулей POD с помощью [kubectl get pods][kubectl-get] команды. Группа pod сообщает об ошибке. Фильтр seccomp не дает команде `chmod` запускаться, как показано в следующем примере выходных данных.

```
$ kubectl get pods

NAME                      READY     STATUS    RESTARTS   AGE
chmod-prevented           0/1       Error     0          7s
```

Дополнительные сведения о доступных фильтрах см. в разделе [Seccomp безопасности профилями для Docker][seccomp].

## <a name="regularly-update-to-the-latest-version-of-kubernetes"></a>Регулярное обновление до последней версии Kubernetes

**Рекомендации**. Оставайтесь в курсе новых возможностей и исправлений ошибок, регулярно обновляя до версии Kubernetes в кластере AKS.

Kubernetes выпускает новые функции быстрее, чем традиционные платформы инфраструктуры. Обновления Kubernetes включают в себя новые функции и исправления ошибок или безопасности. Новые функции обычно переходят из состояния *альфа-версии* в *бета-версию*, а затем становятся *стабильными*, общедоступными и рекомендованными для использования в рабочей среде. Этот цикл выпуска разрешит обновлять Kubernetes без регулярного возникновения критических изменений или настроек развертывания и шаблонов.

AKS поддерживает четыре дополнительные версии Kubernetes. Это означает, что при введении новой исправленной версии самые старые версии и исправления больше не поддерживаются. Незначительные изменения в Kubernetes происходят периодически. Убедитесь, что у вас есть процесс управления для проверки и обновлений по мере необходимости, чтобы не утратить поддержку. Дополнительные сведения см. в разделе [Kubernetes, поддерживаемые версии AKS][aks-supported-versions]

Чтобы проверить версии, доступные для кластера, используйте [az aks get обновления][az-aks-get-upgrades] команды, как показано в следующем примере:

```azurecli-interactive
az aks get-upgrades --resource-group myResourceGroup --name myAKSCluster
```

Затем вы можете обновить кластер AKS с помощью [обновление az aks][az-aks-upgrade] команды. Процесс обновления безопасно блокирует и очищает узел, устанавливает расписание для объектов pod на остальных узлах, а затем разворачивает новый узел, который работает под управлением последней версии ОС и Kubernetes.

```azurecli-interactive
az aks upgrade --resource-group myResourceGroup --name myAKSCluster --kubernetes-version 1.11.8
```

Дополнительные сведения об обновлении в AKS, см. в разделе [версии поддерживается Kubernetes в AKS][aks-supported-versions] and [Upgrade an AKS cluster][aks-upgrade].

## <a name="process-linux-node-updates-and-reboots-using-kured"></a>Узел Linux процесса обновления и перезагрузки, с помощью kured

**Советы и рекомендации** - AKS автоматически загружает и устанавливает безопасности исправления на каждый узлах Linux, но не перезагружает автоматически при необходимости. Используйте `kured` для отслеживания ожидающих перезагрузок, затем безопасно заблокируйте и очистите узел, чтобы перегрузить его, примените обновления и обеспечьте максимальную безопасность с учетом ОС. Для узлов Windows Server (в настоящее время в предварительной версии в AKS) необходимо регулярно выполнять операцию обновления AKS для безопасного cordon и утечки модулей и развертывания обновленных узлов.

Создается каждый вечер узлами Linux в AKS получить обновления безопасности, доступные через канал обновления их дистрибутива. Это настраивается автоматически, так как узлы развернуты в кластере AKS. Чтобы свести к минимуму нарушения и возможные последствия для рабочих нагрузок, узлы не перезагружается автоматически, если исправление или обновление ядра этого требуют.

Открытым кодом [kured (управляющая программа перезагрузки KUbernetes)][kured] проект по Weaveworks следит за ожидающие перезагрузки узла. Если узел Linux применяется обновлений, требующих перезагрузки, узел безопасно блокируются и останавливаются для перемещения и запланировать POD, содержащихся на других узлах в кластере. После перезагрузки узел добавляется обратно в кластер и Kubernetes возобновляет планирование групп pod, содержащихся на нем. Для минимизации сбоев в работе `kured` может перезагрузить только один узел за раз.

![Процесс перезагрузки узла AKS с использованием kured](media/operator-best-practices-cluster-security/node-reboot-process.png)

Если требуется более точный контроль над временем перезагрузок, `kured` можно интегрировать с Prometheus для предотвращения перезагрузок в том случае, если в процессе обработки находятся другие события обслуживания или проблемы с кластерами. Эта интеграция сводит к минимуму дополнительные сложности, перезагружая узлы, пока вы активно решаете другие проблемы.

Дополнительные сведения об обработке перезагрузки узла см. в разделе [применения обновлений безопасности и ядра на узлы в AKS][aks-kured].

## <a name="next-steps"></a>Следующие шаги

Эта статья посвящена вопросам, связанным с безопасностью кластера AKS. Для реализации части этих рекомендаций требуются сведения, опубликованные в следующих статьях:

* [Интеграция Azure Active Directory с AKS][aks-aad]
* [Обновление кластера AKS до последней версии Kubernetes][aks-upgrade]
* [Процесс обновления для системы безопасности и узел перезагружается с kured][aks-kured]

<!-- EXTERNAL LINKS -->
[kured]: https://github.com/weaveworks/kured
[k8s-apparmor]: https://kubernetes.io/docs/tutorials/clusters/apparmor/
[seccomp]: https://kubernetes.io/docs/concepts/policy/pod-security-policy/#seccomp
[kubectl-apply]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#apply
[kubectl-exec]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#exec
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get

<!-- INTERNAL LINKS -->
[az-aks-get-upgrades]: /cli/azure/aks#az-aks-get-upgrades
[az-aks-upgrade]: /cli/azure/aks#az-aks-upgrade
[aks-supported-versions]: supported-kubernetes-versions.md
[aks-upgrade]: upgrade-cluster.md
[aks-best-practices-identity]: concepts-identity.md
[aks-kured]: node-updates-kured.md
[aks-aad]: azure-ad-integration.md
[best-practices-container-image-management]: operator-best-practices-container-image-management.md
[best-practices-pod-security]: developer-best-practices-pod-security.md
[pod-security-contexts]: developer-best-practices-pod-security.md#secure-pod-access-to-resources
[aks-ssh]: ssh.md
