---
title: Общие шаблоны запросов в Azure Stream Analytics
description: В этой статье приводятся некоторые общие шаблоны и структуры запросов, которые могут использоваться в заданиях Azure Stream Analytics.
services: stream-analytics
author: jseb225
ms.author: jeanb
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 05/16/2019
ms.openlocfilehash: 729385a2ce9feb6e69f9be29c2175b403093be3f
ms.sourcegitcommit: c556477e031f8f82022a8638ca2aec32e79f6fd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/23/2019
ms.locfileid: "68413360"
---
# <a name="query-examples-for-common-stream-analytics-usage-patterns"></a>Примеры запросов для распространенных шаблонов использования Stream Analytics

Запросы в Azure Stream Analytics выражаются на языке запросов на основе SQL. Эти языковые конструкции описаны в [справочнике по языку запросов Stream Analytics](/stream-analytics-query/stream-analytics-query-language-reference). 

Структура запросов может выражать простую логику передачи данных из одного входного потока в выходное хранилище данных, а также может выполнять обширное сопоставление шаблонов и временного анализа для вычисления статистических выражений в различных окнах времени, как в [сборке решения IOT. Использование Stream Analytics](stream-analytics-build-an-iot-solution-using-stream-analytics.md) Guide. Можно объединять данные из нескольких входов для объединения потоковых событий, а также выполнять поиск по статическим эталонным данным, чтобы расширить значения событий. Можно также записывать данные в несколько выходов.

В этой статье описаны решения для нескольких распространенных шаблонов запросов, основанных на реальных сценариях.

## <a name="work-with-complex-data-types-in-json-and-avro"></a>Работа со сложными типами данных в JSON и AVRO

Azure Stream Analytics поддерживает обработку событий в форматах CSV, JSON и Avro.

Данные JSON и Avro могут содержать сложные типы, такие как вложенные объекты (записи) или массивы. Дополнительные сведения о работе с этими сложными типами данных см. в статье [анализ данных JSON и Avro](stream-analytics-parsing-json.md) .

## <a name="query-example-convert-data-types"></a>Пример запроса. Преобразование типов данных

**Описание** Определите типы свойств для входного потока. Например, вес автомобиля поступает во входной поток в виде строк и должен быть преобразован в **тип int** для выполнения **Sum**.

**Входные данные**

| Убедитесь, | Time | Вес |
| --- | --- | --- |
| Honda |2015-01-01T00:00:01.0000000Z |"1000" |
| Honda |2015-01-01T00:00:02.0000000Z |"2000" |

**Выходные данные**:

| Убедитесь, | Вес |
| --- | --- |
| Honda |3000 |

**Решение**

```SQL
    SELECT
        Make,
        SUM(CAST(Weight AS BIGINT)) AS Weight
    FROM
        Input TIMESTAMP BY Time
    GROUP BY
        Make,
        TumblingWindow(second, 10)
```

**Объяснение**. Используйте инструкцию **CAST** в поле **Weight** для указания типа данных. Список поддерживаемых типов данных Azure Stream Analytics см. в [этой статье](/stream-analytics-query/data-types-azure-stream-analytics).

## <a name="query-example-use-likenot-like-to-do-pattern-matching"></a>Пример запроса. Использовать LIKE/не нравится для сопоставления шаблонов

**Описание** Проверьте, соответствует ли значение поля в событии определенному шаблону.
Например, проверьте, возвращает ли результат номерные знаки, которые начинаются с A и заканчиваться на 9.

**Входные данные**

| Убедитесь, | LicensePlate | Time |
| --- | --- | --- |
| Honda |ABC-123 |2015-01-01T00:00:01.0000000Z |
| Toyota |AAA-999 |2015-01-01T00:00:02.0000000Z |
| Nissan |ABC-369 |2015-01-01T00:00:03.0000000Z |

**Выходные данные**:

| Убедитесь, | LicensePlate | Time |
| --- | --- | --- |
| Toyota |AAA-999 |2015-01-01T00:00:02.0000000Z |
| Nissan |ABC-369 |2015-01-01T00:00:03.0000000Z |

**Решение**

```SQL
    SELECT
        *
    FROM
        Input TIMESTAMP BY Time
    WHERE
        LicensePlate LIKE 'A%9'
```

