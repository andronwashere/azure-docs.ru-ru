---
title: Руководство по Обрезка изображения при помощи SDK визуального поиска Bing
description: Используйте пакет SDK визуального поиска Bing, чтобы извлекать ценные сведения из определенных областей изображения.
services: cognitive-services
titleSuffix: Azure Cognitive Services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-visual-search
ms.topic: article
ms.date: 04/26/2019
ms.author: rosh
ms.openlocfilehash: a65994ffb2fc707c59e507cad71699211b6a64a5
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "65896271"
---
# <a name="tutorial-crop-an-image-with-the-bing-visual-search-sdk-for-c"></a>Руководство по Обрезка изображения при помощи SDK визуального поиска Bing — C#

Пакет SDK Bing Visual Search позволяет обрезать изображение, прежде чем удастся найти похожих изображений online. Это приложение вырезает изображение человека со снимка, содержащего несколько человек, и затем возвращает результаты, содержащие похожие изображения, найденные в Интернете.

Полный исходный код этого приложения с дополнительными возможностями по обработке ошибок и аннотациями можно получить на [GitHub](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/blob/master/Tutorials/Bing-Visual-Search/BingVisualSearchCropImage.cs).

Учебное приложение иллюстрирует приведенные ниже задачи.

> [!div class="checklist"]
> * Отправка запроса при помощи пакета SDK визуального поиска Bing
> * Обрезка области изображения для поиска с помощью визуального поиска Bing
> * Отправка запроса и обработка ответа
> * Поиск URL-адреса действий в ответ

## <a name="prerequisites"></a>Технические условия

