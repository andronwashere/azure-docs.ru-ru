---
title: Коды ошибок SQL — ошибки подключения к базе данных | Документация Майкрософт
description: 'Дополнительные сведения о кодах ошибок SQL для клиентских приложений баз данных SQL, например общих ошибок подключения к базе данных, проблем при копировании баз данных и общих ошибок. '
keywords: код ошибки SQL, доступ к SQL, ошибка подключения к базе данных, коды ошибок SQL
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: ''
manager: craigg
ms.date: 03/06/2019
ms.openlocfilehash: 2682f98628f3c1cf22a2c3767f52bedbc148fa62
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "60723506"
---
# <a name="sql-error-codes-for-sql-database-client-applications-database-connection-errors-and-other-issues"></a>Коды ошибок SQL для клиентских приложений базы данных SQL: ошибки подключения базы данных и другие проблемы

В этой статье перечислены коды ошибок SQL для клиентского приложения базы данных SQL, включая ошибки подключения к базе данных, временные ошибки (или временные сбои), ошибки управления ресурсами, проблемы при копировании баз данных, ошибки пула эластичных БД и другие ошибки. Большинство категорий относятся к базам данных SQL Azure и не применяются к Microsoft SQL Server. Дополнительные сведения см. в статье [Системные сообщения об ошибках](https://technet.microsoft.com/library/cc645603(v=sql.105).aspx).

## <a name="database-connection-errors-transient-errors-and-other-temporary-errors"></a>Ошибки подключения к базе данных и временные ошибки

В следующей таблице перечислены коды ошибок SQL для ошибок потери подключения и других временных ошибок, которые могут возникнуть, когда приложение пытается получить доступ к базе данных SQL. Учебники по началу работы с подключением к Базе данных SQL Azure доступны в разделе [Подключение к Базе данных SQL Azure](sql-database-libraries.md).

### <a name="most-common-database-connection-errors-and-transient-fault-errors"></a>Наиболее распространенные ошибки подключения к базе данных, а также временные сбои и ошибки

Инфраструктура Azure способна динамически изменять конфигурацию серверов при высокой рабочей нагрузке на службу баз данных SQL.  Такое динамическое поведение может привести к разрыву подключения между клиентской программой и базой данных SQL. Такое состояние называется *временной ошибкой*.

Настоятельно рекомендуем включить в клиентской программе логику повторных попыток, чтобы программа смогла восстановить подключение после определенного периода, позволяющего временной ошибке самоустраниться.  Рекомендуется подождать 5 секунд, прежде чем выполнять первую повторную попытку. Повторная попытка после ожидания менее 5 секунд может привести к перегрузке облачной службы. Для каждой последующей повторной попытки ожидание должно увеличиваться экспоненциально, но не более чем до 60 секунд.

Временные сбои обычно проявляются в виде одного из следующих сообщений об ошибке из клиентских программ:

* База данных &lt;имя_базы_данных&gt; на сервере &lt;экземпляр_Azure&gt; сейчас недоступна. Повторите попытку подключения позже. Если проблема повторится, обратитесь в службу поддержки клиентов и сообщите ИД трассировки сеанса &lt;ИД_сеанса&gt;.
* База данных &lt;имя_базы_данных&gt; на сервере &lt;экземпляр_Azure&gt; сейчас недоступна. Повторите попытку подключения позже. Если проблема повторится, обратитесь в службу поддержки клиентов и сообщите ИД трассировки сеанса &lt;ИД_сеанса&gt;. (Microsoft SQL Server, ошибка: 40613)
* Существующее подключение было принудительно разорвано удаленным узлом.
* System.Data.Entity.Core.EntityCommandExecutionException: произошла ошибка при выполнении определения команды. Дополнительные сведения см. во внутреннем исключении. ---> System.Data.SqlClient.SqlException: ошибка на транспортном уровне при получении результатов с сервера. (поставщик: поставщик сеансов, ошибка: 19 — физическое подключение недоступно)
* Попытка подключения к базе данных-получателю завершилась сбоем, так как база данных повторно настраивается и к ней применяются новые страницы в процессе активной транзакции в базе данных-источнике. 

Примеры кода с логикой повторных попыток см. в следующих статьях:

* [Библиотеки подключений для базы данных SQL и SQL Server](sql-database-libraries.md) 
* [Исправление ошибок подключения и временных ошибок в базе данных SQL](sql-database-connectivity-issues.md)

Обсуждение *периода блокировки* для клиентов, которые используют ADO.NET, см. в статье [Организация пулов соединений SQL Server (ADO.NET)](https://msdn.microsoft.com/library/8xx3tyca.aspx).

### <a name="transient-fault-error-codes"></a>Коды ошибок для временных сбоев

Ниже приведены временные ошибки, для которых в приложении следует реализовать логику повтора. 

| Код ошибки | Уровень серьезности | Описание |
| ---:| ---:|:--- |
| 4060 |16 |Невозможно открыть базу данных %.&#x2a;ls, запрашиваемую именем входа. Вход в систему не выполнен. Дополнительные сведения см. в разделе [ошибки от 4000 до 4999](https://docs.microsoft.com/sql/relational-databases/errors-events/database-engine-events-and-errors#errors-4000-to-4999)|
| 40197 |17 |При обработке вашего запроса служба обнаружила ошибку. Повторите попытку позже. Код ошибки: %d.<br/><br/>Эта ошибка возникает, если служба не работает по причине обновления программного или аппаратного обеспечения, аппаратных ошибок или проблем при отработке отказа. Код ошибки (%d), указанный в сообщении об ошибке 40197, содержит дополнительные сведения о характере сбоя или отработки отказа. В сообщении об ошибке 40197 может быть указан код ошибки 40020, 40143, 40166 или 40540.<br/><br/>При повторном подключении сервер Базы данных SQL автоматически подключается к рабочей копии базы данных. Приложение должно зафиксировать ошибку 40197, зарегистрировать указанный в сообщении код ошибки (%d) для устранения неполадок и повторять попытки подключения к базе данных SQL, пока не появится доступ к ресурсам, а подключение не будет восстановлено. Дополнительные сведения см. в разделе [временных ошибок](sql-database-connectivity-issues.md#transient-errors-transient-faults).|
| 40501 |20 |Служба в настоящий момент занята. Повторите запрос через 10 секунд. Идентификатор инцидента: %ls. Код: %d. Дополнительные сведения можно найти в разделе <br/>&bull; &nbsp;[Ограничения ресурсов базы данных сервера](sql-database-resource-limits-database-server.md)<br/>&bull; &nbsp;[Ограничения на основе единиц DTU для отдельных баз данных](sql-database-service-tiers-dtu.md)<br/>&bull; &nbsp;[Ограничения на основе единиц DTU для эластичных пулов](sql-database-dtu-resource-limits-elastic-pools.md)<br/>&bull; &nbsp;[ограничения на основе виртуальных ядер для отдельных баз данных](sql-database-vcore-resource-limits-single-databases.md)<br/>&bull; &nbsp;[ограничения на основе виртуальных ядер для эластичных пулов](sql-database-vcore-resource-limits-elastic-pools.md)<br/>&bull; &nbsp;[Управляемый экземпляр ограничения ресурсов](sql-database-managed-instance-resource-limits.md).|
| 40613 |17 |База данных %.&#x2a;ls на сервере %.&#x2a;ls в данный момент недоступна. Повторите попытку подключения позже. Если проблема повторится, обратитесь в службу поддержки пользователей и сообщите идентификатор трассировки сеанса %.&#x2a;ls.<br/><br/> Эта ошибка может возникать, если выделенное административное соединение (DAC) к базе данных уже существует. Дополнительные сведения см. в разделе [временных ошибок](sql-database-connectivity-issues.md#transient-errors-transient-faults).|
| 49918 |16 |Не удается обработать запрос. Недостаточно ресурсов для обработки запроса.<br/><br/>Служба занята. Повторите запрос позже. Дополнительные сведения можно найти в разделе <br/>&bull; &nbsp;[Ограничения ресурсов базы данных сервера](sql-database-resource-limits-database-server.md)<br/>&bull; &nbsp;[Ограничения на основе единиц DTU для отдельных баз данных](sql-database-service-tiers-dtu.md)<br/>&bull; &nbsp;[Ограничения на основе единиц DTU для эластичных пулов](sql-database-dtu-resource-limits-elastic-pools.md)<br/>&bull; &nbsp;[ограничения на основе виртуальных ядер для отдельных баз данных](sql-database-vcore-resource-limits-single-databases.md)<br/>&bull; &nbsp;[ограничения на основе виртуальных ядер для эластичных пулов](sql-database-vcore-resource-limits-elastic-pools.md)<br/>&bull; &nbsp;[Управляемый экземпляр ограничения ресурсов](sql-database-managed-instance-resource-limits.md). |
| 49919 |16 |Невозможно обработать запрос на создание или обновление. Для подписки "%ld" выполняется слишком много операций создания или обновления.<br/><br/>Служба занята обработкой нескольких запросов на создание или обновление для вашей подписки или сервера. В данный момент запросы блокируются для оптимизации ресурсов. Выполните запрос [sys.dm_operation_status](https://msdn.microsoft.com/library/dn270022.aspx) для ожидающих операций. Подождите, пока выполнятся ожидающие запросы на создание или обновление, либо удалите один из ожидающих запросов и повторите свой запрос позже. Дополнительные сведения можно найти в разделе <br/>&bull; &nbsp;[Ограничения ресурсов базы данных сервера](sql-database-resource-limits-database-server.md)<br/>&bull; &nbsp;[Ограничения на основе единиц DTU для отдельных баз данных](sql-database-service-tiers-dtu.md)<br/>&bull; &nbsp;[Ограничения на основе единиц DTU для эластичных пулов](sql-database-dtu-resource-limits-elastic-pools.md)<br/>&bull; &nbsp;[ограничения на основе виртуальных ядер для отдельных баз данных](sql-database-vcore-resource-limits-single-databases.md)<br/>&bull; &nbsp;[ограничения на основе виртуальных ядер для эластичных пулов](sql-database-vcore-resource-limits-elastic-pools.md)<br/>&bull; &nbsp;[Управляемый экземпляр ограничения ресурсов](sql-database-managed-instance-resource-limits.md). |
| 49920 |16 |Не удается обработать запрос. Для подписки «%ld» выполняется слишком много операций.<br/><br/>Служба занята обработкой нескольких запросов для этой подписки. В данный момент запросы блокируются для оптимизации ресурсов. Выполните запрос [sys.dm_operation_status](https://msdn.microsoft.com/library/dn270022.aspx) для состояния операции. Подождите, пока выполнятся ожидающие запросы, либо удалите один из ожидающих запросов и повторите свой запрос позже. Дополнительные сведения можно найти в разделе <br/>&bull; &nbsp;[Ограничения ресурсов базы данных сервера](sql-database-resource-limits-database-server.md)<br/>&bull; &nbsp;[Ограничения на основе единиц DTU для отдельных баз данных](sql-database-service-tiers-dtu.md)<br/>&bull; &nbsp;[Ограничения на основе единиц DTU для эластичных пулов](sql-database-dtu-resource-limits-elastic-pools.md)<br/>&bull; &nbsp;[ограничения на основе виртуальных ядер для отдельных баз данных](sql-database-vcore-resource-limits-single-databases.md)<br/>&bull; &nbsp;[ограничения на основе виртуальных ядер для эластичных пулов](sql-database-vcore-resource-limits-elastic-pools.md)<br/>&bull; &nbsp;[Управляемый экземпляр ограничения ресурсов](sql-database-managed-instance-resource-limits.md). |
| 4221 |16 |Произошел сбой при входе в базу данных-получатель для чтения из-за длительного ожидания выполнения HADR_DATABASE_WAIT_FOR_TRANSITION_TO_VERSIONING. Вход в реплику невозможен, так как отсутствуют версии строк для транзакций, которые выполнялись при перезапуске реплики. Чтобы устранить проблему, откатите реплику или зафиксируйте активные транзакции в первичной реплике. Большинство таких случаев можно избежать, если не выполнять длительные транзакции записи в базе данных-источнике. |

## <a name="database-copy-errors"></a>Ошибки копирования базы данных

При копировании базы данных в базе данных SQL Azure могут возникнуть следующие ошибки. Дополнительные сведения см. в статье [Копирование базы данных SQL Azure](sql-database-copy.md).

| Код ошибки | Уровень серьезности | Описание |
| ---:| ---:|:--- |
| 40635 |16 |Клиент с IP-адресом %.&#x2a;ls временно отключен. |
| 40637 |16 |Возможность создания копии базы данных в настоящее время отключена. |
| 40561 |16 |Не удалось скопировать базу данных. Исходная или целевая база данных не существует. |
| 40562 |16 |Не удалось скопировать базу данных. Исходная база данных удалена. |
| 40563 |16 |Не удалось скопировать базу данных. Целевая база данных удалена. |
| 40564 |16 |Произошел сбой при копировании базы данных из-за внутренней ошибки. Удалите целевую базу данных и повторите попытку. |
| 40565 |16 |Не удалось скопировать базу данных. Допускается не более одной одновременной операции копирования базы данных из одного источника. Удалите целевую базу данных и повторите попытку позднее. |
| 40566 |16 |Произошел сбой при копировании базы данных из-за внутренней ошибки. Удалите целевую базу данных и повторите попытку. |
| 40567 |16 |Произошел сбой при копировании базы данных из-за внутренней ошибки. Удалите целевую базу данных и повторите попытку. |
| 40568 |16 |Не удалось скопировать базу данных. Исходная база данных стала недоступна. Удалите целевую базу данных и повторите попытку. |
| 40569 |16 |Не удалось скопировать базу данных. Целевая база данных стала недоступна. Удалите целевую базу данных и повторите попытку. |
| 40570 |16 |Произошел сбой при копировании базы данных из-за внутренней ошибки. Удалите целевую базу данных и повторите попытку позднее. |
| 40571 |16 |Произошел сбой при копировании базы данных из-за внутренней ошибки. Удалите целевую базу данных и повторите попытку позднее. |

## <a name="resource-governance-errors"></a>Ошибки управления ресурсами

Ниже представлены ошибки, возникающие из-за чрезмерного использования ресурсов при работе с базой данных SQL Azure. Пример.

* транзакция остается открытой слишком долго;
* транзакция содержит слишком много блокировок;
* приложение использует слишком много памяти;
* приложение использует слишком много пространства `TempDb` .

Похожие темы:

* Дополнительные сведения можно найти в разделе
  * [Ограничения ресурсов базы данных сервера](sql-database-resource-limits-database-server.md)
  * [Ограничения на основе единиц DTU для отдельных баз данных](sql-database-service-tiers-dtu.md)
  * [Ограничения на основе единиц DTU для эластичных пулов](sql-database-dtu-resource-limits-elastic-pools.md)
  * [ограничения на основе виртуальных ядер для отдельных баз данных](sql-database-vcore-resource-limits-single-databases.md)
  * [ограничения на основе виртуальных ядер для эластичных пулов](sql-database-vcore-resource-limits-elastic-pools.md)
  * [Управляемый экземпляр ограничения ресурсов](sql-database-managed-instance-resource-limits.md). 

| Код ошибки | Уровень серьезности | Описание |
| ---:| ---:|:--- |
| 10928 |20 |Идентификатор ресурса: %d. Предел %s для базы данных составляет %d, и он достигнут. Дополнительные сведения см. в статье [Ограничения ресурсов Базы данных SQL для отдельных баз данных и баз данных в пуле](sql-database-resource-limits-database-server.md).<br/><br/>Идентификатор ресурса указывает на ресурс, предел которого был достигнут. Для рабочих потоков идентификатор ресурса = 1. Для сеансов идентификатор ресурса — 2.<br/><br/>Дополнительные сведения об этой ошибке и способах ее устранения см. в статье: <br/>&bull; &nbsp;[Ограничения ресурсов базы данных сервера](sql-database-resource-limits-database-server.md)<br/>&bull; &nbsp;[Ограничения на основе единиц DTU для отдельных баз данных](sql-database-service-tiers-dtu.md)<br/>&bull; &nbsp;[Ограничения на основе единиц DTU для эластичных пулов](sql-database-dtu-resource-limits-elastic-pools.md)<br/>&bull; &nbsp;[ограничения на основе виртуальных ядер для отдельных баз данных](sql-database-vcore-resource-limits-single-databases.md)<br/>&bull; &nbsp;[ограничения на основе виртуальных ядер для эластичных пулов](sql-database-vcore-resource-limits-elastic-pools.md)<br/>&bull; &nbsp;[Управляемый экземпляр ограничения ресурсов](sql-database-managed-instance-resource-limits.md). |
| 10929 |20 |Идентификатор ресурса: %d. Минимальная гарантия %s составляет %d, максимальное значение равно %d, а текущее использование для базы данных — %d. Тем не менее, в настоящее время сервер слишком занят, чтобы обработать более чем %d запросов для этой базы данных. Идентификатор ресурса указывает на ресурс, предел которого был достигнут. Для рабочих потоков идентификатор ресурса = 1. Для сеансов идентификатор ресурса — 2. Дополнительные сведения можно найти в разделе <br/>&bull; &nbsp;[Ограничения ресурсов базы данных сервера](sql-database-resource-limits-database-server.md)<br/>&bull; &nbsp;[Ограничения на основе единиц DTU для отдельных баз данных](sql-database-service-tiers-dtu.md)<br/>&bull; &nbsp;[Ограничения на основе единиц DTU для эластичных пулов](sql-database-dtu-resource-limits-elastic-pools.md)<br/>&bull; &nbsp;[ограничения на основе виртуальных ядер для отдельных баз данных](sql-database-vcore-resource-limits-single-databases.md)<br/>&bull; &nbsp;[ограничения на основе виртуальных ядер для эластичных пулов](sql-database-vcore-resource-limits-elastic-pools.md)<br/>&bull; &nbsp;[Управляемый экземпляр ограничения ресурсов](sql-database-managed-instance-resource-limits.md). <br/>В противном случае повторите попытку позже. |
| 40544 |20 |База данных достигла предельного размера. Секционируйте или удалите данные, удалите индексы или попробуйте найти возможное решение в документации. Масштабирование базы данных, см. в разделе [масштабирование отдельной базы данных ресурсов](sql-database-single-database-scale.md) и [масштабирование эластичного пула ресурсов](sql-database-elastic-pool-scale.md).|
| 40549 |16 |Сеанс остановлен из-за наличия транзакции с длительным временем выполнения. Попробуйте сократить время выполнения транзакции. Сведения о пакетной обработке см. в разделе [с помощью пакетной обработки для повышения производительности приложения базы данных SQL](sql-database-use-batching-to-improve-performance.md).|
| 40550 |16 |Сеанс остановлен, поскольку он использует слишком много блокировок. Попробуйте сократить количество читаемых или изменяемых строк в одной транзакции. Сведения о пакетной обработке см. в разделе [с помощью пакетной обработки для повышения производительности приложения базы данных SQL](sql-database-use-batching-to-improve-performance.md).|
| 40551 |16 |Сеанс остановлен из-за чрезмерного использования `TEMPDB`. Попробуйте изменить запрос, чтобы сократить использование временного табличного пространства.<br/><br/>Если вы используете временные объекты, то для экономии места в базе данных `TEMPDB` удаляйте их сразу после того, как в них исчезнет необходимость. Дополнительные сведения об использовании базы данных tempdb в базе данных SQL см. в разделе [базы данных Tempdb в базе данных SQL](https://docs.microsoft.com/sql/relational-databases/databases/tempdb-database#tempdb-database-in-sql-database).|
| 40552 |16 |Сеанс остановлен из-за чрезмерного использования места для журналов транзакций. Попробуйте сократить количество изменяемых строк в одной транзакции. Сведения о пакетной обработке см. в разделе [с помощью пакетной обработки для повышения производительности приложения базы данных SQL](sql-database-use-batching-to-improve-performance.md).<br/><br/>В случае выполнения массовых вставок с использованием служебной программы `bcp.exe` или класса `System.Data.SqlClient.SqlBulkCopy` попробуйте ограничить количество строк, копируемых на сервер при каждой транзакции, с помощью параметра `-b batchsize` или `BatchSize`. В случае перестройки индекса с помощью оператора `ALTER INDEX` попробуйте использовать параметр `REBUILD WITH ONLINE = ON`. Сведения о размерах журнала транзакций для модели приобретения на виртуальное см. в разделе: <br/>&bull; &nbsp;[ограничения на основе виртуальных ядер для отдельных баз данных](sql-database-vcore-resource-limits-single-databases.md)<br/>&bull; &nbsp;[ограничения на основе виртуальных ядер для эластичных пулов](sql-database-vcore-resource-limits-elastic-pools.md)<br/>&bull; &nbsp;[Управляемый экземпляр ограничения ресурсов](sql-database-managed-instance-resource-limits.md).|
| 40553 |16 |Сеанс остановлен из-за чрезмерного потребления памяти. Попробуйте изменить запрос и задать обработку меньшего числа строк.<br/><br/>Сокращение количества операций `ORDER BY` и `GROUP BY` в коде Transact-SQL позволяет уменьшить потребление памяти при выполнении запроса. Масштабирование базы данных, см. в разделе [масштабирование отдельной базы данных ресурсов](sql-database-single-database-scale.md) и [масштабирование эластичного пула ресурсов](sql-database-elastic-pool-scale.md).|

## <a name="elastic-pool-errors"></a>Ошибки пула эластичных БД

Приведенные ниже ошибки связаны с созданием и использованием эластичных пулов.

| Код ошибки | Уровень серьезности | Описание | Корректирующее действие |
|:--- |:--- |:--- |:--- |
| 1132 | 17 |Хранилище эластичного пула достигло своего предельного значения. Уровень использования хранилища для эластичного пула не может превышать (%d) МБ. Попытка записи данных в базу данных, когда было достигнуто предельное значение хранилища эластичного пула. Сведения об ограничениях ресурсов см. в разделе: <br/>&bull; &nbsp;[Ограничения на основе единиц DTU для эластичных пулов](sql-database-dtu-resource-limits-elastic-pools.md)<br/>&bull; &nbsp;[ограничения на основе виртуальных ядер для эластичных пулов](sql-database-vcore-resource-limits-elastic-pools.md). <br/> |Если это возможно, попробуйте увеличить DTU эластичного пула и (или) добавить в него пространство хранения для увеличения предельного размера его хранилища, сократить объем памяти, используемой отдельными базами данных в эластичном пуле, или удалить базы данных из пула. Масштабирование эластичного пула, см. в разделе [масштабирование эластичного пула ресурсов](sql-database-elastic-pool-scale.md).|
| 10929 | 16 |Минимальная гарантия %s составляет %d, максимальное значение равно %d, а текущее использование для базы данных — %d. Тем не менее, в настоящее время сервер слишком занят, чтобы обработать более чем %d запросов для этой базы данных. Сведения об ограничениях ресурсов см. в разделе: <br/>&bull; &nbsp;[Ограничения на основе единиц DTU для эластичных пулов](sql-database-dtu-resource-limits-elastic-pools.md)<br/>&bull; &nbsp;[ограничения на основе виртуальных ядер для эластичных пулов](sql-database-vcore-resource-limits-elastic-pools.md). <br/> В противном случае повторите попытку позже. Минимальное количество DTU или виртуальных ядер на базу данных; максимальное количество DTU или виртуальных ядер на базу данных. Общее количество одновременных работников (запросов) по всем базам данных в эластичном пуле превысило ограничение пула. |Если это возможно, попробуйте увеличить DTU или число виртуальных ядер эластичного пула для увеличения предельно допустимого количества работников или удалить базы данных из эластичного пула. |
| 40844 | 16 |База данных '%ls' на сервере '%ls' является базой данных выпуска '%ls' в эластичном пуле и не может иметь связь непрерывной копии.  |Н/Д |
| 40857 | 16 |Эластичный пул для сервера '%ls' не найден, имя эластичного пула: '%ls'. На указанном сервере указанный эластичный пул не существует. | Укажите допустимое имя эластичного пула. |
| 40858 | 16 |Эластичный пул "%ls" на сервере "%ls" уже существует. Указанный эластичный пул уже существует на указанном сервере Базы данных SQL. | Укажите новое имя эластичного пула. |
| 40859 | 16 |Эластичный пул не поддерживает уровень служб '%ls'. Для подготовки эластичного пула указанный уровень служб не поддерживается. |Укажите правильный выпуск или оставьте значение уровня служб пустым, чтобы использовать значение по умолчанию. |
| 40860 | 16 |Недопустимая комбинация эластичного пула '%ls' и цели службы '%ls'. Эластичный пул и уровень служб можно указать вместе только в том случае, если тип ресурса задан как ElasticPool. |Укажите правильную комбинацию эластичного пула и уровня служб. |
| 40861 | 16 |Выпуск "%.*ls" базы данных не может отличаться от уровня служб пула эластичных БД, который равен "%.* ls". Выпуск базы данных отличается от уровня служб эластичного пула. |Не указывайте выпуск базы данных, который отличается от уровня служб эластичного пула.  Обратите внимание, что выпуск базы данных указывать не требуется. |
| 40862 | 16 |Если указана цель служб эластичного пула, то необходимо указать имя этого эластичного пула. Цель службы эластичного пула неоднозначно определяет эластичный пул. |Если используется цель служб эластичного пула, укажите имя этого эластичного пула. |
| 40864 | 16 |Число DTU для эластичного пула должно составлять по крайней мере (%d) DTU для уровня служб "%.*ls". Попытка задать для эластичного пула DTU, значение которого ниже минимального ограничения. |Повторите попытку, установив для эластичного пула DTU, значение которого хотя бы равно минимальному ограничению. |
| 40865 | 16 |Число DTU для эластичного пула не должно превышать (%d) DTU для уровня служб "%.*ls". Попытка задать для эластичного пула DTU, значение которого выше максимального ограничения. |Повторите попытку, установив для эластичного пула DTU, значение которого не превышает максимальное ограничение. |
| 40867 | 16 |Максимальное количество DTU на каждую базу данных должно составлять как минимум (%d) для уровня служб "%.*ls". Попытка задать максимальное количество DTU на каждую базу данных ниже поддерживаемого ограничения. | Попробуйте использовать уровень служб эластичного пула, который поддерживает желаемый параметр. |
| 40868 | 16 |Максимальное количество DTU на каждую базу данных не должно превышать (%d) для уровня служб "%.*ls". Попытка задать максимальное число DTU на каждую базу данных, которое находится за пределами поддерживаемого ограничения. | Попробуйте использовать уровень служб эластичного пула, который поддерживает желаемый параметр. |
| 40870 | 16 |Минимальное количество DTU на каждую базу данных не должно превышать (%d) для уровня служб "%.*ls". Попытка задать минимальное число DTU на каждую базу, которое находится за пределами поддерживаемого ограничения. | Попробуйте использовать уровень служб эластичного пула, который поддерживает желаемый параметр. |
| 40873 | 16 |Количество баз данных (%d) и минимальное количество DTU на каждую базу данных (%d) не может превышать DTU эластичного пула (%d). Попытка задать минимальное количество DTU для баз данных в эластичном пуле, превышающее DTU эластичного пула. | Попробуйте увеличить количество DTU эластичного пула, уменьшить минимальное количество DTU на каждую базу данных или уменьшить количество баз данных в эластичном пуле. |
| 40877 | 16 |Эластичный пул не может быть удален до тех пор, пока он содержит какую-либо базу данных. Эластичный пул содержит одну или несколько баз данных и не может быть удален. |Чтобы удалить эластичный пул, удалите из него базы данных. |
| 40881 | 16 |В эластичном пуле '%.*ls' достигнуто предельное количество баз данных.  Предельное количество баз данных не может превышать (%d) для эластичного пула с DTU (%d). Попытка создания или добавления базы данных в эластичный пул, когда было достигнуто предельное количество баз данных эластичного пула. | Если это возможно, попробуйте увеличить DTU эластичного пула, чтобы увеличить предельное количество баз данных, или удалить базы данных из эластичного пула. |
| 40889 | 16 |Невозможно уменьшить количество DTU или размер хранилища для эластичного пула '%.*ls', так как недостаточно места в хранилище баз данных. Попытка ограничить размер хранилища эластичного пула, при которой уменьшается размер используемого им хранилища. | Попробуйте сократить использование хранилища отдельными базами данных в эластичном пуле или удалить базы данных из пула, чтобы уменьшить количество DTU или размер хранилища. |
| 40891 | 16 |Минимальное число DTU на базу данных (%d) не может превышать максимального числа DTU на базу данных (%d). Попытка задать минимальное число DTU на каждую базу данных, превышающее максимальное число DTU на каждую базу данных. |Убедитесь, что минимальное число DTU на базу данных не превышает максимального числа DTU на базу данных. |
| Подлежит определению | 16 |Размер хранилища для отдельной базы данных в эластичном пуле не может превышать максимальный размер, допустимый уровнем служб эластичного пула "%.*ls". Максимальный размер базы данных превышает максимальный размер, допустимый уровнем служб эластичного пула. |Укажите максимальный размер базы данных, который находится в пределах ограничений на максимальный размер, допустимый уровнем служб эластичного пула. |

Похожие темы:

* [Создание эластичного пула с помощью C#](sql-database-elastic-pool-manage-csharp.md)
* [Управление эластичным пулом с помощью C#](sql-database-elastic-pool-manage-csharp.md)
* [Создание эластичного пула с помощью PowerShell](sql-database-elastic-pool-manage-powershell.md)
* [Мониторинг эластичного пула и управление им с помощью PowerShell](sql-database-elastic-pool-manage-powershell.md)

## <a name="general-errors"></a>Общие ошибки

Приведенные ниже ошибки не входят ни в одну из предыдущих категорий.

| Код ошибки | Уровень серьезности | Описание |
| ---:| ---:|:--- |
| [15006](https://docs.microsoft.com/sql/relational-databases/errors-events/database-engine-events-and-errors#errors-15000-to-15999) |16 |(AdministratorLogin) не является допустимым именем, так как содержит недопустимые символы.|
| [18452](https://docs.microsoft.com/sql/relational-databases/errors-events/database-engine-events-and-errors#errors-18000-to-18999) |14 |Вход в систему не выполнен. Имя входа принадлежит недоверенному домену и не может использоваться в проверке подлинности Windows.%.&#x2a;ls (имена входа Windows не поддерживаются в данной версии SQL Server). |
| [18456](https://docs.microsoft.com/sql/relational-databases/errors-events/database-engine-events-and-errors#errors-18000-to-18999) |14 |Пользователю %.&#x2a;ls не удалось войти в систему.%.&#x2a;ls%.&#x2a;ls (Пользователю %.&#x2a;ls не удалось войти в систему.) |
| [18470](https://docs.microsoft.com/sql/relational-databases/errors-events/database-engine-events-and-errors#errors-18000-to-18999) |14 |Пользователю %.&#x2a;ls не удалось войти в систему. Причина: учетная запись отключена.%.&#x2a;ls |
| 40014 |16 |Использовать в одной и той же транзакции несколько баз данных нельзя. |
| 40054 |16 |Таблицы без кластеризованного индекса не поддерживаются в данной версии SQL Server. Создайте кластеризованный индекс и повторите попытку. |
| 40133 |15 |Эта операция не поддерживается в данной версии SQL Server. |
| 40506 |16 |Указанный идентификатор SID является недопустимым для данной версии SQL Server. |
| 40507 |16 |В этой версии SQL Server %.&#x2a;ls не может быть вызван с параметрами. |
| 40508 |16 |Использование инструкции USE для переключения между базами данных не поддерживается. Для соединения с другой базой данных следует использовать новое подключение. |
| 40510 |16 |Оператор %.&#x2a;ls не поддерживается в данной версии SQL Server. |
| 40511 |16 |Встроенная функция %.&#x2a;ls не поддерживается в данной версии SQL Server. |
| 40512 |16 |Устаревшая функция %ls не поддерживается в данной версии SQL Server. |
| 40513 |16 |Серверная переменная %.&#x2a;ls не поддерживается в данной версии SQL Server. |
| 40514 |16 |%ls не поддерживается в данной версии SQL Server. |
| 40515 |16 |Ссылка на имя базы данных или сервера в %.&#x2a;ls не поддерживается в данной версии SQL Server. |
| 40516 |16 |Глобальные временные объекты не поддерживаются в данной версии SQL Server. |
| 40517 |16 |Ключевое слово или параметр оператора %.&#x2a;ls не поддерживается в данной версии SQL Server. |
| 40518 |16 |Команда DBCC %.&#x2a;ls не поддерживается в данной версии SQL Server. |
| 40520 |16 |Класс защищаемого объекта %S_MSG не поддерживается в данной версии SQL Server. |
| 40521 |16 |Класс защищаемого объекта %S_MSG не поддерживается в области сервера в данной версии SQL Server. |
| 40522 |16 |Тип участника базы данных %.&#x2a;ls не поддерживается в данной версии SQL Server. |
| 40523 |16 |Создание неявного пользователя %.&#x2a;ls не поддерживается в данной версии SQL Server. Перед использованием пользователя следует создать явно. |
| 40524 |16 |Тип данных %.&#x2a;ls не поддерживается в данной версии SQL Server. |
| 40525 |16 |WITH %ls не поддерживается в данной версии SQL Server. |
| 40526 |16 |Поставщик наборов строк %.&#x2a;ls не поддерживается в данной версии SQL Server. |
| 40527 |16 |Связанные серверы не поддерживаются в данной версии SQL Server. |
| 40528 |16 |Пользователи не могут быть сопоставлены с сертификатами, асимметричными ключами или именами входа Windows в данной версии SQL Server. |
| 40529 |16 |Встроенная функция %.&#x2a;ls в контексте олицетворения не поддерживается в этой версии SQL Server. |
| 40532 |11 |Невозможно открыть сервер %.&#x2a;ls, запрашиваемый именем входа. Вход в систему не выполнен. |
| 40553 |16 |Сеанс остановлен из-за чрезмерного потребления памяти. Попробуйте изменить запрос и задать обработку меньшего числа строк.<br/><br/> Сокращение количества операций `ORDER BY` и `GROUP BY` в коде Transact-SQL позволяет уменьшить потребление памяти при выполнении запроса. |
| 40604 |16 |Не удалось создать или изменить базу данных, поскольку это приведет к превышению квоты сервера. |
| 40606 |16 |Прикрепление баз данных не поддерживается в данной версии SQL Server. |
| 40607 |16 |Имена входа Windows не поддерживаются в данной версии SQL Server. |
| 40611 |16 |На серверах может быть задано не более 128 правил брандмауэра. |
| 40614 |16 |Начальный IP-адрес правила брандмауэра не может превышать конечный IP-адрес этого правила. |
| 40615 |16 |Не удается открыть сервер {0}, запрашиваемый с использованием имени для входа. Для клиента с IP-адресом {1} доступ к серверу запрещен.<br /><br />Разрешите доступ с помощью программы SQL Azure Portal или выполните хранимую процедуру sp\_set\_firewall\_rule в базе данных master, чтобы создать правило брандмауэра для этого IP-адреса или диапазона адресов. Может потребоваться до пяти минут, чтобы это изменение вступило в силу. |
| 40617 |16 |Имя правила брандмауэра, начинающееся с символа имени правила, слишком длинное. Максимальная длина — 128 символов. |
| 40618 |16 |Имя правила брандмауэра не может быть пустым. |
| 40620 |16 |Пользователю %.&#x2a;ls не удалось войти в систему. Не удалось изменить пароль. Изменение пароля во время входа в систему не поддерживается в этой версии SQL Server. |
| 40627 |20 |Выполняется операция на сервере {0} и в базе данных {1}. Подождите несколько минут и повторите попытку. |
| 40630 |16 |Ошибка проверки пароля. Пароль не соответствует требованиям политики, так как он слишком короткий. |
| 40631 |16 |Указан слишком длинный пароль. Пароль должен содержать не более 128 символов. |
| 40632 |16 |Ошибка проверки пароля. Пароль не соответствует требованиям политики, так как он недостаточно сложен. |
| 40636 |16 |Нельзя использовать зарезервированное имя базы данных %.&#x2a;ls в этой операции. |
| 40638 |16 |Недопустимый идентификатор подписки (ИД_подписки). Подписка не существует. |
| 40639 |16 |Запрос не соответствует схеме: (ошибка схемы). |
| 40640 |20 |Сервер обнаружил непредвиденное исключение. |
| 40641 |16 |Указано недопустимое расположение. |
| 40642 |17 |Сервер в данный момент занят. Повторите попытку позже. |
| 40643 |16 |Указано недопустимое значение заголовка x-ms-version. |
| 40644 |14 |Не удалось авторизовать доступ к указанной подписки. |
| 40645 |16 |Имя сервера (имя_сервера) не может быть пустым или иметь значение NULL. Оно может состоять из букв нижнего регистра от a до z, чисел от 0 до 9 и дефисов. Дефис не может находиться в начале или в конце имени. |
| 40646 |16 |Идентификатор подписки не может быть пустым. |
| 40647 |16 |Подписка (ИД_подписки) не содержит имя сервера. |
| 40648 |17 |Выполнено слишком много запросов. Повторите попытку позже. |
| 40649 |16 |Указан недопустимый тип содержимого. Поддерживается только тип application/xml. |
| 40650 |16 |Подписка (ИД_подписки) не существует или не готова к работе. |
| 40651 |16 |Не удалось создать сервер, так как подписка (ИД_подписки) отключена. |
| 40652 |16 |Не удается переместить или создать сервер. Подписка (ИД_подписки) превысит квоту сервера. |
| 40671 |17 |Сбой связи между шлюзом и службой управления. Повторите попытку позже. |
| 40852 |16 |Невозможно открыть базу данных "%.\*ls" на сервере "%.\*ls", запрашиваемую с использованием имени для входа. Доступ к базе данных предоставляется только при использовании строки безопасного подключения. Для доступа к этой базе данных измените строки подключения, добавив слово secure в полное доменное имя сервера: &lt;имя_сервера&gt;.database.windows.net нужно изменить на &lt;имя_сервера&gt;.database`secure`.windows.net. |
| 40914 | 16 | Невозможно открыть сервер *[имя-сервера]* , запрашиваемый именем входа. Клиенту запрещен доступ к серверу.<br /><br />Чтобы устранить проблему, попробуйте добавить [правило виртуальной сети](sql-database-vnet-service-endpoint-rule-overview.md). |
| 45168 |16 |Система SQL Azure испытывает нагрузку, в связи с чем ограничивает максимальное количество параллельных операций CRUD с базами данных на одном сервере Базы данных SQL (например, создание базы данных). Сервер, указанный в сообщении об ошибке, превысил максимальное количество одновременных подключений. Повторите попытку позже. |
| 45169 |16 |Система SQL Azure испытывает нагрузку, в связи с чем ограничивает максимальное количество параллельных операций CRUD с серверами в пределах одной подписки (например, создание сервера). Подписка, указанная в сообщении об ошибке, превысила максимальное количество одновременных подключений, и запрос был отклонен. Повторите попытку позже. |

## <a name="next-steps"></a>Дальнейшие действия

* Узнайте о [возможностях Базы данных SQL Azure](sql-database-features.md).
* Узнайте о [модели приобретения на основе единиц DTU](sql-database-service-tiers-dtu.md).
* См. подробнее о [модели приобретения на основе виртуальных ядер](sql-database-service-tiers-vcore.md).

