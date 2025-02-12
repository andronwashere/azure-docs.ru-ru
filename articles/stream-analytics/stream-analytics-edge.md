---
title: Azure Stream Analytics в IoT Edge
description: Создание заданий Edge в Azure Stream Analytics и развертывание их на устройствах, где работает Azure IoT Edge.
ms.service: stream-analytics
author: mamccrea
ms.author: mamccrea
ms.reviewer: jasonh
ms.topic: conceptual
ms.date: 07/01/2019
ms.custom: seodec18
ms.openlocfilehash: 8e3b6d0fbefb8e3d3437fd5e24f929e453c573df
ms.sourcegitcommit: 6a42dd4b746f3e6de69f7ad0107cc7ad654e39ae
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/07/2019
ms.locfileid: "67621010"
---
# <a name="azure-stream-analytics-on-iot-edge"></a>Azure Stream Analytics в IoT Edge
 
Azure Stream Analytics (ASA) в IoT Edge помогает разработчикам развертывать решения аналитики ближе к устройствам Интернета вещей практически в реальном времени, что позволяет максимально эффективно использовать данные, полученные от устройств. Azure Stream Analytics предоставляет такие преимущества, как минимальная задержка, отказоустойчивость, эффективное использование пропускной способности и соответствие требованиям. Предприятия теперь могут развертывать логику управления вблизи от промышленных объектов и дополнять выполняющийся в облаке анализ больших данных.  

