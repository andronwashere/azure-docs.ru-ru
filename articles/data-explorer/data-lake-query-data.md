---
title: Запрос данных в Azure Data Lake с помощью обозреватель данных Azure
description: Узнайте, как запрашивать данные в Azure Data Lake с помощью обозреватель данных Azure.
author: orspod
ms.author: orspodek
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: conceptual
ms.date: 07/17/2019
ms.openlocfilehash: cd53e1386d9d6f2a38beb1661554c8cc9116169d
ms.sourcegitcommit: 5604661655840c428045eb837fb8704dca811da0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/25/2019
ms.locfileid: "68494862"
---
# <a name="query-data-in-azure-data-lake-using-azure-data-explorer-preview"></a>Запрос данных в Azure Data Lake с помощью обозреватель данных Azure (Предварительная версия)

Azure Data Lake Storage — это высокомасштабируемое и экономичное решение Data Lake для анализа больших данных. Оно сочетает в себе мощь высокопроизводительной файловой системы массивного масштабирования и экономии, что помогает сократить время анализа данных. Data Lake Storage Gen2 расширяет возможности хранилища BLOB-объектов Azure и является оптимизированным для рабочих нагрузок аналитики.
 
Azure обозреватель данных интегрируется с хранилищем BLOB-объектов Azure и Azure Data Lake Storage 2-го поколения, обеспечивая быстрый, кэшированный и индексированный доступ к данным в Lake. Вы можете анализировать и запрашивать данные в службе Lake без предварительного приема в Azure обозреватель данных. Кроме того, можно выполнять запросы одновременно из полученных и непринятых собственных данных Lake.  

> [!TIP]
> Оптимальная производительность запросов требует приема данных в Azure обозреватель данных. Возможность запрашивать данные в Azure Data Lake Storage 2-го поколения без предварительного приема следует использовать только для исторических данных или данных, которые редко запрашиваются.
 
## <a name="optimize-query-performance-in-the-lake"></a>Оптимизация производительности запросов в Lake 

* Секционирование данных для повышения производительности и оптимизированного времени запроса.
* Сжатие данных для повышения производительности (gzip для наилучшего сжатия, lz4 для лучшей производительности).
* Используйте хранилище BLOB-объектов Azure или Azure Data Lake Storage 2-го поколения с тем же регионом, что и кластер Azure обозреватель данных. 

## <a name="create-an-external-table"></a>Создание внешней таблицы

 > [!NOTE]
 > В настоящее время поддерживаются учетные записи хранения BLOB-объектов Azure или Azure Data Lake Storage 2-го поколения. В настоящее время поддерживаются форматы данных JSON, CSV, TSV и txt.

1. Используйте команду `.create external table` , чтобы создать внешнюю таблицу в обозреватель данных Azure. Дополнительные команды внешней таблицы, такие `.show`как `.drop`, и `.alter` , описаны в [командах внешней таблицы](/azure/kusto/management/externaltables).

    ```Kusto
    .create external table ArchivedProducts(
    Timestamp:datetime,
    ProductId:long, ProductDescription:string) 
    kind=blob
    partition by bin(Timestamp, 1d) 
    dataformat=csv (h@'http://storageaccount.blob.core.windows.net/container1;secretKey') 
    with (compressed = true)  
    ```

    Этот запрос создает ежедневные разделы *container1/гггг/мм/дд/all_exported_blobs. csv*. При более детализированном секционировании ожидается повышение производительности. Например, запросы к внешним таблицам с ежедневными секциями, например приведенными выше, будут иметь лучшую производительность, чем запросы с месячными секционированными таблицами.

1. Внешняя таблица отображается в левой области веб-интерфейса.

    ![Внешняя таблица в веб-ИНТЕРФЕЙСе](media/data-lake-query-data/external-tables-web-ui.png)

### <a name="create-an-external-table-with-json-format"></a>Создание внешней таблицы в формате JSON

Можно создать внешнюю таблицу в формате JSON. Дополнительные сведения см. в разделе [команды внешней таблицы](/azure/kusto/management/externaltables) .

