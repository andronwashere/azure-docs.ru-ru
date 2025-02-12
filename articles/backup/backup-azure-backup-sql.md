---
title: Резервное копирование рабочих нагрузок SQL Server в Azure с помощью DPM
description: Общие сведения о резервном копировании баз данных SQL Server с помощью службы архивации Azure
author: kasinh
manager: vvithal
ms.service: backup
ms.topic: conceptual
ms.date: 01/30/2019
ms.author: kasinh
ms.openlocfilehash: 60afba5d094d730aab20f2a7d50206c063f390ff
ms.sourcegitcommit: c72ddb56b5657b2adeb3c4608c3d4c56e3421f2c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/24/2019
ms.locfileid: "68466628"
---
# <a name="back-up-sql-server-to-azure-as-a-dpm-workload"></a>Архивация SQL Server в Azure с помощью рабочей нагрузки DPM
В этой статье описывается процесс настройки архивации баз данных SQL Server с помощью службы архивации Azure.

Чтобы создать резервные копии баз данных SQL Server в Azure, необходима учетная запись Azure. Если ее нет, можно создать бесплатную пробную учетную запись всего за несколько минут. Дополнительные сведения см. в разделе [Бесплатная пробная версия Azure](https://azure.microsoft.com/pricing/free-trial/).

Процесс управления резервным копированием и восстановлением базы данных SQL Server в Azure состоит из трех этапов.

1. Создание политики резервного копирования для защиты баз данных SQL Server в Azure.
2. Создание резервных копий в Azure по запросу.
3. Восстановление базы данных из Azure.

## <a name="before-you-start"></a>Перед началом
Перед началом работы выполните все [предварительные требования](backup-azure-dpm-introduction.md#prerequisites-and-limitations) по использованию службы Microsoft Azure Backup для защиты рабочих нагрузок. Предварительные требования включают в себя такие задачи, как создание хранилища резервных копий, скачивание учетных данных хранилища, установка агента службы архивации Azure и регистрация сервера в хранилище.

## <a name="create-a-backup-policy-to-protect-sql-server-databases-to-azure"></a>Создание политики резервного копирования для защиты баз данных SQL Server в Azure
1. На странице сервера DPM щелкните рабочую область **Защита** .
2. На ленте инструментов щелкните **Создать** , чтобы создать новую группу защиты.

    ![Создайте группу защиты](./media/backup-azure-backup-sql/protection-group.png)
3. Появится начальный экран DPM с рекомендациями по созданию **группы защиты**. Нажмите кнопку **Далее**.
4. Выберите **Серверы**.

    ![Выбор типа для группы защиты — «Серверы»](./media/backup-azure-backup-sql/pg-servers.png)
5. Разверните компьютер SQL Server, где находятся базы данных, для которых будут создаваться резервные копии. В окне DPM отобразятся различные источники данных, для которых на этом сервере можно создать резервные копии. Разверните пункт **Все общие ресурсы SQL** и выберите базы данных для резервного копирования (в данном случае мы выбрали ReportServer$MSDPM2012 и ReportServer$MSDPM2012TempDB). Нажмите кнопку **Далее**.

    ![Выбор базы данных SQL](./media/backup-azure-backup-sql/pg-databases.png)
6. Укажите имя группы защиты и установите флажок **Мне нужна оперативная защита** .

    ![Метод защиты данных — краткосрочная на диске и оперативная в Azure](./media/backup-azure-backup-sql/pg-name.png)
7. На экране **Выбор краткосрочных целей** введите необходимые данные, чтобы создать точки резервного копирования на диске.

    Здесь мы видим, что для параметра **Диапазон хранения** выбрано значение *5 дней*, а для параметра **Частота синхронизации** — *15 минут* (это периодичность, с которой создаются резервные копии). **Быстрая полная архивация** задано значение *20:00*.

    ![Краткосрочные цели](./media/backup-azure-backup-sql/pg-shortterm.png)

   > [!NOTE]
   > Каждый день в 20:00 (в соответствии с данными на экране) создается точка архивации, то есть передаются данные, которые были изменены с момента предыдущей архивации в 20:00. Этот процесс называется **быстрой полной архивацией**. Журналы транзакций синхронизируются каждые 15 минут. Если базу данных нужно восстановить в 21:00, точка создается путем воспроизведения журналов из последней точки быстрой полной архивации (в данном случае 20:00).
   >
   >

8. Щелкните **Далее**

    В DPM отобразится общий доступный объем памяти и сведения о потенциальном использовании дискового пространства.

    ![Выделение места на диске](./media/backup-azure-backup-sql/pg-storage.png)

    По умолчанию DPM создает один том на каждый источник данных (базу данных SQL), используемый для создания начальной резервной копии. При таком подходе диспетчер логических дисков (LDM) ограничивает DPM для защиты максимум 300 источников данных (баз данных SQL). Чтобы обойти это ограничение, выберите параметр **Выравнивать данные в пуле носителей DPM**. Если выбран параметр, то DPM использует один том для нескольких источников данных, что позволяет защищать до 2000 баз данных SQL.

    Если выбран параметр **Автоматически увеличивать размер томов**, то DPM может увеличивать размер томов с резервными копиями по мере увеличения объема рабочих данных. Если параметр **Автоматически увеличивать размер томов** не выбран, то DPM ограничит объем хранилища резервных копий, используемого для архивации источников данных в группе защиты.
9. Администраторы могут передать начальную резервную копию либо вне сети (чтобы не перегружать полосу пропускания), либо по сети. Также они могут настроить время для передачи начальной резервной копии. Нажмите кнопку **Далее**.

    ![Метод первичной репликации](./media/backup-azure-backup-sql/pg-manual.png)

    Для создания начальной резервной копии необходимо полностью перенести источник данных (базу данных SQL Server) с производственного сервера (компьютера SQL Server) на сервер DPM. Объем этих данных может быть велик, и их передача по сети может превысить пропускную способность. По этой причине администраторы могут выбрать передачу начальной резервной копии **вручную** (с помощью съемного носителя), чтобы избежать перегрузки пропускной способности сети, или **автоматически по сети** (в указанное время).

    После создания начальной резервной копии на ее основе будет выполняться добавочная архивация. Обычно добавочные резервные копии имеют небольшой размер и могут быть легко переданы по сети.
10. Выберите время для выполнения проверки согласованности и нажмите кнопку **Далее**.

    ![Проверка согласованности](./media/backup-azure-backup-sql/pg-consistent.png)

    DPM может проверять целостность точки резервного копирования (проверка согласованности). Для этого вычисляется контрольная сумма файла резервной копии на рабочем сервере (в этом сценарии используется SQL Server) и резервной копии данных для этого файла в DPM. В случае конфликта предполагается, что резервная копия файла в DPM повреждена. DPM исправляет резервные копии данных, отправляя блоки, соответствующие расхождению в контрольной сумме. Так как проверка согласованности является ресурсоемкой операцией, администраторам предоставляется возможность запланировать ее на определенное время или настроить ее автоматическое выполнение.
11. Чтобы настроить оперативную защиту источников данных, выберите базы данных для копирования в Azure и нажмите кнопку **Далее**.

    ![Выбор источников данных](./media/backup-azure-backup-sql/pg-sqldatabases.png)
12. Администраторы могут выбирать расписания резервного копирования и политики хранения в соответствии с политиками своей организации.

    ![Планирование и хранение](./media/backup-azure-backup-sql/pg-schedule.png)

    В этом примере резервные копии создаются дважды в день — в 12:00 и в 20:00 (в нижней части экрана).

    > [!NOTE]
    > Чтобы обеспечить быстрое восстановление, рекомендуется создать на диске несколько краткосрочных точек восстановления. Эти точки восстановления используются для "оперативного восстановления". Azure — это удобное внешнее расположение с более высоким уровнем обслуживания и гарантированной доступностью.
    >
    >

    **Рекомендация**. Выполнив резервное копирование локального диска с помощью DPM, создайте расписание резервного копирования в Azure. Таким образом в Azure будет передаваться последняя резервная копия диска.

13. Выберите расписание для политики хранения. Дополнительные сведения о том, как работает политика хранения, см. в статье [Использование службы архивации Azure для замены ленточной инфраструктуры](backup-azure-backup-cloud-as-tape.md).

    ![Политика хранения](./media/backup-azure-backup-sql/pg-retentionschedule.png)

    В данном примере:

    * Резервные копии создаются дважды в день, в 12:00 и в 20:00 (в нижней части экрана), и хранятся в течение 180 дней.
    * Резервная копия, создаваемая в субботу в 12:00, хранится 104 недели.
    * Резервная копия, создаваемая в последнюю субботу в 12:00, хранится 60 месяцев.
    * Резервная копия, создаваемая в последнюю субботу марта в 12:00, хранится 10 лет.
14. Щелкните **Далее** и выберите нужный параметр для передачи начальной резервной копии в Azure. Доступны параметры **Автоматически по сети** или **Автономная архивация**.

    * **автоматически по сети** , то резервные копии данных будут передаваться в Azure по выбранному расписанию архивации.
    * Принцип выполнения **автономной архивации** описан в статье [Автономное резервное копирование в службе архивации Azure](backup-azure-backup-import-export.md).

    Выберите способ передачи начальной резервной копии в Azure и нажмите кнопку **Далее**.
15. Просмотрев сведения о политике в окне **Сводка**, нажмите кнопку **Создать группу**, чтобы завершить процесс. После нажатия кнопки **Закрыть** вы сможете отслеживать ход выполнения задания в рабочей области "Мониторинг".

    ![Создание группы защиты: ход выполнения](./media/backup-azure-backup-sql/pg-summary.png)

## <a name="on-demand-backup-of-a-sql-server-database"></a>Архивация базы данных SQL Server по запросу
С помощью описанных выше шагов мы создали политику резервного копирования, однако «точка восстановления» создается только при первом резервном копировании. Можно не ждать создания точки восстановления с помощью планировщика, а создать ее вручную, выполнив описанные ниже действия.

1. Прежде чем создавать точку восстановления, подождите, пока в группе защиты для базы данных отобразится состояние **ОК** .

    ![Члены группы защиты](./media/backup-azure-backup-sql/sqlbackup-recoverypoint.png)
2. Щелкните базу данных правой кнопкой мыши и выберите **Создать точку восстановления**.

    ![Создание точки оперативного восстановления](./media/backup-azure-backup-sql/sqlbackup-createrp.png)
3. Выберите в раскрывающемся меню пункт **Оперативная защита** и нажмите кнопку **ОК**. Начнется создание точки восстановления в Azure.

    ![Создать точку восстановления](./media/backup-azure-backup-sql/sqlbackup-azure.png)
4. Ход выполнения задания можно просматривать в рабочей области **Мониторинг** , где вы увидите выполняющееся задание, как на рисунке ниже.

    ![Консоль наблюдения](./media/backup-azure-backup-sql/sqlbackup-monitoring.png)

## <a name="recover-a-sql-server-database-from-azure"></a>Восстановление базы данных SQL Server из Azure
Чтобы восстановить защищенный объект (базу данных SQL Server) из Azure, необходимо выполнить следующие шаги.

1. Откройте консоль управления сервером DPM. Перейдите в рабочую область **Восстановление** , где вы увидите серверы, для которых созданы резервные копии с помощью DPM. Найдите нужную базу данных (в данном случае ReportServer$MSDPM2012). Для параметра **Восстановить из** выберите значение времени, заканчивающееся словом **Online**.

    ![Выбор точки восстановления](./media/backup-azure-backup-sql/sqlbackup-restorepoint.png)
2. Щелкните имя базы данных правой кнопкой мыши и выберите **Восстановить**.

    ![Восстановление из Azure](./media/backup-azure-backup-sql/sqlbackup-recover.png)
3. DPM отобразит сведения о точке восстановления. Нажмите кнопку **Далее**. Чтобы перезаписать базу данных, выберите тип восстановления **Восстановить в исходном экземпляре SQL Server**. Нажмите кнопку **Далее**.

    ![Восстановление в исходном расположении](./media/backup-azure-backup-sql/sqlbackup-recoveroriginal.png)

    В этом примере DPM позволяет восстановить базу данных в другом экземпляре SQL Server или в отдельной сетевой папке.
4. В окне **Указание параметров восстановления** можно выбрать параметр "Регулирование использования полосы пропускания сети", чтобы регулировать полосу пропускания, используемую при восстановлении. Нажмите кнопку **Далее**.
5. В окне **Сводка** вы увидите все конфигурации восстановления, доступные на данный момент. Щелкните **Восстановить**.

    В окне отобразится состояние восстановления, показывающее, что выполняется восстановление базы данных. Вы можете закрыть мастер кнопкой **Закрыть** и отслеживать ход выполнения в рабочей области **Мониторинг**.

    ![Запуск процесса восстановления](./media/backup-azure-backup-sql/sqlbackup-recoverying.png)

    По завершении процесса восстановленная база данных будет согласована с приложением.

### <a name="next-steps"></a>Дальнейшие действия
•    [Служба архивации Azure: часто задаваемые вопросы](backup-azure-backup-faq.md)
