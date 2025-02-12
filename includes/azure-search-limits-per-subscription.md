---
title: включение файла
description: включение файла
services: search
author: HeidiSteen
ms.service: search
ms.topic: include
ms.date: 05/06/2019
ms.author: heidist
ms.custom: include file
ms.openlocfilehash: 1e147e8bd9260cd1ece60b70641968a229995ec1
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/18/2019
ms.locfileid: "67185666"
---
Можно создать несколько служб в рамках подписки. Каждый из них можно подготовить на определенном уровне. Вы ограничены только количество служб на каждом уровне. Например, в рамках одной подписки можно создать до двенадцати служб на уровне "Базовый" и еще двенадцать служб на уровне S1. Дополнительные сведения об уровнях см. в разделе [Выбор SKU или уровня для службы поиска Azure](../articles/search/search-sku-tier.md).

Максимальное количество служб можно увеличить по запросу. Если вам требуется больше служб в той же подписке, обратитесь в службу поддержки Azure.

| Ресурс            | Бесплатный<sup>1</sup> | базовая; | S1  | S2 | S3 | S3&nbsp;HD | L1 | L2 |
| ------------------- | ---- | ----- | --- | -- | -- | ----- | -- | -- |
| Максимальное число служб    |1     | 16    | 16  | 8  | 6  | 6     | 6  | 6  |
| Максимальный масштаб в единицах поиска (SU)<sup>2</sup> |Н/Д |3 ЕП |36 ЕП |36 ЕП |36 ЕП |36 ЕП |36 ЕП |36 ЕП |

<sup>1</sup> Уровень "Бесплатный" основан на общих (не выделенных) ресурсах. Вертикальное масштабирование для общих ресурсов не поддерживается.

<sup>2</sup> единиц поиска — это оплачиваемые единицы, выделяемые в виде *реплики* или *секции*. Оба ресурса необходимы для хранения данных, индексирования и операций запроса. Дополнительные сведения о вычислении единиц поиска см. в статье о [масштабировании уровней ресурсов для рабочих нагрузок запросов и индексирования](../articles/search/search-capacity-planning.md). 