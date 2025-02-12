---
title: Анализ производительности запросов в базе данных Azure для MySQL
description: В этой статье описывается функция анализ производительности запросов в базе данных Azure для MySQL
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: conceptual
ms.date: 06/27/2019
ms.openlocfilehash: 8f142933ebf955cbe3aa119f42779109fb6ef7db
ms.sourcegitcommit: ccb9a7b7da48473362266f20950af190ae88c09b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/05/2019
ms.locfileid: "67589069"
---
# <a name="query-performance-insight-in-azure-database-for-mysql"></a>Анализ производительности запросов в базе данных Azure для MySQL

**Применяется к:**  база данных Azure для MySQL 5.7

> [!NOTE]
> Анализ производительности запросов доступен в предварительной версии.

Этот компонент помогает быстро определить наиболее медленно выполняющиеся запросы, их изменение со временем и ожидания каких действий влияют на них.

## <a name="common-scenarios"></a>Распространенные сценарии

### <a name="long-running-queries"></a>Долго выполняющихся запросов.

- определение самых медленно выполняющихся запросов за прошедшие X часов;
- определение основных N запросов, ожидающих ресурсы;
 
### <a name="wait-statistics"></a>Статистика ожидания

- Понимание характера ожидания для запроса
- Основные сведения о трендах для ожидания ресурсов и где существует конфликта ресурсов

## <a name="permissions"></a>Разрешения

**Владелец** или **участник** разрешения, необходимые для просмотра текста запросов в анализ производительности запросов. ** Модуль чтения** могут просматривать диаграммы и таблицы, но не текст запроса.

## <a name="prerequisites"></a>предварительные требования

Анализ производительности запросов к функции, данных должна присутствовать в [Query Store](concepts-query-store.md).

## <a name="viewing-performance-insights"></a>Просмотр анализа производительности

Представление [анализа производительности запросов](concepts-query-performance-insight.md) на портале Azure содержит визуализации ключевых данных из хранилища запросов.

На странице портала базы данных Azure для сервера MySQL, выберите **анализ производительности запросов** под **Intelligent производительности** части строки меню.

### <a name="long-running-queries"></a>Долго выполняющихся запросов.

 **Длительные запросы** вкладке отображается 5 наиболее частых запросов по средней длительности на выполнение, объединяются в 15-минутных интервалов. Дополнительные запросы можно просмотреть, выбрав из **номер из запросов** раскрывающийся список. При этом цвета, соответствующие тому или иному идентификатору запроса на графике, могут изменяться.

Чтобы уменьшить временной интервал графика, перетащите его границу. Кроме того можно используйте zoom in и out значки для просмотра соответственно уменьшить или увеличить период.

![Анализ производительности медленно выполняющихся запросов](./media/concepts-query-performance-insight/query-performance-insight-landing-page.png) 

### <a name="wait-statistics"></a>Статистика ожидания

> [!NOTE]
> Статистика ожидания предназначены для устранения проблем с производительностью запросов. Рекомендуется включить только для устранения неполадок.

Статистика ожидания список ожидания события, происходящие во время выполнения конкретного запроса. Дополнительные сведения о типах ожидания событий в [документация по ядру СУБД MySQL](https://go.microsoft.com/fwlink/?linkid=2098206).

Выберите **статистики ожидания** tab, чтобы просмотреть соответствующие визуализации на ожиданий на сервере.

Запросы, в представлении статистики ожидания группируются в запросах, которые характерны для наибольшего ожиданий во время указанного интервала времени.

![Анализ производительности запросов ожидает статистики](./media/concepts-query-performance-insight/query-performance-insight-wait-statistics.png)

## <a name="next-steps"></a>Следующие шаги

- Дополнительные сведения о [наблюдение и настройка](concepts-monitoring.md) в базе данных Azure для MySQL.