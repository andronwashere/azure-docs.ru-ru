---
title: Функции службы Базы данных SQL Azure | Документы Майкрософт
description: Узнайте о технических характеристиках и возможностях базы данных SQL, а также о реляционной системе управления базами данных (реляционной СУБД) Майкрософт в облаке.
keywords: введение в sql, общие сведения об sql, что такое база данных sql
services: sql-database
ms.service: sql-database
ms.subservice: service
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: carlrab
manager: craigg
ms.date: 04/08/2019
ms.openlocfilehash: 8debd55cd1d5588a24dcd32a619ae772e499a939
ms.sourcegitcommit: fa45c2bcd1b32bc8dd54a5dc8bc206d2fe23d5fb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/12/2019
ms.locfileid: "67850080"
---
# <a name="what-is-azure-sql-database-service"></a>Что такое служба базы данных SQL Azure

База данных SQL Azure — это управляемая служба реляционной базы данных общего назначения, которая позволяет создавать высокодоступные и высокопроизводительные уровни хранилища данных для приложений и решений в Microsoft Azure Cloud. База данных SQL может быть верным выбором для множества современных облачных приложений, поскольку она позволяет использовать мощные функции для обработки реляционных данных и нереляционных [структур](sql-database-multi-model-features.md) , таких как графы, JSON, ПРОСТРАНСТВЕННЫЕ и XML. Он основан на новейшей стабильной версии [ядра субд Microsoft SQL Server](https://docs.microsoft.com/sql/sql-server/sql-server-technical-documentation?toc=/azure/sql-database/toc.json) и позволяет использовать обширный набор расширенных функций обработки запросов, таких как [высокопроизводительные технологии в памяти](sql-database-in-memory.md) и [интеллектуальная обработка запросов. ](https://docs.microsoft.com/sql/relational-databases/performance/intelligent-query-processing?toc=/azure/sql-database/toc.json).
С облачной стратегией Майкрософт новейшие возможности SQL Server сначала выпускаются для базы данных SQL, а затем для самой платформы SQL Server. Такой подход позволяет использовать новейшие возможности SQL Server без лишних расходов на внесение исправлений или обновлений. При этом эти функции проверены на миллионах баз данных. База данных SQL позволяет легко определять и масштабировать производительность в двух разных моделях приобретения: [модели приобретения на основе виртуальное ядро](sql-database-service-tiers-vcore.md) и [модели приобретения на основе DTU](sql-database-service-tiers-dtu.md). База данных SQL — это полностью управляемая служба, которая имеет встроенные ущерба, резервные копии и другие общие операции обслуживания. Корпорация Майкрософт полностью обрабатывает исправления и обновления кода SQL и ОС и абстрагирует все управление базовой инфраструктурой.

> [!NOTE]
> Глоссарий терминов для Базы данных SQL Azure см. [здесь](sql-database-glossary-terms.md).

База данных SQL Azure предоставляет следующие возможности развертывания для Базы данных SQL Azure:

![Варианты развертывания](./media/sql-database-technical-overview/deployment-options.png)

- [Единственная база данных](sql-database-single-database.md) представляет полностью управляемую изолированную базу данных, которая идеально подходит для современных облачных приложений и микрослужб, которым необходим единый надежный источник данных. Одна база данных похожа на [автономные](https://docs.microsoft.com/sql/relational-databases/databases/contained-databases?toc=/azure/sql-database/toc.json) базы данных в [Microsoft SQL Server компоненте Database Engine](https://docs.microsoft.com/sql/sql-server/sql-server-technical-documentation?toc=/azure/sql-database/toc.json).
- [Управляемый экземпляр](sql-database-managed-instance.md) — это полностью управляемый экземпляр компонента [ядра СУБД Microsoft SQL Server](https://docs.microsoft.com/sql/sql-server/sql-server-technical-documentation?toc=/azure/sql-database/toc.json) , содержащего набор баз данных, которые можно использовать совместно. Это идеальный вариант для простой миграции локальных SQL Server баз данных в облако Azure и для приложений, которым требуется использовать мощные функции базы данных, которые SQL Server ядро СУБД.
- [Эластичный пул](sql-database-elastic-pool.md) — это набор [отдельных баз данных](sql-database-single-database.md) с общим набором ресурсов, таких как ЦП или память. Отдельные базы данных можно перемещать в эластичный пул и обратно.

> [!IMPORTANT]
> Чтобы понять отличия между Базой данных SQL и SQL Server, а также различия между различными параметрами развертывания базы данных SQL Azure, см. [эту статью](sql-database-features.md).

База данных SQL предоставляет предсказуемую производительность с несколькими типами ресурсов на нескольких уровнях служб и с объемами вычислительных ресурсов, что обеспечивает динамическое масштабирование без простоя, встроенную интеллектуальную оптимизацию, глобальные масштабируемость и доступность, а также расширенные параметры безопасности. Все это без необходимости администрирования. Эти возможности позволяют вам не тратить драгоценное время и ресурсы на управление виртуальными машинами и инфраструктурой, а сосредоточиться на быстрой разработке приложений и ускорить выход на рынок. Служба базы данных SQL размещается в 38 центрах обработки данных по всему миру. К большинству баз данных можно подключиться в любое время, что позволяет запускать базу данных в близлежащем центре обработки данных.

## <a name="scalable-performance-and-pools"></a>Масштабируемая производительность и пулы

Все разновидности базы данных SQL позволяют определить объем ресурсов, которые будут назначены. 
- В отдельных базах данных каждая база данных изолирована от других и является переносимой. Каждая обеспечивает гарантированный объем ресурсов вычислений, памяти и хранилища. Объем ресурсов, назначенных базе данных, выделен для этой базы данных и не будет использоваться совместно с другими базами данных в облаке Azure. Кроме того, она позволяет динамически [масштабировать ресурсы отдельной базы данных](sql-database-single-database-scale.md) . Отдельная база данных предоставляет различные ресурсы для вычислений, памяти и хранения для различных нужд, которые отличаются от 1 до 80 виртуальных ядер, от 32 ГБ до 4 ТБ и т. д. [Уровень службы](sql-database-service-tier-hyperscale.md) "горизонтальное масштабирование" для отдельной базы данных позволяет масштабировать до 100 ТБ, используя возможности быстрого резервного копирования и восстановления.
- С помощью эластичных пулов можно назначать ресурсы, которые будут совместно использоваться всеми базами данных в пуле. Можно создать новую базу данных или переместить существующие отдельные базы данных в пул ресурсов, чтобы максимально увеличить использование ресурсов и сэкономить деньги, а также возможность динамического [масштабирования ресурсов эластичного пула](sql-database-elastic-pool-scale.md) .
- При использовании управляемых экземпляров каждый из экземпляров изолирован от других и имеет гарантированные ресурсы. В управляемом экземпляре базы данных экземпляра имеют общий набор ресурсов, а также возможность динамического [масштабирования ресурсов управляемого экземпляра](sql-database-managed-instance-resource-limits.md).

Вы можете создать ваше первое приложение на основе небольшой отдельной базы данных на уровне служб общего назначения по оптимальной ежемесячной цене, а затем в любое время изменить уровень служб на "Критически важный для бизнеса" вручную или программным способом в соответствии с требованиями вашего решения. Вы можете настроить производительность без простоя для приложения и работы клиентов. Динамическая масштабируемость позволяет базе данных прозрачно реагировать на быстро меняющиеся требования к ресурсам. Кроме того, таким образом вы можете платить только за необходимые ресурсы, и только когда они вам нужны.

Динамическое масштабирование отличается от автомасштабирования. Автомасштабирование — процесс, когда масштабирование службы проходит автоматически (в зависимости от критериев). В то время как динамическая масштабируемость дает возможность для ручного масштабирования без простоев. Отдельная база данных поддерживает только динамическую масштабируемость вручную, а не автомасштабирование. Чтобы изучить *автомасштабирование* более детально, необходимо рассмотреть возможность использования эластичных пулов, которые позволяют базам данных обмениваться ресурсами в пуле, исходя из потребностей конкретной базы данных. Однако существуют сценарии, которые могут помочь автоматизировать масштабируемость для отдельной базы данных. С ними можно ознакомиться в статье [Мониторинг и масштабирование отдельной базы данных SQL с помощью PowerShell](scripts/sql-database-monitor-and-scale-database-powershell.md).

### <a name="purchasing-models-service-tiers-compute-sizes-and-storage-amounts"></a>Модели приобретения, уровни служб, размеры вычислительных операций и объема хранилища

База данных SQL предлагает две модели приобретения.
- [Модель приобретения на основе виртуальных ядер](sql-database-service-tiers-vcore.md) позволяет выбрать количество виртуальных ядер, объем памяти и размер и скорость хранилища. Модель приобретения на основе виртуальных ядер также позволяет применять [Преимущество гибридного использования Azure для SQL Server](https://azure.microsoft.com/pricing/hybrid-benefit/), чтобы добиться снижения затрат. Дополнительные сведения о Преимуществе гибридного использования Azure см. в разделе [База данных SQL Azure: часто задаваемые вопросы](#sql-database-frequently-asked-questions-faq).
- [Модель приобретения на основе единиц DTU](sql-database-service-tiers-dtu.md) предлагает набор вычислительных операций, памяти и ресурсов ввода-вывода трех уровней обслуживания, каждый из которых предусматривает поддержку различных рабочих нагрузок баз данных. Для каждого объема вычислительных ресурсов на всех уровнях обслуживания предусмотрено отдельное сочетание этих ресурсов, к которым можно добавить ресурсы хранилища.

### <a name="elastic-pools-to-maximize-resource-utilization"></a>Эластичные пулы для максимального использования ресурсов

Для многих организаций и приложений достаточно иметь возможность создавать отдельные базы данных и уменьшать или увеличивать их производительность по запросу, особенно если закономерности использования базы данных предсказуемы. Но если закономерности использования непредсказуемы, это может усложнить управление расходами и бизнес-моделью. [Эластичные пулы](sql-database-elastic-pool.md) предназначены для решения этой проблемы. Идея проста. Вы выделяете ресурсы для повышения производительности пула, а не отдельной базы данных, и оплачиваете ресурсы общей производительности пула, а не производительность отдельных баз данных.

   ![эластичные пулы](./media/sql-database-what-is-a-dtu/sqldb_elastic_pools.png)

Благодаря использованию эластичных пулов не нужно концентрироваться на повышении или понижении производительности базы данных в соответствии с колебаниями потребностей в ресурсах. Базы данных в пуле потребляют ресурсы производительности пула эластичных баз данных по мере необходимости. Базы данных в пуле используют ресурсы пула, но не превышают ограничений пула, поэтому ваши расходы остаются прогнозируемыми, даже если производительность отдельной базы данных прогнозировать сложно. Более того, можно [добавлять базы данных в пул и удалять их из него](sql-database-elastic-pool-manage-portal.md), масштабируя приложение так, чтобы количество используемых баз данных составляло от нескольких экземпляров до многих тысяч, не выходя за рамки бюджета. Вы также можете управлять минимальным и максимальным объемом ресурсов, доступных для баз данных в пуле. Таким образом ни одна база данных в пуле не будет потреблять все ресурсы пула, и в каждой из этих баз данных будет гарантированный минимальный объем ресурсов. Дополнительные сведения о шаблонах разработки для приложений SaaS, использующих эластичные пулы, см. в статье [Шаблоны разработки для мультитенантных приложений SaaS и Базы данных SQL Azure](sql-database-design-patterns-multi-tenancy-saas-applications.md).

Сценарии могут быть полезны для использования в мониторинге и масштабировании эластичных пулов. С ними можно ознакомится в статье [Отслеживание и масштабирование эластичного пула SQL в Базе данных SQL Azure с помощью PowerShell](scripts/sql-database-monitor-and-scale-pool-powershell.md)

> [!IMPORTANT]
> В управляемом экземпляре не поддерживаются эластичные пулы. Управляемый экземпляр является скорее коллекцией баз данных экземпляра, которые совместно используют ресурсы управляемого экземпляра.

### <a name="blend-single-databases-with-pooled-databases"></a>Совмещение отдельных баз данных и баз данных в пуле

Вы можете смешивать отдельные базы данных с эластичными пулами и изменять уровни служб отдельных баз данных и эластичных пулов, что позволяет легко и быстро адаптировать их под конкретные задачи. Благодаря возможностям Azure вы сможете сочетать службы Azure с базами данных SQL, чтобы удовлетворить уникальные потребности архитектуры приложений, повысить эффективность использования ресурсов и снизить расходы, а также узнать о новых возможностях для развития бизнеса.

## <a name="extensive-monitoring-and-alerting-capabilities"></a>Возможности комплексного мониторинга и оповещения

База данных SQL Azure предоставляет набор усовершенствованных функций мониторинга и устранения неполадок, которые могут помочь получить полное представление о характеристиках рабочей нагрузки. Функции и средства могут быть отнесены к категории:
 - Встроенные возможности мониторинга, предоставляемые последней версией SQL Server ядро СУБД, позволяющие находить аналитические сведения о производительности в режиме реального времени. 
 - Возможности мониторинга PaaS, предоставляемые платформой Azure, позволяющие легко отслеживать большое количество экземпляров баз данных, а также предоставлять советы по устранению неполадок, которые могут помочь в устранении проблем с производительностью.

Наиболее важной встроенной функцией мониторинга ядра СУБД, которую следует использовать, является компонент [хранилища запросов](sql-database-operate-query-store.md) , который записывает производительность запросов в реальном времени и позволяет выявление потенциальных проблем с производительностью, а также лучших потребители ресурсов. Автоматическая настройка и рекомендации содержат рекомендации по запросам с регрессионной производительностью, отсутствующими или повторяющимися индексами. Автоматическая настройка в базе данных SQL Azure позволяет либо вручную применить сценарии, которые могут устранить проблемы, либо позволить базе данных SQL Azure применить исправление, проверить и проверить, предоставляет ли он какое-либо преимущество, а также сохранить или отменить изменения в зависимости от результата. Кроме возможностей хранилища запросов и автоматической настройки, можно также использовать стандартные динамические [административные представления и XEvent](sql-database-monitoring-with-dmvs.md) для наблюдения за производительностью рабочей нагрузки.

Платформа Azure предоставляет встроенные средства [мониторинга производительности](sql-database-performance.md) и [оповещения](sql-database-insights-alerts-portal.md) , в сочетании с рейтингами производительности, которые позволяют легко отслеживать состояние тысяч баз данных. Используя эти средства, вы сможете быстро оценить эффект от увеличения и уменьшения масштаба, исходя из текущей или планируемой загрузки. Кроме того, база данных SQL может [выдавать значения метрик и журналы диагностики](sql-database-metrics-diag-logging.md) для упрощения мониторинга. Вы можете настроить Базу данных SQL для хранения сведений об использовании ресурсов, о рабочих ролях и сеансах, а также настроить подключение к одному из этих ресурсов Azure:

- **Служба хранилища Azure**. Для архивации больших объемов телеметрии по оптимальной стоимости.
- **Концентратор событий Azure**. Для интеграции телеметрии Базы данных SQL с настраиваемым решением для мониторинга или горячими конвейерами.
- **Журналы Azure Monitor**. Для встроенного решения для мониторинга с возможностями предоставления отчетов, предупреждений и выполнения исправлений.

    ![архитектура](./media/sql-database-metrics-diag-logging/architecture.png)

## <a name="availability-capabilities"></a>Возможности доступности

В традиционной SQL Server среде обычно есть (по крайней мере) 2 компьютера, на которых локально настроены точные (синхронно сохраняемые) копии данных (используя такие функции, как группы доступности AlwaysOn или экземпляры отказоустойчивого кластера) для защиты от сбой одного компьютера или компонента. Это обеспечивает высокий уровень доступности, но не защищает центр обработки данных от стихийного сбоя.

При аварийном восстановлении предполагается, что разрушительное событие будет достаточно географически локализовано, чтобы иметь еще один компьютер или набор компьютеров с копией данных.  В SQL Server можно использовать группы доступности Always On, которые работают в асинхронном режиме, чтобы получить эту возможность.  Скорость неисправности обычно означает, что пользователи не хотят ждать, пока репликация не зафиксирует транзакцию, поэтому существует вероятность потери данных при выполнении внеплановой отработки отказа.

Базы данных на уровне обслуживания "Премиум" и "критически важный для бизнеса" уже [выполняют что-то вроде](sql-database-high-availability.md#premium-and-business-critical-service-tier-availability) синхронизации группы доступности. Базы данных на более низких уровнях служб обеспечивают избыточность через хранилище, используя [другой, но эквивалентный механизм](sql-database-high-availability.md#basic-standard-and-general-purpose-service-tier-availability). Существует логика, защищающая от сбоя одного компьютера.  Функция активной георепликации дает возможность защититься от аварии, в которой уничтожается весь регион.

Зоны доступности Azure — это игра на проблему высокой доступности.  Он пытается защититься от сбоя одного центра обработки данных в одном регионе.  Таким образом, он хочет защититься от потери питания или сети в здании. В SQL Azure это позволит разместить разные реплики в разных зонах доступности (в разных зданиях) и в других случаях работать как раньше.

На самом деле, Корпоративное соглашение об уровне обслуживания [(SLA)](https://azure.microsoft.com/support/legal/sla/)в отрасли Azure 99,99%, на базе глобальной сети центров обработки данных, управляемых корпорацией Майкрософт, помогает синхронизировать приложение с 24/7. Платформа Azure полностью управляет каждой базой данных и гарантирует высокий процент доступности данных без их потери. Azure автоматически обрабатывает исправления, резервное копирование, репликацию, выявление сбоев, потенциальные сбои основного оборудования, программного обеспечения или сети, исправления ошибок при развертывании, отработку отказа, обновления баз данных и другие задачи обслуживания. На уровне "Стандартный" доступность достигается за счет разделения уровня вычислений и уровня хранения. На уровне "Премиум" доступность достигается за счет интеграции вычислительных систем и хранилища на одном узле для повышения производительности, а также за счет реализации технологии, схожей с группами доступности AlwaysOn. Полное описание возможностей для обеспечения высокого уровня доступности, предоставляемых в Базе данных SQL Azure, см. в статье [Высокая доступность и база данных SQL Azure](sql-database-high-availability.md). Кроме того, база данных SQL обеспечивает встроенные функции [непрерывности бизнес-процессов и глобальной масштабируемости](sql-database-business-continuity.md), в частности:

- **[Автоматическое резервное копирование](sql-database-automated-backups.md)**

  База данных SQL автоматически создает полную, разностную резервную копию и резервные копии журналов транзакций баз данных Azure SQL, позволяя восстановление до любой точки во времени. Для отдельных баз данных и баз данных в пуле можно настроить Базу данных SQL для сохранения полных резервных копий в хранилище Azure для долгосрочного хранения резервной копии. Для управляемых экземпляров также можно создать резервную копию только для копирования для долгосрочного хранения.

- **[Восстановление до точки во времени](sql-database-recovery-using-backups.md)**

  Все параметры развертывания Базы данных SQL поддерживают восстановление в любой момент времени в пределах автоматического периода удержания резервной копии для любой базы данных SQL Azure.
- **[Активная георепликация](sql-database-active-geo-replication.md)**

  Отдельная база данных и базы данных в пуле позволяют настроить до четырех доступных для чтения баз данных-получателей в одном центре обработки данных или в глобально распределенных центрах обработки данных Azure.  Например, при наличии приложения SaaS с базой данных каталога, содержащей большой объем параллельных транзакций, доступных только для чтения, необходимо использовать активную георепликацию, чтобы включить глобальный масштаб чтения и удалить узкие места в базе данных-источнике, возникшие из-за рабочих нагрузок считывания. Используйте группы автоматической отработки отказа для управляемых экземпляров.
- **[Группы автоматической отработки отказа](sql-database-auto-failover-group.md)** .

  Все параметры развертывания Базы данных SQL позволяют использовать группы отработки отказа для обеспечения высокого уровня доступности и балансировки нагрузки в глобальном масштабе, включая прозрачную георепликацию и отработку отказа больших наборов баз данных, эластичных пулов и управляемых экземпляров. Группы отработки отказа позволяют создавать глобально распределенные приложения SaaS с минимальными расходами на администрирование, оставляя комплексный мониторинг, маршрутизацию и оркестрацию отработки отказа Базе данных SQL.
- **[Базы данных, избыточные между зонами](sql-database-high-availability.md)**

  База данных SQL позволяет подготовить базы данных и эластичные пулы уровня "Премиум" или "Критически важный для бизнеса" в нескольких зонах доступности. Так как эти базы данных или эластичные пулы имеют несколько избыточных реплик для обеспечения высокого уровня доступности, размещение этих реплик в нескольких зонах доступности гарантирует более высокую устойчивость, включая возможность автоматического восстановления после масштабных сбоев центра обработки данных без потери данных.

## <a name="built-in-intelligence"></a>Встроенная система аналитики

С базой данных SQL вы получаете встроенную систему аналитики, которая позволяет значительно сократить расходы на выполнение и обслуживание баз данных, а также повышает производительность и безопасность приложения. Круглосуточно выполняя миллионы пользовательских рабочих нагрузок, база данных SQL собирает и обрабатывает большие объемы данных телеметрии при обеспечении конфиденциальности пользователей. Различные алгоритмы постоянно оценивают данные телеметрии, чтобы служба могла согласовать работу с приложением. На основе этого анализа служба определяет рекомендации по улучшению производительности для конкретной рабочей нагрузки.

### <a name="automatic-performance-monitoring-and-tuning"></a>Автоматический мониторинг и настройка производительности

База данных SQL обеспечивает точное представление о запросах, которые необходимо отслеживать. База данных SQL дает возможность адаптировать схемы базы данных к рабочей нагрузке на основе шаблонов базы данных. База данных SQL предоставляет [рекомендации по настройке производительности](sql-database-advisor.md). Вы можете просмотреть действия по настройке и применить их.

Однако постоянный мониторинг базы данных — это сложная и трудоемкая задача, особенно при работе с несколькими базами данных. [Intelligent Insights](sql-database-intelligent-insights.md) делает это автоматически, отслеживая производительность базы данных SQL в нужном масштабе и сообщая о проблемах снижения производительности. Это решение определяет первопричину проблемы и предлагает рекомендации по повышению производительности, если это возможно.

Эффективное управление огромным числом баз данных невозможно даже с учетом всех доступных средств и отчетов, предоставленных базой данных SQL и порталом Azure. Вместо того чтобы вручную выполнять мониторинг и настройку базы данных, мы рекомендуем делегировать некоторые действия по настройке и мониторингу Базе данных SQL с помощью [автоматической настройки](sql-database-automatic-tuning.md). База данных SQL автоматически применяет рекомендации, тесты и проверяет все действия по настройке, чтобы обеспечить повышение производительности. В этом случае база данных SQL автоматически безопасно адаптируется к рабочей нагрузке. Автоматическая настройка означает, что производительность базы данных тщательно отслеживается и сравнивается перед выполнением действия настройки и после него. Если производительность не улучшается, действие настройки отменяется.

Сейчас многие наши партнеры, выполняющие [мультитенантные приложения SaaS](sql-database-design-patterns-multi-tenancy-saas-applications.md) на основе базы данных SQL, используют автоматическую настройку производительности для обеспечения стабильности и предсказуемой производительности приложений. Они уверены, что эта функция значительно уменьшает риск снижения производительности ночью. Кроме того, так как часть клиентской базы также использует SQL Server, они применяют те же рекомендации по индексации, предоставленные базой данных SQL, для поддержки клиентов SQL Server.

[В базе данных SQL](sql-database-automatic-tuning.md) есть две функции автоматической настройки.

- **Автоматическое управление индексами**. Определяет индексы, которые необходимо добавить в базу данных или удалить.
- **Автоматическое изменение плана**. Определяет проблемные планы и исправляет проблемы с производительностью плана SQL (ожидается в ближайшее время, уже доступна в SQL Server 2017).

### <a name="adaptive-query-processing"></a>Адаптивная обработка запросов

Мы также добавили в базу данных SQL семейство функций [адаптивной обработки запросов](/sql/relational-databases/performance/intelligent-query-processing), включая чередующееся выполнение для функций с табличными значениями и несколькими инструкциями, обратную связь с выделением памяти в пакетном режиме и адаптивные соединения в пакетном режиме. Каждая из этих функций адаптивной обработки запросов применяет сходные методы "обучения и адаптации", чтобы устранить в дальнейшем проблемы производительности, связанные с традиционно трудноразрешимыми проблемами оптимизации запросов.

## <a name="advanced-security-and-compliance"></a>Расширенный уровень безопасности и соответствие требованиям

База данных SQL обеспечивает ряд [встроенных функций безопасности и соответствия](sql-database-security-overview.md), чтобы выполнить различные требования по защите вашего приложения.

> [!IMPORTANT]
> База данных SQL Azure (все варианты развертывания) сертифицирована по рядам стандартов соответствия. Дополнительные сведения см. в [центре управления безопасностью Microsoft Azure](https://gallery.technet.microsoft.com/Overview-of-Azure-c1be3942) , где можно найти самый актуальный список сертификатов соответствия базы данных SQL.

### <a name="advance-threat-protection"></a>Расширенная защита от угроз

Расширенная защита данных представляет собой унифицированный пакет расширенных возможностей безопасности SQL. К ним относятся такие функции, как обнаружение и классификация конфиденциальных данных, управление уязвимостями базы данных, а также выявление аномальной активности, которая может указывать на угрозу для базы данных. Эта служба предоставляет единый центр для включения этих возможностей и управления ими.

- [Обнаружение и классификация данных](sql-database-data-discovery-and-classification.md).

  Эта функция (в настоящее время доступна в предварительной версии) предоставляет возможности, встроенные в службу Базы данных SQL Azure, для обнаружения, классификации, добавления меток и защиты конфиденциальных данных в базах данных. Она может использоваться для просмотра состояния классификации базы данных, а также отслеживания доступа к конфиденциальным данным в базе данных и за ее пределами.
- [Оценка уязвимости](sql-vulnerability-assessment.md).

  Эта служба может обнаруживать, отслеживать потенциальные уязвимости базы данных и помогает устранять их. Эта служба обеспечивает представление о состоянии безопасности и предлагает практические действия для устранения проблем безопасности и усиления защиты базы данных.
- [Обнаружение угроз](sql-database-threat-detection.md).

  Эта функция обнаружения угроз выявляет аномальные операции, указывающие на нестандартные и потенциально вредоносные попытки получить доступ к базам данных или воспользоваться ими. Она непрерывно отслеживает базу данных для выявления подозрительных действий и немедленно выдает оповещения системы безопасности о потенциальных уязвимостях, атаках путем внедрения кода SQL и аномальных шаблонах доступа к базам данных. Оповещения об обнаружении угроз содержат сведения о подозрительных операциях и рекомендации о том, как исследовать причину угрозы и устранить ее.

### <a name="auditing-for-compliance-and-security"></a>Аудит для обеспечения безопасности и соответствия

[Аудит](sql-database-auditing.md) позволяет отслеживать события базы данных и записывать их в журнал аудита в учетной записи хранения Azure. Аудит может помочь вам соблюсти требования нормативов, проанализировать работу с базой данных и получить представление о расхождениях и аномалиях, которые могут указывать на бизнес-проблемы или предполагаемые нарушения безопасности.

### <a name="data-encryption"></a>Шифрование данных

База данных SQL обеспечивает защиту данных, шифруя передаваемые данные с использованием протокола [TLS](https://support.microsoft.com/kb/3135244), неактивные данные с использованием [прозрачного шифрования данных](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql) и используемые данные с помощью функции [Always Encrypted](https://docs.microsoft.com/sql/relational-databases/security/encryption/always-encrypted-database-engine).

### <a name="azure-active-directory-integration-and-multi-factor-authentication"></a>Интеграция Azure Active Directory и Многофакторная идентификация

База данных SQL позволяет централизованно управлять удостоверениями пользователя базы данных и другими службами Майкрософт с помощью [интеграции Azure Active Directory](sql-database-aad-authentication.md). Эта возможность упрощает управление разрешениями и повышает уровень безопасности. Azure Active Directory поддерживает [Многофакторную идентификацию](sql-database-ssms-mfa-authentication.md) (MFA) для повышения безопасности данных и приложений, поддерживая процесс единого входа.

### <a name="compliance-certification"></a>Сертификат соответствия

База данных SQL участвует в регулярном аудите и сертифицирована для нескольких стандартов соответствия. Дополнительные сведения см. в [центре управления безопасностью Microsoft Azure](https://gallery.technet.microsoft.com/Overview-of-Azure-c1be3942) , где можно найти самый актуальный список сертификатов соответствия базы данных SQL.

## <a name="easy-to-use-tools"></a>Простые в использовании инструменты

База данных SQL делает создание и обслуживание приложений более удобным и эффективным. Она позволяет сконцентрироваться на том, что у вас получается лучше всего, — на создании отличных приложений. В базе данных SQL вы можете разрабатывать и обслуживать базы данных с помощью средств и навыков, которые у вас уже есть.

- **[Портал Azure](https://portal.azure.com/)**

  Веб-приложение для управления всеми службами Azure.
- **[SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms)**

  Бесплатное, доступное для скачивания клиентское приложение для управления любой инфраструктурой SQL, от SQL Server до Базы данных SQL.
- **[SQL Server Data Tools в Visual Studio](https://docs.microsoft.com/sql/ssdt/download-sql-server-data-tools-ssdt)**

  Бесплатное, доступное для скачивания клиентское приложение для разработки реляционных баз данных SQL Server, баз данных SQL Azure, пакетов Integration Services, моделей данных Analysis Services и отчетов служб Reporting Services.
- **[Visual Studio Code](https://code.visualstudio.com/docs)**

  Бесплатный, доступный для скачивания редактор кода с открытым кодом для Windows, macOS и Linux, поддерживающий расширения, включая [расширение mssql](https://aka.ms/mssql-marketplace) для выполнения запросов к Microsoft SQL Server, базе данных SQL Azure и хранилищу данных SQL.

База данных SQL поддерживает создание приложений на языках Python, Java, Node.js, PHP, Ruby и .NET для операционных систем MacOS, Windows и Linux. База данных SQL поддерживает те же [библиотеки подключений](sql-database-libraries.md), что и SQL Server.

## <a name="sql-database-frequently-asked-questions-faq"></a>База данных SQL Azure: часто задаваемые вопросы

### <a name="what-is-the-current-version-of-sql-database"></a>Какова текущая версия Базы данных SQL Azure?

Текущая версия базы данных SQL — 12. Версия 11 больше не поддерживается.

### <a name="can-i-control-when-patching-downtime-occurs"></a>Могу ли я контролировать простой в связи с установкой исправлений

Нет. Влияние исправления в основном незаметно, если в вашем приложении [применяется логика повторных попыток](sql-database-develop-overview.md#resiliency). Дополнительные сведения о подготовке к запланированным событиям по обслуживанию базы данных SQL Azure см. в статье [Планирование событий обслуживания Azure в Базе данных SQL Azure](sql-database-planned-maintenance.md).

### <a name="azure-hybrid-benefit-questions"></a>Вопросы о Преимуществе гибридного использования Azure

#### <a name="are-there-dual-use-rights-with-azure-hybrid-benefit-for-sql-server"></a>Включает ли программа "Преимущество гибридного использования Azure для SQL Server" права двойного использования?

Права двойного использования лицензии предусматривают 180 дней для обеспечения беспрепятственного выполнения перемещений. После завершения этого 180-дневного периода лицензию SQL Server можно использовать только в облаке в Базе данных SQL и права двойного использования в локальной среде и облаке больше не актуальны.

#### <a name="how-does-azure-hybrid-benefit-for-sql-server-differ-from-license-mobility"></a>Каким образом программа "Преимущество гибридного использования Azure для SQL Server" отличается от перемещения лицензий?

Сегодня мы предлагаем клиентам SQL Server с Software Assurance преимущества перемещения лицензий, которые позволяют повторно назначать лицензии сторонним общим серверам. Это преимущество можно использовать в IaaS Azure и EC2 AWS.
Программа "Преимущество гибридного использования Azure для SQL Server" отличается от программы перемещения лицензий в двух ключевых аспектах.

- Она предоставляет экономические преимущества перемещения высоко виртуализированных рабочих нагрузок в Azure. Клиенты EE SQL могут получить 4 ядра в Azure в номере SKU общего назначения для каждого ядра, которое доступно в локальной среде для высоко виртуализированных приложений. Программа перемещения лицензий не предусматривает каких-либо особых экономических преимуществ миграции виртуальных рабочих нагрузок в облако.
- Эта программа обеспечивает назначение PaaS в Azure (управляемый экземпляр базы данных SQL Azure), которое совместимо с локальным экземпляром SQL Server

#### <a name="what-are-the-specific-rights-of-the-azure-hybrid-benefit-for-sql-server"></a>Что представляют собой права программы "Преимущество гибридного использования Azure для SQL Server"?

У клиентов Базы данных SQL будут следующие права, связанные с программой "Преимущество гибридного использования Azure для SQL Server":

|Тип лицензии|Что вам дает программа "Преимущество гибридного использования Azure для SQL Server"?|
|---|---|
|Основные клиенты SQL Server Enterprise Edition с Software Assurance|<li>Можно платить по базовому тарифу в номере SKU общего назначения или SKU "Критически важный для бизнеса"</li><br><li>1 ядро в локальной среде = 4 ядра в номере SKU общего назначения</li><br><li>1 ядро в локальной среде = 1 ядро в номере SKU "Критически важный для бизнеса"</li>|
|Основные клиенты SQL Server Standard Edition с Software Assurance|<li>Можно платить по базовому тарифу только в номере SKU общего назначения</li><br><li>1 ядро в локальной среде = 1 ядро в номере SKU общего назначения</li>|
|||

## <a name="engage-with-the-sql-server-engineering-team"></a>Связаться с командой разработчиков SQL Server

- [DBA Stack Exchange](https://dba.stackexchange.com/questions/tagged/sql-server). Вопросы по администрированию баз данных.
- [Stack Overflow](https://stackoverflow.com/questions/tagged/sql-server). Вопросы по разработке.
- [Форумы MSDN](https://social.msdn.microsoft.com/Forums/home?category=sqlserver). Технические вопросы.
- [Отзывы](https://aka.ms/sqlfeedback). Информирование об ошибках и подача запросов на функции.
- [Reddit](https://www.reddit.com/r/SQLServer/). Обсуждение SQL Server.

## <a name="next-steps"></a>Следующие шаги

- Сравнение цен и калькуляторы для отдельных баз данных и пулов эластичных баз данных см. на [странице расценок](https://azure.microsoft.com/pricing/details/sql-database/).
- См. эти краткие руководства по быстрому запуску:

  - [Create and query a single Azure SQL database in the Azure portal](sql-database-single-database-get-started.md) (Создание отдельной базы данных SQL с помощью портала Azure)  
  - [Создание базы данных SQL Azure и отправка к ней запросов с помощью Azure CLI](sql-database-get-started-cli.md)
  - [Создание отдельной базы данных SQL Azure с помощью PowerShell](sql-database-get-started-powershell.md)

- Примеры использования Azure CLI и PowerShell:
  - [Примеры Azure CLI для базы данных SQL Azure](sql-database-cli-samples.md)
  - [Примеры Azure PowerShell для базы данных SQL Azure](sql-database-powershell-samples.md)

 - Сведения о новых возможностях по мере их объявления см. в разделе 
   - **[Стратегия Azure для базы данных SQL](https://azure.microsoft.com/roadmap/?category=databases)** — место для получения новых возможностей и дальнейших действий.
  - **[Блог по базе данных SQL Azure](https://azure.microsoft.com/blog/topics/database)** — место, куда SQL Server участники группы разработчиков блогов о новостях и функциях базы данных SQL.

