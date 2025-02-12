---
title: Часто задаваемые вопросы о службе Azure Backup
description: 'Ответы на часто задаваемые вопросы о возможностях службы Azure Backup, в частности о хранилищах служб восстановления, объектах, для которых можно создавать резервные копии, принципе работы, шифровании и ограничениях. '
author: dcurwin
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 07/07/2019
ms.author: dacurwin
ms.openlocfilehash: c60b2bfae0d974d454c03b7eba655cbdacab5943
ms.sourcegitcommit: c72ddb56b5657b2adeb3c4608c3d4c56e3421f2c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/24/2019
ms.locfileid: "68466691"
---
# <a name="azure-backup---frequently-asked-questions"></a>Часто задаваемые вопросы по Azure Backup
Эта статья содержит ответы на часто задаваемые вопросы о службе Azure Backup.

## <a name="recovery-services-vault"></a>Хранилище служб восстановления

### <a name="is-there-any-limit-on-the-number-of-vaults-that-can-be-created-in-each-azure-subscription"></a>Существует ли ограничение на число хранилищ, которые можно создать в каждой подписке Azure?
Да. Вы можете создать не более 500 хранилищ служб восстановления на каждый поддерживаемый регион службы архивации Azure на подписку. Если вам нужно больше хранилищ, создайте дополнительную подписку.

### <a name="are-there-limits-on-the-number-of-serversmachines-that-can-be-registered-against-each-vault"></a>Существует ли ограничение на число серверов и компьютеров, которые можно зарегистрировать в каждом хранилище?
Вы можете зарегистрировать не более 1000 виртуальных машин Azure в одном хранилище. Если вы используете агент Microsoft Azure Backup, вы можете зарегистрировать не более 50 агентов в одном хранилище. Кроме того, вы можете зарегистрировать 50 серверов MAB и (или) DPM в хранилище.

### <a name="if-my-organization-has-one-vault-how-can-i-isolate-data-from-different-servers-in-the-vault-when-restoring-data"></a>Если у моей организации есть только одно хранилище, как мне изолировать в нем данные, хранящиеся на разных серверах, при восстановлении данных?
Для всех серверов, данные на которых нужно восстановить одновременно, следует указать одну и ту же парольную фразу при настройке резервного копирования. Если вы хотите изолировать восстановление одного (или нескольких) сервера, используйте парольную фразу только для этого сервера (или серверов). Например, серверы отдела по работе с персоналом могут использовать одну зашифрованную парольную фразу, серверы бухгалтерии — другую, а серверы-хранилища — третью.

### <a name="can-i-move-my-vault-between-subscriptions"></a>Могу ли я перенести хранилище из одной подписки в другую?
Да. Чтобы переместить хранилище Служб восстановления, прочитайте эту [статью](backup-azure-move-recovery-services-vault.md).

### <a name="can-i-move-backup-data-to-another-vault"></a>Могу ли я перенести данные резервного копирования в другое хранилище?
Нет. Данные резервного копирования, находящиеся в хранилище, нельзя переместить в другое хранилище.

### <a name="can-i-change-from-grs-to-lrs-after-a-backup"></a>Могу ли я изменить тип репликации с GRS на LRS после создания резервной копии?
Нет. Хранилище Служб восстановления может изменять параметры хранилища только до сохранения первой резервной копии.

### <a name="can-i-do-an-item-level-restore-ilr-for-vms-backed-up-to-a-recovery-services-vault"></a>Могу ли я выполнить восстановление на уровне элементов (ILR) для виртуальных машин, для которых созданы резервные копии в хранилище Служб восстановления?
- Восстановление на уровне элементов поддерживается для виртуальных машин Azure, резервные копии которых были созданы с помощью соответствующей функции Azure. Дополнительные сведения см. в этой [статье](backup-azure-restore-files-from-vm.md).
- ILR не поддерживается для точек оперативного восстановления локальных виртуальных машин, резервное копирование которых осуществляется с помощью Azure Backup Server или System Center DPM.


## <a name="azure-backup-agent"></a>Агент Azure Backup

### <a name="where-can-i-find-common-questions-about-the-azure-backup-agent-for-azure-vm-backup"></a>Где мне найти вопросы и ответы об агенте Azure Backup для резервного копирования виртуальных машин Azure?

