---
title: Прозрачное шифрование данных в хранилище данных SQL (на портале) | Документация Майкрософт
description: Прозрачное шифрование данных в хранилище данных SQL на портале
services: sql-data-warehouse
author: KavithaJonnakuti
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: security
ms.date: 04/30/2019
ms.author: kavithaj
ms.reviewer: rortloff
ms.openlocfilehash: e756049110f7d4a81950abf6ebbe73edb3e3ca0a
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "65143164"
---
# <a name="get-started-with-transparent-data-encryption-tde-in-sql-data-warehouse"></a>Начало работы с прозрачным шифрованием данных (TDE) в хранилище данных SQL
> [!div class="op_single_selector"]
> * [Обзор безопасности](sql-data-warehouse-overview-manage-security.md)
> * [Аутентификация](sql-data-warehouse-authentication.md)
> * [Шифрование (портал)](sql-data-warehouse-encryption-tde.md)
> * [Шифрование (T-SQL)](sql-data-warehouse-encryption-tde-tsql.md)
> 
> 

## <a name="required-permissions"></a>Необходимые разрешения
Чтобы включить прозрачное шифрование данных, необходимо иметь права администратора или участника роли dbmanager.

## <a name="enabling-encryption"></a>Включение шифрования
Чтобы включить прозрачное шифрование данных для хранилища данных SQL, выполните указанные ниже действия.

1. Откройте базу данных на [портале Azure](https://portal.azure.com)
2. В колонке базы данных нажмите кнопку **Параметры** .
3. Выберите параметр **Прозрачное шифрование данных** . ![][1]
4. Выберите параметр **Вкл.** ![][2]
5. Щелкните **Сохранить**
   ![][3].  

## <a name="disabling-encryption"></a>Отключение шифрования
Чтобы отключить прозрачное шифрование данных для хранилища данных SQL, выполните указанные ниже действия.

1. Откройте базу данных на [портале Azure](https://portal.azure.com)
2. В колонке базы данных нажмите кнопку **Параметры** .
3. Выберите параметр **Прозрачное шифрование данных** . ![][1]
4. Выберите параметр **Выкл.** ![][4]
5. Щелкните **Сохранить**
   ![][5].  

## <a name="encryption-dmvs"></a>Динамические административные представления шифрования
Шифрование может быть подтверждено с помощью следующих динамических административных представлений.

* [sys.databases]
* [sys.dm_pdw_nodes_database_encryption_keys]

<!--MSDN references-->
[Transparent Data Encryption (TDE)]: https://msdn.microsoft.com/library/bb934049.aspx
[sys.databases]: https://msdn.microsoft.com/library/ms178534.aspx
[sys.dm_pdw_nodes_database_encryption_keys]: https://msdn.microsoft.com/library/mt203922.aspx

<!--Image references-->
[1]: ./media/sql-data-warehouse-security-tde/sql-data-warehouse-security-tde-portal-settings.png
[2]: ./media/sql-data-warehouse-security-tde/sql-data-warehouse-security-tde-portal-settings-on.png
[3]: ./media/sql-data-warehouse-security-tde/sql-data-warehouse-security-tde-portal-settings-save.png
[4]: ./media/sql-data-warehouse-security-tde/sql-data-warehouse-security-tde-portal-settings-off.png
[5]: ./media/sql-data-warehouse-security-tde/sql-data-warehouse-security-tde-portal-settings-save2.png

<!--Link references-->
