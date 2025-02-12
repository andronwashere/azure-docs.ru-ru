---
title: Мониторинг приложений логики с помощью журналов Azure Monitor — Azure Logic Apps | Документация Майкрософт
description: Получите аналитические сведения и данные об отладке при выполнении приложения логики с помощью Azure Log Analytics для устранения неполадок и диагностики
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: jonfan, estfan, LADocs
ms.topic: article
ms.date: 10/19/2018
ms.openlocfilehash: 3f890e6cabd757fdd38374befaaccd1a10c9bd96
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "62106216"
---
# <a name="monitor-logic-apps-with-azure-monitor-logs"></a>Мониторинг приложений логики с помощью журналов Azure Monitor

Чтобы отслеживать и получение подробных данных отладки о приложениях логики, включите [журналы Azure Monitor](../log-analytics/log-analytics-overview.md) при создании приложения логики. Журналы Azure Monitor предоставляет ведение журнала диагностики и мониторинга для приложений логики, если установка решения Logic Apps Management на портале Azure. Это решение также предоставляет статистические данные для выполнений приложений логики с конкретными сведениями, такими как состояние, время выполнения, состояние повторной отправки и идентификаторы корреляции. В этой статье показано, как включить журналы Azure Monitor, можно просмотреть события среды выполнения и запускает данных для приложения логики.

Чтобы включить журналы Azure Monitor для имеющихся приложений логики, выполните следующие действия, чтобы [включить ведение журнала диагностики и отправку данных среды выполнения приложений логики к журналам монитора Azure](../logic-apps/logic-apps-monitor-your-logic-apps.md#azure-diagnostics).

> [!NOTE]
> На этой странице ранее описывались шаги по выполнению таких задач с помощью консоли Microsoft Operations Management Suite (OMS), которая [выводится из эксплуатации в январе 2019 г.](../azure-monitor/platform/oms-portal-transition.md) Теперь эти шаги выполняются с помощью Azure Log Analytics. 

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]

## <a name="prerequisites"></a>Технические условия

Чтобы начать работу, вам потребуется рабочая область Log Analytics. Узнайте, [как создать рабочую область Log Analytics](../azure-monitor/learn/quick-create-workspace.md). 

## <a name="turn-on-diagnostics-logging-when-creating-logic-apps"></a>Включение ведения журнала диагностики при создании приложений логики