- Вопросы и ответы об агенте, выполняемом на виртуальных машинах Azure см. [в этой статье](backup-azure-vm-backup-faq.md).
- Информацию об агенте, который используется для резервного копирования папок службы файлов Azure, см. [здесь](backup-azure-file-folder-backup-faq.md).


## <a name="general-backup"></a>Общие вопросы о резервном копировании

### <a name="are-there-limits-on-backup-scheduling"></a>Существуют ли ограничения на расписание резервного копирования?
Да.
- Резервное копирование для компьютеров под управлением Windows Server или Windows можно выполнять не более трех раз в день. Вы можете настроить в политике резервного копирования ежедневное или еженедельное расписание.
- Вы можете создавать резервные копии DPM до двух раз в день. Вы можете настроить в политике резервного копирования ежедневное, еженедельное, ежемесячное или ежегодное расписание.
- Резервные копии виртуальных машин Azure создаются раз в день.

### <a name="what-operating-systems-are-supported-for-backup"></a>Какие операционные системы поддерживаются для резервного копирования?
Azure Backup поддерживает следующие операционные системы для резервного копирования файлов, папок и приложений с помощью Azure Backup Server и DPM.

**ОС** | **SKU** | **Сведения**
--- | --- | ---
Рабочая станция | |
Windows 10, 64-разрядная версия | Корпоративная, Профессиональная, Домашняя | На компьютерах должны быть установлены последние пакеты обновления и отдельные обновления.
Windows 8.1, 64-разрядная версия | Корпоративная, Профессиональная | На компьютерах должны быть установлены последние пакеты обновления и отдельные обновления.
Windows 8, 64-разрядная версия | Корпоративная, Профессиональная | На компьютерах должны быть установлены последние пакеты обновления и отдельные обновления.
Windows 7, 64-разрядная версия | Максимальная, Корпоративная, Профессиональная, Домашняя расширенная, Домашняя базовая, Начальная | На компьютерах должны быть установлены последние пакеты обновления и отдельные обновления.
Сервер | |
Windows Server 2019 64, бит | Standard, Datacenter, Essentials | Со всеми последними пакетами и обновлениями.
Windows Server 2016, 64-разрядная версия | Standard, Datacenter, Essentials | Со всеми последними пакетами и обновлениями.
Windows Server 2012 R2, 64-разрядная версия | Standard, Datacenter, Foundation | Со всеми последними пакетами и обновлениями.
Windows Server 2012, 64-разрядная версия | Datacenter, Foundation, Standard | Со всеми последними пакетами и обновлениями.
Windows Storage Server 2016, 64-разрядная версия | Standard, Workgroup | Со всеми последними пакетами и обновлениями.
Windows Storage Server 2012 R2, 64-разрядная версия | Standard, Workgroup, Essential | Со всеми последними пакетами и обновлениями.
Windows Storage Server 2012, 64-разрядная версия | Standard, Workgroup | Со всеми последними пакетами и обновлениями.
Windows Server 2008 R2 с пакетом обновления 1, 64-разрядная версия | Standard, Enterprise, Datacenter, Foundation | Со всеми последними обновлениями.
Windows Server 2008, 64-разрядная версия | Standard, Enterprise, Datacenter | Со всеми последними обновлениями.

Для резервного копирования виртуальных машин Azure под управлением Linux служба Azure Backup поддерживает весь [список дистрибутивов, рекомендуемых для использования в Azure](../virtual-machines/linux/endorsed-distros.md), за исключением CoreOS Linux и 32-разрядной операционной системы. Вы можете использовать и другие дистрибутивы Linux. Они должны работать нормально, если для ОС есть агент виртуальной машины и включена поддержка Python.


### <a name="are-there-size-limits-for-data-backup"></a>Существуют ли ограничения на размер резервной копии данных?
Действуют следующие ограничения размеров.

ОС или компьютер | Ограничение на размер источника данных
--- | ---
Windows 8 или более поздняя версия | 54 400 ГБ
Windows 7 |1700 ГБ
Windows Server 2012 или более поздней версии; | 54 400 ГБ
Windows Server 2008, Windows Server 2008 R2 | 1700 ГБ
Azure | 16 дисков данных<br/><br/> Диск данных размером до 4095 ГБ

### <a name="how-is-the-data-source-size-determined"></a>Как вычисляется размер источника данных?
В таблице ниже описано, по какому принципу определяется размер источника данных.

