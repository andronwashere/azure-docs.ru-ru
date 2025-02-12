---
title: Выполнение запросов аналитики к базам данных клиентов с помощью хранилища данных SQL Azure | Документация Майкрософт
description: Узнайте о выполнении межклиентских запросов аналитики с помощью данных, извлеченных со службы "База данных SQL Azure", "Хранилище данных SQL", "Фабрика данных Azure" или Power BI.
services: sql-database
ms.service: sql-database
ms.subservice: scenario
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: anumjs
ms.author: anjangsh
ms.reviewer: MightyPen, sstein
manager: craigg
ms.date: 12/18/2018
ms.openlocfilehash: a658e2fe32ec95dfabad54684a0c9095af7a341d
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "61485079"
---
# <a name="explore-saas-analytics-with-azure-sql-database-sql-data-warehouse-data-factory-and-power-bi"></a>Изучение аналитики SaaS с помощью Базы данных SQL Azure, хранилища данных SQL, фабрики данных и Power BI

В этом руководстве описывается комплексный сценарий аналитики. В сценарии демонстрируется, как аналитика данных клиентов позволяет поставщикам программного обеспечения принимать интеллектуальные решения. Используя данные, извлеченные из каждой базы данных клиента, можно применять аналитику, чтобы получить детальные сведения о поведении клиента, включая использование примера SaaS-приложения Wingtip Tickets. Этот сценарий состоит из трех этапов. 

1.  **Извлечение данных** из каждой базы данных клиентов в хранилище аналитики. В этом случае в хранилище данных SQL.
2.  **Оптимизация извлеченных данных** для обработки аналитики.
3.  Использование средств **бизнес-аналитики** для получения подробных сведений, на основе которых принимаются важные решения. 

Из этого руководства вы узнаете, как выполнить следующие задачи:

> [!div class="checklist"]
> - создавать хранилище аналитики по клиентам для загрузки;
> - использовать фабрику данных Azure (ADF) для извлечения данных из каждой базы данных клиентов в хранилище данных аналитики;
> - оптимизировать извлеченные данные (реорганизовать в схему типа "звезда");
> - выполнять запросы в хранилище данных аналитики;
> - использовать Power BI для визуализации данных, чтобы выделить тренды в данных клиента и составить рекомендации по оптимизации.

![Обзор архитектуры](media/saas-tenancy-tenant-analytics/adf_overview.png)

## <a name="analytics-over-extracted-tenant-data"></a>Межклиентская аналитика с помощью извлеченных данных

Приложения SaaS хранят потенциально огромный объем данных клиентов в облаке. Эти данные являются богатым источником сведений о функционировании и использовании приложения, а также о поведении клиентов. Они могут помочь в разработке функций, повысить удобство использования, а также обеспечить вложение дополнительных средств в приложение и платформу.

Получать доступ к данным всех клиентов просто, когда все данные находятся в одной мультитенантной базе данных. Когда же данные распределены по тысячам масштабируемых баз данных, мы сталкиваемся с более сложным доступом. Один из способов избежать сложностей доступа — извлечь данные в базу данных аналитики или хранилище данных для запроса.

В этом руководстве представлен комплексный сценарий аналитики для приложения Wingtip Tickets. Сначала [ADF](../data-factory/introduction.md) используется как средство оркестрации для извлечения сведений о продаже билетов и связанных данных из каждой базы данных клиентов. Эти данные загружаются в промежуточные таблицы в хранилище аналитики. Оно может представлять собой базу данных SQL или хранилище данных SQL. В этом руководстве [хранилище данных SQL](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-overview-what-is) используется как хранилище аналитики.