**Объяснение**. Используйте инструкцию **LIKE** для проверки значения поля **LicensePlate**. Оно должно начинаться с буквы A, затем содержать любую строку из нуля или более символов, а затем заканчиваться цифрой 9. 

## <a name="query-example-specify-logic-for-different-casesvalues-case-statements"></a>Пример запроса. Указание логики для различных случаев и значений (операторы CASE)

**Описание** Укажите разные вычисления для поля на основе определенных условий. Например, укажите строковое описание для количества переданных автомобилей одной и той же марки с особым условием для значения 1.

**Входные данные**

| Убедитесь, | Time |
| --- | --- |
| Honda |2015-01-01T00:00:01.0000000Z |
| Toyota |2015-01-01T00:00:02.0000000Z |
| Toyota |2015-01-01T00:00:03.0000000Z |

**Выходные данные**:

| CarsPassed | Time |
| --- | --- |
| 1 Honda |2015-01-01T00:00:10.0000000Z |
| 2 Toyota |2015-01-01T00:00:10.0000000Z |

**Решение**

```SQL
    SELECT
        CASE
            WHEN COUNT(*) = 1 THEN CONCAT('1 ', Make)
            ELSE CONCAT(CAST(COUNT(*) AS NVARCHAR(MAX)), ' ', Make, 's')
        END AS CarsPassed,
        System.TimeStamp() AS AsaTime
    FROM
        Input TIMESTAMP BY Time
    GROUP BY
        Make,
        TumblingWindow(second, 10)
```

**Объяснение**. Выражение **CASE** сравнивает выражение с набором простых выражений, чтобы определить их результат. В этом примере транспортное средство с количеством 1 вернуло отличное описание строки по сравнению с транспортным средством с другим количеством.

## <a name="query-example-send-data-to-multiple-outputs"></a>Пример запроса. Отправка данных на несколько выходов

**Описание** Отправьте данные в несколько целевых объектов выходных данных из одного задания. Например, проанализируйте данные для оповещения на основе пороговых значений и заархивируйте все события в хранилище BLOB-объектов.

**Входные данные**

| Убедитесь, | Time |
| --- | --- |
| Honda |2015-01-01T00:00:01.0000000Z |
| Honda |2015-01-01T00:00:02.0000000Z |
| Toyota |2015-01-01T00:00:01.0000000Z |
| Toyota |2015-01-01T00:00:02.0000000Z |
| Toyota |2015-01-01T00:00:03.0000000Z |

**Выходные данные 1**:

| Убедитесь, | Time |
| --- | --- |
| Honda |2015-01-01T00:00:01.0000000Z |
| Honda |2015-01-01T00:00:02.0000000Z |
| Toyota |2015-01-01T00:00:01.0000000Z |
| Toyota |2015-01-01T00:00:02.0000000Z |
| Toyota |2015-01-01T00:00:03.0000000Z |

**Выходные данные 2**:

| Убедитесь, | Time | Count |
| --- | --- | --- |
| Toyota |2015-01-01T00:00:10.0000000Z |3 |

**Решение**

```SQL
    SELECT
        *
    INTO
        ArchiveOutput
    FROM
        Input TIMESTAMP BY Time

    SELECT
        Make,
        System.TimeStamp() AS AsaTime,
        COUNT(*) AS [Count]
    INTO
        AlertOutput
    FROM
        Input TIMESTAMP BY Time
    GROUP BY
        Make,
        TumblingWindow(second, 10)
    HAVING
        [Count] >= 3
```

**Объяснение**. Выражение **INTO** сообщает Stream Analytics, в какие выходные данные записывать данные из этого оператора. Первый запрос — это транзитная часть данных, полученных на выход с именем **ArchiveOutput**. Второй запрос выполняет некоторую простую статистическую обработку и фильтрацию и отправляет результаты в нисходящий систему предупреждений, **алертаутпут**.

Обратите внимание, что можно повторно использовать результаты обобщенных табличных выражений (таких как инструкции **WITH**) в нескольких выходных инструкциях. При этом вы получаете дополнительное преимущество, так как нужно открывать меньше читателей в источнике входных данных.

