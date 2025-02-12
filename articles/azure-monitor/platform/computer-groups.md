---
title: Группы компьютеров в запросах к журналам Azure Monitor | Документация Майкрософт
description: В Azure Monitor вы можете ограничить запросы к журналам определенной группой компьютеров.  В этой статье описывается, как можно создавать группы компьютеров и использовать их в запросах к журналам.
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: ''
ms.assetid: a28b9e8a-6761-4ead-aa61-c8451ca90125
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 02/05/2019
ms.author: bwren
ms.openlocfilehash: ae423b6fb141cab4038e65ba85c6067f1c23aee0
ms.sourcegitcommit: 4b431e86e47b6feb8ac6b61487f910c17a55d121
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/18/2019
ms.locfileid: "68320678"
---
# <a name="computer-groups-in-azure-monitor-log-queries"></a>Группы компьютеров в Azure Monitor запросы журналов
В Azure Monitor вы можете ограничить [запросы к журналам](../log-query/log-query-overview.md) определенной группой компьютеров.  Каждая группа заполняется компьютерами с помощью определяемого запроса или при импорте групп из разных источников.  Если в запрос к журналам включена группа, в результатах возвращаются только те записи, которые относятся к компьютерам в этой группе.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../../includes/azure-monitor-log-analytics-rebrand.md)]

## <a name="creating-a-computer-group"></a>Создание группы компьютеров
В Azure Monitor группу компьютеров можно создать с помощью любого из способов, перечисленных в следующей таблице.  Подробные сведения о способе представлены в разделах ниже. 

| Метод | Описание |
|:--- |:--- |
| Запрос журнала |Создание запроса к журналам, который возвращает список компьютеров. |
| API поиска по журналам |Использование API поиска по журналам для создания группы компьютеров программными средствами на основе результатов запроса к журналам. |
| Active Directory |Автоматическая проверка членства в группе на компьютерах агента, которые являются членами домена Active Directory, и создание группы в Azure Monitor для каждой группы безопасности. (Только для компьютеров Windows.)|
| Диспетчер конфигураций | Импорт коллекций из System Center Configuration Manager и создание группы в службе Azure Monitor для каждой из коллекций. |
| Службы обновления Windows Server |Автоматическая проверка целевых групп на клиентах или серверах WSUS и создание для каждой из них группы в Azure Monitor. |

### <a name="log-query"></a>Запрос журнала
Группы компьютеров, созданные на основе результата запроса к журналам, содержат все компьютеры, возвращенные указанным поисковым запросом.  Так как этот запрос выполняется при каждом использовании группы компьютеров, отражаются все изменения, внесенные с момента создания группы.  

Для группы можно применить любой запрос, но он должен возвращать набор компьютеров без повторов (используйте для этого `distinct Computer`).  Ниже приведен типичный пример запроса, который можно использовать как группу компьютеров.

    Heartbeat | where Computer contains "srv" | distinct Computer

Ниже приведены инструкции по созданию группы компьютеров с помощью поиска по журналам на портале Azure.

1. Щелкните **Журналы** в меню **Azure Monitor** на портале Azure.
1. Создайте и выполните запрос, который возвращает нужную группу компьютеров.
1. Щелкните **Сохранить** в верхней части экрана.
1. Для **Сохранить как** измените значение на **Функция** и щелкните **Сохранить этот запрос как группу компьютеров**.
1. Укажите значения для каждого свойства группы компьютеров в этой таблице и щелкните **Сохранить**.

В приведенной ниже таблице описаны свойства, которые есть у группы компьютеров.

| Свойство | Description |
|:---|:---|
| ИМЯ   | Имя группы, которое отображается на портале. |
| Псевдоним функции | Уникальный псевдоним для идентификации группы компьютеров в запросе. |
| Категория       | Категория для упорядочения поисковых запросов на портале. |


### <a name="active-directory"></a>Active Directory
При настройке Azure Monitor для импорта членства в группах Active Directory выполняется анализ членства в группах всех присоединенных к домену компьютеров с Windows с агентом Log Analytics.  Группа компьютеров создается в Azure Monitor для каждой группы безопасности в Active Directory. При этом каждый компьютер с Windows добавляется в группы компьютеров, соответствующие группам безопасности, членами которых они являются.  Это членство обновляется каждые 4 часа.  

> [!NOTE]
> Импортированные группы Active Directory могут содержать только компьютеры с Windows.

Вы можете настроить в Azure Monitor импорт групп безопасности из Active Directory, используя раздел **Дополнительные параметры** в рабочей области Log Analytics на портале Azure.  Последовательно выберите **Группы компьютеров**, **Active Directory** и **Импорт членств в группах Active Directory с компьютеров**.  Дополнительная настройка не требуется.

![Группы компьютеров в Active Directory](media/computer-groups/configure-activedirectory.png)

При импорте групп в меню отображаются число компьютеров с обнаруженным членством в группах и число импортированных групп.  Можно щелкнуть любую из этих ссылок для возврата записей **ComputerGroup** с этой информацией.

