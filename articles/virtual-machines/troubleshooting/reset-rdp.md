---
title: Сброс служб удаленных рабочих столов или пароля администратора на виртуальной машине Windows | Документация Майкрософт
description: Узнайте, как сбросить пароль учетной записи или конфигурацию служб удаленных рабочих столов для виртуальной машины Windows с помощью портала Azure или Azure PowerShell.
services: virtual-machines-windows
documentationcenter: ''
author: genlin
manager: gwallace
editor: ''
tags: azure-resource-manager
ms.assetid: 45c69812-d3e4-48de-a98d-39a0f5675777
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.topic: troubleshooting
ms.date: 03/25/2019
ms.author: genli
ms.openlocfilehash: caa0066f8020c19cecccaa0421f90188260c644b
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/09/2019
ms.locfileid: "67710628"
---
# <a name="reset-remote-desktop-services-or-its-administrator-password-in-a-windows-vm"></a>Сброс служб удаленных рабочих столов или пароля администратора на виртуальной машине Windows
Если не удается подключиться к виртуальной машине Windows, можно сбросить пароль локального администратора или конфигурацию служб удаленных рабочих столов (не поддерживается для контроллеров домена Windows). Для сброса пароля используйте портал Azure или расширение VMAccess в Azure PowerShell. Войдите на виртуальную машину и сбросьте пароль для этого локального администратора.  
Если вы используете PowerShell, то убедитесь, что у вас [установлен и настроен последний модуль PowerShell](/powershell/azure/overview) и вы вошли в подписку Azure. Вы также можете [выполнить эти действия для виртуальных машин, созданных с использованием классической модели](https://docs.microsoft.com/azure/virtual-machines/windows/classic/reset-rdp).

Службы удаленных рабочих столов и учетные данные можно сбросить следующим образом.

- [Сброс с помощью портала Azure](#reset-by-using-the-azure-portal)

- [Сброс с помощью расширения VMAccess и PowerShell](#reset-by-using-the-vmaccess-extension-and-powershell)

## <a name="reset-by-using-the-azure-portal"></a>Сброс с помощью портала Azure

Сначала войдите на [портал Azure](https://portal.azure.com) и выберите **Виртуальные машины** в меню слева. 

### <a name="reset-the-local-administrator-account-password"></a>**Сброс пароля учетной записи локального администратора**

1. Выберите нужную виртуальную машину Windows, а затем **Сброс пароля** в разделе **Поддержка и устранение неполадок**. Откроется диалоговое окно **Сброс пароля**.

2. Выберите **Сброс пароля**, введите имя пользователя и пароль, а затем выберите **Обновить**. 

3. Снова попробуйте подключиться к виртуальной машине.

### <a name="reset-the-remote-desktop-services-configuration"></a>**Сброс конфигурации служб удаленных рабочих столов**

Этот процесс будет включить службы удаленных рабочих столов на виртуальной машине и создать правило брандмауэра для порта RDP по умолчанию 3389.

1. Выберите нужную виртуальную машину Windows, а затем **Сброс пароля** в разделе **Поддержка и устранение неполадок**. Откроется диалоговое окно **Сброс пароля**. 

2. Выберите **Сбросить только конфигурацию**, а затем **Обновить**. 

3. Снова попробуйте подключиться к виртуальной машине.

## <a name="reset-by-using-the-vmaccess-extension-and-powershell"></a>Сброс с помощью расширения VMAccess и PowerShell

Сначала убедитесь, что у вас [установлен и настроен последний модуль PowerShell](/powershell/azure/overview) и вы вошли в подписку Azure, выполнив командлет [Connect-AzAccount](https://docs.microsoft.com/powershell/module/az.accounts/connect-azaccount).

### <a name="reset-the-local-administrator-account-password"></a>**Сброс пароля учетной записи локального администратора**

- Сбросьте пароль или имя пользователя администратора, выполнив командлет PowerShell [Set-AzVMAccessExtension](https://docs.microsoft.com/powershell/module/az.compute/set-azvmaccessextension). Параметр `typeHandlerVersion` должен быть версии 2.0 или выше, так как версия 1 признана нерекомендуемой. 

    ```powershell
    $SubID = "<SUBSCRIPTION ID>" 
    $RgName = "<RESOURCE GROUP NAME>" 
    $VmName = "<VM NAME>" 
    $Location = "<LOCATION>" 
 
    Connect-AzAccount 
    Select-AzSubscription -SubscriptionId $SubID 
    Set-AzVMAccessExtension -ResourceGroupName $RgName -Location $Location -VMName $VmName -Credential (get-credential) -typeHandlerVersion "2.0" -Name VMAccessAgent 
    ```

    > [!NOTE] 
    > Если вы укажете не текущую учетную запись локального администратора виртуальной машины, а другое имя, то расширение VMAccess добавит учетную запись локального администратора с этим именем и назначит ей указанный пароль. Если учетная запись локального администратора на виртуальной машине существует, расширение VMAccess сбросит пароль. Если учетная запись отключена, расширение VMAccess включит ее.

### <a name="reset-the-remote-desktop-services-configuration"></a>**Сброс конфигурации служб удаленных рабочих столов**

1. Сбросьте параметры удаленного доступа к виртуальной машине с помощью командлета PowerShell [Set-AzVMAccessExtension](https://docs.microsoft.com/powershell/module/az.compute/set-azvmaccessextension). Следующий пример сбрасывает параметры расширения доступа `myVMAccess` на виртуальной машине `myVM` в группе ресурсов `myResourceGroup`.

    ```powershell
    Set-AzVMAccessExtension -ResourceGroupName "myResoureGroup" -VMName "myVM" -Name "myVMAccess" -Location WestUS -typeHandlerVersion "2.0" -ForceRerun
    ```

    > [!TIP]
    > В любой момент на виртуальной машине может быть только один агент доступа. Чтобы задать свойства агента доступа к виртуальной машине, используйте параметр `-ForceRerun`. При использовании `-ForceRerun` используйте то же имя для агента доступа к виртуальной машине, которое использовалось в предыдущей команде.

1. Если по-прежнему не удается удаленно подключиться к виртуальной машине, см. статью [Устранение неполадок с подключением к удаленному рабочему столу на виртуальной машине Azure под управлением Windows](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). При утрате подключения к контроллеру домена Windows необходимо будет восстановить его с помощью резервной копии контроллера домена.

## <a name="next-steps"></a>Следующие шаги

- Если расширение доступа к виртуальной машине Azure не отвечает и сбросить пароль не удается, вы можете [сбросить локальный пароль Windows в автономном режиме](reset-local-password-without-agent.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Этот метод является более сложным и требует подключения виртуального жесткого диска проблемной виртуальной машины к другой виртуальной машине. Сначала выполните действия, описанные в этой статье, и попытайтесь сбросить пароль в автономном режиме, только если они не помогут.

- [Сведения о расширениях и компонентах виртуальных машин Azure](../extensions/features-windows.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

- [Подключение к виртуальной машине Azure по протоколу RDP или SSH](https://msdn.microsoft.com/library/azure/dn535788.aspx)

- [Устранение неполадок с подключением к виртуальной машине Azure для Windows через удаленный рабочий стол](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

