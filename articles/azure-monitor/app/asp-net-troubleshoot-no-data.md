---
title: Устранение неполадок, связанных с тем, что в Application Insights для .NET не отображаются данные
description: Не отображаются данные в Azure Application Insights? Попробуйте здесь.
services: application-insights
documentationcenter: .net
author: mrbullwinkle
manager: carmonm
ms.assetid: e231569f-1b38-48f8-a744-6329f41d91d3
ms.service: application-insights
ms.workload: mobile
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 07/23/2018
ms.author: mbullwin
ms.openlocfilehash: 2966f90dcb381e439c00a6540ef9a01bd24f8743
ms.sourcegitcommit: d3b1f89edceb9bff1870f562bc2c2fd52636fc21
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/04/2019
ms.locfileid: "67561186"
---
# <a name="troubleshooting-no-data---application-insights-for-net"></a>Устранение неполадок, связанных с тем, что в Application Insights для .NET не отображаются данные
## <a name="some-of-my-telemetry-is-missing"></a>Некоторая телеметрия отсутствует
*В Application Insights отображается только часть событий, создаваемых приложением.*

* Если постоянно отображается одна и та же часть, это может быть связано с адаптивной [выборкой](../../azure-monitor/app/sampling.md). Чтобы проверить это, откройте область поиска (из колонки "Обзор") и посмотрите на экземпляр запроса или другого события. В нижней части раздела свойств нажмите кнопку "...", чтобы отобразить все сведения свойств. Если число запросов > 1, то выборка работает.
* В противном случае, возможно, превышено [ограничение на скорость передачи данных](../../azure-monitor/app/pricing.md#limits-summary) для вашего плана ценообразования. Эти ограничения применяются в пропорционально по минутам.

*У меня возникают потери данных случайным образом.*

* Проверьте, если вы столкнулись с потерей данных на [канал телеметрии](telemetry-channels.md#does-the-application-insights-channel-guarantee-telemetry-delivery-if-not-what-are-the-scenarios-in-which-telemetry-can-be-lost)

* Поиск известных проблемах в канал телеметрии [репозиторий Github](https://github.com/Microsoft/ApplicationInsights-dotnet/issues)

*У меня возникают потери данных в консольном приложении или на веб-приложения, когда приложение собирается остановить.*

* Пакет SDK для канала сохраняет данные телеметрии в буфер и отправляет их в пакеты. Если приложение завершает работу, может потребоваться явно вызывать [Flush()](api-custom-events-metrics.md#flushing-data). Поведение `Flush()` зависит от фактического [канал](telemetry-channels.md#built-in-telemetry-channels) используется.

## <a name="no-data-from-my-server"></a>Нет данных с моего сервера
*На моем веб-сервере установлено приложение, но данные телеметрии сервера не отображаются. На компьютере разработки все работало хорошо.*

* Возможно, проблема в брандмауэре. [Установите исключения брандмауэра для отправки данных Application Insights](../../azure-monitor/app/ip-addresses.md).
* На сервере IIS могут отсутствовать некоторые предварительные условия: расширяемость .NET 4.5 и ASP.NET 4.5.

*На моем веб-сервере [установлен монитор состояния](../../azure-monitor/app/monitor-performance-live-website-now.md) для мониторинга существующих приложений. Я не вижу никаких результатов.*

* См. раздел [Устранение неполадок](../../azure-monitor/app/monitor-performance-live-website-now.md#troubleshoot).

## <a name="q01"></a>Параметр "Добавление Application Insights" в Visual Studio не отображается
*Когда я щелкаю существующий проект в обозревателе решений правой кнопкой мыши, параметры Application Insights не отображаются.*

* Средства Application Insights поддерживают не все типы проектов .NET. Проекты WCF и веб-проекты поддерживаются. В проекты других типов, например в приложения-службы и классические приложения, можно [вручную добавить пакет SDK службы Application Insights](../../azure-monitor/app/windows-desktop.md).
* Убедитесь в том, что установлена версия [Visual Studio 2013 с обновлением 3 или более поздним](https://docs.microsoft.com/visualstudio/releasenotes/vs2013-update3-rtm-vs). Решение поставляется предварительно установленным с аналитическими средствами для разработчиков в рамках пакета Application Insights SDK.
* Выберите элементы **Сервис**, **Расширения и обновления** и убедитесь, что пакет **Аналитические средства для разработчиков** установлен и включен. Если это так, щелкните пункт **Обновления** , чтобы проверить, доступно ли обновление.
* Откройте диалоговое окно "Новый проект" и выберите "Веб-приложение ASP.NET". Если параметр Application Insights отображается, соответствующие средства установлены. Если нет, попробуйте удалить и повторно установить Developer Analytics Tools.

## <a name="q02"></a>Сбой при добавлении Application Insights
*При попытке добавить Application Insights в существующий проект я вижу сообщение об ошибке.*

Вероятные причины:

* Сбой связи с порталом Application Insights.
* В вашей учетной записи Azure возникли неполадки.
* У вас есть только [доступ на чтение в подписке или группе, в которой вы пытаетесь создать новый ресурс](../../azure-monitor/app/resources-roles-access-control.md).

Исправление:

* Убедитесь, что вы указали правильные учетные данные для входа в учетную запись Azure.
* В браузере убедитесь, что у вас есть доступ к [порталу Azure](https://portal.azure.com). Откройте параметры и проверьте, нет ли каких-либо ограничений.
* [Добавьте Application Insights в существующий проект](../../azure-monitor/app/asp-net.md). В обозревателе решений щелкните проект правой кнопкой мыши и выберите пункт "Добавить Application Insights".

## <a name="emptykey"></a>Появляется сообщение об ошибке «ключ инструментирования не может быть пустым»
Вероятно, произошла ошибка при установке Application Insights или же адаптера ведения журнала.

В обозревателе решений щелкните проект правой кнопкой мыши и выберите **Application Insights > Настроить Application Insights**. Появится диалоговое окно с предложением войти в Azure и создать новый ресурс Application Insights или повторно использовать уже существующий.

## <a name="NuGetBuild"></a> На сервере сборки возникает ошибка NuGet package(s) are missing (Пакеты NuGet отсутствуют).
*Когда я выполняю отладку на компьютере для разработки, сборка выполняется без ошибок, но на сервере сборки отображается ошибка NuGet.*

См. сведения о [восстановлении пакетов NuGet](https://docs.nuget.org/Consume/Package-Restore) и [автоматическом восстановлении пакетов](https://docs.nuget.org/Consume/package-restore/migrating-to-automatic-package-restore).

## <a name="missing-menu-command-to-open-application-insights-from-visual-studio"></a>Отсутствует команда меню для открытия Application Insights из Visual Studio
*Когда я щелкаю правой кнопкой мыши свой проект в обозревателе решений, не отображаются команды Application Insights или команда "Открыть Application Insights".*

Вероятные причины:

* Ресурс Application Insights создан вручную, либо тип проекта не поддерживается средствами Application Insights.
* Аналитические средства для разработчиков отключены в Visual Studio.
* У вас установлена более старая версия Visual Studio, чем версия 2013 с обновлением 3.

Исправление:

* Обновите Visual Studio до версии 2013 с обновлением 3 или более поздней.
* Выберите элементы **Сервис**, **Расширения и обновления** и убедитесь, что пакет **Аналитические средства для разработчиков** установлен и включен. Если это так, щелкните пункт **Обновления** , чтобы проверить, доступно ли обновление.
* В обозревателе решений щелкните проект правой кнопкой мыши. Если вы видите команду **Application Insights > Настроить Application Insights**, используйте ее, чтобы подключить проект к ресурсу в службе Application Insights.

В противном случае тип проекта не поддерживается напрямую аналитические средства для разработчиков. Для просмотра данных телеметрии войдите на [портал Azure](https://portal.azure.com), выберите пункт Application Insights на панели навигации слева и выберите приложение.

## <a name="access-denied-on-opening-application-insights-from-visual-studio"></a>При открытии Application Insights из Visual Studio возникает ошибка "Доступ запрещен"
*При выборе команды меню "Открыть Application Insights" выполняется переход на портал Azure, но при этом появляется сообщение об ошибке "Доступ запрещен".*

При вводе учетных данных Майкрософт, которые вы в последний раз использовали в стандартном браузере, отсутствует доступ к [ресурсу, который был создан при добавлении Application Insights в это приложение](../../azure-monitor/app/asp-net.md). Есть две вероятные причины:

* Возможно, у вас не одна учетная запись Майкрософт, а несколько — например, рабочая и личная. В последний раз вы использовали в браузере по умолчанию не ту учетную запись, с помощью которой [добавляли Application Insights в проект](../../azure-monitor/app/asp-net.md).
  * Исправление: щелкните свое имя в верхней правой части окна браузера и выполните выход. Затем войдите с учетной записью, необходимой для доступа. Затем на панели навигации слева щелкните Application Insights и выберите приложение.
* Кто-то другой добавил Application Insights в проект и забыл предоставить вам [доступ к группе ресурсов](../../azure-monitor/app/resources-roles-access-control.md) , в которой он был создан.
  * Исправление: если этот пользователь указал учетную запись организации, возможно, он добавил вас в группу или предоставил вам индивидуальный доступ к группе ресурсов.

## <a name="asset-not-found-on-opening-application-insights-from-visual-studio"></a>При открытии Application Insights из Visual Studio возникает ошибка "Ресурс-контейнер не найден"
*При выборе команды меню "Открыть Application Insights" выполняется переход на портал Azure, но при этом появляется сообщение об ошибке "Ресурс-контейнер не найден".*

Вероятные причины:

* Ресурс Application Insights для приложения был удален.
* Ключ инструментирования был задан или изменен непосредственно в файле ApplicationInsights.config, при этом файл проекта не был обновлен.

Ключ инструментирования в файле ApplicationInsights.config контролирует, куда отправляются данные телеметрии. Строка в файле проекта контролирует, какой ресурс открывается при использовании команды в Visual Studio.

Исправление:

* В обозревателе решений щелкните проект правой кнопкой мыши и выберите пункт Application Insights, а затем — "Настроить Application Insights". В открывшемся диалоговом окне можно отправить данные телеметрии в существующий ресурс или создать новый. Или сделайте так:
* Откройте ресурс напрямую. Войдите на [портал Azure](https://portal.azure.com), выберите пункт Application Insights на панели навигации слева и выберите приложение.

## <a name="where-do-i-find-my-telemetry"></a>Где найти мои данные телеметрии
*После входа на [портал Microsoft Azure](https://portal.azure.com) я вижу панель мониторинга на домашней странице Azure. Итак, где найти мои данные Application Insights?*

* На панели навигации слева щелкните Application Insights и выберите имя приложения. Если проекты не отображаются, [добавьте или настройте Application Insights в своем веб-проекте](../../azure-monitor/app/asp-net.md).  
  Вы увидите несколько сводных диаграмм. Щелкните любую из них, чтобы увидеть более подробные данные.
* В Visual Studio во время отладки приложения нажмите кнопку Application Insights.

## <a name="q03"></a> Не отображаются данные сервера (или вообще какие-либо данные)
*После запуска приложения и открытия службы Application Insights в Microsoft Azure на всех диаграммах отображаются сообщения "Сведения о сборе…" или "Не настроено".* Или отображаются *только представление страницы и пользовательские данные, но нет данных сервера.*

* Запустите приложение в режиме отладки в Visual Studio (F5). Используйте приложение таким образом, чтобы создать данные телеметрии. Проверьте, можете ли вы просматривать события, зарегистрированные в окне вывода Visual Studio.  
  ![](./media/asp-net-troubleshoot-no-data/output-window.png)
* На портале Application Insights откройте колонку [Diagnostic Search](../../azure-monitor/app/diagnostic-search.md)(Поиск по журналу диагностики). Как правило, сначала данные отображаются здесь.
* Нажмите кнопку «Обновить». Колонка периодически обновляется, однако ее можно обновить и вручную. При значительных значениях временного диапазона интервал обновления увеличивается.
* Проверьте соответствие ключей инструментирования. В основной колонке приложения на портале Application Insights в раскрывающемся списке **Основные компоненты** найдите **ключ инструментирования**. Затем в Visual Studio в своем проекте откройте файл ApplicationInsights.config и найдите `<instrumentationkey>`. Ключи должны совпадать. В противном случае выполните одно из таких действий:  
  * На портале щелкните Application Insights и найдите ресурс приложения с правильным ключом.
  * В обозревателе решений в Visual Studio щелкните проект правой кнопкой мыши и выберите пункт Application Insights, а затем щелкните "Настроить". Сбросьте настройки приложения, чтобы данные телеметрии отправлялись в нужный ресурс.
  * Если не удается найти совпадающие ключи, убедитесь, что для входа в Visual Studio и на портал используются одни и те же учетные данные.
* На [панели мониторинга на домашней странице Microsoft Azure](https://portal.azure.com)посмотрите на карту работоспособности службы. Если есть какие-либо предупреждения, дождитесь возвращения всех модулей в состояние ОК, затем закройте и заново откройте модуль приложения Application Insights.
* Кроме того, просмотрите [наш блог о состояниях](https://blogs.msdn.microsoft.com/servicemap-status/).
* Возможно, вы написали код для [серверного пакета SDK](../../azure-monitor/app/api-custom-events-metrics.md), который мог изменить ключ инструментирования в экземплярах `TelemetryClient` или в `TelemetryContext`. Или вы создали [конфигурацию фильтра или выборки](../../azure-monitor/app/api-filtering-sampling.md) , которая отфильтровывает слишком много данных.
* Если вы вносили изменения в файл ApplicationInsights.config, внимательно проверьте конфигурацию [TelemetryInitializer и TelemetryProcessor](../../azure-monitor/app/api-filtering-sampling.md). Тип или параметр с неправильным именем может стать причиной неотправки данных пакетом SDK.

## <a name="q04"></a>Нет данных об использовании, браузере и просмотрах страниц
*Данные отображаются на диаграммах "Время ответа сервера" и "Запросы сервера", но отсутствуют в колонках "Время загрузки страницы", "Браузер" или "Использование".*

Данные поступают из сценариев на веб-страницах. 

* Если вы добавили Application Insights в существующий веб-проект, [нужно добавить сценарии вручную](../../azure-monitor/app/javascript.md).
* Убедитесь, что Internet Explorer не отображает ваш сайт в режиме совместимости.
* Воспользуйтесь функцией отладки в браузере (в некоторых браузерах нужно нажать клавишу F12 и выбрать "Сеть"), чтобы убедиться, что данные отправляются по адресу `dc.services.visualstudio.com`.

## <a name="no-dependency-or-exception-data"></a>Нет данных о зависимостях или исключениях
См. статьи, посвященные [телеметрии зависимостей](../../azure-monitor/app/asp-net-dependencies.md) и [телеметрии исключений](asp-net-exceptions.md).

## <a name="no-performance-data"></a>Нет данных о производительности
Данные о производительности (ЦП, скорость ввода-вывода и т. д.) доступны для [веб-служб Java](../../azure-monitor/app/java-collectd.md), [классических приложений Windows](../../azure-monitor/app/windows-desktop.md), [веб-приложений и служб IIS, если установлен монитор состояния](../../azure-monitor/app/monitor-performance-live-website-now.md), а также [облачных служб Azure](../../azure-monitor/app/app-insights-overview.md). Их можно найти, открыв раздел "Параметры" и выбрав "Серверы".

## <a name="no-server-data-since-i-published-the-app-to-my-server"></a>После публикации приложения на сервере данные на нем отсутствуют
* Проверьте, что вы фактически скопировали все библиотеки DLL Microsoft. ApplicationInsights на сервер вместе с Microsoft.Diagnostics.Instrumentation.Extensions.Intercept.dll
* В брандмауэре может потребоваться [открыть некоторые порты TCP](../../azure-monitor/app/ip-addresses.md).
* Если для отправки из корпоративной сети необходимо использовать прокси-сервер, установите значение [defaultProxy](https://msdn.microsoft.com/library/aa903360.aspx) в файле Web.config.
* Для Windows Server 2008. Убедитесь, что установлены следующие обновления: [KB2468871](https://support.microsoft.com/kb/2468871), [KB2533523](https://support.microsoft.com/kb/2533523), [KB2600217](https://support.microsoft.com/kb/2600217).

## <a name="i-used-to-see-data-but-it-has-stopped"></a>Ранее видимые данные перестали отображаться
* Проверьте [блог состояний](https://blogs.msdn.com/b/applicationinsights-status/).
* Вы достигли месячной квоты точек данных? Чтобы выяснить это, последовательно выберите «Параметры», «Квота и расценки». Если вы достигли квоты, вы можете изменить свой тарифный план или заплатить за дополнительную емкость. См. [таблицу цен](https://azure.microsoft.com/pricing/details/application-insights/).

## <a name="i-dont-see-all-the-data-im-expecting"></a>Не отображаются все данные, которые ожидалось увидеть
Если ваше приложение отправляет большие объемы данных, а вы используете Application Insights SDK для ASP.NET версии 2.0.0-beta3 или более поздней, может сработать функция [адаптивной выборки](../../azure-monitor/app/sampling.md) и будет отправлена только часть данных телеметрии.

Ее можно отключить, но это не рекомендуется. Выборка разработана таким образом, чтобы связанные данные телеметрии правильно передавались для диагностических целей.

## <a name="client-ip-address-is-0000"></a>IP-адрес клиента — 0.0.0.0

О прекращении ведения журналов с IP-адресами клиентов было объявлено 5 февраля 2018 г. Это не влияет на географическое расположение.

> [!NOTE]
> Если вам требуются первые три октета IP-адресов, можно использовать [инициализатор телеметрии](https://docs.microsoft.com/azure/application-insights/app-insights-api-filtering-sampling#add-properties-itelemetryinitializer) для добавления пользовательского свойства.
> Это изменение не влияет на данные, собранные до 5 февраля 2018 г.

## <a name="wrong-geographical-data-in-user-telemetry"></a>Неправильные географические данные в телеметрии пользователя
Город, область и страна определяются по IP-адресу и не всегда являются точными. Эти IP-адреса сначала обрабатываются для расположения, а затем изменяются на IP-адрес 0.0.0.0 для хранения.

## <a name="exception-method-not-found-on-running-in-azure-cloud-services"></a>Исключение "метод не найден" при выполнении в облачных службах Azure
Выполняется сборка для .NET 4.6? Версия 4.6 не поддерживается автоматически в ролях облачных служб Azure. [установите версию 4.6 в каждой роли](../../cloud-services/cloud-services-dotnet-install-dotnet.md) .

## <a name="troubleshooting-logs"></a>Журналы для устранения неполадок

Следуйте этим инструкциям, чтобы записать журналы устранения неполадок для своей платформы.

### <a name="net-framework"></a>.NET Framework

1. Установите пакет [Microsoft.AspNet.ApplicationInsights.HostingStartup](https://www.nuget.org/packages/Microsoft.AspNet.ApplicationInsights.HostingStartup) из NuGet. Устанавливаемая версия должна соответствовать текущей установленной версии `Microsoft.ApplicationInsighs`

2. Измените файл applicationinsights.config, чтобы включить следующее:

    ```xml
    <TelemetryModules>
      <Add Type="Microsoft.ApplicationInsights.Extensibility.HostingStartup.FileDiagnosticsTelemetryModule, Microsoft.AspNet.ApplicationInsights.HostingStartup">
        <Severity>Verbose</Severity>
        <LogFileName>mylog.txt</LogFileName>
        <LogFilePath>C:\\SDKLOGS</LogFilePath>
      </Add>
    </TelemetryModules>
    ```
    У приложения должны быть разрешения на запись в настроенное расположение.

3. Перезапустите процесс таким образом, чтобы эти новые параметры были выбраны пакетом SDK.

4. Когда вы закончите, отмените эти изменения.

### <a name="net-core"></a>.NET Core

1. Установите пакет [Microsoft.AspNet.ApplicationInsights.HostingStartup](https://www.nuget.org/packages/Microsoft.AspNet.ApplicationInsights.HostingStartup) из NuGet. Устанавливаемая версия должна соответствовать текущей установленной версии `Microsoft.ApplicationInsights`

Последнюю версию Microsoft.ApplicationInsights.AspNetCore 2.7.1, и он относится к версии 2.10 Microsoft.ApplicationInsights. Таким образом версия Microsoft.AspNet.ApplicationInsights.HostingStartup для установки должна быть 2.10.0

2. Изменить `ConfigureServices` метод в вашем `Startup.cs` классе:

    ```csharp
    services.AddSingleton<ITelemetryModule, FileDiagnosticsTelemetryModule>();
    services.ConfigureTelemetryModule<FileDiagnosticsTelemetryModule>( (module, options) => {
        module.LogFilePath = "C:\\SDKLOGS";
        module.LogFileName = "mylog.txt";
        module.Severity = "Verbose";
    } );
    ```
    У приложения должны быть разрешения на запись в настроенное расположение.

3. Перезапустите процесс таким образом, чтобы эти новые параметры были выбраны пакетом SDK.

4. Когда вы закончите, отмените эти изменения.


## <a name="PerfView"></a> Сбор журналов с помощью PerfView
[PerfView](https://github.com/Microsoft/perfview) — это бесплатное средство диагностики и анализа производительности, изолировать ЦП, памяти и других проблем, собирая и визуализация диагностические данные из множества источников.

Пакет SDK Application Insights журнал, журналы для самостоятельного устранения неполадок EventSource, может отслеживаться с помощью PerfView.

Чтобы собирать журналы, загрузите PerfView и выполните следующую команду:
```cmd
PerfView.exe collect -MaxCollectSec:300 -NoGui /onlyProviders=*Microsoft-ApplicationInsights-Core,*Microsoft-ApplicationInsights-Data,*Microsoft-ApplicationInsights-WindowsServer-TelemetryChannel,*Microsoft-ApplicationInsights-Extensibility-AppMapCorrelation-Dependency,*Microsoft-ApplicationInsights-Extensibility-AppMapCorrelation-Web,*Microsoft-ApplicationInsights-Extensibility-DependencyCollector,*Microsoft-ApplicationInsights-Extensibility-HostingStartup,*Microsoft-ApplicationInsights-Extensibility-PerformanceCollector,*Microsoft-ApplicationInsights-Extensibility-PerformanceCollector-QuickPulse,*Microsoft-ApplicationInsights-Extensibility-Web,*Microsoft-ApplicationInsights-Extensibility-WindowsServer,*Microsoft-ApplicationInsights-WindowsServer-Core,*Microsoft-ApplicationInsights-Extensibility-EventSourceListener,*Microsoft-ApplicationInsights-AspNetCore
```

При необходимости можно изменить эти параметры:
- **MaxCollectSec**. Установите этот параметр, чтобы предотвратить PerfView запускать неограниченное время и повлиять на производительность сервера.
- **OnlyProviders**. Установите этот параметр, чтобы собирать журналы только из пакета SDK. Вы можете настроить этот список, в зависимости от вашей определенные исследования. 
- **NoGui**. Установите этот параметр для сбора журналов без графического пользовательского интерфейса.


Дополнительные сведения
- [Запись трассировок производительности с помощью PerfView](https://github.com/dotnet/roslyn/wiki/Recording-performance-traces-with-PerfView).
- [Источники событий анализа приложения](https://github.com/microsoft/ApplicationInsights-Home/tree/master/Samples/ETW)

## <a name="still-not-working"></a>По-прежнему не работает...
* [Форум Application Insights](https://social.msdn.microsoft.com/Forums/vstudio/en-US/home?forum=ApplicationInsights)
