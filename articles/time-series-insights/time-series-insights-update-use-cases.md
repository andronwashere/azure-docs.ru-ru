---
title: Примеры использования предварительной версии службы "Аналитика временных рядов Azure" | Документация Майкрософт
description: Общие данные о примерах использования предварительной версии службы "Аналитика временных рядов Azure"
author: ashannon7
ms.author: dpalled
ms.workload: big-data
manager: cshankar
ms.service: time-series-insights
services: time-series-insights
ms.topic: conceptual
ms.date: 04/30/2019
ms.custom: seodec18
ms.openlocfilehash: 787445d5186a173b2cba674b36cd95879cc863e5
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "66389992"
---
# <a name="azure-time-series-insights-preview-use-cases"></a>Примеры использования предварительной версии службы "Аналитика временных рядов Azure"

В этой статье перечислены некоторые распространенные варианты использования для предварительной версии Azure время Series Insights. Рекомендации в данной статье служат отправной точкой для разработки приложений и решений с помощью Time Series Insights.

В частности в этой статье содержатся ответы на следующие вопросы:

* Каковы распространенные варианты использования Аналитики временных рядов Azure?
* Каковы преимущества использования Time Series Insights для [исследование данных и обнаружение аномалий visual](#data-exploration-and-visual-anomaly-detection)?
* Каковы преимущества использования Time Series Insights для [оперативной аналитики и эффективность](#operational-analysis-and-driving-process-efficiency)?
* Каковы преимущества использования Time Series Insights для [расширенной аналитики](#advanced-analytics)?

Обзор этих сценариев использования описан в следующих разделах.

## <a name="introduction"></a>Общие сведения

Azure Time Series Insights — это предложение платформы как службы end-to-end. Оно используется, чтобы собирать, обрабатывать, хранить, анализировать и запрашивать масштабируемые данные Интернета вещей с высокой степенью контекстуализации, оптимизированные для временных рядов. Аналитика временных рядов идеально подходит для специализированного исследования данных и оперативного анализа. Time Series Insights является уникальным образом расширяемый, настраиваемый услуги соответствует широкой необходимость развертываний промышленного Интернета вещей.

## <a name="data-exploration-and-visual-anomaly-detection"></a>Исследование данных и визуализация аномалий

Изучайте и анализируйте миллиарды событий, мгновенно выявляя аномалии и скрытые тенденции в данных. Аналитика временных рядов обеспечивает выполнение рабочих нагрузок анализа для Интернета вещей и DevOps в практически реальном времени.

[![Обозреватель данных](media/v2-update-use-cases/data-explorer.svg)](media/v2-update-use-cases/data-explorer.svg#lightbox)

Большинство пользователей согласны, что скорость анализа данных является одной из сильных сторон Аналитики временных рядов. Аналитика временных рядов Azure не требует предварительной подготовки данных. Служба быстро работает и за несколько минут подключается к миллиарду событий в центре Интернета вещей или в центрах событий Azure. Подсоединившись, изучайте и анализируйте миллиарды событий, мгновенно выявляя аномалии и скрытые тенденции в данных.

Служба "Аналитика временных рядов Azure" удобная и простая в использовании. Для взаимодействия с данными вам не потребуется писать свой код. Вам также не нужно осваивать новые языки программирования. Аналитика временных рядов Azure предоставляет подробные текстовые запросы для продвинутых пользователей, знакомых с SQL. Она также предоставляет изучение для новичков с помощью команд "выберите" и "щелкните".

Клиенты пользуются преимуществами скорости для быстрой диагностики проблем со свойствами. Они могут использовать DevOps в целях устранения основной причины ошибки в решениях Интернета вещей. Они также могут определить области для изучения инициатив по обработке и анализу данных.  

Существует три основных способа для взаимодействия с данными, хранящимися в Аналитике временных рядов Azure.

- Первый и самый простой способ начать работу — с помощью обозревателя Аналитики временных рядов Azure. Его можно использовать для быстрой визуализации всех данных Интернета вещей в одном расположении. Он предоставляет такие инструменты, как тепловая карта, для выявления аномалий в данных. Он также предоставляет перспективное представление. Используйте его для сравнения не больше четырех представлений из одной или нескольких сред Аналитики временных рядов на одной панели мониторинга. Панель мониторинга дает вам представление о данных временных рядов для всех расположений. Сведения об [Обозревателе предварительной версии службы "Аналитика временных рядов Azure"](./time-series-insights-update-explorer.md). Узнайте, как спланировать среду Аналитики временных рядов из статьи [Планирование Аналитики временных рядов](./time-series-insights-update-plan.md).

- Второй способ — использовать пакет SDK JavaScript для быстрого внедрения мощных диаграмм и графиков в веб-приложения. С помощью нескольких строк кода вы можете создавать эффективные запросы. Используйте их для заполнения графиков, круговых и линейных диаграмм, тепловых карт, сеток данных и многого другого. Все эти элементы готовы к использованию с помощью пакета SDK. Пакет SDK также извлекает API запросов Аналитики временных рядов. Их можно использовать для создания SQL-подобных предикатов для обращения к данным, которые вы хотите показать на панели мониторинга. Для гибридных решений уровня представления Аналитика временных рядов предлагает параметризованные URL-адреса. Они обеспечивают беспрерывную точку подключения с помощью обозревателя Аналитики временных рядов для подробного анализа данных.

    * Чтение [JS Insights ряда времени клиентская библиотека](tutorial-explore-js-client-lib.md) и [клиента Time Series Insights](https://github.com/Microsoft/tsiclient) документации для получения дополнительных сведений о пакете SDK для JavaScript.

    * Дополнительные сведения об общем доступе к URL-адреса и новый компонент пользовательского интерфейса, просмотрев [визуализации данных в окне предварительного просмотра аналитики Azure во время ряда](time-series-insights-update-explorer.md).

- Третий способ работы — использовать эффективные API-интерфейсы для запроса данных, хранящихся в Аналитике временных рядов. Аналитика временных рядов имеет временных операторов, таких как `from`, `to`, `first`, и `last`. Он имеет статистические схемы и преобразования например `average`, `min`, `max`, `split by`, `order by`, и `DateHistogram`. Есть также, такие как фильтрация операторы `has`, `in`, `and`, `or`, `greater than`, и `REGEX`. Эти операторы позволяют целевому приложению быстро найти необычные тенденции и закономерности в ваших данных. Используйте их для заполнения собственной визуализации для выявления аномалий.

## <a name="operational-analysis-and-driving-process-efficiency"></a>Анализ работы и повышение эффективности процессов

Используйте Аналитику временных рядов для мониторинга работоспособности, использования и производительности оборудования в нужном масштабе. Аналитика временных рядов предоставляет простой способ измерить эффективность производительности. Аналитика временных рядов позволяет обрабатывать разнообразные труднопредсказуемые рабочие нагрузки Интернета вещей без ущерба для приема данных или выполнения запросов.

[![Обзор](media/v2-update-use-cases/overview.svg)](media/v2-update-use-cases/overview.svg#lightbox)

В сочетании с правильными технологиями и решениями потоковая передача и непрерывная обработка данных рабочих процессов могут успешно преобразить любую компанию. Часто эти решения объединяют несколько систем. Они позволяют исследовать и анализировать данные, которые постоянно меняются, особенно в области Интернета вещей, и следуют общему шаблону.

Эти шаблоны часто начинаются с платформ, которые поддерживают Интернет вещей и принимают миллиарды событий от устройств и датчиков с различными языковыми стандартами. Затем они обрабатывают и анализируют потоковые данные для получения подробных сведений в реальном времени. Данные обычно архивируются в "теплое" и "холодное" хранилище для анализа пакетов в практически реальном времени.

Собранные данные проходят через последовательные этапы обработки для очистки и контекстуализации для целевых запросов и сценариев аналитики. Azure предлагает широкий выбор служб, например, производство и обслуживание средств, которые можно применять к сценариям Интернета вещей. Эти службы включают в себя Аналитику временных рядов, центр Интернета вещей, Центры событий Azure, Azure Stream Analytics, Функции Azure, Azure Logic Apps, Azure Databricks, Машинное обучение Azure и Power BI.

Архитектура решений осуществляется следующими способами.

- Принимая данные через центр Интернета вещей или центры событий, сохраняя тем самым лучшую в своем классе безопасность, пропускную способность и задержку.
- Выполняя обработку данных и вычислений. Пропустите полученные данные через следующие службы: Stream Analytics, Logic Apps и Функции Azure. Использование служб зависит от конкретных потребностей обработки данных.
- Отправляя вычисленные сигналы из конвейера обработки в Аналитику временных рядов для хранения и аналитики.

Аналитика временных рядов обеспечивает исследование данных и анализ средств на основе исторических данных в практически реальном времени. В зависимости от требований вашего предприятия задания MapReduce и Hive можно запускать на данных, хранящихся в Аналитике временных рядов, путем подключения службы к Azure HDInsight. Данные, хранящиеся в Аналитике временных рядов, доступны в Power BI и других клиентских приложениях через запросы Аналитики временных рядов с интерфейсов API на общей поверхности. Эти данные можно использовать на продвинутом этапе работы и для оперативной аналитики.

## <a name="advanced-analytics"></a>Расширенная аналитика

Обеспечьте интеграцию со службами расширенной аналитики, такими как машинное обучение Azure и Azure Databricks. Аналитика временных рядов передает необработанные данные с миллионов устройств. Служба добавляет контекстные данные, которые можно легко использовать с помощью набора служб Azure Analytics.

[![Аналитика](media/v2-update-use-cases/advanced-analytics.svg)](media/v2-update-use-cases/advanced-analytics.svg#lightbox)

Расширенная аналитика и машинное обучение используют и обрабатывают большие объемы данных. Эти данные используются для принятия решений на основе данных и прогнозного анализа. В случае использования Интернета вещей расширенные аналитические алгоритмы получают сведения о данных из миллионов устройств. Эти устройства передают данные несколько раз в секунду. Данные, которые поступают с устройств Интернета вещей, не обработаны. Они не содержат контекстной информации, такой как расположение устройства и единицы измерения показания датчика. Таким образом, необработанные данные трудно использовать непосредственно в расширенной аналитике.

Аналитика временных рядов позволяет устранить разрыв между данными Интернета вещей и расширенной аналитикой одним из двух простых и экономически выгодных способов.

- Во-первых, Аналитика временных рядов собирает необработанные данные телеметрии из миллионов устройств с помощью центра Интернета вещей. Она дополняет данные с помощью контекстной информации и преобразует их в формат parquet. Этот формат может легко интегрироваться с такими службами расширенной аналитики, как машинное обучение Azure, Azure Databricks, а также со сторонними приложениями.

    Аналитика временных рядов может служить источником истины для всех данных, используемых на предприятии. Она создает центральный репозиторий для использования рабочих нагрузок по целевой аналитике. Так как Аналитика временных рядов является службой хранения практически в реальном времени, модели расширенной аналитики постоянно дополняются входящими данными телеметрии Интернета вещей. Таким образом, эти модели могут делать более точные прогнозы.

- Во-вторых, Аналитика временных рядов может использоваться для вывода данных моделей машинного обучения и прогнозирования, чтобы визуализировать и сохранять результаты. Эта процедура помогает предприятиям оптимизировать и настроить свои модели. Аналитика временных рядов позволяет легко визуализировать потоковые данные телеметрии в одной плоскости, как и обученные выходные данные моделей. Тем самым она помогает командам обработки и анализа данных выявлять аномалии и определять шаблоны.  

## <a name="next-steps"></a>Дальнейшие действия

- Сведения об [Обозревателе предварительной версии службы "Аналитика временных рядов Azure"](./time-series-insights-update-explorer.md).
- Чтение [предварительного просмотра Insights рядов во время планирования](./time-series-insights-update-plan.md) планирование вашей среды.
- Ознакомьтесь с документацией [Time Series Insights client](https://github.com/Microsoft/tsiclient) (Клиент службы "Аналитика временных рядов").
