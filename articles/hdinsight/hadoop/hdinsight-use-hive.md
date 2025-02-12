---
title: Основные сведения об Apache Hive и HiveQL в Azure HDInsight
description: Apache Hive — это система хранилища данных для Apache Hadoop. Вы можете запрашивать данные, хранящиеся в Hive, с помощью HiveQL, который напоминает Transact-SQL. В рамках этого руководства вы узнаете, как использовать Hive и HiveQL в Azure HDInsight.
keywords: hiveql, что такое hive, hadoop hiveql, как использовать hive, знакомство с hive, обзор hive
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.topic: conceptual
ms.date: 06/06/2019
ms.openlocfilehash: ea8f14a7013a937ddd77baf0f50b8dca09cabad6
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "67076316"
---
# <a name="what-is-apache-hive-and-hiveql-on-azure-hdinsight"></a>Обзор Apache Hive и HiveQL в Azure HDInsight

[Apache Hive](https://hive.apache.org/) — это система хранилища данных для Apache Hadoop. Hive позволяет обобщать, запрашивать и анализировать данные. Запросы Hive создаются на языке запросов HiveQL, который похож на SQL.

Hive позволяет создавать структуру для преимущественно неструктурированных данных. После определения структуры вы можете использовать HiveQL для запроса этих данных без знания Java или MapReduce.

HDInsight предоставляет несколько типов кластера, которые подходят для конкретных рабочих нагрузок. Для запросов Hive наиболее часто используются следующие типы кластеров:

* __Interactive Query__. Кластер Hadoop, который обеспечивает функцию [аналитической обработки с низкой задержкой (LLAP)](https://cwiki.apache.org/confluence/display/Hive/LLAP) для оптимизации времени ответа для интерактивных запросов. Дополнительные сведения см. в статье [Использование Interactive Hive с HDInsight (предварительная версия)](../interactive-query/apache-interactive-query-get-started.md).

* __Hadoop__. Кластер Hadoop, который предназначен для рабочих нагрузок пакетной обработки. Дополнительные сведения см. в статье [Приступая к работе с Apache Hadoop в HDInsight](../hadoop/apache-hadoop-linux-tutorial-get-started.md).

* __Spark__. Apache Spark содержит встроенные функциональные возможности для работы с Hive. Дополнительные сведения см. в статье [Приступая к работе с Apache Spark в HDInsight](../spark/apache-spark-jupyter-spark-sql.md).

* __HBase__: HiveQL может использоваться для создания запросов данных, хранимых в Apache HBase. Дополнительные сведения см. в статье [Приступая к работе с Apache HBase в HDInsight](../hbase/apache-hbase-tutorial-get-started-linux.md).

## <a name="how-to-use-hive"></a>Как использовать Hive

В следующей таблице показаны различные способы использования Hive в HDInsight:

| **Используйте этот метод**, если требуется: | ...**интерактивные** запросы | ...**пакетная** обработка | ...из этого **кластера операционной системы** |
|:--- |:---:|:---:|:--- |:--- |
| [Средства HDInsight для Visual Studio Code](../hdinsight-for-vscode.md) |✔ |✔ | Linux, Unix, Mac OS X или Windows |
| [Средства HDInsight для Visual Studio](../hadoop/apache-hadoop-use-hive-visual-studio.md) |✔ |✔ |Windows |
| [Представление Hive](../hadoop/apache-hadoop-use-hive-ambari-view.md) |✔ |✔ |Для приложений на основе браузера |
| [клиент Beeline](../hadoop/apache-hadoop-use-hive-beeline.md) |✔ |✔ |Linux, Unix, Mac OS X или Windows |
| [REST API](../hadoop/apache-hadoop-use-hive-curl.md) |&nbsp; |✔ |Linux, Unix, Mac OS X или Windows |
| [Windows PowerShell](../hadoop/apache-hadoop-use-hive-powershell.md) |&nbsp; |✔ |Windows |


## <a name="hiveql-language-reference"></a>Справочник по языку HiveQL

Справочник по языку HiveQL доступен в [вручную языка](https://cwiki.apache.org/confluence/display/Hive/LanguageManual).

## <a name="hive-and-data-structure"></a>Hive и структура данных

Hive поддерживает работу со структурированными и частично структурированными данными. Например, с текстовыми файлами, в которых поля разделяются с помощью определенных знаков. С помощью следующей инструкции HiveQL создается таблица для данных, разделенных пробелами:

```hiveql
CREATE EXTERNAL TABLE log4jLogs (
    t1 string,
    t2 string,
    t3 string,
    t4 string,
    t5 string,
    t6 string,
    t7 string)
ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
STORED AS TEXTFILE LOCATION '/example/data/';
```

Hive также поддерживает пользовательские **сериализаторы/десериализаторы (SerDe)** для сложных или беспорядочно структурированных данных. Дополнительные сведения см. в документе [How to use a custom JSON SerDe with HDInsight](https://web.archive.org/web/20190217104719/ https://blogs.msdn.microsoft.com/bigdatasupport/2014/06/18/how-to-use-a-custom-json-serde-with-microsoft-azure-hdinsight/) (Как использовать настраиваемую сериализацию-десериализациюJSON с HDInsight).

Дополнительные сведения о форматах файлов, поддерживаемых Hive, см. в [руководстве по языку (https://cwiki.apache.org/confluence/display/Hive/LanguageManual)](https://cwiki.apache.org/confluence/display/Hive/LanguageManual).

### <a name="hive-internal-tables-vs-external-tables"></a>Сравнение внутренних и внешних таблиц Hive

Существует два типа таблиц, которые вы можете создать с помощью Hive:

* __Внутренние__. Данные хранятся в хранилище данных Hive. Хранилище данных расположено в `/hive/warehouse/`, в хранилище по умолчанию для кластера.

    Используйте внутренние таблицы, если выполняется одно из следующих условий:

    * данные являются временными;
    * вы хотите использовать Hive для управления жизненным циклом таблицы и данных.

* __Внешние__. Данные хранятся за пределами хранилища данных. Данные могут храниться в любом хранилище, доступном для кластера.

    Используйте внешние таблицы, если выполняется одно из следующих условий:

    * данные также используются за пределами Hive (например, файлы данных обновляются с помощью другого процесса, который не блокирует их);
    * данные должны оставаться в базовом расположении даже после удаления таблицы;
    * вам нужно пользовательское расположение, например нестандартная учетная запись хранилища;
    * программа, отличная от Hive, управляет форматом данных, расположением и т. д.

Дополнительные сведения см. в разделе [Hive внутренних и внешних таблиц Введение](https://blogs.msdn.microsoft.com/cindygross/2013/02/05/hdinsight-hive-internal-and-external-tables-intro/) записи блога.

## <a name="user-defined-functions-udf"></a>Определяемые пользователем функции (UDF)

Инфраструктура Hive также может быть расширена с помощью **определяемых пользователем функций (UDF)** . UDF позволяет реализовать функции или логику, сложно моделируемые в HiveQL. Примеры использования определяемых пользователем функций с Hive приведены в следующих документах:

* [Работа с определяемыми пользователем функциями Java с использованием Apache Hive в HDInsight](../hadoop/apache-hadoop-hive-java-udf.md)

* [Работа с определяемыми пользователем функциями Python с использованием Apache Hive в HDInsight](../hadoop/python-udf-hdinsight.md)

* [Работа с определяемыми пользователем функциями C# с использованием Apache Hive в HDInsight](../hadoop/apache-hadoop-hive-pig-udf-dotnet-csharp.md)

* [Как добавить настраиваемые определяемые пользователем функции Apache Hive в HDInsight](https://blogs.msdn.com/b/bigdatasupport/archive/2014/01/14/how-to-add-custom-hive-udfs-to-hdinsight.aspx)

* [Пример определяемой пользователем функции Apache Hive для преобразования форматов даты и времени в метки времени Hive](https://github.com/Azure-Samples/hdinsight-java-hive-udf)

## <a id="data"></a>Демонстрационные данные

Hive в HDInsight поставляется предварительно загруженным с внутренней таблицей `hivesampletable`. HDInsight также предоставляет пример наборов данных, которые могут использоваться с Hive. Эти наборы данных хранятся в каталогах `/example/data` и `/HdiSamples`. Эти каталоги находятся в хранилище по умолчанию для кластера.

## <a id="job"></a>Пример запроса Hive

Приведенная ниже инструкция HiveQL проецирует столбцы проекта в файл `/example/data/sample.log`.

```hiveql
DROP TABLE log4jLogs;
CREATE EXTERNAL TABLE log4jLogs (
    t1 string,
    t2 string,
    t3 string,
    t4 string,
    t5 string,
    t6 string,
    t7 string)
ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
STORED AS TEXTFILE LOCATION '/example/data/';
SELECT t4 AS sev, COUNT(*) AS count FROM log4jLogs 
    WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log' 
    GROUP BY t4;
```

В предыдущем примере операторы HiveQL выполняют следующие действия.


* `DROP TABLE`: Если таблица уже существует, удалите ее.

* `CREATE EXTERNAL TABLE`: Создает **внешнюю** таблицу в Hive. Внешние таблицы хранят только определения таблицы в Hive. Данные остаются в исходном расположении и формате.

* `ROW FORMAT`: инструкции по форматированию данных для Hive. В данном случае поля всех журналов разделены пробелом.

* `STORED AS TEXTFILE LOCATION`: Указывает Hive расположение хранения данных (каталог `example/data`) и их формат (текст). Данные могут храниться в одном файле или быть распределенными по нескольким файлам в каталоге.

* `SELECT`: Подсчитывает количество всех строк, в которых столбец **t4** содержит значение **[ERROR]** . Эта инструкция должна вернуть значение **3**, так как данное значение содержат три строки.

* `INPUT__FILE__NAME LIKE '%.log'`. Hive пытается применить схему ко всем файлам в каталоге. В этом случае каталог содержит файлы, которые не соответствуют схеме. Чтобы исключить лишние данные в результатах, эта инструкция указывает Hive возвращать данные только из файлов, заканчивающихся на .log.

> [!NOTE]  
> Внешние таблицы следует использовать, если исходные данные должны обновляться с использованием внешних источников. Например, процессом автоматизированной передачи данных или другой операцией MapReduce.
>
> Удаление внешней таблицы **не** приводит к удалению данных; будет удалено только описание таблицы.

Для создания **внутренней** таблицы вместо внешней используйте следующий запрос HiveQL.

```hiveql
CREATE TABLE IF NOT EXISTS errorLogs (
    t1 string,
    t2 string,
    t3 string,
    t4 string,
    t5 string,
    t6 string,
    t7 string)
STORED AS ORC;
INSERT OVERWRITE TABLE errorLogs
SELECT t1, t2, t3, t4, t5, t6, t7 
    FROM log4jLogs WHERE t4 = '[ERROR]';
```

Эти операторы выполняют следующие действия:

* `CREATE TABLE IF NOT EXISTS`: Если таблица не существует, создайте ее. Так как ключевое слово **EXTERNAL** не используется, эта инструкция создает внутреннюю таблицу. Таблица хранится в хранилище данных Hive и полностью управляется Hive.

* `STORED AS ORC`: Позволяет сохранить данные в формате ORC. Это высокооптимизированный и эффективный формат для хранения данных Hive.

* `INSERT OVERWRITE ... SELECT`: Выбирает строки из таблицы **log4jLogs**, содержащей значение **[ERROR]** , а затем вставляет данные в таблицу **errorLogs**.

> [!NOTE]  
> В отличие от внешних таблиц, удаление внутренней таблицы приводит к удалению базовых данных.

## <a name="improve-hive-query-performance"></a>Повышение производительности запросов Hive

### <a id="usetez"></a>Apache Tez

[Apache Tez](https://tez.apache.org) — это платформа, которая позволяет повысить производительность приложений, обрабатывающих большие объемы данных (включая Hive). Tez включена по умолчанию.  Раздел [Документация по работе Apache Hive на Tez](https://cwiki.apache.org/confluence/display/Hive/Hive+on+Tez) содержит дополнительные сведения о реализации этого решения и вариантах настроек.

### <a name="low-latency-analytical-processing-llap"></a>Аналитическая обработка с низкой задержкой (LLAP)

[LLAP](https://cwiki.apache.org/confluence/display/Hive/LLAP) (иногда называемая Live Long and Process) — это новая функция в Hive 2.0, которая разрешает кэширование запросов в памяти. LLAP создает запросы Hive гораздо быстрее — в [некоторых случаях в 26 раз быстрее, чем Hive версии 1.x](https://hortonworks.com/blog/announcing-apache-hive-2-1-25x-faster-queries-much/).

HDInsight предоставляет LLAP в кластере интерактивных запросов. Дополнительные сведения см. в статье [Использование Interactive Hive с HDInsight (предварительная версия)](../interactive-query/apache-interactive-query-get-started.md).

## <a name="scheduling-hive-queries"></a>Планирование запросов Hive

Есть несколько служб, которые поддерживают запросы Hive в рамках рабочего процесса по расписанию или по требованию.

### <a name="azure-data-factory"></a>Фабрика данных Azure

Фабрика данных Azure позволяет использовать HDInsight как часть конвейера фабрики данных. Дополнительные сведения об использовании Hive из конвейера см. в документе [Преобразование данных с помощью действия Hadoop Hive в фабрике данных Azure](../../data-factory/transform-data-using-hadoop-hive.md).

### <a name="hive-jobs-and-sql-server-integration-services"></a>Задания Pig и SQL Server Integration Services

С помощью служб SQL Server Integration Services (SSIS) можно выполнить задание Hive. Пакет дополнительных компонентов Azure для служб SSIS предоставляет следующие компоненты, которые работают с заданиями Hive в HDInsight.

* [Задача Hive для Azure HDInsight](https://docs.microsoft.com/sql/integration-services/control-flow/azure-hdinsight-hive-task)

* [Диспетчер подключений подписки Azure](https://docs.microsoft.com/sql/integration-services/connection-manager/azure-subscription-connection-manager)

Дополнительные сведения см. в разделе [пакета дополнительных компонентов Azure](https://docs.microsoft.com/sql/integration-services/azure-feature-pack-for-integration-services-ssis) документации.

### <a name="apache-oozie"></a>Apache Oozie

Apache Oozie — это система рабочих процессов и координации, управляющая заданиями Hadoop. Дополнительные сведения см. в статье об [использовании Apache Oozie с Hive для определения и запуска рабочего процесса](../hdinsight-use-oozie-linux-mac.md).

## <a name="next-steps"></a>Дальнейшие действия

Теперь, когда вы знаете, что такое инфраструктура Hive и как ее использовать с Hadoop в HDInsight, воспользуйтесь следующими ссылками, чтобы изучить другие способы работы с Azure HDInsight.

* [Отправка данных в HDInsight](../hdinsight-upload-data.md)
* [Использование определяемых пользователем функций Python с Apache Hive и Apache Pig в HDInsight](./python-udf-hdinsight.md)
* [Использование заданий MapReduce с HDInsight](hdinsight-use-mapreduce.md)
