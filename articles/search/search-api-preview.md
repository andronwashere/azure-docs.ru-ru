---
title: Предварительная версия REST API для поиска Azure 2019-05-06-Preview — Поиск Azure
description: Azure Служба поиска REST API версии 2019-05-06-Preview включает экспериментальные функции, такие как хранилище знаний и управляемые клиентом ключи шифрования.
services: search
author: HeidiSteen
manager: cgronlun
ms.service: search
ms.devlang: rest-api
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: search
ms.date: 05/02/2019
ms.author: heidist
ms.custom: seodec2018
ms.openlocfilehash: 29d079c4e21352ced5fdcde44acaee66b79f6af9
ms.sourcegitcommit: de47a27defce58b10ef998e8991a2294175d2098
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/15/2019
ms.locfileid: "67876658"
---
# <a name="azure-search-service-rest-api-version-2019-05-06-preview"></a>Azure Служба поиска API для других версий — версия 2019-05-06-Preview
В этой статье описывается версия `api-version=2019-05-06-Preview` REST API службы "Поиск Azure", предлагающая экспериментальные функции, которые пока недоступны вообще.

> [!NOTE]
> Функции предварительной версии доступны для тестирования и экспериментов с целью получения отзывов и могут изменяться. Мы не рекомендуем использовать предварительные версии API в рабочих приложениях.


## <a name="new-in-2019-05-06-preview"></a>Новое в 2019-05-06-Preview

[**Хранилище знаний**](knowledge-store-concept-intro.md) — это новое назначение конвейера обогащения на основе искусственного интеллекта. В дополнение к индексу теперь можно сохранять заполненные структуры данных, созданные во время индексирования в службе хранилища Azure. Вы управляете физическими структурами данных с помощью элементов в наборе навыков, включая способ формирования данных, будь то данные, хранящиеся в хранилище таблиц или хранилище BLOB-объектов, и существует ли несколько представлений.

[**Управляемые клиентом ключи шифрования**](search-security-manage-encryption-keys.md) для шифрования на стороне службы — это также новая функция предварительной версии. В дополнение к встроенному шифрованию неактивных данных, управляемому корпорацией Майкрософт, можно применить дополнительный уровень шифрования, в котором вы являетесь единственным владельцем ключей.

## <a name="other-preview-features"></a>Другие возможности предварительной версии

Возможности, объявленные в более ранних предварительных версиях, все еще находятся в общедоступной предварительной версии. При вызове API с более ранней предварительной версией API вы можете продолжать использовать эту версию или переключиться на `2019-05-06-Preview` без изменений ожидаемого поведения.

+ [Параметр запроса moreLikeThis](search-more-like-this.md) для поиска документов, связанных с указанным документом. Эта компонент был доступен в более ранних предварительных версиях. 
* [Индексирование BLOB-объектов CSV](search-howto-index-csv-blobs.md) создает по одному документу в строке, а не по одному документу на текстовый BLOB-объект.
* [Поддержка MONGODB API для индексаторов Cosmos DB](search-howto-index-cosmosdb.md) доступна в предварительной версии.


## <a name="how-to-call-a-preview-api"></a>Вызов API предварительной версии

Более старые предварительные версии по-прежнему работают, но со временем устаревают. Если код вызывает `api-version=2016-09-01-Preview` или `api-version=2017-11-11-Preview`, эти вызовы остаются действительными. Однако только в последнюю версию предварительного просмотра будут добавлены усовершенствования. 

Следующий пример синтаксиса демонстрирует вызов предварительной версии API.

    GET https://[service name].search.windows.net/indexes/[index name]/docs?search=*&api-version=2019-05-06-Preview

Существуют различные версии службы поиска Azure. Дополнительные сведения см. в разделе [Версии API](search-api-versions.md).

## <a name="next-steps"></a>Следующие шаги

Ознакомьтесь с справочной документацией по Azure Служба поиска REST API. Если вы столкнулись с проблемами, попросите помощь по [StackOverflow](https://stackoverflow.com/) или [обратитесь в службу поддержки](https://azure.microsoft.com/support/community/?product=search).

> [!div class="nextstepaction"]
> [Справочник по службе поиска REST API](https://docs.microsoft.com/rest/api/searchservice/)