---
title: Мониторинг заданий Azure Stream Analytics и управление ими с помощью PowerShell
description: В этом статье объясняется, как с помощью Azure PowerShell и командлетов отслеживать задания Azure Stream Analytics и управлять ими.
services: stream-analytics
author: jseb225
ms.author: jeanb
manager: kfile
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 03/28/2017
ms.openlocfilehash: cdc09973a192924c5b9a81cd4ed49b9f36fc0eb1
ms.sourcegitcommit: 6a42dd4b746f3e6de69f7ad0107cc7ad654e39ae
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/07/2019
ms.locfileid: "67612299"
---
# <a name="monitor-and-manage-stream-analytics-jobs-with-azure-powershell-cmdlets"></a>Отслеживание заданий Stream Analytics и управление ими с помощью командлетов Azure PowerShell
Узнайте, как отслеживать ресурсы Stream Analytics и управлять ими с помощью командлетов Azure PowerShell и сценариев PowerShell, выполняющих базовые задания Stream Analytics.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="prerequisites-for-running-azure-powershell-cmdlets-for-stream-analytics"></a>Необходимые условия для запуска командлетов Azure PowerShell службы Stream Analytics
* Создайте группу ресурсов Azure в своей подписке. Ниже приведен пример сценария Azure PowerShell. Дополнительную информацию об Azure PowerShell см. в разделе [Установка и настройка Azure PowerShell](/powershell/azure/overview).  

Azure PowerShell 0.9.8:  

```powershell
# Log in to your Azure account
Add-AzureAccount
# Select the Azure subscription you want to use to create the resource group if you have more han one subscription on your account.
Select-AzureSubscription -SubscriptionName <subscription name>
# If Stream Analytics has not been registered to the subscription, remove remark symbol below (#)to run the Register-AzureProvider cmdlet to register the provider namespace.
#Register-AzureProvider -Force -ProviderNamespace 'Microsoft.StreamAnalytics'
# Create an Azure resource group
New-AzureResourceGroup -Name <YOUR RESOURCE GROUP NAME> -Location <LOCATION>
```

Azure PowerShell 1.0.

```powershell
# Log in to your Azure account
Connect-AzAccount
# Select the Azure subscription you want to use to create the resource group.
Get-AzSubscription –SubscriptionName "your sub" | Select-AzSubscription
# If Stream Analytics has not been registered to the subscription, remove remark symbol below (#)to run the Register-AzureProvider cmdlet to register the provider namespace.
#Register-AzResourceProvider -Force -ProviderNamespace 'Microsoft.StreamAnalytics'
# Create an Azure resource group
New-AzResourceGroup -Name <YOUR RESOURCE GROUP NAME> -Location <LOCATION>
```


> [!NOTE]
> Отслеживание заданий Stream Analytics, созданных программным путем, по умолчанию отключено.  Вы можете вручную включить отслеживание на портале Azure. Для этого перейдите на страницу "Отслеживание" задания и нажмите кнопку "Включить". Это также можно сделать программным путем, выполнив действия, приведенные в статье [Создание монитора заданий Stream Analytics программным способом](stream-analytics-monitor-jobs.md).
> 
> 

## <a name="azure-powershell-cmdlets-for-stream-analytics"></a>Командлеты Azure PowerShell для службы Stream Analytics
Следующие командлеты Azure PowerShell можно использовать для отслеживания заданий Azure Stream Analytics и управления ими. Обратите внимание, что Azure PowerShell имеет различные версии. 
**В приведенных примерах первая команда приведена для Azure PowerShell 0.9.8, вторая — для Azure PowerShell 1.0.** Команды Azure PowerShell 1.0 всегда будет содержать «Az» в команде.

### <a name="get-azurestreamanalyticsjob--get-azstreamanalyticsjob"></a>Get-AzureStreamAnalyticsJob | Get-AzStreamAnalyticsJob
Выводит список всех заданий Stream Analytics, определенных в подписке Azure или указанной группе ресурсов, или показывает сведения о конкретном задании в группе ресурсов.

**Пример 1**

Azure PowerShell 0.9.8:  

```powershell
Get-AzureStreamAnalyticsJob
```

Azure PowerShell 1.0.  

```powershell
Get-AzStreamAnalyticsJob
```

Эта команда PowerShell возвращает сведения обо всех заданиях Stream Analytics в подписке Azure.

**Пример 2**

Azure PowerShell 0.9.8:  

