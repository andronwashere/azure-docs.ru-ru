---
title: Как восстановить данные Azure Cosmos DB из резервной копии
description: В этой статье описывается, как восстановить данные Azure Cosmos DB из резервной копии, как обращаться в службу поддержки Azure, чтобы восстановить данные, и какие шаги предпринять после восстановления данных.
author: kanshiG
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/23/2019
ms.author: govindk
ms.reviewer: sngun
ms.openlocfilehash: c32c333de94d1ed0089323e00e6dbbaaebb36488
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "66241054"
---
# <a name="restore-data-from-a-backup-in-azure-cosmos-db"></a>Восстановление данных из резервной копии в Azure Cosmos DB 

Если вы случайно удалите базу данных или контейнер, вы можете [отправить запрос в службу поддержки]( https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) или [позвонить в службу поддержки Azure]( https://azure.microsoft.com/support/options/), чтобы восстановить данные из автоматически созданных онлайновых резервных копий. Поддержка Azure доступна для выбранных планов только такие как **стандартный**, **разработчика**и выше, чем их планы. Для плана **Базовый** поддержка Azure не предоставляется. Дополнительные сведения о различных планах поддержки см. на странице [Планы поддержки Azure](https://azure.microsoft.com/support/plans/). 

Чтобы восстановить определенный моментальный снимок архива, службе Azure Cosmos DB требуется, чтобы данные были доступны в течение цикла архивации для данного снимка.

## <a name="request-a-restore"></a>Запрос на восстановление

Прежде чем запрашивать восстановление, нужно подготовить следующие сведения:

* Идентификатор подписки.

* В зависимости от того, каким способом были случайно удалены или изменены ваши данные, может понадобиться подготовить дополнительные сведения. Рекомендуем заранее приготовить информацию, чтобы свести к минимуму время на ее передачу, особенно в тех случаях, когда проблему нужно устранить как можно быстрее.

* Если учетная запись Azure Cosmos DB удалена полностью, необходимо указать имя удаленной учетной записи. Если вы создаете другую учетную запись с тем же именем, что и у удаленной, сообщите об этом в службу поддержки, так как это поможет определить правильную учетную запись для выбора. Рекомендуем подавать отдельные запросы в службу поддержки для каждой учетной записи, которая была удалена, чтобы уменьшить путаницу при восстановлении.

* Если удалена одна или больше баз данных, необходимо указать учетную запись Azure Cosmos и имена баз данных Azure Cosmos, а также сообщить, существует ли новая база данных с тем же именем.

* Если удален один или больше контейнеров, следует указать имя учетной записи Azure Cosmos, имена баз данных и контейнеров. Сообщите также, существует ли контейнер с тем же именем.

* Если у вас случайно удалены или повреждены данные, следует обратиться к [поддержки Azure](https://azure.microsoft.com/support/options/) в течение 8 часов таким образом, команда Azure Cosmos DB может помочь вам восстановить данные из резервных копий.
  
  * Если вы случайно удалили базы данных или контейнер, обратиться в службу поддержки уровень серьезности B или уровень серьезности C Azure. 
  * Если вы случайно удалены или повреждены некоторые документы в контейнере, откройте обращение в службу поддержки уровней серьезности. 

При повреждении данных, а также при изменении либо удалении документов в контейнере нужно **удалить контейнер как можно скорее**. Удалив контейнер, вы можете избежать перезаписи Azure Cosmos DB резервными копиями. Если по какой-либо причине удаление невозможно, следует как можно скорее отправить запрос в службу. В дополнение к имени учетной записи Azure Cosmos, именам баз данных и коллекций необходимо указать точку во времени, до которой можно восстановить данные. Важно действовать с максимальной точностью, чтобы помочь нам определить самые лучшие резервные копии, доступные на этот момент. Важно также указать время в формате UTC. 

На снимке экрана ниже показано, как создать запрос в службу поддержки для контейнера (коллекции, графа, таблицы) на восстановление данных с помощью портала Azure. Предоставьте дополнительные сведения, например тип данных, цель восстановления и время удаления данных, чтобы помочь нам назначить приоритет для запроса.

![Создание запроса на резервное копирование в службу поддержки с помощью портала Azure](./media/how-to-backup-and-restore/backup-support-request-portal.png)

## <a name="post-restore-actions"></a>Действия после восстановления

После восстановления данных вы получите уведомление с указанием имени новой учетной записи (обычно в формате `<original-name>-restored1`) и времени ее восстановления. У восстановленной учетной записи будет такая же подготовленная пропускная способность и политики индексирования, она будет находиться в том же регионе, что и исходная учетная запись. Восстановленная учетная запись доступна для просмотра только пользователю, который является администратором или соадминистратором подписки.

После восстановления данных следует просмотреть и проверить и данные в восстановленной учетной записи и убедиться, что она содержит ожидаемую версию. Если все правильно, необходимо перенести данные обратно в исходную учетную запись с помощью [канала изменений Azure Cosmos DB](change-feed.md) или службы [Фабрика данных Azure](../data-factory/connector-azure-cosmos-db.md).

Рекомендуем удалить контейнер или базу данных сразу после переноса данных. Если восстановленные базы данных или контейнеры не будут удалены, то затраты на единицы запросов, хранение и исходящий трафик увеличатся.

## <a name="next-steps"></a>Дальнейшие действия

Перенос данных в исходную учетную запись описан в следующих статьях:

* [Запрос на восстановление можно отправить с портала Azure](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade), обратившись в службу поддержки Azure.
* Перемещение данных в Azure Cosmos DB с помощью [канала изменений Cosmos DB](change-feed.md).

* Перемещение данных в Azure Cosmos DB с помощью [Фабрики данных Azure](../data-factory/connector-azure-cosmos-db.md).
