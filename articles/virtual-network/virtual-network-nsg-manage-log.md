---
title: Журналы диагностики событий группы безопасности сети и счетчика правил в Azure
titlesuffix: Azure Virtual Network
description: Узнайте, как включить журналы диагностики событий и счетчика правил для группы безопасности сети Azure.
services: virtual-network
documentationcenter: na
author: KumudD
manager: twooley
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/04/2018
ms.author: kumud
ms.openlocfilehash: b3225d8d2f9eb7ccd0f4087d93cd9c1d940783d9
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "64714676"
---
# <a name="diagnostic-logging-for-a-network-security-group"></a>Журнал ведения диагностики для группы безопасности сети

Группа безопасности сети (NSG) содержит правила, которые разрешают или запрещают трафик для подсети виртуальной сети, сетевого интерфейса или и того, и другого. При включении журнала ведения диагностики для NSG можно записывать в журнал сведения следующих категорий.

* **Журналы событий.** Содержат записи, по которым правила NSG на основе MAC-адреса применяются к виртуальным машинам. Состояние этих правил регистрируется каждые 60 секунд.
* **Журналы счетчиков правил.** Содержат записи с информацией о том, сколько раз каждое правило NSG было применено для запрета или разрешения трафика.

Журналы диагностики доступны только для NSG, для которых применена модель развертывания с помощью Azure Resource Manager. Вы не сможете включить ведение журналов диагностики для NSG, развернутых с помощью классической модели развертывания. Чтобы получить более полное представление об этих двух моделях, прочитайте статью [Развертывание с помощью Azure Resource Manager и классическое развертывание: сведения о моделях развертывания и состоянии ресурсов](../resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

Журнал ведения диагностики следует включить отдельно для *каждой* NSG, для которой вы намерены собирать данные. Если вместо этого вам нужны журналы операций (действий), ознакомьтесь с [ведением журнала действий](../azure-monitor/platform/activity-logs-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json) Azure.

## <a name="enable-logging"></a>Включение ведения журналов

Чтобы журнал ведения диагностики, можно использовать [портал Azure](#azure-portal), [PowerShell](#powershell) или [Azure CLI](#azure-cli).

### <a name="azure-portal"></a>Портал Azure

1. Войдите на [портал](https://portal.azure.com).
2. Щелкните **Все службы**, затем введите *группы безопасности сети*. Когда элемент **Группы безопасности сети** появится в результатах поиска, выберите его.
3. Выберите NSG, для которой хотите включить ведение журнала.
4. В разделе **Мониторинг** выберите **Журналы диагностики** и выберите **Включить диагностику**, как показано на следующем рисунке.

   ![Включение диагностики](./media/virtual-network-nsg-manage-log/turn-on-diagnostics.png)

5. В разделе **Параметры диагностики** выберите или введите приведенные ниже сведения, а затем нажмите кнопку **Сохранить**.

    | Параметр                                                                                     | Значение                                                          |
    | ---------                                                                                   |---------                                                       |
    | ИМЯ                                                                                        | Имя на ваш выбор.  Например: *myNsgDiagnostics*.      |
    | **Архивировать в учетной записи хранения**, **Передать в концентратор событий** и **Отправить в Log Analytics** | Можно выбрать любые назначения. Узнайте больше в разделе [Целевое расположение для журналов](#log-destinations).                                                                                                                                           |
    | LOG                                                                                         | Выберите одну или обе категории журналов. Чтобы узнать больше о данных, записываемых в каждую категорию, ознакомьтесь с разделом [Категории журналов](#log-categories).                                                                                                                                             |
6. Просмотрите и проанализируйте журналы. Дополнительные сведения см. в разделе [Просмотр и анализ журналов](#view-and-analyze-logs).

### <a name="powershell"></a>PowerShell

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Вы можете выполнить приведенные ниже команды в [Azure Cloud Shell](https://shell.azure.com/powershell) или с помощью PowerShell на своем компьютере. Azure Cloud Shell — это бесплатная интерактивная оболочка. Она включает предварительно установленные общие инструменты Azure и настроена для использования с вашей учетной записью. Если вы запустите PowerShell на компьютере, требуется модуль Azure PowerShell версии 1.0.0 или более поздней версии. Выполните `Get-Module -ListAvailable Az` на компьютере, чтобы получить сведения об установленной версии. Если вам необходимо выполнить обновление, ознакомьтесь со статьей, посвященной [установке модуля Azure PowerShell](/powershell/azure/install-az-ps). Если вы используете PowerShell локально, необходимо запустить `Connect-AzAccount` для входа Azure с помощью учетной записи [необходимые разрешения](virtual-network-network-interface.md#permissions).

Чтобы включить журнал ведения диагностики, требуется идентификатор существующей NSG. Если у вас нет существующей группы безопасности сети, можно создать с [New AzNetworkSecurityGroup](/powershell/module/az.network/new-aznetworksecuritygroup).

Получить сведения о группе безопасности сети, необходимо включить ведение журналов диагностики для с [Get-AzNetworkSecurityGroup](/powershell/module/az.network/get-aznetworksecuritygroup). Например, чтобы получить NSG *myNsg* в группе ресурсов *myResourceGroup*, введите следующую команду.

```azurepowershell-interactive
$Nsg=Get-AzNetworkSecurityGroup `
  -Name myNsg `
  -ResourceGroupName myResourceGroup
```

Журналы диагностики можно записывать в назначения трех типов. Дополнительные сведения см. в разделе [Целевое расположение для журналов](#log-destinations). В этой статье журналы отправляются в *Log Analytics* в качестве примера. Получить существующую рабочую область Log Analytics с [Get-AzOperationalInsightsWorkspace](/powershell/module/az.operationalinsights/get-azoperationalinsightsworkspace). Например, чтобы получить рабочую область *myWorkspace* в группе ресурсов *myWorkspaces*, введите следующую команду:

```azurepowershell-interactive
$Oms=Get-AzOperationalInsightsWorkspace `
  -ResourceGroupName myWorkspaces `
  -Name myWorkspace
```

Если у вас нет существующей рабочей области, можно создать с [New AzOperationalInsightsWorkspace](/powershell/module/az.operationalinsights/new-azoperationalinsightsworkspace).

Существуют две категории, для которых можно включить журналы. Дополнительные сведения см. в разделе [Категории журналов](#log-categories). Включить ведение журнала диагностики для группы безопасности сети с помощью [AzDiagnosticSetting набора](/powershell/module/az.monitor/set-azdiagnosticsetting). Следующий пример записывает в журнал данные о категориях событий и счетчиков и сохраняет его в рабочую область для NSG, используя идентификаторы NSG и рабочей области, полученные ранее.

```azurepowershell-interactive
Set-AzDiagnosticSetting `
  -ResourceId $Nsg.Id `
  -WorkspaceId $Oms.ResourceId `
  -Enabled $true
```

Если требуется записывать данные только одной из категорий, добавьте в предыдущую команду параметр `-Categories` и *NetworkSecurityGroupEvent* или *NetworkSecurityGroupRuleCounter*. Если вы хотите записывать журналы не в рабочую область Log Analytics, а в другое [назначение](#log-destinations), используйте соответствующие параметры [учетной записи хранения](../azure-monitor/platform/archive-diagnostic-logs.md?toc=%2fazure%2fvirtual-network%2ftoc.json) или [концентратора событий](../azure-monitor/platform/diagnostic-logs-stream-event-hubs.md?toc=%2fazure%2fvirtual-network%2ftoc.json) Azure.

Просмотрите и проанализируйте журналы. Дополнительные сведения см. в разделе [Просмотр и анализ журналов](#view-and-analyze-logs).

### <a name="azure-cli"></a>Инфраструктура CLI Azure

Вы можете выполнить приведенные ниже команды в [Azure Cloud Shell](https://shell.azure.com/bash) или Azure CLI на своем компьютере. Azure Cloud Shell — это бесплатная интерактивная оболочка. Она включает предварительно установленные общие инструменты Azure и настроена для использования с вашей учетной записью. Для запуска CLI на компьютере требуется версия 2.0.38 или выше. Выполните `az --version` на компьютере, чтобы получить сведения об установленной версии. Если вам необходимо выполнить обновление, см. статью [Установка Azure CLI](/cli/azure/install-azure-cli?view=azure-cli-latest). Если CLI работает локально, необходимо также выполнить `az login`, чтобы войти в Azure с учетной записью, предоставляющей [необходимые разрешения](virtual-network-network-interface.md#permissions).

Чтобы включить журнал ведения диагностики, требуется идентификатор существующей NSG. Если у вас нет NSG, ее можно создать, выполнив команду [az network nsg create](/cli/azure/network/nsg#az-network-nsg-create).

Получите группу безопасности сети, для которой вы хотите включить журнал ведения диагностики, выполнив команду [az network nsg show](/cli/azure/network/nsg#az-network-nsg-show). Например, чтобы получить NSG *myNsg* в группе ресурсов *myResourceGroup*, введите следующую команду.

```azurecli-interactive
nsgId=$(az network nsg show \
  --name myNsg \
  --resource-group myResourceGroup \
  --query id \
  --output tsv)
```

Журналы диагностики можно записывать в назначения трех типов. Дополнительные сведения см. в разделе [Целевое расположение для журналов](#log-destinations). В этой статье журналы отправляются в *Log Analytics* в качестве примера. Дополнительные сведения см. в разделе [Категории журналов](#log-categories).

Включите журнал ведения диагностики для NSG с помощью команды [az monitor diagnostic-settings create](/cli/azure/monitor/diagnostic-settings#az-monitor-diagnostic-settings-create). Следующий пример записывает данные событий и категории счетчика в существующую рабочую область с именем *myWorkspace*, которая существует в группе ресурсов с именем *myWorkspaces*, а также идентификатор группы безопасности сети, который вы получили ранее:

```azurecli-interactive
az monitor diagnostic-settings create \
  --name myNsgDiagnostics \
  --resource $nsgId \
  --logs '[ { "category": "NetworkSecurityGroupEvent", "enabled": true, "retentionPolicy": { "days": 30, "enabled": true } }, { "category": "NetworkSecurityGroupRuleCounter", "enabled": true, "retentionPolicy": { "days": 30, "enabled": true } } ]' \
  --workspace myWorkspace \
  --resource-group myWorkspaces
```

Если у вас нет существующей рабочей области, ее можно создать с помощью [портала Azure](../azure-monitor/learn/quick-create-workspace.md?toc=%2fazure%2fvirtual-network%2ftoc.json) или [PowerShell](/powershell/module/az.operationalinsights/new-azoperationalinsightsworkspace). Существуют две категории, для которых можно включить журналы.

Если вы хотите регистрировать данные только для какой-то из категорий, удалите ненужную категорию с помощью предыдущей команды. Если вы хотите записывать журналы не в рабочую область Log Analytics, а в другое [назначение](#log-destinations), используйте соответствующие параметры [учетной записи хранения](../azure-monitor/platform/archive-diagnostic-logs.md?toc=%2fazure%2fvirtual-network%2ftoc.json) или [концентратора событий](../azure-monitor/platform/diagnostic-logs-stream-event-hubs.md?toc=%2fazure%2fvirtual-network%2ftoc.json) Azure.

Просмотрите и проанализируйте журналы. Дополнительные сведения см. в разделе [Просмотр и анализ журналов](#view-and-analyze-logs).

## <a name="log-destinations"></a>Целевое расположение для журналов

Назначения данных диагностики:
- [Запись в учетную запись хранения](../azure-monitor/platform/archive-diagnostic-logs.md?toc=%2fazure%2fvirtual-network%2ftoc.json) для аудита или проверки вручную. В параметрах диагностики ресурсов можно задать время хранения (в днях).
- [Потоковая передача журнала в концентратор событий](../azure-monitor/platform/diagnostic-logs-stream-event-hubs.md?toc=%2fazure%2fvirtual-network%2ftoc.json) для приема сторонней службой или пользовательским аналитическим решением, например PowerBI.
- [Записываются в журналы Azure Monitor](../azure-monitor/platform/collect-azure-metrics-logs.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-diagnostics-direct-to-log-analytics).

## <a name="log-categories"></a>Категории журналов

Данные в формате JSON записываются в журналы следующих категорий.

### <a name="event"></a>Событие

Журнал событий содержит сведения о том, какие правила NSG на основе MAC-адреса применяются к виртуальным машинам. Эти данные регистрируются для каждого события. В следующем примере данные зарегистрированы для виртуальной машины с IP-адресом 192.168.1.4 и MAC-адресом 00-0D-3A-92-6A-7C.

```json
{
    "time": "[DATE-TIME]",
    "systemId": "[ID]",
    "category": "NetworkSecurityGroupEvent",
    "resourceId": "/SUBSCRIPTIONS/[SUBSCRIPTION-ID]/RESOURCEGROUPS/[RESOURCE-GROUP-NAME]/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/[NSG-NAME]",
    "operationName": "NetworkSecurityGroupEvents",
    "properties": {
        "vnetResourceGuid":"[ID]",
        "subnetPrefix":"192.168.1.0/24",
        "macAddress":"00-0D-3A-92-6A-7C",
        "primaryIPv4Address":"192.168.1.4",
        "ruleName":"[SECURITY-RULE-NAME]",
        "direction":"[DIRECTION-SPECIFIED-IN-RULE]",
        "priority":"[PRIORITY-SPECIFIED-IN-RULE]",
        "type":"[ALLOW-OR-DENY-AS-SPECIFIED-IN-RULE]",
        "conditions":{
            "protocols":"[PROTOCOLS-SPECIFIED-IN-RULE]",
            "destinationPortRange":"[PORT-RANGE-SPECIFIED-IN-RULE]",
            "sourcePortRange":"[PORT-RANGE-SPECIFIED-IN-RULE]",
            "sourceIP":"[SOURCE-IP-OR-RANGE-SPECIFIED-IN-RULE]",
            "destinationIP":"[DESTINATION-IP-OR-RANGE-SPECIFIED-IN-RULE]"
            }
        }
}
```

### <a name="rule-counter"></a>Счетчик правил

Журнал счетчика правил содержит сведения о всех случаях применения правил к ресурсам. Данные, представленные в этом примере, регистрируются для каждого применения правила. В следующем примере данные зарегистрированы для виртуальной машины с IP-адресом 192.168.1.4 и MAC-адресом 00-0D-3A-92-6A-7C.

```json
{
    "time": "[DATE-TIME]",
    "systemId": "[ID]",
    "category": "NetworkSecurityGroupRuleCounter",
    "resourceId": "/SUBSCRIPTIONS/[SUBSCRIPTION ID]/RESOURCEGROUPS/[RESOURCE-GROUP-NAME]/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/[NSG-NAME]",
    "operationName": "NetworkSecurityGroupCounters",
    "properties": {
        "vnetResourceGuid":"[ID]",
        "subnetPrefix":"192.168.1.0/24",
        "macAddress":"00-0D-3A-92-6A-7C",
        "primaryIPv4Address":"192.168.1.4",
        "ruleName":"[SECURITY-RULE-NAME]",
        "direction":"[DIRECTION-SPECIFIED-IN-RULE]",
        "type":"[ALLOW-OR-DENY-AS-SPECIFIED-IN-RULE]",
        "matchedConnections":125
        }
}
```

> [!NOTE]
> Исходный IP-адрес для обмена данными записывается. Однако можно включить [ведение журнала потока NSG](../network-watcher/network-watcher-nsg-flow-logging-portal.md) для NSG. В этом случае в журнал будут записываться все сведения о счетчике правил, а также исходный IP-адрес, с которого был инициирован обмен данными. Данные журнала потоков NSG записываются в учетную запись хранения Azure. Данные можно анализировать с помощью функции [анализа трафика](../network-watcher/traffic-analytics.md) службы "Наблюдатель за сетями".

## <a name="view-and-analyze-logs"></a>Просмотр и анализ журналов

Узнайте, как просмотреть данные журнала диагностики, ознакомившись с [обзором журналов диагностики Azure](../azure-monitor/platform/diagnostic-logs-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json). Если данные диагностики отправляются в:
- **Журналы Azure Monitor**. то важные сведения можно получать с помощью решения для [анализа групп безопасности сети](../azure-monitor/insights/azure-networking-analytics.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-network-security-group-analytics-solution-in-azure-monitor
). Это решение наглядно представляет правила NSG, которые разрешают или запрещают трафик по MAC-адресу сетевого интерфейса в виртуальной машине.
- **учетную запись хранения,** то данные записываются в файл PT1H.json. Расположение журналов:
  - журнал событий: `insights-logs-networksecuritygroupevent/resourceId=/SUBSCRIPTIONS/[ID]/RESOURCEGROUPS/[RESOURCE-GROUP-NAME-FOR-NSG]/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/[NSG NAME]/y=[YEAR]/m=[MONTH/d=[DAY]/h=[HOUR]/m=[MINUTE]`
  - журнал счетчика правил: `insights-logs-networksecuritygrouprulecounter/resourceId=/SUBSCRIPTIONS/[ID]/RESOURCEGROUPS/[RESOURCE-GROUP-NAME-FOR-NSG]/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/[NSG NAME]/y=[YEAR]/m=[MONTH/d=[DAY]/h=[HOUR]/m=[MINUTE]`

## <a name="next-steps"></a>Дальнейшие действия

- Узнайте больше о [ведении журнала действий](../azure-monitor/platform/diagnostic-logs-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json), ранее известном как журнал аудита или операционный журнал. Ведение журнала действий включено по умолчанию для всех создаваемых NSG, независимо от модели развертывания Azure. Чтобы определить в журнале активности, какие операции были выполнены с группами безопасности сети, найдите записи, содержащие следующие типы ресурсов:
  - Microsoft.ClassicNetwork/networkSecurityGroups
  - Microsoft.ClassicNetwork/networkSecurityGroups/securityRules
  - Microsoft.Network/networkSecurityGroups
  - Microsoft.Network/networkSecurityGroups/securityRules
- Чтобы узнать, как вести журнал диагностики и записывать в него исходные IP-адреса для каждого потока, ознакомьтесь с разделом [Руководство по регистрации потока сетевого трафика на виртуальную машину и с нее с помощью портала Azure](../network-watcher/network-watcher-nsg-flow-logging-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json).