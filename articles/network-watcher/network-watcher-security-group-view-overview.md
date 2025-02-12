---
title: Общие сведения о представлении групп безопасности в Наблюдателе за сетями Azure | Документация Майкрософт
description: Эта страница содержит обзор представления системы безопасности в Наблюдателе за сетями.
services: network-watcher
documentationcenter: na
author: KumudD
manager: twooley
editor: ''
ms.assetid: ad27ab85-9d84-4759-b2b9-e861ef8ea8d8
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/26/2017
ms.author: kumud
ms.openlocfilehash: ed0ad244a758b92f5ccba5785184190b030c306c
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "64722594"
---
# <a name="introduction-to-network-security-group-view-in-azure-network-watcher"></a>Общие сведения о представлении групп безопасности сети в Наблюдателе за сетями Azure

Группы безопасности сети (NSG) связаны на уровне подсети или сетевой карты. Если она связана на уровне подсети, то применяется ко всем экземплярам виртуальной машины в этой подсети. Представление групп безопасности сети возвращает все настроенные NSG и правила, связанные на уровне сетевой карты и подсети для виртуальной машины, обеспечивая глубокое понимание данных. Кроме того, оно возвращает действующие правила безопасности для каждой из сетевых карт в виртуальной машине. С помощью представления групп безопасности сети можно оценить сетевые уязвимости виртуальной машины, например открытые порты. Можно также проверить, работает ли группа безопасности сети ожидаемым образом, [сравнив настроенные и утвержденные правила безопасности](network-watcher-nsg-auditing-powershell.md).

Более расширенный вариант использования относится к области соответствия требованиям безопасности и аудита. Можно определить предписываемый набор правил безопасности, используемый в качестве модели для контроля безопасности в вашей организации. Можно программно реализовать периодический аудит соответствия требованиям, сравнивая предписанные правила с действующими правилами для каждой из виртуальных машин в сети.

На портале правила разделены на следующие категории: действующие правила, правила подсети, правила сетевого интерфейса и правила по умолчанию. Это упрощает представление правил, применяемых к виртуальной машине. С помощью кнопки скачивания можно легко скачать все правила безопасности с любой вкладки и сохранить их в CSV-файл.

![Представление групп безопасности][1]

Можно выбрать правила, после его откроется новая колонка, содержащая группу безопасности сети и префиксы источника и назначения. В этой колонке можно перейти непосредственно к этому ресурсу группы безопасности сети.

![Детализация][2]

### <a name="next-steps"></a>Дальнейшие действия

Узнайте, как выполнять аудит параметров группы безопасности сети, ознакомившись с разделом [Automate NSG auditing with Azure Network Watcher Security group view](network-watcher-nsg-auditing-powershell.md) (Автоматизация аудита NSG с помощью представления групп безопасности Наблюдателя за сетями Azure).

[1]: ./media/network-watcher-security-group-view-overview/securitygroupview.png
[2]: ./media/network-watcher-security-group-view-overview/figure1.png









