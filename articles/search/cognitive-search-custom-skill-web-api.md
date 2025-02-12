---
title: Пользовательский навык когнитивного поиска — служба "Поиск Azure"
description: Расширьте возможности наборов навыков когнитивного поиска путем вызова веб-API
services: search
manager: pablocas
author: luiscabrer
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: conceptual
ms.date: 05/02/2019
ms.author: luisca
ms.custom: seojan2018
ms.openlocfilehash: e1ca8a5ce7b615ed8d84c91d8a0d72098c175c44
ms.sourcegitcommit: 2e4b99023ecaf2ea3d6d3604da068d04682a8c2d
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/09/2019
ms.locfileid: "67672131"
---
# <a name="custom-web-api-skill"></a>Пользовательский навык веб-API

**Настраиваемого веб-API** навыков позволяет расширить когнитивного поиска путем вызова конечной точки веб-API, обеспечивающий выполнение пользовательских операций. Аналогично встроенным навыкам **пользовательский навык веб-API** имеет входные и выходные данные. В зависимости от входных данных, веб-API получает полезные данные JSON, при выполнении индексатора и выводит полезные данные JSON в качестве ответа, а также код состояния успеха. В ответе должны содержаться выходные данные, указанные в вашем пользовательском навыке. Любой другой ответ считается ошибкой, и никакие обогащения не выполняются.

Структура полезных данных JSON описана далее в этом документе.

> [!NOTE]
> Индексатор будет дважды повторять попытки для определенных стандартных кодов состояния HTTP, возвращаемых из веб-API. Это такие коды состояния HTTP: 
> * `503 Service Unavailable`
> * `429 Too Many Requests`

## <a name="odatatype"></a>@odata.type  
Microsoft.Skills.Custom.WebApiSkill

## <a name="skill-parameters"></a>Параметры навыков

Параметры зависят от регистра.

