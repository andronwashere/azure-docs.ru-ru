---
title: Краткое руководство по управлению устройством из Центра Интернета вещей (.NET) | Документация Майкрософт
description: 'В этом кратком руководстве описано, как запустить два примера приложений C#: внутреннее приложение, которое может удаленно управлять подключенными к центру устройствами, и приложение, которое имитирует подключенное к центру устройство, которым можно управлять удаленно.'
author: robinsh
manager: philmea
ms.author: robinsh
ms.service: iot-hub
services: iot-hub
ms.devlang: csharp
ms.topic: quickstart
ms.custom: mvc
ms.date: 06/21/2019
ms.openlocfilehash: 751db0effb57f19db47be1eed166d7053d617e3d
ms.sourcegitcommit: ac1cfe497341429cf62eb934e87f3b5f3c79948e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/01/2019
ms.locfileid: "67491963"
---
# <a name="quickstart-control-a-device-connected-to-an-iot-hub-net"></a>Краткое руководство. Управление подключенным к центру Интернета вещей устройством (.NET)

[!INCLUDE [iot-hub-quickstarts-2-selector](../../includes/iot-hub-quickstarts-2-selector.md)]

Центр Интернета вещей — это служба Azure, которая позволяет получать большие объемы данных телеметрии с устройств Центра Интернета вещей в облаке и управлять устройствами из облака. В этом кратком руководстве управление имитированным устройством, подключенным к Центру Интернета вещей, осуществляется с помощью *прямого метода*. Этот метод позволяет удаленно изменить поведение подключенного к Центру Интернета вещей устройства.

В этом кратком руководстве используются два предварительно созданных приложения .NET:

* Приложение имитированного устройства, реагирующее на прямые методы, вызванные из внутреннего приложения. Чтобы получать вызовы прямого метода, это приложение подключается к конечной точке конкретного устройства в Центре Интернета вещей.

