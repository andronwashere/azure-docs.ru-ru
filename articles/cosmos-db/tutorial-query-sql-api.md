---
title: Как выполнять запросы с помощью SQL в базе данных Azure Cosmos DB
description: Сведения о выполнении запросы с помощью SQL в базе данных Azure Cosmos DB
author: rimman
ms.author: rimman
ms.service: cosmos-db
ms.custom: tutorial-develop, mvc
ms.topic: tutorial
ms.date: 05/21/2019
ms.reviewer: sngun
ms.openlocfilehash: 9c6bd2040cb90f7a837efdf11d33a6e6999bc944
ms.sourcegitcommit: e9a46b4d22113655181a3e219d16397367e8492d
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/21/2019
ms.locfileid: "65965500"
---
# <a name="tutorial-query-azure-cosmos-db-by-using-the-sql-api"></a>Руководство по Выполнение запросов в Azure Cosmos DB с использованием API SQL

[API SQL](documentdb-introduction.md) базы данных Azure Cosmos DB позволяет выполнять запросы к документам с помощью SQL. В этой статье содержится пример документа и два примера SQL-запросов и результатов.

В этой статье рассматриваются следующие задачи: 

> [!div class="checklist"]
> * Выполнение запросов к данным с помощью SQL.

## <a name="sample-document"></a>Пример документа

Запросы SQL в этой статье используют следующий пример документа.

```json
{
  "id": "WakefieldFamily",
  "parents": [
      { "familyName": "Wakefield", "givenName": "Robin" },
      { "familyName": "Miller", "givenName": "Ben" }
  ],
  "children": [
      {
        "familyName": "Merriam", 
        "givenName": "Jesse", 
        "gender": "female", "grade": 1,
        "pets": [
            { "givenName": "Goofy" },
            { "givenName": "Shadow" }
        ]
      },
      { 
        "familyName": "Miller", 
         "givenName": "Lisa", 
         "gender": "female", 
         "grade": 8 }
  ],
  "address": { "state": "NY", "county": "Manhattan", "city": "NY" },
  "creationDate": 1431620462,
  "isRegistered": false
}
```
## <a name="where-can-i-run-sql-queries"></a>Где могут выполняться SQL-запросы

Вы можете выполнять запросы с помощью обозревателя данных на портале Azure, через [REST API и пакеты SDK](sql-api-sdk-dotnet.md) и даже через [площадку для запросов](https://www.documentdb.com/sql/demo), которая выполняет запросы с использованием существующего набора примера данных.

Дополнительные сведения об SQL-запросах см. в статье:
* [SQL-запросы и синтаксис SQL в DocumentDB](how-to-sql-query.md)

## <a name="prerequisites"></a>Предварительные требования

В этом руководстве предполагается, что у вас есть учетная запись и коллекция базы данных Azure Cosmos DB. У вас их нет? Выполните задачи [5-минутного краткого руководства](create-mongodb-nodejs.md).

## <a name="example-query-1"></a>Пример запроса 1

С учетом приведенного выше образца документа со сведениями о семье следующий SQL-запрос возвращает документы, в которых поле идентификатора соответствует `WakefieldFamily`. Так как это инструкция `SELECT *`, результатом выполнения запроса является сформированный документ JSON:

**Запрос**

    SELECT * 
    FROM Families f 
    WHERE f.id = "WakefieldFamily"

**Результаты**

```json
{
  "id": "WakefieldFamily",
  "parents": [
      { "familyName": "Wakefield", "givenName": "Robin" },
      { "familyName": "Miller", "givenName": "Ben" }
  ],
  "children": [
      {
        "familyName": "Merriam", 
        "givenName": "Jesse", 
        "gender": "female", "grade": 1,
        "pets": [
            { "givenName": "Goofy" },
            { "givenName": "Shadow" }
        ]
      },
      { 
        "familyName": "Miller", 
         "givenName": "Lisa", 
         "gender": "female", 
         "grade": 8 }
  ],
  "address": { "state": "NY", "county": "Manhattan", "city": "NY" },
  "creationDate": 1431620462,
  "isRegistered": false
}
```

## <a name="example-query-2"></a>Пример запроса 2

Следующий запрос возвращает все заданные имена детей в семье, идентификатор которых соответствует `WakefieldFamily`, упорядоченные по классу обучения.

**Запрос**

    SELECT c.givenName 
    FROM Families f 
    JOIN c IN f.children 
    WHERE f.id = 'WakefieldFamily'

**Результаты**

[ { "givenName": "Jesse" }, { "givenName": "Lisa" } ]


## <a name="next-steps"></a>Дополнительная информация

В этом руководстве вы выполнили следующее:

> [!div class="checklist"]
> * Вы научились выполнять запросы с помощью SQL.  

Теперь вы можете приступать к следующему руководству, чтобы узнать, как глобально распределять данные.

> [!div class="nextstepaction"]
> [Глобальное распределение данных](tutorial-global-distribution-sql-api.md)

