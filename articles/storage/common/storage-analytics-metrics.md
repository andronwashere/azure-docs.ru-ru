---
title: Метрики аналитики хранилища Azure (Классическая модель)
description: Узнайте, как использовать метрики в хранилище Azure.
services: storage
author: normesta
ms.service: storage
ms.topic: article
ms.date: 03/11/2019
ms.author: normesta
ms.reviewer: fryu
ms.subservice: common
ms.openlocfilehash: 5fecced844b3580c83fd18d0c14c3a2083f7a4fc
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/17/2019
ms.locfileid: "67165739"
---
# <a name="azure-storage-analytics-metrics-classic"></a>Метрики аналитики хранилища Azure (Классическая модель)

Аналитика хранилища может хранить метрики, включающие сводную статистику по транзакциям и данные о емкости запросов к службе хранилища. Сообщения о транзакциях предоставляются как на уровне операций API, так и на уровне службы хранилища, а сообщения о емкости предоставляются на уровне службы хранилища. Данные метрик можно использовать для анализа использования службы хранилища, диагностики проблем с запросами к службе хранилища и повышения производительности приложений, использующих службу.  

 Метрики аналитики хранилища включены по умолчанию для новых учетных записей хранения. Можно настроить метрики в [портала Azure](https://portal.azure.com/); Дополнительные сведения см. в разделе [мониторинг учетной записи хранения на портале Azure](/azure/storage/storage-monitor-storage-account). Аналитику хранилища также можно включить программно через REST API или клиентскую библиотеку. Чтобы включить аналитику хранилища для каждой службы, используйте операции Set Service Properties.  

> [!NOTE]
> Метрики аналитики хранилища доступны для служб BLOB-объектов, очереди, таблицы и файла.
> Метрики аналитики хранилища стали классической метрики. Корпорация Майкрософт рекомендует использовать [метрик хранилища в Azure Monitor](storage-metrics-in-azure-monitor.md) вместо метрик аналитики хранилища.

## <a name="transaction-metrics"></a>Метрики транзакций  
 Широкий набор данных записывается раз в час или раз в минуту для каждой службы хранилища и запрошенных операций API, включая входящие и выходящие данные, информацию о доступности, ошибки и процент категоризованных запросов. Полный список сведений о транзакциях см. в разделе [Схема таблицы для метрик аналитики хранилища](/rest/api/storageservices/storage-analytics-metrics-table-schema).  

 Данные транзакций записываются на двух уровнях — уровне служб и уровне операций API. На уровне службы статистика, суммирующая все запрошенные операции API, записывается в сущность таблицы каждый час, даже если запросов к службе не было. На уровне операции API статистика записывается в сущность, только если в течение этого часа были запрошены операции.  

 Например, если вы выполняете операцию **GetBlob** в службе BLOB-объектов, метрики аналитики хранилища будут регистрировать запрос и включать его в объединенные данные как по службе BLOB-объектов, так и по операции **GetBlob**. Тем не менее если не **GetBlob** операции запрашивается в течение часа, сущность не записывается в *$MetricsTransactionsBlob* для этой операции.  

 Метрики транзакций записываются как для запросов пользователей, так и для запросов, сделанных самой аналитикой хранилища. Например, записываются запросы аналитики хранилища для записи сущностей журналов и таблиц.

## <a name="capacity-metrics"></a>Метрики емкости  

> [!NOTE]
>  В настоящее время метрики емкости доступны только для службы BLOB-объектов.

 Данные о емкости записываются ежедневно для службы BLOB-объектов учетной записи хранения, и при этом записываются две сущности таблицы. Одна сущность содержит статистику пользовательских данных, а другая — статистику для контейнера больших двоичных объектов `$logs` , используемого аналитикой хранилища. *$MetricsCapacityBlob* таблица включает следующую статистику:  

- **Capacity**. Объем хранилища, используемый службой BLOB-объектов учетной записи хранения, в байтах.  
- **ContainerCount**. Число контейнеров больших двоичных объектов в службе BLOB-объектов учетной записи хранения.  
- **ObjectCount**. Количество зафиксированных и незафиксированных блочных или страничных BLOB-объектов в службе BLOB-объектов учетной записи хранения.  

  Дополнительную информацию о метриках емкости см. в разделе [Схема таблицы для метрик аналитики хранилища](/rest/api/storageservices/storage-analytics-metrics-table-schema).  

## <a name="how-metrics-are-stored"></a>Как хранятся метрики  

 Все данные метрик для каждой из служб хранилища хранятся в таблицах, зарезервированных для этой службы: одна таблица для данных о транзакциях, вторая для данных о минутных транзакциях и третья для данных о емкости. Информация о транзакциях и минутных транзакциях включает в себя данные запросов и ответов, а информация о емкости содержит данные об использовании хранилища. Часовые метрики, минутные метрики и емкость для хранения, учетной записи службы BLOB-объектов можно получить в таблицах, которые имеют имена, как описано в следующей таблице.  

|Уровень метрик|Имена таблиц|Поддерживается для версий|  
|-------------------|-----------------|----------------------------|  
|Часовые метрики, основное расположение|-$MetricsTransactionsBlob<br />-$MetricsTransactionsTable<br />-$MetricsTransactionsQueue|Только версии до 15.08.2013 г. Таблицы с такими именами по-прежнему поддерживаются, но рекомендуется перейти на использование таблиц, приведенных ниже.|  
|Часовые метрики, основное расположение|-   $MetricsHourPrimaryTransactionsBlob<br />-   $MetricsHourPrimaryTransactionsTable<br />-   $MetricsHourPrimaryTransactionsQueue<br />-   $MetricsHourPrimaryTransactionsFile|Все версии. Поддержка метрик службы файл доступен только в версии 2015-04-05 и более поздних версий.|  
|Минутные метрики, основное расположение|-$MetricsMinutePrimaryTransactionsBlob<br />-$MetricsMinutePrimaryTransactionsTable<br />-   $MetricsMinutePrimaryTransactionsQueue<br />-$MetricsMinutePrimaryTransactionsFile|Все версии. Поддержка метрик службы файл доступен только в версии 2015-04-05 и более поздних версий.|  
|Часовые метрики, вторичное расположение|-$MetricsHourSecondaryTransactionsBlob<br />-$MetricsHourSecondaryTransactionsTable<br />-$MetricsHourSecondaryTransactionsQueue|Все версии. Должна быть включена геоизбыточная репликация с доступом для чтения.|  
|Минутные метрики, вторичное расположение|-$MetricsMinuteSecondaryTransactionsBlob<br />-$MetricsMinuteSecondaryTransactionsTable<br />-$MetricsMinuteSecondaryTransactionsQueue|Все версии. Должна быть включена геоизбыточная репликация с доступом для чтения.|  
|Емкость (только для службы BLOB-объектов)|$MetricsCapacityBlob|Все версии.|  

 Эти таблицы создаются автоматически при включении аналитики хранилища для конечной точки службы хранилища. К ним осуществляется через пространство имен учетной записи хранения, например: `https://<accountname>.table.core.windows.net/Tables("$MetricsTransactionsBlob")`. Таблицы метрики не отображаются в операции перечисления и должно осуществляться непосредственно через имя таблицы.  

## <a name="enable-metrics-using-the-azure-portal"></a>Включение метрик с помощью портала Azure
Выполните следующие действия, чтобы включить метрики на [портале Azure](https://portal.azure.com).

1. Войдите в свою учетную запись хранения.
1. Выберите **параметры диагностики (классические)** из **меню** области.
1. Убедитесь, что в разделе **Состояние** задано значение **Вкл**.
1. Выберите метрики для служб, за которыми вы хотите наблюдать.
1. Укажите политику хранения, чтобы задать период хранения метрик и данных журналов.
1. Щелкните **Сохранить**.

Сейчас [портал Azure](https://portal.azure.com) не позволяет настраивать минутные метрики в учетной записи хранения. Их необходимо включить с помощью PowerShell или программно.

> [!NOTE]
>  Обратите внимание на то, что портал Azure не настоящее время позволяет настраивать минутные метрики в учетной записи хранения. Необходимо включить с помощью PowerShell или программно.

## <a name="enable-storage-metrics-using-powershell"></a>Включение метрик хранилища с помощью PowerShell  
Чтобы настроить метрики хранилища в учетной записи хранения с помощью командлета Azure PowerShell можно использовать PowerShell на локальном компьютере **Get-AzureStorageServiceMetricsProperty** для получения текущих настроек и командлет  **SET-AzureStorageServiceMetricsProperty** изменение текущих параметров.  

Командлеты, позволяющие управлять метриками хранилища, имеют следующие параметры.  

* **ServiceType**, возможное значение — **BLOB-объектов**, **очереди**, **таблицы**, и **файл**.
* **MetricsType**, возможные значения: **час** и **минуту**.  
* **MetricsLevel**, возможными значениями являются:
* **Нет**. Отключает мониторинг.
* **Служба**: Сбор метрик, таких как исходящие и входящие данные, доступность, задержка и процент успешных операций, который агрегируется для больших двоичных объектов, очередей, таблиц и файловых служб.
* **ServiceAndApi**: Помимо метрик служба собирает тот же набор метрик для каждой операции с хранилищем в API службы хранилища Azure.

Например следующая команда выключает минутные метрики для службы BLOB-объектов в учетной записи хранения с срока хранения, установленного на пять дней: 

> [!NOTE]
> Эта команда предполагает, что вы вышли в свою подписку Azure с помощью `Connect-AzAccount` команды.

```  
$storageAccount = Get-AzStorageAccount -ResourceGroupName "<resource-group-name>" -AccountName "<storage-account-name>"

Set-AzureStorageServiceMetricsProperty -MetricsType Minute -ServiceType Blob -MetricsLevel ServiceAndApi  -RetentionDays 5 -Context $storageAccount.Context
```  

* Замените `<resource-group-name>` заполнитель с именем группы ресурсов.

* Замените значение заполнителя `<storage-account-name>` именем вашей учетной записи хранения.



Следующая команда получает текущий уровень часовых метрик и длительность периода хранения в днях для службы BLOB-объектов в учетной записи хранения по умолчанию:  

```  
Get-AzureStorageServiceMetricsProperty -MetricsType Hour -ServiceType Blob -Context $storagecontext.Context
```  

Дополнительные сведения о настройке командлетов Azure PowerShell для работы с подпиской Azure и о выборе учетной записи хранения по умолчанию см. в статье с [общими сведениями об Azure PowerShell](https://azure.microsoft.com/documentation/articles/install-configure-powershell/).  

## <a name="enable-storage-metrics-programmatically"></a>Программное Включение метрик хранилища  
Помимо использования портала Azure или командлетов Azure PowerShell управление метриками хранилища, можно также использовать один из API службы хранилища Azure. Например если вы используете язык .NET можно использовать клиентскую библиотеку хранилища.  

Классы **CloudBlobClient**, **CloudQueueClient**, **CloudTableClient**, и **CloudFileClient** все имеют методы, такие как  **SetServiceProperties** и **SetServicePropertiesAsync** , принимающих **ServiceProperties** объект в качестве параметра. Можно использовать **ServiceProperties** объекта, чтобы настроить метрики хранилища. Например, следующая C# фрагменте показано, как изменить метрик дни уровень и хранения для часовых метрик очереди:  

```csharp
var storageAccount = CloudStorageAccount.Parse(connStr);  
var queueClient = storageAccount.CreateCloudQueueClient();  
var serviceProperties = queueClient.GetServiceProperties();  

serviceProperties.HourMetrics.MetricsLevel = MetricsLevel.Service;  
serviceProperties.HourMetrics.RetentionDays = 10;  

queueClient.SetServiceProperties(serviceProperties);  
```  

Дополнительные сведения о настройке метрик хранилища с помощью языка .NET см. в разделе [клиентская библиотека хранилища для .NET](https://msdn.microsoft.com/library/azure/mt347887.aspx).  

Общие сведения о настройке метрик хранилища с помощью REST API см. в разделе [Включение и Настройка аналитики хранилища](/rest/api/storageservices/Enabling-and-Configuring-Storage-Analytics).  

##  <a name="viewing-storage-metrics"></a>Просмотр метрик хранилища  
После настройки метрик аналитики хранилища для мониторинга учетной записи хранения функция аналитики хранилища записывает метрики в набор известных таблиц в учетной записи хранения. Вы можете настроить диаграммы для просмотра почасовых метрик на [портале Azure](https://portal.azure.com).

1. Войдите в свою учетную запись хранения на [портале Azure](https://portal.azure.com).
1. Выберите **метрики (классические)** в **меню** колонке для службы, метрики которой вы хотите просмотреть.
1. Щелкните диаграмму, которую требуется настроить.
1. В колонке **Изменение диаграммы** щелкните **Диапазон времени**, **Тип диаграммы** и метрики, которые необходимо отобразить на диаграмме.

В **мониторинг (Классическая модель)** разделе колонке меню учетной записи хранения на портале Azure, можно настроить [правила генерации оповещений](#metrics-alerts), например, отправку по электронной почте оповещения, уведомляющие о конкретной метрики достигает определенное значение.

Если требуется, чтобы скачивать метрики для длительного хранения или анализа в локальной среде, необходимо использовать инструмент или написать код для чтения таблиц. Необходимо скачать минутные метрики для анализа. Таблицы не отображаются, если в учетной записи хранения выведены все таблицы. Однако к ним можно обращаться напрямую по имени. Многие средства обзора хранилищ поддерживают эти таблицы и позволяют просматривать их напрямую (см. в разделе [клиентские инструменты службы хранилища Azure](/azure/storage/storage-explorers) список доступных средств).

||||  
|-|-|-|  
|**Метрики**|**Имена таблиц**|**Примечания**|  
|Часовые метрики|$MetricsHourPrimaryTransactionsBlob<br /><br /> $MetricsHourPrimaryTransactionsTable<br /><br /> $MetricsHourPrimaryTransactionsQueue<br /><br /> $MetricsHourPrimaryTransactionsFile|В версиях до 2013-08-15 эти таблицы носили:<br /><br /> $MetricsTransactionsBlob<br /><br /> $MetricsTransactionsTable<br /><br /> $MetricsTransactionsQueue<br /><br /> Метрики для службы файлов которые доступны начиная с версии 2015-04-05.|  
|Минутные метрики|$MetricsMinutePrimaryTransactionsBlob<br /><br /> $MetricsMinutePrimaryTransactionsTable<br /><br /> $MetricsMinutePrimaryTransactionsQueue<br /><br /> $MetricsMinutePrimaryTransactionsFile|Можно включить только с помощью PowerShell или программно.<br /><br /> Метрики для службы файлов которые доступны начиная с версии 2015-04-05.|  
|Capacity|$MetricsCapacityBlob|Только служба BLOB-объектов.|  

Подробные сведения о схемах для этих таблиц см. в разделе [Схема таблицы метрик аналитики хранилища](/rest/api/storageservices/storage-analytics-metrics-table-schema). В примере строк ниже отражена только часть доступных столбцов, однако он иллюстрирует некоторые важные возможности сохранения метрик с помощью метрик хранилища:  

||||||||||||  
|-|-|-|-|-|-|-|-|-|-|-|  
|**PartitionKey**|**RowKey**|**Timestamp**|**TotalRequests**|**TotalBillableRequests**|**TotalIngress**|**TotalEgress**|**Доступность**|**AverageE2ELatency**|**AverageServerLatency**|**PercentSuccess**|  
|20140522T1100|user;All|2014-05-22T11:01:16.7650250Z|7|7|4003|46801|100|104.4286|6.857143|100|  
|20140522T1100|user;QueryEntities|2014-05-22T11:01:16.7640250Z|5|5|2694|45951|100|143.8|7.8|100|  
|20140522T1100|user;QueryEntity|2014-05-22T11:01:16.7650250Z|1|1|538|633|100|3|3|100|  
|20140522T1100|user;UpdateEntity|2014-05-22T11:01:16.7650250Z|1|1|771|217|100|9|6|100|  

В этом примере данных минутной метрики для ключа раздела используется время с минутным разрешением. Ключ строки определяет тип данных, которые хранятся в строке, и состоит из двух фрагментов данных — типа доступа и типа запроса:  

-   Типом доступа является либо **пользователя** или **системы**, где **пользователя** ссылается на все запросы пользователей к службе хранилища и **системы** ссылается на запросы, выполненные средством аналитики хранилища.  

-   Типом запроса является либо **все** в этом случае это строка сводки, или он определяет конкретный API, такие как **QueryEntity** или **UpdateEntity**.  

В примере данных выше показаны все записи за одну минуту (начиная с 11:00), поэтому число **QueryEntities** запрашивает плюс количество **QueryEntity** запрашивает плюс количество  **UpdateEntity** запросов добавьте семь, и эта сумма отображается в **user: All** строки. Аналогичным образом, можно создавать производные среднее время задержки end-to-end 104.4286 на **user: All** вычислив ((143.8 * 5) + 3 + 9) / 7.  

## <a name="metrics-alerts"></a>Оповещения метрик
Можно настроить оповещения на [портала Azure](https://portal.azure.com) , вам будут автоматически уведомлены о важных изменениях в поведении служб хранилища. Если скачивание этих метрик в формате с разделителями происходит с помощью средства обозревателя хранилища, для анализа данных можно использовать Microsoft Excel. Список доступных инструментов Storage Explorer см. в разделе [Клиентские инструменты службы хранилища Azure](/azure/storage/storage-explorers). Можно настроить оповещения в **оповещения (классические)** колонке, доступной в разделе **мониторинг (Классическая модель)** в колонке меню учетной записи хранения.

> [!IMPORTANT]
> Возможна задержка между событием службы хранилища и записью данных соответствующей ежечасной или ежеминутной метрики. В случае ежеминутных метрик данные за несколько минут могут записываться одновременно. Это может привести к объединению транзакций за предшествующие минуты с транзакциями за текущую минуту. В этом случае у службы оповещений могут быть данные не всех доступных метрик за заданный интервал оповещения, что может привести к неожиданному срабатыванию оповещений.
>

## <a name="accessing-metrics-data-programmatically"></a>Программный доступ к данным метрик  
В следующем списке показан пример кода на C#, в котором реализован доступ к минутным метрикам для диапазона минут с отображением результатов в окне консоли. В примере кода используется клиентская библиотека хранилища Azure версии 4.x или более поздней версии, который включает **CloudAnalyticsClient** класс, упрощающий доступ к таблицам метрик в хранилище.  

```csharp
private static void PrintMinuteMetrics(CloudAnalyticsClient analyticsClient, DateTimeOffset startDateTime, DateTimeOffset endDateTime)  
{  
 // Convert the dates to the format used in the PartitionKey  
 var start = startDateTime.ToUniversalTime().ToString("yyyyMMdd'T'HHmm");  
 var end = endDateTime.ToUniversalTime().ToString("yyyyMMdd'T'HHmm");  

 var services = Enum.GetValues(typeof(StorageService));  
 foreach (StorageService service in services)  
 {  
     Console.WriteLine("Minute Metrics for Service {0} from {1} to {2} UTC", service, start, end);  
     var metricsQuery = analyticsClient.CreateMinuteMetricsQuery(service, StorageLocation.Primary);  
     var t = analyticsClient.GetMinuteMetricsTable(service);  
     var opContext = new OperationContext();  
     var query =  
             from entity in metricsQuery  
             // Note, you can't filter using the entity properties Time, AccessType, or TransactionType  
             // because they are calculated fields in the MetricsEntity class.  
             // The PartitionKey identifies the DataTime of the metrics.  
             where entity.PartitionKey.CompareTo(start) >= 0 && entity.PartitionKey.CompareTo(end) <= 0   
             select entity;  

     // Filter on "user" transactions after fetching the metrics from Table Storage.  
     // (StartsWith is not supported using LINQ with Azure Table storage)  
     var results = query.ToList().Where(m => m.RowKey.StartsWith("user"));  
     var resultString = results.Aggregate(new StringBuilder(), (builder, metrics) => builder.AppendLine(MetricsString(metrics, opContext))).ToString();  
     Console.WriteLine(resultString);  
 }  
}  

private static string MetricsString(MetricsEntity entity, OperationContext opContext)  
{  
 var entityProperties = entity.WriteEntity(opContext);  
 var entityString =  
         string.Format("Time: {0}, ", entity.Time) +  
         string.Format("AccessType: {0}, ", entity.AccessType) +  
         string.Format("TransactionType: {0}, ", entity.TransactionType) +  
         string.Join(",", entityProperties.Select(e => new KeyValuePair<string, string>(e.Key.ToString(), e.Value.PropertyAsObject.ToString())));  
 return entityString;  
}  
```  

## <a name="billing-on-storage-metrics"></a>Выставление счетов на основе метрик хранилища  
За запросы записи на создание сущностей таблиц для метрик взимается плата в соответствии со стандартными тарифами, применимыми ко всем операциям службы хранилища Azure.  

Запросы на чтение и удаление данных метрик, клиент также являются оплачиваемыми по стандартным тарифам. Если вы настроили политику хранения данных, за удаление старых данных метрик хранилищем Azure плата не взимается. Однако при удалении данных аналитики с учетной записи будет взиматься плата за операции удаления.  

Пространство, занимаемое таблицами метрик, также тарифицируется. Чтобы оценить объем пространства, используемый для хранения данных метрик, можно использовать следующее:  

-   Если каждый час служба использует каждый API в каждой службе, то примерно 148 КБ данных сохраняется каждый час в таблицах транзакций метрик при наличии обоих службы API уровня и сводки.  
-   Если в каждый час служба использует каждый API в службе, а затем примерно 12 КБ данных сохраняется каждый час в таблицах транзакций метрик, если вы включили Сводка только уровень службы.  
-   В таблице емкости больших двоичных объектов содержит две строки, добавленные в день, указанное вам получить согласие на для журналов. Это означает, что каждый день, размер этой таблицы увеличивается примерно на 300 байтов.

## <a name="next-steps"></a>Дальнейшие действия
* [Мониторинг учетной записи хранения](https://www.windowsazure.com/manage/services/storage/how-to-monitor-a-storage-account/)   
* [Схема таблицы метрик аналитики хранилища](/rest/api/storageservices/storage-analytics-metrics-table-schema)   
* [Операции с протоколированием и сообщения о состоянии аналитик хранилища](/rest/api/storageservices/storage-analytics-logged-operations-and-status-messages)   
* [Ведение журнала аналитики службы хранилища](storage-analytics-logging.md)
