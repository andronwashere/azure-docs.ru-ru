---
title: Резервное копирование SAP HANA в Azure на основе моментальных снимков хранилища | Документация Майкрософт
description: Существуют две основные возможности резервного копирования SAP HANA на виртуальных машинах Azure. В этой статье рассматривается резервное копирование на основе моментальных снимков хранилища.
services: virtual-machines-linux
documentationcenter: ''
author: hermanndms
manager: gwallace
editor: ''
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ums.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 07/05/2018
ms.author: rclaus
ms.openlocfilehash: 875060a59cf70d295534c3a4f56136010a560e74
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/09/2019
ms.locfileid: "67709931"
---
# <a name="sap-hana-backup-based-on-storage-snapshots"></a>Резервное копирование SAP HANA на основе моментальных снимков хранилища

## <a name="introduction"></a>Общие сведения

Эта статья входит в цикл из трех связанных статей, посвященных резервному копированию SAP HANA. В [руководстве по резервному копированию SAP HANA на виртуальных машинах Azure](sap-hana-backup-guide.md) содержатся общие сведения, а также информация о начале работы, а в статье [Резервное копирование SAP HANA в Azure на уровне файлов](sap-hana-backup-file-level.md) рассматривается резервное копирование на основе файлов.

При использовании функции резервного копирования виртуальной машины в одноэкземплярной универсальной демонстрационной системе лучше всего создать резервную копию виртуальной машины, а не управлять резервными копиями HANA на уровне операционной системы. Вы также можете создать копии отдельных виртуальных дисков, подключенных к виртуальной машине, с помощью моментальных снимков больших двоичных объектов Azure, а затем сохранить файлы данных HANA. В этом случае при создании резервной копии виртуальной машины или моментального снимка диска во время работы системы важно обеспечить согласованность приложений. Дополнительные сведения см. в разделе _Согласованность данных SAP HANA при создании моментальных снимков хранилища_ связанного [руководства по резервному копированию SAP HANA на виртуальных машинах Azure](sap-hana-backup-guide.md). SAP HANA имеет функцию, которая поддерживает такие типы моментальных снимков хранилища.

## <a name="sap-hana-snapshots-as-central-part-of-application-consistent-backups"></a>Моментальные снимки SAP HANA как центральная часть резервного копирования с согласованием приложений

