---
title: Копирование данных из Amazon Simple Storage Service (S3), с помощью фабрики данных Azure | Документация Майкрософт
description: Узнайте, как копировать данные из Amazon Simple Storage Service (S3) в поддерживаемые хранилища данных приемника с помощью фабрики данных Azure.
services: data-factory
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 04/29/2019
ms.author: jingwang
ms.openlocfilehash: 088dfdbfbadfa43dc2bd161f56f0e2a6dbb94bb7
ms.sourcegitcommit: 5cb0b6645bd5dff9c1a4324793df3fdd776225e4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/21/2019
ms.locfileid: "67311998"
---
# <a name="copy-data-from-amazon-simple-storage-service-using-azure-data-factory"></a>Копирование данных из Amazon Simple Storage Service с помощью фабрики данных Azure
> [!div class="op_single_selector" title1="Выберите версию службы фабрики данных, которую вы используете:"]
>
> * [Версия 1](v1/data-factory-amazon-simple-storage-service-connector.md)
> * [Текущая версия](connector-amazon-simple-storage-service.md)

В этой статье описывается, как копировать данные из Amazon Simple Storage Service (Amazon S3). Дополнительные сведения о Фабрике данных Azure см. во [вводной статье](introduction.md).

## <a name="supported-capabilities"></a>Поддерживаемые возможности

Этот соединитель Amazon S3 поддерживается для следующих действий:

- [Действие копирования](copy-activity-overview.md) с [поддерживается матрицы источника и приемника](copy-activity-overview.md)
- [Действие поиска](control-flow-lookup-activity.md)
- [Действие получения метаданных в Фабрике данных Azure](control-flow-get-metadata-activity.md)

В частности, этот соединитель Amazon S3 поддерживает копирование файлов "как есть" или анализ файлов с использованием [поддерживаемых форматов файлов и кодеков сжатия](supported-file-formats-and-compression-codecs.md). Она использует [AWS подписи версии 4](https://docs.aws.amazon.com/general/latest/gr/signature-version-4.html) для проверки подлинности запросов к службе S3.

>[!TIP]
>Этот соединитель Amazon S3 можно использовать для копирования данных из **всех поставщиков хранилища, совместимых с S3**, например [Google Cloud Storage](connector-google-cloud-storage.md). Укажите соответствующий URL-адрес службы в конфигурации связанной службы.

## <a name="required-permissions"></a>Необходимые разрешения

Чтобы скопировать данные из Amazon S3, убедитесь, что вы предоставили следующие разрешения:

- **Для выполнения операции копирования** — `s3:GetObject` и `s3:GetObjectVersion` для операций с объектами Amazon S3.
- **Для создания графического пользовательского интерфейса Фабрики данных** — разрешения `s3:ListAllMyBuckets` и `s3:ListBucket`/`s3:GetBucketLocation` также требуются для выполнения в контейнере Amazon S3 операций, таких как проверка соединения, просмотр путей к файлам и переход по ним. Если вы не хотите предоставлять эти разрешения, пропустите проверку соединения на странице создания связанной службы и укажите путь непосредственно в параметрах набора данных.

Полный список разрешений Amazon S3 см. в статье, посвященной [назначению разрешений в политике](https://docs.aws.amazon.com/AmazonS3/latest/dev/using-with-s3-actions.html).

## <a name="getting-started"></a>Приступая к работе

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)] 

Следующие разделы содержат сведения о свойствах, которые используются для определения сущностей фабрики данных, характерных для Amazon S3.

## <a name="linked-service-properties"></a>Свойства связанной службы

Для связанной службы Amazon S3 поддерживаются следующие свойства:

