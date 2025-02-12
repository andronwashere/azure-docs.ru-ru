---
title: Планирование ресурсов для аварийного восстановления Hyper-V помощью Azure Site Recovery | Документация Майкрософт
description: Эта статья поможет оценить ресурсы при настройке аварийного восстановления с помощью службы Azure Site Recovery.
author: rayne-wiselman
manager: carmonm
services: site-recovery
ms.service: site-recovery
ms.topic: conceptual
ms.date: 12/27/2018
ms.author: raynew
ms.openlocfilehash: eeadfd6a57ff8a26f3f124e2a807fcd66e77b85f
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "61036749"
---
# <a name="plan-capacity-for-hyper-v-vm-disaster-recovery"></a>Планирование ресурсов для аварийного восстановления виртуальной машины Hyper-V 

Теперь доступна новая улучшенная версия программы [Планировщик развертывания Azure Site Recovery (развертывание виртуальных машин Hyper-V в Azure)](site-recovery-hyper-v-deployment-planner.md). Она используется вместо старого средства. Используйте для планирования развертывания новое средство.
Это средство предоставляет указанные ниже рекомендации.

* Оценка совместимости виртуальной машины в зависимости от количества дисков, их размера, числа операций ввода-вывода в секунду, активности обработки данных и некоторых характеристик виртуальной машины.
* Оценка пропускной способности сети и RPO.
* Требования к инфраструктуре Azure.
* Требования к локальной инфраструктуре.
* Рекомендации по пакетной обработке для начальной репликации.
* Предполагаемые общие затраты на аварийное восстановление в Azure.


Планировщик ресурсов Azure Site Recovery позволяет определить требования к ресурсам для репликации виртуальных машин Hyper-V с помощью Azure Site Recovery.

Используйте планировщик ресурсов Site Recovery для анализа исходной среды и рабочих нагрузок. Так вы сможете оценить требования к пропускной способности, ресурсы сервера, необходимые для исходного расположения, и ресурсы (например, виртуальные машины и хранилище), требуемые в целевом расположении.

Этот инструмент можно использовать в двух режимах.

* **Быстрое планирование**. Предоставляет проекции сети и сервера на основе среднего числа виртуальных машин, дисков, объема хранилища и частоты изменений.
* **Детальное планирование**. Предоставляет сведения о каждой рабочей нагрузке на уровне виртуальной машины. Проанализируйте совместимость виртуальной машины и получите проекции сети и сервера.

## <a name="before-you-start"></a>Перед началом работы

