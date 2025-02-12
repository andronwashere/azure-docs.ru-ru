---
title: Интеллектуальное обнаружение. Аномалии производительности | Документация Майкрософт
description: Служба Application Insights выполняет интеллектуальный анализ телеметрии вашего приложения и предупреждает о потенциальных проблемах производительности. Эта функция не требует настройки.
services: application-insights
documentationcenter: windows
author: mrbullwinkle
manager: carmonm
ms.assetid: 6acd41b9-fbf0-45b8-b83b-117e19062dd2
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 05/04/2017
ms.reviewer: antonfr
ms.author: mbullwin
ms.openlocfilehash: b1a3b04427839736359c88f8ad6a8db5eedf8488
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "61294089"
---
# <a name="smart-detection---performance-anomalies"></a>Интеллектуальное обнаружение. Аномалии производительности

[Application Insights](../../azure-monitor/app/app-insights-overview.md) автоматически анализирует производительность веб-приложения и предупреждает о потенциальных проблемах. Возможно, вы читаете это потому, что получили одно из уведомлений интеллектуального обнаружения.

Эта функция не требует специальной настройки, нужно лишь настроить приложение для использования Application Insights (с помощью [ASP.NET](../../azure-monitor/app/asp-net.md), [Java](../../azure-monitor/app/java-get-started.md) или [Node.js](../../azure-monitor/app/nodejs.md) и в [коде веб-страницы](../../azure-monitor/app/javascript.md)). Она работает, когда приложение создает достаточно данных телеметрии.

## <a name="when-would-i-get-a-smart-detection-notification"></a>Когда я буду получать уведомления интеллектуального обнаружения?

Application Insights обнаруживает снижение производительности приложения по одному из следующих признаков:

* **Увеличение времени отклика**. Приложение начинает медленнее отвечать на запросы, чем должно. Изменение может произойти быстро, например из-за понижения производительности в последней развернутой службе. Или оно может происходить постепенно, например из-за утечки памяти. 
* **Увеличение времени отклика зависимостей**. Приложение выполняет вызовы к REST API, базе данных или другим зависимостям. Зависимость отвечает медленнее, чем должна.
* **Шаблон снижения производительности**. В работе приложения, возможно, возникла проблема производительности, влияющая только на некоторые запросы. Например, страницы приложения загружаются намного медленнее только в конкретном браузере или медленнее обслуживаются запросы только с конкретного сервера. В настоящее время наши алгоритмы анализируют время загрузки страницы, время отклика на запрос и время выполнения запросов к зависимостям.  

Чтобы определить базовые показатели, функции интеллектуального обнаружения требуются данные телеметрии по крайней мере за 8 дней в подходящем объеме. Таким образом, после того, как приложение будет выполняться в течение этого периода, вы начнете получать уведомления о всех значительных проблемах.


## <a name="does-my-app-definitely-have-a-problem"></a>В работе моего приложения обязательно существует проблема?

Уведомление не означает, что в работе приложения возникла проблема. Нет, это просто информация о том, что вам нужно обратить внимание на ту или иную ситуацию.

## <a name="how-do-i-fix-it"></a>Как ее исправить?

Уведомления содержат диагностические сведения. Ниже приведен пример:


![Ниже приведен пример обнаружения увеличения времени отклика сервера.](media/proactive-performance-diagnostics/server_response_time_degradation.png)

1. **Рассмотрение**. Уведомление показывает, скольких пользователей или сколько операций затрагивает проблема. Это поможет назначить приоритет проблемы.
2. **Область**. Проблема влияет на весь трафик или только на некоторые страницы? Она связана с определенными браузерами или расположениями? Эти сведения можно получить из уведомления.
3. **Диагностика**. Нередко диагностические сведения в уведомлении содержат предполагаемый характер проблемы. Например, если время отклика увеличивается при высокой частоте запросов, предлагается, что сервер или зависимости перегружены. 

    В противном случае откройте колонку "Производительность" в Application Insights. Здесь вы найдете данные [профилировщика](profiler.md). Если порождаются исключения, вы можете также воспользоваться [отладчиком моментальных снимков](../../azure-monitor/app/snapshot-debugger.md).



## <a name="configure-email-notifications"></a>Настройка уведомлений по электронной почте

