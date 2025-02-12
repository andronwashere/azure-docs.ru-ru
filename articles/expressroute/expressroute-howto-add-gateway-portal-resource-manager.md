---
title: Добавление шлюза в виртуальную сеть Azure для ExpressRoute с помощью портала | Документация Майкрософт
description: В этой статье рассматривается добавление шлюза виртуальной сети в уже созданную виртуальную сеть Resource Manager для ExpressRoute.
services: expressroute
author: cherylmc
ms.service: expressroute
ms.topic: article
ms.date: 12/06/2018
ms.author: cherylmc
ms.custom: seodec18
ms.openlocfilehash: 68376751a3c673b2d89d028312f992aec40d4dee
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "60366018"
---
# <a name="configure-a-virtual-network-gateway-for-expressroute-using-the-azure-portal"></a>Настройка шлюза виртуальной сети для ExpressRoute с помощью портала Azure
> [!div class="op_single_selector"]
> * [Resource Manager — портал Azure](expressroute-howto-add-gateway-portal-resource-manager.md)
> * [Resource Manager — PowerShell](expressroute-howto-add-gateway-resource-manager.md)
> * [Классическая модель: PowerShell](expressroute-howto-add-gateway-classic.md)
> * [Видео — портал Azure](https://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-a-vpn-gateway-for-your-virtual-network)
> 
> 

Эта статья содержит инструкции по добавлению шлюза виртуальной сети для существующей виртуальной сети. В ней описывается добавление, изменение размера и удаление шлюза виртуальной сети для существующей виртуальной сети. Приведенные действия по настройке предназначены для виртуальных сетей, созданных с помощью модели развертывания Resource Manager, которые будут использоваться в конфигурации ExpressRoute. Дополнительные сведения о шлюзах виртуальных сетей и параметрах конфигурации шлюза для ExpressRoute см. в разделе [Сведения о шлюзах виртуальных сетей ExpressRoute](expressroute-about-virtual-network-gateways.md). 


## <a name="before-beginning"></a>Подготовка

Для выполнения этой задачи используйте виртуальную сеть со значениями, приведенными в справочном списке для настройки ниже. Мы используем этот список для работы с примером. Вы можете скопировать список для справки, заменив значения собственными.

**Справочный список для конфигурации**

* Имя виртуальной сети: TestVNet.
* Адресное пространство виртуальной сети: 192.168.0.0/16.
* Имя подсети: FrontEnd. 
    * Адресное пространство подсети: 192.168.1.0/24.
* Группа ресурсов — TestRG.
* Расположение: восточная часть США.
* Имя подсети шлюза: GatewaySubnet. Подсеть шлюза всегда необходимо называть *GatewaySubnet*.
    * Адресное пространство шлюза подсети: 192.168.200.0/26.
* Имя шлюза: ERGW.
* Общедоступное IP-имя шлюза: MyERGWVIP.
* Тип шлюза: ExpressRoute. Для конфигурации ExpressRoute требуется этот тип.

Можно просмотреть [видео](https://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-a-vpn-gateway-for-your-virtual-network) об этих действиях перед началом настройки.

## <a name="create-the-gateway-subnet"></a>Создание подсети шлюза

1. На [портале](https://portal.azure.com) перейдите к виртуальной сети Resource Manager, для которой необходимо создать шлюз.
2. В колонке виртуальной сети в разделе **Параметры** щелкните **Подсети**, чтобы открыть соответствующую колонку.
3. В колонке **Подсети** щелкните **+Подсеть шлюза**. Откроется колонка **Добавление подсети**. 
   
    ![Добавить подсеть шлюза](./media/expressroute-howto-add-gateway-portal-resource-manager/addgwsubnet.png "Add the gateway subnet")


4. В поле **Имя** автоматически добавляется значение GatewaySubnet. По этому имени Azure идентифицирует подсеть как подсеть шлюза. Замените автоматически заполненное значение **диапазона адресов** в соответствии с требованиями к конфигурации. Рекомендуется создать подсеть шлюза с блоком адресов /27 или больше (/26, /25 и т. д.). Щелкните **ОК**, чтобы сохранить значения и создать подсеть шлюза.

    ![Добавление подсети](./media/expressroute-howto-add-gateway-portal-resource-manager/addsubnetgw.png "Adding the subnet")

## <a name="create-the-virtual-network-gateway"></a>Создание шлюза виртуальной сети

1. На портале слева щелкните **+** и выполните поиск по запросу "Шлюз виртуальной сети". Найдите **шлюз виртуальной сети** в списке результатов и щелкните соответствующую запись. В нижней части колонки **Шлюз виртуальной сети** щелкните **Создать**. Откроется колонка **Create virtual network gateway** (Создание шлюза виртуальной сети).
2. В колонке **Создание шлюза виртуальной сети** укажите необходимые значения параметров.

    ![Поля колонки "Создание шлюза виртуальной сети"](./media/expressroute-howto-add-gateway-portal-resource-manager/gw.png "Поля колонки \"Создание шлюза виртуальной сети\"")
3. **Имя**. Назовите свой шлюз. Это имя должно отличаться от имени подсети шлюза. Это имя объекта шлюза, который создается.
4. **Тип шлюза**. Выберите **ExpressRoute**.
5. **SKU**. Выберите номер SKU шлюза из раскрывающегося списка.
6. **Расположение.** В поле **Расположение** укажите расположение виртуальной сети. Если расположение не указывает на регион, в котором размещена виртуальная сеть, такая виртуальная сеть не будет отображаться в раскрывающемся списке с виртуальными сетями.
7. Выберите виртуальную сеть, в которую нужно добавить этот шлюз. Щелкните **Виртуальная сеть**, чтобы открыть колонку **Выберите виртуальную сеть**. Выберите виртуальную сеть. Если ваша виртуальная сеть не отображается, проверьте, указан ли в поле **Расположение** регион, в котором находится эта сеть.
9. Выберите общедоступный IP-адрес. Щелкните **Общедоступный IP-адрес**, чтобы открыть колонку **Выбрать общедоступный IP-адрес**. Щелкните **+Создать**, чтобы открыть колонку **Создать общедоступный IP-адрес**. Введите имя общедоступного IP-адреса. Будет создан объект общедоступного IP-адреса, которому динамически назначается общедоступный IP-адрес. Нажмите кнопку **ОК**, чтобы сохранить изменения в этой колонке.
10. **Подписка**: Проследите, чтобы отображалась правильная подписка.
11. **Группа ресурсов**. Значение этого параметра зависит от выбранной виртуальной сети.
12. Не изменяйте сведения в поле **Расположение** после указания предыдущих параметров.
13. Проверьте параметры. В нижней части колонки можно выбрать параметр **Закрепить на панели мониторинга**, если необходимо, чтобы шлюз отображался на панели мониторинга.
14. Щелкните **Создать** , чтобы создать шлюз. После проверки параметров шлюз будет развернут. Создание шлюза виртуальной сети может длиться до 45 минут.

## <a name="next-steps"></a>Дальнейшие действия
После создания шлюза виртуальной сети вы можете связать виртуальную сеть с каналом ExpressRoute. Ознакомьтесь со статьей [Связывание виртуальной сети с каналом ExpressRoute](expressroute-howto-linkvnet-portal-resource-manager.md).