Пример: 

```SQL
    WITH AllRedCars AS (
        SELECT
            *
        FROM
            Input TIMESTAMP BY Time
        WHERE
            Color = 'red'
    )
    SELECT * INTO HondaOutput FROM AllRedCars WHERE Make = 'Honda'
    SELECT * INTO ToyotaOutput FROM AllRedCars WHERE Make = 'Toyota'
```

## <a name="query-example-count-unique-values"></a>Пример запроса. Число уникальных значений

**Описание** Подсчитайте количество уникальных значений поля, которые отображаются в потоке в течение определенного интервала. Например, сколько уникальных марок автомобилей проходят через пункт взимания дорожного сбора за 2 секунды?

**Входные данные**

| Убедитесь, | Time |
| --- | --- |
| Honda |2015-01-01T00:00:01.0000000Z |
| Honda |2015-01-01T00:00:02.0000000Z |
| Toyota |2015-01-01T00:00:01.0000000Z |
| Toyota |2015-01-01T00:00:02.0000000Z |
| Toyota |2015-01-01T00:00:03.0000000Z |

**Выходные данные:**

| CountMake | Time |
| --- | --- |
| 2 |2015-01-01T00:00:02.000Z |
| 1 |2015-01-01T00:00:04.000Z |

**Решение.**

```SQL
SELECT
     COUNT(DISTINCT Make) AS CountMake,
     System.TIMESTAMP() AS AsaTIME
FROM Input TIMESTAMP BY TIME
GROUP BY 
     TumblingWindow(second, 2)
```


**Объяснение.** 
**COUNT(DISTINCT Make)** возвращает количество уникальных значений в столбце **Марка** в течение определенного интервала.

## <a name="query-example-determine-if-a-value-has-changed"></a>Пример запроса. Определение изменения значения

**Описание** Изучите предыдущее значение, чтобы определить, отличается ли оно от текущего. Например, предыдущий автомобиль на платной дороге той же марки, что и текущий автомобиль?

**Входные данные**

| Убедитесь, | Time |
| --- | --- |
| Honda |2015-01-01T00:00:01.0000000Z |
| Toyota |2015-01-01T00:00:02.0000000Z |

**Выходные данные**:

| Убедитесь, | Time |
| --- | --- |
| Toyota |2015-01-01T00:00:02.0000000Z |

**Решение**

```SQL
    SELECT
        Make,
        Time
    FROM
        Input TIMESTAMP BY Time
    WHERE
        LAG(Make, 1) OVER (LIMIT DURATION(minute, 1)) <> Make
```

**Объяснение**. Используйте **LAG**, чтобы заглянуть в поток входных данных на одно событие назад и получить значение **Make**. Затем сравните его со значением **Make** в текущем событии и поместите событие в выходные данные, если они отличаются.

## <a name="query-example-find-the-first-event-in-a-window"></a>Пример запроса. Поиск первого события за период

**Описание** Найдите первый автомобиль за каждые 10 минут.

**Входные данные**

| LicensePlate | Убедитесь, | Time |
| --- | --- | --- |
| DXE 5291 |Honda |2015-07-27T00:00:05.0000000Z |
| YZK 5704 |Ford |2015-07-27T00:02:17.0000000Z |
| RMV 8282 |Honda |2015-07-27T00:05:01.0000000Z |
| YHN 6970 |Toyota |2015-07-27T00:06:00.0000000Z |
| VFE 1616 |Toyota |2015-07-27T00:09:31.0000000Z |
| QYF 9358 |Honda |2015-07-27T00:12:02.0000000Z |
| MDR 6128 |BMW |2015-07-27T00:13:45.0000000Z |

**Выходные данные**:

| LicensePlate | Убедитесь, | Time |
| --- | --- | --- |
| DXE 5291 |Honda |2015-07-27T00:00:05.0000000Z |
| QYF 9358 |Honda |2015-07-27T00:12:02.0000000Z |

**Решение**

```SQL
    SELECT 
        LicensePlate,
        Make,
        Time
    FROM 
        Input TIMESTAMP BY Time
    WHERE 
        IsFirst(minute, 10) = 1
```

Теперь давайте изменим проблему и найдем первый автомобиль определенного дела каждые 10 минут.

