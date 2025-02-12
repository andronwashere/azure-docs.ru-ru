---
title: Сетевые компоненты развертывания — в службе приложений Azure | Документация Майкрософт
description: Как использовать различные службы приложений, сетевые компоненты
author: ccompy
manager: stefsch
editor: ''
services: app-service\web
documentationcenter: ''
ms.assetid: 5c61eed1-1ad1-4191-9f71-906d610ee5b7
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 05/28/2019
ms.author: ccompy
ms.custom: seodec18
ms.openlocfilehash: db29d0761084e32d601dc9c6d94082cd09bc5d18
ms.sourcegitcommit: cf438e4b4e351b64fd0320bf17cc02489e61406a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/08/2019
ms.locfileid: "67655467"
---
# <a name="app-service-networking-features"></a>Сетевые функции службы приложений

Приложения в службе приложений Azure можно развернуть несколькими способами. По умолчанию службы приложений, размещенных приложений, непосредственно недоступна через Интернет и можно только достичь конечных точек размещенных Интернет. Однако многие приложения клиента необходимо контролировать входящий и исходящий сетевой трафик. Некоторые возможности доступны в службе приложений для удовлетворения этих потребностей. Задача – знать, какие функции следует использовать для решения данной проблемы. Этот документ призван помочь пользователям определить, какие функции следует использовать в зависимости от некоторых вариантов использования.

Существует два типа основного развертывания для службы приложений Azure. Есть мультитенантная служба public, где размещается планов службы приложений в Free, Shared, Basic, Standard, Premium и премиум v2 цены на SKU. То есть один клиент приложения службы (ase), где размещается планы изолированной службы приложений SKU непосредственно в виртуальной сети Azure (VNet). В мультитенантной службе или в среде ASE, функций, которые использованы зависят. 

## <a name="multi-tenant-app-service-networking-features"></a>Сетевые функции мультитенантной службы приложений 

Служба приложений Azure — это распределенная система. Роли, которые обрабатывают входящие запросы HTTP/HTTPS, называются внешних интерфейсов. Роли, на которых размещены рабочей нагрузки клиентов, называются рабочих ролей. Все роли в развертывании службы приложений существует в нескольких клиентов сети. Так как существует множество разных клиентов в одной единице масштабирования службы приложений, сетевой службы приложений не удается подключиться напрямую к сети. Вместо подключение сетей, нам нужен функций для обработки различных аспектов взаимодействия приложений. Функции, которые обрабатывают запросы к приложению, не может использоваться для решения проблем при выполнении вызовов из приложения. Аналогичным образом функции, которые решают проблемы для вызовов из приложения, не может использоваться для решения проблем в приложение.  

| Входящий функции | Исходящие функции |
|---------------------|-------------------|
| Адреса, присвоенные приложению | Гибридные подключения |
| Ограничения доступа | Шлюз требуется интеграция с виртуальной сетью |
| Конечные точки службы | Интеграция виртуальной сети (Предварительная версия) |

Если не указано иное, все функции могут использоваться совместно. Вы можете объединять компоненты, которые должны решать различные проблемы.

## <a name="use-case-and-features"></a>Вариант использования и компоненты

Для любого определенного сценария использования может существовать несколько способов для решения проблемы.  Для использования функцией иногда бывает из-за причин, которые просто сам вариант использования. В следующих вариантах применения входящий предлагать способы использования службы приложений, сетевые компоненты для решения проблемы в отношении управления трафиком, перейдя в приложение. 
 
