---
title: Настройка подключения к SAP HANA в Azure (крупные экземпляры) с виртуальных машин | Документация Майкрософт
description: Настройка подключения с виртуальных машин для использования SAP HANA в Azure (крупные экземпляры).
services: virtual-machines-linux
documentationcenter: ''
author: msjuergent
manager: patfilot
editor: ''
tags: azure-resource-manager
keywords: ''
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/25/2019
ms.author: juergent
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: df60b31ce950cc6c242c8077e59d90c41771e4c3
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "66239480"
---
# <a name="connecting-azure-vms-to-hana-large-instances"></a>Подключение виртуальных машин Azure к крупным экземплярам HANA

Статья [Что такое SAP HANA в Azure (большие экземпляры)?](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-architecture) упоминает, что минимальное развертывание крупных экземпляров HANA с уровнем приложений SAP в Azure выглядит следующим образом.

![Виртуальная сеть Azure подключена к SAP HANA в Azure (крупные экземпляры) и к локальной сети](./media/hana-overview-architecture/image1-architecture.png)

Приглядевшись к стороне виртуальной сети Azure, нет необходимости для:

- определение виртуальной сети Azure, которое будет использоваться для развертывания виртуальных машин на уровне приложений SAP;
- определение подсети в виртуальной сети Azure по умолчанию — это то, во что развертываются виртуальные машины;
- в создаваемой виртуальной сети Azure должна быть по крайней мере одна подсеть виртуальной машины и одна подсеть шлюза для ExpressRoute. Таким подсетям нужно назначить диапазоны IP-адресов в соответствии со сведениями, указанными в разделах ниже.


## <a name="create-the-azure-virtual-network-for-hana-large-instances"></a>Создание виртуальной сети Azure для крупных экземпляров HANA

>[!Note]
>Эту виртуальную сеть Azure для крупных экземпляров HANA следует создавать с использованием модели развертывания с помощью Azure Resource Manager. Крупные экземпляры HANA не поддерживают старую модель развертывания Azure, известную как классическая модель развертывания.

