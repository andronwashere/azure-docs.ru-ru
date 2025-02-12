---
title: Виртуальная сеть Azure | Документация Майкрософт
description: Узнайте об основных понятиях и функциях виртуальной сети Azure.
services: virtual-network
documentationcenter: na
author: anavinahar
tags: azure-resource-manager
Customer intent: As someone with a basic network background that is new to Azure, I want to understand the capabilities of Azure Virtual Network, so that my Azure resources such as VMs, can securely communicate with each other, the internet, and my on-premises resources.
ms.service: virtual-network
ms.devlang: na
ms.topic: overview
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/19/2019
ms.author: anavin
ms.openlocfilehash: 22c1e3050915fc697a62862620ef492ef22f80b8
ms.sourcegitcommit: 5bdd50e769a4d50ccb89e135cfd38b788ade594d
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/03/2019
ms.locfileid: "67542841"
---
# <a name="what-is-azure-virtual-network"></a>Что такое виртуальная сеть Azure?

Виртуальная сеть (VNet) Azure — это стандартный строительный блок для вашей частной сети в Azure. Виртуальная сеть позволяет ресурсам Azure различных типов (например, виртуальным машинам Azure) обмениваться данными друг с другом через локальные сети и через Интернет. Виртуальная сеть похожа на традиционную сеть, c которой вы будете работать в собственном центре обработки данных, но предлагает дополнительные возможности инфраструктуры Azure, как например масштабирование, доступность и изоляция.

## <a name="vnet-concepts"></a>Основные понятия о виртуальной сети

