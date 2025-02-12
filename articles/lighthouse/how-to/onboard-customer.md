---
title: Azure Lighthouse — подключение клиента к системе делегированного управления ресурсами Azure
description: Узнайте, как подключить клиента к системе делегированного управления ресурсами Azure, предоставив доступ к ресурсам и возможность управления ими через собственный клиент.
author: JnHs
ms.author: jenhayes
ms.service: lighthouse
ms.date: 07/11/2019
ms.topic: overview
manager: carmonm
ms.openlocfilehash: 1885a6220f14de234710b6980b5d3b6a6172bb7e
ms.sourcegitcommit: 47ce9ac1eb1561810b8e4242c45127f7b4a4aa1a
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/11/2019
ms.locfileid: "67810858"
---
# <a name="onboard-a-customer-to-azure-delegated-resource-management"></a>Подключение клиента к системе делегированного управления ресурсами Azure

В этой статье объясняется, как вы в роли поставщика услуг можете подключить клиента к системе делегированного управления ресурсами Azure, позволяя получать доступ к своим делегированным ресурсам (подпискам и (или) группам ресурсов) и управлять ими через собственный клиент Azure Active Directory (Azure AD). Хотя здесь будет идти речь о поставщиках услуг и клиентах, предприятия, управляющие несколькими арендаторами, могут использовать один и тот же процесс для консолидации своих возможностей управления.

Вы можете повторить этот процесс, если управляете ресурсами для нескольких клиентов. Затем, когда полномочный пользователь входит в клиент, он может быть авторизован во всех областях аренды клиента для выполнения операций управления без необходимости входа в каждый отдельный клиент.

