---
title: Управление Azure CDN с помощью PowerShell | Документация Майкрософт
description: Узнайте, как использовать командлеты Azure PowerShell для управления Azure CDN.
services: cdn
documentationcenter: ''
author: mdgattuso
manager: danielgi
editor: ''
ms.assetid: fb6f57a5-6e26-4847-8fd9-b51fb05a79eb
ms.service: azure-cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/13/2018
ms.author: magattus
ms.openlocfilehash: 44274c2a46ce51e51d3d8f16d96a0b50c43a3aa1
ms.sourcegitcommit: ccb9a7b7da48473362266f20950af190ae88c09b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/05/2019
ms.locfileid: "67593695"
---
# <a name="manage-azure-cdn-with-powershell"></a>Управление Azure CDN с помощью PowerShell
PowerShell — это одно из самых гибких средств управления профилями и конечными точками Azure CDN.  PowerShell можно использовать интерактивно или подготовить скрипты для автоматизации задач управления.  В этом руководстве описано несколько распространенных задач по управлению профилями и конечными точками Azure CDN, которые можно выполнять с помощью PowerShell.

## <a name="prerequisites"></a>предварительные требования

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Чтобы использовать PowerShell для управления профилями и конечными точками Azure CDN, прежде всего нужно установить модуль Azure PowerShell.  Сведения об установке Azure PowerShell и подключении к Azure с помощью командлета `Connect-AzAccount` см. в статье [How to install and configure Azure PowerShell](/powershell/azure/overview) (Установка и настройка Azure PowerShell).

> [!IMPORTANT]
> Чтобы выполнять командлеты Azure PowerShell, необходимо сначала войти в систему с помощью `Connect-AzAccount`.
> 
> 

## <a name="listing-the-azure-cdn-cmdlets"></a>Получение списка командлетов Azure CDN
Полный список командлетов Azure CDN можно получить с помощью `Get-Command` .

```text
PS C:\> Get-Command -Module Az.Cdn

CommandType     Name                                               Version    Source
-----------     ----                                               -------    ------
Cmdlet          Get-AzCdnCustomDomain                         2.0.0      Az.Cdn
Cmdlet          Get-AzCdnEndpoint                             2.0.0      Az.Cdn
Cmdlet          Get-AzCdnEndpointNameAvailability             2.0.0      Az.Cdn
Cmdlet          Get-AzCdnOrigin                               2.0.0      Az.Cdn
Cmdlet          Get-AzCdnProfile                              2.0.0      Az.Cdn
Cmdlet          Get-AzCdnProfileSsoUrl                        2.0.0      Az.Cdn
Cmdlet          New-AzCdnCustomDomain                         2.0.0      Az.Cdn
Cmdlet          New-AzCdnEndpoint                             2.0.0      Az.Cdn
Cmdlet          New-AzCdnProfile                              2.0.0      Az.Cdn
Cmdlet          Publish-AzCdnEndpointContent                  2.0.0      Az.Cdn
Cmdlet          Remove-AzCdnCustomDomain                      2.0.0      Az.Cdn
Cmdlet          Remove-AzCdnEndpoint                          2.0.0      Az.Cdn
Cmdlet          Remove-AzCdnProfile                           2.0.0      Az.Cdn
Cmdlet          Set-AzCdnEndpoint                             2.0.0      Az.Cdn
Cmdlet          Set-AzCdnOrigin                               2.0.0      Az.Cdn
Cmdlet          Set-AzCdnProfile                              2.0.0      Az.Cdn
Cmdlet          Start-AzCdnEndpoint                           2.0.0      Az.Cdn
Cmdlet          Stop-AzCdnEndpoint                            2.0.0      Az.Cdn
Cmdlet          Test-AzCdnCustomDomain                        2.0.0      Az.Cdn
Cmdlet          Unpublish-AzCdnEndpointContent                2.0.0      Az.Cdn
```

## <a name="getting-help"></a>Получение справки
Справку по любому из этих командлетов можно получить с помощью `Get-Help` .  `Get-Help` описывает назначение и синтаксис командлетов, а для некоторых из них приводит примеры.

