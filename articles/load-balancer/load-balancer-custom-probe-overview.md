---
title: Используйте пробы работоспособности Load Balancer для масштабирования и обеспечения высокого уровня доступности вашей службы
titlesuffix: Azure Load Balancer
description: Сведения об использовании проб работоспособности для мониторинга экземпляров, находящихся за Load Balancer
services: load-balancer
documentationcenter: na
author: asudbring
manager: twooley
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.custom: seodec18
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/07/2019
ms.author: allensu
ms.openlocfilehash: 75009530940a0cce7adb8469ead5f55f509a1faa
ms.sourcegitcommit: 9a699d7408023d3736961745c753ca3cec708f23
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/16/2019
ms.locfileid: "68275335"
---
# <a name="load-balancer-health-probes"></a>Пробы работоспособности Load Balancer

Azure Load Balancer предоставляет пробы работоспособности для использования с правилами балансировки нагрузки.  Конфигурация пробы работоспособности и ее результаты определяют, какие экземпляры внутреннего пула должны получить новые потоки. Пробы работоспособности можно использовать для обнаружения сбоя приложения в экземпляре серверной части. Вы также можете создать пользовательский ответ на пробу работоспособности и использовать ее для управления потоком, чтобы управлять нагрузкой или запланированным временем простоя. Если проба работоспособности выдает сбой, Load Balancer прекращает отправлять новые потоки в соответствующий неработоспособный экземпляр.

Пробы работоспособности поддерживают множественные протоколы. Доступность определенного типа пробы работоспособности для поддержки определенного протокола зависит от номера SKU Load Balancer.  Кроме того, реакция службы зависит от номера SKU Load Balancer.

