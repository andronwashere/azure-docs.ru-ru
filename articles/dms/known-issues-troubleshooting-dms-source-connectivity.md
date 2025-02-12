---
title: Статья о известные Устранение проблемы или ошибки, связанные с подключение Azure Database Migration Service для базы данных-источники | Документация Майкрософт
description: Узнайте о способах устранения известных проблем и ошибок, связанных с подключение Azure Database Migration Service для базы данных-источники.
services: database-migration
author: HJToland3
ms.author: jtoland
manager: craigg
ms.reviewer: craigg
ms.service: dms
ms.workload: data-services
ms.custom: mvc
ms.topic: article
ms.date: 06/28/2019
ms.openlocfilehash: a4ebd1d4c85631cc6ebc1f5787e7545b78d62b5e
ms.sourcegitcommit: aa66898338a8f8c2eb7c952a8629e6d5c99d1468
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/28/2019
ms.locfileid: "67462854"
---
# <a name="troubleshoot-dms-errors-when-connecting-to-source-databases"></a>Устранение ошибок DMS при подключении к базам данных источника

В следующей статье приведены подробные сведения о способах устранения потенциальных проблем, которые могут возникнуть при подключении Azure Database Migration Service (DMS) для вашей базы данных-источника. В каждом разделе ниже относится к определенному типу базы данных-источника, список ошибки могут возникать вместе с подробные сведения и ссылки на сведения о том, как устранить неполадки с подключением.

## <a name="sql-server"></a>SQL Server;

Потенциальные проблемы, связанные с подключением к базе данных SQL Server источника и способы их решения приведены в следующей таблице.

