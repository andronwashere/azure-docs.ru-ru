---
title: Высокая доступность и надежность — планировщик Azure
description: Сведения о функциях обеспечения высокой доступности и надежности в планировщике Azure
services: scheduler
ms.service: scheduler
author: derek1ee
ms.author: deli
ms.reviewer: klam
ms.assetid: 5ec78e60-a9b9-405a-91a8-f010f3872d50
ms.topic: article
ms.date: 08/16/2016
ms.openlocfilehash: 50ab6cfefe4a7df9d671e7fd1287aa16b803f260
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "64702883"
---
# <a name="high-availability-and-reliability-for-azure-scheduler"></a>Высокая доступность и надежность для планировщика Azure

> [!IMPORTANT]
> Служба [Azure Logic Apps](../logic-apps/logic-apps-overview.md) заменяет планировщик Azure, который выводится из эксплуатации. Для планирования заданий [попробуйте использовать Azure Logic Apps](../scheduler/migrate-from-scheduler-to-logic-apps.md). 

Планировщик Azure обеспечивает [высокий уровень доступности](https://docs.microsoft.com/azure/architecture/guide/pillars#availability) и надежности для заданий. Дополнительные сведения см. в статье [Соглашение об уровне обслуживания для планировщика](https://azure.microsoft.com/support/legal/sla/scheduler).

## <a name="high-availability"></a>Высокий уровень доступности

Планировщик Azure отличается высокой доступностью и использует географически избыточное развертывание служб и репликацию заданий по географическим регионам.

### <a name="geo-redundant-service-deployment"></a>Географически избыточное развертывание служб

Планировщик Azure доступен на портале Azure практически для [всех географических регионов, поддерживаемых Azure на данный момент](https://azure.microsoft.com/global-infrastructure/regions/#services). Таким образом, если центр обработки данных Azure в регионе размещения становится недоступным, вы можете по-прежнему использовать планировщик Azure, так как возможности отработки отказа службы позволяют работать с планировщиком из другого центра обработки данных.

### <a name="geo-regional-job-replication"></a>Репликация заданий по географическим регионам

Ваши задания в планировщике Azure реплицируются в регионах Azure. В случае сбоя в одном регионе планировщик Azure отрабатывает отказ и обеспечивает выполнение задания из другого центра обработки данных в связанном географическом регионе.

Например, если задание создано в центрально-южной части США, планировщик Azure обеспечивает его автоматическую репликацию в центрально-северной части США. В случае сбоя в южно-центральной части США планировщик Azure запускает задание в северо-центральной части США. 

![Репликация заданий по географическим регионам](./media/scheduler-high-availability-reliability/scheduler-high-availability-reliability-image1.png)

Планировщик Azure также гарантирует, что ваши данные остаются в пределах того же, но более широкого географического региона, если сбой происходит в Azure. Таким образом, вам не нужно дублировать задания для реализации высокой доступности. Планировщик Azure автоматически обеспечивает высокую доступность для ваших заданий.

## <a name="reliability"></a>Надежность

Доступность самого планировщика Azure поддерживается на высоком уровне, однако к созданным пользователями заданиям применяются другие методы. Предположим, что конечная точка HTTP, вызываемая вашим заданием, недоступна. Планировщик Azure по-прежнему попытается успешно выполнить задание, предоставляя альтернативные способы для обработки ошибок: 

* настройка политик повтора;
* настройка альтернативных конечных точек.

<a name="retry-policies"></a>

### <a name="retry-policies"></a>Политики повтора

Планировщик Azure позволяет настроить политики повтора. При сбое задания планировщик по умолчанию выполняет еще четыре попытки запустить это задание с интервалом 30 секунд. Вы можете задать собственное значение интенсивности, например 10 раз с интервалом 30 секунд или два раза с интервалом в один день.

Предположим, вы создаете еженедельное задание, которое вызывает конечную точку HTTP. Если конечная точка HTTP становится недоступной в течение нескольких часов во время выполнения задания, возможно, вы не захотите ждать еще одну неделю для выполнения задания (что произойдет, поскольку политика повтора по умолчанию не будет работать в этом случае). Таким образом, может потребоваться изменить стандартную политику повтора, чтобы повторные попытки выполнялись, например, каждые три часа, а не каждые 30 секунд. 

Сведения о настройке политики повтора см. в разделе [retryPolicy](scheduler-concepts-terms.md#retrypolicy).

### <a name="alternate-endpoints"></a>Альтернативные конечные точки

Если задание планировщика Azure вызывает конечную точку, которая недостижима даже после применения политики повтора, планировщик обращается к альтернативной конечной точке, которая может обработать такие ошибки. Таким образом, если вы настроили такую конечную точку, планировщик вызывает эту конечную точку, что обеспечивает высокую доступность ваших заданий в случае сбоев.

Например, на этой схеме показано, как планировщик выполняет политику повтора при вызове веб-службы в Нью-Йорке. Если повторная попытка неудачна, планировщик проверяет наличие альтернативной конечной точки. Если конечная точка существует, планировщик начинает отправлять запросы к альтернативной конечной точке. И исходное, и альтернативное действие выполняются в соответствии с одной и той же политикой повтора.

![Поведение планировщика при наличии политики повтора и альтернативной конечной точки](./media/scheduler-high-availability-reliability/scheduler-high-availability-reliability-image2.png)

Тип действия для альтернативного действия может отличаться от исходного действия. Например, несмотря на то, что исходное действие вызывает конечную точку HTTP, альтернативное действие может регистрировать ошибки с помощью очереди хранилища, очереди служебной шины или действия раздела служебной шины.

Сведения о настройке альтернативной конечной точки см. в разделе [errorAction](scheduler-concepts-terms.md#error-action).

## <a name="see-also"></a>См. также

* [Что такое планировщик Azure?](scheduler-intro.md)
* [Основные понятия, терминология и иерархия сущностей](scheduler-concepts-terms.md)
* [Создание сложных расписаний и расширенных схем повторения](scheduler-advanced-complexity.md)
* [Лимиты, квоты, значения по умолчанию и коды ошибок](scheduler-limits-defaults-errors.md)
