---
title: Сведения об ExpressRoute Direct в Azure | Документация Майкрософт
description: Эта страница содержит обзор ExpressRoute Direct
services: expressroute
author: jaredr80
ms.service: expressroute
ms.topic: conceptual
ms.date: 07/10/2019
ms.author: jaredro
ms.custom: seodec18
ms.openlocfilehash: e598cc03a1b7b4999719152540866c7168130e03
ms.sourcegitcommit: 47ce9ac1eb1561810b8e4242c45127f7b4a4aa1a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/11/2019
ms.locfileid: "67807480"
---
# <a name="about-expressroute-direct"></a>Сведения об ExpressRoute Direct

ExpressRoute Direct дает возможность подключения непосредственно в глобальной сети корпорации Майкрософт в стратегически распределенных по всему миру расположениях пиринга. ExpressRoute Direct предоставляет два 100 Гбит/с или 10 Гбит/с подключения, которая поддерживает подключения активный/активный в нужном масштабе.

К основным особенностям ExpressRoute Direct можно отнести следующее:

* возможность приема больших объемов данных в службах, таких как служба хранилища и Cosmos DB;
* физическая изоляция для отраслей, которые регулируются и требуют специализированной и изолированной связи, например банковское дело, правительство и розничная торговля;
* четкий контроль распределения схем на основе бизнес-единиц.

## <a name="onboard-to-expressroute-direct"></a>Подключение к ExpressRoute Direct

Прежде чем использовать ExpressRoute Direct, сначала необходимо зарегистрировать подписку. Чтобы подписаться, отправьте электронное письмо на адрес <ExpressRouteDirect@microsoft.com>, указав идентификатор подписки и следующие сведения:

* задачи, которые необходимо выполнять с помощью **ExpressRoute Direct**;
* настройки местоположения — полный список всех расположений см. в разделе [Партнеры и одноранговые расположения ExpressRoute](expressroute-locations-providers.md);
* временную шкалу для реализации;
* другие вопросы.

## <a name="expressroute-using-a-service-provider-and-expressroute-direct"></a>ExpressRoute с использованием поставщика служб и ExpressRoute Direct

| **ExpressRoute с использованием поставщика служб** | **ExpressRoute Direct** | 
| --- | --- |
| Использование поставщиков служб для быстрого развертывания и подключения к существующей инфраструктуре. | Требуется 100 инфраструктура Гбит/с/10 Гбит/с и полное управление всех слоев
| Интеграция с сотнями поставщиков, включая Ethernet и MPLS. | Прямая или выделенная емкость для регулируемых отраслей промышленности и массовое проникновение данных. |
| Номера SKU для каналов со скоростью от 50 Мбит/с до 10 Гбит/с. | Клиента напрямую выбрать сочетание следующих канала номера SKU на 100 ExpressRoute Гбит/с: <ul><li>5 Гбит/с</li><li>10 Гбит/с</li><li>40 Гбит/с</li><li>100 Гбит/с</li></ul> Клиента выбрать сочетание следующих номеров SKU на 10 Гбит/с ExpressRoute канала Direct:<ul><li>1 Гбит/с</li><li>2 Гбит/с</li><li>5 Гбит/с</li><li>10 Гбит/с</li></ul>
| Оптимизировано для одного клиента | Оптимизировано для одного клиента,поставщиков облачных служб, нескольких бизнес-единиц

## <a name="expressroute-direct-circuits"></a>Каналы ExpressRoute Direct

Microsoft Azure ExpressRoute позволяет переносить локальные сети в облако Microsoft по частному подключению, которое обеспечивается поставщиком услуг подключения. ExpressRoute позволяет устанавливать подключения к облачным службам Майкрософт, таким как Microsoft Azure, Office 365 и Dynamics 365.  

Каждое место пиринга имеет доступ к глобальной сети Майкрософт и может иметь доступ к любому региону в геополитической зоне по умолчанию и ко всем глобальным регионам с каналом класса "Премиум".  

Функциональные возможности в большинстве случаев соответствуют каналам, в которых используется поставщик службы ExpressRoute. Чтобы поддерживать дополнительную степень детализации и новые возможности, предлагаемые ExpressRoute Direct, для каналов ExpressRoute Direct существуют определенные ключевые возможности.

