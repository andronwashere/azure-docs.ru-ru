---
title: Медиа-аналитика на платформе служб мультимедиа | Документация Майкрософт
description: Обзор общедоступной предварительной версии служб медиа-аналитики — коллекции голосовых служб и служб компьютерного зрения для обеспечения соответствия нормам, безопасности и предоставления глобального доступа на корпоративном уровне.
services: media-services
documentationcenter: ''
author: juliako
manager: femila
editor: ''
ms.assetid: c56e3781-8510-4f7f-b5ff-a218c1bb6f4c
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 03/14/2019
ms.author: milanga;juliako;johndeu
ms.openlocfilehash: ceaf4d3db71d99c3e87157f9847312fdf4000026
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "65991790"
---
# <a name="media-analytics-on-the-media-services-platform"></a>Медиа-аналитика на платформе служб мультимедиа 

## <a name="overview"></a>Обзор
Все больше организаций выбирают видео в качестве предпочтительного средства для обучения сотрудников, взаимодействия с клиентами и документирования бизнес-функций. Облачные вычисления обеспечивают хранение, потоковую передачу и доступ к этим большим файлам мультимедиа. Но по мере увеличения библиотеки видео компании возникает необходимость в столь же эффективных средствах извлечения аналитики из содержимого. 

Для удовлетворения этой растущей потребности службы мультимедиа Azure предлагают службы медиа-аналитики Azure. Службы медиа-аналитики включают в себя набор речевых и визуальных компонентов, которые помогают организациям и компаниям выполнять анализ видеофайлов. Службы медиа-аналитики созданы на базе основных компонентов платформы служб мультимедиа Azure и, следовательно, поддерживают обработку мультимедийных данных объемом в один день.

С помощью медиа-аналитики разработчики могут быстро внедрить расширенные функциональные возможности видео в приложения. Медиа-аналитика предназначена для использования в корпоративных средах для выполнения таких требований крупных организаций, как обеспечение соответствия требованиям, безопасности и предоставление глобального доступа.

На следующей схеме показана медиа-аналитика и другие важные компоненты платформы служб мультимедиа. 

![Рабочий процесс для видео по запросу](./media/media-services-analytics-overview/media-services-analytics-overview01.png)

Обработчики мультимедийных данных служб медиа-аналитики создают файлы в формате MP4 или JSON. Если обработчик мультимедиа создает MP4-файл, такой файл можно загрузить поэтапно. Если обработчик мультимедиа создает JSON-файл, такой файл можно скачать из хранилища BLOB-объектов. 

## <a name="media-analytics-services"></a>Службы медиа-аналитики

### <a name="indexer"></a>Индексатор
Индексатор мультимедийных данных Azure позволяет сделать содержимое доступным для поиска, а также создавать дорожки скрытых субтитров. По сравнению с предыдущей версией индексатор мультимедийных данных Azure 2 (предварительная версия) быстрее выполняет индексирование и предлагает более широкую поддержку языков. В число поддерживаемых языков входят английский, испанский, французский, немецкий, итальянский, китайский, португальский и арабский. Дополнительные сведения и примеры приведены в статье [Индексирование файлов мультимедиа с помощью индексатора мультимедийных данных Azure 2 (предварительная версия)](media-services-process-content-with-indexer2.md).
### <a name="motion-detector"></a>Детектор движения
Motion Detector можно использовать для обнаружения движения на видео с неподвижным фоном. Это дает возможность проверять ложные срабатывания камер наблюдения с функцией обнаружения движения. Дополнительные сведения и примеры приведены в статье [Обнаружение движения с помощью медиа-аналитики Azure](media-services-motion-detection.md).
### <a name="face-detector"></a>Обнаружение лиц
Face Detector позволяет обнаруживать лица людей и определять их эмоции, включая радость, грусть и удивление. Эту службу целесообразно использовать в ряде описанных ниже областей, включая объединение и анализ реакции людей, принимающих участие в мероприятии. Дополнительные сведения и примеры приведены в статье [Обнаружение лиц и определение эмоций с помощью медиа-аналитики Azure](media-services-face-and-emotion-detection.md).
### <a name="video-summarization"></a>Сводные видео
Формирование сводных данных видео помогает создавать краткие видеоролики длинных видеозаписей путем автоматического выбора интересных фрагментов из исходного видеоматериала. Используйте эту функцию, чтобы представить краткий обзор длинного видео. Дополнительные сведения и примеры приведены в статье [Использование Azure Media Video Thumbnail для формирования сводных данных видео](media-services-video-summarization.md).
### <a name="optical-character-recognition"></a>Оптическое распознавание символов
Функция OCR (оптическое распознавание символов) служб медиа-аналитики Azure позволяет преобразовывать текстовое содержимое в видеофайлах в редактируемый и поддерживающий поиск цифровой текст. Это позволяет автоматически извлекать значимые метаданные из видеосигнала файлов мультимедиа.
### <a name="scalable-face-redaction"></a>Масштабируемое скрытие лиц
Azure Media Redactor — это обработчик медиа-аналитики Azure с возможностью масштабируемого скрытия лиц в облаке. Функция скрытия лиц позволяет изменять видео, размывая изображения лиц выбранных людей. Вы можете использовать службу скрытия лиц в ситуациях, требующих соблюдения общественной безопасности, а также при работе с новостями. Редактирование короткого материала с несколькими лицами вручную может занять несколько часов, тогда как с помощью этой функции для скрытия лиц достаточно выполнить несколько простых действий. Дополнительные сведения см. в статье [Скрытие лиц с помощью аналитики мультимедиа Azure](media-services-face-redaction.md).
### <a name="content-moderation"></a>Модерация содержимого
Azure Content Moderator позволяет использовать машинные средства модерации для видео. Например, вашим специалистам-модераторам может потребоваться обнаружить содержимое для взрослых и непристойные материалы в видео и просмотреть помеченное содержимое. Ручная модерация видео на предмет нежелательного содержимого отнимает много времени и средств. С этой службой и связанными инструментами просмотра вы можете объединить машинную модерацию с возможностями специалистов и эффективно и экономически выгодно получить лучшие результаты. Дополнительные сведения см. в статье об [обработке видео с помощью Azure Content Moderator](media-services-content-moderation.md).

