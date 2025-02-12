---
title: Рекомендации по инфраструктуре как коду Azure Service Fabric | Документация Майкрософт
description: Рекомендации по управлению Service Fabric по модели "инфраструктура как код".
services: service-fabric
documentationcenter: .net
author: peterpogorski
manager: chackdan
editor: ''
ms.assetid: 19ca51e8-69b9-4952-b4b5-4bf04cded217
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 01/23/2019
ms.author: pepogors
ms.openlocfilehash: ae1cd0912733116dce1b550dd937cc9fc5f8737b
ms.sourcegitcommit: 4b647be06d677151eb9db7dccc2bd7a8379e5871
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/19/2019
ms.locfileid: "68359771"
---
# <a name="infrastructure-as-code"></a>Инфраструктура как код

В рамках рабочего сценария создайте кластеры Azure Service Fabric с помощью шаблонов Resource Manager. Шаблоны Resource Manager обеспечивают более высокий уровень контроля над свойствами ресурсов и согласованную модель ресурсов.

Примеры шаблонов Resource Manager для Windows и Linux доступны в [образцах Azure на сайте GitHub](https://github.com/Azure-Samples/service-fabric-cluster-templates). Их можно использовать в качестве отправной точки для создания шаблона кластера. Скачайте файлы `azuredeploy.json` и `azuredeploy.parameters.json` и измените их в соответствии со своими требованиями.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Чтобы развернуть шаблоны `azuredeploy.json` и `azuredeploy.parameters.json`, скачанные ранее, используйте следующие команды Azure CLI:

```azurecli
ResourceGroupName="sfclustergroup"
Location="westus"

az group create --name $ResourceGroupName --location $Location 
az group deployment create --name $ResourceGroupName  --template-file azuredeploy.json --parameters @azuredeploy.parameters.json
```

Создание ресурса с помощью PowerShell

```powershell
$ResourceGroupName="sfclustergroup"
$Location="westus"
$Template="azuredeploy.json"
$Parameters="azuredeploy.parameters.json"

New-AzResourceGroup -Name $ResourceGroupName -Location $Location
New-AzResourceGroupDeployment -Name $ResourceGroupName -TemplateFile $Template -TemplateParameterFile $Parameters
```

## <a name="azure-service-fabric-resources"></a>Ресурсы Azure Service Fabric

Вы можете развертывать приложения и службы в кластере Service Fabric с помощью Azure Resource Manager. Дополнительные сведения см. в статье [Управление приложениями и службами как ресурсами Azure Resource Manager](https://docs.microsoft.com/azure/service-fabric/service-fabric-application-arm-resource). Ниже приведены рекомендуемые ресурсы приложения Service Fabric, которые следует добавить в список ресурсов шаблона Resource Manager.

```json
{
    "apiVersion": "2017-07-01-preview",
    "type": "Microsoft.ServiceFabric/clusters/applicationTypes",
    "name": "[concat(parameters('clusterName'), '/', parameters('applicationTypeName'))]",
    "location": "[variables('clusterLocation')]",
},
{
    "apiVersion": "2017-07-01-preview",
    "type": "Microsoft.ServiceFabric/clusters/applicationTypes/versions",
    "name": "[concat(parameters('clusterName'), '/', parameters('applicationTypeName'), '/', parameters('applicationTypeVersion'))]",
    "location": "[variables('clusterLocation')]",
},
{
    "apiVersion": "2017-07-01-preview",
    "type": "Microsoft.ServiceFabric/clusters/applications",
    "name": "[concat(parameters('clusterName'), '/', parameters('applicationName'))]",
    "location": "[variables('clusterLocation')]",
},
{
    "apiVersion": "2017-07-01-preview",
    "type": "Microsoft.ServiceFabric/clusters/applications/services",
    "name": "[concat(parameters('clusterName'), '/', parameters('applicationName'), '/', parameters('serviceName'))]",
    "location": "[variables('clusterLocation')]"
}
```

Чтобы развернуть приложение с помощью Azure Resource Manager, сначала необходимо [создать пакет приложения Service Fabric sfpkg](https://docs.microsoft.com/azure/service-fabric/service-fabric-package-apps#create-an-sfpkg). В следующем сценарии Python приведен пример создания sfpkg:

```python
# Create SFPKG that needs to be uploaded to Azure Storage Blob Container
microservices_sfpkg = zipfile.ZipFile(
    self.microservices_app_package_name, 'w', zipfile.ZIP_DEFLATED)
package_length = len(self.microservices_app_package_path)

for root, dirs, files in os.walk(self.microservices_app_package_path):
    root_folder = root[package_length:]
    for file in files:
        microservices_sfpkg.write(os.path.join(
            root, file), os.path.join(root_folder, file))

microservices_sfpkg.close()
```

## <a name="azure-virtual-machine-operating-system-automatic-upgrade-configuration"></a>Конфигурация автоматического обновления операционной системы виртуальной машины Azure 
Обновление виртуальных машин — это операция, инициированная пользователем, поэтому рекомендуется использовать [масштабируемый набор виртуальных машин автоматическое обновление операционной системы](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-automatic-upgrade) для Azure Service Fabric кластеры управления обновлениями узла. Приложение для управления исправлениями — это альтернативное решение, которое предназначено для размещения за пределами Azure, хотя ВЕХ можно использовать в Azure, и при этом издержки на размещение ВЕХ в Azure являются распространенной причиной для автоматического обновления операционной системы виртуальной машины. Over ВЕХ. Ниже приведены свойства шаблона "вычисление масштабируемого набора виртуальных машин" диспетчер ресурсов для включения автоматического обновления ОС.

```json
"upgradePolicy": {
   "mode": "Automatic",
   "automaticOSUpgradePolicy": {
        "enableAutomaticOSUpgrade": true,
        "disableAutomaticRollback": false
    }
},
```
При использовании автоматических обновлений ОС с Service Fabric новый образ ОС помещается в один домен обновления за раз, чтобы обеспечить высокий уровень доступности служб, работающих в Service Fabric. Для использования автоматических обновлений ОС в Service Fabric ваш кластер должен быть настроен на использование уровня надежности Silver или выше.

Убедитесь, что для следующего раздела реестра установлено значение false, чтобы предотвратить запуск несогласованных обновлений на компьютерах размещения Windows: HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\WindowsUpdate\AU.

Ниже приведены свойства шаблона "вычисленный масштабируемый набор виртуальных машин" диспетчер ресурсов, чтобы задать для раздела реестра WindowsUpdate значение false:
```json
"osProfile": {
        "computerNamePrefix": "{vmss-name}",
        "adminUsername": "{your-username}",
        "secrets": [],
        "windowsConfiguration": {
          "provisionVMAgent": true,
          "enableAutomaticUpdates": false
        }
      },
```

## <a name="azure-service-fabric-cluster-upgrade-configuration"></a>Конфигурация обновления кластера Azure Service Fabric
Ниже приведено свойство шаблона диспетчер ресурсов кластера Service Fabric для включения автоматического обновления.
```json
"upgradeMode": "Automatic",
```
Чтобы вручную обновить кластер, скачайте CAB-файл или debное распределение на виртуальную машину кластера, а затем запустите следующую команду PowerShell:
```powershell
Copy-ServiceFabricClusterPackage -Code -CodePackagePath <"local_VM_path_to_msi"> -CodePackagePathInImageStore ServiceFabric.msi -ImageStoreConnectionString "fabric:ImageStore"
Register-ServiceFabricClusterPackage -Code -CodePackagePath "ServiceFabric.msi"
Start-ServiceFabricClusterUpgrade -Code -CodePackageVersion <"msi_code_version">
```

## <a name="next-steps"></a>Следующие шаги

* Создание кластера на основе виртуальных машин или компьютеров под управлением Windows Server: [Создание изолированного кластера под управлением Windows Server](service-fabric-tutorial-create-vnet-and-windows-cluster.md)
* Создание кластера на основе виртуальных машин или компьютеров под управлением Linux: [Создание кластера Service Fabric в Azure с помощью портала Azure](service-fabric-tutorial-create-vnet-and-linux-cluster.md)
* [Сведения о вариантах поддержки Service Fabric](service-fabric-support.md)
