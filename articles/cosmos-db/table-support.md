---
title: Поддержка Хранилища таблиц в Azure Cosmos DB
description: Узнайте, как совместно использовать API таблиц Azure Cosmos DB и хранилище таблиц Azure.
ms.service: cosmos-db
ms.subservice: cosmosdb-table
ms.topic: overview
ms.date: 11/15/2017
author: wmengmsft
ms.author: wmeng
ms.reviewer: sngun
ms.openlocfilehash: c7c9caad6c454b90a6d50932450922a03bbc1944
ms.sourcegitcommit: 6ea7f0a6e9add35547c77eef26f34d2504796565
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/14/2019
ms.locfileid: "65604179"
---
# <a name="developing-with-azure-cosmos-db-table-api-and-azure-table-storage"></a>Разработка с использованием API таблиц Azure Cosmos DB и хранилища таблиц Azure

API таблиц Azure Cosmos DB и хранилище таблиц Azure используют одну и ту же модель данных и предоставляют одинаковые операции создания, удаления, обновления и запроса в своих пакетах SDK. 

[!INCLUDE [storage-table-cosmos-comparison](../../includes/storage-table-cosmos-comparison.md)]

## <a name="developing-with-the-azure-cosmos-db-table-api"></a>Разработка с использованием API таблиц Azure Cosmos DB

В настоящее время [API таблиц в Azure Cosmos DB](table-introduction.md) содержит четыре пакета SDK для разработки: 

* [Microsoft.Azure.Cosmos.Table](https://www.nuget.org/packages/Microsoft.Azure.Cosmos.Table). Пакет SDK .NET. Эта библиотека предназначена для .NET Standard и включает в себя те же классы и подписи методов, что и общедоступный [пакет SDK WindowsAzure.Storage](https://www.nuget.org/packages/WindowsAzure.Storage). Кроме того, она может подключаться к учетным записям Azure Cosmos DB через API таблиц. Пользователям библиотеки .NET Framework [Microsoft.Azure.CosmosDB.Table](https://www.nuget.org/packages/Microsoft.Azure.CosmosDB.Table/) рекомендуется выполнить обновление до [Microsoft.Azure.Cosmos.Table](https://www.nuget.org/packages/Microsoft.Azure.Cosmos.Table), так как библиотека сейчас находится в режиме обслуживания и ее поддержка скоро будет прекращена.

* [Пакет SDK для Python.](table-sdk-python.md) Новый пакет SDK для Python в Azure Cosmos DB — единственный пакет SDK, который поддерживает хранилище таблиц Azure в Python. Этот пакет SDK подключается к хранилищу таблиц Azure и API таблиц Azure Cosmos DB.

* [Пакет SDK для Java](table-sdk-java.md). Этот пакет SDK для службы хранилища Azure может подключаться к учетным записям Azure Cosmos DB с помощью API таблиц.

* [Пакет SDK для Node.js](table-sdk-nodejs.md). Этот пакет SDK для службы хранилища Azure может подключаться к учетным записям Azure Cosmos DB с помощью API таблиц.


Дополнительные сведения о работе с API таблиц доступны в статье [Часто задаваемые вопросы о различных API в службе Azure Cosmos DB](faq.md#table).

## <a name="developing-with-azure-table-storage"></a>Разработка с использованием хранилища таблиц Azure

Хранилище таблиц Azure содержит следующие пакеты SDK для разработки.

- [Пакет SDK WindowsAzure.Storage .NET](https://www.nuget.org/packages/WindowsAzure.Storage/). Эта библиотека позволяет работать с хранилищем службы таблиц.
- [Пакет SDK для Python](table-sdk-python.md). Пакет SDK таблиц Azure Cosmos DB для Python также поддерживает службу хранилища таблиц.
- [Пакет SDK службы хранилища Azure для Java](https://github.com/azure/azure-storage-java). Этот пакет SDK хранилища Azure предоставляет клиентскую библиотеку Java для использования хранилища таблиц Azure.
- [Пакет SDK для Node.js](table-sdk-nodejs.md). Этот пакет SDK содержит пакет Node.js и совместимую клиентскую библиотеку JavaScript для использования службы хранилища таблиц.
- [Модуль AzureRmStorageTable PowerShell](https://www.powershellgallery.com/packages/AzureRmStorageTable). Этот модуль PowerShell содержит командлеты для работы с таблицами хранилища.
- [Клиентская библиотека службы хранилища Azure для C++](https://github.com/Azure/azure-storage-cpp/). Эта библиотека позволяет создавать приложения для службы хранилища Azure.
- [Клиентская библиотека таблиц хранилища Azure для Ruby](https://github.com/azure/azure-storage-ruby/tree/master/table). Этот проект содержит пакет Ruby, который упрощает доступ к службам таблиц хранилища Azure.
- [Клиентская библиотека PHP для таблиц хранилища Azure](https://github.com/Azure/azure-storage-php/tree/master/azure-storage-table). Этот проект содержит клиентскую библиотеку PHP, которая упрощает доступ к службам таблиц хранилища Azure.


   





