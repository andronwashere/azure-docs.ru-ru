---
title: Экспорт отдельной базы данных или базы данных в составе пула SQL Azure в BACPAC-файл | Документация Майкрософт
description: Экспорт базы данных SQL Azure в BACPAC-файл с помощью портала Azure.
services: sql-database
ms.service: sql-database
ms.subservice: data-movement
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: carlrab
manager: craigg
ms.date: 07/16/2019
ms.openlocfilehash: 8d795fe88721dfed65134b550eb0036c4e7310eb
ms.sourcegitcommit: 770b060438122f090ab90d81e3ff2f023455213b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/17/2019
ms.locfileid: "68305795"
---
# <a name="export-an-azure-sql-database-to-a-bacpac-file"></a>Экспорт базы данных SQL Azure в BACPAC-файл

Если нужно экспортировать базу данных для создания архива или для перехода на другую платформу, можно экспортировать схему и данные базы данных в [BACPAC](https://msdn.microsoft.com/library/ee210546.aspx#Anchor_4)-файл. BACPAC-файл — это ZIP-файл с расширением BACPAC, содержащий метаданные и данные из базы данных SQL Server. BACPAC-файл можно сохранить в хранилище BLOB-объектов Azure или локальном хранилище, а затем импортировать обратно в базу данных SQL Azure или локальный экземпляр SQL Server.

## <a name="considerations-when-exporting-an-azure-sql-database"></a>Рекомендации при экспорте базы данных SQL Azure

- Чтобы экспорт был транзакционно согласованным, необходимо обеспечить отсутствие операций записи во время экспорта или экспортировать данные из [транзакционно согласованной копии](sql-database-copy.md) базы данных SQL Azure.
- Максимальный размер BACPAC-файла при экспорте в хранилище BLOB-объектов составляет 200 ГБ. Для архивации BACPAC-файла большего размера выполняйте экспорт в локальное хранилище.
- Экспорт BACPAC-файла в службу хранилища Azure уровня "Премиум" с использованием методов, описанных в этой статье, не поддерживается.
- Хранилище за брандмауэром сейчас не поддерживается.
- Если операция экспорта из базы данных SQL Azure длится более 20 часов, ее можно отменить. Для повышения производительности во время экспорта можно сделать следующее.

  - Временно повысить объем вычислительных ресурсов.
  - Прекратить все операции чтения и записи во время экспорта.
  - Используйте для всех больших таблиц [кластеризованный индекс](https://msdn.microsoft.com/library/ms190457.aspx) со значениями, отличными от NULL. Без кластеризованных индексов экспорт может завершиться ошибкой, если он длится больше 6–12 часов. Это обусловлено тем, что службам экспорта требуется выполнить проверку таблицы, чтобы экспортировать всю таблицу. Хороший способ определить, оптимизированы ли таблицы для экспорта, — выполнить **DBCC SHOW_STATISTICS** и убедиться, что значение *RANGE_HI_KEY* не равно NULL и имеет хорошее распределение. Дополнительную информацию см. в разделе [DBCC SHOW_STATISTICS](https://msdn.microsoft.com/library/ms174384.aspx).

> [!NOTE]
> BACPAC-файлы не предназначены для операций службы архивации и восстановления. База данных SQL Azure автоматически создает резервные копии для каждой пользовательской базы данных. Дополнительные сведения см. в статьях [Обзор обеспечения непрерывности бизнес-процессов с помощью базы данных SQL Azure](sql-database-business-continuity.md) и [Автоматическое резервное копирование](sql-database-automated-backups.md).

## <a name="export-to-a-bacpac-file-using-the-azure-portal"></a>Экспорт в BACPAC-файл с помощью портала Azure

Экспорт BACPAC базы данных из [управляемого экземпляра](sql-database-managed-instance.md) с помощью Azure PowerShell в настоящее время не поддерживается. Вместо этого используйте SQL Server Management Studio или SQLPackage.

> [!NOTE]
> Компьютеры, обрабатывающие запросы на импорт и экспорт, отправленные с помощью портал Azure или PowerShell, должны хранить BACPAC-файл, а также временные файлы, созданные платформой приложения уровня данных (DacFX). Необходимое дисковое пространство существенно различается в базах данных с одинаковым размером и может потребовать места на диске в 3 раза больше размера базы данных. Компьютеры, на которых работает запрос на импорт или экспорт, имеют только 450GB место на локальном диске. В результате некоторые запросы могут завершиться `There is not enough space on the disk`ошибкой. В этом случае решение заключается в запуске SqlPackage. exe на компьютере с достаточным местом на локальном диске. Мы советуем использовать [SqlPackage](#export-to-a-bacpac-file-using-the-sqlpackage-utility) для импорта и экспорта баз данных, превышающих 150 ГБ, чтобы избежать этой проблемы.

1. Чтобы экспортировать базу данных с помощью [портала Azure](https://portal.azure.com), откройте страницу для базы данных и щелкните **Экспорт** на панели инструментов.

   ![Экспорт базы данных](./media/sql-database-export/database-export1.png)

2. Введите имя BACPAC-файла, выберите учетную запись хранения Azure и контейнер для экспорта, а также соответствующие учетные данные для доступа к базе данных-источнику. Имя для **входа администратора SQL Server** необходимо, даже если вы являетесь администратором Azure, так как администратор Azure не имеет права на SQL Server разрешений администратора.

    ![Экспорт базы данных](./media/sql-database-export/database-export2.png)

3. Нажмите кнопку **ОК**.

4. Чтобы отслеживать ход выполнения операции экспорта, откройте страницу сервера Базы данных SQL, содержащего экспортируемую базу данных. В разделе **Параметры** выберите **Журнал импорта и экспорта**.

   ![журнал экспорта](./media/sql-database-export/export-history.png)

## <a name="export-to-a-bacpac-file-using-the-sqlpackage-utility"></a>Экспорт в BACPAC-файл с помощью служебной программы SQLPackage

Экспорт базы данных SQL с помощью программы командной строки [SqlPackage](https://docs.microsoft.com/sql/tools/sqlpackage) описывается в разделе о [параметрах и свойствах операции экспорта](https://docs.microsoft.com/sql/tools/sqlpackage#export-parameters-and-properties). Служебная программа SqlPackage поставляется вместе с последними версиями [SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) и [SQL Server Data Tools для Visual Studio](https://msdn.microsoft.com/library/mt204009.aspx). Кроме того, вы можете скачать последнюю версию [SqlPackage](https://www.microsoft.com/download/details.aspx?id=53876) непосредственно из Центра загрузки Майкрософт.

Рекомендуется использовать служебную программу SqlPackage для масштабирования и обеспечения производительности в большинстве рабочих сред. Сведения о миграции из SQL Server в Базу данных SQL Azure с использованием BACPAC-файлов см. в [блоге группы консультирования клиентов SQL Server](https://blogs.msdn.microsoft.com/sqlcat/20../../migrating-from-sql-server-to-azure-sql-database-using-bacpac-files/).

В этом примере показано, как экспортировать базу данных с помощью SqlPackage.exe, используя универсальную аутентификацию Active Directory.

```cmd
SqlPackage.exe /a:Export /tf:testExport.bacpac /scs:"Data Source=apptestserver.database.windows.net;Initial Catalog=MyDB;" /ua:True /tid:"apptest.onmicrosoft.com"
```

## <a name="export-to-a-bacpac-file-using-sql-server-management-studio-ssms"></a>Экспорт в BACPAC-файл с помощью SQL Server Management Studio (SSMS)

Самые последние версии SQL Server Management Studio содержат мастер для экспорта базы данных SQL Azure в BACPAC-файл. Ознакомьтесь с разделом [Export a Data-tier Application](https://docs.microsoft.com/sql/relational-databases/data-tier-applications/export-a-data-tier-application) (Экспорт приложения уровня данных).

## <a name="export-to-a-bacpac-file-using-powershell"></a>Экспорт в BACPAC-файл с помощью PowerShell

> [!NOTE]
> Сейчас [управляемый экземпляр](sql-database-managed-instance.md) не поддерживает миграцию базы данных в базу данных экземпляра из BACPAC-файла с помощью Azure PowerShell. Чтобы экспортировать управляемый экземпляр в BACPAC-файл, используйте SQL Server Management Studio или SQLPackage.

Используйте командлет [New-азсклдатабасикспорт](/powershell/module/az.sql/new-azsqldatabaseexport) , чтобы отправить запрос на экспорт базы данных в службу базы данных SQL Azure. Операция экспорта может занять некоторое время в зависимости от размера базы данных.

```powershell
$exportRequest = New-AzSqlDatabaseExport -ResourceGroupName $ResourceGroupName -ServerName $ServerName `
  -DatabaseName $DatabaseName -StorageKeytype $StorageKeytype -StorageKey $StorageKey -StorageUri $BacpacUri `
  -AdministratorLogin $creds.UserName -AdministratorLoginPassword $creds.Password
```

Чтобы проверить состояние запроса на экспорт, используйте командлет [Get-азсклдатабасеимпортекспортстатус](/powershell/module/az.sql/get-azsqldatabaseimportexportstatus) . Если выполнить его немедленно после запроса, то обычно возвращается сообщение **Состояние: выполняется**. Когда появится сообщение **Состояние: выполнено**, экспорт завершен.

```powershell
$exportStatus = Get-AzSqlDatabaseImportExportStatus -OperationStatusLink $exportRequest.OperationStatusLink
[Console]::Write("Exporting")
while ($exportStatus.Status -eq "InProgress")
{
    Start-Sleep -s 10
    $exportStatus = Get-AzSqlDatabaseImportExportStatus -OperationStatusLink $exportRequest.OperationStatusLink
    [Console]::Write(".")
}
[Console]::WriteLine("")
$exportStatus
```

## <a name="next-steps"></a>Следующие шаги

- Дополнительные сведения о долгосрочном хранении резервных копий отдельной базы данных или базы данных в пуле как альтернативе экспорту базы данных для создания архива приведены в статье [Хранение резервных копий базы данных SQL Azure до 10 лет](sql-database-long-term-retention.md). Задания агентов SQL можно использовать для планирования [резервных копий только для копирования базы данных](https://docs.microsoft.com/sql/relational-databases/backup-restore/copy-only-backups-sql-server) как альтернативу долгосрочному хранению архивных копий.
- Сведения о миграции из SQL Server в Базу данных SQL Azure с использованием BACPAC-файлов см. в [блоге группы консультирования клиентов SQL Server](https://blogs.msdn.microsoft.com/sqlcat/2016/10/20/migrating-from-sql-server-to-azure-sql-database-using-bacpac-files/).
- Сведения об импорте BACPAC-файла в базу данных SQL Server см. в статье [Импорт файла BACPAC для создания новой пользовательской базы данных](https://msdn.microsoft.com/library/hh710052.aspx).
- Сведения об экспорте BACPAC-файла из базы данных SQL Server см. в разделе [Export a Data-tier Application](https://docs.microsoft.com/sql/relational-databases/data-tier-applications/export-a-data-tier-application) (Экспорт приложения уровня данных).
- Дополнительные сведения об использовании службы миграции данных для переноса базы данных см. в разделе [Перенос баз данных из SQL Server в Базу данных SQL Azure в автономном режиме с помощью DMS](../dms/tutorial-sql-server-to-azure-sql.md).
- Если вы экспортируете данные из SQL Server, чтобы потом перенести их в Базу данных SQL Azure, см. в статью [Migrate a SQL Server database to Azure SQL Database](sql-database-single-database-migrate.md) (Миграция базы данных SQL Server в базу данных SQL).
- Чтобы узнать, как безопасно управлять ключами к хранилищу данных и подписанными URL-адресами и совместно их использовать, ознакомьтесь с [руководством по безопасности службы хранилища Azure](https://docs.microsoft.com/azure/storage/common/storage-security-guide).
