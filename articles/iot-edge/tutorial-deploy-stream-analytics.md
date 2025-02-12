---
title: Руководство. Развертывание задания Azure Stream Analytics на устройстве — Azure IoT Edge | Документация Майкрософт
description: В этом руководстве Azure Stream Analytics развертывается в качестве модуля на устройстве IoT Edge.
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 09/21/2018
ms.topic: tutorial
ms.service: iot-edge
ms.custom: mvc, seodec18
ms.openlocfilehash: cea0d091620d53892f3334306a341b196765879d
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2019
ms.locfileid: "64575879"
---
# <a name="tutorial-deploy-azure-stream-analytics-as-an-iot-edge-module"></a>Руководство по Развертывание Azure Stream Analytics в качестве модуля IoT Edge

Многие решения Интернета вещей используют службы аналитики, чтобы получать аналитические сведения о данных по мере того, как они попадают в облако с устройств Интернета вещей. С помощью Azure IoT Edge можно переместить логику [Azure Stream Analytics](https://docs.microsoft.com/azure/stream-analytics/) на само устройство. Обрабатывая потоки данных телеметрии в Edge, можно сократить объем отправленных данных и время, необходимое для реагирования на полезные аналитические сведения.

Azure IoT Edge и Azure Stream Analytics интегрированы, поэтому вы можете создать задание Azure Stream Analytics на портале Azure и затем развернуть его в качестве модуля IoT Edge без дополнительного кода.  

Azure Stream Analytics предоставляет расширенный синтаксис структурированных запросов для анализа данных как в облаке, так и на устройствах IoT Edge. Дополнительные сведения об Azure Stream Analytics в IoT Edge см. в статье [Azure Stream Analytics в IoT Edge (предварительная версия)](../stream-analytics/stream-analytics-edge.md).

В этом руководстве модуль Stream Analytics вычисляет среднюю температуру в течение скользящего 30-секундного окна. Когда это значение достигает 70, модуль отправляет предупреждение, чтобы устройство выполнило действие. В этом случае действие заключается в сбросе имитируемого датчика температуры. В рабочей среде вы можете использовать эту функцию для выключения компьютера или принятия профилактических мер, когда температура достигает опасных уровней. 

Из этого руководства вы узнаете, как выполнять следующие задачи:

> [!div class="checklist"]
> * Создание задания Azure Stream Analytics для обработки пограничных данных.
> * Подключение нового задания Azure Stream Analytics с другими модулями IoT Edge.
> * Развертывание задания Azure Stream Analytics на устройстве IoT Edge с портала Azure.

<center>

![Схема рассматриваемой в руководстве архитектуры, а также размещение и развертывание задания ASA](./media/tutorial-deploy-stream-analytics/asa-architecture.png)
</center>


[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Предварительные требования

Устройство Azure IoT Edge.

* В качестве устройства IoT Edge можно использовать виртуальную машину Azure. Для этого выполните действия, описанные в кратком руководстве для устройства [Linux](quickstart-linux.md) или [Windows](quickstart.md).

Облачные ресурсы.

* [Центр Интернета вещей](../iot-hub/iot-hub-create-through-portal.md) ценовой категории "Бесплатный" или "Стандартный" в Azure. 


## <a name="create-an-azure-stream-analytics-job"></a>Создание задания Azure Stream Analytics

В этом разделе создается задание Azure Stream Analytics для получения данных из вашего Центра Интернета вещей, запроса данных телеметрии, отправленных с вашего устройства, и передачи результатов в контейнер хранилища BLOB-объектов Azure. 

### <a name="create-a-storage-account"></a>Создание учетной записи хранения

Когда вы создаете задание Azure Stream Analytics для запуска на устройстве IoT Edge, его необходимо сохранить таким образом, чтобы его можно было вызывать с устройства. Вы можете использовать имеющуюся учетную запись хранения Azure или создать ее. 

1. На портале Azure последовательно выберите **Создать ресурс** > **Хранилище** > **Учетная запись хранения — BLOB-объект, файл, таблица, очередь**. 

1. Чтобы создать учетную запись хранения, введите следующие значения:

   | Поле | Значение |
   | ----- | ----- |
   | ИМЯ | Введите уникальное имя учетной записи хранения. | 
   | Расположение | Выберите расположение рядом с вами. |
   | Подписка | Выберите ту же подписку, к которой относится Центр Интернета вещей. |
   | Группа ресурсов | Мы рекомендуем использовать одну группу ресурсов для всех тестовых ресурсов, которые вы создаете во время работы с краткими руководствами по IoT Edge. Например, **IoTEdgeResources**. |

1. Для других полей используйте значения по умолчанию и выберите **Создать**. 

### <a name="create-a-new-job"></a>Создание задания

1. На портале Azure последовательно выберите **Создать ресурс** >  **"Интернет вещей"**  > **Задание Stream Analytics**.

1. Чтобы создать задание, введите следующие значения:

   | Поле | Значение |
   | ----- | ----- |
   | Имя задания | Укажите имя задания. Например, **IoTEdgeJob**. | 
   | Подписка | Выберите ту же подписку, к которой относится Центр Интернета вещей. |
   | Группа ресурсов | Мы рекомендуем использовать одну группу ресурсов для всех тестовых ресурсов, которые вы создаете во время работы с краткими руководствами по IoT Edge. Например, **IoTEdgeResources**. |
   | Расположение | Выберите расположение рядом с вами. | 
   | Среда размещения | Выберите **Edge**. |
 
1. Нажмите кнопку **Создать**.

### <a name="configure-your-job"></a>Настройка задания

После создания задания Stream Analytics на портале Azure вы можете настроить его с помощью входных, выходных данных и запроса для выполнения в соответствующих данных. 

С помощью трех элементов — входных, выходных данных и запроса — в этом разделе создается задание, которое принимает данные температуры от устройства IoT Edge. Оно анализирует эти данные в течение скользящего 30-секундного окна. Если средняя температура в течение этого окна превышает 70 градусов, тогда на устройство IoT Edge отправляется оповещение. В следующем разделе при развертывании задания вы точно укажете, откуда и куда поступают данные.  

1. Перейдите в задание Stream Analytics на портале Azure. 

1. В разделе **Топология задания** выберите **Входные данные**, а затем **Add stream input** (Добавить потоковый вход).

   ![Добавление входных данных Azure Stream Analytics](./media/tutorial-deploy-stream-analytics/asa_input.png)

1. В раскрывающемся списке выберите **Центр Edge**.

1. На панели **Новые входные данные** введите **temperature** как псевдоним входных данных. 

1. Для других полей используйте значения по умолчанию и нажмите кнопку **Сохранить**.

1. В разделе **Топология задания** откройте **Выходные данные**, а затем выберите **Добавить**.

   ![Добавление выходных данных Azure Stream Analytics](./media/tutorial-deploy-stream-analytics/asa_output.png)

1. В раскрывающемся списке выберите **Центр Edge**.

1. На панели **Новые выходные данные** введите **alert** как псевдоним выходных данных. 

1. Для других полей используйте значения по умолчанию и нажмите кнопку **Сохранить**.

1. В разделе **Топология задания** выберите **Запрос**.

1. Замените текст по умолчанию следующим запросом. Код SQL отправляет команду сброса на вывод оповещения, если средняя температура компьютера в течение 30-секундного окна достигает 70 градусов. Команда сброса предварительно запрограммирована в датчике как действие, которое можно предпринять. 

    ```sql
    SELECT  
        'reset' AS command 
    INTO 
       alert 
    FROM 
       temperature TIMESTAMP BY timeCreated 
    GROUP BY TumblingWindow(second,30) 
    HAVING Avg(machine.temperature) > 70
    ```

1. Щелкните **Сохранить**.

### <a name="configure-iot-edge-settings"></a>Настройка параметров IoT Edge

Чтобы подготовить задание Stream Analytics для развертывания на устройстве IoT Edge, вам необходимо связать задание с контейнером в учетной записи хранения. Когда вы перейдете к развертыванию задания, определение задания экспортируется в контейнер хранилища. 

1. В меню **Настройка** выберите **Параметры учетной записи хранения**.

1. Выберите **Добавление учетной записи хранения**. 

1. Выберите **Учетная запись хранения** из раскрывающегося списка.

1. Для поля **Контейнер** выберите **Создать новый** и укажите имя для контейнера хранилища. 

1. Щелкните **Сохранить**. 

## <a name="deploy-the-job"></a>Развертывание задания

Теперь вы готовы к развертыванию задания Azure Stream Analytics на своем устройстве IoT Edge. 

В этом разделе для создания *манифеста развертывания* вы используете мастер **настройки модулей** на портале Azure. Манифест развертывания — это JSON-файл, описывающий все модули, которые будут развернуты на устройство, регистры контейнера, хранящие образы модулей, сведения об управлении модулями и их взаимодействии друг с другом. Ваше устройство IoT Edge извлекает манифест развертывания из Центра Интернета вещей, а затем использует информацию в нем для развертывания и настройки всех назначенных ему модулей. 

В рамках работы с этим руководством вы развернете два модуля. Первый — **tempSensor** — модуль, имитирующим датчик температуры и влажности. Второй модуль — задание Stream Analytics. Модуль датчика предоставляет поток данных, который будет анализировать запрос задания. 

1. На портале Azure в Центре Интернета вещей перейдите к **IoT Edge**, а затем откройте страницу сведений о своем устройстве IoT Edge.

1. Щелкните **Set modules** (Настроить модули).  

1. Если модуль tempSensor на этом устройстве уже развернут, эти данные могут быть заполнены автоматически. Если же он не установлен, добавьте модуль следующим образом.

   1. Щелкните **Добавить** и выберите **Модуль IoT Edge**.
   1. В качестве имени введите **tempSensor**.
   1. В качестве универсального кода ресурса (URI) образа введите **mcr.microsoft.com/azureiotedge-simulated-temperature-sensor:1.0**. 
   1. Оставьте без изменений другие параметры и выберите **Сохранить**.

1. Добавьте задание Azure Stream Analytics Edge, выполнив следующие шаги:

   1. Нажмите кнопку **Добавить** и выберите модуль **Azure Stream Analytics**.
   1. Выберите свою подписку и созданное задание Edge Azure Stream Analytics. 
   1. Щелкните **Сохранить**.

1. После публикации задания Stream Analytics в созданный контейнер хранилища щелкните имя модуля, чтобы узнать о структуре модуля Stream Analytics. 

   Универсальный код ресурса (URI) образа указывает на стандартный образ Azure Stream Analytics. Это тот же образ, который используется для каждого задания, развертываемого на устройство IoT Edge. 

   Двойник модуля настраивается с помощью нужного свойства с именем **ASAJobInfo**. Значение этого свойства указывает на определение задания в контейнере хранилища. Это свойство отображает настройку образа Stream Analytics с помощью конкретных сведений о задании. 

1. Закройте страницу модуля.

1. Запишите имя модуля Stream Analytics, так как оно понадобится вам на следующем шаге, а затем нажмите кнопку **Далее**, чтобы продолжить.

1. Замените значение по умолчанию во вкладке **Маршруты** на код, приведенный ниже. Обновите все три экземпляра _{moduleName}_ с помощью имени своего модуля Azure Stream Analytics. 

    ```json
    {
        "routes": {
            "telemetryToCloud": "FROM /messages/modules/tempSensor/* INTO $upstream",
            "alertsToCloud": "FROM /messages/modules/{moduleName}/* INTO $upstream",
            "alertsToReset": "FROM /messages/modules/{moduleName}/* INTO BrokeredEndpoint(\"/modules/tempSensor/inputs/control\")",
            "telemetryToAsa": "FROM /messages/modules/tempSensor/* INTO BrokeredEndpoint(\"/modules/{moduleName}/inputs/temperature\")"
        }
    }
    ```

   Маршруты, объявляемые здесь, определяют поток данных через устройство IoT Edge. Данные телеметрии от датчика tempSensor отправляются в Центр Интернета вещей и входные данные **температуры**, настроенные в задании Stream Analytics. Выходные сообщения **оповещения** отправляются в Центр Интернета вещей и модуль tempSensor для запуска команды сброса. 

1. Щелкните **Далее**.

1. В окне **Проверка развертывания** выберите **Отправить**.

1. Вернитесь на страницу сведений об устройстве и выберите **Refresh** (Обновить).  

    Вы должны увидеть новый модуль Stream Analytics, работающий вместе с модулем агента IoT Edge и концентратором IoT Edge.

    ![tempSensor и модуль ASA, о которых передаются данные с устройства](./media/tutorial-deploy-stream-analytics/module_output2.png)

## <a name="view-data"></a>Просмотр данных

Теперь можно перейти к устройству IoT Edge, чтобы проверить взаимодействие между модулями Azure Stream Analytics и tempSensor.

1. Проверьте выполнение всех модулей в Docker.

   ```cmd/sh
   iotedge list  
   ```
   <!--
   ![Docker output](./media/tutorial-deploy-stream-analytics/docker_output.png)
   -->
1. Просмотрите все системные журналы и данные метрик. Используйте имя модуля Stream Analytics.

   ```cmd/sh
   iotedge logs -f {moduleName}  
   ```

Вы должны видать, как температура компьютера постепенно растет, пока не достигнет 70 градусов на 30 секунд. Затем модуль Stream Analytics активирует сброс, и температура компьютера уменьшается до 21 градуса. 

   ![Выходные данные команды сброса в журналах модуля](./media/tutorial-deploy-stream-analytics/docker_log.png)

## <a name="clean-up-resources"></a>Очистка ресурсов 

Если вы планируете перейти к следующей рекомендуемой статье, можно сохранить созданные и повторно используемые ресурсы и конфигурации. Это же устройство IoT Edge также можно использовать в качестве тестового устройства. 

В противном случае вы можете удалить локальные конфигурации и ресурсы Azure, созданные в рамках этой статьи, чтобы избежать расходов. 
 
[!INCLUDE [iot-edge-clean-up-cloud-resources](../../includes/iot-edge-clean-up-cloud-resources.md)]


## <a name="next-steps"></a>Дополнительная информация

В этом руководстве вы настроили задание Azure Streaming Analytics для анализа данных с устройства IoT Edge. Затем вы загрузили этот модуль Azure Stream Analytics на устройство IoT Edge, чтобы обрабатывать данные и реагировать на повышение температуры локально, а также отправлять агрегированный поток данных в облако. Вы можете перейти к изучению других руководств, чтобы узнать, как Azure IoT Edge может создавать решения для вашего бизнеса.

> [!div class="nextstepaction"] 
> [Руководство: развертывание службы "Машинное обучение Azure" в качестве модуля IoT Edge (предварительная версия)](tutorial-deploy-machine-learning.md)
