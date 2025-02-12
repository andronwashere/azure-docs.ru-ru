---
title: Поиск с помощью службы "Карты Azure" | Документация Майкрософт
description: Поиск ближайшей точки интереса с помощью службы "Карты Azure"
author: walsehgal
ms.author: v-musehg
ms.date: 03/07/2019
ms.topic: tutorial
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.custom: mvc
ms.openlocfilehash: a75f3f606129d370457816507537f2cb4491adf8
ms.sourcegitcommit: 75a56915dce1c538dc7a921beb4a5305e79d3c7a
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/24/2019
ms.locfileid: "68478829"
---
# <a name="search-nearby-points-of-interest-using-azure-maps"></a>Поиск ближайшей точки интереса с помощью службы "Карты Azure"

В этом руководстве показано, как настроить учетную запись службы "Карты Azure", а затем использовать API службы "Карты Azure" для поиска точки интереса. Из этого руководства вы узнаете, как выполнять следующие задачи:

> [!div class="checklist"]
> * создание учетной записи службы "Карты Azure";
> * получение первичного ключа для учетной записи службы "Карты Azure";
> * создание веб-страницы с помощью API элементов управления картой;
> * использование службы поиска "Карты Azure" для поиска ближайшей точки интереса.

Если у вас еще нет подписки Azure, [создайте бесплатную учетную запись Azure](https://azure.microsoft.com/free/), прежде чем начинать работу.

## <a name="sign-in-to-the-azure-portal"></a>Вход на портал Azure

Войдите на [портале Azure](https://portal.azure.com).

<a id="createaccount"></a>

## <a name="create-an-account-with-azure-maps"></a>Создание учетной записи службы "Карты Azure"

Создайте учетную запись службы "Карты Azure" с помощью следующих действий:

1. В верхнем левом углу [портала Azure](https://portal.azure.com) щелкните **Создать ресурс**.
2. В поле *Поиск по Marketplace* введите **Карты**.
3. Выберите *Результаты*, а затем — **Карты**. Нажмите кнопку **Создать**, расположенную под картой.
4. На странице **Создание учетной записи Azure Maps** введите следующие значения:
    * *Подписку*, которую необходимо использовать для этой учетной записи.
    * Имя *группы ресурсов* для этой учетной записи. Вы *создать новую* или *использовать существующую* группу ресурсов.
    * *Имя* новой учетной записи.
    * *Ценовая категория* этой учетной записи.
    * Ознакомьтесь с *лицензией* и *заявлением о конфиденциальности*, а затем установите флажок, чтобы принять условия соглашения.
    * Нажмите кнопку **Создать** .

![Создание учетной записи службы "Карты Azure" на портале](./media/tutorial-search-location/create-account.png)

<a id="getkey"></a>

## <a name="get-the-primary-key-for-your-account"></a>Получение первичного ключа для учетной записи

После успешного создания учетной записи службы "Карты Azure" извлеките ключ, позволяющий запрашивать API службы "Карты Azure".

1. Откройте учетную запись службы "Карты Azure" на портале.
2. В разделе "Параметры" выберите **Проверка подлинности**.
3. Скопируйте **первичный ключ** в буфер обмена. Сохраните его локально для использования в этом руководстве позже.

![Получение первичного ключа на портале](./media/tutorial-search-location/get-key.png)

<a id="createmap"></a>

## <a name="create-a-new-map"></a>Создание карты

API элементов управления картой — это удобная клиентская библиотека, которая позволяет легко интегрировать службу "Карты Azure" в веб-приложение. Она скрывает сложность "чистых" вызовов службы REST и ускоряет работу с помощью компонентов, поддерживающих изменение стилей и настройку. Чтобы создать статическую HTML-страницу со встроенным API элементов управления картой, сделайте следующее.

1. На локальном компьютере создайте файл **MapSearch.html**.
2. Добавьте в него следующие компоненты HTML.

   ```HTML
    <!DOCTYPE html>
    <html>
    <head>
        <title>Map Search</title>
        <meta charset="utf-8">
        <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">

        <!-- Add references to the Azure Maps Map control JavaScript and CSS files. -->
        <link rel="stylesheet" href="https://atlas.microsoft.com/sdk/javascript/mapcontrol/2/atlas.min.css" type="text/css">
        <script src="https://atlas.microsoft.com/sdk/javascript/mapcontrol/2/atlas.min.js"></script>

        <!-- Add a reference to the Azure Maps Services Module JavaScript file. -->
        <script src="https://atlas.microsoft.com/sdk/javascript/service/2/atlas-service.min.js"></script>

        <script>
        function GetMap(){
            //Add Map Control JavaScript code here.
        }
        </script>

        <style>
            html,
            body {
                width: 100%;
                height: 100%;
                padding: 0;
                margin: 0;
            }

            #myMap {
                width: 100%;
                height: 100%;
            }
        </style>
    </head>
    <body onload="GetMap()">
        <div id="myMap"></div>
    </body>
    </html>
    ```

   Обратите внимание на то, что заголовок HTML содержит файлы ресурсов CSS и JavaScript, размещенные в библиотеке Azure Map Control. Обратите внимание на событие `onload` в тексте страницы, которое вызовет функцию `GetMap` после загрузки текста страницы. Функция `GetMap` будет содержать внутренний код JavaScript для доступа к интерфейсам службы Azure Maps.

3. Добавьте следующий код JavaScript к функции `GetMap` HTML-файла. Замените строку `<Your Azure Maps Key>` первичным ключом, скопированным из учетной записи службы Maps.

    ```JavaScript
    //Instantiate a map object
    var map = new atlas.Map("myMap", {
        //Add your Azure Maps subscription key to the map SDK. Get an Azure Maps key at https://azure.com/maps
        authOptions: {
            authType: 'subscriptionKey',
            subscriptionKey: '<Your Azure Maps Key>'
        }
    });
    ```

   Этот сегмент инициирует API Map Control для ключа учетной записи службы Azure Maps. `atlas` — это пространство имен, которое содержит API и связанные визуальные компоненты. `atlas.Map` предоставляет элемент управления для визуальной интерактивной веб-карты.

4. Сохраните изменения в файл и откройте HTML-страницу в браузере. Это самая простая карта, которую можно создать, вызвав `atlas.Map`, используя ключ учетной записи.

   ![Просмотр карты](./media/tutorial-search-location/basic-map.png)

5. В функции `GetMap` после инициализации карты добавьте следующий код JavaScript.

    ```JavaScript
    //Wait until the map resources are ready.
    map.events.add('ready', function() {

        //Create a data source and add it to the map.
        datasource = new atlas.source.DataSource();
        map.sources.add(datasource);

        //Add a layer for rendering point data.
        var resultLayer = new atlas.layer.SymbolLayer(datasource, null, {
            iconOptions: {
                image: 'pin-round-darkblue',
                anchor: 'center',
                allowOverlap: true
            },
            textOptions: {
                anchor: "top"
            }
        });

        map.layers.add(resultLayer);
    });
    ```

   В этом сегменте кода на карту добавляется событие `ready`, которое сработает, когда ресурсы карты будут загружены, а карта готова к доступу. В обработчике событий карты `ready` создается источник данных для хранения выходных данных. Слой символов создается и привязывается к источнику данных. Этот слой определяет, как должны отображаться выходные данные в источнике данных. В этом случае — с использованием синего круглого значка, который размещается по центру координат результатов и может перекрываться другими значками. Слой результатов добавляется к слоям карты.

<a id="usesearch"></a>

## <a name="add-search-capabilities"></a>Добавление возможностей поиска

В этом разделе показано, как с помощью [API поиска](https://docs.microsoft.com/rest/api/maps/search) службы Maps искать точки интереса на карте. Это RESTful API, предназначенный для разработчиков. Он позволяет находить адреса, объекты и другие географические сведения. Служба поиска назначает широту и долготу для определенного адреса. С помощью описанного ниже **модуля службы** можно искать расположение, используя API поиска службы "Карты Azure".

### <a name="service-module"></a>Модуль службы

1. В обработчике событий карты `ready` создайте URL-адрес службы поиска, добавив приведенный ниже код JavaScript.

    ```JavaScript
   // Use SubscriptionKeyCredential with a subscription key
   var subscriptionKeyCredential = new atlas.service.SubscriptionKeyCredential(atlas.getSubscriptionKey());

   // Use subscriptionKeyCredential to create a pipeline
   var pipeline = atlas.service.MapsURL.newPipeline(subscriptionKeyCredential);

   // Construct the SearchURL object
   var searchURL = new atlas.service.SearchURL(pipeline); 
   ```

   `SubscriptionKeyCredential` создает `SubscriptionKeyCredentialPolicy` для проверки подлинности HTTP-запросов для Azure Maps с помощью ключа подписки. `atlas.service.MapsURL.newPipeline()` принимает политику `SubscriptionKeyCredential` и создает экземпляр [конвейера](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.pipeline?view=azure-maps-typescript-latest). `searchURL` представляет собой URL-адрес к операциям [поиска](https://docs.microsoft.com/rest/api/maps/search) службы Azure Maps.

2. Затем, чтобы создать поисковый запрос, добавьте следующий блок сценария. В нем используется служба поиска нечетких соответствий, которая является базовым API поиска Службы поиска. Служба поиска нечетких соответствий обрабатывает большинство нечетких входных данных, например адреса, места и точки интереса (POI). Этот код выполняет поиск ближайших бензозаправочных станций для заданных координат широты и долготы в пределах указанного адреса. Затем с помощью метода `geojson.getFeatures()` из ответа извлекается коллекция функций GeoJSON и добавляется к источнику данных, после чего данные автоматически отображаются на карте с помощью слоя символов. Последняя часть сценария устанавливает представление с камер на карте, используя ограничивающий прямоугольник с результатами, с помощью свойства карты [setCamera](/javascript/api/azure-maps-control/atlas.map#setcamera-cameraoptions---cameraboundsoptions---animationoptions-).

    ```JavaScript
    var query =  'gasoline-station';
    var radius = 9000;
    var lat = 47.64452336193245;
    var lon = -122.13687658309935;

    searchURL.searchPOI(atlas.service.Aborter.timeout(10000), query, {
        limit: 10,
        lat: lat,
        lon: lon,
        radius: radius
    }).then((results) => {

        // Extract GeoJSON feature collection from the response and add it to the datasource
        var data = results.geojson.getFeatures();
        datasource.add(data);

        // set camera to bounds to show the results
        map.setCamera({
            bounds: data.bbox,
            zoom: 10
        });
    });
    ```

3. Сохраните файл **MapSearch.html** и обновите страницу в браузере. Теперь на карте будет крупным планом показан Сиэтл, а круглые синие пометки указывают расположение бензозаправочных станций в области.

   ![Просмотр карты с результатами поиска](./media/tutorial-search-location/pins-map.png)

4. Вы можете увидеть необработанные данные, которые отображаются на карте в преобразованном для просмотра виде, введя следующий HTTP-запрос в вашем браузере. Замените \<Your Azure Maps Key\> на первичный ключ.

   ```http
   https://atlas.microsoft.com/search/poi/json?api-version=2&query=gasoline%20station&subscription-key=<subscription-key>&lat=47.6292&lon=-122.2337&radius=100000
   ```

На этом этапе страница MapSearch может отображать расположения точек интереса, возвращаемых из запроса нечеткого поиска. Давайте добавим некоторые интерактивные возможности и дополнительные сведения о расположениях.

## <a name="add-interactive-data"></a>Добавление интерактивных данных

Карта, которую мы создали, учитывает только данные широты и долготы в результатах поиска. Если взглянуть на необработанные данные JSON, которые возвращает служба поиска "Карты Azure", вы увидите, что они содержат дополнительные сведения о каждой заправочной станции, включая название и адрес. Эти данные можно включить в карту с помощью интерактивных всплывающих окон.

1. Добавьте приведенные ниже строки кода в обработчик событий карты `ready` после кода для запроса службы поиска нечетких соответствий. Этот процесс создаст экземпляр всплывающего окна и добавит событие наведения указателя мыши на слой символов.

    ```JavaScript
   //Create a popup but leave it closed so we can update it and display it later.
    popup = new atlas.Popup();

    //Add a mouse over event to the result layer and display a popup when this event fires.
    map.events.add('mouseover', resultLayer, showPopup);
    ```

    `*atlas.Popup` API предоставляет информационное окно, которое можно привязать к нужной позиции на карте. 

2. В функции `GetMap` добавьте следующий код для отображения во всплывающем окне событий наведения указателя мыши поверх сведений о результатах.

    ```JavaScript
    function showPopup(e) {
        //Get the properties and coordinates of the first shape that the event occured on.

        var p = e.shapes[0].getProperties();
        var position = e.shapes[0].getCoordinates();

        //Create HTML from properties of the selected result.
        var html = ['<div style="padding:5px"><div><b>', p.poi.name,
            '</b></div><div>', p.address.freeformAddress,
            '</div><div>', position[1], ', ', position[0], '</div></div>'];

        //Update the content and position of the popup.
        popup.setPopupOptions({
            content: html.join(''),
            position: position
        });

        //Open the popup.
        popup.open(map);
    }
    ```

3. Сохраните файл и обновите страницу в браузере. Теперь на карте в браузере при наведении указателя мыши на любой показанный маркер поиска отображаются информационные всплывающие элементы.

    ![Azure Map Control и служба поиска](./media/tutorial-search-location/popup-map.png)

## <a name="next-steps"></a>Дополнительная информация

Из этого руководства вы узнали, как выполнить следующие задачи:

> [!div class="checklist"]
> * Создание учетной записи службы "Карты Azure"
> * Получение первичного ключа для учетной записи
> * Создание веб-страницы с помощью API Map Control.
> * Использование службы поиска для поиска ближайшего объекта.

> [!div class="nextstepaction"]
> [Просмотр полного исходного кода](https://github.com/Azure-Samples/AzureMapsCodeSamples/blob/master/AzureMapsCodeSamples/Tutorials/search.html)

> [!div class="nextstepaction"]
> [Просмотр примера, работающего в реальном времени](https://azuremapscodesamples.azurewebsites.net/?sample=Search%20for%20points%20of%20interest)

В следующем руководстве показано, как отобразить маршрут между двумя расположениями.

> [!div class="nextstepaction"]
> [Прокладывание маршрута до точки интереса в службе "Карты Azure"](./tutorial-route-location.md)
