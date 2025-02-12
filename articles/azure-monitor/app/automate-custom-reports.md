---
title: Автоматизация настраиваемых отчетов с использованием данных Azure Application Insights
description: Автоматизация настраиваемых ежедневных, еженедельных или ежемесячных отчетов с использованием данных Azure Application Insights
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 05/20/2019
ms.reviewer: sdash
ms.author: mbullwin
ms.openlocfilehash: f22cb620bf8cf56110bec60a4dd809066393a8ff
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "67067664"
---
# <a name="automate-custom-reports-with-azure-application-insights-data"></a>Автоматизация настраиваемых отчетов с использованием данных Azure Application Insights

Благодаря периодическим отчетам команды располагают актуальной информацией о состоянии функционирования критически важных бизнес-служб. Автоматизированные отчеты с надежной доставкой необходимых данных позволяют повысить эффективность труда разработчиков, команд DevOps/SRE и их руководителей. При этом входить на портал для получения сведений не требуется. Эти отчеты также помогают определить постепенный рост частоты задержек, нагрузок или сбоев, подчас не приводящий к запуску правил оповещения.

В каждой организации есть свои уникальные потребности в отчетности, например следующие: 

* определенная процентильная информация по метрикам или настраиваемым метрикам в отчете;
* наличие различных отчетов для ежедневных, еженедельных и ежемесячных сверток данных для различных аудиторий;
* сегментация по настраиваемым атрибутам, таким как регион или среда; 
* объединение некоторых ресурсов AI в одном отчете, даже если они могут находиться в разных подписках или группах ресурсов и т. д.;
* разделение отчетов, содержащих важные метрики, отправляемые выборочной аудитории;
* отчеты для заинтересованных лиц, у которых может отсутствовать доступ к ресурсам портала.

> [!NOTE] 
> Сообщение электронной почты еженедельного дайджеста Application Insights не поддерживало настройки, поэтому вместо него будут использоваться настраиваемые варианты, приведенные ниже. Последнее сообщение электронной почты еженедельного дайджеста будет отправлено 11 июня 2018 г. Настройте один из следующих параметров, чтобы получать аналогичные настраиваемые отчеты (используйте предложенный ниже запрос).

## <a name="to-automate-custom-report-emails"></a>Автоматизация сообщений электронной почты с настраиваемыми отчетами

