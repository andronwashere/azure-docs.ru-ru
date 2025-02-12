---
title: Копирование данных из DB2 с помощью фабрики данных Azure | Документация Майкрософт
description: Узнайте, как копировать данные из DB2 в поддерживаемые хранилища данных-приемники с помощью действия копирования в конвейере фабрики данных Azure.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 08/17/2018
ms.author: jingwang
ms.openlocfilehash: 797db8d0dd321676a3fa436a328a9981a3d3ca3b
ms.sourcegitcommit: 5cb0b6645bd5dff9c1a4324793df3fdd776225e4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/21/2019
ms.locfileid: "67312049"
---
# <a name="copy-data-from-db2-by-using-azure-data-factory"></a>Копирование данных из DB2 с помощью фабрики данных Azure
> [!div class="op_single_selector" title1="Выберите версию службы фабрики данных, которую вы используете:"]
> * [Версия 1](v1/data-factory-onprem-db2-connector.md)
> * [Текущая версия](connector-db2.md)

В этой статье описывается, как с помощью действия копирования в фабрике данных Azure копировать данные из базы данных DB2. Это продолжение [статьи об обзоре действия копирования](copy-activity-overview.md), в которой представлены общие сведения о действии копирования.

## <a name="supported-capabilities"></a>Поддерживаемые возможности

