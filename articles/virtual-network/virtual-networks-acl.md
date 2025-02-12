---
title: Что такое список управления доступом к сети Azure?
description: Дополнительные сведения о списках управления доступом в Azure
services: virtual-network
documentationcenter: na
author: genlin
manager: cshepard
editor: ''
tags: azure-service-management
ms.assetid: 83d66c84-8f6b-4388-8767-cd2de3e72d76
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/15/2016
ms.author: genli
ms.openlocfilehash: 3a7155380a51273d376226c6be7a004f386181ce
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "61035257"
---
# <a name="what-is-an-endpoint-access-control-list"></a>Что такое список управления доступом к конечной точке?

> [!IMPORTANT]
> Azure предусмотрены две [моделей развертывания](../azure-resource-manager/resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json) для создания и работы с ресурсами: Resource Manager и классическая модель. В этой статье рассматривается использование классической модели развертывания. Для большинства новых развертываний корпорация Майкрософт рекомендует использовать модель развертывания с помощью Resource Manager. 

Список управления доступом (ACL) к конечной точке — это дополнительный компонент системы безопасности, который можно использовать для развертывания Azure. Список ACL дает возможность выборочно разрешать или блокировать трафик для конечной точки виртуальной машины. Такая возможность фильтрации пакетов обеспечивает дополнительный уровень безопасности. Задавать сетевые списки ACL можно только для конечных точек. Список ACL нельзя задать для виртуальной сети или конкретной подсети в ней. При любой возможности вместо списков ACL рекомендуется использовать группы безопасности сети (NSG). При использовании групп безопасности сети список управления доступом к конечной точке заменяется и больше не применятся. Дополнительные сведения о группах безопасности сети см. в [обзоре группы безопасности сети](security-overview.md).

