---
title: VPN-устройства для распределенных подключений Azure | Документация Майкрософт
description: В этой статье описываются VPN-устройства и параметры IPsec для подключений типа "сеть — сеть" через VPN-шлюз, а также приводятся ссылки на примеры и инструкции по настройке.
services: vpn-gateway
author: yushwang
ms.service: vpn-gateway
ms.topic: article
ms.date: 07/05/2019
ms.author: yushwang
ms.openlocfilehash: 8301594f63efaa5c6484a4dfd640aafa96cf15a0
ms.sourcegitcommit: 2e4b99023ecaf2ea3d6d3604da068d04682a8c2d
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/09/2019
ms.locfileid: "67666260"
---
# <a name="about-vpn-devices-and-ipsecike-parameters-for-site-to-site-vpn-gateway-connections"></a>VPN-устройства и параметры IPsec/IKE для подключений типа "сеть — сеть" через VPN-шлюз

Для настройки распределенного VPN-подключения типа "сеть — сеть" через VPN-шлюз требуется VPN-устройство. Подключения типа "сеть — сеть" можно использовать для создания гибридного решения, а также когда требуется безопасное подключение между локальной и виртуальной сетями. В этой статье перечислены проверенные VPN-устройства и параметры IPsec/IKE для VPN-шлюзов.

