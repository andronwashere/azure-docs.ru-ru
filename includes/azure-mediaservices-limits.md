---
author: rothja
ms.service: billing
ms.topic: include
ms.date: 11/09/2018
ms.author: jroth
ms.openlocfilehash: de3fd8dc0d45ea10e64af8e2258682a9e98639dc
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/18/2019
ms.locfileid: "67185668"
---
>[!NOTE]
>Для ресурсов, которые не исправлены чтобы запросить увеличение квоты в службу поддержки. Не создавайте дополнительные учетные записи служб мультимедиа Azure, пытаясь расширить ограничения.

| Ресурс | Ограничение по умолчанию | 
| --- | --- | 
| Azure учетные записи служб мультимедиа в рамках одной подписки | 25 (фиксированное значение) |
| Зарезервированные единицы на одну учетную запись служб мультимедиа мультимедиа |25 (S1)<br/>10 (S2, S3)<sup>1</sup> | 
| Число заданий на учетную запись платформы "Cлужбы мультимедиа" | 50 000<sup>2</sup> |
| Цепных задач для задания | 30 (фиксированное значение) |
| Ресурсы каждой учетной записи служб мультимедиа | 1 000 000|
| 50 ресурсов на задачу | 50 |
| Ресурсов на задание | 100 |
| Уникальных указателей, связанных с ресурсом за один раз | 5<sup>4</sup> |
| Каналы в прямом эфире на одну учетную запись служб мультимедиа |5|
| Программ в остановленном состоянии на канал |50|
| Программ в запущенном состоянии на канал |3|
| Конечные точки, работа которых остановлена потоковой передачи или запуск на одну учетную запись служб мультимедиа|2|
| Единиц потоковой передачи на конечную точку потоковой передачи |10 |
| Учетные записи хранения | 1000<sup>5</sup> (фиксированное значение) |
| Политики | 1 000 000<sup>6</sup> |
| Размер файла| В некоторых сценариях есть ограничение на максимальный размер файла для обработки в службах мультимедиа. <sup>7</sup> |

<sup>1</sup>при изменении типа, например, с S2 на S1, максимальное зарезервированных единиц ограничения будут сброшены.

<sup>2</sup>в том числе в очереди, завершения работы, активные и отмененные задания. Она не включает удаленные задания. Можно удалить старые задания с помощью **IJob.Delete** или **удалить** HTTP-запроса.

Начиная с 1 апреля 2017 г. все записи задания в вашей учетной записи, старше 90 дней автоматически удаляется, и с ней записи задач. Автоматическое удаление происходит, даже если общее число записей превышает значение максимальной квоты. Чтобы архивировать данные заданий и задач, используйте код, описанный в [управление ресурсами с помощью пакета SDK .NET служб мультимедиа](../articles/media-services/previous/media-services-dotnet-manage-entities.md).

<sup>3</sup>при выполнении запроса в сущности задания списка на запрос возвращается максимум 1000 заданий. Чтобы хранить список все отправленные задания, используется верхняя или пропустить запросы, как описано в разделе [системные параметры запросов OData](/previous-versions/dynamicscrm-2015/developers-guide/gg309461(v=crm.7)).

<sup>4</sup>указатели не предназначены для управления доступом пользователя. Для предоставления различных прав доступа отдельным пользователям, используйте цифровыми правами (DRM) управления решения. Дополнительные сведения см. в разделе [защитить содержимое с помощью служб мультимедиа Azure](../articles/media-services/previous/media-services-content-protection-overview.md).

<sup>5</sup>учетные записи хранения должно быть из той же подписке Azure.

<sup>6</sup>имеется ограничение в 1 000 000 записей для разных политик служб мультимедиа. Например, для политики Locator или ContentKeyAuthorizationPolicy. 

>[!NOTE]
> Если всегда использовать те же дни и разрешения доступа, используйте тот же идентификатор политики. Сведения и пример см. в разделе [управление ресурсами с помощью пакета SDK .NET служб мультимедиа](../articles/media-services/previous/media-services-dotnet-manage-entities.md#limit-access-policies).

<sup>7</sup>максимальный размер, поддерживаемый для одного большого двоичного объекта в данный момент — до 5 ТБ в хранилище BLOB-объектов. В службах мультимедиа, в зависимости от размеров виртуальных Машин, которые используются службой применяются дополнительные ограничения. Предельный размер применяется к переданные файлы, а также файлы, которые создаются в результате медиаслужбы обработку (кодирование или анализ). Если размер исходного файла превышает 260 ГБ, скорее всего, задание завершится ошибкой. 

В следующей таблице показаны ограничения на мультимедиа зарезервированные единицы S1, S2 и S3. Если исходный файл превышает ограничения, указанные в таблице, происходит сбой задания кодирования. При кодировании источники с разрешением 4K длительные, вам потребуется использовать S3 зарезервированных единиц мультимедиа для достижения необходимой производительности. При наличии содержимого 4K, размер которых превышает ограничение в 260 ГБ для S3 зарезервированных единиц мультимедиа, свяжитесь с нами по адресу amshelp@microsoft.com для возможные способы их устранения для поддержки вашего сценария.

|Зарезервированные единицы типа мультимедиа   |Максимальный размер входных данных (ГБ)|
|---|---|
|S1 |   26|
|S2 | 60|
|S3 |260|
