---
title: Анализ пользователей, сеансов и событий в Azure Application Insights | Документация Майкрософт
description: Демографический анализ пользователей веб-приложения.
services: application-insights
documentationcenter: ''
author: NumberByColors
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 01/24/2018
ms.reviewer: mbullwin
ms.pm_owner: daviste;NumberByColors
ms.author: daviste
ms.openlocfilehash: 7d378c2f72035c3584e1f5cd3c1f0fb9a5d5c2ed
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "60372301"
---
# <a name="users-sessions-and-events-analysis-in-application-insights"></a>Анализ пользователей, сеансов и событий в Application Insights

Узнайте, когда люди используют ваше веб-приложение, какие страницы им наиболее интересны, где находятся ваши пользователи и какие браузеры и операционные системы они используют. Анализируйте данные коммерческой телеметрии и телеметрии использования с помощью [Azure Application Insights](../../azure-monitor/app/app-insights-overview.md).

![Снимок экрана, где отображаются пользователи Application Insights](./media/usage-segmentation/0001-users.png)

## <a name="get-started"></a>Начало работы

Если вы еще не видите данных в колонках "Пользователи", "Сеансы" или "События" на портале Application Insights, [узнайте, как приступить к работе с инструментами для данных об использовании](usage-overview.md).

## <a name="the-users-sessions-and-events-segmentation-tool"></a>Инструменты сегментирования "Пользователи", "Сеансы" и "События"

В трех колонках данных об использовании для анализа данных телеметрии веб-приложения с трех точек зрения используется один и тот же инструмент. С помощью фильтрации и разделения данных можно обнаружить дополнительную информацию об относительном использовании различных страниц и компонентов.

* **Инструмент "Пользователи"** . Узнайте, сколько человек используют ваше приложение и его компоненты.  Пользователи подсчитываются по анонимным идентификаторам, которые хранятся в файлах cookie браузера. Отдельный пользователь, использующий разные браузеры или компьютеры, будет учтен как несколько пользователей.
* **Инструмент "Сеансы"** . Узнайте, в скольких сеансах действий пользователей участвовали определенные страницы и компоненты приложения. Сеанс начинает учитываться после получаса бездействия пользователя или непрерывного использования в течение 24 часов.
* **Инструмент "События"** . Узнайте, как часто используются определенные страницы и компоненты приложения. Просмотр страницы учитывается, когда браузер загружает страницу из приложения, если вы [инструментировали ее](../../azure-monitor/app/javascript.md). 

    Пользовательское событие представляет какое-либо отдельное событие, произошедшее в приложении. Как правило, это взаимодействие с пользователем, например нажатие кнопки, или выполнение какой-либо задачи. В приложение следует добавить код для [создания пользовательских событий](../../azure-monitor/app/api-custom-events-metrics.md#trackevent).

## <a name="querying-for-certain-users"></a>Запросы для определенных пользователей

Изучите различные группы пользователей, настроив параметры запроса в верхней части инструмента "Пользователи".

* "Показать": выберите когорту пользователей для анализа.
* "Которые использовали": выберите пользовательские события и просмотры страниц.
* "Во время": выберите диапазон времени.
* "По": выберите способ сегментирования данных — за период времени или по другому свойству, например по браузеру или городу.
* "Разделение по": выберите свойство для разделения или сегментирования данных. 
* "Добавить фильтры": уточните запрос, ограничив его определенными пользователями, сеансами или событиями на основе их свойств, таких как браузер или город. 
 
## <a name="saving-and-sharing-reports"></a>Сохранение и предоставление отчетов 
Отчеты "Пользователи" можно сохранить как частные, предназначенные только для вас, в разделе "Мои отчеты", или предоставить их всем остальным пользователям, обладающим доступом к ресурсу Application Insights, сохранив в разделе "Shared Reports" (Общие отчеты).

Чтобы предоставить общий доступ к отчету о пользователях, сеансах или событиях, нажмите кнопку **Общий доступ** на панели инструментов, а затем скопируйте ссылку.

Чтобы совместно использовать копию данных в отчете о пользователях, сеансах или событиях, нажмите кнопку **Общий доступ** на панели инструментов, затем щелкните **значок Word**, чтобы создать документ Word с данными. Или щелкните **значок Word** над основной диаграммой.

## <a name="meet-your-users"></a>Знакомство с пользователями

В разделе **Meet your users** (Знакомство с пользователями) отображаются пять примеров пользователей, отвечающих вашему текущему запросу. Учитывая и изучая поведение отдельных экземпляров помимо агрегатов, можно узнать, как люди на самом деле используют приложение.

## <a name="next-steps"></a>Дальнейшие действия

- Чтобы обеспечить оптимальное использование, начните отправлять [пользовательские события](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#trackevent) или [сведения о просмотрах страниц](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#page-views).
- Если вы уже сделали это, изучите инструменты использования, чтобы узнать, как пользователи используют службу.
    - [Воронки](usage-funnels.md)
    - [Сохранение](usage-retention.md)
    - [Средство "Маршруты пользователей"](usage-flows.md)
    - [Книги](../../azure-monitor/app/usage-workbooks.md)
    - [Добавление контекста пользователей](usage-send-user-context.md)