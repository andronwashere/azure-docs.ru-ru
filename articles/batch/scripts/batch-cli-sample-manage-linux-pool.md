---
title: Пример скрипта Azure CLI для создания пула Linux в пакетной службе | Документация Майкрософт
description: 'Пример скрипта Azure CLI: создание пула Linux и управление им в пакетной службе Azure'
services: batch
documentationcenter: ''
author: laurenhughes
manager: gwallace
editor: ''
ms.assetid: ''
ms.service: batch
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 01/29/2018
ms.author: lahugh
ms.openlocfilehash: 13d519c1814cfa1e2038efd6c3828931e8332ec8
ms.sourcegitcommit: 4b431e86e47b6feb8ac6b61487f910c17a55d121
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/18/2019
ms.locfileid: "68322090"
---
# <a name="cli-example-create-and-manage-a-linux-pool-in-azure-batch"></a>Пример использования CLI. Создание пула Linux и управление им в пакетной службе Azure

В этом скрипте показаны некоторые из доступных команд Azure CLI для создания пулов вычислительных узлов Linux в пакетной службе Azure и управления ими.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Если вы решили установить и использовать CLI локально, для выполнения инструкций из этого руководства вам потребуется Azure CLI 2.0.20 или более поздней версии. Чтобы узнать версию, выполните команду `az --version`. Если вам необходимо выполнить установку или обновление, см. статью [Установка Azure CLI 2.0](/cli/azure/install-azure-cli). 

## <a name="example-script"></a>Пример сценария

[!code-azurecli-interactive[main](../../../cli_scripts/batch/manage-pool/manage-pool-linux.sh "Manage Linux Virtual Machine Pool")]

## <a name="clean-up-deployment"></a>Очистка развертывания

Выполните следующую команду, чтобы удалить группу ресурсов и все связанные с ней ресурсы.

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Описание скрипта

Этот скрипт использует следующие команды. Для каждой команды в таблице приведены ссылки на соответствующую документацию.

| Команда | Примечания |
|---|---|
| [az group create](/cli/azure/group#az-group-create) | Создает группу ресурсов, в которой хранятся все ресурсы. |
| [az batch account create](/cli/azure/batch/account#az-batch-account-create) | Создает учетную запись пакетной службы. |
| [az batch account login](/cli/azure/batch/account#az-batch-account-login) | Выполняет проверку подлинности с помощью указанной учетной записи пакетной службы для дальнейшего взаимодействия с интерфейсом командной строки.  |
| [az batch pool node-agent-skus list](https://docs.microsoft.com/cli/azure/batch/pool/node-agent-skus#az-batch-pool-node-agent-skus-list) | Отображает список доступных номеров SKU агента узла и сведения об образе.  |
| [az batch pool create](https://docs.microsoft.com/cli/azure/batch/pool#az-batch-pool-create) | Создает пул вычислительных узлов.  |
| [az batch pool resize](https://docs.microsoft.com/cli/azure/batch/pool#az-batch-pool-resize) | Изменяет количество работающих виртуальных машин в указанном пуле.  |
| [az batch pool show](https://docs.microsoft.com/cli/azure/batch/pool#az-batch-pool-show) | Отображает свойства пула.  |
| [az batch node list](https://docs.microsoft.com/cli/azure/batch/node#az-batch-node-list) | Отображает список всех вычислительных узлов в указанном пуле.  |
| [az batch node reboot](https://docs.microsoft.com/cli/azure/batch/node#az-batch-node-reboot) | Перезапускает указанный вычислительный узел.  |
| [az batch node delete](https://docs.microsoft.com/cli/azure/batch/node#az-batch-node-delete) | Удаляет указанные узлы из выбранного пула.  |
| [az group delete](/cli/azure/group#az-group-delete) | Удаляет группу ресурсов со всеми вложенными ресурсами. |

## <a name="next-steps"></a>Дополнительная информация

Дополнительные сведения об Azure CLI см. в [документации по Azure CLI](https://docs.microsoft.com/cli/azure).
