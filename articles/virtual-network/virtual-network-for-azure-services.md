---
title: Виртуальная сеть для служб Azure
titlesuffix: Azure Virtual Network
description: Сведения о преимуществах развертывания ресурсов в виртуальной сети. Ресурсы в виртуальной сети могут взаимодействовать друг с другом и локальным ресурсом, не передавая трафик через Интернет.
services: virtual-network
documentationcenter: na
author: malopMSFT
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/25/2017
ms.author: malop
ms.reviewer: kumud
ms.openlocfilehash: 80d89914f33273fcb033ab47098a8864b11974c9
ms.sourcegitcommit: de47a27defce58b10ef998e8991a2294175d2098
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/15/2019
ms.locfileid: "67876163"
---
# <a name="virtual-network-integration-for-azure-services"></a>Интеграция виртуальной сети для служб Azure

Интеграция служб Azure с виртуальной сетью Azure обеспечивает закрытый доступ к службе из виртуальных машин или вычислительных ресурсов в виртуальной сети.
Службы Azure можно интегрировать с виртуальной сетью такими способами:
- Развертывание выделенных экземпляров службы в виртуальной сети. В этом случае закрытый доступ к этим службам сможет осуществляться в виртуальной сети и из локальных сетей.
- Расширение виртуальной сети за счет добавления конечной точки службы. Конечные точки позволяют защитить ресурсы отдельной службы в виртуальной сети.

Чтобы интегрировать несколько служб Azure с виртуальной сетью, можно использовать одну или объединить несколько указанных выше схем. Например, вы можете развернуть в виртуальной сети службу HDInsight и защитить учетную запись хранения в подсети HDInsight с помощью конечных точек службы.
 
## <a name="deploy-azure-services-into-virtual-networks"></a>Развертывание служб Azure в виртуальных сетях

При развертывании выделенных служб Azure в [виртуальной сети](virtual-networks-overview.md) вы можете взаимодействовать с ресурсами служб в частном порядке через частные IP-адреса.

![Службы, развернутые в виртуальной сети](./media/virtual-network-for-azure-services/deploy-service-into-vnet.png)

Развертывание служб в виртуальной сети обеспечивает следующие возможности:

