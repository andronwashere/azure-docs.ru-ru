---
title: Вопросы и ответы об уровне служб "Гипермасштабирование" Базы данных SQL Azure | Документация Майкрософт
description: Ответы на распространенные вопросы о Базе данных Azure SQL уровня служб "Гипермасштабирование", обычно называемой базой данных уровня "Гипермасштабирование".
services: sql-database
ms.service: sql-database
ms.subservice: ''
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: ''
manager: craigg
ms.date: 05/06/2019
ms.openlocfilehash: 49d1e171d4d4b2210a98c59332f4842e23a2f2b9
ms.sourcegitcommit: 084630bb22ae4cf037794923a1ef602d84831c57
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/03/2019
ms.locfileid: "67537847"
---
# <a name="faq-about-azure-sql-hyperscale-databases"></a>Часто задаваемые вопросы о базах данных SQL Azure уровня "Гипермасштабирование"

В этой статье содержатся ответы на часто задаваемые вопросы для клиентов, рассматривающих возможность базы данных на уровне службы Гипермасштабируемого базы данных SQL Azure, обычно называется Гипермасштабируемого базы данных. В этой статье описываются сценарии, которые поддерживает уровень службы "Гипермасштабирование", и службы с разными компонентами, совместимые с уровнем "Гипермасштабирование" Базы данных SQL в целом.

- Эта статья предназначена для читателей, которые получили базовые сведения об уровне служб "Гипермасштабирование" и ищут ответы на конкретные вопросы.
- Это не руководство и не ответы на вопросы о том, как использовать Базу данных SQL уровня "Гипермасштабирование". Дополнительные сведения см. в статье [Hyperscale service tier (preview) for up to 100 TB](sql-database-service-tier-hyperscale.md) (Уровень службы "Гипермасштабирование" размером до 100 ТБ (предварительная версия)).

## <a name="general-questions"></a>Общие вопросы

### <a name="what-is-a-hyperscale-database"></a>Что такое база данных уровня "Гипермасштабирование"

База данных уровня "Гипермасштабирование" представляет собой Базу данных Azure SQL уровня служб "Гипермасштабирование" на базе технологии горизонтально масштабируемого хранилища уровня "Гипермасштабирование". База данных уровня "Гипермасштабирование" поддерживает размещение до 100 ТБ данных и обеспечивает высокую пропускную способность и производительность, а также быстрое масштабирование, чтобы адаптироваться к требованиям рабочей нагрузки. Масштабирование прозрачно для приложения — подключение, обработка запросов и другие операции выполняются как и в любой другой базе данных SQL.

### <a name="what-resource-types-and-purchasing-models-support-hyperscale"></a>Какие типы ресурсов и модели приобретения поддерживает уровень "Гипермасштабирование"

Уровень служб "Гипермасштабирование" доступен только для отдельных баз данных с моделью приобретения на основе виртуальных ядер в Базе данных SQL Azure.  

### <a name="how-does-the-hyperscale-service-tier-differ-from-the-general-purpose-and-business-critical-service-tiers"></a>Чем уровень служб "Гипермасштабирование" отличается от уровней "Критически важный для бизнеса" и "Общее назначение"

Служба на основе виртуальных ядер, уровни в основном отличаются в зависимости от доступности, тип хранилища и операций ввода-вывода.

- Уровень служб "Общее назначение" подходит для большинства бизнес-нагрузок, предоставляет сбалансированный набор вычислительных ресурсов и ресурсов хранения, где время ожидания операций ввода-вывода или время отработки отказа не являются приоритетом.
- Уровень служб "Гипермасштабирование" оптимизирован для рабочих нагрузок сверхбольших баз данных.
- Уровень служб "Критически важный для бизнеса" подходит для рабочих нагрузок, где задержка операций ввода-вывода является приоритетной задачей.

