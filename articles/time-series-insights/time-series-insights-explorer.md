---
title: Просмотр данных с помощью обозревателя аналитики временных рядов Azure | Документация Майкрософт
description: В этой статье описывается, как использовать обозреватель службы "Аналитика временных рядов Azure" в веб-браузере, чтобы быстро просмотреть глобальное представление больших данных и проверить среду Интернета вещей.
ms.service: time-series-insights
services: time-series-insights
author: ashannon7
ms.author: dpalled
manager: cshankar
ms.reviewer: v-mamcge, jasonh, kfile
ms.devlang: csharp
ms.workload: big-data
ms.topic: conceptual
ms.date: 05/07/2019
ms.custom: seodec18
ms.openlocfilehash: a7905213d67fd1cb97a8e50c938427b7ad775924
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/17/2019
ms.locfileid: "67165776"
---
# <a name="azure-time-series-insights-explorer"></a>Обозреватель службы "Аналитика временных рядов Azure"

В этой статье описаны функции и параметры доступности в целом для Azure Time Series Insights [веб-приложении обозревателя](https://insights.timeseries.azure.com/). В обозревателе Time Series Insights демонстрирует возможности визуализации данных, предоставляемых службой и может осуществляться в своей среде.

"Аналитика временных рядов Azure" — это полностью управляемая служба аналитики, хранения и визуализации, которая позволяет легко просматривать и анализировать миллиарды событий Интернета вещей одновременно. Она дает вам глобальное представление данных, позволяя быстро проверить решение Интернета вещей и избежать дорогостоящих простоев в работе критически важных устройств. Вы можете обнаруживать скрытые тенденции, аномалии и выполнять анализ их причин практически в реальном времени. Обозреватель службы "Аналитика временных рядов Azure" находится в общедоступной предварительной версии.

> [!TIP]
> Обзор демонстрационной среде, в статье [быстрого запуска Azure Time Series Insights](time-series-quickstart.md).

## <a name="video"></a>Видео

### <a name="learn-about-querying-data-by-using-the-time-series-insights-explorer-br"></a>Дополнительные сведения о запросе данных с помощью обозревателя Time Series Insights. </br>

> [!VIDEO https://www.youtube.com/embed/SHFPZvrR71s]

>[!NOTE]
>См. в предыдущем видео <a href="https://www.youtube.com/watch?v=6ehNf6AJkFo">«Приступая к работе с Time Series Insights с помощью ускоритель решения Интернета вещей Azure.»</a>

## <a name="prerequisites"></a>Технические условия

Прежде чем использовать обозреватель службы "Аналитика временных рядов Azure", необходимо сделать следующее:

- Создайте среду "Аналитика временных рядов". Дополнительные сведения см. в разделе [как приступить к работе с Time Series Insights](./time-series-insights-get-started.md).
- [Предоставить доступ](time-series-insights-data-access.md) свою учетную запись в среде.
- Добавить [центра Интернета вещей](time-series-insights-how-to-add-an-event-source-iothub.md) или [концентратора событий](time-series-insights-how-to-add-an-event-source-eventhub.md) источника события к нему.

## <a name="explore-and-query-data"></a>Просмотр и выполнение запроса данных

Практически сразу после подключения источника события к среде службы "Аналитика временных рядов" вы можете изучать и запрашивать данные временных рядов.

1. Чтобы начать, откройте [обозревателя Time Series Insights](https://insights.timeseries.azure.com/) в веб-браузере. В левой части окна выберите среду. Все среды, к которым имеется доступ, перечислены в алфавитном порядке.

1. После выбора среды, либо воспользоваться **из** и **для** конфигураций в верхней части, или щелкнуть и перетащить за период времени, вы хотите. Выберите лупы в правом верхнем углу или щелкните правой кнопкой мыши на выбранный период времени и выберите **поиска**.

1. Вы также можете обновить доступности автоматически каждую минуту, выбрав **автоматически на** кнопки. **Автоматически на** кнопку применяется только к диаграмме доступности, а не содержимое главной визуализации.

1. Значок облака Azure, чтобы перейти к вашей среде на портале Azure.

   [![Окружение анализа временных рядов](media/time-series-insights-explorer/explorer1.png)](media/time-series-insights-explorer/explorer1.png#lightbox)

1. После этого отобразится диаграмма, где показано количество всех событий за выбранный период времени. Здесь доступно несколько элементов управления.

    - **Панель редактора условий**: Пространство терминов, где вы запрашиваете среду. Он находится в левой части экрана:
      - **Мера**. Этот список раскрывающегося списка содержит все числовые столбцы (**дублеров**).
      - **Разделять по**. Этом раскрывающемся списке показаны категориальные столбцы (**строки**).
      - Можно включить пошаговую интерполяцию, отобразить минимальное и максимальное и изменить ось y с помощью панели управления рядом с полем **мер**. Вы также можете настроить тип отображаемых данных число, среднее или суммарное значение.
      - Вы можете добавить до пяти просматриваемых условий на одной оси x. Используйте **вниз копирования** кнопку, чтобы добавить дополнительное условие, или выберите **добавить** Чтобы добавить новое условие.

        [![Панель редактора условий](media/time-series-insights-explorer/explorer2.png)](media/time-series-insights-explorer/explorer2.png#lightbox)

      - **Предикат**. С помощью предиката к быстро фильтровать события с помощью набора операнд, перечисленных в следующей таблице. При выполнении поиска, выбрав или щелкнув, предикат автоматически обновлений на основе поиска. Поддерживаемые типы операнд:

         |Операция  |Поддерживаемые типы  |Примечания  |
         |---------|---------|---------|
         |`<`, `>`, `<=`, `>=`     |  Double, DateTime, TimeSpan       |         |
         |`=`, `!=`, `<>`     | String, Bool, Double, DateTime, TimeSpan, NULL        |         |
         |IN     | String, Bool, Double, DateTime, TimeSpan, NULL        |  Все операнды должны быть одного типа или являться константой NULL.        |
         |HAS     | String        |  На правой стороне разрешены только константы строковых литералов. Пустая строка и значение NULL не допускаются.       |

      - **Примеры запросов**

         [![Примеры запросов](media/time-series-insights-explorer/explorer9.png)](media/time-series-insights-explorer/explorer9.png#lightbox)

1. Можно использовать **размер интервала** ползунок для изменения масштаба и из него интервалов в одном промежутке. Ползунок обеспечивает более точное управление перемещением между большими срезами времени просматривать тенденции вниз, чтобы фрагменты как миллисекунды, позволяющих осуществлять детальный, с высоким разрешением порезов данных см. в разделе. Начальная точка ползунка по умолчанию будет задан наиболее оптимальный вид данных из вашего выбора для балансировки разрешение, скорость запросов и детализации.

1. **Время кисть** средство позволяет легко переходить от одного временного диапазона к другому.

1. Используйте **Сохранить** команду, чтобы сохранить текущий запрос и предоставить его другим пользователям среды. При использовании **откройте**, вы увидите все сохраненные запросы и другие общие запросы пользователей в тех средах, у вас есть доступ к.

   [![Запросы](media/time-series-insights-explorer/explorer3.png)](media/time-series-insights-explorer/explorer3.png#lightbox)

## <a name="visualize-data"></a>Визуализация данных

1. Используйте **Перспектива** средство для одновременно просматривать до четырех уникальных запросов. **Перспектива** кнопка находится в правом верхнем углу диаграммы.

   [![Перспектива](media/time-series-insights-explorer/explorer4.png)](media/time-series-insights-explorer/explorer4.png#lightbox)

1. Просмотр диаграммы для визуального анализа данных и использовать **диаграммы** средства:

    - **Выберите** или **щелкните** определенный период времени или только один ряд данных.
    - В составе timespan можно выбрать масштаб или просмотреть события.
    - В ряде данных вы можете разделить ряд по другому столбцу, добавить его в качестве нового условия, показать только выбранный ряд, исключить, закрепить ряд или просмотреть события из выбранного ряда.
    - В области фильтра слева от диаграммы можно просматривать все отображаемые ряды данных и переупорядочить, значение или имя. Вы также можете просмотреть все ряды данных или все закрепленные или открепленные ряды. Вы можно выбрать только один ряд данных и разделить ряд по другому столбцу, добавить его в качестве нового условия, Показать только выбранный ряд, исключить, закрепить ряд или просмотреть события из выбранного ряда.
    - При просмотре нескольких условий одновременно можно стека, разупорядочить, просмотреть дополнительные сведения о ряд данных и использовать одну ось y для всех условий. Используйте кнопки в правом верхнем углу диаграммы.

    [![Средство "Диаграмма"](media/time-series-insights-explorer/explorer5.png)](media/time-series-insights-explorer/explorer5.png#lightbox)

1. Используйте **тепловой карты** для быстрого выявления ряда уникальных или аномальных данных в запросе. Только один поисковой запрос можно визуализировать в виде тепловой карты.

    [![Тепловой карты](media/time-series-insights-explorer/explorer6.png)](media/time-series-insights-explorer/explorer6.png#lightbox)

1. При просмотре событий, выбрав или щелкнув правой кнопкой мыши, **события** панель становится доступной. Здесь можно просмотреть все необработанные события и экспортировать их как JSON- или CSV-файлы. Time Series Insights хранит все необработанные данные.

    [![События](media/time-series-insights-explorer/explorer7.png)](media/time-series-insights-explorer/explorer7.png#lightbox)

1. Выберите **STATS** вкладке после исследования события, чтобы предоставить статистику шаблонов и столбцов.

    - **Шаблоны**: Эта возможность заранее определяет наиболее статистически значимые шаблоны в выделенной области данных. Нет необходимости просматривать тысячи событий, чтобы понять, какие шаблоны требуют больше всего времени и энергии. С помощью Time Series Insights можно перейти непосредственно к этим статистически значимым шаблонам, чтобы продолжить проведение анализа. Эта возможность также полезна для последующего анализа журнала данных.
    - **Статистика столбца**. Статистика столбцов предоставляют диаграммы и таблицы, которые разбивают данные из каждого столбца выбранного ряда данных за выбранный период времени.

      [![СТАТИСТИКА](media/time-series-insights-explorer/explorer8.png)](media/time-series-insights-explorer/explorer8.png#lightbox)

Теперь вы видели различные функции и параметры, доступные в веб-приложении обозревателя Time Series Insights.

## <a name="next-steps"></a>Дальнейшие действия

- Узнайте, как [Диагностика и устранение неполадок](time-series-insights-diagnose-and-solve-problems.md) в вашей среде Time Series Insights.
- Воспользоваться интерактивная [быстрого запуска Azure Time Series Insights](time-series-quickstart.md) tour.
