---
title: Руководство по развертыванию классификатора Пользовательской службы визуального распознавания на устройстве в Azure IoT Edge | Документация Майкрософт
description: Из этого руководства вы узнайте, как создать модель компьютерного зрения в качестве контейнера с помощью Пользовательской службы визуального распознавания и IoT Edge.
services: iot-edge
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 06/25/2019
ms.topic: tutorial
ms.service: iot-edge
ms.custom: mvc, seodec18
ms.openlocfilehash: 629b484d27d863727d180bb3e2d01b605ca539a6
ms.sourcegitcommit: fa45c2bcd1b32bc8dd54a5dc8bc206d2fe23d5fb
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/12/2019
ms.locfileid: "67850120"
---
# <a name="tutorial-perform-image-classification-at-the-edge-with-custom-vision-service"></a>Руководство по классификации изображений в пограничной системе с помощью Пользовательской службы визуального распознавания

Azure IoT Edge может сделать решение Интернета вещей более эффективным, перемещая рабочие нагрузки из облака в пограничную систему. Эта возможность хорошо подходит для служб, которые обрабатывают много данных, таких как модели компьютерного зрения. [Пользовательская служба визуального распознавания](../cognitive-services/custom-vision-service/home.md) позволяет создавать пользовательские классификаторы изображений и развертывать их на устройствах в качестве контейнеров. Вместе эти две службы позволяют находить информацию из изображений или видеопотоков, не перенося все данные с сайта. Пользовательское визуальное распознавание предоставляет классификатор, сравнивающий изображение с обученной моделью для создания аналитических данных.

Например, Пользовательское визуальное распознавание на устройстве Edge IoT может определить, присутствует ли на шоссе более высокий или низкий уровень транспорта, чем обычно, или имеется ли в гараже парковочные места в ряду. Эти сведения можно использовать совместно с другой службой для принятия мер.

Из этого руководства вы узнаете, как выполнять следующие задачи:

> [!div class="checklist"]
>
> * Создание классификатора изображений с помощью Пользовательского визуального распознавания.
> * Разработка модуля IoT Edge, который запрашивает веб-сервер Пользовательского визуального распознавания на вашем устройстве.
> * Отправка результатов классификатора изображений в Центр Интернета вещей.

<center>

![Схема рассматриваемой в руководстве архитектуры, а также размещение и развертывание классификатора](./media/tutorial-deploy-custom-vision/custom-vision-architecture.png)
</center>

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Предварительные требования

Предполагается, что перед началом работы с этим руководством вы прошли предыдущее, в рамках которого настроили окружение для разработки контейнеров Linux: [Tutorial: Develop IoT Edge modules for Linux devices](tutorial-develop-for-linux.md) (Руководство. Разработка модулей IoT Edge для устройств с Linux). После работы с ним у вас должны быть готовы все необходимые компоненты: 

