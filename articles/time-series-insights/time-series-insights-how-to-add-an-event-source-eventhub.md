---
title: Добавление источника событий Центра событий в службу "Аналитика временных рядов Azure" | Документация Майкрософт
description: В этой статье описывается, как добавить источник событий, подключенный к Центрам событий Azure и к среде "Аналитика временных рядов".
ms.service: time-series-insights
services: time-series-insights
author: ashannon7
ms.author: dpalled
manager: cshankar
ms.reviewer: v-mamcge, jasonh, kfile
ms.workload: big-data
ms.topic: conceptual
ms.date: 05/01/2019
ms.custom: seodec18
ms.openlocfilehash: 21e054aefab0ee5535376ac86ebbaf1316e671b5
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/17/2019
ms.locfileid: "67165697"
---
# <a name="add-an-event-hub-event-source-to-your-time-series-insights-environment"></a>Добавление источника событий концентратора событий в среду "Аналитика временных рядов Azure"

В этой статье описывается, как добавить источник событий, считывающий данные из Центров событий Azure, в среду "Аналитика временных рядов" с помощью портала Azure.

> [!NOTE]
> Действия, описанные в этой статье относятся к общедоступной версии Insights ряда времени и предварительного просмотра аналитики во время ряда сред.

## <a name="prerequisites"></a>Технические условия

- Создание среды Time Series Insights, как описано в разделе [создание среды Azure Time Series Insights](./time-series-insights-update-create-environment.md).
- Создайте концентратор событий. См. в разделе [создать пространство имен концентраторов событий и концентратора событий с помощью портала Azure](../event-hubs/event-hubs-create.md).
- В концентратор событий должны отправляться активные события сообщений. Узнайте, как [отправки событий в концентраторы событий Azure с помощью .NET Framework](../event-hubs/event-hubs-dotnet-framework-getstarted-send.md).
- Создайте выделенную группу объектов-получателей в концентраторе событий для среды "Аналитика временных рядов". Каждый источник событий Аналитики временных рядов должен иметь собственную выделенную группу объектов-получателей, не используемую другими объектами-получателями. Если несколько модулей чтения используют события из одной группы потребителей, как правило, во всех модулях могут произойти сбои. У каждого концентратора событий должно быть не более 20 групп объектов-получателей. Дополнительные сведения см. в статье [Руководство по программированию Центров событий](../event-hubs/event-hubs-programming-guide.md).

### <a name="add-a-consumer-group-to-your-event-hub"></a>Добавление группы объектов-получателей в концентратор событий

Группы объектов-получателей используются приложениями для извлечения данных из Центров событий Azure. Чтобы надежно считывать данные из концентратора событий, укажите выделенную группу потребителей, используемый только в этой среде Time Series Insights.

Чтобы добавить новую группу объектов-получателей в концентратор событий, выполните следующие действия.

1. На портале Azure найдите и откройте концентратор событий.