Уведомления интеллектуального обнаружения включены по умолчанию и отправляются пользователям, обладающим [доступом с правами владельца, участника или читателя к ресурсу Application Insights](../../azure-monitor/app/resources-roles-access-control.md). Чтобы изменить этот режим, либо щелкните **Настройка** в уведомлении по электронной почте, либо откройте параметры интеллектуального обнаружения в Application Insights. 
  
  ![Параметры интеллектуального обнаружения](media/proactive-performance-diagnostics/smart_detection_configuration.png)
  
  * Вы можете использовать ссылку **Отписаться** в сообщении интеллектуального обнаружения, чтобы прекратить получение уведомлений по электронной почте.

Сообщения интеллектуального обнаружения об аномалиях производительности отправляются один раз в день для каждого ресурса Application Insights. Сообщение будет отправлено только в том случае, если в этот день будет обнаружена хотя бы одна новая проблема. Сообщения не отправляются повторно. 

## <a name="faq"></a>Часто задаваемые вопросы

* *Это означает, что сотрудники Майкрософт отслеживают мои данные?*
  * № Служба работает полностью в автоматическом режиме. Уведомления получаете только вы. Ваши данные остаются [конфиденциальными](../../azure-monitor/app/data-retention-privacy.md).
* *Анализируются ли все данные, собираемые Application Insights?*
  * В настоящее время нет. В настоящее время мы анализируем время ответа на запрос, время отклика на зависимости и время загрузки страницы. Анализ дополнительных метрик добавлен в нашу невыполненную работу, которую нам предстоит сделать.

