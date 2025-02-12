---
title: 'Azure Backup: Резервное копирование виртуальных машин Azure с помощью REST API'
description: Управление операциями резервного копирования виртуальных машин Azure с помощью REST API
author: pvrk
manager: shivamg
keywords: REST API; Azure VM backup; Azure VM restore;
ms.service: backup
ms.topic: conceptual
ms.date: 08/03/2018
ms.author: pullabhk
ms.assetid: b80b3a41-87bf-49ca-8ef2-68e43c04c1a3
ms.openlocfilehash: e78c7ca9e5b39beb160aeef96dbbf6bce07613e4
ms.sourcegitcommit: c72ddb56b5657b2adeb3c4608c3d4c56e3421f2c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/24/2019
ms.locfileid: "68466834"
---
# <a name="back-up-an-azure-vm-using-azure-backup-via-rest-api"></a>Резервное копирование виртуальных машин Azure с помощью службы Azure Backup и REST API

В этой статье описано, как управлять резервными копиями виртуальных машин Azure с помощью службы Azure Backup и REST API. Вы можете настроить защиту ранее незащищенных виртуальных машин Azure, активировать резервное копирование по запросу для защищенной виртуальной машины Azure и изменить свойства резервной копии виртуальной машины с помощью REST API, как описано в этой статье.

См. дополнительные сведения о создании [хранилищ](backup-azure-arm-userestapi-createorupdatevault.md) и [политик](backup-azure-arm-userestapi-createorupdatepolicy.md) с помощью REST API.

Предположим, вы хотите защитить виртуальную машину testVM в группе ресурсов testRG в хранилище Служб восстановления testVault, расположенном в группе ресурсов testVaultRG, с помощью политики по умолчанию (с именем DefaultPolicy).

## <a name="configure-backup-for-an-unprotected-azure-vm-using-rest-api"></a>Настройка резервного копирования для незащищенных виртуальных машин Azure с помощью REST API

### <a name="discover-unprotected-azure-vms"></a>Обнаружение незащищенных виртуальных машин Azure

