---
title: Настройка синхронизации данных SQL Azure | Документация Майкрософт
description: В этом руководстве показано, как настроить синхронизацию данных SQL Azure
services: sql-database
ms.service: sql-database
ms.subservice: data-movement
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: allenwux
ms.author: xiwu
ms.reviewer: carlrab
manager: craigg
ms.date: 01/14/2019
ms.openlocfilehash: 82b85ffd685df52e702db15e5a5b57a53a3b4f64
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "60342241"
---
# <a name="tutorial-set-up-sql-data-sync-between-azure-sql-database-and-sql-server-on-premises"></a>Руководство по Настройка синхронизации данных SQL между базой данных SQL Azure и локальным сервером SQL Server

Из этого руководства вы узнаете, как настроить Синхронизацию данных SQL Azure, создав группу синхронизации с экземплярами Базы данных SQL Azure и SQL Server. Группа синхронизации полностью настраивается и синхронизируется по заданному расписанию.

Для выполнения задач этого руководства требуется определенный опыт работы с Базой данных SQL и SQL Server.

Общие сведения см. в статье о [синхронизации данных в нескольких облачных и локальных базах данных с помощью Синхронизации данных SQL](sql-database-sync-data.md).

Примеры команд PowerShell для настройки Синхронизации данных SQL см. в статьях об использовании PowerShell для синхронизации данных [между несколькими базами данных SQL](scripts/sql-database-sync-data-between-sql-databases.md), а также [между базой данных SQL и локальной базой данных SQL Server](scripts/sql-database-sync-data-between-azure-onprem.md).

> [!IMPORTANT]
> Служба "Синхронизация данных SQL Azure" пока **не** поддерживает Управляемый экземпляры Базы данных SQL Azure.

## <a name="create-sync-group"></a>Создание группы синхронизации

1. Откройте в браузере портал Azure. Найдите на панели мониторинга нужную базу данных SQL или выберите значок **Базы данных SQL** на панели инструментов, а затем на странице **Базы данных SQL** выберите базу данных, которой вы решили назначить роль центральной базы данных для синхронизации данных.

    > [!NOTE]
    > Центральная база данных является центральной конечной точкой в топологии синхронизации, в которой размещаются несколько конечных точек баз данных для группы синхронизации. Остальные базы данных, у которых есть конечные точки в этой группе синхронизации, синхронизируются с центральной базой данных.

1. На странице **База данных SQL** для выбранной базы данных щелкните **Синхронизировать с другими базами данных**.

    ![Параметр "Синхронизировать с другими базами данных"](media/sql-database-get-started-sql-data-sync/datasync-overview.png)

1. На странице **Синхронизировать с другими базами данных** выберите **Новая группа синхронизации**. Откроется страница **Новая группа синхронизации**, в которой выделен шаг 1 **Создание группы синхронизации**.

   ![Шаг 1. Параметры](media/sql-database-get-started-sql-data-sync/stepone.png)

   На странице **Создание группы синхронизации данных** измените указанные ниже параметры.

   | Параметр                        | ОПИСАНИЕ |
   | ------------------------------ | ------------------------------------------------- |
   | **Имя группы синхронизации** | Введите имя для новой группы синхронизации. Это имя должно отличаться от имени самой базы данных. |
   | **База данных для метаданных синхронизации** | Выберите создание новой базы данных (рекомендуется) или использование существующей базы данных.<br/><br/>Если вы выбрали вариант **Новая база данных**, щелкните **Создать новую базу данных**. На странице **База данных SQL** задайте имя и параметры для новой базы данных, затем щелкните **ОК**.<br/><br/>Если вы выбрали вариант **Использовать существующую базу данных**, выберите базу данных из списка. |
   | **Автоматическая синхронизация** | Выберите значение **Включено** или **Выключено**.<br/><br/>Если вы выбрали **Включено**, в разделе **Частота синхронизации** введите число и выберите единицы измерения: **секунды**, **минуты**, **часы** или **дни**. |
   | **Устранение конфликтов** | Выберите **Выиграл концентратор** или **Выиграл член**.<br/><br/>Вариант **Выиграл концентратор** означает, что при возникновении конфликта данные из центральной базы данных перезаписывают данные в рядовой базе данных.<br/><br/>Вариант **Выиграл член** означает, что при возникновении конфликта данные из рядовой базы данных перезаписывают данные в центральной базе данных. |

   > [!NOTE]
   > Корпорация Майкрософт рекомендует создать пустую **базу данных для метаданных синхронизации**. Служба синхронизации данных создает таблицы в этой базе данных и часто выполняет рабочую нагрузку. Эта база совместно используется всеми группами синхронизации в выбранном регионе как **база данных для метаданных синхронизации**, и вы не можете заменить эту базу данных или изменить ее имя, не удаляя все группы синхронизации и агенты синхронизации в регионе.

   Нажмите кнопку **ОК** и подождите, пока будет создана и развернута группа синхронизации.

