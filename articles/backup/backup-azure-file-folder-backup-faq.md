---
title: Часто задаваемые вопросы при резервном копировании файлов и папок с помощью службы архивации Azure
description: Ответы на распространенные вопросы о резервном копировании файлов и папок с помощью службы архивации Azure.
author: dcurwin
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 07/09/2019
ms.author: dacurwin
ms.openlocfilehash: dd800c0eeb18fe45b44a72aeb58b500623b2b366
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/09/2019
ms.locfileid: "67705079"
---
# <a name="common-questions-about-backing-up-files-and-folders"></a>Часто задаваемые вопросы о резервном копировании файлов и папок

В этой статье содержатся ответы на часто задаваемые вопросы существует множество резервное копирование файлов и папок с агентом служб восстановления Microsoft Azure (MARS) в [Azure Backup](backup-overview.md) службы.

## <a name="general"></a>Общие сведения

## <a name="configure-backups"></a>Настройка резервного копирования

### <a name="where-can-i-download-the-latest-version-of-the-mars-agent"></a>Где можно загрузить последнюю версию агента MARS?
Последнюю версию агента MARS, используемые при резервном копировании компьютеров Windows Server, System Center DPM и сервера Microsoft Azure Backup доступна для [загрузить](https://aka.ms/azurebackup_agent).

### <a name="how-long-are-vault-credentials-valid"></a>Как долго допустимые учетные данные хранилища?
Учетные данные хранилища действительны в течение 48 часов. Файл учетных данных после истечения срока действия еще раз загрузите его на портале Azure.

### <a name="from-what-drives-can-i-back-up-files-and-folders"></a>От того, какие диски можно создавать резервные копии файлов и папок?

Нельзя создавать резервные копии следующих типов дисков и томов:

* Съемные носители. Все источники для резервного копирования должны быть фиксированными.
* Тома только для чтения. Том должен быть доступным для записи, иначе служба теневого копирования томов (VSS) не будет работать.
* Автономные тома. Том должен быть доступным в сети, иначе служба VSS не будет работать.
* Общие сетевые ресурсы: Том должен быть локальным томом сервера, иначе оперативное резервное копирование не будет выполняться.
* Тома, зашифрованные с помощью технологии Bitlocker: Для создания резервной копии том должен быть разблокирован.
* Идентификация файловой системы. Эта служба поддерживает только файловую систему NTFS.

### <a name="what-file-and-folder-types-are-supported"></a>Какие типы файлов и папок поддерживаются?

[Дополнительные сведения](backup-support-matrix-mars-agent.md#supported-file-types-for-backup) о типах файлов и папок, поддерживаемые для резервного копирования.

### <a name="can-i-use-the-mars-agent-to-back-up-files-and-folders-on-an-azure-vm"></a>Можно ли использовать агент MARS для резервного копирования файлов и папок на виртуальной Машине Azure?  
Да. Служба архивации Azure предоставляет резервное копирование на уровне виртуальной Машины для виртуальных машин Azure с помощью расширения виртуальной Машины для агента виртуальной Машины Azure. Если вы хотите создавать резервные копии файлов и папок на гостевой операционной системы Windows на виртуальной Машине, можно установить для этого агента MARS.

### <a name="can-i-use-the-mars-agent-to-back-up-files-and-folders-on-temporary-storage-for-the-azure-vm"></a>Можно ли использовать агент MARS для резервного копирования файлов и папок во временном хранилище для виртуальной Машины Azure?
Да. Установите агент служб восстановления Microsoft AZURE, а резервное копирование файлов и папок в операционной системе Windows во временном хранилище.

- При стирание данных из временного хранилища, сбой задания резервного копирования.
- При удалении данных из временного хранилища, ее можно восстановить только для долговременного хранения.

### <a name="how-do-i-register-a-server-to-another-region"></a>Как зарегистрировать сервер в другом регионе?

Резервные копии данных отправляется в центр обработки данных, в котором зарегистрирован сервер хранилища. Самый простой способ изменить ЦОД — удалить и повторно установить агент и затем зарегистрировать компьютер в новое хранилище в регионе, что нужно.

### <a name="does-the-mars-agent-support-windows-server-2012-deduplication"></a>Поддержка режима MARS агентов Windows Server 2012 дедупликации?
Да. Агент MARS преобразует дедуплицированные данные в обычные данные при подготовке операции резервного копирования. Он затем оптимизирует данные для резервного копирования, шифрует данные и затем отправляет зашифрованные данные в хранилище.

## <a name="manage-backups"></a>Управление резервными копиями

### <a name="what-happens-if-i-rename-a-windows-machine-configured-for-backup"></a>Что произойдет, если я переименую компьютере Windows для резервного копирования?

При переименовании компьютера Windows, останавливаются все настроенные процессы резервного копирования.

- Необходимо зарегистрировать новое имя компьютера в хранилище службы архивации.
- При регистрации нового имени в хранилище, является первой операции *полный* резервного копирования.
- Если вам нужно восстановить данные, резервное копирование из хранилища со старым именем сервера, используйте параметр для восстановления в альтернативное расположение в мастер восстановления данных. [Узнайте больше](backup-azure-restore-windows-server.md#use-instant-restore-to-restore-data-to-an-alternate-machine).

### <a name="what-is-the-maximum-file-path-length-for-backup"></a>Какова максимальная длина пути для резервного копирования?
Агент служб восстановления Microsoft AZURE использует NTFS и использует Указание длины filepath, ограничена из-за [Windows API](/windows/desktop/FileIO/naming-a-file#fully-qualified-vs-relative-paths). Если файлы, которые вы хотите защитить превышает допустимое значение, резервное копирование родительской папки или диска.  

### <a name="what-characters-are-allowed-in-file-paths"></a>Какие символы разрешены в путях к файлам?

Агент служб восстановления Microsoft AZURE использует NTFS и позволяет [символы, которые поддерживаются](/windows/desktop/FileIO/naming-a-file#naming-conventions) в файле имена и пути.

### <a name="the-warning-azure-backups-have-not-been-configured-for-this-server-appears"></a>Появится предупреждение «Служба архивации Azure не были настроены для этого сервера».
Это предупреждение может отображаться, несмотря на то, что вы настроили политику резервного копирования, когда параметры расписания резервного копирования, хранящихся на локальном сервере не так же, как параметры, хранящиеся в хранилище службы архивации.
- После восстановления сервера или параметров до известного рабочего состояния расписания резервного копирования можно становятся несинхронизированными.
- Если вы получили это предупреждение, [настроить](backup-azure-manage-windows-server.md) политику резервного копирования еще раз, а затем резервное копирование выполнения по требованию на повторно синхронизировать локальный сервер с Azure.


## <a name="manage-the-backup-cache-folder"></a>Управление папкой кэша резервного копирования

### <a name="whats-the-minimum-size-requirement-for-the-cache-folder"></a>Каков минимальный необходимый размер папки кэша?
Размер папки кэша определяет объем данных, для которых выполняется резервное копирование.
- Тома папку кэша должны иметь свободное место, равное по крайней мере 5 – 10% от общего размера данных резервных копий.
- Если объем составляет менее 5% свободного места, увеличьте размер тома или перемещение папки кэша на том с достаточным объемом места.
- Если создать резервную копию состояния системы Windows, необходимо дополнительно 30 – 35 ГБ свободного места в томе, содержащем папку кэша.

### <a name="how-to-check-if-scratch-folder-is-valid-and-accessible"></a>Как проверить, если временная папка является допустимым и доступным?

1. По умолчанию временная папка находится в `\Program Files\Microsoft Azure Recovery Services Agent\Scratch`
2. Убедитесь, что путь к временной папке совпадает со значениями ключа реестра, показано ниже:

  | Путь к элементу реестра | Ключ реестра | Значение |
  | --- | --- | --- |
  | `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Config` |ScratchLocation |*Новое расположение папки кэша* |
  | `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Config\CloudBackupProvider` |ScratchLocation |*Новое расположение папки кэша* |

### <a name="how-do-i-change-the-cache-location-for-the-mars-agent"></a>Как изменить расположение кэша для агента служб восстановления Microsoft AZURE?

1. Выполните следующую команду в командной строке с повышенными правами для остановки модуля резервного копирования:

    ```PS C:\> Net stop obengine```

2. Не перемещайте файлы. Вместо этого скопируйте папку пространства кэша на другой диск, на котором достаточно свободного места.
3. Обновите следующие записи реестра, указав путь для новой папки кэша.<br/>

    | Путь к элементу реестра | Ключ реестра | Значение |
    | --- | --- | --- |
    | `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Config` |ScratchLocation |*Новое расположение папки кэша* |
    | `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Config\CloudBackupProvider` |ScratchLocation |*Новое расположение папки кэша* |

4. Перезапустите модуль резервного копирования в командной строке с повышенными правами:

    ```PS C:\> Net start obengine```

5. После завершения архивации успешно с использованием нового расположения можно удалить изначальную папку кэша.


### <a name="where-should-the-cache-folder-be-located"></a>Где следует разместить папки кэша?

Для папки кэша не рекомендуется использовать следующие расположения:

* Сетевой общей папки и съемные носители: Папка кэша должна быть локальной по отношению к серверу, для которого нужно выполнить резервное копирование с помощью оперативного резервного копирования. Сетевые расположения и съемные носители (например, USB-накопители) не поддерживаются.
* Автономные тома. Если планируется резервное копирование с помощью агента службы Azure Backup, папку кэша нужно подключить к сети.

### <a name="are-there-any-attributes-of-the-cache-folder-that-arent-supported"></a>Существуют ли какие-либо атрибуты папки кэша, которые не поддерживаются?
Для папки кэша не поддерживаются следующие атрибуты и их комбинации:

* зашифрованные;
* дедупликация;
* сжатые;
* разреженные;
* точка повторного анализа.

Папка кэша и метаданные виртуального жесткого диска не содержат нужных атрибутов для агента службы архивации Azure.

### <a name="is-there-a-way-to-adjust-the-amount-of-bandwidth-used-for-backup"></a>Есть ли способ настроить объем пропускной способности, используемой для резервного копирования?

Да, вы можете использовать **изменить свойства** параметр в агент MARS для регулировки пропускной способности и времени. [Узнайте больше](backup-configure-vault.md#enable-network-throttling).

## <a name="restore"></a>Восстановление

### <a name="what-happens-if-i-cancel-an-ongoing-restore-job"></a>Что произойдет, если отменить выполняемое задание восстановления?

Если задание восстановления текущих отменяется, останавливает процесс восстановления. Все файлы, восстановленные до отмены взаимодействуют целевому (исходное или другое расположение), без любой откатов.


## <a name="next-steps"></a>Следующие шаги

[Узнайте,](tutorial-backup-windows-server-to-azure.md) резервное копирование компьютера Windows.
