---
title: Проектирование для обеспечения высокой доступности с помощью Azure ExpressRoute | Документация Майкрософт
description: Эта страница содержит архитектурные рекомендации для обеспечения высокой доступности при использовании Azure ExpressRoute.
documentationcenter: na
services: networking
author: rambk
manager: tracsman
ms.service: expressroute
ms.topic: article
ms.workload: infrastructure-services
ms.date: 06/28/2019
ms.author: rambala
ms.openlocfilehash: 4984b30daf6170873cad9472bfed2d879af57efe
ms.sourcegitcommit: c63e5031aed4992d5adf45639addcef07c166224
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/28/2019
ms.locfileid: "67466644"
---
# <a name="designing-for-high-availability-with-expressroute"></a>Проектирование для обеспечения высокой доступности с помощью ExpressRoute

ExpressRoute предназначен для обеспечения высокой доступности для обеспечения корпоративного класса подключения к частной сети к ресурсам Microsoft перевозчика. Другими словами является единственной точки сбоя в путь ExpressRoute сети корпорации Майкрософт. Чтобы добиться максимальной доступности, клиента и поставщика службы сегмента канала ExpressRoute должен также быть разработана для обеспечения высокой доступности. В этой статье, сначала давайте рассмотреть вопросы архитектуры сети для построения надежных сетевое подключение с помощью ExpressRoute, а затем рассмотрим точной настройки функций, которые помогут вам для повышения уровня доступности канала ExpressRoute.


## <a name="architecture-considerations"></a>Рекомендации по архитектуре

На следующем рисунке показано рекомендуемый способ подключения через канал ExpressRoute для максимизации доступности канала ExpressRoute.

 [![1]][1]

Для обеспечения высокой доступности очень важно для обеспечения избыточности цепи ExpressRoute во всей сети end-to-end. Другими словами вы должны размещаться избыточности в пределах вашей локальной сети и не следует поставить под угрозу избыточность в сети поставщика услуг. Поддержка избыточности по меньшей мере подразумевает как избежать единых точек отказа сети. Наличие питания и охлаждения для сетевых устройств будет более улучшить высокую доступность.

### <a name="first-mile-physical-layer-design-considerations"></a>Рекомендации по проектированию первой мили физический уровень

 При закрытии программы как основного и дополнительного соединений цепей ExpressRoute на одного клиента локальных устройств, Размещенных при компрометации высокого уровня доступности в вашей локальной сети. Кроме того Если вы настраиваете обоих первичных и вторичных подключений через тот же порт CPE, (либо, прервав двух подключений в группе различные вложенные интерфейсы либо путем слияния двух подключений партнерской сети), принудительной установке партнера злоумышленнику высокий уровень доступности на их сегмента. Этот компромисс показано на рисунке ниже.

[![2]][2]

С другой стороны при закрытии программы первичной и вторичной подключения для каналов ExpressRoute в разных географических точках, затем вам может ущерба производительности сетевого подключения. Если активно, трафик будет равномерно распределяться между первичной и вторичной подключения, которые завершаются в разных географических точках, потенциальные существенно повлиять на сетевую задержку между двумя путями приведет к неоптимальной сети производительность. 

Географически избыточное рекомендации, см. в разделе [разработка для аварийного восстановления с помощью ExpressRoute][DR].

### <a name="active-active-connections"></a>Активных подключений

Microsoft сеть настроена для работы основного и дополнительного соединений каналов ExpressRoute в активный — активный режим. Тем не менее через маршрут рекламу, вы можете принудительно избыточные подключения канала ExpressRoute, чтобы перейти в режим активный / пассивный. Рекламных более конкретные маршруты и BGP, так как атрибут AS path распространенные методы, позволяющее сделать один путь, предпочтительнее, чем другой.

Для повышения уровня доступности, рекомендуется для работы подключения канала ExpressRoute в активный — активный режим. Если вы могли работать в режиме активных подключений, сеть Майкрософт будет балансировки трафика через подключения на основе каждого потока.

Рискуя основного и дополнительного соединений канала ExpressRoute в активный / пассивный режим лиц из подключений, сбоем с последующим сбоем в активный путь. Основные причины для сбоя на переключение: отсутствие активного управления пассивное соединение и пассивный подключения рекламу устаревших маршруты.

Кроме того, под управлением основного и дополнительного соединений канала ExpressRoute в режим активный — активный приводит лишь примерно половину потоки сбоем и получение пересылаются, после сбоя подключения ExpressRoute. Таким образом активный — активный режим может значительно повысить среднее время для восстановления (MTTR).

### <a name="nat-for-microsoft-peering"></a>NAT для пиринга Майкрософт 

Пиринг Майкрософт предназначен для обмена данными между общедоступных конечных точек. Так часто частный или локальных конечных точек, сетевой адрес преобразован (преобразованный через NAT) общедоступного IP-адреса клиентов или партнеров сети прежде, чем они будут взаимодействовать через пиринг Майкрософт. При условии использования основного и дополнительного соединений в активный — активный режим, где и как вы NAT оказывает влияние на скорость восстановления после сбоя в одном из подключения ExpressRoute. На следующем рисунке показаны два различных варианта NAT:

