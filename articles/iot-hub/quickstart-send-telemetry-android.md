---
title: Краткое руководство по отправке данных телеметрии в Центр Интернета вещей Azure (Android) | Документация Майкрософт
description: В рамках этого краткого руководства вы запустите пример приложения Android, которое отправляет имитированные данные телеметрии в центр Интернета вещей и считывает данные телеметрии из этого центра для обработки в облаке.
author: wesmc7777
manager: philmea
ms.service: iot-hub
services: iot-hub
ms.devlang: java
ms.topic: quickstart
ms.custom: mvc
ms.date: 03/15/2019
ms.author: wesmc
ms.openlocfilehash: 6e7d0ff396a4d264ee1f724d192c6c36abb400b1
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "67051563"
---
# <a name="quickstart-send-iot-telemetry-from-an-android-device"></a>Краткое руководство. Отправка данных телеметрии в центр Интернета вещей с устройства Android

[!INCLUDE [iot-hub-quickstarts-1-selector](../../includes/iot-hub-quickstarts-1-selector.md)]

Центр Интернета вещей — это служба Azure, которая позволяет получать большие объемы телеметрии с ваших устройств Центра Интернета вещей в облаке на хранение или обработку. В рамках этого краткого руководства вы отправите данные телеметрии в Центр Интернета вещей из приложения Android, запущенного на физическом или имитированном устройстве.

