---
title: Сведения об Azure Site Recovery | Документация Майкрософт
description: Обзор службы Azure Site Recovery и сводная информация о сценариях аварийного восстановления и развертывания при миграции.
services: site-recovery
author: rayne-wiselman
ms.service: site-recovery
ms.topic: overview
ms.date: 05/30/2019
ms.author: raynew
ms.custom: MVC
ms.openlocfilehash: 907d12464156f551930098f6bd6a6a24596307cb
ms.sourcegitcommit: cababb51721f6ab6b61dda6d18345514f074fb2e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/04/2019
ms.locfileid: "66479916"
---
# <a name="about-site-recovery"></a>Сведения о службе Site Recovery

Вас приветствует служба Azure Site Recovery. В этой статье содержится общий обзор службы.

Для организации необходимо принять стратегию непрерывности бизнеса и аварийного восстановления (BCDR), которая поможет сохранить ваши данные, а также поддерживать работоспособность приложений и рабочих нагрузок при планируемых и незапланированных сбоях.

Службы Azure Site Recovery помогают реализовать стратегию BCDR.

- **Служба Site Recovery**. Site Recovery помогает обеспечить непрерывность бизнеса, сохраняя работоспособность бизнес-приложений и рабочих нагрузок во время сбоев. Site Recovery реплицирует рабочие нагрузки, выполняемые на физических и виртуальных машинах, с основного сайта в дополнительное расположение. Если на вашем основном сайте произошел сбой, выполните отработку отказа во вторичное расположение и получите доступ к приложениям оттуда. После того как основное расположение снова станет доступным, вы сможете восстановить расположение.  
- **Служба Backup**. Служба [Azure Backup](https://docs.microsoft.com/azure/backup/) обеспечивает безопасное хранение данных и их восстановление с помощью резервного копирования в Azure.

Site Recovery управляет репликацией для:

- виртуальных машин Azure между регионами;
- Локальные виртуальные машины, виртуальные машины Azure Stack и физические серверы.


## <a name="what-does-site-recovery-provide"></a>Какие функции предоставляет служба Site Recovery?


**Компонент** | **Дополнительные сведения**
--- | ---
**Простое решение BCDR** | С помощью Site Recovery можно настраивать и администрировать репликацию, отработку отказа, а также восстановление размещения из одного расположения на портале Azure.
**Репликация виртуальной машины Azure** | Вы можете настроить аварийное восстановление виртуальных машин Azure из первичного региона в дополнительный.
**Репликация локальной виртуальной машины** | Вы можете реплицировать локальные виртуальные машины и физические серверы в Azure или дополнительный локальный центр обработки данных. Репликация в Azure позволит сократить затраты и отказаться от обременительного обслуживания дополнительного центра обработки данных.
**Репликация рабочей нагрузки** | Реплицируйте любые рабочие нагрузки, выполняющиеся на поддерживаемых виртуальных машинах Azure, локальных виртуальных машинах Hyper-V, виртуальных машинах VMware и физических серверах под управлением Windows и Linux.
**Устойчивость данных** | Site Recovery управляет репликацией, не мешая передаче данных приложений. Реплицированные данные хранятся в отказоустойчивой службе хранилища Azure. При отработке отказа виртуальные машины Azure создаются на основе реплицированных данных.
**Целевые показатели RTO и RPO** | Поддерживайте целевое время восстановления (RTO) и целевые точки восстановления (RPO) в пределах, установленных организацией. Site Recovery обеспечивает непрерывную репликацию для виртуальных машин Azure и виртуальных машин VMware. Для Hyper-V частота репликации составляет всего 30 секунд. Вы можете сократить RTO за счет интеграции с [диспетчером трафика Azure](https://azure.microsoft.com/blog/reduce-rto-by-using-azure-traffic-manager-with-azure-site-recovery/).
**Согласованность приложений при отработке отказа** | Вы можете выполнить репликацию с помощью точек восстановления с моментальными снимками, согласованных на уровне приложений. В этих моментальных снимках сохраняются расположенные на диске данные, а также вся информация в памяти и все выполняемые транзакции.
**Бесперебойное тестирование** | Вы можете легко запустить тренировочное аварийное восстановление. Это никак не отразится на выполняющейся репликации.
**Гибкая отработка отказа** | Вы можете запускать запланированные процедуры отработки отказа без потери данных при ожидаемых простоях, а также незапланированные процедуры отработки отказа с минимальной потерей данных (в зависимости от частоты репликации) на случай непредвиденных сбоев. Вы можете легко восстановить размещение на первичном сайте, как только он снова станет доступен.
**Настроенные планы восстановления** | С помощью планов восстановления можно настроить отработку отказа и восстановление, а также определить их последовательность для многоуровневых приложений на нескольких виртуальных машинах. Объедините машины в рамках плана восстановления, а также добавьте сценарии и действия, выполняемые вручную. Планы восстановления можно интегрировать с модулями Runbook службы автоматизации Azure.
**Интеграция BCDR** | Site Recovery интегрируется с другими технологиями BCDR. Например, можно использовать Site Recovery для защиты серверной части SQL Server корпоративных рабочих нагрузок со встроенной поддержкой для SQL Server AlwaysOn для управления отработкой отказа групп доступности.
**Интеграция службы автоматизации Azure** | Обширная библиотека службы автоматизации Azure содержит готовые к работе и учитывающие особенности приложений скрипты, которые можно скачать и интегрировать с помощью Site Recovery.
**Сетевая интеграция** | Site Recovery интегрируется с Azure, обеспечивая простое сетевое управление приложениями, включая резервирование IP-адресов, настройку подсистем балансировки нагрузки и интеграцию диспетчера трафика Azure для эффективного переключения сетей.


## <a name="what-can-i-replicate"></a>Что можно реплицировать?

**Поддерживаются** | **Дополнительные сведения**
--- | ---
**Сценарии репликации** | Реплицируйте виртуальные машины Azure из одного региона Azure в другой.<br/><br/>  Реплицируйте локальные виртуальные машины VMware, виртуальные машины Hyper-V, физические серверы (Windows и Linux) и виртуальные машины Azure Stack в Azure.<br/><br/> <br/><br/> Реплицируйте экземпляры AWS Windows в Azure.<br/><br/> Реплицируйте физические серверы, локальные виртуальные машины VMware, а также виртуальные машины Hyper-V под управлением System Center VMM на дополнительный сайт.
**Регионы** | Ознакомьтесь с [поддерживаемыми регионами](https://azure.microsoft.com/regions/services/) для Site Recovery. |
**Реплицируемые компьютеры** | Ознакомьтесь с требованиями для репликации [виртуальных машин Azure](azure-to-azure-support-matrix.md#replicated-machine-operating-systems), [локальных виртуальных машин VMware и физических серверов](vmware-physical-azure-support-matrix.md#replicated-machines), а также [локальных виртуальных машин Hyper-V](hyper-v-azure-support-matrix.md#replicated-vms).
**Рабочие нагрузки** | Вы можете реплицировать любую рабочую нагрузку, выполняемую на поддерживающем репликацию компьютере. Кроме того, команда Site Recovery выполнила специальную проверку [ряда приложений](site-recovery-workload.md#workload-summary).



## <a name="next-steps"></a>Дополнительная информация
* [Какие рабочие нагрузки можно защитить с помощью службы Azure Site Recovery?](site-recovery-workload.md)
* [Репликация виртуальной машины Azure в другой регион Azure (предварительная версия)](azure-to-azure-quickstart.md) 
