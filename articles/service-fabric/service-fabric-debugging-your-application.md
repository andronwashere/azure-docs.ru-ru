---
title: Отладка приложения в Visual Studio | Документация Майкрософт
description: Повысьте надежность и производительность служб, разрабатывая их в Visual Studio и локальном кластере разработки.
services: service-fabric
documentationcenter: .net
author: vturecek
manager: chackdan
editor: ''
ms.assetid: cb888532-bcdb-4e47-95e4-bfbb1f644da4
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.custom: vs-azure
ms.workload: azure-vs
ms.date: 11/02/2017
ms.author: vturecek
ms.openlocfilehash: 682059914b5d86f5e670e373a4acf3e4ac6246ba
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "66428209"
---
# <a name="debug-your-service-fabric-application-by-using-visual-studio"></a>Отладка приложения Service Fabric с помощью Visual Studio
> [!div class="op_single_selector"]
> * [Visual Studio и CSharp](service-fabric-debugging-your-application.md) 
> * [Eclipse и Java](service-fabric-debugging-your-application-java.md)
>


## <a name="debug-a-local-service-fabric-application"></a>Отладка локального приложения Service Fabric
Вы можете сэкономить время и деньги, развернув приложение Azure Service Fabric и выполнив его отладку в кластере для разработки, состоящем из локальных компьютеров. Visual Studio 2019 или Visual Studio 2015 можно развернуть приложение в локальном кластере и автоматически подключить отладчик ко всем экземплярам приложения. Visual Studio должна выполняться от имени администратора для подключения отладчика.

1. Запустите кластер локальной разработки, выполнив действия, описанные в статье [Настройка среды разработки Service Fabric](service-fabric-get-started.md).
2. Нажмите клавишу **F5** или выберите в меню **Отладка** > **Начать отладку**.
   
    ![Запуск отладки приложения][startdebugging]
3. Задайте в коде точки останова и поэтапно выполните приложение, выбирая соответствующие команды в меню **Отладка** .
   
   > [!NOTE]
   > Visual Studio подключается ко всем экземплярам приложения. При пошаговом выполнении кода разные процессы могут одновременно достигать точек останова. Это приводит к возникновению параллельных сеансов. Чтобы этого не происходило, настройте отключение точек останова после их достижения, сделав их зависимыми от идентификатора потока, или используйте события диагностики.
   > 
   > 
4. Окно **События диагностики** открывается автоматически и позволяет просматривать данные о событиях диагностики в реальном времени.
   
    ![Просмотр событий диагностики в режиме реального времени][diagnosticevents]
5. Кроме того, вы можете открыть окно **События диагностики** вручную, используя обозреватель облака.  В разделе **Service Fabric** щелкните любой узел правой кнопкой мыши и выберите пункт **Показать потоковую передачу трассировок**.
   
    ![Окно событий диагностики][viewdiagnosticevents]
   
    Если вы хотите отфильтровать трассировки по определенной службы или приложения, необходимо включите потоковую передачу трассировки по этой службе или приложении.
6. События диагностики можно просматривать в автоматически созданном файле **ServiceEventSource.cs** и вызывать из кода приложения.
   
    ```csharp
    ServiceEventSource.Current.ServiceMessage(this, "My ServiceMessage with a parameter {0}", result.Value.ToString());
    ```
7. В окне **События диагностики** поддерживаются фильтрация, приостановка и проверка событий в реальном времени.  Фильтр представляет собой простой поиск по строкам сообщений о событиях, включая их содержимое.
   
    ![Фильтрация, приостановка, возобновление и проверка событий в реальном времени][diagnosticeventsactions]
8. Отладка служб аналогична отладке приложений. Обычно для простой отладки необходимо установить точки останова с помощью Visual Studio. Несмотря на то, что надежные коллекции реплицируются на нескольких узлах, они все равно реализуют интерфейс IEnumerable. Эта реализация означает, что представления результатов можно использовать в Visual Studio во время отладки для просмотра, хранящиеся внутри. Чтобы сделать это, установите точку останова в любом месте в коде.
   
    ![Запуск отладки приложения][breakpoint]

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->

## <a name="debug-a-remote-service-fabric-application"></a>Отладка удаленного приложения Service Fabric
Если приложения Service Fabric выполняются в кластере Service Fabric в Azure, можно дистанционно отлаживать эти приложения, непосредственно из Visual Studio.

