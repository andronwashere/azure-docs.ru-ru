---
title: Анализаторы для лингвистической обработки и обработки текста в службе "Поиск Azure"
description: Назначьте анализаторы доступным для поиска текстовым полям в индексе, чтобы заменить стандартный, используемый по умолчанию анализатор Lucene стандартными настраиваемыми альтернативными вариантами или вариантами для определенных языков.
services: search
ms.service: search
ms.topic: conceptual
ms.date: 03/27/2019
ms.author: heidist
manager: cgronlun
author: HeidiSteen
ms.custom: seodec2018
ms.openlocfilehash: f76d944f614f07a4428d4e4100f6a08a375d96dc
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "65795795"
---
# <a name="analyzers-for-text-processing-in-azure-search"></a>Анализаторы для обработки текста в службе "Поиск Azure"

*Анализатор* — это компонент [полнотекстовой поисковой системы](search-lucene-query-architecture.md), отвечающий за обработку текста в строках запросов и индексированных документах. Различные анализаторы по-разному обрабатывают текст в зависимости от сценария. Языковые анализаторы обрабатывают текст с помощью лингвистических правил, чтобы улучшить качество поиска, в то время как другие анализаторы выполняют более простые задачи, такие как преобразование символов в нижний регистр. 

Чаще всего используются языковые анализаторы, и каждому поддерживающему поиск полю в индексе службы "Поиск Azure" назначается языковой анализатор по умолчанию. Во время анализа текста обычно выполняются следующие языковые преобразования:

+ удаляются слова, которые не представляют смысла (стоп-слова), и пунктуация;
+ фразы и слова, которые пишутся через дефис, разбиваются на составные части;
+ для слов с заглавными буквами используются строчные буквы.
+ слова редуцируются до корня для поиска соответствий независимо от временных форм.

Языковые анализаторы преобразовывают входной текст в примитивные или начальные формы, которые эффективны для хранилищ информации и для извлечения файлов. Преобразование происходит во время индексирования, когда создается индекс, а затем снова во время поиска, когда выполняется считывание индекса. Чтобы получить ожидаемые результаты поиска, используйте один и тот же анализатор для обоих операций.

## <a name="default-analyzer"></a>Анализатор по умолчанию  

