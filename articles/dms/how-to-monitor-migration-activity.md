---
title: Использование Azure Database Migration Service для мониторинга действий по миграции | Документация Майкрософт
description: Сведения об использовании Azure Database Migration Service для мониторинга действий по миграции.
services: database-migration
author: HJToland3
ms.author: jtoland
manager: craigg
ms.reviewer: craigg
ms.service: dms
ms.workload: data-services
ms.custom: mvc
ms.topic: article
ms.date: 03/12/2019
ms.openlocfilehash: 325bbee3f3d5ad5097f710cb56fe03baff97388a
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "60532804"
---
# <a name="monitor-migration-activity"></a>Мониторинг действия миграции
В этой статье вы узнаете, как отслеживать ход миграции на уровне базы данных и таблицы.

## <a name="monitor-at-the-database-level"></a>Мониторинг на уровне базы данных
Чтобы отслеживать действие на уровне базы данных, просмотрите колонку уровня базы данных:

![Колонка уровня базы данных](media/how-to-monitor-migration-activity/dms-database-level-blade.png)

> [!NOTE]
> При выборе гиперссылки базы данных отобразится список таблиц и текущее состояние миграции.

В следующей таблице перечислены поля в колонке уровня базы данных, а также различные значения состояния, связанные с каждым полем.

<table id='overview' class='overview'>
  <thead>
    <tr>
      <th class="x-hidden-focus"><strong>Имя поля</strong></th>
      <th><strong>Подсостояние поля</strong></th>
      <th><strong>Описание</strong></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td rowspan="3" class="ActivityStatus"><strong>Состояние действия</strong></td>
      <td>Выполнение</td>
      <td>Действие миграции выполняется.</td>
    </tr>
    <tr>
      <td>Succeeded</td>
      <td>Действие миграции выполнено успешно.</td>
    </tr>
    <tr>
      <td>Сбой</td>
      <td>Сбой миграции. Щелкните ссылку для просмотра подробных сведений об ошибке.</td>
    </tr>
    <tr>
      <td rowspan="4" class="Status"><strong>Состояние</strong></td>
      <td>Инициализация</td>
      <td>DMS настраивает конвейер миграции.</td>
    </tr>
    <tr>
      <td>Выполнение</td>
      <td>Конвейер DMS работает и выполняет миграцию.</td>
    </tr>
    <tr>
      <td>Завершено</td>
      <td>Миграция завершена.</td>
    </tr>
    <tr>
      <td>Сбой</td>
      <td>Сбой миграции. Щелкните сведения о миграции, чтобы просмотреть ошибки миграции.</td>
    </tr>
    <tr>
      <td rowspan="5" class="migration-details"><strong>Сведения о миграции</strong></td>
      <td>Initiating the migration pipeline (Инициализация конвейера миграции)</td>
      <td>DMS настраивает конвейер миграции.</td>
    </tr>
    <tr>
      <td>Full data load in progress (Идет полная загрузка данных)</td>
      <td>DMS выполняет начальную загрузку.</td>
    </tr>
    <tr>
      <td>Ready for Cutover (Готово к прямой миграции)</td>
      <td>После того как начальная загрузка будет завершена, DMS пометит базу данных как готовую к прямой миграции. Пользователь должен проверить, предусмотрена ли непрерывная синхронизация данных.</td>
    </tr>
    <tr>
      <td>Все изменения применены</td>
      <td>Начальная загрузка и непрерывная синхронизация успешно выполнены. Это состояние также появляется после успешного завершения прямой миграции базы данных.</td>
    </tr>
    <tr>
      <td>См. сведения об ошибке</td>
      <td>Щелкните эту ссылку, чтобы просмотреть сведения об ошибке.</td>
    </tr>
    <tr>
      <td rowspan="1" class="duration"><strong>Duration</strong></td>
      <td>Н/Д</td>
      <td>Общее время от инициализации действия миграции до завершения или ошибки миграции.</td>
    </tr>
     </tbody>
