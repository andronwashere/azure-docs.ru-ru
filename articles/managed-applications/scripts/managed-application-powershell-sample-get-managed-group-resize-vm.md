---
title: Пример скрипта Azure PowerShell. Получение ресурсов из управляемой группы ресурсов и изменение размера виртуальных машин | Документация Майкрософт
description: Пример скрипта Azure PowerShell. Получение ресурсов из управляемой группы ресурсов и изменение размера виртуальных машин
services: managed-applications
documentationcenter: na
author: tfitzmac
manager: timlt
ms.service: managed-applications
ms.devlang: powershell
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/27/2017
ms.author: tomfitz
ms.openlocfilehash: 9e8930c95495673c0082a82757ed6d8137900b6f
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/23/2019
ms.locfileid: "66171492"
---
# <a name="get-resources-in-a-managed-resource-group-and-resize-vms-with-powershell"></a>Получение ресурсов из управляемой группы ресурсов и изменение размера виртуальных машин с помощью Azure PowerShell

С помощью этого скрипта можно получить ресурсы из управляемой группы ресурсов и изменить размер виртуальных машин в этой группе.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Пример скрипта

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

[!code-powershell[main](../../../powershell_scripts/managed-applications/get-application/get-application.ps1 "Get application")]


## <a name="script-explanation"></a>Описание скрипта

В этом скрипте используются следующие команды для развертывания управляемого приложения. Для каждой команды в таблице приведены ссылки на соответствующую документацию.

| Команда | Примечания |
|---|---|
| [Get-AzManagedApplication](https://docs.microsoft.com/powershell/module/az.resources/get-azmanagedapplication) | Перечисление управляемых приложений Azure. Укажите имя группы ресурсов, чтобы отфильтровать результаты. |
| [Get-AzResource](https://docs.microsoft.com/powershell/module/az.resources/get-azresource) | Перечисление ресурсов. Укажите группу и тип ресурсов, чтобы отфильтровать результаты. |
| [Update-AzVM](https://docs.microsoft.com/powershell/module/az.compute/update-azvm) | Обновление размера виртуальной машины. |


## <a name="next-steps"></a>Дополнительная информация

* Общие сведения об управляемых приложениях Azure см. в [этой статье](../overview.md).
* Дополнительные сведения см. в [документации по Azure PowerShell](https://docs.microsoft.com/powershell/azure/get-started-azureps).
