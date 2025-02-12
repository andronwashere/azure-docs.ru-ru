---
title: Повторное включение защиты на виртуальных машинах Azure, которые переносятся обратно в основной регион Azure, с помощью Azure Site Recovery после отработки отказа | Документация Майкрософт
description: В этой статье описывается, как с помощью Azure Site Recovery включить повторную защиту виртуальных машин Azure в дополнительном регионе после отработки отказа в основном регионе.
services: site-recovery
author: rajani-janaki-ram
manager: gauravd
ms.service: site-recovery
ms.topic: article
ms.date: 11/27/2018
ms.author: rajanaki
ms.openlocfilehash: eabb7d194a3ef65282befab1ae59e85ba56f2f5b
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "65472158"
---
# <a name="reprotect-failed-over-azure-vms-to-the-primary-region"></a>Повторное включение защиты виртуальных машин Azure в основном регионе после отработки отказа


При выполнении [отработки отказа](site-recovery-failover.md) виртуальных машин Azure из одного региона в другой с помощью [Azure Site Recovery](site-recovery-overview.md) их начальная загрузка в дополнительном регионе не защищена. Чтобы выполнить восстановление размещения виртуальных машин в основной регион, сделайте следующее.

- Повторно включите защиту виртуальных машин в дополнительном регионе, чтобы запустить репликацию в основном регионе.
- После повторного включения защиты и репликации виртуальных машин вы можете выполнить отработку отказа из дополнительного в основной регион.

## <a name="prerequisites"></a>Технические условия
1. Данные о выполнении отработки отказа виртуальной машины из основного в дополнительный регион необходимо фиксировать.
2. Основной целевой регион должен быть доступен и у вас должен быть доступ к ресурсам или возможность создавать их в этом регионе.

## <a name="reprotect-a-vm"></a>Повторное включение защиты виртуальной машины

1. В разделе **Хранилище** > **Реплицированные элементы** щелкните правой кнопкой мыши виртуальную машину, для которой проводилась отработка отказа, и выберите **Защитить повторно**. Должно отображаться направление повторной защиты от дополнительного в основной.

   ![Повторное включение защиты](./media/site-recovery-how-to-reprotect-azure-to-azure/reprotect.png)

2. Просмотрите сведения о группе ресурсов, сети, хранилище и группах доступности. Нажмите кнопку **ОК**. Если есть ресурсы, помеченные как новые, они создаются во время повторного включения защиты.
3. Задание повторной защиты заполняет целевой сайт актуальными данными. По завершении выполняется разностная репликация. Затем вы можете выполнить отработку отказа на основном сайте. Используя параметр настройки, вы можете выбрать учетную запись хранения или сеть для использования во время повторного включения защиты.

   ![Параметр "Настройка"](./media/site-recovery-how-to-reprotect-azure-to-azure/customize.png)

### <a name="customize-reprotect-settings"></a>Настройка параметров повторного включения защиты

Во время повторного включения защиты можно настроить следующие свойства целевой виртуальной машины.

![Настройка](./media/site-recovery-how-to-reprotect-azure-to-azure/customizeblade.png)

|Свойство |Примечания  |
|---------|---------|
|Целевая группа ресурсов     | Измените целевую группу ресурсов, в которой создана виртуальная машина. В целях повторного включения защиты целевая виртуальная машина удаляется. Вы можете выбрать новую группу ресурсов, в которой нужно создать виртуальную машину после отработки отказа.        |
|Целевая виртуальная сеть     | Во время включения задания повторной защиты невозможно изменить целевую сеть. Чтобы изменить сети, выполните повторное сопоставление сети.         |
|Целевое хранилище (дополнительная виртуальная машина, которая не использует управляемые диски)     | Вы можете изменить учетную запись хранения, которую виртуальная машина использует после отработки отказа.         |
|Реплика управляемых дисков (дополнительная виртуальная машина, которая использует управляемые диски)    | Site Recovery создает управляемые диски реплики в основном регионе, чтобы дублировать управляемые диски дополнительной виртуальной машины.         |
|Хранилище кэша     | Вы можете указать учетную запись хранения кэша, которую нужно использовать во время репликации. По умолчанию, если учетная запись хранения кэша не существует, ее необходимо создать.         |
|Группа доступности     |Если виртуальная машина в дополнительном регионе является частью группы доступности, вы можете выбрать группу доступности для целевой виртуальной машины в основном регионе. По умолчанию Site Recovery пытается найти и использовать имеющуюся группу доступности в основном регионе. Во время настройки можно указать новую группу доступности.         |


### <a name="what-happens-during-reprotection"></a>Что происходит при повторном включении защиты?

По умолчанию происходит следующее.

