---
title: Автомасштабирование и Среда службы приложений Azure версии 1
description: Автоматическое масштабирование и среда службы приложений
services: app-service
documentationcenter: ''
author: btardif
manager: erikre
editor: ''
ms.assetid: c23af2d8-d370-4b1f-9b3e-8782321ddccb
ms.service: app-service
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: ccompy
ms.custom: seodec18
ms.openlocfilehash: 6660aa4e21aa36dc94c4ed9201fecb5637dddb3a
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "65955966"
---
# <a name="autoscaling-and-app-service-environment-v1"></a>Автомасштабирование и среда службы приложений версии 1

> [!NOTE]
> Эта статья посвящена среде службы приложений версии 1.  Имеется более новая версия среды службы приложений, которая проще в использовании и которая работает на более мощной инфраструктуре. Чтобы узнать больше о новой версии, начните с изучения статьи [Введение в среду службы приложения](intro.md).
> 

Среды службы приложений Azure поддерживают *автоматическое масштабирование*. Вы можете настроить автоматическое масштабирование отдельных рабочих пулов на основе метрик или расписаний.

![Параметры автоматического масштабирования для рабочего пула.][intro]

Автоматическое масштабирование оптимизирует использование ресурсов, без вашего участия увеличивая и уменьшая среду службы приложений с учетом бюджета и (или) профиля нагрузки.

## <a name="configure-worker-pool-autoscale"></a>Настройка автоматического масштабирования рабочего пула
Управлять автоматическим масштабированием можно на вкладке **Параметры** для рабочего пула.

![Вкладка "Параметры" для рабочего пула.][settings-scale]

Этот интерфейс должен быть вам уже знаком — точно такой же используется при масштабировании плана службы приложений. 

![Параметры масштабирования вручную.][scale-manual]

Можно также настроить профиль автомасштабирования.

![Параметры автомасштабирования.][scale-profile]

Профили автомасштабирования используются для настройки ограничений масштаба. Вы обеспечите согласованную производительность, установив значение нижней границы масштаба (1), а также прогнозируемый лимит затрат, установив значение верхней границы (2).

![Параметры масштабирования в профиле.][scale-profile2]

Определив профиль, вы можете добавить правила автоматического масштабирования для увеличения или уменьшения числа экземпляров в рабочем пуле в пределах границ, определяемых профилем. Правила автоматического масштабирования основаны на метриках.

![Правило масштабирования.][scale-rule]

 Чтобы определить правила автомасштабирования, можно использовать любые метрики рабочего пула или внешнего интерфейса. Это те же метрики, которые можно отслеживать на графиках в колонке ресурсов и для которых можно настраивать оповещения.

## <a name="autoscale-example"></a>Пример автоматического масштабирования
Автомасштабирование среды службы приложений лучше всего проиллюстрировать пошаговым сценарием.

В этой статье приведены все рекомендации, которые необходимо учитывать при настройке автомасштабирования. В ней рассматриваются все действия, которые выполняются при планировании автомасштабирования сред службы приложений, размещенных в среде службы приложений.

### <a name="scenario-introduction"></a>Общие сведения о сценарии
Федор — системный администратор предприятия, который перенес часть рабочих нагрузок, которые можно решить для среды службы приложений.

Для этой среды службы приложений масштабирование вручную настроено так:

* **Внешние интерфейсы**: 3
* **Рабочий пул 1**: 10
* **Рабочий пул 2**: 5
* **Рабочий пул 3**: 5

Рабочий пул 1 используется для производственных рабочих нагрузок, а рабочий пул 2 и рабочий пул 3 — для задач контроля качества и разработки.

Планы службы приложений для контроля качества и разработки настроены на ручное масштабирование. Для производственного плана службы приложений настроено автомасштабирование. Это помогает справляться с колебаниями нагрузки и трафика.

Федор хорошо знаком с приложением. Они знают, что Пиковая нагрузка являются между 9:00 до 18:00:00, так как это бизнес приложения (LOB) используется сотрудниками, если они находятся в офисе. В конце рабочего дня показатели использования падают. В этот период сохраняется некоторая рабочая нагрузка, так как пользователи могут использовать приложение удаленно с мобильных устройств или домашних компьютеров. Производственный план службы приложений уже настроен на автоматическое масштабирование в соответствии с загрузкой ЦП и на основе следующих правил:

![Параметры бизнес-приложения.][asp-scale]

| **Профиль автоматического масштабирования — рабочие дни — план службы приложений** | **Профиль автоматического масштабирования — выходные дни — план службы приложений** |
| --- | --- |
| **Имя:** профиль рабочего дня |**Имя:** профиль выходного дня |
| **Масштабирование:** расписание и правила производительности |**Масштабирование:** расписание и правила производительности |
| **Профиль:** По рабочим дням |**Профиль:** выходные |
| **Тип:** Периодичность |**Тип:** Периодичность |
| **Целевой диапазон:** 5–20 экземпляров |**Целевой диапазон:** 3–10 экземпляров |
| **Дни:** понедельник, вторник, среда, четверг, пятница |**Дни:** суббота, воскресенье |
| **Время начала:** 9:00 |**Время начала:** 9:00 |
| **Часовой пояс:** UTC-08 |**Часовой пояс:** UTC-08 |
|  | |
| **Правило автоматического масштабирования (увеличение масштаба)** |**Правило автоматического масштабирования (увеличение масштаба)** |
| **Ресурс:** производство (Среда службы приложений) |**Ресурс:** производство (Среда службы приложений) |
| **Метрика:** % загрузки ЦП |**Метрика:** % загрузки ЦП |
| **Операция:** больше чем 60 % |**Операция:** больше чем 80 % |
| **Длительность:** 5 мин |**Длительность:** 10 минут |
| **Агрегат времени:** Средние |**Агрегат времени:** Средние |
| **Действие:** увеличить счетчик на 2 |**Действие:** увеличить счетчик на 1 |
| **Охлаждение (в минутах):** 15 |**Охлаждение (в минутах):** 20 |
|  | |
| **Правило автоматического масштабирования (уменьшение масштаба)** |**Правило автоматического масштабирования (уменьшение масштаба)** |
| **Ресурс:** производство (Среда службы приложений) |**Ресурс:** производство (Среда службы приложений) |
| **Метрика:** % загрузки ЦП |**Метрика:** % загрузки ЦП |
| **Операция:** меньше чем 30 % |**Операция:** меньше чем 20 % |
| **Длительность:** 10 минут |**Длительность:** 15 минут |
| **Агрегат времени:** Средние |**Агрегат времени:** Средние |
| **Действие:** уменьшить счетчик на 1 |**Действие:** уменьшить счетчик на 1 |
| **Охлаждение (в минутах):** 20 |**Охлаждение (в минутах):** 10 |

### <a name="app-service-plan-inflation-rate"></a>Скорость роста плана службы приложений
Автоматически масштабирующиеся планы службы приложений масштабируются с определенной максимальной скоростью. Эту скорость можно рассчитать на основе значений, указанных в правиле автоматического масштабирования.

Чтобы успешно применять автомасштабирование среды службы приложений, важно разбираться в значениях *скорости роста плана службы приложений* , а также уметь их рассчитывать, так как изменения масштаба рабочего пула вступают в силу не мгновенно.

Скорость роста плана службы приложений рассчитывается так:

![Расчет скорости роста плана службы приложений.][ASP-Inflation]

Для правила "Автоматическое масштабирование — увеличение масштаба" в профиле "Рабочие дни" производственного плана службы приложений расчет выглядит так:

![Скорость роста плана службы приложений для рабочих дней на основе правила "Автомасштабирование — увеличение масштаба".][Equation1]

Для правила "Автоматическое масштабирование — увеличение масштаба" в профиле "Выходные дни" производственного плана службы приложений эта формула выглядит так:

![Скорость роста плана службы приложений для выходных дней на основе правила "Автомасштабирование — увеличение масштаба".][Equation2]

Это значение можно вычислить и для операций уменьшения масштаба.

Для правила "Автоматическое масштабирование — уменьшение масштаба" в профиле "Рабочие дни" производственного плана службы приложений расчет выглядит так:

![Скорость роста плана службы приложений для рабочих дней на основе правила "Автомасштабирование — уменьшение масштаба".][Equation3]

Для правила "Автоматическое масштабирование — уменьшение масштаба" в профиле "Выходные дни" производственного плана службы приложений эта формула выглядит так:  

![Скорость роста плана службы приложений для выходных дней на основе правила "Автомасштабирование — уменьшение масштаба".][Equation4]

Производственный план службы приложений может увеличиваться со скоростью восемь экземпляров в час в рабочие дни и четыре экземпляра в час в выходные дни. Экземпляры могут высвобождаться с максимальной скоростью четыре экземпляра в час в рабочие дни и шесть экземпляров в час в выходные дни.

Если в рабочем пуле размещено несколько планов службы приложений, вам нужно рассчитать *общую скорость роста* , то есть сумму скоростей роста для всех планов службы приложений, размещенных в этом рабочем пуле.

![Расчет общей скорости роста по нескольким планам службы приложений, размещенным в рабочем пуле.][ASP-Total-Inflation]

### <a name="use-the-app-service-plan-inflation-rate-to-define-worker-pool-autoscale-rules"></a>Использование показателей скорости роста плана службы приложений для определения правил автоматического масштабирования рабочего пула
Для рабочих пулов, в которых размещены автоматически масштабирующиеся планы службы приложений, следует выделить резервную емкость (буфер). Этот буфер нужен для автоматического увеличения или уменьшения плана службы приложений по мере необходимости. Минимальный размер буфера рассчитывается с учетом общей скорости роста плана службы приложений.

Так как масштабирование среды службы приложений выполняется некоторое время, при любом изменении нужно учитывать дальнейшие колебания спроса, которые могут произойти в ходе операции. Чтобы включить в расчет эту задержку, мы рекомендуем использовать расчетную общую скорость роста плана службы приложений как минимальное число экземпляров, добавляемых для каждой операции автоматического масштабирования.

Эти сведения помогут Федору определить профиль и правила автоматического масштабирования:

![Правила профиля автомасштабирования на примере бизнес-приложения.][Worker-Pool-Scale]

| **Профиль автоматического масштабирования — рабочие дни** | **Профиль автоматического масштабирования — выходные дни** |
| --- | --- |
| **Имя:** профиль рабочего дня |**Имя:** профиль выходного дня |
| **Масштабирование:** расписание и правила производительности |**Масштабирование:** расписание и правила производительности |
| **Профиль:** По рабочим дням |**Профиль:** выходные |
| **Тип:** Периодичность |**Тип:** Периодичность |
| **Целевой диапазон:** 13–25 экземпляров |**Целевой диапазон:** 6–15 экземпляров |
| **Дни:** понедельник, вторник, среда, четверг, пятница |**Дни:** суббота, воскресенье |
| **Время начала:** 7:00 |**Время начала:** 9:00 |
| **Часовой пояс:** UTC-08 |**Часовой пояс:** UTC-08 |
|  | |
| **Правило автоматического масштабирования (увеличение масштаба)** |**Правило автоматического масштабирования (увеличение масштаба)** |
| **Ресурс:** рабочий пул 1 |**Ресурс:** рабочий пул 1 |
| **Метрика:** WorkersAvailable |**Метрика:** WorkersAvailable |
| **Операция:** меньше чем 8 |**Операция:** меньше чем 3 |
| **Длительность:** 20 минут |**Длительность:** 30 минут |
| **Агрегат времени:** Средние |**Агрегат времени:** Средние |
| **Действие:** увеличить счетчик на 8 |**Действие:** увеличить счетчик на 3 |
| **Охлаждение (в минутах):** 180 |**Охлаждение (в минутах):** 180 |
|  | |
| **Правило автоматического масштабирования (уменьшение масштаба)** |**Правило автоматического масштабирования (уменьшение масштаба)** |
| **Ресурс:** рабочий пул 1 |**Ресурс:** рабочий пул 1 |
| **Метрика:** WorkersAvailable |**Метрика:** WorkersAvailable |
| **Операция:** больше чем 8 |**Операция:** больше чем 3 |
| **Длительность:** 20 минут |**Длительность:** 15 минут |
| **Агрегат времени:** Средние |**Агрегат времени:** Средние |
| **Действие:** уменьшить счетчик на 2 |**Действие:** уменьшить счетчик на 3 |
| **Охлаждение (в минутах):** 120 |**Охлаждение (в минутах):** 120 |

