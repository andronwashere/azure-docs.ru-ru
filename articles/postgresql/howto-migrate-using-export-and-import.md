---
title: 'Перенос базы данных с помощью импорта и экспорта в базе данных Azure для PostgreSQL: один сервер'
description: Описывается, как извлечь базу данных PostgreSQL в файл сценария и импортировать данные из этого файла в целевую базу данных.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 5/6/2019
ms.openlocfilehash: 785e9ec77dea749546e3f1d59007706eac14f2ea
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "65067025"
---
# <a name="migrate-your-postgresql-database-using-export-and-import"></a>Перенос базы данных PostgreSQL с помощью экспорта и импорта
Можно извлечь базу данных PostgreSQL в файл сценария с помощью [pg_dump](https://www.postgresql.org/docs/current/static/app-pgdump.html) и импортировать данные из этого файла в целевую базу данных с помощью [psql](https://www.postgresql.org/docs/current/static/app-psql.html).

## <a name="prerequisites"></a>Технические условия
Прежде чем приступить к выполнению этого руководства, необходимы следующие компоненты:
- [сервер базы данных Azure для PostgreSQL](quickstart-create-server-database-portal.md) с правилами брандмауэра, разрешающими доступ к этом серверу и его базам данных;
- установленная программа командной строки [pg_dump](https://www.postgresql.org/docs/9.6/static/app-pgdump.html);
- установленная программа командной строки [psql](https://www.postgresql.org/docs/9.6/static/app-psql.html).

Выполните приведенные ниже действия, чтобы экспортировать и импортировать базу данных PostgreSQL.

## <a name="create-a-script-file-using-pgdump-that-contains-the-data-to-be-loaded"></a>Создание файла сценария, содержащего загружаемые данные, с помощью pg_dump
Чтобы экспортировать имеющуюся базу данных PostgreSQL в локальную среду или на виртуальную машину в виде файла сценария SQL, выполните следующую команду:
```bash
pg_dump –-host=<host> --username=<name> --dbname=<database name> --file=<database>.sql
```
Например, если имеется локальный сервер с базой данных **testdb**.
```bash
pg_dump --host=localhost --username=masterlogin --dbname=testdb --file=testdb.sql
```

## <a name="import-the-data-on-target-azure-database-for-postgresql"></a>Импорт данных в целевую базу данных Azure для PostrgeSQL
Вы можете использовать командную строку psql с параметром --dbname (-d), чтобы импортировать данные в базу данных Azure для сервера PostrgeSQL и загрузить данные из SQL-файла.
```bash
psql --file=<database>.sql --host=<server name> --port=5432 --username=<user@servername> --dbname=<target database name>
```
В этом примере используется программа psql и файл сценария **testdb.sql** из предыдущего шага, чтобы импортировать данные в базу данных **mypgsqldb** на целевом сервере **mydemoserver.postgres.database.azure.com**.
```bash
psql --file=testdb.sql --host=mydemoserver.database.windows.net --port=5432 --username=mylogin@mydemoserver --dbname=mypgsqldb
```

## <a name="next-steps"></a>Дальнейшие действия
- Сведения о переносе базы данных PostgreSQL с помощью дампа и ее восстановлении см. в [этой статье](howto-migrate-using-dump-and-restore.md).
- Дополнительные сведения о переносе баз данных в службу "База данных Azure для PostgreSQL" см. в [этой статье](https://aka.ms/datamigration). 
