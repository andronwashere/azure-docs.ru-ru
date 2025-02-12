---
title: Импорт данных Log Analytics Azure в Power BI | Документация Майкрософт
description: Power BI — это облачная служба бизнес-аналитики от Майкрософт, предоставляющая расширенную визуализацию данных и отчеты по анализу различных наборов данных.  В этой статье описываются способы настройки и импорта данных Log Analytics в Power BI, а также настройка этой службы для автоматического обновления.
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: tysonn
ms.assetid: 83edc411-6886-4de1-aadd-33982147b9c3
ms.service: log-analytics
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/01/2019
ms.author: bwren
ms.openlocfilehash: 0b1627306f1a8e9d9285c72118bfebdcb53d369b
ms.sourcegitcommit: c0419208061b2b5579f6e16f78d9d45513bb7bbc
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/08/2019
ms.locfileid: "67626116"
---
# <a name="import-azure-monitor-log-data-into-power-bi"></a>Импорт данных журнала в Azure Monitor в Power BI


[Power BI](https://powerbi.microsoft.com/documentation/powerbi-service-get-started/) — это облачная служба бизнес-аналитики от Майкрософт, предоставляющая расширенную визуализацию данных и отчеты по анализу различных наборов данных.  Результаты запроса журнала Azure Monitor можно импортировать в набор данных Power BI, чтобы вы могли использовать преимущества этих функций, таких как объединение данных из различных источников и совместное использование отчетов в веб- и мобильных устройств.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../../includes/azure-monitor-log-analytics-rebrand.md)]

## <a name="overview"></a>Обзор
Для импорта данных из [рабочей области Log Analytics](manage-access.md) в Azure Monitor в Power BI, вы создадите набор данных в Power BI на основе [запроса журнала](../log-query/log-query-overview.md) в Azure Monitor.  Запрос выполняется каждый раз при обновлении набора данных.  Затем можно создавать отчеты Power BI, которые используют данные из набора данных.  Чтобы создать набор данных в Power BI, нужно экспортировать запрос из Log Analytics в [язык Power Query (M)](https://docs.microsoft.com/powerquery-m/power-query-m-language-specification).  Потом этот импортированный запрос используется для создания запроса в Power BI Desktop, а затем публикуется в Power BI в качестве набора данных.  Ниже описаны подробности этого процесса.

![Из Log Analytics в Power BI](media/powerbi/overview.png)

## <a name="export-query"></a>Экспорт запроса
Начните с создания [запроса журнала](../log-query/log-query-overview.md) , возвращающее данные, вам нужно заполнить набор данных Power BI.  Затем экспортируйте этот запрос в [язык Power Query (M)](https://docs.microsoft.com/powerquery-m/power-query-m-language-specification), который может использоваться в Power BI Desktop.

1. [Создание запроса к журналу в Log Analytics](../log-query/get-started-portal.md) для извлечения данных для набора данных.
2. Выберите **Экспорт** > **Power BI Query (M)** .  Запрос будет экспортирован в текстовый файл с именем **PowerBIQuery.txt**. 

    ![Экспорт поиска по журналам](media/powerbi/export-analytics.png)

3. Откройте текстовый файл и скопируйте его содержимое.

## <a name="import-query-into-power-bi-desktop"></a>Импорт запроса в Power BI Desktop
Power BI Desktop — это классическое приложение, позволяющее создавать наборы данных и отчеты, которые можно опубликовать в Power BI.  Его также можно использовать для создания запроса на языке Power Query, экспортированные из Azure Monitor. 

1. Установите приложение [Power BI Desktop](https://powerbi.microsoft.com/desktop/), если у вас его еще нет, а затем откройте его.
2. Чтобы открыть новый запрос, выберите **Получить данные** > **Пустой запрос**.  Затем выберите **Расширенный редактор** и вставьте содержимое экспортированного файла в запрос. Нажмите кнопку **Done**(Готово).

    ![Запрос Power BI Desktop](media/powerbi/desktop-new-query.png)

5. Запрос выполняется и его результаты отображаются.  Возможно потребуется ввести учетные данные для подключения к Azure.  
6. Введите описательное имя для запроса.  По умолчанию используется значение **Query1**. Чтобы добавить набор данных в отчет, нажмите кнопку **Close and Apply** (Закрыть и применить).

    ![Имя Power BI Desktop](media/powerbi/desktop-results.png)



## <a name="publish-to-power-bi"></a>Публикация в Power BI
При публикации в Power BI создается набор данных и отчет.  При создании отчета в Power BI Desktop он будет опубликован с вашими данными.  Если нет, то будет создан пустой отчет.  В Power BI можно изменить отчет или создать новый на основе набора данных.

1. Создайте отчет на основе данных.  Прочитайте [документацию по Power BI Desktop](https://docs.microsoft.com/power-bi/desktop-report-view), если вы не знакомы с ней.  
1. Когда вы будете готовы отправить отчет в Power BI, выберите **Опубликовать**.  
1. При появлении запроса выберите место назначения в своей учетной записи Power BI.  При отсутствии определенного места назначения можно использовать область **Моя рабочая область**.

    ![Публикация в Power BI Desktop](media/powerbi/desktop-publish.png)

1. Чтобы открыть Power BI с новым набором данных, после завершения публикации щелкните **Открыть в Power BI**.


### <a name="configure-scheduled-refresh"></a>Настройка запланированного обновления
В наборе данных, созданном в Power BI, будут содержаться те же данные, что и ранее в Power BI Desktop.  Необходимо обновить набор данных периодически, чтобы повторно выполнить запрос и заполнить его последними данными из Azure Monitor.  

1. Щелкните в рабочей области, где находится переданный отчет, и выберите меню **Наборы данных**. 
1. Откройте контекстное меню нового набора данных и выберите **Параметры**. 
1. В разделе **Учетные данные источника данных** должно быть указано, что учетные данные недействительны.  Это связано с тем, что учетные данные, используемые при обновлении данных в наборе данных, еще не указаны.  
1. Нажмите кнопку **изменить учетные данные** и укажите учетные данные с доступом к рабочей области Log Analytics в Azure Monitor. Если требуется двухфакторная проверка подлинности, выберите **OAuth2** для **метод проверки подлинности** будет предложено войти в систему с учетными данными.

    ![Расписание Power BI](media/powerbi/powerbi-schedule.png)

5. В разделе **Запланированное обновление** включите переключатель в области **Поддерживать актуальность данных**.  При необходимости можно изменить **периодичность обновления**, а также задать одну или несколько точек времени для обновления.

    ![Обновление Power BI](media/powerbi/powerbi-schedule-refresh.png)



## <a name="next-steps"></a>Следующие шаги
* Узнайте больше о [запросах поиска по журналам](../log-query/log-query-overview.md) , чтобы создавать запросы, которые можно экспортировать в Power BI.
* Дополнительные сведения о [Power BI](https://powerbi.microsoft.com) для создания визуализаций, на основании экспортов журнала Azure Monitor.