---
title: Краткое руководство. Обнаружение лиц на изображении с помощью REST API и Node.js
titleSuffix: Azure Cognitive Services
description: В этом кратком руководстве описано, как определить лица на изображении с помощью REST API распознавания лиц Azure и Node.js.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: quickstart
ms.date: 07/03/2019
ms.author: pafarley
ms.openlocfilehash: aade8552e7fd39a61334c1c6c7c9ca62085961be
ms.sourcegitcommit: f10ae7078e477531af5b61a7fe64ab0e389830e8
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/05/2019
ms.locfileid: "67603280"
---
# <a name="quickstart-detect-faces-in-an-image-using-the-face-rest-api-and-nodejs"></a>Краткое руководство. Обнаружение лиц на изображении с помощью REST API и Node.js

В этом кратком руководстве описано, как определить лица на изображении с помощью REST API распознавания лиц Azure и Node.js.

Если у вас еще нет подписки Azure, [создайте бесплатную учетную запись Azure](https://azure.microsoft.com/free/?WT.mc_id=A261C142F), прежде чем начинать работу. 

## <a name="prerequisites"></a>Предварительные требования

- Ключ подписки на API распознавания лиц. Вы можете получить ключ бесплатной пробной подписки на странице [Пробная версия Cognitive Services](https://azure.microsoft.com/try/cognitive-services/?api=face-api). Или следуйте инструкциям в руководстве по [созданию учетной записи Cognitive Services](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account), чтобы получить подписку на API распознавания лиц и свой ключ.
- Редактор кода, например [Visual Studio Code](https://code.visualstudio.com/download).

## <a name="set-up-the-node-environment"></a>Настройка среды Node

В папке, где вы хотите создать проект, создайте файл *facedetection.js*. Затем установите в этот проект модуль `requests`. Он позволит вашим скриптам отправлять HTTP-запросы.

```shell
npm install request --save
```

## <a name="write-the-nodejs-script"></a>Запись скрипта Node.js

Вставьте следующий код в *facedetection.js*. Эти поля указывают, как подключиться к службе распознавания лиц и где получить входные данные. Вам нужно будет обновить поле `subscriptionKey`, указав значение вашего ключа подписки, и изменить строку `uriBase`, чтобы она содержала идентификатор правильного региона (список конечных точек для всех регионов см. в документации по [API распознавания лиц](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236)). Вам может потребоваться изменить поле `imageUrl`, чтобы указать свое входное изображение.

```javascript
'use strict';

const request = require('request');

// Replace <Subscription Key> with your valid subscription key.
const subscriptionKey = '<Subscription Key>';

// You must use the same location in your REST call as you used to get your
// subscription keys. For example, if you got your subscription keys from
// westus, replace "westcentralus" in the URL below with "westus".
const uriBase = 'https://westcentralus.api.cognitive.microsoft.com/face/v1.0/detect';

const imageUrl =
    'https://upload.wikimedia.org/wikipedia/commons/3/37/Dagestani_man_and_woman.jpg';
```

Затем добавьте следующий код, чтобы вызвать API распознавания лиц и получить данные об атрибутах лиц из входного изображения. Поле `returnFaceAttributes` определяет атрибуты лица, которые нужно получить. Вы можете изменить эту строку в зависимости от предполагаемого использования.


```javascript
// Request parameters.
const params = {
    'returnFaceId': 'true',
    'returnFaceLandmarks': 'false',
    'returnFaceAttributes': 'age,gender,headPose,smile,facialHair,glasses,' +
        'emotion,hair,makeup,occlusion,accessories,blur,exposure,noise'
};

const options = {
    uri: uriBase,
    qs: params,
    body: '{"url": ' + '"' + imageUrl + '"}',
    headers: {
        'Content-Type': 'application/json',
        'Ocp-Apim-Subscription-Key' : subscriptionKey
    }
};

request.post(options, (error, response, body) => {
  if (error) {
    console.log('Error: ', error);
    return;
  }
  let jsonResponse = JSON.stringify(JSON.parse(body), null, '  ');
  console.log('JSON Response\n');
  console.log(jsonResponse);
});
```

## <a name="save-and-run-the-script"></a>Сохранение и запуск скрипта

После внесения изменений откройте командную строку и запустите файл с помощью команды `node`.

```
node facedetection.js
```

В окне консоли должна появиться информация о лицах в виде данных JSON. Например:

```json
[
  {
    "faceId": "ae8952c1-7b5e-4a5a-a330-a6aa351262c9",
    "faceRectangle": {
      "top": 621,
      "left": 616,
      "width": 195,
      "height": 195
    },
    "faceAttributes": {
      "smile": 0,
      "headPose": {
        "pitch": 0,
        "roll": 6.8,
        "yaw": 3.7
      },
      "gender": "male",
      "age": 37,
      "facialHair": {
        "moustache": 0.4,
        "beard": 0.4,
        "sideburns": 0.1
      },
      "glasses": "NoGlasses",
      "emotion": {
        "anger": 0,
        "contempt": 0,
        "disgust": 0,
        "fear": 0,
        "happiness": 0,
        "neutral": 0.999,
        "sadness": 0.001,
        "surprise": 0
      },
      "blur": {
        "blurLevel": "high",
        "value": 0.89
      },
      "exposure": {
        "exposureLevel": "goodExposure",
        "value": 0.51
      },
      "noise": {
        "noiseLevel": "medium",
        "value": 0.59
      },
      "makeup": {
        "eyeMakeup": true,
        "lipMakeup": false
      },
      "accessories": [],
      "occlusion": {
        "foreheadOccluded": false,
        "eyeOccluded": false,
        "mouthOccluded": false
      },
      "hair": {
        "bald": 0.04,
        "invisible": false,
        "hairColor": [
          {
            "color": "black",
            "confidence": 0.98
          },
          {
            "color": "brown",
            "confidence": 0.87
          },
          {
            "color": "gray",
            "confidence": 0.85
          },
          {
            "color": "other",
            "confidence": 0.25
          },
          {
            "color": "blond",
            "confidence": 0.07
          },
          {
            "color": "red",
            "confidence": 0.02
          }
        ]
      }
    }
  },
  {
    "faceId": "b1bb3cbe-5a73-4f8d-96c8-836a5aca9415",
    "faceRectangle": {
      "top": 693,
      "left": 1503,
      "width": 180,
      "height": 180
    },
    "faceAttributes": {
      "smile": 0.003,
      "headPose": {
        "pitch": 0,
        "roll": 2,
        "yaw": -2.2
      },
      "gender": "female",
      "age": 56,
      "facialHair": {
        "moustache": 0,
        "beard": 0,
        "sideburns": 0
      },
      "glasses": "NoGlasses",
      "emotion": {
        "anger": 0,
        "contempt": 0.001,
        "disgust": 0,
        "fear": 0,
        "happiness": 0.003,
        "neutral": 0.984,
        "sadness": 0.011,
        "surprise": 0
      },
      "blur": {
        "blurLevel": "high",
        "value": 0.83
      },
      "exposure": {
        "exposureLevel": "goodExposure",
        "value": 0.41
      },
      "noise": {
        "noiseLevel": "high",
        "value": 0.76
      },
      "makeup": {
        "eyeMakeup": false,
        "lipMakeup": false
      },
      "accessories": [],
      "occlusion": {
        "foreheadOccluded": false,
        "eyeOccluded": false,
        "mouthOccluded": false
      },
      "hair": {
        "bald": 0.06,
        "invisible": false,
        "hairColor": [
          {
            "color": "black",
            "confidence": 0.99
          },
          {
            "color": "gray",
            "confidence": 0.89
          },
          {
            "color": "other",
            "confidence": 0.64
          },
          {
            "color": "brown",
            "confidence": 0.34
          },
          {
            "color": "blond",
            "confidence": 0.07
          },
          {
            "color": "red",
            "confidence": 0.03
          }
        ]
      }
    }
  }
]
```

## <a name="next-steps"></a>Дополнительная информация

В этом кратком руководстве вы написали скрипт Node.js, который вызывает API распознавания лиц Azure для обнаружения лиц на изображении и возвращения их атрибутов. Ознакомьтесь со справочной документацией по API распознавания лиц, чтобы узнать больше.

> [!div class="nextstepaction"]
> [API Распознавания лиц](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236)
