---
title: Рекомендации по настройке производительности Azure Data Lake Storage 2-го поколения | Документация Майкрософт
description: Рекомендации по настройке производительности Azure Data Lake Storage 2-го поколения
services: storage
author: normesta
ms.subservice: data-lake-storage-gen2
ms.service: storage
ms.topic: conceptual
ms.date: 12/06/2018
ms.author: normesta
ms.reviewer: stewu
ms.openlocfilehash: 6f831dd0cde4641eb48f3c23e010f8c5e8aa3fa2
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "64939357"
---
# <a name="tuning-azure-data-lake-storage-gen2-for-performance"></a>Настройка Azure Data Lake Storage 2-го поколения для повышения производительности

Azure Data Lake Storage 2-го поколения поддерживает высокую пропускную способность при анализе с интенсивным использованием ввода-вывода и перемещением данных.  В Data Lake Storage 2-го поколения важно использовать всю доступную пропускную способность (объем данных, которые можно читать или записывать в секунду), чтобы обеспечить высокую производительность.  Для этого нужно выполнить как можно больше операций чтения и записи параллельно.

![Производительность Data Lake Storage 2-го поколения](./media/data-lake-storage-performance-tuning-guidance/throughput.png)

Data Lake Storage 2-го поколения можно масштабировать, чтобы предоставить необходимую пропускную способность для всех сценариев аналитики. По умолчанию учетная запись Data Lake Storage 2-го поколения автоматически предоставляет достаточную пропускную способность для выполнения различных сценариев использования. В случаях, когда клиенты достигают лимита по умолчанию, учетную запись Data Lake Storage 2-го поколения можно настроить для предоставления большей пропускной способности, связавшись с [поддержкой Azure](https://azure.microsoft.com/support/faq/).

## <a name="data-ingestion"></a>Прием данных

При передаче данных из исходной системы в Data Lake Storage 2-го поколения необходимо учесть, что исходное оборудование, сетевое оборудование источника и сетевое подключение к Data Lake Storage 2-го поколения могут быть узким местом.  

![Производительность Data Lake Storage 2-го поколения](./media/data-lake-storage-performance-tuning-guidance/bottleneck.png)

Очень важно убедиться, что перемещение данных не зависит от следующих факторов.

### <a name="source-hardware"></a>Исходное оборудование

При использовании локальных компьютеров или виртуальных машин в Azure следует тщательно выбирать соответствующее оборудование. В качестве исходного дискового оборудования следует использовать SSD, а не жесткие диски, поэтому выбирайте дисковое оборудование с быстро работающими шпинделями. В качестве исходного сетевого оборудования используйте самые быстрые сетевые адаптеры.  В Azure мы советуем виртуальные машины Azure D14 с соответствующим мощным диском и сетевым оборудованием.

### <a name="network-connectivity-to-data-lake-storage-gen2"></a>Сетевое подключение к Data Lake Storage 2-го поколения

Сетевое подключение между исходными данными и Data Lake Storage 2-го поколения иногда может быть узким местом. Если исходные данные находятся в локальной среде, рекомендуется использовать выделенный канал с [Azure ExpressRoute](https://azure.microsoft.com/services/expressroute/). Если исходные данные находятся в Azure, вы сможете обеспечить лучшую производительность, если разместите данные в том же регионе Azure, что и учетную запись Data Lake Storage 2-го поколения.

### <a name="configure-data-ingestion-tools-for-maximum-parallelization"></a>Настройка средств приема данных для обеспечения максимальной параллелизации

Решив проблему с узкими местами исходного оборудования и сетевого подключения, можно приступить к настройке средств приема. В следующей таблице перечислены ключевые параметры нескольких популярных средств приема и предоставлены подробные статьи по настройке производительности для них.  Дополнительные сведения о выборе подходящего средства для вашего сценария см. в [этой статье](data-lake-storage-data-scenarios.md).

| Средство               | Параметры     | Дополнительные сведения                                                                 |
|--------------------|------------------------------------------------------|------------------------------|
| DistCp            | -m (mapper)   | [Ссылка](data-lake-storage-use-distcp.md#performance-considerations-while-using-distcp)                             |
| Фабрика данных Azure| parallelCopies    | [Ссылка](../../data-factory/copy-activity-performance.md)                          |
| Sqoop           | fs.azure.block.size, -m (mapper)    |   [Ссылка](https://blogs.msdn.microsoft.com/bigdatasupport/2015/02/17/sqoop-job-performance-tuning-in-hdinsight-hadoop/)        |

## <a name="structure-your-data-set"></a>Структура набора данных

При хранении данных в Data Lake Storage 2-го поколения размер файла, число файлов и структура папок влияют на производительность.  В следующем разделе описаны рекомендации в этих областях.  

### <a name="file-size"></a>Размер файла

Как правило, у модулей аналитики, таких как HDInsight и Azure Data Lake Analytics, нагрузка зависит от количества файлов. Если вы храните данные в большом количестве небольших файлов, это может отрицательно сказаться на производительности. Упорядочивайте свои данные в файлы большего размера для лучшей производительности (от 256 МБ до 100 ГБ). В некоторых подсистемах и приложениях могут возникнуть проблемы с эффективной обработкой файлов размером более 100 ГБ.

В некоторых случаях конвейеры данных имеют ограниченный контроль над необработанными данными, которые содержат множество небольших файлов. Рекомендуется использовать процесс подготовки, который создает файлы большего размера для дочерних приложений.

### <a name="organizing-time-series-data-in-folders"></a>Упорядочение данных временных рядов в папки

Для рабочих нагрузок Hive удаление секций данных временных рядов может помочь некоторым запросам считывать только подмножество данных, что улучшает производительность.    

Конвейеры, принимающие данные временных рядов, часто помещают свои файлы с сильно структурированными именами файлов и папок. Ниже приведен обычный пример для данных, структурированных по дате:

    \DataSet\YYYY\MM\DD\datafile_YYYY_MM_DD.tsv

Обратите внимание, что данные времени и даты отображаются и как папки, и в имени файла.

Ниже приведен распространенный шаблон для даты и времени:

    \DataSet\YYYY\MM\DD\HH\mm\datafile_YYYY_MM_DD_HH_mm.tsv

Выбор, который вы делаете с помощью упорядочения папки и файлов, должен быть оптимизирован для больших размеров файлов и разумного количества файлов в каждой папке.

## <a name="optimizing-io-intensive-jobs-on-hadoop-and-spark-workloads-on-hdinsight"></a>Оптимизация заданий с интенсивными вычислениями ввода-вывода для рабочих нагрузок Hadoop и Spark в HDInsight

Задания можно отнести к одной из трех категорий:

* **Интенсивно использующие ЦП.**  Эти задания имеют долгое время вычисления с минимальным временем ввода-вывода.  Примеры включают машинное обучение и задания обработки естественных языков.  
* **С высоким объемом требуемой памяти.**  Эти задания используют большой объем памяти.  Примеры включают PageRank и задания аналитики в реальном времени.  
* **С большим количеством операций ввода-вывода.**  Эти задания большую часть своего времени выполняют операции ввода-вывода.  Распространенным примером является задание копирования, которое использует только операции чтения и записи.  Другие примеры включают задания подготовки данных, которые считывают большое количество данных, выполняют некоторую трансформацию данных, а затем записывают данные обратно в хранилище.  

Следующее руководство применяется только к заданиям с большим объемом операций ввода-вывода.

## <a name="general-considerations"></a>Общие рекомендации

У вас может быть задание, которое считывает или записывает до 100 МБ за одну операцию, но буфер такого размера может снизить производительность.
Для оптимизации производительности попытайтесь сохранить размер операции ввода-вывода в пределах 4–16 МБ.

### <a name="general-considerations-for-an-hdinsight-cluster"></a>Общие рекомендации, связанные с кластером HDInsight

* **Поддерживаемые версии HDInsight.** Для повышения производительности используйте последний выпуск HDInsight.
* **Регионы.** Разместите учетную запись Data Lake Storage 2-го поколения в том же регионе, что и кластер HDInsight.  

Кластер HDInsight An состоит из двух головных узлов и нескольких рабочих узлов. Каждый рабочий узел предоставляет определенное количество ядер и памяти, которые определяют тип виртуальной машины.  При выполнении задания YARN выступает в качестве согласователя ресурсов, который выделяет доступную память и ядра для создания контейнеров.  Каждый контейнер выполняет задачи, которые необходимы для выполнения задания.  Контейнеры выполняются параллельно для быстрой обработки задачи. Таким образом производительность повышается за счет выполнения максимально возможного количества контейнеров параллельно.

В пределах кластера HDInsight имеется три уровня, которые можно настроить, чтобы увеличить число контейнеров и использовать всю доступную пропускную способность.  

* **Физический уровень**
* **Уровень YARN**
* **Уровень рабочей нагрузки**

### <a name="physical-layer"></a>Физический уровень

**Запустите кластер с большим количеством узлов и/или на виртуальной машине большего размера.**  Больший кластер позволит вам выполнять дополнительные контейнеры YARN, как это показано на рисунке ниже.

![Производительность Data Lake Storage 2-го поколения](./media/data-lake-storage-performance-tuning-guidance/VM.png)

**Используйте виртуальные машины с большей пропускной способностью сети.**  Пропускная способность сети может быть узким местом, если она меньше, чем пропускная способность Data Lake Storage 2-го поколения.  У различных виртуальных машин будет разная пропускная способность сети.  Выберите тип виртуальной машины с самой большой пропускной способностью сети.

### <a name="yarn-layer"></a>Уровень YARN

**Используйте контейнеры YARN меньшего размера.**  Уменьшите размер каждого контейнера YARN, чтобы создать больше контейнеров с тем же объемом ресурсов.

![Производительность Data Lake Storage 2-го поколения](./media/data-lake-storage-performance-tuning-guidance/small-containers.png)

В зависимости от рабочей нагрузки минимальный необходимый размер контейнера YARN будет иметься всегда. Если выбрать слишком маленький контейнер, у заданий будут возникать проблемы из-за нехватки памяти. Как правило, контейнеры YARN должны быть размером не менее 1 ГБ. Обычно можно увидеть контейнеры YARN размером 3 ГБ. Для некоторых рабочих нагрузок могут потребоваться контейнеры YARN большего размера.  

**Увеличьте количество ядер на контейнер YARN.**  Увеличьте количество ядер, выделенных для каждого контейнера, чтобы увеличить число параллельных задач, которые выполняются в каждом контейнере.  Это работает для приложений, которые выполняют несколько задач на контейнер, например Spark.  Для приложений, например Hive, которые выполняют один поток в каждом контейнере, лучше иметь несколько контейнеров, а не больше ядер на контейнер.

### <a name="workload-layer"></a>Уровень рабочей нагрузки

**Используйте все доступные контейнеры.**  Задайте число задач равное или большее количества доступных контейнеров, чтобы использовать все ресурсы.

![Производительность Data Lake Storage 2-го поколения](./media/data-lake-storage-performance-tuning-guidance/use-containers.png)

**Незавершенные задачи ресурсоемки.** Если у каждой задачи есть большой объем данных для обработки, неудачное выполнение задачи приводит к дорогостоящей повторной попытке.  Поэтому лучше создавать дополнительные задачи, каждая из которых обрабатывает небольшой объем данных.

Помимо общих рекомендаций, приведенных выше, каждое приложение имеет различные параметры, которые можно настроить для каждого конкретного приложения. В следующей таблице перечислены некоторые параметры и ссылки для начала работы с настройкой производительности для каждого приложения.

| Рабочая нагрузка | Параметры для настройки задач |
|----------|------------------------|
| [Spark в HDInsight](data-lake-storage-performance-tuning-spark.md) | <ul><li>Num-executors</li><li>Executor-memory</li><li>Executor-cores</li></ul> |
| [Hive в HDInsight](data-lake-storage-performance-tuning-hive.md) | <ul><li>hive.tez.container.size</li></ul> |
| [MapReduce в HDInsight](data-lake-storage-performance-tuning-mapreduce.md) | <ul><li>Mapreduce.map.memory</li><li>Mapreduce.job.maps</li><li>Mapreduce.reduce.memory</li><li>Mapreduce.job.reduces</li></ul> |
| [Storm в HDInsight](data-lake-storage-performance-tuning-storm.md)| <ul><li>Количество рабочих процессов</li><li>Количество экземпляров исполнителей воронки</li><li>Количество экземпляров исполнителей сита </li><li>Количество задач воронки</li><li>Количество задач сита</li></ul>|

## <a name="see-also"></a>См. также
* [Общие сведения об Azure Data Lake Storage 2-го поколения](data-lake-storage-introduction.md)
