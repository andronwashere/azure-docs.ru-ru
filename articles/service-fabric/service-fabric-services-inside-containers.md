---
title: Контейнеризация служб Azure Service Fabric в Windows
description: Узнайте, как контейнеризовать службы Service Fabric Reliable Services и Reliable Actors в Windows.
services: service-fabric
documentationcenter: .net
author: aljo-microsoft
manager: anmolah
editor: roroutra
ms.assetid: 0b41efb3-4063-4600-89f5-b077ea81fa3a
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 5/23/2018
ms.author: anmola
ms.openlocfilehash: 1210b34590484379ae487ad1b87e76a433e4582a
ms.sourcegitcommit: 6a42dd4b746f3e6de69f7ad0107cc7ad654e39ae
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/07/2019
ms.locfileid: "67621818"
---
# <a name="containerize-your-service-fabric-reliable-services-and-reliable-actors-on-windows"></a>Контейнеризация служб Service Fabric Reliable Services и Reliable Actors в Windows

Service Fabric поддерживает контейнеризацию микрослужб Service Fabric (службы на основе Reliable Services и Reliable Actors). Дополнительные сведения см. в статье [Service Fabric и контейнеры](service-fabric-containers-overview.md).

Из этого документа вы узнаете, как обеспечить работу службы в контейнере Windows.

> [!NOTE]
> Сейчас эта функция работает только в Windows. Чтобы запустить контейнеры, кластер должен работать на компьютере под управлением Windows Server 2016 с контейнерами.

## <a name="steps-to-containerize-your-service-fabric-application"></a>Инструкции по контейнеризации приложения Service Fabric

1. Откройте приложение Service Fabric в Visual Studio.

