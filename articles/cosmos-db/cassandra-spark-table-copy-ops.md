---
title: Операции копирования табличных данных в API Cassandra для Azure Cosmos DB из Spark.
description: В этой статье рассказывается, как копировать данные между таблицами в API Cassandra для Azure Cosmos DB.
author: kanshiG
ms.author: govindk
ms.reviewer: sngun
ms.service: cosmos-db
ms.subservice: cosmosdb-cassandra
ms.topic: conceptual
ms.date: 09/24/2018
ms.openlocfilehash: c98582d5fe11b87d2ba88d5fb247a87cc905e1dc
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "60895187"
---
# <a name="table-copy-operations-on-azure-cosmos-db-cassandra-api-from-spark"></a>Операции копирования табличных данных в API Cassandra для Azure Cosmos DB из Spark

В этой статье рассказывается, как копировать данные между таблицами в API Cassandra для Azure Cosmos DB из Spark. Команды, описанные в этой статье, также можно использовать для копирования данных из таблиц Apache Cassandra в таблицы API Cassandra для Azure Cosmos DB.

## <a name="cassandra-api-configuration"></a>Конфигурация API Cassandra

```scala
import org.apache.spark.sql.cassandra._
//Spark connector
import com.datastax.spark.connector._
import com.datastax.spark.connector.cql.CassandraConnector

//CosmosDB library for multiple retry
import com.microsoft.azure.cosmosdb.cassandra

//Connection-related
spark.conf.set("spark.cassandra.connection.host","YOUR_ACCOUNT_NAME.cassandra.cosmosdb.azure.com")
spark.conf.set("spark.cassandra.connection.port","10350")
spark.conf.set("spark.cassandra.connection.ssl.enabled","true")
spark.conf.set("spark.cassandra.auth.username","YOUR_ACCOUNT_NAME")
spark.conf.set("spark.cassandra.auth.password","YOUR_ACCOUNT_KEY")
spark.conf.set("spark.cassandra.connection.factory", "com.microsoft.azure.cosmosdb.cassandra.CosmosDbConnectionFactory")
//Throughput-related...adjust as needed
spark.conf.set("spark.cassandra.output.batch.size.rows", "1")
spark.conf.set("spark.cassandra.connection.connections_per_executor_max", "10")
spark.conf.set("spark.cassandra.output.concurrent.writes", "1000")
spark.conf.set("spark.cassandra.concurrent.reads", "512")
spark.conf.set("spark.cassandra.output.batch.grouping.buffer.size", "1000")
spark.conf.set("spark.cassandra.connection.keep_alive_ms", "600000000")
```

## <a name="insert-sample-data"></a>Вставка демонстрационных данных 
```scala
val booksDF = Seq(
   ("b00001", "Arthur Conan Doyle", "A study in scarlet", 1887,11.33),
   ("b00023", "Arthur Conan Doyle", "A sign of four", 1890,22.45),
   ("b01001", "Arthur Conan Doyle", "The adventures of Sherlock Holmes", 1892,19.83),
   ("b00501", "Arthur Conan Doyle", "The memoirs of Sherlock Holmes", 1893,14.22),
   ("b00300", "Arthur Conan Doyle", "The hounds of Baskerville", 1901,12.25)
).toDF("book_id", "book_author", "book_name", "book_pub_year","book_price")

booksDF.write
  .mode("append")
  .format("org.apache.spark.sql.cassandra")
  .options(Map( "table" -> "books", "keyspace" -> "books_ks", "output.consistency.level" -> "ALL", "ttl" -> "10000000"))
  .save()
```

## <a name="copy-data-between-tables"></a>Копирование данных между таблицами

### <a name="copy-data-between-tables-destination-table-exists"></a>Копирование данных между таблицами (целевая таблица существует)

```scala
//1) Create destination table
val cdbConnector = CassandraConnector(sc)
cdbConnector.withSessionDo(session => session.execute("CREATE TABLE IF NOT EXISTS books_ks.books_copy(book_id TEXT PRIMARY KEY,book_author TEXT, book_name TEXT,book_pub_year INT,book_price FLOAT) WITH cosmosdb_provisioned_throughput=4000;"))

//2) Read from one table
val readBooksDF = sqlContext
  .read
  .format("org.apache.spark.sql.cassandra")
  .options(Map( "table" -> "books", "keyspace" -> "books_ks"))
  .load

//3) Save to destination table
readBooksDF.write
  .cassandraFormat("books_copy", "books_ks", "")
  .save()

//4) Validate copy to destination table
sqlContext
  .read
  .format("org.apache.spark.sql.cassandra")
  .options(Map( "table" -> "books_copy", "keyspace" -> "books_ks"))
  .load
  .show
```

### <a name="copy-data-between-tables-destination-table-does-not-exist"></a>Копирование данных между таблицами (целевая таблица не существует)

```scala
import com.datastax.spark.connector._

//1) Read from source table
val readBooksDF = sqlContext
  .read
  .format("org.apache.spark.sql.cassandra")
  .options(Map( "table" -> "books", "keyspace" -> "books_ks"))
  .load

//2) Creates an empty table in the keyspace based off of source table
val newBooksDF = readBooksDF
newBooksDF.createCassandraTable(
    "books_ks", 
    "books_new", 
    partitionKeyColumns = Some(Seq("book_id"))
    //clusteringKeyColumns = Some(Seq("some column"))
    )

//3) Saves the data from the source table into the newly created table
newBooksDF.write
  .cassandraFormat("books_new", "books_ks","")
  .save()

//4) Validate table creation and data load
sqlContext
  .read
  .format("org.apache.spark.sql.cassandra")
  .options(Map( "table" -> "books_new", "keyspace" -> "books_ks"))
  .load
  .show
```
Результаты
```
+-------+------------------+--------------------+----------+-------------+
|book_id|       book_author|           book_name|book_price|book_pub_year|
+-------+------------------+--------------------+----------+-------------+
| b00300|Arthur Conan Doyle|The hounds of Bas...|     12.25|         1901|
| b00001|Arthur Conan Doyle|  A study in scarlet|     11.33|         1887|
| b00023|Arthur Conan Doyle|      A sign of four|     22.45|         1890|
| b00501|Arthur Conan Doyle|The memoirs of Sh...|     14.22|         1893|
| b01001|Arthur Conan Doyle|The adventures of...|     19.83|         1892|
+-------+------------------+--------------------+----------+-------------+

import com.datastax.spark.connector._
readBooksDF: org.apache.spark.sql.DataFrame = [book_id: string, book_author: string ... 3 more fields]
newBooksDF: org.apache.spark.sql.DataFrame = [book_id: string, book_author: string ... 3 more fields]
```

## <a name="next-steps"></a>Дальнейшие действия

 * Начните с [создания учетной записи API Cassandra, базы данных и таблицы](create-cassandra-api-account-java.md) с помощью приложения Java.
 * [Загрузите демонстрационные данные в таблицу API Cassandra](cassandra-api-load-data.md) с помощью приложения Java.
 * [Запросите данные из учетной записи API Cassandra](cassandra-api-query-data.md) с помощью приложения Java.
