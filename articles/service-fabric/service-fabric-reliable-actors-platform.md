---
title: Reliable Actors в Service Fabric | Документация Майкрософт
description: В этой статье описывается структура субъектов Reliable Actors в службах Reliable Services и работа с функциями платформы Service Fabric.
services: service-fabric
documentationcenter: .net
author: vturecek
manager: chackdan
editor: amanbha
ms.assetid: 45839a7f-0536-46f1-ae2b-8ba3556407fb
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 3/9/2018
ms.author: vturecek
ms.openlocfilehash: bc7569c9f230abb7677a8df9fc0cc0268e57296f
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "60725929"
---
# <a name="how-reliable-actors-use-the-service-fabric-platform"></a>Использование платформы Service Fabric надежными субъектами
В этой статье объясняется, как работают субъекты Reliable Actors на платформе Azure Service Fabric. Субъекты Reliable Actors выполняются в среде, размещенной в реализации службы Reliable Services с отслеживанием состояния под названием *служба субъектов*. Служба субъектов содержит все компоненты, необходимые для управления жизненным циклом и диспетчеризации относящихся к субъектам сообщений:

* Среда выполнения субъектов управляет жизненным циклом и сбором мусора, а также обеспечивает однопоточный доступ.
* Прослушиватель удаленных взаимодействий субъекта службы принимает вызовы удаленного доступа к субъектам и отправляет их диспетчеру для маршрутизации в соответствующий экземпляр субъекта.
* Поставщик состояний субъекта служит оболочкой для поставщиков состояний (например для поставщика состояний надежных коллекций) и предоставляет адаптер для управления состоянием субъекта.

Вместе эти компоненты образуют платформу Reliable Actors.

## <a name="service-layering"></a>Структура служб
Так как служба субъектов сама по себе является надежной службой, понятия [модели приложений](service-fabric-application-model.md), жизненного цикла, [упаковки](service-fabric-package-apps.md), [развертывания](service-fabric-deploy-remove-applications.md), обновления и масштабирования, связанные с Reliable Services, также относятся и к службам субъектов.

![Структура службы субъектов][1]

На предыдущей схеме показана связь между платформами приложений Service Fabric и кодом пользователя. Синие элементы обозначают платформу приложений служб Reliable Services, оранжевые — платформу Reliable Actor, а зеленые — код пользователя.

В службах Reliable Services ваша служба наследует класс `StatefulService`, производный от `StatefulServiceBase` (или `StatelessService` для служб без отслеживания состояния). В службе Reliable Actors используется служба субъектов с другой реализацией класса `StatefulServiceBase` — она реализует шаблон субъекта, согласно которому выполняются ваши субъекты. Так как служба субъектов — всего лишь реализация класса `StatefulServiceBase`, вы можете написать свою собственную службу, производную от класса `ActorService`, и реализовать в ней функции уровня службы таким же образом, как при наследовании класса `StatefulService`, например:

* резервное копирование и восстановление службы;
* общие функции для всех субъектов, например автоматическое выключение;
* вызовы удаленных взаимодействий для самой службы субъектов, а также для любого отдельного субъекта.

Дополнительные сведения см. в статье [Реализация функций уровня службы в службе субъектов](service-fabric-reliable-actors-using.md).

## <a name="application-model"></a>Модель приложения
Службы субъектов — это службы Reliable Services, поэтому используют точно такую же модель приложений. В то же время инструменты сборки платформы субъектов создают для вас некоторые файлы модели приложений.

### <a name="service-manifest"></a>Манифест службы
Содержимое файла ServiceManifest.xml для службы субъектов создается инструментами сборки платформы субъектов автоматически. Этот файл содержит:

* Тип службы субъектов. Имя типа создается с учетом имени проекта субъекта. В зависимости от атрибута сохраняемости у субъекта устанавливается флаг HasPersistedState.
* Пакет кода.
* Пакет конфигурации.
* Ресурсы и конечные точки.

