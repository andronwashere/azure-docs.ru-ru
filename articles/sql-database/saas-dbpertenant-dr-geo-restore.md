---
title: Приложения SaaS. Геоизбыточные резервные копии базы данных SQL Azure для аварийного восстановления | Документация Майкрософт
description: Узнайте, как использовать геоизбыточные резервные копии базы данных SQL Azure для восстановления мультитенантных приложений SaaS в случае сбоя
services: sql-database
ms.service: sql-database
ms.subservice: scenario
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: AyoOlubeko
ms.author: craigg
ms.reviewer: sstein
manager: craigg
ms.date: 01/14/2019
ms.openlocfilehash: d6977f9f957aba2c01265f750f82847e16d299ee
ms.sourcegitcommit: de47a27defce58b10ef998e8991a2294175d2098
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/15/2019
ms.locfileid: "67872036"
---
# <a name="use-geo-restore-to-recover-a-multitenant-saas-application-from-database-backups"></a>Использование геовосстановления для восстановления мультитенантного приложения SaaS из резервных копий базы данных

В этом руководстве описан полный сценарий аварийного восстановления для мультитенантных приложений SaaS, использующих модель однотенантных баз данных. С помощью [геовосстановления](sql-database-recovery-using-backups.md) вы восстановите базы данных каталогов и клиентов из автоматически сохраняемых геоизбыточных резервных копий в альтернативный регион восстановления. После устранения сбоя вы вернете измененные базы данных в исходный регион с помощью [георепликации](sql-database-geo-replication-overview.md).

![Архитектура геовосстановления](media/saas-dbpertenant-dr-geo-restore/geo-restore-architecture.png)

Геовосстановление — это наименее затратное решение для аварийного восстановления в Базе данных SQL Azure. Но восстановление из геоизбыточных резервных копий может приводить к потере данных за период длительностью до одного часа. Также оно может занимать много времени в зависимости от размера баз данных. 

> [!NOTE]
> Чтобы добиться минимальных значений RPO и RTO при восстановлении приложений, используйте георепликацию вместо геовосстановления.

В этом руководстве рассматриваются рабочие процессы восстановления и репатриации. Вы узнаете, как выполнять следующие задачи:
> [!div class="checklist"]
> 
> * синхронизация сведений о конфигурации базы данных и эластичного пула с каталогом клиента;
> * настройка зеркальной копии среды в регионе восстановления, включая приложение, серверы и пулы;   
> * восстановление баз данных каталога и клиента с помощью геовосстановления;
> * возвращение измененных баз данных каталога и клиента в исходное расположение после устранения сбоя с помощью георепликации;
> * обновление каталога после восстановления (возврата) каждой базы данных, чтобы отслеживать текущее расположение активных копий баз данных для каждого клиента;
> * снижение задержки благодаря тому, что приложение и база данных клиента постоянно размещаются в одном регионе Azure. 
 

