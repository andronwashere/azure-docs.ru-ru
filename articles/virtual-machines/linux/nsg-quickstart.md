---
title: Открытие портов для виртуальной машины Linux с помощью Azure CLI | Документы Майкрософт
description: Узнайте, как открыть порт или создать конечную точку для виртуальной машины Linux, используя модель развертывания с помощью Azure Resource Manager и Azure CLI.
services: virtual-machines-linux
documentationcenter: ''
author: cynthn
manager: gwallace
editor: ''
ms.assetid: eef9842b-495a-46cf-99a6-74e49807e74e
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 12/13/2017
ms.author: cynthn
ms.openlocfilehash: 1dec41f9c33bba94db2cd75b60d3490fe853482c
ms.sourcegitcommit: 2e4b99023ecaf2ea3d6d3604da068d04682a8c2d
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/09/2019
ms.locfileid: "67671142"
---
# <a name="open-ports-and-endpoints-to-a-linux-vm-with-the-azure-cli"></a>Открытие портов и конечных точек для виртуальной машины Linux с помощью интерфейса командной строки Azure

Чтобы открыть порт или создать конечную точку для виртуальной машины в Azure, создайте сетевой фильтр для подсети или сетевого интерфейса виртуальной машины. Эти фильтры, контролирующие входящий и исходящий трафик, добавляются в группу безопасности сети и присоединяются к ресурсу, который будет получать трафик. Давайте используем распространенный пример веб-трафика через порт 80. В этой статье показано, как открыть порт для виртуальной машины с помощью Azure CLI. 


Для создания группы безопасности сети и правил необходимо установить последнюю версию [Azure CLI](/cli/azure/install-az-cli2) и войти в учетную запись Azure с помощью команды [az login](/cli/azure/reference-index).

В следующих примерах замените имена параметров собственными значениями. Примеры имен параметров: *myResourceGroup*, *mystorageaccount* и *myVM*.


## <a name="quickly-open-a-port-for-a-vm"></a>Как быстро открыть порт для виртуальной машины
Если вам нужно быстро открыть порт для виртуальной машины в рамках сценария разработки и тестирования, можно воспользоваться командой [az vm open-port](/cli/azure/vm). С помощью этой команды можно создать группу безопасности сети, добавить правило и применить его к виртуальной машине или подсети. В следующем примере открывается порт *80* виртуальной машины *myVM*, входящей в группу ресурсов с именем *myResourceGroup*.

```azure-cli
az vm open-port --resource-group myResourceGroup --name myVM --port 80
```

Чтобы получить дополнительный контроль над правилами, такими как определение исходного диапазона IP-адресов, выполните остальные шаги из этой статьи.


## <a name="create-a-network-security-group-and-rules"></a>Создание группы безопасности сети и правил
Создайте группу безопасности сети с помощью команды [az network nsg create](/cli/azure/network/nsg). В следующем примере создается группа безопасности сети *myNetworkSecurityGroup* в расположении *eastus*.

```azurecli
az network nsg create \
    --resource-group myResourceGroup \
    --location eastus \
    --name myNetworkSecurityGroup
```

С помощью команды [az network nsg rule create](/cli/azure/network/nsg/rule) добавьте правило, разрешающее HTTP-трафик к вашему веб-серверу (или настройте правило под собственные нужды, например доступ по протоколу SSH или подключение к базе данных). В следующем примере создается правило с именем *myNetworkSecurityGroupRule*. Это правило разрешает TCP-трафик через порт 80:

```azurecli
az network nsg rule create \
    --resource-group myResourceGroup \
    --nsg-name myNetworkSecurityGroup \
    --name myNetworkSecurityGroupRule \
    --protocol tcp \
    --priority 1000 \
    --destination-port-range 80
```


## <a name="apply-network-security-group-to-vm"></a>Применение группы безопасности сети к виртуальной машине
Свяжите группу безопасности сети с сетевым интерфейсом виртуальной машины с помощью команды [az network nic update](/cli/azure/network/nic). В следующем примере существующий сетевой адаптер *myNic* связывается с группой безопасности сети *myNetworkSecurityGroup*:

```azurecli
az network nic update \
    --resource-group myResourceGroup \
    --name myNic \
    --network-security-group myNetworkSecurityGroup
```

Кроме того, группу безопасности сети с помощью команды [az network vnet subnet update](/cli/azure/network/vnet/subnet) можно связать с подсетью виртуальной сети, а не только с сетевым интерфейсом на отдельной виртуальной машине. В следующем примере существующий сетевой адаптер *myNic* в виртуальной сети *myVnet* связывается с группой безопасности сети *myNetworkSecurityGroup*:

```azurecli
az network vnet subnet update \
    --resource-group myResourceGroup \
    --vnet-name myVnet \
    --name mySubnet \
    --network-security-group myNetworkSecurityGroup
```

## <a name="more-information-on-network-security-groups"></a>Дополнительная информация о группах безопасности сети
Приведенные здесь быстрые команды позволят настроить трафик, поступающий в виртуальную машину. Группы безопасности сети предоставляют множество полезных функций и всевозможные настройки для управления доступом к ресурсам. [Здесь](tutorial-virtual-network.md#secure-network-traffic)вы можете больше прочитать о создании группы безопасности сети и правил ACL.

Для веб-приложений с высокой доступностью необходимо поместить виртуальную машину за Azure Load Balancer. Балансировщик нагрузки распределяет трафик между виртуальными машинами с группой безопасности сети, обеспечивающей фильтрацию трафика. Подробные сведения см. в статье [Балансировка нагрузки виртуальных машин Windows в Azure для создания высокодоступного приложения](tutorial-load-balancer.md).

## <a name="next-steps"></a>Следующие шаги
В этом примере создано простое правило, разрешающее трафик HTTP. Информацию о создании более детализированных сред можно найти в следующих статьях.

* [Общие сведения об Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md)
* [Группа безопасности сети](../../virtual-network/security-overview.md)
