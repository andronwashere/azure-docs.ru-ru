---
title: Настройка пользовательского ответа для брандмауэра веб-приложения на передней дверце Azure
description: Узнайте, как настроить пользовательский код ответа и сообщение, когда брандмауэр веб-приложения (WAF) блокирует запрос.
services: frontdoor
author: KumudD
ms.service: frontdoor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/21/2019
ms.author: kumud
ms.reviewer: tyao
ms.openlocfilehash: 657dc3a43302d16bc403d790bf2c34c2d147dd6c
ms.sourcegitcommit: fa45c2bcd1b32bc8dd54a5dc8bc206d2fe23d5fb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/12/2019
ms.locfileid: "67846373"
---
# <a name="configure-a-custom-response-for-azure-web-application-firewall"></a>Настройка пользовательского ответа для брандмауэра веб-приложения Azure

По умолчанию, когда брандмауэр веб-приложения Azure (WAF) с передней дверцей Azure блокирует запрос из-за сопоставленного правила, он возвращает код состояния 403 с **запросом блокирует** сообщение. В этой статье описывается, как настроить код состояния пользовательского ответа и ответное сообщение, если запрос заблокирован WAF.

## <a name="set-up-your-powershell-environment"></a>Настройка среды PowerShell
В Azure PowerShell доступен набор командлетов, которые используют модель [Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) для управления ресурсами Azure. 

Вы можете установить [Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview) на локальном компьютере и использовать его в любом сеансе PowerShell. Следуя инструкциям на странице, войдите с учетными данными Azure и установите модуль Az PowerShell.

### <a name="connect-to-azure-with-an-interactive-dialog-for-sign-in"></a>Подключение к Azure с помощью интерактивного диалогового окна для входа
```
Connect-AzAccount
Install-Module -Name Az
```
Убедитесь, что у вас установлена текущая версия PowerShellGet. Выполните следующую команду и снова откройте PowerShell.

```
Install-Module PowerShellGet -Force -AllowClobber
``` 
### <a name="install-azfrontdoor-module"></a>Установка модуля Az.FrontDoor 

```
Install-Module -Name Az.FrontDoor
```

## <a name="create-a-resource-group"></a>Создание группы ресурсов

В Azure выделите связанные ресурсы группе ресурсов. В этом примере создается группа ресурсов с помощью команды [New-азресаурцеграуп](/powershell/module/Az.resources/new-Azresourcegroup).

```azurepowershell-interactive
New-AzResourceGroup -Name myResourceGroupWAF
```

## <a name="create-a-new-waf-policy-with-custom-response"></a>Создание новой политики WAF с настраиваемым ответом 

Ниже приведен пример создания новой политики WAF с кодом состояния настраиваемого ответа, равным 405, а сообщение **— заблокировано.** с помощью [New-азфронтдурвафполици](/powershell/module/az.frontdoor/new-azfrontdoorwafpolicy).

```azurepowershell
# WAF policy setting
New-AzFrontDoorWafPolicy `
-Name myWAFPolicy `
-ResourceGroupName myResourceGroupWAF `
-EnabledState enabled `
-Mode Detection `
-CustomBlockResponseStatusCode 405 `
-CustomBlockResponseBody "<html><head><title>You are blocked.</title></head><body></body></html>"
```

Измените пользовательский код ответа или параметры текста ответа существующей политики WAF с помощью [Update-азфронтдурфиреваллполици](/powershell/module/az.frontdoor/Update-AzFrontDoorWafPolicy).

```azurepowershell
# modify WAF response code
Update-AzFrontDoorFireWallPolicy `
-Name myWAFPolicy `
-ResourceGroupName myResourceGroupWAF `
-EnabledState enabled `
-Mode Detection `
-CustomBlockResponseStatusCode 403
```

```azurepowershell
# modify WAF response body
Update-AzFrontDoorFireWallPolicy `
-Name myWAFPolicy `
-ResourceGroupName myResourceGroupWAF `
-CustomBlockResponseBody "<html><head><title> Forbidden</title></head><body></body></html>"
```

## <a name="next-steps"></a>Следующие шаги
- Дополнительные сведения о [передней дверце](front-door-overview.md)