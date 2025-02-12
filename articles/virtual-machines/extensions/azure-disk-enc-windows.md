---
title: Шифрование дисков Azure для Windows | Документация Майкрософт
description: Развертывание шифрования дисков Azure на виртуальной машине Windows с помощью расширения виртуальной машины.
services: virtual-machines-windows
documentationcenter: ''
author: ejarvi
manager: gwallace
editor: ''
ms.assetid: ''
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 06/12/2018
ms.author: ejarvi
ms.openlocfilehash: 9a3e135172f0744c053da816b3c77762dbe783c3
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/09/2019
ms.locfileid: "67706101"
---
# <a name="azure-disk-encryption-for-windows-microsoftazuresecurityazurediskencryption"></a>Шифрование дисков Azure для Windows (Microsoft.Azure.Security.AzureDiskEncryption)

## <a name="overview"></a>Обзор

При шифровании дисков Azure используется Bitlocker для полного шифрования диска на виртуальных машинах Azure под управлением Windows.  Шифрование дисков Azure интегрировано в Azure Key Vault, что позволяет управлять секретами и ключами шифрования дисков в подписке Key Vault. 

## <a name="prerequisites"></a>Предварительные требования

См. полный список [необходимых компонентов для шифрования дисков Azure](
../../security/azure-security-disk-encryption-prerequisites.md).

### <a name="operating-system"></a>Операционная система

Список текущих поддерживаемых версий Windows см. в разделе с описанием [необходимых компонентов для шифрования дисков Azure](../../security/azure-security-disk-encryption-prerequisites.md).

### <a name="internet-connectivity"></a>Подключение к Интернету

При шифровании дисков Azure требуется подключение к Интернету для доступа к Active Directory, хранилищу Key Vault, хранения и конечным точкам управления пакетами.  Список параметров безопасности сети см. в разделе с описанием [необходимых компонентов для шифрования дисков Azure](
../../security/azure-security-disk-encryption-prerequisites.md).

## <a name="extension-schemata"></a>Расширение схемы

Существуют две схемы для шифрования дисков Azure: v1.1, более новые и рекомендуемые схему, которая не использует свойства Azure Active Directory (AAD) и v0.1, более старую схему, требуются свойства AAD. Необходимо использовать версию схемы, соответствующее расширению, вы используете: v1.1 схемы для AzureDiskEncryption расширение версии 1.1, v0.1 схемы для AzureDiskEncryption версия расширения 0,1.

### <a name="schema-v11-no-aad-recommended"></a>Версия 1.1 схемы. Нет AAD (рекомендуется)

Схема v1.1 рекомендуется и не требует свойства Azure Active Directory.

```json
{
  "type": "extensions",
  "name": "[name]",
  "apiVersion": "2015-06-15",
  "location": "[location]",
  "properties": {
    "publisher": "Microsoft.Azure.Security",
    "settings": {
      "EncryptionOperation": "[encryptionOperation]",
      "KeyEncryptionAlgorithm": "[keyEncryptionAlgorithm]",
      "KeyEncryptionKeyURL": "[keyEncryptionKeyURL]",
      "KekVaultResourceId": "[keyVaultResourceID]",
      "KeyVaultURL": "[keyVaultURL]",
      "KeyVaultResourceId": "[keyVaultResourceID]",
      "SequenceVersion": "sequenceVersion]",
      "VolumeType": "[volumeType]"
    },
  "type": "AzureDiskEncryption",
  "typeHandlerVersion": "[extensionVersion]"
  }
}
```


### <a name="schema-v01-with-aad"></a>Схемы v0.1: с помощью AAD 

Схема 0,1 требует `aadClientID` и либо `aadClientSecret` или `AADClientCertificate`.

С помощью `aadClientSecret`:

```json
{
  "type": "extensions",
  "name": "[name]",
  "apiVersion": "2015-06-15",
  "location": "[location]",
  "properties": {
    "protectedSettings": {
      "AADClientSecret": "[aadClientSecret]"
    },    
    "publisher": "Microsoft.Azure.Security",
    "settings": {
      "AADClientID": "[aadClientID]",
      "EncryptionOperation": "[encryptionOperation]",
      "KeyEncryptionAlgorithm": "[keyEncryptionAlgorithm]",
      "KeyEncryptionKeyURL": "[keyEncryptionKeyURL]",
      "KekVaultResourceId": "[keyVaultResourceID]",
      "KeyVaultURL": "[keyVaultURL]",
      "KeyVaultResourceId": "[keyVaultResourceID]",
      "SequenceVersion": "sequenceVersion]",
      "VolumeType": "[volumeType]"
    },
  "type": "AzureDiskEncryption",
  "typeHandlerVersion": "[extensionVersion]"
  }
}
```

С помощью `AADClientCertificate`:

```json
{
  "type": "extensions",
  "name": "[name]",
  "apiVersion": "2015-06-15",
  "location": "[location]",
  "properties": {
    "protectedSettings": {
      "AADClientCertificate": "[aadClientCertificate]"
    },    
    "publisher": "Microsoft.Azure.Security",
    "settings": {
      "AADClientID": "[aadClientID]",
      "EncryptionOperation": "[encryptionOperation]",
      "KeyEncryptionAlgorithm": "[keyEncryptionAlgorithm]",
      "KeyEncryptionKeyURL": "[keyEncryptionKeyURL]",
      "KekVaultResourceId": "[keyVaultResourceID]",
      "KeyVaultURL": "[keyVaultURL]",
      "KeyVaultResourceId": "[keyVaultResourceID]",
      "SequenceVersion": "sequenceVersion]",
      "VolumeType": "[volumeType]"
    },
  "type": "AzureDiskEncryption",
  "typeHandlerVersion": "[extensionVersion]"
  }
}
```


### <a name="property-values"></a>Значения свойств

| Имя | Значение и пример | Тип данных |
| ---- | ---- | ---- |
| apiVersion | 2015-06-15 | date |
| publisher | Microsoft.Azure.Security | строка |
| type | AzureDiskEncryptionForLinux | строка |
| typeHandlerVersion | 0.1, 1.1 | ssNoversion |
| (0,1 схема) AADClientID | xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx | GUID | 
| (0,1 схема) AADClientSecret | password | строка |
| (0,1 схема) AADClientCertificate | thumbprint | строка |
| DiskFormatQuery | {"dev_path":"","name":"","file_system":""} | Словарь JSON |
| EncryptionOperation | EnableEncryption, EnableEncryptionFormatAll | строка | 
| KeyEncryptionAlgorithm | 'RSA-OAEP', 'RSA-OAEP-256', 'RSA1_5' | строка |
| KeyEncryptionKeyURL | url | строка |
| KeyVaultURL | url | строка |
| (необязательно) Passphrase | password | строка | 
| SequenceVersion | uniqueidentifier | строка |
| VolumeType | OS, Data, All | строка |

## <a name="template-deployment"></a>Развертывание шаблона
Пример шаблона развертывания см. в руководстве по [созданию зашифрованной виртуальной машины Windows из образа коллекции](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-create-new-vm-gallery-image).

## <a name="azure-cli-deployment"></a>Развертывание с помощью Azure CLI

Инструкции см. в последней версии [документации по Azure CLI](/cli/azure/vm/encryption?view=azure-cli-latest). 

## <a name="troubleshoot-and-support"></a>Устранение неполадок и поддержка

### <a name="troubleshoot"></a>Устранение неполадок

Дополнительные сведения см. в [руководстве по устранению неполадок с шифрованием дисков Azure](../../security/azure-security-disk-encryption-tsg.md).

### <a name="support"></a>Поддержка

Если в любой момент при изучении этой статьи вам потребуется дополнительная помощь, вы можете обратиться к экспертам по Azure на [форумах MSDN Azure и Stack Overflow](https://azure.microsoft.com/support/community/). Кроме того, можно зарегистрировать обращение в службу поддержки Azure. Перейдите на [сайт поддержки Azure](https://azure.microsoft.com/support/options/) и щелкните "Получить поддержку". Дополнительные сведения об использовании службы поддержки Azure см. в статье [Часто задаваемые вопросы о поддержке Microsoft Azure](https://azure.microsoft.com/support/faq/).

## <a name="next-steps"></a>Следующие шаги
См. дополнительные сведения о [расширениях и компонентах виртуальных машин Windows](features-windows.md).
