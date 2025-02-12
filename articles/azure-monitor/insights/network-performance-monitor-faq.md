---
title: Вопросы и ответы по решению "Монитор производительности сети" (Azure) | Документация Майкрософт
description: В этой статье захватывает часто задаваемые вопросы о мониторе производительности сети в Azure. Сетевой монитор производительности (NPM) позволяет отслеживать производительность ваших сетей практически в реальном времени и обнаружить и найдите сети узких мест производительности.
services: log-analytics
documentationcenter: ''
author: vinynigam
manager: agummadi
editor: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 10/12/2018
ms.author: vinigam
ms.openlocfilehash: 71eb789c92452353029613265fe97411c8c00649
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/09/2019
ms.locfileid: "67706332"
---
# <a name="network-performance-monitor-solution-faq"></a>Вопросы и ответы по решению "Монитор производительности сети"

![Символ монитора производительности сети](media/network-performance-monitor-faq/npm-symbol.png)

В этой статье собраны часто задаваемые вопросы о решении "Монитор производительности сети" в Azure

[Монитор производительности сети](/azure/networking/network-monitoring-overview) — это облачное [гибридное решение для сетевого мониторинга](../../azure-monitor/insights/network-performance-monitor-performance-monitor.md), которое позволяет отслеживать производительность сети между разными точками в сетевой инфраструктуре. Это решение также помогает отслеживать сетевое подключение к [конечным точкам служб и приложений](../../azure-monitor/insights/network-performance-monitor-service-connectivity.md), а также [производительность Azure ExpressRoute](../../azure-monitor/insights/network-performance-monitor-expressroute.md). 

Монитор производительности сети обнаруживает проблемы сети, такие как появление "черных дыр" в трафике, ошибки маршрутизации и проблемы, которые невозможно обнаружить с помощью традиционных методов мониторинга сети. Решение создает предупреждения и уведомляет о превышении порогового значения для сетевого соединения. Кроте того, оно обеспечивает своевременное обнаружение проблем с производительностью сети и определяет конкретный сегмент сети или сетевое устройство как источник проблемы. 