| Варианты использования для входящего трафика | Компонент |
|---------------------|-------------------|
| Поддержки SSL на основе потребностей приложения | Адреса, присвоенные приложению |
| Не в общем, выделенный адрес для входящего трафика для приложения | Адреса, присвоенные приложению |
| Ограничение доступа к приложению из набора четко определенных адресов | Ограничения доступа |
| Предоставить Мое приложение на частные IP-адреса в виртуальной сети | Среда ASE с ILB </br> Шлюз приложений с помощью конечных точек службы |
| Ограничение доступа в мое приложение на основе ресурсов в виртуальной сети | Конечные точки службы </br> Среда ASE с ILB |
| Предоставить Мое приложение на частный IP-адрес в виртуальной сети | Среда ASE с ILB </br> частный IP-адрес для входящих подключений на шлюз приложений с помощью конечных точек службы |
| Защита приложения с помощью WAF | Шлюз приложений + ASE с внутренним балансировщиком Нагрузки </br> Шлюз приложений с помощью конечных точек службы </br> Azure передовой линии с помощью ограничения доступа |
| Балансировка трафика на мои приложения в разных регионах | Azure передовой линии с помощью ограничения доступа | 
| Балансировка нагрузки трафика в одном регионе | Шлюз приложений с помощью конечных точек службы | 

В следующих вариантах применения исходящих предлагать способы использования службы приложений, сетевые компоненты по удовлетворению требований исходящий доступ для вашего приложения. 

| Варианты использования исходящего трафика | Компонент |
|---------------------|-------------------|
| Доступ к ресурсам в виртуальной сети Azure в одном регионе | интеграция с виртуальной сетью; </br> ASE |
| Доступ к ресурсам в виртуальной сети Azure в другом регионе | Шлюз требуется интеграция с виртуальной сетью </br> ASE и пиринговая виртуальная сеть |
| Доступ к ресурсам, защищенным с помощью конечных точек службы | интеграция с виртуальной сетью; </br> ASE |
| Доступ к ресурсам в частной сети не подключены к Azure | Гибридные подключения |
| Доступ к ресурсам через каналы ExpressRoute | Интеграция с виртуальной сетью (сейчас ограничены адреса RFC 1918) </br> ASE | 


### <a name="default-networking-behavior"></a>Сетевые поведение по умолчанию

Единицы масштабирования службы приложений Azure поддерживает многие клиенты в каждом развертывании. Планы Free и Shared SKU размещать рабочие нагрузки клиентов в мультитенантных рабочих ролях. Базовыми типами данных и более поздних версий планы размещения клиента рабочих нагрузок, которые выделены для только один план службы приложений (ASP). Если у вас есть план Standard службы приложений, все приложения в этом плане запустится на одной рабочей роли. При горизонтальном масштабировании рабочей роли, все приложения в этом ASP будут реплицированы на новый рабочий процесс для каждого экземпляра в ASP. Рабочие процессы, которые используются для премиум v2 отличаются от рабочих процессов, используемых для других планов. Каждое развертывание службы приложений имеет один IP-адрес, который используется для весь входящий трафик к приложениям в данном развертывании службы приложений. Однако в любом из 4 11 адреса, используемые для выполнения исходящих вызовов. Эти адреса являются общими для всех приложений в данном развертывании службы приложений. Исходящие адреса различаются в зависимости от типов рабочих. Это означает, что адреса, используемые Free, Shared, Basic, Standard и Premium поддерживается отличаются от адреса, используемые для исходящих вызовов от поддерживается премиум v2. Если взглянуть в свойствах для вашего приложения, вы увидите адреса входящего и исходящего трафика, которые используются приложением. Если вам нужно заблокировать зависимость с помощью ACL IP-адрес, используйте possibleOutboundAddresses. 

![Свойства приложения](media/networking-features/app-properties.png)

Служба приложений имеет ряд конечных точек, используемых для управления службой.  Эти адреса публикуются в виде отдельного документа они также тега AppServiceManagement IP-адрес службы. Тег AppServiceManagement используется только с помощью службы приложений среда (ASE) где необходимо разрешить такой трафик. Служба приложений, которые входящих адресов, отслеживаются в тег службы IP-адрес службы приложений. Нет тега службы не IP-адрес, который содержит исходящие адреса, используемые службой приложений. 

![Схема входящего и исходящего трафика службы приложений](media/networking-features/default-behavior.png)

### <a name="app-assigned-address"></a>Адреса, присвоенные приложению 

