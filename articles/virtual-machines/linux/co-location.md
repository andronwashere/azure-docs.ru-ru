---
title: Совместное размещение виртуальных машин Azure в Linux | Документация Майкрософт
description: Узнайте, как совместное размещение ресурсов виртуальных машин Azure может повысить задержку.
services: virtual-machines-linux
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 06/28/2019
ms.author: cynthn
ms.openlocfilehash: d6d8986117bd2899ea0de0aa6490954aef6c3a56
ms.sourcegitcommit: fa45c2bcd1b32bc8dd54a5dc8bc206d2fe23d5fb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/12/2019
ms.locfileid: "67850344"
---
# <a name="co-locate-resources-for-improved-latency"></a>Совместное размещение ресурсов для повышения задержки

При развертывании приложения в Azure распределение экземпляров по регионам или зонам доступности создает задержку в сети, что может повлиять на общую производительность приложения. 

## <a name="preview-proximity-placement-groups"></a>Предварительный просмотр: Группы размещения с учетом расположения

[!INCLUDE [virtual-machines-common-ppg-overview](../../../includes/virtual-machines-common-ppg-overview.md)]

## <a name="next-steps"></a>Следующие шаги

Разверните виртуальную машину в [группе размещения](proximity-placement-groups.md) с помощью Azure CLI.

