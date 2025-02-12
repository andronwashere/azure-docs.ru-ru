---
title: Поставщик состояний сеансов ASP.NET кэша | Документация Майкрософт
description: Информация о хранении состояния сеанса ASP.NET с помощью кэша Azure для Redis
services: cache
documentationcenter: na
author: yegu-ms
manager: jhubbard
editor: tysonn
ms.assetid: 192f384c-836a-479a-bb65-8c3e6d6522bb
ms.service: cache
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: cache
ms.workload: tbd
ms.date: 05/01/2017
ms.author: yegu
ms.openlocfilehash: 7333fa51da1cd5bbd9175d56571ec1d17cbbe33f
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "65203924"
---
# <a name="aspnet-session-state-provider-for-azure-cache-for-redis"></a>Поставщик состояний сеансов ASP.NET для кэша Azure для Redis

Кэш Azure для Redis предоставляет поставщика состояний сеансов, который вы можете использовать для сохранения состояния сеанса в памяти с помощью кэша Azure для Redis вместо базы данных SQL Server. Для использования поставщика состояний сеансов с кэшированием сначала настройте кэш, а затем настройте приложение ASP.NET для кэша с помощью пакета кэша Azure для Redis NuGet для состояний сеансов.

Не хранить определенные виды состояний сеансов пользователя в реальном облачном приложении бывает нецелесообразно, но разные подходы по-разному влияют на производительность и масштабируемость. Если вам нужно хранить состояния, лучшим решением будет хранить небольшой объем состояний в файлах cookie. Если это невозможно, лучшей альтернативой будет использование состояний сеансов ASP.NET с помощью поставщика распределенного кэша в памяти. Худшее решение с точки зрения масштабируемости и производительности — использовать базу данных резервного поставщика состояний сеансов. Эта статья содержит указания по использованию поставщика состояний сеансов ASP.NET для кэша Azure для Redis. Сведения о других параметрах состояний сеансов см. в разделе [Параметры состояний сеансов ASP.NET](#aspnet-session-state-options).

## <a name="store-aspnet-session-state-in-the-cache"></a>Сохранение состояния сеанса ASP.NET в кэше

Чтобы настроить клиентское приложение в Visual Studio, используя пакет кэша Azure для Redis NuGet для состояний сеансов, в меню **Сервис** выберите **Диспетчер пакетов NuGet**, а затем **Консоль диспетчера пакетов**.

Выполните следующую команду в окне `Package Manager Console`:
    

```powershell
Install-Package Microsoft.Web.RedisSessionStateProvider
```

> [!IMPORTANT]
> Если вы используете функцию кластеризации на уровне "Премиум", вы увидите [RedisSessionStateProvider](https://www.nuget.org/packages/Microsoft.Web.RedisSessionStateProvider) 2.0.1 (или более новую версию), либо будет создано исключение. Переход на 2.0.1 или более новую версию — это критическое изменение. Дополнительные сведения см. на странице [v2.0.0 Breaking Change Details](https://github.com/Azure/aspnet-redis-providers/wiki/v2.0.0-Breaking-Change-Details) (Подробные сведения о критических изменениях в версии 2.0.0). На момент обновления этой статьи текущей версией пакета является версия 2.2.3.
> 
> 

Пакет NuGet поставщика состояний сеансов Redis зависит от пакета StackExchange.Redis.StrongName. Если в проекте отсутствует пакет StackExchange.Redis.StrongName, то он будет установлен.

>[!NOTE]
>Помимо пакета со строгим именем StackExchange.Redis.StrongName существует также версия с нестрогим именем StackExchange.Redis. Если проект использует версию с нестрогим именем StackExchange.Redis, то необходимо удалить ее. В противном случае в проекте возникнет конфликт имен. Дополнительные сведения об этих пакетах см. в разделе [Настройка клиентов кэша .NET](cache-dotnet-how-to-use-azure-redis-cache.md#configure-the-cache-clients).
>
>

Пакет NuGet скачивает и добавляет требуемые ссылки на сборку и добавляет следующий раздел в файл web.config. Этот раздел содержит необходимые настройки для приложения ASP.NET, позволяющие использовать поставщика состояний сеансов для кэша Azure для Redis.

```xml
<sessionState mode="Custom" customProvider="MySessionStateStore">
  <providers>
    <!-- Either use 'connectionString' OR 'settingsClassName' and 'settingsMethodName' OR use 'host','port','accessKey','ssl','connectionTimeoutInMilliseconds' and 'operationTimeoutInMilliseconds'. -->
    <!-- 'throwOnError','retryTimeoutInMilliseconds','databaseId' and 'applicationName' can be used with both options. -->
    <!--
      <add name="MySessionStateStore" 
        host = "127.0.0.1" [String]
        port = "" [number]
        accessKey = "" [String]
        ssl = "false" [true|false]
        throwOnError = "true" [true|false]
        retryTimeoutInMilliseconds = "5000" [number]
        databaseId = "0" [number]
        applicationName = "" [String]
        connectionTimeoutInMilliseconds = "5000" [number]
        operationTimeoutInMilliseconds = "1000" [number]
        connectionString = "<Valid StackExchange.Redis connection string>" [String]
        settingsClassName = "<Assembly qualified class name that contains settings method specified below. Which basically return 'connectionString' value>" [String]
        settingsMethodName = "<Settings method should be defined in settingsClass. It should be public, static, does not take any parameters and should have a return type of 'String', which is basically 'connectionString' value.>" [String]
        loggingClassName = "<Assembly qualified class name that contains logging method specified below>" [String]
        loggingMethodName = "<Logging method should be defined in loggingClass. It should be public, static, does not take any parameters and should have a return type of System.IO.TextWriter.>" [String]
        redisSerializerType = "<Assembly qualified class name that implements Microsoft.Web.Redis.ISerializer>" [String]
      />
    -->
    <add name="MySessionStateStore" type="Microsoft.Web.Redis.RedisSessionStateProvider"
         host=""
         accessKey=""
         ssl="true" />
  </providers>
</sessionState>
```

Раздел с комментариями содержит пример атрибутов и возможные настройки для каждого из них.

Задайте для атрибутов значения из колонки своего кэша на портале Microsoft Azure и настройте другие параметры по своему усмотрению. Инструкции по доступу к свойствам кэша см. в разделе [Настройка параметров кэша Azure для Redis](cache-configure.md#configure-azure-cache-for-redis-settings).

* **host** — укажите конечную точку кэша.
* **port** — используйте порт без или с SSL в зависимости от параметров SSL.
* **accessKey** — используйте для кэша первичный или вторичный ключ.
* **ssl** — задайте значение true, если нужно обеспечить безопасный обмен данными между клиентом и кэшем с помощью SSL. В противном случае задайте значение false. Обязательно укажите правильный порт.
  * Для новых кэшей не SSL порт по умолчанию запрещен. Если вы хотите, чтобы для этого параметра использовался SSL-порт, укажите значение true. Дополнительные сведения о том, как настроить использование порта без SSL, см. в статье [Настройка кэша](cache-configure.md#access-ports) в разделе [Порты доступа](cache-configure.md).
* **throwOnError** — укажите значение true, если в случае сбоя следует вызывать исключение, или укажите значение false, если вы не хотите получать уведомления о сбоях операций. Наличие сбоя можно обнаружить путем проверки статического свойства Microsoft.Web.Redis.RedisSessionStateProvider.LastException. Значение по умолчанию — true.
* **retryTimeoutInMilliseconds** — в течение этого интервала (указывается в миллисекундах) для неудачных операций выполняются повторные попытки. Первая повторная попытка происходит через 20 миллисекунд, а затем попытки повторяются каждую секунду до истечения интервала retryTimeoutInMilliseconds. Сразу после этого интервала операция повторяется еще один последний раз. Если операция по-прежнему заканчивается сбоем, вызывающему объекту отправляется исключение (в зависимости от значения параметра throwOnError). Значение по умолчанию — 0, что означает нулевое количество попыток.
* **databaseId** — этот параметр указывает базу данных, которую необходимо использовать для выходных данных кэша. Если значение в этом поле не задано, по умолчанию используется значение 0.
* **applicationName** — ключи хранятся в кэше Redis как `{<Application Name>_<Session ID>}_Data`. Такая схема именования позволяет нескольким приложениям совместно использовать один экземпляр Redis. Этот параметр — необязательный, и, если для него не указано другое значение, используется значение по умолчанию.
* **connectionTimeoutInMilliseconds** — этот параметр позволяет переопределить параметр connectTimeout в клиенте StackExchange.Redis. Если для параметра connectTimeout значение не указано, по умолчанию используется значение 5000. Дополнительную информацию см. в статье [Модель конфигурации StackExchange.Redis](https://go.microsoft.com/fwlink/?LinkId=398705).
* **operationTimeoutInMilliseconds** — этот параметр позволяет переопределить параметр syncTimeout в клиенте StackExchange.Redis. Если для параметра syncTimeout значение не указано, по умолчанию используется значение 1000. Дополнительную информацию см. в статье [Модель конфигурации StackExchange.Redis](https://go.microsoft.com/fwlink/?LinkId=398705).
* **redisSerializerType** — этот параметр позволяет задать пользовательскую сериализацию содержимого сеанса, отправляемого в Redis. Указанный тип должен реализовывать `Microsoft.Web.Redis.ISerializer` и объявить общедоступный конструктор без параметров. По умолчанию используется `System.Runtime.Serialization.Formatters.Binary.BinaryFormatter`.

Дополнительные сведения об этих свойствах см. в записи блога [Объявление поставщика состояний сеансов ASP.NET для кэша Redis](https://blogs.msdn.com/b/webdev/archive/2014/05/12/announcing-asp-net-session-state-provider-for-redis-preview-release.aspx).

Не забудьте закомментировать стандартный раздел поставщика состояний сеансов InProc в своем файле web.config.

```xml
<!-- <sessionState mode="InProc"
     customProvider="DefaultSessionProvider">
     <providers>
        <add name="DefaultSessionProvider"
              type="System.Web.Providers.DefaultSessionStateProvider,
                    System.Web.Providers, Version=1.0.0.0, Culture=neutral,
                    PublicKeyToken=31bf3856ad364e35"
              connectionStringName="DefaultConnection" />
      </providers>
</sessionState> -->
```

После выполнения этих шагов приложение будет настроено для использования поставщика состояний сеансов для кэша Azure для Redis. При использовании состояния сеанса в приложении состояние хранится в экземпляре кэша Azure для Redis.

> [!IMPORTANT]
> В отличие от данных, которые могут храниться в используемом по умолчанию в памяти поставщике состояний сеансов ASP.NET, данные, хранящиеся в кэше, должны быть сериализуемыми. При использовании поставщика состояний сеансов для кэша Redis убедитесь, что типы данных, хранящиеся в состоянии сеанса, — сериализуемые.
> 
> 

## <a name="aspnet-session-state-options"></a>Параметры состояния сеанса ASP.NET

* Поставщик состояний сеансов в памяти сохраняет состояние сеанса в памяти. Преимущества использования этого поставщика заключаются в его простоте и скорости. Но если вы используете поставщик в памяти, вы не сможете масштабировать свои веб-приложения, так как поставщик не распределяется.
* Поставщик состояний сеансов в базе данных SQL Server сохраняет состояние сеанса в базе данных SQL Server. Используйте этот поставщик, если требуется сохранять состояние сеанса в постоянном хранилище. Вы можете масштабировать свое веб-приложение, но использование SQL Server для сеанса может негативно повлиять на производительность веб-приложения. Кроме того, вы можете повысить производительность, применяя этот поставщик с [конфигурацией выполняющейся в памяти OLTP](https://blogs.msdn.microsoft.com/sqlserverstorageengine/2017/11/28/asp-net-session-state-with-sql-server-in-memory-oltp/).
* Распределенный в памяти поставщик состояний сеансов, например поставщик состояний сеансов для кэша Azure для Redis, обладает преимуществами предыдущих двух поставщиков. Веб-приложение может использовать простой, быстрый и масштабируемый поставщик состояний сеансов. Этот поставщик сохраняет состояние сеанса в кэше, поэтому ваше приложение должно принимать в расчет все характеристики, связанные с распределенным кэшем в памяти, например временные неполадки в сети. Рекомендации по использованию кэша см. в статье [Руководство по кэшированию](../best-practices-caching.md) из коллекции шаблонов и рекомендаций Майкрософт [Руководство по разработке и реализации облачного приложения Azure](https://github.com/mspnp/azure-guidance).

Дополнительные сведения о состоянии сеанса и другие рекомендации см. в статье [Рекомендации по веб-разработке (создание реальных облачных приложений с помощью Azure)](https://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/web-development-best-practices).

## <a name="next-steps"></a>Дальнейшие действия

Ознакомьтесь со статьей [ASP.NET Output Cache Provider for Azure Cache for Redis](cache-aspnet-output-cache-provider.md) (Поставщик кэша вывода ASP.NET для кэша Azure для Redis).