Функция назначенный адрес приложения offshoot возможности SSL на основе IP-адресов и обращается к настройке SSL-сертификата с вашим приложением. Эта функция может использоваться для SSL на основе вызовов, но его также можно предоставить адрес, который имеет только она для приложения. 

![Схема адреса, присвоенные приложению](media/networking-features/app-assigned-address.png)

При использовании приложения назначается адрес трафик по-прежнему проходит через те же роли внешнего интерфейса, которые обрабатывают весь входящий трафик в единицу масштабирования службы приложений. Адрес, назначенный в приложение, тем не менее, используется только для приложения. Варианты использования для этой функции должны:

* Поддержки SSL на основе потребностей приложения
* Задать выделенный адрес для вашего приложения, которое не используется совместно с другими компонентами

Узнайте, как задать адрес в приложении к руководству для [SSL на основе IP, Настройка][appassignedaddress]. 

### <a name="access-restrictions"></a>Ограничения доступа 

Позволяет возможность ограничения доступа, фильтровать **входящий** запросы в соответствии с IP-адрес источника сообщений. Действие фильтра производится на вышестоящий из собирает данные о рабочих ролей внешнего интерфейса которой работают ваши приложения. Так как внутренним ролям вышестоящего из работников, возможность ограничения доступа может рассматриваться как защита на уровне сети для приложений. Эта функция позволяет создать список разрешенных и запрещенных блоки адресов, которые оцениваются в порядке приоритета. Это похоже на функцию группы безопасности сети (NSG), который существует в сети Azure.  Эту функцию можно использовать в среде ASE или в службе несколько клиентов. При использовании ASE с ILB, можно ограничить доступ из блоков частных адресов.

![Ограничения доступа](media/networking-features/access-restrictions.png)

Ограничения доступа позволяет в сценариях, где вы хотите ограничить IP-адреса, которые могут использоваться в приложении. Помимо использования приведены варианты этой функции:

* Ограничение доступа к приложению из набора четко определенных адресов 
* Ограничьте доступ к поступающих через службу балансировки нагрузки, такие как входной двери Azure. Если вы хотите заблокировать ваш входящий трафик на входной двери Azure, создайте правила, чтобы разрешить трафик от 147.243.0.0/16 и 2a01:111:2050:: / 44. 

![Ограничения доступа с помощью передовой линии](media/networking-features/access-restrictions-afd.png)

Если вы хотите заблокировать доступ к приложению, чтобы он может быть достигнуто только ресурсы в виртуальной сети Azure (VNet), требуется статический общедоступный адрес на независимо от источника находится в виртуальной сети. Если ресурсы отсутствует общедоступный адрес, следует использовать функцию конечных точек службы. Узнайте, как включить эту функцию с руководством для [Настройка ограничения доступа][iprestrictions].

### <a name="service-endpoints"></a>Конечные точки службы

Конечные точки службы позволяет заблокировать **входящий** доступ приложению таким образом, что адрес источника должны поступать из набор подсетей, которые можно выбрать. Эта функция работает в сочетании с ограничения доступа к IP-адресов. Конечные точки служб задаются по той же схеме пользователя, как ограничения доступа к IP-адресов. Вы можете создавать разрешить или запретить список правил доступа, включающий общедоступных адресов, а также подсетей в виртуальных сетях. Эта функция поддерживает такие сценарии:

![Конечные точки службы](media/networking-features/service-endpoints.png)

* Настройка шлюза приложений с вашим приложением для блокировки входящего трафика в приложение
* Ограничение доступа к приложению, к ресурсам в виртуальной сети. Это может включать виртуальные машины, среды ASE или даже другие приложения, использующие интеграцию с виртуальной сетью 

![конечные точки службы с помощью шлюза приложений](media/networking-features/service-endpoints-appgw.png)

Дополнительные сведения о настройке конечных точек службы в приложении в этом руководстве на [Настройка ограничения доступа конечной точки службы][serviceendpoints]
 
### <a name="hybrid-connections"></a>Гибридные подключения

