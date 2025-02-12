---
title: Геоизбыточное аварийное восстановление — Центры событий Azure | Документация Майкрософт
description: Способы использования географических регионов для отработки отказа и аварийного восстановления в Центрах событий Azure
services: event-hubs
documentationcenter: ''
author: ShubhaVijayasarathy
manager: timlt
editor: ''
ms.service: event-hubs
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.custom: seodec18
ms.date: 12/06/2018
ms.author: shvija
ms.openlocfilehash: a1dafb8e4c16a59bfed51016ce9ccb0ec3eb7d6c
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "66754765"
---
# <a name="azure-event-hubs---geo-disaster-recovery"></a>Геоизбыточное аварийное восстановление в службе "Центры событий Azure" 

При простое целых регионов Azure или центров обработки данных (если не используются [зоны доступности](../availability-zones/az-overview.md)) крайне важно, чтобы обработка данных продолжала работать в другом регионе или центре обработки данных. Таким образом *географическое аварийное восстановление* и *георепликация* являются важными функциями для любого предприятия. Центры событий Azure поддерживают географическое аварийное восстановления и георепликацию на уровне пространства имен. 

Функция географического аварийного восстановления глобально доступна для концентраторы событий уровня Standard и выделенные SKU. Обратите внимание на то, что вы можете только пространства имен геосвязи через один и тот же уровень SKU. К примеру Если в кластере, в которой предлагается только на наших SKU выделенного пространства имен, его можно только составлять пару с пространства имен в другой кластер. 

## <a name="outages-and-disasters"></a>Сбои и аварийные ситуации

Важно отметить различия между "сбоями" и "авариями". *Сбой* — это временная недоступность Центров событий Azure. Он может повлиять на некоторые компоненты службы, такие как хранилище сообщений, или даже весь центр обработки данных. Однако после устранения проблемы Центры событий снова становятся доступны. Как правило, простой не приводит к утрате сообщений или других данных. Примером такого сбоя может быть сбой питания в центре обработки данных. Некоторые сбои являются всего лишь короткими потерями подключения из-за временных или сетевых проблем. 

*Авария* определяется как полная или долговременная потеря кластера Центров событий, региона Azure или центра обработки данных. Регион или центр обработки данных может снова стать доступным, но может и не стать или быть отключенным в течение нескольких часов или дней. Примеры аварий — пожар, наводнение или землетрясение. Длительное аварийное состояние может привести к потере некоторых сообщений, событий или других данных. Но в большинстве случаев потеря данных не происходит и сообщения можно восстановить сразу же после резервного копирования в центре обработки данных.

Функция географического аварийного восстановления Центров событий Azure — это решение для аварийного восстановления. Основные понятия и рабочий процесс, описанные в этой статье, относятся к сценариям восстановления после аварий, но не временных сбоев. Дополнительные сведения об аварийном восстановлении в Microsoft Azure см. в статье [Аварийное восстановление для приложений на платформе Azure](/azure/architecture/resiliency/disaster-recovery-azure-applications).

## <a name="basic-concepts-and-terms"></a>Основные понятия и термины

