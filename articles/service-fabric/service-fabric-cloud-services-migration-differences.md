---
title: Различия между облачными службами и Service Fabric | Документация Майкрософт
description: Общие сведения о переносе приложений из облачных служб в Service Fabric.
services: service-fabric
documentationcenter: .net
author: vturecek
manager: chackdan
editor: ''
ms.assetid: 0b87b1d3-88ad-4658-a465-9f05a3376dee
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/02/2017
ms.author: vturecek
ms.openlocfilehash: 8b486e617389e1611dfebf3d347d2d64df088593
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "66258651"
---
# <a name="learn-about-the-differences-between-cloud-services-and-service-fabric-before-migrating-applications"></a>Узнайте о различиях между облачными службами и Service Fabric перед переносом приложений.
Microsoft Azure Service Fabric — это платформа облачных приложений следующего поколения для построения масштабируемых и высоконадежных распределенных приложений. Она содержит множество новых функций для упаковки, развертывания, обновления распределенных облачных приложений и управления ими. 

Эта статья представляет собой начальное руководство по переносу приложений из облачных служб в Service Fabric. Она посвящена преимущественно архитектурным и структурным различиям между облачными службами и Service Fabric.

## <a name="applications-and-infrastructure"></a>Приложения и инфраструктура
Фундаментальное различие между облачными службами и Service Fabric заключается в связи между виртуальными машинами, рабочими нагрузками и приложениями. Рабочая нагрузка здесь определяется как код, который создается для выполнения определенной задачи или предоставления службы.

* **Облачные службы подразумевают развертывание приложений в качестве виртуальных машин.** Код, который вы пишете, тесно связан с экземпляром виртуальной машины, например веб-ролью или рабочей ролью. Для развертывания рабочей нагрузки в облачных службах необходимо развернуть один или несколько экземпляров виртуальных машин, на которых запущена рабочая нагрузка. Приложения и виртуальные машины не отделяются друг от друга, формального определения приложения также нет. Приложение можно рассматривать как набор экземпляров веб-ролей или рабочих ролей в рамках развертывания облачных служб или как развертывание облачных служб полностью. В этом примере приложение отображается как набор экземпляров ролей.

![Приложения и топология облачных служб][1]

* **Service Fabric подразумевает развертывание приложений на существующие виртуальные машины или на компьютеры с Windows или Linux, на которых запущена Service Fabric.** Службы, которые вы пишете, полностью отделены от базовой инфраструктуры, которая абстрагируется с помощью платформы приложений Service Fabric, поэтому приложения могут развертываться в нескольких средах. Рабочая нагрузка в Service Fabric называется "службой", и одна или несколько служб группируются в формально определенное приложение, работающее на платформе приложений Service Fabric. В один кластер Service Fabric можно развернуть несколько приложений.

![Приложения и топология Service Fabric][2]

Собственно Service Fabric представляет собой слой платформы приложений, который работает под управлением Windows или Linux, а облачные службы — систему для развертывания виртуальных машин под управлением Azure с присоединенными рабочими нагрузками.
Модель приложений Service Fabric имеет ряд преимуществ:

* Быстрое развертывание. Создание экземпляров виртуальной машины может занять много времени. В Service Fabric виртуальные машины развертываются только один раз для создания кластера, в котором размещается платформа приложений Service Fabric. С этого момента можно очень быстро разворачивать пакеты приложений в кластер.
* Размещение с высокой плотностью. В облачных службах на виртуальной машине рабочей роли размещается одна рабочая нагрузка. В Service Fabric приложения отделены от виртуальных машин, на которых они запускаются. Это означает, что можно развернуть большое количество приложений на небольшом количестве виртуальных машин, что может снизить общие затраты для крупных развертываний.
* Платформу Service Fabric можно запускать везде, где есть компьютеры Windows Server или Linux, как в Azure, так и в локальной среде. Платформа предоставляет уровень абстракции поверх базовой инфраструктуры, поэтому приложения могут выполняться в разных средах. 
* Распределенное управление приложениями. Service Fabric — это платформа, которая не только содержит распределенные приложения, но и помогает управлять их жизненным циклом, независимо от размещения виртуальных машин и жизненного цикла виртуальной машины.

