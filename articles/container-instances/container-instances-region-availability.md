---
title: Доступность ресурсов службы "Экземпляры контейнеров Azure"
description: Доступность вычислительных ресурсов и памяти для службы "Экземпляры контейнеров Azure" в разных регионах Azure.
services: container-instances
author: dlepow
manager: gwallace
ms.service: container-instances
ms.topic: overview
ms.date: 05/14/2019
ms.author: danlep
ms.openlocfilehash: 14e7b9a3ea11e59aabeb901c4039e69208ea0cfd
ms.sourcegitcommit: 4b431e86e47b6feb8ac6b61487f910c17a55d121
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/18/2019
ms.locfileid: "68325717"
---
# <a name="resource-availability-for-azure-container-instances-in-azure-regions"></a>Доступность ресурсов службы "Экземпляры контейнеров Azure" в регионах Azure

В этой статье подробно описывается доступность вычислительных ресурсов и ресурсов памяти экземпляров Azure в регионах Azure. 

Представленные значения — это максимальные ресурсы, доступные для развертывания [группы контейнеров](container-instances-container-groups.md). Значения актуальны на момент публикации. 

> [!NOTE]
> Группы контейнеров, созданные в пределах границ этих ресурсов, зависят от доступности в регионе развертывания. Если в регионе высокая нагрузка, при развертывании экземпляров могут возникать сбои. Чтобы устранить такой сбой развертывания, попробуйте развернуть экземпляры с более низкими параметрами ресурсов или выполнить развертывание позднее.

Сведения о квотах и ​​других ограничениях в развертываниях см. в разделе [Квоты и доступность по регионам для службы "Экземпляры контейнеров Azure"](container-instances-quotas.md).

## <a name="availability---general"></a>Доступность: общая

Для групп контейнеров Linux и [поддерживаемых](container-instances-faq.md#what-windows-base-os-images-are-supported) контейнеров на базе Windows Server 2016 доступны следующие регионы и ресурсы.

| Location | ОС | ЦП | Память (ГБ) |
| -------- | -- | :---: | :-----------: |
| Центральная Канада, Центральная Индия, центральная часть США, Восточная Азия, восточная часть США, восточная часть США 2, Северная Европа, центрально-южная часть США, Юго-Восточная Азия, южная часть Соединенного Королевства, западная часть США | Linux | 4\. | 16 |
| Западная Европа, западная часть США 2 | Linux | 4\. | 14 |
| Восточная Австралия, Восточная Япония | Linux | 2 | 8 |
| Центрально-северная часть США, Южная Индия | Linux | 2 | 3,5 |
| Западная Европа | Windows | 4\. | 16 |
| Восточная часть США, западная часть США | Windows | 4\. | 14 |
| Восточная Австралия, Центральная Канада, Центральная Индия, центральная часть США, Восточная Азия, восточная часть США 2, Восточная Япония, центрально-северная часть США, Северная Европа, центрально-южная часть США, Юго-Восточная Азия, Южная Индия, южная часть Соединенного Королевства, западная часть США 2 | Windows | 2 | 3,5 |

## <a name="availability---windows-server-2019-ltsc-1809-deployments-preview"></a>Доступность — Windows Server 2019 LTSC, развертывания версии 1809 (предварительная версия)

Для групп контейнеров на базе Windows Server 2019 (предварительная версия) доступны следующие регионы и ресурсы.

| Location | ОС | ЦП | Память (ГБ) |
| -------- | -- | :---: | :-----------: |
| Юго-Восточная Азия, Северная Европа, Западная Европа, центральная часть США, восточная часть США, западная часть США, западная часть США 2 | Windows | 4\. | 16 |
| Восток США 2 | Windows | 2 | 3,5 |


## <a name="availability---virtual-network-deployment-preview"></a>Доступность: развертывание виртуальной сети (предварительный просмотр)

Для групп контейнеров, развернутых в [виртуальной сети Azure](container-instances-vnet.md) (предварительная версия) доступны следующие регионы и ресурсы.

[!INCLUDE [container-instances-vnet-limits](../../includes/container-instances-vnet-limits.md)]

## <a name="availability---gpu-resources-preview"></a>Доступность: ресурсы GPU (предварительная версия)

Для групп контейнеров, развернутых с помощью [ресурсов GPU](container-instances-gpu.md) (предварительная версия) доступны следующие регионы и ресурсы.

[!INCLUDE [container-instances-gpu-regions](../../includes/container-instances-gpu-regions.md)]
[!INCLUDE [container-instances-gpu-limits](../../includes/container-instances-gpu-limits.md)]

## <a name="next-steps"></a>Дополнительная информация

Если вы хотите увидеть дополнительные регионы или увеличить доступность ресурсов, сообщите об этом команде, перейдя по адресу [​​aka.ms/aci/feedback](https://aka.ms/aci/feedback).

Сведения об устранении неполадок развертывания экземпляра контейнера см. в статье [Устранение распространенных неполадок с помощью службы "Экземпляры контейнеров Azure"](container-instances-troubleshooting.md).
