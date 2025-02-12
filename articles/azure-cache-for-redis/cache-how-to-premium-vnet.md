---
title: Настройка виртуальной сети для кэша Azure для Redis ценовой категории "Премиум" | Документация Майкрософт
description: Узнайте, как настроить поддержку виртуальной сети и управлять ей для экземпляров кэша Azure для Redis ценовой категории "Премиум"
services: cache
documentationcenter: ''
author: yegu-ms
manager: jhubbard
editor: ''
ms.assetid: 8b1e43a0-a70e-41e6-8994-0ac246d8bf7f
ms.service: cache
ms.workload: tbd
ms.tgt_pltfrm: cache
ms.devlang: na
ms.topic: article
ms.date: 05/15/2017
ms.author: yegu
ms.openlocfilehash: 4f97f6925c482cb282324dcc1c97bbfe2a701643
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "67074210"
---
# <a name="how-to-configure-virtual-network-support-for-a-premium-azure-cache-for-redis"></a>Настройка поддержки виртуальной сети для кэша Azure для Redis ценовой категории "Премиум"
Кэш Azure для Redis предлагает разные варианты кэша, которые позволяют выбирать размер и функции кэша, включая функции ценовой категории "Премиум", такие как кластеризация, постоянное хранение данных и поддержка виртуальной сети. Виртуальная сеть — это частная сеть в облаке. Если экземпляр кэша Azure для Redis настроен в виртуальной сети, он не является общедоступным, а доступен только для виртуальных машин и приложений в этой виртуальной сети. В этой статье описана настройка поддержки виртуальных сетей для экземпляра кэша Azure для Redis ценовой категории "Премиум".

> [!NOTE]
> Кэш Azure для Redis поддерживает классические виртуальные сети и виртуальные сети Resource Manager.
> 
> 

См. дополнительные сведения о других функциях [кэша Azure для Redis ценовой категории "Премиум"](cache-premium-tier-intro.md).

