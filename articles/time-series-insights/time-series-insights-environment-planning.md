---
title: Подготовка к масштабированию среды Аналитики временных рядов Azure | Документация Майкрософт
description: В этой статье описывается, как важно Следование рекомендациям при планировании среды аналитики временных рядов Azure. Области, которые рассматриваются включают емкость хранилища, хранение данных, объем входящих данных, мониторинг и бизнеса непрерывности и аварийного восстановления (BCDR).
services: time-series-insights
ms.service: time-series-insights
author: ashannon7
ms.author: dpalled
manager: cshankar
ms.reviewer: v-mamcge, jasonh, kfile
ms.devlang: csharp
ms.workload: big-data
ms.topic: conceptual
ms.date: 04/29/2019
ms.custom: seodec18
ms.openlocfilehash: 2c11e3f623817894cea801173239cc386c6c3313
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/17/2019
ms.locfileid: "67165838"
---
# <a name="plan-your-azure-time-series-insights-ga-environment"></a>Планирование среды времени серии Azure Insights-общедоступной версии

В этой статье описывается планирование среды Azure Time Series Insights общедоступная (GA), на основе скорости передачи ожидаемого объема входящих данных и требуемого срока хранения данных.

## <a name="video"></a>Видео

**Просмотрите это видео, чтобы узнать больше о хранении данных в Azure Time Series Insights, а также планирование для него**:<br /><br />

