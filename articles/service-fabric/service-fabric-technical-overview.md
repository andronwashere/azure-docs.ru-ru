---
title: Описание терминологии Azure Service Fabric | Документация Майкрософт
description: Общие сведения о терминологии Service Fabric. Обсуждаются ключевые понятия и термины, используемые в остальной части документации.
services: service-fabric
documentationcenter: .net
author: masnider
manager: chackdan
editor: chackdan;subramar
ms.assetid: 3a970679-e19e-43b3-9be8-71773f307c57
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 09/17/2018
ms.author: masnider
ms.openlocfilehash: 085d5e560eec090ab76c263f8f93140786f2d734
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "60543212"
---
# <a name="service-fabric-terminology-overview"></a>Общие сведения о терминологии Service Fabric
Azure Service Fabric — это платформа распределенных систем, которая дает возможность не только легко упаковывать и развертывать масштабируемые и надежные микрослужбы, но и управлять ими.  Вы можете [разместить кластеры Service Fabric где угодно](service-fabric-deploy-anywhere.md): в локальном центре обработки данных Azure или любого другого поставщика облачных служб.  Service Fabric — это оркестратор, лежащий в основе службы [Сетка Azure Service Fabric](/azure/service-fabric-mesh). Для записи служб можно использовать любую платформу. Кроме того, место для запуска приложения можно выбрать из нескольких сред. В этой статье описана терминология Service Fabric, повсеместно используемая в документации.

## <a name="infrastructure-concepts"></a>Основные понятия для инфраструктуры
**Кластер** — подключенный к сети набор виртуальных машин или физических компьютеров, в котором вы развертываете микрослужбы и управляете ими.  Кластеры можно масштабировать до нескольких тысяч машин.

**Узел** — компьютер или виртуальная машина, которая входит в состав кластера, называется *узлом*. Каждому узлу назначается имя (строка). Узлы имеют свои характеристики, в частности свойства размещения. Каждый компьютер или виртуальная машина имеет автоматически запускаемую службу Windows `FabricHost.exe`. Она запускается после загрузки системы и запускает два исполняемых файла: `Fabric.exe` и `FabricGateway.exe`. Эти два файла формируют узел. В целях тестирования на одном компьютере или виртуальной машине можно разместить несколько узлов, запустив несколько экземпляров `Fabric.exe` и `FabricGateway.exe`.

## <a name="application-and-service-concepts"></a>Концепции приложения и службы

**Приложение Сетки Service Fabric**: приложения Сетки Service Fabric описаны в модели ресурсов (файлы ресурсов YAML и JSON). Такие приложения можно развернуть в любой среде, в которой выполняется Service Fabric.

**Собственное приложение Service Fabric**: собственные приложения Service Fabric описаны в модели собственных приложений (приложения на основе XML и манифесты служб).  Собственные приложения Service Fabric не работают в Сетке Service Fabric.

### <a name="service-fabric-mesh-application-concepts"></a>Концепции приложения Сетки Service Fabric

**Приложение**: Приложение — это единица развертывания, управления версиями и времени существования приложения Сетки. Возможно независимое управление жизненным циклом каждого экземпляра приложения.  Приложения состоят из одного или нескольких пакетов кода службы и параметров. Приложение определяется с помощью схемы модели ресурсов Azure (RM).  Службы описываются как свойства ресурса приложения в шаблоне RM.  Приложение ссылается на сети и тома, используемые приложением.  При создании приложения само приложение, службы, сеть и тома моделируются с помощью модели ресурсов Service Fabric.

**Служба**: служба в приложении представляет микрослужбу и выполняет полноценную отдельную функцию. Каждая служба состоит из одного или нескольких пакетов кода, которые описывают все необходимое для запуска образа контейнера, связанного с пакетом кода.  Количество служб в приложении можно увеличивать и уменьшать.

**Сеть**. сетевой ресурс создает частную сеть для приложений и не зависит от приложений или служб, которые могут ссылаться на него. Несколько служб из разных приложений могут входить в одну сеть. Сети — это развертываемые ресурсы, на которые ссылается приложение.

