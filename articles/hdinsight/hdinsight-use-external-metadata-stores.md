---
title: Использование внешних хранилищ метаданных в Azure HDInsight
description: Использование внешних хранилищ метаданных с кластерами HDInsight.
author: hrasheed-msft
ms.reviewer: jasonh
ms.author: hrasheed
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 05/27/2019
ms.openlocfilehash: 705ced82ad4edad0bb4adc057414f6b20b80d8d3
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "66298873"
---
# <a name="use-external-metadata-stores-in-azure-hdinsight"></a>Использование внешних хранилищ метаданных в Azure HDInsight

Хранилище метаданных Apache Hive в HDInsight является важной частью архитектуры Apache Hadoop. Хранилище метаданных — это центральный репозиторий схемы, который могут использовать другие инструменты для доступа к большим данным, такие как Apache Spark, Interactive Query (LLAP), Presto или Apache Pig. HDInsight использует базу данных SQL Azure в качестве хранилища метаданных Hive.

![Архитектура хранилища метаданных Hive HDInsight](./media/hdinsight-use-external-metadata-stores/metadata-store-architecture.png)

Есть два способа настроить хранилище метаданных для кластеров HDInsight:

* [Хранилище метаданных по умолчанию](#default-metastore).
* [Пользовательское хранилище метаданных](#custom-metastore).

## <a name="default-metastore"></a>Хранилище метаданных по умолчанию

По умолчанию HDInsight создает хранилище метаданных с каждым типом кластера. Вместо этого можно указать пользовательское хранилище метаданных. При использовании хранилища метаданных по умолчанию учитывайте следующее:
- Дополнительные затраты не требуются. HDInsight создает хранилище метаданных с каждым типом кластера без дополнительных затрат с вашей стороны.
- Каждое хранилище метаданных по умолчанию является частью жизненного цикла кластера. При удалении кластера соответствующее хранилище метаданных и метаданные также удаляются.
- Хранилище метаданных по умолчанию не может совместно использоваться с другими кластерами.
- Хранилище метаданных по умолчанию использует стандартную базу данных SQL Azure с ограничением в пять DTU (единица транзакций базы данных).
Это хранилище метаданных по умолчанию обычно используется для относительно простых рабочих нагрузок, которым не требуется несколько кластеров, а метаданные не нужно сохранять за пределами жизненного цикла кластера.


## <a name="custom-metastore"></a>Пользовательское хранилище метаданных

HDInsight также поддерживает пользовательские хранилища метаданных, которые рекомендуется использовать для кластеров в рабочей среде:
- В качестве хранилища метаданных указывается собственная база данных SQL Azure.
- Жизненный цикл хранилища метаданных не привязан к жизненному циклу кластеров, поэтому вы можете создавать и удалять кластеры без потери метаданных. Метаданные, такие как схемы Hive, сохраняются даже после удаления и повторного создания кластера HDInsight.
- Пользовательское хранилище метаданных позволяет присоединять к этому хранилищу метаданных кластеры различных типов. Например, одно хранилище метаданных может совместно использоваться в кластерах Interactive Query, Hive и Hive в HDInsight.
- Вы оплачиваете стоимость хранилища метаданных (База данных SQL Azure) в соответствии с выбранным уровнем производительности.
- При необходимости хранилище метаданных можно масштабировать.

![Вариант использования хранилища метаданных Hive HDInsight](./media/hdinsight-use-external-metadata-stores/metadata-store-use-case.png)


### <a name="select-a-custom-metastore-during-cluster-creation"></a>Выбор пользовательского хранилища метаданных во время создания кластера

При создании кластеру можно указать ранее созданную базу данных Azure SQL или настроить базу данных SQL после его создания. Этот параметр указан с **хранилища > Параметры хранилища метаданных** при создании нового Hadoop, Spark или интерактивного кластера Hive на портале Azure.

![Хранилище метаданных Hive HDInsight на портале Azure](./media/hdinsight-use-external-metadata-stores/metadata-store-azure-portal.png)

Кроме того, вы можете добавлять дополнительные кластеры из пользовательского хранилища метаданных с портала Azure или из конфигураций Ambari ("Hive > Расширенный").

![Хранилище метаданных Ambari Hive HDInsight](./media/hdinsight-use-external-metadata-stores/metadata-store-ambari.png)

## <a name="hive-metastore-best-practices"></a>Рекомендации по хранилищу метаданных Hive

Ниже приведены некоторые распространенные рекомендации по хранилищу метаданных Hive HDInsight:

- По возможности используйте пользовательское хранилище метаданных, так как это поможет разделить вычислительные ресурсы в выполняемом кластере и метаданные из хранилища метаданных.
- Начните с уровня S2, предоставляющего 50 DTU и хранилище размером 250 ГБ. Если вы видите узкое место, можно увеличить масштаб базы данных.
- Если требуется несколько кластеров HDInsight для доступа к отдельным данным, используйте отдельную базу данных для хранилища метаданных для каждого кластера. Если хранилище метаданных используется совместно в нескольких кластерах HDInsight, это означает, что кластеры используют одинаковые метаданные и базовые файлы данных пользователей.
- Периодически архивируйте пользовательское хранилище метаданных. База данных SQL Azure автоматически создает резервные копии, но период их хранения различается. Дополнительные сведения см. в статье [Подробнее об автоматическом резервном копировании базы данных SQL](../sql-database/sql-database-automated-backups.md).
- Найдите свое хранилище метаданных и кластер HDInsight в одном регионе для максимальной производительности и минимальной платы за исходящий трафик сети.
- Мониторинг производительности и доступности, с помощью средств мониторинга базы данных SQL Azure, таких как портал Azure или Azure Monitor журналы хранилища метаданных.
- Если создается новая версия Azure HDInsight для существующей пользовательской базы данных хранилища метаданных, система обновляет схему хранилища метаданных, и это необратимая операция без восстановления базы данных из резервной копии.
- Если вы совместно используете хранилище метаданных в нескольких кластерах, убедитесь, что все кластеры имеют одну и ту же версию HDInsight. Различные версии Hive используют разные версии базы данных хранилища метаданных. Например, хранилище метаданных нельзя совместно использовать с кластерами Hive 1.2 и Hive 2.1. 

##  <a name="apache-oozie-metastore"></a>Хранилище метаданных Apache Oozie

Apache Oozie — это система координации рабочих процессов, которая управляет заданиями Hadoop.  Эта система поддерживает задания Hadoop для Apache MapReduce, Pig, Hive и т. д.  Oozie использует хранилище метаданных, чтобы сохранять сведения о текущих и завершенных рабочих процессах. Для повышения производительности Oozie можно использовать Базу данных SQL Azure в качестве хранилища метаданных. Хранилище метаданных также позволяет осуществлять доступ к данным задания Oozie после удаления кластера.

Дополнительные инструкции по созданию хранилища метаданных Oozie с базой данных SQL Azure см. в статье [Использование Oozie с Hadoop для определения и запуска рабочих процессов в Azure HDInsight под управлением Linux](hdinsight-use-oozie-linux-mac.md).

## <a name="next-steps"></a>Дальнейшие действия

- [Установка кластеров в HDInsight с использованием Apache Hadoop, Apache Spark, Apache Kafka и других технологий](./hdinsight-hadoop-provision-linux-clusters.md)
