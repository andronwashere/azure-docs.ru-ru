---
title: Перенос базы данных MariaDB в базу данных Azure для MariaDB с помощью дампа и восстановления
description: В этой статье описываются два распространенных способа архивации и восстановления баз данных в базе данных Azure для MariaDB с помощью таких средств, как mysqldump, MySQL Workbench и PHPMyAdmin.
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.topic: conceptual
ms.date: 09/24/2018
ms.openlocfilehash: bcb76fcbba02bf53b48cc462e3dad8f264db02ed
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "60745966"
---
# <a name="migrate-your-mariadb-database-to-azure-database-for-mariadb-using-dump-and-restore"></a>Перенос базы данных MariaDB в базу данных Azure для MariaDB с помощью резервного копирования и восстановления
В этой статье описываются два распространенных способа архивации и восстановления баз данных в базе данных Azure для MariaDB.
- дамп и восстановление из командной строки (с помощью mysqldump); 
- дамп и восстановление с помощью PHPMyAdmin.

## <a name="before-you-begin"></a>Перед началом работы
Прежде чем приступить к выполнению этого руководства, необходимо выполнить следующее:
- [Создание базы данных Azure для сервера MariaDB с помощью портала Azure](quickstart-create-mariadb-server-database-using-azure-portal.md)
- установить на компьютере служебную программу командной строки [mysqldump](https://mariadb.com/kb/en/library/mysqldump/);
- установить MySQL Workbench [ссылка на скачивание MySQL Workbench](https://dev.mysql.com/downloads/workbench/), Toad, Navicat или любой сторонний инструмент MySQL для выполнения команд дампа и восстановления.

## <a name="use-common-tools"></a>Использование распространенных инструментов
Используйте распространенные инструменты и служебные программы, такие как MySQL Workbench, mysqldump, Toad или Navicat, для удаленного подключения и восстановления данных в базу данных Azure для MariaDB. Используйте эти инструменты на своем клиентском компьютере, подключенном к Интернету, чтобы подключиться к базе данных Azure для MariaDB. Для обеспечения безопасности используйте подключение с SSL-шифрованием, а также см. статью [SSL-соединения в базе данных Azure для MariaDB](concepts-ssl-connection-security.md). При переносе в базу данных Azure для MariaDB не нужно перемещать файлы дампа в особое облачное расположение. 

## <a name="common-uses-for-dump-and-restore"></a>Распространенные варианты использования дампа и восстановления
Вы можете использовать такие служебные программы MySQL, как mysqldump и mysqlpump, для дампа и загрузки баз данных в базу данных Azure для сервера MariaDB в нескольких распространенных сценариях. 

<!--In other scenarios, you may use the [Import and Export](howto-migrate-import-export.md) approach instead.-->

- Используйте дампы базы данных при перемещении всей базы данных. Мы рекомендуем использовать этот способ при перемещении большого количества данных MariaDB, или если вы не хотите прерывать работу службы для динамичных сайтов или приложений. 
-  Все таблицы в базе данных должны использовать подсистему хранилища InnoDB при загрузке данных в базу данных Azure для MariaDB. База данных Azure для MariaDB поддерживает только подсистему хранилища InnoDB и не поддерживает другие подсистемы хранилища. Если таблицы настроены с помощью других подсистем хранилища, преобразуйте их в формат ядра InnoDB перед перемещением в базу данных Azure для MariaDB.
   Например, если у вас есть WordPress или WebApp, использующее таблицы MyISAM, то перед восстановлением в базе данных Azure для MariaDB вам сначала нужно преобразовать эти таблицы в формат InnoDB путем перемещения. Используйте предложение `ENGINE=InnoDB`, чтобы задать ядро, используемое при создании таблицы, а затем передайте данные в совместимую таблицу перед восстановлением. 

   ```sql
   INSERT INTO innodb_table SELECT * FROM myisam_table ORDER BY primary_key_columns
   ```
- Чтобы избежать проблем с совместимостью, должна использоваться одна версия MariaDB в системах источника и назначения при дампе баз данных. Например, если вы используете существующий сервер MariaDB версии 10.2, вы должны выполнить перемещение в базу данных Azure для MariaDB, настроенную на запуск версии 10.2. Команда `mysql_upgrade` не будет работать в базе данных Azure для MariaDB и является неподдерживаемой. Если вам необходимо обновить все версии MariaDB, используйте сначала дамп или экспорт базы данных ранней версии, чтобы получить наиболее актуальную версию MariaDB в собственной среде. Затем перед выполнением перемещения в базу данных Azure для MariaDB запустите `mysql_upgrade`.

## <a name="performance-considerations"></a>Рекомендации по производительности
Для оптимизации производительности при дампе больших баз данных мы рекомендуем ознакомиться с рекомендациями ниже.
-   Используйте параметр `exclude-triggers` в mysqldump при выполнении дампа баз данных. Исключите триггеры из файлов дампа, чтобы избежать сбоев запуска команд триггера во время восстановления данных. 
-   Используйте параметр `single-transaction`, чтобы задать режим изоляции транзакций REPEATABLE READ и отправлять на сервер инструкцию SQL START TRANSACTION перед созданием дампа данных. Формирование дампа нескольких таблиц в одной транзакции приведет к использованию дополнительных ресурсов хранилища во время восстановления. Параметры `single-transaction` и `lock-tables` являются взаимоисключающими, так как LOCK TABLES приводит к неявной фиксации всех ожидающих транзакций. Для формирования дампа больших таблиц используйте параметр `single-transaction` с параметром `quick`. 
-   Используйте многострочный синтаксис `extended-insert` с несколькими списками VALUE. Это позволяет уменьшить размер файла дампа и ускорить операции вставки при перезагрузке файла.
-  Используйте параметр `order-by-primary` в mysqldump при выполнении дампа баз данных, чтобы данные были добавлены в сценарий в порядке первичных ключей.
-   Используйте параметр `disable-keys` в mysqldump при выполнении дампа данных, чтобы отключить ограничения для внешнего ключа перед загрузкой. Отключение проверок внешнего ключа обеспечивает значительный прирост производительности. Включите ограничения и проверьте данные после загрузки, чтобы обеспечить целостность данных.
-   Используйте секционированные таблицы, когда это необходимо.
-   Загружайте данные в параллельном режиме. Не выполняйте слишком много параллельных операций, так как можно достигнуть лимита ресурсов. Отслеживайте ресурсы с помощью метрик, доступных на портале Azure. 
-   Используйте параметр `defer-table-indexes` в mysqlpump при выполнении дампа баз данных для создания индекса после загрузки данных таблиц.
-   Скопируйте файлы резервной копии в большой двоичный объект Azure или хранилище Azure и выполните восстановление из них, что должно быть намного быстрее, чем восстановление через Интернет.

## <a name="create-a-backup-file"></a>Создание резервного файла
Чтобы создать резервную копию существующей базы данных MariaDB на локальном сервере или виртуальной машине, выполните команду mysqldump: 
```bash
$ mysqldump --opt -u [uname] -p[pass] [dbname] > [backupfile.sql]
```

Необходимо указать следующие параметры:
- [uname] — имя пользователя базы данных; 
- [pass] — пароль для базы данных (обратите внимание, что между "-p" и паролем нет пробела); 
- [dbname] — имя базы данных; 
- [backupfile.sql] — имя файла резервной копии базы данных; 
- [--opt] — параметр mysqldump. 

Например, чтобы создать резервную копию базы данных с именем testdb на сервере MariaDB с именем пользователя testuser и без пароля и сохранить ее в файл testdb_backup.sql, используйте приведенную ниже команду. Команда создает резервную копию базы данных `testdb` в файле с именем `testdb_backup.sql`, который содержит все инструкции SQL, необходимые для повторного создания базы данных. 

```bash
$ mysqldump -u root -p testdb > testdb_backup.sql
```
Чтобы выбрать для создания резервной копии конкретные таблицы в базе данных, укажите имена этих таблиц в виде списка, разделенного пробелами. Например, чтобы создать резервную копию только для таблиц table1 и table2 из базы данных testdb, используйте этот пример: 
```bash
$ mysqldump -u root -p testdb table1 table2 > testdb_tables_backup.sql
```
Чтобы создать резервную копию сразу нескольких баз данных, используйте параметр --databases и укажите имена этих баз данных в виде списка, разделенного пробелами. 
```bash
$ mysqldump -u root -p --databases testdb1 testdb3 testdb5 > testdb135_backup.sql 
```
Чтобы одновременно создать резервную копию всех баз данных на сервере, используйте параметр --all-databases.
```bash
$ mysqldump -u root -p --all-databases > alldb_backup.sql 
```

## <a name="create-a-database-on-the-target-server"></a>Создание базы данных на целевом сервере
Создайте пустую базу данных в целевой базе данных Azure для сервера MariaDB, куда необходимо перенести данные. Для этого используйте средство MySQL Workbench, Toad или Navicat. База данных может иметь то же имя, что и база данных, содержащая данные дампа. Вы также можете создать базу данных с другим именем.

Чтобы подключиться, найдите сведения о подключении на странице **Обзор** базы данных Azure для MariaDB.

![Поиск сведений о подключении на портале Azure](./media/howto-migrate-dump-restore/1_server-overview-name-login.png)

Добавьте сведения о подключении в MySQL Workbench.

![Строка подключения MySQL Workbench](./media/howto-migrate-dump-restore/2_setup-new-connection.png)

## <a name="restore-your-mariadb-database"></a>Восстановление базы данных MariaDB
После создания целевой базы данных можно воспользоваться командой mysql или клиентом MySQL Workbench, чтобы восстановить данные в определенную вновь созданную базу данных из файла дампа.
```bash
mysql -h [hostname] -u [uname] -p[pass] [db_to_restore] < [backupfile.sql]
```
В этом примере восстановите данные в созданную базу данных в целевой базе данных Azure для сервера MariaDB.
```bash
$ mysql -h mydemoserver.mariadb.database.azure.com -u myadmin@mydemoserver -p testdb < testdb_backup.sql
```

## <a name="export-using-phpmyadmin"></a>Экспорт с помощью PHPMyAdmin
Для экспорта можно использовать распространенный инструмент phpMyAdmin, который уже может быть установлен в вашей локальной среде. Чтобы экспортировать базу данных MariaDB с помощью PHPMyAdmin, выполните следующие действия:
1. Откройте phpMyAdmin.
2. Выберите свою базу данных. Щелкните имя базы данных в списке слева. 
3. Щелкните ссылку **Экспорт**. Появится страница для просмотра дампа базы данных.
4. В области экспорта щелкните ссылку **Выбрать все**, чтобы выбрать все таблицы в базе данных. 
5. В области параметров SQL щелкните необходимые параметры. 
6. Щелкните параметр **Сохранить как файл**, выберите соответствующий вариант сжатия, а затем нажмите кнопку **Перейти**. Появится диалоговое окно, предлагающее сохранить файл локально.

## <a name="import-using-phpmyadmin"></a>Импорт с помощью PHPMyAdmin
Импорт базы данных выполняется подобно экспорту. Выполните следующие действия:
1. Откройте phpMyAdmin. 
2. На странице настройки phpMyAdmin щелкните **Добавить**, чтобы добавить базу данных Azure для сервера MariaDB. Укажите сведения о подключении и учетные данные.
3. Создайте базу данных, присвоив ей соответствующее имя. Затем выберите эту базу данных, щелкнув ее имя в списке в левой части экрана. Чтобы перезаписать существующую базу данных, щелкните имя базы данных, установите все флажки рядом с именами таблиц, а затем выберите **Удалить**, чтобы удалить существующие таблицы. 
4. Щелкните ссылку **SQL**, чтобы отобразилась страница, на которой можно ввести команды SQL или передать файл SQL. 
5. Нажмите кнопку **обзора**, чтобы найти файл базы данных. 
6. Нажмите кнопку **Перейти**, чтобы экспортировать резервную копию, выполнить команды SQL и повторно создать базу данных.

## <a name="next-steps"></a>Дальнейшие действия
- [Подключение приложений к базе данных Azure для MariaDB](./howto-connection-string.md).
 
<!--
- For more information about migrating databases to Azure Database for MariaDB, see the [Database Migration Guide](https://aka.ms/datamigration).
-->
