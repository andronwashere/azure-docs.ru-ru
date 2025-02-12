---
title: Поддерживаемые форматы файлов в фабрике данных Azure | Документация Майкрософт
description: В этой статье описаны форматы файлов и коды сжатия, поддерживаемые соединителями на основе файлов в фабрике данных Azure.
author: linda33wj
manager: craigg
ms.reviewer: craigg
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 05/22/2019
ms.author: jingwang
ms.openlocfilehash: 9f6edc45316eaeceb75da643ed64b39382712852
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "66165940"
---
# <a name="supported-file-formats-and-compression-codecs-in-azure-data-factory"></a>Поддерживаемые форматы файлов и кодеки сжатия в фабрике данных Azure

*Эта статья применима к следующим соединителям: [Amazon S3](connector-amazon-simple-storage-service.md), [BLOB-объекты Azure](connector-azure-blob-storage.md), [Azure Data Lake Storage 1-го поколения](connector-azure-data-lake-store.md), [Azure Data Lake Storage 2-го поколения](connector-azure-data-lake-storage.md), [хранилище файлов Azure](connector-azure-file-storage.md), [файловая система](connector-file-system.md), [FTP](connector-ftp.md), [Google Cloud Storage;](connector-google-cloud-storage.md), [HDFS](connector-hdfs.md), [HTTP](connector-http.md) и [SFTP](connector-sftp.md).*

Если требуется скопировать файлы между файловыми хранилищами **как есть** (двоичное копирование), можно пропустить раздел форматирования в определениях входного и выходного наборов данных. Вам может понадобиться **анализировать или создавать файлы, имеющие определенный формат**. Фабрика данных Azure поддерживает следующие форматы файлов:

