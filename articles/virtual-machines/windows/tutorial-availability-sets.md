---
title: Руководство. Виртуальные машины Windows высокого уровня доступности в Azure | Документация Майкрософт
description: Из этого руководства вы узнаете, как развертывать виртуальные машины высокого уровня доступности в группах доступности с помощью Azure PowerShell
documentationcenter: ''
services: virtual-machines-windows
author: cynthn
manager: gwallace
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: tutorial
ms.date: 11/30/2018
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: d9e3fc52b9cdf5126cb8d58778a59b8e8691f80a
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/09/2019
ms.locfileid: "67708150"
---
# <a name="tutorial-create-and-deploy-highly-available-virtual-machines-with-azure-powershell"></a>Руководство по Создание и развертывание виртуальных машин высокого уровня доступности с помощью Azure PowerShell

В этом руководстве показано, как повысить доступность и надежность виртуальных машин с помощью групп доступности. Группы доступности распределяют развернутые в Azure виртуальные машины между несколькими изолированными аппаратными узлами в кластере. 

Из этого руководства вы узнаете, как выполнять следующие задачи:

> [!div class="checklist"]
> * "Создать группу доступности"
> * Создание виртуальной машины в группе доступности
> * Проверка доступных размеров виртуальных машин.
> * Проверка службы "Помощник по Azure"


## <a name="availability-set-overview"></a>Обзор групп доступности

Группа доступности — это возможность логического группирования для изоляции ресурсов виртуальной машины друг от друга при их развертывании. Azure гарантирует, что виртуальные машины, размещенные в группе доступности, выполняются на нескольких физических серверах, в вычислительных стойках, в пределах единиц хранения и сетевых коммутаторов. При сбое программного обеспечения или оборудования будет затронуто только подмножество ваших виртуальных машин, а все решение продолжит функционировать. Группы доступности крайне важны для создания надежных облачных решений.

Рассмотрим стандартное решение на основе виртуальной машины, которое может включать четыре внешних веб-сервера и две внутренние виртуальные машины. С помощью Azure вы можете определить две группы доступности перед развертыванием виртуальных машин: группу доступности для веб-уровня и группу доступности для внутреннего уровня. При создании виртуальной машины необходимо указать группу доступности в качестве параметра. В таком случае Azure изолирует виртуальные машины в нескольких ресурсах физического оборудования. Если возникает проблема с физическим оборудованием, на котором запущен один из серверов, то другие экземпляры серверов продолжат работу, так как они используют другое оборудование.

Если требуется развернуть надежные решения на основе виртуальной машины в Azure, используйте группы доступности.

## <a name="launch-azure-cloud-shell"></a>Запуск Azure Cloud Shell

Azure Cloud Shell — это бесплатная интерактивная оболочка, с помощью которой можно выполнять действия, описанные в этой статье. Она включает предварительно установленные общие инструменты Azure и настроена для использования с вашей учетной записью. 