Целевой диапазон, определяемый в профиле, вычисляется по минимальному числу экземпляров, указанных в профиле для плана службы приложений с учетом емкости буфера.

Максимальный диапазон вычисляется как сумма всех максимальных диапазонов для всех планов службы приложений, размещенных в рабочем пуле.

Значение увеличения для правил увеличения масштаба должно включать не менее одной скорости роста плана службы приложений.

Значение уменьшения можно установить в пределах от половины до одной скорости роста плана службы приложений.

### <a name="autoscale-for-front-end-pool"></a>Автоматическое масштабирование для интерфейсного пула
Правила автомасштабирования для интерфейсных пулов проще, чем правила для рабочих пулов. Важно,  
чтобы при определении продолжительности измерений и таймеров ожидания вы учитывали, что масштабирование плана службы приложений не происходит мгновенно.

Федор знает, что частота ошибок увеличивается после достижения внешним интерфейсом 80 % загрузки ЦП. Он устанавливает правило автомасштабирования для увеличения экземпляров следующим образом:

![Параметры автомасштабирования для интерфейсного пула.][Front-End-Scale]

| **Профиль автоматического масштабирования — внешние интерфейсы** |
| --- |
| **Имя:** автоматическое масштабирование — внешние интерфейсы |
| **Масштабирование:** расписание и правила производительности |
| **Профиль:** ежедневно |
| **Тип:** Периодичность |
| **Целевой диапазон:** 3–10 экземпляров |
| **Дни:** ежедневно |
| **Время начала:** 9:00 |
| **Часовой пояс:** UTC-08 |
|  |
| **Правило автоматического масштабирования (увеличение масштаба)** |
| **Ресурс:** пул внешних интерфейсов |
| **Метрика:** % загрузки ЦП |
| **Операция:** больше чем 60 % |
| **Длительность:** 20 минут |
| **Агрегат времени:** Средние |
| **Действие:** увеличить счетчик на 3 |
| **Охлаждение (в минутах):** 120 |
|  |
| **Правило автоматического масштабирования (уменьшение масштаба)** |
| **Ресурс:** рабочий пул 1 |
| **Метрика:** % загрузки ЦП |
| **Операция:** меньше чем 30 % |
| **Длительность:** 20 минут |
| **Агрегат времени:** Средние |
| **Действие:** уменьшить счетчик на 3 |
| **Охлаждение (в минутах):** 120 |

<!-- IMAGES -->
[intro]: ./media/app-service-environment-auto-scale/introduction.png
[settings-scale]: ./media/app-service-environment-auto-scale/settings-scale.png
[scale-manual]: ./media/app-service-environment-auto-scale/scale-manual.png
[scale-profile]: ./media/app-service-environment-auto-scale/scale-profile.png
[scale-profile2]: ./media/app-service-environment-auto-scale/scale-profile-2.png
[scale-rule]: ./media/app-service-environment-auto-scale/scale-rule.png
[asp-scale]: ./media/app-service-environment-auto-scale/asp-scale.png
[ASP-Inflation]: ./media/app-service-environment-auto-scale/asp-inflation-rate.png
[Equation1]: ./media/app-service-environment-auto-scale/equation1.png
[Equation2]: ./media/app-service-environment-auto-scale/equation2.png
[Equation3]: ./media/app-service-environment-auto-scale/equation3.png
[Equation4]: ./media/app-service-environment-auto-scale/equation4.png
[ASP-Total-Inflation]: ./media/app-service-environment-auto-scale/asp-total-inflation-rate.png
[Worker-Pool-Scale]: ./media/app-service-environment-auto-scale/wp-scale.png
[Front-End-Scale]: ./media/app-service-environment-auto-scale/fe-scale.png