Далее извлеченные данные преобразовываются и загружаются в набор таблиц со [схемой типа "звезда"](https://www.wikipedia.org/wiki/Star_schema). Таблицы состоят из центральной таблицы фактов, а также соответствующих таблиц измерения:

- Центральная таблица фактов в схеме типа "звезда" содержит данные о билетах.
- Таблицы измерения содержат данные о местах проведения, мероприятиях, клиентах и датах покупки.

Совместно эти таблицы обеспечивают эффективную аналитическую обработку. Схема типа "звезда", используемая в этом руководстве, показана на рисунке ниже:
 
![Обзор архитектуры](media/saas-tenancy-tenant-analytics/starschematables.JPG)

И наконец, выполняется запрос таблиц схемы типа "звезда". Результаты запроса отображаются визуально с помощью Power BI, чтобы выделить важные сведения о поведении клиента и о том, как он использует приложение. С помощью этой схемы типа "звезда" вы выполняете запросы, которые предоставляют следующие сведения:

- кто приобретает билеты и в каком месте проведения;
- шаблоны и тренды в продаже билетов;
- соответствующая популярность каждого места проведения.

В этом руководстве приведены базовые примеры важных сведений, которые можно получить из данных Wingtip Tickets. Понимание того, каким образом служба используется в каждом месте проведения, может натолкнуть поставщика Wingtip Tickets на мысль о других планах службы, ориентированных на более или менее активные места проведения. 

## <a name="setup"></a>Настройка

### <a name="prerequisites"></a>Технические условия

> [!NOTE]
> В этом руководстве используются функции фабрики данных Azure, находящиеся на этапе ограниченной предварительной версии (параметризация связанной службы). Если вы хотите изучить это руководство, укажите идентификатор своей подписки [здесь](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbRxrVywox1_tHk9wgd5P8SVJUNlFINjNEOElTVFdMUEREMjVVUlJCUDdIRyQlQCN0PWcu). Мы отправим подтверждающее сообщение сразу после активации вашей подписки.

Для работы с этим руководством выполните следующие предварительные требования:
- Разверните SaaS-приложение Wingtip Tickets c однотенантной БД. Это можно сделать менее чем за пять минут, следуя инструкциям из статьи [Развертывание и изучение мультитенантного приложения SaaS на основе базы данных SQL Azure, в котором используется отдельная база данных для каждого клиента](saas-dbpertenant-get-started-deploy.md).
- Загрузите из GitHub [исходный код](https://github.com/Microsoft/WingtipTicketsSaaS-DbPerTenant/) и сценарии для SaaS-приложения Wingtip Tickets c однотенантной БД. Ознакомьтесь с инструкциями по загрузке. *Разблокируйте ZIP-файл*, а затем извлеките его содержимое.
- Установите Power BI Desktop. [Загрузить Power BI Desktop.](https://powerbi.microsoft.com/downloads/)
- Подготовьте пакет дополнительных клиентов, используя рекомендации в руководстве по [**подготовке новых клиентов**](saas-dbpertenant-provision-and-catalog.md).

### <a name="create-data-for-the-demo"></a>Создание данных для демонстрации

В этом руководстве изучается аналитика данных о продаже билетов. На этом этапе вам необходимо создать данные билетов для всех клиентов. На следующих шагах эти данные извлекаются для анализа. _Подготовьте пакет клиентов_ (как описано выше), чтобы получить достаточно данных для предоставления ряда различных шаблонов покупок билетов.

1. В интегрированной среде сценариев PowerShell откройте файл *…\Learning Modules\Operational Analytics\Tenant Analytics DW\Demo-TenantAnalyticsDW.ps1* и задайте значение, приведенное ниже.
    - **$DemoScenario** = **1**, чтобы приобрести билеты на мероприятия во всех местах проведения.
2. Нажмите клавишу **F5** для запуска скрипта и создания журнала покупок билетов для всех мест проведения. С 20 клиентами сценарий создает десятки тысяч билетов, и это может занять 10 минут или более.

### <a name="deploy-sql-data-warehouse-data-factory-and-blob-storage"></a>Развертывание хранилища данных SQL, фабрики данных и хранилища BLOB-объектов 
В приложении Wingtip Tickets данные о транзакциях клиентов распределяются по нескольким базам данных. Фабрика данных Azure (ADF) используется для оркестрации извлечения, загрузки и преобразования (ELT) этих данных в хранилище данных. Чтобы наиболее эффективно загрузить данные в хранилище данных SQL, ADF извлекает данные в промежуточные файлы больших двоичных объектов, а затем загружает данные в хранилище данных с помощью [PolyBase](https://docs.microsoft.com/azure/sql-data-warehouse/design-elt-data-loading).   

На этом шаге развертываются дополнительные ресурсы, используемые в этом руководстве: хранилище данных SQL под названием _tenantanalytics_, фабрика данных Azure под названием _dbtodwload-\<пользователь\>_ и учетная запись хранения Azure под названием _wingtipstaging\<пользователь\>_ . Учетная запись хранения используется для временного хранения файлов извлеченных данных в виде больших двоичных объектов перед их загрузкой в хранилище данных. На этом шаге также развертывается схема хранилища данных и определяются конвейеры ADF, которые выполняют оркестрацию процесса ELT.
1. В интегрированной среде сценариев PowerShell откройте файл *…\Learning Modules\Operational Analytics\Tenant Analytics DW\Demo-TenantAnalyticsDW.ps1* и задайте:
    - **$DemoScenario** = **2**. Разверните хранилище данных аналитики по клиентам, хранилище BLOB-объектов и фабрику данных. 
1. Нажмите клавишу **F5**, чтобы запустить демонстрационный скрипт и развернуть ресурсы Azure. 

Теперь просмотрите развернутые ресурсы Azure.
#### <a name="tenant-databases-and-analytics-store"></a>Базы данных клиентов и хранилище аналитики
Подключитесь к серверам **tenants1-dpt-&lt;пользователь&gt;** и **catalog-dpt-&lt;пользователь&gt;** с помощью [SQL Server Management Studio (SSMS)](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms). Замените &lt;user&gt; значением, используемым при развертывании приложения. Используйте имя входа = *разработчика* и пароль = *P\@ssword1*. Дополнительные рекомендации см. в этом [ознакомительном руководстве](saas-dbpertenant-wingtip-app-overview.md).

![Подключение к серверу Базы данных SQL из SSMS](media/saas-tenancy-tenant-analytics/ssmsSignIn.JPG)

В обозревателе объектов:

1. Разверните сервер *tenants1-dpt-&lt;пользователь&gt;* .
1. Разверните узел баз данных и просмотрите список клиентских баз данных.
1. Разверните сервер *catalog-dpt-&lt;пользователь&gt;* .
1. Убедитесь, что отображается хранилище аналитики, содержащее следующие объекты:
    1. Таблицы **raw_Tickets**, **raw_Customers**, **raw_Events** и **raw_Venues** содержат необработанные извлеченные данные из баз данных клиентов.
    1. Присутствуют таблицы схемы типа "звезда" **fact_Tickets**, **dim_Customers**, **dim_Venues**, **dim_Events** и **dim_Dates**.
    1. Хранимая процедура **sp_transformExtractedData** используется для преобразования данных и загрузки их в таблицы схемы типа "звезда".

![DWtables](media/saas-tenancy-tenant-analytics/DWtables.JPG)

#### <a name="blob-storage"></a>Хранилище BLOB-объектов
1. На [портале Azure](https://ms.portal.azure.com) перейдите к группе ресурсов, используемой для развертывания приложения. Проверьте, добавлена ли учетная запись хранения с именем **wingtipstaging\<пользователь\>** .

   ![DWtables](media/saas-tenancy-tenant-analytics/adf-staging-storage.PNG)

1. Щелкните учетную запись хранения **wingtipstaging\<пользователь\>** , чтобы просмотреть объекты.
1. Щелкните плитку **Большие двоичные объекты**.
1. Щелкните контейнер **configfile**.
1. Убедитесь, что **configfile** содержит файл JSON с именем **TableConfig.json**. Этот файл содержит имена исходной и целевой таблиц, имена столбцов и имя столбца отслеживания.

#### <a name="azure-data-factory-adf"></a>Фабрика данных Azure (ADF)
Убедитесь, что на [портале Azure](https://ms.portal.azure.com) в группе ресурсов добавлена фабрика данных Azure с именем _dbtodwload-\<пользователь\>_ . 

 ![adf_portal](media/saas-tenancy-tenant-analytics/adf-data-factory-portal.png)

В этом разделе рассматривается созданная фабрика данных. Чтобы запустить фабрику данных, сделайте следующее:
1. На портале щелкните фабрику данных с именем **dbtodwload-\<пользователь\>** .
2. Щелкните плитку **Author & Monitor** (Создание и мониторинг), чтобы запустить конструктор фабрики данных на отдельной вкладке. 

## <a name="extract-load-and-transform-data"></a>Извлечение, загрузка и преобразование данных
Фабрика данных Azure используется для оркестрации извлечения, загрузки и преобразования данных. В этом руководстве будут извлекаться данные из четырех различных представлений SQL из каждой базы данных клиентов: **rawTickets**, **rawCustomers**, **rawEvents** и **rawVenues**. Эти представления содержат идентификатор места проведения, поэтому можно отличить данные из каждого места проведения в хранилище данных. Данные загружаются в соответствующие промежуточные таблицы в хранилище данных: **raw_Tickets**, **raw_customers**, **raw_Events** и **raw_Venue**. Хранимая процедура преобразует необработанные данные и заполняет таблицы схемы типа "звезда": **fact_Tickets**, **dim_Customers**, **dim_Venues**, **dim_Events**  и **dim_Dates**.

В предыдущем разделе вы развернули и инициализировали необходимые ресурсы Azure, включая фабрику данных. Развернутая фабрика данных содержит конвейеры, наборы данных, связанные службы и другое, необходимые для извлечения, загрузки и преобразования данных клиентов. Рассмотрим эти объекты подробнее, а затем активируем конвейер для перемещения данных из баз данных клиентов в хранилище данных.

### <a name="data-factory-pipeline-overview"></a>Обзор конвейера фабрики данных
В этом разделе рассматриваются объекты, созданные в фабрике данных. На следующем рисунке показан полный рабочий процесс конвейера ADF, используемый в этом руководстве. Если вы хотите изучить конвейер позже и увидеть результаты сейчас, перейдите к следующему разделу **Активация запуска конвейера**.

![adf_overview](media/saas-tenancy-tenant-analytics/adf-data-factory.PNG)

На странице обзора перейдите на вкладку **Author** (Создание) на левой панели и обратите внимание, что создано три [конвейера](https://docs.microsoft.com/azure/data-factory/concepts-pipelines-activities) и три [набора данных](https://docs.microsoft.com/azure/data-factory/concepts-datasets-linked-services).
![adf_author](media/saas-tenancy-tenant-analytics/adf_author_tab.JPG)

Ниже приведено описание трех вложенных конвейеров: SQLDBToDW, DBCopy и TableCopy.

**Конвейер 1 — SQLDBToDW** ищет имена баз данных клиентов, хранящихся в базе данных каталога (имя таблицы: [__ShardManagement].[ShardsGlobal]), и для каждой клиентской базы данных выполняет конвейер **DBCopy**. По завершении выполняется указанная схема хранимой процедуры **sp_TransformExtractedData**. Эта хранимая процедура преобразовывает загруженные данные в промежуточные таблицы и заполняет таблицы схемы типа "звезда".

**Конвейер 2 — DBCopy** ищет имена исходных таблиц и столбцов из файла конфигурации, хранящегося в хранилище BLOB-объектов.  Затем конвейер **TableCopy** выполняется для каждой из четырех таблиц: TicketFacts, CustomerFacts, EventFacts и VenueFacts. Действие **[Foreach](https://docs.microsoft.com/azure/data-factory/control-flow-for-each-activity)** выполняется в параллельном режиме для всех 20 баз данных. ADF поддерживает параллельное выполнение не более 20 итераций цикла. Рассмотрите возможность создания нескольких конвейеров для большего количества баз данных.    

**Конвейер 3 — TableCopy** использует номера версий строк в базе данных SQL (_rowversion_) для определения измененных или обновленных строк. Это действие выполняет поиск версии начальной и конечной строк для извлечения строк из исходных таблиц. Таблица **CopyTracker**, хранящаяся в каждой базе данных клиентов, отслеживает последнюю строку, извлеченную из каждой исходной таблицы при каждом запуске. Новые или измененные строки копируются в соответствующие промежуточные таблицы в хранилище данных: **raw_Tickets**, **raw_Customers**, **raw_Venues** и **raw_Events**. Наконец, версия последней строки сохраняется в таблице **CopyTracker** для использования в качестве версии начальной строки для следующего сеанса извлечения. 

Есть также три параметризованные связанные службы, которые связывают фабрику данных с исходными базами данных SQL, целевым хранилищем данных SQL и промежуточным хранилищем BLOB-объектов. На вкладке **Author** (Создание) щелкните **Подключения**, чтобы изучить связанные службы, как показано на следующем рисунке:

![adf_linkedservices](media/saas-tenancy-tenant-analytics/linkedservices.JPG)

Для трех связанных служб есть три набора данных, ссылающихся на данные, которые вы используете в действиях конвейера в качестве входных и выходных данных. Изучите используемые подключения и параметры в каждом наборе данных. _AzureBlob_ указывает на файл конфигурации, содержащий исходные и целевые таблицы и столбцы, а также столбец отслеживания в каждом источнике.
  
### <a name="data-warehouse-pattern-overview"></a>Обзор шаблона хранилища данных
Хранилище данных SQL используется в качестве хранилища аналитики для выполнения агрегирования данных клиентов. В этом примере PolyBase используется для загрузки данных в хранилище данных SQL. Необработанные данные загружаются в промежуточные таблицы, в которых есть столбец идентификаторов, используемый для отслеживания строк, преобразованных в таблицы схемы типа "звезда". На следующем рисунке показан шаблон загрузки: ![loadingpattern](media/saas-tenancy-tenant-analytics/loadingpattern.JPG)

В этом примере используются таблицы измерения SCD (медленно изменяющиеся измерения) типа 1. В каждом измерении есть суррогатный ключ, определенный с помощью столбца идентификаторов. Мы рекомендуем заранее заполнять таблицу измерения даты, чтобы сэкономить время. Для других таблиц измерений оператор Create Table As Select... (CTAS) используется для создания временной таблицы, содержащей имеющиеся измененные и неизмененные строки, а также суррогатные ключи. Это делается с помощью оператора IDENTITY_INSERT=ON. Затем новые строки вставляются в таблицу с помощью оператора IDENTITY_INSERT=OFF. Чтобы выполнить откат без проблем, имеющейся таблице измерения и временной таблице присваивается имя новой таблицы измерения. Перед каждым запуском старая таблица измерения удаляется.

Таблицы измерения загружаются перед таблицей фактов. Благодаря такой последовательности для каждого прибывающего факта уже будут существовать все ссылочные измерения. После загрузки фактов для каждого соответствующего измерения сопоставляется бизнес-ключ и соответствующие суррогатные ключи добавляются в каждый из фактов.

На последнем этапе преобразования удаляются промежуточные данные, готовые к следующему выполнению конвейера.
   
### <a name="trigger-the-pipeline-run"></a>Активация запуска конвейера
Чтобы выполнить полный конвейер извлечения, загрузки и преобразования для всех баз данных клиентов, сделайте следующее:
1. На вкладке **Author** (Создание) пользовательского интерфейса ADF выберите конвейер **SQLDBToDW** в левой области.
1. Щелкните **Активировать** и в извлеченном меню выберите **Trigger Now** (Активировать сейчас). Это действие запускается в конвейере немедленно. В сценарии для рабочей среды можно задать расписание для запуска конвейера, чтобы обновлять данные по расписанию.
  ![adf_trigger](media/saas-tenancy-tenant-analytics/adf_trigger.JPG)
1. На странице **Pipeline Run** (Запуск конвейера) щелкните **Готово**.
 
### <a name="monitor-the-pipeline-run"></a>Мониторинг конвейера
1. В пользовательском интерфейсе ADF переключитесь на вкладку **Монитор** в меню слева.
1. Нажимайте кнопку **Обновить**, пока состояние конвейера SQLDBToDW не изменится на **Успешно**.
  ![adf_monitoring](media/saas-tenancy-tenant-analytics/adf_monitoring.JPG)
1. Подключитесь к хранилищу данных с помощью SSMS и выполните запрос таблиц схемы типа "звезда", чтобы проверить, загружены ли данные в эти таблицы.

По завершении выполнения конвейера в таблице фактов будут содержаться данные о продаже билетов для всех мест проведения и таблицы измерений будут заполнены соответствующими местами проведения, событиями и клиентами.

## <a name="data-exploration"></a>Исследование данных

### <a name="visualize-tenant-data"></a>Визуализация данных клиента

В схеме типа "звезда" содержатся все данные о продаже билетов, необходимые для анализа. Графическая визуализация помогает упростить отслеживание тенденций в больших наборах данных. В этом разделе используется **Power BI** для визуализации данных клиентов в хранилище данных и их обработки.

Выполните шаги ниже, чтобы подключиться к Power BI, а также импортировать созданные ранее представления:

1. Запустите Power BI Desktop.
2. На вкладке "Главная" выберите **Получение данных**, а затем выберите **Дополнительно…** . в меню.
3. В окне **Get Data** (Получение данных) выберите **База данных SQL Azure**.
4. В окне входа в базу данных введите имя сервера (**catalog-dpt-&lt;Пользователь&gt;.database.windows.net**). Выберите **Импорт** для параметра **Режим подключения к данным**, а затем нажмите кнопку **ОК**. 

    ![sign-in-to-power-bi](./media/saas-tenancy-tenant-analytics/powerBISignIn.PNG)

5. Выберите **базы данных** в области слева, затем введите имя пользователя = *разработчика*и введите пароль = *P\@ssword1*. Щелкните **Подключить**.  

    ![database-sign-in](./media/saas-tenancy-tenant-analytics/databaseSignIn.PNG)

6. В области **Навигатор** в разделе базы данных аналитики выберите таблицы схемы типа "звезда": **fact_Tickets**, **dim_Events**, **dim_Venues**, **dim_Customers** и **dim_Dates**. Затем выберите **Загрузка**. 

Поздравляем! Вы успешно загрузили данные в Power BI. Теперь приступите к анализу визуализаций для получения подробных сведений о клиентах. Давайте подробно рассмотрим, как с помощью аналитики предоставить рекомендации на основе данных команде Wingtip Tickets. С помощью рекомендаций можно оптимизировать бизнес-модель и обслуживание клиентов.

Для начала мы проанализируем данные о продажах билетов, чтобы отследить различия в использовании для разных мест проведения мероприятий. Выберите показанные параметры в Power BI для построения линейчатой диаграммы общего числа билетов, проданных по каждому месту проведения. (Из-за случайных изменений в работе генератора билетов результаты могут отличаться.)
 
![TotalTicketsByVenues](./media/saas-tenancy-tenant-analytics/TotalTicketsByVenues-DW.PNG)

Рисунок выше подтверждает, что количество билетов, проданных по различным местам проведения, отличается. Места проведения, в которых продается больше билетов, используют службу более интенсивно. Здесь можно применить настройку выделения ресурсов в соответствии с различными потребностями клиента.

Можно глубже проанализировать данные, чтобы определить изменения в продаже билетов с течением времени. Выберите параметры, показанные на следующем рисунке в Power BI, для построения диаграммы общего числа билетов, продаваемых каждый день в течение 60 дней.
 
![SaleVersusDate](./media/saas-tenancy-tenant-analytics/SaleVersusDate-DW.PNG)

На предыдущей диаграмме отображается пик продажи билетов для некоторых мест проведения. Таким образом, можно предположить, что в некоторых местах проведения происходит непропорциональное использование системных ресурсов. Пока нет явного шаблона в отношении времени возникновения пиков.

Далее давайте рассмотрим значимость дней, когда наблюдаются пики продаж. Когда возникают пики после поступления в продажу билетов? Чтобы построить график для билетов, продаваемых за день, выберите параметры, показанные на следующем рисунке в Power BI.

![SaleDayDistribution](./media/saas-tenancy-tenant-analytics/SaleDistributionPerDay-DW.PNG)

На графике показано, что в некоторых местах проведения большее количество билетов продается в первый день. Как только билеты поступают в продажу в этих местах проведения, их начинают сразу же раскупать. Такая активность в нескольких местах проведения может повлиять на работу службы для других клиентов.

Вы можете еще раз подробно ознакомиться с данными, чтобы проверить, действительно ли наблюдается такая активность для всех мероприятий, проводимых в этих местах. На графиках выше можно заметить, что Contoso Concert Hall продает большое количество билетов, а также пик продаж билетов Contoso в определенные дни. Поэкспериментируйте с параметрами Power BI, чтобы построить график кумулятивной продажи билетов для Contoso Concert Hall, сфокусировавшись на тенденциях продаж для каждого из мероприятий. Для всех ли мероприятий используется один шаблон продаж? Попробуйте создать график, аналогичный приведенному ниже.

![ContosoSales](media/saas-tenancy-tenant-analytics/EventSaleTrends.PNG)

На этом графике кумулятивных продаж билетов с течением времени для каждого события в Contoso Concert Hall показано, что пики наблюдаются не для всех событий. Поэкспериментируйте с параметрами фильтра, чтобы отследить тенденции продаж для других мест проведения.

Подробное изучение шаблонов продаж билетов может способствовать оптимизации бизнес-модели Wingtip Tickets. Вместо взимания оплаты со всех клиентов в равной степени Wingtip, возможно, должен представить уровни служб с разными объемами вычислительных ресурсов. Более крупным местам проведения, где необходимо продавать больше билетов в день, можно предложить более высокий уровень обслуживания с расширенным соглашением об уровне обслуживания (SLA). Такие места проведения могли бы разместить свои базы данных в пуле с меньшими ограничениями в отношении ресурсов на базу данных. Для каждого уровня служб можно задать выделение ресурсов на продажи в час, а также включить дополнительную плату за превышение выделения. Крупным местам проведения с периодическими пиками продаж было бы выгодно использовать высокие уровни обслуживания, а Wingtip Tickets получила бы большую прибыль за обеспечение обслуживания.

В то же время некоторые клиенты Wingtip Tickets отмечают, что им приходится прикладывать значительные усилия, чтобы продать достаточное количество билетов и оправдать расходы на обслуживание. С этой точки зрения, вероятно, можно повысить продажи билетов для неэффективных мест проведения. Большие объемы продаж увеличили бы ценность предлагаемого обслуживания. Щелкните правой кнопкой мыши fact_Tickets и выберите **Новая мера**. Введите следующее выражение для новой меры **AverageTicketsSold**:

```
AverageTicketsSold = DIVIDE(DIVIDE(COUNTROWS(fact_Tickets),DISTINCT(dim_Venues[VenueCapacity]))*100, COUNTROWS(dim_Events))
```

Выберите следующие параметры визуализации для построения графика по процентам продаж билетов в каждом месте проведения, чтобы определить их показатели.

![AvgTicketsByVenues](media/saas-tenancy-tenant-analytics/AvgTicketsByVenues-DW.PNG)

На графике выше видно, что несмотря на то что большинство мест проведения продают более 80 % билетов, некоторым с трудом удается заполнить половину зала. Поэкспериментируйте с областью значений, чтобы выбрать максимальный или минимальный процент билетов, проданных в каждом месте проведения.

## <a name="embedding-analytics-in-your-apps"></a>Внедрение аналитики в приложения 
В этом руководстве основное внимание уделяется аналитике по всем клиентам, используемой для того, чтобы поставщики программного обеспечения лучше понимали своих клиентов. Аналитика также может предоставить сведения _клиентам_, чтобы они более эффективно управляли бизнесом самостоятельно. 

В примере Wingtip Tickets ранее была выявлена связь между продажами билетов и прогнозируемыми шаблонами. Это представление может использоваться для увеличения объемов продажи билетов в местах проведения с низкими показателями. Вероятно, здесь можно использовать методы машинного обучения для прогнозирования продаж билетов для мероприятий. Можно также смоделировать последствия изменения цен, чтобы можно было предсказать влияние предложения скидок. Power BI Embedded можно интегрировать в приложение управления событиями для визуализации прогнозируемых продаж, включая влияние скидок на общее количество проданных мест и выручки от событий с низким объемом продаж. С Power BI Embedded можно даже интегрировать фактическое применение скидки к ценам на билеты непосредственно в интерфейсе визуализации.


## <a name="next-steps"></a>Дальнейшие действия

В этом руководстве вы узнали, как:

> [!div class="checklist"]
> * развертывать хранилище данных SQL, заполненное по схеме типа "звезда", для аналитики по клиентам;
> * использовать фабрику данных Azure для извлечения данных из каждой базы данных клиентов в хранилище данных аналитики;
> * оптимизировать извлеченные данные (реорганизовать в схему типа "звезда");
> * выполнять запросы в хранилище данных аналитики; 
> * использовать Power BI для визуализации тенденций в данных по всем клиентам.

Поздравляем!

## <a name="additional-resources"></a>Дополнительные ресурсы

- Дополнительные [руководства по работе с приложением SaaS Wingtip](saas-dbpertenant-wingtip-app-overview.md#sql-database-wingtip-saas-tutorials).
