---
title: Примеры для службы хранилища Azure с использованием Java | Документация Майкрософт
description: Просмотрите, загрузите и запустите образцы кода и приложений для хранилища Azure. Воспользуйтесь примерами для начала работы с большими двоичными объектами, очередями, таблицами и файлами с помощью клиентских библиотек хранилища Java.
services: storage
author: mhopkins-msft
ms.service: storage
ms.devlang: java
ms.topic: article
ms.date: 05/03/2019
ms.author: mhopkins
ms.subservice: common
ms.openlocfilehash: 3d241f1905244d3a8039372262f84ba0fd25220d
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "65209774"
---
# <a name="azure-storage-samples-using-java"></a>Примеры для службы хранилища Azure с использованием Java

## <a name="java-sample-index"></a>Указатель по примерам для Java

В таблице ниже приведен обзор репозитория примеров и сценарии, описанные в каждом примере. Щелкните ссылки для просмотра соответствующего примера кода на сайте GitHub.

<table style="font-size:90%"><thead><tr><th style="font-size:110%">Конечная точка</th><th style="font-size:110%">Сценарий</th><th style="font-size:110%">Пример кода</th></tr></thead><tbody>
<tr>
<td rowspan="16"><b>Большой двоичный объект</b></td>
<td>Добавление больших двоичных объектов</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Getting Started with Azure Blob Service in Java</a> (Приступая к работе со службой BLOB-объектов Azure на языке Java)</td>
</tr>
<tr>
<td>Блочный BLOB-объект</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Getting Started with Azure Blob Service in Java</a> (Приступая к работе со службой BLOB-объектов Azure на языке Java)</td>
</tr>
<tr>
<td>Шифрование на стороне клиента</td>
<td><a href="https://github.com/Azure-Samples/storage-java-client-side-encryption">Приступая к работе с шифрованием на стороне клиента Azure в Java</a></td>
</tr>
<tr>
<td>Копирование BLOB-объекта</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Getting Started with Azure Blob Service in Java</a> (Приступая к работе со службой BLOB-объектов Azure на языке Java)</td>
</tr>
<tr>
<td>Create Container (Создание контейнера)</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Getting Started with Azure Blob Service in Java</a> (Приступая к работе со службой BLOB-объектов Azure на языке Java)</td>
</tr>
<tr>
<td>Delete BLOB (Удаление BLOB-объекта)</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Getting Started with Azure Blob Service in Java</a> (Приступая к работе со службой BLOB-объектов Azure на языке Java)</td>
</tr>
<tr>
<td>Delete Container (Удаление контейнера)</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Getting Started with Azure Blob Service in Java</a> (Приступая к работе со службой BLOB-объектов Azure на языке Java)</td>
</tr>
<tr>
<td>Метаданные, свойства и статистика больших двоичных объектов</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobAdvanced.java">Getting Started with Azure Blob Service in Java</a> (Приступая к работе со службой BLOB-объектов Azure на языке Java)</td>
</tr>
<tr>
<td>ACL, метаданные и свойства контейнера</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobAdvanced.java">Getting Started with Azure Blob Service in Java</a> (Приступая к работе со службой BLOB-объектов Azure на языке Java)</td>
</tr>
<tr>
<td>Get Page Ranges (Получение диапазона страницы)</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java#L399">Getting Started with Azure Blob Service in Java</a> (Приступая к работе со службой BLOB-объектов Azure на языке Java)</td>
</tr>
<tr>
<td>Аренда большого двоичного объекта и контейнера</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Getting Started with Azure Blob Service in Java</a> (Приступая к работе со службой BLOB-объектов Azure на языке Java)</td>
</tr>
<tr>
<td>Список больших двоичных объектов и контейнеров</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Getting Started with Azure Blob Service in Java</a> (Приступая к работе со службой BLOB-объектов Azure на языке Java)</td>
</tr>
<tr>
<td>Страничный BLOB-объект</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Getting Started with Azure Blob Service in Java</a> (Приступая к работе со службой BLOB-объектов Azure на языке Java)</td>
</tr>
<tr>
<td>SAS</td>
<td><a href="https://github.com/Azure/azure-storage-java/blob/89540f018f1160ce55619c6fe7b5f5ff57d0ce10/src/test/java/com/microsoft/azure/storage/Samples.java#L513">Пример тестов SAS</a></td>
</tr>   
<tr>
<td>Свойства службы</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobAdvanced.java">Getting Started with Azure Blob Service in Java</a> (Приступая к работе со службой BLOB-объектов Azure на языке Java)</td>
</tr>
<tr>
<td>Создание моментального снимка большого двоичного объекта</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Getting Started with Azure Blob Service in Java</a> (Приступая к работе со службой BLOB-объектов Azure на языке Java)</td>
</tr>
<tr>
<td rowspan="9"><b>File</b></td>
<td>Создание общих папок, каталогов и файлов</td>
<td><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileBasics.java">Getting Started with Azure File Service in Java</a> (Приступая к работе с файловой службой Azure на языке Java)</td>
</tr>
<tr>
<td>Удаление общих папок, каталогов и файлов</td>
<td><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileBasics.java">Getting Started with Azure File Service in Java</a> (Приступая к работе с файловой службой Azure на языке Java)</td>
</tr>
<tr>
<td>Метаданные и свойства каталога</td>
<td><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileAdvanced.java">Getting Started with Azure File Service in Java</a> (Приступая к работе с файловой службой Azure на языке Java)</td>
</tr>
<tr>
<td>Скачивание файлов</td>
<td><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileBasics.java">Getting Started with Azure File Service in Java</a> (Приступая к работе с файловой службой Azure на языке Java)</td>
</tr>
<tr>
<td>Метрики, метаданные и свойства файла</td>
<td><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileAdvanced.java">Getting Started with Azure File Service in Java</a> (Приступая к работе с файловой службой Azure на языке Java)</td>
</tr>
<tr>
<td>Свойства службы файлов</td>
<td><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileAdvanced.java">Getting Started with Azure File Service in Java</a> (Приступая к работе с файловой службой Azure на языке Java)</td>
</tr>
<tr>
<td>Список каталогов и файлов</td>
<td><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileBasics.java">Getting Started with Azure File Service in Java</a> (Приступая к работе с файловой службой Azure на языке Java)</td>
</tr>
<tr>
<td>Список общих папок</td>
<td><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileBasics.java">Getting Started with Azure File Service in Java</a> (Приступая к работе с файловой службой Azure на языке Java)</td>
</tr>
<tr>
<td>Статистика, метаданные и свойства общей папки</td>
<td><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileAdvanced.java">Getting Started with Azure File Service in Java</a> (Приступая к работе с файловой службой Azure на языке Java)</td>
</tr>
<tr>
<td rowspan="8"><b>Очередь</b></td>
<td>Добавление сообщения</td>
<td><a href="https://github.com/Azure-Samples/storage-queue-java-getting-started/blob/master/src/QueueBasics.java#L63">Getting Started with Azure Queue Service in Java</a> (Приступая к работе со службой очередей Azure на языке Java)</td>
</tr>
<tr>
<td>Шифрование на стороне клиента</td>
<td><a href="https://github.com/Azure-Samples/storage-java-client-side-encryption/blob/master/src/gettingstarted/KeyVaultGettingStarted.java">Приступая к работе с шифрованием на стороне клиента Azure в Java</a></td>
</tr>
<tr>
<td>Создание очередей</td>
<td><a href="https://github.com/Azure-Samples/storage-queue-java-getting-started/blob/master/src/QueueBasics.java">Getting Started with Azure Queue Service in Java</a> (Приступая к работе со службой очередей Azure на языке Java)</td>
</tr>
<tr>
<td>Удаление сообщения и очереди</td>
<td><a href="https://github.com/Azure-Samples/storage-queue-java-getting-started/blob/master/src/QueueBasics.java">Getting Started with Azure Queue Service in Java</a> (Приступая к работе со службой очередей Azure на языке Java)</td>
</tr>
<tr>
<td>Просмотр сообщения</td>
<td><a href="https://github.com/Azure-Samples/storage-queue-java-getting-started/blob/master/src/QueueBasics.java">Getting Started with Azure Queue Service in Java</a> (Приступая к работе со службой очередей Azure на языке Java)</td>
</tr>
<tr>
<td>ACL, метаданные и статистика очереди</td>
<td><a href="https://github.com/Azure-Samples/storage-queue-java-getting-started/blob/master/src/QueueAdvanced.java">Getting Started with Azure Queue Service in Java</a> (Приступая к работе со службой очередей Azure на языке Java)</td>
</tr>
<tr>
<td>Свойства службы очередей</td>
<td><a href="https://github.com/Azure-Samples/storage-queue-java-getting-started/blob/master/src/QueueAdvanced.java">Getting Started with Azure Queue Service in Java</a> (Приступая к работе со службой очередей Azure на языке Java)</td>
</tr>
<tr>
<td>Сообщение об обновлении</td>
<td><a href="https://github.com/Azure-Samples/storage-queue-java-getting-started/blob/master/src/QueueBasics.java">Getting Started with Azure Queue Service in Java</a> (Приступая к работе со службой очередей Azure на языке Java)</td>
</tr>
<tr>
<td rowspan="7"><b>Таблица</b></td>
<td>Создание таблицы</td>
<td><a href="https://github.com/Azure-Samples/storage-table-java-getting-started/blob/master/src/main/java/com/microsoft/azure/cosmosdb/tablesample/TableBasics.java">Getting Started with Azure Table Service in Java</a> (Приступая к работе со службой таблиц Azure на языке Java)</td>
</tr>
<tr>
<td>Удаление сущности или таблицы</td>
<td><a href="https://github.com/Azure-Samples/storage-table-java-getting-started/blob/master/src/main/java/com/microsoft/azure/cosmosdb/tablesample/TableBasics.java">Getting Started with Azure Table Service in Java</a> (Приступая к работе со службой таблиц Azure на языке Java)</td>
</tr>
<tr>
<td>Вставка, слияние или замена сущностей</td>
<td><a href="https://github.com/Azure-Samples/storage-table-java-getting-started/blob/master/src/main/java/com/microsoft/azure/cosmosdb/tablesample/TableBasics.java">Getting Started with Azure Table Service in Java</a> (Приступая к работе со службой таблиц Azure на языке Java)</td>
</tr>
<tr>
<td>Query Entities (Сущности запроса)</td>
<td><a href="https://github.com/Azure-Samples/storage-table-java-getting-started/blob/master/src/main/java/com/microsoft/azure/cosmosdb/tablesample/TableBasics.java">Getting Started with Azure Table Service in Java</a> (Приступая к работе со службой таблиц Azure на языке Java)</td>
</tr>
<tr>
<td>Запросы к таблицам</td>
<td><a href="https://github.com/Azure-Samples/storage-table-java-getting-started/blob/master/src/main/java/com/microsoft/azure/cosmosdb/tablesample/TableBasics.java">Getting Started with Azure Table Service in Java</a> (Приступая к работе со службой таблиц Azure на языке Java)</td>
</tr>
<tr>
<td>Свойства и ACL таблицы</td>
<td><a href="https://github.com/Azure-Samples/storage-table-java-getting-started/blob/master/src/main/java/com/microsoft/azure/cosmosdb/tablesample/TableAdvanced.java">Getting Started with Azure Table Service in Java</a> (Приступая к работе со службой таблиц Azure на языке Java)</td>
</tr>
<tr>
<td>Update Entity (Обновление сущности)</td>
<td><a href="https://github.com/Azure-Samples/storage-table-java-getting-started/blob/master/src/main/java/com/microsoft/azure/cosmosdb/tablesample/TableBasics.java">Getting Started with Azure Table Service in Java</a> (Приступая к работе со службой таблиц Azure на языке Java)</td>
</tr>
</tbody>
</table>
<br/>

