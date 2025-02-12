---
title: Руководство. Создание и администрирование виртуальных сетей Azure для виртуальных машин Linux | Документация Майкрософт
description: В этом руководстве описано, как с помощью Azure CLI создать и администрировать виртуальные сети Azure для виртуальных машин Linux.
services: virtual-machines-linux
documentationcenter: virtual-machines
author: cynthn
manager: gwallace
editor: tysonn
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/10/2017
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: ecafd2b1a98ab38d7149ebddecd16a695847eccc
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/09/2019
ms.locfileid: "67703497"
---
# <a name="tutorial-create-and-manage-azure-virtual-networks-for-linux-virtual-machines-with-the-azure-cli"></a>Руководство по Создание и администрирование виртуальных сетей Azure для виртуальных машин Linux с помощью Azure CLI

Виртуальные машины Azure осуществляют внутреннее и внешнее взаимодействие через сеть Azure. В этом руководстве содержатся сведения о развертывании двух виртуальных машин и настройке для них сети Azure. Примеры, описанные в этом руководстве, предполагают, что на виртуальных машинах размещается веб-приложение с сервером базы данных, но здесь не описывается развертывание самого приложения. Из этого руководства вы узнаете, как выполнять следующие задачи:

> [!div class="checklist"]
> * Создание виртуальной сети и подсети
> * Создание общедоступного IP-адреса
> * Создание интерфейсной виртуальной машины
> * защищают сетевой трафик;
> * Создание внутренней виртуальной машины.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Если вы решили установить и использовать интерфейс командной строки локально, то для работы с этим руководством вам понадобится Azure CLI 2.0.30 или более поздней версии. Чтобы узнать версию, выполните команду `az --version`. Если вам необходимо выполнить установку или обновление, см. статью [Установка Azure CLI 2.0]( /cli/azure/install-azure-cli).

## <a name="vm-networking-overview"></a>Обзор сети виртуальной машины

Виртуальные сети Azure позволяют устанавливать безопасные сетевые подключения между виртуальными машинами, в Интернете, а также между другими службами Azure, такими как база данных SQL Azure. Виртуальные сети разбиты на логические сегменты — подсети. Подсети позволяют контролировать поток сетевого трафика. Это своего рода периметр безопасности. При развертывании виртуальная машина обычно содержит виртуальный сетевой интерфейс, подключенный к подсети.

Завершив работу с руководством, вы создадите следующие ресурсы виртуальной сети:

![Виртуальная сеть с двумя подсетями](./media/tutorial-virtual-network/networktutorial.png)

- *myVNet.* Виртуальная сеть, которую используют виртуальные машины для взаимодействия между собой и в Интернете.
- *myFrontendSubnet.* Подсеть в *myVNet*, используемая ресурсами внешних интерфейсов.
- *myPublicIPAddress.* Общедоступный IP-адрес, используемый для доступа к *myFrontendVM* из Интернета.
- *myFrontentNic.* Сетевой интерфейс, используемый *myFrontendVM* для взаимодействия с *myBackendVM*.
- *myFrontendVM.* Виртуальная машина, используемая для взаимодействия Интернета и *myBackendVM*.
- *myBackendNSG.* Группа безопасности сети, которая управляет взаимодействием между *myFrontendVM* и *myBackendVM*.
- *myBackendSubnet.* Подсеть, связанная с *myBackendNSG* и используемая внутренними ресурсами.
- *myBackendNic.* Сетевой интерфейс, используемый *myBackendVM* для взаимодействия с *myFrontendVM*.
- *myBackendVM.* Виртуальная машина, использующая порт 22 и 3306 для взаимодействия с *myFrontendVM*.

## <a name="create-a-virtual-network-and-subnet"></a>Создание виртуальной сети и подсети

В этом руководстве создается виртуальная сеть с двумя подсетями: интерфейсная подсеть для размещения веб-приложения и внутренняя подсеть для размещения сервера базы данных.

Прежде чем создать виртуальную сеть, выполните команду [az group create](/cli/azure/group), чтобы создать группу ресурсов. Следующий пример позволяет создать группу ресурсов *myRGNetwork* в расположении eastus.

```azurecli-interactive 
az group create --name myRGNetwork --location eastus
```

### <a name="create-virtual-network"></a>Создание виртуальной сети

Создайте виртуальную сеть с помощью команды [az network vnet create](/cli/azure/network/vnet). В этом примере создается сеть *mvVnet* с префиксом адреса *10.0.0.0/16*, а также подсеть *myFrontendSubnet* с префиксом *10.0.1.0/24*. Далее в этом руководстве мы подключим интерфейсную виртуальную машину к этой подсети. 

