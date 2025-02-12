---
title: Устойчивость и аварийное восстановление Службы Azure SignalR
description: Общие сведения о настройке нескольких экземпляров службы SignalR для обеспечения устойчивости и аварийного восстановления
author: chenkennt
ms.service: signalr
ms.topic: conceptual
ms.date: 03/01/2019
ms.author: kenchen
ms.openlocfilehash: eb70e65db4a086afc60e91cadf55a8844b102591
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "61402155"
---
# <a name="resiliency-and-disaster-recovery"></a>Устойчивость и аварийное восстановление

Устойчивость и аварийное восстановление — это одно из основных требований практически ко всем веб-системам. Служба Azure SignalR гарантирует показатель доступности 99,9 %, но по-прежнему работает в пределах региона.
Экземпляр службы всегда работает в одном регионе и не отработки отказа в другой регион при сбое всего региона.

Вместо этого в пакете SDK службы предусмотрены функции для поддержки нескольких экземпляров Службы Azure SignalR и автоматического переключения на другие экземпляры, если какой-либо из экземпляров недоступен.
С помощью этой функции вы можете восстановить работу в случае аварийного отключения, но нужно будет самостоятельно и правильно настроить топологию системы. Из этого документа вы узнаете, как выполнить настройку.

## <a name="high-available-architecture-for-signalr-service"></a>Отказоустойчивая архитектура для Службы Azure SignalR

Чтобы обеспечить устойчивость между регионами для службы Azure SignalR, настройте несколько экземпляров службы в разных регионах. В таком случае, если один из регионов не работает, другие регионы можно использовать для резервного копирования.
При подключении нескольких экземпляров службы к серверу приложения используются две роли экземпляра — первичный и вторичный.
Первичный экземпляр обрабатывает весь онлайн-трафик. Вторичный экземпляр также полностью работает, но является резервным для первичного.
В нашей реализации пакета SDK функция согласования будет возвращать только первичные конечные точки, поэтому обычно клиенты подключаются только к первичным конечным точкам.
Но когда первичный экземпляр не работает, функция согласования возвращает конечные точки вторичного экземпляра, чтобы клиент по-прежнему смог подключиться.
Экземпляра сервера-источника и сервера приложений соединены через обычный подключения а экземпляра-получателя и сервера приложений через специальный тип подключения, называется слабой соединением.
Основное различие слабого соединения заключается в том, что она не принимает маршрутизации для подключения клиента, так как второй экземпляр находится в другом регионе. Маршрутизация клиента в другой регион не является оптимальным решением (увеличивается задержка).

При подключении к нескольким серверам приложений у одного экземпляра службы могут быть разные роли.
В стандартной конфигурации межрегионального сценария используются две (или более) пары экземпляров Службы SignalR и серверов приложений.
В каждой паре сервер приложений и Служба SignalR расположены в одном и том же регионе. Служба SignalR подключена к серверу приложений как к первичному.
Кроме того, между каждой парой подключены сервер приложений и Служба SignalR, но при подключении к серверу в другом регионе Служба SignalR становится вторичной.

Так как все серверы приложений и экземпляры Службы SignalR взаимосвязаны, при использовании этой топологии сообщение от одного сервера по-прежнему может доставляться всем клиентам.
Если клиент подключен, чтобы обеспечить оптимальную сетевую задержку, он всегда направляется на сервер приложений в этом же регионе.

На схеме ниже показан пример такой топологии сети:

![топология](media/signalr-concept-disaster-recovery/topology.png)

## <a name="configure-app-servers-with-multiple-signalr-service-instances"></a>Настройка серверов приложений с несколькими экземплярами Службы SignalR

Если вы используете Службу Azure SignalR и серверы приложений, созданные в каждом регионе, серверы приложений можно настроить для подключения ко всем экземплярам Службы SignalR.

Это можно сделать двумя способами:

### <a name="through-config"></a>С помощью файла конфигурации.

Должно было быть известно как задать строку подключения SignalR службы через settings/web.cofig приложения или переменные среды, через запись конфигурации с именем `Azure:SignalR:ConnectionString`.
Если у вас есть несколько конечных точек, их можно задать в нескольких записях в файле конфигурации в следующем формате:

```
Azure:SignalR:Connection:<name>:<role>
```

Здесь `<name>` — имя конечной точки, `<role>` — ее роль (первичная или вторичная).
Имя — необязательный параметр, но его удобно использовать, если вы хотите настроить маршрутизацию между несколькими конечными точками.

### <a name="through-code"></a>С помощью кода.

Если вы предпочитаете хранить строку подключения в другом расположении, можно прочитать ее с помощью кода и использовать полученные данные как параметры при вызове `AddAzureSignalR()` (в ASP.NET Core) или `MapAzureSignalR()` (в ASP.NET).

Пример кода:

ASP.NET Core:

```cs
services.AddSignalR()
        .AddAzureSignalR(options => options.Endpoints = new ServiceEndpoint[]
        {
            new ServiceEndpoint("<connection_string1>", EndpointType.Primary, "region1"),
            new ServiceEndpoint("<connection_string2>", EndpointType.Secondary, "region2"),
        });
```

