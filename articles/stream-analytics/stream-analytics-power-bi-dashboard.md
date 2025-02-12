---
title: Интеграция панели мониторинга Power BI с Azure Stream Analytics
description: В этой статье описывается, как использовать панель мониторинга Power BI в режиме реального времени для визуализации выходных данных задания Azure Stream Analytics.
services: stream-analytics
author: jseb225
ms.author: jeanb
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 06/11/2019
ms.openlocfilehash: c415bdecdaf55f3068dcd804ab34de402fe7a31f
ms.sourcegitcommit: 6a42dd4b746f3e6de69f7ad0107cc7ad654e39ae
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/07/2019
ms.locfileid: "67612295"
---
# <a name="stream-analytics-and-power-bi-a-real-time-analytics-dashboard-for-streaming-data"></a>Stream Analytics и Power BI: панель мониторинга аналитики для потоковой передачи данных в режиме реального времени

Azure Stream Analytics позволяет воспользоваться одним из лучших инструментов для бизнес-аналитики, [Microsoft Power BI](https://powerbi.com/). В этой статье вы узнаете, как создавать средства бизнес-аналитики, отображая в Power BI выходные данные заданий Azure Stream Analytics. Вы также узнаете, как создавать информационные панели и использовать их в режиме реального времени.

Эта статья является продолжением руководства Stream Analytics по [выявлению мошенничества в реальном времени](stream-analytics-real-time-fraud-detection.md). Она создана на основе рабочего процесса, изложенного в этом руководстве. Кроме того, в ней добавлены выходные данные Power BI для визуализации мошеннических телефонных вызовов, обнаруженных заданием Stream Analytics. 

Посмотрите [видео](https://www.youtube.com/watch?v=SGUpT-a99MA) об этом сценарии.


## <a name="prerequisites"></a>предварительные требования

Чтобы начать, у вас должны быть следующие компоненты:

* Учетная запись Azure.
* Учетная запись для Power BI. Вы можете использовать рабочую или учебную учетную запись.
* Полная версия руководства по выявлению мошенничества в режиме реального времени представлена [здесь](stream-analytics-real-time-fraud-detection.md). Руководство включает приложение, создающее вымышленные метаданные телефонных вызовов. В этом руководстве описано создание концентратора событий и отправка потоковых данных телефонных вызовов к нему. Вы напишете запрос, определяющий мошеннические вызовы (вызовы с одного и того же номера одновременно из разных мест). 


## <a name="add-power-bi-output"></a>Добавление Power BI в качестве места назначения выходных данных
В руководстве по выявлению мошенничества в реальном времени выходные данные отправлялись в хранилище BLOB-объектов Azure. В этом разделе вы добавите выход, который отправляет сведения в Power BI.

1. На портале Azure откройте задание Streaming Analytics, созданное ранее. Если вы использовали рекомендуемое имя, задание называется `sa_frauddetection_job_demo`.

2. В меню слева, выберите **выходные данные** под **топология задания**. Выберите **+ добавить** и выберите **Power BI** в раскрывающемся меню.

3. Выберите **+ Добавить** > **Power BI**. Затем заполните форму следующими сведениями и нажмите кнопку **Авторизовать**:

   |**Параметр**  |**Рекомендуемое значение**  |
   |---------|---------|
   |Псевдоним выходных данных  |  CallStream PowerBI  |
   |Имя набора данных  |   SA-набора данных  |
   |Имя таблицы |  мошеннические вызовы  |

   ![Настройка выходных данных Stream Analytics](media/stream-analytics-power-bi-dashboard/configure-stream-analytics-output.png)

   > [!WARNING]
   > Если в Power BI есть набор данных и таблица с такими же именами, которые вы указали в задании Stream Analytics, имеющиеся имена будут перезаписаны.
   > Мы не рекомендуем явным образом создавать набор данных и таблицу в учетной записи Power BI. При запуске задания Stream Analytics они будут созданы автоматически, и задание начнет вносить выходные данные для Power BI. Если ваш запрос задания не возвращает никаких результатов, набор данных и таблица не создаются.
   >

4. Когда вы нажмете кнопку **Авторизовать**, откроется всплывающее окно и вам будет предложено ввести учетные данные для проверки подлинности учетной записи Power BI. Когда авторизация будет выполнена успешно, **сохраните** параметры.

8. Нажмите кнопку **Создать**.

Набор данных создается со следующими параметрами:

* **defaultRetentionPolicy: BasicFIFO** -данных — FIFO, более 200 000 строк.
* **defaultMode: pushStreaming** -набор данных поддерживает элементов потоковой передачи и традиционных визуальных объектов для отчетов (также известный как Push-уведомлений).

Сейчас нельзя создавать наборы данных с другими флагами.

Дополнительные сведения о наборах данных Power BI см. в справочнике по [REST API Power BI](https://msdn.microsoft.com/library/mt203562.aspx).


## <a name="write-the-query"></a>Написание запроса

1. Закройте колонку **Выходные данные** и вернитесь в колонку задания.

2. Щелкните поле **Запрос**. 

3. Введите следующий запрос. Этот запрос аналогичен запросу самосоединения, созданному в руководстве по обнаружению мошенничества. Различие заключается в том, что этот запрос передает результаты в новые выходные данные, созданные вами (`CallStream-PowerBI`). 

    >[!NOTE]
    >Если вы не присвоили имя входным данным `CallStream` в руководстве по обнаружению мошенничества, замените имя `CallStream` в предложениях **FROM** и **JOIN** в запросе.

   ```SQL
   /* Our criteria for fraud:
   Calls made from the same caller to two phone switches in different locations (for example, Australia and Europe) within five seconds */

   SELECT System.Timestamp AS WindowEnd, COUNT(*) AS FraudulentCalls
   INTO "CallStream-PowerBI"
   FROM "CallStream" CS1 TIMESTAMP BY CallRecTime
   JOIN "CallStream" CS2 TIMESTAMP BY CallRecTime

   /* Where the caller is the same, as indicated by IMSI (International Mobile Subscriber Identity) */
   ON CS1.CallingIMSI = CS2.CallingIMSI

   /* ...and date between CS1 and CS2 is between one and five seconds */
   AND DATEDIFF(ss, CS1, CS2) BETWEEN 1 AND 5

   /* Where the switch location is different */
   WHERE CS1.SwitchNum != CS2.SwitchNum
   GROUP BY TumblingWindow(Duration(second, 1))
   ```

4. Нажмите кнопку **Сохранить**.


## <a name="test-the-query"></a>Проверка запроса

Этот раздел необязательный, но мы рекомендуем ознакомиться с ним. 

1. Если сейчас приложение TelcoStreaming не запущено, запустите его, сделав следующее:

    * Откройте командную строку.
    * Перейдите в папку, где находятся измененные файлы telcogenerator.exe и telcodatagen.exe.config.
    * Выполните следующую команду:

       `telcodatagen.exe 1000 .2 2`

2. На **запроса** страницы для задания Stream Analytics, щелкните точки рядом с полем `CallStream` , а затем выберите **образец данных со входа**.

3. Укажите, что необходим трехминутный образец данных, и щелкните **ОК**. Дождитесь уведомления о выборке данных.

4. Нажмите кнопку **тестирования** и просмотрите результаты.

## <a name="run-the-job"></a>Выполнение задания

1. Убедитесь, что приложение TelcoStreaming выполняется.

2. Перейдите к **Обзор** для задания Stream Analytics и укажите **запустить**.

    ![Запуск задания Stream Analytics](./media/stream-analytics-power-bi-dashboard/stream-analytics-sa-job-start-output.png)

Задание Stream Analytics начинает искать мошеннические вызовы во входящем потоке. Задание также создает набор данных и таблицу в Power BI и начинает отправлять данные о мошеннических вызовах к ним.


## <a name="create-the-dashboard-in-power-bi"></a>Создание панели мониторинга в Power BI

1. Перейдите на сайт [Powerbi.com](https://powerbi.com) и войдите с помощью рабочей или учебной учетной записи. Если результаты запроса задания Stream Analytics были выведены, вы увидите уже созданный набор данных:

    ![Местоположение набора данных потоковой передачи в Power BI](./media/stream-analytics-power-bi-dashboard/stream-analytics-streaming-dataset.png)

2. В рабочей области щелкните **+&nbsp;Создать**.

    ![Кнопка "Создать" в рабочей области Power BI](./media/stream-analytics-power-bi-dashboard/pbi-create-dashboard.png)

3. Создайте информационную панель и назовите ее `Fraudulent Calls`.

    ![Создание информационной панели и присвоение ей имени в рабочей области Power BI](./media/stream-analytics-power-bi-dashboard/pbi-create-dashboard-name.png)

4. В верхней части окна щелкните **Добавить плитку**, выберите **Пользовательские данные потоковой передачи**, а затем щелкните **Далее**.

    ![Настраиваемый элемент набора данных потоковой передачи в Power BI](./media/stream-analytics-power-bi-dashboard/custom-streaming-data.png)

5. В области **Your datsets** (Ваши наборы данных) выберите свой набор данных и щелкните **Далее**.

    ![Набор данных потоковой передачи в Power BI](./media/stream-analytics-power-bi-dashboard/your-streaming-dataset.png)

6. В поле **Тип визуализации** выберите **Карточка**, а затем в списке **Поля** выберите **fraudulentcalls**.

    ![Сведения о визуализации для новой плитки](./media/stream-analytics-power-bi-dashboard/add-fraudulent-calls-tile.png)

7. Нажмите кнопку **Далее**.

8. Заполните сведения о плитке, такие как заголовок и подзаголовок.

    ![Заголовок и подзаголовок для новой плитки](./media/stream-analytics-power-bi-dashboard/pbi-new-tile-details.png)

9. Нажмите кнопку **Применить**.

    Теперь у вас есть счетчик попыток совершения мошенничества.

    ![Счетчик попыток совершения мошенничества на панели мониторинга Power BI](./media/stream-analytics-power-bi-dashboard/power-bi-fraud-counter-tile.png)

8. Повторите весь процесс еще раз, чтобы добавить плитку (начиная с шага 4). В этот раз сделайте следующее:

    * В поле **Тип визуализации** выберите **График**. 
    * Добавьте ось и выберите **windowend**. 
    * Добавьте значение и выберите **fraudulentcalls**.
    * В списке **Отображаемый интервал времени** укажите последние 10 минут.

      ![Создание плитки графика в Power BI](./media/stream-analytics-power-bi-dashboard/pbi-create-tile-line-chart.png)

9. Щелкните **Далее**, добавьте заголовок и подзаголовок, а затем щелкните **Применить**.

     Теперь на информационной панели Power BI есть два представления данных о мошеннических вызовах, обнаруженных в потоковой передаче данных.

     ![Готовая информационная панель Power BI, отображающая 2 плитки со сведениями о мошеннических вызовах](./media/stream-analytics-power-bi-dashboard/pbi-dashboard-fraudulent-calls-finished.png)


## <a name="learn-more-about-power-bi"></a>Подробнее о Power BI

В этом руководстве демонстрируется, как создать некоторые визуализации для набора данных. Power BI позволяет создавать другие клиентские инструменты бизнес-аналитики для вашей организации. Дополнительные идеи см. в следующих ресурсах:

* С еще одним примером панели мониторинга Power BI можно ознакомиться в видео [Приступая к работе с Power BI](https://youtu.be/L-Z_6P56aas?t=1m58s) .
* Дополнительные сведения о настройке вывода задания Streaming Analytics в Power BI и использовании групп Power BI см. разделе [Power BI](stream-analytics-define-outputs.md#power-bi) статьи [Выходные данные Stream Analytics: возможности хранения и анализа](stream-analytics-define-outputs.md). 
* Дополнительные сведения об использовании Power BI см. в статье [Панели мониторинга в службе Power BI](https://powerbi.microsoft.com/documentation/powerbi-service-dashboards/).


## <a name="learn-about-limitations-and-best-practices"></a>Информация об ограничениях и рекомендациях
Сейчас Power BI можно вызывать примерно один раз в секунду. Визуальные элементы потоковой передачи поддерживают пакеты размером в 15 КБ. При превышении этого порога визуальные элементы потоковой передачи останавливаются (но данные для отправки продолжают обрабатываться). С учетом этого Power BI лучше всего подходит для случаев, когда Azure Stream Analytics значительно уменьшает объем передаваемых данных. Мы рекомендуем использовать переворачивающееся или прыгающее окно, чтобы отправка данных происходила не чаще одного раза в секунду, а запрос отвечал требованиям к пропускной способности.

Можно использовать следующую формулу для вычисления длительности окна в секундах:

![Формула для вычисления длительности окна в секундах](./media/stream-analytics-power-bi-dashboard/compute-window-seconds-equation.png)  

Пример:

* У вас есть 1000 устройств, отправляющих данные с интервалами в 1 секунду.
* Вы используете лицензию Power BI Pro, которая поддерживает 1 000 000 строк в час.
* Вы хотите публиковать в Power BI средний объем данных на каждом устройстве.

Соответствующее выражение для расчета выглядит так:

![Формула на основе критериев примера](./media/stream-analytics-power-bi-dashboard/power-bi-example-equation.png)  

При такой конфигурации можно изменить исходный запрос на следующий:

```SQL
    SELECT
        MAX(hmdt) AS hmdt,
        MAX(temp) AS temp,
        System.TimeStamp AS time,
        dspl
    INTO "CallStream-PowerBI"
    FROM
        Input TIMESTAMP BY time
    GROUP BY
        TUMBLINGWINDOW(ss,4),
        dspl
```

### <a name="renew-authorization"></a>Обновление авторизации
Если с момента создания задания или последней аутентификации пароль был изменен, необходимо повторно выполнить аутентификацию с учетной записью Power BI. Если в клиенте Azure Active Directory (AAD) настроена Многофакторная идентификация Azure, вам потребуется каждые две недели обновлять авторизацию Power BI. Если вы этого не сделаете, возможны такие проблемы, как отсутствие выходных данных задания или `Authenticate user error` в журналах операций.

Аналогично, если задание запускается после истечения срока действия маркера, возникает ошибка и происходит сбой задания. Чтобы устранить эту проблему, остановите выполнение задания и перейдите к выходным данным Power BI. Чтобы избежать потери данных, щелкните ссылку **Обновить авторизацию** и перезапустите задание со **времени последней остановки**.

Когда вы обновите авторизацию в Power BI, в области авторизации появится оповещение зеленого цвета о том, что проблема устранена.

## <a name="get-help"></a>Получение справки
За дополнительной помощью обращайтесь на наш [форум Azure Stream Analytics](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureStreamAnalytics).

## <a name="next-steps"></a>Дальнейшие действия
* [Введение в Azure Stream Analytics](stream-analytics-introduction.md)
* [Приступая к работе с Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Масштабирование заданий в службе Azure Stream Analytics](stream-analytics-scale-jobs.md)
* [Azure Stream Analytics query language reference](https://docs.microsoft.com/stream-analytics-query/stream-analytics-query-language-reference) (Справочник по языку запросов Azure Stream Analytics).
* [Справочник по API-интерфейсу REST управления Stream Analytics](https://msdn.microsoft.com/library/azure/dn835031.aspx)
