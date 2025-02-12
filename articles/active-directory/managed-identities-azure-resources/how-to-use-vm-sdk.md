---
title: Как использовать управляемые удостоверения для ресурсов Azure на виртуальной машине Azure с пакетами SDK для Azure
description: Примеры кода для использования пакетов SDK для Azure с виртуальной машиной Azure, для которой включены управляемые удостоверения для ресурсов Azure.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: ''
ms.service: active-directory
ms.subservice: msi
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/01/2017
ms.author: markvi
ms.collection: M365-identity-device-management
ms.openlocfilehash: 00c86562e0fdb4e6d62d44088b7aba08e45e22a4
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "60293240"
---
# <a name="how-to-use-managed-identities-for-azure-resources-on-an-azure-vm-with-azure-sdks"></a>Как использовать управляемые удостоверения для ресурсов Azure на виртуальной машине Azure с пакетами SDK для Azure 

[!INCLUDE [preview-notice](../../../includes/active-directory-msi-preview-notice.md)]  
В этой статье приведен список примеров для пакета SDK, в которых демонстрируется использование поддержки пакета SDK Azure для управляемых удостоверений для ресурсов Azure.

## <a name="prerequisites"></a>Технические условия

[!INCLUDE [msi-qs-configure-prereqs](../../../includes/active-directory-msi-qs-configure-prereqs.md)]

> [!IMPORTANT]
> - Во всех примерах кода и сценариев в этой статье предполагается, что клиент выполняется на виртуальной машине с включенными управляемыми удостоверениями для ресурсов Azure. Используйте функцию подключения виртуальной машины на портале Azure для удаленного подключения к своей виртуальной машине. Дополнительные сведения о включении управляемых удостоверений для ресурсов Azure на виртуальной машине см. в статье [Настройка управляемых удостоверений для ресурсов Azure на виртуальной машине Azure с помощью Azure CLI](qs-configure-portal-windows-vm.md) или в одном из вариантов статьи (с использованием PowerShell, интерфейса командной строки, шаблона или пакета SDK для Azure). 

## <a name="sdk-code-samples"></a>Примеры кода SDK

| SDK             | Пример кода |
| --------------- | ----------- |
| .NET            | [Развертывание шаблона Azure Resource Manager с виртуальной машины Windows с помощью управляемых удостоверений для ресурсов Azure](https://github.com/Azure-Samples/windowsvm-msi-arm-dotnet) |
| .NET Core       | [Вызов служб Azure с виртуальной машины Linux с помощью управляемых удостоверений для ресурсов Azure](https://github.com/Azure-Samples/linuxvm-msi-keyvault-arm-dotnet/) |
| Node.js         | [Управление ресурсами с помощью управляемых удостоверений для ресурсов Azure](https://azure.microsoft.com/resources/samples/resources-node-manage-resources-with-msi/) |
| Python          | [Использование управляемых удостоверений для ресурсов Azure для аутентификации из виртуальной машины](https://azure.microsoft.com/resources/samples/resource-manager-python-manage-resources-with-msi/) |
| Ruby            | [Управление ресурсами из виртуальной машины, на которой включены управляемые удостоверения для ресурсов Azure](https://azure.microsoft.com/resources/samples/resources-ruby-manage-resources-with-msi/) |

## <a name="next-steps"></a>Дальнейшие действия

- В разделе [Пакеты SDK](https://azure.microsoft.com/downloads/) приведен полный список ресурсов для пакета Azure SDK, включая файлы для скачивания библиотеки, документацию и многое другое.
- Чтобы включить управляемые удостоверения для ресурсов Azure на виртуальной машине Azure, ознакомьтесь с разделом [Настройка управляемых удостоверений для ресурсов Azure на виртуальной машине с помощью портала Azure](qs-configure-portal-windows-vm.md).








