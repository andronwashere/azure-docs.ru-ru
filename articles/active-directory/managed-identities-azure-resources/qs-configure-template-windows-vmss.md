---
title: Настройка управляемых удостоверений для ресурсов Azure в масштабируемом наборе виртуальных машин с помощью шаблона
description: Пошаговые инструкции по настройке управляемых удостоверений для ресурсов Azure в масштабируемом наборе виртуальных машин с помощью шаблона Azure Resource Manager.
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
ms.date: 02/20/2018
ms.author: markvi
ms.collection: M365-identity-device-management
ms.openlocfilehash: 6ecbac8af86c3c2c76b7710eb61f71481b86291b
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "66112493"
---
# <a name="configure-managed-identities-for-azure-resources-on-an-azure-virtual-machine-scale-using-a-template"></a>Настройка управляемого удостоверения для ресурсов Azure в масштабируемом виртуальной машины Azure с помощью шаблона

[!INCLUDE [preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Функция управляемых удостоверений для ресурсов Azure предоставляет службам Azure автоматически управляемые удостоверения в Azure Active Directory. Это удостоверение можно использовать для аутентификации в любой службе, которая поддерживает аутентификацию Azure AD, не храня какие-либо учетные данные в коде. 

В этой статье вы узнаете, как выполнять приведенные ниже операции с управляемыми удостоверениями для ресурсов Azure в масштабируемом наборе виртуальных машин Azure с помощью шаблона развертывания Azure Resource Manager.
- Включение и отключение управляемого удостоверения, назначаемого системой, в масштабируемом наборе виртуальных машин Azure
- Добавление и удаление управляемого удостоверения, назначаемого пользователем, в масштабируемом наборе виртуальных машин Azure

## <a name="prerequisites"></a>Технические условия

- Если вы не работали с управляемыми удостоверениями для ресурсов Azure, изучите [общие сведения](overview.md). **Обратите внимание на [различие между управляемыми удостоверениями, назначаемыми системой и назначаемыми пользователями](overview.md#how-does-it-work)** .
- Если у вас нет учетной записи Azure, [зарегистрируйтесь для получения бесплатной пробной учетной записи](https://azure.microsoft.com/free/), прежде чем продолжать.
- Для выполнения операций управления, описанных в этой статье, учетной записи требуются следующие назначения управления доступом на основе ролей Azure:

    > [!NOTE]
    > Дополнительные назначения ролей в каталоге Azure AD не требуются.

    - [Участник виртуальных машин](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor) для создания масштабируемого набора виртуальных машин, а также для включения и удаления управляемого удостоверения, назначаемого системой, и (или) управляемого удостоверения, назначаемого пользователем, из масштабируемого набора виртуальных машин.
    - Роль [Участник управляемого удостоверения](/azure/role-based-access-control/built-in-roles#managed-identity-contributor): для создания управляемого удостоверения, назначаемого пользователем.
    - Роль [Оператор управляемого удостоверения](/azure/role-based-access-control/built-in-roles#managed-identity-operator): для назначения и удаления управляемого удостоверения, назначаемого пользователем, в масштабируемом наборе виртуальных машин.

## <a name="azure-resource-manager-templates"></a>Шаблоны Azure Resource Manager

Так же как портал Azure и сценарии, шаблоны [Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md) предоставляют возможность развертывать новые или измененные ресурсы, определенные в группе ресурсов Azure. Доступно несколько способов редактирования и развертывания шаблона, локально и на портале, в том числе:

   - С помощью [пользовательского шаблона из Azure Marketplace](../../azure-resource-manager/resource-group-template-deploy-portal.md#deploy-resources-from-custom-template), что позволяет создать шаблон с нуля, или основу имеющийся общий или [шаблон быстрого запуска](https://azure.microsoft.com/documentation/templates/).
   - Наследование от имеющейся группы ресурсов путем экспорта шаблона из [исходного развертывания](../../azure-resource-manager/manage-resource-groups-portal.md#export-resource-groups-to-templates) или от [текущего состояния развертывания](../../azure-resource-manager/manage-resource-groups-portal.md#export-resource-groups-to-templates).
   - Использование локального [редактора JSON (например, VS Code)](../../azure-resource-manager/resource-manager-create-first-template.md), а затем передача и развертывание с помощью PowerShell или интерфейса командной строки.
   - Использование [проекта группы ресурсов Azure](../../azure-resource-manager/vs-azure-tools-resource-groups-deployment-projects-create-deploy.md) Visual Studio для создания и развертывания шаблона.  

Независимо оттого, какой вариант выбран, во время первоначального развертывания и повторного развертывания в шаблоне используется одинаковый синтаксис. Включение управляемых удостоверений для ресурсов Azure на новой или существующей виртуальной машине выполняется таким же образом. Коме того, по умолчанию Azure Resource Manager выполняет [добавочное обновление](../../azure-resource-manager/deployment-modes.md) для развертываний.

## <a name="system-assigned-managed-identity"></a>Управляемое удостоверение, назначаемое системой

В этом разделе вы узнаете, как включить и отключить управляемое удостоверение, назначаемое системой, с помощью шаблона Azure Resource Manager.

### <a name="enable-system-assigned-managed-identity-during-creation-the-creation-of-a-virtual-machines-scale-set-or-an-existing-virtual-machine-scale-set"></a>Включить назначенный системой управляемого удостоверения во время создания Создание масштабируемого набора виртуальных машин или имеющийся масштабируемый набор виртуальных машин

1. После входа в Azure локально или через портал Azure используйте учетную запись, связанную с подпиской Azure, которая содержит масштабируемый набор виртуальных машин.
2. Чтобы включить управляемое удостоверение, назначаемое системой, загрузите шаблон в редактор, найдите интересующий ресурс `Microsoft.Compute/virtualMachinesScaleSets` в разделе ресурсов и добавьте свойство `identity` на том же уровне, что и свойство `"type": "Microsoft.Compute/virtualMachinesScaleSets"`. Используйте следующий синтаксис:

   ```JSON
   "identity": { 
       "type": "SystemAssigned"
   }
   ```

> [!NOTE]
> Можно также подготовить к работе управляемых удостоверений для ресурсов Azure масштабируемого набора виртуальных машин расширение, указав его в `extensionProfile` элемент шаблона. Этот шаг необязателен, так как для получения токенов можно также использовать конечную точку службы метаданных экземпляров Azure (IMDS).  Дополнительные сведения см. в разделе [перенос из расширения виртуальной Машины для Azure IMDS для проверки подлинности](howto-migrate-vm-extension.md).


4. Когда все будет готово, необходимо добавить следующие разделы в раздел ресурсов шаблона, который должен выглядеть следующим образом.

   ```json
    "resources": [
        {
            //other resource provider properties...
            "apiVersion": "2018-06-01",
            "type": "Microsoft.Compute/virtualMachineScaleSets",
            "name": "[variables('vmssName')]",
            "location": "[resourceGroup().location]",
            "identity": {
                "type": "SystemAssigned",
            },
           "properties": {
                //other resource provider properties...
                "virtualMachineProfile": {
                    //other virtual machine profile properties...
                    //The following appears only if you provisioned the optional virtual machine scale set extension (to be deprecated)
                    "extensionProfile": {
                        "extensions": [
                            {
                                "name": "ManagedIdentityWindowsExtension",
                                "properties": {
                                  "publisher": "Microsoft.ManagedIdentity",
                                  "type": "ManagedIdentityExtensionForWindows",
                                  "typeHandlerVersion": "1.0",
                                  "autoUpgradeMinorVersion": true,
                                  "settings": {
                                      "port": 50342
                                  }
                                }
                            } 
                        ]
                    }
                }
            }
        }
    ]
   ``` 

### <a name="disable-a-system-assigned-managed-identity-from-an-azure-virtual-machine-scale-set"></a>Отключение управляемого удостоверения, назначаемого системой, в масштабируемом наборе виртуальных машин Azure

Если для масштабируемого набора виртуальных машин управляемое удостоверение, назначаемое системой, больше не требуется, сделайте следующее.

1. После входа в Azure локально или через портал Azure используйте учетную запись, связанную с подпиской Azure, которая содержит масштабируемый набор виртуальных машин.

2. Загрузив шаблон в [редактор](#azure-resource-manager-templates), найдите нужный ресурс `Microsoft.Compute/virtualMachineScaleSets` в разделе `resources`. Если у вашей виртуальной машины есть только управляемое удостоверение, назначаемое системой, его можно отключить, изменив тип удостоверения на `None`.

   **Microsoft.Compute/virtualMachineScaleSets API версии 2018-06-01**

   Если apiVersion имеет значение `2018-06-01` и в виртуальной машине есть управляемые удостоверения, назначаемые как системой, так и пользователем, удалите `SystemAssigned` из типа удостоверения и оставьте `UserAssigned` вместе со значениями словаря userAssignedIdentities.

   **Microsoft.Compute/virtualMachineScaleSets API версии 2018-06-01**

   Если apiVersion имеет значение `2017-12-01` и у масштабируемого набора виртуальных машин есть управляемые удостоверения, назначаемые как системой, так и пользователем, удалите `SystemAssigned` из типов удостоверения и оставьте `UserAssigned` вместе с массивом управляемых удостоверений, назначаемых пользователем, `identityIds`. 
   
    

   В следующем примере показано, как удалить управляемое удостоверение, назначаемое системой, из масштабируемого набора виртуальных машин без управляемых удостоверений, назначаемых пользователем.
   
   ```json
   {
       "name": "[variables('vmssName')]",
       "apiVersion": "2018-06-01",
       "location": "[parameters(Location')]",
       "identity": {
           "type": "None"
        }

   }
   ```

## <a name="user-assigned-managed-identity"></a>Управляемое удостоверение, назначаемое пользователем

В этом разделе вы зададите управляемое удостоверение, назначаемое пользователем, для масштабируемого набора виртуальных машин с помощью шаблона Azure Resource Manager.

> [!Note]
> Чтобы создать управляемое удостоверение, назначаемое пользователем, с помощью шаблона Azure Resource Manager, обратитесь к разделу [Создание управляемого удостоверения, назначаемого пользователем](how-to-manage-ua-identity-arm.md#create-a-user-assigned-managed-identity).

### <a name="assign-a-user-assigned-managed-identity-to-a-virtual-machine-scale-set"></a>Присвоение управляемого удостоверения, назначаемого пользователем, для масштабируемого набора виртуальных машин

1. Чтобы задать управлемое удостоверение, назначаемое пользователем, для масштабируемого набора виртуальных машин, в элемент `resources` добавьте приведенную ниже запись.  Не забудьте заменить `<USERASSIGNEDIDENTITY>` именем созданного управляемого удостоверения, назначаемого пользователем.
   
   **Microsoft.Compute/virtualMachineScaleSets API версии 2018-06-01**

   Если apiVersion имеет значение `2018-06-01`, то управляемые удостоверения, назначаемые пользователем, хранятся в формате словаря `userAssignedIdentities`, а значение `<USERASSIGNEDIDENTITYNAME>` должно храниться в переменной, определенной в разделе `variables` шаблона.

   ```json
   {
       "name": "[variables('vmssName')]",
       "apiVersion": "2018-06-01",
       "location": "[parameters(Location')]",
       "identity": {
           "type": "userAssigned",
           "userAssignedIdentities": {
               "[resourceID('Microsoft.ManagedIdentity/userAssignedIdentities/',variables('<USERASSIGNEDIDENTITYNAME>'))]": {}
           }
       }
    
   }
   ```   

   **Microsoft.Compute/virtualMachineScaleSets API версии 2017-12-01**
    
   Если `apiVersion` соответствует `2017-12-01` или более ранней версии, то управляемые удостоверения, назначаемые пользователем, хранятся в массиве `identityIds`, а значение `<USERASSIGNEDIDENTITYNAME>` должно храниться в переменной, определенной в разделе variables шаблона.

   ```json
   {
       "name": "[variables('vmssName')]",
       "apiVersion": "2017-03-30",
       "location": "[parameters(Location')]",
       "identity": {
           "type": "userAssigned",
           "identityIds": [
               "[resourceID('Micrososft.ManagedIdentity/userAssignedIdentities/',variables('<USERASSIGNEDIDENTITY>'))]"
           ]
       }

   }
   ``` 
> [!NOTE]
> Можно также подготовить к работе управляемых удостоверений для ресурсов Azure масштабируемого набора виртуальных машин расширение, указав его в `extensionProfile` элемент шаблона. Этот шаг необязателен, так как для получения токенов можно также использовать конечную точку службы метаданных экземпляров Azure (IMDS).  Дополнительные сведения см. в разделе [перенос из расширения виртуальной Машины для Azure IMDS для проверки подлинности](howto-migrate-vm-extension.md).

3. По завершении шаблон должен выглядеть следующим образом.
   
   **Microsoft.Compute/virtualMachineScaleSets API версии 2018-06-01**   

   ```json
   "resources": [
        {
            //other resource provider properties...
            "apiVersion": "2018-06-01",
            "type": "Microsoft.Compute/virtualMachineScaleSets",
            "name": "[variables('vmssName')]",
            "location": "[resourceGroup().location]",
            "identity": {
                "type": "UserAssigned",
                "userAssignedIdentities": {
                    "[resourceID('Microsoft.ManagedIdentity/userAssignedIdentities/',variables('<USERASSIGNEDIDENTITYNAME>'))]": {}
                }
            },
           "properties": {
                //other virtual machine properties...
                "virtualMachineProfile": {
                    //other virtual machine profile properties...
                    //The following appears only if you provisioned the optional virtual machine scale set extension (to be deprecated)
                    "extensionProfile": {
                        "extensions": [
                            {
                                "name": "ManagedIdentityWindowsExtension",
                                "properties": {
                                  "publisher": "Microsoft.ManagedIdentity",
                                  "type": "ManagedIdentityExtensionForWindows",
                                  "typeHandlerVersion": "1.0",
                                  "autoUpgradeMinorVersion": true,
                                  "settings": {
                                      "port": 50342
                                  }
                                }
                            } 
                        ]
                    }
                }
            }
        }
    ]
   ```

   **Microsoft.Compute/virtualMachines API версии 2017-12-01**

   ```json
   "resources": [
        {
            //other resource provider properties...
            "apiVersion": "2017-12-01",
            "type": "Microsoft.Compute/virtualMachineScaleSets",
            "name": "[variables('vmssName')]",
            "location": "[resourceGroup().location]",
            "identity": {
                "type": "UserAssigned",
                "identityIds": [
                    "[resourceID('Microsoft.ManagedIdentity/userAssignedIdentities/',variables('<USERASSIGNEDIDENTITYNAME>'))]"
                ]
            },
           "properties": {
                //other virtual machine properties...
                "virtualMachineProfile": {
                    //other virtual machine profile properties...
                    //The following appears only if you provisioned the optional virtual machine scale set extension (to be deprecated)    
                    "extensionProfile": {
                        "extensions": [
                            {
                                "name": "ManagedIdentityWindowsExtension",
                                "properties": {
                                  "publisher": "Microsoft.ManagedIdentity",
                                  "type": "ManagedIdentityExtensionForWindows",
                                  "typeHandlerVersion": "1.0",
                                  "autoUpgradeMinorVersion": true,
                                  "settings": {
                                      "port": 50342
                                  }
                                }
                            } 
                        ]
                    }
                }
            }
        }
    ]
   ```
   ### <a name="remove-user-assigned-managed-identity-from-an-azure-virtual-machine-scale-set"></a>Удаление управляемого удостоверения, назначаемого пользователем, из масштабируемого набора виртуальных машин Azure

Если для масштабируемого набора виртуальных машин больше не требуется управляемое удостоверение, назначаемое пользователем, сделайте следующее.

1. После входа в Azure локально или через портал Azure используйте учетную запись, связанную с подпиской Azure, которая содержит масштабируемый набор виртуальных машин.

2. Загрузив шаблон в [редактор](#azure-resource-manager-templates), найдите нужный ресурс `Microsoft.Compute/virtualMachineScaleSets` в разделе `resources`. Если у масштабируемого набора виртуальных машин есть только управляемое удостоверение, назначаемое пользователем, его можно отключить, изменив тип удостоверения на `None`.

   В следующем примере показано, как удалить все управляемые удостоверения, назначаемые пользователем, из виртуальной машины без управляемых удостоверений, назначаемых системой.

   ```json
   {
       "name": "[variables('vmssName')]",
       "apiVersion": "2018-06-01",
       "location": "[parameters(Location')]",
       "identity": {
           "type": "None"
        }
   }
   ```
   
   **Microsoft.Compute/virtualMachineScaleSets API версии 2018-06-01**
    
   Чтобы удалить отдельное управляемое удостоверение, назначаемое пользователем, из масштабируемого набора виртуальных машин, удалите его из словаря `userAssignedIdentities`.

   Если у вас есть удостоверение, назначаемое системой, сохраните его в значении `type` в рамках значения `identity`.

   **Microsoft.Compute/virtualMachineScaleSets API версии 2017-12-01**

   Чтобы удалить отдельное управляемое удостоверение, назначаемое пользователем, из масштабируемого набора виртуальных машин, удалите его из массива `identityIds`.

   Если у вас есть управляемое удостоверение, назначаемое системой, сохраните его в значении `type` в рамках значения `identity`.
   
## <a name="next-steps"></a>Дальнейшие действия

- [Обзор управляемых удостоверений для ресурсов Azure](overview.md).

