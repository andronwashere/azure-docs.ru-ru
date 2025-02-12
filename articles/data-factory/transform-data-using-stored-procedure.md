---
title: Преобразование данных с помощью действия хранимой процедуры в фабрике данных Azure | Документация Майкрософт
description: В этой статье содержатся сведения о том, как использовать действия хранимой процедуры SQL Server для вызова хранимой процедуры в базе данных SQL Azure или хранилище данных из конвейера фабрики данных.
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 11/27/2018
author: nabhishek
ms.author: abnarain
manager: craigg
ms.openlocfilehash: 806654b7586895b62b014a49b8b3a00fb18f008f
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "60764413"
---
# <a name="transform-data-by-using-the-sql-server-stored-procedure-activity-in-azure-data-factory"></a>Преобразование данных с помощью действия хранимой процедуры SQL Server в фабрике данных Azure
> [!div class="op_single_selector" title1="Выберите версию службы фабрики данных, которую вы используете:"]
> * [Версия 1](v1/data-factory-stored-proc-activity.md)
> * [Текущая версия](transform-data-using-stored-procedure.md)

Действия преобразования данных в [конвейере](concepts-pipelines-activities.md) фабрики данных позволяют преобразовать необработанные данные и переработать их в прогнозы и аналитику. Действие хранимой процедуры — это одно из действий преобразования данных, которые поддерживает фабрика данных. Данная статья основана на материалах статьи о [преобразовании данных](transform-data.md), в которой приведен общий обзор преобразования данных и список поддерживаемых действий преобразования в фабрике данных.

> [!NOTE]
> Если вы не знакомы с Фабрикой данных Azure, сначала ознакомьтесь со статьей [Введение в фабрику данных Azure](introduction.md) и руководством [Преобразование данных в облаке с помощью действия Spark в фабрике данных Azure](tutorial-transform-data-spark-powershell.md) перед чтением этой статьи. 

C его помощью можно вызвать хранимую процедуру в одном из следующих хранилищ данных вашего предприятия или на виртуальной машине Azure. 

- База данных SQL Azure
- Хранилище данных SQL Azure
- База данных SQL Server.  Если вы используете SQL Server, установите локальную среду выполнения интеграции на том же компьютере, на котором размещена база данных, или на отдельном компьютере, имеющем доступ к базе данных. Локальная среда выполнения интеграции — это компонент, который обеспечивает безопасное и управляемое подключение локальных источников данных или данных виртуальной машины Azure к облачным службам. Дополнительные сведения см. в статье [Создание и настройка локальной среды выполнения интеграции](create-self-hosted-integration-runtime.md).

> [!IMPORTANT]
> При копировании данных в базу данных SQL Azure или SQL Server можно настроить класс **SqlSink** в действии копирования для вызова хранимой процедуры с помощью свойства **sqlWriterStoredProcedureName**. Дополнительные сведения о свойствах см. в этих статьях: [Копирование данных в базу данных Azure SQL и из нее с помощью фабрики данных Azure](connector-azure-sql-database.md), [Копирование данных в базу данных SQL Server и из нее с помощью фабрики данных Azure](connector-sql-server.md). Вызов хранимой процедуры во время копирования данных в хранилище данных SQL Azure с помощью операции копирования не поддерживается. Однако действие хранимой процедуры можно использовать для вызова хранимой процедуры в хранилище данных SQL. 
>
> При копировании данных из базы данных SQL Azure, SQL Server или хранилища данных SQL Azure можно настроить **SqlSource** в действии копирования, чтобы вызвать хранимую процедуру для считывания данных из исходной базы данных с помощью свойства **sqlReaderStoredProcedureName**. Дополнительные сведения в этих статьях: [Копирование данных в базу данных Azure SQL и из нее с помощью фабрики данных Azure](connector-azure-sql-database.md), [Копирование данных в базу данных SQL Server и из нее с помощью фабрики данных Azure](connector-sql-server.md), [Копирование данных в хранилище данных Azure SQL и из него с помощью фабрики данных Azure](connector-azure-sql-data-warehouse.md).          

 

## <a name="syntax-details"></a>Сведения о синтаксисе
Ниже приведен формат JSON для определения действия хранимой процедуры:

```json
{
    "name": "Stored Procedure Activity",
    "description":"Description",
    "type": "SqlServerStoredProcedure",
    "linkedServiceName": {
        "referenceName": "AzureSqlLinkedService",
        "type": "LinkedServiceReference"
    },
    "typeProperties": {
        "storedProcedureName": "usp_sample",
        "storedProcedureParameters": {
            "identifier": { "value": "1", "type": "Int" },
            "stringData": { "value": "str1" }

        }
    }
}
```

В следующей таблице описаны эти свойства JSON:

| Свойство                  | ОПИСАНИЕ                              | Обязательно для заполнения |
| ------------------------- | ---------------------------------------- | -------- |
| name                      | Имя действия.                     | Yes      |
| description               | Текст, описывающий, для чего используется действие | Нет       |
| type                      | Для действия хранимой процедуры используется тип действия **SqlServerStoredProcedure**. | Да      |
| linkedServiceName         | Ссылка на **базу данных SQL Azure**, **хранилище данных SQL Azure** или сервер **SQL Server**, зарегистрированный в качестве связанной службы в фабрике данных. Дополнительные сведения об этой связанной службе см. в статье [Вычислительные среды, поддерживаемые фабрикой данных Azure](compute-linked-services.md). | Да      |
| storedProcedureName       | Укажите имя хранимой процедуры, которую нужно вызвать. | Да      |
| storedProcedureParameters | Укажите значения для параметров хранимой процедуры. Используйте `"param1": { "value": "param1Value","type":"param1Type" }`, чтобы передать значения параметра и их тип, поддерживаемый источником данных. Если для параметра необходимо передать значение null, используйте синтаксис `"param1": { "value": null }` (все знаки в нижнем регистре). | Нет       |

## <a name="error-info"></a>Сведения об ошибке

Если не удается выполнить хранимую процедуру и возвращаются сведения об ошибке, нельзя получить их непосредственно из выходных данных действия. Но Фабрика данных переносит все события выполнения действий в Azure Monitor. В составе этих событий Фабрика данных отправляет в Azure Monitor и сведения об ошибке. Можно, например, настроить для таких событий оповещения по электронной почте. Дополнительные сведения см. в статье об [использовании оповещений и мониторинге фабрик данных с помощью Azure Monitor](monitor-using-azure-monitor.md).

## <a name="next-steps"></a>Дальнейшие действия
Ознакомьтесь со следующими ссылками, в которых описаны способы преобразования данных другими способами: 

* [Действие U-SQL](transform-data-using-data-lake-analytics.md)
* [Действие Hive](transform-data-using-hadoop-hive.md)
* [Действие Pig](transform-data-using-hadoop-pig.md)
* [Действие MapReduce](transform-data-using-hadoop-map-reduce.md)
* [Потоковая активность Hadoop](transform-data-using-hadoop-streaming.md)
* [Действие Spark](transform-data-using-spark.md)
* [Настраиваемое действие .NET](transform-data-using-dotnet-custom-activity.md)
* [Создание прогнозирующих конвейеров с помощью службы "Машинное обучение Azure" и фабрики данных Azure](transform-data-using-machine-learning.md)
* [Действие хранимой процедуры](transform-data-using-stored-procedure.md)