**Пакет кода**: пакеты кода описывают все необходимое для запуска образа контейнера, связанного с пакетом кода, включая следующие элементы:

* Имя контейнера, версия и реестр
* Ресурсы ЦП и памяти, необходимые для каждого контейнера
* Конечные точки сети
* Тома для подключения в контейнере со ссылкой на ресурс отдельного тома.

Все пакеты кода, определенные как часть ресурса приложения, развертываются и активируются как одна группа.

**Том**. Тома — это каталоги, которые вставляются в экземпляры контейнеров и используются для сохранения состояния. Драйвер тома службы файлов Azure вставляет файловый ресурс Azure в контейнер и предоставляет надежное хранилище данных через любой интерфейс API, который поддерживает хранилище файлов. Тома — это развертываемые ресурсы, на которые ссылается приложение.

### <a name="service-fabric-native-application-concepts"></a>Концепции собственных приложений Service Fabric

**Приложение**: Приложение представляет собой коллекцию составляющих его служб, которые выполняют определенные функции. Возможно независимое управление жизненным циклом каждого экземпляра приложения.

**Служба**: Служба выполняет завершенную и отдельную функцию и может запускаться и работать независимо от других служб. Она состоит из кода, конфигурации и данных. Для каждой службы код состоит из исполняемых двоичных файлов, конфигурация состоит из параметров службы, которые могут быть загружены во время выполнения, а данные состоят из произвольных статических данных, которые должны обрабатываться рассматриваемой службой.

**Тип приложения**. имя и версия, назначенные коллекции типов служб. Он определен в файле `ApplicationManifest.xml` и внедрен в каталоге пакета приложения. Затем этот каталог копируется в хранилище образов кластера Service Fabric. Из этого типа приложения в кластере можно создать именованное приложение.

Дополнительные сведения см. в статье [Моделирование приложения в структуре службы](service-fabric-application-model.md).

**Пакет приложения** — каталог на диске, в котором хранится файл типа приложения `ApplicationManifest.xml`. В этом файле указаны ссылки на пакеты служб, типы которых определяют тип приложения. Файлы в каталоге пакета приложения копируются в хранилище образов кластера Service Fabric. Например, пакет почтового приложения может содержать ссылки на пакет службы очередей, пакет службы внешнего интерфейса и пакет службы базы данных.

**Именованное приложение.** После копирования пакета приложения в хранилище образов можно создать экземпляр приложения в пределах кластера. Создайте экземпляр при указании типа приложения пакета приложения, используя его имя или версию. Каждому экземпляру приложения этого типа назначается универсальный код ресурса (URI), который выглядит следующим образом: `"fabric:/MyNamedApp"`. Из одного типа приложения в кластере можно создать несколько именованных приложений. Именованные приложения можно также создавать из разных типов приложений. Для каждого именованного приложения управление и контроль версий выполняются отдельно.

**Тип службы**. имя и версия, назначенные пакетам кода, пакетам данных и пакетам конфигураций службы. Тип службы определен в файле `ServiceManifest.xml` и внедрен в каталог пакета службы. Затем на каталог пакета службы ссылается файл пакета приложения `ApplicationManifest.xml`. Создав именованное приложение, вы можете создать в кластере именованную службу, используя для этого один из типов служб соответствующего типа приложения. Файл типа службы, `ServiceManifest.xml`, содержит ее описание.

Дополнительные сведения см. в статье [Моделирование приложения в структуре службы](service-fabric-application-model.md).

Существует два типа служб.

