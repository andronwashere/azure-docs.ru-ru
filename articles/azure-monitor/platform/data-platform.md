---
title: Платформы данных Azure Monitor | Документация Майкрософт
description: Данные мониторинга, которые собирает Azure Monitor, делятся на метрики и журналы. Метрики занимают малый объем, их можно использовать в сценариях, предусматривающих работу в режиме практически реального времени. Журналы используются для расширенного анализа.
documentationcenter: ''
author: bwren
manager: carmonm
editor: tysonn
ms.service: azure-monitor
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/26/2019
ms.author: bwren
ms.openlocfilehash: 00c0fea9d8ca7ee299a9a19473917eba90edd675
ms.sourcegitcommit: f10ae7078e477531af5b61a7fe64ab0e389830e8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/05/2019
ms.locfileid: "67606971"
---
# <a name="azure-monitor-data-platform"></a>Платформы данных Azure Monitor

Включение наблюдаемость в современных сложных вычислительных средах под управлением распределенных приложений, которые используют облачных и локальных служб, требует сбора оперативных данных из каждого слоя и все компоненты распределенных систем. Необходимо иметь возможность выполнить детальную аналитику на основе этих данных и ее объединение в одну область прозрачного стекла с различных точек зрения для поддержки множества заинтересованных лиц в организации.

[Azure Monitor](../overview.md) собирает и объединяет данные из различных источников в общую платформу данных, где его можно использовать для анализа, визуализации и оповещений. Он обеспечивает согласованную работу на основе данных из нескольких источников, который предоставляет ценную информацию из всех отслеживаемых ресурсов и даже с данными из других служб, которые хранят свои данные в Azure Monitor.


![Общие сведения о службе Azure Monitor](media/data-platform/overview.png)

## <a name="observability-data-in-azure-monitor"></a>Наблюдаемость данных в Azure Monitor
Метрики, журналы и распределенной трассировки известен как три фундаментальных принципа наблюдаемость. Это различные виды данных, средства мониторинга необходимо собирать и анализировать для обеспечения достаточно наблюдаемость из отслеживаемой системе. Наблюдаемость достигается путем нахождения связей между данными из нескольких основных принципов и статистическая обработка данных ко всему набору отслеживаемых ресурсов. Так как Azure Monitor хранятся данные из нескольких источников вместе, данные могут быть взаимосвязаны и анализируются с помощью общего набора средств. Он также сопоставляет данные в нескольких подписках Azure и клиентов в дополнение к размещению данных для других служб.

Ресурсы Azure создают значительный объем данных мониторинга. Azure Monitor объединяет эти данные, а также данные мониторинга из других источников в один метрики или журналы платформы. Каждый оптимизированный для определенных сценариев мониторинга, и каждый поддерживает различные функции в Azure Monitor. Функции, такие как анализ данных, визуализации и оповещений необходимо понять различия, таким образом, чтобы наиболее эффективно и экономически эффективным способом можно реализовать нужного сценария. Анализ данных в Azure Monitor, такие как [Application Insights](../app/app-insights-overview.md) или [Azure Monitor для виртуальных машин](../insights/vminsights-overview.md) иметь средства анализа, которые позволяют вам сконцентрироваться на определенных сценариев мониторинга, не обязательно знать различия между двумя типами данных. 


### <a name="metrics"></a>metrics
[Метрики](data-platform-metrics.md) — это числовые значения, описывающие конкретный аспект системы в определенный момент времени. Они собираются с регулярными интервалами и идентифицируются при помощи метку времени, имя, значение и одной или нескольких меток определение. Метрики можно вычислять с помощью различных алгоритмов, по сравнению с другими метриками и анализировать для тенденции с течением времени. 

Метрики в Azure Monitor хранятся в базе данных временных рядов, оптимизированная для анализа данных с меткой времени. Это делает метрики, особенно подходит для отображения предупреждений и быстрое обнаружение проблем. Они могут сказать о производительности системы, но обычно необходимо объединить с журналы, чтобы определить причину проблемы.

Метрики можно использовать для интерактивного анализа на портале Azure с помощью [обозревателя метрик](../app/metrics-explorer.md). Они могут добавляться к [мониторинга Azure](../learn/tutorial-app-dashboards.md) для визуализации в сочетании с другими данными, а для практически в реальном времени [предупреждения](alerts-metric.md).

Дополнительные сведения о метрик Azure Monitor, включая их источники данных в [метрики в Azure Monitor](data-platform-metrics.md).

### <a name="logs"></a>Журналы
[Журналы](data-platform-logs.md) являются событиями, произошедшими в системе. Они могут содержать различные виды данных и могут быть структурированы или произвольного текста формы с меткой времени. Они могут быть созданы нерегулярно события в среде создают записи журнала, а система в условиях большой нагрузки обычно создает дополнительные тома журнала.

Журналы в Azure Monitor хранятся в рабочей области Log Analytics, основанный на [обозреватель данных Azure](/azure/data-explorer/) которого предоставляет механизм мощные функции анализа и [полнофункциональный язык запросов](/azure/kusto/query/). Журналы обычно предоставляет достаточно сведений, чтобы предоставить полный контекст обнаруженной проблемы и можно использовать для идентификации основная причина проблем.

