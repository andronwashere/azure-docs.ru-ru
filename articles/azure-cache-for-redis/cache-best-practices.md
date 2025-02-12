---
title: Советы и рекомендации для кэша Redis для Azure
description: Узнайте, как эффективно использовать кэш Azure для Redis, следуя приведенным ниже рекомендациям.
services: cache
documentationcenter: na
author: joncole
manager: jhubbard
editor: tysonn
ms.assetid: 3e4905e3-89e3-47f7-8cfb-12caf1c6e50e
ms.service: cache
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: cache
ms.workload: tbd
ms.date: 06/21/2019
ms.author: joncole
ms.openlocfilehash: bdc75033e0aa2e401a511789728feef3248d46ad
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/28/2019
ms.locfileid: "67452466"
---
# <a name="best-practices-for-azure-cache-for-redis"></a>Советы и рекомендации для кэша Redis для Azure 
Следуя приведенным ниже рекомендациям, помогает добиться максимальной производительности и экономичное использование кэша Azure для экземпляра Redis.

## <a name="configuration-and-concepts"></a>Конфигурация и основные понятия
 * **Используйте уровень "стандартный" или "премиум" для рабочих систем.**  Уровень "базовый" — это система одним узлом без репликации данных и нет соглашения об уровне ОБСЛУЖИВАНИЯ. Кроме того, используйте по крайней мере кэш C1.  Кэши C0 предназначены для простой разработки и тестирования сценариев, так как они имеют общее ядро ЦП, объем памяти и могут быть проблемы «шумного соседа».

 * **Помните, что Redis — это хранилище данных в памяти.**  [В этой статье](https://gist.github.com/JonCole/b6354d92a2d51c141490f10142884ea4#file-whathappenedtomydatainredis-md) описаны некоторые сценарии, где может произойти потеря данных.

 * **Создайте свою систему таким образом, что он может обрабатывать кратковременные сбои подключения** [из-за установки исправлений и отработки отказа](https://gist.github.com/JonCole/317fe03805d5802e31cfa37e646e419d#file-azureredis-patchingexplained-md).

 * **Настройка вашей [зарезервированная параметр](cache-configure.md#maxmemory-policy-and-maxmemory-reserved) повысить скорость реагирования системы** условиях нехватки памяти.  Этот параметр особенно важен для рабочих нагрузок с интенсивными операциями записи, или если вы храните большие значения (100 КБ или больше) в Redis.  Я бы рекомендуем начать с 10% от размера кэша, а затем увеличить, если у вас есть нагрузок с интенсивными операциями записи. См. в разделе [некоторые соображения](cache-how-to-troubleshoot.md#considerations-for-memory-reservations) при выборе значения.

 * **Лучше всего redis работает с меньшими значениями**, поэтому рассмотрите возможность разделения больших данных на несколько ключей.  В [этом обсуждении Redis](https://stackoverflow.com/questions/55517224/what-is-the-ideal-value-size-range-for-redis-is-100kb-too-large/), приведены некоторые соображения следует тщательно проанализировать.  Прочитайте [эту статью](cache-how-to-troubleshoot.md#large-requestresponse-size) , чтобы ознакомиться с примером проблемы, вызванной большими значениями.

 * **Найдите свой экземпляр кэша и приложения в одном регионе.**  Подключения к кэшу в другом регионе можно увеличить задержку и значительно снизить надежность.  Хотя вы можете подключиться из за пределами Azure, его не рекомендуется делать *особенно при использовании в качестве кэша Redis*.  Если вы используете Redis как только хранилище ключ значение, задержка может оказаться главной проблемой. 

 * **Повторное использование подключений** -создать новое подключение является дорогостоящим и увеличивает задержку, поэтому повторное использование подключений максимально. Если вы решили создать новые подключения, не забудьте закрыть старые подключения, прежде чем их выпуска (даже в управляемой памяти языков .NET или Java).

 * **Настройка библиотеки клиента для использования *время ожидания подключения* по крайней мере 15 секунд**, предоставляя системное время для подключения, даже выше условиях ЦП.  Значение времени ожидания подключения не гарантирует, что подключение будет установлено в этом интервале времени.  Если что-то неправильный переход (ЦП высокой клиента, высокой нагрузки ЦП и т. д.), а затем значение времени ожидания короткие соединение вызовет попытка подключения завершится ошибкой. Это поведение часто делает хуже ситуацию.  Вместо помощь, короткое время ожидания к ухудшению неполадку путем перезагрузки компьютера, чтобы перезапустить процесс повторного подключения, что может привести к *подключения "->" fail "->" повтора* цикла. Обычно рекомендуется оставить подключения время ожидания составляет 15 секунд или более поздней версии. Лучше позволить невозможны после 15 или 20 секунд, а не для его завершатся быстро попытка подключения только для того, чтобы повторить попытку. Цикл повторных попыток может вызвать сбой таким образом, чтобы длится дольше, чем если просто позволить системе занять больше изначально.  
     > [!NOTE]
     > Это руководство относится только к *попытки подключения* и не связано с времени, вы хотите дождаться *операции* как GET или SET для завершения.
 

 * **Избежать ресурсоемкие команды** -некоторые redis операций, таких как [команду ключи](https://redis.io/commands/keys), являются *очень* дорогостоящим и следует избегать.  Дополнительные сведения см. в разделе [некоторые рекомендации, связанные с ресурсоемкие команды](cache-how-to-troubleshoot.md#expensive-commands)


 
## <a name="memory-management"></a>Управление памятью
Существует несколько моментов, связанные с использованием памяти в экземпляре сервера Redis, который может понадобиться рассмотреть.  Вот некоторые из них:

 * **Выберите [политика вытеснения](https://redis.io/topics/lru-cache) , подходит для вашего приложения.**  Политика по умолчанию Redis для Azure — *volatile-lru*, означающее, что задано значение только ключи, которые имеют срок ЖИЗНИ могут быть обновлены для вытеснения.  Если ключи не могут иметь значение TTL, система не будет исключить любые ключи.  Если вы хотите системы, чтобы разрешить любую клавишу для удаления при нехватке памяти, то может понадобиться рассмотреть *allkeys-lru* политики.

 * **Значение срока действия ключей.**  Это приведет к удалению ключей заранее, а не ждать, пока имеется достаточный объем памяти.  Когда вытеснения запускаются из-за нехватки памяти, это может вызвать дополнительную нагрузку на сервере.  Дополнительные сведения см. в документации по [срок](https://redis.io/commands/expire) и [ExpireAt](https://redis.io/commands/expireat) команды.
 
## <a name="client-library-specific-guidance"></a>Конкретные рекомендации по использованию клиентских библиотек
 * [StackExchange.Redis (.NET)](https://gist.github.com/JonCole/925630df72be1351b21440625ff2671f#file-redis-bestpractices-stackexchange-redis-md)
 * [Java - какой клиент использовать?](https://gist.github.com/warrenzhu25/1beb02a09b6afd41dff2c27c53918ce7#file-azure-redis-java-best-practices-md)
 * [Салат (Java)](https://gist.github.com/warrenzhu25/181ccac7fa70411f7eb72aff23aa8a6a#file-azure-redis-lettuce-best-practices-md)
 * [Jedis (Java)](https://gist.github.com/JonCole/925630df72be1351b21440625ff2671f#file-redis-bestpractices-java-jedis-md)
 * [Node.js](https://gist.github.com/JonCole/925630df72be1351b21440625ff2671f#file-redis-bestpractices-node-js-md)
 * [PHP](https://gist.github.com/JonCole/925630df72be1351b21440625ff2671f#file-redis-bestpractices-php-md)
 * [Поставщик состояний сеансов Asp.Net](https://gist.github.com/JonCole/925630df72be1351b21440625ff2671f#file-redis-bestpractices-session-state-provider-md)


## <a name="when-is-it-safe-to-retry"></a>Если это безопасно повторить попытку?
К сожалению нет простого ответа нет.  Каждое приложение необходимо решить, какие операции могут быть повторены, а какие нельзя.  Каждая операция имеет другие требования и зависимости между ключа.  Ниже приведены некоторые действия, которые может потребоваться.

 * Вы можете получить ошибки на стороне клиента, несмотря на то, что Redis успешно выполнена команда которую его запуск.  Пример:
     - Время ожидания — это понятие стороне клиента.  Если операцию достигает сервера, сервера будет выполнена команда, даже если клиент предоставляет в состоянии ожидания.  
     - При возникновении ошибки на подключение к сокету, не сможете узнать, если операция в действительности выполняются на сервере.  Например ошибка подключения может произойти после обработки запроса сервером, но до получения ответа клиенту.
 *  Как мое приложение реагируют если случайно запустить ту же операцию дважды?  Например что делать, если увеличивается целое дважды, а не только один раз?  Мое приложение записывает тому же ключу из нескольких мест?  Что делать, если Моя логика повторных попыток перезаписывает значение, заданное параметром другой части Мое приложение?

Если вы хотите проверить, как код работает в условиях ошибки, рассмотрите возможность использования [перезагрузить функция](cache-administration.md#reboot). Это позволяет увидеть, как кратковременные сбои подключения влияют на приложения.

## <a name="performance-testing"></a>Тестирование производительности
 * **Запустить с помощью `redis-benchmark.exe`**  почувствовать возможная пропускная способность и задержка перед написанием собственных perf тестов.  Redis-benchmark документации может быть [см. Здесь](http://redis.io/topics/benchmarks).  Обратите внимание, что redis-benchmark не поддерживает протокол SSL, поэтому вам придется [включить порт без SSL посредством портала](cache-configure.md#access-ports) перед запуском теста.  [Совместимой версии redis-benchmark.exe windows можно найти здесь](https://github.com/MSOpenTech/redis/releases)
 * Клиент виртуальной Машины, используемой для тестирования должен быть **в одном регионе** как экземпляр кэша Redis.
 * **Мы рекомендуем использовать виртуальные Машины серии Dv2** для клиентских, так как они более производительное оборудование и дает лучшие результаты.
 * Убедитесь, что используется виртуальная машина имеет клиент **по крайней мере столько вычисления и пропускная способность* тестируемой кэшу. 
 * **Включите VRSS** на клиентском компьютере при работе в Windows.  [Щелкните здесь, чтобы узнать больше](https://technet.microsoft.com/library/dn383582(v=ws.11).aspx).  Пример сценария powershell:
     >PowerShell - ExecutionPolicy Unrestricted Enable-NetAdapterRSS-имя (Get-NetAdapter). Имя 
     
 * **Рассмотрите возможность использования экземпляры Redis уровня "премиум"** .  Эти размеры кэша будет иметь лучше задержки в сети и пропускной способности, так как они работают на более производительное оборудование для ЦП и сети.
 
     > [!NOTE]
     > Наши результаты производительности, наблюдаемой [опубликованные здесь](cache-faq.md#azure-cache-for-redis-performance) для справки.   Кроме того Имейте в виду, что SSL/TLS увеличивает непроизводительные затраты ресурсов, поэтому можно получить разные задержки и пропускной способности при использовании шифрования транспорта.
 
### <a name="redis-benchmark-examples"></a>Примеры redis-Benchmark
**Предтестовая Настройка**: Это будет подготовить экземпляр кэша с данными, необходимыми для задержки и пропускной способности тестирование команд, перечисленных ниже.
> redis-benchmark.exe -h yourcache.redis.cache.windows.net -a yourAccesskey -t SET -n 10 -d 1024 

**Для тестирования задержки**: Таким образом можно проверить запросы GET с полезными данными 1 КБ.
> redis-benchmark.exe -h yourcache.redis.cache.windows.net -a yourAccesskey -t GET -d 1024 -P 50 -c 4

**Для тестирования пропускной способности.** Этот механизм использует запросы конвейерная GET с полезными данными 1 КБ.
> redis-benchmark.exe -h yourcache.redis.cache.windows.net -a yourAccesskey -t  GET -n 1000000 -d 1024 -P 50  -c 50
