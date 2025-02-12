---
title: Общие сведения об API потребления ресурсов Azure | Документация Майкрософт
description: Узнайте, как интерфейсы API потребления ресурсов Azure предоставляют программный доступ к сведениям о стоимости и потреблении ресурсов Azure.
services: billing
documentationcenter: ''
author: bandersmsft
manager: dougeby
editor: ''
tags: billing
ms.assetid: 68825e85-de64-466d-b11a-8baffde836b5
ms.service: billing
ms.devlang: na
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: billing
ms.date: 6/07/2018
ms.author: banders
ms.openlocfilehash: 2a2922daef5e75af41252119f9a467fae976bcda
ms.sourcegitcommit: a874064e903f845d755abffdb5eac4868b390de7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/24/2019
ms.locfileid: "68443270"
---
# <a name="azure-consumption-api-overview"></a>Общие сведения об API потребления ресурсов Azure 

Интерфейсы API потребления ресурсов Azure предоставляют программный доступ к сведениям о стоимости и потреблении ресурсов Azure. Сейчас эти API поддерживают только регистрации Enterprise и подписки Web Direct (за некоторыми исключениями). API-интерфейсы постоянно обновляются для поддержки других типов подписок Azure.

Интерфейсы API потребления в Azure обеспечивают доступ к следующим сведениям:
- Для пользователей Enterprise и Web Direct: 
    - сведения о потреблении; 
    - сведения о расходах на Marketplace; 
    - рекомендации по резервированию; 
    - Сведения о резервировании 
    - Сводные данные о резервировании 
- Только для пользователей Enterprise: 
    - прейскурант; 
    - сведения о бюджете; 
    - сведения о балансе. 

## <a name="usage-details-api"></a>API сведений о потреблении

API сведений о потреблении позволяет получить данные стоимости и потребления для всех собственных ресурсов Azure. Эти данные предоставляются в форме записей о потреблении, которые сейчас создаются ежедневно для каждого средства измерения по каждому ресурсу. Такие сведения позволяют оценить совокупные расходы по всем ресурсам или изучить показатели затрат и потребления для конкретных ресурсов. 

API включает в себя следующие функции:

