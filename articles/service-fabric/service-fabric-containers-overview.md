---
title: Общие сведения о Service Fabric и контейнерах | Документация Майкрософт
description: Обзор Service Fabric и использования контейнеров для развертывания приложений микрослужб. В этой статье содержатся общие сведения об использовании контейнеров и возможностях, предоставляемых Service Fabric.
services: service-fabric
documentationcenter: .net
author: aljo-microsoft
manager: chackdan
editor: ''
ms.assetid: c98b3fcb-c992-4dd9-b67d-2598a9bf8aab
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 8/8/2018
ms.author: aljo
ms.openlocfilehash: 5a45f14e5ac1da5152f320bd92b1ebb42be1d214
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "60881424"
---
# <a name="service-fabric-and-containers"></a>Service Fabric и контейнеры

## <a name="introduction"></a>Общие сведения

Azure Service Fabric — это платформа распределенных систем, которая дает возможность не только легко упаковывать и развертывать масштабируемые надежные микрослужбы и контейнеры, но и управлять ими.

Service Fabric — это [оркестратор контейнеров](service-fabric-cluster-resource-manager-introduction.md) корпорации Майкрософт, который развертывает микрослужбы в кластере виртуальных машин. Service Fabric развивается с учетом опыта многолетнего выполнения служб корпорации Майкрософт в крупном масштабе.

Микрослужбы можно разрабатывать различными способами — от использования [моделей программирования Service Fabric](service-fabric-choose-framework.md) и [ASP.NET Core](service-fabric-reliable-services-communication-aspnetcore.md) до развертывания [любого кода на ваш выбор](service-fabric-guest-executables-introduction.md). Если же вам нужно просто [развертывать контейнеры и управлять ими](service-fabric-containers-overview.md), Service Fabric идеально подойдет и для этого.

По умолчанию Service Fabric развертывает и активирует эти службы как процессы. Процессы обеспечивают самую быструю активацию и максимальную плотность использования ресурсов в кластере. Service Fabric также позволяет развертывать службы в образах контейнеров. Эти два подхода можно объединить, используя в одном приложении службы с процессами и контейнерами.

Чтобы приступить к работе и попробовать контейнеры в Service Fabric, ознакомьтесь с кратким руководством, полным руководством или примером:  

