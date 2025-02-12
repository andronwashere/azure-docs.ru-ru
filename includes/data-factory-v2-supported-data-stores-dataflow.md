---
author: linda33wj
ms.service: data-factory
ms.topic: include
ms.date: 05/24/2019
ms.author: jingwang
ms.openlocfilehash: 70ffc91477d4a77619ba7df3b2ab121fea5f8bac
ms.sourcegitcommit: 79496a96e8bd064e951004d474f05e26bada6fa0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/02/2019
ms.locfileid: "67509911"
---
| Category | Хранилище данных | Поддерживаются в качестве [действие копирования](../articles/data-factory/copy-activity-overview.md) источника | Поддерживаются в качестве [действие копирования](../articles/data-factory/copy-activity-overview.md) приемника | Поддерживается [Azure IR](../articles/data-factory/concepts-integration-runtime.md#azure-integration-runtime) | Поддерживается [локальной IR](../articles/data-factory/concepts-integration-runtime.md#self-hosted-integration-runtime) | Поддерживаемые [потока данных](../articles/data-factory/concepts-data-flow-overview.md)
|:--- |:--- |:--- |:--- |:--- |:--- |
| **Таблицы Azure** |[Хранилище BLOB-объектов Azure](../articles/data-factory/connector-azure-blob-storage.md) |✓ |✓ |✓ |✓  | ✓ <br> <small>Поддерживаемые форматы: Текст с разделителями, Parquet</small> |
| &nbsp; |[Azure Cosmos DB (SQL API)](../articles/data-factory/connector-azure-cosmos-db.md) |✓ |✓ |✓ |✓  ||
| &nbsp; |[API Azure Cosmos DB для MongoDB](../articles/data-factory/connector-azure-cosmos-db-mongodb-api.md) |✓ |✓ |✓ |✓  ||
| &nbsp; |[Обозреватель данных Azure](../articles/data-factory/connector-azure-data-explorer.md) |✓ |✓ |✓ |✓ ||
| &nbsp; |[Хранилище Azure Data Lake Gen1](../articles/data-factory/connector-azure-data-lake-store.md) |✓ |✓ |✓ |✓  |✓ <br> <small>Поддерживаемые форматы: Текст с разделителями, Parquet</small> |
| &nbsp; |[Хранилище Azure Data Lake Storage 2-го поколения](../articles/data-factory/connector-azure-data-lake-storage.md) |✓ |✓ |✓ |✓  |✓ <br> <small>Поддерживаемые форматы: Текст с разделителями, Parquet</small> |
| &nbsp; |[База данных Azure для MariaDB](../articles/data-factory/connector-azure-database-for-mariadb.md) |✓ | |✓ |✓  ||
| &nbsp; |[База данных Azure для MySQL](../articles/data-factory/connector-azure-database-for-mysql.md) |✓ | |✓ |✓  ||
| &nbsp; |[База данных Azure для PostgreSQL](../articles/data-factory/connector-azure-database-for-postgresql.md) |✓ | |✓ |✓  ||
| &nbsp; |[Хранилище файлов Azure](../articles/data-factory/connector-azure-file-storage.md) |✓ |✓ |✓ |✓  ||
| &nbsp; |[база данных SQL Azure;](../articles/data-factory/connector-azure-sql-database.md) |✓ |✓ |✓ |✓  |✓  |
| &nbsp; |[Управляемый экземпляр базы данных SQL Azure](../articles/data-factory/connector-azure-sql-database-managed-insance.md) |✓ |✓ |✓ |✓  ||
| &nbsp; |[Хранилище данных Azure SQL](../articles/data-factory/connector-azure-sql-data-warehouse.md) |✓ |✓ |✓ |✓  |✓  |
| &nbsp; |[Индекс службы поиска Azure](../articles/data-factory/connector-azure-search.md) | |✓ |✓ |✓  ||
| &nbsp; |[хранилище таблиц Azure](../articles/data-factory/connector-azure-table-storage.md) |✓ |✓ |✓ |✓  ||
| **База данных** |[Amazon Redshift](../articles/data-factory/connector-amazon-redshift.md) |✓ | |✓ |✓  ||
| &nbsp; |[DB2](../articles/data-factory/connector-db2.md) |✓ | |✓ |✓  ||
| &nbsp; |[Детализация (предварительная версия)](../articles/data-factory/connector-drill.md) |✓ | |✓ |✓  ||
| &nbsp; |[Google BigQuery](../articles/data-factory/connector-google-bigquery.md) |✓ | |✓ |✓  ||
| &nbsp; |[Greenplum](../articles/data-factory/connector-greenplum.md) |✓ | |✓ |✓  ||
| &nbsp; |[HBase](../articles/data-factory/connector-hbase.md) |✓ | |✓ |✓  ||
| &nbsp; |[Hive](../articles/data-factory/connector-hive.md) |✓ | |✓ |✓  ||
| &nbsp; |[Apache Impala (предварительная версия)](../articles/data-factory/connector-impala.md) |✓ | |✓ |✓  ||
| &nbsp; |[Informix](../articles/data-factory/connector-odbc.md#ibm-informix-source) |✓ | | |✓  ||
| &nbsp; |[MariaDB](../articles/data-factory/connector-mariadb.md) |✓ | |✓ |✓  ||
| &nbsp; |[Microsoft Access](../articles/data-factory/connector-odbc.md#microsoft-access-source) |✓ | | |✓  ||
| &nbsp; |[MySQL](../articles/data-factory/connector-mysql.md) |✓ | |✓ |✓  ||
| &nbsp; |[Netezza](../articles/data-factory/connector-netezza.md) |✓ | |✓ |✓  ||
| &nbsp; |[Oracle](../articles/data-factory/connector-oracle.md) |✓ |✓ |✓ |✓  ||
| &nbsp; |[Phoenix](../articles/data-factory/connector-phoenix.md) |✓ | |✓ |✓  ||
| &nbsp; |[PostgreSQL](../articles/data-factory/connector-postgresql.md) |✓ | |✓ |✓  ||
| &nbsp; |[Presto (предварительная версия)](../articles/data-factory/connector-presto.md) |✓ | |✓ |✓  ||
| &nbsp; |[Открытый центр SAP Business Warehouse](../articles/data-factory/connector-sap-business-warehouse-open-hub.md) |✓ | | |✓  ||
| &nbsp; |[SAP Business Warehouse через MDX](../articles/data-factory/connector-sap-business-warehouse.md) |✓ | | |✓  ||
| &nbsp; |[SAP HANA](../articles/data-factory/connector-sap-hana.md) |✓ |✓ | |✓  ||
| &nbsp; |[Таблица SAP](../articles/data-factory/connector-sap-table.md) |✓ | | |✓  ||
| &nbsp; |[Spark](../articles/data-factory/connector-spark.md) |✓ | |✓ |✓  ||
| &nbsp; |[SQL Server](../articles/data-factory/connector-sql-server.md) |✓ |✓ |✓ |✓  ||
| &nbsp; |[Sybase](../articles/data-factory/connector-sybase.md) |✓ | | |✓  ||
| &nbsp; |[Teradata](../articles/data-factory/connector-teradata.md) |✓ | |✓ |✓  ||
| &nbsp; |[Vertica](../articles/data-factory/connector-vertica.md) |✓ | |✓ |✓  ||
| **NoSQL** |[Cassandra](../articles/data-factory/connector-cassandra.md) |✓ | |✓ |✓  ||
| &nbsp; |[Couchbase (предварительная версия)](../articles/data-factory/connector-couchbase.md) |✓ | |✓ |✓  ||
| &nbsp; |[MongoDB](../articles/data-factory/connector-mongodb.md) |✓ | |✓ |✓  ||
| **File** |[Amazon S3](../articles/data-factory/connector-amazon-simple-storage-service.md) |✓ | |✓ |✓  ||
| &nbsp; |[Перемещение данных в локальную файловую систему или из нее с помощью фабрики данных Azure](../articles/data-factory/connector-file-system.md) |✓ |✓ |✓ |✓  ||
| &nbsp; |[FTP](../articles/data-factory/connector-ftp.md) |✓ | |✓ |✓  ||
| &nbsp; |[Google Cloud Storage](../articles/data-factory/connector-google-cloud-storage.md) |✓ | |✓ |✓  ||
| &nbsp; |[HDFS](../articles/data-factory/connector-hdfs.md) |✓ | |✓ |✓  ||
| &nbsp; |[SFTP](../articles/data-factory/connector-sftp.md) |✓ | |✓ |✓  ||
| **Универсальный протокол** |[Базовый HTTP-запрос](../articles/data-factory/connector-http.md) |✓ | |✓ |✓  ||
| &nbsp; |[Generic OData](../articles/data-factory/connector-odata.md) |✓ | |✓ |✓  ||
| &nbsp; |[Универсальные данные ODBC](../articles/data-factory/connector-odbc.md) |✓ |✓ | |✓  ||
| &nbsp; |[Универсальные данные REST](../articles/data-factory/connector-rest.md) |✓ | |✓ |✓  ||
| **Службы и приложения** |[Веб-служба Amazon Marketplace (предварительная версия)](../articles/data-factory/connector-amazon-marketplace-web-service.md) |✓ | |✓ |✓  ||
| &nbsp; |[Common Data Service для приложений](../articles/data-factory/connector-dynamics-crm-office-365.md) |✓ |✓ |✓ |✓  ||
| &nbsp; |[Concur (предварительная версия)](../articles/data-factory/connector-concur.md) |✓ | |✓ |✓  ||
| &nbsp; |[Dynamics 365](../articles/data-factory/connector-dynamics-crm-office-365.md) |✓ |✓ |✓ |✓  ||
| &nbsp; |[Dynamics AX (предварительная версия)](../articles/data-factory/connector-dynamics-ax.md) |✓ | |✓ |✓  ||
| &nbsp; |[Dynamics CRM](../articles/data-factory/connector-dynamics-crm-office-365.md) |✓ |✓ |✓ |✓  ||
| &nbsp; |[Google AdWords (предварительная версия)](../articles/data-factory/connector-google-adwords.md) |✓ | |✓ |✓  ||
| &nbsp; |[HubSpot (предварительная версия)](../articles/data-factory/connector-hubspot.md) |✓ | |✓ |✓  ||
| &nbsp; |[Jira (предварительная версия)](../articles/data-factory/connector-jira.md) |✓ | |✓ |✓  ||
| &nbsp; |[Magento (предварительная версия)](../articles/data-factory/connector-magento.md) |✓ | |✓ |✓  ||
| &nbsp; |[Marketo (предварительная версия)](../articles/data-factory/connector-marketo.md) |✓ | |✓ |✓  ||
| &nbsp; |[Office 365](../articles/data-factory/connector-office-365.md) |✓ | |✓ |✓  ||
| &nbsp; |[Oracle Eloqua (предварительная версия)](../articles/data-factory/connector-oracle-eloqua.md) |✓ | |✓ |✓  ||
| &nbsp; |[Oracle Responsys (предварительная версия)](../articles/data-factory/connector-oracle-responsys.md) |✓ | |✓ |✓  ||
| &nbsp; |[Oracle Service Cloud (предварительная версия)](../articles/data-factory/connector-oracle-service-cloud.md) |✓ | |✓ |✓  ||
| &nbsp; |[Paypal (предварительная версия)](../articles/data-factory/connector-paypal.md) |✓ | |✓ |✓  ||
| &nbsp; |[QuickBooks (предварительная версия)](../articles/data-factory/connector-quickbooks.md) |✓ | |✓ |✓  ||
| &nbsp; |[Salesforce](../articles/data-factory/connector-salesforce.md) |✓ |✓ |✓ |✓  ||
| &nbsp; |[Облачная служба SalesForce](../articles/data-factory/connector-salesforce.md) |✓ |✓ |✓ |✓  ||
| &nbsp; |[Salesforce Marketing Cloud (предварительная версия)](../articles/data-factory/connector-salesforce-marketing-cloud.md) |✓ | |✓ |✓  ||
| &nbsp; |[SAP Cloud for Customer (C4C)](../articles/data-factory/connector-sap-cloud-for-customer.md) |✓ |✓ |✓ |✓  ||
| &nbsp; |[SAP ECC](../articles/data-factory/connector-sap-ecc.md) |✓ | |✓ |✓ ||
| &nbsp; |[ServiceNow](../articles/data-factory/connector-servicenow.md) |✓ | |✓ |✓  ||
| &nbsp; |[Shopify (предварительная версия)](../articles/data-factory/connector-shopify.md) |✓ | |✓ |✓  ||
| &nbsp; |[Square (предварительная версия)](../articles/data-factory/connector-square.md) |✓ | |✓ |✓  ||
| &nbsp; |[Веб-таблица (таблица HTML)](../articles/data-factory/connector-web-table.md) |✓ | | |✓  ||
| &nbsp; |[Xero (предварительная версия)](../articles/data-factory/connector-xero.md) |✓ | |✓ |✓  ||
| &nbsp; |[Zoho (предварительная версия)](../articles/data-factory/connector-zoho.md) |✓ | |✓ |✓  ||

> [!NOTE]
> Пометка *предварительная версия* возле соединителя означает, что его можно опробовать и оставить отзыв.  Если вы хотите использовать в своем решении зависимость от соединителей в предварительной версии, обратитесь в службу [поддержки Azure](https://azure.microsoft.com/support/).
