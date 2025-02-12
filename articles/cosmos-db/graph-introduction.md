---
title: 'Общие сведения об Azure Cosmos DB: API Gremlin'
description: Узнайте, как можно использовать базу данных Azure Cosmos DB, чтобы хранить, запрашивать и передавать массивные графы с минимальной задержкой с помощью языка запросов графа Gremlin в Apache TinkerPop.
author: LuisBosquez
ms.service: cosmos-db
ms.subservice: cosmosdb-graph
ms.topic: overview
ms.date: 07/18/2019
ms.author: lbosq
ms.openlocfilehash: 1f46eb1995e2e7cb098098ebd22eedbd194dc6a6
ms.sourcegitcommit: f5075cffb60128360a9e2e0a538a29652b409af9
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/18/2019
ms.locfileid: "68310236"
---
# <a name="introduction-to-azure-cosmos-db-gremlin-api"></a>Знакомство с Azure Cosmos DB: API Gremlin

[Azure Cosmos DB](introduction.md)  — это глобально распределенная, многомодельная служба базы данных Майкрософт, необходимая для работы с критически важными приложениями. Это многомодельная база данных, которая поддерживает модели документов, пар "ключ-значение", графов и "столбцов-семейств". Azure Cosmos DB: API Gremlin используется для хранения и работы с графическими данными в полностью управляемой службе базы данных при любом масштабе.  

![Архитектура графа базы данных Azure Cosmos DB](./media/graph-introduction/cosmosdb-graph-architecture.png)

