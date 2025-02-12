---
title: Часто используемые метрики автоматического масштабирования
description: Узнайте, какие метрики обычно используются для автоматического масштабирования облачных служб, виртуальных машин и веб-приложений.
author: anirudhcavale
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 12/6/2016
ms.author: ancav
ms.subservice: autoscale
ms.openlocfilehash: 9da8e5fb88ff34e561b579b760973ecd23c884a3
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "66129734"
---
# <a name="azure-monitor-autoscaling-common-metrics"></a>Общие метрики автомасштабирования Azure Monitor

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Автомасштабирование Azure Monitor позволяет увеличивать или уменьшать количество запущенных экземпляров на основе данных телеметрии (метрик). В этой статье рассматриваются общие метрики, которые вы можете использовать. На портале Azure можно выбрать метрики ресурсов, по которым будет выполняться масштабирование. Однако вы также можете выбрать любую метрику из другого ресурса.

Автомасштабирование Azure Monitor используется только с [масштабируемыми наборами виртуальных машин](https://azure.microsoft.com/services/virtual-machine-scale-sets/), [облачными службами](https://azure.microsoft.com/services/cloud-services/), [веб-приложениями службы приложений](https://azure.microsoft.com/services/app-service/web/) и [службами управления API](https://docs.microsoft.com/azure/api-management/api-management-key-concepts). Прочие службы Azure используют другие методы масштабирования.

## <a name="compute-metrics-for-resource-manager-based-vms"></a>Расчет метрик для виртуальных машин, развернутых с помощью Resource Manager
По умолчанию виртуальные машины и масштабируемые наборы виртуальных машин, развернутые с помощью Resource Manager, генерируют базовые метрики (уровня узла). Кроме того, при настройке сбора данных диагностики для виртуальной машины и масштабируемого набора виртуальных машин Azure расширение диагностики Azure также генерирует счетчики производительности гостевой ОС (известные как "метрики гостевой ОС").  Используйте все эти метрики в правилах автомасштабирования.

Чтобы просмотреть метрики, доступные для ресурса VMSS, в API, PoSH и интерфейсе командной строки можно выполнить команду `Get MetricDefinitions`.

Если вы используете наборы масштабирования виртуальных машин и не находите определенной метрики в списке, скорее всего, она *отключена* в расширении системы диагностики.

Если выборка или передача определенной метрики осуществляется с частотой, которая не соответствует вашим требованиям, можно обновить конфигурацию системы диагностики.

Если это так, то см. сведения о настройке и обновлении расширения системы диагностики виртуальной машины Azure для включения метрики с использованием PowerShell в статье [Включение системы диагностики Azure на виртуальной машине под управлением Windows с помощью PowerShell](../../virtual-machines/extensions/diagnostics-windows.md). В этой статье также содержится пример файла конфигурации системы диагностики.

### <a name="host-metrics-for-resource-manager-based-windows-and-linux-vms"></a>Метрики уровня узла для виртуальных машин под управлением Windows и Linux, развернутых с помощью Resource Manager
Следующие метрики уровня узла генерируются по умолчанию для виртуальной машины и масштабируемого набора виртуальных машин Azure в экземплярах Windows и Linux. Эти метрики описывают виртуальную машину Azure, но собираются из узла виртуальной машины Azure, а не через агент, установленный на гостевой виртуальной машине. Эти метрики можно использовать в правилах автомасштабирования.

- [Метрики уровня узла для виртуальных машин под управлением Windows и Linux, развернутых с помощью Resource Manager](../../azure-monitor/platform/metrics-supported.md#microsoftcomputevirtualmachines)
- [Метрики уровня узла для масштабируемых наборов виртуальных машин под управлением Windows и Linux, развернутых с помощью Resource Manager](../../azure-monitor/platform/metrics-supported.md#microsoftcomputevirtualmachinescalesets)

### <a name="guest-os-metrics-resource-manager-based-windows-vms"></a>Метрики гостевой ОС для виртуальных машин под управлением Windows, развернутых с помощью Resource Manager
При создании виртуальной машины в Azure система диагностики включается с помощью расширения диагностики. Расширение диагностики генерирует набор метрик, полученных из виртуальной машины. Это означает, что вы можете отключить автомасштабирование метрик, которые не генерируются по умолчанию.

Вы можете создать список метрик с помощью следующей команды PowerShell:

```
Get-AzMetricDefinition -ResourceId <resource_id> | Format-Table -Property Name,Unit
```

Вы можете создать оповещение для метрик, приведенных в следующей таблице:

| Имя метрики | Единица измерения |
| --- | --- |
| \Процессор (_общий объем ресурсов)\% загруженности процессора |Процент |
| \Процессор(_общий объем ресурсов)\% времени в привилегированном режиме |Процент |
| \Процессор(_общий объем ресурсов)\% времени в пользовательском режиме |Процент |
| \Процессор(_общий объем ресурсов)\частота процессора |Количество |
| \Система\процессы |Количество |
| \Процессор (_общий объем ресурсов)\число потоков |Количество |
| \Процессор (_общий объем ресурсов)\число обработанных элементов |Количество |
| \Память\% использования выделенной памяти (в байтах) |Процент |
| \Память\доступные байты |Байты |
| \Память\выделенная память (в байтах) |Байты |
| \Память\предел выделенной памяти |Байты |
| \Память\выгружаемый пул (в байтах) |Байты |
| \Память\невыгружаемый пул (в байтах) |Байты |
| \Физический диск(_общий объем ресурсов)\% времени работы диска |Процент |
| \Физический диск(_общий объем ресурсов)\% времени чтения с диска |Процент |
| \Физический диск(_общий объем ресурсов)\% времени записи на диск |Процент |
| \Диск\Физический диск(_общий объем ресурсов) в секунду |Число/с |
| \Физический диск(_общий объем ресурсов)\скорость чтения с диска/с |Число/с |
| \Физический диск(_общий объем ресурсов)\скорость записи на диск/с |Число/с |
| \Физический диск(_общий объем ресурсов)\скорость передачи данных (в байтах)/с |Байт/с |
| \Физический диск(_общий объем ресурсов)\скорость чтения с диска (в байтах)/с |Байт/с |
| \Физический диск(_общий объем ресурсов)\скорость записи на диск (в байтах)/с |Байт/с |
| \Физический диск(_общий объем ресурсов)\среднее значение Длина дисковой очереди |Количество |
| \Физический диск(_общий объем ресурсов)\среднее значение Длина очереди чтения с диска |Количество |
| \Физический диск(_общий объем ресурсов)\среднее значение Длина очереди записи на диск |Количество |
| \Физический диск(_общий объем ресурсов)\% свободного места |Процент |
| \Логический диск(_общий объем ресурсов)\свободная память (в мегабайтах) |Количество |

### <a name="guest-os-metrics-linux-vms"></a>Метрики гостевой ОС для виртуальных машин под управлением Linux
При создании виртуальной машины в Azure система диагностики включается по умолчанию с помощью расширения диагностики.

Вы можете создать список метрик с помощью следующей команды PowerShell:

```
Get-AzMetricDefinition -ResourceId <resource_id> | Format-Table -Property Name,Unit
```

 Вы можете создать оповещение для метрик, приведенных в следующей таблице:

| Имя метрики | Единица измерения |
| --- | --- |
| \Память\Доступная память |Байты |
| \Память\Процент доступной памяти |Процент |
| \Память\Используемая память |Байты |
| \Память\Процент используемой памяти |Процент |
| \Память\Процент памяти, используемой кэшем |Процент |
| \Память\Страниц в с |Число/с |
| \Память\Прочитано страниц в с |Число/с |
| \Память\Записано страниц в с |Число/с |
| \Память\Доступное пространство файла подкачки |Байты |
| \Память\Процент доступного пространства файла подкачки |Процент |
| \Память\Используемое пространство файла подкачки |Байты |
| \Память\Процент использованного пространства файла подкачки |Процент |
| \Процессор\Процент времени простоя |Процент |
| \Процессор\Процент работы в пользовательском режиме |Процент |
| \Процессор\Процент времени оптимальной работы |Процент |
| \Процессор\Процент работы в привилегированном режиме |Процент |
| \Процессор\Процент времени прерываний |Процент |
| \Процессор\Процент времени DPC |Процент |
| \Процессор\Процент загруженности процессора |Процент |
| \Процессор\Процент времени ожидания ввода/вывода |Процент |
| \Физический диск\Байт/с |Байт/с |
| \Физический диск\Прочитано байт/с |Байт/с |
| \Физический диск\Записано байт/с |Байт/с |
| \Физический диск\Передач/с |Число/с |
| \Физический диск\Операций чтения/с |Число/с |
| \Физический диск\Операций записи/с |Число/с |
| \Физический диск\Среднее время чтения |Секунды |
| \Физический диск\Среднее время записи |Секунды |
| \Физический диск\Среднее время передачи |Секунды |
| \Физический диск\Средняя длина очереди диска |Количество |
| \Сетевой интерфейс\Передано байт |Байты |
| \Сетевой интерфейс\Получено байт |Байты |
| \Сетевой интерфейс\Передано пакетов |Количество |
| \Сетевой интерфейс\ Получено пакетов |Количество |
| \Сетевой интерфейс\Всего байт |Байты |
| \Сетевой интерфейс\Всего ошибок RX |Количество |
| \Сетевой интерфейс\Всего ошибок TX |Количество |
| \Сетевой интерфейс\Всего конфликтов |Количество |

## <a name="commonly-used-web-server-farm-metrics"></a>Часто используемые метрики веб-приложений (фермы серверов)
Можно также выполнить автомасштабирование на основе общих метрик веб-сервера, таких как длина очереди HTTP. Имя метрики — **HttpQueueLength**.  В следующем разделе перечислены доступные метрики фермы серверов (веб-приложений).

### <a name="web-apps-metrics"></a>Метрики веб-приложений
Список метрик веб-приложений можно создать с помощью следующей команды PowerShell:

```
Get-AzMetricDefinition -ResourceId <resource_id> | Format-Table -Property Name,Unit
```

Можно создавать оповещения и выполнять масштабирование на основе метрик в таблице ниже.

| Имя метрики | Единица измерения |
| --- | --- |
| Процент загруженности ЦП |Процент |
| Процент использования памяти |Процент |
| Длина очереди диска |Количество |
| Длина очереди HTTP |Количество |
| Получено байт |Байты |
| BytesSent |Байты |

## <a name="commonly-used-storage-metrics"></a>Часто используемые метрики хранилища
Можно масштабировать по длине очереди хранилища, которая представляет собой количество сообщений в очереди хранилища. Пороговым значением для этой специальной метрики является число сообщений на один экземпляр. Например, если есть два экземпляра и если пороговое значение — 100, то масштабирование выполняется, когда общее число сообщений в очереди достигнет 200. Число сообщений на экземпляр может быть 100, или 120 и 80, или любое другое сочетание, которое в сумме дает 200 или более.

Настройте эти значения на портале Azure в колонке **Параметры**. Для масштабируемых наборов виртуальных машин можно настроить параметр автомасштабирования в шаблоне Resource Manager так, чтобы для параметра *metricName* использовалось значение *ApproximateMessageCount*, а идентификатор очереди хранилища передавался в параметре *metricResourceUri*.

Например, в классической учетной записи хранения metricTrigger параметра автомасштабирования включает следующие данные:

```
"metricName": "ApproximateMessageCount",
 "metricNamespace": "",
 "metricResourceUri": "/subscriptions/SUBSCRIPTION_ID/resourceGroups/RES_GROUP_NAME/providers/Microsoft.ClassicStorage/storageAccounts/STORAGE_ACCOUNT_NAME/services/queue/queues/QUEUE_NAME"
 ```

В обычной учетной записи хранения (неклассической) metricTrigger включает следующие данные:

```
"metricName": "ApproximateMessageCount",
"metricNamespace": "",
"metricResourceUri": "/subscriptions/SUBSCRIPTION_ID/resourceGroups/RES_GROUP_NAME/providers/Microsoft.Storage/storageAccounts/STORAGE_ACCOUNT_NAME/services/queue/queues/QUEUE_NAME"
```

## <a name="commonly-used-service-bus-metrics"></a>Часто используемые метрики служебной шины
Масштабирование возможно по длине очереди служебной шины. Длина очереди представляет собой количество сообщений в очереди. Пороговым значением для этой специальной метрики является число сообщений на один экземпляр. Например, если есть два экземпляра и если пороговое значение — 100, то масштабирование выполняется, когда общее число сообщений в очереди достигнет 200. Число сообщений на экземпляр может быть 100, или 120 и 80, или любое другое сочетание, которое в сумме дает 200 или более.

Для масштабируемых наборов виртуальных машин можно настроить параметр автомасштабирования в шаблоне Resource Manager так, чтобы для параметра *metricName* использовалось значение *ApproximateMessageCount*, а идентификатор очереди хранилища передавался в параметре *metricResourceUri*.

```
"metricName": "MessageCount",
 "metricNamespace": "",
"metricResourceUri": "/subscriptions/SUBSCRIPTION_ID/resourceGroups/RES_GROUP_NAME/providers/Microsoft.ServiceBus/namespaces/SB_NAMESPACE/queues/QUEUE_NAME"
```

> [!NOTE]
> Для служебной шины концепции группы ресурсов не существует, но Azure Resource Manager создает группу ресурсов по умолчанию на регион. Группа ресурсов обычно имеет формат Default-ServiceBus-[region]. Например, Default-ServiceBus-EastUS, Default-ServiceBus-WestUS, Default-ServiceBus-AustraliaEast и т. д.
>
>