```text
PS C:\> Get-Help Get-AzCdnProfile

NAME
    Get-AzCdnProfile

SYNOPSIS
    Gets an Azure CDN profile.


SYNTAX
    Get-AzCdnProfile [-ProfileName <String>] [-ResourceGroupName <String>] [-InformationAction
    <ActionPreference>] [-InformationVariable <String>] [<CommonParameters>]


DESCRIPTION
    Gets an Azure CDN profile and all related information.


RELATED LINKS

REMARKS
    To see the examples, type: "get-help Get-AzCdnProfile -examples".
    For more information, type: "get-help Get-AzCdnProfile -detailed".
    For technical information, type: "get-help Get-AzCdnProfile -full".

```

## <a name="listing-existing-azure-cdn-profiles"></a>Получение списка существующих профилей Azure CDN
Командлет `Get-AzCdnProfile` , запущенный без параметров, возвращает все существующие CDN профили.

```powershell
Get-AzCdnProfile
```

Этот результат можно передавать в командлеты для перечисления.

```powershell
# Output the name of all profiles on this subscription.
Get-AzCdnProfile | ForEach-Object { Write-Host $_.Name }

# Return only **Azure CDN from Verizon** profiles.
Get-AzCdnProfile | Where-Object { $_.Sku.Name -eq "Standard_Verizon" }
```

Также можно получить отдельный профиль, указав имя профиля и группу ресурсов.

```powershell
Get-AzCdnProfile -ProfileName CdnDemo -ResourceGroupName CdnDemoRG
```

> [!TIP]
> Несколько профилей CDN могут иметь одинаковые имена, если они находятся в разных группах ресурсов.  Исключив параметр `ResourceGroupName` , вы получите все профили с указанным именем.
> 
> 

## <a name="listing-existing-cdn-endpoints"></a>Получение списка существующих конечных точек CDN
`Get-AzCdnEndpoint` позволяет получить отдельную конечную точку или все конечные точки в профиле.  

```powershell
# Get a single endpoint.
Get-AzCdnEndpoint -ProfileName CdnDemo -ResourceGroupName CdnDemoRG -EndpointName cdndocdemo

# Get all of the endpoints on a given profile. 
Get-AzCdnEndpoint -ProfileName CdnDemo -ResourceGroupName CdnDemoRG

# Return all of the endpoints on all of the profiles.
Get-AzCdnProfile | Get-AzCdnEndpoint

# Return all of the endpoints in this subscription that are currently running.
Get-AzCdnProfile | Get-AzCdnEndpoint | Where-Object { $_.ResourceState -eq "Running" }
```

## <a name="creating-cdn-profiles-and-endpoints"></a>Создание конечных точек и профилей CDN
Для создания конечных точек и профилей CDN используются командлеты `New-AzCdnProfile` и `New-AzCdnEndpoint`. Поддерживаются такие типы номера SKU:
- Standard_Verizon
- Premium_Verizon
- Custom_Verizon
- Standard_Akamai
- Standard_Microsoft
- Standard_ChinaCdn

```powershell
# Create a new profile
New-AzCdnProfile -ProfileName CdnPoshDemo -ResourceGroupName CdnDemoRG -Sku Standard_Akamai -Location "Central US"

# Create a new endpoint
New-AzCdnEndpoint -ProfileName CdnPoshDemo -ResourceGroupName CdnDemoRG -Location "Central US" -EndpointName cdnposhdoc -OriginName "Contoso" -OriginHostName "www.contoso.com"

# Create a new profile and endpoint (same as above) in one line
New-AzCdnProfile -ProfileName CdnPoshDemo -ResourceGroupName CdnDemoRG -Sku Standard_Akamai -Location "Central US" | New-AzCdnEndpoint -EndpointName cdnposhdoc -OriginName "Contoso" -OriginHostName "www.contoso.com"

```

## <a name="checking-endpoint-name-availability"></a>Проверка доступности имени конечной точки
`Get-AzCdnEndpointNameAvailability` возвращает объект с информацией о том, свободно ли указанное имя конечной точки.

```powershell
# Retrieve availability
$availability = Get-AzCdnEndpointNameAvailability -EndpointName "cdnposhdoc"

# If available, write a message to the console.
If($availability.NameAvailable) { Write-Host "Yes, that endpoint name is available." }
Else { Write-Host "No, that endpoint name is not available." }
```

## <a name="adding-a-custom-domain"></a>Добавление пользовательского домена
`New-AzCdnCustomDomain` добавляет пользовательское имя домена для существующей конечной точки.

