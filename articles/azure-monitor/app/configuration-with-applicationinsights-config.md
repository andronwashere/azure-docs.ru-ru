---
title: Справочник по файлу ApplicationInsights.config в Azure | Документация Майкрософт
description: Включение или отключение модулей сбора данных и добавление счетчиков производительности, а также других параметров.
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.assetid: 6e397752-c086-46e9-8648-a1196e8078c2
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 05/22/2019
ms.reviewer: olegan
ms.author: mbullwin
ms.openlocfilehash: 382f43156ab450600ff0d2e5e2db763cd6bd94df
ms.sourcegitcommit: de47a27defce58b10ef998e8991a2294175d2098
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/15/2019
ms.locfileid: "67875053"
---
# <a name="configuring-the-application-insights-sdk-with-applicationinsightsconfig-or-xml"></a>Настройка пакета SDK для Application Insights с использованием файла ApplicationInsights.config или ApplicationInsights.xml
Пакет SDK .NET Application Insights состоит из нескольких пакетов NuGet. [Основной пакет](https://www.nuget.org/packages/Microsoft.ApplicationInsights) предоставляет API для отправки телеметрии в Application Insights. [Дополнительные пакеты](https://www.nuget.org/packages?q=Microsoft.ApplicationInsights) предоставляют *модули* и *инициализаторы* телеметрии для автоматического отслеживания телеметрии вашего приложения и его контекста. Настроив файл конфигурации, можно включить или отключить модули телеметрии и инициализаторы, а также задать параметры для некоторых из них.

Имя файла конфигурации – `ApplicationInsights.config` или `ApplicationInsights.xml` в зависимости от типа приложения. Он автоматически добавляется в проект при [установке большинства версий пакета SDK][start]. Он также добавляется в веб-приложение с [Монитор состояния на сервере IIS][redfield]. Файл конфигурации не учитывается, если используется [расширение для веб-сайта](azure-web-apps.md) или [расширения Azure для виртуальной машины Azure и масштабируемого набора виртуальных машин](azure-vm-vmss-apps.md) .

Нет эквивалентного файла для управления пакетом [SDK на веб-странице][client].

В этом документе описываются разделы файла конфигурации и то, как они управляют компонентами пакета SDK. Кроме того, в нем содержатся сведения о пакетах NuGet, загружающих эти компоненты.

> [!NOTE]
> ApplicationInsights.config и XML-файлы инструкций не применяются к пакету SDK для .NET Core. Для настройки приложений .NET Core следуйте [этому](../../azure-monitor/app/asp-net-core.md) руководству.

## <a name="telemetry-modules-aspnet"></a>Модули телеметрии (ASP.NET)
Каждый модуль телеметрии собирает данные определенного типа и использует основной API для отправки данных. Модули устанавливаются разными пакетами NuGet, что также добавляет необходимые строки в CONFIG-файл.

В файле конфигурации есть узел для каждого модуля. Чтобы удалить модуль, удалите узел или закомментируйте его.

### <a name="dependency-tracking"></a>Отслеживание зависимостей
[Dependency tracking](../../azure-monitor/app/asp-net-dependencies.md) собирает данные телеметрии о вызовах, выполняемых приложением к базам данных и внешним службам. Чтобы обеспечить работу этого модуля на сервере IIS, необходимо [установить монитор состояния][redfield].

Можно также написать код отслеживания зависимостей, используя [API TrackDependency](../../azure-monitor/app/api-custom-events-metrics.md#trackdependency).

* `Microsoft.ApplicationInsights.DependencyCollector.DependencyTrackingTelemetryModule`
* [Microsoft.ApplicationInsights.DependencyCollector](https://www.nuget.org/packages/Microsoft.ApplicationInsights.DependencyCollector) .

Зависимости можно собирать в автоматическом виде, не изменяя код с помощью присоединения на основе агента (без кода). Чтобы использовать его в веб-приложениях Azure, включите [расширение Application Insights](azure-web-apps.md). Чтобы использовать его в ВИРТУАЛЬНОЙ машине Azure или в масштабируемом наборе виртуальных машин Azure, включите [расширение мониторинга приложений для виртуальных машин и масштабируемых наборов виртуальных машин](azure-vm-vmss-apps.md).

### <a name="performance-collector"></a>Сборщик данных производительности
[Собирает данные счетчиков производительности системы](../../azure-monitor/app/performance-counters.md), например ЦП, памяти и сетевой нагрузки из установок IIS. Можно указать конкретные счетчики для сбора данных, включая счетчики производительности, настроенные самостоятельно.

* `Microsoft.ApplicationInsights.Extensibility.PerfCounterCollector.PerformanceCollectorModule`
* [Microsoft.ApplicationInsights.PerfCounterCollector](https://www.nuget.org/packages/Microsoft.ApplicationInsights.PerfCounterCollector) .

### <a name="application-insights-diagnostics-telemetry"></a>Телеметрия диагностики Application Insights
`DiagnosticsTelemetryModule` информирует об ошибках в самом коде инструментирования Application Insights, например, если код не может получить доступ к счетчикам производительности или `ITelemetryInitializer` вызывает исключение. Данные телеметрии трассировки, отслеживаемые этим модулем, отображаются в [поиске][diagnostic]по диагностике.

```
* `Microsoft.ApplicationInsights.Extensibility.Implementation.Tracing.DiagnosticsTelemetryModule`
* [Microsoft.ApplicationInsights](https://www.nuget.org/packages/Microsoft.ApplicationInsights) NuGet package. If you only install this package, the ApplicationInsights.config file is not automatically created.
```

### <a name="developer-mode"></a>Режим разработчика
`DeveloperModeWithDebuggerAttachedTelemetryModule` заставляет Application Insights `TelemetryChannel` отправлять данные незамедлительно (по одному элементу телеметрии за один раз), как только к процессу приложения присоединяется отладчик. Это сокращает период времени между моментом получения данных телеметрии вашим приложением и моментом их отображения на портале Application Insights. Это вызывает значительную нагрузку на процессор и пропускную способность сети.

* `Microsoft.ApplicationInsights.WindowsServer.DeveloperModeWithDebuggerAttachedTelemetryModule`
* [Application Insights Windows Server](https://www.nuget.org/packages/Microsoft.ApplicationInsights.WindowsServer/)

### <a name="web-request-tracking"></a>Отслеживание веб-запросов
Передает данные о [времени отклика и коде результата](../../azure-monitor/app/asp-net.md) HTTP-запросов.

* `Microsoft.ApplicationInsights.Web.RequestTrackingTelemetryModule`
* [Microsoft.ApplicationInsights.Web](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Web)

### <a name="exception-tracking"></a>Отслеживание исключений
`ExceptionTrackingTelemetryModule` отслеживает количество необработанных исключений в вашем веб-приложении. См. раздел [сбои и исключения][exceptions].

* `Microsoft.ApplicationInsights.Web.ExceptionTrackingTelemetryModule`
* [Microsoft.ApplicationInsights.Web](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Web)
* `Microsoft.ApplicationInsights.WindowsServer.UnobservedExceptionTelemetryModule` отслеживает [незамеченные исключения задач](https://blogs.msdn.com/b/pfxteam/archive/2011/09/28/task-exception-handling-in-net-4-5.aspx).
* `Microsoft.ApplicationInsights.WindowsServer.UnhandledExceptionTelemetryModule` отслеживает необработанные исключения для рабочих ролей, служб Windows и консольных приложений.
* [Application Insights Windows Server](https://www.nuget.org/packages/Microsoft.ApplicationInsights.WindowsServer/) .

### <a name="eventsource-tracking"></a>Отслеживание EventSource
`EventSourceTelemetryModule` позволяет настроить отправку событий EventSource в Application Insights в качестве трассировок. Сведения об отслеживании событий EventSource см. в разделе [Использование событий EventSource](../../azure-monitor/app/asp-net-trace-logs.md#use-eventsource-events).

* `Microsoft.ApplicationInsights.EventSourceListener.EventSourceTelemetryModule`
* [Microsoft.ApplicationInsights.EventSourceListener](https://www.nuget.org/packages/Microsoft.ApplicationInsights.EventSourceListener) 

### <a name="etw-event-tracking"></a>Трассировка событий Windows
`EtwCollectorTelemetryModule` позволяет настроить отправку событий от поставщиков трассировки событий Windows в Application Insights в качестве трассировок. Сведения об отслеживании событий трассировки событий Windows см. в разделе [Использование событий трассировки событий Windows](../../azure-monitor/app/asp-net-trace-logs.md#use-etw-events).

* `Microsoft.ApplicationInsights.EtwCollector.EtwCollectorTelemetryModule`
* [Microsoft.ApplicationInsights.EtwCollector](https://www.nuget.org/packages/Microsoft.ApplicationInsights.EtwCollector) 

### <a name="microsoftapplicationinsights"></a>Microsoft.ApplicationInsights
Пакет Microsoft.ApplicationInsights предоставляет [основной API](https://msdn.microsoft.com/library/mt420197.aspx) пакета SDK. Другие модули телеметрии используют это, и его также можно [использовать для определения собственных данных телеметрии](../../azure-monitor/app/api-custom-events-metrics.md).

* Нет записей в файле ApplicationInsights.config.
* [Microsoft.ApplicationInsights](https://www.nuget.org/packages/Microsoft.ApplicationInsights) . Если просто установить этот пакет NuGet, CONFIG-файл не создается.

## <a name="telemetry-channel"></a>Канал телеметрии
[Канал телеметрии](telemetry-channels.md) управляет буферизацией и передачей данных телеметрии в службу Application Insights.

* `Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel.ServerTelemetryChannel`— Это канал по умолчанию для веб-приложений. Он помещает данные в память и использует механизмы повтора и хранилище локального диска для более надежной доставки телеметрии.
* `Microsoft.ApplicationInsights.InMemoryChannel`— Это упрощенный канал телеметрии, который используется, если другие каналы не настроены. 

## <a name="telemetry-initializers-aspnet"></a>Инициализаторы телеметрии (ASP.NET)
Инициализаторы телеметрии устанавливают свойства контекста, которые отправляются вместе с каждым элементом телеметрии.

Вы можете [написать собственные инициализаторы](../../azure-monitor/app/api-filtering-sampling.md#add-properties) для задания свойств контекста.

Все стандартные инициализаторы задаются пакетами NuGet Web или WindowsServer.

* `AccountIdTelemetryInitializer` задает свойство AccountId.
* `AuthenticatedUserIdTelemetryInitializer` задает свойство AuthenticatedUserId, как задано пакетом SDK JavaScript.
* `AzureRoleEnvironmentTelemetryInitializer` обновляет свойства `RoleName` и `RoleInstance` контекста `Device` для всех элементов телеметрии с учетом данных, извлеченных из среды выполнения Azure.
* `BuildInfoConfigComponentVersionTelemetryInitializer` обновляет свойство `Version` контекста `Component` для всех элементов телеметрии с учетом значения, извлеченного из файла `BuildInfo.config`, созданного платформой MSBuild.
* `ClientIpHeaderTelemetryInitializer` обновляет свойство `Ip` контекста `Location` всех элементов телеметрии в соответствии с HTTP-заголовком запроса `X-Forwarded-For`.
* `DeviceTelemetryInitializer` обновляет следующие свойства контекста `Device` для всех элементов телеметрии.
  * `Type` получает значение «PC» (Компьютер).
  * `Id` получает значение, соответствующее доменному имени компьютера, на котором выполняется веб-приложение.
  * `OemName` получает значение, извлеченное из поля `Win32_ComputerSystem.Manufacturer` с помощью WMI.
  * `Model` получает значение, извлеченное из поля `Win32_ComputerSystem.Model` с помощью WMI.
  * `NetworkType` получает значение, извлеченное из `NetworkInterface`.
  * `Language` получает значение, соответствующее имени `CurrentCulture`.
* `DomainNameRoleInstanceTelemetryInitializer` обновляет свойства `RoleInstance` контекста `Device` для всех элементов телеметрии с учетом доменного имени компьютера, на котором выполняется веб-приложение.
* `OperationNameTelemetryInitializer`обновляет свойство `Name` контекста `RequestTelemetry` и свойство `Name` контекста `Operation` всех элементов телеметрии в соответствии с методом HTTP, а также именами контроллера MVC ASP.NET и действия, вызываемого для обработки запроса.
* `OperationIdTelemetryInitializer` или `OperationCorrelationTelemetryInitializer` обновляет свойство контекста `Operation.Id` всех элементов телеметрии, отслеживаемых при обработке запроса с автоматически созданным `RequestTelemetry.Id`.
* `SessionTelemetryInitializer` обновляет свойство `Id` контекста `Session` для всех элементов телеметрии со значениями, извлеченными из файла cookie `ai_session`, созданного кодом JavaScript инструментирования Application Insights, который выполняется в браузере пользователя.
* `SyntheticTelemetryInitializer` или `SyntheticUserAgentTelemetryInitializer` обновляет свойства контекстов `User`, `Session` и `Operation` всех элементов телеметрии, отслеживаемых при обработке запроса от искусственного источника, например теста доступности или программы-робота поисковой системы. По умолчанию [обозреватель метрик](../../azure-monitor/app/metrics-explorer.md) не отображает данные телеметрии искусственных источников.

    `<Filters>` задает идентификационные свойства запросов.
* `UserTelemetryInitializer` обновляет свойства `Id` и `AcquisitionDate` контекста `User` для всех элементов телеметрии со значениями, извлеченными из файла cookie `ai_user`, созданного кодом JavaScript инструментирования Application Insights, который выполняется в браузере пользователя.
* `WebTestTelemetryInitializer`Задает идентификатор пользователя, идентификатор сеанса и свойства искусственного источника для HTTP-запросов, поступающих из [тестов доступности](../../azure-monitor/app/monitor-web-app-availability.md).
  `<Filters>` задает идентификационные свойства запросов.

Для приложений .NET, работающих в Service Fabric, можно добавить пакет NuGet `Microsoft.ApplicationInsights.ServiceFabric`. Данный пакет содержит компонент `FabricTelemetryInitializer`, который добавляет свойства Service Fabric в элементы телеметрии. Дополнительные сведения см. на [странице GitHub](https://github.com/Microsoft/ApplicationInsights-ServiceFabric/blob/master/README.md), посвященной свойствам, добавляемым пакетом NuGet.

## <a name="telemetry-processors-aspnet"></a>Обработчики данных телеметрии (ASP.NET)
Обработчики данных телеметрии могут фильтровать и изменять каждый элемент телеметрии непосредственно перед его отправкой из пакета SDK на портал.

Вы можете [создавать собственные обработчики данных телеметрии](../../azure-monitor/app/api-filtering-sampling.md#filtering).

#### <a name="adaptive-sampling-telemetry-processor-from-200-beta3"></a>Обработчик данных телеметрии адаптивной выборки (из 2.0.0 — beta3)
Эта функция включена по умолчанию. Если приложение отправляет слишком много телеметрических данных, обработчик удаляет часть из них.

```xml

    <TelemetryProcessors>
      <Add Type="Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel.AdaptiveSamplingTelemetryProcessor, Microsoft.AI.ServerTelemetryChannel">
        <MaxTelemetryItemsPerSecond>5</MaxTelemetryItemsPerSecond>
      </Add>
    </TelemetryProcessors>

```

С помощью параметра можно задать целевой показатель, которого алгоритм будет пытаться достичь. Каждый экземпляр пакета SDK работает независимо от других, поэтому если сервер представляет собой кластер из нескольких машин, фактический объем телеметрических данных будет кратен их количеству.

[Дополнительная информация о выборке](../../azure-monitor/app/sampling.md).

#### <a name="fixed-rate-sampling-telemetry-processor-from-200-beta1"></a>Обработчик данных телеметрии с фиксированной частотой (из 2.0.0-beta1)
Кроме того, существует стандартный [обработчик данных телеметрии](../../azure-monitor/app/api-filtering-sampling.md) с выборкой (от 2.0.1):

```XML

    <TelemetryProcessors>
     <Add Type="Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel.SamplingTelemetryProcessor, Microsoft.AI.ServerTelemetryChannel">

     <!-- Set a percentage close to 100/N where N is an integer. -->
     <!-- E.g. 50 (=100/2), 33.33 (=100/3), 25 (=100/4), 20, 1 (=100/100), 0.1 (=100/1000) -->
     <SamplingPercentage>10</SamplingPercentage>
     </Add>
   </TelemetryProcessors>

```



## <a name="channel-parameters-java"></a>Параметры канала (Java)
Эти параметры влияют на то, как в пакетах SDK для Java хранятся и удаляются собранные ими данные телеметрии.

#### <a name="maxtelemetrybuffercapacity"></a>MaxTelemetryBufferCapacity
Этот параметр определяет количество элементов телеметрии, которые могут храниться в хранилище в памяти пакета SDK. По достижении этого количества происходит очистка буфера телеметрии, то есть элементы телеметрии отправляются на сервер Application Insights.

* Минимум: 1
* Максимум: 1000
* По умолчанию: 500

```

  <ApplicationInsights>
      ...
      <Channel>
       <MaxTelemetryBufferCapacity>100</MaxTelemetryBufferCapacity>
      </Channel>
      ...
  </ApplicationInsights>
```

#### <a name="flushintervalinseconds"></a>FlushIntervalInSeconds
Этот параметр определяет, как часто следует очищать (отправлять в Application Insights) данные, хранящиеся в хранилище в памяти.

* Минимум: 1
* Максимум: 300
* По умолчанию: 5

```

    <ApplicationInsights>
      ...
      <Channel>
        <FlushIntervalInSeconds>100</FlushIntervalInSeconds>
      </Channel>
      ...
    </ApplicationInsights>
```

#### <a name="maxtransmissionstoragecapacityinmb"></a>MaxTransmissionStorageCapacityInMB
Этот параметр определяет максимальный размер пространства (в МБ), выделенного для постоянного хранилища на локальном диске. Это хранилище используется для хранения элементов телеметрии, которые не удалось передать в конечную точку Application Insights. Если хранилище будет заполнено, новые элементы телеметрии будут отклонены.

* Минимум: 1
* Максимум: 100
* По умолчанию: 10

```

   <ApplicationInsights>
      ...
      <Channel>
        <MaxTransmissionStorageCapacityInMB>50</MaxTransmissionStorageCapacityInMB>
      </Channel>
      ...
   </ApplicationInsights>
```

#### <a name="local-forwarder"></a>Локальный сервер переадресации

[Локальный сервер пересылки](opencensus-local-forwarder.md) — это агент, который собирает данные телеметрии Application Insights или [OpenCensus](https://opencensus.io/) из разных платформ и пакетов SDK и направляет их в Application Insights. Он может работать под управлением Windows и Linux. В сочетании с Java SDK Application Insights локальный экспедитор обеспечивает полную поддержку [​​Live Metrics](../../azure-monitor/app/live-stream.md) и адаптивной выборки.

```xml
<Channel type="com.microsoft.applicationinsights.channel.concrete.localforwarder.LocalForwarderTelemetryChannel">
<EndpointAddress><!-- put the hostname:port of your LocalForwarder instance here --></EndpointAddress>

<!-- The properties below are optional. The values shown are the defaults for each property -->

<FlushIntervalInSeconds>5</FlushIntervalInSeconds><!-- must be between [1, 500]. values outside the bound will be rounded to nearest bound -->
<MaxTelemetryBufferCapacity>500</MaxTelemetryBufferCapacity><!-- units=number of telemetry items; must be between [1, 1000] -->
</Channel>
```

Если вы используете инициализатор SpringBoot, добавьте следующее в файл конфигурации (application.properties):

```yml
azure.application-insights.channel.local-forwarder.endpoint-address=<!--put the hostname:port of your LocalForwarder instance here-->
azure.application-insights.channel.local-forwarder.flush-interval-in-seconds=<!--optional-->
azure.application-insights.channel.local-forwarder.max-telemetry-buffer-capacity=<!--optional-->
```

Значения по умолчанию одинаковы для конфигурации SpringBoot application.properties и applicationinsights.xml.

## <a name="instrumentationkey"></a>InstrumentationKey
Он определяет ресурс Application Insights, в котором отображаются данные. Обычно создается отдельный ресурс с отдельным ключом инструментирования для каждого приложения.

Если необходимо задать ключ динамически, например если вам нужно отправлять результаты из приложения в различные ресурсы, можно удалить ключ в файле конфигурации и задать его в коде.

Чтобы задать ключ для всех экземпляров TelemetryClient, включая стандартные модули телеметрии, установите ключ в Телеметриконфигуратион. Active. Задайте ключ в методе инициализации, таком как global.aspx.cs, в службе ASP.NET:

```csharp

    protected void Application_Start()
    {
      Microsoft.ApplicationInsights.Extensibility.
        TelemetryConfiguration.Active.InstrumentationKey =
          // - for example -
          WebConfigurationManager.AppSettings["ikey"];
      //...
```

Если нужно только отправлять определенный набор событий в другой ресурс, можно задать ключ для конкретного TelemetryClient:

```csharp

    var tc = new TelemetryClient();
    tc.Context.InstrumentationKey = "----- my key ----";
    tc.TrackEvent("myEvent");
    // ...

```

Чтобы получить новый ключ, [Создайте новый ресурс на портале Application Insights][new].



## <a name="applicationid-provider"></a>Поставщик идентификаторов приложений

_Доступно начиная с версии 2.6.0_.

Этот поставщик предназначен для поиска идентификатора приложения по ключу инструментирования. Идентификатор приложения включен в RequestTelemetry и DependencyTelemetry и используется для определения корреляции на портале.

Чтобы использовать его, установите `TelemetryConfiguration.ApplicationIdProvider` в коде или в конфигурации приложения.

### <a name="interface-iapplicationidprovider"></a>Интерфейс: IApplicationIdProvider

```csharp
public interface IApplicationIdProvider
{
    bool TryGetApplicationId(string instrumentationKey, out string applicationId);
}
```


В пакете SDK [Microsoft.ApplicationInsights](https://www.nuget.org/packages/Microsoft.ApplicationInsights) мы предоставляем две его реализации: `ApplicationInsightsApplicationIdProvider` и `DictionaryApplicationIdProvider`.

### <a name="applicationinsightsapplicationidprovider"></a>ApplicationInsightsApplicationIdProvider

Этот интерфейс является оболочкой API-интерфейса Profile. Он регулирует запросы и кэширует их результаты.

Этот поставщик добавляется в файл конфигурации после установки [Microsoft.ApplicationInsights.DependencyCollector](https://www.nuget.org/packages/Microsoft.ApplicationInsights.DependencyCollector) или [Microsoft.ApplicationInsights.Web](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Web/).

Этот класс содержит необязательное свойство `ProfileQueryEndpoint`.
По умолчанию оно имеет значение `https://dc.services.visualstudio.com/api/profiles/{0}/appId`.
Если для этой конфигурации вам нужен прокси-сервер, мы рекомендуем настроить на нем базовый адрес и добавить /api/profiles/{0}/appId. Обратите внимание, что во время выполнения заполнитель {0} заменяется ключом инструментирования для каждого конкретного запроса.

#### <a name="example-configuration-via-applicationinsightsconfig"></a>Пример настройки в ApplicationInsights.config:
```xml
<ApplicationInsights>
    ...
    <ApplicationIdProvider Type="Microsoft.ApplicationInsights.Extensibility.Implementation.ApplicationId.ApplicationInsightsApplicationIdProvider, Microsoft.ApplicationInsights">
        <ProfileQueryEndpoint>https://dc.services.visualstudio.com/api/profiles/{0}/appId</ProfileQueryEndpoint>
    </ApplicationIdProvider>
    ...
</ApplicationInsights>
```

#### <a name="example-configuration-via-code"></a>Пример настройки напрямую в коде:
```csharp
TelemetryConfiguration.Active.ApplicationIdProvider = new ApplicationInsightsApplicationIdProvider();
```

### <a name="dictionaryapplicationidprovider"></a>DictionaryApplicationIdProvider

Это статический поставщик, который использует настроенные пары ключей инструментирования и идентификаторов приложений.

Этот класс имеет свойство `Defined`, в котором в формате Dictionary<строка,строка> сохраняются пары ключей инструментирования и идентификаторов приложений.

Этот класс содержит необязательное свойство `Next`, с помощью которого вы можете настроить другой поставщик, который будет использоваться при запросе тех ключей инструментирования, которых не существует в вашей конфигурации.

#### <a name="example-configuration-via-applicationinsightsconfig"></a>Пример настройки в ApplicationInsights.config:
```xml
<ApplicationInsights>
    ...
    <ApplicationIdProvider Type="Microsoft.ApplicationInsights.Extensibility.Implementation.ApplicationId.DictionaryApplicationIdProvider, Microsoft.ApplicationInsights">
        <Defined>
            <Type key="InstrumentationKey_1" value="ApplicationId_1"/>
            <Type key="InstrumentationKey_2" value="ApplicationId_2"/>
        </Defined>
        <Next Type="Microsoft.ApplicationInsights.Extensibility.Implementation.ApplicationId.ApplicationInsightsApplicationIdProvider, Microsoft.ApplicationInsights" />
    </ApplicationIdProvider>
    ...
</ApplicationInsights>
```

#### <a name="example-configuration-via-code"></a>Пример настройки напрямую в коде:
```csharp
TelemetryConfiguration.Active.ApplicationIdProvider = new DictionaryApplicationIdProvider{
 Defined = new Dictionary<string, string>
    {
        {"InstrumentationKey_1", "ApplicationId_1"},
        {"InstrumentationKey_2", "ApplicationId_2"}
    }
};
```




## <a name="next-steps"></a>Следующие шаги
Дополнительные [сведения об API][api].

<!--Link references-->

[api]: ../../azure-monitor/app/api-custom-events-metrics.md
[client]: ../../azure-monitor/app/javascript.md
[diagnostic]: ../../azure-monitor/app/diagnostic-search.md
[exceptions]: ../../azure-monitor/app/asp-net-exceptions.md
[netlogs]: ../../azure-monitor/app/asp-net-trace-logs.md
[new]: ../../azure-monitor/app/create-new-resource.md 
[redfield]: ../../azure-monitor/app/monitor-performance-live-website-now.md
[start]: ../../azure-monitor/app/app-insights-overview.md
