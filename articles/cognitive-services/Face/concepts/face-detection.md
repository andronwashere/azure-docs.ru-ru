---
title: Обнаружение лиц и основные понятия атрибуты
titleSuffix: Azure Cognitive Services
description: Сведения о концепциях об обнаружении лиц и черт лица.
services: cognitive-services
author: PatrickFarley
manager: nitime
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: conceptual
ms.date: 04/26/2019
ms.author: pafarley
ms.openlocfilehash: e61048eeab9d7061c18f3237db22fc87ca52f526
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "65891149"
---
# <a name="face-detection-and-attributes"></a>Обнаружение лиц и атрибуты

В этой статье описываются основные понятия обнаружение лиц и данные атрибутов лиц. Обнаружение лиц, — это поиск человеческих лиц на изображении и при необходимости возврата различные виды данных, связанных с лиц.

Использовании [лиц — обнаруживать](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) операции для обнаружения лиц на изображении. Как минимум каждого обнаруженного лица соответствует полю faceRectangle в ответе. Этот набор координаты в пикселях для левую, верхнюю, ширины и высоты пометить расположения лиц. Используя эти координаты, можно получить расположение лиц и его размер. В ответ API лиц, перечислены в порядке размер от наибольшего к наименьшему.

## <a name="face-id"></a>Идентификатор лица

Face ID является строкой уникального идентификатора для каждого обнаруженного лица на изображении. Вы можете запросить face ID в вашей [лиц — обнаруживать](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) вызова API.

## <a name="face-landmarks"></a>Ориентиры лица

Позу представляют собой набор точек для поиска на грани, например учеников или кончика нос. По умолчанию используются 27 предопределенных точек ориентиров. На следующем рисунке показана все 27 точки:

![Схема лиц с все 27 ориентиров с меткой](../Images/landmarks.1.jpg)

Координаты точки возвращаются в единицах точек.

## <a name="attributes"></a>Атрибуты

Атрибуты представляют собой набор функций, которые при необходимости могут определяться с помощью [лиц — обнаруживать](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) API. Могут быть обнаружены следующие атрибуты:

* **Возраст**. Предполагаемый возраст в годах конкретного лица.
* **Размытие**. Размытости лица на изображении. Этот атрибут возвращает значение между 0 и один и неофициальные оценка низкий, средний или высокий.
* **Определение эмоций**. Список эмоции с уверенностью их обнаружения для данного лица. Нормализуются достоверности и показатели по всем эмоции добавьте больше одного. Распознаваемые эмоции возвращаются, счастье, печаль, neutral, гнев, презрение, отвращение, неожиданные и страха.
* **Раскрытие**. Раскрытие лица на изображении. Этот атрибут возвращает значение между 0 и один и неофициальные оценка underExposure, goodExposure или изображения.
* **Улыбка**. Наличие предполагаемое улыбка и длина для данного лица.
* **Пол**. Предполагаемый Пол лицом. Возможные значения: Мужской, Женский и genderless.
* **Очков**. Имеет ли данная грань очков. Возможные значения: NoGlasses, ReadingGlasses, очков и Swimming Goggles.
* **Волосы**. Тип волосы поверхности. Этот атрибут показывает, виден ли волосы, обнаруживается ли baldness и обнаруживаются набор цветов, волосы.
* **HEAD представлять**. Начертание шрифта положение в трехмерном пространстве. Этот атрибут описывается шаг наката и нутации углы в градусах. Диапазоны значений — от-90 градусов в 90 градусов, - 180 градусов, 180 градусов и градусов-90 до 90 градусов, соответственно. См. на следующей схеме сопоставления угол.

    ![Head с высоты тона, отката и поворот оси с меткой](../Images/headpose.1.jpg)
* **Состав**. Начертание шрифта, имеет ли состав. Этот атрибут возвращает значение типа Boolean для eyeMakeup и lipMakeup.
* **Пропускаемые**. Visual ошибочных оповещений, обнаружено в лицо. Этот атрибут возвращает значение между 0 и один и неофициальные оценка низкий, средний или высокий.
* **Перекрытия**. Существует ли объектов блокировки части поверхности. Этот атрибут возвращает значение типа Boolean для eyeOccluded foreheadOccluded и mouthOccluded.
* **Одобрения**. Выражение одобрение данного лица. Это значение находится между ноль для без одобрения и один для очистки одобрение.

> [!IMPORTANT]
> Благодаря использованию статистических алгоритмов по прогнозу черт лица. Они могут быть неточными. Будьте осторожны при принятии решений на основе данных атрибута.

## <a name="input-data"></a>Входные данные

Следуйте приведенным ниже советам, чтобы убедиться в том, что входных изображений предоставить наиболее точные результаты обнаружения:

* Поддерживаемые входные форматы изображения, JPEG, PNG, GIF, первый кадр и BMP.
* Размер изображения должен быть не больше 4 МБ.
* Диапазон определяемого размера лица составляет от 36 х 36 до 4096 х 4096 пикселей. Не будет обнаружено лица за пределами этого диапазона.
* Некоторые лица могут не обнаруживаться из-за технических проблем. Extreme лицевой угол (головной поза) или перекрытия лиц (объекты, например очков или руки, блокирующих части поверхности) может повлиять на обнаружение. Анфас или практически в анфас лиц выдают лучшие результаты.

Если обнаружение лиц из видео веб-канала, можно повысить производительность путем настройки определенных параметров камеры видео:

* **Сглаживание**: Многие видеокамеры применить сглаживающий эффект. Если это возможно, поскольку он создает размытия между кадрами и уменьшает ясности следует выключить это свойство.
* **Скорость затвора**: Ускорить обработку затвора снизить количество движения между кадрами и каждого кадра яснее. Мы рекомендуем скорости затвора второй 1/60 или более.
* **Затвора угол**: Некоторые камеры укажите угол затвора вместо выдержка. По возможности следует использовать нижний угол затвора. Это приведет к более ясной кадров видео.

    >[!NOTE]
    > Камеры с нижней угол затвора получит меньше свет в каждом кадре, изображение будет темнее. Вам потребуется для определения необходимого уровня для использования.

## <a name="next-steps"></a>Дальнейшие действия

Теперь, когда вы знакомы с основными понятиями обнаружения лиц, вы можете научиться писать сценарий, который определяет лиц в данного изображения.

* [Определение лиц на изображении](../Face-API-How-to-Topics/HowtoDetectFacesinImage.md)