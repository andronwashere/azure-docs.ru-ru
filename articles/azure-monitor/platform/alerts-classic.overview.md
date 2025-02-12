---
title: Обзор классических оповещений в Microsoft Azure и Azure Monitor
description: Классические оповещения устарели. Оповещения позволяют отслеживать метрики ресурсов Azure, события или журналы и получать уведомления при выполнении заданных условий.
author: rboucher
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 05/19/2018
ms.author: robb
ms.openlocfilehash: 96183e22f0aeafc681a782e7d3d8bd29a6ec9617
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "65914706"
---
# <a name="what-are-classic-alerts-in-microsoft-azure"></a>Что такое классические оповещения в Microsoft Azure?

> [!NOTE]
> В этой статье объясняется, как создавать классические оповещения метрик прежней версии. Azure Monitor теперь поддерживает [новые оповещения метрик практически в реальном времени и новые функции оповещений](../../azure-monitor/platform/alerts-overview.md). [Планируется прекращение поддержки](https://docs.microsoft.com/azure/azure-monitor/platform/monitoring-classic-retirement) классических оповещений.  
>

Оповещения позволяют настроить условия для данных и получать уведомления, когда последние данные мониторинга соответствуют этим условиям.

## <a name="old-and-new-alerting-capabilities"></a>Старые и новые возможности оповещений

В прошлом Azure Monitor, Application Insights, Log Analytics и служба "Работоспособность служб" обладали отдельными возможностями оповещений. Со временем пользовательский интерфейс и различные методы оповещений были улучшены и объединены в Azure. Консолидация все еще выполняется.

Просмотреть классические оповещения можно только на экране классических оповещений пользователя портала Azure. Вы увидите этот экран, нажав на кнопку **Просмотреть классические оповещения** на экране оповещений. 

 ![Варианты оповещений на портале Azure](media/alerts-classic.overview/monitor-alert-screen2.png)

Новый интерфейс оповещений имеет следующие преимущества над классическим вариантом.
-   **Усовершенствованная система уведомлений.** - Во всех новых оповещениях используются группы действий. Это именованные группы уведомлений и действий, которые можно использовать в нескольких оповещениях. В классических оповещениях метрик и оповещениях прежней версии Log Analytics группы действий не используются.
-   **Единая среда разработки** - Все оповещения (для метрик, действий и журналов действий в Azure Monitor, Log Analytics и Application Insights) создаются в едином местоположении.
-   **Просмотр сработавших оповещений Log Analytics на портале Azure**. Теперь вы можете просмотреть в подписке все сработавшие оповещения Log Analytics. Ранее для них использовался отдельный портал.
-   **Разделение сработавших оповещений и правил генерации оповещений**. Правила генерации оповещений (определение условия, которое инициирует оповещение) и сработавшие оповещения (экземпляр выполнения правила оповещения) разнесены, то есть отображаются и настраиваются отдельно.
-   **Улучшенный рабочий процесс**. В новом интерфейсе создания оповещений пользователю предлагаются рекомендации по настройке правил генерации оповещений, которые помогают быстрее разобраться в параметрах и характеристиках оповещений.
-   **Консолидация смарт-оповещений** и **параметры состояния оповещения**.Новые оповещения включают функцию автоматической группировки, отображающую похожие оповещения вместе, чтобы уменьшить перегрузку в пользовательском интерфейсе. 

Более новые оповещения о метриках имеют следующие преимущества по сравнению с классическими.
-   **Уменьшение задержки.** Новые оповещения метрик могут выполняться каждую минуту. Раньше оповещения метрик всегда проверялись с частотой в 5 минут. В новых оповещениях задержка с момента появления проблемы до уведомления или действия меньше (от 3 до 5 минут). В старых оповещениях — от 5 до 15 минут в зависимости от типа.  Задержка в оповещениях журнала обычно составляет от 10 до 15 минут из-за времени, затрачиваемого на прием журналов, но более новые методы обработки сокращают это время. 
-   **Поддержка многомерных метрик.** Вы можете создать оповещение по многомерным метрикам, чтобы отслеживать конкретный сегмент метрики.
-   **Дополнительный контроль метрик.** Вы можете определить расширенные правила генерации оповещений. Новые оповещения поддерживают мониторинг максимальных, минимальных, средних и общих значений метрик.
-   **Объединенный мониторинг нескольких метрик.** Вы сможете выполнять мониторинг нескольких метрик (пока только двух) с помощью одного правила. Оповещение активируется, если обе метрики превышают соответствующие пороговые значения для заданного периода времени.
-   **Усовершенствованная система уведомлений.** Во всех новых оповещениях используются [группы действий](../../azure-monitor/platform/action-groups.md). Это именованные группы уведомлений и действий, которые можно использовать в нескольких оповещениях.  В классических оповещениях метрик и оповещениях прежней версии Log Analytics группы действий не используются. 
-   **Метрики из журналов** (общедоступная предварительная версия). Данные журналов, поступающие в службу Log Analytics, теперь можно извлекать и преобразовывать в метрики Azure Monitor и создавать на их основе оповещения, так же как и на основе других метрик. Для изучения терминологии, характерной для классических оповещений, обратитесь к разделу [What are classic alerts in Microsoft Azure?](alerts-classic.overview.md) (Что такое классические оповещения в Microsoft Azure?). 


## <a name="classic-alerts-on-azure-monitor-data"></a>Классические оповещения для данных Azure Monitor
Существуют два типа классических оповещений: оповещения метрик и оповещения журнала действий.

* **Классические оповещения метрик**. Это оповещение активируется, когда значение указанной метрики выходит за рамки заданного порога. Оповещение формирует уведомление, когда достигается заданный порог и выполняются условия оповещения. В этот момент считается, что оповещение "Активировано". Оповещение генерирует другое уведомление, когда оно "Разрешено", когда пороговое значение снова пересекается, и условие больше не выполняется.

* **Классические оповещения журнала действий** — оповещение журнала потоковой передачи, которое активируется событием журнала действий, которое соответствует вашим условиям фильтров. Эти оповещения имеют только одно состояние — "Активировано". Механизм оповещения просто применяет критерии фильтра к любому новому событию. Он не выполняет поиск, чтобы найти более старые записи. Данные оповещения могут использоваться для получения уведомлений, когда возникает новый инцидент службы "Работоспособность служб" или когда пользователь или приложение выполняет какую-либо операцию в подписке, например "Удаление виртуальной машины".

Для данных журнала диагностики, доступных через Azure Monitor, перенаправьте данные в Log Analytics (прежнее название — OMS) и используйте оповещение о запросе Log Analytics. Теперь в Log Analytics используется [новый метод оповещения](../../azure-monitor/platform/alerts-overview.md). 

На следующей диаграмме показаны источники данных в Azure Monitor и принципы создания оповещений на их основе.

![Работа с оповещениями](media/alerts-classic.overview/Alerts_Overview_Resource_v5.png)

## <a name="taxonomy-of-alerts-classic"></a>Классификация оповещений (классических)
Для описания классических оповещений и их функций в Azure используются следующие термины.
* **Оповещение**. Определение условий (одно или несколько правил либо условий), которое активируется при соблюдении этих условий.
* **Активное**. Состояние, когда выполняются условия, определенные в классическом оповещении.
* **Разрешено**. Состояние, когда условия классического оповещения больше не выполняются после того, как они были выполнены.
* **Уведомление**. Действие, которое выполняется при активации классического оповещения.
* **Действие**. Определенный вызов, отправляемый получателю уведомления (например, отправка сообщения на электронный адрес или передача данных на URL-адрес веб-перехватчика). Обычно уведомления могут активировать несколько действий.

## <a name="how-do-i-receive-a-notification-from-an-azure-monitor-classic-alert"></a>Как получить уведомление о классическом оповещении Azure Monitor?
Исторически так сложилось, что оповещения Azure от различных служб использовали собственные встроенные методы уведомления. 

В Azure Monitor создано многократно используемое группирование уведомлений, называемое *группами действий*. Группы действий позволяют указать получателей уведомления. Каждый раз, когда активируется оповещение, которое ссылается на группу действий, все получатели получают соответствующее уведомление. Группы уведомлений позволяют многократно использовать группирование получателей (например, список дежурных инженеров) для многих объектов оповещений. Помимо рассылки писем на адреса электронной почты и SMS-сообщений номера телефонов, группы действий поддерживают уведомление путем передачи данных на URL-адрес веб-перехватчика, а также многие другие действия.  Дополнительные сведения см. в разделе [Создание групп действий и управление ими на портале Azure](../../azure-monitor/platform/action-groups.md). 

Более старые классические оповещения журнала действий используют группы действий.

Однако более старые оповещения метрик не используют группы действий. Вместо этого можно настроить следующие действия: 
- отправка уведомлений по электронной почте администратору службы, соадминистраторам или на дополнительные электронные адреса, указанные вами;
- вызов webhook, который позволяет запускать дополнительные автоматизированные действия.

Веб-перехватчики позволяют автоматизировать работу и вносить исправления, например с помощью:
- Runbook службы автоматизации Azure
- функции Azure;
- Приложение логики Azure
- сторонней службы.

## <a name="next-steps"></a>Дальнейшие действия
Изучите правила создания оповещений и ознакомьтесь с их настройкой с помощью следующих средств:

* Узнайте больше о [метриках](data-platform.md).
* Настройте [оповещения метрик на портале Azure](alerts-classic-portal.md).
* Настройте [классические оповещения метрик с помощью PowerShell](alerts-classic-portal.md).
* Настройте [классические оповещения метрик с помощью интерфейса командной строки](alerts-classic-portal.md).
* Настройте [классические оповещения метрик с помощью REST API Azure Monitor](https://msdn.microsoft.com/library/azure/dn931945.aspx).
* Узнайте больше о [журнале действий](activity-logs-overview.md).
* Настройте [оповещения журнала действий на портале Azure](activity-log-alerts.md).
* Настройте [оповещения журнала действий с помощью Resource Manager](alerts-activity-log.md).
* Просмотрите [схему webhook оповещений журнала действий](activity-log-alerts-webhook.md).
* Дополнительные сведения о [группах действий](action-groups.md).
* Настройте [новые оповещения](alerts-metric.md).
