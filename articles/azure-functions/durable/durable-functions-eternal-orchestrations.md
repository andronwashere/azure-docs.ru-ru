---
title: Нескончаемые оркестрации в устойчивых функциях — Azure
description: Узнайте, как реализовать нескончаемые оркестрации с помощью расширения устойчивых функций для Функций Azure.
services: functions
author: ggailey777
manager: jeconnoc
keywords: ''
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 12/07/2018
ms.author: azfuncdf
ms.openlocfilehash: 99eabf3bc91887ff19b3a0bc9cf6647d32fa6750
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "65787566"
---
# <a name="eternal-orchestrations-in-durable-functions-azure-functions"></a>Нескончаемые оркестрации в устойчивых функциях (Функции Azure)

*Нескончаемые оркестрации* — это функции оркестрации, которые никогда не останавливаются. Они позволяют создать [устойчивые функции](durable-functions-overview.md) для агрегаторов или в любых других ситуациях, когда нужен бесконечный цикл.

## <a name="orchestration-history"></a>Журнал оркестраций

Как описано в статье [Checkpointing and Replay](durable-functions-checkpointing-and-replay.md) (Назначение контрольных точек и воспроизведение), платформа устойчивых задач сохраняет журнал оркестраций для каждой функции. Этот журнал постоянно растет, пока функция оркестрации продолжает планировать новые работы. Если функция оркестрации уходит в бесконечный цикл и продолжает планировать работы, этот журнал может достигнуть критического размера, что приведет к существенным проблемам с производительностью. Для устранения таких проблем в приложениях с бесконечными циклами была разработана концепция *нескончаемой оркестрации*.

## <a name="resetting-and-restarting"></a>Сброс и перезапуск

Вместо бесконечных циклов функции оркестрации сбрасывают состояние, вызывая метод [ContinueAsNew](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html#Microsoft_Azure_WebJobs_DurableOrchestrationContext_ContinueAsNew_). Этот метод принимает один параметр в формате JSON, который передает входные данные в следующие поколение функции оркестрации.

При вызове `ContinueAsNew` экземпляр помещает в очередь сообщение для себя и завершает выполнение. Это сообщение приводит к повторному запуску экземпляра с новыми входными значениями. Функция оркестратора сохраняет тот же идентификатор экземпляра, но журнал для нее обнуляется.

> [!NOTE]
> Для функции оркестрации, которая выполняет сброс с помощью `ContinueAsNew`, платформа устойчивых задач сохраняет идентификатор экземпляра, но создает новый внутренний *идентификатор выполнения*. Обычно этот идентификатор выполнения не передается наружу, но его можно использовать при отладке выполнения оркестрации.

## <a name="periodic-work-example"></a>Пример периодической работы

Нескончаемые оркестрации могут быть полезны, например, в коде, который должен неограниченно долго выполнять периодические работы.

### <a name="c"></a>C#

```csharp
[FunctionName("Periodic_Cleanup_Loop")]
public static async Task Run(
    [OrchestrationTrigger] DurableOrchestrationContext context)
{
    await context.CallActivityAsync("DoCleanup");

    // sleep for one hour between cleanups
    DateTime nextCleanup = context.CurrentUtcDateTime.AddHours(1);
    await context.CreateTimer(nextCleanup, CancellationToken.None);

    context.ContinueAsNew(null);
}
```

### <a name="javascript-functions-2x-only"></a>JavaScript (только для решения "Функции" версии 2.x)

```javascript
const df = require("durable-functions");
const moment = require("moment");

module.exports = df.orchestrator(function*(context) {
    yield context.df.callActivity("DoCleanup");

    // sleep for one hour between cleanups
    const nextCleanup = moment.utc(context.df.currentUtcDateTime).add(1, "h");
    yield context.df.createTimer(nextCleanup.toDate());

    context.df.continueAsNew(undefined);
});
```

Разница между этим примером и запуском функции по таймеру заключается в том, что триггер очистки здесь можно не привязывать к расписанию. Например, если функция выполняется каждый час по расписанию CRON, она будет запущена в 1:00, 2:00, 3:00 и т.д., что может привести к проблемам наложения. А в нашем варианте, если очистка продолжается 30 минут, функция будет выполняться в 1:00, 2:30, 4:00, т. д., что позволяет избежать наложения.

## <a name="exit-from-an-eternal-orchestration"></a>Выход из нескончаемой оркестрации

Если потребуется завершить функцию оркестрации, достаточно лишь *не* вызывать метод `ContinueAsNew` и дождаться обычного выхода из функции.

Если функция оркестрации, которую нужно остановить, находится в бесконечном цикле, используйте для ее остановки метод [TerminateAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_TerminateAsync_). Дополнительные сведения см. в [статье об управлении экземплярами](durable-functions-instance-management.md).

## <a name="next-steps"></a>Дальнейшие действия

> [!div class="nextstepaction"]
> [Узнайте, как реализовать одноэкземплярные оркестрации](durable-functions-singletons.md)