| Свойство | ОПИСАНИЕ | Обязательно для заполнения |
|:--- |:--- |:--- |
| type | Для свойства типа необходимо задать значение **AmazonS3**. | Yes |
| accessKeyId | Идентификатор секретного ключа доступа. |Yes |
| secretAccessKey | Сам секретный ключ доступа. Пометьте это поле как SecureString, чтобы безопасно хранить его в фабрике данных, или [добавьте ссылку на секрет, хранящийся в Azure Key Vault](store-credentials-in-key-vault.md). |Yes |
| serviceUrl | Укажите настраиваемую конечную точку S3, если вы копируете данные из поставщика хранилища, совместимого с S3, но отличного от официальной службы Amazon S3. Например, для копирования данных из Google Cloud Storage укажите `https://storage.googleapis.com`. | Нет |
| connectVia | [Среда выполнения интеграции](concepts-integration-runtime.md), используемая для подключения к хранилищу данных. Вы можете использовать среду выполнения интеграции Azure или локальную среду IR (если хранилище данных расположено в частной сети). Если не указано другое, по умолчанию используется интегрированная среда выполнения Azure. |Нет |

>[!TIP]
>Укажите настраиваемый URL-адрес службы S3, если вы копируете данные из хранилища, совместимого с S3, но отличного от официальной службы Amazon S3.

>[!NOTE]
>Для этого соединителя требуются ключи доступа к учетной записи IAM для копирования данных из Amazon S3. [Временные учетные данные безопасности](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_temp.html) не поддерживаются.
>

Вот пример:

```json
{
    "name": "AmazonS3LinkedService",
    "properties": {
        "type": "AmazonS3",
        "typeProperties": {
            "accessKeyId": "<access key id>",
            "secretAccessKey": {
                "type": "SecureString",
                "value": "<secret access key>"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

## <a name="dataset-properties"></a>Свойства набора данных

Полный список разделов и свойств, доступных для определения наборов данных, см. в статье о [наборах данных](concepts-datasets-linked-services.md). 

- Для **Parquet и текстовый формат с разделителями**, см. [Parquet и текстовый файл с разделителями, формат набора данных](#parquet-and-delimited-text-format-dataset) раздел.
- Для других форматов, например **формата ORC и Avro/JSON/двоичных файлов**, см. [другой набор данных в формате](#other-format-dataset) раздел.

### <a name="parquet-and-delimited-text-format-dataset"></a>Parquet и набор данных формат с разделителями

Чтобы скопировать данные из Amazon S3 в **Parquet или текстовый формат с разделителями**, см. [формат Parquet](format-parquet.md) и [текстовый формат с разделителями](format-delimited-text.md) статья на основе формат набора данных и поддерживается Параметры. Следующие свойства поддерживаются для Amazon S3 в разделе `location` параметры в наборе данных на основе формата:

| Свойство   | ОПИСАНИЕ                                                  | Обязательно для заполнения |
| ---------- | ------------------------------------------------------------ | -------- |
| type       | Свойство типа в списке `location` в наборе данных должно быть присвоено **AmazonS3Location**. | Yes      |
| bucketName | Имя контейнера S3.                                          | Yes      |
| folderPath | Путь к папке данного контейнера. Если вы хотите использовать подстановочный знак в папку фильтра, пропустить этот параметр и укажите в параметрах исходного действия. | Нет       |
| fileName   | Имя заданного контейнера и folderPath. Если вы хотите использовать подстановочный знак для фильтрации файлов, пропустите этот параметр и укажите в параметрах исходного действия. | Нет       |
| version | Версия объекта S3, если включено управление версиями S3. Если не указан, будут выбираться последняя версия. |Нет |

> [!NOTE]
> **AmazonS3Object** типа набора данных в формате Parquet или текст, описанные в следующем разделе по-прежнему поддерживается в качестве-для действия копирования/подстановки/GetMetadata для обеспечения обратной совместимости, но он не работает с сопоставление потока данных. Рекомендуется использовать эту новую модель, в дальнейшем и разработки пользовательского интерфейса ADF изменился на создание этих новых типов.

**Пример.**

```json
{
    "name": "DelimitedTextDataset",
    "properties": {
        "type": "DelimitedText",
        "linkedServiceName": {
            "referenceName": "<Amazon S3 linked service name>",
            "type": "LinkedServiceReference"
        },
        "schema": [ < physical schema, optional, auto retrieved during authoring > ],
        "typeProperties": {
            "location": {
                "type": "AmazonS3Location",
                "bucketName": "bucketname",
                "folderPath": "folder/subfolder"
            },
            "columnDelimiter": ",",
            "quoteChar": "\"",
            "firstRowAsHeader": true,
            "compressionCodec": "gzip"
        }
    }
}
```

### <a name="other-format-dataset"></a>Другой набор данных в формате

Чтобы скопировать данные из Amazon S3 в **формата ORC и Avro/JSON/двоичных файлов**, поддерживаются следующие свойства:

| Свойство | ОПИСАНИЕ | Обязательно для заполнения |
|:--- |:--- |:--- |
| type | Для свойства type набора данных необходимо задать следующее значение: **AmazonS3Object** |Yes |
| bucketName | Имя контейнера S3. Фильтр подстановочных знаков не поддерживается. |Yes для действия Copy/Lookup, No для действия GetMetadata |
| key | **Имя или фильтр подстановочных знаков** ключа объекта S3 в указанном контейнере. Применяется, только если свойство prefix не указано. <br/><br/>Фильтр с подстановочными знаками поддерживается для пути к папке и имени файла. Допустимые знаки подстановки: `*` (соответствует нулю или нескольким символам) и `?` (соответствует нулю или одному символу).<br/>Пример 1. `"key": "rootfolder/subfolder/*.csv"`<br/>Пример 2. `"key": "rootfolder/subfolder/???20180427.txt"`<br/>Дополнительные примеры приведены в разделе [Примеры фильтров папок и файлов](#folder-and-file-filter-examples). Используйте `^` для экранирования знаков, если фактическое имя файла или папки содержит подстановочный знак или этот escape-символ. |Нет |
| prefix | Префикс для ключа объекта S3. Выбираются объекты, ключи которых начинаются с этого префикса. Применяется, только если свойство key не указано. |Нет |
| version | Версия объекта S3, если включено управление версиями S3. Если не указан, будут выбираться последняя версия. |Нет |
| modifiedDatetimeStart | Фильтр файлов на основе атрибута: последнее изменение. Файлы будут выбраны, если время их последнего изменения находится в диапазоне времени `modifiedDatetimeStart` и `modifiedDatetimeEnd`. Время представлено часовым поясом UTC в формате "2018-12-01T05:00:00Z". <br/><br/> Имейте в виду, что при включении этого параметра, если вы хотите зарегистрировать фильтр из огромные объемы файлы повлияет общую производительность перемещения данных. <br/><br/> Свойства могут иметь значение NULL. Это означает, что фильтры атрибута файла не будут применяться к набору данных.  Если для параметра `modifiedDatetimeStart` задано значение даты и времени, но параметр `modifiedDatetimeEnd` имеет значение NULL, то будут выбраны файлы, чей атрибут последнего изменения больше указанного значения даты и времени или равен ему.  Если для параметра `modifiedDatetimeEnd` задано значение даты и времени, но параметр `modifiedDatetimeStart` имеет значение NULL, то будут выбраны все файлы, чей атрибут последнего изменения меньше указанного значения даты и времени.| Нет |
| modifiedDatetimeEnd | Фильтр файлов на основе атрибута: последнее изменение. Файлы будут выбраны, если время их последнего изменения находится в диапазоне времени `modifiedDatetimeStart` и `modifiedDatetimeEnd`. Время представлено часовым поясом UTC в формате "2018-12-01T05:00:00Z". <br/><br/> Имейте в виду, что при включении этого параметра, если вы хотите зарегистрировать фильтр из огромные объемы файлы повлияет общую производительность перемещения данных. <br/><br/> Свойства могут иметь значение NULL. Это означает, что фильтры атрибута файла не будут применяться к набору данных.  Если для параметра `modifiedDatetimeStart` задано значение даты и времени, но параметр `modifiedDatetimeEnd` имеет значение NULL, то будут выбраны файлы, чей атрибут последнего изменения больше указанного значения даты и времени или равен ему.  Если для параметра `modifiedDatetimeEnd` задано значение даты и времени, но параметр `modifiedDatetimeStart` имеет значение NULL, то будут выбраны все файлы, чей атрибут последнего изменения меньше указанного значения даты и времени.| Нет |
| format | Если требуется скопировать файлы между файловыми хранилищами **как есть** (двоичное копирование), можно пропустить раздел форматирования в определениях входного и выходного наборов данных.<br/><br/>Если необходимо проанализировать или создать файлы определенного формата, поддерживаются следующие форматы файлов: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**. Свойству **type** в разделе format необходимо присвоить одно из этих значений. Дополнительные сведения см. в разделах о [текстовом формате](supported-file-formats-and-compression-codecs.md#text-format), [формате Json](supported-file-formats-and-compression-codecs.md#json-format), [формате Avro](supported-file-formats-and-compression-codecs.md#avro-format), [формате Orc](supported-file-formats-and-compression-codecs.md#orc-format) и [ формате Parquet](supported-file-formats-and-compression-codecs.md#parquet-format). |Нет (только для сценария двоичного копирования) |
| compression | Укажите тип и уровень сжатия данных. Дополнительные сведения см. в разделе [Поддержка сжатия](supported-file-formats-and-compression-codecs.md#compression-support).<br/>Поддерживаемые типы: **GZip**, **Deflate**, **BZip2** и **ZipDeflate**.<br/>Поддерживаемые уровни: **Оптимальный** и **Самый быстрый**. |Нет |

>[!TIP]
>Чтобы скопировать все файлы в папке, укажите **bucketName** для контейнеров и **prefix** для части папки.<br>Чтобы скопировать один файл с заданным именем, укажите **bucketName** для контейнеров и **key** для части папки вместе с именем файла.<br>Чтобы скопировать подмножество файлов в папке, укажите **bucketName** для контейнеров и **key** для части папки и фильтра подстановочных знаков.

**Пример. Использование префикса**

```json
{
    "name": "AmazonS3Dataset",
    "properties": {
        "type": "AmazonS3Object",
        "linkedServiceName": {
            "referenceName": "<Amazon S3 linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "bucketName": "testbucket",
            "prefix": "testFolder/test",
            "modifiedDatetimeStart": "2018-12-01T05:00:00Z",
            "modifiedDatetimeEnd": "2018-12-01T06:00:00Z",
            "format": {
                "type": "TextFormat",
                "columnDelimiter": ",",
                "rowDelimiter": "\n"
            },
            "compression": {
                "type": "GZip",
                "level": "Optimal"
            }
        }
    }
}
```

**Пример. Использование ключа и версии (необязательно)**

```json
{
    "name": "AmazonS3Dataset",
    "properties": {
        "type": "AmazonS3",
        "linkedServiceName": {
            "referenceName": "<Amazon S3 linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "bucketName": "testbucket",
            "key": "testFolder/testfile.csv.gz",
            "version": "XXXXXXXXXczm0CJajYkHf0_k6LhBmkcL",
            "format": {
                "type": "TextFormat",
                "columnDelimiter": ",",
                "rowDelimiter": "\n"
            },
            "compression": {
                "type": "GZip",
                "level": "Optimal"
            }
        }
    }
}
```

## <a name="copy-activity-properties"></a>Свойства действия копирования

Полный список разделов и свойств, используемых для определения действий, см. в статье [Конвейеры и действия в фабрике данных Azure](concepts-pipelines-activities.md). Этот раздел содержит список свойств, поддерживаемых источником Amazon S3.

### <a name="amazon-s3-as-source"></a>Amazon S3 в качестве источника

- Для копирования из **Parquet и текстовый формат с разделителями**, см. [Parquet и исходный формат с разделителями](#parquet-and-delimited-text-format-source) раздел.
- Для копирования из других форматов, например **формата ORC и Avro/JSON/двоичных файлов**, см. [другого формата источника](#other-format-source) раздел.

#### <a name="parquet-and-delimited-text-format-source"></a>Parquet и исходный формат с разделителями

Чтобы скопировать данные из Amazon S3 в **Parquet или текстовый формат с разделителями**, см. [формат Parquet](format-parquet.md) и [текстовый формат с разделителями](format-delimited-text.md) статьи на источника действия копирования на основе формата и Поддерживаемые параметры. Следующие свойства поддерживаются для Amazon S3 в разделе `storeSettings` параметры источника копирования на основе формата:

| Свойство                 | ОПИСАНИЕ                                                  | Обязательно для заполнения                                                    |
| ------------------------ | ------------------------------------------------------------ | ----------------------------------------------------------- |
| type                     | Свойство типа в списке `storeSettings` должно быть присвоено **AmazonS3ReadSetting**. | Yes                                                         |
| recursive                | Указывает, следует ли читать данные рекурсивно из вложенных папок или только из указанной папки. Обратите внимание, что если для свойства recursive задано значение true, а приемником является файловое хранилище, пустые папки и вложенные папки не создаются в приемнике. Допустимые значения: **true** (по умолчанию) и **false**. | Нет                                                          |
| prefix                   | Префикс для ключа объекта S3 в разделе данного сегмента, настроены в наборе данных для фильтрации источника объектов. Выбираются объекты, ключи которых начинаются с этого префикса. <br>Применяется, только если `wildcardFolderPath` и `wildcardFileName` свойства не заданы. | Нет                                                          |
| wildcardFolderPath       | Путь к папке с использованием подстановочных знаков в разделе данного сегмента, настроены в наборе данных для папки источника фильтра. <br>Допустимые подстановочные знаки: `*` (соответствует нулю или большему количеству знаков) и `?` (соответствует нулю или одному знаку). Для экранирования используйте `^`, если фактическое имя папки содержит подстановочный знак или escape-символ. <br>Дополнительные примеры приведены в разделе [Примеры фильтров папок и файлов](#folder-and-file-filter-examples). | Нет                                                          |
| wildcardFileName         | Имя файла с использованием подстановочных знаков в категории заданного контейнера и folderPath/wildcardFolderPath к исходным файлам фильтра. <br>Допустимые подстановочные знаки: `*` (соответствует нулю или большему количеству знаков) и `?` (соответствует нулю или одному знаку). Для экранирования используйте `^`, если фактическое имя папки содержит подстановочный знак или escape-символ.  Дополнительные примеры приведены в разделе [Примеры фильтров папок и файлов](#folder-and-file-filter-examples). | Да, в том случае, если `fileName` в наборе данных и `prefix` не указаны |
| modifiedDatetimeStart    | Фильтр файлов на основе атрибута: последнее изменение. Файлы будут выбраны, если время их последнего изменения находится в диапазоне времени `modifiedDatetimeStart` и `modifiedDatetimeEnd`. Время представлено часовым поясом UTC в формате "2018-12-01T05:00:00Z". <br> Свойства могут иметь значение NULL. Это означает, что фильтры атрибута файла не будут применяться к набору данных.  Если для параметра `modifiedDatetimeStart` задано значение даты и времени, но параметр `modifiedDatetimeEnd` имеет значение NULL, то будут выбраны файлы, чей атрибут последнего изменения больше указанного значения даты и времени или равен ему.  Если для параметра `modifiedDatetimeEnd` задано значение даты и времени, но параметр `modifiedDatetimeStart` имеет значение NULL, то будут выбраны все файлы, чей атрибут последнего изменения меньше указанного значения даты и времени. | Нет                                                          |
| modifiedDatetimeEnd      | То же, что и выше.                                               | Нет                                                          |
| maxConcurrentConnections | Число подключений для подключения к хранилищу хранилища, одновременно. Укажите только в том случае, если вы хотите ограничить максимальное количество одновременных подключений к хранилищу данных. | Нет                                                          |

> [!NOTE]
> Для Parquet/текстового формата с разделителями **FileSystemSource** источника действия копирования типа, описанные в следующем разделе по-прежнему поддерживается в качестве-— для обеспечения обратной совместимости. Рекомендуется использовать эту новую модель, в дальнейшем и разработки пользовательского интерфейса ADF изменился на создание этих новых типов.

**Пример.**

```json
"activities":[
    {
        "name": "CopyFromAmazonS3",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Delimited text input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "DelimitedTextSource",
                "formatSettings":{
                    "type": "DelimitedTextReadSetting",
                    "skipLineCount": 10
                },
                "storeSettings":{
                    "type": "AmazonS3ReadSetting",
                    "recursive": true,
                    "wildcardFolderPath": "myfolder*A",
                    "wildcardFileName": "*.csv"
                }
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

#### <a name="other-format-source"></a>Другой источник формат

Чтобы скопировать данные из Amazon S3 в **формата ORC и Avro/JSON/двоичных файлов**, в действии копирования поддерживаются следующие свойства **источника** раздел:

| Свойство | ОПИСАНИЕ | Обязательно для заполнения |
|:--- |:--- |:--- |
| type | Свойство type источника действия копирования должно иметь следующее значение: **FileSystemSource**. |Yes |
| recursive | Указывает, следует ли читать данные рекурсивно из вложенных папок или только из указанной папки. Обратите внимание, что если для свойства recursive задано значение true, а приемником является файловое хранилище, в приемнике не будут создаваться пустые папки и вложенные папки.<br/>Допустимые значения: **true** (по умолчанию), **false**. | Нет |
| maxConcurrentConnections | Число подключений для подключения к хранилищу данных, одновременно. Укажите только в том случае, если вы хотите ограничить максимальное количество одновременных подключений к хранилищу данных. | Нет |

**Пример.**

```json
"activities":[
    {
        "name": "CopyFromAmazonS3",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Amazon S3 input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "FileSystemSource",
                "recursive": true
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

### <a name="folder-and-file-filter-examples"></a>Примеры фильтров папок и файлов

В этом разделе описываются результаты применения фильтров с подстановочными знаками к пути папки и имени файла.

| bucket | key | recursive | Структура исходной папки и результат фильтрации (извлекаются файлы, выделенные полужирным шрифтом)|
|:--- |:--- |:--- |:--- |
| bucket | `Folder*/*` | false | bucket<br/>&nbsp;&nbsp;&nbsp;&nbsp;ПапкаA<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**Файл1.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**Файл2.json**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Вложенная_папка1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Файл3.csv<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Файл4.json<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Файл5.csv<br/>&nbsp;&nbsp;&nbsp;&nbsp;Другая_папкаB<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Файл6.csv |
| bucket | `Folder*/*` | true | bucket<br/>&nbsp;&nbsp;&nbsp;&nbsp;ПапкаA<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**Файл1.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**Файл2.json**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Вложенная_папка1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**Файл3.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**Файл4.json**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**Файл5.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;Другая_папкаB<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Файл6.csv |
| bucket | `Folder*/*.csv` | false | bucket<br/>&nbsp;&nbsp;&nbsp;&nbsp;ПапкаA<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**Файл1.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Файл2.json<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Вложенная_папка1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Файл3.csv<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Файл4.json<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Файл5.csv<br/>&nbsp;&nbsp;&nbsp;&nbsp;Другая_папкаB<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Файл6.csv |
| bucket | `Folder*/*.csv` | true | bucket<br/>&nbsp;&nbsp;&nbsp;&nbsp;ПапкаA<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**Файл1.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Файл2.json<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Вложенная_папка1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**Файл3.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Файл4.json<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**Файл5.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;Другая_папкаB<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Файл6.csv |

## <a name="next-steps"></a>Дальнейшие действия
В статье [Действие копирования в Фабрике данных Azure](copy-activity-overview.md##supported-data-stores-and-formats) приведен список хранилищ данных, которые поддерживаются в качестве источников и приемников для действия копирования в Фабрике данных Azure.
