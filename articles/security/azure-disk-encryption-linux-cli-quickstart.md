---
title: Создание и шифрование виртуальной машины Linux с помощью Azure CLI
description: В этом кратком руководстве содержатся сведения о создании и шифровании виртуальной машины Linux с помощью Azure CLI.
author: msmbaldwin
ms.author: mbaldwin
ms.service: security
ms.topic: quickstart
ms.date: 05/17/2019
ms.openlocfilehash: c0ae2f6588ede0305e02edf1942ff196aeb807bb
ms.sourcegitcommit: 08138eab740c12bf68c787062b101a4333292075
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/22/2019
ms.locfileid: "67331583"
---
# <a name="quickstart-create-and-encrypt-a-linux-vm-with-the-azure-cli"></a>Краткое руководство. Создание и шифрование виртуальной машины Linux с помощью Azure CLI

Azure CLI используется для создания ресурсов Azure и управления ими из командной строки или с помощью скриптов. В этом кратком руководстве показано, как с помощью Azure CLI создать и зашифровать виртуальную машину Linux.

Если у вас еще нет подписки Azure, [создайте бесплатную учетную запись Azure](https://azure.microsoft.com/free/?WT.mc_id=A261C142F), прежде чем начинать работу.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Если вы решили установить и использовать CLI локально, для выполнения инструкций из этого руководства вам потребуется Azure CLI 2.0.30 или более поздней версии. Чтобы узнать версию, выполните команду `az --version`. Если вам необходимо выполнить установку или обновление, см. статью [Установка Azure CLI 2.0]( /cli/azure/install-azure-cli).

## <a name="create-a-resource-group"></a>Создание группы ресурсов

Создайте группу ресурсов с помощью команды [az group create](/cli/azure/group?view=azure-cli-latest#az-group-create). Группа ресурсов Azure является логическим контейнером, в котором происходит развертывание ресурсов Azure и управление ими. В следующем примере создается группа ресурсов с именем *myResourceGroup* в расположении *eastus*.

```azurecli-interactive
az group create --name myResourceGroup --location eastus
```

## <a name="create-a-virtual-machine"></a>Создание виртуальной машины

Создайте виртуальную машину с помощью команды [az vm create](https://docs.microsoft.com/cli/azure/vm?view=azure-cli-latest#az-vm-create). В следующем примере создается виртуальная машина с именем *myVM*.

```azurecli-interactive
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image Canonical:UbuntuServer:16.04-LTS:latest \
    --size Standard_D2S_V3
```

Создание виртуальной машины и вспомогательных ресурсов занимает несколько минут. В следующем примере выходных данных показано, что виртуальная машина успешно создана.

```azurecli-interactive
{
  "fqdns": "",
  "id": "/subscriptions/<guid>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM",
  "location": "eastus",
  "macAddress": "00-0D-3A-23-9A-49",
  "powerState": "VM running",
  "privateIpAddress": "10.0.0.4",
  "publicIpAddress": "52.174.34.95",
  "resourceGroup": "myResourceGroup"
}
```

## <a name="create-a-key-vault-configured-for-encryption-keys"></a>Создание Key Vault, настроенного для ключей шифрования

Шифрование дисков Azure хранит ключи шифрования в Azure Key Vault. Создайте Key Vault, используя команду [az keyvault create](/cli/azure/keyvault?view=azure-cli-latest#az-keyvault-create). Чтобы включить в Key Vault возможность хранения ключей шифрования, используйте параметр --enabled-for-disk-encryption.

> [!Important]
> Каждый Key Vault должен иметь уникальное имя. В следующем примере показан процесс создания Key Vault с именем *myKV*. Назовите свое хранилище ключей иначе.

```azurecli
az keyvault create --name "myKV" --resource-group "myResourceGroup" --location eastus --enabled-for-disk-encryption
```

## <a name="encrypt-the-virtual-machine"></a>Шифрование виртуальной машины

Зашифруйте виртуальную машину, выполнив команду [az vm encryption](/cli/azure/vm/encryption?view=azure-cli-latest) и предоставив уникальное имя хранилища ключей в параметр --disk-encryption-keyvault.

```azurecli-interactive
az vm encryption enable -g MyResourceGroup --name MyVM --disk-encryption-keyvault myKV
```

Через некоторое время процесс вернет сообщение: The encryption request was accepted. Please use 'show' command to monitor the progress (Запрос на шифрование принят. Выполните команду для просмотра, чтобы отследить прогресс.). Командой для просмотра является команда [az vm show](/cli/azure/vm/encryption#az-vm-encryption-show).

```azurecli-interactive
az vm show --name MyVM -g MyResourceGroup
```

Если шифрование включено, в полученных выходных данных вы увидите следующий код:

```azurecli-interactive
"EncryptionOperation": "EnableEncryption"
```

## <a name="clean-up-resources"></a>Очистка ресурсов

Когда группа ресурсов, виртуальная машина и Key Vault станут ненужными, их можно удалить с помощью команды [az group delete](/cli/azure/group). 

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Дополнительная информация

В рамках этого краткого руководства вы создали виртуальную машину, включенный для ключей шифрования Key Vault и зашифровали виртуальную машину.  Перейдите к следующей статье, чтобы узнать больше о предварительных требованиях для шифрования дисков Azure для виртуальных машин IaaS.

> [!div class="nextstepaction"]
> [Предварительные требования для шифрования дисков Azure](azure-security-disk-encryption-prerequisites.md)
