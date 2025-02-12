---
title: Уровень служб общего назначения — база данных SQL Azure | Документация Майкрософт
description: Дополнительные сведения о универсальном уровне базы данных SQL Azure
services: sql-database
ms.service: sql-database
ms.subservice: service
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: sstein
manager: craigg
ms.date: 02/07/2019
ms.openlocfilehash: ef64fa1285efc2c271d8d7f37bdecabd67242cc0
ms.sourcegitcommit: de47a27defce58b10ef998e8991a2294175d2098
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/15/2019
ms.locfileid: "67874960"
---
# <a name="general-purpose-service-tier---azure-sql-database"></a>База данных SQL Azure на уровне служб общего назначения

> [!NOTE]
> Уровень служб общего назначения в модели приобретения на основе виртуальное ядро называется стандартным уровнем служб в модели приобретения на основе DTU. Сравнение модели приобретения на основе виртуальных ядер и DTU см. в статье [Ресурсы и модели приобретения для Базы данных SQL Azure](sql-database-purchase-models.md).

База данных SQL Azure основана на архитектуре ядра СУБД SQL Server, которая соответствует облачной среде, чтобы даже в случае сбоя инфраструктуры обеспечить доступность на уровне 99,99 %. Существует три уровня служб, которые используются в Базе данных SQL Azure, каждый из которых имеет разные архитектурные модели. Ниже указаны эти уровни служб.

- Общего назначения
- Критически важный для бизнеса
- Уровень "Гипермасштабирование"

Модель архитектуры для уровня служб общего назначения основана на разделении вычислений и хранилища. Эта модель архитектуры зависит от высокого уровня доступности и надежности хранилища BLOB-объектов Azure, которое прозрачно реплицирует файлы базы данных и предотвращает потерю данных, если происходит сбой базовой инфраструктуры.

На следующем рисунке показано четыре узла в стандартной архитектурной модели с разделенными уровнями вычислений и хранения.

![Разделение уровней вычисления и хранения](media/sql-database-managed-instance/general-purpose-service-tier.png)

В архитектурной модели для уровня служб общего назначения существует два уровня:

- Уровень вычислений без отслеживания состояния, на котором выполняется процесс `sqlserver.exe` и содержатся только временные и кэшированные данные (например, кэш планов, буферный пул, пул хранения столбцов). Этим узлом SQL Server без отслеживания состояния управляет Azure Service Fabric, которая инициализирует процесс, контролирует работоспособность узла и при необходимости выполняет переход на другой ресурс.
- Уровень данных с отслеживанием состояния, где файлы базы данных (MDF/LDF) хранятся в хранилище BLOB-объектов Azure. Хранилище BLOB-объектов Azure гарантирует, что ни одна из записей, размещенных в любом файле базы данных, не будет потеряна. Благодаря встроенной доступности и избыточности этого хранилища каждая запись в файле журнала или страница в файле данных будет сохранена даже в случае отказа процесса SQL Server.

Каждый раз, когда ядро СУБД или операционная система обновляется, некоторые компоненты базовой инфраструктуры выдают ошибку, или если в процессе SQL Server обнаруживается критическая проблема, Azure Service Fabric перемещает процесс SQL Server без отслеживания состояния на другой узел вычислений без отслеживания состояния. Существует набор резервных узлов, на которых можно запустить новую службу вычислений в случае отработки отказа, чтобы свести к минимуму время отработки отказа. Это не влияет на данные на уровне хранилища Azure, а файлы данных и журнала присоединяются к только что инициализированному процессу SQL Server. Этот процесс гарантирует доступность на уровне 99,99 %, но он может иметь некоторое воздействие на производительность интенсивной выполняемой рабочей нагрузки из-за времени перехода и того, что новый узел SQL Server запускается с холодным кэшем.

## <a name="when-to-choose-this-service-tier"></a>Когда нужно выбирать этот уровень служб?

Уровень служб общего назначения является уровнем служб по умолчанию в Базе данных SQL Azure и предназначен для большей части универсальных рабочих нагрузок. Если вам требуется полностью управляемое ядро СУБД с соглашением об уровне обслуживания 99,99% с задержкой хранилища от 5 до 10 мс, которые соответствуют SQL Azure IaaS в большинстве случаев, общего назначения уровень является вариантом.

## <a name="next-steps"></a>Следующие шаги

- Найдите характеристики ресурсов (количество ядер, операций ввода-вывода, памяти) общего назначения или уровня Standard в [управляемый экземпляр](sql-database-managed-instance-resource-limits.md#service-tier-characteristics), отдельной базе данных в [модели Виртуальное ядро](sql-database-vcore-resource-limits-single-databases.md#general-purpose-service-tier-storage-sizes-and-compute-sizes) или [модели DTU](sql-database-dtu-resource-limits-single-databases.md#single-database-storage-sizes-and-compute-sizes)или эластичном пуле в модели [Виртуальное ядро](sql-database-vcore-resource-limits-elastic-pools.md#general-purpose-service-tier-storage-sizes-and-compute-sizes) и [DTU](sql-database-dtu-resource-limits-elastic-pools.md#standard-elastic-pool-limits).
- Дополнительные сведения об уровнях [Критически важный для бизнеса](sql-database-service-tier-business-critical.md) и [Гипермасштабирование](sql-database-service-tier-hyperscale.md).
- Дополнительные сведения о [Service Fabric](../service-fabric/service-fabric-overview.md).
- Дополнительные сведения об обеспечении высокой доступности и аварийного восстановления см. в руководстве по [обеспечении непрерывности бизнес-процессов](sql-database-business-continuity.md).