* [Центр Интернета вещей](../iot-hub/iot-hub-create-through-portal.md) ценовой категории "Бесплатный" или "Стандартный" в Azure.
* [устройство Linux, на котором выполняется Azure IoT Edge](quickstart-linux.md);
* реестр контейнеров, например [Реестр контейнеров Azure](https://docs.microsoft.com/azure/container-registry/);
* средство [Visual Studio Code](https://code.visualstudio.com/), настроенное с помощью [Azure IoT Tools](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-tools);
* выпуск [Docker CE](https://docs.docker.com/install/), настроенный для выполнения контейнеров Linux.

Для разработки модуля IoT Edge со службой "Пользовательское визуальное распознавание" установите на компьютере разработки следующие дополнительные компоненты: 

* [Python](https://www.python.org/downloads/)
* [Git](https://git-scm.com/downloads)
* Расширение Visual Studio Code для [Python](https://marketplace.visualstudio.com/items?itemName=ms-python.python).

## <a name="build-an-image-classifier-with-custom-vision"></a>Создание классификатора изображений с помощью Пользовательского визуального распознавания

Для создания классификатора изображений необходимо создать проект Пользовательского визуального распознавания и предоставить изображения для обучения. Дополнительные сведения о шагах в этом разделе см. в статье [Как создать классификатор с помощью Пользовательской службы визуального распознавания](../cognitive-services/custom-vision-service/getting-started-build-a-classifier.md).

После создания и обучения классификатора изображений можно экспортировать его как контейнер Docker и развернуть на устройстве IoT Edge. 

### <a name="create-a-new-project"></a>Создание нового проекта

1. В веб-браузере перейдите на веб-страницу [Пользовательской службы визуального распознавания](https://customvision.ai/).

2. Выберите **Вход** и войдите с помощью той же учетной записи, которая используется для доступа к ресурсам Azure. 

3. Выберите **Создать проект**.

4. Создайте проект со следующими значениями:

   | Поле | Значение |
   | ----- | ----- |
   | ИМЯ | Укажите имя проекта, например **EdgeTreeClassifier**. |
   | ОПИСАНИЕ | Необязательное описание проекта. |
   | Группа ресурсов | Выберите одну из групп ресурсов Azure, включающую ресурс службы "Пользовательское визуальное распознавание", или **создайте** группу, если вы ее еще не добавили. |
   | Типы проектов | **Классификация** |
   | Типы классификации | **Multiclass (single tag per image)** (Многоклассовая классификация (один тег на изображение)) |
   | Домены | **General (compact)** (Общий (компактный)) |
   | Возможности экспорта | **Основные платформы (Tensorflow, CoreML, ONNX и т. д.)** |

5. Щелкните **Создать проект**.

### <a name="upload-images-and-train-your-classifier"></a>Загрузка изображений и обучение классификатора

Для создания классификатора изображений требуется набор обучающих и тестовых изображений. 

1. Клонируйте или скачайте примеры изображений из репозитория [Cognitive-CustomVision-Windows](https://github.com/Microsoft/Cognitive-CustomVision-Windows) на локальном компьютере разработки. 

   ```cmd/sh
   git clone https://github.com/Microsoft/Cognitive-CustomVision-Windows.git
   ```

2. Вернитесь к проекту Пользовательского визуального распознавания и щелкните **Добавить изображения**. 

3. Перейдите в репозиторий git, клонированный локально, и перейдите к первой папке изображений **Cognitive-CustomVision-Windows / Samples / Images / Hemlock**. Выберите все 10 изображений в папке, а затем **откройте**. 

4. Добавьте тэг **hemlock** для этой группы изображений и нажмите клавишу **ВВОД** для применения тега. 

5. Нажмите кнопку **Upload 10 files** (Передать 10 файлов). 

   ![Передача файлов с тегом hemlock в Пользовательскую службу визуального распознавания](./media/tutorial-deploy-custom-vision/upload-hemlock.png)

6. Когда изображения успешно загрузятся, нажмите кнопку **Готово**.

7. Щелкните **Добавить изображения** снова.

8. Перейдите ко второй папке изображений **Cognitive-CustomVision-Windows / Samples / Images / Japanese Cherry**. Выберите все 10 изображений в папке, а затем **откройте**. 

9. Добавьте тэг **japanese cherry** для этой группы изображений и нажмите клавишу **ВВОД** для применения тега. 

10. Нажмите кнопку **Upload 10 files** (Передать 10 файлов). Когда изображения успешно загрузятся, нажмите кнопку **Готово**. 

11. Когда оба набора изображений помечены и загружены, выберите **Обучение** для обучения классификатора. 

### <a name="export-your-classifier"></a>Экспорт классификатора

1. После тренировки классификатора щелкните **Экспортировать** на странице производительности классификатора. 

   ![Экспорт обученного классификатора изображений](./media/tutorial-deploy-custom-vision/export.png)

2. Выберите **DockerFile** в качестве платформы. 

3. Укажите **Linux** при выборе версии.  

4. Щелкните **Экспортировать**. 

   ![Экспорт как файл DockerFile с контейнерами Linux](./media/tutorial-deploy-custom-vision/export-2.png)

5. После завершения экспорта нажмите кнопку **Скачать** и сохраните ZIP-пакет локально на вашем компьютере. Извлеките все файлы из пакета. Эти файлы будут использоваться для создания модуля Edge IoT, содержащего сервер классификации изображений. 

Когда достигнете этого момента, вы закончили создание и обучение вашего проекта Пользовательского визуального распознавания. Экспортированные файлы будут использоваться в следующем разделе, но веб-страница Пользовательского визуального распознавания готова. 

## <a name="create-an-iot-edge-solution"></a>Создание решения IoT Edge

Теперь у вас есть файлы для версии контейнера вашего классификатора изображений на компьютере разработчика. В этом разделе вы настраиваете контейнер классификатора образов для запуска в качестве модуля Edge IoT. Кроме того, создается второй модуль, который будет развертываться вместе с классификатором изображений. Второй модуль выполняет запросы к классификатору и отправляет результаты в виде сообщений в Центр Интернета вещей. 

### <a name="create-a-new-solution"></a>Создание решения

Решение — это логический способ разработки и организации нескольких модулей для одного развертывания IoT Edge. Решение содержит код для одного или нескольких модулей, а также манифест развертывания, который объявляет, как их настроить на устройстве IoT Edge. 

1. В Visual Studio Code выберите **Вид** > **Терминал**, чтобы открыть интегрированный терминал VS Code.

1. Выберите **Представление** > **Палитра команд** для открытия палитры команд VS Code. 

1. В палитре команд введите и выполните команду **Azure IoT Edge: New IoT Edge Solution** (Azure IoT Edge: создать решение IoT Edge). В палитре команд укажите следующие сведения для создания решения: 

   | Поле | Значение |
   | ----- | ----- |
   | Выбор папки | Выберите расположение на компьютере разработчика для VS Code, чтобы создать файлы решения. |
   | Введите название решения. | Введите описательное имя для решения, например **CustomVisionSolution**, или примите значение по умолчанию. |
   | Выбор шаблона модуля | Выберите **Модуль Python**. |
   | Указание имени модуля | Присвойте модулю имя **classifier**.<br><br>Важно, чтобы буквы в имени были строчными. При обращении к модулям IoT Edge учитывается регистр. В этом решении используется библиотека, которая форматирует все запросы в нижнем регистре. |
   | Указание репозитория изображений Docker для модуля | Репозиторий изображений включает в себя имя реестра контейнеров и имя образа контейнера. Образ контейнера предварительно заполняется на последнем шаге. Замените **localhost:5000** на значение сервера входа из реестра контейнеров Azure. Вы можете извлечь сервер входа на странице "Обзор" реестра контейнеров на портале Azure. Окончательная строка выглядит так: \<registry name\>.azurecr.io/classifier. |
 
   ![Выбор репозитория образа Docker](./media/tutorial-deploy-custom-vision/repository.png)

После этого окно VS Code открывает рабочую область решения IoT Edge.

### <a name="add-your-registry-credentials"></a>Добавление учетных данных реестра

Файл среды хранит учетные данные для реестра контейнеров и совместно использует их со средой выполнения IoT Edge. Среде выполнения нужны эти учетные данные, чтобы извлечь частный образ на устройство IoT Edge.

1. Откройте в обозревателе VS Code файл с расширением ENV.
2. Обновите поля с **именем пользователя** и **паролем**, скопированные из своего реестра контейнера Azure.
3. Сохраните этот файл.

### <a name="select-your-target-architecture"></a>Выбор целевой архитектуры

Сейчас Visual Studio Code позволяет разрабатывать модули для устройств Linux AMD64 и Linux ARM32v7. Для каждого решения вам нужно выбрать одну целевую архитектуру, так как сборка и запуск контейнера для разных архитектур различаются. По умолчанию используется Linux AMD64. 

1. Откройте палитру команд и выполните поиск **Azure IoT Edge: Set Default Target Platform for Edge Solution** (Azure IoT Edge: установить целевую платформу по умолчанию для решения Edge) или выберите значок ярлыка на боковой панели в нижней части окна. 

2. В палитре команд выберите целевую архитектуру из списка параметров. В рамках этого руководства в качестве устройства IoT Edge мы используем виртуальную машину Ubuntu, поэтому сохраним значение по умолчанию **amd64**. 

### <a name="add-your-image-classifier"></a>Добавление классификатора изображений

Шаблон модуля Python в Visual Studio Сode содержит пример кода, который можно запустить для проверки IoT Edge. Этот код не будет использоваться в этом сценарии. Вместо этого выполните действия, описанные в этом разделе, чтобы заменить пример кода контейнером классификатора изображений, который был экспортирован ранее. 

1. В обозревателе файлов перейдите к скачанному и извлеченному пакету Пользовательского визуального распознавания. Скопируйте все содержимое из извлеченного пакета. Это должны быть две папки, **app** и **azureml**, и два файла, **Dockerfile** и **README**. 

2. В обозревателе файлов перейдите в каталог, в котором вы сообщили Visual Studio Code создать решение Edge IoT. 

3. Откройте папку модуля классификатора. Если вы использовали предложенные имена в предыдущем разделе, структура папок будет выглядеть так: **CustomVisionSolution / modules / classifier**. 

4. Вставьте файлы в папку **classifier**. 

5. Вернитесь к окну Visual Studio Code. В рабочей области решения теперь должны отображаться файлы классификатора изображений в папке модуля. 

   ![Рабочая область решения с файлами классификатора изображений](./media/tutorial-deploy-custom-vision/workspace.png)

6. Откройте файл **module.json** в папке классификатора. 

7. Обновите параметр **platforms**, чтобы он указывал на новый добавленный Dockerfile и удалил параметры архитектуры ARM32 и AMD64.debug, которые в настоящее время не поддерживаются для модуля Пользовательского визуального распознавания. 

   ```json
   "platforms": {
       "amd64": "./Dockerfile"
   }
   ```

8. Сохраните изменения. 

### <a name="create-a-simulated-camera-module"></a>Создание моделируемого модуля камеры

В реальном развертывании Пользовательского визуального распознавания у вас будет камера, предоставляющая живые изображения или видеопотоки. В этом сценарии вы моделируете камеру путем создания модуля, отправляющего тестовое изображение в классификатор изображений. 

#### <a name="add-and-configure-a-new-module"></a>Добавление и настройка нового модуля

В этом разделе вы добавляете новый модуль к тому же CustomVisionSolution и предоставляете код для создания моделируемой камеры. 

1. В том же окне Visual Studio Code используйте палитру команд для выполнения команды **Azure IoT Edge: Add IoT Edge Module** (Azure IoT Edge: добавить модуль IoT Edge). В палитре команд укажите следующую информацию для нового модуля: 

   | prompt | Значение | 
   | ------ | ----- |
   | Выбор файла шаблона развертывания | Выберите файл deployment.template.json в папке CustomVisionSolution. |
   | Выбор шаблона модуля | Выберите **Модуль Python**. |
   | Указание имени модуля | Назовите модуль **cameraCapture**. |
   | Указание репозитория изображений Docker для модуля | Замените **localhost: 5000** на значение сервера входа из реестра контейнеров Azure. Окончательная строка выглядит так: **\<registry name\>.azurecr.io/cameracapture**. |

   В окне VS Code загружается новый модуль в рабочей области решения и обновляется файл deployment.template.json. Теперь вы должны увидеть две папки модуля: classifier и cameraCapture. 

2. Откройте файл **main.py** в папке **modules** / **cameraCapture**. 

3. Замените весь файл следующим кодом. Этот пример кода отправляет запросы POST в службу обработки изображений, работающую в модуле классификатора. Мы предоставляем этот модуль контейнера с примером изображения для использования в запросах. Затем он упаковывает ответ в качестве сообщения Центра Интернета вещей и отправляет его в выходную очередь.  

    ```python
    # Copyright (c) Microsoft. All rights reserved.
    # Licensed under the MIT license. See LICENSE file in the project root for
    # full license information.

    import time
    import sys
    import os
    import requests
    import json

    import iothub_client
    # pylint: disable=E0611
    from iothub_client import IoTHubModuleClient, IoTHubClientError, IoTHubTransportProvider
    from iothub_client import IoTHubMessage, IoTHubMessageDispositionResult, IoTHubError
    # pylint: disable=E0401

    # messageTimeout - the maximum time in milliseconds until a message times out.
    # The timeout period starts at IoTHubModuleClient.send_event_async.
    MESSAGE_TIMEOUT = 10000

    # Choose HTTP, AMQP or MQTT as transport protocol.  
    PROTOCOL = IoTHubTransportProvider.MQTT

    # global counters
    SEND_CALLBACKS = 0

    # Send a message to IoT Hub
    # Route output1 to $upstream in deployment.template.json
    def send_to_hub(strMessage):
        message = IoTHubMessage(bytearray(strMessage, 'utf8'))
        hubManager.send_event_to_output("output1", message, 0)

    # Callback received when the message that we send to IoT Hub is processed.
    def send_confirmation_callback(message, result, user_context):
        global SEND_CALLBACKS
        SEND_CALLBACKS += 1
        print ( "Confirmation received for message with result = %s" % result )
        print ( "   Total calls confirmed: %d \n" % SEND_CALLBACKS )

    # Send an image to the image classifying server
    # Return the JSON response from the server with the prediction result
    def sendFrameForProcessing(imagePath, imageProcessingEndpoint):
        headers = {'Content-Type': 'application/octet-stream'}

        with open(imagePath, mode="rb") as test_image:
            try:
                response = requests.post(imageProcessingEndpoint, headers = headers, data = test_image)
                print("Response from classification service: (" + str(response.status_code) + ") " + json.dumps(response.json()) + "\n")
            except Exception as e:
                print(e)
                print("Response from classification service: (" + str(response.status_code))

        return json.dumps(response.json())

    class HubManager(object):
        def __init__(self, protocol, message_timeout):
            self.client_protocol = protocol
            self.client = IoTHubModuleClient()
            self.client.create_from_environment(protocol)
            # set the time until a message times out
            self.client.set_option("messageTimeout", message_timeout)
            
        # Sends a message to an output queue, to be routed by IoT Edge hub. 
        def send_event_to_output(self, outputQueueName, event, send_context):
            self.client.send_event_async(
                outputQueueName, event, send_confirmation_callback, send_context)

    def main(imagePath, imageProcessingEndpoint):
        try:
            print ( "Simulated camera module for Azure IoT Edge. Press Ctrl-C to exit." )

            try:
                global hubManager 
                hubManager = HubManager(PROTOCOL, MESSAGE_TIMEOUT)
            except IoTHubError as iothub_error:
                print ( "Unexpected error %s from IoTHub" % iothub_error )
                return

            print ( "The sample is now sending images for processing and will indefinitely.")

            while True:
                classification = sendFrameForProcessing(imagePath, imageProcessingEndpoint)
                send_to_hub(classification)
                time.sleep(10)

        except KeyboardInterrupt:
            print ( "IoT Edge module sample stopped" )

    if __name__ == '__main__':
        try:
            # Retrieve the image location and image classifying server endpoint from container environment
            IMAGE_PATH = os.getenv('IMAGE_PATH', "")
            IMAGE_PROCESSING_ENDPOINT = os.getenv('IMAGE_PROCESSING_ENDPOINT', "")
        except ValueError as error:
            print ( error )
            sys.exit(1)

        if ((IMAGE_PATH and IMAGE_PROCESSING_ENDPOINT) != ""):
            main(IMAGE_PATH, IMAGE_PROCESSING_ENDPOINT)
        else: 
            print ( "Error: Image path or image-processing endpoint missing" )
    ```

4. Сохраните файл **main.py**. 

5. Откройте файл **requrements.txt**. 

6. Добавьте новую строку для библиотеки, включаемой в контейнер.

   ```Text
   requests
   ```

7. Сохраните файл **requirements.txt** .


#### <a name="add-a-test-image-to-the-container"></a>Добавление тестового изображения в контейнер

Вместо того чтобы использовать реальную камеру для предоставления изображения для этого сценария, мы собираемся использовать одно тестовое изображение. Тестовое изображение включено в скачанный репозиторий GitHub, ранее описанный в этом руководстве. 

1. Выберите тестовое изображение, расположенное в каталоге по такому пути: **Cognitive-CustomVision-Windows** / **Примеры** / **Изображения** / **Тестировать**. 

2. Скопируйте файл **test_image.jpg**. 

3. Перейдите к каталогу решения Edge IoT и вставьте тестовое изображение в папку**modules** / **cameraCapture**. Изображение должно находиться в той же папке, что и основной файл main.py, который вы изменили в предыдущем разделе. 

3. В Visual Studio Code откройте файл **Dockerfile.amd64** модуля cameraCapture. (ARM32 в настоящее время не поддерживается модулем Пользовательского визуального распознавания). 

4. После строки, которая устанавливает рабочий каталог, `WORKDIR /app`, добавьте следующую строку кода: 

   ```Dockerfile
   ADD ./test_image.jpg .
   ```

5. Сохраните файл Dockerfile. 

### <a name="prepare-a-deployment-manifest"></a>Подготовка манифеста развертывания

До сих пор в этом руководстве вы обучали модель Пользовательского визуального распознавания для классификации изображений деревьев и упаковали ее, как модуль Edge IoT. Затем был создан второй модуль, который может запросить сервер классификации изображений и сообщить о результатах обратно в Центр Интернета вещей. Теперь вы готовы создать манифест развертывания, который расскажет устройству Edge IoT о том, как запускать и выполнять эти два модуля вместе. 

Расширение Edge IoT для Visual Studio Code предоставляет шаблон в каждом решении Edge IoT, помогающем создать манифест развертывания. 

1. Откройте файл **deployment.template.json** в папке решения. 

2. Найдите раздел **modules**, который должен содержать три модуля: два, которые вы создали, classifier и cameraCapture, и третий, включенный по умолчанию, tempSensor. 

3. Удалите модуль **tempSensor** со всеми параметрами. Этот модуль включен для предоставления примеров данных для тестовых сценариев, но в этом развертывании он не нужен. 

4. Если дали модулю классификации изображений имя, отличное от **classifier**, проверьте имя и убедитесь, что все буквы строчные. Модуль cameraCapture вызывает модуль классификатора с помощью библиотеки запросов, которая форматирует все запросы в нижнем регистре, а Edge IoT учитывает регистр. 

5. Обновите параметр **createOptions** для модуля cameraCapture следующим кодом JSON. Эта информация создает переменные среды в контейнере модуля, которые извлекаются в процессе main.py. Включив эти сведения в манифест развертывания, можно изменить изображение или конечную точку без необходимости перестроения образа модуля. 

    ```json
    "createOptions": "{\"Env\":[\"IMAGE_PATH=test_image.jpg\",\"IMAGE_PROCESSING_ENDPOINT=http://classifier/image\"]}"
    ```

    Если дали модулю Пользовательского визуального распознавания имя, отличное от *classifier*, обновите значение конечной точки обработки изображения, чтобы оно соответствовало имени. 

5. В нижней части файла обновите параметр **routes** для модуля $edgeHub. Необходимо направить результаты прогнозирования из cameraCapture в Центр Интернета вещей. 

    ```json
        "routes": {
          "CameraCaptureToIoTHub": "FROM /messages/modules/cameraCapture/outputs/* INTO $upstream"
        },
    ```

    Если дали второму модулю имя, отличное от *cameraCapture*, обновите значение маршрута для соответствия. 

7. Сохраните файл **deployment.template.json**.

## <a name="build-and-deploy-your-iot-edge-solution"></a>Сборка и развертывание решения IoT Edge

Если оба модуля созданы и настроен шаблон манифеста развертывания, можно создать образы контейнеров и передать их в реестр контейнеров. 

После того как образ находятся в реестре, можно развернуть решение на устройстве IoT Edge. Модули на устройстве можно задать с помощью Центра Интернета вещей, однако доступ к Центру Интернета вещей и устройствам можно также получить через Visual Studio Code. В этом разделе настраивается доступ к Центру Интернета вещей. Используйте VS Code для развертывания решения на устройстве IoT Edge.

Сначала создайте и передайте ваше решение в реестр контейнеров. 

1. В обозревателе VS Code щелкните правой кнопкой мыши файл **deployment.template.json** и выберите **Build and Push IoT Edge solution** (Создать и отправить решение IoT Edge). Вы можете наблюдать за ходом этой операции в интегрированном терминале в VS Code. 
2. Обратите внимание, что в решение была добавлена новая папка **config**. Разверните эту папку и откройте файл развертывания **deployment.json**.
3. Просмотрите сведения в файле deployment.json. Файл deployment.json создается (или обновляется) автоматически на основе файла шаблона развертывания, который вы настроили, и информации из решения, включая файл .env и файл module.json. 

Затем выберите устройство и разверните решение.

1. В обозревателе VS Code разверните раздел **Azure IoT Hub Devices** (Устройства Центра Интернета вещей Azure). 
2. Щелкните правой кнопкой мыши имя устройства, которое необходимо сделать целевым объектом развертывания, и выберите **Create Deployment for IoT Edge** (Создание развертывания для устройств IoT Edge). 
3. В проводнике перейдите к папке **config** в решении и выберите **deployment.json**. Щелкните **Select Edge Deployment Manifest** (Выбрать манифест развертывания Edge). 

Если развертывание завершено успешно, сообщение о подтверждении будет выведено в выходных данных VS Code. В обозревателе VS Code разверните сведения об устройстве IoT Edge, которое использовалось для данного развертывания. Наведите курсор на заголовок **Устройства Центра Интернета вещей Azure**, чтобы активировать кнопку обновления, если модули не отображаются сразу. Для запуска модулей и обратного сообщения в Центр Интернета вещей может потребоваться несколько минут. 

Кроме того, можно проверить, все ли модули отображаются и запущены на своем устройстве. На своем устройстве IoT Edge выполните следующую команду, чтобы увидеть состояние модулей. Запуск модулей может занять несколько минут.

   ```bash
   iotedge list
   ```

## <a name="view-classification-results"></a>Просмотр результатов классификации

Существует два способа просмотра результатов модулей: на самом устройстве, когда сообщения создаются и отправляются, или из Visual Studio Code по мере поступления сообщений в Центр Интернета вещей. 

На устройстве просмотрите журналы модуля cameraCapture, чтобы просмотреть отправляемые сообщения и подтверждение того, что они были получены Центром Интернета вещей. 

   ```bash
   iotedge logs cameraCapture
   ```

В Visual Studio Code щелкните имя устройства IoT Edge правой кнопкой мыши и выберите **Start Monitoring Built-in Event Endpoint** (Начать мониторинг строенной конечной точки событий). 

Результаты из модуля Пользовательского визуального распознавания, которые отправляются в виде сообщений из модуля cameraCapture, содержат вероятность того, является ли дерево на изображении болиголовом или японской вишней. Так как на изображении находится болиголов, вы должны увидеть вероятность 1.0. 


## <a name="clean-up-resources"></a>Очистка ресурсов

Если вы планируете перейти к следующей рекомендуемой статье, можно сохранить созданные и повторно используемые ресурсы и конфигурации. Это же устройство IoT Edge также можно использовать в качестве тестового устройства. 

В противном случае вы можете удалить локальные конфигурации и ресурсы Azure, созданные в рамках этой статьи, чтобы избежать расходов. 

[!INCLUDE [iot-edge-clean-up-cloud-resources](../../includes/iot-edge-clean-up-cloud-resources.md)]


## <a name="next-steps"></a>Дополнительная информация

В этом руководстве вы обучили модель Пользовательского визуального распознавания и развернули ее как модуль на устройстве IoT Edge. Затем был создан второй модуль, который может запросить службу классификации изображений и сообщить о результатах обратно в Центр Интернета вещей. 

Если вы хотите попробовать более углубленную версию этого сценария с потоковыми данными камеры, см. [эту статью](https://github.com/Azure-Samples/Custom-vision-service-iot-edge-raspberry-pi). 

Перейдите к следующим руководствам, чтобы узнать о других способах, с помощью которых Azure IoT Edge может помочь вам превратить данные в бизнес-аналитику на пограничном устройстве.

> [!div class="nextstepaction"]
> [Руководство по хранению данных в пограничной системе с помощью баз данных SQL Server](tutorial-store-data-sql-server.md)
