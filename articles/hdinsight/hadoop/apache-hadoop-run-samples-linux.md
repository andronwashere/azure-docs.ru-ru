---
title: Выполнение примеров Apache Hadoop MapReduce в HDInsight в Azure
description: Начало работы с образцами MapReduce в файлах JAR, включенных в HDInsight. Для подключения к кластеру используйте SSH, после чего используйте команду Hadoop для запуска примеров заданий.
keywords: пример jar-файла hadoop,примеры jar-файлов hadoop,примеры hadoop mapreduce,примеры mapreduce
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.topic: conceptual
ms.date: 04/25/2019
ms.author: hrasheed
ms.openlocfilehash: f0251e3926c569b45ebebcd18b98df5af4564443
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "64706662"
---
# <a name="run-the-mapreduce-examples-included-in-hdinsight"></a>Выполнение примеров MapReduce, включенных в HDInsight

[!INCLUDE [samples-selector](../../../includes/hdinsight-run-samples-selector.md)]

Узнайте, как выполнять примеры MapReduce, входящие в состав Apache Hadoop в HDInsight.

## <a name="prerequisites"></a>Технические условия

* Кластер Apache Hadoop в HDInsight. Ознакомьтесь со статьей [Краткое руководство. Использование Apache Hadoop и Apache Hive в Azure HDInsight с шаблоном Resource Manager](./apache-hadoop-linux-tutorial-get-started.md).

* Клиент SSH. Дополнительные сведения см. в руководстве по [подключению к HDInsight (Apache Hadoop) с помощью SSH](../hdinsight-hadoop-linux-use-ssh-unix.md).

## <a name="the-mapreduce-examples"></a>Примеры MapReduce

**Расположение**. Примеры расположены в кластере HDInsight в `/usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar`.

**Содержимое**. В архиве содержатся следующие примеры:

* `aggregatewordcount`: программа MapReduce для подсчета слов во входных файлах с использованием статистических выражений.
* `aggregatewordhist`: программа MapReduce для создания гистограммы слов во входных файлах с использованием статистических выражений.
* `bbp`: программа MapReduce для вычисления знаков числа π с использованием формулы Бэйли-Боруэйна-Плаффа.
* `dbcount`: пример задания для подсчета журналов просмотра страниц, сохраненных в базе данных.
* `distbbp`: программа MapReduce для вычисления знаков числа π с использованием формулы ББП.
* `grep`: программа MapReduce для подсчета вхождений регулярного выражения во входных данных.
* `join`: задание для объединения сортированных наборов данных одного размера.
* `multifilewc`: задание для подсчета слов в нескольких файлах.
* `pentomino`: программа MapReduce для укладки фигур с целью поиска решений при игре в пентамино.
* `pi`: программа MapReduce для расчета числа π по методу квази-Монте-Карло.
* `randomtextwriter`: программа MapReduce для записи 10 ГБ случайных текстовых данных на узел.
* `randomwriter`: программа MapReduce для записи 10 ГБ случайных данных на узел.
* `secondarysort`: пример определения вторичной сортировки для этапа редукции.
* `sort`: программа MapReduce для сортировки случайных данных.
* `sudoku`: программа решения судоку.
* `teragen`: создание данных для программы TeraSort.
* `terasort`: выполнение программы TeraSort.
* `teravalidate`: проверка результатов выполнения программы TeraSort.
* `wordcount`: программа MapReduce для подсчета слов во входных файлах.
* `wordmean`: программа MapReduce для подсчета средней длины слов во входных файлах.
* `wordmedian`: программа MapReduce для подсчета средней длины слов по Смиту во входных файлах.
* `wordstandarddeviation`: программа MapReduce для подсчета стандартного отклонения в длинах слов во входных файлах.

**Исходный код**. Исходный код этих примеров расположен в кластере HDInsight в `/usr/hdp/current/hadoop-client/src/hadoop-mapreduce-project/hadoop-mapreduce-examples`.

## <a name="run-the-wordcount-example"></a>Запуск примера для подсчета слов

1. Подключитесь к HDInsight с помощью протокола SSH. Замените `CLUSTER` с именем кластера и введите следующую команду:

    ```cmd
    ssh sshuser@CLUSTER-ssh.azurehdinsight.net
    ```

2. В командной строке `username@#######:~$` введите следующую команду, чтобы вывести список примеров:

    ```bash
    yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar
    ```

    Эта команда создает список примеров из предыдущего раздела данного документа.

