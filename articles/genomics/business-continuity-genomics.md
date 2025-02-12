---
title: Обеспечение непрерывности бизнес-процессов в Microsoft Genomics | Документация Майкрософт
titleSuffix: Azure
description: В этом обзоре описываются возможности Microsoft Genomics по обеспечению непрерывности бизнес-процессов и аварийного восстановления. Здесь вы узнаете о вариантах восстановления после аварийных событий, таких как сбой региона Azure, которые могут привести к потере данных.
keywords: business continuity, disaster recovery
services: genomics
author: grhuynh
manager: cgronlun
ms.author: grhuynh
ms.service: genomics
ms.topic: article
ms.date: 04/06/2018
ms.openlocfilehash: 7a51477dbbf6f4e50959a6d979342961c7e49ad9
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "60641115"
---
# <a name="overview-of-business-continuity-with-microsoft-genomics"></a>Обзор обеспечения непрерывности бизнес-процессов с помощью Microsoft Genomics
В этом обзоре описываются возможности Microsoft Genomics по обеспечению непрерывности бизнес-процессов и аварийного восстановления. Здесь вы узнаете о вариантах восстановления после аварийных событий, таких как сбой региона Azure, которые могут привести к потере данных. 


## <a name="microsoft-genomics-features-that-support-business-continuity"></a>Функции Microsoft Genomics, которые поддерживают непрерывность бизнес-процессов 
Хотя и редко, в центре обработки данных Azure могут возникнуть сбои, что может привести к нарушению работы бизнеса, которое может длиться от нескольких минут до нескольких часов. При возникновении сбоев все задания, которые в текущий момент выполняются в центре обработки данных, завершатся с ошибкой, а служба Microsoft Genomics не будет автоматически повторно отправлять их в дополнительный регион. 

* Один из вариантов — дождаться возобновления работы центра обработки данных после того, как сбой будет устранен. При кратковременном сбое служба Microsoft Genomics автоматически определит невыполненные задания и рабочий процесс будет автоматически перезапущен.

* Другой вариант — заранее отправить рабочий процесс в другую область данных. Microsoft Genomics развертывает экземпляры в нескольких [регионах Azure](https://azure.microsoft.com/regions/services/), и каждый экземпляр конкретного региона является независимым. Если в одном из экземпляров Microsoft Genomics происходит региональный локальный сбой, другие регионы, работающие с экземплярами Microsoft Genomics, будут продолжать обрабатывать задания. Эта передача в альтернативный регион находится под контролем пользователя и доступна в любое время.


### <a name="manually-failover-microsoft-genomics-workflows-to-another-region"></a>Перенаправление рабочих процессов Microsoft Genomics в другой регион вручную
В случае сбоя регионального центра обработки данных вы можете перенаправить рабочие процессы Microsoft Genomics в дополнительный регион на основе индивидуальных требований к независимости данных и непрерывности бизнес-процессов. Чтобы перенаправить рабочие процессы Microsoft Genomics в другой регион вручную, необходимо использовать учетную запись Genomics конкретного региона и отправить задание с соответствующими учетными данными этой учетной записи и учетной записи хранения.

В частности, необходимо:
* создать учетную запись Genomics в дополнительном регионе, используя портал Azure; 
* перенести входные данные в учетную запись хранения в дополнительном регионе и настроить выходную папку в дополнительном регионе;
* отправить рабочий процесс в дополнительный регион.

После восстановления исходного региона служба Microsoft Genomics не будет переносить данные из дополнительного региона в исходный. Можно также переместить входные и выходные файлы из дополнительного региона обратно в исходный.  Если принято решение о перемещении данных, перенос выполняется за пределами службы Genomics, а значит все расходы, связанные с перемещением данных, несете вы. 

### <a name="preparing-for-a-possible-region-specific-outage"></a>Подготовка к возможному сбою в конкретном регионе
Если вы беспокоитесь о более быстром восстановлении в случае сбоя в центре обработки данных, можно выполнить несколько шагов, чтобы сократить время, необходимое для повторной отправки рабочих процессов Microsoft Genomics вручную в дополнительный регион:

* определить соответствующий дополнительный регион и заблаговременно создать учетную запись Genomics в этом регионе;
* дублировать данные в основном и дополнительном регионе, чтобы данные были незамедлительно доступны в дополнительном регионе. Это можно сделать вручную или с помощью [геоизбыточного хранилища](https://docs.microsoft.com/azure/storage/common/storage-redundancy), доступного в хранилище Azure. 

## <a name="next-steps"></a>Дальнейшие действия
В этой статье были рассмотрены варианты обеспечения непрерывности бизнес-процессов и аварийного восстановления при использовании службы Microsoft Genomics. Дополнительные сведения о непрерывности бизнес-процессов и аварийном восстановлении в рамках Azure в целом см. в [техническом руководстве по обеспечению устойчивости в Azure](https://docs.microsoft.com/azure/architecture/resiliency/recovery-loss-azure-region). 