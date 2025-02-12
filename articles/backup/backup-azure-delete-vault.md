---
title: Удаление хранилища служб восстановления в Azure
description: Описывает, как удалить хранилище служб восстановления.
author: rayne-wiselman
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 07/11/2019
ms.author: raynew
ms.openlocfilehash: f98b9a02d12cc53ba23857b203ee3eaed9dd7cfa
ms.sourcegitcommit: c72ddb56b5657b2adeb3c4608c3d4c56e3421f2c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/24/2019
ms.locfileid: "68466653"
---
# <a name="delete-a-recovery-services-vault"></a>Удаление хранилища Служб восстановления

В этой статье описывается, как удалить хранилище служб восстановления [Azure Backup](backup-overview.md) . В нем содержатся инструкции по удалению зависимостей, а также удалению хранилища.


## <a name="before-you-start"></a>Перед началом

Нельзя удалить хранилище служб восстановления с зависимостями, такими как защищенные серверы или серверы управления архивацией, связанные с хранилищем.<br/>
Невозможно удалить хранилище, содержащее данные резервного копирования (даже если вы остановили защиту, но сохранили данные резервной копии).

При удалении хранилища, содержащего зависимости, отобразится следующая ошибка:

![Ошибка удаления хранилища](./media/backup-azure-delete-vault/error.png)

Чтобы правильно удалить хранилище, выберите сценарий, соответствующий вашей настройке, и следуйте рекомендуемым действиям.

