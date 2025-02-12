---
title: Фильтры ограничения результатов поиска в индексе (служба "Поиск Azure")
description: Фильтрация по удостоверениям безопасности, языку, географическому расположению или числовым значениям для уменьшения результатов поиска в запросах в службе "Поиск Azure", размещенной облачной службе поиска в Microsoft Azure.
author: HeidiSteen
manager: cgronlun
services: search
ms.service: search
ms.topic: conceptual
ms.date: 06/13/2019
ms.author: heidist
ms.custom: seodec2018
ms.openlocfilehash: 4b5d198506473c598f058c881f781a06e191df88
ms.sourcegitcommit: cf438e4b4e351b64fd0320bf17cc02489e61406a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/08/2019
ms.locfileid: "67653436"
---
# <a name="filters-in-azure-search"></a>Фильтры в службе "Поиск Azure" 

*Фильтр* предоставляет критерии для выбора документов, используемых в запросе службы "Поиск Azure". Неотфильтрованные результаты поиска содержат все документы в индексе. Фильтр ограничивает поисковой запрос подмножеством документов. Например, фильтр может ограничивать полнотекстовый поиск продуктами определенного бренда, цвета или ценового диапазона, превышающего определенное пороговое значение.

Некоторые поисковые запросы устанавливают требования к фильтрам как часть реализации, но вы можете использовать фильтры, когда требуется ограничить поиск с помощью критериев *на основе значений* (поиск с ограничением поиска по типу продукта "книги" для категории "не фантастика", издательства Simon & Schuster).

Если вместо этого требуется выполнить поиск по определенным *структурам* данных (поиск с ограничением по полю "отзывы клиентов"), существуют альтернативные методы, описанные ниже.

## <a name="when-to-use-a-filter"></a>Когда следует использовать фильтр

Фильтры основаны на нескольких поисковых функциях, включая функцию поиска соседних объектов, навигацию по аспектам и фильтры безопасности, которые показывают только разрешенные документы. При реализации любой из этих функций требуется фильтр. Это фильтр, прикрепленный к поисковому запросу, который предоставляет координаты геолокации, выбранную пользователем категорию аспекта или идентификатор безопасности запрашивающего.

Ниже приведены примеры сценариев:

1. Используйте фильтр для создания среза индекса на основе значений данных в индексе. Учитывая схему с городом, типом жилья и удобствами, вы можете создать фильтр, чтобы явно выбирать документы, которые удовлетворяют вашим критериям (в Сиэтле, кондоминиумы, набережная). 

   Полнотекстовый поиск в одних и тех же входных данных часто приводит к аналогичным результатам, а его точность зависит от совпадения термина фильтра с содержимым вашего индекса. 

2. Используйте фильтр, если он требуется для поиска.

   * [Навигация по аспектам](search-faceted-navigation.md) использует фильтр для возврата категории аспекта, выбранной пользователем.
   * Геопространственный поиск использует фильтр для передачи координат текущего местоположения в приложениях с возможностью поиска соседних объектов. 
   * Фильтры безопасности передают идентификаторы безопасности в качестве критериев фильтра, где совпадение в индексе служит в качестве прокси-сервера для получения прав доступа к документу.

3. Используйте фильтр, если вы хотите использовать критерии поиска в числовом поле. 

   Числовые поля можно получить в документе, а также они могут отображаться в результатах поиска, но индивидуально они недоступны для поиска (при условии полнотекстового поиска). Если вам нужны критерии выбора, основанные на числовых данных, используйте фильтр.

### <a name="alternative-methods-for-reducing-scope"></a>Альтернативные методы для сокращения области поиска

Если требуется сузить область поиска в результатах, можно использовать не только фильтры. Эти возможности могут быть более эффективными в зависимости от задачи:

 + Параметр запроса `searchFields` ограничивает поиск конкретными полями. Например, если в вашем индексе содержатся отдельные поля для описания на английском и испанском языках, вы можете использовать searchFields, чтобы указать конкретные поля для полнотекстового поиска. 