Данные из базы данных DB2 можно скопировать в любое хранилище данных, поддерживаемое в качестве приемника. Список хранилищ данных, которые поддерживаются в качестве источников и приемников для действия копирования, приведен в таблице [Поддерживаемые хранилища данных и форматы](copy-activity-overview.md#supported-data-stores-and-formats).

В частности, этот соединитель DB2 поддерживает приведенные ниже платформы и версии IBM DB2 с архитектурой распределенной реляционной базы данных (DRDA) SQL Access Manager (SQLAM) версии 9, 10 и 11:

* IBM DB2 для z/OS версии 11.1
* IBM DB2 для z/OS версии 10.1
* IBM DB2 для i версии 7.3
* IBM DB2 для i версии 7.2
* IBM DB2 для i версии 7.1
* IBM DB2 для LUW версии 11
* IBM DB2 для LUW версии 10.5
* IBM DB2 для LUW версии 10.1

> [!TIP]
> Если отобразилось сообщение об ошибке "Пакет, соответствующий запросу на выполнение инструкции SQL, не найден. SQLSTATE=51002 SQLCODE=-805", это указывает на то, что в данной операционной системе не создан требуемый пакет для обычного пользователя. Следуйте этим инструкциям в соответствии с типом сервера DB2.
> - DB2 для i (AS400): позволяет опытному пользователю создать коллекцию для имени пользователя для входа перед использованием действия копирования. Команда: `create collection <username>`
> - DB2 для z/OS или LUW: использование учетной записи с высоким уровнем привилегий — опытного пользователя или администратора с правами на работу с пакетами и разрешениями BIND, BINDADD, GRANT EXECUTE TO PUBLIC — для однократного выполнения действия копирования, после чего необходимый пакет создается автоматически во время копирования. Для выполнения последующих действий копирования можно переключиться на обычную учетную запись.

## <a name="prerequisites"></a>Технические условия

Чтобы использовать копирование данных из базы данных DB2, которая не является общедоступной, необходимо настроить локальную среду IR. Дополнительные сведения о локальной среде IR см. в статье [How to create and configure Self-hosted Integration Runtime](create-self-hosted-integration-runtime.md) (Создание и настройка локальной среды IR). Среда выполнения интеграции предоставляет встроенный драйвер DB2, поэтому при копировании данных из DB2 вам не потребуется устанавливать драйвер вручную.

## <a name="getting-started"></a>Приступая к работе

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Следующие разделы содержат сведения о свойствах JSON, которые используются для определения сущностей фабрики данных, относящихся к соединителю DB2.

## <a name="linked-service-properties"></a>Свойства связанной службы

Для связанной службы DB2 поддерживаются следующие свойства:

| Свойство | ОПИСАНИЕ | Обязательно для заполнения |
|:--- |:--- |:--- |
| type | Свойству type необходимо задать значение **Db2** | Да |
| server |Имя сервера DB2. Вы можете указать номер порта следом за именем сервера, разделив их двоеточием, например `server:port`. |Да |
| database |Имя базы данных DB2. |Да |
| authenticationType |Тип проверки подлинности, используемый для подключения к базе данных DB2.<br/>Допустимое значение: **Базовый** |Да |
| username |Укажите имя пользователя для подключения к базе данных DB2. |Да |
| password |Введите пароль для учетной записи пользователя, указанной для выбранного имени пользователя. Пометьте это поле как SecureString, чтобы безопасно хранить его в фабрике данных, или [добавьте ссылку на секрет, хранящийся в Azure Key Vault](store-credentials-in-key-vault.md). |Yes |
| connectVia | [Среда выполнения интеграции](concepts-integration-runtime.md), используемая для подключения к хранилищу данных. Вы можете использовать локальную среду выполнения интеграции или среду выполнения интеграции Azure (если хранилище данных является общедоступным). Если не указано другое, по умолчанию используется интегрированная среда выполнения Azure. |Нет |

**Пример.**

```json
{
    "name": "Db2LinkedService",
    "properties": {
        "type": "Db2",
        "typeProperties": {
            "server": "<servername:port>",
            "database": "<dbname>",
            "authenticationType": "Basic",
            "username": "<username>",
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

## <a name="dataset-properties"></a>Свойства набора данных

Полный список разделов и свойств, доступных для определения наборов данных, см. в статье о наборах данных. Этот раздел содержит список свойств, поддерживаемых набором данных DB2.

Чтобы скопировать данные из DB2, установите свойство типа набора данных **RelationalTable**. Поддерживаются следующие свойства:

| Свойство | ОПИСАНИЕ | Обязательно для заполнения |
|:--- |:--- |:--- |
| type | Для набора данных необходимо задать значение **RelationalTable**. | Да |
| tableName | Имя таблицы в базе данных DB2. | Нет (если свойство query указано в источнике действия) |

**Пример**

```json
{
    "name": "DB2Dataset",
    "properties":
    {
        "type": "RelationalTable",
        "linkedServiceName": {
            "referenceName": "<DB2 linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {}
    }
}
```

## <a name="copy-activity-properties"></a>Свойства действия копирования

Полный список разделов и свойств, используемых для определения действий, см. в статье [Конвейеры и действия в фабрике данных Azure](concepts-pipelines-activities.md). Этот раздел содержит список свойств, поддерживаемых источником DB2.

### <a name="db2-as-source"></a>DB2 в качестве источника

Чтобы копировать данные из DB2, установите тип источника в действии копирования **RelationalSource**. В разделе **source** действия копирования поддерживаются следующие свойства:

| Свойство | ОПИСАНИЕ | Обязательно для заполнения |
|:--- |:--- |:--- |
| type | Свойству type источника действия копирования необходимо задать значение **RelationalSource**. | Да |
| query | Используйте пользовательский SQL-запрос для чтения данных. Например, `"query": "SELECT * FROM \"DB2ADMIN\".\"Customers\""`. | Нет (если для набора данных задано свойство tableName) |

**Пример.**

```json
"activities":[
    {
        "name": "CopyFromDB2",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<DB2 input dataset name>",
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
                "query": "SELECT * FROM \"DB2ADMIN\".\"Customers\""
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

## <a name="data-type-mapping-for-db2"></a>Сопоставление типов данных для DB2

При копировании данных из DB2 используются следующие сопоставления из типов данных DB2 с промежуточными типами данных фабрики данных Azure. Дополнительные сведения о том, как действие копирования сопоставляет исходную схему и типы данных для приемника, см. в статье [Сопоставление схем в действии копирования](copy-activity-schema-and-type-mapping.md).

| Тип базы данных DB2 | Тип промежуточных данных фабрики данных |
|:--- |:--- |
| BigInt |Int64 |
| Binary |Byte[] |
| Blob |Byte[] |
| Char |String |
| Clob |String |
| Date |Datetime |
| DB2DynArray |String |
| DbClob |String |
| Decimal |Decimal |
| DecimalFloat |Decimal |
| Double |Double |
| Float |Double |
| Graphic |String |
| Integer |Int32 |
| LongVarBinary |Byte[] |
| LongVarChar |String |
| LongVarGraphic |String |
| Numeric |Decimal |
| Real |Single |
| SmallInt |Int16 |
| Time |TimeSpan |
| Timestamp |Datetime |
| VarBinary |Byte[] |
| VarChar |String |
| VarGraphic |String |
| Xml |Byte[] |


## <a name="next-steps"></a>Дальнейшие действия
В таблице [Поддерживаемые хранилища данных](copy-activity-overview.md##supported-data-stores-and-formats) приведен список хранилищ данных, которые поддерживаются в качестве источников и приемников для действия копирования в фабрике данных Azure.