Гибридные подключения службы приложений позволяет приложениям осуществлять **исходящих** вызовы для заданных конечных точек TCP. Конечная точка может быть локально, в виртуальной сети или в любом месте, разрешает исходящий трафик в Azure через порт 443. Эта функция требует установки агента ретрансляции, вызвать диспетчер гибридных подключений (HCM) для Windows Server 2012 или более новые узла. Диспетчер должен иметь доступ к Ретранслятору через порт 443. В пользовательском Интерфейсе приложения службы гибридных подключений на портале можно загрузить диспетчер гибридного подключения. 

![Гибридный поток сетевого подключения](media/networking-features/hybrid-connections.png)

Функция гибридных подключений службы приложений основывается на возможности гибридных подключений ретранслятора Azure. Служба приложений использует особый вид компонент, который поддерживает только внесения исходящих вызовов из приложения к TCP-узлу и порту. Этот узел и порт достаточно разрешаться на узле, где установлен диспетчер гибридного подключения. Когда приложение, в службе приложений, выполняет поиск DNS на узел и порт, определенный в гибридное подключение, трафик автоматически перенаправляется через гибридное подключение и out диспетчер гибридных подключений. Чтобы узнать больше о гибридных подключений, ознакомьтесь с документацией на [гибридные подключения службы приложений][hybridconn]

Эта функция обычно используется для:

* Доступ к ресурсам в частных сетях, которые не подключены к Azure с помощью VPN или ExpressRoute
* Поддержка lift- and -shift локальных приложений в службе приложений без необходимости также переместить вспомогательные базы данных  
* Безопасно предоставить доступ для одного узла и порта для гибридного подключения. Большинство функций сети откройте доступ к сети и с гибридными подключениями имеется только один узел и порт, которому требуется получить доступ.
* Описаны сценарии, не связанная с другими методами исходящие подключения
* Разработка выполняется в службе приложений, где приложения могут легко использовать с локальными ресурсами 

Поскольку эта функция позволяет доступ к локальным ресурсам без отверстия брандмауэра для входящих подключений, популярность у разработчиков. Другие исходящего трафика службы приложений сетевые возможности, очень Azure виртуальной сети связанные. Гибридные подключения не имеет зависимость от через виртуальную сеть и может использоваться для разнообразных потребностей сети. Это важно отметить, что функция гибридных подключений службы приложений care или не знать, что делают поверх нее. Это значит, что его можно использовать для доступа к базе данных, веб-службы или произвольные TCP-сокет на мэйнфрейме. Эта функция фактически туннелей TCP-пакетов. 

Несмотря на то широко применяется для разработки гибридных подключений, также используется в также множество рабочих приложениях. Он отлично подходит для доступа к веб-службы или базы данных, но не подходит для ситуаций, связанных с создания большого количества подключений. 

### <a name="gateway-required-vnet-integration"></a>Шлюз требуется интеграция с виртуальной сетью 

Шлюз требуется интеграции с виртуальной сетью службы приложения позволяет приложению вносить **исходящих** запросы в виртуальной сети Azure. Эта функция работает, подключившись узла, приложения, запущенного на к шлюзу виртуальной сети в виртуальной сети с помощью VPN между сайтами. При настройке компонентов, приложение получает один из адресов между сайтами, назначенный каждому экземпляру. Эта функция позволяет получить доступ к ресурсам в классические виртуальные сети или виртуальные сети Resource Manager в любом регионе. 

![Шлюз требуется интеграция с виртуальной сетью](media/networking-features/gw-vnet-integration.png)

Эта функция позволяет решить проблему доступа к ресурсам в других виртуальных сетей и даже может использоваться для подключения через виртуальную сеть к другим виртуальным сетям или даже локально. Он не работает с подключенные ExpressRoute виртуальные сети но не с помощью VPN сеть связанные сети. Это обычно подходят для использования этой функции из приложения в службы приложений среда (ASE), так как ASE уже находится в виртуальной сети. Ниже приведены варианты использования, эта функция позволяет решить.

