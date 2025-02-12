---
title: Настройка подключения VPN-шлюза "виртуальная сеть — виртуальная сеть" с использованием портала Azure | Документация Майкрософт
description: Создайте подключение VPN-шлюза между виртуальными сетями с использованием Resource Manager и портала Azure.
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: jpconnock
editor: ''
tags: azure-resource-manager
ms.assetid: a7015cfc-764b-46a1-bfac-043d30a275df
ms.service: vpn-gateway
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/03/2018
ms.author: cherylmc
ms.openlocfilehash: 94b32595cf2c884ccfd1362f6c8d03f542aabfc5
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "62128387"
---
# <a name="configure-a-vnet-to-vnet-vpn-gateway-connection-by-using-the-azure-portal"></a>Настройка подключения VPN-шлюза "виртуальная сеть — виртуальная сеть" с использованием портала Azure

Эта статья поможет установить подключение "виртуальная сеть — виртуальная сеть" между виртуальными сетями. Виртуальные сети могут быть из разных подписок и в разных регионах. При подключении виртуальных сетей из разных подписок подписки не обязательно должны быть связаны с одним и тем же клиентом Active Directory. 

![Схема подключения между виртуальными сетями](./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/v2vrmps.png)

Приведенные в этой статье инструкции относятся к модели развертывания с помощью Azure Resource Manager и используют портал Azure. Эту конфигурацию можно создать с помощью другого средства развертывания или модели с помощью параметров, описанных в следующих статьях:

> [!div class="op_single_selector"]
> * [портал Azure](vpn-gateway-howto-vnet-vnet-resource-manager-portal.md)
> * [PowerShell](vpn-gateway-vnet-vnet-rm-ps.md)
> * [Интерфейс командной строки Azure](vpn-gateway-howto-vnet-vnet-cli.md)
> * [Портал Azure (классический)](vpn-gateway-howto-vnet-vnet-portal-classic.md)
> * [Подключение с использованием разных моделей развертывания — портал Azure](vpn-gateway-connect-different-deployment-models-portal.md)
> * [Подключение с использованием разных моделей развертывания — PowerShell](vpn-gateway-connect-different-deployment-models-powershell.md)
>
>


## <a name="about-connecting-vnets"></a>Сведения о подключении виртуальных сетей

В следующих разделах описаны различные способы подключения виртуальных сетей.

### <a name="vnet-to-vnet"></a>Подключение типа "виртуальная сеть — виртуальная сеть"

Чтобы легко подключать виртуальные сети, рекомендуем настроить подключение "виртуальная сеть — виртуальная сеть". Когда вы подключаете виртуальную сеть к другой виртуальной сети с помощью подключения "виртуальная сеть — виртуальная сеть", это похоже на создание подключения IPsec "сеть —сеть" к локальному расположению. В обоих типах соединений применяется VPN-шлюз для создания защищенного туннеля, использующего IPsec/IKE. При обмене данными оба типа подключений работают одинаково. Тем не менее они отличаются способом настройки шлюза локальной сети. 

При создании подключения "виртуальная сеть — виртуальная сеть" диапазон адресов для шлюза локальной сети автоматически создается и распространяется. Если вы обновите диапазон адресов для одной виртуальной сети, другая виртуальная сеть автоматически определит маршрут к обновленному диапазону адресов. Как правило, намного проще и быстрее создать подключение "виртуальная сеть — виртуальная сеть", чем подключение "сеть — сеть".

### <a name="site-to-site-ipsec"></a>Подключение "сеть — сеть" (IPsec)

При работе со сложной конфигурацией сети для виртуальной сети в качестве альтернативы можно использовать [подключение "сеть — сеть"](vpn-gateway-howto-site-to-site-resource-manager-portal.md). Используя подключение "сеть — сеть" (IPsec), вы вручную создаете и настраиваете локальные сетевые шлюзы. Шлюз локальной сети для каждой виртуальной сети воспринимает другую виртуальную сеть как локальный сайт. Это позволит вам указать дополнительный диапазон адресов для шлюза локальной сети, чтобы маршрутизировать трафик. Если диапазон адресов для виртуальной сети изменится, вам потребуется вручную обновить соответствующий шлюз локальной сети.

