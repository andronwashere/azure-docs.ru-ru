---
title: 'Резервные копии базы данных SQL Azure: автоматически создаваемые и геоизбыточные | Документация Майкрософт'
description: База данных SQL автоматически создает локальную резервную копию базы данных каждые пять минут и использует геоизбыточное хранилище Azure с доступом на чтение для обеспечения геоизбыточности.
services: sql-database
ms.service: sql-database
ms.subservice: backup-restore
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: anosov1960
ms.author: sashan
ms.reviewer: mathoma, carlrab
manager: craigg
ms.date: 06/27/2019
ms.openlocfilehash: c75b19fff478c14ff47996cf9159e48f3ff69724
ms.sourcegitcommit: a6873b710ca07eb956d45596d4ec2c1d5dc57353
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/16/2019
ms.locfileid: "68261193"
---
# <a name="automated-backups"></a>Автоматическое резервное копирование

База данных SQL автоматически создает резервные копии базы данных, которые хранятся в интервале от 7 до 35 дней, и использует геоизбыточное [хранилище с доступом на чтение Azure (RA-GRS)](../storage/common/storage-redundancy-grs.md#read-access-geo-redundant-storage) , чтобы гарантировать их сохранность, даже если центр обработки данных недоступен. Эти резервные копии создаются автоматически. Резервные копии базы данных являются важной частью любой стратегии непрерывности бизнес-процессов и аварийного восстановления, так как они защищают данные от случайного повреждения или удаления. Если для правил безопасности требуется, чтобы резервные копии были доступны в течение продолжительного периода времени (до 10 лет), можно настроить [долгосрочное хранение](sql-database-long-term-retention.md) в одноэлементных базах данных и пулах эластичных БД.

[!INCLUDE [GDPR-related guidance](../../includes/gdpr-intro-sentence.md)]

## <a name="what-is-a-sql-database-backup"></a>Резервная копия базы данных SQL

База данных SQL использует технологию SQL Server для создания [полных резервных копий](https://docs.microsoft.com/sql/relational-databases/backup-restore/full-database-backups-sql-server) каждую неделю, разностные [резервные копии](https://docs.microsoft.com/sql/relational-databases/backup-restore/differential-backups-sql-server) каждые 12 часов и [резервные копии журналов транзакций](https://docs.microsoft.com/sql/relational-databases/backup-restore/transaction-log-backups-sql-server) каждые 5-10 минут. Резервные копии хранятся в [хранилищах данных RA-GRS](../storage/common/storage-redundancy-grs.md#read-access-geo-redundant-storage) , которые реплицируются в [парный центр обработки](../best-practices-availability-paired-regions.md) данных для защиты от сбоя центра обработки данных. При восстановлении базы данных служба сама определяет, какие резервные копии (полные, разностные или резервные копии журналов транзакций) следует восстановить.

Эти резервные копии позволяют выполнить следующие операции:

- **Восстановите существующую базу данных до точки во времени в прошлом** в течение срока хранения с помощью портал Azure, Azure PowerShell, Azure CLI или REST API. В одной базе данных и эластичных пулах эта операция создаст новую базу данных на том же сервере, что и исходная база данных. В Управляемый экземпляр эта операция может создать копию базы данных или ту же или другую Управляемый экземпляр в той же подписке.
  - Чтобы настроить политику архивации, **[измените срок хранения резервных копий](#how-to-change-the-pitr-backup-retention-period)** с 7 до 35 дней.
  - **Измените политику долгосрочного хранения до 10 лет** на отдельная база данных и эластичных пулах с помощью [портал Azure](sql-database-long-term-backup-retention-configure.md#configure-long-term-retention-policies) или [Azure PowerShell](sql-database-long-term-backup-retention-configure.md#use-powershell-to-configure-long-term-retention-policies-and-restore-backups).
- **Восстановление удаленной базы данных до момента ее удаления** или в любое время в течение срока хранения. Удаленную базу данных можно восстановить только на том же логическом сервере или Управляемый экземпляр, где была создана исходная база данных.
- **Восстановление базы данных в другой географический регион**. Геовосстановление позволяет выполнить восстановление после сбоя в регионе при отсутствии доступа к серверу и базе данных. В результате создается база данных на любом существующем сервере в любой точке мира.
- **Восстановление базы данных из определенной долгосрочной резервной копии** на отдельная база данных или эластичный пул, если для базы данных настроена политика долгосрочного хранения (LTR). LTR позволяет восстановить старую версию базы данных с помощью [портал Azure](sql-database-long-term-backup-retention-configure.md#view-backups-and-restore-from-a-backup-using-azure-portal) или [Azure PowerShell](sql-database-long-term-backup-retention-configure.md#use-powershell-to-configure-long-term-retention-policies-and-restore-backups) для удовлетворения запроса на соответствие или запуска старой версии приложения. Дополнительные сведения см. в статье [Storing Azure SQL Database Backups for up to 10 years](sql-database-long-term-retention.md) (Хранение резервных копий баз данных SQL Azure до 10 лет).
- Инструкции по восстановлению см. в статье о [восстановлении базы данных из резервной копии](sql-database-recovery-using-backups.md).

> [!NOTE]
> В службе хранилища Azure термин *репликация* обозначает копирование файлов из одного расположения в другое. *Репликация базы данных SQL* означает хранение нескольких баз данных-получателей, синхронизированных с основной базой данных.

Некоторые из этих операций можно выполнить с помощью следующих примеров.

| | Портал Azure | Azure PowerShell |
|---|---|---|
| Изменение срока хранения резервной копии | [отдельная база данных](sql-database-automated-backups.md#change-pitr-backup-retention-period-using-the-azure-portal) <br/> [Управляемый экземпляр](sql-database-automated-backups.md#change-pitr-for-a-managed-instance) | [отдельная база данных](sql-database-automated-backups.md#change-pitr-backup-retention-period-using-powershell) <br/>[Управляемый экземпляр](https://docs.microsoft.com/powershell/module/az.sql/set-azsqlinstancedatabasebackupshorttermretentionpolicy) |
| Изменение долгосрочного хранения резервных копий | [Отдельная база данных](sql-database-long-term-backup-retention-configure.md#configure-long-term-retention-policies)<br/>Управляемый экземпляр-н/д  | [отдельная база данных](sql-database-long-term-backup-retention-configure.md#use-powershell-to-configure-long-term-retention-policies-and-restore-backups)<br/>Управляемый экземпляр-н/д  |
| Восстановление базы данных с точки во времени | [Отдельная база данных](sql-database-recovery-using-backups.md#point-in-time-restore) | [Отдельная база данных](https://docs.microsoft.com/powershell/module/az.sql/restore-azsqldatabase) <br/> [Управляемый экземпляр](https://docs.microsoft.com/powershell/module/az.sql/restore-azsqlinstancedatabase) |
| Восстановление удаленной базы данных | [Отдельная база данных](sql-database-recovery-using-backups.md#deleted-database-restore-using-the-azure-portal) | [Отдельная база данных](https://docs.microsoft.com/powershell/module/az.sql/get-azsqldeleteddatabasebackup) <br/> [Управляемый экземпляр](https://docs.microsoft.com/powershell/module/az.sql/get-azsqldeletedinstancedatabasebackup)|
| Восстановление базы данных из хранилища BLOB-объектов Azure | Отдельная база данных — н/д <br/>Управляемый экземпляр-н/д  | Отдельная база данных — н/д <br/>[Управляемый экземпляр](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-get-started-restore) |

## <a name="how-long-are-backups-kept"></a>Срок хранения резервных копий

Все базы данных SQL Azure (отдельные, пулы и управляемые экземпляры) имеют стандартный срок хранения резервных копий в **семь** дней. [Срок хранения резервных копий можно изменить до 35 дней](#how-to-change-the-pitr-backup-retention-period).

При удалении базы данных резервные копии для базы данных SQL будут храниться точно так же, как и для базы данных в Интернете. Например, если вы удалите базу данных уровня "Базовый" с периодом хранения семь дней, резервная копия, хранящаяся четыре дня, будет храниться еще три дня.

Если вам нужно сохранить резервные копии на более долгий срок по сравнению с максимальным периодом хранения, вы можете изменить свойства резервной копии, добавив в базу данных один или несколько периодов долгосрочного хранения. Дополнительные сведения см. в статье [Storing Azure SQL Database Backups for up to 10 years](sql-database-long-term-retention.md) (Хранение резервных копий баз данных SQL Azure до 10 лет).

> [!IMPORTANT]
> Если вы удаляете сервер SQL Azure, на котором размещены базы данных SQL, то все эластичные пулы и базы данных, находящиеся на этом сервере, также будут удалены без возможности восстановления. Удаленный сервер восстановить невозможно. Но если вы настроили долгосрочное хранение, резервные копии баз данных с LTR не будут удалены, и вы сможете восстановить эти базы данных.

## <a name="how-often-do-backups-happen"></a>Частота создания резервных копий

### <a name="backups-for-point-in-time-restore"></a>Резервные копии для восстановления на момент времени

База данных SQL поддерживает самообслуживание для восстановления на момент времени (PITR) путем автоматического создания полной резервной копии, разностных резервных копий и резервных копий журналов транзакций. Полные резервные копии базы данных создаются каждую неделю, разностные резервные копии — как правило, каждые 12 часов, а резервные копии журналов транзакций создаются каждые 5 – 10 минут. Периодичность архивации зависит от объема вычислительных ресурсов и числа действий, производимых с базой данных. Первое полное резервное копирование планируется сразу после создания базы данных. Обычно оно занимает не более 30 минут, но для базы данных большого размера может потребоваться больше времени. Например, начальное резервное копирование будет выполняться дольше для восстановленной базы данных или копии базы данных. После первого полного резервного копирования все последующие операции резервного копирования планируются автоматически и управляются без участия пользователя в фоновом режиме. Точное время создания всех копий базы данных определяет служба Базы данных SQL с учетом распределения общей рабочей нагрузки в системе. Нельзя изменить или отключить задания резервного копирования. 

Резервные копии PITR также обладают географической избыточностью и защищены с помощью [репликации службы хранилища Azure между регионами](../storage/common/storage-redundancy-grs.md#read-access-geo-redundant-storage).

Дополнительную информацию см. в статье [Восстановление до точки во времени](sql-database-recovery-using-backups.md#point-in-time-restore).

### <a name="backups-for-long-term-retention"></a>Резервные копии для долгосрочного хранения

Изолированные базы данных и базы данных в пуле предоставляют возможность настройки долгосрочного хранения (LTR) полных резервных копий до 10 лет в хранилище BLOB-объектов Azure. Если политика LTR включена, еженедельные полные резервные копии автоматически копируются в другой контейнер геоизбыточного хранилища с доступом на чтение. Чтобы удовлетворять различные требования к соответствию, вы можете выбрать разные периоды хранения для резервных копий, создаваемых еженедельно, ежемесячно или ежегодно. Использование хранилища зависит от частоты резервного копирования и периода хранения. Вы можете использовать [калькулятор цен LTR](https://azure.microsoft.com/pricing/calculator/?service=sql-database), чтобы определить стоимость долгосрочного хранения резервных копий.

Как и резервные копии PITR, резервные копии LTR также обладают географической избыточностью и защищены с помощью [репликации службы хранилища Azure между регионами](../storage/common/storage-redundancy-grs.md#read-access-geo-redundant-storage).

Дополнительные сведения см. в разделе [Долгосрочное хранение резервных копий](sql-database-long-term-retention.md).

## <a name="storage-costs"></a>Затраты на хранение
Резервные копии баз данных, автоматически созданные за последние 7 дней, будут по умолчанию копироваться в хранилище BLOB-объектов RA-GRS ценовой категории "Стандартный". Хранилище используется для полных резервных копий, созданных за неделю, разностных резервных копий, созданных за день, и резервных копий журналов транзакций, копируемых каждые 5 минут. Размер журнала транзакций зависит от скорости изменения базы данных. Минимальный объем хранилища, соответствующий полному размеру базы данных, предоставляется без дополнительной оплаты. Плата за дополнительное пространство хранилища резервных копий будет взиматься за количество ГБ в месяц.

Дополнительные сведения о ценах на службу хранилища см. на странице [Обзор цен на хранилище Azure](https://azure.microsoft.com/pricing/details/sql-database/single/). 

## <a name="are-backups-encrypted"></a>Шифрование резервных копий

Если база данных зашифрована с использованием прозрачного шифрования, резервные копии, включая резервные копии LTR, будут автоматически шифроваться при хранении. Когда для базы данных Azure SQL включено прозрачное шифрование данных, также шифруются резервные копии. Прозрачное шифрование данных включено по умолчанию для всех новых баз данных SQL Azure. Дополнительные сведения о прозрачном шифровании данных см. в статье [Прозрачное шифрование данных в базе данных SQL Azure](/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql).

## <a name="how-does-microsoft-ensure-backup-integrity"></a>Каким образом корпорация Майкрософт обеспечивает целостность резервной копии?

На постоянной основе группа инженеров по базам данных SQL Azure автоматически проверяет восстановление резервных копий баз данных, размещенных на логических серверах и эластичных пулах (это недоступно в Управляемый экземпляр). При восстановлении до точки во времени базы данных также получают проверки целостности с помощью DBCC CHECKDB.

После завершения миграции управляемый экземпляр автоматически выполняет `CHECKSUM` начальную архивацию баз данных `RESTORE` , восстановленных с помощью собственной команды или службы переноса данных.

При обнаружении ошибок в процессе проверки целостности команда разработки получает оповещения. Дополнительные сведения о целостности данных в службе "База данных SQL Azure" см. в [этой записи блога](https://azure.microsoft.com/blog/data-integrity-in-azure-sql-database/).

## <a name="how-do-automated-backups-impact-compliance"></a>Влияние автоматически создаваемых резервных копий на соответствие

При переносе базы данных с уровня служб на основе DTU с периодом хранения PITR по умолчанию 35 дней на уровень служб на основе виртуальных ядер, чтобы не нарушить политику восстановления данных приложения период хранения PITR сохраняется. Если период хранения по умолчанию не соответствует вашим требованиям соответствия, вы можете изменить период хранения PITR с помощью PowerShell или REST API. Дополнительные сведения см. в разделе [Как изменить период хранения резервной копии](#how-to-change-the-pitr-backup-retention-period).

[!INCLUDE [GDPR-related guidance](../../includes/gdpr-intro-sentence.md)]

## <a name="how-to-change-the-pitr-backup-retention-period"></a>Как изменить период хранения резервной копии PITR

Вы можете изменить срок хранения резервной копии PITR по умолчанию с помощью портал Azure, PowerShell или REST API. Поддерживаются такие значения: 7, 14, 21, 28 или 35 дней. В следующих примерах показано, как установить период хранения PITR на 28 дней.

> [!WARNING]
> Если сократить текущий срок хранения, все существующие резервные копии старше нового срока хранения больше не будут доступны. Если увеличить текущий период хранения, то база данных SQL будет хранить существующие резервные копии до достижения нового установленного периода хранения.

> [!NOTE]
> Эти API-интерфейсы влияют только на период хранения PITR. Если для базы данных настроено LTR, оно не будет затронуто. Дополнительные сведения о том, как изменить период хранения LTR, см. в статье [Хранение резервных копий базы данных SQL Azure до 10 лет](sql-database-long-term-retention.md).

### <a name="change-pitr-backup-retention-period-using-the-azure-portal"></a>Изменение периода хранения PITR с помощью портала Azure

Чтобы изменить срок хранения резервной копии PITR с помощью портал Azure, перейдите к объекту сервера, срок хранения которого вы хотите изменить на портале, а затем выберите соответствующий параметр в зависимости от изменяемого объекта сервера.

#### <a name="change-pitr-for-a-sql-database-server"></a>Изменение PITR для сервера Базы данных SQL

![Изменение PITR на портале Azure](./media/sql-database-automated-backup/configure-backup-retention-sqldb.png)

#### <a name="change-pitr-for-a-managed-instance"></a>Изменение PITR для управляемого экземпляра

![Изменение PITR на портале Azure](./media/sql-database-automated-backup/configure-backup-retention-sqlmi.png)

### <a name="change-pitr-backup-retention-period-using-powershell"></a>Изменение периода хранения PITR с помощью PowerShell

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]
> [!IMPORTANT]
> Модуль PowerShell Azure Resource Manager по-прежнему поддерживается базой данных SQL Azure, но вся будущая разработка предназначена для модуля AZ. SQL. Эти командлеты см. в разделе [AzureRM. SQL](https://docs.microsoft.com/powershell/module/AzureRM.Sql/). Аргументы для команд в модуле AZ и в модулях AzureRm существенно идентичны.

```powershell
Set-AzSqlDatabaseBackupShortTermRetentionPolicy -ResourceGroupName resourceGroup -ServerName testserver -DatabaseName testDatabase -RetentionDays 28
```

### <a name="change-pitr-retention-period-using-rest-api"></a>Изменение периода хранения PITR с помощью REST API

#### <a name="sample-request"></a>Пример запроса

```http
PUT https://management.azure.com/subscriptions/00000000-1111-2222-3333-444444444444/resourceGroups/resourceGroup/providers/Microsoft.Sql/servers/testserver/databases/testDatabase/backupShortTermRetentionPolicies/default?api-version=2017-10-01-preview
```

#### <a name="request-body"></a>Текст запроса

```json
{
  "properties":{
    "retentionDays":28
  }
}
```

#### <a name="sample-response"></a>Пример ответа

Код состояния: 200

```json
{
  "id": "/subscriptions/00000000-1111-2222-3333-444444444444/providers/Microsoft.Sql/resourceGroups/resourceGroup/servers/testserver/databases/testDatabase/backupShortTermRetentionPolicies/default",
  "name": "default",
  "type": "Microsoft.Sql/resourceGroups/servers/databases/backupShortTermRetentionPolicies",
  "properties": {
    "retentionDays": 28
  }
}
```

Дополнительные сведения о политиках краткосрочного хранения резервных копий см. [здесь](https://docs.microsoft.com/rest/api/sql/backupshorttermretentionpolicies).

## <a name="next-steps"></a>Следующие шаги

- Резервные копии базы данных являются важной частью любой стратегии непрерывности бизнес-процессов и аварийного восстановления, так как они защищают данные от случайного повреждения или удаления. Дополнительные сведения о других решениях для Базы данных SQL Azure, обеспечивающих непрерывность бизнес-процессов, см. в статье [Обзор обеспечения непрерывности бизнес-процессов с помощью Базы данных SQL Azure](sql-database-business-continuity.md).
- Сведения о восстановлении базы данных на определенный момент времени с помощью портала Azure см .в [этой статье](sql-database-recovery-using-backups.md).
- Сведения о восстановлении базы данных на определенный момент времени с помощью PowerShell см .в [этой статье](scripts/sql-database-restore-database-powershell.md).
- Сведения о настройке и управлении долгосрочного хранения создаваемых автоматически резервных копий в хранилище BLOB-объектов Azure, а также о восстановлении таких резервных копий на портале Azure см. в статье [Управление долгосрочным хранением резервных копий базы данных SQL Azure](sql-database-long-term-backup-retention-configure.md).
- Сведения о настройке, управлении и восстановлении долгосрочного хранения автоматически создаваемых резервных копий в хранилище BLOB-объектов Azure с помощью PowerShell см. в статье [Управление долгосрочным хранением резервных копий с помощью PowerShell](sql-database-long-term-backup-retention-configure.md).
