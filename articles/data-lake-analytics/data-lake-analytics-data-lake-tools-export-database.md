---
title: Экспорт базы данных U-SQL с помощью Средств Azure Data Lake для Visual Studio
description: Сведения об использовании Средств Azure Data Lake для Visual Studio для экспорта и автоматического импорта базы данных U-SQL в локальную учетную запись.
services: data-lake-analytics
author: yanancai
ms.author: yanacai
ms.reviewer: jasonwhowell
ms.assetid: dc9b21d8-c5f4-4f77-bcbc-eff458f48de2
ms.service: data-lake-analytics
ms.topic: conceptual
ms.date: 11/27/2017
ms.openlocfilehash: fe28aa8b88f557d4bbcdabf1de1c4bc6491743ce
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "60628579"
---
# <a name="export-a-u-sql-database"></a>Экспорт базы данных U-SQL

В этой статье вы узнаете, как использовать [Средства Azure Data Lake для Visual Studio](https://aka.ms/adltoolsvs), чтобы экспортировать базу данных U-SQL в виде одного скрипта U-SQL и скачанных ресурсов. Вы можете импортировать экспортированную базу данных в локальную учетную запись в том же процессе.

Пользователи обычно поддерживают несколько сред для разработки, тестирования и работы. Эти среды размещаются как в локальной учетной записи, так и на локальном компьютере разработчика, а также в учетной записи Azure Data Lake Analytics в Azure. 

Часто разработчикам необходимо пересоздавать свои работы в рабочей базе данных при разработке и настройке запросов U-SQL в среде разработки и тестирования. Мастер экспорта баз данных помогает ускорить этот процесс. Используя мастер, разработчики могут клонировать существующую среду базы данных и образцы данных в другие учетные записи Data Lake Analytics.

## <a name="export-steps"></a>Экспорт

### <a name="step-1-export-the-database-in-server-explorer"></a>Шаг 1. Экспорт базы данных в обозревателе серверов

Все учетные записи Data Lake Analytics, на которые у вас есть разрешения, перечислены в обозревателе сервера. Чтобы экспортировать базу данных:

1. В обозревателе сервера разверните учетную запись, содержащую базу данных, которую требуется экспортировать.
2. Щелкните базу данных правой кнопкой мыши и выберите **Экспорт**. 
   
    ![Обозреватель сервера — экспорта базы данных](./media/data-lake-analytics-data-lake-tools-export-database/export-database.png)

     Если параметр меню **Экспорт** недоступен, необходимо [обновить средство до последней версии](https://aka.ms/adltoolsvs).

### <a name="step-2-configure-the-objects-that-you-want-to-export"></a>Шаг 2. Настроить объекты, которые вы хотите экспортировать

Если вам требуется только небольшая часть большой базы данных, в окне мастера экспорта можно настроить подмножество объектов, которые необходимо экспортировать. 

Действие экспорта выполняется с помощью запуска задания U-SQL, и поэтому экспорт из учетной записи Azure имеет определенную стоимость.

![Мастер экспорта базы данных — выбор объектов экспорта](./media/data-lake-analytics-data-lake-tools-export-database/export-database-wizard.png)

### <a name="step-3-check-the-objects-list-and-other-configurations"></a>Шаг 3. Проверка списка объектов и других конфигураций

На этом шаге можно проверить выбранные объекты в поле **Export object list** (Экспорт списка объектов). Если есть какие-либо ошибки, выберите **Предыдущий**, чтобы вернуться назад и правильно настроить объекты, которые нужно экспортировать.

Кроме того, можно настроить другие параметры для цели экспорта. Описание конфигурации приведено в следующей таблице:

|Параметр Configuration|Описание|
|-------------|-----------|
|Имя конечной папки|Это имя указывает, где требуется сохранить экспортированные ресурсы базы данных. Примерами являются сборки, дополнительные файлы и демонстрационные данные. В корневой папке локальных данных создается папка с таким именем.|
|Каталог проекта|Этот путь определяет, где требуется сохранить экспортированный скрипт U-SQL. Определения всех объектов базы данных сохраняются в этом расположении.|
|Только схема|Если вы выберете этот параметр, будут экспортированы только определения и ресурсы базы данных (например, сборки и дополнительные файлы).|
|Схема и данные|Если вы выберете этот параметр, будут экспортированы определения, ресурсы и данные базы данных. Экспортируются верхние N строк из таблицы.|
|Автоматический импорт в локальную базу данных|Если выбран этот параметр, то после экспорта экспортированная база данных автоматически импортируется в локальную базу данных.|

![Мастер экспорта баз данных — экспорт списка объектов и других конфигураций](./media/data-lake-analytics-data-lake-tools-export-database/export-database-wizard-configuration.png)

### <a name="step-4-check-the-export-results"></a>Шаг 4. Проверка результатов экспорта

После экспорта результаты можно просмотреть в окне журнала мастера. В приведенном ниже примере показано, как найти экспортированный скрипт U-SQL и ресурсы базы данных, включая сборки, дополнительные файлы и демонстрационные данные.

![Мастер экспорта базы данных — экспорт результатов](./media/data-lake-analytics-data-lake-tools-export-database/export-database-wizard-completed.png)

## <a name="import-the-exported-database-to-a-local-account"></a>Импорт экспортированной базы данных в локальную учетную запись

Удобнее всего импортировать экспортированную базу данных можно с использованием флажка **Import to Local Database Automatically** (Импортировать локальную базу данных автоматически) во время экспорта на шаге 3. Если этот флажок не установлен, то сначала найдите экспортированный скрипт U-SQL в журнал экспорта. Затем запустите скрипт U-SQL локально, чтобы импортировать базу данных для локальной учетной записи.

## <a name="import-the-exported-database-to-a-data-lake-analytics-account"></a>Импорт экспортированной базы данных в учетную запись Data Lake Analytics

Чтобы импортировать базу данных в другую учетную запись Data Lake Analytics:

1. Отправьте экспортируемые ресурсы, включая сборки, дополнительные файлы и примеры данных для учетной записи Azure Data Lake Store по умолчанию учетной записи Data Lake Analytics, в которую требуется выполнить импорт. В корневой папке локальных данных можно найти папку экспортированного ресурса. Отправьте всю папку в корневую учетную запись Data Lake Store по умолчанию.
2. Когда отправка завершится, отправьте экспортированный скрипт U-SQL в учетную запись Data Lake Analytics, в которую вы хотите импортировать базу данных.

## <a name="known-limitations"></a>Известные ограничения

Если вы выбрали параметр **Схема и данные** на шаге 3, средство выполняет задание U-SQL для экспорта данных, хранящихся в таблицах. Из-за этого данные могут экспортироваться медленно и экспорт может быть платным. 

## <a name="next-steps"></a>Дальнейшие действия

* [Data Definition Language (DDL) Statements (U-SQL)](/u-sql/data-definition-language-ddl-statements) (Операторы U-SQL языка описания данных DDL) 
* [Тестирование и отладка заданий U-SQL с помощью локального выполнения и пакета SDK U-SQL для Azure Data Lake](data-lake-analytics-data-lake-tools-local-run.md)


