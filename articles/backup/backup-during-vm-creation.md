---
title: Включить резервное копирование, при создании виртуальной Машины Azure с помощью службы архивации Azure
description: Описывает, как включить архивацию при создании виртуальной Машины Azure с помощью службы архивации Azure.
author: rayne-wiselman
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 06/13/2019
ms.author: raynew
ms.openlocfilehash: a19653f7ae3900fd7999f347ef4d3ef710be1430
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/28/2019
ms.locfileid: "67436336"
---
# <a name="enable-backup-when-you-create-an-azure-vm"></a>Включение резервного копирования при создании виртуальной машины Azure

Используйте службу архивации Azure для резервного копирования виртуальных машин (ВМ) Azure. В соответствии с расписанием, заданным в политику резервного копирования резервного копирования виртуальных машин, и создания точек восстановления из резервных копий. Точки восстановления хранятся в хранилищах служб восстановления.

В этой статье подробно описано, как включить архивацию при создании виртуальной машины (VM) на портале Azure.  

## <a name="before-you-start"></a>Перед началом работы

- [Проверьте](backup-support-matrix-iaas.md#supported-backup-actions) какие операционные системы поддерживаются в том случае, если включить резервное копирование, когда вы создаете виртуальную Машину.

## <a name="sign-in-to-azure"></a>Вход в Azure

Если вы еще не вошли учетную запись, войдите в [портала Azure](https://portal.azure.com).

## <a name="create-a-vm-with-backup-configured"></a>Создание виртуальной Машины с настроенной архивацией

1. На портале Azure щелкните **создать ресурс**.

2. В Azure Marketplace, щелкните **вычислений**, а затем выберите образ виртуальной Машины.

3. Настройка виртуальной Машины в соответствии с [Windows](https://docs.microsoft.com/azure/virtual-machines/windows/quick-create-portal) или [Linux](https://docs.microsoft.com/azure/virtual-machines/linux/quick-create-portal) инструкции.

4. На **управления** на вкладке **включить резервное копирование**, нажмите кнопку **на**.
5. Резервное копирование Azure резервной копии в хранилище служб восстановления. Нажмите кнопку **Create New** Если у вас нет существующего хранилища.
6. Примите предлагаемые хранилище с именем или указать собственное.
7. Укажите или создайте группу ресурсов, в которой будет находиться хранилище. Хранилище группы ресурсов может отличаться от группы ресурсов виртуальной Машины.

    ![Включение резервного копирования для виртуальной Машины](./media/backup-during-vm-creation/enable-backup.png)

8. Примите политику резервного копирования по умолчанию или измените параметры.
    - Политика архивации указывает частоту создания резервных моментальных снимков виртуальной машины и срок их хранения этих резервных копий.
    - Политика по умолчанию резервная копия виртуальной Машины один раз в день.
    - Вы можете настроить собственные политики архивации для виртуальной Машины Azure создавать резервные копии ежедневно или еженедельно.
    - [Дополнительные сведения](backup-azure-vms-introduction.md#backup-and-restore-considerations) о вопросы резервного копирования для виртуальных машин Azure.
    - [Дополнительные сведения](backup-instant-restore-capability.md) о момент восстановления работоспособности.

      ![Политика архивации по умолчанию](./media/backup-during-vm-creation/daily-policy.png)


> [!NOTE]
> Служба Azure Backup создает отдельную группу ресурсов (отличное от группы ресурсов виртуальной Машины) для хранения моментального снимка с формат именования **AzureBackupRG_geography_number** (пример: AzureBackupRG_northeurope_1). Данные в этой группе ресурсов, которые будут храниться в течение дня, как указано в *сохранить моментальный снимок мгновенного восстановления* разделе политики резервного копирования виртуальных машин Azure.  Применения блокировки к этой группе ресурсов может привести к сбоям резервного копирования.<br>
Эта группа ресурсов также должны быть исключены из каких-либо ограничений и тега. имя, так как политика ограниченного использования программ будет блокировать создание коллекции точек ресурсов в ней еще раз приводит к сбоям резервного копирования.


## <a name="start-a-backup-after-creating-the-vm"></a>Начать резервное копирование после создания виртуальной Машины

Резервного копирования виртуальной Машины будет выполняться в соответствии с политикой резервного копирования. Тем не менее мы рекомендуем запустить начальное резервное копирование.

После создания виртуальной Машины, сделайте следующее:

1. В свойствах виртуальной Машины, выберите **резервного копирования**. Состояние виртуальной Машины ожидается начальной резервной копии до запуска начальное резервное копирование
2. Нажмите кнопку **моментальная Архивация** чтобы выполнять резервное копирование по запросу.

    ![Выполнение резервного копирования по требованию](./media/backup-during-vm-creation/run-backup.png)

## <a name="use-a-resource-manager-template-to-deploy-a-protected-vm"></a>Использовать шаблон Resource Manager для развертывания защищенной виртуальной Машины

Предыдущих шагах объясняется, как использовать портал Azure для создания виртуальной машины и защитить его в хранилище служб восстановления. Чтобы быстро развернуть один или несколько виртуальных машин и защитить их в хранилище служб восстановления, см. в разделе шаблона [развернуть виртуальную Машину Windows и включить резервное копирование](https://azure.microsoft.com/resources/templates/101-recovery-services-create-vm-and-configure-backup/).



## <a name="next-steps"></a>Дальнейшие действия

Теперь, когда виртуальная машина защищена, узнайте, как управлять и восстанавливать их.

- [Управление и мониторинг виртуальных машин](backup-azure-manage-vms.md)
- [Восстановление виртуальной машины](backup-azure-arm-restore-vms.md)

Если возникнут проблемы, [просмотрите](backup-azure-vms-troubleshoot.md) руководство по устранению неполадок.
