---
title: Управление журналами для кластера HDInsight в Azure HDInsight
description: Определение типов, размеров и политик хранения для файлов журнала действий HDInsight.
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 03/19/2019
ms.author: hrasheed
ms.openlocfilehash: b42eb51b510423ffc0d15ee3a646bca3d4392f7f
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "64686857"
---
# <a name="manage-logs-for-an-hdinsight-cluster"></a>Управление журналами для кластера HDInsight

Кластер HDInsight создает множество файлов журналов. Например, Apache Hadoop и связанные с ним службы, такие как Apache Spark, создают подробные журналы выполнения заданий. Управление файлами журналов является частью поддержания работоспособности кластера HDInsight. Кроме того, к архивации журналов могут применяться нормативные требования.  Из-за количества и размера файлов журналов оптимизация хранения и архивирования журналов помогает в управлении затратами на обслуживание.

Управление журналами кластера HDInsight включает в себя хранение информации обо всех аспектах среды кластера. Эти сведения включают все связанные журналы служб Azure, конфигурацию кластера, сведения о выполнении заданий, любые состояния ошибок и другие данные по мере необходимости.

Типичные шаги в управлении журналом HDInsight:

* Шаг 1. Определение политики хранения журналов
* Шаг 2. Управление журналами конфигурации версий службы кластера
* Шаг 3. Управление файлами журнала выполнения заданий кластера
* Шаг 4. Прогнозирование размера хранилища журналов и затрат
* Шаг 5. Определение политик архивации журнала процессов

## <a name="step-1-determine-log-retention-policies"></a>Шаг 1. Определение политики хранения журналов

Первым шагом при создании стратегии управления журналом кластера HDInsight является сбор сведений о бизнес-сценариях и требованиях хранения истории выполнения заданий.

### <a name="cluster-details"></a>Сведения о кластере

Следующие сведения о кластере полезны для сбора информации в вашей стратегии управления журналами. Собирайте эти сведения из всех кластеров HDInsight, созданных в определенной учетной записи Azure.

* Имя кластера
* Регионы кластеров и зоны доступности Azure
* Состояние кластера, включая сведения о последнем изменении состояния
* Тип и количество экземпляров HDInsight для головного узла, узлов ядра и задач

Вы можете получить большую часть этих сведений верхнего уровня, используя портал Azure.  Кроме того, можно использовать [Azure CLI](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest) для получения сведений о вашей кластеров HDInsight с:

```azurecli
    az hdinsight list --resource-group <ResourceGroup>
    az hdinsight show --resource-group <ResourceGroup> --name <ClusterName>
```

Чтобы просмотреть эти сведения, можно также использовать PowerShell.  Дополнительные сведения см. в статье [Управление кластерами Apache Hadoop в HDInsight с помощью Azure PowerShell](hdinsight-administer-use-powershell.md).

### <a name="understand-the-workloads-running-on-your-clusters"></a>Общие сведения о рабочих нагрузках в кластерах

Важно понимать типы рабочих нагрузок, выполняемых в вашем кластере HDInsight, для разработки соответствующих стратегий ведения журналов для каждого типа.

* Являются ли рабочие нагрузки экспериментальными (для разработки или тестирования) или рабочего уровня?
* Как часто обычно выполняются рабочие нагрузки рабочего уровня?
* Являются ли какие-либо из рабочих нагрузок ресурсоемкими или долгосрочными?
* Используется ли в какой-либо из рабочих нагрузок сложный набор служб Hadoop, для которых создается несколько типов журналов?
* Имеет ли какая-либо из рабочих нагрузок соответствующие нормативные требования к выполнению преобразования?

### <a name="example-log-retention-patterns-and-practices"></a>Пример журнала хранения шаблонов и рекомендации по работе с ним

* Рассмотрите возможность отслеживания преобразования данных путем добавления идентификатора к каждой записи в журнале или с помощью других методов. Это позволит отследить исходный источник данных и операцию и наблюдать за данными на каждом этапе, чтобы понять их последовательность и допустимость.

* Подумайте, как вы можете получать журналы из кластера или из нескольких кластеров для таких целей, как аудит, мониторинг, планирование и оповещение. Вы можете использовать пользовательское решение для доступа к файлам журнала и их загрузке на регулярной основе, а также комбинировать и анализировать их для отображения панели мониторинга. Вы также можете добавить дополнительные возможности для оповещения о безопасности или обнаружении сбоев. Вы можете создавать эти программы с помощью PowerShell, SDK HDInsight или кода, который обращается к классической модели развертывания Azure.