1. На [портале Azure](https://portal.azure.com) создайте приложение логики. Выберите **Создать ресурс** > **Интеграция** > **Приложение логики**.

   ![Создайте приложение логики](media/logic-apps-monitor-your-logic-apps-oms/find-logic-apps-azure.png)

1. На странице **Создание приложения логики** выполните показанные ниже действия.

   1. Введите имя приложения логики и выберите подписку Azure. 

   1. Создайте или выберите существующую группу ресурсов Azure.

   1. Установите переключатель **Log Analytics** в положение **Вкл.** 

   1. Выберите рабочую область из списка рабочей области Log Analytics, в которую будут отправляться данные о выполнении приложения логики. 

      ![Создание приложения логики](./media/logic-apps-monitor-your-logic-apps-oms/create-logic-app.png)

      По завершении этого шага Azure создает приложение логики, которое теперь связано с рабочей областью Log Analytics. 
      На этом шаге в рабочую область автоматически устанавливается решение по управлению Logic Apps.

   1. Когда все будет готово, выберите **Создать**.

1. Чтобы просмотреть сведения о запусках приложений логики, [выполните описанные здесь действия](#view-logic-app-runs-oms).

## <a name="install-logic-apps-management-solution"></a>Установка решения Logic Apps Management

Если вы уже включили журналы Azure Monitor при создании приложения логики, пропустите этот шаг. Решение по управлению Logic Apps уже установлено.

1. На [портале Azure](https://portal.azure.com) выберите **Все службы**. В поле поиска введите "log analytics" и выберите **Log Analytics**.

   ![Выбор "Log Analytics"](./media/logic-apps-monitor-your-logic-apps-oms/find-log-analytics.png)

1. В разделе **Log Analytics** найдите и выберите рабочую область Log Analytics. 

   ![Выбор рабочей области Log Analytics](./media/logic-apps-monitor-your-logic-apps-oms/select-log-analytics-workspace.png)

1. В разделе **Начало работы с Log Analytics** > **Настройка решений мониторинга** выберите **Просмотреть решения**.

   ![Выбор "Просмотреть решения"](media/logic-apps-monitor-your-logic-apps-oms/log-analytics-workspace.png)

1. На странице обзора выберите **Добавить**, чтобы открыть список **Решений по управлению**. Из этого списка выберите **Logic Apps Management**. 

   ![Выбор "Logic Apps Management"](./media/logic-apps-monitor-your-logic-apps-oms/add-logic-apps-management-solution.png)

   Если не удается найти решение, в нижней части списка выберите **Загрузить еще**, пока не появится решение.

1. Выберите **Создать**, затем подтвердите, в какой рабочей области Log Analytics вы хотите установить решение и еще раз щелкните **Создать**.   

   ![Кнопка "Создать" для решения Logic Apps Management](./media/logic-apps-monitor-your-logic-apps-oms/create-logic-apps-management-solution.png)

   Если вы не хотите использовать существующую рабочую область, можно прямо сейчас создать новую.

   Когда все будет готово, решение Logic Apps Management появится на странице "Обзор". 

<a name="view-logic-app-runs-oms"></a>

## <a name="view-logic-app-run-information"></a>Просмотр сведений о выполнении приложения логики

Когда приложение логики завершит работу, состояние и количество выполнений можно просмотреть на плитке **Logic Apps Management**. 

1. Перейдите к рабочей области Log Analytics и откройте страницу "Обзор". Выберите **Logic Apps Management**. 

   ![Состояние выполнения приложения логики и счетчик](media/logic-apps-monitor-your-logic-apps-oms/overview.png)

   Здесь сведения о выполнении приложений логики группируются по имени или по состоянию выполнения. 
   Кроме того, можно просмотреть сведения о сбоях в действиях или триггерах для выполнений приложения логики.

   ![Сводные данные о состоянии выполнения приложений логики](media/logic-apps-monitor-your-logic-apps-oms/logic-apps-runs-summary.png)
   
1. Чтобы просмотреть все сведения о выполнении конкретного приложения логики или о конкретном состоянии, выберите строку с соответствующим приложением логики или состоянием.

   Ниже приведен пример, в котором показаны все сведения о выполнении конкретного приложения логики.

   ![Просмотр сведений о выполнении по приложению логики или по состоянию](media/logic-apps-monitor-your-logic-apps-oms/logic-app-run-details.png)

   Эта страница содержит следующие дополнительные параметры.

   * **Отслеживаемые свойства**.

     В этом столбце отображаются отслеживаемые свойства для приложения логики, которые группируются по действиям. Чтобы просмотреть отслеживаемые свойства, щелкните **Просмотреть**. 
     Для поиска отслеживаемых свойств можно использовать фильтр столбцов.
   
     ![Просмотр отслеживаемых свойств для приложения логики](media/logic-apps-monitor-your-logic-apps-oms/logic-app-tracked-properties.png)

     Прежде чем любое новое добавленное отслеживаемое свойство появится впервые, может пройти 10–15 минут. Узнайте, [как добавить отслеживаемые свойства в приложение логики](logic-apps-monitor-your-logic-apps.md#azure-diagnostics-event-settings-and-details).

   * **Отправить повторно**. Можно повторно отправить одну или несколько операций приложения логики, завершившихся сбоем, выполненных успешно или все еще выполняемых. Установите флажки для выполнений, которые необходимо отправить повторно, и щелкните **Отправить повторно**. 

     ![Повторная отправка выполнений приложения логики](media/logic-apps-monitor-your-logic-apps-oms/logic-app-resubmit.png)

1. Чтобы отфильтровать эти результаты, можно применить фильтр как на стороне клиента, так и на стороне сервера.

   * **Клиентский фильтр**. Для каждого столбца выберите необходимые фильтры.

     ![Пример фильтров столбцов](media/logic-apps-monitor-your-logic-apps-oms/filters.png)

   * **Серверный фильтр**. Чтобы выбрать определенное временное окно или ограничить отображаемое количество выполнений, воспользуйтесь элементом ограничения области в верхней части страницы. По умолчанию за раз отображается только 1000 записей.
   
     ![Изменение временного окна](media/logic-apps-monitor-your-logic-apps-oms/change-interval.png)
 
1. Чтобы просмотреть все действия и сведения о них для определенного выполнения приложения логики, выберите его строку.

   Ниже приведен пример, в котором показаны все действия при определенном выполнении приложения логики.

   ![Просмотр действий для выполнения приложения логики](media/logic-apps-monitor-your-logic-apps-oms/logic-app-action-details.png)
   
1. На любой странице результатов, чтобы просмотреть результаты запроса или все результаты, щелкните элемент **Показать все**, которая открывает страницу "Поиск по журналу".
   
   ![Элемент "Показать все" на странице "Результаты"](media/logic-apps-monitor-your-logic-apps-oms/logic-app-seeall.png)
   
   На странице "Поиск по журналу" сделайте следующее.

   * Чтобы просмотреть результаты запроса в таблице, выберите **Таблица**.

   * Чтобы изменить запрос, отредактируйте строку запроса на панели поиска. 
   Для доступа к дополнительным функциональным возможностям выберите **Расширенная аналитика**.

     ![Просмотр действий и сведений о них для выполнения приложения логики](media/logic-apps-monitor-your-logic-apps-oms/log-search-page.png)
     
     На странице log analytics можно обновить запросы и просматривать результаты в таблице. В этом запросе используется [язык запросов Kusto](https://aka.ms/LogAnalyticsLanguageReference), который можно изменять, если требуется просмотреть другие результаты. 

     ![log analytics — представление запроса](media/logic-apps-monitor-your-logic-apps-oms/query.png)

## <a name="next-steps"></a>Дальнейшие действия

* [Мониторинг сообщений B2B](../logic-apps/logic-apps-monitor-b2b-message.md)