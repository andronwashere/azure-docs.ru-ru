---
title: Копирование данных из источников OData с помощью Фабрики данных Azure | Документация Майкрософт
description: Узнайте, как копировать данные из источников OData на поддерживаемые хранилища данных-приемники с помощью действия копирования в конвейере фабрики данных Azure.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 07/02/2019
ms.author: jingwang
ms.openlocfilehash: 5a54892c1d6e05e27e349e519d41ebd937ff64c7
ms.sourcegitcommit: 79496a96e8bd064e951004d474f05e26bada6fa0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/02/2019
ms.locfileid: "67509220"
---
# <a name="copy-data-from-an-odata-source-by-using-azure-data-factory"></a>Копирование данных из источника OData с помощью Фабрики данных Azure

> [!div class="op_single_selector" title1="Выберите версию службы фабрики данных, которую вы используете:"]
> * [Версия 1](v1/data-factory-odata-connector.md)
> * [Текущая версия](connector-odata.md)

В этой статье описывается, как с помощью действия копирования в Фабрике данных Azure копировать данные из источника OData. Это продолжение статьи о [действии копирования в Фабрике данных Azure](copy-activity-overview.md), в которой представлены общие сведения о действии копирования.

## <a name="supported-capabilities"></a>Поддерживаемые возможности

