---
title: Перенести существующие стандартные пространства имен служебной шины Azure уровня "премиум" | Документация Майкрософт
description: Руководство, чтобы разрешить перенос существующих стандартные пространства имен служебной шины Azure уровня "премиум"
services: service-bus-messaging
documentationcenter: ''
author: axisc
manager: darosa
editor: spelluru
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/18/2019
ms.author: aschhab
ms.openlocfilehash: 57ab281e8d07537c22bd3cf60306dfb1c7e81541
ms.sourcegitcommit: d2785f020e134c3680ca1c8500aa2c0211aa1e24
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/04/2019
ms.locfileid: "67566067"
---
# <a name="migrate-existing-azure-service-bus-standard-namespaces-to-the-premium-tier"></a>Перенести существующие стандартные пространства имен служебной шины Azure уровня "премиум"
Ранее служебная шина Azure следующих пространств имен только на уровень "стандартный". Пространства имен, настройки нескольких клиентов, которые оптимизированы для Низкая пропускная способность и сред разработки. Уровень "премиум" предлагает выделенных ресурсов на пространство имен для Прогнозируемая задержка и увеличения пропускной способности по фиксированной цене. Уровень "премиум" оптимизировано для высокой пропускной способностью и рабочих средах, требующих дополнительных корпоративных функций.

В этой статье описывается, как перенести существующие пространства имен уровня "стандартный" до уровня "премиум".  

>[!WARNING]
> Миграции предназначен для служебной шины стандартные пространства имен должны быть обновлены до уровня "премиум". Средство миграции не поддерживает возврат консолей.

Некоторые аспекты, которые Обратите внимание: 
- Этот вид миграции предназначен для происходить в месте, это означает, что существующие приложениями отправителя и получателя **не требуется вносить изменения в коде или конфигурации**. Существующая строка соединения автоматически укажет новое пространство имен уровня "премиум".
- **Уровня "премиум"** пространство имен должно иметь **сущности не** в нем для успешного выполнения миграции. 
- Все **сущностей** в стандартное пространство имен являются **копируются** к пространству имен уровня "премиум" во время процесса миграции. 
- Поддерживает миграцию **1000 сущностей, на единицу обмена сообщениями** на уровень "премиум". Чтобы определить, сколько единиц обмена сообщениями, вам потребуется, начинающихся с цифры сущностей, находящихся на текущем стандартные пространства имен. 
- Нельзя напрямую перенести из **уровня "базовый"** для **уровня premier**, но вы можете сделать это косвенным образом, путем переноса из базовых стандартных первым, а затем из "стандартный" до уровня "премиум", на следующем шаге.

## <a name="migration-steps"></a>Этапы миграции
Некоторые условия связаны с процессом переноса. Ознакомьтесь с следующие действия, чтобы уменьшить вероятность возникновения ошибок. Эти описан процесс миграции и пошаговые инструкции, приведенные в следующих разделах.

1. Создайте новое пространство имен уровня "премиум".
1. Пары "стандартный" и "премиум" пространства имен, друг с другом.
1. Синхронизации (копирования отработка отказа) сущности из стандартного пространства имен уровня "премиум".
1. Фиксации миграции.
1. Очистите сущностей в стандартное пространство имен, используя имя после миграции пространства имен.
1. Удаление стандартного пространства имен.

>[!IMPORTANT]
> После миграции не будет зафиксирована, доступ к старой стандартное пространство имен и очистки очереди и подписки. После сообщения были остановлены, они могут отправляться в новое пространство имен уровня "премиум" для обработки приложений получателя. После потери очередей и подписок, мы рекомендуем удалить старый стандартное пространство имен.

### <a name="migrate-by-using-the-azure-cli-or-powershell"></a>Перенос с помощью Azure CLI или PowerShell

Чтобы перенести стандартное пространство имен служебной шины уровня "премиум" с помощью средства PowerShell или Azure CLI, выполните следующие действия.

1. Создайте новое пространство имен служебной шины уровня "премиум". Вы можете ссылаться на [шаблонов Azure Resource Manager](service-bus-resource-manager-namespace.md) или [с помощью портала Azure](service-bus-create-namespace-portal.md). Не забудьте выбрать **уровня "премиум"** для **serviceBusSku** параметра.

