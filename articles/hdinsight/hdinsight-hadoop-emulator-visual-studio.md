---
title: Использование средств Data Lake для Visual Studio с песочницей Hortonworks в Azure HDInsight
description: Узнайте, как использовать средства Azure Data Lake для Visual Studio с песочницей Hortonworks, работающей в локальной виртуальной машине. С помощью этих средств можно создавать и выполнять задания Hive и Pig в песочнице и просматривать выходные данные и журнал задания.
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 05/07/2018
ms.author: hrasheed
ms.openlocfilehash: 3286ca3b9c85236ff322eb19324bc5ac7a904e22
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "65605456"
---
# <a name="use-the-azure-data-lake-tools-for-visual-studio-with-the-hortonworks-sandbox"></a>Использование средств Azure Data Lake для Visual Studio с песочницей Hortonworks

Azure Data Lake включает в себя средства для работы с универсальными кластерами Apache Hadoop. В этом документе описываются действия, необходимые для использования средств Data Lake с песочницей Hortonworks, работающей на локальной виртуальной машине.

Использование песочницы Hortonworks позволяет работать с Hadoop локально в среде разработки. Если вам необходимо развернуть решение в нужном масштабе после его разработки, можно перейти к кластеру HDInsight.

## <a name="prerequisites"></a>Технические условия

* Песочница Hortonworks, работающая в виртуальной машине в среде разработки. Приведенные в этом документе сведения протестированы с использованием песочницы, работающей в Oracle VirtualBox. Сведения о настройке песочницы см. в статье [Начало работы с песочницей Hadoop, эмулятором на виртуальной машине](hadoop/apache-hadoop-emulator-get-started.md) .

* приведенному.