ASP.NET:

```cs
app.MapAzureSignalR(GetType().FullName, hub,  options => options.Endpoints = new ServiceEndpoint[]
    {
        new ServiceEndpoint("<connection_string1>", EndpointType.Primary, "region1"),
        new ServiceEndpoint("<connection_string2>", EndpointType.Secondary, "region2"),
    };
```

## <a name="failover-sequence-and-best-practice"></a>Последовательность отработки отказа и рекомендации по ней

Теперь топология вашей системы настроена правильно. Если один экземпляр Службы Azure SignalR не работает, онлайн-трафик направляется к другим экземплярам.
Вот что происходит, когда первичный экземпляр не работает (и восстанавливает работу через некоторое время):

1. Первичный экземпляр службы не работает, все подключения между серверами и этим экземпляром будут разорваны.
2. Все серверы, подключенные к этому экземпляру, пометят его как автономный, функция согласования прекратит возвращать эту конечную точку и начнет возвращать вторичную.
3. Все подключения клиентов в данном экземпляре закрываются, и клиенты подключаются повторно. Так как серверы приложений теперь возвращают вторичную конечную точку, клиенты будут подключаться к вторичному экземпляру.
4. Теперь вторичный экземпляр принимает весь онлайн-трафик. Все сообщения от сервера к клиентам по-прежнему доставляются, так как вторичный сервер подключен ко всем серверам приложений. Сообщения от клиента к серверу направляются только на сервер приложений в этом же регионе.
5. После восстановления первичного экземпляра и его повторного подключения сервер приложений восстанавливает подключения к экземпляру и помечает его как подключенный. Функция согласования теперь снова возвращает первичную конечную точку. Таким образом, новые клиенты повторно подключаются к первичному экземпляру. Для подключенных ранее клиентов соединение не сбрасывается, и они по-прежнему направляется во вторичный регион, пока не отключаются сами.

На схемах ниже показано, как в Службе Azure SignalR осуществляется аварийное переключение:

Рис. 1. Перед отработкой отказа ![Before Failover](media/signalr-concept-disaster-recovery/before-failover.png)

Рис. 2. После отработки отказа ![After Failover](media/signalr-concept-disaster-recovery/after-failover.png)

Рис. 3. Спустя некоторое время после восстановления первичной службы ![Short time after primary recovers](media/signalr-concept-disaster-recovery/after-recover.png)

Обычно онлайн-трафик обрабатывает только основной сервер приложений и первичная Служба ignalR (показано синим).
После отработки отказа вторичный сервер приложений и вторичная Служба Azure SignalR также активируются.
После повторного подключения первичной Службы Azure SignalR новые клиенты будут подключаться к ней. При этом клиенты продолжают подключаться к вторичному экземпляру, поэтому оба экземпляра принимают трафик.
После отключения всех подключенных клиентов ваша система перейдет в обычное состояние (рис. 1).

Ниже описаны два основных вида реализации архитектуры высокого уровня, доступной между регионами.

1. Первый — пара серверов приложений и экземпляр Службы SignalR принимают весь онлайн-трафик, а другая пара используется для резервного копирования (режим "активный — пассивный", см. рис. 1). 
2. Другой — две (или более) пары серверов приложений и экземпляров Службы SignalR, каждая из которых принимает участие в обработке онлайн-трафика и служит резервной копией для других пар (режим "активный — активный", см. рис. 3).

Служба SignalR может поддерживать оба шаблона. Основное различие заключается в том, как реализованы серверы приложений.
Если серверы приложений работают в режиме "активный — пассивный", Служба SignalR также будет работать в режиме "активный — пассивный" (так как основной сервер приложений возвращает только свой первичный экземпляр Службы SignalR).
Если серверы приложений работают в режиме "активный — активный", Служба SignalR также будет работать в режиме "активный — активный" (так как все серверы приложений будут возвращать свои собственные первичные экземпляры Службы SignalR, чтобы все они могли получать трафик).

Отметить независимо от того, какие шаблоны, вы решили использовать, вам потребуется для подключения к серверу приложений основным каждый экземпляр службы SignalR.

Кроме того, из-за характера подключения Службы Azure SignalR (длинное подключение), когда происходит сбой и аварийное переключение, подключение клиентов будет обрываться.
Такие случаи необходимо обрабатывать на стороне клиента, чтобы предоставить своим конечным клиентам всю необходимую информацию. Например, подключитесь повторно после закрытия подключения.

## <a name="next-steps"></a>Дальнейшие действия

Из этой статьи вы узнали, как настроить приложение и обеспечить устойчивость Службы Azure SignalR. Дополнительные сведения о подключении "сервер — клиент" и маршрутизации подключений Службы Azure SignalR см. в [этой статье](signalr-concept-internals.md) с описанием внутренних компонентов Службы Azure SignalR.

Для масштабирования сценариев, например, Сегментирование, совместно использовать несколько экземпляров для обработки большого количества подключений, чтение [как масштабировать несколько экземпляров](signalr-howto-scale-multi-instances.md)?