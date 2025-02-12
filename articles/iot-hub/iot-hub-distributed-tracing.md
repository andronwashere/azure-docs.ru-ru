---
title: Добавление идентификаторов корреляции в сообщения Интернета вещей с помощью функции распределенной трассировки (предварительная версия)
description: ''
author: jlian
manager: briz
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 02/06/2019
ms.author: jlian
ms.openlocfilehash: 302c382a7e19e9dcc4c979d31ddc0768655a1465
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "60400852"
---
# <a name="trace-azure-iot-device-to-cloud-messages-with-distributed-tracing-preview"></a>Трассировка отправляемых с устройства в облако сообщений Интернета вещей Azure с помощью функции распределенной трассировки (предварительная версия)

Центр Интернета вещей Microsoft Azure поддерживает функцию распределенной трассировки, которая сейчас доступна в режиме [предварительной версии](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Центр Интернета вещей — это одна из первых служб Azure, поддерживающих функцию распределенной трассировки. Благодаря тому, что службы Azure поддерживают эту функцию, вы сможете выполнять трассировку сообщений Интернета вещей через службы Azure, связанные с определенным решением. Сведения о распределенной трассировке см. в [этой статье](../azure-monitor/app/distributed-tracing.md).

Включив распределенную трассировку в Центре Интернета вещей, вы сможете:

- Точно отслеживать поток каждого сообщения через Центр Интернета вещей с помощью [контекста трассировки](https://github.com/w3c/trace-context). Этот контекст содержит идентификаторы корреляции, позволяющие коррелировать события одного компонента с событиями из другого. Его можно применять к некоторым или ко всем сообщениям устройства Интернета вещей с помощью [двойника устройства](iot-hub-devguide-device-twins.md).
- Настроить автоматическую запись контекста трассировки в [журналы диагностики Azure Monitor](iot-hub-monitor-resource-health.md).
- Измерять и распознавать поток сообщений, отправляемых с устройств в Центр Интернета вещей и на конечные точки маршрутизации, и задержки.
- Рассмотрим, как реализовать распределенную трассировку для служб, не относящихся к Azure, в своем решении Интернета вещей.

В этой статье используется [пакет SDK для устройств Azure IoT для C](./iot-hub-device-sdk-c-intro.md) с функцией распределенной трассировки. Реализация поддержки функции распределенной трассировки все еще выполняется для других пакетов SDK.

## <a name="prerequisites"></a>Технические условия

- Предварительная версия функции распределенной трассировки сейчас поддерживается только в Центрах Интернета вещей, созданных в следующих регионах:

  - **Северная Европа**
  - **Юго-Восточная Азия**
  - **Западная часть США 2**

- В этой статье предполагается, что вы знаете, как отправлять сообщения телеметрии в Центр Интернета вещей. Убедитесь, что вы завершили все задачи краткого руководства [Отправка данных телеметрии из устройства в Центр Интернета вещей и их чтение с помощью внутреннего приложения (C)](./quickstart-send-telemetry-c.md).

- Зарегистрируйте устройство в Центре Интернета вещей (шаги приведены в каждом кратком руководстве) и запишите строку подключения.

- Установите последнюю версию [Git](https://git-scm.com/download/).

## <a name="configure-iot-hub"></a>Настройка Центра Интернета вещей

В этом разделе вы настроите регистрацию распределенных атрибутов трассировки (идентификаторы корреляции и метки времени) в Центре Интернета вещей.

1. Перейдите в Центр Интернета вещей на [портале Azure](https://portal.azure.com/).

1. В левой области Центра Интернета вещей прокрутите вниз до раздела **Мониторинг** и нажмите кнопку **Параметры диагностики**.

1. Если параметры диагностики выключены, щелкните **Включить диагностику**. Если вы уже включили параметры диагностики, нажмите кнопку **Add diagnostic setting** (Добавить параметр диагностики).

1. В поле **Имя** введите имя нового параметра диагностики. Например, **DistributedTracingSettings**.

1. Выберите один или несколько из следующих параметров, которые определяют, куда будут отправляться данные ведения журнала:

    - **Архивировать в учетной записи хранения.** Настройте учетную запись хранения, которая будет содержать данные ведения журнала.
    - **Передать в концентратор событий.** Настройте концентратор событий, который будет содержать данные ведения журнала.
    - **Отправить в Log Analytics.** Настройте рабочую область Log Analytics, которая будет содержать данные ведения журнала.

1. В разделе **Журнал** выберите операции, сведения о которых необходимо регистрировать.

    Не забудьте выбрать параметр **DistributedTracing** и настроить длительность **хранения** журналов (в днях). Длительность хранения журналов влияет на затраты на хранилище.

    ![Снимок экрана с категорией DistributedTracing в параметрах диагностики Интернета вещей](./media/iot-hub-distributed-tracing/diag-logs.png)

1. Нажмите кнопку **Сохранить**, чтобы сохранить новый параметр.

1. (Необязательно) Чтобы настроить отправку потока сообщений в разные места, установите [правила маршрутизации по крайней мере на две разные конечные точки](iot-hub-devguide-messages-d2c.md).

После включения ведения журнала Центр Интернета вещей регистрирует сообщения, содержащие действительные свойства трассировки, в любой из следующих ситуаций:

- сообщения поступают в шлюз Центра Интернета вещей;
- сообщение обрабатывается Центром Интернета вещей;
- сообщение маршрутизируется на пользовательские конечные точки (необходимо включить маршрутизацию).

Дополнительные сведения об этих журналах и их схемах см. в статье [Мониторинг работоспособности Центра Интернета вещей Azure и быстрая диагностика неполадок](iot-hub-monitor-resource-health.md#distributed-tracing-preview).

## <a name="set-up-device"></a>Настройка устройства

В этом разделе вы подготовите среду разработки для использования с [пакетом SDK Azure IoT для C](https://github.com/Azure/azure-iot-sdk-c). Затем вы измените один из примеров, чтобы включить распределенную трассировку сообщений телеметрии устройства.

Эти инструкции предназначены для создания примера в операционной системе Windows. Сведения о других средах см. в разделах о [компиляции пакета SDK для C](https://github.com/Azure/azure-iot-sdk-c/blob/master/iothub_client/readme.md#compile) или [предварительно собранном пакете SDK для C для разработки на определенной платформе](https://github.com/Azure/azure-iot-sdk-c/blob/master/iothub_client/readme.md#prepackaged-c-sdk-for-platform-specific-development).

### <a name="clone-the-source-code-and-initialize"></a>Клонирование исходного кода и инициализация

1. Установите [рабочую нагрузку "Разработка классических приложений на C++"](https://docs.microsoft.com/cpp/build/vscpp-step-0-installation?view=vs-2017) для Visual Studio 2015 или 2017.

1. Установка [CMake](https://cmake.org/). Убедитесь, что это средство расположено в каталоге по вашему пути (`PATH`). Для этого в окне командной строки введите `cmake -version`.

1. Откройте командную строку или оболочку Git Bash. Выполните следующую команду для клонирования репозитория GitHub [пакета SDK для устройства C Интернета вещей Azure](https://github.com/Azure/azure-iot-sdk-c):

    ```cmd
    git clone https://github.com/Azure/azure-iot-sdk-c.git --recursive -b public-preview
    ```

    Выполнение этой операции может занять несколько минут.

1. Создайте подкаталог `cmake` в корневом каталоге репозитория Git и перейдите в эту папку.

    ```cmd
    cd azure-iot-sdk-c    
    mkdir cmake
    cd cmake
    cmake ..
    ```

    Если `cmake` не удается найти компилятор C++, могут возникнуть ошибки сборки во время выполнения предыдущей команды. В этом случае попробуйте, выполнить эту команду в [командной строке Visual Studio](https://docs.microsoft.com/dotnet/framework/tools/developer-command-prompt-for-vs). 

    После успешного создания последние несколько строк выходных данных будут выглядеть следующим образом:

    ```cmd
    $ cmake ..
    -- Building for: Visual Studio 15 2017
    -- Selecting Windows SDK version 10.0.16299.0 to target Windows 10.0.17134.
    -- The C compiler identification is MSVC 19.12.25835.0
    -- The CXX compiler identification is MSVC 19.12.25835.0

    ...

    -- Configuring done
    -- Generating done
    -- Build files have been written to: E:/IoT Testing/azure-iot-sdk-c/cmake
    ```

### <a name="edit-the-send-telemetry-sample-to-enable-distributed-tracing"></a>Изменение примера отправки данных телеметрии для включения распределенной трассировки

1. Используйте редактор, чтобы открыть исходный файл `azure-iot-sdk-c/iothub_client/samples/iothub_ll_telemetry_sample/iothub_ll_telemetry_sample.c`.

1. Найдите объявление константы `connectionString`.

    [!code-c[](~/samples-iot-distributed-tracing/iothub_ll_telemetry_sample-c/iothub_ll_telemetry_sample.c?name=snippet_config&highlight=2)]

    Замените значение константы `connectionString` на строку подключения устройства, записанную в разделе [Регистрация устройства](./quickstart-send-telemetry-c.md#register-a-device) в кратком руководстве [Отправка данных телеметрии из устройства в Центр Интернета вещей и их чтение с помощью внутреннего приложения (C)](./quickstart-send-telemetry-c.md).

1. Изменение определения `MESSAGE_COUNT` на `5000`:

    [!code-c[](~/samples-iot-distributed-tracing/iothub_ll_telemetry_sample-c/iothub_ll_telemetry_sample.c?name=snippet_config&highlight=3)]

1. Найдите строку, вызывающую `IoTHubDeviceClient_LL_SetConnectionStatusCallback`, чтобы зарегистрировать функцию обратного вызова состояния подключения перед отправкой цикла сообщений. Добавьте код под этой строкой, как показано ниже, чтобы вызвать `IoTHubDeviceClient_LL_EnablePolicyConfiguration` и включить функцию распределенной трассировки на устройстве:

    [!code-c[](~/samples-iot-distributed-tracing/iothub_ll_telemetry_sample-c/iothub_ll_telemetry_sample.c?name=snippet_tracing&highlight=5)]

    Функция `IoTHubDeviceClient_LL_EnablePolicyConfiguration` включает политики определенных функций Центра Интернета вещей, которые настраиваются через [двойники устройств](./iot-hub-devguide-device-twins.md). После включения `POLICY_CONFIGURATION_DISTRIBUTED_TRACING` с помощью приведенной выше строки кода в поведении трассировки устройства отразятся изменения распределенной трассировки, внесенные на двойнике устройства.

1. Чтобы сохранить пример приложения, выполняющегося без использования квоты, добавьте секундную задержку в конце цикла обработки отправки сообщений:

    [!code-c[](~/samples-iot-distributed-tracing/iothub_ll_telemetry_sample-c/iothub_ll_telemetry_sample.c?name=snippet_sleep&highlight=8)]

### <a name="compile-and-run"></a>Компиляция и запуск

1. Перейдите к каталогу проекта *iothub_ll_telemetry_sample* в созданном ранее каталоге CMake (`azure-iot-sdk-c/cmake`) и компилируйте пример:

    ```cmd
    cd iothub_client/samples/iothub_ll_telemetry_sample
    cmake --build . --target iothub_ll_telemetry_sample --config Debug
    ```

1. Запустите приложение. Устройство отправляет данные телеметрии, которые поддерживают распределенную трассировку.

    ```cmd
    Debug/iothub_ll_telemetry_sample.exe
    ```

1. Не отключайте приложение. Дополнительно наблюдайте за сообщением, отправленным в Центр Интернета вещей, в окне консоли.

<!-- For a client app that can receive sampling decisions from the cloud, check out [this sample](https://aka.ms/iottracingCsample).  -->

### <a name="workaround-for-third-party-clients"></a>Обходной путь для сторонних клиентов

Он имеет **не тривиальный** для предварительного просмотра функцию распределенную трассировку без использования пакета SDK для C. Таким образом этот подход не рекомендуется.

Во-первых, необходимо реализовать все примитивы центра Интернета вещей протокола в сообщениях, следуя указаниям руководства разработки [Создание и чтение сообщений центра Интернета вещей](iot-hub-devguide-messages-construct.md). Измените свойства протокола в сообщениях MQTT или AMQP, чтобы добавить `tracestate` как **системное свойство**. В частности:

* Протоколы MQTT, добавьте `%24.tracestate=timestamp%3d1539243209` в раздел сообщения, где `1539243209` следует заменить на время создания сообщения в формат метки времени unix. Например, см. в реализацию [в пакете SDK для C](https://github.com/Azure/azure-iot-sdk-c/blob/6633c5b18710febf1af7713cf1a336fd38f623ed/iothub_client/src/iothubtransport_mqtt_common.c#L761)
* AMQP, добавьте `key("tracestate")` и `value("timestamp=1539243209")` как сообщение заметки. Справочную реализацию, см. в разделе [здесь](https://github.com/Azure/azure-iot-sdk-c/blob/6633c5b18710febf1af7713cf1a336fd38f623ed/iothub_client/src/uamqp_messaging.c#L527).

Чтобы контролировать процентную долю сообщений, содержащих это свойство, внедрите логику прослушивания инициированных облаком событий, таких как обновления двойника.

## <a name="update-sampling-options"></a>Обновление параметров выборки 

Чтобы изменить процентную долю трассируемых из облака сообщений, необходимо обновить двойник устройства. Этого можно достигнуть несколькими способами, включая редактор JSON на портале и пакет SDK для службы Центра Интернета вещей. Примеры приведены в следующих подразделах.

### <a name="update-using-the-portal"></a>Обновление с помощью портала

1. Перейдите в центр Интернета вещей на [портале Azure](https://portal.azure.com/), а затем нажмите кнопку **Устройства Интернета вещей**.

1. Выберите устройство.

1. Найдите параметр **Enable distributed tracing (preview)** (Включение распределенной трассировки (предварительная версия)), а затем выберите **Включить**.

    ![Включение распределенной трассировки на портале Azure](./media/iot-hub-distributed-tracing/azure-portal.png)

1. Укажите **частоту выборки** в диапазоне от 0 до 100 %.

1. Выберите команду **Сохранить**.

1. Подождите несколько секунд и нажмите кнопку **Обновить**. Если устройство подтвердит эту операцию, появится значок синхронизации с флажком.

1. Вернитесь в окно консоли приложения отправки сообщений телеметрии. Вы увидите сообщения, отправленные с параметром `tracestate` в свойствах приложения.

    ![Состояние трассировки](./media/iot-hub-distributed-tracing/MicrosoftTeams-image.png)

1. (Необязательно) Измените частоту выборки и понаблюдайте за изменением частоты, с которой сообщения включают параметр `tracestate` в свойства приложения.

### <a name="update-using-azure-iot-hub-toolkit-for-vs-code"></a>Обновление с использованием набора средств Центра Интернета вещей Azure для VS Code

1. Установите VS Code, а затем перейдите на [эту страницу](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-tools) и установите последнюю версию набора средств Центра Интернета вещей Azure для VS Code.

1. Откройте VS Code и [настройте строку подключения к Центру Интернета вещей](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-toolkit#user-content-prerequisites).

1. Разверните устройство и найдите узел **Distributed Tracing Setting (Preview)** (Параметр распределенной трассировки (предварительная версия)). Ниже щелкните подузел **Update Distributed Tracing Setting (Preview)** (Обновление параметра распределенной трассировки (предварительная версия)).

    ![Включение распределенной трассировки в наборе средств Центра Интернета вещей Azure](./media/iot-hub-distributed-tracing/update-distributed-tracing-setting-1.png)

1. Во всплывающем окне выберите **Включить**, а затем нажмите клавишу ВВОД, чтобы подтвердить выборку с частотой 100.

    ![Обновление режима выборки](./media/iot-hub-distributed-tracing/update-distributed-tracing-setting-2.png)

    ![Обновление частоты выборки](./media/iot-hub-distributed-tracing/update-distributed-tracing-setting-3.png)

### <a name="bulk-update-for-multiple-devices"></a>Массовое обновление нескольких устройств

Чтобы обновить конфигурацию выборки распределенной трассировки нескольких устройств, используйте [автоматическую конфигурацию устройства](iot-hub-auto-device-config.md). Убедитесь, что вы следуете этой схеме двойника:

```json
{
    "properties": {
        "desired": {
            "azureiot*com^dtracing^1": {
                "sampling_mode": 1,
                "sampling_rate": 100
            }
        }
    }
}
```

| Имя элемента | Обязательно для заполнения | type | Описание |
|-----------------|----------|---------|-----------------------------------------------------|
| `sampling_mode` | Да | Целое число | Для включения и отключения выборки в настоящее время поддерживается два режима. `1` — включен, а `2` — отключен. |
| `sampling_rate` | Да | Целое число | Это значение является процентной долей. Разрешены только значения в диапазоне от `0` до `100` включительно.  |

## <a name="query-and-visualize"></a>Отправка запросов и визуализация

Чтобы просмотреть все трассировки, вносимые в Центр Интернета вещей, запросите хранилище журналов, выбранное в параметрах диагностики. В этом разделе рассматривается несколько различных вариантов.

### <a name="query-using-log-analytics"></a>Запрос с помощью Log Analytics

Если вы настроили [Log Analytics с журналами диагностики](../azure-monitor/platform/diagnostic-logs-stream-log-store.md), выполните запрос поиска журналов в категории `DistributedTracing`. Например, этот запрос показывает все трассировки в журнале:

```Kusto
// All distributed traces 
AzureDiagnostics 
| where Category == "DistributedTracing" 
| project TimeGenerated, Category, OperationName, Level, CorrelationId, DurationMs, properties_s 
| order by TimeGenerated asc  
```

Примеры журналов, как показано службой Log Analytics:

| TimeGenerated | OperationName | Категория | Уровень | CorrelationId | DurationMs | properties |
|--------------------------|---------------|--------------------|---------------|---------------------------------------------------------|------------|------------------------------------------------------------------------------------------------------------------------------------------|
| 2018-02-22T03:28:28.633Z | DiagnosticIoTHubD2C | DistributedTracing | Информация | 00-8cd869a412459a25f5b4f31311223344-0144d2590aacd909-01 |  | {"deviceId":"AZ3166","messageSize":"96","callerLocalTimeUtc":"2018-02-22T03:27:28.633Z","calleeLocalTimeUtc":"2018-02-22T03:27:28.687Z"} |
| 2018-02-22T03:28:38.633Z | DiagnosticIoTHubIngress | DistributedTracing | Информация | 00-8cd869a412459a25f5b4f31311223344-349810a9bbd28730-01 | 20 | {"isRoutingEnabled":"false","parentSpanId":"0144d2590aacd909"} |
| 2018-02-22T03:28:48.633Z | DiagnosticIoTHubEgress | DistributedTracing | Информация | 00-8cd869a412459a25f5b4f31311223344-349810a9bbd28730-01 | 23 | {"endpointType":"EventHub","endpointName":"myEventHub", "parentSpanId":"0144d2590aacd909"} |

Сведения о различных типах журналов см. в разделе [о журналах диагностики Центра Интернета вещей Azure](iot-hub-monitor-resource-health.md#distributed-tracing-preview).

### <a name="application-map"></a>Схема приложений

Чтобы визуализировать поток сообщений Интернета вещей, настройте пример приложения схемы приложений. Пример приложения отправляет журналы распределенной трассировки в [схему приложений](../application-insights/app-insights-app-map.md) с помощью функции Azure и концентратора событий.

> [!div class="button"]
> <a href="https://github.com/Azure-Samples/e2e-diagnostic-provision-cli" target="_blank">Получить пример когда на GitHub</a>

На изображении ниже показана распределенная трассировка в схеме приложений с тремя конечными точками маршрутизации:

![Распределенная трассировка Интернета вещей в схеме приложений](./media/iot-hub-distributed-tracing/app-map.png)

## <a name="understand-azure-iot-distributed-tracing"></a>Сведения о распределенной трассировке Интернета вещей Azure

### <a name="context"></a>Context

Множество решений Интернета вещей, включая нашу собственную [эталонную архитектуру](https://aka.ms/iotrefarchitecture) (только на английском языке) обычно являются вариантом [архитектуры микрослужб](https://docs.microsoft.com/azure/architecture/microservices/). По мере усложнения решений Интернета вещей вам придется использовать дюжину или более микрослужб. Эти микрослужбы могут располагаться как в Azure, так и за его пределами. Определение, где сообщения Интернета вещей удаляются или замедляются может стать сложной задачей. Например, у вас есть решение Интернета вещей, которое использует 5 различных служб Azure и 1500 активных устройств. Каждое устройство отправляет 10 сообщений с устройства в облако в секунду (всего 15 000 сообщений в секунду), однако вы обращаете внимание, что веб-приложение видит только 10 000 сообщений в секунду. В чем заключается проблема? Как найти причину?

### <a name="distributed-tracing-pattern-in-microservice-architecture"></a>Шаблон распределенной трассировки в архитектуре микрослужб

Чтобы воспроизвести поток сообщения Интернета вещей для разных служб, каждая служба должна распространять *идентификатор корреляции*, который однозначно определяет сообщение. После сбора в централизованную систему идентификаторы корреляции позволяют просматривать поток сообщений. Этот метод называется [шаблоном распределенной трассировки](https://docs.microsoft.com/azure/architecture/microservices/logging-monitoring#distributed-tracing).

Для поддержки более широкого внедрения распределенной трассировки, корпорация Майкрософт вносит свой вклад в разработку [стандартного предложения W3C по распределенной трассировке](https://w3c.github.io/trace-context/).

### <a name="iot-hub-support"></a>Поддержка Центра Интернета вещей

После включения поддержки распределенной трассировки для центра Интернета вещей следуйте этому потоку:

1. На устройстве Интернета вещей формируется сообщение.
1. Устройство Интернета вещей (с помощью облака) решает, что это сообщение должно назначаться с контекстом трассировки.
1. Пакет SDK добавляет `tracestate` в свойство сообщения приложения, содержащее отметку времени создания сообщения.
1. Устройство Интернета вещей отправляет сообщение в Центр Интернета вещей.
1. Сообщение поступает в шлюз Центра Интернета вещей.
1. Центр Интернета вещей выполняет поиск `tracestate` в свойствах приложения сообщения и проверяет, находится ли оно в правильном формате.
1. Если да, Центр Интернета вещей создает `trace-id` и `span-id`, и записывает их в журналы диагностики Azure Monitor в категории `DiagnosticIoTHubD2C`.
1. После завершения обработки сообщения, Центр Интернета вещей создает другое свойство `span-id` и записывает его в журнал наряду с существующим свойством `trace-id` категории `DiagnosticIoTHubIngress`.
1. Если для сообщения включена маршрутизация, Центр Интернета вещей записывает его в пользовательскую конечную точку и записывает в журнал другой `span-id` с тем же `trace-id` в категории `DiagnosticIoTHubEgress`.
1. Описанные выше действия повторяются для каждого созданного сообщения.

## <a name="public-preview-limits-and-considerations"></a>Ограничения общедоступной предварительной версии и рекомендации

- Предложения для стандартного контекста трассировки консорциума W3C в настоящее время является рабочим проектом.
- В настоящее время единственным языком разработки, поддерживаемым клиентским пакетом SDK является C.
- Возможность переноса из облака на двойник устройства недоступна на [уровне "Базовый" Центра Интернета вещей](iot-hub-scaling.md#basic-and-standard-tiers). Тем не менее, Центр Интернета вещей все равно будет регистрироваться в Azure Monitor, если видит правильно составленный заголовок контекста трассировки.
- Чтобы обеспечить эффективность операции, Центр Интернета вещей применит регулирование на основе частоты регистрации, которая может возникнуть как часть распределенной трассировки.

## <a name="next-steps"></a>Дальнейшие действия

- Дополнительные сведения об общем шаблоне распределенной трассировки в микрослужбах см. в [этой статье](https://microservices.io/patterns/observability/distributed-tracing.html).
- Чтобы настроить конфигурацию для применения параметров распределенной трассировки на большое количество устройств, ознакомьтесь со статьей [Настройка и мониторинг устройств Центра Интернета вещей с помощью портала Azure](iot-hub-auto-device-config.md).
- Дополнительные сведения о службе Azure см. в статье [Настройка и мониторинг устройств Центра Интернета вещей с помощью портала Azure](../azure-monitor/overview.md).
