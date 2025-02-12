---
title: Резервное копирование и восстановление Service Fabric | Документация Майкрософт
description: Основная документация по резервному копированию и восстановлению платформы Service Fabric
services: service-fabric
documentationcenter: .net
author: mcoskun
manager: chackdan
editor: subramar,zhol
ms.assetid: 91ea6ca4-cc2a-4155-9823-dcbd0b996349
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/29/2018
ms.author: mcoskun
ms.openlocfilehash: cd40f59cfa7846911c68206c3bc1e85a770b0fcc
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "60723873"
---
# <a name="backup-and-restore-reliable-services-and-reliable-actors"></a>Резервное копирование и восстановление Reliable Services и Reliable Actors
Azure Service Fabric — это платформа высокой доступности, которая реплицирует состояние между несколькими узлами для обеспечения высокой доступности.  Таким образом, даже в случае сбоя одного узла в кластере службы будут оставаться доступными. Хотя иногда предоставленной платформой встроенной избыточности достаточно, в некоторых случаях желательно выполнять резервное копирование данных службы (во внешнее хранилище).

> [!NOTE]
> Крайне важно создавать резервные копии и восстанавливать данные (и проверить, что они работают) в случае потери данных.
> 

> [!NOTE]
> Корпорация Майкрософт рекомендует настроить [периодическое резервное копирование и восстановление](service-fabric-backuprestoreservice-quickstart-azurecluster.md) данных из служб с отслеживанием состояния Reliable Services и Reliable Actors. 
> 


Например, службе может потребоваться создать резервную копию данных для защиты от таких сценариев:

- в случае полной потери всего кластера Service Fabric.
- Полной потери большей части реплик секций служб.
- Если из-за административных ошибок произошло случайное удаление или повреждение состояния. Это может произойти, если администратор с достаточными привилегиями ошибочно удалил службу.
- Если из-за ошибок в службе происходит повреждение данных. Например, это может произойти, если при обновлении кода службы начинается запись поврежденных данных в надежную коллекцию. В этом случае может потребоваться вернуть код и данные в предыдущее состояние.
- При автономной обработке данных. Автономная обработка данных для бизнес-аналитики, которая происходит отдельно от службы, создающей данные, может быть очень удобной.

Благодаря функции резервного копирования и восстановления службы, созданные на основе интерфейса API Reliable Services, могут создавать и восстанавливать резервные копии. Интерфейсы API резервного копирования, предоставляемые платформой, позволяют выполнять резервное копирование состояния секции службы без блокировки операций чтения или записи. Благодаря им можно восстановить состояние раздела службы из выбранной резервной копии.

## <a name="types-of-backup"></a>Типы архивации
Существует два вида резервного копирования: полное и добавочное.
Полная резервная копия — это копия, которая содержит все данные, необходимые для воссоздания состояния реплики: контрольные точки и все записи журнала.
Так как она содержит контрольные точки и журнал, полная резервная копия может быть восстановлена самостоятельно.

Проблемы с полными резервными копиями возникают, когда контрольные точки имеют большой размер.
Например, у реплики, которая имеет 16 ГБ данных состояния, будут контрольные точки, которые добавят примерно до 16 ГБ данных.
Если целевая точка восстановления составляет пять минут, реплику необходимо архивировать каждые пять минут.
При каждой архивации требуется скопировать 16 ГБ контрольных точек в дополнение к 50 МБ журналов (можно настроить с помощью `CheckpointThresholdInMB`).

![Пример полной архивации.](media/service-fabric-reliable-services-backup-restore/FullBackupExample.PNG)

Решить эту проблему можно с помощью добавочного резервного копирования, при котором резервные копии создаются только для записей журнала, измененных после последнего резервного копирования.

![Пример добавочной архивации.](media/service-fabric-reliable-services-backup-restore/IncrementalBackupExample.PNG)

Так как в добавочные резервные копии архивируются только изменения, внесенные с момента последней архивации (без контрольных точек), то обычно они быстрее, однако их нельзя восстановить самостоятельно.
Чтобы восстановить добавочную резервную копию, требуется вся цепочка резервных копий.
Цепочка резервных копий — это ряд резервных копий, начинающийся с полной резервной копии, за которой следует несколько добавочных резервных копий, которые создавались непрерывно.

## <a name="backup-reliable-services"></a>Резервное копирование служб Reliable Services
Создатель службы полностью контролирует время выполнения резервного копирования и место хранения резервных копий.

Чтобы начать архивацию, служба должна вызвать наследуемую функцию-член `BackupAsync`.  
Резервное копирование можно выполнить только из первичных реплик, которым необходимо присвоить состояние записи.