## <a name="common-scenarios"></a>Распространенные сценарии
Службы медиа-аналитики помогают организациям и предприятиям получать новую ценную информацию из видео и более эффективно управлять большими объемами видеосодержимого. Ниже приведено несколько сценариев.

* **Центры обработки вызовов**. Даже с появлением социальных сетей центры обработки вызовов клиентов по-прежнему выполняют значительную долю операций по обслуживанию клиентов. В получаемых ими звуковых данных содержится много сведений о клиентах, которые можно анализировать, чтобы повысить удовлетворенность клиентов. С помощью индексатора мультимедийных данных организации могут извлекать текст и создавать индексы поиска и панели мониторинга. Затем они могут извлекать аналитику по распространенным жалобам, источникам жалоб и другим связанным данным.
* **Модерация создаваемого пользователями содержимого**. Во многих организациях — от новостных агентств до отделений полиции — существуют общедоступные интернет-порталы, на которых размещаются создаваемые пользователями мультимедийные данные, такие как видео и изображения. При возникновении непредвиденных событий объем содержимого может резко возрастать. В таких случаях сложно эффективно вручную проверить содержимое на предмет пригодности для публикации. Клиенты могут полагаться на службу модерации, которая допускает для размещения только соответствующие материалы.

## <a name="media-analytics-media-processors"></a>Обработчики мультимедиа медиа-аналитики
В этом разделе перечислены все обработчики мультимедиа медиа-аналитики, а также показано, как использовать .NET и REST для получения объекта обработчика мультимедиа (MP).

### <a name="mp-names"></a>Названия обработчиков мультимедиа
* Azure Media Indexer 2 (предварительная версия)
* Azure Media Indexer
* Azure Media Face Detector
* Azure Media Motion Detector
* Azure Media Video Thumbnails
* Azure Media OCR
* Azure Media Content Moderator

### <a name="net"></a>.NET
Следующая функция принимает одно из указанных имен обработчиков мультимедиа и возвращает объект обработчика мультимедиа.

    static IMediaProcessor GetLatestMediaProcessorByName(string mediaProcessorName)
    {
        var processor = _context.MediaProcessors
            .Where(p => p.Name == mediaProcessorName)
            .ToList()
            .OrderBy(p => new Version(p.Version))
            .LastOrDefault();

        if (processor == null)
            throw new ArgumentException(string.Format("Unknown media processor",
                                                       mediaProcessorName));

        return processor;
    }


### <a name="rest"></a>REST
Запрос:

    GET https://media.windows.net/api/MediaProcessors()?$filter=Name%20eq%20'Azure%20Media%20OCR' HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    User-Agent: Microsoft ADO.NET Data Services
    Authorization: Bearer <token>
    x-ms-version: 2.12
    Host: media.windows.net

Ответ:

    . . .

    {  
       "odata.metadata":"https://media.windows.net/api/$metadata#MediaProcessors",
       "value":[  
          {  
             "Id":"nb:mpid:UUID:074c3899-d9fb-448f-9ae1-4ebcbe633056",
             "Description":"Azure Media OCR",
             "Name":"Azure Media OCR",
             "Sku":"",
             "Vendor":"Microsoft",
             "Version":"1.1"
          }
       ]
    }

## <a name="demos"></a>Демонстрационные материалы
Просмотрите [ролики о медиа-аналитике Azure](https://azuremedialabs.azurewebsites.net/demos/Analytics.html).

## <a name="provide-feedback"></a>Отзывы
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

## <a name="related-articles"></a>Связанные статьи
Ознакомьтесь с разделом [Общие сведения об аналитике служб мультимедиа](https://azure.microsoft.com/blog/introducing-azure-media-analytics/).

<!-- Images -->

[overview]: ./media/media-services-video-on-demand-workflow/media-services-video-on-demand.png

## <a name="next-steps"></a>Дальнейшие действия
Просмотрите схемы обучения работе со службами мультимедиа.

[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]
