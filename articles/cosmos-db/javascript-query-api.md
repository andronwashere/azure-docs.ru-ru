---
title: Работа с API запросов на основе языка JavaScript в Azure Cosmos DB
description: В этой статье рассматриваются понятия, связанные с API запросов на основе языка JavaScript и необходимые, чтобы создать хранимые процедуры и триггеры в Azure Cosmos DB.
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/23/2019
ms.author: mjbrown
ms.reviewer: sngun
ms.openlocfilehash: cc1815ce4a7a9ed40848e4a67a7fd9e032c1daa1
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "66226195"
---
# <a name="javascript-query-api-in-azure-cosmos-db"></a>API для выполнения запросов JavaScript в Azure Cosmos DB

Кроме выдачи запросов с использованием API SQL в Azure Cosmos DB, [серверный пакет SDK Cosmos DB](https://azure.github.io/azure-cosmosdb-js-server/) позволяет выполнять оптимизированные запросы с помощью интерфейса JavaScript. Не обязательно знать язык SQL для использования этого интерфейса JavaScript. API запросов в JavaScript позволяет программно создавать запросы, передавая предикатные функции в образующие последовательность вызовы функций. Для этого используется синтаксис, который понимают встроенные элементы массива ECMAScript5 и популярные библиотеки JavaScript, например Lodash. Чтобы запросы были эффективно выполнены с помощью индексов Azure Cosmos DB, их анализирует среда выполнения JavaScript.

## <a name="supported-javascript-functions"></a>Поддерживаемые функции JavaScript

| **Function** | **Описание** |
|---------|---------|
|`chain() ... .value([callback] [, options])`|Начинает вызов по цепочке, который должен завершаться value().|
|`filter(predicateFunction [, options] [, callback])`|Фильтрует входные данные с помощью функции предиката, которая возвращает значение true или false для фильтрации входных документов в наборе результатов. Эта функция действует так же, как предложение WHERE в SQL.|
|`flatten([isShallow] [, options] [, callback])`|Собирает и объединяет массивы для каждого входного элемента в один массив. Эта функция действует так же, как предложение SelectMany в LINQ.|
|`map(transformationFunction [, options] [, callback])`|Применяет проекцию, заданную функцией преобразования, которая сопоставляет каждый входной элемент с объектом или значением JavaScript. Эта функция действует так же, как предложение SELECT в SQL.|
|`pluck([propertyName] [, options] [, callback])`|Эта функция заменяет собой сопоставление, которое извлекает значение определенного свойства из каждого входного элемента.|
|`sortBy([predicate] [, options] [, callback])`|Используя заданный предикат, создает новый набор документов путем сортировки документов во входном потоке документов по возрастанию. Эта функция действует так же, как предложение ORDER BY в SQL.|
|`sortByDescending([predicate] [, options] [, callback])`|Используя заданный предикат, создает новый набор документов путем сортировки документов во входном потоке документов по убыванию. Эта функция действует так же, как предложение ORDER BY x DESC в SQL.|
|`unwind(collectionSelector, [resultSelector], [options], [callback])`|Выполняет самосоединение со внутренним массивом и добавляет в итоговую проекцию результаты с обеих сторон в виде кортежей. Например, в результате присоединения документа person к person.pets создадутся кортежи [person, pet]. Эта функция похожа на SelectMany в .NET LINK.|

Если конструкции JavaScript включены в предикатную функцию и/или селектор, они автоматически оптимизированы для непосредственного выполнения в индексах Azure Cosmos DB:

- Простые операторы: `=` `+` `-` `*` `/` `%` `|` `^` `&` `==` `!=` `===` `!===` `<` `>` `<=` `>=` `||` `&&` `<<` `>>` `>>>!` `~`
- литералы, включая литерал объекта: {};
- var, return.

Следующие конструкции JavaScript не оптимизированы для использования в индексах Azure Cosmos DB:

- Поток управления (например if, for или while)
- вызовы функций.

Более подробные сведения см. в [документации по серверному пакету JavaScript для Cosmos DB](https://azure.github.io/azure-cosmosdb-js-server/).

## <a name="sql-to-javascript-cheat-sheet"></a>Таблица соответствия запросов SQL и JavaScript

В следующей таблице представлены разные запросы SQL и соответствующие им запросы JavaScript. Как и запросы SQL, свойства (например, item.id) чувствительны к регистру.

> [!NOTE]
> `__` (двойное подчеркивание) является псевдонимом для `getContext().getCollection()` при использовании API запросов JavaScript.

|**SQL**|**API запросов JavaScript**|**Описание**|
|---|---|---|
|SELECT *<br>FROM docs| __.map(function(doc) { <br>&nbsp;&nbsp;&nbsp;&nbsp;return doc;<br>});|Возвращает все документы (с разбивкой на страницы с отметками "продолжить").|
|SELECT <br>&nbsp;&nbsp;&nbsp;docs.id,<br>&nbsp;&nbsp;&nbsp;docs.message AS msg,<br>&nbsp;&nbsp;&nbsp;docs.actions <br>FROM docs|__.map(function(doc) {<br>&nbsp;&nbsp;&nbsp;&nbsp;return {<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;id: doc.id,<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;msg: doc.message,<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;actions:doc.actions<br>&nbsp;&nbsp;&nbsp;&nbsp;};<br>});|Создает проекцию идентификатора, сообщения (msg) и действия для всех документов.|
|SELECT *<br>FROM docs<br>WHERE<br>&nbsp;&nbsp;&nbsp;docs.id="X998_Y998"|__.filter(function(doc) {<br>&nbsp;&nbsp;&nbsp;&nbsp;return doc.id ==="X998_Y998";<br>});|Запрашивает документы с предикатом: id = "X998_Y998".|
|SELECT *<br>FROM docs<br>WHERE<br>&nbsp;&nbsp;&nbsp;ARRAY_CONTAINS(docs.Tags, 123)|__.filter(function(x) {<br>&nbsp;&nbsp;&nbsp;&nbsp;return x.Tags && x.Tags.indexOf(123) > -1;<br>});|Запрашивает все документы, имеющие свойство Tags. Tags является массивом, содержащим значение 123.|
|SELECT<br>&nbsp;&nbsp;&nbsp;docs.id,<br>&nbsp;&nbsp;&nbsp;docs.message AS msg<br>FROM docs<br>WHERE<br>&nbsp;&nbsp;&nbsp;docs.id="X998_Y998"|__.chain()<br>&nbsp;&nbsp;&nbsp;&nbsp;.filter(function(doc) {<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;return doc.id ==="X998_Y998";<br>&nbsp;&nbsp;&nbsp;&nbsp;})<br>&nbsp;&nbsp;&nbsp;&nbsp;.map(function(doc) {<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;return {<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;id: doc.id,<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;msg: doc.message<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;};<br>&nbsp;&nbsp;&nbsp;&nbsp;})<br>.value();|Запрашивает документы с предикатом id = "X998_Y998" и затем выполняет проекцию идентификатора и сообщения (msg).|
|SELECT VALUE tag<br>FROM docs<br>JOIN tag IN docs.Tags<br>ORDER BY docs._ts|__.chain()<br>&nbsp;&nbsp;&nbsp;&nbsp;.filter(function(doc) {<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;return doc.Tags && Array.isArray(doc.Tags);<br>&nbsp;&nbsp;&nbsp;&nbsp;})<br>&nbsp;&nbsp;&nbsp;&nbsp;.sortBy(function(doc) {<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;return doc._ts;<br>&nbsp;&nbsp;&nbsp;&nbsp;})<br>&nbsp;&nbsp;&nbsp;&nbsp;.pluck("Tags")<br>&nbsp;&nbsp;&nbsp;&nbsp;.flatten()<br>&nbsp;&nbsp;&nbsp;&nbsp;.value()|Выполняет фильтрацию для получения документов, у которых есть свойство массива Tags, и сортирует отобранные документы по системному свойству метки отметки _ts, после чего выполняет проекцию и объединение массива Tags.|

## <a name="next-steps"></a>Дальнейшие действия

Дополнительные сведения о том, как записать и использовать хранимые процедуры, триггеры и определяемые пользователем функции в Azure Cosmos DB, см. в статьях ниже:

- [How to write stored procedures and triggers using Javascript Query API](how-to-write-javascript-query-api.md) (Как записывать хранимые процедуры и триггеры с помощью API запросов JavaScript)
- [Working with Azure Cosmos DB stored procedures, triggers, and user-defined functions](stored-procedures-triggers-udfs.md) (Работа с хранимыми процедурами, триггерами и определяемыми пользователем функциями в Azure Cosmos DB)
- [Как зарегистрировать и использовать хранимые процедуры, триггеры и определяемые пользователем функции в Azure Cosmos DB](how-to-use-stored-procedures-triggers-udfs.md)
- [Azure Cosmos DB JavaScript server-side API reference](https://azure.github.io/azure-cosmosdb-js-server) (Справочные материалы по API на стороне сервера для JavaScript Azure Cosmos DB)
- [Статья о JavaScript ES6 (ECMA 2015)](https://www.ecma-international.org/ecma-262/6.0/)
