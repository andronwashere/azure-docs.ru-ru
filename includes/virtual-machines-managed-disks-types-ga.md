---
title: включение файла
description: включение файла
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 05/14/2019
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: 2bc5602011ed64b11b1b8c96b7e69a8d5ee9bf32
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "67133453"
---
## <a name="premium-ssd"></a>SSD (цен. категория "Премиум")

Диски SSD (цен. категория "Премиум") Azure обеспечивают диски с высокой производительностью и малой задержкой для виртуальных машин, предназначенных для рабочих нагрузок с большим числом операций ввода-вывода. Диски виртуальной машины приложения можно перенести в хранилище класса Premium на основе дисков SSD (цен. категория "Премиум"), чтобы использовать их быстродействие и производительность. Диски SSD (цен. категория "Премиум") подходят для критически важных рабочих приложений. Службы SSDs уровня "премиум" может использоваться только с серией виртуальных Машин, совместимой с хранилищем класса premium.

Дополнительные сведения об отдельных типах и размерах в Azure для Windows, включая какие размеры уровня "премиум" совместимой с хранилищем, см. в разделе [размеры виртуальных Машин Windows](../articles/virtual-machines/windows/sizes.md). Дополнительные сведения об отдельных типах и размерах в Azure для Linux, в том числе какие размеры уровня "премиум" совместимой с хранилищем, см. в разделе [размеры виртуальных Машин Linux](../articles/virtual-machines/linux/sizes.md).

### <a name="disk-size"></a>Размер диска
[!INCLUDE [disk-storage-premium-ssd-sizes](disk-storage-premium-ssd-sizes.md)]

При подготовке диска хранилища класса Premium, в отличие от хранилища класса Standard, гарантируются определенные показатели емкости, числа операций ввода-вывода в секунду и пропускной способности. Например, при создании диска P50 Azure подготавливает для него 4095 ГБ емкости хранилища, а также пропускную способность 250 МБ в секунду и 7500 операций ввода-вывода в секунду. Приложение может использовать ресурсы емкости и производительности как полностью, так и частично. Диски SSD класса Premium предназначены для обеспечения низкой миллисекунд задержки и предназначенных для операций ввода-ВЫВОДА и пропускную способность, описанные в предыдущей таблице 99,9% времени.

### <a name="transactions"></a>Транзакции

Для уровня "премиум" SSD при каждой операции ввода-вывода меньше или равно 256 Киб пропускной способности, считается за одну операцию ввода-вывода. Операций ввода-вывода, размер которых превышает 256 Киб пропускной способности, считаются несколько операций ввода-вывода размером 256 Киб.

## <a name="standard-ssd"></a>SSD (цен. категория "Стандартный")

Диски SSD (цен. категория "Стандартный") Azure — это экономичный вариант хранилища, оптимизированный для рабочих нагрузок, требующих стабильной производительности при более низких объемах операций ввода-вывода в секунду. Диски SSD уровня "Стандартный" предоставляют хорошее взаимодействие начального уровня для тех, кто хочет перейти в облако. Это особенно актуально, если возникают проблемы с расхождением рабочих нагрузок, выполняющихся в ваших решениях HDD на локальном компьютере. По сравнению с стандартные жесткие диски, стандартный SSDs обеспечивают более высокую доступность, согласованность, надежность и задержки. Диски SSD (цен. категория "Стандартный") подходят для веб-серверов, серверов приложений с небольшим количеством операций ввода-вывода в секунду, редко используемых корпоративных приложений и рабочих нагрузок разработки и тестирования. Как стандартные жесткие диски Стандартная SSDs доступны на всех виртуальных машинах Azure.

### <a name="disk-size"></a>Размер диска
[!INCLUDE [disk-storage-standard-ssd-sizes](disk-storage-standard-ssd-sizes.md)]

Стандартный SSDs предназначены для предоставления миллисекунд задержки и операций ввода-ВЫВОДА и пропускной способности до ограничения, описанные в предыдущей таблице 99% случаев. Фактические операций ввода-ВЫВОДА и пропускной способности иногда зависят от шаблонов трафика. Диски SSD уровня "Стандартный" обеспечивают более высокий уровень доступности и надежности, чем диски HDD с низким уровнем задержки.

### <a name="transactions"></a>Транзакции

Для стандартных SSD при каждой операции ввода-вывода меньше или равно 256 Киб пропускной способности, считается за одну операцию ввода-вывода. Операций ввода-вывода, размер которых превышает 256 Киб пропускной способности, считаются несколько операций ввода-вывода размером 256 Киб. Эти транзакции должны влияния на выставление счетов.

## <a name="standard-hdd"></a>Диск HDD (цен. категория "Стандартный")

Диски HDD (цен. категория "Стандартный") обеспечивает надежные экономичные диски для виртуальных машин, на которых выполняются рабочие нагрузки с большими задержками. Данные, переданные в хранилище ценовой категории "Стандартный", хранятся на жестких дисках (HDD). Задержка, операций ввода-ВЫВОДА и пропускной способности стандартной HDD дисков может сильно более по сравнению с дисками на основе SSD. Стандартные диски HDD предназначены для предоставления задержки при записи в разделе 10 мс и чтения задержки в 20 мс для большинства операций ввода-ВЫВОДА, однако реальная производительность варьируется в зависимости от шаблона размер и рабочую нагрузку операций ввода-ВЫВОДА. При работе с виртуальными машинами, можно использовать стандартные диски HDD для сценариев разработки и тестирования и менее важных рабочих нагрузок. Стандартные жесткие диски доступны во всех регионах Azure и может использоваться со всех виртуальных машинах Azure.

### <a name="disk-size"></a>Размер диска
[!INCLUDE [disk-storage-standard-hdd-sizes](disk-storage-standard-hdd-sizes.md)]

### <a name="transactions"></a>Транзакции

Для стандартных жестких дисков каждая операция ввода-ВЫВОДА считается как одна транзакция, независимо от объема ввода-вывода. Эти транзакции должны влияния на выставление счетов.

## <a name="billing"></a>Выставление счетов

При использовании управляемых дисков счета выставляются за следующее.

- Тип диска
- Размер управляемого диска
- Моментальные снимки
- Передача исходящих данных
- количество транзакций;

**Размер управляемого диска**. Выставление счета за использование управляемых дисков зависит от размера подготовленного диска. Служба Azure сопоставляет подготовленный размер (округленный) с ближайшим предлагаемым размером диска. Узнать предлагаемые размеры дисков можно в предыдущих таблицах. Каждый диск сопоставляется с одним из поддерживаемых подготовленных размеров дисков, и за него выставляются соответствующие счета. Например, если вы подготовили диск SSD (цен. категория "Стандартный") размером 200 ГиБ, то он будет сопоставлен с предлагаемым размера диска E15 (256 Гиб). Счета для любого подготовленного диска выставляются пропорционально с учетом часов по ежемесячной стоимости хранилища класса Premium. Например, если вы подготовили диск E10 и удалили его через 20 часов, взимается пропорциональная плата за использование диска E10 в течение 20 часов. Это происходит независимо от фактического объема данных, записанных на диск.

**Моментальные снимки**. Плата за моментальные снимки взимается в зависимости от используемой емкости. Например, если создается моментальный снимок управляемого диска с подготовленной емкостью 64 Гиб и фактическим объемом используемых данных 10 Гиб, оплачиваются только используемые данные, то есть 10 Гиб.
