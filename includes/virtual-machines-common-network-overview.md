---
title: включение файла
description: включение файла
services: virtual-machines-windows
author: cynthn
ms.service: virtual-machines-windows
ms.topic: include
ms.date: 11/01/2018
ms.author: cynthn
ms.custom: include file
ms.openlocfilehash: cd3b7d6cc75afc5d83ff02a15b920d9f8b05f608
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "66391429"
---
При создании виртуальной машины Azure вам потребуется [виртуальная сеть](../articles/virtual-network/virtual-networks-overview.md). Вы можете создать ее или использовать уже готовую. Кроме того, необходимо решить, как в этой виртуальной сети будет предоставляться доступ к виртуальным машинам. Очень важно [спланировать это перед созданием ресурсов](../articles/virtual-network/virtual-network-vnet-plan-design-arm.md), а также ознакомиться с [ограничениями, применяемыми к сетевым ресурсам](../articles/azure-subscription-service-limits.md#networking-limits).

На следующем рисунке виртуальные машины представлены в качестве веб-серверов и серверов баз данных. Каждый набор виртуальных машин назначен отдельной подсети в виртуальной сети.

![виртуальной сети Azure](./media/virtual-machines-common-network-overview/vnetoverview.png)

Виртуальную сеть можно создать перед созданием виртуальной машины или в процессе ее создания. Чтобы обеспечить взаимодействие с виртуальной машиной, создайте следующие ресурсы:

- Сетевые интерфейсы
- IP-адреса;
- Виртуальные сети и подсети

Помимо этих основных ресурсов, могут также потребоваться следующие дополнительные ресурсы:

- Группы безопасности сети
- Балансировщики нагрузки 

[!INCLUDE [updated-for-az](./updated-for-az.md)]

## <a name="network-interfaces"></a>Сетевые интерфейсы

[Сетевой интерфейс (NIC)](../articles/virtual-network/virtual-network-network-interface.md) обеспечивает взаимодействие между виртуальной машиной и виртуальной сетью. Виртуальная машина должна иметь по крайней мере один сетевой интерфейс, но в зависимости от ее размера количество сетевых интерфейсов можно увеличить. Узнайте, сколько сетевых адаптеров поддерживает каждый из размеров виртуальной машины для [Windows](../articles/virtual-machines/windows/sizes.md) или [Linux](../articles/virtual-machines/linux/sizes.md).

Вы можете создать виртуальную машину с несколькими сетевыми интерфейсами и добавлять или удалять их на протяжении жизненного цикла виртуальной машины. При наличии нескольких сетевых адаптеров виртуальная машина может подключаться к разным подсетям и отправлять или получать трафик через наиболее подходящий интерфейс. В одной группе доступности могут находиться виртуальные машины с любым количеством сетевых интерфейсов (в пределах количества, поддерживаемого размером виртуальной машины). 

Каждый сетевой интерфейс, подключенный к виртуальной машине, должен находиться в том же расположении и в той же подписке, что и виртуальная машина. Сетевые интерфейсы необходимо подключить к виртуальной сети, созданной в том же расположении Azure и в той же подписке, что и сам сетевой интерфейс. После создания виртуальной машины вы можете изменить подсеть, к которой она подключена, но изменить виртуальную сеть невозможно. Каждому сетевому интерфейсу, подключенному к виртуальной машине, назначается MAC-адрес, который не изменяется до удаления виртуальной машины.

В таблице ниже перечислены методы создания сетевого интерфейса.

| Метод | Описание |
| ------ | ----------- |
| Портал Azure | При создании виртуальной машины на портале Azure сетевой интерфейс создается автоматически (нельзя использовать сетевой интерфейс, созданный отдельно). На портале виртуальные машины создаются только с одним сетевым интерфейсом. Чтобы создать виртуальную машину с несколькими сетевыми интерфейсами, используйте другой метод. |
| [Azure PowerShell](../articles/virtual-machines/windows/multiple-nics.md) | Воспользуйтесь командлетом [New-AzNetworkInterface](https://docs.microsoft.com/powershell/module/az.network/new-aznetworkinterface) с параметром **-PublicIpAddressId**, чтобы указать идентификатор созданного ранее общедоступного IP-адреса. |
| [Интерфейс командной строки Azure](../articles/virtual-machines/linux/multiple-nics.md) | Чтобы указать идентификатор созданного ранее общедоступного IP-адреса, выполните команду [az network nic create](https://docs.microsoft.com/cli/azure/network/nic) с параметром **--public-ip-address**. |
| [Шаблон](../articles/virtual-network/template-samples.md) | Используйте шаблон [Network Interface in a Virtual Network with Public IP Address](https://github.com/Azure/azure-quickstart-templates/tree/master/101-nic-publicip-dns-vnet) (Сетевой интерфейс в виртуальной сети с общедоступным IP-адресом) в качестве руководства по развертыванию сетевого интерфейса с помощью шаблона. |

## <a name="ip-addresses"></a>IP-адреса 

В Azure сетевому интерфейсу можно назначить следующие типы [IP-адресов](../articles/virtual-network/virtual-network-ip-addresses-overview-arm.md).

- **Общедоступные IP-адреса.** Используются для входящего и исходящего подключения к Интернету и другим ресурсам Azure, не подключенным к виртуальной сети (без преобразования сетевых адресов (NAT)). Для сетевого интерфейса общедоступные IP-адреса назначать необязательно. За общедоступные IP-адреса взимается номинальная плата. В рамках подписки их использование ограничено.
- **Частные IP-адреса.** Используются для обмена данными в виртуальной сети, локальной сети и в Интернете (с преобразованием сетевых адресов). Виртуальная машина должна иметь по крайней мере один частный IP-адрес. Дополнительные сведения о преобразовании сетевых адресов см. в статье [Общие сведения об исходящих подключениях в Azure](../articles/load-balancer/load-balancer-outbound-connections.md).

Общедоступные IP-адреса можно назначать виртуальным машинам или балансировщикам нагрузки с выходом в Интернет, а частные IP-адреса — виртуальным машинам или внутренним балансировщикам нагрузки. IP-адреса для виртуальной машины можно назначить с помощью сетевого интерфейса.

Существует два способа выделения IP-адреса ресурсу: динамический и статический. По умолчанию используется динамический способ выделения, в рамках которого IP-адрес выделяется не тогда, когда он создается. В этом случае IP-адрес выделяется при создании, а также при запуске остановленной виртуальной машины. Если остановить или удалить виртуальную машину, IP-адрес освобождается. 

Чтобы IP-адрес виртуальной машины не изменялся, вы можете явным образом задать статический способ выделения. В рамках этого способа IP-адрес назначается немедленно, а освобождается только тогда, когда вы удаляете виртуальную машину или задаете динамический способ выделения.
    
В таблице ниже перечислены методы создания IP-адреса.

| Метод | Описание |
| ------ | ----------- |
| [портал Azure](../articles/virtual-network/virtual-network-deploy-static-pip-arm-portal.md) | По умолчанию общедоступные IP-адреса являются динамическими, и адреса, связанные с ними, могут измениться при удалении или остановке виртуальной машины. Чтобы общедоступный IP-адрес виртуальной машины не изменялся, необходимо создать статический общедоступный IP-адрес. По умолчанию при создании виртуальной машины на портале сетевому интерфейсу назначается динамический частный IP-адрес. После создания виртуальной машины его можно изменить на статический.|
| [Azure PowerShell](../articles/virtual-network/virtual-network-deploy-static-pip-arm-ps.md) | Используйте командлет [New-AzPublicIpAddress](https://docs.microsoft.com/powershell/module/az.network/new-azpublicipaddress) с параметром **-AllocationMethod**. Укажите для этого параметра значение Dynamic или Static. |
| [Интерфейс командной строки Azure](../articles/virtual-network/virtual-network-deploy-static-pip-arm-cli.md) | Используйте команду [az network public-ip create](https://docs.microsoft.com/cli/azure/network/public-ip) с параметром **--allocation-method**. Укажите для этого параметра значение Dynamic или Static. |
| [Шаблон](../articles/virtual-network/template-samples.md) | Используйте шаблон [Network Interface in a Virtual Network with Public IP Address](https://github.com/Azure/azure-quickstart-templates/tree/master/101-nic-publicip-dns-vnet) (Сетевой интерфейс в виртуальной сети с общедоступным IP-адресом) в качестве руководства по развертыванию общедоступного IP-адреса с помощью шаблона. |

После создания общедоступного IP-адреса его можно связать с виртуальной машиной, присвоив его сетевому интерфейсу.

## <a name="virtual-network-and-subnets"></a>Виртуальные сети и подсети

Подсеть — это диапазон IP-адресов в виртуальной сети. Виртуальную сеть можно разделить на несколько подсетей, чтобы организовать ее и повысить безопасность. Каждый сетевой интерфейс виртуальной машины подключен к одной подсети в одной виртуальной сети. Сетевые интерфейсы, подключенные к подсетям (одной или разным) в виртуальной сети, могут взаимодействовать друг с другом без дополнительной настройки.

При настройке виртуальной сети вы указываете топологию, в том числе доступные адресные пространства и подсети. Если виртуальная сеть будет подключаться к другим виртуальным или локальным сетям, необходимо выбрать неперекрывающиеся диапазоны адресов. Эти IP-адреса частные, и доступ к ним нельзя было получить через Интернет. Это касалось только IP-адресов, не поддерживающих маршрутизацию (например, 10.0.0.0/8, 172.16.0.0/12 или 192.168.0.0/16). Теперь Azure обрабатывает любой диапазон адресов как часть закрытого пространства IP-адресов виртуальной сети, доступного только в пределах виртуальной сети, взаимосвязанных виртуальных сетей и из локального расположения. 

Если вы работаете в организации, где за внутренние сети отвечает другой сотрудник, выбор адресного пространства необходимо обсудить с ним. Убедитесь, что диапазоны адресов не перекрываются, и сообщите этому сотруднику, какое пространство вы хотите использовать, чтобы он не использовал тот же диапазон IP-адресов. 

По умолчанию между подсетями отсутствует периметр безопасности, поэтому виртуальные машины, которые находятся в них, могут взаимодействовать. Однако вы можете настроить группы безопасности сети (NSG), которые позволяют управлять входящим и исходящим потоком трафика в подсетях и на виртуальных машинах. 

В таблице ниже перечислены методы создания виртуальной сети и подсетей. 

| Метод | Описание |
| ------ | ----------- |
| [портал Azure](../articles/virtual-network/quick-create-portal.md) | Если виртуальная сеть создана при создании виртуальной машины, то ее имя состоит из имени группы ресурсов, в которой находится виртуальная сеть, и части **-vnet**. Адресное пространство — 10.0.0.0/24, обязательное имя подсети — **default**, а диапазон адресов подсети — 10.0.0.0/24. |
| [Azure PowerShell](../articles/virtual-network/quick-create-powershell.md) | Чтобы создать подсеть и виртуальную сеть, используйте командлеты [New-AzVirtualNetworkSubnetConfig](https://docs.microsoft.com/powershell/module/az.network/new-azvirtualnetworkSubnetConfig) и [New-AzVirtualNetwork](https://docs.microsoft.com/powershell/module/az.network/new-azvirtualnetwork). Вы также можете добавить подсеть в существующую виртуальную сеть с помощью командлета [Add-AzVirtualNetworkSubnetConfig](https://docs.microsoft.com/powershell/module/Az.Network/Add-AzVirtualNetworkSubnetConfig). |
| [Интерфейс командной строки Azure](../articles/virtual-network/quick-create-cli.md) | Подсеть и виртуальная сеть создаются одновременно. В команду [az network vnet create](https://docs.microsoft.com/cli/azure/network/vnet) добавьте параметр **--subnet-name** с именем подсети. |
| Шаблон | Для создания виртуальной сети и подсетей проще всего скачать готовый шаблон, например [Virtual Network with two Subnets](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vnet-two-subnets) (Виртуальная сеть с двумя подсетями), и изменить его в соответствии со своими потребностями. |

## <a name="network-security-groups"></a>Группы безопасности сети

[Группа безопасности сети](../articles/virtual-network/virtual-network-vnet-plan-design-arm.md) содержит перечень правил списка управления доступом (ACL), которые разрешают или запрещают сетевой трафик к подсетям и сетевым интерфейсам (или и к тому, и к другому). Группы безопасности сети можно связать с подсетями или отдельными сетевыми интерфейсами, подключенными к подсети. Если группа безопасности сети связана с подсетью, правила списка управления доступом применяются ко всем виртуальным машинам в этой подсети. Кроме того, трафик к отдельным сетевым интерфейсам можно ограничить, связав группу безопасности сети непосредственно с нужным сетевым интерфейсом.

Группы безопасности сети содержат два набора правил: для входящего и исходящего трафика. Приоритет для правила должен быть уникальным в пределах каждого набора. Каждое правило имеет свойства протокола, диапазоны исходных портов и портов назначения, префиксы адресов, направление трафика, приоритет и тип доступа. 

Все группы NSG содержат набор правил по умолчанию. Эти правила нельзя удалить, но у них самый низкий приоритет, поэтому их можно переопределить, создав другие правила. 

При связывании группы безопасности сети с сетевой картой правила доступа к сети в этой группе NSG применяются только для данной сетевой карты. Если группа безопасности сети применяется к одному сетевому интерфейсу на виртуальной машине с несколькими сетевыми интерфейсами, она не влияет на трафик других сетевых интерфейсов. Различные группы безопасности сети можно связать с сетевым интерфейсом (или виртуальной машиной в зависимости от модели развертывания) и подсетью, с которой связан сетевой интерфейс или виртуальная машина. Приоритет определяется в зависимости от направления трафика.

При планировании виртуальных машин и сети [спланируйте](../articles/virtual-network/virtual-network-vnet-plan-design-arm.md) также группы безопасности сети.

В таблице ниже перечислены методы создания группы безопасности сети.

| Метод | Описание |
| ------ | ----------- |
| [портал Azure](../articles/virtual-network/tutorial-filter-network-traffic.md) | При создании виртуальной машины на портале Azure группа безопасности сети создается автоматически и связывается с сетевым интерфейсом, созданным на портале. Имя группы безопасности сети состоит из имени виртуальной машины и части **-nsg**. Эта группа безопасности сети содержит одно правило для входящего подключения с приоритетом 1000, разрешением RDP, протоколом TCP, портом 3389 и действием "Разрешить". Если вы хотите разрешить любой другой входящий трафик к виртуальной машине, добавьте в эту группу безопасности сети дополнительные правила. |
| [Azure PowerShell](../articles/virtual-network/tutorial-filter-network-traffic.md) | Используйте командлет [New AzNetworkSecurityRuleConfig](https://docs.microsoft.com/powershell/module/az.network/new-aznetworksecurityruleconfig) и предоставьте сведения о необходимом правиле. Используйте командлет [New-AzNetworkSecurityGroup](https://docs.microsoft.com/powershell/module/az.network/new-aznetworksecuritygroup), чтобы создать группу безопасности сети. Используйте командлет [Set-AzVirtualNetworkSubnetConfig](https://docs.microsoft.com/powershell/module/az.network/set-azvirtualnetworksubnetconfig), чтобы настроить группу безопасности сети для подсети. Используйте командлет [Set-AzVirtualNetwork](https://docs.microsoft.com/powershell/module/az.network/set-azvirtualnetwork), чтобы добавить группу безопасности сети в виртуальную сеть. |
| [Интерфейс командной строки Azure](../articles/virtual-network/tutorial-filter-network-traffic-cli.md) | Используйте команду [az network nsg create](https://docs.microsoft.com/cli/azure/network/nsg), чтобы изначально создать группу безопасности сети. Команда [az network nsg rule create](https://docs.microsoft.com/cli/azure/network/nsg/rule) позволяет добавить правила в группу безопасности сети, а команда [az network vnet subnet update](https://docs.microsoft.com/cli/azure/network/vnet/subnet) — добавить группу безопасности сети в подсеть. |
| [Шаблон](../articles/virtual-network/template-samples.md) | Используйте шаблон [Create a Network Security Group](https://github.com/Azure/azure-quickstart-templates/tree/master/101-security-group-create) (Создание группы безопасности сети) в качестве руководства по развертыванию группы безопасности сети с помощью шаблона. |

## <a name="load-balancers"></a>Балансировщики нагрузки

[Azure Load Balancer](../articles/load-balancer/load-balancer-overview.md) обеспечивает высокую доступность и производительность сети для приложений. Балансировщик нагрузки можно настроить для [балансировки входящего интернет-трафика](../articles/load-balancer/load-balancer-internet-overview.md) виртуальных машин или для [балансировки трафика между виртуальными машинами в виртуальной сети](../articles/load-balancer/load-balancer-internal-overview.md). Кроме того, балансировщик нагрузки может выполнять балансировку трафика между локальными компьютерами и виртуальными машинами в распределенных сетях или перенаправлять внешний трафик на определенную виртуальную машину.

Балансировщик нагрузки сопоставляет входящий и исходящий трафик между общедоступным IP-адресом и портом в балансировщике нагрузки и частным IP-адресом и портом виртуальной машины.

При создании балансировщика нагрузки необходимо также учитывать следующие элементы конфигурации.

- **Конфигурация внешних IP-адресов.** Балансировщик нагрузки может включать в себя один или несколько внешних IP-адресов, которые также называются виртуальными IP-адресами (VIP). Такие IP-адреса обеспечивают поступление трафика.
- **Пул внутренних адресов.** Это IP-адреса, связанные с сетевым интерфейсом, на который передается нагрузка.
- **Правила преобразования сетевых адресов.** Эти правила определяют, как входящий трафик проходит через внешний IP-адрес и передается внутреннему IP-адресу.
- **Правила балансировщика нагрузки.** Сопоставляют указанное сочетание внешнего IP-адреса и порта с набором внутренних IP-адресов и портов. Отдельный балансировщик нагрузки может применять несколько правил балансировки нагрузки. Каждое правило представляет собой сочетание интерфейсных IP-адреса и порта и внутренних IP-адреса и порта, связанных с виртуальными машинами.
- **[Пробы.](../articles/load-balancer/load-balancer-custom-probe-overview.md)** Пробы позволяют отслеживать работоспособность виртуальных машин. Если проба не отвечает, балансировщик нагрузки прекращает отправлять новые подключения неработоспособным виртуальным машинам. Это не влияет на имеющиеся подключения, а новые подключения отправляются на работоспособные виртуальные машины.

В таблице ниже перечислены методы создания балансировщика нагрузки с выходом в Интернет.

| Метод | Описание |
| ------ | ----------- |
| Портал Azure |  Вы можете [Балансировка нагрузки на Интернет-трафик к виртуальным машинам с помощью портала Azure](../articles/load-balancer/tutorial-load-balancer-standard-manage-portal.md). |
| [Azure PowerShell](../articles/load-balancer/load-balancer-get-started-internet-arm-ps.md) | Используйте командлет [New-AzLoadBalancerFrontendIpConfig](https://docs.microsoft.com/powershell/module/az.network/new-azloadbalancerfrontendipconfig) с параметром **-PublicIpAddress**, чтобы указать идентификатор созданного ранее общедоступного IP-адреса. Используйте командлет [New-AzLoadBalancerBackendAddressPoolConfig](https://docs.microsoft.com/powershell/module/az.network/new-azloadbalancerbackendaddresspoolconfig), чтобы создать конфигурацию пула внутренних адресов. Используйте командлет [New-AzLoadBalancerInboundNatRuleConfig](https://docs.microsoft.com/powershell/module/az.network/new-azloadbalancerinboundnatruleconfig), чтобы создать правила преобразования сетевых адресов для входящих подключений, связанные с созданной конфигурацией внешних IP-адресов. Используйте командлет [New-AzLoadBalancerProbeConfig](https://docs.microsoft.com/powershell/module/az.network/new-azloadbalancerprobeconfig), чтобы создать необходимые пробы. Используйте командлет [New-AzLoadBalancerRuleConfig](https://docs.microsoft.com/powershell/module/az.network/new-azloadbalancerruleconfig), чтобы создать конфигурацию балансировщика нагрузки. Используйте командлет [New-AzLoadBalancer](https://docs.microsoft.com/powershell/module/az.network/new-azloadbalancer), чтобы создать балансировщик нагрузки.|
| [Интерфейс командной строки Azure](../articles/load-balancer/load-balancer-get-started-internet-arm-cli.md) | Используйте команду [az network lb create](https://docs.microsoft.com/cli/azure/network/lb), чтобы создать начальную конфигурацию балансировщика нагрузки. Команда [az network lb frontend-ip create](https://docs.microsoft.com/cli/azure/network/lb/frontend-ip) позволяет добавить созданный ранее общедоступный IP-адрес, команда [az network lb address-pool create](https://docs.microsoft.com/cli/azure/network/lb/address-pool) — конфигурацию пула внутренних адресов, команда [az network lb inbound-nat-rule create](https://docs.microsoft.com/cli/azure/network/lb/inbound-nat-rule) — правила преобразования сетевых адресов, команда [az network lb rule create](https://docs.microsoft.com/cli/azure/network/lb/rule) — правила балансировщика нагрузки, а команда [az network lb probe create](https://docs.microsoft.com/cli/azure/network/lb/probe) — пробы. |
| [Шаблон](../articles/load-balancer/load-balancer-get-started-internet-arm-template.md) | Используйте шаблон [2 VMs in a Load Balancer and configure NAT rules on the LB](https://github.com/Azure/azure-quickstart-templates/tree/master/201-2-vms-loadbalancer-natrules) (2 виртуальные машины в балансировщике нагрузки и настройка правил преобразования сетевых адресов для балансировки нагрузки) в качестве руководства по развертыванию балансировщика нагрузки с помощью шаблона. |
    
В таблице ниже перечислены методы создания внутреннего балансировщика нагрузки.

| Метод | Описание |
| ------ | ----------- |
| Портал Azure | Вы можете [балансировки нагрузки внутреннего трафика с основных балансировкой нагрузки на портале Azure](../articles/load-balancer/tutorial-load-balancer-basic-internal-portal.md). |
| [Azure PowerShell](../articles/load-balancer/load-balancer-get-started-ilb-arm-ps.md) | Используйте командлет [New-AzLoadBalancerFrontendIpConfig](https://docs.microsoft.com/powershell/module/az.network/new-azloadbalancerfrontendipconfig) с параметром **-PrivateIpAddress**, чтобы указать частный IP-адрес в подсети сети. Используйте командлет [New-AzLoadBalancerBackendAddressPoolConfig](https://docs.microsoft.com/powershell/module/az.network/new-azloadbalancerbackendaddresspoolconfig), чтобы создать конфигурацию пула внутренних адресов. Используйте командлет [New-AzLoadBalancerInboundNatRuleConfig](https://docs.microsoft.com/powershell/module/az.network/new-azloadbalancerinboundnatruleconfig), чтобы создать правила преобразования сетевых адресов для входящих подключений, связанные с созданной конфигурацией внешних IP-адресов. Используйте командлет [New-AzLoadBalancerProbeConfig](https://docs.microsoft.com/powershell/module/az.network/new-azloadbalancerprobeconfig), чтобы создать необходимые пробы. Используйте командлет [New-AzLoadBalancerRuleConfig](https://docs.microsoft.com/powershell/module/az.network/new-azloadbalancerruleconfig), чтобы создать конфигурацию балансировщика нагрузки. Используйте командлет [New-AzLoadBalancer](https://docs.microsoft.com/powershell/module/az.network/new-azloadbalancer), чтобы создать балансировщик нагрузки.|
| [Интерфейс командной строки Azure](../articles/load-balancer/load-balancer-get-started-ilb-arm-cli.md) | Используйте команду [az network lb create](https://docs.microsoft.com/cli/azure/network/lb), чтобы создать начальную конфигурацию балансировщика нагрузки, и команду [az network lb frontend-ip create](https://docs.microsoft.com/cli/azure/network/lb/frontend-ip) с параметром **--private-ip-address**, чтобы определить частный IP-адрес. Команда [az network lb address-pool create](https://docs.microsoft.com/cli/azure/network/lb/address-pool) позволяет добавить конфигурацию пула внутренних адресов, команда [az network lb inbound-nat-rule create](https://docs.microsoft.com/cli/azure/network/lb/inbound-nat-rule) — правила преобразования сетевых адресов, команда [az network lb rule create](https://docs.microsoft.com/cli/azure/network/lb/rule) — правила балансировщика нагрузки, а команда [az network lb probe create](https://docs.microsoft.com/cli/azure/network/lb/probe) — пробы.|
| [Шаблон](../articles/load-balancer/load-balancer-get-started-ilb-arm-template.md) | Используйте шаблон [2 VMs in a Load Balancer and configure NAT rules on the LB](https://github.com/Azure/azure-quickstart-templates/tree/master/201-2-vms-internal-load-balancer) (2 виртуальные машины в балансировщике нагрузки и настройка правил преобразования сетевых адресов для балансировки нагрузки) в качестве руководства по развертыванию балансировщика нагрузки с помощью шаблона. |

## <a name="vms"></a>Виртуальные машины

Виртуальные машины можно создать в одной виртуальной сети, и они могут подключаться друг к другу с помощью частных IP-адресов. Они могут взаимодействовать, даже если находятся в разных подсетях. Для этого не нужно настраивать шлюз или использовать общедоступные IP-адреса. Чтобы добавить виртуальные машины в виртуальную сеть, необходимо сначала создать виртуальную сеть, а затем, создавая виртуальные машины, назначить каждую определенной виртуальной сети или подсети. Параметры сети виртуальных машин задаются во время развертывания или запуска,  

а IP-адрес назначается во время развертывания. Если в виртуальную сеть или подсеть развертывается несколько виртуальных машин, каждая из них получает свой IP-адрес во время загрузки. Виртуальной машине можно также выделить статический IP-адрес. При выделении статического IP-адреса используйте определенную подсеть. Это позволит избежать назначения этого IP-адреса для другой виртуальной машины.  

Если вы создали виртуальную машину, а затем хотите добавить ее в виртуальную сеть, потребуется много усилий, чтобы изменить настройки. В этом случае виртуальную машину необходимо повторно развернуть в виртуальную сеть. Проще всего удалить виртуальную машину (но не присоединенные к ней диски), а затем повторно создать ее в виртуальной сети, используя исходные диски. 

В таблице ниже перечислены методы создания виртуальной машины в виртуальной сети.

| Метод | Описание |
| ------ | ----------- |
| [портал Azure](../articles/virtual-machines/windows/quick-create-portal.md) | Используйте указанные ранее стандартные параметры сети, чтобы создать виртуальную машину с одним сетевым интерфейсом. Однако виртуальная машина с несколькими сетевыми интерфейсами создается другим методом. |
| [Azure PowerShell](../articles/virtual-machines/windows/tutorial-manage-vm.md) | Используйте командлет [Add-AzVMNetworkInterface](https://docs.microsoft.com/powershell/module/az.compute/add-azvmnetworkinterface), чтобы добавить в конфигурацию виртуальной машины созданный ранее сетевой интерфейс. |
| [Интерфейс командной строки Azure](../articles/virtual-machines/linux/create-cli-complete.md) | Создайте виртуальную машину и подключите ее к виртуальной сети, подсети и сетевому адаптеру, созданным на отдельных шагах. |
| [Шаблон](../articles/virtual-machines/windows/ps-template.md) | Используйте шаблон [Very simple deployment of a Windows VM](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-simple-windows) (Простое развертывание виртуальной машины Windows) в качестве руководства по развертыванию виртуальной машины с помощью шаблона. |

## <a name="next-steps"></a>Дальнейшие действия
Действия по управлению виртуальными сетями Azure для виртуальных машин см. в руководствах для [Windows](../articles/virtual-machines/windows/tutorial-virtual-network.md) или [Linux](../articles/virtual-machines/linux/tutorial-virtual-network.md).

Также ознакомьтесь с руководствами по распределению нагрузки виртуальных машин и созданию приложений с высоким уровнем доступности для [Windows](../articles/virtual-machines/windows/tutorial-load-balancer.md) или [Linux](../articles/virtual-machines/linux/tutorial-load-balancer.md).

- Дополнительные сведения об [определяемых пользователем маршрутах и IP-пересылке](../articles/virtual-network/virtual-networks-udr-overview.md). 
- Дополнительные сведения о [настройке подключения между виртуальными сетями](../articles/vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md).
- Дополнительные сведения об [устранении проблем с маршрутами](../articles/virtual-network/diagnose-network-routing-problem.md).
