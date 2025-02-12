---
title: Поддержка Gremlin в базе данных Azure Cosmos DB
description: Узнайте о языке Gremlin из Apache TinkerPop. Узнайте, какие функции и действия доступны в Azure Cosmos DB
author: LuisBosquez
ms.service: cosmos-db
ms.subservice: cosmosdb-graph
ms.topic: overview
ms.date: 06/24/2019
ms.author: lbosq
ms.openlocfilehash: db263c1c7f0a8b87b315c5aa6da31336229c9643
ms.sourcegitcommit: 837dfd2c84a810c75b009d5813ecb67237aaf6b8
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/02/2019
ms.locfileid: "67502726"
---
# <a name="azure-cosmos-db-gremlin-graph-support"></a>Поддержка графа Gremlin в базе данных Azure Cosmos DB
Azure Cosmos DB поддерживает язык обхода графов [Apache Tinkerpop](https://tinkerpop.apache.org), известный как [Gremlin](https://tinkerpop.apache.org/docs/3.3.2/reference/#graph-traversal-steps). Вы можете использовать язык Gremlin, чтобы создать сущности графа (вершины и ребра), изменить свойства в этих сущностях, выполнить запросы и обходы графа, а также удалить сущности. 

Эта статья содержит краткое руководство по языку Gremlin с перечислением функций Gremlin, поддерживаемых в API Gremlin.

## <a name="compatible-client-libraries"></a>Совместимые клиентские библиотеки

В таблице ниже приведены распространенные драйверы Gremlin, которые вы можете использовать для базы данных Azure Cosmos DB.

| Скачивание | Источник | Начало работы | Поддерживаемая версия соединителя |
| --- | --- | --- | --- |
| [.NET](https://tinkerpop.apache.org/docs/3.3.1/reference/#gremlin-DotNet) | [Gremlin.NET в GitHub](https://github.com/apache/tinkerpop/tree/master/gremlin-dotnet) | [Создание приложения Graph с помощью .NET](create-graph-dotnet.md) | 3.4.0-RC2 |
| [Java](https://mvnrepository.com/artifact/com.tinkerpop.gremlin/gremlin-java) | [Документация по Gremlin для Java](https://tinkerpop.apache.org/javadocs/current/full/) | [Создание приложения Graph с помощью Java](create-graph-java.md) | 3.2.0 и выше |
| [Node.js](https://www.npmjs.com/package/gremlin) | [Gremlin для JavaScript в GitHub](https://github.com/jbmusso/gremlin-javascript) | [Создание приложения Graph с помощью Node.js](create-graph-nodejs.md) | 3.3.4 и выше |
| [Python](https://tinkerpop.apache.org/docs/3.3.1/reference/#gremlin-python) | [Gremlin-Python в GitHub](https://github.com/apache/tinkerpop/tree/master/gremlin-python) | [Создание приложения Graph с помощью Python](create-graph-python.md) | 3.2.7 |
| [PHP](https://packagist.org/packages/brightzone/gremlin-php) | [Gremlin-PHP в GitHub](https://github.com/PommeVerte/gremlin-php) | [Создание приложения Graph с помощью PHP](create-graph-php.md) | 3.1.0 |
| [Консоль Gremlin](https://tinkerpop.apache.org/downloads.html) | [Документация по TinkerPop](https://tinkerpop.apache.org/docs/current/reference/#gremlin-console) |  [Создание приложения Graph с помощью консоли Gremlin](create-graph-gremlin-console.md) | 3.2.0 и выше |

## <a name="supported-graph-objects"></a>Поддерживаемые объекты Graph
TinkerPop — это стандартная платформа, которая охватывает широкий ряд технологий графа. Таким образом, она содержит стандартную терминологию для описания функций, которые предоставляет поставщик графа. База данных Azure Cosmos DB обеспечивает временную базу данных графа с высокой степенью параллелизма и возможностью записи, которую можно разделить на секции на нескольких серверах или кластерах. 

В таблице ниже перечислены функции TinkerPop, реализованные в базе данных Azure Cosmos DB: 

| Категория | Реализация базы данных Azure Cosmos DB |  Примечания | 
| --- | --- | --- |
| Функции графа | Обеспечивает сохраняемость и одновременный доступ. Разработаны для поддержки транзакций. | Вычислительные методы могут применяться через соединитель Spark. |
| Функции переменной | Поддерживаются такие типы значений: логическое значение, целое число, байт, двойное число с плавающей точкой, число с плавающей точкой, целое число со знаком, полная дата, строка. | Поддерживаются несложные типы. Совместимы со сложными типами через модель данных. |
| Функции вершины | Поддерживаются RemoveVertices, MetaProperties, AddVertices, MultiProperties, StringIds, UserSuppliedIds, AddProperty, RemoveProperty.  | Поддерживается создание, изменение и удаление вершин. |
| Функции свойств вершины | Поддерживаются StringIds, UserSuppliedIds, AddProperty, RemoveProperty, BooleanValues, ByteValues, DoubleValues, FloatValues, IntegerValues, LongValues, StringValues. | Поддерживается создание, изменение и удаление свойств вершины. |
| Функции ребра | AddEdges, RemoveEdges, StringIds, UserSuppliedIds, AddProperty, RemoveProperty | Поддерживается создание, изменение и удаление ребра. |
| Функции свойств ребра | Поддерживаются типы Properties, BooleanValues, ByteValues, DoubleValues, FloatValues, IntegerValues, LongValues, StringValues. | Поддерживается создание, изменение и удаление свойств ребра. |

## <a name="gremlin-wire-format-graphson"></a>Формат подключения Gremlin: GraphSON

При возвращении результатов операций Gremlin в базе данных Azure Cosmos DB используется [формат GraphSON](https://tinkerpop.apache.org/docs/3.3.2/reference/#graphson-reader-writer). GraphSON представляет собой стандартный формат Gremlin для представления вершин, ребер и свойств (единичных и со множественными значениями) с помощью JSON. 

Например, в следующем фрагменте кода показано представление GraphSON вершины, *возвращенной клиенту* из базы данных Azure Cosmos DB. 

```json
  {
    "id": "a7111ba7-0ea1-43c9-b6b2-efc5e3aea4c0",
    "label": "person",
    "type": "vertex",
    "outE": {
      "knows": [
        {
          "id": "3ee53a60-c561-4c5e-9a9f-9c7924bc9aef",
          "inV": "04779300-1c8e-489d-9493-50fd1325a658"
        },
        {
          "id": "21984248-ee9e-43a8-a7f6-30642bc14609",
          "inV": "a8e3e741-2ef7-4c01-b7c8-199f8e43e3bc"
        }
      ]
    },
    "properties": {
      "firstName": [
        {
          "value": "Thomas"
        }
      ],
      "lastName": [
        {
          "value": "Andersen"
        }
      ],
      "age": [
        {
          "value": 45
        }
      ]
    }
  }
```

Свойства вершин, используемые в формате GraphSON, описаны ниже:

| Свойство | ОПИСАНИЕ | 
| --- | --- | --- |
| `id` | Идентификатор вершины. Должен иметь уникальное значение (если применимо — со значением `_partition`). Если значение не указано, оно будет предоставляться автоматически с помощью идентификатора GUID. | 
| `label` | Метка вершины. Это свойство используется для описания типа сущности. |
| `type` | Используется для отделения вершин от документов, не связанных с графом. |
| `properties` | Набор определенных пользователем свойств, связанных с вершиной. Каждое свойство может иметь несколько значений. |
| `_partition` | Ключ секции вершины. Используется для [секционирования данных графа](graph-partitioning.md). |
| `outE` | Это свойство содержит список внешних ребер вершины. Хранение смежных сведений с помощью вершины обеспечивает быстрое выполнение обхода графов. Ребра сгруппированы на основе меток. |

Ребро содержит приведенные ниже сведения, необходимые для перехода в другие части графа.

| Свойство | ОПИСАНИЕ |
| --- | --- |
| `id` | Идентификатор ребра. Должен иметь уникальное значение (если применимо — со значением `_partition`). |
| `label` | Метка ребра. Это свойство является необязательным и используется для описания типа связи. |
| `inV` | Это свойство содержит список внутренних вершин для ребра. Хранение смежных сведений с помощью ребра обеспечивает быстрое выполнение обхода графов. Вершины сгруппированы на основе меток. |
| `properties` | Набор определенных пользователем свойств, связанных с ребром. Каждое свойство может иметь несколько значений. |

Каждое свойство может хранить несколько значений в массиве. 

| Свойство | ОПИСАНИЕ |
| --- | --- |
| `value` | Значение свойства.

## <a name="gremlin-steps"></a>Шаги Gremlin
Теперь рассмотрим шаги Gremlin, поддерживаемые базой данных Azure Cosmos DB. Дополнительные сведения о Gremlin см. в [руководстве по TinkerPop](https://tinkerpop.apache.org/docs/3.3.2/reference).

| Шаг | ОПИСАНИЕ | Руководство по TinkerPop 3.2 |
| --- | --- | --- |
| `addE` | Добавляет ребро между двумя вершинами. | [Шаг addE](https://tinkerpop.apache.org/docs/3.3.2/reference/#addedge-step) |
| `addV` | Добавляет вершину в граф. | [Шаг addV](https://tinkerpop.apache.org/docs/3.3.2/reference/#addvertex-step) |
| `and` | Обеспечивает возвращение значения для всех обходов. | [Шаг and](https://tinkerpop.apache.org/docs/3.3.2/reference/#and-step) |
| `as` | Модулятор шага для назначения переменной выходным данным шага. | [Шаг as](https://tinkerpop.apache.org/docs/3.3.2/reference/#as-step) |
| `by` | Модулятор шага, используемый с `group` и `order`. | [Шаг by](https://tinkerpop.apache.org/docs/3.3.2/reference/#by-step) |
| `coalesce` | Возвращает первый обход, который возвращает результат. | [Шаг coalesce](https://tinkerpop.apache.org/docs/3.3.2/reference/#coalesce-step) |
| `constant` | Возвращает постоянное значение. Используется с `coalesce`.| [Шаг constant](https://tinkerpop.apache.org/docs/3.3.2/reference/#constant-step) |
| `count` | Возвращает число из обхода. | [Шаг count](https://tinkerpop.apache.org/docs/3.3.2/reference/#count-step) |
| `dedup` | Возвращает значения с удаленными повторяющимися значениями. | [Шаг dedup](https://tinkerpop.apache.org/docs/3.3.2/reference/#dedup-step) |
| `drop` | Удаляет значения (вершины или ребра). | [Шаг drop](https://tinkerpop.apache.org/docs/3.3.2/reference/#drop-step) |
| `executionProfile` | Создает описание всех операций, формируемых выполненным шагом Gremlin | [Шаг executionProfile](graph-execution-profile.md) |
| `fold` | Действует как барьер, который вычисляет статистическое значение результатов.| [Шаг fold](https://tinkerpop.apache.org/docs/3.3.2/reference/#fold-step) |
| `group` | Группирует значения на основе указанных меток.| [Шаг group](https://tinkerpop.apache.org/docs/3.3.2/reference/#group-step) |
| `has` | Используется для фильтрации свойств, вершин и ребер. Поддерживает варианты `hasLabel`, `hasId`, `hasNot` и `has`. | [Шаг has](https://tinkerpop.apache.org/docs/3.3.2/reference/#has-step) |
| `inject` | Вставляет значения в поток.| [Шаг inject](https://tinkerpop.apache.org/docs/3.3.2/reference/#inject-step) |
| `is` | Используется для выполнения фильтра с помощью логического выражения. | [Шаг is](https://tinkerpop.apache.org/docs/3.3.2/reference/#is-step) |
| `limit` | Используется для ограничения числа элементов в обходе.| [Шаг limit](https://tinkerpop.apache.org/docs/3.3.2/reference/#limit-step) |
| `local` | Локально обертывает раздел обхода аналогично вложенному запросу. | [Шаг local](https://tinkerpop.apache.org/docs/3.3.2/reference/#local-step) |
| `not` | Используется для создания отрицания фильтра. | [Шаг not](https://tinkerpop.apache.org/docs/3.3.2/reference/#not-step) |
| `optional` | Возвращает результат указанного обхода, если он выдается. В противном случае возвращается вызывающий элемент. | [Шаг optional](https://tinkerpop.apache.org/docs/3.3.2/reference/#optional-step) |
| `or` | Гарантирует, что по крайней мере один из обходов возвращает значение. | [Шаг or](https://tinkerpop.apache.org/docs/3.3.2/reference/#or-step) |
| `order` | Возвращает результаты в заданном порядке сортировки. | [Шаг order](https://tinkerpop.apache.org/docs/3.3.2/reference/#order-step) |
| `path` | Возвращает полный путь обхода. | [Шаг path](https://tinkerpop.apache.org/docs/3.3.2/reference/#path-step) |
| `project` | Выполняет проекцию свойств в виде сопоставления. | [Шаг project](https://tinkerpop.apache.org/docs/3.3.2/reference/#project-step) |
| `properties` | Возвращает свойства для указанных меток. | [Шаг properties](https://tinkerpop.apache.org/docs/3.3.2/reference/#_properties_step) |
| `range` | Выполняет фильтрацию до заданного диапазона значений.| [Шаг range](https://tinkerpop.apache.org/docs/3.3.2/reference/#range-step) |
| `repeat` | Повторяет шаг указанное количество раз. Используется для циклов. | [Шаг repeat](https://tinkerpop.apache.org/docs/3.3.2/reference/#repeat-step) |
| `sample` | Используется для вывода примеров результатов из обхода. | [Шаг sample](https://tinkerpop.apache.org/docs/3.3.2/reference/#sample-step) |
| `select` | Используется для проектирования результатов из обхода. |  [Шаг select](https://tinkerpop.apache.org/docs/3.3.2/reference/#select-step) |
| `store` | Используется для статистических функций из обхода без блокировки. | [Шаг store](https://tinkerpop.apache.org/docs/3.3.2/reference/#store-step) |
| `TextP.startingWith(string)` | Функция фильтрации строк. Эта функция используется в качестве предиката для шага `has()` для сопоставления свойства, начинающегося с определенной строки. | [Предикаты TextP](https://tinkerpop.apache.org/docs/3.4.0/reference/#a-note-on-predicates) |
| `TextP.endingWith(string)` |  Функция фильтрации строк. Эта функция используется в качестве предиката для шага `has()` для сопоставления свойства, заканчивающегося определенной строкой. | [Предикаты TextP](https://tinkerpop.apache.org/docs/3.4.0/reference/#a-note-on-predicates) |
| `TextP.containing(string)` | Функция фильтрации строк. Эта функция используется в качестве предиката для шага `has()` для сопоставления свойства, содержащего определенную строку. | [Предикаты TextP](https://tinkerpop.apache.org/docs/3.4.0/reference/#a-note-on-predicates) |
| `TextP.notStartingWith(string)` | Функция фильтрации строк. Эта функция используется в качестве предиката для шага `has()` для сопоставления свойства, не начинающегося с определенной строки. | [Предикаты TextP](https://tinkerpop.apache.org/docs/3.4.0/reference/#a-note-on-predicates) |
| `TextP.notEndingWith(string)` | Функция фильтрации строк. Эта функция используется в качестве предиката для шага `has()` для сопоставления свойства, не заканчивающегося определенной строкой. | [Предикаты TextP](https://tinkerpop.apache.org/docs/3.4.0/reference/#a-note-on-predicates) |
| `TextP.notContaining(string)` | Функция фильтрации строк. Эта функция используется в качестве предиката для шага `has()` для сопоставления свойства, не содержащего определенную строку. | [Предикаты TextP](https://tinkerpop.apache.org/docs/3.4.0/reference/#a-note-on-predicates) |
| `tree` | Выполняет статистическое вычисление путей из вершины в дерево. | [Шаг tree](https://tinkerpop.apache.org/docs/3.3.2/reference/#tree-step) |
| `unfold` | Развертывает итератор.| [Шаг unfold](https://tinkerpop.apache.org/docs/3.3.2/reference/#unfold-step) |
| `union` | Объединяет результаты из нескольких обходов.| [Шаг union](https://tinkerpop.apache.org/docs/3.3.2/reference/#union-step) |
| `V` | Содержит шаги, необходимые для обходов между вершинами и ребрами (`V`, `E`, `out`, `in`, `both`, `outE`, `inE`, `bothE`, `outV`, `inV`, `bothV`) и `otherV` — для других вершин. | [Шаги vertex](https://tinkerpop.apache.org/docs/3.3.2/reference/#vertex-steps) |
| `where` | Используется для фильтрации результатов из обхода. Поддерживает операторы `eq`, `neq`, `lt`, `lte`, `gt`, `gte` и `between`.  | [Шаг where](https://tinkerpop.apache.org/docs/3.3.2/reference/#where-step) |

Оптимизированный для операций записи обработчик Azure Cosmos DB по умолчанию поддерживает автоматическое индексирование всех свойств вершин и ребер. Следовательно, запросы с фильтрами, запросы диапазона, сортировка или статистические функции для любого свойства обрабатываются из индекса и эффективно обслуживаются. Дополнительные сведения о выполнении индексирования в базе данных Azure Cosmos DB см. в руководстве об [индексировании без использования схем](https://www.vldb.org/pvldb/vol8/p1668-shukla.pdf).

## <a name="next-steps"></a>Дополнительная информация
* Создайте приложение графа [с использованием пакетов SDK](create-graph-dotnet.md). 
* Дополнительные сведения о [поддержке графа](graph-introduction.md) в Azure Cosmos DB