| | SKU "Стандартный" | SKU "Базовый" |
| --- | --- | --- |
| [Типы проб](#types) | TCP, HTTP, HTTPS | TCP, HTTP |
| [Реакция на сбой пробы работоспособности](#probedown) | При сбое всех проб работоспособности потоки TCP продолжаются. | Все вызонды выключаются, истекает срок действия всех TCP-потоков. | 

> [!IMPORTANT]
> Пробы работоспособности Load Balancer поступают с IP-адреса 168.63.129.16 и не должны блокироваться, чтобы они отметили экземпляр как работоспособный.  Дополнительные сведения см. в разделе [IP-адрес источника пробы](#probesource).

## <a name="types"></a>

Пробу работоспособности можно настроить для прослушивателей, используя такие три протокола:

- [прослушиватели TCP](#tcpprobe);
- [конечные точки HTTP](#httpprobe);
- [конечные точки HTTPS](#httpsprobe);

Доступные типы проб работоспособности зависят от выбранного SKU Load Balancer:

|| TCP | HTTP | HTTPS |
| --- | --- | --- | --- |
| SKU "Стандартный" |    &#9989; |   &#9989; |   &#9989; |
| SKU "Базовый" |   &#9989; |   &#9989; | &#10060; |

Вне зависимости от того, какой тип пробы работоспособности вы выберете, пробы работоспособности могут наблюдать за любым портом во внутреннем экземпляре, включая порт, на котором предоставляется фактическое обслуживание.

### <a name="tcpprobe"></a>Проба TCP

Проверки TCP инициализируют подключение, выполняя трехстороннее подтверждение по открытому TCP-протоколу на определенном порту.  Пробы протокола TCP завершают подключение с помощью закрытого четырехстороннего подтверждения протокола TCP.

Минимальный интервал проверки составляет 5 секунд, а минимальное количество ответов о неработоспособности — 2.  Общая продолжительность всех интервалов не может превышать 120 секунд.

Проба TCP завершается сбоем в следующих случаях:
* Прослушиватель TCP в экземпляре не отвечает в течение периода времени ожидания.  Проба отмечается как неработоспособная в зависимости от количества неудачных запросов на пробу, которых достаточного для того, чтобы проба была отмечена как неработоспособная.
* Проба получает сброс TCP-соединения от экземпляра.

#### <a name="resource-manager-template"></a>Шаблон Resource Manager

```json
    {
      "name": "tcp",
      "properties": {
        "protocol": "Tcp",
        "port": 1234,
        "intervalInSeconds": 5,
        "numberOfProbes": 2
      },
```

### <a name="httpprobe"></a> <a name="httpsprobe"></a>Проба HTTP/HTTPS

> [!NOTE]
> Проба HTTPS доступна только для [Load Balancer (цен. категория "Стандартный")](load-balancer-standard-overview.md).

Пробы HTTP и HTTPS основываются на пробе протокола TCP и выдают запрос HTTP GET с указанным расположением. Обе пробы поддерживают относительные пути для HTTP GET. Пробы HTTPS аналогичны пробам HTTP, но к ним добавлены оболочки TLS (это протокол ранее назывался SSL). Проба работоспособности отмечается как работоспособная, когда экземпляр отвечает с HTTP-состоянием 200 в течение периода ожидания.  По умолчанию эта проба работоспособности пытается проверить настроенный порт каждые 15 секунд. Минимальный интервал проверки составляет 5 секунд. Общая продолжительность всех интервалов не может превышать 120 секунд.

Пробы HTTP / HTTPS также могут быть полезны, если вы хотите выразить пробу работоспособности.  реализовать собственную логику для удаления экземпляров из поворотов балансировщика нагрузки, если порт для пробы также является прослушивателем самой службы. Например, можно удалить экземпляр, если он использует более 90 % ресурсов ЦП и возвращает состояние HTTP, отличное от кода 200. 

Если вы используете облачные службы и у вас есть веб-роли, использующие w3wp.exe, вам доступен также автоматический мониторинг веб-сайта. Ошибки в коде веб-сайта возвращают проверке подсистемы балансировки нагрузки состояние с кодом, отличным от 200.

Проба HTTP/HTTPS завершается сбоем в следующих случаях:
* Конечная точка пробы возвращает код ответа HTTP, отличный от 200 (например, 403, 404 или 500). В этом случае проба будет сразу же отмечена как неработоспособная. 
* Конечная точка пробы не отвечает в течение 31-секундного периода ожидания. Множество запросов на пробу могут оставаться без ответа до того, как проба будет отмечена как неработоспособная, и пока не будет достигнуто суммы всех интервалов времени ожидания.
* Конечная точка пробы закрывает подключение путем сброса TCP-соединения.

#### <a name="resource-manager-templates"></a>Шаблоны Resource Manager

```json
    {
      "name": "http",
      "properties": {
        "protocol": "Http",
        "port": 80,
        "requestPath": "/",
        "intervalInSeconds": 5,
        "numberOfProbes": 2
      },
```

```json
    {
      "name": "https",
      "properties": {
        "protocol": "Https",
        "port": 443,
        "requestPath": "/",
        "intervalInSeconds": 5,
        "numberOfProbes": 2
      },
```

### <a name="guestagent"></a>Проба гостевого агента (только для классической версии)

Роли облачной службы (рабочие роли и веб-роли) используют гостевой агент для мониторинга проб по умолчанию.  Проба гостевого агента является последней инстанцией.  Всегда явно используйте пробу работоспособности как пробу TCP или HTTP. Проба гостевого агента не так эффективна, как явно определенные пробы для большинства сценариев приложений.

Проба гостевого агента представляет собой проверку гостевого агента внутри виртуальной машины. Затем она ожидает передачу данных и передает ответ HTTP с кодом 200 (ОК) только в том случае, если экземпляр находится в состоянии готовности. (Другие возможные состояния: занято, перезапуск или остановлено.)

Дополнительные сведения см. в статьях [Azure Service Configuration Schema (.cscfg File)](https://msdn.microsoft.com/library/azure/ee758710.aspx) (Схема конфигурации службы Azure (CSCFG-файл)) и [Приступая к работе по созданию общедоступного балансировщика нагрузки для облачных служб](load-balancer-get-started-internet-classic-cloud.md#check-load-balancer-health-status-for-cloud-services).

Если гостевой агент не отправляет ответ HTTP с кодом 200 (ОК), подсистема балансировки нагрузки отмечает экземпляр как неработоспособный. Затем он прекращает отправлять потоки к этому экземпляру. Подсистема балансировки нагрузки продолжит проверять связь с этим экземпляром. 

Если гостевой агент отправляет ответ HTTP с кодом 200, подсистема балансировки нагрузки снова начинает отправлять потоки к этому экземпляру.

При использовании веб-роли код веб-сайта обычно выполняется в процессе w3wp.exe, который не отслеживается структурой Azure или гостевым агентом. Ошибки в w3wp.exe (например, ответы HTTP с кодом 500) не передаются в гостевой агент. Следовательно, подсистема балансировки нагрузки не убирает этот экземпляр из ротации.

<a name="health"></a>
## <a name="probehealth"></a>Проба работоспособности

Пробы работоспособности TCP, HTTP и HTTPS считаются исправными и отмечают экземпляр роли как работоспособный в следующих случаях:

* Проба работоспособности выполнена успешно уже после загрузки виртуальной машины.
* Достигнуто указанное количество проб, необходимых для отметки экземпляра роли как работоспособного.

> [!NOTE]
> Если проба работоспособности постоянно меняется, подсистема балансировки нагрузки ожидает более длительное время перед переводом экземпляра роли обратно в работоспособное состояние. Это дополнительное время ожидания защищает пользователя и инфраструктуру и преднамеренно заложено в политику.

## <a name="probe-count-and-timeout"></a>Количество проб и время ожидания

Поведение проверки зависит от следующих параметров:

* количества успешных проб, достаточного для того, чтобы пометить экземпляр как работоспособный;
* количества неудачных проверок, достаточного для того, чтобы пометить экземпляр как неработоспособный.

Заданные значения времени ожидания и интервала определяют, будет ли экземпляр отмечен как "включен" или "отключен".

## <a name="probedown"></a>Реакция на сбой проверки работоспособности

### <a name="tcp-connections"></a>TCP-подключения

Новые соединения протокола TCP преуспеют в сохранении работоспособных экземпляров сервера.

При сбое пробы работоспособности для экземпляра серверной части установленные к нему TCP-подключения прерваны не будут.

При сбое в серверном пуле всех проб для всех экземпляров новые потоки в этот пул передаваться не будут. Load Balancer (цен. категория "Стандартный") позволит поддерживать установленные потоки TCP.  Load Balancer (цен. категория "Базовый") завершит передачу всех имеющихся потоков TCP в серверный пул.
 
Load Balancer – это сквозная служба (не прерывает соединения протокола TCP), и поток всегда находится между клиентом, гостевой ОС виртуальной машины и приложением. Пул со всеми пробами, завершившимися сбоем, приведет к отсутствию ответа внешнего интерфейса на попытки открыть TCP-подключение (SYN) из-за отсутствия в серверной части работоспособного экземпляра, необходимого для получения потока и ответа с SYN-ACK.

### <a name="udp-datagrams"></a>Датаграммы UDP

Датаграммы UDP будут доставлены работоспособным экземплярам серверной части.

UDP не требует соединения, поэтому состояние этого потока не отслеживается. При сбое пробы работоспособности экземпляра серверной части имеющиеся UDP-потоки могут переместиться в другой работоспособный экземпляр в серверном пуле.

Если в серверном пуле все пробы всех экземпляров завершатся сбоем, будут прекращены все имеющиеся потоки UDP для служб Load Balancer (цен. категория "Базовый" и "Стандартный").

<a name="source"></a>
## <a name="probesource"></a>IP-адрес источника пробы

Балансировщик нагрузки использует распределенную службу проверки работоспособности для внутренней модели работоспособности. Служба проб находится на каждом узле, на котором размещаются виртуальные машины, и по требованию может быть запрограммирована для создания проверок работоспособности для каждой конфигурации клиента. Трафик проверок работоспособности находится непосредственно между службой проб, которая создает проверку, и виртуальной машиной клиента. Все пробы работоспособности Load Balancer поступают с IP-адреса 168.63.129.16. Он является их источником.  Вы можете использовать в виртуальной сети диапазон IP-адресов, который не является пространством RFC1918.  Использование глобально зарезервированного IP-адреса, принадлежащего Майкрософт, снижает вероятность конфликта IP-адресов с диапазоном IP-адресов, который вы используете в виртуальной сети.  Этот IP-адрес одинаков во всех регионах, он не меняется и не являет собой угрозу безопасности, поскольку с этого IP-адреса отправлять пакеты может только внутренний компонент платформы Azure. 

Тег службы AzureLoadBalancer определяет этот исходный IP-адрес в ваших [группах безопасности сети](../virtual-network/security-overview.md) и разрешает трафик проверки работоспособности по умолчанию.

В дополнение к Load Balancer пробы работоспособности [следующие операции используют этот IP-адрес](../virtual-network/what-is-ip-address-168-63-129-16.md):

- Позволяет агенту виртуальной машины взаимодействовать с платформой, чтобы сигнализировать, что он находится в состоянии "Готов".
- Включает связь с виртуальным сервером DNS для предоставления разрешения отфильтрованных имен для клиентов, которые не указывают настраиваемые DNS-серверы.  Эта фильтрация гарантирует, что клиенты могут разрешать имена узлов для их развертывания.
- Позволяет виртуальной машине получить динамический IP-адрес от службы DHCP в Azure.

## <a name="design"></a> Руководство по проектированию

Пробы работоспособности используются для повышения устойчивости вашей службы и ее масштабирования. Неправильная конфигурация или неправильный конструктивный шаблон могут повлиять на доступность и масштабируемость вашей службы. Просмотрите весь этот документ и подумайте, как повлияет на ваш сценарий то, что эта проба отмечена как "отключен" или "подключен" и как это влияет на доступность сценария вашего приложения.

Когда вы разрабатываете модель работоспособности для своего приложения, вы должны проверить на экземпляре сервера порт, который отражает работоспособность этого экземпляра __и__ предоставляемой вами службы приложения.  Порт приложения и порт пробы не должны обязательно совпадать.  В некоторых случаях желательно, чтобы порт пробы отличался от функционального порта вашего приложения.  

Иногда для вашего приложения может быть полезно сгенерировать ответ пробы работоспособности, чтобы не только определить его работоспособность, но и напрямую подать сигнал Load Balancer о том, должен ли ваш экземпляр получать или не получать новые потоки.  Вы можете использовать ответ пробы, чтобы позволить своему приложению создавать противодавление и регулировать поставку новых потоков в экземпляр путем проведения неудачной пробы работоспособности или подготовки к обслуживанию вашего приложения и инициирования опустошения вашего сценария.  При использовании Load Balancer (цен. категория "Стандартный") сигнал [пробы "отключен"](#probedown) будет всегда разрешать потокам протокола TCP продолжаться, пока не начнется время ожидания или не прервется подключение. 

Для балансировки нагрузки UDP вы должны сгенерировать пользовательский сигнал пробы работоспособности из экземпляра сервера и использовать пробу работоспособности TCP, HTTP или HTTPS, предназначенную для соответствующего прослушивателя, чтобы отразить работоспособность вашего приложения UDP.

При использовании [правил балансировки нагрузки портов HA](load-balancer-ha-ports-overview.md) с [Load Balancer (цен. категория "Стандартный")](load-balancer-standard-overview.md) на всех портах выполняется балансировка нагрузки, а в одном ответе пробы работоспособности должно отражаться состояние всего экземпляра.

Не преобразовывайте или не используйте прокси-сервер через экземпляр, который получает пробу работоспособности к другому экземпляру в виртуальной сети, так как эта конфигурация может привести к каскадным сбоям в сценарии.  Рассмотрим следующий сценарий. Набор сторонних устройств развернут в серверном пуле ресурса Load Balancer, чтобы обеспечить масштабирование и избыточность для устройств, а датчик работоспособности настроен на проверку порта, который стороннее устройство передает через прокси-сервер или преобразовывает на другие виртуальные машины вне устройства.  Если вы проверяете тот же порт, который используете для преобразования или прокси-запросов к другим виртуальным машинам вне устройства, любой результат пробы от одной виртуальной машины вне устройства отметит само устройство как неработоспособное. Эта конфигурация может привести к каскадному сбою всего сценария приложения в результате одного серверного экземпляра вне устройства.  Триггером может быть прерывистый сбой пробы, который приведет к тому, что Load Balancer будет отмечать исходное место назначения (экземпляр устройства) и, в свою очередь, может отключить весь сценарий приложения. Вместо этого проверьте работоспособность самого устройства. Выбор пробы для определения сигнала работоспособности является важным фактором для сценариев сетевых виртуальных устройств (NVA), и вы должны проконсультироваться с поставщиком приложения о том, какой сигнал работоспособности подходит для таких сценариев.

Если вы не разрешите [исходный IP-адрес](#probesource) пробы в политиках брандмауэра, то проба работоспособности завершится сбоем, поскольку она не может достичь вашего экземпляра.  В свою очередь, из-за сбоя пробы Load Balancer пометит экземпляр как неработоспособный.  Эта неверная конфигурация может привести к сбою сценария приложения с балансировкой нагрузки.

Чтобы проба Load Balancer отметила ваш экземпляр как работоспособный, **необходимо** разрешить этот IP-адрес во всех [группах безопасности сетей](../virtual-network/security-overview.md) Azure и политиках локального брандмауэра.  По умолчанию каждая группа безопасности сети включает [служебный тег](../virtual-network/security-overview.md#service-tags) AzureLoadBalancer для разрешения трафика пробы работоспособности.

Если вы хотите проверить сбой проверки работоспособности или выделить отдельный экземпляр, вы можете использовать [группы безопасности сети](../virtual-network/security-overview.md), чтобы явно заблокировать проверку работоспособности (порт назначения или [исходный IP-адрес](#probesource)) и моделировать сбой пробы.

Не настраивайте свою виртуальную сеть с диапазоном IP-адресов, принадлежащим корпорации Майкрософт, который содержит 168.63.129.16.  Такие конфигурации будут конфликтовать с IP-адресом пробы работоспособности и могут вызвать сбой вашего сценария.

Если на вашей виртуальной машине имеется несколько интерфейсов, нужно ответить на пробу в интерфейсе, куда поступил запрос.  Вам может потребоваться, чтобы исходный сетевой адрес преобразовал этот адрес в виртуальную машину для каждого интерфейса.

Не включайте [метки времени протокола TCP](https://tools.ietf.org/html/rfc1323).  Включение меток времени протокола TCP приведет к сбою проб работоспособности из-за того, что TCP-пакеты сбрасываются стеком гостевой ОС виртуальной машины, что приводит к тому, что Load Balancer отмечает соответствующую конечную точку.  Метки времени протокола TCP обычно включены по умолчанию на защищенных образах виртуальных машин и должны быть отключены.

## <a name="monitoring"></a>Мониторинг

Общедоступные и внутренние службы [Load Balancer (цен. категория "Стандартный")](load-balancer-standard-overview.md) предоставляют для каждой конечной точки и экземпляра серверной части состояние пробы работоспособности в качестве многомерных метрик в Azure Monitor. Эти метрики можно использовать в других службах Azure или партнерских приложениях. 

Базовый общедоступный Load Balancer предоставляет сводное состояние проверки работоспособности для каждого внутреннего пула с помощью журналов Azure Monitor.  Журналы Azure Monitor недоступны для внутренних базовых подсистем балансировки нагрузки.  Вы можете использовать [журналы Azure Monitor](load-balancer-monitor-log.md) , чтобы проверить состояние работоспособности и число проб для общедоступной подсистемы балансировки нагрузки. Функция ведения журналов в Power BI или Azure Operational Insights позволяет получать статистические данные о работоспособности подсистемы балансировки нагрузки.

## <a name="limitations"></a>Ограничения

- Пробы HTTPS не поддерживают взаимную проверку подлинности с помощью сертификата клиента.
- Если метки времени протокола TCP включены, то выполнение проб работоспособности невозможно.

## <a name="next-steps"></a>Следующие шаги

- Узнайте больше об [Azure Load Balancer уровня "Стандартный"](load-balancer-standard-overview.md).
- [Краткое руководство. Создание общедоступной подсистемы балансировки нагрузки с помощью Azure PowerShell](load-balancer-get-started-internet-arm-ps.md)
- [Load Balancer Probes - Get](https://docs.microsoft.com/rest/api/load-balancer/loadbalancerprobes/) (Получение проб работоспособности Load Balancer)
- Запросите новые возможности пробы работоспособности на [платформе Uservoice для Load Balancer](https://aka.ms/lbuservoice).
