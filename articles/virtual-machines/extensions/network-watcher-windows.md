---
title: Расширение виртуальной машины агента Наблюдателя за сетями Azure для Windows | Документация Майкрософт
description: Развертывание агента Наблюдателя за сетями на виртуальной машине Windows с помощью расширения виртуальной машины.
services: virtual-machines-windows
documentationcenter: ''
author: gurudennis
manager: amku
editor: ''
tags: azure-resource-manager
ms.assetid: 27e46af7-2150-45e8-b084-ba33de8c5e3f
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 02/14/2017
ms.author: dennisg
ms.openlocfilehash: 6e02f5a5b42da9c99a08782903cdc05ee32ec9d4
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "60743332"
---
# <a name="network-watcher-agent-virtual-machine-extension-for-windows"></a>Расширение виртуальной машины агента Наблюдателя за сетями для Windows

## <a name="overview"></a>Обзор

[Наблюдатель за сетями Azure](../../network-watcher/network-watcher-monitoring-overview.md) — это служба мониторинга производительности, диагностики и анализа сети, которая позволяет наблюдать за сетями Azure. Расширение виртуальной машины для агента службы "Наблюдатель за сетями" необходимо для записи трафика по требованию и других дополнительных функций виртуальных машин Azure.


В этом документе подробно описаны поддерживаемые платформы и параметры развертывания для расширения виртуальной машины агента Наблюдателя за сетями для Windows. Установка агента не прерывается и не требует перезагрузки виртуальной машины. Расширение можно развернуть на развертываемой виртуальной машине. Если виртуальная машина развернута службой Azure, необходимо проверить документацию службы на наличие разрешения на установку расширения для виртуальной машины.

## <a name="prerequisites"></a>Технические условия

### <a name="operating-system"></a>Операционная система

Расширение агента Наблюдателя за сетями для Windows может выполняться в выпусках Windows Server 2008 R2, Windows Server 2012, Windows Server 2012 R2 и Windows Server 2016. Nano Server в данный момент не поддерживается.

### <a name="internet-connectivity"></a>Подключение к Интернету

Для некоторых функциональных возможностей агента Наблюдателя за сетями требуется, чтобы целевая виртуальная машина была подключена к Интернету. Без возможности устанавливать исходящие подключения агент службы "Наблюдатель за сетями" не сможет передавать записанные пакеты в вашу учетную запись хранения. Дополнительные сведения см. в [документации по Наблюдателю за сетями](../../network-watcher/network-watcher-monitoring-overview.md).

## <a name="extension-schema"></a>Схема расширения

В следующем коде JSON показана схема для расширения агента Наблюдателя за сетями. Для этого расширения не требуется вводить какие-либо пользовательские параметры. Они не поддерживаются. Конфигурация расширения задана по умолчанию.

```json
{
    "type": "extensions",
    "name": "Microsoft.Azure.NetworkWatcher",
    "apiVersion": "[variables('apiVersion')]",
    "location": "[resourceGroup().location]",
    "dependsOn": [
        "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
    ],
    "properties": {
        "publisher": "Microsoft.Azure.NetworkWatcher",
        "type": "NetworkWatcherAgentWindows",
        "typeHandlerVersion": "1.4",
        "autoUpgradeMinorVersion": true
    }
}
```

### <a name="property-values"></a>Значения свойств

| ИМЯ | Значение и пример |
| ---- | ---- |
| apiVersion | 2015-06-15 |
| publisher | Microsoft.Azure.NetworkWatcher |
| type | NetworkWatcherAgentWindows |
| typeHandlerVersion | 1.4 |


## <a name="template-deployment"></a>Развертывание шаблона

Вы можете развернуть расширения виртуальной машины Azure с помощью шаблонов Azure Resource Manager. Чтобы запустить расширение агента службы "Наблюдатель за сетями" во время развертывания шаблона Azure Resource Manager, в нем можно использовать схему JSON, описанную в предыдущем разделе.

## <a name="powershell-deployment"></a>Развертывание с помощью PowerShell

Чтобы развернуть расширения виртуальной машины агента службы "Наблюдатель за сетями" на существующей виртуальной машине, выполните команду `Set-AzVMExtension`:

```powershell
Set-AzVMExtension `
  -ResourceGroupName "myResourceGroup1" `
  -Location "WestUS" `
  -VMName "myVM1" `
  -Name "networkWatcherAgent" `
  -Publisher "Microsoft.Azure.NetworkWatcher" `
  -Type "NetworkWatcherAgentWindows" `
  -TypeHandlerVersion "1.4"
```

## <a name="troubleshooting-and-support"></a>Устранение неполадок и поддержка

### <a name="troubleshooting"></a>Устранение неполадок

Сведения о состоянии развертывания расширения можно получить на портале Azure и при помощи PowerShell. Чтобы просмотреть состояние развертывания расширений для определенной виртуальной машины, выполните следующую команду в модуле Azure PowerShell:

```powershell
Get-AzVMExtension -ResourceGroupName myResourceGroup1 -VMName myVM1 -Name networkWatcherAgent
```

Выходные данные выполнения расширения регистрируются в файле, расположенном в следующем каталоге:

```cmd
C:\WindowsAzure\Logs\Plugins\Microsoft.Azure.NetworkWatcher.NetworkWatcherAgentWindows\
```

### <a name="support"></a>Поддержка

Если в любой момент при изучении этой статьи вам потребуется дополнительная помощь, вы можете ознакомиться с руководством пользователя для Наблюдателя за сетями или обратиться к экспертам по Azure на [форумах MSDN Azure и Stack Overflow](https://azure.microsoft.com/support/forums/). Кроме того, можно зарегистрировать обращение в службу поддержки Azure. Перейдите на [сайт поддержки Azure](https://azure.microsoft.com/support/options/) и щелкните "Получить поддержку". Дополнительные сведения об использовании службы поддержки Azure см. в статье [Часто задаваемые вопросы о поддержке Microsoft Azure](https://azure.microsoft.com/support/faq/).
