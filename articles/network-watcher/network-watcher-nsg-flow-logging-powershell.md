---
title: Управление журналами потоков для групп безопасности сети с помощью Наблюдателя за сетями (PowerShell) | Документация Майкрософт
description: На этой странице объясняется, как управлять журналами потоков для групп безопасности сети с помощью Наблюдателя за сетями и PowerShell
services: network-watcher
documentationcenter: na
author: KumudD
manager: twooley
editor: ''
ms.assetid: 2dfc3112-8294-4357-b2f8-f81840da67d3
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: kumud
ms.openlocfilehash: eb48ffcc0ef0f6113e3880fc34f70ffd165ddd99
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "64730312"
---
# <a name="configuring-network-security-group-flow-logs-with-powershell"></a>Настройка журналов потоков для групп безопасности сети с помощью PowerShell

> [!div class="op_single_selector"]
> - [портал Azure](network-watcher-nsg-flow-logging-portal.md)
> - [PowerShell](network-watcher-nsg-flow-logging-powershell.md)
> - [Интерфейс командной строки Azure](network-watcher-nsg-flow-logging-cli.md)
> - [REST API](network-watcher-nsg-flow-logging-rest.md)

Журналы потоков для групп безопасности сети — это компонент Наблюдателя за сетями, который позволяет просматривать сведения о входящем и исходящем IP-трафике через группу безопасности сети. Эти журналы потоков записываются в формате JSON. В них отображаются входящие и исходящие потоки по каждому правилу, сетевая карта, с которой связан поток, сведения о 5 кортежах потока (IP-адрес источника и места назначения, порт источника и места назначения, протокол), а также сведения о состоянии трафика (разрешен или запрещен).

## <a name="register-insights-provider"></a>Регистрация поставщика Microsoft Insights

Для успешного ведения журналов потоков должен быть зарегистрирован поставщик **Microsoft.Insights**. Если вы не знаете, зарегистрирован ли поставщик **Microsoft.Insights**, выполните следующий сценарий.

```powershell
Register-AzResourceProvider -ProviderNamespace Microsoft.Insights
```

## <a name="enable-network-security-group-flow-logs-and-traffic-analytics"></a>Включение Аналитики трафика и журналов потоков для группы безопасности сети

Команда для включения журналов потоков показана в следующем примере.

```powershell
$NW = Get-AzNetworkWatcher -ResourceGroupName NetworkWatcherRg -Name NetworkWatcher_westcentralus
$nsg = Get-AzNetworkSecurityGroup -ResourceGroupName nsgRG -Name nsgName
$storageAccount = Get-AzStorageAccount -ResourceGroupName StorageRG -Name contosostorage123
Get-AzNetworkWatcherFlowLogStatus -NetworkWatcher $NW -TargetResourceId $nsg.Id

#Traffic Analytics Parameters
$workspaceResourceId = "/subscriptions/bbbbbbbb-bbbb-bbbb-bbbb-bbbbbbbbbbbb/resourcegroups/trafficanalyticsrg/providers/microsoft.operationalinsights/workspaces/taworkspace"
$workspaceGUID = "cccccccc-cccc-cccc-cccc-cccccccccccc"
$workspaceLocation = "westeurope"

#Configure Version 1 Flow Logs
Set-AzNetworkWatcherConfigFlowLog -NetworkWatcher $NW -TargetResourceId $nsg.Id -StorageAccountId $storageAccount.Id -EnableFlowLog $true -FormatType Json -FormatVersion 1

#Configure Version 2 Flow Logs, and configure Traffic Analytics
Set-AzNetworkWatcherConfigFlowLog -NetworkWatcher $NW -TargetResourceId $nsg.Id -StorageAccountId $storageAccount.Id -EnableFlowLog $true -FormatType Json -FormatVersion 2

#Configure Version 2 FLow Logs with Traffic Analytics Configured
Set-AzNetworkWatcherConfigFlowLog -NetworkWatcher $NW -TargetResourceId $nsg.Id -StorageAccountId $storageAccount.Id -EnableFlowLog $true -FormatType Json -FormatVersion 2 -EnableTrafficAnalytics -WorkspaceResourceId $workspaceResourceId -WorkspaceGUID $workspaceid -WorkspaceLocation $workspaceRegion

#Query Flow Log Status
Get-AzNetworkWatcherFlowLogStatus -NetworkWatcher $NW -TargetResourceId $nsg.Id
```

В указанной учетной записи хранения не может быть настроенных сетевых правил, ограничивающих доступ к сети только службами Майкрософт или конкретными виртуальными сетями. Учетная запись хранения может быть в той же подписке Azure, что и NSG, для которой включается журнал потока, или в другой подписке Azure. Если используются разные подписки, они должны быть связаны с одним клиентом Azure Active Directory. Используемая учетная запись для каждой подписки должна иметь [необходимые разрешения](required-rbac-permissions.md).

## <a name="disable-traffic-analytics-and-network-security-group-flow-logs"></a>Отключение Аналитики трафика и журналов потоков для группы безопасности сети

Чтобы отключить журналы потоков и аналитику трафика, используйте следующий пример:

```powershell
#Disable Traffic Analaytics by removing -EnableTrafficAnalytics property
Set-AzNetworkWatcherConfigFlowLog -NetworkWatcher $NW -TargetResourceId $nsg.Id -StorageAccountId $storageAccount.Id -EnableFlowLog $true -FormatType Json -FormatVersion 2 -WorkspaceResourceId $workspaceResourceId -WorkspaceGUID $workspaceGUID -WorkspaceLocation $workspaceLocation

#Disable Flow Logging
Set-AzNetworkWatcherConfigFlowLog -NetworkWatcher $NW -TargetResourceId $nsg.Id -StorageAccountId $storageAccount.Id -EnableFlowLog $false
```

## <a name="download-a-flow-log"></a>Скачивание журнала потоков

Место хранения журнала потоков определяется при его создании. Удобное средство для доступа к этим журналам потоков, сохраненным в учетной записи хранения, — обозреватель службы хранилища Microsoft Azure, который можно скачать по адресу https://storageexplorer.com/

При указании учетной записи хранения файлы журнала потоков сохраняются в ней по следующему адресу:

```
https://{storageAccountName}.blob.core.windows.net/insights-logs-networksecuritygroupflowevent/resourceId=/SUBSCRIPTIONS/{subscriptionID}/RESOURCEGROUPS/{resourceGroupName}/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/{nsgName}/y={year}/m={month}/d={day}/h={hour}/m=00/macAddress={macAddress}/PT1H.json
```

Сведения о структуре журнала см. в статье [Network Security Group Flow log Overview](network-watcher-nsg-flow-logging-overview.md) (Обзор журнала потоков для группы безопасности сети).

## <a name="next-steps"></a>Следующие шаги

Узнайте, как [визуализировать сведения журналов потоков группы безопасности сети с помощью PowerBI](network-watcher-visualize-nsg-flow-logs-power-bi.md).

Узнайте, как [визуализировать сведения журналов потоков группы безопасности сети с помощью средств с открытым кодом](network-watcher-visualize-nsg-flow-logs-open-source-tools.md).
