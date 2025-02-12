---
title: Сценарии развертывания и объединения в устойчивых функциях — Azure
description: Сведения о том, как реализовать сценарии развертывания и объединения в расширении устойчивых функций для Функций Azure.
services: functions
author: ggailey777
manager: jeconnoc
keywords: ''
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 12/07/2018
ms.author: azfuncdf
ms.openlocfilehash: 0bef5f1b64ec9f322070ba5c36cab138c7327da2
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "60741279"
---
# <a name="fan-outfan-in-scenario-in-durable-functions---cloud-backup-example"></a>Сценарии развертывания и объединения в устойчивых функциях. Пример резервного копирования в облако

*Развертывание и объединение* — это шаблон параллельного выполнения нескольких функций с последующим статистическим вычислением результатов. В этой статье описывается пример, использующий [устойчивые функции](durable-functions-overview.md) для реализации сценариев развертывания и объединения. Образец представляет устойчивую функцию, создающую резервную копию всего или некоторого содержимого сайта приложения в службе хранилища Azure.

[!INCLUDE [durable-functions-prerequisites](../../../includes/durable-functions-prerequisites.md)]

## <a name="scenario-overview"></a>Обзор сценария

В этом образце функции рекурсивно отправляют все файлы в указанном каталоге в хранилище BLOB-объектов и подсчитывают общее число отправленных байтов.

Можно написать одну функцию, которая отвечает за все. Основная проблема, которая может возникнуть, связана с выполнением **масштабируемости**. На одной виртуальной машине можно запустить выполнение только одной функции, поэтому пропускная способность будет ограничена пропускной способностью одной виртуальной машины. Следующая проблема заключается в **надежности**. Если в середине процесса происходит сбой или весь процесс занимает более 5 минут, резервное копирование может завершиться ошибкой в состоянии частичного завершения. В таком случае его необходимо начать сначала.

Более надежным подходом было бы записать две обычные функции: одна для перечисления файлов и добавления их имен в очередь, а другая для чтения из очереди и отправки файлов в хранилище BLOB-объектов. Это лучше с точки зрения пропускной способность и надежности, но для этого необходимо подготовить очередь и управлять ею. Если требуется выполнить какое-либо действие (например, сообщить об общем количестве отправленных байтов), могут возникнуть значительные проблемы в контексте **управления состоянием** и **координацией**.

Подход к устойчивым функциям обеспечивает получение всех упомянутых преимуществ с очень низкими издержками.

## <a name="the-functions"></a>Функции

В этой статье описаны следующие функции в примере приложения:

* `E2_BackupSiteContent`
* `E2_GetFileList`
* `E2_CopyFileToBlob`

В следующих разделах рассматривается конфигурация и код, которые используются для написания скриптов на языке C#. Код для разработки с помощью Visual Studio представлен в конце этой статьи.

## <a name="the-cloud-backup-orchestration-visual-studio-code-and-azure-portal-sample-code"></a>Оркестрация облачной резервной копии (пример кода Visual Studio Code и портала Azure)

Функция `E2_BackupSiteContent` использует стандартный файл *function.json* для функций оркестратора.

[!code-json[Main](~/samples-durable-functions/samples/csx/E2_BackupSiteContent/function.json)]

Ниже приведен код, реализующий функцию оркестратора.

### <a name="c"></a>C#

[!code-csharp[Main](~/samples-durable-functions/samples/csx/E2_BackupSiteContent/run.csx)]

### <a name="javascript-functions-2x-only"></a>JavaScript (только для решения "Функции" версии 2.x)

[!code-javascript[Main](~/samples-durable-functions/samples/javascript/E2_BackupSiteContent/index.js)]

Эта функция оркестратора выполняет следующие задачи:

1. Принимает значение `rootDirectory` в качестве входного параметра.
2. Вызывает функцию для получения рекурсивного списка файлов в `rootDirectory`.
3. Выполняет несколько параллельных вызовов функции для отправки каждого файла в хранилище BLOB-объектов Azure.
4. Ожидает завершения всех передач.
5. Возвращает сумму общего количества байтов, отправленных в хранилище BLOB-объектов Azure.