* **Без отслеживания состояния.** Служба без отслеживания состояния используется, когда сведения об устойчивом состоянии службы хранятся во внешней службе хранилища, например в службе хранилища Azure, Базе данных SQL Azure или Azure Cosmos DB. Службу без отслеживания состояния следует использовать, когда у службы нет постоянного хранилища. Примером может быть служба калькулятора: в службу передаются значения, служба выполняет вычисления с использованием этих значений и возвращает результат.
* **С отслеживанием состояния.** Служба с отслеживанием состояния используется, когда нужно, чтобы платформа Service Fabric управляла состоянием службы через модели программирования Reliable Collections или Reliable Actors. При создании именованной службы укажите количество секций, на которые вы хотите распределить состояние (для масштабируемости). Укажите также, сколько раз следует реплицировать состояние между узлами (для обеспечения надежности). У каждой именованной службы есть одна первичная реплика и несколько вторичных. Изменение состояния именованной службы происходит при записи в первичную реплику. Затем Service Fabric реплицирует это состояние на все вторичные реплики, тем самым синхронизируя состояние. Когда происходит сбой первичной реплики, Service Fabric автоматически обнаруживает это и делает одну вторичную реплику первичной. После этого Service Fabric создает новую вторичную реплику.  

**Реплики или экземпляры** относятся к коду (и состоянию для служб с отслеживанием состояния) службы, которая развернута и запущена. Ознакомьтесь с разделом [Реплики и экземпляры](service-fabric-concepts-replica-lifecycle.md).

**Перенастройка** означает обработку любого изменения в наборе реплик службы. Ознакомьтесь с разделом [Перенастройка](service-fabric-concepts-reconfiguration.md).

**Пакет службы** — каталог на диске, в котором хранится файл типа службы `ServiceManifest.xml`. В этом файле указаны ссылки на пакеты кода, статических данных и конфигураций для данного типа службы. Ссылки на файлы из каталога пакетов служб указываются в файле `ApplicationManifest.xml` для соответствующего типа приложения. Например, в пакете службы могут быть указаны ссылки на пакеты кода, статических данных и конфигураций, определяющие службу баз данных.

**Именованная служба.** Создав именованное приложение, можно создать в кластере экземпляр одной из его служб определенного типа. Укажите тип службы, используя его имя или версию. Каждому экземпляру службы определенного типа назначается код URI, частью которого будет URI именованного приложения. Например, если в именованном приложении MyNamedApp вы создадите именованную службу MyDatabase, то ее универсальный код ресурса (URI) будет выглядеть так: `"fabric:/MyNamedApp/MyDatabase"`. В именованном приложении можно создать несколько именованных служб. У каждой именованной службы может быть своя схема секционирования и свое количество реплик или экземпляров.

**Пакет кода**: каталог на диске, в котором хранятся исполняемые файлы службы определенного типа (обычно файлы EXE или DLL). Ссылки на файлы из каталога пакетов кода указываются в файле `ServiceManifest.xml` для соответствующего типа службы. При создании именованной службы пакет кода копируется на один или несколько узлов, выбранных для запуска этой службы. Затем код запускается. Существует два типа исполняемых файлов для пакетов кода.

* **Гостевые исполняемые файлы** — исполняемые файлы, выполняющиеся "как есть" в операционной системе сервера виртуальных машин (Windows или Linux). Эти исполняемые файлы никак не связаны с файлами среды выполнения Service Fabric, поэтому они не используют ни одну из моделей программирования Service Fabric. Эти исполняемые файлы не могут использовать некоторые возможности Service Fabric, в том числе службу именования для обнаружения конечных точек. Гостевые исполняемые файлы не могут сообщать о метриках нагрузки, относящихся к каждому экземпляру службы.
* **Исполняемые файлы узла службы** — исполняемые файлы, использующие модели программирования Service Fabric. Они создают связи с файлами среды выполнения Service Fabric и позволяют использовать возможности Service Fabric. Например, экземпляр именованной службы может регистрировать конечные точки в службе именования Service Fabric и передавать показатели нагрузки.

**Пакет данных** — каталог на диске, в котором хранятся статические файлы данных службы определенного типа, предназначенные только для чтения (обычно фотографии, аудио и видео). Ссылки на файлы из каталога пакетов данных указываются в файле `ServiceManifest.xml` для соответствующего типа службы. При создании именованной службы пакет данных копируется на один или несколько узлов, выбранных для запуска этой службы. После этого начинается выполнение кода, и теперь у него имеется доступ к файлам данных.