## <a name="application-architecture"></a>Архитектура приложения
Архитектура приложения облачных служб обычно включает многочисленные зависимости от внешних служб, таких как служебная шина, таблицы Azure и хранилище BLOB-объектов Azure, SQL, Redis и других служб для управления состоянием и данными приложения и взаимодействия между веб-ролями и рабочими ролями в развернутой облачной службе. Пример полного приложения облачных служб может выглядеть следующим образом:  

![Архитектура облачных служб][9]

Приложения Service Fabric также могут использовать те же внешние службы в готовом приложении. С использованием этого примера архитектуры облачных служб простейший способ переноса из облачных служб в Service Fabric — это замена развернутого приложения облачных служб приложением Service Fabric с сохранением общей архитектуры прежней. Веб-роли и рабочие роли можно перенести в службы без отслеживания состояния Service Fabric с минимальными изменениями в коде.

![Архитектура Service Fabric после простого переноса][10]

На этом этапе система должна продолжить работу так же, как и раньше. Используя преимущества возможностей Service Fabric с отслеживанием состояния, можно преобразовать внешние хранилища состояния во внутренние службы с отслеживанием состояния там, где это возможно. Это сложнее, чем простой перенос веб-ролей и рабочих ролей в службы без отслеживания состояния Service Fabric, поскольку требует написания пользовательских служб, которые предоставляют вашему приложению функции, эквивалентные прежним функциям внешних служб. Эти преимущества включают: 

* удаление внешних зависимостей; 
* унификация управления развертыванием, а также моделей обновления. 

Пример архитектуры после преобразования этих служб во внутренние может выглядеть следующим образом:

![Архитектура Service Fabric после полного переноса][11]

## <a name="communication-and-workflow"></a>Взаимодействие и рабочий процесс
Большинство приложений облачной службы состоят из более чем одного уровня. По аналогии приложения Service Fabric, как правило, состоят из нескольких служб. Две распространенные модели взаимодействия — это прямое взаимодействие и взаимодействие через внешнее долговременное хранилище.

### <a name="direct-communication"></a>Прямое взаимодействие
С прямым взаимодействием различные уровни могут взаимодействовать напрямую через конечные точки, предоставляемые каждым уровнем. В среде без отслеживания состояния, например в облачных службах, это означает выбор экземпляра роли виртуальной машины (случайный или циклический с целью балансировки нагрузки) и прямое подключение к конечной точке.

![Прямое взаимодействие для облачных служб][5]

 Прямое взаимодействие — это основная модель взаимодействия в Service Fabric. Основное различие между Service Fabric и облачными службами состоит в том, что в облачных службах вы подключаетесь к виртуальной машине, а в Service Fabric — к службе. Это различие важно по нескольким причинам:

* Службы в Service Fabric не привязаны к виртуальным машинам, на которых размещены их; службы могут перемещаться в кластере и на самом деле ожидается для перемещения по различным причинам: Балансировки ресурсов, отработка отказа, обновления приложений и инфраструктуры и ограничения размещения или нагрузки. Это означает, что адрес экземпляра службы может измениться в любое время. 
* В Service Fabric на виртуальной машине может размещаться несколько служб, каждая с уникальными конечными точками.

Service Fabric предоставляет механизм обнаружения служб, который называется службой именования. Она может использоваться для разрешения адресов конечных точек служб. 

![Прямое взаимодействие для Service Fabric][6]

### <a name="queues"></a>Очереди
Общий механизм взаимодействия между уровнями в средах без отслеживания состояния, таких как облачные службы — использование внешнего хранилища очередей для постоянного хранения рабочих задач из одного уровня на другой. Распространенным сценарием является веб-уровень, который отправляет задания в очередь Azure или служебную шину, из которой экземпляры рабочих ролей могут извлекать задания и обрабатывать их.