```powershell
Get-AzureStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US 
```

Azure PowerShell 1.0.  

```powershell
Get-AzStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US 
```

Эта команда PowerShell возвращает сведения о всех заданиях Stream Analytics в группе ресурсов StreamAnalytics-Default-Central-US.

**Пример 3**

Azure PowerShell 0.9.8:  

```powershell
Get-AzureStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US -Name StreamingJob
```

Azure PowerShell 1.0.  

```powershell
Get-AzStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US -Name StreamingJob
```

Эта команда PowerShell возвращает сведения о задании Stream Analytics StreamingJob в группе ресурсов StreamAnalytics-Default-Central-US.

### <a name="get-azurestreamanalyticsinput--get-azstreamanalyticsinput"></a>Get-AzureStreamAnalyticsInput | Get-AzStreamAnalyticsInput
Выводит список всех входных данных, определенных в указанном задании Stream Analytics, или показывает сведения о конкретных данных.

**Пример 1**

Azure PowerShell 0.9.8:  

```powershell
Get-AzureStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob
```

Azure PowerShell 1.0.  

```powershell
Get-AzStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob
```

Эта команда PowerShell возвращает сведения о всех входных данных, определенных в задании StreamingJob.

**Пример 2**

Azure PowerShell 0.9.8:  

```powershell
Get-AzureStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –Name EntryStream
```

Azure PowerShell 1.0.  

```powershell
Get-AzStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –Name EntryStream
```

Эта команда PowerShell возвращает сведения о входных данных EntryStream, определенных в задании StreamingJob.

### <a name="get-azurestreamanalyticsoutput--get-azstreamanalyticsoutput"></a>Get-AzureStreamAnalyticsOutput | Get-AzStreamAnalyticsOutput
Выводит список всех выходных данных, определенных в указанном задании Stream Analytics, или показывает сведения о конкретных данных.

**Пример 1**

Azure PowerShell 0.9.8:  

```powershell
Get-AzureStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob
```

Azure PowerShell 1.0.  

```powershell
Get-AzStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob
```

Эта команда PowerShell возвращает сведения о выходных данных, определенных в задании StreamingJob.

**Пример 2**

Azure PowerShell 0.9.8:  

```powershell
Get-AzureStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –Name Output
```

Azure PowerShell 1.0.  

```powershell
Get-AzStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –Name Output
```

Эта команда PowerShell возвращает сведения о выходных данных Output, определенных в задании StreamingJob.

### <a name="get-azurestreamanalyticsquota--get-azstreamanalyticsquota"></a>Get-AzureStreamAnalyticsQuota | Get-AzStreamAnalyticsQuota
Возвращает сведения о квоте единиц потоковой передачи в указанном регионе.

**Пример 1**

Azure PowerShell 0.9.8:  

```powershell
Get-AzureStreamAnalyticsQuota –Location "Central US" 
```

Azure PowerShell 1.0.  

```powershell
Get-AzStreamAnalyticsQuota –Location "Central US" 
```

Эта команда PowerShell возвращает сведения о квоте и использовании единиц потоковой передачи в центральном регионе США.

### <a name="get-azurestreamanalyticstransformation--get-azstreamanalyticstransformation"></a>Get-AzureStreamAnalyticsTransformation | Get-AzStreamAnalyticsTransformation
Возвращает сведения о конкретном преобразовании, определенном в задании Stream Analytics.

**Пример 1**

Azure PowerShell 0.9.8:  

```powershell
Get-AzureStreamAnalyticsTransformation -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –Name StreamingJob
```

Azure PowerShell 1.0.  

```powershell
Get-AzStreamAnalyticsTransformation -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –Name StreamingJob
```

Эта команда PowerShell возвращает сведения о преобразовании StreamingJob в задании StreamingJob.

### <a name="new-azurestreamanalyticsinput--new-azstreamanalyticsinput"></a>Новый AzureStreamAnalyticsInput | Новый AzStreamAnalyticsInput
Создает новые или обновляет существующие входные данные в задании Stream Analytics.

Имя входных данных можно указать в JSON-файле или в командной строке. Если указаны оба, имя в командной строке должно совпадать с именем в файле.

Если указаны существующие входные данные и не задан параметр –Force, командлет предложит заменить существующие входные данные.

Если указать параметр –Force и существующее имя входных данных, входные данные будут заменены без подтверждения.

