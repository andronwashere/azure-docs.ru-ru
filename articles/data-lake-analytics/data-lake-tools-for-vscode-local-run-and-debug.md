---
title: Запуск и отладка заданий U-SQL локально в средствах Azure Data Lake для Visual Studio Code
description: Узнайте, как выполнять локальные запуск и отладку заданий U-SQL с помощью средств Azure Data Lake для Visual Studio Code.
services: data-lake-analytics
ms.service: data-lake-analytics
author: jejiang
ms.author: jejiang
ms.reviewer: jasonwhowell
ms.assetid: dc9b21d8-c5f4-4f77-bcbc-eff458f48de2
ms.topic: conceptual
ms.date: 07/14/2017
ms.openlocfilehash: 765bcaab0f91e097be827bfa6e8f505ef5330d57
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "60814395"
---
# <a name="run-u-sql-and-debug-locally-in-visual-studio-code"></a>Запуск и отладка заданий U-SQL локально в Visual Studio Code
В этой статье описывается, как выполнять задания U-SQL на локальном компьютере для разработки, чтобы ускорить программирование на ранних этапах, и отлаживать код локально в Visual Studio Code. Инструкции по использованию средств Azure Data Lake для Visual Studio Code см. в статье [Использование средств Azure Data Lake для Visual Studio Code](data-lake-analytics-data-lake-tools-for-vscode.md).

Действие по локальному запуску и локальной отладке U-SQL поддерживают только установки средств Azure Data Lake для Visual Studio в Windows. Установки в macOS и операционных системах Linux не поддерживают эту функцию.

## <a name="set-up-the-u-sql-local-run-environment"></a>Настройка среды для локального запуска U-SQL

1. Выберите сочетание клавиш Ctrl + Shift + P, чтобы открыть палитру команд, а затем введите **ADL: Скачайте локальный запуск пакета** чтобы скачать пакеты.  

   ![Скачивание пакетов зависимостей для локального запуска ADL](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/downloadtheadllocalrunpackage.png)

2. Найдите пакеты зависимостей по пути, показанному на панели **вывода**, затем установите BuildTools и Win10SDK 10240. Пример пути:  
`C:\Users\xxx\AppData\Roaming\LocalRunDependency` 

   ![Поиск пакетов зависимостей](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/LocateDependencyPath.png)

   2.1. Чтобы установить **BuildTools**, щелкните файл visualcppbuildtools_full.exe в папке LocalRunDependency, а затем выполните инструкции мастера установки.   

    ![Установка BuildTools](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/InstallBuildTools.png)

   2.2. Чтобы установить **Win10SDK 10240**, щелкните файл sdksetup.exe в папке LocalRunDependency/Win10SDK_10.0.10240_2, а затем выполните инструкции мастера установки.  

    ![Установка Win10SDK 10240](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/InstallWin10SDK.png)

3. Настройте переменную среды. Установите для переменной среды **SCOPE_CPP_SDK** значение:  
`C:\Users\XXX\AppData\Roaming\LocalRunDependency\CppSDK_3rdparty`  


## <a name="start-the-local-run-service-and-submit-the-u-sql-job-to-a-local-account"></a>Локальный запуск службы и отправка задания U-SQL в локальную учетную запись 
Для новых пользователей, используйте **ADL: Скачайте локальный запуск пакета** для загрузки локального выполнения пакетов, если вы еще не [настроить среду для локального запуска U-SQL](#set-up-the-u-sql-local-run-environment).

1. Выберите сочетание клавиш Ctrl + Shift + P, чтобы открыть палитру команд, а затем введите **ADL: Запустите службу локально**.   
2. Выберите **Принять**, чтобы принять условия лицензионного соглашения об использовании программного обеспечения Майкрософт в первый раз. 

   ![Принятие условий лицензионного соглашения об использовании программного обеспечения Майкрософт](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/AcceptEULA.png)   
3. Откроется консоль команд. Для новых пользователей введите **3**, а затем найдите локальную папку для входных и выходных данных. Для остальных параметров можно использовать значения по умолчанию. 

   ![Локальный запуск команд в средствах Data Lake для Visual Studio Code](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/data-lake-tools-for-vscode-local-run-cmd.png)
4. Выберите сочетание клавиш Ctrl + Shift + P, чтобы открыть палитру команд, введите **ADL: Отправка задания**, а затем выберите **локального** для отправки задания в вашей локальной учетной записи.

   ![Выбор локальной учетной записи в средствах Data Lake для Visual Studio Code](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/data-lake-tools-for-vscode-select-local.png)
5. После отправки задания можно просмотреть сведения об отправке. Чтобы просмотреть сведения об отправке, выберите **jobUrl** в окне **вывода**. Вы также можете просмотреть состояние отправки задания в консоли команд. Для просмотра сведений о задании в консоли команд введите **7**.

   ![Результат локального запуска для средств Data Lake в Visual Studio Code](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/data-lake-tools-for-vscode-local-run-result.png)
   ![Состояние локального запуска команды для средств Data Lake в Visual Studio Code](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/data-lake-tools-for-vscode-localrun-cmd-status.png) 


## <a name="start-a-local-debug-for-the-u-sql-job"></a>Запуск локальной отладки для задания U-SQL  
Для новых пользователей:

1. Используйте **ADL: Скачайте локальный запуск пакета** для загрузки локального выполнения пакетов, если вы еще не [настроить среду для локального запуска U-SQL](#set-up-the-u-sql-local-run-environment).
2. Установите .NET Core SDK 2.0 как предложено в окне сообщения, если не установлен.
 
  ![Напоминание об установке Dotnet](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/remind-install-dotnet.png)
3. Установка C# для Visual Studio Code как предложено в окне сообщения, если не установлен. Нажмите кнопку **установить** чтобы продолжить, а затем перезапустите VSCode.

    ![Напоминание об установке C#](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/install-csharp.png)

Чтобы выполнить отладку локально, сделайте следующее:
  
1. Выберите сочетание клавиш Ctrl + Shift + P, чтобы открыть палитру команд, а затем введите **ADL: Запустите службу локально**. Откроется консоль команд. Убедитесь, что установлено значение параметра **DataRoot**.
2. Установите точку останова в коде программной части C#.
3. К редактор скриптов, щелкните правой кнопкой мыши и выберите **ADL: Локальная отладка**.
    
   ![Результат локальной отладки в средствах Data Lake для Visual Studio Code](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/data-lake-tools-for-vscode-local-debug-result.png)


## <a name="next-steps"></a>Дальнейшие действия
* [Использование средств Azure Data Lake для Visual Studio Code](data-lake-analytics-data-lake-tools-for-vscode.md)
* [Разработка U-SQL с помощью Python, R, и CSharp для Azure Data Lake Analytics в VSCode](data-lake-analytics-u-sql-develop-with-python-r-csharp-in-vscode.md)
* [Приступая к работе с аналитикой озера данных с помощью PowerShell](data-lake-analytics-get-started-powershell.md)
* [Приступая к работе с аналитикой озера данных с помощью портала Azure](data-lake-analytics-get-started-portal.md)
* [Использование инструментов озера данных для Visual Studio для разработки приложений U-SQL](data-lake-analytics-data-lake-tools-get-started.md)
* [Использование каталога U-SQL Azure Data Lake Analytics](data-lake-analytics-use-u-sql-catalog.md)