Списки ACL можно настраивать с помощью PowerShell или портала Azure. Сведения о настройке сетевого ACL с помощью PowerShell см. в статье [Управление списками управления доступом конечной точки с помощью PowerShell в классической модели развертывания](virtual-networks-acl-powershell.md). Сведения о настройке сетевого ACL с помощью портала Azure см. в статье [Настройка конечных точек в классической виртуальной машине Windows в Azure](../virtual-machines/windows/classic/setup-endpoints.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

Сетевые списки ACL позволяют:

* выборочно разрешать или запрещать входящий трафик на входной конечной точке виртуальной машины на основе диапазона IPv4-адресов удаленной подсети;
* добавлять IP-адреса в список блокировок;
* создавать по несколько правил для каждой конечной точки виртуальной машины;
* упорядочивать правила, что дает возможность использовать на той или иной конечной точке виртуальной машины необходимый набор правил (чем ниже номер правила, тем выше приоритет);
* указывать список ACL для конкретного IPv4-адреса удаленной подсети.

Ознакомьтесь с дополнительными сведениями об ограничениях ACL в Azure в [этой статье](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#networking-limits).

## <a name="how-acls-work"></a>Принцип работы списков управления доступом
ACL — это объект, содержащий список правил. После создания списка ACL и его применения к конечной точке виртуальной машины на главном узле виртуальной машины начинается фильтрация пакетов. Это означает, что трафик с удаленных IP-адресов фильтруется на предмет соответствия правилам ACL главным узлом, а не виртуальной машиной. Благодаря этому виртуальная машина не тратит циклы ЦП на фильтрование пакетов.

После создания виртуальной машины для блокирования всего входящего трафика используется список ACL по умолчанию. Но если для какого-то порта, например 3389, создана конечная точка, список ACL по умолчанию изменяется таким образом, чтобы разрешить весь входящий трафик для этой конечной точки. В результате к этой конечной точке пропускается весь входящий трафик из любой удаленной подсети и подготовка брандмауэра не требуется. Входящий трафик на всех остальных портах будет блокироваться, пока для них не будут созданы отдельные конечные точки. Исходящий трафик по умолчанию разрешен.

**Пример списка ACL по умолчанию**

| **Номер правила** | **Удаленная подсеть** | **Конечная точка** | **Разрешение или запрет** |
| --- | --- | --- | --- |
| 100 |0.0.0.0/0 |3389 |Разрешение |

## <a name="permit-and-deny"></a>Разрешение и запрет
Для входной конечной точки виртуальной машины можно выборочно разрешать или запрещать сетевой трафик. Для этого нужно создать правила, разрешающие или блокирующие трафик. Важно отметить, что по умолчанию после создания конечной точки весь трафик к ней разрешен. В связи с этим вам необходимо научиться создавать правила, разрешающие или блокирующие трафик, и размещать их в порядке приоритетности. Это позволит вам точно контролировать сетевой трафик, который вы пропускаете к конечной точке виртуальной машины.

Необходимо учесть следующие моменты.

1. **Без ACL.** По умолчанию после создании конечной точки весь трафик для нее разрешен.
2. **Разрешение.** При добавлении одного или нескольких разрешенных диапазонов все остальные диапазоны запрещаются автоматически. Конечная точка виртуальной машины сможет обмениваться пакетами данных только с IP-адресами из разрешенного диапазона.
3. **Запрет.** При добавлении одного или нескольких запрещенных диапазонов все остальные диапазоны разрешаются автоматически.
4. **Сочетание разрешений и запретов.** Если нужно выделить диапазон запрещенных и разрешенных IP-адресов, можно использовать сочетание разрешений и запретов.

## <a name="rules-and-rule-precedence"></a>Правила и их приоритетность
Сетевые списки ACL можно настроить на конкретных конечных точках виртуальной машины. Например, вы можете настроить сетевой список ACL для конечной точки RDP, созданной на виртуальной машине, который будет блокировать доступ для определенных IP-адресов. В таблице ниже показан способ предоставления доступа для общедоступных виртуальных IP-адресов (VIP) определенного диапазона. С этих адресов к виртуальной машине можно будет подключаться по протоколу RDP. Все другие удаленные IP-адреса будут блокироваться. Используется правило *чем меньший номер, тем больший приоритет*.

### <a name="multiple-rules"></a>Несколько правил
Если вы хотите разрешить доступ к конечной точке RDP только из двух диапазонов общедоступных IPv4-адресов (65.0.0.0/8 и 159.0.0.0/8), это можно сделать, настроив два *разрешающих правила*  (см. пример ниже). Так как протокол RDP создается по умолчанию для виртуальной машины, вам может потребоваться заблокировать доступ к порту RDP из удаленной подсети. В примере ниже показан способ предоставления доступа для общедоступных виртуальных IP-адресов (VIP) определенного диапазона. С этих адресов к виртуальной машине можно будет подключаться по протоколу RDP. Все другие удаленные IP-адреса будут блокироваться. Это работает, потому что сетевые списки управления доступом можно настроить для конкретной конечной точки виртуальной машины и по умолчанию доступ будет запрещен.

**Пример нескольких правил**

| **Номер правила** | **Удаленная подсеть** | **Конечная точка** | **Разрешение или запрет** |
| --- | --- | --- | --- |
| 100 |65.0.0.0/8 |3389 |Разрешение |
| 200 |159.0.0.0/8 |3389 |Разрешение |

### <a name="rule-order"></a>Порядок правил
Так как для конечной точки можно задать несколько правил, должен быть способ упорядочить их с определенной приоритетностью. Порядок правил определяет их приоритет. В сетевых списках ACL используется правило *чем меньший номер, тем больший приоритет* . В приведенном ниже примере конечной точке на порту 80 предоставляется доступ только к определенным диапазонам IP-адресов. Чтобы настроить это, мы указали запрещающее правило (номер \# 100) для адресов в подсети 175.1.0.1/24. Затем указали второе правило с номером 200, которое разрешает доступ всем другим адресам в подсети 175.0.0.0/8.

**Пример приоритета правил**

| **Номер правила** | **Удаленная подсеть** | **Конечная точка** | **Разрешение или запрет** |
| --- | --- | --- | --- |
| 100 |175.1.0.1/24 |80 |Запрет |
| 200 |175.0.0.0/8 |80 |Разрешение |

## <a name="network-acls-and-load-balanced-sets"></a>Сетевые списки ACL и наборы с балансировкой нагрузки
Сетевые ACL можно указывать в конечной точке набора с балансировкой нагрузки. Если для набора с балансировкой нагрузки задан ACL, то этот сетевой ACL применяется ко всем виртуальным машинам в данном наборе. Например, если создать набор с балансировкой нагрузки с портом 80, содержащий три виртуальные машины, сетевой ACL, созданный для порта 80 конечной точки одной из виртуальных машин, будет автоматически применен к остальным виртуальным машинам.

![Сетевые списки ACL и наборы с балансировкой нагрузки](./media/virtual-networks-acl/IC674733.png)

## <a name="next-steps"></a>Следующие шаги
[Управление списками управления доступом для конечных точек с помощью PowerShell](virtual-networks-acl-powershell.md)