Подробные сведения о файле структуре и содержимом JSON см. [Создание входных данных (Azure Stream Analytics)][msdn-rest-api-create-stream-analytics-input] section of the [Stream Analytics Management REST API Reference Library][stream.analytics.rest.api.reference].

**Пример 1**

Azure PowerShell 0.9.8:  

```powershell
New-AzureStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –File "C:\Input.json" 
```

Azure PowerShell 1.0.  

```powershell
New-AzStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –File "C:\Input.json" 
```

Эта команда PowerShell создает новые входные данные из файла Input.json. Если существующие входные данные с именем, указанным во входном файле определения, уже определены, командлет предложит их заменить.

**Пример 2**

Azure PowerShell 0.9.8:  

```powershell
New-AzureStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –File "C:\Input.json" –Name EntryStream
```

Azure PowerShell 1.0.  

```powershell
New-AzStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –File "C:\Input.json" –Name EntryStream
```

Эта команда PowerShell создает новые входные данные в задании EntryStream. Если существующие входные данные с таким именем уже определены, командлет предложит их заменить.

**Пример 3**

Azure PowerShell 0.9.8:  

```powershell
New-AzureStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –File "C:\Input.json" –Name EntryStream -Force
```

Azure PowerShell 1.0.  

```powershell
New-AzStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –File "C:\Input.json" –Name EntryStream -Force
```

Эта команда PowerShell заменяет определение существующего источника входных данных EntryStream на определение из файла.

### <a name="new-azurestreamanalyticsjob--new-azstreamanalyticsjob"></a>New-AzureStreamAnalyticsJob | New-AzStreamAnalyticsJob
Создает задание Stream Analytics в Microsoft Azure или обновляет определение существующего задания.

Имя задания можно указать в JSON-файле или в командной строке. Если указаны оба, имя в командной строке должно совпадать с именем в файле.

Если указано существующее имя задания и не указан параметр –Force, командлет предложит заменить существующее задание.

Если указать параметр –Force и существующее имя задания, определение задания будет заменено без подтверждения.

Подробные сведения о файле структуре и содержимом JSON см. [создать задание Stream Analytics][msdn-rest-api-create-stream-analytics-job] section of the [Stream Analytics Management REST API Reference Library][stream.analytics.rest.api.reference].

**Пример 1**

Azure PowerShell 0.9.8:  

```powershell
New-AzureStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\JobDefinition.json" 
```

Azure PowerShell 1.0.  

```powershell
New-AzStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\JobDefinition.json" 
```

Эта команда PowerShell создает новое задание из определения в JobDefinition.json. Если существующее задание с именем, указанным в файле определения задания, уже определено, командлет предложит его заменить.

**Пример 2**

Azure PowerShell 0.9.8:  

```powershell
New-AzureStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\JobDefinition.json" –Name StreamingJob -Force
```

Azure PowerShell 1.0.  

```powershell
New-AzStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\JobDefinition.json" –Name StreamingJob -Force
```

Эта команда PowerShell заменяет определение задания StreamingJob.

### <a name="new-azurestreamanalyticsoutput--new-azstreamanalyticsoutput"></a>Новый AzureStreamAnalyticsOutput | Новый AzStreamAnalyticsOutput
Создает новые или обновляет существующие выходные данные в задании Stream Analytics.  

Имя выходных данных можно указать в JSON-файле или в командной строке. Если указаны оба, имя в командной строке должно совпадать с именем в файле.

Если указаны существующие выходные данные и не задан параметр –Force, командлет предложит заменить существующие выходные данные.

Если указать параметр –Force и существующее имя выходных данных, выходные данные будут заменены без подтверждения.

Подробные сведения о файле структуре и содержимом JSON см. [создание выходных данных (Azure Stream Analytics)][msdn-rest-api-create-stream-analytics-output] section of the [Stream Analytics Management REST API Reference Library][stream.analytics.rest.api.reference].

**Пример 1**

Azure PowerShell 0.9.8:  

```powershell
New-AzureStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Output.json" –JobName StreamingJob –Name output
```

Azure PowerShell 1.0.  

```powershell
New-AzStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Output.json" –JobName StreamingJob –Name output
```

Эта команда PowerShell создает новые выходные данные output в задании StreamingJob. Если существующие выходные данные с таким именем уже определены, командлет предложит их заменить.

**Пример 2**

Azure PowerShell 0.9.8:  