Функция аварийного восстановления реализует аварийное восстановление метаданных и основывается на первичном и вторичном пространстве имен для аварийного восстановления. Обратите внимание, что функция географического аварийного восстановления доступна только для [SKU "Стандартный"](https://azure.microsoft.com/pricing/details/event-hubs/). Вам не нужно изменять какие-либо строки подключения, так как соединение выполняется через псевдоним.

В этом руководстве используются термины, представленные ниже.

-  *Псевдоним*. Имя настроенной конфигурации аварийного восстановления. Псевдоним предоставляет единую стабильную строку подключения полного доменного имени (FQDN). Приложения используют ее для подключения к пространству имен. 

-  *Основное или дополнительное пространство имен*. Пространство имен, соответствующее псевдониму. Основное пространство имен является "активным" и получает сообщения (это может быть существующее или новое пространство имен). Дополнительное пространство имен является "пассивным" и не получает сообщений. Метаданные между ними синхронизированы, поэтому они могут беспрепятственно принимать сообщения без каких-либо изменений кода или строки подключения приложения. Чтобы убедиться, что только активное пространство имен получает сообщения, необходимо использовать псевдоним. 

-  *Метаданные*. Сущности, такие как концентраторы событий и группы объектов-получателей, и их свойства службы, связанные с пространством имен. Обратите внимание, что только сущности и их параметры реплицируются автоматически. Сообщения и события не реплицируются. 

-  *Отработка отказа*. Процесс активации дополнительного пространства имен.

## <a name="setup-and-failover-flow"></a>Настройка и поток отработки отказа

Здесь приведен обзор процесса отработки отказа и объясняется, как настроить начальную отработку отказа. 

![1][]

### <a name="setup"></a>Настройка

Сначала создается или используется существующее основное пространство имен и новое дополнительное пространство имен, а затем они связываются. Это связывание предоставит псевдоним, который можно использовать для подключения. Так как используется псевдоним, нет необходимости изменять строки подключения. Только новые пространства имен могут быть добавлены к вашему связыванию. Наконец, следует добавить некоторые функции мониторинга, чтобы определить, необходимо ли выполнять отработку отказа. В большинстве случаев служба — это одна из частей большой экосистемы, поэтому автоматический переход на другой ресурс не всегда возможен, так как часто отработки отказа следует выполнять при синхронизации с оставшимися подсистемами или инфраструктурой.

### <a name="example"></a>Пример

Рассмотрим пример сценария с решением точки розничной продажи (POS), которое создает сообщения и события. Центры событий передают эти события решению сопоставления или переформатирования, которое затем переадресовывает сопоставленные данные в другую систему для дальнейшей обработки. В этот момент все эти системы могут размещаться в одном регионе Azure. Решение о том, когда и для каких частей выполнять отработку отказа, зависит от потока данных в инфраструктуре. 

Отработку отказа можно автоматизировать с помощью систем мониторинга или пользовательских решений для мониторинга. Однако такая автоматизация требует дополнительного планирования и работы, что выходит за рамки данной статьи.

### <a name="failover-flow"></a>Поток отработки отказа

Если вы инициируете отработку отказа, необходимо выполнить два шага:

1. В случае другого сбоя необходимо иметь возможность повторной отработки отказа. Поэтому настройте другое пассивное пространство имен и обновите связывание. 

2. Извлеките сообщения из прежнего основного пространства имен, как только оно станет доступным. После этого используйте это пространство имен для регулярного обмена сообщениями вне настройки географического восстановления или удалите старое основное пространство имен.

> [!NOTE]
> Поддерживается только семантика переадресации сбоя. В этом сценарии выполняется отработка отказа, а затем повторное связывание с новым пространством имен. Восстановление размещения не поддерживается, к примеру, в кластере SQL. 

![2][]

## <a name="management"></a>Управление

Если допущена ошибка, например, во время первоначальной настройки связаны неправильные регионы, связь двух пространств имен можно разорвать в любое время. Если вы хотите использовать сопряженные пространства имен в качестве обычных пространств имен, удалите псевдоним.

## <a name="samples"></a>Примеры

В [примере из GitHub](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/GeoDRClient) показано, как настроить и инициировать отработку отказа. В примере демонстрируются следующие понятия:

- параметры, необходимые в Azure Active Directory для использования Azure Resource Manager с Центрами событий; 
- шаги, необходимые для выполнения примера кода; 
- отправка и получение из текущего основного пространства имен; 

## <a name="considerations"></a>Рекомендации

Обратите внимание на следующие моменты для этого выпуска:

1. При планировании отработки отказа следует также учитывать фактор времени. Например, в случае потери подключения на более чем 15–20 минут можно запустить отработку отказа. 
 
2. Тот факт, что данные не реплицируются, означает, что в настоящее время активные сеансы не реплицируются. Кроме того, обнаружение дубликатов и запланированные сообщения могут не работать. Новые сеансы, запланированные сообщения и новые дубликаты будут работать. 

3. Отработку отказа сложной распределенной инфраструктуры необходимо [протестировать](/azure/architecture/reliability/disaster-recovery#disaster-recovery-plan) по крайней мере один раз. 

4. Синхронизация сущностей может занять некоторое время (примерно 50–100 сущностей в минуту).

## <a name="availability-zones"></a>зоны доступности; 

Номер SKU уровня "Стандартный" для Центров событий поддерживает функцию [Зоны доступности](../availability-zones/az-overview.md), предоставляя изолированные от сбоев расположения в регионе Azure. 

> [!NOTE]
> Поддержка Зон доступности Центров событий Azure ценовой категории "Стандартный" доступна только в [регионах Azure](../availability-zones/az-overview.md#services-support-by-region), в которых представлены зоны доступности.

Вы можете включить эту функцию только в новых пространствах имен с помощью портала Azure. Центры событий не поддерживает перенос существующих пространств имен. Вы не можете отключить избыточность в пределах зоны после ее включения в пространстве имен.

![3][]

## <a name="next-steps"></a>Дальнейшие действия

* В разделе с [примером на сайте GitHub](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/GeoDRClient) рассматривается простой рабочий процесс, который создает геосвязь и инициирует отработку отказа для сценария аварийного восстановления.
* В [справочнике для REST API](/rest/api/eventhub/disasterrecoveryconfigs) описываются программные интерфейсы, позволяющие настроить географическое аварийное восстановление.

Дополнительные сведения о Центрах событий см. по следующим ссылкам:

* Начало работы с помощью [учебника по Центрам событий](event-hubs-dotnet-standard-getstarted-send.md)
* [Часто задаваемые вопросы о Центрах событий](event-hubs-faq.md)
* [Примеры приложений, использующих Центры событий](https://github.com/Azure/azure-event-hubs/tree/master/samples)

[1]: ./media/event-hubs-geo-dr/geo1.png
[2]: ./media/event-hubs-geo-dr/geo2.png
[3]: ./media/event-hubs-geo-dr/eh-az.png