> [!NOTE]
> Компоненту требуется [пакет SDK 2.0 для Service Fabric](https://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-VS2015) и [пакет SDK Azure для .NET 2.9](https://azure.microsoft.com/downloads/).    
> 
> 

<!-- -->
> [!WARNING]
> Удаленная отладка предназначена для сценариев разработки и тестирования и не должна использоваться в производственной среде, поскольку влияет на запущенные приложения.
> 
> 

1. Перейдите к своему кластеру в **Cloud Explorer**. Щелкните правой кнопкой мыши и выберите **включить отладку**
   
    ![Включение удаленной отладки][enableremotedebugging]
   
    Это действие запустит процесс включения расширение удаленной отладки на узлах кластера и необходимые конфигурации сети.
2. Щелкните узел кластера в **обозревателе облака** правой кнопкой мыши и выберите пункт **Подключить отладчик**.
   
    ![Подключить отладчик][attachdebugger]
3. В диалоговом окне **Подключение к процессу** выберите процесс, который нужно отладить, и нажмите кнопку **Подключить**.
   
    ![Выбор процесса][chooseprocess]
   
    Имя процесса, к которому нужно подключиться, совпадает с именем сборки для проекта службы.
   
    Отладчик подключится ко всем узлам, на которых выполняется процесс.
   
   * В случае, когда отладке службы без отслеживания состояния все экземпляры службы на все узлы являются частью сеанса отладки.
   * При отладке службы с отслеживанием состояния, первичная реплика каждого раздела будет активна и таким образом, отладчик затрагивает. Если во время сеанса отладки первичная реплика переносится, обработка этой реплики по-прежнему входит в сеанс отладки.
   * Чтобы перехватывать только релевантные секции или экземпляры данной службы, можно использовать условные точки останова будут останавливать только определенную секцию или экземпляр.
     
     ![Условная точка останова][conditionalbreakpoint]
     
     > [!NOTE]
     > Сейчас отладка кластера Service Fabric с несколькими экземплярами исполняемого имени одной и той же службы не поддерживается.
     > 
     > 
4. После того как отладка приложения будет завершена, расширение удаленной отладки можно отключить, щелкнув кластер в **обозревателе облака** правой кнопкой мыши и выбрав пункт **Отключить отладку**.
   
    ![Отключение удаленной отладки][disableremotedebugging]

## <a name="streaming-traces-from-a-remote-cluster-node"></a>Потоковая передача трассировок из удаленного узла кластера
Вы можете также в тех трассировках потока напрямую из удаленного узла кластера для Visual Studio. Эта функция позволяет передавать события трассировки ETW, возникающие на узле кластера Service Fabric.

> [!NOTE]
> Компоненту требуется [пакет SDK 2.0 для Service Fabric](https://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-VS2015) и [пакет SDK Azure для .NET 2.9](https://azure.microsoft.com/downloads/).
> Эта функция поддерживает только кластеры на платформе Azure.
> 
> 

<!-- -->
> [!WARNING]
> Потоковая передача трассировок предназначена для сценариев разработки и тестирования и не должна использоваться в производственной среде, поскольку влияет на запущенные приложения.
> В рабочем сценарии следует применять пересылку событий с помощью средств диагностики Azure.
> 
> 

1. Перейдите к своему кластеру в **Cloud Explorer**. Щелкните правой кнопкой мыши и выберите **включить трассировки потоковой передачи**
   
    ![Включение трассировки удаленной потоковой передачи][enablestreamingtraces]
   
    Это действие запустит процесс включается расширение потоковой передачи трассировок на узлах кластера, а также необходимые конфигурации сети.
2. Разверните элемент **Узлы** в **обозревателе облака**, щелкните правой кнопкой мыши узел, потоковую передачу трассировок с которого вы хотите включить, и выберите пункт **Показать потоковую передачу трассировок**.
   
    ![Просмотр трассировки удаленной потоковой передачи][viewremotestreamingtraces]
   
    Повторите шаг 2 для всех узлов, трассировку которых вы хотите получать. Потоковая передача каждого узла будет отображаться в отдельном окне.
   
    Теперь вы можете также получать трассировки, которые выпускает Service Fabric и ваши собственные службы. Если вы хотите отфильтровать события по определенному приложению, просто введите название этого приложения в качестве фильтра.
   
    ![Просмотр трассировки потоковой передачи][viewingstreamingtraces]
3. После того как потоковая передача трассировок будет завершена, вы можете отключить удаленную потоковую передачу трассировок, щелкнув кластер в **обозревателе облака** правой кнопкой мыши и выбрав пункт **Отключить трассировки потоковой передачи**.
   
    ![Отключение трассировки удаленной потоковой передачи][disablestreamingtraces]

## <a name="next-steps"></a>Дальнейшие действия
* [Проверка службы Service Fabric](service-fabric-testability-overview.md)
* [Управление приложениями Service Fabric в Visual Studio](service-fabric-manage-application-in-visual-studio.md)

<!--Image references-->
[startdebugging]: ./media/service-fabric-debugging-your-application/startdebugging.png
[diagnosticevents]: ./media/service-fabric-debugging-your-application/diagnosticevents.png
[viewdiagnosticevents]: ./media/service-fabric-debugging-your-application/viewdiagnosticevents.png
[diagnosticeventsactions]: ./media/service-fabric-debugging-your-application/diagnosticeventsactions.png
[breakpoint]: ./media/service-fabric-debugging-your-application/breakpoint.png
[enableremotedebugging]: ./media/service-fabric-debugging-your-application/enableremotedebugging.png
[attachdebugger]: ./media/service-fabric-debugging-your-application/attachdebugger.png
[chooseprocess]: ./media/service-fabric-debugging-your-application/chooseprocess.png
[conditionalbreakpoint]: ./media/service-fabric-debugging-your-application/conditionalbreakpoint.png
[disableremotedebugging]: ./media/service-fabric-debugging-your-application/disableremotedebugging.png
[enablestreamingtraces]: ./media/service-fabric-debugging-your-application/enablestreamingtraces.png
[viewingstreamingtraces]: ./media/service-fabric-debugging-your-application/viewingstreamingtraces.png
[viewremotestreamingtraces]: ./media/service-fabric-debugging-your-application/viewremotestreamingtraces.png
[disablestreamingtraces]: ./media/service-fabric-debugging-your-application/disablestreamingtraces.png
