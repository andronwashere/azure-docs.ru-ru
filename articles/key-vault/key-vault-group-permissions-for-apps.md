---
title: Предоставление разрешения на доступ к хранилищу ключей Azure нескольким приложениям — Azure Key Vault | Документация Майкрософт
description: Узнайте, как нескольким приложениям предоставить разрешение на доступ к хранилищу ключей
services: key-vault
author: amitbapat
manager: barbkess
tags: azure-resource-manager
ms.service: key-vault
ms.topic: conceptual
ms.date: 01/07/2019
ms.author: ambapat
ms.openlocfilehash: b1d0b0948e089d41f460ac2a54150ee51333f87c
ms.sourcegitcommit: 1289f956f897786090166982a8b66f708c9deea1
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/17/2019
ms.locfileid: "64721993"
---
# <a name="grant-several-applications-access-to-a-key-vault"></a>Предоставление нескольким приложениям доступа к хранилищу ключей

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Для предоставления нескольким приложениям доступа к хранилищу ключей можно использовать политики управления доступом. Политика управления доступом может поддерживать до 1024 приложений и настроена следующим образом:

1. Создайте группу безопасности Azure Active Directory. 
2. Добавьте все приложения, связанные с субъекты-службами в группу безопасности.
3. Предоставьте доступ к группе безопасности к вашему хранилищу ключей.

## <a name="prerequisites"></a>Технические условия

Выполните следующее:
* [Установите Azure PowerShell](/powershell/azure/overview).
* [Установите модуль PowerShell для Azure Active Directory версии 2](https://www.powershellgallery.com/packages/AzureAD).
* Получите разрешения на создание и изменение групп в клиенте Azure Active Directory. Если у вас нет разрешений, обратитесь к администратору Azure Active Directory. Ознакомиться со сведениями о разрешениях политики доступа Key Vault можно в статье [Сведения о ключах, секретах и сертификатах](about-keys-secrets-and-certificates.md).

## <a name="granting-key-vault-access-to-applications"></a>Предоставление Key Vault доступа к приложениям

В PowerShell выполните следующие команды:

```powershell
# Connect to Azure AD 
Connect-AzureAD 
 
# Create Azure Active Directory Security Group 
$aadGroup = New-AzureADGroup -Description "Contoso App Group" -DisplayName "ContosoAppGroup" -MailEnabled 0 -MailNickName none -SecurityEnabled 1 
 
# Find and add your applications (ServicePrincipal ObjectID) as members to this group 
$spn = Get-AzureADServicePrincipal –SearchString "ContosoApp1" 
Add-AzureADGroupMember –ObjectId $aadGroup.ObjectId -RefObjectId $spn.ObjectId 
 
# You can add several members to this group, in this fashion. 
 
# Set the Key Vault ACLs 
Set-AzKeyVaultAccessPolicy –VaultName ContosoVault –ObjectId $aadGroup.ObjectId `
-PermissionsToKeys decrypt,encrypt,unwrapKey,wrapKey,verify,sign,get,list,update,create,import,delete,backup,restore,recover,purge `
–PermissionsToSecrets get,list,set,delete,backup,restore,recover,purge `
–PermissionsToCertificates get,list,delete,create,import,update,managecontacts,getissuers,listissuers,setissuers,deleteissuers,manageissuers,recover,purge,backup,restore `
-PermissionsToStorage get,list,delete,set,update,regeneratekey,getsas,listsas,deletesas,setsas,recover,backup,restore,purge 
 
# Of course you can adjust the permissions as required 
```

Если группе приложений необходимо предоставить другой набор разрешений, создайте для таких приложений отдельную группу безопасности Azure Active Directory.

## <a name="next-steps"></a>Дальнейшие действия

Дополнительные сведения см. в статье [Защита хранилища ключей](key-vault-secure-your-key-vault.md).
