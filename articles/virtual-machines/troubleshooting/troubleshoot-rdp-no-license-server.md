---
title: Сервер лицензирования удаленных рабочих столов недоступен при подключении к виртуальной машине Azure | Документация Майкрософт
description: Как устранить сбой RDP из-за отсутствия сервера лицензирования удаленных рабочих столов | Документация Майкрософт
services: virtual-machines-windows
documentationCenter: ''
author: genlin
manager: cshepard
editor: ''
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: troubleshooting
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 10/23/2018
ms.author: genli
ms.openlocfilehash: 550b971602d1736e0ba3981a5b7ca546862ea034
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "60318958"
---
# <a name="remote-desktop-license-server-isnt-available-when-you-connect-to-an-azure-vm"></a>Сервер лицензирования удаленных рабочих столов недоступен при подключении к виртуальной машине Azure

Эта статья поможет устранить проблему, при которой не удается подключиться к виртуальной машине Azure, поскольку для предоставления лицензии нет сервера лицензирования удаленных рабочих столов.

## <a name="symptoms"></a>Проблемы

При попытке подключиться к виртуальной машине возникают приведенные ниже сценарии.

- Снимок экрана виртуальной машины, на котором операционная система полностью загружена и ожидает учетные данные.
- Вы увидите следующие сообщения об ошибках при попытке подключиться по протоколу удаленного рабочего стола Майкрософт (RDP):

  - Удаленный сеанс отключен, так как отсутствуют доступные серверы лицензирования удаленных рабочих столов, которые могли бы провести лицензирование.

  - Сервер лицензирования удаленных рабочих столов недоступен. Службы удаленных рабочих столов перестанут работать, поскольку для этого компьютера завершен льготный период и он не связался с допустимым сервером лицензирования Windows Server 2008. Выберите это сообщение, чтобы открыть конфигурацию сервера узла сеансов удаленных рабочих столов для диагностики лицензирования.

Тем не менее вы можете подключиться к виртуальной машине обычным образом с помощью сеанса с правами администратора:

```
mstsc /v:<Server>[:<Port>] /admin
```

## <a name="cause"></a>Причина:

Эта проблема возникает, когда сервер лицензирования удаленных рабочих столов недоступен и не может предоставить лицензию для начала удаленного сеанса. Эта проблема может возникнуть в связи с несколькими сценариями, даже если роль узла сеансов удаленных рабочих столов настроена на виртуальной машине.

- В среде никогда не было роли лицензирования удаленных рабочих столов, и льготный период (180 дней) завершен.
- Лицензия удаленного рабочего стола установлена в среде, но не активирована.
- Лицензия удаленного рабочего стола в среде не имеет лицензий клиентского доступа (CAL) для настройки подключения.
- Лицензия удаленного рабочего стола была установлена в среде. Лицензии клиентского доступа существуют, но настроены неправильно.
- Лицензия удаленного рабочего стола имеет лицензии клиентского доступа и была активирована. Но другие проблемы на сервере лицензирования удаленных рабочих столов мешают предоставлению лицензий в среде.

## <a name="solution"></a>Решение

Чтобы устранить эту проблему, [выполните резервное копирование диска операционной системы](../windows/snapshot-copy-managed-disk.md) и следуйте инструкциям:

1. Подключитесь к виртуальной машине с помощью сеанса с правами администратора:

   ```
   mstsc /v:<Server>[:<Port>] /admin
   ```

    Если не удается подключиться к виртуальной машине с помощью сеанса с правами администратора, можно использовать [последовательную консоль виртуальной машины в Azure](serial-console-windows.md), чтобы получить доступ к виртуальной машине следующим образом.

    1. Откройте последовательную консоль, выбрав **Поддержка и устранение неисправностей** > **Последовательная консоль (предварительная версия)** . Если эта функция включена на виртуальной машине, вы сможете успешно подключиться к виртуальной машине.

    2. Создайте канал для экземпляра командной строки. Введите **CMD**, чтобы запустить канал и получить имя канала.

    3. Переключитесь на канал, который запускает экземпляр CMD. В нашем случае это должен быть канал 1:

       ```
       ch -si 1
       ```

    4. Еще раз нажмите клавишу **ВВОД** и введите допустимое имя пользователя и пароль, локальный или доменный идентификатор для виртуальной машины.

2. Проверьте, включена ли на виртуальной машине роль узла сеансов удаленных рабочих столов. Если роль включена, убедитесь, что она работает правильно. Откройте экземпляр CMD с повышенными привилегиями и выполните следующие действия:

    1. Чтобы проверить состояние роли узла сеансов удаленных рабочих столов, используйте следующую команду:

       ```
        reg query "HKLM\SOFTWARE\Microsoft\ServerManager\ServicingStorage\ServerComponentCache\RDS-RD-Server" /v InstallState
        ```

        Если команда возвращает значение 0, это означает, что роль отключена и можно перейти к шагу 3.

    2. Чтобы проверить политики и правильно настроить их, используйте следующую команду:

       ```
        reg query "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\RCM\Licensing Core" /v LicensingMode reg query "HKLM\SYSTEM\CurrentControlSet\Services\TermService\Parameters" /v SpecifiedLicenseServers
       ```

        Если **LicensingMode** имеет значение, отличное от 4 (на пользователя), установите значение 4.

         ```
        reg add "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\RCM\Licensing Core" /v LicensingMode /t REG_DWORD /d 4
        ```

       Если значение **SpecifiedLicenseServers** не существует или имеет неверные сведения о сервере лицензирования, измените его, как показано ниже.

       ```
        reg add "HKLM\SYSTEM\CurrentControlSet\Services\TermService\Parameters" /v SpecifiedLicenseServers /t REG_MULTI_SZ /d "<FQDN / IP License server>"
       ```

    3. После внесения изменения в реестр перезапустите виртуальную машину.

    4. Если у вас нет лицензий клиентского доступа, удалите роль узла сеансов удаленных рабочих столов. Затем протокол удаленного рабочего стола вернется в нормальное состояние. К виртуальной машине можно подключить только два параллельных протокола удаленного рабочего стола.

        ```
       dism /ONLINE /Disable-feature /FeatureName:Remote-Desktop-Services
        ```

        Если виртуальная машина имеет роль лицензирования удаленного рабочего стола и она не используется, можно также удалить эту роль.

       ```
        dism /ONLINE /Disable-feature /FeatureName:Licensing
       ```

    5. Убедитесь, что виртуальная машина может подключиться к серверу лицензирования удаленных рабочих столов. Проверьте возможность подключения к порту 135 между виртуальной машиной и сервером лицензирования. 

       ```
       telnet <FQDN / IP License Server> 135
       ```

3. Если в среде нет сервера лицензирования удаленных рабочих столов, но вам он нужен, [установите службу роли лицензирования удаленного рабочего стола](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc731765(v=ws.11)). Затем [настройте лицензирование RDS](https://blogs.technet.microsoft.com/askperf/2013/09/20/rd-licensing-configuration-on-windows-server-2012/).

4. Если сервер лицензирования удаленных рабочих столов работает правильно, убедитесь, что сервер лицензирования удаленных рабочих столов активирован с помощью лицензий клиентского доступа.

## <a name="need-help-contact-support"></a>Требуется помощь? Обратитесь в службу поддержки.

Если вам все еще нужна помощь, [обратитесь в службу поддержки](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade), которая поможет быстро устранить проблему.
