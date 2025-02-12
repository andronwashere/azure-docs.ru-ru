---
title: Вложенные оркестрации в устойчивых функциях — Azure
description: Узнайте, как вызывать оркестрации из оркестраций в расширении устойчивых функций для Функций Azure.
services: functions
author: ggailey777
manager: jeconnoc
keywords: ''
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 12/07/2018
ms.author: azfuncdf
ms.openlocfilehash: 1ab9a5714a7ef24b51957bd48b1b67240cf13adb
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "60730248"
---
# <a name="sub-orchestrations-in-durable-functions-azure-functions"></a>Вложенные оркестрации в устойчивых функциях (Функции Azure)

Кроме вызова функций действия, функции оркестратора могут вызывать другие функции оркестратора. Например, вы можете построить создать крупную оркестрацию из библиотеки функций оркестратора. Или можно запустить несколько экземпляров функции оркестратора одновременно.

Функция оркестратора может вызывать другую функцию оркестратора в ходе вызова методов [CallSubOrchestratorAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html#Microsoft_Azure_WebJobs_DurableOrchestrationContext_CallSubOrchestratorAsync_) или [CallSubOrchestratorWithRetryAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html#Microsoft_Azure_WebJobs_DurableOrchestrationContext_CallSubOrchestratorWithRetryAsync_) (.NET) и `callSubOrchestrator` или `callSubOrchestratorWithRetry` (JavaScript). Дополнительные сведения об автоматических повторных попытках см. в руководстве по [обработке ошибок и компенсации](durable-functions-error-handling.md#automatic-retry-on-failure).

Для вызывающего функции вложенного оркестратора и функции действия ведут себя одинаково. Они возвращают значение, выдают исключение и могут ожидаться родительской функцией оркестратора.

## <a name="example"></a>Пример

В следующем примере показан сценарий использования Интернета вещей с несколькими устройствами, которые нужно подготовить. Для каждого из этих устройств требуется определенная оркестрация. Это может выглядеть так:

### <a name="c"></a>C#

```csharp
public static async Task DeviceProvisioningOrchestration(
    [OrchestrationTrigger] DurableOrchestrationContext context)
{
    string deviceId = context.GetInput<string>();

    // Step 1: Create an installation package in blob storage and return a SAS URL.
    Uri sasUrl = await context.CallActivityAsync<Uri>("CreateInstallationPackage", deviceId);

    // Step 2: Notify the device that the installation package is ready.
    await context.CallActivityAsync("SendPackageUrlToDevice", Tuple.Create(deviceId, sasUrl));

    // Step 3: Wait for the device to acknowledge that it has downloaded the new package.
    await context.WaitForExternalEvent<bool>("DownloadCompletedAck");

    // Step 4: ...
}
```

### <a name="javascript-functions-2x-only"></a>JavaScript (только для решения "Функции" версии 2.x)

```javascript
const df = require("durable-functions");

module.exports = df.orchestrator(function*(context) {
    const deviceId = context.df.getInput();

    // Step 1: Create an installation package in blob storage and return a SAS URL.
    const sasUrl = yield context.df.callActivity("CreateInstallationPackage", deviceId);

    // Step 2: Notify the device that the installation package is ready.
    yield context.df.callActivity("SendPackageUrlToDevice", { id: deviceId, url: sasUrl });

    // Step 3: Wait for the device to acknowledge that it has downloaded the new package.
    yield context.df.waitForExternalEvent("DownloadCompletedAck");

    // Step 4: ...
});
```

Эту функцию оркестратора можно использовать как есть — для однократной подготовки устройства — или же как часть более крупной оркестрации. В последнем случае родительская функция оркестратора может планировать экземпляры `DeviceProvisioningOrchestration` с помощью API `CallSubOrchestratorAsync` (C#) или `callSubOrchestrator` (JavaScript).

В примере ниже показано, как реализовать параллельное выполнение нескольких функций оркестратора.

### <a name="c"></a>C#

```csharp
[FunctionName("ProvisionNewDevices")]
public static async Task ProvisionNewDevices(
    [OrchestrationTrigger] DurableOrchestrationContext context)
{
    string[] deviceIds = await context.CallActivityAsync<string[]>("GetNewDeviceIds");

    // Run multiple device provisioning flows in parallel
    var provisioningTasks = new List<Task>();
    foreach (string deviceId in deviceIds)
    {
        Task provisionTask = context.CallSubOrchestratorAsync("DeviceProvisioningOrchestration", deviceId);
        provisioningTasks.Add(provisionTask);
    }

    await Task.WhenAll(provisioningTasks);

    // ...
}
```

### <a name="javascript-functions-2x-only"></a>JavaScript (только для решения "Функции" версии 2.x)

```javascript
const df = require("durable-functions");

module.exports = df.orchestrator(function*(context) {
    const deviceIds = yield context.df.callActivity("GetNewDeviceIds");

    // Run multiple device provisioning flows in parallel
    const provisioningTasks = [];
    for (const deviceId of deviceIds) {
        const provisionTask = context.df.callSubOrchestrator("DeviceProvisioningOrchestration", deviceId);
        provisioningTasks.push(provisionTask);
    }

    yield context.df.Task.all(provisioningTasks);

    // ...
});
```

## <a name="next-steps"></a>Дальнейшие действия

> [!div class="nextstepaction"]
> [Узнайте, что такое концентраторы задач и как их настраивать](durable-functions-task-hubs.md)