3. Чтобы получить справку по конкретному примеру, используйте следующую команду: В данном случае запускается пример **wordcount**.

    ```bash
    yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar wordcount
    ```

    Отобразится следующее сообщение.

        Usage: wordcount <in> [<in>...] <out>

    Это сообщение означает, что можно указать несколько входных путей для исходных документов. Окончательный путь — это место, где сохраняются выходные данные (число слов в исходных документах).

4. Используйте следующие параметры для подсчета всех слов в записные книжки из Леонардо да Винчи, которые предоставляются в качестве образца данных с кластером.

    ```bash
    yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar wordcount /example/data/gutenberg/davinci.txt /example/data/davinciwordcount
    ```

    Для этого задания считываются входные данные из файла `/example/data/gutenberg/davinci.txt`. Выходные данные для этого примера сохраняются в `/example/data/davinciwordcount`. Оба пути расположены в хранилище по умолчанию для кластера, а не в локальной файловой системе.

   > [!NOTE]  
   > Как сказано в справке по примеру wordcount, вы также можете указать несколько входных файлов. Например, команда `hadoop jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar wordcount /example/data/gutenberg/davinci.txt /example/data/gutenberg/ulysses.txt /example/data/twowordcount` позволит подсчитать слова в файлах davinci.txt и ulysses.txt.

5. По завершении задания воспользуйтесь следующей командой, чтобы просмотреть результат:

    ```bash
    hdfs dfs -cat /example/data/davinciwordcount/*
    ```

    Эта команда сцепляет все выходные файлы, созданные заданием. Она отображает выходные данные в консоли. Результат будет аналогичен приведенному ниже:

        zum     1
        zur     1
        zwanzig 1
        zweite  1

    Каждая строка соответствует одному слову и частоте его появления в исходных данных.

## <a name="the-sudoku-example"></a>Пример судоку

