---
title: Изучите выходные данные индексатора видео Azure Media Services API версии 2
titlesuffix: Azure Media Services
description: В этом разделе рассматриваются выходные данные Индексатора видео, полученные с помощью API версии 2.
services: media-services
author: Juliako
manager: femila
ms.service: media-services
ms.subservice: video-indexer
ms.topic: article
ms.date: 05/15/2019
ms.author: juliako
ms.openlocfilehash: 205dc7d9e69788ea29a48ff342844a4b74e143bd
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "65799082"
---
# <a name="examine-the-video-indexer-output-produced-by-api"></a>Изучите выходные данные индексатора видео API

Если при вызове API **Get Video Index** отображается состояние ответа "OK", вы получите подробные выходные данные в формате JSON в виде содержимого ответа. В содержимом JSON представлены данные по выбранным аналитическим сведениям из видео. Insights включает измерений, например: табель OCRs, лиц, разделы, блоков и т. д. Измерения содержат экземпляры диапазонов времени, которые указывают, когда каждое измерение появляется на видео.  

Также можно просмотреть сводные аналитические сведения о видео, нажав кнопку **воспроизведения** на видео, размещенном на веб-сайте [Индексатора видео](https://www.videoindexer.ai/). Дополнительные сведения см. в руководстве по [просмотру и анализу аналитических сведений в Индексаторе видео](video-indexer-view-edit.md).

![Аналитика](./media/video-indexer-output-json/video-indexer-summarized-insights.png)

В этой статье рассматривается содержимое JSON, которое возвращает API **Get Video Index**. 

> [!NOTE]
> Срок действия всех маркеров доступа в Индексаторе видео составляет один час.


## <a name="root-elements"></a>Корневые элементы

|Name|Описание|
|---|---|
|accountId|Идентификатор учетной записи Индексатора видео, содержащей список воспроизведения.|
|id|Идентификатор списка воспроизведения.|
|name|Имя списка воспроизведения.|
|description|Описание списка воспроизведения.|
|userName|Имя пользователя, создавшего список воспроизведения.|
|created|Время создания списка воспроизведения.|
|privacyMode|Режим конфиденциальности списка воспроизведения (закрытый или общедоступный).|
|состояние|Состояние списка воспроизведения (отправлен, обрабатывается, обработан, сбой, помещен в карантин).|
|isOwned|Указывает, был ли создан список воспроизведения текущим пользователем.|
|isEditable|Указывает, имеет ли текущий пользователь право изменять список воспроизведения.|
|isBase|Указывает, является ли список воспроизведения основным (одно видео) или составлен на основе других видео (производный).|
|durationInSeconds|Общая длительность списка воспроизведения.|
|summarizedInsights|Содержит один объект [summarizedInsights](#summarizedinsights).
|videos|Список [видео](#videos), которые составляют список воспроизведения.<br/>Если этот список воспроизведения содержит диапазоны времени других видео (производный), видео в этом списке будут содержать только данные из добавленных диапазонов времени.|

```json
{
  "accountId": "bca61527-1221-bca6-1527-a90000002000",
  "id": "abc3454321",
  "name": "My first video",
  "description": "I am trying VI",
  "userName": "Some name",
  "created": "2018/2/2 18:00:00.000",
  "privacyMode": "Private",
  "state": "Processed",
  "isOwned": true,
  "isEditable": false,
  "isBase": false,
  "durationInSeconds": 120, 
  "summarizedInsights" : { . . . }
  "videos": [{ . . . }]
}
```

## <a name="summarizedinsights"></a>summarizedInsights

В этом разделе приводится сводка аналитических сведений.

|Атрибут | Описание|
|---|---|
|name|Имя видео. Например, Azure Monitor.|
|id|Идентификатор видео. Например, 63c6d532ff.|
|privacyMode|Анализируемое видео может иметь один из следующих режимов: **Private**, **Public**. **Public** — видео доступно для всех пользователей в вашей учетной записи и любому пользователю, у которого есть ссылка на видео. **Private** — видео доступно для всех пользователей в вашей учетной записи.|
|длительность|Содержит данные об отрезке времени, в котором обнаружены аналитические сведения. Отрезок времени указан в секундах.|
|thumbnailVideoId|Идентификатор видео, из которого создан эскиз.
|thumbnailId|Идентификатор эскиза видео. Чтобы получить фактический эскиз, вызовите [Get-эскиз](https://api-portal.videoindexer.ai/docs/services/operations/operations/Get-Video-Thumbnail) и передать его thumbnailVideoId и thumbnailId.|
|faces|Может содержать или не содержать изображение лица. Дополнительные сведения см. в разделе [faces](#faces).|
|keywords|Может содержать или не содержать ключевые слова. Дополнительные сведения см. в разделе [keywords](#keywords).|
|sentiments|Может содержать или не содержать сведения о тональности. Дополнительные сведения см. в разделе [sentiments](#sentiments).|
|audioEffects| Может содержать или не содержать объекты audioEffects. Дополнительные сведения см. в разделе [audioEffects](#audioEffects).|
|метки;| Может содержать или не содержать метки. Дополнительные сведения см. в разделе [labels](#labels).|
|brands| Может содержать или не содержать сведения о торговых марках. Дополнительные сведения см. в разделе [brands](#brands).|
|statistics | Дополнительные сведения см. в разделе [statistics](#statistics).|
|emotions| Может содержать или не содержать значения эмоций. Дополнительные сведения см. в разделе [emotions](#emotions).|
|topics|Может содержать или не содержать значения тем. Измерение [topics](#topics).|

## <a name="videos"></a>videos

|Name|Описание|
|---|---|
|accountId|Идентификатор учетной записи Индексатора видео, содержащей видео.|
|id|Идентификатор видео.|
|name|Имя видео.
|состояние|Состояние видео (отправлено, обрабатывается, обработано, ошибка, помещено в карантин).|
|processingProgress|Ход выполнения обработки (например, 20 %).|
|failureCode|Код ошибки, если не удалось выполнить обработку (например, UnsupportedFileType).|
|failureMessage|Сообщение об ошибке, если не удалось выполнить обработку.|
|externalId|Внешний идентификатор видео (если указан пользователем).|
|externalUrl|Внешний URL-адрес видео (если указан пользователем).|
|метаданные|Внешние метаданные видео (если указаны пользователем).|
|isAdult|Указывает, определено ли видео как содержимое для взрослых при проверке вручную.|
|insights|Объект insights. Дополнительные сведения см. в разделе [insights](#insights).|
|thumbnailId|Идентификатор эскиза видео. Чтобы получить фактический вызов эскизов [Get-эскиз](https://api-portal.videoindexer.ai/docs/services/operations/operations/Get-Video-Thumbnail) и передать его идентификатор и thumbnailId видео.|
|publishedUrl|URL-адрес для потоковой передачи видео.|
|publishedUrlProxy|URL-адрес для потоковой передачи видео (для устройств Apple).|
|viewToken|Маркер просмотра с ограниченным сроком действия для потоковой передачи видео.|
|sourceLanguage|Исходный язык видео.|
|language|Фактический язык видео (язык перевода).|
|indexingPreset|Предустановка, используемая для индексирования видео.|
|streamingPreset|Предустановка, используемая для публикации видео.|
|linguisticModelId|Модель CRIS, используемая для транскрибирования видео.|
|statistics | Дополнительные сведения см. в разделе [statistics](#statistics).|

```json
{
    "videos": [{
        "accountId": "2cbbed36-1972-4506-9bc7-55367912df2d",
        "id": "142a356aa6",
        "state": "Processed",
        "privacyMode": "Private",
        "processingProgress": "100%",
        "failureCode": "General",
        "failureMessage": "",
        "externalId": null,
        "externalUrl": null,
        "metadata": null,
        "insights": {. . . },
        "thumbnailId": "89d7192c-1dab-4377-9872-473eac723845",
        "publishedUrl": "https://videvmediaservices.streaming.mediaservices.windows.net:443/d88a652d-334b-4a66-a294-3826402100cd/Xamarine.ism/manifest",
        "publishedProxyUrl": null,
        "viewToken": "Bearer=<token>",
        "sourceLanguage": "En-US",
        "language": "En-US",
        "indexingPreset": "Default",
        "linguisticModelId": "00000000-0000-0000-0000-000000000000"
    }],
}
```
### <a name="insights"></a>insights

Аналитические сведения — это набор измерений (например, строки расшифровки, лица, торговые марки и т.д.). Каждое измерение представляет собой список уникальных элементов (например, face1, face2, face3), а каждый элемент имеет собственные метаданные и список экземпляров (временные интервалы с дополнительными необязательными метаданными).

Элемент face может иметь идентификатор, имя, эскиз, другие метаданные и список временных экземпляров (например, 00:00:05–00:00:10, 00:01:00–00:02:30 и 00:41:21–00:41:49). Каждый временной экземпляр может содержать дополнительные метаданные. Например, координаты прямоугольной рамки вокруг лица (20,230,60,60).

|Version|Версия кода|
|---|---|
|sourceLanguage|Исходный язык видео (предполагается один основной язык). В строковом формате на основе [BCP-47](https://tools.ietf.org/html/bcp47).|
|language|Язык аналитических сведений (перевод с исходного языка). В строковом формате на основе [BCP-47](https://tools.ietf.org/html/bcp47).|
|transcript|Измерение [transcript](#transcript).|
|ocr|[OCR](#ocr) измерения.|
|keywords|Измерение [keywords](#keywords).|
|blocks|Может содержать один или несколько [блоков](#blocks).|
|faces|Измерение [faces](#faces).|
|метки;|Измерение [labels](#labels).|
|shots|Измерение [shots](#shots).|
|brands|Измерение [brands](#brands).|
|audioEffects|Измерение [audioEffects](#audioEffects).|
|sentiments|Измерение [sentiments](#sentiments).|
|visualContentModeration|Измерение [VisualContentModeration](#visualcontentmoderation).|
|textualContentModeration|Измерение [textualConentModeration](#textualcontentmoderation).|
|emotions| Измерение [emotions](#emotions).|
|topics|Измерение [topics](#topics).|

Пример:

```json
{
  "version": "0.9.0.0",
  "sourceLanguage": "en-US",
  "language": "es-ES",
  "transcript": ...,
  "ocr": ...,
  "keywords": ...,
  "faces": ...,
  "labels": ...,
  "shots": ...,
  "brands": ...,
  "audioEffects": ...,
  "sentiments": ...,
  "visualContentModeration": ...,
  "textualContentModeration": ...
}
```

#### <a name="blocks"></a>blocks

Атрибут | Описание
---|---
id|Идентификатор блока.|
instances|Список диапазонов времени этого блока.|

#### <a name="transcript"></a>transcript

|Name|Описание|
|---|---|
|id|Идентификатор строки.|
|Text|Сама расшифровка аудиозаписи.|
|language|Язык расшифровки аудиозаписи. Предназначен для поддержки расшифровки, где каждая строка может быть написана на другом языке.|
|instances|Список диапазонов времени, в которых появилась эта строка. Если экземпляр является расшифровкой, будет отображаться только один экземпляр.|

Пример:

```json
"transcript": [
{
    "id": 0,
    "text": "Hi I'm Doug from office.",
    "language": "en-US",
    "instances": [
    {
        "start": "00:00:00.5100000",
        "end": "00:00:02.7200000"
    }
    ]
},
{
    "id": 1,
    "text": "I have a guest. It's Michelle.",
    "language": "en-US",
    "instances": [
    {
        "start": "00:00:02.7200000",
        "end": "00:00:03.9600000"
    }
    ]
}
] 
```

#### <a name="ocr"></a>ocr

|Name|Описание|
|---|---|
|id|Идентификатор строки OCR.|
|Text|Текст OCR.|
|confidence|Достоверность распознавания.|
|language|Язык OCR.|
|instances|Список диапазонов времени, где появилось это OCR (одно и то же OCR может появляться несколько раз).|
|height|Высота прямоугольника OCR|
|top|Расположение верхнего в px|
|Левое| Элемент левой px|
|width|Ширина прямоугольника OCR|

```json
"ocr": [
    {
      "id": 0,
      "text": "LIVE FROM NEW YORK",
      "confidence": 675.971,
      "height": 35,
      "language": "en-US",
      "left": 31,
      "top": 97,
      "width": 400,      
      "instances": [
        {
          "start": "00:00:26",
          "end": "00:00:52"
        }
      ]
    }
  ],
```

#### <a name="keywords"></a>keywords

|ИМЯ|Описание|
|---|---|
|id|Идентификатор ключевого слова.|
|Text|Идентификатор текста.|
|confidence|Достоверность распознавания ключевого слова.|
|language|Язык ключевого слова (при наличии перевода).|
|instances|Список диапазонов времени, где появилось это ключевое слово (ключевое слово может появляться несколько раз).|

```json
{
    id: 0,
    text: "technology",
    confidence: 1,
    language: "en-US",
    instances: [{
            adjustedStart: "0:05:15.782",
            adjustedEnd: "0:05:16.249",
            start: "0:05:15.782",
            end: "0:05:16.249"
    },
    {
            adjustedStart: "0:04:54.761",
            adjustedEnd: "0:04:55.228",
            start: "0:04:54.761",
            end: "0:04:55.228"
    }]
}
```

#### <a name="faces"></a>faces

|ИМЯ|Описание|
|---|---|
|id|Идентификатор лица.|
|name|Имя лица. Это может быть "Неизвестный № 0", идентифицированная знаменитость или заданный клиентом человек.|
|confidence|Достоверности идентификации лица.|
|description|Текстовое описание знаменитости. |
|thumbnailId|Идентификатор эскиза лица.|
|knownPersonId|Если это известный человек, значением будет внутренний идентификатор.|
|referenceId|Если это знаменитость, найденная с помощью Bing, значением будет ее идентификатор Bing.|
|referenceType|В настоящее время только Bing.|
|title|Если это знаменитость, значением будет ее должность (например, генеральный директор Майкрософт).|
|imageurl|Если это знаменитость, значением будет URL-адрес изображения с ней.|
|instances|Это примеры того, где лицо появилось в заданном интервале времени. У каждого экземпляра также есть thumbnailsId. |

```json
"faces": [{
    "id": 2002,
    "name": "Xam 007",
    "confidence": 0.93844,
    "description": null,
    "thumbnailId": "00000000-aee4-4be2-a4d5-d01817c07955",
    "knownPersonId": "8340004b-5cf5-4611-9cc4-3b13cca10634",
    "referenceId": null,
    "title": null,
    "imageUrl": null,
    "instances": [{
        "thumbnailsIds": ["00000000-9f68-4bb2-ab27-3b4d9f2d998e",
        "cef03f24-b0c7-4145-94d4-a84f81bb588c"],
        "adjustedStart": "00:00:07.2400000",
        "adjustedEnd": "00:00:45.6780000",
        "start": "00:00:07.2400000",
        "end": "00:00:45.6780000"
    },
    {
        "thumbnailsIds": ["00000000-51e5-4260-91a5-890fa05c68b0"],
        "adjustedStart": "00:10:23.9570000",
        "adjustedEnd": "00:10:39.2390000",
        "start": "00:10:23.9570000",
        "end": "00:10:39.2390000"
    }]
}]
```

#### <a name="labels"></a>метки;

|ИМЯ|Описание|
|---|---|
|id|Идентификатор метки.|
|name|Название метки (например, "Компьютер", "Телевизор").|
|language|Язык метки (при наличии перевода). BCP-47|
|instances|Список диапазонов времени, где появилась эта метка (метка может появляться несколько раз). У каждого экземпляра есть поле достоверности. |


```json
"labels": [
    {
      "id": 0,
      "name": "person",
      "language": "en-US",
      "instances": [
        {
          "confidence": 1.0,
          "start": "00: 00: 00.0000000",
          "end": "00: 00: 25.6000000"
        },
        {
          "confidence": 1.0,
          "start": "00: 01: 33.8670000",
          "end": "00: 01: 39.2000000"
        }
      ]
    },
    {
      "name": "indoor",
      "language": "en-US",
      "id": 1,
      "instances": [
        {
          "confidence": 1.0,
          "start": "00: 00: 06.4000000",
          "end": "00: 00: 07.4670000"
        },
        {
          "confidence": 1.0,
          "start": "00: 00: 09.6000000",
          "end": "00: 00: 10.6670000"
        },
        {
          "confidence": 1.0,
          "start": "00: 00: 11.7330000",
          "end": "00: 00: 20.2670000"
        },
        {
          "confidence": 1.0,
          "start": "00: 00: 21.3330000",
          "end": "00: 00: 25.6000000"
        }
      ]
    }
  ] 
```

#### <a name="scenes"></a>scenes

|ИМЯ|Описание|
|---|---|
|id|Идентификатор сцены.|
|instances|Список диапазонов времени этой сценой (сцены может иметь только один экземпляр).|

```json
"scenes":[  
    {  
      "id":0,
      "instances":[  
          {  
            "start":"0:00:00",
            "end":"0:00:06.34",
            "duration":"0:00:06.34"
          }
      ]
    },
    {  
      "id":1,
      "instances":[  
          {  
            "start":"0:00:06.34",
            "end":"0:00:47.047",
            "duration":"0:00:40.707"
          }
      ]
    },

]
```

#### <a name="shots"></a>shots

|Name|Описание|
|---|---|
|id|Идентификатор снимка.|
|keyframes|Список ключевых кадров в снимке, (каждый имеет идентификатор и список экземпляров временных диапазонов). Каждый экземпляр опорный кадр имеет thumbnailId поля, которое содержит эскиз опорный кадр идентификатор.|
|instances|Список диапазонов времени этот снимок (снимок может иметь только один экземпляр).|

```json
"shots":[  
    {  
      "id":0,
      "keyFrames":[  
          {  
            "id":0,
            "instances":[  
                {  
                  "thumbnailId":"00000000-0000-0000-0000-000000000000",
                  "start":"0:00:00.209",
                  "end":"0:00:00.251",
                  "duration":"0:00:00.042"
                }
            ]
          },
          {  
            "id":1,
            "instances":[  
                {  
                  "thumbnailId":"00000000-0000-0000-0000-000000000000",
                  "start":"0:00:04.755",
                  "end":"0:00:04.797",
                  "duration":"0:00:00.042"
                }
            ]
          }
      ],
      "instances":[  
          {  
            "start":"0:00:00",
            "end":"0:00:06.34",
            "duration":"0:00:06.34"
          }
      ]
    },

]
```

#### <a name="brands"></a>brands

Названия организаций и торговые марки продуктов, обнаруженные при преобразовании речи в текст и (или) распознавании текста на видео. Сюда не входит распознавание торговых марок или логотипов на изображении.

|ИМЯ|Описание|
|---|---|
|id|Идентификатор торговой марки.|
|name|Название торговой марки.|
|referenceId | Суффикс URL-адреса статьи о торговой марке в Википедии. Например, Target_Corporation — это суффикс ссылки [https://en.wikipedia.org/wiki/Target_Corporation](https://en.wikipedia.org/wiki/Target_Corporation).
|referenceUrl | URL-адрес статьи о торговой марке в Википедии (если существует). (например, [https://en.wikipedia.org/wiki/Target_Corporation](https://en.wikipedia.org/wiki/Target_Corporation)).
|description|Описание торговой марки.|
|теги|Список предопределенных тегов, которые связаны с этой торговой маркой.|
|confidence|Значение достоверности средства обнаружения торговых марок в Индексаторе видео (0–1).|
|instances|Список диапазонов времени, в которых упоминается эта торговая марка. Каждый экземпляр содержит объект brandType, который указывает, появилась ли торговая марка при расшифровке или распознавании текста.|

```json
"brands": [
{
    "id": 0,
    "name": "MicrosoftExcel",
    "referenceId": "Microsoft_Excel",
    "referenceUrl": "http: //en.wikipedia.org/wiki/Microsoft_Excel",
    "referenceType": "Wiki",
    "description": "Microsoft Excel is a sprea..",
    "tags": [],
    "confidence": 0.975,
    "instances": [
    {
        "brandType": "Transcript",
        "start": "00: 00: 31.3000000",
        "end": "00: 00: 39.0600000"
    }
    ]
},
{
    "id": 1,
    "name": "Microsoft",
    "referenceId": "Microsoft",
    "referenceUrl": "http: //en.wikipedia.org/wiki/Microsoft",
    "description": "Microsoft Corporation is...",
    "tags": [
    "competitors",
    "technology"
    ],
    "confidence": 1.0,
    "instances": [
    {
        "brandType": "Transcript",
        "start": "00: 01: 44",
        "end": "00: 01: 45.3670000"
    },
    {
        "brandType": "Ocr",
        "start": "00: 01: 54",
        "end": "00: 02: 45.3670000"
    }
    ]
}
]
```

#### <a name="statistics"></a>statistics

|Name|Описание|
|---|---|
|CorrespondenceCount|Количество соответствий в видео.|
|SpeakerWordCount|Число слов каждого говорящего.|
|SpeakerNumberOfFragments|Число фрагментов в видео на определенного говорящего.|
|SpeakerLongestMonolog|Самый длинный монолог говорящего. Учитываются также паузы в монологе говорящего. Паузы в начале и конце монолога удаляются.| 
|SpeakerTalkToListenRatio|Результат вычисления основан на длительности монолога говорящего (без учета пауз между монологами), деленной на общее время видео. Значение времени округляется до трех знаков после запятой.|

#### <a name="a-idaudioeffectsaudioeffects"></a><a id="audioEffects"/>audioEffects

|Name|Описание|
|---|---|
|id|Идентификатор аудиоэффекта.|
|type|Тип аудиоэффекта (например, овации, речь, тишина).|
|instances|Список диапазонов времени, в которых появились эти аудиоэффекты.|

```json
"audioEffects": [
{
    "id": 0,
    "type": "Clapping",
    "instances": [
    {
        "start": "00:00:00",
        "end": "00:00:03"
    },
    {
        "start": "00:01:13",
        "end": "00:01:21"
    }
    ]
}
]
```

#### <a name="sentiments"></a>sentiments

Тональность агрегируется по полю sensimentType (позитивная/нейтральная/негативная). Например, 0–0,1 0,2–0,1.

|ИМЯ|Описание|
|---|---|
|id|Идентификатор тональности.|
|averageScore |Среднее значение всех оценок всех экземпляров этого типа тональности — позитивная/нейтральная/негативная.|
|instances|Список диапазонов времени, в которых появилась эта тональность.|
|sentimentType |Может иметь значение Positive, Neutral или Negative.|

```json
"sentiments": [
{
    "id": 0,
    "averageScore": 0.87,
    "sentimentType": "Positive",
    "instances": [
    {
        "start": "00:00:23",
        "end": "00:00:41"
    }
    ]
}, {
    "id": 1,
    "averageScore": 0.11,
    "sentimentType": "Positive",
    "instances": [
    {
        "start": "00:00:13",
        "end": "00:00:21"
    }
    ]
}
]
```

#### <a name="visualcontentmoderation"></a>visualContentModeration

Блок visualContentModeration содержит диапазоны времени, в которых Индексатор видео, вероятно, обнаружил содержимое для взрослых. Если блок visualContentModeration пуст, содержимое для взрослых не обнаружено.

Видео, в которых обнаружено содержимое для взрослых или содержимое непристойного характера, могут быть доступны только для закрытого просмотра. Пользователи могут отправить запрос для проверки содержимого человеком. В таком случае атрибут IsAdult будет содержать результат этой проверки.

|Name|Описание|
|---|---|
|id|Идентификатор модерации визуального контента.|
|adultScore|Результат оценки содержимого для взрослых (от Content Moderator).|
|racyScore|Результат оценки содержимого непристойного характера (от Content Moderator).|
|instances|Список диапазонов времени, где применяется модерация контента.|

```json
"VisualContentModeration": [
{
    "id": 0,
    "adultScore": 0.00069,
    "racyScore": 0.91129,
    "instances": [
    {
        "start": "00:00:25.4840000",
        "end": "00:00:25.5260000"
    }
    ]
},
{
    "id": 1,
    "adultScore": 0.99231,
    "racyScore": 0.99912,
    "instances": [
    {
        "start": "00:00:35.5360000",
        "end": "00:00:35.5780000"
    }
    ]
}
] 
```

#### <a name="textualcontentmoderation"></a>textualContentModeration 

|ИМЯ|Описание|
|---|---|
|id|Идентификатор модерации текстового контента.|
|bannedWordsCount |Число запрещенных слов.|
|bannedWordsRatio |Соотношение к общему числу слов.|

#### <a name="emotions"></a>emotions

Индексатор видео определяет эмоции на основе речевых и звуковых признаков. Определяются следующие эмоции: радость, грусть, гнев или страх.

|Name|Описание|
|---|---|
|id|Идентификатор эмоции.|
|type|Момент проявления эмоции, определенный на основе речевых и звуковых признаков. Возможные эмоции: радость, грусть, гнев или страх.|
|instances|Список диапазонов времени, когда проявлялась эта эмоция.|

```json
"emotions": [{
    "id": 0,
    "type": "Fear",
    "instances": [{
      "adjustedStart": "0:00:39.47",
      "adjustedEnd": "0:00:45.56",
      "start": "0:00:39.47",
      "end": "0:00:45.56"
    },
    {
      "adjustedStart": "0:07:19.57",
      "adjustedEnd": "0:07:23.25",
      "start": "0:07:19.57",
      "end": "0:07:23.25"
    }]
  },
  {
    "id": 1,
    "type": "Anger",
    "instances": [{
      "adjustedStart": "0:03:55.99",
      "adjustedEnd": "0:04:05.06",
      "start": "0:03:55.99",
      "end": "0:04:05.06"
    },
    {
      "adjustedStart": "0:04:56.5",
      "adjustedEnd": "0:05:04.35",
      "start": "0:04:56.5",
      "end": "0:05:04.35"
    }]
  },
  {
    "id": 2,
    "type": "Joy",
    "instances": [{
      "adjustedStart": "0:12:23.68",
      "adjustedEnd": "0:12:34.76",
      "start": "0:12:23.68",
      "end": "0:12:34.76"
    },
    {
      "adjustedStart": "0:12:46.73",
      "adjustedEnd": "0:12:52.8",
      "start": "0:12:46.73",
      "end": "0:12:52.8"
    },
    {
      "adjustedStart": "0:30:11.29",
      "adjustedEnd": "0:30:16.43",
      "start": "0:30:11.29",
      "end": "0:30:16.43"
    },
    {
      "adjustedStart": "0:41:37.23",
      "adjustedEnd": "0:41:39.85",
      "start": "0:41:37.23",
      "end": "0:41:39.85"
    }]
  },
  {
    "id": 3,
    "type": "Sad",
    "instances": [{
      "adjustedStart": "0:13:38.67",
      "adjustedEnd": "0:13:41.3",
      "start": "0:13:38.67",
      "end": "0:13:41.3"
    },
    {
      "adjustedStart": "0:28:08.88",
      "adjustedEnd": "0:28:18.16",
      "start": "0:28:08.88",
      "end": "0:28:18.16"
    }]
  }
],
```

#### <a name="topics"></a>topics

Индексатор видео делает выводы о главных темах в расшифровке речи. При возможности включается 1-й уровень классификации [IPTC](https://iptc.org/standards/media-topics/). 

|ИМЯ|Описание|
|---|---|
|id|Идентификатор темы.|
|name|Название темы (например, "Лекарственные препараты").|
|referenceId|Навигация, отражающая иерархию темы. Пример: "Здоровый образ жизни/Медицина и здравоохранение/Фармацевтика".|
|confidence|Оценка надежности в диапазоне [0,1]. Чем больше значение, тем выше степень надежности.|
|language|Язык темы.|
|iptcName|Название кода мультимедиа IPTC, если таковой обнаружен.|
|instances |В настоящее время Индексатор видео не выполняет индексацию темы по интервалам времени, поэтому в качестве интервала используется все видео.|

```json
"topics": [{
    "id": 0,
    "name": "INTERNATIONAL RELATIONS",
    "referenceId": "POLITICS AND GOVERNMENT/FOREIGN POLICY/INTERNATIONAL RELATIONS",
    "referenceType": "VideoIndexer",
    "confidence": 1,
    "language": "en-US",
    "instances": [{
        "adjustedStart": "0:00:00",
        "adjustedEnd": "0:03:36.25",
        "start": "0:00:00",
        "end": "0:03:36.25"
    }]
}, {
    "id": 1,
    "name": "Politics and Government",
    "referenceType": "VideoIndexer",
    "iptcName": "Politics",
    "confidence": 0.9041,
    "language": "en-US",
    "instances": [{
        "adjustedStart": "0:00:00",
        "adjustedEnd": "0:03:36.25",
        "start": "0:00:00",
        "end": "0:03:36.25"
    }]
}]
. . .
```

## <a name="next-steps"></a>Дальнейшие действия

[Video Indexer Developer Portal](https://api-portal.videoindexer.ai) (Портал разработчика Индексатора видео)

См. дополнительные сведения о том, как [внедрить мини-приложения Индексатора видео в свое приложение](video-indexer-embed-widgets.md). 

