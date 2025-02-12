---
title: Развертывание модуля двойника OPC в существующем проекте Azure | Документация Майкрософт
description: Развертывание OPC двойника в существующем проекте.
author: dominicbetts
ms.author: dobett
ms.date: 11/26/2018
ms.topic: conceptual
ms.service: industrial-iot
services: iot-industrialiot
manager: philmea
ms.openlocfilehash: fc70d140479be100e6aa52cf8105d3e466342cd7
ms.sourcegitcommit: af58483a9c574a10edc546f2737939a93af87b73
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/17/2019
ms.locfileid: "68302663"
---
# <a name="deploy-opc-twin-to-an-existing-project"></a>Развертывание OPC двойника в существующем проекте

Модуль OPC двойника выполняется на IoT Edge и предоставляет несколько служб пограничных услуг для двойника OPC и служб реестра.

Микрослужба OPC двойника упрощает обмен данными между операторами фабрики и серверными устройствами OPC UA в фабричном этаже через модуль IoT Edge OPC двойника. Микрослужба предоставляет службы OPC UA (Просмотр, чтение, запись и выполнение) с помощью REST API. 

Микрослужба реестра устройств OPC UA предоставляет доступ к зарегистрированным приложениям OPC UA и их конечным точкам. Операторы и администраторы могут регистрировать и отменять регистрацию новых приложений OPC UA и просматривать существующие, включая конечные точки. В дополнение к управлению приложениями и конечными точками служба реестра также хранит каталоги зарегистрированных модулей IoT Edge OPC двойника. API службы обеспечивает управление функциями пограничных модулей, например запуск или остановку обнаружения сервера (сканирование служб), или активацию новой конечной точки двойников, доступ к которой можно получить с помощью микрослужбы OPC двойника.

Ядром модуля является удостоверение супервизора. Супервизор управляет конечной точкой двойника, которая соответствует конечным точкам сервера OPC UA, которые активируются с помощью соответствующего API реестра OPC UA. Эта конечная точка двойников перевод JSON UA, полученного от микрослужбы OPC двойника, работающей в облаке, в двоичные сообщения OPC UA, которые отправляются по защищенному каналу с отслеживанием состояния в управляемую конечную точку. Супервизор также предоставляет службы обнаружения, которые отправляют события обнаружения устройств в службу адаптации устройств OPC UA для обработки, где эти события приводят к обновлению реестра OPC UA.  В этой статье показано, как развернуть модуль OPC двойника в существующем проекте.