```azurecli-interactive 
az network vnet create \
  --resource-group myRGNetwork \
  --name myVNet \
  --address-prefix 10.0.0.0/16 \
  --subnet-name myFrontendSubnet \
  --subnet-prefix 10.0.1.0/24
```

### <a name="create-subnet"></a>Создание подсети

Добавьте новую подсеть к виртуальной сети с помощью команды [az network vnet subnet create](/cli/azure/network/vnet/subnet). В этом примере создается подсеть *myBackendSubnet* с префиксом адреса *10.0.2.0/24*. Она используется со всеми внутренними службами.

```azurecli-interactive 
az network vnet subnet create \
  --resource-group myRGNetwork \
  --vnet-name myVNet \
  --name myBackendSubnet \
  --address-prefix 10.0.2.0/24
```

На этом этапе мы создали сеть и разбили ее на две подсети — одна для интерфейсных служб, а вторая для внутренних служб. В следующем разделе мы создадим виртуальные машины и подключим их к этим подсетям.

## <a name="create-a-public-ip-address"></a>Создание общедоступного IP-адреса

Общедоступный IP-адрес обеспечивает доступность ресурсов Azure в Интернете. Метод распределения общедоступного IP-адреса может быть динамическим или статическим. По умолчанию выделяется динамический общедоступный IP-адрес. После освобождения виртуальной машины освобождаются также и динамические IP-адреса. Таким образом, IP-адреса изменяются во время каждой операции с освобождением виртуальной машины.

При использовании статического метода выделения IP-адрес остается назначенным виртуальной машине, даже если она в освобожденном состоянии. В этом случае вы не сможете указать IP-адрес. Он выделяется из пула доступных адресов.

```azurecli-interactive
az network public-ip create --resource-group myRGNetwork --name myPublicIPAddress
```

При создании виртуальной машины с помощью команды [az vm create](/cli/azure/vm) по умолчанию выделяется динамический общедоступный IP-адрес. При создании виртуальной машины с помощью команды [az vm create](/cli/azure/vm) добавьте аргумент `--public-ip-address-allocation static`, чтобы назначить статический общедоступный IP-адрес. Эта операция не описана в этом руководстве, но в следующем разделе показано, как изменить динамический IP-адрес на статический. 

### <a name="change-allocation-method"></a>Изменение метода выделения

Метод выделения IP-адресов можно изменить с помощью команды [az network public-ip update](/cli/azure/network/public-ip). В этом примере метод выделения IP-адреса интерфейсной виртуальной машины изменяется на статический.

Сначала отмените подготовку виртуальной машины.

```azurecli-interactive 
az vm deallocate --resource-group myRGNetwork --name myFrontendVM
```

Обновите метод выделения, используя команду [az network public-ip update](/cli/azure/network/public-ip). В этом случае для параметра `--allocation-method` необходимо задать значение *static*.

```azurecli-interactive 
az network public-ip update --resource-group myRGNetwork --name myPublicIPAddress --allocation-method static
```

Запустите виртуальную машину.

```azurecli-interactive 
az vm start --resource-group myRGNetwork --name myFrontendVM --no-wait
```

### <a name="no-public-ip-address"></a>Создание виртуальной машины без общедоступного IP-адреса

Во многих случаях виртуальная машина не должна быть доступна через Интернет. Чтобы создать виртуальную машину без общедоступного IP-адреса, используйте аргумент `--public-ip-address ""` с пустыми двойными кавычками. Эта конфигурация рассматривается далее в этом руководстве.

## <a name="create-a-front-end-vm"></a>Создание интерфейсной виртуальной машины.

Выполните команду [az vm create](/cli/azure/vm), чтобы создать виртуальную машину *myFrontendVM*, использующую *myPublicIPAddress*.

```azurecli-interactive 
az vm create \
  --resource-group myRGNetwork \
  --name myFrontendVM \
  --vnet-name myVNet \
  --subnet myFrontendSubnet \
  --nsg myFrontendNSG \
  --public-ip-address myPublicIPAddress \
  --image UbuntuLTS \
  --generate-ssh-keys
```

## <a name="secure-network-traffic"></a>защищают сетевой трафик;

