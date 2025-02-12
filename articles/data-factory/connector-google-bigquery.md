---
title: Копирование данных из Google BigQuery с помощью фабрики данных Azure | Документация Майкрософт
description: Узнайте, как копировать данные из Google BigQuery в поддерживаемые хранилища данных-приемники с помощью действия копирования в конвейере фабрики данных.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 12/07/2018
ms.author: jingwang
ms.openlocfilehash: c9320c8d0cf512bc9145accc07ab4c79630a7c84
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "60808888"
---
# <a name="copy-data-from-google-bigquery-by-using-azure-data-factory"></a>Копирование данных из Google BigQuery с помощью фабрики данных Azure

Из этой статьи вы узнаете, как с помощью действия копирования в фабрике данных Azure копировать данные из Google BigQuery. Это продолжение [статьи с обзором действия копирования](copy-activity-overview.md), в которой представлены общие сведения о действии копирования.

## <a name="supported-capabilities"></a>Поддерживаемые возможности

Данные из Google BigQuery можно скопировать в любое поддерживаемое хранилище данных, используемое в качестве приемника. Список хранилищ данных, которые поддерживаются в качестве источников и приемников для действия копирования, приведен в таблице [Поддерживаемые хранилища данных и форматы](copy-activity-overview.md#supported-data-stores-and-formats).

В фабрике данных предоставляется встроенный драйвер, который обеспечивает подключение. Поэтому не нужно вручную устанавливать драйвер для использования этого соединителя.

>[!NOTE]
>Этот соединитель Google BigQuery создан на основе API-интерфейсов BigQuery. Учтите, что в BigQuery ограничено максимальное число входящих запросов и применяются соответствующие квоты на каждый проект. Дополнительные сведения см. в разделе о [квотах и ограничениях на запросы API](https://cloud.google.com/bigquery/quotas#api_requests). Не активируйте слишком много одновременных запросов к учетной записи.

## <a name="get-started"></a>Начало работы

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Следующие разделы содержат сведения о свойствах, которые используются для определения сущностей фабрики данных, относящихся к соединителю Google BigQuery.

## <a name="linked-service-properties"></a>Свойства связанной службы

Для связанной службы Google BigQuery поддерживаются следующие свойства.

| Свойство | ОПИСАНИЕ | Обязательно для заполнения |
|:--- |:--- |:--- |
| type | Для свойства type необходимо задать значение **GoogleBigQuery**. | Да |
| project | Идентификатор проекта BigQuery по умолчанию для отправки запросов.  | Да |
| additionalProjects | Список разделенных запятыми идентификаторов общедоступных проектов BigQuery для доступа.  | Нет |
| requestGoogleDriveScope | Определяет, следует ли подавать запрос на доступ к Google Drive. Если разрешить доступ к Google Drive, включится поддержка для федеративных таблиц, которые объединяют данные BigQuery с данными с Google Drive. Значение по умолчанию — **false**.  | Нет |
| authenticationType | Механизм OAuth 2.0 для аутентификации. ServiceAuthentication может использоваться только в локальных средах выполнения интеграции. <br/>Допустимые значения: **UserAuthentication** и **ServiceAuthentication**. В разделах ниже описываются дополнительные свойства и приведены примеры JSON для поддерживаемых типов проверки подлинности. | Да |

### <a name="using-user-authentication"></a>Использование проверки подлинности пользователей

Задайте для свойства authenticationType значение **UserAuthentication** и укажите следующие свойства вместе с универсальными свойствами, описанными в предыдущем разделе:

| Свойство | ОПИСАНИЕ | Обязательно для заполнения |
|:--- |:--- |:--- |
| clientId | Идентификатор приложения, используемого для создания маркера обновления. | Нет |
| clientSecret | Секрет приложения, используемого для создания маркера обновления. Пометьте это поле как SecureString, чтобы безопасно хранить его в фабрике данных, или [добавьте ссылку на секрет, хранящийся в Azure Key Vault](store-credentials-in-key-vault.md). | Нет |
| refreshtoken | Маркер обновления, полученный из Google, используемый для авторизации доступа к BigQuery. Сведения о том, как его получить, см. [здесь](https://developers.google.com/identity/protocols/OAuth2WebServer#obtainingaccesstokens) и [здесь](https://jpd.ms/getting-your-bigquery-refresh-token-for-azure-datafactory-f884ff815a59). Пометьте это поле как SecureString, чтобы безопасно хранить его в фабрике данных, или [добавьте ссылку на секрет, хранящийся в Azure Key Vault](store-credentials-in-key-vault.md). | Нет |

**Пример.**

```json
{
    "name": "GoogleBigQueryLinkedService",
    "properties": {
        "type": "GoogleBigQuery",
        "typeProperties": {
            "project" : "<project ID>",
            "additionalProjects" : "<additional project IDs>",
            "requestGoogleDriveScope" : true,
            "authenticationType" : "UserAuthentication",
            "clientId": "<id of the application used to generate the refresh token>",
            "clientSecret": {
                "type": "SecureString",
                "value":"<secret of the application used to generate the refresh token>"
            },
            "refreshToken": {
                "type": "SecureString",
                "value": "<refresh token>"
            }
        }
    }
}
```

### <a name="using-service-authentication"></a>Использование проверки подлинности службы

Задайте для свойства authenticationType значение **ServiceAuthentication** и укажите перечисленные ниже свойства вместе с универсальными свойствами, описанными в предыдущем разделе. Этот тип проверки подлинности можно использовать только в локальных средах выполнения интеграции.

| Свойство | ОПИСАНИЕ | Обязательно для заполнения |
|:--- |:--- |:--- |
| email | Идентификатор электронной почты учетной записи службы, используемый для ServiceAuthentication. Может использоваться только в локальной среде выполнения интеграции.  | Нет |
| keyFilePath | Полный путь к файлу ключа P12, используемый для аутентификации адреса электронной почты учетной записи службы. | Нет |
| trustedCertPath | Полный путь к PEM-файлу, который содержит сертификаты доверенного ЦС, используемые для проверки сервера при подключении по протоколу SSL. Это свойство можно задать, только если SSL используется в локальной среде выполнения интеграции. Значением по умолчанию является файл cacerts.pem, который устанавливается вместе со средой выполнения интеграции.  | Нет |
| useSystemTrustStore | Указывает, следует ли использовать сертификат ЦС из доверенного системного хранилища или из указанного PEM-файла. Значение по умолчанию — **false**.  | Нет |

**Пример.**

```json
{
    "name": "GoogleBigQueryLinkedService",
    "properties": {
        "type": "GoogleBigQuery",
        "typeProperties": {
            "project" : "<project id>",
            "requestGoogleDriveScope" : true,
            "authenticationType" : "ServiceAuthentication",
            "email": "<email>",
            "keyFilePath": "<.p12 key path on the IR machine>"
        },
        "connectVia": {
            "referenceName": "<name of Self-hosted Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

## <a name="dataset-properties"></a>Свойства набора данных

Полный список разделов и свойств, доступных для определения наборов данных, см. в статье о [наборах данных](concepts-datasets-linked-services.md). В этом разделе содержится список свойств, поддерживаемых набором данных Google BigQuery.

Чтобы скопировать данные из Google BigQuery, установите для свойства type набора данных значение **GoogleBigQueryObject**. Поддерживаются следующие свойства:

| Свойство | ОПИСАНИЕ | Обязательно для заполнения |
|:--- |:--- |:--- |
| type | Для свойства type набора данных необходимо задать значение **GoogleBigQueryObject** | Да |
| tableName | Имя таблицы. | Нет (если свойство query указано в источнике действия) |

**Пример**

```json
{
    "name": "GoogleBigQueryDataset",
    "properties": {
        "type": "GoogleBigQueryObject",
        "linkedServiceName": {
            "referenceName": "<GoogleBigQuery linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {}
    }
}
```

## <a name="copy-activity-properties"></a>Свойства действия копирования

Полный список разделов и свойств, используемых для определения действий, см. в статье [Конвейеры и действия в фабрике данных Azure](concepts-pipelines-activities.md). В этом разделе содержится список свойств, поддерживаемых типом источника Google BigQuery.

### <a name="googlebigquerysource-as-a-source-type"></a>GoogleBigQuerySource в качестве типа источника

Чтобы копировать данные Google BigQuery, установите тип источника **GoogleBigQuerySource** в действии копирования. В разделе **source** действия копирования поддерживаются следующие свойства.

| Свойство | ОПИСАНИЕ | Обязательно для заполнения |
|:--- |:--- |:--- |
| type | Для свойства type источника действия копирования необходимо задать значение **GoogleBigQuerySource**. | Да |
| query | Используйте пользовательский SQL-запрос для чтения данных. Например, `"SELECT * FROM MyTable"`. | Нет (если для набора данных задано свойство tableName) |

**Пример.**

```json
"activities":[
    {
        "name": "CopyFromGoogleBigQuery",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<GoogleBigQuery input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "GoogleBigQuerySource",
                "query": "SELECT * FROM MyTable"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

## <a name="next-steps"></a>Дальнейшие действия
В таблице [Поддерживаемые хранилища данных и форматы](copy-activity-overview.md#supported-data-stores-and-formats) приведен список хранилищ данных, которые поддерживаются в качестве источников и приемников для действия копирования в фабрике данных.
