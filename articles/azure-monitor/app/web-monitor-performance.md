---
title: Отслеживание работоспособности и использования приложения с помощью Application Insights
description: Приступая к работе с Application Insights. Анализ использования, доступности и производительности локальных приложений или веб-приложений Microsoft Azure.
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.assetid: 40650472-e860-4c1b-a589-9956245df307
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 05/10/2018
ms.reviewer: sdash
ms.author: mbullwin
ms.openlocfilehash: d7b8037f50fc4877fe233925f3e922648169f73b
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "60373152"
---
# <a name="monitor-performance-in-web-applications"></a>Отслеживание производительности в веб-приложениях


Она позволяет удостовериться, что приложение работает с хорошей производительностью, и быстро выяснить, были ли сбои. [Application Insights][start] сообщает о любых проблемах с производительностью и исключениях, помогая выявлять и диагностировать их причины.

Application Insights можно отслеживать веб-приложения и службы Java и ASP.NET, а также службы WCF. Они могут размещаться локально, на виртуальных машинах, а также как веб-сайты Microsoft Azure. 

На стороне клиента Application Insights может извлекать телеметрию с веб-страниц и разнообразных устройств, включая приложения для iOS, Android и Магазина Windows.

## <a name="setup"></a>Настройка мониторинга производительности
Если вы еще не добавили Application Insights в проект (т. е. если в нем нет файла ApplicationInsights.config), вы можете приступить к работе с этой службой одним из следующих способов:

* [Веб-приложения ASP.NET](../../azure-monitor/app/asp-net.md)
  * [Добавление мониторинга исключений](../../azure-monitor/app/asp-net-exceptions.md)
  * [Добавление мониторинга зависимостей](../../azure-monitor/app/monitor-performance-live-website-now.md)
* [Веб-приложений Java EE](../../azure-monitor/app/java-get-started.md)
  * [Добавление мониторинга зависимостей](../../azure-monitor/app/java-agent.md)

