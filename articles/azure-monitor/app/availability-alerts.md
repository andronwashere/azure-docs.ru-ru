---
title: Настроить оповещения доступности с помощью Azure Application Insights | Документация Майкрософт
description: Настройка веб-тестов в Application Insights. Получение оповещений, когда веб-сайт становится недоступным или медленно реагирует на запросы.
services: application-insights
documentationcenter: ''
author: lgayhardt
manager: carmonm
ms.assetid: 46dc13b4-eb2e-4142-a21c-94a156f760ee
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 06/19/2019
ms.reviewer: sdash
ms.author: lagayhar
ms.openlocfilehash: cc022f91d4b4fec42929769df8c979320548a1f9
ms.sourcegitcommit: 82efacfaffbb051ab6dc73d9fe78c74f96f549c2
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/20/2019
ms.locfileid: "67305109"
---
# <a name="availability-alerts"></a>Оповещения о доступности

[Azure Application Insights](../../azure-monitor/app/app-insights-overview.md) отправляет веб-запросы через одинаковые промежутки времени из разных точек по всему миру. Он может выдать Если приложение не отвечает, или если она отвечает слишком медленно.

## <a name="enable-alerts"></a>Включение оповещений

Оповещения автоматически включаются по умолчанию. Однако чтобы полностью настроить оповещение необходимо сначала изначально создать тест доступности.

![Интерфейс создания](./media/availability-alerts/create-test.png)

> [!NOTE]
>  В [новых унифицированных оповещениях](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-unified-alerts) настройки серьезности и уведомлений для правила генерации оповещений с помощью [группы действий](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-action-groups) **должны** настраиваться в интерфейсе оповещений. Без следующих действий вы будете получать уведомления только на портале.

1. Сохранив тест доступности, щелкните многоточие на вкладке "Сведения" для созданного теста. Выберите "Изменить оповещение".

   ![Изменить после сохранения](./media/availability-alerts/edit-alert.png)

2. Задайте требуемый уровень серьезности, описание правила и самое важное — группу действий, настройки уведомлений которой вы хотите использовать для этого правила генерации оповещений.

   ![Изменить после сохранения](./media/availability-alerts/set-action-group.png)

### <a name="alert-on-x-out-of-y-locations-reporting-failures"></a>Оповещение о том, что X из Y расположений сообщают о сбоях

Правило генерации оповещений о том, что X из Y расположений сообщают о сбоях, включено по умолчанию в [новом унифицированном интерфейсе оповещений](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-unified-alerts) при создании нового теста доступности. Вы можете отказаться, выбрав "классический" вариант или отключив правило генерации оповещений.

> [!NOTE]
> Настройте группы действий, чтобы получать уведомления при активации оповещения, выполнив описанные выше действия. Без этого шага вы будете получать при активации правила только уведомления на портале.
>

### <a name="alert-on-availability-metrics"></a>Оповещения на основе метрик доступности

С помощью [новых унифицированных оповещений](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-unified-alerts) можно создавать оповещения о доступности сегментированных агрегированных метрик доступности и длительности теста:

1. Выберите ресурс Application Insights в интерфейсе метрик и укажите метрику доступности:

    ![Выбор метрик доступности](./media/availability-alerts/select-metric.png)

2. Команда настройки оповещений в меню перенаправит вас на новый интерфейс, где можно выбрать определенные тесты или расположения, чтобы настроить правило генерации оповещений. Здесь можно также настроить группы действий для этого правила.

### <a name="alert-on-custom-analytics-queries"></a>Оповещение о настраиваемых аналитических запросах

С помощью [новых унифицированных оповещений](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-unified-alerts) вы можете создать оповещение [о настраиваемых запросах к журналу](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitor-alerts-unified-log). С помощью настраиваемых запросов вы можете создать оповещение с любым произвольным условием, которое поможет вам получить самый надежный сигнал о проблемах с доступностью. Это особенно применимо при отправке настраиваемых результатов доступности с помощью пакета TrackAvailability SDK. 

> [!Tip]
> К метрикам доступности данных относятся все настраиваемые результаты доступности, которые вы может отправлять путем вызова TrackAvailability SDK. Поддержку оповещений о метриках можно использовать для оповещения о доступности результатов.
>

## <a name="troubleshooting"></a>Устранение неполадок

Выделенные [статьи об устранении неполадок](troubleshoot-availability.md).

## <a name="next-steps"></a>Дальнейшие действия

* [Многошаговые веб-тесты](availability-multistep.md)
* [URL-адрес проверки связи веб-тестов](monitor-web-app-availability.md)

