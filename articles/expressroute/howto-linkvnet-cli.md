---
title: 'Связывание виртуальной сети с каналом ExpressRoute — CLI: Azure | Документация Майкрософт'
description: В этой статье показано, как связать виртуальные сети с каналами ExpressRoute с помощью модели развертывания Resource Manager и Azure CLI.
services: expressroute
author: cherylmc
ms.service: expressroute
ms.topic: conceptual
ms.date: 05/21/2019
ms.author: cherylmc
ms.reviewer: anzaman
ms.custom: seodec18
ms.openlocfilehash: d858c83fb6669e5348b4256931e080656be0ebad
ms.sourcegitcommit: 6a42dd4b746f3e6de69f7ad0107cc7ad654e39ae
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/07/2019
ms.locfileid: "67621056"
---
# <a name="connect-a-virtual-network-to-an-expressroute-circuit-using-cli"></a>Подключение виртуальной сети к каналу ExpressRoute с помощью CLI

Эта статья поможет вам связать виртуальные сети с каналами Azure ExpressRoute с помощью CLI. С помощью Azure CLI можно связывать только виртуальные сети, созданные по модели развертывания с помощью Resource Manager. Они могут входить в одну и ту же подписку или в разные подписки. Для подключения виртуальной сети к каналу ExpressRoute можно использовать другие методы, информация о которых приводится в статьях из следующего списка:

