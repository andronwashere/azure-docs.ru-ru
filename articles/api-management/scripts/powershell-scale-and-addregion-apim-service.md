---
title: Пример скрипта Azure PowerShell. Масштабирование экземпляра службы | Документация Майкрософт
description: Пример скрипта Azure PowerShell. Масштабирование экземпляра службы
services: api-management
documentationcenter: ''
author: vladvino
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.devlang: na
ms.topic: sample
ms.date: 11/16/2017
ms.author: apimpm
ms.custom: mvc
ms.openlocfilehash: ec603ec7bd87062d19651fc13b8cf452a8cda2af
ms.sourcegitcommit: 1aefdf876c95bf6c07b12eb8c5fab98e92948000
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/06/2019
ms.locfileid: "66729720"
---
# <a name="scale-the-service-instance"></a>Масштабирование экземпляра службы

В этом примере скрипта выполняется масштабирование и добавляется регион в экземпляр службы управления API.

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Чтобы установить и использовать PowerShell локально для работы с этим руководством, вам понадобится модуль Azure PowerShell 1.0 или последующих версий. Чтобы узнать версию, выполните команду `Get-Module -ListAvailable Az`. Если вам необходимо выполнить обновление, ознакомьтесь со статьей, посвященной [установке модуля Azure PowerShell](/powershell/azure/install-Az-ps). Если модуль PowerShell запущен локально, необходимо также выполнить командлет `Connect-AzAccount`, чтобы создать подключение к Azure.

## <a name="sample-script"></a>Пример скрипта

[!code-powershell[main](../../../powershell_scripts/api-management/scale-and-addregion-apim-service/scale_and_addregion_apim_service.ps1 "Scale the APIM service instance")]

## <a name="clean-up-resources"></a>Очистка ресурсов

Вы можете удалить ставшие ненужными группу ресурсов и все связанные с ней ресурсы, выполнив команду [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup).

```azurepowershell-interactive
Remove-AzResourceGroup -Name myResourceGroup
```

## <a name="next-steps"></a>Дополнительная информация

Дополнительные сведения о модуле Azure PowerShell см. в [документации по Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview).

Дополнительные примеры скриптов Azure PowerShell для службы управления API Azure см. в [этой статье](../powershell-samples.md).
