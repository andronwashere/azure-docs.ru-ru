---
title: Выполнение задачи очистки базы данных с помощью Функций Azure | Документы Майкрософт
description: С помощью Функций Azure можно запланировать задачу, которая периодически подключается к базе данных SQL Azure для очистки строк.
services: functions
documentationcenter: na
author: ggailey777
manager: jeconnoc
ms.assetid: 076f5f95-f8d2-42c7-b7fd-6798856ba0bb
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 10/28/2018
ms.author: glenga
ms.openlocfilehash: 19a5fe4c087d477ff15d2237a36d1c4ecaa0e070
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "65908192"
---
# <a name="use-azure-functions-to-connect-to-an-azure-sql-database"></a>Подключение к базе данных SQL Azure с помощью Функций Azure

В этой статье показано, как с помощью Функций Azure создать запланированное задание, которое подключается к экземпляру Базы данных SQL Azure. Код функции очищает строки в таблице базы данных. Новый C# функция создается на основе стандартного шаблона триггера в Visual Studio 2019. Для выполнения этого сценария необходимо также задать строку подключения к базе данных как параметр приложений в приложении-функции. В этом сценарии к базе данных применяется массовая операция. 

Если вы впервые работаете с Функциями C#, обратитесь к статье [Справочник разработчика C# по функциям Azure](functions-dotnet-class-library.md).

## <a name="prerequisites"></a>Технические условия

+ Выполните шаги, приведенные в статье [Создание первой функции с помощью Visual Studio](functions-create-your-first-function-visual-studio.md), чтобы создать локальное приложение-функцию, предназначенное для среды выполнения версии 2.x. Кроме того, опубликуйте свой проект в приложении-функции в Azure.

+ В этой статье демонстрируется команда Transact-SQL, которая выполняет операцию массовой очистки в таблице **SalesOrderHeader** образца базы данных AdventureWorksLT. Чтобы создать образец базы данных AdventureWorksLT, выполните инструкции, приведенные в статье [Создание базы данных SQL Azure на портале Azure](../sql-database/sql-database-get-started-portal.md).

+ Добавьте [правило брандмауэра уровня сервера](../sql-database/sql-database-get-started-portal-firewall.md) для общедоступного IP-адреса компьютера, на котором выполняются действия из этого краткого руководства. Это правило требуется для получения доступа к экземпляру базы данных SQL с вашего локального компьютера.  

## <a name="get-connection-information"></a>Получение сведений о подключении

Необходимо получить строку подключения к базе данных, созданную при выполнении инструкций в статье [Создание базы данных SQL Azure на портале Azure](../sql-database/sql-database-get-started-portal.md).