* Внутреннее приложение, вызывающее прямые методы в имитированном устройстве. Чтобы вызвать прямой метод в устройстве, это приложение подключается к конечной точке на стороне службы в Центре Интернета вещей.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Если у вас еще нет подписки Azure, [создайте бесплатную учетную запись Azure](https://azure.microsoft.com/free/?WT.mc_id=A261C142F), прежде чем начинать работу.

## <a name="prerequisites"></a>Предварительные требования

Примеры приложений, запускаемые в рамках этого краткого руководства, написаны на языке C#. На компьютере, на котором ведется разработка, необходимо установить пакет SDK для .NET Core версии 2.1.0 или более поздней.

Пакет SDK для .NET Core, предназначенный для нескольких платформ, можно загрузить из [.NET](https://www.microsoft.com/net/download/all).

Текущую версию C# на компьютере, на котором ведется разработка, можно проверить, используя следующую команду:

```cmd/sh
dotnet --version
```

Выполните следующую команду, чтобы добавить расширение Интернета вещей Microsoft Azure для Azure CLI в экземпляр Cloud Shell. Расширение Интернета вещей добавляет в Azure CLI специальные команды Центра Интернета вещей, IoT Edge и службы подготовки устройств Интернета вещей (DPS).

```azurecli-interactive
az extension add --name azure-cli-iot-ext
```

Если вы это еще не сделали, загрузите пример проекта C# по адресу https://github.com/Azure-Samples/azure-iot-samples-csharp/archive/master.zip и извлеките ZIP-архив.

## <a name="create-an-iot-hub"></a>Создание Центра Интернета вещей

Если вы закончили работу с предыдущим руководством по [ отправке данных телеметрии с устройства в Центр Интернета вещей](quickstart-send-telemetry-dotnet.md), можете пропустить этот шаг.

[!INCLUDE [iot-hub-include-create-hub](../../includes/iot-hub-include-create-hub.md)]

## <a name="register-a-device"></a>Регистрация устройства

Если вы закончили работу с предыдущим руководством по [ отправке данных телеметрии с устройства в Центр Интернета вещей](quickstart-send-telemetry-dotnet.md), можете пропустить этот шаг.

Устройство должно быть зарегистрировано в Центре Интернета вещей, прежде чем оно сможет подключиться. В этом кратком руководстве для регистрации имитируемого устройства используется Azure Cloud Shell.

1. Выполните приведенные ниже команды в Azure Cloud Shell, чтобы создать удостоверение устройства.

   **YourIoTHubName**. Замените этот заполнитель именем вашего центра Интернета вещей.

   **MyDotnetDevice**. Имя регистрируемого устройства. Используйте **MyDotnetDevice**, как показано ниже. Если вы выбрали другое имя для устройства, используйте его при работе с этим руководством и обновите имя устройства в примерах приложений перед их запуском.

    ```azurecli-interactive
    az iot hub device-identity create \
      --hub-name YourIoTHubName --device-id MyDotnetDevice
    ```

2. Выполните следующую команду в Azure Cloud Shell, чтобы получить _строку подключения_ зарегистрированного устройства:

   **YourIoTHubName**. Замените этот заполнитель именем вашего центра Интернета вещей.

    ```azurecli-interactive
    az iot hub device-identity show-connection-string \
      --hub-name YourIoTHubName \
      --device-id MyDotnetDevice \
      --output table
    ```

    Запишите строку подключения устройства, которая выглядит так:

   `HostName={YourIoTHubName}.azure-devices.net;DeviceId=MyNodeDevice;SharedAccessKey={YourSharedAccessKey}`

    Это значение понадобится позже в рамках этого краткого руководства.

## <a name="retrieve-the-service-connection-string"></a>Получение строки подключения к службе

Чтобы разрешить внутренним приложениям подключаться к Центру Интернета вещей и получать сообщения, необходима _строка подключения к службе_. Следующая команда извлекает строку подключения службы для Центра Интернета вещей:

```azurecli-interactive
az iot hub show-connection-string --name YourIoTHubName --policy-name service --output table
```

Запишите строку подключения к службе, которая выглядит так:

   `HostName={YourIoTHubName}.azure-devices.net;SharedAccessKeyName=service;SharedAccessKey={YourSharedAccessKey}`

Это значение понадобится позже в рамках этого краткого руководства. Строка подключения к службе отличается от строки подключения к устройству.  

## <a name="listen-for-direct-method-calls"></a>Ожидание передачи данных при вызове прямого метода

Приложение имитированного устройства подключается к конечной точке конкретного устройства в Центре Интернета вещей, отправляет имитированные данные телеметрии и ожидает передачи данных при вызове прямого метода из центра. В рамках этого краткого руководства вызов прямого метода из центра инициирует изменение интервала, с которым устройство отправляет данные телеметрии. После выполнения прямого метода имитированное устройство отправляет подтверждение в центр.

1. В окне терминала на локальном компьютере перейдите в корневую папку примера проекта C#. Затем перейдите в папку **iot-hub\Quickstarts\simulated-device-2**.

2. Откройте файл **SimulatedDevice.cs** в любом текстовом редакторе.

    Замените значение переменной `s_connectionString` записанной ранее строкой подключения к устройству. Сохраните изменения в файле **SimulatedDevice.cs**.

3. Установите необходимые пакеты приложения имитированного устройства, выполнив в окне терминала на локальном компьютере следующие команды:

    ```cmd/sh
    dotnet restore
    ```

4. Создайте и запустите приложение имитированного устройства, выполнив в окне терминала на локальном компьютере следующие команды:

    ```cmd/sh
    dotnet run
    ```

    На следующем снимке экрана показан пример выходных данных, когда приложение имитированного устройства отправляет данные телеметрии в Центр Интернета вещей:

    ![Запуск виртуального устройства](./media/quickstart-control-device-dotnet/SimulatedDevice-1.png)

## <a name="call-the-direct-method"></a>Вызов прямого метода

Внутреннее приложение подключается к конечной точке на стороне службы в Центре Интернета вещей. Приложение выполняет вызовы прямых методов к устройству через Центр Интернета вещей и ожидает подтверждений. Внутреннее приложение Центра Интернета вещей обычно работает в облаке.

1. В другом окне терминала на локальном компьютере перейдите в корневую папку примера проекта C#. Затем перейдите в папку **iot-hub\Quickstarts\back-end-application**.

2. Откройте файл **BackEndApplication.cs** в любом текстовом редакторе.

    Замените значение переменной `s_connectionString` записанной ранее строкой подключения к службе. Сохраните изменения в файле **BackEndApplication.cs**.

3. Установите необходимые библиотеки внутреннего приложения, выполнив в окне терминала на локальном компьютере следующие команды:

    ```cmd/sh
    dotnet restore
    ```

4. Создайте и запустите внутреннее приложение, выполнив в окне терминала на локальном компьютере следующие команды:

    ```cmd/sh
    dotnet run
    ```

    На следующем снимке экрана показан пример выходных данных, когда приложение выполняет вызов прямого метода к устройству и получает подтверждение:

    ![Запуск внутреннего приложения](./media/quickstart-control-device-dotnet/BackEndApplication.png)

    После запуска внутреннего приложения в окне консоли, в котором выполняется имитированное устройство, отобразится сообщение и изменится интервал, с которым это приложение отправляет сообщения:

    ![Изменения в имитированном клиенте](./media/quickstart-control-device-dotnet/SimulatedDevice-2.png)

## <a name="clean-up-resources"></a>Очистка ресурсов

[!INCLUDE [iot-hub-quickstarts-clean-up-resources](../../includes/iot-hub-quickstarts-clean-up-resources.md)]

## <a name="next-steps"></a>Дополнительная информация

В рамках этого краткого руководства вы вызвали прямой метод на устройстве из внутреннего приложения, а также ответили на этот вызов в приложении имитированного устройства.

Чтобы узнать, как маршрутизировать сообщения с устройства в облако в разные расположения в облаке, перейдите к следующему руководству.

> [!div class="nextstepaction"]
> [Руководство. Маршрутизация телеметрии в разные конечные точки для обработки](tutorial-routing.md)
