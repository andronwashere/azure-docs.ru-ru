---
title: Перемещение хранилища Служб восстановления между подписками Azure или в другую группу ресурсов
description: Инструкции по перемещению хранилища Служб восстановления между подписками Azure и группами ресурсов.
author: sogup
manager: vijayts
ms.service: backup
ms.topic: conceptual
ms.date: 04/08/2019
ms.author: sogup
ms.openlocfilehash: 0e351a842a303430e66a52a87305cba23f774ef2
ms.sourcegitcommit: c72ddb56b5657b2adeb3c4608c3d4c56e3421f2c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/24/2019
ms.locfileid: "68465817"
---
# <a name="move-a-recovery-services-vault-across-azure-subscriptions-and-resource-groups"></a>Перемещение хранилища служб восстановления между подписками Azure и группами ресурсов

В этой статье объясняется, как переместить хранилище Служб восстановления, настроенное для Azure Backup, между подписками Azure или в другую группу ресурсов в той же подписке. Для перемещения хранилища Служб восстановления можно использовать портал Azure или PowerShell.

## <a name="supported-region"></a>Поддерживаемый регион

Перемещение ресурсов для хранилища служб восстановления поддерживается в Восточная Австралия, Юго-Восточной Австралии, центральном Канаде, Восточной Азии, Южной Восточная Азия, Восточная Азия, центральной части США, северо-центральном регионе США, Восточная часть США, восток США 2, Юго-Центральный регион США, Западная Центральная часть US, Запад Центральной США 2, Западная часть США, Центральная Индия, Южная Индия, Восточная Япония, Западная Япония, Центральная Корея, Южная Корея, Северная Европа, Западная Европа, Северная Африка, Юго-Восточная Африка, южная часть Соединенного Королевства и западная часть Соединенного Королевства.

## <a name="prerequisites-for-moving-recovery-services-vault"></a>Предварительные требования для перемещения хранилища служб восстановления