Вы можете связать свой идентификатор Microsoft Partner Network (MPN) со встроенными подписками, чтобы отслеживать свое влияние на взаимодействие с клиентами. Дополнительные сведения см. в статье [Link a partner ID to your Azure accounts](https://docs.microsoft.com/azure/billing/billing-partner-admin-link-started) (Привязка идентификатора партнера к учетной записи Azure).

> [!NOTE]
> Клиенты могут быть автоматически подключены при покупке предложения управляемых служб (общедоступных или частных), которое вы опубликовали в Azure Marketplace. Дополнительные сведения см. в статье [Publish a managed services offer to Azure Marketplace](publish-managed-services-offers.md) (Публикация предложения управляемых служб в Azure Marketplace). Описанный здесь процесс подключения также можно использовать с предложением, опубликованным в Azure Marketplace.

Для процесса подключения требуется, чтобы действия выполнялись в клиенте поставщика услуг и клиенте пользователя. Все эти шаги описаны в данной статье.

> [!IMPORTANT]
> В настоящее время вы не можете подключить подписку (или группу ресурсов в подписке) для системы делегированного управления ресурсами Azure, если в подписке используется Azure Databricks. Аналогичным образом, если подписка зарегистрирована для подключения к поставщику ресурсов **Microsoft.ManagedServices**, вы не сможете на данный момент создать рабочее пространство Databricks для этой подписки.

## <a name="gather-tenant-and-subscription-details"></a>Сбор сведений о клиентах и подписках

Чтобы подключить клиент пользователя, у него должна быть активная подписка Azure. Для этого потребуется следующее:

- идентификатор клиента поставщика услуг (где вы будете управлять ресурсами клиента);
- идентификатор пользовательского клиента (в котором будут ресурсы, управляемые поставщиком услуг);
- идентификаторы подписки для каждой конкретной подписки в клиенте пользователя, которой будет управлять поставщик услуг (или которая содержит группы ресурсов, управляемые поставщиком услуг).

Если у вас еще нет этих данных, вы можете получить их с помощью одного из следующих способов.

### <a name="azure-portal"></a>Портал Azure

Идентификатор клиента можно увидеть, наведя указатель мыши на имя учетной записи в правом верхнем углу портала Azure либо выбрав **Переключение каталога**. Чтобы выбрать и скопировать идентификатор клиента, выполните поиск по запросу "Azure Active Directory" на портале, затем выберите параметр **Свойства** и скопируйте значение, отображаемое в поле **Идентификатор каталога**. Чтобы найти идентификатор подписки, выполните поиск по запросу "Подписки", а затем выберите идентификатор соответствующей подписки.

### <a name="powershell"></a>PowerShell

```azurepowershell-interactive
# Log in first with Connect-AzAccount if you're not using Cloud Shell

Select-AzSubscription <subscriptionId>
```

### <a name="azure-cli"></a>Инфраструктура CLI Azure

```azurecli-interactive
# Log in first with az login if you're not using Cloud Shell

az account set --subscription <subscriptionId/name>
az account show
```


## <a name="ensure-the-customers-subscription-is-registered-for-onboarding"></a>Проверка регистрации подписки клиента для подключения

Каждая подписка должна быть авторизована для подключения путем регистрации поставщика ресурсов **Microsoft.ManagedServices** вручную. Клиент может зарегистрировать подписку, выполнив действия, описанные в статье [Azure resource providers and types](../../azure-resource-manager/resource-manager-supported-services.md) (Поставщики и типы ресурсов Azure).

Клиент может подтвердить, что подписка готова для подключения, одним из следующих способов.

### <a name="azure-portal"></a>Портал Azure

1. Выберите подписку на портале Azure.
1. Выберите параметр **Поставщики ресурсов**.
1. Убедитесь, что для поставщика ресурсов **Microsoft.ManagedServices** отображается состояние **Зарегистрировано**.

### <a name="powershell"></a>PowerShell

```azurepowershell-interactive
# Log in first with Connect-AzAccount if you're not using Cloud Shell

Set-AzContext -Subscription <subscriptionId>
Get-AzResourceProvider -ProviderNameSpace 'Microsoft.ManagedServices'
```

Эта команда должна вернуть результаты следующего вида:

```output
ProviderNamespace : Microsoft.ManagedServices
RegistrationState : Registered
ResourceTypes     : {registrationDefinitions}
Locations         : {}

ProviderNamespace : Microsoft.ManagedServices
RegistrationState : Registered
ResourceTypes     : {registrationAssignments}
Locations         : {}

ProviderNamespace : Microsoft.ManagedServices
RegistrationState : Registered
ResourceTypes     : {operations}
Locations         : {}
```

### <a name="azure-cli"></a>Инфраструктура CLI Azure

```azurecli-interactive
# Log in first with az login if you're not using Cloud Shell

az account set –subscription <subscriptionId>
az provider show –namespace "Microsoft.ManagedServices" –-output table
```

Эта команда должна вернуть результаты следующего вида:

```output
Namespace                  RegistrationState
-------------------------  -------------------
Microsoft.ManagedServices  Registered
```

## <a name="define-roles-and-permissions"></a>Определение ролей и разрешений

Как поставщик услуг вы можете использовать несколько предложений с одним клиентом, для которого требуется разный доступ для разных областей.

Чтобы упростить управление, для каждой роли рекомендуется использовать группы пользователей Azure AD. Благодаря этому можно добавлять или удалять отдельных пользователей в группе, а не назначать разрешения непосредственно этому пользователю. Вы также можете назначить роли субъекту-службе. Обязательно следуйте принципам минимальных привилегий, чтобы у пользователей были только разрешения, необходимые для выполнения их заданий. Это помогает снизить вероятность непреднамеренных ошибок. Дополнительные сведения см. в статье [Recommended security practices](../concepts/recommended-security-practices.md) (Рекомендации по безопасности).

> [!NOTE]
> При назначении ролей должны использоваться [встроенные роли](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles) управления доступом на основе ролей (RBAC). В настоящее время все встроенные роли поддерживаются с помощью системы делегированного управления ресурсами Azure, за исключением роли "Владелец" и любых встроенных ролей с разрешением [DataActions](https://docs.microsoft.com/azure/role-based-access-control/role-definitions#dataactions). Встроенная роль "Администратор доступа пользователей" поддерживается для ограниченного использования, как описано ниже. Пользовательские роли и [роли классического администратора подписки](https://docs.microsoft.com/azure/role-based-access-control/classic-administrators) также не поддерживаются.

Чтобы определить авторизации, вам необходимо знать значения идентификатора для каждого пользователя, группы пользователей или субъекта-службы, которым требуется предоставить доступ. Вам также потребуется идентификатор определения роли для каждой встроенной роли, которую вы хотите назначить. Если их у вас еще нет, вы можете получить их одним из следующих способов.



### <a name="powershell"></a>PowerShell

```azurepowershell-interactive
# Log in first with Connect-AzAccount if you're not using Cloud Shell

# To retrieve the objectId for an Azure AD group
(Get-AzADGroup -DisplayName '<yourGroupName>').id

# To retrieve the objectId for an Azure AD user
(Get-AzADUser -UserPrincipalName '<yourUPN>').id

# To retrieve the objectId for an SPN
(Get-AzADApplication -DisplayName '<appDisplayName>').objectId

# To retrieve role definition IDs
(Get-AzRoleDefinition -Name '<roleName>').id
```

### <a name="azure-cli"></a>Инфраструктура CLI Azure

```azurecli-interactive
# Log in first with az login if you're not using Cloud Shell

# To retrieve the objectId for an Azure AD group
az ad group list –-query "[?displayName == '<yourGroupName>'].objectId" –-output tsv

# To retrieve the objectId for an Azure AD user
az ad user show –-upn-or-object-id "<yourUPN>" –-query "objectId" –-output tsv

# To retrieve the objectId for an SPN
az ad sp list –-query "[?displayName == '<spDisplayName>'].objectId" –-output tsv

# To retrieve role definition IDs
az role definition list –-name "<roleName>" | grep name
```

## <a name="create-an-azure-resource-manager-template"></a>Создание шаблона Azure Resource Manager

Чтобы подключить клиента, необходимо создать шаблон [Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/), который включает следующее:

|Поле  |Определение  |
|---------|---------|
|**mspName**     |Имя поставщика услуг         |
|**mspOfferDescription**     |Краткое описание предложения (например, "Предложение для управления виртуальной машиной Contoso")      |
|**managedByTenantId**     |Идентификатор клиента         |
|**Авторизации**     |Значения **principalId** для пользователей/групп/имен субъектов-служб из клиента, каждое из которых имеет параметр **principalIdDisplayName**, помогающий клиенту понять назначение авторизации, и сопоставленное со встроенным значением **roleDefinitionId**, определяющее уровень доступа         |

Чтобы подключить подписку клиента, используйте соответствующий шаблон Azure Resource Manager, который предоставляется в [репозитории с примерами](https://github.com/Azure/Azure-Lighthouse-samples/), вместе с соответствующим файлом параметров, который вы затем изменяете в соответствии со своей конфигурацией и определяете свои авторизации. Отдельные шаблоны предоставляются в зависимости от того, подключаете ли вы всю подписку, группу ресурсов или несколько групп ресурсов в рамках подписки. Мы также предоставляем шаблон для клиентов, которые приобрели предложение управляемых служб, опубликованное в Azure Marketplace, если вы предпочитаете таким образом подключать свои подписки.

|**Чтобы подключить**  |**Используйте этот шаблон Azure Resource Manager**  |**И измените этот файл параметров** |
|---------|---------|---------|
|Subscription   |[delegatedResourceManagement.json](https://github.com/Azure/Azure-Lighthouse-samples/blob/master/Azure-Delegated-Resource-Management/templates/delegated-resource-management/delegatedResourceManagement.json)  |[delegatedResourceManagement.parameters.json](https://github.com/Azure/Azure-Lighthouse-samples/blob/master/Azure-Delegated-Resource-Management/templates/delegated-resource-management/delegatedResourceManagement.parameters.json)    |
|Resource group   |[rgDelegatedResourceManagement.json](https://github.com/Azure/Azure-Lighthouse-samples/blob/master/Azure-Delegated-Resource-Management/templates/rg-delegated-resource-management/rgDelegatedResourceManagement.json)  |[rgDelegatedResourceManagement.parameters.json](https://github.com/Azure/Azure-Lighthouse-samples/blob/master/Azure-Delegated-Resource-Management/templates/rg-delegated-resource-management/rgDelegatedResourceManagement.parameters.json)    |
|Несколько групп ресурсов в рамках подписки   |[multipleRgDelegatedResourceManagement.json](https://github.com/Azure/Azure-Lighthouse-samples/blob/master/Azure-Delegated-Resource-Management/templates/rg-delegated-resource-management/multipleRgDelegatedResourceManagement.json)  |[multipleRgDelegatedResourceManagement.parameters.json](https://github.com/Azure/Azure-Lighthouse-samples/blob/master/Azure-Delegated-Resource-Management/templates/rg-delegated-resource-management/multipleRgDelegatedResourceManagement.parameters.json)    |
|Подписка (при использовании предложения, опубликованного в Azure Marketplace)   |[marketplaceDelegatedResourceManagement.json](https://github.com/Azure/Azure-Lighthouse-samples/blob/master/Azure-Delegated-Resource-Management/templates/marketplace-delegated-resource-management/marketplaceDelegatedResourceManagement.json)  |[marketplaceDelegatedResourceManagement.parameters.json](https://github.com/Azure/Azure-Lighthouse-samples/blob/master/Azure-Delegated-Resource-Management/templates/marketplace-delegated-resource-management/marketplaceDelegatedResourceManagement.parameters.json)    |

> [!IMPORTANT]
> Для описанного здесь процесса требуется отдельное развертывание для каждой подключенной подписки.
> 
> Требуются также отдельные развертывания, если вы подключаете несколько групп ресурсов в разных подписках. Тем не менее подключение нескольких групп ресурсов в рамках одной подписки может быть выполнено за одно развертывание.

В следующем примере показан измененный файл **resourceProjection.parameters.json**, который будет использоваться для подключения подписки. Файлы параметров группы ресурсов (расположенные в папке [rg-delegated-resource-management](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/Azure-Delegated-Resource-Management/templates/rg-delegated-resource-management)) аналогичны, но также содержат параметр **rgName** для определения конкретных групп ресурсов, которые должны быть подключены.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2018-05-01/subscriptionDeploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "mspName": {
            "value": "Fabrikam Managed Services - Interstellar"
        },
        "mspOfferDescription": {
            "value": "Fabrikam Managed Services - Interstellar"
        },
        "managedByTenantId": {
            "value": "df4602a3-920c-435f-98c4-49ff031b9ef6"
        },
        "authorizations": {
            "value": [
                {
                    "principalId": "0019bcfb-6d35-48c1-a491-a701cf73b419",
                    "principalIdDisplayName": "Tier 1 Support",
                    "roleDefinitionId": "b24988ac-6180-42a0-ab88-20f7382dd24c"
                },
                {
                    "principalId": "0019bcfb-6d35-48c1-a491-a701cf73b419",
                    "principalIdDisplayName": "Tier 1 Support",
                    "roleDefinitionId": "36243c78-bf99-498c-9df9-86d9f8d28608"
                },
                {
                    "principalId": "0afd8497-7bff-4873-a7ff-b19a6b7b332c",
                    "principalIdDisplayName": "Tier 2 Support",
                    "roleDefinitionId": "acdd72a7-3385-48ef-bd42-f606fba81ae7"
                },
                {
                    "principalId": "9fe47fff-5655-4779-b726-2cf02b07c7c7",
                    "principalIdDisplayName": "Service Automation Account",
                    "roleDefinitionId": "b24988ac-6180-42a0-ab88-20f7382dd24c"
                },
                {
                    "principalId": "3kl47fff-5655-4779-b726-2cf02b05c7c4",
                    "principalIdDisplayName": "Policy Automation Account",
                    "roleDefinitionId": "18d7d88d-d35e-4fb5-a5c3-7773c20a72d9",
                    "delegatedRoleDefinitionIds": [
                        "b24988ac-6180-42a0-ab88-20f7382dd24c",
                        "92aaf0da-9dab-42b6-94a3-d43ce8d16293"
                    ]
                }
            ]
        }
    }
}
```
Последняя авторизация в приведенном выше примере добавляет параметр **principalId** с ролью "Администратор доступа пользователя" (18d7d88d-d35e-4fb5-a5c3-7773c20a72d9). При назначении этой роли вы должны включить свойство **delegatedRoleDefinitionIds** и одну или несколько встроенных ролей. Пользователь, созданный в этой авторизации, сможет назначать управляемым удостоверениям эти встроенные роли. Обратите внимание, что к этому пользователю не будут применяться никакие другие разрешения, обычно связанные с ролью администратора доступа пользователей.

## <a name="deploy-the-azure-resource-manager-templates"></a>Развертывание шаблонов Azure Resource Manager

После обновления файла параметров клиент должен развернуть шаблон Resource Management в своем клиенте в качестве развертывания на уровне подписки. Отдельное развертывание необходимо для каждой подписки, которую вы хотите подключить к системе делегированного управления ресурсами Azure (или для каждой подписки, которая содержит группы ресурсов, которые вы хотите подключить).

> [!IMPORTANT]
> Развертывание должно быть выполнено с помощью не гостевой учетной записи в клиенте пользователя, которая имеет [встроенную роль владельца](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#owner) для подключаемой подписки (или которая содержит группы ресурсов, которые подключаются).

```azurepowershell-interactive
# Log in first with Connect-AzAccount if you're not using Cloud Shell

# Deploy Azure Resource Manager template using template and parameter file locally
New-AzDeployment -Name <deploymentName> `
                 -Location <AzureRegion> `
                 -TemplateFile <pathToTemplateFile> `
                 -TemplateParameterFile <pathToParameterFile> `
                 -Verbose

# Deploy Azure Resource Manager template that is located externally
New-AzDeployment -Name <deploymentName> `
                 -Location <AzureRegion> `
                 -TemplateUri <templateUri> `
                 -TemplateParameterUri <parameterUri> `
                 -Verbose
```

### <a name="azure-cli"></a>Инфраструктура CLI Azure

```azurecli-interactive
# Log in first with az login if you're not using Cloud Shell

# Deploy Azure Resource Manager template using template and parameter file locally
az deployment create –-name <deploymentName> \
                     --location <AzureRegion> \
                     --template-file <pathToTemplateFile> \
                     --parameters <parameters/parameterFile> \
                     --verbose

# Deploy external Azure Resource Manager template, with local parameter file
az deployment create –-name <deploymentName \
                     –-location <AzureRegion> \
                     --template-uri <templateUri> \
                     --parameters <parameterFile> \
                     --verbose
```

## <a name="confirm-successful-onboarding"></a>Подтверждение успешного подключения

После того, как подписка клиента была успешно подключена к системе делегированного управления ресурсами Azure, пользователи в клиенте поставщика услуг смогут видеть подписку и ее ресурсы (если им был предоставлен доступ к ней в рамках вышеописанного процесса, индивидуально или в качестве участника группы Azure AD с соответствующими разрешениями). Чтобы подтвердить это, убедитесь, что подписка отображается одним из следующих способов.  

### <a name="azure-portal"></a>Портал Azure

В клиенте поставщика услуг выполните приведенные ниже действия.

1. Перейдите на страницу [Мои клиенты](view-manage-customers.md).
2. Выберите **Клиенты**.
3. Убедитесь, что вы видите подписки с именем предложения, которое вы указали в шаблоне Resource Manager.

В клиенте пользователя выполните следующее.

1. Перейдите на страницу [Поставщики услуг](view-manage-service-providers.md).
2. Выберите **Предложения поставщика услуг**.
3. Убедитесь, что вы видите подписки с именем предложения, которое вы указали в шаблоне Resource Manager.

> [!NOTE]
> После завершения развертывания может пройти несколько минут, прежде чем обновления отобразятся на портале Azure.

### <a name="powershell"></a>PowerShell

```azurepowershell-interactive
# Log in first with Connect-AzAccount if you're not using Cloud Shell

Get-AzContext
```

### <a name="azure-cli"></a>Инфраструктура CLI Azure

```azurecli-interactive
# Log in first with az login if you're not using Cloud Shell

az account list
```

## <a name="next-steps"></a>Дополнительная информация

- Узнайте больше об [интерфейсах управления для различных клиентов](../concepts/cross-tenant-management-experience.md).
- [Просматривайте клиентов и управляйте ими](view-manage-customers.md), перейдя в раздел **Мои клиенты** на портале Azure.
