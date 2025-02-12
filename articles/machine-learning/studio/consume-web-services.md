---
title: Использование веб-службы
titleSuffix: Azure Machine Learning Studio
description: Развернув службу машинного обучения с помощью Студии машинного обучения Azure, веб-службу RESTFul можно использовать в качестве службы обработки запросов и ответов в режиме реального времени или службы пакетного выполнения.
services: machine-learning
ms.service: machine-learning
ms.subservice: studio
ms.topic: conceptual
author: xiaoharper
ms.author: amlstudiodocs
ms.custom: seodec18
ms.date: 06/02/2017
ms.openlocfilehash: a537227a7003391122e10f7f39233040cef49db3
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "60751303"
---
# <a name="how-to-consume-an-azure-machine-learning-studio-web-service"></a>Как использовать веб-службу Студии машинного обучения Azure

После развертывания прогнозной модели Студии машинного обучения Azure в качестве веб-службы можно использовать REST API для отправки данных и получения прогнозов. Данные можно отправлять в режиме реального времени или пакетном режиме.

Дополнительные сведения о способах создания и развертывания веб-службы машинного обучения с помощью Студии машинного обучения можно найти в следующих статьях:

* Руководство по созданию эксперимента в Студии машинного обучения см. в [этой статье](create-experiment.md).
* Дополнительные сведения о развертывании веб-службы см. в [этой статье](publish-a-machine-learning-web-service.md).
* Дополнительные сведения о машинном обучении см. в [центре документации по Машинному обучению Azure](https://azure.microsoft.com/documentation/services/machine-learning/).



## <a name="overview"></a>Обзор
С помощью веб-службы машинного обучения Azure внешнее приложение взаимодействует с рабочим процессом машинного обучения, оценивая модель в режиме реального времени. Вызов веб-службы машинного обучения возвращает результаты прогноза внешнему приложению. Чтобы вызвать веб-службу машинного обучения, следует передать ключ API, создаваемый при развертывании прогноза. Веб-служба машинного обучения работает на основе архитектуры REST, используемой в основном в проектах с веб-программированием.

Студия машинного обучения Azure содержит два типа служб:

* Служба обработки запросов и ответов (RRS) — высокомасштабируемая служба с малым временем задержки, используемая в качестве интерфейса для моделей без изменения состояния, которые создаются и развертываются в студии машинного обучения Microsoft Azure.
* Служба пакетного выполнения (BES) — асинхронная служба, оценивающая пакет записей данных.

Дополнительные сведения о веб-службах машинного обучения приведены в статье [Развертывание веб-службы машинного обучения Azure](publish-a-machine-learning-web-service.md).

## <a name="get-an-azure-machine-learning-studio-authorization-key"></a>Получение ключа авторизации для Студии машинного обучения Azure
При развертывании эксперимента создаются ключи API для веб-службы. Ключи можно извлечь из нескольких расположений.

### <a name="from-the-microsoft-azure-machine-learning-web-services-portal"></a>Из портала веб-службы Машинного обучения Microsoft Azure
Войдите на портал [веб-службы Машинного обучения Microsoft Azure](https://services.azureml.net).

Чтобы получить ключ API для новой веб-службы машинного обучения, сделайте следующее:

1. На портале веб-служб Машинного обучения Azure в верхнем меню щелкните **Веб-службы**.
2. Щелкните веб-службу, для которой требуется получить ключ.
3. В верхнем меню щелкните **Consume**(Использование).
4. Скопируйте и сохраните **Primary Key**(Первичный ключ).

Чтобы получить ключ API для классической веб-службы машинного обучения, сделайте следующее:

1. На портале веб-служб Машинного обучения Azure в верхнем меню щелкните **Classic Web Services** (Классические веб-службы).
2. Выберите веб-службу, с которой вы работаете.
3. Щелкните конечную точку, для которой требуется получить ключ.
4. В верхнем меню щелкните **Consume**(Использование).
5. Скопируйте и сохраните **Primary Key**(Первичный ключ).

### <a name="classic-web-service"></a>Классическая веб-служба
 Ключ для классической веб-службы можно также получить из Студии машинного обучения.

#### <a name="machine-learning-studio"></a>Студия машинного обучения
1. В студии машинного обучения щелкните **WEB SERVICES** (ВЕБ-СЛУЖБЫ) слева.
2. Щелкните веб-службу. **Ключ API** указан на вкладке **Панель мониторинга**.

## <a id="connect"></a>Подключение к веб-службе машинного обучения
К веб-службе машинного обучения можно подключиться, используя любой язык программирования, поддерживающий HTTP-запрос и HTTP-ответ. Примеры на языках C#, Python и R можно просмотреть на странице справки веб-службы машинного обучения.

**Справка по API машинного обучения.** Страница справки по API машинного обучения Azure создается при развертывании веб-службы. См. [Руководство 3: Развертывание модели кредитных рисков в Студии машинного обучения Azure](tutorial-part3-credit-risk-deploy.md).
Справка по API машинного обучения содержит сведения о веб-службе прогнозирования.

1. Выберите веб-службу, с которой вы работаете.
2. Щелкните конечную точку, для которой требуется просмотреть страницу справки по API.
3. В верхнем меню щелкните **Consume**(Использование).
4. Щелкните **API help page** (Страница справки API) под конечными точками "Запрос — ответ" или "Выполнение пакета".

**Просмотр справки по API машинного обучения для новой веб-службы**

На [портале веб-служб машинного обучения Azure](https://services.azureml.net/) сделайте следующее:

1. В верхнем меню щелкните **WEB SERVICES** (ВЕБ-СЛУЖБЫ).
2. Щелкните веб-службу, для которой требуется получить ключ.

Щелкните **Use Web Service** (Использовать веб-службу), чтобы получить значения URI для службы "запрос и ответ" и службы пакетного выполнения, а также для примеров кода на C#, R и Python.

Щелкните **Swagger API**, чтобы получить документацию на основе Swagger для интерфейсов API, вызываемых из передаваемых URI.

### <a name="c-sample"></a>Пример на языке C#
Для подключения к веб-службе машинного обучения воспользуйтесь клиентом **HttpClient**, передав ScoreData. Данные набора содержат Вектор свойств, n-мерный вектор числовых параметров, представляющий Данные набора. Службу машинного обучения можно аутентифицировать с помощью ключа API.

Для подключения к веб-службе машинного обучения должен быть установлен пакет NuGet **Microsoft.AspNet.WebApi.Client**.

**Установка Microsoft.AspNet.WebApi.Client NuGet в Visual Studio**

1. Опубликуйте веб-службу "Download dataset from UCI: Adult 2 class dataset".
2. Выберите **Инструменты** > **Диспетчер пакетов NuGet** > **Консоль диспетчера пакетов**.
3. Выберите **Установить пакет Microsoft.AspNet.WebApi.Client**.

**Для запуска примера выполните следующие действия:**

1. Опубликуйте эксперимент "Sample 1: Download dataset from UCI: Adult 2 class dataset", входящий в коллекцию примеров для Машинного обучения.
2. Назначьте apiKey ключ из веб-службы. См. раздел **Получение ключа авторизации для Студии машинного обучения Azure** выше.
3. Назначьте serviceUri универсальный код ресурса запроса.

**Вот как будет выглядеть полный запрос:**
```csharp
using System;
using System.Collections.Generic;
using System.IO;
using System.Net.Http;
using System.Net.Http.Formatting;
using System.Net.Http.Headers;
using System.Text;
using System.Threading.Tasks;

namespace CallRequestResponseService
{
    class Program
    {
        static void Main(string[] args)
        {
            InvokeRequestResponseService().Wait();
        }

        static async Task InvokeRequestResponseService()
        {
            using (var client = new HttpClient())
            {
                var scoreRequest = new
                {
                    Inputs = new Dictionary<string, List<Dictionary<string, string>>> () {
                        {
                            "input1",
                            // Replace columns labels with those used in your dataset
                            new List<Dictionary<string, string>>(){new Dictionary<string, string>(){
                                    {
                                        "column1", "value1"
                                    },
                                    {
                                        "column2", "value2"
                                    },
                                    {
                                        "column3", "value3"
                                    }
                                }
                            }
                        },
                    },
                    GlobalParameters = new Dictionary<string, string>() {}
                };

                // Replace these values with your API key and URI found on https://services.azureml.net/
                const string apiKey = "<your-api-key>"; 
                const string apiUri = "<your-api-uri>";
                
                client.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue( "Bearer", apiKey);
                client.BaseAddress = new Uri(apiUri);

                // WARNING: The 'await' statement below can result in a deadlock
                // if you are calling this code from the UI thread of an ASP.NET application.
                // One way to address this would be to call ConfigureAwait(false)
                // so that the execution does not attempt to resume on the original context.
                // For instance, replace code such as:
                //      result = await DoSomeTask()
                // with the following:
                //      result = await DoSomeTask().ConfigureAwait(false)

                HttpResponseMessage response = await client.PostAsJsonAsync("", scoreRequest);

                if (response.IsSuccessStatusCode)
                {
                    string result = await response.Content.ReadAsStringAsync();
                    Console.WriteLine("Result: {0}", result);
                }
                else
                {
                    Console.WriteLine(string.Format("The request failed with status code: {0}", response.StatusCode));

                    // Print the headers - they include the request ID and the timestamp,
                    // which are useful for debugging the failure
                    Console.WriteLine(response.Headers.ToString());

                    string responseContent = await response.Content.ReadAsStringAsync();
                    Console.WriteLine(responseContent);
                }
            }
        }
    }
}
```

### <a name="python-sample"></a>Пример на Python
Чтобы подключиться к веб-службе машинного обучения, используйте библиотеку **urllib2** для Python 2.X и библиотеку **urllib.request** для Python 3.X. Вы передадите данные ScoreData, содержащие FeatureVector — n-мерный вектор числовых параметров, представляющий ScoreData. Службу машинного обучения можно аутентифицировать с помощью ключа API.

**Для запуска примера выполните следующие действия:**

1. Разверните эксперимент "Sample 1: Download dataset from UCI: Adult 2 class dataset", входящий в коллекцию примеров для Машинного обучения.
2. Назначьте apiKey ключ из веб-службы. См. раздел **Получение ключа авторизации для Студии машинного обучения Azure** в начале этой статьи.
3. Назначьте serviceUri универсальный код ресурса запроса.

**Вот как будет выглядеть полный запрос:**
```python
import urllib2 # urllib.request for Python 3.X
import json

data = {
    "Inputs": {
        "input1":
        [
            {
                'column1': "value1",   
                'column2': "value2",   
                'column3': "value3"
            }
        ],
    },
    "GlobalParameters":  {}
}

body = str.encode(json.dumps(data))

# Replace this with the URI and API Key for your web service
url = '<your-api-uri>'
api_key = '<your-api-key>'
headers = {'Content-Type':'application/json', 'Authorization':('Bearer '+ api_key)}

# "urllib.request.Request(uri, body, headers)" for Python 3.X
req = urllib2.Request(url, body, headers)

try:
    # "urllib.request.urlopen(req)" for Python 3.X
    response = urllib2.urlopen(req)

    result = response.read()
    print(result)
# "urllib.error.HTTPError as error" for Python 3.X
except urllib2.HTTPError, error: 
    print("The request failed with status code: " + str(error.code))

    # Print the headers - they include the request ID and the timestamp, which are useful for debugging the failure
    print(error.info())
    print(json.loads(error.read())) 
```

### <a name="r-sample"></a>Пример на языке R

Чтобы подключиться к веб-службе машинного обучения, используйте для выполнения запроса и обработки ответа в формате JSON библиотеки **RCurl** и **rjson**. Вы передадите данные ScoreData, содержащие FeatureVector — n-мерный вектор числовых параметров, представляющий ScoreData. Службу машинного обучения можно аутентифицировать с помощью ключа API.

**Вот как будет выглядеть полный запрос:**
```r
library("RCurl")
library("rjson")

# Accept SSL certificates issued by public Certificate Authorities
options(RCurlOptions = list(cainfo = system.file("CurlSSL", "cacert.pem", package = "RCurl")))

h = basicTextGatherer()
hdr = basicHeaderGatherer()

req = list(
    Inputs = list(
            "input1" = list(
                list(
                        'column1' = "value1",
                        'column2' = "value2",
                        'column3' = "value3"
                    )
            )
        ),
        GlobalParameters = setNames(fromJSON('{}'), character(0))
)

body = enc2utf8(toJSON(req))
api_key = "<your-api-key>" # Replace this with the API key for the web service
authz_hdr = paste('Bearer', api_key, sep=' ')

h$reset()
curlPerform(url = "<your-api-uri>",
httpheader=c('Content-Type' = "application/json", 'Authorization' = authz_hdr),
postfields=body,
writefunction = h$update,
headerfunction = hdr$update,
verbose = TRUE
)

headers = hdr$value()
httpStatus = headers["status"]
if (httpStatus >= 400)
{
print(paste("The request failed with status code:", httpStatus, sep=" "))

# Print the headers - they include the request ID and the timestamp, which are useful for debugging the failure
print(headers)
}

print("Result:")
result = h$value()
print(fromJSON(result))
```

### <a name="javascript-sample"></a>Пример на языке JavaScript

Чтобы подключиться к веб-службе машинного обучения, используйте в своем проекте пакет npm **request**. Кроме того, для форматирования входных данных и анализа результата вы будете использовать объект `JSON`. Выполните установку с помощью `npm install request --save` или добавьте `"request": "*"` в файл package.json в разделе `dependencies` и запустите `npm install`.

**Вот как будет выглядеть полный запрос:**
```js
let req = require("request");

const uri = "<your-api-uri>";
const apiKey = "<your-api-key>";

let data = {
    "Inputs": {
        "input1":
        [
            {
                'column1': "value1",
                'column2': "value2",
                'column3': "value3"
            }
        ],
    },
    "GlobalParameters": {}
}

const options = {
    uri: uri,
    method: "POST",
    headers: {
        "Content-Type": "application/json",
        "Authorization": "Bearer " + apiKey,
    },
    body: JSON.stringify(data)
}

req(options, (err, res, body) => {
    if (!err && res.statusCode == 200) {
        console.log(body);
    } else {
        console.log("The request failed with status code: " + res.statusCode);
    }
});
```
