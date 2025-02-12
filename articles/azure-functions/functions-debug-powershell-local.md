---
title: Локальная отладка функций Azure PowerShell
description: Узнайте, как разрабатывать функции с помощью PowerShell.
services: functions
documentationcenter: na
author: tylerleonhardt
manager: jeconnoc
ms.service: azure-functions
ms.devlang: powershell
ms.topic: conceptual
ms.date: 04/22/2019
ms.author: tyleonha
ms.reviewer: glenga
ms.openlocfilehash: fc30a2efb21d5b7f3168d9229ec5baf9a7f05eb1
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/09/2019
ms.locfileid: "67706415"
---
# <a name="debug-powershell-azure-functions-locally"></a>Локальная отладка функций Azure PowerShell

Функции Azure предоставляют возможность разрабатывать функции как скрипты PowerShell.

[!INCLUDE [functions-powershell-preview-note](../../includes/functions-powershell-preview-note.md)]

Функции PowerShell можно отлаживать локально, как это делается, чтобы все скрипты PowerShell, используя следующие средства стандартной разработки:

* [Visual Studio Code](https://code.visualstudio.com/). Бесплатные, облегченные, открытый текстовый редактор корпорации Майкрософт с расширением PowerShell, предоставляет полные возможности разработки PowerShell.
* Консоль Windows PowerShell: Отладка, используя те же команды, который используется для отладки любого другого процесса PowerShell.

[Основные инструменты службы функции Azure](functions-run-local.md) поддерживает локальная отладка функций Azure, включая функции PowerShell.

## <a name="example-function-app"></a>Пример приложения-функции

Приложения-функции, используемые в этой статье содержит одной функции, активируемой HTTP и содержит следующие файлы:

```
PSFunctionApp
 | - HttpTriggerFunction
 | | - run.ps1
 | | - function.json
 | - local.settings.json
 | - host.json
 | - profile.ps1
```

Это приложение-функцию аналогична той, вы получите после завершения [быстрого запуска PowerShell](functions-create-first-function-powershell.md).

Код функции в `run.ps1` выглядит как следующий скрипт:

```powershell
param($Request)

$name = $Request.Query.Name

if($name) {
    $status = 200
    $body = "Hello $name"
}
else {
    $status = 400
    $body = "Please pass a name on the query string or in the request body."
}

Push-OutputBinding -Name Response -Value ([HttpResponseContext]@{
    StatusCode = $status
    Body = $body
})
```

## <a name="set-the-attach-point"></a>Задать точка подключения

Отладка любого функции PowerShell, функция должна остановить отладчик для присоединения. `Wait-Debugger` Командлет останавливает выполнение и ожидает отладчика.

Вам нужно лишь добавить вызов `Wait-Debugger` командлет непосредственно над `if` следующим образом:

```powershell
param($Request)

$name = $Request.Query.Name

# This is where we will wait for the debugger to attach
Wait-Debugger

if($name) {
    $status = 200
    $body = "Hello $name"
}
# ...
```

Отладка начинается с `if` инструкции. 

С помощью `Wait-Debugger` на месте, теперь можно отлаживать функции, используя Visual Studio Code или консоль Windows PowerShell.

## <a name="debug-in-visual-studio-code"></a>Отладка в Visual Studio Code

Для отладки функций PowerShell в Visual Studio Code, необходимо иметь следующие компоненты:

* [Расширения PowerShell для Visual Studio Code](/powershell/scripting/components/vscode/using-vscode)
* [Расширение "Функции Azure" для Visual Studio Code](functions-create-first-function-vs-code.md)
* [PowerShell Core 6.2 или более поздней версии](/powershell/scripting/install/installing-powershell-core-on-windows)

После установки этих зависимостей, загрузки существующего проекта функции PowerShell, или [создайте свой первый проект функции PowerShell](functions-create-first-function-powershell.md).

>[!NOTE]
> Проект не следует файлы необходимые настройки, будет предложено добавить их.

### <a name="set-the-powershell-version"></a>Задайте версию PowerShell

PowerShell Core устанавливается параллельно с Windows PowerShell. PowerShell Core в качестве версии PowerShell для использования с расширением PowerShell для Visual Studio Code.

1. Нажмите клавишу F1, чтобы отобразить палитру команд, а затем найдите `Session`.

1. Выберите **PowerShell: Показать меню сеанса**.

1. Если ваш **текущего сеанса** не **PowerShell Core 6**, выберите **переключиться в режим: PowerShell Core 6**.

При наличии файла откройте PowerShell, вы видите версию, показаны зеленым цветом в нижней правой части окна. Также при выборе этот текст отображается в меню сеанса. Дополнительные сведения см. в разделе [выбирается версия PowerShell для использования с расширением](/powershell/scripting/components/vscode/using-vscode#choosing-a-version-of-powershell-to-use-with-the-extension).

### <a name="start-the-function-app"></a>Запустите приложение-функция

Убедитесь, что `Wait-Debugger` задается в функции, где вы хотите присоединить отладчик.  С помощью `Wait-Debugger` добавлен, можно отлаживать приложение-функцию с помощью Visual Studio Code.

Выберите **Отладка** области и затем **присоединить к функции PowerShell**.

![Отладчик](https://user-images.githubusercontent.com/2644648/56166073-8a7b3780-5f89-11e9-85ce-36ed38e221a2.png)

Также можно нажать клавишу F5, чтобы начать отладку.

Запускайте процесс отладки операция выполняет следующие задачи:

* Запуски `func extensions install` в окне терминала, чтобы установить все расширения функций Azure, требуемые приложением-функцией.
* Запуски `func host start` в терминале для запуска приложения-функции в узле функций.
* Присоедините отладчик PowerShell пространство выполнения PowerShell в среде выполнения функций.

Приложения-функции под управлением необходимо отдельной консоли PowerShell для вызова функции, активируемой HTTP.

В этом случае консоль PowerShell является клиентом. `Invoke-RestMethod` Используется для активации функции.

В консоли PowerShell выполните следующую команду:

```powershell
Invoke-RestMethod "http://localhost:7071/api/HttpTrigger?Name=Functions"
```

Вы заметите, что ответ не возвращается немедленно. Это потому, что `Wait-Debugger` присоединил отладчика и PowerShell выполнения перешел в режим приостановки выполнения, как только он может. Это из-за [BreakAll концепция](#breakall-might-cause-your-debugger-to-break-in-an-unexpected-place), которое будет разъяснено позднее. После нажатия клавиши `continue` кнопки, отладчик теперь останавливается на строке непосредственно после `Wait-Debugger`.

На этом этапе подключен отладчик, и все обычного отладчика можно выполнить операции. Дополнительные сведения об использовании отладчика в Visual Studio Code см. в разделе [официальной документации](https://code.visualstudio.com/Docs/editor/debugging#_debug-actions).

После продолжения и полностью вызвать сценарий, вы заметите, что:

* В консоли PowerShell, который выполнял `Invoke-RestMethod` вернула результат
* Встроенная консоль PowerShell в Visual Studio Code ожидает скрипт, выполняемый

Позже при вызове той же функции, отладчик в расширении PowerShell разбивает непосредственно после `Wait-Debugger`.

## <a name="debugging-in-a-powershell-console"></a>Отладка в консоли PowerShell

>[!NOTE]
> В этом разделе предполагается, что вы прочитали [документация основных инструментов функций Azure](functions-run-local.md) и уметь использовать `func host start` команду для запуска приложения-функции.

Откройте консоль, `cd` в каталоге приложения-функции, и выполните следующую команду:

```sh
func host start
```

С помощью приложения-функции под управлением и `Wait-Debugger` на месте, можно присоединить к процессу. Вам нужен две дополнительные консоли PowerShell.

Одной из консолей действует в качестве клиента. Из этого, вызовите `Invoke-RestMethod` для запуска функции. Например можно выполнить следующую команду:

```powershell
Invoke-RestMethod "http://localhost:7071/api/HttpTrigger?Name=Functions"
```

Вы заметите, что он не возвращает ответ, который является результатом из `Wait-Debugger`. Пространство выполнения PowerShell теперь Ожидание подключения отладчика. Давайте, присоединена.

В других консоли PowerShell выполните следующую команду:

```powershell
Get-PSHostProcessInfo
```

Этот командлет возвращает таблицу, которая выглядит как следующий результат:

```output
ProcessName ProcessId AppDomainName
----------- --------- -------------
dotnet          49988 None
pwsh            43796 None
pwsh            49970 None
pwsh             3533 None
pwsh            79544 None
pwsh            34881 None
pwsh            32071 None
pwsh            88785 None
```

Запишите `ProcessId` для элемента в таблице с `ProcessName` как `dotnet`. Этот процесс является приложение-функцию.

Затем выполните следующий фрагмент кода:

```powershell
# This enters into the Azure Functions PowerShell process.
# Put your value of `ProcessId` here.
Enter-PSHostProcess -Id $ProcessId

# This triggers the debugger.
Debug-Runspace 1
```

После запуска отладчика и примерно следующие выходные данные:

```
Debugging Runspace: Runspace1

To end the debugging session type the 'Detach' command at the debugger prompt, or type 'Ctrl+C' otherwise.

At /Path/To/PSFunctionApp/HttpTriggerFunction/run.ps1:13 char:1
+ if($name) { ...
+ ~~~~~~~~~~~
[DBG]: [Process:49988]: [Runspace1]: PS /Path/To/PSFunctionApp>>
```

На этом этапе вы останавливаетесь в точке останова в [отладчик PowerShell](/powershell/module/microsoft.powershell.core/about/about_debuggers). На этой странице можно выполнять все операции обычные отладки, выполните шаг с обходом, шаг с заходом, по-прежнему, выйти из программы и другие. Чтобы просмотреть полный набор команд отладки, доступных в консоли, выполните `h` или `?` команды.

Можно также установить точки останова на этом уровне с `Set-PSBreakpoint` командлета.

После продолжения и полностью вызвать сценарий, вы заметите, что:

* В консоли PowerShell, в которой выполнялся `Invoke-RestMethod` вернула результат.
* В консоли PowerShell, в которой выполнялся `Debug-Runspace` ожидает скрипт для выполнения.

Ту же функцию можно вызвать снова (с помощью `Invoke-RestMethod` к примеру) и отладчик приостанавливает выполнение сразу после `Wait-Debugger` команды.

## <a name="considerations-for-debugging"></a>Соображения по отладке

Имейте в виду следующие проблемы при отладке кода своих функций.

### <a name="breakall-might-cause-your-debugger-to-break-in-an-unexpected-place"></a>`BreakAll` может вызвать для отладчика прерывание в неподходящем месте

В PowerShell используется расширение `Debug-Runspace`, который в свою очередь использует PowerShell `BreakAll` функции. Эта функция сообщает PowerShell для остановки в первой командой, выполняется. Это дает возможность задавать точки останова в пространство выполнения отладки.

Среда выполнения функций Azure выполняется несколько команд перед фактически для вызова вашей `run.ps1` сценария, так что вполне возможно, отладчик оказывается критические в `Microsoft.Azure.Functions.PowerShellWorker.psm1` или `Microsoft.Azure.Functions.PowerShellWorker.psd1`.

Должно происходить это нарушение, запустите `continue` или `c` команду, чтобы пропустить эту точку останова. Затем остановите в ожидаемые точки останова.

## <a name="next-steps"></a>Следующие шаги

Дополнительные сведения о разработке функций, с помощью PowerShell, см. в разделе [руководство разработчика функций Azure PowerShell](functions-reference-powershell.md).