* [текстовый формат](#text-format);
* [формат JSON](#json-format);
* [формат Parquet](#parquet-format).
* [формат ORC](#orc-format);
* [формат Avro](#avro-format);

> [!TIP]
> Узнайте, как действие копирования сопоставляет данные источника в приемник из [сопоставление схем в действии копирования](copy-activity-schema-and-type-mapping.md).

## <a name="text-format"></a>Текстовый формат

>[!NOTE]
>Появились новые фабрики данных с разделителями набором текстовый формат данных, см. в разделе [текстовый формат с разделителями](format-delimited-text.md) статье с подробными сведениями. Следующие конфигурации на основе набора данных хранилища данных на основе файла по-прежнему поддерживается в качестве-предназначен для обратной compabitility. Рекомендуется использовать новую модель, в дальнейшем.

Если вам нужно считать данные из текстового файла или записать в него данные, задайте для свойства `type` в разделе `format` набора данных значение **TextFormat**. В разделе `format` также можно указать следующие **необязательные** свойства. Инструкции по настройке см. в разделе [Пример TextFormat](#textformat-example).

| Свойство | ОПИСАНИЕ | Допустимые значения | Обязательно для заполнения |
| --- | --- | --- | --- |
| columnDelimiter |Знак, используемый для разделения столбцов в файле. Вы можете использовать редкие непечатаемые символы, которые не содержатся в ваших данных. Например, укажите "\u0001", что соответствует символу начала заголовка (SOH). |Допускается только один знак. Значение **по умолчанию** — **запятая (,)** . <br/><br/>Чтобы использовать символ Юникода, см. соответствующие коды в статье о [символах Юникода](https://en.wikipedia.org/wiki/List_of_Unicode_characters). |Нет |
| rowDelimiter |Знак, используемый для разделения строк в файле. |Допускается только один знак. **По умолчанию** используется одно из следующих значений: **для чтения — [\r\n, \r, \n]** , для записи — **\r\n**. |Нет |
| escapeChar |Специальный знак, используемый для экранирования разделителя столбцов в содержимом входного файла. <br/><br/>Для таблицы нельзя указать и escapeChar, и quoteChar. |Допускается только один знак. Значение по умолчанию отсутствует. <br/><br/>Например, если в качестве разделителя столбцов используется запятая (,), но этот знак встречается и в тексте (пример: Hello, world), то в качестве escape-символа можно определить знак доллара ($) и использовать в исходном тексте строку Hello$, world. |Нет |
| quoteChar |Знак, используемый в качестве кавычки для заключения строкового значения. Разделители столбцов и строк внутри знаков кавычек будут рассматриваться как часть строкового значения. Это свойство применимо к входным и выходным наборам данных.<br/><br/>Для таблицы нельзя указать и escapeChar, и quoteChar. |Допускается только один знак. Значение по умолчанию отсутствует. <br/><br/>Например, если в качестве разделителя столбцов используется запятая (,) и нужно, чтобы этот знак встречался в тексте (например, <Hello, world>), то можно в качестве знака кавычек определить двойную кавычку (") и использовать в исходном тексте строку "Hello, world". |Нет |
| nullValue |Один или несколько знаков, используемых для представления значения NULL. |Один или несколько знаков. Значения **по умолчанию**: **\N и NULL** для чтения и **\N** для записи. |Нет |
| encodingName |Имя кодировки. |Допустимое имя кодировки. Ознакомьтесь с описанием свойства [Encoding.EncodingName](https://msdn.microsoft.com/library/system.text.encoding.aspx). Пример: windows-1250 или shift_jis. **По умолчанию** используется **UTF-8**. |Нет |
| firstRowAsHeader |Указывает, следует ли рассматривать первую строки в качестве заголовка. Фабрика данных считывает первую строку входного набора данных как заголовок. Фабрика данных записывает первую строку как заголовок в выходной набор данных. <br/><br/>Примеры сценариев см. в разделе [Сценарии использования `firstRowAsHeader` и `skipLineCount`](#scenarios-for-using-firstrowasheader-and-skiplinecount). |Истина<br/><b>False (по умолчанию)</b> |Нет |
| skipLineCount |Указывает количество **непустых** строк, которые нужно пропустить при чтении данных из входных файлов. Если указаны skipLineCount и firstRowAsHeader, то сначала пропускаются строки, после чего из входного файла считываются данные заголовка. <br/><br/>Примеры сценариев см. в разделе [Сценарии использования `firstRowAsHeader` и `skipLineCount`](#scenarios-for-using-firstrowasheader-and-skiplinecount). |Integer |Нет |
| treatEmptyAsNull |Указывает, следует ли интерпретировать строки со значением NULL или пустые строки как значение NULL при чтении данных из входного файла. |**True (по умолчанию)**<br/>False |Нет |

### <a name="textformat-example"></a>Пример TextFormat

В следующем определении JSON для набора данных задаются некоторые необязательные свойства.

```json
"typeProperties":
{
    "folderPath": "mycontainer/myfolder",
    "fileName": "myblobname",
    "format":
    {
        "type": "TextFormat",
        "columnDelimiter": ",",
        "rowDelimiter": ";",
        "quoteChar": "\"",
        "NullValue": "NaN",
        "firstRowAsHeader": true,
        "skipLineCount": 0,
        "treatEmptyAsNull": true
    }
},
```

Чтобы использовать `escapeChar` вместо `quoteChar`, замените строку с `quoteChar` следующим escape-символом:

```json
"escapeChar": "$",
```

### <a name="scenarios-for-using-firstrowasheader-and-skiplinecount"></a>Сценарии использования firstRowAsHeader и skipLineCount

* Вы выполняете копирование из нефайлового источника в текстовый файл и хотите добавить строку заголовка, содержащую метаданные схемы (например, схемы SQL). В этом случае укажите `firstRowAsHeader` со значением true в выходном наборе данных.
* Вы копируете данные из текстового файла, содержащего строку заголовка, в нефайловый приемник и хотите удалить эту строку. Укажите `firstRowAsHeader` со значением true во входном наборе данных.
* Вы копируете данные из текстового файла и хотите пропустить несколько строк в начале, которые не содержат ни данных, ни заголовка. Укажите `skipLineCount`, чтобы задать число пропускаемых строк. Если остальная часть файла содержит строку заголовка, можно также указать `firstRowAsHeader`. Если указаны `skipLineCount` и `firstRowAsHeader`, сначала пропускаются строки, а затем из входного файла считываются данные заголовка.

## <a name="json-format"></a>Формат JSON

Чтобы **импортировать JSON-файл "как есть" в базу данных Azure Cosmos DB или экспортировать его из нее**, см. раздел "Документы JSON для импорта и экспорта" статьи о [перемещении данных в базу данных Azure Cosmos DB и из нее](connector-azure-cosmos-db.md).

Если требуется проанализировать JSON-файлы или записать данные в формате JSON, задайте для свойства `type` в разделе `format` значение **JsonFormat**. В разделе `format` также можно указать следующие **необязательные** свойства. Инструкции по настройке см. в разделе [Пример JsonFormat](#jsonformat-example).

| Свойство | ОПИСАНИЕ | Обязательно для заполнения |
| --- | --- | --- |
| filePattern |Шаблон данных, хранящихся в каждом JSON-файле. Допустимые значения: **setOfObjects** и **arrayOfObjects**. Значение **по умолчанию** — **setOfObjects**. Подробные сведения об этих шаблонах см. в разделе [Шаблоны файлов JSON](#json-file-patterns). |Нет |
| jsonNodeReference | Для итерации и извлечения данных из объектов в поле массива с таким же шаблоном укажите путь JSON этого массива. Это свойство поддерживается только в том случае, если данные копируются **из** JSON-файлов. | Нет |
| jsonPathDefinition | Выражение пути JSON для каждого столбца с его сопоставлением с настраиваемым именем столбца (начало в нижнем регистре). Это свойство поддерживается только в том случае, если данные копируются **из** JSON-файлов и данные можно извлечь из объекта или массива. <br/><br/> Для полей в области корневого объекта выражение пути должно начинаться с корня $. Для полей внутри массива, выбранных с помощью свойства `jsonNodeReference`, выражение должно начинаться с элемента массива. Инструкции по настройке см. в разделе [Пример JsonFormat](#jsonformat-example). | Нет |
| encodingName |Имя кодировки. Список допустимых имен кодировок приведен в описании свойства [Encoding.EncodingName](https://msdn.microsoft.com/library/system.text.encoding.aspx). Например: windows-1250 или shift_jis. По **умолчанию** используется **UTF-8**. |Нет |
| nestingSeparator |Символ, используемый для разделения уровней вложенности. Значение по умолчанию — точка (.). |Нет |

>[!NOTE]
>В случае кросс применить данные в массиве на несколько строк (вариант 1 "->" Пример 2 в [примеры JsonFormat](#jsonformat-example)), можно выбрать только развернуть один массив, с помощью свойства `jsonNodeReference`.

### <a name="json-file-patterns"></a>Шаблоны файлов JSON

Действие копирования может проанализировать следующие шаблоны JSON-файлов.

- **Тип 1: setOfObjects**

    Каждый файл содержит один объект или несколько разделенных строками или объединенных объектов. Если этот параметр выбран в выходном наборе данных, то в результате копирования будет создан JSON-файл, где каждый объект будет находиться в отдельной строке (файл с разделителем-строкой).

    * **Пример единого объекта JSON**

        ```json
        {
            "time": "2015-04-29T07:12:20.9100000Z",
            "callingimsi": "466920403025604",
            "callingnum1": "678948008",
            "callingnum2": "567834760",
            "switch1": "China",
            "switch2": "Germany"
        }
        ```

    * **Пример JSON-файла с разделителем-строкой**

        ```json
        {"time":"2015-04-29T07:12:20.9100000Z","callingimsi":"466920403025604","callingnum1":"678948008","callingnum2":"567834760","switch1":"China","switch2":"Germany"}
        {"time":"2015-04-29T07:13:21.0220000Z","callingimsi":"466922202613463","callingnum1":"123436380","callingnum2":"789037573","switch1":"US","switch2":"UK"}
        {"time":"2015-04-29T07:13:21.4370000Z","callingimsi":"466923101048691","callingnum1":"678901578","callingnum2":"345626404","switch1":"Germany","switch2":"UK"}
        ```

    * **Пример объединенного JSON-файла**

        ```json
        {
            "time": "2015-04-29T07:12:20.9100000Z",
            "callingimsi": "466920403025604",
            "callingnum1": "678948008",
            "callingnum2": "567834760",
            "switch1": "China",
            "switch2": "Germany"
        }
        {
            "time": "2015-04-29T07:13:21.0220000Z",
            "callingimsi": "466922202613463",
            "callingnum1": "123436380",
            "callingnum2": "789037573",
            "switch1": "US",
            "switch2": "UK"
        }
        {
            "time": "2015-04-29T07:13:21.4370000Z",
            "callingimsi": "466923101048691",
            "callingnum1": "678901578",
            "callingnum2": "345626404",
            "switch1": "Germany",
            "switch2": "UK"
        }
        ```

- **Тип 2: arrayOfObjects**

    Каждый файл содержит массив объектов.

    ```json
    [
        {
            "time": "2015-04-29T07:12:20.9100000Z",
            "callingimsi": "466920403025604",
            "callingnum1": "678948008",
            "callingnum2": "567834760",
            "switch1": "China",
            "switch2": "Germany"
        },
        {
            "time": "2015-04-29T07:13:21.0220000Z",
            "callingimsi": "466922202613463",
            "callingnum1": "123436380",
            "callingnum2": "789037573",
            "switch1": "US",
            "switch2": "UK"
        },
        {
            "time": "2015-04-29T07:13:21.4370000Z",
            "callingimsi": "466923101048691",
            "callingnum1": "678901578",
            "callingnum2": "345626404",
            "switch1": "Germany",
            "switch2": "UK"
        }
    ]
    ```

### <a name="jsonformat-example"></a>Пример JsonFormat

**Вариант 1. Копирование данных из JSON-файлов**

**Пример 1. Извлечение данных из объекта и массива**

В этом примере предполагается, что один корневой объект JSON соответствует одной записи в таблице результатов. Если у вас есть JSON-файл со следующим содержимым:

```json
{
    "id": "ed0e4960-d9c5-11e6-85dc-d7996816aad3",
    "context": {
        "device": {
            "type": "PC"
        },
        "custom": {
            "dimensions": [
                {
                    "TargetResourceType": "Microsoft.Compute/virtualMachines"
                },
                {
                    "ResourceManagementProcessRunId": "827f8aaa-ab72-437c-ba48-d8917a7336a3"
                },
                {
                    "OccurrenceTime": "1/13/2017 11:24:37 AM"
                }
            ]
        }
    }
}
```

и вы хотите скопировать это содержимое (посредством извлечения данных из объекта и массива) в таблицу SQL Azure в следующем формате:

| ИД | deviceType | targetResourceType | resourceManagementProcessRunId | occurrenceTime |
| --- | --- | --- | --- | --- |
| ed0e4960-d9c5-11e6-85dc-d7996816aad3 | PC | Microsoft.Compute/virtualMachines | 827f8aaa-ab72-437c-ba48-d8917a7336a3 | 1/13/2017 11:24:37 AM |

Входной набор данных с типом **JsonFormat** определяется следующим образом (частичное определение только соответствующих частей). В частности:

- Раздел `structure` определяет настраиваемые имена столбцов и соответствующие типы данных при преобразовании в табличные данные. Этот раздел является **необязательным**, если вам не нужно сопоставлять столбцы. Дополнительные сведения см. в статье о [сопоставлении столбцов исходного набора данных со столбцами целевого набора данных](copy-activity-schema-and-type-mapping.md).
- `jsonPathDefinition` указывает путь к файлу JSON для каждого столбца, который определяет, откуда следует извлекать данные. Чтобы скопировать данные из массива, с помощью `array[x].property` можно извлечь значение нужного свойства из объекта `xth` или с помощью `array[*].property` найти нужное значение в любом объекте с таким свойством.

```json
"properties": {
    "structure": [
        {
            "name": "id",
            "type": "String"
        },
        {
            "name": "deviceType",
            "type": "String"
        },
        {
            "name": "targetResourceType",
            "type": "String"
        },
        {
            "name": "resourceManagementProcessRunId",
            "type": "String"
        },
        {
            "name": "occurrenceTime",
            "type": "DateTime"
        }
    ],
    "typeProperties": {
        "folderPath": "mycontainer/myfolder",
        "format": {
            "type": "JsonFormat",
            "filePattern": "setOfObjects",
            "jsonPathDefinition": {"id": "$.id", "deviceType": "$.context.device.type", "targetResourceType": "$.context.custom.dimensions[0].TargetResourceType", "resourceManagementProcessRunId": "$.context.custom.dimensions[1].ResourceManagementProcessRunId", "occurrenceTime": " $.context.custom.dimensions[2].OccurrenceTime"}
        }
    }
}
```

**Пример 2. Применение нескольких объектов с одинаковым шаблоном из массива**

В этом примере предполагается, что один корневой объект JSON будет преобразован в несколько записей в таблице результатов. Если у вас есть JSON-файл со следующим содержимым:

```json
{
    "ordernumber": "01",
    "orderdate": "20170122",
    "orderlines": [
        {
            "prod": "p1",
            "price": 23
        },
        {
            "prod": "p2",
            "price": 13
        },
        {
            "prod": "p3",
            "price": 231
        }
    ],
    "city": [ { "sanmateo": "No 1" } ]
}
```

И если вы хотите скопировать этот файл в таблицу Azure SQL в следующем формате путем сведения данных внутри массива и перекрестного соединения с общими сведениями о корневом объекте:

| `ordernumber` | `orderdate` | `order_pd` | `order_price` | `city` |
| --- | --- | --- | --- | --- |
| 01 | 20170122 | P1 | 23 | `[{"sanmateo":"No 1"}]` |
| 01 | 20170122 | P2 | 13 | `[{"sanmateo":"No 1"}]` |
| 01 | 20170122 | P3 | 231 | `[{"sanmateo":"No 1"}]` |


Входной набор данных с типом **JsonFormat** определяется следующим образом (частичное определение только соответствующих частей). В частности:

- Раздел `structure` определяет настраиваемые имена столбцов и соответствующие типы данных при преобразовании в табличные данные. Этот раздел является **необязательным**, если вам не нужно сопоставлять столбцы. Дополнительные сведения см. в статье о [сопоставлении столбцов исходного набора данных со столбцами целевого набора данных](copy-activity-schema-and-type-mapping.md).
- Параметр `jsonNodeReference` обозначает итерацию и извлечение данных из объектов с одинаковым шаблоном в разделе **массива** `orderlines`.
- `jsonPathDefinition` указывает путь к файлу JSON для каждого столбца, который определяет, откуда следует извлекать данные. В этом примере `ordernumber`, `orderdate` и `city` расположены в корневом объекте. Путь JSON к нему начинается с `$.`, а `order_pd` и `order_price` определяются с помощью пути, производного от элемента массива без `$.`.

```json
"properties": {
    "structure": [
        {
            "name": "ordernumber",
            "type": "String"
        },
        {
            "name": "orderdate",
            "type": "String"
        },
        {
            "name": "order_pd",
            "type": "String"
        },
        {
            "name": "order_price",
            "type": "Int64"
        },
        {
            "name": "city",
            "type": "String"
        }
    ],
    "typeProperties": {
        "folderPath": "mycontainer/myfolder",
        "format": {
            "type": "JsonFormat",
            "filePattern": "setOfObjects",
            "jsonNodeReference": "$.orderlines",
            "jsonPathDefinition": {"ordernumber": "$.ordernumber", "orderdate": "$.orderdate", "order_pd": "prod", "order_price": "price", "city": " $.city"}
        }
    }
}
```

**Обратите внимание на следующие моменты.**

* Если параметры `structure` и `jsonPathDefinition` не определены в наборе данных фабрики данных, действие копирования обнаружит схему из первого объекта и выполнит сведение всего объекта.
* Если входной JSON-файл содержит массив, по умолчанию действие копирования преобразует все значение массива в строку. Вы можете извлечь данные из строки с помощью `jsonNodeReference` или `jsonPathDefinition`. Или можно пропустить строку, не указывая ее в `jsonPathDefinition`.
* Если на том же уровне существует повторяющиеся имена, то действие копирования выберет последнее из них.
* В именах свойств учитывается регистр. Два свойства с одинаковым именем, но в разных регистрах, рассматриваются как два отдельных свойства.

**Вариант 2. Запись данных в JSON-файл**

Если в базе данных SQL есть следующая таблица:

| ИД | order_date | order_price | order_by |
| --- | --- | --- | --- |
| 1 | 20170119 | 2000 | David |
| 2 | 20170120 | 3500 | Patrick |
| 3 | 20170121 | 4000 | Jason |

и для каждой записи вы предполагаете запись в объект JSON в следующем формате:

```json
{
    "id": "1",
    "order": {
        "date": "20170119",
        "price": 2000,
        "customer": "David"
    }
}
```

Выходной набор данных с типом **JsonFormat** определяется следующим образом (частичное определение только соответствующих частей). В частности, раздел `structure` определяет настраиваемые имена свойств в конечном файле. Для определения уровня вложенности от имен будет использоваться разделитель вложенности `nestingSeparator` (по умолчанию — точка (.)). Этот раздел является **необязательным**, если вы не собираетесь изменять исходное имя свойства или вкладывать свойства.

```json
"properties": {
    "structure": [
        {
            "name": "id",
            "type": "String"
        },
        {
            "name": "order.date",
            "type": "String"
        },
        {
            "name": "order.price",
            "type": "Int64"
        },
        {
            "name": "order.customer",
            "type": "String"
        }
    ],
    "typeProperties": {
        "folderPath": "mycontainer/myfolder",
        "format": {
            "type": "JsonFormat"
        }
    }
}
```

## <a name="parquet-format"></a>Формат Parquet

>[!NOTE]
>Фабрика данных появился новый формат набора данных Parquet, см. в разделе [формат Parquet](format-parquet.md) статье с подробными сведениями. Следующие конфигурации на основе набора данных хранилища данных на основе файла по-прежнему поддерживается в качестве-предназначен для обратной compabitility. Рекомендуется использовать новую модель, в дальнейшем.

Если требуется проанализировать файлы Parquet или записать данные в формате Parquet, установите для свойства `format` `type` значение **ParquetFormat**. Вам не нужно указывать какие-либо свойства в подразделе Format раздела typeProperties. Пример:

```json
"format":
{
    "type": "ParquetFormat"
}
```

Обратите внимание на следующие моменты.

* Данные сложных типов (MAP, LIST) не поддерживаются.
* Пробелы в именах столбцов не поддерживаются.
* Parquet-файл имеет следующие варианты сжатия: NONE, SNAPPY, GZIP и LZO. Фабрика данных поддерживает чтение данных из файла Parquet в любом из указанных форматов сжатия за исключением LZO — для чтения данных используется кодек сжатия, указанный в метаданных. Однако при записи в Parquet-файл фабрика данных по умолчанию выбирает SNAPPY. В настоящее время изменить это поведение нельзя.

> [!IMPORTANT]
> Для копирования посредством локальной среды выполнения интеграции (IR), то есть между локальным и облачным хранилищами данных, если вы не копируете файлы Parquet **как есть**, на компьютере среды выполнения интеграции необходимо установить **64-разрядную JRE 8 (среду выполнения Java) или OpenJDK**. Подробные сведения приведены в следующем абзаце.

Для операций копирования, выполняемых в локальной среде IR с сериализацией или десериализацией файлов Parquet, файл определения приложения определяет среду выполнения Java, сначала проверив реестр *`(SOFTWARE\JavaSoft\Java Runtime Environment\{Current Version}\JavaHome)`* для поиска JRE, а затем (если не ничего найдено) — системную переменную *`JAVA_HOME`* для поиска OpenJDK.

- **Для использования JRE**. Для 64-разрядной версии среды выполнения интеграции требуется 64-разрядная версия JRE. Ее можно найти [здесь](https://go.microsoft.com/fwlink/?LinkId=808605).
- **Для использования OpenJDK**: он поддерживается в среде выполнения интеграции, начиная с версии 3.13. Упакуйте jvm.dll со всеми другими необходимыми сборками OpenJDK на компьютере с локальной IR и соответственно установите системную переменную среды JAVA_HOME.

>[!TIP]
>Если вы копируете данные в формат Parquet или из формата Parquet с помощью локальной среди выполнения интеграции и возникает ошибка: "Ошибка при вызове Java, сообщение: **java.lang.OutOfMemoryError:Java heap space**", можно добавить переменную среды `_JAVA_OPTIONS` в компьютере, на котором размещена локальная среда выполнения интеграции для настройки минимального и максимального размера кучи для виртуальной машины Java, чтобы расширить возможности такой копии, а затем повторно запустить конвейер.

![Установка размера кучи виртуальной машины Java на локальной среде выполнения интеграции](./media/supported-file-formats-and-compression-codecs/set-jvm-heap-size-on-selfhosted-ir.png)

Пример: установите переменную `_JAVA_OPTIONS` со значением `-Xms256m -Xmx16g`. Флаг `Xms` указывает начальный пул выделения памяти для виртуальной машины Java (JVM), а `Xmx` указывает максимальный пул выделения памяти. Это означает, что JVM будет запущена с объемом памяти `Xms` и сможет использовать не более `Xmx` объема памяти. По умолчанию ADF использует минимум 64 МБ и максимум 1 ГБ.

### <a name="data-type-mapping-for-parquet-files"></a>Сопоставление типов данных для файлов Parquet

| Тип промежуточных данных фабрики данных | Тип-примитив Parquet | Исходный тип Parquet (десериализация) | Исходный тип Parquet (сериализация) |
|:--- |:--- |:--- |:--- |
| Boolean | Boolean | Н/Д | Н/Д |
| SByte | Int32 | Int8 | Int8 |
| Byte | Int32 | UInt8 | Int16 |
| Int16 | Int32 | Int16 | Int16 |
| UInt16 | Int32 | UInt16 | Int32 |
| Int32 | Int32 | Int32 | Int32 |
| UInt32 | Int64 | UInt32 | Int64 |
| Int64 | Int64 | Int64 | Int64 |
| UInt64 | Binary или Int64 | UInt64 | Decimal |
| Single | Float | Н/Д | Н/Д |
| Double | Double | Н/Д | Н/Д |
| Decimal | Binary | Decimal | Decimal |
| String | Binary | Utf8 | Utf8 |
| DateTime | Int96 | Н/Д | Н/Д |
| TimeSpan | Int96 | Н/Д | Н/Д |
| DateTimeOffset | Int96 | Н/Д | Н/Д |
| ByteArray | Binary | Н/Д | Н/Д |
| Guid | Binary | Utf8 | Utf8 |
| Char | Binary | Utf8 | Utf8 |
| CharArray | Не поддерживается | Н/Д | Н/Д |

## <a name="orc-format"></a>Формат ORC

Если требуется проанализировать ORC-файлы или записать данные в формате ORC, установите для свойства `format` `type` значение **OrcFormat**. Вам не нужно указывать какие-либо свойства в подразделе Format раздела typeProperties. Пример:

```json
"format":
{
    "type": "OrcFormat"
}
```

Обратите внимание на следующие моменты.

* Данные сложных типов (STRUCT, MAP, LIST, UNION) не поддерживаются.
* Пробелы в именах столбцов не поддерживаются.
* Для ORC-файлов используется три [параметра сжатия](https://hortonworks.com/blog/orcfile-in-hdp-2-better-compression-better-performance/): NONE, ZLIB и SNAPPY. Фабрика данных поддерживает чтение данных из ORC-файла в любом из этих форматов. Для чтения данных используется кодек сжатия из метаданных. Однако при записи в ORC-файл фабрика данных по умолчанию выбирает ZLIB. В настоящее время изменить это поведение нельзя.

> [!IMPORTANT]
> Для копирования посредством локальной среды выполнения интеграции (IR), то есть между локальным и облачным хранилищами данных, если вы не копируете файлы ORC **как есть**, на компьютере среды выполнения интеграции необходимо установить **64-разрядную JRE 8 (среда выполнения Java) или OpenJDK**. Подробные сведения приведены в следующем абзаце.

Для операций копирования, выполняемых в локальной среде IR с сериализацией или десериализацией файлов ORC, файл определения приложения определяет среду выполнения Java, сначала проверив реестр *`(SOFTWARE\JavaSoft\Java Runtime Environment\{Current Version}\JavaHome)`* для поиска JRE, а затем (если не ничего найдено) — системную переменную *`JAVA_HOME`* для поиска OpenJDK.

- **Для использования JRE**. Для 64-разрядной версии среды выполнения интеграции требуется 64-разрядная версия JRE. Ее можно найти [здесь](https://go.microsoft.com/fwlink/?LinkId=808605).
- **Для использования OpenJDK**: он поддерживается в среде выполнения интеграции, начиная с версии 3.13. Упакуйте jvm.dll со всеми другими необходимыми сборками OpenJDK на компьютере с локальной IR и соответственно установите системную переменную среды JAVA_HOME.

### <a name="data-type-mapping-for-orc-files"></a>Сопоставление типов данных для ORC-файлов

| Тип промежуточных данных фабрики данных | Типы ORC |
|:--- |:--- |
| Boolean | Boolean |
| SByte | Byte |
| Byte | Сокращение |
| Int16 | Сокращение |
| UInt16 | Int |
| Int32 | Int |
| UInt32 | длинное целое |
| Int64 | длинное целое |
| UInt64 | String |
| Single | Float |
| Double | Double |
| Decimal | Decimal |
| String | String |
| DateTime | Timestamp |
| DateTimeOffset | Timestamp |
| TimeSpan | Timestamp |
| ByteArray | Binary |
| Guid | String |
| Char | Char(1) |

## <a name="avro-format"></a>Формат Avro

Если требуется проанализировать файлы Avro или записать данные в формате Avro, установите для свойства `format` `type` значение **AvroFormat**. Вам не нужно указывать какие-либо свойства в подразделе Format раздела typeProperties. Пример:

```json
"format":
{
    "type": "AvroFormat",
}
```

Сведения об использовании формата Avro в таблице Hive см. в [руководстве по Apache Hive](https://cwiki.apache.org/confluence/display/Hive/AvroSerDe).

Обратите внимание на следующие моменты.

* [Сложные типы данных](https://avro.apache.org/docs/current/spec.html#schema_complex) (записи, перечисления, массивы, сопоставления, объединения и фиксированные данные) не поддерживаются.

## <a name="compression-support"></a>Поддержка сжатия

Фабрика данных Azure поддерживает сжатие и распаковку данных во время копирования. Если вы указываете свойство `compression` во входном наборе данных, действие копирования читает сжатые файлы из источника и распаковывает их. При указании этого свойства в выходном наборе данных действие копирования сжимает, а затем записывает данные в приемник. Ниже приведено несколько примеров сценариев:

* Считайте сжатые с помощью кодека GZIP данные из BLOB-объекта Azure, распакуйте их и запишите результирующие данные в базу данных SQL Azure. Вы определяете входной набор данных BLOB-объекта Azure с помощью свойства `compression` `type` как GZIP.
* Считайте данные из обычного текстового файла в локальной файловой системе, сожмите их в формате GZip и запишите сжатые данные в BLOB-объект Azure. Вы определяете выходной набор данных BLOB-объекта Azure с помощью свойства `compression` `type` как GZIP.
* Считайте ZIP-файл с FTP-сервера, распакуйте его, чтобы получить содержащиеся в нем файлы, и отправьте их в хранилище Azure Data Lake Store. Вы определяете входной набор данных FTP с помощью свойства `compression` `type` как ZipDeflate.
* Считайте сжатые с помощью кодека GZIP данные из BLOB-объекта Azure, распакуйте их и сожмите с помощью BZIP2, а затем запишите результирующие данные в BLOB-объект Azure. Вы определяете входной набор данных BLOB-объекта Azure, установив для `compression` `type` значение GZIP, и выходной набор данных, установив для `compression` `type` значение BZIP2.

Чтобы указать сжатие для набора данных, используйте свойство **compression** в наборе данных JSON, как показано в следующем примере.

```json
{
    "name": "AzureBlobDataSet",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": {
            "referenceName": "StorageLinkedService",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "fileName": "pagecounts.csv.gz",
            "folderPath": "compression/file/",
            "format": {
                "type": "TextFormat"
            },
            "compression": {
                "type": "GZip",
                "level": "Optimal"
            }
        }
    }
}
```

Раздел **compression** содержит два свойства:

* **Type** — кодек сжатия. Возможные значения: **GZIP**, **Deflate**, **BZIP2** или **ZipDeflate**.
* **Level** — коэффициент сжатия; возможные значения: **Optimal** и **Fastest**.

  * **Fastest**: операция сжатия должна выполняться как можно быстрее, даже если итоговый файл сжимается не оптимально.
  * **Optimal**: операция сжатия должна выполняться оптимально, даже если для ее завершения требуется больше времени.

    Дополнительные сведения см. в разделе [Уровень сжатия](https://msdn.microsoft.com/library/system.io.compression.compressionlevel.aspx).

> [!NOTE]
> Параметры сжатия для данных в форматах **AvroFormat**, **OrcFormat** или **ParquetFormat** не поддерживаются. Для чтения данных в этих форматах фабрика данных выявляет и использует в метаданных кодек сжатия. При записи в файл в одном из этих форматов фабрика данных выбирает кодек сжатия по умолчанию для этого формата. Например, ZLIB для OrcFormat и SNAPPY для ParquetFormat.

## <a name="unsupported-file-types-and-compression-formats"></a>Неподдерживаемые типы файлов и форматов сжатия

Функции расширения фабрики данных Azure можно использовать для преобразования файлов, которые не поддерживаются.
Два варианта включения функций Azure и пользовательские задачи с помощью пакетной службы Azure.

Вы увидите пример, использующий функцию Azure для [Извлеките содержимое tar-файл](https://github.com/Azure/Azure-DataFactory/tree/master/SamplesV2/UntarAzureFilesWithAzureFunction). Дополнительные сведения см. в разделе [действия "функции Azure"](https://docs.microsoft.com/azure/data-factory/control-flow-azure-function-activity).

Можно также создать эту функцию, с помощью действия пользовательского dotnet. Дополнительные сведения недоступны [здесь](https://docs.microsoft.com/azure/data-factory/transform-data-using-dotnet-custom-activity)

## <a name="next-steps"></a>Дальнейшие действия

Ниже приведены статьи для файловых хранилищ данных, поддерживаемых фабрикой данных Azure.

- [Соединитель хранилища BLOB-объектов Azure](connector-azure-blob-storage.md)
- [Соединитель Azure Data Lake Store](connector-azure-data-lake-store.md)
- [Соединитель Amazon S3](connector-amazon-simple-storage-service.md)
- [Соединитель файловой системы](connector-file-system.md)
- [Соединитель FTP](connector-ftp.md)
- [Соединитель SFTP](connector-sftp.md)
- [Соединитель HDFS](connector-hdfs.md)
- [Соединитель HTTP](connector-http.md)
