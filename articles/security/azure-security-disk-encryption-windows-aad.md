---
title: Шифрование дисков Azure с использованием виртуальных машин IaaS под управлением Windows для Azure AD App (предыдущий выпуск)
description: В этой статье приведены инструкции по включению шифрования дисков Microsoft Azure для виртуальных машин IaaS под управлением Windows.
author: msmbaldwin
ms.service: security
ms.topic: article
ms.author: mbaldwin
ms.date: 03/15/2019
ms.custom: seodec18
ms.openlocfilehash: fa9b970ee9319af061ceab99844b0497253881ad
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "66117932"
---
# <a name="enable-azure-disk-encryption-for-windows-iaas-vms-previous-release"></a>Включение шифрования дисков Azure для виртуальных машин IaaS под управлением Windows (предыдущий выпуск)

**В новом выпуске шифрования дисков Azure устранена необходимость предоставлять параметр приложения Azure AD для включения шифрования дисков виртуальной машины. В новом выпуске больше не требуется вводить учетные данные Azure AD на этапе включения шифрования. При использовании нового выпуска все новые виртуальные машины должны быть зашифрованы без параметров приложения Azure AD. Инструкции по включению шифрования дисков виртуальных машин при использовании нового выпуска см. в статье [Включение шифрования дисков Azure для виртуальных машин IaaS под управлением Windows](azure-security-disk-encryption-windows.md). Виртуальные машины, которые уже были зашифрованы с помощью параметров приложения Azure AD, по-прежнему поддерживаются. Их следует и дальше обслуживать с использованием синтаксиса AAD.**


Возможно несколько сценариев включения шифрования дисков, и последовательность действий для каждого может отличаться. В следующих разделах сценарии для виртуальных машин IaaS под управлением Windows описаны более подробно. Чтобы использовать шифрование дисков Azure, нужно выполнить [эти предварительные требования](../security/azure-security-disk-encryption-prerequisites-aad.md). 

Создайте [моментальный снимок](../virtual-machines/windows/snapshot-copy-managed-disk.md) и (или) резервную копию перед шифрованием дисков. Резервные копии обеспечивают возможность восстановления в случае любого непредвиденного сбоя во время шифрования. Для виртуальных машин с управляемыми дисками необходимо создать резервную копию до начала шифрования. Как только будет установлено резервной копии, можно использовать командлет Set-AzVMDiskEncryptionExtension зашифровать управляемые диски, указав параметр - skipVmBackup. Дополнительные сведения см. в статье [Резервное копирование и восстановление зашифрованных виртуальных машин с помощью службы Azure Backup](../backup/backup-azure-vms-encryption.md). 

>[!WARNING]
> - Если вы уже использовали [шифрование дисков Azure с помощью приложения Azure AD](azure-security-disk-encryption-prerequisites-aad.md) для шифрования этой виртуальной машины, этот способ нужно применять и далее для шифрования виртуальной машины. На этой зашифрованной виртуальной машине нельзя использовать [шифрование дисков Azure](azure-security-disk-encryption-prerequisites.md), так как этот сценарий не работает — переключение из приложения AAD для данной зашифрованной виртуальной машины сейчас не поддерживается.
> - Чтобы убедиться, что секреты шифрования не пересекают региональные границы, шифрованию дисков Azure требуется, чтобы хранилище ключей и виртуальные машины были расположены в одном регионе. Создайте и используйте хранилище ключей, которое находится в том же регионе, что и виртуальная машина, которую нужно зашифровать. 

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="enable-encryption-on-new-iaas-vms-created-from-the-marketplace"></a>Включение шифрования в новых виртуальных машинах IaaS, созданных с помощью Marketplace
Включить шифрование дисков на новой виртуальной машине IaaS под управлением Windows, созданной из образа Marketplace в Azure, можно с помощью шаблона Resource Manager. Этот шаблон создает зашифрованную виртуальную машину Windows на основе образа из коллекции Windows Server 2012.

1. На странице шаблона [Azure Resource Manager](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-create-new-vm-gallery-image) нажмите кнопку **Deploy to Azure** (Развернуть в Azure).

2. Выберите подписку, группу ресурсов, расположение группы ресурсов, параметры, условия использования и соглашение. Нажмите кнопку **Покупка**, чтобы развернуть новую виртуальную машину IaaS с включенным шифрованием.