### <a name="windows-server-update-service"></a>Служба обновления Windows Server
При настройке Azure Monitor для импорта членства в группах WSUS выполняется анализ членства в целевых группах всех компьютеров с агентом Log Analytics.  Если выбор осуществляется на стороне клиента, данные о членстве в группе для всех компьютеров, подключенных к Azure Monitor и входящих в любую целевую группу WSUS, импортируются в Azure Monitor. Если выбор осуществляется на стороне сервера, для импорта в Azure Monitor данных о членстве в группе на сервере WSUS должен быть установлен агент Log Analytics.  Это членство обновляется каждые 4 часа. 

Вы можете настроить в Azure Monitor импорт групп безопасности из WSUS, используя раздел **Дополнительные параметры** в рабочей области Log Analytics на портале Azure.  Последовательно выберите **Группы компьютеров**, **WSUS** и **Импорт членств в группах WSUS**.  Дополнительная настройка не требуется.

![Группы компьютеров из WSUS](media/computer-groups/configure-wsus.png)

При импорте групп в меню отображаются число компьютеров с обнаруженным членством в группах и число импортированных групп.  Можно щелкнуть любую из этих ссылок для возврата записей **ComputerGroup** с этой информацией.

### <a name="system-center-configuration-manager"></a>System Center Configuration Manager
Если вы настроите в Azure Monitor импорт данных о членстве в коллекциях Configuration Manager, служба создаст отдельную группу компьютеров для каждой коллекции.  Данные о членстве в коллекциях извлекаются каждые три часа, чтобы поддерживать актуальность групп компьютеров. 

Перед импортом коллекций Configuration Manager следует [подключить Configuration Manager к Azure Monitor](collect-sccm.md).  

![Группы компьютеров из SCCM](media/computer-groups/configure-sccm.png)

При импорте коллекций в меню отображается число компьютеров с обнаруженным членством в группах и число импортированных групп.  Можно щелкнуть любую из этих ссылок для возврата записей **ComputerGroup** с этой информацией.

## <a name="managing-computer-groups"></a>Управление группами компьютеров
Группы компьютеров, созданные с помощью операции или API запроса к журналам, можно просматривать в меню **Дополнительные параметры** для рабочей области Log Analytics на портале Azure.  Выберите **Группы компьютеров**, а затем **Сохраненные группы**.  

Щелкните значок **x** в столбце **Удаление**, чтобы удалить группу компьютеров.  Щелкните значок **просмотра участников** группы, чтобы выполнить поиск по журналам группы, который возвращает ее участников.  Вы не можете изменить группу компьютеров. Вместо этого следует удалить группу и создать ее заново с новыми параметрами.

![Сохраненные группы компьютеров](media/computer-groups/configure-saved.png)


## <a name="using-a-computer-group-in-a-log-query"></a>Использование группы компьютеров в запросе к журналам
Группу компьютеров, созданную с помощью запроса к журналам, можно использовать в запросах, применяя ее псевдоним как функцию. Вот типичный синтаксис для этого:

  `Table | where Computer in (ComputerGroup)`

Например, следующий запрос возвращает записи UpdateSummary только для тех компьютеров, которые входят в группу компьютеров mycomputergroup.
 
  `UpdateSummary | where Computer in (mycomputergroup)`


Импортированные группы компьютеров и их включенные компьютеры хранятся в таблице **ComputerGroup**.  Например, следующий запрос вернет список компьютеров в группе компьютеров домена из Active Directory. 

  `ComputerGroup | where GroupSource == "ActiveDirectory" and Group == "Domain Computers" | distinct Computer`

Следующий запрос вернет записи UpdateSummary только для компьютеров из группы компьютеров домена.

  ```
  let ADComputers = ComputerGroup | where GroupSource == "ActiveDirectory" and Group == "Domain Computers" | distinct Computer;
  UpdateSummary | where Computer in (ADComputers)
  ```




## <a name="computer-group-records"></a>Записи группы компьютеров
Запись создается в рабочей области Log Analytics для каждого членства в группе компьютеров, создаваемого в Active Directory или WSUS.  Записи о производительности имеют тип **ComputerGroup**. Их свойства описаны в таблице ниже.  Для групп компьютеров, созданных с помощью запроса к журналам, записи не создаются.

| Свойство | Описание |
|:--- |:--- |
| `Type` |*ComputerGroup* |
| `SourceSystem` |*SourceSystem* |
| `Computer` |Имя компьютера-члена. |
| `Group` |Имя группы. |
| `GroupFullName` |Полный путь к группе, включая источник и имя источника. |
| `GroupSource` |Источник, из которого получена группа. <br><br>ActiveDirectory<br>WSUS<br>WSUSClientTargeting |
| `GroupSourceName` |Имя источника, из которого получена группа.  Для Active Directory это имя домена. |
| `ManagementGroupName` |Имя группы управления для агентов SCOM.  Для других агентов это AOI-\<идентификатор_рабочей_области\>. |
| `TimeGenerated` |Дата и время создания или изменения группы компьютеров. |

## <a name="next-steps"></a>Следующие шаги
* Узнайте больше о [запросах журнала](../log-query/log-query-overview.md), которые можно применять для анализа данных, собираемых из источников данных и решений.  