**Пакет конфигурации** — каталог на диске, в котором хранятся статические файлы конфигурации службы определенного типа, предназначенные только для чтения (обычно текстовые файлы). Ссылки на файлы из каталога пакетов конфигураций указываются в файле `ServiceManifest.xml` для соответствующего типа службы. При создании именованной службы файлы из пакета конфигурации копируются на один или несколько узлов, выбранных для запуска этой службы. Затем начинается выполнение кода, и теперь у него имеется доступ к файлам конфигурации.

**Контейнеры.** По умолчанию Service Fabric развертывает и активирует службы как процессы. Service Fabric также позволяет развертывать службы в образах контейнеров. Контейнеры — это технология виртуализации, которая виртуализирует базовую операционную систему в приложениях. Приложение, его среда выполнения, зависимости и системные библиотеки выполняются внутри контейнера. Контейнер имеет полный частный доступ к собственному изолированному представлению конструкций операционной системы. Service Fabric поддерживает контейнеры Docker в Linux и контейнеры Windows Server. Дополнительные сведения см. в статье [Service Fabric и контейнеры (предварительная версия)](service-fabric-containers-overview.md).

**Схема секционирования.** При создании именованной службы указывается схема секционирования. Службы с большим количеством состояний разделяют данные по секциям, которые распределяют состояние между узлами кластера. Разделяя данные по секциям, можно масштабировать состояние именованной службы. В пределах одной секции именованные службы без отслеживания состояния хранят экземпляры, тогда как именованные службы с отслеживанием состояния — реплики. Как правило, именованные службы без отслеживания состояния имеют только одну секцию, так как у них нет внутреннего состояния. Экземпляры в секциях обеспечивают доступность. Если один экземпляр выходит из строя, другие продолжают работать обычным образом, а Service Fabric создает новый экземпляр. Именованные службы с отслеживанием состояния хранят сведения о своем состоянии в репликах, а у каждой секции есть свой набор реплик. Все сведения о состоянии службы постоянно синхронизируются. Если в реплике возникает сбой, Service Fabric создает новую реплику из существующих.

Дополнительные сведения см. в статье [Секционирование служб Reliable Services в Service Fabric](service-fabric-concepts-partitioning.md).

## <a name="system-services"></a>Системные службы
Существуют системные службы, которые создаются в каждом кластере и предоставляют возможности платформы Service Fabric.

**Служба именования.** У каждого кластера Service Fabric есть служба именования, которая разрешает имена службы в определенные расположения на кластере. Вы можете управлять именами и свойствами служб так же, как и служба доменных имен (DNS) для кластера. Используя службу именования для разрешения имени службы в ее расположение, клиенты могут безопасно обмениваться данными с любым узлом в кластере. Приложения перемещаются в кластере. Это может быть из-за сбоев, балансировки ресурсов или изменения размера кластера. Вы можете разрабатывать службы и клиенты, которые разрешают текущее сетевое расположение. Клиенты получают фактические IP-адрес и порт компьютера, на котором она запущена.

Чтобы получить дополнительные сведения об использовании интерфейсов API связи клиента и службы, которые работают со службой именования, см. статью [Подключение к службам в Service Fabric и взаимодействие с ними](service-fabric-connect-and-communicate-with-services.md).

**Служба хранилища образов.** У каждого кластера Service Fabric есть служба хранилища образов, в которой хранятся развернутые пакеты приложения с управлением версиями. Скопируйте содержимое пакета приложения в хранилище образов и затем зарегистрируйте тип приложения, содержащегося в этом пакете приложения. Когда тип приложения будет подготовлен, вы сможете создавать из него именованное приложение. Отменить регистрацию типа приложения в службе хранилища образов можно только после удаления всех связанных с этим типом именованных приложений.

Дополнительные сведения о службе хранилища образов см. в разделе [Общие сведения о параметре ImageStoreConnectionString](service-fabric-image-store-connection-string.md).

Дополнительные сведения о развертывании приложений в службе хранилища образов см. в статье [Развертывание и удаление приложений с помощью PowerShell](service-fabric-deploy-remove-applications.md).

