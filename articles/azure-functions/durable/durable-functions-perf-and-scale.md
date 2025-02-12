---
title: Производительность и масштабирование в устойчивых функциях — Azure
description: Общие сведения о расширении устойчивых функций для Функций Azure.
services: functions
author: cgillum
manager: jeconnoc
keywords: ''
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 03/14/2019
ms.author: azfuncdf
ms.openlocfilehash: e6ae4cc527ae0828f530ab7f3904d2b3c64c910b
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "60733252"
---
# <a name="performance-and-scale-in-durable-functions-azure-functions"></a>Производительность и масштабирование в устойчивых функциях (Функции Azure)

Чтобы оптимизировать производительность и масштабирование, важно понять уникальные характеристики масштабирования [устойчивых функций](durable-functions-overview.md).

Чтобы понять реакции на события масштабирования, необходимо изучить некоторые сведения о базовом поставщике службы хранилища Azure.

## <a name="history-table"></a>Таблица журнала

Таблица **журнала** — это таблица службы хранилища Azure, содержащая события журнала для всех экземпляров оркестрации внутри центра задач. Имя этой таблицы — указывается в формате журнала *TaskHubName*. При выполнении экземпляров в таблицу добавляются новые строки. Ключ раздела этой таблицы является производным от идентификатора экземпляра оркестрации. В большинстве случаев экземпляр идентификатора является случайным, что обеспечивает оптимальное распределение внутренних разделов в службе хранилища Azure.

