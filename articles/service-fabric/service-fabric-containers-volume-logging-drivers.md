---
title: Драйвер тома службы файлов Azure для Service Fabric (предварительная версия) | Документация Майкрософт
description: Service Fabric поддерживает использование службы файлов Azure для резервного копирования томов в контейнере. Сейчас этот компонент доступен в предварительной версии.
services: service-fabric
documentationcenter: other
author: aljo-microsoft
manager: chackdan
editor: ''
ms.assetid: ab49c4b9-74a8-4907-b75b-8d2ee84c6d90
ms.service: service-fabric
ms.devlang: other
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 6/10/2018
ms.author: aljo, subramar
ms.openlocfilehash: 09ee729fea952665350aa25c21cdb3d5823b899f
ms.sourcegitcommit: bafb70af41ad1326adf3b7f8db50493e20a64926
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/25/2019
ms.locfileid: "68489892"
---
# <a name="service-fabric-azure-files-volume-driver-preview"></a>Драйвер тома службы файлов Azure для Service Fabric (предварительная версия)
Подключаемый модуль тома службы файлов Azure — это [подключаемый модуль тома Docker](https://docs.docker.com/engine/extend/plugins_volume/), предоставляющий тома [службы файлов Azure](https:///azure/storage/files/storage-files-introduction) для контейнеров Docker. Этот подключаемый модуль тома Docker упакован в виде приложения Service Fabric, которое можно развернуть в кластерах Service Fabric. Его целью является предоставление томов на основе файлов Azure для других Service Fabric приложений-контейнеров, развернутых в кластере.

> [!NOTE]
> Версия подключаемого модуля тома файлов Azure версии 6.5.516.9494 — это предварительная версия, доступная в этом документе. Как и любой предварительный выпуск, эта версия **не** предназначена для рабочих сред.
>

## <a name="prerequisites"></a>предварительные требования
* Версия подключаемого модуля тома службы файлов Azure для Windows работает только в операционных системах [Windows Server версии 1709](https:///windows-server/get-started/whats-new-in-windows-server-1709), [Windows 10 версии 1709](https://docs.microsoft.com/windows/whats-new/whats-new-windows-10-version-1709) или более поздних версий.

* Версия подключаемого модуля тома службы файлов Azure для Linux работает во всех версиях операционной системы, поддерживаемых Service Fabric.

* Подключаемый модуль тома службы файлов Azure работает с Service Fabric версии 6.2 и новее.

* Следуйте инструкциям в [документации по службе файлов Azure](https:///azure/storage/files/storage-how-to-create-file-share), чтобы создать файловый ресурс для контейнерного приложения Service Fabric, который будет использован в качестве тома.

* Вам потребуется установить [PowerShell с модулем Service Fabric](https:///azure/service-fabric/service-fabric-get-started) или [SFCTL](https://docs.microsoft.com/azure/service-fabric/service-fabric-cli).

* Если вы используете контейнеры Hyper-V, необходимо добавить следующие фрагменты в раздел ClusterManifest (локальный кластер) или fabricSettings в шаблоне Azure Resource Manager (кластер Azure) или ClusterConfig. JSON (изолированный кластер).

В файле ClusterManifest в раздел Hosting нужно добавить приведенный ниже код. В этом примере имя тома — **сфазурефиле** , а порт, который он прослушивает в кластере, — **19100**. Замените их правильными значениями для кластера.

``` xml 
<Section Name="Hosting">
  <Parameter Name="VolumePluginPorts" Value="sfazurefile:19100" />
</Section>
```

В разделе fabricSettings шаблона Azure Resource Manager (для развертываний Azure) или ClusterConfig. JSON (для автономных развертываний) необходимо добавить следующий фрагмент кода. Опять же, замените имя тома и значения порта своим собственным.

```json
"fabricSettings": [
  {
    "name": "Hosting",
    "parameters": [
      {
          "name": "VolumePluginPorts",
          "value": "sfazurefile:19100"
      }
    ]
  }
]
```


## <a name="deploy-the-service-fabric-azure-files-application"></a>Развертывание приложения Service Fabric для службы файлов Azure

### <a name="using-azure-resource-manager-via-the-provided-powershell-script-recommended"></a>Использование Azure Resource Manager с помощью предоставленного скрипта PowerShell (рекомендуется)

Если кластер основан на Azure, мы рекомендуем развертывать приложения на нем, используя модель ресурсов приложения Azure Resource Manager, чтобы упростить использование и перейти к модели обслуживания инфраструктуры как кода. Такой подход исключает необходимость отслеживания версии приложения для драйвера тома файлов Azure. Он также позволяет поддерживать отдельные шаблоны Azure Resource Manager для каждой поддерживаемой ОС. В сценарии предполагается, что вы развертываете последнюю версию приложения службы файлов Azure и принимаете параметры для типа ОС, идентификатора подписки кластера и группы ресурсов. Скрипт можно скачать на [сайте загрузки Service Fabric](https://sfazfilevd.blob.core.windows.net/sfazfilevd/DeployAzureFilesVolumeDriver.zip). Обратите внимание, что этот параметр автоматически задает ListenPort, который является портом, на котором подключаемый модуль тома файлов Azure прослушивает запросы от управляющей программы DOCKER до 19100. Его можно изменить, добавив параметр с именем "listenPort". Убедитесь, что порт не конфликтует с любым другим портом, используемым кластером или вашими приложениями.
 

Команда развертывания Azure Resource Manager для Windows:
```powershell
.\DeployAzureFilesVolumeDriver.ps1 -subscriptionId [subscriptionId] -resourceGroupName [resourceGroupName] -clusterName [clusterName] -windows
```

Команда развертывания Azure Resource Manager для Linux:
```powershell
.\DeployAzureFilesVolumeDriver.ps1 -subscriptionId [subscriptionId] -resourceGroupName [resourceGroupName] -clusterName [clusterName] -linux
```

После успешного выполнения скрипта можно перейти к [разделу Настройка приложения.](https:////azure/service-fabric/service-fabric-containers-volume-logging-drivers#configure-your-applications-to-use-the-volume)


### <a name="manual-deployment-for-standalone-clusters"></a>Ручное развертывание автономных кластеров

Приложение Service Fabric, которое предоставляет тома для контейнеров, можно скачать с [сайта Service Fabric скачивания](https://sfazfilevd.blob.core.windows.net/sfazfilevd/AzureFilesVolumePlugin.6.5.516.9494.zip). Это приложение можно развернуть в кластере с помощью [PowerShell](https:///azure/service-fabric/service-fabric-deploy-remove-applications), [интерфейса командной строки](https://docs.microsoft.com/azure/service-fabric/service-fabric-application-lifecycle-sfctl) или [интерфейсов API FabricClient](https://docs.microsoft.com/azure/service-fabric/service-fabric-deploy-remove-applications-fabricclient).

1. С помощью командной строки перейдите в корневой каталог скачанного пакета приложения.

    ```powershell
    cd .\AzureFilesVolume\
    ```

    ```bash
    cd ~/AzureFilesVolume
    ```

2. Затем скопируйте пакет приложения в хранилище образов с соответствующими значениями для [Аппликатионпаккажепас] и [ImageStoreConnectionString]:

    ```powershell
    Copy-ServiceFabricApplicationPackage -ApplicationPackagePath [ApplicationPackagePath] -ImageStoreConnectionString [ImageStoreConnectionString] -ApplicationPackagePathInImageStore AzureFilesVolumePlugin
    ```

    ```bash
    sfctl cluster select --endpoint https://testcluster.westus.cloudapp.azure.com:19080 --pem test.pem --no-verify
    sfctl application upload --path [ApplicationPackagePath] --show-progress
    ```

3. регистрация типа приложения;

    ```powershell
    Register-ServiceFabricApplicationType -ApplicationPathInImageStore AzureFilesVolumePlugin
    ```

    ```bash
    sfctl application provision --application-type-build-path [ApplicationPackagePath]
    ```

4. Создайте приложение, обращая особое внимание на значение параметра приложения **ListenPort** . Это значение представляет собой порт, на котором подключаемый модуль тома файлов Azure прослушивает запросы от управляющей программы DOCKER. Убедитесь, что порт, предоставленный приложению, соответствует Волумеплугинпортс в ClusterManifest и не конфликтует с любым другим портом, используемым кластером или вашими приложениями.

    ```powershell
    New-ServiceFabricApplication -ApplicationName fabric:/AzureFilesVolumePluginApp -ApplicationTypeName AzureFilesVolumePluginType -ApplicationTypeVersion 6.5.516.9494  -ApplicationParameter @{ListenPort='19100'}
    ```

    ```bash
    sfctl application create --app-name fabric:/AzureFilesVolumePluginApp --app-type AzureFilesVolumePluginType --app-version 6.5.516.9494 --parameter '{"ListenPort":"19100"}'
    ```

> [!NOTE]
> 
> Windows Server 2016 Datacenter не поддерживает сопоставление подключений SMB с контейнерами ([поддерживается только в Windows Server версии 1709](https:///virtualization/windowscontainers/manage-containers/container-storage)). Это ограничение предотвращает сопоставление сетевых томов и использование драйверов томов службы файлов Azure в версиях, предшествующих версии 1709.

#### <a name="deploy-the-application-on-a-local-development-cluster"></a>Развертывание приложения в локальном кластере разработки
Выполните шаги 1-3 из [приведенного выше.](https:////azure/service-fabric/service-fabric-containers-volume-logging-drivers#manual-deployment-for-standalone-clusters)

 Число экземпляров службы по умолчанию для приложения подключаемого модуля тома службы файлов Azure равно –1. Это означает, что экземпляр службы развернут на каждом узле в кластере. Тем не менее при развертывании приложения подключаемого модуля тома службы файлов Azure в локальном кластере разработки нужно указать число экземпляров, равное 1. Это можно сделать с помощью параметра приложения **InstanceCount**. Поэтому команда для создания приложения подключаемого модуля тома файлов Azure в локальном кластере разработки:

```powershell
New-ServiceFabricApplication -ApplicationName fabric:/AzureFilesVolumePluginApp -ApplicationTypeName AzureFilesVolumePluginType -ApplicationTypeVersion 6.5.516.9494 -ApplicationParameter @{ListenPort='19100';InstanceCount='1'}
```

```bash
sfctl application create --app-name fabric:/AzureFilesVolumePluginApp --app-type AzureFilesVolumePluginType --app-version 6.5.516.9494 --parameter '{"ListenPort": "19100","InstanceCount": "1"}'
```

## <a name="configure-your-applications-to-use-the-volume"></a>Настройка приложений для использования тома
В следующем фрагменте кода показано, как можно указать том на основе файлов Azure в файле манифеста приложения. Отдельного внимания заслуживает тег **Volume**.

```xml
?xml version="1.0" encoding="UTF-8"?>
<ApplicationManifest ApplicationTypeName="WinNodeJsApp" ApplicationTypeVersion="1.0" xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance">
    <Description>Calculator Application</Description>
    <Parameters>
      <Parameter Name="ServiceInstanceCount" DefaultValue="3"></Parameter>
      <Parameter Name="MyCpuShares" DefaultValue="3"></Parameter>
      <Parameter Name="MyStorageVar" DefaultValue="c:\tmp"></Parameter>
    </Parameters>
    <ServiceManifestImport>
        <ServiceManifestRef ServiceManifestName="NodeServicePackage" ServiceManifestVersion="1.0"/>
     <Policies>
       <ContainerHostPolicies CodePackageRef="NodeService.Code" Isolation="hyperv">
            <PortBinding ContainerPort="8905" EndpointRef="Endpoint1"/>
            <RepositoryCredentials PasswordEncrypted="false" Password="****" AccountName="test"/>
            <Volume Source="azfiles" Destination="c:\VolumeTest\Data" Driver="sfazurefile">
                <DriverOption Name="shareName" Value="" />
                <DriverOption Name="storageAccountName" Value="" />
                <DriverOption Name="storageAccountKey" Value="" />
                <DriverOption Name="storageAccountFQDN" Value="" />
            </Volume>
       </ContainerHostPolicies>
   </Policies>
    </ServiceManifestImport>
    <ServiceTemplates>
        <StatelessService ServiceTypeName="StatelessNodeService" InstanceCount="5">
            <SingletonPartition></SingletonPartition>
        </StatelessService>
    </ServiceTemplates>
</ApplicationManifest>
```

Имя драйвера для подключаемого модуля тома службы файлов Azure — **sfazurefile**. Это значение задается для атрибута **Driver** элемента тега **тома** в манифесте приложения.

В теге **Volume** в приведенном выше фрагменте подключаемый модуль тома файлов Azure требует наличия следующих атрибутов:
- **Source**. Это имя тома. Для тома можно выбрать любое имя.
- **Назначение** . Этот атрибут представляет собой расположение, в котором сопоставляется том в работающем контейнере. Таким образом, конечной папкой не может быть существующее в контейнере расположение.

Как показано в элементах **DriverOption** в приведенном выше фрагменте кода, подключаемый модуль тома службы файлов Azure поддерживает следующие параметры драйвера.
- **shareName**. Имя файлового ресурса службы файлов Azure, предоставляющего том для контейнера.
- **storageAccountName**. Имя учетной записи хранения Azure, которая содержит файловый ресурс службы файлов Azure.
- **storageAccountKey**. Ключ доступа учетной записи хранения Azure, которая содержит файловый ресурс службы файлов Azure.
- **storageAccountFQDN** — доменное имя, связанное с учетной записью хранения. Если параметр storageAccountFQDN не указан, доменное имя будет сформировано с использованием суффикса по умолчанию (.file.core.windows.net) с именем storageAccountName.  

    ```xml
    - Example1: 
        <DriverOption Name="shareName" Value="myshare1" />
        <DriverOption Name="storageAccountName" Value="myaccount1" />
        <DriverOption Name="storageAccountKey" Value="mykey1" />
        <!-- storageAccountFQDN will be "myaccount1.file.core.windows.net" -->
    - Example2: 
        <DriverOption Name="shareName" Value="myshare2" />
        <DriverOption Name="storageAccountName" Value="myaccount2" />
        <DriverOption Name="storageAccountKey" Value="mykey2" />
        <DriverOption Name="storageAccountFQDN" Value="myaccount2.file.core.chinacloudapi.cn" />
    ```

## <a name="using-your-own-volume-or-logging-driver"></a>Использование драйвера собственного тома или ведения журнала
Service Fabric дает возможность использовать драйверы пользовательских [томов](https://docs.docker.com/engine/extend/plugins_volume/) и драйверы [ведения журналов](https://docs.docker.com/engine/admin/logging/overview/). Если в кластере не установлен драйвер тома или ведения журнала Docker, его можно установить вручную с помощью протоколов RDP и SSH. Используя эти протоколы, можно выполнить установку с помощью [скрипта запуска масштабируемого набора виртуальных машин](https://azure.microsoft.com/resources/templates/201-vmss-custom-script-windows/) или [скрипта SetupEntryPoint](https:///azure/service-fabric/service-fabric-application-model).

Пример скрипта установки [драйвера тома Docker для Azure](https://docs.docker.com/docker-for-azure/persistent-data-volumes/):

```bash
docker plugin install --alias azure --grant-all-permissions docker4x/cloudstor:17.09.0-ce-azure1  \
    CLOUD_PLATFORM=AZURE \
    AZURE_STORAGE_ACCOUNT="[MY-STORAGE-ACCOUNT-NAME]" \
    AZURE_STORAGE_ACCOUNT_KEY="[MY-STORAGE-ACCOUNT-KEY]" \
    DEBUG=1
```

Чтобы использовать установленный драйвер тома или ведения журнала в своих приложениях, потребуется указать соответствующие значения в элементах **Volume** и **LogConfig** в разделе  **ContainerHostPolicies** в манифесте приложения.

```xml
<ContainerHostPolicies CodePackageRef="NodeService.Code" Isolation="hyperv">
    <PortBinding ContainerPort="8905" EndpointRef="Endpoint1"/>
    <RepositoryCredentials PasswordEncrypted="false" Password="****" AccountName="test"/>
    <LogConfig Driver="[YOUR_LOG_DRIVER]" >
        <DriverOption Name="test" Value="vale"/>
    </LogConfig>
    <Volume Source="c:\workspace" Destination="c:\testmountlocation1" IsReadOnly="false"></Volume>
    <Volume Source="[MyStorageVar]" Destination="c:\testmountlocation2" IsReadOnly="true"> </Volume>
    <Volume Source="myvolume1" Destination="c:\testmountlocation2" Driver="[YOUR_VOLUME_DRIVER]" IsReadOnly="true">
        <DriverOption Name="[name]" Value="[value]"/>
    </Volume>
</ContainerHostPolicies>
```

При указании подключаемого модуля тома Service Fabric автоматически создает том, используя заданные параметры. Тег **Source** в элементе **Volume** — это имя тома, а тег **Driver** — подключаемый модуль драйвера тома. Тег **Destination** указывает на расположение, к которому подключается исходная папка (**Source**) в работающем контейнере. Таким образом, конечной папкой не может быть существующее в контейнере расположение. Параметры можно указать с помощью тега **DriverOption** следующим образом:

```xml
<Volume Source="myvolume1" Destination="c:\testmountlocation4" Driver="azure" IsReadOnly="true">
           <DriverOption Name="share" Value="models"/>
</Volume>
```

Параметры приложения поддерживаются для томов, как показано в предыдущем фрагменте кода манифеста (пример использования можно найти по тексту `MyStorageVar`).

Если задан драйвер ведения журналов Docker, необходимо развернуть агенты (или контейнеры) для обработки журналов в кластере. Чтобы указать параметры драйвера ведения журналов можно использовать тег **DriverOption**.

## <a name="next-steps"></a>Следующие шаги
* Примеры контейнеров, включая драйвер тома, приведены в разделе [Примеры контейнеров Service Fabric](https://github.com/Azure-Samples/service-fabric-containers).
* Развертывание контейнеров в кластере Service Fabric описывается в разделе [Развертывание контейнера в Service Fabric](service-fabric-deploy-container.md).
