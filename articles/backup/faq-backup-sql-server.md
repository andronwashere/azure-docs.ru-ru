---
title: Часто задаваемые вопросы о резервном копировании баз данных SQL Server на виртуальных машинах Azure с помощью Azure Backup
description: Найдите ответы на часто задаваемые вопросы о резервном копировании SQL Server баз данных на виртуальных машинах Azure с Azure Backup.
author: sachdevaswati
manager: vijayts
ms.service: backup
ms.topic: conceptual
ms.date: 04/23/2019
ms.author: vijayts
ms.openlocfilehash: 082817899adc6ab9b4d57f7e0d4bc4e7c2f0a2ab
ms.sourcegitcommit: c72ddb56b5657b2adeb3c4608c3d4c56e3421f2c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/24/2019
ms.locfileid: "68464816"
---
# <a name="faq-about-sql-server-databases-that-are-running-on-an-azure-vm-backup"></a>Часто задаваемые вопросы о SQL Server базах данных, работающих в резервной копии виртуальной машины Azure

В этой статье содержатся ответы на часто задаваемые вопросы о резервном копировании SQL Server баз данных, работающих на виртуальных машинах Azure и использующих службу [Azure Backup](backup-overview.md) .

## <a name="can-i-use-azure-backup-for-iaas-vm-as-well-as-sql-server-on-the-same-machine"></a>Можно ли использовать службу архивации Azure для виртуальной машины IaaS, а также SQL Server на одном компьютере?
Да, резервное копирование виртуальных машин и резервное копирование SQL можно выполнять на одной виртуальной машине. В этом случае мы внутренним образом активируем полную резервную копию только для копирования на виртуальной машине, чтобы не усекать журналы.


## <a name="does-the-solution-retry-or-auto-heal-the-backups"></a>Выполняет ли решение повторную попытку или автоматическое восстановление резервных копий?

В некоторых обстоятельствах Azure Backupная служба активирует резервные копии носителей. Автоматическое восстановление может происходить для любого из шести перечисленных ниже условий.

  - Если журнал или разностная резервная копия не удается выполнить из-за ошибки проверки номера LSN, следующий журнал или разностная резервная копия преобразуются в полную резервную копию.
  - Если полная резервная копия не была создана до создания журнала или разностной резервной копии, то этот журнал или разностная резервная копия преобразуется в полную резервную копию.
  - Если последняя полная резервная копия на момент времени старше 15 дней, следующий журнал или разностная резервная копия преобразуются в полную резервную копию.
  - Все задания резервного копирования, которые будут отменены из-за обновления расширения, повторно активируются после завершения обновления и запуска расширения.
  - Если вы решили перезаписать базу данных во время восстановления, произойдет сбой следующей резервной копии журнала или разностного копирования, а затем будет активирована полная резервная копия.
  - В случаях, когда требуется полная резервная копия для сброса цепочек журналов из-за изменения в модели восстановления базы данных, полная активация активируется автоматически в следующем расписании.

Функция автовосстановления по умолчанию включена для всех пользователей. Однако если вы решили отказаться от него, выполните следующие действия.

  * На экземпляре SQL Server в папке *C:\Program Files\azure Multi рабочей нагрузки баккуп\бин* Создайте или измените файл **екстенсионсеттингсоверридес. JSON** .
  * В **екстенсионсеттингсоверридес. JSON**задайте *{"енаблеаутохеалер": false}* .
  * Сохраните изменения и закройте файл.
  * На SQL Server экземпляр откройте **задачу Управление** , а затем перезапустите службу **азуревлбаккупкурдинаторсвк** .  

## <a name="can-i-control-as-to-how-many-concurrent-backups-run-on-the-sql-server"></a>Можно ли контролировать, сколько параллельных операций резервного копирования выполняется на сервере SQL Server?

Да. Вы можете регулировать частоту, с которой выполняется политика резервного копирования, чтобы свести к минимуму влияние на экземпляр SQL Server. Чтобы изменить этот параметр:
1. На экземпляре SQL Server в папке *C:\Program Files\azure Multi рабочей нагрузки баккуп\бин* создайте файл *екстенсионсеттингсоверридес. JSON* .
2. В файле *екстенсионсеттингсоверридес. JSON* измените параметр **дефаултбаккуптаскссрешолд** на более низкое значение (например, 5). <br>
  `{"DefaultBackupTasksThreshold": 5}`

