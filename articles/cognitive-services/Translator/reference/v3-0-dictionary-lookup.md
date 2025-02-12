---
title: Метод поиска по словарю в API перевода текстов
titleSuffix: Azure Cognitive Services
description: Использование метода поиска по словарю в API перевода текстов.
services: cognitive-services
author: swmachan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: reference
ms.date: 03/29/2018
ms.author: swmachan
ms.openlocfilehash: 9a06c8e3b50c3f54971694f8d3924a3a5ba5f071
ms.sourcegitcommit: fe6b91c5f287078e4b4c7356e0fa597e78361abe
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/29/2019
ms.locfileid: "68595038"
---
# <a name="translator-text-api-30-dictionary-lookup"></a>API перевода текстов 3.0: Поиск по словарю

Предоставляет альтернативные переводы слова и небольших идиоматических выражений. Каждый перевод содержит части речи и список обратных переводов. Обратные переводы позволяют пользователю понять перевод в контексте. Операция [Dictionary Example](./v3-0-dictionary-examples.md) позволяет продолжить детализацию, чтобы увидеть пример использования каждой пары переводов.

## <a name="request-url"></a>URL-адрес запроса

Отправьте запрос `POST` на следующий адрес.

```HTTP
https://api.cognitive.microsofttranslator.com/dictionary/lookup?api-version=3.0
```

## <a name="request-parameters"></a>Параметры запроса

В таблице ниже приведены параметры, которые передаются в строке запроса.

<table width="100%">
  <th width="20%">Параметр запроса</th>
  <th>Описание</th>
  <tr>
    <td>api-version</td>
    <td>*Обязательный параметр.*<br/>Версия API, запрошенная клиентом. Необходимое значение: `3.0`.</td>
  </tr>
  <tr>
    <td>from</td>
    <td>*Обязательный параметр.*<br/>Определяет язык вводимого текста. Исходный язык должен быть одним из [поддерживаемых языков](./v3-0-languages.md), включенных в область `dictionary`.</td>
  </tr>
  <tr>
    <td>to</td>
    <td>*Обязательный параметр.*<br/>Определяет язык выходного текста. Целевой язык должен быть одним из [поддерживаемых языков](./v3-0-languages.md), включенных в область `dictionary`.</td>
  </tr>
</table>

Заголовки запроса:

<table width="100%">
  <th width="20%">Заголовки</th>
  <th>Описание</th>
  <tr>
    <td>Заголовки проверки подлинности</td>
    <td><em>Обязательный заголовок запроса</em>.<br/>См. <a href="https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-reference#authentication">описание доступных способов аутентификации</a>.</td>
  </tr>
  <tr>
    <td>Content-Type</td>
    <td>*Обязательный заголовок запроса*.<br/>Указывает тип содержимого для полезных данных. Возможные значения: `application/json`.</td>
  </tr>
  <tr>
    <td>Content-Length</td>
    <td>*Обязательный заголовок запроса*.<br/>Длина текста запроса.</td>
  </tr>
  <tr>
    <td>X-ClientTraceId</td>
    <td>*Необязательный*.<br/>Созданный клиентом идентификатор GUID, позволяющий уникально идентифицировать запрос. Этот заголовок можно опустить, если в строке запроса указан идентификатор трассировки в параметре с именем `ClientTraceId`.</td>
  </tr>
</table> 

## <a name="request-body"></a>Текст запроса

Текст запроса является массивом в формате JSON. Каждый элемент этого массива представляет собой объект JSON со строковым свойством `Text`, который соответствует термину для поиска.

```json
[
    {"Text":"fly"}
]
```

Действительны следующие ограничения.

* Массив может содержать не более 10 элементов.
* Длина текстового значения в одном элементе массива не может превышать 100 символов, включая пробелы.

## <a name="response-body"></a>Тело ответа

