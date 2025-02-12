---
title: 'Непрерывность бизнес-процессов и аварийное восстановление (BCDR): пары регионов Azure | Документация Майкрософт'
description: Дополнительные сведения о подключении регионов Azure для обеспечения отказоустойчивости приложений во время сбоев в центрах данных.
author: rayne-wiselman
manager: carmon
ms.service: multiple
ms.topic: article
ms.date: 07/01/2019
ms.author: raynew
ms.openlocfilehash: 81ba993e6cbe55b45d34325545754bec561ce479
ms.sourcegitcommit: 6cb4dd784dd5a6c72edaff56cf6bcdcd8c579ee7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/02/2019
ms.locfileid: "67514460"
---
# <a name="business-continuity-and-disaster-recovery-bcdr-azure-paired-regions"></a>Непрерывность бизнес-процессов и аварийное восстановление (BCDR): пары регионов Azure

## <a name="what-are-paired-regions"></a>Что такое пары регионов?

Azure работает в различных странах по всему миру. Географическая территория Azure — это определенная область мира, содержащая по крайней мере один регион Azure. Регион Azure — это область в пределах географической территории, содержащая один или несколько центров обработки данных.

Каждый регион Azure образует пару с другим регионом в пределах той же географической территории. Эти два региона формируют пару регионов. Исключением является южная Бразилия, которая образует пару с регионом за пределами своей территории. Azure сериализует обновления платформ во всех регионах (плановое обслуживание), чтобы одновременно обновлялся только один парный регион. Если сбой затрагивает несколько регионов, по меньшей мере один регион в каждой паре будет приоритетным для восстановления.

![AzureGeography](./media/best-practices-availability-paired-regions/GeoRegionDataCenter.png)

Рисунок 1. Пары регионов Azure

| "География" | Пары регионов |  |
|:--- |:--- |:--- |
| Азия |Восточная Азия |Юго-Восточная Азия |
| Австралия |Восток Австралии |Юго-Восточная часть Австралии |
| Австралия |Центральная Австралия |Центральная Австралия 2 |
| Бразилия |Южная часть Бразилии |Центрально-южная часть США |
| Канада |Центральная Канада |Восточная Канада |
| Китай |Север Китая |Восток Китая|
| Китай |Северный Китай 2 |Восточный Китай 2|
| Европа |Северная Европа (Ирландия) |Западная Европа (Нидерланды) |
| Франция |Центральная Франция|Южная Франция|
| Германия |Центральная Германия |Северо-восточная Германия |
| Индия |Центральная Индия |Южная Индия |
| Индия |Западная Индия |Южная Индия |
| Япония |Япония, восток |Западная часть Японии |
| Корея |Центральная Корея |Корея, юг |
| Северная Америка |Восточная часть США |Запад США |
| Северная Америка |Восток США 2 |Центральный регион США |
| Северная Америка |Центрально-северная часть США |Центрально-южная часть США |
| Северная Америка |Западный регион США 2 |Западно-центральная часть США 
| ЮАР | Север ЮАР | Запад ЮАР
| Соединенное Королевство |Западная часть Великобритании |Южная часть Великобритании |
| Объединенные Арабские Эмираты | Северная часть ОАЭ | Центральная часть ОАЭ
| Министерства обороны США |Восток США, МО |Центральная часть США, МО |
| Правительство США |Аризона (для обслуживания государственных организаций США) |Техас (для обслуживания государственных организаций США) |
| Правительство США |US Gov (Айова) |Правительство штата Вирджиния |
| Правительство США |Правительство штата Вирджиния |Техас (для обслуживания государственных организаций США) |

Таблица 1. Сопоставление пар регионов Azure

- Западная Индия сопряжен только в одном направлении. Дополнительный регион для региона "Западная Индия" — "Южная Индия", а дополнительный регион для региона "Южная Индия — "Центральная Индия".
- Южная Бразилия является уникальным регионом, так как она образует пару за пределами собственной географической территории. Дополнительный регион для Южной Бразилии является Юго-Центральная часть США. Дополнительный регион Южная Центральная часть США не является Южная Бразилия.
- Дополнительный регион Айова, США — US Gov Virginia.
- Правительство штата Вирджиния дополнительный регион — Техас правительственные учреждения США.
- Дополнительный регион Техас для обслуживания государственных организаций США — Аризона правительственные учреждения США.


Рекомендуется настроить аварийное восстановление непрерывности бизнеса (BCDR) по парным регионам, чтобы пользоваться преимуществами политик изоляции и доступности Azure. Для приложений, которые поддерживают несколько активных областей, рекомендуется использовать оба региона в паре региона, если это возможно. Это обеспечит оптимальную доступность приложений и минимизирует время восстановления в случае аварии. 

