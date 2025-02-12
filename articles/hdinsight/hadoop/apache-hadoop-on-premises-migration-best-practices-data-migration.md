---
title: Миграция локальных кластеров Apache Hadoop в HDInsight Azure — рекомендации по использованию миграции данных
description: Ознакомьтесь с рекомендациями по использованию архитектуры в рамках миграции локальных кластеров Hadoop в Azure HDInsight.
author: hrasheed-msft
ms.reviewer: ashishth
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 04/08/2019
ms.author: hrasheed
ms.openlocfilehash: 732cb118b7a0eebdbf28c7d7fe6ced435ce7920e
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "64713721"
---
# <a name="migrate-on-premises-apache-hadoop-clusters-to-azure-hdinsight---data-migration-best-practices"></a>Миграция локальных кластеров Apache Hadoop в HDInsight Azure — рекомендации по использованию миграции данных

В этой статье представлены рекомендации по миграции данных в Azure HDInsight. Это часть цикла, где приведены лучшие методики, применимые при перемещении локальных систем Apache Hadoop в Azure HDInsight.

## <a name="migrate-on-premises-data-to-azure"></a>Перенос локальных данных в Azure

Существует два основных способа для миграции данных из локальной среды Azure.

1.  Передача данных по сети с помощью протокола TLS
    1. Интернет. Данные можно перенести в службу хранилища Azure, используя обычное интернет-подключение и любое из средств, таких как Обозреватель службы хранилища Azure, AzCopy, Azure Powershell и Azure CLI.  Дополнительные сведения см. в руководстве по [перемещению данных в службу хранилища Azure и обратно](../../storage/common/storage-moving-data.md).
    2. ExpressRoute. ExpressRoute — это служба Azure, которая позволяет создавать частные подключения между центрами обработки данных Microsoft и инфраструктурой вашей локальной среды или среды для совместной работы. Подключения ExpressRoute не осуществляются через общедоступный Интернет и обеспечивают повышенный уровень безопасности, надежности и быстродействия с низким уровнем задержки по сравнению с обычными подключениями через Интернет. Дополнительные сведения см. в руководстве по [созданию и изменению канала ExpressRoute](../../expressroute/expressroute-howto-circuit-portal-resource-manager.md).
    1. Data Box. Data Box Edge и Шлюз Data Box — это продукты для сетевой передачи данных, которые работают подобно шлюзам сетевого хранилища для управления данными между сайтом и Azure. Data Box Edge (локальное сетевое устройство) передает данные в Azure и обратно и использует вычисления Edge с поддержкой искусственного интеллекта (ИИ) для обработки данных. Шлюз Azure Data Box — это виртуальный модуль с возможностями шлюза хранилища. Дополнительные сведения см. в документации по [сетевой передаче данных с помощью Azure Data Box](https://docs.microsoft.com/azure/databox-online/).
1.  Отправка данных в автономном режиме
    1. Data Box. Устройства Data Box, Диск Data Box и Data Box Heavy позволяют передавать большие объемы данных в Azure, когда отсутствует доступ к сети. Эти устройства для автономной передачи данных отправляются из организации в центр обработки данных Azure и обратно. Они используют шифрование AES для защиты передаваемых данных и проходят тщательный процесс очистки после отправки для удаления данных с устройства. Дополнительные сведения о Data Box автономный перенос устройств, см. в разделе [Azure документации по Data Box — автономный перенос](https://docs.microsoft.com/azure/databox/). Дополнительные сведения о миграции кластеров Hadoop, см. в разделе [использование Azure Data Box для переноса из локального хранилища HDFS в хранилище Azure](../../storage/blobs/data-lake-storage-migrate-on-premises-hdfs-cluster.md).

В следующей таблице приведены приблизительные данные времени передачи, в основе которых лежат данные тома и пропускная способность сети. Используйте Data Box, если предполагается, что миграция данных займет более трех недель.

|**Количество данных**|**Сеть Bandwidth**||||
|---|---|---|---|---|
|| **45 Мбит/с (T3)**|**100 Мбит/с**|**1 Гбит/с**|**10 Гбит/с**|
|1 ТБ|2 дня|1 день| 2 ч|14 минут|
|10 ТБ|22 дня|10 дней|1 день|2 ч|
|35 ТБ|76 дней|34 дня|3 дня|8 ч|
|80 ТБ|173 дня|78 дней|8 дней|19 часов|
|100 ТБ|216 дней|97 дней|10 дней|1 день|
|200 ТБ|Год|194 дня|19 дней|2 дня|
|500 TБ|3 года|Год|49 дней|5 дней|
|1 ПБ|6 год|3 года|97 дней|10 дней|
|2 ПБ|12 год|5 год|194 дня|19 дней|

Средства, встроенные в Azure (например, Apache Hadoop DistCp, Фабрика данных Azure и AzureCp), можно использовать для передачи данных по сети. Независимый инструмент WANDisco можно также использовать с той же целью. Apache Kafka Mirrormaker и Apache Sqoop можно использовать для передачи текущих данных из локальной системы хранения Azure.


## <a name="performance-considerations-when-using-apache-hadoop-distcp"></a>Рекомендации по производительности при использовании Apache Hadoop DistCp


DistCp — проект Apache, который использует задание сопоставления MapReduce для передачи данных, обработки и восстановления ошибок. Для каждой задачи сопоставления он назначает список исходных файлов. Задача сопоставления копирует все присвоенные файлы в назначение. Существует несколько методов, которые позволяют повысить производительность DistCp.

### <a name="increase-the-number-of-mappers"></a>Увеличение количества модулей сопоставления

DistCp пытается создать задачу сопоставления таким образом, чтобы каждый из них копировал примерно одинаковое число байтов. По умолчанию задания DistCp используют 20 модулей сопоставления. Использование большего количества модулей сопоставления для Distcp (с параметром "m" в командной строке) увеличивает параллелизм во время процесса передачи данных и уменьшает объем передаваемых данных. При увеличении числа модулей сопоставления необходимо учитывать следующие две вещи.

1. Наименьшая степень детализации DistCp — один файл. Указывать количество модулей сопоставления больше, чем количество исходных файлов, бесполезно, и будут тратиться доступные ресурсы кластера.
1. Чтобы определить количество модулей сопоставления, рассмотрите доступную память Yarn в кластере. Каждая задача сопоставления запускается как контейнер Yarn. Предположим, что в кластере выполняются не самые ресурсоемкие приложения, число модулей сопоставления можно определить, используя следующую формулу: m = (число рабочих узлов \* памяти YARN для каждого рабочего узла) / размер контейнера YARN. Если другие приложения используют память, то для заданий DistCp можно использовать только часть памяти YARN.

### <a name="use-more-than-one-distcp-job"></a>Использование нескольких заданий DistCp

Используйте несколько заданий DistCp в том случае, если размер перемещаемого набора данных больше 1 ТБ. Использование более одного задания сокращает влияние сбоев. В случае сбоя любого задания необходимо перезапустить только это задание.

### <a name="consider-splitting-files"></a>Рассмотрим возможность разделения файлов

При наличии небольшого количества больших файлов попробуйте разбить их на фрагменты по 256 МБ, чтобы с помощью модулей сопоставления получить повышения степени параллелизма.

### <a name="use-the-strategy-command-line-parameter"></a>Использование "стратегии" параметров командной строки

Используйте параметр `strategy = dynamic` в командной строке. Значение параметра `strategy` по умолчанию — `uniform size`, в этом случае каждое сопоставление копирует примерно одинаковое число байтов. Если этот параметр изменяется на `dynamic`. Файл листинга разбивается на несколько блоков файлов. Количество блоков файлов — это любое количество сопоставлений. Каждой задачи сопоставления назначается один из блоков файлов. После обработки всех путей в блоке текущий блок удаляется, а новый — запрашивается. Процесс продолжается до тех пор, пока доступны блоки. Этот "динамический" подход позволяет быстрым задачам сопоставления использовать другие пути, в отличие от медленных, которые тем самым ускоряют общее задание DistCp.

### <a name="increase-the-number-of-threads"></a>Увеличение количества потоков

См., улучшается ли производительность при увеличении параметра `-numListstatusThreads`. Этот параметр управляет количеством потоков, используемых для создания списка файлов (максимальное значение — 40).

### <a name="use-the-output-committer-algorithm"></a>Использование алгоритма разработчика выходных данных

См., улучшается ли производительность при передачи параметра `-Dmapreduce.fileoutputcommitter.algorithm.version=2`. Этот алгоритм разработчика выходных данных выполняет оптимизацию написания выходных файлов в назначение. Ниже приведен пример команды, в котором показано использование различных параметров.

```bash
hadoop distcp -Dmapreduce.fileoutputcommitter.algorithm.version=2 -numListstatusThreads 30 -m 100 -strategy dynamic hdfs://nn1:8020/foo/bar wasb://<container_name>@<storage_account_name>.blob.core.windows.net/foo/
```

## <a name="metadata-migration"></a>Миграция метаданных

### <a name="apache-hive"></a>Apache Hive

Хранилище метаданных Hive можно перенести с помощью сценариев или с помощью репликации базы данных.

#### <a name="hive-metastore-migration-using-scripts"></a>Миграция хранилища метаданных Hive с помощью сценариев

1. Создание DDL Hive из в локальной среде хранилища метаданных Hive. Это действие можно выполнить, используя [оболочку скрипта Bash](https://github.com/hdinsight/hdinsight.github.io/blob/master/hive/hive-export-import-metastore.md).
1. Измените сформированный код DDL, чтобы заменить URL-адрес HDFS URL-адресами WASB/ADLS/ABFS.
1. Запустите обновленный код DDL в хранилище метаданных из кластера HDInsight.
1. Убедитесь, что версия хранилища метаданных Hive совместима с локальной средой и облаком.

#### <a name="hive-metastore-migration-using-db-replication"></a>Миграция хранилища метаданных Hive с помощью репликации базы данных

- Настройте репликацию базы данных между локальным хранилищем метаданных Hive DB и хранилищем метаданных HDInsight DB.
- Используйте Hive MetaTool, чтобы заменить URL-адрес HDFS URL-адресами WASB/ADLS/ABFS (пример см. ниже).

```bash
./hive --service metatool -updateLocation hdfs://nn1:8020/ wasb://<container_name>@<storage_account_name>.blob.core.windows.net/
```

### <a name="apache-ranger"></a>Apache Ranger

- Экспортируйте локальные политики Ranger для XML-файлов.
- Преобразование на локальном компьютере конкретный путь на базе HDFS к WASB/ADLS, с помощью такого средства, как XSLT.
- Импортируйте политики Range, запущенные на HDInsight

## <a name="next-steps"></a>Дальнейшие действия

Прочитайте следующую статью в этом цикле:

- [Миграция локальных кластеров Apache Hadoop в HDInsight Azure — рекомендации по настройке безопасности и использованию DevOps](apache-hadoop-on-premises-migration-best-practices-security-devops.md).
