---
title: Работа с типами содержимого — Azure Logic Apps
description: Узнайте, как служба Logic Apps обрабатывает типы содержимого во время разработки и при выполнении
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
ms.topic: conceptual
ms.date: 07/20/2018
ms.openlocfilehash: 97897da13c70c29834b1fc276829b316416efd8d
ms.sourcegitcommit: 10251d2a134c37c00f0ec10e0da4a3dffa436fb3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/13/2019
ms.locfileid: "67868910"
---
# <a name="handle-content-types-in-azure-logic-apps"></a>Обработка типов содержимого в Azure Logic Apps

Различные типы содержимого могут проходить через приложение логики, например JSON, XML, неструктурированные файлы и двоичные данные. Служба Logic Apps поддерживает все типы содержимого, некоторые приложения имеют встроенную поддержку и не требуют приведения или преобразования. Другие могут потребовать преобразования или приведения, по необходимости. В этой статье описывается, как служба Logic Apps обрабатывает типы содержимого и как правильно при необходимости приводить или преобразовывать эти типы.

Чтобы определить подходящий метод для обработки типов содержимого, служба Logic Apps полагается на заголовок значения `Content-Type` в вызовах HTTP.

* [application/json](#application-json) (собственный тип)
* [text/plain](#text-plain) (собственный тип)
* [application/xml and application/octet-stream](#application-xml-octet-stream)
* [Другие типы содержимого](#other-content-types)

<a name="application-json"></a>

## <a name="applicationjson"></a>приложение/json

Служба Logic Apps хранит и обрабатывает любой запрос с типом содержимого *application/json* как объект JavaScript Notation (JSON). По умолчанию можно проанализировать содержимое JSON без приведения. Чтобы проанализировать запрос, который имеет заголовок с типом содержимого "application/json", можно использовать выражение. Этот пример возвращает значение `dog` из массива `animal-type` без приведения. 
 
`@body('myAction')['animal-type'][0]` 
  
  ```json
  {
    "client": {
       "name": "Fido",
       "animal-type": [ "dog", "cat", "rabbit", "snake" ]
    }
  }
  ```

Если вы работаете с данными JSON, в которых не указан заголовок, их можно приводить вручную с помощью [функции json()](../logic-apps/workflow-definition-language-functions-reference.md#json). 
  
`@json(triggerBody())['animal-type']`

### <a name="create-tokens-for-json-properties"></a>Создание токенов для свойств JSON

Служба Logic Apps предоставляет возможность создания удобных пользовательских токенов, представляющих свойства в содержимом JSON, чтобы иметь возможность ссылаться и упрощать использование этих свойств в рабочем процессе приложений логики.

* **Триггер запросов**

  При использовании этого триггера в конструкторе приложений логики можно предоставить схему JSON, которая описывает полезные данные, которые рассчитывается получить. 
  С помощью этой схемы конструктор выполняет синтаксический анализ содержимого JSON и создает удобные пользовательские токены, представляющие свойства в содержимом JSON. 
  Можно легко ссылаться на эти свойства и использовать их во время рабочего процесса приложения логики. 
  
  Если нет схемы, ее можно создать. 
  
  1. В триггере запросов выберите **Использовать пример полезной нагрузки, чтобы создать схему**.  
  
  2. В разделе **Введение или вставка примера полезных данных JSON** предоставьте пример полезных данных, а затем выберите **Готово**. Пример: 

     ![Предоставление примера полезных данных JSON](./media/logic-apps-content-type/request-trigger.png)

     Созданная схема теперь отображается в триггере.

     ![Предоставление примера полезных данных JSON](./media/logic-apps-content-type/generated-schema.png)

     Ниже приведено базовое определение для триггера запросов в редакторе представления кода.

     ```json
     "triggers": { 
        "manual": {
           "type": "Request",
           "kind": "Http",
           "inputs": { 
              "schema": {
                 "type": "object",
                 "properties": {
                    "client": {
                       "type": "object",
                       "properties": {
                          "animal-type": {
                             "type": "array",
                             "items": {
                                "type": "string"
                             },
                          },
                          "name": {
                             "type": "string"
                          }
                       }
                    }
                 }
              }
           }
        }
     }
     ```

  3. В запросе не забудьте включить заголовок `Content-Type` и присвоить значение заголовка для `application/json`.

* **Анализ действия JSON**

  При использовании этого действия в конструкторе приложений логики вы можете проанализировать выходные данные JSON и создать удобные пользовательские токены, представляющие свойства в содержимом JSON. 
  Можно легко ссылаться на эти свойства и использовать их во время рабочего процесса приложения логики. Подобно триггеру запроса можно предоставить или создать схему, описывающую содержимое JSON, которое необходимо проанализировать. 
  Таким образом можно легко использовать данные из Служебной шины Azure, Azure Cosmos DB и другого.

  ![Parse JSON](./media/logic-apps-content-type/parse-json.png)

<a name="text-plain"></a>

## <a name="textplain"></a>text/plain

Когда приложение логики получает сообщения HTTP, имеющие заголовок `Content-Type` со значением `text/plain`, оно хранит эти сообщения в необработанном виде. Если включить эти сообщения в последующие действия без какого-либо приведения, запросы будут переданы с заголовком `Content-Type` со значением `text/plain`. 

Например, при работе с неструктурированным файлом, может появиться запрос HTTP с заголовком `Content-Type` со значением типа содержимого `text/plain`.

`Date,Name,Address`</br>
`Oct-1,Frank,123 Ave`

Если отправить этот запрос в позднем действии как тело другого запроса, например `@body('flatfile')`, этот второй запрос также имеет заголовок `Content-Type` со значением `text/plain`. Если вы работаете с данными, которые являются простым текстом, но не указали заголовок, можно вручную перевести эти данные в текст с помощью [функции string()](../logic-apps/workflow-definition-language-functions-reference.md#string), такой как в следующем выражении. 

`@string(triggerBody())`

<a name="application-xml-octet-stream"></a>

## <a name="applicationxml-and-applicationoctet-stream"></a>application/xml and application/octet-stream

Служба Logic Apps всегда сохраняет `Content-Type` в полученных запросе или ответе HTTP. Поэтому приложение логики получает содержимое с `Content-Type` установленным как `application/octet-stream` и вы добавляете это содержимое в следующее действие без приведения. Исходящий запрос будет иметь заголовок `Content-Type` со значением `application/octet-stream`. Таким образом служба Logic Apps может гарантировать, что данные не потеряются при прохождении через рабочий процесс. При этом состояние действия или входов и выходов хранится в объекте JSON, в то время как состояние проходит через рабочий процесс. 

## <a name="converter-functions"></a>Функции преобразователя

Для сохранения некоторых типов данных служба Logic Apps преобразует содержимое в двоичную строку в кодировке base64 с соответствующими метаданными, сохраняя полезные данные `$content` и `$content-type`, преобразование которых выполняется автоматически. 

Ниже приводится список преобразований службой Logic Apps содержимого при использовании этих [функций](../logic-apps/workflow-definition-language-functions-reference.md).

* `json()`. Приводит данные к`application/json`
* `xml()`. Приводит данные к`application/xml`
* `binary()`. Приводит данные к`application/octet-stream`
* `string()`. Приводит данные к`text/plain`
* `base64()`. Преобразует содержимое в строку в кодировке Base64
* `base64toString()`. Преобразует строку в кодировке Base64 в`text/plain`
* `base64toBinary()`. Преобразует строку в кодировке Base64 в`application/octet-stream`
* `dataUri()`. Преобразует строку в универсальный код ресурса (URI) данных
* `dataUriToBinary()`. Преобразует универсальный код ресурса (URI) данных в двоичную строку
* `dataUriToString()`. Преобразует универсальный код ресурса (URI) данных в строку

Например, при получении HTTP-запроса, где `Content-Type` установлено как `application/xml`, как в этом содержимом.

```html
<?xml version="1.0" encoding="UTF-8" ?>
<CustomerName>Frank</CustomerName>
```

Вы можете привести это содержимое с помощью выражения `@xml(triggerBody())` с функциями `xml()` и `triggerBody()`, а затем использовать это содержимое позднее. Можно также использовать выражение `@xpath(xml(triggerBody()), '/CustomerName')` с функциями `xpath()` и `xml()`. 

## <a name="other-content-types"></a>Другие типы содержимого

Служба Logic Apps работает и поддерживает другие типы содержимого, но может потребоваться получить тело сообщения вручную, расшифровав переменную `$content`.

Например, приложение логики получает инициируемый запрос с типом содержимого `application/x-www-url-formencoded`. Для сохранения всех данных переменная `$content` в теле запроса имеет полезные данные, которые кодируются в виде строки base64.

`CustomerName=Frank&Address=123+Avenue`

Так как запрос не имеет формат простого текста или JSON, он сохраняется в действии в следующем виде:

```json
"body": {
   "$content-type": "application/x-www-url-formencoded",
   "$content": "AAB1241BACDFA=="
}
```

Служба Logic Apps предоставляет собственные функции для обработки данных формы. Например следующие. 

* [triggerFormDataValue()](../logic-apps/workflow-definition-language-functions-reference.md#triggerFormDataValue)
* [triggerFormDataMultiValues()](../logic-apps/workflow-definition-language-functions-reference.md#triggerFormDataMultiValues)
* [formDataValue()](../logic-apps/workflow-definition-language-functions-reference.md#formDataValue) 
* [formDataMultiValues()](../logic-apps/workflow-definition-language-functions-reference.md#formDataMultiValues)

Или можно вручную сделать доступ к данным с помощью выражения следующего вида.

`@string(body('formdataAction'))` 

Если исходящему запросу необходимо иметь такой же тип заголовка содержимого `application/x-www-url-formencoded`, можно добавить запрос в тело действия используя выражение, как `@body('formdataAction')`. Однако этот метод работает, только если body является единственным параметром в `body` входа. Если попытаться использовать выражение `@body('formdataAction')` в запросе `application/json`, вы получите ошибку выполнения, так как тело будет отправлено в закодированном виде.
