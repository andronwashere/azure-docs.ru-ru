---
title: Пример скрипта Azure CLI. Создание виртуальной машины Linux с помощью виртуального жесткого диска | Документы Майкрософт
description: Пример скрипта Azure CLI. Создание виртуальной машины с помощью виртуального жесткого диска.
services: virtual-machines-linux
documentationcenter: virtual-machines
author: allclark
manager: douge
editor: tysonn
tags: azure-service-management
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 03/09/2017
ms.author: douge
ms.custom: mvc
ms.openlocfilehash: 32f6794f6607fee04364d10a3f91128f169802b0
ms.sourcegitcommit: de47a27defce58b10ef998e8991a2294175d2098
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/15/2019
ms.locfileid: "67875430"
---
# <a name="create-a-vm-with-a-virtual-hard-disk"></a>Создание виртуальной машины с помощью виртуального жесткого диска

Этот пример создает виртуальную машину с помощью виртуального жесткого диска.
Он создает группу ресурсов, учетную запись хранение и контейнер, а затем создает виртуальную машину путем отправки виртуального жесткого диска в контейнер.
Он заменяет открытый ключ SSH вашим открытым ключом, чтобы предоставить вам доступ к виртуальной машине.

Потребуется загрузочный виртуальный жесткий диск. Скрипт ищет `~/sample.vhd`.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Пример скрипта

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/create-vm-vhd/create-vm-vhd.sh "Create VM using a VHD")]

## <a name="clean-up-deployment"></a>Очистка развертывания 

Выполните следующую команду, чтобы удалить группу ресурсов, виртуальную машину и все связанные с ней ресурсы.

```azurecli-interactive 
az group delete -n az-cli-vhd
```

## <a name="script-explanation"></a>Описание скрипта

Для создания группы ресурсов, виртуальной машины, группы доступности, балансировщика нагрузки и всех связанных ресурсов этот скрипт использует следующие команды. Для каждой команды в таблице приведены ссылки на соответствующую документацию.

| Команда | Примечания |
|---|---|
| [az group create](https://docs.microsoft.com/cli/azure/group) | Создает группу ресурсов, в которой хранятся все ресурсы. |
| [az storage account list](https://docs.microsoft.com/cli/azure/storage/account) | Выводит список учетных записей хранения. |
| [az storage account check-name](https://docs.microsoft.com/cli/azure/storage/account) | Проверяет допустимость имени учетной записи хранения и что она еще не существует. |
| [az storage account keys list](https://docs.microsoft.com/cli/azure/storage/account/keys) | Выводит список ключей для учетных записей хранения. |
| [az storage blob exists](https://docs.microsoft.com/cli/azure/storage/blob) | Проверяет, существует ли BLOB-объект. |
| [az storage container create](https://docs.microsoft.com/cli/azure/storage/container) | Создает контейнер в учетной записи хранения. |
| [az storage blob upload](https://docs.microsoft.com/cli/azure/storage/blob) | Создает BLOB-объект в контейнере путем отправки виртуального жесткого диска. |
| [az vm list](https://docs.microsoft.com/cli/azure/vm) | Используется с `--query`, чтобы проверить, используется ли имя виртуальной машины. | 
| [az vm create](https://docs.microsoft.com/cli/azure/vm/availability-set) | Создает виртуальные машины. |
| [az vm list-ip-addresses](https://docs.microsoft.com/cli/azure/vm#az-vm-list-ip-addresses) | Возвращает IP-адрес созданной виртуальной машины. |

## <a name="next-steps"></a>Дополнительная информация

Дополнительные сведения об Azure CLI см. в [документации по Azure CLI](https://docs.microsoft.com/cli/azure).

Дополнительные примеры скриптов интерфейса командной строки для виртуальных машин см. в [документации по виртуальным машинам Azure под управлением Linux](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
