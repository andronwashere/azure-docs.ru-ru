---
title: Установка Symantec Endpoint Protection на виртуальную машину Windows в Azure | Документация Майкрософт
description: Узнайте, как установить и настроить расширение безопасности Symantec Endpoint Protection на новой или существующей виртуальной машине Azure, созданной с помощью классической модели развертывания.
services: virtual-machines-windows
documentationcenter: ''
author: roiyz
manager: gwallace
editor: ''
tags: azure-service-management
ms.assetid: 19dcebc7-da6b-4510-907b-d64088e81fa2
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-multiple
ms.devlang: na
ms.topic: article
ms.date: 03/31/2017
ms.author: roiyz
ms.openlocfilehash: d79e46467c24277200ef72bb64e8c5b7427bf269
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/09/2019
ms.locfileid: "67705878"
---
# <a name="how-to-install-and-configure-symantec-endpoint-protection-on-a-windows-vm"></a>Установка и настройка Symantec Endpoint Protection на виртуальной машине Windows
> [!IMPORTANT] 
> В Azure предлагаются две модели развертывания для создания ресурсов и работы с ними: [модель развертывания с помощью Resource Manager и классическая модель](../../azure-resource-manager/resource-manager-deployment-model.md). В этой статье рассматривается использование классической модели развертывания. Для большинства новых развертываний Майкрософт рекомендует использовать модель диспетчера ресурсов.

В этой статье показывается, как можно установить и настроить клиент Symantec Endpoint Protection на существующей виртуальной машине под управлением Windows Server. Этот полный клиент включает такие службы, как защита от вирусов и шпионских программ, брандмауэр и система предотвращения вторжений. Клиент устанавливается как модуль безопасности с помощью агента ВМ.

При наличии действительной подписки от Symantec для локального решения можно использовать ее для защиты виртуальных машин Azure. Если у вас еще нет подписки, можно зарегистрироваться для получения пробной подписки. Дополнительные сведения об этом решении см. в разделе [Symantec Endpoint Protection на платформе Microsoft Azure][Symantec]. На этой странице также приведены ссылки на сведения о лицензировании и инструкции по установке клиента для тех, кто уже использует Symantec.

## <a name="install-symantec-endpoint-protection-on-an-existing-vm"></a>Установка Symantec Endpoint Protection на существующей виртуальной машине
Для этого потребуются следующие компоненты.

* Модуль Azure PowerShell версии не ниже 0.8.2, установленный на рабочем компьютере. Установленную версию Azure PowerShell можно проверить с помощью команды **Get-Module azure | format-table version**. Инструкции и ссылку на последнюю версию см. в статье [Установка и настройка служб Azure PowerShell][PS]. Войдите в свою подписку Azure с помощью `Add-AzureAccount`.
* Агент VM запущен на виртуальной машине Azure.

Сначала убедитесь, что агент VM уже установлен на виртуальной машине. Укажите имя облачной службы и имя виртуальной машины, а затем выполните следующие команды в командной строке Azure PowerShell уровня администратора. Замените все содержимое внутри кавычек, включая знаки < и >.

> [!TIP]
> Если вы не знаете имена облачной службы и виртуальной машины, выполните командлет **Get-AzureVM** и просмотрите список имен всех виртуальных машин в вашей текущей подписке.

```powershell
$CSName = "<cloud service name>"
$VMName = "<virtual machine name>"
$vm = Get-AzureVM -ServiceName $CSName -Name $VMName
write-host $vm.VM.ProvisionGuestAgent
```

Если команда **write-host** отображает значение **True**, агент ВМ установлен. Если она отображает **False**, см. в разделе инструкции и ссылку для скачивания в записи блога Azure [агент виртуальной Машины и расширения — часть 2][Agent].

Если агент ВМ установлен, выполните следующие команды, чтобы установить агент Symantec Endpoint Protection.

```powershell
$Agent = Get-AzureVMAvailableExtension -Publisher Symantec -ExtensionName SymantecEndpointProtection

Set-AzureVMExtension -Publisher Symantec –Version $Agent.Version -ExtensionName SymantecEndpointProtection \
    -VM $vm | Update-AzureVM
```

Чтобы проверить, установлен ли модуль безопасности Symantec, и убедиться в актуальности его версии, сделайте следующее:

1. Войдите в виртуальную машину. Инструкции см. в статье [Вход в виртуальную машину под управлением Windows с помощью классического портала Azure][Logon].
2. Для Windows Server 2008 R2: щелкните **Пуск > Symantec Endpoint Protection**. Для Windows Server 2012 или Windows Server 2012 R2: на начальном экране введите **Symantec**, а затем щелкните **Symantec Endpoint Protection**.
3. На вкладке **Состояние** окна **Status-Symantec Endpoint Protection** (Состояние Symantec Endpoint Protection) примените обновления или выполните перезагрузку, если это необходимо.

## <a name="additional-resources"></a>Дополнительные ресурсы
[Вход в виртуальную машину под управлением Windows с помощью классического портала Azure][Logon]

[Расширения и компоненты виртуальных машин Azure][Ext]

<!--Link references-->
[Symantec]: https://www.symantec.com/connect/blogs/symantec-endpoint-protection-now-microsoft-azure

[Create]:../windows/classic/tutorial.md

[PS]: /powershell/azureps-cmdlets-docs

[Agent]: https://go.microsoft.com/fwlink/p/?LinkId=403947

[Logon]:../windows/classic/connect-logon.md

[Ext]: https://go.microsoft.com/fwlink/p/?linkid=390493