1. Войдите на [портале Azure](https://portal.azure.com/).

1. В меню слева выберите пункт **Базы данных SQL** и на странице **Базы данных SQL** выберите имя своей базы данных.

1. Щелкните **Строки подключения** в разделе **Параметры** и полностью скопируйте строку подключения **ADO.NET**.

    ![Скопируйте строку подключения ADO.NET.](./media/functions-scenario-database-table-cleanup/adonet-connection-string.png)

## <a name="set-the-connection-string"></a>Задание строки подключения

Выполнение функций в Azure происходит в рамках приложений-функций. В целях безопасности рекомендуется хранить строки подключения и другие секретные данные в параметрах приложения-функции. Использование параметров приложения позволяет предотвратить случайное раскрытие строки подключения в коде. Доступ к параметрам приложения-функции можно получить прямо из Visual Studio.

Вы должны были ранее опубликовать свое приложение в Azure. Если вы еще этого не сделали, выполните инструкции, приведенные в разделе [Публикация в Azure](functions-develop-vs.md#publish-to-azure).

1. В обозревателе решений щелкните правой кнопкой мыши проект приложения-функции и выберите **Публикация** > **Управление параметрами приложения...** Выберите **Добавить параметр**, а затем в окне **Имя нового параметра приложения** введите `sqldb_connection` и щелкните **ОК**.

    ![Параметры приложения-функции](./media/functions-scenario-database-table-cleanup/functions-app-service-add-setting.png)

1. В новом параметре **sqldb_connection** вставьте строку подключения, скопированную в предыдущем разделе, в поле **Локальный** и замените заполнители `{your_username}` и `{your_password}` настоящими значениями. Щелкните **Вставка локального значения**, чтобы скопировать обновленное значение в поле **Удаленный**, а затем нажмите кнопку **ОК**.

    ![Параметр добавления строки подключения SQL.](./media/functions-scenario-database-table-cleanup/functions-app-service-settings-connection-string.png)

    Строки подключения хранятся в зашифрованном виде в Azure (**Удаленный**). Чтобы предотвратить утечку секретов, файл проекта local.settings.json (**Локальный**) должен быть исключен из системы управления версиями, например, с помощью файла GITIGNORE.

## <a name="add-the-sqlclient-package-to-the-project"></a>Добавление пакета SqlClient в проект

Вам необходимо добавить пакет NuGet, содержащий библиотеку SqlClient. Эта библиотека доступа к данным требуется для подключения к базе данных SQL.

1. Откройте проект приложения локальной функции в Visual Studio 2019.

1. В обозревателе решений щелкните правой кнопкой мыши проект приложения-функции и выберите **Управление пакетами NuGet**.

1. Перейдите на вкладку **Обзор**, выполните поиск по запросу ```System.Data.SqlClient``` и выберите его по завершении поиска.

1. На странице **System.Data.SqlClient** выберите версию `4.5.1` и щелкните **Установить**.

1. После завершения установки просмотрите изменения, а затем нажмите кнопку **ОК**, чтобы закрыть окно **предварительного просмотра**.

1. Если откроется окно **Прием условий лицензионного соглашения**, щелкните **Я принимаю**.

Теперь можно добавить код функции C#, который подключается к базе данных SQL.

## <a name="add-a-timer-triggered-function"></a>Добавление функции, активируемой с помощью таймера

1. В обозревателе решений щелкните правой кнопкой мыши проект приложения-функции и выберите **Добавить** > **Новая функция Azure**.

1. Выбрав шаблон **Функции Azure**, назовите новый элемент, например `DatabaseCleanup.cs`, и выберите **Добавить**.

1. В диалоговом окне **Новая функция Azure** выберите **Триггер таймера**, а затем нажмите кнопку **ОК**. В этом диалоговом окне создается файл кода для функции, активируемой по таймеру.

1. Откройте новый файл кода и добавьте в начало файла следующие операторы using:

    ```cs
    using System.Data.SqlClient;
    using System.Threading.Tasks;
    ```

1. Замените имеющуюся функцию `Run` следующим кодом:

    ```cs
    [FunctionName("DatabaseCleanup")]
    public static async Task Run([TimerTrigger("*/15 * * * * *")]TimerInfo myTimer, ILogger log)
    {
        // Get the connection string from app settings and use it to create a connection.
        var str = Environment.GetEnvironmentVariable("sqldb_connection");
        using (SqlConnection conn = new SqlConnection(str))
        {
            conn.Open();
            var text = "UPDATE SalesLT.SalesOrderHeader " +
                    "SET [Status] = 5  WHERE ShipDate < GetDate();";

            using (SqlCommand cmd = new SqlCommand(text, conn))
            {
                // Execute the command and log the # rows affected.
                var rows = await cmd.ExecuteNonQueryAsync();
                log.LogInformation($"{rows} rows were updated");
            }
        }
    }
    ```

    Эта функция выполняется каждые 15 секунд, чтобы обновить столбец `Status` на основе даты отправки. Дополнительные сведения о триггере таймера см. в статье [Триггеры таймера для службы "Функции Azure"](functions-bindings-timer.md).

1. Нажмите клавишу **F5** для запуска приложения-функции. Позади Visual Studio откроется окно выполнения [Azure Functions Core Tools](functions-develop-local.md).

1. Через 15 секунд после запуска функция начнет выполняться. Просмотрите выходные данные и запишите число строк, обновленных в таблице **SalesOrderHeader**.

    ![Просмотрите журналы функции.](./media/functions-scenario-database-table-cleanup/function-execution-results-log.png)

    При первом выполнении необходимо обновить 32 строки данных. Следующие запуски не будут обновлять строки данных, если вы не внесете изменения в данные таблицы SalesOrderHeader, чтобы оператор `UPDATE` выбрал больше строк.

Если вы планируете [опубликовать эту функцию](functions-develop-vs.md#publish-to-azure), не забудьте указать для атрибута `TimerTrigger` другое [расписание в формате выражения Cron](functions-bindings-timer.md#cron-expressions) (выполнение каждые 15 секунд).

## <a name="next-steps"></a>Дальнейшие действия

Узнайте, как использовать Функции с Logic Apps для интеграции с другими службами.

> [!div class="nextstepaction"]
> [Создание функции, интегрируемой с Logic Apps](functions-twitter-email.md)

Дополнительные сведения о Функциях см. в следующих статьях:

+ [Справочник разработчика по функциям Azure](functions-reference.md)  
  Справочник программиста по созданию функций, а также определению триггеров и привязок.
+ [Testing Azure Functions](functions-test-a-function.md)  
  (Тестирование функций Azure) Описание различных средств и методов тестирования функций.  
