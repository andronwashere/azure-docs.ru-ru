---
title: Application Insights Azure для ASP.NET Core приложений | Документация Майкрософт
description: Отслеживайте доступность, производительность и использование веб-приложений ASP.NET Core.
services: application-insights
documentationcenter: .net
author: mrbullwinkle
manager: carmonm
ms.assetid: 3b722e47-38bd-4667-9ba4-65b7006c074c
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 05/22/2019
ms.author: mbullwin
ms.openlocfilehash: 7e0143a25c0bb25b936d072cc2652e8b38a0be66
ms.sourcegitcommit: af58483a9c574a10edc546f2737939a93af87b73
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/17/2019
ms.locfileid: "68302717"
---
# <a name="application-insights-for-aspnet-core-applications"></a>Application Insights для ASP.NET Core приложений

В этой статье описано, как включить Application Insights для [ASP.NET Core](https://docs.microsoft.com/aspnet/core) приложения. После выполнения инструкций, описанных в этой статье, Application Insights соберет запросы, зависимости, исключения, счетчики производительности, пульсы и журналы из приложения ASP.NET Core. 

В качестве примера мы будем использовать [приложение MVC](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app) , предназначенное `netcoreapp2.2`для. Эти инструкции можно применить ко всем ASP.NET Coreным приложениям.

## <a name="supported-scenarios"></a>Поддерживаемые сценарии использования.

[Пакет SDK Application Insights для ASP.NET Core](https://nuget.org/packages/Microsoft.ApplicationInsights.AspNetCore) может отслеживать приложения независимо от того, где или как они выполняются. Если приложение работает и имеет сетевое подключение к Azure, можно собирать данные телеметрии. Мониторинг Application Insights поддерживается везде, где поддерживается .NET Core. Поддержка:
* **Операционная система**. Windows, Linux или Mac.
* **Метод размещения**: В процессе или вне процесса. 
* **Метод развертывания**: Зависимая от платформы или автономная.
* **Веб-сервер**: IIS (Internet Information Server) или Kestrel. 
* **Платформа размещения**: Функция веб-приложений службы приложений Azure, виртуальная машина Azure, Docker, служба Azure Kubernetes Service (AKS) и т. д.
* **ИНТЕГРИРОВАННАЯ СРЕДА РАЗРАБОТКИ**: Visual Studio, VS Code или Командная строка.

## <a name="prerequisites"></a>предварительные требования

- Работающее приложение ASP.NET Core. Если необходимо создать ASP.NET Core приложение, следуйте указаниям в этом [ASP.NET Coreном руководстве](https://docs.microsoft.com/aspnet/core/getting-started/).
- Допустимый ключ инструментирования Application Insights. Этот ключ необходим для отправки любых данных телеметрии в Application Insights. Если необходимо создать новый Application Insights ресурс для получения ключа инструментирования, см. раздел [Создание ресурса Application Insights](https://docs.microsoft.com/azure/azure-monitor/app/create-new-resource).

## <a name="enable-application-insights-server-side-telemetry-visual-studio"></a>Включение телеметрии на стороне сервера Application Insights (Visual Studio)

1. Откройте проект в Visual Studio.

    > [!TIP]
    > При необходимости можно настроить систему управления версиями для проекта, чтобы можно было отвестися от всех изменений, которые Application Insights делает. Чтобы включить систему управления версиями, выберите **файл** > **Добавить в систему управления версиями**.

2. Выберите **Проект** > **Добавить телеметрию Application Insights**.

3. Выберите **Начать**. Текст этого варианта может отличаться в зависимости от используемой версии Visual Studio. В некоторых более ранних версиях вместо нее используется кнопка **запустить бесплатно** .

4. Выберите свою подписку. Затем выберите пункт**регистр** **ресурсов** > .

5. После добавления Application Insights в проект убедитесь, что вы используете последний стабильный выпуск пакета SDK. Перейдите в **проект** > **Управление пакетами** > NuGet**Microsoft. ApplicationInsights. AspNetCore**. При необходимости выберите **Обновить**.

     ![Снимок экрана, на котором показано, где выбрать пакет Application Insights для обновления](./media/asp-net-core/update-nuget-package.png)

6. Если вы применяете необязательную подсказку и добавили проект в систему управления  > версиями, перейдите к просмотру**Team Explorer** > **изменений**. Затем выберите каждый файл, чтобы просмотреть различие изменений, внесенных Application Insights телеметрии.

## <a name="enable-application-insights-server-side-telemetry-no-visual-studio"></a>Включить телеметрию на стороне сервера Application Insights (без Visual Studio)

1. Установите [пакет NuGet для Application Insights SDK для ASP.NET Core](https://nuget.org/packages/Microsoft.ApplicationInsights.AspNetCore). Рекомендуется всегда использовать последнюю стабильную версию. Ознакомьтесь с полными сведениями о выпуске пакета SDK в [репозитории GitHub с открытым исходным кодом](https://github.com/Microsoft/ApplicationInsights-aspnetcore/releases).

    В следующем примере кода показаны изменения, которые необходимо добавить в `.csproj` файл проекта.

    ```xml
        <ItemGroup>
          <PackageReference Include="Microsoft.ApplicationInsights.AspNetCore" Version="2.7.0" />
        </ItemGroup>
    ```

2. Добавьте `services.AddApplicationInsightsTelemetry();`в методв`Startup` классе, как показано в следующем примере: `ConfigureServices()`

    ```csharp
        // This method gets called by the runtime. Use this method to add services to the container.
        public void ConfigureServices(IServiceCollection services)
        {
            // The following line enables Application Insights telemetry collection.
            services.AddApplicationInsightsTelemetry();
    
            // This code adds other services for your application.
            services.AddMvc();
        }
    ```

3. Настройте ключ инструментирования.

    Хотя ключ инструментирования можно указать в качестве аргумента `AddApplicationInsightsTelemetry`, рекомендуется указать ключ инструментирования в конфигурации. В следующем образце кода показано, как указать ключ инструментирования в `appsettings.json`. Убедитесь, `appsettings.json` что во время публикации скопировано в корневую папку приложения.

    ```json
        {
          "ApplicationInsights": {
            "InstrumentationKey": "putinstrumentationkeyhere"
          },
          "Logging": {
            "LogLevel": {
              "Default": "Warning"
            }
          }
        }
    ```

    Кроме того, можно указать ключ инструментирования в любой из следующих переменных среды:

    * `APPINSIGHTS_INSTRUMENTATIONKEY`

    * `ApplicationInsights:InstrumentationKey`

    Пример:

    * `SET ApplicationInsights:InstrumentationKey=putinstrumentationkeyhere`

    * `SET APPINSIGHTS_INSTRUMENTATIONKEY=putinstrumentationkeyhere`

    Как правило `APPINSIGHTS_INSTRUMENTATIONKEY` , указывает ключ инструментирования для приложений, развернутых в веб-приложениях.

    > [!NOTE]
    > Ключ инструментирования, указанный в коде, передается через `APPINSIGHTS_INSTRUMENTATIONKEY`переменную среды, которая WINS поверх других параметров.

## <a name="run-your-application"></a>Запуск приложения

Запустите приложение и выполните запросы к нему. Теперь данные телеметрии должны передаваться в Application Insights. Пакет SDK для Application Insights автоматически собирает следующие данные телеметрии.

|Запросы и зависимости |Сведения|
|---------------|-------|
|Requests | Входящие веб-запросы к приложению. |
|HTTP или HTTPS | Вызовы метода выполняются `HttpClient`с помощью. |
|SQL | Вызовы метода выполняются `SqlClient`с помощью. |
|[Хранилище Azure](https://www.nuget.org/packages/WindowsAzure.Storage/) | Вызовы, выполняемые с помощью клиента службы хранилища Azure. |
|[Пакет SDK для клиента EventHubs](https://www.nuget.org/packages/Microsoft.Azure.EventHubs) | Версия 1.1.0 и более поздние версии. |
|[Пакет SDK для клиента ServiceBus](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus)| Версия 3.0.0 и более поздние версии. |
|Azure Cosmos DB | Отслеживание автоматически, только если используется HTTP/HTTPS. Application Insights не захватывает режим TCP. |

### <a name="performance-counters"></a>Счетчики производительности

Поддержка [счетчиков производительности](https://azure.microsoft.com/documentation/articles/app-insights-web-monitor-performance/) в ASP.NET Core ограничена:

   * Версии пакета SDK 2.4.1 и более поздних версий собираются счетчики производительности, если приложение выполняется в веб-приложениях (Windows).
   * Версии пакета SDK 2.7.0-beta3 и более поздних версий собираются счетчики производительности, если приложение `NETSTANDARD2.0` выполняется в Windows и целевые или более поздние версии.
   * Для приложений, предназначенных для .NET Framework, все версии пакетов SDK поддерживают счетчики производительности.
 
Эта статья будет обновлена при добавлении поддержки счетчика производительности в Linux.

### <a name="ilogger-logs"></a>Журналы ILogger

[Журналы ILogger](https://docs.microsoft.com/azure/azure-monitor/app/ilogger) с уровнем серьезности `Warning` или выше автоматически фиксируются в пакетах SDK 2.7.0-beta3 и более поздних версий.

### <a name="live-metrics"></a>Динамические метрики

На портале может потребоваться несколько минут, прежде чем начнется отображение телеметрических данных. Чтобы быстро убедиться в том, что все работает, лучше использовать [динамические метрики](https://docs.microsoft.com/azure/application-insights/app-insights-live-stream) при выполнении запросов к выполняющемуся приложению.

## <a name="enable-client-side-telemetry-for-web-applications"></a>Включение телеметрии на стороне клиента для веб-приложений

Предыдущие шаги достаточно, чтобы помочь начать сбор данных телеметрии на стороне сервера. Если приложение имеет клиентские компоненты, выполните следующие действия, чтобы начать сбор данных [телеметрии использования](https://docs.microsoft.com/azure/azure-monitor/app/usage-overview).

1. В `_ViewImports.cshtml`добавьте внедрение:

    ```cshtml
        @inject Microsoft.ApplicationInsights.AspNetCore.JavaScriptSnippet JavaScriptSnippet
    ```

2. В `_Layout.cshtml`вставьте `HtmlHelper` в конец`<head>` раздела, но перед любым другим сценарием. Если вы хотите сообщить о любых пользовательских данных телеметрии JavaScript со страницы, вставьте ее после этого фрагмента кода:

    ```cshtml
        @Html.Raw(JavaScriptSnippet.FullScript)
        </head>
    ```

Ранее упомянутые имена файлов относятся к шаблону приложения MVC по умолчанию. `.cshtml` В конечном счете, если вы хотите правильно включить наблюдение на стороне клиента для вашего приложения, фрагмент JavaScript должен отображаться в `<head>` разделе каждой страницы приложения, которое необходимо отслеживать. Эту цель можно выполнить для этого шаблона приложения, добавив фрагмент кода JavaScript в `_Layout.cshtml`. 

Если проект не включен `_Layout.cshtml`, можно по-прежнему добавить [наблюдение на стороне клиента](https://docs.microsoft.com/azure/azure-monitor/app/website-monitoring). Это можно сделать, добавив фрагмент кода JavaScript в эквивалентный файл, который управляет `<head>` всеми страницами в приложении. Кроме того, можно добавить фрагмент кода на несколько страниц, но это решение сложно поддерживать, и в большинстве случаев это не рекомендуется.

## <a name="configure-the-application-insights-sdk"></a>Настройка пакета SDK для Application Insights

Можно настроить Application Insights SDK для ASP.NET Core, чтобы изменить конфигурацию по умолчанию. Пользователи Application Insights ASP.NET SDK могут быть знакомы с изменением конфигурации с помощью `ApplicationInsights.config` или путем изменения. `TelemetryConfiguration.Active` Конфигурация изменяется иначе для ASP.NET Core. Добавьте пакет SDK для ASP.NET Core в приложение и настройте его с помощью ASP.NET Core встроенного [внедрения зависимостей](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection). Сделайте почти все изменения конфигурации в `ConfigureServices()` методе `Startup.cs` класса, пока не будет направлено иное. Дополнительные сведения см. в следующих разделах.

> [!NOTE]
> В ASP.NET Coreных приложениях изменение конфигурации путем изменения `TelemetryConfiguration.Active` не поддерживается.

### <a name="using-applicationinsightsserviceoptions"></a>Использование Аппликатионинсигхтссервицеоптионс

Можно изменить несколько общих параметров, передав `ApplicationInsightsServiceOptions` `AddApplicationInsightsTelemetry`, как показано в следующем примере:

```csharp
    public void ConfigureServices(IServiceCollection services)
    {
        Microsoft.ApplicationInsights.AspNetCore.Extensions.ApplicationInsightsServiceOptions aiOptions
                    = new Microsoft.ApplicationInsights.AspNetCore.Extensions.ApplicationInsightsServiceOptions();
        // Disables adaptive sampling.
        aiOptions.EnableAdaptiveSampling = false;

        // Disables QuickPulse (Live Metrics stream).
        aiOptions.EnableQuickPulseMetricStream = false;
        services.AddApplicationInsightsTelemetry(aiOptions);
    }
```

Дополнительные сведения см. в разделе [Настраиваемые параметры в `ApplicationInsightsServiceOptions` ](https://github.com/microsoft/ApplicationInsights-aspnetcore/blob/develop/src/Microsoft.ApplicationInsights.AspNetCore/Extensions/ApplicationInsightsServiceOptions.cs).

### <a name="sampling"></a>Выборка

Пакет SDK Application Insights для ASP.NET Core поддерживает как фиксированную, так и адаптивную выборку. Адаптивная выборка включена по умолчанию. 

Дополнительные сведения см. в статье [Настройка адаптивной выборки для приложений ASP.NET Core](../../azure-monitor/app/sampling.md#configuring-adaptive-sampling-for-aspnet-core-applications).

### <a name="adding-telemetryinitializers"></a>Добавление Telemetryinitializer

Используйте [инициализаторы телеметрии](https://docs.microsoft.com/azure/azure-monitor/app/api-filtering-sampling#add-properties-itelemetryinitializer) , если необходимо определить глобальные свойства, которые отправляются со всеми данными телеметрии.

Добавьте новые `TelemetryInitializer` `DependencyInjection` в контейнер, как показано в следующем коде. Пакет SDK автоматически берет все `TelemetryInitializer` , что добавляется `DependencyInjection` в контейнер.

```csharp
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddSingleton<ITelemetryInitializer, MyCustomTelemetryInitializer>();
    }
```

### <a name="removing-telemetryinitializers"></a>Удаление Telemetryinitializer

Инициализаторы телеметрии представлены по умолчанию. Чтобы удалить все или конкретные инициализаторы телеметрии, используйте следующий пример кода *после* вызова метода `AddApplicationInsightsTelemetry()`.

```csharp
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddApplicationInsightsTelemetry();

        // Remove a specific built-in telemetry initializer
        var tiToRemove = services.FirstOrDefault<ServiceDescriptor>
                         (t => t.ImplementationType == typeof(AspNetCoreEnvironmentTelemetryInitializer));
        if (tiToRemove != null)
        {
            services.Remove(tiToRemove);
        }

        // Remove all initializers
        // This requires importing namespace by using Microsoft.Extensions.DependencyInjection.Extensions;
        services.RemoveAll(typeof(ITelemetryInitializer));
    }
```

### <a name="adding-telemetry-processors"></a>Добавление обработчиков данных телеметрии

Можно добавить пользовательские обработчики данных телеметрии `TelemetryConfiguration` в с помощью метода `AddApplicationInsightsTelemetryProcessor` расширения в `IServiceCollection`. Вы используете обработчики данных телеметрии в [сценариях расширенной фильтрации](https://docs.microsoft.com/azure/azure-monitor/app/api-filtering-sampling#filtering-itelemetryprocessor) , чтобы обеспечить более прямое управление включенными или исключенными из данных телеметрии, отправляемых в службу Application Insights. Используйте следующий пример.

```csharp
    public void ConfigureServices(IServiceCollection services)
    {
        // ...
        services.AddApplicationInsightsTelemetry();
        services.AddApplicationInsightsTelemetryProcessor<MyFirstCustomTelemetryProcessor>();

        // If you have more processors:
        services.AddApplicationInsightsTelemetryProcessor<MySecondCustomTelemetryProcessor>();
    }
```

### <a name="configuring-or-removing-default-telemetrymodules"></a>Настройка или удаление Телеметримодулес по умолчанию

Application Insights использует модули телеметрии для [автоматического сбора полезной информации](https://docs.microsoft.com/azure/azure-monitor/app/auto-collect-dependencies) о конкретных рабочих нагрузках без необходимости дополнительной настройки.

Следующие модули автоматической коллекции включены по умолчанию. Эти модули отвечают за автоматическое сбор данных телеметрии. Их можно отключить или настроить для изменения их поведения по умолчанию.

* `RequestTrackingTelemetryModule`
* `DependencyTrackingTelemetryModule`
* `PerformanceCollectorModule`
* `QuickPulseTelemetryModule`
* `AppServicesHeartbeatTelemetryModule`
* `AzureInstanceMetadataTelemetryModule`

Чтобы настроить любое значение `TelemetryModule`по умолчанию, используйте `ConfigureTelemetryModule<T>` метод `IServiceCollection`расширения для, как показано в следующем примере.

```csharp
using Microsoft.ApplicationInsights.DependencyCollector;
using Microsoft.ApplicationInsights.Extensibility.PerfCounterCollector;

    public void ConfigureServices(IServiceCollection services)
    {
        services.AddApplicationInsightsTelemetry();

        // The following configures DependencyTrackingTelemetryModule.
        // Similarly, any other default modules can be configured.
        services.ConfigureTelemetryModule<DependencyTrackingTelemetryModule>((module, o) =>
                        {
                            module.EnableW3CHeadersInjection = true;
                        });

        // The following removes PerformanceCollectorModule to disable perf-counter collection.
        // Similarly, any other default modules can be removed.
        var performanceCounterService = services.FirstOrDefault<ServiceDescriptor>(t => t.ImplementationType == typeof(PerformanceCollectorModule));
        if (performanceCounterService != null)
        {
         services.Remove(performanceCounterService);
        }
    }
```

### <a name="configuring-a-telemetry-channel"></a>Настройка канала телеметрии

Каналом по умолчанию является `ServerTelemetryChannel`. Его можно переопределить, как показано в следующем примере.

```csharp
using Microsoft.ApplicationInsights.Channel;

    public void ConfigureServices(IServiceCollection services)
    {
        // Use the following to replace the default channel with InMemoryChannel.
        // This can also be applied to ServerTelemetryChannel.
        services.AddSingleton(typeof(ITelemetryChannel), new InMemoryChannel() {MaxTelemetryBufferCapacity = 19898 });

        services.AddApplicationInsightsTelemetry();
    }
```

### <a name="disable-telemetry-dynamically"></a>Динамическое отключение телеметрии

Если вы хотите отключить данные телеметрии условно и динамически, вы можете разрешить `TelemetryConfiguration` экземпляр с контейнером ASP.NET Core внедрения зависимостей в любом месте кода и установить `DisableTelemetry` для него флаг.

```csharp
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddApplicationInsightsTelemetry();
    }

    public void Configure(IApplicationBuilder app, IHostingEnvironment env, TelemetryConfiguration configuration)
    {
        configuration.DisableTelemetry = true;
        ...
    }
```

## <a name="frequently-asked-questions"></a>Часто задаваемые вопросы

### <a name="how-can-i-track-telemetry-thats-not-automatically-collected"></a>Как можно отвестить данные телеметрии, которые не собираются автоматически?

Получите экземпляр `TelemetryClient` с помощью внедрения конструктора и вызовите для него необходимый `TrackXXX()` метод. Не рекомендуется создавать новые `TelemetryClient` экземпляры в приложении ASP.NET Core. Одноэлементный экземпляр `TelemetryClient` уже зарегистрирован `DependencyInjection` в контейнере, который совместно `TelemetryConfiguration` используется с остальными данными телеметрии. Создание нового `TelemetryClient` экземпляра рекомендуется, только если требуется конфигурация, отделяющая остальные данные телеметрии. 

В следующем примере показано, как отслеживанию дополнительных данных телеметрии из контроллера.

```csharp
using Microsoft.ApplicationInsights;

public class HomeController : Controller
{
    private TelemetryClient telemetry;

    // Use constructor injection to get a TelemetryClient instance.
    public HomeController(TelemetryClient telemetry)
    {
        this.telemetry = telemetry;
    }

    public IActionResult Index()
    {
        // Call the required TrackXXX method.
        this.telemetry.TrackEvent("HomePageRequested");
        return View();
    }
```

Дополнительные сведения о настраиваемых отчетах данных в Application Insights см. в разделе [Справочник по API настраиваемых метрик Application Insights](https://docs.microsoft.com/azure/azure-monitor/app/api-custom-events-metrics/).

### <a name="some-visual-studio-templates-used-the-useapplicationinsights-extension-method-on-iwebhostbuilder-to-enable-application-insights-is-this-usage-still-valid"></a>Некоторые шаблоны Visual Studio использовали метод расширения UseApplicationInsights () в Ивебхостбуилдер для включения Application Insights. Является ли это использование допустимым?

Да, включение Application Insights с этим методом является допустимым. Этот метод используется в адаптации Visual Studio и в расширениях веб-приложений. Однако рекомендуется использовать `services.AddApplicationInsightsTelemetry()` , так как он предоставляет перегрузки для управления некоторой конфигурацией. Оба метода выполняют одни и те же функции, поэтому, если не нужно применять пользовательскую конфигурацию, можно вызвать любой из этих методов.

### <a name="im-deploying-my-aspnet-core-application-to-web-apps-should-i-still-enable-the-application-insights-extension-from-web-apps"></a>Я развертываю приложение ASP.NET Core в веб-приложениях. Следует ли по-прежнему включать расширение Application Insights из веб-приложений?

Если пакет SDK устанавливается во время сборки, как показано в этой статье, вам не нужно включать [расширение Application Insights](https://docs.microsoft.com/azure/azure-monitor/app/azure-web-apps) на портале службы приложений. Даже если расширение установлено, оно будет восстановлено при обнаружении того, что пакет SDK уже добавлен в приложение. Если включить Application Insights из расширения, вам не нужно устанавливать и обновлять пакет SDK. Но если вы включили Application Insights, следуя инструкциям в этой статье, вы получите большую гибкость, поскольку:

   * Данные телеметрии Application Insights будут продолжать работать в:
       * Все операционные системы, включая Windows, Linux и Mac.
       * Все режимы публикации, включая автономные или зависящие от платформы.
       * Все целевые платформы, включая полный .NET Framework.
       * Все варианты размещения, включая веб-приложения, виртуальные машины, Linux, контейнеры, службу Azure Kubernetes и размещение без Azure.
   * Вы можете просматривать данные телеметрии локально при отладке из Visual Studio.
   * Вы можете отвести отслеживание дополнительных пользовательских данных телеметрии `TrackXXX()` с помощью API.
   * Вы полностью контролируете конфигурацию.

### <a name="can-i-enable-application-insights-monitoring-by-using-tools-like-status-monitor"></a>Можно ли включить Application Insights мониторинг с помощью таких средств, как монитор состояния?

Нет. В настоящее время [Монитор состояния](https://docs.microsoft.com/azure/azure-monitor/app/monitor-performance-live-website-now) и [Монитор состояния v2](https://docs.microsoft.com/azure/azure-monitor/app/status-monitor-v2-overview) поддерживают только ASP.NET 4. x.

### <a name="is-application-insights-automatically-enabled-for-my-aspnet-core-20-application"></a>Включено ли Application Insights для приложения ASP.NET Core 2,0 автоматически?

`Microsoft.AspNetCore.All` 2,0 метапакет включает пакет SDK для Application Insights (версия 2.1.0). При запуске приложения в отладчике Visual Studio Visual Studio включает Application Insights и отображает данные телеметрии локально в самой интегрированной среде разработки. Данные телеметрии не были отправлены в службу Application Insights, если не был указан ключ инструментирования. Мы рекомендуем использовать инструкции, приведенные в этой статье, чтобы включить Application Insights, даже для приложений 2,0.

### <a name="if-i-run-my-application-in-linux-are-all-features-supported"></a>Если я запускаю приложение в Linux, поддерживаются ли все функции?

Да. Поддержка функций для пакета SDK одинакова на всех платформах, за исключением следующих:

* Счетчики производительности поддерживаются только в Windows.
* `ServerTelemetryChannel` Несмотря на то что включен по умолчанию, если приложение выполняется в Linux или MacOS, канал не создает папку локального хранилища, чтобы временно сохранять данные телеметрии в случае проблем с сетью. Из-за этого ограничения данные телеметрии теряются при наличии временных проблем с сетью или сервером. Чтобы обойти эту ошибку, настройте локальную папку для канала:

```csharp
using Microsoft.ApplicationInsights.Channel;
using Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel;

    public void ConfigureServices(IServiceCollection services)
    {
        // The following will configure the channel to use the given folder to temporarily
        // store telemetry items during network or Application Insights server issues.
        // User should ensure that the given folder already exists
        // and that the application has read/write permissions.
        services.AddSingleton(typeof(ITelemetryChannel),
                                new ServerTelemetryChannel () {StorageFolder = "/tmp/myfolder"});
        services.AddApplicationInsightsTelemetry();
    }
```

## <a name="open-source-sdk"></a>Пакет SDK с открытым исходным кодом

[Чтение кода и дополнительные наработки](https://github.com/Microsoft/ApplicationInsights-aspnetcore#recent-updates).

## <a name="video"></a>Видео

- Ознакомьтесь с этим внешним пошаговым видео, чтобы [настроить Application Insights с .NET Core и Visual Studio](https://www.youtube.com/watch?v=NoS9UhcR4gA&t) с нуля.
- Ознакомьтесь с этим внешним пошаговым видео, чтобы [настроить Application Insights с .NET Core и Visual Studio Code](https://youtu.be/ygGt84GDync) с нуля.

## <a name="next-steps"></a>Следующие шаги

* [Изучите потоки пользователей](../../azure-monitor/app/usage-flows.md) , чтобы понять, как пользователи переходят через приложение.
* [Настройте сбор моментальных снимков](https://docs.microsoft.com/azure/application-insights/app-insights-snapshot-debugger) для просмотра состояния исходного кода и переменных в момент возникновения исключения.
* [Используйте API](../../azure-monitor/app/api-custom-events-metrics.md) для отправки собственных событий и метрик для получения подробного представления о производительности и использовании приложения.
* Используйте [тесты доступности](../../azure-monitor/app/monitor-web-app-availability.md), чтобы постоянно проверять работу приложения из всех точек мира.
* [Внедрение зависимостей в ASP.NET Core](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection)
