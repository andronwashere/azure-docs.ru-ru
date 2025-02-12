---
title: Прекращение использования уровня служб "Премиум RS" для Базы данных SQL Azure | Документация Майкрософт
description: Использование уровня служб "Премиум RS" прекращается, для продолжения его поддержки см. раздел "Параметры миграции".
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
ms.date: 02/07/2019
ms.openlocfilehash: 7f184178343f69f522148777752c51afc5c5dcb6
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "65790404"
---
# <a name="azure-sql-database-premium-rs-service-tier-preview-is-being-retired---options-for-migration"></a>Прекращение использования уровня служб "Премиум RS" для Базы данных SQL Azure (предварительная версия). Параметры миграции

В феврале 2018 года, корпорация Майкрософт объявила, что уровень служб "Премиум RS" для Базы данных SQL Azure не будет больше выпускаться в общедоступной версии и не будет поддерживаться после 31 января 2019 года. Окончание срока поддержки было продлено до 30 июня 2019 года. В этой статье рассматриваются варианты перехода с уровня служб "Премиум RS" на другой уровень. После 30 июня 2019 года корпорация автоматически перенесет ваши базы данных уровня "Премиум RS" на общедоступный уровень служб, который наиболее точно соответствует требованиям по производительности вашей базы данных.

Ниже приведены подходящие варианты миграций и ценообразования для клиентов, которые используют уровень "Премиум RS".

- Уровни служб виртуальных ядер

  Уровни служб **Общее назначение** и **Критически важный для бизнеса** в [модели приобретения на основе виртуальных ядер](sql-database-service-tiers-vcore.md). Эти два уровня служб являются общедоступными. Модель приобретения на основе виртуальных ядер также предлагает **Гипермасштабируемого** уровень служб, который адаптируется по требованию для потребностей рабочей нагрузки с помощью автоматического масштабирования до 100 ТБ на одну базу данных. Уровень служб "Гипермасштабирование" обеспечивает производительность ввода-вывода, сопоставимую с уровнем служб "Премиум" в [ модели приобретения на основе единицы передачи данных](sql-database-service-tiers-dtu.md), по цене, более близкой к уровню служб "Премиум RS".