Дополнительные сведения о различных возможностях, поддерживаемых решение "Монитор производительности сети", см. в статье [Решения для мониторинга сетей](https://docs.microsoft.com/azure/networking/network-monitoring-overview).

## <a name="set-up-and-configure-agents"></a>Установка и настройка агентов

### <a name="what-are-the-platform-requirements-for-the-nodes-to-be-used-for-monitoring-by-npm"></a>Каковы требования к платформе для узлов, которые будут использоваться решением "Монитор производительности сети"?
Ниже перечислены требования к платформе для разных функций решения "Монитор производительности сети".

- NPM мониторинга производительности и возможности подключения монитор службы поддержки Windows server и Windows настольных компьютеров и клиентских операционных системах. Поддерживаемые версии операционной системы Windows, 2008 с пакетом обновления 1 или более поздней версии. Поддерживаемые версии рабочие столы или клиента Windows, Windows 10, Windows 8.1, Windows 8 и Windows 7. 
- Мониторинг ExpressRoute, входящий в состав решения "Монитор производительности сети", поддерживает только операционную систему Windows Server (2008 с пакетом обновления 1 или более поздней версии).

### <a name="can-i-use-linux-machines-as-monitoring-nodes-in-npm"></a>Можно ли использовать компьютеры Linux в качестве узлов мониторинга в решении "Монитор производительности сети"?
Возможность наблюдать за сетями с помощью узлы под управлением Linux в настоящее время доступна Предварительная версия. Дополнительные сведения вы можете получить у своего менеджера. Агенты Linux поддерживают при работе с решением "Монитор производительности сети" только функцию мониторинга производительности. Мониторинг подключений службы и (или) ExpressRoute не поддерживается.

### <a name="what-are-the-size-requirements-of-the-nodes-to-be-used-for-monitoring-by-npm"></a>Каковы требования к размеру для узлов, которые будут использоваться решением "Монитор производительности сети"?
Чтобы решение "Монитор производительности сети" работало на виртуальных машинах узла и выполняло мониторинг сети, этот узел должен располагать как минимум 500 МБ памяти и одним ядром. Не нужно использовать отдельные узлы для запуска NPM. Это решение может выполняться на узлах, которые выполняют другие рабочие нагрузки. Решение имеет возможность остановить процесс наблюдения, если оно использует более 5% ресурсов ЦП.

### <a name="to-use-npm-should-i-connect-my-nodes-as-direct-agent-or-through-system-center-operations-manager"></a>Нужно ли подключать узлы как прямой агент? Или же для решения "Монитор производительности сети" следует использовать System Center Operations Manager?
Монитор производительности и возможности монитор подключения службы поддерживает узлы [подключиться в качестве прямых агентов](../../azure-monitor/platform/agent-windows.md) и [подключен через Operations Manager](../../azure-monitor/platform/om-agents.md).

Для функции мониторинга ExpressRoute узлы Azure должны быть обязательно подключены как прямые агенты. Эта функция не поддерживает узлы Azure, подключенные через Operations Manager. Для локальных узлов узлов, подключенных как прямые агенты и через Operations Manager поддерживаются для наблюдения за канал ExpressRoute.

### <a name="which-protocol-among-tcp-and-icmp-should-be-chosen-for-monitoring"></a>Какой протокол лучше выбрать для мониторинга — TCP или ICMP?
Если вы отслеживаете сети, используя узлы под управлением сервера Windows, мы рекомендуем использовать протокол TCP как протокол мониторинга, так как он обеспечивает большую точность. 

ICMP рекомендуется для настольных компьютеров под управлением Windows и узлов на базе клиентской операционной системы. Does'nt этой платформы позволяют TCP данные, отправляемые через незащищенные сокеты, в которых NPM использует для обнаружения топологии сети.

Дополнительные сведения о сравнительных преимуществах протоколов вы можете получить [здесь](../../azure-monitor/insights/network-performance-monitor-performance-monitor.md#choose-the-protocol).

### <a name="how-can-i-configure-a-node-to-support-monitoring-using-tcp-protocol"></a>Как настроить узел для поддержки мониторинга по протоколу TCP?
Чтобы узел поддерживал мониторинг по протоколу TCP, нужно сделать следующее: 
* Убедитесь, что узел использует платформу Windows Server (2008 с пакетом обновления 1 или более поздней версии).
* Выполните на этом узле скрипт Powershell [EnableRules.ps1](https://aka.ms/npmpowershellscript). Дополнительные сведения см. в разделе [инструкций](../../azure-monitor/insights/network-performance-monitor.md#configure-log-analytics-agents-for-monitoring).


### <a name="how-can-i-change-the-tcp-port-being-used-by-npm-for-monitoring"></a>Как изменить порт TCP, используемый решением "Монитор производительности сети"?
Чтобы изменить TCP-порт, используемый решением "Монитор производительности сети", выполните скрипт [EnableRules.ps1](https://aka.ms/npmpowershellscript). В качестве параметра передайте ему новый номер порта, который вы намерены использовать. Например, для переключения TCP на порт 8060 запустите команду `EnableRules.ps1 8060`. Следите за тем, чтобы использовать один и тот же порт TCP на всех узлах, участвующих в мониторинге.

Сценарий настраивает брандмауэр Windows только локально. Если вы используете брандмауэр сети или правила группы безопасности сети (NSG), разрешите на них входящий трафик к TCP-порту, который настроен для решения "Монитор производительности сети".

### <a name="how-many-agents-should-i-use"></a>Сколько агентов нужно использовать?
Следует настроить хотя бы один агент для каждой подсети, к которой применяется мониторинг.

### <a name="what-is-the-maximum-number-of-agents-i-can-use-or-i-see-error--youve-reached-your-configuration-limit"></a>Каково максимальное число агентов, которые можно использовать, или я вижу ошибку «... Вы достигли предела по конфигурации»?
NPM ограничивает количество IP-адресов до 5000 IP-адресов на каждую рабочую область. Если у узла есть IP-адреса версий 4 и 6, то считается, что для этого узла задано два IP-адреса. Таким образом, это ограничение в 5000 IP-адресов определяет максимально допустимое число агентов. Неактивных агентов можно удалить на вкладке "Узлы" в разделе NPM > "Настройка". NPM также ведет журнал всех IP-адресов, которые были когда-либо назначены виртуальной Машины с агентом, и каждый считается как отдельный IP-адрес, влияющие на этот верхний предел 5000 IP-адресов. Чтобы освободить IP-адресов для рабочей области, страница узлов удалить IP-адресов, не используются.

## <a name="monitoring"></a>Мониторинг

### <a name="how-are-loss-and-latency-calculated"></a>Как вычисляются потери и задержки?
Агенты-источники отправляют запросы TCP SYN (если выбран протокол мониторинга TCP) или ICMP ECHO (если выбран протокол мониторинга ICMP) по IP-адресу назначения с регулярными интервалами. Этот механизм позволяет проверить все пути между разными сочетаниями IP-адресов источников и назначений. Для вычисления потерь и задержки по каждому пути измеряется процент полученных пакетов и время кругового пути для этих пакетов. Такие данные собираются на протяжении всего интервала опроса и по всем путям. На их основе вычисляются статистические значения потерь и задержек для конкретных сочетаний IP-адресов за конкретные интервалы опроса.

### <a name="with-what-frequency-does-the-source-agent-send-packets-to-the-destination-for-monitoring"></a>С какой частотой агенты-источники отправляют по адресу назначения пакеты для мониторинга?
Для функций монитора производительности и монитора ExpressRoute каждый источник отправляет пакеты каждые 5 секунд и фиксирует измеренные данные. Эти данные агрегируются по 3-минутным интервалам опроса, для которых вычисляются среднее и пиковые значения потерь и задержек. Что касается монитора подключений службы, частота отправки пакетов для анализа сети зависит от частоты, введенной пользователем при настройке конкретного теста.

### <a name="how-many-packets-are-sent-for-monitoring"></a>Сколько пакетов отправляется для мониторинга?
Количество пакетов, отправленных агентом-источником по адресу назначения в процессе опроса зависит от сетевой топологии. Для корректировки мы используем специальный алгоритм собственной разработки. В среднем, чем больше сетевых путей может использоваться между каждым сочетанием IP-адресов источника и назначения, тем больше отправляется пакетов. Система гарантирует, что в анализ включаются все пути между сочетаниями IP-адресов источника и назначения.

### <a name="how-does-npm-discover-network-topology-between-source-and-destination"></a>Как решение "Монитор производительности сети" определяет топологию сети между источником и назначением?
Решение "Монитор производительности сети" использует наш собственный алгоритм, основанный на команде Traceroute, для обнаружения всех путей и прыжков между источником и назначением.

### <a name="does-npm-provide-routing-and-switching-level-info"></a>Предоставляет ли решение "Монитор производительности сети" сведения об уровне маршрутизации и коммутации? 
Решение "Монитор производительности сети" может обнаруживать все возможные маршруты между агентом-источником и назначением. Но сведения о том, какой путь использовался для пакетов от конкретной рабочей нагрузки, не предоставляются. Зато это решение поможет вам выявить возможные пути и прыжки в сети на более низком уровне, по которым задержки превышают ожидаемый показатель.

### <a name="why-are-some-of-the-paths-unhealthy"></a>Какие могут быть причины плохой работоспособности путей?
Между IP-адресами источника и назначения может быть много разных сетевых путей, для каждого из которых будут получены разные значения потерь и задержек. Решение "Монитор производительности сети" помечает как неработоспособные (и обозначает красным цветом) пути, для которых измеренные значения потерь и (или) задержек оказались выше порогового значения, заданного в конфигурации мониторинга.

### <a name="what-does-a-hop-in-red-color-signify-in-the-network-topology-map"></a>Что означает красный цвет прыжка на карте топологии сети?
Если прыжок выделен красным цветом, значит он входит хотя бы в один неработоспособный путь. Решение "Монитор производительности сети" только помечает неработоспособные пути, но не анализирует состояние работоспособности подробно для каждого пути. Чтобы определить прыжки, создающие проблемы, просмотрите значения задержки для отдельных узлов и выделите среди них те, на которых задержка увеличивается больше ожидаемого.

### <a name="how-does-fault-localization-in-performance-monitor-work"></a>Как работает определение расположения ошибок в мониторе производительности?
Решение "Монитор производительности сети" использует вероятностный механизм для присвоения значений вероятности сбоя каждому сетевому пути, сегменту сети и входящим в них сетевым прыжком. Эта оценка основывается на количестве неработоспособных путей, частью которых является конкретный элемент. Чем больше неработоспособных путей проходит через определенный сегмент сети или содержит определенный прыжок, тем более высокая вероятность сбоя им присваивается. Этот алгоритм лучше всего работает при наличии большого числа подключенных друг к другу узлов с установленным агентом решения "Монитор производительности сети", так как это дает больше точек данных для вычисления вероятностей сбоя.

### <a name="how-can-i-create-alerts-in-npm"></a>Как создавать оповещения в решении "Монитор производительности сети"?
Изучите пошаговые инструкции из [раздела документации, посвященного оповещениям](https://docs.microsoft.com/azure/log-analytics/log-analytics-network-performance-monitor#alerts).

### <a name="can-npm-monitor-routers-and-servers-as-individual-devices"></a>Может ли решение "Монитор производительности сети" отслеживать маршрутизаторы и серверы как отдельные устройства?
Решение "Монитор производительности сети" определяет IP-адреса и имена узлов для всех сетевых прыжков (коммутаторы, маршрутизаторы, серверы и др.) между IP-адресами источника и назначения. Также он вычисляет задержку между обнаруженными прыжками. Но мониторинг на уровне каждого прыжка не выполняется.

### <a name="can-npm-be-used-to-monitor-network-connectivity-between-azure-and-aws"></a>Можно ли использовать решение "Монитор производительности сети" для мониторинга сетевого подключения между Azure и AWS?
Да. Дополнительные сведения см. в статье о [мониторинге Azure, AWS и локальных сетей с помощью решения "Монитор производительности сети"](https://blogs.technet.microsoft.com/msoms/2016/08/30/monitor-on-premises-cloud-iaas-and-hybrid-networks-using-oms-network-performance-monitor/).

### <a name="is-the-expressroute-bandwidth-usage-incoming-or-outgoing"></a>Какая пропускная способность ExpressRoute учитывается в оценке потребления — входящая или исходящая?
Потребление пропускной способности оценивается по сумме входящей и исходящей пропускной способности. Этот параметр выражается в битах в секунду.

### <a name="can-we-get-incoming-and-outgoing-bandwidth-information-for-the-expressroute"></a>Могу ли я получить сведения о входящей и исходящей пропускной способности для ExpressRoute?
Есть возможность собирать данные о входящей и исходящей пропускной способности для основного и вторичного подключений.

Чтобы получить информацию на уровне пиринга, выполните в поиске по журналам указанный ниже запрос.

    NetworkMonitoring 
    | where SubType == "ExpressRoutePeeringUtilization"
    | project CircuitName,PeeringName,PrimaryBytesInPerSecond,PrimaryBytesOutPerSecond,SecondaryBytesInPerSecond,SecondaryBytesOutPerSecond
  
Чтобы получить информацию на уровне цепи, выполните указанный ниже запрос. 

    NetworkMonitoring 
    | where SubType == "ExpressRouteCircuitUtilization"
    | project CircuitName,PrimaryBytesInPerSecond, PrimaryBytesOutPerSecond,SecondaryBytesInPerSecond,SecondaryBytesOutPerSecond

### <a name="which-regions-are-supported-for-npms-performance-monitor"></a>Какие регионы поддерживаются для монитора производительности, входящего в решение "Монитор производительности сети"?
Решение "Монитор производительности сети" может отслеживать подключение между сетями, расположенными в любых частях мира, если он размещен в рабочей области в одном из [поддерживаемых регионов](../../azure-monitor/insights/network-performance-monitor.md#supported-regions).

### <a name="which-regions-are-supported-for-npms-service-connectivity-monitor"></a>Какие регионы поддерживаются для монитора подключений службы, входящего в решение "Монитор производительности сети"?
Решение "Монитор производительности сети" может отслеживать подключение к службам, расположенными в любых частях мира, если он размещен в рабочей области в одном из [поддерживаемых регионов](../../azure-monitor/insights/network-performance-monitor.md#supported-regions).

### <a name="which-regions-are-supported-for-npms-expressroute-monitor"></a>Какие регионы поддерживаются для монитора ExpressRoute, входящего в решение "Монитор производительности сети"?
Решение "Монитор производительности сети" может отслеживать каналы ExpressRoute, расположенные в любом регионе Azure. Чтобы подключить канал к решению "Монитор производительности сети", нужно создать рабочую область Log Analytics в одном из [поддерживаемых регионов](/azure/expressroute/how-to-npm).

## <a name="troubleshoot"></a>Устранение неполадок

### <a name="why-are-some-of-the-hops-marked-as-unidentified-in-the-network-topology-view"></a>Почему некоторые прыжки в представлении топологии сети обозначены как "не определено"?
Решение "Монитор производительности сети" использует модифицированную версию команды traceroute для определения топологии сети между агентом-источником и назначением. Неопределенным считается прыжок, который не предоставил ответ на запрос traceroute от агента-источника. Если три последовательных сетевых переходов не отвечают на "Traceroute" агента, решение помечает отвечать на запросы прыжков как идентифицированы и предпринимается попытка найти дополнительные Прыжки.

Прыжок может не отвечать на запросы traceroute по одной или нескольким причинам из перечисленных ниже:

* настройки маршрутизатора запрещают ему идентифицировать себя;
* сетевые устройства не пропускают трафик типа ICMP_TTL_EXCEEDED;
* брандмауэр блокирует ответ ICMP_TTL_EXCEEDED от сетевого устройства.

### <a name="why-does-my-link-show-unhealthy-but-the-topology-does-not"></a>Почему неработоспособное демонстрации ссылку, но топологии не 
NPM отслеживает end-to-end потери, задержки и топологии с разными интервалами. Потери и задержки измеряются один раз каждые 5 секунд и статистическая обработка проводится каждые три минуты (для мониторинга производительности и Express Route мониторинга) пока топологии вычисляется с помощью "Traceroute" каждые 10 минут. Например, между 3:44 и 4:04, топологии могут быть обновлены три раза (3:44, 3:54, 4:04), но потери и задержки обновляются примерно 7 секунд (3:44, 3:47, 3:50, 3:53, 3:56, 3:59 4:02). Топологии, созданные на 3:54 будет изображен на потери и задержки, который получает рассчитан на 3:56, 3:59 и 4:02. Предположим, что вы получаете оповещение, что к каналу ER неработоспособности в 3:59. Выполните вход на NPM и попытаться установить время топологии до 3:59. NPM будет отображаться топологии, созданные на 3:54. Сведения о последней известной топологию сети, сравнение полей TimeProcessed (время в какие потери и задержки были вычислены) и TracerouteCompletedTime (время в свою топологию, были вычислены). 

### <a name="what-is-the-difference-between-the-fields-e2emedianlatency-and-avghoplatencylist-in-the-networkmonitoring-table"></a>Какова разница между полями E2EMedianLatency и AvgHopLatencyList в таблице NetworkMonitoring
E2EMedianLatency — задержка обновления каждые три минуты после статистической обработки результаты проверок связи tcp, тогда как AvgHopLatencyList обновления каждые 10 минут, в зависимости от "Traceroute". Чтобы понять точное время, по которому были вычислены E2EMedianLatency, используйте поле TimeProcessed. Чтобы понять точное время завершения какие traceroute и обновленные AvgHopLatencyList, используйте поле TracerouteCompletedTime

### <a name="why-does-hop-by-hop-latency-numbers-differ-from-hoplatencyvalues"></a>Почему показатели задержки по прыжкам отличается от HopLatencyValues 
HopLatencyValues являются источником к конечной точке.
Например: Число прыжков - A, B, C. AvgHopLatency - 10,15,20. Это означает, что источник задержкой = 10, источника к задержке B = 15 и источника к задержке C — 20. Пользовательский Интерфейс будет вычислять задержка прыжков A-B 5 в топологии

### <a name="the-solution-shows-100-loss-but-there-is-connectivity-between-the-source-and-destination"></a>Решение показывает 100 % потерь, но связь между источником и назначением работает нормально. Что происходит?
Такая ситуация возникает, если брандмауэр узла или промежуточный брандмауэр (сетевой брандмауэр или группа безопасности сети Azure) блокирует обмен данными между агентом-источником и назначением по тому порту, который назначен для решения "Монитор производительности сети" (по умолчанию используется порт 8084, если клиент не изменял этот параметр).

* Чтобы проверить, блокирует ли брандмауэр обмен данными по используемому порту, просмотрите состояния работоспособности для узлов источника и назначения в представлении: "Монитор производительности сети -> Конфигурация -> Узлы". 
  Если они отмечены как неработоспособные, просмотрите инструкции и примите меры по исправлению. Если узлы находятся в работоспособном состоянии, перейдите к шагу "b". ниже.
* Чтобы убедиться, что промежуточный сетевой брандмауэр или группа безопасности сети Azure не блокируют обмен данными по используемому порту, примените служебную программу PsPing стороннего разработчика в соответствии с приведенными ниже инструкциями.
  * Служебную программу PsPing можно скачать [здесь](https://technet.microsoft.com/sysinternals/psping.aspx). 
  * Выполните приведенную ниже команду на узле источника.
    * psping -n 15 \<IP-адрес узла назначения\>:номер_порта (по умолчанию решение "Монитор производительности сети" использует порт 8084). Если вы явным образом изменяли это значение с помощью скрипта EnableRules.ps1, введите фактически настроенный номер порта. Это проверка связи от компьютера Azure до локального узла.
* Убедитесь, что проверка проходит успешно. Если это не так, значит, трафик на этот порт блокируется промежуточным сетевым брандмауэром и (или) группой безопасности сети.
* Теперь выполните аналогичную команду на узле назначения, указав IP-адрес источника.


### <a name="there-is-loss-from-node-a-to-b-but-not-from-node-b-to-a-why"></a>Я наблюдаю потери при передаче данных из узла A в узел B, но не из B в A. В чем может быть причина?
Возможно, используются разные сетевые пути для передачи данных из A в B и из B в A. При этом значения потерь и задержек могут оказаться разными.

### <a name="why-are-all-my-expressroute-circuits-and-peering-connections-not-being-discovered"></a>Почему не выполняется обнаружение всех моих каналов ExpressRoute и пиринговых подключений?
Решение "Монитор производительности сети" теперь обнаруживает каналы ExpressRoute и пиринговые подключения во всех подписках, к которым у пользователя есть доступ. Выберите все подписки, где есть ссылки на ресурсы Express Route и включите мониторинг для каждого обнаруженного ресурса. При обнаружении частного пиринга решение "Монитор производительности сети" ищет подключенные объекты, поэтому убедитесь, что виртуальная сеть связана с пирингом.

### <a name="the-er-monitor-capability-has-a-diagnostic-message-traffic-is-not-passing-through-any-circuit-what-does-that-mean"></a>При использовании функции мониторинга ExpressRoute отображается диагностическое сообщение "Traffic is not passing through ANY circuit" (Не передается трафик через любой канал). Что это означает?

Может сложиться такая ситуация, в которой между локальными узлами и Azure есть работоспособное подключение, но трафик не проходит через канал ExpressRoute, который должен отслеживаться решением "Монитор производительности сети". 

Причины могут быть следующими:

* Канал ExpressRoute не работает.
* Фильтры маршрутов настроены таким образом, что приоритет имеет не отслеживаемый канал ExpressRoute, а другие маршруты (например, VPN-подключение или другой канал ExpressRoute). 
* Между локальными узлами и узлами Azure, настроенными для мониторинга канала ExpressRoute, нет подключения через отслеживаемый канал ExpressRoute. Убедитесь, что вы правильно выбрали узлы и у них есть возможность связи друг с другом через канал ExpressRoute, который вы хотите отслеживать.

### <a name="while-configuring-monitoring-of-my-expressroute-circuit-the-azure-nodes-are-not-being-detected"></a>При настройке мониторинга для канала ExpressRoute не удается обнаружить узлы Azure.
Такое может случиться, если узлы Azure подключены через Operations Manager. Функция мониторинга ExpressRoute поддерживает только узлы Azure, которые подключены как прямые агенты.

### <a name="i-cannot-discover-by-expressroute-circuits-in-the-oms-portal"></a>Почему мне не удается обнаружить каналы ExpressRoute на портале OMS?
Решение "Монитор производительности сети" можно использовать и на портале Azure, и на портале OMS. Но функция обнаружения каналов в ExpressRoute Monitor работает только на портале Azure. Успешно выполнив обнаружение каналов через портал Azure, вы можете использовать остальные функции на любом из двух порталов. 

### <a name="in-the-service-connectivity-monitor-capability-the-service-response-time-network-loss-as-well-as-latency-are-shown-as-na"></a>В мониторе подключений службы отображаются значения "недоступно" для всех параметров отклика службы, потери пакетов в сети и задержек.
Такое может происходить при любом из следующих условий:

* Служба не работает.
* Узел, используемый для проверки сетевого подключения к службе, не работает.
* Неверный целевой объект, введенный в конфигурации теста.
* У узла нет сетевых подключений.

### <a name="in-the-service-connectivity-monitor-capability-a-valid-service-response-time-is-shown-but-network-loss-as-well-as-latency-are-shown-as-na"></a>В мониторе подключений службы отображаются разумные значения параметров отклика службы, но значения "недоступно" для параметров потери пакетов в сети и задержек.
 Такое может происходить при любом из следующих условий:

* Если узел, используемый для проверки сетевых подключений к службе, является клиентским компьютером Windows, целевая служба блокирует запросы ICMP или сетевой брандмауэр блокирует запросы ICMP, исходящие из узла.
* В конфигурации теста не установлен флажок "Проведите измерения сети".

### <a name="in-the-service-connectivity-monitor-capability-the-service-response-time-is-na-but-network-loss-as-well-as-latency-are-valid"></a>В мониторе подключений службы отображаются значения "недоступно" для параметра отклика службы, хотя потери пакетов в сети и задержки отображаются правильно.
Это может происходить, если целевая служба не является веб-приложением, а тест для нее настроен как веб-тест. Измените конфигурацию теста, выбрав для него тип "Сеть" вместо "Веб".

## <a name="miscellaneous"></a>Прочее

### <a name="is-there-a-performance-impact-on-the-node-being-used-for-monitoring"></a>Как мониторинг влияет на производительность узла, на котором он настроен?
Решение "Монитор производительности сети" будет прекращать работу, если оно использует более 5 % ресурсов ЦП узла. Благодаря этому узлы можно всегда использовать для обычных рабочих нагрузок без ущерба для производительности.

### <a name="does-npm-edit-firewall-rules-for-monitoring"></a>Изменяет ли решение "Монитор производительности сети" правила брандмауэра?
Решение "Монитор производительности сети" создает только одно локальное правило в брандмауэре Windows на тех узлах, где выполняется скрипт Powershell EnableRules.ps1. Это правило разрешает агентам создавать TCP-подключения друг с другом по указанному порту. Это решение никак не изменяет все остальные правила брандмауэра и (или) правил группы безопасности сети.

### <a name="how-can-i-check-the-health-of-the-nodes-being-used-for-monitoring"></a>Как проверить работоспособность узлов, используемых для мониторинга?
Состояние работоспособности узлов, используемых для мониторинга, вы можете проверить с помощью представления: "Монитор производительности сети -> Конфигурация -> Узлы". Если вы найдете здесь узел в неработоспособном состоянии, просмотрите сведения об ошибке и выполните предложенные меры по исправлению.

### <a name="can-npm-report-latency-numbers-in-microseconds"></a>Можно ли в решении "Монитор производительности сети" узнать значения задержки в микросекундах?
Решение "Монитор производительности сети" округляет показатели задержки в пользовательском интерфейсе до миллисекунд. Но эти данные хранятся с большей точностью (иногда до четырех десятичных разрядов).

## <a name="next-steps"></a>Следующие шаги

- Дополнительные сведения о решении "Монитор производительности сети" в Azure вы можете получить [в этой статье](../../azure-monitor/insights/network-performance-monitor.md).
