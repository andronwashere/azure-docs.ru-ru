---
title: Использование Azure Media Clipper на портале | Документация Майкрософт
description: Создание клипов на портале Azure с помощью Azure Media Clipper
services: media-services
keywords: clip;subclip;encoding;media
author: dbgeorge
manager: jasonsue
ms.author: dwgeo
ms.date: 03/14/2019
ms.topic: article
ms.service: media-services
ms.openlocfilehash: 8c88caefb0909da55de87116a23fa520c1679cc2
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "61465853"
---
# <a name="create-clips-with-azure-media-clipper-in-the-portal"></a>Создание клипов на портале с помощью Azure Media Clipper  

С помощью средства Azure Media Clipper вы можете создавать на портале клипы из ресурсов, сохраненных в учетных записях служб мультимедиа. Чтобы начать работу, найдите на портале учетную запись служб мультимедиа. Выберите для нее вкладку **Субклип**.

На вкладке **Субклип** вы можете начать составление клипа. Средство Clipper на портале загружает MP4-файлы с одной скоростью или с несколькими скоростями, а также динамические архивы, которые публикуются с допустимым указателем потоковой передачи. Неопубликованные ресурсы не загружаются.

Clipper сейчас предоставляется в режиме общедоступной предварительной версии. Чтобы получить доступ к Clipper на портале Azure, перейдите на эту [страницу общедоступной предварительной версии](https://portal.azure.com/?feature.subclipper=true).

На следующем изображении показана целевая страница Clipper в учетной записи служб мультимедиа. ![Azure Media Clipper на портале Azure](media/media-services-azure-media-clipper-portal/media-services-azure-media-clipper-portal.png)

## <a name="producing-clips"></a>Создание клипов
Чтобы создать клип, перетащите ресурс в интерфейс клипа. Если вам известны отметки времени, их можно вручную ввести в интерфейс. В противном случае запустите воспроизведение ресурса или перетащите указатель воспроизведения, чтобы найти нужные метки начала и завершения клипа. Если вы не укажете метки начала и завершения, клип будет начинаться с самого начала исходного ресурса или продолжаться до его конца, соответственно.

Чтобы перемещаться по ресурсу с точностью до кадра и (или) GOP, используйте кнопки перехода на один кадр (GOP) вперед и на один кадр (GOP) назад. Чтобы собрать клип из нескольких ресурсов, перетащите все нужные ресурсы из панели выбора ресурсов в интерфейс клипа. Вы можете выбрать и переместить любой ресурс в интерфейсе, чтобы изменить их порядок. Область выбора ресурсов предоставляет метаданные о длительности, типе и разрешении каждого ресурса. При объединении нескольких ресурсов учитывайте исходное разрешение каждого исходного файла. Если разрешения нескольких источников различаются, файлы с низким разрешением будут конвертироваться в самое высокое разрешение из всех исходных ресурсов. Чтобы просмотреть результат задания обрезки, выберите кнопку просмотра, которая воспроизводит клип, заданный выбранными метками времени.

## <a name="producing-dynamic-manifest-filters"></a>Создание фильтров динамических манифестов
[Фильтры динамических манифестов](https://azure.microsoft.com/blog/dynamic-manifest/) описывают наборы правил, основанных на атрибутах манифеста и временной шкале ресурсов. Эти правила определяют, как конечная точка потоковой передачи управляет списком воспроизведения (манифестом). Фильтр позволяет изменить сегменты, назначенные для потокового воспроизведения. Фильтры, созданные средством Clipper, применяются локально и только для конкретного исходного ресурса. В отличие от созданных клипов, фильтры не являются новыми ресурсами и для их создания не требуется задание кодирования. Их можно создать очень быстро с помощью [пакета .NET SDK](https://docs.microsoft.com/azure/media-services/media-services-dotnet-dynamic-manifest) или [API-интерфейса REST](https://docs.microsoft.com/azure/media-services/media-services-rest-dynamic-manifest), но только с точностью до уровня GOP. Обычно в ресурсах, закодированных для потоковой передачи, GOP имеет размер в две секунды.

Чтобы создать фильтр динамического манифеста, перейдите на вкладку **Ресурсы** и выберите нужный ресурс. Нажмите кнопку **Субклип** в верхнем меню. В меню дополнительных параметров выберите режим обрезки "Фильтр динамического манифеста". Затем, чтобы создать фильтр, примените ту же процедуру, что и для создания клипа, преобразованного для просмотра. Фильтры можно создавать только из одного ресурса.

На следующем изображении показан Clipper в режиме фильтрации динамического манифеста на портале Azure. ![Azure Media Clipper в режиме фильтрации динамического манифеста на портале Azure](media/media-services-azure-media-clipper-portal/media-services-azure-media-clipper-filter.PNG)

## <a name="submitting-clipping-jobs"></a>Отправка заданий обрезки
Когда вы закончите составление клипа, нажмите кнопку отправки задания, которая запускает соответствующее задание обрезки или вызывает динамический манифест.

## <a name="next-steps"></a>Дальнейшие действия
Чтобы начать работу с Azure Media Clipper, прочитайте статью о [начале работы](media-services-azure-media-clipper-getting-started.md). Из нее вы узнаете, как развернуть мини-приложение.
