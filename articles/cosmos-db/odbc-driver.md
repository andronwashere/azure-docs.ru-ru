---
title: Подключение к Azure Cosmos DB с помощью инструментов бизнес-аналитики
description: Сведения о создании таблиц и представлений с помощью драйвера ODBC для Azure Cosmos DB, чтобы просматривать нормализованные данные в программном обеспечении для бизнес-аналитики и анализа данных.
author: SnehaGunda
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/28/2019
ms.author: sngun
ms.openlocfilehash: 352cd23f00e911b895e52aacaced1bfba38f7f84
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "66257250"
---
# <a name="connect-to-azure-cosmos-db-using-bi-analytics-tools-with-the-odbc-driver"></a>Подключение к Azure Cosmos DB с помощью драйвера ODBC с использованием инструментов бизнес-аналитики

Драйвер ODBC для Azure Cosmos DB позволяет подключаться к Azure Cosmos DB с помощью инструментов бизнес-аналитики, таких как SQL Server Integration Services, Power BI Desktop и Tableau, а также анализировать данные Azure Cosmos DB и создавать их визуализации в этих решениях.

Драйвер ODBC для Azure Cosmos DB совместим с ODBC версии 3.8 и поддерживает синтаксис ANSI SQL-92. Он обеспечивает широкие возможности для ренормализации данных в Azure Cosmos DB. Используя драйвер, вы можете представить данные в Azure Cosmos DB в виде таблиц и представлений. Драйвер позволяет выполнять операции SQL с таблицами и представлениями, включая группирование по запросам, вставкам, обновлениям и удалениям.

> [!NOTE]
> Подключение к Azure Cosmos DB с помощью драйвера ODBC в настоящее время поддерживается только для учетных записей API SQL для Azure Cosmos DB.

## <a name="why-do-i-need-to-normalize-my-data"></a>Причины для нормализации данных
Azure Cosmos DB является базой данных без схемы, которая позволяет быстро разрабатывать приложения и выполнять итерацию моделей данных без ограничений строгой схемы. Отдельная база данных Azure Cosmos DB может содержать документы JSON разной структуры. Это отлично подходит для быстрой разработки приложений. Однако если вам нужно проанализировать данные и создать отчеты на их основе, используя инструменты для анализа данных и бизнес-аналитики, часто данные необходимо сделать плоскими и применить к ним определенную схему.

Именно в этом вам поможет драйвер ODBC. С помощью драйвера ODBC теперь можно ренормализировать данные в Azure Cosmos DB в таблицы и представления в соответствии с требованиями к анализу данных и созданию отчетов. Ренормализированные схемы не влияют на базовые данные, и разработчики не обязаны им следовать. Эти схемы позволяют использовать совместимые с ODBC инструменты для доступа к данным. База данных Azure Cosmos DB придется по душе не только разработчикам, но и специалистам по обработке и анализу данных.

Давайте приступим к работе с драйвером ODBC.

## <a id="install"></a>Шаг 1. Установка драйвера ODBC для Azure Cosmos DB

