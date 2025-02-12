---
title: Использование виртуальной машины Windows для устранения неполадок с помощью портала Azure | Документация Майкрософт
description: Узнайте, как устранять неполадки с виртуальными машинами Windows в Azure, подключив диск операционной системы к виртуальной машине восстановления с помощью портала Azure.
services: virtual-machines-windows
documentationCenter: ''
author: genlin
manager: gwallace
editor: ''
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: troubleshooting
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 08/13/2018
ms.author: genli
ms.openlocfilehash: 8ab6fc75475cd99e3d803450476880175f12d2b6
ms.sourcegitcommit: 1b7b0e1c915f586a906c33d7315a5dc7050a2f34
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/15/2019
ms.locfileid: "67881128"
---
# <a name="troubleshoot-a-windows-vm-by-attaching-the-os-disk-to-a-recovery-vm-using-the-azure-portal"></a>Устранение неполадок с виртуальной машиной Windows при подключении диска операционной системы к виртуальной машине восстановления с помощью портала Azure
Если возникает проблема с загрузкой или диском на виртуальной машине Windows в Azure, возможно, вам нужно устранить неполадки, связанные с самим виртуальным жестким диском. Например, такая ситуация может возникнуть из-за сбоя обновления приложения, который мешает успешно загрузить виртуальную машину. В этой статье подробно описано, как с помощью портала Azure подключить виртуальный жесткий диск к другой виртуальной машине Windows для устранения ошибок, а затем восстановить исходную виртуальную машину. 

## <a name="recovery-process-overview"></a>Обзор процесса восстановления
Процесс устранения неполадок выглядит следующим образом.

1. Удалите виртуальную машину, на которой возникли проблемы, сохранив ее виртуальные жесткие диски.
2. Присоедините и подключите виртуальный жесткий диск к другой виртуальной машине Windows для устранения неполадок.
3. Подключитесь к этой виртуальной машине. Измените файлы или запустите средства, которые нужны для устранения неполадок на исходном виртуальном жестком диске.
4. Отключите и отсоедините виртуальный жесткий диск от виртуальной машины, на которой выполняется устранение неполадок.
5. Создайте другую виртуальную машину, используя исходный виртуальный жесткий диск.

Теперь в виртуальной машине, в которой используется управляемый диск, можно использовать Azure PowerShell, что позволит изменить диск операционной системы для виртуальной машины. Больше не нужно удалять и повторно создавать виртуальную машину. Дополнительные сведения см. в статье [Устранение неполадок с виртуальной машиной Windows при подключении диска операционной системы к виртуальной машине восстановления с помощью Azure PowerShell](troubleshoot-recovery-disks-windows.md).

> [!NOTE]
> Эта статья не относится к виртуальной машине с неуправляемым диском.

## <a name="determine-boot-issues"></a>Выявление проблем при загрузке
Изучите диагностические сведения о загрузке и снимки экрана виртуальной машины, чтобы определить, почему виртуальная машина не может правильно загрузиться. Например, причиной может быть сбой обновления приложения, удаление или перемещение базового виртуального жесткого диска и т. п.

Выберите на портале эту виртуальную машину и прокрутите экран вниз до раздела **Поддержка и устранение неполадок**. Щелкните **Boot diagnostics** (Диагностика загрузки), чтобы просмотреть снимок экрана. Обратите внимание на сообщения об ошибках или коды ошибок, чтобы определить причины проблемы с виртуальной машиной.

![Просмотр журналов консоли для диагностики загрузки виртуальной машины](./media/troubleshoot-recovery-disks-portal-windows/screenshot-error.png)

Можно также щелкнуть **Скачать снимок экрана**, чтобы скачать снимок экрана виртуальной машины.

## <a name="view-existing-virtual-hard-disk-details"></a>Просмотр сведений для существующего виртуального жесткого диска
Прежде чем присоединить виртуальный жесткий диск к другой виртуальной машине, следует определить имя этого виртуального жесткого диска.

Выберите виртуальную машину с проблемой, а затем выберите **диски**. Запишите имя диска операционной системы, как показано в следующем примере:

![Выбор больших двоичных объектов для хранения](./media/troubleshoot-recovery-disks-portal-windows/view-disk.png)

