---
title: Создание кластера и базы данных Azure Data Explorer с помощью C#
description: Сведения о создании кластера и базы данных Azure Data Explorer с использованием C#
author: oflipman
ms.author: oflipman
ms.reviewer: orspodek
ms.service: data-explorer
ms.topic: conceptual
ms.date: 06/03/2019
ms.openlocfilehash: 64f16c2ad6fdeeb47b747eab24587b43f3df5130
ms.sourcegitcommit: 4b647be06d677151eb9db7dccc2bd7a8379e5871
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/19/2019
ms.locfileid: "68355945"
---
# <a name="create-an-azure-data-explorer-cluster-and-database-by-using-c"></a>Создание кластера и базы данных Azure Data Explorer с помощью C#

> [!div class="op_single_selector"]
> * [Портал](create-cluster-database-portal.md)
> * [ИНТЕРФЕЙС КОМАНДНОЙ СТРОКИ](create-cluster-database-cli.md)
> * [PowerShell](create-cluster-database-powershell.md)
> * [C#](create-cluster-database-csharp.md)
> * [Python](create-cluster-database-python.md)

Azure Data Explorer — это быстрая и полностью управляемая служба для анализа большого объема потоковых данных в реальном времени, поступающих из приложений, а также с веб-сайтов, устройств Интернета вещей и т. д. Чтобы использовать обозреватель данных Azure, сначала нужно создать кластер и одну или несколько баз данных в этом кластере. Затем вы должны принять (загрузить) данные в базы данных, чтобы к ним можно было выполнять запросы. В этой статье вы создадите кластер и базу данных с помощью C#.

## <a name="prerequisites"></a>предварительные требования

* Если вы еще не установили Visual Studio 2019, вы можете скачать и использовать **бесплатный** [выпуск Visual Studio 2019 Community Edition](https://www.visualstudio.com/downloads/). При установке Visual Studio необходимо включить возможность **разработки для Azure**.

* Если у вас еще нет подписки Azure, создайте [бесплатную учетную запись](https://azure.microsoft.com/free/) Azure, прежде чем начинать работу.

## <a name="install-c-nuget"></a>Установка C# NuGet

1. Установите [пакет NuGet для Azure Data Explorer (Kusto)](https://www.nuget.org/packages/Microsoft.Azure.Management.Kusto/).

1. Установите [пакет NuGet Microsoft.IdentityModel.Clients.ActiveDirectory](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory/) для проверки подлинности.

## <a name="create-the-azure-data-explorer-cluster"></a>Создание кластера Azure Data Explorer

1. Создайте кластер, используя приведенный ниже код:

    ```csharp
    var resourceGroupName = "testrg";
    var clusterName = "mykustocluster";
    var location = "Central US";
    var sku = new AzureSku("D13_v2", 5);
    var cluster = new Cluster(location, sku);

    var authenticationContext = new AuthenticationContext("https://login.windows.net/{tenantName}");
    var credential = new ClientCredential(clientId: "xxxxxxxx-xxxxx-xxxx-xxxx-xxxxxxxxx", clientSecret: "xxxxxxxxxxxxxx");
    var result = await authenticationContext.AcquireTokenAsync(resource: "https://management.core.windows.net/", clientCredential: credential);

    var credentials = new TokenCredentials(result.AccessToken, result.AccessTokenType);

    var kustoManagementClient = new KustoManagementClient(credentials)
    {
        SubscriptionId = "xxxxxxxx-xxxxx-xxxx-xxxx-xxxxxxxxx"
    };

    kustoManagementClient.Clusters.CreateOrUpdate(resourceGroupName, clusterName, cluster);
    ```

   |**Параметр** | **Рекомендуемое значение** | **Описание поля**|
   |---|---|---|
   | clusterName | *mykustocluster* | Необходимое имя кластера.|
   | sku | *D13_v2* | Номер SKU, который будет использоваться для кластера. |
   | resourceGroupName | *testrg* | Имя группы ресурсов, в которой будет создан кластер. |

    Можно использовать дополнительные необязательные параметры, например емкость кластера и т. д.

1. Введите [свои учетные данные](https://docs.microsoft.com/dotnet/azure/dotnet-sdk-azure-authenticate?view=azure-dotnet).

1. Чтобы проверить, успешно ли создан кластер, можно выполнить следующую команду:

    ```csharp
    kustoManagementClient.Clusters.Get(resourceGroupName, clusterName);
    ```

Если результат содержит параметр `ProvisioningState` со значением `Succeeded`, это означает, что кластер создан успешно.

## <a name="create-the-database-in-the-azure-data-explorer-cluster"></a>Создание базы данных в кластере Azure Data Explorer

1. Создайте базу данных, используя приведенный ниже код:

    ```csharp
    var hotCachePeriod = new TimeSpan(3650, 0, 0, 0);
    var softDeletePeriod = new TimeSpan(3650, 0, 0, 0);
    var databaseName = "mykustodatabase";
    var database = new Database(location: location, softDeletePeriod: softDeletePeriod, hotCachePeriod: hotCachePeriod);

    kustoManagementClient.Databases.CreateOrUpdate(resourceGroupName, clusterName, databaseName, database);
    ```

   |**Параметр** | **Рекомендуемое значение** | **Описание поля**|
   |---|---|---|
   | clusterName | *mykustocluster* | Имя кластера, в котором необходимо создать базу данных.|
   | databaseName | *mykustodatabase* | Имя базы данных.|
   | resourceGroupName | *testrg* | Имя группы ресурсов, в которой будет создан кластер. |
   | softDeletePeriod | *3650:00:00:00* | Время, на протяжении которого данные будут храниться таким образом, чтобы они были доступны для запроса. |
   | hotCachePeriod | *3650:00:00:00* | Время, на протяжении которого данные будут храниться в кэше. |

2. Выполните следующую команду, чтобы просмотреть созданную базу данных:

    ```csharp
    kustoManagementClient.Databases.Get(resourceGroupName, clusterName, databaseName);
    ```

Теперь у вас есть кластер и база данных.

## <a name="clean-up-resources"></a>Очистка ресурсов

* Если вы планируете следовать нашим другим статьям, не заполняйте созданные ресурсы.
* Чтобы очистить ресурсы, удалите кластер. При удалении кластера также удаляются все содержащиеся в нем базы данных. Для удаления кластера используйте следующую команду:

    ```csharp
    kustoManagementClient.Clusters.Delete(resourceGroupName, clusterName);
    ```

## <a name="next-steps"></a>Следующие шаги

* [Краткое руководство. Прием данных с помощью пакета SDK .NET Standard для Azure Data Explorer (предварительная версия)](net-standard-ingest-data.md)
