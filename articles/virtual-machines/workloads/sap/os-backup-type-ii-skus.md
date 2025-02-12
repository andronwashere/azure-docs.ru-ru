---
title: Резервное копирование и восстановление операционной системы SAP HANA в Azure (большие экземпляры) для номеров SKU типа II | Документация Майкрософт
description: Резервное копирование и восстановление операционной системы для SAP HANA для номеров SKU типа II в Azure (крупные экземпляры)
services: virtual-machines-linux
documentationcenter: ''
author: saghorpa
manager: gwallace
editor: ''
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 07/12/2019
ms.author: juergent
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 3afcd429351a0d988ff0e82ecf09f524ceac70f1
ms.sourcegitcommit: 10251d2a134c37c00f0ec10e0da4a3dffa436fb3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/13/2019
ms.locfileid: "67868969"
---
# <a name="os-backup-and-restore-for-type-ii-skus-of-revision-3-stamps"></a>Резервное копирование и восстановление ОС для номеров SKU типа II для версий 3

В этом документе описаны действия по выполнению резервного копирования и восстановления на уровне файлов операционной системы для **номеров SKU типа II** крупных экземпляров Hana версии 3. 

>[!Important]
> **Эта статья не относится к развертываниям SKU типа II в метках крупных экземпляров HANA версии 4.** Загрузочные номера LUN для единиц крупного экземпляра типа II (крупные экземпляры), которые развернуты в метках крупных экземпляров HANA версии 4, можно архивировать с помощью моментальных снимков хранилища, так как в этом случае номера SKU типа I уже находятся в метках версии 3.


>[!NOTE]
>Для сценариев резервного копирования операционной системы используется предварительно установленное на сервере программное обеспечение ReaR.  

После завершения подготовки командой Майкрософт `Service Management` по умолчанию сервер настраивается с помощью двух расписаний резервного копирования, чтобы создать резервную копию операционной системы на уровне файловой системы. Вы можете проверить расписания заданий резервного копирования с помощью следующей команды:
```
#crontab –l
```
Расписание резервного копирования можно изменить в любое время с помощью следующей команды:
```
#crontab -e
```
## <a name="how-to-take-a-manual-backup"></a>Как создать резервную копию вручную?

Резервное копирование файловой системы ОС планируется с помощью **задания cron** . Тем не менее вы также можете выполнить резервное копирование операционной системы на уровне файловой системы вручную. Для этого выполните команду ниже:

```
#rear -v mkbackup
```
На снимке экрана ниже показан пример создания резервной копии вручную:

![пример](media/HowToHLI/OSBackupTypeIISKUs/HowtoTakeManualBackup.PNG)


## <a name="how-to-restore-a-backup"></a>Как восстановить резервную копию?

Вы можете восстановить полную резервную копию или отдельный файл из нее. Для этого воспользуйтесь следующей командой:

```
#tar  -xvf  <backup file>  [Optional <file to restore>]
```
После операции восстановления файл будет восстановлен в текущем рабочем каталоге.

С помощью команды ниже показано восстановление файла */etc/fstabfrom* из файла резервной копии *backup.tar.gz*.
```
#tar  -xvf  /osbackups/hostname/backup.tar.gz  etc/fstab 
```
>[!NOTE] 
>Вам необходимо скопировать файл в нужное расположение после его восстановления из файла резервной копии.

На следующем снимке экрана показано восстановление полной резервной копии.

![HowtoRestoreaBackup.PNG](media/HowToHLI/OSBackupTypeIISKUs/HowtoRestoreaBackup.PNG)

## <a name="how-to-install-the-rear-tool-and-change-the-configuration"></a>Как установить инструмент ReaR и изменить конфигурацию? 

Для **номеров SKU типа II** больших экземпляров HANA пакеты Relax-and-Recover (ReaR) **предварительно установлены**, поэтому никакие дополнительные действия с вашей стороны не требуются. Для резервного копирования операционной системы вы можете напрямую запустить ReaR.
Тем не менее в случаях, когда требуется отдельная установка пакетов, вы можете выполнить шаги ниже, чтобы установить и настроить инструмент ReaR.

Чтобы установить пакеты резервного копирования **ReaR**, выполните команды ниже:

Для операционной системы **SLES** используйте следующую команду:
```
#zypper install <rear rpm package>
```
Для операционной системы **RHEL** используйте следующую команду: 
```
#yum install rear -y
```
Чтобы настроить инструмент ReaR, вам необходимо обновить параметры **OUTPUT_URL** и **BACKUP_URL** в *файле /etc/rear/local.conf*.
```
OUTPUT=ISO
ISO_MKISOFS_BIN=/usr/bin/ebiso
BACKUP=NETFS
OUTPUT_URL="nfs://nfsip/nfspath/"
BACKUP_URL="nfs://nfsip/nfspath/"
BACKUP_OPTIONS="nfsvers=4,nolock"
NETFS_KEEP_OLD_BACKUP_COPY=
EXCLUDE_VG=( vgHANA-data-HC2 vgHANA-data-HC3 vgHANA-log-HC2 vgHANA-log-HC3 vgHANA-shared-HC2 vgHANA-shared-HC3 )
BACKUP_PROG_EXCLUDE=("${BACKUP_PROG_EXCLUDE[@]}" '/media' '/var/tmp/*' '/var/crash' '/hana' '/usr/sap'  ‘/proc’)
```

На следующем снимке экрана показано восстановление полной резервной копии. ![Реартулконфигуратион. PNG](media/HowToHLI/OSBackupTypeIISKUs/RearToolConfiguration.PNG)
