---
title: Пример сценария Azure CLI. Балансировка трафика на виртуальных машинах для обеспечения высокой доступности | Документация Майкрософт
description: Пример сценария Azure CLI. Балансировка трафика на виртуальных машинах для обеспечения высокой доступности
services: load-balancer
documentationcenter: load-balancer
author: KumudD
manager: timlt
editor: tysonn
tags: ''
ms.assetid: ''
ms.service: load-balancer
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: infrastructure
ms.date: 07/07/2017
ms.author: kumud
ms.openlocfilehash: 916e9e1f7287757f19e93ecf7b8cc38f556e2dc8
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "60564652"
---
# <a name="load-balance-traffic-to-vms-for-high-availability"></a>Балансировка трафика на виртуальных машинах для обеспечения высокой доступности

Этот пример скрипта позволяет создать все необходимые компоненты для запуска нескольких виртуальных машин Ubuntu, настроенных в высокодоступной конфигурации с балансировкой нагрузки. После выполнения этого сценария будут созданы три виртуальные машины, которые будут добавлены в группу доступности Azure. Доступ к ним можно будет получить через Azure Load Balancer. 

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Пример скрипта

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/create-vm-nlb/create-vm-nlb.sh "Quick Create VM")]

## <a name="clean-up-deployment"></a>Очистка развертывания 

Выполните следующую команду, чтобы удалить группу ресурсов, виртуальную машину и все связанные с ней ресурсы.

```azurecli
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Описание скрипта

Для создания группы ресурсов, виртуальной машины, группы доступности, балансировщика нагрузки и всех связанных ресурсов этот скрипт использует следующие команды. Для каждой команды в таблице приведены ссылки на соответствующую документацию.

| Команда | Примечания |
|---|---|
| [az group create](https://docs.microsoft.com/cli/azure/group) | Создает группу ресурсов, в которой хранятся все ресурсы. |
| [az network vnet create](https://docs.microsoft.com/cli/azure/network/vnet) | Создает виртуальную сеть и подсеть Azure. |
| [az network public-ip create](https://docs.microsoft.com/cli/azure/network/public-ip) | Создает общедоступный IP-адрес со статическим IP-адресом и связанным DNS-именем. |
| [az network lb create](https://docs.microsoft.com/cli/azure/network/lb) | Создает службу Azure Load Balancer. |
| [az network lb probe create](https://docs.microsoft.com/cli/azure/network/lb/probe) | Создает зонд подсистемы балансировки нагрузки. Зонд подсистемы балансировки нагрузки используется для мониторинга каждой виртуальной машины в наборе подсистемы балансировки нагрузки. Если любая виртуальная машина становится недоступной, к ней не направляется трафик. |
| [az network lb rule create](https://docs.microsoft.com/cli/azure/network/lb/rule) | Создает правило подсистемы балансировки нагрузки. В этом примере создается правило для порта 80. Так как трафик HTTP поступает в подсистему балансировки нагрузки, он перенаправляется на порт 80 одной из виртуальных машин в наборе подсистемы балансировки нагрузки. |
| [az network lb inbound-nat-rule create](https://docs.microsoft.com/cli/azure/network/lb/inbound-nat-rule) | Создает правило преобразования сетевых адресов (NAT) подсистемы балансировки нагрузки.  Правила NAT сопоставляют порт подсистемы балансировки нагрузки с портом виртуальной машины. В этом примере создается правило NAT для SSH-трафика к каждой виртуальной машине в наборе подсистемы балансировки нагрузки.  |
| [az network nsg create](https://docs.microsoft.com/cli/azure/network/nsg) | Создает группу безопасности сети (NSG), которая выполняет роль периметра безопасности между Интернетом и виртуальной машиной. |
| [az network nsg rule create](https://docs.microsoft.com/cli/azure/network/nsg/rule) | Создает правило NSG, разрешающее входящий трафик. В этом примере открывается порт 22 для трафика SSH. |
| [az network nic create](https://docs.microsoft.com/cli/azure/network/nic) | Создает виртуальную сетевую карту и подключает ее к виртуальной сети, подсети и группе безопасности сети. |
| [az vm availability-set create](https://docs.microsoft.com/cli/azure/network/lb/rule) | Создает группу доступности. Группы доступности обеспечивают непрерывную работу приложения, распределяя виртуальные машины по физическим ресурсам. Таким образом, в случае сбоя он не затронет весь набор ресурсов. |
| [az vm create](/cli/azure/vm) | Создает виртуальную машину и подключает ее к сетевой карте, виртуальной сети, подсети и группе безопасности сети. Эта команда также указывает образ виртуальной машины, который будет использоваться, и учетные данные администратора.  |
| [az group delete](https://docs.microsoft.com/cli/azure/vm/extension) | Удаляет группу ресурсов со всеми вложенными ресурсами. |

## <a name="next-steps"></a>Дальнейшие действия

Дополнительные сведения об Azure CLI см. в [документации по Azure CLI](https://docs.microsoft.com/cli/azure).

Дополнительные примеры сценариев Azure CLI для сетей Azure см. в [документации по сетям Azure](../cli-samples.md).
