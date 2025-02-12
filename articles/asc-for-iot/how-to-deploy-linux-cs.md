---
title: Руководство по установке и развертывании Linux C# агента центра безопасности Azure для Интернета вещей Preview | Документация Майкрософт
description: Сведения об установке агента IoT в центре безопасности Azure в 32-разрядных и 64-разрядной Linux.
services: asc-for-iot
ms.service: asc-for-iot
documentationcenter: na
author: mlottner
manager: rkarlin
editor: ''
ms.assetid: b0982203-c3c8-4a0b-8717-5b5ac4038d8c
ms.subservice: asc-for-iot
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/27/2019
ms.author: mlottner
ms.openlocfilehash: 808ff912a997a4c09a22048ada7546daab895701
ms.sourcegitcommit: 6a42dd4b746f3e6de69f7ad0107cc7ad654e39ae
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/07/2019
ms.locfileid: "67618261"
---
# <a name="deploy-azure-security-center-for-iot-c-based-security-agent-for-linux"></a>Развертывание центра безопасности Azure для IoT C#-основе security agent для Linux

> [!IMPORTANT]
> Центр безопасности Azure для Интернета вещей сейчас предоставляется в общедоступной предварительной версии.
> Эта предварительная версия предоставляется без соглашения об уровне обслуживания и не рекомендована для использования рабочей среде. Некоторые функции могут не поддерживаться или их возможности могут быть ограничены. Дополнительные сведения см. в статье [Дополнительные условия использования предварительных выпусков Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

В этом руководстве объясняется, как установить и развернуть центр безопасности Azure (ASC) для Интернета вещей C#-агент безопасности на базе Linux.

Из этого руководства вы узнаете, как выполнить следующие задачи: 
> [!div class="checklist"]
> * Установка
> * Проверка развертывания
> * Удаление агента.
> * Устранение неполадок 

## <a name="prerequisites"></a>Предварительные требования

Другие платформы и версии агента, см. в разделе [выберите агент защиты](how-to-deploy-agent.md).

1. Для развертывания агента безопасности требуются права локального администратора на компьютере, используемом для установки. 

1. [Создайте модуль безопасности](quickstart-create-security-twin.md) для устройства.

## <a name="installation"></a>Установка 

Чтобы развернуть агент безопасности, выполните следующие действия.

1. На сайте [GitHub](https://aka.ms/iot-security-github-cs) скачайте самую последнюю версию на свой компьютер.

1. Извлеките содержимое пакета и перейдите в папку _/Install_.

1. Добавьте права на выполнение к **скрипту InstallSecurityAgent**, выполнив следующую команду `chmod +x InstallSecurityAgent.sh`. 

1. Затем выполните приведенную ниже команду: 

   ```
   ./InstallSecurityAgent.sh -i -aui <authentication identity>  -aum <authentication method> -f <file path> -hn <host name>  -di <device id> -cl <certificate location kind>
   ```
   
   Дополнительные сведения о параметрах проверки подлинности см. в [этой статье](concept-security-agent-authentication-methods.md).

Скрипт выполняет следующее:

- Установка необходимых компонентов.

- Добавление пользователя службы (с отключенным интерактивным входом).

- Установка агента в качестве **управляющей программы** (это предполагает, что устройство использует **systemd** для управления службой).

- Редактирование файла **sudoers** таким образом, чтобы позволить агенту выполнять определенные задачи от имени пользователя с правами root.

- Настройка агента с предоставленными параметрами проверки подлинности.


Для получения дополнительной справки запустите скрипт с параметром –help: `./InstallSecurityAgent.sh --help`.

### <a name="uninstall-the-agent"></a>Удаление агента

Чтобы удалить агент, запустите скрипт с параметром –u: `./InstallSecurityAgent.sh -u`. 

> [!NOTE]
> Удаление не приводит к удалению всех ранее отсутствующих необходимых компонентов, которые были установлены во время установки.

## <a name="troubleshooting"></a>Устранение неполадок  

1. Проверьте состояние развертывания, выполнив следующую команду:

    `systemctl status ASCIoTAgent.service`

2. Включите ведение журналов.  
   Если агент не запускается, включите ведение журнала, чтобы просмотреть дополнительные сведения.

   Чтобы включить ведение журнала:

   1. Откройте файл конфигурации для редактирования в любом редакторе Linux:

        `vi /var/ASCIoTAgent/General.config`

   1. Измените следующие значения: 

      ```
      <add key="logLevel" value="Debug"/>
      <add key="fileLogLevel" value="Debug"/>
      <add key="diagnosticVerbosityLevel" value="Some" /> 
      <add key="logFilePath" value="IotAgentLog.log"/>
      ```
       Значение **logFilePath** настраивается. 

       > [!NOTE]
       > Мы рекомендуем **отключить** ведение журнала после устранения неполадок. Если оставить ведение журнала **включенным**, размер файла журнала и использование данных увеличится.

   1. Перезапустите агент, выполнив команду:

       `systemctl restart ASCIoTAgent.service`

   1. Просмотрите дополнительные сведения о сбое в файле журнала.  

       Расположение файла журнала: `/var/ASCIoTAgent/IotAgentLog.log`.

       Измените путь к расположению файла в соответствии с именем, выбранным для значения **logFilePath** на шаге 2. 

## <a name="next-steps"></a>Следующие шаги

- Просмотрите [обзор](overview.md) службы ASC для Интернета вещей.
- Ознакомьтесь с дополнительными сведениями об [архитектуре](architecture.md) ASC для Интернета вещей.
- Включите [службу](quickstart-onboard-iot-hub.md).
- Просмотрите [часто задаваемые вопросы](resources-frequently-asked-questions.md).
- Ознакомьтесь со сведениями об [оповещениях](concept-security-alerts.md).