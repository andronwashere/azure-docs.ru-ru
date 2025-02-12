---
title: Анализ отчета Планировщика развертывания Azure Site Recovery для аварийного восстановления VMware в Azure | Документация Майкрософт
description: В этой статье описано, как проанализировать отчет, сформированный Планировщиком развертывания Azure Site Recovery для аварийного развертывания VMware в Azure.
author: mayurigupta13
manager: rochakm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 3/20/2019
ms.author: mayg
ms.openlocfilehash: cbea6785239c70a3cdb229d0811497f051224238
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "61472613"
---
# <a name="analyze-the-azure-site-recovery-deployment-planner-report-for-vmware-disaster-recovery-to-azure"></a>Анализ отчета Планировщика развертывания Azure Site Recovery для аварийного восстановления VMware в Azure

Созданный отчет Microsoft Excel содержит описанные ниже листы.
## <a name="on-premises-summary"></a>Сводка по локальной инфраструктуре
На листе On-premises summary (Сводка по локальной инфраструктуре) содержатся общие сведения о профилированной среде VMware.

![Сводка по локальной инфраструктуре для среды VMware](media/site-recovery-vmware-deployment-planner-analyze-report/on-premises-summary-v2a.png)

**Дата начала** и **дата окончания**. Это даты начала и окончания профилирования данных, используемых при создании отчета. По умолчанию "Дата начала" — это дата начала профилирования, а "Дата окончания" — это дата окончания. При создании отчета с этими параметрами им соответствуют значения StartDate и EndDate.

**Total number of profiling days** (Общее число дней профилирования). Общее число дней профилирования между датами начала и окончания, для которых создается отчет.

**Number of compatible virtual machines** (Количество совместимых виртуальных машин). Это общее число совместимых виртуальных машин, использующих пропускную способность сети, а также необходимое число учетных записей хранения, ядер Microsoft Azure, серверов конфигурации и дополнительных серверов обработки.

**Total number of disks across all compatible virtual machines** (Общее число дисков на всех совместимых виртуальных машинах). Это значение используется в качестве одного из входных параметров при определении необходимого числа серверов конфигурации и дополнительных серверов обработки, используемых во время развертывания.

**Average number of disks per compatible virtual machine** (Среднее число дисков на совместимую виртуальную машину). Среднее число дисков, рассчитанное по всем совместимым виртуальным машинам.

**Average disk size (GB)** (Средний размер диска (ГБ)). Средний размер диска, вычисленный для всех совместимых виртуальных машин.

**Desired RPO (minutes)** (Требуемое значение RPO (мин)). Значение целевой точки восстановления по умолчанию или значение, заданное для параметра DesiredRPO во время создания отчета для оценки необходимой пропускной способности.

**Desired bandwidth (Mbps)** (Требуемая пропускная способность (Мбит/с)). Значение, заданное для параметра Bandwidth во время создания отчета для оценки достигаемого значения RPO.

**Observed typical data churn per day (GB)** (Стандартная активность обработки данных в день (ГБ)) Среднее значение скорости обработки данных по всем дням профилирования. Это значение используется в качестве одного из входных параметров при определении необходимого числа серверов конфигурации и дополнительных серверов обработки, используемых во время развертывания.

## <a name="recommendations"></a>Рекомендации

>[!Note]
>При репликации непосредственно в управляемые диски, игнорируйте рекомендации для учетных записей хранения.

Лист рекомендаций отчета по развертыванию VMware в Azure содержит следующие сведения с учетом выбранного требуемого значения RPO.

![Рекомендации для отчета по развертыванию VMware в Azure](media/site-recovery-vmware-deployment-planner-analyze-report/Recommendations-v2a.png)

### <a name="profiled-data"></a>Данные профилирования
![Представления данных профилирования в планировщике ресурсов](media/site-recovery-vmware-deployment-planner-analyze-report/profiled-data-v2a.png)

**Profiled data period** (Период профилирования данных). Длительность выполнения профилирования. По умолчанию программа использует для вычисления все данные профилирования, за исключением случаев, когда создается отчет для определенного периода времени с использованием параметров StartDate и EndDate.

**Имя сервера**. Имя или IP-адрес сервера VMware vCenter Server или узла ESXi, для виртуальных машин которого создается отчет.

