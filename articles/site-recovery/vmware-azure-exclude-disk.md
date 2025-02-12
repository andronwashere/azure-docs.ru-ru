---
title: Исключение дисков из репликации для аварийного восстановления виртуальных машин VMware в Azure с помощью Azure Site Recovery | Документация Майкрософт
description: В статье объясняется, как и зачем исключать диски виртуальных машин из репликации для аварийного восстановления VMware в Azure.
author: mayurigupta13
manager: rochakm
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.date: 3/3/2019
ms.author: mayg
ms.topic: conceptual
ms.openlocfilehash: 105074892cc6dfa4da1e7c8ddd0a0aad9f1b60a1
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "60922116"
---
# <a name="exclude-disks-from-replication-of-vmware-vms-to-azure"></a>Исключение дисков из репликации виртуальных машин VMware в Azure

В этой статье описано, как исключать диски при репликации виртуальных машин VMware в Azure. Благодаря этому можно оптимизировать использование пропускной способности при репликации или же ресурсы целевой стороны, используемые такими дисками. См. дополнительные сведения об [исключении дисков для Hyper-V](hyper-v-exclude-disk.md).


## <a name="prerequisites"></a>Технические условия

По умолчанию реплицируются все диски на виртуальной машине. Чтобы исключить диск из репликации, вам необходимо установить вручную на компьютер службу Mobility Service до включения репликации (при репликации из VMware в Azure).


## <a name="why-exclude-disks-from-replication"></a>Зачем исключать диски из репликации
Исключение дисков из репликации часто бывает необходимо:

- Если данные, находящиеся на исключенных дисках, не важны или их репликация не требуется.

- Ресурсы хранилища и сети можно сохранять, не реплицируя данные этого потока.

## <a name="what-are-the-typical-scenarios"></a>Стандартные сценарии
Вы можете определить конкретные примеры потоков данных, которые подходят для исключения. Это могут быть записи в файл подкачки (pagefile.sys) и записи в файл tempdb Microsoft SQL Server. В зависимости от рабочей нагрузки и подсистемы хранилища файл подкачки может регистрировать значительный поток данных. Для репликации таких данных из основного сайта в Azure понадобится много ресурсов. Чтобы оптимизировать репликацию виртуальной машины с одним виртуальным диском, включающим операционную систему и файл подкачки, сделайте следующее:

1. Разделите один виртуальный диск на два диска. Один виртуальный диск будет содержать операционную систему, а второй — файл подкачки.
2. Исключите диск с файлом подкачки из репликации.

Так же осуществляется оптимизация диска с файлом tempdb Microsoft SQL Server и файлом системной базы данных.

1. Файл системной базы данных и файл tempdb Microsoft SQL Server должны находиться на разных дисках.
2. Исключите диск с файлом tempdb из репликации.

## <a name="how-to-exclude-disks-from-replication"></a>Как исключать диски из репликации

На портале Azure Site Recovery для защиты виртуальной машины выполните рабочий процесс [Включение репликации](vmware-azure-enable-replication.md). На четвертом шаге рабочего процесса используйте столбец **Disk to replicate** (Диск для репликации), чтобы исключить диски из репликации. По умолчанию для репликации выбраны все диски. Снимите флажок для дисков, которые вы хотите исключить из репликации, а затем продолжите выполнение рабочего процесса для включения репликации.

![Исключение дисков из репликации и включение репликации для восстановления размещения виртуальных машин VMware в Azure](./media/vmware-azure-exclude-disk/enable-replication-exclude-disk1.png)


