---
title: Ограничения уровней и номеров SKU в службе "Поиск Azure"
description: Ограничения службы поиска Azure, используемые при планировании мощности, а также ограничения запросов и ответов на них.
author: HeidiSteen
manager: cgronlun
services: search
ms.service: search
ms.devlang: NA
ms.topic: conceptual
ms.date: 07/01/2019
ms.author: heidist
ms.custom: seodec2018
ms.openlocfilehash: 35beb55d7326b954a568a377b73696fe598742c5
ms.sourcegitcommit: e72073911f7635cdae6b75066b0a88ce00b9053b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/19/2019
ms.locfileid: "68348298"
---
# <a name="service-limits-in-azure-search"></a>Ограничения поиска Azure
Максимальные ограничения на хранилище, рабочие нагрузки и количество индексов, документов и других объектов зависят от того, [подготавливаются ли службы поиска Azure](search-create-service-portal.md) на уровне " **бесплатный**", " **базовый**", " **стандартный**" или "оптимизированный для **хранилища** " ценовой уровень.

+ **Бесплатный** представляет собой мультитенантную службу, которая предоставляется вместе с подпиской Azure.

+ Уровень **Базовый** предоставляет выделенные вычислительные ресурсы для небольших рабочих нагрузок в рабочей среде.

+ Для уровня **Стандартный** используются выделенные виртуальные машины. Это обеспечивает увеличение емкости хранилища и вычислительной мощности на всех уровнях. В категории "Стандартный" доступно четыре уровня: S1, S2, S3 и S3 HD (S3, высокая плотность).

+ **Оптимизация хранилища** выполняется на выделенных компьютерах с более общим хранилищем, пропускной способностью хранилища и памятью, чем **стандартные**. Оптимизация хранилища достигается на двух уровнях: L1 и L2