**Desired RPO** (Требуемое значение RPO). Значение целевой точки восстановления для развертывания. По умолчанию необходимая пропускная способность рассчитывается для значений RPO, равных 15, 30 и 60 минутам. В зависимости от выбора в отчете обновляются затронутые значения. Если во время создания отчета вы использовали параметр *DesiredRPOinMin*, это значение отображается в результатах по параметру Desired RPO (Требуемое значение RPO).

### <a name="profiling-overview"></a>Общие сведения о профилировании

![Результаты профилирования в планировщике ресурсов](media/site-recovery-vmware-deployment-planner-analyze-report/profiling-overview-v2a.png)

**Total Profiled Virtual Machines** (Общее число профилируемых виртуальных машин). Общее количество виртуальных машин, по которым доступны данные профилирования. Если в качестве значений параметра VMListFile заданы виртуальные машины, профилирование которых не выполнялось, они не учитываются при создании отчета и исключаются из общего числа профилированных виртуальных машин.

**Compatible Virtual Machines** (Совместимых виртуальных машин). Количество виртуальных машин, которые можно защитить в Azure с помощью Site Recovery. Это общее число совместимых виртуальных машин, использующих пропускную способность сети, а также необходимое число учетных записей хранения, ядер Azure, серверов конфигурации и дополнительных серверов обработки. Сведения о каждой совместимой виртуальной машине см. в соответствующем разделе.

**Incompatible Virtual Machines** (Несовместимых виртуальных машин). Число профилируемых виртуальных машин, которые нельзя защитить с помощью Site Recovery. Причины несовместимости см. в разделе "Несовместимые виртуальные машины". Если в качестве значений параметра VMListFile заданы виртуальные машины, профилирование которых не выполнялось, они исключаются из общего числа несовместимых виртуальных машин. Эти виртуальные машины перечислены в списке Data not found (Данные не найдены) в нижней части раздела Incompatible Virtual Machines (Несовместимые виртуальные машины).

**Desired RPO** (Требуемое значение RPO). Требуемая целевая точка восстановления, выраженная в минутах. Отчет создается на основе трех значений RPO: 15 (по умолчанию), 30 и 60 минут. Рекомендации относительно пропускной способности изменяются в отчете в зависимости от значения, выбранного в раскрывающемся списке Desired RPO (Требуемое значение RPO). Этот список расположен в верхнем правом углу страницы. Если при создании отчета вы использовали другое значение параметра *-DesiredRPO*, оно будет отображаться в этом раскрывающемся списке в качестве значения по умолчанию.

### <a name="required-network-bandwidth-mbps"></a>Требуемая пропускная способность сети (Мбит/с)

![Требуемая пропускная способность сети в планировщике ресурсов](media/site-recovery-vmware-deployment-planner-analyze-report/required-network-bandwidth-v2a.png)

**To meet RPO 100 percent of the time** (Соблюдение требований к RPO 100 % времени). Рекомендуемая пропускная способность (в Мбит/с), необходимая для соблюдения требований к RPO в течение 100 % времени. Это объем пропускной способности, необходимый для разностной репликации в устойчивом состоянии всех совместимых виртуальных машин, чтобы избежать каких-либо нарушений требований к RPO.

**To meet RPO 90 percent of the time** (Соблюдение требований к RPO 90 % времени). Из-за цены на высокоскоростное подключение или по какой-либо другой причине, когда вы не можете установить объем пропускной способности, необходимый для соблюдения требований к RPO в 100 % случаев, вы можете установить меньший объем пропускной способности, необходимый для соблюдения требований к RPO в 90 % случаев. Чтобы оценить влияние установки меньшего объема, в отчете можно просмотреть сведения о возможном количестве нарушений требований к RPO и их продолжительности.

**Achieved Throughput** (Достигнутая пропускная способность). Это пропускная способность сервера, на котором вы выполнили команду GetThroughput, доступная для региона Microsoft Azure, где расположена учетная запись хранения. Это значение указывает примерную пропускную способность, которой можно достичь при защите совместимых виртуальных машин с помощью Site Recovery при условии, что характеристики хранилища и сети сервера конфигурации и сервера обработки такие же, как и на сервере, на котором вы запустили программу.

При выполнении репликации нужно установить рекомендуемый объем пропускной способности, необходимый для соблюдения требований к RPO в 100 % случаев. Если после установки пропускной способности результаты в отчете не улучшились, сделайте следующее:

1. Проверьте наличие требований к качеству обслуживания сети, которые ограничивают пропускную способность Site Recovery.

