---
title: Добавление аналитики в API Bing для поиска в Интернете
titleSuffix: Azure Cognitive Services
description: Статистика Bing предоставляет аналитику для API Bing для поиска изображений. Аналитика включает в себя вызов тома, часто используемые строки запроса, географическое распределение и многое другое.
services: cognitive-services
author: swhite-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-web-search
ms.topic: article
ms.date: 07/17/2019
ms.author: scottwhi
ms.custom: seodec2018
ms.openlocfilehash: 40a5e03f4149381f096f71243361eacbdc7c16c2
ms.sourcegitcommit: fecb6bae3f29633c222f0b2680475f8f7d7a8885
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/30/2019
ms.locfileid: "68667604"
---
# <a name="add-analytics-to-the-bing-search-apis"></a>Добавление аналитики в API-интерфейсы поиска Bing

Статистика Bing предоставляет аналитику для API-интерфейсы поиска Bing. К этим аналитикам относятся объем вызовов, основные строки запросов, географическое распределение и многое другое. Вы можете включить статистику Bing в [портал Azure](https://ms.portal.azure.com) , перейдя к своему ресурсу Azure и выбрав **Включить статистику Bing**.

> [!IMPORTANT]
> * Статистика Bing недоступна для бесплатных пробных подписок или ресурсов в ценовой `F0` категории "бесплатный".
> * Вы не можете использовать любые данные, доступные с помощью панели инструментов статистики Bing, для создания приложений, которые можно распространять третьим лицам.
> * Включение статистики Bing немного увеличивает скорость подписки. Дополнительные сведения см. в разделе [цены](https://aka.ms/bingstatisticspricing) .


На следующем рисунке показана доступная аналитика для каждой конечной точки API Поиск Bing.

![Распределение по матрице поддержки конечной точки](./media/bing-statistics/bing-statistics-matrix.png)

## <a name="access-your-analytics"></a>Доступ к аналитике

Службы Bing обновляют данные аналитики каждые 24 часа и обслуживают историю, доступную на [панели мониторинга аналитики](https://bingapistatistics.com)до 13 месяцев. Убедитесь, что вы вошли с помощью того же учетная запись Майкрософт (MSA), который использовался для регистрации статистики Bing.

> [!NOTE]  
> * Метрики могут отобразится на панели мониторинга в течение 24 часов. На панели мониторинга отображаются дата и время последнего обновления данных.  
> * Метрики доступны с момента включения надстройки "Статистика Bing".

## <a name="filter-the-data"></a>Фильтрация данных

По умолчанию на диаграммах и диаграммах отображаются все метрики и данные, к которым у вас есть доступ. Можно отфильтровать данные, отображенные в диаграммах и графах, выбрав интересующие вас ресурсы, рынки, конечные точки и отчетный период. Вы можете изменить следующие фильтры:

- **Идентификатор ресурса**: Уникальный идентификатор ресурса, который определяет подписку Azure. Список содержит несколько идентификаторов, если вы подписаны на несколько уровней API поиска Bing. По умолчанию выбраны все ресурсы.  
  
- **Рынки**. Рынки, по которым возвращаются результаты. Например, en-us (английский, США). По умолчанию выбраны все рынки. `en-WW` Рынка — это тот, который используется в Bing, если звонок не указывает на рынке, и Bing не может определить рынка пользователей.  
  
- **Конечные точки**. Конечные точки API-интерфейсов поиска Bing. Список содержит все конечные точки, для которых у вас есть платная подписка. По умолчанию выбраны все конечные точки.  

- **Интервал времени**. Отчетный период. Можно указать:
  - **Все**. Включает данные до 13 месяцев.  
  - **За последние 24 часа**: Включает аналитику за последние 24 часа  
  - **Прошлая неделя**: Включает аналитику за предыдущие семь дней  
  - **Последний месяц**: Включает аналитику за предыдущие 30 дней  
  - **Настраиваемый диапазон дат**: Включает аналитику из указанного диапазона дат, если он доступен  

## <a name="charts-and-graphs"></a>Диаграммы и графы

На панели мониторинга показаны диаграммы и графы показателей, доступных для выбранной конечной точки. Не все метрики доступны для всех конечных точек. Диаграммы и графы для каждой конечной точки являются статическими (вы не можете отображать диаграммы и графы). На панели мониторинга отображаются только диаграммы и графы, для которых отсутствуют данные.

<!--
For example, if you don't include the User-Agent header in your calls, the dashboard will not include device-related graphs.
-->

Ниже приведены возможные метрики и ограничения конечной точки.

- **Количество вызовов**. Показывает количество вызовов, сделанных в течение отчетного периода. Если отчетный период составляет один день, диаграмма показывает количество вызовов в час. В противном случае на диаграмме показано количество вызовов, сделанных за день отчетного периода.  
  
  > [!NOTE]
  > Объем вызова может отличаться от объема, указанного в отчетах по выставлению счетов, который обычно содержит только успешные вызовы.

- **Наиболее активные запросы**. Показывает самые активные запросы и количество повторений каждого запроса за отчетный период. Можно настроить количество отображаемых запросов. Например можно показать верхние запросы — 25, 50 или 75. Метрика Top Queries не доступна для следующих конечных точек:  

  - /images/trending
  - /images/details
  - /images/visualsearch
  - /videos/trending
  - /videos/details
  - /news
  - /news/trendingtopics
  - /suggestions  
  
  > [!NOTE]  
  > Некоторые условия запроса могут подавляться для удаления конфиденциальной информации, такой как электронные письма, номера телефонов, SSN и т. д.

- **Географическое распределение**. Рынки, на которых берутся результаты поиска. Например, `en-us` (английский, США). Bing использует параметр запроса `mkt` для определения рынка запроса, если указано. В противном случае Bing использует сигналы, такие как IP-адрес вызывающего абонента для определения рынка.

- **Распределение кода отклика**. Коды состояния HTTP всех вызовов за отчетный период.

- **Распределение источников вызовов**. Типы браузеров, используемые пользователями. Например, Microsoft Edge, Chrome, Safari и FireFox. Вызовы, выполненные извне браузера (например, программы-роботы, POST или с помощью функции «перелистывание» из консольного приложения), группируются в разделе библиотеки. Источник определяется с помощью значения заголовка User-Agent в запросе. Если запрос не содержит заголовок User-Agent, Bing пытается получить источник из других сигналов.  

- **Распределение безопасного поиска**. Распределение безопасных значений поиска. Например, "Выключено", "Среднее" или "Строгое". Параметр запроса `safeSearch` содержит значение, если указано. В противном случае Bing использует значение по умолчанию — "Среднее".  

- **Распределение по запрошенным ответам**. Поиск в Интернете API отвечает на `responseFilter` запросы, запрошенные в параметре запроса.  

- **Распределение по возвращенным ответам**. Ответы, которые API веб-поиска возвращает в ответе.

- **Распределение по ответам сервера**. Сервер приложений, который обслуживал ваши запросы API. Возможные значения: Bing.com (для трафика, полученного с настольных и переносных устройств) и Bing.com-mobile (для трафика, полученного с мобильных устройств). Сервер определяется с помощью значения заголовка User-Agent в запросе. Если запрос не содержит заголовок User-Agent, Bing пытается получить сервер из других сигналов.

## <a name="next-steps"></a>Следующие шаги

* [Что такое API-интерфейсы поиска Bing?](bing-api-comparison.md)
* [Использование и отображение требований к Поиск Bing API](use-display-requirements.md)
