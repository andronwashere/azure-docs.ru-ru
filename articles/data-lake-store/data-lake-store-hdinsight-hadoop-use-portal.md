---
title: Создание кластеров Azure HDInsight, использующих Azure Data Lake Storage 1-го поколения, с помощью портала Azure | Документы Майкрософт
description: Создание и использование кластеров HDInsight для работы с Azure Data Lake Storage 1-го поколения с помощью портала Azure
services: data-lake-store,hdinsight
documentationcenter: ''
author: twooley
manager: mtillman
editor: cgronlun
ms.assetid: a8c45a83-a8e3-4227-8b02-1bc1e1de6767
ms.service: data-lake-store
ms.devlang: na
ms.topic: conceptual
ms.date: 05/29/2018
ms.author: twooley
ms.openlocfilehash: 6f9064c6027499fff3a8551ee60722cd66c54dc2
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "60877701"
---
# <a name="create-hdinsight-clusters-with-azure-data-lake-storage-gen1-by-using-the-azure-portal"></a>Создание кластеров HDInsight, использующих Azure Data Lake Storage 1-го поколения, с помощью портала Azure
> [!div class="op_single_selector"]
> * [Использование портала Azure](data-lake-store-hdinsight-hadoop-use-portal.md)
> * [Использование PowerShell (для хранилища по умолчанию)](data-lake-store-hdinsight-hadoop-use-powershell-for-default-storage.md)
> * [Использование PowerShell (для дополнительного хранилища)](data-lake-store-hdinsight-hadoop-use-powershell.md)
> * [Использование Resource Manager](data-lake-store-hdinsight-hadoop-use-resource-manager-template.md)
>
>

Узнайте, как использовать портал Azure для создания кластера HDInsight с учетной записью Azure Data Lake Storage 1-го поколения в качестве хранилища по умолчанию или дополнительного хранилища. Хотя дополнительное хранилище для кластера HDInsight является необязательным, бизнес-данные рекомендуется хранить в дополнительных учетных записях хранения.

## <a name="prerequisites"></a>Технические условия
Прежде чем приступить к изучению этого руководства, убедитесь, что выполнены следующие требования.