Как показано ниже, `BackupAsync` принимает объект `BackupDescription`, где можно указать полную или добавочную архивацию, а также функцию обратного вызова `Func<< BackupInfo, CancellationToken, Task<bool>>>`, которая вызывается, если папка резервных копий была создана локально и готова к переносу во внешнее хранилище.

```csharp

BackupDescription myBackupDescription = new BackupDescription(backupOption.Incremental,this.BackupCallbackAsync);

await this.BackupAsync(myBackupDescription);

```

Запрос на добавочную архивацию может завершиться сбоем с исключением `FabricMissingFullBackupException`. Это исключение указывает, что происходит одно из следующих действий:

- для реплики никогда не создавалась полная резервная копия, так как она стала первичной,
- либо с момента последнего ее создания были усечены некоторые записи журнала, либо
- размер реплики превысил ограничение `MaxAccumulatedBackupLogSizeInMB`.

Пользователи могут увеличить вероятность выполнения добавочной архивации, настроив `MinLogSizeInMB` или `TruncationThresholdFactor`.
Обратите внимание, что увеличение этих значений приведет к использованию большего объема дискового пространства каждой репликой.
Чтобы узнать больше, ознакомьтесь с [конфигурацией Reliable Services](service-fabric-reliable-services-configuration.md).

В `BackupInfo` содержатся сведения о резервной копии, включая расположение папки в среде выполнения, где сохранена резервная копия (`BackupInfo.Directory`). Чтобы выполнить функцию обратного вызова, необходимо переместить `BackupInfo.Directory` во внешнее хранилище или другое расположение.  Эта функция также возвращает логическое значение bool, указывающее, успешно ли перемещена папка резервного копирования в целевое расположение.

В следующем коде показано, как с помощью метода `BackupCallbackAsync` можно передать резервную копию в службу хранилища Microsoft Azure:

```csharp
private async Task<bool> BackupCallbackAsync(BackupInfo backupInfo, CancellationToken cancellationToken)
{
    var backupId = Guid.NewGuid();

    await externalBackupStore.UploadBackupFolderAsync(backupInfo.Directory, backupId, cancellationToken);

    return true;
}
```

В примере выше `ExternalBackupStore` — это пример класса, используемого для взаимодействия с хранилищем BLOB-объектов Azure, а `UploadBackupFolderAsync` — это метод, используемый для сжатия и перемещения папки в хранилище BLOB-объектов Azure.

Обратите внимание на следующее.

  - В любой момент времени в реплике можно выполнить только одну операцию резервного копирования. Выполнение нескольких вызовов метода `BackupAsync` одновременно приведет к вызову исключения `FabricBackupInProgressException`, которое ограничит создание резервных копий до одной за раз.
  - В случае сбоя реплики при резервном копировании операция не будет завершена. Таким образом после завершения отработки отказа служба должна перезапустить архивацию, при необходимости вызвав метод `BackupAsync`.

## <a name="restore-reliable-services"></a>Восстановление служб Reliable Services
Как правило, ситуации, в которых может потребоваться выполнить операцию восстановления, относятся к одной из следующих категорий:

  - Потеря данных в разделе службы. Например, диск для двух из трех реплик для раздела (включая первичную реплику) поврежден или очищен. Новой первичной реплике может потребоваться восстановить данные из резервной копии.
  - Утеря всей службы. К примеру, администратор удаляет всю службу, и поэтому необходимо восстановить службу и данные.
  - Репликация поврежденных данных приложения (например, из-за ошибки приложения) в службе. В этом случае необходимо обновить службу или вернуть ее в прежнее состояние, чтобы устранить причину повреждения и восстановить неповрежденные данные.

Несмотря на то что это можно сделать многими способами, мы приведем некоторые примеры использования `RestoreAsync` для восстановления в указанных выше сценариях.

## <a name="partition-data-loss-in-reliable-services"></a>Потеря данных раздела в службах Reliable Services
В этом случае среда выполнения автоматически обнаруживает потерю данных и вызывает интерфейс API `OnDataLossAsync`.

Чтобы восстановить данные, создателю службы необходимо сделать следующее:

  - Переопределите метод виртуального базового класса `OnDataLossAsync`.
  - Найти последнюю резервную копию во внешнем расположении, в котором содержатся резервные копии службы.
  - Скачать последнюю резервную копию (и распаковать ее в папку резервного копирования, если она была сжата).
  - Метод `OnDataLossAsync` предоставляет `RestoreContext`. Вызовите API `RestoreAsync` для указанного `RestoreContext`.
  - Вернуть значение true, если восстановление выполнено успешно.

