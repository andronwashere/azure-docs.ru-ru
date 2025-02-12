---
title: Включение дампов кучи для служб Apache Hadoop в HDInsight в Azure
description: Включение дампов кучи для служб Apache Hadoop с кластеров HDInsight под управлением Linux для отладки и анализа.
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 02/27/2018
ms.author: hrasheed
ms.openlocfilehash: 62c9dcc039c68b0b6c8b8bf29ed9f13f88936723
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "67059504"
---
# <a name="enable-heap-dumps-for-apache-hadoop-services-on-linux-based-hdinsight"></a>Включение дампов кучи для служб Apache Hadoop в HDInsight под управлением Linux

[!INCLUDE [heapdump-selector](../../includes/hdinsight-selector-heap-dump.md)]

Дампы кучи содержат снимок памяти приложения, включая значения переменных на момент создания дампа. Поэтому они полезны для диагностики проблем, возникающих во время выполнения.

## <a name="whichServices"></a>Службы

Вы можете включить дампы кучи для следующих служб:

* **Apache Hcatalog** — tempelton;
* **Apache Hive** — hiveserver2, metastore, derbyserver;
* **Mapreduce** — jobhistoryserver;
* **Apache Yarn** — resourcemanager, nodemanager, timelineserver;
* **Apache HDFS** — datanode, secondarynamenode, namenode.

Вы можете также включить дампы кучи для процессов сопоставления и уменьшения, запущенных HDInsight.

## <a name="configuration"></a>Основные сведения о настройке дампа кучи