## <a name="add-sync-members"></a>Добавление членов синхронизации

После создания и развертывания группы синхронизации на странице **Новая группа синхронизации** выделяется шаг 2 **Добавление членов синхронизации**.

В разделе **Центральная база данных** введите существующие учетные данные для сервера Базы данных SQL, на котором расположена центральная база данных. Не вводите *новые* учетные данные в этом разделе.

![Шаг 2. Параметры](media/sql-database-get-started-sql-data-sync/steptwo.png)

### <a name="to-add-an-azure-sql-database"></a>Добавление базы данных SQL Azure

В разделе **Рядовая база данных** можно дополнительно добавить в группу синхронизации базу данных SQL Azure, щелкнув **Add an Azure SQL Database** (Добавление базы данных SQL Azure). Откроется страница **Configure Azure SQL Database** (Настройка Базы данных SQL Azure).

  ![Шаг 2. Настройка базы данных](media/sql-database-get-started-sql-data-sync/steptwo-configure.png)

  На странице **Configure Azure SQL Database** (Настройка Базы данных SQL Azure) измените указанные ниже параметры.

  | Параметр                       | ОПИСАНИЕ |
  | ----------------------------- | ------------------------------------------------- |
  | **Имя участника синхронизации** | Укажите имя нового участника синхронизации. Это имя должно отличаться от имени самой базы данных. |
  | **Подписка** | Выберите связанную подписку Azure для выставления счетов. |
  | **Azure SQL Server** | Выберите существующий сервер Базы данных SQL. |
  | **база данных SQL Azure;** | Выберите существующую базу данных SQL. |
  | **Направления синхронизации** | Выберите вариант **Двунаправленная синхронизация**, **К концентратору** или **Из концентратора**. |
  | **Имя пользователя** и **Пароль** | Введите существующие учетные данные для сервера Базы данных SQL, на котором расположена рядовая база данных. Не вводите *новые* учетные данные в этом разделе. |

  Нажмите кнопку **ОК** и подождите, пока будет создан и развернут новый член синхронизации.

<a name="add-on-prem"></a>
### <a name="to-add-an-on-premises-sql-server-database"></a>Добавление локальной базы данных SQL Server

В разделе **Рядовая база данных** можно дополнительно добавить в группу синхронизации локальную базу данных SQL Server, щелкнув **Добавление локальной базы данных**. Откроется страница **Настройка локальной среды**, где вы можете выполнить описанные ниже действия.

1. Щелкните **Выбор шлюза агента синхронизации**. Откроется страница **Выбор агента синхронизации**.

   ![Создание агента синхронизации](media/sql-database-get-started-sql-data-sync/steptwo-agent.png)