3. Сохраните изменения и закройте файл.
4. На экземпляре SQL Server откройте **диспетчер задач**. Перезагрузите службу **AzureWLBackupCoordinatorSvc**.<br/> <br/>
 Хотя этот метод помогает в том, что приложение резервного копирования потребляет много ресурсов, SQL Server [Resource Governor](https://docs.microsoft.com/sql/relational-databases/resource-governor/resource-governor?view=sql-server-2017) является более универсальным способом указания ограничений на объем ЦП, физических операций ввода-вывода и памяти, которые могут использоваться входящими запросами приложения.

> [!NOTE]
> В UX вы по-прежнему можете запланировать столько резервных копий в любой момент времени, однако они будут обрабатываться в скользящем окне, скажем, 5, как в примере выше.

## <a name="can-i-run-a-full-backup-from-a-secondary-replica"></a>Можно ли запустить полное резервное копирование из вторичной реплики?
В соответствии с ограничениями SQL можно запускать только полную резервную копию на вторичной реплике; Однако полное резервное копирование запрещено.

## <a name="can-i-protect-availability-groups-on-premises"></a>Можно ли защитить группы доступности локально?
Нет. Azure Backup защищает базы данных SQL Server, работающие в Azure. Если группа доступности (AG) распределена между Azure и локальными компьютерами, то для нее можно использовать защиту, только если первичная реплика выполняется в Azure. Кроме того, Azure Backup защищает только узлы, работающие в том же регионе Azure, что и хранилище служб восстановления.

## <a name="can-i-protect-availability-groups-across-regions"></a>Можно ли защитить группы доступности в разных регионах?
Хранилище служб восстановления Azure Backup может обнаруживать и защищать все узлы, наявляющиеся в том же регионе, что и хранилище. Если группа доступности SQL Server Always On охватывает несколько регионов Azure, настройте резервное копирование из региона, в котором находится основной узел. Azure Backup может обнаруживать и защищать все базы данных в группе доступности в соответствии с установленным параметром резервного копирования. Если настройка резервного копирования не соблюдается, резервное копирование завершается сбоем и появляется предупреждение о сбое.

## <a name="do-successful-backup-jobs-create-alerts"></a>Отправляются ли оповещения об успешно выполненных заданиях резервного копирования?
Нет. Для успешно выполненных заданий резервного копирования не создаются оповещения. Оповещения отправляются только в случае сбоя. Подробное поведение оповещений портала описано [здесь](backup-azure-monitoring-built-in-monitor.md). Тем не менее, если вы заинтересованы в наличии оповещений даже для успешного выполнения заданий, можно использовать [мониторинг с помощью Azure Monitor](backup-azure-monitoring-use-azuremonitor.md).

## <a name="can-i-see-scheduled-backup-jobs-in-the-backup-jobs-menu"></a>Можно ли просмотреть запланированные задания резервного копирования в меню задания резервного копирования?
В меню " **Задание архивации** " отображаются только задания резервного копирования с нерегламентированными правами. Для отслеживания использования запланированных заданий [с помощью Azure Monitor](backup-azure-monitoring-use-azuremonitor.md).

## <a name="are-future-databases-automatically-added-for-backup"></a>Добавляются ли автоматически будущие базы данных в резервное копирование?
Да, эту возможность можно реализовать с помощью [автоматической защиты](backup-sql-server-database-azure-vms.md#enable-auto-protection).  

## <a name="if-i-delete-a-database-from-an-autoprotected-instance-what-will-happen-to-the-backups"></a>Что произойдет с резервным копированием, если удалить базу данных из автозащищаемого экземпляра?
Если база данных удаляется из автозащищаемого экземпляра, резервные копии базы данных по-прежнему будут пытаться. Это означает, что удаленная база данных начнет отображаться как неработоспособная в рамках **резервного копирования элементов** и все еще защищена.

Правильный способ отключения защиты этой базы данных — **Отключить резервное копирование** с **удалением данных** в этой базе данных.  

## <a name="if-i-do-stop-backup-operation-of-an-autoprotected-database-what-will-be-its-behavior"></a>Что будет поведению при выполнении резервного копирования базы данных с автозащитой?
Если вы **останавливаете резервное копирование с сохранением данных**, дальнейшие резервные копии не будут выполняться, а существующие точки восстановления останутся без изменений. База данных по-прежнему будет рассматриваться как защищенная и должна отображаться в архивных элементах.

Если вы **останавливаете резервное копирование с удалением данных**, последующие резервные копии не будут выполняться, а существующие точки восстановления также будут удалены. База данных будет считаться незащищенной и будет отображаться в экземпляре в разделе Настройка резервного копирования. Однако в отличие от других защищенных баз данных, которые могут быть выбраны вручную или автоматически защищены, эта база данных отображается серым цветом и не может быть выбрана. Единственный способ повторно защитить эту базу данных — отключить автоматическую защиту для экземпляра. Теперь можно выбрать эту базу данных и настроить ее защиту или повторно включить автоматическую защиту для экземпляра.

## <a name="if-i-change-the-name-of-the-database-after-it-has-been-protected-what-will-be-the-behavior"></a>Что будет поведению при изменении имени базы данных после ее защиты?
База данных с переименованием рассматривается как новая база данных. Таким образом, служба будет рассматривать эту ситуацию так, как если бы база данных не была найдена и в ней не было ошибок резервного копирования.

Можно выбрать базу данных, которая будет переименована, и настроить для нее защиту. Если на экземпляре включена автоматическая защита, переименованная база данных будет автоматически обнаружена и защищена.

##  <a name="why-cant-i-see-an-added-database-for-an-autoprotected-instance"></a>Почему я не вижу добавленной базы данных для автозащищенного экземпляра?
База данных, [добавляемая в автозащищаемый экземпляр](backup-sql-server-database-azure-vms.md#enable-auto-protection) , может не сразу отображаться в разделе защищенные элементы. Это обусловлено тем, что обнаружение обычно выполняется раз в 8 часов. Однако вы можете сразу обнаружить и защитить новые базы данных, если вручную выполнить обнаружение, выбрав **восстановить баз данных**, как показано на следующем рисунке.

  ![Обнаружение вновь добавленной базы данных вручную](./media/backup-azure-sql-database/view-newly-added-database.png)


## <a name="next-steps"></a>Следующие шаги

Узнайте, как [создать резервную копию базы данных SQL Server](backup-azure-sql-database.md) , которая работает на виртуальной машине Azure.
