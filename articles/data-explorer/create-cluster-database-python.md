---
title: Создание кластера и базы данных Azure Data Explorer с использованием Python
description: Сведения о создании кластера и базы данных Azure Data Explorer с использованием Python.
author: oflipman
ms.author: oflipman
ms.reviewer: orspodek
ms.service: data-explorer
ms.topic: conceptual
ms.date: 06/03/2019
ms.openlocfilehash: caac53aec08e234f320ee4278a5a58bbc62bff68
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "66494580"
---
# <a name="create-an-azure-data-explorer-cluster-and-database-by-using-python"></a>Создание кластера и базы данных Azure Data Explorer с использованием Python

> [!div class="op_single_selector"]
> * [Портал](create-cluster-database-portal.md)
> * [ИНТЕРФЕЙС КОМАНДНОЙ СТРОКИ](create-cluster-database-cli.md)
> * [PowerShell](create-cluster-database-powershell.md)
> * [C#](create-cluster-database-csharp.md)
> * [Python](create-cluster-database-python.md)
>  

Azure Data Explorer — это быстрая и полностью управляемая служба для анализа большого объема потоковых данных в реальном времени, поступающих из приложений, а также с веб-сайтов, устройств Интернета вещей и т. д. Чтобы использовать обозреватель данных Azure, сначала нужно создать кластер и одну или несколько баз данных в этом кластере. Затем вы должны принять (загрузить) данные в базы данных, чтобы к ним можно было выполнять запросы. В этой статье Создайте кластер и базы данных с помощью Python.

## <a name="prerequisites"></a>Технические условия

Если у вас еще нет подписки Azure, создайте [бесплатную учетную запись](https://azure.microsoft.com/free/) Azure, прежде чем начинать работу.

## <a name="install-python-package"></a>Установка пакета Python

Чтобы установить пакет Python для Azure Data Explorer (Kusto), откройте окно командной строки с Python в пути. Выполните следующую команду:

```
pip install azure-mgmt-kusto
```

## <a name="create-the-azure-data-explorer-cluster"></a>Создание кластера Azure Data Explorer

1. Создайте кластер, используя приведенную ниже команду:

    ```Python
    from azure.mgmt.kusto.kusto_management_client import KustoManagementClient
    from azure.mgmt.kusto.models import Cluster, AzureSku

    credentials = xxxxxxxxxxxxxxx
    
    subscription_id = 'xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxx'
    location = 'Central US'
    sku = 'D13_v2'
    capacity = 5
    resource_group_name = 'testrg'
    cluster_name = 'mykustocluster'
    cluster = Cluster(location=location, sku=AzureSku(name=sku, capacity=capacity))
    
    kustoManagementClient = KustoManagementClient(credentials, subscription_id)
    
    cluster_operations = kustoManagementClient.clusters
    
    cluster_operations.create_or_update(resource_group_name, cluster_name, cluster)
    ```

   |**Параметр** | **Рекомендуемое значение** | **Описание поля**|
   |---|---|---|
   | cluster_name | *mykustocluster* | Необходимое имя кластера.|
   | sku | *D13_v2* | Номер SKU, который будет использоваться для кластера. |
   | resource_group_name | *testrg* | Имя группы ресурсов, в которой будет создан кластер. |

    Можно использовать дополнительные необязательные параметры, например емкость кластера и т. д.
    
1. Введите [*свои учетные данные*](https://docs.microsoft.com/python/azure/python-sdk-azure-authenticate?view=azure-python).

1. Чтобы проверить, успешно ли создан кластер, можно выполнить следующую команду:

    ```Python
    cluster_operations.get(resource_group_name = resource_group_name, cluster_name= clusterName, custom_headers=None, raw=False)
    ```

Если результат содержит параметр `provisioningState` со значением `Succeeded`, это означает, что кластер создан успешно.

## <a name="create-the-database-in-the-azure-data-explorer-cluster"></a>Создание базы данных в кластере Azure Data Explorer

1. Создайте базу данных, используя приведенную ниже команду:

    ```Python
    from azure.mgmt.kusto.models import Database
    from datetime import timedelta
    
    softDeletePeriod = timedelta(days=3650)
    hotCachePeriod = timedelta(days=3650)
    databaseName="mykustodatabase"
    
    database_operations = kusto_management_client.databases 
    _database = Database(location=location,
                        soft_delete_period=softDeletePeriod,
                        hot_cache_period=hotCachePeriod)
    
    database_operations.create_or_update(resource_group_name = resource_group_name, cluster_name = clusterName, database_name = databaseName, parameters = _database)
    ```

   |**Параметр** | **Рекомендуемое значение** | **Описание поля**|
   |---|---|---|
   | cluster_name | *mykustocluster* | Имя кластера, в котором необходимо создать базу данных.|
   | database_name | *mykustodatabase* | Имя базы данных.|
   | resource_group_name | *testrg* | Имя группы ресурсов, в которой будет создан кластер. |
   | soft_delete_period | *3650 days, 0:00:00* | Время, на протяжении которого данные будут храниться таким образом, чтобы они были доступны для запроса. |
   | hot_cache_period | *3650 days, 0:00:00* | Время, на протяжении которого данные будут храниться в кэше. |

1. Выполните следующую команду, чтобы просмотреть созданную базу данных:

    ```Python
    database_operations.get(resource_group_name = resource_group_name, cluster_name = clusterName, database_name = databaseName)
    ```

Теперь у вас есть кластер и база данных.

## <a name="clean-up-resources"></a>Очистка ресурсов

* Если вы планируете выполнить другие статьи, сохраните созданные ресурсы.
* Чтобы очистить ресурсы, удалите кластер. При удалении кластера также удаляются все содержащиеся в нем базы данных. Для удаления кластера используйте следующую команду:

    ```Python
    cluster_operations.delete(resource_group_name = resource_group_name, cluster_name = clusterName)
    ```

## <a name="next-steps"></a>Дальнейшие действия

* [Краткое руководство. Прием данных с помощью библиотеки Python в Azure Data Explorer](python-ingest-data.md)
