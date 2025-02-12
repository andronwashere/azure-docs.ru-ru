---
title: Перемещение ресурсов в виртуальных сетях Azure в новую подписку или группу ресурсов | Документация Майкрософт
description: Позволяет переносить виртуальные сети в новую группу ресурсов или подписку Azure Resource Manager.
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: conceptual
ms.date: 07/09/2019
ms.author: tomfitz
ms.openlocfilehash: 809d403a463048910a284e7f8fcdc18cf7c65b69
ms.sourcegitcommit: dad277fbcfe0ed532b555298c9d6bc01fcaa94e2
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/10/2019
ms.locfileid: "67723497"
---
# <a name="move-guidance-for-virtual-networks"></a>Переместить рекомендации для виртуальных сетей

В этой статье описывается перемещение виртуальных сетей для конкретных сценариев.

## <a name="dependent-resources"></a>Зависимые ресурсы

При перемещении виртуальной сети также необходимо переместить зависимые от нее ресурсы, Для VPN-шлюзов необходимо переместить IP-адреса, шлюзы виртуальной сети и все связанные с ними ресурсы подключения. Локальные сетевые шлюзы могут находиться в другой группе ресурсов.

Чтобы переместить виртуальную машину с сетевой картой, необходимо переместить все зависимые ресурсы. Переместите виртуальную сеть для сетевой карты, все другие сетевые карты для виртуальной сети и VPN-шлюзов.

## <a name="peered-virtual-network"></a>Пиринговой виртуальной сети

Чтобы переместить виртуальную сеть с пиринговым подключением, сначала нужно отключить это подключение. После отключения виртуальную сеть можно переместить. После перемещения установите пиринговое подключение виртуальной сети заново.

## <a name="subnet-links"></a>Ссылки на подсети

Виртуальную сеть невозможно переместить в другую подписку, если эта сеть содержит подсеть со ссылками перехода к ресурсам. Например, если ресурс кэша Azure для Redis развернут в подсеть, эта подсеть содержит ссылку навигации по ресурсам.

## <a name="next-steps"></a>Следующие шаги

Команды для перемещения ресурсов см. в статье [Перемещение ресурсов в новую группу ресурсов или подписку](../resource-group-move-resources.md).
