---
title: Преимущества Azure Cosmos DB несколькими хозяевами
description: Ознакомиться с преимуществами несколькими хозяевами в Azure Cosmos DB.
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 07/08/2019
ms.author: mjbrown
ms.openlocfilehash: c78e5e4f8d396d777738bddfd6baf086c0b2ecf4
ms.sourcegitcommit: 1572b615c8f863be4986c23ea2ff7642b02bc605
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/10/2019
ms.locfileid: "67789297"
---
# <a name="understand-multi-master-benefits-in-azure-cosmos-db"></a>Понять преимущества хозяев в Azure Cosmos DB

Операторы учетной записи Cosmos DB следует выбрать конфигурацию соответствующие Глобальное распределение, чтобы обеспечить задержки, доступности и требования к RTO для своих приложений. Настроено несколько расположений записи учетные записи Azure Cosmos обладают существенными преимуществами по учетным записям, в том числе расположение одной операции записи, запись 99,999% доступности соглашения об уровне ОБСЛУЖИВАНИЯ, < 10 мс задержки соглашение об уровне ОБСЛУЖИВАНИЯ, на уровне 99-го процентиля и RTO записи = 0 в региональной аварии.

## <a name="comparison-of-features"></a>Сравнение функций

|Требование к приложению|Несколько расположений записи|Расположение одной операции записи|Примечание|
|---|---|---|---|
|Записи задержки соглашения об уровне ОБСЛУЖИВАНИЯ < 10 мс при уровне P99|**Да**|Нет|Учетные записи с одного места записи увеличивает задержку дополнительной сети между регионами для каждой операции записи.|
|Чтение задержки соглашения об уровне ОБСЛУЖИВАНИЯ < 10 мс при уровне P99|**Да**|Да| |
|Соглашение об уровне ОБСЛУЖИВАНИЯ 99,999% записи|**Да**|Нет|Учетные записи с одного места записи гарантирует 99,99%|
|RTO = 0|**Да**|Нет|Ноль простоя для операций записи в случае региональных сбоев. Учетные записи с одной операции записи расположения у RTO 15 мин.|

## <a name="next-steps"></a>Следующие шаги

Если вы по-прежнему хотите отключить EnableMultipleWriteLocations в вашей учетной записи Azure Cosmos, [в службу поддержки](https://azure.microsoft.com/support/create-ticket/).
