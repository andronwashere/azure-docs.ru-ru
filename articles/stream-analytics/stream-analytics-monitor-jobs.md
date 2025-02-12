---
title: Мониторинг заданий Azure Stream Analytics и управление ими с помощью программных средств
description: В этой статье объясняется, как с помощью программных средств отслеживать задания Stream Analytics, созданные с помощью REST API, пакета SDK для Azure или PowerShell.
services: stream-analytics
author: jseb225
ms.author: jeanb
manager: kfile
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 04/20/2017
ms.openlocfilehash: bf84a5b89e4769e37c45714a30d5d98300a4328d
ms.sourcegitcommit: 6a42dd4b746f3e6de69f7ad0107cc7ad654e39ae
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/07/2019
ms.locfileid: "67612259"
---
# <a name="programmatically-create-a-stream-analytics-job-monitor"></a>Создание монитора заданий Stream Analytics программным способом

В этой статье рассказывается, как включить функцию отслеживания задания Stream Analytics. Отслеживание заданий Stream Analytics, созданных с помощью интерфейсов API REST, пакета SDK для Azure и оболочки PowerShell, по умолчанию отключено. Вы можете вручную включить отслеживание на портале Azure. Для этого перейдите на страницу "Отслеживание" задания и нажмите кнопку "Включить". Этот процесс также можно автоматизировать, выполнив описанные в этой статье действия. Данные отслеживания будут отображаться в области метрики на портале Azure для задания Stream Analytics.

## <a name="prerequisites"></a>Предварительные требования

Прежде чем начать этот процесс, необходимо иметь следующее:

* 2019 г. Visual Studio или Visual Studio 2015
* Скачанный и установленный [пакет SDK Azure для .NET](https://azure.microsoft.com/downloads/).
* Существующее задание Stream Analytics, отслеживание которого нужно включить.

## <a name="create-a-project"></a>Создание проекта

1. Создайте консольное приложение Visual Studio C# .NET.
2. В консоли диспетчера пакетов выполните следующие команды, чтобы установить пакеты NuGet. Первый — пакет SDK для .NET для управления Azure Stream Analytics. Второй — пакет SDK для Azure Monitor, который будет использоваться для включения отслеживания. Последний — клиент Azure Active Directory, который будет использоваться для проверки подлинности.
   
   ```powershell
   Install-Package Microsoft.Azure.Management.StreamAnalytics
   Install-Package Microsoft.Azure.Insights -Pre
   Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory
   ```
3. Добавьте следующий раздел appSettings в файл App.config.
   
   ```csharp
   <appSettings>
     <!--CSM Prod related values-->
     <add key="ResourceGroupName" value="RESOURCE GROUP NAME" />
     <add key="JobName" value="YOUR JOB NAME" />
     <add key="StorageAccountName" value="YOUR STORAGE ACCOUNT"/>
     <add key="ActiveDirectoryEndpoint" value="https://login.microsoftonline.com/" />
     <add key="ResourceManagerEndpoint" value="https://management.azure.com/" />
     <add key="WindowsManagementUri" value="https://management.core.windows.net/" />
     <add key="AsaClientId" value="1950a258-227b-4e31-a9cf-717495945fc2" />
     <add key="RedirectUri" value="urn:ietf:wg:oauth:2.0:oob" />
     <add key="SubscriptionId" value="YOUR AZURE SUBSCRIPTION ID" />
     <add key="ActiveDirectoryTenantId" value="YOUR TENANT ID" />
   </appSettings>
   ```
   Замените значения *SubscriptionId* и *ActiveDirectoryTenantId* идентификаторами подписки Azure и клиента. Вы можете получить эти значения, запустив следующий командлет Azure PowerShell:
   
   ```powershell
   Get-AzureAccount
   ```
4. Добавьте следующие инструкции с оператором using в исходный файл (Program.cs) в проекте.
   
   ```csharp
     using System;
     using System.Configuration;
     using System.Threading;
     using Microsoft.Azure;
     using Microsoft.Azure.Management.Insights;
     using Microsoft.Azure.Management.Insights.Models;
     using Microsoft.Azure.Management.StreamAnalytics;
     using Microsoft.Azure.Management.StreamAnalytics.Models;
     using Microsoft.IdentityModel.Clients.ActiveDirectory;
   ```
5. Добавьте вспомогательный метод проверки подлинности.

   ```csharp   
   public static string GetAuthorizationHeader()
   {
      AuthenticationResult result = null;
      var thread = new Thread(() =>
      {
         try
         {
             var context = new AuthenticationContext(
                ConfigurationManager.AppSettings["ActiveDirectoryEndpoint"] +
                ConfigurationManager.AppSettings["ActiveDirectoryTenantId"]);
             result = context.AcquireToken(
                 resource: ConfigurationManager.AppSettings["WindowsManagementUri"],
                 clientId: ConfigurationManager.AppSettings["AsaClientId"],
                 redirectUri: new Uri(ConfigurationManager.AppSettings["RedirectUri"]),
                 promptBehavior: PromptBehavior.Always);
         }
         catch (Exception threadEx)
         {
             Console.WriteLine(threadEx.Message);
         }
     });

     thread.SetApartmentState(ApartmentState.STA);
     thread.Name = "AcquireTokenThread";
     thread.Start();
     thread.Join();
   
     if (result != null)
     {
         return result.AccessToken;
     }
         throw new InvalidOperationException("Failed to acquire token");
   }
   ```

## <a name="create-management-clients"></a>Создание клиентов управления

С помощью следующего кода можно настроить необходимые переменные и клиенты управления.

   ```csharp
    string resourceGroupName = "<YOUR AZURE RESOURCE GROUP NAME>";
    string streamAnalyticsJobName = "<YOUR STREAM ANALYTICS JOB NAME>";

    // Get authentication token
    TokenCloudCredentials aadTokenCredentials =
        new TokenCloudCredentials(
            ConfigurationManager.AppSettings["SubscriptionId"],
            GetAuthorizationHeader());

    Uri resourceManagerUri = new
    Uri(ConfigurationManager.AppSettings["ResourceManagerEndpoint"]);

    // Create Stream Analytics and Insights management client
    StreamAnalyticsManagementClient streamAnalyticsClient = new
    StreamAnalyticsManagementClient(aadTokenCredentials, resourceManagerUri);
    InsightsManagementClient insightsClient = new
    InsightsManagementClient(aadTokenCredentials, resourceManagerUri);
   ```

## <a name="enable-monitoring-for-an-existing-stream-analytics-job"></a>Включение отслеживания существующего задания Stream Analytics

С помощью следующего кода можно включить отслеживание **имеющегося** задания Stream Analytics. В первой части кода в службу Stream Analytics отправляется запрос GET, что позволяет получить сведения о конкретном задании Stream Analytics. Она использует *идентификатор* свойство (полученное в результате выполнения запроса GET) в качестве параметра метода Put во второй части кода, который отправляет запрос PUT запрос в службу Insights для включения отслеживания и задание Stream Analytics.

> [!WARNING]
> Если ранее вы включили функцию отслеживания для другого задания Stream Analytics либо на портале Azure, либо программным способом с помощью указанного ниже кода, **мы рекомендуем использовать ту же учетную запись хранения, которую вы использовали для включения функции отслеживания.**
> 
> Учетная запись хранения связана с регионом, в котором вы создали свое задание Stream Analytics, а не с самим заданием.
> 
> Все задания Stream Analytics (и все остальные ресурсы Azure) в этом регионе совместно используют эту учетную запись хранения для хранения данных отслеживания. Если указать другую учетную запись хранения, это может привести к непредвиденным побочным эффектам при отслеживании других заданий Stream Analytics или других ресурсов Azure.
> 
> Имя учетной записи хранения, используемой для замены `<YOUR STORAGE ACCOUNT NAME>` в следующем коде, должно представлять собой учетную запись хранения, которая входит в ту же подписку, что и задание Stream Analytics, для которого вы включаете функцию отслеживания.
> 
> 
>    ```csharp
>    // Get an existing Stream Analytics job
>     JobGetParameters jobGetParameters = new JobGetParameters()
>     {
>         PropertiesToExpand = "inputs,transformation,outputs"
>     };
>     JobGetResponse jobGetResponse = streamAnalyticsClient.StreamingJobs.Get(resourceGroupName, streamAnalyticsJobName, jobGetParameters);
>
>    // Enable monitoring
>    ServiceDiagnosticSettingsPutParameters insightPutParameters = new ServiceDiagnosticSettingsPutParameters()
>    {
>            Properties = new ServiceDiagnosticSettings()
>           {
>               StorageAccountName = "<YOUR STORAGE ACCOUNT NAME>"
>           }
>    };
>   insightsClient.ServiceDiagnosticSettingsOperations.Put(jobGetResponse.Job.Id, insightPutParameters);
>   ```


## <a name="get-support"></a>Получение поддержки

За дополнительной помощью обращайтесь на наш [форум Azure Stream Analytics](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureStreamAnalytics).

## <a name="next-steps"></a>Дальнейшие действия

* [Введение в Azure Stream Analytics](stream-analytics-introduction.md)
* [Приступая к работе с Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Масштабирование заданий в службе Azure Stream Analytics](stream-analytics-scale-jobs.md)
* [Справочник по языку запросов Azure Stream Analytics](https://docs.microsoft.com/stream-analytics-query/stream-analytics-query-language-reference)
* [Справочник по API-интерфейсу REST управления Stream Analytics](https://msdn.microsoft.com/library/azure/dn835031.aspx)
