---
title: Справочник разработчика C# по функциям Azure
description: Узнайте, как разрабатывать Функции Azure с помощью C#.
services: functions
documentationcenter: na
author: ggailey777
manager: jeconnoc
keywords: функции azure, функции, обработка событий, веб-перехватчики, динамические вычисления, независимая архитектура
ms.service: azure-functions
ms.devlang: dotnet
ms.topic: reference
ms.date: 09/12/2018
ms.author: glenga
ms.openlocfilehash: 388b389cca7c3e820ea3ccfd37a2a93ccd476b31
ms.sourcegitcommit: a6873b710ca07eb956d45596d4ec2c1d5dc57353
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/16/2019
ms.locfileid: "68254635"
---
# <a name="azure-functions-c-developer-reference"></a>Справочник разработчика C# по функциям Azure

<!-- When updating this article, make corresponding changes to any duplicate content in functions-reference-csharp.md -->

Эта статья описывает разработку функций Azure с помощью C# в библиотеках классов .NET.

Решение "Функции Azure" поддерживает языки программирования C# и скрипт C#. Рекомендации по [использованию C# на портале Azure](functions-create-function-app-portal.md) см. в [справочнике разработчика скриптов C# (.csx)](functions-reference-csharp.md).

В статье предполагается, что вы уже прочли следующие статьи:

* [Руководство для разработчиков по Функциям Azure](functions-reference.md)
* [Средства Visual Studio 2019 для функций Azure](functions-develop-vs.md)

## <a name="functions-class-library-project"></a>Проект библиотеки классов функций

В Visual Studio шаблон проекта **Функции Azure** создает проект библиотеки классов C#, содержащий следующие файлы:

* [host.json](functions-host-json.md) — хранит параметры конфигурации, которые влияют на все функции в проекте при выполнении в локальной среде или в Azure.
* [local.settings.json](functions-run-local.md#local-settings-file) — хранит параметры приложений и строки подключения, которые используются при выполнении в локальной среде. Этот файл содержит секретные данные и не будет опубликован в приложении-функции в Azure. Вместо этого [добавьте параметры приложения в приложение функции](functions-develop-vs.md#function-app-settings).

При построении проекта структура папок, которая выглядит следующим образом, создается в выходном каталоге сборки:

```
<framework.version>
 | - bin
 | - MyFirstFunction
 | | - function.json
 | - MySecondFunction
 | | - function.json
 | - host.json
```

Этот каталог — то, что будет развернуто в вашем приложении-функции в Azure. Расширения привязки, необходимые в [версии 2.x](functions-versions.md) среды выполнения функций, [добавляются в проект как пакеты NuGet](./functions-bindings-register.md#vs).

> [!IMPORTANT]
> Процесс сборки создает файл *function.json* для каждой функции. Этот файл *function.json* не предназначен для непосредственного редактирования. Невозможно изменить конфигурацию привязки или отключить функцию путем редактирования этого файла. Чтобы узнать, как отключить функцию, см. раздел [Отключение функций](disable-function.md#functions-2x---c-class-libraries).

## <a name="methods-recognized-as-functions"></a>Методы, распознаваемые как функции

В библиотеке классов функция — это статический метод с `FunctionName` и атрибутом триггера, как показано в следующем примере:

```csharp
public static class SimpleExample
{
    [FunctionName("QueueTrigger")]
    public static void Run(
        [QueueTrigger("myqueue-items")] string myQueueItem, 
        ILogger log)
    {
        log.LogInformation($"C# function processed: {myQueueItem}");
    }
} 
```

Атрибут `FunctionName` помечает метод как точку входа функции. Имя должно быть уникальным в пределах проекта, начинаться с буквы и содержать только буквы, цифры, `_`и `-`, до 127 символов. Шаблоны проектов часто создают метод `Run`, но метод может иметь любое допустимое имя для метода C#.

Атрибут триггера указывает тип триггера и привязывает входные данные к параметру метода. Пример функции срабатывает по сообщению очереди, а сообщение очереди передается методу в параметре `myQueueItem`.

## <a name="method-signature-parameters"></a>Параметры сигнатуры метода

Сигнатура метода может содержать параметры, отличные от используемых с атрибутом триггера. Ниже приведены некоторые дополнительные параметры, которые можно включить:

* [Входные и выходные привязки](functions-triggers-bindings.md), помеченные как таковые путем дополнения атрибутами.  
* Параметр `ILogger` или `TraceWriter` ([только для версий 1.x](functions-versions.md#creating-1x-apps)) для [ведения журнала](#logging).
* Параметр `CancellationToken` для [нормального завершения работы](#cancellation-tokens).
* Параметры [выражений привязок](./functions-bindings-expressions-patterns.md) для получения метаданных триггера.

Порядок параметров в сигнатуре функции не имеет значения. Например, можно указать параметры триггера до или после других привязок, а параметр для средства ведения журнала — до или после параметров триггера или привязки.

### <a name="output-binding-example"></a>Пример выходной привязки

В следующем примере изменяется предыдущий путем добавления привязки очереди вывода. Функция записывает сообщение очереди, запускающее функцию для нового сообщения очереди в другую очередь.

```csharp
public static class SimpleExampleWithOutput
{
    [FunctionName("CopyQueueMessage")]
    public static void Run(
        [QueueTrigger("myqueue-items-source")] string myQueueItem, 
        [Queue("myqueue-items-destination")] out string myQueueItemCopy,
        ILogger log)
    {
        log.LogInformation($"CopyQueueMessage function processed: {myQueueItem}");
        myQueueItemCopy = myQueueItem;
    }
}
```

В справочных статьях по привязкам (например, [Привязки хранилища очередей Azure для службы "Функции Azure"](functions-bindings-storage-queue.md)) объясняется, какие типы параметров можно использовать с триггерами, а также с атрибутами входных или выходных привязок.

### <a name="binding-expressions-example"></a>Пример выражений привязки

Приведенный ниже код позволяет получить имя очереди для мониторинга из настроек приложения и время создания сообщения очереди в параметре `insertionTime`.

```csharp
public static class BindingExpressionsExample
{
    [FunctionName("LogQueueMessage")]
    public static void Run(
        [QueueTrigger("%queueappsetting%")] string myQueueItem,
        DateTimeOffset insertionTime,
        ILogger log)
    {
        log.LogInformation($"Message content: {myQueueItem}");
        log.LogInformation($"Created at: {insertionTime}");
    }
}
```

## <a name="autogenerated-functionjson"></a>Автоматически созданный файл function.json

Процесс сборки создает файл *function.json* в папке функции в папке сборки. Как уже говорилось, этот файл не предназначен для непосредственного редактирования. Невозможно изменить конфигурацию привязки или отключить функцию путем редактирования этого файла. 

Назначение этого файла — предоставить сведения контроллеру масштаба для [принятия решений о масштабировании плана потребления](functions-scale.md#how-the-consumption-and-premium-plans-work). По этой причине файл содержит только сведения о триггерах, но не о входных или выходных привязках.

Создаваемый файл *function.json* содержит свойство `configurationSource`, которое указывает среде выполнения использовать атрибуты .NET для привязок, вместо конфигурации *function.json*. Ниже приведен пример:

```json
{
  "generatedBy": "Microsoft.NET.Sdk.Functions-1.0.0.0",
  "configurationSource": "attributes",
  "bindings": [
    {
      "type": "queueTrigger",
      "queueName": "%input-queue-name%",
      "name": "myQueueItem"
    }
  ],
  "disabled": false,
  "scriptFile": "..\\bin\\FunctionApp1.dll",
  "entryPoint": "FunctionApp1.QueueTrigger.Run"
}
```

## <a name="microsoftnetsdkfunctions"></a>Microsoft.NET.Sdk.Functions

Создание файла *function.json* выполняется пакетом NuGet [Microsoft\.NET\.Sdk\.Functions](https://www.nuget.org/packages/Microsoft.NET.Sdk.Functions). 

Тот же пакет используется в обеих версиях (1.x и 2.x) среды выполнения Функций. Проекты 1.x и 2.x отличаются только требуемой версией .NET Framework. Ниже приведены фрагменты файлов *.csproj* с разными требуемыми версиями .NET Framework и одним пакетом `Sdk`.

**Функции 1.x**

```xml
<PropertyGroup>
  <TargetFramework>net461</TargetFramework>
</PropertyGroup>
<ItemGroup>
  <PackageReference Include="Microsoft.NET.Sdk.Functions" Version="1.0.8" />
</ItemGroup>
```

**Функции 2.x**

```xml
<PropertyGroup>
  <TargetFramework>netcoreapp2.1</TargetFramework>
  <AzureFunctionsVersion>v2</AzureFunctionsVersion>
</PropertyGroup>
<ItemGroup>
  <PackageReference Include="Microsoft.NET.Sdk.Functions" Version="1.0.8" />
</ItemGroup>
```

К зависимостям пакетов `Sdk` относятся триггеры и привязки. Проект 1. x ссылается на триггеры и привязки 1. x, так как эти триггеры и привязки предназначены для .NET Framework, а триггеры и привязки 2. x предназначены для .NET Core.

Пакет `Sdk` также зависит от пакета [Newtonsoft.Json](https://www.nuget.org/packages/Newtonsoft.Json) и косвенно от пакета [WindowsAzure.Storage](https://www.nuget.org/packages/WindowsAzure.Storage). Эти зависимости гарантируют, что проект использует версии пакетов, совместимые с версией среды выполнения Функций, для которой он предназначен. Например, при использовании платформы .NET Framework 4.6.1 пакет `Newtonsoft.Json` имеет версию 11, но среда выполнения Функций, предназначенная для платформы .NET Framework 4.6.1, совместима только с пакетом `Newtonsoft.Json` версии 9.0.1. Поэтому код функции в этом проекте также должен использовать пакет `Newtonsoft.Json` версии 9.0.1.

Исходный код пакета `Microsoft.NET.Sdk.Functions` доступен в репозитории GitHub [azure\-functions\-vs\-build\-sdk](https://github.com/Azure/azure-functions-vs-build-sdk).

## <a name="runtime-version"></a>Версия среды выполнения

Visual Studio выполняет проекты Функций с помощью [основных инструментов Функций Azure](functions-run-local.md#install-the-azure-functions-core-tools). Основные инструменты — это интерфейс командной строки среды выполнения Функций.

Если вы установите основные инструменты с помощью npm, это не повлияет на версию инструментов, используемую Visual Studio. Версии основных инструментов среды выполнения Функций 1.x хранятся в файле *%USERPROFILE%\AppData\Local\Azure.Functions.Cli*. Visual Studio использует последнюю версию. Основные инструменты Функций 2.x включены в расширение **Инструменты для Функций Azure и веб-заданий**. Используемую в 1.x и 2.x версию можно просмотреть в выходных данных консоли при запуске проекта Функций:

```terminal
[3/1/2018 9:59:53 AM] Starting Host (HostId=contoso2-1518597420, Version=2.0.11353.0, ProcessId=22020, Debug=False, Attempt=0, FunctionsExtensionVersion=)
```

## <a name="supported-types-for-bindings"></a>Поддерживаемые типы для привязок

Все привязки поддерживают определенные типы. Например, атрибут триггера большого двоичного объекта можно применить к строковому параметру, параметру POCO, параметру `CloudBlockBlob` или любому из нескольких других поддерживаемых типов. В [справочной статье о привязках для больших двоичных объектов](functions-bindings-storage-blob.md#trigger---usage) содержится список всех поддерживаемых типов параметров. Дополнительные сведения см. в статье о [триггерах и привязках](functions-triggers-bindings.md) и в [справочной документации по каждому типу привязки](functions-triggers-bindings.md#next-steps).

[!INCLUDE [HTTP client best practices](../../includes/functions-http-client-best-practices.md)]

## <a name="binding-to-method-return-value"></a>Привязка к возвращаемому значению метода

Возвращаемое значение метода можно использовать для привязки выходных данных. Для этого примените атрибут к возвращаемому значению метода. Примеры см. в статье о [триггерах и привязках](./functions-bindings-return-value.md). 

Используйте возвращаемое значение, только если в результате успешного выполнения функции всегда возвращается значение для передачи в привязку для вывода. В противном случае используйте `ICollector` или `IAsyncCollector`, как указано в следующем разделе.

## <a name="writing-multiple-output-values"></a>Написание нескольких значений выходных данных

Чтобы записать несколько значений в привязку для вывода или если после успешного вызова функции не возвращается значение для передачи в привязку для вывода, используйте типы [`ICollector`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/ICollector.cs) или [`IAsyncCollector`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/IAsyncCollector.cs). Эти типы представляют собой доступные только для записи коллекции, записываемые в выходную привязку по завершении метода.

В следующем примере записываются несколько сообщений очереди в ту же очередь с помощью `ICollector`:

```csharp
public static class ICollectorExample
{
    [FunctionName("CopyQueueMessageICollector")]
    public static void Run(
        [QueueTrigger("myqueue-items-source-3")] string myQueueItem,
        [Queue("myqueue-items-destination")] ICollector<string> myDestinationQueue,
        ILogger log)
    {
        log.LogInformation($"C# function processed: {myQueueItem}");
        myDestinationQueue.Add($"Copy 1: {myQueueItem}");
        myDestinationQueue.Add($"Copy 2: {myQueueItem}");
    }
}
```

## <a name="logging"></a>Ведение журнала

Для записи выходных данных в потоковые журналы в C# включите аргумент с типом [ILogger](https://docs.microsoft.com/dotnet/api/microsoft.extensions.logging.ilogger). Мы рекомендуем использовать имя `log`, как показано в следующем примере:  

```csharp
public static class SimpleExample
{
    [FunctionName("QueueTrigger")]
    public static void Run(
        [QueueTrigger("myqueue-items")] string myQueueItem, 
        ILogger log)
    {
        log.LogInformation($"C# function processed: {myQueueItem}");
    }
} 
```

Не используйте `Console.Write` в Функциях Azure. Дополнительные сведения см. в разделе [Запись журналов в функциях C#](functions-monitoring.md#write-logs-in-c-functions) статьи **Мониторинг Функций Azure**.

## <a name="async"></a>Асинхронный режим

Чтобы сделать функцию [асинхронной](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/async/), используйте ключевое слово `async` и верните объект `Task`.

```csharp
public static class AsyncExample
{
    [FunctionName("BlobCopy")]
    public static async Task RunAsync(
        [BlobTrigger("sample-images/{blobName}")] Stream blobInput,
        [Blob("sample-images-copies/{blobName}", FileAccess.Write)] Stream blobOutput,
        CancellationToken token,
        ILogger log)
    {
        log.LogInformation($"BlobCopy function processed.");
        await blobInput.CopyToAsync(blobOutput, 4096, token);
    }
}
```

Вы не можете использовать параметры `out` в асинхронных функциях. Вместо этих параметров для выходных привязок используйте [возвращаемое значение функции](#binding-to-method-return-value) или [объект сборщика](#writing-multiple-output-values).

## <a name="cancellation-tokens"></a>Токены отмены

Функция может принимать параметр [CancellationToken](/dotnet/api/system.threading.cancellationtoken), который позволяет операционной системе передавать в ваш код сведения о том, что выполнение функции будет завершено. Это уведомление можно использовать для предотвращения ситуации, когда выполнение функции завершается неожиданно, оставляя данные в несогласованном состоянии.

В следующем примере показано, как проверить, не приближается ли завершение выполнения функции.

```csharp
public static class CancellationTokenExample
{
    public static void Run(
        [QueueTrigger("inputqueue")] string inputText,
        TextWriter logger,
        CancellationToken token)
    {
        for (int i = 0; i < 100; i++)
        {
            if (token.IsCancellationRequested)
            {
                logger.WriteLine("Function was cancelled at iteration {0}", i);
                break;
            }
            Thread.Sleep(5000);
            logger.WriteLine("Normal processing for queue message={0}", inputText);
        }
    }
}
```

## <a name="environment-variables"></a>Переменные среды

Чтобы получить значение переменной среды или значение параметра приложения, используйте `System.Environment.GetEnvironmentVariable`, как показано в следующем примере кода.

```csharp
public static class EnvironmentVariablesExample
{
    [FunctionName("GetEnvironmentVariables")]
    public static void Run([TimerTrigger("0 */5 * * * *")]TimerInfo myTimer, ILogger log)
    {
        log.LogInformation($"C# Timer trigger function executed at: {DateTime.Now}");
        log.LogInformation(GetEnvironmentVariable("AzureWebJobsStorage"));
        log.LogInformation(GetEnvironmentVariable("WEBSITE_SITE_NAME"));
    }

    public static string GetEnvironmentVariable(string name)
    {
        return name + ": " +
            System.Environment.GetEnvironmentVariable(name, EnvironmentVariableTarget.Process);
    }
}
```

Параметры приложения могут считываться из переменных среды при разработке локально и при запуске в Azure. При локальной разработке параметры приложения поступают из коллекции `Values` файла *local.settings.json*. Значение именованного параметра приложения `GetEnvironmentVariable("<app setting name>")` извлекается в локальной среде и среде Azure. Например, при локальном запуске будет возвращено "Имя_сайта", если файл *local.settings.json* содержит `{ "Values": { "WEBSITE_SITE_NAME": "My Site Name" } }`.

Свойство [System.Configuration.ConfigurationManager.AppSettings](https://docs.microsoft.com/dotnet/api/system.configuration.configurationmanager.appsettings) — альтернативный API-интерфейс для получения значения параметра приложения, но рекомендуется использовать `GetEnvironmentVariable`, как показано ниже.

## <a name="binding-at-runtime"></a>Привязка во время выполнения

Для C# и других языков .NET можно использовать шаблон [императивной](https://en.wikipedia.org/wiki/Imperative_programming) привязки, которая отличается от [*декларативной*](https://en.wikipedia.org/wiki/Declarative_programming) привязки в атрибутах. Императивную привязку удобно использовать, когда параметры привязки должны вычисляться не при проектировании, а во время выполнения. С использованием такого шаблона можно моментально выполнить привязку к поддерживаемым входным и выходным привязкам в коде функции.

Определите принудительную привязку следующим образом.

- **Не** добавляйте атрибут в сигнатуру функции для нужных императивных привязок.
- Передайте входной параметр [`Binder binder`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.Host/Bindings/Runtime/Binder.cs) или [`IBinder binder`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/IBinder.cs).
- Используйте следующий шаблон C# для привязки данных,

  ```cs
  using (var output = await binder.BindAsync<T>(new BindingTypeAttribute(...)))
  {
      ...
  }
  ```

  где `BindingTypeAttribute` — атрибут .NET, определяющий пользовательскую привязку, а `T` — входной или выходной тип, поддерживаемый этим типом привязки. `T` не может быть параметром типа `out` (например, `out JObject`). Например, выходная привязка таблицы мобильных приложений поддерживает [шесть выходных типов](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.MobileApps/MobileTableAttribute.cs#L17-L22), но можно использовать только [\<ICollector t >](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/ICollector.cs) или [IAsyncCollector\<t >](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/IAsyncCollector.cs) с императивной привязкой.

### <a name="single-attribute-example"></a>Пример с одним атрибутом

В следующем примере кода создается [выходная привязка большого двоичного объекта службы хранилища](functions-bindings-storage-blob.md#output) с путем к большому двоичному объекту, определенному во время выполнения, а затем записывается строка в большой двоичный объект.

```cs
public static class IBinderExample
{
    [FunctionName("CreateBlobUsingBinder")]
    public static void Run(
        [QueueTrigger("myqueue-items-source-4")] string myQueueItem,
        IBinder binder,
        ILogger log)
    {
        log.LogInformation($"CreateBlobUsingBinder function processed: {myQueueItem}");
        using (var writer = binder.Bind<TextWriter>(new BlobAttribute(
                    $"samples-output/{myQueueItem}", FileAccess.Write)))
        {
            writer.Write("Hello World!");
        };
    }
}
```

[BlobAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/BlobAttribute.cs) определяет входную или выходную привязку [большого двоичного объекта службы хранилища](functions-bindings-storage-blob.md), а [TextWriter](/dotnet/api/system.io.textwriter) представляет собой поддерживаемый тип выходной привязки.

### <a name="multiple-attribute-example"></a>Пример с несколькими атрибутами

В предыдущем примере код получает параметр приложения для строки подключения основной учетной записи хранения приложения-функции (т. е. `AzureWebJobsStorage`). Вы можете указать пользовательский параметр приложения, который следует использовать для учетной записи хранения. Для этого добавьте [StorageAccountAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/StorageAccountAttribute.cs) и передайте массив атрибутов в `BindAsync<T>()`. Используйте параметр `Binder`, а не `IBinder`.  Пример:

```cs
public static class IBinderExampleMultipleAttributes
{
    [FunctionName("CreateBlobInDifferentStorageAccount")]
    public async static Task RunAsync(
            [QueueTrigger("myqueue-items-source-binder2")] string myQueueItem,
            Binder binder,
            ILogger log)
    {
        log.LogInformation($"CreateBlobInDifferentStorageAccount function processed: {myQueueItem}");
        var attributes = new Attribute[]
        {
        new BlobAttribute($"samples-output/{myQueueItem}", FileAccess.Write),
        new StorageAccountAttribute("MyStorageAccount")
        };
        using (var writer = await binder.BindAsync<TextWriter>(attributes))
        {
            await writer.WriteAsync("Hello World!!");
        }
    }
}
```

## <a name="triggers-and-bindings"></a>Триггеры и привязки 

[!INCLUDE [Supported triggers and bindings](../../includes/functions-bindings.md)]

## <a name="next-steps"></a>Следующие шаги

> [!div class="nextstepaction"]
> [Дополнительные сведения о триггерах и привязках](functions-triggers-bindings.md)

> [!div class="nextstepaction"]
> [Дополнительные сведения о рекомендациях по решению "Функции Azure"](functions-best-practices.md)