По умолчанию служба "Поиск Azure" использует [стандартный анализатор Apache Lucene Standard](https://lucene.apache.org/core/4_10_3/analyzers-common/org/apache/lucene/analysis/standard/StandardAnalyzer.html), который разбивает текст на элементы согласно правилам [сегментации текста Юникода](https://unicode.org/reports/tr29/). Кроме того, стандартный анализатор преобразует все знаки в нижний регистр. Анализу подвергаются как индексируемые документы, так и условия поиска во время индексирования и обработки запросов.  

Анализатор применяется автоматически для каждого поддерживающего поиск поля. Можно переопределить значение по умолчанию для каждого поля. Альтернативными анализаторами могут быть [языковые анализаторы](index-add-language-analyzers.md), [пользовательские анализаторы](index-add-custom-analyzers.md) или стандартные анализаторы, приведенные в [списке доступных анализаторов](index-add-custom-analyzers.md#AnalyzerTable).


## <a name="types-of-analyzers"></a>Типы анализаторов

В списке ниже описаны анализаторы, доступные в службе "Поиск Azure".

| Категория | Описание |
|----------|-------------|
| [Стандартный анализатор Lucene](https://lucene.apache.org/core/4_0_0/analyzers-common/org/apache/lucene/analysis/standard/StandardAnalyzer.html) | По умолчанию. Не требуется спецификация или настройка. Этот универсальный анализатор подходит для большинства языков и сценариев.|
| Стандартные анализаторы | Представлены в виде готового продукта для использования "как есть". <br/>Есть два типа: специализированные и зависящие от языка. Они являются стандартными, так как ссылаться на них можно по имени без дополнительной конфигурации. <br/><br/>[Специализированные анализаторы (независящие от языка)](index-add-custom-analyzers.md#AnalyzerTable) используются для текстовых входных данных, требующих специализированной или минимальной обработки. К стандартным анализаторам, независящим от языка, относятся **Asciifolding**, **Keyword**, **Pattern**, **Simple**, **Stop**, **Whitespace**.<br/><br/>[Анализаторы языка](index-add-language-analyzers.md) используются, когда требуется расширенная лингвистическая поддержка отдельных языков. Поиск Azure поддерживает 35 анализаторов языка Lucene и 50 анализаторов обработки естественного языка Майкрософт. |
|[Пользовательские анализаторы](https://docs.microsoft.com/rest/api/searchservice/Custom-analyzers-in-Azure-Search) | Относится к определенной пользователем конфигурации сочетания имеющихся элементов, которая состоит из одного лексического анализатора (обязательно) и необязательных фильтров (char или маркеров).|

Несколько стандартных анализаторов, таких как **Pattern** или **Stop**, поддерживают ограниченный набор параметров. Чтобы задать эти параметры, необходимо создать пользовательский анализатор, состоящий из стандартного анализатора и одного из альтернативных вариантов, описанных в разделе [Справочные данные по стандартным анализаторам](index-add-custom-analyzers.md#AnalyzerTable). Как и в случае любой другой пользовательской настройки, присвойте новой конфигурации имя, например *myPatternAnalyzer*, чтобы отличать ее от анализатора Pattern Lucene.

## <a name="how-to-specify-analyzers"></a>Как указать анализатор

1. (Только для пользовательских анализаторов.) Создайте раздел **analyzer** в определении индекса. Дополнительные сведения см. в статьях о [создании индекса](https://docs.microsoft.com/rest/api/searchservice/create-index) и [пользовательских анализаторов](index-add-custom-analyzers.md).

2. В [определении поля](https://docs.microsoft.com/rest/api/searchservice/create-index) в индексе задайте для свойства **analyzer** поля имя целевого анализатора (например, `"analyzer" = "keyword"`). Допустимые значения: стандартный анализатор, анализатор языка или пользовательский анализатор, также определенный в схеме индекса. Запланируйте назначение анализатора на этапе определения индекса, прежде чем создавать индекс в службе.

3. Кроме того, вместо одного свойства **analyzer** вы можете задать различные анализаторы для индексирования и создания запросов, используя параметры полей **indexAnalyzer** и **searchAnalyzer**. Для подготовки и извлечения данных можно использовать различные анализаторы, если одно из этих действий требует определенного преобразования, которое не требуется другому.

Назначение свойств **analyzer** или **indexAnalyzer** полю, которое уже физически создано, запрещено. Если что-то из этого вам не понятно, просмотрите приведенную ниже таблицу, чтобы определить, какие действия следует пересмотреть и почему.
 
 | Сценарий | Влияние | Действия |
 |----------|--------|-------|
 | Добавление нового поля | минимальный | Если поле еще не существует в схеме, то редакция поля не требуется, так как поле еще физически не присутствует в вашем индексе. Вы можете использовать запрос [Update Index](https://docs.microsoft.com/rest/api/searchservice/update-index), чтобы добавить новое поле к имеющемуся индексу, запрос [mergeOrUpload](https://docs.microsoft.com/rest/api/searchservice/addupdate-or-delete-documents), чтобы заполнить его.|
 | Добавьте атрибуты **analyzer** или **indexAnalyzer** в имеющееся индексированное поле. | [перестроение](search-howto-reindex.md) | Инвертированный индекс для этого поля нужно воссоздать с нуля, а содержимое для этих полей нужно повторно проиндексировать. <br/> <br/>Для разрабатываемых индексов [удалите](https://docs.microsoft.com/rest/api/searchservice/delete-index) и [создайте](https://docs.microsoft.com/rest/api/searchservice/create-index) индекс, чтобы выбрать новое определение поля. <br/> <br/>Для индексов в рабочей среде перестроение можно отложить, создав другое поле, чтобы указать измененное определение и начать использовать его. Для внедрения нового поля используется запрос [Update Index](https://docs.microsoft.com/rest/api/searchservice/update-index), а запрос [mergeOrUpload](https://docs.microsoft.com/rest/api/searchservice/addupdate-or-delete-documents) позволяет заполнить его. Позже в рамках планового обслуживания индекса можно очистить его, чтобы удалить устаревшие поля. |

## <a name="when-to-add-analyzers"></a>Добавление анализаторов

Добавлять и назначать анализаторы лучше всего во время активной разработки, когда часто выполняется удаление и воссоздание индексов.

По мере того, как закрепляется определение индекса, вы можете добавлять новые конструкции анализа в индекс, но вам нужно будет передать флаг **allowIndexDowntime** в запрос [Update Index](https://docs.microsoft.com/rest/api/searchservice/update-index), если вы хотите избежать следующей ошибки:

*"Обновление индекса не разрешено, поскольку это может привести к простою. Чтобы добавить новые анализаторы, создатели маркеров, фильтры маркеров или символьные фильтры к имеющемуся индексу, в запросе на обновление индекса установите значение true для параметра запроса allowIndexDowntime. Обратите внимание, что эта операция переведет индекс в автономный режим на несколько секунд, что приведет к сбоям при выполнении индексирования и запросов. После обновления индекса его производительность и доступность для записи может быть снижена на несколько минут или дольше, если индекс очень большой."*

То же самое верно и при назначении анализатора полю. Анализатор является неотъемлемой частью определения поля, поэтому его можно добавить только при создании поля. Если вы хотите добавить анализаторы к имеющимся полям, вам придется [удалить и повторно создать](search-howto-reindex.md) индекс или добавить новое поле с необходимым анализатором.

Как уже отмечалось, исключением является вариант **searchAnalyzer**. Из трех способов указания анализаторов (**analyzer**, **indexAnalyzer**, **searchAnalyzer**) в имеющемся поле можно изменить только атрибут **searchAnalyzer**.

## <a name="recommendations-for-working-with-analyzers"></a>Рекомендации по работе с анализаторами

В этом разделе даны рекомендации по работе с анализаторами.

### <a name="one-analyzer-for-read-write-unless-you-have-specific-requirements"></a>Использование одного анализатора для чтения и записи при отсутствии особых требований

Служба "Поиск Azure" позволяет указать различные анализаторы для индексирования и поиска с помощью дополнительных параметров полей — **indexAnalyzer** и **searchAnalyzer**. Если их не указать, анализатор с заданным свойством **analyzer** будет использоваться для двух операций — индексирования и поиска. Если не указать `analyzer`, будет использоваться стандартный анализатор Lucene.

Мы рекомендуем использовать один анализатор для индексации и запросов при отсутствии специальных требований. Обязательно тщательно протестируйте его. Когда обработка текста отличается по времени поиска и индексирования, появляется риск несовпадения терминов запроса и индексирования, если конфигурации анализаторов поиска и индексации не согласованы.

### <a name="test-during-active-development"></a>Тестирование во время активной разработки

Переопределение стандартного анализатора требует перестроения индекса. Если это возможно, перед обработкой индекса в рабочей среде решите, для какого анализатора его использовать во время активной разработки.

### <a name="inspect-tokenized-terms"></a>Изучение терминов, снабженных маркером

Если поиск не возвращает ожидаемые результаты, скорее всего маркер не соответствует условиям поиска терминов в запросе и терминам, снабженным маркером в индексе. Если маркеры не одинаковые, совпадения не материализуются. Чтобы изучить выходные данные создателя маркеров, мы рекомендуем использовать [API анализа](https://docs.microsoft.com/rest/api/searchservice/test-analyzer) в качестве инструмента исследования. Ответ содержит маркеры, которые создаются определенным анализатором.

<a name="examples"></a>

## <a name="rest-examples"></a>Примеры использования REST

В примерах ниже приведены определения анализатора для нескольких ключевых сценариев.

+ [Пример пользовательского анализатора](#Custom-analyzer-example)
+ [Назначение анализаторов примеру поля](#Per-field-analyzer-assignment-example)
+ [Совмещение анализаторов для операций индексирования и поиска](#Mixing-analyzers-for-indexing-and-search-operations)
+ [Пример анализатора языка](#Language-analyzer-example)

<a name="Custom-analyzer-example"></a>

### <a name="custom-analyzer-example"></a>Пример пользовательского анализатора

В этом примере демонстрируется определение анализатора с настраиваемыми параметрами. Настраиваемые параметры для фильтров типа char, лексических анализаторов и фильтров маркеров указываются отдельно как именованные конструкции, а затем в определении анализатора на них приводятся ссылки. Стандартные элементы используются "как есть" и сослаться на них можно с помощью имени.

Вот описание этого примера:

* Анализаторы являются свойством класса поля для поля, поддерживающего поиск.
* Пользовательский анализатор входит в определение индекса. Он может быть незначительно настроенным (например, настройка одного параметра в одном фильтре) или настроенным в нескольких местах.
* В таком случае пользовательский анализатор "my_analyzer" в свою очередь использует настроенный стандартный лексический анализатор "my_standard_tokenizer" и два фильтра маркеров: с нижним регистром и настроенный фильтр типа asciifolding — "my_asciifolding".
* Он также определяет 2 пользовательских фильтра char: "map_dash" и "remove_whitespace". Первый заменяет все тире символом подчеркивания, в то время как второй удаляет все пробелы. В соответствии с правилами сопоставления, все пробелы должны быть закодированы в формате UTF-8. Фильтры символов применяются перед разметкой и будут влиять на полученные маркеры (стандартная разметка делится на тире и пробелы, но не на подчеркивания).

~~~~
  {
     "name":"myindex",
     "fields":[
        {
           "name":"id",
           "type":"Edm.String",
           "key":true,
           "searchable":false
        },
        {
           "name":"text",
           "type":"Edm.String",
           "searchable":true,
           "analyzer":"my_analyzer"
        }
     ],
     "analyzers":[
        {
           "name":"my_analyzer",
           "@odata.type":"#Microsoft.Azure.Search.CustomAnalyzer",
           "charFilters":[
              "map_dash",
              "remove_whitespace"
           ],
           "tokenizer":"my_standard_tokenizer",
           "tokenFilters":[
              "my_asciifolding",
              "lowercase"
           ]
        }
     ],
     "charFilters":[
        {
           "name":"map_dash",
           "@odata.type":"#Microsoft.Azure.Search.MappingCharFilter",
           "mappings":["-=>_"]
        },
        {
           "name":"remove_whitespace",
           "@odata.type":"#Microsoft.Azure.Search.MappingCharFilter",
           "mappings":["\\u0020=>"]
        }
     ],
     "tokenizers":[
        {
           "name":"my_standard_tokenizer",
           "@odata.type":"#Microsoft.Azure.Search.StandardTokenizerV2",
           "maxTokenLength":20
        }
     ],
     "tokenFilters":[
        {
           "name":"my_asciifolding",
           "@odata.type":"#Microsoft.Azure.Search.AsciiFoldingTokenFilter",
           "preserveOriginal":true
        }
     ]
  }
~~~~

<a name="Per-field-analyzer-assignment-example"></a>

### <a name="per-field-analyzer-assignment-example"></a>Пример назначения анализатора для каждого поля

Стандартный анализатор является анализатором по умолчанию. Предположим, что необходимо заменить анализатор по умолчанию другим стандартным анализатором, например анализатором шаблона. Если не нужно задавать настраиваемые параметры, вам достаточно указать его имя в определении поля.

Элемент "analyzer" переопределяет стандартный анализатор для каждого поля. Масштабное переопределение отсутствует. В этом примере `text1` использует анализатор шаблона, а значение `text2`, которое не указывает анализатор, использует значение по умолчанию.

~~~~
  {
     "name":"myindex",
     "fields":[
        {
           "name":"id",
           "type":"Edm.String",
           "key":true,
           "searchable":false
        },
        {
           "name":"text1",
           "type":"Edm.String",
           "searchable":true,
           "analyzer":"pattern"
        },
        {
           "name":"text2",
           "type":"Edm.String",
           "searchable":true
        }
     ]
  }
~~~~

<a name="Mixing-analyzers-for-indexing-and-search-operations"></a>

### <a name="mixing-analyzers-for-indexing-and-search-operations"></a>Совмещение анализаторов для индексирования и поиска

API включает в себя дополнительные атрибуты индекса для указания других анализаторов для индексирования и поиска. Атрибуты **searchAnalyzer** и **indexAnalyzer** нужно указывать в сочетании, заменив отдельный атрибут **analyzer**.


~~~~
  {
     "name":"myindex",
     "fields":[
        {
           "name":"id",
           "type":"Edm.String",
           "key":true,
           "searchable":false
        },
        {
           "name":"text",
           "type":"Edm.String",
           "searchable":true,
           "indexAnalyzer":"whitespace",
           "searchAnalyzer":"simple"
        },
     ],
  }
~~~~

<a name="Language-analyzer-example"></a>

### <a name="language-analyzer-example"></a>Пример анализатора языка

Поля, содержащие строки на разных языках, могут использовать анализатор языка, тогда как другие поля используют анализатор по умолчанию (или используют некоторые другие стандартные или настраиваемые анализаторы). При использовании анализатора языка он должен применяться и для индексирования, и для поиска. Поля, для которых используется анализатор языка, не могут иметь разные анализаторы для индексирования и поиска.

~~~~
  {
     "name":"myindex",
     "fields":[
        {
           "name":"id",
           "type":"Edm.String",
           "key":true,
           "searchable":false
        },
        {
           "name":"text",
           "type":"Edm.String",
           "searchable":true,
           "indexAnalyzer":"whitespace",
           "searchAnalyzer":"simple"
        },
        {
           "name":"text_fr",
           "type":"Edm.String",
           "searchable":true,
           "analyzer":"fr.lucene"
        }
     ],
  }
~~~~

## <a name="c-examples"></a>C#Примеры

Если вы используете примеры кода пакета SDK для .NET, можно добавить эти примеры, чтобы использовать или настраивать анализаторы.

+ [Назначить это встроенный анализатор](#Assign-a-language-analyzer)
+ [Настройка анализатора](#Define-a-custom-analyzer)

<a name="Assign-a-language-analyzer"></a>

### <a name="assign-a-language-analyzer"></a>Назначить анализатор языка

Любой анализатор, который используется в качестве-, без дополнительной настройки, указанные в определении поля. Нет необходимости для создания конструкцией анализатора. 

В этом примере назначается описание поля анализаторы Microsoft английский и французский языки. Это фрагмент кода взят из большего размера определение индекса отелей, создание с помощью класса Hotel в файле hotels.cs [DotNetHowTo](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetHowTo) образца.

Вызовите [анализатор](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.analyzer?view=azure-dotnet), указав [AnalyzerName](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.analyzername?view=azure-dotnet) типом, предоставляющим анализатор текста, поддерживаемые в службе поиска Azure.

```csharp
    public partial class Hotel
    {
       . . . 

        [IsSearchable]
        [Analyzer(AnalyzerName.AsString.EnMicrosoft)]
        [JsonProperty("description")]
        public string Description { get; set; }

        [IsSearchable]
        [Analyzer(AnalyzerName.AsString.FrLucene)]
        [JsonProperty("description_fr")]
        public string DescriptionFr { get; set; }

      . . .
    }
```
<a name="Define-a-custom-analyzer"></a>

### <a name="define-a-custom-analyzer"></a>Определить пользовательский анализатор

При необходимости настройки или конфигурации необходимо добавить конструкцией анализатора индекса. После определения, можно добавить его в определение field, как показано в предыдущем примере.

Создание [CustomAnalyzer](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.customanalyzer?view=azure-dotnet) объекта. Дополнительные примеры см. в разделе [CustomAnalyzerTests.cs](https://github.com/Azure/azure-sdk-for-net/blob/master/src/SDKs/Search/DataPlane/Search.Tests/Tests/CustomAnalyzerTests.cs).

```csharp
{
   var definition = new Index()
   {
         Name = "hotels",
         Fields = FieldBuilder.BuildForType<Hotel>(),
         Analyzers = new[]
            {
               new CustomAnalyzer()
               {
                     Name = "url-analyze",
                     Tokenizer = TokenizerName.UaxUrlEmail,
                     TokenFilters = new[] { TokenFilterName.Lowercase }
               }
            },
   };

   serviceClient.Indexes.Create(definition);
```

## <a name="next-steps"></a>Дальнейшие действия

+ Ознакомьтесь с комплексным обзором [работы полнотекстового поиска в поиске Azure](search-lucene-query-architecture.md). В этой статье используются примеры для пояснения режимов, являющихся, на первый взгляд, нелогичными.

+ Попробуйте использовать другой синтаксис запросов на основе примеров в статьях о [поиске документов](https://docs.microsoft.com/rest/api/searchservice/search-documents#bkmk_examples) или [простом синтаксисе запросов](query-simple-syntax.md) в проводнике поиска на портале.

+ Узнайте, как применить [лексические анализаторы для конкретного языка](index-add-language-analyzers.md).

+ [Настройте пользовательские анализаторы](index-add-custom-analyzers.md) для минимальной или специализированной обработки определенных полей.

## <a name="see-also"></a>См. также

 [Search Documents (Azure Search Service REST API)](https://docs.microsoft.com/rest/api/searchservice/search-documents) (Поиск по документам (REST API службы поиска Azure)) 

 [Простой синтаксис запросов](query-simple-syntax.md) 

 [Lucene query syntax in Azure Search](query-lucene-syntax.md) (Синтаксис запросов Lucene в службе поиска Azure) 
 
 [Обработка результатов поиска](search-pagination-page-layout.md)

<!--Image references-->
[1]: ./media/search-lucene-query-architecture/architecture-diagram2.png
