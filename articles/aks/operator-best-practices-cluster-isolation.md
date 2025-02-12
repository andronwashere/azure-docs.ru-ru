---
title: Рекомендации для операторов. Изоляция кластера в службах Azure Kubernetes (AKS)
description: Самые актуальные рекомендации для оператора кластера по изоляции в службе Azure Kubernetes (AKS)
services: container-service
author: mlearned
ms.service: container-service
ms.topic: conceptual
ms.date: 11/26/2018
ms.author: mlearned
ms.openlocfilehash: 8150e184f0c7533d5a6e7e4847bf126206f5e6c6
ms.sourcegitcommit: 6a42dd4b746f3e6de69f7ad0107cc7ad654e39ae
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/07/2019
ms.locfileid: "67614927"
---
# <a name="best-practices-for-cluster-isolation-in-azure-kubernetes-service-aks"></a>Рекомендации по изоляции кластера в службах Azure Kubernetes (AKS)

При управлении кластерами в Cлужбе Azure Kubernetes (AKS) часто возникает необходимость изолировать команды и рабочие нагрузки. AKS позволяет гибко настраивать конфигурацию запуска кластеров с несколькими клиентами и изолировать ресурсы. Чтобы добиться максимальной эффективности Kubernetes, следует изучить и применить эти возможности мультитенантности и изоляции.

Это статья содержит рекомендации по изоляции для операторов кластера. В этой статье раскрываются следующие темы:

> [!div class="checklist"]
> * планирование кластеров с несколькими клиентами и разделение ресурсов;
> * использование логической или физической изоляции в кластерах AKS;

## <a name="design-clusters-for-multi-tenancy"></a>проектирование кластеров с учетом мультитенантности.

Kubernetes предоставляет возможности логической изоляции команд и рабочих нагрузок в одном кластере. Задача — предоставить минимально необходимый набор привилегий для использования ресурсов, которые нужны каждой команде. Объект [пространства имен][k8s-namespaces] в Kubernetes создает периметр логической изоляции. Следует учитывать следующие функции Kubernetes и дополнительные рекомендации по обеспечению изоляции и мультитенантности.

* **Планирование** ограничивает использование основных возможностей. Сюда относятся квоты ресурсов и затраты, связанные с неработоспособностью групп pod. Дополнительные сведения об этих функциях см. в разделе [советы и рекомендации для возможности основные планировщика в AKS][aks-best-practices-scheduler].
  * Планировщик поддерживает и более сложные функции, например использование отметок и толерантностей, выбор узлов, а также добавление и удаление сходства узлов и групп pod. Дополнительные сведения об этих функциях см. в разделе [советы и рекомендации для возможности расширенной планировщика в AKS][aks-best-practices-advanced-scheduler].
* **Сеть** позволяет использовать политики сети для управления потоком входящего и исходящего трафика для групп pod.
* **Проверка подлинности и авторизация** включает пользователя управления доступом на основе ролей (RBAC) и интеграции Azure Active Directory (AD), удостоверения групп pod и секреты в Azure Key Vault. Дополнительные сведения об этих функциях см. в разделе [советы и рекомендации для проверки подлинности и авторизации в AKS][aks-best-practices-identity].
* **Контейнеры** включают политики безопасности и контексты безопасности групп pod, сканирование образов и сред выполнения для поиска уязвимостей. Также они включают использование App Armor или Seccomp (Secure Computing) для ограничения доступа контейнера к базовому узлу.

## <a name="logically-isolate-clusters"></a>Логическая изоляция кластеров

**Советы и рекомендации** используйте логическую изоляцию для разделения команд и проектов. Старайтесь сократить число физических кластеров AKS, которые вы развертываете для изоляции команд и приложений.

Логическая изоляция позволяет использовать один кластер AKS для нескольких рабочих нагрузок, групп или сред. Kubernetes [пространства имен][k8s-namespaces] образуют логическую изоляцию границу для рабочих нагрузок и ресурсов.

![Логическая изоляция кластера Kubernetes в AKS](media/operator-best-practices-cluster-isolation/logical-isolation.png)

Логическое разделение кластеров обычно обеспечивает более высокую плотность групп pod, чем физически изолированные кластеры. В этом варианте меньший объем вычислительной мощности простаивает в кластере. Используя средство автомасштабирования кластера Kubernetes, вы можете масштабировать число узлов в соответствии с текущими требованиями. Этот подход к автоматическому масштабированию считается оптимальным, так как он позволяет выполнять только строго необходимое число узлов и минимизировать затраты.

Среды Kubernetes в AKS или в другой службе сейчас не полностью безопасны для использования в неблагоприятных мультитенантных средах. Дополнительные функциональные возможности безопасности, такие как *политики безопасности pod* или более точные элементы управления доступом на основе ролей (RBAC) для узлов, усложняют эксплойты. Однако для обеспечения полноценной безопасности при выполнении неблагоприятных мультитенантных рабочих нагрузок гипервизор является единственным уровнем безопасности, которому следует доверять. Домен безопасности для Kubernetes становится целым кластером, а не отдельным узлом. Для таких типов неблагоприятных мультитенантных рабочих нагрузок следует использовать физически изолированные кластеры.

## <a name="physically-isolate-clusters"></a>Физическая изоляция кластеров

**Советы и рекомендации** — старайтесь как можно меньше использовать физическую изоляцию для отдельных групп или развертываний приложений. Всегда используйте *логическую* изоляцию, которая описана в предыдущем разделе.

Изоляция кластеров достаточно часто выполняется путем физического разделения кластеров AKS. При такой модели изоляции каждая команда или рабочая нагрузка получает собственный кластер AKS. Этот подход к изоляции рабочих нагрузок или команд кажется на первый взгляд самым простым, но он сопряжен со значительными административными и финансовыми издержками. Вам придется поддерживать несколько раздельных кластеров, а также отдельно предоставлять для них доступ и назначать разрешения. Кроме того, вы будете оплачивать все отдельные узлы.

![Физическая изоляция отдельных кластеров Kubernetes в AKS](media/operator-best-practices-cluster-isolation/physical-isolation.png)

Физически разделенные кластеры обычно имеют низкую плотность групп pod. Так как каждой команде или рабочей нагрузке предоставляется собственный кластер AKS, все эти кластеры обычно получают избыточные вычислительные ресурсы. В результате между такими узлами часто распределяется малое число групп pod. Неиспользуемое пространство на узлах недоступно для приложений или служб, над которыми работают другие команды. Лишние ресурсы дополнительно повышают затраты на систему с физически разделенными кластерами.

## <a name="next-steps"></a>Следующие шаги

Эта статья посвящена изоляции кластера. Дополнительную информацию об операциях кластера в AKS см. в рекомендациях на такие темы:

* [Основные возможности планировщик Kubernetes][aks-best-practices-scheduler]
* [Расширенные возможности планировщик Kubernetes][aks-best-practices-advanced-scheduler]
* [Аутентификация и авторизация][aks-best-practices-identity]

<!-- EXTERNAL LINKS -->

<!-- INTERNAL LINKS -->
[k8s-namespaces]: concepts-clusters-workloads.md#namespaces
[aks-best-practices-scheduler]: operator-best-practices-scheduler.md
[aks-best-practices-advanced-scheduler]: operator-best-practices-advanced-scheduler.md
[aks-best-practices-identity]: operator-best-practices-identity.md
