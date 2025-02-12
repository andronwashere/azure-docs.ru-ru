---
title: Руководство. Создание пользовательского модуля Java — Azure IoT Edge | Документация Майкрософт
description: В этом руководстве показано, как создать модуль IoT Edge c кодом Java и развернуть его на пограничном устройстве.
services: iot-edge
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 04/04/2019
ms.topic: tutorial
ms.service: iot-edge
ms.custom: mvc, seodec18
ms.openlocfilehash: 95fab84fb6472339c9d6b847f8c09139b55e1466
ms.sourcegitcommit: 009334a842d08b1c83ee183b5830092e067f4374
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/29/2019
ms.locfileid: "66303832"
---
# <a name="tutorial-develop-a-java-iot-edge-module-for-linux-devices"></a>Руководство по Разработка модуля IoT Edge на языке Java для устройств Linux

Вы можете использовать модули Azure IoT Edge для развертывания кода, который реализует бизнес-логику непосредственно на устройствах IoT Edge. В этом руководстве рассматриваются создание и развертывание модуля IoT Edge, который фильтрует данные датчика. Вы будете использовать имитированное устройство IoT Edge, созданное с помощью процедуры развертывания Azure IoT Edge на имитированном устройстве, описанной в кратком руководстве для [Linux](quickstart-linux.md). Из этого руководства вы узнаете, как выполнять следующие задачи:    

> [!div class="checklist"]
> * Использовать Visual Studio Code для создания модуля IoT Edge Java на основе пакета шаблонов Maven для Azure IoT Edge и пакета SDK для устройств Интернета вещей Azure для Java.
> * Использовать Visual Studio Code и Docker для создания образа Docker и его публикации в реестре.
> * Развертывать модуль на устройстве IoT Edge.
> * Просматривать сформированные данные.


Модуль IoT Edge, создаваемый в этом руководстве, фильтрует данные температуры, которые создаются устройством. Оно отправляет сообщения, только если температура превышает заданное пороговое значение. Такой тип пограничного анализа удобен для сокращения объема данных, передаваемых в облако и сохраняемых в нем. 

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="solution-scope"></a>Область действия решения

В этом руководстве показана разработка модуля на языке **Java** с помощью **Visual Studio Code** и его развертывание на **устройстве Linux**. IoT Edge не поддерживает модули Java для устройств Windows. 

В следующей таблице перечислены возможные варианты для разработки и развертывания модулей Java. 

| Java | Visual Studio Code | Visual Studio 2017 или 2019 | 
| - | ------------------ | ------------------ |
| **Linux AMD64** | ![Использование VS Code для модулей Java в Linux AMD64](./media/tutorial-c-module/green-check.png) |  |
| **Linux ARM32** | ![Использование VS Code для модулей Java в Linux AMD32](./media/tutorial-c-module/green-check.png) |  |

## <a name="prerequisites"></a>Предварительные требования

Предполагается, что перед началом работы с этим руководством вы прошли предыдущее, в рамках которого настроили окружение для разработки контейнеров Linux: [Develop IoT Edge modules for Linux devices](tutorial-develop-for-linux.md) (Разработка модулей IoT Edge для устройств Linux). После работы с любым из учебников у вас должны быть готовы все необходимые компоненты: 