## <a name="an-example-of-paired-regions"></a>Пример пар регионов
На рис. 2 ниже показано гипотетическое приложение, использующее пары регионов для аварийного восстановления. Зелеными числами выделены межрегиональные действия трех служб Azure (служба вычислений, служба хранилища и база данных Azure) и их настройка для репликации по регионам. Уникальные преимущества развертывания в парных регионах выделены оранжевыми числами.

![Обзор преимуществ парных регионов](./media/best-practices-availability-paired-regions/PairedRegionsOverview2.png)

Рис. 2. Гипотетическая пара регионов Azure

## <a name="cross-region-activities"></a>Межрегиональные действия
Как показано на рисунке 2.

![IaaS](./media/best-practices-availability-paired-regions/1Green.png) **Служба вычислений Azure (IaaS)** . Вы должны подготовить дополнительные вычислительные ресурсы заранее, чтобы обеспечить доступность ресурсов в другом регионе во время аварии. Дополнительные сведения см. в [техническом руководстве по обеспечению устойчивости в Azure](resiliency/resiliency-technical-guidance.md).

![Хранилище](./media/best-practices-availability-paired-regions/2Green.png) **хранилища Azure** — Если вы используете управляемые диски, Дополнительные сведения о [резервного копирования между регионами](https://docs.microsoft.com/azure/architecture/resiliency/recovery-loss-azure-region#virtual-machines) со службой Azure Backup и [репликации виртуальных машин](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-tutorial-enable-replication) из одного региона в другой с помощью Azure Site Recovery. Если вы используете учетные записи хранения, геоизбыточное хранилище (GRS) настраивается по умолчанию, при создании учетной записи хранения Azure. При использовании GRS данные реплицируются трижды в основном регионе и трижды в дополнительном. Дополнительные сведения см. в статье [Репликация службы хранилища Azure](storage/common/storage-redundancy.md).

![Azure SQL](./media/best-practices-availability-paired-regions/3Green.png) **База данных SQL Azure**. С помощью георепликации базы данных SQL Azure можно настроить асинхронную репликацию транзакций в любой регион мира. Тем не менее, для наиболее эффективного аварийного восстановления мы рекомендуем развертывать эти ресурсы в связанном регионе. Дополнительные сведения см. в статье [Обзор: активная георепликация для базы данных SQL Azure](sql-database/sql-database-geo-replication-overview.md).

![Resource Manager](./media/best-practices-availability-paired-regions/4Green.png) **Azure Resource Manager**. По своему существу Resource Manager обеспечивает логическую изоляцию компонентов в регионах. Это означает, что логические сбои в одном регионе едва ли затронут другой регион.

## <a name="benefits-of-paired-regions"></a>Преимущества пар регионов
Как показано на рисунке 2.  

![Изоляция](./media/best-practices-availability-paired-regions/5Orange.png)
 **Физическая изоляция**. По возможности центры обработки данных Azure в паре регионов располагаются как минимум на расстоянии 483 км друг от друга, хотя это не всегда практично и не всегда можно реализовать во всех географических областях. Физическое разделение центров обработки данных уменьшает вероятность последствий стихийных бедствий, массовых беспорядков, отключения электроэнергии или физического выхода сети из строя в обоих регионах одновременно. Уровень изоляции зависит от местных условий (размер географической территории, доступность инфраструктуры электропитания/сети, законодательство и т. д.).  

![Репликация](./media/best-practices-availability-paired-regions/6Orange.png)
 **Платформенная репликация**. Некоторые службы, например геоизбыточное хранилище, автоматически реплицируют данные в дополнительный регион.

![Восстановление](./media/best-practices-availability-paired-regions/7Orange.png)
 **Приоритет регионов для восстановления**. На случай масштабного сбоя в каждой паре определяется приоритетный регион для восстановления. Для приложений, развернутых в парных регионах, один из регионов гарантированно восстанавливается в первую очередь. Если приложение развернуто в регионах, которые не являются парными, восстановление может происходить с задержкой. В худшем случае оба выбранных региона будут восстановлены в последнюю очередь.

![Обновления](./media/best-practices-availability-paired-regions/8Orange.png)
 **Последовательные обновления**. Запланированные обновления системы Azure распространяются среди парных регионов последовательно (не одновременно). Это сводит к минимуму время простоя, последствия ошибок и логических сбоев в редких случаях неправильного обновления.

![Данные](./media/best-practices-availability-paired-regions/9Orange.png)
 **Размещение данных**. Регион находится в той же географической области, что и его пара (за исключением Южной Бразилии). Таким образом соблюдаются требования к размещению данных, связанные с налогообложением и юрисдикцией применения законодательства.
