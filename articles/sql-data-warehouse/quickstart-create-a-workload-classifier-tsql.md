---
title: Краткое руководство. Создание рабочей нагрузки классификатора. T-SQL | Документация Майкрософт
description: Использование T-SQL для создания классификатора рабочей нагрузки с высокой важностью.
services: sql-data-warehouse
author: ronortloff
manager: craigg
ms.service: sql-data-warehouse
ms.topic: quickstart
ms.subservice: workload-management
ms.date: 05/01/2019
ms.author: rortloff
ms.reviewer: jrasnick
ms.openlocfilehash: f400989fdbdede4f4a07ee13c5a606d51529150c
ms.sourcegitcommit: ccb9a7b7da48473362266f20950af190ae88c09b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/05/2019
ms.locfileid: "67588701"
---
# <a name="quickstart-create-a-workload-classifier-using-t-sql"></a>Краткое руководство. Создание рабочей нагрузки классификатора с помощью T-SQL

Благодаря этому краткому руководству вы быстро создадите классификатор рабочей нагрузки с высоким уровнем важности для руководителей своей организации. Этот классификатор рабочей нагрузки позволит запросам руководителей иметь приоритет над другими запросами с меньшей важностью в очереди.

Если у вас еще нет подписки Azure, создайте [бесплатную](https://azure.microsoft.com/free/) учетную запись Azure, прежде чем начинать работу.

> [!NOTE]
> Создание хранилища данных SQL может привести к дополнительным расходам.  Дополнительные сведения см. на странице [цен на хранилище данных SQL](https://azure.microsoft.com/pricing/details/sql-data-warehouse/).
>
>

## <a name="prerequisites"></a>Предварительные требования

В этом кратком руководстве предполагается, что у вас уже есть хранилище данных SQL и права доступа к системе управления базой данных. Если его требуется создать, используйте инструкции из раздела [Создание хранилища данных SQL Azure на портале Azure и отправка запросов к этому хранилищу данных](create-data-warehouse-portal.md), чтобы создать хранилище данных **mySampleDataWarehouse**.

## <a name="sign-in-to-the-azure-portal"></a>Вход на портал Azure

Войдите на [портале Azure](https://portal.azure.com/).

## <a name="create-login-for-theceo"></a>Создание имени для входа TheCEO

Создайте имя для входа с проверкой подлинности SQL Server в базу данных `master`, используя функцию [CREATE LOGIN](/sql/t-sql/statements/create-login-transact-sql) для TheCEO.

```sql
IF NOT EXISTS (SELECT * FROM sys.sql_logins WHERE name = 'TheCEO')
BEGIN
CREATE LOGIN [TheCEO] WITH PASSWORD='<strongpassword>'
END
;
```

## <a name="create-user"></a>Создать пользователя

[Создайте пользователя](/sql/t-sql/statements/create-user-transact-sql?view=azure-sqldw-latest) TheCEO в mySampleDataWarehouse.

```sql
IF NOT EXISTS (SELECT * FROM sys.database_principals WHERE name = 'THECEO')
BEGIN
CREATE USER [TheCEO] FOR LOGIN [TheCEO]
END
;
```

## <a name="create-a-workload-classifier"></a>Создание классификатора рабочих нагрузок

Для пользователя TheCEO создайте [классификатор рабочей нагрузки](/sql/t-sql/statements/create-workload-classifier-transact-sql?view=azure-sqldw-latest), задав его параметру относительной важности запроса IMPORTANCE значение HIGH.

```sql
DROP WORKLOAD CLASSIFIER [wgcTheCEO];
CREATE WORKLOAD CLASSIFIER [wgcTheCEO]
WITH (WORKLOAD_GROUP = 'xlargerc'
      ,MEMBERNAME = 'TheCEO'
      ,IMPORTANCE = HIGH);
```

## <a name="view-existing-classifiers"></a>Просмотр существующих классификаторов

```sql
SELECT * FROM sys.workload_management_workload_classifiers
```

## <a name="clean-up-resources"></a>Очистка ресурсов

```sql
DROP WORKLOAD CLASSIFIER [wgcTheCEO]
DROP USER [TheCEO]
;
```

Плата взимается за единицы хранилища данных и данные, которые содержатся в нем. Плата за вычислительные ресурсы и ресурсы хранилища взимается отдельно.

- Если вы хотите сохранить данные в хранилище, то можете приостановить работу вычислительных ресурсов, когда не используете хранилище данных. При приостановке вычислений плата взимается только за хранение данных. Когда вы будете готовы работать с данными, возобновите вычисление.
- Если вы хотите исключить будущие расходы, то можете удалить хранилище данных.

Выполните следующие действия, чтобы очистить ресурсы.

1. Войдите на [портал Azure](https://portal.azure.com) и выберите хранилище данных.

    ![Очистка ресурсов](media/load-data-from-azure-blob-storage-using-polybase/clean-up-resources.png)

2. Чтобы приостановить вычисление, нажмите кнопку **Пауза**. Если работа хранилища данных приостановлена, вы увидите кнопку **Запуск**.  Чтобы возобновить вычисление, нажмите кнопку **Пуск**.

3. Чтобы удалить хранилище данных во избежание дальнейших платежей за вычисления или хранение, нажмите кнопку **Удалить**.

4. Чтобы удалить созданный вами сервер SQL, выберите сервер **mynewserver-20180430.database.windows.net**, выделенный на предыдущем изображении, а затем нажмите **Удалить**.  Будьте внимательны, так как удаление сервера приведет к удалению всех баз данных, назначенных этому серверу.

5. Чтобы удалить группу ресурсов, выберите **myResourceGroup**, а затем **Удалить группу ресурсов**.

## <a name="next-steps"></a>Дополнительная информация

- Теперь вы создали классификатор рабочей нагрузки. Выполните несколько запросов как TheCEO, чтобы проверить, как они работают. Запросы и назначенную важность см. в разделе [sys.dm_pdw_exec_requests](/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-exec-requests-transact-sql).
- Дополнительные сведения об управлении рабочей нагрузкой Хранилища данных SQL Azure см. в статьях [Azure SQL Data Warehouse workload importance](sql-data-warehouse-workload-importance.md) (Важность рабочих нагрузок Хранилища данных SQL Azure) и [Azure SQL Data Warehouse workload classification](sql-data-warehouse-workload-classification.md) (Классификация рабочих нагрузок Хранилища данных SQL Azure).
- См. статьи с инструкциями по [настройке уровня важности рабочих нагрузок](sql-data-warehouse-how-to-configure-workload-importance.md), а также по [администрированию и мониторингу рабочих нагрузок](sql-data-warehouse-how-to-manage-and-monitor-workload-importance.md).
