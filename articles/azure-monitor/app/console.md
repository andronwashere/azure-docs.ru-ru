---
title: Azure Application Insights для консольных приложений | Документация Майкрософт
description: Отслеживайте доступность, производительность и использование веб-приложений.
services: application-insights
documentationcenter: .net
author: mrbullwinkle
manager: carmonm
ms.assetid: 3b722e47-38bd-4667-9ba4-65b7006c074c
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 01/30/2019
ms.reviewer: lmolkova
ms.author: mbullwin
ms.openlocfilehash: 0c2a28462633d47ad1d3f247793e3fcf6f4d40c0
ms.sourcegitcommit: 66237bcd9b08359a6cce8d671f846b0c93ee6a82
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/11/2019
ms.locfileid: "67795448"
---
# <a name="application-insights-for-net-console-applications"></a>Application Insights для консольных приложений .NET
[Application Insights](../../azure-monitor/app/app-insights-overview.md) позволяет отслеживать доступность, производительность и использование вашего веб-приложения.

Вам потребуется подписка [Microsoft Azure](https://azure.com). Войдите с помощью учетной записи Майкрософт, которую вы, возможно, уже используете для Windows, XBox Live или других облачных служб Майкрософт. Возможно, у вашей группы есть подписка организации Azure: попросите ее владельца добавить вас к ней с помощью вашей учетной записи Майкрософт.

## <a name="getting-started"></a>Начало работы

* На [портале Azure](https://portal.azure.com) [создайте ресурс Application Insights](../../azure-monitor/app/create-new-resource.md). Для параметра типа приложения выберите приложение **Общий**.
* Сделайте копию ключа инструментирования. Найдите ключ в раскрывающемся списке **Основные компоненты** нового ресурса, который вы создали. 
* Установите последнюю версию пакета [Microsoft.ApplicationInsights](https://www.nuget.org/packages/Microsoft.ApplicationInsights).
* Прежде чем отслеживать телеметрию, задайте ключ инструментирования в коде или переменную среды APPINSIGHTS_INSTRUMENTATIONKEY. После этого вы сможете отслеживать телеметрию вручную и просматривать данные телеметрии на портале Azure.

```csharp
// you may use different options to create configuration as shown later in this article
TelemetryConfiguration configuration = TelemetryConfiguration.CreateDefault();
configuration.InstrumentationKey = " *your key* ";
var telemetryClient = new TelemetryClient(configuration);
telemetryClient.TrackTrace("Hello World!");
```

* Установите последнюю версию пакета [Microsoft.ApplicationInsights.DependencyCollector](https://www.nuget.org/packages/Microsoft.ApplicationInsights.DependencyCollector), который позволяет автоматически отслеживать HTTP-вызовы, SQL-вызовы или другие внешние вызовы зависимостей.

Вы можете инициализировать и настроить Application Insights с помощью кода или файла `ApplicationInsights.config`. Выполните инициализацию как можно раньше. 

> [!NOTE]
> Инструкции для **ApplicationInsights.config** применимы только к приложениям, которые нацелены на платформу .NET Framework, и не подходят для платформы .NET Core.

### <a name="using-config-file"></a>Использование файла конфигурации
По умолчанию пакет SDK для Application Insights ищет файл `ApplicationInsights.config` в рабочей папке при создании `TelemetryConfiguration`.

```csharp
TelemetryConfiguration config = TelemetryConfiguration.Active; // Reads ApplicationInsights.config file if present
```

Вы также можете указать путь к файлу конфигурации.

```csharp
using System.IO;
TelemetryConfiguration configuration = TelemetryConfiguration.CreateFromConfiguration(File.ReadAllText("C:\\ApplicationInsights.config"));
var telemetryClient = new TelemetryClient(configuration);
```

Дополнительные сведения см. в [справочных материалах по файлу конфигурации](configuration-with-applicationinsights-config.md).

Чтобы получить полный пример файла конфигурации, установите последнюю версию пакета [Microsoft.ApplicationInsights.WindowsServer](https://www.nuget.org/packages/Microsoft.ApplicationInsights.WindowsServer). В этом руководстве приводится **минимальная** конфигурация для коллекции зависимостей, как в примере кода ниже.

```XML
<?xml version="1.0" encoding="utf-8"?>
<ApplicationInsights xmlns="http://schemas.microsoft.com/ApplicationInsights/2013/Settings">
  <InstrumentationKey>Your Key</InstrumentationKey>
  <TelemetryInitializers>
    <Add Type="Microsoft.ApplicationInsights.DependencyCollector.HttpDependenciesParsingTelemetryInitializer, Microsoft.AI.DependencyCollector"/>
  </TelemetryInitializers>
  <TelemetryModules>
    <Add Type="Microsoft.ApplicationInsights.DependencyCollector.DependencyTrackingTelemetryModule, Microsoft.AI.DependencyCollector">
      <ExcludeComponentCorrelationHttpHeadersOnDomains>
        <Add>core.windows.net</Add>
        <Add>core.chinacloudapi.cn</Add>
        <Add>core.cloudapi.de</Add>
        <Add>core.usgovcloudapi.net</Add>
        <Add>localhost</Add>
        <Add>127.0.0.1</Add>
      </ExcludeComponentCorrelationHttpHeadersOnDomains>
      <IncludeDiagnosticSourceActivities>
        <Add>Microsoft.Azure.ServiceBus</Add>
        <Add>Microsoft.Azure.EventHubs</Add>
      </IncludeDiagnosticSourceActivities>
    </Add>
  </TelemetryModules>
  <TelemetryChannel Type="Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel.ServerTelemetryChannel, Microsoft.AI.ServerTelemetryChannel"/>
</ApplicationInsights>

```

### <a name="configuring-telemetry-collection-from-code"></a>Настройка сбора данных телеметрии с помощью кода
> [!NOTE]
> При чтении файла конфигурации не поддерживается в .NET Core. Можно использовать [пакет SDK Application Insights для ASP.NET Core](../../azure-monitor/app/asp-net-core.md)

* При запуске приложения создайте и настройте экземпляр `DependencyTrackingTelemetryModule`. Он должен быть одноэлементным и сохраняться в течение всего времени существования приложения.

```csharp
var module = new DependencyTrackingTelemetryModule();

// prevent Correlation Id to be sent to certain endpoints. You may add other domains as needed.
module.ExcludeComponentCorrelationHttpHeadersOnDomains.Add("core.windows.net");
//...

// enable known dependency tracking, note that in future versions, we will extend this list. 
// please check default settings in https://github.com/Microsoft/ApplicationInsights-dotnet-server/blob/develop/Src/DependencyCollector/DependencyCollector/ApplicationInsights.config.install.xdt

module.IncludeDiagnosticSourceActivities.Add("Microsoft.Azure.ServiceBus");
module.IncludeDiagnosticSourceActivities.Add("Microsoft.Azure.EventHubs");
//....

// initialize the module
module.Initialize(configuration);
```

* Добавьте общие инициализаторы телеметрии.

```csharp
// ensures proper DependencyTelemetry.Type is set for Azure RESTful API calls
configuration.TelemetryInitializers.Add(new HttpDependenciesParsingTelemetryInitializer());
```

Если вы создали конфигурации с помощью обычного `TelemetryConfiguration()` конструктор, необходимо включить поддержку корреляции, кроме того. **Он не нужен** при чтении из файла конфигурации, используемый `TelemetryConfiguration.CreateDefault()` или `TelemetryConfiguration.Active`.

```csharp
configuration.TelemetryInitializers.Add(new OperationCorrelationTelemetryInitializer());
```

* Можно также установить и инициализировать модуль сборщика для счетчика производительности, как описано [здесь](https://apmtips.com/blog/2017/02/13/enable-application-insights-live-metrics-from-code/)


#### <a name="full-example"></a>Полный пример

```csharp
using Microsoft.ApplicationInsights;
using Microsoft.ApplicationInsights.DependencyCollector;
using Microsoft.ApplicationInsights.Extensibility;
using System.Net.Http;
using System.Threading.Tasks;

namespace ConsoleApp
{
    class Program
    {
        static void Main(string[] args)
        {
            TelemetryConfiguration configuration = TelemetryConfiguration.CreateDefault();

            configuration.InstrumentationKey = "removed";
            configuration.TelemetryInitializers.Add(new HttpDependenciesParsingTelemetryInitializer());

            var telemetryClient = new TelemetryClient();
            using (InitializeDependencyTracking(configuration))
            {
                // run app...

                telemetryClient.TrackTrace("Hello World!");

                using (var httpClient = new HttpClient())
                {
                    // Http dependency is automatically tracked!
                    httpClient.GetAsync("https://microsoft.com").Wait();
                }

            }

            // before exit, flush the remaining data
            telemetryClient.Flush();

            // flush is not blocking so wait a bit
            Task.Delay(5000).Wait();

        }

        static DependencyTrackingTelemetryModule InitializeDependencyTracking(TelemetryConfiguration configuration)
        {
            var module = new DependencyTrackingTelemetryModule();

            // prevent Correlation Id to be sent to certain endpoints. You may add other domains as needed.
            module.ExcludeComponentCorrelationHttpHeadersOnDomains.Add("core.windows.net");
            module.ExcludeComponentCorrelationHttpHeadersOnDomains.Add("core.chinacloudapi.cn");
            module.ExcludeComponentCorrelationHttpHeadersOnDomains.Add("core.cloudapi.de");
            module.ExcludeComponentCorrelationHttpHeadersOnDomains.Add("core.usgovcloudapi.net");
            module.ExcludeComponentCorrelationHttpHeadersOnDomains.Add("localhost");
            module.ExcludeComponentCorrelationHttpHeadersOnDomains.Add("127.0.0.1");

            // enable known dependency tracking, note that in future versions, we will extend this list. 
            // please check default settings in https://github.com/Microsoft/ApplicationInsights-dotnet-server/blob/develop/Src/DependencyCollector/DependencyCollector/ApplicationInsights.config.install.xdt

            module.IncludeDiagnosticSourceActivities.Add("Microsoft.Azure.ServiceBus");
            module.IncludeDiagnosticSourceActivities.Add("Microsoft.Azure.EventHubs");

            // initialize the module
            module.Initialize(configuration);

            return module;
        }
    }
}

```

## <a name="next-steps"></a>Следующие шаги
* [Отслеживайте зависимости](../../azure-monitor/app/asp-net-dependencies.md), чтобы выяснить, что стало причиной медленной работы — REST, SQL или другие внешние ресурсы.
* [Используйте API](../../azure-monitor/app/api-custom-events-metrics.md) для отправки собственных событий и метрик для более четкого представления о производительности и использовании приложения.
