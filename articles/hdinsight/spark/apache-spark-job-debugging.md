---
title: Отладка заданий Apache Spark в Azure HDInsight
description: Использование пользовательского интерфейса YARN, пользовательского интерфейса Spark и сервера журнала Spark для отслеживания и отладки заданий, запущенных в кластере Spark в Azure HDInsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 12/05/2018
ms.author: hrasheed
ms.openlocfilehash: 5e384520c1b8d6cf5e3b182bbddf41a5f4f7f8f6
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "64707826"
---
# <a name="debug-apache-spark-jobs-running-on-azure-hdinsight"></a>Отладка заданий Apache Spark в Azure HDInsight

Из этой статьи вы узнаете, как выполнять отслеживание и отладку заданий [Apache Spark](https://spark.apache.org/), запущенных в кластерах HDInsight, с помощью пользовательского интерфейса [Apache Hadoop YARN](https://hadoop.apache.org/docs/current/hadoop-yarn/hadoop-yarn-site/YARN.html), пользовательского интерфейса Spark и сервера журнала Spark. Вы создадите задание Spark с помощью записной книжки, прилагающейся к кластеру Spark: **Машинное обучение. Прогнозный анализ на основе данных контроля качества пищевых продуктов с использованием MLLib**. С помощью описанных ниже действий вы сможете отслеживать приложение, отправленное любым другим методом, например с помощью **spark-submit**.

## <a name="prerequisites"></a>Технические условия
Необходимо следующее:

* Подписка Azure. См. страницу [бесплатной пробной версии Azure](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* Кластер Apache Spark в HDInsight. Инструкции см. в статье [Начало работы. Создание кластера Apache Spark в HDInsight на платформе Linux и выполнение интерактивных запросов с помощью SQL Spark](apache-spark-jupyter-spark-sql.md).
* Для начала вам следует запустить записную книжку **[Машинное обучение. Прогнозный анализ на основе данных контроля качества пищевых продуктов с использованием MLLib](apache-spark-machine-learning-mllib-ipython.md)** . Перейдите по ссылке, чтобы получить инструкции по запуску этой записной книжки.  

## <a name="track-an-application-in-the-yarn-ui"></a>Отслеживание приложения в пользовательском интерфейсе YARN
1. Запустите пользовательский интерфейс YARN. Щелкните **Yarn** в разделе **Панели мониторинга кластера**.
   
    ![Запуск пользовательского интерфейса YARN](./media/apache-spark-job-debugging/launch-yarn-ui.png)
   
   > [!TIP]  
   > Также пользовательский интерфейс YARN можно открыть из пользовательского интерфейса Ambari. Чтобы запустить пользовательский Интерфейс Ambari, в разделе **Панели мониторинга кластера** щелкните **Домашняя страница Ambari**. В пользовательском интерфейсе Ambari щелкните **YARN**, затем — **Quick Links** (Быстрые ссылки). Щелкните активный Resource Manager и щелкните **ResourceManager UI** (Пользовательский интерфейс Resource Manager). 

2. Так как вы запустили задание Spark с помощью записных книжек Jupyter, приложение получило имя **remotesparkmagics** (это стандартное имя для всех приложений, запускаемых из записных книжек). Нажмите идентификатор приложения рядом с именем приложения, чтобы посмотреть дополнительные сведения о задании. Откроется представление приложения.
   
    ![Поиск идентификатора приложения Spark](./media/apache-spark-job-debugging/find-application-id.png)
   
    Для приложений, которые запущены из записных книжек Jupyter, состояние всегда будет иметь значение **Запущено** , пока вы не выйдете из записной книжки.
3. Из представления приложения вы можете ознакомиться с подробными сведениями о контейнерах, связанных с приложением, а также изучить журналы (stdout и stderr). Пользовательский интерфейс Spark можно запустить, щелкнув ссылку в графе **URL-адрес отслеживания**, как показано ниже. 
   
    ![Скачивание журналов контейнера](./media/apache-spark-job-debugging/download-container-logs.png)

## <a name="track-an-application-in-the-spark-ui"></a>Отслеживание приложения в пользовательском интерфейсе Spark
В пользовательском интерфейсе Spark вы можете подробно изучить задания Spark, которые создаются запущенным вами приложением.

1. Чтобы запустить пользовательский интерфейс Spark, щелкните ссылку **URL-адрес отслеживания**в представлении приложения, как показано на снимке экрана выше. Здесь вы увидите все задания Spark, созданные приложением, которое запущено в записной книжке Jupyter.
   
    ![Просмотр заданий Spark](./media/apache-spark-job-debugging/view-spark-jobs.png)
2. Щелкните вкладку **Исполнители** , чтобы увидеть сведения о вычислениях и хранении для каждого исполнителя. Также можно получить стек вызовов, щелкнув ссылку **Дамп потока** .
   
    ![Просмотр исполнителей Spark](./media/apache-spark-job-debugging/view-spark-executors.png)
3. Щелкните вкладку **Этапы** , чтобы просмотреть этапы, связанные с приложением.
   
    ![Просмотр этапов Spark](./media/apache-spark-job-debugging/view-spark-stages.png)
   
    Каждый этап может включать несколько задач. Вы можете просмотреть для них статистику выполнения, как показано ниже.
   
    ![Просмотр этапов Spark](./media/apache-spark-job-debugging/view-spark-stages-details.png) 
4. На странице сведений об этапе вы можете открыть визуализацию DAG. Для этого разверните ссылку **Визуализация DAG** в верхней части страницы, как показано ниже.
   
    ![Просмотр этапов Spark в формате визуализации DAG](./media/apache-spark-job-debugging/view-spark-stages-dag-visualization.png)
   
    Направленный ациклический граф (DAG) представляет различные этапы в приложении. Каждый синий блок графа соответствует определенной операции Spark, вызываемой из приложения.
5. На странице сведений об этапах можно также запустить представление временной шкалы для приложения. Для этого разверните ссылку **Представление временной шкалы** в верхней части страницы, как показано ниже.
   
    ![Представление временной шкалы для этапов Spark](./media/apache-spark-job-debugging/view-spark-stages-event-timeline.png)
   
    Здесь вы увидите события Spark в формате временной шкалы. Это представление доступно на трех уровнях: для заданий, в пределах одного задания и в пределах этапа. На рисунке выше представлен пример представления временной шкалы для одного этапа.
   
   > [!TIP]  
   > Если установить флажок **Изменять масштаб** , представление временной шкалы можно будет прокручивать влево и вправо.

6. Другие вкладки в пользовательском интерфейсе Spark содержат полезные сведения о самом экземпляре Spark.
   
   * Вкладка "Хранилище" — если приложение создает RDD, информацию о них вы найдете на этой вкладке.
   * Вкладка "Среда" — на этой вкладке есть много полезных сведений о вашем экземпляре Spark, например: 
     * версия Scala;
     * каталог журнала событий, связанный с кластером;
     * число ядер исполнителя для приложения;
     * и т. д.

## <a name="find-information-about-completed-jobs-using-the-spark-history-server"></a>Поиск сведений о выполненных заданиях с помощью сервера журнала Spark
Когда задание завершается, сведения о нем сохраняются на сервере журнала Spark.

1. Чтобы открыть сервер журнала Spark, в колонке "Обзор" в разделе **Панели мониторинга кластера** щелкните **Сервер журнала Spark**.
   
    ![Запуск сервера журнала Spark](./media/apache-spark-job-debugging/launch-spark-history-server.png)
   
   > [!TIP]  
   > Также пользовательский интерфейс сервера журнала Spark можно открыть из пользовательского интерфейса Ambari. Чтобы пользовательский интерфейс Ambari, в колонке "Обзор" в разделе **Панели мониторинга кластера** щелкните **Домашняя страница Ambari**. В пользовательском интерфейсе Ambari щелкните **Spark**, **Быстрые ссылки**, а затем — **Spark History Server UI** (Пользовательский интерфейс сервера журнала Spark).

2. Отобразится список всех завершенных приложений. Выберите идентификатор приложения, чтобы открыть подробные сведения о приложении.
   
    ![Запуск сервера журнала Spark](./media/apache-spark-job-debugging/view-completed-applications.png)

## <a name="see-also"></a>См. также
*  [Управление ресурсами кластера Apache Spark в Azure HDInsight](apache-spark-resource-manager.md)
*  [Отладка заданий Apache Spark с помощью расширенного сервера журнала Spark](apache-azure-spark-history-server.md)

### <a name="for-data-analysts"></a>Для специалистов по анализу данных

* [Использование Apache Spark с Машинным обучением. Создание приложения машинного обучения Apache Spark в HDInsight](apache-spark-ipython-notebook-machine-learning.md)
* [Использование Apache Spark MLlib для создания приложения машинного обучения и анализа набора данных](apache-spark-machine-learning-mllib-ipython.md)
* [Анализ журналов веб-сайтов с помощью Apache Spark в HDInsight](apache-spark-custom-library-website-log-analysis.md)
* [Анализ журналов телеметрии Application Insights с помощью Apache Spark в HDInsight](apache-spark-analyze-application-insight-logs.md)
* [Использование Caffe в кластере Azure HDInsight Spark для распределенного глубокого обучения](apache-spark-deep-learning-caffe.md)

### <a name="for-spark-developers"></a>Для разработчиков Spark

* [Создание автономного приложения с использованием Scala](apache-spark-create-standalone-application.md)
* [Удаленный запуск заданий с помощью Apache Livy в кластере Apache Spark](apache-spark-livy-rest-interface.md)
* [Использование подключаемого модуля средств HDInsight для IntelliJ IDEA для создания и отправки приложений Spark Scala](apache-spark-intellij-tool-plugin.md)
* [Удаленная отладка приложений Apache Spark с помощью подключаемого модуля средств HDInsight для IntelliJ IDEA](apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [Использование записных книжек Zeppelin с кластером Apache Spark в Azure HDInsight](apache-spark-zeppelin-notebook.md)
* [Ядра для записной книжки Jupyter в кластерах Apache Spark в Azure HDInsight](apache-spark-jupyter-notebook-kernels.md)
* [Использование внешних пакетов с записными книжками Jupyter](apache-spark-jupyter-notebook-use-external-packages.md)
* [Установка записной книжки Jupyter на компьютере и ее подключение к кластеру Apache Spark в Azure HDInsight (предварительная версия)](apache-spark-jupyter-notebook-install-locally.md)