## <a name="delete-existing-vm"></a>Удаление существующей виртуальной машины
Виртуальные жесткие диски и виртуальные машины — это разные ресурсы в Azure. Виртуальный жесткий диск является местом хранения операционной системы, приложений и конфигурации. Виртуальная машина — это просто метаданные, которые определяют размер или расположение объекта, а также ссылки на ресурсы, такие как виртуальные жесткие диски или виртуальные сетевые адаптеры. При присоединении виртуального жесткого диска к виртуальной машине для него регистрируется аренда. Диски данных можно присоединять и отсоединять во время работы виртуальной машины, но диск операционной системы отсоединить невозможно, пока ресурс виртуальной машины не будет удален. Аренда сохраняет привязку диска операционной системы к виртуальной машине, даже когда эта виртуальная машина остановлена или отменено ее распределение.

Поэтому для восстановления виртуальной машины нужно прежде всего удалить ресурс виртуальной машины. Удаление виртуальной машины не повлияет на виртуальные жесткие диски, размещенные в учетной записи хранения. После удаления виртуальной машины присоедините виртуальный жесткий диск к другой виртуальной машине, чтобы устранить неполадки.

Выберите на портале нужную виртуальную машину и щелкните **Удалить**.

![Снимок экрана диагностики загрузки виртуальной машины с сообщением об ошибке загрузки](./media/troubleshoot-recovery-disks-portal-windows/stop-delete-vm.png)

Дождитесь, пока удаление завершится, прежде чем присоединять виртуальный жесткий диск к другой виртуальной машине. Чтобы присоединить виртуальный жесткий диск к другой виртуальной машине, необходимо снять аренду, которая привязывает диск к виртуальной машине.

## <a name="attach-existing-virtual-hard-disk-to-another-vm"></a>Присоединение существующего виртуального жесткого диска к другой виртуальной машине
В следующих нескольких шагах описывается использование другой виртуальной машины для устранения неполадок. Присоедините существующий виртуальный жесткий диск к виртуальной машине для устранения неполадок, чтобы можно было просматривать и изменять содержимое диска. Этот процесс позволяет, например, исправить ошибки конфигурации или изучить дополнительные журналы протоколов или системы. Выберите или создайте другую виртуальную машину, которая будет использоваться для устранения неполадок.

1. Выберите на портале группу ресурсов и виртуальную машину для устранения неполадок. Выберите **диски**, выберите **изменить**, а затем щелкните **Добавить диск данных**:

    ![Присоединение существующего диска на портале](./media/troubleshoot-recovery-disks-portal-windows/attach-existing-disk.png)

2. В списке **диски данных** выберите диск операционной системы виртуальной машины, которую вы определили. Если диск операционной системы не отображается, убедитесь, что устранение неполадок виртуальной машины и диска ОС находятся в том же регионе (расположении).
3. Нажмите кнопку **сохранить** , чтобы применить изменения.

## <a name="mount-the-attached-data-disk"></a>Подключение присоединенного диска данных

1. Откройте подключение к удаленному рабочему столу своей виртуальной машины. Выберите на портале свою виртуальную машину и щелкните **Подключиться**. Скачайте и откройте RDP-файл подключения. Введите учетные данные для входа в виртуальную машину, как показано ниже.

    ![Вход в виртуальную машину с помощью удаленного рабочего стола](./media/troubleshoot-recovery-disks-portal-windows/open-remote-desktop.png)

2. Откройте **диспетчер сервера** и выберите **Файловые службы и службы хранилища**. 

    ![Выбор параметра "Файловые службы и службы хранилища" в диспетчере сервера](./media/troubleshoot-recovery-disks-portal-windows/server-manager-select-storage.png)

3. Диск данных будет автоматически обнаружен и подключен. Для просмотра списка подключенных дисков выберите **Диски**. Можно выбрать диск данных, чтобы просмотреть сведения о томах, включая букву диска. В следующем примере показан подключенный диск данных **F**.

    ![Подключенный диск и сведения о томах в диспетчере сервера](./media/troubleshoot-recovery-disks-portal-windows/server-manager-disk-attached.png)


## <a name="fix-issues-on-original-virtual-hard-disk"></a>Устранение неполадок на исходном виртуальном жестком диске
Теперь, когда существующий виртуальный жесткий диск подключен, вы можете выполнить любые необходимые действия по обслуживанию и (или) устранению неполадок. После устранения проблем выполните следующие действия.


## <a name="unmount-and-detach-original-virtual-hard-disk"></a>Отключение и отсоединение исходного виртуального жесткого диска
После устранения ошибок отсоедините существующий виртуальный жесткий диск от виртуальной машины, которую использовали для устранения неполадок. Виртуальный жесткий диск нельзя использовать с другой виртуальной машиной, пока вы не отмените аренду, присоединяющую виртуальный жесткий диск к виртуальной машине для устранения неполадок.

