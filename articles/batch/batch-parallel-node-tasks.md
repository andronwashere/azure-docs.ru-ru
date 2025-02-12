---
title: Параллельное выполнение задач для эффективного использования вычислительных ресурсов пакетной службы Azure | Документация Майкрософт
description: Вы можете увеличить эффективность и снизить стоимость, используя меньшее количество вычислительных узлов. Это возможно благодаря параллельному выполнению задач на каждом узле в пуле пакетной службы Azure.
services: batch
documentationcenter: .net
author: laurenhughes
manager: gwallace
editor: ''
ms.assetid: 538a067c-1f6e-44eb-a92b-8d51c33d3e1a
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: big-compute
ms.date: 04/17/2019
ms.author: lahugh
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: cc6a607da2227ecf9acd6209e31b7aa0ef1c62d8
ms.sourcegitcommit: 4b431e86e47b6feb8ac6b61487f910c17a55d121
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/18/2019
ms.locfileid: "68323365"
---
# <a name="run-tasks-concurrently-to-maximize-usage-of-batch-compute-nodes"></a>Параллельное выполнение задач для эффективного использования вычислительных узлов пакетной службы 

Выполняя несколько задач одновременно на каждом вычислительном узле в пуле пакетной службы Azure, можно увеличить использование ресурсов при меньшем количестве узлов в пуле. Для некоторых рабочих нагрузок это позволит сократить время выполнения заданий и уменьшить затраты.

Во многих случаях выгоднее использовать все ресурсы узла для выполнения одной задачи. Но для нескольких ситуаций совместное использование ресурсов несколькими задачами дает следующие преимущества:

* **уменьшается объем передачи данных** . В этом сценарии можно существенно сократить стоимость передачи данных, копируя общие данные на меньшее количество узлов и выполняя задачи в параллельном режиме на каждом узле. Это особенно актуально, если данные, которые копируются на каждый узел, необходимо передавать между географическими регионами.
* **повышается эффективность использования памяти** . Для эффективной обработки таких нагрузок можно использовать меньшее число более крупных вычислительных узлов с большим объемом памяти. Эти узлы могли бы иметь параллельные задачи, которые выполняются на каждом узле, но каждая задача использовала бы большой объем памяти узлов в разное время.
* **уменьшается ограничение по количеству узлов** . Сейчас существует ограничение в 50 вычислительных узлов для пулов, в которых настроен обмен данными между узлами. Поэтому если каждый узел в таком пуле будет выполнять задачи параллельно, можно будет выполнять больше задач одновременно.
* **Выполняется репликация локального вычислительного кластера**, например, как при первичном переходе в вычислительную среду Azure. Увеличение максимального числа задач на узле позволит точнее скопировать существующую физическую конфигурацию, если текущее локальное решение позволяет выполнять несколько задач на каждом вычислительном узле.

## <a name="example-scenario"></a>Пример сценария
Чтобы продемонстрировать преимущества параллельного выполнения задач, рассмотрим такой пример. Допустим, ваше приложение имеет такие требования к ЦП и памяти, что для его работы достаточно узла размера [Standard\_D1](../cloud-services/cloud-services-sizes-specs.md). Однако для выполнения задачи в заданное время потребуется 1000 таких узлов.

Вместо узлов размера Standard\_D1, каждый из которых содержит одно ядро ЦП, вы можете использовать 16-ядерные узлы [Standard\_D14](../cloud-services/cloud-services-sizes-specs.md), включив на них параллельное выполнение задач. Таким образом, потребуется *в 16 раз меньше узлов* , то есть 63 узла вместо 1000. Кроме того, если каждому узлу требуются большие файлы приложений либо эталонные данные, время выполнения и эффективность будут улучшены, так как данные будут копироваться только на 63 узла.

## <a name="enable-parallel-task-execution"></a>Включение параллельного выполнения задач
Настройка параллельного выполнения задач для вычислительных узлов выполняется на уровне пула. С помощью библиотеки .NET пакетной службы задайте элемент [CloudPool. MaxTasksPerComputeNode][maxtasks_net] property when you create a pool. If you are using the Batch REST API, set the [maxTasksPerNode][rest_addpool] в тексте запроса во время создания пула.

Пакетная служба Azure позволяет устанавливать задачи на каждом узле вплоть до (4X) количества основных узлов. Например, если для пула настроены узлы размера "Большой" (четыре ядра), для параметра `maxTasksPerNode` можно задать значение 16. Однако независимо от того, сколько ядер имеет узел, у вас не может быть более 256 задач для каждого узла. Сведения о количестве ядер для каждого узла см. в статье [Размеры для облачных служб](../cloud-services/cloud-services-sizes-specs.md). Дополнительные сведения об ограничениях службы см. в статье [Квоты и ограничения пакетной службы Azure](batch-quota-limit.md).

> [!TIP]
> При создании [формулы автомасштабирования][enable_autoscaling] для пула `maxTasksPerNode` обязательно примите во внимание значение. Например, если в формуле учитывается параметр `$RunningTasks`, изменение количества задач существенно повлияет на результат ее применения. Дополнительные сведения см. в статье [Автоматическое масштабирование вычислительных узлов в пуле пакетной службы Azure](batch-automatic-scaling.md).
>
>

## <a name="distribution-of-tasks"></a>Распределение задач
Если вычислительные узлы в пуле могут выполнять задачи параллельно, важно указать правила распределения задач между узлами пула.

С помощью свойства [CloudPool. TaskSchedulingPolicy][task_schedule] можно указать, что задачи должны быть равномерно назначены на всех узлах в пуле ("распространение"). Или можно указать, что перед переходом к следующему узлу необходимо назначить максимальное количество задач текущему узлу ("упаковка").