Система SAP HANA предоставляет функцию создания моментальных снимков хранилища. К системам с одним контейнером применяется определенное ограничение. Сценарии с несколькими подключениями, использующие SAP HANA с более чем одним клиентом, не поддерживают этот тип моментального снимка базы данных SAP HANA (см. статью [Create a Storage Snapshot (SAP HANA Studio)](https://help.sap.com/saphelp_hanaplatform/helpdata/en/a0/3f8f08501e44d89115db3c5aa08e3f/content.htm) (Создание моментального снимка хранилища (SAP HANA Studio))).

Он работает следующим образом:

- подготовка моментального снимка хранилища путем инициации создания моментального снимка SAP HANA;
- запуск моментального снимка хранилища (например, моментального снимка большого двоичного объекта Azure);
- подтверждение моментального снимка SAP HANA.

![Создание моментального снимка данных SAP HANA с помощью инструкции SQL](media/sap-hana-backup-storage-snapshots/image011.png)

На рисунке выше показан процесс создания моментального снимка данных SAP HANA с помощью инструкции SQL.

![Моментальный снимок в каталоге резервного копирования в SAP HANA Studio](media/sap-hana-backup-storage-snapshots/image012.png)

Затем этот моментальный снимок появляется в каталоге резервного копирования в SAP HANA Studio.

![Моментальный снимок в каталоге данных SAP HANA на диске](media/sap-hana-backup-storage-snapshots/image013.png)

На диске моментальный снимок хранится в каталоге данных SAP HANA.

Когда моментальный снимок SAP HANA находится в режиме подготовки, перед созданием моментального снимка хранилища необходимо обеспечить согласованность файловой системы. Дополнительные сведения см. в разделе _Согласованность данных SAP HANA при создании моментальных снимков хранилища_ связанного [руководства по резервному копированию SAP HANA на виртуальных машинах Azure](sap-hana-backup-guide.md).

Созданный моментальный снимок хранилища SAP HANA нужно подтвердить. Для этого необходимо выполнить соответствующую инструкцию SQL: BACKUP DATA CLOSE SNAPSHOT. Дополнительные сведения см. в статье [BACKUP DATA CLOSE SNAPSHOT Statement (Backup and Recovery)](https://help.sap.com/saphelp_hanaplatform/helpdata/en/c3/9739966f7f4bd5818769ad4ce6a7f8/content.htm) (Инструкция BACKUP DATA CLOSE SNAPSHOT (резервное копирование и восстановление)).

> [!IMPORTANT]
> Подтвердите моментальный снимок HANA. В режиме подготовки моментального снимка системе SAP HANA может потребоваться дополнительное дисковое пространство, так как этот процесс сопровождается &quot;копированием при записи&quot;. Кроме того, вы также не сможете запустить новое резервное копирование до подтверждения моментального снимка SAP HANA.

## <a name="hana-vm-backup-via-azure-backup-service"></a>Резервное копирование виртуальной машины HANA с помощью службы архивации Azure

Агент службы архивации не поддерживается на виртуальных машинах Linux. Более того, в Linux отсутствует функциональный аналог службы VSS в Windows.  Чтобы использовать службу архивации Azure на уровне файлов или каталогов, файлы резервных копий SAP HANA необходимо скопировать на виртуальную машину Windows, а затем использовать агент службы архивации. 

В противном случае с помощью службы архивации Azure можно будет создать только полную резервную копию виртуальной машины Linux. Дополнительные сведения см. в статье [Общие сведения о возможностях в службе архивации Azure](../../../backup/backup-introduction-to-azure-backup.md).

Служба архивации Azure позволяет выполнять резервное копирование и восстановление виртуальных машин. Дополнительные сведения об этой службе и принципах ее работы см. в статье [Планирование инфраструктуры резервного копирования виртуальных машин в Azure](../../../backup/backup-azure-vms-introduction.md).

В этой статье приведены два важных соображения, которые следует учитывать:

_&quot;Для виртуальных машин Linux архивация возможна только при целостности файлов, так как на Linux не существует аналогичной платформы для службы VSS&quot;_ .

_&quot;Приложения должны реализовывать собственный механизм &quot;исправления&quot; восстановленных из архива данных&quot;_ .

Таким образом следует убедиться, что при запуске резервного копирования система SAP HANA находится в согласованном состоянии на диске. Сведения см. в разделе _Моментальные снимки SAP HANA_ выше. Но когда SAP HANA находится в режиме подготовки моментального снимка, существует одна потенциальная проблема. Дополнительные сведения см. в статье [Create a Storage Snapshot (SAP HANA Studio)](https://help.sap.com/saphelp_hanaplatform/helpdata/en/a0/3f8f08501e44d89115db3c5aa08e3f/content.htm) (Создание моментального снимка хранилища (SAP HANA Studio)).

В этой статье указано:

_&quot;Мы настоятельно советуем как можно скорее подтвердить или отклонить созданный моментальный снимок хранилища. Во время подготовки или создания моментального снимка относящиеся к нему данные заморожены. Тем не менее в базу данных по-прежнему можно вносить изменения. Но в результате этого замороженные данные не изменяются. Вместо этого изменения записываются в те позиции в области данных, которые не связаны с моментальным снимком хранилища, а также в журнал. Тем не менее чем дольше заморожены данные, относящиеся к моментальному снимку, тем большим может стать объем данных&quot;_ .

Служба архивации Azure обеспечивает согласованность файловой системы с помощью расширений виртуальной машины Azure. Отдельно эти расширения недоступны. Они работают только в сочетании со службой архивации Azure. Тем не менее, чтобы гарантировать согласованность приложений, по-прежнему необходимо создавать сценарии для создания и удаления моментального снимка SAP HANA.

Служба архивации Azure выполняет четыре основных этапа:

- выполнение сценария подготовки (сценарий необходим для создания моментального снимка SAP HANA);
- создание снимка;
- выполнение сценария после создания моментального снимка (сценарий необходим для удаления моментального снимка SAP HANA, созданного с помощью сценария подготовки);
- передача данных в хранилище.

Сведения о том, где можно скопировать эти сценарии и как именно работает Azure Backup, см. в следующих статьях:

- [Планирование инфраструктуры резервного копирования виртуальных машин в Azure](https://docs.microsoft.com/azure/backup/backup-azure-vms-introduction)
- [Согласованное с приложениями резервное копирование виртуальных машин Linux в Azure](https://docs.microsoft.com/azure/backup/backup-azure-linux-app-consistent)



На данный момент корпорация Майкрософт еще не опубликовала сценарий подготовки и сценарий после создания моментального снимка для SAP HANA. Клиентам и системным интеграторам необходимо создать эти сценарии и выполнить настройку в соответствии с приведенной выше документацией.


## <a name="restore-from-application-consistent-backup-against-a-vm"></a>Восстановление из согласованной с приложениями резервной копии виртуальной машины
Процесс восстановления согласованной с приложениями резервной копии, созданной службой Azure Backup, изложен в статье [Восстановление файлов из резервной копии виртуальной машины Azure](https://docs.microsoft.com/azure/backup/backup-azure-restore-files-from-vm). 

> [!IMPORTANT]
> В статье [Восстановление файлов из резервной копии виртуальной машины Azure](https://docs.microsoft.com/azure/backup/backup-azure-restore-files-from-vm) приводится список исключений и действий при использовании чередующихся наборов дисков. Диски с чередованием, вероятно, являются обычной конфигурацией в виртуальных машинах для SAP HANA. Поэтому очень важно ознакомиться с этой статьей и выполнить тестирование процесса восстановления в указанных в статье случаях. 



## <a name="hana-license-key-and-vm-restore-via-azure-backup-service"></a>Восстановление виртуальной машины и лицензионного ключа HANA в службе архивации Azure

Во время восстановления служба архивации Azure создает виртуальную машину. Сейчас мы не планируем добавлять возможность восстановления имеющейся виртуальной машины Azure &quot;на месте&quot;.

![Восстановление службы Azure на портале Azure](media/sap-hana-backup-storage-snapshots/image019.png)

На рисунке выше показан параметр восстановления службы Azure на портале Azure. Вы можете создать виртуальную машину во время восстановления или восстановить диски. После восстановления дисков по-прежнему необходимо создать виртуальную машину. Каждая создаваемая виртуальная машина в Azure получает новый уникальный идентификатор. (Дополнительные сведения см. в статье [Получение и использование уникального идентификатора виртуальной машины Azure](https://azure.microsoft.com/blog/accessing-and-using-azure-vm-unique-id/).)

![Уникальный идентификатор виртуальной машины Azure до и после ее восстановления в службе архивации Azure](media/sap-hana-backup-storage-snapshots/image020.png)

На рисунке ниже показан уникальный идентификатор виртуальной машины Azure до и после ее восстановления в службе архивации Azure. Этот уникальный идентификатор виртуальной машины используется в ключе оборудования лицензии SAP. Поэтому после восстановления виртуальной машины следует установить новую лицензию SAP.

Во время создания этого руководства по резервному копированию в службе архивации Azure была представлена новая функция (в режиме предварительного просмотра). Она позволяет выполнять восстановление на уровне файлов на основе моментального снимка, сделанного для резервной копии виртуальной машины. Это устраняет потребность в развертывании новой виртуальной машины, и поэтому уникальный идентификатор не изменяется, за счет чего новый ключ лицензии SAP HANA не требуется. Дополнительная документация будет предоставлена, когда эта функция будет полностью протестирована.

В конечном итоге служба архивации Azure позволит создавать резервные копии отдельных виртуальных дисков, а также файлов и каталогов, расположенных на виртуальной машине. Главное преимущество службы архивации Azure заключается в том, что она самостоятельно управляет всеми резервными копиями, избавляя пользователя от этого задания. Если требуется восстановление, служба архивации Azure выбирает правильную резервную копию.

## <a name="sap-hana-vm-backup-via-manual-disk-snapshot"></a>Создание резервной копии виртуальной машины SAP HANA вручную с помощью моментального снимка диска

Вместо использования службы архивации Azure вы можете настроить отдельное решение для резервного копирования, вручную создав моментальные снимки больших двоичных объектов виртуальных жестких дисков Azure с помощью PowerShell. Инструкции см. в статье [Using blob snapshots with PowerShell](https://blogs.msdn.microsoft.com/cie/2016/05/17/using-blob-snapshots-with-powershell/) (Использование моментальных снимков больших двоичных объектов в PowerShell).

Этот процесс обеспечивает большую гибкость, но не разрешает проблемы, описанные ранее в этом документе:

- Вы по-прежнему должны обеспечивать согласованность системы SAP HANA путем создания моментального снимка SAP HANA
- Диск ОС невозможно перезаписать даже после освобождения виртуальной машины из-за ошибки, связанной с наличием аренды. Его можно перезаписать только после удаления виртуальной машины. Но в этом случае вам потребуется новый уникальный идентификатор виртуальной машины и новая лицензия SAP.

![Восстановление дисков данных виртуальной машины Azure](media/sap-hana-backup-storage-snapshots/image021.png)

Виртуальная машина Azure поддерживает восстановление только дисков данных. Это позволяет избежать проблем, связанных с получением нового идентификатора и действительной лицензии SAP. В рамках тестирования мы сделали следующее:

- Подключили к виртуальной машине два диска данных Azure и на их основе определили программный RAID-массив. 
- С помощью функции создания моментальных снимков SAP HANA подтвердили, что система SAP HANA находится в согласованном состоянии.
- Заморозили файловую систему. (Дополнительные сведения см. в разделе _Согласованность данных SAP HANA при создании моментальных снимков хранилища_ связанного [руководства по резервному копированию SAP HANA на виртуальных машинах Azure](sap-hana-backup-guide.md).)
- Сделали моментальные снимки больших двоичных объектов обоих дисков данных.
- Освободили файловую систему.
- Подтвердили моментальный снимок SAP HANA.
- Чтобы восстановить диски данных, остановили виртуальную машину и отсоединили оба диска.
- После отсоединения диски были перезаписаны с помощью исходных моментальных снимков больших двоичных объектов.
- Затем восстановленные виртуальные диски снова присоединили к виртуальной машине.
- После запуска виртуальной машины все компоненты в программном RAID-массиве работали должным образом и были возвращены в состояние на момент создания моментального снимка большого двоичного объекта.
- Для системы HANA было восстановлено состояние на момент создания моментального снимка.

Эту процедуру можно упростить, остановив виртуальную машину SAP HANA перед созданием моментальных снимков больших двоичных объектов. В этом случае вы пропустите этап создания моментального снимка HANA (если в системе больше ничего не происходит), а также этап остановки файловой системы. Дополнительные этапы необходимы при создании моментальных снимков с включенной системой. Дополнительные сведения см. в разделе _Согласованность данных SAP HANA при создании моментальных снимков хранилища_ связанного [руководства по резервному копированию SAP HANA на виртуальных машинах Azure](sap-hana-backup-guide.md).

## <a name="next-steps"></a>Следующие шаги
* Общие сведения и информацию о начале работы см. в [руководстве по резервному копированию SAP HANA на виртуальных машинах Azure](sap-hana-backup-guide.md).
* В статье [Резервное копирование SAP HANA в Azure на уровне файлов](sap-hana-backup-file-level.md) представлены сведения о файловой резервной копии.
* Дополнительные сведения об обеспечении высокого уровня доступности и планировании аварийного восстановления SAP HANA в Azure (крупные экземпляры) см. в [этой статье](hana-overview-high-availability-disaster-recovery.md).