Во-первых, для хранилища нужно обеспечить возможность идентификации виртуальной машины Azure. Это можно сделать с помощью [операции обновления](https://docs.microsoft.com/rest/api/backup/protectioncontainers/refresh). Это асинхронная операция *POST*, которая предоставляет хранилищу самый последний список всех незащищенных виртуальных машин в текущей подписке для кэширования. После кэширования виртуальной машины Службы восстановления получат доступ к виртуальной машине для включения защиты.

```http
POST https://management.azure.com/Subscriptions/{subscriptionId}/resourceGroups/{vaultresourceGroupname}/providers/Microsoft.RecoveryServices/vaults/{vaultName}/backupFabrics/{fabricName}/refreshContainers?api-version=2016-12-01
```

Для URI POST используются параметры `{subscriptionId}`, `{vaultName}`, `{vaultresourceGroupName}`, `{fabricName}`. Значение параметра `{fabricName}` — Azure. В нашем примере значения параметров `{vaultName}` и `{vaultresourceGroupName}` — testVault и testVaultRG. Так как все необходимые параметры указаны в URI, текст запроса не требуется.

```http
POST https://management.azure.com/Subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testVaultRG/providers/Microsoft.RecoveryServices/vaults/testVault/backupFabrics/Azure/refreshContainers?api-version=2016-12-01
```

#### <a name="responses"></a>Responses

Операция обновления — это [асинхронная операция](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-async-operations). Это означает, что такая операция создает другую операцию, которая должна отслеживаться отдельно.

Она возвращает два ответа: 202 (принято), когда создается другая операция, и 200 (ОК), когда эта операция завершается.

|Имя  |Тип  |Описание  |
|---------|---------|---------|
|204 No Content (содержимое отсутствует)     |         |  ОК (без содержимого, которое возвращаются)      |
|202 — принято     |         |     Принято    |

##### <a name="example-responses"></a>Примеры ответов

После отправления запроса *POST* возвращается ответ 202 (принято).

```http
HTTP/1.1 202 Accepted
Pragma: no-cache
Retry-After: 60
X-Content-Type-Options: nosniff
x-ms-request-id: 43cf550d-e463-421c-8922-37e4766db27d
x-ms-client-request-id: 4910609f-bb9b-4c23-8527-eb6fa2d3253f; 4910609f-bb9b-4c23-8527-eb6fa2d3253f
Strict-Transport-Security: max-age=31536000; includeSubDomains
x-ms-ratelimit-remaining-subscription-writes: 1199
x-ms-correlation-request-id: 43cf550d-e463-421c-8922-37e4766db27d
x-ms-routing-request-id: SOUTHINDIA:20180521T105701Z:43cf550d-e463-421c-8922-37e4766db27d
Cache-Control: no-cache
Date: Mon, 21 May 2018 10:57:00 GMT
Location: https://management.azure.com/subscriptions//00000000-0000-0000-0000-000000000000/resourceGroups/testVaultRG/providers/microsoft.recoveryservices/vaults/testVault/backupFabrics/Azure/operationResults/aad204aa-a5cf-4be2-a7db-a224819e5890?api-version=2016-12-01
X-Powered-By: ASP.NET
```

Отследите итоговую операцию, используя заголовок location с помощью простой команды *GET*.

```http
GET https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testVaultRG/providers/microsoft.recoveryservices/vaults/testVault/backupFabrics/Azure/operationResults/aad204aa-a5cf-4be2-a7db-a224819e5890?api-version=2016-12-01
```

После обнаружения всех виртуальных машин Azure команда GET возвращает номер ответа 204 (содержимое отсутствует). Хранилище теперь сможет обнаруживать все виртуальные машины в пределах подписки.

```http
HTTP/1.1 204 NoContent
Pragma: no-cache
X-Content-Type-Options: nosniff
x-ms-request-id: cf6cd73b-9189-4942-a61d-878fcf76b1c1
x-ms-client-request-id: 25bb6345-f9fc-4406-be1a-dc6db0eefafe; 25bb6345-f9fc-4406-be1a-dc6db0eefafe
Strict-Transport-Security: max-age=31536000; includeSubDomains
x-ms-ratelimit-remaining-subscription-reads: 14997
x-ms-correlation-request-id: cf6cd73b-9189-4942-a61d-878fcf76b1c1
x-ms-routing-request-id: SOUTHINDIA:20180521T105825Z:cf6cd73b-9189-4942-a61d-878fcf76b1c1
Cache-Control: no-cache
Date: Mon, 21 May 2018 10:58:25 GMT
X-Powered-By: ASP.NET
```

### <a name="selecting-the-relevant-azure-vm"></a>Выбор нужной виртуальной машины Azure

 Вы можете убедиться, что кэширование выполнено, [отобразив список всех защищаемых элементов](https://docs.microsoft.com/rest/api/backup/backupprotectableitems/list) в пределах подписки. Затем в ответе найдите нужную виртуальную машину. [Ответ этой операции](#example-responses-1) также информирует о том, как Службы восстановления определяют виртуальную машину.  Ознакомившись с этой процедурой, вы можете пропустить этот шаг и перейти непосредственно к [включению защиты](#enabling-protection-for-the-azure-vm).

Это можно сделать с помощью операции *GET*.

```http
GET https://management.azure.com/Subscriptions/{subscriptionId}/resourceGroups/{vaultresourceGroupName}/providers/Microsoft.RecoveryServices/vaults/{vaultName}/backupProtectableItems?api-version=2016-12-01&$filter=backupManagementType eq 'AzureIaasVM'
```

URI *GET* имеет все необходимые параметры. Для этой операции текст запроса также не требуется.

##### <a name="responses-1"></a>Правляют

|Имя  |Тип  |Описание  |
|---------|---------|---------|
|200 ОК     | [WorkloadProtectableItemResourceList](https://docs.microsoft.com/rest/api/backup/backupprotectableitems/list#workloadprotectableitemresourcelist)        |       OK |

##### <a name="example-responses-1"></a>Примеры ответов

После отправки запроса *GET* возвращается ответ 200 (OK).

```http
HTTP/1.1 200 OK
Pragma: no-cache
X-Content-Type-Options: nosniff
x-ms-request-id: 7c2cf56a-e6be-4345-96df-c27ed849fe36
x-ms-client-request-id: 40c8601a-c217-4c68-87da-01db8dac93dd; 40c8601a-c217-4c68-87da-01db8dac93dd
Strict-Transport-Security: max-age=31536000; includeSubDomains
x-ms-ratelimit-remaining-subscription-reads: 14979
x-ms-correlation-request-id: 7c2cf56a-e6be-4345-96df-c27ed849fe36
x-ms-routing-request-id: SOUTHINDIA:20180521T071408Z:7c2cf56a-e6be-4345-96df-c27ed849fe36
Cache-Control: no-cache
Date: Mon, 21 May 2018 07:14:08 GMT
Server: Microsoft-IIS/8.0
X-Powered-By: ASP.NET

{
  "value": [
    {
      "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testVaultRG/providers/microsoft.recoveryservices/vaults/testVault/backupFabrics/Azure/protectionContainers/IaasVMContainer;iaasvmcontainerv2;testRG;testVM/protectableItems/vm;iaasvmcontainerv2;testRG;testVM",
      "name": "iaasvmcontainerv2;testRG;testVM",
      "type": "Microsoft.RecoveryServices/vaults/backupFabrics/protectionContainers/protectableItems",
      "properties": {
        "virtualMachineId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testRG/providers/Microsoft.Compute/virtualMachines/testVM",
        "virtualMachineVersion": "Compute",
        "resourceGroup": "testRG",
        "backupManagementType": "AzureIaasVM",
        "protectableItemType": "Microsoft.Compute/virtualMachines",
        "friendlyName": "testVM",
        "protectionState": "NotProtected"
      }
    },……………..

```

> [!TIP]
> Число значений в ответе *GET* ограничено 200 на страницу. Используйте поле nextLink, чтобы получить URL-адрес для следующего набора ответов.

Ответ содержит список всех незащищенных виртуальных машин Azure, где каждое значение `{value}` содержит все сведения, необходимые для настройки резервного копирования Службами восстановления Azure. При настройке резервного копирования запишите значения полей `{name}` и `{virtualMachineId}` в разделе `{properties}`. Создайте из этих значений полей две переменные, как показано ниже.

- containerName = "iaasvmcontainer;"+`{name}`
- protectedItemName = "vm;"+ `{name}`
- `{virtualMachineId}` будет использоваться позже в [тексте запроса](#example-request-body).

В нашем примере указанные выше значения будут такими:

- containerName = "iaasvmcontainer;iaasvmcontainerv2;testRG;testVM"
- protectedItemName = "vm;iaasvmcontainerv2;testRG;testVM"

### <a name="enabling-protection-for-the-azure-vm"></a>Включение защиты для виртуальной машины Azure

После кэширования и идентификации соответствующей виртуальной машины выберите политику защиты. Дополнительные сведения о существующих политиках в хранилище см. в [списке API политик](https://docs.microsoft.com/rest/api/backup/backuppolicies/list). Ссылаясь на имя политики, выберите [соответствующую политику](https://docs.microsoft.com/rest/api/backup/protectionpolicies/get). См. дополнительные сведения о [создании политик](backup-azure-arm-userestapi-createorupdatepolicy.md). В приведенном ниже примере выбрана политика DefaultPolicy.

Включение защиты — это асинхронная операция *PUT*, которая создает защищенный элемент.

```http
https://management.azure.com/Subscriptions/{subscriptionId}/resourceGroups/{vaultresourceGroupName}/providers/Microsoft.RecoveryServices/vaults/{vaultName}/backupFabrics/{fabricName}/protectionContainers/{containerName}/protectedItems/{protectedItemName}?api-version=2016-12-01
```

Значения параметров `{containerName}` и `{protectedItemName}` соответствуют указанным выше. Значение параметра `{fabricName}` — Azure. В нашем примере мы получим следующее:

```http
PUT https://management.azure.com/Subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testVaultRG/providers/Microsoft.RecoveryServices/vaults/testVault/backupFabrics/Azure/protectionContainers/iaasvmcontainer;iaasvmcontainerv2;testRG;testVM/protectedItems/vm;iaasvmcontainerv2;testRG;testVM?api-version=2016-12-01
```

#### <a name="create-the-request-body"></a>Создание текста запроса

Чтобы создать защищенный элемент, используйте компоненты текста запроса.

|Название  |Тип  |Описание  |
|---------|---------|---------|
|свойства     | AzureIaaSVMProtectedItem        |Свойства ресурса ProtectedItem         |

Полный список определений в тексте запроса и другие сведения см. в [документации по созданию защищенного элемента для REST API](https://docs.microsoft.com/rest/api/backup/protecteditems/createorupdate#request-body).

##### <a name="example-request-body"></a>Примеры текста запроса

Следующий текст запроса определяет свойства, необходимые для создания защищенного элемента.

```json
{
  "properties": {
    "protectedItemType": "Microsoft.Compute/virtualMachines",
    "sourceResourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testRG/providers/Microsoft.Compute/virtualMachines/testVM",
    "policyId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testVaultRG/providers/microsoft.recoveryservices/vaults/testVault/backupPolicies/DefaultPolicy"
  }
}
```

`{sourceResourceId}` и `{virtualMachineId}` — это использованные ранее параметры [ответа со списком защищаемых элементов](#example-responses-1).

#### <a name="responses"></a>Responses

Создание защищенного элемента — это [асинхронная операция](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-async-operations). Это означает, что такая операция создает другую операцию, которая должна отслеживаться отдельно.

Она возвращает два ответа: 202 (принято), когда создается другая операция, и 200 (ОК), когда эта операция завершается.

|Имя  |Тип  |Описание  |
|---------|---------|---------|
|200 ОК     |    [ProtectedItemResource](https://docs.microsoft.com/rest/api/backup/protecteditemoperationresults/get#protecteditemresource)     |  OK       |
|202 — принято     |         |     Принято    |

##### <a name="example-responses"></a>Примеры ответов

После отправки запроса *PUT* для создания или обновления защищенного элемента будет получен первоначальный ответ 202 (принято) с заголовками location или Azure-AsyncOperation.

```http
HTTP/1.1 202 Accepted
Pragma: no-cache
Retry-After: 60
Azure-AsyncOperation: https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testVaultRG/providers/microsoft.recoveryservices/vaults/testVault/backupFabrics/Azure/protectionContainers/iaasvmcontainer;iaasvmcontainerv2;testRG;testVM/protectedItems/vm;testRG;testVM/operationsStatus/a0866047-6fc7-4ac3-ba38-fb0ae8aa550f?api-version=2016-12-01
X-Content-Type-Options: nosniff
x-ms-request-id: db785be0-bb20-4598-bc9f-70c9428b170b
x-ms-client-request-id: e1f94eef-9b2d-45c4-85b8-151e12b07d03; e1f94eef-9b2d-45c4-85b8-151e12b07d03
Strict-Transport-Security: max-age=31536000; includeSubDomains
x-ms-ratelimit-remaining-subscription-writes: 1199
x-ms-correlation-request-id: db785be0-bb20-4598-bc9f-70c9428b170b
x-ms-routing-request-id: SOUTHINDIA:20180521T073907Z:db785be0-bb20-4598-bc9f-70c9428b170b
Cache-Control: no-cache
Date: Mon, 21 May 2018 07:39:06 GMT
Location: https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testVaultRG/providers/microsoft.recoveryservices/vaults/testVault/backupFabrics/Azure/protectionContainers/iaasvmcontainer;iaasvmcontainerv2;testRG;testVM/protectedItems/vm;testRG;testVM/operationResults/a0866047-6fc7-4ac3-ba38-fb0ae8aa550f?api-version=2016-12-01
X-Powered-By: ASP.NET
```

Отследите итоговую операцию, используя заголовки location или Azure-AsyncOperation с помощью простой команды *GET*.

```http
GET https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testVaultRG/providers/microsoft.recoveryservices/vaults/testVault/backupFabrics/Azure/protectionContainers/iaasvmcontainer;iaasvmcontainerv2;testRG;testVM/protectedItems/vm;testRG;testVM/operationsStatus/a0866047-6fc7-4ac3-ba38-fb0ae8aa550f?api-version=2016-12-01
```

Завершенная операция возвращает код 200 (ОК) с содержимым защищенного элемента в тексте ответа.

```json
{
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testVaultRG/providers/microsoft.recoveryservices/vaults/testVault/backupFabrics/Azure/protectionContainers/iaasvmcontainer;iaasvmcontainerv2;testRG;testVM/protectedItems/vm;testRG;testVM",
  "name": "VM;testRG;testVM",
  "type": "Microsoft.RecoveryServices/vaults/backupFabrics/protectionContainers/protectedItems",
  "properties": {
    "friendlyName": "testVM",
    "virtualMachineId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testRG/providers/Microsoft.Compute/virtualMachines/testVM",
    "protectionStatus": "Healthy",
    "protectionState": "IRPending",
    "healthStatus": "Passed",
    "lastBackupStatus": "",
    "lastBackupTime": "2001-01-01T00:00:00Z",
    "protectedItemDataId": "17592691116891",
    "extendedInfo": {
      "recoveryPointCount": 0,
      "policyInconsistent": false
    },
    "protectedItemType": "Microsoft.Compute/virtualMachines",
    "backupManagementType": "AzureIaasVM",
    "workloadType": "VM",
    "containerName": "iaasvmcontainerv2;testRG;testVM",
    "sourceResourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testRG/providers/Microsoft.Compute/virtualMachines/testVM",
    "policyId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testVaultRG/providers/microsoft.recoveryservices/vaults/testVault/backupPolicies/DefaultPolicy",
    "policyName": "DefaultPolicy"
  }
}
```

Это является подтверждением того, что защита виртуальной машины включена, и первая операция резервного копирования будет активирована в соответствии с расписанием политики.

## <a name="trigger-an-on-demand-backup-for-a-protected-azure-vm"></a>Активация резервного копирования по запросу для защищенной виртуальной машины Azure

После настройки виртуальной машины Azure для резервного копирования эта операция будет активирована в соответствии с расписанием политики. Вы можете дождаться первого запланированного резервного копирования или активировать эту операцию по требованию в любое время. Срок хранения для резервных копий по запросу отличается от срока хранения политики резервного копирования. Кроме того, вы можете указать определенные дату и время. Если срок хранения не будет указан, будет использоваться значение по умолчанию — 30 дней со дня активации резервного копирования по запросу.

Активация резервного копирования по запросу — это операция *POST*.

```http
POST https://management.azure.com/Subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.RecoveryServices/vaults/{vaultName}/backupFabrics/{fabricName}/protectionContainers/{containerName}/protectedItems/{protectedItemName}/backup?api-version=2016-12-01
```

Значения параметров `{containerName}` и `{protectedItemName}` соответствуют указанным [выше](#responses-1). Значение параметра `{fabricName}` — Azure. В нашем примере мы получим следующее:

```http
POST https://management.azure.com/Subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testVaultRG/providers/Microsoft.RecoveryServices/vaults/testVault/backupFabrics/Azure/protectionContainers/iaasvmcontainer;iaasvmcontainerv2;testRG;testVM/protectedItems/vm;iaasvmcontainerv2;testRG;testVM/backup?api-version=2016-12-01
```

### <a name="create-the-request-body"></a>Создание текста запроса

Чтобы активировать резервное копирование по запросу, используйте компоненты текста запроса.

|Название  |Тип  |Описание  |
|---------|---------|---------|
|свойства     | [IaaSVMBackupRequest](https://docs.microsoft.com/rest/api/backup/backups/trigger#iaasvmbackuprequest)        |Свойства BackupRequestResource         |

Полный список определений в тексте запроса и другие сведения см. в [документации по активации резервного копирования для защищенных элементов для REST API](https://docs.microsoft.com/rest/api/backup/backups/trigger#request-body).

#### <a name="example-request-body"></a>Примеры текста запроса

Следующий текст запроса определяет свойства, необходимые для активации резервного копирования для защищенного элемента. Если срок хранения не указан, он будет составлять 30 дней с момента активации задания резервного копирования.

```json
{
   "properties": {
    "objectType": "IaasVMBackupRequest",
    "recoveryPointExpiryTimeInUTC": "2018-12-01T02:16:20.3156909Z"
  }
}
```

### <a name="responses"></a>Responses

Активация резервного копирования по запросу — это [асинхронная операция](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-async-operations). Это означает, что такая операция создает другую операцию, которая должна отслеживаться отдельно.

Она возвращает два ответа: 202 (принято), когда создается другая операция, и 200 (ОК), когда эта операция завершается.

|Имя  |Тип  |Описание  |
|---------|---------|---------|
|202 — принято     |         |     Принято    |

##### <a name="example-responses-3"></a>Примеры ответов

После отправки запроса *POST* для выполнения резервного копирования по запросу будет получен первоначальный ответ 202 (принято) с заголовками location или Azure-AsyncOperation.

```http
HTTP/1.1 202 Accepted
Pragma: no-cache
Retry-After: 60
Azure-AsyncOperation: https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testVaultRG/providers/microsoft.recoveryservices/vaults/testVault/backupFabrics/Azure/protectionContainers/iaasvmcontainer;iaasvmcontainerv2;testVaultRG;testVM/protectedItems/vm;testRG;testVM/operationsStatus/b8daecaa-f8f5-44ed-9f18-491a9e9ba01f?api-version=2016-12-01
X-Content-Type-Options: nosniff
x-ms-request-id: 7885ca75-c7c6-43fb-a38c-c0cc437d8810
x-ms-client-request-id: 7df8e874-1d66-4f81-8e91-da2fe054811d; 7df8e874-1d66-4f81-8e91-da2fe054811d
Strict-Transport-Security: max-age=31536000; includeSubDomains
x-ms-ratelimit-remaining-subscription-writes: 1199
x-ms-correlation-request-id: 7885ca75-c7c6-43fb-a38c-c0cc437d8810
x-ms-routing-request-id: SOUTHINDIA:20180521T083541Z:7885ca75-c7c6-43fb-a38c-c0cc437d8810
Cache-Control: no-cache
Date: Mon, 21 May 2018 08:35:41 GMT
Location: https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testVaultRG/providers/microsoft.recoveryservices/vaults/testVault/backupFabrics/Azure/protectionContainers/iaasvmcontainer;iaasvmcontainerv2;testVaultRG;testVM/protectedItems/vm;testRG;testVM/operationResults/b8daecaa-f8f5-44ed-9f18-491a9e9ba01f?api-version=2016-12-01
X-Powered-By: ASP.NET
```

Отследите итоговую операцию, используя заголовки location или Azure-AsyncOperation с помощью простой команды *GET*.

```http
GET https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testVaultRG/providers/microsoft.recoveryservices/vaults/testVault/backupFabrics/Azure/protectionContainers/iaasvmcontainer;iaasvmcontainerv2;testRG;testVM/protectedItems/vm;testRG;testVM/operationsStatus/a0866047-6fc7-4ac3-ba38-fb0ae8aa550f?api-version=2016-12-01
```

Завершенная операция возвращает код 200 (ОК) с идентификатором результата задания резервного копирования в тексте ответа.

```json
HTTP/1.1 200 OK
Pragma: no-cache
X-Content-Type-Options: nosniff
x-ms-request-id: a8b13524-2c95-445f-b107-920806f385c1
x-ms-client-request-id: 5a63209d-3708-4e69-a75f-9499f4c8977c; 5a63209d-3708-4e69-a75f-9499f4c8977c
Strict-Transport-Security: max-age=31536000; includeSubDomains
x-ms-ratelimit-remaining-subscription-reads: 14995
x-ms-correlation-request-id: a8b13524-2c95-445f-b107-920806f385c1
x-ms-routing-request-id: SOUTHINDIA:20180521T083723Z:a8b13524-2c95-445f-b107-920806f385c1
Cache-Control: no-cache
Date: Mon, 21 May 2018 08:37:22 GMT
Server: Microsoft-IIS/8.0
X-Powered-By: ASP.NET

{
  "id": "00000000-0000-0000-0000-000000000000",
  "name": "00000000-0000-0000-0000-000000000000",
  "status": "Succeeded",
  "startTime": "2018-05-21T08:35:40.9488967Z",
  "endTime": "2018-05-21T08:35:40.9488967Z",
  "properties": {
    "objectType": "OperationStatusJobExtendedInfo",
    "jobId": "7ddead57-bcb9-4269-ac31-6a1b57588700"
  }
}
```

Так как задание резервного копирования — это длительная операция, оно должно отслеживаться, как описано в [документации по мониторингу заданий для REST API](backup-azure-arm-userestapi-managejobs.md#tracking-the-job).

## <a name="modify-the-backup-configuration-for-a-protected-azure-vm"></a>Изменение параметров резервного копирования для защищенной виртуальной машины Azure

### <a name="changing-the-policy-of-protection"></a>Изменение политики защиты

Чтобы изменить политику, с помощью которой обеспечивается защита виртуальной машины, можно использовать тот же формат, что при [включении защиты](#enabling-protection-for-the-azure-vm). Просто укажите идентификатор политики в [тексте запроса](#example-request-body) и отправьте запрос. Пример: Чтобы изменить политику testVM с "DefaultPolicy" на "Продполици", укажите идентификатор "Продполици" в тексте запроса.

```http
{
  "properties": {
    "protectedItemType": "Microsoft.Compute/virtualMachines",
    "sourceResourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testRG/providers/Microsoft.Compute/virtualMachines/testVM",
    "policyId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testVaultRG/providers/microsoft.recoveryservices/vaults/testVault/backupPolicies/ProdPolicy"
  }
}
```

Как уже упоминалось, ответ будет в таком же формате, как и при [включении защиты](#responses-2).

### <a name="stop-protection-but-retain-existing-data"></a>Снятие защиты с сохранением существующих данных

Чтобы снять защиту с защищенной виртуальной машины и сохранить резервную копию данных, удалите политику из текста запроса и отправьте запрос. После удаления связи с политикой операции резервного копирования больше не будут активироваться и новые точки восстановления не будут создаваться.

```http
{
  "properties": {
    "protectedItemType": "Microsoft.Compute/virtualMachines",
    "sourceResourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testRG/providers/Microsoft.Compute/virtualMachines/testVM",
    "policyId": ""
  }
}
```

Ответ будет в том же формате, что и при [активации резервного копирования по запросу](#example-responses-3). Итоговое задание должно отслеживаться, как описано в [документации по мониторингу заданий для REST API](backup-azure-arm-userestapi-managejobs.md#tracking-the-job).

### <a name="stop-protection-and-delete-data"></a>Снятие защиты и удаление данных

Чтобы снять защиту с защищенной виртуальной машины и удалить резервную копию данных, выполните [операцию удаления](https://docs.microsoft.com/rest/api/backup/protecteditems/delete).

Остановка защиты и удаление данных осуществляется с помощью операции *DELETE*.

```http
DELETE https://management.azure.com/Subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.RecoveryServices/vaults/{vaultName}/backupFabrics/{fabricName}/protectionContainers/{containerName}/protectedItems/{protectedItemName}?api-version=2016-12-01
```

Значения параметров `{containerName}` и `{protectedItemName}` соответствуют указанным [выше](#responses-1). Значение параметра `{fabricName}` — Azure. В нашем примере мы получим следующее:

```http
DELETE https://management.azure.com//Subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testVaultRG/providers/Microsoft.RecoveryServices/vaults/testVault/backupFabrics/Azure/protectionContainers/iaasvmcontainer;iaasvmcontainerv2;testRG;testVM/protectedItems/vm;iaasvmcontainerv2;testRG;testVM?api-version=2016-12-01
```

### <a name="responses-2"></a>Правляют

*DELETE* — это [асинхронная операция](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-async-operations). Это означает, что такая операция создает другую операцию, которая должна отслеживаться отдельно.

Она возвращает два ответа: 202 (принято) при создании другой операции, а затем 204 (неполное содержимое) после завершения этой операции.

|Имя  |Тип  |Описание  |
|---------|---------|---------|
|204 NoContent (содержимое отсутствует)     |         |  NoContent       |
|202 — принято     |         |     Принято    |

## <a name="next-steps"></a>Следующие шаги

[Восстановление данных из резервной копии виртуальной машины Azure](backup-azure-arm-userestapi-restoreazurevms.md)

Дополнительные сведения о REST API Azure Backup см. в следующих документах:

- [Recovery Services](/rest/api/recoveryservices/) (Службы восстановления)
- [Начало работы с Azure REST API](/rest/api/azure/)
