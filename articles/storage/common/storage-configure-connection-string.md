---
title: Настроить строку подключения для службы хранилища Azure
description: Настройка строки подключения для учетной записи хранения Azure. Строка подключения содержит сведения, необходимые для авторизации доступа к учетной записи хранения из приложения во время выполнения с помощью общего ключа авторизации.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 06/20/2019
ms.author: tamram
ms.reviewer: cbrooks
ms.subservice: common
ms.openlocfilehash: 7c83b382f8aca3d8fda1c0de4785c51f3f3b1fc5
ms.sourcegitcommit: 82efacfaffbb051ab6dc73d9fe78c74f96f549c2
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/20/2019
ms.locfileid: "67302530"
---
# <a name="configure-azure-storage-connection-strings"></a>Настройка строк подключения службы хранилища Azure

Строка подключения содержит сведения о проверке подлинности, необходимые приложению для доступа к данным в учетной записи хранения Azure во время выполнения с помощью общего ключа авторизации. Настройка строки подключения позволяет выполнить следующие действия:

* подключиться к эмулятору хранилища Azure;
* получить доступ к учетной записи хранения в Azure;
* получить доступ к указанным ресурсам Azure через подписанный URL-адрес (SAS).

[!INCLUDE [storage-recommend-azure-ad-include](../../../includes/storage-recommend-azure-ad-include.md)]

[!INCLUDE [storage-account-key-note-include](../../../includes/storage-account-key-note-include.md)]

## <a name="view-and-copy-a-connection-string"></a>Просмотр и копирование строки подключения

[!INCLUDE [storage-view-keys-include](../../../includes/storage-view-keys-include.md)]

## <a name="store-a-connection-string"></a>Store строку подключения

Приложению требуется доступ к строке подключения во время выполнения, чтобы выполнять авторизацию запросов к службе хранилища Azure. Есть несколько вариантов для хранения строки подключения.

