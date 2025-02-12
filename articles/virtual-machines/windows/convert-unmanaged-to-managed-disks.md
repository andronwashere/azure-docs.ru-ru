---
title: Управляемые диски Azure. Преобразование виртуальной машины Windows с неуправляемыми дисками для использования управляемых дисков | Документация Майкрософт
description: Переключение виртуальной машины Windows, развернутой в рамках модели Resource Manager, с неуправляемых дисков на управляемые диски с помощью PowerShell.
services: virtual-machines-windows
documentationcenter: ''
author: roygara
manager: twooley
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 07/12/2018
ms.author: rogarana
ms.openlocfilehash: 9cbee4f9f4f694510e852fe3790c8242ef346576
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "66416051"
---
# <a name="convert-a-windows-virtual-machine-from-unmanaged-disks-to-managed-disks"></a>Переключение виртуальной машины Windows с неуправляемых дисков на управляемые диски

При наличии виртуальных машин Windows, использующих неуправляемые диски, их можно преобразовать для использования управляемых дисков с помощью службы [Управляемые диски Azure](managed-disks-overview.md). При этом преобразуются диск операционной системы и все подключенные диски данных.

[!INCLUDE [updated-for-az.md](../../../includes/updated-for-az.md)]

## <a name="before-you-begin"></a>Перед началом работы


