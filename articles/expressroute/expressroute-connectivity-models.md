---
title: Модели подключения ExpressRoute в Azure | Документация Майкрософт
description: В этой статье описаны различные режимы подключения между сетью клиента и службами Microsoft Azure, Office 365 и Dynamics 365. Клиенты могут использовать поставщиков MPLS, поставщиков облачных служб Exchange и поставщиков Ethernet.
services: expressroute
author: cherylmc
ms.service: expressroute
ms.topic: conceptual
ms.date: 12/06/2018
ms.author: cherylmc
ms.custom: seodec18
ms.openlocfilehash: ef53f9d3cefa35624d6f94dc98db4ebe8b6646cd
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "60367462"
---
# <a name="expressroute-connectivity-models"></a>Модели подключения ExpressRoute
Вы можете создать между локальной сетью и облаком Майкрософт подключение трех разных типов: [совместное размещение в Cloud Exchange](#CloudExchange), [Ethernet-подключение типа "точка — точка"](#Ethernet) и [подключение типа "любой к любому" (IPVPN)](#IPVPN). Поставщики услуг подключения могут предлагать одну или несколько моделей подключения. Обратитесь к поставщику услуг подключения и подберите наиболее подходящую вам модель.
<br><br>

![Схема моделей подключения ExpressRoute](./media/expressroute-connectivity-models/expressroute-connectivity-models-diagram.png)

## <a name="CloudExchange"></a>Совместное размещение в Cloud Exchange
При размещении на одном сервере с Cloud Exchange можно заказать виртуальное кросс-подключение к облаку Майкрософт через Ethernet Exchange поставщика совместного размещения. Поставщики услуг совместного размещения могут предлагать либо кросс-подключения второго уровня, либо управляемые кросс-подключения третьего уровня между вашей инфраструктурой на сервере совместного размещения и облаком Майкрософт.

## <a name="Ethernet"></a>Подключения Ethernet типа "точка — точка"
Локальные центры обработки данных и офисы можно подключить к облаку Майкрософт через связи Ethernet типа "точка-точка". Поставщики подключений Ethernet типа "точка-точка" могут предлагать подключения второго уровня или управляемые подключения третьего уровня между вашей сетью и облаком Майкрософт.

## <a name="IPVPN"></a>Сети типа "любой к любому" (IPVPN)
Глобальную вычислительную сеть можно интегрировать с облаком Майкрософт. Поставщики IP VPN (обычно это MPLS VPN) предлагают подключение между филиалами и центрами обработки данных типа "любой к любому". Облако Майкрософт можно связать с вашей глобальной вычислительной сетью, так чтобы оно выглядело как любой другой филиал. Обычно поставщики глобальных вычислительных сетей предлагают управляемые подключения третьего уровня. Возможности и функции ExpressRoute одинаковы для всех описанных выше моделей. 

## <a name="next-steps"></a>Дальнейшие действия
* Узнайте больше о подключениях ExpressRoute и доменах маршрутизации. См. статью [Каналы ExpressRoute и домены маршрутизации](expressroute-circuit-peerings.md).
* Узнайте о функциях ExpressRoute. См. статью [Технический обзор ExpressRoute](expressroute-introduction.md).
* Найти поставщика услуг. См. статью [Партнеры и одноранговые расположения ExpressRoute](expressroute-locations.md).
* Убедитесь, что выполнены все необходимые условия. Ознакомьтесь с разделом [Предварительные требования и контрольный список для ExpressRoute](expressroute-prerequisites.md).
* См. сведения о требованиях для [маршрутизации](expressroute-routing.md) и [NAT](expressroute-nat.md) и [QoS](expressroute-qos.md).
* Настройте подключение ExpressRoute.
  * [Создайте канал ExpressRoute.](expressroute-howto-circuit-portal-resource-manager.md)
  * [Настройка маршрутизации](expressroute-howto-routing-portal-resource-manager.md)
  * [Связывание виртуальной сети с каналом ExpressRoute](expressroute-howto-linkvnet-portal-resource-manager.md)