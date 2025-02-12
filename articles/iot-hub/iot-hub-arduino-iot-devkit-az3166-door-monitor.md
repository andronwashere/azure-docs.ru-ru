---
title: Получение сообщения электронной почты в случае открытия двери с помощью службы SendGrid и решения "Функции Azure" | Документация Майкрософт
description: Наблюдайте за магнитным датчиком, чтобы обнаружить, когда дверь открыта, и используйте решение "Функции Azure" для отправки уведомления по электронной почте.
author: liydu
manager: jeffya
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.tgt_pltfrm: arduino
ms.date: 03/19/2018
ms.author: liydu
ms.openlocfilehash: a620b592a33f9de11de53d623d257f203da2157b
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "61370362"
---
# <a name="door-monitor"></a>Мониторинг двери          

Плата MXChip IoT DevKit содержит встроенный магнитный датчик. В этом проекте необходимо обнаружить присутствие или отсутствие поблизости мощного магнитного поля, которое в этом случае исходит от маленького постоянного магнита.

## <a name="what-you-learn"></a>Что вы узнаете

В этом проекте вы узнаете:
- как использовать магнитный датчик MXChip IoT DevKit для обнаружения движения расположенного рядом магнита;
- как использовать службу SendGrid для отправки уведомления на адрес электронной почты.

> [!NOTE]
> Для практического использования этого проекта выполните следующие задачи:
> - установите магнит на краю двери;
> - установите DevKit на дверной косяк рядом с магнитом. Открытие и закрытие двери активирует датчик, что приведет к получению уведомления о событии на электронную почту.

## <a name="what-you-need"></a>Необходимые элементы

Выполните указания в [руководстве по началу работы](iot-hub-arduino-iot-devkit-az3166-get-started.md), чтобы перейти к таким этапам:

* Подключение DevKit к сети Wi-Fi.
* Подготовка среды разработки

Активная подписка Azure. Если у вас еще нет ее, вы можете зарегистрироваться одним из этих способов:

* Вы можете активировать [бесплатную 30-дневную пробную учетную запись Microsoft Azure](https://azure.microsoft.com/free/).
* Запросите [деньги на счете в Azure](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/), если у вас есть подписка MSDN или Visual Studio.

## <a name="deploy-the-sendgrid-service-in-azure"></a>Развертывание службы SendGrid в Azure

[SendGrid](https://sendgrid.com/) — это облачная платформа отправки сообщений электронной почты. Эта служба будет использоваться для отправки уведомлений по электронной почте.

> [!NOTE]
> Если служба SendGrid уже развернута, можно перейти напрямую к разделу [Развертывание Центра Интернета вещей в Azure](#deploy-iot-hub-in-azure).

### <a name="sendgrid-deployment"></a>Развертывание SendGrid

Чтобы подготовить службы Azure, нажмите кнопку **Развертывание в Azure**. С помощью этой кнопки можно быстро и легко развернуть проекты с открытым исходным кодом в Microsoft Azure.

Нажмите кнопку **Deploy to Azure** (Развернуть в Azure) ниже. 

[![Развертывание в Azure](https://azuredeploy.net/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FVSChina%2Fdevkit-door-monitor%2Fmaster%2FSendGridDeploy%2Fazuredeploy.json)

Если вы еще не вошли в учетную запись Azure, войдите сейчас. 

Теперь отображается форма регистрации SendGrid.

![Развертывание SendGrid](media/iot-hub-arduino-iot-devkit-az3166-door-monitor/sendgrid-deploy.png)

Заполните форму регистрации:

   * **Группа ресурсов.** Создайте группу ресурсов для размещения службы SendGrid или используйте имеющуюся. Дополнительные сведения см. в статье [Управление ресурсами Azure через портал](../azure-resource-manager/manage-resource-groups-portal.md).

   * **Имя.** Имя службы SendGrid. Выберите уникальное имя, отличающееся от других служб, которые могут быть у вас.

   * **Пароль**. Службе необходим пароль, не используемый нигде в этом проекте.

   * **Электронная почта:** Служба SendGrid отправит запрос на проверку на этот адрес электронной почты.

Установите флажок **Закрепить на панели мониторинга**, чтобы упростить поиск этого приложения в будущем. Затем щелкните **Покупка** для отправки формы регистрации.
 
### <a name="sendgrid-api-key-creation"></a>Создание ключа API SendGrid

После завершения развертывания щелкните его и нажмите кнопку **Управление**. Появится страница вашей учетной записи SendGrid, на которой нужно подтвердить адрес электронной почты.

![Управление SendGrid](media/iot-hub-arduino-iot-devkit-az3166-door-monitor/sendgrid-manage.png)

На странице SendGrid щелкните **Параметры** > **Ключи API** > **Создание ключа API**.

![Первый снимок экрана создания API SendGrid](media/iot-hub-arduino-iot-devkit-az3166-door-monitor/sendgrid-create-api-first.png)

На странице **Создание ключа API** введите **имя ключа API** и щелкните **Create & View** (Создать и просмотреть).

![Второй снимок экрана создания API SendGrid](media/iot-hub-arduino-iot-devkit-az3166-door-monitor/sendgrid-create-api-second.png)

Ключ API отображается только один раз. Не забудьте скопировать и сохранить его в надежном месте, так как он используется в следующем шаге.

## <a name="deploy-iot-hub-in-azure"></a>Развертывание Центра Интернета вещей в Azure

Ниже показано, как подготовить другие связанные с Центром Интернета вещей службы и развернуть решение "Функции Azure" для этого проекта.

Нажмите кнопку **Deploy to Azure** (Развернуть в Azure) ниже. 

[![Развертывание в Azure](https://azuredeploy.net/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FVSChina%2Fdevkit-door-monitor%2Fmaster%2Fazuredeploy.json)

Появится форма регистрации.

![Развертывание Центра Интернета вещей](media/iot-hub-arduino-iot-devkit-az3166-door-monitor/iot-hub-deploy.png)

Заполните поля в форме регистрации.

   * **Группа ресурсов.** Создайте группу ресурсов для размещения службы SendGrid или используйте имеющуюся. Дополнительные сведения см. в статье [Управление ресурсами Azure через портал](../azure-resource-manager/manage-resource-groups-portal.md).

   * **Имя Центра Интернета вещей**. Имя центра Интернета вещей. Выберите уникальное имя, отличающееся от других служб, которые могут быть у вас.

   * **Iot Hub Sku** (SKU Центра Интернета вещей). F1 (ограничено, по одному на подписку) является бесплатным. Дополнительные сведения о ценах можно просмотреть на [странице с ценами](https://azure.microsoft.com/pricing/details/iot-hub/).

   * **Электронная почта отправителя**. Это поле должно иметь тот же адрес электронной почты, который использовался при настройке службы SendGrid.

Установите флажок **Закрепить на панели мониторинга**, чтобы упростить поиск этого приложения в будущем. Затем щелкните **Покупка**, если вы готовы перейти к следующему шагу.
 
## <a name="build-and-upload-the-code"></a>Создание и передача кода

Теперь загрузите пример кода в VS Code и подготовьте к работе необходимые службы Azure.

### <a name="start-vs-code"></a>Запуск VS Code

- Убедитесь, что плата DevKit **не** подключена к компьютеру.
- Запустите VSCode.
- Подключите DevKit на компьютере.

> [!NOTE]
> При запуске VS Code может появиться сообщение об ошибке о том, что не обнаружена среда IDE Arduino или связанный пакет платы. Если возникла эта ошибка, закройте VS Code, запустите интегрированную среду разработки Arduino, и VS Code правильно определит путь к ней.

### <a name="open-arduino-examples-folder"></a>Открытие папки с примерами Arduino

Разверните раздел **ARDUINO EXAMPLES** (ПРИМЕРЫ ARDUINO) слева, перейдите в папку **Examples for MXCHIP AZ3166 (Примеры для MXCHIP AZ3166) > AzureIoT** и щелкните **DoorMonitor**. Откроется новое окно VSCode с папкой проекта.

![Минирешение: примеры](media/iot-hub-arduino-iot-devkit-az3166-door-monitor/vscode-examples.png)

Пример приложения также можно открыть из палитры команд. Нажмите `Ctrl+Shift+P` (macOS: `Cmd+Shift+P`) для вызова палитры команд. Введите **Arduino**, а затем найдите и выберите **Arduino: примеры**.

### <a name="provision-azure-services"></a>Подготовка служб Azure

В окне решения выполните задачу подготовки в облаке:
- нажмите клавиши `Ctrl+P` (macOS: `Cmd+P`);
- введите `task cloud-provision` в соответствующем тестовом поле.

В терминале VS Code с помощью указаний в интерактивной командной строке подготовьте необходимые службы Azure. Выберите те же элементы из показанного списка, которые вы предоставили ранее в разделе [Развертывание Центра Интернета вещей в Azure](#deploy-iot-hub-in-azure).

![Подготовка в облаке](media/iot-hub-arduino-iot-devkit-az3166-door-monitor/cloud-provision.png)

> [!NOTE]
> Если страница зависает в состоянии загрузки на этапе входа в Azure, ознакомьтесь с [разделом в часто задаваемых вопросах об IoT DevKit "Page hangs when log in Azure"](https://microsoft.github.io/azure-iot-developer-kit/docs/faq/#page-hangs-when-log-in-azure) (Страница зависает при входе в Azure). 

### <a name="build-and-upload-the-device-code"></a>Создание и передача кода устройства

Затем загрузите код для устройства.

#### <a name="windows"></a>Windows

1. Нажмите `Ctrl+P`, чтобы запустить задачу `task device-upload`.

2. Терминал предложит перейти в режим настройки. Чтобы сделать это, удерживая нажатой кнопку "A", нажмите и отпустите кнопку "Reset" (Сброс). На экране отображается идентификационный номер DevKit и слово *Configuration*.

#### <a name="macos"></a>macOS

1. Переведите DevKit в режим настройки: Удерживая нажатой кнопку "A", нажмите и отпустите кнопку "Reset" (Сброс). На экране отобразится надпись "Configuration" (Настройка).

2. Щелкните `Cmd+P` для запуска `task device-upload`.

#### <a name="verify-upload-and-run-the-sample-app"></a>Проверка, отправка и запуск примера приложения

Строка подключения, полученная на шаге [подготовки служб Azure](#provision-azure-services), установлена. 

Теперь VS Code начинает проверку и отправку эскиза Arduino на DevKit.

![device-upload](media/iot-hub-arduino-iot-devkit-az3166-door-monitor/device-upload.png)

Плата DevKit перезагрузится и начнет выполнение кода.

> [!NOTE]
> В некоторых случаях появляется сообщение об ошибке "Error: AZ3166: Unknown package" (Ошибка AZ3166: неизвестный пакет). Такая ошибка возникает, если индекс пакета платы неправильно обновлен. Чтобы устранить эту ошибку, ознакомьтесь с [разделом по разработке](https://microsoft.github.io/azure-iot-developer-kit/docs/faq/#development) в статье с часто задаваемыми вопросами об IoT DevKit.

## <a name="test-the-project"></a>Тестирование проекта

Изначально программа инициализируется, когда DevKit находится в присутствии стабильного магнитного поля.

После инициализации на экране отображается `Door closed`. При изменении в магнитном поле состояние изменяется на `Door opened`. При каждом изменении состояния двери вы получаете уведомление по электронной почте. (Получение этих сообщений электронной почты может занять до пяти минут.)

![Магниты близко к датчику: дверь закрыта Door Closed](media/iot-hub-arduino-iot-devkit-az3166-door-monitor/test-door-closed.jpg "Магниты близко к датчику: дверь закрыта")

![Magnet moved away from the sensor: Door Opened](media/iot-hub-arduino-iot-devkit-az3166-door-monitor/test-door-opened.jpg "Магниты отдалились от датчика: дверь открыта")

## <a name="problems-and-feedback"></a>Проблемы и обратная связь

Если вы столкнулись с проблемами, ознакомьтесь с [вопросами и ответами](https://microsoft.github.io/azure-iot-developer-kit/docs/faq/) об IoT DevKit или используйте один из следующих каналов связи:

* [Gitter.im](https://gitter.im/Microsoft/azure-iot-developer-kit)
* [Stack Overflow](https://stackoverflow.com/questions/tagged/iot-devkit)

## <a name="next-steps"></a>Дальнейшие действия

Вы узнали, как подключить устройство DevKit к акселератору решения Azure IoT для удаленного мониторинга и как использовать службу SendGrid для отправки сообщений электронной почты. Ниже приведены предлагаемые дальнейшие действия:

* [Общие сведения об акселераторе решений для удаленного мониторинга Интернета вещей Azure](https://docs.microsoft.com/azure/iot-suite/)
* [Connect an MXChip IoT DevKit device to your Azure IoT Central application](https://docs.microsoft.com/microsoft-iot-central/howto-connect-devkit) (Подключение устройства MXChip IoT DevKit к приложению Azure IoT Central)