* Доступ к ресурсам на частные IP-адреса в виртуальных сетях Azure 
* Доступ к ресурсам в локальной имеется ли VPN-Подключение сайт сайт 
* Доступ к ресурсам в пиринговых виртуальных сетях 

Если эта функция включена, приложение будет использовать DNS-сервер, в которой настроена целевая виртуальная сеть. Дополнительные сведения об этой возможности в документации на [интеграции виртуальной сети для службы приложений][vnetintegrationp2s]. 

### <a name="vnet-integration"></a>интеграция с виртуальной сетью;

Шлюз требуется интеграции с виртуальной сетью, очень полезно, но по-прежнему не решает доступ к ресурсам через ExpressRoute. На основе необходимости взаимодействовать через подключения ExpressRoute, нет необходимости для приложений иметь возможность вызывать конечную точку службы, защищенным службам. Чтобы решить оба этих дополнительных требований, была добавлена другая возможность интеграции с виртуальной сетью. Новая функция интеграции с виртуальной сетью позволяет поместить серверной части приложения в подсети в виртуальной сети Resource Manager, в том же регионе. Эта функция не доступен из среды службы приложений, которая уже находится в виртуальной сети. Возможности этой функции:

* Доступ к ресурсам в виртуальные сети Resource Manager в одном регионе
* Доступ к ресурсам, защищенным с помощью конечных точек службы 
* Доступ к ресурсам, которые доступны через ExpressRoute или VPN-подключения

![интеграция с виртуальной сетью;](media/networking-features/vnet-integration.png)

Эта функция доступна в предварительной версии и не должны использоваться для производственных рабочих нагрузок. Дополнительные сведения об этой функции, читайте документацию на [интеграции виртуальной сети для службы приложений][vnetintegration].

## <a name="app-service-environment"></a>Среда службы приложений 

Среда службы приложений (ASE) — это развертывание одного клиента службы приложений Azure, который работает в виртуальной сети. ASE позволяет варианты использования, такие как:

* Доступ к ресурсам в виртуальной сети
* Доступ к ресурсам в ExpressRoute
* Предоставление приложений с частным адресом в виртуальной сети 
* Доступ к ресурсам через конечные точки службы 

При использовании среды ASE вы не обязательно должны использовать функции, такие как интеграция виртуальной сети или конечные точки службы, поскольку ASE уже находится в виртуальной сети. Если вы хотите получить доступ к ресурсам, например SQL или хранилища через конечные точки службы, включите конечные точки службы в подсети ASE. Если вы хотите получить доступ к ресурсам в виртуальной сети, нет каких-либо дополнительных настроек.  Если вы хотите получить доступ к ресурсам через ExpressRoute, вы уже находятся в виртуальной сети и не требуется ничего настраивать ASE или приложение в нем. 

Так как приложения в ASE с ILB может быть выставлено на частный IP-адрес, можно легко добавить устройства WAF для предоставления только для приложений, чтобы Интернет и защиту остальной. Он пригоден для простой разработки многоуровневых приложений. 

Есть несколько моментов, которые еще не доступно начиная с версии мультитенантная служба из ASE. Те, включают в себя:

* Предоставлять свои приложения в частный IP-адрес
* Защитить весь исходящий трафик с элементами управления сети, которые не являются частью приложения 
* Размещать приложения в службе одного клиента 
* Увеличение масштаба до гораздо большего числа экземпляров, чем в случае в мультитенантной службе 
* Закрытые сертификаты клиента ЦС нагрузки для использования с приложениями с помощью закрытого центра сертификации защиты конечных точек 
* Принудительно TLS 1.1 во всех приложениях, размещенных в системе без любой возможность отключения на уровне приложения 
* Укажите выделенную исходящий адрес для всех приложений в среде ASE, не распространяется на все клиенты 

![ASE в виртуальной сети](media/networking-features/app-service-environment.png)