[Судоку](https://en.wikipedia.org/wiki/Sudoku) — это логическая головоломка, которая состоит из девяти полей размером 3 x 3 клетки. Некоторые ячейки в клетках содержат числа, остальные ячейки пустые. Задача заключается в поиске чисел для пустых ячеек. С помощью указанной выше ссылки можно получить дополнительные сведения о головоломке, однако целью этого примера является поиск значений для пустых ячеек. Таким образом, на входе программы должен быть файл в следующем формате.

* Девять строк и девять столбцов
* Каждый столбец может содержать число или знак `?` (означает пустую ячейку).
* Ячейки разделяются пробелами

Следующая задача — составление головоломки судоку, по правилам которой не допускается использование одного и того же числа в строке или столбце. В хранилище кластера HDInsight есть соответствующий пример. Он находится в `/usr/hdp/*/hadoop/src/hadoop-mapreduce-project/hadoop-mapreduce-examples/src/main/java/org/apache/hadoop/examples/dancing/puzzle1.dta` и содержит приведенный ниже текст.

    8 5 ? 3 9 ? ? ? ?
    ? ? 2 ? ? ? ? ? ?
    ? ? 6 ? 1 ? ? ? 2
    ? ? 4 ? ? 3 ? 5 9
    ? ? 8 9 ? 1 4 ? ?
    3 2 ? 4 ? ? 8 ? ?
    9 ? ? ? 8 ? 5 ? ?
    ? ? ? ? ? ? 2 ? ?
    ? ? ? ? 4 5 ? 7 8

Чтобы обработать эти данные в примере судоку, используйте следующую команду.

```bash
yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar sudoku /usr/hdp/*/hadoop/src/hadoop-mapreduce-project/hadoop-mapreduce-examples/src/main/java/org/apache/hadoop/examples/dancing/puzzle1.dta
```

Полученный текст должен выглядеть следующим образом.

    8 5 1 3 9 2 6 4 7
    4 3 2 6 7 8 1 9 5
    7 9 6 5 1 4 3 8 2
    6 1 4 8 2 3 7 5 9
    5 7 8 9 6 1 4 2 3
    3 2 9 4 5 7 8 1 6
    9 4 7 2 8 6 5 3 1
    1 8 5 7 3 9 2 6 4
    2 6 3 1 4 5 9 7 8

## <a name="pi--example"></a>Пример "Пи" (π)

В примере «Пи» используется статистический метод (квази-Монте-Карло) оценки значения числа пи. Точки в произвольном порядке помещаются внутри единичного квадрата. В квадрат также вписан круг. Вероятность того, что точки попадут в круг, равна площади круга, π/4. Значение числа π можно оценить по формуле 4R. R — это отношение количества точек, находящихся внутри круга, к общему количеству точек, находящихся внутри квадрата. Чем больше выборка используемых точек, тем точнее оценка.

Для запуска примера используйте следующую команду. Для оценки числа π в команде используется 16 карт с 10 000 000 примерами в каждой.

```bash
yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar pi 16 10000000
```

Команда должна возвращать значение наподобие **3,14159155000000000000**. Для справки, первые 10 знаков числа пи: 3,1415926535.

## <a name="10-gb-graysort-example"></a>Пример сортировки GraySort 10 ГБ

GraySort — это измерение производительности сортировки. Его показателем служит скорость (ТБ/мин), достигаемая при сортировке очень больших объемов данных, обычно не менее 100 ТБ.

В этом примере используется небольшой объем данных, 10 ГБ, чтобы можно было выполнить сортировку достаточно быстро. Используются приложения MapReduce, разработанные Оуэном О'Мэлли (Owen O'Malley) и Аруном Мёрти (Arun Murthy). Эти приложения выиграла годовой тестирования сортировки общего назначения терабайт («году») в 2009 г., показав скорость 0,578 ТБ/мин (100 ТБ за 173 минуты). Дополнительные сведения об этом и других производительности сортировки см. в разделе [тест производительности сортировки](https://sortbenchmark.org/) сайта.

В этом примере используются три набора программ MapReduce.

* **Teragen**: программа MapReduce, которая создает строки с данными для последующей сортировки.

* **Terasort**: производит выборку входных данных и использует MapReduce для сортировки данных в общем порядке.

    TeraSort представляет собой стандартную сортировку MapReduce, за исключением настраиваемого разделителя. В разделителе используется отсортированный список выборки ключей N-1, определяющий диапазон ключей для каждой функции reduce. В частности, все ключи, подобные этому примеру [i-1] <= key < sample[i] отправляются для сокращения i. Этот разделитель гарантирует, что выходные данные сокращения i будут меньше, чем выходные данные i+1.

* **Teravalidate**: программа MapReduce, которая проверяет глобальную сортировку выходных данных.

    В выходном каталоге создается одна функция map для каждого файла, и каждая функция map гарантирует, что каждый ключ будет меньше или равен предыдущему. Функция map создает записи первого и последнего ключей каждого файла. Функция reduce гарантирует, что первый ключ файла i больше последнего ключа файла i-1. Все проблемы указываются в выходных данных этапа редукции вместе с неотсортированными ключами.

Для создания данных, сортировки и проверки выходных данных используйте следующую команду:

1. Создайте 10 ГБ данных, которые сохранятся в хранилище по умолчанию кластера HDInsight в `/example/data/10GB-sort-input`.

    ```bash
    yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar teragen -Dmapred.map.tasks=50 100000000 /example/data/10GB-sort-input
    ```

    Ключ `-Dmapred.map.tasks` говорит Hadoop о том, сколько задач сопоставления будет использоваться в этом задании. Последние два параметра означают, что задание создаст 10 ГБ данных и сохранит их в `/example/data/10GB-sort-input`.

2. Выполните следующую команду, чтобы отсортировать данные:

    ```bash
    yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar terasort -Dmapred.map.tasks=50 -Dmapred.reduce.tasks=25 /example/data/10GB-sort-input /example/data/10GB-sort-output
    ```

    Ключ `-Dmapred.reduce.tasks` говорит Hadoop о том, сколько задач сокращения будет использоваться в этом задании. Последние два параметра соответствуют путям расположения входных и выходных данных.

3. Используйте следующую команду, чтобы просмотреть отсортированные данные:

    ```bash
    yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar teravalidate -Dmapred.map.tasks=50 -Dmapred.reduce.tasks=25 /example/data/10GB-sort-output /example/data/10GB-sort-validate
    ```

## <a name="next-steps"></a>Дальнейшие действия

В данной статье рассмотрено выполнение примеров, поставляемых с кластерами HDInsight под управлением Linux. Учебники по использованию Pig, Hive и MapReduce в службе HDInsight см. в следующих статьях.

* [Использование Apache Pig с Apache Hadoop в HDInsight](hdinsight-use-pig.md)
* [Использование Apache Hive с Apache Hadoop в HDInsight](hdinsight-use-hive.md)
* [Использование MapReduce в Apache Hadoop в HDInsight](hdinsight-use-mapreduce.md)