1. Используйте команду, чтобы создать таблицу с именем *екстерналтаблежсон:* `.create external table`

    ```kusto
    .create external table ExternalTableJson (rownumber:int, rowguid:guid) 
    kind=blob
    dataformat=json
    ( 
       h@'http://storageaccount.blob.core.windows.net/container1;secretKey'
    )
    with 
    (
       docstring = "Docs",
       folder = "ExternalTables",
       namePrefix="Prefix"
    ) 
    ```
 
1. Формат JSON требует второго шага создания сопоставления со столбцами, как показано ниже. В следующем запросе создайте определенное сопоставление JSON с именем *mappingName*:

    ```kusto
    .create external table ExternalTableJson json mapping "mappingName" '[{ "column" : "rownumber", "datatype" : "int", "path" : "$.rownumber"},{ "column" : "rowguid", "path" : "$.rowguid" }]' 
    ```

### <a name="external-table-permissions"></a>Разрешения внешней таблицы
 
* Пользователь базы данных может создать внешнюю таблицу. Создатель таблицы автоматически станет администратором таблиц.
* Администратор кластера, базы данных или таблицы может изменять существующую таблицу.
* Любой пользователь или читатель базы данных может запрашивать внешнюю таблицу.
 
## <a name="query-an-external-table"></a>Запрос к внешней таблице
 
Чтобы запросить внешнюю таблицу, используйте `external_table()` функцию и укажите имя таблицы в качестве аргумента функции. Остальная часть запроса — это стандартный язык запросов Kusto.

```Kusto
external_table("ArchivedProducts") | take 100
```

> [!TIP]
> Технология IntelliSense в настоящее время не поддерживается для запросов внешних таблиц.

### <a name="query-an-external-table-with-json-format"></a>Запрос к внешней таблице в формате JSON

Чтобы запросить внешнюю таблицу в формате JSON, используйте `external_table()` функцию и укажите имя таблицы и имя сопоставления в качестве аргументов функции. В приведенном ниже запросе, если *mappingName* не указан, будет использоваться ранее созданное сопоставление.

```kusto
external_table(‘ExternalTableJson’, ‘mappingName’)
```

## <a name="query-external-and-ingested-data-together"></a>Совместный запрос внешних и полученных данных

В одном запросе можно запрашивать как внешние таблицы, так и полученные таблицы данных. Вы [`join`](/azure/kusto/query/joinoperator) [или`union`](/azure/kusto/query/unionoperator) внешняя таблица с дополнительными данными из обозреватель данных Azure, серверов SQL Server или других источников. Используйте, [`let( ) statement`](/azure/kusto/query/letstatement) чтобы присвоить сокращенное имя ссылке на внешнюю таблицу.

В приведенном ниже примере *Products* является принимающей таблицей данных, а *арчиведпродуктс* — внешней таблицей, содержащей данные в Azure Data Lake Storage 2-го поколения:

```kusto
let T1 = external_table("ArchivedProducts") |  where TimeStamp > ago(100d);
let T = Products; //T is an internal table
T1 | join T on ProductId | take 10
```

## <a name="query-taxirides-external-table-in-the-help-cluster"></a>Запрос внешней таблицы *таксиридес* в справочном кластере