2. Добавьте класс [SFBinaryLoader.cs](https://github.com/Azure/service-fabric-scripts-and-templates/blob/master/code/SFBinaryLoaderForContainers/SFBinaryLoader.cs) в проект. Код в этом классе является вспомогательным приложением для правильной загрузки двоичных файлов среды выполнения Service Fabric в приложение, которое работает в контейнере.

3. Каждый пакет кода, который вы бы хотели контейнеризовать, инициализирует загрузчик на точке входа программы. Добавьте статический конструктор, показанный в следующем фрагменте кода, в файл точки входа программы.

   ```csharp
   namespace MyApplication
   {
      internal static class Program
      {
          static Program()
          {
              SFBinaryLoader.Initialize();
          }

          /// <summary>
          /// This is the entry point of the service host process.
          /// </summary>
          private static void Main()
          {
   ```

4. Выполните сборку проекта и [упакуйте](service-fabric-package-apps.md#Package-App) его. Чтобы выполнить сборку проекта и создать пакет, щелкните правой кнопкой проект приложения в обозревателе решений и выберите команду **Пакет**.

5. Для каждого пакета кода, который нужно контейнеризовать, запустите скрипт PowerShell [CreateDockerPackage.ps1](https://github.com/Azure/service-fabric-scripts-and-templates/blob/master/scripts/CodePackageToDockerPackage/CreateDockerPackage.ps1). Оно используется следующим образом.

    Полная версия .NET
      ```powershell
        $codePackagePath = 'Path to the code package to containerize.'
        $dockerPackageOutputDirectoryPath = 'Output path for the generated docker folder.'
        $applicationExeName = 'Name of the Code package executable.'
        CreateDockerPackage.ps1 -CodePackageDirectoryPath $codePackagePath -DockerPackageOutputDirectoryPath $dockerPackageOutputDirectoryPath -ApplicationExeName $applicationExeName
      ```
    .NET Core
      ```powershell
        $codePackagePath = 'Path to the code package to containerize.'
        $dockerPackageOutputDirectoryPath = 'Output path for the generated docker folder.'
        $dotnetCoreDllName = 'Name of the Code package dotnet Core Dll.'
        CreateDockerPackage.ps1 -CodePackageDirectoryPath $codePackagePath -DockerPackageOutputDirectoryPath $dockerPackageOutputDirectoryPath -DotnetCoreDllName $dotnetCoreDllName
      ```
      Скрипт создает папку с артефактами Docker в папке $dockerPackageOutputDirectoryPath. Измените созданный файл Dockerfile, чтобы выполнить `expose` для всех портов, запустить скрипты установки и выполнить другие действия в соответствии с потребностями.

6. Теперь [выполните сборку](service-fabric-get-started-containers.md#Build-Containers) пакета контейнера Docker и [отправьте](service-fabric-get-started-containers.md#Push-Containers) его в репозиторий.

7. Измените файлы ApplicationManifest.xml и ServiceManifest.xml, чтобы добавить образ контейнера, сведения о репозитории, проверку подлинности реестра и сопоставление портов с узлами. Сведения об изменении манифестов см. в статье [Создание первого контейнера-приложения Service Fabric в Windows](service-fabric-get-started-containers.md). Определение пакета кода в манифесте службы необходимо заменить соответствующим образом контейнера. Обязательно замените тип EntryPoint типом ContainerHost.

   ```xml
   <!-- Code package is your service executable. -->
   <CodePackage Name="Code" Version="1.0.0">
   <EntryPoint>
    <!-- Follow this link for more information about deploying Windows containers to Service Fabric: https://aka.ms/sfguestcontainers -->
    <ContainerHost>
      <ImageName>myregistry.azurecr.io/samples/helloworldapp</ImageName>
    </ContainerHost>
   </EntryPoint>
   <!-- Pass environment variables to your container: -->
   </CodePackage>
   ```

8. Добавьте сопоставление портов с узлами для репликатора и конечной точки службы. Так как оба эти порта назначены в среде выполнения платформой Service Fabric, для ContainerPort устанавливается значение нуль, чтобы использовать порт, назначенный для сопоставления.

   ```xml
   <Policies>
   <ContainerHostPolicies CodePackageRef="Code">
    <PortBinding ContainerPort="0" EndpointRef="ServiceEndpoint"/>
    <PortBinding ContainerPort="0" EndpointRef="ReplicatorEndpoint"/>
   </ContainerHostPolicies>
   </Policies>
   ```

9. Ознакомьтесь с инструкциями по [настройке режима изоляции для контейнера]( https://docs.microsoft.com/azure/service-fabric/service-fabric-get-started-containers#configure-isolation-mode). Windows поддерживает два режима изоляции для контейнеров: режим изоляции процессов и Hyper-V. В указанных ниже фрагментах кода показано, как режим изоляции указывается в файле манифеста приложения.

   ```xml
   <Policies>
   <ContainerHostPolicies CodePackageRef="Code" Isolation="process">
   ...
   </ContainerHostPolicies>
   </Policies>
   ```
   ```xml
   <Policies>
   <ContainerHostPolicies CodePackageRef="Code" Isolation="hyperv">
   ...
   </ContainerHostPolicies>
   </Policies>
   ```

> [!NOTE] 
> По умолчанию Service Fabric приложения имеют доступ к среде выполнения Service Fabric, в виде конечной точки, принимая запросы от приложения. Рекомендуется отключить такой доступ, если приложения размещены ненадежного кода. Дополнительные сведения см. в разделе [рекомендации по обеспечению безопасности в Service Fabric](service-fabric-best-practices-security.md#platform-isolation). Чтобы отключить доступ к среде выполнения Service Fabric, добавьте следующий параметр в разделе "политики" манифеста приложения, соответствующее коду импортированного манифеста службы, следующим образом:
>
```xml
  <Policies>
      <ServiceFabricRuntimeAccessPolicy RemoveServiceFabricRuntimeAccess="true"/>
  </Policies>
```
>

10. Чтобы протестировать это приложение, необходимо развернуть его в кластере под управлением версии 5.7 или более поздней. Для версий среды выполнения 6.1 и ниже необходимо изменить и обновить параметры кластера, чтобы включить эту предварительную версию функции. Выполните инструкции из этой [статьи](service-fabric-cluster-fabric-settings.md), чтобы добавить параметр, показанный далее.
    ```
      {
        "name": "Hosting",
        "parameters": [
          {
            "name": "FabricContainerAppsEnabled",
            "value": "true"
          }
        ]
      }
    ```

11. Затем [разверните](service-fabric-deploy-remove-applications.md) измененный пакет приложения в этом кластере.

Теперь в вашем кластере работает контейнерное приложение Service Fabric.

## <a name="next-steps"></a>Следующие шаги
* Дополнительные сведения о запуске [контейнеров в Service Fabric](service-fabric-get-started-containers.md).
* Дополнительные сведения о [жизненном цикле приложения](service-fabric-application-lifecycle.md) Service Fabric.
