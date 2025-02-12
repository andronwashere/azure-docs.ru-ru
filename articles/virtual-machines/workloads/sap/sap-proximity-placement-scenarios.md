---
title: Группы размещения службы "близость" Azure для оптимальной задержки в сети с приложениями SAP | Документация Майкрософт
description: Описание сценариев развертывания SAP с группами размещения с учетом расположения в Azure
services: virtual-machines-linux,virtual-machines-windows
documentationcenter: ''
author: msjuergent
manager: bburns
editor: ''
tags: azure-resource-manager
keywords: ''
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 07/15/2019
ms.author: juergent
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: f97cce2d1a8b2de5634215629ddb997dc8f7196a
ms.sourcegitcommit: b2db98f55785ff920140f117bfc01f1177c7f7e2
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/16/2019
ms.locfileid: "68235201"
---
# <a name="azure-proximity-placement-groups-for-optimal-network-latency-with-sap-applications"></a>Группа размещения близкого взаимодействия Azure для оптимизации сетевой задержки с использованием приложений SAP
Приложения SAP на основе архитектуры SAP NetWeaver или SAP S/4HANA чувствительны к задержке в сети между уровнем приложений SAP и уровнем базы данных SAP. Причина этого чувствительности этих архитектур заключается в том, что основная часть бизнес-логики выполняется на уровне приложения. В результате выполнения бизнес-логики уровень приложений SAP выдает запросы на уровень базы данных с высокой частотой тысяч и десятков тысяч в секунду. В большинстве случаев характер этих запросов прост. И часто могут выполняться на уровне базы данных менее 500 микросекунд или даже меньше. Время, затраченное сетью на отправку такого запроса с уровня приложения на уровень базы данных и получение результирующего набора от уровня базы данных, оказывает значительное влияние на время, затрачиваемое на выполнение бизнес-процессов. Эта чувствительность к задержке в сети — это причина, по которой необходимо потратить время на проекты развертывания SAP для достижения оптимальной задержки в сети. В [заметке SAP #1100926 — часто задаваемые вопросы: Производительность](https://launchpad.support.sap.com/#/notes/1100926/E)сети. SAP опубликовал некоторые рекомендации по классификации сетевой задержки.

На стороне "один" во многих регионах Azure количество центров обработки данных увеличилось, а также инициируется появлением Зоны доступности. С другой стороны, клиенты, особенно для высокопроизводительных систем SAP, использовали более Специальные номера SKU виртуальных машин из семейства серии M или крупных экземпляров HANA. Типы виртуальных машин Azure, которые не находятся во всех центрах обработки данных в определенном регионе Azure. В результате этих двух тенденциих клиентов возникают случаи, когда задержка в сети не находилась в оптимальном диапазоне, и в некоторых случаях она привела к неоптимальной производительности систем SAP.

Чтобы предотвратить такую проблему, Azure предлагает конструкцию, которая называется [группой размещения в Azure для близости](https://docs.microsoft.com/azure/virtual-machines/linux/co-location). Эта новая функциональная возможность уже используется для развертывания различных систем SAP. Просмотрите статью, на которую ссылаются ограничения групп размещения. В этой статье рассматриваются различные сценарии SAP, в которых можно использовать группы размещения с близкой близости к Azure или использовать их.

## <a name="what-are-proximity-placement-groups"></a>Что такое группы размещения близкого расположения 
Группа размещения с учетом расположения Azure — это логическая конструкция, которая на этапе определения привязана к региону Azure и группе ресурсов Azure. При развертывании виртуальных машин ссылка на группу размещения с учетом расположения находится в следующих случаях:

- Первая развернутая виртуальная машина Azure для сопоставления в центре обработки данных. Первую виртуальную машину можно рассматривать как виртуальную машину привязки, которая развертывается в центре обработки данных на основе алгоритмов распределения Azure в конечном итоге вместе с определениями пользователей для определенной зоны доступности Azure.
- Всеми последующими виртуальными машинами, развернутыми с помощью ссылки на группу размещения, размещают все последующие развернутые виртуальные машины Azure в том же центре обработки данных, в котором была установлена первая виртуальная машина.

> [!NOTE]
> Если не развернуто оборудование узла, которое может запустить конкретный тип виртуальной машины в том же центре обработки данных, что и первая виртуальная машина, развертывание требуемого типа виртуальной машины не будет выполнено и завершится сообщением об ошибке. Это могут быть случаи, когда между виртуальными машинами, такими как виртуальные машины с графическими процессорами или типами виртуальных машин HPC, должны быть центрированы, например, виртуальная машина серии M, развернутая как первый тип виртуальной машины.

Одной [группе ресурсов Azure](https://docs.microsoft.com/azure/azure-resource-manager/manage-resources-portal) может быть назначено несколько групп размещения с учетом расположения. Однако одну группу размещения с учетом расположения можно назначить только одной группе ресурсов Azure.

С помощью групп размещения с учетом расположения вы должны иметь в виду следующее:

- При максимально оптимальной производительности системы SAP и ограничении одного центра обработки данных Azure для этой системы с помощью групп размещения с учетом расположения, возможно, вы не сможете объединить все типы семейств виртуальных машин в рамках такой группы размещения близости. Причина в том, что определенное оборудование узла, необходимое для монопольного выполнения определенного типа виртуальной машины, может отсутствовать в центре обработки данных, для которого была развернута виртуальная машина привязки для группы размещения.
- В жизненном цикле такой системы SAP можно было принудительно переместить систему в другой центр обработки данных. Такое перемещение может быть принудительно применено в случаях, когда вы решили, что для уровня масштабируемости СУБД HANA необходимо переместить с четырех узлов на 16 узлов. Но больше не хватает емкости, чтобы получить дополнительные 12 виртуальных машин типа, которые уже использовались в том же центре обработки данных.
- Из-за списания оборудования Корпорация Майкрософт может создать емкость для типов виртуальных машин, которые использовались в другом центре обработки данных, а не в том же центре обработки данных. Такое событие может означать перемещение всех виртуальных машин группы размещения с учетом расположения в другой центр обработки данных.


## <a name="azure-proximity-placement-groups-with-sap-systems-using-azure-vms-exclusively"></a>Группы размещения в Azure с системами SAP с монопольным использованием виртуальных машин Azure
В большинстве случаев при развертывании систем SAP NetWeaver и S/4HANA в Azure [крупные экземпляры Hana](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-architecture)не используются. Для развертываний таких систем важно обеспечить наиболее оптимальную производительность между уровнем приложений SAP и уровнем СУБД. Для этого необходимо определить группу размещения для службы "близость" Azure только для этой системы. В большинстве клиентских развертываний клиенты решили создать отдельную [группу ресурсов Azure](https://docs.microsoft.com/azure/azure-resource-manager/manage-resources-portal) для систем SAP. В таком случае будет соотношение 1:1 между, например, группами рабочих ресурсов системы ERP и группой размещения близости. В некоторых других случаях развертывания клиенты упорядочивают группы ресурсов по горизонтали и собирают все рабочие системы в одной группе ресурсов. В этом случае у вас будет одна связь между группой ресурсов для производственных систем SAP и несколькими группами размещения в производственной системе SAP ERP, SAP BW и т. д. Следует избегать объединения нескольких производственных или даже непроизводственных систем SAP в одной группе размещения с учетом расположения. В исключениях, где небольшое количество систем SAP или системы SAP и некоторые окружающие приложения должны иметь сетевые подключения с низкой задержкой, можно рассмотреть возможность перемещения этих систем в одну группу размещения. Причина такой рекомендации заключается в том, что чем больше систем вы группируюте в группу размещения с учетом расположения, тем выше вероятность:

- Для этого требуется тип виртуальной машины, который не может выполняться в конкретном центре обработки данных, к которому привязана группа размещения.
- Ресурсы некоторых неосновных виртуальных машин, таких как серия M, в конечном итоге могут быть не выполнены, когда вы запрашиваете дополнительные программное обеспечение в существующей группе размещения с течением времени.

Оптимальное использование, как описано, будет выглядеть следующим образом:

![Группы плацемен близкого взаимодействия для всех виртуальных машин Azure](./media/sap-proximity-placement-scenarios/ppg-for-all-azure-vms.png)

В этом случае одиночные системы SAP группируются в одну группу ресурсов, каждая из которых имеет одну группу размещения с учетом расположения. Зависимости от того, используете ли вы масштабируемые конфигурации HANA или масштабируемость СУБД, нет.


## <a name="azure-proximity-placement-groups-and-hana-large-instances"></a>Группы размещения службы близости к Azure и крупные экземпляры HANA
В случаях, когда некоторые системы SAP используют [крупные экземпляры Hana](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-architecture) в качестве прикладного уровня, серьезные улучшения задержки сети между единицей крупных экземпляров Hana и виртуальными машинами Azure можно достичь при использовании единиц крупных экземпляров Hana, которые были развертывается в [строках и метках редакции 4](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-network-architecture#networking-architecture-for-hana-large-instance). Одним из усовершенствований является то, что крупные экземпляры HANA, как они были развернуты, получают уже развернутую группу размещения с близкой нагрузкой. Вы можете использовать эту группу размещения с учетом расположения для развертывания виртуальных машин уровня приложения. В результате эти виртуальные машины будут развернуты в том же центре обработки данных, где размещена единица крупного экземпляра HANA.

Чтобы определить, развернута ли единица крупного экземпляра HANA в метке или строке редакции версии 4, ознакомьтесь со статьей управление крупными экземплярами [Azure Hana с помощью портал Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-li-portal#look-at-attributes-of-single-hli-unit). В обзоре атрибутов единицы крупного экземпляра HANA вы также можете узнать имя группы размещения с учетом расположения, которое было создано во время развертывания для единицы крупного экземпляра HANA. Имя, отображаемое в обзоре атрибутов, — это имя группы размещения с учетом расположения, которое следует использовать для развертывания виртуальных машин уровня приложения в.

В отличие от систем SAP, использующих только виртуальные машины Azure, решение о том, сколько [групп ресурсов Azure](https://docs.microsoft.com/azure/azure-resource-manager/manage-resources-portal) следует использовать, будет извлекаться от вас к степени при использовании крупных экземпляров Hana. Все крупные экземпляры HANA [клиента крупного экземпляра Hana](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-know-terms) группируются в одну группу ресурсов Azure, как описано [здесь](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-li-portal#display-of-hana-large-instance-units-in-the-azure-portal). Если вам не требуется разделять развертывание в разных клиентах, например производственных и непроизводственных, или определенных систем, все единицы крупных экземпляров HANA будут развернуты в одном клиенте крупного экземпляра HANA, который снова имеет отношение 1:1 с Azure. Группа ресурсов. В то время как для всех отдельных единиц будет определена отдельная группа размещения с учетом расположения. 

В результате группирование между группами ресурсов Azure и группами размещения с учетом расположения для одного клиента будет выглядеть следующим образом:

![Группы размещения с учетом расположения для всех виртуальных машин Azure](./media/sap-proximity-placement-scenarios/ppg-for-hana-large-instance-units.png)


## <a name="short-example-of-deploying-with-azure-proximity-placement-groups"></a>Краткий пример развертывания с помощью групп размещения с учетом расположения Azure
Чтобы продемонстрировать, как можно использовать группы размещения для поиска в Azure для развертывания виртуальной машины, см. список команд PowerShell, демонстрирующих использование для первого упражнения с группами размещения с учетом расположения Azure.

Первый шаг после входа в систему с помощью [Azure Cloud Shell](https://azure.microsoft.com/features/cloud-shell/) — проверка правильности подписки Azure, которую вы хотите использовать для развертывания с помощью команды:

<pre><code>
Get-AzureRmContext
</code></pre>

Если необходимо изменить другую подписку, это можно сделать, выполнив следующую команду:

<pre><code>
Set-AzureRmContext -Subscription "my PPG test subscription"
</code></pre>

В качестве третьего шага необходимо создать новую группу ресурсов Azure с помощью следующей команды:

<pre><code>
New-AzResourceGroup -Name "myfirstppgexercise" -Location "westus2"
</code></pre>

Вы можете создать новую группу размещения с учетом расположения с помощью:

<pre><code>
New-AzProximityPlacementGroup -ResourceGroupName "myfirstppgexercise" -Name "letsgetclose" -Location "westus2"
</code></pre>

Теперь вы можете приступить к развертыванию первой виртуальной машины в этой группе размещения с помощью команды, например:

<pre><code>
New-AzVm -ResourceGroupName "myfirstppgexercise" -Name "myppganchorvm" -Location "westus2" -OpenPorts 80,3389 -ProximityPlacementGroup "letsgetclose" -Size "Standard_DS11_v2"
</code></pre>

В приведенной выше команде развернута виртуальная машина на основе Windows. После успешной установки этой виртуальной машины область центра обработки данных в группе размещения с учетом расположения определяется в регионе Azure. Все последующие развертывания виртуальных машин, которые ссылаются на группу размещения с учетом расположения, как и в последней команде, будут развернуты в том же центре обработки данных Azure, если тип виртуальной машины может размещаться на оборудовании, размещенном в этом центре обработки данных, и (или) емкости для этого типа виртуальной машины.

## <a name="combine-availability-sets-and-availability-zones-with-proximity-placement-groups"></a>Объединение групп доступности и Зоны доступности с группами размещения с учетом расположения 
При использовании Зоны доступности для развертываний системы SAP один из недостатков заключается в том, что уровень приложений SAP невозможно развернуть с помощью групп доступности в определенной зоне. Так как вы хотите, чтобы уровень приложений SAP был развернут в тех же зонах, что и уровень СУБД, и ссылка на зону доступности и группу доступности при развертывании одной виртуальной машины не поддерживается, вы вынуждены развернуть уровень приложения, обратившись к зоне. и, таким образом, потеряет возможность убедиться, что виртуальные машины уровня приложения распределены между разными доменами обновления и сбоя. С помощью групп размещения близости можно преодолеть это ограничение. Последовательность развертываний будет выглядеть следующим образом:

- Создание группы размещения близкого взаимодействия
- Развертывание "привязки виртуальной машины" (как правило, сервера СУБД) с помощью ссылки на определенную зону доступности Azure
- Создание группы доступности, которая ссылается на группу близости Azure (см. ниже)
- Развертывание виртуальных машин уровня приложений путем ссылки на группу доступности и группы размещения с учетом расположения

Вместо развертывания первой виртуальной машины, как показано выше, вы ссылаетесь на зону доступности Azure и группу размещения с учетом расположения при развертывании виртуальной машины, например:

<pre><code>
New-AzVm -ResourceGroupName "myfirstppgexercise" -Name "myppganchorvm" -Location "westus2" -OpenPorts 80,3389 -Zone "1" -ProximityPlacementGroup "letsgetclose" -Size "Standard_E16_v3"
</code></pre>

При успешном развертывании этой виртуальной машины, в которой будет размещаться экземпляр базы данных системы SAP в одной зоне доступности Azure, область группы размещения с учетом расположения фиксируется в одном из центров обработки данных, представляющих определенную зону доступности. .

Предполагается, что вы развертываете виртуальные машины центральных служб точно так же, как и виртуальные машины СУБД, ссылаясь на те же зоны, что и для виртуальных машин СУБД и тех же групп размещения. На следующем шаге необходимо создать группы доступности, которые будут использоваться для прикладного уровня системы SAP.
Необходимо определить и создать группу размещения с учетом расположения. Команда для создания группы доступности требует дополнительной ссылки на идентификатор групп размещения близости (не имя). Идентификатор группы размещения с учетом расположения можно получить с помощью:



<pre><code>
Get-AzProximityPlacementGroup -ResourceGroupName "myfirstppgexercise" -Name "letsgetclose"
</code></pre>

При создании группы доступности необходимо рассмотреть дополнительные параметры при использовании управляемых дисков (по умолчанию, если они не заданы по-разному) и групп размещения с учетом расположения:

<pre><code>
New-AzAvailabilitySet -ResourceGroupName "myfirstppgexercise" -Name "myppgavset" -Location "westus2" -ProximityPlacementGroupId "/subscriptions/my very long ppg id string" -sku "aligned" -PlatformUpdateDomainCount 3 -PlatformFaultDomainCount 2 
</code></pre>

В идеале следует использовать три домена сбоя. Однако количество поддерживаемых доменов сбоя может отличаться от региона к региону. В этом случае максимальное количество доменов сбоя, возможное для конкретных регионов, было равно двум. Чтобы развернуть виртуальные машины уровня приложения, необходимо добавить ссылку на имя группы доступности и имя группе размещения с учетом расположения, как показано ниже:

<pre><code>
New-AzVm -ResourceGroupName "myfirstppgexercise" -Name "myppgavsetappvm" -Location "westus2" -OpenPorts 80,3389 -AvailabilitySetName "myppgavset" -ProximityPlacementGroup "letsgetclose" -Size "Standard_DS11_v2"
</code></pre>

Конечным результатом этой последовательности будет уровень СУБД и центральные службы системы SAP, расположенные в определенной зоне доступности, и уровень приложений SAP, расположенный в группах доступности в тех же центрах обработки данных Azure, что и виртуальные машины СУБД. развертывания.

> [!NOTE]
> При развертывании одной виртуальной машины СУБД в одной зоне и второй виртуальной машине СУБД в другой зоне для создания конфигурации высокого уровня доступности для каждой из зон потребовалось использовать разные группы размещения с учетом расположения. То же самое справедливо для группы доступности, которую можно использовать

## <a name="get-an-existing-system-into-azure-proximity-placement-groups"></a>Получение существующей системы в группах размещения для службы "близость" Azure
Так как система SAP уже развернута, может потребоваться оптимизировать сетевую задержку некоторых важных систем и разместить уровень приложения и уровень СУБД в одном центре обработки данных. На этапе общедоступной предварительной версии функции группы размещения с учетом расположения, для выполнения такого перехода в группы размещения с учетом расположения необходимо удалить виртуальные машины и создать новые виртуальные машины. На этом этапе работы не требуется завершать работу виртуальных машин, чтобы они могли назначить эти виртуальные машины для завершения работы в группы размещения.


## <a name="next-steps"></a>Следующие шаги
Изучите следующую документацию:

- [Контрольная нагрузка SAP по планированию и развертыванию Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-deployment-checklist)
- [Предварительная версия: Развертывание виртуальных машин в группах размещения с помощью Azure CLI](https://docs.microsoft.com/azure/virtual-machines/linux/proximity-placement-groups)
- [Предварительная версия: Развертывание виртуальных машин в группах размещения с помощью PowerShell](https://docs.microsoft.com/azure/virtual-machines/windows/proximity-placement-groups)
- [Вопросы развертывания СУБД для рабочей нагрузки SAP на виртуальных машинах Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/dbms_guide_general)

