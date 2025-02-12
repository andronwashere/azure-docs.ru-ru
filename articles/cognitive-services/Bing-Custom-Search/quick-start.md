---
title: Краткое руководство. Создание первого экземпляра Пользовательского поиска Bing | Документация Майкрософт
titleSuffix: Azure Cognitive Services
description: В этой статье показано, как создать пользовательский экземпляр Bing, который может выполнять поиск в доменах и на веб-страницах, которые вы определяете.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-custom-search
ms.topic: quickstart
ms.date: 06/18/2019
ms.author: aahi
ms.openlocfilehash: 2806ca6f7079ffac3d2222363cd5b3839ef8f97d
ms.sourcegitcommit: 9dc7517db9c5817a3acd52d789547f2e3efff848
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/23/2019
ms.locfileid: "68405018"
---
# <a name="quickstart-create-your-first-bing-custom-search-instance"></a>Краткое руководство. Создание первого экземпляра Пользовательского поиска Bing

Для использования Пользовательского поиска Bing необходимо создать экземпляр пользовательского поиска, который определяет представление или срез Интернета. Экземпляр содержит общедоступные домены, веб-сайты и веб-страницы для выполнения поиска, а также любые корректировки ранжирования. 

Чтобы создать экземпляр, используйте [портал Пользовательского поиска Bing](https://customsearch.ai). 

![Изображение портала Пользовательского поиска Bing](media/blockedCustomSrch.png)

## <a name="prerequisites"></a>Предварительные требования

[!INCLUDE [cognitive-services-bing-custom-search-prerequisites](../../../includes/cognitive-services-bing-custom-search-signup-requirements.md)]

## <a name="create-a-custom-search-instance"></a>Создание экземпляра пользовательского поиска

Чтобы создать экземпляр Пользовательского поиска Bing, сделайте следующее:

1. Щелкните **Get Started** (Начало работы) на веб-странице [портала Пользовательского поиска Bing](https://customsearch.ai) и войдите в свою учетную запись Майкрософт.

2. Выберите **New Instance** (Новый экземпляр) и введите описательное имя. Имя своего экземпляра можно изменить в любое время.
 
3. На вкладке **Активные** в разделе **Режим поиска** введите URL-адреса веб-сайтов, которые нужно включить в поиск. 

    > [!NOTE]
    > Экземпляры Пользовательского поиска Bing будут возвращать результаты только для доменов и веб-страниц, которые общедоступны и индексированы Bing.

4. Вводить запрос и просматривать результаты поиска, возвращенные экземпляром поиска, можно в правой части портала Пользовательского поиска Bing. Если результаты не возвращаются, попробуйте ввести другой URL-адрес.  

5. Щелкните **Publish** (Опубликовать), чтобы опубликовать изменения в рабочую среду и обновить конечные точки экземпляра.

6.  Щелкните вкладку **Production** (Рабочая среда). В разделе **Endpoints** (Конечные точки) скопируйте **идентификатор пользовательской конфигурации**. Этот идентификатор необходим для вызова API пользовательского поиска. Для этого его нужно добавить к параметру запроса `customconfig=` в своих вызовах.


## <a name="next-steps"></a>Дополнительная информация

> [!div class="nextstepaction"]
> [Краткое руководство по вызову конечной точки службы "Пользовательский поиск Bing"](./call-endpoint-csharp.md)
