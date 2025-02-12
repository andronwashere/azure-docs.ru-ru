---
title: Клонирование приложения с помощью PowerShell — служба приложений Azure
description: Узнайте, как клонировать приложение службы приложений в новое приложение с помощью PowerShell.
services: app-service\web
documentationcenter: ''
author: ahmedelnably
manager: stefsch
editor: ''
ms.assetid: f9a5cfa1-fbb0-41e6-95d1-75d457347a35
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/14/2016
ms.author: aelnably
ms.custom: seodec18
ms.openlocfilehash: d31a6ee13965aa326ab8a71b5b5435025bc26057
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/09/2019
ms.locfileid: "67705728"
---
# <a name="azure-app-service-app-cloning-using-powershell"></a>Клонирование приложений службы приложений Azure с помощью PowerShell

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

С выходом Microsoft Azure PowerShell версии 1.1.0 был добавлен новый параметр `New-AzWebApp`, позволяющий клонировать существующее приложение службы приложений во вновь созданное приложение, размещенное в том же или в другом регионе. Так пользователи смогут легко и быстро развернуть целый ряд приложений в различных регионах.

Клонирование приложений поддерживается для планов службы приложений Standard, "премиум", "премиум" V2 и Isolated. В новой функции действуют те же ограничения, что и в функции архивации службы приложений (см. статью [Резервное копирование приложений в службе приложений Azure](manage-backup.md)).

## <a name="cloning-an-existing-app"></a>Клонирование существующего приложения
Сценарий. Существует приложение в центрально-южной части США; его содержимое необходимо клонировать в новое приложение в центрально-северной части США. Это можно решить с помощью командлета PowerShell на основе Azure Resource Manager для создания приложения с параметром `-SourceWebApp`.

Зная имя группы ресурсов, содержащей исходное приложение, можно выполнить следующую команду PowerShell для получения данных исходного приложения (в данном случае оно называется `source-webapp`).

```powershell
$srcapp = Get-AzWebApp -ResourceGroupName SourceAzureResourceGroup -Name source-webapp
```

Чтобы создать план службы приложений, можно выполнить команду `New-AzAppServicePlan`, как показано в следующем примере:

```powershell
New-AzAppServicePlan -Location "South Central US" -ResourceGroupName DestinationAzureResourceGroup -Name NewAppServicePlan -Tier Premium
```

С помощью команды `New-AzWebApp` создайте приложение в центрально-северной части США и привяжите его к имеющемуся плану службы приложений уровня "Премиум". Более того, можно использовать в качестве исходного приложения ту же группу ресурсов или определить новую, как показано в следующей команде.

```powershell
$destapp = New-AzWebApp -ResourceGroupName DestinationAzureResourceGroup -Name dest-webapp -Location "North Central US" -AppServicePlan DestinationAppServicePlan -SourceWebApp $srcapp
```

Для клонирования существующего приложения, включая все соответствующие слоты развертывания, пользователю необходимо будет задать параметр `IncludeSourceWebAppSlots`. Следующая команда PowerShell показывает, как использовать этот параметр с командой `New-AzWebApp`:

```powershell
$destapp = New-AzWebApp -ResourceGroupName DestinationAzureResourceGroup -Name dest-webapp -Location "North Central US" -AppServicePlan DestinationAppServicePlan -SourceWebApp $srcapp -IncludeSourceWebAppSlots
```

Для клонирования существующего приложения в том же регионе вам нужно будет создать новую группу ресурсов и новый план службы приложений в том же регионе, а затем клонировать приложение с помощью следующей команды PowerShell.

```powershell
$destapp = New-AzWebApp -ResourceGroupName NewAzureResourceGroup -Name dest-webapp -Location "South Central US" -AppServicePlan NewAppServicePlan -SourceWebApp $srcap
```

## <a name="cloning-an-existing-app-to-an-app-service-environment"></a>Клонирование существующего приложения в среду службы приложений
Сценарий. Существует приложение в центрально-южной части США, содержимое которого необходимо клонировать в новое приложение в имеющуюся среду службы приложений (ASE).

Зная имя группы ресурсов, содержащей исходное приложение, можно выполнить следующую команду PowerShell для получения данных исходного приложения (в данном случае оно называется `source-webapp`).

```powershell
$srcapp = Get-AzWebApp -ResourceGroupName SourceAzureResourceGroup -Name source-webapp
```

Зная имя ASE и имя группы ресурсов, к которой относится ASE, можно создать приложение в существующей ASE, выполнив следующую команду.

```powershell
$destapp = New-AzWebApp -ResourceGroupName DestinationAzureResourceGroup -Name dest-webapp -Location "North Central US" -AppServicePlan DestinationAppServicePlan -ASEName DestinationASE -ASEResourceGroupName DestinationASEResourceGroupName -SourceWebApp $srcapp
```