Перед началом работы с этим руководством выполните следующие обязательные действия:
* Разверните приложение SaaS Wingtip Tickets с однотенантной базой данных. Изучите инструкции из руководства по быстрому [развертыванию приложения SaaS Wingtip Tickets, использующего однотенантную базу данных](saas-dbpertenant-get-started-deploy.md). 
* Установите Azure PowerShell. Дополнительные сведения см. в статье [Начало работы с Azure PowerShell](https://docs.microsoft.com/powershell/azure/get-started-azureps).

## <a name="introduction-to-the-geo-restore-recovery-pattern"></a>Введение в шаблон восстановления с использованием шаблона геовосстановления

Аварийное восстановление является важным аспектом для многих приложений с точки зрения соответствия требованиям или непрерывности бизнес-процессов. Если сбой службы продлится долго, хорошо подготовленный план аварийного восстановления сведет к минимуму нарушение бизнес-процессов. План аварийного восстановления на основе геовосстановления должен предусматривать несколько целей:
 * максимально быстро резервировать все необходимые ресурсы в выбранном регионе восстановления, чтобы гарантировать их доступность для восстановления баз данных клиентов;
 * создавать зеркальную среду восстановления, которая полностью соответствует исходной конфигурации пула и базы данных; 
 * допускать отмену незавершенного процесса восстановления, если исходный регион становится работоспособным;
 * обеспечивать оперативную подготовку клиента, чтобы как можно быстрее восстановить подключение новых клиентов;
 * обеспечивать восстановление клиентов в порядке приоритета;
 * гарантировать быстрое восстановление работоспособности клиентов за счет параллельного выполнения всех процессов, насколько это возможно;
 * обеспечивать идемпотентность и устойчивость к отказам и перезапускам;
 * После устранения сбоя выполнять репатриацию баз данных в их исходный регион с минимальными последствиями для клиентов.  

> [!NOTE]
> Приложение восстанавливается в регионе, который является парным для того региона, в котором оно развернуто. Дополнительные сведения см. в статье [Непрерывность бизнес-процессов и аварийное восстановление в службах BizTalk: пары регионов Azure](https://docs.microsoft.com/azure/best-practices-availability-paired-regions).   

В этом руководстве такие задачи решаются с применением следующих возможностей Базы данных SQL Azure и платформы Azure.

* [Шаблоны Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-create-first-template) для максимально быстрого резервирования необходимой емкости. Шаблоны Azure Resource Manager используются для подготовки зеркального образа исходных серверов и эластичных пулов в регионе восстановления. Для подготовки новых клиентов также создаются отдельные сервер и пул.
* [Клиентская библиотека эластичной базы данных](sql-database-elastic-database-client-library.md) (EDCL) — создание и обслуживание каталога клиентской базы данных. Расширенный каталог содержит периодически обновляемые сведения о конфигурации пула и базы данных.
* [Функции восстановления управления сегментами](sql-database-elastic-database-recovery-manager.md) EDCL — для сохранения записей расположения базы данных в каталоге во время восстановления и возвращения в исходное расположение.  
* [Геовосстановление](sql-database-disaster-recovery.md) — восстановление баз данных каталогов и клиентов из автоматически сохраняемых геоизбыточных резервных копий. 
* [Асинхронные операции восстановления](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-async-operations) создаются в порядке приоритета клиентов, упорядочиваются в отдельную очередь для каждого пула и обрабатываются в пакетном режиме, чтобы не перегружать пул. Эти операции при необходимости можно отменить до или во время выполнения.   
* [Георепликация](sql-database-geo-replication-overview.md) — репатриация баз данных в исходную область после сбоя. Георепликация гарантирует отсутствие потерь данных и минимальное влияние на клиента.
* [DNS-псевдонимы сервера SQL](dns-alias-overview.md) позволяют процессу синхронизации каталога подключаться к активному каталогу независимо от его расположения.  

## <a name="get-the-disaster-recovery-scripts"></a>Получение скриптов аварийного восстановления

Скрипты восстановления, используемые в этом руководстве, доступны в [репозитории WingtipTicketsSaaS-DbPerTenant на сайте GitHub](https://github.com/Microsoft/WingtipTicketsSaaS-DbPerTenant). Ознакомьтесь с инструкциями по скачиванию и разблокированию скриптов управления Wingtip Tickets в статье [Общие рекомендации по работе с примерами приложений SaaS Wingtip Tickets](saas-tenancy-wingtip-app-guidance-tips.md).

> [!IMPORTANT]
> Как и все скрипты управления Wingtip Tickets, скрипты аварийного восстановления являются образцом качества и не должны использоваться в рабочей среде.

## <a name="review-the-healthy-state-of-the-application"></a>Проверка состояния работоспособности приложения
Перед тем как начать процесс восстановления, проверьте состояние работоспособности приложения.

1. В веб-браузере откройте концентратор событий Wingtip Tickets по адресу http://events.wingtip-dpt.&lt ;user&gt;.trafficmanager.net, где &lt; user&gt; — это имя пользователя для вашего развертывания.
    
   Прокрутите страницу вниз и обратите внимание на имя и расположение сервера каталогов в нижнем колонтитуле. Расположение — это регион, в котором вы развернули приложение.    

   > [!TIP]
   > Наведите указатель мыши на расположение, чтобы увеличить область отображения.

   ![Работоспособное состояние концентратора событий в исходной области](media/saas-dbpertenant-dr-geo-restore/events-hub-original-region.png)

2. Выберите клиент Contoso Concert Hall и откройте его страницу событий.

   В нижнем колонтитуле отобразится имя сервера клиента. Его расположение будет таким же, как и у сервера каталога.

   ![Исходный регион Contoso Concert Hall](media/saas-dbpertenant-dr-geo-restore/contoso-original-location.png) 

3. На [портале Azure](https://portal.azure.com) найдите и откройте группу ресурсов, в которой было развернуто приложение.

   Обратите внимание на ресурсы и регион развертывания компонентов службы приложений и серверов Базы данных SQL.

## <a name="sync-the-tenant-configuration-into-the-catalog"></a>Синхронизация сведений о конфигурации клиента с каталогом

В этой задаче вы запускаете процесс синхронизации конфигурации серверов, эластичных пулов и баз данных с каталогом клиентов. Эта информация используется позднее для настройки среды зеркалирования в регионе восстановления.

> [!IMPORTANT]
> Для простоты процесс синхронизации и другие длительные процессы восстановления и возвращения реализованы в этих примерах в виде локальных заданий или сеансов PowerShell, которые выполняются под именем пользователя вашего клиента. Срок действия маркеров аутентификации, выданных при входе в систему, истекает через несколько часов, и это приведет к сбою незавершенных заданий. В рабочем сценарии длительные процессы должны быть реализованы как надежные службы Azure, работающие под управлением субъекта-службы. Дополнительные сведения см. в статье [Использование Azure PowerShell для создания субъекта-службы с сертификатом](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-authenticate-service-principal). 

1. В интегрированной среде сценариев PowerShell откройте файл ...\Learning Modules\UserConfig.psm1. Замените `<resourcegroup>` и `<user>` в строках 10 и 11 значениями, которые вы указали при развертывании приложения. Сохраните файл.

2. В интегрированной среде сценариев PowerShell откройте скрипт ...\Learning Modules\Business Continuity and Disaster Recovery\DR-RestoreFromBackup\Demo-RestoreFromBackup.ps1.

    Этот скрипт PowerShell потребуется вам для каждого сценария, описанного в этом руководстве, поэтому не закрывайте этот файл.

3. Задайте следующие параметры:

    $DemoScenario = 1. Запуск фонового задания, которое синхронизирует данные о конфигурации сервера клиента и пула в каталог.

4. Нажмите клавишу F5, чтобы запустить скрипт синхронизации. 

    Эта информация используется позднее, чтобы убедиться, что восстановления создает зеркальный образ баз данных, пулов и серверов в регионе восстановления.  

    ![Процесс синхронизации](media/saas-dbpertenant-dr-geo-restore/sync-process.png)

Оставьте окно PowerShell в фоновом режиме и продолжите работу с руководством.

> [!NOTE]
> Процесс синхронизации подключается к каталогу с помощью псевдонима DNS. Этот псевдоним изменяется во время восстановления и репатриации, чтобы указать на активный каталог. Процесс синхронизации поддерживает актуальность каталога, сохраняя все изменения конфигурации базы данных или пула, внесенные в регионе восстановления. Во время репатриации эти изменения применяются к эквивалентным ресурсам в исходном регионе.

## <a name="geo-restore-recovery-process-overview"></a>Общие сведения о процессе геовосстановления

В процессе геовосстановления приложение развертывается и базы данных восстанавливаются из резервных копий в регионе восстановления.

Процесс восстановления выполняет следующие задачи.

1. Отключается конечная точка диспетчера трафика Azure для веб-приложения в исходном регионе. Отключение конечной точки предотвращает подключение пользователей к приложению в нерабочем состоянии, если исходный регион вернется к работе в сети во время восстановления.

2. Подготавливается сервер каталогов в регионе восстановления, выполняет геовосстановление баз данных каталогов и обновляет псевдоним activecatalog, чтобы он указывал на восстановленный сервер каталогов. Изменение псевдонима каталога гарантирует, что синхронизируется всегда именно активный каталог.

3. Помечает все существующие клиенты в каталоге восстановления как автономные, чтобы предотвратить доступ к базам данных клиентов, прежде чем они будут восстановлены.

4. Подготавливает экземпляр приложения в регионе восстановления и настраивает его на использование восстановленного каталога в таком регионе. Чтобы свести задержки к минимуму, этот пример приложения всегда подключается к базе данных клиента в своем регионе.

5. Подготавливается сервер и эластичный пул, в котором подготавливаются новые клиенты. Создание этих ресурсов гарантирует, что подготовка новых клиентов не будет препятствовать восстановлению существующих клиентов.

6. Обновляется псевдоним нового клиента, чтобы он указывал на сервер для новых баз данных клиентов в регионе восстановления. Изменение этого псевдонима гарантирует, что базы данных для любых новых клиентов будут подготовлены в регионе восстановления.
        
7. Подготавливает серверы и эластичные пулы в регионе восстановления для восстановления баз данных клиентов. Эти серверы и кластеры являются зеркальными образами конфигурации в исходном районе. При заблаговременной подготовке пулов резервируются ресурсы, необходимые для восстановления всех баз данных.

    Сбой в регионе может создать значительную нагрузку на ресурсы, доступные в парном регионе. Если для аварийного восстановления используется геовосстановление, рекомендуется прибегнуть к быстрому резервированию ресурсов. Попробуйте применить георепликацию, если вам важно всегда восстанавливать приложения в определенном регионе. 

8. Включает конечную точку диспетчера трафика для веб-приложения в регионе восстановления. Это позволяет приложению подготавливать новые клиенты. На этом этапе существующие клиенты остаются в автономном режиме.

9. Отправляет пакеты запросов на восстановление баз данных в порядке приоритета. 

    * Пакеты упорядочены таким образом, чтобы восстановления баз данных не выполнялись параллельно во всех пулах.  

    * Запросы на восстановление отправляются асинхронно, поэтому отправка происходит быстро, а запросы формируют очередь на выполнение в каждом пуле.

    * Так как запросы на восстановление во всех пулах обрабатываются параллельно, лучше распределить важных клиентов по нескольким пулам. 

10. Отслеживается служба Базы данных SQL, чтобы определить момент восстановления баз данных. После восстановления базы данных клиента она помечается в каталоге как подключенная и регистрируется сумма rowversion для этой базы данных клиента. 

    * Приложение сможет получить доступ к базам данных клиентов, как только они будут помечены в каталоге с состоянием "В сети".

    * Сумма значений rowversion в базе данных клиента сохраняется в каталоге. Она выполняет роль отпечатка, который позволяет в процессе возвращения в исходное расположение определить, обновлялась ли база данных ​​в регионе восстановления.       

## <a name="run-the-recovery-script"></a>Запустите скрипт восстановления

> [!IMPORTANT]
> В этом руководстве рассматривается восстановление баз данных из геоизбыточных резервных копий. Обычно резервные копии становятся доступны уже через 10 минут, но иногда этот процесс может занять до одного часа. Скрипт приостанавливает работу, пока они не станут доступны.

Смоделируйте ситуацию сбоя в регионе, где развернуто приложение, выполнив скрипт восстановления:

1. В интегрированной среде сценариев PowerShell откройте скрипт ...\Learning Modules\Business Continuity and Disaster Recovery\DR-RestoreFromBackup\Demo-RestoreFromBackup.ps1 и установите следующее значение.

    $DemoScenario = 2. Восстановление приложения в регионе восстановления из геоизбыточных резервных копий.

2. Нажмите клавишу F5, чтобы выполнить скрипт.  

    * Скрипт открывается в новом окне PowerShell, а затем запускает ряд заданий PowerShell, которые выполняются параллельно. Эти задания восстанавливают серверы, пулы и базы данных в регионе восстановления.

    * Регионом восстановления считается парный регион, связанный с регионом Azure, в котором развернуто приложение. Дополнительные сведения см. в статье [Непрерывность бизнес-процессов и аварийное восстановление в службах BizTalk: пары регионов Azure](https://docs.microsoft.com/azure/best-practices-availability-paired-regions). 

3. Отслеживайте состояние процесса восстановления в окне PowerShell.

    ![Процесс восстановления](media/saas-dbpertenant-dr-geo-restore/dr-in-progress.png)

> [!NOTE]
> Чтобы изучить код заданий восстановления, просмотрите скрипты PowerShell в папке ...\Learning Modules\Business Continuity and Disaster Recovery\DR-RestoreFromBackup\RecoveryJobs.

## <a name="review-the-application-state-during-recovery"></a>Проверка состояния приложения во время восстановления
Когда конечная точка приложения отключена в диспетчере трафика, приложение недоступно. После восстановления каталога все клиенты помечаются как отключенные. После этого включается конечная точка приложения в регионе восстановления и приложение снова становится доступным. Несмотря на то что приложение доступно, клиент отключен в концентраторе событий до восстановления своих баз данных. Важно, чтобы ваше приложение обрабатывало автономные базы данных клиентов.

* После восстановления базы данных каталогов, но до возобновления работы клиентов, обновите концентратор событий Wingtip Tickets в веб-браузере.

  * В нижнем колонтитуле вы увидите, что имя сервера каталогов имеет суффикс -recovery и этот сервер размещен в регионе восстановления.

  * Обратите внимание, что клиенты, которые еще не восстановлены, отмечены как автономные и не могут быть выбраны.   
 
    ![Процесс восстановления](media/saas-dbpertenant-dr-geo-restore/events-hub-tenants-offline-in-recovery-region.png)    

  * На странице событий клиента, который в этот момент не подключен, отображается уведомление об автономном режиме клиента. Например, попробуйте перейти по адресу http://events.wingtip-dpt.&lt ;user&gt;.trafficmanager.net/contosoconcerthall, когда клиент Contoso Concert Hall отключен.

    ![Процесс восстановления](media/saas-dbpertenant-dr-geo-restore/dr-in-progress-offline-contosoconcerthall.png)

## <a name="provision-a-new-tenant-in-the-recovery-region"></a>Подготовка нового клиента в регионе восстановления
Даже перед восстановлением баз данных клиентов вы можете подготовить новые клиенты в регионе восстановления. Новые базы данных клиентов, подготовленные в регионе восстановления, позже возвращаются в исходное расположение с восстановленными базами данных.   

1. В интегрированной среде сценариев PowerShell откройте скрипт ...\Learning Modules\Business Continuity and Disaster Recovery\DR-RestoreFromBackup\Demo-RestoreFromBackup.ps1 и установите следующее свойство.

    $DemoScenario = 3. Подготовка нового клиента в регионе восстановления.

2. Нажмите клавишу F5, чтобы выполнить скрипт.

3. По завершении подготовки в браузере откроется страница событий для Hawthorn Hall. 

    Обратите внимание, что база данных Hawthorn Hall расположена в регионе восстановления.

    ![Клиент Hawthorn Hall, подготовленный в регионе восстановления](media/saas-dbpertenant-dr-geo-restore/hawthorn-hall-provisioned-in-recovery-region.png)

4. В браузере обновите страницу концентратора событий Widtip Tickets, чтобы проверить добавление клиента Hawthorn Hall. 

    Если вы подготовили Hawthorn Hall, не дожидаясь восстановления других клиентов, некоторые из них могут быть еще отключены.

## <a name="review-the-recovered-state-of-the-application"></a>Проверка восстановленного состояния приложения

Когда процесс восстановления завершится, приложение и все клиенты будут полностью работоспособны в регионе восстановления. 

1. Как только в окне консоли PowerShell будет указано, что все клиенты восстановлены, обновите страницу концентратора событий. 

    Все клиенты, включая Hawthorn Hall, будут обозначены как подключенные.

    ![Восстановленные и новые клиенты в концентраторе событий](media/saas-dbpertenant-dr-geo-restore/events-hub-with-hawthorn-hall.png)

2. Щелкните Contoso Concert Hall и откройте страницу событий. 

    В нижнем колонтитуле можно увидеть, что база данных находится на сервере восстановления в целевом регионе восстановления.

    ![Contoso в регионе восстановления](media/saas-dbpertenant-dr-geo-restore/contoso-recovery-location.png)

3. На [портале Azure](https://portal.azure.com) перейдите к списку групп ресурсов.  

    Обратите внимание на группу ресурсов, которую вы развернули, а также группу ресурсов восстановления с суффиксом -recovery. Группа ресурсов восстановления содержит все ресурсы, созданные во время процесса восстановления, а также новые ресурсы, созданные во время сбоя. 

4. Откройте группу ресурсов восстановления и обратите внимание на следующие элементы:

   * Восстановленные версии серверов для каталога и tenants1 с суффиксом -recovery. Восстановленные базы данных каталога и клиентов на этих серверах имеют имена, используемые в исходном регионе.

   * SQL Server с именем tenants2-dpt-&lt;пользователь&gt;-recovery. Этот сервер используется для подготовки новых клиентов во время сбоя.

   * Служба приложений с именем events-wingtip-dpt-&lt;регион_восстановления&gt;-&lt;пользователь&gt;, которая является восстановленным экземпляром приложения событий.

     ![Ресурсы Contoso в регионе восстановления](media/saas-dbpertenant-dr-geo-restore/resources-in-recovery-region.png) 
    
5. Откройте SQL Server с именем tenants2-dpt-&lt;пользователь&gt;-recovery. Обратите внимание, что он содержит базу данных hawthornhall и эластичный пул Pool1. База данных hawthornhall настроена как эластичная база данных в эластичном пуле Pool1.

## <a name="change-the-tenant-data"></a>Изменение данных клиента 
В этой задаче вы обновляете одну из восстановленных баз данных клиентов. При возвращении в исходный регион копируются восстановленные базы данных, в которые вносились изменения. 

1. Найдите в браузере список событий для Contoso Concert Hall и прокрутите до последнего события Seriously Strauss.

2. В интегрированной среде сценариев PowerShell откройте скрипт ...\Learning Modules\Business Continuity and Disaster Recovery\DR-RestoreFromBackup\Demo-RestoreFromBackup.ps1 и установите следующее значение.

    $DemoScenario = 4. Удаление события из клиента в регионе восстановления.

3. Нажмите клавишу F5, чтобы выполнить скрипт.

4. Обновите страницу событий Contoso Concert Hall (http://events.wingtip-dpt.&lt ;user&gt;.trafficmanager.net/contosoconcerthall) и обратите внимание, что событие Seriously Strauss исчезло.

На этом этапе в руководстве уже восстановлено приложение, которое выполняется в регионе восстановления. Новый клиент подготовлен в регионе восстановления, а также подготовлены измененные данные одного из восстановленных клиентов.  

> [!NOTE]
> Другие руководства в этом примере не предназначены для работы с приложением в состоянии восстановления. Если вы хотите изучить другие руководства, предварительно убедитесь, что приложение репатриировано.

## <a name="repatriation-process-overview"></a>Обзор процесса репатриации

После устранения сбоя процесс репатриации возвращает приложение и его базы данных в его исходный регион.

![Репатриация геовосстановления](media/saas-dbpertenant-dr-geo-restore/geo-restore-repatriation.png) 

Процесс:

1. Останавливает любые текущие процессы восстановления и отменяет все необработанные или активные запросы на восстановление баз данных.

2. Реактивирует клиентские базы данных в исходном регионе, которые не были изменены с момента сбоя. К ним относятся все базы данных, которые еще не были восстановлены, а также восстановленные, но не измененные базы данных. Реактивированные базы данных будут точно такими же, как в момент последнего доступа к ним их клиентов.

3. Подготавливает зеркальную копию сервера и эластичного пула для нового клиента в исходном регионе. После завершения этого действия обновляется псевдоним нового клиента, чтобы он указывал на этот сервер. Обновление псевдонима вызывает адаптацию нового клиента в первоначальном регионе вместо региона восстановления.

3. С помощью георепликации каталог перемещается из региона восстановления в исходный регион.

4. Обновляет конфигурацию пула в исходном регионе, чтобы в ней были отражены все изменения, внесенные в регионе восстановления за время сбоя.

5. Создает необходимые серверы и пулы для размещения новых баз данных, созданных за время сбоя.

6. Применяет георепликацию для возврата баз данных клиентов, которые были обновлены после восстановления и подготовлены для новых клиентов за время сбоя. 

7. Очищает ресурсы, созданные в регионе восстановления во время восстановления.

Чтобы сократить количество возвращаемых клиентских баз данных, шаги 1–3 выполняются в кратчайшие сроки.  

Шаг 4 выполняется только в том случае, если каталог в регионе восстановления был изменен во время сбоя. Каталог обновляется после создания новых клиентов или после какого-либо изменения конфигурации базы данных или пула в регионе восстановления.

Очень важно, что шаг 7 вызывает минимальное нарушение работы для клиентов, и данные не теряются. Для этого используется георепликация.

Перед георепликацией каждой базы данных соответствующая база данных в исходном регионе удаляется. База данных в регионе восстановления затем будет геореплицирована — когда создается вторичная реплика в исходном регионе. После завершения репликации клиент помечается в каталоге как отключенный, что разрывает все подключения к его базе данных в регионе восстановления. Затем для этой базы данных выполняется отработка отказа, в результате чего все незавершенные транзакции обрабатываются на целевом сервере без потери данных. 

При отработке отказа роли базы данных меняются. База данных-получатель в исходном регионе становится основной базой данных чтения и записи, а база данных в регионе восстановления становится доступной только для чтения вторичной репликой. Запись клиента в каталоге обновляется, чтобы указывать на базу данных в исходном регионе, и клиент снова помечается как подключенный. На этом этапе репатриация базы данных завершена. 

В приложениях должна быть реализована логика повторных попыток, чтобы они автоматически подключались повторно после разрыва подключения. Если повторные подключения выполняются по данным каталога, то приложение сразу подключится к возвращенной базе данных в исходном регионе. Обычно клиенты не обращают внимания на кратковременное отключение, но все равно лучше возвращать базы данных в нерабочее время.

После возвращения базы данных в исходное расположение можно удалить дополнительную базу данных в регионе восстановления. Теперь для защиты базы данных с помощью аварийного восстановления в исходном регионе снова применяется геовосстановление.

На шаге 8 ресурсы в регионе восстановления, включая серверы и пулы восстановления, будут удалены.

## <a name="run-the-repatriation-script"></a>Запуск скрипта репатриации
Смоделируйте ситуацию устранения сбоя, запустив скрипт возвращения в исходное расположение.

Если вы точно следовали руководству, этот скрипт немедленно активирует клиентов Fabrikam Jazz Club и Dogwood Dojo в исходном регионе, так как данные для них не изменялись. Затем выполняется репатриация нового клиента, Hawthorn Hall и Contoso Concert Hall, так как были внесены изменения. Скрипт также репатриирует каталог, который был обновлен во время подготовки Hawthorn Hall.
  
1. В интегрированной среде сценариев PowerShell откройте скрипт ...\Learning Modules\Business Continuity and Disaster Recovery\DR-RestoreFromBackup\Demo-RestoreFromBackup.ps1 и убедитесь, что процесс Catalog Sync еще выполняется в соответствующем экземпляре PowerShell. При необходимости перезапустите его, задав:

    $DemoScenario = 1. Запуск синхронизации сведений о конфигурации сервера клиента, пула и базы данных с каталогом.

    Нажмите клавишу F5, чтобы выполнить скрипт.

2.  Затем, чтобы начать процесс репатриации, установите:

    $DemoScenario = 5. Возвращение приложения в исходный регион.

    Нажмите клавишу F5, чтобы запустить скрипт восстановления в новом окне PowerShell. Возвращение займет несколько минут, а его состояние можно отслеживать в окне PowerShell.

3. Пока выполняется сценарий, обновите страницу концентратора событий (http://events.wingtip-dpt.&lt ;user&gt;.trafficmanager.net).

    Обратите внимание, что все клиенты находятся в оперативном режиме и доступны на протяжении всего процесса.

4. Выберите Jazz Fabrikam Club, чтобы открыть сведения о клиенте. Если вы не изменяли его данные, в нижнем колонтитуле в качестве расположения уже будет указан исходный сервер.

5. Откройте или обновите страницу событий для Contoso Concert Hall. В нижнем колонтитуле еще некоторое время будет указываться, что база данных находится на сервере восстановления (-recovery). 

6. Когда завершится возвращение, обновите страницу событий для Contoso Concert Hall. Теперь база данных снова расположена в исходном регионе.

7. Еще раз обновите страницу концентратора событий и откройте Hawthorn Hall. Здесь также указано, что база данных находится в исходном регионе. 

## <a name="clean-up-recovery-region-resources-after-repatriation"></a>Очистка ресурсов региона восстановления после репатриации
Когда завершится возвращение, можно безопасно удалить все ресурсы в регионе восстановления. 

> [!IMPORTANT]
> Удалите эти ресурсы незамедлительно, чтобы остановить все выставления счетов за них.

Процесс восстановления создает все ресурсы восстановления в группе ресурсов для восстановления. В процессе очистки удаляется эта группа ресурсов и убираются из каталога все ссылки на эти ресурсы. 

1. В интегрированной среде сценариев PowerShell откройте скрипт ...\Learning Modules\Business Continuity and Disaster Recovery\DR-RestoreFromBackup\Demo-RestoreFromBackup.ps1 и задайте следующее значение.
    
    $DemoScenario = 6. Удаление устаревших ресурсов из региона восстановления.

2. Нажмите клавишу F5, чтобы выполнить скрипт.

После очистки скриптов приложение вернется туда, откуда оно было запущено. Теперь вы можете снова запустить скрипт или сразу переходить к другим руководствам.

## <a name="designing-the-application-to-ensure-that-the-app-and-the-database-are-co-located"></a>Разработка механизмов, позволяющих обеспечить постоянное размещение приложения и базы данных в одном расположении 
Приложение специально разработано так, чтобы к базе данных клиента подключался экземпляр приложения из того же региона. Это уменьшает задержку между приложением и базой данных. Такая оптимизация предполагает, что взаимодействие приложения с базой данных происходит чаще, чем взаимодействие между пользователем и приложением.  

В ходе возвращения в исходное расположение базы данных клиентов еще некоторое время могут быть распределены между регионом восстановления и исходным регионом. Для каждой базы данных приложение ищет регион, в котором размещена база данных, выполняя поиск DNS по имени сервера клиента. В базе данных SQL имя сервера является псевдонимом. Псевдоним имени сервера содержит имя региона. Если приложение не находится в том же регионе, что и база данных, оно перенаправляется к экземпляру в том же регионе, где находится сервер базы данных. Перенаправление к экземпляру в том же регионе, где расположена база данных, минимизирует задержку между приложением и базой данных.  

## <a name="next-steps"></a>Следующие шаги

В этом руководстве вы узнали, как:
> [!div class="checklist"]
> 
> * сохранение периодически обновляемой конфигурации в каталоге клиента, что позволяет создать зеркальную копию среды для восстановления в другом регионе;
> * геовосстановление баз данных SQL Azure в регионе восстановления;
> * внесение в каталог клиентов новых расположений восстановленной базы данных клиента; 
> * использование DNS-псевдонима для подключения приложения к каталогу клиента без изменения конфигурации;
> * использование георепликации для возвращения восстановленных баз данных в исходный регион после устранения сбоя.

Чтобы узнать, как георепликация позволяет значительно сократить время восстановления для крупномасштабных многопользовательских приложений, изучите руководство [Аварийное восстановление мультитенантных приложений SaaS с использованием георепликации базы данных](saas-dbpertenant-dr-geo-replication.md).

## <a name="additional-resources"></a>Дополнительные ресурсы

[Дополнительные руководства по работе с приложением SaaS Wingtip](saas-dbpertenant-wingtip-app-overview.md#sql-database-wingtip-saas-tutorials)