| LicensePlate | Убедитесь, | Time |
| --- | --- | --- |
| DXE 5291 |Honda |2015-07-27T00:00:05.0000000Z |
| YZK 5704 |Ford |2015-07-27T00:02:17.0000000Z |
| YHN 6970 |Toyota |2015-07-27T00:06:00.0000000Z |
| QYF 9358 |Honda |2015-07-27T00:12:02.0000000Z |
| MDR 6128 |BMW |2015-07-27T00:13:45.0000000Z |

**Решение**

```SQL
    SELECT 
        LicensePlate,
        Make,
        Time
    FROM 
        Input TIMESTAMP BY Time
    WHERE 
        IsFirst(minute, 10) OVER (PARTITION BY Make) = 1
```

## <a name="query-example-find-the-last-event-in-a-window"></a>Пример запроса. Поиск последнего события за период

**Описание** Найдите последний автомобиль за каждые 10 минут.

**Входные данные**

| LicensePlate | Убедитесь, | Time |
| --- | --- | --- |
| DXE 5291 |Honda |2015-07-27T00:00:05.0000000Z |
| YZK 5704 |Ford |2015-07-27T00:02:17.0000000Z |
| RMV 8282 |Honda |2015-07-27T00:05:01.0000000Z |
| YHN 6970 |Toyota |2015-07-27T00:06:00.0000000Z |
| VFE 1616 |Toyota |2015-07-27T00:09:31.0000000Z |
| QYF 9358 |Honda |2015-07-27T00:12:02.0000000Z |
| MDR 6128 |BMW |2015-07-27T00:13:45.0000000Z |

**Выходные данные**:

| LicensePlate | Убедитесь, | Time |
| --- | --- | --- |
| VFE 1616 |Toyota |2015-07-27T00:09:31.0000000Z |
| MDR 6128 |BMW |2015-07-27T00:13:45.0000000Z |

**Решение**

```SQL
    WITH LastInWindow AS
    (
        SELECT 
            MAX(Time) AS LastEventTime
        FROM 
            Input TIMESTAMP BY Time
        GROUP BY 
            TumblingWindow(minute, 10)
    )
    SELECT 
        Input.LicensePlate,
        Input.Make,
        Input.Time
    FROM
        Input TIMESTAMP BY Time 
        INNER JOIN LastInWindow
        ON DATEDIFF(minute, Input, LastInWindow) BETWEEN 0 AND 10
        AND Input.Time = LastInWindow.LastEventTime
```

**Объяснение**. Запрос состоит из двух этапов. На первом выполняется поиск последней метки времени в 10-минутном окне, а на втором — объединение результатов первого запроса с исходным потоком, чтобы найти события, соответствующие последней метке времени в каждом окне. 

## <a name="query-example-locate-correlated-events-in-a-stream"></a>Пример запроса. Обнаружение коррелированных событий в потоке

**Описание** Найти коррелированные события в потоке. Например, два автомобиля одной и той же марки подряд въезжают на платную дорогу за последние 90 секунд.

**Входные данные**

| Убедитесь, | LicensePlate | Time |
| --- | --- | --- |
| Honda |ABC-123 |2015-01-01T00:00:01.0000000Z |
| Honda |AAA-999 |2015-01-01T00:00:02.0000000Z |
| Toyota |DEF-987 |2015-01-01T00:00:03.0000000Z |
| Honda |GHI-345 |2015-01-01T00:00:04.0000000Z |

**Выходные данные**:

| Убедитесь, | Time | CurrentCarLicensePlate | FirstCarLicensePlate | FirstCarTime |
| --- | --- | --- | --- | --- |
| Honda |2015-01-01T00:00:02.0000000Z |AAA-999 |ABC-123 |2015-01-01T00:00:01.0000000Z |

**Решение**

```SQL
    SELECT
        Make,
        Time,
        LicensePlate AS CurrentCarLicensePlate,
        LAG(LicensePlate, 1) OVER (LIMIT DURATION(second, 90)) AS FirstCarLicensePlate,
        LAG(Time, 1) OVER (LIMIT DURATION(second, 90)) AS FirstCarTime
    FROM
        Input TIMESTAMP BY Time
    WHERE
        LAG(Make, 1) OVER (LIMIT DURATION(second, 90)) = Make
```