-   **Данные об измеренном уровне потребления.** Сюда входят такие данные, как расходы на потребление, сведения о средстве измерения, от которого поступают данные расходов, и о том, к какому ресурсу Azure относятся эти расходы. Все записи со сведениями о потреблении сопоставляются с данными ежедневного контейнера.
-   **Управление доступом на основе ролей Azure.** Настройте политики доступа с помощью [портала Azure](https://portal.azure.com), [Azure CLI](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-cli) или [командлетов Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview), чтобы указать, какие пользователи или приложения могут получить доступ к сведениям о потреблении в рамках подписки. Вызывающие объекты должны использовать стандартные маркеры Azure Active Directory для проверки подлинности. Добавьте вызывающий объект в роль читателя счетов, читателя, владельца или участника, чтобы получить доступ к данным об использовании для конкретной подписки Azure. 
-   **Фильтрация.** Сократите набор результатов, полученных от API, до меньшего набора записей о потреблении с помощью следующих фильтров:
    - конец и начало периода потребления;
    - Группа ресурсов
    - Имя ресурса
-   **Объединение данных.** С помощью OData вы можете применить к сведениям о потреблении статистические выражения, используя теги или свойства фильтров.
-   **Потребление для разных типов предложений.** Сведения о потреблении сейчас доступны для пользователей Enterprise и Web Direct.

Дополнительные сведения вы найдете в технической спецификации [API сведений о потреблении](https://docs.microsoft.com/rest/api/consumption/usagedetails).

## <a name="marketplace-charges-api"></a>API расходов на Marketplace

Используйте API расходов Marketplace, чтобы получить сведения о расходах и потреблении для всех ресурсов Marketplace (предложения сторонних производителей для Azure). Эти сведения позволяют оценить совокупные расходы по всем ресурсам Marketplace или изучить показатели затрат и потребления для конкретных ресурсов. 

API включает в себя следующие функции:

-   **Данные об измеренном уровне потребления.** Сюда входят такие данные, как расходы на потребление для Marketplace, сведения о средстве измерения, от которого поступают данные о расходах, и о том к какому ресурсу Azure относятся эти расходы. Все записи со сведениями о потреблении сопоставляются с данными ежедневного контейнера.
-   **Управление доступом на основе ролей Azure.** Настройте политики доступа с помощью [портала Azure](https://portal.azure.com), [Azure CLI](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-cli) или [командлетов Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview), чтобы указать, какие пользователи или приложения могут получить доступ к сведениям о потреблении в рамках подписки. Вызывающие объекты должны использовать стандартные маркеры Azure Active Directory для проверки подлинности. Добавьте вызывающий объект в роль читателя счетов, читателя, владельца или участника, чтобы получить доступ к данным об использовании для конкретной подписки Azure. 
-   **Фильтрация.** Сократите набор результатов, полученных от API, до меньшего набора записей о Marketplace с помощью следующих фильтров:
    - начало и (или) конец периода потребления;
    - Группа ресурсов
    - Имя ресурса
-   **Потребление для разных типов предложений.** Сведения о потреблении для Marketplace в настоящее время доступны для пользователей Enterprise и Web Direct.

Дополнительные сведения вы найдете в технической спецификации [API расходов для Marketplace](https://docs.microsoft.com/rest/api/consumption/marketplaces).

## <a name="balances-api"></a>API балансов

С помощью API балансов пользователи Enterprise могут получать ежемесячную сводку по состоянию баланса, новым покупкам, расходам на службы Azure Marketplace, корректировкам и начислению платы за превышение. Эту информацию можно получить для текущего расчетного периода или за любой период в прошлом. Предприятия могут использовать эти данные для сравнения с суммами расходов, вычисленными вручную. Этот API не предоставляет информацию по конкретным ресурсам и статистическое представление затрат. 

API включает в себя следующие функции:

-   **Управление доступом на основе ролей Azure.** Настройте политики доступа с помощью [портала Azure](https://portal.azure.com), [Azure CLI](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-cli) или [командлетов Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview), чтобы указать, какие пользователи или приложения могут получить доступ к сведениям о потреблении в рамках подписки. Вызывающие объекты должны использовать стандартные маркеры Azure Active Directory для проверки подлинности. Добавьте вызывающий объект в роль читателя счетов, читателя, владельца или участника, чтобы получить доступ к данным об использовании для конкретной подписки Azure. 
-   **Только для пользователей Enterprise.** Этот API доступен только для пользователей EA. 
    - Чтобы обращаться к этому API, пользователю нужны права администратора организации. 

Дополнительные сведения вы найдете в технической спецификации [API балансов](https://docs.microsoft.com/rest/api/consumption/balances).

## <a name="budgets-api"></a>API бюджетов

С помощью этого API-интерфейса пользователи Enterprise могут создавать бюджеты затрат или потребления для ресурсов, групп ресурсов и (или) средств измерения для выставления счетов. Определив эту информацию, можно настроить оповещения о превышении порогов бюджета, установленных пользователем. 

API включает в себя следующие функции:

-   **Управление доступом на основе ролей Azure.** Настройте политики доступа с помощью [портала Azure](https://portal.azure.com), [Azure CLI](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-cli) или [командлетов Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview), чтобы указать, какие пользователи или приложения могут получить доступ к сведениям о потреблении в рамках подписки. Вызывающие объекты должны использовать стандартные маркеры Azure Active Directory для проверки подлинности. Добавьте вызывающий объект в роль читателя счетов, читателя, владельца или участника, чтобы получить доступ к данным об использовании для конкретной подписки Azure. 
-   **Только для пользователей Enterprise.** Этот API доступен только для пользователей EA.
-   **Настраиваемые уведомления.** Укажите пользователя или пользователей, которых нужно уведомлять о превышении бюджета.
-   **Бюджеты на основе показателей потребления или затрат.** Создайте свой бюджет на основе сведений о потреблении или затратах в соответствии с конкретными задачами и потребностями.
-   **Фильтрация.** Отфильтруйте бюджет до меньшего подмножества ресурсов с помощью следующих настраиваемых фильтров:
    - Группа ресурсов
    - Имя ресурса
    - Отслеживание использования
-   **Настраиваемые периоды бюджетирования.** Укажите, как часто сбрасываются данные для бюджета и как долго он действует.

Дополнительные сведения вы найдете в технической спецификации [API бюджетов](https://docs.microsoft.com/rest/api/consumption/budgets).

## <a name="reservation-recommendations-api"></a>API рекомендаций по резервированию

Этот API позволяет получать рекомендации по приобретению зарезервированных экземпляров виртуальных машин. Эти рекомендации помогут пользователям оценить ожидаемую экономию и объемы закупок. 

API включает в себя следующие функции:

-   **Управление доступом на основе ролей Azure.** Настройте политики доступа с помощью [портала Azure](https://portal.azure.com), [Azure CLI](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-cli) или [командлетов Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview), чтобы указать, какие пользователи или приложения могут получить доступ к сведениям о потреблении в рамках подписки. Вызывающие объекты должны использовать стандартные маркеры Azure Active Directory для проверки подлинности. Добавьте вызывающий объект в роль читателя счетов, читателя, владельца или участника, чтобы получить доступ к данным об использовании для конкретной подписки Azure. 
-   **Фильтрация.** Сократите результаты с рекомендациями с помощью следующих фильтров:
    - `Scope`
    - период ретроспективного обзора.
-   **Сведения о резервировании для разных типов предложений.** Сведения о резервировании сейчас доступны для пользователей Enterprise и Web Direct.

Дополнительные сведения вы найдете в технической спецификации [API рекомендаций по резервированию](https://docs.microsoft.com/rest/api/consumption/reservationrecommendations).

## <a name="reservation-details-api"></a>API сведений о резервировании

С помощью API сведений о резервировании вы можете просмотреть сведения о приобретенных ранее зарезервированных виртуальных машинах, например результаты сравнения зарезервированных и фактических объемов потребления. Вы можете отобразить данные с детализацией до конкретной виртуальной машины. 

API включает в себя следующие функции:

-   **Управление доступом на основе ролей Azure.** Настройте политики доступа с помощью [портала Azure](https://portal.azure.com), [Azure CLI](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-cli) или [командлетов Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview), чтобы указать, какие пользователи или приложения могут получить доступ к сведениям о потреблении в рамках подписки. Вызывающие объекты должны использовать стандартные маркеры Azure Active Directory для проверки подлинности. Добавьте вызывающий объект в роль читателя счетов, читателя, владельца или участника, чтобы получить доступ к данным об использовании для конкретной подписки Azure. 
-   **Фильтрация**. Сократите набор результатов, полученных от API, до меньшего набора записей о резервировании с помощью следующего фильтра:
    - Диапазон дат
-   **Сведения о резервировании для разных типов предложений.** Сведения о резервировании сейчас доступны для пользователей Enterprise и Web Direct.

Дополнительные сведения вы найдете в технической спецификации [API сведений о резервировании](https://docs.microsoft.com/rest/api/consumption/reservationsdetails).

## <a name="reservation-summaries-api"></a>API сводок по резервированию

Этот API позволяет получить сводную информацию по приобретенным ранее зарезервированным виртуальным машинам, например результаты сравнения общих зарезервированных и фактических объемов потребления. 

API включает в себя следующие функции:

-   **Управление доступом на основе ролей Azure.** Настройте политики доступа с помощью [портала Azure](https://portal.azure.com), [Azure CLI](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-cli) или [командлетов Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview), чтобы указать, какие пользователи или приложения могут получить доступ к сведениям о потреблении в рамках подписки. Вызывающие объекты должны использовать стандартные маркеры Azure Active Directory для проверки подлинности. Добавьте вызывающий объект в роль читателя счетов, читателя, владельца или участника, чтобы получить доступ к данным об использовании для конкретной подписки Azure. 
-   **Фильтрация**. Если используется детализация по дням, сократите результаты с помощью следующего фильтра:
    - Дата использования
-   **Сведения о резервировании для разных типов предложений.** Сведения о резервировании сейчас доступны для пользователей Enterprise и Web Direct.
-   **Суммирование по дням или месяцам.** Вызывающие объекты могут указать, как суммировать сводные данные о резервировании — по дням или по месяцам.

Дополнительные сведения вы найдете в технической спецификации [API сводок по резервированию](https://docs.microsoft.com/rest/api/consumption/reservationssummaries).

## <a name="price-sheet-api"></a>API прейскурантов
Пользователи Enterprise могут использовать этот API, чтобы получать сведения о ценах для всех средств измерения. Организации могут объединять эти данные со сведениями о потреблении для собственных ресурсов и Marketplace, чтобы рассчитать затраты на них. 

API включает в себя следующие функции:

-   **Управление доступом на основе ролей Azure.** Настройте политики доступа с помощью [портала Azure](https://portal.azure.com), [Azure CLI](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-cli) или [командлетов Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview), чтобы указать, какие пользователи или приложения могут получить доступ к сведениям о потреблении в рамках подписки. Вызывающие объекты должны использовать стандартные маркеры Azure Active Directory для проверки подлинности. Добавьте вызывающий объект в роль читателя счетов, читателя, владельца или участника, чтобы получить доступ к данным об использовании для конкретной подписки Azure. 
-   **Только для пользователей Enterprise.** Этот API доступен только для пользователей EA. Пользователи Web Direct могут получить сведения о ценах через API RateCard. 

Дополнительные сведения вы найдете в технической спецификации [API прейскурантов](https://docs.microsoft.com/rest/api/consumption/pricesheet).

## <a name="scenarios"></a>Сценарии

Ниже приведены примеры сценариев, которые можно реализовать с помощью API-интерфейсов потребления:

-   **Сверка счетов.** Правильно ли корпорация Майкрософт определила сумму к оплате?  Сколько мне нужно платить и как мне проверить эти вычисления?
-   **Взаимозачет.** Я знаю сумму своих расходов, а как их распределить между субъектами внутри организации?
-   **Оптимизация издержек.** Я знаю сумму своих расходов, ... но могу ли я более эффективно использовать средства, которые тратятся на Azure?
-   **Отслеживание затрат.** Я хочу точно контролировать суммы затрат и объемы потребления за разные периоды времени. Какие тенденции здесь можно заметить? Что и в какой области я могу улучшить?
-   **Затраты на Azure в течение месяца.** Какая сумма расходов уже накопилась за текущий месяц? Возможно, нужно как-то скорректировать расходы и (или) режим потребления ресурсов Azure? В какой период месяца ресурсы Azure используются наиболее активно?
-   **Настройка оповещений.** Я хочу настроить оповещения о бюджете, основанные на объемах потребления ресурсов или суммах расходов в денежном выражении.

## <a name="next-steps"></a>Следующие шаги

- См. [общие сведения о программном использовании API-интерфейсов выставления счетов Azure](billing-usage-rate-card-overview.md) для анализа информации о потреблении ресурсов Azure.



