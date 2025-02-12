---
title: Сброс TCP-подключений Load Balancer при простое в Azure
titlesuffix: Azure Load Balancer
description: Load Balancer с двунаправленными TCP-пакетами RST при истечении времени ожидания простоя
services: load-balancer
documentationcenter: na
author: asudbring
ms.custom: seodec18
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/03/2019
ms.author: allensu
ms.openlocfilehash: 8485f4b6e8d4ff55de4930b3cfb7a07802cf1d41
ms.sourcegitcommit: 9a699d7408023d3736961745c753ca3cec708f23
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/16/2019
ms.locfileid: "68274161"
---
# <a name="load-balancer-with-tcp-reset-on-idle-public-preview"></a>Load Balancer со сбросом TCP-подключений при простое (общедоступная предварительная версия)

Чтобы создать более предсказуемое поведение приложения для ваших сценариев, вы можете использовать [Load Balancer (цен. категория "Стандартный")](load-balancer-standard-overview.md), включив сброс протокола TCP в неактивный режим для данного правила. По умолчанию, когда истекает время ожидания простоя потока, Load Balancer автоматически прерывает его.  Включение этой функции приведет к тому, что Load Balancer будет отправлять двунаправленные сбросы TCP (пакет RST TCP) по истечении времени простоя.  Это сообщит конечным точкам вашего приложения о том, что время соединения истекло и оно больше не используется.  При необходимости конечные точки могут немедленно установить новое соединение.

![Сброс TCP-подключений в Load Balancer](media/load-balancer-tcp-reset/load-balancer-tcp-reset.png)

>[!NOTE] 
>В настоящее время в данный момент в качестве общедоступной предварительной версии можно использовать Load Balancer с сбросом TCP в режиме ожидания простоя. Предварительная версия предоставляется без соглашения об уровне обслуживания. Не рекомендуем использовать ее в рабочей среде. Некоторые функции могут не поддерживаться или их возможности могут быть ограничены. См. [дополнительные условия использования для предварительных версий Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).
 
Изменить это поведение по умолчанию и включить отправку признаков TCP Reset при истечении времени ожидания простоя можно в правилах NAT для входящего трафика, правилах балансировки нагрузки и [правилах для исходящего трафика](https://aka.ms/lboutboundrules).  Если эта функция включена для отдельного правила, Load Balancer отправляет двунаправленные признаки TCP Reset (TCP-пакеты RST) клиентским и серверным конечным точкам при истечении времени ожидания простоя для всех соответствующих потоков.

Конечные точки, получившие TCP-пакеты RST, немедленно закрывают соответствующий сокет. Таким образом конечные точки немедленно уведомляются об освобождении соединения, и дальнейшие попытки передачи данных по этому TCP-соединению будут завершаться неудачно.  Приложения могут очищать соединения при закрытии сокета и повторно устанавливать их по мере необходимости, не дожидаясь, когда время ожидания TCP-соединения окончательно истечет.

Во многих ситуациях это позволяет уменьшить количество сообщений об активности TCP (или прикладного уровня), которые требуется отправлять для обновления времени ожидания потока. 

Однако если время простоя превышает допускаемое конфигурацией или приложение демонстрирует нежелательное поведение при включенном сбросе TCP, вам может требоваться использовать сообщения об активности TCP (или прикладного уровня) для отслеживания активности TCP-соединений.  Кроме того, сообщения об активности, особенно об активности прикладного уровня, могут по-прежнему быть полезными, если где-то на пути соединения есть прокси-сервер.  

Тщательно проанализируйте весь сценарий, чтобы решить, даст ли преимущества включение сброса TCP и настройка времени ожидания простоя, а также потребуются ли дополнительные действия для обеспечения требуемого поведения приложения.

## <a name="enabling-tcp-reset-on-idle-timeout"></a>Включение сброса подключений TCP при истечении времени ожидания простоя

С помощью интерфейса API версии 2018-07-01 можно включить двунаправленную отправку пакетов TCP Reset при истечении времени ожидания простоя для отдельных правил:

```json
      "loadBalancingRules": [
        {
          "enableTcpReset": true | false,
        }
      ]
```

```json
      "inboundNatRules": [
        {
          "enableTcpReset": true | false,
        }
      ]
```

```json
      "outboundRules": [
        {
          "enableTcpReset": true | false,
        }
      ]
```

## <a name="regions"></a> Доступность по регионам

Доступно во всех регионах.

## <a name="limitations"></a>Ограничения

- С помощью портала нельзя настраивать или просматривать состояние функции сброса TCP-подключений.  Вместо этого используйте шаблоны, REST API, Azure CLI 2.0 или PowerShell.
- TCP RST отправлен только во время TCP-соединения в УСТАНОВЛЕНном состоянии.

## <a name="next-steps"></a>Следующие шаги

- Сведения о [Load Balancer ценовой категории "Стандартный"](load-balancer-standard-overview.md).
- Сведения о [правилах для исходящих подключений](load-balancer-outbound-rules-overview.md).
