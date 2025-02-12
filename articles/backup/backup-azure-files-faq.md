---
title: Часто задаваемые вопросы о резервном копировании файлов Azure
description: Эта статья содержит сведения о том, как защитить файловые ресурсы Azure.
author: dcurwin
ms.author: dacurwin
ms.date: 01/31/2019
ms.topic: tutorial
ms.service: backup
manager: carmonm
ms.openlocfilehash: e3f94badb3af2e3f26a857f27f0b68a78338db92
ms.sourcegitcommit: c72ddb56b5657b2adeb3c4608c3d4c56e3421f2c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/24/2019
ms.locfileid: "68466310"
---
# <a name="questions-about-backing-up-azure-files"></a>Вопросы о резервном копировании файлов Azure
В этой статье содержатся ответы на часто задаваемые вопросы о резервном копировании файлов Azure. В некоторых ответах приведены ссылки на статьи, содержащие более подробные сведения. Кроме того, их также можно задать на [форуме для обсуждений](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazureonlinebackup).

Чтобы быстро просмотреть содержание этой статьи, используйте ссылки справа в разделе **В этой статье**.

## <a name="configuring-the-backup-job-for-azure-files"></a>Настройка задания резервного копирования файлов Azure

### <a name="why-cant-i-see-some-of-my-storage-accounts-i-want-to-protect-that-contain-valid-azure-file-shares-br"></a>Почему я не вижу некоторые учетные записи хранения, содержащие файловые ресурсы, которые требуется защитить в Azure? <br/>
В предварительной версии службы Backup для файловых ресурсов Azure поддерживаются не все типы учетных записей хранения. Ознакомьтесь с [этим](troubleshoot-azure-files.md#limitations-for-azure-file-share-backup-during-preview) списком, чтобы просмотреть поддерживаемые учетные записи хранения. Также вполне возможно, что учетная запись хранения, которую вы ищете, уже защищена или зарегистрирована в другом хранилище. [Отмените регистрацию](troubleshoot-azure-files.md#configuring-backup) в хранилище, чтобы найти учетную запись хранения, которую требуется защитить, в других хранилищах.

### <a name="why-cant-i-see-some-of-my-azure-file-shares-in-the-storage-account-when-im-trying-to-configure-backup-br"></a>Почему я не вижу некоторые файловые ресурсы Azure в учетной записи хранения при попытке настроить резервное копирование? <br/>
Проверьте, не защищен ли файловый ресурс Azure в том же хранилище служб восстановления и не удален ли он.

### <a name="can-i-protect-file-shares-connected-to-a-sync-group-in-azure-files-sync-br"></a>Могу ли я защитить файловые ресурсы, подключенные к группе синхронизации в службе синхронизации файлов Azure? <br/>
Да. Функция защиты файловых ресурсов Azure, подключенных к группам синхронизации, доступна в общедоступной предварительной версии.

### <a name="when-trying-to-back-up-file-shares-i-clicked-on-a-storage-account-for-discovering-the-file-shares-in-it-however-i-did-not-protect-them-how-do-i-protect-these-file-shares-with-any-other-vault"></a>При попытке создать резервную копию общих файловых ресурсов я выбираю учетную запись хранения для обнаружения файловых ресурсов. Но я не могу защитить эти ресурсы. Как защитить эти общие файловые ресурсы с помощью другого хранилища?
Если во время создания резервной копии выбрать учетную запись хранения для обнаружения файловых ресурсов в ней, учетная запись хранения регистрируется в хранилище, из которого выбирается учетная запись. Если вы выбираете для защиты файловые ресурсы в другом хранилище, [отмените регистрацию](troubleshoot-azure-files.md#configuring-backup) выбранной учетной записи хранения в этом хранилище.

### <a name="can-i-change-the-vault-to-which-i-backup-my-file-shares"></a>Можно ли изменить хранилище, в котором создана резервная копия моих файловых ресурсов?
Да. Но вам потребуется [остановить защиту](backup-azure-files.md#stop-protecting-an-azure-file-share) из подключенного хранилища, [отменить регистрацию](troubleshoot-azure-files.md#configuring-backup) этой учетной записи хранения и защитить ее из другого хранилища.

### <a name="in-which-geos-can-i-back-up-azure-file-shares-br"></a>В каких географических областях можно выполнять резервное копирование файловых ресурсов Azure? <br/>
Сейчас служба Backup для файловых ресурсов Azure находится в предварительной версии и доступна только в следующих географических регионах.
- Восточная Австралия (AE);
- Юго-Восточная Австралия (ASE).
- Южная Бразилия (BRS).
- Центральная Канада (CNC).
- Восточная Канада (CE).
- Центральная часть США (CUS).
- Восточная Азия (EA).
- Восточная часть США (EUS).
- Восточная часть США 2 (EUS2).
- Восточная Япония (JPE);
- Западная Япония (JPW);
- Центральная Индия (INC).
- Южная Индия (INS);
- Республика Корея, центральный регион (KRC)
- Южная Корея (KRS);
- Центрально-северная часть США (NCUS).
- Северная Европа (NE).
- Центрально-южная часть США (SCUS).
- Юго-Восточная Азия (SEA).
- Южная часть Соединенного Королевства (UKS).
- Западная часть Соединенного Королевства (UKW).
- Западная Европа (WE).
- Западная часть США (WUS).
- Западно-центральная часть США (WCUS).
- Западная часть США 2 (WUS 2).
- US Gov (Аризона) (UGA)
- US Gov (Техас) (UGT)
- US Gov (Вирджиния) (UGV)

Отправьте электронное сообщение по адресу [AskAzureBackupTeam@microsoft.com](email:askazurebackupteam@microsoft.com), если вам нужно использовать функцию в географическом регионе, не указанном выше.

### <a name="how-many-azure-file-shares-can-i-protect-in-a-vaultbr"></a>Сколько файловых ресурсов Azure можно защитить в хранилище?<br/>
Используя предварительную версию, в каждом хранилище вы можете защитить файловые ресурсы Azure из максимум 50 учетных записей хранения, не более 200 файловых ресурсов Azure на хранилище.

### <a name="can-i-protect-two-different-file-shares-from-the-same-storage-account-to-different-vaults"></a>Можно ли создать резервные копии двух разных файловых ресурсов из одной учетной записи хранения в разных хранилищах?
№ Все файловые ресурсы в учетной записи хранения могут быть защищены только одним хранилищем.

## <a name="backup"></a>Azure Backup

### <a name="how-many-on-demand-backups-can-i-take-per-file-share-br"></a>Сколько раз можно выполнить резервное копирование по запросу для каждого файлового ресурса? <br/>
В любое время вы можете хранить до 200 моментальных снимков для файлового ресурса. При этом учитываются моментальные снимки, сделанные службой Azure Backup в соответствии с политикой. Если операции резервного копирования завершаются сбоем после достижения этого ограничения, удалите точки восстановления по запросу для успешной архивации.

### <a name="after-enabling-virtual-networks-on-my-storage-account-the-backup-of-file-shares-in-the-account-started-failing-why"></a>После включения виртуальных сетей в учетной записи хранения начались сбои при резервном копировании файловых ресурсов в этой учетной записи. Почему?
Служба Backup для файловых ресурсов Azure не поддерживает учетные записи хранения с включенными виртуальными сетями. Отключите виртуальные сети в учетных записях хранения, чтобы выполнять резервное копирование.

## <a name="restore"></a>Восстановление

### <a name="can-i-recover-from-a-deleted-azure-file-share-br"></a>Можно ли выполнить восстановление из удаленного файлового ресурса Azure? <br/>
При удалении файлового ресурса Azure отобразится список резервных копий, которые будут удалены, а также запрос на подтверждение. Удаленный файловый ресурс Azure не подлежит восстановлению.

### <a name="can-i-restore-from-backups-if-i-stopped-protection-on-an-azure-file-share-br"></a>Можно ли выполнить восстановление из резервных копий после отключения защиты в файловом ресурсе Azure? <br/>
Да. Если вы выбрали **Retain Backup Data** (Сохранить данные архивации) при отключении защиты, то можете выполнить восстановление из всех имеющихся точек.

### <a name="what-happens-if-i-cancel-an-ongoing-restore-job"></a>Что произойдет, если отменить выполняемое задание восстановления?
Если отменить выполняемое задание восстановления, процесс восстановления прекращается и все файлы, восстановленные до отмены, остаются в заданном целевом расположении (исходном или другом указанном) без выполнения отката. 


## <a name="manage-backup"></a>Управление резервным копированием

### <a name="can-i-use-powershell-to-configuremanagerestore-backups-of-azure-file-shares-br"></a>Можно ли использовать PowerShell для настройки, администрирования и восстановления резервных копий файловых ресурсов Azure? <br/>
Да. Дополнительные сведения см. [здесь](backup-azure-afs-automation.md).

### <a name="can-i-access-the-snapshots-taken-by-azure-backups-and-mount-it-br"></a>Можно ли получить доступ к моментальным снимкам, сделанным службой Azure Backup, и подключить их? <br/>
Все моментальные снимки, сделанные службой Azure Backup, можно просмотреть на портале с помощью PowerShell или интерфейса командной строки. Дополнительных сведений о моментальных снимках файловых ресурсов Azure см. в статье [Обзор моментальных снимков для службы файлов Azure (предварительная версия)](../storage/files/storage-snapshots-files.md).

### <a name="what-is-the-maximum-retention-i-can-configure-for-backups-br"></a>Какой максимальный срок хранения можно настроить для резервных копий? <br/>
Резервное копирование для файловых ресурсов Azure позволяет настроить политики со сроком хранения до 180 дней. Хотя, используя [параметр резервного копирования по запросу в PowerShell](backup-azure-afs-automation.md#trigger-an-on-demand-backup), вы можете хранить точки восстановления даже в течение 10 лет.

### <a name="what-happens-when-i-change-the-backup-policy-for-an-azure-file-share-br"></a>Что происходит при изменении политики резервного копирования для файловых ресурсов Azure? <br/>
Если к файловым ресурсам применяется новая политика, также применяется ее расписание и период хранения. Если период хранения продлевается, существующие точки восстановления отмечаются для хранения в соответствии с новой политикой. Если период хранения сокращается, они помечаются для удаления при следующей очистке, а затем удаляются.

## <a name="see-also"></a>См. также
Здесь представлены сведения только о резервном копировании файлов Azure. Чтобы узнать больше о других областях службы Azure Backup, см. следующие разделы с часто задаваемыми вопросами.
-  [Часто задаваемые вопросы о службах восстановления](backup-azure-backup-faq.md).
-  [Часто задаваемые вопросы о резервном копировании виртуальных машин Azure](backup-azure-vm-backup-faq.md).
-  [Часто задаваемые вопросы об агенте службы Azure Backup](backup-azure-file-folder-backup-faq.md).