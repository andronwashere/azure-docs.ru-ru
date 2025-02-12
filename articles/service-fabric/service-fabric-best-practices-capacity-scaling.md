---
title: Планирование емкости и масштабирование для Azure Service Fabric | Документация Майкрософт
description: Рекомендации по планированию и масштабированию кластеров и приложений Service Fabric.
services: service-fabric
documentationcenter: .net
author: peterpogorski
manager: chackdan
editor: ''
ms.assetid: 19ca51e8-69b9-4952-b4b5-4bf04cded217
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 04/25/2019
ms.author: pepogors
ms.openlocfilehash: d4daa7ae9c7e58c1949dfbe4427a154c389100d4
ms.sourcegitcommit: e72073911f7635cdae6b75066b0a88ce00b9053b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/19/2019
ms.locfileid: "68348370"
---
# <a name="capacity-planning-and-scaling-for-azure-service-fabric"></a>Планирование емкости и масштабирование для Azure Service Fabric

Перед созданием кластера Azure Service Fabric или масштабирования ресурсов вычислений, в которых размещается кластер, важно спланировать емкость. См. дополнительные сведения о [планировании загрузки кластера Service Fabric](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-capacity). Дополнительные рекомендации по масштабируемости кластера см. в статье [Service Fabric рекомендации](https://docs.microsoft.com/azure/architecture/reference-architectures/microservices/service-fabric#scalability-considerations)по масштабируемости.

В дополнение к рассмотрению типа узла и характеристик кластера следует рассчитывать, что операции масштабирования должны выполняться дольше, чем за час, для рабочей среды. Это справедливо, независимо от числа добавляемых виртуальных машин.

## <a name="autoscaling"></a>Автомасштабирование
Операции масштабирования следует выполнять с помощью шаблонов Azure Resource Manager, поскольку рекомендуется рассматривать [конфигурации ресурсов как код]( https://docs.microsoft.com/azure/service-fabric/service-fabric-best-practices-infrastructure-as-code). 

Использование автоматического масштабирования с помощью масштабируемых наборов виртуальных машин сделает неточную версию диспетчер ресурсов шаблона неточное определение количества экземпляров для масштабируемых наборов виртуальных машин. Неверное определение повышает риск того, что будущие развертывания приведут к непредвиденным операциям масштабирования. Как правило, автоматическое масштабирование следует использовать, если:

* Если развертывание шаблонов Resource Manager с объявленной емкостью не подходит для вашего варианта использования.
     
   В дополнение к ручному масштабированию можно настроить [конвейер непрерывной интеграции и доставки в Azure DevOps Services с помощью проектов развертывания группы ресурсов Azure](https://docs.microsoft.com/azure/vs-azure-tools-resource-groups-ci-in-vsts). Этот конвейер обычно запускается приложением логики, которое использует метрики производительности виртуальной машины, запрашиваемые из [Azure Monitor REST API](https://docs.microsoft.com/azure/azure-monitor/platform/rest-api-walkthrough). Конвейер эффективно масштабируется на основе любых нужных метрик, а оптимизация для шаблонов диспетчер ресурсов.
* За один раз необходимо горизонтальное масштабирование только одного узла масштабируемого набора виртуальных машин.
   
   Для горизонтального масштабирования по трем или более узлам необходимо [масштабировать кластер Service Fabric, добавив масштабируемый набор виртуальных машин](virtual-machine-scale-set-scale-node-type-scale-out.md). Это самый надежный способ масштабирования и масштабирования масштабируемых наборов виртуальных машин по горизонтали, по одному узлу за раз.
* У вас есть серебряная надежность или выше для кластера Service Fabric, а также Серебряная устойчивость или выше в любом масштабе, где настраиваются правила автоматического масштабирования.
  
   Минимальная емкость для правил автоматического масштабирования должна быть больше или равна пяти экземплярам виртуальных машин. Оно также должно быть больше или равно минимальному уровню надежности для типа первичного узла.

> [!NOTE]
> Service Fabric с отслеживанием состояния Service Fabric:/System/Инфаструктуресервице/< NODE_TYPE_NAME > выполняется на каждом типе узла, который имеет серебряную или более высокую устойчивость. Это единственная системная служба, которая поддерживается в Azure на любом из типов узлов кластера.

## <a name="vertical-scaling-considerations"></a>Рекомендации по вертикальному масштабированию

[Вертикальное масштабирование](https://docs.microsoft.com/azure/service-fabric/virtual-machine-scale-set-scale-node-type-scale-out) типа узла в Service Fabric Azure требует ряда шагов и рекомендаций. Пример:

* Масштабируемый кластер должен быть работоспособным. В противном случае вы будете дестабилизировать кластер.
* Для всех Service Fabric типов узлов кластера, на которых размещены службы с отслеживанием состояния, требуется уровень устойчивости или выше.

> [!NOTE]
> Тип первичного узла, на котором размещены системные службы Service Fabric, должен быть серебряным уровнем устойчивости или большим. После того как вы включите серебряную устойчивость, операции кластера, такие как обновление, добавление или удаление узлов и так далее, будут выполняться медленнее, так как система оптимизирует безопасность данных по скорости операций.

Вертикальное масштабирование масштабируемого набора виртуальных машин является обратимой операцией. Вместо этого необходимо горизонтально масштабировать кластер, добавив новый масштабируемый набор с требуемым номером SKU. Затем перенесите службы в нужный номер SKU, чтобы выполнить операцию безопасного вертикального масштабирования. Изменение номера SKU ресурса масштабируемого набора виртуальных машин является обратимой операцией, так как она перестраивает узлы, удаляя все локально сохраняемое состояние.

Кластер использует [Свойства узла Service Fabric и ограничения на размещение](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-resource-manager-cluster-description#node-properties-and-placement-constraints) , чтобы решить, где следует размещать службы приложения. При вертикальном масштабировании типа первичного узла объявите одинаковые значения свойств для `"nodeTypeRef"`. Эти значения можно найти в расширении Service Fabric для масштабируемых наборов виртуальных машин. 

В следующем фрагменте шаблона диспетчер ресурсов отображаются свойства, которые вы объявите. Он имеет то же значение для вновь подготовленных масштабируемых наборов, на которые выполняется масштабирование, и поддерживается только в качестве временной службы с отслеживанием состояния для кластера.

```json
"settings": {
   "nodeTypeRef": ["[parameters('primaryNodetypeName')]"]
}
```

> [!NOTE]
> Не оставляйте кластер, работающий с несколькими масштабируемыми наборами, `nodeTypeRef` которые используют одно и то же значение свойства дольше, чем требуется для успешного выполнения операции вертикального масштабирования.
>
> Всегда проверяйте операции в тестовых средах, прежде чем выполнять изменения в рабочей среде. По умолчанию Service Fabric системные службы кластера имеют ограничение на размещение только для целевого типа первичного узла.

Объявив свойства узла и ограничения на размещение, выполните следующие шаги поочередно для каждого экземпляра виртуальной машины. Это позволяет корректно завершить работу системных служб (и служб с отслеживанием состояния) на удаляемом экземпляре виртуальной машины, так как новые реплики создаются в других местах.

1. В PowerShell выполните команду `Disable-ServiceFabricNode` с намерением `RemoveNode` отключить узел, который вы собираетесь удалить. Удалите тип узла с наибольшим номером. Например, если имеется кластер из шести узлов, удалите экземпляр виртуальной машины "MyNodeType_5".
2. Выполните `Get-ServiceFabricNode` и убедитесь, что этот узел перешел в отключенное состояние. Если это не так, подождите, пока узел не будет отключен. Для каждого узла может потребоваться несколько часов. Не продолжайте процесс, пока узел не перейдет в отключенное состояние.
3. Сократите количество виртуальных машин на единицу в этом типе узла. Будет удален самый верхний экземпляр виртуальной машины.
4. При необходимости повторите шаги 1–3, но никогда не следует уменьшать число экземпляров в типах первичных узлов до значения меньше гарантируемого уровнем надежности. См. [рекомендации по планированию загрузки кластера Service Fabric](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-capacity).
5. После того как все виртуальные машины будут удалены (представленные ниже), структура:/System/Инфраструктуресервице/[имя узла] отобразит состояние ошибки. Затем можно обновить ресурс кластера, чтобы удалить тип узла. Можно либо использовать развертывание шаблона ARM, либо изменить ресурс кластера с помощью [Azure Resource Manager](https://resources.azure.com). Это приведет к запуску обновления кластера, которое удалит службу Fabric:/System/Инфраструктуресервице/[тип узла], которая находится в состоянии ошибки.
 6. После этого вы по-прежнему можете удалить для vmscaleset, но в этом случае узлы будут отображаться как "вниз" из Service Fabric Explorer представления. Последним шагом было бы очистить их с помощью `Remove-ServiceFabricNodeState` команды.

## <a name="horizontal-scaling"></a>горизонтальное масштабирование;

Горизонтальное масштабирование можно выполнять либо [вручную](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-scale-up-down) , либо [программно](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-programmatic-scaling).

> [!NOTE]
> При масштабировании типа узла, имеющего серебряную или золотую устойчивость, масштабирование будет выполняться очень долго.

### <a name="scaling-out"></a>Развертывание

Развертывание Service Fabric кластера путем увеличения числа экземпляров для конкретного масштабируемого набора виртуальных машин. Можно масштабировать программным путем с помощью `AzureClient` и идентификатора для нужного масштабируемого набора, чтобы увеличить емкость.

```csharp
var scaleSet = AzureClient.VirtualMachineScaleSets.GetById(ScaleSetId);
var newCapacity = (int)Math.Min(MaximumNodeCount, scaleSet.Capacity + 1);
scaleSet.Update().WithCapacity(newCapacity).Apply(); 
```

Чтобы масштабировать вручную, обновите емкость в свойстве SKU требуемого ресурса [масштабируемого набора виртуальных машин](https://docs.microsoft.com/rest/api/compute/virtualmachinescalesets/createorupdate#virtualmachinescalesetosprofile) .

```json
"sku": {
    "name": "[parameters('vmNodeType0Size')]",
    "capacity": "[parameters('nt0InstanceCount')]",
    "tier": "Standard"
}
```

### <a name="scaling-in"></a>Уменьшение масштаба

Уменьшение масштаба нужно выполнять осторожнее, чем увеличение. Пример:

* Service Fabric системные службы выполняются в кластере в типе первичного узла. Никогда не уменьшайте число экземпляров узлов для этого типа ниже значения, которое гарантирует соответствующий уровень надежности. 
* Для службы с отслеживанием состояния требуется определенное количество узлов, которые всегда поддерживают доступность и сохраняют состояние службы. Как минимум, требуется число узлов, равное количеству целевых реплик, заданных для раздела или службы.

Чтобы уменьшить масштаб вручную, выполните следующие действия.

1. В PowerShell выполните команду `Disable-ServiceFabricNode` с намерением `RemoveNode` отключить узел, который вы собираетесь удалить. Удалите тип узла с наибольшим номером. Например, если имеется кластер из шести узлов, удалите экземпляр виртуальной машины "MyNodeType_5".
2. Выполните `Get-ServiceFabricNode` и убедитесь, что этот узел перешел в отключенное состояние. Если это не так, подождите, пока узел не будет отключен. Для каждого узла может потребоваться несколько часов. Не продолжайте процесс, пока узел не перейдет в отключенное состояние.
3. Сократите количество виртуальных машин на единицу в этом типе узла. Будет удален самый верхний экземпляр виртуальной машины.
4. Повторяйте шаги 1 – 3 по мере необходимости, пока не будет выполнена подготавливается Требуемая емкость. Никогда не уменьшайте число экземпляров в узлах основного типа ниже значения, которое гарантирует соответствующий уровень надежности. См. [рекомендации по планированию загрузки кластера Service Fabric](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-capacity).

Чтобы выполнить масштабирование вручную, обновите емкость в свойстве SKU требуемого ресурса [масштабируемого набора виртуальных машин](https://docs.microsoft.com/rest/api/compute/virtualmachinescalesets/createorupdate#virtualmachinescalesetosprofile) .

```json
"sku": {
    "name": "[parameters('vmNodeType0Size')]",
    "capacity": "[parameters('nt0InstanceCount')]",
    "tier": "Standard"
}
```

Необходимо подготовить узел к завершению работы для программного масштабирования. Найдите узел, который нужно удалить (узел с наибольшим экземпляром). Пример:

```csharp
using (var client = new FabricClient())
{
    var mostRecentLiveNode = (await client.QueryManager.GetNodeListAsync())
        .Where(n => n.NodeType.Equals(NodeTypeToScale, StringComparison.OrdinalIgnoreCase))
        .Where(n => n.NodeStatus == System.Fabric.Query.NodeStatus.Up)
        .OrderByDescending(n =>
        {
            var instanceIdIndex = n.NodeName.LastIndexOf("_");
            var instanceIdString = n.NodeName.Substring(instanceIdIndex + 1);
            return int.Parse(instanceIdString);
        })
        .FirstOrDefault();
```

Отключите и удалите узел, используя тот же `FabricClient` экземпляр (`client` в данном случае) и экземпляр узла (`instanceIdString` в данном случае), который использовался в предыдущем коде:

```csharp
var scaleSet = AzureClient.VirtualMachineScaleSets.GetById(ScaleSetId);

// Remove the node from the Service Fabric cluster
ServiceEventSource.Current.ServiceMessage(Context, $"Disabling node {mostRecentLiveNode.NodeName}");
await client.ClusterManager.DeactivateNodeAsync(mostRecentLiveNode.NodeName, NodeDeactivationIntent.RemoveNode);

// Wait (up to a timeout) for the node to gracefully shut down
var timeout = TimeSpan.FromMinutes(5);
var waitStart = DateTime.Now;
while ((mostRecentLiveNode.NodeStatus == System.Fabric.Query.NodeStatus.Up || mostRecentLiveNode.NodeStatus == System.Fabric.Query.NodeStatus.Disabling) &&
        DateTime.Now - waitStart < timeout)
{
    mostRecentLiveNode = (await client.QueryManager.GetNodeListAsync()).FirstOrDefault(n => n.NodeName == mostRecentLiveNode.NodeName);
    await Task.Delay(10 * 1000);
}

// Decrement virtual machine scale set capacity
var newCapacity = (int)Math.Max(MinimumNodeCount, scaleSet.Capacity - 1); // Check min count 

scaleSet.Update().WithCapacity(newCapacity).Apply();
```

> [!NOTE]
> При уменьшении масштаба кластера вы увидите, что удаленный экземпляр узла или виртуальной машины отображается в неработоспособном состоянии Service Fabric Explorer. Описание этого поведения см. [в разделе варианты поведения, которые могут возникнуть в Service Fabric Explorer](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-scale-up-down#behaviors-you-may-observe-in-service-fabric-explorer). Можно выполнить следующие действия:
> * Вызовите [команду Remove-ServiceFabricNodeState](https://docs.microsoft.com/powershell/module/servicefabric/remove-servicefabricnodestate?view=azureservicefabricps) с соответствующим именем узла.
> * Разверните в кластере [вспомогательное приложение автомасштабирования Service Fabric](https://github.com/Azure/service-fabric-autoscale-helper/) . Это приложение гарантирует, что масштабируемые узлы будут удалены из Service Fabric Explorer.

## <a name="reliability-levels"></a>Уровни надежности

[Уровень надежности](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-capacity) — это свойство ресурса кластера Service Fabric. Его нельзя настроить по-разному для отдельных типов узлов. Он определяет коэффициент репликации для системных служб кластера и применяется на уровне ресурса кластера. 

Уровень надежности определяет минимальное количество узлов для узла основного типа. Уровень надежности может принимать следующие значения:

* Корпоратив Запускает системные службы с количеством целевых реплик, равным семи и девять начальных узлов.
* Цветом Запускает системные службы с целевым набором целевых реплик, количеством семи и семи начальных узлов.
* Зеркаль Запускает системные службы с числом целевых реплик, равным пяти и пять начальных узлов.
* Bronze Запускает системные службы с целевым набором целевых реплик, количеством трех и трех начальных узлов.

Мы рекомендуем использовать уровень надежности не ниже Silver.

Уровень надежности задается в разделе свойств для [ресурса Microsoft.ServiceFabric/clusters](https://docs.microsoft.com/azure/templates/microsoft.servicefabric/2018-02-01/clusters), как показано ниже:

```json
"properties":{
    "reliabilityLevel": "Silver"
}
```

## <a name="durability-levels"></a>Уровни устойчивости

> [!WARNING]
> Типы узлов, выполняющиеся с устойчивостью Bronze, _не получают привилегий_. Задания инфраструктуры, влияющие на рабочие нагрузки без отслеживания состояния, не будут остановлены или отложены, что может повлиять на рабочие нагрузки. 
>
> Используйте уровень устойчивости Bronze только для тех типов узлов, на которых выполняются рабочие нагрузки без учета состояния. Для рабочих нагрузок в рабочей среде используйте серебряную или более позднюю версию, чтобы обеспечить согласованность состояния. Выберите уровень надежности, следуя рекомендациям в [документации по планированию ресурсов](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-capacity).

Уровень устойчивости настраивается для двух ресурсов. Один из них является профилем расширения для [ресурса масштабируемого набора виртуальных машин](https://docs.microsoft.com/rest/api/compute/virtualmachinescalesets/createorupdate#virtualmachinescalesetosprofile):

```json
"extensionProfile": {
    "extensions":          {
        "name": "[concat('ServiceFabricNodeVmExt','_vmNodeType0Name')]",
        "properties": {
            "settings": {
                "durabilityLevel": "Bronze"
            }
        }
    }
}
```

Другой ресурс находится `nodeTypes` в [ресурсе Microsoft. ServiceFabric/Clusters](https://docs.microsoft.com/azure/templates/microsoft.servicefabric/2018-02-01/clusters): 

```json
"nodeTypes": [
    {
        "name": "[variables('vmNodeType0Name')]",
        "durabilityLevel": "Bronze"
    }
]
```

## <a name="next-steps"></a>Следующие шаги

* Создание кластера на основе виртуальных машин или компьютеров под управлением Windows Server: [Создание изолированного кластера под управлением Windows Server](service-fabric-cluster-creation-for-windows-server.md).
* Создание кластера на основе виртуальных машин или компьютеров под управлением Linux: [Создание кластера Service Fabric в Azure с помощью портала Azure](service-fabric-cluster-creation-via-portal.md).
* Узнайте о [вариантах поддержки Service Fabric](service-fabric-support.md).

[Image1]: ./media/service-fabric-best-practices/generate-common-name-cert-portal.png
