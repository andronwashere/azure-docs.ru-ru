---
title: Отправка данных в индекс поиска с использованием фабрики данных | Документация Майкрософт
description: Узнайте о том, как передавать данные в индекс Поиска Azure с использованием фабрики данных Azure.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.assetid: f8d46e1e-5c37-4408-80fb-c54be532a4ab
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/22/2018
ms.author: jingwang
robots: noindex
ms.openlocfilehash: 30a5bc9c5f0b7d1443e7ca2a16d9f0e0d1120dd8
ms.sourcegitcommit: 64798b4f722623ea2bb53b374fb95e8d2b679318
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/11/2019
ms.locfileid: "67836627"
---
# <a name="push-data-to-an-azure-search-index-by-using-azure-data-factory"></a>Передача данных в индекс Поиска Azure с использованием фабрики данных Azure
> [!div class="op_single_selector" title1="Выберите используемую версию службы "Фабрика данных":"]
> * [Версия 1](data-factory-azure-search-connector.md)
> * [Версия 2 (текущая)](../connector-azure-search.md)

> [!NOTE]
> В этой статье рассматривается служба "Фабрика данных Azure" версии 1. Если вы используете текущую версию Фабрики данных, см. статью о [соединителе "Поиск Azure" в службе "Фабрика данных Azure" версии 2](../connector-azure-search.md).