> [!IMPORTANT]
> Ознакомьтесь с [известными проблемами совместимости устройств](#known), если вы столкнулись с проблемами подключения между локальными VPN-устройствами и VPN-шлюзами.
>

### <a name="items-to-note-when-viewing-the-tables"></a>На что следует обратите внимание при просмотре таблицы:

* Изменилась терминология, связанная с VPN-шлюзами Azure. Изменились только названия. Функциональность осталась прежней,
  * Статическая маршрутизация — на основе политик (PolicyBased).
  * Динамическая маршрутизация — на основе маршрутов (RouteBased).
* Спецификации высокопроизводительных VPN-шлюзов и VPN-шлюзов типа RouteBased одинаковы, если не указано иное. Например, проверенные VPN-устройства, совместимые с VPN-шлюзами RouteBased, также совместимы с высокопроизводительными VPN-шлюзами.

## <a name="devicetable"></a>Проверенные VPN-устройства и руководства по конфигурации устройства

В сотрудничестве с поставщиками устройств мы утвердили набор стандартных VPN-устройств. Все устройства из приведенного ниже списка семейств должны работать с VPN-шлюзами. Чтобы понять, какой тип VPN (PolicyBased или RouteBased) использовать для VPN-шлюза, который необходимо настроить, ознакомьтесь с [этим разделом](vpn-gateway-about-vpn-gateway-settings.md#vpntype).

Чтобы настроить VPN-устройство, перейдите по ссылкам, соответствующим семейству устройств. Ссылки на инструкции по настройке будут предоставляться по мере возможности. За поддержкой VPN-устройства обратитесь к его изготовителю.

|**поставщик**          |**Семейство устройств**     |**Минимальная версия ОС** |**Инструкции по настройке PolicyBased** |**Инструкции по настройке RouteBased** |
| ---                | ---                  | ---                   | ---            | ---           |
| A10 Networks, Inc. |Thunder CFW           |ACOS 4.1.1             |Не совместимо  |[Руководство по настройке](https://www.a10networks.com/resources/deployment-guides/a10-thunder-cfw-ipsec-vpn-interoperability-azure-vpn-gateways)|
| Allied Telesis     |VPN-маршрутизаторы серии AR |Серии AR 5.4.7+               |Скоро     |[Руководство по настройке](https://www.alliedtelesis.com/documents/how-to/configure/site-to-site-vpn-between-azure-and-ar-series-router)|
| Barracuda Networks, Inc. |Брандмауэр Barracuda NextGen серии F |PolicyBased: 5.4.3<br>RouteBased: 6.2.0 |[Руководство по настройке](https://techlib.barracuda.com/NGF/AzurePolicyBasedVPNGW) |[Руководство по настройке](https://techlib.barracuda.com/NGF/AzureRouteBasedVPNGW) |
| Barracuda Networks, Inc. |Брандмауэр Barracuda NextGen серии Х |Barracuda Firewall 6.5 |[Руководство по настройке](https://techlib.barracuda.com/BFW/ConfigAzureVPNGateway) |Не совместимо |
| Check Point |Security Gateway |R80.10 |[Руководство по настройке](https://supportcenter.checkpoint.com/supportcenter/portal?eventSubmit_doGoviewsolutiondetails=&solutionid=sk101275) |[Руководство по настройке](https://supportcenter.checkpoint.com/supportcenter/portal?eventSubmit_doGoviewsolutiondetails=&solutionid=sk101275) |
| Cisco              |ASA       |8.3<br>8.4+ (IKEv2*) |Поддерживаются |[Руководство по настройке*](https://www.cisco.com/c/en/us/support/docs/security/adaptive-security-appliance-asa-software/214109-configure-asa-ipsec-vti-connection-to-az.html) |
| Cisco |ASR |PolicyBased: iOS 15.1<br>RouteBased: iOS 15.2 |Поддерживаются |Поддерживаются |
| Cisco | CSR-ФАЙЛА | RouteBased: IOS-XE 16.10 | | [Скрипт настройки](vpn-gateway-download-vpndevicescript.md) |
| Cisco |ISR |PolicyBased: iOS 15.0<br>RouteBased*: iOS 15.1 |Поддерживаются |Поддерживаются |
| Cisco |Meraki |Н/Д |Не совместимо |Не совместимо |
| Citrix |NetScaler MPX, SDX, VPX |10.1 и выше |[Руководство по настройке](https://docs.citrix.com/en-us/netscaler/11-1/system/cloudbridge-connector-introduction/cloudbridge-connector-azure.html) |Не совместимо |
| F5 |Серия BIG-IP |12.0 |[Руководство по настройке](https://devcentral.f5.com/articles/connecting-to-windows-azure-with-the-big-ip) |[Руководство по настройке](https://devcentral.f5.com/articles/big-ip-to-azure-dynamic-ipsec-tunneling) |
| Fortinet |FortiGate |FortiOS 5.6 |  |[Руководство по настройке](https://cookbook.fortinet.com/ipsec-vpn-microsoft-azure-56/) |
| Internet Initiative Japan (IIJ) |Серия SEIL |SEIL/X 4.60<br>SEIL/B1 4.60<br>SEIL/x86 3.20 |[Руководство по настройке](https://www.iij.ad.jp/biz/seil/ConfigAzureSEILVPN.pdf) |Не совместимо |
| Juniper |SRX |PolicyBased: JunOS 10.2<br>RouteBased: JunOS 11.4 |Поддерживаются |[Скрипт настройки](vpn-gateway-download-vpndevicescript.md) |
| Juniper |Серия J |PolicyBased: JunOS 10.4r9<br>RouteBased: JunOS 11.4 |Поддерживаются |[Скрипт настройки](vpn-gateway-download-vpndevicescript.md) |
| Juniper |ISG |ScreenOS 6.3 |Поддерживаются |[Скрипт настройки](vpn-gateway-download-vpndevicescript.md) |
| Juniper |SSG |ScreenOS 6.2 |Поддерживаются |[Скрипт настройки](vpn-gateway-download-vpndevicescript.md) |
| Juniper |MX |JunOS 12.x|Поддерживаются |[Скрипт настройки](vpn-gateway-download-vpndevicescript.md) |
| Майкрософт |Служба маршрутизации и удаленного доступа |Windows Server 2012 |Не совместимо |Поддерживаются |
| Open Systems AG |Шлюз безопасности Mission Control |Н/Д |[Руководство по настройке](https://www.open.ch/_pdf/Azure/AzureVPNSetup_Installation_Guide.pdf) |Не совместимо |
| Palo Alto Networks |Все устройства под управлением PAN-OS |PAN-OS<br>PolicyBased: 6.1.5 или более поздней версии<br>RouteBased: 7.1.4 |[Руководство по настройке](https://live.paloaltonetworks.com/t5/Configuration-Articles/How-to-Configure-VPN-Tunnel-Between-a-Palo-Alto-Networks/ta-p/59065) |[Руководство по настройке](https://knowledgebase.paloaltonetworks.com/KCSArticleDetail?id=kA10g000000Cm6WCAS) |
| ShareTech | UTM нового поколения (серия NU) | 9.0.1.3 | Не совместимо | [Руководство по настройке](http://www.sharetech.com.tw/images/file/Solution/NU_UTM/S2S_VPN_with_Azure_Route_Based_en.pdf) |
| SonicWall |Серия TZ и NSA<br>Серия SuperMassive<br>Серия NSA класса E |SonicOS 5.8.x<br>SonicOS 5.9.x<br>SonicOS 6.x |Не совместимо |[Руководство по настройке](https://www.sonicwall.com/support/knowledge-base/170505320011694) |
| Sophos | Брандмауэр следующего поколения XG | XG версии 17 | | [Руководство по настройке](https://community.sophos.com/kb/127546)<br><br>[Руководство по настройке нескольких SA](https://community.sophos.com/kb/en-us/133154) |
| Synology | MR2200ac <br>RT2600ac <br>RT1900ac | SRM1.1.5/VpnPlusServer-1.2.0 |  | [Руководство по настройке](https://www.synology.com/en-global/knowledgebase/SRM/tutorial/VPN/How_to_set_up_Site_to_Site_VPN_between_Synology_Router_and_MS_Azure) |
| Ubiquiti | EdgeRouter | EdgeOS v1.10 |  | [BGP через IKEv2 или IPsec](https://help.ubnt.com/hc/en-us/articles/115012374708)<br><br>[VTI через IKEv2 или IPsec](https://help.ubnt.com/hc/en-us/articles/115012305347)
| WatchGuard |Все |Fireware XTM<br> PolicyBased: 11.11.x<br>RouteBased: 11.12.x |[Руководство по настройке](http://watchguardsupport.force.com/publicKB?type=KBArticle&SFDCID=kA2F00000000LI7KAM&lang=en_US) |[Руководство по настройке](http://watchguardsupport.force.com/publicKB?type=KBArticle&SFDCID=kA22A000000XZogSAG&lang=en_US)|
| Zyxel |Серии ZyWALL универсальную группу безопасности<br>Серии ZyWALL ATP<br>Серии ZyWALL VPN | ZLD v4.32 + | | [VTI через IKEv2 или IPsec](https://businessforum.zyxel.com/discussion/2648/)<br>[BGP через IKEv2 или IPsec](https://businessforum.zyxel.com/discussion/2650/)|

> [!NOTE]
>
> (*) Версии Cisco ASA 8.4+ с поддержкой IKEv2 могут подключаться к VPN-шлюзу Azure при помощи настраиваемой политики IPsec/IKE с параметром UsePolicyBasedTrafficSelectors. Дополнительные сведения см. в этой [статье](vpn-gateway-connect-multiple-policybased-rm-ps.md).
>
> (\*\*) Маршрутизаторы ISR серии 7200 поддерживают только VPN типа PolicyBased.

## <a name="configscripts"></a>Загрузка сценариев конфигурации VPN-устройства из Azure

Для некоторых устройств можно загрузить сценарии конфигурации непосредственно из Azure. Дополнительные сведения см. в статье [Загрузка сценариев конфигурации VPN-устройств для VPN-подключений типа "сеть — сеть"](vpn-gateway-download-vpndevicescript.md).

### <a name="devices-with-available-configuration-scripts"></a>Устройства с доступными сценариями конфигурации

[!INCLUDE [scripts](../../includes/vpn-gateway-device-configuration-scripts.md)]

## <a name="additionaldevices"></a>Непроверенные VPN-устройства

Даже если ваше устройство не указано в представленной таблице проверенных VPN-устройств, оно может поддерживать подключение типа "сеть — сеть". Чтобы получить дополнительную информацию и инструкции по настройке, обратитесь к изготовителю устройства.

## <a name="editing"></a>Изменение примеров конфигурации устройств

После скачивания предоставленного примера конфигурации VPN-устройства некоторые значения необходимо заменить в соответствии с параметрами вашей среды.

### <a name="to-edit-a-sample"></a>Чтобы изменить образец, выполните описанные ниже действия

1. Откройте пример с помощью блокнота.
2. Найдите и замените все строки <*text*> значениями, отражающими свойства вашей среды. Не забудьте добавить "<" и ">". Если указывается имя, оно должно быть уникальным. Если команда не работает, обратитесь к документации изготовителя устройства.

| **Пример текста** | **Изменить на** |
| --- | --- |
| &lt;RP_OnPremisesNetwork&gt; |Выбранное имя для данного объекта. Пример: myOnPremisesNetwork. |
| &lt;RP_AzureNetwork&gt; |Выбранное имя для данного объекта. Пример: myAzureNetwork. |
| &lt;RP_AccessList&gt; |Выбранное имя для данного объекта. Пример: myAzureAccessList. |
| &lt;RP_IPSecTransformSet&gt; |Выбранное имя для данного объекта. Пример: myIPSecTransformSet. |
| &lt;RP_IPSecCryptoMap&gt; |Выбранное имя для данного объекта. Пример: myIPSecCryptoMap. |
| &lt;SP_AzureNetworkIpRange&gt; |Укажите диапазон. Пример: 192.168.0.0 |
| &lt;SP_AzureNetworkSubnetMask&gt; |Укажите маску подсети. Пример: 255.255.0.0 |
| &lt;SP_AzureNetworkIpRange&gt; |Укажите локальный диапазон. Пример: 10.2.1.0 |
| &lt;SP_AzureNetworkSubnetMask&gt; |Укажите локальную маску подсети. Пример: 255.255.255.0 |
| &lt;SP_AzureGatewayIpAddress&gt; |Эта информация относится к виртуальной сети и находится на портале управления в поле **IP-адрес шлюза**. |
| &lt;SP_PresharedKey&gt; |Эта информация относится к виртуальной сети и находится на портале управления в разделе «Управление ключами». |

## <a name="ipsec"></a>Параметры IPsec/IKE

> [!IMPORTANT]
> 1. Таблицы содержат комбинации алгоритмов и параметров VPN-шлюзов Azure, используемые в стандартной конфигурации. Для VPN-шлюзов на основе маршрутов, созданных с помощью модели развертывания на основе Azure Resource Management, можно указать пользовательскую политику для каждого отдельного подключения. Дополнительные сведения см. в инструкциях по [настройке политик IPsec/IKE](vpn-gateway-ipsecikepolicy-rm-powershell.md).
>
> 2. Кроме того, необходимо установить для TCP **MSS** значение **1350**. Если же VPN-устройства не поддерживают фиксацию MSS, в качестве альтернативы в интерфейсе туннеля можно указать для **MTU** **1400** байт.
>

В таблицах ниже приведены следующие сведения:

* SA — сопоставление безопасности.
* Этап 1 IKE также называется "Главный режим".
* Этап 2 IKE также называется "Быстрый режим".

### <a name="ike-phase-1-main-mode-parameters"></a>Параметры этапа 1 IKE (главный режим)

| **Property**          |**PolicyBased**    | **RouteBased**    |
| ---                   | ---               | ---               |
| Версия IKE           |IKEv1              |IKEv2              |
| Группа Диффи — Хелмана  |Группа 2 (1024 бита) |Группа 2 (1024 бита) |
| Метод проверки подлинности |Общий ключ     |Общий ключ     |
| Алгоритмы шифрования и хэширования |1. AES256, SHA256<br>2. AES256, SHA1<br>3. AES128, SHA1<br>4. 3DES, SHA1 |1. AES256, SHA1<br>2. AES256, SHA256<br>3. AES128, SHA1<br>4. AES128, SHA256<br>5. 3DES, SHA1<br>6. 3DES, SHA256 |
| Срок действия SA           |28 800 сек     |28 800 сек     |

### <a name="ike-phase-2-quick-mode-parameters"></a>Параметры этапа 2 IKE (быстрый режим)

| **Property**                  |**PolicyBased**| **RouteBased**                              |
| ---                           | ---           | ---                                         |
| Версия IKE                   |IKEv1          |IKEv2                                        |
| Алгоритмы шифрования и хэширования |1. AES256, SHA256<br>2. AES256, SHA1<br>3. AES128, SHA1<br>4. 3DES, SHA1 |[Предложения по сопоставлению безопасности в быстром режиме на основе маршрутизации](#RouteBasedOffers) |
| Срок действия SA (время)            |3600 секунд  |27 000 секунд                                |
| Срок действия SA (байты)           |102 400 000 КБ | -                                           |
| Полная безопасность пересылки (PFS) |Нет             |[Предложения по сопоставлению безопасности в быстром режиме на основе маршрутизации](#RouteBasedOffers) |
| Обнаружение неиспользуемых одноранговых узлов (DPD)     |Не поддерживается  |Поддерживаются                                    |


### <a name ="RouteBasedOffers"></a>Предложения по сопоставлению безопасности IPsec для VPN на основе маршрутов (в быстром режиме)

В следующей таблице перечислены предложения по сопоставлению безопасности IPsec (в быстром режиме). Предложения перечислены в порядке предпочтения представления или принятия предложения.

#### <a name="azure-gateway-as-initiator"></a>Шлюз Azure в качестве инициатора

|-  |**Шифрование**|**Authentication**|**Группа PFS**|
|---| ---          |---               |---          |
| 1 |GCM AES256    |GCM (AES256)      |Нет         |
| 2 |AES256        |SHA1              |Нет         |
| 3 |3DES          |SHA1              |Нет         |
| 4 |AES256        |SHA256            |Нет         |
| 5 |AES128        |SHA1              |Нет         |
| 6 |3DES          |SHA256            |Нет         |

#### <a name="azure-gateway-as-responder"></a>Шлюз Azure в качестве ответчика

|-  |**Шифрование**|**Authentication**|**Группа PFS**|
|---| ---          | ---              |---          |
| 1 |GCM AES256    |GCM (AES256)      |Нет         |
| 2 |AES256        |SHA1              |Нет         |
| 3 |3DES          |SHA1              |Нет         |
| 4 |AES256        |SHA256            |Нет         |
| 5 |AES128        |SHA1              |Нет         |
| 6 |3DES          |SHA256            |Нет         |
| 7 |DES           |SHA1              |Нет         |
| 8 |AES256        |SHA1              |1            |
| 9 |AES256        |SHA1              |2            |
| 10|AES256        |SHA1              |14           |
| 11|AES128        |SHA1              |1            |
| 12|AES128        |SHA1              |2            |
| 13|AES128        |SHA1              |14           |
| 14|3DES          |SHA1              |1            |
| 15|3DES          |SHA1              |2            |
| 16|3DES          |SHA256            |2            |
| 17|AES256        |SHA256            |1            |
| 18|AES256        |SHA256            |2            |
| 19|AES256        |SHA256            |14           |
| 20|AES256        |SHA1              |24           |
| 21|AES256        |SHA256            |24           |
| 22|AES128        |SHA256            |Нет         |
| 23|AES128        |SHA256            |1            |
| 24|AES128        |SHA256            |2            |
| 25|AES128        |SHA256            |14           |
| 26|3DES          |SHA1              |14           |

* Вы можете указать NULL-шифрование ESP IPsec для высокопроизводительных VPN-шлюзов и VPN-шлюзов типа RouteBased. NULL-шифрование не защищает данные при передаче. Его следует использовать только в случаях, когда требуется максимальная пропускная способность и минимальная задержка. Клиенты могут выбрать его в случаях, когда данные передаются между виртуальными сетями или где-то в решении применяется шифрование.
* Для распределенных подключений через Интернет используйте параметры по умолчанию VPN-шлюзов Azure с алгоритмами шифрования и хэширования, перечисленными в таблицах выше, чтобы обеспечить безопасность обмена важными данными.

## <a name="known"></a>Известные проблемы совместимости устройств

> [!IMPORTANT]
> Известно о нескольких проблемах совместимости между VPN-устройствами сторонних производителей и VPN-шлюзами Azure. Команда разработчиков Azure активно сотрудничает с поставщиками для устранения проблем, перечисленных ниже. По мере устранения проблем данная страница будет дополняться наиболее актуальными сведениями. Рекомендуется периодически просматривать ее.
>
>

### <a name="feb-16-2017"></a>16 февраля 2017 г.

**Устройства Palo Alto Networks с версией PAN-OS, предшествующей 7.1.4,** для VPN на основе маршрутов Azure: если используются VPN-устройства от Palo Alto Networks с версией PAN-OS, предшествующей 7.1.4, и возникают проблемы с подключением к VPN-шлюзам Azure на основе маршрутов, выполните приведенные ниже действия.

1. Проверьте версию встроенного ПО устройства Palo Alto Networks. Если версия PAN-OS предшествует версии 7.1.4, выполните обновление до версии 7.1.4.
2. На устройстве Palo Alto Networks измените время существования этапа 2 сопоставления безопасности (или быстрого режима сопоставления безопасности) на 28 800 секунд (8 часов) при подключении к VPN-шлюзу Azure.
3. Если проблема с подключением не исчезнет, отправьте запрос в службу поддержки на портале Azure.
