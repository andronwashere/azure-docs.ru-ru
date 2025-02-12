---
title: Таблица поддержки аварийного восстановления локальных виртуальных машин Hyper-V в Azure | Документация Майкрософт
description: Краткое описание поддерживаемых компонентов и требований для аварийного восстановления виртуальных машин Hyper-V в Azure с помощью службы Azure Site Recovery
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 05/30/2019
ms.author: raynew
ms.openlocfilehash: 78a531c769612af0597e732f0dc539286dd7cdac
ms.sourcegitcommit: bafb70af41ad1326adf3b7f8db50493e20a64926
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/25/2019
ms.locfileid: "68489876"
---
# <a name="support-matrix-for-disaster-recovery-of-on-premises-hyper-v-vms-to-azure"></a>Таблица поддержки аварийного восстановления локальных виртуальных машин Hyper-V в Azure


В этой статье перечислены компоненты и параметры, поддерживаемые для аварийного восстановления локальных виртуальных машин Hyper-V в Azure с помощью [Azure Site Recovery](site-recovery-overview.md).


## <a name="supported-scenarios"></a>Поддерживаемые сценарии использования.

**Сценарий** | **Сведения**
--- | ---
Hyper-V с Virtual Machine Manager | Вы можете выполнять аварийное восстановление в Azure для виртуальных машин, работающих на узлах Hyper-V под управлением структуры System Center Virtual Machine Manager.<br/><br/> Этот сценарий можно развернуть с помощью портала Azure или PowerShell.<br/><br/> Если узлами Hyper-V управляет Virtual Machine Manager, вы можете также выполнять аварийное восстановление на вторичный локальный сайт. Дополнительные сведения об этом сценарии см. в [этом руководстве](hyper-v-vmm-disaster-recovery.md).
Hyper-V без Virtual Machine Manager | Вы можете выполнять аварийное восстановление в Azure для виртуальных машин, работающих на узлах Hyper-V без управления Virtual Machine Manager.<br/><br/> Этот сценарий можно развернуть с помощью портала Azure или PowerShell.


## <a name="on-premises-servers"></a>Локальные серверы

**Server** | **Требования** | **Сведения**
--- | --- | ---
Hyper-V (без Virtual Machine Manager) |  Windows Server 2019, Windows Server 2016 (включая установку Server Core), Windows Server 2012 R2 с последними обновлениями | Если вы уже настроили Windows Server 2012 R2 или SCVMM 2012 R2 с помощью Azure Site Recovery и планируете обновить операционную систему, следуйте указаниям в [документации.](upgrade-2012R2-to-2016.md) 
Hyper-V (с Virtual Machine Manager) | Virtual Machine Manager 2019, Virtual Machine Manager 2016, Virtual Machine Manager 2012 R2 | Если используется Virtual Machine Manager, узлы Windows Server 2019 должны управляться в Virtual Machine Manager 2019. Аналогичным образом узлы Windows Server 2016 должны управляться в Virtual Machine Manager 2016.<br/><br/>


## <a name="replicated-vms"></a>Реплицированные виртуальные машины


В таблице ниже перечислены требования к виртуальной машине. Служба Site Recovery поддерживает все рабочие нагрузки в поддерживаемой операционной системе.

 **Компонент** | **Сведения**
