---
title: Основные сведения о Службах машинного обучения в Azure HDInsight
description: Узнайте, как с помощью служб машинного обучения в HDInsight создать приложения для анализа больших данных.
ms.service: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.custom: hdinsightactive
ms.topic: overview
ms.date: 06/12/2019
ms.openlocfilehash: a5e4c7acd407cef1bb5fc43d7e40014a6b088a0a
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/28/2019
ms.locfileid: "67448893"
---
# <a name="what-is-ml-services-in-azure-hdinsight"></a>Общие сведения о службах машинного обучения в HDInsight

Microsoft Machine Learning Server доступен в качестве варианта развертывания при создании кластеров HDInsight в Azure. Тип кластера, предоставляющий этот вариант, называется **Службы машинного обучения**. Это позволяет специалистам по обработке и анализу данных, статистикам и R-программистам получать доступ к масштабируемым, распределенным методам аналитики в HDInsight по требованию.

Службы машинного обучения в HDInsight предоставляют новейшие возможности для анализа наборов данных практически любого размера, загруженных в хранилище BLOB-объектов Azure или Data Lake Store, на базе языка R. Так как кластер служб машинного обучения основывается на R с открытым кодом, вы сможете использовать в своих R-приложениях любые из более чем 8000 R-пакетов с открытым кодом. Также доступны процедуры из ScaleR — пакета аналитики больших данных от Майкрософт.

Для подключения к кластеру и выполнения скриптов на языке R удобно использовать граничный узел кластеров. На граничном узле вы можете выполнять распараллеленные распределенные функции ScaleR на ядрах сервера граничного узла. Кроме того, вы можете выполнять эти функции на узлах кластера с помощью контекста вычислений Hadoop Map Reduce или Apache Spark ScaleR.

