---
title: Квоты и ограничения платформы "Службы мультимедиа Azure" версии 3 | Документация Майкрософт
description: В этом разделе описываются квоты и ограничения платформы "Службы мультимедиа Azure" версии 3.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 05/16/2019
ms.author: juliako
ms.openlocfilehash: 709ed293dbb0550dc1bb43bf1e1e1cc50906cc31
ms.sourcegitcommit: 2d3b1d7653c6c585e9423cf41658de0c68d883fa
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/20/2019
ms.locfileid: "67293441"
---
# <a name="quotas-and-limitations-in-azure-media-services-v3"></a>Квоты и ограничения платформы "Службы мультимедиа Azure" версии 3

В этой статье описаны квоты и ограничения платформы "Службы мультимедиа Azure" версии 3.

| Ресурс | Ограничение по умолчанию | 
| --- | --- | 
| Число ресурсов на учетную запись "Службы мультимедиа Azure". | 1 000 000|
| Фильтры динамических манифестов|100|
| Число входных данных на задание | 50 (фиксированное значение)|
| Число выходных данных на задание | 20 (фиксированное значение) |
| TransformOutputs преобразования | 20 (фиксированное значение) |
| Количество файлов на JobInput|10 (фиксированное значение)|
| Размер файла| В некоторых ситуациях применяется ограничение на максимальный размер файла, который могут обработать службы мультимедиа. <sup>(1)</sup> |
| Число заданий на учетную запись платформы "Cлужбы мультимедиа" | 500 000 <sup>(2)</sup> (фиксированное значение)|
| Вывод списка преобразований|Постраничный список, 1000 преобразований на каждой странице|
| Вывод списка заданий|Постраничный список, 500 заданий на каждой странице|
| Число трансляций на учетную запись Cлужб мультимедиа |5|
| Число учетных записей платформы "Cлужбы мультимедиа" в одной подписке | 25 (фиксированное значение) |
| Live выходных данных для события прямой трансляции |3 <sup>(3)</sup> |
| Выходные данные Live Максимальная длительность | 25 часов |
| Учетные записи хранения | 100<sup>(4)</sup> (фиксированное значение) |
| Число конечных точек потоковой передачи (остановленных или запущенных) учетной записи Служб мультимедиа|2 (фиксировано)|
| Политики потоковой передачи | 100 <sup>(5)</sup> |
| Число преобразований на учетную запись платформы "Cлужбы мультимедиа" | 100 (фиксированное значение)|
| Число уникальных указателей потоковой передачи, одновременно связанных с ресурсом | 100<sup>(6)</sup> (фиксировано) |
| Параметры каждой содержимого ключа политики |30 | 
| Лицензии в месяц для каждого типа DRM в службах мультимедиа ключ службы доставки на одну учетную запись|1 000 000|

<sup>1</sup> Максимальный размер, поддерживаемый для одного большого двоичного объекта, сейчас составляет до 5 ТБ в хранилище BLOB-объектов Azure. В службах мультимедиа, в зависимости от размеров виртуальных Машин, которые используются службой применяются дополнительные ограничения. Предельный размер применяется к переданные файлы, а также файлы, которые создаются в результате медиаслужбы обработку (кодирование или анализ). Если размер исходного файла превышает 260 ГБ, скорее всего, задание завершится ошибкой. 

В следующей таблице показаны ограничения на мультимедиа зарезервированные единицы S1, S2 и S3. Если исходный файл превышает ограничения, указанные в таблице, происходит сбой задания кодирования. При кодировании источники с разрешением 4K длительные, вам потребуется использовать S3 зарезервированных единиц мультимедиа для достижения необходимой производительности. При наличии содержимого 4K, размер которых превышает ограничение в 260 ГБ для S3 зарезервированных единиц мультимедиа, свяжитесь с нами по адресу amshelp@microsoft.com для возможные способы их устранения для поддержки вашего сценария.

|Зарезервированные единицы типа мультимедиа   |Максимальный размер входных данных (ГБ)|
|---|---|
|S1 |   26|
|S2 | 60|
|S3 |260|

<sup>2</sup> Это число включает в себя задания в очереди, завершенные, активные и отмененные задания. Удаленные задания не учитываются. 

Все записи заданий в вашей учетной записи старше 90 дней будут автоматически удалены, даже если общее число записей не превышает значение максимальной квоты. 

<sup>3</sup> live выходные данные при создании начисление платы прекращается при удалении.

<sup>4</sup> Учетные записи хранения должны быть из той же подписки Azure.

<sup>5</sup> при использовании пользовательской [потоковой передачи политики](https://docs.microsoft.com/rest/api/media/streamingpolicies), следует разработать ограниченный набор таких политик для учетной записи службы мультимедиа и их повторного использования для вашей StreamingLocators всякий раз, когда же шифрования параметров и протоколов необходимы. Вы не должны создавать новую политику потоковой передачи для каждого указателя потоковой передачи.

<sup>6</sup> потоковой передачи указатели не предназначены для управления доступом пользователя. Для предоставления различных прав доступа отдельным пользователям используйте решения для управления цифровыми правами (DRM).

## <a name="support-ticket"></a>Запрос в службу поддержки

Для ресурсов, которые не являются фиксированными, можно попросить увеличить квоты, отправив [запрос в службу поддержки](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest). Укажите в запросе подробную информацию о необходимых изменениях квоты, сценариях использования и регионах. <br/>**Не** создавайте дополнительные учетные записи служб мультимедиа Azure, пытаясь расширить ограничения.

## <a name="next-steps"></a>Дальнейшие действия

[Обзор набора средств Visual Studio для Unity](media-services-overview.md)