+ Параметр `$select` используется, чтобы указать поля, которые требуется включить в результирующий набор для эффективной обрезки ответа перед его отправкой вызывающему приложению. Этот параметр не уточняет запрос и не уменьшает коллекцию документов, но если целью является меньшего размера ответ, этот параметр можно рассмотреть этот вариант. 

Дополнительные сведения о параметрах запроса см. в подразделе ["Параметры запроса" раздела "Запрос" статьи Search Documents (Azure Search Service REST API)](https://docs.microsoft.com/rest/api/searchservice/search-documents#request) (Поиск документов (REST API службы "Поиск" Azure)).


## <a name="how-filters-are-executed"></a>Том, как выполняются фильтры

Во время обработки запроса средство синтаксического анализа фильтра принимает критерии в качестве входных данных, преобразует их в атомарные логические выражения, представлены в виде дерева и затем оценивает дереве фильтров по фильтруемым полям в индексе.

Фильтрация выполняется вместе со службой поиска, допускающих какие документы включать в последующую обработку для извлечения документа и оценки релевантности. В сочетании со строкой поиска фильтр эффективно уменьшает отзыв набор последующей операции поиска. При использовании отдельно (например, когда строка запроса пуста, где `search=*`), критерием фильтра являются только входные данные. 

## <a name="defining-filters"></a>Определение фильтров

Фильтры представляют собой выражения OData, сформулированные с использованием [подмножества синтаксиса OData V4, поддерживаемого в службе "Поиск Azure"](https://docs.microsoft.com/rest/api/searchservice/odata-expression-syntax-for-azure-search). 

Вы можете указать один фильтр для каждого **поиска** операция, но сам фильтр может включать несколько полей, критериев и при использовании **ismatch** функция, несколько полнотекстового поиска. В выражении нескольких частей фильтра можно указать предикаты в любом порядке (в соответствии с правилами приоритета). Попытка поставить предикаты в определенной последовательности не даст значительного прироста производительности.

Одно из ограничений на выражение фильтра является ограничение максимального размера запроса. Весь запрос, включая функции фильтра, не может содержать более 16 МБ данных для POST или 8 КБ для GET. Также имеется ограничение на количество предложений в выражении фильтра. Хорошее проверенное правило: если у вас есть сотни предложений, вы рискуете превысить лимит. Мы рекомендуем разрабатывать приложение таким образом, чтобы оно не создавало фильтры неограниченного размера.

Следующие примеры представляют собой прототипные определения фильтров в нескольких API.

```http
# Option 1:  Use $filter for GET
GET https://[service name].search.windows.net/indexes/hotels/docs?search=*&$filter=baseRate lt 150&$select=hotelId,description&api-version=2019-05-06

# Option 2: Use filter for POST and pass it in the request body
POST https://[service name].search.windows.net/indexes/hotels/docs/search?api-version=2019-05-06
{
    "search": "*",
    "filter": "baseRate lt 150",
    "select": "hotelId,description"
}
```

```csharp
    parameters =
        new SearchParameters()
        {
            Filter = "baseRate lt 150",
            Select = new[] { "hotelId", "description" }
        };

    var results = searchIndexClient.Documents.Search("*", parameters);
```

## <a name="filter-usage-patterns"></a>Шаблоны использования фильтров

В следующих примерах показаны несколько шаблонов использования для сценариев фильтра. Дополнительные примеры можно найти в разделе [Примеры OData](https://docs.microsoft.com/azure/search/search-query-odata-filter#examples).

+ Автономный параметр **$filter** без строки запроса полезен, когда выражение фильтра может полностью определить интересующие документы. Без строки запроса не выполняется лексический или лингвистический анализ, нет оценки и рейтинга. Обратите внимание, что строка поиска является просто звездочку, что означает «сопоставление все документы».

   ```
   search=*&$filter=(baseRate ge 60 and baseRate lt 300) and accommodation eq 'Hotel' and city eq 'Nogales'
   ```

+ Сочетание строки запроса и **$filter**, где фильтр создает подмножество, а строка запроса предоставляет термины для полнотекстового поиска по отфильтрованному подмножеству. Использование фильтра со строкой запроса является наиболее распространенным шаблоном использования.

   ```
   search=hotels ocean$filter=(baseRate ge 60 and baseRate lt 300) and city eq 'Los Angeles'
   ```

+ Составные запросы, разделенные с помощью логического оператора or, каждый со своими критериями фильтра (например, beagles в категории dog или siamese в категории cat). Выражения в сочетании с `or` оцениваются индивидуально, с помощью оператора union документов, соответствующих каждое выражение, отправки обратно в ответе. Этот шаблон использования достигается за счет `search.ismatchscoring` функции. Можно также использовать версию без оценки, `search.ismatch`.

   ```
   # Match on hostels rated higher than 4 OR 5-star motels.
   $filter=search.ismatchscoring('hostel') and rating ge 4 or search.ismatchscoring('motel') and rating eq 5

   # Match on 'luxury' or 'high-end' in the description field OR on category exactly equal to 'Luxury'.
   $filter=search.ismatchscoring('luxury | high-end', 'description') or category eq 'Luxury'
   ```

  Можно также объединить полнотекстового поиска с помощью `search.ismatchscoring` с фильтрами с помощью `and` вместо `or`, но это функционально эквивалентно использованию `search` и `$filter` параметров в запросе поиска. Например следующие два запроса дают одинаковый результат:

  ```
  $filter=search.ismatchscoring('pool') and rating ge 4

  search=pool&$filter=rating ge 4
  ```

Исчерпывающие сведения о конкретных вариантах использования см. в следующих статьях:

+ [Фильтры аспекта](search-filters-facets.md)
+ [Фильтры языка](search-filters-language.md)
+ [Фильтрация в соответствии с политикой безопасности](search-security-trimming-for-azure-search.md) 

## <a name="field-requirements-for-filtering"></a>Требования к полям для фильтрации

В REST API фильтруемые является *на* по умолчанию для простых полей. Фильтруемые поля увеличивают размер индекса. Не забудьте установить `"filterable": false` для полей, которые вы не планируете фактически использовать в фильтре. Дополнительные сведения о параметрах для определения полей см. в статье [Create Index (Azure Search Service REST API)](https://docs.microsoft.com/rest/api/searchservice/create-index) (Создание индекса (REST API службы "Поиск Azure")).

В пакете SDK для .NET фильтруемые поля *отключены* по умолчанию. Вы можете обозначить поле, фильтруемое, задав [свойство IsFilterable](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.field.isfilterable?view=azure-dotnet) соответствующего [поле](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.field?view=azure-dotnet) объект `true`. Вы также можно сделать декларативно с помощью [IsFilterable атрибут](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.isfilterableattribute). В следующем примере атрибут задан для `BaseRate` свойства класса модели, которая сопоставляется с определением индекса.

```csharp
    [IsFilterable, IsSortable, IsFacetable]
    public double? BaseRate { get; set; }
```

### <a name="making-an-existing-field-filterable"></a>Создание фильтруемых существующего поля

Невозможно изменить существующие поля, чтобы сделать их фильтрацию. Вместо этого необходимо добавить новое поле или перестроить индекс. Дополнительные сведения о перестройке индекса или повторному заполнению полей, см. в разделе [перестроении индекса службы поиска Azure](search-howto-reindex.md).

## <a name="text-filter-fundamentals"></a>Принципы работы текстовых фильтров

Текстовые фильтры соответствует строковых полей для строковых литералов, которые вы указываете в фильтре. В отличие от полнотекстового поиска нет лексического анализа или разбиения на слова для текстовых фильтров, поэтому они только для точных совпадений. Например, предположим, что поле *f* содержит «солнечный день», `$filter=f eq 'Sunny'` не совпадают, но `$filter=f eq 'sunny day'` будет. 

Текстовые строки учитывают регистр. Нет нет нижнего регистра в верхнем регистре, слов: `$filter=f eq 'Sunny day'` не найдет «солнечный день».

### <a name="approaches-for-filtering-on-text"></a>Подходы для фильтрации по тексту

| Подход | Описание | Сценарии использования |
|----------|-------------|-------------|
| [`search.in`](search-query-odata-search-in-function.md) | Функция, которая совпадает с полем со списком с разделителями строк. | Рекомендуется для [фильтров безопасности](search-security-trimming-for-azure-search.md) и для любых фильтров, когда многие необработанные текстовые значения должны совпадать со строковым. **Search.in** функция предназначен для повышения быстродействия и намного быстрее, чем явное сравнение поля для каждой строки с помощью `eq` и `or`. | 
| [`search.ismatch`](search-query-odata-full-text-search-functions.md) | Функция, которая позволяет совместно использовать операции полнотекстового поиска вместе с операциями строго логического фильтра в одном выражении фильтра. | Используйте **search.ismatch** (или эквивалентное оценки **search.ismatchscoring**) Если требуется несколько сочетаний фильтра поиска в одном запросе. Вы также можете использовать ее для фильтра *contains* (для фильтрации в частичной строке в контексте большей строки). |
| [`$filter=field operator string`](search-query-odata-comparison-operators.md) | Определенное пользователем выражение, состоящее из поля, операторов и значений. | Используется, если вы хотите найти точные совпадения между полем строки и строковое значение. |

## <a name="numeric-filter-fundamentals"></a>Основные компоненты числового фильтра

Числовые поля недоступны для поиска (`searchable`) в контексте полнотекстового поиска. Только строки подлежат полному текстовому поиску. Например, при вводе 99,99 в качестве поискового запроса вы не сможете найти товары по цене 99,99 доллара США. Вместо этого отображаются элементы, которые содержат строку поля документа с номером 99. Таким образом, если у вас есть числовые данные, предполагается, что вы будете использовать их для фильтров, включая диапазоны, аспекты, группы и т. д. 

Документы, содержащие числовые поля (цена, размер, SKU, идентификатор), предоставляют эти значения в результатах поиска, если поле отмечено `retrievable`. Суть в том, что полнотекстовый поиск не применим к числовым типам полей.

## <a name="next-steps"></a>Следующие шаги

Сначала попробуйте использовать **обозреватель поиска** на портале, чтобы отправить запросы с параметрами **$filter**. [Пример индекса выборки недвижимости](search-get-started-portal.md) предоставляет полезные результаты для следующих отфильтрованных запросов при их вставке в строку поиска:

```
# Geo-filter returning documents within 5 kilometers of Redmond, Washington state
# Use $count=true to get a number of hits returned by the query
# Use $select to trim results, showing values for named fields only
# Use search=* for an empty query string. The filter is the sole input

search=*&$count=true&$select=description,city,postCode&$filter=geo.distance(location,geography'POINT(-122.121513 47.673988)') le 5

# Numeric filters use comparison like greater than (gt), less than (lt), not equal (ne)
# Include "and" to filter on multiple fields (baths and bed)
# Full text search is on John Leclerc, matching on John or Leclerc

search=John Leclerc&$count=true&$select=source,city,postCode,baths,beds&$filter=baths gt 3 and beds gt 4

# Text filters can also use comparison operators
# Wrap text in single or double quotes and use the correct case
# Full text search is on John Leclerc, matching on John or Leclerc

search=John Leclerc&$count=true&$select=source,city,postCode,baths,beds&$filter=city gt 'Seattle'
```

Дополнительные примеры можно найти в разделе [Примеры OData](https://docs.microsoft.com/azure/search/search-query-odata-filter#examples).

## <a name="see-also"></a>См. также

+ [Как работает полнотекстовый поиск в службе поиска Azure](search-lucene-query-architecture.md)
+ [Search Documents (Azure Search Service REST API)](https://docs.microsoft.com/rest/api/searchservice/search-documents) (Поиск по документам (REST API службы поиска Azure))
+ [Синтаксис простых запросов](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search)
+ [Синтаксис запросов Lucene](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search)
+ [Supported data types (Azure Search)](https://docs.microsoft.com/rest/api/searchservice/supported-data-types) (Поддерживаемые типы данных (служба "Поиск Azure")).
