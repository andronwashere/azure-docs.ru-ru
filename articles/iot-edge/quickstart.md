---
title: Краткое руководство. Создание устройства Azure IoT Edge в Windows | Документация Майкрософт
description: Из этого краткого руководства вы узнаете, как создать устройство IoT Edge и удаленно развернуть готовый код на портале Azure.
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 03/19/2019
ms.topic: quickstart
ms.service: iot-edge
services: iot-edge
ms.custom: mvc, seodec18
ms.openlocfilehash: ce6703c507e955ffe98e71f26feca08f9f37dfe5
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/23/2019
ms.locfileid: "66146771"
---
# <a name="quickstart-deploy-your-first-iot-edge-module-from-the-azure-portal-to-a-windows-device"></a>Краткое руководство. Развертывание модуля IoT Edge на устройстве с Windows на портале Azure

В этом кратком руководстве вы примените облачный интерфейс Azure IoT Edge для удаленного развертывания готового кода на устройстве IoT Edge. Чтобы выполнить эту задачу, сначала вы создадите и настроите виртуальную машину Windows для работы в качестве устройства IoT Edge, а затем развернете модуль на ней.

Из этого краткого руководства вы узнаете, как выполнять следующие задачи:

1. Создайте Центр Интернета вещей.
2. Регистрация устройства IoT Edge в Центре Интернета вещей.
3. Установка и запуск среды выполнения IoT Edge на устройстве.
4. Удаленное развертывание модуля на устройстве IoT Edge и отправка телеметрии в Центр Интернета вещей.

![Схема рассматриваемой в этом кратком руководстве архитектуры для устройства и облака](./media/quickstart/install-edge-full.png)

Модуль, который вы развернете в рамках этого краткого руководства, представляет собой имитированный датчик, генерирующий данные температуры, влажности и давления. В других руководствах по Azure IoT Edge используются наработки из этой статьи: развернутые модули, которые анализируют смоделированные данные для бизнес-аналитики.