> [!div class="op_single_selector"]
> * [портал Azure](expressroute-howto-linkvnet-portal-resource-manager.md)
> * [PowerShell](expressroute-howto-linkvnet-arm.md)
> * [Интерфейс командной строки Azure](howto-linkvnet-cli.md)
> * [Видео — портал Azure](https://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-a-connection-between-your-vpn-gateway-and-expressroute-circuit)
> * [PowerShell (классическая модель)](expressroute-howto-linkvnet-classic.md)
> 

## <a name="configuration-prerequisites"></a>Предварительные требования для настройки

* Требуется последняя версия интерфейса командной строки (CLI). Дополнительные сведения см. в статье [Установка Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli).

* Прежде чем приступить к настройке, необходимо изучить [предварительные требования](expressroute-prerequisites.md), [требования к маршрутизации](expressroute-routing.md) и [рабочие процессы](expressroute-workflows.md).

* Вам потребуется активный канал ExpressRoute. 
  * Следуйте инструкциям, чтобы [создать канал ExpressRoute](howto-circuit-cli.md) и включить его на стороне поставщика услуг подключения. 
  * Убедитесь, что для вашего канала настроен частный пиринг Azure. Инструкции по маршрутизации см. в статье [Настройка маршрутизации](howto-routing-cli.md). 
  * Убедитесь, что настроен частный пиринг Azure. Для создания сквозного подключения потребуется также пиринг BGP между вашей сетью и сетью Майкрософт.
  * Вам необходимо создать и полностью подготовить виртуальную сеть и шлюз виртуальной сети. Следуйте инструкциям по [созданию шлюза виртуальной сети для ExpressRoute](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-cli). Обязательно используйте `--gateway-type ExpressRoute`.

* К стандартному каналу ExpressRoute можно подключить не более 10 виртуальных сетей. Если используется стандартный канал ExpressRoute, все виртуальные сети должны находиться в одном геополитическом регионе. 

* Отдельную виртуальную сеть можно связать не более чем с четырьмя каналами ExpressRoute. Для создания объекта подключения для каждого канала ExpressRoute, к которому вы подключаетесь, используйте процесс, описанный ниже. Каналы ExpressRoute могут быть размещены в той же подписке, в других подписках или и там, и там.

* Если вы включите надстройку ExpressRoute Premium, вы сможете подключить к каналу ExpressRoute больше виртуальных сетей или подключить сеть из другого геополитического региона. Дополнительные сведения о надстройке версии Premium см. в статье [Вопросы и ответы по ExpressRoute](expressroute-faqs.md).

## <a name="connect-a-virtual-network-in-the-same-subscription-to-a-circuit"></a>Подключение к каналу виртуальной сети в той же подписке

Вы можете связать шлюз виртуальной сети с каналом ExpressRoute, используя приведенный ниже пример. Прежде чем выполнять эту команду, убедитесь, что шлюз виртуальной сети существует и готов к связыванию.

```azurecli
az network vpn-connection create --name ERConnection --resource-group ExpressRouteResourceGroup --vnet-gateway1 VNet1GW --express-route-circuit2 MyCircuit
```

## <a name="connect-a-virtual-network-in-a-different-subscription-to-a-circuit"></a>Подключение к каналу виртуальной сети в другой подписке

Канал ExpressRoute может совместно использоваться несколькими подписками. На рисунке ниже схематично показан способ совместного использования каналов ExpressRoute несколькими подписками.

Каждое маленькое облако внутри большого облака представляет подписки, принадлежащие различным подразделениям одной организации. Любое подразделение в организации может использовать свою собственную подписку для развертывания служб. Кроме того, оно может совместно использовать один выделенный канал ExpressRoute для подключения к корпоративной сети. Владельцем канала ExpressRoute может выступать один отдел (в этом примере — отдел ИТ). Другие подписки в организации могут использовать канал ExpressRoute.

> [!NOTE]
> Плата за подключение и использование пропускной способности выделенного канала взимается с владельца канала ExpressRoute. Полоса пропускания распределяется между всеми виртуальными сетями.
> 
> 

![Подключение между подписками](./media/expressroute-howto-linkvnet-classic/cross-subscription.png)

### <a name="administration---circuit-owners-and-circuit-users"></a>Администрирование: владельцы и пользователи канала

Владельцем канала считается уполномоченный опытный пользователь ресурса канала ExpressRoute. Владелец канала может создавать разрешения, которые будут использовать пользователи канала. Пользователями канала считаются владельцы шлюзов виртуальных сетей, не включенных в подписку, к которой относится канал ExpressRoute. Пользователи канала могут использовать разрешения (по одному разрешению на виртуальную сеть).

Владелец канала имеет право в любой момент изменить или отменить эти разрешения. Отмена разрешения приводит к удалению всех связывающих подключений из подписки, для которой был отменен доступ.

### <a name="circuit-owner-operations"></a>Действия владельца канала

**Создание разрешения**

Владелец канала создает разрешение, в результате чего создается ключ, с помощью которого пользователь канала сможет подключить шлюзы виртуальной сети к каналу ExpressRoute. Разрешение действительно только для одного подключения.

В следующем примере показано, как создать разрешение.

```azurecli
az network express-route auth create --circuit-name MyCircuit -g ExpressRouteResourceGroup -n MyAuthorization
```

Ответ содержит ключ и состояние разрешения.

```azurecli
"authorizationKey": "0a7f3020-541f-4b4b-844a-5fb43472e3d7",
"authorizationUseStatus": "Available",
"etag": "W/\"010353d4-8955-4984-807a-585c21a22ae0\"",
"id": "/subscriptions/81ab786c-56eb-4a4d-bb5f-f60329772466/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/MyCircuit/authorizations/MyAuthorization1",
"name": "MyAuthorization1",
"provisioningState": "Succeeded",
"resourceGroup": "ExpressRouteResourceGroup"
```

**Просмотр разрешений**

Владелец канала может просмотреть все разрешения, выданные для определенного канала, выполнив команду из следующего примера.

```azurecli
az network express-route auth list --circuit-name MyCircuit -g ExpressRouteResourceGroup
```

**Добавление разрешений**

Владелец канала может добавлять разрешения с помощью следующей команды.

```azurecli
az network express-route auth create --circuit-name MyCircuit -g ExpressRouteResourceGroup -n MyAuthorization1
```

**Удаление разрешений**

Владелец канала может отменять и удалять разрешения, выданные пользователю, как показано в следующем примере.

```azurecli
az network express-route auth delete --circuit-name MyCircuit -g ExpressRouteResourceGroup -n MyAuthorization1
```

### <a name="circuit-user-operations"></a>Действия пользователя канала

Пользователь канала должен получить идентификатор однорангового узла и ключ разрешения от владельца канала. Ключ разрешения представляет собой идентификатор GUID.

```azurecli
Get-AzExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "MyRG"
```

**Активация разрешения на подключение**

Пользователь канала может активировать разрешение на подключение, выполнив следующую команду.

```azurecli
az network vpn-connection create --name ERConnection --resource-group ExpressRouteResourceGroup --vnet-gateway1 VNet1GW --express-route-circuit2 MyCircuit --authorization-key "^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^"
```

**Освобождение разрешения на подключение**

Разрешение можно освободить, удалив подключение, связывающее канал ExpressRoute и виртуальную сеть.

## <a name="modify-a-virtual-network-connection"></a>Изменение подключения к виртуальной сети
Вы можете изменить определенные свойства подключения к виртуальной сети. 

**Изменение веса подключения**

Виртуальная сеть может подключаться к нескольким каналам ExpressRoute. Одинаковый префикс может быть получен из нескольких каналов ExpressRoute. Чтобы выбрать подключение для отправки трафика, предназначенного для этого префикса, можно изменить значение *RoutingWeight* подключения. Трафик будет отправляться через подключение с самым высоким значением *RoutingWeight*.

```azurecli
az network vpn-connection update --name ERConnection --resource-group ExpressRouteResourceGroup --routing-weight 100
```

Диапазон значений *RoutingWeight*: 0 до 32 000. Значение по умолчанию — 0.

## <a name="configure-expressroute-fastpath"></a>Настройка ExpressRoute FastPath 
Вы можете включить [ExpressRoute FastPath](expressroute-about-virtual-network-gateways.md) Если ваша цепь ExpressRoute находится на [ExpressRoute Direct](expressroute-erdirect-about.md) и шлюз виртуальной newtork максимальной производительностью или ErGw3AZ. FastPath улучшает данных о производительности пути данных, например пакетов в секунду и соединений в секунду между локальной сетью и виртуальной сети. 

> [!NOTE] 
> Если вы уже есть подключение к виртуальной сети, но еще не включили FastPath необходимо удалить подключение виртуальной сети и создать новую. 
> 
>  

```azurecli
az network vpn-connection create --name ERConnection --resource-group ExpressRouteResourceGroup --express-route-gateway-bypass true --vnet-gateway1 VNet1GW --express-route-circuit2 MyCircuit
```


## <a name="next-steps"></a>Следующие шаги

Дополнительные сведения об ExpressRoute см. в статье [Вопросы и ответы по ExpressRoute](expressroute-faqs.md).