**Источник данных** | **Сведения**
--- | ---
Том |Объем данных, для которых выполняется резервное копирование с одного тома виртуальной машины.
База данных SQL Server |Размер одной базы данных SQL, для которой выполняется резервное копирование.
SharePoint | Суммарный объем баз данных содержимого и конфигурации в ферме SharePoint, для которой выполняется резервное копирование.
Exchange |Сумма размеров всех баз данных Exchange на сервере Exchange, для которого выполняется резервное копирование.
Восстановление исходного состояния системы и состояние системы |Каждая отдельная копия восстановления исходного состояния системы или состояния системы компьютера, для которого выполняется резервное копирование

### <a name="is-there-a-limit-on-the-amount-of-data-backed-up-using-a-recovery-services-vault"></a>Существует ли ограничение на объем данных резервного копирования в хранилище Служб восстановления?
Не существует ограничений на объем данных резервного копирования в хранилище Служб восстановления.

### <a name="why-is-the-size-of-the-data-transferred-to-the-recovery-services-vault-smaller-than-the-data-selected-for-backup"></a>Почему размер данных, переданных в хранилище Служб восстановления, меньше размера данных, выбранных для резервного копирования?
Все данные, для которых создается резервная копия через агент Azure Backup, DPM и Azure Backup Server, сжимаются и шифруются перед передачей. После сжатия и шифрования размер данных, сохраненных в хранилище, снижается на 30–40 %.

### <a name="can-i-delete-individual-files-from-a-recovery-point-in-the-vault"></a>Можно ли удалить отдельные файлы из точки восстановления в хранилище?
Нет, Azure Backup не поддерживает удаление или очистку отдельных элементов из хранимых резервных копий.

### <a name="if-i-cancel-a-backup-job-after-it-starts-is-the-transferred-backup-data-deleted"></a>Удалятся ли переданные данные резервной копии, если я отменю уже запущенную операцию резервного копирования?
Нет. Все данные, переданные в хранилище до момента отмены задания резервного копирования, остаются в хранилище.

- Служба архивации Azure использует механизм контрольных точек, чтобы добавлять точки в данные резервной копии во время резервного копирования.
- Благодаря наличию контрольных точек во время следующего резервного копирования можно проверить целостность файлов.
- Следующее задание выполнит добавочное резервное копирование относительно уже переданных данных. При добавочном резервном копировании передаются только новые или измененные данные, что позволяет более эффективно использовать пропускную способность сети.

Если вы отмените задание резервного копирования для виртуальной машины Azure, все переданные данные игнорируются. Следующее задание резервного копирования передает добавочные данные (изменения), произошедшие с момента последнего успешного задания.

## <a name="retention-and-recovery"></a>Период хранения и восстановление

### <a name="are-the-retention-policies-for-dpm-and-windows-machines-without-dpm-the-same"></a>Совпадают ли политики периода хранения, которые применяются для DPM и компьютеров Windows без DPM?
Да, в обоих случаях применяются ежедневные, еженедельные, ежемесячные и ежегодные политики периода хранения.

### <a name="can-i-customize-retention-policies"></a>Могу ли я настроить свои политики периода хранения?
Да, политики можно настраивать. Например, вы можете применить еженедельные и ежедневные требования к периоду хранения, но не использовать ежегодные и ежемесячные политики.

### <a name="can-i-use-different-times-for-backup-scheduling-and-retention-policies"></a>Могу ли я ли использовать разное время для расписания резервного копирования и политик периода хранения?
Нет. Политики хранения можно применять только в точках резервного копирования. Например, на этих изображениях представлена политика хранения для резервных копий, созданных в 00:00 и 18:00.

![Планирование резервного копирования и хранения](./media/backup-azure-backup-faq/Schedule.png)


### <a name="if-a-backup-is-kept-for-a-long-time-does-it-take-more-time-to-recover-an-older-data-point-br"></a>Если резервная копия хранится в течение длительного времени, приведет ли это к более медленному восстановлению старой точки данных? <br/>
Нет. Для восстановления старых и новых точек требуется одинаковое время. Каждая точка восстановления ведет себя как полная точка.

### <a name="if-each-recovery-point-is-like-a-full-point-does-it-impact-the-total-billable-backup-storage"></a>Если каждая точка представляет собой полную точку, влияет ли это на общее оплачиваемое хранилище резервных копий?
Обычно продукты с точками длительного срока хранения хранят резервные данные как полные точки.

