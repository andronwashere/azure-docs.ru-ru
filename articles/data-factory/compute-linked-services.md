---
title: Вычислительные среды, поддерживаемые фабрикой данных Azure | Документация Майкрософт
description: Сведения о вычислительных средах, которые можно использовать в конвейерах фабрики данных Azure (например, Azure HDInsight) для преобразования или обработки данных.
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/15/2019
author: nabhishek
ms.author: abnarain
manager: craigg
ms.openlocfilehash: 0e7405e48307091ff5df12096d49a00c011e2de3
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "66480430"
---
# <a name="compute-environments-supported-by-azure-data-factory"></a>Вычислительные среды, поддерживаемые фабрикой данных Azure
В этой статье описываются различные среды вычислений, которые можно использовать для обработки и преобразования данных. Здесь содержатся также сведения о различных конфигурациях (конфигурациях по запросу и ваших собственных), которые поддерживаются фабрикой данных при настройке связанных служб, связывающих эти среды вычислений с фабрикой данных Azure.

Следующая таблица содержит список вычислительных сред, поддерживаемых фабрикой данных, и доступных в них действий. 

| Вычислительная среда                                          | activities                                                   |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| [Кластер HDInsight по запросу](#azure-hdinsight-on-demand-linked-service) или [собственный кластер HDInsight](#azure-hdinsight-linked-service) | [Hive](transform-data-using-hadoop-hive.md), [Pig](transform-data-using-hadoop-pig.md), [Spark](transform-data-using-spark.md), [MapReduce](transform-data-using-hadoop-map-reduce.md), [потоковая передача Hadoop](transform-data-using-hadoop-streaming.md) |
| [Пакетная служба Azure](#azure-batch-linked-service)                   | [Настраиваемая](transform-data-using-dotnet-custom-activity.md)     |
| [машинное обучение Azure](#azure-machine-learning-linked-service) | [Создание прогнозирующих конвейеров с помощью машинного обучения Azure и фабрики данных Azure](transform-data-using-machine-learning.md) |
| [Аналитика озера данных Azure](#azure-data-lake-analytics-linked-service) | [Аналитика озера данных U-SQL](transform-data-using-data-lake-analytics.md) |
| [Azure SQL](#azure-sql-database-linked-service), [хранилище данных Azure SQL](#azure-sql-data-warehouse-linked-service), [SQL Server](#sql-server-linked-service) | [Хранимая процедура](transform-data-using-stored-procedure.md) |
| [Azure Databricks](#azure-databricks-linked-service)         | [Notebook](transform-data-databricks-notebook.md), [Jar](transform-data-databricks-jar.md), [Python](transform-data-databricks-python.md) |

>  

## <a name="on-demand-hdinsight-compute-environment"></a>Вычислительная среда HDInsight по запросу
В конфигурации такого типа среда вычислений полностью управляется службой фабрики данных Azure. Среда автоматически создается службой фабрики данных перед отправкой задания для обработки данных и удаляется после его выполнения. Вы можете создать связанную службу для среды вычислений по запросу, настроить ее и управлять детализированными параметрами выполнения задания, управления кластером и параметрами действий начальной загрузки.

> [!NOTE]
> Конфигурации по запросу в настоящее время поддерживаются только для кластеров Azure HDInsight. Azure Databricks также поддерживает задания по запросу, используя кластер заданий. Для получения дополнительных сведений ознакомьтесь с разделом [Связанная служба Azure Databricks](#azure-databricks-linked-service).

## <a name="azure-hdinsight-on-demand-linked-service"></a>Связанная служба Azure HDInsight по запросу
Для обработки данных служба фабрики данных Azure автоматически создает кластер HDInsight по запросу. Кластер создается в том же регионе, что и учетная запись хранения (свойство linkedServiceName в JSON), связанная с кластером. Это должна быть стандартная универсальная учетная запись хранения Azure. 

Обратите внимание на следующие **важные** моменты, касающиеся связанной службы HDInsight по запросу.

* Кластер HDInsight по запросу создается в вашей подписке Azure. Сразу после запуска кластер отобразится на портале Azure. 
* Журналы заданий, которые выполняются в кластере HDInsight по запросу, копируются в учетную запись хранения, связанную с кластером HDInsight. ClusterUserName, clusterPassword, clusterSshUserName, clusterSshPassword, заданные в определении связанной службы, используются для входа в кластер, чтобы выполнить глубокую диагностику в течение жизненного цикла кластера. 
* Вы оплачиваете только время, когда кластер HDInsight работает и выполняет задания.
* Со связанными службами Azure HDInsight, которые доступны по запросу, можно использовать **действие скрипта**.  

> [!IMPORTANT]
> Подготовка к работе кластера HDInsight Azure по запросу обычно занимает от **20 минут**.

### <a name="example"></a>Пример
Представленный ниже код JSON определяет связанную службу HDInsight по запросу под управлением Linux. Служба фабрики данных автоматически создает кластер HDInsight **на основе Linux** для обработки необходимых действий. 

```json
{
  "name": "HDInsightOnDemandLinkedService",
  "properties": {
    "type": "HDInsightOnDemand",
    "typeProperties": {
      "clusterType": "hadoop",
      "clusterSize": 1,
      "timeToLive": "00:15:00",
      "hostSubscriptionId": "<subscription ID>",
      "servicePrincipalId": "<service principal ID>",
      "servicePrincipalKey": {
        "value": "<service principal key>",
        "type": "SecureString"
      },
      "tenant": "<tenent id>",
      "clusterResourceGroup": "<resource group name>",
      "version": "3.6",
      "osType": "Linux",
      "linkedServiceName": {
        "referenceName": "AzureStorageLinkedService",
        "type": "LinkedServiceReference"
      }
    },
    "connectVia": {
      "referenceName": "<name of Integration Runtime>",
      "type": "IntegrationRuntimeReference"
    }
  }
}
```

> [!IMPORTANT]
> Кластер HDInsight создает **контейнер по умолчанию** в хранилище BLOB-объектов, указанном в коде JSON (**linkedServiceName**). При удалении кластера HDInsight этот контейнер не удаляется. В этом весь замысел. Если используется связанная служба HDInsight по запросу, кластер HDInsight создается всякий раз, когда нужно обработать срез данных (если не используется динамический кластер**timeToLive**), после чего кластер удаляется. 
>
> По мере выполнения действий количество контейнеров в хранилище BLOB-объектов будет увеличиваться. Если эти контейнеры не используются для устранения неполадок с заданиями, удалите их — это позволит сократить расходы на хранение. Имена этих контейнеров указаны по шаблону `adf**yourdatafactoryname**-**linkedservicename**-datetimestamp`. Для удаления контейнеров в хранилище BLOB-объектов Azure используйте такие инструменты, как [Microsoft Storage Explorer](https://storageexplorer.com/) .
>
> 

### <a name="properties"></a>properties
| Свойство                     | ОПИСАНИЕ                              | Обязательно для заполнения |
| ---------------------------- | ---------------------------------------- | -------- |
| type                         | Свойству type необходимо присвоить значение **HDInsightOnDemand**. | Да      |
| clusterSize                  | Общее количество рабочих узлов и узлов данных в кластере. Кластер HDInsight создается с 2 головными узлами и количеством рабочих узлов, заданным в этом свойстве. Узлы имеют размер Standard_D3 с 4 ядрами, то есть кластер с 4 рабочими узлами использует 24 ядра (4\*4 = 16 для рабочих узлов + 2\*4 = 8 для головных узлов). Дополнительные сведения см. в статье [Установка кластеров в HDInsight с использованием Hadoop, Spark, Kafka и других технологий](../hdinsight/hdinsight-hadoop-provision-linux-clusters.md). | Да      |
| linkedServiceName            | Связанная служба хранилища Azure, которую кластер по запросу должен использовать для хранения и обработки данных. Кластер HDInsight создается в том же регионе, что и учетная запись хранения Azure. Azure HDInsight имеет ограничение на общее количество ядер, которые можно использовать в каждом поддерживаемом регионе Azure. Убедитесь, что у вас есть достаточное количество квот ядер в необходимом регионе Azure в соответствии с необходимым размером кластера. Дополнительные сведения см. в статье [Установка кластеров в HDInsight с использованием Hadoop, Spark, Kafka и других технологий](../hdinsight/hdinsight-hadoop-provision-linux-clusters.md).<p>В настоящее время недоступно создание кластера HDInsight по запросу, который использует в качестве хранилища Azure Data Lake Store. Чтобы сохранить данные результатов обработки HDInsight в Azure Data Lake Store, воспользуйтесь действием копирования и скопируйте данные из хранилища BLOB-объектов Azure в Azure Data Lake Store. </p> | Да      |
| clusterResourceGroup         | В этой группе ресурсов создается кластер HDInsight. | Да      |
| timeToLive                   | Допустимое время простоя кластера HDInsight по запросу. Указывает, как долго кластер HDInsight по запросу остается активным после выполнения действия, если в кластере нет других активных заданий. Минимальное допустимое значение — 5 минут (00:05:00).<br/><br/>Например, если выполнение действия занимает 6 минут, а значение свойства timetolive равно 5 минутам, кластер остается активным в течение 5 минут по истечении 6-минутного выполнения действия. Если в течение этих 6 минут выполняется другое действие, оно обрабатывается в том же кластере.<br/><br/>Создание кластера HDInsight по запросу является ресурсоемкой операцией и может занять некоторое время. При необходимости используйте этот параметр для повышения производительности фабрики данных путем повторного использования кластера HDInsight по запросу.<br/><br/>Если значение timetolive равно 0, кластер удаляется сразу после выполнения действия. С другой стороны, если задать большое значение, кластер может простаивать и вы не сможете войти в систему, чтобы устранить неполадки. Тем не менее это может привести к большим затратам. Поэтому необходимо установить соответствующее значение в соответствии со своими потребностями.<br/><br/>Если значение свойства timetolive задано правильно, один и тот же экземпляр кластера HDInsight по запросу могут совместно использовать несколько конвейеров. | Да      |
| clusterType                  | Тип создаваемого кластера HDInsight. Допустимые значения: Hadoop и Spark. Если не указано другое, по умолчанию используется значение Hadoop. Кластеры с корпоративными пакетами безопасности нельзя создать по запросу, вместо этого используйте существующий кластер или ваше собственное вычисление, подробнее см. в разделе [Связанная служба Azure HDInsight](#azure-hdinsight-linked-service). | Нет       |
| версия                      | Версия кластера HDInsight. Если не указано другое, используется текущая заданная версия HDInsight по умолчанию. | Нет       |
| hostSubscriptionId           | Идентификатор подписки Azure, используемый для создания кластера HDInsight. Если не указано другое, будет использоваться идентификатор подписки контекста входа в Azure. | Нет       |
| clusterNamePrefix           | Префикс имени кластера HDI. Метка времени автоматически добавляется в конец имени кластера.| Нет       |
| sparkVersion                 | Версия Spark, если используется тип кластера Spark. | Нет       |
| additionalLinkedServiceNames | Указывает дополнительные учетные записи хранения для связанной службы HDInsight, чтобы служба фабрики данных могла регистрировать их от вашего имени. Эти учетные записи хранения должны находиться в том же регионе, что и кластер HDInsight, который создается в одном регионе с учетной записью хранения, указанной параметром linkedServiceName. | Нет       |
| osType                       | Тип операционной системы. Допустимые значения: Linux и Windows (только для HDInsight 3.3). Значение по умолчанию — Linux. | Нет       |
| hcatalogLinkedServiceName    | Имя связанной службы SQL Azure, указывающие на базу данных HCatalog. При создании кластера HDInsight по запросу используется база данных SQL Azure в качестве хранилища метаданных. | Нет       |
| connectVia                   | Среда выполнения интеграции, используемая для отправки действий в связанную службу HDInsight. Для связанной службы HDInsight по запросу поддерживается только среда выполнения интеграции Azure. Если не указано другое, по умолчанию используется интегрированная среда выполнения Azure. | Нет       |
| clusterUserName                   | Имя пользователя для доступа к кластеру. | Нет       |
| clusterPassword                   | Пароль в строке защищенного типа для доступа к кластеру. | Нет       |
| clusterSshUserName         | Имя пользователя для установки удаленного подключения SSH к узлу кластера (для Linux). | Нет       |
| clusterSshPassword         | Пароль в строке защищенного типа для установки удаленного подключения SSH к узлу кластера (для Linux). | Нет       |
| scriptActions | Во время создания кластера по запросу укажите скрипт для [настроек кластера HDInsight](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-customize-cluster-linux). <br />Сейчас средство разработки пользовательского интерфейса Фабрики данных Azure поддерживает указание только 1 действия скрипта, но вы можете обойти это ограничение в JSON (укажите несколько действий скрипта в формате JSON). | Нет |


> [!IMPORTANT]
> HDInsight поддерживает несколько версий кластера Hadoop, которые могут быть развернуты. Каждая из версий создает конкретную версию платформы HortonWorks Data Platform (HDP) и набор компонентов, содержащихся в этой версии. Список поддерживаемых версий HDInsight продолжает обновляться, чтобы предоставлять новейшие компоненты и исправления для экосистемы Hadoop. Ознакомьтесь с актуальной информацией о [поддерживаемой версии HDInsight и типе ОС](../hdinsight/hdinsight-component-versioning.md#supported-hdinsight-versions), чтобы убедиться, что используется поддерживаемая версия HDInsight. 
>
> [!IMPORTANT]
> Сейчас службы, связанные с HDInsight, не поддерживают HBase, интерактивные запросы (Hive LLAP), Storm. 
>
> 

#### <a name="additionallinkedservicenames-json-example"></a>Пример кода JSON additionalLinkedServiceNames

```json
"additionalLinkedServiceNames": [{
    "referenceName": "MyStorageLinkedService2",
    "type": "LinkedServiceReference"          
}]
```

### <a name="service-principal-authentication"></a>Проверка подлинности субъекта-службы

Для связанной службы HDInsight по запросу необходимо выполнить проверку подлинности субъекта-службы, чтобы создать кластеры HDInsight от вашего имени. Чтобы использовать проверку подлинности субъекта-службы, зарегистрируйте сущность приложения в Azure Active Directory (Azure AD) и предоставьте ей роль **участника** подписки или группы ресурсов, в которой создан кластер HDInsight. Подробные инструкции см. в статье [Создание приложения Azure Active Directory и субъекта-службы с доступом к ресурсам с помощью портала](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-create-service-principal-portal). Запишите следующие значения, которые используются для определения связанной службы:

- Идентификатор приложения
- Ключ приложения 
- Tenant ID

Используйте проверку подлинности на основе субъекта-службы, указав следующие свойства:

| Свойство                | ОПИСАНИЕ                              | Обязательно для заполнения |
| :---------------------- | :--------------------------------------- | :------- |
| **servicePrincipalId**  | Укажите идентификатора клиента приложения.     | Да      |
| **servicePrincipalKey** | Укажите ключ приложения.           | Да      |
| **tenant**              | Укажите сведения о клиенте (доменное имя или идентификатор клиента), в котором находится приложение. Эти сведения можно получить, наведя указатель мыши на правый верхний угол страницы портала Azure. | Да      |

### <a name="advanced-properties"></a>Дополнительные свойства

Для детализированной настройки кластера HDInsight по запросу можно также указать следующие свойства.

| Свойство               | ОПИСАНИЕ                              | Обязательно для заполнения |
| :--------------------- | :--------------------------------------- | :------- |
| coreConfiguration      | Задает параметры конфигурации ядра (как в файле core-site.xml) для создаваемого кластера HDInsight. | Нет       |
| hBaseConfiguration     | Задает основные параметры конфигурации HBase (hbase-site.xml) для кластера HDInsight. | Нет       |
| hdfsConfiguration      | Задает основные параметры конфигурации HDFS (hdfs-site.xml) для кластера HDInsight. | Нет       |
| hiveConfiguration      | Задает основные параметры конфигурации Hive (hive-site.xml) для кластера HDInsight. | Нет       |
| mapReduceConfiguration | Задает параметры конфигурации MapReduce (mapred-site.xml) для кластера HDInsight. | Нет       |
| oozieConfiguration     | Задает параметры конфигурации Oozie (oozie-site.xml) для кластера HDInsight. | Нет       |
| stormConfiguration     | Задает параметры конфигурации Storm (storm-site.xml) для кластера HDInsight. | Нет       |
| yarnConfiguration      | Задает параметры конфигурации Yarn (yarn-site.xml) для кластера HDInsight. | Нет       |

#### <a name="example--on-demand-hdinsight-cluster-configuration-with-advanced-properties"></a>Пример: конфигурация кластера HDInsight по запросу с дополнительными свойствами

```json
{
    "name": " HDInsightOnDemandLinkedService",
    "properties": {
      "type": "HDInsightOnDemand",
      "typeProperties": {
          "clusterSize": 16,
          "timeToLive": "01:30:00",
          "hostSubscriptionId": "<subscription ID>",
          "servicePrincipalId": "<service principal ID>",
          "servicePrincipalKey": {
            "value": "<service principal key>",
            "type": "SecureString"
          },
          "tenant": "<tenent id>",
          "clusterResourceGroup": "<resource group name>",
          "version": "3.6",
          "osType": "Linux",
          "linkedServiceName": {
              "referenceName": "AzureStorageLinkedService",
              "type": "LinkedServiceReference"
            },
            "coreConfiguration": {
                "templeton.mapper.memory.mb": "5000"
            },
            "hiveConfiguration": {
                "templeton.mapper.memory.mb": "5000"
            },
            "mapReduceConfiguration": {
                "mapreduce.reduce.java.opts": "-Xmx4000m",
                "mapreduce.map.java.opts": "-Xmx4000m",
                "mapreduce.map.memory.mb": "5000",
                "mapreduce.reduce.memory.mb": "5000",
                "mapreduce.job.reduce.slowstart.completedmaps": "0.8"
            },
            "yarnConfiguration": {
                "yarn.app.mapreduce.am.resource.mb": "5000",
                "mapreduce.map.memory.mb": "5000"
            },
            "additionalLinkedServiceNames": [{
                "referenceName": "MyStorageLinkedService2",
                "type": "LinkedServiceReference"          
            }]
        }
    },
      "connectVia": {
      "referenceName": "<name of Integration Runtime>",
      "type": "IntegrationRuntimeReference"
    }
}
```

### <a name="node-sizes"></a>Размеры узлов
Вы можете указать размеры головных узлов, узлов данных и узлов zookeeper, используя следующие свойства. 

| Свойство          | ОПИСАНИЕ                              | Обязательно для заполнения |
| :---------------- | :--------------------------------------- | :------- |
| headNodeSize      | Указывает размер головного узла. По умолчанию используется значение Standard_D3. Дополнительные сведения см. в разделе **Указание размеров узлов**. | Нет       |
| dataNodeSize      | Задает размер узла данных. По умолчанию используется значение Standard_D3. | Нет       |
| zookeeperNodeSize | Задает размер узла Zoo Keeper. По умолчанию используется значение Standard_D3. | Нет       |

#### <a name="specifying-node-sizes"></a>Указание размеров узлов
Сведения о строковых значениях, необходимых для задания указанных выше свойств, см. в статье [Размеры виртуальных машин в Azure](../virtual-machines/linux/sizes.md). Значения должны соответствовать указанным в статье **командлетам и API**. Как указано в статье, узел данных большого размера (по умолчанию) имеет 7 ГБ памяти, которых может быть недостаточно для вашего сценария. 

Если вы хотите создать головные узлы и рабочие узлы размера D4, укажите **Standard_D4** в качестве значения для свойств headNodeSize и dataNodeSize. 

```json
"headNodeSize": "Standard_D4",    
"dataNodeSize": "Standard_D4",
```

Если для этих свойств указано неверное значение, может возникнуть следующая **ошибка**: "Не удалось создать кластер. Исключение: не удается завершить операцию создания кластера. Операция завершилась ошибкой с кодом 400. Оставшееся состояние кластера: "Ошибка". Сообщение: "PreClusterCreationValidationFailure". При появлении этой ошибки убедитесь, что вы используете имя **командлета или API** из таблицы в статье [Размеры виртуальных машин в Azure](../virtual-machines/linux/sizes.md).        

## <a name="bring-your-own-compute-environment"></a>Использование собственной среды вычислений
В конфигурации такого типа вы можете зарегистрировать уже существующую среду вычислений как связанную службу в фабрике данных. Вы будете управлять средой вычислений, а служба фабрики данных — использовать ее для выполнения действий.

Такая конфигурация поддерживается в следующих средах вычислений:

* Azure HDInsight
* Пакетная служба Azure
* Машинное обучение Azure
* Azure Data Lake Analytics
* База данных SQL Azure, хранилища данных SQL Azure, SQL Server

## <a name="azure-hdinsight-linked-service"></a>Связанная служба Azure HDInsight
Чтобы зарегистрировать собственный кластер HDInsight в фабрике данных, вы можете создать связанную службу Azure HDInsight.

### <a name="example"></a>Пример

```json
{
    "name": "HDInsightLinkedService",
    "properties": {
      "type": "HDInsight",
      "typeProperties": {
        "clusterUri": " https://<hdinsightclustername>.azurehdinsight.net/",
        "userName": "username",
        "password": {
            "value": "passwordvalue",
            "type": "SecureString"
          },
        "linkedServiceName": {
              "referenceName": "AzureStorageLinkedService",
              "type": "LinkedServiceReference"
        }
      },
      "connectVia": {
        "referenceName": "<name of Integration Runtime>",
        "type": "IntegrationRuntimeReference"
      }
    }
  }
```

### <a name="properties"></a>properties
| Свойство          | ОПИСАНИЕ                                                  | Обязательно для заполнения |
| ----------------- | ------------------------------------------------------------ | -------- |
| type              | Свойству type необходимо присвоить значение **HDInsight**.            | Да      |
| clusterUri        | Универсальный код ресурса (URI) кластера HDInsight.                            | Да      |
| username          | Укажите имя пользователя, которое будет использоваться для подключения к существующему кластеру HDInsight. | Да      |
| password          | Укажите пароль для учетной записи пользователя.                       | Да      |
| linkedServiceName | Имя связанной службы для службы хранилища Azure, которая обращается к хранилищу BLOB-объектов Azure, используемому кластером HDInsight. <p>В настоящее время для этого свойства невозможно указать связанную службу Azure Data Lake Store. Вы можете получать доступ к данным в Azure Data Lake Store из сценариев Hive или Pig, если кластер HDInsight имеет доступ к Data Lake Store. </p> | Да      |
| isEspEnabled      | Укажите значение *true*, если кластер HDInsight доступный с [Корпоративными пакетами безопасности](https://docs.microsoft.com/azure/hdinsight/domain-joined/apache-domain-joined-introduction). Значение по умолчанию — *false*. | Нет       |
| connectVia        | Среда выполнения интеграции, используемая для отправки действий в связанную службу. Вы можете использовать среду выполнения интеграции Azure или локальную среду выполнения интеграции. Если не указано другое, по умолчанию используется интегрированная среда выполнения Azure. <br />Для кластеров HDInsight с Корпоративными пакетами безопасности (ESP) следует использовать локальную среду выполнения интеграции, которая имеет "прямую видимость" кластера или должна развертываться в той же виртуальной сети, что и кластер HDInsight для ESP. | Нет       |

> [!IMPORTANT]
> HDInsight поддерживает несколько версий кластера Hadoop, которые могут быть развернуты. Каждая из версий создает конкретную версию платформы HortonWorks Data Platform (HDP) и набор компонентов, содержащихся в этой версии. Список поддерживаемых версий HDInsight продолжает обновляться, чтобы предоставлять новейшие компоненты и исправления для экосистемы Hadoop. Ознакомьтесь с актуальной информацией о [поддерживаемой версии HDInsight и типе ОС](../hdinsight/hdinsight-component-versioning.md#supported-hdinsight-versions), чтобы убедиться, что используется поддерживаемая версия HDInsight. 
>
> [!IMPORTANT]
> Сейчас службы, связанные с HDInsight, не поддерживают HBase, интерактивные запросы (Hive LLAP), Storm. 
>
> 

## <a name="azure-batch-linked-service"></a>Связанная пакетная служба Azure

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Чтобы зарегистрировать пакетный пул виртуальных машин (ВМ) в фабрике данных, можно создать связанную пакетную службу Azure. Вы можете выполнить пользовательские действия с помощью пакетной службы Azure.

Если вы еще не знакомы с пакетной службой Azure, см. следующие статьи.

* [Основные сведения о пакетной службе Azure](../batch/batch-technical-overview.md) — общие сведения о пакетной службе Azure.
* [Новый AzBatchAccount](/powershell/module/az.batch/New-azBatchAccount) командлет, чтобы создать учетную запись пакетной службы Azure (или) [портала Azure](../batch/batch-account-create-portal.md) для создания учетной записи пакетной службы Azure, с помощью портала Azure. Подробные инструкции по использованию этого командлета см. в статье [Using PowerShell to manage Azure Batch Account](https://blogs.technet.com/b/windowshpc/archive/2014/10/28/using-azure-powershell-to-manage-azure-batch-account.aspx) (Использование PowerShell для управления учетной записью пакетной службы Azure).
* [Новый AzBatchPool](/powershell/module/az.batch/New-AzBatchPool) командлет, чтобы создать пул пакетной службы Azure.

### <a name="example"></a>Пример

```json
{
    "name": "AzureBatchLinkedService",
    "properties": {
      "type": "AzureBatch",
      "typeProperties": {
        "accountName": "batchaccount",
        "accessKey": {
          "type": "SecureString",
          "value": "access key"
        },
        "batchUri": "https://batchaccount.region.batch.azure.com",
        "poolName": "poolname",
        "linkedServiceName": {
          "referenceName": "StorageLinkedService",
          "type": "LinkedServiceReference"
        }
      },
      "connectVia": {
        "referenceName": "<name of Integration Runtime>",
        "type": "IntegrationRuntimeReference"
      }
    }
  }
```


### <a name="properties"></a>properties
| Свойство          | ОПИСАНИЕ                              | Обязательно для заполнения |
| ----------------- | ---------------------------------------- | -------- |
| type              | Свойству type необходимо присвоить значение **AzureBatch**. | Да      |
| accountName       | Имя учетной записи пакетной службы Azure         | Да      |
| accessKey         | Ключ доступа к учетной записи пакетной службы Azure.  | Да      |
| batchUri          | URL-адрес учетной записи пакетной службы Azure в формате https://*batchaccountname.region*.batch.azure.com. | Да      |
| poolName          | Имя пула виртуальных машин.    | Да      |
| linkedServiceName | Имя связанной службы хранилища Azure, которая ассоциируется с этой связанной пакетной службой Azure. Эта связанная служба используется для промежуточных файлов, необходимых для выполнения действий. | Да      |
| connectVia        | Среда выполнения интеграции, используемая для отправки действий в связанную службу. Вы можете использовать среду выполнения интеграции Azure или локальную среду выполнения интеграции. Если не указано другое, по умолчанию используется интегрированная среда выполнения Azure. | Нет       |

## <a name="azure-machine-learning-linked-service"></a>Связанная служба Машинного обучения Azure
Создайте связанную службу Машинного обучения Azure, чтобы зарегистрировать конечную точку пакетной оценки показателей машинного обучения оценки в фабрике данных.

### <a name="example"></a>Пример

```json
{
    "name": "AzureMLLinkedService",
    "properties": {
      "type": "AzureML",
      "typeProperties": {
        "mlEndpoint": "https://[batch scoring endpoint]/jobs",
        "apiKey": {
            "type": "SecureString",
            "value": "access key"
        }
     },
     "connectVia": {
        "referenceName": "<name of Integration Runtime>",
        "type": "IntegrationRuntimeReference"
      }
    }
}
```

### <a name="properties"></a>properties
| Свойство               | ОПИСАНИЕ                              | Обязательно для заполнения                                 |
| ---------------------- | ---------------------------------------- | ---------------------------------------- |
| type                   | Свойству type необходимо присвоить значение **AzureML**. | Да                                      |
| mlEndpoint             | URL-адрес пакетной оценки.                   | Да                                      |
| apiKey                 | API модели опубликованной рабочей области.     | Да                                      |
| updateResourceEndpoint | URL-адрес обновления ресурса для конечной точки веб-службы "Машинное обучение Azure", используемый для обновления прогнозной веб-службы с помощью файла обученной модели. | Нет                                       |
| servicePrincipalId     | Укажите идентификатора клиента приложения.     | Требуется, если задано свойство updateResourceEndpoint |
| servicePrincipalKey    | Укажите ключ приложения.           | Требуется, если задано свойство updateResourceEndpoint |
| tenant                 | Укажите сведения о клиенте (доменное имя или идентификатор клиента), в котором находится приложение. Эти сведения можно получить, наведя указатель мыши на правый верхний угол страницы портала Azure. | Требуется, если задано свойство updateResourceEndpoint |
| connectVia             | Среда выполнения интеграции, используемая для отправки действий в связанную службу. Вы можете использовать среду выполнения интеграции Azure или локальную среду выполнения интеграции. Если не указано другое, по умолчанию используется интегрированная среда выполнения Azure. | Нет                                       |

## <a name="azure-data-lake-analytics-linked-service"></a>Связанная служба Azure Data Lake Analytics
Можно создать связанную службу **Azure Data Lake Analytics** , чтобы связать службу вычислений Azure Data Lake Analytics с фабрикой данных Azure. Действие U-SQL Data Lake Analytics в конвейере ссылается на эту связанную службу. 

### <a name="example"></a>Пример

```json
{
    "name": "AzureDataLakeAnalyticsLinkedService",
    "properties": {
        "type": "AzureDataLakeAnalytics",
        "typeProperties": {
            "accountName": "adftestaccount",
            "dataLakeAnalyticsUri": "azuredatalakeanalytics URI",
            "servicePrincipalId": "service principal id",
            "servicePrincipalKey": {
                "value": "service principal key",
                "type": "SecureString"
            },
            "tenant": "tenant ID",
            "subscriptionId": "<optional, subscription id of ADLA>",
            "resourceGroupName": "<optional, resource group name of ADLA>"
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

### <a name="properties"></a>properties

| Свойство             | ОПИСАНИЕ                              | Обязательно для заполнения                                 |
| -------------------- | ---------------------------------------- | ---------------------------------------- |
| type                 | Свойству type необходимо присвоить значение **AzureDataLakeAnalytics**. | Да                                      |
| accountName          | Имя учетной записи аналитики озера данных Azure.  | Да                                      |
| dataLakeAnalyticsUri | Универсальный код ресурса (URI) аналитики озера данных Azure.           | Нет                                       |
| subscriptionId       | Идентификатор подписки Azure                    | Нет                                       |
| resourceGroupName    | Имя группы ресурсов Azure                | Нет                                       |
| servicePrincipalId   | Укажите идентификатора клиента приложения.     | Да                                      |
| servicePrincipalKey  | Укажите ключ приложения.           | Да                                      |
| клиент               | Укажите сведения о клиенте (доменное имя или идентификатор клиента), в котором находится приложение. Эти сведения можно получить, наведя указатель мыши на правый верхний угол страницы портала Azure. | Да                                      |
| connectVia           | Среда выполнения интеграции, используемая для отправки действий в связанную службу. Вы можете использовать среду выполнения интеграции Azure или локальную среду выполнения интеграции. Если не указано другое, по умолчанию используется интегрированная среда выполнения Azure. | Нет                                       |



## <a name="azure-databricks-linked-service"></a>Связанная служба Azure Databricks
Вы можете создать **связанную службу Azure Databricks**, чтобы зарегистрировать рабочую область Databricks, которая будет использоваться для выполнения рабочих нагрузок (записных книжек) Databricks.

### <a name="example---using-new-job-cluster-in-databricks"></a>Пример. Использование нового кластера заданий в Databricks

```json
{
    "name": "AzureDatabricks_LS",
    "properties": {
        "type": "AzureDatabricks",
        "typeProperties": {
            "domain": "https://eastus.azuredatabricks.net",
            "newClusterNodeType": "Standard_D3_v2",
            "newClusterNumOfWorker": "1:10",
            "newClusterVersion": "4.0.x-scala2.11",
            "accessToken": {
                "type": "SecureString",
                "value": "dapif33c9c721144c3a790b35000b57f7124f"
            }
        }
    }
}

```

### <a name="example---using-existing-interactive-cluster-in-databricks"></a>Пример. Использование существующего интерактивного кластера в Databricks

```json
{
    "name": " AzureDataBricksLinedService",
    "properties": {
      "type": " AzureDatabricks",
      "typeProperties": {
        "domain": "https://westeurope.azuredatabricks.net",
        "accessToken": {
            "type": "SecureString", 
            "value": "dapif33c9c72344c3a790b35000b57f7124f"
          },
        "existingClusterId": "{clusterId}"
        }
}

```

### <a name="properties"></a>properties

| Свойство             | ОПИСАНИЕ                              | Обязательно для заполнения                                 |
| -------------------- | ---------------------------------------- | ---------------------------------------- |
| name                 | Имя связанной службы               | Да   |
| type                 | Свойству type необходимо присвоить значение **AzureDatabricks**. | Да                                      |
| домен               | Укажите регион Azure на основе региона рабочей области Databricks. Пример: https://eastus.azuredatabricks.net | Да                                 |
| accessToken          | Чтобы фабрика данных прошла аутентификацию в Azure Databricks, необходим маркер доступа. Маркер доступа должен быть создан в рабочей области Databricks. Подробные инструкции по поиску маркера доступа см. в [этой статье](https://docs.azuredatabricks.net/api/latest/authentication.html#generate-token).  | Да                                       |
| existingClusterId    | Идентификатор существующего кластера, где будут выполняться все задания. Это должен быть уже созданный интерактивный кластер. Если кластер перестанет отвечать, вам может понадобиться перезапустить его вручную. Для улучшения надежности Databricks предлагает выполнять задания на новых кластерах. Идентификатор интерактивного кластера можно найти, выбрав "Рабочая область Databricks -> Кластеры -> Имя интерактивного кластера -> Конфигурация -> Теги". [Дополнительные сведения](https://docs.databricks.com/user-guide/clusters/tags.html) | Нет 
| newClusterVersion    | Версия Spark кластера. Она создаст кластер заданий в Databricks. | Нет  |
| newClusterNumOfWorker| Необходимое число рабочих узлов текущего кластера. В кластере присутствует один драйвер Spark и исполнители num_workers для такого числа узлов: Spark num_workers + 1. Строка в формате Int32, например "1", означает, что параметр numOfWorker имеет значение 1, а "1:10" — автомасштабирование от 1 (минимум) до 10 (максимум).  | Нет                |
| newClusterNodeType   | Используя отдельное значение, это поле кодирует доступные ресурсы для каждого узла Spark в этом кластере. Например, узлы Spark могут быть подготовлены и оптимизированы для операций в памяти или для ресурсоемких рабочих нагрузок. Это поле также обязательно для нового кластера.                | Нет               |
| newClusterSparkConf  | Набор необязательных, определяемых пользователем пар "ключ — значение" в конфигурации Spark. Пользователи также могут передавать строку дополнительных параметров JVM драйверу и исполнителям через spark.driver.extraJavaOptions и spark.executor.extraJavaOptions соответственно. | Нет  |
| newClusterInitScripts| набор сценариев инициализации необязательное, определяемое пользователем для нового кластера. Указав путь к DBFS в сценарии инициализации. | Нет  |


## <a name="azure-sql-database-linked-service"></a>Связанная служба "База данных SQL Azure"
Связанная служба SQL Azure создается и применяется к [действию хранимой процедуры](transform-data-using-stored-procedure.md) для вызова хранимой процедуры из конвейера фабрики данных. Дополнительную информацию см. в статье о [связанной службе SQL Azure](connector-azure-sql-database.md#linked-service-properties).

## <a name="azure-sql-data-warehouse-linked-service"></a>Связанная служба хранилища данных SQL Azure
Связанная служба хранилища данных SQL Azure создается и применяется к [действию хранимой процедуры](transform-data-using-stored-procedure.md) для вызова хранимой процедуры из конвейера фабрики данных. Дополнительные сведения об этой связанной службе см. в соответствующем разделе статьи [Перемещение данных в хранилище данных Azure SQL и из него с помощью фабрики данных Azure](connector-azure-sql-data-warehouse.md#linked-service-properties).

## <a name="sql-server-linked-service"></a>Связанная служба SQL Server
Связанная служба SQL Server создается и применяется к [действию хранимой процедуры](transform-data-using-stored-procedure.md) для вызова хранимой процедуры из конвейера фабрики данных. Дополнительные сведения о связанной службе SQL Server см. в соответствующем разделе статьи [Перемещение данных в базу данных SQL Server и обратно на локальных компьютерах и виртуальных машинах Azure IaaS с помощью фабрики данных Azure](connector-sql-server.md#linked-service-properties).

## <a name="next-steps"></a>Дальнейшие действия
Список действий преобразования данных, поддерживаемых фабрикой данных Azure, см. [в этой статье](transform-data.md).