- Цены на разработку и тестирование

  [Цены на разработку и тестирование](https://azure.microsoft.com/pricing/dev-test/) обеспечивает экономию до 55 % и лицензию, которая входит в вашу подписку Visual Studio.
- Преимущество гибридного использования Azure и цены на резервную мощность

  [Преимущество гибридного использования Azure и цены на резервную мощность](https://azure.microsoft.com/pricing/details/sql-database/) обеспечивает экономию до 80 % в отличие от тарифов в лицензии. Дополнительные сведения об этих параметрах см. в разделе [Преимущество гибридного использования Azure для SQL Server](https://azure.microsoft.com/pricing/hybrid-benefit/) и [Резервная мощность Базы данных SQL Azure](sql-database-reserved-capacity.md).

## <a name="act-now-to-migrate-your-premium-rs-databases-to-alternative-sql-database-service-tiers"></a>Перенесите свои базы данных уровня "Премиум RS" на альтернативные уровни служб баз данных SQL

Просмотрите рекомендации в этой статье, а также цены и документации для определения оптимальных вариантов миграции для ваших рабочих нагрузок уровня "Премиум RS".

## <a name="migrate-compute-intensive-workloads-and-save"></a>Миграция и хранение рабочих нагрузок для ресурсоемких вычислений

Для рабочих нагрузок для ресурсоемких вычислений уровня "Премиум RS" мы рекомендуем перейти на уровень служб общего назначения на основе числа виртуальных ядер и больше сэкономить в сравнении с тарифами лицензии с использованием предложений преимущества гибридного использования Azure для SQL Server и резервной мощности. Если вы предпочитаете остановится на варианте приобретения на основе единиц передачи данных, можно перенести базы данных уровня служб "Премиум RS" ресурсоемких вычислений на уровень "Стандартный" и по-прежнему сэкономить на цене общедоступного уровня служб "Премиум RS" (если он перешел в общедоступную версию).

> [!WARNING]
> Миграция рабочих нагрузок базы данных уровня служб "Премиум RS" на уровень служб на основе единиц передачи данных уровня "Премиум" может увеличить ежемесячные затраты и текущие цены на уровень"Премиум RS". Рекомендуем рассматривать уровни "Гипермасштабирование"или" Критически важный для бизнеса" с помощью преимущества гибридного использования Azure и цены на резервную мощность для поддержания подобных или уровней, дешевле чем "Премиум RS".

### <a name="premium-rs-databases"></a>Базы данных уровня "Премиум RS"

|**Если вы в данный момент...**|**Перенос на основе виртуальных ядер для сравнимых...**|**Перенос на основе единиц передачи данных для сравнимых...**|
|---|---|---|
|Уровень "Премиум RS 1"|Общее назначение, 1 виртуальное ядро (поколение 4)|Уровень "Стандартный 3"|
|Уровень "Премиум RS 2"|Общее назначение, 2 виртуальных ядра (поколение 4)|Уровень "Стандартный 4"|
|Уровень "Премиум RS 4"|Общее назначение 4 виртуальных ядер (поколение 4)|Уровень "Стандартный 6"|
|Уровень "Премиум RS 6"|Общее назначение 6 виртуальных ядер (поколение 4)|Уровень "Стандартный 7"|

### <a name="premium-rs-pools"></a>Пулы уровня "Премиум RS"

|**Если вы в данный момент...**|**Перенос на основе виртуальных ядер для сравнимых...**|**Перенос на основе единиц передачи данных для сравнимых...**|
|---|---|---|
|125 единиц передачи данных пула уровня "Премиум RS"|Общее назначение, 1 виртуальное ядро (поколение 4)|100 единиц передачи данных пула уровня "Стандартный"|
|250 единиц передачи данных пула уровня "Премиум RS"|Общее назначение, 2 виртуальных ядра (поколение 4)|250 единиц передачи данных пула уровня "Стандартный"|
|500 единиц передачи данных пула уровня "Премиум RS"|Общее назначение 4 виртуальных ядер (поколение 4)|500 единиц передачи данных пула уровня "Стандартный"|
|1000 единиц передачи данных пула уровня "Премиум RS"|Общее назначение 8 виртуальных ядер (поколение 4)|1000 единиц передачи данных пула уровня "Стандартный"|

## <a name="optimize-savings-and-performance-for-your-io-intensive-workloads"></a>Оптимизация экономии и производительности для ваших рабочих нагрузок с интенсивным использованием ввода-вывода

Для оптимального сочетания производительности и стоимости мы рекомендуем перенести одиночные базы данных с интенсивным вводом-выводом на уровень "Гипермасштабирование" на основе виртуального ядра, который в настоящее время находится в предварительной версии, и пулы баз данных с интенсивным вводом-выводом на общедоступный уровень "Критически важный для бизнеса".  Следующие параметры на основе виртуального ядра поддержат или улучшат текущую производительность и могут сэкономить ваши деньги в сочетании с преимуществом гибридного использования Azure и ценой резервной мощности.

|**Если вы в данный момент...**|**Перенос на основе виртуальных ядер для сравнимых...**|**Перенос на основе единиц передачи данных для сравнимых...**|
|---|---|---|
|Уровень "Премиум RS 1"| Виртуальное 1 Гипермасштабируемого (4-го поколения) или Business Critical 1 виртуальное ядро (4-го поколения)|Уровень "Премиум 1"|
|Уровень "Премиум RS 2"| Гипермасштабируемый 2 виртуальных ядра (4-го поколения) или виртуальных ядер 2 критические для бизнеса (4-го поколения|Уровень "Премиум 2"|
|Уровень "Премиум RS 4"| Гипермасштабируемый 4 виртуальных ядра (4-го поколения) или виртуальных ядер 4 критические для бизнеса (4-го поколения)|Уровень "Премиум 4"
|Уровень "Премиум RS 6"| Количество виртуальных ядер Гипермасштабируемого 6 (4-го поколения) или виртуальных ядер 6 критические для бизнеса (4-го поколения)|Уровень "Премиум 6"|

|**Если вы в данный момент...**|**Перенос на основе виртуальных ядер для сравнимых...**|**Перенос на основе единиц передачи данных для сравнимых...**|
|---|---|---|
|125 единиц передачи данных пула уровня "Премиум RS"|"Критически важный для бизнеса на основе 2 виртуальных ядер (поколение 4)"|125 единиц передачи данных пула уровня "Премиум"|
|250 единиц передачи данных пула уровня "Премиум RS"|"Критически важный для бизнеса на основе 2 виртуальных ядер (поколение 4)"|250 единиц передачи данных пула уровня "Премиум"|
|500 единиц передачи данных пула уровня "Премиум RS"|"Критически важный для бизнеса на основе 4 виртуальных ядер (поколение 4)"|500 единиц передачи данных пула уровня "Премиум"|
|1000 единиц передачи данных пула уровня "Премиум RS"|"Критически важный для бизнеса на основе 8 виртуальных ядер" (поколение 4)"|1000 единиц передачи данных пула уровня "Премиум"|

## <a name="take-advantage-of-our-new-offers"></a>Преимуществами наших новых предложений

Наши уровни служб в рамках модели приобретения на основе виртуальных ядер, предусматривают специальные предложения, которые помогут вам сэкономить до 80 % от цены, включенной в лицензию. Используйте лицензии SQL Server Standard или Enterprise Edition с активным Software Assurance, чтобы сэкономить до 55 % по сравнению с ценами, включенными в лицензию, с [Преимуществом гибридного использования Azure для SQL Server](https://azure.microsoft.com/pricing/hybrid-benefit/). Вы можете объединить Преимущество гибридного использования с ценой на [резервную мощность Базы данных SQL Azure](sql-database-reserved-capacity.md) и сэкономить до 80 % при внесении предоплаты на один или три года.  Активируйте преимущества для этих вариантов на портале Azure уже сегодня.

Если у вас возникли вопросы или трудности, связанные с этими изменениями, или если требуется помощь при миграции, свяжитесь с [Корпорацией Майкрософт](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview).

## <a name="migration-from-a-premium-rs-service-tier-to-a-service-tier-in-either-the-dtu-or-the-vcore-model"></a>Модель миграции с уровня служб "Премиум RS" на уровень служб на основе единиц передачи данных или виртуальных ядер

### <a name="migration-of-a-database"></a>Миграция базы данных

Миграция базы данных из уровня служб "Премиум RS" на уровень служб на основе единиц передачи данных или виртуальных ядер аналогичен обновлению или понижению уровня обслуживания между уровнями на уровне служб "Премиум RS".

### <a name="using-database-copy-to-convert-a-premium-rs-database-to-a-dtu-based-or-vcore-based-database"></a>Использование копии базы данных для перевода базы данных с уровня служб "Премиум RS" на модель на основе единиц передачи данных

Вы можете скопировать любую базу данных с объемом вычислительных ресурсов уровня служб "Премиум RS" в базу данных на основе единиц передачи данных с объемом вычислительных ресурсов на основе виртуальных ядер без ограничений или соблюдения специальной последовательности, если целевой объем вычислительных ресурсов поддерживает максимальный размер базы данных-источника. Это связано с тем, что копия базы данных создает моментальный снимок данных на момент начала операции копирования и не выполняет синхронизацию данных между источником и целевым объектом.

## <a name="next-steps"></a>Дальнейшие действия

- Дополнительные сведения о конкретных объемах вычислительных ресурсов и доступных размерах хранилища для отдельной базы данных см. в статье [Ограничения ресурсов для отдельной базы данных в Базе данных SQL Azure при использовании модели приобретения на основе виртуальных ядер](sql-database-vcore-resource-limits-single-databases.md)
- Дополнительные сведения о конкретных объемах вычислительных ресурсов и доступных размерах хранилища для эластичных пулов см. в статье [Ограничения для эластичных пулов в службе "База данных SQL Azure" в модели приобретения на основе виртуальных ядер](sql-database-vcore-resource-limits-elastic-pools.md).