```powershell
New-AzureStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Output.json" –JobName StreamingJob –Name output -Force
```

Azure PowerShell 1.0.  

```powershell
New-AzStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Output.json" –JobName StreamingJob –Name output -Force
```

Эта команда PowerShell заменяет определение для output в задании StreamingJob.

### <a name="new-azurestreamanalyticstransformation--new-azstreamanalyticstransformation"></a>New-AzureStreamAnalyticsTransformation | New-AzStreamAnalyticsTransformation
Создает новое или обновляет существующее преобразование в задании Stream Analytics.

Имя преобразования можно указано в JSON-файле или в командной строке. Если указаны оба, имя в командной строке должно совпадать с именем в файле.

Если указано существующее преобразование и не задан параметр –Force, командлет предложит заменить существующее преобразование.

Если указать параметр –Force и существующее имя преобразования, преобразование будет заменено без подтверждения.

Подробные сведения о файле структуре и содержимом JSON см. [Создание трансформации (Azure Stream Analytics)][msdn-rest-api-create-stream-analytics-transformation] section of the [Stream Analytics Management REST API Reference Library][stream.analytics.rest.api.reference].

**Пример 1**

Azure PowerShell 0.9.8:  

```powershell
New-AzureStreamAnalyticsTransformation -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Transformation.json" –JobName StreamingJob –Name StreamingJobTransform
```

Azure PowerShell 1.0.  

```powershell
New-AzStreamAnalyticsTransformation -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Transformation.json" –JobName StreamingJob –Name StreamingJobTransform
```

Эта команда PowerShell создает новое преобразование StreamingJobTransform в задании StreamingJob. Если существующее преобразование с таким именем уже определено, командлет предложит его заменить.

**Пример 2**

Azure PowerShell 0.9.8:  

```powershell
New-AzureStreamAnalyticsTransformation -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Transformation.json" –JobName StreamingJob –Name StreamingJobTransform -Force
```

Azure PowerShell 1.0.  

```powershell
New-AzStreamAnalyticsTransformation -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Transformation.json" –JobName StreamingJob –Name StreamingJobTransform -Force
```

 Эта команда PowerShell заменяет определение StreamingJobTransform в задании StreamingJob.

### <a name="remove-azurestreamanalyticsinput--remove-azstreamanalyticsinput"></a>Remove-AzureStreamAnalyticsInput | Remove-AzStreamAnalyticsInput
Асинхронно удаляет указанные входные данные из задания Stream Analytics в Microsoft Azure.  
Если указать параметр –Force, входные данные будут удалены без подтверждения.

**Пример 1**

Azure PowerShell 0.9.8:  

```powershell
Remove-AzureStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name EventStream
```

Azure PowerShell 1.0.  

```powershell
Remove-AzStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name EventStream
```

Эта команда PowerShell удаляет входные данные EventStream в задании StreamingJob.  

### <a name="remove-azurestreamanalyticsjob--remove-azstreamanalyticsjob"></a>Remove-AzureStreamAnalyticsJob | Remove-AzStreamAnalyticsJob
Асинхронно удаляет указанное задание Stream Analytics в Microsoft Azure.  
Если указать параметр –Force, задание будет удалено без подтверждения.

**Пример 1**

Azure PowerShell 0.9.8:  

```powershell
Remove-AzureStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –Name StreamingJob 
```

Azure PowerShell 1.0.  

```powershell
Remove-AzStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –Name StreamingJob 
```

Эта команда PowerShell удаляет задание StreamingJob.  

### <a name="remove-azurestreamanalyticsoutput--remove-azstreamanalyticsoutput"></a>Remove-AzureStreamAnalyticsOutput | Remove-AzStreamAnalyticsOutput
Асинхронно удаляет указанные выходные данные из задания Stream Analytics в Microsoft Azure.  
Если указать параметр –Force, выходные данные будут удалены без подтверждения.

**Пример 1**

Azure PowerShell 0.9.8:  

```powershell
Remove-AzureStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name Output
```

Azure PowerShell 1.0.  

```powershell
Remove-AzStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name Output
```

Эта команда PowerShell удаляет выходные данные Output в задании StreamingJob.  

### <a name="start-azurestreamanalyticsjob--start-azstreamanalyticsjob"></a>Start-AzureStreamAnalyticsJob | Start-AzStreamAnalyticsJob
Асинхронно развертывает и запускает задание Stream Analytics в Microsoft Azure.