Когда необходимо выполнить экземпляр оркестрации, в память загружаются нужные строки таблицы журнала. Затем эти *события журнала* воспроизводятся в коде функции оркестратора, чтобы вернуть их в предыдущее состояние из контрольной точки. Использование журнала выполнения для перестройки состояния, является путем, обусловленным [шаблоном источников событий](https://docs.microsoft.com/azure/architecture/patterns/event-sourcing).

## <a name="instances-table"></a>Экземпляры таблицы

Таблица **Экземпляров** — является другой таблицей службы хранилища Azure, содержащей состояния для всех экземпляров оркестрации внутри центра задач. При создании экземпляров в таблицу добавляются новые строки. Ключ раздела является идентификатором экземпляра оркестрации этой таблицы, а ключ строки — фиксированной константой. На экземпляр оркестрации приходится одна строка.

Эта таблица используется для удовлетворения запросов экземпляра от API [GetStatusAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_GetStatusAsync_System_String_) (.NET) и `getStatus` (JavaScript), а также [API HTTP запроса состояния](durable-functions-http-api.md#get-instance-status). В конечном итоге она поддерживается в соответствии с содержанием таблицы **Журнала**, упомянутой ранее. Использование отдельных таблиц хранилища Azure для эффективного удовлетворения операций запроса экземпляров, является путем, обусловленным [шаблоном CQRS](https://docs.microsoft.com/azure/architecture/patterns/cqrs).

## <a name="internal-queue-triggers"></a>Триггеры внутренней очереди

Функции оркестратора и действия активируются внутренними очередями в учетной записи хранения центра задач приложения-функции. Такое использование очередей обеспечивает надежные гарантии доставки сообщений, как минимум один раз. Существует два типа очередей в устойчивых функциях: очередь **управления** и **очередь рабочих элементов**.

### <a name="the-work-item-queue"></a>Очередь рабочих элементов

В устойчивых функциях на каждый центр задач приходится одна очередь рабочих элементов. Это основная очередь, и ее поведение аналогично любым другим очередям `queueTrigger` в Функциях Azure. Эта очередь используется для активации *функций действий* путем вывода только одного сообщения за один раз. Каждое из этих сообщений содержит входные данные функции действия и дополнительные метаданные, например, какую функцию выполнять. Когда приложение устойчивых функций масштабируется до нескольких виртуальных машин, все они конкурируют за получение работы из очереди рабочих элементов.

### <a name="control-queues"></a>Очередь управления

В устойчивых функциях на каждый центр задач имеется несколько *очередей управления*. *Очередь управления* является более сложной, чем очередь рабочих элементов. Очереди управления используются для активации функций оркестратора с отслеживанием состояния. Так как экземпляры функции оркестратора представляют собой единичные экземпляры с отслеживанием состояния, невозможно использовать конкурирующую потребительскую модель для распределения нагрузки между виртуальными машинами. Вместо этого сообщения оркестратора распределяются по очередям управления. Дополнительные сведения об этом поведении можно найти в последующих разделах.

Очереди управления содержат различные типы сообщений жизненного цикла оркестрации. Примеры включают [сообщения об управлении оркестрацией](durable-functions-instance-management.md), сообщения *ответа* функции действия и сообщения таймера. За один опрос из очереди управления может быть удалено до 32 сообщений. Эти сообщения содержат полезные данные, а также метаданные, включая экземпляр оркестрации, для которого они предназначены. Если для одного и того же экземпляра оркестрации предназначены несколько сообщений удаленных из очереди, они будут обрабатываться как пакет.

### <a name="queue-polling"></a>Очередь опроса

Расширения устойчивых задач реализован экспоненциальной отсрочки алгоритм случайной позволяет уменьшить влияние опроса очереди на затраты на транзакции хранилища. Когда сообщение найдено, среда выполнения проверяет немедленно еще одно сообщение; Если сообщение не найдено, он ожидает определенного периода времени перед повторной попыткой. После последующих неудачных попыток получения сообщения очереди время ожидания продолжает увеличиваться, пока не достигнет максимального времени ожидания, по умолчанию — 30 секунд.

Задержка максимальное опроса можно настроить с помощью `maxQueuePollingInterval` свойство в [файле host.json](../functions-host-json.md#durabletask). Если установить более высокое значение может привести обработки задержки более поздней версии сообщения. Более длительных задержек ожидается только после окончания периода бездействия. Этому параметру присвоено меньшее значение может привести к больше затрат на хранение, из-за транзакции увеличению размера хранилища.

> [!NOTE]
> При выполнении в планах потребления функций Azure и "премиум", [контроллера масштабирования функций Azure](../functions-scale.md#how-the-consumption-and-premium-plans-work) будет проводить опрос каждого элемента управления и рабочих элементов очередь каждые 10 секунд. Этот дополнительный опроса возникает необходимость определить, когда для активации экземпляров приложения-функции и для принятия решений масштабирования. Во время написания этой статьи этот 10 второй интервал является константой и не может быть настроен.

## <a name="storage-account-selection"></a>Выбор учетной записи хранения

Очереди, таблицы и большие двоичные объекты, используемом устойчивыми функциями создаются в настроенной учетной записи хранения Azure. Используемая учетная запись может быть задана с помощью параметра `durableTask/azureStorageConnectionStringName` в файле **host.json**.

### <a name="functions-1x"></a>Функции 1.x

```json
{
  "durableTask": {
    "azureStorageConnectionStringName": "MyStorageAccountAppSetting"
  }
}
```

### <a name="functions-2x"></a>Функции 2.x

```json
{
  "extensions": {
    "durableTask": {
      "azureStorageConnectionStringName": "MyStorageAccountAppSetting"
    }
  }
}
```

Если значение не указано, учетная запись `AzureWebJobsStorage` используется по умолчанию. Однако, для рабочих нагрузок, требующих высокой производительности, рекомендуется настроить учетную запись хранения, кроме используемой по умолчанию. Устойчивые функции сильно полагаются на хранилище Azure и используют отдельную учетную запись хранения для изоляции использования хранилища устойчивых функций узлом функций Azure.

## <a name="orchestrator-scale-out"></a>Развертывание оркестратора

Функции действий не отслеживают состояние и автоматически масштабируются путем добавления виртуальных машин. Функции оркестратора, в свою очередь, *разделяются* между одной или несколькими очередями управления. Число очередей управления определено в файле **host.json**. В следующем примере отображено, как в фрагменте файла host.json устанавливается значение свойства `durableTask/partitionCount` на `3`.

### <a name="functions-1x"></a>Функции 1.x

```json
{
  "durableTask": {
    "partitionCount": 3
  }
}
```

### <a name="functions-2x"></a>Функции 2.x

```json
{
  "extensions": {
    "durableTask": {
      "partitionCount": 3
    }
  }
}
```

Центр задач может быть настроен в диапазоне от 1 до 16 секций. Если значение не задано, по умолчанию количеству секций соответствует значение **4**.

При развертывании нескольких экземпляров узлов функции (обычно на разных виртуальных машинах) каждый экземпляр блокируется на одной из очередей управления. Эти блокировки внутренне реализуются как аренда хранилища BLOB-объектов. Необходимо убедиться, что за один раз экземпляр оркестрации запускается только на едином узле. Если центр задач настроен с тремя очередями управления, экземпляры оркестрации можно распределять максимум между тремя виртуальными машинами. Для повышения емкости выполнения функции действия можно добавить дополнительные виртуальные машины.

На схеме ниже показано взаимодействие узла Функций Azure с сущностями хранилища в развернутой среде.

![Схема масштабирования](./media/durable-functions-perf-and-scale/scale-diagram.png)

Как показано на предыдущей схеме, все виртуальные машины могут конкурировать за сообщения в очереди рабочих элементов. Тем не менее получать сообщения из очередей управления могут только три виртуальные машины, и каждая из них блокирует одиночную очередь управления.

Экземпляры оркестрации распространяются по всем экземплярам очереди управления. Распространение происходит путем хэширования идентификатора экземпляра оркестрации. По умолчанию идентификаторы экземпляров являются случайными идентификаторами GUID, гарантируя равномерное распределение экземпляров между всеми очередями управления.

Как правило, функции оркестратора должны быть упрощенными, поэтому они не должны требовать больших вычислительных мощностей. Поэтому не нужно создавать большое количество разделов очереди управления для получения большой пропускной способности. Большая часть работы должна выполняться в функциях действий без отслеживания состояния, которые можно масштабировать бесконечно.

## <a name="auto-scale"></a>Автомасштабирование

Как и все функции Azure, работающие в рамках плана потребления, устойчивые функции поддерживают автомасштабирование через [контроллер масштабирования Функций Azure](../functions-scale.md#runtime-scaling). Контроллер масштабирования занимается мониторингом задержки всех очередей, периодически выполняя команды _peek_. Основываясь на задержках просматриваемых сообщений, контроллер масштабирования будет решать, следует ли добавлять или удалять виртуальные машины.

Если контроллер масштабирования определит что задержка от сообщения очереди управления слишком большая, он будет увеличивать количество экземпляров виртуальных машин пока она не вернется до приемлемого уровня или достигнет максимального количества разделов очереди управления. Аналогичным образом контроллер масштабирования будет непрерывно добавлять экземпляры виртуальных машин в случае если задержки в очереди на рабочих элементах слишком большие, независимо от количества разделов.

## <a name="thread-usage"></a>Использование потока

Функции оркестратора выполняются в одном потоке, чтобы убедится, что выполнение можно детерминировать на множество воспроизведений. Так как выполнение является однопоточным, очень важно, чтобы потоки функции оркестратора по любой причине не выполняли задачи, интенсивно использующие ЦП, операции ввода-вывода или блокировку. Все операции, которым может потребоваться ввод-вывод, блокировка или несколько потоков, должны выполняться с помощью функций действий.

Функции действий имеют такие же реакции на события, как и регулярные активируемые очередью функции. Они могут безопасно выполнять операции ввода-вывода, операции с интенсивным потреблением ЦП и использовать несколько потоков. Так как триггеры действия не отслеживают состояние, они могут свободно масштабироваться на неограниченное количество виртуальных машин.

## <a name="concurrency-throttles"></a>Регулирование параллелизма

Функции Azure поддерживают параллельное выполнение нескольких функций в рамках одного экземпляра приложения. Параллельное выполнение помогает улучшить параллелизм и минимизировать число холодного запуска, с которым со времени столкнется стандартное приложение. Тем не менее высокий уровень параллелизма может привести к большому использованию памяти виртуальных машин. В зависимости от потребностей приложения функции, может потребоваться отрегулировать параллелизм для каждого экземпляра, чтобы избежать возможности исчерпания памяти в ситуациях с высокой нагрузкой.

Ограничения параллелизма для функции действия и функции оркестратора можно настроить в файле **host.json**. Актуальными параметрами являются `durableTask/maxConcurrentActivityFunctions` и `durableTask/maxConcurrentOrchestratorFunctions`.

### <a name="functions-1x"></a>Функции 1.x

```json
{
  "durableTask": {
    "maxConcurrentActivityFunctions": 10,
    "maxConcurrentOrchestratorFunctions": 10
  }
}
```

### <a name="functions-2x"></a>Функции 2.x

```json
{
  "extensions": {
    "durableTask": {
      "maxConcurrentActivityFunctions": 10,
      "maxConcurrentOrchestratorFunctions": 10
    }
  }
}
```

В предыдущем примере не более 10 функции оркестратора и 10 функции действия смогли параллельно выполняться на одной виртуальной машине. Если не указано, число параллельно выполняемых функций действия и оркестратора ограничено 10х количеством ядер виртуальной машины.

> [!NOTE]
> Эти параметры полезны для управления памятью и использованием ЦП на одной виртуальной машине. Тем не менее при развертывании на нескольких виртуальных машинах каждая из них будет иметь собственный набор ограничений. Эти параметры невозможно использовать для регулировки параллелизма на глобальном уровне.

## <a name="orchestrator-function-replay"></a>Повторение функции оркестратора

Как было упомянуто ранее, функции оркестратора воспроизводятся с помощью содержимого таблицы **Журнал**. По умолчанию код функции оркестратора воспроизводится каждый раз, когда пакет сообщений удаляется из очереди управления.

Включив **расширенные сеансы**, это агрессивное поведение воспроизведения можно отключить. Когда расширенные сеансы включены, экземпляры функции оркестратора остаются в памяти дольше и новые сообщения могут обрабатываться без полного воспроизведения. Расширенные сеансы включаются установкой значения `durableTask/extendedSessionsEnabled` на `true` в файле **host.json**. Параметр `durableTask/extendedSessionIdleTimeoutInSeconds` используется для управления периодом нахождения бездействующих сеансов в памяти.

### <a name="functions-1x"></a>Функции 1.x

```json
{
  "durableTask": {
    "extendedSessionsEnabled": true,
    "extendedSessionIdleTimeoutInSeconds": 30
  }
}
```

### <a name="functions-2x"></a>Функции 2.x

```json
{
  "extensions": {
    "durableTask": {
      "extendedSessionsEnabled": true,
      "extendedSessionIdleTimeoutInSeconds": 30
    }
  }
}
```

Распространенным эффектом от включения расширенных сеансов является сокращение операций ввода-вывода с использованием учетной записи службы хранения Azure и повсеместное улучшение пропускной способности.

Однако, одним из минусов этой функции является то, что экземпляры функции бездействующих сеансов будут дольше находится в памяти. Существует два эффекты, которые следует учитывать:

1. Общее увеличение использования памяти приложением.
2. Существование многих одновременных, кратковременных выполнений функции оркестратора приведет к уменьшению пропускной способности.

Например, если выполнение `durableTask/extendedSessionIdleTimeoutInSeconds` установлено на 30 секунд, то кратковременный эпизод функции оркестратора, который выполняется менее 1 секунды, по-прежнему будут занимать память на 30 секунд. Он также учитывает указанную ранее квоту `durableTask/maxConcurrentOrchestratorFunctions`, предотвращая потенциальный запуска других функций оркестратора.

> [!NOTE]
> Эти параметры следует использовать только после того, как функция оркестратора будет полностью разработана и протестирована. Агрессивное повторяемое поведения по умолчанию, полезно для обнаружения ошибок идемпотентности в оркестровых функциях во время разработки.

## <a name="performance-targets"></a>Цели анализа производительности

Планируя использовать устойчивые функции в рабочем приложении, на раннем этапе планирования необходимо учесть требования к производительности. В этом разделе рассматриваются некоторое основные сценарии использования и ожидаемое максимальной число пропускной способности.

* **Выполнение последовательного действия.** В этом сценарии описывается функция оркестратора, которая по очереди выполняет ряд функций действия. Максимально похожим на этот сценарий является [цепочка функции](durable-functions-sequence.md).
* **Параллельное выполнение одного действия.** В этом сценарии описано действие оркестратора, которое выполняет множество функций действий параллельно, с помощью шаблона [развертывания и слияния](durable-functions-cloud-backup.md).
* **Параллельная обработка ответа.** Этот сценарий является второй половиной шаблона [развертывания и слияния](durable-functions-cloud-backup.md). Этот раздел посвящен производительность слияния. Важно отметить, что в отличие от развертывания, слияние создается единым экземпляром функции а затем может быть запущено на единой виртуальной машине.
* **Обработка внешних событий.** В этом сценарии представлен единый экземпляр функции оркестратора, который последовательно ожидает [внешних событий](durable-functions-external-events.md).

> [!TIP]
> В отличие от развертывания, операции слияния могут размещаться только на одной виртуальной машине. Если ваше приложение использует шаблоны развертывания и слияния, и вы беспокоитесь через производительность развертывания, необходимо рассмотреть разделение действия функции развертывания на несколько [вложенных оркестраций](durable-functions-sub-orchestrations.md).

В следующей таблице показаны ожидаемые *максимальные* показателей пропускной способности для сценариев, описанных ранее. Термин "экземпляр" относиться к единому экземпляру функции оркестратора, запущенной на небольшом экземпляре виртуальной машины ([серии A1](../../virtual-machines/windows/sizes-previous-gen.md#a-series)) в службе приложений Azure. Во всех случаях предполагается, что [расширенные сеансы](#orchestrator-function-replay) включены. Фактические результаты могут варьировать, в зависимости от ЦП или ввода-вывода, выполняемых кодом функции.

| Сценарий | Максимальная пропускная способность |
|-|-|
| Выполнение последовательного действия | 5 действий на экземпляр в секунду |
| Параллельное выполнение одного действия (при развертывании) | 100 действий на экземпляр в секунду |
| Параллельная обработка ответа (при слиянии) | 150 ответов на экземпляр в секунду |
| Обработка внешних событий | 50 событий на экземпляр в секунду |

> [!NOTE]
> Эти номера являются текущими по сравнению с выпуском расширения устойчивых функций v1.4.0 (GA). Со временем эти числа могут изменятся (по мере роста функции и оптимизации).

Если вы не видите показателей пропускной способности, и ваш ЦП и использование памяти отображаются в работоспособном состоянии, проверьте, описаны ли эти случаи в разделе [рекомендации по устранению неполадок](../../storage/common/storage-monitoring-diagnosing-troubleshooting.md#troubleshooting-guidance). Расширение устойчивых функций может существенно увеличить нагрузку на учетную запись службы хранения Azure, что при достаточно высоких загрузках может привести к регулированию учетной записи.

## <a name="next-steps"></a>Дальнейшие действия

> [!div class="nextstepaction"]
> [Create your first durable function in C#](durable-functions-create-first-csharp.md) (Создание устойчивой функции: C#)