Успешный ответ возвращается в формате массива JSON с одним результатом для каждой строки входного массива. Объект результата содержит следующие свойства.

  * `normalizedSource`. строка, задающая нормализованную форму термина в оригинале. Например если запросом является "JOHN", нормализованной формой будет "john". Содержимое этого поля становится входными данными для [примеров поиска](./v3-0-dictionary-examples.md).
    
  * `displaySource`. строка, задающая исходный термин в форме, наиболее подходящей для отображения конечному пользователю. Например, если входное значение — "JOHN", форма просмотра будет отражать обычное правописание имени: "John". 

  * `translations`. список переводов для исходного термина. Каждый элемент этого списка — это объект со следующими свойствами:

    * `normalizedTarget`. строка, задающая нормализованную форму этого термина в целевом языке. Это значение должно использоваться как входные данные для [примеров поиска](./v3-0-dictionary-examples.md).

    * `displayTarget`. строка, задающая термин в целевом языке в форме, наиболее подходящей для отображения конечному пользователю. Как правило, это будет отличаться от `normalizedTarget` с учетом регистра букв. Например, имя собственное, такое как "Juan" будет иметь `normalizedTarget = "juan"` и `displayTarget = "Juan"`.

    * `posTag`. строка, связывающая этот термин с тегом части речи.

        | Имя тега | Описание  |
        |----------|--------------|
        | ADJ      | Прилагательные   |
        | ADV      | Наречия      |
        | CONJ     | Союзы |
        | DET      | Определители  |
        | MODAL    | Команды        |
        | NOUN     | Существительные        |
        | PREP     | Предлоги |
        | PRON     | Местоимения     |
        | VERB     | Команды        |
        | OTHER    | Другой        |

        В качестве примечания о внедрении эти теги были определены с помощью тегов частей речи, помеченных английской стороной, а затем выполнялся наиболее частый тег для каждой исходной или целевой пары. Поэтому, если пользователи часто переводят испанское слово на английский язык с тегом другой части речи, теги могут оказаться неправильными (в отношении испанского слова).

    * `confidence`. значение в диапазоне от 0,0 до 1,0, которое представляет "достоверность" (или, точнее, "вероятность в данных обучения") этой пары переводов. Сумма оценок достоверности для одного исходного слова может равняться или не равняться 1,0. 

    * `prefixWord`. строка, предоставляющая слово для отображения в качестве префикса перевода. В настоящее время это гендерный определитель существительных на языках с гендерными детерминантами. Например, префиксом испанского слова "mosca" является "la", поскольку "mosca" на испанском языке — это женское имя существительное. Это зависит только от перевода, а не от источника. Если префикс отсутствует, это будет пустая строка.
    
    * `backTranslations`. список обратных переводов переведенного текста. Например, исходные слова, на которые может быть переведен целевой объект. Список, как правило, содержит искомое слово, которое было запрошено (например, если искомое слово будет "fly", тогда гарантируется, что "fly" будет в списке `backTranslations`). Однако оно не обязательно будет находиться на первом месте. Каждый элемент списка `backTranslations` — это объект, описанный следующими свойствами:

        * `normalizedText`. строка, задающая нормализованную форму исходного термина, который представляет собой обратный перевод переведенного текста. Это значение должно использоваться как входные данные для [примеров поиска](./v3-0-dictionary-examples.md).        

        * `displayText`. строка, задающая исходный термин, который представляет собой обратный перевод переведенного текста в форме, наиболее подходящей для отображения конечному пользователю.

        * `numExamples`. целое число, представляющее собой количество доступных примеров для данной пары переводов. Фактические примеры должны быть получены с помощью отдельного вызова [примеров поиска](./v3-0-dictionary-examples.md). Номер предназначен главным образом для облегчения отображения в UX. Например, пользовательский интерфейс может добавить гиперссылку на обратный перевод, если число примеров больше нуля, и показать обратный перевод как обычный текст, если примеров нет. Обратите внимание, что фактическое количество примеров, возвращаемых вызовом [примеров поиска](./v3-0-dictionary-examples.md), может быть меньше `numExamples`, так как дополнительная фильтрация может применяться на лету для удаления плохих примеров.
        
        * `frequencyCount`. целое число, представляющее частоту этой пары переводов в данных. Основное назначение этого поля — предоставить пользовательский интерфейс со средствами сортировки обратных переводов, поэтому наиболее частыми являются первые условия.

    > [!NOTE]
    > Если в словаре нет искомого слова, ответ будет равен 200 (ОК), а список `translations` будет пустым.

## <a name="examples"></a>Примеры

В этом примере показано, как искать альтернативные переводы английского термина на испанском языке `fly`.

# <a name="curltabcurl"></a>[curl](#tab/curl)

```
curl -X POST "https://api.cognitive.microsofttranslator.com/dictionary/lookup?api-version=3.0&from=en&to=es" -H "Ocp-Apim-Subscription-Key: <client-secret>" -H "Content-Type: application/json" -d "[{'Text':'fly'}]"
```

---

Текст ответа (сокращен для ясности):

```
[
    {
        "normalizedSource":"fly",
        "displaySource":"fly",
        "translations":[
            {
                "normalizedTarget":"volar",
                "displayTarget":"volar",
                "posTag":"VERB",
                "confidence":0.4081,
                "prefixWord":"",
                "backTranslations":[
                    {"normalizedText":"fly","displayText":"fly","numExamples":15,"frequencyCount":4637},
                    {"normalizedText":"flying","displayText":"flying","numExamples":15,"frequencyCount":1365},
                    {"normalizedText":"blow","displayText":"blow","numExamples":15,"frequencyCount":503},
                    {"normalizedText":"flight","displayText":"flight","numExamples":15,"frequencyCount":135}
                ]
            },
            {
                "normalizedTarget":"mosca",
                "displayTarget":"mosca",
                "posTag":"NOUN",
                "confidence":0.2668,
                "prefixWord":"",
                "backTranslations":[
                    {"normalizedText":"fly","displayText":"fly","numExamples":15,"frequencyCount":1697},
                    {"normalizedText":"flyweight","displayText":"flyweight","numExamples":0,"frequencyCount":48},
                    {"normalizedText":"flies","displayText":"flies","numExamples":9,"frequencyCount":34}
                ]
            },
            //
            // ...list abbreviated for documentation clarity
            //
        ]
    }
]
```

В этом примере показано, что происходит, когда искомый термин не существует для действительной пары словаря.

# <a name="curltabcurl"></a>[curl](#tab/curl)

```
curl -X POST "https://api.cognitive.microsofttranslator.com/dictionary/lookup?api-version=3.0&from=en&to=es" -H "X-ClientTraceId: 875030C7-5380-40B8-8A03-63DACCF69C11" -H "Ocp-Apim-Subscription-Key: <client-secret>" -H "Content-Type: application/json" -d "[{'Text':'fly123456'}]"
```

---

Так как этот термин не найден в словаре, текст ответа включает пустой список `translations`.

```
[
    {
        "normalizedSource":"fly123456",
        "displaySource":"fly123456",
        "translations":[]
    }
]
```
