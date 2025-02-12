---
ms.topic: include
ms.service: time-series-insights
author: kingdomofends
ms.author: adgera
ms.date: 04/29/2019
ms.openlocfilehash: 8a3c630b54ff95a9b1200e2421c787a514a0aa52
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "66431039"
---
## <a name="business-disaster-recovery"></a>Непрерывность бизнес-процессов и аварийное восстановление

В этом разделе описываются возможности Azure Time Series Insights, чтобы не приложений и служб, запущенных, даже если произойдет авария (известный как *аварийного восстановления для бизнеса*).

### <a name="high-availability"></a>Высокий уровень доступности

Как службу Azure Time Series Insights предоставляет определенные *высокий уровень доступности* функции с помощью избыточных данных на уровне региона Azure. Например, Azure поддерживает возможности по аварийному восстановлению посредством Azure *межрегиональной доступностью* функции.

Предоставляют дополнительные возможности высокого уровня доступности, обеспечивается за счет Azure (и также доступна в любой экземпляр Time Series Insights).

- **Отработка отказа**. Azure предоставляет [георепликации и балансировка нагрузки](https://docs.microsoft.com/azure/architecture/resiliency/recovery-loss-azure-region).
- **Восстановление данных** и **восстановления хранилища**: Azure предоставляет [несколько вариантов для сохранения и восстановления данных](https://docs.microsoft.com/azure/architecture/resiliency/recovery-data-corruption).
- **Восстановление сайтов**: Azure предоставляет возможности восстановления сайта через [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/).

Убедитесь, что включить соответствующие функции Azure для обеспечения высокой доступности глобального, между регионами для устройств и пользователей.

> [!NOTE]
> Если обеспечить доступность между регионами Azure, не требуется никакой настройки дополнительных доступности между регионами в Azure Time Series Insights.

### <a name="iot-and-event-hubs"></a>Интернета вещей и концентраторы событий

Некоторые службы Azure IoT также включать встроенные бизнеса функции аварийного восстановления:

- [Высокого уровня доступности аварийное восстановление центра Интернета вещей](https://docs.microsoft.com/azure/iot-hub/iot-hub-ha-dr), включая избыточности внутри регионов
- [Политики концентраторов событий](https://docs.microsoft.com/azure/event-hubs/event-hubs-geo-dr)
- [Репликация службы хранилища Azure](https://docs.microsoft.com/azure/storage/common/storage-redundancy)

Интеграция Time Series Insights с другими службами обеспечивает аварийное дополнительные возможности восстановления. Например данные телеметрии, отправляемые в концентратор событий может быть сохранена в резервной копии базы данных хранилища BLOB-объектов Azure.

### <a name="time-series-insights"></a>Аналитика временных рядов

Существует несколько способов защиты данных Time Series Insights, приложения и службы, запущенные, даже если они при сбое. 

Тем не менее можно предположить, что полная резервная копия среды аналитика временных рядов Azure также требуется, для следующих целей:

- Как *экземпляра отработки отказа* специально для Time Series Insights перенаправлять данные и трафик
- Чтобы сохранить данные и данные аудита

Как правило лучшим способом для дублирования среды Time Series Insights является создание второй среде Time Series Insights в резервной копии регион Azure. События также отправляются в этой вторичной среды из вашего источника событий первичной. Убедитесь в том, использовать группу потребителей, во-вторых, выделенный. Рекомендации этого источника бизнеса аварийного восстановления, как описано выше.

Чтобы создать среду:

1. Создайте среду во втором регионе. Дополнительные сведения см. в разделе [создания новой среды Time Series Insights на портале Azure](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-get-started).
1. Создайте для вашего источника событий вторую выделенную группу потребителей.
1. Подключите этот источник событий к новой среде. Убедитесь, что назначить группу получателей, во-вторых, выделенный.
1. Просмотрите Time Series Insights [центра Интернета вещей](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-how-to-add-an-event-source-iothub) и [концентраторов событий](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-data-access) документации.

При возникновении события:

1. Если во время инцидента ваш основной регион дал сбой, переключите операции на резервную среду службы "Аналитика временных рядов".
1. Используйте ваши во второй регион для резервного копирования и восстановления любых данных телеметрии и запроса аналитики временных рядов.

> [!IMPORTANT]
> При отработке отказа:
> 
> * Кроме того, возможна некоторая задержка.
> * Небольшой пик при обработке сообщений могут возникнуть, так как операции, пересылаются.
> 
> Дополнительные сведения см. в статье [Отслеживание и уменьшение регулирования для сокращения задержек в службе "Аналитика временных рядов Azure"](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-environment-mitigate-latency).

