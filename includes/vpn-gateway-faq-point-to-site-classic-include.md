---
title: включение файла
description: включение файла
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 12/06/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 0c0ad6ea5a687d066c78533b45a7f531561661bf
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/18/2019
ms.locfileid: "67185154"
---
Эти вопросы и ответы применимы к подключениям P2S, использующих классическую модель развертывания.

### <a name="what-client-operating-systems-can-i-use-with-point-to-site"></a>Какие клиентские операционные системы можно использовать для подключения типа "точка — сеть"?

Поддерживаются следующие клиентские операционные системы:

* Windows 7 (32-разрядная и 64-разрядная версии)
* Windows Server 2008 R2 (только 64-разрядная версия)
* Windows 8 (32-разрядная и 64-разрядная версии)
* Windows 8.1 (32-разрядная и 64-разрядная версии)
* Windows Server 2012 (только 64-разрядная версия)
* Windows Server 2012 R2 (только 64-разрядная версия)
* Windows 10

### <a name="can-i-use-any-software-vpn-client-that-supports-sstp-for-point-to-site"></a>Можно ли использовать для подключения "точка — сеть" любой VPN-клиент, поддерживающий SSTP?

№ Поддержка предоставляется только для перечисленных выше версий операционной системы Windows.

### <a name="how-many-vpn-client-endpoints-can-exist-in-my-point-to-site-configuration"></a>Сколько конечных точек VPN-клиента может существовать в конфигурации "точка — сеть"?

До 128 VPN-клиентов могут одновременного подключаться к виртуальной сети.

### <a name="can-i-use-my-own-internal-pki-root-ca-for-point-to-site-connectivity"></a>Можно ли использовать корневой ЦС собственной внутренней системы PKI для подключения "точка — сеть"?

Да. Раньше поддерживались только самозаверяющие корневые сертификаты. Вы можете отправить до 20 корневых сертификатов.

### <a name="can-i-traverse-proxies-and-firewalls-by-using-point-to-site"></a>Можно ли просматривать прокси-серверы и брандмауэры с использованием конфигурации "точка — сеть"?

Да. Для туннелирования через брандмауэры используется протокол SSTP (Secure Socket Tunneling Protocol). Этот туннель отображается как HTTPS-подключение.

### <a name="if-i-restart-a-client-computer-configured-for-point-to-site-will-the-vpn-automatically-reconnect"></a>Если перезапустить клиентский компьютер, настроенный для подключения "точка — сеть", произойдет ли автоматическое переподключение VPN?

По умолчанию клиентский компьютер не восстанавливает VPN-подключение автоматически.

### <a name="does-point-to-site-support-auto-reconnect-and-ddns-on-the-vpn-clients"></a>Поддерживает ли подключение "точка — сеть" автоматическое повторное подключение и DDNS для VPN-клиентов?

№ В конфигурациях VPN "точка — сеть" в настоящее время не поддерживаются автоматическое повторное подключение и DDNS.

### <a name="can-i-have-site-to-site-and-point-to-site-configurations-for-the-same-virtual-network"></a>Могут ли сосуществовать в одной виртуальной сети конфигурации "сеть — сеть" и "точка — сеть"?

Да. Оба решения будут работать, если ваш шлюз использует тип VPN на основе маршрутов. В классической модели развертывания требуется динамический шлюз. Мы не поддерживаем подключения "точка — сеть" для VPN-шлюзов статической маршрутизации или шлюзов, использующих тип **VPN PolicyBased**.

### <a name="can-i-configure-a-point-to-site-client-to-connect-to-multiple-virtual-networks-at-the-same-time"></a>Можно ли настроить клиент "точка — сеть" для подключения к нескольким виртуальным сетям одновременно?

Да. Но виртуальные сети не могут иметь перекрывающиеся префиксы IP-адресов и адресные пространства "точка — сеть" не должны перекрываться между виртуальными сетями.

### <a name="how-much-throughput-can-i-expect-through-site-to-site-or-point-to-site-connections"></a>На какую пропускную способность можно рассчитывать в конфигурациях подключения "сеть — сеть" и "точка — сеть"?

Сложно поддерживать конкретную пропускную способность для туннелей VPN. IPsec и SSTP — это надежно зашифрованные протоколы VPN. Пропускная способность также зависит от задержки и пропускной способности между локальной сетью и Интернетом.