Ниже приведен пример реализации метода `OnDataLossAsync`:

```csharp
protected override async Task<bool> OnDataLossAsync(RestoreContext restoreCtx, CancellationToken cancellationToken)
{
    var backupFolder = await this.externalBackupStore.DownloadLastBackupAsync(cancellationToken);

    var restoreDescription = new RestoreDescription(backupFolder);

    await restoreCtx.RestoreAsync(restoreDescription);

    return true;
}
```

Элемент `RestoreDescription`, переданный в вызов `RestoreContext.RestoreAsync`, содержит элемент с именем `BackupFolderPath`.
При восстановлении одной полной резервной копии этому элементу `BackupFolderPath` должен быть присвоен локальный путь к папке, содержащей полную резервную копию.
При восстановлении полной резервной копии и нескольких добавочных резервных копий `BackupFolderPath` должен быть присвоен локальный путь к папке, которая содержит не только полную резервную копию, но и все добавочные резервные копии.
Вызов `RestoreAsync` может породить `FabricMissingFullBackupException`, если указанный `BackupFolderPath` не содержит путь к полной резервной копии.
Он также может породить `ArgumentException`, если `BackupFolderPath` указывает на папку с нарушенной цепочкой добавочных резервных копий.
Например, если она содержит полную резервную копию, а также первую и третью добавочные резервные копии, но не содержит вторую добавочную резервную копию.

> [!NOTE]
> Для параметра RestorePolicy по умолчанию установлено значение Safe.  Это означает, что в случае обнаружения в папке резервных копий состояния, которое старее состояния в данной реплике или эквивалентно ему, произойдет сбой API `RestoreAsync` с исключением ArgumentException.  `RestorePolicy.Force` может использоваться для пропуска такой проверки. Этот параметр указывается как часть `RestoreDescription`.
> 