Azure Stream Analytics в IoT Edge работает на платформе [Azure IoT Edge](https://azure.microsoft.com/campaigns/iot-edge/). Когда задание создано в ASA, вы можете развертывать его и управлять им с помощью Центра Интернета вещей.

## <a name="scenarios"></a>Сценарии
![Схема высокого уровня Azure IoT Edge](media/stream-analytics-edge/ASAedge-highlevel-diagram.png)

* **Инструменты контроля и управление с низкой задержкой.** Например, производственные системы безопасности должны реагировать на операционные данные со сверхнизкой задержкой. Используя ASA в IoT Edge, вы можете анализировать данные датчиков практически в реальном времени, а также при обнаружении аномалий отдавать команды на остановку машины или активацию предупреждений.
*   **Ограниченные возможности подключения к облаку.** Критически важные системы, такие как оборудование для удаленного интеллектуального анализа данных, подключенные резервуары или оборудование для детализации данных внешней разработки, должны анализировать данные и реагировать на них, даже если подключение к облаку периодически прерывается. С ASA логика потоковой передачи выполняется независимо от сетевого подключения, и вы можете выбирать, какие именно данные необходимо отправить в облако для дальнейшей обработки и хранения.
* **Ограниченная пропускная способность.** Объем данных, создаваемых с помощью реактивных двигателей или подключенных автомобилей, может быть настолько большим, что данные необходимо отфильтровывать или предварительно обрабатывать, прежде чем отправить их в облако. Используя ASA, можно отфильтровать или статистически обработать данные, которые необходимо передать в облако.
* **Соответствие.** Для обеспечения соответствия нормативным требованиям может потребоваться локально анонимизировать или статистически обработать некоторые данные перед их отправкой в облако.

## <a name="edge-jobs-in-azure-stream-analytics"></a>Задания Edge в службе Azure Stream Analytics
### <a name="what-is-an-edge-job"></a>Что такое задание "Edge"?

Задания Edge ASA выполняются в контейнерах, развернутых на [устройствах Azure IoT Edge](https://docs.microsoft.com/azure/iot-edge/how-iot-edge-works). Они состоят из двух частей:
1.  Облачная часть, которая отвечает за определение задания: пользователи определяют входы и выходы, запросы и другие параметры (неупорядоченные события и т. д.) в облаке.
2.  Модуль, выполняющийся на устройствах IoT Edge. Он содержит механизм обработки ASA и получает определение задания из облака. 

ASA развертывает задания Edge на устройства с помощью Центра Интернета вещей. Дополнительные сведения о развертывании IoT Edge [см. здесь](https://docs.microsoft.com/azure/iot-edge/module-deployment-monitoring).

![Задание Edge Azure Stream Analytics](media/stream-analytics-edge/stream-analytics-edge-job.png)


### <a name="installation-instructions"></a>Инструкции по установке
В следующей таблице приведены основные действия. Дополнительные сведения см. в следующих разделах.

|      |Шаг   | Примечания   |
| ---   | ---   |  ---      |
| 1   | **Создание контейнера хранилища**   | Контейнеры хранилища используются для сохранения определения задания, доступ к которому можно получить с устройств IoT. <br>  Любой имеющийся контейнер хранилища можно использовать повторно.     |
| 2   | **Создание задания Edge ASA**   |  Создайте новое задание, выберите **Edge** в качестве **среды размещения**. <br> Создание этих заданий и управление ими осуществляется из облака, а выполняются они на ваших устройствах IoT Edge.     |
| 3   | **Настройка среды IoT Edge на ваших устройствах**   | Инструкции для [Windows](https://docs.microsoft.com/azure/iot-edge/quickstart) или [Linux](https://docs.microsoft.com/azure/iot-edge/quickstart-linux).          |
| 4   | **Развертывание ASA на ваших устройствах IoT Edge**   |  Определение задания ASA экспортируется в контейнер хранилища, созданный ранее.       |

Вы можете выполнить инструкции из [этого пошагового руководства](https://docs.microsoft.com/azure/iot-edge/tutorial-deploy-stream-analytics), чтобы развернуть свое первое задание ASA в IoT Edge. Следующее видео поможет вам понять процесс выполнения задания Stream Analytics на устройстве IoT Edge:  


> [!VIDEO https://channel9.msdn.com/Events/Connect/2017/T157/player]

#### <a name="create-a-storage-container"></a>Создание контейнера хранилища
Контейнер хранилища необходим для экспорта скомпилированных запросов и конфигурации задания ASA. Он используется для настройки образа ASA Docker с определенным запросом. 
1. Следуйте [этим инструкциям](https://docs.microsoft.com/azure/storage/common/storage-create-storage-account) по созданию учетной записи хранения на портале Azure. Вы можете сохранить все параметры по умолчанию, чтобы использовать эту учетную запись с ASA.
2. В созданной учетной записи хранения создайте контейнер хранилища BLOB-объектов:
    1. Щелкните **Большие двоичные объекты**, а затем — **+ Контейнер**. 
    2. Введите имя и сохраните контейнер как **Частный**.

#### <a name="create-an-asa-edge-job"></a>Создание задания Edge ASA
> [!Note]
> Это руководство посвящено созданию задания ASA с помощью портала Azure. Кроме того, [чтобы создать задание ASA Edge, вы можете использовать подключаемый модуль Visual Studio](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-tools-for-visual-studio-edge-jobs).

1. На портале Azure создайте "задание Stream Analytics". [Прямая ссылка для создания задания ASA](https://ms.portal.azure.com/#create/Microsoft.StreamAnalyticsJob).

2. На экране создания выберите **Edge** в качестве **среды размещения** (см. следующий рисунок).

   ![Создание задания Azure Stream Analytics в Edge](media/stream-analytics-edge/create-asa-edge-job.png)
3. Определение задания
    1. **Определите входные потоки**. Определите один или несколько входных потоков для своего задания.
    2. Определите эталонные данные (необязательно).
    3. **Определите потоки вывода**. Определите один или несколько потоков вывода для своего задания. 
    4. **Определите запрос**. Определите запрос ASA в облаке с использованием встроенного редактора. Компилятор автоматически проверяет синтаксис, включенный для ASA Edge. Запрос также можно протестировать путем передачи образцов данных. 

4. Укажите сведения о контейнере хранилища в меню **Параметры IoT Edge**.

5. Указание необязательных параметров
    1. **Упорядочивание событий**. Неупорядоченную политику можно настроить на портале. Документация доступна [здесь](https://docs.microsoft.com/stream-analytics-query/time-skew-policies-azure-stream-analytics).
    2. **Языковой стандарт**. Задайте формат интернализации.



> [!Note]
> При создании развертывания ASA экспортирует определение задания в контейнер хранилища. Это определение задания не изменяется на протяжении развертывания. Как следствие, если вы хотите обновить задание, выполняющееся в Edge, необходимо изменить задание в ASA, а затем создать развертывание в Центре Интернета вещей.


#### <a name="set-up-your-iot-edge-environment-on-your-devices"></a>Настройка среды IoT Edge на ваших устройствах
Задания Edge можно развернуть на устройствах, работающих с Azure IoT Edge.
Для этого необходимо сделать следующее:
- Создайте Центр Интернета вещей.
- Установите среду выполнения IoT Edge и Docker на устройствах Edge.
- Задайте свои устройства как **Устройства IoT Edge** в Центре Интернета вещей.

Эти действия описаны в документации по IoT Edge для [Windows](https://docs.microsoft.com/azure/iot-edge/quickstart) и [Linux](https://docs.microsoft.com/azure/iot-edge/quickstart-linux).  


####  <a name="deployment-asa-on-your-iot-edge-devices"></a>Развертывание ASA на ваших устройствах IoT Edge
##### <a name="add-asa-to-your-deployment"></a>Добавление развертывания ASA
- На портале Azure откройте Центр Интернета вещей, перейдите в обозреватель **IoT Edge** и щелкните устройство, для которого следует применить параметры.
- Щелкните **Задание модулей**, а затем выберите **+Добавить** и **Azure Stream Analytics Module** (Модуль Azure Stream Analytics).
- Выберите подписку и созданное задание ASA Edge. Щелкните Сохранить.
![Добавление модуля ASA в развертывание](media/stream-analytics-edge/add-stream-analytics-module.png)


> [!Note]
> На этом этапе ASA создает папку с именем EdgeJobs в контейнере хранения (если она еще не существует). Для каждого развертывания в папке "EdgeJobs" создается новая вложенная папка.
> Если развернуть задание на устройствах IoT Edge, ASA создает подпись общего доступа (SAS) для файла определения задания. Ключ SAS безопасно передается на устройства IoT Edge с помощью двойника устройства. Срок действия этого ключа составляет три года после даты создания. При обновлении задания Edge Интернета вещей, подписанный URL-адрес изменится, но версия образа не изменится. Когда вы **обновление**, следуйте рабочему процессу, развертывания и уведомлений об обновлении регистрируется на устройстве.


Дополнительные сведения о развертываниях IoT Edge см. на [этой странице](https://docs.microsoft.com/azure/iot-edge/module-deployment-monitoring).


##### <a name="configure-routes"></a>Настройка маршрутов
IoT Edge предоставляет способ декларативно маршрутизировать сообщения между модулями, а также между модулями и Центром Интернета вещей. Полный синтаксис описан [здесь](https://docs.microsoft.com/azure/iot-edge/module-composition).
Имена входов и выходов, сформированных в задании ASA, можно использовать в качестве конечных точек для маршрутизации.  

###### <a name="example"></a>Пример

```json
{
    "routes": {
        "sensorToAsa":   "FROM /messages/modules/tempSensor/* INTO BrokeredEndpoint(\"/modules/ASA/inputs/temperature\")",
        "alertsToCloud": "FROM /messages/modules/ASA/* INTO $upstream",
        "alertsToReset": "FROM /messages/modules/ASA/* INTO BrokeredEndpoint(\"/modules/tempSensor/inputs/control\")"
    }
}

```
В этом примере показаны маршруты для сценария, описанного на рисунке ниже. Он содержит задание Edge с именем **ASA**, ввод с именем **temperature** и вывод с именем **alert**.
![Схема примера маршрутизации сообщений](media/stream-analytics-edge/edge-message-routing-example.png)

В этом примере определяются следующие маршруты:
- Каждое сообщение из **tempSensor** отправляется в модуль с именем **ASA** и ввод с именем **temperature**.
- Все выходные данные модуля **ASA** отправляются в Центр Интернета вещей, связанный с этим устройством ($upstream).
- Все выходные данные модуля **ASA** отправляются в конечную точку **control** **tempSensor**.


## <a name="technical-information"></a>Технические сведения
### <a name="current-limitations-for-iot-edge-jobs-compared-to-cloud-jobs"></a>Текущие ограничения заданий IoT Edge в сравнении с облачными заданиями
Целью является обеспечение соответствия между заданиями IoT Edge и облачными заданиями. Поддерживаются большинство функций языка запросов SQL, позволяя выполнять ту же логику на Edge Интернета вещей и облачным.
Тем не менее для заданий Edge пока не поддерживаются следующие компоненты:
* Определяемые пользователем функции (UDF) в JavaScript. Определяемые пользователем функции доступны в [C# для заданий IoT Edge](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-edge-csharp-udf) (предварительная версия).
* Определяемые пользователем статистические функции (UDA).
* Функции Машинного обучения Azure.
* Использование более 14 статических выражений за один шаг.
* Формат AVRO для входных и выходных данных. На данный момент поддерживаются только форматы CSV и JSON.
* Следующие операторы SQL:
    * PARTITION BY;
    * GetMetadataPropertyValue.


### <a name="runtime-and-hardware-requirements"></a>Требования к оборудованию и среде выполнения
Для запуска ASA в IoT Edge вам потребуются устройства, которые могут работать в [Azure IoT Edge](https://azure.microsoft.com/campaigns/iot-edge/). 

В ASA и Azure IoT Edge используются контейнеры **Docker** для предоставления переносимого решения, которое выполняется на нескольких ОС узла (Windows, Linux).

ASA в IoT Edge доступна в качестве образов Windows и Linux, работающих под управлением архитектур x86–64 или ARM (Advanced RISC Machines). 


### <a name="input-and-output"></a>Входные и выходные данные
#### <a name="input-and-output-streams"></a>Входные потоки и потоки вывода
Задания ASA Edge могут получать входные и выходные данные из других модулей, запущенных на устройствах IoT Edge. Чтобы подключиться к конкретным модулям и из них, можно задать конфигурацию маршрутизации во время развертывания. Дополнительные сведения см. в [документации по композиции модуля IoT Edge](https://docs.microsoft.com/azure/iot-edge/module-composition).

Для входных и выходных данных поддерживаются форматы CSV и JSON.

Для каждого входного потока и потока вывода, создаваемого в задании ASA, в развернутом модуле создается соответствующая конечная точка. Эти конечные точки можно использовать в маршрутах развертывания.

В настоящее время единственными поддерживаемыми типами потоков входных и выходных данных являются концентраторы Edge. Эталонные входные данные поддерживают тип эталонного файла. Другие выходные данные можно получить с использованием подчиненного облачного задания. Например, задание Stream Analytics, размещенное в Edge, отправляет выходные данные в концентратор Edge, который затем может отправлять их в Центр Интернета вещей. Вы можете использовать второе облачное приложение Azure Stream Analytics с входными данными из Центра Интернета вещей и выходными данными в Power BI или другими типами данных.



##### <a name="reference-data"></a>Ссылочные данные
Эталонные данные (которые также называются таблицами подстановки) — это ограниченный набор данных, являющийся по своей сути статическим или медленно изменяющимся. Он используется для уточняющего запроса или для согласования с потоком данных. Для использования эталонных данных в задании Azure Stream Analytics обычно используется [соединение эталонных данных](https://docs.microsoft.com/stream-analytics-query/reference-data-join-azure-stream-analytics) в запросе. Дополнительные сведения см. в статье [Использование эталонных данных для уточняющих запросов в Stream Analytics](stream-analytics-use-reference-data.md).

Поддерживаются только локальные эталонные данные. Когда задание развертывается на устройстве IoT Edge, оно загружает эталонные данные из определенного пользователем пути к файлу.

Чтобы создать задание с эталонными данными в Edge, сделайте следующее:

1. Создайте входные данные для своего задания.

2. Выберите значение **Эталонные данные** для поля **Тип источника**.

3. Подготовьте на устройстве файл с эталонными данными. Для контейнера Windows поместите файл с эталонными данными на локальный диск и предоставьте доступ к этому диску контейнеру Docker. Для контейнера Linux создайте том Docker и разместите на нем файл с данными.

4. Задайте путь к файлу. Для ОС узла Windows и контейнера Windows используйте абсолютный путь: `E:\<PathToFile>\v1.csv`. Для ОС узла Windows и контейнера Linux или ОС Linux и контейнера Linux используйте путь в томе: `<VolumeName>/file1.txt`.

![Новые входные эталонные данные для задания Azure Stream Analytics в IoT Edge](./media/stream-analytics-edge/Reference-Data-New-Input.png)

Эталонные данные в обновлении IoT Edge запускаются с помощью развертывания. После запуска модуль ASA выбирает обновленные данные, не останавливая выполняющееся задание.

Есть два способа обновить эталонные данные:
* обновить путь эталонных данных в задании ASA на портале Azure;
* обновить развертывание IoT Edge.

## <a name="license-and-third-party-notices"></a>Уведомления о лицензии и сторонних производителях
* [Лицензия Azure Stream Analytics на IoT Edge](https://go.microsoft.com/fwlink/?linkid=862827). 
* [Уведомление сторонних производителей для Azure Stream Analytics на IoT Edge](https://go.microsoft.com/fwlink/?linkid=862828).

## <a name="azure-stream-analytics-module-image-information"></a>Данные образа модуля Azure Stream Analytics 

Эти сведения на 2019-06-27 последнего обновления:

- Изображение: `asaedge.azurecr.io/public/azure-stream-analytics/azureiotedge:1.0.3-linux-amd64`
   - базовый образ: microsoft/dotnet:2.1.6-runtime-alpine3.7
   - Платформа:
      - Архитектура: amd64
      - ОС: linux
  
- Изображение: `asaedge.azurecr.io/public/azure-stream-analytics/azureiotedge:1.0.3-linux-arm32v7`
   - базовый образ: microsoft/dotnet:2.1.6-runtime-bionic-arm32v7
   - Платформа:
      - Архитектура: arm
      - ОС: linux
  
- Изображение: `asaedge.azurecr.io/public/azure-stream-analytics/azureiotedge:1.0.3-windows-amd64`
   - базовый образ: microsoft/dotnet:2.1.6-runtime-nanoserver-1809
   - Платформа:
      - Архитектура: amd64
      - ОС: windows
      
      
## <a name="get-help"></a>Получение справки
За дополнительной помощью обращайтесь на [форум Azure Stream Analytics](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureStreamAnalytics).

## <a name="next-steps"></a>Следующие шаги

* [Что такое Azure IoT Edge (предварительная версия)](https://docs.microsoft.com/azure/iot-edge/how-iot-edge-works)
* [Deploy Azure Stream Analytics as an IoT Edge module - preview](https://docs.microsoft.com/azure/iot-edge/tutorial-deploy-stream-analytics) (Развертывание Azure Stream Analytics в качестве модуля IoT Edge (предварительная версия))
* [Разработка заданий Edge Stream Analytics с помощью средств Visual Studio](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-tools-for-visual-studio-edge-jobs)
* [Сведения о реализации CI/CD для Stream Analytics с использованием API-интерфейсов](stream-analytics-cicd-api.md)

<!--Link references-->
[stream.analytics.developer.guide]: ../stream-analytics-developer-guide.md
[stream.analytics.scale.jobs]: stream-analytics-scale-jobs.md
[stream.analytics.introduction]: stream-analytics-introduction.md
[stream.analytics.get.started]: stream-analytics-real-time-fraud-detection.md
[stream.analytics.query.language.reference]: https://go.microsoft.com/fwlink/?LinkID=513299
[stream.analytics.rest.api.reference]: https://go.microsoft.com/fwlink/?LinkId=517301
