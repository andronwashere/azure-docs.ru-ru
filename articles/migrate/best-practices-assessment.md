---
title: Рекомендации по созданию оценки с помощью Azure Migrate Server — Оценка серверов | Документация Майкрософт
description: Содержит советы по созданию оценок с оценкой сервера "миграция Azure".
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 07/15/2019
ms.author: raynew
ms.openlocfilehash: 18b82b5553f7045c38c9de532199c2a0fd815ee1
ms.sourcegitcommit: b2db98f55785ff920140f117bfc01f1177c7f7e2
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/16/2019
ms.locfileid: "68234312"
---
# <a name="best-practices-for-creating-assessments"></a>Рекомендации по созданию оценок

Служба " [Миграция Azure](migrate-overview.md) " предоставляет центр средств, помогающих обнаруживать, оценивать и переносить приложения, инфраструктуру и рабочие нагрузки в Microsoft Azure. Концентратор включает средства миграции Azure, а также предложения независимых поставщиков программного обеспечения (ISV) сторонних разработчиков.

В этой статье приводятся рекомендации по созданию оценок с помощью средства Azure Migrate Server для оценки.

## <a name="about-assessments"></a>Об оценках

Оценки, создаваемые с помощью Azure "миграция сервера", — это моментальный снимок данных на момент времени. Существует два типа оценок в службе "миграция Azure".

**Тип оценки** | **Сведения** | **Данные**
--- | --- | ---
**На основе производительности** | Оценки, которые делают рекомендации на основе собранных данных о производительности | Рекомендуемый размер виртуальной машины зависит от данных об использовании ЦП и памяти.<br/><br/> Рекомендации по типу диска ("Стандартный" жесткий диск, SSD или модуль "Премиум") основаны на операциях ввода-вывода и пропускной способности локальных дисков.
**Как есть в локальной среде** | Оценки, которые не используют данные о производительности для создания рекомендаций. | Рекомендуемый размер ВИРТУАЛЬНОЙ машины зависит от размера локальной виртуальной машины.<br/><br> Рекомендуемый тип диска зависит от того, что выбрано в параметре тип хранения для оценки.

### <a name="example"></a>Пример
Например, если у вас есть локальная виртуальная машина с четырьмя ядрами с использованием 20% и объем памяти в 8 ГБ с использованием 10%, оценки будут выглядеть следующим образом:

- **Оценка на основе производительности**:
    - Определяет действующие ядра и память на основе ядра (4 x 0,20 = 0,8) и использования памяти (8 ГБ x 0,10 = 0,8).
    - Применяет коэффициент использования, указанный в свойствах оценки (ле'тс, скажем, 1.3 x), для получения значений, которые будут использоваться для изменения размера. 
    - Рекомендует ближайший размер виртуальной машины в Azure, который может поддерживать память размером не более 1,4 ядер (0,8 x 1,3) и ~ 1,4 ГБ (0,8 x 1,3).

- **Оценка "как есть" (как и в локальной среде)** :
    -  Рекомендует виртуальную машину с четырьмя ядрами; 8 ГБ памяти.

## <a name="best-practices-for-creating-assessments"></a>Рекомендации по созданию оценок

Устройство "миграция Azure" постоянно выполняет профилирование локальной среды и отправляет метаданные и данные о производительности в Azure. Следуйте приведенным ниже рекомендациям по созданию оценок.

- **Создать оценку "как есть**": Вы можете создавать оценки "как есть" сразу же после того, как компьютеры будут отображаться на портале службы "миграция Azure".
- **Создание оценки на основе производительности**. После настройки обнаружения рекомендуется подождать по крайней мере один день, прежде чем выполнять оценку на основе производительности:
    - Сбор данных о производительности занимает некоторое время. Ожидание по крайней мере дня гарантирует наличие достаточного количества точек данных о производительности перед выполнением оценки.
    - При выполнении оценки на основе производительности убедитесь, что вы проведете Профилирование среды для оценки длительности. Например, если вы создаете оценку с длительностью производительности, равной одной неделе, необходимо подождать не менее недели после начала обнаружения, чтобы собирать все точки данных. Если этого не сделать, оценка не будет оцениваться пятью звездами.
- **Пересчет оценок**: Поскольку оценки представляют собой моментальные снимки на момент времени, они не обновляются автоматически с использованием последних данных. Чтобы обновить оценку с использованием последних данных, необходимо повторно вычислить их.

## <a name="best-practices-for-confidence-ratings"></a>Рекомендации по оценкам достоверности

При выполнении оценок на основе производительности Оценка достоверности от 1 до 5 звезд (самая высокая) выдается для оценки. Эффективное использование оценок достоверности:
- Для оценки сервера "миграция Azure" требуются данные об использовании ЦП или памяти виртуальной машины.
- Для каждого диска, подключенного к локальной виртуальной машине, требуются данные о пропускной способности для операций чтения и записи.
- Для каждого сетевого адаптера, подключенного к виртуальной машине, требуются данные сети и.

В зависимости от процента точек данных, доступных в течение выбранного времени, Оценка достоверности для оценки предоставляется, как показано в следующей таблице.

   **Доступность точки данных** | **Оценка достоверности**
   --- | ---
   0–20 % | 1 звезда
   21–40 % | 2 звезды
   41–60 % | 3 звезды
   61–80 % | 4 звезды
   81–100 % | 5 звезд


## <a name="common-assessment-issues"></a>Распространенные проблемы с оценкой

Вот как можно решить некоторые распространенные проблемы среды, влияющие на оценку.

###  <a name="out-of-sync-assessments"></a>Оценки вне синхронизации

Если после создания оценки вы добавите или удалите компьютеры из группы, то созданная вами оценка будет помечена как Несинхронизированная. Запустите оценку еще раз (**пересчитать**), чтобы отразить изменения группы.

### <a name="outdated-assessments"></a>Устаревшие оценки

При наличии локальных изменений на виртуальных машинах, которые находятся в оцениваемой группе, оценка будет помечена как устаревшая. Чтобы отразить изменения, запустите оценку еще раз.

### <a name="low-confidence-rating"></a>Низкая степень достоверности

Оценка может не иметь всех точек данных по ряду причин:

- Среда не была профилирована на период времени, для которого создается оценка. Например, если вы создаете *оценку на основе производительности* с длительностью "одна неделя", необходимо подождать минимум в неделю после начала обнаружения всех точек данных для сбора. Чтобы увидеть последнюю применимую  оценку достоверности, всегда можно щелкнуть пересчитать. Оценка достоверности применяется, только если вы создаете оценку *на основе производительности* .

- В течение периода, для которого рассчитывается оценка, несколько виртуальных машин были отключены. Если некоторые виртуальные машины были отключены в течение некоторого времени, Оценка сервера не сможет получить данные о производительности за этот период.

- Несколько виртуальных машин были созданы после начала оценки сервера. Например, вы создаете оценку для журнала производительности за последний месяц, при этом несколько виртуальных машин были созданы всего неделю назад. В этом случае данные о производительности для новых виртуальных машин будут недоступны для всей продолжительности, а Оценка достоверности будет низкой.


## <a name="next-steps"></a>Следующие шаги

- [Узнайте](concepts-assessment-calculation.md) , как рассчитываются оценки.
- [Узнайте](how-to-modify-assessment.md) , как настроить оценку.