1. Учетная запись хранения кэша создается в регионе, в котором на виртуальной машине выполняется отработка отказа.
2. Если целевая учетная запись хранения (исходная учетная запись хранения в основном регионе) не существует, будет создана новая. Присвоенное имя учетной записи хранения является именем учетной записи хранения с суффиксом asr, используемой в дополнительной виртуальной машине.
3. Если виртуальная машина использует управляемые диски, управляемые диски реплики создаются в основном регионе для хранения данных, реплицированных с дисков дополнительной виртуальной машины.
4. Если целевая группа доступности не существует, создается группа в рамках задания включения повторной защиты (при необходимости). Если параметры повторной защиты настроены, будет использоваться выбранная группа.

Если запустить задание повторной защиты при наличии целевой виртуальной машины, происходит следующее.

1. Целевое расположение виртуальной машины выключается, если она запущена.
2. Если виртуальная машина использует управляемые диски, то копии исходных дисков создаются с суффиксом "-ASRReplica". Исходные диски удаляются. Копии с суффиксом "-ASRReplica" используются для репликации.
3. Если виртуальная машина использует неуправляемые диски, диски данных целевой виртуальной машины отключаются и используются для репликации. Создается копия диска операционной системы, которая подключается к виртуальной машине. Диск исходной операционной системы отключается и используется для репликации.
4. Синхронизируются только изменения между исходным и целевым дисками. Разница в файлах определяется путем сравнения обоих дисков с последующей передачей. Чтобы найти предполагаемое время, проверьте ниже.
5. После завершения синхронизации запускается разностная репликация данных, которая создает точку восстановления в соответствии с политикой репликации.

Если запустить задание повторной защиты при наличии целевой виртуальной машины и несуществующих дисков, происходит следующее.
1. Если виртуальная машина использует управляемые диски, реплики дисков создаются с суффиксом "-ASRReplica". Копии с суффиксом "-ASRReplica" используются для репликации.
2. Если виртуальная машина использует неуправляемые диски, реплики дисков создаются в целевой учетной записи хранения.
3. Все содержимое дисков копируется из региона отработки отказа в новый целевой регион.
4. После завершения синхронизации запускается разностная репликация данных, которая создает точку восстановления в соответствии с политикой репликации.

#### <a name="estimated-time--to-do-the-reprotection"></a>Предполагаемое время для выполнения повторного включения защиты 

В большинстве случаев Azure Site Recovery не реплицирует все данные в исходный регион. Ниже приведены условия, которые определяет, какой объем данных будет реплицировано.

1.  Если исходные данные виртуальной Машины удален, поврежден или недоступен из-за какой-либо причине как группа ресурсов изменение, удаление, то во время завершения повторного включения защиты среды выполнения Интеграции произойдет в том случае, так как нет доступных данных в исходном регионе для использования.
2.  Если исходные данные виртуальной Машины доступен только разностные резервные копии являются вычисляется путем сравнения дисков и передается. Проверьте ниже таблицу, чтобы получить приблизительное время 

|** Пример ситуации ** | ** Время, необходимое для повторного включения защиты ** |
|--- | --- |
|Исходный регион есть 1 виртуальная машина с 1 ТБ стандартный диск<br/>-Используется только 127 ГБ данных и остальную часть диска пуст<br/>— Тип диска "стандартный" с 60 Миб/С пропускной способности<br/>— Без изменений данных после отработки отказа| Приблизительное время 45 минут — 1,5 часа<br/> -При повторном включении защиты Site Recovery будет заполнять контрольную сумму всех данных, которая перенаправит 127 ГБ / 45 MBs 45 минут<br/>— Ряд накладные времени на это потребуется для восстановления сайта для автоматического масштабирования, 20 – 30 минут<br/>— Без расходов на исходящие данные |
|Исходный регион есть 1 виртуальная машина с 1 ТБ стандартный диск<br/>-Используется только 127 ГБ данных и остальную часть диска пуст<br/>— Тип диска "стандартный" с 60 Миб/С пропускной способности<br/>-45 ГБ изменений данных после отработки отказа| Приблизительное время часы на 1 – 2 часа<br/>-При повторном включении защиты Site Recovery будет заполнять контрольную сумму всех данных, которая перенаправит 127 ГБ / 45 MBs 45 минут<br/>-Передача времени для применения изменений 45 ГБ, за 45 ГБ / 45 Мбит/с ~ 17 минут<br/>— Плата за исходящий трафик бы только для данных 45 ГБ не для контрольной суммы|
 



## <a name="next-steps"></a>Дальнейшие действия

После включения защиты виртуальной машины вы можете инициировать отработку отказа. Отработка отказа завершает работу виртуальной машины в дополнительном регионе, создает и загружает виртуальную машину в основной регион с небольшим временем простоя. Рекомендуется выбрать соответствующее время и выполнить тестовую отработку отказа перед запуском полной отработки отказа на основном сайте. [Узнайте больше](site-recovery-failover.md) об отработке отказа.