> [!NOTE]
> Важно различать журналов Azure Monitor и источников данных журнала в Azure. Например, события уровня подписки Azure записываются в [журнал действий](activity-logs-overview.md) , которые можно просмотреть в меню Azure Monitor. Большинство ресурсов будет записывать сведения о функционировании для [журнала диагностики](diagnostic-logs-overview.md) , вы можете направлять в разных расположениях. Журналы Azure Monitor — это платформа данных журнала, которая собирает журналы действий и журналов диагностики, а также другие данные наблюдения, чтобы обеспечить глубокий анализ всего набора ресурсов.


 Вы можете работать с [журнал запросов](../log-query/log-query-overview.md) интерактивном режиме с помощью [Log Analytics](../log-query/portals.md) на портале Azure или добавить результаты [мониторинга Azure](../learn/tutorial-app-dashboards.md) для визуализации в сочетании с другие данные. Вы также можете создать [оповещения журнала](alerts-log.md) которого будет запускать оповещение, на основе результатов запроса расписание.

Дополнительные сведения о журналов Azure Monitor, включая их источники данных в [журналы в Azure Monitor](data-platform-logs.md).

### <a name="distributed-traces"></a>Распределенной трассировки
Трассировки — ряд связанных событий, выполните запрос пользователя в распределенной системе. Они позволяют определить поведение кода приложения и производительность различных транзакций. Хотя по отдельные компоненты распределенной системы часто создаются журналы, трассировки измеряет ко всему набору компонентов работу и производительность приложения.

Распределенная трассировка в Azure Monitor включен с [пакет SDK Application Insights](../app/distributed-tracing.md), и с другими данными журнала приложения, собранные Application Insights хранятся данные трассировки. Это делает его доступным для тех же средств анализа, как другие данные журнала, включая журнал запросов, панелей мониторинга и оповещений.

Дополнительные сведения о распределенная трассировка в [новые возможности распределенной трассировки?](../app/distributed-tracing.md).


## <a name="compare-azure-monitor-metrics-and-logs"></a>Сравнение метрик в Microsoft Azure и журналы

В следующей таблице сравниваются метрик и журналов в Azure Monitor.

| Атрибут  | metrics | Журналы |
|:---|:---|:---|
| Преимущества | Упрощенный и обрабатывать сценарии близком к реальному времени, например, предупреждение. Идеально подходит для быстрого обнаружения проблем. | Анализ с расширенной язык запросов. Идеально подходит для подробного анализа и выявления основной причины. |
| Data | Только числовые значения | Текстовых или числовых данных |
| structure | Стандартный набор свойств, в том числе время выборки, отслеживаемых ресурсов, числовое значение. Некоторые метрики включают несколько измерений для дальнейшего определения. | Уникальный набор свойств, в зависимости от типа журнала. |
| Collection | Собранные с регулярными интервалами. | Возможен сбор нерегулярно как записи, чтобы создать триггер события. |
| Просмотреть на портале Azure | Обозреватель метрик | Log Analytics |
| Источники данных включают | Сбор показателей платформы из ресурсов Azure.<br>Приложения, отслеживаются Application Insights.<br>Пользовательские приложения или API. | Приложения и журналы диагностики.<br>Решения для мониторинга.<br>Агенты и расширения виртуальной Машины.<br>Исключения и запросы приложения.<br>Центр безопасности Azure.<br>API сборщика данных. |

## <a name="collect-monitoring-data"></a>Сбор данных мониторинга
Различные [источников данных для Azure Monitor](data-sources.md) запишет рабочую область Log Analytics (журналы) и базе данных метрик Azure Monitor (показатели) либо оба. Некоторые источники будет записывать непосредственно в эти хранилища данных, хотя другие может записать в другое расположение, например служба хранилища Azure и требуют определенной настройки для заполнения метрики или журналы. 

См. в разделе [метрики в Azure Monitor](data-platform-metrics.md) и [журналы в Azure Monitor](data-platform-logs.md) пример для различных источников данных, которые заполняют каждого типа.


## <a name="stream-data-to-external-systems"></a>Потоковая передача данных во внешние системы
Помимо анализа данных мониторинга с помощью средств Azure вам может потребоваться перенаправлять их во внешнее средство, например продукт для управления информационной безопасностью и событиями безопасности (SIEM). Такое перенаправление обычно выполняют непосредственно из отслеживаемых ресурсов через [Центры событий Azure](/azure/event-hubs/). Некоторые источники можно настроить для отправки данных непосредственно в концентратор событий, хотя вы можете использовать другой процесс, например, приложения логики для извлечения необходимых данных. См. в разделе [Stream данных мониторинга Azure в концентратор событий для потребления внешним инструментом](stream-monitoring-data-event-hubs.md) подробные сведения.



## <a name="next-steps"></a>Следующие шаги

- Дополнительные сведения о [метрики в Azure Monitor](data-platform-metrics.md).
- Дополнительные сведения о [журналы в Azure Monitor](data-platform-logs.md).
- Узнайте о [доступных данных мониторинга](data-sources.md) для различных источников в Azure.
