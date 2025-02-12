---
title: Выполнение рабочих нагрузок на экономичных низкоприоритетных виртуальных машинах — пакетная служба Azure | Документация Майкрософт
description: Узнайте, как подготовить низкоприоритетные виртуальные машины для сокращения затрат на рабочие нагрузки пакетной службы Azure.
services: batch
author: mscurrell
manager: gwallace
ms.assetid: dc6ba151-1718-468a-b455-2da549225ab2
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.workload: na
ms.date: 03/19/2018
ms.author: markscu
ms.custom: seodec18
ms.openlocfilehash: 90c8f3779283c23a98bac9d36fde2641c15afafe
ms.sourcegitcommit: 4b431e86e47b6feb8ac6b61487f910c17a55d121
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/18/2019
ms.locfileid: "68323466"
---
# <a name="use-low-priority-vms-with-batch"></a>Использование низкоприоритетных виртуальных машин в пакетной службе

Пакетная служба Azure предлагает низкоприоритетные виртуальные машины для сокращения затрат на рабочие нагрузки пакетной службы. Виртуальные машины с низким приоритетом позволяют использовать новые типы рабочих нагрузок пакетной службы, предоставляя большой объем вычислительных ресурсов по очень низкой цене.
 
Низкоприоритетные виртуальные машины используют избыточные ресурсы в Azure. При указании низкоприоритетных виртуальных машин в пулах пакетная служба Azure может использовать этот избыток при его наличии.
 
Недостатком использования виртуальных машин с низким приоритетом является то, что эти виртуальные машины могут быть недоступны для выделения или могут быть замещены в любое время, в зависимости от доступной емкости. Именно поэтому низкоприоритетные виртуальные машины лучше всего подходят для определенных типов рабочих нагрузок. Используйте низкоприоритетные виртуальные машины для рабочих нагрузок пакетной и асинхронной обработки, где время завершения задания гибкое, а работа распределяется по нескольким виртуальным машинам.
 
