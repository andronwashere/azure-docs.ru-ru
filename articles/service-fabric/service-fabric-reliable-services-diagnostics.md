---
title: Диагностика надежных служб с отслеживанием состояния Azure Service Fabric | Документация Майкрософт
description: Диагностические функции для надежных служб с отслеживанием состояния в Azure Service Fabric
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: chackdan
editor: ''
ms.assetid: ae0e8f99-69ab-4d45-896d-1fa80ed45659
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 8/24/2018
ms.author: dekapur
ms.openlocfilehash: f49176f944aa2abfa1d355ce0bd207d1b544c275
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "60772964"
---
# <a name="diagnostic-functionality-for-stateful-reliable-services"></a>Диагностические функции для надежных служб с отслеживанием состояния
Класс StatefulServiceBase служб Reliable Services с отслеживанием состояния Azure Service Fabric генерирует события [EventSource](https://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource.aspx), которые можно использовать для отладки. Они позволяют исследовать работу среды выполнения и помогают устранять неполадки.

## <a name="eventsource-events"></a>События EventSource
Имя EventSource для класса StatefulServiceBase надежных служб с отслеживанием состояния — Microsoft-ServiceFabric-Services. При [отладке службы в Visual Studio](service-fabric-debugging-your-application.md). события из этого источника отображаются в окне [Diagnostics Events](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md#view-service-fabric-system-events-in-visual-studio) (События диагностики).

Для сбора и просмотра событий EventSource вы можете использовать такие средства и технологии, как [PerfView](https://www.microsoft.com/download/details.aspx?id=28567), [система диагностики Azure](../cloud-services/cloud-services-dotnet-diagnostics.md) и [библиотека Microsoft TraceEvent](https://www.nuget.org/packages/Microsoft.Diagnostics.Tracing.TraceEvent).

## <a name="events"></a>События
| Имя события | Идентификатор события | Уровень | Описание события |
| --- | --- | --- | --- |
| StatefulRunAsyncInvocation |1 |Информация |Генерируется, когда запускается задача RunAsync службы. |
| StatefulRunAsyncCancellation |2 |Информация |Генерируется, когда отменяется задача RunAsync службы. |
| StatefulRunAsyncCompletion |3 |Информация |Генерируется, когда завершается задача RunAsync службы. |
| StatefulRunAsyncSlowCancellation |4\. |Предупреждение |Генерируется, когда отмена задачи RunAsync службы выполняется слишком долго. |
| StatefulRunAsyncFailure |5 |Ошибка |Генерируется, когда задача RunAsync службы вызывает исключение. |

## <a name="interpret-events"></a>Интерпретация событий
События StatefulRunAsyncInvocation, StatefulRunAsyncCompletion и StatefulRunAsyncCancellation полезны для разработчика служб тем, что позволяют понять жизненный цикл службы и правильно определить время запуска, отмены или завершения службы. Эти сведения могут быть полезными при отладке службы и анализе ее жизненного цикла.

Разработчикам служб следует обратить особое внимание на события StatefulRunAsyncSlowCancellation и StatefulRunAsyncFailure, так как они свидетельствуют о наличии проблем со службой.

Событие StatefulRunAsyncFailure генерируется каждый раз, когда задача RunAsync() службы вызывает исключение. Обычно исключение указывает на наличие ошибки или дефекта в работе службы. Кроме того, исключение приводит к отказу службы и, как следствие, ее перемещению на другой узел. Эта операция может быть ресурсоемкой, из-за чего входящие запросы будут обработаны с задержкой. Разработчикам службы следует определить причину исключения и по возможности нейтрализовать ее.

Событие StatefulRunAsyncSlowCancellation генерируется всякий раз, когда выполнение запроса на отмену задачи RunAsync занимает больше четырех секунд. Если отмена выполняется слишком долго, служба не может быстро перезапуститься на другом узле. Кроме того, снижается ее общая доступность.

## <a name="performance-counters"></a>Счетчики производительности
Среда выполнения служб Reliable Services определяет указанные ниже категории счетчиков производительности.

| Категория | Описание |
| --- | --- |
| Репликатор транзакций Service Fabric |Счетчики для репликатора транзакций Azure Service Fabric |
| Service Fabric TStore |Счетчики для Azure Service Fabric TStore |

Репликатор транзакций Service Fabric используется [диспетчером надежных состояний](service-fabric-reliable-services-reliable-collections-internals.md) для репликации транзакций в пределах заданного набора [реплик](service-fabric-concepts-replica-lifecycle.md).

Service Fabric TStore — это компонент, который используется в [надежных коллекциях](service-fabric-reliable-services-reliable-collections-internals.md) для хранения и извлечения пар "ключ-значение".

Для сбора и просмотра данных счетчиков производительности можно использовать [системный монитор Windows](https://technet.microsoft.com/library/cc749249.aspx) , доступный по умолчанию в операционной системе Windows. [Диагностика Azure](../cloud-services/cloud-services-dotnet-diagnostics.md) , в свою очередь, позволяет не только собирать данные счетчиков производительности, но и загружать их в таблицы Azure.

### <a name="performance-counter-instance-names"></a>Имена экземпляров счетчиков производительности
В кластере, содержащем большое количество надежных служб или секций надежных служб, находится большое количество экземпляров счетчиков производительности репликатора транзакций. Это же применимо и для счетчиков производительности TStore, но с учетом числа используемых надежных словарей и очередей. Имя экземпляра счетчика производительности помогает определить конкретную [секцию](service-fabric-concepts-partitioning.md), реплику службы и поставщика состояний, с которыми связан этот экземпляр.

#### <a name="service-fabric-transactional-replicator-category"></a>Категория репликатора транзакций Service Fabric
Экземпляры счетчиков в категории `Service Fabric Transactional Replicator`имеют имена в следующем формате:

`ServiceFabricPartitionId:ServiceFabricReplicaId`

*ServiceFabricPartitionID* — это строка идентификатора секции Service Fabric, c которой связан экземпляр счетчика производительности. Это идентификатор GUID, строковое представление которого создается методом [`Guid.ToString`](https://msdn.microsoft.com/library/97af8hh4.aspx) с использованием описателя формата D.

*ServiceFabricReplicaId* — идентификатор, связанный с конкретной репликой надежной службы. В имя экземпляра счетчика производительности идентификатор реплики включается для того, чтобы обеспечить его уникальность и избежать конфликтов с другими экземплярами счетчиков производительности, созданными той же секцией. Дополнительные сведения о репликах и их роли в надежных службах см. в [этой статье](service-fabric-concepts-replica-lifecycle.md).

Следующее имя экземпляра счетчика является типичным для счетчиков в категории `Service Fabric Transactional Replicator`.

`00d0126d-3e36-4d68-98da-cc4f7195d85e:131652217797162571`

В предыдущем примере `00d0126d-3e36-4d68-98da-cc4f7195d85e` является строковым представлением идентификатора секции Service Fabric, а `131652217797162571` — идентификатором реплики.

#### <a name="service-fabric-tstore-category"></a>Категория Service Fabric TStore
Экземпляры счетчиков в категории `Service Fabric TStore`имеют имена в следующем формате:

`ServiceFabricPartitionId:ServiceFabricReplicaId:ServiceFabricStateProviderId_PerformanceCounterInstanceDifferentiator`

*ServiceFabricPartitionID* — это строка идентификатора секции Service Fabric, c которой связан экземпляр счетчика производительности. Это идентификатор GUID, строковое представление которого создается методом [`Guid.ToString`](https://msdn.microsoft.com/library/97af8hh4.aspx) с использованием описателя формата D.

*ServiceFabricReplicaId* — идентификатор, связанный с конкретной репликой надежной службы. В имя экземпляра счетчика производительности идентификатор реплики включается для того, чтобы обеспечить его уникальность и избежать конфликтов с другими экземплярами счетчиков производительности, созданными той же секцией. Дополнительные сведения о репликах и их роли в надежных службах см. в [этой статье](service-fabric-concepts-replica-lifecycle.md).

*ServiceFabricReplicaId* — идентификатор, связанный с поставщиком состояний в пределах надежной службы. Идентификатор поставщика состояния включен в имя экземпляра счетчика производительности, чтобы различать экземпляры TStore.

*PerformanceCounterInstanceDifferentiator* — отличительный идентификатор, связанный с экземпляром счетчика производительности в пределах поставщика состояний. В имя экземпляра счетчика производительности этот идентификатор включается для того, чтобы обеспечить его уникальность и избежать конфликтов с другими экземплярами счетчиков производительности, созданными тем же поставщиком состояний.

Следующее имя экземпляра счетчика является типичным для счетчиков в категории `Service Fabric TStore`.

`00d0126d-3e36-4d68-98da-cc4f7195d85e:131652217797162571:142652217797162571_1337`

В предыдущем примере `00d0126d-3e36-4d68-98da-cc4f7195d85e` является строковым представлением идентификатора секции Service Fabric, `131652217797162571` — идентификатором реплики, `142652217797162571` — идентификатор поставщика состояний, а `1337` — идентификатор экземпляра счетчика производительности.

### <a name="transactional-replicator-performance-counters"></a>Счетчики производительности репликатора транзакций

Среда выполнения Reliable Services выдает следующие события в категории `Service Fabric Transactional Replicator`.

 Имя счетчика | Описание |
| --- | --- |
| Операций начала транзакций/с | Число новых транзакций записи, создаваемых в секунду.|
| Операций транзакций/с | Число операций добавления, обновления и удаления, выполненных для надежных коллекций, в секунду.|
| Записи на диск байтов журнала/с | Число байтов, записываемых на диск репликатором транзакций в секунду. |
| Регулируемых операций/с | Число операций, отклоняемых репликатором транзакций каждую секунду из-за регулирования. |
| Среднее Время транзакции на фиксацию (в мс) | Среднее время задержки фиксации на транзакцию (в мс). |
| Среднее Задержка очистки (мс) | Средняя продолжительность операции окончательной записи данных на диск, инициированной репликатором транзакций (в мс). |

### <a name="tstore-performance-counters"></a>Счетчики производительности TStore

Среда выполнения Reliable Services выдает следующие события в категории `Service Fabric TStore`.

 Имя счетчика | Описание |
| --- | --- |
| Число элементов | Число элементов в хранилище.|
| Размер диска | Общий размер в байтах, занимаемый на диске файлами контрольных точек хранилища.|
| Запись в файл контрольной точки, байт/с | Число байтов, записываемых в секунду в последний файл контрольной точки.|
| Передача на диск при копировании (байт/с) | Число байтов, считываемых (с первичной реплики) или записываемых (на вторичную реплику) в секунду при копировании хранилища.|

## <a name="next-steps"></a>Дальнейшие действия
[Поставщики EventSource в PerfView](https://blogs.msdn.microsoft.com/vancem/2012/07/09/introduction-tutorial-logging-etw-events-in-c-system-diagnostics-tracing-eventsource/)