--- | ---
Конфигурация виртуальной машины | Виртуальные машины, которые реплицируются в Azure, должны соответствовать [требованиям Azure](#azure-vm-requirements).
Операционная система на виртуальной машине | Любая гостевая ОС, [поддерживаемая в Azure](https://docs.microsoft.com/azure/cloud-services/cloud-services-guestos-update-matrix#family-5-releases).<br/><br/> Nano Server Windows Server 2016 не поддерживается.


## <a name="vmdisk-management"></a>Управление виртуальной машиной и диском

**Действие** | **Сведения**
--- | ---
Изменение размера диска на реплицируемой виртуальной машине Hyper-V | Не поддерживается. Отключите репликацию, внесите необходимые изменения, а затем снова включите репликацию для виртуальной машины.
Добавление диска к реплицируемой виртуальной машине Hyper-V | Не поддерживается. Отключите репликацию, внесите необходимые изменения, а затем снова включите репликацию для виртуальной машины.

## <a name="hyper-v-network-configuration"></a>Конфигурация сети Hyper-V

**Компонент** | **Hyper-V с Virtual Machine Manager** | **Hyper-V без Virtual Machine Manager**
--- | --- | ---
Сеть узла: объединение сетевых адаптеров | Да | Да
Сеть узла: Виртуальная ЛС | Да | Да
Сеть узла: IPv4 | Да | Да
Сеть узла: IPv6 | Нет | Нет
Сеть гостевых виртуальных машин: объединение сетевых адаптеров | Нет | Нет
Сеть гостевых виртуальных машин: IPv4 | Да | Да
Сеть гостевых виртуальных машин: IPv6 | Нет | Да
Сеть гостевых виртуальных машин: Статический IP-адрес (Windows) | Да | Да
Сеть гостевых виртуальных машин: Статический IP-адрес (Linux) | Нет | Нет
Сеть гостевых виртуальных машин: Несколько сетевых адаптеров | Да | Да



## <a name="azure-vm-network-configuration-after-failover"></a>Конфигурация сети виртуальных машин Azure (после отработки отказа)

**Компонент** | **Hyper-V с Virtual Machine Manager** | **Hyper-V без Virtual Machine Manager**
--- | --- | ---
Azure ExpressRoute | Да | Да
Внутренний балансировщик нагрузки | Да | Да
Внешний балансировщик нагрузки | Да | Да
Диспетчер трафика Azure | Да | Да
Несколько сетевых адаптеров | Да | Да
Зарезервированный IP-адрес | Да | Да
IPv4 | Да | Да
Сохранение исходного IP-адреса | Да | Да
Конечные точки служб для виртуальной сети Azure<br/> (без брандмауэров службы хранилища Azure) | Да | Да
Ускорение работы в сети | Нет | Нет


## <a name="hyper-v-host-storage"></a>Хранилище узла Hyper-V

**Хранилище** | **Hyper-V с Virtual Machine Manager** | **Hyper-V без Virtual Machine Manager**
--- | --- | --- 
NFS | Н/Д | Н/Д
SMB 3.0 | Да | Да
Сеть SAN (iSCSI) | Да | Да
Многопутевое (Multipath I/O). Протестировано с использованием:<br></br> Microsoft DSM, EMC PowerPath 5.7 SP4<br/><br/> EMC PowerPath DSM для CLARiiON | Да | Да

## <a name="hyper-v-vm-guest-storage"></a>Гостевое хранилище виртуальной машины Hyper-V

**Хранилище** | **Hyper-V с Virtual Machine Manager** | **Hyper-V без Virtual Machine Manager**
--- | --- | ---
VMDK | Н/Д | Н/Д
VHD (VHDX) | Да | Да
Виртуальная машина поколения 2 | Да | Да
UEFI (EFI)| Да | Да
Общий диск кластера | Нет | Нет
Зашифрованный диск | Нет | Нет
NFS | Н/Д | Н/Д
SMB 3.0 | Нет | Нет
RDM | Н/Д | Н/Д
Диски размером более 1 ТБ | Да, до 4095 ГБ | Да, до 4095 ГБ
Диск: логический и физический сектор размером 4 КБ | Не поддерживается: поколение 1, поколение 2 | Не поддерживается: поколение 1, поколение 2
Диск: логический сектор размером 4 КБ и физический сектор размером 512 байт | Да |  Да
Управление логическими томами (LVM). LVM поддерживается только на дисках данных. Azure предоставляет только один диск с ОС. | Да | Да
Том с чередующимся диском размером более 1 ТБ | Да | Да
Дисковые пространства | Нет | Нет
"Горячее" добавление или удаление диска | Нет | Нет
Исключение диска | Да | Да
Многопутевой (MPIO) | Да | Да

## <a name="azure-storage"></a>Служба хранилища Azure

**Компонент** | **Hyper-V с Virtual Machine Manager** | **Hyper-V без Virtual Machine Manager**
--- | --- | ---
Локально избыточное хранилище | Да | Да
Геоизбыточное хранилище | Да | Да
Геоизбыточное хранилище с доступом для чтения | Да | Да
"Холодное" хранилище | Нет | Нет
"Горячее" хранилище| Нет | Нет
Blob-блоки | Нет | Нет
Шифрование неактивных данных (SSE)| Да | Да
Хранилище уровня "Премиум" | Да | Да
Служба импорта и экспорта | Нет | Нет
Брандмауэры службы хранилища Azure для виртуальных сетей, настроенные в целевой учетной записи хранения или кэширования (используемой для хранения данных репликации) | Нет | Нет


## <a name="azure-compute-features"></a>Вычислительные компоненты Azure

**Возможность** | **Hyper-V с Virtual Machine Manager** | **Hyper-V без Virtual Machine Manager**
--- | --- | ---
Группы доступности | Да | Да
Концентратор | Да | Да  
Управляемые диски | Да, для отработки отказа.<br/><br/> Восстановление размещения для управляемых дисков не поддерживается. | Да, для отработки отказа.<br/><br/> Восстановление размещения для управляемых дисков не поддерживается.

## <a name="azure-vm-requirements"></a>Требования для виртуальных машин Azure

Локальные виртуальные машины, которые вы реплицируете в Azure, должны соответствовать требованиям Azure, приведенным в этой таблице.

**Компонент** | **Требования** | **Сведения**
--- | --- | ---
Операционная система на виртуальной машине | Служба Site Recovery поддерживает все операционные системы, [поддерживаемые Azure](https://technet.microsoft.com/library/cc794868%28v=ws.10%29.aspx).  | Если не поддерживается, проверка на соответствие обязательным требованиям завершается ошибкой.
Архитектура операционной системы на виртуальной машине | 64-разрядный | Если не поддерживается, проверка на соответствие обязательным требованиям завершается ошибкой.
Размер диска операционной системы | До 2048 ГБ для виртуальных машин поколения 1.<br/><br/> До 300 ГБ для виртуальных машин поколения 2  | Если не поддерживается, проверка на соответствие обязательным требованиям завершается ошибкой.
Число дисков операционной системы | 1 | Если не поддерживается, проверка на соответствие обязательным требованиям завершается ошибкой.
Число дисков с данными | 16 ГБ или меньше  | Если не поддерживается, проверка на соответствие обязательным требованиям завершается ошибкой.
Размер виртуального жесткого диска с данными | До 4095 ГБ | Если не поддерживается, проверка на соответствие обязательным требованиям завершается ошибкой.
Сетевые адаптеры | Поддерживаются несколько адаптеров |
Общий виртуальный жесткий диск | Не поддерживается | Если не поддерживается, проверка на соответствие обязательным требованиям завершается ошибкой.
Диск FC | Не поддерживается | Если не поддерживается, проверка на соответствие обязательным требованиям завершается ошибкой.
Формат жесткого диска | VHD <br/><br/> VHDX | Служба Site Recovery автоматически преобразует формат VHDX в VHD при отработке отказа в Azure. При восстановлении до локальной системы виртуальные машины продолжают использовать формат VHDX.
BitLocker | Не поддерживается | Прежде чем включать репликацию для виртуальной машины, необходимо отключить BitLocker.
Имя виртуальной машины | От 1 до 63 символов, при этом допустимы только буквы, цифры и дефисы Имя виртуальной машины должно начинаться и заканчиваться буквой или цифрой. | Обновите значение в свойствах виртуальной машины в службе Site Recovery.
Тип виртуальной машины | Поколение 1<br/><br/> Поколение 2 — Windows | Поддерживаются виртуальные машины второго поколения с диском ОС типа "Базовый" (включая один или два тома данных в формате VHDX) и объемом менее 300 ГБ.<br></br>Виртуальные машины Linux второго поколения не поддерживаются. [Узнайте больше](https://azure.microsoft.com/blog/2015/04/28/disaster-recovery-to-azure-enhanced-and-were-listening/).|

## <a name="recovery-services-vault-actions"></a>Действия хранилища служб восстановления

**Действие** |  **Hyper-V с Virtual Machine Manager** | **Hyper-V без Virtual Machine Manager**
--- | --- | ---
Перемещение хранилища между группами ресурсов<br/><br/> В пределах подписок и между ними | Нет | Нет
Перемещение хранилищ, сетей, виртуальных машин Azure между группами ресурсов<br/><br/> В пределах подписок и между ними | Нет | Нет

> [!NOTE]
> При репликации виртуальных машин Hyper-V (управляемые с или без SCVMM) в Azure вы можете реплицировать только один клиент AD с конкретной среды — сайта Hyper-V или SCVMM соответственно.


## <a name="provider-and-agent"></a>Поставщик и агент

Чтобы обеспечить совместимость развертывания с параметрами, описанными в этой статье, используйте последние версии поставщика и агента.

**Name** | **Описание** | **Сведения**
--- | --- | --- 
Поставщик Azure Site Recovery | Координирует взаимодействие между локальными серверами и Azure <br/><br/> Hyper-V с Virtual Machine Manager: установлено на серверы Virtual Machine Manager<br/><br/> Hyper-V без Virtual Machine Manager: установлено на узлах Hyper-V| Последняя версия: 5.1.2700.1 (доступна на портале Azure)<br/><br/> [Новейшие функции и последние исправления](https://support.microsoft.com/help/4091311/update-rollup-23-for-azure-site-recovery)
Агент служб восстановления Microsoft Azure | Координирует репликацию между виртуальными машинами Hyper-V и Azure.<br/><br/> Устанавливается на локальных серверах Hyper-V (с или без Virtual Machine Manager). | Последняя версия агента, доступная на портале






## <a name="next-steps"></a>Следующие шаги
Узнайте, как [подготовить Azure](tutorial-prepare-azure.md) для аварийного восстановления локальных виртуальных машин Hyper-V.