Дампы кучи включаются путем передачи параметров в виртуальную машину Java при запуске службы. Чтобы передать эти параметры, для большинства служб [Apache Hadoop](https://hadoop.apache.org/) можно изменить скрипт оболочки, используемый для запуска службы.

В каждом сценарии есть экспорт для **\*\_OPTS**, который содержит параметры, передаваемые в виртуальную машину Java. Например, в сценарии **hadoop-env.sh** строка, начинающаяся с `export HADOOP_NAMENODE_OPTS=`, содержит параметры для службы NameNode.

Процессы сопоставления и уменьшения немного отличаются друг от друга, поскольку эти операции являются дочерним процессом службы MapReduce. Каждый процесс сопоставления или уменьшения запускается в дочернем контейнере и существуют две записи, содержащие параметры виртуальной машины Java. Оба содержатся в файле **mapred-site.xml**:

* **MapReduce.Admin.map.child.Java.opts**
* **mapreduce.admin.reduce.child.java.opts**

> [!NOTE]  
> Рекомендуется использовать платформу [Apache Ambari](https://ambari.apache.org/) для изменения скриптов и параметров mapred-site.xml, так как Ambari обрабатывает реплицируемые изменения по всем узлам в кластере. Подробные сведения см. в разделе об [использовании Apache Ambari](#using-apache-ambari).

### <a name="enable-heap-dumps"></a>Включение дампов кучи

Следующий параметр включает дампы кучи при возникновении ошибки OutOfMemoryError:

    -XX:+HeapDumpOnOutOfMemoryError

Знак **+** означает, что этот параметр включен. По умолчанию — отключен.

> [!WARNING]  
> Дампы кучи для службы Hadoop в HDInsight не включены по умолчанию, поскольку файлы дампов могут быть очень большими. Включив их для устранения неполадок, не забудьте потом отключить, когда проблема будет воспроизведена и собраны файлы дампа.

### <a name="dump-location"></a>Расположение дампа

По умолчанию файл дампа располагается в текущем рабочем каталоге. Место сохранения файла можно выбрать, используя следующий параметр:

    -XX:HeapDumpPath=/path

Например, с помощью `-XX:HeapDumpPath=/tmp` задается место для сохранения дампов в каталоге /tmp.

### <a name="scripts"></a>Скрипты

Вы можете также запустить сценарий при возникновении ошибки **OutOfMemoryError** . Например, можно запустить уведомление, благодаря которому можно будет узнать, что произошла ошибка. При возникновении ошибки __OutOfMemoryError__ используйте следующий параметр для запуска сценария:

    -XX:OnOutOfMemoryError=/path/to/script

> [!NOTE]  
> Так как Apache Hadoop является распределенной системой, каждый используемый скрипт необходимо помещать во все узлы кластера, на которых запущена служба.
> 
> Сценарий также должен быть в расположении, доступном из учетной записи, с использованием которой запущена служба, и необходимо предоставить разрешение на выполнение. Например, вы можете сохранить сценарии в `/usr/local/bin` и использовать `chmod go+rx /usr/local/bin/filename.sh` для предоставления прав на чтение и выполнение.

## <a name="using-apache-ambari"></a>Использование Apache Ambari

Чтобы изменить конфигурацию службы, выполните следующие действия.

1. Откройте веб-интерфейс Ambari для вашего кластера. Введите URL-адрес https://YOURCLUSTERNAME.azurehdinsight.net.

    В ответ на запрос войдите на сайт с помощью имени учетной записи HTTP (по умолчанию — admin) и пароля для вашего кластера.

   > [!NOTE]  
   > Может потребоваться еще раз ввести имя пользователя и пароль по запросу Ambari. В этом случае введите те же имя учетной записи и пароль.

2. Используя список слева, выберите область службы, которую требуется изменить. Например, **HDFS**. В центральной области выберите вкладку **Конфигурации** .

    ![Сеть Ambari с выбранной вкладкой конфигурации HDFS](./media/hdinsight-hadoop-heap-dump-linux/serviceconfig.png)

3. Используя запись **Фильтр...** , введите **параметры**. Отобразятся только те элементы, в которых есть этот текст.

    ![Фильтрованный список](./media/hdinsight-hadoop-heap-dump-linux/filter.png)

4. Найдите запись **\*\_OPTS** службы, для которой нужно включить дампы кучи, и добавьте параметры, которые требуется включить. На изображении ниже вы можете увидеть, что я добавил `-XX:+HeapDumpOnOutOfMemoryError -XX:HeapDumpPath=/tmp/` в запись **HADOOP\_NAMENODE\_OPTS**:

    ![HADOOP_NAMENODE_OPTS с -XX:+HeapDumpOnOutOfMemoryError -XX:HeapDumpPath=/tmp/](./media/hdinsight-hadoop-heap-dump-linux/opts.png)

   > [!NOTE]  
   > При включении дампов кучи для дочерних процессов сопоставления и уменьшения найдите поля **mapreduce.admin.map.child.java.opts** и **mapreduce.admin.reduce.child.java.opts**.

    Используйте кнопку **Сохранить** , чтобы сохранить изменения. Вы можете ввести краткое примечание, описывающее изменения.

5. После применения изменений появится значок **Требуется перезапуск** рядом с одной или несколькими службами.

    ![Значок «Требуется перезапуск» и кнопка «Перезапуск»](./media/hdinsight-hadoop-heap-dump-linux/restartrequiredicon.png)

6. Выберите все службы, которые требуют перезагрузки, и используйте кнопку **Действия службы** для **включения режима обслуживания**. Режим обслуживания не позволит создавать предупреждения от службы при ее перезапуске.

    ![Включение меню режима обслуживания](./media/hdinsight-hadoop-heap-dump-linux/maintenancemode.png)

7. После включения режима обслуживания используйте кнопку **Перезапуск** для службы, чтобы **перезапустить все затронутые записи**.

    ![Перезапустить все затронутые записи](./media/hdinsight-hadoop-heap-dump-linux/restartbutton.png)

   > [!NOTE]  
   > Записи для кнопки **Перезапуск** могут быть другими для других служб.

8. После перезапуска служб используйте кнопку **Действия службы** для **отключения режима обслуживания**. Эта Ambari для возобновления наблюдения за оповещениями для службы.

