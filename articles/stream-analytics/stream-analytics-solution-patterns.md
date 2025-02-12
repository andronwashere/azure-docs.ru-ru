---
title: Шаблоны решений Azure Stream Analytics
description: Изучите шаблоны решений для Azure Stream Analytics.
author: zhongc
ms.author: zhongc
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 06/21/2019
ms.openlocfilehash: 5929ff439bc31e16643e5c57868cd6b68f9cd99c
ms.sourcegitcommit: 08138eab740c12bf68c787062b101a4333292075
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/22/2019
ms.locfileid: "67329573"
---
# <a name="azure-stream-analytics-solution-patterns"></a>Шаблоны решений Azure Stream Analytics

Как и многие другие службы в Azure Stream Analytics лучше всего использовать с другими службами для создания больших решений end-to-end. В этой статье рассматриваются простые решения Azure Stream Analytics и различные шаблоны архитектуры. Вы можете создать на эти шаблоны для разработки более сложных решений. Шаблоны, описанные в этой статье можно использовать в самых разнообразных сценариях. Примеры шаблонов, зависящих от сценария доступны на [архитектуры решений Azure](https://azure.microsoft.com/solutions/architecture/?product=stream-analytics).

## <a name="create-a-stream-analytics-job-to-power-real-time-dashboarding-experience"></a>Создание задания Stream Analytics для работы в режиме реального времени панелей мониторинга power

С помощью Azure Stream Analytics вы можете быстро настроить в реальном времени панелей мониторинга и предупреждений. Простое решение принимает события из концентраторов событий или центра Интернета вещей и [веб-каналов панели мониторинга Power BI с набором данных потоковой передачи](/power-bi/service-real-time-streaming). Дополнительные сведения см. подробный Учебник [анализ данных телефонных звонков с помощью Stream Analytics и отображение результатов на панели мониторинга Power BI](stream-analytics-manage-job.md).

![Панель мониторинга ASA Power BI](media/stream-analytics-solution-patterns/pbidashboard.png)

Это решение может быть построен в несколько минут с портала Azure. Есть обширный писать код не участвует и язык SQL используется для реализации бизнес-логики.

Этот шаблон решения предлагает наименьшую задержку из источника события к панели мониторинга Power BI в браузере. Azure Stream Analytics является единственную службу Azure с помощью эта встроенная возможность.

## <a name="use-sql-for-dashboard"></a>Для панели мониторинга с помощью SQL

Панели мониторинга Power BI предлагает низкую задержку, но он не может использоваться для составления полной полноценным календарным отчетов Power BI. Стандартная процедура отчетов — для вывода данных в базе данных SQL, сначала. Затем используйте соединитель SQL Power BI запрос SQL для последних данных.

![Панель мониторинга ASA SQL](media/stream-analytics-solution-patterns/sqldashboard.png)

С помощью SQL базы данных обеспечивает большую гибкость, но за счет немного дольше. Это решение является оптимальным для заданий с требованиями к задержке, больше, чем одну секунду. С помощью этого метода можно максимально увеличить возможности Power BI для дальнейшего среза и поперечные срезы данных для отчетов и гораздо больше возможностей для визуализации. Вы также получаете гибкость использования другие панели мониторинга решения, например Tableau.

SQL не является хранилищем данных высокой пропускной способностью. Максимальная пропускная способность в базе данных SQL, из Azure Stream Analytics в настоящее время — около 24 МБ в секунду. Если источники событий в решении предоставляют данные с более высокой ставке, необходимо использовать логику обработки в Stream Analytics для снижения скорости выходных данных для SQL. Методы, такие как фильтрация, агрегаты, сопоставления с временные соединения шаблонов и аналитические функции можно использовать. Частота исходящих to SQL можно дополнительно оптимизировать с помощью методик, описанных в [выходные данные Azure Stream Analytics к базе данных SQL Azure](stream-analytics-sql-output-perf.md).

## <a name="incorporate-real-time-insights-into-your-application-with-event-messaging"></a>Включить в реальном времени ценные сведения приложения с помощью сообщений о событиях

Во-вторых, наиболее популярных Stream Analytics используется для создания предупреждений в реальном времени. В этом шаблоне решения бизнес-логику в Stream Analytics можно использовать для обнаружения [temporal и пространственных шаблонов](stream-analytics-geospatial-functions.md) или [аномалии](stream-analytics-machine-learning-anomaly-detection.md), а затем создают предупреждения сигналы. Тем не менее в отличие от панели мониторинга решения, где Stream Analytics использует Power BI в качестве предпочтительной оконечной точки, можно использовать несколько приемников интеллектуальному анализу данных. Эти приемники включают концентраторов событий служебной шины и функций Azure. Как построитель приложений, необходимо решить, какой приемник данных лучше всего подходит для вашего сценария.

Логику подчиненных событий потребитель должен быть реализован для создания предупреждений в существующие рабочие процессы предприятия. Так как можно реализовать пользовательскую логику в функциях Azure, функции Azure является самым быстрым способом, можно выполнить такой интеграции. Учебник по использованию функции Azure, как выходные данные задания Stream Analytics можно найти в [выполнения функций Azure из заданий Azure Stream Analytics](stream-analytics-with-azure-functions.md). Функции Azure также поддерживает различные типы уведомлений, включая текст и электронной почты. Приложение логики может также использоваться для такой интеграции с концентраторами событий между Stream Analytics и приложение логики.

![Приложение обмена сообщениями событий ASA](media/stream-analytics-solution-patterns/eventmessagingapp.png)

Концентраторы событий, с другой стороны, предлагает самый гибкий точки интеграции. Многие другие службы, такие как обозреватель данных Azure "и" Аналитика временных рядов можно использовать события из концентраторов событий. Службы можно подключить напрямую к приемнику концентраторов событий из Azure Stream Analytics для завершения решения. Концентраторы событий также является наивысшим пропускной способности брокера обмена сообщениями доступный в Azure для таких сценариев интеграции.

## <a name="dynamic-applications-and-websites"></a>Динамические приложения и веб-сайтов

Можно создать пользовательские визуализации в режиме реального времени, таких как панели мониторинга или визуализация, карты, с помощью Azure Stream Analytics и служба Azure SignalR. С помощью SignalR, веб-клиенты могут обновляться и отображения динамического содержимого в режиме реального времени.

![Динамические приложения ASA](media/stream-analytics-solution-patterns/dynamicapp.png)

## <a name="incorporate-real-time-insights-into-your-application-through-data-stores"></a>Включите в реальном времени ценные сведения приложения с помощью хранилища данных

Большинство веб-служб и веб-приложений уже сегодня воспользоваться шаблоном запрос ответ для обслуживания уровня представления данных. Шаблону запрос/ответ можно легко создавать и могут легко масштабироваться с временем отклика низкой, используя внешний интерфейс без отслеживания состояния и масштабируемые хранилищ, таких как Cosmos DB.

Тома данных часто создает узкие места производительности в системе на базе CRUD. [Решения шаблон источников событий](/azure/architecture/patterns/event-sourcing) используется для устранения узких мест производительности. Временные структуры и аналитики, также сложно и неэффективно извлекать из хранилища данных с традиционной. Современные большим объемом данными приложения часто принимают архитектура на основе потока данных. Azure Stream Analytics в качестве системы вычислений для перемещаемых данных является швейных в этой архитектуре.

![Приложение источники событий ASA](media/stream-analytics-solution-patterns/eventsourcingapp.png)

В этом шаблоне решения события обрабатываются и объединяются в хранилища данных в Azure Stream Analytics. На уровне приложения взаимодействует с хранилищами данных, в формате традиционных запрос/ответ. Из-за возможность обработки большого количества событий в Stream Analytics в реальном времени, приложение имеет высокую степень масштабируемости без необходимости массового копирования на уровне хранилища данных. Уровень хранилища данных — по существу материализованное представление в системе. [Выходные данные Azure Stream Analytics в Azure Cosmos DB](stream-analytics-documentdb-output.md) описывает, каким образом Cosmos DB используется в качестве выходных данных Stream Analytics.

В реальных приложениях, где логика обработки сложных и здесь является просто обновить некоторые части логики независимо друг от друга, несколько заданий Stream Analytics может быть задан вместе с концентраторы событий в качестве промежуточных событий event broker.

![Приложение источники сложных событий ASA](media/stream-analytics-solution-patterns/eventsourcingapp2.png)

Этот шаблон повышает устойчивость и управляемости системы. Тем не менее несмотря на то, что Stream Analytics гарантирует обработку только один раз, есть небольшая вероятность того, может оказаться повторяющиеся события в концентраторах событий и промежуточный. Очень важно для дедупликации событий с помощью логики ключи в окне lookback подчиненных задания Stream Analytics. Дополнительные сведения о доставке событий, см. в разделе [гарантирует доставку событий](/stream-analytics-query/event-delivery-guarantees-azure-stream-analytics) ссылки.

## <a name="use-reference-data-for-application-customization"></a>Использование ссылочных данных для индивидуальной настройки приложений

Функция эталонных данных Azure Stream Analytics предназначена для настройки конечных пользователей как порог, правила обработки, предупреждения и [геозон](geospatial-scenarios.md). На уровне приложения можно принять изменения параметров и хранить их в базу данных SQL. В задание Stream Analytics периодически запрашивает изменения из базы данных и делает доступным параметров настройки через соединение ссылочных данных. Дополнительные сведения о том, как использовать ссылочные данные для индивидуальной настройки приложений см. в разделе [SQL ссылочных данных](sql-reference-data.md) и [соединение ссылочных данных](/stream-analytics-query/reference-data-join-azure-stream-analytics).

Этот шаблон может также использоваться для реализации обработчика правил где определенные правила пороговых значений из ссылочных данных. Дополнительные сведения о правилах см. в разделе [обрабатывать можно настроить правила на основе пороговых значений в Azure Stream Analytics](stream-analytics-threshold-based-rules.md).

![ASA ссылку данных приложения](media/stream-analytics-solution-patterns/refdataapp.png)

## <a name="add-machine-learning-to-your-real-time-insights"></a>Добавление машинного обучения в реальном времени аналитические сведения

Azure Stream Analytics встроенные [модель обнаружения аномалий](stream-analytics-machine-learning-anomaly-detection.md) является удобным способом представить машинного обучения в приложение в режиме реального времени. Для различных задач широкий диапазон из машинного обучения, см. в разделе [Azure Stream Analytics интегрируется со службой оценки машинного обучения Azure](stream-analytics-machine-learning-integration-tutorial.md).

Опытные пользователи, которые хотят внедрить интерактивные учебные курсы и оценки в одном конвейере Stream Analytics, см. в этот пример того, как выполнить с помощью [линейной регрессии](stream-analytics-high-frequency-trading.md).

![ASA приложения машинного обучения](media/stream-analytics-solution-patterns/mlapp.png)

## <a name="near-real-time-data-warehousing"></a>Рядом с хранилищ данных в режиме реального времени

Другим распространенным шаблоном является данных в реальном времени хранения, который также называется потоковой передачи хранилище данных. Помимо событий, поступающих в концентраторы событий и центром Интернета вещей из своего приложения [Azure Stream Analytics в IoT Edge](stream-analytics-edge.md) может использоваться для выполнения очистки данных, сокращение данных и хранилища данных и прямой потребностей. Stream Analytics в IoT Edge может корректно обрабатывать ограничение пропускной способности и проблемы с подключением в системе. Выходной адаптер SQL можно использовать для выходных данных в хранилище данных SQL; Тем не менее максимальная пропускная способность ограничена 10 МБ/с.

![Хранилища данных ASA](media/stream-analytics-solution-patterns/datawarehousing.png)

Является одним из способов повышения пропускной способности с помощью некоторых компромисс задержки для архивирования событий в хранилище BLOB-объектов Azure, а затем [импортировать их в хранилище данных SQL с помощью Polybase](../sql-data-warehouse/load-data-from-azure-blob-storage-using-polybase.md). Вы должны вручную Сшивать выходных данных из Stream Analytics в хранилище BLOB-объектов и входные данные из хранилища BLOB-объектов в хранилище данных SQL с [архивировать данные по метке времени](stream-analytics-custom-path-patterns-blob-storage-output.md) и периодически импорта.

В этом шаблоне использования Azure Stream Analytics используется как механизм ETL практически в реальном времени. Вновь поступающие события постоянно будут преобразованы и сохранены для использования службы подчиненных analytics.

![ASA высокую пропускную способность, хранение данных](media/stream-analytics-solution-patterns/datawarehousing2.png)

## <a name="archiving-real-time-data-for-analytics"></a>Архивация данных в реальном времени для аналитики

Большинство действий обработки и анализа данных по-прежнему происходит вне сети. Данные можно архивировать в Azure Stream Analytics через Azure Data Lake Store Gen2 выходных данных и форматы выходных данных Parquet. Эта функция удаляет трудностей для передачи данных непосредственно в Azure Data Lake Analytics, Azure Databricks и Azure HDInsight. Azure Stream Analytics используется в качестве практически в реальном времени ядра ETL в этом решении. Можно просматривать архивированные данные в Data Lake с помощью различных вычислительных ядер.

![ASA автономной аналитике](media/stream-analytics-solution-patterns/offlineanalytics.png)

## <a name="use-reference-data-for-enrichment"></a>Использование ссылочных данных для обогащения

Обогащение данных часто является требованием для обработчиков ETL. Azure Stream Analytics поддерживает обогащение данных с помощью [ссылочные данные](stream-analytics-use-reference-data.md) из базы данных SQL и хранилище BLOB-объектов Azure. Обогащение данных можно сделать для данных, размещения в Azure Data Lake и хранилище данных SQL.

![ASA автономной аналитике с обогащение данных](media/stream-analytics-solution-patterns/offlineanalytics.png)

## <a name="operationalize-insights-from-archived-data"></a>Ввод в эксплуатацию ценной информации из архивных данных

Если шаблон автономной аналитике эмоцию практически в реальном времени приложения шаблона, можно создать цикл обратной связи. Цикл обратной связи позволяет приложению автоматически подстраивается под изменения закономерности в данных. Такая цепь обратной связи, можно изменить пороговое значение для предупреждений, или сложным, как повторное Обучение моделей машинного обучения. Одну и ту же архитектуру решения могут применяться для обоих заданий ASA, выполняемых в облаке и в IoT Edge.

![ASA insights ввода в эксплуатацию](media/stream-analytics-solution-patterns/insightsoperationalization.png)

## <a name="how-to-monitor-asa-jobs"></a>Как отслеживать задания ASA

Задание Azure Stream Analytics может выполняться как 24/7, чтобы обработать входящие события постоянно в режиме реального времени. Его гарантии бесперебойной работы крайне важны для работоспособности приложения в целом. Хотя Stream Analytics — единственная служба потоковой аналитики в отрасли, предлагает [доступность на уровне 99,9%](https://azure.microsoft.com/support/legal/sla/stream-analytics/v1_0/), вы по-прежнему можете понести некоторый уровень время простоя. С годами Stream Analytics был представлен метрик, журналов и задания состояния, отражающие работоспособность задания. Все они регистрируются через службу Azure Monitor и дальнейшей экспортироваться в OMS. Дополнительные сведения см. в разделе [задания понять Stream Analytics, мониторинг и порядок мониторинга запросов](stream-analytics-monitoring.md).

![Мониторинг ASA](media/stream-analytics-solution-patterns/monitoring.png)

Существует два важных момента для мониторинга:

- [Задание в состоянии сбоя](job-states.md)

    Прежде всего необходимо убедиться, что задание выполняется. Без задания в состоянии выполнения создаются не новые метрики и журналы. Задания можно изменить в состоянии сбоя по различным причинам, в том числе высокий уровень использования единиц потоковой Передачи (т. е. заканчиваются ресурсы).

- [Метрики задержки водяного знака](https://azure.microsoft.com/blog/new-metric-in-azure-stream-analytics-tracks-latency-of-your-streaming-pipeline/)

    Эта Метрика отражает, насколько далеко за обработку данных конвейер находится в реального времени (в секундах). Некоторые задержки относимый на логику обработки. В результате мониторинга растущий тренд намного важнее, чем абсолютное значение мониторинга. Задержка устойчивого состояния должны быть обсуждены в при разработке приложения не, мониторинг и оповещения.

В случае сбоя журналы действий и [журналы диагностики](stream-analytics-job-diagnostic-logs.md) являются лучших учебников, содержащих начинать поиск ошибок.

## <a name="build-resilient-and-mission-critical-applications"></a>Устойчивость сборки и критически важных приложений

Независимо от того, для получения гарантии SLA Azure Stream Analytics и тщательности запустите приложение end-to-end, происходят сбои. Если приложение является критически важным, необходимо подготовить для сбоев для корректного восстановления.

Для оповещения приложения, самое главное заключается в обнаружении следующего предупреждения. Вы можете перезапустить задание из текущего времени при восстановлении, игнорируя прошлые оповещения. По в первый раз выходные данные, не входной первом являются семантикой времени начала задания. Входные данные перематывается обратной соответствующий объем времени, чтобы гарантировать, что первые выходные данные в указанное время являются полными и правильными. Не будет получить частичного статистические выражения и активирует оповещения в результате неожиданно.

Вы также можете начало передачи выходных данных из минимальное количество времени в прошлом. Центр Интернета вещей и концентраторов событий политики хранения удерживайте разумный объем данных, чтобы разрешить обработку в прошлом. Недостаток заключается в том как быстро устранить вплоть до текущего времени и приступить к созданию своевременно новые оповещения. Данные быстро теряет его значение со временем, поэтому очень важно быстро отставать на текущий момент времени. Чтобы быстро Догнать двумя способами:

- Подготовьте дополнительные ресурсы (SU), если синхронизируется.
- Перезапустите из текущего времени.

Перезапуск из текущего время можно легко сделать, но обеспечить соответствие оставляя разрыв во время обработки. Перезапуск таким образом может подходить для оповещений, но может быть проблематично для сценариев мониторинга и является не starter для архивирования и хранения данных.

Подготовка дополнительных ресурсов можно ускорить процесс, но результат применения поражен скорость обработки сложных.

- Тест, что задание находится масштабируемыми, чтобы увеличить число единиц потоковой передачи. Не все запросы можно масштабировать. Необходимо убедиться, что запрос готов [параллельно](stream-analytics-parallelization.md).

- Убедитесь, что имеется достаточное количество разделов в вышестоящего концентраторов событий или центра Интернета вещей, которые можно добавить дополнительные единицы пропускной способности единиц пропускной (способности) для масштабирования пропускной способности ввода. Помните, что каждый TU концентраторов событий используется максимальное число qpu ставку выходных данных размером 2 МБ в секунду.

- Убедитесь, что вы подготовили недостаточно ресурсов в приемника выходных данных (т. е. база данных SQL, Cosmos DB), поэтому они не регулировать всплеск результат, который иногда может привести к блокировке в системе.

Наиболее важно предвидеть изменение скорости обработки и протестировать эти сценарии до передачи в производство можно масштабировать обработку правильно во время сбоя восстановления.

В крайнем случае, все входящие события отложенной [это возможно, всем отложенные события будут удалены](stream-analytics-time-handling.md) Если вы применили позднее прибывающих окна к заданию. Удаление событий может показаться загадочным поведение в начале; Тем не менее учитывая Stream Analytics — это модуль обработки в реальном времени, ожидается, что входящие события, чтобы быть близка к реального времени. При этом приходится удалять события, нарушающие эти ограничения.

### <a name="backfilling-process"></a>Процесс предоставляемых каталогом

К счастью корректно обрабатывать эти события можно использовать предыдущий шаблон архивирования данных. Суть в том случае, что задание архивирования обрабатывает входящие события в время прибытия и архивирует события в сегмент нужное время в BLOB-объектов Azure или Azure Data Lake Store с помощью их время события. Неважно, как позднее событие поступает, он никогда не удаляется. Он всегда будет располагаться в контейнере нужное время. Во время восстановления можно выполнить повторную обработку архивные события и задним числом результатов для указанного хранилища.

![ASA задним числом](media/stream-analytics-solution-patterns/backfill.png)

Задним числом процесс должен выполняться с помощью автономной пакетной обработки в системе, которая скорее всего, имеет другую модель программирования, чем Azure Stream Analytics. Это означает, что вам нужно повторно реализовать логику всей обработки.

Для предоставляемых каталогом важно по-прежнему хотя бы временно выделяйте приемников дополнительные ресурсы для выходных данных для обработки более высокой пропускной способности, чем в рабочем режиме, обработки потребностей.

|Сценарии  |Перезапустить сейчас только от  |Перезапуск с момента последней остановки |Перезапуск с now + задним числом с архивные событиями|
|---------|---------|---------|---------|
|**Панелей мониторинга**   |Создает пробел    |ОК для кратковременного отключения    |Используйте для длинных сбоя |
|**Предупреждения**   |Хорошо |ОК для кратковременного отключения    |Не являются обязательными |
|**Приложение источники событий** |Хорошо |ОК для кратковременного отключения    |Используйте для длинных сбоя |
|**Хранение данных**   |Потери данных  |Хорошо |Не являются обязательными |
|**Автономной аналитике**  |Потери данных  |Хорошо |Не являются обязательными|

## <a name="putting-it-all-together"></a>Сборка

Представьте, что все рассмотренные выше модели решения можно объединить в сложной системе end-to-end несложно. Объединенные системы может включать панелей мониторинга, оповещения, приложение источники событий, хранилища данных и возможности автономной аналитике.

Ключ является проектировать системы в составных шаблонов, так, чтобы каждой подсистемы можно создавать, тестировать, обновлен и восстановить независимо друг от друга.

## <a name="next-steps"></a>Дальнейшие действия

Теперь видно широкий набор шаблонов решений, с помощью Azure Stream Analytics. Теперь вы можете вникнуть в детали и создать свое первое задание Stream Analytics:

* [Руководство по созданию задания Stream Analytics с помощью портала Azure](stream-analytics-quick-create-portal.md)
* [Руководство по созданию задания Stream Analytics с помощью Azure PowerShell](stream-analytics-quick-create-powershell.md)
* [Краткое руководство. Создание задания Stream Analytics с использованием инструментов Azure Stream Analytics для Visual Studio](stream-analytics-quick-create-vs.md).
