---
title: Обработка событий с помощью Azure Stream Analytics в режиме реального времени
description: В этой статье описывается эталонная архитектура для аналитики и обработки событий с помощью Azure Stream Analytics в режиме реального времени.
services: stream-analytics
author: jseb225
ms.author: jeanb
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 01/24/2017
ms.openlocfilehash: ab1ca89427f332151120420c3c087902584706f4
ms.sourcegitcommit: 6a42dd4b746f3e6de69f7ad0107cc7ad654e39ae
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/07/2019
ms.locfileid: "67621785"
---
# <a name="reference-architecture-real-time-event-processing-with-microsoft-azure-stream-analytics"></a>Эталонная архитектура: обработка событий в режиме реального времени средствами Microsoft Azure Stream Analytics
Эталонная архитектура для обработки событий в режиме реального времени с помощью инструментов Azure Stream Analytics определяет общие принципы развертывания решения для потоковой обработки данных в реальном времени в рамках модели «платформа как услуга» (platform as a service, PaaS) на базе службы Microsoft Azure.

## <a name="summary"></a>Сводка
Традиционно в основе аналитических решений лежат такие функциональные возможности и компоненты, как извлечение, преобразование и загрузка информации, а также хранилища данных, в которые информация заносится до анализа. Изменение требований, в том числе повышение скорости поступления данных, привело к тому, что существующая модель себя исчерпала. Одним из решений является анализ данных в составе динамических потоков перед их сохранением. Эта возможность не нова, однако такой подход не получил широкого распространения во всех направлениях отрасли. 

Платформа Microsoft Azure предлагает богатый ассортимент аналитических технологий, позволяющих реализовать самые разные сценарии и решения с различными требованиями. Выбор оптимальной службы Azure для развертывания комплексного решения может оказаться непростой задачей с учетом всех возможных вариантов. В этом документе описаны возможности и принципы взаимодействия различных служб Azure, позволяющих реализовать решение для потоковой обработки событий. В нем также рассматриваются возможные сценарии эффективного использования этого подхода клиентами.

## <a name="contents"></a>Оглавление
* Аннотация
* Общие сведения об аналитике в режиме реального времени
* Ценность средств Azure для работы с данными в режиме реального времени
* Распространенные сценарии применения средств аналитики в режиме реального времени
* Архитектура и компоненты
  * Источники данных
  * Уровень интеграции данных
  * Уровень аналитики в режиме реального времени
  * Уровень хранения данных
  * Уровень представления или потребления
* Заключение

**Автор:** Чарльз Феддерсен (Charles Feddersen), архитектор решений, Научно-инновационный центр анализа данных (Data Insights Center of Excellence), корпорация Майкрософт

**Опубликовано:** январь 2015 г.

**Версия:** 1.0

**Загрузка:** [Обработка событий в режиме реального времени средствами Microsoft Azure Stream Analytics](https://download.microsoft.com/download/6/2/3/623924DE-B083-4561-9624-C1AB62B5F82B/real-time-event-processing-with-microsoft-azure-stream-analytics.pdf)

## <a name="get-help"></a>Получение справки
За дополнительной помощью обращайтесь на [форум Azure Stream Analytics](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureStreamAnalytics).

## <a name="next-steps"></a>Следующие шаги
* [Введение в Azure Stream Analytics](stream-analytics-introduction.md)
* [Приступая к работе с Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Масштабирование заданий в службе Azure Stream Analytics](stream-analytics-scale-jobs.md)
* [Справочник по языку запросов Azure Stream Analytics](https://docs.microsoft.com/stream-analytics-query/stream-analytics-query-language-reference)
* [Справочник по API-интерфейсу REST управления Stream Analytics](https://msdn.microsoft.com/library/azure/dn835031.aspx)