* Можно хранить строки подключения в переменной среды.
* Приложение, работающее на настольном компьютере или на устройстве, может хранить строку подключения в файле **app.config** или **web.config**. Добавьте строку подключения в раздел **AppSettings** в этих файлах.
* Приложение, работающее в облачной службе Azure, может хранить строку подключения в [файле схемы конфигурации службы Azure (.cscfg)](https://msdn.microsoft.com/library/ee758710.aspx). Добавьте строку подключения в раздел **ConfigurationSettings** файла конфигурации службы.

Хранение строки подключения в файле конфигурации упрощает обновление строки подключения для переключения между эмулятором хранения и учетной записью хранения Azure в облаке. Вам потребуется только изменить строку подключения, чтобы указать целевую среду.

Вы можете использовать [Microsoft Azure Configuration Manager](https://www.nuget.org/packages/Microsoft.Azure.ConfigurationManager/) для доступа к строке подключения в среде выполнения независимо от того, где выполняется приложение.

## <a name="configure-a-connection-string-for-the-storage-emulator"></a>Настроить строку подключения для эмулятора хранения

[!INCLUDE [storage-emulator-connection-string-include](../../../includes/storage-emulator-connection-string-include.md)]

Дополнительные сведения об эмуляторе хранения см. в статье [Использование эмулятора хранения Azure для разработки и тестирования](storage-use-emulator.md).

## <a name="configure-a-connection-string-for-an-azure-storage-account"></a>Настроить строку подключения для учетной записи хранения Azure

Чтобы создать строку подключения для учетной записи хранения Azure, используйте формат, указанный ниже. Указывает, следует ли подключаться к учетной записи хранения через HTTPS (рекомендуется) или HTTP. Замените `myAccountName` на имя вашей учетной записи хранения и замените `myAccountKey` на ваш ключ доступа учетной записи:

`DefaultEndpointsProtocol=[http|https];AccountName=myAccountName;AccountKey=myAccountKey`

Например, строка подключения может выглядеть так:

`DefaultEndpointsProtocol=https;AccountName=storagesample;AccountKey=<account-key>`

Служба хранилища Azure поддерживает в строке подключения как HTTP, так и HTTPS, однако *настоятельно рекомендуется использовать HTTPS*.

> [!TIP]
> Данные о строках подключения учетной записи хранения можно найти на [портале Azure](https://portal.azure.com). В колонке меню учетной записи хранения выберите **ПАРАМЕТРЫ** > **Ключи доступа**, чтобы просмотреть строки подключения для первичных и вторичных ключей доступа.
>

## <a name="create-a-connection-string-using-a-shared-access-signature"></a>Создание строки подключения с помощью подписанного URL-адреса

[!INCLUDE [storage-use-sas-in-connection-string-include](../../../includes/storage-use-sas-in-connection-string-include.md)]

## <a name="create-a-connection-string-for-an-explicit-storage-endpoint"></a>Создание строки подключения для явной конечной точки хранилища

Вы можете указать явные конечные точки службы в строке подключения вместо конечных точек по умолчанию. Чтобы создать строку подключения, определяющую явную конечную точку, укажите полную конечную точку для каждой службы, включая спецификацию протокола (HTTPS (рекомендуется) или HTTP), в следующем формате:

```
DefaultEndpointsProtocol=[http|https];
BlobEndpoint=myBlobEndpoint;
FileEndpoint=myFileEndpoint;
QueueEndpoint=myQueueEndpoint;
TableEndpoint=myTableEndpoint;
AccountName=myAccountName;
AccountKey=myAccountKey
```

Один из сценариев, когда может потребоваться указать явную конечную точку, — если конечная точка хранилища BLOB-объектов сопоставлена с [личным доменом](../blobs/storage-custom-domain-name.md). В этом случае в строке подключения можно указать пользовательскую конечную точку хранилища BLOB-объектов. Вы можете дополнительно указать конечные точки по умолчанию для других служб, если приложение их использует.

Ниже приведен пример строки подключения, в которой указывается явная конечная точка для службы BLOB-объектов.

```
# Blob endpoint only
DefaultEndpointsProtocol=https;
BlobEndpoint=http://www.mydomain.com;
AccountName=storagesample;
AccountKey=<account-key>
```

В следующем примере указываются явные конечные точки для всех служб, включая личный домен для службы BLOB-объектов:

```
# All service endpoints
DefaultEndpointsProtocol=https;
BlobEndpoint=http://www.mydomain.com;
FileEndpoint=https://myaccount.file.core.windows.net;
QueueEndpoint=https://myaccount.queue.core.windows.net;
TableEndpoint=https://myaccount.table.core.windows.net;
AccountName=storagesample;
AccountKey=<account-key>
```

Значения конечных точек из строки подключения применяются для создания универсальных кодов ресурса (URI) для служб хранилищ, а также определяют форму любых URI, возвращаемых в код.

Если сопоставить конечную точку хранилища с личным доменом и не указывать эту конечную точку в строке подключения, то вы не сможете получить доступ к данным в этой службе из своего кода с помощью данной строки подключения.

> [!IMPORTANT]
> Значения конечных точек служб в строках подключения должны быть в формате правильно сформированного универсального кода ресурса (URI), включая `https://` (рекомендуется) или `http://`. Так как служба хранилища Azure пока не поддерживает протокол HTTPS для личных доменов, *необходимо* указать `http://` в каждом универсальном коде ресурса (URI) конечной точки, который указывает на личный домен.
>

### <a name="create-a-connection-string-with-an-endpoint-suffix"></a>Создание строки подключения с суффиксом конечной точки

Чтобы создать строку подключения для службы хранилища в регионах или экземплярах с разными суффиксами конечных точек, таких как 21Vianet в Azure для Китая или Azure для государственных организаций, используйте следующий формат строки подключения. Укажите, следует ли подключаться к учетной записи хранения через HTTPS (рекомендуется) или HTTP. Замените `myAccountName` именем своей учетной записи хранения, замените `myAccountKey` ключом доступа своей учетной записи и замените `mySuffix` суффиксом URI:

```
DefaultEndpointsProtocol=[http|https];
AccountName=myAccountName;
AccountKey=myAccountKey;
EndpointSuffix=mySuffix;
```

Ниже приведен пример строки подключения для служб хранилища в Azure для Китая 21Vianet.

```
DefaultEndpointsProtocol=https;
AccountName=storagesample;
AccountKey=<account-key>;
EndpointSuffix=core.chinacloudapi.cn;
```

## <a name="parsing-a-connection-string"></a>Анализ строки подключения

[!INCLUDE [storage-cloud-configuration-manager-include](../../../includes/storage-cloud-configuration-manager-include.md)]

## <a name="next-steps"></a>Дальнейшие действия

* [Использование эмулятора хранения Azure для разработки и тестирования](storage-use-emulator.md)
* [Клиентские инструменты службы хранилища Azure](storage-explorers.md)
* [Использование подписанных URL-адресов (SAS)](storage-dotnet-shared-access-signature-part-1.md)