| Ошибка         | Причины и устранение неполадок детализации |
| ------------- | ------------- |
| Ошибка подключения SQL. При подключении к SQL Server произошла ошибка, связанная с сетью или с определенным экземпляром. Сервер не найден или недоступен. Убедитесь, что имя экземпляра указано правильно и что SQL Server настроен для удаленных подключений.<br> | Эта ошибка возникает, если службу не удается найти исходный сервер. Чтобы решить проблему, см. в статье [ошибка подключения к исходному серверу SQL Server, при использовании динамических портов или именованный экземпляр](https://docs.microsoft.com/azure/dms/known-issues-troubleshooting-dms#error-connecting-to-source-sql-server-when-using-dynamic-port-or-named-instance). |
| **Ошибка 53** -ошибка подключения SQL. (Кроме того, для ошибки коды 1, 2, 5, 53, 233, 258, 1225, 11001)<br><br> | Эта ошибка возникает, если службу не удается подключиться к исходному серверу. Чтобы решить проблему, обратитесь к следующим ресурсам и повторите попытку. <br><br>  [Интерактивное руководство для устранения неполадок подключения](https://support.microsoft.com/help/4009936/solving-connectivity-errors-to-sql-server)<br><br> [Предварительные требования для переноса SQL Server в базу данных SQL Azure](https://docs.microsoft.com/azure/dms/pre-reqs#prerequisites-for-migrating-sql-server-to-azure-sql-database) <br><br> [Необходимые условия для миграции SQL Server в базе данных SQL управляемого экземпляра](https://docs.microsoft.com/azure/dms/pre-reqs#prerequisites-for-migrating-sql-server-to-an-azure-sql-database-managed-instance) |
| **Ошибка 18456** -Ошибка входа.<br> | Эта ошибка возникает, если службу не удается подключиться к исходной базе данных, с помощью предоставленных учетных данных T-SQL. Чтобы устранить проблему, проверьте введенные учетные данные. Можно также ссылаться на [MSSQLSERVER_18456](https://docs.microsoft.com/sql/relational-databases/errors-events/mssqlserver-18456-database-engine-error?view=sql-server-2017) или к документам по устранению неполадок, перечисленных в примечании под это таблицы, а затем повторите попытку. |
| Неправильно сформированное значение AccountName "{0}" предоставляются. Ожидаемый формат AccountName: Имя_домена\имя_пользователя<br> | Эта ошибка возникает, если пользователь выбирает проверки подлинности Windows, но предоставляет имя пользователя в недопустимом формате. Чтобы решить проблему, укажите имя пользователя в правильном формате для проверки подлинности Windows или выберите **проверки подлинности SQL**. |

## <a name="aws-rds-mysql"></a>MySQL RDS в AWS

Потенциальные проблемы, связанные с подключением к базе данных MySQL в AWS RDS. источника и способы их решения приведены в следующей таблице.

| Ошибка         | Причины и устранение неполадок детализации |
| ------------- | ------------- |
| **Ошибки [2003]** [HY000] - Ошибка соединения. ОШИБКИ [HY000] [MySQL] [ODBC x.x(w) driver] не удается подключиться к серверу MySQL на «{server}» (10060) | Эта ошибка возникает, если драйвер ODBC для MySQL не удается подключиться к исходному серверу. Чтобы решить проблему, ссылки на документы по устранению неполадок, перечисленных в примечании под этой таблицей, а затем повторите попытку.<br> |
| **Ошибки [2005]** [HY000] - Ошибка соединения. ОШИБКИ [HY000] [MySQL] [ODBC x.x(w) driver] неизвестный MySQL сервера «{server}» | Эта ошибка возникает, если службу не удается найти исходный узел на RDS. Проблема может быть так, как перечисленные источник не существует, или возникла проблема с инфраструктурой служб удаленных рабочих СТОЛОВ. Чтобы решить проблему, ссылки на документы по устранению неполадок, перечисленных в примечании под этой таблицей, а затем повторите попытку.<br> |
| **Ошибки [1045]** [HY000] - Ошибка соединения. ОШИБКИ [HY000] [MySQL] [ODBC x.x(w) driver] нет доступа для пользователя «{user}'@'{server}» (с использованием пароля: "ДА") | Эта ошибка возникает, если драйвер ODBC для MySQL не удается подключиться к исходному серверу из-за недопустимых учетных данных. Проверьте учетные данные, которые вы ввели. Если проблема не устранена, проверьте, что исходный компьютер имеет правильные учетные данные. Может потребоваться сбросить пароль в консоли. Если по-прежнему возникают проблемы, ссылки на документы по устранению неполадок, перечисленных в примечании под этой таблицей и повторите попытку.<br> |
| **Ошибка [9002]** [HY000] - Ошибка соединения. ОШИБКИ [HY000] [MySQL] [ODBC x.x(w) driver] строка подключения может не подходить. Посетите портал для ссылки.| Эта ошибка возникает, если произошел сбой подключения из-за проблемы со строкой подключения. Проверки, допустима ли указанная строка подключения. Чтобы решить проблему, ссылки на документы по устранению неполадок, перечисленных в примечании под этой таблицей, а затем повторите попытку.<br> |
| **Ошибка при ведении двоичного журнала. Переменная binlog_format имеет значение «{value}». Измените его на «строка».** | Эта ошибка возникает, если возникает ошибка при ведении двоичного журнала; переменная binlog_format имеет неверное значение. Чтобы решить проблему, измените binlog_format в группе параметров на «ROW» и затем перезагрузить экземпляр. Дополнительные сведения см. в подразделе [двоичные параметры ведения журналов и переменные](https://dev.mysql.com/doc/refman/5.7/en/replication-options-binary-log.html) или [документации файлов журнала базы данных MySQL служб удаленных рабочих СТОЛОВ AWS](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_LogAccess.Concepts.MySQL.html).<br> |

> [!NOTE]
> Дополнительные сведения об устранении неполадок, связанных с подключением к базе данных MySQL в AWS RDS. источника см. следующие ресурсы:
> * [Информация об устранении неполадок подключения служб удаленных рабочих СТОЛОВ Amazon](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/CHAP_Troubleshooting.html#CHAP_Troubleshooting.Connecting)
> * [Как устранить проблемы с подключением к моему экземпляру базы данных Amazon служб удаленных рабочих СТОЛОВ?](https://aws.amazon.com/premiumsupport/knowledge-center/rds-cannot-connect)

## <a name="aws-rds-postgresql"></a>PostgreSQL RDS в AWS

Потенциальные проблемы, связанные с подключением к базе данных AWS RDS PostgreSQL источника и способы их решения приведены в следующей таблице.

| Ошибка         | Причины и устранение неполадок детализации |
| ------------- | ------------- |
| **Ошибки [101]** [08001] - Ошибка соединения. ОШИБКИ [08001] время ожидания истекло. | Эта ошибка возникает, если драйвер Postgres не удается подключиться к исходному серверу. Чтобы решить проблему, ссылки на документы по устранению неполадок, перечисленных в примечании под этой таблицей, а затем повторите попытку. |
| **Ошибка. Wal_level параметр имеет значение «{value}». Измените его на «logical», чтобы разрешить репликацию.** | Эта ошибка возникает, если параметр wal_level имеет неверное значение. Чтобы решить проблему, измените rds.logical_replication в группе параметров 1 и затем перезагрузить экземпляр. Дополнительные сведения см. в подразделе [предварительные требования для миграции в Azure PostgreSQL с помощью DMS](https://docs.microsoft.com/azure/dms/tutorial-postgresql-azure-postgresql-online#prerequisites) или [PostgreSQL на Amazon RDS](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/CHAP_PostgreSQL.html). |

> [!NOTE]
> Дополнительные сведения об устранении неполадок, связанных с подключением к исходной базы данных PostgreSQL RDS в AWS см. следующие ресурсы:
> * [Информация об устранении неполадок подключения служб удаленных рабочих СТОЛОВ Amazon](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/CHAP_Troubleshooting.html#CHAP_Troubleshooting.Connecting)
> * [Как устранить проблемы с подключением к моему экземпляру базы данных Amazon служб удаленных рабочих СТОЛОВ?](https://aws.amazon.com/premiumsupport/knowledge-center/rds-cannot-connect)

## <a name="aws-rds-sql-server"></a>AWS RDS SQL Server

Потенциальные проблемы, связанные с соединения с базой данных SQL Server для служб удаленных рабочих СТОЛОВ AWS источника и способы их решения приведены в следующей таблице.

| Ошибка         | Причины и устранение неполадок детализации |
| ------------- | ------------- |
| **Ошибка 53** -ошибка подключения SQL. При подключении к SQL Server произошла ошибка, связанная с сетью или с определенным экземпляром. Сервер не был найден или не был доступен. Убедитесь, что имя экземпляра указано правильно и что SQL Server настроен для удаленных подключений. (поставщик: Поставщик именованных каналов, ошибка: 40 - не удалось открыть соединение с SQL Server | Эта ошибка возникает, если службу не удается подключиться к исходному серверу. Чтобы решить проблему, ссылки на документы по устранению неполадок, перечисленных в примечании под этой таблицей, а затем повторите попытку. |
| **Ошибка 18456** -Ошибка входа. Ошибка входа пользователя «{user}» | Эта ошибка возникает, если службу не удается подключиться к базы данных-источника с помощью T-SQL учетные данные. Чтобы устранить проблему, проверьте введенные учетные данные. Можно также ссылаться на [MSSQLSERVER_18456](https://docs.microsoft.com/sql/relational-databases/errors-events/mssqlserver-18456-database-engine-error?view=sql-server-2017) или к документам по устранению неполадок, перечисленных в примечании под это таблицы и повторите попытку. |
| **Ошибка 87** -Недопустимая строка подключения. При подключении к SQL Server произошла ошибка, связанная с сетью или с определенным экземпляром. Сервер не найден или недоступен. Убедитесь, что имя экземпляра указано правильно и что SQL Server настроен для удаленных подключений. (поставщик: Сетевые интерфейсы SQL, ошибка: 25 - Недопустимая строка подключения) | Эта ошибка возникает, если службу не удается подключиться к исходному серверу из-за недопустимой строкой подключения. Чтобы решить проблему, убедитесь, строку подключения. Если проблема будет повторяться, обратитесь к документам по устранению неполадок, перечисленных в примечании под этой таблицей и повторите попытку. |
| **Ошибка — сертификат сервера не является доверенным.** Соединение с сервером успешно установлено, но затем произошла ошибка при входе в систему. (поставщик: Поставщик SSL, ошибка: 0 — цепочка сертификатов выпущена центром сертификации, не является доверенным.) | Эта ошибка возникает, если сертификат, используемый, не являющихся доверенными. Чтобы устранить эту проблему, вам потребуется найти сертификат, который может быть доверенными и затем включите ее на сервере. Кроме того можно выбрать параметр доверять сертификату при подключении. Пользоваться этим способом, только в том случае, если вы знакомы с сертификатом, используемым и вы доверяете ему. <br> Соединения SSL, зашифрованные с помощью самозаверяющего сертификата не обеспечивают надежную защиту — они подвержены атакам man-in--middle. Не следует полагаться на SSL, с использованием самозаверяющих сертификатов в рабочей среде или на серверах, подключенных к Интернету. <br> Дополнительные сведения см. в подразделе [использование SSL с экземпляром Microsoft SQL Server DB](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/SQLServer.Concepts.General.SSL.Using.html) или [руководства: Перенос служб удаленных рабочих СТОЛОВ SQL Server в Azure с помощью DMS](https://docs.microsoft.com/azure/dms/tutorial-rds-sql-server-azure-sql-and-managed-instance-online#prerequisites). |
| **Ошибка 300** -пользователь не имеет необходимых разрешений. Запрещено разрешение VIEW SERVER STATE на объект «{server}», база данных «{}» | Эта ошибка возникает, если пользователь не имеет разрешения для выполнения миграции. Чтобы устранить эту проблему, обратитесь к [ПРЕДОСТАВИТЬ разрешения сервера - Transact-SQL](https://docs.microsoft.com/sql/t-sql/statements/grant-server-permissions-transact-sql?view=sql-server-2017) или [руководства: Перенос служб удаленных рабочих СТОЛОВ SQL Server в Azure с помощью DMS](https://docs.microsoft.com/azure/dms/tutorial-rds-sql-server-azure-sql-and-managed-instance-online#prerequisites) для получения дополнительных сведений. |

> [!NOTE]
> Дополнительные сведения об устранении неполадок, связанных с подключением к исходному серверу SQL Server на использование служб удаленных рабочих СТОЛОВ AWS см. следующие ресурсы:
>
> * [Solving Connectivity errors to SQL Server](https://support.microsoft.com/help/4009936/solving-connectivity-errors-to-sql-server) (Устранение ошибок подключения к серверу SQL Server)
> * [Как устранить проблемы с подключением к моему экземпляру базы данных Amazon служб удаленных рабочих СТОЛОВ?](https://aws.amazon.com/premiumsupport/knowledge-center/rds-cannot-connect)

## <a name="known-issues"></a>Известные проблемы

* [Известные проблемы и переноса ограничения, связанные с online перенос в базу данных SQL Azure](https://docs.microsoft.com/azure/dms/known-issues-azure-sql-online)
* [Известные проблемы и переноса ограничения, связанные с интерактивной миграции к базе данных Azure для MySQL](https://docs.microsoft.com/azure/dms/known-issues-azure-mysql-online)
* [Известные проблемы и переноса ограничения, связанные с интерактивной миграции к базе данных Azure для PostgreSQL](https://docs.microsoft.com/azure/dms/known-issues-azure-postgresql-online)

## <a name="next-steps"></a>Дальнейшие действия

* См. в статье [PowerShell службы миграции базы данных Azure](https://docs.microsoft.com/powershell/module/azurerm.datamigration/?view=azurermps-6.13.0#data_migration).
* См. в статье [как настроить параметры сервера в базе данных Azure для MySQL с помощью портала Azure](https://docs.microsoft.com/azure/mysql/howto-server-parameters).
* См. в статье [предварительные требования для с помощью Azure Database Migration Service](https://docs.microsoft.com/azure/dms/pre-reqs).
* См. в разделе [часто задаваемые вопросы о службе Azure Database Migration Service](https://docs.microsoft.com/azure/dms/faq).
