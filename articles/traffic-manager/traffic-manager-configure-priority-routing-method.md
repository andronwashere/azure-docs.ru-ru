---
title: Настройка метода маршрутизации трафика по приоритету с помощью диспетчера трафика Azure | Документация Майкрософт
description: В этой статье объясняется, как настроить метод маршрутизации трафика по приоритету с помощью диспетчера трафика Azure.
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
ms.openlocfilehash: 259457a604727cba6e6964851ec4fcf4b13a20a6
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "67048487"
---
# <a name="configure-priority-traffic-routing-method-in-traffic-manager"></a>Настройка метода маршрутизации трафика по приоритету в диспетчере трафика

Независимо от режима веб-сайта веб-сайты Azure уже обеспечивают отработку отказов для веб-сайтов в центре данных (регионе). Диспетчер трафика обеспечивает отработку отказа для веб-сайтов в различных центрах данных.

Согласно общему шаблону отработки отказа службы трафик отправляется в основную службу и предоставляется набор идентичных резервных служб для отработки отказа. Ниже описано, как настроить приоритетную отработку отказа с использованием облачных служб и веб-сайтов Azure.

## <a name="to-configure-the-priority-traffic-routing-method"></a>Настройка метода маршрутизации трафика по приоритету

1. В браузере войдите на [портал Azure](https://portal.azure.com). Если у вас нет учетной записи, вы можете зарегистрироваться для получения [бесплатной пробной версии на один месяц](https://azure.microsoft.com/free/). 
2. На панели поиска портала выполните поиск **профилей диспетчера трафика** и щелкните имя профиля, для которого нужно настроить метод маршрутизации.
3. В колонке **Профиль диспетчера трафика** убедитесь, что отображаются и облачные службы, и веб-сайты, которые требуется включить в конфигурацию.
4. В разделе **Параметры** щелкните **Конфигурация** и в колонке **Конфигурация** сделайте следующее.
    1. В области **Настройки метода маршрутизации трафика** убедитесь, что в качестве метода маршрутизации трафика выбран вариант **Приоритет**. В противном случае выберите **Приоритет** из раскрывающегося списка.
    2. Задайте одинаковые **параметры монитора конечной точки** для всех конечных точек в этом профиле, как показано ниже.
        1. Выберите соответствующий **протокол** и укажите номер **порта**. 
        2. В поле **Путь** введите косую черту */* . Для мониторинга конечных точек необходимо указать путь и имя файла. Косая черта ("/") является допустимым относительным путем и указывает, что файл расположен в корневом каталоге (по умолчанию).
        3. В верхней части страницы нажмите кнопку **Сохранить**.
5. В разделе **Параметры** щелкните **Конечные точки**.
6. В колонке **Конечные точки** проверьте порядок приоритетов конечных точек. Для метода маршрутизации трафика **по приоритету** порядок выбранных конечных точек имеет значение. Проверьте порядок приоритетов конечных точек.  Основная конечная точка указана вверху. Внимательно проверьте порядок, в котором отображаются конечные точки. Все запросы будут направляться к первой конечной точке. Если диспетчер трафика обнаружит, что она неработоспособна, то будет автоматически выполнена отработка отказа трафика на следующую конечную точку. 
7. Чтобы изменить порядок приоритета конечной точки, щелкните эту конечную точку. В открывшейся колонке **Конечная точка** щелкните **Изменить** и задайте нужное значение **Приоритет**. 
8. Щелкните **Сохранить**, чтобы сохранить изменения параметров конечной точки.
9. После внесения всех изменений нажмите кнопку **Сохранить** в нижней части экрана.
10. Протестируйте изменения в конфигурации следующим образом.
    1.  На панели поиска портала выполните поиск по имени профиля диспетчера трафика и щелкните профиль диспетчера трафика в отображаемых результатах.
    2.  В колонке **Профиль диспетчера трафика** щелкните **Обзор**.
    3.  В колонке **Профиль диспетчера трафика** отображается DNS-имя только что созданного профиля диспетчера трафика. Его могут использовать клиенты (например, перейдя к нему в веб-браузере) для направления к нужной конечной точке в соответствии с типом маршрутизации. В этом случае все запросы направляются к первой конечной точке. Если диспетчер трафика обнаружит, что она неработоспособна, то будет автоматически выполнена отработка отказа трафика на следующую конечную точку.
11. После запуска профиля диспетчера трафика измените запись DNS на полномочном DNS-сервере так, чтобы доменное имя компании указывало на доменное имя диспетчера трафика.

![Настройка метода маршрутизации трафика по приоритету с помощью диспетчера трафика][1]

## <a name="next-steps"></a>Дальнейшие действия


- Узнайте о [методе взвешенной маршрутизации трафика](traffic-manager-configure-weighted-routing-method.md).
- Узнайте о [методе маршрутизации по производительности](traffic-manager-configure-performance-routing-method.md).
- Узнайте о [методе географической маршрутизации](traffic-manager-configure-geographic-routing-method.md).
- Узнайте, как [проверить параметры диспетчера трафика](traffic-manager-testing-settings.md).

<!--Image references-->
[1]: ./media/traffic-manager-priority-routing-method/traffic-manager-priority-routing-method.png