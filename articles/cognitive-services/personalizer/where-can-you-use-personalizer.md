---
title: Оценка сценария. Персонализатор
titleSuffix: Azure Cognitive Services
description: Персонализатор можно применять в любой ситуации, где ваше приложение может выбрать правильный элемент, действие или продукт для отображения, чтобы улучшить опыт использования, достигать лучших результатов в бизнесе или увеличить производительность.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: personalizer
ms.topic: conceptual
ms.date: 05/07/2019
ms.author: diberry
ms.openlocfilehash: 465fba3a466aceaf9ef7b71e4b1957bfdbcad766
ms.sourcegitcommit: e3b0fb00b27e6d2696acf0b73c6ba05b74efcd85
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/30/2019
ms.locfileid: "68663643"
---
# <a name="where-can-you-use-personalizer"></a>Где можно использовать Персонализатор

Используйте Персонализатор в любой ситуации, где вашему приложению необходимо выбрать правильный элемент, действие или продукт для отображения, чтобы улучшить опыт использования, достигать лучших результатов в бизнесе или увеличить производительность. 

Персонализатор использует машинное обучение, чтобы выбрать, какое действие показывать пользователю. Показываемые варианты значительно зависят от количества, качества и распределения данных, отправленных в службу.

### <a name="checklist-for-applying-personalizer"></a>Контрольный список для применения Персонализатора


Персонализатор можно применять в ситуациях, когда:

* У вас есть бизнес-цель или цель повышения удобства использования вашего приложения.
* В вашем приложении есть место, где принятие контекстуального решения о том, что показывать пользователям, поможет этой цели.
* Лучший выбор может и должен делаться на основе общего поведения пользователей и общей оценки вознаграждения.
* Использование машинного обучения для персонализации соответствует [руководствам по ответственному использованию](ethics-responsible-use.md) и вашему выбору.
* Контекстные решения можно выразить как ранжирование лучших вариантов (действий) при ограниченном наборе вариантов.
* Степень эффективности ранжированного варианта для вашего приложения можно определить, оценив некоторые аспекты поведения пользователя и выразив их в виде _оценки вознаграждения_. Это число в диапазоне от -1 до 1.
* Оценка вознаграждения не приносит слишком много смешанных или внешних факторов. Продолжительность эксперимента достаточна мала, поэтому оценку вознаграждения можно вычислить, пока она еще полезна.
* Контекст для ранга можно выразить как список по крайней мере 5 [функций](concepts-features.md), которые могут помочь сделать правильный выбор и не включают личные сведения (персональные данные).
* У вас есть информация о каждом выборе содержимого, _действия_, как списка с минимум 5 [функциями](concepts-features.md), которые могут помочь Персонализатору сделать правильный выбор.
* Приложение может хранить данные достаточно долго, чтобы накопить журнал с минимум 100 000 взаимодействий.

## <a name="machine-learning-considerations-for-applying-personalizer"></a>Рекомендации для машинного обучения по применению Персонализатора

Персонализатор основан на обучении с подкреплением, подходе к машинному обучению, который основан на обратной связи, которую вы предоставляете. 

Персонализатор будет учиться в ситуациях, в которых:

* Имеется достаточно событий для оптимальной персонализации, если проблема изменяется со временем (например, предпочтения новостей или моды). Персонализатор адаптируется к постоянным изменениям реального мира, однако результаты не будут оптимальными, если событий и данных будет недостаточно, чтобы учиться на них, а также открывать и осваивать новые шаблоны. Следует выбрать вариант использования, который применяется достаточно часто. Рассмотрите возможность поиска вариантов использования, которые попадаются по крайней мере 500 раз в день.
* В контексте и действиях достаточно [функций](concepts-features.md) для упрощения обучения.
* Не более 50 действий для ранжирования на один вызов.
* Параметры хранения данных позволяют Персонализатору собирать достаточно данных для выполнения автономных оценок и оптимизации политики. Обычно это по крайней мере 50 000 точек данных.

## <a name="monitor-effectiveness-of-personalizer"></a>Мониторинг эффективности Персонализатора

Периодически выполняя [автономные оценки](concepts-offline-evaluation.md), вы можете отслеживать эффективность Персонализатора.

## <a name="use-personalizer-with-recommendation-engines"></a>Использование Персонализатора с системами рекомендаций

Многие компании используют системы рекомендаций, маркетинговые и рекламные инструменты, сегментацию и кластеризацию аудитории, совместную фильтрацию и другие средства, чтобы рекомендовать клиентам продукцию из большого каталога.

[Репозиторий рекомендаций Майкрософт на сайте GitHub](https://github.com/Microsoft/Recommenders) содержит примеры и методики по созданию систем рекомендаций, которые представлены в виде записных книжек Jupyter. Он предоставляет рабочие примеры подготовки данных, создания моделей, оценки, настройки и ввода в эксплуатацию систем рекомендаций для многих распространенных подходов, включая xDeepFM, SAR, ALS, RBM, DKN.

Персонализатор может работать с имеющейся системой рекомендаций.

* Системы рекомендаций потребляют большое количество элементов (например, 500 000) и рекомендуют подмножество (например, 20 наиболее популярных) из сотен или тысяч вариантов.
* Персонализатор потребляет небольшое количество действий с большим количеством информации о них и ранжирует их в режиме реального времени для данного форматированного контекста, тогда как большинство систем рекомендаций используют лишь несколько атрибутов пользователей, продуктов и их взаимодействия.
* Персонализатор предназначен для постоянного автономного изучения предпочтений пользователя. Это принесет лучшие результаты в тех случаях, когда содержимое быстро изменяется, например новости, прямые трансляции событий, содержимое сообщества в реальном времени, содержимое с ежедневными обновлениями или сезонное содержимое.

Обычно используется для получения выходных данных системы рекомендаций (например, 20 наиболее популярных продуктов для определенного клиента) и использования их в качестве входного действия для Персонализатора.

## <a name="next-steps"></a>Следующие шаги

[Guidelines for responsible implementation of Personalizer](ethics-responsible-use.md) (Руководство по ответственной реализации Персонализатора).