В этой статье содержатся сведения об интерфейсе API Gremlin для Azure Cosmos DB и объясняется, как его использовать для хранения больших графов с миллиардами вершин и ребер. Вы можете запрашивать графы с задержкой в миллисекунды и легко развивать структуру графов. Azure Cosmos DB Gremlin API основан на стандарте базы данных графа [Apache TinkerPop](https://tinkerpop.apache.org)  и использует язык запросов Gremlin. 

Gremlin API Azure Cosmos DB объединяет мощь алгоритмов базы данных графов с хорошо масштабируемой управляемой инфраструктурой, обеспечивая уникальное гибкое решение для наиболее распространенных проблем с данными, связанных с отсутствием гибкости и реляционных подходов. 

## <a name="features-of-azure-cosmos-db-graph-database"></a>Функции базы данных графов в Azure Cosmos DB
 
База данных Azure Cosmos DB является полностью управляемой базой данных графа, которая обеспечивает глобальное распределение, гибкое масштабирование хранилища и пропускной способности, автоматическое индексирование и возможность выполнять запросы, настраиваемые уровни согласованности, а также поддержку стандартной платформы вычисления графа TinkerPop. 

Ниже перечислены отличительные особенности, которые предлагает Azure Cosmos DB Gremlin API.

* **Гибко масштабируемые пропускная способность и хранилище**

  На практике графы должны масштабироваться за пределы емкости одного сервера. Azure Cosmos DB поддерживает горизонтально масштабируемые базы данных графа, которые могут иметь практически неограниченный размер с точки зрения хранилища и предоставляемой пропускной способности. По мере увеличения масштаба базы данных графа, данные будут автоматически распределяться с использованием [секционирования данных графа](https://docs.microsoft.com/azure/cosmos-db/graph-partitioning).

* **Репликация между несколькими регионами**

  Azure Cosmos DB может автоматически реплицировать данные графа в любой регион Azure по всему миру. Глобальная репликация упрощает разработку приложений, которым требуется глобальный доступ к данным. Помимо минимизации задержки чтения и записи в любой точке мира, Azure Cosmos DB предоставляет механизм автоматической отработки отказа в любом регионе, который может обеспечить непрерывность работы вашего приложения в редких случаях прерывания обслуживания в регионе. 

* **Быстрые запросы и обходы с использованием наиболее распространенного стандарта запросов графа**

  Храните разные типы вершин и ребер, а также выполняйте их запросы, используя знакомый синтаксис Gremlin. Gremlin — это императивный, функциональный язык запросов, который предоставляет комфортный интерфейс для реализации общих алгоритмов графов. 
  
  Azure Cosmos DB позволяет формировать многофункциональные запросы и обходы в режиме реального времени без необходимости указывать подсказки для схемы, вторичные индексы или представления. Дополнительные сведения см. в статье [Поддержка графа Gremlin в базе данных Azure Cosmos DB](gremlin-support.md).

* **Полностью управляемая база данных графа**

  Azure Cosmos DB устраняет необходимость управления базой данных и вычислительными ресурсами. Большинство существующих платформ баз данных графов связаны с ограничениями их инфраструктуры и часто требуют высокой степени обслуживания для обеспечения их работы. 
  
  Полностью управляемая служба Cosmos DB избавляет от необходимости управлять виртуальными машинами, обновлять программное обеспечение среды выполнения, управлять сегментированием или репликацией или заниматься сложными обновлениями на уровне данных. Каждый граф автоматически сохраняется и защищается от региональных сбоев. Эти гарантии позволяют разработчикам сосредоточиться на обеспечении ценности приложения вместо эксплуатации своих баз данных графов и управления ими. 

* **Автоматическая индексация**

  По умолчанию база данных Azure Cosmos DB автоматически индексирует все свойства в узлах и ребрах в графе, не ожидая и не требуя наличия какой-либо схемы или создания дополнительных индексов. Дополнительные сведения об [индексировании в Azure Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/index-overview). 

* **Совместимость с Apache TinkerPop**

  Azure Cosmos DB поддерживает [стандарт Apache TinkerPop с открытым исходным кодом](http://tinkerpop.apache.org/). Стандарт Tinkerpop имеет обширную экосистему приложений и библиотек, которые легко интегрируются с Gremlin API Azure Cosmos DB. 

* **Настраиваемые уровни согласованности**

  Azure Cosmos DB предоставляет пять четко определенных уровней согласованности для достижения правильного компромисса между согласованностью и производительностью приложения. Для запросов и операций чтения служба Azure Cosmos DB предлагает пять отдельных уровней согласованности — сильный, с ограниченным устареванием, сеансовый, с согласованностью префиксов и согласованный в конечном счете. Эти разделенные, четко определенные уровни согласованности позволяют принимать обоснованные компромиссы между показателями согласованности, доступности и задержки. Дополнительные сведения см. в статье [Настраиваемые уровни согласованности данных в Azure Cosmos DB](consistency-levels.md).

## <a name="scenarios-that-can-use-gremlin-api"></a>Сценарии, в которых будет полезен интерфейс API Gremlin
Ниже приведены некоторые полезные примеры использования поддержки графа базы данных Azure Cosmos DB.

* **Социальные сети/Клиент 365**

  Объединяя данные о клиентах и их взаимодействиях с другими людьми, вы можете разработать персонализированные средства, предугадать поведение клиента или объединить людей на основе их интересов. Базу данных Azure Cosmos DB можно использовать для управления социальными сетями и отслеживания данных о клиенте и его предпочтениях.

* **Системы рекомендаций**

  Широко используется в сфере розничной торговли. Объединяя сведения о продукции, пользователях и операциях пользователей, например приобретении, поиске и оценке товара, вы можете создать пользовательские рекомендации. База данных Azure Cosmos DB с минимальной задержкой, гибким масштабированием и встроенной поддержкой графа представляет собой идеальный вариант моделирования этих взаимосвязей.

* **Геопространственные функции**

  Многим приложениям в сферах телекоммуникаций, логистики и туристического планирования требуется поиск интересующего расположения в регионе или же обнаружение самого короткого (оптимального) пути между двумя расположениями. База данных Azure Cosmos DB поможет легко справиться с этими задачами.

* **Интернет вещей**

  Исследуя сеть и подключения между устройствами Интернета вещей, смоделированными как граф, вы сможете разобраться с состоянием своих устройств и ресурсов. Кроме того, вы сможете узнать, как изменения одного компонента сети могут повлиять на другой.

## <a name="introduction-to-graph-databases"></a>Основные сведения о базах данных графов
В реальном мире все данные связаны естественным образом. Традиционное моделирование данных фокусируется на отдельном определении объектов и вычислении их отношений во время выполнения. Несмотря на то, что у этой модели есть свои преимущества, управление данными с высокой степенью связности может оказаться сложной задачей в условиях ограничений.  

Подход базы данных графа основан на сохранении связей на уровне хранилища, что приводит к высокоэффективным операциям извлечения графов. Azure Cosmos DB Gremlin API поддерживает [модель графа свойства](https://tinkerpop.apache.org/docs/current/reference/#intro).

### <a name="property-graph-objects"></a>Объекты графа свойства

[Граф](http://mathworld.wolfram.com/Graph.html) — это структура, состоящая из [вершин](http://mathworld.wolfram.com/GraphVertex.html) и [ребер](http://mathworld.wolfram.com/GraphEdge.html). Оба объекта могут иметь произвольное число пар ключ-значение в качестве свойств. 

* **Вершинами** обозначаются дискретные объекты, например пользователь, место или событие.

* **Ребра** обозначают взаимосвязи между вершинами. Например, пользователь может знать другого пользователя, участвовать в событии или посетить мероприятие. 

* **Свойства** выражают сведения о вершинах и ребрах. В вершинах или ребрах может быть любое количество свойств, и их можно использовать для описания и фильтрации объектов в запросе. Свойства примера включают в себя вершину, у которой есть имя и возраст, или ребро, которое может иметь отметку времени и (или) вес. 

Базы данных графов часто относят к категории баз данных NoSQL, или нереляционных баз данных, поскольку в них нет зависимости от схемы или ограничений модели данных. Это отсутствие схемы позволяет естественно и эффективно моделировать и хранить связанные структуры. 

### <a name="gremlin-by-example"></a>Обзор Gremlin на примере
Давайте рассмотрим пример графа, чтобы лучше понять, как в Gremlin могут выражаться запросы. На рисунке ниже в форме графа показано бизнес-приложение, управляющее данными о пользователях, их интересах и устройствах.  

![Пример базы данных, в котором показаны люди, их интересы и устройства](./media/gremlin-support/sample-graph.png) 

Этот граф содержит следующие типы *вершин* (которые в Gremlin называются "метки"):

- **Люди**. На графе представлено три человека: Робин (Robin), Томас (Thomas) и Бен (Ben).
- **Интересы**. Их интересы. В этом случае — футбол.
- **Устройства**. Устройства, которые эти люди используют.
- **Операционные системы**. Операционные системы, под управлением которых работают устройства.

Мы представим взаимосвязи между этими сущностями, используя следующие типы *ребер* или метки:

- **Знакомства**. Например, "Томас знает Робин".
- **Интересы**. Используется, чтобы представить интересы людей на графе. Например, "Бен интересуется футболом".
- **Использующаяся операционная система**. Ноутбук работает под управлением Windows.
- **Используемые устройства**. Применяется, чтобы представить используемое устройство. Например, Робин использует телефон Motorola с серийным номером 77.

Теперь выполним некоторые операции с этим графом с помощью [консоли Gremlin](https://tinkerpop.apache.org/docs/3.3.2/reference/#gremlin-console). Вы также можете выполнить эти операции с помощью драйверов Gremlin на платформе по вашему усмотрению — Java, Node.js, Python или .NET.  Прежде чем перейти к поддерживаемым базой данных Azure Cosmos DB функциям, давайте рассмотрим несколько примеров, чтобы ознакомиться с синтаксисом.

Сначала рассмотрим операции CRUD (создание, чтение, обновление и удаление). Следующая инструкция Gremlin вставляет вершину "Thomas" в граф:

```java
:> g.addV('person').property('id', 'thomas.1').property('firstName', 'Thomas').property('lastName', 'Andersen').property('age', 44)
```

Затем инструкция Gremlin вставляет ребро "знакомства" между вершинами Thomas и Robin.

```java
:> g.V('thomas.1').addE('knows').to(g.V('robin.1'))
```

Следующий запрос возвращает вершины "людей" в порядке по убыванию их имен:
```java
:> g.V().hasLabel('person').order().by('firstName', decr)
```

Если часть графа подсвечивается, необходимо ответить на вопросы типа: "Какие операционные системы используют друзья Томаса?" Вы можете выполнить эту операцию обхода Gremlin, чтобы получить сведения из графа:

```java
:> g.V('thomas.1').out('knows').out('uses').out('runsos').group().by('name').by(count())
```

## <a name="next-steps"></a>Дополнительная информация
Дополнительные сведения о поддержке графа в базе данных Azure Cosmos DB см. в следующих ресурсах:

* [Azure Cosmos DB. Создание приложения .NET с помощью API Graph](create-graph-dotnet.md)
* Дополнительные сведения см. в статье [Поддержка графа Gremlin в базе данных Azure Cosmos DB](gremlin-support.md).
