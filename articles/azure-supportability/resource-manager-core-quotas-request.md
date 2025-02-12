---
title: Запросы на увеличение квоты виртуальных ЦП Azure Resource Manager | Документация Майкрософт
description: Запросы на увеличение квоты виртуальных ЦП Azure Resource Manager
author: sowmyavenkat86
ms.author: svenkat
ms.date: 06/07/2019
ms.topic: article
ms.service: azure
ms.assetid: ce37c848-ddd9-46ab-978e-6a1445728a3b
ms.openlocfilehash: 9a997af984b92ea59cc02d99fbd66d8967ca31bd
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "67076806"
---
# <a name="quota-increase-requests"></a>Запросы на увеличение квоты

Квоты виртуальных ЦП Resource Manager для виртуальных машин и масштабируемых наборов виртуальных машин применяются на двух уровнях для каждой подписки в каждом регионе. 

Первый уровень — это общие региональные виртуальные ЦП ограничение (по всем рядам виртуальной Машины), а второй — предел виртуальных ЦП серии виртуальных Машин (например, ЦП серии D). Каждый раз при обнаружении новой виртуальной Машины к развертыванию, сумма использования новых и существующих виртуальных ЦП для этого ряда виртуальной Машины не должен превышать квоту на виртуальные ЦП, утвержденные для этого конкретного ряда виртуальной Машины. Кроме того число общее новых и существующих виртуальных ЦП, развернутых на всех виртуальных Машин серии не должно превышать общее региональной квоты виртуальных ЦП, утвержденные для подписки. При превышении любой из этих квот развертывание виртуальной машины будет запрещено.
Вы можете запросить увеличение квоты виртуальных ЦП для серии виртуальных Машин на портале Azure. Увеличение квоты виртуальных Машин серии автоматически увеличивает общие региональные виртуальные ЦП ограничение на величину. 

При создании новой подписки, общие региональные виртуальные ЦП по умолчанию не может быть равно сумме квоты виртуальных ЦП по умолчанию для всех отдельных рядов виртуальной Машины. Это может привести в подписке с достаточной квоты для каждого отдельного ряда виртуальной Машины, который требуется выполнить развертывание, но недостаточно квот для общие региональные виртуальные ЦП для всех развертываний. В этом случае необходимо отправить запрос, чтобы увеличить лимит, общие региональные виртуальные ЦП явным образом. Ограничение общего региональные виртуальные ЦП не может превышать сумму утвержденных квоты по всем рядам виртуальной Машины для региона.

Дополнительные сведения о квотах на [странице квоты виртуальных ЦП виртуальной машины](https://docs.microsoft.com/azure/virtual-machines/windows/quotas) и [подписки Azure и ограничения службы](https://aka.ms/quotalimits) страницы. 