> [!NOTE]
> Дополнительные сведения о развертывании и инструкциях см. в [репозитории](https://github.com/Azure/azure-iiot-opc-twin-module)GitHub.

## <a name="prerequisites"></a>Предварительные требования

Убедитесь, что установлены расширения PowerShell PowerShell и [AzureRM](https://docs.microsoft.com/powershell/azure/azurerm/install-azurerm-ps) . Клонировать этот репозиторий GitHub, если вы еще этого не сделали. В PowerShell выполните следующие команды:

```powershell
git clone --recursive https://github.com/Azure/azure-iiot-components.git
cd azure-iiot-components
```

## <a name="deploy-industrial-iot-services-to-azure"></a>Развертывание промышленных служб IoT в Azure

1. В сеансе PowerShell выполните:

    ```powershell
    set-executionpolicy -ExecutionPolicy Unrestricted -Scope Process
    .\deploy.cmd
    ```

2. Следуйте инструкциям на экране, чтобы назначить имя группе ресурсов развертывания и имени веб-сайта.   Этот сценарий развертывает микрослужбы и их зависимости платформы Azure в группе ресурсов в подписке Azure.  Скрипт также регистрирует приложение в клиенте Azure Active Directory (AAD) для поддержки проверки подлинности на основе OAUTH.  Развертывание займет несколько минут.  Пример того, что можно увидеть после успешного развертывания решения:

   ![Промышленные IoT OPC двойника развертывание в существующий проект](media/howto-opc-twin-deploy-existing/opc-twin-deploy-existing1.png)

   Выходные данные включают URL-адрес общедоступной конечной точки. 

3. После успешного завершения скрипта укажите, нужно ли сохранить `.env` файл.  Если требуется подключиться `.env` к облачной конечной точке с помощью таких средств, как консоль или развертывание модулей для разработки и отладки, необходим файл среды.

## <a name="troubleshooting-deployment-failures"></a>Устранение неполадок при развертывании

### <a name="resource-group-name"></a>Имя группы ресурсов

Убедитесь, что используется короткое и простое имя группы ресурсов.  Имя также используется для присвоения имен ресурсам, которые должны соответствовать требованиям к именованию ресурсов.  

### <a name="website-name-already-in-use"></a>Имя веб-сайта уже используется

Возможно, имя веб-сайта уже используется.  Если вы выпустили эту ошибку, необходимо использовать другое имя приложения.

### <a name="azure-active-directory-aad-registration"></a>Регистрация Azure Active Directory (AAD)

Сценарий развертывания пытается зарегистрировать два приложения AAD в Azure Active Directory.  В зависимости от ваших прав доступа к выбранному клиенту AAD развертывание может завершиться ошибкой. Существуют два варианта:

1. Если вы выбрали клиент AAD из списка клиентов, перезапустите сценарий и выберите другой из списка.
2. Кроме того, можно развернуть частный клиент AAD в другой подписке, перезапустить сценарий и выбрать его для использования.

> [!WARNING]
> НИКОГДА не продолжайте без проверки подлинности.  Если вы решили сделать это, любой пользователь сможет получить доступ к конечным точкам OPC двойника из Интернета без проверки подлинности.   Всегда можно выбрать [вариант развертывания "локальный"](howto-opc-twin-deploy-dependencies.md) , чтобы начать с самого начала.

## <a name="deploy-an-all-in-one-industrial-iot-services-demo"></a>Развертывание встроенной демонстрации служб IoT "все в одном"

Вместо служб и зависимостей можно также развернуть демонстрацию "все в одном".  В одной демонстрации содержится три сервера OPC UA, модуль двойника OPC, все микрослужбы и пример веб-приложения.  Он предназначен для демонстрационных целей.

1. Убедитесь, что у вас есть клон репозитория (см. выше). Откройте командную строку PowerShell в корне репозитория и выполните следующую команду:

    ```powershell
    set-executionpolicy -ExecutionPolicy Unrestricted -Scope Process
    .\deploy -type demo
    ```

2. Следуйте инструкциям на экране, чтобы назначить новое имя для группы ресурсов и имя веб-сайта.  После успешного развертывания скрипт будет отображать URL-адрес конечной точки приложения.

## <a name="deployment-script-options"></a>Параметры скрипта развертывания

Скрипт принимает следующие параметры:

```powershell
-type
```

Тип развертывания (виртуальная машина, локальная, демонстрация)

```powershell
-resourceGroupName
```

Может быть именем существующей или новой группы ресурсов.

```powershell
-subscriptionId
```

(Необязательно) идентификатор подписки, в которой будут развернуты ресурсы.

```powershell
-subscriptionName
```

Или имя подписки.

```powershell
-resourceGroupLocation
```

Необязательно, расположение группы ресурсов. Если указано, попытается создать новую группу ресурсов в этом расположении.

```powershell
-aadApplicationName
```

Имя приложения AAD для регистрации.

```powershell
-tenantId
```

Клиент AAD для использования.

```powershell
-credentials
```

## <a name="next-steps"></a>Следующие шаги

Теперь, когда вы узнали, как развернуть двойника OPC в существующем проекте, предлагаем следующий шаг:

> [!div class="nextstepaction"]
> [Защита обмена данными между клиентом OPC и OPC «компьютер»](howto-opc-vault-deploy-existing-client-plc-communication.md)