Модели или прогнозы, полученные в результате анализа, можно загрузить для локального использования. Их также можно использовать в любой службе Azure, в частности в [веб-службе](../../machine-learning/studio/publish-a-machine-learning-web-service.md) [Студия машинного обучения Microsoft Azure](https://studio.azureml.net).

## <a name="get-started-with-ml-services-on-hdinsight"></a>Начало работы со службами машинного обучения в HDInsight

Чтобы создать кластер служб машинного обучения в Azure HDInsight, необходимо выбрать тип кластера **Службы машинного обучения** при создании кластера HDInsight на портале Azure. Тип кластера "Службы машинного обучения" включает в себя ML Server на узлах данных кластера и ML Server на граничном узле, который служит платформой для анализа данных на базе служб машинного обучения. Пошаговое руководство по созданию кластера см. в статье [Create Linux-based clusters in HDInsight by using the Azure portal](../hdinsight-hadoop-create-linux-clusters-portal.md) (Создание кластеров Linux в HDInsight с помощью портала Azure).

## <a name="why-choose-ml-services-in-hdinsight"></a>Почему следует выбрать службы машинного обучения в HDInsight?

Ниже приведены преимущества использования служб машинного обучения в HDInsight.

### <a name="ai-innovation-from-microsoft-and-open-source"></a>Инновации в области искусственного интеллекта корпорации Майкрософт с открытым кодом

  Службы машинного обучения включают в себя широкий набор высокомасштабируемых распределенных алгоритмов, в частности [RevoscaleR](https://docs.microsoft.com/machine-learning-server/r-reference/revoscaler/revoscaler), [revoscalepy](https://docs.microsoft.com/machine-learning-server/python-reference/revoscalepy/revoscalepy-package) и [microsoftML](https://docs.microsoft.com/machine-learning-server/python-reference/microsoftml/microsoftml-package). Они могут работать с данными большого размера (больше, чем физическая память) и работать на базе различных платформ в режиме распределенного запуска. Узнайте о коллекции пользовательских [R-пакетов](https://docs.microsoft.com/machine-learning-server/r-reference/introducing-r-server-r-package-reference) и [пакетов Python](https://docs.microsoft.com/machine-learning-server/python-reference/introducing-python-package-reference) корпорации Майкрософт, которые включены в продукт.
  
  Службы машинного обучения на основе единой корпоративной платформы объединяют инновации корпорации Майкрософт и продукты, созданные в сообществе открытого кода (набор средств R, Python и искусственного интеллекта). Любые пакеты машинного обучения с открытым кодом R или Python могут работать вместе с любой инновацией от корпорации Майкрософт.

### <a name="simple-secure-and-high-scale-operationalization-and-administration"></a>Простой, безопасный и крупномасштабный ввод в эксплуатацию и администрирование

  Организации опираются на традиционные парадигмы и среды, поэтому им приходится вкладывать большое количество времени и усилий в ввод в эксплуатацию. Это приводит к завышению стоимости и задержкам, в частности из-за таких факторов: время преобразования моделей, итерации, обеспечивающие их корректность и актуальность, нормативное утверждение, управление разрешениями через ввод в эксплуатацию.

  Службы машинного обучения предлагают возможности [ввода в эксплуатацию](https://docs.microsoft.com/machine-learning-server/what-is-operationalization) корпоративного класса. С момента завершения работы над моделью машинного обучения до создания API веб-служб достаточно всего нескольких щелчков. Эти [веб-службы](https://docs.microsoft.com/machine-learning-server/operationalize/concept-what-are-web-services) размещены на облачной серверной сетке и могут быть интегрированы с бизнес-приложениями. Возможность развертывания на базе эластичной сетки позволяет масштабироваться с учетом потребностей бизнеса, как для пакетной поставки, так и для оценки в реальном времени. Инструкции см. в статье о [вводе в эксплуатацию служб машинного обучения в Azure HDInsight](r-server-operationalize.md).

<!---
* **Deep ecosystem engagements to deliver customer success with optimal total cost of ownership**

  Individuals embarking on the journey of making their applications intelligent or simply wanting to learn the new world of AI and machine learning, need the right resources to help them get started. In addition to this documentation, Microsoft provides several learning resources and has engaged several training partners to help you ramp up and become productive quickly.
--->

## <a name="key-features-of-ml-services-on-hdinsight"></a>Основные возможности служб машинного обучения в HDInsight

Следующие компоненты включены в службы машинного обучения в HDInsight.

| Категория компонента | ОПИСАНИЕ |
|------------------|-------------|
| Поддержка R | [Пакеты R](https://docs.microsoft.com/machine-learning-server/r-reference/introducing-r-server-r-package-reference) для решений, написанных на R, с дистрибутивом R с открытым кодом и инфраструктурой среды выполнения для выполнения сценариев. |
| Поддержка Python | [Модули Python](https://docs.microsoft.com/machine-learning-server/python-reference/introducing-python-package-reference) для решений, написанных на Python, с дистрибутивом Python с открытым кодом и инфраструктурой среды выполнения для выполнения сценариев.
| [Предварительно подготовленные модели](https://docs.microsoft.com/machine-learning-server/install/microsoftml-install-pretrained-models) | Для визуального анализа и анализа тональности публикаций, готовых к оценке предоставляемых данных. |
| [Развертывание и потребление](r-server-operationalize.md) | Ввод в эксплуатацию сервера и развернутого решения в качестве веб-службы. |
| [Удаленное выполнение](r-server-hdinsight-manage.md#connect-remotely-to-microsoft-ml-services) | Запускайте удаленные сеансы на кластере служб машинного обучения в своей сети со своей клиентской рабочей станции. |


## <a name="data-storage-options-for-ml-services-on-hdinsight"></a>Варианты хранилищ данных для служб машинного обучения в HDInsight

Объем хранилища по умолчанию для файловой системы HDFS в кластерах HDInsight можно связать с учетной записью службы хранилища Azure или Azure Data Lake Storage. Такая связь гарантирует сохранение любых данных, передающихся в хранилище кластера в ходе анализа, и их доступность даже после удаления кластера. Существуют различные инструменты для обработки передачи данных в выбранное вами хранилище, включая средства передачи портала, доступные учетной записи хранения, и служебную программу [AzCopy](../../storage/common/storage-use-azcopy.md).

Независимо от выбранного основного хранилища вы можете настроить доступ к дополнительным хранилищам BLOB-объектов или Data Lake Store во время подготовки кластера.  Сведения об использовании нескольких учетных записей хранения см. в статье о [вариантах хранилищ хранилища Azure для служб машинного обучения в HDInsight](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-r-server-storage).

На граничном узле для хранения данных можно также использовать [файлы Azure](../../storage/files/storage-how-to-use-files-linux.md). Они позволяют подключить общую папку, созданную в службе хранения Azure, к файловой системе Linux. Дополнительные сведения об этих вариантах хранения данных для служб машинного обучения в кластере HDInsight см. в статье о [вариантах хранилищ хранилища Azure для служб машинного обучения в HDInsight](r-server-storage.md).

## <a name="access-ml-services-edge-node"></a>Доступ к граничному узлу служб машинного обучения

Можно подключить Microsoft ML Server к граничному узлу с помощью браузера. Он устанавливается по умолчанию во время создания кластера.  Для доступа к консоли R можно также подключиться к граничному узлу кластера из командной строки с помощью SSH или PuTTY.

## <a name="develop-and-run-r-scripts"></a>Разработка и выполнение сценариев R

Для создания и выполнения скриптов R можно использовать не только распараллеленные распределенные процедуры из библиотеки ScaleR, но и более 8000 пакетов R с открытым исходным кодом. Как правило, скрипт, выполняемый службами машинного обучения на граничном узле, выполняется в интерпретаторе R на этом узле. Исключением являются действия, которым необходимо вызывать функцию ScaleR с контекстом вычислений Hadoop Map Reduce (RxHadoopMR) или Spark (RxSpark). В этих случаях функция выполняется в распределенном режиме на узлах данных (задач) кластера, связанных с соответствующими данными. Дополнительные сведения о различных вариантах контекста вычислений см. в статье о [вариантах контекста вычислений для служб машинного обучения в HDInsight](r-server-compute-contexts.md).

## <a name="operationalize-a-model"></a>Ввод модели в эксплуатацию

После того как моделирование данных будет завершено, вы можете запустить модель в эксплуатацию для составления прогнозов на основе новых данных в Azure или в локальной среде. Этот процесс называется оценкой. Оценка может осуществляться в HDInsight, Машинном обучении Azure или локальной среде.

### <a name="score-in-hdinsight"></a>Оценка в HDInsight

Для оценки в HDInsight напишите функцию R, которая вызывает модель для создания прогнозов для нового файла данных, загруженного в учетную запись хранения. Сохраните прогнозы в учетной записи хранения. Эту процедуру можно выполнять на граничном узле кластера по требованию или по расписанию.

### <a name="score-in-azure-machine-learning-aml"></a>Оценка в Машинном обучении Azure

Для оценки с помощью Машинного обучения Azure используйте пакет R с открытым кодом для Машинного обучения Azure, известный как [AzureML](https://cran.r-project.org/web/packages/AzureML/vignettes/getting_started.html), чтобы опубликовать модель как веб-службу Azure. Для удобства этот пакет предварительно установлен на граничном узле. Используйте средства машинного обучения Azure, чтобы создать пользовательский интерфейс для веб-службы. Затем вызовите веб-службу, чтобы выполнить оценку.

При выборе этого варианта объекты модели ScaleR необходимо преобразовать в эквивалентные объекты модели с открытым исходным кодом для использования с веб-службой. Для этого используйте функции приведения ScaleR, такие как `as.randomForest()`, для моделей на основе ансамблей.

### <a name="score-on-premises"></a>Оценка в локальной среде

Для оценки в локальной среде созданную модель можно сериализовать в R, загрузить на локальный компьютер, десериализовать, а затем использовать для оценки новых данных. Вы можете оценить новые данные, используя подход, описанный ранее в разделе [Оценка в HDInsight](#score-in-hdinsight), или с помощью [веб-служб](https://docs.microsoft.com/machine-learning-server/operationalize/concept-what-are-web-services).

## <a name="maintain-the-cluster"></a>Обслуживание кластера

### <a name="install-and-maintain-r-packages"></a>Установка и обслуживание пакетов R

Большинство используемых вами пакетов R должны находиться на граничном узле, так как именно там выполняется основная часть действий скриптов R. Для установки дополнительных пакетов R на граничном узле можно использовать метод `install.packages()` в R.

Если вы пользуетесь только подпрограммами из библиотеки ScaleR в кластере, устанавливать дополнительные пакеты R на узлы данных, как правило, не требуется. Однако дополнительные пакеты могут потребоваться для выполнения метода **rxExec** или **RxDataStep** на узлах данных.

В таких случаях после создания кластера дополнительные пакеты можно установить с помощью действия скрипта. Дополнительные сведения см. в статье об [управлении кластером служб машинного обучения в HDInsight](r-server-hdinsight-manage.md).

### <a name="change-apache-hadoop-mapreduce-memory-settings"></a>Изменение параметров памяти Apache Hadoop MapReduce

Изменив кластер, можно скорректировать объем памяти, который доступен службам машинного обучения при выполнении задания MapReduce. Чтобы изменить кластер, используйте пользовательский интерфейс Ambari, который вы найдете в колонке своего кластера на портале Azure. Инструкции по получению доступа к пользовательскому интерфейсу Ambari для кластера см. в статье [Управление кластерами HDInsight с помощью веб-интерфейса Ambari](../hdinsight-hadoop-manage-ambari.md).

Вы также можете изменить объем памяти, доступной для служб машинного обучения, с помощью переключателей Hadoop в вызове метода **RxHadoopMR**, как показано ниже.

    hadoopSwitches = "-libjars /etc/hadoop/conf -Dmapred.job.map.memory.mb=6656"  

### <a name="scale-your-cluster"></a>Масштабирование кластера

Масштаб имеющегося кластера служб машинного обучения в HDInsight можно увеличить или уменьшить на портале. Благодаря увеличению масштаба вы получаете дополнительную мощность, которая может потребоваться для более ресурсоемких задач. Также масштабирование позволяет свернуть кластер во время простоя. Инструкции по масштабированию кластера см. в статье [Управление кластерами Hadoop в HDInsight с помощью портала Azure](../hdinsight-administer-use-portal-linux.md).

### <a name="maintain-the-system"></a>Поддержка системы

Обслуживание, включающее в себя установку исправлений ОС и других обновлений, проводится на базовых виртуальных машинах Linux в кластере HDInsight в нерабочее время. Как правило, обслуживание проводится каждый понедельник и четверг в 3:30 (по местному времени для виртуальной машины). Обновления выполняются таким образом, чтобы они не влияли более чем на четверть кластера одновременно.

Так как головные узлы избыточны, а затрагиваются не все узлы данных, выполнение заданий в это время может занимать больше времени. Тем не менее они все равно будут выполняться до завершения. Обслуживание не затрагивает установленное пользовательское программное обеспечение или локальные данные, если только в системе не возникнет неустранимая ошибка, требующая повторной сборки кластера.

## <a name="ide-options-for-ml-services-on-hdinsight"></a>Варианты интегрированной среды разработки для служб машинного обучения в HDInsight

Граничный узел Linux в кластере HDInsight используется в качестве платформы для анализа на основе R. В последних версиях HDInsight RStudio Server по умолчанию устанавливается на граничный узел в качестве интегрированной среды разработки на основе браузера. Использование RStudio Server в качестве интегрированной среды разработки для разработки и выполнение сценариев R может быть намного более эффективным, чем использование только консоли R.

Кроме того, можно установить классическую интегрированную среду разработки и использовать ее для доступа к кластеру посредством удаленного контекста вычислений MapReduce или Spark. К возможным вариантам относятся [Инструменты R для Visual Studio](https://www.visualstudio.com/features/rtvs-vs.aspx), RStudio и [StatET](http://www.walware.de/goto/statet) на базе Eclipse от Walware.

Кроме того, доступ к консоли R на граничном узле можно получить, просто введя **R** в командной строке Linux после подключения с помощью SSH или PuTTY. При работе с консолью будет удобнее, если вы откроете текстовый редактор для разработки скрипта R в другом окне и будете вырезать и вставлять необходимые фрагменты скрипта в консоль R.

## <a name="pricing"></a>Цены

Кластер HDInsight со службами машинного обучения оплачивается по такой же схеме, что и другие кластеры HDInsight. Стоимость кластера зависит от размера основной виртуальной машины на разных узлах имен, узлах данных и граничных узлах, а также от наценки за ядро-час в кластере. Дополнительные сведения см. на странице [цен на HDInsight](https://azure.microsoft.com/pricing/details/hdinsight/).

## <a name="next-steps"></a>Дополнительная информация

Дополнительные сведения об использовании служб машинного обучения в кластерах HDInsight см. в следующих ресурсах:

* [Краткое руководство: выполнение сценария R в кластере служб машинного обучения в Azure HDInsight с помощью сервера RStudio](machine-learning-services-quickstart-job-rstudio.md)
* [Варианты контекста вычислений для кластера служб машинного обучения в HDInsight](r-server-compute-contexts.md)
* [Решения службы хранилища Azure для служб машинного обучения в Azure HDInsight](r-server-storage.md)
