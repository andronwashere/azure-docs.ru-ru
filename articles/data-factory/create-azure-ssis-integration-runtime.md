---
title: Создание среды выполнения интеграции Azure-SSIS в фабрике данных Azure | Документация Майкрософт
description: Узнайте, как создать среду выполнения интеграции Azure-SSIS в фабрике данных Azure, чтобы запускать и развертывать пакеты SSIS в Azure.
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 06/26/2019
author: swinarko
ms.author: sawinark
ms.reviewer: douglasl
manager: craigg
ms.openlocfilehash: fb5335c8dfd94006ba3f0d8d6b890869dd9f3717
ms.sourcegitcommit: 9b80d1e560b02f74d2237489fa1c6eb7eca5ee10
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/01/2019
ms.locfileid: "67484826"
---
# <a name="create-azure-ssis-integration-runtime-in-azure-data-factory"></a>Создание среды выполнения интеграции Azure-SSIS в фабрике данных Azure

Этом руководстве представлены шаги по подготовке среду Azure-SQL Server Integration Services (SSIS) выполнения интеграции (IR) в фабрике данных Azure (ADF). Поддерживает Azure-SSIS IR выполнение пакетов, развернутых в каталоге служб SSIS (SSISDB), расположенных на сервере базы данных SQL Azure или управляемого экземпляра (модель развертывания проекта) и тех, которые развертываются в системах файл файлы общих папок или Azure (модель развертывания пакета). После подготовки среды выполнения Интеграции Azure SSIS можно затем использовать знакомые средства, например SQL Server Data Tools (SSDT) или SQL Server Management Studio (SSMS) и командной строки служебных программ, таких как `dtinstall` / `dtutil` / `dtexec`, развертывание и запуск пакетов в Azure.

В разделе [Руководство. Подготовка Azure SSIS IR](tutorial-create-azure-ssis-runtime-portal.md) показано, как создать среду IR Azure SSIS с помощью приложения портала/ADF-Azure и при необходимости использовать базы данных SQL server или управляемого экземпляра для размещения SSISDB. Эта статья дополняет руководство и содержит следующие инструкции:

- При необходимости используйте сервер базы данных SQL Azure с помощью виртуальной сети службы конечные точки или управляемого экземпляра в виртуальной сети для размещения SSISDB. Перед началом необходимо настроить разрешения или параметры виртуальной сети для вашей среды выполнения Интеграции Azure SSIS для подключения к виртуальной сети.

- При необходимости используйте проверку подлинности Azure Active Directory (AAD) с управляемым удостоверением, для вашей фабрики данных AZURE для подключения к базе данных SQL server или управляемого экземпляра. Перед началом необходимо будет добавить управляемое удостоверение для ADF-ФАЙЛЕ в качестве пользователя базы данных, которая позволяет создавать SSISDB.

## <a name="overview"></a>Обзор

В этой статье показаны различные способы подготовки среды выполнения интеграции Azure SSIS IR.