> [!IMPORTANT]
> Необходимо настроить запись CNAME у вашего поставщика DNS, как описано в статье [Как сопоставить личный домен с конечной точкой сети доставки содержимого (CDN)](cdn-map-content-to-custom-domain.md).  Это сопоставление можно проверить с помощью `Test-AzCdnCustomDomain`до изменения конечной точки.
> 
> 

```powershell
# Get an existing endpoint
$endpoint = Get-AzCdnEndpoint -ProfileName CdnPoshDemo -ResourceGroupName CdnDemoRG -EndpointName cdnposhdoc

# Check the mapping
$result = Test-AzCdnCustomDomain -CdnEndpoint $endpoint -CustomDomainHostName "cdn.contoso.com"

# Create the custom domain on the endpoint
If($result.CustomDomainValidated){ New-AzCdnCustomDomain -CustomDomainName Contoso -HostName "cdn.contoso.com" -CdnEndpoint $endpoint }
```

## <a name="modifying-an-endpoint"></a>Изменение конечной точки
`Set-AzCdnEndpoint` изменяет существующую конечную точку.

```powershell
# Get an existing endpoint
$endpoint = Get-AzCdnEndpoint -ProfileName CdnPoshDemo -ResourceGroupName CdnDemoRG -EndpointName cdnposhdoc

# Set up content compression
$endpoint.IsCompressionEnabled = $true
$endpoint.ContentTypesToCompress = "text/javascript","text/css","application/json"

# Save the changed endpoint and apply the changes
Set-AzCdnEndpoint -CdnEndpoint $endpoint
```

## <a name="purgingpre-loading-cdn-assets"></a>Очистка и предварительная загрузка ресурсов CDN
`Unpublish-AzCdnEndpointContent` очищает все кэшированные ресурсы, а `Publish-AzCdnEndpointContent` предварительно загружает ресурсы на всех поддерживаемых конечных точках.

```powershell
# Purge some assets.
Unpublish-AzCdnEndpointContent -ProfileName CdnDemo -ResourceGroupName CdnDemoRG -EndpointName cdndocdemo -PurgeContent "/images/kitten.png","/video/rickroll.mp4"

# Pre-load some assets.
Publish-AzCdnEndpointContent -ProfileName CdnDemo -ResourceGroupName CdnDemoRG -EndpointName cdndocdemo -LoadContent "/images/kitten.png","/video/rickroll.mp4"

# Purge everything in /images/ on all endpoints.
Get-AzCdnProfile | Get-AzCdnEndpoint | Unpublish-AzCdnEndpointContent -PurgeContent "/images/*"
```

## <a name="startingstopping-cdn-endpoints"></a>Запуск и остановка конечных точек CDN
`Start-AzCdnEndpoint` и `Stop-AzCdnEndpoint` позволяют запускать и останавливать отдельные конечные точки или группы конечных точек.

```powershell
# Stop the cdndocdemo endpoint
Stop-AzCdnEndpoint -ProfileName CdnDemo -ResourceGroupName CdnDemoRG -EndpointName cdndocdemo

# Stop all endpoints
Get-AzCdnProfile | Get-AzCdnEndpoint | Stop-AzCdnEndpoint

# Start all endpoints
Get-AzCdnProfile | Get-AzCdnEndpoint | Start-AzCdnEndpoint
```

## <a name="deleting-cdn-resources"></a>Удаление ресурсов CDN
`Remove-AzCdnProfile` и `Remove-AzCdnEndpoint` позволяют удалять профили и конечные точки.

```powershell
# Remove a single endpoint
Remove-AzCdnEndpoint -ProfileName CdnPoshDemo -ResourceGroupName CdnDemoRG -EndpointName cdnposhdoc

# Remove all the endpoints on a profile and skip confirmation (-Force)
Get-AzCdnProfile -ProfileName CdnPoshDemo -ResourceGroupName CdnDemoRG | Get-AzCdnEndpoint | Remove-AzCdnEndpoint -Force

# Remove a single profile
Remove-AzCdnProfile -ProfileName CdnPoshDemo -ResourceGroupName CdnDemoRG
```

## <a name="next-steps"></a>Следующие шаги
Узнайте об автоматизации Azure CDN с помощью [.NET](cdn-app-dev-net.md) или [Node.js](cdn-app-dev-node.md).

Сведения о функциях CDN см. в статье [Общие сведения о сети доставки содержимого (CDN) Azure](cdn-overview.md).

