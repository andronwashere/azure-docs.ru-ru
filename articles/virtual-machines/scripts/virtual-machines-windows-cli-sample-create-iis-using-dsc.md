---
title: Пример скрипта Azure CLI. Создание виртуальной машины Windows Server 2016 с IIS с помощью DSC | Документация Майкрософт
description: Пример скрипта Azure CLI. Создание виртуальной машины Windows Server 2016 с IIS с помощью DSC
services: virtual-machines-windows
documentationcenter: virtual-machines
author: rickstercdn
manager: gwallace
editor: tysonn
tags: ''
ms.assetid: ''
ms.service: virtual-machines-windows
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 02/23/2017
ms.author: rclaus
ms.custom: mvc
ms.openlocfilehash: a17bb1fc4f2d297742620dc39756d2f89ec8df1e
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/09/2019
ms.locfileid: "67708288"
---
# <a name="create-a-vm-with-iis-using-dsc"></a>Создание виртуальной машины с IIS с помощью DSC

Этот сценарий создает виртуальную машину и использует расширение пользовательских сценариев DSC виртуальной машины Azure для установки и настройки IIS. 

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Пример скрипта

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/create-windows-iis-using-dsc/create-windows-iis-using-dsc.sh "Quick Create VM")]

## <a name="clean-up-deployment"></a>Очистка развертывания 

Выполните следующую команду, чтобы удалить группу ресурсов, виртуальную машину и все связанные с ней ресурсы.

```azurecli-interactive 
az group delete --name myResourceGroup --yes
```

## <a name="script-explanation"></a>Описание скрипта

Для создания группы ресурсов, виртуальной машины и всех связанных ресурсов этот скрипт использует следующие команды. Для каждой команды в таблице приведены ссылки на соответствующую документацию.

| Команда | Примечания |
|---|---|
| [az group create](https://docs.microsoft.com/cli/azure/group) | Создает группу ресурсов, в которой хранятся все ресурсы. |
| [az vm create](https://docs.microsoft.com/cli/azure/vm) | Создает виртуальную машину и подключает ее к сетевой карте, виртуальной сети, подсети и группе безопасности сети. Эта команда также указывает образ виртуальной машины, который будет использоваться, и учетные данные администратора.  |
| [az vm extension set](https://docs.microsoft.com/cli/azure/vm) | Добавляет расширение пользовательских скриптов на виртуальную машину, вызывающее скрипт для установки IIS. |
| [az vm open-port](https://docs.microsoft.com/cli/azure/vm) | Создает правило группы безопасности сети, разрешающее входящий трафик. В этом примере открывается порт 80 для трафика HTTP. |
| [az group delete](https://docs.microsoft.com/cli/azure/vm/extension) | Удаляет группу ресурсов со всеми вложенными ресурсами. |

## <a name="next-steps"></a>Дополнительная информация

Дополнительные сведения об Azure CLI см. в [документации по Azure CLI](https://docs.microsoft.com/cli/azure).

Дополнительные примеры скриптов интерфейса командной строки для виртуальных машин см. в [документации по виртуальным машинам Azure под управлением Windows](../windows/cli-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
