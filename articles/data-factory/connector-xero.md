---
title: Копирование данных из Xero с помощью фабрики данных Azure (предварительная версия) | Документы Майкрософт
description: Узнайте, как копировать данные из Xero в поддерживаемые хранилища данных-приемники с помощью действия копирования в конвейере фабрики данных Azure.
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
ms.openlocfilehash: 6793fbcc50711e10231b87fa6e1f11f54f90d325
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "60445440"
---
# <a name="copy-data-from-xero-using-azure-data-factory-preview"></a>Копирование данных из Xero с помощью фабрики данных Azure (предварительная версия)

В этой статье описывается, как с помощью действия копирования в фабрике данных Azure копировать данные из Xero. Это продолжение [статьи об обзоре действия копирования](copy-activity-overview.md), в которой представлены общие сведения о действии копирования.

> [!IMPORTANT]
> Сейчас этот соединитель доступен в режиме предварительной версии. Попробуйте его и оставьте свой отзыв. Если вы хотите использовать в своем решении зависимость от соединителей в предварительной версии, обратитесь в службу [поддержки Azure](https://azure.microsoft.com/support/).

## <a name="supported-capabilities"></a>Поддерживаемые возможности

Данные из Xero можно скопировать в любое поддерживаемое хранилище данных, используемое в качестве приемника. Список хранилищ данных, которые поддерживаются в качестве источников и приемников для действия копирования, приведен в таблице [Поддерживаемые хранилища данных и форматы](copy-activity-overview.md#supported-data-stores-and-formats).

В частности, этот соединитель Xero поддерживает:

- [частное приложение](https://developer.xero.com/documentation/getting-started/api-application-types) Xero (не общедоступное);
- все таблицы Xero (конечные точки API), кроме таблицы "Отчеты". 

Фабрика данных Azure имеет встроенный драйвер для настройки подключения. Поэтому с использованием этого соединителя вам не нужно устанавливать драйверы вручную.

## <a name="getting-started"></a>Приступая к работе

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Следующие разделы содержат сведения о свойствах, которые используются для определения сущностей фабрики данных, относящихся к соединителю Xero.

## <a name="linked-service-properties"></a>Свойства связанной службы

Для связанной службы Xero поддерживаются следующие свойства:

| Свойство | ОПИСАНИЕ | Обязательно для заполнения |
|:--- |:--- |:--- |
| type | Свойству type необходимо задать следующее значение: **Xero** | Да |
| host | Конечная точка сервера Xero (`api.xero.com`).  | Да |
| consumerKey | Ключ пользователя, связанный с приложением Xero. Пометьте это поле как SecureString, чтобы безопасно хранить его в фабрике данных, или [добавьте ссылку на секрет, хранящийся в Azure Key Vault](store-credentials-in-key-vault.md). | Да |
| privateKey | Закрытый ключ из PEM-файла, созданный для частного приложения Xero. Дополнительные сведения см. в статье [Create a public/private key pair](https://developer.xero.com/documentation/api-guides/create-publicprivate-key) (Создание открытого и закрытого ключей). Примечание для **создания privatekey.pem с numbits 512** с использованием `openssl genrsa -out privatekey.pem 512`; 1024 не поддерживается. Включает весь текст из PEM-файла, в том числе окончания строк Unix (\n). Пример см. ниже.<br/><br/>Пометьте это поле как SecureString, чтобы безопасно хранить его в фабрике данных, или [добавьте ссылку на секрет, хранящийся в Azure Key Vault](store-credentials-in-key-vault.md). | Да |
| useEncryptedEndpoints | Указывает, шифруются ли конечные точки источника данных с помощью протокола HTTPS. По умолчанию используется значение true.  | Нет |
| useHostVerification | Указывает, нужно ли, чтобы имя узла в сертификате сервера совпадало с именем узла сервера при подключении по протоколу SSL. По умолчанию используется значение true.  | Нет |
| usePeerVerification | Указывает, следует ли проверять удостоверение сервера при подключении по протоколу SSL. По умолчанию используется значение true.  | Нет |

**Пример.**

```json
{
    "name": "XeroLinkedService",
    "properties": {
        "type": "Xero",
        "typeProperties": {
            "host" : "api.xero.com",
            "consumerKey": {
                 "type": "SecureString",
                 "value": "<consumerKey>"
            },
            "privateKey": {
                 "type": "SecureString",
                 "value": "<privateKey>"
            }
        }
    }
}
```

**Пример значения закрытого ключа**

Включает весь текст из PEM-файла, в том числе окончания строк Unix (\n).

```
"-----BEGIN RSA PRIVATE KEY-----\nMII***************************************************P\nbu****************************************************s\nU/****************************************************B\nA*****************************************************W\njH****************************************************e\nsx*****************************************************l\nq******************************************************X\nh*****************************************************i\nd*****************************************************s\nA*****************************************************dsfb\nN*****************************************************M\np*****************************************************Ly\nK*****************************************************Y=\n-----END RSA PRIVATE KEY-----"
```

## <a name="dataset-properties"></a>Свойства набора данных

Полный список разделов и свойств, доступных для определения наборов данных, см. в статье о [наборах данных](concepts-datasets-linked-services.md). В этом разделе содержится список свойств, поддерживаемых набором данных Xero.

Чтобы копировать данные из Xero, установите свойство type набора данных **XeroObject**. Поддерживаются следующие свойства:

| Свойство | ОПИСАНИЕ | Обязательно для заполнения |
|:--- |:--- |:--- |
| type | Для свойства type набора данных необходимо задать следующее значение: **XeroObject** | Да |
| tableName | Имя таблицы. | Нет (если свойство query указано в источнике действия) |

**Пример**

```json
{
    "name": "XeroDataset",
    "properties": {
        "type": "XeroObject",
        "linkedServiceName": {
            "referenceName": "<Xero linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {}
    }
}
```

## <a name="copy-activity-properties"></a>Свойства действия копирования

Полный список разделов и свойств, используемых для определения действий, см. в статье [Конвейеры и действия в фабрике данных Azure](concepts-pipelines-activities.md). В этом разделе содержится список свойств, поддерживаемых источником Xero.

### <a name="xero-as-source"></a>Xero в качестве источника

Чтобы копировать данные из HTTP, установите тип источника **XeroSource** в действии копирования. В разделе **source** действия копирования поддерживаются следующие свойства:

| Свойство | ОПИСАНИЕ | Обязательно для заполнения |
|:--- |:--- |:--- |
| type | Свойство type источника действия копирования должно иметь следующее значение: **XeroSource** | Да |
| query | Используйте пользовательский SQL-запрос для чтения данных. Например, `"SELECT * FROM Contacts"`. | Нет (если для набора данных задано свойство tableName) |

**Пример.**

```json
"activities":[
    {
        "name": "CopyFromXero",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Xero input dataset name>",
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
                "type": "XeroSource",
                "query": "SELECT * FROM Contacts"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

При указании запроса Xero обратите внимание на следующее:

- Таблицы со сложными элементами необходимо разделять на несколько. Например, банковские переводы имеют сложную структуру данных LineItems. Поэтому данные перевода сопоставляются с таблицами `Bank_Transaction` и `Bank_Transaction_Line_Items`, где `Bank_Transaction_ID` — это внешний ключ, который связывает их.

- Данные Xero имеют две схемы: `Minimal` (по умолчанию) и `Complete`. Полная схема содержит необходимые таблицы вызовов, которые требуют дополнительные данные (например, столбец идентификатора) перед выполнением запроса.

Ниже приведены таблицы с одинаковой информацией в полной и минимальной схеме. Чтобы уменьшить число вызовов API, используйте минимальную схему (по умолчанию).

- Bank_Transactions
- Contact_Groups 
- Контакты 
- Contacts_Sales_Tracking_Categories 
- Contacts_Phones 
- Contacts_Addresses 
- Contacts_Purchases_Tracking_Categories 
- Credit_Notes 
- Credit_Notes_Allocations 
- Expense_Claims 
- Expense_Claim_Validation_Errors
- Счета 
- Invoices_Credit_Notes
- Invoices_ Prepayments 
- Invoices_Overpayments 
- Manual_Journals 
- Overpayments 
- Overpayments_Allocations 
- Prepayments 
- Prepayments_Allocations 
- Receipts 
- Receipt_Validation_Errors 
- Tracking_Categories

Следующие таблицы можно запрашивать только с использованием полной схемы:

- Complete.Bank_Transaction_Line_Items 
- Complete.Bank_Transaction_Line_Item_Tracking 
- Complete.Contact_Group_Contacts 
- Complete.Contacts_Contact_ Persons 
- Complete.Credit_Note_Line_Items 
- Complete.Credit_Notes_Line_Items_Tracking 
- Complete.Expense_Claim_ Payments 
- Complete.Expense_Claim_Receipts 
- Complete.Invoice_Line_Items 
- Complete.Invoices_Line_Items_Tracking
- Complete.Manual_Journal_Lines 
- Complete.Manual_Journal_Line_Tracking 
- Complete.Overpayment_Line_Items 
- Complete.Overpayment_Line_Items_Tracking 
- Complete.Prepayment_Line_Items 
- Complete.Prepayment_Line_Item_Tracking 
- Complete.Receipt_Line_Items 
- Complete.Receipt_Line_Item_Tracking 
- Complete.Tracking_Category_Options

## <a name="next-steps"></a>Дальнейшие действия
Список источников данных, которые поддерживает действие копирования, приведен в таблице [поддерживаемых хранилищ данных и форматов](copy-activity-overview.md#supported-data-stores-and-formats).
