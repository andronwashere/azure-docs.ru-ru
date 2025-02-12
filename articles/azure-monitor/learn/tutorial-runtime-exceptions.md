---
title: Диагностика исключений во время выполнения с помощью Azure Application Insights | Документация Майкрософт
description: Руководство по поиску и диагностике исключений во время выполнения в приложении с помощью Azure Application Insights.
services: application-insights
keywords: ''
author: mrbullwinkle
ms.author: mbullwin
ms.date: 09/19/2017
ms.service: application-insights
ms.custom: mvc
ms.topic: tutorial
manager: carmonm
ms.openlocfilehash: 19455998ca13b9abf48bb1cb3856e38b5c47ef52
ms.sourcegitcommit: 1fbc75b822d7fe8d766329f443506b830e101a5e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/14/2019
ms.locfileid: "65595605"
---
# <a name="find-and-diagnose-run-time-exceptions-with-azure-application-insights"></a>Поиск и диагностика исключений во время выполнения с помощью Azure Application Insights

Azure Application Insights собирает данные телеметрии из приложения для определения и диагностики исключений во время выполнения.  В этом руководстве содержатся сведения о выполнении этого процесса для вашего приложения.  Вы узнаете, как выполнять следующие задачи:

> [!div class="checklist"]
> * Изменение проекта для включения функции отслеживания исключений.
> * Определение исключения для различных компонентов приложения.
> * Просмотр сведений об исключении.
> * Скачивание моментального снимка исключения в Visual Studio для выполнения отладки.
> * Анализ сведений о неудачных запросах с помощью языка запросов.
> * Создание рабочего элемента для исправления ошибочного кода.


## <a name="prerequisites"></a>Предварительные требования

Для работы с этим руководством:

- Установите [Visual Studio 2019](https://www.visualstudio.com/downloads/) с указанными ниже рабочими нагрузками:
    - ASP.NET и веб-разработка.
    - разработка Azure;
- Загрузите и установите [отладчик моментальных снимков Visual Studio](https://aka.ms/snapshotdebugger).
- Включите [отладчик моментальных снимков Visual Studio](https://docs.microsoft.com/azure/application-insights/app-insights-snapshot-debugger).
- Разверните приложение .NET в Azure и [включите пакет SDK для Application Insights](../../azure-monitor/app/asp-net.md). 
- Это руководство содержит сведения об отслеживании идентификатора исключения в вашем приложении, поэтому измените свой код в среде разработки или тестирования, чтобы создать исключение. 

## <a name="log-in-to-azure"></a>Вход в Azure
Войдите на портал Azure по адресу [https://portal.azure.com](https://portal.azure.com).


## <a name="analyze-failures"></a>Анализ сбоев
Application Insights собирает сведения об ошибках в вашем приложении и предоставляет возможность просматривать частоту их появления во время различных операций, чтобы помочь вам сконцентрировать усилия на наиболее важных из них.  Вы можете просмотреть сведения об этих ошибках, чтобы определить их первопричину.   

1. Выберите **Application Insights**, а затем выберите подписку.  
2. Чтобы открыть панель **Сбои**, выберите **Сбои** в меню **Исследовать** или выберите диаграмму **Неудачные запросы**.

    ![Failed requests (Неудачные запросы)](media/tutorial-runtime-exceptions/failed-requests.png)

3. На панели **неудачных запросов** отображается количество неудачных запросов и количество затронутых пользователей каждой операции приложения.  Отсортировав эту информацию по пользователям, вы можете определить ошибки, которые больше всего влияют на пользователей.  В этом примере операции **GET Employees/Create** и **GET Customers/Details** наиболее вероятные кандидаты для анализа из-за их высокого количества ошибок и затронутых пользователей.  Выберите операцию, чтобы отобразить о ней дополнительную информацию на панели справа.

    ![Панель неудачных запросов](media/tutorial-runtime-exceptions/failed-requests-blade.png)

4. Сократите временное окно, чтобы увеличить масштаб периода с наиболее вероятным риском сбоя.

    ![Окно неудачных запросов](media/tutorial-runtime-exceptions/failed-requests-window.png)

5. Просмотрите связанные примеры, нажав кнопку с числом отфильтрованных результатов. Предложенные примеры включают связанные данные телеметрии из всех компонентов, даже если выборка могла применяться только к одному из них. Щелкните результат поиска, чтобы просмотреть сведения о сбое.

    ![Примеры неудачно завершенных запросов](media/tutorial-runtime-exceptions/failed-requests-search.png)

6. Сведения о невыполненных запросах включают диаграмму Ганта. На ней видно две ошибки зависимостей в этой транзакции, что также добавило более 50 % от общей продолжительности транзакции. Этот интерфейс представляет все данные телеметрии от компонентов распределенного приложения, которые связаны с этим идентификатором операции. [См. дополнительные сведения о новом интерфейсе](../../azure-monitor/app/transaction-diagnostics.md). Щелкните любой элемент, чтобы просмотреть соответствующие сведения в области справа. 

    ![Сведения о невыполненных запросах](media/tutorial-runtime-exceptions/failed-request-details.png)

7. В сведениях об операциях также отображаются исключения FormatException, которые, по-видимому, вызвали сбой.  Вы увидите, что причиной является недопустимый почтовый индекс. Вы можете открыть отладочный моментальный снимок, чтобы просмотреть сведения уровня отладки кода в Visual Studio.

    ![Сведения об исключении](media/tutorial-runtime-exceptions/failed-requests-exception.png)

## <a name="identify-failing-code"></a>Определение неисправного кода
Отладчик моментальных снимков собирает моментальные снимки наиболее частых исключений в приложении, чтобы помочь вам определить их первопричину в рабочей среде.  Вы можете просмотреть отладочные моментальные снимки на портале, чтобы изучить стек вызовов и проверить значения переменных в каждом кадре стека вызовов. Затем вы можете выполнить отладку исходного кода, скачав моментальный снимок и открыв его в Visual Studio 2019 Enterprise.

1. В свойствах исключения выберите **Open debug snapshot** (Открыть отладочный моментальный снимок).
2. Откроется панель **отладки моментального снимка** со стеком вызова для запроса.  Выберите любой из методов, чтобы просмотреть значения локальных переменных во время запроса.  Начиная с верхнего метода, в этом примере мы можем увидеть локальные переменные без значений.

    ![Отладка моментального снимка](media/tutorial-runtime-exceptions/debug-snapshot-01.png)

3. Первый вызов с допустимыми значениями — это **ValidZipCode**. Мы можем увидеть, что почтовый индекс содержал буквы, которые нельзя преобразовать в целевое число.  Это вызовет ошибку в коде, которую необходимо исправить.

    ![Отладка моментального снимка](media/tutorial-runtime-exceptions/debug-snapshot-02.png)

4. После этого можно загрузить моментальный снимок в Visual Studio, где мы можем определить фактический код, который нужно исправить. Для этого щелкните **Скачать моментальный снимок**.
5. Моментальный снимок будет загружен в Visual Studio.
6. Теперь в Visual Studio Enterprise можно запустить сеанс отладки, который позволяет быстро определить строку кода, вызвавшую исключение.

    ![Исключение в коде](media/tutorial-runtime-exceptions/exception-code.png)


## <a name="use-analytics-data"></a>Использование данных аналитики
Все данные, собранные Application Insights, хранятся в службе Azure Log Analytics, которая предоставляет полнофункциональный язык запросов, позволяющий анализировать данные разными способами.  Эти данные можно использовать для анализа запросов, создавших исключения, которые мы ищем. 

1. Выберите сведения CodeLens, расположенные над кодом, чтобы просмотреть данные телеметрии, предоставленные Application Insights.

    ![Код](media/tutorial-runtime-exceptions/codelens.png)

1. Выберите **Анализировать влияние**, чтобы открыть аналитику Application Insights.  Она содержит несколько запросов, предоставляющих сведения о неудачных запросах, например сведения о затронутых пользователях, браузерах и регионах.<br><br>![Аналитика в Application Insights](media/tutorial-runtime-exceptions/analytics.png)<br>

## <a name="add-work-item"></a>Добавление рабочего элемента
Если подключить Application Insights к системе отслеживания, например Azure DevOps или GitHub, можно создать рабочий элемент непосредственно в Application Insights.

1. Вернитесь на панель **Свойства исключения** в Application Insights.
2. Выберите **Создать рабочий элемент**.
3. Откроется панель **Создать рабочий элемент**, содержащая сведения об уже добавленных исключениях.  Вы можете добавить любую дополнительную информацию, прежде чем ее сохранить.

    ![Создать рабочий элемент](media/tutorial-runtime-exceptions/new-work-item.png)

## <a name="next-steps"></a>Дополнительная информация
Теперь, когда вы узнали, как определить исключения времени выполнения, перейдите к следующему руководству, чтобы научиться определять и диагностировать проблемы с производительностью.

> [!div class="nextstepaction"]
> [Поиск и диагностика проблем производительности с помощью Azure Application Insights](../../azure-monitor/learn/tutorial-performance.md)