1. На странице **Choose the Sync Agent** (Выбор агента синхронизации) укажите, следует ли использовать существующий агент или создать новый.

   Если вы выбрали вариант **Существующие агенты**, выберите агент в списке.

   Если вы выбрали вариант **Создание агента**, сделайте следующее:

   1. Скачайте агент синхронизации данных, воспользовавшись предоставленной ссылкой, и установите его на компьютере, на котором размещен сервер SQL Server. Вы также можете скачать агент непосредственно [на странице агента Синхронизации данных SQL Azure](https://www.microsoft.com/download/details.aspx?id=27693).

      > [!IMPORTANT]
      > Необходимо открыть исходящий TCP-порт 1433 в брандмауэре, чтобы позволить агенту клиента обмениваться данными с сервером.

   1. Введите имя агента.

   1. Выберите **Создание и генерация ключа** и скопируйте ключ агента в буфер обмена

   1. Нажмите кнопку **ОК**, чтобы закрыть страницу **Выбор агента синхронизации**.

1. На компьютере SQL Server найдите и запустите приложение агента клиента синхронизации.

   ![Приложение агента клиента синхронизации данных](media/sql-database-get-started-sql-data-sync/datasync-preview-clientagent.png)

    1. В приложении агента синхронизации выберите **Submit Agent Key** (Отправить ключ агента). Откроется диалоговое окно **Sync Metadata Database Configuration** (Конфигурация базы данных для метаданных синхронизации).

    1. В диалоговом окне **Sync Metadata Database Configuration** (Конфигурация базы данных для метаданных синхронизации) вставьте ключ агента, скопированный на портале Azure. Укажите также существующие учетные данные для сервера Базы данных SQL Azure, на котором расположена база данных для метаданных. (если вы создали базу данных для метаданных, то она находится на том же сервере, что и центральная база данных). Нажмите кнопку **ОК** и дождитесь завершения настройки.

        ![Ввод ключа агента и учетных данных сервера](media/sql-database-get-started-sql-data-sync/datasync-preview-agent-enterkey.png)

        > [!NOTE]
        > Если вы получите сообщение об ошибке брандмауэра, создайте в Azure правило брандмауэра, которое разрешает входящий трафик от компьютера SQL Server. Такое правило можно создать вручную на портале или с помощью SQL Server Management Studio (SSMS). В среде SSMS для подключения к центральной базе данных в Azure введите имя этой базы данных в формате <имя_центральной_базы_данных>.database.windows.net.

    1. Щелкните **Регистрация**, чтобы зарегистрировать базу данных SQL Server в агенте. Откроется диалоговое окно **SQL Server Configuration** (Конфигурация SQL Server).

        ![Добавление и настройка базы данных SQL Server](media/sql-database-get-started-sql-data-sync/datasync-preview-agent-adddb.png)

    1. В диалоговом окне **Конфигурация SQL Server** выберите, следует ли устанавливать подключение с использованием аутентификации SQL Server или аутентификации Windows. Если выбрана аутентификация SQL Server, введите существующие учетные данные. Укажите имя SQL Server и имя базы данных, которую требуется синхронизировать, и щелкните **Тестирование подключения** для проверки настроек. Теперь щелкните **Сохранить**, и зарегистрированная база данных появится в списке.

        ![База данных SQL Server зарегистрирована](media/sql-database-get-started-sql-data-sync/datasync-preview-agent-dbadded.png)

    1. Закройте приложение агента синхронизации клиента.

1. На странице портала **Настройка локальной среды** щелкните **Выбор базы данных**.

1. На странице **Выбор базы данных** в поле **Имя участника синхронизации** укажите имя нового члена синхронизации. Это имя должно отличаться от имени самой базы данных. Выберите базу данных из списка. В поле **Направления синхронизации** выберите значение **Двунаправленная синхронизация**, **К концентратору** или **Из концентратора**.

    ![Выбор локальной базы данных](media/sql-database-get-started-sql-data-sync/datasync-preview-selectdb.png)

1. Нажмите кнопку **ОК**, чтобы закрыть страницу **Выбор базы данных**. Затем нажмите кнопку **ОК**, чтобы закрыть страницу **Настройка локальной среды**, и подождите, пока новый член синхронизации не будет создан и развернут. Наконец, нажмите кнопку **ОК**, чтобы закрыть страницу **Выбор членов синхронизации**.

> [!NOTE]
> Чтобы подключиться к службе синхронизации данных SQL и к локальному агенту, добавьте используемое имя пользователя в роль *DataSync_Executor*. Служба синхронизации данных создает эту роль в экземпляре SQL Server.

## <a name="configure-sync-group"></a>Настройка группы синхронизации

После создания и развертывания участников группы синхронизации на странице **Новая группа синхронизации** выделяется шаг 3, **Настройка группы синхронизации**.

![Шаг 3. Параметры](media/sql-database-get-started-sql-data-sync/stepthree.png)

1. На странице **Таблицы** выберите базу данных из списка участников группы синхронизации, а затем щелкните **Обновить схему**.

1. Выберите в списке таблицы для синхронизации. По умолчанию здесь выбраны все столбцы, поэтому снимите флажки для тех столбцов, которые вам не нужно синхронизировать. Обязательно оставьте выбранным столбец первичного ключа.

1. Щелкните **Сохранить**.

1. По умолчанию базы данных не синхронизируются, пока вы не настроите расписание или не запустите синхронизацию вручную. Чтобы выполнить синхронизацию вручную, перейдите к нужной базе данных SQL на портале Azure, щелкните действие **Синхронизировать с другими базами данных** и выберите группу синхронизации. Откроется страница **Синхронизация данных**. Выберите **Синхронизация**.

    ![Синхронизация вручную](media/sql-database-get-started-sql-data-sync/datasync-sync.png)

## <a name="faq"></a>Часто задаваемые вопросы

**Как часто Синхронизация данных может синхронизировать данные?**

Минимальный интервал времени между запусками синхронизации составляет пять минут.

**Создает ли Синхронизация данных SQL полные таблицы?**

Если в целевой базе данных нет таблиц схемы синхронизации, то Синхронизация данных SQL создает их с выбранным для синхронизации набором столбцов. Но этот механизм не позволяет создать полноценную схему по следующим причинам:

- В целевой таблице создаются только выбранные столбцы. Все невыбранные столбцы игнорируются.
- В таблице назначения создаются индексы только по выделенным столбцам. Для невыбранных столбцов все индексы игнорируются.
- Индексы для столбцов типа XML не создаются.
- Ограничения CHECK не создаются.
- Триггеры из исходных таблиц не создаются.
- Представления и хранимые процедуры не создаются.

Из-за этих ограничений мы рекомендуем применять следующий подход:

- В рабочей среде следует самостоятельно создать полноценную схему.
- Для экспериментов со службой можно применить функцию автоматической подготовки.

**Почему в списке отображаются таблицы, созданные не мной?**

Синхронизация данных создает в базе данных дополнительные таблицы для отслеживания изменений. Не удаляйте их, иначе функция синхронизации данных перестанет работать.

**Гарантируется ли согласованность данных после синхронизации?**

Не обязательно. Для примера рассмотрим группу синхронизации с концентратором и тремя рядовыми базами данных (A, B и C), в которой настроена синхронизация из концентратора в A, из концентратора в B и из концентратора в C. Если в базу данных A внесены изменения *после* синхронизации из концентратора в A, эти изменения не попадут в базы данных B и C до следующей задачи синхронизации.

**Как применить изменения схемы в группе синхронизации?**

Все изменения схемы нужно вносить и распространять вручную.

1. Реплицируйте изменения схемы вручную в центр и все остальные члены синхронизации.
1. Обновление схемы синхронизации.

Добавление новых таблиц или столбцов

Новые таблицы и столбцы не влияют на текущую синхронизацию и Синхронизация данных игнорирует их, пока они не добавлены в схему синхронизации. При добавлении новых объектов базы данных соблюдайте такую последовательность:

1. Добавьте новые таблицы или столбцы в центральной базе данных и на всех участниках синхронизации.
1. Добавьте новые таблицы или столбцы в схему синхронизации.
1. Вносите данные в новые таблицы и столбцы.

Изменение типа данных для столбца

Если изменить тип данных существующего столбца, синхронизация данных продолжает выполняться до тех пор, пока новые значения соответствуют исходному типу данных, определенному в схеме синхронизации. Например, если вы измените в базе данных-источнике тип с **int** на **bigint**, синхронизация данных будет продолжаться, пока на обработку не поступит слишком большое для типа **int** значение. Чтобы завершить изменение, вручную реплицируйте изменения схемы в концентратор и на все участники синхронизации, а затем обновите схему синхронизации.

**Как правильно экспортировать и импортировать базу данных, для которой выполняется синхронизация данных?**

Сначала следует экспортировать базу данных в файл с расширением *.bacpac* и импортировать его для создания новой базы данных. После этого нужно применить синхронизацию к новой базе данных. Для этого выполните описанные ниже действия.

1. Очистите объекты синхронизации данных и все дополнительные таблицы в новой базе данных с помощью [этого скрипта](https://github.com/vitomaz-msft/DataSyncMetadataCleanup/blob/master/Data%20Sync%20complete%20cleanup.sql). Этот скрип удаляет из базы данных все необходимые объекты Синхронизации данных.
1. Повторно создайте группу синхронизации с новой базой данных. Если старая группа синхронизации больше не нужна, удалите ее.

**Где найти сведения об агенте клиента?**

Часто задаваемые вопросы об агенте клиента см. в [этом разделе](sql-database-data-sync-agent.md#agent-faq).

## <a name="next-steps"></a>Дальнейшие действия

Поздравляем! Вы создали группу синхронизации, которая включает и экземпляр базы данных SQL, и базу данных SQL Server.

Дополнительные сведения о синхронизации данных SQL:

- Сведения об [агенте синхронизации данных для синхронизации данных SQL Azure](sql-database-data-sync-agent.md).
- [Рекомендации по синхронизации данных SQL](sql-database-best-practices-data-sync.md) и инструкции по [устранению неполадок с синхронизацией данных SQL](sql-database-troubleshoot-data-sync.md).
- [Мониторинг синхронизации данных SQL с помощью журналов Azure Monitor](sql-database-sync-monitor-oms.md)
- Сведения об обновлении схемы синхронизации с помощью [Transact-SQL](sql-database-update-sync-schema.md) или [PowerShell](scripts/sql-database-sync-update-schema.md).

Дополнительные сведения о Базе данных SQL:

- [Обзор Базы данных SQL](sql-database-technical-overview.md)
- [Управление жизненным циклом базы данных](https://msdn.microsoft.com/library/jj907294.aspx)