- [портал Azure](#azure-portal)
- [Azure PowerShell](#azure-powershell)
- [Шаблон Azure Resource Manager](#azure-resource-manager-template)

## <a name="prerequisites"></a>Технические условия

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

- **Подписка Azure**. Если у вас нет подписки, вы можете [создать бесплатную пробную учетную запись](https://azure.microsoft.com/pricing/free-trial/).
- **Azure базы данных SQL server или управляемого экземпляра (необязательно)** . Если у вас еще нет сервера базы данных, создайте на портале Azure перед началом работы. ADF в свою очередь создаст SSISDB на этом сервере базы данных. Мы рекомендуем создать сервер базы данных в одном регионе Azure со средой интеграции. Эта конфигурация позволяет среде выполнения интеграции записывать журналы выполнения в SSISDB, не пересекая регионы Azure. 
    - В зависимости от выбранного сервера базы данных SSISDB может быть создана от вашего имени как отдельная база данных, а также как часть эластичного пула или Управляемого экземпляра. Доступ к ней можно получить через общедоступную сеть или после присоединения к виртуальной сети. Рекомендации по выбору типа сервера базы данных для размещения SSISDB см. в разделе [одной базы данных сравнения базы данных SQL Azure: гибкое пула или управляемого экземпляра](../data-factory/create-azure-ssis-integration-runtime.md#compare-sql-database-single-databaseelastic-pool-and-sql-database-managed-instance). При использовании сервера базы данных SQL Azure с помощью виртуальной сети службы конечные точки или управляемого экземпляра в виртуальной сети для размещения SSISDB или требуется доступ к локальным данным, вам нужно присоединить среду выполнения Интеграции Azure SSIS к виртуальной сети, см. в разделе [соединения Azure-SSIS IR к Виртуальная сеть](https://docs.microsoft.com/azure/data-factory/join-azure-ssis-integration-runtime-virtual-network).
    - Убедитесь, что для сервера базы данных включен параметр **Разрешить доступ к службам Azure**. Это не применимо, при использовании сервера базы данных SQL Azure с помощью виртуальной сети службы конечные точки или управляемого экземпляра в виртуальной сети для размещения SSISDB. Дополнительные сведения см. в разделе [Создание правила брандмауэра на уровне сервера с помощью портала Azure](../sql-database/sql-database-security-tutorial.md#create-firewall-rules). Сведения о включении этого параметра с помощью PowerShell см. в статье [New-AzSqlServerFirewallRule](/powershell/module/az.sql/new-azsqlserverfirewallrule).
    - Добавьте IP-адрес клиентского компьютера или диапазон IP-адресов, который включает IP-адрес клиентского компьютера, список клиентских IP-адресов в параметрах брандмауэра сервера базы данных. Дополнительные сведения см. в разделе [Правила брандмауэра уровня сервера Базы данных SQL Azure и уровня базы данных SQL Azure](../sql-database/sql-database-firewall-configure.md).
    - Можно подключиться к серверу базы данных, с помощью проверки подлинности SQL с вашими учетными данными администратора сервера или проверки подлинности Azure Active Directory (AAD) с управляемым удостоверением для ADF-ФАЙЛЕ.  Для последнего необходимо добавить управляемое удостоверение для ADF-ФАЙЛЕ в группу AAD, разрешения на доступ к серверу базы данных, см. в разделе [AAD включить проверку подлинности для Azure-SSIS IR](https://docs.microsoft.com/azure/data-factory/enable-aad-authentication-azure-ssis-ir).
    - Убедитесь, что серверу базы данных имеют SSISDB уже. Подготовка среды выполнения Интеграции Azure SSIS не поддерживает использование существующих SSISDB.
- **Виртуальная сеть Azure Resource Manager (необязательно)** . Виртуальная сеть Azure Resource Manager нужна, если выполняется хотя бы одно из следующих условий.
    - Вы размещаете SSISDB на сервере базы данных SQL Azure с помощью виртуальной сети службы конечные точки или управляемого экземпляра в виртуальной сети.
    - Вы хотите подключиться к локальным хранилищам данных из пакетов SSIS, работающих в среде выполнения интеграции Azure-SSIS.
- **Azure PowerShell (необязательно)** . Следуйте инструкциям на [описывается установка и настройка Azure PowerShell](/powershell/azure/install-az-ps), если вы хотите запустить сценарий PowerShell для подготовки среды выполнения интеграции Azure SSIS

### <a name="region-support"></a>Поддержка регионов

Список регионов Azure, в которых сейчас доступны ADF и среда выполнения интеграции Azure-SSIS, см. на странице [Доступность ADF и среды выполнения интеграции Azure-SSIS по регионам](https://azure.microsoft.com/global-infrastructure/services/?products=data-factory&regions=all).

### <a name="compare-sql-database-single-databaseelastic-pool-and-sql-database-managed-instance"></a>Сравнение отдельной Базы данных SQL или пула эластичных баз данных и Управляемого экземпляра Базы данных SQL

В следующей таблице приведены сравнения некоторых функций сервера Базы данных SQL Azure и управляемого экземпляра, так как они связаны со средой выполнения интеграции Azure-SSIR.

| Функция | Отдельная база данных / эластичный пул| Управляемый экземпляр |
|---------|--------------|------------------|
| **Планирование** | Агент SQL Server недоступен.<br/><br/>См. раздел [Планирование выполнения пакета в конвейере ADF](https://docs.microsoft.com/sql/integration-services/lift-shift/ssis-azure-schedule-packages?view=sql-server-2017#activity).| Доступен агент управляемого экземпляра. |
| **Проверка подлинности** | Вы можете создать SSISDB, выбрав пользователя автономной базы данных, представляющего любую группу AAD с управляемым удостоверением ADF, в качестве члена роли **db_owner**.<br/><br/>См. раздел [Использование проверки подлинности Azure AD для создания SSISDB на сервере Базы данных SQL Azure](enable-aad-authentication-azure-ssis-ir.md#enable-azure-ad-on-azure-sql-database). | Вы можете создать SSISDB, выбрав пользователя автономной базы данных, представляющего управляемое удостоверение ADF. <br/><br/>См. раздел [Использование проверки подлинности Azure AD для создания SSISDB в управляемом экземпляре Базы данных SQL Azure](enable-aad-authentication-azure-ssis-ir.md#enable-azure-ad-on-azure-sql-database-managed-instance). |
| **Уровень служб** | При создании среды выполнения интеграции Azure-SSIS на сервере Базы данных SQL Azure для SSISDB можно выбрать уровень служб. Существует несколько уровней служб. | При создании среды выполнения интеграции Azure-SSIS в управляемом экземпляре уровень служб для SSISDB выбрать невозможно. Все базы данных одного управляемого экземпляра используют один и тот же ресурс, который ему выделен. |
| **Виртуальная сеть** | Если вы используете сервер базы данных SQL Azure с конечными точками служб для виртуальной сети или вам нужен доступ к локальным хранилищам данных, для присоединения Azure-SSIS поддерживаются только виртуальные сети Azure Resource Manager. | Для присоединения среды выполнения интеграции Azure-SSIS поддерживаются только виртуальные сети Azure Resource Manager. Виртуальная сеть требуется во всех случаях.<br/><br/>Если среда выполнения интеграции Azure-SSIS присоединяется к виртуальной сети управляемого экземпляра, используйте для них разные подсети. Если среда выполнения интеграции Azure-SSIS присоединяется к другой виртуальной сети, отличной от сети управляемого экземпляра, используйте пиринг между виртуальными сетями или подключение типа "виртуальная сеть — виртуальная сеть". См. статью [Подключение приложения к Управляемому экземпляру Базы данных SQL Azure](../sql-database/sql-database-managed-instance-connect-app.md) |
| **Распределенные транзакции** | Поддержка с помощью эластичных транзакций. Транзакции координатора распределенных транзакций (MSDTC) не поддерживаются. Если ваши пакеты SSIS используют MSDTC для координации распределенных транзакций, рекомендуем перейти на эластичные транзакции для Базы данных SQL Azure. Дополнительные сведения см. в статье [Распределенные транзакции по облачным базам данных](../sql-database/sql-database-elastic-transactions-overview.md). | Не поддерживается. |
| | | |

## <a name="azure-portal"></a>Портал Azure

В этом разделе для создания среды выполнения интеграции Azure-SSIS используется портал Azure, в частности пользовательский интерфейс или приложение ADF.

### <a name="create-a-data-factory"></a>Создание фабрики данных

1. Запустите веб-браузер **Microsoft Edge** или **Google Chrome**. Сейчас только эти браузеры поддерживают пользовательский интерфейс фабрики данных.
1. Войдите на [портале Azure](https://portal.azure.com/).
1. В меню слева щелкните **Создать**, выберите **Данные+аналитика** и щелкните **Фабрика данных**.

   ![Создать -> Фабрика данных](./media/tutorial-create-azure-ssis-runtime-portal/new-data-factory-menu.png)

1. На странице **Новая фабрика данных** введите **MyAzureSsisDataFactory** в поле **Имя**.

   ![Страница "Новая фабрика данных"](./media/tutorial-create-azure-ssis-runtime-portal/new-azure-data-factory.png)

   Имя фабрики данных Azure должно быть **глобально уникальным**. Если вы получите указанную ниже ошибку, введите другое имя фабрики данных (например, ваше_имя_MyAzureSsisDataFactory) и попробуйте создать фабрику данных снова. Ознакомьтесь со статьей [Фабрика данных Azure — правила именования](naming-rules.md), чтобы узнать правила именования для артефактов службы "Фабрика данных".

   `Data factory name “MyAzureSsisDataFactory” is not available`

1. Выберите **подписку** Azure, в рамках которой вы хотите создать фабрику данных.
1. Для **группы ресурсов** выполните одно из следующих действий.

   - Выберите **Использовать существующую**и укажите существующую группу ресурсов в раскрывающемся списке.
   - Выберите **Создать новую**и укажите имя группы ресурсов.

   Сведения о группах ресурсов см. в статье, где описывается [использование групп ресурсов для управления ресурсами Azure](../azure-resource-manager/resource-group-overview.md).

1. Укажите **V2** при выборе **версии**.
1. Укажите **расположение** фабрики данных. В списке отображаются только те расположения, в которых можно создать фабрики данных.
1. Кроме того, установите флажок **Закрепить на панели мониторинга**.
1. Нажмите кнопку **Создать**.
1. На панели мониторинга вы увидите приведенный ниже элемент с состоянием **Deploying data factory** (Развертывание фабрики данных).

    ![Элемент Deploying data factory (Развертывание фабрики данных)](media/tutorial-create-azure-ssis-runtime-portal/deploying-data-factory.png)

1. Когда завершится создание, откроется страница **Фабрика данных**, как показано на рисунке ниже.

    ![Домашняя страница фабрики данных](./media/tutorial-create-azure-ssis-runtime-portal/data-factory-home-page.png)

1. Щелкните **Создание и мониторинг**, чтобы открыть на отдельной вкладке пользовательский интерфейс фабрики данных.

### <a name="provision-an-azure-ssis-integration-runtime"></a>Подготовка среды выполнения интеграции Azure SSIS

1. На странице начала работы щелкните плитку **Configure SSIS Integration Runtime** (Настройка среды выполнения интеграции SSIS).

   ![Плитка настройки среды выполнения интеграции SSIS](./media/tutorial-create-azure-ssis-runtime-portal/configure-ssis-integration-runtime-tile.png)

2. На странице **General Settings** (Общие параметры) для **настройки среды выполнения интеграции** выполните следующие действия:

   ![Общие параметры](./media/tutorial-create-azure-ssis-runtime-portal/general-settings.png)

    a. В поле **Имя** введите имя среды выполнения интеграции.

    2\. В поле **Описание** введите описание среды выполнения интеграции.

    c. В поле **Расположение** введите расположение среды выполнения интеграции. Здесь отображаются только поддерживаемые расположения. Для узла SSISDB сервера базы данных рекомендуется выбирать одинаковые расположения.

    d. В раскрывающемся списке **Размер узла** выберите размер узла кластера среды выполнения интеграции. Отображаются только поддерживаемые размеры узла. Если планируется использовать большое количество компьютеров или памяти ресурсоемких пакетов, необходимо выбрать большой размер узла (увеличение масштаба).

    д. В поле **Номер узла** введите количество узлов в кластере среды выполнения интеграции. Отображается только поддерживаемое число узлов. Если в параллельном режиме требуется запустить несколько пакетов, необходимо выбрать больной кластер с большим числом узлов (с поддержкой горизонтального масштабирования).

    Е. В раскрывающемся списке **Edition/License** (Выпуск/лицензия) выберите выпуск SQL Server или лицензию SQL Server для среды выполнения интеграции: Standard или Enterprise. Если в среде выполнения интеграции необходимо использовать дополнительные функции или функции, которые доступны в версии "Премиум", выберите SQL Server Enterprise.

    ж. В поле **Экономия** для параметра "Преимущество гибридного использования Azure" выберите один из вариантов: "Да" или "Нет". Если для получения преимуществ и экономии затрат необходимо использовать собственную лицензию SQL Server вместе с программой Software Assurance, выберите "Да".

    h. Нажмите кнопку **Далее**.

3. На странице **SQL Settings** (Параметры SQL) сделайте следующее:

   ![Раздел "SQL Settings" (Параметры SQL)](./media/tutorial-create-azure-ssis-runtime-portal/sql-settings.png)

    a. На **каталога служб SSIS, создание...**  флажок, выберите модель развертывания пакетов для запуска на среду выполнения Интеграции Azure SSIS: Модель развертывания проекта, где пакеты развертываются в SSISDB, размещенной на сервере базы данных или модель развертывания пакета, где пакеты развертываются в файл системы/файл общие папки/Azure файлы. Если проверить его, необходимо перевести серверу базы данных для размещения SSISDB, мы создадим и управлять ими от вашего имени.
   
    2\. В раскрывающемся списке **Подписка** выберите подписку Azure, которая соответствует серверу базы данных узла SSISDB. 

    c. В раскрывающемся списке **Расположение** выберите расположение сервера базы данных узла SSISDB. Для среды выполнения интеграции рекомендуется выбирать одинаковые расположения. 

    d. В раскрывающемся списке **Catalog Database Server Endpoint** (Конечная точка сервера базы данных каталога) выберите конечную точку сервера базы данных узла SSISDB. В зависимости от выбранного сервера базы данных SSISDB может быть создана от вашего имени как отдельная база данных, а также как часть эластичного пула или Управляемого экземпляра. Доступ к ней можно получить через общедоступную сеть или после присоединения к виртуальной сети. Рекомендации по выбору типа сервера базы данных для размещения SSISDB см. в разделе [одной базы данных сравнения базы данных SQL Azure: гибкое пула или управляемого экземпляра](../data-factory/create-azure-ssis-integration-runtime.md#compare-sql-database-single-databaseelastic-pool-and-sql-database-managed-instance). При выборе сервера базы данных SQL Azure с помощью конечных точек службы виртуальной сети или управляемого экземпляра в виртуальной сети для размещения SSISDB или требуется доступ к локальным данным, вам нужно присоединить среду выполнения Интеграции Azure SSIS к виртуальной сети, см. в разделе [соединения Azure-SSIS IR к виртуальной сети](https://docs.microsoft.com/azure/data-factory/join-azure-ssis-integration-runtime-virtual-network). 

    д. С помощью флажка в поле **Use AAD authentication** (Использовать проверку подлинности AAD) выберите метод проверки подлинности для сервера базы данных, на котором будет размещаться SSISDB: Проверка подлинности SQL или проверку подлинности AAD с управляемым удостоверением для ADF-ФАЙЛЕ. Если проверить его, необходимо добавить управляемое удостоверение для ADF-ФАЙЛЕ в группу AAD, разрешения на доступ к серверу базы данных, см. в разделе [AAD включить проверку подлинности для Azure-SSIS IR](https://docs.microsoft.com/azure/data-factory/enable-aad-authentication-azure-ssis-ir). 

    Е. В поле **Имя администратора** введите имя входа SQL для проверки подлинности базы данных сервера узла SSISDB. 

    ж. В поле **Пароль администратора** введите пароль входа SQL для проверки подлинности базы данных сервера узла SSISDB. 

    h. В поле **Catalog Database Service Tier** (Каталог уровней служб базы данных) выберите уровень служб для сервера базы данных, на котором будет размещаться SSISDB: Basic (Базовый), Standard (Стандартный), Premium (Премиум) или укажите имя эластичного пула. 

    i. Нажмите копку **Проверить подключение**, и если проверка прошла успешно, щелкните **Далее**. 

4. На странице **Дополнительные параметры** сделайте следующее:

    ![Дополнительные параметры](./media/tutorial-create-azure-ssis-runtime-portal/advanced-settings.png)

    a. В раскрывающемся списке **Maximum Parallel Executions Per Node** (Максимальное количество параллельных выполнений на каждом узле) выберите максимальное количество пакетов, которые будут выполняться одновременно на один узел кластера интегрированной среды выполнения. Отображается только поддерживаемое число пакетов. Если возникла необходимость использовать больше чем одно ядро для запуска одного большого пакета или пакета, обработка которого отнимает много вычислительных ресурсов или памяти, выберите меньшее число. Если возникла необходимость запуска одного или нескольких небольших пакетов, которые не потребляют много ресурсов при запуске одним ядром, выберите число побольше.

    2\. Дополнительные сведения о необязательном для заполнения поле **Custom Setup Container SAS URI** (Пользовательская настройка контейнера URI SAS), в которое можно ввести подписанный URL-адрес универсального кода ресурса (URI) контейнера Azure Storage Blob, в котором хранятся связанные с ним файлы, можно узнать из статьи [Пользовательская установка для среды выполнения интеграции Azure MSSQL Integration Services](https://docs.microsoft.com/azure/data-factory/how-to-configure-azure-ssis-ir-custom-setup).

5. После установки флажка в поле **Select a virtual network...** (Выбрать виртуальную сеть…) необходимо определиться, будет ли среда выполнения интеграции присоединена к виртуальной сети. Проверьте его, если вы используете сервер базы данных SQL Azure с помощью виртуальной сети службы конечные точки или управляемого экземпляра в виртуальной сети для размещения SSISDB или требуется доступ к локальным данным; то есть у локальной источники и назначения данных в пакетах служб SSIS, см. в разделе [присоединить Azure SSIS IR к виртуальной сети](https://docs.microsoft.com/azure/data-factory/join-azure-ssis-integration-runtime-virtual-network). Если вы установили его, выполните следующие действия:

   ![Дополнительные параметры с использованием виртуальной сети](./media/tutorial-create-azure-ssis-runtime-portal/advanced-settings-vnet.png)

    a. В поле **Подписка** выберите подписку Azure, в которой расположена виртуальная сеть.

    2\. В поле **Расположение** введите то же расположение среды выполнения интеграции.

    c. В поле **Type** (Тип) выберите тип виртуальной сети: Classic (Классическая) или Azure Resource Manager. Рекомендуется выбрать виртуальную сеть Azure Resource Manager, так как поддержка классических виртуальных сетей скоро будет прекращена.

    d. В поле **Имя виртуальной сети** выберите имя вашей виртуальной сети. Этой виртуальной сети должен быть совпадает с используемым для сервера базы данных SQL Azure с помощью виртуальной сети службы конечные точки или управляемого экземпляра в виртуальной сети для размещения SSISDB или подключенный к локальной сети.

    д. В поле **Имя подсети** выберите имя подсети в виртуальной сети. Это должно быть в разных подсетях, используемого для управляемого экземпляра в виртуальной сети для размещения SSISDB.

6. Щелкните **VNet Validation** (Проверка виртуальной сети) и в случае успеха нажмите кнопку**Готово**, чтобы начать создание вашей среды выполнения интеграции Azure-SSIS.

    > [!NOTE]
    > Исключение любое время пользовательской установки этот процесс должен быть завершен в течение пяти минут, но может занять около 20 – 30 минут для Azure-SSIS IR, присоединение к виртуальной сети.
    >
    > Если вы используете SSISDB, служба фабрики данных AZURE подключится к серверу базы данных для подготовки SSISDB. Он также настраивает разрешения и параметры для виртуальной сети, если указан и соединяет среду выполнения Интеграции Azure SSIS к виртуальной сети.
    > 
    > При подготовке IR Azure SSIS, также устанавливается пакет дополнительных компонентов Azure для служб SSIS и распространяемый компонент Access. Эти компоненты обеспечивают подключение к файлам Excel и Access и различных источников данных Azure, а также источники данных, уже поддерживаются встроенными компонентами. Можно также установить дополнительные компоненты, см. в разделе [Выборочная установка Azure-SSIS IR](how-to-configure-azure-ssis-ir-custom-setup.md).

7. В окне **Connections** (Подключения) перейдите на **среду выполнения интеграции**. Чтобы обновить состояние, щелкните **Refresh** (Обновить).

   ![Состояние создания](./media/tutorial-create-azure-ssis-runtime-portal/azure-ssis-ir-creation-status.png)

8. С помощью ссылок в столбце **Actions** (Действия) вы можете остановить, запустить, изменить или удалить среду выполнения интеграции. Последняя из этих ссылок позволяет просмотреть код JSON для среды выполнения интеграции. Кнопки изменения и удаления действуют только в том случае, если среда выполнения интеграции остановлена.

   ![Среда выполнения интеграции Azure SSIS — действия](./media/tutorial-create-azure-ssis-runtime-portal/azure-ssis-ir-actions.png)

### <a name="azure-ssis-integration-runtimes-in-the-portal"></a>Среда выполнения интеграции Azure SSIS на портале

1. В пользовательском интерфейсе фабрики данных Azure перейдите на вкладку **Изменить**, щелкните **Подключения**, а затем откройте вкладку **Integration Runtimes** (Среды выполнения интеграции), чтобы просмотреть все существующие в фабрике данных среды выполнения интеграции.

   ![Просмотр существующих сред выполнения интеграции](./media/tutorial-create-azure-ssis-runtime-portal/view-azure-ssis-integration-runtimes.png)

2. Щелкните **New** (Создать), чтобы создать новую среду выполнения интеграции Azure SSIS.

   ![Доступ к среде выполнения интеграции через меню](./media/tutorial-create-azure-ssis-runtime-portal/edit-connections-new-integration-runtime-button.png)

3. Чтобы создать среду выполнения интеграции Azure SSIS, щелкните действие **New** (Создать), как показано на рисунке.

4. В окне настройки среды выполнения интеграции выберите **Lift-and-shift existing SSIS packages to execute in Azure** (Перенос существующих пакетов служб SSIS по методике lift-and-shift для выполнения в Azure), а затем щелкните **Next** (Далее).

   ![Указание типа среды выполнения интеграции](./media/tutorial-create-azure-ssis-runtime-portal/integration-runtime-setup-options.png)

5. Остальные шаги описаны в разделе [Подготовка среды выполнения интеграции Azure SSIS](#provision-an-azure-ssis-integration-runtime).

## <a name="azure-powershell"></a>Azure PowerShell

В этом разделе для создания среды выполнения интеграции Azure SSIS используется Azure PowerShell.

### <a name="create-variables"></a>Создание переменных

Скопируйте и вставьте следующий сценарий — Указание значения для переменных. 

```powershell
### Azure Data Factory information
# If your input contains a PSH special character, e.g. "$", precede it with the escape character "`" like "`$"
$SubscriptionName = "[your Azure subscription name]"
$ResourceGroupName = "[your Azure resource group name]"
# Data factory name - Must be globally unique
$DataFactoryName = "[your data factory name]"
# For supported regions, see https://azure.microsoft.com/global-infrastructure/services/?products=data-factory&regions=all
$DataFactoryLocation = "EastUS"

### Azure-SSIS integration runtime information - This is a Data Factory compute resource for running SSIS packages
$AzureSSISName = "[your Azure-SSIS IR name]"
$AzureSSISDescription = "[your Azure-SSIS IR description]"
# For supported regions, see https://azure.microsoft.com/global-infrastructure/services/?products=data-factory&regions=all
$AzureSSISLocation = "EastUS"
# For supported node sizes, see https://azure.microsoft.com/pricing/details/data-factory/ssis/
$AzureSSISNodeSize = "Standard_D8_v3"
# 1-10 nodes are currently supported
$AzureSSISNodeNumber = 2
# Azure-SSIS IR edition/license info: Standard or Enterprise
$AzureSSISEdition = "Standard" # Standard by default, while Enterprise lets you use advanced/premium features on your Azure-SSIS IR
# Azure-SSIS IR hybrid usage info: LicenseIncluded or BasePrice
$AzureSSISLicenseType = "LicenseIncluded" # LicenseIncluded by default, while BasePrice lets you bring your own on-premises SQL Server license with Software Assurance to earn cost savings from Azure Hybrid Benefit (AHB) option
# For a Standard_D1_v2 node, up to 4 parallel executions per node are supported, but for other nodes, up to (2 x number of cores) are currently supported
$AzureSSISMaxParallelExecutionsPerNode = 8
# Custom setup info
$SetupScriptContainerSasUri = "" # OPTIONAL to provide SAS URI of blob container where your custom setup script and its associated files are stored
# Virtual network info: Classic or Azure Resource Manager
$VnetId = "[your virtual network resource ID or leave it empty]" # REQUIRED if you use Azure SQL Database server with virtual network service endpoints/Managed Instance in a virtual network/on-premises data, Azure Resource Manager virtual network is recommended, Classic virtual network will be deprecated soon
$SubnetName = "[your subnet name or leave it empty]" # WARNING: Please use the same subnet as the one used with your Azure SQL Database server with virtual network service endpoints or a different subnet than the one used for your Managed Instance in a virtual network

### SSISDB info
$SSISDBServerEndpoint = "[your Azure SQL Database server name.database.windows.net or Managed Instance name.DNS prefix.database.windows.net or Managed Instance name.public.DNS prefix.database.windows.net,3342 or leave it empty if you do not use SSISDB]" # WARNING: If you use SSISDB, please ensure that there is no existing SSISDB on your database server, so we can prepare and manage one on your behalf
# Authentication info: SQL or Azure Active Directory (AAD)
$SSISDBServerAdminUserName = "[your server admin username for SQL authentication or leave it empty for AAD authentication]"
$SSISDBServerAdminPassword = "[your server admin password for SQL authentication or leave it empty for AAD authentication]"
# For the basic pricing tier, specify "Basic", not "B" - For standard/premium/elastic pool tiers, specify "S0", "S1", "S2", "S3", etc., see https://docs.microsoft.com/azure/sql-database/sql-database-resource-limits-database-server
$SSISDBPricingTier = "[Basic|S0|S1|S2|S3|S4|S6|S7|S9|S12|P1|P2|P4|P6|P11|P15|…|ELASTIC_POOL(name = <elastic_pool_name>) for Azure SQL Database server or leave it empty for Managed Instance]"
```

### <a name="sign-in-and-select-subscription"></a>Вход и Выбор подписки

Добавьте приведенный ниже сценарий на вход и выберите свою подписку Azure:

```powershell
Connect-AzAccount
Select-AzSubscription -SubscriptionName $SubscriptionName
```

### <a name="validate-the-connection-to-database-server"></a>Проверка подключения к серверу базы данных

Добавьте следующий скрипт для проверки вашего сервера базы данных SQL Azure или управляемого экземпляра.

```powershell
# Validate only if you use SSISDB and do not use VNet or AAD authentication
if(![string]::IsNullOrEmpty($SSISDBServerEndpoint))
{
    if([string]::IsNullOrEmpty($VnetId) -and [string]::IsNullOrEmpty($SubnetName))
    {
        if(![string]::IsNullOrEmpty($SSISDBServerAdminUserName) -and ![string]::IsNullOrEmpty($SSISDBServerAdminPassword))
        {
            $SSISDBConnectionString = "Data Source=" + $SSISDBServerEndpoint + ";User ID=" + $SSISDBServerAdminUserName + ";Password=" + $SSISDBServerAdminPassword
            $sqlConnection = New-Object System.Data.SqlClient.SqlConnection $SSISDBConnectionString;
            Try
            {
                $sqlConnection.Open();
            }
            Catch [System.Data.SqlClient.SqlException]
            {
                Write-Warning "Cannot connect to your Azure SQL Database server, exception: $_";
                Write-Warning "Please make sure the server you specified has already been created. Do you want to proceed? [Y/N]"
                $yn = Read-Host
                if(!($yn -ieq "Y"))
                {
                    Return;
                }
            }
        }
    }
}
```

### <a name="configure-virtual-network"></a>Настройка виртуальной сети

Добавьте следующий скрипт, чтобы автоматически настроить разрешения или параметры виртуальной сети вашей среды интеграции Azure SSIS.

```powershell
# Make sure to run this script against the subscription to which the virtual network belongs
if(![string]::IsNullOrEmpty($VnetId) -and ![string]::IsNullOrEmpty($SubnetName))
{
    # Register to the Azure Batch resource provider
    $BatchApplicationId = "ddbf3205-c6bd-46ae-8127-60eb93363864"
    $BatchObjectId = (Get-AzADServicePrincipal -ServicePrincipalName $BatchApplicationId).Id
    Register-AzResourceProvider -ProviderNamespace Microsoft.Batch
    while(!(Get-AzResourceProvider -ProviderNamespace "Microsoft.Batch").RegistrationState.Contains("Registered"))
    {
    Start-Sleep -s 10
    }
    if($VnetId -match "/providers/Microsoft.ClassicNetwork/")
    {
        # Assign the VM contributor role to Microsoft.Batch
        New-AzRoleAssignment -ObjectId $BatchObjectId -RoleDefinitionName "Classic Virtual Machine Contributor" -Scope $VnetId
    }
}
```

### <a name="create-a-resource-group"></a>Создание группы ресурсов

Создайте [группу ресурсов Azure ](../azure-resource-manager/resource-group-overview.md) с помощью команды [New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup). Группа ресурсов — это логический контейнер, в котором ресурсы Azure развертываются и администрируются как группа.

Если ваша группа ресурсов уже существует, не копируйте этот код в скрипт. 

```powershell
New-AzResourceGroup -Location $DataFactoryLocation -Name $ResourceGroupName
```

### <a name="create-a-data-factory"></a>Создание фабрики данных

Чтобы создать фабрику данных, выполните следующую команду.

```powershell
Set-AzDataFactoryV2 -ResourceGroupName $ResourceGroupName `
                         -Location $DataFactoryLocation `
                         -Name $DataFactoryName
```

### <a name="create-an-integration-runtime"></a>Создание среды выполнения интеграции

Выполните следующую команду для создания среды выполнения интеграции Azure SSIS, запускающей пакеты служб SSIS в Azure:

Если вы не используете SSISDB, можно опустить параметры CatalogServerEndpoint, CatalogPricingTier и CatalogAdminCredential.

Если не использовать сервер базы данных SQL Azure с помощью виртуальной сети службы конечные точки или управляемого экземпляра в виртуальной сети для размещения SSISDB или требуется доступ к локальным данным, можно опустить параметрам VNetId и Subnet или передать пустые значения для них. В противном случае их нельзя опускать и нужно передать действительные значения из конфигурации вашей виртуальной сети. См. статью [Присоединение среды выполнения интеграции Azure SSIS к виртуальной сети](https://docs.microsoft.com/azure/data-factory/join-azure-ssis-integration-runtime-virtual-network).

При использовании управляемого экземпляра для размещения SSISDB можно опустить параметр CatalogPricingTier или передать для него пустое значение. В противном случае его нельзя опускать и нужно передать действительное значение из списка поддерживаемых ценовых категорий для службы "База данных SQL Azure". Дополнительные сведения см. в статье [Overview Azure SQL Database resource limits](../sql-database/sql-database-resource-limits.md) (Общие сведения об ограничении ресурсов службы "База данных SQL Azure").

При использовании проверки подлинности Azure Active Directory (AAD) с управляемым удостоверением, для вашей фабрики данных AZURE для подключения к серверу базы данных, можно опустить параметр CatalogAdminCredential, но управляемое удостоверение для ADF-ФАЙЛЕ необходимо добавить в группу AAD с доступом разрешения на сервер базы данных, см. в разделе [AAD включить проверку подлинности для Azure-SSIS IR](https://docs.microsoft.com/azure/data-factory/enable-aad-authentication-azure-ssis-ir). В противном случае этот параметр опустить невозможно и для проверки подлинности SQL возникает необходимость передать действительный объект, который сформирован из имени пользователя и пароля администратора сервера.

```powershell
Set-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                           -DataFactoryName $DataFactoryName `
                                           -Name $AzureSSISName `
                                           -Description $AzureSSISDescription `
                                           -Type Managed `
                                           -Location $AzureSSISLocation `
                                           -NodeSize $AzureSSISNodeSize `
                                           -NodeCount $AzureSSISNodeNumber `
                                           -Edition $AzureSSISEdition `
                                           -LicenseType $AzureSSISLicenseType `
                                           -MaxParallelExecutionsPerNode $AzureSSISMaxParallelExecutionsPerNode `
                                           -VnetId $VnetId `
                                           -Subnet $SubnetName
       
# Add CatalogServerEndpoint, CatalogPricingTier, and CatalogAdminCredential parameters if you use SSISDB
if(![string]::IsNullOrEmpty($SSISDBServerEndpoint))
{
    Set-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                               -DataFactoryName $DataFactoryName `
                                               -Name $AzureSSISName `
                                               -CatalogServerEndpoint $SSISDBServerEndpoint `
                                               -CatalogPricingTier $SSISDBPricingTier

    if(![string]::IsNullOrEmpty($SSISDBServerAdminUserName) –and ![string]::IsNullOrEmpty($SSISDBServerAdminPassword)) # Add CatalogAdminCredential parameter if you do not use AAD authentication
    {
        $secpasswd = ConvertTo-SecureString $SSISDBServerAdminPassword -AsPlainText -Force
        $serverCreds = New-Object System.Management.Automation.PSCredential($SSISDBServerAdminUserName, $secpasswd)

        Set-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                                   -DataFactoryName $DataFactoryName `
                                                   -Name $AzureSSISName `
                                                   -CatalogAdminCredential $serverCreds
    }
}

# Add SetupScriptContainerSasUri parameter if you use custom setup
if(![string]::IsNullOrEmpty($SetupScriptContainerSasUri))
{
    Set-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                               -DataFactoryName $DataFactoryName `
                                               -Name $AzureSSISName `
                                               -SetupScriptContainerSasUri $SetupScriptContainerSasUri
}
```

### <a name="start-integration-runtime"></a>Запуск среды выполнения интеграции

Выполните следующие команды для запуска среды выполнения интеграции Azure SSIS.

```powershell
write-host("##### Starting #####")
Start-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                             -DataFactoryName $DataFactoryName `
                                             -Name $AzureSSISName `
                                             -Force

write-host("##### Completed #####")
write-host("If any cmdlet is unsuccessful, please consider using -Debug option for diagnostics.")
```

> [!NOTE]
> Исключение любое время пользовательской установки этот процесс должен быть завершен в течение пяти минут, но может занять около 20 – 30 минут для Azure-SSIS IR, присоединение к виртуальной сети.
>
> Если вы используете SSISDB, служба фабрики данных AZURE подключится к серверу базы данных для подготовки SSISDB. Он также настраивает разрешения и параметры для виртуальной сети, если указан и соединяет среду выполнения Интеграции Azure SSIS к виртуальной сети.
> 
> При подготовке IR Azure SSIS, также устанавливается пакет дополнительных компонентов Azure для служб SSIS и распространяемый компонент Access. Эти компоненты обеспечивают подключение к файлам Excel и Access и различных источников данных Azure, а также источники данных, уже поддерживаются встроенными компонентами. Можно также установить дополнительные компоненты, см. в разделе [Выборочная установка Azure-SSIS IR](how-to-configure-azure-ssis-ir-custom-setup.md).

### <a name="full-script"></a>Полный сценарий

Ниже приведен полный скрипт, который создает среду выполнения интеграции Azure-SSIS.

```powershell
### Azure Data Factory information
# If your input contains a PSH special character, e.g. "$", precede it with the escape character "`" like "`$"
$SubscriptionName = "[your Azure subscription name]"
$ResourceGroupName = "[your Azure resource group name]"
# Data factory name - Must be globally unique
$DataFactoryName = "[your data factory name]"
# For supported regions, see https://azure.microsoft.com/global-infrastructure/services/?products=data-factory&regions=all
$DataFactoryLocation = "EastUS"

### Azure-SSIS integration runtime information - This is a Data Factory compute resource for running SSIS packages
$AzureSSISName = "[your Azure-SSIS IR name]"
$AzureSSISDescription = "[your Azure-SSIS IR description]"
# For supported regions, see https://azure.microsoft.com/global-infrastructure/services/?products=data-factory&regions=all
$AzureSSISLocation = "EastUS"
# For supported node sizes, see https://azure.microsoft.com/pricing/details/data-factory/ssis/
$AzureSSISNodeSize = "Standard_D8_v3"
# 1-10 nodes are currently supported
$AzureSSISNodeNumber = 2
# Azure-SSIS IR edition/license info: Standard or Enterprise
$AzureSSISEdition = "Standard" # Standard by default, while Enterprise lets you use advanced/premium features on your Azure-SSIS IR
# Azure-SSIS IR hybrid usage info: LicenseIncluded or BasePrice
$AzureSSISLicenseType = "LicenseIncluded" # LicenseIncluded by default, while BasePrice lets you bring your own on-premises SQL Server license with Software Assurance to earn cost savings from Azure Hybrid Benefit (AHB) option
# For a Standard_D1_v2 node, up to 4 parallel executions per node are supported, but for other nodes, up to (2 x number of cores) are currently supported
$AzureSSISMaxParallelExecutionsPerNode = 8
# Custom setup info
$SetupScriptContainerSasUri = "" # OPTIONAL to provide SAS URI of blob container where your custom setup script and its associated files are stored
# Virtual network info: Classic or Azure Resource Manager
$VnetId = "[your virtual network resource ID or leave it empty]" # REQUIRED if you use Azure SQL Database server with virtual network service endpoints/Managed Instance in a virtual network/on-premises data, Azure Resource Manager virtual network is recommended, Classic virtual network will be deprecated soon
$SubnetName = "[your subnet name or leave it empty]" # WARNING: Please use the same subnet as the one used with your Azure SQL Database server with virtual network service endpoints or a different subnet than the one used for your Managed Instance in a virtual network

### SSISDB info
$SSISDBServerEndpoint = "[your Azure SQL Database server name.database.windows.net or Managed Instance name.DNS prefix.database.windows.net or Managed Instance name.public.DNS prefix.database.windows.net,3342 or leave it empty if you do not use SSISDB]" # WARNING: If you use SSISDB, please ensure that there is no existing SSISDB on your database server, so we can prepare and manage one on your behalf
# Authentication info: SQL or Azure Active Directory (AAD)
$SSISDBServerAdminUserName = "[your server admin username for SQL authentication or leave it empty for AAD authentication]"
$SSISDBServerAdminPassword = "[your server admin password for SQL authentication or leave it empty for AAD authentication]"
# For the basic pricing tier, specify "Basic", not "B" - For standard/premium/elastic pool tiers, specify "S0", "S1", "S2", "S3", etc., see https://docs.microsoft.com/azure/sql-database/sql-database-resource-limits-database-server
$SSISDBPricingTier = "[Basic|S0|S1|S2|S3|S4|S6|S7|S9|S12|P1|P2|P4|P6|P11|P15|…|ELASTIC_POOL(name = <elastic_pool_name>) for Azure SQL Database server or leave it empty for Managed Instance]"

### Sign in and select subscription
Connect-AzAccount
Select-AzSubscription -SubscriptionName $SubscriptionName

### Validate the connection to database server
# Validate only if you use SSISDB and do not use VNet or AAD authentication
if(![string]::IsNullOrEmpty($SSISDBServerEndpoint))
{
    if([string]::IsNullOrEmpty($VnetId) -and [string]::IsNullOrEmpty($SubnetName))
    {
        if(![string]::IsNullOrEmpty($SSISDBServerAdminUserName) -and ![string]::IsNullOrEmpty($SSISDBServerAdminPassword))
        {
            $SSISDBConnectionString = "Data Source=" + $SSISDBServerEndpoint + ";User ID=" + $SSISDBServerAdminUserName + ";Password=" + $SSISDBServerAdminPassword
            $sqlConnection = New-Object System.Data.SqlClient.SqlConnection $SSISDBConnectionString;
            Try
            {
                $sqlConnection.Open();
            }
            Catch [System.Data.SqlClient.SqlException]
            {
                Write-Warning "Cannot connect to your Azure SQL Database server, exception: $_";
                Write-Warning "Please make sure the server you specified has already been created. Do you want to proceed? [Y/N]"
                $yn = Read-Host
                if(!($yn -ieq "Y"))
                {
                    Return;
                }
            }
        }
    }
}

### Configure virtual network
# Make sure to run this script against the subscription to which the virtual network belongs
if(![string]::IsNullOrEmpty($VnetId) -and ![string]::IsNullOrEmpty($SubnetName))
{
    # Register to the Azure Batch resource provider
    $BatchApplicationId = "ddbf3205-c6bd-46ae-8127-60eb93363864"
    $BatchObjectId = (Get-AzADServicePrincipal -ServicePrincipalName $BatchApplicationId).Id
    Register-AzResourceProvider -ProviderNamespace Microsoft.Batch
    while(!(Get-AzResourceProvider -ProviderNamespace "Microsoft.Batch").RegistrationState.Contains("Registered"))
    {
    Start-Sleep -s 10
    }
    if($VnetId -match "/providers/Microsoft.ClassicNetwork/")
    {
        # Assign the VM contributor role to Microsoft.Batch
        New-AzRoleAssignment -ObjectId $BatchObjectId -RoleDefinitionName "Classic Virtual Machine Contributor" -Scope $VnetId
    }
}

### Create a data factory
Set-AzDataFactoryV2 -ResourceGroupName $ResourceGroupName `
                         -Location $DataFactoryLocation `
                         -Name $DataFactoryName

### Create an integration runtime
Set-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                           -DataFactoryName $DataFactoryName `
                                           -Name $AzureSSISName `
                                           -Description $AzureSSISDescription `
                                           -Type Managed `
                                           -Location $AzureSSISLocation `
                                           -NodeSize $AzureSSISNodeSize `
                                           -NodeCount $AzureSSISNodeNumber `
                                           -Edition $AzureSSISEdition `
                                           -LicenseType $AzureSSISLicenseType `
                                           -MaxParallelExecutionsPerNode $AzureSSISMaxParallelExecutionsPerNode `
                                           -VnetId $VnetId `
                                           -Subnet $SubnetName
       
# Add CatalogServerEndpoint, CatalogPricingTier, and CatalogAdminCredential parameters if you use SSISDB
if(![string]::IsNullOrEmpty($SSISDBServerEndpoint))
{
    Set-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                               -DataFactoryName $DataFactoryName `
                                               -Name $AzureSSISName `
                                               -CatalogServerEndpoint $SSISDBServerEndpoint `
                                               -CatalogPricingTier $SSISDBPricingTier

    if(![string]::IsNullOrEmpty($SSISDBServerAdminUserName) –and ![string]::IsNullOrEmpty($SSISDBServerAdminPassword)) # Add CatalogAdminCredential parameter if you do not use AAD authentication
    {
        $secpasswd = ConvertTo-SecureString $SSISDBServerAdminPassword -AsPlainText -Force
        $serverCreds = New-Object System.Management.Automation.PSCredential($SSISDBServerAdminUserName, $secpasswd)

        Set-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                                   -DataFactoryName $DataFactoryName `
                                                   -Name $AzureSSISName `
                                                   -CatalogAdminCredential $serverCreds
    }
}

# Add SetupScriptContainerSasUri parameter when you use custom setup
if(![string]::IsNullOrEmpty($SetupScriptContainerSasUri))
{
    Set-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                               -DataFactoryName $DataFactoryName `
                                               -Name $AzureSSISName `
                                               -SetupScriptContainerSasUri $SetupScriptContainerSasUri
}

### Start integration runtime
write-host("##### Starting #####")
Start-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                             -DataFactoryName $DataFactoryName `
                                             -Name $AzureSSISName `
                                             -Force

write-host("##### Completed #####")
write-host("If any cmdlet is unsuccessful, please consider using -Debug option for diagnostics.")
```

## <a name="azure-resource-manager-template"></a>Шаблон Azure Resource Manager

В этом разделе для создания среды выполнения интеграции Azure-SSIS используется шаблон Azure Resource Manager. Ниже приведен пример пошагового руководства.

1. Создайте файл JSON со следующим шаблоном Resource Manager Azure. Замените значения в угловых скобках (заполнители) на собственные.

    ```json
    {
      "contentVersion": "1.0.0.0",
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "parameters": {},
        "variables": {},
        "resources": [{
            "name": "<Specify a name for your data factory>",
            "apiVersion": "2018-06-01",
            "type": "Microsoft.DataFactory/factories",
            "location": "East US",
            "properties": {},
            "resources": [{
                "type": "integrationruntimes",
                "name": "<Specify a name for your Azure-SSIS IR>",
                "dependsOn": [ "<The name of the data factory you specified at the beginning>" ],
                "apiVersion": "2018-06-01",
                "properties": {
                    "type": "Managed",
                    "typeProperties": {
                        "computeProperties": {
                            "location": "East US",
                            "nodeSize": "Standard_D8_v3",
                            "numberOfNodes": 1,
                            "maxParallelExecutionsPerNode": 8
                        },
                        "ssisProperties": {
                            "catalogInfo": {
                                "catalogServerEndpoint": "<Azure SQL Database server name>.database.windows.net",
                                "catalogAdminUserName": "<Azure SQL Database server admin username>",
                                "catalogAdminPassword": {
                                    "type": "SecureString",
                                    "value": "<Azure SQL Database server admin password>"
                                },
                                "catalogPricingTier": "Basic"
                            }
                        }
                    }
                }
            }]
        }]
    }
    ```

2. Для развертывания шаблона Azure Resource Manager, выполните команду New-AzResourceGroupDeployment, как показано в следующем примере, где ADFTutorialResourceGroup — имя группы ресурсов, а ADFTutorialARM.json — файл, содержащий определение JSON для фабрики данных и среды выполнения интеграции Azure SSIS

    ```powershell
    New-AzResourceGroupDeployment -Name MyARMDeployment -ResourceGroupName ADFTutorialResourceGroup -TemplateFile ADFTutorialARM.json
    ```

    С помощью этой команды создается фабрика данных, а в ней — среда выполнения интеграции Azure SSIS, но текущая команда не запускает среду выполнения интеграции.

3. Чтобы запустить среду выполнения Интеграции Azure SSIS, выполните команду Start-AzDataFactoryV2IntegrationRuntime:

    ```powershell
    Start-AzDataFactoryV2IntegrationRuntime -ResourceGroupName "<Resource Group Name>" `
                                                 -DataFactoryName "<Data Factory Name>" `
                                                 -Name "<Azure SSIS IR Name>" `
                                                 -Force
    ```

> [!NOTE]
> Исключение любое время пользовательской установки этот процесс должен быть завершен в течение пяти минут, но может занять около 20 – 30 минут для Azure-SSIS IR, присоединение к виртуальной сети.
>
> Если вы используете SSISDB, служба фабрики данных AZURE подключится к серверу базы данных для подготовки SSISDB. Он также настраивает разрешения и параметры для виртуальной сети, если указан и соединяет среду выполнения Интеграции Azure SSIS к виртуальной сети.
> 
> При подготовке IR Azure SSIS, также устанавливается пакет дополнительных компонентов Azure для служб SSIS и распространяемый компонент Access. Эти компоненты обеспечивают подключение к файлам Excel и Access и различных источников данных Azure, а также источники данных, уже поддерживаются встроенными компонентами. Можно также установить дополнительные компоненты, см. в разделе [Выборочная установка Azure-SSIS IR](how-to-configure-azure-ssis-ir-custom-setup.md).

## <a name="deploy-ssis-packages"></a>Развертывание пакетов служб SSIS.

Если вы используете SSISDB, можно развертывать пакеты в него и запускайте их в Azure-SSIS IR с помощью средств SSDT и SSMS, которые подключаются к серверу базы данных с помощью конечной точки сервера.  Для базы данных SQL server или управляемого экземпляра в виртуальной сети или управляемого экземпляра с общедоступной конечной точки, является формат конечной точки сервера `<server name>.database.windows.net` / `<server name>.<dns prefix>.database.windows.net` / `<server name>.public.<dns prefix>.database.windows.net,3342`, соответственно. Если вы не используете SSISDB, можно развернуть пакеты в файл системы/файл общих папок или Azure файлы и их выполнение с помощью Azure-SSIS IR `dtinstall` / `dtutil` / `dtexec` служебные программы командной строки. Дополнительные сведения см. в разделе [развертывание пакетов SSIS](/sql/integration-services/packages/deploy-integration-services-ssis-projects-and-packages#deploy-packages-to-integration-services-server). В обоих случаях также можно запустить в развернутых пакетов на Azure-SSIS IR с помощью действия выполнения пакета служб SSIS в конвейерах фабрики данных AZURE, см. в разделе [выполнения как первого класса фабрики данных AZURE действие пакета служб SSIS на вызов](https://docs.microsoft.com/azure/data-factory/how-to-invoke-ssis-package-ssis-activity).

## <a name="next-steps"></a>Дальнейшие действия

См. также другие разделы Azure-SSIS IR в этой документации:

- [Среда выполнения интеграции Azure SSIS](concepts-integration-runtime.md#azure-ssis-integration-runtime). Эта статья содержит сведения о средах выполнения интеграции в целом, включая Azure SSIS IR.
- [Мониторинг Azure-SSIS IR](monitor-integration-runtime.md#azure-ssis-integration-runtime). В этой статье показано, как получение и интерпретация сведения среды выполнения интеграции Azure SSIS
- [Управление Azure-SSIS IR](manage-azure-ssis-integration-runtime.md). В этой статье показано, как остановить, запустить или удалить среду выполнения Интеграции Azure SSIS — он также показано, как масштабировать среду Azure-SSIS IR путем добавления дополнительных узлов.
- [Присоединение среды выполнения Интеграции Azure SSIS к виртуальной сети](join-azure-ssis-integration-runtime-virtual-network.md). В этой статье сведения о присоединении среды выполнения Интеграции Azure SSIS к виртуальной сети.