* [Пакет Azure SDK для .NET](https://azure.microsoft.com/downloads/) версии 2.7.1 или более поздней.

* [Средства Azure Data Lake для Visual Studio](https://www.microsoft.com/download/details.aspx?id=49504).

## <a name="configure-passwords-for-the-sandbox"></a>Настройка паролей для песочницы

Убедитесь в том, что песочница Hortonworks работает. Выполните инструкции из документа [Начало работы с песочницей Hadoop, эмулятором на виртуальной машине](hadoop/apache-hadoop-emulator-get-started.md#set-sandbox-passwords). Эти инструкции используются для настройки пароля для учетной записи SSH `root` и учетной записи Apache Ambari `admin`. Эти пароли используются при подключении к песочнице из Visual Studio.

## <a name="connect-the-tools-to-the-sandbox"></a>Подключение инструментов к песочнице

1. Откройте Visual Studio и выберите **Вид**, а затем — **Обозреватель серверов**.

2. В окне **Обозреватель серверов** щелкните правой кнопкой мыши запись **HDInsight**, а затем выберите **Connect to HDInsight Emulator** (Подключиться к эмулятору HDInsight).

    ![Снимок экрана: обозреватель сервера с выделенной командой "Подключиться к эмулятору HDInsight".](./media/hdinsight-hadoop-emulator-visual-studio/connect-emulator.png)

3. В диалоговом окне **Подключиться к эмулятору HDInsight** введите пароль, настроенный для Ambari.

    ![Снимок экрана: диалоговое окно с выделенным текстовым полем для ввода пароля](./media/hdinsight-hadoop-emulator-visual-studio/enter-ambari-password.png)

    Нажмите кнопку **Далее**, чтобы продолжить.

4. В поле **Пароль** введите пароль, настроенный для учетной записи `root`. Оставьте в остальных полях значения по умолчанию.

    ![Снимок экрана: диалоговое окно с выделенным текстовым полем для ввода пароля](./media/hdinsight-hadoop-emulator-visual-studio/enter-root-password.png)

    Нажмите кнопку **Далее**, чтобы продолжить.

5. Подождите, пока завершится проверка служб. В некоторых случаях проверка может завершиться ошибкой, и тогда появится запрос на обновление конфигурации. В случае ошибки проверки нажмите кнопку **Обновить** и подождите, пока завершится настройка и проверка службы.

    ![Снимок экрана: диалоговое окно с выделенной кнопкой "Обновить"](./media/hdinsight-hadoop-emulator-visual-studio/fail-and-update.png)

    > [!NOTE]  
    > В процессе обновления используется Ambari для изменения конфигурации песочницы Hortonworks до ожидаемых значений для средств Data Lake для Visual Studio.

6. После завершения проверки нажмите кнопку **Готово**, чтобы завершить настройку.
    ![Снимок экрана: диалоговое окно с выделенной кнопкой "Готово"](./media/hdinsight-hadoop-emulator-visual-studio/finished-connect.png)

     >[!NOTE]  
     > В зависимости от скорости среды разработки и объема памяти, выделенного для виртуальной машины, настройка и проверка служб может занять несколько минут.

После выполнения этих действий в обозревателе серверов в разделе **HDInsight** появится запись **Локальный кластер HDInsight**.

## <a name="write-an-apache-hive-query"></a>Создание запроса Apache Hive

Hive предоставляет язык запросов по типу SQL (HiveQL) для работы со структурированными данными. Чтобы узнать, как выполнять запросы по требованию к локальному кластеру, следуйте указанной ниже инструкции.

1. В **обозревателе сервера** щелкните правой кнопкой мыши добавленную ранее запись для локального кластера, а затем выберите пункт **Написать запрос Hive**.

    ![Снимок экрана: обозреватель сервера с выделенным пунктом "Написать запрос Hive"](./media/hdinsight-hadoop-emulator-visual-studio/write-hive-query.png)

    Откроется окно нового запроса. Здесь можно быстро написать и отправить запрос к локальному кластеру.

2. В новом окне создания запроса введите следующую команду:

        select count(*) from sample_08;

    Чтобы выполнить запрос, нажмите кнопку **Отправить** в верхней части окна. Оставьте в остальных полях (**Пакет** и "Имя сервера") значения по умолчанию.

    ![Снимок экрана: окно запроса с выделенной кнопкой "Отправить"](./media/hdinsight-hadoop-emulator-visual-studio/submit-hive.png)

    Меню **Дополнительно** можно также открыть с помощью раскрывающегося меню рядом с кнопкой **Отправить**. В диалоговом окне дополнительных параметров можно указать дополнительные параметры при отправке задания.

    ![Снимок экрана: диалоговое окно "Отправить скрипт"](./media/hdinsight-hadoop-emulator-visual-studio/advanced-hive.png)

3. После отправки запроса отобразится состояние задания. В нем содержатся сведения о ходе обработки задания с помощью Hadoop. В записи **Состояние задания** отображается состояние задания. Это состояние периодически обновляется. Его также можно обновить вручную, щелкнув значок обновления.

    ![Снимок экрана: диалоговое окно "Представление заданий" с выделенной записью "Состояние задания"](./media/hdinsight-hadoop-emulator-visual-studio/job-state.png)

    После того как значение записи **Состояние задания** изменится на **Завершено**, отобразится направленный ациклический граф (DAG). На этой схеме показан путь выполнения, определенный с помощью Tez при обработке запроса Hive. Tez — это механизм выполнения по умолчанию для Hive в локальном кластере.

    > [!NOTE]  
    > Apache Tez также применяется по умолчанию при использовании кластеров HDInsight под управлением Linux. Для HDInsight под управлением Windows он не является механизмом по умолчанию. Для использования его в этом случае необходимо добавить строку `set hive.execution.engine = tez;` в начало запроса Hive.

    Воспользуйтесь ссылкой **Job Output** (Выходные данные задания), чтобы просмотреть выходные данные. В данном случае это 823 — количество строк в таблице sample_08. Диагностические сведения о задании можно просмотреть, воспользовавшись ссылками **Журнал задания** и **Download YARN Log** (Скачать журнал YARN).

4. Кроме того, можно выполнять задания Hive интерактивно. Для этого нужно изменить значение поля **Пакет** на **Интерактивный**, а затем нажать кнопку **Выполнить**.

    ![Снимок экрана с выделенным элементом "Интерактивный" и кнопкой "Выполнить"](./media/hdinsight-hadoop-emulator-visual-studio/interactive-query.png)

    Интерактивный запрос передает журнал выходных данных, созданный во время обработки, в окно **HiveServer2 Output** (Выходные данные HiveServer2).

    > [!NOTE]  
    > Это те же сведения, которые можно получить, перейдя по ссылке **Журнал задания** после завершения задания.

    ![Снимок экрана: журнал выходных данных](./media/hdinsight-hadoop-emulator-visual-studio/hiveserver2-output.png)

## <a name="create-a-hive-project"></a>Создание проекта Hive

Вы также можете создать проект, который содержит несколько скриптов Hive. Если у вас есть связанные скрипты или вы хотите сохранить скрипты в системе управления версиями, используйте проект.

1. Откройте Visual Studio, выберите **Файл**, **Создать**, а затем **Проект**.

2. В списке проектов разверните список **Шаблоны**, раздел **Azure Data Lake**, а затем выберите **HIVE (HDInsight)** . В списке шаблонов выберите **Hive Sample** (Пример Hive). Введите имя и расположение, а затем нажмите кнопку **ОК**.

    ![Снимок экрана; окно нового проекта с выделенными элементами "Azure Data Lake", "HIVE", "Пример Hive" и "ОК"](./media/hdinsight-hadoop-emulator-visual-studio/new-hive-project.png)

Проект **Hive Sample** (Пример Hive) содержит два скрипта: **WebLogAnalysis.hql** и **SensorDataAnalysis.hql**. Чтобы отправить скрипты, нажмите ту же кнопку **Отправить** в верхней части окна.

## <a name="create-an-apache-pig-project"></a>Создание проекта Apache Pig

Тогда как Hive предоставляет язык по типу SQL для работы со структурированными данными, Pig работает, преобразуя данные. Pig предоставляет язык (Pig Latin), с помощью которого можно разрабатывать конвейер преобразований. Чтобы использовать Pig в локальном кластере, выполните указанные ниже действия.

1. Откройте Visual Studio, выберите **Файл**, **Создать**, а затем — **Проект**. В списке проектов разверните список **Шаблоны**, разверните **Azure Data Lake**, а затем выберите **Pig (HDInsight)** . В списке шаблонов выберите **Pig Application** (Приложение Pig). Введите имя и расположение, а затем нажмите кнопку **ОК**.

    ![Снимок экрана; окно нового проекта с выделенными элементами "Azure Data Lake", "Pig", "Приложение Pig" и "ОК"](./media/hdinsight-hadoop-emulator-visual-studio/new-pig.png)

2. Введите в качестве содержимого файла **script.pig**, созданного с помощью этого проекта, следующий текст.

        a = LOAD '/demo/data/Website/Website-Logs' AS (
            log_id:int,
            ip_address:chararray,
            date:chararray,
            time:chararray,
            landing_page:chararray,
            source:chararray);
        b = FILTER a BY (log_id > 100);
        c = GROUP b BY ip_address;
        DUMP c;

    Хотя в Pig и Hive используются разные языки, способ выполнения задания одинаковый для обоих языков (нужно нажать кнопку **Отправить**). Если выбрать раскрывающийся список рядом с кнопкой **Отправить**, отобразится диалоговое окно расширенной отправки для Pig.

    ![Снимок экрана: диалоговое окно "Отправить скрипт"](./media/hdinsight-hadoop-emulator-visual-studio/advanced-pig.png)

3. Состояние и выходные данные задания отображаются так же, как и для запроса Hive.

    ![Снимок экрана: завершенное задание Pig](./media/hdinsight-hadoop-emulator-visual-studio/completed-pig.png)

## <a name="view-jobs"></a>Просмотр заданий

Средства Data Lake также позволяют с легкостью просмотреть сведения о заданиях, выполненных в Hadoop. Чтобы увидеть задания, которые были запущены в локальном кластере, выполните указанные ниже действия.

1. В **обозревателе сервера** щелкните правой кнопкой мыши локальный кластер, а затем выберите пункт **Просмотреть задания**. Отобразится список заданий, переданных в кластер.

    ![Снимок экрана: обозреватель сервера с выделенным пунктом "Просмотреть задания"](./media/hdinsight-hadoop-emulator-visual-studio/view-jobs.png)

2. Выберите в этом списке задание, чтобы просмотреть сведения о нем.

    ![Снимок экрана: браузер заданий с выделенным заданием](./media/hdinsight-hadoop-emulator-visual-studio/view-job-details.png)

    Отображаемые сведения похожи на сведения, отображаемые после выполнения запроса Pig или Hive, включая ссылки для просмотра выходных данных и сведений журнала.

3. Кроме того, на этом этапе можно изменить и повторно отправить задания.

## <a name="view-hive-databases"></a>Просмотр баз данных Hive

1. В окне **Обозреватель серверов** разверните запись **HDInsight local cluster** (Локальный кластер HDInsight), а затем — **Hive Databases** (Базы данных Hive). В локальном кластере отобразятся базы данных **по умолчанию** и **xademo**. Если развернуть базу данных, отобразятся все таблицы, содержащиеся в ней.

    ![Снимок экрана: обозреватель сервера с развернутыми базами данных](./media/hdinsight-hadoop-emulator-visual-studio/expanded-databases.png)

2. Если развернуть таблицу, отобразятся все столбцы этой таблицы. Чтобы быстро просмотреть данные, щелкните таблицу правой кнопкой мыши и выберите пункт **Просмотреть первые 100 строк**.

    ![Снимок экрана: обозреватель сервера с развернутой таблицей и выбранным пунктом "Просмотреть первые 100 строк"](./media/hdinsight-hadoop-emulator-visual-studio/view-100.png)

### <a name="database-and-table-properties"></a>Свойства базы данных и таблицы

Вы можете просмотреть свойства базы данных или таблицы. После нажатия **Свойства** отобразятся сведения для выбранного элемента в окне свойств. Например, см. сведения, показанные на приведенном ниже снимке экрана:

![Снимок экрана: окно свойств](./media/hdinsight-hadoop-emulator-visual-studio/properties.png)

### <a name="create-a-table"></a>Создание таблицы

Чтобы создать таблицу, щелкните базу данных правой кнопкой мыши и выберите пункт **Создать таблицу**.

![Снимок экрана: обозреватель сервера с выделенным пунктом "Создать таблицу"](./media/hdinsight-hadoop-emulator-visual-studio/create-table.png)

Затем можно создать таблицу с помощью формы. В нижней части приведенного ниже снимка экрана можно видеть необработанный запрос HiveQL, который используется для создания таблицы.

![Снимок экрана: форма для создания таблицы](./media/hdinsight-hadoop-emulator-visual-studio/create-table-form.png)

## <a name="next-steps"></a>Дальнейшие действия

* [Learning the ropes of the Hortonworks Sandbox](https://hortonworks.com/hadoop-tutorial/learning-the-ropes-of-the-hortonworks-sandbox/)
* [Hadoop tutorial - Getting started with HDP](https://hortonworks.com/hadoop-tutorial/hello-world-an-introduction-to-hadoop-hcatalog-hive-and-pig/) (Руководство по началу работы с Hadoop)