1. Задайте следующие переменные среды для упрощения миграции команды.
   ```azurecli
   resourceGroup = <resource group for the standard namespace>
   standardNamespace = <standard namespace to migrate>
   premiumNamespaceArmId = <Azure Resource Manager ID of the premium namespace to migrate to>
   postMigrationDnsName = <post migration DNS name entry to access the standard namespace>
   ```

    >[!IMPORTANT]
    > После миграции/имя псевдонима (post_migration_dns_name) будет использоваться для доступа к старой после миграции стандартное пространство имен. Используется для очистки очереди и подписки, а затем удалите пространство имен.

1. Сопряжение пространства имен "стандартный" и "премиум" и запустите синхронизацию с помощью следующей команды:

    ```azurecli
    az servicebus migration start --resource-group $resourceGroup --name $standardNamespace --target-namespace $premiumNamespaceArmId --post-migration-name $postMigrationDnsName
    ```


1. Проверка состояния переноса с помощью следующей команды:
    ```azurecli
    az servicebus migration show --resource-group $resourceGroup --name $standardNamespace
    ```

    Миграция считается завершенной, когда появятся следующие значения:
    * MigrationState = "Active"
    * pendingReplicationsOperationsCount = 0
    * provisioningState = «Succeeded»

    Кроме того, эта команда отображает конфигурацию для переноса. Установите этот флажок, чтобы убедиться, что значения заданы правильно. Также проверьте пространство имен уровня "премиум" на портале, чтобы убедиться, очереди и разделы будут созданы, и что они совпадают, что существовали в стандартное пространство имен.

1. Фиксировать миграцию, выполнив следующую команду завершения:
   ```azurecli
   az servicebus migration complete --resource-group $resourceGroup --name $standardNamespace
   ```

### <a name="migrate-by-using-the-azure-portal"></a>Перенос с помощью портала Azure

Миграция с помощью портала Azure имеет одного логического потока как миграция с помощью команд. Выполните следующие действия для миграции с помощью портала Azure.

1. На **навигации** меню в левой области выберите **уровня "премиум" Служба "Миграция"** . Нажмите кнопку **приступить к работе** кнопку, чтобы продолжить на следующую страницу.
    ![Целевая страница миграции][]

1. Полный **установки**.
   ![Настройка пространства имен][]
   1. Создайте и назначьте для переноса существующих стандартное пространство имен для пространства имен уровня "премиум".
        ![Настройка пространства имен - создание пространства имен уровня "премиум"][]
   1. Выберите **имя после миграции**. Это имя будет использоваться для доступа к стандартное пространство имен после завершения миграции.
        ![Настройка пространства имен — выбрать имя миграции post][]
   1. Выберите **«Далее»** для продолжения.
1. Объекты синхронизации между пространствами имен "стандартный" и "премиум".
    ![Настройка пространства имен – синхронизации сущности - start][]

   1. Выберите **начать синхронизацию** начинается синхронизация сущностей.
   1. Выберите **Да** в диалоговом окне для подтверждения и запустите синхронизацию.
   1. Подождите, пока не завершится синхронизация. Состояние можно найти в строке состояния.
        ![Пространство имен – синхронизации сущности - ход установки][]
        >[!IMPORTANT]
        > Если вам нужно прервать миграцию по любой причине, просмотрите прерывания потока раздел этого документа.
   1. После завершения синхронизации выберите **Далее** в нижней части страницы.

1. Просмотрите изменения на странице "Сводка". Выберите **завершение миграции** для переключения пространства имен и для завершения миграции.
    ![Переключение пространства имен - меню коммутатора][] отображается на странице подтверждения, после завершения миграции.
    ![Пространство имен switch - успех][]

## <a name="caveats"></a>Предупреждения

Некоторые возможности, предоставляемые на уровне Standard служебной шины Azure не поддерживаются с уровня "премиум" шины службы Azure. Они являются по своей природе, так как на уровне premium предоставляет выделенные ресурсы для предсказуемой пропускной способности и задержки.