* С какими типами приложений это работает?
  * Это снижение производительности обнаруживается в любом приложении, которое создает соответствующие данные телеметрии. Если вы установили Application Insights в веб-приложении, то запросы и зависимости отслеживаются автоматически. Но если во внутренние службы или другие приложения вы вставили вызовы к [TrackRequest()](../../azure-monitor/app/api-custom-events-metrics.md#trackrequest) или [TrackDependency](../../azure-monitor/app/api-custom-events-metrics.md#trackdependency), то интеллектуальное обнаружение будет работать точно так же.

* *Можно ли создавать собственные правила обнаружения аномалий или настраивать имеющиеся правила?*

  * Пока еще нет, но вы можете:
    * [настроить оповещения](../../azure-monitor/app/alerts.md), которые предупреждают вас, когда метрика достигает порогового значения;
    * [экспортировать данные телеметрии](../../azure-monitor/app/export-telemetry.md) в [базу данных](../../azure-monitor/app/code-sample-export-sql-stream-analytics.md) или в [Power BI](../../azure-monitor/app/export-power-bi.md ), с помощью которых можно проанализировать информацию самостоятельно.
* *Как часто выполняется анализ?*

  * Мы проводим анализ ежедневно, обрабатывая данные телеметрии за предыдущий день (полный день в часовом поясе UTC).
* *То есть эта функция заменяет [оповещения о метриках](../../azure-monitor/app/alerts.md)?*
  * №  В наши задачи не входит обнаружение каждого события, которое вы можете счесть нестандартным или аварийным.


* *Если, получив уведомление, я ничего не сделаю, то получу ли я напоминание?*
  * Нет, вы получаете сообщение о каждой проблеме только один раз. Если проблема повторяется, сведения о ней обновляются в колонке веб-канала интеллектуального обнаружения.
* *Не могу найти письмо. Где найти уведомления на портале?*
  * В обзоре приложения службы Application Insights щелкните плитку **Smart Detection** (Интеллектуальное обнаружение). Там вы сможете найти все уведомления за последние 90 дней.

## <a name="how-can-i-improve-performance"></a>Как можно повысить производительность?
Медленные и неудачные отклики доставляют большие неудобства пользователям веб-сайтов, как вы знаете из собственного опыта. Поэтому очень важно устранить проблемы.

### <a name="triage"></a>Рассмотрение
Во-первых, имеет ли это значение? Если страница всегда загружается медленно, но ее просматривает всего 1 % пользователей веб-сайта, наверное, вам лучше заняться более важными делами. С другой стороны, если страницу открывает только 1 % пользователей, но она каждый раз возвращает исключения, возможно, есть смысл в том, чтобы изучить проблему.

В общем, ориентируйтесь на отчет о влиянии (затронутые пользователи или процент трафика). Но имейте в виду, что полностью полагаться на него нельзя. Соберите дополнительные данные для подтверждения.

Рассмотрите все аспекты проблемы. Причиной может быть географическое расположение. Выполните [тесты доступности](../../azure-monitor/app/monitor-web-app-availability.md), включив этот регион. Возможно, в этой области просто проблемы с сетью.

### <a name="diagnose-slow-page-loads"></a>Диагностика медленных загрузок страниц
В чем суть проблемы? Сервер медленно отвечает на запросы, страница очень большая или браузеру требуется проделать большой объем работы, чтобы отобразить ее?

Откройте колонку метрик браузеров. Сегментированное отображение показателей времени загрузки страницы в браузере показывает, на что уходит время. 

* Высокий показатель **времени отправки запроса** означает, что сервер отвечает медленно или запрос является публикацией с большим объемом данных. Чтобы узнать время отклика, просмотрите [метрики производительности](../../azure-monitor/app/web-monitor-performance.md#metrics) .
* Чтобы узнать, что замедляет работу — внешние службы или базы данных, — настройте [отслеживание зависимостей](../../azure-monitor/app/asp-net-dependencies.md) .
* Если преобладает метрика **Получение ответа**, это значит, что страница и ее зависимые компоненты (JavaScript, CSS, изображения и т. п. — но не асинхронно загруженные данные) имеют большой размер. Выполните [тест доступности](../../azure-monitor/app/monitor-web-app-availability.md) и обязательно настройте загрузку зависимых компонентов. После получения результатов откройте сведения и разверните их, чтобы просмотреть показатели времени загрузки различных файлов.
* Высокое значение счетчика **Время обработки клиента** может указывать на медленное выполнение сценариев. Если причина не очевидна, рекомендуем добавить код времени и отправить значения времени в вызовах trackMetric.

### <a name="improve-slow-pages"></a>Улучшение производительности медленных страниц
В Интернете достаточно советов о том, как ускорить ответы серверов и сократить время загрузки страниц, поэтому мы не будем повторять их здесь. Ниже приведено несколько советов, которые, вероятно, вам уже известны. Может, они натолкнут вас на нужные мысли.

* Медленная загрузка из-за большого объема файлов. Загружайте скрипты и другие компоненты по отдельности. Используйте объединение скриптов. Разбейте главную страницу на мини-приложения, которые загружают свои данные по отдельности. Не используйте обычный HTML для отправки длинных таблиц — используйте скрипт для запроса данных в формате JSON (или другом компактном формате) и уже затем заполняйте таблицу. Для всего этого есть замечательные платформы. (Они, конечно же, также содержат большие скрипты).
* Медленные серверные зависимости. Обратите внимание на географическое расположение компонентов. Например, при использовании Azure веб-сервер и база данных должны быть расположены в одном регионе. Проверьте, не получают ли запросы больше данных, чем требуется. Возможно, поможет кэширование или пакетная обработка.
* Проблемы мощности. Просмотрите метрики времени отклика сервера и количество запросов. Если пики времени ответа непропорциональны пикам количества запросов, вполне вероятно, что ресурсы серверов исчерпаны.


## <a name="server-response-time-degradation"></a>Увеличение времени отклика сервера

Это уведомление об увеличении времени отклика сервера сообщает следующее:

* Время отклика по сравнению с обычным временем отклика для этой операции.
* Сколько пользователей затронуто.
* Среднее время отклика и время отклика для 90-го процентиля для этой операции в день обнаружения и за 7 предыдущих дней. 
* Число запросов этой операции в день обнаружения и за 7 предыдущих дней.
* Корреляция между снижением производительности данной операции и снижением производительности связанных зависимостей. 
* Ссылки, которые помогут при диагностике проблемы:
  * Трассировки профилировщика, позволяющие узнать, на что потрачено время операции (ссылка будет доступна, если для этой операции в период обнаружения были собраны примеры трассировки профилировщика). 
  * Отчеты о производительности в обозревателе метрик, позволяющие анализировать эту операцию на различных диапазонах времени с помощью разнообразных фильтров.
  * Результаты поиска этих вызовов для просмотра конкретных свойств вызовов.
  * Отчеты о сбоях. Если их больше 1, это означает, что в данной операции обнаружены сбои, которые могли способствовать снижению производительности.

## <a name="dependency-duration-degradation"></a>Увеличение времени отклика зависимостей

В современных приложениях все больше применяются принципы разработки микрослужб, что во многих случаях приводит к сильной зависимости от внешних служб. Например, если приложение зависит от какой-либо платформы для управления данными или вы разрабатываете собственную службу программ-роботов, вероятно, вы воспользуетесь услугами поставщика служб когнитивных вычислений, чтобы ваши программы-роботы вели себя человекоподобно, а также воспользуетесь службой банка данных, из которой программы-роботы будут извлекать ответы.  

Пример уведомления об увеличении времени отклика зависимостей.

![Ниже приведен пример обнаружения увеличения времени отклика зависимостей.](media/proactive-performance-diagnostics/dependency_duration_degradation.png)

Обратите внимание на то, что оно содержит:

* Время отклика по сравнению с обычным временем отклика для этой операции.
* Сколько пользователей затронуто.
* Среднее время отклика и время отклика для 90-го процентиля этой зависимости в день обнаружения и за 7 предыдущих дней.
* Количество вызовов к зависимости в день обнаружения и за 7 предыдущих дней.
* Ссылки, которые помогут при диагностике проблемы:
  * Отчеты о производительности в обозревателе метрик для этой зависимости.
  * Результаты поиска этих вызовов к зависимости для просмотра свойств вызовов.
  * Отчеты о сбоях. Если их больше 1, это означает, что в период обнаружения произошли сбои вызовов зависимостей, которые могли способствовать снижению производительности. 
  * Служба анализа с запросами, которые вычисляют длительность и число вызовов этой зависимости.  

## <a name="smart-detection-of-slow-performing-patterns"></a>Интеллектуальное обнаружение шаблонов снижения производительности 

Application Insights определяет проблемы производительности, которые влияют только на часть пользователей или влияют на пользователей лишь в некоторых случаях. Например, уведомляет о том, что страницы загружаются медленнее только в конкретном браузере или что намного медленнее обслуживаются запросы только с конкретного сервера. Можно также выявлять проблемы, связанные с сочетаниями свойств. Например, можно обнаружить медленную загрузку страниц в одном географическом регионе для клиентов в определенной операционной системе.  

Такие аномалии очень сложно обнаружить, просто просматривая данные, но они встречаются чаще, чем вы могли бы ожидать. Часто они обнаруживаются только после жалоб ваших клиентов. Но это уже слишком поздно: столкнувшиеся с проблемами пользователи уже переключаются на ваших конкурентов.

В настоящее время наши алгоритмы анализируют время загрузки страницы, время отклика на запрос на сервере и время выполнения зависимых запросов.  

Чтобы воспользоваться этой возможностью, не нужно устанавливать пороговые значения или настраивать правила. Для обнаружения аномальных шаблонов используются алгоритмы машинного обучения и интеллектуального анализа данных.

![В электронном оповещении щелкните ссылку, чтобы открыть диагностический отчет в Azure.](./media/proactive-performance-diagnostics/03.png)

* **Когда** показано время обнаружения проблемы.
* **Что** описывается:

  * обнаруженная проблема;
  * характеристики набора событий, которые демонстрировали проблемное поведение.
* В таблице набор с низкой производительностью сравнивается со средним поведением всех других событий.

С помощью ссылок перейдите к обозревателю метрик и поиска в соответствующих отчетах, отфильтрованных по времени и свойствам набора с низкой производительностью.

Измените диапазон времени и фильтры для просмотра телеметрии.

## <a name="next-steps"></a>Дальнейшие действия
Эти диагностические средства позволяют проверять данные телеметрии из приложения:

* [Профилировщик](profiler.md) 
* [Отладчик моментальных снимков](../../azure-monitor/app/snapshot-debugger.md)
* [Аналитика в Application Insights](../../azure-monitor/log-query/get-started-portal.md)
* [Интеллектуальная диагностика и анализ](../../azure-monitor/app/analytics.md)

Интеллектуальное обнаружение — это полностью автоматическая функция, но, возможно, вам потребуется настроить некоторые дополнительные оповещения.

* [Настройка оповещений в Application Insights](../../azure-monitor/app/alerts.md)
* [Доступность веб-тестов](../../azure-monitor/app/monitor-web-app-availability.md)