**Объяснение**. Используйте **LAG**, чтобы заглянуть в поток входных данных на одно событие назад и получить значение **Make**. Сравните его со значением **MAKE** в текущем событии и поместите событие в выходные данные, если они совпадают. **LAG** также можно использовать для получения данных о предыдущем автомобиле.

## <a name="query-example-detect-the-duration-between-events"></a>Пример запроса. Определение промежутка между событиями

**Описание** Найдите длительность заданного события. Например, на основе сведений о посещениях веб-сайта определите время, затраченное на использование функции.

**Входные данные**  

| пользователя | Компонент | Событие | Time |
| --- | --- | --- | --- |
| user@location.com |RightMenu |Начать |2015-01-01T00:00:01.0000000Z |
| user@location.com |RightMenu |Конец |2015-01-01T00:00:08.0000000Z |

**Выходные данные**:  

| пользователя | Компонент | Продолжительность |
| --- | --- | --- |
| user@location.com |RightMenu |7 |

**Решение**

```SQL
    SELECT
        [user],
    feature,
    DATEDIFF(
        second,
        LAST(Time) OVER (PARTITION BY [user], feature LIMIT DURATION(hour, 1) WHEN Event = 'start'),
        Time) as duration
    FROM input TIMESTAMP BY Time
    WHERE
        Event = 'end'
```