[![3]][3]

В параметре 1 NAT применяется после разделения трафика между основного и дополнительного соединений ExpressRoute. Для соответствия требованиям с отслеживанием состояния NAT, независимых пулы NAT используются между первичной и вторичной устройств, чтобы обратный трафик будет поступать на то же устройство edge, через который же поток.

В параметре 2 распространенные пула NAT используется перед разделением трафика между основного и дополнительного соединений ExpressRoute. Очень важно знать об отличиях в общий пул NAT перед разделение трафика не означает Знакомство с единой точкой сбоя, тем самым ущерба высокого уровня доступности.

С параметром 1, после сбоя подключения ExpressRoute возможностью добраться до соответствующего пула NAT недействительна. Таким образом, все неработающие потоки должны быть повторно созданы либо по протоколу TCP или уровень приложения после соответствующего времени ожидания окна. Если хотя бы один из пулов NAT используется для внешнего интерфейса любой из на локальных серверах и если соответствующие подключения переход на другой, на локальных серверах становится недоступной из Azure пока подключение не будет устранена.

В то время как с параметром 2, NAT доступен даже после сбоя подключения первичной или вторичной. Таким образом на уровне сети, сам может перенаправить пакетов и справки более быстрое восстановление после сбоя. 

> [!NOTE]
> Если вы используете NAT вариант 1 (независимые пулов NAT для основного и дополнительного соединений ExpressRoute) и сопоставления порта IP-адреса из одного пула NAT на локальный сервер, сервер не будут доступны через ExpressRoute канала, когда соответствующий Сбой подключения.
> 

## <a name="fine-tuning-features-for-private-peering"></a>Тонкая настройка функций для частного пиринга

В этом разделе, сообщите нам необязательно (в зависимости от развертывания Azure и как конфиденциальные вы для MTTR) для проверки функций, Помогите нам улучшить высокого уровня доступности канала ExpressRoute. В частности давайте рассмотрим часовыми поясами развертывания шлюзов виртуальной сети ExpressRoute и пересылки обнаружения двунаправленных (BFD).

### <a name="availability-zone-aware-expressroute-virtual-network-gateways"></a>Зона доступности виду шлюзах виртуальных сетей ExpressRoute

Зона доступности в регионе Azure — это сочетание домена сбоя и домена обновления. Если вы выбрали избыточные в пределах зоны развертывания Azure IaaS, можно также настроить шлюзы виртуальной сети, избыточные в пределах зоны, приводящих к прекращению частный пиринг ExpressRoute. Дополнительные сведения, см. в разделе [о шлюзах избыточные в пределах зоны виртуальных сетей в зонах доступности Azure][zone redundant vgw]. To configure zone-redundant virtual network gateway, see [Create a zone-redundant virtual network gateway in Azure Availability Zones][conf zone redundant vgw].

### <a name="improving-failure-detection-time"></a>Улучшение время обнаружения сбоя

ExpressRoute поддерживает BFD через частный пиринг. BFD уменьшает время обнаружения сбоя сети уровня 2 между Microsoft Enterprise Edge (Msee) и их соседями BGP на локальной стороне из около трех минут (по умолчанию), до менее секунды. Время обнаружения быстрого сбоя помогает уменьшенному восстановление после сбоя. Дополнительные сведения, см. в разделе [Настройка BFD через ExpressRoute][BFD].

## <a name="next-steps"></a>Дальнейшие действия

В этой статье мы рассмотрели способ проектирования для обеспечения высокой доступности, подключения канала ExpressRoute. Точка пиринга канала ExpressRoute закрепляется на географическое расположение и таким образом может быть нарушена после разрушительного сбоя, которые влияют на весь расположение. 

Вопросы проектирования для построения геоизбыточное сетевое подключение к магистраль Майкрософт, которое может выдержать катастрофических сбоев, которые влияют на весь регион, см. в разделе [проектирование для аварийного восстановления с помощью частного пирингаExpressRoute][DR].

<!--Image References-->
[1]: ./media/designing-for-high-availability-with-expressroute/exr-reco.png "рекомендуемый способ подключения с помощью ExpressRoute"
[2]: ./media/designing-for-high-availability-with-expressroute/suboptimal-lastmile-connectivity.png "Suboptimal последней мили подключения"
[3]: ./media/designing-for-high-availability-with-expressroute/nat-options.png "параметров NAT"


<!--Link References-->
[zone redundant vgw]: https://docs.microsoft.com/azure/vpn-gateway/about-zone-redundant-vnet-gateways
[conf zone redundant vgw]: https://docs.microsoft.com/azure/vpn-gateway/create-zone-redundant-vnet-gateway
[Configure Global Reach]: https://docs.microsoft.com/azure/expressroute/expressroute-howto-set-global-reach
[BFD]: https://docs.microsoft.com/azure/expressroute/expressroute-bfd
[DR]: https://docs.microsoft.com/azure/expressroute/designing-for-disaster-recovery-with-expressroute-privatepeering




