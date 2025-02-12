---
title: Создание ресурсов Azure Cosmos DB и управление ими с помощью PowerShell
description: Используйте Azure Powershell для управления учетными записями, базами данных, контейнерами и пропускной способностью Azure Cosmos DB.
author: markjbrown
ms.service: cosmos-db
ms.topic: sample
ms.date: 07/09/2019
ms.author: mjbrown
ms.custom: seodec18
ms.openlocfilehash: b61c7bbc06d8d265e5dd5dddd31aceadce1f623b
ms.sourcegitcommit: 66237bcd9b08359a6cce8d671f846b0c93ee6a82
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/11/2019
ms.locfileid: "67797054"
---
# <a name="manage-azure-cosmos-db-sql-api-resources-using-powershell"></a>Управление ресурсами API SQL для Azure Cosmos DB с помощью PowerShell

В этом руководстве описывается, как с помощью PowerShell написать сценарии и автоматизировать управление ресурсами Azure Cosmos DB, включая учетную запись, базу данных, контейнер и пропускную способность. Управление ресурсами Azure Cosmos DB осуществляется с помощью командлета AzResource, направленного напрямую к поставщику ресурсов Azure Cosmos DB. Все свойства поставщика ресурсов Azure Cosmos DB, которыми можно управлять с помощью PowerShell, см. в статье со [схемой поставщика ресурсов Azure Cosmos DB](/azure/templates/microsoft.documentdb/allversions).

