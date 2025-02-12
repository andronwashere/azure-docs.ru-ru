---
title: Вход на виртуальную машину Linux с учетными данными Azure Active Directory | Документация Майкрософт
description: Из этого практического руководства вы узнаете, как создать и настроить виртуальную машину Linux для аутентификации Azure Active Directory при входе пользователей
services: virtual-machines-linux
documentationcenter: ''
author: cynthn
manager: gwallace
editor: ''
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 06/17/2018
ms.author: cynthn
ms.openlocfilehash: f8f00c49ced4e06eb634cbbfb1b786e6729783d2
ms.sourcegitcommit: 2e4b99023ecaf2ea3d6d3604da068d04682a8c2d
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/09/2019
ms.locfileid: "67667651"
---
# <a name="log-in-to-a-linux-virtual-machine-in-azure-using-azure-active-directory-authentication-preview"></a>Вход на виртуальную машину Linux в Azure с аутентификацией Azure Active Directory (предварительная версия)

Для повышения безопасности виртуальных машин Linux в Azure их можно интегрировать с аутентификацией Azure Active Directory (AD). При аутентификации Azure AD для виртуальных машин Linux осуществляется централизованное управление и применение политик, которые разрешают или запрещают доступ к виртуальным машинам. В этой статье показано, как создать и настроить виртуальную машину Linux для аутентификации Azure AD.

> [!NOTE]
> Эта функция доступна в режиме предварительной версии. Не рекомендуем использовать ее для виртуальных машин или рабочих нагрузок в рабочей среде. Функцию следует применять на виртуальной машине, которая будет использоваться только для тестирования.

Есть много преимуществ аутентификации Azure AD при входе на виртуальные машины Linux в Azure, в том числе следующие:

- **Повышение уровня безопасности.**
  - Для входа на виртуальные машины Linux в Azure можно использовать корпоративные учетные данные AD. Нет необходимости создавать учетные записи локального администратора и управлять временем существования учетных данных.
  - Так как учетные записи локального администратора не требуются, вам не придется беспокоиться о потере или краже учетных данных, недостаточной надежности учетных данных пользователей и т. д.
  - Политики сложности паролей и времени их существования, настроенные для вашего каталога Azure AD, также помогают защитить виртуальные машины Linux.
  - Для дальнейшей защиты входа на виртуальные машины Azure можно настроить аутентификацию MFA.
  - Возможность войти на виртуальные машины Linux с помощью Azure Active Directory также доступна для клиентов, использующих [службы федерации](../../active-directory/hybrid/how-to-connect-fed-whatis.md).

- **Эффективная совместная работа.** С помощью управления доступом на основе ролей (RBAC) можно указать, кто может выполнить вход на заданную виртуальную машину как обычный пользователь или с правами администратора. По мере присоединения пользователей к вашей группе или ухода из нее можно обновлять политику RBAC для виртуальной машины, предоставляя соответствующий доступ. Это намного проще, чем очистка виртуальных машин вручную для удаления ненужных открытых ключей SSH. Когда сотрудники уходят из вашей организации, а их учетная запись пользователя отключается или удаляется из Azure AD, они теряют доступ к вашим ресурсам.

## <a name="supported-azure-regions-and-linux-distributions"></a>Поддерживаемые регионы Azure и дистрибутивы Linux

В режиме предварительной версии этой функции сейчас поддерживаются следующие дистрибутивы Linux:

| Дистрибутив | Version |
| --- | --- |
| CentOS | CentOS 6, CentOS 7 |
| Debian | Debian 9 |
| openSUSE | openSUSE Leap 42.3 |
| RedHat Enterprise Linux | RHEL 6, RHEL 7 | 
| SUSE Linux Enterprise Server | SLES 12 |
| Сервер Ubuntu | Ubuntu 14.04 LTS, Ubuntu Server 16.04 и Ubuntu Server 18.04 |


В режиме предварительной версии этой функции сейчас поддерживаются следующие регионы Azure:

- Все глобальные регионы Azure

>[!IMPORTANT]
> Чтобы использовать эту предварительную версию функции, развертывайте только поддерживаемые дистрибутивы Linux и в поддерживаемых регионах Azure. Функция не поддерживается в Azure для государственных организаций и в национальных облаках.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Чтобы установить и использовать интерфейс командной строки локально, для работы с этим руководством вам понадобится Azure CLI 2.0.31 или более поздней версии. Чтобы узнать версию, выполните команду `az --version`. Если вам необходимо выполнить установку или обновление, см. статью [Установка Azure CLI 2.0]( /cli/azure/install-azure-cli).

## <a name="create-a-linux-virtual-machine"></a>Создание виртуальной машины Linux

