---
title: Маршрутизация, зависящая от данных, с использованием службы "База данных SQL Azure" | Документация Майкрософт
description: Как использовать класс ShardMapManager в приложениях .NET для маршрутизации, зависящей от данных, — функции сегментированных баз данных для Базы данных SQL Azure.
services: sql-database
ms.service: sql-database
ms.subservice: scale-out
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: ''
manager: craigg
ms.date: 01/25/2019
ms.openlocfilehash: fe9098592fcfde2d5e23b78a3e33f2b4ebb9e2dc
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "60584970"
---
# <a name="use-data-dependent-routing-to-route-a-query-to-appropriate-database"></a>Использование маршрутизации с зависимостью от данных для маршрутизации запроса в соответствующую базу данных

**Маршрутизация с зависимостью от данных** — это возможность использования данных в запросе для направления запроса к соответствующей базе данных. Это основная модель при работе с сегментированными базами данных. Контекст запроса может также использоваться для маршрутизации запроса, особенно в том случае, если ключ сегментирования не является частью запроса. Каждый конкретный запрос или транзакция в приложении, которое использует зависящую от данных маршрутизацию, ограничены обращением к одной отдельной базе данных за раз. Для средств эластичной базы данных SQL Azure эта маршрутизация выполняется с помощью класса **ShardMapManager** ([Java](/java/api/com.microsoft.azure.elasticdb.shard.mapmanager.shardmapmanager), [.NET](https://docs.microsoft.com/dotnet/api/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager)).

Приложение не должно отслеживать различные строки подключения или расположения базы данных, связанные с различными срезами данных в сегментированной среде. Вместо этого [диспетчер сопоставления сегментов](sql-database-elastic-scale-shard-map-management.md) при необходимости открывает подключения к соответствующим базам данных на основе данных в сопоставлении сегментов и значения ключа сегментирования, которое является целевым объектом запроса приложения. Обычно это ключ *customer_id*, *tenant_id*, *date_key* или другой конкретный идентификатор, являющийся основным параметром запроса к базе данных.

Дополнительные сведения см. в статье [Scaling Out SQL Server with Data Dependent Routing](https://technet.microsoft.com/library/cc966448.aspx) (Масштабирование SQL Server с помощью маршрутизации с зависимостью от данных).

## <a name="download-the-client-library"></a>Скачивание клиентской библиотеки

Скачать библиотеку можно из двух расположений:

* версия для Java доступна в репозитории [Maven Central Repository](https://search.maven.org/#search%7Cga%7C1%7Celastic-db-tools);
* версия для .NET доступна [здесь](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/).

## <a name="using-a-shardmapmanager-in-a-data-dependent-routing-application"></a>Использование ShardMapManager при маршрутизации с зависимостью от данных

Приложения должны получить объект **ShardMapManager** во время инициализации, используя вызов фабрики **GetSQLShardMapManager** ([Java](/java/api/com.microsoft.azure.elasticdb.shard.mapmanager.shardmapmanagerfactory.getsqlshardmapmanager), [.NET](https://docs.microsoft.com/dotnet/api/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.getsqlshardmapmanager)). В этом примере инициализируются как диспетчер **ShardMapManager**, так и конкретное сопоставление **ShardMap**, которое он содержит. В этом примере показаны методы GetSqlShardMapManager и GetRangeShardMap ([Java](/java/api/com.microsoft.azure.elasticdb.shard.mapmanager.shardmapmanager.getrangeshardmap), [.NET](https://docs.microsoft.com/previous-versions/azure/dn824173(v=azure.100))).

```Java
ShardMapManager smm = ShardMapManagerFactory.getSqlShardMapManager(connectionString, ShardMapManagerLoadPolicy.Lazy);
RangeShardMap<int> rangeShardMap = smm.getRangeShardMap(Configuration.getRangeShardMapName(), ShardKeyType.Int32);
```

```csharp
ShardMapManager smm = ShardMapManagerFactory.GetSqlShardMapManager(smmConnectionString, ShardMapManagerLoadPolicy.Lazy);
RangeShardMap<int> customerShardMap = smm.GetRangeShardMap<int>("customerMap"); 
```

### <a name="use-lowest-privilege-credentials-possible-for-getting-the-shard-map"></a>Для получения карты сегментов используйте учетную запись с минимально возможными привилегиями.

Если приложение не работает с картой сегментов, учетные данные, используемые в методе фабрики, должны иметь разрешения только для чтения в базе данных **глобального сопоставления сегментов**. Обычно эти учетные данные отличаются от учетных данных, используемых в открытых подключениях к диспетчеру карты сегментов. См. также статью [Учетные данные для доступа к клиентской библиотеке эластичной базы данных](sql-database-elastic-scale-manage-credentials.md).

## <a name="call-the-openconnectionforkey-method"></a>Вызовите метод OpenConnectionForKey.

Метод **ShardMap.OpenConnectionForKey** ([Java](/java/api/com.microsoft.azure.elasticdb.shard.mapper.listshardmapper.openconnectionforkey), [.NET](https://docs.microsoft.com/dotnet/api/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmap.openconnectionforkey)) возвращает подключение, готовое к выдаче команд в соответствующей базе данных на основе значения параметра **key**. Сведения о сегментации кэшируются в приложении с помощью диспетчера **ShardMapManager**, поэтому такие запросы обычно не сопровождаются поиском в базе данных **глобального сопоставления сегментов**.

```Java
// Syntax:
public Connection openConnectionForKey(Object key, String connectionString, ConnectionOptions options)
```

```csharp
// Syntax:
public SqlConnection OpenConnectionForKey<TKey>(TKey key, string connectionString, ConnectionOptions options)
```

* Параметр **key** используется как ключ поиска в карте сегментов для определения соответствующей базы данных для запроса.
* Строка **connectionString** используется для передачи только учетных данных пользователя для необходимого подключения. Имя базы данных и имя сервера не включаются в строку *connectionString*, так как метод определяет базу данных и сервер с помощью сопоставления **ShardMap**.
* Если среда, в которой находятся сегменты, может измениться и строки могут быть перемещены в другие базы данных в результате операций разбиения или слияния, то для параметра **connectionOptions** ([Java](/java/api/com.microsoft.azure.elasticdb.shard.mapper.connectionoptions), [.NET](https://docs.microsoft.com/dotnet/api/microsoft.azure.sqldatabase.elasticscale.shardmanagement.connectionoptions)) необходимо установить значение **ConnectionOptions.Validate**. Эта проверка включает краткий запрос для сопоставления локальной карты сегментов в целевой базе данных (не на глобальной карте сегментов), прежде чем подключение предоставляется приложению.

В случае сбоя при проверке локальной карты сегментов (что означает неправильный кэш) диспетчер карты сегментов запрашивает глобальную карту сегментирования, чтобы получить новые правильные значения для поиска, обновления кэша и возобновления подключения к соответствующей базе данных.

Используйте **ConnectionOptions.None** только в том случае, если внесение изменений в сопоставление сегментов не предполагается, т. е. когда приложение подключено к сети. В этом случае можно предположить, что кэшированные значения всегда являются правильными, и дополнительный проверочный вызов в два конца к целевой базе данных можно безопасно пропустить. Это позволяет снизить объем трафика базы данных. Кроме того, можно задать параметры **connectionOptions** через значение в файле конфигурации, указывающее, ожидаются ли изменения сегментов в течение определенного периода времени.  

В этом примере используется значение целочисленного ключа **CustomerID** с использованием объекта **ShardMap** с именем **customerShardMap**.  

```Java
int customerId = 12345;
int productId = 4321;
// Looks up the key in the shard map and opens a connection to the shard
try (Connection conn = shardMap.openConnectionForKey(customerId, Configuration.getCredentialsConnectionString())) {
    // Create a simple command that will insert or update the customer information
    PreparedStatement ps = conn.prepareStatement("UPDATE Sales.Customer SET PersonID = ? WHERE CustomerID = ?");

    ps.setInt(1, productId);
    ps.setInt(2, customerId);
    ps.executeUpdate();
} catch (SQLException e) {
    e.printStackTrace();
}
```

```csharp
int customerId = 12345;
int newPersonId = 4321;

// Connect to the shard for that customer ID. No need to call a SqlConnection
// constructor followed by the Open method.
using (SqlConnection conn = customerShardMap.OpenConnectionForKey(customerId, Configuration.GetCredentialsConnectionString(), ConnectionOptions.Validate))
{
    // Execute a simple command.
    SqlCommand cmd = conn.CreateCommand();
    cmd.CommandText = @"UPDATE Sales.Customer
                        SET PersonID = @newPersonID WHERE CustomerID = @customerID";

    cmd.Parameters.AddWithValue("@customerID", customerId);cmd.Parameters.AddWithValue("@newPersonID", newPersonId);
    cmd.ExecuteNonQuery();
}  
```

Метод **OpenConnectionForKey** возвращает новое, уже открытое подключение к нужной базе данных. Подключения, используемые в этом случае, по-прежнему имеют все преимущества объединенных подключений.

**Метод OpenConnectionForKeyAsync** ([Java](/java/api/com.microsoft.azure.elasticdb.shard.mapper.listshardmapper.openconnectionforkeyasync), [.NET](https://docs.microsoft.com/dotnet/api/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmap.openconnectionforkeyasync)) также доступен, если в приложении используется асинхронное программирование.

## <a name="integrating-with-transient-fault-handling"></a>Интеграция с обработкой временных сбоев

При разработке облачных приложений для доступа к данным рекомендуется убедиться, что временные сбои перехватываются приложением и что попытка выполнения операции осуществляется несколько раз, прежде чем возникнет сообщение об ошибке. Обработка временных сбоев в облачных приложениях рассматривается в статье "Transient Fault Handling" (Обработка временных сбоев) ([Java](/java/api/com.microsoft.azure.elasticdb.core.commons.transientfaulthandling), [.NET](https://docs.microsoft.com/previous-versions/msp-n-p/dn440719(v=pandp.60))).

Обработка временных сбоев может естественным образом сосуществовать с шаблоном маршрутизации с зависимостью от данных. Основным требованием является повтор всего запроса доступа к данным, включая блок **using** , с которым осуществляется подключение с маршрутизацией, зависящей от данных. Предыдущий пример можно переписать следующим образом.

### <a name="example---data-dependent-routing-with-transient-fault-handling"></a>Пример. Маршрутизация, зависящая от данных, с обработкой временных сбоев

```Java
int customerId = 12345;
int productId = 4321;
try {
    SqlDatabaseUtils.getSqlRetryPolicy().executeAction(() -> {
        // Looks up the key in the shard map and opens a connection to the shard
        try (Connection conn = shardMap.openConnectionForKey(customerId, Configuration.getCredentialsConnectionString())) {
            // Create a simple command that will insert or update the customer information
            PreparedStatement ps = conn.prepareStatement("UPDATE Sales.Customer SET PersonID = ? WHERE CustomerID = ?");

            ps.setInt(1, productId);
            ps.setInt(2, customerId);
            ps.executeUpdate();
        } catch (SQLException e) {
            e.printStackTrace();
        }
    });
} catch (Exception e) {
    throw new StoreException(e.getMessage(), e);
}
```

```csharp
int customerId = 12345;
int newPersonId = 4321;

Configuration.SqlRetryPolicy.ExecuteAction(() =&gt;
{
    // Connect to the shard for a customer ID.
    using (SqlConnection conn = customerShardMap.OpenConnectionForKey(customerId, Configuration.GetCredentialsConnectionString(), ConnectionOptions.Validate))
    {
        // Execute a simple command
        SqlCommand cmd = conn.CreateCommand();

        cmd.CommandText = @"UPDATE Sales.Customer
                            SET PersonID = @newPersonID
                            WHERE CustomerID = @customerID";

        cmd.Parameters.AddWithValue("@customerID", customerId);
        cmd.Parameters.AddWithValue("@newPersonID", newPersonId);
        cmd.ExecuteNonQuery();

        Console.WriteLine("Update completed");
    }
});
```

Пакеты, необходимые для реализации обработки временных сбоев, автоматически загружаются при построении примера приложения эластичной базы данных.

## <a name="transactional-consistency"></a>Согласованность транзакций

Свойства транзакций сохраняются для всех операций, являющихся локальными для сегмента. Например, транзакции, осуществленные посредством маршрутизации, зависящей от данных, выполняются в области целевого сегмента для подключения. В данный момент нет возможности связывания нескольких подключений с транзакцией, поэтому невозможно гарантировать осуществление транзакций для операций, выполняемых через сегменты.

## <a name="next-steps"></a>Дальнейшие действия

Сведения об отсоединении сегмента или его повторном присоединении см. в статье [Устранение проблем сопоставления сегментов с помощью класса RecoveryManager](sql-database-elastic-database-recovery-manager.md).

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]