### <a name="application-manifest"></a>Манифест приложения
Средства сборки платформы субъектов автоматически создают для вашей службы субъектов определение по умолчанию. Свойства службы по умолчанию заполняются инструментами сборки:

* Счетчик наборов реплик определяется атрибутом сохраняемости у субъекта. Каждый раз, когда атрибут сохраняемости у субъекта изменяется, счетчик наборов реплик в определении службы по умолчанию сбрасывается.
* Устанавливается схема секционирования и диапазон Uniform Int64 с полным диапазоном ключей Int64.

## <a name="service-fabric-partition-concepts-for-actors"></a>Основные понятия о секции Service Fabric для субъектов
Службы субъектов — это секционированные службы с отслеживанием состояния. Каждая секция службы субъектов содержит набор субъектов. Секции службы автоматически распределяются между несколькими узлами Service Fabric. В результате распространяются экземпляры субъектов.

![Секционирование и распределение субъектов][5]

Службы Reliable Services создаются с использованием различных схем и диапазонов ключей секционирования. Служба субъектов использует схему секционирования Int64 с полным диапазоном ключей Int64 для сопоставления субъектов и секций.

### <a name="actor-id"></a>Идентификатор субъекта
Каждый созданный в службе субъект имеет уникальный идентификатор, представляемый классом `ActorId` . `ActorId` — это непрозрачное значение идентификатора, которое можно использовать для равномерного распределения субъектов между секциями служб за счет генерации идентификаторов случайным образом:

```csharp
ActorProxy.Create<IMyActor>(ActorId.CreateRandom());
```
```Java
ActorProxyBase.create<MyActor>(MyActor.class, ActorId.newId());
```


Каждый `ActorId` хэшируется в значение типа Int64, поэтому в службе субъектов необходимо использовать схему секционирования Int64 с полным диапазоном ключей Int64. Тем не менее для `ActorID` можно использовать пользовательские значения идентификаторов, включая строковые значения, а также GUID или UUID и значения типа Int64.

```csharp
ActorProxy.Create<IMyActor>(new ActorId(Guid.NewGuid()));
ActorProxy.Create<IMyActor>(new ActorId("myActorId"));
ActorProxy.Create<IMyActor>(new ActorId(1234));
```
```Java
ActorProxyBase.create(MyActor.class, new ActorId(UUID.randomUUID()));
ActorProxyBase.create(MyActor.class, new ActorId("myActorId"));
ActorProxyBase.create(MyActor.class, new ActorId(1234));
```

При использовании строк и GUID или UUID значения хэшируются в значения типа Int64. Однако, если значение типа Int64 предоставляется в `ActorId` напрямую, Int64 сопоставляется с секцией напрямую без дополнительного хэширования. Этот метод можно использовать для управления тем, в каких секциях будут размещаться субъекты.


## <a name="next-steps"></a>Дальнейшие действия
* [Управление состоянием субъекта](service-fabric-reliable-actors-state-management.md)
* [Жизненный цикл субъектов и сбор мусора](service-fabric-reliable-actors-lifecycle.md)
* [Справочная документация по API субъектов](https://docs.microsoft.com/dotnet/api/microsoft.servicefabric.actors?redirectedfrom=MSDN&view=azure-dotnet)
* [Пример кода .NET](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started)
* [Пример кода Java](https://github.com/Azure-Samples/service-fabric-java-getting-started)

<!--Image references-->
[1]: ./media/service-fabric-reliable-actors-platform/actor-service.png
[2]: ./media/service-fabric-reliable-actors-platform/app-deployment-scripts.png
[3]: ./media/service-fabric-reliable-actors-platform/actor-partition-info.png
[4]: ./media/service-fabric-reliable-actors-platform/actor-replica-role.png
[5]: ./media/service-fabric-reliable-actors-introduction/distribution.png