## <a name="view"></a>Просмотр метрик производительности
На [портале Azure](https://portal.azure.com)перейдите к ресурсу Application Insights, настроенному для приложения. В колонке "Обзор" отображаются основные данные производительности:

Щелкните любую диаграмму, чтобы увидеть результаты более подробно и за больший период. Например, щелкните плитку "Запросы" и выберите временной диапазон.

![Щелкните плитки, чтобы увидеть подробные данные, и выберите временной диапазон](./media/web-monitor-performance/appinsights-48metrics.png)

Щелкните диаграмму, чтобы выбрать, какие метрики отображать, а также добавить новую диаграмму и выбрать метрики для нее.

![Щелкните график, чтобы выбрать метрики](./media/web-monitor-performance/appinsights-61perfchoices.png)

> [!NOTE]
> **Отмените выбор всех метрик** , чтобы увидеть полный набор доступных метрик. Метрики разделены на группы: если выбран один из элементов группы, отображаются только метрики этой группы.

## <a name="metrics"></a>Что все это означает? Плитки и отчеты о производительности
Имеются различные метрики производительности, которые можно получить. Начнем с тех, которые отображаются в колонке приложения по умолчанию.

### <a name="requests"></a>Requests (Запросы)
Это число HTTP-запросов, полученных за указанный период. Сравните это значение с результатами в других отчетах, чтобы узнать, как ведет себя приложение при изменении нагрузки.

HTTP-запросы включают в себя все запросы GET и POST для страниц, данных и изображений.

Чтобы просмотреть счетчики для конкретных URL-адресов, щелкните плитку.

### <a name="average-response-time"></a>Average response time (Среднее время ответа)
Это время от поступления веб-запроса в приложение до возвращения ответа.

Точки показывают скользящее среднее. Если запросов много, для некоторых из них значения могут отклоняться от среднего, но при этом на графике не будет явных пиков или провалов.

Обращайте внимание на необычные пики. Как правило, время ответа растет с увеличением числа запросов. Если этот рост непропорционален, возможно, приложение исчерпало какой-либо ресурс, например ЦП или емкость используемой службы.

Чтобы просмотреть значения для конкретных URL-адресов, щелкните плитку.

![](./media/web-monitor-performance/appinsights-42reqs.png)

### <a name="slowest-requests"></a>Slowest requests (Самые медленные запросы)
![](./media/web-monitor-performance/appinsights-44slowest.png)

Показывает, какие запросы могут потребовать оптимизации производительности.

### <a name="failed-requests"></a>Failed requests (Неудачные запросы)
![](./media/web-monitor-performance/appinsights-46failed.png)

Это счетчик запросов, которые вызвали не перехваченные исключения.

Чтобы просмотреть подробные сведения о конкретных сбоях, щелкните плитку. Чтобы просмотреть подробные сведения об отдельном запросе, выберите его. 

Для детального изучения доступна только репрезентативная выборка сбоев.

### <a name="other-metrics"></a>Другие метрики
Чтобы увидеть, какие еще метрики доступны, щелкните график и снимите все флажки метрик. Чтобы просмотреть определение конкретной метрики, щелкните значок (i).

![Снимите все флажки, чтобы увидеть полный набор метрик](./media/web-monitor-performance/appinsights-62allchoices.png)

Выбор любой метрики отключит остальные, и они не будут отображаться на этой диаграмме.

## <a name="set-alerts"></a>Настройка оповещений
Чтобы получать уведомления по электронной почте о нетипичных значениях любой метрики, добавьте предупреждение. Можно выбрать отправку либо по электронной почте администраторам учетной записи, либо на указанный электронный адрес.

![](./media/web-monitor-performance/appinsights-413setMetricAlert.png)

Выбирайте ресурс прежде других свойств. Не выбирайте ресурсы webtest, если нужно задать предупреждения метрик производительности или использования.

Аккуратно указывайте единицы измерения для пороговых значений.

*Отсутствует кнопка «Добавить оповещение».* Вы используете учетную запись группы с доступом только для чтения? Обратитесь к администратору учетной записи.

## <a name="diagnosis"></a>Диагностические проблемы
Вот некоторые советы по выявлению и диагностике проблем с производительностью:

* настройте [веб-тесты][availability], чтобы получать оповещения, когда веб-сайт выходит из строя либо отвечает неправильно или медленно; 
* сравните число запросов с другими метриками, чтобы узнать, не связаны ли сбои или медленный ответ с нагрузкой;
* [вставьте в код инструкции трассировки и выполняйте поиск по ним][diagnostic], чтобы упростить выявление проблем.
* Отслеживайте работу веб-приложения с помощью [Live Metrics Stream][livestream].
* Записать состояние приложения .NET с помощью [Snapshot Debugger][snapshot].

## <a name="find-and-fix-performance-bottlenecks-with-performance-investigation-experience"></a>Поиск и устранение узких мест производительности с помощью исследования производительности

Вы можете использовать возможности функции исследования производительности для обнаружения операций с низкой производительностью в веб-приложении. Вы можете быстро выбрать определенную операцию с низкой производительностью и использовать [Профилировщик](../../azure-monitor/app/profiler.md), чтобы определить основную причину низкой производительности операций в коде. Используя новое распространение длительности, отображаемое для выбранной операции, вы можете практически за секунды определить ее качество производительности для клиентов. Кроме того, для каждой операции с низкой производительностью вы можете увидеть количество затронутых взаимодействий с пользователем. В примере ниже мы решили более подробно ознакомиться с операцией получения сведений о клиенте. В распространении длительности видно три пика. Пик слева составляет 400 мс и представляет достаточно высокую скорость. Средний пик составляет 1,2 с и представляет среднестатистическую скорость. И наконец, еще один небольшой пик 3,6 с, который представляет 99-й процентиль, что совсем разочарует клиентов. Эта скорость в 10 раз медленнее, чем первая скорость выше для той же операции. 

![Три пика длительности операции получения сведений о клиенте](./media/web-monitor-performance/PerformanceTriageViewZoomedDistribution.png)

Чтобы получить более точное представление о взаимодействиях с пользователем для этой операции, можно выбрать более широкий диапазон времени. Затем можно также ограничить время по определенному временному окну, где скорость выполнения операции была низкой. В примере ниже мы переключили диапазон времени с 24 часов по умолчанию на 7 дней и выбрали временное окно с 9:47 до 12:47 между вторником (12) и средой (13). Распространение длительности и количество трассировок образца и профилировщика обновлены в правой области.

![Три пика длительности операции с диапазоном времени 7 дней и временным окном](./media/web-monitor-performance/PerformanceTriageView7DaysZoomedTrend.png)

Чтобы рассмотреть медленные операции, выберем длительности между процентилями 95–99. Они представляют 4 % от взаимодействий с пользователем, скорость которых была ограничена.

![Три пика длительности операции с диапазоном времени 7 дней и временным окном](./media/web-monitor-performance/PerformanceTriageView7DaysZoomedTrendZoomed95th99th.png)

Теперь мы можем ознакомиться с типичными примерами, нажав кнопку "Образцы", или с типичными трассировками профилировщика, нажав кнопку трассировок профилировщика. В рамках этого примера имеется 4 трассировки, собранные для операции получения сведений о клиенте с определенным временным окном и диапазоном времени.

Иногда проблема заключается не в коде, а в зависимости, которую он вызывает. Вы можете переключиться на вкладку "Зависимости" в представлении рассмотрения производительности, чтобы более подробно ознакомиться с зависимостями с низкой производительностью. По умолчанию представление производительности стремится к средним значениям, но большую важность имеет 95-й процентиль (или 99, если вы отслеживаете сформировавшуюся службу). В примере ниже рассматривается медленная зависимость большого двоичного объекта Azure, где мы вызываем PUT fabrikamaccount. Хорошая скорость кластера составляет около 40 мс, в то время как скорость медленных вызовов одной зависимости в три раза меньше — 120 мс. Потребуется немного этих вызовов, чтобы значительно снизить скорость соответствующих операций. Вы можете более подробно ознакомиться с типичными примерами и трассировками профилировщика, а также вкладкой "Операции".

![Три пика длительности операции с диапазоном времени 7 дней и временным окном](./media/web-monitor-performance/SlowDependencies95thTrend.png)

Интерфейс для исследования производительности показывает актуальные аналитические сведения вместе с примером, на котором вы решили сосредоточиться. Чтобы просмотреть все доступные аналитики, рекомендуется переключиться на диапазон времени 30 дней, а затем выбрать параметр "Всего" для просмотра аналитики всех операций за прошлый месяц.

![Три пика длительности операции с диапазоном времени 7 дней и временным окном](./media/web-monitor-performance/Performance30DayOveralllnsights.png)


## <a name="next"></a>Дальнейшие действия
[Веб-тесты][availability] — настройте отправку веб-запросов к приложению через равные интервалы из разных точек мира.

[Сбор данных трассировки диагностики и поиск по ним][diagnostic] — добавьте вызовы трассировки и анализируйте результаты для выявления проблем.

[Отслеживание использования][usage] — получайте информацию о том, как люди используют ваше приложение.

[Устранение неполадок][qna] — вопросы и ответы



<!--Link references-->

[availability]: ../../azure-monitor/app/monitor-web-app-availability.md
[diagnostic]: ../../azure-monitor/app/diagnostic-search.md
[greenbrown]: ../../azure-monitor/app/asp-net.md
[qna]: ../../azure-monitor/app/troubleshoot-faq.md
[redfield]: ../../azure-monitor/app/monitor-performance-live-website-now.md
[start]: ../../azure-monitor/app/app-insights-overview.md
[usage]: usage-overview.md
[livestream]: ../../azure-monitor/app/live-stream.md
[snapshot]: ../../azure-monitor/app/snapshot-debugger.md



