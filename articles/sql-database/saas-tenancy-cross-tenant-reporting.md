---
title: Выполнение запросов системы отчетности к нескольким базам данных SQL Azure | Документация Майкрософт
description: Сведения о формировании отчетов по всем клиентам с использованием распределенных запросов.
services: sql-database
ms.service: sql-database
ms.subservice: scenario
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewers: billgib,ayolubek
manager: craigg
ms.date: 01/25/2019
ms.openlocfilehash: 9562d0cd1ad97a459c3630456a6070ac2b6e63f3
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "61484740"
---
# <a name="cross-tenant-reporting-using-distributed-queries"></a>Отчеты по всем клиентам с использованием распределенных запросов

В этом руководстве выполняются распределенные запросы ко всему набору баз данных клиента, чтобы обеспечить формирование отчетов. С помощью таких запросов можно найти интересные сведения в огромных объемах текущих операционных данных клиентов SaaS Wingtip Tickets. Для этого разверните дополнительную базу данных отчетов на сервере каталога и примените эластичные распределенные запросы.


Из этого руководства вы узнаете следующее:

> [!div class="checklist"]
> 
> * как развертывать базу данных отчетов;
> * как выполнять распределенные запросы по всем клиентским базам данных.
> * как с помощью глобальных представлений в каждой базе данных можно обеспечить эффективные запросы ко всем клиентам.


Для работы с этим руководством выполните следующие предварительные требования:


* Развернуть SaaS-приложение Wingtip Tickets c однотенантной базой данных. См. инструкции из руководства по быстрому [развертыванию SaaS-приложение Wingtip Tickets c однотенантной БД](saas-dbpertenant-get-started-deploy.md).
* Установите Azure PowerShell. Дополнительные сведения см. в статье [Начало работы с Azure PowerShell](https://docs.microsoft.com/powershell/azure/get-started-azureps).
* Установите SQL Server Management Studio (SSMS). Сведения о том, как скачать и установить SSMS, см. в статье [Скачивание SQL Server Management Studio (SSMS)](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms).


## <a name="cross-tenant-reporting-pattern"></a>Шаблон отчетов по всем клиентам

![шаблон распределенного запроса по всем клиентам](media/saas-tenancy-cross-tenant-reporting/cross-tenant-distributed-query.png)

Одна из важнейших возможностей приложений SaaS — вы можете использовать огромный объем клиентских данных, хранящихся в облаке, для анализа работы и использования такого приложения. Эти сведения будут полезны при разработке функций, помогут повысить удобство использования, а также обеспечить вложение дополнительных средств в приложения и службы.

Доступ к этим данным получить очень просто, если они содержатся в отдельной мультитенантной базе данных, но не так просто, если они распределены по тысячам масштабируемых баз данных. Одним из подходов является использование [эластичного запроса](sql-database-elastic-query-overview.md), который позволяет выполнять запросы в распределенном наборе баз данных с общей схемой. Этот набор может включать базы данных из нескольких групп ресурсов и разных подписок, но в них нужно создать общую учетную запись. Эластичный запрос использует отдельную *головную* базу данных, в которой внешние таблицы определены как зеркальные таблицы или представления в распределенных (клиентских) базах данных. Запросы, отправленные к этой головной базе данных, компилируются для создания распределенного плана запроса с частями запроса, которые при необходимости можно принудительно установить в клиентских базах данных. Эластичный запрос использует карту сегментов в базе данных каталога для определения расположения всех клиентских баз данных. При настройке и выполнении запроса к головной базе данных используется стандартный язык [Transact-SQL](https://docs.microsoft.com/sql/t-sql/language-reference) и поддерживаются настраиваемые запросы из специализированных инструментов, таких как Power BI и Excel.

Благодаря распределению запросов между базами данных клиента эластичный запрос позволяет мгновенно получить представление о текущих производственных данных. Эластичный запрос извлекает данные из потенциального множества баз данных, задержка при выполнении запроса может быть выше, чем для аналогичных запросов, отправленных к отдельной мультитенантной базе данных. Чтобы сократить объем данных, возвращаемых в головную базу данных, следует применить проектирование запросов. Как правило, эластичный запрос лучше всего подходит для получения небольших объемов данных в реальном времени, в отличие от часто используемых или сложных аналитических запросов или отчетов. Если запросы не выполняются надлежащим образом, изучите [план выполнения](https://docs.microsoft.com/sql/relational-databases/performance/display-an-actual-execution-plan), чтобы узнать, какая часть запроса перемещается в удаленную базу данных и сколько данных возвращается. Запросы, требующие сложной агрегации или аналитической обработки, могут обслуживаться лучше, если данные клиента будут извлечены в базу данных или хранилище данных, оптимизированное для аналитических запросов. Эта схема описана в [руководстве по аналитическим запросам с использованием клиентов](saas-tenancy-tenant-analytics.md). 

## <a name="get-the-wingtip-tickets-saas-database-per-tenant-application-scripts"></a>Получение скриптов для SaaS-приложения Wingtip Tickets c однотенантной БД

Сценарии для приложения SaaS Wingtip Tickets c мультитенантной базой данных и исходный код этого приложения вы найдете в репозитории GitHub [WingtipTicketsSaaS-DbPerTenant](https://github.com/Microsoft/WingtipTicketsSaaS-DbPerTenant). Инструкции по скачиванию и разблокированию сценариев приложения SaaS Wingtip Tickets см. в статье [Общие рекомендации по работе с примерами приложений SaaS Wingtip Tickets](saas-tenancy-wingtip-app-guidance-tips.md).

## <a name="create-ticket-sales-data"></a>Создание данных о продажах билетов

Чтобы выполнять запросы к более интересному набору данных, создайте данные о продаже билетов, запустив генератор данных билетов.

1. В *интегрированной среде сценариев PowerShell* откройте скрипт …\\Learning Modules\\Operational Analytics\\Adhoc Reporting\\*Demo-AdhocReporting.ps1* и задайте в нем следующее значение:
   * **$DemoScenario** = 1, чтобы **приобрести билеты на мероприятия во всех местах проведения**.
2. Нажмите клавишу **F5**, чтобы запустить сценарий и создать данные о продаже билетов. Пока сценарий выполняется, продолжите процедуру, описанную в этом руководстве. В разделе *Run ad hoc distributed queries* (Выполнение запросов автоматизированной системы отчетности) идет обращение к данным о билетах, поэтому дождитесь, пока генератор данных билетов завершит работу.

## <a name="explore-the-global-views"></a>Обзор глобальных представлений

В этом примере приложения SaaS Wingtip Tickets каждому клиенту предоставляется отдельная база данных. Это означает, что в каждой таблице базы данных хранится информация, относящаяся только к одному клиенту. Тем не менее при выполнении запросов по всем базам данных важно, чтобы эластичный запрос мог рассматривать данные как часть отдельной логической базы данных, сегментированной по клиентам. 

Чтобы смоделировать такой подход, в базу данных клиента добавлен набор глобальных представлений, которые проецируют идентификатор клиента на каждую из таблиц, для которых выполняются глобальные запросы. Например, представление *VenueEvents* добавляет вычисляемый столбец *VenueId* к столбцам, спроецированным из таблицы *Events*. Точно так же представления *VenueTicketPurchases* и *VenueTickets* добавляют вычисляемый столбец *VenueId*, к данным из соответствующих таблиц. Эластичный запрос использует представления со столбцами *VenueId*, чтобы разделить запросы на несколько параллельных запросов и передать их соответствующим удаленным клиентским базам данных. Это значительно сокращает объем возвращаемых данных и приводит к заметному увеличению производительности многих запросов. Эти глобальные представления создаются заранее во всех клиентских базах данных.

1. Откройте среду SSMS и подключитесь к серверу [tenants1-&lt;пользователь&gt;](saas-tenancy-wingtip-app-guidance-tips.md#explore-database-schema-and-execute-sql-queries-using-ssms).
1. Разверните узел **Базы данных**, щелкните _contosoconcerthall_ правой кнопкой мыши и выберите **Создать запрос**.
1. Выполните следующие запросы, чтобы увидеть разницу между таблицами одного клиента и глобальными представлениями:

   ```T-SQL
   -- The base Venue table, that has no VenueId associated.
   SELECT * FROM Venue

   -- Notice the plural name 'Venues'. This view projects a VenueId column.
   SELECT * FROM Venues

   -- The base Events table, which has no VenueId column.
   SELECT * FROM Events

   -- This view projects the VenueId retrieved from the Venues table.
   SELECT * FROM VenueEvents
   ```

В этих представлениях значение *VenueId* было вычислено как хэш названия места проведения (Venue), но уникальное значение можно ввести любым другим способом. Аналогичным способом вычисляется в ключ клиента для использования в каталоге.

Вот как можно просмотреть определение представления *Venues*.

1. В **обозревателе объектов** разверните узел **contosoconcerthall** > **Представления**:

   ![узел "Представления"](media/saas-tenancy-cross-tenant-reporting/views.png)

2. Щелкните **dbo.Venues** правой кнопкой мыши.
3. Выберите **Создать скрипт для представления** > **Используя CREATE** > **В новом окне редактора запросов**.

Создайте сценарий, используя любые другие представления *Venue*, чтобы увидеть, как они добавляют *VenueId*.

## <a name="deploy-the-database-used-for-distributed-queries"></a>Развертывание базы данных, используемой для распределенных запросов

Этот пример развертывает базу данных _adhocreporting_. Это головная база данных, которая будет содержать схему, используемую для выполнения запросов по всем базам данных клиента. Эта база данных развертывается на имеющемся сервере каталога, используемом для всех баз данных для управления в данном примере приложения.

1. В *интегрированной среде сценариев PowerShell* откройте сценарий \\Learning Modules\\Operational Analytics\\Adhoc Reporting\\*Demo-AdhocReporting.ps1*. 

1. Задайте **$DemoScenario = 2**, _развертывание Ad hoc, база данных отчетов_.

1. Нажмите клавишу **F5**, чтобы выполнить скрипт и создать базу данных *adhocreporting*.

В следующем разделе вы добавите схему в базу данных, чтобы ее можно было использовать для выполнения распределенных запросов.

## <a name="configure-the-head-database-for-running-distributed-queries"></a>Настройка головной базы данных для выполнения распределенных запросов

Этот пример добавляет в базу данных _автоматизированной системы отчетности_ схему (внешний источник данных и определения внешних таблиц), которая позволяет выполнять запросы ко всем клиентским базам данных.

1. Откройте SQL Server Management Studio и подключитесь к базе данных автоматизированной системы отчетности, которую вы создали на предыдущем шаге. Это база данных с именем *adhocreporting*.
2. В SSMS откройте файл ...\Learning Modules\Operational Analytics\Adhoc Reporting\ _Initialize-AdhocReportingDB.sql_.
3. Просмотрите этот сценарий SQL и обратите внимание на следующее.

   Эластичный запрос использует учетные данные уровня базы данных для доступа к каждой базе данных клиента. Эти учетные данные должны присутствовать во всех базах данных. Обычно им предоставляются минимальные права, необходимые для поддержки запросов автоматизированной системы отчетности.

    ![Создание учетных данных](media/saas-tenancy-cross-tenant-reporting/create-credential.png)

   При использовании базы данных каталога в качестве внешнего источника данных запросы при выполнении распределяются ко всем базам данных, зарегистрированным в каталоге. Так как имена серверов для каждого развертывания отличаются, этот скрипт получает расположение базы данных каталога из имени текущего сервера (@@servername), на котором выполняется скрипт.

    ![Создание внешнего источника данных](media/saas-tenancy-cross-tenant-reporting/create-external-data-source.png)

   Внешние таблицы, которые ссылаются на глобальные представления, описаны в предыдущем разделе и определены с помощью параметра **DISTRIBUTION = SHARDED(VenueId)** . Так как каждое значение *VenueId* сопоставляется с отдельной базой данных, это повышает производительность во многих ситуациях, как показано в следующем разделе.

    ![Создание внешних таблиц](media/saas-tenancy-cross-tenant-reporting/external-tables.png)

   Локальная таблица _VenueTypes_, которая создается и заполняется. Эта таблица эталонных данных является общей во всех базах данных клиента, поэтому здесь ее можно представить в качестве локальной таблицы и заполнить общими данными. Для некоторых запросов наличие этой таблицы в головной базе данных может снизить объем данных, которые нужно переместить в эту базу данных.

    ![Создание таблицы](media/saas-tenancy-cross-tenant-reporting/create-table.png)

   В случае добавления ссылочных таблиц таким способом необходимо обновлять схему и данные таблицы при каждом обновлении базы данных клиента.

4. Нажмите клавишу **F5**, чтобы выполнить скрипт и инициализировать базу данных *adhocreporting*. 

Теперь можно выполнять распределенные запросы и собирать данные аналитики по всем клиентам.

## <a name="run-distributed-queries"></a>Выполнение распределенных запросов

Теперь вы завершили настройку базы данных *adhocreporting* и можете выполнить примеры распределенных запросов. Добавьте план выполнения, чтобы лучше понимать, где происходит обработка запросов. 

При проверке плана выполнения наведите указатель мыши на значки плана, чтобы получить дополнительные сведения. 

Важно отметить, что параметр **DISTRIBUTION = SHARDED(VenueId)** , заданный при определении внешнего источника данных, повышает производительность во многих ситуациях. Так как каждое *VenueId* сопоставляется с отдельной базы данных, фильтрацию легко выполнять удаленно, возвращая только необходимые данные.

1. В SSMS откройте файл \\Learning Modules\\Operational Analytics\\Adhoc Reporting\\*Demo-AdhocReportingQueries.sql*.
2. Убедитесь в наличии подключения к базе данных **adhocreporting**.
3. В меню **Запрос** выберите **Включить действительный план выполнения**.
4. Выделите запрос *Which venues are currently registered?* и нажмите клавишу **F5**.

   Запрос вернет полный список мест проведения, демонстрируя, как быстро и легко можно выполнить запрос по всем клиентам и вернуть данные из каждого из них.

   Просмотрите план, и вы увидите, что все затраты связаны исключительно с удаленным запросом. Каждая база данных клиента удаленно выполняет запрос и возвращает информацию о местах проведения в головную базу данных.

   ![SELECT * FROM dbo.Venues](media/saas-tenancy-cross-tenant-reporting/query1-plan.png)

5. Выберите следующий запрос и нажмите клавишу **F5**.

   Этот запрос объединяет данные из клиентских баз данных и локальной таблицы *VenueTypes* (она считается локальной, поскольку размещена в базе данных *adhocreporting*).

   Просмотрите план, и вы увидите, что большинство затрат связаны исключительно с удаленным запросом. Каждый клиент базы данных возвращает сведения о местах проведения и выполняет соединение с локальной таблицей *VenueTypes* для отображения понятного имени.

   ![Соединение удаленных и локальных данных](media/saas-tenancy-cross-tenant-reporting/query2-plan.png)

6. Теперь выберите запрос *On which day were the most tickets sold?* и нажмите клавишу **F5**.

   Этот запрос выполняет более сложное соединение и агрегирование. Большая часть обработки происходит удаленно.  В головную базу данных будут возвращены только единичные строки, содержащие количество билетов, продаваемых в день по каждому месту проведения.

   ![query](media/saas-tenancy-cross-tenant-reporting/query3-plan.png)


## <a name="next-steps"></a>Дальнейшие действия

Из этого руководства вы узнали, как выполнять такие задачи:

> [!div class="checklist"]
> 
> * как выполнить распределенные запросы по всем клиентским базам данных.
> * как развернуть базу данных системы отчетности и определить схему для выполнения распределенных запросов.


Теперь ознакомьтесь с руководством [Межклиентская аналитика с помощью извлеченных данных](saas-tenancy-tenant-analytics.md), чтобы изучить извлечение данных в отдельную базу данных аналитики для более сложной аналитической обработки.

## <a name="additional-resources"></a>Дополнительные ресурсы

* Дополнительные [руководства по SaaS-приложению Wingtip Tickets c однотенантной БД](saas-dbpertenant-wingtip-app-overview.md#sql-database-wingtip-saas-tutorials).
* [Обзор эластичных запросов к базе данных SQL Azure (предварительная версия)](sql-database-elastic-query-overview.md)