| | Тип ресурса | Общего назначения |  Уровень "Гипермасштабирование" | Критически важный для бизнеса |
|:---|:---:|:---:|:---:|:---:|:---:|
| **Оптимально для** |Все|  Большинства рабочих нагрузок. Предлагает бюджетные сбалансированные варианты вычислительных ресурсов и ресурсов хранилища. | Приложения данных с высокими требованиями к емкости данных и возможностью автоматического масштабирования ресурсов хранилища и быстрого масштабирования вычислительных ресурсов. | Приложения OLTP с большим количеством транзакций и минимальной задержкой операций ввода-вывода. Предлагает самую высокую отказоустойчивость к сбоям благодаря использованию нескольких изолированных реплик.|
|  **Тип ресурса** ||Отдельная база данных / эластичный пул / управляемый экземпляр | Отдельная база данных | Отдельная база данных / эластичный пул / управляемый экземпляр |
| **Объем вычислительных ресурсов**|Отдельная база данных / эластичный пул* | От 1 до 80 виртуальных ядер | От 1 до 80 виртуальных ядер* | От 1 до 80 виртуальных ядер |
| |Управляемый экземпляр | 8, 16, 24, 32, 40, 64, 80 виртуальных ядер | Н/Д | 8, 16, 24, 32, 40, 64, 80 виртуальных ядер |
| **Тип хранилища** | Все |Удаленное хранилище класса Premium (для каждого экземпляра) | Не связанное хранилище с локальным кэшем SSD (для каждого экземпляра) | Сверхбыстрое локальное хранилище SSD (для каждого экземпляра) |
| **Размер хранилища** | Отдельная база данных / эластичный пул | От 5 ГБ до 4 ТБ | До 100 ТБ | От 5 ГБ до 4 ТБ |
| | Управляемый экземпляр  | От 32 ГБ до 8 ТБ | Н/Д | От 32 ГБ до 4 ТБ |
| **Пропускная способность ввода-вывода** | Отдельная база данных** | 500 операций ввода-вывода в секунду на виртуальное ядро с максимальным количеством 7000 | ИТ — это многоуровневая архитектура с кэшированием на нескольких уровнях. Действующие операций ввода-вывода будет зависеть от рабочей нагрузки. | 5000 операций ввода-вывода с максимальным значением 200 000 операций ввода-вывода|
| | Управляемый экземпляр | Зависит от размера файла | Н/Д | Управляемый экземпляр: Зависит от размера файла|
|**Доступность**|Все|1 реплика, отсутствие возможности масштабирования для чтения, отсутствие локального кэша | Несколько реплик, до 15 операций масштабирования для чтения, частичный локальный кэш | 3 реплики, 1 операция масштабирования для чтения, высокая доступность, избыточная между зонами, полный локальный кэш |
|**Резервные копии**|Все|Геоизбыточное хранилище с доступом на чтение, 7–35 дней (7 дней по умолчанию)| RA-GRS, 7 – 35 дней (7 дней по умолчанию), восстановления на определенный момент времени (PITR) | Геоизбыточное хранилище с доступом на чтение, 7–35 дней (7 дней по умолчанию) |

\* Эластичные пулы не поддерживают уровень службы "Гипермасштабирование".

### <a name="who-should-use-the-hyperscale-service-tier"></a>Кому рекомендуется использовать уровень служб "Гипермасштабирование"

Уровень служб "Гипермасштабирование" в первую очередь предназначен для клиентов с большими локальными базами данных SQL Server, которые хотят модернизировать свои приложения, перейдя в облако, или для клиентов, которые уже используют Базу данных Azure SQL и желают значительно расширить возможности для роста базы данных. Он также предназначен для клиентов, которым нужна высокая производительность и масштабируемость. Уровень "Гипермасштабирование" представляет следующие возможности:

- Поддержка размера базы данных до 100 ТБ.
- Быстрое резервное копирование базы данных независимо от размера базы данных (резервные копии основаны на моментальных снимках файлов).
- Быстрое восстановление базы данных независимо от размера базы данных (восстановление основано на моментальных снимках файлов).
- Высокая пропускная способность ведения журнала позволяет ускорить выполнение транзакций независимо от объема базы данных.
- Масштабирование для чтения до одного или нескольких узлов только для чтения обеспечивает разгрузку рабочей нагрузки для чтения и для горячих резервных копий.
- Быстрое постоянное увеличение масштаба вычислительных ресурсов позволяет увеличить мощность для обработки высокой рабочей нагрузки и мгновенное уменьшение масштаба. Это похоже на масштабирование между размерами P6 и P11, но гораздо быстрее, так как это не размер операций с данными.

### <a name="what-regions-currently-support-hyperscale"></a>Какие регионы сейчас поддерживают уровень "Гипермасштабирование"

