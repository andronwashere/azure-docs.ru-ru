---
title: Отправка событий в среду Azure Time Series Insights | Документация Майкрософт
description: Сведения о настройке концентратора событий и запуске примера приложения для передачи событий и их отображения в службе "Аналитика временных рядов Azure".
ms.service: time-series-insights
services: time-series-insights
author: ashannon7
ms.author: dpalled
manager: cshankar
ms.reviewer: v-mamcge, jasonh, kfile
ms.devlang: csharp
ms.workload: big-data
ms.topic: conceptual
ms.date: 05/06/2019
ms.custom: seodec18
ms.openlocfilehash: ae59e8115ca2d1ba69c8a3a099216eb3d98e2658
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "66237698"
---
# <a name="send-events-to-a-time-series-insights-environment-by-using-an-event-hub"></a>Отправка событий в среду службы "Аналитика временных рядов" с помощью концентратора событий

В этой статье объясняется, как создать и настроить концентратор событий в концентраторы событий Azure. Также описывается запуск примера приложения для передачи событий в Azure Time Series Insights из концентраторов событий. Если имеется существующий концентратор событий с событиями в формате JSON, пропустите это руководство и просмотрите свою среду в [Azure Time Series Insights](./time-series-insights-update-create-environment.md).

## <a name="configure-an-event-hub"></a>Настройка концентратора событий

