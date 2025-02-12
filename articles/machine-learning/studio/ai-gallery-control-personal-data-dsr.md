---
title: Управление данными из Коллекции решений ИИ Azure
titleSuffix: Azure Machine Learning Studio
description: Можно экспортировать и удалять хранящиеся в продукте пользовательские данные из коллекции решений ИИ Azure, используя интерфейс или API каталога коллекции решений ИИ. В этой статье показано, как это сделать.
services: machine-learning
ms.service: machine-learning
ms.subservice: studio
ms.topic: conceptual
author: xiaoharper
ms.author: amlstudiodocs
ms.custom: seodec18
ms.date: 05/25/2018
ms.reviewer: jmartens, mldocs
ms.openlocfilehash: 44ff2a5b723c086604acf39e9f975deb53759ae1
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "60752051"
---
# <a name="view-and-delete-in-product-user-data-from-azure-ai-gallery"></a>Просмотр и удаление хранящихся в продукте пользовательских данных из коллекции решений ИИ Azure

Вы можете просматривать и удалять хранящиеся в продукте пользовательские данные из коллекции решений ИИ Azure, используя интерфейс или API каталога коллекции решений ИИ. В этой статье описано, каким образом это можно сделать.

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-dsr-and-stp-note.md)]

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-intro-sentence.md)]

## <a name="view-your-data-in-ai-gallery-with-the-ui"></a>Просмотр данных в коллекции решений ИИ с помощью пользовательского интерфейса

Вы можете просматривать опубликованные элементы через пользовательский интерфейс веб-сайта "Коллекция решений ИИ Azure". Пользователи могут просматривать общедоступные и частные решения, проекты, эксперименты и другие опубликованные элементы.