* Любой выпуск [Visual Studio 2019](https://www.visualstudio.com/downloads/).
* Если вы используете Linux или MacOS, это приложение можно запустить с помощью [Mono](https://www.mono-project.com/).
* Установленный пакет [NuGet для пользовательского поиска](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Search.CustomSearch/1.2.0).
    - В обозревателе решений в Visual Studio, щелкните правой кнопкой мыши проект и выберите **управление пакетами NuGet** в меню. Установите пакет `Microsoft.Azure.CognitiveServices.Search.CustomSearch`. При установке пакета NuGet для пользовательского поиска также будут установлены следующие сборки:
        - Microsoft.Rest.ClientRuntime
        - Microsoft.Rest.ClientRuntime.Azure
        - Newtonsoft.Json.

[!INCLUDE [cognitive-services-bing-image-search-signup-requirements](../../../includes/cognitive-services-bing-visual-search-signup-requirements.md)]

## <a name="specify-the-image-crop-area"></a>Укажите обрезаемую область изображения.

Это приложение обрезает область изображения группы разработчиков Майкрософт. В этой области обрезки определяется с помощью координат верхнего левого и правого нижнего, представленное в виде процента от всего изображения:  

![Группа представителей высшего руководства корпорации Майкрософт](./media/MS_SrLeaders.jpg)

Это изображение обрезается путем создания объекта `ImageInfo` из области обрезки и загрузки `ImageInfo` в `VisualSearchRequest`. `ImageInfo` Объект также включает URL-адрес изображения:

```csharp
CropArea CropArea = new CropArea(top: (float)0.01, bottom: (float)0.30, left: (float)0.01, right: (float)0.20);
string imageURL = "https://docs.microsoft.com/azure/cognitive-services/bing-visual-search/media/ms_srleaders.jpg";
ImageInfo imageInfo = new ImageInfo(cropArea: CropArea, url: imageURL);

VisualSearchRequest visualSearchRequest = new VisualSearchRequest(imageInfo: imageInfo);
```

## <a name="search-for-images-similar-to-the-crop-area"></a>Поиск изображений, аналогичных области обрезки

Переменная `VisualSearchRequest` содержит сведения об области обрезки изображения и его URL-адрес. `VisualSearchMethodAsync()` Метод возвращает результаты:

```csharp
Console.WriteLine("\r\nSending visual search request with knowledgeRequest that contains URL and crop area");
var visualSearchResults = client.Images.VisualSearchMethodAsync(knowledgeRequest: visualSearchRequest).Result;

```

## <a name="get-the-url-data-from-imagemoduleaction"></a>Получить данные из URL-адрес `ImageModuleAction`

Результатами визуального поиска Bing являются объекты `ImageTag`. Каждый тег содержит список объектов `ImageAction`. Каждый `ImageAction` содержит `Data` поля, что список значений, которые зависят от типа действия.

С помощью следующего кода можно печатать различные типы.

```csharp
Console.WriteLine("\r\n" + "ActionType: " + i.ActionType + " -> WebSearchUrl: " + i.WebSearchUrl);
```

Готовое приложение возвращает:

|Тип действия  |URL-адрес  | |
|---------|---------|---------|
|PagesIncluding WebSearchURL     |         |
|MoreSizes WebSearchURL     |         |  
|VisualSearch WebSearchURL    |         |
|ImageById WebSearchURL     |         |  
|RelatedSearches WebSearchURL     |         |
|Entity -> WebSearchUrl     | https://www.bing.com/cr?IG=E40D0E1A13404994ACB073504BC937A4&CID=03DCF882D7386A442137F49BD6596BEF&rd=1&h=BvvDoRtmZ35Xc_UZE4lZx6_eg7FHgcCkigU1D98NHQo&v=1&r=https%3a%2f%2fwww.bing.com%2fsearch%3fq%3dSatya%2bNadella&p=DevEx,5380.1        |
|TopicResults -> WebSearchUrl    |  https://www.bing.com/cr?IG=E40D0E1A13404994ACB073504BC937A4&CID=03DCF882D7386A442137F49BD6596BEF&rd=1&h=3QGtxPb3W9LemuHRxAlW4CW7XN4sPkUYCUynxAqI9zQ&v=1&r=https%3a%2f%2fwww.bing.com%2fdiscover%2fnadella%2bsatya&p=DevEx,5382.1        |
|ImageResults -> WebSearchUrl    |  https://www.bing.com/cr?IG=E40D0E1A13404994ACB073504BC937A4&CID=03DCF882D7386A442137F49BD6596BEF&rd=1&h=l-WNHO89Kkw69AmIGe2MhlUp6MxR6YsJszgOuM5sVLs&v=1&r=https%3a%2f%2fwww.bing.com%2fimages%2fsearch%3fq%3dSatya%2bNadella&p=DevEx,5384.1        |

Как показано выше, тип действия `Entity` содержит запрос поиска Bing, который возвращает сведения о распознаваемом человеке, месте или предмете. Типы `TopicResults` и `ImageResults` содержат запросы для связанных изображений. URL-адреса в списке ведут на результаты поиска Bing.

## <a name="get-urls-for-pagesincluding-actiontype-images"></a>Получить URL-адреса для `PagesIncluding` `ActionType` образов

Для получения URL-адресов фактических изображений требуется приведение к типу, который считывает `ActionType` как `ImageModuleAction`, содержащий элемент `Data` со списком значений. Каждое значение является URL-адресом изображения. Следующих приведений `PagesIncluding` тип действия, чтобы `ImageModuleAction` и считывает значения:

```csharp
    if (i.ActionType == "PagesIncluding")
    {
        foreach(ImageObject o in (i as ImageModuleAction).Data.Value)
        {
            Console.WriteLine("ContentURL: " + o.ContentUrl);
        }
    }
```

## <a name="next-steps"></a>Дальнейшие действия
> [!div class="nextstepaction"]
> [Create a Visual Search single-page web app](tutorial-bing-visual-search-single-page-app.md) (Создание одностраничного веб-приложения Визуального поиска)

## <a name="see-also"></a>См. также
> [Что такое API визуального поиска Bing?](https://docs.microsoft.com/azure/cognitive-services/bing-visual-search/overview)
