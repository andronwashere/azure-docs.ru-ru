---
title: Создание и изменение канала ExpressRoute с помощью PowerShell в Azure | Документация Майкрософт
description: Создание, подготовка, проверка, обновление, удаление и отзыв канала ExpressRoute.
services: expressroute
author: cherylmc
ms.service: expressroute
ms.topic: article
ms.date: 02/20/2019
ms.author: cherylmc
ms.reviewer: ganesr
ms.custom: seodec18
ms.openlocfilehash: 2ff22a3c0087ff7e24517f792ba9abacbae76e3a
ms.sourcegitcommit: fa45c2bcd1b32bc8dd54a5dc8bc206d2fe23d5fb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/12/2019
ms.locfileid: "67846643"
---
# <a name="create-and-modify-an-expressroute-circuit-using-powershell"></a>Создание и изменение канала ExpressRoute с помощью PowerShell
> [!div class="op_single_selector"]
> * [портал Azure](expressroute-howto-circuit-portal-resource-manager.md)
> * [PowerShell](expressroute-howto-circuit-arm.md)
> * [Интерфейс командной строки Azure](howto-circuit-cli.md)
> * [Шаблон Azure Resource Manager](expressroute-howto-circuit-resource-manager-template.md)
> * [Видео — портал Azure](https://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-an-expressroute-circuit)
> * [PowerShell (классическая модель)](expressroute-howto-circuit-classic.md)
>

В этой статье описывается, как создать канал ExpressRoute, используя командлеты PowerShell и модель развертывания с помощью Azure Resource Manager. Вы также сможете проверять состояние каналов, обновлять, удалять или отзывать их.

## <a name="before-you-begin"></a>Перед началом работы

Изучите [предварительные требования](expressroute-prerequisites.md) и [рабочие процессы](expressroute-workflows.md), прежде чем приступить к настройке.

### <a name="working-with-azure-powershell"></a>Работа с Azure PowerShell

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

[!INCLUDE [expressroute-cloudshell](../../includes/expressroute-cloudshell-powershell-about.md)]

## <a name="create"></a>Создание и подготовка канала ExpressRoute
### <a name="1-sign-in-to-your-azure-account-and-select-your-subscription"></a>1. Войдите в учетную запись Azure и выберите подписку.

[!INCLUDE [sign in](../../includes/expressroute-cloud-shell-connect.md)]

### <a name="2-get-the-list-of-supported-providers-locations-and-bandwidths"></a>2. Получение списка поддерживаемых поставщиков, расположений и значений пропускной способности
Перед созданием канала ExpressRoute потребуется список поддерживаемых поставщиков услуг подключения, расположений и вариантов пропускной способности.

Командлет PowerShell **Get-азекспрессраутесервицепровидер** возвращает эти сведения, которые будут использоваться в последующих шагах:

```azurepowershell-interactive
Get-AzExpressRouteServiceProvider
```

Проверьте, указан ли в списке поставщик услуг подключения. Запишите следующие сведения, которые потребуются позднее при создании канала:

* Имя
* PeeringLocations
* BandwidthsOffered

Теперь все готово к созданию канала ExpressRoute.

### <a name="3-create-an-expressroute-circuit"></a>3. Создание канала ExpressRoute
Перед созданием канала ExpressRoute необходимо создать группу ресурсов (если вы этого еще не сделали) с помощью следующей команды:

```azurepowershell-interactive
New-AzResourceGroup -Name "ExpressRouteResourceGroup" -Location "West US"
```

В приведенном ниже примере показано, как создать канал ExpressRoute со скоростью 200 Мбит/с каналом через Equinix в Кремниевой долине. Если вы используете другой поставщик и другие параметры, подставьте в запрос соответствующие данные. Для запроса нового ключа службы воспользуйтесь следующим примером:

```azurepowershell-interactive
New-AzExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup" -Location "West US" -SkuTier Standard -SkuFamily MeteredData -ServiceProviderName "Equinix" -PeeringLocation "Silicon Valley" -BandwidthInMbps 200
```

Убедитесь, что указаны правильный уровень SKU и семейство SKU:

* Уровень SKU определяет, какая надстройка включена — ExpressRoute Standard или ExpressRoute Premium. Укажите *Standard*, чтобы получить SKU Standard, или *Premium*, чтобы получить надстройку Premium.
* Семейство SKU определяет тип выставления счетов. Выберите *Metereddata* для тарифного плана с оплатой за трафик или *Unlimiteddata* для безлимитного тарифного плана. Тип выставления счетов можно изменить с *Metereddata* на *Unlimiteddata* но не наоборот. *Unlimiteddata* для *Metereddata*

> [!IMPORTANT]
> Выставление счетов за использование ExpressRoute начинается после получения ключа службы. Обязательно выполните эту операцию, как только поставщик услуг подключения будет готов предоставить канал.
>
>

Ответ будет содержать ключ службы. Подробное описание всех параметров можно получить, выполнив следующую команду.

```azurepowershell-interactive
get-help New-AzExpressRouteCircuit -detailed
```


### <a name="4-list-all-expressroute-circuits"></a>4. Получение списка всех каналов ExpressRoute
Чтобы получить список всех созданных каналов ExpressRoute, выполните команду **Get-азекспрессраутеЦиркуит** :

```azurepowershell-interactive
Get-AzExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"
```

Ответ выглядит примерно так:

    Name                             : ExpressRouteARMCircuit
    ResourceGroupName                : ExpressRouteResourceGroup
    Location                         : westus
    Id                               : /subscriptions/***************************/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/ExpressRouteARMCircuit
    Etag                             : W/"################################"
    ProvisioningState                : Succeeded
    Sku                              : {
                                         "Name": "Standard_MeteredData",
                                         "Tier": "Standard",
                                         "Family": "MeteredData"
                                          }
    CircuitProvisioningState          : Enabled
    ServiceProviderProvisioningState  : NotProvisioned
    ServiceProviderNotes              :
    ServiceProviderProperties         : {
                                          "ServiceProviderName": "Equinix",
                                          "PeeringLocation": "Silicon Valley",
                                          "BandwidthInMbps": 200
                                        }
    ServiceKey                        : **************************************
    Peerings                          : []

Вы можете получить эти сведения в любое время с помощью командлета `Get-AzExpressRouteCircuit` . Если командлет вызывается без параметров, выводится список всех каналов. Ваш ключ службы показан в поле *ServiceKey*:

```azurepowershell-interactive
Get-AzExpressRouteCircuit
```


Ответ выглядит примерно так:

    Name                             : ExpressRouteARMCircuit
    ResourceGroupName                : ExpressRouteResourceGroup
    Location                         : westus
    Id                               : /subscriptions/***************************/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/ExpressRouteARMCircuit
    Etag                             : W/"################################"
    ProvisioningState                : Succeeded
    Sku                              : {
                                         "Name": "Standard_MeteredData",
                                         "Tier": "Standard",
                                         "Family": "MeteredData"
                                          }
    CircuitProvisioningState         : Enabled
    ServiceProviderProvisioningState : NotProvisioned
    ServiceProviderNotes             :
    ServiceProviderProperties        : {
                                         "ServiceProviderName": "Equinix",
                                         "PeeringLocation": "Silicon Valley",
                                         "BandwidthInMbps": 200
                                          }
    ServiceKey                       : **************************************
    Peerings                         : []


### <a name="5-send-the-service-key-to-your-connectivity-provider-for-provisioning"></a>5. Отправка ключа службы поставщику услуг подключения для подготовки
Параметр *ServiceProviderProvisioningState* предоставляет сведения о текущем состоянии подготовки на стороне поставщика услуг. Параметр Status предоставляет состояние на стороне Майкрософт. Дополнительные сведения о состояниях подготовки канала см. в разделе о [рабочих процессах](expressroute-workflows.md#expressroute-circuit-provisioning-states).

Вновь созданный канал ExpressRoute будет имеет следующее состояние:

    ServiceProviderProvisioningState : NotProvisioned
    CircuitProvisioningState         : Enabled



Когда поставщик услуг подключения находится в процессе его включения, канал переходит в следующее состояние:

    ServiceProviderProvisioningState : Provisioning
    Status                           : Enabled

Для того чтобы канал ExpressRoute можно было использовать, он должен находиться в следующем состоянии:

    ServiceProviderProvisioningState : Provisioned
    CircuitProvisioningState         : Enabled

### <a name="6-periodically-check-the-status-and-the-state-of-the-circuit-key"></a>6. Периодическая проверка состояния и статуса ключа канала
Проверка состояния и состояния ключа канала позволит вам узнать, когда поставщик активирует ваш канал. После настройки канала значение параметра *ServiceProviderProvisioningState* изменится на *Provisioned*, как показано в примере ниже.

```azurepowershell-interactive
Get-AzExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"
```


Ответ выглядит примерно так:

    Name                             : ExpressRouteARMCircuit
    ResourceGroupName                : ExpressRouteResourceGroup
    Location                         : westus
    Id                               : /subscriptions/***************************/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/ExpressRouteARMCircuit
    Etag                             : W/"################################"
    ProvisioningState                : Succeeded
    Sku                              : {
                                         "Name": "Standard_MeteredData",
                                         "Tier": "Standard",
                                         "Family": "MeteredData"
                                       }
    CircuitProvisioningState         : Enabled
    ServiceProviderProvisioningState : Provisioned
    ServiceProviderNotes             :
    ServiceProviderProperties        : {
                                         "ServiceProviderName": "Equinix",
                                         "PeeringLocation": "Silicon Valley",
                                         "BandwidthInMbps": 200
                                       }
    ServiceKey                       : **************************************
    Peerings                         : []

### <a name="7-create-your-routing-configuration"></a>7. Создание конфигурации маршрутизации
Пошаговые инструкции по созданию и изменению пиринга каналов см. в статье [Создание и изменение маршрутизации для канала ExpressRoute](expressroute-howto-routing-arm.md).

> [!IMPORTANT]
> Эти инструкции распространяются только на каналы от поставщиков, предоставляющих услуги подключения второго уровня. Если ваш поставщик услуг подключения предлагает услуги третьего уровня (обычно это IPVPN, например MPLS), то он возьмет на себя настройку маршрутизации и управление ею.
>
>

### <a name="8-link-a-virtual-network-to-an-expressroute-circuit"></a>8. Связывание виртуальной сети с каналом ExpressRoute
Теперь свяжите виртуальную сеть с каналом ExpressRoute. При работе с моделью развертывания Resource Manager пользуйтесь статьей [Связывание виртуальной сети с каналом ExpressRoute](expressroute-howto-linkvnet-arm.md) .

## <a name="getting-the-status-of-an-expressroute-circuit"></a>Получение состояния канала ExpressRoute
Эти сведения можно получить в любое время с помощью командлета **Get-азекспрессраутеЦиркуит** . Если командлет вызывается без параметров, выводится список всех каналов.

```azurepowershell-interactive
Get-AzExpressRouteCircuit
```


Ответ будет выглядеть примерно так:

    Name                             : ExpressRouteARMCircuit
    ResourceGroupName                : ExpressRouteResourceGroup
    Location                         : westus
    Id                               : /subscriptions/***************************/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/ExpressRouteARMCircuit
    Etag                             : W/"################################"
    ProvisioningState                : Succeeded
    Sku                              : {
                                         "Name": "Standard_MeteredData",
                                         "Tier": "Standard",
                                         "Family": "MeteredData"
                                       }
    CircuitProvisioningState         : Enabled
    ServiceProviderProvisioningState : Provisioned
    ServiceProviderNotes             :
    ServiceProviderProperties        : {
                                            "ServiceProviderName": "Equinix",
                                            "PeeringLocation": "Silicon Valley",
                                            "BandwidthInMbps": 200
                                          }
    ServiceKey                       : **************************************
    Peerings                         : []


Для получения сведений об определенном канале ExpressRoute передайте имя группы ресурсов и имя канала как параметр вызова.

```azurepowershell-interactive
Get-AzExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"
```


Ответ выглядит примерно так:

    Name                             : ExpressRouteARMCircuit
    ResourceGroupName                : ExpressRouteResourceGroup
    Location                         : westus
    Id                               : /subscriptions/***************************/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/ExpressRouteARMCircuit
    Etag                             : W/"################################"
    ProvisioningState                : Succeeded
    Sku                              : {
                                         "Name": "Standard_MeteredData",
                                            "Tier": "Standard",
                                            "Family": "MeteredData"
                                          }
    CircuitProvisioningState         : Enabled
    ServiceProviderProvisioningState : Provisioned
    ServiceProviderNotes             :
    ServiceProviderProperties        : {
                                         "ServiceProviderName": "Equinix",
                                         "PeeringLocation": "Silicon Valley",
                                         "BandwidthInMbps": 200
                                          }
    ServiceKey                       : **************************************
    Peerings                         : []


Подробное описание всех параметров можно получить, выполнив следующую команду.

```azurepowershell-interactive
get-help get-azurededicatedcircuit -detailed
```

## <a name="modify"></a>Изменение канала ExpressRoute
Некоторые свойства канала ExpressRoute можно изменить, не повлияв на подключение.

Вы можете выполнять следующие задачи без простоя:

* включать и отключать надстройку ExpressRoute "Премиум" для канала ExpressRoute;
* увеличивать пропускную способность канала ExpressRoute при условии, что в порту имеется доступная емкость. Снижение уровня пропускной способности канала не поддерживается.
* Перейдите с тарифного плана с оплатой за трафик на безлимитный тарифный план. Переход с безлимитного тарифного плана на тарифный план с оплатой за трафик не поддерживается.
* Параметр *Allow Classic Operations*(Разрешить классические операции) можно включать и отключать.

Дополнительные сведения об ограничениях см. в статье [Вопросы и ответы по ExpressRoute](expressroute-faqs.md).

### <a name="to-enable-the-expressroute-premium-add-on"></a>Включение надстройки ExpressRoute "Премиум"
Вы можете включить надстройку ExpressRoute "Премиум" для существующего канала с помощью следующего фрагмента кода PowerShell:

```azurepowershell-interactive
$ckt = Get-AzExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

$ckt.Sku.Tier = "Premium"
$ckt.sku.Name = "Premium_MeteredData"

Set-AzExpressRouteCircuit -ExpressRouteCircuit $ckt
```

Теперь для канала включены функции надстройки ExpressRoute "Премиум". С момента успешного выполнения команды мы начнем выставлять счета по тарифам надстройки уровня "Премиум".

### <a name="to-disable-the-expressroute-premium-add-on"></a>Отключение надстройки ExpressRoute "Премиум"
> [!IMPORTANT]
> Если использовать больше ресурсов, чем разрешено для канала "Стандартный", операция может завершиться ошибкой.
>
>

Обратите внимание на следующие сведения:

* Прежде чем переходить с канала "Премиум" на "Стандартный", убедитесь, что к каналу привязано менее 10 виртуальных сетей. Если этого не сделать, запрос на обновление завершится ошибкой и мы будем выставлять вам счета по тарифам ценовой категории "Премиум".
* Все связи с виртуальными сетями в других геополитических регионах необходимо разорвать. Если этого не сделать, запрос на обновление завершится ошибкой и мы будем выставлять вам счета по тарифам ценовой категории "Премиум".
* Для частного пиринга таблица маршрутов должна содержать менее 4000 маршрутов. Если в ней больше 4000 маршрутов, сеанс BGP будет сброшен. Его можно будет снова активировать только после того, как количество объявленных префиксов станет меньше 4000.

Вы можете отключить надстройку ExpressRoute "Премиум" для существующего канала с помощью следующего командлета PowerShell:

```azurepowershell-interactive
$ckt = Get-AzExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

$ckt.Sku.Tier = "Standard"
$ckt.sku.Name = "Standard_MeteredData"

Set-AzExpressRouteCircuit -ExpressRouteCircuit $ckt
```

### <a name="to-update-the-expressroute-circuit-bandwidth"></a>Обновление пропускной способности канала ExpressRoute
Параметры пропускной способности, поддерживаемые для вашего поставщика, приведены в статье [Вопросы и ответы по ExpressRoute](expressroute-faqs.md). Можно выбрать любой размер, больший, чем размер существующего канала.

> [!IMPORTANT]
> Может потребоваться заново создать канал ExpressRoute, если существующий порт не обеспечивает достаточную емкость. Канал невозможно обновить, если в его расположении нет доступной дополнительной емкости.
>
> Уменьшить пропускную способность канала ExpressRoute без прерывания его работы нельзя. Для снижения пропускной способности нужно будет отозвать канал ExpressRoute и повторно подготовить новый канал ExpressRoute.
>

Выберите необходимый размер и используйте следующую команду для изменения размера канала:

```azurepowershell-interactive
$ckt = Get-AzExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

$ckt.ServiceProviderProperties.BandwidthInMbps = 1000

Set-AzExpressRouteCircuit -ExpressRouteCircuit $ckt
```


Размер канала будет изменен со стороны Майкрософт. Свяжитесь с поставщиком услуг подключения и попросите обновить конфигурации с его стороны в соответствии с этим изменением. Когда вы сообщите поставщику услуг об изменении размера канала, мы начнем выставлять счета за обновленную пропускную способность.

### <a name="to-move-the-sku-from-metered-to-unlimited"></a>Перевод SKU с измеряемых на неограниченные данные
Изменить SKU канала ExpressRoute можно, используя следующий фрагмент кода PowerShell:

```azurepowershell-interactive
$ckt = Get-AzExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

$ckt.Sku.Family = "UnlimitedData"
$ckt.sku.Name = "Premium_UnlimitedData"

Set-AzExpressRouteCircuit -ExpressRouteCircuit $ckt
```

### <a name="to-control-access-to-the-classic-and-resource-manager-environments"></a>Управление доступом к классической среде и среде диспетчера ресурсов
Инструкции см. в статье [Перемещение каналов ExpressRoute из классической модели развертывания в модель Resource Manager](expressroute-howto-move-arm.md).

## <a name="delete"></a>Отзыв и удаление канала ExpressRoute
Обратите внимание на следующие сведения:

* Связь между ExpressRoute и всеми виртуальными сетями необходимо разорвать. Если операция завершится ошибкой, проверьте, не привязаны ли к каналу какие-либо виртуальные сети.
* Если подготовка поставщика услуг канала ExpressRoute находится в состоянии **Идет подготовка** или **Подготовлено** то свяжитесь с поставщиком услуг, чтобы отозвать канал с его стороны. Мы будем резервировать ресурсы и выставлять вам счета до тех пор, пока поставщик услуг не завершит отзыв канала и не отправит нам соответствующее уведомление.
* Если поставщик услуг отзовет канал (состояние подготовки поставщика услуг изменится на **Не подготовлено**), вы можете удалить такой канал. Это приостанавливает выставление счетов для канала.

Для удаления канала ExpressRoute выполните следующую команду:

```azurepowershell-interactive
Remove-AzExpressRouteCircuit -ResourceGroupName "ExpressRouteResourceGroup" -Name "ExpressRouteARMCircuit"
```

## <a name="next-steps"></a>Следующие шаги

После создания канала обязательно выполните задачи, описанные в следующих статьях:

* [Создание и изменение маршрутизации для канала ExpressRoute](expressroute-howto-routing-arm.md)
* [Связывание виртуальной сети с каналом ExpressRoute](expressroute-howto-linkvnet-arm.md)