* [Центр Интернета вещей](../iot-hub/iot-hub-create-through-portal.md) ценовой категории "Бесплатный" или "Стандартный" в Azure.
* [устройство Linux, на котором выполняется Azure IoT Edge](quickstart-linux.md);
* реестр контейнеров, например [Реестр контейнеров Azure](https://docs.microsoft.com/azure/container-registry/);
* средство [Visual Studio Code](https://code.visualstudio.com/), настроенное с помощью [Azure IoT Tools](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-tools);
* выпуск [Docker CE](https://docs.docker.com/install/), настроенный для выполнения контейнеров Linux.

Для разработки модуля IoT Edge на языке Java установите на компьютере разработки следующие дополнительные компоненты. 

* [Пакет расширения Java](https://marketplace.visualstudio.com/items?itemName=vscjava.vscode-java-pack) для Visual Studio Code.
* [Пакет SDK для Java SE 10](https://aka.ms/azure-jdks) и [переменная среды `JAVA_HOME`,](https://docs.oracle.com/cd/E19182-01/820-7851/inst_cli_jdk_javahome_t/) которая указывает на эту установку JDK.
* [Maven](https://maven.apache.org/)

## <a name="create-a-module-project"></a>Создание проекта модуля
Ниже описана процедура создания проекта модуля IoT Edge на основе пакета шаблонов Maven для Azure IoT Edge и пакета SDK для устройств Интернета вещей Azure для Java. Для создания проекта используется Visual Studio Code и средства Azure IoT.

### <a name="create-a-new-project"></a>Создание нового проекта

Создайте шаблон решения Java, в который вы сможете поместить собственный код. 

1. В Visual Studio Code выберите **Представление** > **Палитра команд** для открытия палитры команд VS Code. 

2. В палитре команд введите и выполните команду **Azure IoT Edge: New IoT Edge Solution** (Azure IoT Edge: создать решение IoT Edge). Следуйте инструкциям на экране в палитре команд для создания решения.

   | Поле | Значение |
   | ----- | ----- |
   | Выбор папки | Выберите расположение на компьютере разработчика для VS Code, чтобы создать файлы решения. |
   | Введите название решения. | Введите описательное имя решения или примите имя по умолчанию **EdgeSolution**. |
   | Выбор шаблона модуля | Выберите **Java Module** (Модуль Java). |
   | Указание значения идентификатора группы | Введите значение идентификатора группы или примите значение по умолчанию **com.edgemodule**. |
   | Указание имени модуля | Присвойте модулю имя **JavaModule**. |
   | Указание репозитория изображений Docker для модуля | Репозиторий изображений включает в себя имя реестра контейнеров и имя образа контейнера. Образ контейнера предварительно заполняется именем, которое вы указали на последнем шаге. Замените **localhost:5000** на значение сервера входа из реестра контейнеров Azure. Вы можете извлечь сервер входа на странице "Обзор" реестра контейнеров на портале Azure. <br><br>Окончательный репозиторий образов выглядит так: \<имя_реестра\>.azurecr.io/javamodule. |
 
   ![Выбор репозитория образа Docker](./media/tutorial-java-module/repository.png)
   
Если вы впервые создаете модуль Java, может потребоваться несколько минут на скачивание пакетов Maven. После этого окно VS Code открывает рабочую область решения IoT Edge. Рабочая область решения содержит пять компонентов верхнего уровня. Папка **modules** содержит код Java для модуля и файлы Docker для создания модуля в качестве образа контейнера. Файл **\.env** хранит учетные данные реестра контейнеров. Файл **deployment.template.json** содержит сведения, которые среда выполнения IoT Edge использует для развертывания модулей на устройстве. Файл **deployment.debug.template.json** содержит отладочные версии модулей. Работая с этим руководством, вы не будете изменять папку **\.vscode** или файл **\.gitignore**. 

Если вы не указывали реестр контейнеров при создании решения, но приняли значение по умолчанию localhost:5000, у вас не будет файла \.env. 

### <a name="add-your-registry-credentials"></a>Добавление учетных данных реестра

Файл среды хранит учетные данные для реестра контейнеров и совместно использует их со средой выполнения IoT Edge. Среде выполнения нужны эти учетные данные, чтобы извлечь частный образ на устройство IoT Edge. 

1. Откройте в обозревателе VS Code файл с расширением ENV. 
2. Обновите поля с **именем пользователя** и **паролем**, скопированные из своего реестра контейнера Azure. 
3. Сохраните этот файл. 

### <a name="select-your-target-architecture"></a>Выбор целевой архитектуры

Сейчас Visual Studio Code позволяет разрабатывать модули Java для устройств Linux AMD64 и Linux ARM32v7. Для каждого решения вам нужно выбрать одну целевую архитектуру, так как сборка и запуск контейнера для разных архитектур различаются. По умолчанию используется Linux AMD64. 

1. Откройте палитру команд и выполните поиск **Azure IoT Edge: Set Default Target Platform for Edge Solution** (Azure IoT Edge: установить целевую платформу по умолчанию для решения Edge) или выберите значок ярлыка на боковой панели в нижней части окна. 

2. В палитре команд выберите целевую архитектуру из списка параметров. В рамках этого руководства мы используем в качестве устройства IoT Edge виртуальную машину Ubuntu. Поэтому сохраним значение по умолчанию **amd64**. 

### <a name="update-the-module-with-custom-code"></a>Обновление модуля с помощью пользовательского кода

1. В обозревателе VS Code откройте **модули** > **JavaModule** > **src** > **main**  >  **java** > **com** > **edgemodule** > **App.java**.

2. Добавьте в начало файла следующий код, который импортирует новые ссылочные классы.

    ```java
    import java.io.StringReader;
    import java.util.concurrent.atomic.AtomicLong;
    import java.util.HashMap;
    import java.util.Map;
    
    import javax.json.Json;
    import javax.json.JsonObject;
    import javax.json.JsonReader;
    
    import com.microsoft.azure.sdk.iot.device.DeviceTwin.Pair;
    import com.microsoft.azure.sdk.iot.device.DeviceTwin.Property;
    import com.microsoft.azure.sdk.iot.device.DeviceTwin.TwinPropertyCallBack;
    ```

3. Добавьте следующее определение в класс **App**. Эта переменная устанавливает пороговое значение температуры. Измеренное значение температуры компьютера отправляется в Центр Интернета вещей, только если оно превышает порог. 

    ```java
    private static final String TEMP_THRESHOLD = "TemperatureThreshold";
    private static AtomicLong tempThreshold = new AtomicLong(25);
    ```

4. В объекте **MessageCallbackMqtt** замените метод execute следующим кодом. Этот метод вызывается каждый раз, когда модуль получает сообщение MQTT из центра IoT Edge. Он отфильтровывает сообщения о температуре ниже порогового значения, настроенного с помощью двойника модуля.

    ```java
    protected static class MessageCallbackMqtt implements MessageCallback {
        private int counter = 0;
        @Override
        public IotHubMessageResult execute(Message msg, Object context) {
            this.counter += 1;
 
            String msgString = new String(msg.getBytes(), Message.DEFAULT_IOTHUB_MESSAGE_CHARSET);
            System.out.println(
                   String.format("Received message %d: %s",
                            this.counter, msgString));
            if (context instanceof ModuleClient) {
                try (JsonReader jsonReader = Json.createReader(new StringReader(msgString))) {
                    final JsonObject msgObject = jsonReader.readObject();
                    double temperature = msgObject.getJsonObject("machine").getJsonNumber("temperature").doubleValue();
                    long threshold = App.tempThreshold.get();
                    if (temperature >= threshold) {
                        ModuleClient client = (ModuleClient) context;
                        System.out.println(
                            String.format("Temperature above threshold %d. Sending message: %s",
                            threshold, msgString));
                        client.sendEventAsync(msg, eventCallback, msg, App.OUTPUT_NAME);
                    }
                } catch (Exception e) {
                    e.printStackTrace();
                }
            }
            return IotHubMessageResult.COMPLETE;
        }
    }
    ```

5. Добавьте следующие два внутренних статических класса в класс **App**. Эти классы обновляют переменную tempThreshold при изменении требуемого свойства двойника модуля. У каждого модуля есть собственный модуль-двойник, что позволяет настроить код, выполняемый в модуле, непосредственно из облака.

    ```java
    protected static class DeviceTwinStatusCallBack implements IotHubEventCallback {
        @Override
        public void execute(IotHubStatusCode status, Object context) {
            System.out.println("IoT Hub responded to device twin operation with status " + status.name());
        }
    }
 
    protected static class OnProperty implements TwinPropertyCallBack {
        @Override
        public void TwinPropertyCallBack(Property property, Object context) {
            if (!property.getIsReported()) {
                if (property.getKey().equals(App.TEMP_THRESHOLD)) {
                    try {
                        long threshold = Math.round((double) property.getValue());
                        App.tempThreshold.set(threshold);
                    } catch (Exception e) {
                        System.out.println("Faile to set TemperatureThread with exception");
                        e.printStackTrace();
                    }
                }
            }
        }
    }
    ```

6. Добавьте следующие строки в метод **main** после строки **client.open()** , чтобы подписаться на обновления двойника модуля.

    ```java
    client.startTwin(new DeviceTwinStatusCallBack(), null, new OnProperty(), null);
    Map<Property, Pair<TwinPropertyCallBack, Object>> onDesiredPropertyChange = new HashMap<Property, Pair<TwinPropertyCallBack, Object>>() {
        {
            put(new Property(App.TEMP_THRESHOLD, null), new Pair<TwinPropertyCallBack, Object>(new OnProperty(), null));
        }
    };
    client.subscribeToTwinDesiredProperties(onDesiredPropertyChange);
    client.getTwin();
    ```

7. Сохраните файл App.java.

8. В обозревателе VS Code откройте файл **deployment.template.json** в рабочей области решения IoT Edge.

9. Добавьте двойник модуля **JavaModule** в манифест развертывания. Вставьте следующее содержимое JSON в нижней части раздела **moduleContent** после двойника модуля **$edgeHub**: 

   ```json
     "JavaModule": {
         "properties.desired":{
             "TemperatureThreshold":25
         }
     }
   ```

   ![Добавление двойника модуля в шаблон развертывания](./media/tutorial-java-module/module-twin.png)

10. Сохраните файл deployment.template.json.

## <a name="build-and-push-your-module"></a>Сборка и отправка модуля

В предыдущем разделе вы создали решение IoT Edge и добавили в **JavaModule** код для фильтрации сообщений, в которых указана температура компьютера ниже допустимого лимита. Теперь создайте решение в качестве образа контейнера и передайте его в реестр контейнеров. 

1. Откройте интегрированный терминал VS Code, выбрав **Вид** > **Терминал**.

1. Войдите в Docker, введя следующую команду в окне терминала. Выполните вход в систему, используя имя пользователя, пароль и сервер входа из реестра контейнеров Azure. Вы можете получить эти значения в разделе **Ключи доступа** в разделе реестра на портале Azure.
     
   ```bash
   docker login -u <ACR username> -p <ACR password> <ACR login server>
   ```

   Возможно, появится предупреждение системы безопасности, в котором рекомендуется использовать `--password-stdin`. Для рабочих сценариев это лучшая методика, но мы не будем рассматривать ее в этом учебнике. Дополнительные сведения см. в описании команды [docker login](https://docs.docker.com/engine/reference/commandline/login/#provide-a-password-using-stdin) в справочнике.

2. В обозревателе VS Code щелкните правой кнопкой мыши файл **deployment.template.json** и выберите **Build and Push IoT Edge solution** (Создать и отправить решение IoT Edge).

   Эта команда сборки и отправки позволяет запустить три операции. Во-первых,в решении создается папка с именем **config**, которая содержит полный манифест развертывания на основе информации из шаблона развертывания и других файлов решения. Во-вторых, выполняется `docker build` для сборки образа контейнера на основе подходящего файла dockerfile для целевой архитектуры. В-третьих, выполняется `docker push` для отправки образа в реестр контейнеров.

## <a name="deploy-modules-to-device"></a>Развертывание модулей на устройстве

Разверните проект модуля на устройстве IoT Edge с помощью обозревателя Visual Studio Code и расширения Azure IoT Tools. У вас уже есть манифест развертывания, подготовленный для вашего сценария (файл **deployment.json** в папке config). Теперь вам осталось выбрать устройство для получения развертывания.

Убедитесь, что устройство IoT Edge работает.

1. В обозревателе Visual Studio Code разверните раздел **Azure IoT Hub Devices** (Устройства Центра Интернета вещей Azure), чтобы отобразился список устройство Интернета вещей.

2. Щелкните имя устройства IoT Edge правой кнопкой мыши, а затем выберите **Create Deployment for Single Device** (Создание развертывания для одного устройства).

3. Выберите файл **deployment.json** в папке **config** и щелкните **Select Edge Deployment Manifest** (Выбрать манифест развертывания Edge). Не используйте файл deployment.template.json.

4. Нажмите кнопку "Обновить". Появится новый работающий модуль **JavaModule**, а также модули **TempSensor**, **$edgeAgent** и **$edgeHub**.  

## <a name="view-generated-data"></a>Просмотр сформированных данных

Когда вы примените манифест развертывания к устройству IoT Edge, в среде выполнения IoT Edge на устройстве начнется сбор сведений о новом развертывании и выполнение операций. Работа всех выполняющихся на устройстве модулей, которые не включены в манифест развертывания, будет остановлена. А модули, которых нет на устройстве, будут запущены.

Сведения о состоянии устройства IoT Edge можно просмотреть в разделе **Azure IoT Hub Devices** (Устройства Центра Интернета вещей Azure) в обозревателе Visual Studio Code. Разверните раздел со сведениями об устройстве, чтобы просмотреть список развернутых и запущенных модулей.

1. В обозревателе Visual Studio Code щелкните имя устройства IoT Edge правой кнопкой мыши и выберите **Start Monitoring Built-in Event Endpoint** (Начать мониторинг строенной конечной точки событий).

2. Просмотрите сообщения, поступающие в Центр Интернета вещей. Для поступления сообщений может потребоваться некоторое время, так как устройство IoT Edge должно сначала получить новое развертывание и запустить все модули. После этого измененный нами код JavaModule ожидает, пока температура компьютера не достигнет 25 градусов, и лишь затем отправляет сообщения. Он также присваивает тип **Предупреждение** всем сообщениям, которые сообщают о достижении порогового значения температуры. 

## <a name="edit-the-module-twin"></a>Изменение двойника модуля

Мы использовали двойник модуля JavaModule в манифесте развертывания, чтобы задать порог температуры на уровне 25 градусов. С помощью двойника модуля вы можете изменять функциональные возможности без необходимости обновлять код модуля.

1. В Visual Studio Code разверните подробные сведения об устройстве IoT Edge, чтобы просмотреть список выполняющихся модулей. 

2. Щелкните **JavaModule** правой кнопкой мыши и выберите **Edit module twin** (Изменение двойника модуля). 

3. В списке требуемых свойств найдите **TemperatureThreshold**. Измените его значение, увеличив температуру на 5 или 10 градусов относительно последней сообщаемой температуры. 

4. Сохраните файл двойника модуля.

5. Щелкните правой кнопкой мыши в любой точки на панели редактирования двойника модуля и выберите **Update module twin** (Обновление двойника модуля). 

6. Выполните мониторинг сообщений, поступающих с устройства в облако. Вы увидите, что поток сообщений прекратится, пока не будет достигнут новый порог температуры. 

## <a name="clean-up-resources"></a>Очистка ресурсов 

Если вы планируете перейти к следующей рекомендуемой статье, можно сохранить созданные и повторно используемые ресурсы и конфигурации. Это же устройство IoT Edge также можно использовать в качестве тестового устройства. 

В противном случае чтобы избежать расходов, можно удалить локальные конфигурации и ресурсы Azure, созданные в рамках этой статьи. 

[!INCLUDE [iot-edge-clean-up-cloud-resources](../../includes/iot-edge-clean-up-cloud-resources.md)]

## <a name="next-steps"></a>Дополнительная информация

В этом руководстве вы создали модуль IoT Edge, который содержит код для фильтрации необработанных данных, созданных вашим устройством IoT Edge. Дополнительные сведения см. в статьях о [разработке модулей IoT Edge](module-development.md) и [разработке модулей с помощью Visual Studio Code](how-to-vs-code-develop-module.md). Перейдите к следующим руководствам, чтобы узнать, как Azure IoT Edge поможет развернуть облачные службы Azure для обработки данных на пограничном устройстве.

> [!div class="nextstepaction"]
> [Функции](tutorial-deploy-function.md)
> [Stream Analytics](tutorial-deploy-stream-analytics.md)
> [Машинное обучение](tutorial-deploy-machine-learning.md)
> [Служба пользовательского визуального распознавания](tutorial-deploy-custom-vision.md)
