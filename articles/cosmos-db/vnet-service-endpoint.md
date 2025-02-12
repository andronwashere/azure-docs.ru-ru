---
title: Безопасный доступ к учетной записи Azure Cosmos DB с использованием конечной точки службы для виртуальной сети Azure
description: В этом документе описано управление доступом к виртуальной сети и подсети для учетной записи Azure Cosmos.
author: kanshiG
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/23/2019
ms.author: govindk
ms.reviewer: sngun
ms.openlocfilehash: dfc3ebc0274c87466d6dc27c93880483df023085
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "66242466"
---
# <a name="access-azure-cosmos-db-from-virtual-networks-vnet"></a>Доступ к Azure Cosmos DB из виртуальных сетей

Учетные записи Azure Cosmos можно настроить для предоставления доступа только из определенной подсети виртуальной сети. Если включить [конечную точку службы](../virtual-network/virtual-network-service-endpoints-overview.md) для доступа к Azure Cosmos DB в подсети в виртуальной сети, трафик из этой подсети отправляется в базу данных Azure Cosmos DB с удостоверением подсети и виртуальной сети. После включения конечной точки службы Azure Cosmos DB можно ограничить доступ к подсети, добавив ее в учетную запись Azure Cosmos.

По умолчанию доступ к учетной записи Azure Cosmos можно получить через любой источник при условии, что запрос сопровождается допустимым маркером проверки подлинности. При добавлении одной или нескольких подсетей в виртуальную сеть только запросы, полученные из этих подсетей, будут получать правильный ответ. Запросы, полученные из любого другого источника, получат ответ "403 (запрещено)". 

## <a name="frequently-asked-questions"></a>Часто задаваемые вопросы

Ниже приведены некоторые часто задаваемые вопросы о настройке доступа из виртуальных сетей.

### <a name="can-i-specify-both-virtual-network-service-endpoint-and-ip-access-control-policy-on-an-azure-cosmos-account"></a>Можно ли указать политику конечной точки службы для виртуальной сети и политику контроля доступа к IP-адресу в учетной записи Azure Cosmos? 

Вы можете включить как конечную точку службы для виртуальной сети, так и политику управления доступом к IP-адресу (брандмауэр) в свою учетную запись Azure Cosmos. Эти две возможности дополняют друг друга и совместно обеспечивают изоляцию и безопасность вашей учетной записи Azure Cosmos. Использование брандмауэра IP гарантирует, что статические IP-адреса могут получать доступ к учетной записи Azure Cosmos DB. 

### <a name="how-do-i-limit-access-to-subnet-within-a-virtual-network"></a>Как ограничить доступ к подсети в виртуальной сети? 

Существует два шага, необходимые для ограничения доступа к учетной записи Azure Cosmos из подсети. Во-первых, разрешите трафику из подсети переносить свою подсеть и идентификатор виртуальной сети в Azure Cosmos DB. Это делается путем включения конечной точки службы для Azure Cosmos DB в подсети. Далее добавляется правило в учетной записи Azure Cosmos, определяющее эту подсеть как источник, с которого можно получить доступ к учетной записи.

### <a name="will-virtual-network-acls-and-ip-firewall-reject-requests-or-connections"></a>Будут ли ACL виртуальной сети и брандмауэр IP-адресов отклонять запросы или соединения? 

Когда добавляются правила доступа брандмауэра IP-адресов или виртуальной сети, только запросы из разрешенных источников получают действительные ответы. Другие запросы отклоняются с ошибкой "403 (запрещено)". Важно отличать брандмауэр учетной записи Azure Cosmos от брандмауэра на уровне подключения. Источник все еще может подключиться к службе, а сами соединения не будут отклонены.

### <a name="my-requests-started-getting-blocked-when-i-enabled-service-endpoint-to-azure-cosmos-db-on-the-subnet-what-happened"></a>Мои запросы начали блокироваться при включении конечной точки службы для Azure Cosmos DB в подсети. Что произошло?

После того как конечная точка службы для Azure Cosmos DB будет включена в подсети, источник трафика, подключающийся к учетной записи, переключится с общего IP-адреса на виртуальную сеть и подсеть. Если ваша учетная запись Azure Cosmos имеет только брандмауэр на основе IP-адреса, трафик из подсети с поддержкой службы больше не будет соответствовать правилам брандмауэра для IP-адресов и, следовательно, будет отклонен. Выполните действия, чтобы прозрачно выполнить миграцию из брандмауэра на основе IP-адресов в виртуальную сеть с управлением доступом.

### <a name="do-the-peered-virtual-networks-also-have-access-to-azure-cosmos-account"></a>У одноранговых виртуальных сетей также есть доступ к учетной записи Azure Cosmos? 
Доступ имеют только виртуальные сети и их подсети, добавленные к учетной записи Azure Cosmos. Одноранговые виртуальные сети не могут получить доступ к учетной записи, пока подсети в одноранговых виртуальных сетях не будут добавлены в учетную запись.

### <a name="what-is-the-maximum-number-of-subnets-allowed-to-access-a-single-cosmos-account"></a>Каково максимальное число подсетей, которым разрешен доступ к одной учетной записи Cosmos? 
В настоящее время может иметь не более 64 подсетей, разрешенное для учетной записи Azure Cosmos.

### <a name="can-i-enable-access-from-vpn-and-express-route"></a>Можно ли включить доступ для VPN и Express Route? 
Для доступа к учетной записи Azure Cosmos через Express route из локальной среды, необходимо включить пиринг Майкрософт. После размещения правил доступа к брандмауэру IP-адресов или виртуальной сети можно добавить общедоступные IP-адреса, используемые для пиринга Майкрософт в брандмауэре учетной записи Azure Cosmos, чтобы разрешить локальным службам доступ к учетной записи. 

### <a name="do-i-need-to-update-the-network-security-groups-nsg-rules"></a>Нужно ли обновлять правила группы безопасности сети (NSG)? 
Правила группы безопасности сети позволяют ограничить возможность подключения к подсети с помощью виртуальной сети. При добавлении конечной точки службы для Azure Cosmos DB к подсети нет необходимости открывать исходящие подключения в группе безопасности сети для учетной записи Azure Cosmos. 

### <a name="are-service-endpoints-available-for-all-vnets"></a>Доступны ли конечные точки служб для всех виртуальных сетей?
Нет, только виртуальные сети Azure Resource Manager могут использовать конечную точку службы. Классические виртуальные сети не поддерживают конечные точки службы.

### <a name="can-i-accept-connections-from-within-public-azure-datacenters-when-service-endpoint-access-is-enabled-for-azure-cosmos-db"></a>Можно ли принимать подключения из общедоступных центров данных Azure, если для Azure Cosmos DB включен доступ через конечную точку службы?  
Это необходимо, только если требуется, чтобы к учетной записи Azure Cosmos DB могли получать доступ другие службы Azure, такие как фабрика данных Azure, поиск Azure или любые службы, развернутые в данном регионе Azure.


## <a name="next-steps"></a>Дальнейшие действия

* [Получение доступа к ресурсам Azure Cosmos DB из виртуальных сетей](how-to-configure-vnet-service-endpoint.md)
* [How-to configure IP firewall for your Azure Cosmos account](how-to-configure-firewall.md) (Инструкции по настройке брандмауэра IP-адресов для учетной записи Azure Cosmos)