ASE предоставляет лучше всего представить данные вокруг размещение приложений изолированную и выделенную, но в с определенными трудностями управления. Ниже приведены некоторые моменты, которые следует учитывать перед использованием в рабочей среде ASE.
 
 * ASE выполняется внутри виртуальной сети, но имеют зависимости за пределами виртуальной сети. Необходимо разрешить эти зависимости. Дополнительные сведения в [рекомендации по сетям для среды службы приложений][networkinfo]
 * ASE не масштабируется сразу же как и служба несколькими клиентами. Вам нужно спрогнозировать удовлетворения потребностей в масштабировании, а не по факту масштабирования. 
 * ASE имеют более высокий заранее затраты, связанные с ней. Чтобы максимально эффективно использовать свою среду ASE, вы должны планируете долгие одна среда ASE многих рабочих нагрузок, а не его использовать для внедрения инициатив по small
 * Приложениям в ASE нельзя ограничить доступ к приложениям в ASE с внутренним и не разрешать этого другим.
 * ASE находится в подсети и какие-либо сетевые правила применяются к весь трафик из ASE. Если вы хотите назначить правила входящего трафика для только одно приложение, используйте ограничения доступа. 

## <a name="combining-features"></a>Объединение функций 

Функции, которые указаны для нескольких клиентов службы могут использоваться совместно для решения более сложных вариантов использования. Здесь описываются два наиболее распространенных вариантов использования, но они доступны только в качестве примера. Понимая, что сделать различных функций, вы можете решить почти все потребности архитектуры системы.

### <a name="inject-app-into-a-vnet"></a>Внедрить приложение в виртуальной сети

Распространенным требованием является о том, как для размещения приложения в виртуальной сети. Размещение приложения в виртуальной сети, это означает, что конечные точки входящего и исходящего трафика для приложения, в виртуальной сети. ASE обеспечивает лучшее решение для решения этой проблемы, но вы можете получить большую часть необходимых с несколькими клиентами службы, объединяя функции. Например можно разместить только приложения интрасети с частными адресами входящего и исходящего трафика по:

* Создание шлюза приложений с частным адресом входящего и исходящего трафика
* Защита входящего трафика в приложение с помощью конечных точек службы 
* Используется новая Интеграция виртуальной сети, поэтому серверной части приложения в виртуальной сети 

Этот стиль развертывания бы не дают выделенный адрес для исходящего трафика в Интернете и предоставляют следующие возможности для блокировки всего исходящего трафика из вашего приложения.  Этот стиль развертывания даст соответствующая часть только в противном случае максимально с ASE. 

### <a name="create-multi-tier-applications"></a>Создавать многоуровневые приложения

Многоуровневое приложение является приложением, где приложения API серверной части может осуществляться только из внешнего уровня. Чтобы создать многоуровневое приложение, вы можете:

* Использовать интеграцию с виртуальной сетью для подключения в серверную часть для внешнего интерфейса веб-приложения с подсетью в виртуальной сети
* Использовать конечные точки службы для защиты входящего трафика для своего приложения API для поступающих только из подсети, используемой интерфейсный веб-приложения

![многоуровневого приложения](media/networking-features/multi-tier-app.png)

Может иметь несколько внешние приложения. Используйте то же приложение API с помощью интеграции виртуальной сети из других внешние приложения и конечные точки службы из приложения API с соответствующим подсетям.  

<!--Links-->
[appassignedaddress]: https://docs.microsoft.com/azure/app-service/app-service-web-tutorial-custom-ssl
[iprestrictions]: https://docs.microsoft.com/azure/app-service/app-service-ip-restrictions
[serviceendpoints]: https://docs.microsoft.com/azure/app-service/app-service-ip-restrictions
[hybridconn]: https://docs.microsoft.com/azure/app-service/app-service-hybrid-connections
[vnetintegrationp2s]: https://docs.microsoft.com/azure/app-service/web-sites-integrate-with-vnet
[vnetintegration]: https://docs.microsoft.com/azure/app-service/web-sites-integrate-with-vnet
[networkinfo]: https://docs.microsoft.com/azure/app-service/environment/network-info
