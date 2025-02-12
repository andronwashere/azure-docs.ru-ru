---
title: Запрос данных из HDFS-совместимой службы хранилища Azure в Azure HDInsight
description: Узнайте, как запрашивать данные из службы хранилища Azure и Azure Data Lake Storage для сохранения результатов анализа.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 04/23/2019
ms.openlocfilehash: 6e0192029decef95dcaecc0c60dce5fd5b6f99ff
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "66479909"
---
# <a name="use-azure-storage-with-azure-hdinsight-clusters"></a>Использование службы хранилища Azure с кластерами Azure HDInsight

Для анализа данных в кластере HDInsight, можно сохранить данные либо в [хранилища Azure](../storage/common/storage-introduction.md), [Azure Data Lake хранилища Gen 1](../data-lake-store/data-lake-store-overview.md)/[Azure Data Lake хранилища Gen 2](../storage/blobs/data-lake-storage-introduction.md), или их сочетание. Эти параметры хранилища позволяют безопасно и удалять кластеры HDInsight, которые используются для вычислений без потери пользовательских данных.

В Apache Hadoop поддерживается концепция файловой системы по умолчанию. Файловая система по умолчанию подразумевает использование центра сертификации и схемы по умолчанию. Она также может использоваться для разрешения относительных путей. При создании кластера HDInsight в качестве файловой системы по умолчанию можно указать контейнер больших двоичных объектов в службе хранилища Azure, а в случае использования HDInsight 3.6 можно выбрать службу хранилища Azure или Azure Data Lake Storage (1-го или 2-го поколения) с несколькими исключениями. Дополнительные сведения о поддержке Data Lake Storage 1-го поколения в качестве связанного хранилища и хранилища по умолчанию см. в руководстве по [обеспечению доступности для кластеров HDInsight](./hdinsight-hadoop-use-data-lake-store.md#availability-for-hdinsight-clusters).

Из этой статьи вы узнаете, как служба хранилища Azure работает с кластерами HDInsight. Чтобы узнать, как Data Lake Storage 1-го поколения работает с кластерами HDInsight, см. статью [Создание кластеров HDInsight, использующих Azure Data Lake Storage, с помощью портала Azure](hdinsight-hadoop-use-data-lake-store.md). Дополнительные сведения о создании кластера HDInsight см. в статье [Установка кластеров в HDInsight с использованием Hadoop, Spark, Kafka и других технологий](hdinsight-hadoop-provision-linux-clusters.md).

Служба хранилища Azure — это надежное, универсальное решение, которое полностью интегрируется с HDInsight. HDInsight может использовать контейнер больших двоичных объектов в службе хранилища Azure в качестве файловой системы по умолчанию для кластера. С помощью интерфейса распределенной файловой системы Hadoop все компоненты HDInsight могут напрямую взаимодействовать со структурированными или неструктурированными данными, хранимыми в виде больших двоичных объектов.

> [!WARNING]  
> Тип учетной записи хранения **BlobStorage** может использоваться только как дополнительное хранилище для кластеров HDInsight.

| Тип учетной записи хранения | Поддерживаемые службы | Поддерживаемые уровни производительности | Поддерживаемые уровни доступа |
|----------------------|--------------------|-----------------------------|------------------------|
| StorageV2 (учетная запись общего назначения версии 2)  | BLOB-объект     | Стандартная                    | "Горячий", "холодный", архивировать\*   |
| Хранилище (общего назначения версии 1)   | BLOB-объект     | Стандартная                    | Н/Д                    |
| BlobStorage                    | BLOB-объект     | Стандартная                    | "Горячий", "холодный", архивировать\*   |

Применяемый по умолчанию контейнер больших двоичных объектов не рекомендуется использовать для хранения бизнес-данных. Чтобы сократить затраты на хранение, контейнер больших двоичных объектов по умолчанию рекомендуется удалять после каждого использования. Контейнер по умолчанию содержит журналы приложений и системный журнал. Обязательно извлеките эти журналы перед удалением контейнера.

Совместное использование одного контейнера больших двоичных объектов в качестве файловой системы по умолчанию для нескольких кластеров не поддерживается.

> [!NOTE]  
> Архивный уровень доступа — это автономный уровень с задержкой извлечения в несколько часов, который не рекомендуется использовать вместе с HDInsight. Дополнительные сведения см. в разделе [Архивный уровень доступа](../storage/blobs/storage-blob-storage-tiers.md#archive-access-tier).

Если выбран для защиты вашей учетной записи хранения с **брандмауэры и виртуальные сети** ограничения на **выбранные сети**, не забудьте включить исключения **Разрешить надежные Microsoft службы...**  HDInsight можно получить доступ к вашей учетной записи хранения.

## <a name="hdinsight-storage-architecture"></a>Архитектура хранилища HDInsight
Следующая схема является абстрактным представлением архитектуры хранилища HDInsight с использованием службы хранилища Azure.

![Кластеры Hadoop используют API HDFS для доступа к структурированным и неструктурированным данным и их хранения в хранилище BLOB-объектов.](./media/hdinsight-hadoop-use-blob-storage/HDI.WASB.Arch.png "Архитектура хранилища HDInsight")

HDInsight предоставляет доступ к распределенной файловой системе, которая локально присоединена к вычислительным узлам. Доступ к этой файловой системе может осуществляться с использованием полного универсального кода ресурса (URI), например:

    hdfs://<namenodehost>/<path>

Кроме того, HDInsight позволяет получить доступ к данным, содержащимся в службе хранилища Azure. Синтаксис:

    wasb://<containername>@<accountname>.blob.core.windows.net/<path>

Ниже приведены некоторые рекомендации для использования учетной записи хранения Azure с кластерами HDInsight.

* **Контейнеры в учетных записях хранения, подключенных к кластеру.** Так как имя учетной записи и ключ связываются с кластером во время создания, вы получаете полный доступ к большим двоичным объектам в этих контейнерах.

* **Общедоступные контейнеры или общедоступные большие двоичные объекты в учетных записях хранения, не подключенных к кластеру.** У вас есть разрешение только для чтения к большим двоичным объектам в контейнерах.
  
> [!NOTE]  
> Общедоступные контейнеры позволяют получить список всех доступных в этом контейнере BLOB-объектов, а также метаданные контейнера. Общедоступные BLOB-объекты позволяют получить доступ к BLOB-объектам только при условии, что вам известен точный URL-адрес. Дополнительные сведения см. в статье [Управление анонимным доступом на чтение к контейнерам и большим двоичным объектам](../storage/blobs/storage-manage-access-to-resources.md).

* **Частные контейнеры в учетных записях хранения, не подключенных к кластеру.** Вы не можете получить доступ к большим двоичным объектам в контейнерах, если не определите учетную запись хранения при отправке заданий WebHCat. Это объясняется далее в статье.

Учетные записи хранения, которые определены в процесс создания и их ключи хранятся в `%HADOOP_HOME%/conf/core-site.xml` на узлах кластера. По умолчанию HDInsight будет использовать учетные записи хранения, описанные в файле core-site.xml. Этот параметр можно изменить с помощью [Apache Ambari](./hdinsight-hadoop-manage-ambari.md).

Множество заданий WebHCat, включая Apache Hive, MapReduce, потоковую передачу Apache Hadoop и Apache Pig, могут переносить описание учетных записей хранения и метаданные вместе с ними. (В настоящее время эта функция работает для Pig с учетными записями хранения, но не с метаданными). Дополнительные сведения см. в разделе [Использование кластера HDInsight с дополнительными учетными записями хранения и метахранилищами](https://social.technet.microsoft.com/wiki/contents/articles/23256.using-an-hdinsight-cluster-with-alternate-storage-accounts-and-metastores.aspx).

Большие двоичные объекты могут использоваться для хранения как структурированных, так и неструктурированных данных. В контейнерах больших двоичных объектов данные хранятся в виде пар "ключ — значение" и отсутствует иерархия каталогов. Тем не менее, в имени ключа может использоваться знак косой черты "/", чтобы оно выглядело так, будто файл хранится в структуре каталогов. Например, ключ BLOB-объекта может выглядеть следующим образом: *input/log1.txt*. На самом деле никакого каталога *input* не существует, но из-за наличия знака косой черты "/" имя ключа выглядит как путь к файлу.

## <a id="benefits"></a>Преимущества службы хранилища Azure
Предполагаемые рабочие затраты не совместное размещение ресурсов хранилища и вычислительных кластеров устраняется с помощью вычислительных кластеров создавались близко к использующим ресурсов учетной записи хранения в регионе Azure, в котором Высокоскоростная сеть обеспечивает эффективный способ вычислительные узлы для доступа к данным в службе хранилища Azure.

Ниже перечислены некоторые преимущества, связанные с хранением данных в службе хранилища Azure (вместо HDFS).

* **Повторное использование данных и общий доступ.** Данные в файловой системе HDFS расположены в вычислительном кластере. Только приложения, имеющие доступ к вычислительному кластеру, могут использовать данные через API HDFS. Данные в службе хранилища Azure может осуществляться через интерфейсы API HDFS или через [интерфейсы REST API хранилища BLOB-объектов](https://docs.microsoft.com/rest/api/storageservices/Blob-Service-REST-API). Таким образом, для создания и использования данных можно применять больший набор приложений (включая другие кластеры HDInsight) и средств.

* **Архивация данных.** Хранение данных в службе хранилища Azure позволяет безопасно (без потери пользовательских данных) удалять используемые для расчетов кластеры HDInsight.

* **Затраты на хранение данных.** Хранение данных в файловой системе DFS в долгосрочной перспективе является более затратным, чем хранение данных в службе хранилища Azure, так как стоимость вычислительного кластера превышает стоимость службы хранилища Azure. Кроме того, поскольку данные не требуется повторно загружать при создании каждого вычислительного кластера, вы экономите также на загрузке данных.

* **Гибкое горизонтальное масштабирование.** Хотя HDFS и представляет собой масштабируемую файловую систему, масштаб определяется количеством узлов, создаваемых для кластера. Изменение масштаба может оказаться более сложным процессом, чем использование гибких возможностей масштабирования службы хранилища Azure, которые вы получаете автоматически.

* **Георепликация.** Доступна функция георепликации для вашей службы хранилища Azure. Хотя это обеспечивает возможность географического восстановления и избыточность данных, переход в расположение геореплицированных данных при отработке отказа заметно сказывается на производительности, что может привести к дополнительным затратам. Поэтому мы рекомендуем взвешенно подходить к выбору георепликации и выбирать ее только в том случае, если ценность данных окупит дополнительные затраты.

Определенные задания и пакеты MapReduce могут создавать промежуточные результаты, которые нет нужды хранить в службе хранилища Azure. В таком случае можно выбрать хранение данных в локальной системе HDFS. На деле HDInsight использует DFS для некоторых таких промежуточных результатов в заданиях Hive и других процессах.

> [!NOTE]  
> Большинство команд HDFS (например, `ls`, `copyFromLocal` и `mkdir`) по-прежнему работают правильно. В службе хранилища Azure будет отличаться поведение только тех команд, которые относятся к стандартной реализации HDFS (под названием DFS), например `fschk` и `dfsadmin`.

## <a name="address-files-in-azure-storage"></a>Обращение к файлам в службе хранилища Azure
Схема URI для доступа к файлам в службе хранилища Azure из HDInsight:

```config
wasb://<BlobStorageContainerName>@<StorageAccountName>.blob.core.windows.net/<path>
```

Эта схема URI предоставляет как незашифрованный доступ с префиксом *wasb:* , так и доступ с использованием SSL-шифрования с *wasbs*. Мы рекомендуем использовать *wasbs* всегда, когда это возможно, даже при обращении к данным, которые хранятся в том же регионе Azure.

`<BlobStorageContainerName>` Определяет имя контейнера больших двоичных объектов в службе хранилища Azure.
`<StorageAccountName>` Определяет имя учетной записи хранения Azure. Обязательно использовать полное доменное имя (FQDN).

Если ни один из `<BlobStorageContainerName>` , ни `<StorageAccountName>` был указан, используется файловая система по умолчанию. Для файлов в файловой системе по умолчанию можно использовать относительный или абсолютный путь. Например, для ссылки на файл *hadoop-mapreduce-examples.jar* , который поставляется с кластерами HDInsight, можно использовать один из следующих вариантов:

```config
wasb://mycontainer@myaccount.blob.core.windows.net/example/jars/hadoop-mapreduce-examples.jar
wasb:///example/jars/hadoop-mapreduce-examples.jar
/example/jars/hadoop-mapreduce-examples.jar
```

> [!NOTE]  
> В кластерах HDInsight версий 2.1 и 1.6 файл называется `hadoop-examples.jar`.

Путь является имя путь к файлу или каталогу HDFS. Так как контейнеры службы хранилища Azure содержат данные типа "ключ — значение", подлинная иерархическая файловая система в них отсутствует. Знак косой черты "/" в ключе BLOB-объекта интерпретируется как разделитель каталогов. Например, имя BLOB-объекта для файла *hadoop-mapreduce-examples.jar* выглядит следующим образом:

```bash
example/jars/hadoop-mapreduce-examples.jar
```

> [!NOTE]  
> При работе с большими двоичными объектами вне HDInsight большинство программ не распознают формат WASB и вместо этого ожидают формат базового пути, например `example/jars/hadoop-mapreduce-examples.jar`.

##  <a name="blob-containers"></a>Контейнеры больших двоичных объектов
Чтобы использовать большие двоичные объекты, сначала нужно создать [учетной записи хранения Azure](../storage/common/storage-create-storage-account.md). В рамках этого процесса укажите регион Azure, в котором создается учетная запись хранения. Кластер и учетная запись хранения должны размещаться в одном регионе. База данных SQL Server хранилища метаданных Hive и база данных SQL Server хранилища метаданных Apache Oozie также должны располагаться в одном регионе.

Где бы ни находился созданный BLOB-объект, он принадлежит контейнеру в вашей учетной записи хранения Azure. Этим контейнером может быть существующий контейнер хранилища BLOB-объектов, созданный вне HDInsight, или контейнер, созданный для кластера HDInsight.

Стандартный контейнер больших двоичных объектов хранит сведения о кластере, включая журналы заданий. Не используйте стандартный контейнер BLOB-объектов с несколькими кластерами HDInsight. Это может привести к искажению истории заданий. С разными кластерами рекомендуется использовать разные контейнеры, размещая общие данные в связанной учетной записи хранения, которая указывается при развертывании всех соответствующих кластеров. Использовать учетную запись хранения по умолчанию не рекомендуется. Дополнительные сведения о настройке связанных учетных записей хранения см. в разделе [кластеров HDInsight, создайте](hdinsight-hadoop-provision-linux-clusters.md). Тем не менее, вы можете повторно использовать контейнер хранения по умолчанию после удаления исходного кластера HDInsight. Для кластеров HBase можно фактически сохранить все данные и схемы таблицы HBase, создав кластер HBase с помощью контейнера больших двоичных объектов по умолчанию, который используется удаленным кластером HBase.

[!INCLUDE [secure-transfer-enabled-storage-account](../../includes/hdinsight-secure-transfer.md)]

## <a name="interacting-with-azure-storage"></a>Взаимодействие со службой хранилища Azure

Корпорация Майкрософт предоставляет следующие средства для работы со службой хранилища Azure:

| Средство | Linux | OS X | Windows |
| --- |:---:|:---:|:---:|
| [портал Azure](../storage/blobs/storage-quickstart-blobs-portal.md) |✔ |✔ |✔ |
| [Интерфейс командной строки Azure](../storage/blobs/storage-quickstart-blobs-cli.md) |✔ |✔ |✔ |
| [Azure PowerShell](../storage/blobs/storage-quickstart-blobs-powershell.md) | | |✔ |
| [AzCopy](../storage/common/storage-use-azcopy-v10.md) |✔ | |✔ |

## <a name="use-additional-storage-accounts"></a>Использование дополнительных учетных записей хранения

При создании кластера HDInsight укажите учетную запись хранения Azure, которую необходимо с ним связать. Помимо этой учетной записи хранения в процессе создания или после создания кластера можно добавить дополнительные учетные записи хранения из той же или других подписок Azure. Инструкции по добавлению дополнительных учетных записей хранения см. в статье [Создание кластеров Hadoop в HDInsight](hdinsight-hadoop-provision-linux-clusters.md).

> [!WARNING]  
> Использование дополнительной учетной записи хранения, расположение которой отличается от расположения кластера HDInsight, не поддерживается.

## <a name="next-steps"></a>Дальнейшие действия

Из этой статьи вы узнали, как использовать HDFS-совместимую службу хранилища Azure с HDInsight. Это позволяет создавать масштабируемые, долгосрочные решения для получения данных архивирования, а также использовать HDInsight для разблокирования информации внутри хранимых структурированных и неструктурированных данных.

Дополнительные сведения можно найти в разделе

* [Приступая к работе с Azure HDInsight](hadoop/apache-hadoop-linux-tutorial-get-started.md)
* [Начало работы с Azure Data Lake Storage](../data-lake-store/data-lake-store-get-started-portal.md)
* [Отправка данных в HDInsight](hdinsight-upload-data.md)
* [Использование Hive и HiveQL с Hadoop в HDInsight для анализа примера файла Apache log4j](hadoop/hdinsight-use-hive.md)
* [Использование Pig с Hadoop в HDInsight](hadoop/hdinsight-use-pig.md)
* [Использовать хранилище подписанных Azure для ограничения доступа к данным с помощью HDInsight](hdinsight-storage-sharedaccesssignature-permissions.md)
* [Использование Azure Data Lake Storage Gen2 с кластерами Azure HDInsight](hdinsight-hadoop-use-data-lake-storage-gen2.md)