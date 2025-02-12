---
title: Емкость экземпляра управления API Azure | Документация Майкрософт
description: В этой статье описаны метрики емкости и способы принятия обоснованных решений относительно масштабирования экземпляра управления API Azure.
services: api-management
documentationcenter: ''
author: mikebudzynski
manager: anneta
editor: ''
ms.service: api-management
ms.workload: integration
ms.topic: article
ms.date: 06/18/2018
ms.author: apimpm
ms.openlocfilehash: c39c585d9947422260868734ec89814d8a510089
ms.sourcegitcommit: 64798b4f722623ea2bb53b374fb95e8d2b679318
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/11/2019
ms.locfileid: "67836949"
---
# <a name="capacity-of-an-azure-api-management-instance"></a>Емкость экземпляра управления API Azure

**Емкость** является наиболее важным [метрик Azure Monitor](api-management-howto-use-azure-monitor.md#view-metrics-of-your-apis) для принятия обоснованных решений нужно ли выполнять масштабирование экземпляра службы управления API в соответствии с большей нагрузки. Это сложная метрика, обязывающая следовать определенному поведению.

В этой статье объясняется, что такое **емкость** и ее поведение. Здесь показано, как получить доступ к метрикам **емкости** на портале Azure, и описаны случаи, когда следует рассмотреть возможность масштабирования или обновления экземпляра управления API.

## <a name="prerequisites"></a>предварительные требования

Чтобы выполнить шаги из этой статьи, понадобится следующее:

+ Активная подписка Azure.

    [!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

+ Экземпляр APIM. Дополнительные сведения см. в статье о [создании экземпляра управления API Azure](get-started-create-service-instance.md).

[!INCLUDE [premium-dev-standard-basic.md](../../includes/api-management-availability-premium-dev-standard-basic.md)]

## <a name="what-is-capacity"></a>Что такое емкость

![Метрики емкости](./media/api-management-capacity/capacity-ingredients.png)

**Емкость** является индикатором нагрузку на экземпляр управления API. Он отражает использование ресурсов (ЦП, памяти) и длину очереди сети. Использование ЦП и памяти показывает потребление ресурсов по:

+ Службы управления API, такие как обработка управления действия или запрос, который может включать пересылку запросов или под управлением политики
+ выбранным процессам операционной системы, включая процессы, содержащие стоимость подтверждений протокола SSL для новых подключений.

Общей **емкостью** является среднее значение всех единиц экземпляра управления API.

Несмотря на то что **метрики производительности** — предназначено для поверхности проблемы с вашего экземпляра службы управления API, бывают случаи, когда проблемы не будут отражены изменения в **метрики производительности**.

## <a name="capacity-metric-behavior"></a>Поведение метрик емкости

На практике из-за собственной конструкции на **емкость** может повлиять множество переменных, например:

+ Шаблоны подключения (новое подключение по запросу и повторное использование имеющегося подключения).
+ Размеры запроса и ответа.
+ Политики, настроенные на каждом API, или количество клиентов, отправляющих запросы.

Чем сложнее операции в запросе, тем выше **емкость** потребления. Например, сложные политики преобразования потребляют гораздо больше ресурсов ЦП, чем перенаправление простого запроса. Медленные ответы внутренней службы также приводят к повышенному потреблению емкости.

> [!IMPORTANT]
> **Емкость** не является прямым расчетом числа обрабатываемых запросов.

![Пики метрик емкости](./media/api-management-capacity/capacity-spikes.png)

В **емкости** могут также наблюдаться периодические пики или значения больше нуля (даже при отсутствии обрабатываемых запросов). Это происходит из-за определенных действий системы или платформы. Такие случаи не следует принимать во внимание при принятии решения о масштабировании экземпляра.

Низкий **метрики производительности** не обязательно означает, что экземпляру службы управления API не испытываю никаких проблем.
  
## <a name="use-the-azure-portal-to-examine-capacity"></a>Использование портала Azure для изучения емкости
  
![Метрики емкости](./media/api-management-capacity/capacity-metric.png)  

1. Перейдите к экземпляру APIM на [портале Azure](https://portal.azure.com/).
2. Выберите **Метрики (предварительная версия)** .
3. В сиреневом разделе выберите метрику **Емкость** из доступных метрик и оставьте значение агрегирования по умолчанию **Среднее**.

    > [!TIP]
    > Всегда следует обращать внимание на подразделение метрики **емкости** для каждого расположения с целью предотвращения неверной интерпретации.

4. В зеленом разделе выберите **Расположение**, чтобы разделить метрики по измерению.
5. Выберите нужный временной интервал на верхней панели раздела.

    Также можно настроить оповещение по метрикам, чтобы получать информацию о непредвиденных ситуациях. Например получаете уведомления, когда вашего экземпляра службы управления API превышает ожидаемый предел производительности в течение более чем 20 минут.

    >[!TIP]
    > Вы можете настроить получение оповещений, когда служба работает в условиях нехватки емкости, или использовать функциональные возможности автоматического масштабирования Azure Monitor, чтобы автоматически добавлять единицу управления API Azure. Операция масштабирования может занять около 30 минут, поэтому следует планировать правила соответствующим образом.  
    > Допускается только масштабирование главного расположения.

## <a name="use-capacity-for-scaling-decisions"></a>Использование емкости для решений масштабирования

**Емкость** является метрикой, позволяющей принимать решения относительно масштабирования экземпляра управления API для предоставления дополнительных ресурсов, позволяющих справиться с большей нагрузкой. Попробуйте следующее:

+ Просмотреть долгосрочные тенденции и средние значения.
+ Игнорировать внезапные пики, которые, скорее всего, не связаны с увеличением нагрузки (подробное описание см. в разделе "Поведение метрик емкости").
+ Обновить или масштабировать экземпляр, если значение **емкости** превышает 60 % или 70 % на протяжении длительного времени (например, 30 минут). Разные значения могут работать лучше в вашей службе или сценарии.

>[!TIP]  
> Если вы можете заранее оценить трафик, выполните тестирование экземпляра APIM на ожидаемых рабочих нагрузках. Вы можете постепенно увеличивать нагрузку запросов на клиент и отследить, какое значение метрики емкости соответствует пиковой нагрузке. Выполните шаги из предыдущего раздела, чтобы понять, какая емкость используется в любой момент времени, используя портал Azure.

## <a name="next-steps"></a>Следующие шаги

[Обновление и масштабирование экземпляра API Management](upgrade-and-scale.md)