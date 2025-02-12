---
title: Шифрование дисков Azure для Linux | Документация Майкрософт
description: Развертывание шифрования дисков Azure на виртуальной машине Linux с помощью расширения виртуальной машины.
services: virtual-machines-linux
documentationcenter: ''
author: ejarvi
manager: gwallace
editor: ''
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 06/10/2019
ms.author: ejarvi
ms.openlocfilehash: d544aae33faf60be00a2b4ea0a45f405efcedb39
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/09/2019
ms.locfileid: "67706139"
---
# <a name="azure-disk-encryption-for-linux-microsoftazuresecurityazurediskencryptionforlinux"></a>Шифрование дисков Azure для Linux (Microsoft.Azure.Security.AzureDiskEncryptionForLinux)

## <a name="overview"></a>Обзор

Шифрование дисков Azure использует подсистему dm-crypt в Linux для шифрования всего диска в [выбранных дистрибутивах Azure Linux](https://aka.ms/adelinux).  Решение интегрируется с Azure Key Vault, обеспечивая управление секретами и ключами шифрования диска.

## <a name="prerequisites"></a>Предварительные требования

См. полный список [необходимых компонентов для шифрования дисков Azure](
../../security/azure-security-disk-encryption-prerequisites.md).

### <a name="operating-system"></a>Операционная система

Шифрование дисков Azure сейчас поддерживается для некоторых дистрибутивов и версий.  См. в разделе [шифрования дисков Azure поддерживаемые операционные системы: Linux](../../security/azure-security-disk-encryption-prerequisites.md#linux) список дистрибутивов Linux, которые поддерживаются.

### <a name="internet-connectivity"></a>Подключение к Интернету

При шифровании дисков Azure для Linux требуется подключение к Интернету для доступа к Active Directory, хранилищу Key Vault, хранения и конечным точкам управления пакетами.  См. список [необходимых компонентов для шифрования дисков Azure](../../security/azure-security-disk-encryption-prerequisites.md).

## <a name="extension-schemata"></a>Расширение схемы

Существуют две схемы для шифрования дисков Azure: v1.1, более новые и рекомендуемые схему, которая не использует свойства Azure Active Directory (AAD) и v0.1, более старую схему, требуются свойства AAD. Необходимо использовать версию схемы, соответствующее расширению, вы используете: v1.1 схемы для AzureDiskEncryptionForLinux расширение версии 1.1, v0.1 схемы для AzureDiskEncryptionForLinux версия расширения 0,1.
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
          "DiskFormatQuery": "[diskFormatQuery]",
          "EncryptionOperation": "[encryptionOperation]",
          "KeyEncryptionAlgorithm": "[keyEncryptionAlgorithm]",
          "KeyEncryptionKeyURL": "[keyEncryptionKeyURL]",
          "KeyVaultURL": "[keyVaultURL]",
          "SequenceVersion": "sequenceVersion]",
          "VolumeType": "[volumeType]"
        },
        "type": "AzureDiskEncryptionForLinux",
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
      "AADClientSecret": "[aadClientSecret]",
      "Passphrase": "[passphrase]"
    },
    "publisher": "Microsoft.Azure.Security",
    "settings": {
      "AADClientID": "[aadClientID]",
      "DiskFormatQuery": "[diskFormatQuery]",
      "EncryptionOperation": "[encryptionOperation]",
      "KeyEncryptionAlgorithm": "[keyEncryptionAlgorithm]",
      "KeyEncryptionKeyURL": "[keyEncryptionKeyURL]",
      "KeyVaultURL": "[keyVaultURL]",
      "SequenceVersion": "sequenceVersion]",
      "VolumeType": "[volumeType]"
    },
    "type": "AzureDiskEncryptionForLinux",
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
      "AADClientCertificate": "[aadClientCertificate]",
      "Passphrase": "[passphrase]"
    },
    "publisher": "Microsoft.Azure.Security",
    "settings": {
      "AADClientID": "[aadClientID]",
      "DiskFormatQuery": "[diskFormatQuery]",
      "EncryptionOperation": "[encryptionOperation]",
      "KeyEncryptionAlgorithm": "[keyEncryptionAlgorithm]",
      "KeyEncryptionKeyURL": "[keyEncryptionKeyURL]",
      "KeyVaultURL": "[keyVaultURL]",
      "SequenceVersion": "sequenceVersion]",
      "VolumeType": "[volumeType]"
    },
    "type": "AzureDiskEncryptionForLinux",
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
| (необязательно) KeyVaultURL | url | string |
| Passphrase | password | строка | 
| SequenceVersion | uniqueidentifier | строка |
| VolumeType | OS, Data, All | строка |

## <a name="template-deployment"></a>Развертывание шаблона

Пример шаблона развертывания, см. в руководстве по [включению шифрования на работающей виртуальной машине Linux](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-running-linux-vm).

## <a name="azure-cli-deployment"></a>Развертывание с помощью Azure CLI

Инструкции см. в последней версии [документации по Azure CLI](/cli/azure/vm/encryption?view=azure-cli-latest). 

## <a name="troubleshoot-and-support"></a>Устранение неполадок и поддержка

### <a name="troubleshoot"></a>Устранение неполадок

Дополнительные сведения см. в [руководстве по устранению неполадок с шифрованием дисков Azure](../../security/azure-security-disk-encryption-tsg.md).

### <a name="support"></a>Поддержка

Если в любой момент при изучении этой статьи вам потребуется дополнительная помощь, вы можете обратиться к экспертам по Azure на [форумах MSDN Azure и Stack Overflow](https://azure.microsoft.com/support/community/). Кроме того, можно зарегистрировать обращение в службу поддержки Azure. Перейдите на [сайт поддержки Azure](https://azure.microsoft.com/support/options/) и щелкните "Получить поддержку". Дополнительные сведения об использовании службы поддержки Azure см. в статье [Часто задаваемые вопросы о поддержке Microsoft Azure](https://azure.microsoft.com/support/faq/).

## <a name="next-steps"></a>Следующие шаги

Дополнительные сведения о расширениях виртуальных машин см. в обзоре [расширений и компонентов виртуальной машины для Linux](features-linux.md).