* Подумайте, будет ли решение для мониторинга или служба полезной. Microsoft System Center предоставляет [пакет управления HDInsight](https://www.microsoft.com/download/details.aspx?id=42521). Вы также можете использовать сторонние средства, такие как Apache Chukwa и Ganglia, для сбора и централизации журналов. Многие компании предлагают услуги мониторинга решений больших данных на основе Hadoop, например: Centerity, Compuware APM, Sematext SPM и Zettaset Orchestrator.

## <a name="step-2-manage-cluster-service-versions-and-view-script-action-logs"></a>Шаг 2. Управление версиями журналов конфигурации и просмотр журналов действий сценариев

Типичный кластер HDInsight использует несколько служб и программные пакеты с открытым кодом (такие как Apache HBase, Apache Spark и т. д.). Для некоторых рабочих нагрузок, таких как биоинформатика, вам может потребоваться сохранить историю журналов конфигурации службы в дополнение к журналам выполнения заданий.

### <a name="view-cluster-configuration-settings-with-the-ambari-ui"></a>Просмотр параметров конфигурации кластера с помощью пользовательского интерфейса Ambari

Apache Ambari упрощает конфигурацию кластера HDInsight, управление им и его мониторинг за счет пользовательского веб-интерфейса и интерфейса REST API. Ambari есть в кластерах HDInsight на основе Linux. Выберите **панель мониторинга кластера** панели на странице портала Azure HDInsight для открытия **панели мониторинга кластера** странице ссылку.  Выберите область **Панель мониторинга кластера HDInsight**, чтобы открыть пользовательский интерфейс Ambari.  Введите учетные данные для входа в кластер.

Чтобы открыть список просмотров службы, на странице портала Azure для HDInsight выберите область **Просмотры Ambari**.  Этот список меняется в зависимости от установленных библиотек.  Например, могут появиться диспетчер очередей YARN, представления Hive и Tez.  Выберите ссылку на службу для просмотра сведений о конфигурации и службе.  На странице **стека и версии** пользовательского интерфейса Ambari содержатся сведения о конфигурации служб в кластере и история версий служб. Чтобы перейти к этому разделу, в пользовательском интерфейсе Ambari выберите меню **Администратор**, а затем — **Stacks and Versions** (Стеки и версии).  Выберите вкладку **версий**, чтобы просмотреть сведения о версии службы:

![Стек и версии](./media/hdinsight-log-management/stack-versions.png)

С помощью пользовательского интерфейса Ambari вы можете загрузить конфигурацию для любых служб, запущенных на конкретном узле в кластере.  Выберите меню **Узлы**, а затем перейдите по ссылке на нужный узел. На странице этого узла выберите **Host Actions** (Действия на узле), а затем **Download Client Configs** (Скачать конфигурацию клиента). 

![Конфигурация узла клиента](./media/hdinsight-log-management/client-configs.png)

### <a name="view-the-script-action-logs"></a>Просмотр журналов действий сценария

С помощью [действий сценария](hdinsight-hadoop-customize-cluster-linux.md) HDInsight можно запустить сценарии в кластере вручную или в соответствии с указанным временем. Например, действия сценария можно использовать для установки дополнительного программного обеспечения в кластере или изменения параметров конфигурации из значений по умолчанию. Журналы сценариев действий могут помочь понять ошибки, возникшие во время настройки кластера, а также изменения настроек конфигурации, которые могут повлиять на производительность и доступность кластера.  Чтобы увидеть состояние действия сценария, нажмите кнопку **Операции** в пользовательском интерфейсе Ambari или получите доступ к журналам состояний в учетной записи хранения по умолчанию. Журналы хранилища находятся в `/STORAGE_ACCOUNT_NAME/DEFAULT_CONTAINER_NAME/custom-scriptaction-logs/CLUSTER_NAME/DATE`.

## <a name="step-3-manage-the-cluster-job-execution-log-files"></a>Шаг 3. Управление файлами журнала выполнения заданий кластера

Следующий шаг заключается в анализе файлов журнала выполнения заданий для различных служб,  таких как Apache HBase, Apache Spark и т. д. Кластер Hadoop создает большое количество подробных журналов, чтобы определить, какие журналы используются (а какие — нет), что может занять некоторое время.  Важно понимать систему ведения журналов для целевого управления файлами журналов.  Ниже приведен пример файла журнала.

![Пример файла журнала HDInsight](./media/hdinsight-troubleshoot-failed-cluster/logs.png)

### <a name="access-the-hadoop-log-files"></a>Доступ к файлам журналов Hadoop

HDInsight хранит файлы журналов в файловой системе кластера и в хранилище Azure. Файлы журналов в кластере можно проверить, открыв [SSH](hdinsight-hadoop-linux-use-ssh-unix.md) подключения к кластеру и просмотрев файловую систему, или с помощью портала состояния Hadoop YARN на сервере удаленного головного узла. Файлы журналов можно проверить в хранилище Azure с помощью любых инструментов, которые могут получать доступ к данным из хранилища Azure и скачивать их. Примерами являются [AzCopy](../storage/common/storage-use-azcopy.md), [CloudXplorer](http://clumsyleaf.com/products/cloudxplorer)и обозреватель серверов Visual Studio. Кроме того, можно использовать PowerShell и библиотеки клиента хранилища Azure или пакеты SDK .NET Azure для доступа к данным в хранилище BLOB-объектов.

Hadoop выполняет работу заданий в виде *попыток завершения задач* на различных узлах в кластере. HDInsight может инициировать попытки спекулятивных задач, завершая все попытки, не выполненные в первую очередь. Это создает множество операций, которые регистрируются в файлах журнала контроллера, stderr и syslog в режиме реального времени. Кроме того, несколько попыток выполняются одновременно, но файл журнала может отображать результаты только линейно.

#### <a name="hdinsight-logs-written-to-azure-blob-storage"></a>Журналы HDInsight, записываемые в хранилище BLOB-объектов Azure

Кластеры HDInsight сконфигурированы для записи журналов задач в учетную запись хранения BLOB-объектов Azure для любого задания, которое отправляется с помощью командлетов Azure PowerShell или API-интерфейсов отправки .NET.  При отправке задания через SSH в кластер сведения о регистрации выполнения хранятся в таблицах Azure, как описано в предыдущем разделе.

В дополнение к основным файлам журнала, созданным HDInsight, установленные службы, такие как YARN, также создают файлы журналов выполнения задач.  Число и тип файлов журналов зависит от установленных служб.  Стандартные службы — Apache HBase Apache Spark и т. д.  Просмотрите файлы выполнения журнала заданий для каждой службы, чтобы понять общие сведения о файлах ведения журналов, доступных в вашем кластере.  Каждая служба имеет собственный уникальный метод ведения журналов и расположение для хранения файлов журналов.  Примеры подробных сведений о доступе к наиболее распространенным файлам журналов служб (из YARN) обсуждаются в следующем разделе.

### <a name="hdinsight-logs-generated-by-yarn"></a>Журналы HDInsight, формируемые службой YARN

Служба YARN объединяет журналы со всех контейнеров на рабочем узле и хранит их как один сводный файл журнала на рабочем узле. Этот журнал хранится в файловой системе по умолчанию после завершения приложения. Ваше приложение может использовать сотни или тысячи контейнеров, но журналы для всех контейнеров, выполненных на одном рабочем узле, всегда объединяются в один файл. Поэтому на каждый рабочий узел, используемый приложением, приходится один файл журнала. Объединение журналов включено по умолчанию на кластерах HDInsight версии 3.0 и более поздних версий. Объединенные журналы находятся в хранилище по умолчанию для кластера.

```
    /app-logs/<user>/logs/<applicationId>
```

Сводные журналы не могут считываться напрямую, так как они записаны в TFile, двоичном формате, индексированном контейнером. Чтобы отобразить эти журналы для интересующих вас приложений или контейнеров в виде обычного текста, используйте журналы YARN ResourceManager или средства CLI.

#### <a name="yarn-cli-tools"></a>Средства CLI для YARN

Чтобы использовать инструменты интерфейса командной строки для YARN, необходимо сначала подключиться к кластеру HDInsight с помощью SSH. Укажите сведения `<applicationId>`, `<user-who-started-the-application>`, `<containerId>` и `<worker-node-address>` при выполнении этих команд. Эти журналы можно отобразить в виде обычного текста, запустив одну из указанных ниже команд.

```bash
    yarn logs -applicationId <applicationId> -appOwner <user-who-started-the-application>
    yarn logs -applicationId <applicationId> -appOwner <user-who-started-the-application> -containerId <containerId> -nodeAddress <worker-node-address>
```

#### <a name="yarn-resourcemanager-ui"></a>Пользовательский интерфейс YARN ResourceManager

Пользовательский интерфейс YARN ResourceManager работает на головном узле кластера. Для доступа к нему можно использовать веб-интерфейс Ambari. Чтобы просмотреть журналы YARN, выполните следующие действия:

1. Откройте браузер и перейдите по адресу `https://CLUSTERNAME.azurehdinsight.net`. Параметр CLUSTERNAME нужно заменить именем кластера HDInsight.
2. В списке служб в левой части страницы выберите YARN.
3. В раскрывающемся списке "Быстрые ссылки" выберите один из главных узлов кластера, а затем щелкните **ResourceManager logs** (Журналы Resource Manager). Отобразится список ссылок на журналы YARN.

## <a name="step-4-forecast-log-volume-storage-sizes-and-costs"></a>Шаг 4. Прогнозирование размера хранилища журналов и затрат

После завершения предыдущих шагов у вас появится представление о типах и объемах файлов журналов, которые создаются кластерами HDInsight.

Далее проанализируйте объем данных журнала в местах хранения ключей журнала за период времени. Например, вы можете анализировать увеличение объема в течение 30, 60 или 90 дней.  Запишите эти сведения в электронную таблицу или используйте другие инструменты, такие как Visual Studio, обозреватель хранилища Azure или Power Query для Excel. Дополнительные сведения см. в статье об [анализе журналов HDInsight](hdinsight-debug-jobs.md).  

Теперь у вас достаточно информации для создания стратегии управления ключевыми журналами.  Используйте электронную таблицу (или другой инструмент) для прогнозирования роста объема журналов и затрат на хранение в Azure.  Учитывайте также требования к хранению журналов для набора изучаемых журналов.  Теперь вы можете спрогнозировать будущие расходы на хранение журналов, определив, какие файлы журналов можно удалить, а какие журналы следует сохранить и заархивировать в менее затратном хранилище Azure.

## <a name="step-5-determine-log-archive-policies-and-processes"></a>Шаг 5. Определение политик архивации журнала процессов

Определив файлы журналов для удаления, вы можете настроить параметры ведения журнала во многих службах Hadoop для автоматического удаления файлов журнала по истечении заданного периода времени.

Для некоторых файлов журнала можно использовать более экономный подход архивирования файлов журналов. Для журналов действий Azure Resource Manager вы можете изучить этот подход, используя портал Azure.  Настройте архивацию журналов ARM, выбрав ссылку **Журнал действий** на портале Azure для своего экземпляра HDInsight.  В верхней части страницы поиска журналов действий выберите пункт меню **Экспорт**, чтобы открыть область **Экспорт журнала действий**.  Укажите подписку, регион, необходимость экспорта в учетную запись хранения и количество дней хранения журналов. В этой же области можно также указать, следует ли экспортировать журналы в концентратор событий. 

![Экспорт файлов журналов](./media/hdinsight-log-management/archive.png)

Кроме того, можно создать сценарий архивации журнала с помощью PowerShell.  Пример сценария PowerShell см. в разделе [Archive Azure Automation logs to Azure Blob Storage](https://gallery.technet.microsoft.com/scriptcenter/Archive-Azure-Automation-898a1aa8) (Архивация журналов службы автоматизации Azure для хранилища BLOB-объектов).

### <a name="accessing-azure-storage-metrics"></a>Доступ к метрикам хранилища Azure

Хранилище Azure можно настроить для регистрации операций хранения и доступа. Вы можете использовать эти подробные журналы для мониторинга и планирования емкости, а также для аудита запросов на хранение. Зарегистрированная информация включает в себя детали задержки, позволяющие отслеживать и точно настраивать производительность ваших решений.
Пакет SDK .NET для Hadoop позволяет проанализировать файлы журналов, созданные для хранилища Azure, которые содержат данные для кластера HDInsight.

### <a name="control-the-size-and-number-of-backup-indexes-for-old-log-files"></a>Управление размером и числом индексов резервного копирования для старых файлов журнала

Чтобы контролировать размер и количество хранимых файлов журналов, задайте следующие свойства `RollingFileAppender`:

* `maxFileSize` — это критический размер файла, при котором будет выполнен откат файла. Значение по умолчанию — 10 МБ.
* `maxBackupIndex` указывает количество создаваемых файлов резервных копий, по умолчанию 1.

### <a name="other-log-management-techniques"></a>Другие способы управления журналами

Чтобы избежать нехватки места на диске, можно использовать некоторые средства ОС например [logrotate](https://linux.die.net/man/8/logrotate) для управления обработкой файлов журналов. `logrotate` можно настроить для ежедневного запуска для сжатия файлов журналов и удаления старых файлов. Подход зависит от требований, например, как долго следует хранить файлы на локальных узлах.  

Вы также можете проверить, включено ли ведение журнала отладки для одной или нескольких служб, что значительно увеличивает размер журнала выходных данных.  

Чтобы собирать журналы со всех узлов в централизованном месте, можно создать поток данных, такой как получение всех записей журнала в Solr.

## <a name="next-steps"></a>Дальнейшие действия

* [Monitoring and logging](https://msdn.microsoft.com/library/dn749790.aspx) (Мониторинг и ведение журнала)
* [Журналы приложений доступ Apache Hadoop YARN в HDInsight под управлением Linux](hdinsight-hadoop-access-yarn-app-logs-linux.md)
* [How to control size of log files for various HDP components?](https://community.hortonworks.com/articles/8882/how-to-control-size-of-log-files-for-various-hdp-c.html) (Как контролировать размер файлов журнала для различных компонентов HDP).