В этой статье описывается, как использовать действие копирования для передачи данных из поддерживаемого исходного хранилища данных в индекс Поиска Azure. Поддерживаемые исходные хранилища данных перечислены в столбце "Источник" таблицы [поддерживаемых источников и приемников](data-factory-data-movement-activities.md#supported-data-stores-and-formats). Эта статья основана на статье о [действиях перемещения данных](data-factory-data-movement-activities.md) , в которой приведены общие сведения о перемещении данных с помощью действия копирования и поддерживаемых сочетаниях хранилищ данных.

## <a name="enabling-connectivity"></a>Включение соединения
Чтобы разрешить подключение службы фабрики данных к локальному хранилищу данных, установите в локальной среде шлюз управления данными. Его можно установить на том же компьютере, на котором размещено исходное хранилище данных, или на отдельном компьютере во избежание конкуренции за ресурсы с хранилищем данных.

Шлюз управления данными обеспечивает безопасное и управляемое подключение локальных источников данных к облачным службам. Сведения о шлюзе управления данными см. в статье [Перемещение данных между локальными источниками и облаком](data-factory-move-data-between-onprem-and-cloud.md).

## <a name="getting-started"></a>Начало работы
Вы можете создать конвейер с действием копирования, который перемещает данные из исходного хранилища данных в индекс Поиска Azure, с помощью различных инструментов и интерфейсов API.

Проще всего создать конвейер с помощью **мастера копирования**. Пошаговые инструкции см. в [руководстве Создание конвейера с действием копирования с помощью мастера копирования фабрики данных](data-factory-copy-data-wizard-tutorial.md), где приведено краткое пошаговое руководство по созданию конвейера с помощью мастера копирования данных.

Для создания конвейера можно использовать указанные ниже средства. **Visual Studio**, **Azure PowerShell**, **шаблона Azure Resource Manager**, **.NET API**, и **REST API**. Пошаговые инструкции по созданию конвейера с действием копирования см. в [руководстве по действию копирования](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).

Независимо от используемого средства или API-интерфейса, для создания конвейера, который перемещает данные из источника данных в приемник, выполняются следующие шаги:

1. Создайте **связанные службы**, чтобы связать входные и выходные данные с фабрикой данных.
2. Создайте **наборы данных**, которые представляют входные и выходные данные для операции копирования.
3. Создайте **конвейер** с действием копирования, который принимает входной набор данных и возвращает выходной набор данных.

Если вы используете мастер, то он автоматически создает определения JSON для сущностей фабрики данных (связанных служб, наборов данных и конвейера). При использовании инструментов и интерфейсов API (за исключением API .NET) вы самостоятельно определяете эти сущности фабрики данных в формате JSON.  Пример определений JSON для сущностей Фабрики данных, которые используются для копирования данных в индекс Поиска Azure, доступен в разделе [Пример JSON. Копирование данных с локального SQL Server в индекс поиска Azure](#json-example-copy-data-from-on-premises-sql-server-to-azure-search-index) далее в этой статье.

Следующие разделы содержат сведения о свойствах JSON, которые используются для определения сущностей фабрики данных, относящихся к индексу Поиска Azure.

## <a name="linked-service-properties"></a>Свойства связанной службы

В таблице ниже приведены описания элементов JSON, которые относятся к связанной службе Поиска Azure.

| Свойство | Описание | Обязательно для заполнения |
| -------- | ----------- | -------- |
| type | Свойству type необходимо задать значение **AzureSearch**. | Да |
| url | URL-адрес службы Поиска Azure. | Да |
| ключ | Ключ администратора службы Поиска Azure. | Да |

## <a name="dataset-properties"></a>Свойства набора данных

Полный список разделов и свойств, используемых для определения наборов данных, см. в статье [Наборы данных в фабрике данных Azure](data-factory-create-datasets.md). Разделы структуры, доступности и политики JSON набора данных одинаковы для всех типов наборов данных. Разделы **typeProperties** для каждого типа набора данных отличаются. Раздел typeProperties для набора данных типа **AzureSearchIndex** содержит следующие свойства.

| Свойство | Описание | Обязательно для заполнения |
| -------- | ----------- | -------- |
| type | Для свойства type необходимо задать значение **AzureSearchIndex**| Да |
| indexName | Имя индекса Поиска Azure. Фабрика данных не создает индекс. Индекс должен существовать в Поиске Azure. | Да |


## <a name="copy-activity-properties"></a>Свойства действия копирования
Полный список разделов и свойств, используемых для определения действий, см. в статье [Конвейеры и действия в фабрике данных Azure: создание конвейеров, цепочки действий и расписаний для них](data-factory-create-pipelines.md). Свойства (такие как имя, описание, входные и выходные таблицы, политики и т. д.) доступны для всех типов действий. В свою очередь свойства, доступные в разделе typeProperties, зависят от конкретного типа действия. Для действия копирования они различаются в зависимости от типов источников и приемников.

Для действия копирования, когда приемник относится к типу **AzureSearchIndexSink**, в разделе typeProperties доступны указанные ниже свойства.

| Свойство | Описание | Допустимые значения | Обязательно для заполнения |
| -------- | ----------- | -------------- | -------- |
| WriteBehavior | Указывает действие (объединение или замена), выполняемое, если документ уже существует в индексе. Ознакомьтесь с разделом [Свойство WriteBehavior](#writebehavior-property).| Merge (по умолчанию)<br/>Передать| Нет |
| WriteBatchSize | Передает данные в индекс Поиска Azure, когда размер буфера достигает значения writeBatchSize. Ознакомьтесь с разделом [Свойство WriteBatchSize](#writebatchsize-property). | 1–1000. Значение по умолчанию — 1000. | Нет |

### <a name="writebehavior-property"></a>Свойство WriteBehavior
При записи данных AzureSearchSink выполняет операцию upsert. Другими словами, если при создании документа ключ документа уже существует в индексе Поиска Azure, Поиск Azure обновляет существующий документ, а не создает исключения из-за конфликта.

AzureSearchSink проявляет два типа поведения upsert (с использованием пакета SDK AzureSearch):

- **Объединение**. Все столбцы в новом документе объединяются со столбцами в существующем. Для столбцов с значением null в новом документе значение столбцов в существующем документе сохраняется.
- **Upload** (отправка). Новый документ заменяет существующий. Для столбцов, не указанных в новом документе, задается значение null независимо от того, есть ли в существующем документе столбцы с значением, отличным от null.

Поведение по умолчанию — **объединение**.

### <a name="writebatchsize-property"></a>Свойство WriteBatchSize
Служба Поиска Azure поддерживает запись документов в виде пакета. Пакет может содержать от 1 до 1000 действий. Действие обрабатывает один документ для выполнения операции передачи или объединения.

### <a name="data-type-support"></a>Поддержка типов данных
В следующей таблице представлены сведения о поддержке типов данных Поиска Azure.

| Тип данных Поиска Azure | Поддерживается в приемнике Поиска Azure |
| ---------------------- | ------------------------------ |
| String | Y |
| Int32 | Y |
| Int64 | Y |
| Double | Y |
| Boolean | Y |
| DataTimeOffset | Y |
| String Array | в |
| GeographyPoint | в |

## <a name="json-example-copy-data-from-on-premises-sql-server-to-azure-search-index"></a>Пример JSON. Копирование данных с локального SQL Server в индекс поиска Azure

В примере ниже используется следующее:

1. Связанная служба типа [AzureSqlDW](#linked-service-properties).
2. Связанная служба типа [OnPremisesSqlServer](data-factory-sqlserver-connector.md#linked-service-properties).
3. Входной [набор данных](data-factory-create-datasets.md) типа [SqlServerTable](data-factory-sqlserver-connector.md#dataset-properties).
4. Выходной [набор данных](data-factory-create-datasets.md) типа [AzureSearchIndex](#dataset-properties).
4. [Конвейер](data-factory-create-pipelines.md) с действием копирования, в котором используются [SqlSource](data-factory-sqlserver-connector.md#copy-activity-properties) и [AzureSearchIndexSink](#copy-activity-properties).

В этом примере каждый час копируются данные временных рядов из локальной базы данных SQL Server в индекс Поиска Azure. Используемые в этом примере свойства JSON описаны в разделах, следующих за примерами.

Сначала настройте шлюз управления данными на локальном компьютере. Инструкции приведены в статье [Перемещение данных между локальными источниками и облаком с помощью шлюза управления данными](data-factory-move-data-between-onprem-and-cloud.md) .

**Связанная служба Поиска Azure:**

```JSON
{
    "name": "AzureSearchLinkedService",
    "properties": {
        "type": "AzureSearch",
        "typeProperties": {
            "url": "https://<service>.search.windows.net",
            "key": "<AdminKey>"
        }
    }
}
```

**Связанная служба SQL Server**

```JSON
{
  "Name": "SqlServerLinkedService",
  "properties": {
    "type": "OnPremisesSqlServer",
    "typeProperties": {
      "connectionString": "Data Source=<servername>;Initial Catalog=<databasename>;Integrated Security=False;User ID=<username>;Password=<password>;",
      "gatewayName": "<gatewayname>"
    }
  }
}
```

**Входной набор данных SQL Server**

В примере предполагается, что вы уже создали таблицу MyTable в SQL Server и она содержит столбец с именем timestampcolumn для данных временного ряда. Можно выполнить запрос к нескольким таблицам в одной базе данных, используя один набор данных, но для typeProperty tableName набора данных нужно указать одну таблицу.

Если для параметра external задать значение true, то фабрика данных воспримет этот набор данных как внешний, который создан не в результате какого-либо действия в этой службе.

```JSON
{
  "name": "SqlServerDataset",
  "properties": {
    "type": "SqlServerTable",
    "linkedServiceName": "SqlServerLinkedService",
    "typeProperties": {
      "tableName": "MyTable"
    },
    "external": true,
    "availability": {
      "frequency": "Hour",
      "interval": 1
    },
    "policy": {
      "externalData": {
        "retryInterval": "00:01:00",
        "retryTimeout": "00:10:00",
        "maximumRetry": 3
      }
    }
  }
}
```

**Выходной набор данных Поиска Azure:**

В примере данные копируются в индекс Поиска Azure с именем **products**. Фабрика данных не создает индекс. Чтобы проверить пример, создайте индекс с таким именем. Создайте индекс Поиска Azure с таким же количеством столбцов, что и во входном наборе данных. Новые записи добавляются в индекс Поиска Azure каждый час.

```JSON
{
    "name": "AzureSearchIndexDataset",
    "properties": {
        "type": "AzureSearchIndex",
        "linkedServiceName": "AzureSearchLinkedService",
        "typeProperties" : {
            "indexName": "products",
        },
        "availability": {
            "frequency": "Minute",
            "interval": 15
        }
    }
}
```

**Действие копирования в конвейере со средой SQL в качестве источника и индексом Поиска Azure в качестве приемника**

Конвейер содержит действие копирования, которое использует входной и выходной наборы данных и выполняется каждый час. В определении JSON конвейера для свойства type параметра **source** установлено значение **SqlSource**, а для свойства type параметра **sink** — значение **AzureSearchIndexSink**. SQL-запрос, указанный для свойства **SqlReaderQuery** , выбирает для копирования данные за последний час.

```JSON
{
  "name":"SamplePipeline",
  "properties":{
    "start":"2014-06-01T18:00:00",
    "end":"2014-06-01T19:00:00",
    "description":"pipeline for copy activity",
    "activities":[
      {
        "name": "SqlServertoAzureSearchIndex",
        "description": "copy activity",
        "type": "Copy",
        "inputs": [
          {
            "name": " SqlServerInput"
          }
        ],
        "outputs": [
          {
            "name": "AzureSearchIndexDataset"
          }
        ],
        "typeProperties": {
          "source": {
            "type": "SqlSource",
            "SqlReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd HH:mm}\\' AND timestampcolumn < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
          },
          "sink": {
            "type": "AzureSearchIndexSink"
          }
        },
        "scheduler": {
          "frequency": "Hour",
          "interval": 1
        },
        "policy": {
          "concurrency": 1,
          "executionPriorityOrder": "OldestFirst",
          "retry": 0,
          "timeout": "01:00:00"
        }
      }
    ]
  }
}
```

При копировании данных из облачного хранилища данных в Поиск Azure свойство `executionLocation` является обязательным. Для примера в приведенном ниже фрагменте JSON показано изменение, необходимое в действии копирования `typeProperties`. Дополнительные сведения и поддерживаемые значения см. в разделе [Копирование данных из одного облачного хранилища данных в другое](data-factory-data-movement-activities.md#global).

```JSON
"typeProperties": {
  "source": {
    "type": "BlobSource"
  },
  "sink": {
    "type": "AzureSearchIndexSink"
  },
  "executionLocation": "West US"
}
```


## <a name="copy-from-a-cloud-source"></a>Копирование из облачного источника
При копировании данных из облачного хранилища данных в Поиск Azure свойство `executionLocation` является обязательным. Для примера в приведенном ниже фрагменте JSON показано изменение, необходимое в действии копирования `typeProperties`. Дополнительные сведения и поддерживаемые значения см. в разделе [Копирование данных из одного облачного хранилища данных в другое](data-factory-data-movement-activities.md#global).

```JSON
"typeProperties": {
  "source": {
    "type": "BlobSource"
  },
  "sink": {
    "type": "AzureSearchIndexSink"
  },
  "executionLocation": "West US"
}
```

Также можно сопоставить столбцы из набора данных, используемого в качестве источника, со столбцами из приемника в определении действия копирования. Дополнительные сведения см. в статье [Сопоставление столбцов исходного набора данных со столбцами целевого набора данных](data-factory-map-columns.md).

## <a name="performance-and-tuning"></a>Производительность и настройка
Сведения о ключевых факторах, влияющих на производительность перемещения данных (действие копирования), и различных способах оптимизации этого процесса см. в статье [Руководство по настройке производительности действия копирования](data-factory-copy-activity-performance.md).

## <a name="next-steps"></a>Следующие шаги
Ознакомьтесь со следующими статьями:

* [руководстве по действию копирования](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) .