В качестве примера того, как эта функция является полезной, следует рассмотреть пул [стандартных\_узлов D14](../cloud-services/cloud-services-sizes-specs.md) (в примере выше), настроенный с помощью [CloudPool. MaxTasksPerComputeNode][maxtasks_net] value of 16. If the [CloudPool.TaskSchedulingPolicy][task_schedule] , настроен с помощью [ Компутенодефиллтипе][Fill_type] of *Pack*, он позволит максимально эффективно использовать все 16 ядер каждого узла и позволить [пулу автомасштабирования](batch-automatic-scaling.md) удалять неиспользуемые узлы из пула (узлы без назначенных задач). Это снизит использование ресурсов и расходы на них.

## <a name="batch-net-example"></a>Пример использования компонента .NET пакетной службы
Этот [пакет .NET][api_net] API code snippet shows a request to create a pool that contains four nodes with a maximum of four tasks per node. It specifies a task scheduling policy that will fill each node with tasks prior to assigning tasks to another node in the pool. For more information on adding pools by using the Batch .NET API, see [BatchClient.PoolOperations.CreatePool][poolcreate_net].

```csharp
CloudPool pool =
    batchClient.PoolOperations.CreatePool(
        poolId: "mypool",
        targetDedicatedComputeNodes: 4
        virtualMachineSize: "standard_d1_v2",
        cloudServiceConfiguration: new CloudServiceConfiguration(osFamily: "5"));

pool.MaxTasksPerComputeNode = 4;
pool.TaskSchedulingPolicy = new TaskSchedulingPolicy(ComputeNodeFillType.Pack);
pool.Commit();
```

## <a name="batch-rest-example"></a>Пример интерфейса REST API пакетной службы
API snippet shows a request to create a pool that contains two large nodes with a maximum of four tasks per node. For more information on adding pools by using the REST API, see [Add a pool to an account][rest_addpool]Эта [оставшаяся партия пакета][api_rest] .

```json
{
  "odata.metadata":"https://myaccount.myregion.batch.azure.com/$metadata#pools/@Element",
  "id":"mypool",
  "vmSize":"large",
  "cloudServiceConfiguration": {
    "osFamily":"4",
    "targetOSVersion":"*",
  }
  "targetDedicatedComputeNodes":2,
  "maxTasksPerNode":4,
  "enableInterNodeCommunication":true,
}
```

> [!NOTE]
> `maxTasksPerNode` Элемент и свойство [MaxTasksPerComputeNode][maxtasks_net] можно задать только во время создания пула. Их невозможно изменить после создания пула.
>
>

## <a name="code-sample"></a>Пример кода
Свойство [параллелнодетаскс][parallel_tasks_sample] project on GitHub illustrates the use of the [CloudPool.MaxTasksPerComputeNode][maxtasks_net] .

Это C# консольное приложение использует библиотеку [.NET пакетной][api_net] службы для создания пула с одним или несколькими узлами вычислений. Для имитации переменной нагрузки оно выполняет заданное количество задач на этих узлах. Вывод приложения указывает, на каких узлах выполнялась каждая задача. Приложение также предоставляет сводку параметров задания и времени его выполнения. Ниже приводится часть сводки выходных данных примера приложения по двум различным запускам.

```
Nodes: 1
Node size: large
Max tasks per node: 1
Tasks: 32
Duration: 00:30:01.4638023
```

Первый запуск примера приложения показывает, что с одним узлом в пуле и с одной задачей на узел (настройка по умолчанию) на выполнение задания потребовалось более 30 минут.

```
Nodes: 1
Node size: large
Max tasks per node: 4
Tasks: 32
Duration: 00:08:48.2423500
```

Второй запуск примера показывает значительное уменьшение времени выполнения задания. Это вызвано тем, что пул был настроен с четырьмя задачами на узел, что позволяет параллельно выполнять задачи для выполнения задания примерно за четверть исходного времени.

> [!NOTE]
> В приведенных выше сводках длительность выполнения задания не включает время создания пула. Каждое из заданий в примерах выше отправлялось в уже созданные пулы, вычислительные узлы которых на момент отправки находились в состоянии *простоя* .
>
>

## <a name="next-steps"></a>Следующие шаги
### <a name="batch-explorer-heat-map"></a>Тепловая карта обозревателя пакетной службы
[Batch Explorer][batch_labs] — это бесплатный автономный клиентский инструмент с множеством функций для создания, отладки и мониторинга приложений пакетной службы Azure. Batch Explorer содержит компонент *тепловой карты*, который предоставляет визуализацию выполнения задач. При выполнении примера приложения [ParallelTasks][parallel_tasks_sample] можно использовать функцию тепловой карт, чтобы легко визуализировать выполнение параллельных задач на каждом узле.


[api_net]: https://msdn.microsoft.com/library/azure/mt348682.aspx
[api_rest]: https://msdn.microsoft.com/library/azure/dn820158.aspx
[batch_labs]: https://azure.github.io/BatchExplorer/
[cloudpool]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.aspx
[enable_autoscaling]: https://msdn.microsoft.com/library/azure/dn820173.aspx
[fill_type]: https://msdn.microsoft.com/library/microsoft.azure.batch.common.computenodefilltype.aspx
[github_samples]: https://github.com/Azure/azure-batch-samples
[maxtasks_net]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.maxtaskspercomputenode.aspx
[rest_addpool]: https://msdn.microsoft.com/library/azure/dn820174.aspx
[parallel_tasks_sample]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/ParallelTasks
[poolcreate_net]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.createpool.aspx
[task_schedule]: https://msdn.microsoft.com/library/microsoft.azure.batch.cloudpool.taskschedulingpolicy.aspx

