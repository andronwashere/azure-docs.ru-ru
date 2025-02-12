---
author: robinsh
ms.author: robinsh
ms.service: iot-hub
ms.topic: include
ms.date: 10/26/2018
ms.openlocfilehash: 104849557a8580e16fa1860b7919d1c0252debe9
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/18/2019
ms.locfileid: "67185682"
---
В следующей таблице перечислены ограничения, связанные с разными уровнями служб S1, S2, S3 и F1. Сведения о стоимости каждой *единицы* на каждом уровне, см. в разделе [ценами на центр Интернета вещей Azure](https://azure.microsoft.com/pricing/details/iot-hub/).

| Ресурс | Стандартный S1 | Стандартный S2 | Стандартный S3 | Бесплатный F1 |
| --- | --- | --- | --- | --- |
| Сообщений в день |400,000 |6 000 000 |300 000 000 |8000 |
| Максимальное число единиц |200 |200 |10 |1 |

> [!NOTE]
> Если требуется более 200 единиц с центром уровня S1 или S2 либо 10 единиц с центром уровня S3, обратитесь в службу поддержки Майкрософт.
> 
> 

В следующей таблице перечислены ограничения, которые применяются к ресурсам центра Интернета вещей.

| Ресурс | Ограничение |
| --- | --- |
| Максимальное число платных центров IoT на подписку Azure |50 |
| Максимальное число бесплатных центров IoT на подписку Azure |1 |
| Максимальное число символов в Идентификаторе таблицы | 128 |
| Максимальное число удостоверений устройств,<br/> возвращаемых в одном вызове |1000 |
| Максимальный срок хранения сообщений Центра Интернета вещей, передаваемых с устройства в облако |7 дней |
| Максимальный размер сообщения, отправляемого с устройства в облако |256 KB |
| Максимальный размер пакета, отправляемого с устройства в облако |AMQP и HTTP: 256 КБ для всего пакета <br/>MQTT: 256 КБ для каждого сообщения |
| Максимальное число сообщений в пакете, отправляемом с устройства в облако |500 |
| Максимальный размер сообщения, отправляемого из облака на устройство |64 КБ |
| Максимальный срок жизни для сообщений, отправляемых из облака на устройство |2 дня |
| Максимальное число доставок для сообщений, <br/> отправляемых из облака на устройство |100 |
| Максимальное число доставок для сообщений обратной связи <br/> в ответ на сообщение, отправляемое из облака на устройство |100 |
| Максимальный срок жизни для сообщений обратной связи <br/> в ответ на сообщение, отправляемое из облака на устройство |2 дня |
| [Максимальный размер двойника устройства](../articles/iot-hub/iot-hub-devguide-device-twins.md#device-twin-size) <br/> (теги, сообщаемые и требуемые свойства) | 8 КБ |
| Максимальный размер строкового значения двойника устройства | 4 КБ |
| [Максимальная глубина объекта в двойниках устройств](../articles/iot-hub/iot-hub-devguide-device-twins.md#tags-and-properties-format) | 5 |
| Максимальный размер полезных данных прямого метода | 128 КБ |
| Максимальный период удержания журнала заданий | 30 дней |
| Максимальное число параллельных заданий | 10 (S3), 5 (S2), 1 (S1) |
| Максимальное число дополнительных конечных точек | 10 (для S1, S2 и S3) |
| Максимальное число правил маршрутизации сообщений | 100 (для S1, S2 и S3) |
| Максимальное количество одновременно подключенных потоков устройств | 50 (только для S1, S2, S3 и F1) |
| Максимальная скорость передачи данных потоков устройств | 300 МБ в день (только для S1, S2, S3 и F1) |

> [!NOTE]
> Если вам требуется более чем 50 платных центров Интернета вещей в подписке Azure, обратитесь в службу поддержки Майкрософт.

> [!NOTE]
> Сейчас максимальное число устройств, которые можно подключить к одному Центру Интернета вещей, составляет 1 000 000. Если вам необходимо увеличить это ограничение, обратитесь в [службу поддержки Майкрософт](https://azure.microsoft.com/support/options/).

Центр Интернета вещей регулирует запросы при превышении следующих квот.

| Регулирование | Значение концентратора |
| --- | --- |
| операции с реестром удостоверений. <br/> (Создание, получение, перечисления, обновления и удаления), <br/> индивидуальный или массовый импорт и экспорт |83.33/sec/Unit (5000 в минуту на единицу) (для S3). <br/> 1,67 операций в секунду на единицу (100 операций в минуту на единицу; для S1 и S2). |
| Подключение устройств |6000 в секунду на единицу (для S3), 120/в секунду на единицу (для S2), 12 в секунду на единицу (для S1). <br/>Минимальное значение — 100/с. |
| Передачи с устройства в облако |6000 в секунду на единицу (для S3), 120/в секунду на единицу (для S2), 12 в секунду на единицу (для S1). <br/>Минимальное значение — 100/с. |
| Передачи из облака на устройство | 83.33/sec/Unit (5000/мин на единицу) (для S3), 1.67/sec/unit (100 в минуту на единицу) (для S1 и S2). |
| Получение из облака на устройство |833.33/sec/Unit (50 000/мин на единицу) (для S3), 16.67/sec/unit (1000 в минуту на единицу) (для S1 и S2). |
| Операции отправки файлов |83,33 файл отправки уведомлений в секунду на единицу (5000/мин на единицу) (для S3), 1,67 файл отправки уведомлений и в секунду на единицу (100 в минуту на единицу) (для S1 и S2). <br/> 10 000 SAS URI могут быть для учетной записи хранения Azure за один раз.<br/> Допускается одновременная выдача до 10 универсальных кодов ресурса (URI) SAS на устройство. |
| Прямые методы | 24 МБ в секунду/единицу (для S3), 480 КБ/с/на единицу (S2), 160 КБ/с/единицу (для S1).<br/> В зависимости от регулирования индикатор размера 8 КБ. |
| Операции чтения двойника устройства | 500 в секунду на единицу (для S3), не более 100/с или 10 в секунду на единицу (для S2), 100 в секунду (для S1) |
| Обновления двойников устройств | 250 в секунду на единицу (для S3), не более 50 в секунду или 5 в секунду на единицу (для S2), 50 в секунду (для S1) |
| Операции заданий <br/> (Создание, обновление, список и удаление) | 1\.67/sec/unit 83.33/sec/Unit (5000/мин на единицу) (для S3), (100 в минуту на единицу) (для S2), 1.67/sec/unit (100 в минуту на единицу) (для S1). |
| Пропускная способность для операций заданий на уровне отдельного устройства | 50 в секунду на единицу (для S3), не более 10 в секунду или 1 в секунду на единицу (для S2), 10 в секунду (для S1). |
| Начальная скорость потоков устройств | 5 новых потоков в секунду (для S1, S2, S3 и только F1). |