> [!NOTE]
> По состоянию на 1 июля все уровни общедоступны, включая уровень, оптимизированный для хранилища. Все цены можно найти на странице [сведений о ценах](https://azure.microsoft.com/pricing/details/search/) .

  Уровень S3 HD (S3, высокая плотность) разработан для рабочих нагрузок с особыми характеристиками: [мультитенантность](search-modeling-multitenant-saas-applications.md) и большое количество небольших индексов (до миллиона документов на индекс, до трех тысяч индексов на каждую службу). На этом уровне не предоставляется [компонент индексатора](search-indexer-overview.md). На уровне S3 HD прием данных необходимо выполнять через механизм push, то есть передавать данные из источника в индекс с помощью вызовов API. 

> [!NOTE]
> Служба подготавливается на определенном уровне. Чтобы перейти к другому уровню для получения дополнительной емкости, нужно подготовить новую службу (обновление на месте не поддерживается). Дополнительные сведения см. в статье [Выбор SKU или ценовой категории для службы поиска Azure](search-sku-tier.md). Чтобы узнать больше о настройке емкости в уже подготовленной службе, ознакомьтесь с разделом [Масштабирование уровней ресурсов для рабочих нагрузок запросов и индексирования в Поиске Azure](search-capacity-planning.md).
>

## <a name="subscription-limits"></a>Ограничения подписки
[!INCLUDE [azure-search-limits-per-subscription](../../includes/azure-search-limits-per-subscription.md)]

## <a name="storage-limits"></a>Ограничения хранилища
[!INCLUDE [azure-search-limits-per-service](../../includes/azure-search-limits-per-service.md)]

<a name="index-limits"></a>

## <a name="index-limits"></a>Ограничения индексов

| Resource | Бесплатный | Базовый&nbsp;<sup>1</sup>  | S1 | S2 | S3 | S3&nbsp;HD | L1 | L2 |
| -------- | ---- | ------------------- | --- | --- | --- | --- | --- | --- |
| Максимальное число индексов |3 |5 или 15 |50 |200 |200 |1000 на секцию или 3000 на службу |10 |10 |
| Максимальное число простых полей на индекс |1000 |100 |1000 |1000 |1000 |1000 |1000 |1000 |
| Максимум сложных полей коллекций на индекс |40 |40 |40 |40 |40 |40 |40 |40 |
| Максимальное число элементов во всех сложных коллекциях на документ |3000 |3000 |3000 |3000 |3000 |3000 |3000 |3000 |
| Максимальная глубина сложных полей |10 |10 |10 |10 |10 |10 |10 |10 |
| Максимальное число [средств подбора](https://docs.microsoft.com/rest/api/searchservice/suggesters) на индекс |1 |1 |1 |1 |1 |1 |1 |1 |
| Максимальное число [профилей оценки](https://docs.microsoft.com/rest/api/searchservice/add-scoring-profiles-to-a-search-index) на индекс |100 |100 |100 |100 |100 |100 |100 |100 |
| Максимальное число функций на профиль |8 |8 |8 |8 |8 |8 |8 |8 |

<sup>1</sup> базовые службы, созданные до декабря 2017, имеют низкие пределы (5 вместо 15) на индексах. Уровень "Базовый" — единственный SKU с ограничением в минимум 100 полей на индекс.

<a name="document-limits"></a>

## <a name="document-limits"></a>Ограничения документов 

По состоянию на октябрь 2018 года больше нет никаких ограничений для новых служб, созданных на каком либо платном уровне (Basic, S1, S2, S3, S3 HD) в любом регионе. Несмотря на то, что с ноября по декабрь 2017 года большинство регионов имели неограниченное количество документов, были пять регионов, которые продолжали накладывать ограничения на документы. В зависимости от того, когда и где вы создали службу поиска, вы можете запустить службу, которая по-прежнему накладывает ограничения на документ.

Чтобы определить, есть ли ограничения документов для вашей службы, откройте плитку "Использование" на странице общей информации о службе. Здесь, в зависимости от уровня, вы увидите применимое ограничение или отметку об отсутствии ограничений для количества документов.

  ![Плитка "Использование"](media/search-limits-quotas-capacity/portal-usage-tile.png)

### <a name="regions-previously-having-document-limits"></a>Регионы, ранее имеющие ограничения на документы

Если портал указывает на ограничение документа, ваша служба была создана в конце 2017 года или в центре обработки данных с использованием кластеров меньшей емкости для размещения служб поиска Azure:

+ Восточная Австралия
+ Восточная Азия
+ Центральная Индия
+ Западная Япония
+ Центрально-западная часть США

Для соответствующих служб применяются следующие ограничения:

|  Бесплатный | Стандартная | S1 | S2 | S3 | S3&nbsp;HD |
|-------|-------|----|----|----|-------|
|  10 000 |1&nbsp;млн |15 млн на секцию или 180 млн на службу |60 млн на секцию или 720 млн на службу |120 млн на секцию или 1,4 млрд на службу |1 млн на индекс или 200 млн на секцию |

Если служба имеет ограничения, которые блокируют вас, создать новую службу и повторно опубликуйте все содержимое в эту службу. Не существует механизма для беспрепятственного воспроизведения службы на новом оборудовании сопутствующих ресурсов.

> [!Note] 
> В службах уровня S3 с высокой плотностью, созданных в конце 2017 года, устранено ограничение в 200 миллионов документов на раздел, но ограничение в 1 миллион документов на индекс осталось.


### <a name="document-size-limits-per-api-call"></a>Ограничения размера документа на один вызов API

Максимальный размер документа при вызове API индекса составляет примерно 16 МБ.

Фактически размер документа ограничивает размер текста запроса API индекса. Так как API индекса позволяет одновременно передать пакет из нескольких документов, фактически предельный размер зависит от количества документов в таком пакете. Для пакета с одним документом максимальный размер документа составляет 16 МБ данных JSON.

Чтобы уменьшить размер документа, не забудьте исключить из запроса недоступные для запроса данные. Образы и другие двоичные данные не запрашиваются напрямую и не должны храниться в индексе. Чтобы добавить в результаты поиска недоступные для запроса данные, определите поле, недоступное для поиска, в котором хранится ссылка на URL-адрес ресурса.

## <a name="indexer-limits"></a>Ограничения индексатора

Для обеспечения баланса и стабильности службы в целом существует максимально допустимое время выполнения, но большие наборы данных могут потребовать больше времени индексирования, чем максимально допустимое. Если задание индексирования не может завершиться в течение максимально допустимого времени ожидания, попробуйте запустить его по расписанию. Планировщик сохраняет сведения о состоянии индексирования. Если запланированное задание индексирования прервано по какой-либо причине, индексатор может продолжить выполнение с того же места при следующем запланированном выполнении.


| Resource | Бесплатный&nbsp;<sup>1</sup> | Базовый&nbsp;<sup>2</sup>| S1 | S2 | S3 | S3&nbsp;HD&nbsp;<sup>3</sup>|L1 |L2 |
| -------- | ----------------- | ----------------- | --- | --- | --- | --- | --- | --- |
| Максимальное число индексаторов |3 |5 или 15|50 |200 |200 |Н/Д |10 |10 |
| Максимальное количество источников данных |3 |5 или 15 |50 |200 |200 |Н/Д |10 |10 |
| Максимальное число наборов квалификационных навыков <sup>4</sup> |3 |5 или 15 |50 |200 |200 |Н/Д |10 |10 |
| Максимальная нагрузка индексирования на вызов |10 000 документов |Ограничивается только максимальным числом документов |Ограничивается только максимальным числом документов |Ограничивается только максимальным числом документов |Ограничивается только максимальным числом документов |Н/Д |Без ограничений |Без ограничений |
| Минимальное расписание | 5 минут |5 минут |5 минут |5 минут |5 минут |5 минут |5 минут | 5 минут |
| Максимальное время выполнения<sup>5</sup> | 1–3 мин |24 часа |24 часа |24 часа |24 часа |Н/Д  |24 часа |24 часа |
| Максимальное время выполнения наборов квалификационных навыков когнитивного поиска или индексирования больших двоичных объектов с помощью анализа образов<sup>5</sup> | 3–10 минут |2 часа |2 часа |2 часа |2 часа |Н/Д  |2 часа |2 часа |
| Индексатор BLOB-объектов: максимальный размер BLOB-объектов в МБ |16 |16 |128 |256 |256 |Н/Д  |256 |256 |
| Индексатор BLOB-объектов: максимальное число символов в содержимом, извлеченном из BLOB-объекта |32 000 |64 000 |4&nbsp;млн |4&nbsp;млн |4&nbsp;млн |Н/Д |4&nbsp;млн |4&nbsp;млн |

<sup>1</sup> Максимальное время выполнения индексатора для служб уровня "Бесплатный" составляет 3 минуты для источников больших двоичных объектов или 1 минуту для прочих источников данных. Для индексирования искусственного интеллекта, которое вызывает Cognitive Services, бесплатные службы ограничены 20 свободными транзакциями в день, где транзакция определяется как документ, успешно прошедший через конвейер обогащения.

<sup>2</sup> базовые службы, созданные до декабря 2017, имеют более низкие пределы (5 вместо 15) на индексаторах, источниках данных и навыков.

<sup>3</sup> Службы уровня S3 HD не поддерживают индексаторы.

<sup>4</sup> До 30 навыков на набор квалификационных навыков.

<sup>5</sup>. Рабочие нагрузки когнитивного поиска и анализ образов в индексировании больших двоичных объектов Azure выполняются быстрее, чем обычное индексирование текста. Анализ образов и обработка естественного языка требуют интенсивных вычислений и потребляют непропорциональные объемы доступных вычислительных мощностей. Время выполнения было уменьшено, чтобы могли выполняться другие задания в очереди.  

## <a name="queries-per-second-qps"></a>Число запросов в секунду (QPS)

Оценка QPS должна отдельно проводиться каждым клиентом. Основными определителями QPS являются размер и сложность индексов, размер и сложность запросов, а также объем трафика. Если такие факторы неизвестны, оценка не будет информативной.

Прогнозируемость показателей повышается, если оцениваются службы, работающие в выделенных ресурсах (категорий "Базовый" и "Стандартный"). Можно точнее оценить QPS, так как вы можете контролировать больше параметров. Рекомендации по оценке см. в разделе о [производительности и оптимизации службы "Поиск Azure"](search-performance-optimization.md).

Для уровней, оптимизированных для хранилища, следует рассчитывать на меньшую пропускную способность запросов и более высокую задержку по сравнению со стандартными уровнями.  Методология оценки производительности запросов, которую вы будете испытывать, аналогична стандартным уровням.

## <a name="data-limits-cognitive-search"></a>Ограничения данных (когнитивный поиск)

Ограничение данных применяется к [конвейеру когнитивного поиска](cognitive-search-concept-intro.md), который обращается к ресурсу Анализа текста для [распознавания сущностей](cognitive-search-skill-entity-recognition.md), [извлечения ключевых фраз](cognitive-search-skill-keyphrases.md), [анализа тональности](cognitive-search-skill-sentiment.md) и [распознавания языка](cognitive-search-skill-language-detection.md). Максимальный размер записи должен составлять 50 000 символов, [`String.Length`](https://docs.microsoft.com/dotnet/api/system.string.length)измеряемый. Если вам нужно разбить данные перед отправкой в анализатор тональности, можно воспользоваться [навыком разделения текста](cognitive-search-skill-textsplit.md).

## <a name="api-request-limits"></a>Ограничения запросов API
* Максимум 16 МБ на один запрос <sup>1</sup>
* Максимальная длина URL-адреса — 8 КБ.
* Максимум 1000 документов на одну операцию загрузки индексов, объединения или удаления.
* Максимум 32 поля в предложении $orderby.
* Максимальный размер поискового запроса — 32 766 байтов (32 КБ минус 2 байта) текста в кодировке UTF-8.

<sup>1</sup> В Поиске Azure размер текста запроса должен быть не больше 16 МБ. Это накладывает фактическое ограничение на отдельные поля или коллекции, для которых теоретические ограничения не установлены (дополнительные сведения о составе и ограничениях полей см. в разделе [Supported data types (Azure Search)](https://docs.microsoft.com/rest/api/searchservice/supported-data-types) (Поддерживаемые типы данных (Поиск Azure))).

## <a name="api-response-limits"></a>Ограничения ответа API
* Максимум 1000 документов на одну страницу результатов поиска.
* Максимум 100 предложений на один запрос API предложений.

## <a name="api-key-limits"></a>Ограничения ключей API
Ключи API используются для проверки подлинности служб. Существует два типа ключей. Ключи администратора указываются в заголовке запроса и предоставляют доступ к службе на чтение и запись. Ключи запросов доступны только для чтения, они указываются в URL-адресе и обычно передаются в клиентские приложения.

* Максимум 2 ключа администратора на одну службу.
* Максимум 50 ключей запросов на одну службу.