На уровне обработки базы данных SQL Azure сейчас доступна в регионах, перечисленных в разделе [Обзор Гипермасштабируемого базы данных SQL Azure](sql-database-service-tier-hyperscale.md#regions).

### <a name="can-i-create-multiple-hyperscale-databases-per-logical-server"></a>Можно ли создать несколько баз данных уровня "Гипермасштабирование" для каждого логического сервера

Да. Сведения об ограничениях числа баз данных уровня "Гипермасштабирование" на логическом сервере см. в статье [SQL Database resource limits for single and pooled databases on a logical server](sql-database-resource-limits-logical-server.md) (Ограничения ресурсов Базы данных SQL для отдельных баз данных и баз данных в пуле на логическом сервере).

### <a name="what-are-the-performance-characteristics-of-a-hyperscale-database"></a>Каковы характеристики производительности базы данных Гипермасштабируемого

Архитектура уровня "Гипермасштабирование" Базы данных SQL обеспечивает высокую производительность и пропускную способность, поддерживая большие размеры баз данных. 

### <a name="what-is-the-scalability-of-a-hyperscale-database"></a>Какова масштабируемость баз данных уровня "Гипермасштабирование"

Уровень "Гипермасштабирование" Базы данных SQL обеспечивает быстрое масштабирование в зависимости от потребностей рабочей нагрузки.

- **Увеличение и уменьшение масштаба**

  На уровне "Гипермасштабирование" вы можете масштабировать основной объем вычислительных ресурсов с точки зрения ресурсов, таких как процессор и память, а затем мгновенно уменьшать масштаб. Так как хранилище является общим, увеличение и уменьшение масштаба — это не размер операции с данными.  
- **Свертывание и развертывание**

  На уровне "Гипермасштабирование" вы также можете подготовить к работе один или несколько дополнительных вычислительных узлов, которые можно использовать для обслуживания запросов на чтение. Это означает, что вы можете использовать эти дополнительные вычислительные узлы как узлы только для чтения, чтобы выгрузить рабочую нагрузку чтения из первичного вычислительного узла. В дополнение к только для чтения, для этих узлов также могут использоваться как горячей замены, при отработке отказа с сервера-источника.

  Подготовка каждого из этих дополнительных вычислительных узлов может выполняться мгновенно и является сетевой операцией. Вы можете подключиться к этим дополнительным вычислительным узлам только для чтения, установив для аргумента `ApplicationIntent` в строке подключения значение `readonly`. Все подключения со значением `readonly` автоматически направляются на один из дополнительных вычислительных узлов только для чтения.

## <a name="deep-dive-questions"></a>Детализированные вопросы

### <a name="can-i-mix-hyperscale-and-single-databases-in-a-single-logical-server"></a>Можно ли комбинировать Гипермасштабируемого и отдельные базы данных на одном логическом сервере

Да, можно.

### <a name="does-hyperscale-require-my-application-programming-model-to-change"></a>Требуется ли для уровня "Гипермасштабирование" менять модель программирования приложения

Нет, модель программирования приложения остается как есть. Используйте строку подключения как обычно вместе с другими режимами взаимодействия с базой данных Azure SQL.

### <a name="what-transaction-isolation-levels-are-going-to-be-default-on-sql-database-hyperscale-database"></a>Какие уровни изоляции транзакций будут использоваться по умолчанию в Базе данных SQL уровня "Гипермасштабирование"

На основном узле уровень изоляции транзакции — "Изоляция фиксации моментального снимка при чтении (RCSI)". На вторичных узлах с масштабированием для чтения используется уровень изоляции "Моментальный снимок".

### <a name="can-i-bring-my-on-premises-or-iaas-sql-server-license-to-sql-database-hyperscale"></a>Можно ли использовать локальную лицензию или лицензию IaaS SQL Server для уровня "Гипермасштабирование" Базы данных SQL

Да, [Преимущество гибридного использования Azure](https://azure.microsoft.com/pricing/hybrid-benefit/) доступно на уровне "Гипермасштабирование". Каждое ядро SQL Server Standard можно сопоставить с 1 виртуальным ядром уровня "Гипермасштабирование". Каждое ядро SQL Server Enterprise можно сопоставить с 4 виртуальными ядрами уровня "Гипермасштабирование". Для вторичных реплик не требуется лицензия SQL. Цена Преимущества гибридного использования Azure автоматически применяется к репликам с масштабированием для чтения (вторичным).

### <a name="what-kind-of-workloads-is-sql-database-hyperscale-designed-for"></a>Для каких рабочих нагрузок предназначен уровень "Гипермасштабирование" Базы данных SQL

Уровень "Гипермасштабирование" Базы данных SQL поддерживает все рабочие нагрузки SQL Server, но в первую очередь он оптимизирован для OLTP. Вы можете добавить гибридное (HTAP) и Analytical (Киоск данных) рабочих нагрузок также.

### <a name="how-can-i-choose-between-azure-sql-data-warehouse-and-sql-database-hyperscale"></a>Как выбрать между Хранилищем данных SQL Azure и уровнем "Гипермасштабирование" Базы данных SQL

Если в настоящее время выполняется интерактивные аналитические запросы, используя в качестве хранилища данных SQL Server, Гипермасштабируемого базы данных SQL очень удобно, поскольку теперь можно размещать хранилищ данных относительно небольшой (например, несколько ТБ до нескольких десятков ТБ) по более низкой цене и вы можете перенести ваши данные w клад рабочей нагрузки для обработки базы данных SQL без изменения кода T-SQL.

Если вы используете аналитику данных в больших масштабах со сложными запросами, а также хранилище данных Parallel Data Warehouse (PDW), Teradata или другие хранилища данных MPP, Хранилище данных SQL может быть самым оптимальным вариантом.
  
## <a name="sql-database-hyperscale-compute-questions"></a>Вопросы о вычислениях на уровне "Гипермасштабирование" Базы данных SQL

### <a name="can-i-pause-my-compute-at-any-time"></a>Можно ли приостановить вычисления в любое время

В настоящее время нет, однако можно масштабировать вычислительные ресурсы и число реплик вниз, чтобы снизить стоимость периоды низкой нагрузки.

### <a name="can-i-provision-a-compute-with-extra-ram-for-my-memory-intensive-workload"></a>Можно ли подготовить вычислительный ресурс с дополнительной оперативной памятью для рабочей нагрузки с интенсивным использованием памяти

№ Для увеличения объема ОЗУ необходимо использовать более высокий уровень объема вычислительных ресурсов. Дополнительные сведения см. в разделе [Уровень служб "Гипермасштабирование" (предварительная версия)](sql-database-vcore-resource-limits-single-databases.md#hyperscale-service-tier).

### <a name="can-i-provision-multiple-compute-nodes-of-different-sizes"></a>Можно ли подготовить несколько вычислительных узлов разного размера

№

### <a name="how-many-read-scale-replicas-are-supported"></a>Сколько поддерживается реплик с масштабированием для чтения

Гипермасштабируемый базы данных создаются с одной репликой масштабирования для чтения (две реплики в целом), по умолчанию. Вы можете масштабировать число реплик только для чтения от 0 до 4 с помощью [портала Azure](https://portal.azure.com), [T-SQL](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql?view=azuresqldb-current), [Powershell](https://docs.microsoft.com/powershell/module/azurerm.sql/set-azurermsqldatabase) или [CLI](https://docs.microsoft.com/cli/azure/sql/db#az-sql-db-update).

### <a name="for-high-availability-do-i-need-to-provision-additional-compute-nodes"></a>Нужно ли подготавливать дополнительные вычислительные узлы для обеспечения высокой доступности

В базах данных Гипермасштабируемого устойчивость предоставляется на уровне хранилища. Требуется только одна реплика для обеспечения устойчивости. Когда реплика вычисления выходит из строя, новая реплика создается автоматически без потери данных.

Однако, если есть только одна реплика, может потребоваться некоторое время для создания локального кэша в новой реплике после отработки отказа. На этапе перестроения кэша база данных извлекает данные непосредственно с серверов страниц, что приводит к снижению производительности операций ввода-вывода и запросов.

Для критически важных приложений, требующих высокой доступности, необходимо подготовить по крайней мере два вычислительных узла, включая основной (по умолчанию). Таким образом, в случае сбоя имеется горячая резервная копия.

## <a name="data-size-and-storage-questions"></a>Вопросы по размеру и хранению данных

### <a name="what-is-the-max-db-size-supported-with-sql-database-hyperscale"></a>Что такое максимальный размер базы данных, поддерживаемый на уровне "Гипермасштабирование" Базы данных SQL

100 ТБ

### <a name="what-is-the-size-of-the-transaction-log-with-hyperscale"></a>Каков размер журнала транзакций на уровне "Гипермасштабирование"

На уровне "Гипермасштабирование" журнал транзакций практически бесконечен. Вам не нужно беспокоиться об исчерпании пространства журнала в системе с высокой пропускной способностью журнала. Тем не менее, скорость создания журнала может регулироваться для непрерывных агрессивных рабочих нагрузок. Скорость создания журнала устойчивый пиковой — приблизительно 100 МБИТ/с.

### <a name="does-my-temp-db-scale-as-my-database-grows"></a>Масштабируется ли база данных tempdb по мере роста базы данных

Ваша база данных `tempdb` находится в локальном хранилище SSD и настроена на основе подготавливаемого объема вычислительных ресурсов. `tempdb` оптимизирована, чтобы обеспечить максимальную производительность. Размер `tempdb` не настраивается и управляется подсистемой хранения.

### <a name="does-my-database-size-automatically-grow-or-do-i-have-to-manage-the-size-of-the-data-files"></a>Увеличивается ли размер базы данных автоматически, или же нужно управлять размером файлов данных

Размер вашей базы данных автоматически увеличивается, когда вы вставляете или принимаете дополнительные данные.

### <a name="what-is-the-smallest-database-size-that-sql-database-hyperscale-supports-or-starts-with"></a>Каков минимальный и начальный поддерживаемый размер на уровне "Гипермасштабирование" Базы данных SQL

10 ГБ

### <a name="in-what-increments-does-my-database-size-grow"></a>На сколько увеличивается размер базы данных за раз

1 GB

### <a name="is-the-storage-in-sql-database-hyperscale-local-or-remote"></a>Является ли хранилище на уровне "Гипермасштабирование" Базы данных SQL локальным или удаленным

На уровне "Гипермасштабирование" файлы данных хранятся в стандартном хранилище Azure. Данные эффективно кэшируются в локальном хранилище SSD на компьютерах, близких к вычислительным узлам. Кроме того, кэш вычислительных узлов хранится на локальном SSD и в памяти (буферный пул и т. д.), чтобы данные реже поступали с удаленных узлов.

### <a name="can-i-manage-or-define-files-or-filegroups-with-hyperscale"></a>Можно ли управлять файлами или файловыми группами или определять их на уровне "Гипермасштабирование"

Нет
  
### <a name="can-i-provision-a-hard-cap-on-the-data-growth-for-my-database"></a>Можно ли задать жестко фиксированное увеличение объема данных в базе данных

Нет

### <a name="how-are-data-files-laid-out-with-sql-database-hyperscale"></a>Как файлы данных располагаются на уровне "Гипермасштабирование" Базы данных SQL

Файлами данных управляют серверы страниц. По мере увеличения размера данных добавляются файлы данных и связанные узлы сервера страниц.

### <a name="is-database-shrink-supported"></a>Поддерживается ли уменьшение базы данных

Нет

### <a name="is-database-compression-supported"></a>Поддерживается ли сжатие базы данных

Yes

### <a name="if-i-have-a-huge-table-does-my-table-data-get-spread-out-across-multiple-data-files"></a>Распространяются ли данные в огромной таблице по нескольким файлам данных

Да. Страницы данных, связанные с заданной таблицей, могут оказаться в нескольких файлах данных, которые входят в одну файловую группу. SQL Server использует [стратегию пропорционального заполнения](https://docs.microsoft.com/sql/relational-databases/databases/database-files-and-filegroups#file-and-filegroup-fill-strategy) для распространения данных по файлам данных.

## <a name="data-migration-questions"></a>Вопросы, связанные с миграцией данных

### <a name="can-i-move-my-existing-azure-sql-databases-to-the-hyperscale-service-tier"></a>Можно ли переместить имеющиеся базы данных Azure SQL на уровень служб "Гипермасштабирование"

Да. Имеющиеся базы данных Azure SQL можно переместить на уровень "Гипермасштабирование". Это односторонняя миграции. Невозможно переместить базы данных с уровня служб "Гипермасштабирование" на другой уровень служб. Мы рекомендуем сделать копию ваших рабочих баз данных и выполнить перенос на уровень "Гипермасштабирование" для подтверждения концепции.
  
### <a name="can-i-move-my-hyperscale-databases-to-other-editions"></a>Можно ли перенести базы данных уровня "Гипермасштабирование" в другие выпуски

№ В настоящее время невозможно переместить Гипермасштабируемого базы данных на другой уровень службы.

### <a name="do-i-lose-any-functionality-or-capabilities-after-migration-to-the-hyperscale-service-tier"></a>Будут ли утеряны функции или возможности после миграции на уровень служб "Гипермасштабирование"

Да. Некоторые функции базы данных SQL Azure пока не поддерживаются в ИТ, включая, но не ограничиваясь долго резервной копии долгосрочного хранения. После переноса баз данных на уровень "Гипермасштабирование" эти функции перестают работать.  Ожидается, что эти ограничения временный.

### <a name="can-i-move-my--on-premises-sql-server-database-or-my-sql-server-virtual-machine-database-to-hyperscale"></a>Можно ли перенести локальную базу данных SQL Server или базу данных на виртуальной машине SQL Server на уровень "Гипермасштабирование"

Да. Для миграции на уровень служб "Гипермасштабирование" можно использовать все имеющиеся технологии миграции, включая BACPAC, репликацию транзакций, загрузку логических данных. Дополнительные сведения см. в статье [Что такое Azure Database Migration Service?](../dms/dms-overview.md).

### <a name="what-is-my-downtime-during-migration-from-an-on-premises-or-virtual-machine-environment-to-hyperscale-and-how-can-i-minimize-it"></a>Что такое простой во время миграции из локальной среды или виртуальной машины на уровень "Гипермасштабирование", и как свести его минимуму

Время простоя такое же, как и время простоя при переносе баз данных в отдельную базу данных в Базе данных Azure SQL. Вы можете использовать [репликацию транзакций](replication-to-sql-database.md#data-migration-scenario
), чтобы минимизировать время простоя при миграции баз данных размером до нескольких TБ. Для очень больших баз данных (более 10 ТБ) вы можете перенести данные с помощью ADF, Spark или других технологий перемещения данных.

### <a name="how-much-time-would-it-take-to-bring-in-x-amount-of-data-to-sql-database-hyperscale"></a>Сколько времени потребуется, чтобы перенести X данных на уровень "Гипермасштабирование" Базы данных SQL

Гипермасштабируемый способен потребляет 100 МБ в секунду для новых или измененных данных.

### <a name="can-i-read-data-from-blob-storage-and-do-fast-load-like-polybase-and-sql-data-warehouse"></a>Можно ли считывать данные из хранилища BLOB-объектов и выполнять быструю загрузку (например, как в Polybase и Хранилище данных SQL)

Вы можете считывать данные из службы хранилища Azure Storage и загружать данные в базу данных уровня "Гипермасштабирование" (так же как и в обычной отдельной базе данных). В настоящее время Polybase не поддерживается в Базе данных SQL Azure. Polybase можно сделать с помощью [Фабрики данных Azure](https://docs.microsoft.com/azure/data-factory/) или при запуске задания Spark в [Azure Databricks](https://docs.microsoft.com/azure/azure-databricks/) с помощью [соединителя Spark для SQL](sql-database-spark-connector.md). Соединитель Spark для SQL поддерживает массовую вставку.

На уровне "Гипермасштабирование" не поддерживается простая модель восстановления или массового ведения журнала. Для обеспечения высокой доступности требуется модель полного восстановления. Однако на уровне "Гипермасштабирование" обеспечивается более быстрая скорость сбора данных по сравнению с отдельной базой данных SQL Azure благодаря новой архитектуре журнала.

### <a name="does-sql-database-hyperscale-allow-provisioning-multiple-nodes-for-ingesting-large-amounts-of-data"></a>Допускается ли на уровне "Гипермасштабирование" Базы данных SQL подготовка нескольких узлов для приема больших объемов данных

№ Уровень "Гипермасштабирование" Базы данных SQL — это архитектура SMP, а не архитектура с асимметричной многопроцессорной обработкой или архитектура с несколькими источниками. Вы можете создать всего несколько реплик для развертывания рабочих нагрузок только для чтения.

### <a name="what-is-the-oldest-sql-server-version-will-sql-database-hyperscale-support-migration-from"></a>Какова самая старая версия SQL Server, поддерживающая миграция на уровень "Гипермасштабирование" Базы данных SQL

SQL Server 2005. Дополнительные сведения см. в разделе [Перенос в отдельную базу данных или базу данных в пуле](sql-database-single-database-migrate.md#migrate-to-a-single-database-or-a-pooled-database). Дополнительные сведения см. в разделе [Устранение проблем совместимости при миграции базы данных](sql-database-single-database-migrate.md#resolving-database-migration-compatibility-issues).

### <a name="does-sql-database-hyperscale-support-migration-from-other-data-sources-such-as-aurora-mysql-oracle-db2-and-other-database-platforms"></a>Поддерживается ли на уровне "Гипермасштабирование" Базы данных SQL миграция из других источников данных, таких как Aurora, MySQL, Oracle, DB2 и прочих платформ баз данных

Да. В случае разных источников данных, отличных от SQL Server, требуется логическая миграция. Вы можете использовать [Azure Database Migration Service](../dms/dms-overview.md) для логической миграции.

## <a name="business-continuity-and-disaster-recovery-questions"></a>Вопросы о непрерывности бизнес-процессов и аварийном восстановлении

### <a name="what-slas-are-provided-for-a-hyperscale-database"></a>Какие Соглашения об уровне обслуживания поддерживаются в базах данных уровня "Гипермасштабирование"

Значение по умолчанию, основная плюс 1 вторичная реплика соглашение об уровне ОБСЛУЖИВАНИЯ является доступности 99,95%.  С помощью нескольких реплик соглашение об уровне ОБСЛУЖИВАНИЯ переходит на уровне 99,99%.  

### <a name="are-the-database-backups-managed-for-me-by-the-azure-sql-database-service"></a>Управляет ли резервными копиями базы данных служба "База данных SQL Azure"

Yes

### <a name="how-often-are-the-database-backups-taken"></a>Как часто создаются резервные копии баз данных

Для баз данных SQL уровня "Гипермасштабирование" не предусмотрены традиционные полные, разностные резервные копии и резервные копии журналов. Вместо этого делаются регулярные моментальные снимки файлов данных и создаваемый журнал просто сохраняется в течение настроенного или доступного периода хранения.

### <a name="does-sql-database-hyperscale-support-point-in-time-restore"></a>Поддерживается ли на уровне "Гипермасштабирование" Базы данных SQL восстановление на момент времени

Yes

### <a name="what-is-the-recovery-point-objective-rporecovery-time-objective-rto-with-backuprestore-in-sql-database-hyperscale"></a>Каково целевое время восстановления (RTO) и целевая точка восстановления (RPO) для резервного копирования или восстановления на уровне "Гипермасштабирование" Базы данных SQL

RPO составляет 0 мин, а RTO — менее 10 минут независимо от размера базы данных. 

### <a name="do-backups-of-large-databases-affect-compute-performance-on-my-primary"></a>Влияет ли резервное копирование больших баз данных на производительность вычислений на основном узле

№ Резервным копированием управляет подсистема хранения. В процессе используются моментальные снимки файлов. Они не влияют на нагрузку пользователя на основном узле.

### <a name="can-i-perform-geo-restore-with-a-sql-database-hyperscale-database"></a>Можно ли выполнить геовосстановление на уровне "Гипермасштабирование" Базы данных SQL

Да.  Географическое восстановление полностью поддерживается.

### <a name="can-i-setup-geo-replication-with-sql-database-hyperscale-database"></a>Можно ли настроить георепликацию на уровне "Гипермасштабирование" Базы данных SQL

На данный момент нет.

### <a name="do-my-secondary-compute-nodes-get-geo-replicated-with-sql-database-hyperscale"></a>Могут ли мои вторичные вычислительные узлы геореплицироваться на уровне "Гипермасштабирование" Базы данных SQL

На данный момент нет.

### <a name="can-i-take-a-sql-database-hyperscale-database-backup-and-restore-it-to-my-on-premises-server-or-sql-server-in-vm"></a>Можно ли создать резервную копию базы данных SQL уровня "Гипермасштабирование" и восстановить ее на локальном сервере или SQL Server на виртуальной машине

№ Формат хранения для баз данных уровня "Гипермасштабирование" отличается от традиционного SQL Server. Вы не управляете резервными копиями и не имеете к ним доступа. Чтобы извлечь данные из базы данных SQL уровня "Гипермасштабирование", используйте службу экспорта или сценарии вместе с BCP.

## <a name="cross-feature-questions"></a>Вопросы о кросс-функциях

### <a name="do-i-lose-any-functionality-or-capabilities-after-migration-to-the-hyperscale-service-tier"></a>Будут ли утеряны функции или возможности после миграции на уровень служб "Гипермасштабирование"

Да. Некоторые функции базы данных SQL Azure не поддерживаются в ИТ, включая, но не ограничиваясь длительный срок хранения архивации. После переноса баз данных на уровень "Гипермасштабирование" эти функции перестают работать.

### <a name="will-polybase-work-with-sql-database-hyperscale"></a>Будет ли Polybase работать с уровнем "Гипермасштабирование" Базы данных SQL

№ Polybase не поддерживается в Базе данных SQL Azure.

### <a name="does-the-compute-have-support-for-r-and-python"></a>Поддерживают ли вычислительные ресурсы R и Python

№ R и Python не поддерживаются в Базе данных SQL Azure.

### <a name="are-the-compute-nodes-containerized"></a>Являются ли вычислительные узлы контейнерными

№ База данных находится в вычислительной виртуальной машине, а не в контейнере.

## <a name="performance-questions"></a>Вопросы о производительности

### <a name="how-much-throughput-can-i-push-on-the-largest-sql-database-hyperscale-compute"></a>Сколько пропускной способности может обеспечить самый большой вычислительный ресурс уровня "Гипермасштабирование" Базы данных SQL

Мы уже видели согласованные 100 МБ/с информации об изменениях (создания данных журнала транзакций)

### <a name="how-many-iops-do-i-get-on-the-largest-sql-database-hyperscale-compute"></a>Сколько операции ввода-вывода в секунду может обеспечить самый большой вычислительный ресурс уровня "Гипермасштабирование" Базы данных SQL

Операций ввода-ВЫВОДА и задержки операций ввода-ВЫВОДА будет зависеть от шаблонов рабочих нагрузок.  Если данные нужно получить доступ, локальный кэш вычислений, она будет те же шаблоны операций ввода-ВЫВОДА, что локальный SSD.   

### <a name="does-my-throughput-get-affected-by-backups"></a>Влияет ли резервное копирование на пропускную способность

№ Вычислительные ресурсы не связаны с уровнем хранения, чтобы избежать влияния на вычисления.

### <a name="does-my-throughput-get-affected-as-i-provision-additional-compute-nodes"></a>Меняется ли пропускная способность при подготовке дополнительных вычислительных узлов

Так как хранилище совместно и прямого физического репликация данных не происходит между первичной и вторичной вычислительных узлов, с технической точки зрения пропускной способности на основном узле не повлияет путем добавления узлов чтения и масштабирования. Тем не менее мы можем регулировать непрерывную агрессивную рабочую нагрузку, чтобы ускорить применение журналов на вторичных узлах и серверах страниц и избежать плохой производительности чтения на вторичных узлах.

## <a name="scalability-questions"></a>Вопросы о масштабируемости

### <a name="how-long-would-it-take-to-scale-up-and-down-a-compute-node"></a>Сколько времени потребуется для увеличения и уменьшения масштаба вычислительного узла

Масштабирование вычислений или вниз, занимает 5 – 10 минут независимо от размера данных.

### <a name="is-my-database-offline-while-the-scaling-updown-operation-is-in-progress"></a>Переходит ли база данных в автономный режим при увеличении и уменьшении масштаба

№ Увеличение и уменьшение масштаба будет выполнятся в сетевом режиме.

### <a name="should-i-expect-connection-drop-when-the-scaling-operations-are-in-progress"></a>Прервется ли соединение при выполнении операции масштабирования

При масштабировании имеющиеся соединения прерываются во время отработки отказа вычислительного узла целевого размера. Добавление реплик для чтения не приводит к разрыву соединения.

### <a name="is-the-scaling-up-and-down-of-compute-nodes-automatic-or-end-user-triggered-operation"></a>Является ли масштабирование вычислительных узлов автоматическим или управляется пользователем

Масштабированием управляет пользователь. Оно не автоматическое.  

### <a name="does-my-tempb-also-grow-as-the-compute-is-scaled-up"></a>Увеличивается ли `tempb` при увеличении масштаба вычислений

Да. База данных tempdb автоматически масштабируется по мере роста вычислительных ресурсов.  

### <a name="can-i-provision-multiple-primary-compute-nodes-such-as-a-multi-master-system-where-multiple-primary-compute-heads-can-drive-a-higher-level-of-concurrency"></a>Можно подготовить несколько основной вычислительных узлов например хозяев системы, где несколько головок основной вычислений может управлять более высокая степень параллелизма

№ Только основной вычислительный узел принимает запросы на чтение и запись. Вторичные вычислительные узлы принимают только запросы на чтение.

## <a name="read-scale-questions"></a>Вопросы о масштабировании для чтения

### <a name="how-many-secondary-compute-nodes-can-i-provision"></a>Сколько вторичных вычислительных узлов можно подготовить

Мы создадим 2 реплики для Гипермасштабируемых баз данных по умолчанию. Если вы решили изменить число реплик, можно сделать с помощью [портала Azure](https://portal.azure.com).

### <a name="how-do-i-connect-to-these-secondary-compute-nodes"></a>Как подключиться к этим вторичным вычислительным узлам

Вы можете подключиться к этим дополнительным вычислительным узлам только для чтения, установив для аргумента `ApplicationIntent` в строке подключения значение `readonly`. Все подключения со значением `readonly` автоматически направляются на один из дополнительных вычислительных узлов только для чтения.  

### <a name="can-i-create-a-dedicated-endpoint-for-the-read-scale-replica"></a>Можно ли создать выделенную конечную точку реплики с масштабированием для чтения

№ Можно подключиться только к реплике для чтения и масштабирования, указав `ApplicationIntent=ReadOnly`.

### <a name="does-the-system-do-intelligent-load-balancing-of-the-read-workload"></a>Выполняет ли система интеллектуальную балансировку рабочей нагрузки чтения

№ Рабочая нагрузка только для чтения перенаправляется в случайные реплики чтения и масштабирования.

### <a name="can-i-scale-updown-the-secondary-compute-nodes-independently-of-the-primary-compute"></a>Можно ли увеличивать и уменьшать масштаб вторичных вычислительных узлов независимо от первичного узла

№ Вторичный вычислительных узлов также используются для обеспечения высокой ДОСТУПНОСТИ, поэтому они должны иметь ту же конфигурацию, источник, в случае отработки отказа.

### <a name="do-i-get-different-temp-db-sizing-for-my-primary-compute-and-my-additional-secondary-compute-nodes"></a>Будет ли отличаться размер tempdb основного и дополнительных вычислительных узлов

№ Ваш `tempdb` настроен на основании подготовки размер вычислений, вторичный вычислительные узлы имеют одинаковый размер как основной вычислений.

### <a name="can-i-add-indexes-and-views-on-my-secondary-compute-nodes"></a>Можно ли добавлять индексы и представления на вторичных вычислительных узлах

№ Базы данных уровня "Гипермасштабирование" имеют общее хранилище, что означает, что все вычислительные узлы видят одни и те же таблицы, индексы и представления. Если вам нужны дополнительные индексы, оптимизированные для чтения на вторичном узле, сначала их нужно добавить в первичный.

### <a name="how-much-delay-is-there-going-to-be-between-the-primary-and-secondary-compute-node"></a>Какой будет задержка между первичным и вторичным вычислительными узлами

С момента фиксации транзакции на первичном узле в зависимости от скорости создания журнала задержка может отсутствовать или быть миллисекундной.

## <a name="next-steps"></a>Дальнейшие действия

Дополнительные сведения о уровня Гипермасштабируемые службы, см. в разделе [уровня службы Гипермасштабируемого](sql-database-service-tier-hyperscale.md).
