---
title: Настройка метода взвешенной маршрутизации трафика с циклическим перебором с помощью диспетчера трафика Azure | Документация Майкрософт
description: В этой статье объясняется, как балансировать нагрузку трафика методом циклического перебора в диспетчере трафика.
services: traffic-manager
documentationcenter: ''
author: asudbring
manager: twooley
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/20/2017
ms.author: allensu
ms.openlocfilehash: 4ca43bf958606a71911bf5d35f31e4fe0b342601
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "67071284"
---
# <a name="configure-the-weighted-traffic-routing-method-in-traffic-manager"></a>Настройка метода взвешенной маршрутизации трафика в диспетчере трафика

Общепринятый подход к методу маршрутизации трафика состоит в предоставлении набора идентичных конечных точек (облачных служб и веб-сайтов) и равномерной отправке трафика в каждую из них. Ниже приведены инструкции по настройке такого метода маршрутизации трафика.

> [!NOTE]
> Веб-сайты Azure уже обеспечивают балансировку нагрузки с циклическим перебором для веб-сайтов в любом регионе Azure (который может содержать несколько центров обработки данных). Диспетчер трафика позволяет распределять трафик между веб-сайтами в разных центрах обработки данных.

## <a name="to-configure-the-weighted-traffic-routing-method"></a>Настройка метода взвешенной маршрутизации трафика

1. В браузере войдите на [портал Azure](https://portal.azure.com). Если у вас нет учетной записи, вы можете зарегистрироваться для получения [бесплатной пробной версии на один месяц](https://azure.microsoft.com/free/). 
2. На панели поиска портала выполните поиск **профилей диспетчера трафика** и щелкните имя профиля, для которого нужно настроить метод маршрутизации.
3. В колонке **Профиль диспетчера трафика** убедитесь, что отображаются и облачные службы, и веб-сайты, которые требуется включить в конфигурацию.
4. В разделе **Параметры** щелкните **Конфигурация** и в колонке **Конфигурация** сделайте следующее.
    1. В области **Настройки метода маршрутизации трафика** убедитесь, что в качестве метода маршрутизации трафика выбран вариант **Взвешенный**. В противном случае выберите пункт **Взвешенный** в раскрывающемся списке.
    2. Задайте одинаковые **параметры монитора конечной точки** для всех конечных точек в этом профиле, как показано ниже.
        1. Выберите соответствующий **протокол** и укажите номер **порта**. 
        2. В поле **Путь** введите косую черту */* . Для мониторинга конечных точек необходимо указать путь и имя файла. Косая черта ("/") является допустимым относительным путем и указывает, что файл расположен в корневом каталоге (по умолчанию).
        3. В верхней части страницы нажмите кнопку **Сохранить**.
5. Протестируйте изменения в конфигурации следующим образом.
    1.  На панели поиска портала выполните поиск по имени профиля диспетчера трафика и щелкните профиль диспетчера трафика в отображаемых результатах.
    2.  В колонке **Профиль диспетчера трафика** щелкните **Обзор**.
    3.  В колонке **Профиль диспетчера трафика** отображается DNS-имя только что созданного профиля диспетчера трафика. Его могут использовать клиенты (например, перейдя к нему в веб-браузере) для направления к нужной конечной точке в соответствии с типом маршрутизации. В этом случае все запросы направляются к каждой конечной точке методом циклического перебора.
6. После запуска профиля диспетчера трафика измените запись DNS на полномочном DNS-сервере так, чтобы доменное имя компании указывало на доменное имя диспетчера трафика.

![Настройка метода взвешенной маршрутизации трафика с помощью диспетчера трафика][1]

## <a name="next-steps"></a>Дальнейшие действия

- Узнайте больше о [методе маршрутизации по приоритету](traffic-manager-configure-priority-routing-method.md).
- Узнайте больше о [методе маршрутизации по производительности](traffic-manager-configure-performance-routing-method.md).
- Узнайте о [методе географической маршрутизации](traffic-manager-configure-geographic-routing-method.md).
- Узнайте, как [проверить параметры диспетчера трафика](traffic-manager-testing-settings.md).

<!--Image references-->
[1]: ./media/traffic-manager-weighted-routing-method/traffic-manager-weighted-routing-method.png
