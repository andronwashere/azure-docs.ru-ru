---
title: Управление учетными данными в клиентской библиотеке эластичной базы данных | Документация Майкрософт
description: Как установить правильный уровень учетных данных (от администратора до доступа только для чтения) для приложений эластичных баз данных.
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
ms.date: 01/03/2019
ms.openlocfilehash: 8a62ec95c715c08a8fddc09f0c8e5f5bba368556
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "66241758"
---
# <a name="credentials-used-to-access-the-elastic-database-client-library"></a>Учетные данные для доступа к клиентской библиотеке эластичной базы данных

[Клиентская библиотека эластичной базы данных](sql-database-elastic-database-client-library.md) использует три различных типа учетных данных для доступа к [диспетчеру карты сегментов](sql-database-elastic-scale-shard-map-management.md). В зависимости от выполняемых задач используйте учетные данные с максимально низким уровнем доступа.

* **Учетные данные управления** предназначены для создания диспетчера карты сегментов или операций с ним. (См. [глоссарий](sql-database-elastic-scale-glossary.md).)
* **Учетные данные для доступа**: обеспечивают доступ к существующему диспетчеру карт сегментов для получения сведений о сегментах.
* **Учетные данные подключения**: предназначены для подключения к сегментам.

См. также статью [Проверка подлинности и авторизация в Базе данных SQL Azure: предоставление доступа](sql-database-manage-logins.md).

## <a name="about-management-credentials"></a>Об учетных данных управления

Учетные данные управления используются для создания объекта **ShardMapManager** ([Java](/java/api/com.microsoft.azure.elasticdb.shard.mapmanager.shardmapmanager), [.NET](https://docs.microsoft.com/dotnet/api/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager)) в приложениях, работающих с картами сегментов. (Например, ознакомьтесь с разделами [Добавление сегмента с использованием средств эластичных баз данных](sql-database-elastic-scale-add-a-shard.md) и [Маршрутизация, зависящая от данных](sql-database-elastic-scale-data-dependent-routing.md).) Пользователь клиентской библиотеки эластичного масштабирования создает пользователей и имена для входа SQL и проверяет, имеют ли они разрешения на чтение и запись для базы данных глобального сопоставления сегментов, а также для всех баз данных сегментов. Эти учетные данные используются для обновления глобальной карты сегментов и локальных карт сегментов при внесении изменений в карты. Например, используйте учетные данные управления для создания объекта диспетчера карты сегментов с помощью **GetSqlShardMapManager** ([Java](/java/api/com.microsoft.azure.elasticdb.shard.mapmanager.shardmapmanagerfactory.getsqlshardmapmanager), [.NET](https://docs.microsoft.com/dotnet/api/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.getsqlshardmapmanager)):

```java
// Obtain a shard map manager.
ShardMapManager shardMapManager = ShardMapManagerFactory.GetSqlShardMapManager(smmAdminConnectionString,ShardMapManagerLoadPolicy.Lazy);
```

Переменная **smmAdminConnectionString** — это строка подключения с учетными данными управления. Идентификатор пользователя и пароль предоставляют доступ на чтение и запись как к базе данных сопоставления сегментов, так и к отдельным сегментам. Строка подключения для управления также содержит имя сервера и имя базы данных для идентификации базы данных глобальной карты сегментов. Вот типичная строка подключения, используемая в таком случае:

```java
"Server=<yourserver>.database.windows.net;Database=<yourdatabase>;User ID=<yourmgmtusername>;Password=<yourmgmtpassword>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30;”
```

Не используйте значения в формате username@server. Вместо этого используйте значение "имя_пользователя".  Это обусловлено тем, что учетные данные должны действовать для базы данных диспетчера карт сегментов и отдельных сегментов, которые могут находиться на разных серверах.

## <a name="access-credentials"></a>Учетные данные для доступа

При создании диспетчера карт сегментов в приложении, которое не администрирует эти карты сегментов, используйте учетные данные, дающие разрешения только на чтение глобальной карты сегментов. Данные, полученные из глобального сопоставления сегментов под этими учетными данными, используются для [зависящей от данных маршрутизации](sql-database-elastic-scale-data-dependent-routing.md) и для заполнения кэша сопоставлений сегментов на клиенте. Учетные данные передаются через тот же шаблон вызова **GetSqlShardMapManager**.

```java
// Obtain shard map manager.
ShardMapManager shardMapManager = ShardMapManagerFactory.GetSqlShardMapManager(smmReadOnlyConnectionString, ShardMapManagerLoadPolicy.Lazy);  
```

Обратите внимание на использование **smmReadOnlyConnectionString**. Для такого доступа применяются другие учетные данные от имени пользователей, **не являющихся администраторами**. Эти учетные данные не должны давать разрешения на запись в глобальную карту сегментов.

## <a name="connection-credentials"></a>Учетные данные подключения

При использовании метода **OpenConnectionForKey** ([Java](/java/api/com.microsoft.azure.elasticdb.shard.mapper.listshardmapper.openconnectionforkey), [.NET](https://docs.microsoft.com/dotnet/api/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmap.openconnectionforkey)) для доступа к сегменту, связанному с ключом сегментирования, необходимы дополнительные учетные данные. Эти учетные данные должны предоставлять разрешения на доступ только для чтения к таблицам локальных карт сегментов, размещенным в сегменте. Это необходимо для выполнения проверки подключения для маршрутизации на основе данных в сегменте. Этот фрагмент кода разрешает доступ к данным в контексте зависящей от данных маршрутизации.

```csharp
using (SqlConnection conn = rangeMap.OpenConnectionForKey<int>(targetWarehouse, smmUserConnectionString, ConnectionOptions.Validate))
```

В этом примере **smmUserConnectionString** содержит строку подключения для учетных данных пользователя. В базах данных SQL Azure обычно используется такая строка подключения для учетных данных пользователя:

```java
"User ID=<yourusername>; Password=<youruserpassword>; Trusted_Connection=False; Encrypt=True; Connection Timeout=30;”  
```

Как и для учетных данных администратора, не используйте значения в формате username@server. Вместо этого используйте просто «имя_пользователя@сервер».  Кроме того, обратите внимание, что строка подключения не содержит имя сервера и имя базы данных. Их нет, потому что вызов **OpenConnectionForKey** автоматически перенаправляет подключение в нужный сегмент на основе ключа. Таким образом, имя сервера и имя базы данных не указываются.

## <a name="see-also"></a>См. также

[Управление базами данных и именами входа в Базе данных SQL Azure](sql-database-manage-logins.md)

[Защита Базы данных SQL](sql-database-security-overview.md)

[Задания эластичной базы данных](elastic-jobs-overview.md)

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]
