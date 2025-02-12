---
title: Предоставление доступа к службам "База данных SQL Azure" и "Хранилище данных SQL" | Документация Майкрософт
description: Предоставление доступа к службам "База данных SQL Azure" и "Хранилище данных SQL".
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: sql-data-warehouse
ms.devlang: ''
ms.topic: conceptual
author: VanMSFT
ms.author: vanto
ms.reviewer: carlrab
manager: craigg
ms.date: 05/08/2019
ms.openlocfilehash: 783a8f0bc25717f1c2bf78a9c0d40b209a07939b
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "65473346"
---
# <a name="azure-sql-database-and-sql-data-warehouse-access-control"></a>Управление доступом к службам "База данных SQL Azure" и "Хранилище данных SQL"

Чтобы обеспечить безопасность, службы [База данных SQL Azure](sql-database-technical-overview.md) и [Хранилище данных SQL](../sql-data-warehouse/sql-data-warehouse-overview-what-is.md) управляют доступом с помощью правил брандмауэра, ограничивающих подключение по IP-адресу, механизмов проверки подлинности, требующих удостоверений пользователей, и методов авторизации, ограничивающих действия и данные для пользователей. 

> [!IMPORTANT]
> Обзор функций защиты в базе данных SQL см. в [этой статье](sql-database-security-overview.md). Ознакомьтесь с руководством [Защита базы данных SQL Azure](sql-database-security-tutorial.md). Обзор всех функций защиты в службе "Хранилище данных SQL" см. в [этой статье](../sql-data-warehouse/sql-data-warehouse-overview-manage-security.md).

## <a name="firewall-and-firewall-rules"></a>Брандмауэр и правила брандмауэра

База данных SQL Microsoft Azure предоставляет службу реляционных баз данных для Azure и других интернет-приложений. Чтобы защитить ваши данные, брандмауэр запрещает любой доступ к серверу базы данных, пока вы не укажете компьютеры, у которых есть разрешение на доступ. Брандмауэр предоставляет доступ к базам данным на основе исходного IP-адреса каждого запроса. Дополнительные сведения см. в статье [Обзор правил брандмауэра базы данных SQL Azure](sql-database-firewall-configure.md).

Служба "База данных SQL Azure" доступна только на TCP-порте 1433. Для получения доступа к базе данных Azure со своего компьютера убедитесь, что брандмауэр клиентского компьютера пропускает исходящие подключения по протоколу TCP через порт 1433. Заблокируйте входящие подключения по протоколу TCP через порт 1433, если они не требуются для других приложений. 

В рамках процесса подключения все подключения от виртуальных машин Azure будут перенаправлены на другой IP-адрес и порт, уникальный для каждой рабочей роли. Номер порта находится в диапазоне от 11000 до 11999. Дополнительные сведения о TCP-портах см. в статье [Порты для ADO.NET 4.5, отличные от порта 1433](sql-database-develop-direct-route-ports-adonet-v12.md).

## <a name="authentication"></a>Authentication

База данных SQL поддерживает два типа аутентификации:

- **Аутентификация SQL**.

  Это метод аутентификации с использованием имени пользователя и пароля. При создании сервера Базы данных SQL для базы данных вы указали имя для входа "Администратор сервера" и пароль. Используя эти учетные данные, можно выполнить проверку подлинности для любой базы данных на этом сервере в качестве владельца базы данных (или "dbo"). 
- **Аутентификация Azure Active Directory**.

  Это метод аутентификации с использованием удостоверений, контролируемых Azure Active Directory, которая поддерживается управляемыми и интегрированными доменами. Если вы хотите использовать аутентификацию Azure Active Directory, необходимо создать другого администратора сервера с именем Azure AD admin, которому разрешено администрировать пользователей и группы Azure AD. Этот администратор также может выполнять все операции, доступные обычному администратору. В разделе [Подключение к базе данных SQL с использованием проверки подлинности Azure Active Directory](sql-database-aad-authentication.md) содержится пошаговое руководство по созданию администратора Azure AD для включения аутентификации Azure Active Directory.