Сценарий | Действия по удалению зависимостей для удаления хранилища |
-- | --
Локальные файлы и папки, защищенные с помощью агента Azure Backup (режим MARS), архивируются в Azure | Выполните действия, описанные в разделе Удаление резервных копий данных и элементов резервного копирования — [для агента Mars](#for-mars-agent) .
У меня есть локальные компьютеры, защищенные с помощью MABS (Microsoft Azure Backup Server) или DPM в Azure (System Center Data Protection Manager) | Выполните действия, описанные в разделе Удаление резервных копий данных и элементов архивации [для агента MABS](#for-mabs-agent) .
У меня есть защищенные элементы в облаке (например, Виртуальная машина Лаас, файловый ресурс Azure и т. д.)  | Выполните действия, описанные в разделе Удаление резервных копий данных и элементов архивации — [для защищенных элементов в облаке](#for-protected-items-in-cloud) .
У меня есть защищенные элементы как в локальной среде, так и в облаке | Выполните действия, описанные в разделе Удаление резервных копий данных и архивных элементов в следующей последовательности: <br> - [Для защищенных элементов в облаке](#for-protected-items-in-cloud)<br> - [Для агента MABS](#for-mars-agent) <br> - [Для агента MABS](#for-mabs-agent)
У меня нет защищенных элементов в локальной среде или в облаке; Однако я по-прежнему получаю сообщение об ошибке удаления хранилища | Выполните действия, описанные в статье [Удаление хранилища служб восстановления с помощью клиента Azure Resource Manager](#delete-the-recovery-services-vault-using-azure-resource-manager-client) .
Я больше не использую исходный локальный сервер (потерян или списан) и хочу удалить хранилище служб восстановления | Обратитесь в службу поддержки Майкрософт.

## <a name="delete-backup-data-and-backup-items"></a>Удаление резервных копий данных и элементов архивации

Прежде чем продолжить, ознакомьтесь с **[этим](#before-you-start)** разделом, чтобы ознакомиться с зависимостями и процессом удаления хранилища.

### <a name="for-protected-items-in-cloud"></a>Для защищенных элементов в облаке

Чтобы отключить защиту и удалить данные резервной копии, выполните следующие действия.

1. На портале > хранилище служб восстановления > элементы резервного копирования выберите защищенные элементы в облаке.

    ![выбор типа резервной копии](./media/backup-azure-delete-vault/azure-storage-selected.jpg)

2. Для каждого элемента необходимо щелкнуть правой кнопкой мыши и выбрать пункт **прерывать резервное копирование**.

    ![выбор типа резервной копии](./media/backup-azure-delete-vault/stop-backup-item.png)

3. В окне « **Завершение резервного копирования** >  **» выберите вариант**« **удалить данные архивации**».
4. Введите имя элемента и нажмите кнопку "прерывать **резервную копию**".
   - Это подтверждает, что вы хотите удалить элемент.
   - Кнопка " **закончить архивацию** " активируется после проверки.
   - Если вы сохранили и не удалили данные, вы не сможете удалить хранилище.

     ![Удалить данные архивации](./media/backup-azure-delete-vault/stop-backup-blade-delete-backup-data.png)

5. Проверьте **уведомление** ![удаление данных](./media/backup-azure-delete-vault/messages.png)резервных копий. После завершения служба отобразит сообщение: **Идет остановка резервного копирования и удаление данных резервного копирования для*элемента Backup*** . **Операция успешно завершена**.
6. Щелкните **Обновить** в меню **архивные элементы** , чтобы проверить, удален ли элемент резервного копирования.

      ![Удалить данные архивации](./media/backup-azure-delete-vault/empty-items-list.png)

### <a name="for-mars-agent"></a>Для агента MARS

Чтобы отключить защиту и удалить данные резервных копий, выполните действия в указанном ниже порядке.

- [Шаг 1. Удаление элементов архивации из консоли управления MARS](#step-1-delete-backup-items-from-mars-management-console)
- [Шаг 2. Из портала удаление Azure Backup агент](#step-1-delete-backup-items-from-mars-management-console)


#### <a name="step-1-delete-backup-items-from-mars-management-console"></a>Шаг 1.: Удаление элементов архивации из консоли управления MARS

Если вы не можете выполнить этот шаг из-за недоступности сервера, обратитесь в службу поддержки Майкрософт.

- Запустите консоль управления MARS, перейдите в область **действия** и выберите **Расписание архивации**.
- В мастере **изменения или завершения архивации** по расписанию выберите параметр прерывать **с помощью этого расписания архивации и удалите все сохраненные резервные копии** и нажмите кнопку **Далее**.

    ![Изменение или остановка архивации по расписанию](./media/backup-azure-delete-vault/modify-schedule-backup.png)

- В мастере **завершения архивации** по расписанию нажмите кнопку **Готово**.

    ![Завершение архивации по расписанию](./media/backup-azure-delete-vault/stop-schedule-backup.png)
- Вам будет предложено ввести ПИН-код безопасности. Чтобы создать ПИН-код, выполните следующие действия.
  - Войдите на портал Azure.
  - Перейдите к разделу **Хранилище службы восстановления** > **Параметры** > **Свойства**.
  - В разделе **ПИН-код безопасности** щелкните **Создать**. Скопируйте этот ПИН-код. (Этот ПИН-код действителен только пять минут)
- В консоли управления (клиентское приложение) Вставьте ПИН-код и нажмите кнопку **ОК**.

  ![ПИН-код безопасности](./media/backup-azure-delete-vault/security-pin.png)

- В мастере **изменения хода архивации** вы увидите *, что удаленные резервные копии данных будут храниться в течение 14 дней. По истечении этого времени данные резервного копирования будут окончательно удалены.*  

    ![Удаление инфраструктуры резервного копирования](./media/backup-azure-delete-vault/deleted-backup-data.png)

Теперь, когда архивные элементы удалены из локальной среды, выполните следующие действия на портале.

#### <a name="step-2-from-portal-remove-azure-backup-agent"></a>Шаг 2.: Из портала удаление Azure Backup агент

Прежде чем продолжить, убедитесь, что [Шаг 1](#step-1-delete-backup-items-from-mars-management-console) завершен.

1. В меню панели мониторинга хранилища выберите пункт **инфраструктура резервного копирования**.
2. Щелкните **защищенные серверы** , чтобы просмотреть серверы инфраструктуры.

    ![выберите хранилище, чтобы открыть его панель мониторинга](./media/backup-azure-delete-vault/identify-protected-servers.png)

3. В списке **Защищенные серверы** выберите агент Azure Backup.

    ![выбор типа резервной копии](./media/backup-azure-delete-vault/list-of-protected-server-types.png)

4. Щелкните сервер в списке серверов, защищенных с помощью агента Azure Backup.

    ![выбор определенного защищенного сервера](./media/backup-azure-delete-vault/azure-backup-agent-protected-servers.png)

5. На выбранной панели мониторинга сервера нажмите кнопку **Удалить**.

    ![удаление выбранного сервера](./media/backup-azure-delete-vault/selected-protected-server-click-delete.png)

6. В меню **Удалить** введите имя сервера и нажмите кнопку **Удалить**.

     ![Удалить данные архивации](./media/backup-azure-delete-vault/delete-protected-server-dialog.png)

> [!NOTE]
> Если вы видите приведенную ниже ошибку, сначала выполните действия, описанные в разделе [Удаление архивных элементов из консоли управления](#step-1-delete-backup-items-from-mars-management-console).
>
>![Сбой удаления](./media/backup-azure-delete-vault/deletion-failed.png)
>
>Если не удается выполнить действия по удалению резервных копий из консоли управления, например из-за недоступности сервера с помощью консоли управления, обратитесь в службу поддержки Майкрософт.

7. Проверьте **уведомление** ![удаление данных](./media/backup-azure-delete-vault/messages.png)резервных копий. После завершения служба отобразит сообщение: **Идет остановка резервного копирования и удаление данных резервного копирования для*элемента Backup*** . **Операция успешно завершена**.
8. Щелкните **Обновить** в меню **архивные элементы** , чтобы проверить, удален ли элемент резервного копирования.


### <a name="for-mabs-agent"></a>Для агента MABS

Чтобы отключить защиту и удалить данные резервных копий, выполните действия в указанном ниже порядке.

- [Шаг 1. Удаление архивных элементов из консоли управления MABS](#step-1-delete-backup-items-from-mabs-management-console)
- [Шаг 2. Удаление Azure Backup серверов управления с портала](#step-2-from-portal-remove-azure-backup-agent)

#### <a name="step-1-delete-backup-items-from-mabs-management-console"></a>Шаг 1.: Удаление архивных элементов из консоли управления MABS

Если вы не можете выполнить этот шаг из-за недоступности сервера, обратитесь в службу поддержки Майкрософт.

**Метод 1** Чтобы отключить защиту и удалить данные резервных копий, выполните следующие действия.

1.  В консоль администрирования DPM щелкните **Защита** на панели навигации.
2.  На панели "дисплей" выберите член группы защиты, который необходимо удалить. Щелкните правой кнопкой мыши, чтобы выбрать команду " **Отключить защиту членов группы** ".
3.  В диалоговом окне " **Завершение защиты** " установите флажок **Удалить защищенные данные** > **удалить хранилище в сети** и нажмите кнопку **Отключить защиту**.

    ![Удаление хранилища в сети](./media/backup-azure-delete-vault/delete-storage-online.png)

Состояние защищенного участника изменено на **доступная реплика**неактивна.

5. Щелкните правой кнопкой мыши неактивная группа защиты и выберите пункт **Удалить неактивную защиту**.

    ![Удалить неактивную защиту](./media/backup-azure-delete-vault/remove-inactive-protection.png)

6. В окне **Удаление неактивной защиты** выберите **Удалить оперативное хранилище** и нажмите кнопку **ОК**.

    ![Удаление реплик на диске и в сети](./media/backup-azure-delete-vault/remove-replica-on-disk-and-online.png)

**Метод 2** Запустите консоль **управления MABS** . В разделе **Выбор метода защиты данных** снимите флажок **требуется оперативная защита**.

  ![Выбор метода защиты данных](./media/backup-azure-delete-vault/data-protection-method.png)

Теперь, когда архивные элементы удалены из локальной среды, выполните следующие действия на портале.

#### <a name="step-2-from-portal-remove-azure-backup-management-servers"></a>Шаг 2.: Удаление Azure Backup серверов управления с портала

Прежде чем продолжить, убедитесь, что [Шаг 1](#step-1-delete-backup-items-from-mabs-management-console) завершен.

1. В меню панели мониторинга хранилища выберите пункт **инфраструктура резервного копирования**.
2. Щелкните **серверы управления архивацией** , чтобы просмотреть серверы.

    ![Выберите хранилище, чтобы открыть его панель мониторинга](./media/backup-azure-delete-vault/delete-backup-management-servers.png)

3. Щелкните элемент правой кнопкой мыши > **Удалить**.
4. В меню **Удалить** введите имя сервера и нажмите кнопку **Удалить**.

     ![Удалить данные архивации](./media/backup-azure-delete-vault/delete-protected-server-dialog.png)

> [!NOTE]
> Если вы видите приведенную ниже ошибку, сначала выполните действия, описанные в разделе [Удаление архивных элементов из консоли управления](#step-2-from-portal-remove-azure-backup-management-servers).
>
>![Сбой удаления](./media/backup-azure-delete-vault/deletion-failed.png)
>
> Если не удается выполнить действия по удалению резервных копий из консоли управления, например из-за недоступности сервера с помощью консоли управления, обратитесь в службу поддержки Майкрософт.

5. Проверьте **уведомление** ![удаление данных](./media/backup-azure-delete-vault/messages.png)резервных копий. После завершения служба отобразит сообщение: **Идет остановка резервного копирования и удаление данных резервного копирования для*элемента Backup*** . **Операция успешно завершена**.
6. Щелкните **Обновить** в меню **архивные элементы** , чтобы проверить, удален ли элемент резервного копирования.


## <a name="delete-the-recovery-services-vault"></a>Удаление хранилища служб восстановления

1. После удаления всех зависимостей перейдите к области **основные компоненты** в меню хранилище.
2. Убедитесь, что в списке отсутствуют **элементы архивации**, **серверы управления архивацией**или реплицированные **элементы** . Если элементы по-прежнему отображаются в хранилище, [удалите их](#delete-backup-data-and-backup-items).

3. Когда на панели мониторинга хранилища не останется элементов, щелкните **Удалить**.

    ![Удалить данные архивации](./media/backup-azure-delete-vault/vault-ready-to-delete.png)

4. Чтобы подтвердить удаление хранилища, нажмите кнопку **Да**. Хранилище удаляется, а на портале открывается меню **Новая служба** .

## <a name="delete-the-recovery-services-vault-using-azure-resource-manager-client"></a>Удаление хранилища служб восстановления с помощью клиента Azure Resource Manager

Этот параметр для удаления хранилища служб восстановления рекомендуется только в том случае, если все зависимости удаляются, и вы по-прежнему получаете *ошибку удаления хранилища*.



- На панели " **основные компоненты** " в меню "хранилище" убедитесь в том, что в списке отсутствуют **элементы архивации**, **серверы управления архивацией**или реплицированные **элементы** . Если имеются архивные элементы, выполните действия, описанные в разделе [удаление резервных копий данных и архивация элементов](#delete-backup-data-and-backup-items).
- Повторите попытку [удаления хранилища с портала](#delete-the-recovery-services-vault).
- Если все зависимости будут удалены и вы по-прежнему получаете *ошибку удаления хранилища* , используйте средство ARMClient для выполнения указанных ниже действий.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

1. Установите шоколад [отсюда и установите](https://chocolatey.org/) ARMClient, выполнив приведенную ниже команду.

   `choco install armclient --source=https://chocolatey.org/api/v2/`
2. Войдите в учетную запись Azure и выполните следующую команду:

    `ARMClient.exe login [environment name]`

3. В портал Azure Соберите идентификатор подписки и имя группы ресурсов для хранилища, которое необходимо удалить.

Дополнительные сведения о команде ARMClient см. в этом [документе](https://github.com/projectkudu/ARMClient/blob/master/README.md).

### <a name="use-azure-resource-manager-client-to-delete-recovery-services-vault"></a>Использование клиента Azure Resource Manager для удаления хранилища служб восстановления

1. Выполните следующую команду, используя идентификатор подписки, имя группы ресурсов и имя хранилища. При выполнении команды она удаляет хранилище, если у вас нет зависимостей.

   ```
   ARMClient.exe delete /subscriptions/<subscriptionID>/resourceGroups/<resourcegroupname>/providers/Microsoft.RecoveryServices/vaults/<recovery services vault name>?api-version=2015-03-15
   ```
2. Если хранилище не пусто, появится сообщение об ошибке "хранилище не может быть удалено, так как в этом хранилище есть ресурсы. Чтобы удалить защищенные элементы или контейнеры в хранилище, выполните следующие действия.

   ```
   ARMClient.exe delete /subscriptions/<subscriptionID>/resourceGroups/<resourcegroupname>/providers/Microsoft.RecoveryServices/vaults/<recovery services vault name>/registeredIdentities/<container name>?api-version=2016-06-01
   ```

3. В портал Azure убедитесь, что хранилище удалено.


## <a name="next-steps"></a>Следующие шаги

[Дополнительные сведения](backup-azure-recovery-services-vault-overview.md) Хранилища служб восстановления.<br/>
[Узнайте, как](backup-azure-manage-windows-server.md) отслеживать хранилища служб восстановления и управлять ими.
