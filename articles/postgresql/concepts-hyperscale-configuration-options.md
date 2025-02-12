---
title: База данных Azure для PostgreSQL — Гипермасштабируемость (Citus) (Предварительная версия) параметры производительности
description: Параметры для группы серверов Гипермасштабируемого (Citus), включая узел вычислений, хранения и регионов.
author: jonels-msft
ms.author: jonels
ms.service: postgresql
ms.subservice: hyperscale-citus
ms.topic: conceptual
ms.date: 05/06/2019
ms.openlocfilehash: e8c1c2e51ca14ae9b17f0d7efb20552cdd55139b
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "65077295"
---
# <a name="azure-database-for-postgresql--hyperscale-citus-preview-performance-options"></a>База данных Azure для PostgreSQL — Гипермасштабируемость (Citus) (Предварительная версия) параметры производительности

## <a name="compute-and-storage"></a>Вычисления и хранение
 
В группе серверов Гипермасштабируемого (Citus) можно выбрать параметры вычислений и хранения независимо друг от друга для рабочих узлов и узел-координатор.  Вычислительные ресурсы поставляются в виде виртуальных ядер, которые представляют собой логические ЦП основного оборудования. Размер хранилища для подготовки ссылается на емкость, доступную для координатора и рабочих узлов в вашей группы серверов Гипермасштабируемого (Citus). Хранилище, включает файлы базы данных, временные файлы, журналы транзакций и журналы сервера Postgres. Общий объем хранилища, которое вы подготавливаете также определяет доступную производительность операций ввода-вывода для каждого узла рабочей роли и координатор.
 
|                       | Рабочий узел           | Узел-координатор      |
|-----------------------|-----------------------|-----------------------|
| Вычислительные ресурсы, виртуальных ядер       | 4, 8, 16, 32          | 4, 8, 16, 32          |
| Память на виртуальное ядро, Гиб | 8                     | 4\.                     |
| Размер хранилища, Тиб     | 0.5, 1, 2             | 0.5, 1, 2             |
| Тип хранилища          | Общего назначения (SSD) | Общего назначения (SSD) |
| IOPS                  | До трех операций ввода-ВЫВОДА/Гиб      | До трех операций ввода-ВЫВОДА/Гиб      |


## <a name="regions"></a>регионы
Группы серверов Гипермасштабируемого (Citus) доступны в следующих регионах Azure:
* Восток США 2
* Юго-Восточная Азия
* Западная Европа
* Западный регион США 2

## <a name="pricing"></a>Цены
Наиболее актуальные сведения о стоимости см. в статье [Цены на Базу данных Azure для PostgreSQL](https://azure.microsoft.com/pricing/details/postgresql/).
Для просмотра затрат для конфигурации следует [портала Azure](https://portal.azure.com/#create/Microsoft.PostgreSQLServer) показаны ежемесячные расходы на **Настройка** вкладке зависимости от выбранных параметров. Если у вас нет подписки Azure, для расчета цены можно воспользоваться калькулятором цен Azure. На [калькулятором цен Azure](https://azure.microsoft.com/pricing/calculator/) веб-сайта, выберите **добавить элементы**, разверните **баз данных** категории и выберите **база данных Azure для PostgreSQL — Гипермасштабируемый (Citus)** для настройки параметров.
 
## <a name="next-steps"></a>Дальнейшие действия
Узнайте, как [Создание группы серверов Гипермасштабируемого (Citus) на портале](quickstart-create-hyperscale-portal.md).
