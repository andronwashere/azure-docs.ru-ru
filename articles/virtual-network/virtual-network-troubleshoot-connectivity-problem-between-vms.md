---
title: Устранение проблем с подключением между виртуальными машинами Azure | Документация Майкрософт
description: Сведения об устранении проблем с подключениями между виртуальными машинами Azure.
services: virtual-network
documentationcenter: na
author: chadmath
manager: cshepard
editor: ''
tags: azure-resource-manager
ms.service: virtual-network
ms.devlang: na
ms.topic: troubleshooting
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/30/2018
ms.author: genli
ms.openlocfilehash: fc3d6ab1d7fdf05963d9ecd350deccd940a95b87
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "61036389"
---
# <a name="troubleshooting-connectivity-problems-between-azure-vms"></a>Устранение проблем с подключением между виртуальными машинами Azure

При подключении между виртуальными машинами Azure могут возникать проблемы. В этой статье приведены действия по устранению этой проблемы. 

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="symptom"></a>Симптом

Виртуальной машине Azure не удается подключиться к другой виртуальной машине Azure.

## <a name="troubleshooting-guidance"></a>Рекомендации по устранению неполадок 

1. [Проверьте, правильно ли настроен сетевой интерфейс](#step-1-check-whether-nic-is-misconfigured).
2. [Проверьте, не блокируется ли трафик группой безопасности сети (NSG) или определяемым пользователем маршрутом (UDR)](#step-2-check-whether-network-traffic-is-blocked-by-nsg-or-udr).
3. [Проверьте, не блокируется ли трафик брандмауэром виртуальной машины](#step-3-check-whether-network-traffic-is-blocked-by-vm-firewall).
4. [Проверьте, прослушивается ли порт приложением или службой виртуальной машины](#step-4-check-whether-vm-app-or-service-is-listening-on-the-port).
5. [Проверьте, не вызвана ли проблема SNAT](#step-5-check-whether-the-problem-is-caused-by-snat).
6. [Проверьте, блокируется ли трафик классической виртуальной машины списками управления доступом (ACL)](#step-6-check-whether-traffic-is-blocked-by-acls-for-the-classic-vm).
7. [Проверьте, создана ли конечная точка классической виртуальной машины](#step-7-check-whether-the-endpoint-is-created-for-the-classic-vm).
8. [Попробуйте подключиться к сетевой папке виртуальной машины](#step-8-try-to-connect-to-a-vm-network-share).
9. [Проверьте подключение Inter-Vnet](#step-9-check-inter-vnet-connectivity).

## <a name="troubleshooting-steps"></a>Действия по устранению неполадок

Чтобы устранить проблему, выполните действия ниже, а затем проверьте, помогло ли это. 

### <a name="step-1-check-whether-nic-is-misconfigured"></a>Шаг 1. Проверьте, правильно ли настроен сетевой адаптер

Выполните действия, указанные в статье [Как сбросить сетевой интерфейс для виртуальной машины Azure под управлением Windows](../virtual-machines/windows/reset-network-interface.md). 

Если изменение конфигурации сетевого интерфейса не решило проблему, выполните указанные ниже действия.

**Виртуальные машины с несколькими сетевыми интерфейсами**

1. Добавьте сетевой интерфейс.
2. Устраните проблемы в поврежденном сетевом интерфейсе или удалите его.  Затем повторно добавьте сетевой интерфейс.

Дополнительные сведения см. в статье [Добавление или удаление сетевых интерфейсов виртуальных машин](virtual-network-network-interface-vm.md).

**Виртуальные машины с одним сетевым интерфейсом** 

- [Повторное развертывание виртуальной машины Windows](../virtual-machines/windows/redeploy-to-new-node.md)
- [Повторное развертывание виртуальной машины Linux](../virtual-machines/linux/redeploy-to-new-node.md)

### <a name="step-2-check-whether-network-traffic-is-blocked-by-nsg-or-udr"></a>Шаг 2. Проверьте, не блокируется ли трафик группой безопасности сети (NSG) или определяемым пользователем маршрутом (UDR)

Возможности [проверки IP-потока с помощью Наблюдателя за сетями](../network-watcher/network-watcher-ip-flow-verify-overview.md) и [ведения журнала потоков NSG](../network-watcher/network-watcher-nsg-flow-logging-overview.md) позволяют определить наличие группы безопасности сети или определяемого пользователем маршрута, которые влияют на поток трафика.

### <a name="step-3-check-whether-network-traffic-is-blocked-by-vm-firewall"></a>Шаг 3. Проверьте, не блокируется ли трафик брандмауэром виртуальной машины

Отключите брандмауэр и проверьте результат. Если проблема устранена, проверьте настройки брандмауэра и повторно включите его.

### <a name="step-4-check-whether-vm-app-or-service-is-listening-on-the-port"></a>Шаг 4. Проверьте, не ожидает ли передачи данных через порт приложение или служба виртуальной машины

Ниже приведены методы, которые позволяют проверить, прослушивается ли порт приложением или службой виртуальной машины.

- Чтобы проверить, прослушивается ли порт сервером, выполните команды ниже.

**Виртуальные машины Windows**

    netstat –ano

**Виртуальные машины Linux**

    netstat -l

- Выполните команду **telnet** на виртуальной машине, чтобы протестировать порт. Если проверка завершается сбоем, конфигурация приложения или службы не поддерживает прослушивание порта.

### <a name="step-5-check-whether-the-problem-is-caused-by-snat"></a>Шаг 5. Проверьте, не вызвана ли проблема SNAT

В некоторых сценариях виртуальная машина расположена за решением балансировки нагрузки, которое зависит от ресурсов за пределами Azure. Проблемы с промежуточными соединениями в этих сценариях могут возникать из-за [нехватки портов SNAT](../load-balancer/load-balancer-outbound-connections.md). Чтобы устранить проблему, создайте виртуальный IP-адрес для каждой виртуальной машины (или ILPIP-адрес для классической виртуальной машины), расположенной за подсистемой балансировки нагрузки и защищенной NSG или ACL. 

### <a name="step-6-check-whether-traffic-is-blocked-by-acls-for-the-classic-vm"></a>Шаг 6. Проверьте, блокируется ли трафик классической виртуальной машины списками управления доступом (ACL)

Список управления доступом дает возможность выборочно разрешать или блокировать трафик для конечной точки виртуальной машины. Дополнительные сведения см. в разделе [Управление ACL для конечной точки](/previous-versions/azure/virtual-machines/windows/classic/setup-endpoints#manage-the-acl-on-an-endpoint).

### <a name="step-7-check-whether-the-endpoint-is-created-for-the-classic-vm"></a>Шаг 7. Проверьте, создана ли конечная точка классической виртуальной машины

Все виртуальные машины, созданные в Azure с помощью классической модели развертывания, могут автоматически взаимодействовать с другими виртуальными машинами в той же облачной службе или виртуальной сети, используя канал частной сети. Однако компьютерам в других виртуальных сетях требуются конечные точки, чтобы направить входящий трафик к виртуальной машине. Дополнительные сведения см. в статье [Настройка конечных точек в классической виртуальной машине Windows в Azure](../virtual-machines/windows/classic/setup-endpoints.md).

### <a name="step-8-try-to-connect-to-a-vm-network-share"></a>Шаг 8. Попробуйте подключиться к сетевой папке виртуальной машины

Если вы не можете подключиться к сетевой папке виртуальной машины, проблема может быть вызвана недоступными сетевыми интерфейсами виртуальной машины. Сведения об удалении недоступных сетевых интерфейсов см. в [этом разделе](../virtual-machines/troubleshooting/reset-network-interface.md#delete-the-unavailable-nics).

### <a name="step-9-check-inter-vnet-connectivity"></a>Шаг 9. Проверьте подключение между виртуальными сетями

Возможности [проверки IP-потока с помощью Наблюдателя за сетями](../network-watcher/network-watcher-ip-flow-verify-overview.md) и [ведения журнала потоков NSG](../network-watcher/network-watcher-nsg-flow-logging-overview.md) позволяют определить наличие NSG или UDR, которые влияют на поток трафика. Вы также можете проверить подключение Inter-Vnet [здесь](https://support.microsoft.com/en-us/help/4032151/configuring-and-validating-vnet-or-vpn-connections).

### <a name="need-help-contact-support"></a>Требуется помощь? Обратитесь в службу поддержки.
Если вам все еще нужна помощь, [обратитесь в службу поддержки](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade), которая поможет быстро устранить проблему.