- Во время перемещения хранилища между группами ресурсов исходная и Целевая группы ресурсов блокируются, предотвращая операции записи и удаления. Дополнительные сведения см. в этой [статье](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-move-resources).
- Только подписка администратора имеет разрешения на перемещение хранилища.
- Для перемещения хранилища между подписками Целевая подписка должна находиться в том же клиенте, что и исходная подписка, и ее состояние должно быть включено.
- Вам необходимо получить разрешение на выполнение операций записи в целевой группе ресурсов.
- Перемещение хранилища изменяет только группу ресурсов. Хранилище служб восстановления будет находиться в том же расположении и не может быть изменено.
- За один раз можно переместить только одно хранилище служб восстановления на регион.
- Если виртуальная машина не перемещается в хранилище служб восстановления между подписками или в новую группу ресурсов, текущие точки восстановления виртуальной машины останутся в хранилище неизменными до истечения срока их действия.
- Независимо от того, была ли виртуальная машина перемещена с хранилищем или нет, вы в любом случае можете восстановить виртуальную машину из сохраненного журнала резервного копирования в хранилище.
- Для шифрования дисков Azure требуется, чтобы хранилище ключей и виртуальные машины находились в одном регионе Azure и подписке.
- Чтобы узнать, как переместить виртуальную машину с управляемыми дисками, обратитесь к [этой статье](https://azure.microsoft.com/blog/move-managed-disks-and-vms-now-available/).
- Процедуры перемещения ресурсов, развернутых с помощью классической модели, отличаются в зависимости от того, перемещаются ли ресурсы в пределах одной подписки, или они переносятся в новую подписку. Дополнительные сведения см. в этой [статье](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-move-resources).
- Политики службы Backup, определенные для хранилища, сохраняются после перемещения хранилища между подписками или в новую группу ресурсов.
- Перемещение хранилища с помощью файлов Azure, Синхронизация файлов Azure или SQL в виртуальных машинах IaaS по подпискам и группам ресурсов не поддерживается.
- Если вы перемещаете хранилище с данными резервного копирования виртуальной машины между подписками, вы должны переместить виртуальные машины в одну подписку и использовать одну целевую группу ресурсов для продолжения резервного копирования.<br>

> [!NOTE]
>
> Хранилища Служб восстановления, настроенные для использования с **Azure Site Recovery**, еще недоступны для перемещения. Если вы настроили виртуальные машины (Azure IaaS, Hyper-V, VMware) или физические компьютеры для аварийного восстановления с помощью **Azure Site Recovery**, операция перемещения будет заблокирована. Функция перемещения ресурсов для службы Site Recovery еще недоступна.


## <a name="use-azure-portal-to-move-recovery-services-vault-to-different-resource-group"></a>Использование портал Azure для перемещения хранилища служб восстановления в другую группу ресурсов

Для перемещения хранилища Служб восстановления и его связанных ресурсов в другую группу ресурсов выполните следующие инструкции.

1. Войдите на [портале Azure](https://portal.azure.com/).
2. Откройте список **Хранилища служб восстановления** и выберите хранилище, которое нужно переместить. Открытая панель мониторинга хранилища выглядит, как показано на следующем рисунке.

   ![Открытое хранилище Служб восстановления](./media/backup-azure-move-recovery-services/open-recover-service-vault.png)

   Если вы не видите информацию о хранилище в разделе **Основное**, щелкните значок раскрывающегося списка. Раздел "Основное" будет развернут, и вы увидите основную информацию о хранилище.

   ![Вкладка с основными сведениями](./media/backup-azure-move-recovery-services/essentials-information-tab.png)

3. В меню обзора хранилища щелкните ссылку **изменить** рядом с полем **Группа ресурсов**, чтобы открыть колонку **Перемещение ресурсов**.

   ![Изменение группы ресурсов](./media/backup-azure-move-recovery-services/change-resource-group.png)

4. В колонке **Перемещение ресурсов** для выбранного хранилища рекомендуется переместить дополнительные связанные ресурсы, установив флажок, как показано на следующем рисунке.

   ![Перемещение подписки](./media/backup-azure-move-recovery-services/move-resource.png)

5. Чтобы добавить целевую группу ресурсов, в раскрывающемся списке **Группа ресурсов** выберите имеющуюся группу ресурсов или щелкните параметр **Создать группу**.

   ![Создание ресурса](./media/backup-azure-move-recovery-services/create-a-new-resource.png)

6. После добавления группы ресурсов установите флажок **Я понимаю, что инструменты и скрипты, связанные с перемещенными ресурсами, не будут работать, пока не будут обновлены используемые ими идентификаторы ресурсов** и нажмите кнопку **ОК**, чтобы завершить перемещение хранилища.

   ![Сообщение с подтверждением](./media/backup-azure-move-recovery-services/confirmation-message.png)


## <a name="use-azure-portal-to-move-recovery-services-vault-to-a-different-subscription"></a>Использование портал Azure для перемещения хранилища служб восстановления в другую подписку

Для перемещения хранилища Служб восстановления и его связанных ресурсов в другую подписку выполните следующие инструкции.

1. Войдите на [портале Azure](https://portal.azure.com/).
2. Откройте список "Хранилища служб восстановления" и выберите хранилище, которое нужно переместить. Открытая панель мониторинга хранилища выглядит, как показано на следующем рисунке.

    ![Открытое хранилище Служб восстановления](./media/backup-azure-move-recovery-services/open-recover-service-vault.png)

    Если вы не видите информацию о хранилище в разделе **Основное**, щелкните значок раскрывающегося списка. Раздел "Основное" будет развернут, и вы увидите основную информацию о хранилище.

    ![Вкладка с основными сведениями](./media/backup-azure-move-recovery-services/essentials-information-tab.png)

3. В меню обзора хранилища щелкните **изменить** рядом с полем **Подписка**, чтобы открыть колонку **Перемещение ресурсов**.

   ![Изменение подписки](./media/backup-azure-move-recovery-services/change-resource-subscription.png)

4. Выберите ресурсы для перемещения. На этом шаге рекомендуется применить параметр **Выбрать все**, чтобы выбрать все перечисленные дополнительные ресурсы.

   ![Перемещение ресурса](./media/backup-azure-move-recovery-services/move-resource-source-subscription.png)

5. Из раскрывающегося списка **Подписка** выберите целевую подписку, в которую следует переместить хранилище.
6. Чтобы добавить целевую группу ресурсов, в раскрывающемся списке **Группа ресурсов** выберите имеющуюся группу ресурсов или щелкните параметр **Создать группу**.

   ![Добавить подписку](./media/backup-azure-move-recovery-services/add-subscription.png)

7. Установите флажок **Я понимаю, что инструменты и скрипты, связанные с перемещенными ресурсами, не будут работать, пока не будут обновлены используемые ими идентификаторы ресурсов**, а затем нажмите кнопку **OК**.

> [!NOTE]
> Резервное копирование между подписками (хранилище Служб восстановления и защищенные виртуальные машины находятся в разных подписках) не поддерживается. Кроме того, параметр избыточности хранилища нельзя изменить с локального избыточного хранилища (LRS) до глобального избыточного хранилища (GRS) и наоборот во время выполнения операции перемещения хранилища.
>
>

## <a name="use-powershell-to-move-recovery-services-vault"></a>Перемещение хранилища служб восстановления с помощью PowerShell

Чтобы переместить хранилище Служб восстановления в другую группу ресурсов, используйте командлет `Move-AzureRMResource`. Для `Move-AzureRMResource` требуется указать имя и тип ресурса. Эти данные можно получить, выполнив командлет `Get-AzureRmRecoveryServicesVault`.

```
$destinationRG = "<destinationResourceGroupName>"
$vault = Get-AzureRmRecoveryServicesVault -Name <vaultname> -ResourceGroupName <vaultRGname>
Move-AzureRmResource -DestinationResourceGroupName $destinationRG -ResourceId $vault.ID
```

Чтобы переместить ресурсы в другую подписку, добавьте параметр `-DestinationSubscriptionId`.

```
Move-AzureRmResource -DestinationSubscriptionId "<destinationSubscriptionID>" -DestinationResourceGroupName $destinationRG -ResourceId $vault.ID
```

После выполнения указанных выше командлетов вам будет предложено подтвердить перемещение указанных ресурсов. Чтобы подтвердить, введите **Y**. После успешной проверки начнется перемещение ресурса.

## <a name="use-cli-to-move-recovery-services-vault"></a>Перемещение хранилища служб восстановления с помощью интерфейса командной строки

Чтобы переместить хранилище Служб восстановления в другую группу ресурсов, используйте следующий командлет:

```
az resource move --destination-group <destinationResourceGroupName> --ids <VaultResourceID>
```

Для перемещения в новую подписку нужно указать параметр `--destination-subscription-id`.

## <a name="post-migration"></a>Действия после перемещения

1. Вам необходимо настроить и проверить элементы управления доступом для групп ресурсов.  
2. После завершения перемещения для хранилища необходимо еще раз настроить отчеты службы Backup и функцию мониторинга. Во время операции перемещения будет утеряна предыдущая конфигурация.



## <a name="next-steps"></a>Следующие шаги

Вы можете перемещать разные типы ресурсов между группами ресурсов и подписками.

Дополнительные сведения см. в статье [Перемещение ресурсов в новую группу ресурсов или подписку](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-move-resources).