## <a name="deleted-or-lost-service"></a>Удаленная или утерянная служба
Если служба удалена, прежде чем восстанавливать данные, необходимо заново создать службу.  Важно создать службу с такой же конфигурацией, например с такой же схемой секционирования, чтобы можно было легко восстановить данные.  После запуска службы для восстановления данных службы необходимо вызвать интерфейс API (`OnDataLossAsync` выше) в каждой секции этой службы. Один из способов сделать это — использовать [FabricClient.TestManagementClient.StartPartitionDataLossAsync](https://msdn.microsoft.com/library/mt693569.aspx) в каждой секции.  

С этого момента реализация осуществляется по описанному выше сценарию. Необходимо восстановить последнюю соответствующую резервную копию из внешнего хранилища для каждого раздела. Следует учитывать один недостаток: идентификатор раздела, возможно, изменен, так как среда выполнения создает идентификаторы разделов динамически. Таким образом, служба должна хранить сведения о соответствующем разделе и имени службы, чтобы правильно определить последнюю резервную копию, которую необходимо восстановить для каждого раздела.

> [!NOTE]
> Не рекомендуется использовать `FabricClient.ServiceManager.InvokeDataLossAsync` в каждой из секций для восстановления всей службы, так как это может привести к повреждению состояния кластера.
> 

## <a name="replication-of-corrupt-application-data"></a>Репликация поврежденных данных приложения
Наличие ошибки в только что развернутом обновленном приложении может привести к повреждению данных. Например, обновленное приложение может запустить обновление всех записей телефонных номеров в надежном словаре с использованием недопустимого кода региона.  В этом случае недопустимые телефонные номера будут реплицированы, так как Service Fabric не учитывает характер хранимых данных.

Первое, что необходимо сделать после обнаружения такой серьезной ошибки, которая приводит к повреждению данных, — заморозить службу на уровне приложения и по возможности обновить приложение до версии без ошибки в коде.  Даже после исправления кода службы данные по-прежнему могут быть повреждены. Поэтому может потребоваться их восстановление.  В таких случаях восстановления последней резервной копии может оказаться недостаточно, так как последние резервные копии также могут быть повреждены.  Поэтому необходимо найти последнюю резервную копию, выполненную до повреждения данных.

Если вы не знаете, какие резервные копии повреждены, нужно развернуть новый кластер Service Fabric и восстановить резервные копии поврежденных разделов, как описано выше в сценарии «Удаленная или утерянная служба».  Для каждой секции начните восстановление резервных копий от самой последней до самой ранней. Обнаружив резервную копию без повреждений, переместите или удалите все резервные копии этой секции, которые новее этой резервной копии. Повторите эту процедуру для каждой секции. Теперь при вызове метода `OnDataLossAsync` для раздела в рабочем кластере будет обнаружена необходимая последняя резервная копия во внешнем хранилище.

Чтобы восстановить службу до состояния, в котором она находилась до повреждения из-за ошибки в коде, можно использовать действия, описанные в разделе "Удаленная или утерянная служба".

Обратите внимание на следующее.

  - При восстановлении есть вероятность того, что состояние восстанавливаемой резервной копии старее состояния раздела до потери данных. Из-за этого восстановление следует использовать только в качестве последнего средства для восстановления максимального объема данных.
  - Строка, представляющая собой путь к папке резервного копирования и пути к файлам в этой папке, может содержать больше 255 символов в зависимости от пути FabricDataRoot и длины имени типа приложения. Из-за этого такие методы .NET, как `Directory.Move`, могут породить исключение `PathTooLongException`. Одно из возможных решений — напрямую вызывать интерфейсы API kernel32, такие как `CopyFile`.

## <a name="back-up-and-restore-reliable-actors"></a>Резервное копирование и восстановление Reliable Actors


Платформа Reliable Actors создана на основе Reliable Services. ActorService, в котором размещены субъекты, является надежной службой с отслеживанием состояния. Таким образом все функции архивации и восстановления, доступные в Reliable Services, также доступны для Reliable Actors (кроме определенных поведений поставщика состояний). Так как резервные копии будут создаваться для каждого раздела, состояния всех субъектов в этом разделе будут архивированы (восстановление будет происходить для каждого раздела). Для выполнения архивации и восстановления владельцу службы следует создать класс пользовательской службы субъектов, а затем выполнить архивацию или восстановление (аналогично Reliable Services), как описано в предыдущих разделах.

```csharp
class MyCustomActorService : ActorService
{
    public MyCustomActorService(StatefulServiceContext context, ActorTypeInformation actorTypeInfo)
          : base(context, actorTypeInfo)
    {
    }
    
    //
    // Method overrides and other code.
    //
}
```

При создании класса пользовательской службы субъекта необходимо зарегистрировать его, также как при регистрации субъекта.

```csharp
ActorRuntime.RegisterActorAsync<MyActor>(
    (context, typeInfo) => new MyCustomActorService(context, typeInfo)).GetAwaiter().GetResult();
```

Поставщик состояний по умолчанию для Reliable Actors — `KvsActorStateProvider`. Добавочная архивация для `KvsActorStateProvider` не включена по умолчанию. Добавочную архивацию можно включить, создав `KvsActorStateProvider` с соответствующим параметром в конструкторе и передав в конструктор ActorService, как показано в следующем фрагменте кода:

```csharp
class MyCustomActorService : ActorService
{
    public MyCustomActorService(StatefulServiceContext context, ActorTypeInformation actorTypeInfo)
          : base(context, actorTypeInfo, null, null, new KvsActorStateProvider(true)) // Enable incremental backup
    {
    }
    
    //
    // Method overrides and other code.
    //
}
```

После включения добавочной архивации по одной из следующих причин во время его выполнения может возникнуть ошибка FabricMissingFullBackupException, после чего необходимо будет выполнить полную архивацию перед повторной попыткой запуска добавочной:

  - Для реплики никогда не создавалась полная резервная копия, так как она стала первичной.
  - Некоторые записи журнала были усечены с момента последней архивации.

Если добавочная архивация включена, `KvsActorStateProvider` не использует циклический буфер для управления записями журнала и периодически усекает его. Если пользователь не создает резервную копию в течение 45 минут, система автоматически усекает записи журнала. Этот интервал можно настроить, указав `logTruncationIntervalInMinutes` в конструкторе `KvsActorStateProvider` (как при включении добавочной архивации). Если нужно создать первичную реплику путем отправки всех ее данных, записи журнала также могут усекаться.

При выполнении восстановления из цепочки резервных копий BackupFolderPath (аналогично Reliable Services) должен содержать подкаталоги. Один подкаталог должен содержать полную резервную копию, а остальные — добавочные резервные копии. API восстановления выдаст исключение FabricException с соответствующим сообщением об ошибке при сбое проверки цепочки резервных копий. 

> [!NOTE]
> `KvsActorStateProvider` в настоящее время пропускает параметр RestorePolicy.Safe. В следующих выпусках планируется поддержка этой функции.
> 

## <a name="testing-back-up-and-restore"></a>Тестирование резервного копирования и восстановления
Важно убедиться, что для критически важных данных создается резервная копия и их можно восстановить. Это можно сделать, вызвав командлет `Start-ServiceFabricPartitionDataLoss` в PowerShell, что может стать причиной потери данных в определенной секции, чтобы проверить работоспособность архивации и восстановления данных для вашей службы.  Можно также программно вызвать потерю данных и восстановить их из этого события.

> [!NOTE]
> Пример реализации функций архивации и восстановления можно найти в эталонном веб-приложении на сайте GitHub. Подробные сведения см. в описании службы `Inventory.Service`.
> 
> 

## <a name="under-the-hood-more-details-on-backup-and-restore"></a>Дополнительные сведения о резервном копировании и восстановлении
Далее представлены дополнительные сведения о резервном копировании и восстановлении.

### <a name="backup"></a>Azure Backup
Диспетчер надежных состояний позволяет создавать согласованные резервные копии без блокировки операций чтения и записи. Для этого используется механизм сохранения контрольных точек и журналов.  Диспетчер надежных состояний создает нечеткие (упрощенные) контрольные точки в определенных точках, чтобы уменьшить нагрузку на журнал транзакций и сократить время восстановления.  При вызове метода `BackupAsync` диспетчер надежных состояний дает всем надежным объектам команду копировать их последние файлы контрольных точек в локальную папку резервных копий.  Затем он копирует в папку резервного копирования все записи журнала — от «отправной точки» до последней записи в журнале.  Так как все записи журнала включаются в резервную копию и диспетчер надежных состояний сохраняет упреждающие записи журнала, все зафиксированные транзакции (которые успешно вернул `CommitAsync`) будут сохранены при архивации.

Любая транзакция, зафиксированная после вызова метода `BackupAsync`, может быть не включена в резервную копию.  После заполнения локальной папки резервного копирования с помощью платформы (то есть среда выполнения завершает локальное резервное копирование) осуществляется обратный вызов службы резервного копирования.  Этот обратный вызов отвечает за перемещение папки резервного копирования во внешнее расположение, например службу хранилища Azure.

### <a name="restore"></a>восстановление;
Диспетчер надежных состояний позволяет выполнять восстановление из резервной копии с помощью интерфейса API `RestoreAsync`.  
Метод `RestoreAsync` в `RestoreContext` можно вызвать только внутри метода `OnDataLossAsync`.
Возвращенное методом `OnDataLossAsync` логическое значение указывает, удалось ли восстановить состояние службы из внешнего источника.
Если метод `OnDataLossAsync` возвращает значение true, то Service Fabric повторит сборку всех других реплик из этой первичной реплики. Service Fabric гарантирует, что реплики, которые получат вызов `OnDataLossAsync`, сначала получат роль первичных, но им не будет присвоено состояние чтения или записи.
Это означает, что для исполнителей StatefulService метод `RunAsync` будет вызван только после успешного завершения метода `OnDataLossAsync`.
Затем метод `OnDataLossAsync` будет вызван для новой первичной реплики.
Интерфейс API будет постоянно вызываться, пока служба успешно не завершит его (возвратив значение "true" или "false").

`RestoreAsync` сначала удаляет все имеющиеся состояния в первичной реплике, для которой он вызван. После этого диспетчер надежных состояний создает все надежные объекты, существующие в папке резервного копирования. Затем все надежные объекты получают команду выполнить восстановление из контрольных точек в папке резервного копирования. Наконец, диспетчер надежных состояний восстанавливает собственное состояние из записей журнала в папке резервного копирования и выполняет восстановление. В рамках процесса восстановления операции, начиная с отправной точки, записи журнала которых сохранены в папке резервного копирования, воспроизводятся в надежных объектах. Этот шаг обеспечивает согласованность восстановленного состояния.

## <a name="next-steps"></a>Дальнейшие действия
  - [Reliable Collections](service-fabric-work-with-reliable-collections.md)
  - [Get started with Reliable Services](service-fabric-reliable-services-quick-start.md) (Начало работы с Reliable Services)
  - [Уведомления Reliable Services](service-fabric-reliable-services-notifications.md)
  - [Конфигурация Reliable Services](service-fabric-reliable-services-configuration.md)
  - [Справочник разработчика по надежным коллекциям](https://msdn.microsoft.com/library/azure/microsoft.servicefabric.data.collections.aspx)
  - [Periodic backup and restore in Azure Service Fabric](service-fabric-backuprestoreservice-quickstart-azurecluster.md) (Периодическое резервное копирование и восстановление в Azure Service Fabric)