Ниже приведен список возможностей, не поддерживаемых уровня "премиум" и их устранение- 

### <a name="express-entities"></a>Экспресс-сущности

   Экспресс-сущностей, которые не зафиксированы все данные сообщения в хранилище не поддерживаются в уровня "премиум". Выделенные ресурсы предоставлены значительные улучшения гарантией того, что данные сохраняются, как и ожидалось от любого предприятия, системы обмена сообщениями.
   
   Во время миграции любой экспресс сущности в стандартные пространства имен создается в пространстве имен уровня "премиум" как сущность отличных от express.
   
   Если вы используете шаблоны Azure Resource Manager (ARM), убедитесь, удалите флаг «enableExpress» из конфигурации развертывания, чтобы автоматические рабочие процессы выполняются без ошибок.

### <a name="partitioned-entities"></a>Секционированных сущностей

   Секционированных сущностей поддерживаются в уровень "стандартный" для предоставления более котором в мультитенантной конфигурации. С помощью предоставления выделенных ресурсов, доступных на пространство имен уровня "премиум" это больше не используется.
   
   Во время миграции любой секционированной сущности в стандартное пространство имен создается в пространстве имен уровня "премиум" как сущность без секционирования.
   
   Если шаблон ARM устанавливает «enablePartitioning», «true» для определенной очереди или раздела, затем она будет игнорироваться компонентом Service broker.

## <a name="faqs"></a>Часто задаваемые вопросы

### <a name="what-happens-when-the-migration-is-committed"></a>Что происходит при фиксации миграции?

После фиксации миграции, строку подключения, на которую указывает стандартное пространство имен будет указывать пространство имен уровня "премиум".

Приложениями отправителя и получателя будет отключиться от стандартного пространства имен и подключиться к пространству имен уровня "премиум", автоматически.

### <a name="what-do-i-do-after-the-standard-to-premium-migration-is-complete"></a>Что следует делать после завершения "стандартный" до уровня "премиум" миграции?

"Стандартный" до уровня "премиум" миграции гарантирует, что метаданные сущности, такие как разделы, подписки и фильтры копируются из стандартного пространства имен для пространства имен уровня "премиум". Данные сообщения, которые были зафиксированы в стандартное пространство имен не будут копироваться из стандартного пространства имен пространства имен уровня "премиум".

Стандартное пространство имен может иметь некоторые сообщения, которые были отправлены и зафиксирована во время миграции был уничтожен. Вручную завершить эти сообщения из стандартного пространства имен и вручную отправить их в пространство имен категории "премиум". Чтобы вручную завершить сообщения, используйте консольное приложение или сценарий, который выполняет сток стандартное пространство имен сущностей с помощью после миграции DNS имя, которое вы указали в командах миграции. Отправьте эти сообщения к пространству имен уровня "премиум", чтобы они могут обрабатываться получателями.

После сообщения были остановлены, удалите стандартное пространство имен.

>[!IMPORTANT]
> После сообщения из стандартного пространства имен были остановлены, удалите стандартное пространство имен. Это важно, так как строка подключения, которые изначально ссылка стандартное пространство имен теперь ссылается на пространство имен уровня "премиум". Больше не нужны стандартное пространство имен. Удаление стандартного пространства имен, которые перенесены помогает избежать проблем с более поздней версии.

### <a name="how-much-downtime-do-i-expect"></a>Как долго ожидать?
Процесс миграции призвана уменьшить время простоя для приложений. Время простоя сокращается с помощью строки подключения, используемого приложениями отправителя и получателя указывал на новое пространство имен уровня "премиум".

Время простоя, в которых приложение ограничено время, необходимое для обновления записи DNS, чтобы она указывала на пространство имен уровня "премиум". Время простоя составляет примерно 5 минут.

### <a name="do-i-have-to-make-any-configuration-changes-while-doing-the-migration"></a>Нужно ли внести изменения конфигурации во время выполнения миграции?
Нет, никаких изменений кода или конфигурации для миграции. Строку подключения, отправителя и получателя приложения используют для доступа к стандартное пространство имен автоматически сопоставляется в качестве псевдонима для пространства имен уровня "премиум".