2. Проверьте, находится ли хранилище Site Recovery в ближайшем поддерживаемом физическом регионе Microsoft Azure, чтобы минимизировать сетевые задержки.

3. Проверьте характеристики локального хранилища, чтобы определить, можно ли улучшить оборудование (например, замените жесткие диски на твердотельные).

4. Измените параметры Site Recovery на сервере обработки, чтобы [увеличить объем пропускной способности сети, необходимой для репликации](./site-recovery-plan-capacity-vmware.md#control-network-bandwidth).

Если вы запустили программу на сервере конфигурации или сервере обработки, на котором уже расположены защищенные виртуальные машины, запустите ее еще несколько раз, так как значение достигаемой пропускной способности будет изменяться в зависимости от активности обработки данных в определенный момент времени.

Для всех развертываний Site Recovery в корпоративных средах мы рекомендуем использовать [ExpressRoute](https://aka.ms/expressroute).

### <a name="required-storage-accounts"></a>Диаграмма Required Azure Storage Accounts (Требуемые учетные записи хранения Azure)
На следующей диаграмме показано общее число учетных записей хранения Azure (уровня Standard и Premium), необходимых для защиты всех совместимых виртуальных машин. Чтобы узнать, какую учетную запись хранения необходимо использовать для каждой виртуальной машины, см. раздел "Размещение хранилища виртуальной машины".

![Требуемые учетные записи хранения в планировщике развертывания](media/site-recovery-vmware-deployment-planner-analyze-report/required-storage-accounts-v2a.png)

### <a name="required-number-of-azure-cores"></a>Область Required Number of Azure Cores (Необходимое число ядер Azure)
Это значение указывает на общее число ядер, которое необходимо задать перед отработкой отказа (или тестовой отработкой отказа) всех совместимых виртуальных машин. Если в подписке доступно слишком мало ядер, Site Recovery не сможет создать виртуальные машины и этот процесс завершится сбоем во время отработки отказа (или тестовой отработки отказа).

![Требуемое количество ядер Azure в планировщике ресурсов](media/site-recovery-vmware-deployment-planner-analyze-report/required-cores-v2a.png)

### <a name="required-on-premises-infrastructure"></a>Область Required On-premises Infrastructure (Необходимая локальная инфраструктура)
Это значение указывает на общее число серверов конфигурации и дополнительных серверов обработки, которых хватит для защиты всех совместимых виртуальных машин. В зависимости от поддерживаемых [рекомендаций по размеру сервера конфигурации](https://aka.ms/asr-v2a-on-prem-components), например рекомендаций по большему значению объема обработки данных в день или максимальному количеству защищенных виртуальных машин (в зависимости от значения, которое достигается первым, при условии, что к каждой виртуальной машине подключено три диска), на сервере конфигурации или дополнительном сервере планировщик может порекомендовать использовать дополнительные серверы. Сведения об общем количестве данных, обработанных за день, и общем числе защищенных дисков см. в разделе "Сводка по локальной инфраструктуре".

![Требуемая локальная инфраструктура в планировщике ресурсов](media/site-recovery-vmware-deployment-planner-analyze-report/required-on-premises-components-v2a.png)

### <a name="what-if-analysis"></a>Анализ "что если"
В этом анализе отображаются сведения о возможном количестве нарушений, которые могут произойти в процессе профилирования при установке меньшего объема пропускной способности, необходимой для соблюдения требований к RPO только в 90 % случаев. Это может быть одно или несколько нарушений, которые могут произойти в любой заданный день. На диаграмме показано максимальное значение RPO за день.
По результатам анализа вы можете определить, допустимо ли количество нарушений требований к RPO за все время и максимальное значение RPO за день для выделенного объема пропускной способности. В зависимости от результатов вы можете выделить меньший объем пропускной способности для репликации или повысить ее пределы для соблюдения требований к RPO в 100 % случаев.

![Анализ "что если" в планировщике ресурсов](media/site-recovery-vmware-deployment-planner-analyze-report/what-if-analysis-v2a.png)

### <a name="recommended-vm-batch-size-for-initial-replication"></a>Recommended VM batch size for initial replication (Рекомендуемый размер пакета виртуальных машин для начальной репликации)
В этом разделе приведено рекомендуемое число виртуальных машин, которые можно защищать в параллельном режиме, чтобы завершить начальную репликацию в течение 72 часов с пропускной способностью, необходимой для соблюдения требований к RPO в 100 % случаев. Это значение можно изменить во время создания отчетов с помощью параметра *GoalToCompleteIR*.

На диаграмме отображается диапазон значений пропускной способности и рекомендуемый размер пакета виртуальных машин, необходимые для завершения начальной репликации в течение 72 часов. Размер пакета определяется на основе среднего размера всех совместимых виртуальных машин.

В общедоступной предварительной версии в отчет не входят сведения о том, какие виртуальные машины необходимо добавить в пакет. Вы можете просмотреть размер каждой виртуальной машины в разделе "Совместимые виртуальные машины", а затем на основе этих сведений или известных характеристик рабочей нагрузки сформировать пакет. Время выполнения начальной репликации изменяется пропорционально в зависимости от фактического размера диска виртуальной машины, используемого дискового пространства и доступной пропускной способности сети.

![Рекомендуемый размер пакета виртуальных машин](media/site-recovery-vmware-deployment-planner-analyze-report/ir-batching-v2a.png)

### <a name="cost-estimation"></a>Оценка затрат
На следующих диаграммах представлена сводная оценка общих затрат на аварийное восстановление (DR) в Azure для выбранного целевого региона и валюты, которые вы указали при создании отчета.

![Результаты оценки затрат](media/site-recovery-vmware-deployment-planner-analyze-report/cost-estimation-summary-v2a.png)

Сводка поможет вам составить представление о затратах на хранилище, вычисления, сеть и лицензии при защите всех совместимых виртуальных машин в Azure с помощью Azure Site Recovery. Затраты рассчитываются только для совместимых, а не для всех профилируемых виртуальных машин.  

Можно просмотреть расходы за месяц или за год. Узнайте больше о поддерживаемых [целевых регионах](./site-recovery-vmware-deployment-planner-cost-estimation.md#supported-target-regions) и [валютах](./site-recovery-vmware-deployment-planner-cost-estimation.md#supported-currencies).

**Cost by components** (Затраты по составляющим). Расходы на аварийное восстановление разделены на четыре составляющие: стоимость вычислительных ресурсов, хранилища, сети и лицензии Azure Site Recovery. Затраты рассчитываются на основе данных потребления при репликации и тестировании DR. Сюда входит стоимость вычислительных ресурсов, хранилища (классы Premium и Standard), подключения ExpressRoute или VPN, настроенного между локальной средой и Azure, а также лицензии Azure Site Recovery.

**Cost by states** (Затраты по состояниям). Общие расходы на аварийное восстановление (DR) рассчитываются на основе двух состояний: репликации и тестирования DR.

**Replication cost** (Затраты на репликацию).  Стоимость репликации. Сюда входит стоимость хранилища, сети и лицензии Azure Site Recovery.

**DR-Drill cost** (Затраты на тестирование аварийного восстановления). Стоимость тестовой отработки отказа. Azure Site Recovery запускает виртуальные машины при тестовой отработке отказа. Затраты на тестирование DR включают расходы на вычислительные ресурсы и хранилище для работающих виртуальных машин.

**Azure storage cost per Month/Year** (Месячные или годовые затраты на хранилище). Это общие расходы на хранилище классов Premium и Standard при репликации и тестировании DR.
Подробный анализ затрат на виртуальную машину см. на листе [оценки затрат](site-recovery-vmware-deployment-planner-cost-estimation.md).

### <a name="growth-factor-and-percentile-values-used"></a>Коэффициент роста и используемые значения процентилей
Этот раздел находится в нижней части страницы. В нем содержатся сведения о значении процентиля, используемом для всех счетчиков производительности профилированных виртуальных машин (по умолчанию 95-й процентиль), а также о коэффициенте роста, используемом во всех вычислениях (по умолчанию — 30 %).

![Коэффициент роста и используемые значения процентилей](media/site-recovery-vmware-deployment-planner-analyze-report/growth-factor-v2a.png)

## <a name="recommendations-with-available-bandwidth-as-input"></a>Рекомендации по вводу значения доступной пропускной способности

![Рекомендации по вводу значения доступной пропускной способности](media/site-recovery-vmware-deployment-planner-analyze-report/profiling-overview-bandwidth-input-v2a.png)

Возможны ситуации, когда вы не сможете установить для репликации Site Recovery достаточный объем пропускной способности. Эта программа позволяет вам во время создания отчета ввести значение доступной пропускной способности (с помощью параметра -Bandwidth) и получить достигаемое значение RPO в минутах. На основе полученного значения вы можете определить, нужно ли установить дополнительный объем пропускной способности или можно просто создать решение для аварийного восстановления.

![Достигаемое значение RPO для пропускной способности 500 Мбит/с](media/site-recovery-vmware-deployment-planner-analyze-report/achievable-rpo-v2a.png)

## <a name="vm-storage-placement"></a>Размещение хранилища виртуальной машины

>[!Note]
>При репликации непосредственно в управляемые диски, вы не обязательно должны беспокоиться о количество учетных записей хранения. Для хранения используйте только рекомендации на тип хранилища ("стандартный" или "премиум"). Для управляемых дисков применяется тот же тип.

![Размещение хранилища виртуальной машины](media/site-recovery-vmware-deployment-planner-analyze-report/vm-storage-placement-v2a.png)

**Disk Storage Type** (Тип хранилища дисков). Учетная запись хранилища класса Standard или Premium, используемая для репликации всех виртуальных машин, указанных в столбце **VMs to Place** (Виртуальные машины для размещения).

**Suggested Prefix** (Рекомендуемый префикс). Рекомендуемый префикс из трех символов, который можно использовать для создания имени учетной записи хранения. Вы можете использовать собственный префикс, но для программы мы рекомендуем использовать префикс, соответствующий требованиям [соглашения об именовании секций учетных записей хранения](https://aka.ms/storage-performance-checklist).

**Suggested Account Name** (Рекомендуемое имя учетной записи). Имя учетной записи хранения с добавлением рекомендуемого префикса. Замените имя в угловых скобках (< и >) именем своей учетной записи.

**Log Storage Account** (Учетная запись хранения журналов). Все журналы репликации хранятся в учетной записи хранилища класса Standard. Если репликация виртуальных машин выполняется в учетную запись хранения уровня Premium, для хранения журналов необходимо настроить дополнительную учетную запись уровня Standard. Одна учетная запись хранения журналов уровня Standard может использоваться несколькими учетными записями хранения репликации уровня Premium. Виртуальные машины, реплицированные в учетные записи хранения уровня Standard, используют ту же учетную запись хранения для журналов.

**Suggested Log Account Name** (Рекомендуемое имя учетной записи журналов). Имя учетной записи хранения журналов с добавлением рекомендуемого префикса. Замените имя в угловых скобках (< и >) именем своей учетной записи.

**Placement Summary** (Сводка по размещению). Сводные данные об общем числе виртуальных машин, передаваемых в учетную запись хранения во время репликации и отработки отказа (или тестовой отработки отказа). В этом столбце содержатся сведения об общем числе виртуальных машин, сопоставленных с учетной записью хранения, общем количестве операций записи и чтения на диск в секунду на всех виртуальных машинах, размещенных в этой учетной записи, общем количестве операций ввода-вывода в секунду при записи (репликации), общем объеме установленного пространства на всех дисках и общем числе дисков.

**Virtual Machines to Place** (Виртуальные машины для размещения). Список всех виртуальных машин, которые необходимо поместить в указанную учетную запись хранения для обеспечения оптимальной производительности и эффективного использования.

## <a name="compatible-vms"></a>Совместимые виртуальные машины
![Электронная таблица Excel с совместимыми виртуальными машинами](media/site-recovery-vmware-deployment-planner-analyze-report/compatible-vms-v2a.png)

**Имя виртуальной машины**. Имя или IP-адрес виртуальной машины, заданные в качестве значения параметра VMListFile во время создания отчета. В этом столбце также перечислены диски (VMDK-файлы), присоединенные к виртуальным машинам. Чтобы различать виртуальные машины, для виртуальных машин в vCenter с повторяющимися именами или IP-адресами указывается имя узла ESXi. Это узел ESXi, на котором размещалась виртуальная машина, когда она была впервые обнаружена с помощью средства в процессе профилирования.

**VM Compatibility** (Совместимость виртуальной машины). Допускаются значения **Да** и **Да**\*. Значение **Да**\* используется в тех случаях, когда виртуальная машина подготавливается для [SSD (цен. категория "Премиум")](../virtual-machines/windows/disks-types.md). За счет высокой активности обработки данных и обработки большого числа операций ввода-вывода в секунду на диске она подходит для категории P20 или P30, но из-за размера диска она сопоставляется с категорией P10 или P20. Служба хранилища Azure определяет, с каким типом диска хранилища класса Premium сопоставить диск виртуальной машины на основе его размера. Пример:
* если емкость диска составляет меньше 128 ГБ, он сопоставляется с категорией P10;
* если емкость диска составляет от 128 до 256 ГБ, он сопоставляется с категорией P15;
* если емкость диска составляет от 256 до 512 ГБ, он сопоставляется с категорией P20;
* если емкость диска составляет от 512 до 1024 ГБ, он сопоставляется с категорией P30;
* если емкость диска составляет от 1025 до 2048 ГБ, он сопоставляется с категорией P40;
* если емкость диска составляет от 2049 до 4095 ГБ, он сопоставляется с категорией P50.

Например, если из-за характеристик рабочей нагрузки диска виртуальная машина подпадает под категорию P20 или P30, но из-за своего размера этот диск сопоставляется с типом диска хранилища класса Premium более низкой категории, программа добавляет к этой виртуальной машине метку **Да**\*. и рекомендует вам либо изменить размер исходного диска для соответствия рекомендуемому типу диска хранилища класса Premium, либо изменить тип целевого диска после отработки отказа.

**Storage Type** (Тип хранилища). "Стандартный" или "Премиум".

**Suggested Prefix** (Рекомендуемый префикс). Префикс имени учетной записи хранения, состоящий из трех символов.

**Storage Account** (Учетная запись хранения). Имя учетной записи хранения с добавлением рекомендуемого префикса.

**Peak R/W IOPS (with Growth Factor)** (Пиковое число операций ввода-вывода в секунду с учетом коэффициента роста). Пиковое значение числа операций ввода-вывода в секунду при чтении и записи на диск (по умолчанию используется 95-й процентиль) с учетом коэффициента будущего роста (по умолчанию используется значение 30 %). Обратите внимание, что общее число операций записи и чтения на диск в секунду не всегда вычисляется путем сложения операций записи и чтения каждого отдельного диска виртуальной машины, так как максимальное число операций записи и чтения в секунду виртуальной машины состоит из максимальных значений каждого отдельного диска в течение каждой минуты процесса профилирования.

**Peak Data Churn in Mbps (with Growth Factor)** (Пиковая скорость обработки данных (Мбит/с) с учетом коэффициента роста). Пиковое значение скорости обработки данных для диска (по умолчанию используется 95-й процентиль) с учетом коэффициента будущего роста (по умолчанию используется значение 30 %). Обратите внимание, что общая скорость обработки данных виртуальной машины не всегда вычисляется путем сложения скорости обработки данных на каждом ее отдельном диске, так как максимальная скорость обработки данных виртуальной машины состоит из максимальных значений обработки на каждом отдельном диске в течение каждой минуты процесса профилирования.

**Azure VM Size** (Размер виртуальной машины Azure). Размер виртуальной машины облачных служб Azure, который лучше всего использовать для сопоставления с локальной виртуальной машиной. Это сопоставление выполняется на основе памяти, числа дисков, ядер, сетевых карт и операций записи и чтения на диск в секунду локальной виртуальной машины. Мы рекомендуем всегда использовать виртуальную машину Azure наименьшего размера, характеристики которой соответствуют всем этим характеристикам локальной виртуальной машины.

**Количество дисков**. Общее количество дисков (VMDK-файлов) на виртуальной машине.

**Размер диска (ГБ)** . Общий размер, настроенный на всех дисках виртуальной машины. Эта программа также показывает размер каждого отдельного диска виртуальной машины.

**Cores** (Ядра). Количество ядер ЦП на виртуальной машине.

**Память (МБ)** . Объем ОЗУ на виртуальной машине.

**NICs** (Сетевые адаптеры). Количество сетевых адаптеров на виртуальной машине.

**Тип загрузки**. Тип загрузки виртуальной машины. (BIOS или EFI).  Сейчас Azure Site Recovery поддерживает виртуальные машины EFI (Windows Server 2012, 2012 R2 и 2016) для Windows Server при условии, что в загрузочном диске меньше 4 секций, а размер загрузочного сектора равен 512 байт. Чтобы защитить виртуальные машины EFI, версия службы Azure Site Recovery должна быть 9.13 или выше. Отработка отказа поддерживается только для виртуальных машин EFI. Восстановление размещения не поддерживается.  

**Тип ОС**. Тип операционной системы виртуальной машины. Это может быть Windows, Linux или другая ОС в зависимости от выбранного шаблона в VMware vSphere при создании виртуальной машины.  

## <a name="incompatible-vms"></a>Несовместимые виртуальные машины

![Электронная таблица Excel с несовместимыми виртуальными машинами
](media/site-recovery-vmware-deployment-planner-analyze-report/incompatible-vms-v2a.png)

**Имя виртуальной машины**. Имя или IP-адрес виртуальной машины, заданные в качестве значения параметра VMListFile во время создания отчета. В этом столбце также перечислены диски (VMDK-файлы), присоединенные к виртуальным машинам. Чтобы различать виртуальные машины, для виртуальных машин в vCenter с повторяющимися именами или IP-адресами указывается имя узла ESXi. Это узел ESXi, на котором размещалась виртуальная машина, когда она была впервые обнаружена с помощью средства в процессе профилирования.

**VM Compatibility** (Совместимость виртуальной машины). Указывает, почему определенная виртуальная машина не поддерживает Site Recovery. Причины приводятся для каждого несовместимого диска виртуальной машины, и в зависимости от опубликованных [ограничений хранилища](https://aka.ms/azure-storage-scalbility-performance) эта проблема может быть вызвана следующим:

* Размер диска превышает 4095 ГБ. Сейчас служба хранилища Azure не поддерживает диски данных емкостью более 4095 ГБ.

* Размер диска ОС превышает 2048 ГБ. Сейчас служба хранилища Azure не поддерживает диски ОС емкостью более 2048 ГБ.

* Общий размер виртуальной машины (репликация и тестовая отработка отказа) превышает поддерживаемый размер учетной записи хранения (35 ТБ). Это обычно происходит на виртуальных машинах с одним диском, показатели производительности которого превышают максимально поддерживаемые ограничения Microsoft Azure или Azure Site Recovery для хранилища класса Standard, используемого для отправки виртуальной машины в зону хранилища класса Premium. Однако максимальный поддерживаемый размер учетной записи хранения класса Premium составляет 35 ТБ, и поэтому невозможно обеспечить защиту одной виртуальной машины в нескольких учетных записях хранения. Кроме того, обратите внимание, что если тестовая отработка отказа выполняется на защищенной виртуальной машине, этот процесс происходит в той же учетной записи хранения, где выполняется репликация. Поэтому на диске необходимо настроить в 2 раза больше пространства для выполнения репликации и тестовой отработки отказа в параллельном режиме.

* Число операций ввода-вывода в секунду источника превышает поддерживаемое хранилищем ограничение в 7500 операций ввода-вывода в секунду на диск.

* Число операций ввода-вывода в секунду источника превышает поддерживаемое хранилищем ограничение в 80 000 операций ввода-вывода в секунду на виртуальную машину.

* Среднее значение скорости обработки данных превышает поддерживаемое Site Recovery ограничение в 10 МБ/с для среднего объема ввода-вывода на диск.

* Среднее значение скорости обработки данных превышает поддерживаемое Site Recovery ограничение в 25 МБ/с для среднего числа операций ввода-вывода на диск (суммируются все диски в обработке).

* Пиковая скорость обработки данных на всех дисках виртуальной машины превышает максимально поддерживаемое Site Recovery ограничение в 54 МБ/с на виртуальную машину.

* Среднее эффективное значение количества операций ввода-вывода в секунду при записи превышает поддерживаемое Site Recovery ограничение в 840 операций на диск.

* Вычисленный объем хранилища моментальных снимков превышает поддерживаемое ограничение в 10 ТБ.

* Обработка данных в день превышает поддерживаемое ограничение 2 ТБ на сервер обработки.


**Peak R/W IOPS (with Growth Factor)** (Пиковое число операций ввода-вывода в секунду с учетом коэффициента роста). Пиковое значение числа операций ввода-вывода в секунду для диска (по умолчанию используется 95-й процентиль) с учетом коэффициента будущего роста (по умолчанию используется значение 30 %). Обратите внимание, что общее число операций записи и чтения на диск в секунду не всегда вычисляется путем сложения операций записи и чтения каждого отдельного диска виртуальной машины, так как максимальное число операций записи и чтения в секунду виртуальной машины состоит из максимальных значений каждого отдельного диска в течение каждой минуты процесса профилирования.

**Peak Data Churn in Mbps (with Growth Factor)** (Пиковая скорость обработки данных (Мбит/с) с учетом коэффициента роста). Пиковое значение скорости обработки данных для диска (по умолчанию используется 95-й процентиль) с учетом коэффициента будущего роста (по умолчанию используется значение 30 %). Обратите внимание, что общая скорость обработки данных виртуальной машины не всегда вычисляется путем сложения скорости обработки данных на каждом ее отдельном диске, так как максимальная скорость обработки данных виртуальной машины состоит из максимальных значений обработки на каждом отдельном диске в течение каждой минуты процесса профилирования.

**Количество дисков**. Общее количество VMDK-файлов на виртуальной машине.

**Размер диска (ГБ)** . Общий размер, настроенный на всех дисках виртуальной машины. Эта программа также показывает размер каждого отдельного диска виртуальной машины.

**Cores** (Ядра). Количество ядер ЦП на виртуальной машине.

**Память (МБ)** . Объем ОЗУ на виртуальной машине.

**NICs** (Сетевые адаптеры). Количество сетевых адаптеров на виртуальной машине.

**Тип загрузки**. Тип загрузки виртуальной машины. (BIOS или EFI).  Сейчас Azure Site Recovery поддерживает виртуальные машины EFI (Windows Server 2012, 2012 R2 и 2016) для Windows Server при условии, что в загрузочном диске меньше 4 секций, а размер загрузочного сектора равен 512 байт. Чтобы защитить виртуальные машины EFI, версия службы Azure Site Recovery должна быть 9.13 или выше. Отработка отказа поддерживается только для виртуальных машин EFI. Восстановление размещения не поддерживается.

**Тип ОС**.  Тип операционной системы виртуальной машины. Это может быть Windows, Linux или другая ОС в зависимости от выбранного шаблона в VMware vSphere при создании виртуальной машины.

## <a name="azure-site-recovery-limits"></a>Ограничения Azure Site Recovery
В таблице ниже приведены ограничения Azure Site Recovery. Эти ограничения получены на основе наших проверок, но они не охватывают все возможные сочетания операций ввода-вывода в приложении. Фактические результаты зависят от сочетания операций ввода-вывода приложения. Чтобы получить более точные сведения о производительности приложения, мы рекомендуем всегда выполнять расширенное тестирование приложения с помощью тестовой отработки отказа.

**Целевое хранилище репликации** | **Средний размер ввода-вывода исходного диска** |**Средняя скорость обработки данных исходного диска** | **Общий размер обработанных данных на исходном диске за день**
---|---|---|---
Хранилище уровня "Стандартный" | 8 КБ | 2 МБ/с | 168 ГБ на диск
Диск P10 или P15 класса Premium | 8 КБ  | 2 МБ/с | 168 ГБ на диск
Диск P10 или P15 класса Premium | 16 КБ | 4 МБ/с |  336 ГБ на диск
Диск P10 или P15 класса Premium | 32 КБ или выше | 8 МБ/с | 672 ГБ на диск
Диск P20, P30, P40 или P50 класса Premium | 8 КБ    | 5 МБ/с | 421 ГБ на диск
Диск P20, P30, P40 или P50 класса Premium | 16 КБ или выше | 20 МБ в секунду. | 1684 ГБ на диск

**Исходная скорость обработки данных** | **Максимальное ограничение**
---|---
Средняя скорость обработки данных на виртуальную машину| 25 МБ/с
Пиковая скорость обработки данных на всех дисках виртуальной машины | 54 МБ/с
Максимальный объем обработки данных, поддерживаемый сервером обработки | 2 ТБ

В указанных средних значениях предполагается 30-процентное перекрытие операций ввода-вывода. В зависимости от коэффициента перекрытия, размера записи и фактической рабочей нагрузки ввода-вывода Site Recovery может обрабатывать более высокую пропускную способность. В приведенных выше значениях предполагается, что длина очереди невыполненной работы составляет примерно 5 минут, то есть после передачи выполняется обработка данных, а созданная точка восстановления составляет 5 минут.


## <a name="cost-estimation"></a>Оценка затрат
Подробнее об [оценке затрат](site-recovery-vmware-deployment-planner-cost-estimation.md).


## <a name="next-steps"></a>Дальнейшие действия
Подробнее об [оценке затрат](site-recovery-vmware-deployment-planner-cost-estimation.md).