## <a name="circuit-skus"></a>Каналы SKU

ExpressRoute Direct поддерживает сценарии приема данных в хранилище Azure и другие сервисы больших данных. Каналы ExpressRoute на 100 прямой Гбит/с ExpressRoute теперь также поддерживают **40 Гбит/с** и **100 Гбит/с** номера SKU для канала. Пары физического порта **100 или 10 Гбит/с** только и может иметь несколько виртуальных каналов. Размеры канала:

| **ExpressRoute-Direct 100 Гбит/с** | **ExpressRoute-Direct 10 Гбит/с** | 
| --- | --- |
| **Подписка пропускной способности**: 200 Гбит/с | **Подписка пропускной способности**: 20 Гбит/с |
| <ul><li>5 Гбит/с</li><li>10 Гбит/с</li><li>40 Гбит/с</li><li>100 Гбит/с</li></ul> | <ul><li>1 Гбит/с</li><li>2 Гбит/с</li><li>5 Гбит/с</li><li>10 Гбит/с</li></ul>

## <a name="technical-requirements"></a>технические требования;

* Интерфейсы (MSEE) маршрутизатора Microsoft Enterprise Edge:
    * Два 10 или 100 порта Gigabit Ethernet только через пару маршрутизатора
    * Один режим волокон LR подключения
    * IPv4 и IPv6
    * IP-адрес MTU 1500 байтов

* Подключение уровня 3, 2 или коммутатор/маршрутизатор уровня:
    * Должен поддерживать тег 1 802.1Q (Dot1Q) или два тега 802.1Q (QinQ) тега инкапсуляции
    * Ethertype = 0x8100
    * Необходимо добавить внешнего тега виртуальной локальной сети (STAG) на основе идентификатора виртуальной локальной сети корпорации Майкрософт - *применимо только в ОС QinQ*
    * Должен поддерживать несколько сеансов BGP (VLAN) для порта и устройства
    * Подключения IPv4 и IPv6. *Для IPv6 будет создаваться без дополнительных вложенного интерфейса. IPv6-адрес добавляется в существующий интерфейс*. 
    * Необязательно: [Обнаружение пересылки двунаправленный (BFD)](https://docs.microsoft.com/azure/expressroute/expressroute-bfd) поддерживает, настроенный по умолчанию на все частные Пиринги на каналы ExpressRoute

## <a name="vlan-tagging"></a>Добавление тегов в виртуальной локальной сети

ExpressRoute Direct поддерживает добавление тегов в виртуальной локальной сети как QinQ так и Dot1Q.

* **Добавление тегов виртуальной локальной сети QinQ** позволяет изолировать домены маршрутизации на каждом канале ExpressRoute. Azure динамически выделяет S-тег при создании канала, который не может быть изменен. Каждый пиринг на канале (закрытый и Майкрософт) будет использовать уникальный C-тег в качестве виртуальной локальной сети. C-тег не обязательно должен быть уникальным во всех каналах портов ExpressRoute Direct.

* **Добавление тегов в виртуальной локальной сети Dot1Q** позволяет использовать единственную виртуальную локальную сеть с тегами для каждой пары ExpressRoute Direct. C-тег, используемый для пиринга, должен быть уникальным для всех каналов и пирингов в паре ExpressRoute Direct.

## <a name="workflow"></a>Рабочий процесс

[![Рабочий процесс](./media/expressroute-erdirect-about/workflow1.png)](./media/expressroute-erdirect-about/workflow1.png#lightbox)

## <a name="sla"></a>Соглашение об уровне обслуживания

ExpressRoute Direct предоставляет одно и то же соглашение об обслуживании корпоративного уровня с резервными подключениями "активный — активный" в глобальной сети Майкрософт. Инфраструктура ExpressRoute является резервной, а подключение к глобальной сети Майкрософт — резервной и разноплановой, соответствующей требованиям пользователей. 

## <a name="next-steps"></a>Следующие шаги

[Настройка ExpressRoute Direct](expressroute-howto-erdirect.md)