## <a name="why-vnet"></a>Зачем использовать виртуальные сети?
Развертывание [виртуальной сети Azure](https://azure.microsoft.com/services/virtual-network/) обеспечивает дополнительные возможности защиты и изоляции для кэша Azure для Redis. Такие развертывания позволяют определять подсети, настраивать политики контроля доступа, а также использовать другие функции ограничения доступа.

## <a name="virtual-network-support"></a>Поддержка виртуальной сети
Поддержка виртуальной сети настраивается во время создания кэша в колонке **New Azure Cache for Redis** (Новый кэш Azure для Redis). 

[!INCLUDE [redis-cache-create](../../includes/redis-cache-premium-create.md)]

После выбора ценовой категории "Премиум" можно настроить интеграцию виртуальной сети Redis, выбрав виртуальную сеть, которая находится в той же подписке и расположении, что и кэш. Чтобы использовать новую виртуальную сеть, сначала создайте ее, выполнив действия, описанные в разделе [Создать виртуальную сеть](../virtual-network/manage-virtual-network.md#create-a-virtual-network) или в статье [Создание (классической) виртуальной сети с помощью портала Azure](../virtual-network/virtual-networks-create-vnet-classic-pportal.md), а затем вернитесь в колонку **New Azure Cache for Redis** (Новый кэш Azure для Redis), чтобы создать и настроить кэш ценовой категории "Премиум".

Чтобы настроить виртуальную сеть для нового кэша, щелкните **Виртуальная сеть** в колонке **New Azure Cache for Redis** (Новый кэш Azure для Redis) и в раскрывающемся списке выберите нужную виртуальную сеть.

![Виртуальная сеть][redis-cache-vnet]

Выберите нужную подсеть в раскрывающемся списке **Подсети** и укажите необходимый **статический IP-адрес**. При использовании классической виртуальной сети заполнять поле **Статический IP-адрес** не обязательно, а если этот адрес не задан, то он выбирается из указанной подсети.

> [!IMPORTANT]
> При развертывании кэша Azure для Redis в виртуальной сети Resource Manager этот кэш следует разместить в выделенной подсети, которая не содержит других ресурсов, кроме экземпляров кэша Azure для Redis. При попытке развернуть кэш Azure для Redis в подсети виртуальной сети Resource Manager, содержащей другие ресурсы, развертывание завершается сбоем.
> 
> 

![Виртуальная сеть][redis-cache-vnet-ip]

> [!IMPORTANT]
> Azure резервирует некоторые IP-адреса в каждой подсети, которые нельзя использовать. Зарезервированы первый и последний IP-адреса подсетей (для соответствия требованиям протокола), а также еще три адреса, используемые для служб Azure. Дополнительные сведения см. в разделе [Существуют ли ограничения на использование IP-адресов в пределах этих подсетей?](../virtual-network/virtual-networks-faq.md#are-there-any-restrictions-on-using-ip-addresses-within-these-subnets)
> 
> Помимо IP-адресов, используемых инфраструктурой виртуальной сети Azure, каждый экземпляр Redis в подсети использует два IP-адреса на сегмент и еще один IP-адрес для балансировщика нагрузки. Некластеризованный кэш считается имеющим один сегмент.
> 
> 

Создав кэш, конфигурацию виртуальной сети можно просмотреть, щелкнув **Виртуальная сеть** в **меню ресурсов**.

![Виртуальная сеть][redis-cache-vnet-info]

Чтобы подключиться к экземпляру кэша Azure для Redis при использовании виртуальной сети, укажите в строке подключения имя узла кэша, как показано в приведенном ниже примере.

    private static Lazy<ConnectionMultiplexer> lazyConnection = new Lazy<ConnectionMultiplexer>(() =>
    {
        return ConnectionMultiplexer.Connect("contoso5premium.redis.cache.windows.net,abortConnect=false,ssl=true,password=password");
    });

    public static ConnectionMultiplexer Connection
    {
        get
        {
            return lazyConnection.Value;
        }
    }

## <a name="azure-cache-for-redis-vnet-faq"></a>Вопросы и ответы по виртуальной сети кэша Azure для Redis
Следующий список содержит ответы на часто задаваемые вопросы о виртуальных сетях кэша Azure для Redis.

* Каковы распространенные ошибки в конфигурации кэша Azure для Redis и виртуальных сетей?
* [Как проверить, что кэш работает в виртуальной сети?](#how-can-i-verify-that-my-cache-is-working-in-a-vnet)
* Почему при попытке подключения к моему кэшу Azure для Redis в виртуальной сети происходит ошибка и появляется сообщение о том, что удаленный сертификат недействителен?
* [Можно ли использовать виртуальные сети в кэше уровня "Стандартный" или "Базовый"?](#can-i-use-vnets-with-a-standard-or-basic-cache)
* Почему в одних подсетях не удается создать кэш Azure для Redis, а в других удается?
* [Каковы требования к адресному пространству подсети?](#what-are-the-subnet-address-space-requirements)
* [Do all cache features work when hosting a cache in a VNET? (Все ли функции кэша работают, когда он размещен в виртуальной сети?)](#do-all-cache-features-work-when-hosting-a-cache-in-a-vnet)

### <a name="what-are-some-common-misconfiguration-issues-with-azure-cache-for-redis-and-vnets"></a>Каковы распространенные ошибки в конфигурации кэша Azure для Redis и виртуальных сетей?
При размещении кэша Redis Azure для Redis в виртуальной сети используются порты, указанные в следующих таблицах. 

>[!IMPORTANT]
>Если эти порты заблокированы, кэш может работать неправильно. Блокировка одного или нескольких из этих портов является самой распространенной ошибкой при настройке кэша Azure для Redis в виртуальной сети.
> 
> 

- [Обязательные порты для исходящего трафика](#outbound-port-requirements)
- [Обязательные порты для входящего трафика](#inbound-port-requirements)

#### <a name="outbound-port-requirements"></a>Обязательные порты для исходящего трафика

Существует семь обязательных портов для исходящего трафика.

- Все исходящие подключения к Интернету осуществляется через клиент пользователя локального аудита устройства.
- Три порта направляют трафик к конечным точкам Azure, обслуживающим службу хранилища Azure и Azure DNS.
- Остальные диапазоны портов предназначены для внутреннего обмена данными в подсети Redis. Для внутреннего обмена данными в подсети Redis не требуются правила группы безопасности сети в подсети.

| Порты | Направление | Транспортный протокол | Назначение | Локальный IP-адрес | Удаленный IP-адрес |
| --- | --- | --- | --- | --- | --- |
| 80, 443 |Исходящие |TCP |Зависимости Redis в службе хранилища Azure или PKI (Интернет) | (Подсеть Redis) |* |
| 53 |Исходящие |TCP/UDP |Зависимости Redis в DNS (Интернет/виртуальная сеть) | (Подсеть Redis) | 168.63.129.16 и 169.254.169.254 <sup>1</sup> и любой пользовательский DNS-сервер для подсети <sup>3</sup> |
| 8443 |Исходящие |TCP |Внутренний обмен данными для Redis | (Подсеть Redis) | (Подсеть Redis) |
| 10221-10231 |Исходящие |TCP |Внутренний обмен данными для Redis | (Подсеть Redis) | (Подсеть Redis) |
| 20226 |Исходящие |TCP |Внутренний обмен данными для Redis | (Подсеть Redis) |(Подсеть Redis) |
| 13000-13999 |Исходящие |TCP |Внутренний обмен данными для Redis | (Подсеть Redis) |(Подсеть Redis) |
| 15000-15999 |Исходящие |TCP |Внутренний обмен данными для Redis | (Подсеть Redis) |(Подсеть Redis) |
| 6379-6380 |Исходящие |TCP |Внутренний обмен данными для Redis | (Подсеть Redis) |(Подсеть Redis) |

<sup>1</sup> указанные IP-адреса принадлежит корпорации Майкрософт, предназначенных для виртуальной Машины узла, который служит в Azure DNS.

<sup>3</sup> не требуется для подсетей, с помощью пользовательского DNS-сервер, или более поздней версии redis кэши, игнорировать пользовательский DNS-сервер.

#### <a name="inbound-port-requirements"></a>Обязательные порты для входящего трафика

Существует восемь обязательных диапазонов портов для входящего трафика. Входящие запросы в этих диапазонах исходят от других служб, размещенных в той же виртуальной сети, или являются результатом внутреннего обмена данными в подсети Redis.

| Порты | Направление | Транспортный протокол | Назначение | Локальный IP-адрес | Удаленный IP-адрес |
| --- | --- | --- | --- | --- | --- |
| 6379, 6380 |Входящий трафик |TCP |Обмен данными между клиентом и Redis, балансировка нагрузки Azure | (Подсеть Redis) | (Подсеть redis), виртуальная сеть, Azure Load Balancer <sup>2</sup> |
| 8443 |Входящий трафик |TCP |Внутренний обмен данными для Redis | (Подсеть Redis) |(Подсеть Redis) |
| 8500 |Входящий трафик |TCP/UDP |Балансировка нагрузки Azure | (Подсеть Redis) |Azure Load Balancer |
| 10221-10231 |Входящий трафик |TCP |Внутренний обмен данными для Redis | (Подсеть Redis) |(Подсеть Redis) Azure Load Balancer |
| 13000-13999 |Входящий трафик |TCP |Обмен данными между клиентом и кластерами Redis, балансировка нагрузки Azure | (Подсеть Redis) |Виртуальная сеть, Azure Load Balancer |
| 15000-15999 |Входящий трафик |TCP |Обмен данными между клиентом и кластерами Redis, балансировка нагрузки Azure | (Подсеть Redis) |Виртуальная сеть, Azure Load Balancer |
| 16001 |Входящий трафик |TCP/UDP |Балансировка нагрузки Azure | (Подсеть Redis) |Azure Load Balancer |
| 20226 |Входящий трафик |TCP |Внутренний обмен данными для Redis | (Подсеть Redis) |(Подсеть Redis) |

<sup>2</sup> можно использовать этот тег «AzureLoadBalancer» (Resource Manager) (или «AZURE_LOADBALANCER» для классических развертываний), для создания правила группы безопасности сети.

#### <a name="additional-vnet-network-connectivity-requirements"></a>Дополнительные требования к подключению к виртуальной сети

Существуют требования к сетевому подключению кэша Azure для Redis, которым виртуальная сеть изначально может не соответствовать. Чтобы кэш Azure для Redis в виртуальной сети работал правильно, нужно выполнить все перечисленные ниже требования.

* Исходящие сетевые подключения к конечным точкам хранилища Azure по всему миру. Это относится к конечным точкам, находящимся в одном регионе с экземпляром кэша Azure для Redis, а также к конечным точкам хранилища, расположенным в **других** регионах Azure. Конечные точки службы хранилища Azure разрешаются в следующих DNS-доменах: *table.core.windows.net*, *blob.core.windows.net*, *queue.core.windows.net* и *file.core.windows.net*. 
* Исходящие сетевые подключения к *ocsp.msocsp.com*, *mscrl.microsoft.com* и *crl.microsoft.com*. Такое подключение необходимо для поддержки функций протокола SSL.
* Конфигурация DNS для виртуальной сети должна поддерживать разрешение всех конечных точек и доменов, указанных в предыдущих точках. Эти требования DNS обеспечиваются за счет настройки допустимой инфраструктуры DNS и ее поддержания для виртуальной сети.
* Исходящие сетевые подключения к следующим конечным точкам мониторинга Azure, которые разрешаются в таких DNS-доменах: shoebox2-black.shoebox2.metrics.nsatc.net, north-prod2.prod2.metrics.nsatc.net, azglobal-black.azglobal.metrics.nsatc.net, shoebox2-red.shoebox2.metrics.nsatc.net, east-prod2.prod2.metrics.nsatc.net, azglobal-red.azglobal.metrics.nsatc.net.

### <a name="how-can-i-verify-that-my-cache-is-working-in-a-vnet"></a>Как проверить, что кэш работает в виртуальной сети?

>[!IMPORTANT]
>При подключении к экземпляру кэша Azure для Redis, размещенному в виртуальной сети, клиенты кэша должны находиться в той же виртуальной сети или в виртуальной сети, в которой включен пиринг виртуальных сетей. Сюда входят все тестовые приложения и средства диагностики для проверки связи. Независимо от того, где размещено клиентское приложение, сетевые группы безопасности необходимо настроить таким образом, чтобы сетевой трафик клиента достигал экземпляра Redis.
>
>

Когда обязательные порты настроены, как описано в предыдущем разделе, можно проверить работу кэша, выполнив следующие действия.

- [Перезагрузите](cache-administration.md#reboot) все узлы кэша. Если не установить все необходимые зависимости кэша (как описано в разделах [Обязательные порты для входящего трафика](cache-how-to-premium-vnet.md#inbound-port-requirements) и [Обязательные порты для исходящего трафика](cache-how-to-premium-vnet.md#outbound-port-requirements)), то кэш не сможет перезапускаться.
- После перезапуска узлов кэша (по данным о состоянии кэша на портале Azure) можно выполнить следующие поверки:
  - проверить связь с конечной точкой кэша (через порт 6380) с компьютера, который находится с кэшем в одной виртуальной сети, используя инструмент [tcping](https://www.elifulkerson.com/projects/tcping.php). Пример:
    
    `tcping.exe contosocache.redis.cache.windows.net 6380`
    
    Если инструмент `tcping` сообщает, что порт открыт, то кэш доступен для подключения с клиентов в виртуальной сети.

  - Другой способ проверки — создать тестовый клиент кэша (это может быть простое консольное приложение, использующее StackExchange.Redis), который подключается к кэшу и добавляет какие-то элементы в кэш или извлекает их из него. Установите пример клиентского приложения на виртуальную машину, которая находится с кэшем в одной виртуальной сети, и запустите его, чтобы проверить подключение к кэшу.


### <a name="when-trying-to-connect-to-my-azure-cache-for-redis-in-a-vnet-why-am-i-getting-an-error-stating-the-remote-certificate-is-invalid"></a>Почему при попытке подключения к моему кэшу Azure для Redis в виртуальной сети происходит ошибка и появляется сообщение о том, что удаленный сертификат недействителен?

При попытке подключения к кэшу Azure для Redis в виртуальной сети появляется сообщение об ошибке проверки сертификатов, например такое:

`{"No connection is available to service this operation: SET mykey; The remote certificate is invalid according to the validation procedure.; …"}`

Причина может заключаться в том, что вы подключаетесь к узлу по IP-адресу. Мы рекомендуем использовать имя узла. Другими словами, используйте следующее:     

`[mycachename].redis.windows.net:6380,password=xxxxxxxxxxxxxxxxxxxx,ssl=True,abortConnect=False`

Не используйте IP-адрес, похожий на следующую строку подключения:

`10.128.2.84:6380,password=xxxxxxxxxxxxxxxxxxxx,ssl=True,abortConnect=False`

Если не удается разрешить имя DNS, некоторые клиентские библиотеки содержат параметры конфигурации, например `sslHost`, которые предоставляет клиент StackExchange.Redis. Это позволяет переопределить имя узла, используемое для проверки сертификата. Пример:

`10.128.2.84:6380,password=xxxxxxxxxxxxxxxxxxxx,ssl=True,abortConnect=False;sslHost=[mycachename].redis.windows.net`

### <a name="can-i-use-vnets-with-a-standard-or-basic-cache"></a>Можно ли использовать виртуальные сети в кэше уровня "Стандартный" или "Базовый"?
Виртуальные сети доступны только для кэшей уровня "Премиум".

### <a name="why-does-creating-an-azure-cache-for-redis-fail-in-some-subnets-but-not-others"></a>Почему в одних подсетях не удается создать кэш Azure для Redis, а в других удается?
При развертывании кэша Azure для Redis в виртуальной сети Resource Manager он должен размещаться в выделенной подсети, которая не содержит ресурсов другого типа. При попытке развернуть кэш Azure для Redis в подсети виртуальной сети Resource Manager, содержащей другие ресурсы, развертывание завершается сбоем. Прежде чем создавать новый кэш Azure для Redis, необходимо удалить существующие ресурсы из подсети.

В классической виртуальной сети можно развернуть ресурсы нескольких типов, пока для них имеется достаточно доступных IP-адресов.

### <a name="what-are-the-subnet-address-space-requirements"></a>Каковы требования к адресному пространству подсети?
Azure резервирует некоторые IP-адреса в каждой подсети, которые нельзя использовать. Зарезервированы первый и последний IP-адреса подсетей (для соответствия требованиям протокола), а также еще три адреса, используемые для служб Azure. Дополнительные сведения см. в разделе [Существуют ли ограничения на использование IP-адресов в пределах этих подсетей?](../virtual-network/virtual-networks-faq.md#are-there-any-restrictions-on-using-ip-addresses-within-these-subnets)

Помимо IP-адресов, используемых инфраструктурой виртуальной сети Azure, каждый экземпляр Redis в подсети использует два IP-адреса на сегмент и еще один IP-адрес для балансировщика нагрузки. Некластеризованный кэш считается имеющим один сегмент.

### <a name="do-all-cache-features-work-when-hosting-a-cache-in-a-vnet"></a>Do all cache features work when hosting a cache in a VNET? (Все ли функции кэша работают, когда он размещен в виртуальной сети?)
Если кэш является частью виртуальной сети, то к нему могут обращаться только клиенты в этой виртуальной сети. Поэтому следующие функции управления кэшем в данный момент не работают.

* Консоль Redis. Так как консоль Redis работает в локальном браузере вне виртуальной сети, она не может подключиться к кэшу.


## <a name="use-expressroute-with-azure-cache-for-redis"></a>Использование ExpressRoute с кэшем Azure для Redis

Клиенты могут подключить канал [Azure ExpressRoute](https://azure.microsoft.com/services/expressroute/) к своей инфраструктуре виртуальной сети, расширяя таким образом свою локальную сеть в Azure. 

По умолчанию только что созданный канал ExpressRoute не выполняет принудительное туннелирование (объявление маршрута по умолчанию, 0.0.0.0/0) в виртуальной сети. В результате исходящие подключения к Интернету выполняются напрямую из виртуальной сети, а клиентские приложения могут подключаться к другим конечным точкам Azure, включая кэш Azure для Redis.

Тем не менее типовой клиентской конфигурацией является использование принудительного туннелирования (объявление маршрута по умолчанию), которое вместо этого направляет исходящий интернет-трафик в локальную среду. Этот поток трафика прерывает подключение к кэшу Azure для Redis, если исходящий трафик затем блокируется локально, так что экземпляр кэша Azure для Redis не может связаться со своими зависимыми компонентами.

Чтобы устранить эту проблему, следует указать один или несколько определяемых пользователем маршрутов в подсети, которая содержит кэш Azure для Redis. Определяемые пользователем маршруты задают маршруты для подсети, которые будут использоваться вместо основного маршрута.

При возможности рекомендуется использовать следующую конфигурацию:

* Конфигурация ExpressRoute объявляет маршрут 0.0.0.0/0 и по умолчанию организует принудительное туннелирование всех исходящих подключений в локальную среду.
* Определяемый пользователем маршрут в подсети, содержащей кэш Azure для Redis, направляет трафик TCP/IP к диапазону адресов 0.0.0.0/0 в общедоступный Интернет, например определяя тип "Интернет" для следующего прыжка.

В результате этих действий определяемый пользователем маршрут уровня подсети получает приоритет над принудительным туннелированием ExpressRoute, обеспечивая исходящий интернет-доступ из кэша Azure для Redis.

Подключение к экземпляру кэша Azure для Redis из локального приложения с помощью ExpressRoute используется редко, так как оказывает отрицательное влияние на производительность (для повышения производительности клиенты кэша Azure для Redis должны находиться в том же регионе, что и кэш Azure для Redis).

>[!IMPORTANT] 
>Маршруты, указанные в UDR, **должны** быть достаточно конкретными, чтобы иметь приоритет над всеми маршрутами, объявленными в конфигурации ExpressRoute. В приведенном ниже примере используется широкий диапазон адресов 0.0.0.0/0, и, таким образом, он может быть случайно заменен объявлениями маршрутов с более узкими диапазонами адресов.

>[!WARNING]  
>Кэш Azure для Redis не поддерживается с конфигурациями ExpressRoute, которые **неправильно осуществляют перекрестное объявление маршрутов из пути общедоступного пиринга в путь частного пиринга**. Конфигурации ExpressRoute, для которых настроен общедоступный пиринг, будут получать объявления маршрутов от корпорации Майкрософт для большого набора диапазонов IP-адресов Microsoft Azure. Если эти диапазоны адресов неправильно перекрестно объявляются в пути частного пиринга, то все исходящие сетевые пакеты из подсети экземпляра кэша Azure для Redis ошибочно принудительно туннелируются в локальную сетевую инфраструктуру клиента. Такой сетевой поток нарушает работу кэша Azure для Redis. Решением этой проблемы является остановка перекрестного объявления маршрутов между путем общедоступного и закрытого пиринга.


Справочные сведения об определяемых пользователем маршрутах см. в этом [обзоре](../virtual-network/virtual-networks-udr-overview.md).

Дополнительные сведения об ExpressRoute см. в статье [Технический обзор ExpressRoute](../expressroute/expressroute-introduction.md).

## <a name="next-steps"></a>Дальнейшие действия
Узнайте, как использовать расширенные функции кэша.

* [Общие сведения о кэше Azure для Redis уровня "Премиум"](cache-premium-tier-intro.md)

<!-- IMAGES -->

[redis-cache-vnet]: ./media/cache-how-to-premium-vnet/redis-cache-vnet.png

[redis-cache-vnet-ip]: ./media/cache-how-to-premium-vnet/redis-cache-vnet-ip.png

[redis-cache-vnet-info]: ./media/cache-how-to-premium-vnet/redis-cache-vnet-info.png