[Краткое руководство Развертывание приложения-контейнера Linux в Service Fabric](service-fabric-quickstart-containers-linux.md)  
[Краткое руководство Развертывание приложения-контейнера Windows в Service Fabric](service-fabric-quickstart-containers.md)  
[Руководство по развертыванию приложения .NET в контейнере Windows в Azure Service Fabric](service-fabric-host-app-in-a-container.md)  
[Примеры контейнеров Service Fabric](https://azure.microsoft.com/resources/samples/service-fabric-containers/)  

## <a name="what-are-containers"></a>Сведения о контейнерах

Контейнеры надежно решают проблему выполнения приложений в различных вычислительных средах путем предоставления неизменяемой среды для работы приложения. Контейнеры помещают приложение и все его зависимости, такие как библиотеки и файлы конфигурации, в собственную изолированную оболочку, которая содержит все необходимое для выполнения программного обеспечения в контейнере. Где бы не работал контейнер, приложение внутри всегда имеет все необходимое для выполнения, например правильные версии зависимых библиотек, какие-либо файлы конфигурации и все остальное.

Они запускаются непосредственно поверх ядра и предоставляют изолированное представление файловой системы и других ресурсов. Приложение в контейнере не имеет сведений о любых других приложениях или процессах за пределами контейнера. Каждое приложение и его среда выполнения, зависимости и системные библиотеки выполняются внутри контейнера, который предоставляет полный закрытый доступ к собственному изолированному представлению операционной системы. Контейнеры должны не только предоставлять все зависимости приложения, но и выполняться в различных вычислительных средах. Безопасность и изоляция ресурсов являются важными преимуществами использования контейнеров с Service Fabric, которые в противном случае выполняют службы в процессе.

По сравнению с виртуальными машинами использование контейнеров связано со следующими преимуществами.

* **Малый**. Контейнеры используют единое дисковое пространство, версию уровней и обновления для повышения эффективности.
* **Быстро**: Контейнерам не нужно загружать всю операционную систему, чтобы они могли начать гораздо быстрее — обычно в секундах.
* **Переносимость**: Образ контейнерного приложения можно перенести и запустить в облаке, на локальном компьютере, на виртуальных машинах или непосредственно на физических компьютерах.
* **Управление ресурсами**: Контейнер можно ограничить физические ресурсы, его можно использовать на узле.

### <a name="container-types-and-supported-environments"></a>Типы контейнеров и поддерживаемые среды

Service Fabric поддерживает контейнеры как в Windows, так и в Linux. В Windows он поддерживает режим изоляции Hyper-V.

#### <a name="docker-containers-on-linux"></a>Контейнеры Docker в Linux

Docker предоставляет API-интерфейсы для создания контейнеров и управления ими поверх контейнеров из ядер Linux. Docker Hub — это центральный репозиторий для хранения и извлечения образов контейнеров.
Руководство для Linux см. в статье [Создание первого приложения-контейнера Service Fabric в Linux](service-fabric-get-started-containers-linux.md).

#### <a name="windows-server-containers"></a>Контейнеры Windows Server

Windows Server 2016 поддерживает два типа контейнеров, которые отличаются уровнем изоляции. Как и в контейнерах Docker, в контейнерах Windows Server предусмотрена изоляция пространства имен и файловой системы. Эти контейнеры совместно используют ядро и узел, на котором выполняются. В Linux эта изоляция обычно обеспечивается с помощью cgroups и пространств имен. У контейнеров Windows Server все аналогично.

Контейнеры Windows с поддержкой Hyper-V обеспечивают более высокую степень изоляции и безопасности, так как контейнер не использует ядро операционной системы совместно с любым другим контейнером или узлом. Благодаря высокому уровню изоляции и безопасности контейнеры с поддержкой Hyper-V предназначены для потенциально неблагоприятных мультитенантных сред.
Руководство для Windows см. в статье [Создание первого контейнера-приложения Service Fabric в Windows](service-fabric-get-started-containers.md).

На рисунке ниже показаны различные доступные типы уровней изоляции и виртуализации.
![Платформа Service Fabric][Image1]

## <a name="scenarios-for-using-containers"></a>Сценарии использования контейнеров

Ниже представлены распространенные примеры использования контейнеров.

* **IIS lift- and -shift**: Вы можете поместить существующий [ASP.NET MVC](https://www.asp.net/mvc) приложения в контейнер вместо перемещения его в ASP.NET Core. Эти приложения ASP.NET MVC зависят от служб Internet Information Services (IIS). Приложения можно упаковать в образ контейнера из предварительно созданного образа IIS и развернуть с помощью Service Fabric. Дополнительные сведения о контейнерах Windows см. в статье [Образы контейнеров в Windows Server](https://docs.microsoft.com/virtualization/windowscontainers/quick-start/quick-start-windows-server).

* **Совмещение контейнеров и микрослужб Service Fabric**: Используйте имеющийся образ контейнера для части приложения. Например, можно использовать [контейнер NGINX](https://hub.docker.com/_/nginx/) для веб-интерфейса приложения и службы с отслеживанием состояния для ресурсоемких вычислений серверной части.

* **Сокращение влияния «шумных соседей»** : Возможность управления ресурсами контейнеров можно использовать для ограничения ресурсов, используемых службой на узле. Службы, которые используют много ресурсов и таким образом влияют на производительность других служб (например, выполняют длительные запросы), рекомендуется поместить в контейнеры с возможностью управления ресурсами.

## <a name="service-fabric-support-for-containers"></a>Поддержка Service Fabric для контейнеров

Service Fabric поддерживает развертывание контейнеров Docker в Linux и контейнеров Windows Server в Windows Server 2016, а также режим изоляции Hyper-V. 

Service Fabric предоставляет [модель приложения](service-fabric-application-model.md), в которой контейнер представляет собой узел приложения, где размещаются несколько реплик службы. Service Fabric также поддерживает [гостевой исполняемый сценарий](service-fabric-guest-executables-introduction.md), в котором не используются встроенные модели программирования Service Fabric. Вместо этого имеющееся приложение, написанное с использованием любого языка или платформы, помещается в контейнер. Это распространенный вариант использования для контейнеров.

Вы также можете выполнять [службы Service Fabric в контейнере](service-fabric-services-inside-containers.md). Поддержка служб Service Fabric, выполняемых в контейнере, в настоящее время ограничена.

Служба Service Fabric предоставляет несколько связанных с контейнерами возможностей, позволяющих создавать приложения, которые состоят из контейнерных микрослужб, в частности:

* развертывание и активация образа контейнера;
* управление ресурсами, в том числе настройка значений ресурсов по умолчанию для кластеров Azure;
* проверка подлинности репозитория;
* сопоставление порта контейнера с портом узла;
* межконтейнерное обнаружение и взаимодействие;
* возможность настройки и установки переменных среды.
* возможность настроить учетные данные безопасности для контейнера;
* выбор различных сетевых режимов для контейнеров.

Подробный обзор поддержки контейнера в Azure, например, сведения о создании кластера Kubernetes со Службой Azure Kubernetes, создании частного реестра Docker в службе "Реестр контейнеров Azure" и многом другом, см. в статье [Azure для контейнеров](https://docs.microsoft.com/azure/containers/).

## <a name="next-steps"></a>Дальнейшие действия

В этой статье вы узнали о поддержке выполнения контейнеров в Service Fabric. Теперь можно переходить к изучению примеров использования каждой функции.

[Создание первого приложения-контейнера Service Fabric в Linux](service-fabric-get-started-containers-linux.md)  
[Создание первого приложения-контейнера Service Fabric в Windows](service-fabric-get-started-containers.md)  
[Дополнительные сведения о контейнерах Windows](https://docs.microsoft.com/virtualization/windowscontainers/about/)

[Image1]: media/service-fabric-containers/Service-Fabric-Types-of-Isolation.png