### <a name="what-happens-when-i-abort-the-migration"></a>Что происходит, когда я прервать миграцию?
Вы можете прервать миграцию, либо с помощью `Abort` команды или с помощью портала Azure. 

#### <a name="azure-cli"></a>Инфраструктура CLI Azure

```azurecli
az servicebus migration abort --resource-group $resourceGroup --name $standardNamespace
```

#### <a name="azure-portal"></a>Портал Azure

![Прервать поток - abort синхронизации][]
![прервать поток - abort завершения][]

При прерывании процесса миграции, он прерывает процесс копирования сущностей (разделы, подписки и фильтры) из стандартного пространства имен уровня "премиум" и прерывает сопряжение.

Строку подключения не обновляется, чтобы она указывала на пространство имен уровня "премиум". Существующие приложения продолжают работать как перед началом миграции.

Тем не менее он не удалять сущности в пространстве имен уровня "премиум" или удалите пространство имен уровня "премиум". Удаление сущностей вручную, если вы решили не переходить к миграции.

>[!IMPORTANT]
> Если решено прервать миграцию, следует удалите пространство имен, которое подготовлено для миграции, чтобы не оплачивать ресурсы категории "премиум".

#### <a name="i-dont-want-to-have-to-drain-the-messages-what-do-i-do"></a>Я не хочу завершить сообщения. Что делать?

Возможно, сообщения, отправленные приложения отправителя и зафиксирована в хранилище через стандартное пространство имен во время миграции, и просто перед фиксацией миграции.

Во время миграции фактические данные/полезные данные сообщения не будут копироваться из стандартного пространства имен уровня "премиум". Сообщения должны быть вручную останавливаются и затем отправляется на пространство имен уровня "премиум".

Тем не менее если вы не хотите вручную очистки и отправки сообщений можно перенести в период планового обслуживания или по обслуживанию, выполните следующие действия.

1. Остановите приложения отправителя. Принимающие приложения будет обрабатывать сообщения, которые в настоящее время находятся в стандартное пространство имен и удалит очереди.
1. После очередей и подписок в стандартное пространство имен пусты, выполните процедуру, описанную ранее для выполнения миграции из стандарта к пространству имен уровня "премиум".
1. После завершения миграции вы можете перезапустить приложения отправителя.
1. Отправителями и получателями теперь автоматически подключится с пространством имен уровня "премиум".

    >[!NOTE]
    > У вас нет останавливать приложения получателя для миграции.
    >
    > После завершения миграции, принимающие приложения отключиться от стандартного пространства имен и автоматически подключиться к пространству имен уровня "премиум".

## <a name="next-steps"></a>Дальнейшие действия

* Дополнительные сведения о [различия между "стандартный" и "премиум" обмена сообщениями](./service-bus-premium-messaging.md).
* Дополнительные сведения о [высокого уровня доступности и аварийное восстановление аспектов для уровня "премиум" служебной шины](service-bus-outages-disasters.md#protecting-against-outages-and-disasters---service-bus-premium).

[Целевая страница миграции]: ./media/service-bus-standard-premium-migration/1.png
[Настройка пространства имен]: ./media/service-bus-standard-premium-migration/2.png
[Настройка пространства имен - создание пространства имен уровня "премиум"]: ./media/service-bus-standard-premium-migration/3.png
[Настройка пространства имен — выбрать имя миграции post]: ./media/service-bus-standard-premium-migration/4.png
[Настройка пространства имен – синхронизации сущности - start]: ./media/service-bus-standard-premium-migration/5.png
[Пространство имен – синхронизации сущности - ход установки]: ./media/service-bus-standard-premium-migration/8.png
[Пространство имен switch - меню коммутатора]: ./media/service-bus-standard-premium-migration/9.png
[Пространство имен switch - успех]: ./media/service-bus-standard-premium-migration/12.png

[Прервать поток - abort синхронизации]: ./media/service-bus-standard-premium-migration/abort1.png
[Прервать поток - abort завершения]: ./media/service-bus-standard-premium-migration/abort3.png