* Соберите сведения о среде, включая информацию о виртуальных машинах, количестве дисков на виртуальную машину и объеме хранилища на диск.
* Определите частоту ежедневных изменений (обновлений) реплицированных данных. Скачайте [инструмент планирования ресурсов Hyper-V](https://www.microsoft.com/download/details.aspx?id=39057), чтобы получить сведения о частоте изменений. См. [дополнительные сведения](site-recovery-capacity-planning-for-hyper-v-replication.md) об этом средстве. Мы советуем использовать это средство в течение недели, чтобы получить средние значения.


## <a name="run-the-quick-planner"></a>Запуск средства быстрого планирования
1. Скачайте и откройте [планировщик ресурсов Azure Site Recovery](https://aka.ms/asr-capacity-planner-excel). Выполните макрос. Когда появится запрос, сделайте выбор, чтобы включить редактирование и содержимое.

2. В списке **Select a planner type** (Выбрать тип планировщика) выберите **Quick Planner** (Средство быстрого планирования).

   ![Начало работы](./media/site-recovery-capacity-planner/getting-started.png)

3. На листе **Планировщик ресурсов** введите необходимую информацию. Заполните все поля, помеченные красными кружками на снимке экрана ниже.

   a. В списке **Select your scenario** (Выберите сценарий) выберите пункт **Из Hyper-V в Azure** или **Из VMware или физического сервера в Azure**.

   2\. В поле **Average daily data change rate (%)** (Средняя частота ежедневного изменения данных (%)) введите данные, полученные с помощью [инструмента планирования ресурсов Hyper-V](site-recovery-capacity-planning-for-hyper-v-replication.md) или [планировщика ресурсов Site Recovery](./site-recovery-deployment-planner.md).

   c. Параметр **Compression** (Сжатие) не используется при репликации виртуальных машин Hyper-V в Azure. Чтобы использовать сжатие, воспользуйтесь устройством сторонних производителей, например Riverbed.

   d. В **Retention in days** (Период хранения в днях) укажите, как долго следует хранить реплики (в днях).

   д. В полях **Number of hours in which initial replication for the batch of virtual machines should complete** (Период (в часах), в течение которого должна быть выполнена начальная репликация для пакета виртуальных машин) и **Number of virtual machines per initial replication batch** (Число виртуальных машин на пакет начальной репликации) указываются параметры, используемые для вычисления требований к начальной репликации. При развертывании службы Site Recovery выполняется отправка всего начального набора данных.

   ![Входные данные](./media/site-recovery-capacity-planner/inputs.png)

4. Указав значения для исходной среды, вы увидите следующие выходные данные.

   * **Пропускная способность, требуемая для разностной репликации данных (Мбит/с)** . Пропускная способность сети для разностной репликации рассчитывается с учетом средней частоты ежедневного изменения данных.
   * **Пропускная способность, требуемая для начальной репликации (Мбит/с)** . Пропускная способность сети для начальной репликации рассчитывается с учетом указанных значений начальной репликации.
   * **Требуемое хранилище (ГБ)** . Общий требуемый объем в службе хранилища Azure.
   * **Общее число операций ввода-вывода в секунду для учетных записей хранения уровня "Стандартный"** . Рассчитывается на основе единицы операций ввода-вывода размером 8 КБ на общее число стандартных учетных записей хранения. Для средства быстрого планирования этот показатель вычисляется на основе сведений об общем количестве дисков исходной виртуальной машины и частоте ежедневных изменений данных. Для средства детального планирования этот показатель вычисляется на основе сведений об общем числе виртуальных машин, сопоставленных со стандартными виртуальными машинами Azure, и частоте изменений данных на этих виртуальных машинах.
   * **Число учетных записей хранения уровня "Стандартный"** . Общее число учетных записей хранения уровня "Стандартный", необходимых для защиты виртуальных машин. Стандартная учетная запись хранения обеспечивает выполнение до 20 000 операций ввода-вывода в секунду на всех виртуальных машинах в стандартном хранилище. На диске поддерживается до 500 операций ввода-вывода в секунду.
   * **Требуемое число дисков больших двоичных объектов**. Число дисков, создаваемых в службе хранилища Azure.
   * **Требуемое число учетных записей хранения уровня "Премиум"** . Общее число учетных записей хранения уровня "Премиум", необходимых для защиты виртуальных машин. Для исходной виртуальной машины с большим числом операций ввода-вывода в секунду (более 20 000) требуется учетная запись хранилища класса Premium. Учетная запись хранилища класса Premium обеспечивает выполнение до 80 000 операций ввода-вывода в секунду.
   * **Общее число операций ввода-вывода в секунду для хранилища класса Premium**. Рассчитывается на основе единицы операций ввода-вывода размером 256 КБ на общее число учетных записей хранилища класса Premium. Для средства быстрого планирования этот показатель вычисляется на основе сведений об общем количестве дисков исходной виртуальной машины и частоте ежедневных изменений данных. Для средства детального планирования этот показатель вычисляется на основе сведений об общем числе виртуальных машин, сопоставленных с виртуальными машинами Azure класса Premium (серии DS и GS), и частоте изменений данных на этих виртуальных машинах.
   * **Требуемое число серверов конфигурации**. Показывает, сколько серверов конфигурации требуется для развертывания.
   * **Требуемое число дополнительных серверов обработки**. Показывает, необходимы ли дополнительные серверы обработки кроме сервера обработки, запущенного на сервере конфигурации по умолчанию.
   * **100 % дополнительного хранилища в исходной среде**. Показывает, требуется ли дополнительное хранилище в исходной среде.

      ![Выход](./media/site-recovery-capacity-planner/output.png)

## <a name="run-the-detailed-planner"></a>Запуск средства детального планирования

1. Скачайте и откройте [планировщик ресурсов Azure Site Recovery](https://aka.ms/asr-capacity-planner-excel). Выполните макрос. Когда появится запрос, сделайте выбор, чтобы включить редактирование и содержимое.

2. В списке **Select a planner type** (Выбрать тип планировщика) выберите пункт **Detailed Planner** (Средство детального планирования).

   ![Руководство по началу работы](./media/site-recovery-capacity-planner/getting-started-2.png)

3. На листе **Workload Qualification** (Оценка рабочей нагрузки) введите необходимую информацию. Необходимо заполнить все помеченные поля.

   a. В поле **Processor Cores** (Ядра процессора) укажите общее число ядер исходного сервера.

   2\. В поле **Memory allocation (in MBs)** (Выделение памяти (в МБ)) укажите объем ОЗУ исходного сервера.

   c. В поле **Number of NICs** (Число сетевых карт) укажите количество сетевых карт исходного сервера.

   d. В поле **Total Storage (in GB)** (Общий объем хранилища (в ГБ)) укажите общий размер хранилища виртуальной машины. Например, если на исходном сервере есть три диска, каждый по 500 ГБ, общий размер хранилища составляет 1500 ГБ.

   д. В поле **Number of disks attached** (Число подключенных дисков) укажите общее число дисков исходного сервера.

   Е. В поле **Disk capacity utilization (%)** (Уровень использования емкости диска (в %)) укажите среднее значение использования.

   ж. В поле **Daily data change rate (%)** (Частота ежедневного изменения данных (%)) укажите частоту ежедневных изменений данных исходного сервера.

   h. В поле **Mapping Azure VM size** (Сопоставление размера виртуальной машины Azure) введите размер виртуальной машины Azure, которую необходимо сопоставить. Если вы не хотите делать это вручную, щелкните**Compute IaaS VMs** (Вычислить виртуальные машины IaaS). Если вы ввели параметры вручную и выбрали **Compute IaaS VMs** (Вычислить виртуальные машины IaaS), этот параметр можно перезаписать вручную. Процесс вычисления автоматически определяет наилучшее соответствие размера виртуальной машины Azure.

   ![Лист Workload Qualification (Оценка рабочей нагрузки)](./media/site-recovery-capacity-planner/workload-qualification.png)

4. При выборе **Compute IaaS VMs** (Вычислить виртуальные машины IaaS) выполняются следующие операции.

   * Проверяет обязательные входные данные.
   * Расчет количества операций ввода-вывода в секунду и предлагает оптимальную виртуальную машину Azure для каждой виртуальной машины, соответствующей требованиям к репликации в Azure. При невозможности определения соответствующего размера виртуальной машины Azure отображается ошибка. Например, если подключено 65 дисков, отобразится ошибка, так как в Azure можно использовать максимум 64 виртуальные машины.
   * Определение учетной записи хранения, которую можно использовать для виртуальной машины Azure.
   * Вычисление общего числа стандартных учетных записей и учетных записей хранилища класса Premium, необходимых для рабочей нагрузки. Прокрутите вниз, чтобы просмотреть тип хранилища Azure и учетную запись хранения, которые можно использовать для исходного сервера.
   * Заполнение и сортировка остальной части таблицы с учетом требуемого типа хранилища (стандартное или класса Premium), назначенного виртуальной машине, и количества подключенных дисков. Для всех виртуальных машин, отвечающих требованиям Azure, в столбце **Is VM qualified?** (Проверена ли виртуальная машина?) отображается значение **Да**. Если нельзя создать резервную копию виртуальной машины в Azure, отображается ошибка.

Столбцы AA—AE представляют собой выходные данные и содержат сведения о каждой виртуальной машине.

![Выходные данные столбцов AA–AE](./media/site-recovery-capacity-planner/workload-qualification-2.png)

### <a name="example"></a>Пример
Например, для шести виртуальных машин, значения которых приведены в таблице, средство вычисляет и назначает самую подходящую виртуальную машину Azure и определяет самые оптимальные требования к службе хранилища Azure.

![Задания оценки рабочей нагрузки](./media/site-recovery-capacity-planner/workload-qualification-3.png)

* В выходных данных к этому примеру обратите внимание на следующее:

  * Первый столбец — столбец проверки для виртуальных машин, дисков и обновлений.
  * Для пяти виртуальных машин требуется две стандартные учетные записи хранения и одна учетная запись хранилища класса Premium.
  * VM3 не соответствует требованиям к защите, так как к этой виртуальной машине подключены один или несколько дисков более 1 ТБ.
  * VM1 и VM2 могут использовать первую стандартную учетную запись хранения.
  * VM4 может использовать вторую стандартную учетную запись хранения.
  * Для VM5 и VM6 необходима учетная запись хранения класса Premium, и каждая из них может использовать одну учетную запись.

    > [!NOTE]
    > Количество операций ввода-вывода в секунду для хранилищ уровня Standard и Premium рассчитывается не на уровне дисков, а на уровне виртуальных машин. Стандартная виртуальная машина может обрабатывать до 500 операций ввода-вывода в секунду на диск. Если число операций ввода-вывода на диск больше 500, необходимо хранилище класса Premium. Если операций ввода-вывода в секунду на диск больше 500, но общее число операций ввода-вывода для всех дисков виртуальной машины соответствует ограничениям для стандартной виртуальной машины Azure, то вместо серии DS или GS планировщик выберет стандартную виртуальную машину. (Ограничения виртуальной машины Azure: размер виртуальной машины, число дисков, число адаптеров, процессор и память.) Необходимо вручную обновить ячейку сопоставления размера Azure, указав соответствующую виртуальную машину серии DS или GS.


После ввода всех сведений выберите **Submit data to the planner tool** (Отправить данные в средство "Планировщик"), чтобы открыть планировщик ресурсов. Рабочие нагрузки выделяются, чтобы указать, разрешено ли их использовать для защиты.

### <a name="submit-data-in-capacity-planner"></a>Отправка данных в планировщик ресурсов
1. Открываемый лист **планировщика ресурсов** заполняется данными в зависимости от заданных вами параметров. В ячейке **Infra inputs source** (Исходная среда входных данных инфраструктуры) появится слово "Рабочая нагрузка", чтобы показать, что источником входных данных является лист **Workload Qualification** (Оценка рабочей нагрузки).

2. Чтобы внести изменения, измените данные на листе **Workload Qualification** (Оценка рабочей нагрузки). Затем выберите **Submit data to the planner tool** (Отправить данные в средство "Планировщик") еще раз.

   ![Планировщик ресурсов](./media/site-recovery-capacity-planner/capacity-planner.png)

## <a name="next-steps"></a>Дальнейшие действия
[Узнайте, как запустить](site-recovery-capacity-planning-for-hyper-v-replication.md) инструмент планирования ресурсов.
