---
title: Начало работы с языком U-SQL в Azure Data Lake Analytics
description: Ознакомьтесь с основами работы с языком U-SQL в Azure Data Lake Analytics.
services: data-lake-analytics
author: saveenr
ms.author: saveenr
ms.reviewer: jasonwhowell
ms.assetid: 57143396-ab86-47dd-b6f8-613ba28c28d2
ms.service: data-lake-analytics
ms.topic: conceptual
ms.date: 06/23/2017
ms.openlocfilehash: 2a138801ba13c6008880e3d24c89d1c23323b853
ms.sourcegitcommit: c0419208061b2b5579f6e16f78d9d45513bb7bbc
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/08/2019
ms.locfileid: "67626217"
---
# <a name="get-started-with-u-sql-in-azure-data-lake-analytics"></a>Начало работы с языком U-SQL в Azure Data Lake Analytics
U-SQL — это язык, который объединяет декларативный SQL и императивный C#, позволяя обрабатывать данные любого масштаба. Масштабируемые распределенные запросы U-SQL позволяют эффективно анализировать данные в таких реляционных хранилищах, как база данных SQL Azure. С помощью U-SQL вы можете обрабатывать неструктурированные данные, применяя схему к пользовательской логике чтения и вставки, а также определяемым пользователем функциям. Кроме того, U-SQL содержит расширения, которые помогают разработчику точно управлять способом выполнения в нужном масштабе. 

## <a name="learning-resources"></a>Учебные материалы