![Взаимодействие через очередь для облачных служб][7]

Ту же модель взаимодействия можно использовать в Service Fabric. Это может быть удобно при переносе существующего приложения облачных служб в Service Fabric. 

![Прямое взаимодействие для Service Fabric][8]

## <a name="parity"></a>Четность
[Облачные службы аналогична Service Fabric в степени контроля и простота в использовании, она теперь устарели и для разработки новых приложений рекомендуется использовать Service Fabric](https://docs.microsoft.com/azure/app-service/overview-compare); ниже приведен сравнением API:


| **Облачная служба API** | **Service Fabric API** | **Примечания** |
| --- | --- | --- |
| RoleInstance.GetID | FabricRuntime.GetNodeContext.NodeId или. NodeName | Идентификатор — это свойство имя_узла |
| RoleInstance.GetFaultDomain | FabricClient.QueryManager.GetNodeList | Отфильтруйте NodeName и FD-свойство |
| RoleInstance.GetUpgradeDomain | FabricClient.QueryManager.GetNodeList | Отфильтруйте NodeName и свойство обновления |
| RoleInstance.GetInstanceEndpoints | FabricRuntime.GetActivationContext или именования (ResolveService) | CodePackageActivationContext, который предоставляется как по FabricRuntime.GetActivationContext, так и в рамках реплики с помощью ServiceInitializationParameters.CodePackageActivationContext, указанные во время. Инициализация |
| RoleEnvironment.GetRoles | FabricClient.QueryManager.GetNodeList | Если вы хотите сделать же сортировки, фильтрации по типу, который можно получить список типов узлов из кластера манифеста с помощью FabricClient.ClusterManager.GetClusterManifest и взять роли или node типы оттуда. |
| RoleEnvironment.GetIsAvailable | Подключение WindowsFabricCluster или создайте FabricRuntime, на который указывает на конкретный узел | * |
| RoleEnvironment.GetLocalResource | CodePackageActivationContext.Log/Temp/Work | * |
| RoleEnvironment.GetCurrentRoleInstance | CodePackageActivationContext.Log/Temp/Work | * |
| LocalResource.GetRootPath | CodePackageActivationContext.Log/Temp/Work | * |
| Role.GetInstances | FabricClient.QueryManager.GetNodeList или ResolveService | * |
| RoleInstanceEndpoint.GetIPEndpoint | FabricRuntime.GetActivationContext или именования (ResolveService) | * |

## <a name="next-steps"></a>Следующие шаги
Простейший способ переноса из облачных служб в Service Fabric — это замена развернутого приложения облачных служб приложением Service Fabric с сохранением общей архитектуры приложения прежней. В следующей статье предоставлено руководство по преобразованию веб-роли или рабочей роли в службу без отслеживания состояния Service Fabric.

* [Простая миграция: преобразование веб-роли или рабочей роли в службу без отслеживания состояния Service Fabric](service-fabric-cloud-services-migration-worker-role-stateless-service.md)

<!--Image references-->
[1]: ./media/service-fabric-cloud-services-migration-differences/topology-cloud-services.png
[2]: ./media/service-fabric-cloud-services-migration-differences/topology-service-fabric.png
[5]: ./media/service-fabric-cloud-services-migration-differences/cloud-service-communication-direct.png
[6]: ./media/service-fabric-cloud-services-migration-differences/service-fabric-communication-direct.png
[7]: ./media/service-fabric-cloud-services-migration-differences/cloud-service-communication-queues.png
[8]: ./media/service-fabric-cloud-services-migration-differences/service-fabric-communication-queues.png
[9]: ./media/service-fabric-cloud-services-migration-differences/cloud-services-architecture.png
[10]: ./media/service-fabric-cloud-services-migration-differences/service-fabric-architecture-simple.png
[11]: ./media/service-fabric-cloud-services-migration-differences/service-fabric-architecture-full.png
