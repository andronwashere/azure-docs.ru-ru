---
title: Развертывание центра безопасности Azure для модуля IoT Edge | Документация Майкрософт
description: Узнайте о том, как развернуть агент безопасности Интернета вещей в IoT Edge в центре безопасности Azure.
services: asc-for-iot
ms.service: asc-for-iot
documentationcenter: na
author: mlottner
manager: rkarlin
editor: ''
ms.assetid: 32a9564d-16fd-4b0d-9618-7d78d614ce76
ms.subservice: asc-for-iot
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/1/2019
ms.author: mlottner
ms.openlocfilehash: 49ed4c6515f8fb63c3331b05e1bb29b13985e6b3
ms.sourcegitcommit: 6a42dd4b746f3e6de69f7ad0107cc7ad654e39ae
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/07/2019
ms.locfileid: "67618323"
---
# <a name="deploy-a-security-module-on-your-iot-edge-device"></a>Развертывание модуля безопасности на вашем устройстве IoT Edge

> [!IMPORTANT]
> Центр безопасности Azure для Интернета вещей сейчас предоставляется в общедоступной предварительной версии.
> Эта предварительная версия предоставляется без соглашения об уровне обслуживания и не рекомендована для использования рабочей среде. Некоторые функции могут не поддерживаться или их возможности могут быть ограничены. Дополнительные сведения см. в статье [Дополнительные условия использования предварительных выпусков Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

**Azure центр безопасности (ASC) для Интернета вещей** модуль представляет собой решение полнофункциональных средств безопасности для устройства IoT Edge.
Модуль безопасности собирает и объединяет анализирует данные необработанной безопасности из операционной системы и контейнера системы в практические рекомендации по безопасности и оповещения.
Дополнительные сведения см. в разделе [модуль безопасности для IoT Edge](security-edge-architecture.md).

В этом руководстве вы узнаете, как развернуть модуль безопасности на вашем устройстве IoT Edge.

## <a name="deploy-security-module"></a>Развертывание модуля безопасности

Выполните следующие действия для развертывания ASC для модуля безопасности Интернета вещей для IoT Edge.

### <a name="prerequisites"></a>предварительные требования

- В центре Интернета вещей, убедитесь, что устройство является [зарегистрировано как устройство IoT Edge](https://docs.microsoft.com/azure/iot-edge/how-to-register-device-portal).

- ASC для модуля Edge Интернета вещей требует [AuditD framework](https://linux.die.net/man/8/auditd) устанавливается на устройстве IoT Edge.

    - Установите платформу, выполнив следующую команду на вашем устройстве IoT Edge:
   
      `sudo apt-get install auditd audispd-plugins`
   
    - Убедитесь, что AuditD активен, выполнив следующую команду:
   
      `sudo systemctl status auditd`
      
        Ожидаемый ответ `active (running)`. 

### <a name="deployment-using-azure-portal"></a>Развертывание с помощью портала Azure

1. На портале Azure откройте **Marketplace**.

1. Выберите **Интернета вещей**, а затем найдите **центра безопасности Azure для IoT** и выберите его.

   ![Выберите центр безопасности Azure для IoT](media/howto/edge-onboarding-8.png)

1. Нажмите кнопку **создать** для настройки развертывания. 

1. Выбор Azure **подписки** центра Интернета вещей, выберите ваш **центра Интернета вещей**.<br>Выберите **развертывание на устройстве** предназначенных для одного устройства или выбрать **развертывания в масштабе** ориентироваться на различные устройства, а затем нажмите кнопку **создать**. Дополнительные сведения о развертывании в масштабе см. в разделе [развертывание](https://docs.microsoft.com/azure/iot-edge/how-to-deploy-monitor). 

    >[!Note] 
    >Если вы выбрали **развертывания в масштабе**, добавьте имя устройства и сведения, прежде чем переходить к **Добавление модулей** вкладку в следующих инструкциях.     

Существует три действия, чтобы создать развертывание IoT Edge для центра безопасности Azure для Интернета вещей. В следующих разделах описан каждый из этих шагов. 

#### <a name="step-1-add-modules"></a>Шаг 1. Добавление модулей

1. Из **Добавление модулей** вкладке **развертывания модулей** область, нажмите кнопку **AzureSecurityCenterforIoT**. 
   
1. Изменение **имя** для **azureiotsecurity**.
1. Изменение **изображения URI** для **mcr.microsoft.com/ascforiot/azureiotsecurity:0.0.3**.
1. Проверьте **параметры создания контейнера** присваивается значение:      
    ``` json
    {
        "NetworkingConfig": {
            "EndpointsConfig": {
                "host": {}
            }
        },
        "HostConfig": {
            "Privileged": true,
            "NetworkMode": "host",
            "PidMode": "host",
            "Binds": [
                "/:/host"
            ]
        }
    }    
    ```
1. Убедитесь, что **требуемые свойства двойника модуля набор** выбран и изменить объект конфигурации, чтобы:
      
    ``` json
      "properties.desired": {
        "azureiot*com^securityAgentConfiguration^1*0*0": {
        }
      }
      ```

1. Нажмите кнопку **Сохранить**.
1. Прокрутите до нижней части вкладки и выберите **Настройка дополнительных параметров среды выполнения Edge**.
   
   >[!Note]
   > Сделать **не** AMQP взаимодействие для концентратора Edge Интернета вещей.
   > Центр безопасности Azure для Интернета вещей модуля требует связи AMQP с концентратором Edge Интернета вещей.
   
1. Изменение **изображение** под **концентратора Edge** для **mcr.microsoft.com/ascforiot/edgehub:1.0.9-preview**.

   >[!Note]
   > Центр безопасности Azure для Интернета вещей модуля требуется разветвленного версия Edge Интернета вещей, в зависимости от пакета SDK версии 1,20.
   > Изменение образа концентратора Edge Интернета вещей, вы разрешаете устройству IoT Edge для замены последнего стабильного выпуска разветвленного версии концентратор Edge Интернета вещей, который официально не поддерживается службой IoT Edge.

1. Проверьте **параметры создания** имеет значение: 
         
    ``` json
    {
      "HostConfig": {
        "PortBindings": {
          "8883/tcp": [{"HostPort": "8883"}],
          "443/tcp": [{"HostPort": "443"}],
          "5671/tcp": [{"HostPort": "5671"}]
        }
      }
    }
    ```
      
1. Нажмите кнопку **Сохранить**.
   
1. Нажмите кнопку **Далее**.

#### <a name="step-2-specify-routes"></a>Шаг 2. Укажите маршруты 

1. В **указания маршрутов** вкладке **ASCForIoTToIoTHub** маршрутизации **«/ messages/модули/azureiotsecurity /\* в $ вышестоящего источника»** и нажмите кнопку  **Далее**.

   ![Настройка маршрутов](media/howto/edge-onboarding-9.png)

#### <a name="step-3-review-deployment"></a>Шаг 3. Проверка развертывания

1. В **просмотрите развертывания** , просмотрите сведения о развертывании, а затем установите **отправить** для завершения развертывания.

## <a name="diagnostic-steps"></a>Этапы диагностики

Если возникают проблемы, журналы контейнера являются наилучший способ узнать о состоянии устройства IoT Edge модуль безопасности. Для сбора сведений используйте команды и инструменты, приведенные в этом разделе.

### <a name="verify-the-required-containers-are-installed-and-functioning-as-expected"></a>Убедитесь, что указаны необходимые контейнеры, установлена и работает должным образом

1. Выполните следующую команду на вашем устройстве IoT Edge:
    
     `sudo docker ps`
   
1. Убедитесь, что со следующими контейнерами:
   
   | ИМЯ | ИЗОБРАЖЕНИЕ |
   | --- | --- |
   | azureiotsecurity | mcr.Microsoft.com/ascforiot/azureiotsecurity:0.0.3 |
   | edgeHub | mcr.Microsoft.com/ascforiot/edgehub:1.0.9-Preview |
   | edgeAgent | mcr.Microsoft.com/azureiotedge-Agent:1.0 |
   
   Если Минимальная требуемая контейнеры не отображаются, проверьте, если манифест развертывания IoT Edge выравнивается с рекомендованными настройками. Дополнительные сведения см. в разделе [модуля IoT Edge, развернуть](#deployment-using-azure-portal).

### <a name="inspect-the-module-logs-for-errors"></a>Проверьте журналы модуля для ошибок
   
1. Выполните следующую команду на вашем устройстве IoT Edge:

   `sudo docker logs azureiotsecurity`
   
1. Для более подробных журналов, добавьте следующую переменную среды для **azureiotsecurity** развертывание модуля: `logLevel=Debug`.

## <a name="next-steps"></a>Следующие шаги

Дополнительные сведения о параметрах конфигурации, продолжайте практическим руководством по конфигурации модуля. 
> [!div class="nextstepaction"]
> [Практическое руководство по конфигурации модуля](./how-to-agent-configuration.md)