>[!NOTE]
>
> * Исключать можно только те диски на виртуальных машинах, которые уже установлены в службе Mobility. Службу Mobility Service необходимо установить вручную, так как механизм принудительной установки для нее можно использовать только после включения репликации.
> * Из репликации можно исключать только базовые диски. Вы не можете исключать диски операционной системы или динамические диски.
> * После включения репликации добавить или удалить диски для репликации нельзя. Если вы хотите добавить или исключить диск, отключите защиту компьютера, а затем включите ее повторно.
> * Если вы исключили диск, необходимый для работы приложения, для выполнения реплицируемого приложения после отработки отказа в Azure вам необходимо будет создать его в Azure вручную. Кроме того, для создания диска во время отработки отказа компьютера вы можете интегрировать службу автоматизации Azure в план восстановления.
> * Виртуальная машина Windows. Диски, создаваемые в Azure вручную, не восстанавливаются. Например, если вы выполните отработку отказа трех дисков и создадите два диска непосредственно на виртуальной машине Azure, то восстановление размещения будет выполнено только для трех дисков, задействованных при отработке отказа. Восстановление размещения после сбоя или защита из локальной среды в Azure недоступны для дисков, созданных вручную.
> * Виртуальная машина Linux. Диски, создаваемые в Azure вручную, восстанавливаются. Например, если для трех дисков выполняется отработка отказа, а еще два создаются непосредственно в виртуальных машинах Azure, восстановление будет выполнено для всех пяти дисков. Диски, созданные вручную при восстановлении расположения, исключать нельзя.
>


## <a name="end-to-end-scenarios-of-exclude-disks"></a>Полные сценарии исключения дисков
Рассмотрим два сценария, чтобы лучше понять функцию исключения дисков.

- Диск с файлом tempdb SQL Server.
- Диск с файлом подкачки (pagefile.sys).

## <a name="example-1-exclude-the-sql-server-tempdb-disk"></a>Пример 1 Исключение диска с файлом tempdb SQL Server
Рассмотрим виртуальную машину SQL Server с диском tempdb, который может быть исключен из репликации.

Имя виртуального диска — SalesDB.

Ниже представлены диски на исходной виртуальной машине.


**Имя диска** | **№ диска операционной системы на виртуальной машине** | **Буква диска** | **Тип данных на диске**
--- | --- | --- | ---
DB-Disk0-OS | Диск 0 | C:\ | Диск операционной системы
DB-Disk1| Диск 1 | D:\ | Системная база данных SQL и база данных пользователя Database1
DB-Disk 2 (исключенный из репликации) | Диск 2 | E:\ | Временные файлы
DB-Disk 3 (исключенный из репликации) | Диск 3 | F:\ | База данных tempdb SQL (путь к папке — F:\MSSQL\Data\) <br /> <br />Запишите путь к папке перед выполнением отработки отказа.
DB-Disk4 | Диск 4 |G:\ |База данных пользователя Database2

Так как поток данных на двух дисках виртуальной машины является временным, при защите виртуальной машины SalesDB исключите диски 2 и 3 из репликации. Эти диски не будут реплицироваться в Azure Site Recovery. Эти диски не будут присутствовать в виртуальной машине, перенесенной в Azure при отработке отказа.

Ниже представлены диски виртуальной машины Azure после отработки отказа.

**№ диска операционной системы на виртуальной машине** | **Буква диска** | **Тип данных на диске**
--- | --- | ---
Диск 0 | C:\ | Диск операционной системы
Диск 1 | E:\ | Временное хранилище<br /> <br />Azure добавляет этот диск и назначает первую доступную букву диска.
Диск 2 | D:\ | Системная база данных SQL и база данных пользователя Database1
Диск 3 | G:\ | База данных пользователя Database2

Так как диски 2 и 3 были исключены из репликации виртуальной машины SalesDB, "E:" является первой доступной буквой из диска. Azure назначает "E:" тому временного хранилища. Для всех реплицированных дисков буквы диска не изменятся.

Диск 3, который являлся диском с tempdb SQL (путь к папке с tempdb — F:\MSSQL\Data\), был исключен из репликации. Этот диск не будет присутствовать в перенесенной виртуальной машине. В результате служба SQL будет остановлена и ей понадобится путь F:\MSSQL\Data.

Существует два способа создания этого пути.

- Добавьте новый диск и назначьте путь к папке tempdb.
- Используйте существующий диск временного хранилища для пути к папке tempdb.

### <a name="add-a-new-disk"></a>Добавьте новый диск:

1. Запишите пути к файлам tempdb.mdf и tempdb.ldf для базы данных SQL перед отработкой отказа.
2. На портале Azure добавьте новый диск для восстановленной виртуальной машины с размером, не меньшим, чем у исходного диска базы данных tempdb SQL (диск 3).
3. Выполните вход на виртуальную машину Azure. В консоли управления (diskmgmt.msc) диском выполните инициализацию и форматирование добавленного диска.
4. Назначьте букву диска, используемую для диска базы данных tempdb SQL ("F:").
5. Создайте папку tempdb в томе "F:" (F:\MSSQL\Data).
6. Запустите службу SQL из консоли службы.

