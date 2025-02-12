---
title: Распределенные транзакции по облачным базам данных
description: Общие сведения о транзакциях эластичной базы данных в Базе данных SQL Azure
services: sql-database
ms.service: sql-database
ms.subservice: scale-out
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: ''
manager: craigg
ms.date: 03/12/2019
ms.openlocfilehash: 7fc945a00360e4eea4a5cf1e3962da2390d3bfeb
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "65506848"
---
# <a name="distributed-transactions-across-cloud-databases"></a>Распределенные транзакции по облачным базам данных

Возможности эластичной базы данных позволяют выполнять транзакции, охватывающие несколько баз данных в Базе данных SQL Azure. Транзакции эластичной базы данных в базе данных SQL можно выполнять с помощью ADO.NET и приложений .NET. Кроме того, транзакции можно объединить с уже знакомыми приемами программирования, используя классы [System.Transaction](https://msdn.microsoft.com/library/system.transactions.aspx). Инструкции по подключению библиотеки см. в статье [Microsoft .NET Framework 4.6.1 (веб-установщик) для Windows 7 SP1, Windows 8, Windows 8.1, Windows 10, Windows Server 2008 R2 SP1, Windows Server 2012 и Windows Server 2012 R2](https://www.microsoft.com/download/details.aspx?id=49981).

При локальном выполнении такого сценария, как правило, требуется использовать координатор распределенных транзакций Майкрософт (MSDTC). Так как MSDTC нельзя использовать с приложениями платформы как услуги в Azure, возможность координировать распределенные транзакции теперь интегрирована в саму базу данных SQL. Чтобы запустить распределенные транзакции, приложения можно подключить к любой базе данных SQL. При этом одна из баз данных будет отвечать за прозрачную координацию распределенных транзакций, как показано на следующем рисунке. 

  ![Осуществление распределенных транзакций в Базе данных SQL Azure с использованием транзакций эластичной базы данных ][1]

## <a name="common-scenarios"></a>Распространенные сценарии

Транзакции эластичной базы данных в базе данных SQL позволяют вносить атомарные изменения в данные, хранящиеся в нескольких базах данных SQL, с помощью приложений. Эта предварительная версия ориентирована на клиентскую разработку на языке C# с использованием платформы .NET. В будущем планируется предоставить возможность такой работы на сервере с использованием T-SQL.  
Транзакции эластичной базы данных применяются в следующих сценариях.

* Приложения в Azure, использующие несколько баз данных. Этот сценарий предусматривает вертикальное секционирование данных между несколькими базами данных в базе данных SQL. В результате разные виды данных находятся в разных базах данных. При выполнении некоторых операций необходимо вносить изменения в данные, которые хранятся в нескольких базах данных. Приложение использует транзакции эластичной базы данных для скоординированного внесения изменений в базах данных и обеспечения атомарности.
* Приложения в Azure, использующие сегментированные базы данных. Этот сценарий предусматривает использование [клиентской библиотеки эластичной базы данных](sql-database-elastic-database-client-library.md) или выполнения самостоятельного сегментирования для горизонтального разделения данных между несколькими базами данных в базе данных SQL. Первый пример использования заключается в необходимости внести атомарные изменения в сегментированное мультитенантное приложение, если изменения распространяются на клиенты. Например, рассмотрим передачу данных из одного клиента в другой, при этом оба клиента находятся в разных базах данных. Второй пример предполагает точное сегментирование в целях соблюдения требований к емкости для клиентов большого размера. В свою очередь, это обычно означает, что некоторые атомарные операции необходимо выполнять в нескольких базах данных, которые использует один клиент. Третий пример предусматривает атомарное обновление, обеспечивающее возможность ссылаться на данные, реплицируемые в базах данных. Теперь благодаря предварительной версии стала возможна координация атомарных транзакционных операций со всеми этими строками в нескольких базах данных.
  Транзакции эластичной базы данных используют двухфазную фиксацию для обеспечения атомарности транзакций в базах данных. Этот вариант прекрасно подходит для тех случаев, когда в одной транзакции одновременно участвует менее 100 баз данных. Эти ограничения не применяются принудительно, но следует учитывать, что их превышение отрицательно влияет на производительность и успешность выполнения транзакций эластичной базы данных.

## <a name="installation-and-migration"></a>Установка и миграция

Возможности для транзакций эластичной базы данных в базе данных SQL предоставляются с обновлениями библиотек .NET — System.Data.dll и System.Transactions.dll. Библиотеки DLL обеспечивают использование двухфазной фиксации для соблюдения атомарности. Чтобы начать разработку приложений, использующих транзакции эластичной базы данных, установите платформу [.NET 4.6.1](https://www.microsoft.com/download/details.aspx?id=49981) или более поздней версии. При использовании платформы .NET предыдущей версии происходит ошибка повышения уровня транзакций до распределенных, после чего возникает исключение.

После установки можно использовать API распределенных транзакций в классах System.Transactions с подключением к базе данных SQL. При наличии приложений MSDTC, использующих эти API, после установки платформы 4.6.1 просто повторно создайте существующие приложения .NET 4.6. Если ваши проекты предназначены для использования на платформе .NET 4.6, обновленные библиотеки DLL новой версии платформы будут использоваться автоматически, а вызовы API распределенных транзакций и подключения к базе данных SQL будут успешными.

Учтите, что для выполнения транзакций эластичной базы данных не требуется установка MSDTC. Вместо этого управление такими транзакциями осуществляется непосредственно в базе данных SQL. Это значительно упрощает сценарии, в которых используется облако, так как для использования распределенных транзакций в базе данных SQL не нужно развертывать MSDTC. В разделе 4 более подробно описано развертывание транзакций эластичной базы данных и требуемой платформы .NET вместе с облачными приложениями в Azure.

## <a name="development-experience"></a>Разработка

### <a name="multi-database-applications"></a>Приложения, использующие несколько баз данных

В следующем примере кода знакомые приемы программирования используются в сочетании с System.Transactions .NET. Класс TransactionScope позволяет выполнить внешнюю транзакцию в .NET. («Внешняя транзакция» — это транзакция, которая происходит в текущем потоке.) В транзакции участвуют все подключения, установленные в TransactionScope. Если в транзакции задействовано несколько разных баз данных, ее уровень автоматически повышается и она стает распределенной. Для этого необходимо задать в настройках области ее завершение, что позволяет обозначить выполнение фиксации.

    using (var scope = new TransactionScope())
    {
        using (var conn1 = new SqlConnection(connStrDb1))
        {
            conn1.Open();
            SqlCommand cmd1 = conn1.CreateCommand();
            cmd1.CommandText = string.Format("insert into T1 values(1)");
            cmd1.ExecuteNonQuery();
        }

        using (var conn2 = new SqlConnection(connStrDb2))
        {
            conn2.Open();
            var cmd2 = conn2.CreateCommand();
            cmd2.CommandText = string.Format("insert into T2 values(2)");
            cmd2.ExecuteNonQuery();
        }

        scope.Complete();
    }

### <a name="sharded-database-applications"></a>Приложения, использующие сегментированные базы данных

Транзакции эластичной базы данных в базе данных SQL также поддерживают координацию распределенных транзакций. При этом для установки подключений к развернутому уровню данных используется метод OpenConnectionForKey клиентской библиотеки эластичной базы данных. Рассмотрим случаи, когда необходимо обеспечить согласованность изменений транзакций для нескольких разных значений ключей сегментирования. Подключения к сегментам с разными значениями ключей сегментирования устанавливаются через посредника с использованием OpenConnectionForKey. В большинстве случаев устанавливаются подключения к разным сегментам. Поэтому для осуществления транзакций требуется выполнение распределенной транзакции. Этот подход показан в следующем примере кода. В нем предполагается использование переменной shardmap для представления карты сегментов в клиентской библиотеке эластичной базы данных:

    using (var scope = new TransactionScope())
    {
        using (var conn1 = shardmap.OpenConnectionForKey(tenantId1, credentialsStr))
        {
            conn1.Open();
            SqlCommand cmd1 = conn1.CreateCommand();
            cmd1.CommandText = string.Format("insert into T1 values(1)");
            cmd1.ExecuteNonQuery();
        }

        using (var conn2 = shardmap.OpenConnectionForKey(tenantId2, credentialsStr))
        {
            conn2.Open();
            var cmd2 = conn2.CreateCommand();
            cmd2.CommandText = string.Format("insert into T1 values(2)");
            cmd2.ExecuteNonQuery();
        }

        scope.Complete();
    }


## <a name="net-installation-for-azure-cloud-services"></a>Установка .NET для облачных служб Azure

Azure включает несколько предложений для размещения приложений .NET. Сравнение различных предложений приведено в статье [Сравнение службы приложений, облачных служб и виртуальных машин Azure](/azure/architecture/guide/technology-choices/compute-decision-tree). Если версия гостевой операционной системы предложения меньше .NET 4.6.1 (версия, необходимая для эластичных транзакций), необходимо обновить гостевую ОС до версии 4.6.1. 

Для службы приложений Azure обновления гостевой ОС в настоящее время не поддерживаются. В виртуальных машинах Azure необходимо просто выполнить вход и запустить установщик для последней версии .NET Framework. Для облачных служб Azure необходимо включить установку более новой версии .NET в задачи запуска данного развертывания. Основные понятия и рекомендованные шаги см. в статье [Установка .NET для роли облачной службы](../cloud-services/cloud-services-dotnet-install-dotnet.md).  

Обратите внимание на то, что в процессе начальной загрузки в облачных службах Azure для установки .NET 4.6.1 может потребоваться больше объема временного хранилища, чем для установки .NET 4.6. Для успешной установки необходимо увеличить временное хранилище для облачной службы Azure в разделе LocalResources файла ServiceDefinition.csdef и в параметрах среды для задачи запуска, как показано в следующем примере:

    <LocalResources>
    ...
        <LocalStorage name="TEMP" sizeInMB="5000" cleanOnRoleRecycle="false" />
        <LocalStorage name="TMP" sizeInMB="5000" cleanOnRoleRecycle="false" />
    </LocalResources>
    <Startup>
        <Task commandLine="install.cmd" executionContext="elevated" taskType="simple">
            <Environment>
        ...
                <Variable name="TEMP">
                    <RoleInstanceValue xpath="/RoleEnvironment/CurrentInstance/LocalResources/LocalResource[@name='TEMP']/@path" />
                </Variable>
                <Variable name="TMP">
                    <RoleInstanceValue xpath="/RoleEnvironment/CurrentInstance/LocalResources/LocalResource[@name='TMP']/@path" />
                </Variable>
            </Environment>
        </Task>
    </Startup>

## <a name="transactions-across-multiple-servers"></a>Транзакции между несколькими серверами

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]
> [!IMPORTANT]
> Модуль PowerShell Azure Resource Manager по-прежнему поддерживается базой данных SQL Azure, но все будущие разработки — для модуля Az.Sql. Для этих командлетов см. в разделе [AzureRM.Sql](https://docs.microsoft.com/powershell/module/AzureRM.Sql/). Аргументы для команд в модуле Az и в модуле AzureRm практически идентичны.

Транзакции эластичной базы данных поддерживаются для разных серверов Базе данных SQL Azure. Если транзакции пересекают границы сервера Базы данных SQL, для серверов-участников сначала необходимо настроить отношение взаимного обмена данными. После установления отношения взаимодействия любая база данных на любом из двух серверов может участвовать в эластичных транзакциях с базами данных другого сервера. Если транзакции охватывают более двух логических серверов, отношение взаимодействия необходимо настроить для каждой пары серверов Базы данных SQL.

Используйте следующие командлеты PowerShell для управления отношениями взаимодействия между серверами для транзакций эластичной базы данных:

* **New-AzSqlServerCommunicationLink**: Используйте этот командлет для создания отношения взаимодействия между двумя серверами Базы данных SQL Azure. Отношение симметрично, то есть оба сервера могут инициировать транзакции с другим сервером.
* **Get-AzSqlServerCommunicationLink**: Служит для получения существующих отношений взаимодействия и их свойств.
* **Remove-AzSqlServerCommunicationLink**: Служит для удаления существующих отношений взаимодействия. 

## <a name="monitoring-transaction-status"></a>Мониторинг состояния транзакций

Используйте динамические административные представления для мониторинга состояния и хода выполнения текущих транзакций эластичной базы данных в базе данных SQL. Все представления, связанные с транзакциями, соответствуют распределенным транзакциям в базе данных SQL. Соответствующий список динамических административных представлений см. здесь: [Динамические административные представления и функции, связанные с транзакциями (Transact-SQL)](https://msdn.microsoft.com/library/ms178621.aspx).

Особенно полезны следующие динамические административные представления.

* **sys.dm\_tran\_active\_transactions**. Позволяет просматривать список активных транзакций и их состояние. В столбце UOW (единица работы) можно просмотреть различные дочерние транзакции, которые относятся к одной распределенной транзакции. У всех транзакций в рамках одной распределенной транзакции одинаковое значение UOW. Дополнительные сведения см. в [документации по динамическим административным представлениям](https://msdn.microsoft.com/library/ms174302.aspx).
* **sys.dm\_tran\_database\_transactions**. Позволяет просматривать дополнительные сведения о транзакциях, включая расположение транзакции в журнале. Дополнительные сведения см. в [документации по динамическим административным представлениям](https://msdn.microsoft.com/library/ms186957.aspx).
* **sys.dm\_tran\_locks**. Предоставляет сведения о текущих блокировках транзакций. Дополнительные сведения см. в [документации по динамическим административным представлениям](https://msdn.microsoft.com/library/ms190345.aspx).

## <a name="limitations"></a>Ограничения

В настоящее время в отношении транзакций эластичной базы данных в базе данных SQL действует ряд ограничений.

* Поддерживаются только транзакции, выполняемые между базами данных в базе данных SQL. В них не могут участвовать поставщики ресурсов и базы данных [X или Open XA](https://en.wikipedia.org/wiki/X/Open_XA) , которые не принадлежат к базе данных SQL. Это означает, что транзакции эластичной базы данных не распространяются на локальные базы данных SQL Server и SQL Azure. Чтобы осуществлять распределенные транзакции локально, нужно использовать MSDTC. 
* Поддерживаются только транзакции, которые осуществляются с помощью приложения .NET и которые координирует клиент. Сейчас выполнение запросов T-SQL на серверах (например, BEGIN DISTRIBUTED TRANSACTION) не поддерживается, но в будущем мы планируем добавить эту возможность. 
* Транзакции между службами WCF не поддерживаются. Например, если у вас есть метод службы WCF, выполняющий транзакцию, включение вызова в область транзакции завершится ошибкой с исключением [System.ServiceModel.ProtocolException](https://msdn.microsoft.com/library/system.servicemodel.protocolexception).

## <a name="next-steps"></a>Дальнейшие действия

Все возникшие вопросы задавайте на [форуме по базам данных SQL](https://social.msdn.microsoft.com/forums/azure/home?forum=ssdsgetstarted), а запросы новых функций оставляйте на [форуме отзывов и предложений по базам данных SQL](https://feedback.azure.com/forums/217321-sql-database/).

<!--Image references-->
[1]: ./media/sql-database-elastic-transactions-overview/distributed-transactions.png