Группа безопасности сети (NSG) содержит перечень правил безопасности, которые разрешают или запрещают передачу сетевого трафика к ресурсам, подключенным к виртуальным сетям Azure. Группы безопасности сети (NSG) можно связать с подсетями или отдельными сетевыми интерфейсами. Группа безопасности сети, связанная с сетевым интерфейсом, применяется только к связанной виртуальной машине. Если группа безопасности сети связана с подсетью, эти правила применяются ко всем ресурсам в подсети. 

### <a name="network-security-group-rules"></a>Правила группы безопасности сети

Правила группы безопасности сети определяют сетевые порты, через которые разрешен или запрещен трафик. Эти правила также могут включать исходные и целевые диапазоны IP-адресов, что позволяет контролировать трафик между определенными системами и подсетями. Правилам также присваивается приоритет (от 1 до 4096), и они оцениваются в порядке приоритета. Правило с приоритетом 100 оценивается перед правилом с приоритетом 200.

Все группы NSG содержат набор правил по умолчанию. Эти правила нельзя удалить, но у них самый низкий приоритет, поэтому их можно переопределить, создав другие правила.

Стандартные правила для NSG:

- **Виртуальная сеть.** Входящий и исходящий трафик виртуальной сети разрешен в обоих направлениях.
- **Интернет.** Исходящий трафик разрешен, но входящий трафик заблокирован.
- **Подсистема балансировки нагрузки.** Разрешает Azure Load Balancer инициировать проверку работоспособности виртуальных машин и экземпляров роли. Если набор балансировки нагрузки не используется, это правило можно переопределить.

### <a name="create-network-security-groups"></a>Создание групп безопасности сети

Группу безопасности сети можно создать вместе с виртуальной машиной, используя команду [az vm create](/cli/azure/vm). В этом случае группа безопасности сети связывается с сетевым интерфейсом виртуальных машин и автоматически создается правило NSG, разрешающее прохождение трафика через порт *22* из любого источника. Ранее в этом руководстве мы создали интерфейсную виртуальную машину, вместе с которой была автоматически создана группа безопасности сети переднего плана. Кроме того, было также создано правило NSG для порта 22. 

В некоторых случаях есть смысл заранее создать группу безопасности сети, например, если не нужно создавать правила SSH по умолчанию или если группу безопасности сети нужно присоединить к подсети. 

Создайте группу безопасности сети с помощью команды [az network nsg create](/cli/azure/network/nsg).

```azurecli-interactive 
az network nsg create --resource-group myRGNetwork --name myBackendNSG
```

В этом случае группа безопасности сети будет связана с подсетью, а не сетевым интерфейсом. В этой конфигурации все виртуальные машины, подключенные к подсети, наследуют правила NSG.

Обновите имеющуюся подсеть *myBackendSubnet*, включив в нее новую группу безопасности сети.

```azurecli-interactive 
az network vnet subnet update \
  --resource-group myRGNetwork \
  --vnet-name myVNet \
  --name myBackendSubnet \
  --network-security-group myBackendNSG
```

### <a name="secure-incoming-traffic"></a>Защита входящего трафика

При создании интерфейсной виртуальной машины также было создано правило NSG, разрешающее входящий трафик через порт 22. Это правило разрешает SSH-подключения к виртуальной машине. В этом примере трафик также необходимо разрешить через порт *80*. Такая конфигурация обеспечивает доступ к веб-приложению на виртуальной машине.

Создайте правило для порта *80* с помощью команды [az network nsg rule create](/cli/azure/network/nsg/rule).

```azurecli-interactive 
az network nsg rule create \
  --resource-group myRGNetwork \
  --nsg-name myFrontendNSG \
  --name http \
  --access allow \
  --protocol Tcp \
  --direction Inbound \
  --priority 200 \
  --source-address-prefix "*" \
  --source-port-range "*" \
  --destination-address-prefix "*" \
  --destination-port-range 80
```

Интерфейсная виртуальная машина доступна только через порты *22* и *80*. Весь остальной входящий трафик блокируется группой безопасности сети. Иногда есть смысл визуализировать конфигурации правил NSG. Получите конфигурацию правила NSG с помощью команды [az network rule list](/cli/azure/network/nsg/rule). 

```azurecli-interactive 
az network nsg rule list --resource-group myRGNetwork --nsg-name myFrontendNSG --output table
```

### <a name="secure-vm-to-vm-traffic"></a>защищать трафик между виртуальными машинами.

