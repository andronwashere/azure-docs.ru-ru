---
title: Масштабирование ресурсов эластичного пула — база данных SQL Azure | Документы Майкрософт
description: На этой странице описано масштабирование ресурсов для эластичных пулов в Базе данных SQL Azure.
services: sql-database
ms.service: sql-database
ms.subservice: elastic-pools
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: oslake
ms.author: moslake
ms.reviewer: carlrab
manager: craigg
ms.date: 3/14/2019
ms.openlocfilehash: f73fc58abfa6cde4133bd56858b7f26bf0c3d4a3
ms.sourcegitcommit: b7a44709a0f82974578126f25abee27399f0887f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/18/2019
ms.locfileid: "67204817"
---
# <a name="scale-elastic-pool-resources-in-azure-sql-database"></a>Масштабирование ресурсов эластичного пула в Базе данных SQL Azure

В этой статье описано масштабирование вычислительных ресурсов и ресурсов хранилища для эластичных пулов и баз данных в пуле в базе данных SQL Azure.

## <a name="change-compute-resources-vcores-or-dtus"></a>Для изменения вычислительных ресурсов (виртуальных ядер или Dtu)

Изначально выбрав количество виртуальных ядер или Edtu, можно масштабировать пул эластичных баз данных вверх или вниз динамически на основе фактического использования с помощью [портала Azure](sql-database-elastic-pool-manage.md#azure-portal-manage-elastic-pools-and-pooled-databases), [PowerShell](/powershell/module/az.sql/Get-AzSqlElasticPool), [Azure CLI ](/cli/azure/sql/elastic-pool#az-sql-elastic-pool-update), или [REST API](https://docs.microsoft.com/rest/api/sql/elasticpools/update).

### <a name="impact-of-changing-service-tier-or-rescaling-compute-size"></a>Связанные с изменением размера службы уровня или при изменении масштаба вычислений

Изменение службы уровня или вычисляет размер эластичного пула следует той же модели, что и для отдельных баз данных и обычно включает в себя службу, выполнив следующие действия:

1. Создайте новый экземпляр вычислений для эластичного пула  

    Новый экземпляр вычислений для эластичного пула создается с уровня запрошенную службу и объем вычислений. Для некоторых комбинаций уровень служб и изменение размеров вычислительных копию каждой базы данных должны создаваться в новом экземпляре вычислений, который включает в себя копирование данных и строго могут повлиять на общую задержку. В любом случае базы данных оставаться в сети на этом этапе и подключений по-прежнему должны направляться в базы данных на исходном экземпляре вычислений.

2. Переключение Маршрутизация подключения к новой вычислительный экземпляр

    Существующие подключения к базам данных на исходном экземпляре вычислений, удаляются. Все новые подключения к базам данных в новом экземпляре вычислений. Для некоторых комбинаций уровень служб и изменение размеров вычислительных файлы базы данных отсоединить и присоединить повторно во время переключения.  В любом случае коммутатор может привести к краткому перерыву при базы данных недоступны, обычно для менее чем за 30 секунд и часто для всего несколько секунд. Если есть длительные транзакции, выполняемые при подключения прерываются, продолжительность этого шага может занять больше времени для восстановления прерванных транзакций. [Ускорение восстановления базы данных](sql-database-accelerated-database-recovery.md) могут уменьшить воздействие из прерывания длительных транзакций.

> [!IMPORTANT]
> Данные не будут утеряны при какой-либо шаг в рабочем процессе.

### <a name="latency-of-changing-service-tier-or-rescaling-compute-size"></a>Задержка изменения размера службы уровня или при изменении масштаба вычислений

Предполагаемый задержки для изменения уровня обслуживания или изменения размера вычислительных отдельной базы данных или эластичного пула параметризован следующим образом:

|Уровень служб|Основные отдельной базы данных</br>Standard (S0-S1)|Основные эластичного пула</br>Стандартный (S2 – S12) </br>Гипермасштабируемый, </br>Общие цели отдельной базы данных или эластичного пула|"Премиум" или критически важный для отдельной базы данных или эластичного пула|
|:---|:---|:---|:---|
|**Основные отдельной базы данных,</br> Standard (S0-S1)**|&bull; &nbsp;Время задержки независимых используемого дискового пространства</br>&bull; &nbsp;Как правило, меньше 5 минут|&bull; &nbsp;Задержка пропорционально занятое пространство базы данных из-за копирование данных</br>&bull; &nbsp;Как правило, меньше 1 мин на ГБ используемого дискового пространства|&bull; &nbsp;Задержка пропорционально занятое пространство базы данных из-за копирование данных</br>&bull; &nbsp;Как правило, меньше 1 мин на ГБ используемого дискового пространства|
|**Основные эластичного пула, </br>Standard (S2 – S12) </br>Гипермасштабируемого, </br>общего назначения одной базы данных или эластичного пула**|&bull; &nbsp;Задержка пропорционально занятое пространство базы данных из-за копирование данных</br>&bull; &nbsp;Как правило, меньше 1 мин на ГБ используемого дискового пространства|&bull; &nbsp;Время задержки независимых используемого дискового пространства</br>&bull; &nbsp;Как правило, меньше 5 минут|&bull; &nbsp;Задержка пропорционально занятое пространство базы данных из-за копирование данных</br>&bull; &nbsp;Как правило, меньше 1 мин на ГБ используемого дискового пространства|
|**"Премиум" или критически важный для отдельной базы данных или эластичного пула**|&bull; &nbsp;Задержка пропорционально занятое пространство базы данных из-за копирование данных</br>&bull; &nbsp;Как правило, меньше 1 мин на ГБ используемого дискового пространства|&bull; &nbsp;Задержка пропорционально занятое пространство базы данных из-за копирование данных</br>&bull; &nbsp;Как правило, меньше 1 мин на ГБ используемого дискового пространства|&bull; &nbsp;Задержка пропорционально занятое пространство базы данных из-за копирование данных</br>&bull; &nbsp;Как правило, меньше 1 мин на ГБ используемого дискового пространства|

> [!NOTE]
>
> - В случае смены уровня службы или при изменении масштаба вычислений для эластичного пула, суммирование пространство, используемое для всех баз данных в пуле должен использоваться для вычисления оценки.
> - В случае перемещения базы данных из эластичного пула, только пространство, используемое базой данных влияет на задержку, не занимаемое эластичного пула.
>
> [!TIP]
> Для отслеживания выполняемых операций см. разделы [Управление операциями с помощью SQL REST API](https://docs.microsoft.com/rest/api/sql/operations/list), [Управление операциями с помощью интерфейса командной строки](/cli/azure/sql/db/op), [Отслеживание операций с помощью T-SQL](/sql/relational-databases/system-dynamic-management-views/sys-dm-operation-status-azure-sql-database) и следующие две команды PowerShell: [Get-AzSqlDatabaseActivity](/powershell/module/az.sql/get-azsqldatabaseactivity) и [Stop AzSqlDatabaseActivity](/powershell/module/az.sql/stop-azsqldatabaseactivity).

### <a name="additional-considerations-when-changing-service-tier-or-rescaling-compute-size"></a>Дополнительные соображения при изменении службы уровня или при изменении масштаба объем вычислений

- При уменьшении числа виртуальных ядер или Edtu для эластичного пула, пула используемое пространство должно быть меньше, чем максимальный размер целевой службы и уровня производительности.
- При масштабировании виртуальных ядер или Edtu для пула эластичных БД, взимается дополнительная плата, если (1) максимальный размер хранилища пула поддерживается целевым пулом и (2 превышает объем включенного хранилища целевого пула. Например, если пул категории "Стандартный" с максимальным размером в 100 ГБ, содержащий 100 eDTU, уменьшается до пула категории "Стандартный" с максимальным размером в 100 ГБ, содержащего 50 eDTU, то взимается плата за дополнительное хранилище, так как целевой пул поддерживает максимальный размер в 100 ГБ, а его включенный объем хранилища составляет только 50 ГБ. Поэтому дополнительный объем хранилища равен 100 – 50 = 50 ГБ. Чтобы узнать о ценах на дополнительное хранилище, ознакомьтесь с [ценами на Базу данных SQL](https://azure.microsoft.com/pricing/details/sql-database/). Если фактический объем пространства, которое используется, меньше, чем включенный объем хранилища, то этих дополнительных затрат можно избежать, уменьшив максимальный размер базы данных до включенного объема.

### <a name="billing-during-rescaling"></a>Выставление счетов при масштабировании

Плата взимается за каждый час существования базы данных с учетом самого высокого уровня служб и объема вычислительных ресурсов, которые использовались в течение этого часа, даже если база данных использовалась или была активна менее часа. Например, если вы создадите отдельную базу данных и через 5 минут удалите ее, вам будет выставлен счет за 1 час использования базы данных.

## <a name="change-elastic-pool-storage-size"></a>изменение размера хранилища эластичного пула

> [!IMPORTANT]
> Иногда требуется сжать базу данных, чтобы освободить неиспользуемое пространство. Дополнительные сведения см. в статье об [управлении файловым пространством в Базе данных SQL Azure](sql-database-file-space-management.md).

### <a name="vcore-based-purchasing-model"></a>Модель приобретения на основе виртуальных ядер

- Объем подготавливаемого хранилища не может превышать максимального допустимого размера.

  - Для хранения в хранилище уровня служб "Стандартный" или "Общего назначения" можно увеличить или уменьшить размер с шагом приращения в 10 ГБ.
  - Для хранения в хранилище уровня служб "Премиум" "Критически важный для бизнеса" можно увеличить или уменьшить размер с шагом приращения в 250 ГБ.
- Для подготовки хранилища эластичного пула можно увеличить или уменьшить его максимальный размер.
- Стоимость хранилища для эластичного пула равна объему хранилища, умноженному на цену единицы хранения этого хранилища для уровня служб. Сведения о цене на дополнительное хранилище см. на [странице цен на Базу данных SQL](https://azure.microsoft.com/pricing/details/sql-database/).

> [!IMPORTANT]
> Иногда требуется сжать базу данных, чтобы освободить неиспользуемое пространство. Дополнительные сведения см. в статье об [управлении файловым пространством в Базе данных SQL Azure](sql-database-file-space-management.md).

### <a name="dtu-based-purchasing-model"></a>Модель приобретения на основе единиц DTU

- Цена DTU для эластичного пула включает в себя определенный объем хранилища, не требующий дополнительной платы. Дополнительный объем хранилища, сверх включенного, можно подготовить за дополнительную плату в пределах максимального допустимого размера с шагом в 250 ГБ при объеме хранилища до 1 ТБ и с шагом в 256 ГБ — при объеме более 1 ТБ. Сведения о включенном объеме хранилища и ограничениях максимального размера см. в разделе [Отдельная база данных: размеры хранилища и объемы вычислительных ресурсов](sql-database-dtu-resource-limits-elastic-pools.md#elastic-pool-storage-sizes-and-compute-sizes).
- Дополнительное хранилище для эластичного пула можно подготовить, увеличив его максимальный размер с помощью [портала Azure](sql-database-elastic-pool-manage.md#azure-portal-manage-elastic-pools-and-pooled-databases), [PowerShell](/powershell/module/az.sql/Get-AzSqlElasticPool), [Azure CLI](/cli/azure/sql/elastic-pool#az-sql-elastic-pool-update) или [REST API](https://docs.microsoft.com/rest/api/sql/elasticpools/update).
- Стоимость дополнительного хранилища для эластичного пула равна объему хранилища, умноженному на цену единицы хранения этого хранилища для уровня служб. Сведения о цене на дополнительное хранилище см. на [странице цен на Базу данных SQL](https://azure.microsoft.com/pricing/details/sql-database/).

> [!IMPORTANT]
> Иногда требуется сжать базу данных, чтобы освободить неиспользуемое пространство. Дополнительные сведения см. в статье об [управлении файловым пространством в Базе данных SQL Azure](sql-database-file-space-management.md).

## <a name="next-steps"></a>Дальнейшие действия

Сведения об общих ограничениях ресурсов см. в статьях [Ограничения ресурсов базы данных SQL в модели приобретения на основе виртуальных ядер — эластичные пулы](sql-database-vcore-resource-limits-elastic-pools.md) и [Ограничения ресурсов базы данных SQL в модели приобретения на основе единиц DTU — эластичные пулы](sql-database-dtu-resource-limits-elastic-pools.md).