В рамках этого руководства для отправки данных телеметрии используется заранее разработанное приложение Android. Для считывания данных телеметрии из Центра Интернета вещей используется Azure Cloud Shell. Прежде чем запускать приложение, создайте центр Интернета вещей и зарегистрируйте устройство в центре.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Если у вас еще нет подписки Azure, [создайте бесплатную учетную запись Azure](https://azure.microsoft.com/free/?WT.mc_id=A261C142F), прежде чем начинать работу.

## <a name="prerequisites"></a>Предварительные требования

* Скачайте Android Studio по ссылке https://developer.android.com/studio/. Дополнительные сведения об установке Android Studio см. на [этой странице](https://developer.android.com/studio/install).

* Пакет SDK для Android 27, используемый в примере, который описан в этой статье.

* Выполните следующую команду, чтобы добавить расширение Интернета вещей Microsoft Azure для Azure CLI в экземпляр Cloud Shell. Расширение Интернета вещей добавляет в Azure CLI специальные команды Центра Интернета вещей, IoT Edge и службы подготовки устройств Интернета вещей (DPS).

   ```azurecli-interactive
   az extension add --name azure-cli-iot-ext
   ```

* [Пример приложения Android](https://github.com/Azure-Samples/azure-iot-samples-java/tree/master/iot-hub/Samples/device/AndroidSample), которое запускается при работе с этим кратким руководством, содержится в репозитории azure-iot-samples-java на сайте GitHub. Скачайте или клонируйте репозиторий [azure-iot-samples-java](https://github.com/Azure-Samples/azure-iot-samples-java).

## <a name="create-an-iot-hub"></a>Создание Центра Интернета вещей

[!INCLUDE [iot-hub-include-create-hub](../../includes/iot-hub-include-create-hub.md)]

## <a name="register-a-device"></a>Регистрация устройства

Устройство должно быть зарегистрировано в Центре Интернета вещей, прежде чем оно сможет подключиться. В этом кратком руководстве для регистрации имитируемого устройства используется Azure Cloud Shell.

1. Выполните приведенные ниже команды в Azure Cloud Shell, чтобы создать удостоверение устройства.

   **YourIoTHubName**. Замените этот заполнитель именем вашего центра Интернета вещей.

   **MyAndroidDevice**. Это имя, присвоенное зарегистрированному устройству. Используйте имя MyAndroidDevice, как показано ниже. Если вы выбрали другое имя для устройства, используйте его при работе с этим руководством и обновите имя устройства в примерах приложений перед их запуском.

    ```azurecli-interactive
    az iot hub device-identity create --hub-name YourIoTHubName --device-id MyAndroidDevice
    ```

2. Выполните следующую команду в Azure Cloud Shell, чтобы получить _строку подключения_ зарегистрированного устройства:

    **YourIoTHubName**. Замените этот заполнитель именем вашего центра Интернета вещей.

    ```azurecli-interactive
    az iot hub device-identity show-connection-string --hub-name YourIoTHubName --device-id MyAndroidDevice --output table
    ```

    Запишите строку подключения устройства, которая выглядит так:

   `HostName={YourIoTHubName}.azure-devices.net;DeviceId=MyAndroidDevice;SharedAccessKey={YourSharedAccessKey}`

    Это значение потребуется вам в дальнейшем для отправки данных телеметрии в рамках этого руководства.

## <a name="send-telemetry"></a>Отправка данных телеметрии

1. В Android Studio откройте пример проекта Android, полученный из GitHub. Проект находится в указанном ниже каталоге копии клонированного или скачанного репозитория [azure-iot-sample-java](https://github.com/Azure-Samples/azure-iot-samples-java).

        \azure-iot-samples-java\iot-hub\Samples\device\AndroidSample

2. В Android Studio откройте *gradle.properties* для примера проекта и замените заполнитель **Device_Connection_String** строкой подключения к устройству, которую вы записали ранее.

    ```
    DeviceConnectionString=HostName={YourIoTHubName}.azure-devices.net;DeviceId=MyNodeDevice;SharedAccessKey={YourSharedAccessKey}
    ```

3. В Android Studio последовательно выберите **File (Файл)**  > **Sync Project with Gradle Files (Синхронизировать проект с файлами Gradle)** . Проверьте, завершена ли сборка.

   > [!NOTE]
   > Возможные причины сбоя синхронизации проекта:
   >
   > * Версии плагина Android Gradle и Gradle, указанные в проекте, устарели для вашей версии Android Studio. Выполните [эти инструкции](https://developer.android.com/studio/releases/gradle-plugin), чтобы установить надлежащие версии плагина и Gradle, а также добавить на них ссылки.
   > * Лицензионное соглашение для пакета SDK для Android не подписано. Следуйте инструкциям в выходных данных сборки, чтобы подписать лицензионное соглашение и скачать пакет SDK.

4. По завершении сборки щелкните **Run (Запуск)**  > **Run "app" (Запустить "app")** . Настройте приложение для запуска на физическом устройстве Android или в эмуляторе Android. Дополнительные сведения о запуске приложения Android на физическом устройстве или в эмуляторе см. в [этой статье](https://developer.android.com/training/basics/firstapp/running-app).

5. После загрузки приложения нажмите кнопку **Start** (Начать), чтобы начать отправку данных телеметрии в Центр Интернета вещей:

    ![Приложение](media/quickstart-send-telemetry-android/sample-screenshot.png)


## <a name="read-the-telemetry-from-your-hub"></a>Чтение данных телеметрии из концентратора

В этом разделе объясняется, как использовать Azure Cloud Shell с [расширением Интернета вещей](https://docs.microsoft.com/cli/azure/ext/azure-cli-iot-ext/iot?view=azure-cli-latest) для мониторинга сообщений, отправляемых устройством Android.

1. С помощью Azure Cloud Shell выполните следующую команду для установки подключения к центру Интернета вещей и чтения поступающих из него сообщений:

   **YourIoTHubName**. Замените этот заполнитель именем вашего Центра Интернета вещей.

    ```azurecli-interactive
    az iot hub monitor-events --hub-name YourIoTHubName --output table
    ```

    На следующем снимке экрана показан пример выходных данных, когда центр Интернета вещей получает данные телеметрии, отправленные устройством Android:

      ![Чтение сообщений устройства с помощью Azure CLI](media/quickstart-send-telemetry-android/read-data.png)
## <a name="clean-up-resources"></a>Очистка ресурсов

[!INCLUDE [iot-hub-quickstarts-clean-up-resources](../../includes/iot-hub-quickstarts-clean-up-resources.md)]

## <a name="next-steps"></a>Дополнительная информация

При работе с этим кратким руководством вы настроили Центр Интернета вещей, зарегистрировали устройство, отправили имитированные данные телеметрии в центр с помощью приложения Android, а также считали данные телеметрии из центра, используя Azure Cloud Shell.

Чтобы узнать, как управлять имитированным устройством из внутреннего приложения, перейдите к следующему краткому руководству.

> [!div class="nextstepaction"]
> [Краткое руководство. Управление подключенным к Центру Интернета вещей устройством](quickstart-control-device-android.md)