Образец набора данных *таксиридес* содержит сведения о городе Нью Йорк из [Нью такси и лимузинов комиссии](https://www1.nyc.gov/site/tlc/about/tlc-trip-record-data.page).

### <a name="create-external-table-taxirides"></a>Создание внешней таблицы *таксиридес* 

> [!NOTE]
> В этом разделе описывается запрос, используемый для создания внешней таблицы *таксиридес* в кластере *справки* . Так как эта таблица уже создана, можно пропустить этот раздел и выполнить [запрос к данным внешней таблицы *таксиридес* ](#query-taxirides-external-table-data). 

1. Следующий запрос использовался для создания внешней таблицы *таксиридес* в кластере справки. 

    ```kusto
    .create external table TaxiRides
    (
    trip_id: long,
    vendor_id: string, 
    pickup_datetime: datetime,
    dropoff_datetime: datetime,
    store_and_fwd_flag: string,
    rate_code_id: int,
    pickup_longitude: real,
    pickup_latitude: real,
    dropoff_longitude: real,
    dropoff_latitude: real,
    passenger_count: int,
    trip_distance: real,
    fare_amount: real,
    extra: real,
    mta_tax: real,
    tip_amount: real,
    tolls_amount: real,
    ehail_fee: real,
    improvement_surcharge: real,
    total_amount: real,
    payment_type: string,
    trip_type: int,
    pickup: string,
    dropoff: string,
    cab_type: string,
    precipitation: int,
    snow_depth: int,
    snowfall: int,
    max_temperature: int,
    min_temperature: int,
    average_wind_speed: int,
    pickup_nyct2010_gid: int,
    pickup_ctlabel: string,
    pickup_borocode: int,
    pickup_boroname: string,
    pickup_ct2010: string,
    pickup_boroct2010: string,
    pickup_cdeligibil: string,
    pickup_ntacode: string,
    pickup_ntaname: string,
    pickup_puma: string,
    dropoff_nyct2010_gid: int,
    dropoff_ctlabel: string,
    dropoff_borocode: int,
    dropoff_boroname: string,
    dropoff_ct2010: string,
    dropoff_boroct2010: string,
    dropoff_cdeligibil: string,
    dropoff_ntacode: string,
    dropoff_ntaname: string,
    dropoff_puma: string
    )
    kind=blob 
    partition by bin(pickup_datetime, 1d)
    dataformat=csv
    ( 
        h@'http://storageaccount.blob.core.windows.net/container1;secretKey''
    )
    ```
1. Результирующая таблица была создана в кластере *справки* :

    ![Внешняя таблица Таксиридес](media/data-lake-query-data/taxirides-external-table.png) 

### <a name="query-taxirides-external-table-data"></a>Запрос данных внешней таблицы *таксиридес* 

Войдите в, чтобы  [выполнитьзапросквнешнейтаблице https://dataexplorer.azure.com/clusters/help/databases/Samples](https://dataexplorer.azure.com/clusters/help/databases/Samples) таксиридес. 

#### <a name="query-taxirides-external-table-without-partitioning"></a>Запрос внешней таблицы *таксиридес* без секционирования

[Выполните этот запрос](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAAAx3LSwqAMAwFwL3gHYKreh1xL7F9YrCtElP84OEV9zM4DZo5DsZjhGt6PqWTgL1p6+qhvaTEKjeI/FqyuZbGiwJf63QAi9vEL2UbAhtMEv6jyAH6+VhS9jOr1dULfUgAm2cAAAA=) во внешней таблице *таксиридес* , чтобы отобразить значения для каждого дня недели во всем наборе данных. 

```kusto
external_table("TaxiRides")
| summarize count() by dayofweek(pickup_datetime)
| render columnchart
```

Этот запрос показывает самый загруженный день недели. Поскольку данные не секционированы, выполнение этого запроса может занять много времени (до нескольких минут).

![обработать несекционированный запрос](media/data-lake-query-data/taxirides-no-partition.png)

#### <a name="query-taxirides-external-table-with-partitioning"></a>Запрос внешней таблицы Таксиридес с секционированием 

[Выполните этот запрос](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAAA13NQQqDMBQE0L3gHT6ukkVF3fQepXv5SQYMNWmIP6ilh68WuinM6jHMYBPkyPMobGao5s6bv3mHpdF19aZ1QgYlbx8ljY4F4gPIQFYgkvqJGrr+eun6I5ralv58OP27t5QQOPsXiOyzRFGazE6WzSh7wtnIiA75uISdOEtdfQDLWmP+ogAAAA==) во внешней таблице *таксиридес* , где показаны типы CAB-файлов такси (желтые или зеленые), используемые в январе 2017. 

```kusto
external_table("TaxiRides")
| where pickup_datetime between (datetime(2017-01-01) .. datetime(2017-02-01))
| summarize count() by cab_type
| render piechart
```

В этом запросе используется секционирование, что позволяет оптимизировать время и производительность запросов. Фильтры запросов в секционированном столбце (pickup_datetime) и возвращают результаты через несколько секунд.

![отобразить секционированный запрос](media/data-lake-query-data/taxirides-with-partition.png)
  
Можно написать дополнительные запросы для выполнения во внешней таблице *таксиридес* и получить дополнительные сведения о данных. 

## <a name="next-steps"></a>Следующие шаги

Запросите данные в Azure Data Lake с помощью обозреватель данных Azure. Научитесь [писать запросы](write-queries.md) и получать дополнительные аналитические сведения из данных.