Низкоприоритетные виртуальные машины предлагаются по более низкой цене по сравнению с выделенными виртуальными машинами. Сведения о ценах на пакетную службу см. [здесь](https://azure.microsoft.com/pricing/details/batch/).

## <a name="use-cases-for-low-priority-vms"></a>Варианты использования низкоприоритетных виртуальных машин

Какие же рабочие нагрузки можно выполнять на низкоприоритетных виртуальных машинах с учетом их характеристик? Как правило, это рабочие нагрузки пакетной обработки, так как задания разбиваются на несколько параллельных задач или развертываются и распределяются по нескольким виртуальным машинам.

-   Чтобы использовать избыточные ресурсы в Azure по максимуму, вы можете развернуть подходящие задания.

-   Иногда виртуальные машины недоступны или замещаются, что приводит к сокращению емкости для заданий и может привести к прерыванию и повторному запуску задач. Таким образом, во время выполнения задания должны быть гибкими.

-   Прерывание заданий с более продолжительными задачами может привести к более серьезным последствиям. Если реализовать контрольные точки в длительных задачах для сохранения при выполнении, то последствия прерывания менее серьезные. Менее продолжительные задачи, как правило, лучше всего выполняются на виртуальных машинах с низким приоритетом, так как результаты их прерывания не столь серьезные.

-   Длительные задания MPI, использующие несколько виртуальных машин, невыгодно выполнять на низкоприоритетных виртуальных машинах, так как замещение одной виртуальной машины, скорее всего приведет к перезапуску программы.

Ниже перечислены некоторые примеры случаев использования пакетной обработки, подходящих для виртуальных машин с низким приоритетом.

-   **Разработка и тестирование**: при разработке крупномасштабных решений (в частности) можно существенно сэкономить средства. Преимущества доступны для всех видов тестирования, но лучше всего подходит крупномасштабное нагрузочное тестирование и тестирование регрессии.

-   **Дополнение емкости по запросу**: виртуальные машины с низким приоритетом можно использовать для дополнения обычных выделенных виртуальных машин. Если они доступны, задания можно масштабировать, ускорив их и сократив расходы, если нет — базовые выделенные виртуальные машины остаются доступными.

-   **Гибкое время выполнения задания**: если время выполнения задания можно скорректировать, допускается возможное сокращение емкости. Тем не менее при добавлении виртуальных машин с низким приоритетом задания выполняются быстрее за более низкую цену.

Пулы пакетной службы можно настроить для использования низкоприоритетных виртуальных машин несколькими способами в зависимости от гибкости во времени выполнения задания:

-   Низкоприоритетные виртуальные машины можно использовать только в пуле. При этом пакетная служба будет просто восстанавливать замещенные ресурсы, когда они станут доступными. Эта конфигурация является самым экономичным способом выполнения заданий, так как используются только низкоприоритетные виртуальные машины.

-   Низкоприоритетные виртуальные машины можно использовать в сочетании с фиксированными базовыми выделенными виртуальными машинами. Фиксированное число выделенных виртуальных машин обеспечивает наличие ресурсов для выполнения задания.

-   Вы можете использовать динамическое сочетание выделенных и низкоприоритетных виртуальных машин, пользуясь только более дешевыми низкоприоритетными, когда к ним есть доступ, а в другое время заменять их выделенными виртуальными машинами, увеличивая по мере необходимости их масштаб, чтобы обеспечить минимальную емкость для выполнения заданий.

## <a name="batch-support-for-low-priority-vms"></a>Поддержка пакетной службы для виртуальных машин с низким приоритетом

Пакетная служба Azure предоставляет несколько возможностей, которые упрощают использование виртуальных машин с низким приоритетом и позволяют воспользоваться их преимуществами:

-   Пулы пакетной службы могут содержать выделенные и низкоприоритетные виртуальные машины. Число виртуальных машин любого типа можно указать при создании пула или изменить в любое время для имеющегося пула, используя операцию явного изменения размера или автоматическое масштабирование. Отправка заданий и задач не изменяется независимо от типа виртуальной машины в пуле. Вы можете настроить пул так, чтобы он в основном использовал виртуальные машины с низким приоритетом для выполнения заданий как можно дешевле и развертывал выделенные виртуальные машины, если значение емкости падает ниже заданного порогового значения, чтобы выполнение заданий продолжалось.

-   Пулы пакетной службы автоматически ищут целевое количество виртуальных машин с низким приоритетом. Если виртуальные машины замещаются, пакетная служба пытается заменить потерянную емкость и вернуться к целевому показателю.

-   После прерывания задач пакетная служба обнаруживает и автоматически помещает в очередь задания для повторного запуска.

-   Низкоприоритетные виртуальные машины имеют отдельную квоту на виртуальные ЦП, которая отличается от квоты выделенных виртуальных машин. 
    Квота для низкоприоритетных виртуальных машин выше, чем для выделенных виртуальных машин, так как они меньше стоят. Дополнительные сведения см. в разделе о [квотах и ограничениях пакетной службы](batch-quota-limit.md#resource-quotas).    

> [!NOTE]
> Сейчас виртуальные машины с низким приоритетом не поддерживаются для учетных записей пакетной службы, созданных в [режиме подписки пользователя](batch-api-basics.md#account).
>

## <a name="create-and-update-pools"></a>Создание и обновление пулов

Пул пакетной службы может содержать выделенные и низкоприоритетные виртуальные машины (вычислительные узлы). Вы можете задать целевое количество вычислительных узлов и для тех, и для других виртуальных машин. Оно указывает на количество виртуальных машин, которые должны находиться в пуле.

Пример создания пула с использованием виртуальных машин облачной службы Azure с целевым показателем в 5 выделенных и 20 низкоприоритетных виртуальных машин:

```csharp
CloudPool pool = batchClient.PoolOperations.CreatePool(
    poolId: "cspool",
    targetDedicatedComputeNodes: 5,
    targetLowPriorityComputeNodes: 20,
    virtualMachineSize: "Standard_D2_v2",
    cloudServiceConfiguration: new CloudServiceConfiguration(osFamily: "5") // WS 2016
);
```

Пример создания пула с использованием виртуальных машин Azure (в данном случае виртуальных машин Linux) с целевым показателем в 5 выделенных и 20 низкоприоритетных виртуальных машин:

```csharp
ImageReference imageRef = new ImageReference(
    publisher: "Canonical",
    offer: "UbuntuServer",
    sku: "16.04-LTS",
    version: "latest");

// Create the pool
VirtualMachineConfiguration virtualMachineConfiguration =
    new VirtualMachineConfiguration("batch.node.ubuntu 16.04", imageRef);

pool = batchClient.PoolOperations.CreatePool(
    poolId: "vmpool",
    targetDedicatedComputeNodes: 5,
    targetLowPriorityComputeNodes: 20,
    virtualMachineSize: "Standard_D2_v2",
    virtualMachineConfiguration: virtualMachineConfiguration);
```

Вы можете получить текущее количество узлов для выделенных и низкоприоритетных виртуальных машин:

```csharp
int? numDedicated = pool1.CurrentDedicatedComputeNodes;
int? numLowPri = pool1.CurrentLowPriorityComputeNodes;
```

В узлах пула есть свойство, указывающее на принадлежность узла (выделенная или низкоприоритетная виртуальная машина):

```csharp
bool? isNodeDedicated = poolNode.IsDedicated;
```

После замещения одного или нескольких узлов в пуле операция выведения списка узлов в пуле будет по-прежнему возвращать эти узлы. Текущее число узлов с низким приоритетом не изменится, но их состояние изменится на **Preempted** (Замещено). Пакетная служба пытается найти виртуальные машины на замену. Если она находит их, состояние узлов сменится на **Создание** и **Запуск** перед тем, как они станут доступными для выполнения задачи, как и новые узлы.

## <a name="scale-a-pool-containing-low-priority-vms"></a>Масштабирование пула низкоприоритетных виртуальных машин

Как и пулы, состоящие исключительно из выделенных виртуальных машин, можно масштабировать пулы низкоприоритетных виртуальных машин, вызвав метод "Изменить размер" или использовав автомасштабирование.

Операция изменения размера пула принимает второй необязательный параметр, обновляющий значение **targetLowPriorityNodes**:

```csharp
pool.Resize(targetDedicatedComputeNodes: 0, targetLowPriorityComputeNodes: 25);
```

Формула автомасштабирования пула реализует поддержку низкоприоритетных виртуальных машин следующим образом:

-   Вы можете получить или задать значение переменной, определяемой службой ( **$TargetLowPriorityNodes**).

-   Вы можете получить значение переменной, определяемой службой ( **$CurrentLowPriorityNodes**).

-   Вы можете получить значение переменной, определяемой службой ( **$PreemptedNodeCount**). 
    Эта переменная возвращает количество узлов в состоянии "Отложено" и позволяет масштабировать число выделенных узлов в зависимости от числа недоступных замещенных узлов.

## <a name="jobs-and-tasks"></a>Задания и задачи

Заданиям и задачам почти не требуется дополнительная настройка узлов с низким приоритетом. Они поддерживают следующее:

-   Свойство JobManagerTask задания имеет новое свойство **AllowLowPriorityNode**. 
    Если это свойство имеет значение true, задачу диспетчера заданий можно запланировать на выделенном или низкоприоритетном узле. Если это свойство имеет значение false, задача диспетчера заданий планируется только на выделенном узле.

-   Для приложения задачи доступна [переменная среда](batch-compute-node-environment-variables.md), позволяющая определить узел, на котором она выполняется (низкоприоритетный или выделенный). Переменная среды — AZ_BATCH_NODE_IS_DEDICATED.

## <a name="handling-preemption"></a>Обработка замещения

Иногда виртуальные машины могут замещаться. В этом случае пакетная служба делает следующее:

-   Для замещенных виртуальных машин устанавливается состояние **Замещено**.
-   Если на узле замещенных виртуальных машинах выполнялись задачи, они помещаются в очередь и перезапускаются.
-   Виртуальная машина удаляется, что приводит к потере всех ее локальных данных в хранилище.
-   Пул постоянно будет пытаться подключиться к целевым доступным низкоприоритетным узлам. При обнаружении замены идентификаторы узлов сохраняются, но узлы повторно инициализируются и их состояние меняется на **Создание** и **Запуск** перед тем, как они станут доступными для планирования задач.
-   Счетчики замещения доступны в качестве метрики на портале Azure.

## <a name="metrics"></a>Метрики

На [портале Azure](https://portal.azure.com) доступны новые метрики для узлов с низким приоритетом. Это такие метрики:

- Low-Priority Node Count
- Low-Priority Core Count; 
- Preempted Node Count

Для просмотра метрик на портале Azure сделайте следующее.

1. Перейдите к своей учетной записи пакетной службы на портале и просмотрите параметры этой учетной записи.
2. В разделе **Мониторинг** щелкните **Метрики**.
3. Выберите требуемые метрики в списке **Доступные метрики**.

![Метрики для узлов с низким приоритетом](media/batch-low-pri-vms/low-pri-metrics.png)

## <a name="next-steps"></a>Следующие шаги

* Информация, необходимая для тех, кто готовится использовать пакетную службу, доступна в статье [Обзор функций пакетной службы для разработчиков](batch-api-basics.md). Эта статья содержит дополнительные подробные сведения о таких ресурсах пакетной службы, как пулы, узлы, задания, задачи и многие функции API, которые можно использовать при создании приложения пакетной службы.
* См. дополнительные сведения об [API-интерфейсах и средствах пакетной службы](batch-apis-tools.md) для сборки решений пакетной службы.