1. Чтобы создать концентратор событий, следуйте инструкциям из [документации по Центрам событий](https://docs.microsoft.com/azure/event-hubs/).
1. В поле поиска введите **Центры событий**. В возвращенном списке выберите **Центры событий**.
1. Выберите свой концентратор событий.
1. При создании концентратора событий, вы создаете пространство имен концентратора событий. Если вы еще не создали концентратор событий в пространстве имен, в меню в разделе **сущностей**, создание концентратора событий.  

    [![Список концентраторов событий](media/send-events/updated.png)](media/send-events/updated.png#lightbox)

1. После создания концентратора событий выберите его в списке.
1. В меню в разделе **сущностей**выберите **концентраторов событий**.
1. Выберите имя концентратора событий, чтобы настроить его.
1. В разделе **Сущности** щелкните **Группы потребителей**, а затем выберите **группу объектов-получателей**.

    [![Создание группы потребителей](media/send-events/consumer-group.png)](media/send-events/consumer-group.png#lightbox)

1. Убедитесь, что вы создадите группу потребителей, которая используется исключительно источником событий Time Series Insights.

    > [!IMPORTANT]
    > Убедитесь, что эта группа потребителей не используется никакими другими службами, например задание Azure Stream Analytics или другой среды Time Series Insights. Если группа объектов-получателей используется другими службами, это негативно скажется на операции чтения в этой среде и в других службах. Использование **$Default** в качестве группы объектов-получателей может потенциально привести к ее повторному использованию другими читателями.

1. В меню в разделе **параметры**выберите **политики общего доступа**, а затем выберите **добавить**.

    [![Выберите политики общего доступа, а затем выберите «Добавить»](media/send-events/shared-access-policy.png)](media/send-events/shared-access-policy.png#lightbox)

1. На панели **Добавить новую политику общего доступа** создайте политику общего доступа с именем **MySendPolicy**. Использовать эту политику общего доступа для отправки событий в C# примеры далее в этой статье.

    [![В поле Имя политики введите MySendPolicy](media/send-events/shared-access-policy-2.png)](media/send-events/shared-access-policy-2.png#lightbox)

1. В разделе **утверждения**выберите **отправки** "флажок".

## <a name="add-a-time-series-insights-instance"></a>Добавление экземпляра службы "Аналитика временных рядов"

При обновлении службы "Аналитика временных рядов" используются экземпляры для добавления контекстных данных ко входящим данным телеметрии. Данные объединяются во время выполнения запроса на основе **идентификатора временного ряда**. **Идентификатор ряда времени** для windmills пример — проект, который мы используем далее в этой статье `id`. Дополнительные сведения об экземплярах службы "Аналитика временных рядов" и **идентификаторе временного ряда** см. в [этой статье](./time-series-insights-update-tsm.md).

### <a name="create-a-time-series-insights-event-source"></a>Создание источника событий службы "Аналитика временных рядов"

1. Если вы еще не создали источник событий, создайте его, [выполнив соответствующие шаги](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-how-to-add-an-event-source-eventhub).

1. Задайте значение для `timeSeriesId`. Дополнительные сведения об **идентификаторе временного ряда** см. в статье [Time Series Model](./time-series-insights-update-tsm.md) (Модель временных рядов).

### <a name="push-events"></a>Принудительная отправка событий (пример с ветряными мельницами)

1. В поле поиска введите **Центры событий**. В возвращенном списке выберите **Центры событий**.

1. Выберите свой концентратор событий.

1. Выберите **Политики общего доступа** > **RootManageSharedAccessKey**. Скопируйте значение **строка подключения — первичный ключ**.

    [![Скопируйте значение для строки подключения первичного ключа](media/send-events/sample-code-connection-string.png)](media/send-events/sample-code-connection-string.png#lightbox)

1. Перейдите на сайт https://tsiclientsample.azurewebsites.net/windFarmGen.html. После перехода по этому URL-адресу будут запущены имитированные устройства ветряных мельниц.
1. В поле **Event Hub Connection String** (Строка подключения концентратора событий) на веб-странице вставьте строку подключения, скопированную в разделе о [принудительной отправке событий](#push-events).
  
    [![Вставьте строку подключения первичного ключа в поле Строка подключения концентратора событий](media/send-events/updated_two.png)](media/send-events/updated_two.png#lightbox)

1. Выберите **Click to start** (Запустить). Симулятор создаст JSON-файл экземпляра, который вы можете использовать напрямую.

1. Перейдите к концентратору событий на портале Azure. На **Обзор** страницы, вы увидите новые события, которые получены в концентратор событий.

    [![На странице Обзор концентратора событий, показывающий метрики для концентратора событий](media/send-events/telemetry.png)](media/send-events/telemetry.png#lightbox)

## <a name="json"></a>Поддерживаемые формы JSON

### <a name="example-one"></a>Первый пример

* **Входные данные** Простой объект JSON.

    ```JSON
    {
        "id":"device1",
        "timestamp":"2016-01-08T01:08:00Z"
    }
    ```

* **Выходные данные**: Одно событие.

    |id|timestamp|
    |--------|---------------|
    |device1|2016-01-08T01:08:00Z|

### <a name="example-two"></a>Второй пример

* **Входные данные** Массив JSON с двумя объектами JSON. Каждый объект JSON преобразуется в событие.

    ```JSON
    [
        {
            "id":"device1",
            "timestamp":"2016-01-08T01:08:00Z"
        },
        {
            "id":"device2",
            "timestamp":"2016-01-17T01:17:00Z"
        }
    ]
    ```

* **Выходные данные**: Два события.

    |id|timestamp|
    |--------|---------------|
    |device1|2016-01-08T01:08:00Z|
    |device2|2016-01-08T01:17:00Z|

### <a name="example-three"></a>Третий пример

* **Входные данные** Объект JSON со вложенным массивом JSON, содержащий два объекта JSON.

    ```JSON
    {
        "location":"WestUs",
        "events":[
            {
                "id":"device1",
                "timestamp":"2016-01-08T01:08:00Z"
            },
            {
                "id":"device2",
                "timestamp":"2016-01-17T01:17:00Z"
            }
        ]
    }
    ```

* **Выходные данные**: Два события. Свойство **location** копируется для каждого события.

    |location|events.id|events.timestamp|
    |--------|---------------|----------------------|
    |WestUs|device1|2016-01-08T01:08:00Z|
    |WestUs|device2|2016-01-08T01:17:00Z|

### <a name="example-four"></a>Пример 4

* **Входные данные** Объект JSON со вложенным массивом JSON, содержащий два объекта JSON. Эти входные данные указывают на то, что глобальные свойства могут быть представлены сложным объектом JSON.

    ```JSON
    {
        "location":"WestUs",
        "manufacturer":{
            "name":"manufacturer1",
            "location":"EastUs"
        },
        "events":[
            {
                "id":"device1",
                "timestamp":"2016-01-08T01:08:00Z",
                "data":{
                    "type":"pressure",
                    "units":"psi",
                    "value":108.09
                }
            },
            {
                "id":"device2",
                "timestamp":"2016-01-17T01:17:00Z",
                "data":{
                    "type":"vibration",
                    "units":"abs G",
                    "value":217.09
                }
            }
        ]
    }
    ```

* **Выходные данные**: Два события.

    |location|manufacturer.name|manufacturer.location|events.id|events.timestamp|events.data.type|events.data.units|events.data.value|
    |---|---|---|---|---|---|---|---|
    |WestUs|manufacturer1|EastUs|device1|2016-01-08T01:08:00Z|pressure|psi|108.09|
    |WestUs|manufacturer1|EastUs|device2|2016-01-08T01:17:00Z|vibration|abs G|217.09|

## <a name="next-steps"></a>Дальнейшие действия

- [Просмотрите свою среду](https://insights.timeseries.azure.com) в обозревателе Time Series Insights.
