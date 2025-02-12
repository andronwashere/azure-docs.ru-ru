---
title: Ядра для записной книжки Jupyter в кластерах Spark в Azure HDInsight
description: Сведения о ядрах PySpark, PySpark3 и Spark для записной книжки Jupyter, доступной с кластерами Spark в Azure HDInsight.
keywords: записная книжка jupyter в spark, jupyter в spark
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.topic: conceptual
ms.date: 05/27/2019
ms.author: hrasheed
ms.openlocfilehash: b2ae24c0449b009db6fcecdd8a1366ea5154629a
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "66257797"
---
# <a name="kernels-for-jupyter-notebook-on-apache-spark-clusters-in-azure-hdinsight"></a>Ядра для записной книжки Jupyter в кластерах Apache Spark в Azure HDInsight 

Кластеры HDInsight Spark предоставляют ядра, которые можно использовать с записной книжкой Jupyter в [Apache Spark](https://spark.apache.org/) для тестирования приложений. Ядра — это программа, которая выполняет и интерпретирует ваш код. Вот эти ядра:

- **PySpark** — для приложений, написанных на языке Python2.
- **PySpark3** — для приложений, написанных на языке Python3.
- **Spark** — для приложений, написанных на языке Scala.

В этой статье вы узнаете, как использовать эти ядра, а также преимущества их использования.

## <a name="prerequisites"></a>Технические условия

Кластер Apache Spark в HDInsight. Инструкции см. в статье [Начало работы. Создание кластера Apache Spark в HDInsight на платформе Linux и выполнение интерактивных запросов с помощью SQL Spark](apache-spark-jupyter-spark-sql.md).

## <a name="create-a-jupyter-notebook-on-spark-hdinsight"></a>Создание записной книжки Jupyter в Spark HDInsight

1. Из [портала Azure](https://portal.azure.com/), выберите свой кластер Spark.  Инструкции см. в разделе [Отображение кластеров](../hdinsight-administer-use-portal-linux.md#showClusters). **Обзор** открывается представление.

2. Из **Обзор** представления **панели мониторинга кластера** выберите **записной книжки Jupyter**. При появлении запроса введите учетные данные администратора для кластера.

    ![Записная книжка Jupyter в Spark](./media/apache-spark-jupyter-notebook-kernels/hdinsight-spark-open-jupyter-interactive-spark-sql-query.png "Записная книжка Jupyter в Spark") 
  
   > [!NOTE]  
   > Вы также можете получить доступ к записной книжке Jupyter в кластере Spark, открыв следующий URL-адрес в браузере. Замените **CLUSTERNAME** именем кластера:
   >
   > `https://CLUSTERNAME.azurehdinsight.net/jupyter`

3. Выберите **New**, а затем выберите пункт **Pyspark**, **PySpark3**, или **Spark** Чтобы создать записную книжку. Для приложений Scala используйте ядро Spark, для приложений Python2 — ядро PySpark, а для приложений Python3 — ядро PySpark3.
   
    ![Ядра для записной книжки Jupyter в Spark](./media/apache-spark-jupyter-notebook-kernels/kernel-jupyter-notebook-on-spark.png "Ядра для записной книжки Jupyter в Spark") 

4. Объект Notebook должен открыться с помощью выбранного ядра.

## <a name="benefits-of-using-the-kernels"></a>Преимущества использования ядер

Ниже приведены некоторые преимущества использования новых ядер для записной книжки Jupyter в кластерах Spark HDInsight.

- **Предустановленные контексты**. Благодаря ядрам **PySpark**, **PySpark3** и **Spark** вам не требуется явно настраивать контексты Spark или Hive перед началом работы с приложением. Они доступны по умолчанию. а именно:
   
  * **sc** для контекста Spark;
  * **sqlContext** для контекста Hive.
   
    Это значит, что для настройки этих контекстов вам не придется выполнять операторы следующего вида:
   
         sc = SparkContext('yarn-client')
         sqlContext = HiveContext(sc)
   
    Вместо этого вы сможете сразу использовать в своем приложении предустановленные контексты.

- **Волшебные команды.** Ядро PySpark предоставляет несколько "магических команд". Это специальные команды, которые можно вызывать с помощью `%%` (например, `%%MAGIC` `<args>`). Волшебная команда должна быть первым словом в ячейке кода и может состоять из нескольких строк содержимого. Волшебное слово должно быть первым словом в ячейке. Любые другие слова перед магической командой, даже комментарии, приведут к ошибке.     Дополнительные сведения о волшебных командах см. [здесь](https://ipython.readthedocs.org/en/stable/interactive/magics.html).
   
    В следующей таблице перечислены различные магические команды, доступные для ядер.

   | Волшебная команда | Пример | Описание |
   | --- | --- | --- |
   | help |`%%help` |Формирует таблицу из всех доступных волшебных слов с примерами и описанием. |
   | info |`%%info` |Выводит сведения о сеансе для текущей конечной точки Livy. |
   | Настройка |`%%configure -f`<br>`{"executorMemory": "1000M"`,<br>`"executorCores": 4`} |Настраивает параметры для создания сеанса. Флаг force (-f) является обязательным, если сеанс уже был создан, иначе сеанс будет удален и создан заново. Список допустимых параметров приведен в разделе, посвященном [тексту запроса сеансов POST Livy](https://github.com/cloudera/livy#request-body) . Параметры должны передаваться в виде строки JSON, следующей после волшебной команды, как показано в столбце примера. |
   | sql |`%%sql -o <variable name>`<br> `SHOW TABLES` |Выполняет запрос Hive к sqlContext. Если передан параметр `-o` , результат запроса сохраняется в контексте Python %%local в качестве таблицы данных [Pandas](https://pandas.pydata.org/) . |
   | local |`%%local`<br>`a=1` |Весь код в последующих строках выполняется локально. В качестве кода должен быть указан допустимый код на языке Python2 (вне зависимости от используемого ядра). Таким образом, даже если вы выбрали **PySpark3** или **Spark** ядра при создании записной книжки, если вы используете `%%local` ничего в ячейке этой ячейке должен содержаться только допустимый код Python2. |
   | logs |`%%logs` |Выводит журналы для текущего сеанса Livy. |
   | удалить |`%%delete -f -s <session number>` |Удаляет указанный сеанс для текущей конечной точки Livy. Удалить сеанс, который был инициирован самим ядром, невозможно. |
   | cleanup |`%%cleanup -f` |Удаляет все сеансы для текущей конечной точки Livy, включая сеанс этой записной книжки. Флаг -f является обязательным. |

   > [!NOTE]  
   > Помимо магических команд, добавленных ядром PySpark, можно также использовать [встроенные магические команды](https://ipython.org/ipython-doc/3/interactive/magics.html#cell-magics) IPython, в том числе `%%sh`. Можно использовать магическую команду `%%sh` для выполнения сценариев и блоков кода на головном узле кластера.

- **Автоматическая визуализация**. Ядро Pyspark автоматически визуализирует выходные данные запросов Hive и SQL. Вы можете выбрать различные типы средства визуализации, включая таблицы, круговые диаграммы, графики, диаграммы с областями и линейчатые диаграммы.

## <a name="parameters-supported-with-the-sql-magic"></a>Параметры, поддерживаемые волшебной командой %%sql
Магическая команда `%%sql` поддерживает различные параметры, позволяющие управлять результатом выполнения запросов. Возможные результаты показаны в следующей таблице.

| Параметр | Пример | Описание |
| --- | --- | --- |
| -o |`-o <VARIABLE NAME>` |При использовании этого параметра результат запроса сохраняется в контексте Python %%local в качестве таблицы данных [Pandas](https://pandas.pydata.org/) . Именем переменной таблицы данных служит указанное вами имя переменной. |
| -q |`-q` |Позволяет отключить визуализации для ячейки. Если вы не хотите autovisualize содержимого ячейки и просто хотите записать ее как таблицу данных, используйте `-q -o <VARIABLE>`. Если вы хотите отключить визуализацию, не записывая результаты (например, для выполнения запроса SQL, такого как инструкция `CREATE TABLE`), то используйте параметр `-q` без аргумента `-o`. |
| -m |`-m <METHOD>` |Параметр **METHOD** имеет значение **take** или **sample** (по умолчанию используется значение **take**). Если используется метод **take**, то ядро выбирает элементы из верхней части результирующего набора данных, который определяется параметром MAXROWS (описывается далее в этой таблице). Если используется метод **sample**, то ядро выбирает элементы из набора данных случайным образом в соответствии с параметром `-r`, описанным далее в этой таблице. |
| -r |`-r <FRACTION>` |Здесь **FRACTION** — это число с плавающей запятой от 0,0 до 1,0. Если для SQL-запроса используется метод выборки `sample`, то ядро выбирает заданную долю элементов из результирующего набора случайным образом. Например, при выполнении SQL-запроса с аргументами `-m sample -r 0.01` из результирующего набора данных случайным образом отбирается 1 % строк. |
| -n |`-n <MAXROWS>` |**MAXROWS** должно быть выражено целым числом. Число выходных строк для параметра **MAXROWS** ограничивается ядром. Если значение параметра **MAXROWS** выражено отрицательным числом, например **-1**, то число строк в результирующем наборе не ограничивается. |

**Пример.**

    %%sql -q -m sample -r 0.1 -n 500 -o query2
    SELECT * FROM hivesampletable

Приведенная выше инструкция делает следующее:

* Выбирает все записи из таблицы **hivesampletable**.
* Так как параметр - q, отключается autovisualization.
* Случайным образом выбирает 10 % строк из таблицы hivesampletable и ограничивает размер результирующего набора 500 строками, так как включает параметр `-m sample -r 0.1 -n 500` .
* И наконец, сохраняет выходные данные в таблицу данных **query2**, так как включает параметр `-o query2`.

## <a name="considerations-while-using-the-new-kernels"></a>Рекомендации по использованию новых ядер

Какое бы ядро вы ни использовали, работающие объекты Notebook потребляют ресурсы кластера.  Используя эти ядра (так как контексты заданы предварительно), при простом выходе из объектов Notebook контекст не завершается, а значит ресурсы кластера продолжают использоваться. После завершения работы с объектом Notebook рекомендуется выбрать **Close and Halt** (Закрыть и остановить) в меню **File** (Файл) объекта Notebook. Это действие завершит контекст и закроет объект Notebook.

## <a name="where-are-the-notebooks-stored"></a>Где хранятся записные книжки?

Если кластер HDInsight использует службу хранилища Azure в качестве учетной записи хранения по умолчанию, записные книжки Jupyter сохраняются в папке **/HdiNotebooks** в учетной записи хранения.  Доступ к объектам Notebook, текстовым файлам и папкам, создаваемым в Jupyter, можно получить через учетную запись хранения.  Например, если Jupyter используется для создания папки **myfolder** и объекта Notebook **myfolder/mynotebook.ipynb**, то доступ к этому объекту можно получить в расположении `/HdiNotebooks/myfolder/mynotebook.ipynb` в учетной записи хранения.  Верно и обратное: если вы передаете объект Notebook непосредственно в свою учетную запись хранения в `/HdiNotebooks/mynotebook1.ipynb`, то этот объект также отображается в Jupyter.  Объекты Notebook хранятся в учетной записи хранения даже после удаления кластера.

> [!NOTE]  
> Для кластеров HDInsight, использующих Azure Data Lake Storage в качестве хранилища по умолчанию, записные книжки не сохраняются в связанном хранилище.

Записные книжки сохраняются в учетной записи хранения, совместимой с [Apache Hadoop HDFS](https://hadoop.apache.org/docs/r1.2.1/hdfs_design.html). Таким образом, подключаясь к кластеру по протоколу SSH, вы можете использовать команды управления файлами, как показано в следующем фрагменте:

    hdfs dfs -ls /HdiNotebooks                            # List everything at the root directory – everything in this directory is visible to Jupyter from the home page
    hdfs dfs –copyToLocal /HdiNotebooks                   # Download the contents of the HdiNotebooks folder
    hdfs dfs –copyFromLocal example.ipynb /HdiNotebooks   # Upload a notebook example.ipynb to the root folder so it’s visible from Jupyter

Вне зависимости от того, использует кластер службу хранилища Azure или Azure Data Lake Storage в качестве учетной записи хранения по умолчанию, записные книжки также сохраняются на головном узле кластера в `/var/lib/jupyter`.

## <a name="supported-browser"></a>Поддерживаемый браузер

Записные книжки Jupyter, выполняемые в кластерах HDInsight Spark, поддерживаются только браузером Google Chrome.

## <a name="feedback"></a>Отзыв

Новые ядра находятся в стадии развития и будут улучшаться со временем. Кроме того, это может означать, что по мере развития этих ядер API могут измениться. Мы будем признательны вам за любые отзывы о работе с новыми ядрами. Ваши комментарии помогут нам оформить финальную версию этих ядер. Можно оставить отзывы и замечания под **отзывы** в нижней части этой статьи.

## <a name="seealso"></a>Дополнительные материалы
* [Apache Spark в Azure HDInsight](apache-spark-overview.md)

### <a name="scenarios"></a>Сценарии
* [Руководство. Анализ данных Apache Spark с использованием Power BI в HDInsight](apache-spark-use-bi-tools.md)
* [Использование Apache Spark MLlib для Создание приложения машинного обучения Apache Spark в HDInsight](apache-spark-ipython-notebook-machine-learning.md)
* [Использование Apache Spark MLlib для создания приложения машинного обучения и анализа набора данных](apache-spark-machine-learning-mllib-ipython.md)
* [Анализ журналов веб-сайтов с помощью Apache Spark в HDInsight](apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-run-applications"></a>Создание и запуск приложений
* [Создание автономного приложения с использованием Scala](apache-spark-create-standalone-application.md)
* [Удаленный запуск заданий с помощью Apache Livy в кластере Apache Spark](apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a>Средства и расширения
* [Использование подключаемого модуля средств HDInsight для IntelliJ IDEA для создания и отправки приложений Spark Scala](apache-spark-intellij-tool-plugin.md)
* [Удаленная отладка приложений Apache Spark с помощью подключаемого модуля средств HDInsight для IntelliJ IDEA](apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [Использование записных книжек Zeppelin с кластером Apache Spark в Azure HDInsight](apache-spark-zeppelin-notebook.md)
* [Использование внешних пакетов с записными книжками Jupyter](apache-spark-jupyter-notebook-use-external-packages.md)
* [Установка записной книжки Jupyter на компьютере и ее подключение к кластеру Apache Spark в Azure HDInsight (предварительная версия)](apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Управление ресурсами
* [Управление ресурсами кластера Apache Spark в Azure HDInsight](apache-spark-resource-manager.md)
* [Отслеживание и отладка заданий в кластере Apache Spark в HDInsight на платформе Linux](apache-spark-job-debugging.md)