1.  Войдите на веб-сайт [Коллекция решений ИИ Azure](https://gallery.azure.ai/).
2.  Щелкните изображение профиля в правом верхнем углу, а затем имя учетной записи, чтобы загрузить страницу профиля.
3.  На странице профиля отображаются все элементы, опубликованные в коллекции, включая частные.

## <a name="use-the-ai-gallery-catalog-api-to-view-your-data"></a>Просмотр данных с помощью API каталога коллекции решений ИИ

Вы можете программным образом просмотреть собранные данные с помощью API каталога коллекции решений ИИ, который доступен по адресу https://catalog.cortanaanalytics.com/entities. Для просмотра данных нужен идентификатор автора. Чтобы просмотреть с помощью API каталога частные объекты, требуется маркер доступа.

Ответы каталога возвращаются в формате JSON.

### <a name="get-an-author-id"></a>Получение идентификатора автора
Идентификатор автора основан на адресе электронной почты, используемом при публикации в коллекции решений ИИ Azure. Он не изменяется.

1.  Войдите на веб-сайт [Коллекция решений ИИ Azure](https://gallery.azure.ai/).
2.  Щелкните изображение профиля в правом верхнем углу, а затем имя учетной записи, чтобы загрузить страницу профиля.
3.  В URL-адресе в адресной строке после `authorId=` содержится буквенно-цифровой идентификатор. К примеру, возьмем URL-адрес: `https://gallery.azure.ai/Home/Author?authorId=99F1F5C6260295F1078187FA179FBE08B618CB62129976F09C6AF0923B02A5BA`
        
    Идентификатор автора такой: `99F1F5C6260295F1078187FA179FBE08B618CB62129976F09C6AF0923B02A5BA`

### <a name="get-your-access-token"></a>Получение маркера доступа

Маркер доступа требуется, чтобы просмотреть с помощью API каталога частные объекты. Без маркера доступа пользователи по-прежнему могут просматривать общедоступные объекты и другую пользовательскую информацию.

Чтобы получить маркер доступа, необходимо проверить заголовок `DataLabAccessToken` HTTP-запроса, который браузер отправляет к API каталога. При этом вам нужно выполнить вход.

1.  Войдите на веб-сайт [Коллекция решений ИИ Azure](https://gallery.azure.ai/).
2.  Щелкните изображение профиля в правом верхнем углу, а затем имя учетной записи, чтобы загрузить страницу профиля.
3.  Откройте панель средств для разработчиков браузера, нажав клавишу F12, выберите вкладку Network и обновите страницу. 
4. Отфильтруйте запросы по строке *catalog*, введя ее в текстовом поле фильтра.
5.  В запросах к URL-адресу `https://catalog.cortanaanalytics.com/entities` найдите запрос GET и выберите вкладку *Headers*. Прокрутите вниз к разделу *Request Headers*.
6.  Под заголовком `DataLabAccessToken` находится буквенно-цифровой маркер. Чтобы защитить свои данные, не передайте никому этот маркер.

### <a name="view-user-information"></a>Просмотр пользовательских сведений
Просмотрите сведения в профиле пользователя, заменив `[AuthorId]` идентификатором автора, полученным на предыдущих шагах, в следующем URL-адресе:

    https://catalog.cortanaanalytics.com/users/[AuthorID]

К примеру, возьмем такой запрос URL-адреса:
    
    https://catalog.cortanaanalytics.com/users/99F1F5C6260295F1078187FA179FBE08B618CB62129976F09C6AF0923B02A5BA

Он возвращает такой ответ:

    {"entities_count":9,"contribution_score":86.351575190956922,"scored_at":"2018-05-07T14:30:25.9305671+00:00","contributed_at":"2018-05-07T14:26:55.0381756+00:00","created_at":"2017-12-15T00:49:15.6733094+00:00","updated_at":"2017-12-15T00:49:15.6733094+00:00","name":"First Last","slugs":["First-Last"],"tenant_id":"14b2744cf8d6418c87ffddc3f3127242","community_id":"9502630827244d60a1214f250e3bbca7","id":"99F1F5C6260295F1078187FA179FBE08B618CB62129976F09C6AF0923B02A5BA","_links":{"self":"https://catalog.azureml.net/tenants/14b2744cf8d6418c87ffddc3f3127242/communities/9502630827244d60a1214f250e3bbca7/users/99F1F5C6260295F1078187FA179FBE08B618CB62129976F09C6AF0923B02A5BA"},"etag":"\"2100d185-0000-0000-0000-5af063010000\""}


### <a name="view-public-entities"></a>Просмотр общедоступных объектов

API каталога сохраняет сведения об опубликованных объектах в коллекции решений ИИ Azure, которые также можно просмотреть непосредственно на веб-сайте [Коллекция решений ИИ](https://gallery.azure.ai/). 

Чтобы просмотреть опубликованные объекты, перейдите по следующему URL-адресу, заменив `[AuthorId]` идентификатором автора, полученным при работе с [разделом](#get-an-author-id) выше.

    https://catalog.cortanaanalytics.com/entities?$filter=author/id eq '[AuthorId]'

Пример:

    https://catalog.cortanaanalytics.com/entities?$filter=author/id eq '99F1F5C6260295F1078187FA179FBE08B618CB62129976F09C6AF0923B02A5BA'

### <a name="view-unlisted-and-public-entities"></a>Просмотр частных и общедоступных объектов

Этот запрос отображает только общедоступные объекты. Чтобы просмотреть все объекты, включая частные, укажите маркер доступа, полученный в предыдущем разделе.

1.  Создайте HTTP-запрос GET к URL-адресу каталога с помощью таких средств, как [Postman](https://www.getpostman.com), как описано в разделе [Получение маркера доступа](#get-your-access-token).
2.  Создайте заголовок HTTP-запроса `DataLabAccessToken` и задайте в качестве значения маркер доступа.
3.  Отправьте HTTP-запрос.

> [!TIP]
> Если частные объекты не отображаются в ответах от API каталога, вероятно, у пользователя недопустимый или просроченный маркер доступа. Выйдите из коллекции решений ИИ Azure, а затем повторите действия, описанные в разделе [Получение маркера доступа](#get-your-access-token), для обновления маркера. 