Обратите внимание на строки `await Task.WhenAll(tasks);` (C#) и `yield context.df.Task.all(tasks);` (JavaScript). Все отдельные вызовы функции `E2_CopyFileToBlob` *не* ожидались. Это сделано намеренно, чтобы они могли выполняться параллельно. При передаче массива задач в `Task.WhenAll` (C#) или `context.df.Task.all` (JavaScript) мы получаем задачу, которая не выполнится до *завершения всех операций копирования*. Если вы знакомы с библиотекой параллельных задач (TPL) в .NET или с [`Promise.all`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/all) в JavaScript, то вы уже знаете об этой особенности. Разница заключается в том, что задачи могут выполняться на нескольких виртуальных машинах одновременно, а расширение устойчивых функций гарантирует, что комплексное выполнение устойчиво к перезапуску процессов.

> [!NOTE]
> Несмотря на то что задачи концептуально похожи на обещания JavaScript, функции оркестратора должны использовать `context.df.Task.all` и `context.df.Task.any` вместо `Promise.all` и `Promise.race` для управления параллелизацией задач.

Если задача `Task.WhenAll` завершена (или задача `context.df.Task.all` приостановлена), это говорит о том, что все вызовы функций завершены и значения возвращены. Каждый вызов `E2_CopyFileToBlob` возвращает число переданных байтов, поэтому, чтобы узнать сумму всех байтов, нужно сложить все возвращаемые значения.

## <a name="helper-activity-functions"></a>Вспомогательные функции действий

Вспомогательные функции действий, как и другие примеры, являются обычными функциями, которые используют привязку триггера `activityTrigger`. Например, файл *function.json* для `E2_GetFileList` выглядит следующим образом:

[!code-json[Main](~/samples-durable-functions/samples/csx/E2_GetFileList/function.json)]

Вот сама реализация:

### <a name="c"></a>C#

[!code-csharp[Main](~/samples-durable-functions/samples/csx/E2_GetFileList/run.csx)]

### <a name="javascript-functions-2x-only"></a>JavaScript (только для решения "Функции" версии 2.x)

[!code-javascript[Main](~/samples-durable-functions/samples/javascript/E2_GetFileList/index.js)]

В реализации `E2_GetFileList` для JavaScript используется модуль `readdirp` для рекурсивного считывания структуры каталогов.

> [!NOTE]
> Вы можете спросить, почему нельзя просто поместить этот код непосредственно в функцию оркестратора. Это действие может нарушить одно из основных правил функций оркестратора — они не должны выполнять операции ввода-вывода, включая доступ к локальной файловой системе.

Файл *unction.json* для `E2_CopyFileToBlob` такой же простой:

[!code-json[Main](~/samples-durable-functions/samples/csx/E2_CopyFileToBlob/function.json)]

Реализация для C# также не вызывает затруднений. Иногда используются некоторые дополнительные функции привязок Функций Azure (то есть параметр `Binder`). Но не нужно беспокоиться об этом в рамках нашего пошагового руководства.

### <a name="c"></a>C#

[!code-csharp[Main](~/samples-durable-functions/samples/csx/E2_CopyFileToBlob/run.csx)]

### <a name="javascript-functions-2x-only"></a>JavaScript (только для решения "Функции" версии 2.x)

В реализации JavaScript нет доступа к компоненту `Binder` решения "Функции Azure". Вместо этого используется [пакет SDK службы хранилища Azure для Node](https://github.com/Azure/azure-storage-node).

[!code-javascript[Main](~/samples-durable-functions/samples/javascript/E2_CopyFileToBlob/index.js)]

Реализация загружает файл с диска и асинхронно выполняет потоковую передачу содержимого в одноименный большой двоичный объект в контейнере резервных копий. В результате возвращается количество байтов, скопированных в хранилище, которое затем используется функцией оркестратора для вычисления общей суммы.

> [!NOTE]
> Это отличный пример перемещения операций ввода-вывода в функцию `activityTrigger`. Можно не только распределить работу между многими виртуальными машинами, но и получить преимущества создания контрольных точек этапов выполнения. Если хост-процесс по какой-либо причине завершится, вы будете знать, какие передачи уже выполнены.

## <a name="run-the-sample"></a>Запуск примера

Можно начать оркестрацию, отправив приведенный ниже запрос HTTP POST.

```
POST http://{host}/orchestrators/E2_BackupSiteContent
Content-Type: application/json
Content-Length: 20

"D:\\home\\LogFiles"
```

> [!NOTE]
> Вызываемая функция `HttpStart` работает только с содержимым в формате JSON. По этой причине требуется заголовок `Content-Type: application/json` и путь к каталогу кодируется в виде строки JSON. Кроме того, в предыдущем фрагменте HTTP предполагается, что в файле `host.json` существует запись, которая позволяет удалить префикс `api/` по умолчанию из всех URL-адресов функций для триггеров HTTP. Разметку для этой конфигурации можно найти в файле `host.json` в примерах.

Этот запрос HTTP активирует оркестратор `E2_BackupSiteContent` и передает строку `D:\home\LogFiles` в качестве параметра. В ответе содержится ссылка на получение информации о состоянии операции резервного копирования:

```
HTTP/1.1 202 Accepted
Content-Length: 719
Content-Type: application/json; charset=utf-8
Location: http://{host}/admin/extensions/DurableTaskExtension/instances/b4e9bdcc435d460f8dc008115ff0a8a9?taskHub=DurableFunctionsHub&connection=Storage&code={systemKey}

(...trimmed...)
```

В зависимости от количества файлов журнала в приложении-функции, эта операция может занять несколько минут. Информацию об актуальном состоянии можно получить, отправив запрос на URL-адрес в заголовке `Location` предыдущего ответа HTTP 202.

```
GET http://{host}/admin/extensions/DurableTaskExtension/instances/b4e9bdcc435d460f8dc008115ff0a8a9?taskHub=DurableFunctionsHub&connection=Storage&code={systemKey}
```

```
HTTP/1.1 202 Accepted
Content-Length: 148
Content-Type: application/json; charset=utf-8
Location: http://{host}/admin/extensions/DurableTaskExtension/instances/b4e9bdcc435d460f8dc008115ff0a8a9?taskHub=DurableFunctionsHub&connection=Storage&code={systemKey}

{"runtimeStatus":"Running","input":"D:\\home\\LogFiles","output":null,"createdTime":"2017-06-29T18:50:55Z","lastUpdatedTime":"2017-06-29T18:51:16Z"}
```

В этом случае функция все еще выполняется. Вы можете просмотреть входные данные, сохраненные в состоянии оркестратора, и последнее время обновления. Для опроса состояния завершения операции можно продолжать использовать значения заголовка `Location`. Если состояние — "Завершено", вы увидите HTTP-ответ, как в примере ниже:

```
HTTP/1.1 200 OK
Content-Length: 152
Content-Type: application/json; charset=utf-8

{"runtimeStatus":"Completed","input":"D:\\home\\LogFiles","output":452071,"createdTime":"2017-06-29T18:50:55Z","lastUpdatedTime":"2017-06-29T18:51:26Z"}
```

Теперь оркестрация завершена, и вы можете оценить, сколько времени ушло на ее выполнение. Вы также увидите значение поля `output`, в котором указано, что отправлено примерно 450 КБ данных журналов.

## <a name="visual-studio-sample-code"></a>Пример кода для Visual Studio

Пример описанной оркестрации, реализованной в одном файле C# в проекте Visual Studio:

> [!NOTE]
> Чтобы запустить приведенный ниже пример кода, потребуется установить пакет Nuget `Microsoft.Azure.WebJobs.Extensions.Storage`.

[!code-csharp[Main](~/samples-durable-functions/samples/precompiled/BackupSiteContent.cs)]

## <a name="next-steps"></a>Дальнейшие действия

В этом примере показано, как реализовать шаблон развертывания и объединения. В следующем примере показано, как реализовать шаблон мониторинга с помощью [устойчивых таймеров](durable-functions-timers.md).

> [!div class="nextstepaction"]
> [Запуск примера мониторинга](durable-functions-monitor.md)