| Имя параметра     | Описание |
|--------------------|-------------|
| uri | URI веб-API, к которому _JSON_ полезные данные будут отправляться. Допускается только схема URI **HTTPS**. |
| httpMethod | Метод, используемый при отправке полезных данных. Допустимые методы: `PUT` или `POST`. |
| httpHeaders | Коллекция пар "ключ — значение", в которой ключи представляют имена заголовков, а значения — значения заголовков, которые будут отправляться в веб-API вместе с полезными данными. В этой коллекции запрещены следующие заголовки: `Accept`, `Accept-Charset`, `Accept-Encoding`, `Content-Length`, `Content-Type`, `Cookie`, `Host`, `TE`, `Upgrade`, `Via`. |
| timeout | (Необязательно.) Если указано, означает время ожидания вызова API HTTP-клиента. Значение должно быть отформатировано как значение dayTimeDuration XSD (ограниченное подмножество значения [продолжительности ISO 8601](https://www.w3.org/TR/xmlschema11-2/#dayTimeDuration) ). Например, `PT60S` для 60 секунд. Если не задано, выбирается значение по умолчанию — 30 секунд. Время ожидания можно задать в диапазоне от 1 до 90 секунд. |
| batchSize | (Необязательно.) Указывает, сколько "записей данных" (см. в разделе о структуре полезных данных _JSON_ ниже) будет отправлено на один вызов API. В противном случае выбирается значение по умолчанию — 1000. Мы советуем вам использовать этот параметр для достижения подходящего баланса между пропускной способностью индексации и нагрузкой на API. |

## <a name="skill-inputs"></a>Входные данные навыков

Предварительно заданные входные данные для этого навыка отсутствуют. Вы можете выбрать одно или несколько полей, которые уже будут доступны во время выполнения этого навыка в качестве входных данных. Полезные данные _JSON_, отправляемые в веб-API, будут иметь разные поля.

## <a name="skill-outputs"></a>Выходные данные навыка

Предварительно заданные выходные данные для этого навыка отсутствуют. В зависимости от ответа, который вернет веб-API, добавьте поля вывода, чтобы их можно было получить из ответа _JSON_.


## <a name="sample-definition"></a>Пример определения

```json
  {
        "@odata.type": "#Microsoft.Skills.Custom.WebApiSkill",
        "description": "A custom skill that can count the number of words or characters or lines in text",
        "uri": "https://contoso.count-things.com",
        "batchSize": 4,
        "context": "/document",
        "inputs": [
          {
            "name": "text",
            "source": "/document/content"
          },
          {
            "name": "language",
            "source": "/document/languageCode"
          },
          {
            "name": "countOf",
            "source": "/document/propertyToCount"
          }
        ],
        "outputs": [
          {
            "name": "count",
            "targetName": "countOfThings"
          }
        ]
      }
```
## <a name="sample-input-json-structure"></a>Пример структуры входных данных JSON

Эта структура _JSON_ представляет полезные данные, которые будут отправляться в веб-API.
Она всегда будет соответствовать этим ограничениям:

* Сущность верхнего уровня называется `values` и будет массивом объектов. Количество таких объектов не будет превышать `batchSize`.
* Каждый объект в массиве `values` будет иметь:
    * Свойство `recordId`, которое является **уникальной** строкой, используемой для идентификации этой записи.
    * Свойство `data`, которое является объектом _JSON_. Поля свойства `data` будут соответствовать именам, указанным в разделе `inputs` определения навыков. Значения этих полей будут взяты из источника (`source`) этих полей (например, из поля в документе или, возможно, из другого навыка).

```json
{
    "values": [
      {
        "recordId": "0",
        "data":
           {
             "text": "Este es un contrato en Inglés",
             "language": "es",
             "countOf": "words"
           }
      },
      {
        "recordId": "1",
        "data":
           {
             "text": "Hello world",
             "language": "en",
             "countOf": "characters"
           }
      },
      {
        "recordId": "2",
        "data":
           {
             "text": "Hello world \r\n Hi World",
             "language": "en",
             "countOf": "lines"
           }
      },
      {
        "recordId": "3",
        "data":
           {
             "text": "Test",
             "language": "es",
             "countOf": null
           }
      }
    ]
}
```

## <a name="sample-output-json-structure"></a>Пример структуры выходных данных JSON

«Выход» соответствует ответа, возвращенного на веб-API. Веб-API должен вернуть _JSON_ полезных данных (убедиться, просмотрев `Content-Type` заголовок ответа) и должны удовлетворять следующим ограничениям:

* Должна существовать сущность верхнего уровня, называемая `values`, которая должна быть массивом объектов.
* Количество объектов в массиве должен быть таким же, как количество объектов, отправляемые веб-API.
* Каждый объект должен иметь:
   * Свойство `recordId`.
   * Свойство `data`, являющееся объектом, в котором поля представляют собой обогащения, соответствующие именам в `output` (их значения считаются обогащением).
   * Свойство `errors` — массив, перечисляющий все обнаруженные ошибки, добавляемые в журнал выполнения индексатора. Это свойство является обязательным, но может иметь значение `null`.
   * Свойство `warnings` — массив, перечисляющий все обнаруженные предупреждения, добавляемые в журнал выполнения индексатора. Это свойство является обязательным, но может иметь значение `null`.
* Объекты в массиве `values` не обязательно должны находиться в том же порядке, что и объекты в массиве `values`, отправленные в виде запроса к веб-API. Тем не менее `recordId` используется для корреляции, поэтому любые записи в ответе с идентификатором `recordId`, который не запрашивался в исходном запросе к веб-API, будут отклонены.

```json
{
    "values": [
        {
            "recordId": "3",
            "data": {
            },
            "errors": [
              {
                "message" : "Cannot understand what needs to be counted"
              }
            ],
            "warnings": null
        },
        {
            "recordId": "2",
            "data": {
                "count": 2
            },
            "errors": null,
            "warnings": null
        },
        {
            "recordId": "0",
            "data": {
                "count": 6
            },
            "errors": null,
            "warnings": null
        },
        {
            "recordId": "1",
            "data": {
                "count": 11
            },
            "errors": null,
            "warnings": null
        },
    ]
}

```

## <a name="error-cases"></a>Варианты ошибок
В дополнение к недоступности веб-API или отправке неуспешных кодов состояния случаями ошибок также считаются следующие:

* Если веб-API возвращает код состояния успеха, но ответ указывает, что это не `application/json`, тогда ответ считается недействительным, и никакие обогащения не будут выполнены.
* При наличии **недопустимых** (если `recordId` не находится в исходном запросе или если значения дублируются) записей в массиве `values` ответа **для этих записей** обогащение выполняться не будет.

В тех случаях, когда веб-API недоступен или возвращает ошибку HTTP, в журнал выполнения индексатора будет добавлено понятное сообщение об ошибке с доступными сведениями об ошибке HTTP.

## <a name="see-also"></a>См. также

+ [Определение набора навыков](cognitive-search-defining-skillset.md)
+ [Добавление пользовательского навыка в конвейер когнитивного поиска](cognitive-search-custom-skill-interface.md)
+ [Пример. Создание пользовательских опыт для когнитивный поиск](cognitive-search-create-custom-skill-example.md)