Ядро СУБД закрывает подключения, которые оставались неактивными более 30 минут. Перед использованием подключения необходимо выполнить вход повторно. Для постоянных активных подключений к базе данных SQL требуется повторная авторизация (выполняется ядром СУБД) по крайней мере каждые 10 часов. Ядро СУБД пытается пройти повторную авторизацию с помощью изначально предоставленного пароля и не требует выполнения каких-либо действий со стороны пользователя. Для повышения производительности после сброса пароля в Базе данных SQL повторная аутентификация подключения не выполняется, даже если это подключение сброшено из-за пула подключений.Для повышения производительности после сброса пароля в Базе данных SQL повторная аутентификация подключения не выполняется, даже если это подключение сброшено из-за пула подключений. В этом состоит отличие от поведения локального экземпляра SQL Server. Если со времени первой авторизации подключения пароль был изменен, произойдет разрыв подключения, а новое подключение будет установлено с использованием нового пароля. Пользователь с разрешением `KILL DATABASE CONNECTION` может явно разорвать подключение к базе данных SQL с помощью команды [KILL](https://docs.microsoft.com/sql/t-sql/language-elements/kill-transact-sql).

Учетные записи пользователей можно создать в базе данных master (и предоставить им разрешения во всех базах данных на сервере) или в самой базе данных (так называемые автономные пользователи). Сведения о создании имен для входа и управлении ими см. в [этой статье](sql-database-manage-logins.md). Для улучшения переносимости и масштабируемости используйте автономные базы данных. Дополнительные сведения об автономных пользователях см. в статьях [Пользователи автономной базы данных — создание переносимой базы данных](https://docs.microsoft.com/sql/relational-databases/security/contained-database-users-making-your-database-portable), [CREATE USER (Transact-SQL)](https://docs.microsoft.com/sql/t-sql/statements/create-user-transact-sql) и [Автономные базы данных](https://docs.microsoft.com/sql/relational-databases/databases/contained-databases).

Но рекомендуется, чтобы приложение использовало отдельную учетную запись для проверки подлинности. Это позволит ограничить разрешения, предоставляемые приложению, и снизить риски вредоносных действий в случае, если оно уязвимо для атак путем внедрения кода SQL. В этом случае мы рекомендуем создать [пользователя автономной базы данных](https://docs.microsoft.com/sql/relational-databases/security/contained-database-users-making-your-database-portable), что позволяет приложению выполнять проверку подлинности непосредственно в базе данных. 

## <a name="authorization"></a>Авторизация

Авторизация подразумевает набор действий, которые пользователь может выполнять в базе данных SQL Azure. Такой набор действий определяется принадлежностью к [ролям](https://docs.microsoft.com/sql/relational-databases/security/authentication-access/database-level-roles) базы данных учетной записи пользователя и [разрешениями на уровне объектов](https://docs.microsoft.com/sql/relational-databases/security/permissions-database-engine). Обычно пользователям рекомендуется предоставлять наименьшие необходимые привилегии. Учетной записи администратора сервера, которая используется для подключения, присвоена роль db_owner, обладатель которой может выполнять любые действия в базе данных. Сохраните эту учетную запись для развертывания обновлений схемы и выполнения других действий по управлению. Для подключения к базе данных из приложения с использованием наименьших привилегий, необходимых приложению, используйте учетную запись "ApplicationUser" с более ограниченными разрешениями. Дополнительные сведения см. в статье [Проверка подлинности и авторизация в базе данных SQL: предоставление доступа](sql-database-manage-logins.md).

Как правило, доступ к базе данных `master` нужен только администраторам. Постоянный доступ к каждой пользовательской базе данных должен обеспечиваться через пользователей автономной базы данных, созданных в каждой базе данных и не являющихся администраторами. В этом случае создавать имена входа в базе данных `master` не требуется. Дополнительные сведения см. в статье [Пользователи автономной базы данных — создание переносимой базы данных](https://docs.microsoft.com/sql/relational-databases/security/contained-database-users-making-your-database-portable).

Ознакомьтесь со следующими возможностями, которые можно использовать, чтобы ограничить или расширить права:

- Чтобы безопасно временно повысить уровень разрешений, можно использовать [олицетворение](https://docs.microsoft.com/dotnet/framework/data/adonet/sql/customizing-permissions-with-impersonation-in-sql-server) и [подписание модулей](https://docs.microsoft.com/dotnet/framework/data/adonet/sql/signing-stored-procedures-in-sql-server).
- [Безопасности на уровне строк](https://docs.microsoft.com/sql/relational-databases/security/row-level-security) может использоваться, чтобы ограничить доступ пользователя к строкам.
- [Маскирование данных](sql-database-dynamic-data-masking-get-started.md) позволяет снизить уязвимость конфиденциальных данных.
- [хранимые процедуры](https://docs.microsoft.com/sql/relational-databases/stored-procedures/stored-procedures-database-engine) .

## <a name="next-steps"></a>Дальнейшие действия

- Обзор функций защиты в базе данных SQL см. в [этой статье](sql-database-security-overview.md).
- Общие сведения о правилах брандмауэра см. в [этой статье](sql-database-firewall-configure.md).
- Сведения о пользователях и имена для входа см. в статье [Проверка подлинности и авторизация в базе данных SQL: предоставление доступа](sql-database-manage-logins.md). 
- Описание упреждающего мониторинга см. в статьях [Приступая к работе с аудитом базы данных SQL](sql-database-auditing.md) и [Обнаружение угроз Базы данных SQL Azure для отдельной базы данных или базы данных в составе пула](sql-database-threat-detection.md).
- Ознакомьтесь с руководством [Защита базы данных SQL Azure](sql-database-security-tutorial.md).