Правила группы безопасности сети также можно применить между виртуальными машинами. В этом примере интерфейсная виртуальная машина должна взаимодействовать с внутренней виртуальной машиной через порт *22* и *3306*. Эта конфигурация разрешает SSH-подключения из интерфейсной виртуальной машины, а также разрешает приложению, расположенному на этой виртуальной машине, взаимодействовать с внутренней базой данных MySQL. Весь остальной трафик между интерфейсной и внутренней виртуальными машинами должен блокироваться.

Создайте правило для порта 22 с помощью команды [az network nsg rule create](/cli/azure/network/nsg/rule). Обратите внимание, что аргумент `--source-address-prefix` имеет значение *10.0.1.0/24*. Такая конфигурация гарантирует, что только трафик из интерфейсной подсети сможет пройти через группу безопасности сети.

```azurecli-interactive 
az network nsg rule create \
  --resource-group myRGNetwork \
  --nsg-name myBackendNSG \
  --name SSH \
  --access Allow \
  --protocol Tcp \
  --direction Inbound \
  --priority 100 \
  --source-address-prefix 10.0.1.0/24 \
  --source-port-range "*" \
  --destination-address-prefix "*" \
  --destination-port-range "22"
```

Теперь добавьте правило, разрешающее трафик MySQL через порт 3306.

```azurecli-interactive 
az network nsg rule create \
  --resource-group myRGNetwork \
  --nsg-name myBackendNSG \
  --name MySQL \
  --access Allow \
  --protocol Tcp \
  --direction Inbound \
  --priority 200 \
  --source-address-prefix 10.0.1.0/24 \
  --source-port-range "*" \
  --destination-address-prefix "*" \
  --destination-port-range "3306"
```

И наконец, так как группы безопасности сети имеют правила по умолчанию, разрешающие весь трафик между виртуальными машинами в той же виртуальной сети, вы можете создать правило блокировки всего трафика во внутренних группах безопасности сети. Обратите внимание, что аргумент `--priority` имеет значение *300*, что меньше значения приоритета правил NSG и MySQL. Такая конфигурация гарантирует, что трафик SSH и MySQL по-прежнему сможет проходить через группу безопасности сети.

```azurecli-interactive 
az network nsg rule create \
  --resource-group myRGNetwork \
  --nsg-name myBackendNSG \
  --name denyAll \
  --access Deny \
  --protocol Tcp \
  --direction Inbound \
  --priority 300 \
  --source-address-prefix "*" \
  --source-port-range "*" \
  --destination-address-prefix "*" \
  --destination-port-range "*"
```

## <a name="create-back-end-vm"></a>Создание внутренней виртуальной машины

Теперь создайте виртуальную машину и присоедините ее к подсети *myBackendSubnet*. Обратите внимание, что в качестве значения аргумента `--nsg` используются пустые двойные кавычки. Вместе с виртуальной машиной не нужно создавать группу безопасности сети. Виртуальная машина будет подключена к внутренней подсети, которая защищена с помощью предварительно созданной внутренней группы безопасности сети. Эта группа безопасности сети применяется к виртуальной машине. Кроме того, обратите внимание, что в качестве значения аргумента `--public-ip-address` используются пустые двойные кавычки. Эта конфигурация создает виртуальную машину без общедоступного IP-адреса. 

```azurecli-interactive 
az vm create \
  --resource-group myRGNetwork \
  --name myBackendVM \
  --vnet-name myVNet \
  --subnet myBackendSubnet \
  --public-ip-address "" \
  --nsg "" \
  --image UbuntuLTS \
  --generate-ssh-keys
```

Внутренняя виртуальная машина доступна только через порты *22* и *3306* из интерфейсной подсети. Весь остальной входящий трафик блокируется группой безопасности сети. Иногда есть смысл визуализировать конфигурации правил NSG. Получите конфигурацию правила NSG с помощью команды [az network rule list](/cli/azure/network/nsg/rule). 

```azurecli-interactive 
az network nsg rule list --resource-group myRGNetwork --nsg-name myBackendNSG --output table
```

## <a name="next-steps"></a>Дополнительная информация

В этом руководстве вы создали и защитили сети Azure с точки зрения виртуальных машин. Вы научились выполнять следующие задачи:

> [!div class="checklist"]
> * Создание виртуальной сети и подсети
> * Создание общедоступного IP-адреса
> * Создание интерфейсной виртуальной машины
> * защищают сетевой трафик;
> * Создание внутренней виртуальной машины

Перейдите к следующему руководству, чтобы узнать о защите данных на виртуальных машинах с помощью службы архивации Azure. 

> [!div class="nextstepaction"]
> [Архивация виртуальных машин Linux в Azure](./tutorial-backup-vms.md)