Если у вас нет активной подписки Azure, перед началом работы создайте [бесплатную учетную запись](https://azure.microsoft.com/free).

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Для выполнения многих действий, описанных в этом кратком руководстве, используется Azure CLI, а также расширение Интернета вещей Azure для предоставления дополнительных функций.

Добавьте расширение Интернета вещей Azure в экземпляр Cloud Shell.

   ```azurecli-interactive
   az extension add --name azure-cli-iot-ext
   ```

## <a name="prerequisites"></a>Предварительные требования

Облачные ресурсы.

* Группа ресурсов для управления всеми ресурсами, которые вы используете в этом кратком руководстве.

   ```azurecli-interactive
   az group create --name IoTEdgeResources --location westus2
   ```

Устройства IoT Edge:

* Виртуальная машина Windows для работы в качестве устройства IoT Edge. Вы можете создать эту виртуальную машину с помощью следующей команды, заменив *{password}* надежным паролем:

  ```azurecli-interactive
  az vm create --resource-group IoTEdgeResources --name EdgeVM --image MicrosoftWindowsDesktop:Windows-10:rs5-pro:latest --admin-username azureuser --admin-password {password} --size Standard_DS1_v2
  ```

  Создание и запуск виртуальной машины может занять несколько минут. Затем вы можете загрузить RDP-файл, который будет использоваться при подключении к виртуальной машине:

  1. Перейдите к новой виртуальной машине Windows на портале Azure.
  1. Нажмите кнопку **Подключиться**.
  1. На вкладке **RDP** выберите **Скачать RDP-файл**.

  Откройте этот файл, используя подключение к удаленному рабочему столу, чтобы подключиться к виртуальной машине Windows, указав имя и пароль администратора, заданные в команде `az vm create`.


> [!NOTE]
> Для простоты в этом кратком руководстве используется виртуальная машина Windows для настольных систем. Сведения об общедоступных версиях ОС Windows для рабочих сценариев см. в статье [Azure IoT Edge supported systems](support.md) (Системы, поддерживаемые в Azure IoT Edge).

## <a name="create-an-iot-hub"></a>Создание Центра Интернета вещей

Начните с создания Центра Интернета вещей с помощью Azure CLI.

![Схема создания Центра Интернета вещей в облаке](./media/quickstart/create-iot-hub.png)

Для целей этого руководства можно использовать бесплатный уровень. Если вы уже использовали бесплатный Центр Интернета вещей и он у вас сохранился, можете использовать его. В подписке может быть только один бесплатный Центр Интернета вещей.

При помощи следующего кода создается бесплатный центр **F1** в группе ресурсов **IoTEdgeResources**. Замените *{hub_name}* уникальным именем для вашего Центра Интернета вещей.

   ```azurecli-interactive
   az iot hub create --resource-group IoTEdgeResources --name {hub_name} --sku F1
   ```

   Если отобразится сообщение об ошибке с уведомлением о том, что в вашей подписке уже имеется один бесплатный центр, измените номер SKU на **S1**. Если отобразится сообщение об ошибке с уведомлением о том, что имя недоступно, значит кто-то уже создал Центр Интернета вещей с таким именем. Выберите другое имя.

## <a name="register-an-iot-edge-device"></a>Регистрация устройства IoT Edge

Зарегистрируйте устройство IoT Edge в только что созданном Центре Интернета вещей.
![Схема регистрации устройства с помощью удостоверения Центра Интернета вещей](./media/quickstart/register-device.png)

Создайте удостоверение для своего имитированного устройства, чтобы оно могло обмениваться данными с Центром Интернета вещей. Удостоверение устройства находится в облаке. Чтобы связать физическое устройство с удостоверением, нужно использовать уникальную строку подключения к устройству.

Так как поведение и управление устройств IoT Edge и обычных устройств Интернета вещей отличаются, укажите в удостоверении, что это устройство IoT Edge, с помощью флага `--edge-enabled`.

1. Чтобы создать устройство с именем **myEdgeDevice** в Центре Интернета вещей, введите следующую команду в Azure Cloud Shell.

   ```azurecli-interactive
   az iot hub device-identity create --device-id myEdgeDevice --hub-name {hub_name} --edge-enabled
   ```

   Если отобразится сообщение об ошибке при использовании ключей политики iothubowner, убедитесь, что в Cloud Shell установлена последняя версия расширения azure-cli-iot-ext.

2. Получите строку подключения для устройства, которая связывает физическое устройство с его идентификатором в Центре Интернета вещей.

   ```azurecli-interactive
   az iot hub device-identity show-connection-string --device-id myEdgeDevice --hub-name {hub_name}
   ```

3. Скопируйте значение ключа `connectionString` из выходных данных JSON и сохраните его. Это значение — строка подключения устройства. Эта строка потребуется для настройки среды выполнения IoT Edge в следующем разделе.

   ![Получение строки подключения из выходных данных интерфейса командной строки](./media/quickstart/retrieve-connection-string.png)

## <a name="install-and-start-the-iot-edge-runtime"></a>Установка и запуск среды выполнения IoT Edge

Установите среду выполнения Azure IoT Edge на устройстве IoT Edge и настройте для нее строку подключения к устройству.
![Схема запуска среды выполнения на устройстве](./media/quickstart/start-runtime.png)

Среда выполнения IoT Edge развертывается на всех устройствах IoT Edge. Она состоит из трех компонентов. **Управляющая программа безопасности IoT Edge** запускается при каждой загрузке устройства Edge, перезагружая его путем запуска агента IoT Edge. **Агент IoT Edge** упрощает развертывание и мониторинг модулей на устройстве IoT Edge, включая центр IoT Edge. **Центр IoT Edge** управляет взаимодействием между модулями на устройстве IoT Edge, а также между устройством и Центром Интернета вещей.

Скрипт установки также включает в себя обработчик контейнеров (Moby), который управляет образами контейнеров на вашем устройстве IoT Edge.

Во время установки среды выполнения появится запрос на строку подключения к устройству. Используйте строку, полученную с помощью Azure CLI. Эта строка свяжет ваше физическое устройство с удостоверением устройства IoT Edge в Azure.

### <a name="connect-to-your-iot-edge-device"></a>Подключение к устройству IoT Edge

Действия, описанные в этом разделе, будут происходить на вашем устройстве IoT Edge, поэтому вам необходимо подключиться к этой виртуальной машине через удаленный рабочий стол.

### <a name="install-and-configure-the-iot-edge-service"></a>Установка и настройка службы IoT Edge

Загрузите и установите среду выполнения IoT Edge с помощью PowerShell. Настройте свое устройство, используя строку подключения к устройству, которую вы извлекли из Центра Интернета вещей.

1. Если вы этого еще не сделали, выполните действия, описанные в статье [Register a new Azure IoT Edge device from the Azure portal](how-to-register-device-portal.md) (Регистрация нового устройства Azure IoT Edge на портале Azure), чтобы зарегистрировать устройство и получить для него строку подключения. 

2. Откройте сеанс PowerShell от имени администратора.

   >[!NOTE]
   >Для установки IoT Edge используйте сеанс AMD64 PowerShell, а не PowerShell (x86). Чтобы узнать, какой тип сеанса используется, выполните следующую команду:
   >
   >```powershell
   >(Get-Process -Id $PID).StartInfo.EnvironmentVariables["PROCESSOR_ARCHITECTURE"]
   >```

3. Команда **Deploy-IoTEdge** позволяет проверить, установлена ли на компьютере Windows поддерживаемая версия, включает функцию контейнеров, скачивает среду выполнения Moby и затем скачивает среду выполнения IoT Edge.

   ```powershell
   . {Invoke-WebRequest -useb aka.ms/iotedge-win} | Invoke-Expression; `
   Deploy-IoTEdge -ContainerOs Windows
   ```

4. Компьютер может перезагрузиться автоматически. Если при выполнении команды Deploy-IoTEdge появится запрос на перезагрузку, сделайте это. 

5. Откройте сеанс PowerShell от имени администратора снова.

6. Команда **Initialize-IoTEdge** настраивает среду выполнения IoT Edge на вашем компьютере. По умолчанию при выполнении команды применяется подготовка вручную с помощью контейнеров Windows. 

   ```powershell
   . {Invoke-WebRequest -useb aka.ms/iotedge-win} | Invoke-Expression; `
   Initialize-IoTEdge -ContainerOs Windows
   ```

7. При появлении запроса на строку **DeviceConnectionString** укажите строку, скопированную в предыдущем разделе. Не заключайте строку подключения в кавычки.

### <a name="view-the-iot-edge-runtime-status"></a>Просмотр состояния среды выполнения IoT Edge

Убедитесь, что среда выполнения успешно установлена и настроена.

1. Проверьте состояние службы IoT Edge.

   ```powershell
   Get-Service iotedge
   ```

2. Если нужно устранить неполадки со службой, извлеките журналы службы.

   ```powershell
   . {Invoke-WebRequest -useb aka.ms/iotedge-win} | Invoke-Expression; Get-IoTEdgeLog
   ```

3. Просмотрите данные обо всех модулях, запущенных на устройстве IoT Edge. Так как служба запущена первый раз, отобразится только запущенный модуль **edgeAgent**. Модуль edgeAgent запускается по умолчанию и позволяет установить и запустить любые дополнительные модули, развертываемые на устройстве.

   ```powershell
   iotedge list
   ```

   ![Просмотр данных об одном модуле на устройстве](./media/quickstart/iotedge-list-1.png)

Для завершения установки и запуска модуля агента IoT Edge может потребоваться несколько минут.

Теперь устройство IoT Edge настроено. Оно готово для запуска модулей, развернутых в облаке.

## <a name="deploy-a-module"></a>Развертывание модуля

Управляя устройством Azure IoT Edge из облака, разверните модуль, который будет передавать данные телеметрии в Центр Интернета вещей.
![Схема развертывания модуля из облака на устройство](./media/quickstart/deploy-module.png)

[!INCLUDE [iot-edge-deploy-module](../../includes/iot-edge-deploy-module.md)]

## <a name="view-generated-data"></a>Просмотр сформированных данных

В этом руководстве описано, как создать устройство IoT Edge и установить на нем среду выполнения IoT Edge. Затем с помощью портала Azure мы развернули модуль IoT Edge на устройстве, обеспечив возможность запуска без необходимости менять настройки на устройстве.

В этом примере отправленный модуль создает пример данных, которые можно использовать для тестирования. Имитированный модуль датчика температуры создает данные среды, которые вы сможете позже использовать для тестирования. Имитируемый датчик выполняет мониторинг оборудования и окружающей среды. Например, этот датчик может быть в серверной комнате, производственном цехе или ветроэлектрической установке. В сообщении отображаются данные о температуре и влажности окружающей среды, температуре и давлении оборудования, а также метка времени. При работе с руководствами по IoT Edge используйте данные, созданные этим модулем, как тестовые данные для аналитики.

Убедитесь, что развернутый из облака модуль работает на вашем устройстве IoT Edge.

```powershell
iotedge list
```

   ![Просмотр трех модулей на устройстве](./media/quickstart/iotedge-list-2.png)

Убедитесь, что сообщения отправляются с модуля датчика температуры в облако.

```powershell
iotedge logs SimulatedTemperatureSensor -f
```

   >[!TIP]
   >В командах IoT Edge при использовании имен модулей учитывается регистр.

   ![Просмотр получаемых с модуля данных](./media/quickstart/iotedge-logs.png)

Вы также можете просматривать сообщения, которые поступают в Центр Интернета вещей, с помощью [расширения набора средств Центра Интернета вещей для Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-toolkit) (прежнее название — расширение набора средств Azure IoT).

## <a name="clean-up-resources"></a>Очистка ресурсов

Если вы хотите продолжить ознакомление с руководствами по IoT Edge, используйте устройство, зарегистрированное и настроенное в рамках этого краткого руководства. Если нет, вы можете удалить созданные ресурсы Azure, чтобы избежать расходов.

Если вы создали виртуальную машину и Центр Интернета вещей в новой группе ресурсов, можно удалить эту группу и все связанные с ней ресурсы. Внимательно проверьте содержимое группы ресурсов. В ней не должно быть важных ресурсов. Если вы не хотите удалять всю группу, можно удалить отдельные ресурсы.

Удалите группу **IoTEdgeResources**.

```azurecli-interactive
az group delete --name IoTEdgeResources
```

## <a name="next-steps"></a>Дополнительная информация

При работе с этим кратким руководством вы создали устройство IoT Edge и с помощью облачного интерфейса Azure IoT Edge развернули код на устройстве. В итоге вы получили устройство для тестирования, генерирующее необработанные данные о своей среде.

Далее вы можете настроить локальную среду разработки, чтобы приступить к созданию модулей IoT Edge, которые выполняют бизнес-логику. 

> [!div class="nextstepaction"]
> [Tutorial: Develop IoT Edge modules for Windows devices](tutorial-develop-for-windows.md) (Руководство: разработка модулей IoT Edge для устройств с Windows)