Для создания настраиваемых отчетов по расписанию можно [программно запрашивать данные Application Insights](https://dev.applicationinsights.io/). Указанные ниже варианты помогут быстро приступить к работе.

* [Автоматизация отчетов с помощью Microsoft Flow](automate-with-flow.md)
* [Автоматизация отчетов с помощью Logic Apps](automate-with-logic-apps.md)
* В сценарии мониторинга используйте [функцию Azure](https://docs.microsoft.com/azure/azure-functions/functions-create-first-azure-function) "Дайджест по расписанию AppInsights". Для доставки сообщения электронной почты эта функция использует SendGrid. 

    ![Шаблон функции Azure](./media/automate-custom-reports/azure-function-template.png)

## <a name="sample-query-for-a-weekly-digest-email"></a>Пример запроса для сообщения электронной почты еженедельного дайджеста
В следующем запросе показано объединение нескольких наборов данных для формирования отчета, аналогичного сообщению электронной почты еженедельного дайджеста. При необходимости настройте его и используйте с любым приведенным выше вариантом для автоматизации еженедельного отчета.   

```AIQL
let period=7d;
requests
| where timestamp > ago(period)
| summarize Row = 1, TotalRequests = sum(itemCount), FailedRequests = sum(toint(success == 'False')),
    RequestsDuration = iff(isnan(avg(duration)), '------', tostring(toint(avg(duration) * 100) / 100.0))
| join (
dependencies
| where timestamp > ago(period)
| summarize Row = 1, TotalDependencies = sum(itemCount), FailedDependencies = sum(success == 'False'),
    DependenciesDuration = iff(isnan(avg(duration)), '------', tostring(toint(avg(duration) * 100) / 100.0))
) on Row | join (
pageViews
| where timestamp > ago(period)
| summarize Row = 1, TotalViews = sum(itemCount)
) on Row | join (
exceptions
| where timestamp > ago(period)
| summarize Row = 1, TotalExceptions = sum(itemCount)
) on Row | join (
availabilityResults
| where timestamp > ago(period)
| summarize Row = 1, OverallAvailability = iff(isnan(avg(toint(success))), '------', tostring(toint(avg(toint(success)) * 10000) / 100.0)),
    AvailabilityDuration = iff(isnan(avg(duration)), '------', tostring(toint(avg(duration) * 100) / 100.0))
) on Row
| project TotalRequests, FailedRequests, RequestsDuration, TotalDependencies, FailedDependencies, DependenciesDuration, TotalViews, TotalExceptions, OverallAvailability, AvailabilityDuration
```

## <a name="application-insights-scheduled-digest-report"></a>Отчет дайджеста планирования Application Insights

1. На портале Azure последовательно выберите **Создать ресурс** > **Вычисление** > **Приложение-функция**.

   ![Снимок экрана "Создание приложения-функции ресурсов Azure"](./media/automate-custom-reports/function-app-01.png)

2. Введите соответствующие данные приложения и нажмите кнопку _Создать_. (_Включение_ Application Insights является обязательным только в том случае, если с его помощью необходимо отследить приложение-функцию Application Insights)

   ![Снимок экрана "Параметры создания приложения-функции ресурсов Azure"](./media/automate-custom-reports/function-app-02.png)

3. После завершения развертывания нового приложения-функции выберите **Перейти к ресурсу**.

4. Выберите **Новая функция**.

   ![Снимок экрана "Создание функции"](./media/automate-custom-reports/function-app-03.png)

5. Выберите **_Шаблон дайджеста планирования Application Insights_** .

     > [!NOTE]
     > По умолчанию приложения-функции создаются на основе среды выполнения версии 2.x. Вы должны [целевая версия среды выполнения функций Azure](https://docs.microsoft.com/azure/azure-functions/set-runtime-version) 1.x для использования Application Insights запланированные хэш-кода шаблона.

   ![Снимок экрана "Шаблон нового приложения-функции анализа"](./media/automate-custom-reports/function-app-04.png)

6. Введите адрес электронной почты получателя отчета и нажмите кнопку **Создать**.

   ![Снимок экрана "Параметры функции"](./media/automate-custom-reports/function-app-05.png)

7. Выберите **Приложение-функция** > **Функции платформы** > **Параметры приложения**.

    ![Снимок экрана "Параметры приложения-функции Azure"](./media/automate-custom-reports/function-app-07.png)

8. Создайте три параметра приложения с соответствующими значениями ``AI_APP_ID``, ``AI_APP_KEY`` и ``SendGridAPI``. Щелкните **Сохранить**.

     ![Снимок экрана "Интерфейс функции интеграции"](./media/automate-custom-reports/function-app-08.png)
    
    (Параметр AI_ для отчета о ресурсе Application Insights можно найти в разделе "Доступ через API". Если отсутствует ключ API Application Insights, то **создать ключ API** можно по другому.)
    
   * AI_APP_ID = идентификатор приложения
   * AI_APP_KEY = ключ API
   * SendGridAPI = ключ API SendGrid

     > [!NOTE]
     > Если у вас нет учетной записи SendGrid, вы можете ее создать. Документация относительно функций Azure SendGrid находится [здесь](https://docs.microsoft.com/azure/azure-functions/functions-bindings-sendgrid). В конце статьи приведено сокращенное объяснение о том, как установить SendGrid и создать ключ API. 

9. Выберите **Интегрировать** и в разделе "Выходные данные" щелкните **SendGrid ($return)** .

     ![Снимок экрана "Выходные данные"](./media/automate-custom-reports/function-app-09.png)

10. В разделе **SendGridAPI Key App Setting** (Параметры приложения API-ключа SendGrid) выберите недавно созданный параметр приложения **SendGridAPI**.

     ![Снимок экрана "Запуск приложения-функции"](./media/automate-custom-reports/function-app-010.png)

11. Запустите и протестируйте приложение-функцию

     ![Снимок экрана "Тестирование"](./media/automate-custom-reports/function-app-11.png)

12. Чтобы убедиться, что сообщение успешно отправлено или получено, необходимо проверить электронную почту.

     ![Снимок экрана "Адресная строка электронной почты"](./media/automate-custom-reports/function-app-12.png)

## <a name="sendgrid-with-azure"></a>SendGrid с Azure

Эти действия применяются только в том случае, если у вас еще нет учетной записи SendGrid.

1. На портале Azure выберите **Create a resource** (Создать ресурс), затем выберите**SendGrid Email Delivery** (Почтовая служба SendGrid) > **Создать** > и заполните указанные поля для создания SendGrid. 

     ![Снимок экрана "Создание ресурса SendGrid"](./media/automate-custom-reports/function-app-13.png)

2. После создания ресурса в разделе учетная запись SendGrid выберите **Управление**.

     ![Снимок экрана "API-ключ"](./media/automate-custom-reports/function-app-14.png)

3. Это действие запустит сайт SendGrid. Выберите **Параметры** > **Ключи API**.

     ![Снимок экрана "Создание и просмотр приложения Ключ API"](./media/automate-custom-reports/function-app-15.png)

4. Создайте ключ API > выберите **Create & View** (Создать и просмотреть) (Ознакомьтесь с документацией SendGrid об ограниченном доступе, чтобы определить, какой уровень разрешений подходит для вашего ключа API. Здесь можно выбрать полный доступ, только в качестве примера).

   ![Снимок экрана "Полный доступ"](./media/automate-custom-reports/function-app-16.png)

5. Скопируйте весь ключ. Это значение является необходимым параметром приложения-функции в качестве значения поля SendGridAPI

   ![Снимок экрана "Копирование ключа API"](./media/automate-custom-reports/function-app-17.png)

## <a name="next-steps"></a>Дальнейшие действия

* Узнайте больше о создании [запросов Analytics](../../azure-monitor/log-query/get-started-queries.md).
* Дополнительные сведения о [программных запросах к данным Application Insights](https://dev.applicationinsights.io/).
* Дополнительные сведения о [Logic Apps](https://docs.microsoft.com/azure/logic-apps/logic-apps-what-are-logic-apps).
* Дополнительные сведения о [Microsoft Flow](https://ms.flow.microsoft.com).