1. В сеансе подключения к удаленному рабочему столу своей виртуальной машины откройте **диспетчер сервера** и выберите **Файловые службы и службы хранилища**.

    ![Выбор параметра "Файловые службы и службы хранилища" в диспетчере сервера](./media/troubleshoot-recovery-disks-portal-windows/server-manager-select-storage.png)

2. Щелкните **Диски** и выберите свой диск данных. Щелкните его правой кнопкой мыши и выберите **Отключить**.

    ![Отключение диска данных в диспетчере сервера](./media/troubleshoot-recovery-disks-portal-windows/server-manager-set-disk-offline.png)

3. Теперь отсоедините виртуальный жесткий диск от виртуальной машины. Выберите на портале Azure виртуальную машину и щелкните **Диски**. 
4. Выберите **изменить**, выберите подключенный диск ОС и нажмите кнопку **отсоединить**:

    ![Отсоединение существующего виртуального жесткого диска](./media/troubleshoot-recovery-disks-portal-windows/detach-disk.png)

    Подождите, пока виртуальная машина успешно отсоединит диск данных, прежде чем продолжать процесс.

## <a name="create-vm-from-original-hard-disk"></a>Создание виртуальной машины из исходного жесткого диска

### <a name="method-1-use-azure-resource-manager-template"></a>Метод 1. Использование шаблона Azure Resource Manager
Чтобы создать виртуальную машину из исходного виртуального жесткого диска, используйте [этот шаблон Azure Resource Manager](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-specialized-vhd-new-or-existing-vnet). Этот шаблон развертывает виртуальную машину в существующую или новую виртуальную сеть, используя URL-адрес виртуального жесткого диска из выполненной выше команды. Нажмите кнопку **Развернуть в Azure**.

![Развертывание виртуальной машины из шаблона GitHub](./media/troubleshoot-recovery-disks-portal-windows/deploy-template-from-github.png)

Шаблон загружается на портал Azure для развертывания. Введите имя для новой виртуальной машины и укажите существующие ресурсы Azure, а затем вставьте URL-адрес исходного виртуального жесткого диска. Чтобы начать развертывание, щелкните **Приобрести**.

![Развертывание виртуальной машины на основе шаблона](./media/troubleshoot-recovery-disks-portal-windows/deploy-from-image.png)

### <a name="method-2-create-a-vm-from-the-disk"></a>Способ 2. Создание виртуальной машины с диска

1. На портал Azure выберите группу ресурсов на портале, а затем найдите диск операционной системы. Вы также можете найти диск с помощью имени диска:

    ![Поиск на диске с портал Azure](./media/troubleshoot-recovery-disks-portal-windows/search-disk.png)
1. Выберите **Обзор**, а затем выберите **создать виртуальную машину**.
    ![Создание виртуальной машины с диска из портал Azure](./media/troubleshoot-recovery-disks-portal-windows/create-vm-from-disk.png)
1. Следуйте указаниям мастера, чтобы создать виртуальную машину.

## <a name="re-enable-boot-diagnostics"></a>Восстановление диагностики загрузки
При создании виртуальной машины на основе существующего виртуального жесткого диска диагностика загрузки не всегда автоматически включена. Выберите эту виртуальную машину на портале, чтобы проверить состояние диагностики загрузки и включить ее, если потребуется. В разделе **Мониторинг** щелкните **Параметры диагностики**. Убедитесь, что выбрано состояние **Включено** и установлен флажок **Диагностика загрузки**. Если вы внесете здесь изменения, нажмите кнопку **Сохранить**.

![Обновление параметров диагностики загрузки](./media/troubleshoot-recovery-disks-portal-windows/reenable-boot-diagnostics.png)

## <a name="next-steps"></a>Следующие шаги
При возникновении проблем с подключением к виртуальной машине ознакомьтесь со статьей [Устранение неполадок с подключением к удаленному рабочему столу на виртуальной машине Azure под управлением Windows](troubleshoot-rdp-connection.md). Для устранения проблем с доступом к приложениям, выполняющимся на виртуальной машине, прочитайте статью [Устранение неполадок доступа к приложению, выполняющемуся в виртуальной машине Azure](troubleshoot-app-connection.md).

Дополнительные сведения об использовании Resource Manager вы найдете в статье [Общие сведения об Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md).

