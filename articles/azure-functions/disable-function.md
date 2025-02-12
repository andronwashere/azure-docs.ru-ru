---
title: Способы отключения функций в решении "Функции Azure"
description: Узнайте, как отключать и включать функции в решении "Функции Azure" версии 1.x и 2.x.
services: functions
documentationcenter: ''
author: ggailey777
manager: jeconnoc
ms.service: azure-functions
ms.topic: conceptual
ms.date: 07/24/2018
ms.author: glenga
ms.openlocfilehash: a32b4815a2716428ceeec034ddc5589e3aa062e8
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "60710579"
---
# <a name="how-to-disable-functions-in-azure-functions"></a>Способы отключения функций в решении "Функции Azure"

В этой статье объясняется, как отключить функцию в решении "Функции Azure". *Отключить* функцию означает сделать так, чтобы среда выполнения игнорировала автоматический триггер, который определен для функции. Способ, с помощью которого вы это сделаете, зависит от версии среды выполнения и языка:

* Функции 1.x
  * Языки сценариев.
  * Библиотеки классов C#.
* Функции 2.x
  * Один способ для всех языков.
  * Дополнительный способ для библиотек классов C#.

## <a name="functions-1x---scripting-languages"></a>Функции 1.x — языки сценариев

Для языков сценариев, таких как сценарий C# и JavaScript, используйте свойство `disabled` файла *function.json*, чтобы сообщить среде выполнения не активировать функцию. Этому свойству можно задать значение `true` или имя параметра приложения:

```json
{
    "bindings": [
        {
            "type": "queueTrigger",
            "direction": "in",
            "name": "myQueueItem",
            "queueName": "myqueue-items",
            "connection":"MyStorageConnectionAppSetting"
        }
    ],
    "disabled": true
}
```
или 

```json
    "bindings": [
        ...
    ],
    "disabled": "IS_DISABLED"
```

Во втором примере функция отключается, если имеется параметр приложения с именем IS_DISABLED и ему задано значение `true` или 1.

Вы можете изменить файл на портале Azure или использовать переключатель **состояния функции** на вкладке **Управление** для функции. Переключатель на портале работает путем изменения файла *function.json*.

![Переключатель состояния функции](media/disable-function/function-state-switch.png)

## <a name="functions-1x---c-class-libraries"></a>Функции 1.x — библиотеки классов C#

В библиотеке классов Функций 1.x используйте атрибут `Disable` для предотвращения активации функции. Можно использовать атрибут без параметра конструктора, как показано в следующем примере:

```csharp
public static class QueueFunctions
{
    [Disable]
    [FunctionName("QueueTrigger")]
    public static void QueueTrigger(
        [QueueTrigger("myqueue-items")] string myQueueItem, 
        TraceWriter log)
    {
        log.Info($"C# function processed: {myQueueItem}");
    }
}
```

Если используется атрибут без параметра конструктора, необходимо перекомпилировать и повторно развернуть проект, чтобы изменить отключенное состояние функции. Более гибкий способ использования атрибута — включить параметр конструктора, который ссылается на логический параметр приложение, как показано в следующем примере:

```csharp
public static class QueueFunctions
{
    [Disable("MY_TIMER_DISABLED")]
    [FunctionName("QueueTrigger")]
    public static void QueueTrigger(
        [QueueTrigger("myqueue-items")] string myQueueItem, 
        TraceWriter log)
    {
        log.Info($"C# function processed: {myQueueItem}");
    }
}
```

Этот метод позволяет включать и отключать функцию, изменив параметр приложения без повторной компиляции или повторного развертывания. Изменение параметра приложения вызывает перезапуск приложения-функции, поэтому изменение отключенного состоянии сразу же распознается.

> [!IMPORTANT]
> Атрибут `Disabled` является единственным способом отключить функцию библиотеки классов. Созданный файл *function.json* для функции библиотеки классов не предназначен для непосредственного редактирования. Во время редактирования этого файла любые действия со свойством `disabled` не окажут никакого влияния.
>
> То же самое касается переключателя **состояния функции** на вкладке **Управление**, так как он работает, изменяя файл *function.json*.
>
> Обратите также внимание, что портал может указывать, что функция отключена, когда это не так.



## <a name="functions-2x---all-languages"></a>Функции 2.x — все языки

В Функциях версии 2.x отключите функцию с помощью параметра приложения. Например, чтобы отключить функцию с именем `QueueTrigger`, создайте параметр приложения `AzureWebJobs.QueueTrigger.Disabled` и присвойте ему значение `true`. Чтобы включить функцию, задайте параметру приложения значение `false`. Можно также использовать переключатель **состояния функции** на вкладке **Управление** для функции. Переключатель работает путем создания и удаления параметра приложения `AzureWebJobs.<functionname>.Disabled`.

![Переключатель состояния функции](media/disable-function/function-state-switch.png)

## <a name="functions-2x---c-class-libraries"></a>Функции 2.x — библиотеки классов C#

В библиотеке классов Функций 2.x рекомендуется использовать метод, который работает для всех языков. Но при желании вы можете [использовать атрибут Disable, как в Функциях 1.x](#functions-1x---c-class-libraries).

## <a name="next-steps"></a>Дальнейшие действия

Эта статья посвящена отключению автоматических триггеров. Дополнительные сведения о триггерах см. в статье [Основные понятия триггеров и привязок в Функциях Azure](functions-triggers-bindings.md).
