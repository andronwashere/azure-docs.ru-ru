---
title: Распространенные вопросы о аварийном восстановлении из Azure в Azure с помощью Azure Site Recovery
description: В этой статье содержатся ответы на часто задаваемые вопросы о аварийном восстановлении виртуальных машин Azure в другой регион Azure с помощью Azure Site Recovery
author: asgang
manager: rochakm
ms.service: site-recovery
ms.date: 04/29/2019
ms.topic: conceptual
ms.author: asgang
ms.openlocfilehash: d479a568ddeac29be88d0709b7544ba645274afa
ms.sourcegitcommit: de47a27defce58b10ef998e8991a2294175d2098
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/15/2019
ms.locfileid: "67875661"
---
# <a name="common-questions-azure-to-azure-disaster-recovery"></a>Часто задаваемые вопросы: Аварийное восстановление из Azure в Azure

В этой статье приведены ответы на часто задаваемые вопросы об аварийном восстановлении виртуальных машин Azure в другой регион Azure с помощью [Site Recovery](site-recovery-overview.md). 


## <a name="general"></a>Общие сведения

### <a name="how-is-site-recovery-priced"></a>Как образуются цены на Site Recovery?
Ознакомьтесь со сведениями о [расценках на Azure Site Recovery](https://azure.microsoft.com/blog/know-exactly-how-much-it-will-cost-for-enabling-dr-to-your-azure-vm/).
### <a name="how-does-the-free-tier-for-azure-site-recovery-work"></a>Как работает бесплатный уровень Azure Site Recovery?
Для каждого экземпляра, защищенного с помощью Azure Site Recovery, предоставляется бесплатная защита в течение первого 31-дневного периода использования. Начиная с 32-го дня и далее плата за защиту экземпляра взимается по тарифам, указанным выше.
### <a name="during-the-first-31-days-will-i-incur-any-other-azure-charges"></a>Будет ли с меня взиматься плата за другие услуги Azure в течение первого 31-дневного периода?
Да, хотя для Azure Site Recovery предусмотрена бесплатная защита экземпляра в течение первого 31-дневного периода, с вас может взиматься плата за хранилище Azure, транзакции с хранилищем и передачу данных. При восстановлении виртуальной машины может взиматься плата за вычислительные операции Azure. Полные сведения о ценах см. [здесь](https://azure.microsoft.com/pricing/details/site-recovery)

### <a name="where-can-i-find-best-practices-for-azure-vm-disaster-recovery"></a>Где можно найти рекомендации по аварийному восстановлению виртуальной машины Azure? 
1. [Архитектура аварийного восстановления Azure-Azure](azure-to-azure-architecture.md)
2. [Поддерживаемые и неподдерживаемые конфигурации](azure-to-azure-support-matrix.md)
3. [Настройка аварийного восстановления для виртуальных машин Azure](azure-to-azure-how-to-enable-replication.md)
4. [Запуск тестовой отработки отказа](azure-to-azure-tutorial-dr-drill.md)
5. [Отработка отказа и восстановление размещения виртуальных машин Azure между регионами Azure](azure-to-azure-tutorial-failover-failback.md)

### <a name="how-is-capacity-guaranteed-in-the-target-region"></a>Как гарантируется емкость в целевом регионе?
Группа разработчиков Site Recovery работает с группой управления производительностью Azure, чтобы спланировать достаточную производительность инфраструктуры и гарантировать, что виртуальные машины, защищенные Site Recovery для, будут успешно развернуты в целевом регионе при запуске отработки отказа.

## <a name="replication"></a>Репликация

### <a name="can-i-replicate-vms-enabled-through-azure-disk-encryption"></a>Можно ли реплицировать виртуальные машины, включенные с помощью шифрования дисков Azure?
Да, их можно реплицировать. См. статью [Репликация виртуальных машин с поддержкой шифрования дисков Azure в другой регион Azure](azure-to-azure-how-to-enable-replication-ade-vms.md). В настоящее время Azure Site Recovery поддерживает только виртуальные машины Azure, работающие под управлением ОС Windows и включенные для шифрования с помощью приложений Azure Active Directory (Azure AD).

### <a name="can-i-replicate-vms-to-another-subscription"></a>Можно ли реплицировать виртуальные машины в другую подписку?
Да, вы можете реплицировать виртуальные машины Azure в другую подписку в том же клиенте Azure AD.
Настроить аварийное восстановление [между подписками](https://azure.microsoft.com/blog/cross-subscription-dr) просто. Вы можете выбрать другую подписку во время репликации.

### <a name="can-i-replicate-zone-pinned-azure-vms-to-another-region"></a>Можно ли реплицировать виртуальные машины Azure, прикрепленные к зонам, в другой регион?
Да, вы можете [реплицировать виртуальные машины, прикрепленные к зонам](https://azure.microsoft.com/blog/disaster-recovery-of-zone-pinned-azure-virtual-machines-to-another-region), в другой регион.

### <a name="can-i-exclude-disks"></a>Можно ли исключать диски?

Да, вы можете исключить диски во время защиты с помощью PowerShell. Дополнительные сведения см. в [статье](azure-to-azure-exclude-disks.md)

### <a name="can-i-add-new-disks-to-replicated-vms-and-enable-replication-for-them"></a>Можно ли добавить новые диски к реплицируемым виртуальным машинам и включить для них репликацию?

Да, это поддерживается для виртуальных машин Azure с управляемыми дисками. При добавлении нового диска на виртуальную машину Azure, для которой включена репликация, при репликации работоспособности виртуальной машины отображается предупреждение с заметкой о том, что один или несколько дисков на виртуальной машине доступны для защиты. Вы можете включить репликацию для добавленных дисков.
- Если включить защиту для добавленных дисков, предупреждение исчезнет после начальной репликации.
- Если вы решили не включать репликацию для диска, можно отклонить предупреждение.
- При отработки отказа виртуальной машины, к которой добавляется диск, и включения репликации, в точках репликации будут показаны диски, доступные для восстановления. Например, если виртуальная машины имеет один диск и вы добавляете новый, точки репликации, созданные перед добавлением диска, покажут, что точка репликации состоит из "1 из 2 дисков".

Site Recovery не поддерживает "горячее" Удаление диска из реплицированной виртуальной машины. При удалении диска виртуальной машины необходимо отключить и снова включить репликацию для виртуальной машины.


### <a name="how-often-can-i-replicate-to-azure"></a>Как часто можно выполнять репликацию в Azure?
Этот процесс является непрерывным при репликации виртуальных машин Azure в другой регион Azure. Дополнительные сведения см. в статье [Архитектура аварийного восстановления из Azure в Azure](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-architecture#replication-process).

### <a name="can-i-replicate-virtual-machines-within-a-region-i-need-this-to-migrate-vms"></a>Можно ли реплицировать виртуальные машины в одном регионе? Мне нужно знать это для переноса виртуальных машин.
Решение аварийного восстановления Azure — Azure нельзя использовать для репликации виртуальных машин в одном регионе.

### <a name="can-i-replicate-vms-to-any-azure-region"></a>Можно ли реплицировать виртуальные машины в любом регионе Azure?
С Site Recovery виртуальные машины можно реплицировать и восстанавливать между любыми двумя регионами в том же географическом кластере. Географические кластеры определены с учетом задержки и независимости данных. Дополнительные сведения см. в разделе [Поддержка регионов](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-support-matrix#region-support).

### <a name="does-site-recovery-require-internet-connectivity"></a>Требуется ли подключение к Интернету для Site Recovery?

Нет, Site Recovery не требует подключения к Интернету, но требуется доступ к URL-адресам Site Recovery и диапазонам IP-адресов, как упоминалось в [этой статье](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-about-networking#outbound-connectivity-for-ip-address-ranges).

### <a name="can-i-replicate-the-application-having-separate-resource-group-for-separate-tiers"></a>Могу ли я реплицировать приложение с отдельной группой ресурсов для отдельных уровней?
Да, вы можете реплицировать приложение и сохранить настройку аварийного восстановления также и для отдельной группы ресурсов.
Например, если у вас есть приложение с уровнем, базой данных и сетью в отдельной группе ресурсов, то вам нужно трижды щелкнуть [мастер репликации](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-how-to-enable-replication#enable-replication), чтобы защитить все уровни. Site Recovery будет реплицировать эти три уровня в трех разных группах ресурсов.

## <a name="replication-policy"></a>Политика репликации

### <a name="what-is-a-replication-policy"></a>Что такое политика репликации?
Определяет параметры для журнала хранения точек восстановления и периодичность создания моментальных снимков, совместимых на уровне приложений. По умолчанию Azure Site Recovery создает новую политику репликации с параметрами по умолчанию:

* 24 часа для хранения точек восстановления;
* 60 минут для периодичности создания моментальных снимков с согласованием приложений.

[Узнайте больше](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-tutorial-enable-replication#configure-replication-settings).

### <a name="what-is-a-crash-consistent-recovery-point"></a>Что такое отказоустойчивая точка восстановления?
Отказоустойчивая точка восстановления представляет данные на диске на момент, когда произошел сбой виртуальной машины или шнур питания был извлечен из сервера во время создания моментального снимка. Она не включает ничего, что было в памяти при создании моментального снимка.

В настоящее время большинство приложений может успешно восстанавливаться из отказоустойчивых моментальных снимков. Отказоустойчивой точки восстановления обычно достаточно для операционных систем (кроме баз данных), файловых серверов приложений, DHCP-серверов и серверов печати.

### <a name="what-is-the-frequency-of-crash-consistent-recovery-point-generation"></a>Какая частота создания отказоустойчивой точки восстановления?
Site Recovery создает отказоустойчивую точку восстановления каждые 5 минут.

### <a name="what-is-an-application-consistent-recovery-point"></a>Что такое точка восстановления, согласованная с приложением?
Согласованные с приложением точки восстановления создаются из согласованных с приложением моментальных снимков. Согласованные с приложением точки восстановления записывают те же данные, что и отказоустойчивые моментальные снимки, а также все данные в памяти и все выполняемые транзакции.
Из-за дополнительного содержимого моментальные снимки, согласованные с приложением, создаются дольше других. Согласованные с приложениями точки восстановления рекомендуется использовать для операционных систем баз данных и таких приложений, как SQL Server.

### <a name="what-is-the-impact-of-application-consistent-recovery-points-on-application-performance"></a>Как влияют согласованные с приложениями точки восстановления на производительность приложения?
Учитывая, что согласованные с приложением точки восстановления захватывают все активные данные и данные в памяти, то для остановки приложения требуется среда, подобная VSS в ОС Windows. При частом использовании это может повлиять на производительность, если наблюдается очень высокая рабочая нагрузка. Обычно рекомендуется не использовать низкую частоту для согласованных с приложением точек восстановления в рабочих нагрузках, которые не связаны с базой данных, для этого достаточно 1 часа.

### <a name="what-is-the-minimum-frequency-of-application-consistent-recovery-point-generation"></a>Какова минимальная частота создания точек восстановления, согласованных с приложением?
Site Recovery может создать последовательную точку восстановления с минимальной частотой в 1 час.

### <a name="how-are-recovery-points-generated-and-saved"></a>Каким образом точки восстановления создаются и сохраняются?
Чтобы понять, как Site Recovery создает точки восстановления, давайте рассмотрим пример политики репликации, которая предусматривает период хранения точек восстановления в 24 часа и периодичность создания моментальных снимков, согласованных с приложением, в 1 час.

Site Recovery создает отказоустойчивую точку восстановления каждые 5 минут. Пользователь не может изменить эту частоту. Таким образом, за последний час у пользователя будет 12 отказоустойчивых точек и 1 точка, согласованная с приложением. Со временем Site Recovery урезает все точки восстановления старше 1 часа и сохраняет только 1 точку восстановления в час.

На следующем снимке экрана показан пример. На снимке экрана:

1. для периода меньше 1 часа существуют точки восстановления с частотой в 5 минут;
2. для периода больше 1 часа Site Recovery сохраняет только 1 точку восстановления.

   ![Список созданных точек восстановления](./media/azure-to-azure-troubleshoot-errors/recoverypoints.png)


### <a name="how-far-back-can-i-recover"></a>До какой точки во времени можно выполнить восстановление?
Самая давняя доступная точка восстановления — 72 часа.

### <a name="what-will-happen-if-i-have-a-replication-policy-of-24-hours-and-a-problem-prevents-site-recovery-from-generating-recovery-points-for-more-than-24-hours-will-my-previous-recovery-points-be-lost"></a>Что произойдет, если при наличии политики репликации с периодом в 24 часа Site Recovery не сможет из-за проблемы создать точки восстановления в течение более 24 часов? Потеряю ли я предыдущие точки восстановления?
Нет. В этом случае Site Recovery будет хранить все предыдущие точки восстановления. В зависимости от периода хранения точек восстановления, в этом случае 24 часа, Site Recovery заменяет старые точки, только если есть новое поколение точек. В этом случае, поскольку не будет нового поколения точек восстановления из-за некоторых проблем, все старые точки не изменятся, когда закончится период хранения.

### <a name="after-replication-is-enabled-on-a-vm-how-do-i-change-the-replication-policy"></a>Как изменить политику репликации после включения репликации на виртуальной машине?
Перейдите в раздел **Хранилище Site Recovery** > **Инфраструктура Site Recovery** > **Политики репликации**. Выберите политику, которую требуется изменить, и сохраните изменения. Изменения будут применяться и к существующим репликациям.

### <a name="are-all-the-recovery-points-a-complete-copy-of-the-vm-or-a-differential"></a>Все точки восстановления представляют полную или разностную копию виртуальной машины?
Первая сгенерированная точка восстановления имеет полную копию. Все последующие точки восстановления имеют разностные изменения.

### <a name="does-increasing-the-retention-period-of-recovery-points-increase-the-storage-cost"></a>Увеличивает ли возрастание периода хранения точки восстановления затраты на хранилище?
Да. При увеличении срока хранения с 24 до 72 часов Site Recovery будет хранить точки восстановления дополнительные 48 часов. За дополнительное время хранения будет взиматься плата. Например, если одна точка восстановления содержит разностные изменения объемом 10 ГБ, а стоимость хранения за гигабайт составляет 0,16 долларов США в месяц, расходы увеличатся на 1,6 долларов США * 48 в месяц.

## <a name="multi-vm-consistency"></a>Согласованность нескольких виртуальных машин

### <a name="what-is-multi-vm-consistency"></a>Что такое согласованность нескольких виртуальных машин?
Это означает, что точка восстановления согласована на всех реплицированных виртуальных машинах.
В Site Recovery предусмотрен параметр "Согласованность нескольких виртуальных машин", при выборе которого создается группа репликации для совместной репликации всех компьютеров, которые являются частью группы.
На всех компьютерах в группе репликации будут общие отказоустойчивые и согласованные с приложением точки восстановления после отработки отказа.
Изучите руководство, чтобы [включить согласованность нескольких виртуальных машин](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-tutorial-enable-replication#enable-replication-for-a-vm).

### <a name="can-i-failover-single-virtual-machine-within-a-multi-vm-consistency-replication-group"></a>Можно ли выполнить отработку отказа одной виртуальной машины в группе репликации согласованности нескольких виртуальных машин?
Выбрав параметр "Согласованность нескольких виртуальных машин", вы заявляете, что приложение зависит от всех виртуальных машин в группе. Таким образом, отработка отказа одной виртуальной машины не допускается.

### <a name="how-many-virtual-machines-can-i-replicate-as-a-part-of-a-multi-vm-consistency-replication-group"></a>Сколько виртуальных машин можно реплицировать как часть группы репликации согласованности нескольких виртуальных машин?
Вы можете совместно реплицировать 16 виртуальных машин в группе репликации.

### <a name="when-should-i-enable-multi-vm-consistency-"></a>Когда следует включать согласованность нескольких виртуальных машин?
Из-за интенсивного использования ресурсов ЦП включение согласованности нескольких виртуальных машин может повлиять на производительность рабочей нагрузки. Поэтому ее необходимо использовать, только если компьютеры выполняют одну и ту же рабочую нагрузку и должны действовать согласованно несколько компьютеров. Например, если у вас есть два экземпляра SQL Server и два веб-сервера в приложении, у вас должна быть согласованность нескольких виртуальных машин только для экземпляров SQL Server.


## <a name="failover"></a>Отработка отказа

### <a name="how-is-capacity-assured-in-target-region-for-azure-vms"></a>Как поддается емкости в целевом регионе для виртуальных машин Azure?
Группа разработчиков Site Recovery работает с группой управления производительностью Azure, чтобы спланировать достаточную производительность инфраструктуры, чтобы гарантировать, что виртуальные машины, для которых включено аварийное восстановление, будут успешно развернуты в целевом регионе при запуске отработки отказа.

### <a name="is-failover-automatic"></a>Отработка отказа выполняется автоматически?

Отработка отказа не выполняется автоматически. Чтобы активировать ее, необходимо щелкнуть в соответствующем месте на портале или воспользоваться [PowerShell](azure-to-azure-powershell.md).

### <a name="can-i-retain-a-public-ip-address-after-failover"></a>Можно ли сохранить общедоступный IP-адрес после отработки отказа?

Общедоступный IP-адрес рабочего приложения не может быть сохранены после отработки отказа.
- Рабочим нагрузкам, возникающим в процессе отработки отказа, должен быть присвоен ресурс общедоступного IP-адреса Azure, доступный в целевом регионе.
- Это можно сделать вручную или автоматизировать с помощью плана восстановления.
- Узнайте, как [настроить общедоступные IP-адреса после](concepts-public-ip-address-with-site-recovery.md#public-ip-address-assignment-using-recovery-plan)отработки отказа.  

### <a name="can-i-retain-a-private-ip-address-during-failover"></a>Можно ли сохранить частный IP-адрес во время отработки отказа?
Да, вы можете оставаться частным IP-адресом. По умолчанию при включении аварийного восстановления для виртуальных машин Azure служба Site Recovery создает целевые ресурсы на основе параметров исходных ресурсов. — Для виртуальных машин Azure, настроенных со статическими IP-адресами, Site Recovery пытается подготавливать тот же IP-адрес для целевой виртуальной машины, если он не используется.
Узнайте, как [хранить IP-адреса во время](site-recovery-retain-ip-azure-vm-failover.md)отработки отказа.

### <a name="after-failover-why-is-the-server-assigned-a-new-ip-address"></a>Почему сервер назначает новый IP-адрес после отработки отказа?

Site Recovery пытается предоставить IP-адрес во время отработки отказа. Если другая виртуальная машина принимает этот адрес, Site Recovery устанавливает следующий доступный IP-адрес.
Дополнительные сведения о [настройке сетевого сопоставления и IP-адресации для виртуальных сетей](azure-to-azure-network-mapping.md#set-up-ip-addressing-for-target-vms).

### <a name="what-are-latest-lowest-rpo-recovery-points"></a>Что такое точка восстановления с меткой **Последняя (наименьшее значение RPO)** ?
Параметр **Последняя (наименьшее значение RPO)** сначала обрабатывает все данные, отправленные в службу Site Recovery, чтобы создать точку восстановления для каждой виртуальной машины, прежде чем выполнять в нее отработку отказа. Этот вариант обеспечивает самое низкое значение целевой точки восстановления (RPO), так как виртуальная машина, созданная после отработки отказа, имеет все данные, реплицированные в службу Site Recovery до момента активации отработки отказа.

### <a name="do-latest-lowest-rpo-recovery-points-have-an-impact-on-failover-rto"></a>Влияет ли точка восстановления с меткой **Последняя (наименьшее значение RPO)** на RTO отработки отказа?
Да. Site Recovery обрабатывает все ожидающие данные перед отработкой отказа, поэтому этот параметр имеет более высокое значение целевого времени восстановления (RTO) по сравнению с другими.

### <a name="what-does-the-latest-processed-option-in-recovery-points-mean"></a>Что означает параметр точки восстановления **Последняя обработанная**?
Параметр **Последняя обработанная** выполняет отработку отказа для всех виртуальных машин в плане в последнюю точку восстановления, которая была обработана Site Recovery. Для просмотра последней точки восстановления конкретной виртуальной машины ознакомьтесь с разделом **Последние точки восстановления** в параметрах виртуальной машины. Этот вариант обеспечивает низкий показатель RTO, так как не требует времени на обработку данных.

### <a name="what-happens-if-my-primary-region-experiences-an-unexpected-outage"></a>Что произойдет, если в основном регионе произойдет непредвиденный сбой?
Вы можете активировать отработку отказа после сбоя. Для отработки отказа службе Site Recovery не требуется подключение из основного региона.

### <a name="what-is-a-rto-of-a-vm-failover-"></a>Что такое RTO отработки отказа виртуальной машины?
Site Recovery имеет Соглашение об уровне обслуживания RTO на протяжении 2 часов. Для дополнительной информации см. [Соглашение об уровне обслуживания для Восстановление сайтов Azure](https://azure.microsoft.com/support/legal/sla/site-recovery/v1_2/). Однако в большинстве случаев Site Recovery выполняет отработку отказа виртуальных машин в течении нескольких минут. Вы можете рассчитать RTO, перейдя к заданиям отработки отказа, в которых указано время, необходимое для запуска виртуальной машины. Дополнительные сведения о плане восстановления RTO см. в разделе, приведенном ниже.

## <a name="recovery-plans"></a>Планы восстановления

### <a name="what-is-a-recovery-plan"></a>Что такое план восстановления?
План восстановления в Site Recovery управляет восстановлением виртуальных машин после отработки отказа. Они помогают реализовать точное, воспроизводимое и автоматическое восстановление. План восстановления решает следующие задачи пользователя.

- Определение группы виртуальных машин, для которых выполняется единая отработка отказа
- Определение зависимостей между виртуальными машинами для точного восстановления приложения
- Автоматизация восстановления и пользовательских действий, выполняемых вручную, чтобы можно было выполнить на виртуальных машинах другие задачи, кроме отработки отказа

[Узнайте подробнее](site-recovery-create-recovery-plans.md) о планах восстановления.

### <a name="how-is-sequencing-achieved-in-a-recovery-plan"></a>Каким образом в плане восстановления достигается виртуализация?

В плане восстановления можно создать несколько групп для достижения виртуализации. Отработка отказа каждой группы выполняется по очереди. Для виртуальных машин из одной группы отработка отказа выполняется одновременно, а затем следует другая группа. Чтобы узнать, как смоделировать приложение с помощью плана восстановления, обратитесь к статье [Сведения о планах восстановления](recovery-plan-overview.md#model-apps).

### <a name="how-can-i-find-the-rto-of-a-recovery-plan"></a>Как определить RTO плана восстановления?
Чтобы проверить RTO плана восстановления, протестируйте отработку отказа плана восстановления и перейдите к **заданиям Site Recovery**.
В следующем примере выполнение отработки отказа всех виртуальных машин и выполнение указанных действий задания SAPTestRecoveryPlan заняло 8 минут 59 секунд.

![Список заданий Site Recovery](./media/azure-to-azure-troubleshoot-errors/recoveryplanrto.PNG)

### <a name="can-i-add-automation-runbooks-to-the-recovery-plan"></a>Можно ли добавлять модули runbook службы автоматизации в планы восстановления?
Да, вы можете интегрировать в план восстановления модули runbook службы автоматизации Azure. [Узнайте больше](site-recovery-runbook-automation.md).

## <a name="reprotection-and-failback"></a>Повторное включение защиты и восстановление размещения

### <a name="after-a-failover-from-the-primary-region-to-a-disaster-recovery-region-are-vms-in-a-dr-region-protected-automatically"></a>Защищаются ли автоматически виртуальные машины в регионе аварийного восстановления после выполнения в него отработки отказа из основного региона?
Нет. При выполнении [отработки отказа](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-tutorial-failover-failback) виртуальных машин Azure из одного региона в другой их начальная загрузка в регионе аварийного восстановления не защищена. Чтобы восстановить размещение виртуальных машин в основном регионе, [повторно включите защиту](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-how-to-reprotect) виртуальных машин в дополнительном регионе.

### <a name="at-the-time-of-reprotection-does-site-recovery-replicate-complete-data-from-the-secondary-region-to-the-primary-region"></a>Реплицирует ли Site Recovery во время повторного включения защиты все данные из дополнительного региона в основной регион?
Это зависит от ситуации. Например, если исходный регион виртуальной машины существует, то синхронизируются только изменения между исходным и целевым диском. Site Recovery вычисляет различия путем сравнения дисков, а затем передает данные. Этот процесс обычно занимает несколько часов. Дополнительные сведения о том, что происходит во время восстановления, см. в статье [Повторное включение защиты виртуальных машин Azure в основном регионе после отработки отказа]( https://docs.microsoft.com/azure/site-recovery/azure-to-azure-how-to-reprotect#what-happens-during-reprotection).

### <a name="how-much-time-does-it-take-to-fail-back"></a>Сколько времени занимает восстановление размещения?
После повторного включения защиты время восстановления размещения обычно напоминает время, необходимое для отработки отказа из основного региона в дополнительный.

## <a name="capacity"></a>Ресурсов

### <a name="how-is-capacity-assured-in-target-region-for-azure-vms"></a>Как поддается емкости в целевом регионе для виртуальных машин Azure?
Группа разработчиков Site Recovery работает с группой управления производительностью Azure, чтобы спланировать достаточную производительность инфраструктуры, чтобы гарантировать, что виртуальные машины, для которых включено аварийное восстановление, будут успешно развернуты в целевом регионе при запуске отработки отказа.

### <a name="does-site-recovery-work-with-reserved-instances"></a>Site Recovery работает с зарезервированными экземплярами?
Да, вы можете приобрести [экземпляры резерва](https://azure.microsoft.com/pricing/reserved-vm-instances/) в регионе аварийного восстановления, а Site Recovery операции отработки отказа будут использовать их. </br> Никаких дополнительных настроек не требуется.


## <a name="security"></a>Безопасность

### <a name="is-replication-data-sent-to-the-site-recovery-service"></a>Отправляются ли данные репликации в службу Site Recovery?
Нет, Site Recovery не перехватывает реплицированные данные и не содержит сведений о том, что работает на ваших виртуальных машинах. В службу Site Recovery передаются только метаданные, необходимые для управления репликацией и отработкой отказа.  
Site Recovery имеет сертификаты ISO 27001:2013, 27018, HIPAA, DPA и сейчас проходит проверки на соответствие SOC2 и FedRAMP JAB.

### <a name="does-site-recovery-encrypt-replication"></a>Выполняет ли служба Site Recovery шифрование репликации?
Да, поддерживаются и шифрование, и [Шифрование неактивных в Azure](https://docs.microsoft.com/azure/storage/storage-service-encryption) .

## <a name="next-steps"></a>Следующие шаги
* [Ознакомьтесь с требованиями к поддержке.](azure-to-azure-support-matrix.md)
* [Настройка](azure-to-azure-tutorial-enable-replication.md) репликации из Azure в Azure.
- Если после прочтения статьи у вас возникли какие-либо вопросы, вы можете задать их на [форуме, посвященном службам восстановления Azure](https://social.msdn.microsoft.com/Forums/azure/home?forum=hypervrecovmgr).