* **Подписка Azure**. Перейдите на сайт [бесплатной пробной версии Azure](https://azure.microsoft.com/pricing/free-trial/).
* **Учетная запись Data Lake Storage 1-го поколения**. Следуйте инструкциям в статье [Начало работы с Azure Data Lake Storage 1-го поколения с помощью портала Azure](data-lake-store-get-started-portal.md). В учетной записи также необходимо создать корневую папку.  В этом учебнике используется корневая папка __/clusters__.
* **Субъект-служба Azure Active Directory**. В этом руководстве приведены инструкции по созданию субъекта-службы в Azure Active Directory (Azure AD). Чтобы создать субъект-службу, необходимо быть администратором Azure AD. Если вы являетесь администратором, то можете пропустить это предварительное требование и продолжить работу с руководством.

    >[!NOTE]
    >Создать субъект-службу может только администратор Azure AD. Администратор Azure AD должен сначала создать субъект-службу, после чего вы сможете создать кластер HDInsight, использующий Data Lake Storage 1-го поколения. При создании субъекта-службы также необходимо использовать сертификат, как описано в разделе [Создание субъекта-службы с использованием сертификата](../active-directory/develop/howto-authenticate-service-principal-powershell.md#create-service-principal-with-self-signed-certificate).
    >

## <a name="create-an-hdinsight-cluster"></a>Создание кластера HDInsight

В этом разделе вы создадите кластер HDInsight с учетными записями Data Lake Storage 1-го поколения в качестве хранилища по умолчанию или дополнительного хранилища. В этой статье описывается только часть процесса настройки учетных записей Data Lake Storage 1-го поколения.  Общие сведения о создании кластеров и соответствующие процедуры см. в статье [Создание кластеров Hadoop в HDInsight](../hdinsight/hdinsight-hadoop-provision-linux-clusters.md).

### <a name="create-a-cluster-with-data-lake-storage-gen1-as-default-storage"></a>Создание кластера HDInsight, использующего Data Lake Storage 1-го поколения в качестве хранилища по умолчанию

**Создание кластера HDInsight, использующего учетную запись Data Lake Storage 1-го поколения в качестве учетной записи хранения по умолчанию**

1. Войдите на [портале Azure](https://portal.azure.com).
2. Общие сведения о создании кластеров HDInsight см. в разделе [Создание кластеров](../hdinsight/hdinsight-hadoop-create-linux-clusters-portal.md#create-clusters).
3. В колонке **Хранилище** в разделе **Тип первичного хранилища** выберите **Data Lake Storage 1-го поколения**, а затем введите указанные ниже сведения.

    ![Добавление субъекта-службы в кластер HDInsight](./media/data-lake-store-hdinsight-hadoop-use-portal/hdi.adl.1.adls.storage.png "Добавление субъекта-службы в кластер HDInsight")

    - **Выбрать учетную запись Data Lake Store**: выберите имеющуюся учетную запись Data Lake Storage 1-го поколения. Требуется существующая учетная запись Data Lake Storage 1-го поколения.  См. раздел [Предварительные требования](#prerequisites).
    - **Корневой путь**: введите путь к каталогу, в котором будут храниться файлы, связанные с кластером. На снимке экрана это путь __/clusters/myhdiadlcluster/__ , в котором должна существовать папка __/clusters__ и в котором портал создает папку *myhdicluster*.  *myhdicluster* — это имя кластера.
    - **Доступ к Data Lake Store**: настройте доступ между учетной записью Data Lake Storage 1-го поколения и кластером HDInsight. Инструкции см. в разделе "Настройка доступа к Data Lake Storage 1-го поколения".
    - **Дополнительные учетные записи хранения**: добавьте учетные записи хранения Azure в качестве дополнительных для кластера. Добавление дополнительных хранилищ Data Lake Storage 1-го поколения осуществляется путем предоставления разрешений на доступ к данным в кластере дополнительным учетным записям Data Lake Storage 1-го поколения. При этом учетная запись Data Lake Storage 1-го поколения настраивается в качестве основного типа хранилища. Ознакомьтесь с разделом "Настройка доступа к Data Lake Storage 1-го поколения".

4. В колонке **Доступ к Data Lake Store** нажмите кнопку **Выбрать** и продолжайте создание кластера, как описано в статье [Создание кластеров под управлением Linux в HDInsight с помощью портала Azure](../hdinsight/hdinsight-hadoop-create-linux-clusters-portal.md).


### <a name="create-a-cluster-with-data-lake-storage-gen1-as-additional-storage"></a>Создание кластера HDInsight, использующего Data Lake Storage 1-го поколения в качестве дополнительного хранилища

Ниже приведены инструкции по созданию кластера HDInsight, использующего учетную запись хранения Azure в качестве хранилища по умолчанию и учетную запись Data Lake Storage 1-го поколения в качестве дополнительного хранилища.

**Создание кластера HDInsight, использующего учетную запись Data Lake Storage 1-го поколения в качестве учетной записи дополнительного хранилища**

1. Войдите на [портале Azure](https://portal.azure.com).
2. Общие сведения о создании кластеров HDInsight см. в разделе [Создание кластеров](../hdinsight/hdinsight-hadoop-create-linux-clusters-portal.md#create-clusters).
3. В колонке **Хранилище** в разделе **Тип первичного хранилища** выберите **Хранилище Azure**, а затем введите указанные ниже сведения.

    ![Добавление субъекта-службы в кластер HDInsight](./media/data-lake-store-hdinsight-hadoop-use-portal/hdi.adl.1.png "Добавление субъекта-службы в кластер HDInsight")

    - **Метод выбора**: выберите один из указанных ниже вариантов.

        * Выберите **Мои подписки**, чтобы указать учетную запись хранения, которая является частью вашей подписки Azure, а затем выберите учетную запись хранения.
        * Чтобы указать учетную запись хранения, не входящую в вашу подписку Azure, выберите **Ключ доступа**, а затем введите данные внешней учетной записи хранения.

    - **Контейнер по умолчанию**: используйте значение по умолчанию или укажите собственное имя.

    - "Дополнительные учетные записи хранения": добавьте учетные записи хранения Azure в качестве дополнительного хранилища.
    - "Доступ к Data Lake Store": настройте доступ между учетной записью Data Lake Storage 1-го поколения и кластером HDInsight. Инструкции см. в разделе [Настройка доступа к Data Lake Storage 1-го поколения](#configure-data-lake-storage-gen1-access).

## <a name="configure-data-lake-storage-gen1-access"></a>Настройка доступа к Data Lake Storage 1-го поколения 

В этом разделе вы настроите доступ к Data Lake Storage 1-го поколения из кластеров HDInsight с помощью субъекта-службы Azure Active Directory. 

### <a name="specify-a-service-principal"></a>Указание субъекта-службы

На портале Azure можно использовать существующий субъект-службу или создать новый.

**Создание субъекта-службы на портале Azure**

1. В колонке "Хранилище" щелкните **Доступ к Data Lake Store**.
2. В колонке **Доступ к Data Lake Storage 1-го поколения** щелкните **Создать**.
3. Щелкните **Субъект-служба**, а затем выполните инструкции по созданию субъекта-службы.
4. Скачайте сертификат, если его потребуется использовать в будущем. Скачать сертификат удобно, если вы холите использовать этот же субъект-службу для создания дополнительных кластеров HDInsight.

    ![Добавление субъекта-службы в кластер HDInsight](./media/data-lake-store-hdinsight-hadoop-use-portal/hdi.adl.2.png "Добавление субъекта-службы в кластер HDInsight")

4. Щелкните **Доступ**, чтобы настроить доступ к папке.  См. раздел [Настройка разрешений для файлов](#configure-file-permissions).


**Использование существующего субъекта-службы на портале Azure**

1. Щелкните **Доступ к Data Lake Store**.
1. В колонке **Доступ к Data Lake Storage 1-го поколения** щелкните **Использовать имеющийся**.
2. Щелкните **Субъект-служба**, а затем выберите субъект-службу. 
3. Отправьте связанный с выбранным субъектом-службой сертификат (PFX-файл) и введите пароль этого сертификата.

    ![Добавление субъекта-службы в кластер HDInsight](./media/data-lake-store-hdinsight-hadoop-use-portal/hdi.adl.5.png "Добавление субъекта-службы в кластер HDInsight")

4. Щелкните **Доступ**, чтобы настроить доступ к папке.  См. раздел [Настройка разрешений для файлов](#configure-file-permissions).


### <a name="configure-file-permissions"></a>Настройка разрешений для файлов

Настройки зависят от того, используется ли учетная запись как хранилище по умолчанию или как дополнительная учетная запись хранения.

- В качестве хранилища по умолчанию:

    - разрешение на корневом уровне учетной записи Data Lake Storage 1-го поколения;
    - разрешение на корневом уровне хранилища кластера HDInsight. Например, в этом учебнике это папка __/clusters__.
- В качестве дополнительного хранилища:

    - разрешение на доступ к папкам, в которых находятся требуемые файлы.

**Назначение разрешения на корневом уровне учетной записи Data Lake Storage 1-го поколения**

1. В колонке **Доступ к Data Lake Storage 1-го поколения** щелкните **Доступ**. Откроется колонка **Выбор разрешений для файла**. В ней отображены все учетные записи Data Lake Storage 1-го поколения в вашей подписке.
2. Наведите указатель мыши (но не щелкайте) на имя учетной записи Data Lake Storage 1-го поколения, чтобы отобразился флажок, а затем установите этот флажок.

    ![Добавление субъекта-службы в кластер HDInsight](./media/data-lake-store-hdinsight-hadoop-use-portal/hdi.adl.3.png "Добавление субъекта-службы в кластер HDInsight")

   По умолчанию выбраны разрешения __Чтение__, __Запись__ и __Выполнение__.

3. В нижней части страницы щелкните **Выбрать**.
4. Чтобы назначить разрешение, щелкните **Выполнить**.
5. Нажмите кнопку **Done**(Готово).

**Назначение разрешения на корневом уровне кластера HDInsight**

1. В колонке **Доступ к Data Lake Storage 1-го поколения** щелкните **Доступ**. Откроется колонка **Выбор разрешений для файла**. В ней отображены все учетные записи Data Lake Storage 1-го поколения в вашей подписке.
1. В колонке **Выбор разрешений для файла** щелкните имя учетной записи Data Lake Storage 1-го поколения, чтобы увидеть ее содержимое.
2. Выберите корень хранилища кластера HDInsight, установив флажок слева от папки. В соответствии с предыдущим снимком экрана корнем хранилища кластера является папка __/clusters__, указанная при выборе Data Lake Storage 1-го поколения в качестве хранилища по умолчанию.
3. Задайте разрешения для папки.  По умолчанию выбраны разрешения на чтение, запись и выполнение.
4. В нижней части страницы щелкните **Выбрать**.
5. Нажмите кнопку **Запустить**.
6. Нажмите кнопку **Done**(Готово).

При использовании Data Lake Storage 1-го поколения в качестве дополнительного хранилища необходимо назначить разрешение только для папок, к которым нужен доступ из кластера HDInsight. Например, на снимке экрана ниже предоставляется доступ только к папке **mynewfolder** в учетной записи Data Lake Storage 1-го поколения.

![Назначение разрешений субъекта-службы для кластера HDInsight](./media/data-lake-store-hdinsight-hadoop-use-portal/hdi.adl.3-1.png "Назначение разрешений субъекта-службы для кластера HDInsight")


## <a name="verify-cluster-set-up"></a>Проверка настроек кластера

После завершения настройки кластера проверьте результат в колонке кластера, выполнив одно или оба указанных ниже действия.

* Чтобы проверить, является ли указанная вами учетная запись Data Lake Storage 1-го поколения связанным хранилищем для кластера, щелкните **Учетные записи хранения** в левой области.

    ![Добавление субъекта-службы в кластер HDInsight](./media/data-lake-store-hdinsight-hadoop-use-portal/hdi.adl.6-1.png "Добавление субъекта-службы в кластер HDInsight")

* Чтобы проверить, правильно ли связан субъект-служба с кластером HDInsight, щелкните **Доступ к Data Lake Storage 1-го поколения** в левой области.

    ![Добавление субъекта-службы в кластер HDInsight](./media/data-lake-store-hdinsight-hadoop-use-portal/hdi.adl.6.png "Добавление субъекта-службы в кластер HDInsight")


## <a name="examples"></a>Примеры

Настроив кластер для работы с Data Lake Storage 1-го поколения в качестве хранилища, вы можете изучить следующие примеры использования кластера HDInsight для анализа данных, хранимых в Data Lake Storage 1-го поколения.

### <a name="run-a-hive-query-against-data-in-a-data-lake-storage-gen1-account-as-primary-storage"></a>Выполнение запроса Hive к данным, хранящимся в Data Lake Storage 1-го поколения (основное хранилище)

Чтобы выполнить запрос Hive, используйте интерфейс представлений Hive, доступный на портале Ambari. Инструкции по использованию представлений Ambari Hive см. в статье [Использование представления Hive с Hadoop в HDInsight](../hdinsight/hadoop/apache-hadoop-use-hive-ambari-view.md).

При работе с данными в Data Lake Storage 1-го поколения изменить можно лишь несколько строк.

Например, если вы используете созданный кластер, в котором основным хранилищем является Data Lake Storage 1-го поколения, то путь к данным будет следующим: *adl://<имя_учетной_записи_data_lake_storage_gen1>/azuredatalakestore.net/path/to/file*. Запрос Hive для создания таблицы на основе демонстрационных данных, хранящихся в учетной записи Data Lake Storage 1-го поколения, выглядит следующим образом:

    CREATE EXTERNAL TABLE websitelog (str string) LOCATION 'adl://hdiadlsg1storage.azuredatalakestore.net/clusters/myhdiadlcluster/HdiSamples/HdiSamples/WebsiteLogSampleData/SampleLog/'

Описание
* `adl://hdiadlsg1storage.azuredatalakestore.net/` — корневой элемент учетной записи Data Lake Storage 1-го поколения;
* `/clusters/myhdiadlcluster` — корень данных кластера, указанный при создании кластера;
* `/HdiSamples/HdiSamples/WebsiteLogSampleData/SampleLog/` — расположение примера файла, который используется в запросе.

### <a name="run-a-hive-query-against-data-in-a-data-lake-storage-gen1-account-as-additional-storage"></a>Выполнение запроса Hive к данным, хранящимся в Data Lake Storage 1-го поколения (дополнительное хранилище)

Если созданный кластер в качестве хранилища по умолчанию использует хранилище BLOB-объектов, то демонстрационные данные не будут находиться в учетной записи Data Lake Storage 1-го поколения, используемой в качестве дополнительного хранилища. В этом случае сначала передайте данные из хранилища BLOB-объектов в Data Lake Storage 1-го поколения, а затем выполните запросы, как показано в предыдущем примере.

Сведения о том, как скопировать данные из хранилища BLOB-объектов в Data Lake Storage 1-го поколения, см. в следующих статьях:

* [Использование Distcp для копирования данных между BLOB-объектами и Data Lake Storage 1-го поколения](data-lake-store-copy-data-wasb-distcp.md)
* [Использование AdlCopy для копирования данных из BLOB-объектов в Data Lake Storage 1-го поколения](data-lake-store-copy-data-azure-storage-blob.md)

### <a name="use-data-lake-storage-gen1-with-a-spark-cluster"></a>Использование Data Lake Storage 1-го поколения с кластером Spark
Кластер Spark можно использовать для выполнения заданий Spark с данными, хранящимися в Data Lake Storage 1-го поколения. Дополнительные сведения см. в статье [Использование кластера HDInsight Spark для анализа данных в Data Lake Storage 1-го поколения](../hdinsight/spark/apache-spark-use-with-data-lake-store.md).


### <a name="use-data-lake-storage-gen1-in-a-storm-topology"></a>Использование Data Lake Storage 1-го поколения в топологии Storm
Data Lake Storage 1-го поколения можно использовать для записи данных из топологии Storm. Инструкции по реализации этого сценария см. в статье [Использование Azure Data Lake Storage 1-го поколения с Apache Storm в HDInsight](../hdinsight/storm/apache-storm-write-data-lake-store.md).

## <a name="see-also"></a>См. также
* [Использование Data Lake Storage 1-го поколения с кластерами Azure HDInsight](../hdinsight/hdinsight-hadoop-use-data-lake-store.md)
* [PowerShell: создание кластера HDInsight для работы с Data Lake Storage 1-го поколения](data-lake-store-hdinsight-hadoop-use-powershell.md)

[makecert]: https://msdn.microsoft.com/library/windows/desktop/ff548309(v=vs.85).aspx
[pvk2pfx]: https://msdn.microsoft.com/library/windows/desktop/ff550672(v=vs.85).aspx