Чтобы создать виртуальную сеть, можно использовать портал Azure, PowerShell, шаблон Azure или Azure CLI. (Дополнительные сведения см. в разделе [Создание виртуальной сети](../../../virtual-network/manage-virtual-network.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json#create-a-virtual-network).) В следующем примере мы рассмотрим виртуальную сеть, которая создается с помощью портала Azure.

При ссылке на **адресное пространство** в этой документации, чтобы адресное пространство, которое разрешено использовать виртуальной сети Azure. Оно будет использоваться в виртуальной сети для распространения маршрутов BGP. Это **адресное пространство** можно увидеть на рисунке ниже.

![Адресное пространство виртуальной сети Azure на портале Azure](./media/hana-overview-connectivity/image1-azure-vnet-address-space.png)

В предыдущем примере, с помощью 10.16.0.0/16 виртуальной сети Azure было передано достаточно большого и широкий диапазон IP-адресов для использования. Таким образом, все диапазоны IP-адресов последующих подсетей в виртуальной сети могут входить в указанное адресное пространство. Обычно мы не рекомендуем такой большой диапазон адресов для одной виртуальной сети в Azure. Давайте взглянем на подсети, которые определены в виртуальной сети Azure.

![Подсети виртуальной сети Azure и диапазоны их IP-адресов](./media/hana-overview-connectivity/image2b-vnet-subnets.png)

Как видно, создана виртуальная сеть с первой подсетью виртуальной машины (с именем "default") и подсеть GatewaySubnet.

В двух предыдущих рисунках **адресное пространство виртуальной сети** описаны платформы **диапазон IP-адресов подсети виртуальной машины Azure** и шлюза виртуальной сети.

Можно ограничить **адресное пространство виртуальной сети** определенными диапазонами, используемыми каждой подсетью. В этом случае **адресное пространство виртуальной сети** можно определить через несколько конкретных диапазонов, как показано ниже.

![Адресное пространство виртуальной сети Azure в качестве двух пространств](./media/hana-overview-connectivity/image3-azure-vnet-address-space_alternate.png)

В этом случае для **адресного пространства виртуальной сети** определено два пространства. Они идентичны диапазонам IP-адресов, определенным для диапазона IP-адресов подсети виртуальной машины Azure и диапазона IP-адресов подсети шлюза виртуальной сети. 

Для этих подсетей клиента (подсетей виртуальной машины) можно использовать любой стандарт именования. Тем не менее **каждой виртуальной подсети должна соответствовать только одна подсеть шлюза**, подключающаяся к цепи ExpressRoute для SAP HANA в Azure (крупные экземпляры). **Эту подсеть шлюза должна иметь имя «GatewaySubnet»** чтобы убедиться в том, что шлюз ExpressRoute размещается должным образом.

> [!WARNING] 
> Крайне важно, что подсеть шлюза называлась «GatewaySubnet».

Можно использовать несколько подсетей виртуальной машины, а также диапазоны не связанных адресов. **Адресное пространство виртуальной сети** должно охватывать эти диапазоны адресов. Они могут находиться в сводной форме. Они могут также быть заданы списком точных диапазонов подсетей виртуальной машины и подсети шлюза.

Ниже приведен краткий перечень важных сведений о виртуальной сети Azure, которая подключается к крупным экземплярам HANA.

- Вам нужно отправить **адресное пространство виртуальной сети** в корпорацию Майкрософт, при выполнении первоначального развертывания крупных экземпляров HANA. 
- **Адресное пространство виртуальной сети** может представлять собой один большой диапазон, охватывающий диапазоны IP-адресов подсети виртуальной машины Azure и диапазон IP-адресов подсети шлюза виртуальной сети.
- Можно отправить несколько диапазонов, охватывающих разные диапазоны IP-адресов в пределах диапазонов IP-адресов подсети виртуальной машины и диапазона IP-адресов подсети шлюза.
- Определенное **адресное пространство виртуальной сети** используется для распространения маршрутов BGP.
- Для подсети шлюза должно использоваться имя **GatewaySubnet**.
- Адресное пространство используется в качестве фильтра на стороне крупных экземпляров HANA, чтобы разрешать или запрещать передачу трафика к единицам крупных экземпляров HANA из Azure. Убедитесь, что сведения о маршрутах BGP виртуальной сети Azure и диапазонах IP-адресов, настроенных для фильтрации на стороне крупных экземпляров HANA, совпадают. В противном случае могут возникнуть проблемы с подключением.
- В разделе **Подключение виртуальной сети к каналу ExpressRoute крупных экземпляров HANA** ниже указаны некоторые дополнительные сведения о подсети шлюза.



## <a name="different-ip-address-ranges-to-be-defined"></a>Определение разных диапазонов IP-адресов 

Уже появилась некоторые диапазоны IP-адресов, которые необходимы для развертывания крупных экземпляров HANA. Существуют и другие важные диапазоны IP-адресов. Не все следующие диапазоны IP-адресов необходимо отправить в корпорацию Майкрософт. Тем не менее их нужно определить перед отправкой запроса на первоначальное развертывание.

- **Диапазон адресов виртуальной сети**: **Адресное пространство виртуальной сети** — это диапазоны IP-адресов, которые назначены пространству адресов в виртуальных сетях Azure. Эти сети будут подключаться к среде крупных экземпляров SAP HANA. Рекомендуется задавать этот параметр пространства адресов при помощи многострочного значения. Оно должно состоять из диапазона подсети виртуальной машины Azure и диапазонов подсети шлюза Azure. Диапазон подсети был показан на предыдущем рисунке. Он НЕ должен пересекаться с локальным диапазоном адресов, диапазоном IP-адресов пула серверов или с диапазоном адресов ER-P2P. Как это можно осуществить? Вашей корпоративной сети специалисты или поставщик услуг должен предоставить один или несколько IP-адрес, диапазон, не используются в локальной сети. Например подсеть виртуальной машины Azure 10.0.1.0/24 и 10.0.2.0/28 — подсеть в подсети шлюза Azure.  Мы рекомендуем, что ваше адресное пространство виртуальной сети Azure определяется как: 10.0.1.0/24 и 10.0.2.0/28. Несмотря на то что значения адреса пространств можно агрегировать, мы рекомендуем сопоставить их с диапазонами подсетей. Таким образом можно избежать случайного повторного использования неиспользуемых диапазонов IP-адресов в больших адресных пространствах в других местах вашей сети. **Адресное пространство виртуальной сети — это диапазон IP-адресов. Его нужно отправить в корпорацию Майкрософт, когда поступает запрос на первоначальное развертывание**.
- **Диапазон IP-адресов в подсети виртуальных машин Azure**. Этот диапазон IP-адресов назначается параметру подсети виртуальной сети Azure. Этот параметр находится в виртуальной сети Azure и подключается к среде крупных экземпляров SAP HANA. Этот диапазон IP-адресов используется для назначения IP-адресов виртуальным машинам Azure. IP-адреса вне этого диапазона разрешено использовать для подключения к серверам крупных экземпляров SAP HANA. При необходимости можно использовать несколько подсетей виртуальной машины Azure. Для каждой подсети виртуальной машины Azure рекомендуется блок /24 CIDR. Этот диапазон адресов должен быть частью значений, используемых в адресном пространстве виртуальной сети Azure. Как это можно осуществить? Специалисты по работе с корпоративными сетями или поставщик услуг должны предоставить диапазон IP-адресов, используемый вне сети.
- **Диапазон IP-адресов подсети шлюза виртуальной сети.** В зависимости от функций, которые вы планируете использовать, рекомендуемый размер может быть следующим.
   - Ультрапроизводительный шлюз ExpressRoute: блок адресов размером /26 — требуется для номеров SKU класса II.
   - Сосуществование с VPN и ExpressRoute с применением шлюза HighPerformance ExpressRoute (или более низкой версии): блок адресов размером /27.
   - Другие ситуации: блок адресов размером /28. Этот диапазон адресов должен быть частью значений, используемых в адресном пространстве виртуальной сети. Этот диапазон адресов должен быть частью значений, используемых в адресном пространстве виртуальной сети Azure, которые нужно отправить в корпорацию Майкрософт. Как это можно осуществить? Специалисты по работе с корпоративными сетями или поставщик услуг должны предоставить диапазон IP-адресов, используемых вне сети. 
- **Диапазон IP-адресов для подключений ER-P2P**. Это диапазон IP-адресов для однорангового подключения ExpressRoute (ER) крупных экземпляров SAP HANA. Это должен быть диапазон IP-адресов CIDR с маской /29. Этот диапазон НЕ ДОЛЖЕН пересекаться с локальным диапазоном адресов и с другими диапазонами IP-адресов Azure. Он используется для настройки подключения ER между шлюзом ExpressRoute виртуальной сети Azure и серверами крупных экземпляров SAP HANA. Как это можно осуществить? Специалисты по работе с корпоративными сетями или поставщик услуг должны предоставить диапазон IP-адресов, используемых вне сети. **Это диапазон IP-адресов. Его нужно отправить в корпорацию Майкрософт, когда поступает запрос на первоначальное развертывание**.  
- **Диапазон IP-адресов пула серверов**. Этот диапазон IP-адресов используется для назначения отдельных IP-адресов серверам больших экземпляров HANA. Размер рекомендуемой подсети — блок /24 CIDR. При необходимости его можно уменьшить всего лишь до 64 IP-адресов. Первые 30 IP-адресов в этом диапазоне будут зарезервированы для использования корпорацией Майкрософт. Убедитесь, что этот факт при выборе размера диапазона учитывается. Этот диапазон НЕ ДОЛЖЕН пересекаться с локальным диапазоном адресов и с другими диапазонами IP-адресов Azure. Как это можно осуществить? Специалисты по работе с корпоративными сетями или поставщик услуг должны предоставить диапазон IP-адресов, используемых вне сети.  **Этот диапазон IP-адресов нужно отправить в корпорацию Майкрософт вместе с запросом на первоначальное развертывание.**

Дополнительные диапазоны IP-адресов, которые со временем необходимо отправить в корпорацию Майкрософт:

- Если вы решили использовать [ExpressRoute глобальным доступом](https://docs.microsoft.com/azure/expressroute/expressroute-global-reach) Чтобы включить прямой маршрутизации из локальной к единицам крупных экземпляров HANA, необходимо зарезервировать другой маской/29 диапазон IP-адресов. Этот диапазон не может пересекаться с любыми другими диапазонов IP-адресов, определенные до.
- Если вы решили использовать [ExpressRoute глобальным доступом](https://docs.microsoft.com/azure/expressroute/expressroute-global-reach) чтобы прямая Маршрутизация отдельный клиент крупных экземпляров HANA в одном регионе Azure в другой клиент крупных экземпляров HANA в другом регионе Azure, необходимо зарезервировать другой маской/29 диапазон IP-адресов . Этот диапазон не может пересекаться с любыми другими диапазонов IP-адресов, определенные до.

Дополнительные сведения об ExpressRoute глобальным доступом и каникул крупных экземпляров HANA проверка документов:

- [Сетевой архитектуры SAP HANA (крупные экземпляры)](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-network-architecture)
- [Подключение виртуальной сети к крупным экземплярам HANA](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-connect-vnet-express-route)
 
Необходимо определить и спланировать диапазоны IP-адресов, которые были описаны ранее. Тем не менее передавать все из них в корпорацию Майкрософт не нужно. В корпорацию Майкрософт необходимо отправить следующие диапазоны IP-адресов.

- Адресное пространство виртуальной сети.
- Диапазон IP-адресов для подключений ER-P2P.
- Диапазон IP-адресов пула серверов.

При добавлении дополнительных виртуальных сетей, которым требуется подключиться к крупным экземплярам HANA, необходимо отправить новое адресное пространство виртуальной сети Azure, добавляемой в корпорацию Майкрософт. 

Ниже приведен пример различных диапазонов, которые нужно настроить и отправить в корпорацию Майкрософт. Значение для адресного пространства виртуальной сети Azure не агрегируется в первом примере. Оно определяется на основе диапазона IP-адресов подсети виртуальной машины Azure и диапазона IP-адресов подсети шлюза виртуальной сети. 

Можно использовать несколько подсетей виртуальной машины в виртуальной сети Azure, настроив и отправив дополнительные диапазоны IP-адресов дополнительных виртуальных подсетей в составе адресного пространства виртуальной сети Azure.

![Диапазоны IP-адресов, необходимые для минимального развертывания SAP HANA в Azure (крупные экземпляры)](./media/hana-overview-connectivity/image4b-ip-addres-ranges-necessary.png)

Рисунок не содержит дополнительный IP-адрес, диапазон, которые необходимы для дополнительное использование ExpressRoute глобальным доступом.

Можно также объединить данные, отправленные в корпорацию Майкрософт. В этом случае адресное пространство виртуальной сети Azure будет содержать всего одно пространство. Используя диапазоны IP-адресов из примера выше, объединенное адресное пространство виртуальной сети может выглядеть, как на следующем изображении.

![Второй вариант диапазонов IP-адресов, необходимых для минимального развертывания SAP HANA в Azure (крупные экземпляры)](./media/hana-overview-connectivity/image5b-ip-addres-ranges-necessary-one-value.png)

Как можно заметить в примере, вместо двух диапазонов меньшего размера, определяющих адресное пространство виртуальной сети Azure, у нас есть один больший диапазон, охватывающий 4096 IP-адресов. При таком крупном определении адресного пространства некоторые крупные диапазоны не используются. Так как значения адресного пространства виртуальной сети используются для распространения маршрутов BGP, использование неиспользуемых диапазонов локально или в другой части сети может вызвать проблемы маршрутизации. Рисунок не содержит дополнительный IP-адрес, диапазон, которые необходимы для дополнительное использование ExpressRoute глобальным доступом.

Мы рекомендуем сохранить точное соответствие фактическое адресное пространство подсети, используемом адресное пространство. При необходимости вы можете в любое время добавить новые значения адресного пространства позже, не вызывая простоя в виртуальной сети.
 
> [!IMPORTANT] 
> Каждый диапазон IP-адресов ER-P2P, пула серверов IP-адресов и адресное пространство виртуальной сети Azure необходимо **НЕ** перекрывать друг с другом или с другими диапазонами, используемыми в вашей сети. Они должны быть дискретными. Как показано на двух предыдущих рисунках, они не могут быть подсетями другого диапазона. Если диапазоны пересекаются, виртуальная сеть Azure может не подключиться к цепи ExpressRoute.

## <a name="next-steps-after-address-ranges-have-been-defined"></a>Дальнейшие действия после определения диапазонов адресов
После определения диапазонов IP-адресов нужно сделать следующее:

1. Отправить диапазоны IP-адресов для адресного пространства виртуальной сети Azure, подключения ER-P2P и диапазона IP-адресов пула серверов, а также другие данные, указанные в начале документа. На этом этапе также можно приступить к созданию виртуальной сети и подсети виртуальной машины. 
2. Корпорация Майкрософт создает канал ExpressRoute между подпиской Azure и меткой крупных экземпляров HANA.
3. Корпорация Майкрософт создает клиентскую сеть в стеке крупных экземпляров.
4. Специалисты Майкрософт настроят сеть таким образом, чтобы инфраструктура SAP HANA в Azure (крупные экземпляры) принимала IP-адреса из адресного диапазона виртуальной сети Azure, которая будет взаимодействовать с крупными экземплярами HANA.
5. В зависимости от конкретного приобретенного номера SKU SAP HANA в Azure (крупные экземпляры) корпорация Майкрософт назначит единицу вычисления в сети клиента, выделит и подключит хранилище, а также установит операционную систему (SUSE или Red Hat Linux). IP-адреса для этих единиц берутся из диапазона IP-адресов пула серверов, отправленного в корпорацию Майкрософт.

В конце развертывания специалисты Майкрософт отправят вам следующие данные:
- Сведения, нужные для подключения виртуальной сети Azure к каналу ExpressRoute, по которому они подключаются к крупным экземплярам HANA, включают:
     - ключи авторизации;
     - код узла ExpressRoute.
- Данные для доступа к крупным экземплярам HANA по настроенному каналу ExpressRoute и виртуальной сети Azure.

Можно также обратиться к последовательности подключения крупных экземпляров HANA, которая описана в документе [SAP HANA on Azure (Large Instances) Setup](https://azure.microsoft.com/resources/sap-hana-on-azure-large-instances-setup/) (Комплексная настройка крупных экземпляров SAP HANA). Многие из следующих действий демонстрируются в примере развертывания в этом документе. 

## <a name="next-steps"></a>Дальнейшие действия

- См. статью [Подключение виртуальной сети к крупным экземплярам HANA](hana-connect-vnet-express-route.md).