## <a name="azure-code-samples-library"></a>Библиотека примеров кода Azure

Чтобы просмотреть полную библиотеку примеров, перейдите в [библиотеку примеров кода Azure](https://azure.microsoft.com/resources/samples/?service=storage), включающую примеры для службы хранилища Azure, которые можно скачать и запустить локально. Пример библиотеки кода содержит пример кода в формате ZIP. Кроме того, можно просмотреть и клонировать репозиторий GitHub для каждого примера.

[!INCLUDE [storage-java-samples-include](../../../includes/storage-java-samples-include.md)]

## <a name="getting-started-guides"></a>Руководства по началу работы

Ознакомьтесь со следующими руководствами, если вам нужны инструкции по установке клиентских библиотек службы хранилища Azure и началу работы с ними.

* [Getting Started with Azure Blob Service in Java](../blobs/storage-quickstart-blobs-java.md) (Приступая к работе со службой BLOB-объектов Azure на языке Java)
* [Getting Started with Azure Queue Service in Java](../queues/storage-java-how-to-use-queue-storage.md) (Приступая к работе со службой очередей Azure на языке Java)
* [Getting Started with Azure Table Service in Java](../../cosmos-db/table-storage-how-to-use-java.md) (Приступая к работе со службой таблиц Azure на языке Java)
* [Getting Started with Azure File Service in Java](../files/storage-java-how-to-use-file-storage.md) (Приступая к работе с файловой службой Azure на языке Java)

## <a name="next-steps"></a>Дальнейшие действия

Дополнительные сведения о примерах для других языков см. здесь:

* .NET: [Примеры для службы хранилища Azure с использованием .NET](storage-samples-dotnet.md)
* Остальные языки: [Примеры для службы хранилища Azure](storage-samples.md)
