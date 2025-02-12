---
title: Отслеживание кэша Azure для Redis | Документация Майкрософт
description: Узнайте, как отслеживать работоспособность и производительность экземпляров кэша Azure для Redis
services: cache
documentationcenter: ''
author: yegu-ms
manager: jhubbard
editor: ''
ms.assetid: 7e70b153-9c87-4290-85af-2228f31df118
ms.service: cache
ms.workload: tbd
ms.tgt_pltfrm: cache
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: yegu
ms.openlocfilehash: 2cfd5a99144af1120afbf06fe6222228a9332bb6
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "65787427"
---
# <a name="how-to-monitor-azure-cache-for-redis"></a>Как отслеживать кэш Azure для Redis
В кэше Azure для Redis используется [Azure Monitor](https://docs.microsoft.com/azure/monitoring-and-diagnostics/) для обеспечения нескольких параметров мониторинга экземпляров кэша. Вы можете просматривать метрики, закреплять диаграммы метрик на начальной панели, настраивать диапазоны дат и времени для диаграмм мониторинга, добавлять и удалять метрики в диаграммах, а также настраивать отправку оповещений при выполнении определенных условий. Эти инструменты позволяют наблюдать за работоспособностью экземпляров кэша Azure для Redis и помогают управлять кэшируемыми приложениями.

Метрики для экземпляров кэша Azure для Redis собираются с помощью команды Redis [INFO](https://redis.io/commands/info) примерно два раза в минуту и автоматически хранятся в течение 30 дней (если вы хотите настроить другую политику хранения, см. сведения в разделе [Export cache metrics](#export-cache-metrics) (Экспорт метрик кэша)). Таким образом, их можно открыть в диаграммах метрик и оценить с помощью правил генерации оповещений. Дополнительные сведения о различных значениях INFO, используемых для каждой метрики кэша, см. в разделе [Доступные метрики и интервалы отчетности](#available-metrics-and-reporting-intervals).

<a name="view-cache-metrics"></a>

Чтобы просмотреть метрики кэша, [перейдите](cache-configure.md#configure-azure-cache-for-redis-settings) к своему экземпляру кэша на [портале Azure](https://portal.azure.com).  Кэш Azure для Redis предоставляет несколько встроенных диаграмм в колонках **Обзор** и **Метрики Redis**. Каждую из этих диаграмм можно настроить путем добавления и удаления метрик либо изменения интервала отчетности.

![Метрики Redis](./media/cache-how-to-monitor/redis-cache-redis-metrics-blade.png)

## <a name="view-pre-configured-metrics-charts"></a>Просмотр предварительно настроенных диаграмм метрик

Колонка **Обзор** содержит следующие предварительно настроенные диаграммы мониторинга.

* [Диаграммы мониторинга](#monitoring-charts)
* [Диаграммы использования](#usage-charts)

### <a name="monitoring-charts"></a>Диаграммы мониторинга
В разделе **Мониторинг** колонки **Обзор** есть диаграммы **Попадания и промахи**, **Извлечения и занесения**, **Подключения** и **Всего команд**.

![Диаграммы мониторинга](./media/cache-how-to-monitor/redis-cache-monitoring-part.png)

### <a name="usage-charts"></a>Диаграммы использования
В разделе **Использование** колонки **Обзор** представлены диаграммы **Нагрузка сервера Redis**, **Использование памяти**, **Пропускная способность сети** и **Использование ЦП**, а также указана **ценовая категория** для экземпляра кэша.

![Диаграммы использования](./media/cache-how-to-monitor/redis-cache-usage-part.png)

В разделе **Ценовая категория** отображается ценовая категория кэша, и здесь его можно [перевести](cache-how-to-scale.md) в другую категорию.

## <a name="view-metrics-with-azure-monitor"></a>Просмотр метрик с помощью Azure Monitor
Чтобы просмотреть метрики Redis и создать собственные диаграммы в Azure Monitor, щелкните **Метрики** в меню **Ресурсы**. Здесь вы сможете настроить тип диаграммы, отображаемые метрики, интервал составления отчетов и другие параметры.

![Метрики Redis](./media/cache-how-to-monitor/redis-cache-monitor.png)

Дополнительные сведения о работе с метриками в Azure Monitor см. в статье [Обзор метрик в Microsoft Azure](../monitoring-and-diagnostics/monitoring-overview-metrics.md).

<a name="how-to-view-metrics-and-customize-chart"></a>
<a name="enable-cache-diagnostics"></a>
## <a name="export-cache-metrics"></a>Экспорт метрик кэша
По умолчанию в Azure Monitor метрики кэша [хранятся в течение 30 дней](../azure-monitor/platform/data-platform-metrics.md), а затем удаляются. Чтобы хранить метрики кэша дольше 30 дней, назначьте для метрик кэша [выделенную учетную запись хранения](../azure-monitor/platform/archive-diagnostic-logs.md) и укажите для них политику **Хранение (дни)** . 

Чтобы настроить учетную запись хранения для метрик кэша, сделайте следующее:

1. В колонке **кэша Azure для Redis** в **меню ресурсов** щелкните **Диагностика**.
2. Щелкните **Вкл.**
3. Установите флажок **Архивировать в учетной записи хранения**.
4. Выберите учетную запись хранения, в которой будут храниться метрики кэша.
5. Установите флажок **1 минута** и укажите политику **Хранение (дни)** . Если вы не хотите применять политику хранения, а предпочитаете хранить данные неограниченное время, укажите для параметра **Хранение (дни)** значение **0**;
6. Выберите команду **Сохранить**.

![Диагностика Redis](./media/cache-how-to-monitor/redis-cache-diagnostics.png)

>[!NOTE]
>В дополнение к архивированию метрик кэша в хранилище, вы также можете [выполнять их потоковую передачу в концентратор событий или отправку в Azure Monitor журналы](../azure-monitor/platform/rest-api-walkthrough.md#retrieve-metric-values).
>
>

Чтобы просмотреть метрики, их можно открыть на портале Azure, как описано выше в этой статье, или же обратиться к ним с помощью [REST API для метрик Azure Monitor](../azure-monitor/platform/stream-monitoring-data-event-hubs.md).

> [!NOTE]
> В случае ее смены данные из прежней учетной записи хранения можно будет скачать, однако они не будут отображаться на портале Azure.  
> 
> 

## <a name="available-metrics-and-reporting-intervals"></a>Доступные метрики и интервалы отчетности
Показатели кэша регистрируются с определенной периодичностью: **за последний час**, **за сегодня**, **за последнюю неделю** и с **настраиваемым** интервалом. В колонке **Метрика** для каждой диаграммы метрик отображаются среднее, минимальное и максимальное значения каждой метрики на данной диаграмме, а для некоторых также можно увидеть суммарное значение за весь интервал отчетности. 

Каждый показатель имеет две версии. Один показатель измеряет производительность для всего кэша, а также для кэшей, использующих [кластеризацию](cache-how-to-premium-clustering.md), а вторая версия метрики, имя которой содержит `(Shard 0-9)`, измеряет производительность отдельного сегмента в кэше. Например если кэш содержит 4 сегмента, `Cache Hits` отражает общее количество попаданий для всего кэша, а `Cache Hits (Shard 3)` — количество попаданий для отдельного сегмента кэша.

> [!NOTE]
> Даже когда кэш неактивен и к нему не подключены активные клиентские приложения, вы можете наблюдать определенную активность (подключенных клиентов, использование памяти и выполнение операций). Это нормальное явление в штатном режиме работы экземпляра кэша Azure для Redis.
> 
> 

| Метрика | ОПИСАНИЕ |
| --- | --- |
| Попаданий в кэш |Количество успешных операций поиска по ключу за указанный интервал отчетности. Это значение соответствует значению `keyspace_hits` команды [INFO](https://redis.io/commands/info) Redis. |
| Задержка кэша (предварительная версия) | Задержка кэша определяется на основе задержки между узлами кэша. Эта метрика измеряется в микросекундах и содержит три измерения: Avg, Min и Max, которые, соответственно, представляют среднее, минимальное и максимальное время задержки кэша за указанный интервал создания отчетов. |
| Промахов в кэше |Количество неудачных операций поиска по ключу за указанный интервал отчетности. Это значение соответствует значению `keyspace_misses` команды INFO Redis. Промахи в кэше не означают, что с кэшем возникли какие-то неполадки. Например, при использовании шаблона программирования "Отдельно от кэша" приложение сначала ищет элемент в кэше. Если он там отсутствует (то есть фиксируется промах), элемент извлекается из базы данных и добавляется в кэш. Промахи в кэше – нормальное явление для программирования на базе шаблона "Отдельно от кэше". Если количество промахов в кэше больше, чем предполагается, проанализируйте алгоритм приложения, который заполняет кэш и считывает оттуда значения. Промахи также могут возникать в случае удаления из кэша элементов из-за нехватки памяти, однако для контроля состояния памяти лучше использовать метрики `Used Memory` и `Evicted Keys`. |
| Чтение из кэша |Объем данных (в МБ/с), считанных из кэша за указанный интервал отчетности. Это значение рассчитывается на основе трафика сетевых адаптеров, поддерживающих виртуальную машину, на которой размещен кэш, и не связано непосредственно с системой Redis. **Это значение соответствует пропускной способности сети для этого кэша. Если вы хотите настроить оповещения об ограничениях пропускной способности сети на стороне сервера, создайте их с помощью этого счетчика `Cache Read`. В [этой таблице](cache-faq.md#cache-performance) приводятся наблюдаемые ограничения пропускной способности для различных ценовых категорий и размеров кэша.** |
| Запись в кэш |Объем данных (в МБ/с), записываемых в кэш за указанный интервал отчетности. Это значение рассчитывается на основе трафика сетевых адаптеров, поддерживающих виртуальную машину, на которой размещен кэш, и не связано непосредственно с системой Redis. Это значение соответствует пропускной способности сети в отношении данных, отправляемых из кэша клиенту. |
| Подключенные клиенты |Количество подключенных к кэшу клиентов за указанный интервал отчетности. Это значение соответствует значению `connected_clients` команды INFO Redis. При достижении [предельного числа подключений](cache-configure.md#default-redis-server-configuration) последующие попытки подключения к кэшу завершаются ошибкой. Обратите внимание на то, что даже при отсутствии активных клиентских приложений к кэшу может быть подключено несколько клиентов, которые соответствуют внутренним процессам и подключениям. |
| ЦП |Использование ресурсов ЦП сервера кэша Azure для Redis (в виде процентной доли) за указанный интервал отчетности. Это значение соответствует счетчику производительности `\Processor(_Total)\% Processor Time` в операционной системе. |
| Errors | Конкретные сбои и проблемы с производительностью кэша, которые могут возникать за указанный интервал создания отчетов. Эта метрика имеет восемь измерений, представляющих различные типы ошибок (в будущем могут быть добавлены другие измерения). Текущие типы ошибок приведены ниже. <br/><ul><li>**Отработка отказа** — при сбое кэша (подчиненный элемент передает транзакцию в master)</li><li>**Crash** — когда происходит непредвиденный сбой кэша на любом из узлов.</li><li>**Dataloss** — при возникновении потери данных в кэше.</li><li>**UnresponsiveClients** — когда клиенты не считывают данные с сервера достаточно быстро.</li><li>**AOF** — когда возникает проблема, связанная с сохраняемостью AOF.</li><li>**RDB** — когда возникает проблема, связанная с сохраняемостью RDB.</li><li>**Import** — когда возникает проблема, связанная с RDB импорта.</li><li>**Export** — когда возникает проблема, связанная с RDB экспорта.</li></ul> |
| Исключенные ключи |Количество ключей, исключенных из кэша за указанный интервал отчетности из-за ограничения `maxmemory` . Это значение соответствует значению `evicted_keys` команды INFO Redis. |
| Ключи с истекшим сроком действия |Количество ключей, для которых истек срок действия за указанный интервал отчетности. Это значение соответствует значению `expired_keys` команды INFO Redis.|
| Операций считывания |Количество операций считывания из кэша за указанный интервал отчетности. Эта метрика представляет собой сумму следующих значений INFO-команды Redis: `cmdstat_get`, `cmdstat_hget`, `cmdstat_hgetall`, `cmdstat_hmget`, `cmdstat_mget`, `cmdstat_getbit` и `cmdstat_getrange` Она эквивалентна сумме попаданий и промахов за указанный интервал отчетности. |
| Количество операций в секунду | Общее количество команд, обработанных сервером кэша в секунду за указанный интервал создания отчетов.  Это значение соответствует instantaneous_ops_per_sec из команды Redis INFO. |
| Загрузка сервера Redis |Процентная доля циклов, в течение которых сервер Redis занят обработкой и не находится в неактивном состоянии, ожидая поступления сообщений. Если эта метрика достигает значения 100, это говорит о том, что сервер Redis достиг максимальной производительности и ЦП не может работать быстрее. При высоком показателе загрузки сервера Redis в клиенте возникают исключения из-за истечения времени ожидания. В этом случае имеет смысл увеличить масштаб или сегментировать данные на несколько экземпляров кэша. |
| Операций записи |Количество операций записи в кэш за указанный интервал отчетности. Эта метрика представляет собой сумму следующих значений INFO-команды all Redis: `cmdstat_set`, `cmdstat_hset`, `cmdstat_hmset`, `cmdstat_hsetnx`, `cmdstat_lset`, `cmdstat_mset`, `cmdstat_msetnx`, `cmdstat_setbit`, `cmdstat_setex`, `cmdstat_setrange` и `cmdstat_setnx`. |
| Всего ключей  | Максимальное количество ключей в кэше за последний отчетный период. Это значение соответствует значению `keyspace` команды INFO Redis. Из-за ограничений базовой системы метрик для кэшей с включенной кластеризацией параметр "Всего ключей" возвращает максимальное количество ключей одного сегмента, в котором за отчетный период зарегистрировано максимальное количество ключей.  |
| Всего операций |Общее количество команд, обработанных сервером кэша за указанный интервал отчетности. Это значение соответствует значению `total_commands_processed` команды INFO Redis. Обратите внимание, что при использовании кэша Azure для Redis исключительно в режиме pub/sub метрики `Cache Hits`, `Cache Misses`, `Gets` и `Sets` не отслеживаются. Вместо них фиксируется метрика `Total Operations`, которая отражает использование кэша операциями pub/sub. |
| Используемая память |Объем памяти кэша, используемой для хранения пар «ключ — значение» (в МБ), за указанный интервал отчетности. Это значение соответствует значению `used_memory` команды INFO Redis. Не содержит метаданных или фрагментации. |
| Процент используемой памяти | Доля от общего объема памяти, которая используется в течение указанного интервала создания отчетов.  Это значение ссылается на значение used_memory из команды Redis INFO для вычисления процента. |
| RSS используемой памяти |Объем используемой памяти кэша (в МБ) за указанный интервал отчетности с учетом фрагментации и метаданных. Это значение соответствует значению `used_memory_rss` команды INFO Redis. |

<a name="operations-and-alerts"></a>
## <a name="alerts"></a>Оповещения
Вы можете настроить получение уведомлений на основе метрик и журналов действий. Azure Monitor позволяет настроить действие, выполняемое при активации оповещения:

* Отправка уведомления по электронной почте.
* Вызов webhook.
* Вызов приложения логики Azure.

Чтобы настроить правила генерации оповещений для кэша, щелкните **Правила оповещения** в меню **Ресурсы**.

![Мониторинг](./media/cache-how-to-monitor/redis-cache-monitoring.png)

Дополнительные сведения о настройке и использовании оповещений см. в статье [Создание оповещений метрик в Azure Monitor для служб Azure с помощью портала Azure](../monitoring-and-diagnostics/insights-alerts-portal.md).

## <a name="activity-logs"></a>Журналы действий
Журналы действий позволяют подробно проанализировать операции, выполненные с экземплярами кэша Azure для Redis. Ранее они назывались журналами аудита или журналами операций. С помощью журналов действий можно получить ответы на вопросы "что? кто? когда?" в отношении любой операции записи (PUT, POST, DELETE), выполненной с экземплярами кэша Azure для Redis. 

> [!NOTE]
> Журналы действий не содержат сведений об операциях чтения (GET).
>
>

Чтобы просмотреть журналы действий для кэша, щелкните **Журналы действий** в меню **Ресурсы**.

Дополнительные сведения о журналах действий см. в статье [Общие сведения о журнале действий Azure](../azure-monitor/platform/activity-logs-overview.md).