**Служба диспетчера отработки отказов.** В каждом кластере Service Fabric есть служба диспетчера отработки отказов, которая:
   - выполняет функции, связанные с обеспечением высокой доступности и согласованности служб;
   - управляет обновлениями приложения и кластера;
   - взаимодействует с другими компонентами системы.

**Служба диспетчера восстановления.** Это необязательная системная служба, которая обеспечивает безопасность, прозрачность и возможность автоматизации действий по восстановлению в кластере. Диспетчер восстановления используется в следующих ситуациях:
   - Выполнение задач по восстановлению в ходе обслуживания Azure в кластерах Azure Service Fabric на [уровнях устойчивости Silver и Gold](service-fabric-cluster-capacity.md#the-durability-characteristics-of-the-cluster).
   - Выполнение действий по восстановлению для [приложения оркестрации исправлений](service-fabric-patch-orchestration-application.md).

## <a name="deployment-and-application-models"></a>Модели развертывания и приложения 

Чтобы развернуть свои службы, необходимо описать их запуск. Service Fabric поддерживает три разные модели развертывания.

### <a name="resource-model-preview"></a>Модель ресурсов (предварительная версия)
Ресурсы Service Fabric — это все, что может быть по отдельности развернуто в Service Fabric, включая приложения, службы, сети и тома. Ресурсы определяются с помощью JSON-файла, который можно развернуть для конечной точки кластера.  Для Сетки Service Fabric используется схема модели ресурсов Azure. Также можно использовать схему файла YAML для упрощенного создания файлов определений. Ресурсы можно развертывать везде, где выполняется Service Fabric. Модель ресурсов — это самый простой способ описания приложений Service Fabric. Ее основная задача — простое развертывание и администрирование контейнерных служб. Дополнительные сведения см. в разделе [Общие сведения о модели ресурсов Service Fabric](/azure/service-fabric-mesh/service-fabric-mesh-service-fabric-resources).

### <a name="native-model"></a>Собственная модель
Собственная модель приложения предоставляет приложениям полный низкоуровневый доступ к Service Fabric. Приложения и службы определяются как зарегистрированные типы в XML-файлах манифеста.

Собственная модель поддерживает платформу Reliable Services и Reliable Actors, которая предоставляет доступ к интерфейсам API среды выполнения Service Fabric и интерфейсам API управления кластерами в средах C# и Java. Собственная модель также поддерживает произвольные контейнеры и исполняемые файлы. Собственная модель не поддерживается в среде [Сетка Service Fabric](/azure/service-fabric-mesh/service-fabric-mesh-overview).

**Reliable Services** — API для создания служб без отслеживания и с отслеживанием состояния. Служба с отслеживанием состояния хранит сведения о своем состоянии в коллекциях Reliable Collections (например, словарь или очередь). Кроме того, вы можете использовать разнообразные коммуникационные стеки (например, веб-API и Windows Communication Foundation (WCF)).

**Reliable Actors** — API для создания объектов без отслеживания и с отслеживанием состояния с помощью модели программирования с виртуальными субъектами. Эту модель можно использовать, если у вас есть множество независимых единиц вычисления или состояния. Так как в этой модели применяется пошаговая потоковая модель, мы не рекомендуем использовать код, который обращается к другим субъектам или службам, потому что отдельный субъект не может обрабатывать другие входящие запросы, пока не будут обработаны все его исходящие запросы.

Также в Service Fabric можно выполнять существующие приложения:

**Контейнеры.**  Service Fabric поддерживает развертывание контейнеров Docker в Linux и контейнеров Windows Server в Windows Server 2016, а также режим изоляции Hyper-V. В [модели приложения](service-fabric-application-model.md)Service Fabric контейнер представляет собой узел приложения, в котором размещается несколько реплик службы. В Service Fabric можно запускать все контейнеры. Сценарий аналогичен сценарию с готовым исполняемым файлом, при котором вы упаковываете существующее приложение внутри контейнера. Кроме того, [внутри контейнеров можно выполнять службы Service Fabric](service-fabric-services-inside-containers.md).

**Гостевые исполняемые файлы.** В Azure Service Fabric можно запустить как службу приложение любого типа, в том числе приложения Node.js, Java или C++. В Service Fabric такие типы служб называются гостевыми исполняемыми файлами и обрабатываются как службы без отслеживания состояния. Преимущества запуска гостевого исполняемого файла в кластере Service Fabric — высокий уровень доступности, мониторинг работоспособности, управление жизненным циклом приложений, высокая плотность и возможность обнаружения.

Дополнительные сведения см. в статье [Общие сведения о модели программирования Service Fabric](service-fabric-choose-framework.md).

### <a name="docker-compose"></a>Docker Compose 
[Docker Compose](https://docs.docker.com/compose/) является частью проекта Docker. Service Fabric предоставляет ограниченную поддержку [развертывания приложений с помощью модели Docker Compose](service-fabric-docker-compose.md).

## <a name="environments"></a>средами;

Service Fabric — это платформа технологий с открытым кодом, основанная на нескольких различных службах и продуктах. Корпорация Майкрософт предоставляет следующие решения.

 - **Сетка Azure Service Fabric** — полностью управляемая служба для запуска приложений Service Fabric в Microsoft Azure.
 - **Azure Service Fabric**: предложение кластера Azure Service Fabric. Он обеспечивает интеграцию Service Fabric и инфраструктуры Azure, а также обновление и управление конфигурацией кластеров Service Fabric.
 - **Изолированная служба Service Fabric**: набор инструментов для установки и настройки для [развертывания кластеров Service Fabric в любой среде](/azure/service-fabric/service-fabric-deploy-anywhere) (в локальной среде или среде любого поставщика облачных служб). Эта служба управляется Azure.
 - **Кластер разработки Service Fabric**: предоставляет локальную среду разработки на основе Windows, Linux или Mac для разработки приложений Service Fabric.

## <a name="environment-framework-and-deployment-model-support-matrix"></a>Матрица поддержки сред, платформ и моделей развертывания
Различные среды имеют разные уровни поддержки платформ и моделей развертывания. В таблице ниже перечислены поддерживаемые сочетания платформ и моделей развертывания.

| Тип приложения | Описание | Служба "Сетка Azure Service Fabric" | Кластеры Azure Service Fabric (любая ОС)| Локальный кластер | Изолированный кластер |
|---|---|---|---|---|---|
| Приложения службы "Сетка Service Fabric" | Модель ресурсов (в формате YAML и JSON) | Поддерживаются |Не поддерживается | Windows поддерживается, Linux и Mac не поддерживаются | Windows не поддерживается |
|Собственные приложения Service Fabric | Собственная модель приложения (в формате XML) | Не поддерживается| Поддерживаются|Поддерживаются|Windows поддерживается|

Ниже перечислены другие модели приложений и наборы инструментов для них в Service Fabric.

| Тип приложения | Описание | Visual Studio | Eclipse | SFCTL | AZ CLI | PowerShell|
|---|---|---|---|---|---|---|
| Приложения службы "Сетка Service Fabric" | Модель ресурсов (в формате YAML и JSON) | VS 2017 |Не поддерживается |Не поддерживается | Поддерживается только среда "Сетка" | Не поддерживается|
|Собственные приложения Service Fabric | Собственная модель приложения (в формате XML) | VS 2017 и VS 2015| Поддерживаются|Поддерживаются|Поддерживаются|Поддерживаются|

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Дальнейшие действия
Дополнительные сведения о платформе Service Fabric см. в следующих статьях.

* [Общие сведения о Service Fabric](service-fabric-overview.md)
* [Разработка приложений с использованием микрослужб](service-fabric-overview-microservices.md)
* [Сценарии приложений](service-fabric-application-scenarios.md)

Дополнительные сведения о Сетке Service Fabric:

* [Общие сведения о Сетке Service Fabric](/azure/service-fabric-mesh/service-fabric-mesh-overview)
