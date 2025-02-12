---
title: включение файла
description: включение файла
services: networking
author: anavinahar
ms.service: networking
ms.topic: include
ms.date: 06/25/2019
ms.author: anavin
ms.custom: include file
ms.openlocfilehash: 1fd08efd12e723a16445eba1d341f017a86a212e
ms.sourcegitcommit: a874064e903f845d755abffdb5eac4868b390de7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/24/2019
ms.locfileid: "68481542"
---
<a name="azure-resource-manager-virtual-networking-limits"></a>Ограничения сети. Azure Resource Manager следующие ограничения применяются только к сетевым ресурсам, управляемым с помощью **Azure Resource Manager** для каждого региона на подписку. Узнайте, как [просмотреть текущие данные об использовании ресурсов в соответствии с ограничениями подписки](../articles/networking/check-usage-against-limits.md).

> [!NOTE]
> Недавно мы увеличили все ограничения по умолчанию до максимального уровня. Если столбец максимального ограничения отсутствует, ресурс не имеет регулируемых ограничений. Если эти ограничения были увеличены за счет поддержки в прошлом и не видят обновленные ограничения в следующих таблицах, отправьте [запрос в службу поддержки по сети](../articles/azure-resource-manager/resource-manager-quota-errors.md) бесплатно.

| Resource | По умолчанию/максимальное ограничение | 
| --- | --- |
| Виртуальные сети |1000 |
| Подсетей на виртуальную сеть |3000 |
| Пиринг виртуальных сетей на виртуальную сеть |500 |
| DNS-серверов на виртуальную сеть |20 |
| Частных IP-адресов на виртуальную сеть |65 536 |
| Частные IP-адреса на сетевой интерфейс |256 |
| Частных IP-адресов на виртуальную машину |256 |
| Общедоступные IP-адреса на сетевой интерфейс |256 |
| Общедоступные IP-адреса на виртуальную машину |256 |
| Параллельные потоки TCP или UDP для каждого сетевого адаптера виртуальной машины или экземпляра роли |500,000 |
| Сетевые карты |65 536 |
| группы сетевой безопасности; |5 000 |
| Правил группы NSG на группу NSG |1000 |
| IP-адреса и диапазоны, указанные для источника или назначения в группе безопасности |4000 |
| Группы безопасности приложений |3000 |
| Группы безопасности приложений на IP-конфигурацию для каждого сетевого адаптера |20 |
| IP-конфигураций на группу безопасности приложения |4000 |
| Группы безопасности приложений, которые могут быть указаны в пределах всех правил безопасности группы безопасности сети |100 |
| Определяемые пользователем таблицы маршрутов |200 |
| Определяемые пользователем маршруты на таблицу маршрутов |400 |
| Корневые сертификаты типа "точка — сеть" для VPN-шлюза Azure |20 |
| Элементы TAP виртуальной сети |100 |
| Число конфигураций TAP сетевых интерфейсов на TAP виртуальной сети |100 |

#### <a name="publicip-address"></a>Ограничения для общедоступных IP-адресов
| Resource | Ограничение по умолчанию | Максимальное ограничение |
| --- | --- | --- |
| Общедоступные IP-адреса (динамические) | 1 000 для Basic. |Обратитесь в службу поддержки. |
| Общедоступные IP-адреса (статические) | 1 000 для Basic. |Обратитесь в службу поддержки. |
| Общедоступные IP-адреса (статические) | 200 для Standard.|Обратитесь в службу поддержки. |
| Длина префикса общедоступного IP-адреса | /28 | Обратитесь в службу поддержки. |

#### <a name="load-balancer"></a>Ограничения подсистемы балансировки нагрузки
Следующие ограничения применяются только к сетевым ресурсам, управление которыми осуществляется с помощью Azure Resource Manager для региона и на подписку. Узнайте, как [просмотреть текущие данные об использовании ресурсов в соответствии с ограничениями подписки](../articles/networking/check-usage-against-limits.md).

| Resource | По умолчанию/максимальное ограничение |
| --- | --- |
| Подсистемы балансировки нагрузки | 1000 | 
| Правил на ресурс, уровень "Базовый" | 250 |
| Правил на ресурс, уровень "Стандартный" | 1,500 | 
| Правил на IP-конфигурацию | 299 |
| Правил на сетевой адаптер | 300 |
| IP-конфигурации внешних интерфейсов, базовый | 200 |
| IP-конфигурации внешних интерфейсов, Стандартный | 600 |
| Пул серверной части, базовый | 100, одна группа доступности |
| Пул серверной части, Стандартный | 1 000, одна виртуальная сеть |
| Ресурсы серверной части на подсистему балансировки нагрузки, Стандартный<sup>1</sup> | 150 |
| Порты высокого уровня доступности, Standard | 1 для внутреннего внешнего интерфейса |

<sup>1</sup> Ограничение составляет до 150 ресурсов в любом сочетании автономных ресурсов виртуальной машины, ресурсов группы доступности и масштабируемых наборов виртуальных машин.

#### <a name="virtual-networking-limits-classic"></a>Следующие ограничения применяются только к сетевым ресурсам, управляемым с помощью **классической** модели развертывания на подписку. Узнайте, как [просмотреть текущие данные об использовании ресурсов в соответствии с ограничениями подписки](../articles/networking/check-usage-against-limits.md).

| Resource | Ограничение по умолчанию | Максимальное ограничение |
| --- | --- | --- |
| Виртуальные сети |100 |100 |
| Сайты локальной сети |20 |Обратитесь в службу поддержки. |
| DNS-серверов на виртуальную сеть |20 |20 |
| Частных IP-адресов на виртуальную сеть |4096 |4096 |
| Параллельные потоки TCP или UDP для каждого сетевого адаптера виртуальной машины или экземпляра роли |500 000, до 1 000 000 для двух или более сетевых карт. |500 000, до 1 000 000 для двух или более сетевых карт. |
| Группы безопасности сети (NSG) |200 |200 |
| Правил группы NSG на группу NSG |1000 |1000 |
| Определяемые пользователем таблицы маршрутов |200 |200 |
| Определяемые пользователем маршруты на таблицу маршрутов |400 |400 |
| Общедоступные IP-адреса (динамические) |500 |500 |
| Зарезервированные общедоступные IP-адреса |500 |500 |
| Виртуальный IP-адрес на развертывание |5 |Связаться со службой поддержки |
| Частный виртуальный IP-адрес (внутренняя балансировка нагрузки) для каждого развертывания |1 |1 |
| Списки управления доступом к конечным точкам (ACL) |50 |50 |