**Объяснение**. Функция **LAST** позволяет получить последнее значение времени **TIME** для события типа **Start**. Для обозначения того, что результат будет вычисляться для каждого уникального пользователя, функция **LAST** включает параметр **PARTITION BY [user]** . Максимальный промежуток между событиями **Start** и **Stop** в запросе составляет 1 час, но при необходимости это значение можно изменить **(LIMIT DURATION(hour, 1)** .

## <a name="query-example-detect-the-duration-of-a-condition"></a>Пример запроса. Определение продолжительности условия
**Описание** Определите продолжительность условия.
Предположим, произошла ошибка, которая привела к неправильному отображению массы всех автомобилей (больше 20 000 фунтов). Необходимо вычислить длительность ошибки.

**Входные данные**

| Убедитесь, | Time | Вес |
| --- | --- | --- |
| Honda |2015-01-01T00:00:01.0000000Z |2000 |
| Toyota |2015-01-01T00:00:02.0000000Z |25000 |
| Honda |2015-01-01T00:00:03.0000000Z |26000 |
| Toyota |2015-01-01T00:00:04.0000000Z |25000 |
| Honda |2015-01-01T00:00:05.0000000Z |26000 |
| Toyota |2015-01-01T00:00:06.0000000Z |25000 |
| Honda |2015-01-01T00:00:07.0000000Z |26000 |
| Toyota |2015-01-01T00:00:08.0000000Z |2000 |

**Выходные данные**:

| StartFault | EndFault |
| --- | --- |
| 2015-01-01T00:00:02.000Z |2015-01-01T00:00:07.000Z |

**Решение**

```SQL
    WITH SelectPreviousEvent AS
    (
    SELECT
    *,
        LAG([time]) OVER (LIMIT DURATION(hour, 24)) as previousTime,
        LAG([weight]) OVER (LIMIT DURATION(hour, 24)) as previousWeight
    FROM input TIMESTAMP BY [time]
    )

    SELECT 
        LAG(time) OVER (LIMIT DURATION(hour, 24) WHEN previousWeight < 20000 ) [StartFault],
        previousTime [EndFault]
    FROM SelectPreviousEvent
    WHERE
        [weight] < 20000
        AND previousWeight > 20000
```

**Объяснение**. Функция **LAG** позволяет просмотреть входной поток данных за 24-часовой период и найти экземпляры, в которых **StartFault** и **StopFault** связаны весом менее 20 000.

## <a name="query-example-fill-missing-values"></a>Пример запроса. Заполнение отсутствующих значений

**Описание** Для потока событий с отсутствующими значениями создайте поток событий с регулярными интервалами. Например, создавайте каждые 5 секунд событие, сообщающее последнюю видимую точку данных.

**Входные данные**

| t | value |
| --- | --- |
| "2014-01-01T06:01:00" |1 |
| "2014-01-01T06:01:05" |2 |
| "2014-01-01T06:01:10" |3 |
| "2014-01-01T06:01:15" |4 |
| "2014-01-01T06:01:30" |5 |
| "2014-01-01T06:01:35" |6 |

**Выходные данные (первые 10 строк)** :

| windowend | lastevent.t | lastevent.value |
| --- | --- | --- |
| 2014-01-01T14:01:00.000Z |2014-01-01T14:01:00.000Z |1 |
| 2014-01-01T14:01:05.000Z |2014-01-01T14:01:05.000Z |2 |
| 2014-01-01T14:01:10.000Z |2014-01-01T14:01:10.000Z |3 |
| 2014-01-01T14:01:15.000Z |2014-01-01T14:01:15.000Z |4 |
| 2014-01-01T14:01:20.000Z |2014-01-01T14:01:15.000Z |4 |
| 2014-01-01T14:01:25.000Z |2014-01-01T14:01:15.000Z |4 |
| 2014-01-01T14:01:30.000Z |2014-01-01T14:01:30.000Z |5 |
| 2014-01-01T14:01:35.000Z |2014-01-01T14:01:35.000Z |6 |
| 2014-01-01T14:01:40.000Z |2014-01-01T14:01:35.000Z |6 |
| 2014-01-01T14:01:45.000Z |2014-01-01T14:01:35.000Z |6 |

**Решение**

```SQL
    SELECT
        System.Timestamp() AS windowEnd,
        TopOne() OVER (ORDER BY t DESC) AS lastEvent
    FROM
        input TIMESTAMP BY t
    GROUP BY HOPPINGWINDOW(second, 300, 5)
```

**Объяснение**. Этот запрос создает события каждые 5 секунд и выводит последнее событие, полученное ранее. Длительность [окна прыгающее»](/stream-analytics-query/hopping-window-azure-stream-analytics) определяет, на какое время будет выглядеть запрос, чтобы найти Последнее событие (300 секунд в этом примере).


## <a name="query-example-correlate-two-event-types-within-the-same-stream"></a>Пример запроса. Корреляция двух типов событий в одном потоке

**Описание** Иногда требуется создавать оповещения, которые основаны на нескольких типах событий, произошедших в определенный диапазон времени. Например, рассмотрим сценарий Центра Интернета вещей для домашних печей. Предположим, нам нужно создать оповещение, когда температура вентилятора опускается ниже 40 и максимальная мощность за последние 3 минуты не превышает 10.

**Входные данные**

| time | deviceId | sensorName | value |
| --- | --- | --- | --- |
| "2018-01-01T16:01:00" | "Oven1" (Печь 1) | "temp" (температура) |120 |
| "2018-01-01T16:01:00" | "Oven1" (Печь 1) | "power" (питание) |15 |
| "2018-01-01T16:02:00" | "Oven1" (Печь 1) | "temp" (температура) |100 |
| "2018-01-01T16:02:00" | "Oven1" (Печь 1) | "power" (питание) |15 |
| "2018-01-01T16:03:00" | "Oven1" (Печь 1) | "temp" (температура) |70 |
| "2018-01-01T16:03:00" | "Oven1" (Печь 1) | "power" (питание) |15 |
| "2018-01-01T16:04:00" | "Oven1" (Печь 1) | "temp" (температура) |50 |
| "2018-01-01T16:04:00" | "Oven1" (Печь 1) | "power" (питание) |15 |
| "2018-01-01T16:05:00" | "Oven1" (Печь 1) | "temp" (температура) |30 |
| "2018-01-01T16:05:00" | "Oven1" (Печь 1) | "power" (питание) |8 |
| "2018-01-01T16:06:00" | "Oven1" (Печь 1) | "temp" (температура) |20 |
| "2018-01-01T16:06:00" | "Oven1" (Печь 1) | "power" (питание) |8 |
| "2018-01-01T16:07:00" | "Oven1" (Печь 1) | "temp" (температура) |20 |
| "2018-01-01T16:07:00" | "Oven1" (Печь 1) | "power" (питание) |8 |
| "2018-01-01T16:08:00" | "Oven1" (Печь 1) | "temp" (температура) |20 |
| "2018-01-01T16:08:00" | "Oven1" (Печь 1) | "power" (питание) |8 |

**Выходные данные**:

| eventTime | deviceId | temp | alertMessage | maxPowerDuringLast3mins |
| --- | --- | --- | --- | --- | 
| "2018-01-01T16:05:00" | "Oven1" (Печь 1) |30 | "Short circuit heating elements" (Короткое замыкание нагревательных элементов) |15 |
| "2018-01-01T16:06:00" | "Oven1" (Печь 1) |20 | "Short circuit heating elements" (Короткое замыкание нагревательных элементов) |15 |
| "2018-01-01T16:07:00" | "Oven1" (Печь 1) |20 | "Short circuit heating elements" (Короткое замыкание нагревательных элементов) |15 |

**Решение**

```SQL
WITH max_power_during_last_3_mins AS (
    SELECT 
        System.TimeStamp() AS windowTime,
        deviceId,
        max(value) as maxPower
    FROM
        input TIMESTAMP BY t
    WHERE 
        sensorName = 'power' 
    GROUP BY 
        deviceId, 
        SlidingWindow(minute, 3) 
)

SELECT 
    t1.t AS eventTime,
    t1.deviceId, 
    t1.value AS temp,
    'Short circuit heating elements' as alertMessage,
    t2.maxPower AS maxPowerDuringLast3mins
    
INTO resultsr

FROM input t1 TIMESTAMP BY t
JOIN max_power_during_last_3_mins t2
    ON t1.deviceId = t2.deviceId 
    AND t1.t = t2.windowTime
    AND DATEDIFF(minute,t1,t2) between 0 and 3
    
WHERE
    t1.sensorName = 'temp'
    AND t1.value <= 40
    AND t2.maxPower > 10
```

**Объяснение**. В первом запросе `max_power_during_last_3_mins` используется [скользящее окно](/stream-analytics-query/sliding-window-azure-stream-analytics), чтобы найти максимальное значение датчика питания для каждого устройства за последние 3 минуты. Второй запрос объединяется с первым запросом, чтобы найти значение питания в последнем окне, которое связано с текущим событием. Затем, если условия выполняются, создается оповещение для устройства.

## <a name="query-example-process-events-independent-of-device-clock-skew-substreams"></a>Пример запроса. Обработка событий, независимо от рассинхронизации часов устройства (подпотоки)

**Описание** События могут поступать с опозданием или не по порядку из-за рассинхронизации часов поставщиков событий, секций или сетевой задержки. В следующем примере часы устройства для TollID 2 представляют собой пять секунд после TollID 1, а часы устройства для TollID 3 — десять секунд после TollID 1. 

**Входные данные**

| LicensePlate | Убедитесь, | Time | ИД пункта сбора |
| --- | --- | --- | --- |
| DXE 5291 |Honda |2015-07-27T00:00:01.0000000Z | 1 |
| YHN 6970 |Toyota |2015-07-27T00:00:05.0000000Z | 1 |
| QYF 9358 |Honda |2015-07-27T00:00:01.0000000Z | 2 |
| GXF 9462 |BMW |2015-07-27T00:00:04.0000000Z | 2 |
| VFE 1616 |Toyota |2015-07-27T00:00:10.0000000Z | 1 |
| RMV 8282 |Honda |2015-07-27T00:00:03.0000000Z | 3 |
| MDR 6128 |BMW |2015-07-27T00:00:11.0000000Z | 2 |
| YZK 5704 |Ford |2015-07-27T00:00:07.0000000Z | 3 |

**Выходные данные**:

| ИД пункта сбора | Count |
| --- | --- |
| 1 | 2 |
| 2 | 2 |
| 1 | 1 |
| 3 | 1 |
| 2 | 1 |
| 3 | 1 |

**Решение**

```SQL
SELECT
      TollId,
      COUNT(*) AS Count
FROM input
      TIMESTAMP BY Time OVER TollId
GROUP BY TUMBLINGWINDOW(second, 5), TollId
```

**Объяснение**. Предложение [TIMESTAMP BY OVER](/stream-analytics-query/timestamp-by-azure-stream-analytics#over-clause-interacts-with-event-ordering) просматривает временную шкалу каждого устройства отдельно с помощью подпотоков. Исходящие события для каждого TollID создаются по мере их вычисления. Это означает, что события отображаются по порядку по отношению к каждому TollID, а не переупорядочиваются, как если бы часы всех устройств были синхронизированы.

## <a name="query-example-remove-duplicate-events-in-a-window"></a>Пример запроса. Удаление повторяющихся событий за период

**Описание** При выполнении операции, такой как вычисление средних значений событий за заданный период времени, повторяющиеся события должны быть отфильтрованы. В следующем примере второе событие является дубликатом первого.

**Входные данные**  

| Идентификатор устройства | Time | Атрибут | Значение |
| --- | --- | --- | --- |
| 1 |2018-07-27T00:00:01.0000000Z |Температура |50 |
| 1 |2018-07-27T00:00:01.0000000Z |Температура |50 |
| 2 |2018-07-27T00:00:01.0000000Z |Температура |40 |
| 1 |2018-07-27T00:00:05.0000000Z |Температура |60 |
| 2 |2018-07-27T00:00:05.0000000Z |Температура |50 |
| 1 |2018-07-27T00:00:10.0000000Z |Температура |100 |

**Выходные данные**:  

| AverageValue | Идентификатор устройства |
| --- | --- |
| 70 | 1 |
|45 | 2 |

**Решение**

```SQL
With Temp AS (
    SELECT
        COUNT(DISTINCT Time) AS CountTime,
        Value,
        DeviceId
    FROM
        Input TIMESTAMP BY Time
    GROUP BY
        Value,
        DeviceId,
        SYSTEM.TIMESTAMP()
)

SELECT
    AVG(Value) AS AverageValue, DeviceId
INTO Output
FROM Temp
GROUP BY DeviceId,TumblingWindow(minute, 5)
```

**Объяснение**. [COUNT(DISTINCT Time)](/stream-analytics-query/count-azure-stream-analytics) возвращает количество уникальных значений в столбце "Время" в течение определенного интервала. Затем можно использовать выходные данные этого действия для вычисления среднего для каждого устройства путем удаления дубликатов.

## <a name="geofencing-and-geospatial-queries"></a>Геоограждение и геопространственные запросы
Azure Stream Analytics предоставляет встроенные геопространственные функции, которые можно использовать для реализации таких сценариев, как управление препарком, перестроение общего доступа, подключенные автомобили и отслеживание ресурсов. Геопространственные данные могут быть приняты в формате геоjson или WKT в составе потока событий или ссылочных данных. Дополнительные сведения см. в разделе [сценарии использования геопространственной и геопространственных статистических схем с Azure Stream Analyticsной](geospatial-scenarios.md) статьей.

## <a name="language-extensibility-through-javascript-and-c"></a>Расширяемость языков с помощью JavaScript иC#
Ананлитикс запросов Azure Stream лангугае можно расширить с помощью пользовательских функций, написанных на C# JavaScript или языках. Дополнительные сведения см. в статьях будут:
* [Azure Stream Analytics определяемые пользователем функции JavaScript](stream-analytics-javascript-user-defined-functions.md)
* [Azure Stream Analytics определяемые пользователем статистические функции JavaScript](stream-analytics-javascript-user-defined-aggregates.md)
* [Разработка .NET Standard пользовательских функций для Azure Stream Analyticsных заданий пограничных устройств](stream-analytics-edge-csharp-udf-methods.md)

## <a name="get-help"></a>Справка

За дополнительной помощью обращайтесь на наш [форум Azure Stream Analytics](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureStreamAnalytics).

## <a name="next-steps"></a>Дальнейшие действия
* [Введение в Azure Stream Analytics](stream-analytics-introduction.md)
* [Приступая к работе с Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Масштабирование заданий в службе Azure Stream Analytics](stream-analytics-scale-jobs.md)
* [Справочник по языку запросов Azure Stream Analytics](https://docs.microsoft.com/stream-analytics-query/stream-analytics-query-language-reference)
* [Справочник по API-интерфейсу REST управления Stream Analytics](https://msdn.microsoft.com/library/azure/dn835031.aspx)