Для управления ресурсами Azure Cosmos DB между несколькими платформами можно использовать [Azure CLI](manage-with-cli.md), [REST API][rp-rest-api] или [портал Azure](create-sql-api-dotnet.md#create-account).

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="getting-started"></a>Начало работы

Следуйте инструкциям из статьи [Общие сведения об Azure PowerShell][powershell-install-configure], чтобы установить средство PowerShell и войти через него в учетную запись Azure.

* Чтобы выполнять следующие команды без запроса пользовательского подтверждения, добавьте в команду флаг `-Force`.
* Все следующие команды синхронные.

## <a name="azure-cosmos-accounts"></a>Учетные записи Azure Cosmos

В следующих разделах описано управление учетной записью Azure Cosmos, в том числе такие действия:

* [создание учетной записи Azure Cosmos;](#create-account)
* [обновление учетной записи Azure Cosmos;](#update-account)
* [вывод списка всех учетных записей Azure Cosmos в подписке;](#list-accounts)
* [создание учетной записи Azure Cosmos;](#get-account)
* [Удаление учетной записи Azure Cosmos](#delete-account)
* [обновление тегов учетной записи Azure Cosmos;](#update-tags)
* [вывод списка ключей учетной записи Azure Cosmos;](#list-keys)
* [повторное создание ключей для учетной записи Azure Cosmos;](#regenerate-keys)
* [вывод списка строк подключения для учетной записи Azure Cosmos;](#list-connection-strings)
* [изменение приоритета при отработке отказа в учетной записи Azure Cosmos.](#modify-failover-priority)

### <a id="create-account"></a> Создание учетной записи Azure Cosmos

Эта команда создает учетную запись базы данных Azure Cosmos DB с поддержкой [множества регионов][distribute-data-globally] и [политикой согласованности](consistency-levels.md) с ограниченным устареванием.

```azurepowershell-interactive
# Create an Azure Cosmos Account for Core (SQL) API
$resourceGroupName = "myResourceGroup"
$location = "West US 2"
$accountName = "mycosmosaccount" # must be lower case.

$locations = @(
    @{ "locationName"="West US 2"; "failoverPriority"=0 },
    @{ "locationName"="East US 2"; "failoverPriority"=1 }
)

$consistencyPolicy = @{
    "defaultConsistencyLevel"="BoundedStaleness";
    "maxIntervalInSeconds"=300;
    "maxStalenessPrefix"=100000
}

$CosmosDBProperties = @{
    "databaseAccountOfferType"="Standard";
    "locations"=$locations;
    "consistencyPolicy"=$consistencyPolicy;
    "enableMultipleWriteLocations"="false"
}

New-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName -Location $location `
    -Name $accountName -PropertyObject $CosmosDBProperties
```

* `$accountName`: имя учетной записи Azure Cosmos. Имя должно состоять из букв нижнего регистра, включать буквенно-цифровые символы, а также символ "-" и иметь длину от 3 до 31 символа.
* `$location`: расположение ресурсов учетной записи Azure Cosmos.
* `$locations`: регионы реплик для учетной записи базы данных. Каждая учетная запись базы данных должна содержать один регион записи со значением приоритета обработки отказа 0.
* `$consistencyPolicy`: уровень согласованности по умолчанию в учетной записи Azure Cosmos. Дополнительные сведения см. в статье о [настраиваемых уровнях согласованности в Azure Cosmos DB](consistency-levels.md).
* `$CosmosDBProperties`: значения свойств, передаваемые поставщику Azure Resource Manager Cosmos DB для подготовки учетной записи.

Учетные записи Azure Cosmos могут быть настроены с помощью брандмауэра IP-адресов или конечных точек службы виртуальной сети. Сведения о настройке брандмауэра IP-адресов для Azure Cosmos DB см. в [этой статье](how-to-configure-firewall.md).  Дополнительные сведения о включении конечных точек службы для Azure Cosmos DB см. в статье [Настройка доступа из виртуальных сетей (VNet)](how-to-configure-vnet-service-endpoint.md).

### <a id="list-accounts"></a> Вывод списка всех учетных записей Azure Cosmos в подписке

Эта команда позволяет вывести список всех учетных записей Azure Cosmos в подписке.

```azurepowershell-interactive
# List Azure Cosmos Accounts

Get-AzResource -ResourceType Microsoft.DocumentDb/databaseAccounts | ft
```

### <a id="get-account"></a> Получение свойств учетной записи Azure Cosmos

Эта команда позволяет получить свойства имеющейся учетной записи Azure Cosmos.

```azurepowershell-interactive
# Get the properties of an Azure Cosmos Account

$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"

Get-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName `
    -Name $accountName | Select-Object Properties
```

### <a id="update-account"></a> Обновление учетной записи Azure Cosmos

Эта команда позволяет обновить свойства учетной записи базы данных Azure Cosmos DB. В контексте свойств можно выполнить такие изменения:

* добавление или удаление регионов;
* изменение политики согласованности по умолчанию;
* изменение политики отработки отказа;
* изменение фильтра диапазона IP-адресов;
* изменение конфигурации виртуальной сети;
* включение поддержки нескольких источников.

> [!NOTE]
> Кроме того, эта команда позволяет добавлять или удалять регионы, но не изменять приоритеты при отработке отказа. Сведения об изменении приоритета при отработке отказа для учетной записи Azure Cosmos см. в [этом разделе](#modify-failover-priority).

```azurepowershell-interactive
# Update an Azure Cosmos Account and set Consistency level to Session

$resourceGroupName = "myResourceGroup"
$accountName = "myaccountname"

$account = Get-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName -Name $accountName

$consistencyPolicy = @{ "defaultConsistencyLevel"="Session" }

$account.Properties.consistencyPolicy = $consistencyPolicy
$CosmosDBProperties = $account.Properties

Set-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName `
    -Name $accountName -PropertyObject $CosmosDBProperties
```

### <a id="delete-account"></a> Удаление учетной записи Azure Cosmos

Эта команда позволяет удалить имеющуюся учетную запись Azure Cosmos.

```azurepowershell-interactive
# Delete an Azure Cosmos Account
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"

Remove-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName `
    -Name $accountName
```

### <a id="update-tags"></a> Обновление тегов учетной записи Azure Cosmos

В следующем примере показано, как установить [теги ресурсов Azure][azure-resource-tags] для учетной записи Azure Cosmos.

> [!NOTE]
> Эту команду можно выполнять вместе с командами создания или обновления. Для этого необходимо добавить флаг `-Tags` с соответствующим параметром.

```azurepowershell-interactive
# Update tags for an Azure Cosmos Account

$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"

$tags = @{
    "dept" = "Finance";
    "environment" = "Production"
}

Set-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName `
    -Name $accountName -Tags $tags
```

### <a id="list-keys"></a> Вывод ключей учетной записи

При создании учетной записи Azure Cosmos DB служба создает два главных ключа доступа, которые можно использовать для проверки подлинности при доступе к учетной записи Azure Cosmos DB. Предоставляя два ключа, Azure Cosmos DB позволяет вам повторно создавать ключи без перерывов в работе учетной записи Azure Cosmos DB. Ключи только для чтения, используемые для выполнения проверки подлинности операций чтения, также доступны. Доступны два ключа для чтения и записи (первичный и вторичный), а также два ключа только для чтения (первичный и вторичный).

```azurepowershell-interactive
# List keys for an Azure Cosmos Account

$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"

$keys = Invoke-AzResourceAction -Action listKeys `
    -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" `
    -ResourceGroupName $resourceGroupName -Name $accountName

Select-Object $keys
```

### <a id="list-connection-strings"></a> Вывод строк подключения

Для учетных записей MongoDB строку подключения приложения MongoDB к учетной записи базы данных можно получить с помощью следующей команды:

```azurepowershell-interactive
# List connection strings for an Azure Cosmos Account

$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"

$keys = Invoke-AzResourceAction -Action listConnectionStrings `
    -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" `
    -ResourceGroupName $resourceGroupName -Name $accountName

Select-Object $keys
```

### <a id="regenerate-keys"></a> Повторное создание ключей учетной записи

Для поддержания безопасности подключений ключи доступа к учетной записи Azure Cosmos необходимо периодически создавать повторно. Для учетной записи назначаются первичный и вторичный ключи доступа. Таким образом клиент может получить доступ с помощью одного ключа, пока второй повторно создается. Существует четыре типа ключей для учетной записи Azure Cosmos (Primary, Secondary, PrimaryReadonly и SecondaryReadonly).

```azurepowershell-interactive
# Regenerate the primary key for an Azure Cosmos Account

$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"

$keyKind = @{ "keyKind"="Primary" }

$keys = Invoke-AzResourceAction -Action regenerateKey `
    -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" `
    -ResourceGroupName $resourceGroupName -Name $accountName -Parameters $keyKind

Select-Object $keys
```

### <a id="modify-failover-priority"></a> Изменение приоритета при отработке отказа

Для учетных записей баз данных с поддержкой нескольких регионов можно изменить порядок повышения уровня вторичных реплик для чтения в учетной записи Cosmos в случае региональной отработки отказа в первичной реплике записи. При изменении региона с `failoverPriority=0` эту команду также можно использовать для инициации отработки аварийного восстановления, чтобы протестировать планирование аварийного восстановления.

В примере ниже предполагается, что учетная запись имеет приоритет при отработке отказа westus=0 и eastus=1, а затем регионы меняются.

> [!CAUTION]
> Если изменить свойство `locationName` для `failoverPriority=0`, для учетной записи Azure Cosmos активируется переход на другой ресурс вручную. Любые другие изменения приоритета не приведут к активации отработки отказа.

```azurepowershell-interactive
# Change the failover priority for an Azure Cosmos Account

$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"

$failoverPolicies = @(
    @{ "locationName"="East US"; "failoverPriority"=0 },
    @{ "locationName"="West US"; "failoverPriority"=1 }
)

Invoke-AzResourceAction -Action failoverPriorityChange `
    -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" `
    -ResourceGroupName $resourceGroupName -Name $accountName -Parameters $failoverPolicies
```

## <a name="azure-cosmos-database"></a>База данных Azure Cosmos

В следующих разделах описано управление базой данных Azure Cosmos, включая:

* [создание базы данных Azure Cosmos;](#create-db)
* [создание базы данных Azure Cosmos с общей пропускной способностью;](#create-db-ru)
* [получение сведений о пропускной способности базы данных Azure Cosmos;](#get-db-ru)
* [вывод списка всех баз данных Azure Cosmos в учетной записи;](#list-db)
* [получение сведений об одной базе данных Azure Cosmos;](#get-db)
* [удаление базы данных Azure Cosmos.](#delete-db)

### <a id="create-db"></a>Создание базы данных Azure Cosmos

```azurepowershell-interactive
# Create an Azure Cosmos database
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"
$databaseName = "database1"
$resourceName = $accountName + "/sql/" + $databaseName

$DataBaseProperties = @{
    "resource"=@{"id"=$databaseName}
}

New-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts/apis/databases" `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName `
    -Name $resourceName -PropertyObject $DataBaseProperties
```

### <a id="create-db-ru"></a>Создание базы данных Azure Cosmos с общей пропускной способностью

```azurepowershell-interactive
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"
$databaseName = "database2"
$resourceName = $accountName + "/sql/" + $databaseName

$DataBaseProperties = @{
    "resource"=@{ "id"=$databaseName };
    "options"=@{ "Throughput"="400" }
}

New-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts/apis/databases" `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName `
    -Name $resourceName -PropertyObject $DataBaseProperties
```

### <a id="get-db-ru"></a>Получение сведений о пропускной способности базы данных Azure Cosmos

```azurepowershell-interactive
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"
$databaseName = "database1"
$containerName = "container1"
$databaseThroughputResourceType = "Microsoft.DocumentDb/databaseAccounts/apis/databases/settings"
$databaseThroughputResourceName = $accountName + "/sql/" + $databaseName + "/throughput"

Get-AzResource -ResourceType $databaseThroughputResourceType `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName `
    -Name $databaseThroughputResourceName  | Select-Object Properties
```

### <a id="list-db"></a>Получение сведений обо всех базах данных Azure Cosmos в учетной записи

```azurepowershell-interactive
# Get all databases in an Azure Cosmos account
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"
$resourceName = $accountName + "/sql/"

Get-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts/apis/databases" `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName `
    -Name $resourceName  | Select-Object Properties
```

### <a id="get-db"></a>Получение сведений об одной базе данных Azure Cosmos

```azurepowershell-interactive
# Get a single database in an Azure Cosmos account
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"
$databaseName = "database1"
$resourceName = $accountName + "/sql/" + $databaseName

Get-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts/apis/databases" `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName `
    -Name $resourceName | Select-Object Properties
```

### <a id="delete-db"></a>Удаление базы данных Azure Cosmos

```azurepowershell-interactive
# Delete a database in an Azure Cosmos account
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"
$databaseName = "database1"
$resourceName = $accountName + "/sql/" + $databaseName
Remove-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts/apis/databases" `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName -Name $resourceName
```

## <a name="azure-cosmos-container"></a>Контейнер Azure Cosmos

В следующих разделах описано управление контейнером Azure Cosmos, включая:

* [создание контейнера Azure Cosmos;](#create-container)
* [создание контейнера Azure Cosmos с ключом большого раздела;](#create-container-big-pk)
* [получение сведений о пропускной способности контейнера Azure Cosmos;](#get-container-ru)
* [создание контейнера Azure Cosmos с общей пропускной способностью;](#create-container-ru)
* [создание контейнера Azure Cosmos с настраиваемой индексацией;](#create-container-custom-index)
* [создание контейнера Azure Cosmos с выключенной индексацией;](#create-container-no-index)
* [создание контейнера Azure Cosmos с уникальным ключом и сроком жизни;](#create-container-unique-key-ttl)
* [создание контейнера Azure Cosmos с разрешением конфликтов;](#create-container-lww)
* [вывод списка всех контейнеров Azure Cosmos в базе данных;](#list-containers)
* [получение сведений об отдельном контейнере Azure Cosmos в базе данных;](#get-container)
* [удаление контейнера Azure Cosmos.](#delete-container)

### <a id="create-container"></a>Создание контейнера Azure Cosmos

```azurepowershell-interactive
# Create an Azure Cosmos container with default indexes and throughput at 400 RU
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"
$databaseName = "database1"
$containerName = "container1"
$resourceName = $accountName + "/sql/" + $databaseName + "/" + $containerName

$ContainerProperties = @{
    "resource"=@{
        "id"=$containerName;
        "partitionKey"=@{
            "paths"=@("/myPartitionKey");
            "kind"="Hash"
        }
    };
    "options"=@{ "Throughput"="400" }
}

New-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts/apis/databases/containers" `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName `
    -Name $resourceName -PropertyObject $ContainerProperties
```

### <a id="create-container-big-pk"></a>Создание контейнера Azure Cosmos с ключом большого раздела

```azurepowershell-interactive
# Create an Azure Cosmos container with a large partition key value (version = 2)
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"
$databaseName = "database1"
$containerName = "container1"
$resourceName = $accountName + "/sql/" + $databaseName + "/" + $containerName

$ContainerProperties = @{
    "resource"=@{
        "id"=$containerName;
        "partitionKey"=@{
            "paths"=@("/myPartitionKey");
            "kind"="Hash";
            "version" = 2
        }
    };
    "options"=@{ "Throughput"="400" }
}

New-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts/apis/databases/containers" `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName `
    -Name $resourceName -PropertyObject $ContainerProperties
```

### <a id="get-container-ru"></a>Получение сведений о пропускной способности контейнера Azure Cosmos

```azurepowershell-interactive
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"
$databaseName = "database1"
$containerName = "container1"
$containerThroughputResourceType = "Microsoft.DocumentDb/databaseAccounts/apis/databases/containers/settings"
$containerThroughputResourceName = $accountName + "/sql/" + $databaseName + "/" + $containerName + "/throughput"

Get-AzResource -ResourceType $containerThroughputResourceType `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName `
    -Name $containerThroughputResourceName  | Select-Object Properties
```

### <a id="create-container-ru"></a>Создание контейнера Azure Cosmos с общей пропускной способностью

```azurepowershell-interactive
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"
$databaseName = "database1"
$containerName = "container2"
$resourceName = $accountName + "/sql/" + $databaseName + "/" + $containerName

$ContainerProperties = @{
    "resource"=@{
        "id"=$containerName; 
        "partitionKey"=@{
            "paths"=@("/myPartitionKey");
            "kind"="Hash"
        }
    };
    "options"=@{}
}

New-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts/apis/databases/containers" `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName `
    -Name $resourceName -PropertyObject $ContainerProperties 
```

### <a id="create-container-custom-index"></a>Создание контейнера Azure Cosmos с настраиваемой политикой индексации

```azurepowershell-interactive
# Create a container with a custom indexing policy
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"
$databaseName = "database1"
$containerName = "container3"
$resourceName = $accountName + "/sql/" + $databaseName + "/" + $containerName

$ContainerProperties = @{
    "resource"=@{
        "id"=$containerName; 
        "partitionKey"=@{
            "paths"=@("/myPartitionKey"); 
            "kind"="Hash"
        }; 
        "indexingPolicy"=@{
            "indexingMode"="Consistent";
            "includedPaths"= @(@{
                "path"="/*";
                "indexes"= @()
            });
            "excludedPaths"= @(@{
                "path"="/myPathToNotIndex/*"
            })
        }
    };
    "options"=@{ "Throughput"="400" }
}

New-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts/apis/databases/containers" `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName `
    -Name $resourceName -PropertyObject $ContainerProperties
```

### <a id="create-container-no-index"></a>Создание контейнера Azure Cosmos с выключенной индексацией

```azurepowershell-interactive
# Create an Azure Cosmos container with no indexing
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"
$databaseName = "database1"
$containerName = "container4"
$resourceName = $accountName + "/sql/" + $databaseName + "/" + $containerName

$ContainerProperties = @{
    "resource"=@{
        "id"=$containerName; 
        "partitionKey"=@{
            "paths"=@("/myPartitionKey");
            "kind"="Hash"
        };
        "indexingPolicy"=@{
            "indexingMode"="none"
        }
    };
    "options"=@{ "Throughput"="400" }
} 

New-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts/apis/databases/containers" `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName `
    -Name $resourceName -PropertyObject $ContainerProperties
```

### <a id="create-container-unique-key-ttl"></a>Создание контейнера Azure Cosmos с политикой создания уникального ключа и срока жизни

```azurepowershell-interactive
# Create a container with a unique key policy and TTL
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"
$databaseName = "database1"
$containerName = "container5"
$resourceName = $accountName + "/sql/" + $databaseName + "/" + $containerName

$ContainerProperties = @{
    "resource"=@{
        "id"=$containerName;
        "partitionKey"=@{
            "paths"=@("/myPartitionKey");
            "kind"="Hash"
        }; 
        "indexingPolicy"=@{
            "indexingMode"="Consistent";
            "includedPaths"= @(@{
                "path"="/*";
                "indexes"= @()
            });
            "excludedPaths"= @()
        };
        "uniqueKeyPolicy"= @{
            "uniqueKeys"= @(@{
                "paths"= @(
                    "/myUniqueKey1";
                    "/myUniqueKey2"
                )
            })
        };
        "defaultTtl"= 100;
    };
    "options"=@{ "Throughput"="400" }
}

New-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts/apis/databases/containers" `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName `
    -Name $resourceName -PropertyObject $ContainerProperties
```

### <a id="create-container-lww"></a>Создание контейнера Azure Cosmos с разрешением конфликтов

Для создания политики разрешения конфликтов, использующей хранимую процедуру, установите `"mode"="custom"` и задайте для пути разрешения имя хранимой процедуры: `"conflictResolutionPath"="myResolverStoredProcedure"`. Для записи всех конфликтов в канал ConflictsFeed и их отдельной обработки установите `"mode"="custom"` и `"conflictResolutionPath"=""`.

```azurepowershell-interactive
# Create container with last-writer-wins conflict resolution policy
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"
$databaseName = "database1"
$containerName = "container6"
$resourceName = $accountName + "/sql/" + $databaseName + "/" + $containerName

$ContainerProperties = @{
    "resource"=@{
        "id"=$containerName;
        "partitionKey"=@{
            "paths"=@("/myPartitionKey");
            "kind"="Hash"
        };
        "conflictResolutionPolicy"=@{
            "mode"="lastWriterWins";
            "conflictResolutionPath"="/myResolutionPath"
        }
    };
    "options"=@{ "Throughput"="400" }
}

New-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts/apis/databases/containers" `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName `
    -Name $resourceName -PropertyObject $ContainerProperties
```

### <a id="list-containers"></a>Вывод списка всех контейнеров Azure Cosmos в базе данных

```azurepowershell-interactive
# List all Azure Cosmos containers in a database
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"
$databaseName = "database1"
$resourceName = $accountName + "/sql/" + $databaseName

Get-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts/apis/databases/containers" `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName `
    -Name $resourceName | Select-Object Properties
```

### <a id="get-container"></a>Получение сведений об отдельном контейнере Azure Cosmos в базе данных

```azurepowershell-interactive
# Get a single Azure Cosmos container in a database
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"
$databaseName = "database1"
$containerName = "container5"
$resourceName = $accountName + "/sql/" + $databaseName + "/" + $containerName

Get-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts/apis/databases/containers" `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName `
    -Name $resourceName | Select-Object Properties
```

### <a id="delete-container"></a>Удаление контейнера Azure Cosmos

```azurepowershell-interactive
# Delete an Azure Cosmos container
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"
$databaseName = "database1"
$containerName = "container1"
$resourceName = $accountName + "/sql/" + $databaseName + "/" + $containerName

Remove-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts/apis/databases/containers" `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName -Name $resourceName
```

## <a name="next-steps"></a>Дополнительная информация

* [Все примеры для PowerShell](powershell-samples.md)
* [Управление учетной записью Azure Cosmos](how-to-manage-database-account.md)
* [Создание контейнера Azure Cosmos](how-to-create-container.md)
* [Настройка срока жизни в Azure Cosmos DB](how-to-time-to-live.md)

<!--Reference style links - using these makes the source content way more readable than using inline links-->

[powershell-install-configure]: https://docs.microsoft.com/azure/powershell-install-configure
[scaling-globally]: distribute-data-globally.md#EnableGlobalDistribution
[distribute-data-globally]: distribute-data-globally.md
[azure-resource-groups]: https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#resource-groups
[azure-resource-tags]: https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags
[rp-rest-api]: https://docs.microsoft.com/rest/api/cosmos-db-resource-provider/