В связи с обратной совместимостью параметр `Location` является обязательным, однако при создании приложения в ASE он игнорируется. 

## <a name="cloning-an-existing-app-slot"></a>Клонирование существующего слота приложения
Сценарий. Вам необходимо клонировать существующий слот развертывания приложения в новое приложение или в новый слот. Новое приложение может размещаться в том же регионе, что и исходный слот, или в другом.

Зная имя группы ресурсов, содержащей исходное приложение, можно выполнить следующую команду PowerShell для получения данных слота исходного приложения (в данном случае оно называется `source-appslot`), привязанных к `source-app`.

```powershell
$srcappslot = Get-AzWebAppSlot -ResourceGroupName SourceAzureResourceGroup -Name source-app -Slot source-appslot
```

Следующая команда демонстрирует создание клона исходного приложения в новом приложении.

```powershell
$destapp = New-AzWebApp -ResourceGroupName DestinationAzureResourceGroup -Name dest-app -Location "North Central US" -AppServicePlan DestinationAppServicePlan -SourceWebApp $srcappslot
```

## <a name="configuring-traffic-manager-while-cloning-an-app"></a>Настройка диспетчера трафика при клонировании приложения
Создание мультирегиональных приложений и настройка в диспетчере трафика Azure маршрутизации трафика ко всем этим приложениям — это важный сценарий, обеспечивающий высокую доступность приложений клиентов. Клонируя существующее приложение, вы можете подключить оба приложения к новому или существующему профилю диспетчера трафика. Обратите внимание, что поддерживается только версия диспетчера трафика на основе Azure Resource Manager.

### <a name="creating-a-new-traffic-manager-profile-while-cloning-an-app"></a>Создание профиля диспетчера трафика при клонировании приложения
Сценарий. Вам необходимо клонировать приложение в другой регион и настроить профиль диспетчера трафика Azure Resource Manager так, чтобы он содержал оба приложения. Следующая команда демонстрирует создание клона исходного приложения в новом приложении с настройкой нового профиля диспетчера трафика.

```powershell
$destapp = New-AzWebApp -ResourceGroupName DestinationAzureResourceGroup -Name dest-webapp -Location "South Central US" -AppServicePlan DestinationAppServicePlan -SourceWebApp $srcapp -TrafficManagerProfileName newTrafficManagerProfile
```

### <a name="adding-new-cloned-app-to-an-existing-traffic-manager-profile"></a>Добавление клонированного приложения в существующий профиль диспетчера трафика
Сценарий. У вас уже есть профиль диспетчера трафика Azure Resource Manager, вам необходимо добавить оба приложения в качестве конечных точек. Для этого нужно собрать идентификатор профиля диспетчера трафика. Кроме того, потребуется идентификатор подписки, имя группы ресурсов и имя имеющегося профиля диспетчера трафика.

```powershell
$TMProfileID = "/subscriptions/<Your subscription ID goes here>/resourceGroups/<Your resource group name goes here>/providers/Microsoft.TrafficManagerProfiles/ExistingTrafficManagerProfileName"
```

Следующая команда показывает, как создать клон исходного приложения в новом приложении и добавить эти приложения в существующий профиль диспетчера трафика, зная идентификатор диспетчера трафика.

```powershell
$destapp = New-AzWebApp -ResourceGroupName <Resource group name> -Name dest-webapp -Location "South Central US" -AppServicePlan DestinationAppServicePlan -SourceWebApp $srcapp -TrafficManagerProfileId $TMProfileID
```

## <a name="current-restrictions"></a>Существующие ограничения
Ниже приведены известные ограничения клонирования приложения:

* Параметры автоматического масштабирования не клонируются
* Параметры расписания резервного копирования не клонируются
* Параметры виртуальных сетей не клонируются
* App Insights не настраивается в целевом приложении автоматически
* Параметры простой авторизации не клонируются
* Расширение Kudu не клонируется
* Правила TiP не клонируются
* Содержимое базы данных не клонируется.
* При клонировании в другую единицу масштабирования исходящие IP-адреса изменяются.
* Недоступно для приложений Linux

### <a name="references"></a>Ссылки
* [Клонирование службы приложений](app-service-web-app-cloning.md)
* [Резервное копирование приложения в службе приложений Azure](manage-backup.md)
* [Предварительная поддержка диспетчера трафика Azure в диспетчере ресурсов Azure](../traffic-manager/traffic-manager-powershell-arm.md)
* [Введение в среду службы приложения](environment/intro.md)
* [Использование Azure PowerShell с диспетчером ресурсов Azure](../azure-resource-manager/manage-resources-powershell.md)

