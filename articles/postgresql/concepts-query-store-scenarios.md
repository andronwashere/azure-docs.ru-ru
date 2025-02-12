---
title: 'Сценарии использования Store запросов в базе данных Azure для PostgreSQL: один сервер'
description: 'Этой статье описаны некоторые сценарии для Store запросов в базе данных Azure для PostgreSQL: один сервер.'
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 5/6/2019
ms.openlocfilehash: 029c595ba983d3b758568fbacaf6577014d893db
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "65067337"
---
# <a name="usage-scenarios-for-query-store"></a>Сценарии использования хранилища запросов

**Область применения** База данных Azure для PostgreSQL — один сервер 9.6 и 10

Хранилище запросов можно использовать в разнообразных сценариях, где важно отслеживать и сохранять прогнозируемую производительность рабочих нагрузок. Рассмотрим следующие примеры. 
- Определение и настройка наиболее ресурсоемких запросов 
- A/B-тестирование 
- Поддержание стабильной производительности во время обновлений 
- Определение и улучшение нерегламентированных рабочих нагрузок 

## <a name="identify-and-tune-expensive-queries"></a>Определение и настройка ресурсоемких запросов 

### <a name="identify-longest-running-queries"></a>Определение длительных запросов 
Используйте представление [Аналитика производительности запросов](concepts-query-performance-insight.md) на портале Azure, чтобы быстро выявить наиболее длительные запросы. Как правило, эти запросы расходуют значительный объем ресурсов. Оптимизация наиболее длительных вопросов может повысить производительность, освободив ресурсы для использования другими запросами, выполняемыми в вашей системе. 

### <a name="target-queries-with-performance-deltas"></a>Обращайте внимание в первую очередь на запросы с разницей производительности 
Хранилище запросов разделяет данные о производительности на временные периоды, так что вы легко можете отследить производительность запроса за определенное время. Это поможет понять, какие запросы способствуют увеличению общего потраченного времени. Следовательно, вы можете выполнять целенаправленную диагностику своей рабочей нагрузки.

### <a name="tuning-expensive-queries"></a>Настройка ресурсоемких запросов 
Обнаружив запрос с неидеальной производительностью, можно принять ряд мер в зависимости от сути проблемы: 
- Воспользуйтесь [рекомендациями по производительности](concepts-performance-recommendations.md), чтобы определить, имеются ли рекомендуемые индексы. Если да, создайте индекс, а затем используйте хранилище запросов, чтобы оценить производительность запроса после создания индекса. 
- Убедитесь, что статистика для базовых таблиц, используемых запросом, актуальна.
- Подумайте о том, чтобы переписать ресурсоемкие запросы. Например, воспользуйтесь параметризацией запросов и уменьшите использование динамического SQL. Реализуйте оптимальную логику чтения данных, например фильтруя данные на стороне базы данных, а не приложения. 


## <a name="ab-testing"></a>A/B-тестирование 
Используйте хранилище запросов для сравнения производительности рабочей нагрузки до и после внесения планируемого изменения. Примеры сценариев по использованию хранилища запросов с целью оценки влияния изменений в среде или приложении на производительность рабочей нагрузки: 
- Развертывание новой версии приложения. 
- Добавление дополнительных ресурсов на сервер. 
- Создание недостающих индексов в таблицах, на которые ссылаются ресурсоемкие запросы. 
 
В любом из этих сценариев можно применить следующий рабочий процесс: 
1. Выполните рабочую нагрузку хранилища запросов до внесения запланированного изменения, чтобы создать базовый уровень производительности. 
2. В контролируемый момент времени внесите изменения в приложение. 
3. Продолжайте выполнять рабочую нагрузку достаточно долго, чтобы получить представление о производительности системы после изменения. 
4. Сравните результаты до и после изменения. 
5. Примите решение о том, что нужно сделать: сохранить изменение или выполнить откат. 


## <a name="identify-and-improve-ad-hoc-workloads"></a>Определите и усовершенствуйте нерегламентированные рабочие нагрузки. 
В некоторых рабочих нагрузках отсутствуют доминантные запросы, которые можно настроить с целью повышения общей производительности приложения. Как правило, эти рабочие нагрузки характеризуются относительно большим числом уникальных запросов, каждый из которых потребляет часть системных ресурсов. Каждый уникальный запрос выполняется редко, поэтому по отдельности их потребление ресурсов среды выполнения некритично. С другой стороны, учитывая, что приложение постоянно создает новые запросы, значительная часть системных ресурсов расходуется на составление запросов, что не является оптимальным сценарием. Как правило, это происходит, если ваше приложение создает запросы (вместо того чтобы использовать хранимые процедуры или параметризованные запросы) либо использует платформы объектно-реляционного сопоставления, которые создают запросы по умолчанию. 
 
Если вы контролируете код приложения, возможно, целесообразно переписать уровень доступа к данным, чтобы использовать хранимые процедуры или параметризованные запросы. Улучшить ситуацию можно и без внесения изменений в приложение: нужно принудительно реализовать параметризацию запросов для всей базы данных (всех запросов) или для отдельных шаблонов запросов с одним хэшем запроса. 

## <a name="next-steps"></a>Дальнейшие действия
- Дополнительные сведения см. в разделе [Рекомендации по использованию хранилища запросов](concepts-query-store-best-practices.md)