- **Адресное пространство.** При создании виртуальной сети необходимо указать пользовательское пространство частных IP-адресов, в котором используются общедоступные и частные адреса (RFC 1918). Azure назначает ресурсам в виртуальной сети частный IP-адрес из определенного вами адресного пространства. Например, при развертывании виртуальной машины в виртуальной сети с адресным пространством 10.0.0.0/16 виртуальная машина назначается частный IP-адрес, подобный 10.0.0.4.
- **Подсети:** Подсети позволяют вам разделить виртуальную сеть на подсети и выделить каждой из них часть адресного пространства виртуальной сети. Затем можно развернуть ресурсы Azure в определенной подсети. Так же, как в традиционной сети, подсети позволяют вам разделить адресное пространство виртуальной сети на сегменты, которые подходят для внутренней сети организации. Это также увеличивает эффективность распределения адресов. Вы можете защищать ресурсы в рамках подсетей с помощью групп безопасности сети. Дополнительные сведения см. в статье о [группах безопасности](security-overview.md).
- **Регионы**. Виртуальная сеть ограничена одним регионом или расположением, но взаимодействие между несколькими виртуальными сетями из разных регионов можно обеспечить с помощью пиринговой связи.
- **Подписка**: Виртуальная сеть предоставляется в пределах подписки. Вы можете реализовать несколько виртуальных сетей в пределах [подписки](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#subscription) и [региона](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#region) Azure.

## <a name="best-practices"></a>Рекомендации

При создании сети в Azure важно учесть следующие универсальные принципы проектирования.

- Убедитесь в отсутствии перекрытий в адресных пространствах. Убедитесь, что ваше адресное пространство виртуальной сети (блок CIDR) не перекрывается в диапазонах других сетей вашей организации.
- Ваши подсети не должны охватывать все адресное пространство виртуальной сети. Подготовьтесь заранее и зарезервируйте некоторое адресное пространство на будущее.
- Мы советуем создать несколько больших виртуальных сетей, а не много маленьких. Это позволит предотвратить расходы на управление.
- Обеспечьте безопасность виртуальной сети средствами групп безопасности сети (NSG).

## <a name="communicate-with-the-internet"></a>Обмен данными через Интернет

По умолчанию все ресурсы в виртуальной сети могут устанавливать исходящие подключения к Интернету. Можно также установить входящее подключение к ресурсу, присвоив ему общедоступный IP-адрес, или общедоступный экземпляр Load Balancer. Общедоступный IP-адрес или общедоступную подсистему балансировки нагрузки также можно использовать для управления исходящими подключениями.  См. дополнительные сведения об [исходящих интернет-подключениях](../load-balancer/load-balancer-outbound-connections.md), [общедоступных IP-адресах](virtual-network-public-ip-address.md) и [Load Balancer](../load-balancer/load-balancer-overview.md).

>[!NOTE]
>При использовании только внутреннего экземпляра [Load Balancer (цен. категория "Стандартный")](../load-balancer/load-balancer-standard-overview.md) исходящие подключения будут недоступными, пока вы не настроите для [исходящих подключений](../load-balancer/load-balancer-outbound-connections.md) использование общедоступного IP-адреса уровня экземпляра или общедоступного экземпляра Load Balancer.

## <a name="communicate-between-azure-resources"></a>Обмен данными между ресурсами Azure

Безопасный обмен данными между ресурсами обеспечивается одним из следующих способов:

- **Через виртуальную сеть.** Вы можете развернуть в виртуальной сети виртуальные машины и несколько других типов ресурсов Azure, например среды Службы приложений Azure, Службу Azure Kubernetes и Масштабируемые наборы виртуальных машин Azure. Полный список ресурсов Azure, которые можно развернуть в виртуальной сети, см. в статье [Интеграция виртуальной сети для служб Azure](virtual-network-for-azure-services.md).
- **Через конечную точку службы для виртуальной сети.** Расширьте пространство частных адресов и возможности идентификации своей виртуальной сети до ресурсов службы Azure (например, учетные записи службы хранилища Microsoft Azure и базы данных SQL Azure), используя прямое подключение. Конечные точки служб позволяют защищать критически важные ресурсы служб Azure в пределах вашей виртуальной сети. Дополнительные сведения см. в статье [Конечные точки служб для виртуальной сети](virtual-network-service-endpoints-overview.md).
- **Базовая пиринговая связь между виртуальными сетями**. Виртуальные сети можно подключать между собой. Таким образом, ресурсы в этих виртуальных сетях могут обмениваться данными, используя пиринговую связь между этими сетями. Виртуальные сети, которые вы подключаете, могут находиться в одном или в разных регионах Azure. Дополнительные сведения см. в статье [Пиринг между виртуальными сетями](virtual-network-peering-overview.md).

## <a name="communicate-with-on-premises-resources"></a>Обмен данными через локальные ресурсы

Локальные компьютеры и сети можно подключить к виртуальной сети, используя любое сочетание следующих способов:

- **Виртуальная частная сеть (VPN) типа "точка — сеть".** Устанавливается между виртуальной сетью и отдельным компьютером в вашей сети. Необходимо настроить подключение для каждого компьютера, который требуется подключить к виртуальной сети. Этот тип подключения идеально подходит для новичков, не умеющих работать в Azure, или для разработчиков, так как при его использовании существующую сеть почти не нужно менять. Обмен данными между компьютером и виртуальной сетью осуществляется через Интернет с помощью зашифрованного туннеля. Дополнительные сведения см. в разделе о [VPN-подключении "точка — сеть"](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%2ftoc.json#P2S).
- **VPN-подключение типа "сеть — сеть".** Устанавливается между локальным VPN-устройством и VPN-шлюзом Azure, развернутым в виртуальной сети. Используя такой тип соединения, авторизованные локальные ресурсы могут получить доступ к виртуальной сети. Обмен данными между локальным VPN-устройством и VPN-шлюзом Azure осуществляется через Интернет с помощью зашифрованного туннеля. Дополнительные сведения см. в разделе о [VPN-подключении "сеть — сеть"](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%2ftoc.json#s2smulti).
- **Azure ExpressRoute.** Устанавливается между вашей сетью и Azure через участник ExpressRoute. Это подключение является закрытым. Трафик не проходит через Интернет. Дополнительные сведения см. в разделе [ExpressRoute (частное подключение)](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%2ftoc.json#ExpressRoute).

## <a name="filter-network-traffic"></a>Фильтрация сетевого трафика

Сетевой трафик между подсетями можно фильтровать следующими способами:

- **Группы безопасности.** Группы безопасности сети и приложения могут содержать несколько правил безопасности относительно входящего и исходящего трафика, что позволяет фильтровать его по исходному и конечному IP-адресу, порту и протоколу. Дополнительные сведения см. в разделах о [группах безопасности сети](security-overview.md#network-security-groups) и [группах безопасности приложений](security-overview.md#application-security-groups).
- **Виртуальные сетевые модули.** Виртуальный сетевой модуль представляет собой виртуальную машину, которая выполняет сетевую функцию (например, брандмауэр, оптимизация WAN и др.). Список доступных сетевых виртуальных модулей, которые можно развернуть в виртуальной сети, см. на странице [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/category/networking?page=1&subcategories=appliances).

## <a name="route-network-traffic"></a>Маршрутизация сетевого трафика

По умолчанию Azure маршрутизирует трафик между подсетями, подключенными виртуальными сетями, локальными сетями и Интернетом. Чтобы переопределить маршруты по умолчанию, создаваемые в Azure, используйте следующие варианты:

- **Таблицы маршрутов.** Вы можете создать пользовательские таблицы с маршрутами, которые определяют направление передачи трафика для каждой подсети. Подробнее о [таблицах маршрутов](virtual-networks-udr-overview.md#user-defined).
- **Маршруты протокола BGP.** При подключении виртуальной сети к локальной сети с помощью VPN-шлюза или канала ExpressRoute Azure можно распространить локальные маршруты BGP в виртуальных сетях. Подробнее об использовании протокола BGP с [VPN-шлюзом Azure](../vpn-gateway/vpn-gateway-bgp-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json) и [ExpressRoute](../expressroute/expressroute-routing.md?toc=%2fazure%2fvirtual-network%2ftoc.json#dynamic-route-exchange).

## <a name="azure-vnet-limits"></a>Ограничения виртуальной сети Azure

Существуют определенные ограничения на количество ресурсов Azure, которые вы можете развернуть. Для большинства ограничений сети Azure заданы максимальные значения. Тем не менее вы можете [увеличить определенные сетевые ограничения](../azure-supportability/networking-quota-requests.md), как указано на [странице ограничений виртуальной сети](../azure-subscription-service-limits.md#networking-limits). 

## <a name="pricing"></a>Цены

Плата за использование виртуальной сети Azure не взимается, она бесплатная. Взимается стандартная плата за ресурсы, такие как виртуальные машины и другие продукты. Дополнительные сведения см. на странице [цен на виртуальную сеть](https://azure.microsoft.com/pricing/details/virtual-network/) и [калькулятора цен](https://azure.microsoft.com/pricing/calculator/) Azure.

## <a name="next-steps"></a>Дополнительная информация

 Чтобы начать использовать виртуальную сеть, создайте ее, разверните в ней несколько виртуальных машин и выполните обмен данными между виртуальными машинами. Чтобы узнать, как это сделать, см. краткое руководство по [созданию виртуальной сети](quick-create-portal.md).