Чтобы открыть Cloud Shell, просто выберите **Попробовать** в правом верхнем углу блока кода. Cloud Shell можно также запустить в отдельной вкладке браузера, перейдя на страницу [https://shell.azure.com/powershell](https://shell.azure.com/powershell). Нажмите кнопку **Копировать**, чтобы скопировать блоки кода. Вставьте код в Cloud Shell и нажмите клавишу "ВВОД", чтобы выполнить его.

## <a name="create-an-availability-set"></a>"Создать группу доступности"

Оборудование в расположении делится на несколько доменов обновления и доменов сбоя. **Домен обновления** — это группа виртуальных машин и базового физического оборудования, которую можно перезагрузить одновременно. Виртуальные машины в одном **домене сбоя** совместно используют общее хранилище, а также общий источник питания и сетевой коммутатор.  

Создать группу доступности можно с помощью командлета [New-AzAvailabilitySet](https://docs.microsoft.com/powershell/module/az.compute/new-azavailabilityset). В этом примере количество доменов сбоя и обновления — *2*, а группа доступности называется *myAvailabilitySet*.

Создайте группу ресурсов.

```azurepowershell-interactive
New-AzResourceGroup `
   -Name myResourceGroupAvailability `
   -Location EastUS
```

Создайте управляемую группу доступности с помощью командлета [New-AzAvailabilitySet](https://docs.microsoft.com/powershell/module/az.compute/new-azavailabilityset) с параметром `-sku aligned`.

```azurepowershell-interactive
New-AzAvailabilitySet `
   -Location "EastUS" `
   -Name "myAvailabilitySet" `
   -ResourceGroupName "myResourceGroupAvailability" `
   -Sku aligned `
   -PlatformFaultDomainCount 2 `
   -PlatformUpdateDomainCount 2
```

## <a name="create-vms-inside-an-availability-set"></a>Создание виртуальных машин в группе доступности
Чтобы виртуальные машины правильно распределялись по оборудованию, их нужно создать внутри группы доступности. Существующую виртуальную машину невозможно добавить в группу доступности после создания. 


При создании конфигурации виртуальной машины с помощью командлета [New-AzVM](https://docs.microsoft.com/powershell/module/az.compute/new-azvm) можно использовать параметр `-AvailabilitySetName`, чтобы указать имя группы доступности.

Сначала укажите имя и пароль администратора для виртуальной машины с помощью командлета [Get-Credential](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.security/Get-Credential):

```azurepowershell-interactive
$cred = Get-Credential
```

Создайте две виртуальные машины в группе доступности с помощью командлета [New-AzVM](https://docs.microsoft.com/powershell/module/az.compute/new-azvm).

```azurepowershell-interactive
for ($i=1; $i -le 2; $i++)
{
    New-AzVm `
        -ResourceGroupName "myResourceGroupAvailability" `
        -Name "myVM$i" `
        -Location "East US" `
        -VirtualNetworkName "myVnet" `
        -SubnetName "mySubnet" `
        -SecurityGroupName "myNetworkSecurityGroup" `
        -PublicIpAddressName "myPublicIpAddress$i" `
        -AvailabilitySetName "myAvailabilitySet" `
        -Credential $cred
}
```

Создание и настройка двух виртуальных машин занимает несколько минут. По завершении у вас будут две виртуальные машины, распределенные по базовому оборудованию. 

Если посмотреть на группу доступности на портале, перейдя в раздел **Группы ресурсов** > **myResourceGroupAvailability** > **myAvailabilitySet**, можно увидеть, как виртуальные машины распределены между двумя доменами сбоя и обновления.

![Группа доступности на портале](./media/tutorial-availability-sets/fd-ud.png)

## <a name="check-for-available-vm-sizes"></a>Знакомство с доступными размерами виртуальной машины 

Позднее вы можете добавить в группу доступности другие виртуальные машины, но следует понимать, какие размеры виртуальных машин доступны на оборудовании. Выполните командлет [Get-AzVMSize](https://docs.microsoft.com/powershell/module/az.compute/get-azvmsize) для получения списка всех доступных размеров в аппаратном кластере для группы доступности.

```azurepowershell-interactive
Get-AzVMSize `
   -ResourceGroupName "myResourceGroupAvailability" `
   -AvailabilitySetName "myAvailabilitySet"
```

## <a name="check-azure-advisor"></a>Проверка службы "Помощник по Azure" 

Службу "Помощник по Azure" можно также использовать для получения дополнительных сведений о том, как повысить доступность виртуальных машин. Помощник по Azure анализирует конфигурацию и данные телеметрии использования и рекомендует решения, которые помогут повысить эффективность затрат, производительность, уровень доступности и безопасности ресурсов Azure.

Войдите на [портал Azure](https://portal.azure.com), выберите **Все службы** и введите **Помощник**. Панель мониторинга службы "Помощник" отображает персонализированные рекомендации для выбранной подписки. Дополнительные сведения можно найти в разделе [Приступая к работе с Azure Advisor](../../advisor/advisor-get-started.md).


## <a name="next-steps"></a>Дополнительная информация

Из этого руководства вы узнали, как выполнить следующие задачи:

> [!div class="checklist"]
> * "Создать группу доступности"
> * Создание виртуальной машины в группе доступности
> * Проверка доступных размеров виртуальных машин.
> * Проверка службы "Помощник по Azure"

Перейдите к следующему руководству, чтобы узнать о масштабируемых наборах виртуальных машин.

> [!div class="nextstepaction"]
> [Создание масштабируемого набора виртуальных машин](tutorial-create-vmss.md)


