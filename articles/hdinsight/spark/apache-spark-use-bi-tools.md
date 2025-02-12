---
title: 'Руководство по Анализ данных Apache Spark с использованием Power BI в Azure HDInsight '
description: 'Учебник: визуализация хранимых данных Apache Spark в кластерах HDInsight с помощью Microsoft Power BI'
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive,mvc
ms.topic: tutorial
ms.date: 05/16/2019
ms.openlocfilehash: d5296fe19cef9e8881d39bd9e59eb4c40d049959
ms.sourcegitcommit: 2d3b1d7653c6c585e9423cf41658de0c68d883fa
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/20/2019
ms.locfileid: "67296190"
---
# <a name="tutorial-analyze-apache-spark-data-using-power-bi-in-hdinsight"></a>Руководство по Анализ данных Apache Spark с использованием Power BI в HDInsight

Из этого учебника вы узнаете, как использовать [Microsoft Power BI](https://powerbi.microsoft.com/) для визуализации данных в кластере Apache Spark в [Azure HDInsight](https://azure.microsoft.com/services/hdinsight/).

Из этого руководства вы узнаете, как выполнять следующие задачи:
> [!div class="checklist"]
> * Визуализация данных Spark с помощью Power BI

Если у вас еще нет подписки Azure, [создайте бесплатную учетную запись Azure](https://azure.microsoft.com/free/?WT.mc_id=A261C142F), прежде чем начинать работу.

## <a name="prerequisites"></a>Предварительные требования

* Выполните инструкции из статьи [Руководство. Загрузка данных и выполнение запросов в кластере Apache Spark в Azure HDInsight](./apache-spark-load-data-run-query.md).

* [Power BI Desktop](https://powerbi.microsoft.com/en-us/desktop/).

* Необязательно: [Пробная подписка Power BI](https://app.powerbi.com/signupredirect?pbi_source=web).

## <a name="verify-the-data"></a>Проверка данных

[Записная книжка Jupyter Notebook](https://jupyter.org/), созданная при работе с [предыдущим руководством](apache-spark-load-data-run-query.md), содержит код для создания таблицы `hvac`. Эта таблица основана на CSV-файле, доступном во всех кластерах HDInsight Spark в `\HdiSamples\HdiSamples\SensorSampleData\hvac\hvac.csv`. Ниже описана процедура проверки данных.

1. В записной книжке Jupyter вставьте приведенный ниже пример кода и нажмите клавиши **SHIFT + ВВОД**. Этот код предназначен для проверки наличия таблиц.

    ```PySpark
    %%sql
    SHOW TABLES
    ```

    Он возвращает примерно такие выходные данные:

    ![Отображение таблиц в Spark](./media/apache-spark-use-bi-tools/show-tables.png)

    Если закрыть записную книжку перед выполнением шага выше, таблица `hvactemptable` будет очищена и не будет включена в выходные данные.  С помощью инструментов бизнес-аналитики можно получить доступ только к таблицам Hive, размещенным в хранилище метаданных (для таких таблиц в столбце **isTemporary** задано значение **false**). В этом руководстве вы подключитесь к созданной ранее таблице **hvac**.

2. Вставьте указанный ниже фрагмент кода в пустую ячейку и нажмите клавиши **SHIFT + ВВОД**. Этот код проверяет данные в таблице.

    ```PySpark
    %%sql
    SELECT * FROM hvac LIMIT 10
    ```

    Он возвращает примерно такие выходные данные:

    ![Отображение строк из таблицы hvac в Spark](./media/apache-spark-use-bi-tools/select-limit.png)

3. Для этого в меню **File** (Файл) элемента Notebook выберите **Close and Halt** (Закрыть и остановить). Завершите работу записной книжки для освобождения ресурсов.

## <a name="visualize-the-data"></a>Визуализация данных

В этом разделе вы с помощью Power BI создадите визуализации, отчеты и панели мониторинга на основе данных из кластера Spark.

### <a name="create-a-report-in-power-bi-desktop"></a>Создание отчета в Power BI Desktop

Начиная работать со Spark, в первую очередь необходимо подключиться к кластеру в Power BI Desktop, загрузить данные из кластера, а затем создать базовую визуализацию на основе этих данных.

> [!NOTE]  
> Соединитель, описанный в этой статье, находится в стадии предварительной версии. Любые отзывы о нем вы можете предоставить на [сайте сообщества Power BI](https://community.powerbi.com/) или на форуме [Power BI ideas](https://ideas.powerbi.com/forums/265200-power-bi-ideas).

1. Откройте Power BI Desktop. Закройте экран-заставку запуска, если он открыт.

2. Перейдите с вкладки **Домашняя страница** к элементу **Получение данных** > **Дополнительно...**

    ![Получение данных в Power BI Desktop из HDInsight Apache Spark](./media/apache-spark-use-bi-tools/hdinsight-spark-power-bi-desktop-get-data.png "Get data into Power BI from Apache Spark BI")

3. Введите в поле поиска `Spark` и выберите **Azure HDInsight Spark**, а затем щелкните **Подключиться**.

    ![Получение данных в Power BI из Apache Spark BI](./media/apache-spark-use-bi-tools/apache-spark-bi-import-data-power-bi.png "Получение данных в Power BI из Apache Spark BI")

4. Введите URL-адрес кластера (в формате `mysparkcluster.azurehdinsight.net`) в текстовое поле **Сервер**.

5. В разделе **Режим подключения к данным** выберите **DirectQuery**. Нажмите кнопку **ОК**.

    Для Spark можно использовать любой режим подключения данных. При использовании DirectQuery изменения отображаются в отчетах, при этом весь набор данных не обновляется. В случае импорта данных необходимо обновить набор данных, чтобы увидеть изменения. Дополнительные сведения о том, как и когда следует использовать DirectQuery, см. в статье [Использование DirectQuery в Power BI](https://powerbi.microsoft.com/documentation/powerbi-desktop-directquery-about/).

6. Введите данные учетной записи для входа в HDInsight и выберите **Подключиться**. Имя учетной записи по умолчанию — *admin*.

7. Выберите таблицу `hvac`, дождитесь отображения данных предварительного просмотра и щелкните **Загрузка**.

    ![Имя пользователя и пароль для кластера Spark](./media/apache-spark-use-bi-tools/apache-spark-bi-select-table.png "Имя пользователя и пароль для кластера Spark")

    Теперь в Power BI Desktop есть все сведения, необходимые для подключения к кластеру Spark и загрузки данных из таблицы `hvac`. Таблица и ее столбцы отображаются в области **Fields** (Поля).

8. Отобразите расхождения между целевой температурой и фактической температурой для каждого здания:

    1. В области **визуализаций** выберите **Диаграмма с областями**.

    2. Перетащите поле **BuildingID** в раздел **Ось**, а поля **ActualTemp** и **TargetTemp** в раздел **значений**.

        ![Создание визуализаций данных Spark с помощью Apache Spark BI](./media/apache-spark-use-bi-tools/apache-spark-bi-add-value-columns.png "Создание визуализаций данных Spark с помощью Apache Spark BI")

        Схема выглядит следующим образом.

        ![Создание визуализаций данных Spark с помощью Apache Spark BI](./media/apache-spark-use-bi-tools/apache-spark-bi-area-graph-sum.png "Создание визуализаций данных Spark с помощью Apache Spark BI")

        По умолчанию представление показывает сумму для **фактической температуры** и **целевой температуры**. Выберите направленную вниз стрелку рядом с полями **ActualTemp** и **TragetTemp** на панели визуализации, и вы увидите выбранную **сумму**.

    3. Щелкните направленную вниз стрелку рядом с полями **ActualTemp** и **TragetTemp** на панели визуализации, выберите **Среднее**, чтобы получить среднее значение фактической и целевой температуры для каждого здания.

        ![Создание визуализаций данных Spark с помощью Apache Spark BI](./media/apache-spark-use-bi-tools/apache-spark-bi-average-of-values.png "Создание визуализаций данных Spark с помощью Apache Spark BI")

        Визуализация данных должна выглядеть примерно так, как показано на снимке экрана ниже. Наведите указатель мыши на визуализацию, чтобы отобразить всплывающие подсказки с соответствующими данными.

        ![Создание визуализаций данных Spark с помощью Apache Spark BI](./media/apache-spark-use-bi-tools/apache-spark-bi-area-graph.png "Создание визуализаций данных Spark с помощью Apache Spark BI")

9. Выберите **Файл** > **Сохранить**, введите имя `BuildingTemperature` для файла, а затем выберите **Сохранить**.

### <a name="publish-the-report-to-the-power-bi-service-optional"></a>Публикация отчета в службе Power BI (необязательно)

Служба Power BI поддерживает совместное использование отчетов и панелей мониторинга для всей организации. В этом разделе вы опубликуете набор данных и отчет. Затем вы закрепите отчет на панели мониторинга. Информационные панели обычно используются, чтобы сосредоточиться на подмножестве данных в отчете. В вашем отчете содержится только одна визуализация, но тем не менее выполнение шагов ниже по-прежнему актуально.

1. Откройте Power BI Desktop.
2. На вкладке **Home** (Главная) щелкните **Publish** (Опубликовать).

    ![Публикация из Power BI Desktop](./media/apache-spark-use-bi-tools/apache-spark-bi-publish.png "Публикация из Power BI Desktop")

2. Выберите рабочую область, в которой вы хотите опубликовать набор данных и отчет, а затем нажмите кнопку **Выбор**. На рисунке ниже выбран параметр по умолчанию **Моя рабочая область**.

    ![Выбор рабочей области для публикации набора данных и отчета](./media/apache-spark-use-bi-tools/apache-spark-bi-select-workspace.png "Выбор рабочей области для публикации набора данных и отчета") 

3. Когда публикация успешно завершится, нажмите кнопку **Open 'BuildingTemperature.pbix' in Power BI** (Открыть BuildingTemperature.pbix в Power BI).

    ![Публикация завершилась успешно, щелкните, чтобы ввести учетные данные](./media/apache-spark-use-bi-tools/apache-spark-bi-publish-success.png "Публикация завершилась успешно, щелкните, чтобы ввести учетные данные") 

4. В службе Power BI щелкните ссылку, чтобы **ввести учетные данные**.

    ![Ввод учетных данных в службе Power BI](./media/apache-spark-use-bi-tools/apache-spark-bi-enter-credentials.png "Ввод учетных данных в службе Power BI")

5. Щелкните ссылку, чтобы **изменить учетные данные**.

    ![Изменение учетных данных в службе Power BI](./media/apache-spark-use-bi-tools/apache-spark-bi-edit-credentials.png "Изменение учетных данных в службе Power BI")

6. Введите данные учетной записи для входа в HDInsight и щелкните **Sign in** (Вход). Имя учетной записи по умолчанию — *admin*.

    ![Вход в кластер Spark](./media/apache-spark-use-bi-tools/apache-spark-bi-sign-in.png "Вход в кластер Spark")

7. В левой области выберите **Рабочие области** > **Моя рабочая область** > **Отчеты**, а затем щелкните **BuildingTemperature**.

    ![Отчет в списке раздела отчетов в левой области](./media/apache-spark-use-bi-tools/apache-spark-bi-service-left-pane.png "Отчет в списке раздела отчетов в левой области")

    Вы также увидите **BuildingTemperature** в списке раздела **Наборы данных** в левой области.

    Визуальный элемент, созданный в Power BI Desktop, теперь доступен в службе Power BI. 

8. Наведите курсор на визуализацию и щелкните значок булавки в правом верхнем углу.

    ![Отчет в службе Power BI](./media/apache-spark-use-bi-tools/apache-spark-bi-service-report.png "Отчет в службе Power BI")

9. Выберите "Новая информационная панель", введите имя `Building temperature`, а затем нажмите кнопку **Pin** (Закрепить).

    ![Закрепление на новой информационной панели](./media/apache-spark-use-bi-tools/apache-spark-bi-pin-dashboard.png "Закрепление на новой информационной панели")

10. В отчете нажмите кнопку **Перейти к информационной панели**. 

Ваш визуальный элемент закреплен на информационной панели. Вы можете добавить в отчет другие визуальные элементы, а затем закрепить их на этой же информационной панели. Дополнительные сведения об отчетах и информационных панелях см. в статьях [Отчеты в Power BI](https://powerbi.microsoft.com/documentation/powerbi-service-reports/) и [Общие сведения о панелях мониторинга для разработчиков Power BI](https://powerbi.microsoft.com/documentation/powerbi-service-dashboards/).

## <a name="clean-up-resources"></a>Очистка ресурсов

После завершения работы с этим руководством кластер можно удалить. В случае с HDInsight ваши данные хранятся в службе хранилища Azure, что позволяет безопасно удалить неиспользуемый кластер. Плата за кластеры HDInsight взимается, даже когда они не используются. Поскольку стоимость кластера во много раз превышает стоимость хранилища, экономически целесообразно удалять неиспользуемые кластеры.

Инструкции по удалению кластера см. в статье [Delete an HDInsight cluster using your browser, PowerShell, or the Azure CLI](../hdinsight-delete-cluster.md) (Удаление кластера HDInsight с помощью браузера, PowerShell или Azure CLI).

## <a name="next-steps"></a>Дополнительная информация

Из этого учебника вы узнали, как использовать [Microsoft Power BI](https://powerbi.microsoft.com/) для визуализации данных в кластере Apache Spark в [Azure HDInsight](https://azure.microsoft.com/services/hdinsight/). Теперь переходите к следующей статье, в которой объясняется, как перенести зарегистрированные в Spark данные в средство бизнес-аналитики, например в Power BI.

> [!div class="nextstepaction"]
> [Запуск потоковой задачи Apache Spark](apache-spark-eventhub-streaming.md)