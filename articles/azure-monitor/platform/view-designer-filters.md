---
title: Фильтры в представлениях Azure Monitor | Документация Майкрософт
description: Фильтр в представлении Azure Monitor позволяет пользователям фильтровать данные в представлении по значению конкретного свойства, не изменяя само представление.  В этой статье описывается, как использовать фильтр и добавить его в настраиваемое представление.
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: ''
ms.assetid: ce41dc30-e568-43c1-97fa-81e5997c946a
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 06/22/2018
ms.author: bwren
ms.openlocfilehash: 31a902302ba806889854330c6517d9f5745f1c0c
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "60551744"
---
# <a name="filters-in-azure-monitor-views"></a>Фильтры в представлениях Azure Monitor
Объект **фильтра** в [представление мониторинга Azure](view-designer.md) позволяет пользователям фильтровать данные в представлении по значению конкретного свойства, не изменяя само представление.  Например, можно разрешить пользователям представления фильтровать это представление, чтобы просматривать данные только с определенного компьютера или набора компьютеров.  Можно создать несколько фильтров в одном представлении, чтобы разрешить пользователям выполнять фильтрацию по нескольким свойствам.  В этой статье описывается, как использовать фильтр и добавить его в настраиваемое представление.

## <a name="using-a-filter"></a>Использование фильтра
Щелкните диапазон времени, даты в верхней части представления, чтобы открыть раскрывающийся список, где можно изменить диапазон времени для представления даты.

![Пример фильтра](media/view-designer-filters/filters-example-time.png)

Щелкните **+** , чтобы добавить фильтр с помощью настраиваемых фильтров, определенных для представления. Выберите значение для фильтра в раскрывающемся списке или введите его самостоятельно. Продолжайте добавлять фильтры, щелкая **+** . 


![Пример фильтра](media/view-designer-filters/filters-example-custom.png)

Если удалить все значения для фильтра, то он больше не будет применяться.


## <a name="creating-a-filter"></a>Создание фильтра

Создайте фильтр на вкладке **Фильтры** при [редактировании представления](view-designer.md).  Фильтр является глобальным для представления и применяется ко всем его частям.  

![Параметры фильтра](media/view-designer-filters/filters-settings.png)

В следующей таблице описаны параметры фильтра.

| Параметр | Описание |
|:---|:---|
| Имя поля | Имя поля, используемого для фильтрации.  Это поле должно соответствовать полю summarize в **запрос значений**. |
| Запрос значений | Запрос, выполняемый для заполнения раскрывающегося списка фильтров для пользователя.  Этот запрос должен использовать либо [суммировать](/azure/kusto/query/summarizeoperator) или [distinct](/azure/kusto/query/distinctoperator) для предоставления уникальных значений для определенного поля и он должен соответствовать **имя поля**.  Можно использовать оператор [sort](/azure/kusto/query/sortoperator) для сортировки значений, отображаемых для пользователя. |
| Тег | Имя поля, которое используется в запросах с поддержкой фильтра, а также отображается для пользователя. |

### <a name="examples"></a>Примеры

В следующей таблице представлены примеры распространенных фильтров.  

| Имя поля | Запрос значений | Тег |
|:--|:--|:--|
| Computer   | Heartbeat &#124; distinct Computer &#124; sort by Computer asc | Компьютеры |
| EventLevelName | Event &#124; distinct EventLevelName | Severity |
| SeverityLevel | Syslog &#124; distinct SeverityLevel | Severity |
| SvcChangeType | ConfigurationChange &#124; distinct svcChangeType | ChangeType |


## <a name="modify-view-queries"></a>Изменение запросов в представлении

Для вступления фильтра в действие необходимо изменить все запросы в представлении, чтобы применить фильтр по выбранным значениям.  Если не изменить все запросы в представлении, а затем все значения, выбранного пользователем не окажет никакого воздействия.

Для использования значения фильтра в запросе используется приведенный ниже синтаксис. 

    where ${filter name}  

Например, если в представлении запроса, который возвращает события и используется фильтр с именем _компьютеров_, можно использовать следующий запрос.

    Event | where ${Computers} | summarize count() by EventLevelName

Если вы добавили еще один фильтр, Severity, то можете использовать следующий запрос для применения обоих фильтров.

    Event | where ${Computers} | where ${Severity} | summarize count() by EventLevelName

## <a name="next-steps"></a>Дальнейшие действия
* Узнайте больше об [элементах визуализации](view-designer-parts.md), которые можно добавить в представление.