> [!VIDEO https://www.youtube.com/embed/03x6zKDQ6DU]

## <a name="best-practices"></a>Рекомендации

Чтобы приступить к работе с Time Series Insights, лучше, если вы знаете, какой объем данных, передаваемых по минутам и срок хранения данных.  

Дополнительные сведения о емкости и сроках хранения для двух предложений Аналитики временных рядов см. в описании [цен на Аналитику временных рядов](https://azure.microsoft.com/pricing/details/time-series-insights/).

Лучше всего планирование среды аналитики временных рядов для долговременного успеха, рассмотрим следующие атрибуты:

- <a href="#storage-capacity">Емкость хранилища</a>
- <a href="#data-retention">Срок хранения данных</a>
- <a href="#ingress-capacity">Объем входящих данных</a>
- <a href="#shape-your-events">Формирование событий</a>
- <a href="#ensure-that-you-have-reference-data">Убедившись в наличии ссылочных данных на месте</a>

## <a name="storage-capacity"></a>Емкость хранилища

По умолчанию аналитика временных рядов определяет данные, основанные на объем хранилища, вы подготавливаете (единицы &#215; объем хранилища на единицу) и входящих данных.

## <a name="data-retention"></a>Хранение данных

Вы можете изменить **время хранения данных** установкой в среде Time Series Insights. Вы можете включить до 400 дней хранения. 

Аналитика временных рядов имеет два режима. Один режим оптимизирует за обеспечение наличия самые актуальные данные. Этот режим включен, по умолчанию. 

Другой режим оптимизирует обеспечивающие соблюдение ограничения. В втором режиме прием входящих данных приостанавливается удовлетворяется емкости хранилища среды. 

Вы можете настроить время хранения и режим между двумя режимами на странице конфигурации среды на портале Azure.

В среде Аналитики временных рядов вы можете указать время хранения не более 400 дней.

### <a name="configure-data-retention"></a>Настройка времени хранения данных

1. Выберите нужную среду Аналитики временных рядов на [портале Azure](https://portal.azure.com).

1. В **среды Time Series Insights** панели в разделе **параметры**выберите **Настройка**.

1. В **время хранения данных (в днях)** введите значение от 1 до 400.

   [![Настройка хранения](media/environment-mitigate-latency/configure-retention.png)](media/environment-mitigate-latency/configure-retention.png#lightbox)

> [!TIP]
> Дополнительные сведения о том, как реализовать соответствующую политику хранения данных, см. в разделе [как настроить период удержания](./time-series-insights-how-to-configure-retention.md).

## <a name="ingress-capacity"></a>Объем входящих данных

Вторая область уделено для планирования среды Time Series Insights — объем входящих данных. Объем входящих данных является производным от основе поминутного распределения.

С точки зрения регулирования входящий пакет данных, размер пакета 32 КБ рассматривается как 32 события, каждый размером 1 КБ. Максимальный допустимый размер события составляет 32 КБ. Пакеты данных, размер которых превышает 32 КБ, усекаются.

В следующей таблице перечислены объем входящих данных на единицу для каждого Time Series Insights номером SKU:

|SKU  |Число событий в месяц  |Размер события в месяц  |Число событий в минуту  |Размер событий в минуту  |
|---------|---------|---------|---------|---------|
|S1     |   30 млн     |  30 ГБ     |  720    |  720 КБ   |
|S2     |   300 млн    |   300 ГБ   | 7200   | 7200 КБ  |

В одной среде допускается до 10 единиц SKU S1 и S2. Нельзя перенести из среды S1 на S2. Нельзя перенести в среду S2 на S1.

Для объем входящих данных необходимо сначала определите общее количество входящих, требуемого на весь месяц. Далее определите требуемую скорость в минуту потребностей. 

Регулирование и задержка играют роль в минуту емкости. При наличии пик в ваш объем входящих данных, которая длится менее 24 часов, аналитика временных рядов можно «Догнать» скоростью входящих данных из два раза ценам, указанным в предыдущей таблице.

Например если у вас есть один номер SKU S1, вы входящих данных со скоростью 720 событий в минуту, и скорость передачи данных, резко возрастает для менее одного часа со скоростью до 1440 события человек, нет не заметную задержку в вашей среде. Тем не менее если вы превысите 1440 событий в минуту в течение более чем за один час, скорее всего, возникнет задержка в данных для визуализации и доступны для запросов в вашей среде.

Вы можете не знать заранее, какой объем данных предполагается, что для принудительной отправки. В этом случае можно найти данные телеметрии для [центра Интернета вещей Azure](https://docs.microsoft.com/azure/iot-hub/iot-hub-metrics) и [концентраторов событий](https://blogs.msdn.microsoft.com/cloud_solution_architect/2016/05/25/using-the-azure-rest-apis-to-retrieve-event-hub-metrics/) в вашей подписки на портале Azure. Данные телеметрии могут помочь определить способ подготовки среды. Используйте **метрики** область на портале Azure для соответствующего источника просмотреть данные телеметрии. Изучив метрики для источника событий вы сможете более эффективно спланировать и подготовить среду Аналитики временных рядов.

### <a name="calculate-ingress-requirements"></a>Расчет требований к объему входящих данных

Для вычисления требований к входящего трафика:

- Убедитесь, что объем входящих данных превышает среднее скорость в минуту и среды является способна выдержать пиковые значения скорости эквивалентно два раза больше емкости для менее одного часа.

- Если пиковые, последнего дольше, чем 1 час, использующих пиковое качестве среднего. Подготовка среды с мощности для обслуживания пиковое.

### <a name="mitigate-throttling-and-latency"></a>Устранение регулирования и задержек

Сведения о том, как предотвратить задержки и регулирование, вы найдете в [этой статье](time-series-insights-environment-mitigate-latency.md).

## <a name="shape-your-events"></a>Формирование ваших событий

Важно, чтобы убедиться, что способ отправки событий Time Series Insights поддерживает размер среды, в которой при подготовке. (И наоборот, можно сопоставить размер среды с количеством событий Time Series Insights считывает и размер каждого события.) Также важно думать об атрибутах, которые можно использовать для срез и фильтрацию при запросе данных.

> [!TIP]
> Просмотрите документацию в формировании JSON [отправки событий](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-send-events).

## <a name="ensure-that-you-have-reference-data"></a>Убедитесь, что ссылочные данные

Объект *ссылка на набор данных* является коллекцией элементов, которые дополняют события из источника событий. Обработчик входящего трафика аналитики временных рядов соединяет каждое событие из источника события с соответствующей строкой данных в вашей эталонного набора данных. Дополненное событие становится доступным для запроса. Соединение основано на **первичный ключ** столбцы, определенные в вашей эталонного набора данных.

> [!NOTE]
> Ссылочные данные не ретроактивное объединение. Только текущие и будущие входящие данные сопоставляются и присоединяются к эталонному набору данных, после настройки и передачи. Если вы планируете отправлять большой объем исторических данных Time Series Insights и не первой передаче или создать ссылочные данные в Time Series Insights, может потребоваться вернуть результаты работы (подсказка: не очень забавно).  

Дополнительные сведения о том, как создание, отправка и управляйте своими данными ссылку в Time Series Insights, см. наш [справочной документации набора данных](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-add-reference-data-set).

[!INCLUDE [business-disaster-recover](../../includes/time-series-insights-business-recovery.md)]

## <a name="next-steps"></a>Дальнейшие действия

- Начните работу с создания [новой среды Time Series Insights на портале Azure](time-series-insights-get-started.md).

- Узнайте, как [добавить источник события концентраторов событий](time-series-insights-how-to-add-an-event-source-eventhub.md) для Time Series Insights.

- Читать о том, как [настроить источник событий центра Интернета вещей](time-series-insights-how-to-add-an-event-source-iothub.md).