1. В разделе **Сущности** щелкните **Группы потребителей**, а затем выберите **группу объектов-получателей**.

   [![Концентратор событий — Добавление группы потребителей](media/time-series-insights-how-to-add-an-event-source-eventhub/5-event-hub-consumer-group.png)](media/time-series-insights-how-to-add-an-event-source-eventhub/5-event-hub-consumer-group.png#lightbox)

1. На странице **Группы потребителей** введите новое уникальное значение для параметра **Имя**.  Используйте это же имя при создании источника событий в среде "Аналитика временных рядов Azure".

1. Нажмите кнопку **Создать**.

## <a name="add-a-new-event-source"></a>Добавление нового источника событий

1. Войдите на [портале Azure](https://portal.azure.com).

1. Найдите существующую среду "Аналитика временных рядов". В меню слева выберите **Все ресурсы**, а затем выберите среду "Аналитика временных рядов".

1. В разделе **Топология окружения** выберите **Источники событий**, а затем щелкните **Добавить**.

   [![В разделе источников событий и выберите «Добавить»](media/time-series-insights-how-to-add-an-event-source-eventhub/1-event-sources.png)](media/time-series-insights-how-to-add-an-event-source-eventhub/1-event-sources.png#lightbox)

1. Введите значение для параметра **Имя источника событий**, уникальное для этой среды "Аналитика временных рядов", например **event-stream**.

1. Для параметра **Источник** выберите значение **Концентратор событий**.

1. Выберите подходящие значения для параметра **Вариант импорта**:
   - Выберите **Использовать концентратор событий из доступных подписок**, если в одной из подписок уже есть концентратор событий. Этот вариант — самый простой подход.

       [![В области нового источника событий введите значения для первых трех аргументов](media/time-series-insights-how-to-add-an-event-source-eventhub/2-import-option.png)](media/time-series-insights-how-to-add-an-event-source-eventhub/2-import-option.png#lightbox)


       [![Сведения о концентраторе подписок и событий](media/time-series-insights-how-to-add-an-event-source-eventhub/3-new-event-source.png)](media/time-series-insights-how-to-add-an-event-source-eventhub/3-new-event-source.png#lightbox)

     Если выбран параметр **Использовать концентратор событий из доступных подписок**, см. описание каждого из обязательных свойств в следующей таблице:

     | Свойство | Описание |
     | --- | --- |
     | Идентификатор подписки | Выберите подписку, в которой был создан этот концентратор событий.
     | Пространство имен служебной шины | Выберите пространство имен Служебной шины Azure, содержащее концентратор событий.
     | Имя концентратора событий | Выберите имя концентратора событий.
     | Имя политики концентратора событий | Выберите политику общего доступа. Политику общего доступа можно создать на вкладке **Настройка** в концентраторе событий. Каждой политике общего доступа присваивается имя, а также для нее задаются разрешения и ключи доступа. Политика общего доступа для источника событий *должна* иметь разрешения на **чтение**.
     | Ключ политики концентратора событий | Значение ключа может быть предварительно заполнено.
     | Группа потребителей концентратора событий | Группа объектов-получателей, которая считывает события из концентратора событий. Мы настоятельно рекомендуем использовать выделенную группу объектов-получателей для источника событий. |
     | Формат сериализации событий | В настоящее время JSON — это единственный доступный формат сериализации. Сообщения событий должны быть в следующем формате, или не удается прочитать данные. |
     | Имя свойства для метки времени | Чтобы определить это значение, необходимо понимать формат данных сообщения, отправляемого в концентратор событий. Это значение представляет собой **имя** определенного свойства события в данных сообщения, которое нужно использовать в качестве метки времени для события. Для этого значения учитывается регистр. Если оставить значение пустым, в качестве метки времени события будет использоваться **время постановки события в очередь** в пределах источника событий. |

    - Выберите **Ввести параметры концентратора событий вручную**, если концентратор событий находится за пределами подписок или требуется выбрать дополнительные параметры.

      В следующей таблице описаны обязательные свойства для параметра **Ввести параметры концентратора событий вручную**:
 
      | Свойство | Описание |
      | --- | --- |
      | Идентификатор подписки | Подписка, в которой был создан этот концентратор событий.
      | Группа ресурсов | Группа ресурсов, в которой создан этот концентратор событий.
      | Пространство имен служебной шины | Пространство имен Service Bus — это контейнер для набора сущностей обмена сообщениями. При создании концентратора событий создается также пространство имен служебной шины.
      | Имя концентратора событий | Имя концентратора событий. При создании вы также присваиваете концентратору событий определенное имя.
      | Имя политики концентратора событий | Политика общего доступа. Политику общего доступа можно создать на вкладке **Настройка** в концентраторе событий. Каждой политике общего доступа присваивается имя, а также для нее задаются разрешения и ключи доступа. Политика общего доступа для источника событий *должна* иметь разрешения на **чтение**.
      | Ключ политики концентратора событий | Ключ общего доступа, используемый для аутентификации доступа к пространству имен Служебной шины. Введите первичный или вторичный ключ здесь.
      | Группа потребителей концентратора событий | Группа объектов-получателей, которая считывает события из концентратора событий. Мы настоятельно рекомендуем использовать выделенную группу объектов-получателей для источника событий.
      | Формат сериализации событий | В настоящее время JSON — это единственный доступный формат сериализации. Сообщения событий должны быть в следующем формате, или не удается прочитать данные. |
      | Имя свойства для метки времени | Чтобы определить это значение, необходимо понимать формат данных сообщения, отправляемого в концентратор событий. Это значение представляет собой **имя** определенного свойства события в данных сообщения, которое нужно использовать в качестве метки времени для события. Для этого значения учитывается регистр. Если оставить значение пустым, в качестве метки времени события будет использоваться **время постановки события в очередь** в пределах источника событий. |

1. Добавьте имя отдельной группы объектов-получателей Аналитики временных рядов, которое вы добавили в концентратор событий.

1. Нажмите кнопку **Создать**.

   [![Выбор создания](media/time-series-insights-how-to-add-an-event-source-eventhub/4-create-button.png)](media/time-series-insights-how-to-add-an-event-source-eventhub/4-create-button.png#lightbox)

   После создания источника событий Аналитика временных рядов автоматически запустит потоковую передачу данных в среду.

## <a name="next-steps"></a>Дальнейшие действия

* [Определите политики доступа к данным](time-series-insights-data-access.md), чтобы защитить ваши данные.

* [Отправьте события](time-series-insights-send-events.md) в источник событий.

* Получите доступ к своей среде в [обозревателе службы "Аналитика временных рядов"](https://insights.timeseries.azure.com).