* В [учебнике по U-SQL](https://aka.ms/usqltutorial) приводятся пошаговые инструкции по выполнению большинства операций с языком U-SQL. Этот документ рекомендуется к ознакомлению всем разработчикам, которые хотят освоить U-SQL.
* Дополнительные сведения о **синтаксисе языка U-SQL** см. в [справочнике по языку U-SQL](https://docs.microsoft.com/u-sql/).
* Чтобы понять **принципы разработки на U-SQL**, см. запись блога Visual Studio, посвященную [использованию языка U-SQL для простой обработки больших данных](https://blogs.msdn.microsoft.com/visualstudio/2015/09/28/introducing-u-sql-a-language-that-makes-big-data-processing-easy/).

## <a name="prerequisites"></a>предварительные требования

Прежде чем переходить к примерам U-SQL в этом документе, прочтите и выполните [руководства: Разработка скриптов U-SQL, с помощью средств Data Lake для Visual Studio](data-lake-analytics-data-lake-tools-get-started.md). Это руководство описывает механизм использования U-SQL с помощью средств Azure Data Lake для Visual Studio.

## <a name="your-first-u-sql-script"></a>Первый скрипт U-SQL

Ниже приведен очень простой скрипт U-SQL, который помогает понять различные аспекты языка U-SQL.

```
@searchlog =
    EXTRACT UserId          int,
            Start           DateTime,
            Region          string,
            Query           string,
            Duration        int?,
            Urls            string,
            ClickedUrls     string
    FROM "/Samples/Data/SearchLog.tsv"
    USING Extractors.Tsv();

OUTPUT @searchlog   
    TO "/output/SearchLog-first-u-sql.csv"
    USING Outputters.Csv();
```

Этот сценарий не содержит действий преобразования. Он считывает данные из исходного файла `SearchLog.tsv`, схематизирует их, а затем записывает набор строк в файл с именем SearchLog-first-u-sql.csv.

Обратите внимание на вопросительный знак рядом с типом данных в поле `Duration`. Это означает, что поле `Duration` может иметь значение NULL.

### <a name="key-concepts"></a>Основные понятия
* **Переменные набора строк**: Каждое выражение запроса, которое возвращает набор строк можно назначить переменной. U-SQL использует в сценарии шаблон именования переменных T-SQL (пример: `@searchlog`).
* Оператор **EXTRACT** считывает данные из файла и определяет схему для чтения. `Extractors.Tsv` — это встроенное средство извлечения U-SQL для файлов со значениями с разделением знаками табуляции. Можно разрабатывать пользовательские средства извлечения.
* Оператор **OUTPUT** записывает данные из набора строк в файл. `Outputters.Csv()` — это встроенное средство вывода данных U-SQL для файлов данных с разделителями-запятыми. Вы можете разработать собственные средства вывода.

### <a name="file-paths"></a>Пути к файлам

Операторы EXTRACT и OUTPUT используют пути к файлам. Пути к файлам могут быть абсолютными или относительными.

Следующий абсолютный путь к файлу ссылается на файл `mystore` в Data Lake Store:

    adl://mystore.azuredatalakestore.net/Samples/Data/SearchLog.tsv

Следующий относительный путь к файлу начинается с `"/"`. Он указывает на файл в учетной записи Data Lake Store по умолчанию:

    /output/SearchLog-first-u-sql.csv

## <a name="use-scalar-variables"></a>Использование скалярных переменных

Скалярные переменные можно использовать также для упрощения обслуживания вашего сценария. Предыдущий скрипт U-SQL также можно записать так:

    DECLARE @in  string = "/Samples/Data/SearchLog.tsv";
    DECLARE @out string = "/output/SearchLog-scalar-variables.csv";

    @searchlog =
        EXTRACT UserId          int,
                Start           DateTime,
                Region          string,
                Query           string,
                Duration        int?,
                Urls            string,
                ClickedUrls     string
        FROM @in
        USING Extractors.Tsv();

    OUTPUT @searchlog   
        TO @out
        USING Outputters.Csv();

## <a name="transform-rowsets"></a>Преобразования наборов строк

Используйте **SELECT** для преобразования наборов строк:

    @searchlog =
        EXTRACT UserId          int,
                Start           DateTime,
                Region          string,
                Query           string,
                Duration        int?,
                Urls            string,
                ClickedUrls     string
        FROM "/Samples/Data/SearchLog.tsv"
        USING Extractors.Tsv();

    @rs1 =
        SELECT Start, Region, Duration
        FROM @searchlog
    WHERE Region == "en-gb";

    OUTPUT @rs1   
        TO "/output/SearchLog-transform-rowsets.csv"
        USING Outputters.Csv();

Предложение WHERE использует [логическое выражение C#](/dotnet/csharp/language-reference/operators/index). Для создания собственных выражений и функций можно использовать язык выражений C#. Можно даже выполнить более сложную фильтрацию, сочетая их с помощью логического объединения (and) и разделения (or).

Следующий сценарий использует метод DateTime.Parse() и объединение.

    @searchlog =
        EXTRACT UserId          int,
                Start           DateTime,
                Region          string,
                Query           string,
                Duration        int?,
                Urls            string,
                ClickedUrls     string
        FROM "/Samples/Data/SearchLog.tsv"
        USING Extractors.Tsv();

    @rs1 =
        SELECT Start, Region, Duration
        FROM @searchlog
    WHERE Region == "en-gb";

    @rs1 =
        SELECT Start, Region, Duration
        FROM @rs1
        WHERE Start >= DateTime.Parse("2012/02/16") AND Start <= DateTime.Parse("2012/02/17");

    OUTPUT @rs1   
        TO "/output/SearchLog-transform-datetime.csv"
        USING Outputters.Csv();

 >[!NOTE]
 >Второй запрос выполняется на основе результатов первого набора строк, объединяя таким образом два фильтра. Можно также повторно использовать имя переменной. Имена объединяются лексически.

## <a name="aggregate-rowsets"></a>Агрегированные наборы строк
U-SQL поддерживает знакомые предложения ORDER BY, GROUP BY и агрегаты.

Следующий запрос определяет общую длительность по регионам, а затем последовательно выводит пять результатов со значениями максимальной длительности.

Наборы строк U-SQL не сохраняют свой порядок для следующего запроса. Таким образом, чтобы упорядочить выходные данные, необходимо добавить предложение ORDER BY в предложение OUTPUT.

    DECLARE @outpref string = "/output/Searchlog-aggregation";
    DECLARE @out1    string = @outpref+"_agg.csv";
    DECLARE @out2    string = @outpref+"_top5agg.csv";

    @searchlog =
        EXTRACT UserId          int,
                Start           DateTime,
                Region          string,
                Query           string,
                Duration        int?,
                Urls            string,
                ClickedUrls     string
        FROM "/Samples/Data/SearchLog.tsv"
        USING Extractors.Tsv();

    @rs1 =
        SELECT
            Region,
            SUM(Duration) AS TotalDuration
        FROM @searchlog
    GROUP BY Region;

    @res =
        SELECT *
        FROM @rs1
        ORDER BY TotalDuration DESC
        FETCH 5 ROWS;

    OUTPUT @rs1
        TO @out1
        ORDER BY TotalDuration DESC
        USING Outputters.Csv();

    OUTPUT @res
        TO @out2
        ORDER BY TotalDuration DESC
        USING Outputters.Csv();

Предложение ORDER BY в U-SQL необходимо использовать вместе с предложением FETCH в выражении SELECT.

Предложение HAVING в U-SQL можно использовать для ограничения выходных данных группами, которые удовлетворяют условиям HAVING:

    @searchlog =
        EXTRACT UserId          int,
                Start           DateTime,
                Region          string,
                Query           string,
                Duration        int?,
                Urls            string,
                ClickedUrls     string
        FROM "/Samples/Data/SearchLog.tsv"
        USING Extractors.Tsv();

    @res =
        SELECT
            Region,
            SUM(Duration) AS TotalDuration
        FROM @searchlog
        GROUP BY Region
        HAVING SUM(Duration) > 200;

    OUTPUT @res
        TO "/output/Searchlog-having.csv"
        ORDER BY TotalDuration DESC
        USING Outputters.Csv();

Сведения о расширенных сценариях агрегирования см. в справочной документации по [статистическим, аналитическим и ссылочным функциям](/u-sql/built-in-functions) в U-SQL

## <a name="next-steps"></a>Следующие шаги
* [Обзор аналитики озера данных Microsoft Azure](data-lake-analytics-overview.md)
* [Разработка скриптов U-SQL с помощью средств Data Lake для Visual Studio](data-lake-analytics-data-lake-tools-get-started.md)