1. Скачайте драйверы для своей среды:

    | Установщик | Поддерживаемые операционные системы| 
    |---|---| 
    |[Microsoft Azure Cosmos DB ODBC 64-bit.msi](https://aka.ms/cosmos-odbc-64x64) для 64-разрядной версии Windows.| 64-разрядные версии Windows 8.1 или более поздней версии, Windows 8, Windows 7, Windows Server 2012 R2, Windows Server 2012 и Windows Server 2008 R2.| 
    |[Microsoft Azure Cosmos DB ODBC 32x64-bit.msi](https://aka.ms/cosmos-odbc-32x64) для 32-разрядной версии на 64-разрядной версии Windows.| 64-разрядные версии Windows 8.1 или более поздней версии, Windows 8, Windows 7, Windows XP, Windows Vista, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 и Windows Server 2003.| 
    |[Microsoft Azure Cosmos DB ODBC 32-bit.msi](https://aka.ms/cosmos-odbc-32x32) для 32-разрядной версии Windows.|32-разрядные версии Windows 8.1 или более поздней версии, Windows 8, Windows 7, Windows XP и Windows Vista.|

    Запустите MSI-файл локально. После этого запустится **мастер установки драйвера ODBC для Microsoft Azure Cosmos DB**. 

1. Завершите работу с мастером установки, введя значения по умолчанию для установки драйвера ODBC.

1. Откройте приложение **Администратор источников данных ODBC** на своем компьютере. Это можно сделать, введя в поле поиска Windows **Источники данных ODBC**. 
    Чтобы убедиться, что драйвер установлен, щелкните вкладку **Драйверы**, на которой должен отображаться **драйвер ODBC для Microsoft Azure Cosmos DB**.

    ![Администратор источников данных ODBC для Azure Cosmos DB](./media/odbc-driver/odbc-driver.png)

## <a id="connect"></a>Шаг 2. Подключение к базе данных Azure Cosmos DB

1. [Установив драйвер ODBC для Azure Cosmos DB](#install), в окне **Администратор источников данных ODBC** щелкните **Добавить**. Вы можете создать пользовательское или системное имя источника данных. В этом примере создается пользовательское имя источника данных.

1. В окне **Создание нового источника данных** выберите **Microsoft Azure Cosmos DB ODBC Driver** (Драйвер ODBC для Microsoft Azure Cosmos DB), а затем щелкните **Готово**.

1. В окне **Azure Cosmos DB ODBC Driver SDN Setup** (Настройка имени источника данных драйвера ODBC для Azure Cosmos DB) укажите следующие сведения: 

    ![Окно настройки имени источника данных драйвера ODBC для Azure Cosmos DB](./media/odbc-driver/odbc-driver-dsn-setup.png)
    - **Имя источника данных**. Понятное имя для источника данных ODBC. Это имя является уникальным для учетной записи Azure Cosmos DB, так что при наличии нескольких учетных записей присваивайте ей уникальное имя.
    - **Описание** Краткое описание источника данных.
    - **Узел**. Универсальный код ресурса (URI) для учетной записи Azure Cosmos DB. Его можно получить на странице ключей Azure Cosmos DB на портале Azure, как показано на следующем снимке экрана. 
    - **Ключ доступа**. Первичный или вторичный ключ для чтения и записи или только для чтения на странице ключей Azure Cosmos DB на портале Azure, как показано на следующем снимке экрана. Мы рекомендуем использовать ключ только для чтения, если имя источника данных используется для обработки данных, предназначенных только для чтения, и создания отчетов на их основе.
    ![Страница ключей Azure Cosmos DB](./media/odbc-driver/odbc-driver-keys.png)
    - **Encrypt Access Key for** (Зашифровать ключ доступа для). Выберите лучший вариант, исходя из того, кто использует этот компьютер. 
    
1. Нажмите кнопку **проверки**, чтобы подключиться к учетной записи Azure Cosmos DB. 

1. Нажмите кнопку **Дополнительные параметры** и задайте следующие значения:
    - **Query Consistency** (Согласованность запросов). Выберите [уровень согласованности](consistency-levels.md) операций. Значение по умолчанию — "Сеанс".
    - **Число повторных попыток**. Введите число повторных попыток выполнения операции, если первоначальный запрос не был выполнен из-за ограничений скорости службы.
    - **Schema File** (Файл схемы). Здесь можно выбрать один из нескольких вариантов.
        - По умолчанию (если не указывать здесь сведения) драйвер сканирует данные первой страницы для всех коллекций для определения схемы каждой коллекции. Этот процесс известен как сопоставление коллекций. Так как файл схемы не определен, драйверу приходится выполнять сканирование для каждого сеанса. При этом запуск приложения с использованием имени источника данных может стать дольше. Мы рекомендуем связывать файл схемы для имени источника данных.
        - Если у вас есть файл схемы (созданный с помощью редактора схем), щелкните **Обзор**, перейдите к файлу, выберите **Сохранить** и щелкните **ОК**.
        - Если нужно создать схему, нажмите кнопку **ОК**, а затем в главном окне нажмите кнопку **Редактор схем**. Перейдите к сведениям редактора схем. После создания файла схемы не забудьте вернуться к окну **Дополнительные параметры**, чтобы добавить новый файл схемы.

1. Когда вы завершите настройку и закроете окно **настройки имени источника данных драйвера ODBC для Azure Cosmos DB**, новое пользовательское имя источника данных будет добавлено на соответствующую вкладку.

    ![Новое имя источника данных ODBC для Azure Cosmos DB на вкладке "DSN пользователя"](./media/odbc-driver/odbc-driver-user-dsn.png)

## <a id="#collection-mapping"></a>Шаг 3. Создание определения схемы с использованием метода сопоставления коллекций

Существует два типа методов выборки, которые можно использовать: **сопоставление коллекций** и **таблицы-разделители**. В сеансе выборки могут использоваться оба метода выборки, но в каждой коллекции можно использовать только один из них. Выполнив приведенные ниже действия, можно создать схему для данных в одной или нескольких коллекциях, используя метод сопоставления коллекций. При использовании этого метода выборки извлекаются данные на странице коллекции для определения структуры данных. Коллекция переносится в таблицу на стороне ODBC. Этот метод выборки действует эффективно и быстро при использовании однородных данных в коллекции. Если коллекция содержит разнородные данные, мы рекомендуем использовать [метод сопоставления таблиц-разделителей](#table-mapping), так как это более надежный метод выборки, позволяющий определить структуру данных в коллекции. 

1. Выполнив шаги 1–4 в разделе о [подключении к базе данных Azure Cosmos DB](#connect), щелкните **Редактор схем** в окне **настройки имени источника данных драйвера ODBC для Azure Cosmos DB**.

    ![Кнопка "Редактор схем" в окне настройки имени источника данных драйвера ODBC для Azure Cosmos DB](./media/odbc-driver/odbc-driver-schema-editor.png)
1. В окне **Редактор схем** нажмите кнопку **Создать**.
    Появится окно **создания схемы** со всеми коллекциями в учетной записи Azure Cosmos DB. 

1. Выберите одну или несколько коллекций для выборки и щелкните **Sample** (Выборка). 

1. На вкладке **Design View** (Конструктор) представлены базы данных, схемы и таблицы. В табличном представлении отображается набор свойств, связанных с именами столбцов ("Имя SQL", "Имя источника" и т. д.).
    Вы можете изменить имя, тип и длину SQL (если применимо), масштаб (если применимо), точность (если применимо) и параметр, допускающий значение null, для каждого столбца.
    - Для параметра **Скрыть столбец** можно задать значение **true**, если необходимо исключить этот столбец из результатов запроса. Столбцы, для параметра "Скрыть столбец" которых задано значение true, не возвращаются для выбора и проекции, несмотря на то, что они по-прежнему являются частью схемы. Например, можно скрыть все обязательные системные свойства Azure Cosmos DB, начинающиеся со знака "_".
    - Столбец **Идентификатор** — это единственное поле, которое нельзя скрыть, так как оно используется в качестве первичного ключа в нормализованной схеме. 

1. Завершив определение схемы, щелкните **Файл** | **Сохранить**, перейдите в каталог, чтобы сохранить схему, и нажмите кнопку **Сохранить**.

1. Чтобы использовать эту схему с именем источника данных, откройте **окно настройки Azure Cosmos DB ODBC Driver DSN** (через администратор источников данных ODBC), нажмите кнопку **Дополнительно**, а затем в **файл схемы** окне перейдите к сохраненной схеме. При сохранении файла схемы в существующем имени источника данных изменяется DSN-соединение в соответствии с данными и структурой, определенными схемой.

## <a id="table-mapping"></a>Шаг 4. Создание определения схемы с использованием метода таблиц-разделителей

Существует два типа методов выборки, которые можно использовать: **сопоставление коллекций** и **таблицы-разделители**. В сеансе выборки могут использоваться оба метода выборки, но в каждой коллекции можно использовать только один из них. 

Выполнив следующие действия, можно создать схему для данных в одной или нескольких коллекциях, используя метод **таблиц-разделителей**. Мы рекомендуем использовать этот метод выборки, если коллекции содержат разнородные данные. С помощью этого метода можно ограничить выборку набором атрибутов и соответствующими значениями. Например, если документ содержит свойство "Тип", можно ограничить выборку значениями этого свойства. В результате выборки вы получите набор таблиц для каждого значения выбранного типа. Например, если выбрать значение "Автомобиль", будет создана таблица со сведениями об автомобилях, а если выбрать значение "Самолет", будет создана таблица со сведениями о самолетах.

1. Выполнив шаги 1–4 в разделе о [подключении к базе данных Azure Cosmos DB](#connect), щелкните **Редактор схем** в окне настройки имени источника данных драйвера ODBC для Azure Cosmos DB.

1. В окне **Редактор схем** нажмите кнопку **Создать**.
    Появится окно **создания схемы** со всеми коллекциями в учетной записи Azure Cosmos DB. 

1. Выберите коллекцию на вкладке **Представление-пример** в столбце **Определение сопоставления** для коллекции и щелкните **Изменить**. Затем в окне **Mapping Definition** (Определение сопоставления) выберите метод **Table Delimiters** (Таблицы-разделители). После этого выполните описанные ниже действия.

    a. В поле **Атрибуты** введите имя свойства разделителя. Это свойство документа, которым нужно ограничить выборку (например, "Город"). Затем нажмите клавишу ВВОД. 

    2\. Если требуется ограничить выборку только конкретными значениями введенного атрибута, выберите атрибут в списке, введите значение в поле **Значение** (например "Сиэтл") и нажмите клавишу ВВОД. Вы можете добавить еще несколько значений для атрибутов. При вводе значения убедитесь, что выбран правильный атрибут.

    Например, если добавить значение "Город" для **атрибута** и при этом нужно, чтобы таблица содержала только строки со значением города Нью-Йорк и Дубаи, в поле "Атрибуты" следует ввести "Город", а "Нью-Йорк" и "Дубаи" — в поле **Значения**.

1. Последовательно выберите **ОК**. 

1. Завершив работу с определениями сопоставлений для коллекций, по которым нужно выполнить выборку, в окне **Редактор схем** щелкните **Sample** (Выборка).
     Вы можете изменить имя, тип и длину SQL (если применимо), масштаб (если применимо), точность (если применимо) и параметр, допускающий значение null, для каждого столбца.
    - Для параметра **Скрыть столбец** можно задать значение **true**, если необходимо исключить этот столбец из результатов запроса. Столбцы, для параметра "Скрыть столбец" которых задано значение true, не возвращаются для выбора и проекции, несмотря на то, что они по-прежнему являются частью схемы. Например, можно скрыть все обязательные системные свойства Azure Cosmos DB, начинающиеся со знака `_`.
    - Столбец **Идентификатор** — это единственное поле, которое нельзя скрыть, так как оно используется в качестве первичного ключа в нормализованной схеме. 

1. Завершив определение схемы, щелкните **Файл** | **Сохранить**, перейдите в каталог, чтобы сохранить схему, и нажмите кнопку **Сохранить**.

1. В окне **настройки имени источника данных драйвера ODBC для Azure Cosmos DB** щелкните **Дополнительные параметры**. Затем в поле **Schema File** (Файл схемы) перейдите к сохраненному файлу схемы и нажмите кнопку **ОК**. Нажмите кнопку **ОК** еще раз, чтобы сохранить имя источника данных. Таким образом сохраняется схема, созданная для имени источника данных. 

## <a name="optional-set-up-linked-server-connection"></a>(Необязательно.) Настройка подключения связанного сервера

Можно отправить запрос к базе данных Azure Cosmos DB из среды SQL Server Management Studio (SSMS), настроив подключение связанного сервера.

1. Создайте системный источник данных, как описано на [шаге 2](#connect), например, с именем `SDS Name`.

1. [Установите SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) и подключитесь к первому серверу SQL Server. 

1. В редакторе запросов SSMS создайте объект связанного сервера `DEMOCOSMOS` для источника данных с помощью следующих команд. Замените `DEMOCOSMOS` именем связанного сервера, а `SDS Name` — именем системного источника данных.

    ```sql
    USE [master]
    GO
    
    EXEC master.dbo.sp_addlinkedserver @server = N'DEMOCOSMOS', @srvproduct=N'', @provider=N'MSDASQL', @datasrc=N'SDS Name'
    
    EXEC master.dbo.sp_addlinkedsrvlogin @rmtsrvname=N'DEMOCOSMOS', @useself=N'False', @locallogin=NULL, @rmtuser=NULL, @rmtpassword=NULL
    
    GO
    ```
    
Чтобы увидеть имя нового связанного сервера, обновите список связанных серверов.

![Связанный сервер в среде SSMS](./media/odbc-driver/odbc-driver-linked-server-ssms.png)

### <a name="query-linked-database"></a>Запрос к связанной базе данных

Чтобы запросить связанную базу данных, введите запрос SSMS. В этом примере запрос выбирает данные из таблицы в коллекции с именем `customers`:

```sql
SELECT * FROM OPENQUERY(DEMOCOSMOS, 'SELECT *  FROM [customers].[customers]')
```

Выполните запрос. Результат должен выглядеть следующим образом:

```
attachments/  1507476156    521 Bassett Avenue, Wikieup, Missouri, 5422   "2602bc56-0000-0000-0000-59da42bc0000"   2015-02-06T05:32:32 +05:00 f1ca3044f17149f3bc61f7b9c78a26df
attachments/  1507476156    167 Nassau Street, Tuskahoma, Illinois, 5998   "2602bd56-0000-0000-0000-59da42bc0000"   2015-06-16T08:54:17 +04:00 f75f949ea8de466a9ef2bdb7ce065ac8
attachments/  1507476156    885 Strong Place, Cassel, Montana, 2069       "2602be56-0000-0000-0000-59da42bc0000"   2015-03-20T07:21:47 +04:00 ef0365fb40c04bb6a3ffc4bc77c905fd
attachments/  1507476156    515 Barwell Terrace, Defiance, Tennessee, 6439     "2602c056-0000-0000-0000-59da42bc0000"   2014-10-16T06:49:04 +04:00      e913fe543490432f871bc42019663518
attachments/  1507476156    570 Ruby Street, Spokane, Idaho, 9025       "2602c156-0000-0000-0000-59da42bc0000"   2014-10-30T05:49:33 +04:00 e53072057d314bc9b36c89a8350048f3
```

> [!NOTE]
> Связанный сервер Cosmos DB не поддерживает четырехкомпонентные имена. Возвращается сообщение об ошибке примерно следующего содержания:

```
Msg 7312, Level 16, State 1, Line 44

Invalid use of schema or catalog for OLE DB provider "MSDASQL" for linked server "DEMOCOSMOS". A four-part name was supplied, but the provider does not expose the necessary interfaces to use a catalog or schema.
``` 

## <a name="optional-creating-views"></a>Создание представлений (необязательно)
В ходе выборки можно определять и создавать представления. Эти представления аналогичны представлениям SQL. Они доступны только для чтения и ограничены выбранными элементами и проекциями определенного SQL-запроса Azure Cosmos DB. 

Чтобы создать представление данных, в окне **Редактор схем** в столбце **Определения представлений** щелкните **Добавить** в строке коллекции для выборки. 
    ![Создание представления данных](./media/odbc-driver/odbc-driver-create-view.png)


Затем в окне **Определения представлений** сделайте следующее:

1. Щелкните **Создать**, введите имя представления, например EmployeesfromSeattleView, и нажмите кнопку **ОК**.

1. В окне **Изменить представление** введите запрос Azure Cosmos DB. Это должен быть [SQL-запрос Azure Cosmos DB](how-to-sql-query.md), например `SELECT c.City, c.EmployeeName, c.Level, c.Age, c.Manager FROM c WHERE c.City = "Seattle"`. Затем нажмите кнопку **ОК**.

    ![Добавление запроса при создании представления](./media/odbc-driver/odbc-driver-create-view-2.png)


Количество создаваемых представлений не ограничено. Завершив определение представлений, можно приступить к выборке данных. 

## <a name="step-5-view-your-data-in-bi-tools-such-as-power-bi-desktop"></a>Шаг 5. Просмотр данных в инструментах бизнес-аналитики, таких как Power BI Desktop

Используя новое имя источника данных, можно подключиться к Azure Cosmos DB с помощью любых инструментов, совместимых с ODBC. На этом шаге показано, как подключиться к Power BI Desktop и создать визуализацию Power BI.

1. Откройте Power BI Desktop.

1. Щелкните **Получить данные**.

    ![Получение данных в Power BI Desktop](./media/odbc-driver/odbc-driver-power-bi-get-data.png)

1. В окне **Получить данные** щелкните **Другие** | **ODBC** | **Подключиться**.

    ![Выбор источника данных ODBC в окне получения данных Power BI](./media/odbc-driver/odbc-driver-power-bi-get-data-2.png)

1. В окне **Из ODBC** выберите созданное имя источника данных и нажмите кнопку **ОК**. Поля на вкладке **Дополнительные параметры** можно не заполнять.

    ![Выбор имени источника данных (DSN) в окне получения данных Power BI](./media/odbc-driver/odbc-driver-power-bi-get-data-3.png)

1. В окне **Access a data source using an ODBC driver** (Получение доступа к источнику данных с помощью драйвера ODBC) выберите параметр **По умолчанию или пользовательский** и щелкните **Подключиться**. Не нужно добавлять **свойства строки подключения учетных данных**.

1. В окне **Навигатор** на панели слева разверните базу данных, схему и выберите таблицу. В область результатов данные добавляются по созданной схеме.

    ![Выбор таблицы в окне получения данных Power BI](./media/odbc-driver/odbc-driver-power-bi-get-data-4.png)

1. Чтобы визуализировать данные в Power BI Desktop, установите флажок рядом с именем таблицы и щелкните **Загрузить**.

1. В Power BI Desktop слева выберите вкладку "Данные" ![вкладка "Данные" в Power BI Desktop,](./media/odbc-driver/odbc-driver-data-tab.png) чтобы убедиться, что данные импортированы.

1. Теперь с помощью Power BI можно создавать визуальные элементы. Для этого щелкните вкладку "Отчет" ![вкладка "Отчет" в Power BI Desktop](./media/odbc-driver/odbc-driver-report-tab.png), выберите **Новый визуальный объект** и настройте элемент. Дополнительные сведения о создании визуализаций в Power BI Desktop см. в статье [Типы визуализаций в Power BI](https://powerbi.microsoft.com/documentation/powerbi-service-visualization-types-for-reports-and-q-and-a/).

## <a name="troubleshooting"></a>Устранение неполадок

Если появляется следующая ошибка, убедитесь, что значения параметров **Узел** и **Ключ доступа**, скопированные на портале Azure на [шаге 2](#connect), правильные, и повторите попытку. Чтобы скопировать значения без ошибок, нажмите кнопки копирования справа от значений параметров **Узел** и **Ключ доступа** на портале Azure.

    [HY000]: [Microsoft][Azure Cosmos DB] (401) HTTP 401 Authentication Error: {"code":"Unauthorized","message":"The input authorization token can't serve the request. Please check that the expected payload is built as per the protocol, and check the key being used. Server used the following payload to sign: 'get\ndbs\n\nfri, 20 jan 2017 03:43:55 gmt\n\n'\r\nActivityId: 9acb3c0d-cb31-4b78-ac0a-413c8d33e373"}`

## <a name="next-steps"></a>Дальнейшие действия

Дополнительные сведения об Azure Cosmos DB см. в статье [Добро пожаловать в базу данных Azure Cosmos DB](introduction.md).
