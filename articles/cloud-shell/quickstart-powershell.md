---
title: Краткое руководство по PowerShell в Azure Cloud Shell | Документация Майкрософт
description: Краткое руководство по использованию PowerShell в Cloud Shell.
services: Azure
documentationcenter: ''
author: maertendmsft
manager: timlt
tags: azure-resource-manager
ms.assetid: ''
ms.service: azure
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 10/18/2018
ms.author: damaerte
ms.openlocfilehash: 1fc9883e0ea35c384c3bfc83e76b8eded48cbcba
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "60199533"
---
# <a name="quickstart-for-powershell-in-azure-cloud-shell"></a>Краткое руководство по использованию PowerShell в Azure Cloud Shell

В этом документе объясняется, как использовать PowerShell в Cloud Shell на [портале Azure](https://portal.azure.com/).

> [!NOTE]
> Также вы можете ознакомиться с кратким руководством по использованию [Bash в Azure Cloud Shell](quickstart.md).

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="start-cloud-shell"></a>Запуск Cloud Shell

1. Нажмите кнопку **Cloud Shell** в верхней панели навигации портала Azure.

   ![](media/quickstart-powershell/shell-icon.png)

2. Выберите среду PowerShell из раскрывающегося списка, и вы перейдете к диску Azure `(Azure:)`.

   ![](media/quickstart-powershell/environment-ps.png)

## <a name="run-powershell-commands"></a>Выполнение команд PowerShell

Выполните обычные команды PowerShell в Cloud Shell. Примеры таких команд приведены ниже.

```azurepowershell-interactive
PS Azure:\> Get-Date

# Expected Output
Friday, July 27, 2018 7:08:48 AM

PS Azure:\> Get-AzVM -Status

# Expected Output
ResourceGroupName       Name       Location                VmSize   OsType     ProvisioningState  PowerState
-----------------       ----       --------                ------   ------     -----------------  ----------
MyResourceGroup2        Demo        westus         Standard_DS1_v2  Windows    Succeeded           running
MyResourceGroup         MyVM1       eastus            Standard_DS1  Windows    Succeeded           running
MyResourceGroup         MyVM2       eastus   Standard_DS2_v2_Promo  Windows    Succeeded           deallocated
```

## <a name="navigate-azure-resources"></a>Переход к ресурсам Azure

 1. Получите список всех подписок с диска `Azure`.

    ```azurepowershell-interactive
    PS Azure:\> dir
    ```

 2. Переход к предпочитаемой подписке с помощью команды `cd`.

    ```azurepowershell-interactive
    PS Azure:\> cd MySubscriptionName
    PS Azure:\MySubscriptionName>
    ```

 3. Просмотр всех ресурсов Azure в текущей подписке.

    Введите `dir`, чтобы вывести несколько представлений ресурсов Azure.

    ```azurepowershell-interactive
    PS Azure:\MySubscriptionName> dir

        Directory: azure:\MySubscriptionName

    Mode Name
    ---- ----
    +    AllResources
    +    ResourceGroups
    +    StorageAccounts
    +    VirtualMachines
    +    WebApps
    ```

### <a name="allresources-view"></a>Представление AllResources

Введите `dir` в каталоге `AllResources`, чтобы просмотреть свои ресурсы Azure.

```azurepowershell-interactive
PS Azure:\MySubscriptionName> dir AllResources
```

### <a name="explore-resource-groups"></a>Изучение групп ресурсов

 Можно перейти в каталог `ResourceGroups` и в определенной группе ресурсов найти виртуальные машины.

```azurepowershell-interactive
PS Azure:\MySubscriptionName> cd ResourceGroups\MyResourceGroup1\Microsoft.Compute\virtualMachines

PS Azure:\MySubscriptionName\ResourceGroups\MyResourceGroup1\Microsoft.Compute\virtualMachines> dir


    Directory: Azure:\MySubscriptionName\ResourceGroups\MyResourceGroup1\Microsoft.Compute\virtualMachines


VMName    Location   ProvisioningState VMSize          OS            SKU             OSVersion AdminUserName  NetworkInterfaceName
------    --------   ----------------- ------          --            ---             --------- -------------  --------------------
TestVm1   westus     Succeeded         Standard_DS2_v2 WindowsServer 2016-Datacenter Latest    AdminUser      demo371
TestVm2   westus     Succeeded         Standard_DS1_v2 WindowsServer 2016-Datacenter Latest    AdminUser      demo271
```

> [!NOTE]
> Вы могли заметить, что при втором вводе команды `dir` служба Cloud Shell отображает элементы намного быстрее.
> Это происходит потому, что дочерние элементы сохраняются в памяти для удобства работы пользователя.
Тем не менее, вы всегда можете использовать `dir -Force`, чтобы получить последние данные.

### <a name="navigate-storage-resources"></a>Переход к ресурсам хранилища

Войдите в каталог `StorageAccounts`, чтобы без труда переходить между всеми своими ресурсами хранилища.

```azurepowershell-interactive
PS Azure:\MySubscriptionName\StorageAccounts\MyStorageAccountName\Files> dir

    Directory: Azure:\MySubscriptionNameStorageAccounts\MyStorageAccountName\Files

Name          ConnectionString
----          ----------------
MyFileShare1  \\MyStorageAccountName.file.core.windows.net\MyFileShare1;AccountName=MyStorageAccountName AccountKey=<key>
MyFileShare2  \\MyStorageAccountName.file.core.windows.net\MyFileShare2;AccountName=MyStorageAccountName AccountKey=<key>
MyFileShare3  \\MyStorageAccountName.file.core.windows.net\MyFileShare3;AccountName=MyStorageAccountName AccountKey=<key>
```

Вы можете использовать приведенную ниже команду для подключения файлового ресурса Azure с помощью строки подключения.

```azurepowershell-interactive
net use <DesiredDriveLetter>: \\<MyStorageAccountName>.file.core.windows.net\<MyFileShareName> <AccountKey> /user:Azure\<MyStorageAccountName>
```

Дополнительные сведения см. в статье [Подключение общей папки Azure и получение доступа к этой папке в Windows][azmount].

Кроме того, можно перейти в каталоги в файловом ресурсе Azure, как показано ниже.

```azurepowershell-interactive
PS Azure:\MySubscriptionName\StorageAccounts\MyStorageAccountName\Files> cd .\MyFileShare1\
PS Azure:\MySubscriptionName\StorageAccounts\MyStorageAccountName\Files\MyFileShare1> dir

Mode  Name
----  ----
+     TestFolder
.     hello.ps1
```

### <a name="interact-with-virtual-machines"></a>Взаимодействие с виртуальными машинами

Все виртуальные машины в текущей подписке можно найти в каталоге `VirtualMachines`.

```azurepowershell-interactive
PS Azure:\MySubscriptionName\VirtualMachines> dir

    Directory: Azure:\MySubscriptionName\VirtualMachines


Name       ResourceGroupName  Location  VmSize          OsType              NIC ProvisioningState  PowerState
----       -----------------  --------  ------          ------              --- -----------------  ----------
TestVm1    MyResourceGroup1   westus    Standard_DS2_v2 Windows       my2008r213         Succeeded     stopped
TestVm2    MyResourceGroup1   westus    Standard_DS1_v2 Windows          jpstest         Succeeded deallocated
TestVm10   MyResourceGroup2   eastus    Standard_DS1_v2 Windows           mytest         Succeeded     running
```

#### <a name="invoke-powershell-script-across-remote-vms"></a>Вызов сценария PowerShell на удаленных виртуальных машинах

 > [!WARNING]
 > Ознакомьтесь с разделом [Устранение неполадок удаленного управления виртуальными машинами Azure](troubleshooting.md#troubleshooting-remote-management-of-azure-vms).

  Если у вас есть виртуальная машина MyVM1, воспользуемся `Invoke-AzVMCommand` для вызова сценария PowerShell на удаленном компьютере.

  ```azurepowershell-interactive
  Invoke-AzVMCommand -Name MyVM1 -ResourceGroupName MyResourceGroup -Scriptblock {Get-ComputerInfo} -EnableRemoting
  ```

  Кроме того, можно сначала перейти к каталогу virtualMachines и выполнить команду `Invoke-AzVMCommand`, как показано ниже.

  ```azurepowershell-interactive
  PS Azure:\> cd MySubscriptionName\MyResourceGroup\Microsoft.Compute\virtualMachines
  PS Azure:\MySubscriptionName\MyResourceGroup\Microsoft.Compute\virtualMachines> Get-Item MyVM1 | Invoke-AzVMCommand -Scriptblock {Get-ComputerInfo}

  # You will see output similar to the following:

  PSComputerName                                          : 65.52.28.207
  RunspaceId                                              : 2c2b60da-f9b9-4f42-a282-93316cb06fe1
  WindowsBuildLabEx                                       : 14393.1066.amd64fre.rs1_release_sec.170327-1835
  WindowsCurrentVersion                                   : 6.3
  WindowsEditionId                                        : ServerDatacenter
  WindowsInstallationType                                 : Server
  WindowsInstallDateFromRegistry                          : 5/18/2017 11:26:08 PM
  WindowsProductId                                        : 00376-40000-00000-AA947
  WindowsProductName                                      : Windows Server 2016 Datacenter
  WindowsRegisteredOrganization                           :
   ...
  ```

#### <a name="interactively-log-on-to-a-remote-vm"></a>Интерактивный вход на удаленную виртуальную машину

Можно выполнить `Enter-AzVM` для интерактивного входа на виртуальную машину в Azure.

  ```azurepowershell-interactive
  PS Azure:\> Enter-AzVM -Name MyVM1 -ResourceGroupName MyResourceGroup -EnableRemoting
  ```

Можно также сначала перейти в каталог `VirtualMachines` и выполнить команду `Enter-AzVM`, как показано ниже.

  ```azurepowershell-interactive
 PS Azure:\MySubscriptionName\ResourceGroups\MyResourceGroup\Microsoft.Compute\virtualMachines> Get-Item MyVM1 | Enter-AzVM
 ```

### <a name="discover-webapps"></a>Обнаружение веб-приложений

Войдите в каталог `WebApps`, чтобы переходить между ресурсами своих веб-приложений.

```azurepowershell-interactive
PS Azure:\MySubscriptionName> dir .\WebApps\

    Directory: Azure:\MySubscriptionName\WebApps

Name            State    ResourceGroup      EnabledHostNames                  Location
----            -----    -------------      ----------------                  --------
mywebapp1       Stopped  MyResourceGroup1   {mywebapp1.azurewebsites.net...   West US
mywebapp2       Running  MyResourceGroup2   {mywebapp2.azurewebsites.net...   West Europe
mywebapp3       Running  MyResourceGroup3   {mywebapp3.azurewebsites.net...   South Central US

# You can use Azure cmdlets to Start/Stop your web apps
PS Azure:\MySubscriptionName\WebApps> Start-AzWebApp -Name mywebapp1 -ResourceGroupName MyResourceGroup1

Name           State    ResourceGroup        EnabledHostNames                   Location
----           -----    -------------        ----------------                   --------
mywebapp1      Running  MyResourceGroup1     {mywebapp1.azurewebsites.net ...   West US

# Refresh the current state with -Force
PS Azure:\MySubscriptionName\WebApps> dir -Force

    Directory: Azure:\MySubscriptionName\WebApps

Name            State    ResourceGroup      EnabledHostNames                  Location
----            -----    -------------      ----------------                  --------
mywebapp1       Running  MyResourceGroup1   {mywebapp1.azurewebsites.net...   West US
mywebapp2       Running  MyResourceGroup2   {mywebapp2.azurewebsites.net...   West Europe
mywebapp3       Running  MyResourceGroup3   {mywebapp3.azurewebsites.net...   South Central US
```

## <a name="ssh"></a>SSH

Для аутентификации серверов или виртуальных машин с помощью SSH создайте пару открытого и закрытого ключей в Cloud Shell и опубликуйте открытый ключ в расположении `authorized_keys` на удаленном компьютере, например `/home/user/.ssh/authorized_keys`.

> [!NOTE]
> Вы можете создать открытый и закрытый ключи SSH с помощью `ssh-keygen` и опубликовать их в расположении `$env:USERPROFILE\.ssh` в Cloud Shell.

### <a name="using-ssh"></a>Использование SSH

Следуйте инструкциям [здесь](https://docs.microsoft.com/azure/virtual-machines/linux/quick-create-powershell) для создания новой конфигурации виртуальной Машины с помощью командлетов Azure PowerShell.
Прежде чем вызвать `New-AzVM` для запуска развертывания, добавьте открытый ключ SSH в конфигурацию виртуальной машины.
Новая виртуальная машина будет содержать открытый ключ в расположении `~\.ssh\authorized_keys`. Это позволит запускать на виртуальной машине сеансы SSH без учетных данных.

```azurepowershell-interactive
# Create VM config object - $vmConfig using instructions on linked page above

# Generate SSH keys in Cloud Shell
ssh-keygen -t rsa -b 2048 -f $HOME\.ssh\id_rsa 

# Ensure VM config is updated with SSH keys
$sshPublicKey = Get-Content "$HOME\.ssh\id_rsa.pub"
Add-AzVMSshPublicKey -VM $vmConfig -KeyData $sshPublicKey -Path "/home/azureuser/.ssh/authorized_keys"

# Create a virtual machine
New-AzVM -ResourceGroupName <yourResourceGroup> -Location <vmLocation> -VM $vmConfig

# SSH to the VM
ssh azureuser@MyVM.Domain.Com
```

## <a name="list-available-commands"></a>Вывод списка доступных команд

На диске `Azure` введите команду `Get-AzCommand`, чтобы вывести контекстные команды Azure.

Кроме того, вы всегда можете ввести команду `Get-Command *az* -Module Az.*`, чтобы узнать доступные команды Azure.

## <a name="install-custom-modules"></a>Установка пользовательских модулей

Можно выполнить команду `Install-Module`, чтобы установить модули из [коллекции PowerShell][gallery].

## <a name="get-help"></a>Get-Help

Введите `Get-Help`, чтобы получить сведения о PowerShell в Azure Cloud Shell.

```azurepowershell-interactive
Get-Help
```

Чтобы получить справку по определенному командлету, введите `Get-Help` и этот командлет.

```azurepowershell-interactive
Get-Help Get-AzVM
```

## <a name="use-azure-files-to-store-your-data"></a>Использование файлов Azure для хранения данных

Создайте скрипт, например `helloworld.ps1`, и сохраните его на диске `clouddrive`, чтобы использовать во всех сеансах оболочки.

```azurepowershell-interactive
cd $HOME\clouddrive
# Create a new file in clouddrive directory
New-Item helloworld.ps1
# Open the new file for editing
code .\helloworld.ps1
# Add the content, such as 'Hello World!'
.\helloworld.ps1
Hello World!
```

В следующий раз при использовании PowerShell в Cloud Shell файл `helloworld.ps1` будет находиться в каталоге `$HOME\clouddrive`, который подключена к вашим файловым ресурсам Azure.

## <a name="use-custom-profile"></a>Использование пользовательского профиля

Можно настроить среду PowerShell, создав профили PowerShell `profile.ps1` (или `Microsoft.PowerShell_profile.ps1`).
Сохраните профиль на диске `$profile.CurrentUserAllHosts` (или `$profile.CurrentUserAllHosts`), чтобы его можно было загрузить в каждый сеанс PowerShell в Cloud Shell.

О том, как создать профиль, можно узнать в разделе [About Profiles][profile] (О профилях).

## <a name="use-git"></a>Использование Git

Чтобы клонировать репозиторий Git в Cloud Shell, создайте [личный маркер доступа][githubtoken] и используйте его в качестве имени пользователя. Создав маркер, клонируйте репозиторий, как показано ниже.

```azurepowershell-interactive
  git clone https://<your-access-token>@github.com/username/repo.git
```

## <a name="exit-the-shell"></a>Выйдите из оболочки

Введите команду `exit`, чтобы завершить сеанс.

[bashqs]:quickstart.md
[gallery]:https://www.powershellgallery.com/
[customex]:https://docs.microsoft.com/azure/virtual-machines/windows/extensions-customscript
[profile]: https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.core/about/about_profiles
[azmount]: https://docs.microsoft.com/azure/storage/files/storage-how-to-use-files-windows
[githubtoken]: https://help.github.com/articles/creating-a-personal-access-token-for-the-command-line/