### <a name="vnet-peering"></a>Пиринговая связь между виртуальными сетями

Вы также можете подключить виртуальные сети с помощью пиринга виртуальной сети. При пиринге виртуальных сетей не используется VPN-шлюз и применяются другие ограничения. Кроме того, [цены на пиринг виртуальных сетей](https://azure.microsoft.com/pricing/details/virtual-network) рассчитываются не так, как [цены на VPN-шлюз "виртуальная сеть — виртуальная сеть"](https://azure.microsoft.com/pricing/details/vpn-gateway). Дополнительную информацию см. в статье [Пиринговая связь между виртуальными сетями](../virtual-network/virtual-network-peering-overview.md).

## <a name="why-create-a-vnet-to-vnet-connection"></a>Зачем создавать подключение "виртуальная сеть — виртуальная сеть"?

Возможно вам потребуется подключить виртуальные сети с помощью подключения "виртуальная сеть — виртуальная сеть" по следующим причинам:

### <a name="cross-region-geo-redundancy-and-geo-presence"></a>Межрегиональная географическая избыточность и географическое присутствие

  * Вы можете настроить собственную георепликацию или синхронизацию с безопасной возможностью подключения без прохождения трафика через конечные точки с выходом в Интернет.
  * С помощью Azure Load Balancer и диспетчера трафика Azure можно настроить высокодоступную рабочую нагрузку с геоизбыточностью в нескольких регионах Azure. Например, можно настроить группы доступности Always On SQL Server в нескольких регионах Azure.

### <a name="regional-multi-tier-applications-with-isolation-or-administrative-boundaries"></a>Региональные многоуровневые приложения с изоляцией или административными границами

  * В одном регионе можно настроить многоуровневые приложения с несколькими виртуальными сетями, которые связаны друг с другом из-за требований к изоляции или административных требований.

Подключение типа "виртуальная сеть — виртуальная сеть" можно комбинировать с многосайтовыми конфигурациями. Эти конфигурации позволяют устанавливать топологии сети, совмещающие распределенные подключения с подключениями между виртуальными сетями, как показано на схеме ниже:

![Сведения о подключениях](./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/aboutconnections.png "Сведения о подключениях")

В этой статье объясняется, как подключить виртуальные сети, используя тип соединения "виртуальная сеть — виртуальная сеть". При выполнении этих шагов в качестве упражнения можно использовать следующие примеры значений параметров. В нашем примере виртуальные сети находятся в одной подписке, но в разных группах ресурсов. Если виртуальные сети связаны с разными подписками, вам не удастся создать подключение на портале. Используйте [PowerShell](vpn-gateway-vnet-vnet-rm-ps.md) или [интерфейс уровня вызова](vpn-gateway-howto-vnet-vnet-cli.md) в качестве альтернативы. Дополнительные сведения о подключениях типа "виртуальная сеть — виртуальная сеть" см. в разделе [Часто задаваемые вопросы о подключениях типа "виртуальная сеть — виртуальная сеть"](#vnet-to-vnet-faq).

### <a name="example-settings"></a>Примеры настроек

**Значения для TestVNet1:**

- **Параметры виртуальной сети**
    - **Имя**. Введите *TestVNet1*.
    - **Адресное пространство**. Введите *10.11.0.0/16*.
    - **Подписка**: Выберите подписку, которую нужно использовать.
    - **Группа ресурсов**. Введите *TestRG1*.
    - **Расположение**. Выберите **Восточная часть США**.
    - **Подсеть**
        - **Имя**. Введите *FrontEnd*.
        - **Диапазон адресов**. Введите *10.11.0.0/24*.
    - **Подсеть шлюза**.
        - **Имя**. *GatewaySubnet* заполняется автоматически.
        - **Диапазон адресов**. Введите *10.11.255.0/27*.
    - **DNS-сервер**. Выберите **Custom** (Пользовательский) и введите IP-адрес вашего DNS-сервера.

- **Параметры шлюза виртуальной сети** 
    - **Имя**. Введите *TestVNet1GW*.
    - **Тип шлюза**. Выберите **VPN**.
    - **Тип VPN.** Выберите **На основе маршрута**.
    - **Номер SKU**. Выберите номер SKU шлюза, который нужно использовать.
    - **Имя общедоступного IP-адреса**. Введите *TestVNet1GWIP*
    - **Connection** 
       - **Имя**. Введите *TestVNet1toTestVNet4*.
       - **Общий ключ**. Введите *abc123*. Общий ключ можно создать самостоятельно. При создании подключения между виртуальными сетями значения должны совпадать.

**Значения для TestVNet4:**

- **Параметры виртуальной сети**
   - **Имя**. Введите *TestVNet4*.
   - **Адресное пространство**. Введите *10.41.0.0/16*.
   - **Подписка**: Выберите подписку, которую нужно использовать.
   - **Группа ресурсов**. Введите *TestRG4*.
   - **Расположение**. Выберите **Западная часть США**.
   - **Подсеть** 
      - **Имя**. Введите *FrontEnd*.
      - **Диапазон адресов**. Введите *10.41.0.0/24*.
   - **Подсеть шлюза** 
      - **Имя**. *GatewaySubnet* заполняется автоматически.
      - **Диапазон адресов**. Введите *10.41.255.0/27*.
   - **DNS-сервер**. Выберите **Custom** (Пользовательский) и введите IP-адрес вашего DNS-сервера.

- **Параметры шлюза виртуальной сети** 
    - **Имя**. Введите *TestVNet4GW*.
    - **Тип шлюза**. Выберите **VPN**.
    - **Тип VPN.** Выберите **На основе маршрута**.
    - **Номер SKU**. Выберите номер SKU шлюза, который нужно использовать.
    - **Имя общедоступного IP-адреса**. Введите *TestVNet4GWIP*.
    - **Connection** 
       - **Имя**. Введите *TestVNet4toTestVNet1*.
       - **Общий ключ**. Введите *abc123*. Общий ключ можно создать самостоятельно. При создании подключения между виртуальными сетями значения должны совпадать.

## <a name="create-and-configure-testvnet1"></a>Создание и настройка TestVNet1
Если у вас уже есть виртуальная сеть, проверьте совместимость параметров со структурой VPN-шлюза. Обратите особое внимание на подсети, которые могут пересекаться с другими сетями. Подключение не будет работать правильно при наличии перекрытых подсетей. После настройки виртуальной сети с правильными параметрами переходите к действиям из раздела "Выбор DNS-сервера".

### <a name="to-create-a-virtual-network"></a>Создание виртуальной сети
[!INCLUDE [vpn-gateway-basic-vnet-rm-portal](../../includes/vpn-gateway-basic-vnet-rm-portal-include.md)]

## <a name="add-additional-address-space-and-create-subnets"></a>Добавление дополнительного адресного пространства и создание подсетей
После создания виртуальной сети в нее можно добавить дополнительное адресное пространство и подсети.

[!INCLUDE [vpn-gateway-additional-address-space](../../includes/vpn-gateway-additional-address-space-include.md)]

## <a name="create-a-gateway-subnet"></a>Создание подсети шлюза
Чтобы создать шлюз виртуальной сети, сначала нужно создать подсеть шлюза. Подсеть шлюза содержит IP-адреса, которые используются шлюзом виртуальной сети. По возможности рекомендуется создать подсеть шлюза с использованием блока CIDR с маской /28 или /27, чтобы обеспечить достаточно IP-адресов для удовлетворения дополнительных будущих требований к конфигурации.

Если вы создаете эту конфигурацию в качестве упражнения, при создании подсети шлюза можно использовать эти [параметры примера](#example-settings).

[!INCLUDE [vpn-gateway-no-nsg](../../includes/vpn-gateway-no-nsg-include.md)]

### <a name="to-create-a-gateway-subnet"></a>Создание подсети шлюза
[!INCLUDE [vpn-gateway-add-gwsubnet-rm-portal](../../includes/vpn-gateway-add-gwsubnet-rm-portal-include.md)]

## <a name="specify-a-dns-server-optional"></a>Выбор DNS-сервера (необязательно)
DNS не требуется при создании подключений "виртуальная сеть — виртуальная сеть". Но если для ресурсов, развернутых в вашей виртуальной сети, требуется разрешение имен, необходимо указать DNS-сервер. Этот параметр позволяет указать DNS-сервер, который вы хотите использовать для разрешения имен в этой виртуальной сети. Это не приводит к созданию DNS-сервера.

[!INCLUDE [vpn-gateway-add-dns-rm-portal](../../includes/vpn-gateway-add-dns-rm-portal-include.md)]

## <a name="create-a-virtual-network-gateway"></a>Создание шлюза виртуальной сети
На этом шаге вы создадите шлюз для своей виртуальной сети. Создание шлюза часто занимает 45 минут и более, в зависимости от выбранного SKU шлюза. Если вы создаете эту конфигурацию в качестве упражнения, можно использовать [пример параметров](#example-settings).

### <a name="to-create-a-virtual-network-gateway"></a>Создание шлюза виртуальной сети
[!INCLUDE [vpn-gateway-add-gw-rm-portal](../../includes/vpn-gateway-add-gw-rm-portal-include.md)]

## <a name="create-and-configure-testvnet4"></a>Создание и настройка TestVNet4
После настройки TestVNet1 создайте TestVNet4. Для этого выполните предыдущие действия, используя значения для TestVNet4. Настройку TestVNet4 можно начинать до завершения создания шлюза виртуальной сети для TestVNet1. При использовании собственных значений убедитесь, что адресные пространства не перекрываются с другими виртуальными сетями, к которым нужно подключиться.

## <a name="configure-the-testvnet1-gateway-connection"></a>Настройка подключения TestVNet1 к шлюзу
После создания шлюзов виртуальной сети для TestVNet1 и TestVNet4 между ними можно создать подключение. В этом разделе мы создадим подключение между VNet1 и VNet4. Эти действия выполняются только для виртуальных сетей в одной подписке. Если виртуальные сети связаны с разными подписками, для подключения необходимо использовать [PowerShell](vpn-gateway-vnet-vnet-rm-ps.md). Но если виртуальные сети находятся в разных группах ресурсов в одной подписке, их можно подключить с помощью портала.

1. На портале Azure выберите **Все ресурсы**, введите *шлюз виртуальной сети* в поле поиска, а затем перейдите к шлюзу виртуальной сети для виртуальной сети. Например, **TestVNet1GW**. Щелкните его, чтобы открыть страницу **шлюза виртуальной сети**.

   ![Страница "Подключения"](./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/1to4connect2.png "Connections page")
2. В разделе **Параметры** выберите **Подключения**и выберите **Добавить**, чтобы открыть страницу **Добавление подключения**.

   ![Добавление подключения](./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/add.png "Добавление подключения")
3. На странице **Добавление подключения** укажите значения для подключения:

   - **Имя**. Введите имя для подключения. Например, *TestVNet1toTestVNet4*.

   - **Тип подключения**. Выберите **Виртуальная сеть — виртуальная сеть** из раскрывающегося списка.

   - **Шлюз первой виртуальной сети**. Значение поля заполняется автоматически, так как это подключение создается с указанного шлюза виртуальной сети.

   - **Шлюз второй виртуальной сети**. Поле является шлюзом виртуальной сети, к которому нужно создать подключение. Щелкните **Выберите другой шлюз виртуальной сети**, чтобы открыть страницу **Выбор шлюза виртуальной сети**.

     - Просмотрите список шлюзов виртуальной сети на этой странице. Обратите внимание, что отображаются только те шлюзы виртуальной сети, которые относятся к вашей подписке. Для подключения к шлюзу виртуальной сети, который не относится к вашей подписке, используйте [PowerShell](vpn-gateway-vnet-vnet-rm-ps.md).

     - Выберите шлюз виртуальной сети, к которому нужно подключиться.

     - **Общий ключ (PSK)** . В этом поле введите общий ключ для подключения. Этот ключ можно сгенерировать или создать самостоятельно. В подключении типа "сеть — сеть" один и тот же ключ используется и для локального устройства, и для подключения шлюза виртуальной сети. То же самое и здесь, но вместо подключения к VPN-устройству выполняется подключение к другому шлюзу виртуальной сети.
    
4. Нажмите кнопку **ОК** , чтобы сохранить изменения.

## <a name="configure-the-testvnet4-gateway-connection"></a>Настройка подключения TestVNet4 к шлюзу
Далее создайте подключение между TestVNet4 и TestVNet1. На портале найдите шлюз виртуальной сети, связанный с TestVNet4. Выполните шаги из предыдущего раздела, заменив значения с TestVNet4 на TestVNet1 для создания подключения. Используйте тот же общий ключ.

## <a name="verify-your-connections"></a>Проверка подключений

Найдите шлюз виртуальной сети на портале Azure. На странице **шлюза виртуальной сети** выберите **Подключения**, чтобы просмотреть страницу **Подключения** для шлюза виртуальной сети. Когда подключение будет установлено, для параметра **Состояние** значения изменятся на **Успешно** и **Подключено**. Выберите подключение, чтобы открыть страницу **Essentials** и просмотреть дополнительные сведения.

![Успешно](./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/connected.png "Успешно")

Когда начнется передача данных, появятся значения для параметров **Входящие данные** и **Исходящие данные**.

![Основные компоненты](./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/essentials.png "Основные компоненты")

## <a name="add-additional-connections"></a>Добавление дополнительных подключений

Если нужно добавить дополнительные подключения, перейдите к шлюзу виртуальной сети, от которого нужно создать подключение, а затем щелкните **Подключения**. Вы можете создать другое подключение "виртуальная сеть — виртуальная сеть" или подключение "сеть — сеть" (IPsec) к локальному расположению. Укажите **тип подключения** в соответствии с типом создаваемого подключения. Прежде чем создавать дополнительные подключения, убедитесь, что диапазон адреса для виртуальной сети не пересекается с другими диапазонами адресов, к которым вы планируете подключаться. Инструкции по созданию подключения "сеть — сеть" см. в статье [Создание подключения типа "сеть — сеть" на портале Azure](vpn-gateway-howto-site-to-site-resource-manager-portal.md).

## <a name="vnet-to-vnet-faq"></a>Часто задаваемые вопросы о подключениях VNet-VNet
Дополнительные сведения см. в ответах на часто задаваемые вопросы о подключениях типа "виртуальная сеть — виртуальная сеть".

[!INCLUDE [vpn-gateway-vnet-vnet-faq](../../includes/vpn-gateway-faq-vnet-vnet-include.md)]

## <a name="next-steps"></a>Дальнейшие действия

Сведения об ограничении сетевого трафика к ресурсам в виртуальной сети см. в статье [Безопасность сети](../virtual-network/security-overview.md).

Сведения о том, как Azure маршрутизирует трафик между средой Azure, локальной средой и интернет-ресурсами, см. в статье [Маршрутизация трафика в виртуальной сети](../virtual-network/virtual-networks-udr-overview.md).