Создайте группу ресурсов с помощью команды [az group create](/cli/azure/group#az-group-create), а затем — виртуальную машину с помощью команды [az vm create](/cli/azure/vm#az-vm-create), используя поддерживаемый дистрибутив в поддерживаемом регионе. В приведенном ниже примере развертывается виртуальная машина с именем *myVM*, использующая *Ubuntu 16.04 LTS*, в группе ресурсов с именем *myResourceGroup* в регионе *southcentralus*. В следующих примерах можно при необходимости указать собственную группу ресурсов и имена виртуальных машин.

```azurecli-interactive
az group create --name myResourceGroup --location southcentralus

az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys
```

Создание виртуальной машины и вспомогательных ресурсов занимает несколько минут.

## <a name="install-the-azure-ad-login-vm-extension"></a>Установка расширения для входа на виртуальную машину с помощью Azure AD

Чтобы войти виртуальную Машину Linux с помощью учетных данных Azure AD, установите входа Azure Active Directory расширения виртуальной Машины. Расширения виртуальных машин — это небольшие приложения, которые выполняют задачи настройки и автоматизации после развертывания виртуальных машин Azure. С помощью команды [az vm extension set](/cli/azure/vm/extension#az-vm-extension-set) установите расширение *AADLoginForLinux* на виртуальную машину с именем *myVM* в группе ресурсов *myResourceGroup*:

```azurecli-interactive
az vm extension set \
    --publisher Microsoft.Azure.ActiveDirectory.LinuxSSH \
    --name AADLoginForLinux \
    --resource-group myResourceGroup \
    --vm-name myVM
```

После установки расширения на виртуальную машину отображается параметр *provisioningState* со значением *Succeeded*.

## <a name="configure-role-assignments-for-the-vm"></a>Настройка назначений ролей для виртуальной машины

Политика управления доступом на основе ролей (RBAC) Azure определяет, кто может входить на виртуальную машину. Для авторизации входа на виртуальную машину используются две роли RBAC:

- **Имя для входа администратора виртуальной машины.** Пользователи с этой ролью могут входить на виртуальную машину Azure с правами администратора Windows или привилегированного пользователя Linux.
- **Имя для входа пользователя виртуальной машины.** Пользователи с этой ролью могут входить на виртуальную машину Azure с правами обычного пользователя.

> [!NOTE]
> Чтобы разрешить пользователю входить в на виртуальную машину через SSH, необходимо назначить ему роль *Имя для входа администратора виртуальной машины* или *Имя для входа пользователя виртуальной машины*. Пользователю Azure с ролью *Владелец* или *Участник*, назначенной для виртуальной машины, права для входа на эту виртуальную машину через SSH не предоставляются автоматически.

В следующем примере используется команда [az role assignment create](/cli/azure/role/assignment#az-role-assignment-create) для назначения роли *Имя для входа администратора виртуальной машины* виртуальной машине текущего пользователя Azure. Имя пользователя активной учетной записи Azure можно получить с помощью команды [az account show](/cli/azure/account#az-account-show). В качестве *области* задается виртуальная машина, созданная на предыдущем шаге с помощью команды [az vm show](/cli/azure/vm#az-vm-show). Область также можно назначить на уровне группы ресурсов или подписки. При этом применяются обычные разрешения наследования RBAC. Дополнительные сведения см. в статье об [управлении доступом на основе ролей](../../role-based-access-control/overview.md).

```azurecli-interactive
username=$(az account show --query user.name --output tsv)
vm=$(az vm show --resource-group myResourceGroup --name myVM --query id -o tsv)

az role assignment create \
    --role "Virtual Machine Administrator Login" \
    --assignee $username \
    --scope $vm
```

> [!NOTE]
> Если домен AAD и домен имени пользователя для входа не совпадают, необходимо указать идентификатор объекта учетной записи пользователя, предоставив *идентификатор объекта уполномоченного*, а не только имя пользователя *уполномоченного*. Идентификатор объекта учетной записи пользователя можно получить с помощью команды [az ad user list](/cli/azure/ad/user#az-ad-user-list).

Дополнительные сведения об использовании RBAC для управления доступом к ресурсам подписки Azure см. в статьях об использовании [Azure](../../role-based-access-control/role-assignments-cli.md), [портала Azure](../../role-based-access-control/role-assignments-portal.md) или [Azure PowerShell](../../role-based-access-control/role-assignments-powershell.md).

Также можно настроить Azure AD, чтобы при входе конкретного пользователя на виртуальную машину Linux требовалось пройти аутентификацию MFA. Дополнительные сведения см. в статье о [начале работы с MFA Azure в облаке](../../multi-factor-authentication/multi-factor-authentication-get-started-cloud.md).

## <a name="log-in-to-the-linux-virtual-machine"></a>Вход на виртуальную машину Linux

Сначала просмотрите общедоступный IP-адрес виртуальной машины с помощью команды [az vm show](/cli/azure/vm#az-vm-show):

```azurecli-interactive
az vm show --resource-group myResourceGroup --name myVM -d --query publicIps -o tsv
```

Войдите на виртуальную машину Azure с Linux, используя учетные данные Azure AD. Параметр `-l` позволяет указать собственный адрес учетной записи Azure AD. Адреса учетных записей следует вводить в нижнем регистре. Используйте общедоступный IP-адрес виртуальной машины из предыдущей команды:

```azurecli-interactive
ssh -l azureuser@contoso.onmicrosoft.com publicIps
```

Вам будет предложено войти в Azure AD с помощью кода однократного использования по адресу [https://microsoft.com/devicelogin](https://microsoft.com/devicelogin). Скопируйте и вставьте код однократного использования на странице входа устройства, как показано в следующем примере:

```bash
~$ ssh -l azureuser@contoso.onmicrosoft.com 13.65.237.247
To sign in, use a web browser to open the page https://microsoft.com/devicelogin and enter the code FJS3K6X4D to authenticate. Press ENTER when ready.
```

При появлении запроса введите учетные данные для входа в Azure AD на странице входа. После успешной аутентификации в браузере появится следующее сообщение:

    You have signed in to the Microsoft Azure Linux Virtual Machine Sign-In application on your device.

Закройте окно браузера, вернитесь к запросу SSH и нажмите клавишу **ВВОД**. Теперь вы выполнили вход на виртуальную машину Azure под управлением Linux с разрешениями назначенных ролей, таких как *пользователь виртуальной машины* или *администратор виртуальной машины*. Если вашей учетной записи назначена роль *Имя для входа администратора виртуальной машины*, можно выполнять команды, для которых требуются права привилегированного пользователя, с помощью `sudo`.

## <a name="sudo-and-aad-login"></a>Вход с использованием sudo и Azure AD

При первом выполнении sudo вам нужно пройти проверку подлинности во второй раз. Чтобы не проходить повторную проверку подлинности при выполнении sudo, вы можете изменить в файле sudoers `/etc/sudoers.d/aad_admins` следующую строку:

```bash
%aad_admins ALL=(ALL) ALL
```
Вставьте вместо нее эту строку:

```bash
%aad_admins ALL=(ALL) NOPASSWD:ALL
```


## <a name="troubleshoot-sign-in-issues"></a>Устранение неполадок при входе

Распространенные ошибки при попытке входа через SSH с учетными данными Azure AD — отсутствие назначенных ролей RBAC и повторяющиеся запросы на вход. Используйте следующие разделы для устранения этих проблем.

### <a name="access-denied-rbac-role-not-assigned"></a>Доступ запрещен: не назначена роль RBAC

Если в запросе SSH отображается приведенное ниже сообщение об ошибке, проверьте наличие настроенных политик RBAC для виртуальной машины, которая предоставляет пользователю роль *Имя для входа администратора виртуальной машины* или *Имя для входа пользователя виртуальной машины*.

```bash
login as: azureuser@contoso.onmicrosoft.com
Using keyboard-interactive authentication.
To sign in, use a web browser to open the page https://microsoft.com/devicelogin and enter the code FJX327AXD to authenticate. Press ENTER when ready.
Using keyboard-interactive authentication.
Access denied:  to sign-in you be assigned a role with action 'Microsoft.Compute/virtualMachines/login/action', for example 'Virtual Machine User Login'
Access denied
```

### <a name="continued-ssh-sign-in-prompts"></a>Непрерывные запросы на вход SSH

Если вы успешно прошли аутентификацию в веб-браузере, может сразу же поступить повторный запрос на вход со свежим кодом. Эта ошибка обычно вызывается несоответствием между именем входа, указанным в командной строке SSH, и учетной записью, с которой вы вошли в Azure AD. Для решения этой проблемы сделайте следующее:

- Проверьте, правильно ли указано имя входа в командной строке SSH. Опечатка в имени входа может вызвать несоответствие между именем входа, указанным в командной строке SSH, и учетной записью, с которой вы вошли в Azure AD. Например, вы ввели *azuresuer\@contoso.onmicrosoft.com* вместо *azureuser\@contoso.onmicrosoft.com*.
- Если у вас есть несколько учетных записей пользователя, убедитесь, что при входе в Azure AD в окне браузера не была указана другая учетная запись.
- В операционной системе Linux учитывается регистр. Различие между "Azureuser@contoso.onmicrosoft.com" и "azureuser@contoso.onmicrosoft.com" может вызвать несоответствие. Убедитесь в том, что имя участника-пользователя указано в командной строке SSH в правильном регистре.

## <a name="preview-feedback"></a>Отзывы о предварительной версии

Поделиться своими отзывами об этой предварительной версии функции или сообщите о проблемах на [форуме отзывов и предложений по Azure AD](https://feedback.azure.com/forums/169401-azure-active-directory?category_id=166032).

## <a name="next-steps"></a>Следующие шаги

Дополнительные сведения об Azure Active Directory см. в статье [Что такое Azure Active Directory](../../active-directory/fundamentals/active-directory-whatis.md).