* Просмотрите раздел [Планирование миграции на управляемые диски](on-prem-to-azure.md#plan-for-the-migration-to-managed-disks).

* Просмотрите раздел с [часто задаваемыми вопросами о переходе на управляемые диски](faq-for-disks.md#migrate-to-managed-disks).

[!INCLUDE [virtual-machines-common-convert-disks-considerations](../../../includes/virtual-machines-common-convert-disks-considerations.md)]

* Исходные VHD и учетная запись хранилища, используемые виртуальной машиной перед преобразованием, не удаляются. За их использование будет и дальше взиматься плата. Чтобы избежать выставления счетов за эти артефакты, удалите исходные большие двоичные объекты VHD, когда убедитесь, что преобразование завершено. Если вам нужно найти эти неподключенные диски, чтобы удалить их, см. в нашей статье [найти и удалить неподключенные управляемые и неуправляемые диски](find-unattached-disks.md).


## <a name="convert-single-instance-vms"></a>Преобразование одноэкземплярных виртуальных машин
В этом разделе описывается, как преобразовать одноэкземплярные виртуальные машины Azure с неуправляемыми дисками, чтобы они могли использовать Управляемые диски. (Если виртуальные машины находятся в группе доступности, ознакомьтесь со следующим разделом.) 

1. Отмените распределение виртуальной машины с помощью командлета [Stop-AzVM](https://docs.microsoft.com/powershell/module/az.compute/stop-azvm). В следующем примере освобождается виртуальная машина `myVM`, входящая в группу ресурсов `myResourceGroup`. 

   ```azurepowershell-interactive
   $rgName = "myResourceGroup"
   $vmName = "myVM"
   Stop-AzVM -ResourceGroupName $rgName -Name $vmName -Force
   ```

2. Преобразуйте виртуальную машину для использования управляемых дисков с помощью командлета [ConvertTo-AzVMManagedDisk](https://docs.microsoft.com/powershell/module/az.compute/convertto-azvmmanageddisk). Приведенный ниже процесс преобразовывает виртуальную машину, включая ее диск ОС и все диски данных, а затем запускает виртуальную машину.

   ```azurepowershell-interactive
   ConvertTo-AzVMManagedDisk -ResourceGroupName $rgName -VMName $vmName
   ```



## <a name="convert-vms-in-an-availability-set"></a>Преобразование виртуальных машин в группе доступности

Если виртуальные машины, которые вы хотите преобразовать для использования управляемых дисков, входят в группу доступности, то необходимо сначала преобразовать эту группу доступности в управляемую группу доступности.

1. Преобразуйте группу доступности с помощью командлета [Update-AzAvailabilitySet](https://docs.microsoft.com/powershell/module/az.compute/update-azavailabilityset). В следующем примере преобразовывается группа доступности `myAvailabilitySet` в группе ресурсов `myResourceGroup`.

   ```azurepowershell-interactive
   $rgName = 'myResourceGroup'
   $avSetName = 'myAvailabilitySet'

   $avSet = Get-AzAvailabilitySet -ResourceGroupName $rgName -Name $avSetName
   Update-AzAvailabilitySet -AvailabilitySet $avSet -Sku Aligned 
   ```

   Если регион, в котором находится группа доступности, имеет только 2 управляемых домена сбоя, но количество неуправляемых доменов сбоя равно 3, отобразится ошибка "Указанное число доменов сбоя 3 должно быть в диапазоне от 1 до 2". Чтобы устранить эту ошибку, необходимо обновить количество доменов сбоя до двух и обновить значение `Sku` к `Aligned` следующим образом:

   ```azurepowershell-interactive
   $avSet.PlatformFaultDomainCount = 2
   Update-AzAvailabilitySet -AvailabilitySet $avSet -Sku Aligned
   ```

2. Освободите и преобразуйте виртуальные машины в группе доступности. Следующий скрипт отменяет распределение каждой виртуальной машины с помощью командлета [Stop-AzVM](https://docs.microsoft.com/powershell/module/az.compute/stop-azvm), а затем преобразует ее с помощью командлета [ConvertTo-AzVMManagedDisk](https://docs.microsoft.com/powershell/module/az.compute/convertto-azvmmanageddisk) и автоматически перезапускает ее в процессе преобразования.

   ```azurepowershell-interactive
   $avSet = Get-AzAvailabilitySet -ResourceGroupName $rgName -Name $avSetName

   foreach($vmInfo in $avSet.VirtualMachinesReferences)
   {
     $vm = Get-AzVM -ResourceGroupName $rgName | Where-Object {$_.Id -eq $vmInfo.id}
     Stop-AzVM -ResourceGroupName $rgName -Name $vm.Name -Force
     ConvertTo-AzVMManagedDisk -ResourceGroupName $rgName -VMName $vm.Name
   }
   ```


## <a name="troubleshooting"></a>Устранение неполадок

Если во время преобразования произойдет ошибка или виртуальная машина находится в состоянии сбоя из-за проблем во время предыдущего преобразования, выполните командлет `ConvertTo-AzVMManagedDisk` еще раз. Простой повтор обычно решает проблему.
Перед преобразованием убедитесь, что все расширения виртуальной машины находятся в состоянии "Подготовка выполнена успешно", или преобразование завершится ошибкой с кодом 409.

## <a name="convert-using-the-azure-portal"></a>Преобразование с помощью портала Azure

Неуправляемые диски также можно преобразовать в управляемые с помощью портала Azure.

1. Войдите на [портале Azure](https://portal.azure.com).
2. Выберите виртуальную машину из списка виртуальных машин на портале.
3. В колонке виртуальной машины в меню выберите пункт **Диски**.
4. В верхней части колонки **Диски** выберите **Перемещение на управляемые диски**.
5. Если виртуальная машина входит в группу доступности, в колонке **Перемещение на управляемые диски** появится предупреждение о том, что сначала необходимо преобразовать группу доступности. В предупреждении должна быть указана ссылка, которую можно щелкнуть, чтобы преобразовать группу доступности. Когда группа доступности будет преобразована (или если виртуальная машина не входит в группу доступности), щелкните **Миграция**, чтобы начать перенос дисков в управляемые диски.

Виртуальная машина будет остановлена и перезапущена после завершения миграции.

## <a name="next-steps"></a>Дальнейшие действия

[Преобразование управляемых дисков уровня "Стандартный" в диски уровня "Премиум"](convert-disk-storage.md)

Создайте копию виртуальной машины, доступную только для чтения, с помощью [моментальных снимков](snapshot-copy-managed-disk.md).