3. После развертывания шаблона проверьте состояние шифрования виртуальной машины предпочитаемым способом.
     - Проверьте состояние с помощью Azure CLI, используя команду [az vm encryption show](/cli/azure/vm/encryption#az-vm-encryption-show). 

         ```azurecli-interactive 
         az vm encryption show --name "MySecureVM" --resource-group "MyVirtualMachineResourceGroup"
         ```

     - Проверка с помощью Azure PowerShell с помощью [Get AzVmDiskEncryptionStatus](/powershell/module/az.compute/get-azvmdiskencryptionstatus) командлета. 

         ```azurepowershell-interactive
         Get-AzVmDiskEncryptionStatus -ResourceGroupName 'MyVirtualMachineResourceGroup' -VMName 'MySecureVM'
         ```

     -  Выберите виртуальную машину, а затем щелкните **Диски** под заголовком **Параметры**, чтобы проверить состояние шифрования на портале. На диаграмме в разделе **Шифрование** вы увидите, включено ли оно. 
           ![Портал Azure: шифрование дисков включено](./media/azure-security-disk-encryption/disk-encryption-fig2.png) В следующей таблице перечислены параметры шаблона Resource Manager для новых виртуальных машин, создаваемых с помощью сценария Marketplace, которые используют идентификатор клиента Azure AD.

| Параметр | Описание | 
| --- | --- |
| adminUserName | Имя пользователя администратора виртуальной машины. |
| adminPassword | Пароль администратора виртуальной машины. |
| newStorageAccountName | Имя учетной записи хранения, в которой будут размещены виртуальные жесткие диски операционной системы и данных. |
| vmSize | Размер виртуальной машины. Сейчас поддерживаются только серии A, D и G уровня "Стандартный". |
| virtualNetworkName | Имя виртуальной сети, которой будет принадлежать сетевая карта виртуальной машины. |
| subnetName | Имя подсети виртуальной сети, которой будет принадлежать сетевая карта виртуальной машины. |
| AADClientID | Идентификатор клиента приложения Azure AD, которое имеет разрешения на запись секретов в ваше хранилище ключей. |
| AADClientSecret | Секрет клиента приложения Azure AD, которое имеет разрешения на запись секретов в ваше хранилище ключей. |
| keyVaultURL | URL-адрес хранилища ключей, в которое будет передан ключ BitLocker. Его можно получить с помощью командлета `(Get-AzKeyVault -VaultName "MyKeyVault" -ResourceGroupName "MyKeyVaultResourceGroupName").VaultURI` или команды Azure CLI `az keyvault show --name "MySecureVault" --query properties.vaultUri`. |
| keyEncryptionKeyURL | URL-адрес ключа шифрования ключей, который используется для шифрования созданного ключа BitLocker (необязательно). </br> </br>KeyEncryptionKeyURL является необязательным параметром. Вы можете добавить собственный ключ шифрования ключей, чтобы дополнительно защитить ключ шифрования данных (секрет в виде парольной фразы) в своем хранилище ключей. |
| keyVaultResourceGroup | Группа ресурсов хранилища ключей. |
| vmName | Имя виртуальной машины, для которой будет выполняться шифрование. |


## <a name="bkmk_RunningWinVM"></a> Включение шифрования на виртуальной машине IaaS Windows, которая уже существует или работает
В этом сценарии шифрование можно включить с помощью шаблона, командлетов PowerShell или команд интерфейса командной строки. В следующих разделах более подробно описано, как включить шифрование дисков Azure. 

>[!IMPORTANT]
 >Нужно обязательно создать образ или резервную копию экземпляра виртуальной машины на основе управляемого диска вне шифрования дисков Azure и перед включением этой функции. Моментальный снимок управляемого диска можно создать с портала или использовать для этого [Azure Backup](../backup/backup-azure-vms-encryption.md). Резервные копии обеспечивают возможность восстановления при любом непредвиденном сбое во время шифрования. Как только будет установлено резервной копии, можно использовать командлет Set-AzVMDiskEncryptionExtension зашифровать управляемые диски, указав параметр - skipVmBackup. Команда Set-AzVMDiskEncryptionExtension завершится ошибкой для виртуальных машин на основе управляемых дисков, пока не сделана резервная копия, и этот параметр был указан. 
>
>Шифрование или отключение шифрования может привести к перезагрузке виртуальной машины. 
>

### <a name="bkmk_RunningWinVMPSH"></a> Включение шифрования на существующих или работающих виртуальных машинах с помощью Azure PowerShell 
Используйте [AzVMDiskEncryptionExtension набора](/powershell/module/az.compute/set-azvmdiskencryptionextension) командлет, чтобы включить шифрование на работающей виртуальной машины IaaS в Azure. Сведения о включении шифрования дисков Azure с помощью командлетов PowerShell см. в статьях [Explore Azure Disk Encryption with Azure Powershell](https://blogs.msdn.com/b/azuresecurity/archive/2015/11/17/explore-azure-disk-encryption-with-azure-powershell.aspx) (Изучение возможностей шифрования дисков Azure с помощью PowerShell) и [Explore Azure Disk Encryption with Azure PowerShell – Part 2](https://blogs.msdn.com/b/azuresecurity/archive/2015/11/21/explore-azure-disk-encryption-with-azure-powershell-part-2.aspx) (Изучение возможностей шифрования дисков Azure с помощью PowerShell. Часть 2).

-  **Шифрование работающей виртуальной машины с использованием секрета клиента:** Приведенный ниже сценарий инициализирует переменные и запускает командлет Set-AzVMDiskEncryptionExtension. Группа ресурсов, виртуальная машина, хранилище ключей, приложение AAD и секрет клиента должны быть уже созданы в качестве необходимых компонентов. Замените значения MyKeyVaultResourceGroup, MyVirtualMachineResourceGroup, MySecureVM, MySecureVault, My-AAD-client-ID и My-AAD-client-secret.
     ```azurepowershell
      $KVRGname = 'MyKeyVaultResourceGroup';
      $VMRGName = 'MyVirtualMachineResourceGroup';
      $vmName = 'MySecureVM';
      $aadClientID = 'My-AAD-client-ID';
      $aadClientSecret = 'My-AAD-client-secret';
      $KeyVaultName = 'MySecureVault';
      $KeyVault = Get-AzKeyVault -VaultName $KeyVaultName -ResourceGroupName $KVRGname;
      $diskEncryptionKeyVaultUrl = $KeyVault.VaultUri;
      $KeyVaultResourceId = $KeyVault.ResourceId;

      Set-AzVMDiskEncryptionExtension -ResourceGroupName $VMRGName -VMName $vmName -AadClientID $aadClientID -AadClientSecret $aadClientSecret -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $KeyVaultResourceId;
    ```
- **Шифрование работающей виртуальной машины с применением ключа шифрования ключей для упаковки секрета клиента:** Шифрование дисков Azure позволяет указать существующий ключ из хранилища ключей для упаковки секретов шифрования диска, которые были сгенерированы при включении шифрования. Когда ключ шифрования ключей указан, шифрование дисков Azure использует его для упаковки секретов шифрования перед записью в Key Vault. 

     ```azurepowershell
     $KVRGname = 'MyKeyVaultResourceGroup';
     $VMRGName = 'MyVirtualMachineResourceGroup';
     $vmName = ‘MyExtraSecureVM’;
     $aadClientID = 'My-AAD-client-ID';
     $aadClientSecret = 'My-AAD-client-secret';
     $KeyVaultName = 'MySecureVault';
     $keyEncryptionKeyName = 'MyKeyEncryptionKey';
     $KeyVault = Get-AzKeyVault -VaultName $KeyVaultName -ResourceGroupName $KVRGname;
     $diskEncryptionKeyVaultUrl = $KeyVault.VaultUri;
     $KeyVaultResourceId = $KeyVault.ResourceId;
     $keyEncryptionKeyUrl = (Get-AzKeyVaultKey -VaultName $KeyVaultName -Name $keyEncryptionKeyName).Key.kid;

     Set-AzVMDiskEncryptionExtension -ResourceGroupName $VMRGname -VMName $vmName -AadClientID $aadClientID -AadClientSecret $aadClientSecret -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $KeyVaultResourceId -KeyEncryptionKeyUrl $keyEncryptionKeyUrl -KeyEncryptionKeyVaultId $KeyVaultResourceId;

     ```
     
   >[!NOTE]
   > Синтаксис значения параметра disk-encryption-keyvault — это полная строка идентификатора: /subscriptions/[ИД или GUID подписки]/resourceGroups/[имя группы ресурсов]/providers/Microsoft.KeyVault/vaults/[имя хранилища ключей].</br> Синтаксис значения параметра key-encryption-key —это полный универсальный код ресурса (URI) для ключа шифрования ключей: https://[имя хранилища ключей].vault.azure.net/keys/[имя KEK]/[ИД KEK]. 

- **Проверка того, что диски зашифрованы:** Чтобы проверить состояние шифрования виртуальной машины IaaS, используйте [Get AzVmDiskEncryptionStatus](/powershell/module/az.compute/get-azvmdiskencryptionstatus) командлета. 
     ```azurepowershell-interactive
     Get-AzVmDiskEncryptionStatus -ResourceGroupName 'MyVirtualMachineResourceGroup' -VMName 'MySecureVM'
     ```
    
- **Отключение шифрования дисков:** Для отключения шифрования используйте командлет [Disable-AzureRmVMDiskEncryption](/powershell/module/az.compute/disable-azvmdiskencryption). 
     ```azurepowershell-interactive
     Disable-AzVMDiskEncryption -ResourceGroupName 'MyVirtualMachineResourceGroup' -VMName 'MySecureVM'
     ```

### <a name="bkmk_RunningWinVMCLI"></a>Включение шифрования на существующих или работающих виртуальных машинах с помощью Azure CLI
Используйте команду [az vm encryption enable](/cli/azure/vm/encryption#az-vm-encryption-enable), чтобы включить шифрование на работающей виртуальной машине IaaS в Azure.

- **Шифрование работающей виртуальной машины с использованием секрета клиента:**

    ```azurecli-interactive
    az vm encryption enable --resource-group "MyVirtualMachineResourceGroup" --name "MySecureVM" --aad-client-id "<my spn created with CLI/my Azure AD ClientID>"  --aad-client-secret "My-AAD-client-secret" --disk-encryption-keyvault "MySecureVault" --volume-type [All|OS|Data]
    ```

- **Шифрование работающей виртуальной машины с применением ключа шифрования ключей для упаковки секрета клиента:**

     ```azurecli-interactive
     az vm encryption enable --resource-group "MyVirtualMachineResourceGroup" --name "MySecureVM" --aad-client-id "<my spn created with CLI which is the Azure AD ClientID>"  --aad-client-secret "My-AAD-client-secret" --disk-encryption-keyvault  "MySecureVault" --key-encryption-key "MyKEK_URI" --key-encryption-keyvault "MySecureVaultContainingTheKEK" --volume-type [All|OS|Data]
     ```

     >[!NOTE]
     > Синтаксис значения параметра disk-encryption-keyvault — это полная строка идентификатора: /subscriptions/[ИД или GUID подписки]/resourceGroups/[имя группы ресурсов]/providers/Microsoft.KeyVault/vaults/[имя хранилища ключей]. </br> Синтаксис значения параметра key-encryption-key —это полный универсальный код ресурса (URI) для ключа шифрования ключей: https://[имя хранилища ключей].vault.azure.net/keys/[имя KEK]/[ИД KEK]. 

- **Проверка того, что диски зашифрованы:** Чтобы проверить состояние шифрования виртуальной машины IaaS, используйте [show шифрования виртуальной машины az](/cli/azure/vm/encryption#az-vm-encryption-show) команды. 

     ```azurecli-interactive
     az vm encryption show --name "MySecureVM" --resource-group "MyVirtualMachineResourceGroup"
     ```

- **Отключение шифрования:** Чтобы отключить шифрование, используйте команду [az vm encryption disable](/cli/azure/vm/encryption#az-vm-encryption-disable). 
     ```azurecli-interactive
     az vm encryption disable --name "MySecureVM" --resource-group "MyVirtualMachineResourceGroup" --volume-type [ALL, DATA, OS]
     ```
 
  > [!NOTE]
  >Нужно обязательно создать образ или резервную копию экземпляра виртуальной машины на основе управляемого диска вне шифрования дисков Azure и перед включением этой функции. Моментальный снимок управляемого диска можно создать с портала или использовать для этого [Azure Backup](../backup/backup-azure-vms-encryption.md). Резервные копии обеспечивают возможность восстановления при любом непредвиденном сбое во время шифрования. Как только будет установлено резервной копии, можно использовать командлет Set-AzVMDiskEncryptionExtension зашифровать управляемые диски, указав параметр - skipVmBackup. Эта команда будет завершаться ошибкой при использовании с виртуальными машинами на основе управляемых дисков, пока не будет сделана резервная копия и указан определенный параметр. 
  >
  >Шифрование или отключение шифрования может привести к перезагрузке виртуальной машины. 

### <a name="bkmk_RunningWinVMwRM"> </a>Использование шаблона Resource Manager
Включить шифрование дисков на существующих или работающих виртуальных машинах IaaS под управлением Windows в Azure можно с помощью [этого шаблона Resource Manager](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-running-windows-vm).


1. На странице шаблона быстрого запуска Azure нажмите кнопку **Deploy to Azure** (Развернуть в Azure).

2. Выберите подписку, группу ресурсов, расположение группы ресурсов, параметры, условия использования и соглашение. Нажмите кнопку **Покупка**, чтобы включить шифрование на существующей или работающей виртуальной машине IaaS.

В следующей таблице перечислены параметры шаблона Resource Manager для существующих или работающих виртуальных машин, которые используют идентификатор клиента Azure AD.

| Параметр | Описание |
| --- | --- |
| AADClientID | Идентификатор клиента приложения Azure AD, которое имеет разрешения на запись секретов в хранилище ключей. |
| AADClientSecret | Секрет клиента приложения Azure AD, которое имеет разрешения на запись секретов в хранилище ключей. |
| keyVaultName | Имя хранилища ключей, в которое будет передан ключ BitLocker. Его можно получить с помощью командлета `(Get-AzKeyVault -ResourceGroupName <MyKeyVaultResourceGroupName>). Vaultname` или команды Azure CLI `az keyvault list --resource-group "MySecureGroup"`.|
|  keyEncryptionKeyURL | URL-адрес ключа шифрования ключей, который используется для шифрования созданного ключа BitLocker. Это необязательный параметр, если выбрать **nokek** из раскрывающегося списка UseExistingKek. Если из раскрывающегося списка UseExistingKek выбрано значение **kek**, то потребуется ввести значение _keyEncryptionKeyURL_. |
| volumeType | Тип тома, для которого будет выполняться шифрование. Допустимые значения: _OS_, _Data_ и _All_. |
| sequenceVersion | Версия последовательности операций с BitLocker. Этот номер версии увеличивается каждый раз, когда шифрование диска выполняется на той же виртуальной машине. |
| vmName | Имя виртуальной машины, для которой будет выполняться шифрование. |


## <a name="bkmk_VHDpre"> </a> Новые виртуальные машины IaaS, при создании которых используются зашифрованные виртуальные жесткие диски и ключи шифрования
В этом сценарии шифрование можно включить с помощью шаблона Resource Manager, командлетов PowerShell или команд интерфейса командной строки. В следующих разделах более подробно описаны шаблон Resource Manager и команды интерфейса командной строки. 

Следуйте инструкциям, приведенным в приложении к этой статье, чтобы подготовить предварительно зашифрованные образы, которые можно использовать в Azure. После создания образа выполните действия, описанные в следующем разделе, чтобы создать зашифрованную виртуальную машину Azure.

* [Подготовка предварительно зашифрованного виртуального жесткого диска Windows](azure-security-disk-encryption-appendix.md#bkmk_preWin)
* [Подготовка предварительно зашифрованного виртуального жесткого диска Linux](azure-security-disk-encryption-appendix.md#bkmk_preLinux)

>[!IMPORTANT]
 >Нужно обязательно создать образ или резервную копию экземпляра виртуальной машины на основе управляемого диска вне шифрования дисков Azure и перед включением этой функции. Моментальный снимок управляемого диска можно создать с портала или использовать для этого [Azure Backup](../backup/backup-azure-vms-encryption.md). Резервные копии обеспечивают возможность восстановления при любом непредвиденном сбое во время шифрования. Как только будет установлено резервной копии, можно использовать командлет Set-AzVMDiskEncryptionExtension зашифровать управляемые диски, указав параметр - skipVmBackup. Команда Set-AzVMDiskEncryptionExtension завершится ошибкой для виртуальных машин на основе управляемых дисков, пока не сделана резервная копия, и этот параметр был указан. 
>
>Шифрование или отключение шифрования может привести к перезагрузке виртуальной машины. 



### <a name="bkmk_VHDprePSH"> </a> Шифрование виртуальных машин с предварительно зашифрованными виртуальными жесткими дисками с помощью Azure PowerShell
Можно включить шифрование дисков для зашифрованного виртуального жесткого диска с помощью командлета PowerShell [AzVMOSDisk набора](/powershell/module/az.compute/set-azvmosdisk#examples). Приведенный ниже пример имеет некоторые общие параметры. 

```powershell
$VirtualMachine = New-AzVMConfig -VMName "MySecureVM" -VMSize "Standard_A1"
$VirtualMachine = Set-AzVMOSDisk -VM $VirtualMachine -Name "SecureOSDisk" -VhdUri "os.vhd" Caching ReadWrite -Windows -CreateOption "Attach" -DiskEncryptionKeyUrl "https://mytestvault.vault.azure.net/secrets/Test1/514ceb769c984379a7e0230bddaaaaaa" -DiskEncryptionKeyVaultId "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myKVresourcegroup/providers/Microsoft.KeyVault/vaults/mytestvault"
New-AzVM -VM $VirtualMachine -ResourceGroupName "MyVirtualMachineResourceGroup"
```

## <a name="enable-encryption-on-a-newly-added-data-disk"></a>Включение шифрования на добавленном диске данных
[Добавить новый диск на виртуальную машину Windows можно с помощью PowerShell](../virtual-machines/windows/attach-disk-ps.md) или [портала Azure](../virtual-machines/windows/attach-managed-disk-portal.md). 

### <a name="enable-encryption-on-a-newly-added-disk-with-azure-powershell"></a>Включение шифрования на добавленном диске данных с помощью Azure PowerShell
 При использовании Powershell для шифрования нового диска для виртуальных машин Windows необходимо указать новую версию последовательности. Версия последовательности должна быть уникальной. Приведенный ниже сценарий создает GUID для версии последовательности. В некоторых случаях новый добавленный диск данных может быть зашифрован автоматически с помощью расширения шифрования дисков Azure. Автоматическое шифрование обычно происходит, когда виртуальная машина перезапускается после включения нового диска. Обычно это происходит, так как для типа тома указано значение "Все", если ранее на виртуальной машине использовалось шифрование дисков. В случае автоматического шифрования на диске только что добавленные данные, мы рекомендуем снова выполнить командлет Set-AzVmDiskEncryptionExtension с новой версии последовательности. Если для нового диска данных включено автоматическое шифрование и вы хотите отказаться от шифрования, сначала расшифруйте все диски, а затем повторите шифрование с новой версией последовательности, указав соответствующую операционную систему для типа тома. 
 

-  **Шифрование работающей виртуальной машины с использованием секрета клиента:** Приведенный ниже сценарий инициализирует переменные и запускает командлет Set-AzVMDiskEncryptionExtension. Группа ресурсов, виртуальная машина, хранилище ключей, приложение AAD и секрет клиента должны быть уже созданы в качестве необходимых компонентов. Замените значения MyKeyVaultResourceGroup, MyVirtualMachineResourceGroup, MySecureVM, MySecureVault, My-AAD-client-ID и My-AAD-client-secret. В этом примере в качестве значения параметра VolumeType используется значение "All", что включает тома операционной системы и тома данных. Если требуется шифрование только томов операционной системы, используйте "OS" в качестве значения параметра VolumeType. 

     ```azurepowershell
      $sequenceVersion = [Guid]::NewGuid();
      $KVRGname = 'MyKeyVaultResourceGroup';
      $VMRGName = 'MyVirtualMachineResourceGroup';
      $vmName = 'MySecureVM';
      $aadClientID = 'My-AAD-client-ID';
      $aadClientSecret = 'My-AAD-client-secret';
      $KeyVaultName = 'MySecureVault';
      $KeyVault = Get-AzKeyVault -VaultName $KeyVaultName -ResourceGroupName $KVRGname;
      $diskEncryptionKeyVaultUrl = $KeyVault.VaultUri;
      $KeyVaultResourceId = $KeyVault.ResourceId;

      Set-AzVMDiskEncryptionExtension -ResourceGroupName $VMRGname -VMName $vmName -AadClientID $aadClientID -AadClientSecret $aadClientSecret -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $KeyVaultResourceId -VolumeType 'all' –SequenceVersion $sequenceVersion;
    ```
- **Шифрование работающей виртуальной машины с применением ключа шифрования ключей для упаковки секрета клиента:** Шифрование дисков Azure позволяет указать существующий ключ из хранилища ключей для упаковки секретов шифрования диска, которые были сгенерированы при включении шифрования. Когда ключ шифрования ключей указан, шифрование дисков Azure использует его для упаковки секретов шифрования перед записью в Key Vault. В этом примере в качестве значения параметра VolumeType используется значение "All", что включает тома операционной системы и тома данных. Если требуется шифрование только томов операционной системы, используйте "OS" в качестве значения параметра VolumeType. 

     ```azurepowershell
     $sequenceVersion = [Guid]::NewGuid();
     $KVRGname = 'MyKeyVaultResourceGroup';
     $VMRGName = 'MyVirtualMachineResourceGroup';
     $vmName = 'MyExtraSecureVM';
     $aadClientID = 'My-AAD-client-ID';
     $aadClientSecret = 'My-AAD-client-secret';
     $KeyVaultName = 'MySecureVault';
     $keyEncryptionKeyName = 'MyKeyEncryptionKey';
     $KeyVault = Get-AzKeyVault -VaultName $KeyVaultName -ResourceGroupName $KVRGname;
     $diskEncryptionKeyVaultUrl = $KeyVault.VaultUri;
     $KeyVaultResourceId = $KeyVault.ResourceId;
     $keyEncryptionKeyUrl = (Get-AzKeyVaultKey -VaultName $KeyVaultName -Name $keyEncryptionKeyName).Key.kid;

     Set-AzVMDiskEncryptionExtension -ResourceGroupName $VMRGname -VMName $vmName -AadClientID $aadClientID -AadClientSecret $aadClientSecret -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $KeyVaultResourceId -KeyEncryptionKeyUrl $keyEncryptionKeyUrl -KeyEncryptionKeyVaultId $KeyVaultResourceId -VolumeType 'all' –SequenceVersion $sequenceVersion;

     ```

    >[!NOTE]
    > Синтаксис значения параметра disk-encryption-keyvault — это полная строка идентификатора: /subscriptions/[ИД или GUID подписки]/resourceGroups/[имя группы ресурсов]/providers/Microsoft.KeyVault/vaults/[имя хранилища ключей].</br> Синтаксис значения параметра key-encryption-key —это полный универсальный код ресурса (URI) для ключа шифрования ключей: https://[имя хранилища ключей].vault.azure.net/keys/[имя KEK]/[ИД KEK]. 

### <a name="enable-encryption-on-a-newly-added-disk-with-azure-cli"></a>Включение шифрования на добавленном диске данных с помощью интерфейса командной строки Azure
  Команда Azure CLI автоматически предоставит вам новую версию последовательности, когда вы запустите команду для включения шифрования. Допустимые значения параметра volume-type: "All", "OS" и "Data" ("Все", "ОС" и "Данные"). Может потребоваться изменить значения параметра volume-type на"OS" или "Data", если необходимо выполнить шифрование только одного типа диска виртуальной машины. В примерах используется "All" для параметра volume-type. 

-  **Шифрование работающей виртуальной машины с использованием секрета клиента:**

     ```azurecli-interactive
     az vm encryption enable --resource-group "MyVirtualMachineResourceGroup" --name "MySecureVM" --aad-client-id "<my spn created with CLI/my Azure AD ClientID>"  --aad-client-secret "My-AAD-client-secret" --disk-encryption-keyvault "MySecureVault" --volume-type "All"
     ```

- **Шифрование работающей виртуальной машины с применением ключа шифрования ключей для упаковки секрета клиента:**

     ```azurecli-interactive
     az vm encryption enable --resource-group "MyVirtualMachineResourceGroup" --name "MySecureVM" --aad-client-id "<my spn created with CLI which is the Azure AD ClientID>"  --aad-client-secret "My-AAD-client-secret" --disk-encryption-keyvault  "MySecureVault" --key-encryption-key "MyKEK_URI" --key-encryption-keyvault "MySecureVaultContainingTheKEK" --volume-type "all"
     ```


## <a name="enable-encryption-using-azure-ad-client-certificate-based-authentication"></a>Включение шифрования с использованием проверки подлинности на основе сертификата клиента Azure AD
Аутентификацию на основе сертификата клиента можно использовать с ключом шифрования ключей или без него. Для запуска сценариев требуется выполнить [предварительные требования для шифрования дисков Azure](azure-security-disk-encryption-prerequisites-aad.md). Прежде чем использовать сценарии PowerShell, необходимо отправить сертификат в хранилище ключей и развернуть его на виртуальной машине. Если вы планируете использовать проверку подлинности с ключом шифрования ключей, он также уже должен существовать. Дополнительные сведения см. в разделе [Аутентификация на основе сертификата (необязательно)](azure-security-disk-encryption-prerequisites-aad.md#bkmk_Cert) статьи с предварительными требованиями.


### <a name="enable-encryption-using-certificate-based-authentication-with-azure-powershell"></a>Включение шифрования с использованием аутентификации на основе сертификата с помощью Azure PowerShell

```powershell
## Fill in 'MyVirtualMachineResourceGroup', 'MyKeyVaultResourceGroup', 'My-AAD-client-ID', 'MySecureVault, and ‘MySecureVM’.

$VMRGName = 'MyVirtualMachineResourceGroup'
$KVRGname = 'MyKeyVaultResourceGroup';
$AADClientID ='My-AAD-client-ID';
$KeyVaultName = 'MySecureVault';
$VMName = 'MySecureVM';
$KeyVault = Get-AzKeyVault -VaultName $KeyVaultName -ResourceGroupName $KVRGname;
$diskEncryptionKeyVaultUrl = $KeyVault.VaultUri;
$KeyVaultResourceId = $KeyVault.ResourceId;

# Fill in the certificate path and the password so the thumbprint can be set as a variable. 

$certPath = '$CertPath = "C:\certificates\mycert.pfx';
$CertPassword ='Password'
$Cert = New-Object System.Security.Cryptography.X509Certificates.X509Certificate2($CertPath, $CertPassword)
$aadClientCertThumbprint = $cert.Thumbprint;

# Enable disk encryption using the client certificate thumbprint

Set-AzVMDiskEncryptionExtension -ResourceGroupName $VMRGName -VMName $VMName -AadClientID $AADClientID -AadClientCertThumbprint $AADClientCertThumbprint -DiskEncryptionKeyVaultUrl $DiskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $KeyVaultResourceId
```
  
### <a name="enable-encryption-using-certificate-based-authentication-and-a-kek-with-azure-powershell"></a>Включение шифрования с использованием проверки подлинности на основе сертификата и KEK с помощью Azure PowerShell

```powershell
# Fill in 'MyVirtualMachineResourceGroup', 'MyKeyVaultResourceGroup', 'My-AAD-client-ID', 'MySecureVault,, 'MySecureVM’, and "KEKName.

$VMRGName = 'MyVirtualMachineResourceGroup';
$KVRGname = 'MyKeyVaultResourceGroup';
$AADClientID ='My-AAD-client-ID';
$KeyVaultName = 'MySecureVault';
$VMName = 'MySecureVM';
$keyEncryptionKeyName ='KEKName';
$KeyVault = Get-AzKeyVault -VaultName $KeyVaultName -ResourceGroupName $KVRGname;
$diskEncryptionKeyVaultUrl = $KeyVault.VaultUri;
$KeyVaultResourceId = $KeyVault.ResourceId;
$keyEncryptionKeyUrl = (Get-AzKeyVaultKey -VaultName $KeyVaultName -Name $keyEncryptionKeyName).Key.kid;

## Fill in the certificate path and the password so the thumbprint can be read and set as a variable.

$certPath = '$CertPath = "C:\certificates\mycert.pfx';
$CertPassword ='Password'
$Cert = New-Object System.Security.Cryptography.X509Certificates.X509Certificate2($CertPath, $CertPassword)
$aadClientCertThumbprint = $cert.Thumbprint;

# Enable disk encryption using the client certificate thumbprint and a KEK

Set-AzVMDiskEncryptionExtension -ResourceGroupName $VMRGName -VMName $VMName -AadClientID $AADClientID -AadClientCertThumbprint $AADClientCertThumbprint -DiskEncryptionKeyVaultUrl $DiskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $KeyVaultResourceId -KeyEncryptionKeyUrl $keyEncryptionKeyUrl -KeyEncryptionKeyVaultId $KeyVaultResourceId
```

## <a name="disable-encryption"></a>Отключение шифрования
Вы можете отключить шифрование с помощью Azure PowerShell, Azure CLI или шаблона Resource Manager. 

- **Отключение шифрования дисков с помощью Azure PowerShell**. для отключения шифрования используйте командлет [Disable-AzureRmVMDiskEncryption](/powershell/module/az.compute/disable-azvmdiskencryption). 
     ```azurepowershell-interactive
     Disable-AzVMDiskEncryption -ResourceGroupName 'MyVirtualMachineResourceGroup' -VMName 'MySecureVM'
     ```

- **Отключение шифрования дисков с помощью Azure CLI:** Чтобы отключить шифрование, используйте команду [az vm encryption disable](/cli/azure/vm/encryption#az-vm-encryption-disable). 
     ```azurecli-interactive
     az vm encryption disable --name "MySecureVM" --resource-group "MyVirtualMachineResourceGroup" --volume-type [ALL, DATA, OS]
     ```
- **Отключение шифрования с помощью шаблона Resource Manager.** 

    1. Нажмите кнопку **Deploy to Azure** (Развернуть в Azure) на странице шаблона [отключения шифрования дисков на виртуальной машине под управлением Windows](https://github.com/Azure/azure-quickstart-templates/tree/master/201-decrypt-running-windows-vm).
    2. Выберите подписку, группу ресурсов, расположение, виртуальную машину, условия использования и соглашение.
    3.  Нажмите кнопку **покупка**, чтобы отключить шифрование дисков на работающей виртуальной машине Windows. 

## <a name="next-steps"></a>Дальнейшие действия

> [!div class="nextstepaction"]
> [Включение шифрования дисков Azure для виртуальных машин IaaS под управлением Linux](azure-security-disk-encryption-linux-aad.md)