### <a name="use-an-existing-temporary-storage-disk-for-the-sql-tempdb-folder-path"></a>Используйте существующий диск временного хранилища для пути к папке tempdb SQL:

1. Откройте окно командной строки.
2. Запустите SQL Server в режиме восстановления из командной строки.

        Net start MSSQLSERVER /f / T3608

3. Запустите следующую команду sqlcmd, чтобы изменить путь к tempdb на новый.

        sqlcmd -A -S SalesDB        **Use your SQL DBname**
        USE master;     
        GO      
        ALTER DATABASE tempdb       
        MODIFY FILE (NAME = tempdev, FILENAME = 'E:\MSSQL\tempdata\tempdb.mdf');
        GO      
        ALTER DATABASE tempdb       
        MODIFY FILE (NAME = templog, FILENAME = 'E:\MSSQL\tempdata\templog.ldf');       
        GO


4. Остановите службу Microsoft SQL Server.

        Net stop MSSQLSERVER
5. Запустите службу Microsoft SQL Server.

        Net start MSSQLSERVER

Обратитесь к следующим рекомендациям Azure для диска временного хранилища:

* [Использование SSD на виртуальных машинах Azure для хранения базы данных TempDB SQL Server и расширений буферного пула](https://blogs.technet.microsoft.com/dataplatforminsider/2014/09/25/using-ssds-in-azure-vms-to-store-sql-server-tempdb-and-buffer-pool-extensions/)
* [Рекомендации по оптимизации производительности SQL Server на виртуальных машинах Azure](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-performance)

## <a name="failback-from-azure-to-an-on-premises-host"></a>Восстановление размещения (из Azure на локальный узел)
Теперь разберемся, какие диски будут реплицированы при выполнении отработки отказа из Azure в локальную среду VMware. Диски, созданные в Azure вручную, не будут реплицированы. Например, если вы выполните отработку отказа трех дисков и создадите два диска непосредственно на виртуальных машинах Azure, то восстановление размещения будет выполнено только для трех дисков, задействованных при отработке отказа. Восстановление размещения после сбоя или повторная защита из локальной среды в Azure недоступны для дисков, созданных вручную. Диски временного хранилища также не реплицируются в локальные узлы.

### <a name="failback-to-original-location-recovery"></a>Восстановление размещения в исходном расположении

В предыдущем примере конфигурация дисков виртуальной машины Azure была представлена следующим образом:

**№ диска операционной системы на виртуальной машине** | **Буква диска** | **Тип данных на диске**
--- | --- | ---
Диск 0 | C:\ | Диск операционной системы
Диск 1 | E:\ | Временное хранилище<br /> <br />Azure добавляет этот диск и назначает первую доступную букву диска.
Диск 2 | D:\ | Системная база данных SQL и база данных пользователя Database1
Диск 3 | G:\ | База данных пользователя Database2

После выполнения восстановления в исходное расположение в конфигурации дисков восстановленной виртуальной машины отсутствуют исключенные из репликации диски. Диски, которые были исключены из репликации VMware в Azure, не будут доступны на восстановленной виртуальной машине.

Ниже представлены диски, доступные после плановой отработки отказа из Azure на локальную виртуальную машину VMware (исходное расположение).

**№ диска операционной системы на виртуальной машине** | **Буква диска** | **Тип данных на диске**
--- | --- | ---
Диск 0 | C:\ | Диск операционной системы
Диск 1 | D:\ | Системная база данных SQL и база данных пользователя Database1
Диск 2 | G:\ | База данных пользователя Database2

## <a name="example-2-exclude-the-paging-file-pagefilesys-disk"></a>Пример 2 Исключение диска с файлом подкачки (pagefile.sys)

Рассмотрим виртуальную машину с диском с файлом подкачки, который можно исключить из репликации.
Есть два способа.

### <a name="case-1-the-paging-file-is-configured-on-the-d-drive"></a>Вариант 1. Файл подкачки настроен на диске "D:"
Конфигурация дисков.

**Имя диска** | **№ диска операционной системы на виртуальной машине** | **Буква диска** | **Тип данных на диске**
--- | --- | --- | ---
DB-Disk0-OS | Диск 0 | C:\ | Диск операционной системы
DB-Disk1 (исключенный из репликации) | Диск 1 | D:\ | pagefile.sys
DB-Disk2 | Диск 2 | E:\ | Пользовательские данные 1
DB-Disk3 | Диск 3 | F:\ | Пользовательские данные 2

Ниже приведены параметры файла подкачки на исходной виртуальной машине.

![Параметры файла подкачки на исходной виртуальной машине](./media/vmware-azure-exclude-disk/pagefile-on-d-drive-sourcevm.png)


Ниже представлены диски, доступные на виртуальной машине Azure после отработки отказа виртуальной машины из VMware в Azure.

**Имя диска** | **№ диска операционной системы на виртуальной машине** | **Буква диска** | **Тип данных на диске**
--- | --- | --- | ---
DB-Disk0-OS | Диск 0 | C:\ | Диск операционной системы
DB-Disk1 | Диск 1 | D:\ | Временное хранилище<br /> <br />pagefile.sys
DB-Disk2 | Диск 2 | E:\ | Пользовательские данные 1
DB-Disk3 | Диск 3 | F:\ | Пользовательские данные 2

Так как диск 1 (D:) был исключен, "D:" является первой доступной буквой диска из списка. Azure назначает "D:" тому временного хранилища. Так как диск "D:" доступен на виртуальной машине Azure, параметры файла подкачки виртуальной машины не изменятся.

Ниже приведены параметры файла подкачки на виртуальной машине Azure.

![Параметры файла подкачки на виртуальной машине Azure](./media/vmware-azure-exclude-disk/pagefile-on-azure-vm-after-failover.png)

### <a name="case-2-the-paging-file-is-configured-on-another-drive-other-than-d-drive"></a>Вариант 2. Файл подкачки настроен на другом диске (отличном от диска "D:")

Ниже приведена конфигурация дисков исходной виртуальной машины.

**Имя диска** | **№ диска операционной системы на виртуальной машине** | **Буква диска** | **Тип данных на диске**
--- | --- | --- | ---
DB-Disk0-OS | Диск 0 | C:\ | Диск операционной системы
DB-Disk 1 (исключенный из репликации) | Диск 1 | G:\ | pagefile.sys
DB-Disk2 | Диск 2 | E:\ | Пользовательские данные 1
DB-Disk3 | Диск 3 | F:\ | Пользовательские данные 2

Ниже приведены параметры файла подкачки на локальной виртуальной машине.

![Параметры файла подкачки на локальной виртуальной машине](./media/vmware-azure-exclude-disk/pagefile-on-g-drive-sourcevm.png)

Ниже представлены диски, доступные на виртуальной машине Azure после отработки отказа виртуальной машины из VMware в Azure.

**Имя диска** | **№ диска операционной системы на виртуальной машине** | **Буква диска** | **Тип данных на диске**
--- | --- | --- | ---
DB-Disk0-OS | Диск 0  |C:\ |Диск операционной системы
DB-Disk1 | Диск 1 | D:\ | Временное хранилище<br /> <br />pagefile.sys
DB-Disk2 | Диск 2 | E:\ | Пользовательские данные 1
DB-Disk3 | Диск 3 | F:\ | Пользовательские данные 2

Так как "D:" является первой доступной буквой диска в списке, Azure назначит ее тому временного хранилища. Для всех реплицированных дисков буква диска не изменится. Так как диск "G:" недоступен, для файла подкачки система будет использовать диск "C:".

Ниже приведены параметры файла подкачки на виртуальной машине Azure.

![Параметры файла подкачки на виртуальной машине Azure](./media/vmware-azure-exclude-disk/pagefile-on-azure-vm-after-failover-2.png)

## <a name="next-steps"></a>Дальнейшие действия
Настроив и запустив развертывание, вы можете ознакомиться с [дополнительными сведениями](site-recovery-failover.md) о различных типах обработки отказа.