- Ресурсы в виртуальной сети могут взаимодействовать друг с другом в частном порядке через частные IP-адреса, например непосредственно передавать данные между службой HDInsight и сервером SQL Server, запущенным на виртуальной машине в виртуальной сети.
- Локальные ресурсы могут обращаться к ресурсам в виртуальной сети с помощью частных IP-адресов через [VPN-подключение типа "сеть —сеть" (VPN-шлюз)](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%2ftoc.json#s2smulti) или [канал ExpressRoute](../expressroute/expressroute-introduction.md?toc=%2fazure%2fvirtual-network%2ftoc.json).
- Виртуальные сети могут создавать [пиринговую связь](virtual-network-peering-overview.md), что позволяет ресурсам в виртуальной сети взаимодействовать друг с другом через частные IP-адреса.
- Экземпляры службы в виртуальной сети обычно полностью управляются службой Azure. Сюда входит отслеживание работоспособности ресурсов и масштабирование с помощью нагрузки.
- Экземпляры служб развертываются в подсети в виртуальной сети. Входящий и исходящий сетевой доступ к подсети должен быть открыт с помощью [групп безопасности сети](security-overview.md#network-security-groups), согласно инструкциям службы.
- Некоторые службы также накладывают ограничения на подсеть, в которой они развернуты, ограничивая применение политик, маршруты или сочетание виртуальных машин и ресурсов служб в одной подсети. Проверяйте каждую службу по определенным ограничениям, так как они могут меняться со временем. Примерами таких служб являются Azure NetApp Files, выделенный HSM, экземпляры контейнеров Azure, служба приложений. 
- При необходимости для служб можно использовать [делегированную подсеть](virtual-network-manage-subnet.md#add-a-subnet), чтобы явно указать, что в этой подсети может размещаться определенная служба. Делегируя, службы получают явные разрешения на создание ресурсов, зависящих от службы, в делегированной подсети.
- См. Пример ответа REST API в [виртуальной сети с делегированной](https://docs.microsoft.com/rest/api/virtualnetwork/virtualnetworks/get#get-virtual-network-with-a-delegated-subnet)подсетью. Полный список служб, использующих делегированную модель подсети, можно получить с помощью доступного API [делегирования](https://docs.microsoft.com/rest/api/virtualnetwork/availabledelegations/list) .

### <a name="services-that-can-be-deployed-into-a-virtual-network"></a>Службы, которые можно развернуть в виртуальной сети

|Категория|Service| Выделенная подсеть ¹
|-|-|-|
| Службы вычислений | Виртуальные машины: [Linux](../virtual-machines/linux/infrastructure-networking-guidelines.md?toc=%2fazure%2fvirtual-network%2ftoc.json) или [Windows](../virtual-machines/windows/infrastructure-networking-guidelines.md?toc=%2fazure%2fvirtual-network%2ftoc.json) <br/>[Масштабируемые наборы виртуальных машин](../virtual-machine-scale-sets/virtual-machine-scale-sets-mvss-existing-vnet.md?toc=%2fazure%2fvirtual-network%2ftoc.json)<br/>[Облачная служба](https://msdn.microsoft.com/library/azure/jj156091): только виртуальная сеть (классическая)<br/> [Пакетная служба Azure](../batch/batch-api-basics.md?toc=%2fazure%2fvirtual-network%2ftoc.json#virtual-network-vnet-and-firewall-configuration)| Нет <br/> Нет <br/> Нет <br/> Без ²
| Сеть | [Шлюз приложений — WAF](../application-gateway/application-gateway-ilb-arm.md?toc=%2fazure%2fvirtual-network%2ftoc.json)<br/>[VPN-шлюз](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%2ftoc.json)<br/>[Брандмауэр Azure](../firewall/overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json) <br/>[Виртуальные сетевые модули](/windows-server/networking/sdn/manage/use-network-virtual-appliances-on-a-vn) | Да <br/> Да <br/> Да <br/> Нет
|Data|[Кэш Redis](../azure-cache-for-redis/cache-how-to-premium-vnet.md?toc=%2fazure%2fvirtual-network%2ftoc.json)<br/>[Управляемый экземпляр Базы данных SQL Azure](../sql-database/sql-database-managed-instance-connectivity-architecture.md?toc=%2fazure%2fvirtual-network%2ftoc.json)| Да <br/> Да <br/> 
|Analytics | [Azure HDInsight](../hdinsight/hdinsight-extend-hadoop-virtual-network.md?toc=%2fazure%2fvirtual-network%2ftoc.json)<br/>[Azure Databricks](../azure-databricks/what-is-azure-databricks.md?toc=%2fazure%2fvirtual-network%2ftoc.json) |Без ² <br/> Без ² <br/> 
| идентификации | [Доменные службы Azure Active Directory](../active-directory-domain-services/active-directory-ds-getting-started-vnet.md?toc=%2fazure%2fvirtual-network%2ftoc.json) |Нет <br/>
| Контейнеры | [Служба Azure Kubernetes (AKS)](../aks/concepts-network.md?toc=%2fazure%2fvirtual-network%2ftoc.json)<br/>[Экземпляр контейнера Azure (ACI)](https://www.aka.ms/acivnet)<br/>[Обработчик службы контейнеров Azure](https://github.com/Azure/acs-engine) с [подключаемым модулем](https://github.com/Azure/acs-engine/tree/master/examples/vnet) CNI для виртуальной сети Azure|Без ²<br/> Да <br/><br/> Нет
| Интернет | [Управление API](../api-management/api-management-using-with-vnet.md?toc=%2fazure%2fvirtual-network%2ftoc.json)<br/>[Среда службы приложений](../app-service/web-sites-integrate-with-vnet.md?toc=%2fazure%2fvirtual-network%2ftoc.json)<br/>[Azure Logic Apps](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json)<br/>|Да <br/> Да <br/> Да
| Размещенные* | [Выделенное устройство HSM Azure](../dedicated-hsm/index.yml?toc=%2fazure%2fvirtual-network%2ftoc.json)<br/>[Azure NetApp Files](../azure-netapp-files/azure-netapp-files-introduction.md?toc=%2fazure%2fvirtual-network%2ftoc.json)<br/>|Да <br/> Да <br/>
| | |

¹ "выделенный" означает, что в этой подсети можно развертывать только ресурсы, относящиеся к службам, и не может сочетаться с клиентом VM/Вмссс. <br/> рекомендуется, но не обязательное требование, накладываемое службой.


## <a name="service-endpoints-for-azure-services"></a>Конечные точки служб Azure

Некоторые службы Azure невозможно развернуть в виртуальной сети. Вы можете предоставить доступ к некоторым ресурсам службы только определенным подсетям виртуальной сети, включив конечные точки служб для виртуальной сети.  Дополнительные сведения о конечных точках служб для виртуальной сети и службах, для которых можно создать конечные точки, см. [здесь](virtual-network-service-endpoints-overview.md).