- Полные точки *неэффективно* используют хранилище, но их восстановление осуществляется проще и быстрее.
- Добавочные копии используют хранилище *эффективно*, но требуют восстанавливать целую цепочку данных, что влияет на время восстановления.

В архитектуре хранилища службы архивации Azure учтены все преимущества обоих подходов: вы получаете оптимальное хранилище с возможностью быстрого восстановления данных и низкими затратами на хранение. Такой подход предусматривает эффективное использование пропускной способности для входящих и исходящих данных. При этом использование емкости хранилища данных и время восстановления сводятся к минимуму. См. дополнительные сведения о [добавочных резервных копиях](https://azure.microsoft.com/blog/microsoft-azure-backup-save-on-long-term-storage/).

### <a name="is-there-a-limit-on-the-number-of-recovery-points-that-can-be-created"></a>Существует ли ограничение на число создаваемых точек восстановления?
Для одного защищенного экземпляра можно создать до 9999 точек восстановления. Защищенный экземпляр — это компьютер, сервер (физический или виртуальный) или рабочая нагрузка, для которых настроено резервное копирование данных в Azure.

- См. дополнительные сведения о [резервном копировании и восстановлении](./backup-overview.md#backup-and-retention).


### <a name="how-many-times-can-i-recover-data-thats-backed-up-to-azure"></a>Сколько раз можно восстановить данные, которые архивируются в Azure?
Ограничение на количество операций восстановления из службы архивации Azure отсутствует.

### <a name="when-restoring-data-do-i-pay-for-the-egress-traffic-from-azure"></a>Осуществляется ли оплата за исходящий трафик из Azure при восстановлении данных?
Нет. Восстановление выполняется бесплатно и плата за исходящий трафик не взимается.

### <a name="what-happens-when-i-change-my-backup-policy"></a>Что происходит при изменении политики резервного копирования?
Если применяется новая политика, также применяется ее расписание и период хранения.

- Если период хранения продлевается, существующие точки восстановления отмечаются для хранения в соответствии с новой политикой.
- Если период хранения сокращается, они отмечаются для удаления (и удаляются) при следующем задании очистки.

## <a name="encryption"></a>Шифрование

### <a name="is-the-data-sent-to-azure-encrypted"></a>Шифруются ли передаваемые в Azure данные?
Да. Данные шифруются на локальном компьютере по протоколу AES256. Данные передаются через защищенное соединение HTTPS. Для защиты данных, передаваемых в облако, используется связь по протоколу HTTPS только между службой хранения и службой восстановления. Для защиты данных, передаваемых между службой восстановления и пользовательским компьютером, используется протокол iSCSI. Для защиты канала iSCSI применяется безопасное туннелирование.

### <a name="is-the-backup-data-on-azure-encrypted-as-well"></a>Шифруются ли резервные копии данных в Azure?
Да. Данные в Azure шифруются при хранении.

- Для резервных копий локальных данных шифрование выполняется с использованием парольной фразы, которую вы указываете при настройке резервного копирования в Azure.
- Для виртуальных машин Azure шифрование выполняется с помощью функции "Шифрование службы хранилища" (SSE).

Мы никогда не расшифровываем резервные копии данных.

### <a name="what-is-the-minimum-length-of-encryption-the-key-used-to-encrypt-backup-data"></a>Какова минимальная длина ключа шифрования, используемого для шифрования резервных копий данных?
Если используется агент резервного копирования, ключ шифрования должен содержать минимум 16 символов. Для виртуальных машин Azure нет ограничений длины ключей, используемых в Azure Key Vault.

### <a name="what-happens-if-i-misplace-the-encryption-key-can-i-recover-the-data-can-microsoft-recover-the-data"></a>Что будет в случае потери ключа шифрования? Смогу ли я восстановить данные? Может ли корпорация Майкрософт восстановить мои данные?
Ключ, используемый для шифрования резервных копий, хранится только у вас. Мы не храним копии ключей в Azure и не имеет к ключам никакого доступа. Если вы утратите этот ключ, корпорация Майкрософт не сможет восстановить данные резервной копии.

## <a name="next-steps"></a>Следующие шаги

См. другие статьи с вопросами и ответами:

- [Распространенные вопросы](backup-azure-vm-backup-faq.md) о резервном копировании виртуальных машин Azure
- [Распространенные вопросы](backup-azure-file-folder-backup-faq.md) об агенте Azure Backup
