---
title: Пример сценария Azure PowerShell для изменения диапазона портов RDP | Документация Майкрософт
description: Пример сценария Azure PowerShell для изменения диапазона портов RDP развернутого кластера.
services: service-fabric
documentationcenter: ''
author: athinanthny
manager: chackdan
editor: ''
tags: azure-service-management
ms.assetid: ''
ms.service: service-fabric
ms.workload: multiple
ms.devlang: na
ms.topic: sample
ms.date: 03/19/2018
ms.author: atsenthi
ms.custom: mvc
ms.openlocfilehash: 4470cf8898f1d174f8ec2bb6860e2e30aeff9fe2
ms.sourcegitcommit: fe6b91c5f287078e4b4c7356e0fa597e78361abe
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/29/2019
ms.locfileid: "68592315"
---
# <a name="update-the-rdp-port-range-values"></a>Обновление значений диапазона портов RDP

Этот сценарий изменяет значения диапазона портов RDP на виртуальных машинах узла кластера после развертывания кластера.  Используется Azure PowerShell, чтобы порты базовых виртуальных машин не сменялись циклически.  Сценарий возвращает ресурс `Microsoft.Network/loadBalancers` в группе ресурсов кластера и обновляет значения `inboundNatPools.frontendPortRangeStart` и `inboundNatPools.frontendPortRangeEnd`. Измените параметры, если это необходимо.

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

При необходимости установите Azure PowerShell с помощью инструкции, приведенной в [руководстве по Azure PowerShell](/powershell/azure/overview). 

## <a name="sample-script"></a>Пример скрипта

[!code-powershell[main](../../../powershell_scripts/service-fabric/change-rdp-port-range/change-rdp-port-range.ps1 "Update the RDP port range values")]

## <a name="script-explanation"></a>Описание скрипта

Этот скрипт использует следующие команды. Для каждой команды в таблице приведены ссылки на соответствующую документацию.

| Команда | Примечания |
|---|---|
| [Get-AzResource](/powershell/module/az.resources/get-azresource) | Возвращает ресурс `Microsoft.Network/loadBalancers`. |
|[Set-AzResource](/powershell/module/az.resources/set-azresource)|Обновляет ресурс `Microsoft.Network/loadBalancers`.|

## <a name="next-steps"></a>Дополнительная информация

Дополнительные сведения о модуле Azure PowerShell см. в [документации по Azure PowerShell](/powershell/azure/overview).

Дополнительные примеры сценариев Azure PowerShell для Azure Service Fabric см. в разделе [Примеры сценариев Azure PowerShell](../service-fabric-powershell-samples.md).