</table>

## <a name="monitor-at-table-level--quick-summary"></a>Мониторинг на уровне таблицы — краткая сводка
Чтобы отслеживать действие на уровне таблицы, просмотрите колонку уровня таблицы. В верхней части колонки отображается подробное количество строк, перенесенных при полной загрузке и добавочных обновлениях. 

В нижней части колонки содержится список таблиц и отображается краткая сводка о ходе выполнения миграции.

![Колонка уровня таблицы — краткая сводка](media/how-to-monitor-migration-activity/dms-table-level-blade-summary.png)

В следующей таблице описаны поля, отображаемые в сведениях на уровне таблицы.

| Имя поля        | Описание       |
| ------------- | ------------- |
| **Полная загрузка завершена**      | Количество таблиц с завершенной полной загрузкой данных. |
| **Полная загрузка поставлена в очередь**      | Количество таблиц, поставленных в очередь для полной загрузки.      |
| **Идет полная загрузка** | Количество таблиц с ошибками.      |
| **Добавочные операции обновления**      | Количество обновлений отслеживания измененных данных (CDC) в строках, примененных к целевому объекту. |
| **Добавочные операции вставки**      | Количество операций вставки CDC в строках, примененных к целевому объекту.      |
| **Добавочные операции удаления** | Количество операций удаления CDC в строках, примененных к целевому объекту.      |
| **Ожидающие изменения**      | Количество CDC в строках, по-прежнему ожидающих применения к целевому объекту. |
| **Примененные изменения**      | Общее количество операций обновления, вставки и удаления CDC в строках, примененных к целевому объекту.      |
| **Таблицы в состоянии ошибки** | Количество таблиц, которые находятся в состоянии ошибки во время миграции. Таблицы могут переходить в состояние ошибки, если в целевом объекте определены повторяющиеся значения или данные несовместимы с загрузкой в целевой таблице.      |

## <a name="monitor-at-table-level--detailed-summary"></a>Мониторинг на уровне таблицы — подробная сводка
Есть две вкладки, показывающие ход миграции в режиме полной загрузки и добавочной синхронизации данных.
    
![Вкладка полной загрузки](media/how-to-monitor-migration-activity/dms-full-load-tab.png)

![Вкладка добавочной синхронизации данных](media/how-to-monitor-migration-activity/dms-incremental-data-sync-tab.png)

В следующей таблице описаны поля, показанные в ходе миграции на уровне таблицы.

| Имя поля        | Описание       |
| ------------- | ------------- |
| **Состояние — синхронизация**      | Выполняется непрерывная синхронизация. |
| **Вставка**      | Количество операций вставки CDC в строках, примененных к целевому объекту.      |
| **Update** | Количество операций обновления CDC в строках, примененных к целевому объекту.      |
| **Удаление**      | Количество операций удаления CDC в строках, примененных к целевому объекту. |
| **Всего применено**      | Общее количество операций обновления, вставки и удаления CDC в строках, примененных к целевому объекту. |
| **Ошибки данных** | Количество ошибок данных, случившихся в этой таблице. Примерами ошибок могут быть *511: Cannot create a row of size %d which is greater than the allowable maximum row size of %d, 8114: Error converting data type %ls to %ls.* (511: не удалось создать строку размером %d, который превышает допустимый размер строки — %d, 8114: ошибка преобразования типа данных %ls в %ls).  Клиенту нужно выполнить запрос из таблицы dms_apply_exceptions в целевом объекте Azure, чтобы просмотреть сведения об ошибке.    |

> [!NOTE]
> Значения CDC полей операций вставки, обновления, удаления и поля "Всего применено" могут уменьшаться при прямой миграции базы данных или перезапуске миграции.

## <a name="next-steps"></a>Дальнейшие действия
- Просмотрите [руководство по миграции базы данных Майкрософт](https://datamigration.microsoft.com/).