Данные из источника OData можно скопировать в любое хранилище данных, поддерживаемое в качестве приемника. Список хранилищ данных, поддерживаемых действием копирования в качестве источников и приемников, приведен в разделе [Поддерживаемые хранилища данных и форматы](copy-activity-overview.md#supported-data-stores-and-formats).

В частности, этот соединитель OData поддерживает:

- OData версии 3.0 и 4.0.
- Копирование данных с использованием одного из следующих типов аутентификации: **Анонимная**, **Базовая**, **Windows**, **Субъект-служба AAD** и **Управляемые удостоверения для ресурсов Azure**.

## <a name="get-started"></a>Начало работы

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

В разделах ниже приведены сведения о свойствах, которые используются для определения сущностей Фабрики данных, относящихся к соединителю OData.

## <a name="linked-service-properties"></a>Свойства связанной службы

Для связанной службы OData поддерживаются следующие свойства.

| Свойство | ОПИСАНИЕ | Обязательно для заполнения |
|:--- |:--- |:--- |
| type | Для свойства **type** необходимо задать значение **OData**. |Да |
| url | Корневой URL-адрес службы OData. |Да |
| authenticationType | Тип проверки подлинности, используемый для подключения к источнику OData. Допустимые значения: **Anonymous**, **Basic**, **Windows**, **AadServicePrincipal** и **ManagedServiceIdentity**. OAuth на основе пользователя не поддерживается. | Да |
| userName | Укажите **имя пользователя**, если вы используете проверку подлинности типа "Обычная" или "Windows". | Нет |
| password | Введите **пароль** для учетной записи пользователя, указанной для **имени пользователя**. Пометьте это поле как **SecureString**, чтобы безопасно хранить его в фабрике данных. Вы можете также [указать секрет, хранящийся в Azure Key Vault](store-credentials-in-key-vault.md). | Нет |
| servicePrincipalId | Укажите идентификатор клиента приложения Azure Active Directory. | Нет |
| aadServicePrincipalCredentialType | Укажите тип учетных данных для использования при аутентификации субъекта-службы. Допустимые значения: `ServicePrincipalKey` или `ServicePrincipalCert`. | Нет |
| servicePrincipalKey | Укажите ключ приложения Azure Active Directory. Пометьте это поле как **SecureString**, чтобы безопасно хранить его в фабрике данных, или [добавьте ссылку на секрет, хранящийся в Azure Key Vault](store-credentials-in-key-vault.md). | Нет |
| servicePrincipalEmbeddedCert | Укажите сертификат в кодировке base64 приложения, зарегистрированного в Azure Active Directory. Пометьте это поле как **SecureString**, чтобы безопасно хранить его в фабрике данных, или [добавьте ссылку на секрет, хранящийся в Azure Key Vault](store-credentials-in-key-vault.md). | Нет |
| servicePrincipalEmbeddedCertPassword | Если ваш сертификат защищен паролем, укажите пароль сертификата. Пометьте это поле как **SecureString**, чтобы безопасно хранить его в фабрике данных, или [добавьте ссылку на секрет, хранящийся в Azure Key Vault](store-credentials-in-key-vault.md).  | Нет|
| tenant | Укажите сведения о клиенте (доменное имя или идентификатор клиента), в котором находится приложение. Его можно получить, наведя указатель мыши на правый верхний угол страницы портала Azure. | Нет |
| aadResourceId | Укажите ресурс AAD, для которого запрашивается авторизация.| Нет |
| connectVia | [Среда выполнения интеграции](concepts-integration-runtime.md), используемая для подключения к хранилищу данных. Вы можете выбрать среду выполнения интеграции Azure или локальную среду IR (если хранилище данных расположено в частной сети). Если не указано другое, по умолчанию используется интегрированная Azure Integration Runtime. |Нет |

**Пример 1. Использование анонимной аутентификации**

```json
{
    "name": "ODataLinkedService",
    "properties": {
        "type": "OData",
        "typeProperties": {
            "url": "https://services.odata.org/OData/OData.svc",
            "authenticationType": "Anonymous"
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

**Пример 2. Использование обычной аутентификации**

```json
{
    "name": "ODataLinkedService",
    "properties": {
        "type": "OData",
        "typeProperties": {
            "url": "<endpoint of OData source>",
            "authenticationType": "Basic",
            "userName": "<user name>",
            "password": {
                "type": "SecureString",
                "value": "<password>"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

**Пример 3. Использование аутентификации Windows**

```json
{
    "name": "ODataLinkedService",
    "properties": {
        "type": "OData",
        "typeProperties": {
            "url": "<endpoint of OData source>",
            "authenticationType": "Windows",
            "userName": "<domain>\\<user>",
            "password": {
                "type": "SecureString",
                "value": "<password>"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

**Пример 4. Использование аутентификации с помощью ключа субъекта-службы**

```json
{
    "name": "ODataLinkedService",
    "properties": {
        "type": "OData",
        "typeProperties": {
            "url": "<endpoint of OData source>",
            "authenticationType": "AadServicePrincipal",
            "servicePrincipalId": "<service principal id>",
            "aadServicePrincipalCredentialType": "ServicePrincipalKey",
            "servicePrincipalKey": {
                "type": "SecureString",
                "value": "<service principal key>"
            },
            "tenant": "<tenant info, e.g. microsoft.onmicrosoft.com>",
            "aadResourceId": "<AAD resource URL>"
        }
    },
    "connectVia": {
        "referenceName": "<name of Integration Runtime>",
        "type": "IntegrationRuntimeReference"
    }
}
```

**Пример 5. Использование аутентификации на основе сертификата субъекта-службы**

```json
{
    "name": "ODataLinkedService",
    "properties": {
        "type": "OData",
        "typeProperties": {
            "url": "<endpoint of OData source>",
            "authenticationType": "AadServicePrincipal",
            "servicePrincipalId": "<service principal id>",
            "aadServicePrincipalCredentialType": "ServicePrincipalCert",
            "servicePrincipalEmbeddedCert": { 
                "type": "SecureString", 
                "value": "<base64 encoded string of (.pfx) certificate data>"
            },
            "servicePrincipalEmbeddedCertPassword": { 
                "type": "SecureString", 
                "value": "<password of your certificate>"
            },
            "tenant": "<tenant info, e.g. microsoft.onmicrosoft.com>",
            "aadResourceId": "<AAD resource e.g. https://tenant.sharepoint.com>"
        }
    },
    "connectVia": {
        "referenceName": "<name of Integration Runtime>",
        "type": "IntegrationRuntimeReference"
    }
}
```

## <a name="dataset-properties"></a>Свойства набора данных

Этот раздел содержит список свойств, поддерживаемых набором данных OData.

Полный список разделов и свойств, используемых для определения наборов данных, приведен в статье [Наборы данных и связанные службы в фабрике данных Azure](concepts-datasets-linked-services.md). 

Чтобы скопировать данные из OData, установите свойство **type** набора данных **ODataResource**. Поддерживаются следующие свойства:

| Свойство | ОПИСАНИЕ | Обязательно для заполнения |
|:--- |:--- |:--- |
| type | Свойство **type** для набора данных должно иметь значение **ODataResource**. | Да |
| path | Путь к ресурсу OData. | Да |

**Пример**

```json
{
    "name": "ODataDataset",
    "properties":
    {
        "type": "ODataResource",
        "linkedServiceName": {
            "referenceName": "<OData linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties":
        {
            "path": "Products"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Свойства действия копирования

Этот раздел содержит список свойств, поддерживаемых источником OData.

Полный список разделов и свойств, доступных для определения действий, см. в статье, посвященной [конвейерам и действиям в Фабрике данных Azure](concepts-pipelines-activities.md). 

### <a name="odata-as-source"></a>OData в качестве источника

Чтобы копировать данные из OData, установите тип **источника** **RelationalSource** в действии копирования. В разделе **source** действия копирования поддерживаются следующие свойства:

| Свойство | ОПИСАНИЕ | Обязательно для заполнения |
|:--- |:--- |:--- |
| type | Свойство **type** источника действия копирования должно иметь значение **RelationalSource**. | Да |
| query | Параметры запроса OData для фильтрации данных. Пример: `"$select=Name,Description&$top=5"`.<br/><br/>**Примечание**. Соединитель OData копирует данные из объединенного URL-адреса: `[URL specified in linked service]/[path specified in dataset]?[query specified in copy activity source]`. Дополнительные сведения см. в статье о [компонентах URL-адреса OData](https://www.odata.org/documentation/odata-version-3-0/url-conventions/). | Нет |

**Пример**

```json
"activities":[
    {
        "name": "CopyFromOData",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<OData input dataset name>",
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
                "type": "RelationalSource",
                "query": "$select=Name,Description&$top=5"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

## <a name="data-type-mapping-for-odata"></a>Сопоставление типов данных для OData

При копировании данных из OData используются следующие сопоставления типов данных OData с промежуточными типами данных Фабрики данных Azure. Дополнительные сведения о том, как действие копирования сопоставляет исходную схему и типы данных для приемника, см. в статье [Сопоставление схем в действии копирования](copy-activity-schema-and-type-mapping.md).

| Тип данных OData | Тип промежуточных данных фабрики данных |
|:--- |:--- |
| Edm.Binary | Byte[] |
| Edm.Boolean | Логический |
| Edm.Byte | Byte[] |
| Edm.DateTime | DateTime |
| Edm.Decimal | Decimal |
| Edm.Double | Double |
| Edm.Single | Single |
| Edm.Guid | Guid |
| Edm.Int16 | Int16 |
| Edm.Int32 | Int32 |
| Edm.Int64 | Int64 |
| Edm.SByte | Int16 |
| Edm.String | String |
| Edm.Time | TimeSpan |
| Edm.DateTimeOffset | DateTimeOffset |

> [!NOTE]
> Сложные типы данных OData (например **объекты**), не поддерживаются.


## <a name="next-steps"></a>Дальнейшие действия

В разделе [Поддерживаемые хранилища данных и форматы](copy-activity-overview.md##supported-data-stores-and-formats) приведен список хранилищ данных, которые поддерживаются в качестве источников и приемников для действия копирования в Фабрике данных Azure.