**Пример 1**

Azure PowerShell 0.9.8:  

```powershell
Start-AzureStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US -Name StreamingJob -OutputStartMode CustomTime -OutputStartTime 2012-12-12T12:12:12Z
```

Azure PowerShell 1.0.  

```powershell
Start-AzStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US -Name StreamingJob -OutputStartMode CustomTime -OutputStartTime 2012-12-12T12:12:12Z
```

Это команда PowerShell запускает задание StreamingJob с пользовательским временем запуска выходных данных «12 декабря 2012 г., 12:12:12 UTC».

### <a name="stop-azurestreamanalyticsjob--stop-azstreamanalyticsjob"></a>Stop-AzureStreamAnalyticsJob | Stop-AzStreamAnalyticsJob
Асинхронно останавливает задание Stream Analytics в Microsoft Azure и освобождает используемые ресурсы. Определение задания и метаданные остаются доступны в подписке через портал Azure и интерфейсы API управления, поэтому задание всегда можно изменить и перезапустить. Вы не платите за задание в состоянии "Остановлено".

**Пример 1**

Azure PowerShell 0.9.8:  

```powershell
Stop-AzureStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –Name StreamingJob 
```

Azure PowerShell 1.0.  

```powershell
Stop-AzStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –Name StreamingJob 
```

Эта команда PowerShell останавливает задание StreamingJob.  

### <a name="test-azurestreamanalyticsinput--test-azstreamanalyticsinput"></a>Тест AzureStreamAnalyticsInput | AzStreamAnalyticsInput теста
Проверяет возможность подключения Stream Analytics к указанным входным данным.

**Пример 1**

Azure PowerShell 0.9.8:  

```powershell
Test-AzureStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name EntryStream
```

Azure PowerShell 1.0.  

```powershell
Test-AzStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name EntryStream
```

Эта команда PowerShell проверяет состояние подключения входных данных EntryStream в StreamingJob.  

### <a name="test-azurestreamanalyticsoutput--test-azstreamanalyticsoutput"></a>Тест AzureStreamAnalyticsOutput | AzStreamAnalyticsOutput теста
Проверяет возможность подключения Stream Analytics к указанным выходным данным.

**Пример 1**

Azure PowerShell 0.9.8:  

```powershell
Test-AzureStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name Output
```

Azure PowerShell 1.0.  

```powershell
Test-AzStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name Output
```

Эта команда PowerShell проверяет состояние подключения выходных данных Output в StreamingJob.  

## <a name="get-support"></a>Получение поддержки
За дополнительной помощью обращайтесь на наш [форум Azure Stream Analytics](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureStreamAnalytics). 

## <a name="next-steps"></a>Дальнейшие действия
* [Введение в Azure Stream Analytics](stream-analytics-introduction.md)
* [Приступая к работе с Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Масштабирование заданий в службе Azure Stream Analytics](stream-analytics-scale-jobs.md)
* [Справочник по языку запросов Azure Stream Analytics](https://docs.microsoft.com/stream-analytics-query/stream-analytics-query-language-reference)
* [Справочник по API-интерфейсу REST управления Stream Analytics](https://msdn.microsoft.com/library/azure/dn835031.aspx)

[msdn-switch-azuremode]: https://msdn.microsoft.com/library/dn722470.aspx
[powershell-install]: https://azure.microsoft.com/documentation/articles/powershell-install-configure/
[msdn-rest-api-create-stream-analytics-job]: https://msdn.microsoft.com/library/dn834994.aspx
[msdn-rest-api-create-stream-analytics-input]: https://msdn.microsoft.com/library/dn835010.aspx
[msdn-rest-api-create-stream-analytics-output]: https://msdn.microsoft.com/library/dn835015.aspx
[msdn-rest-api-create-stream-analytics-transformation]: https://msdn.microsoft.com/library/dn835007.aspx

[stream.analytics.introduction]: stream-analytics-introduction.md
[stream.analytics.get.started]: stream-analytics-real-time-fraud-detection.md
[stream.analytics.developer.guide]: ../stream-analytics-developer-guide.md
[stream.analytics.scale.jobs]: stream-analytics-scale-jobs.md
[stream.analytics.query.language.reference]: https://go.microsoft.com/fwlink/?LinkID=513299
[stream.analytics.rest.api.reference]: https://go.microsoft.com/fwlink/?LinkId=517301

