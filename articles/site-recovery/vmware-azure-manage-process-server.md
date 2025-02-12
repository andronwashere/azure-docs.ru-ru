---
title: Управление сервером обработки, используемый для аварийного восстановления виртуальных машин VMware и физических серверов в Azure с помощью Azure Site Recovery | Документация Майкрософт
description: В этой статье описывается управление сервером обработки для аварийного восстановления виртуальных машин VMware и физических серверов в Azure с помощью Azure Site Recovery.
author: Rajeswari-Mamilla
manager: rochakm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 04/28/2019
ms.author: ramamill
ms.openlocfilehash: 2c27779719c73adf4d7fc1a61a0c77d03df71815
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "64925629"
---
# <a name="manage-process-servers"></a>Управление серверами обработки

В этой статье описываются распространенные задачи по управлению сервера обработки Site Recovery.

Сервер обработки используется для получения, оптимизировать и отправляют данные репликации в Azure. Он также выполняет принудительную установку службы Mobility Service на виртуальных машинах VMware и физических серверов, которые требуется реплицировать, и выполняет автоматическое обнаружение локальных компьютеров. Для репликации локальных виртуальных машин VMware или физических серверов в Azure, сервер обработки устанавливается по умолчанию на компьютере сервера конфигурации. 

- Для больших развертываний могут понадобиться дополнительные серверы обработки в локальной среде, чтобы изменять масштаб емкости.
- Восстановление размещения из Azure в локальной необходимо настроить временный сервер обработки в Azure. Восстановив размещение, вы можете удалить эту виртуальную машину. 

Дополнительные сведения о сервере обработки.


## <a name="upgrade-a-process-server"></a>Обновление сервера обработки

При развертывании сервера обработки в локальной среде или как виртуальная машина Azure для восстановления размещения, будет установлена последняя версия сервера обработки. Команды разработчиков Site Recovery регулярно выпускают исправления и улучшения, и мы рекомендуем вам систематически обновлять свои серверы обработки. Сервер обработки можно обновить следующим образом:

[!INCLUDE [site-recovery-vmware-upgrade -process-server](../../includes/site-recovery-vmware-upgrade-process-server-internal.md)]


## <a name="move-vms-to-balance-the-process-server-load"></a>Перемещение виртуальных машин для балансировки нагрузки сервера обработки

Балансировка нагрузки, переместив виртуальные машины между двумя серверами обработки, следующим образом:

1. В хранилище в разделе **управление** щелкните **инфраструктура Site Recovery**. В разделе **для VMware и физических машин**, нажмите кнопку **серверы конфигурации**.
2. Щелкните сервер конфигурации, с помощью которого зарегистрированы серверы обработки.
3. Щелкните сервер обработки, для которого требуется балансировать нагрузку трафика.

    ![LoadBalance](media/vmware-azure-manage-process-server/LoadBalance.png)

4. Нажмите кнопку **балансировать нагрузку**, Выбор целевого сервера обработки, к которому вы хотите переместить машин. Нажмите кнопку **ОК**

    ![LoadPS](media/vmware-azure-manage-process-server/LoadPS.PNG)

2. Нажмите кнопку **Выбор машин**и выберите компьютеры, которые вы хотите переместить из текущего процесса на целевой сервер. Сведения о среднем значении изменения данных отображаются возле каждой виртуальной машины. Нажмите кнопку **ОК**. 
3. В хранилище, отслеживать ход выполнения задания в разделе **мониторинг** > **задания Site Recovery**.

Займет около 15 минут для изменения отобразятся на портале. Быстрее эффект [обновить конфигурацию сервера](vmware-azure-manage-configuration-server.md#refresh-configuration-server).

## <a name="switch-an-entire-workload-to-another-process-server"></a>Переключиться на другой сервер обработки всей рабочей нагрузки

Перемещение всей рабочей нагрузки, обрабатываются сервером обработки на другой сервер обработки, следующим образом:

1. В хранилище в разделе **управление** щелкните **инфраструктура Site Recovery**. В разделе **для VMware и физических машин**, нажмите кнопку **серверы конфигурации**.
2. Щелкните сервер конфигурации, с помощью которого зарегистрированы серверы обработки.
3. Щелкните сервер обработки, из которого нужно выполнить переключение рабочей нагрузки.
4. Щелкните **коммутатора**, Выбор целевого сервера обработки, к которому вы хотите переместить рабочую нагрузку. Нажмите кнопку **ОК**

    ![Параметр](media/vmware-azure-manage-process-server/Switch.PNG)

5. В хранилище, отслеживать ход выполнения задания в разделе **мониторинг** > **задания Site Recovery**.

Займет около 15 минут для изменения отобразятся на портале. Быстрее эффект [обновить конфигурацию сервера](vmware-azure-manage-configuration-server.md#refresh-configuration-server).



## <a name="reregister-a-process-server"></a>Повторная регистрация сервера обработки

Повторная регистрация сервера обработки, выполняемого в локальной или виртуальной Машине Azure с сервером конфигурации следующим образом:

[!INCLUDE [site-recovery-vmware-register-process-server](../../includes/site-recovery-vmware-register-process-server.md)]

Сохранив параметры, сделайте следующее:

1. Откройте командную строку администратора на сервере обработки.
2. Перейдите к папке **%PROGRAMDATA%\ASR\Agent** и выполните эту команду:

    ```
    cdpcli.exe --registermt
    net stop obengine
    net start obengine
    ```

## <a name="modify-proxy-settings-for-an-on-premises-process-server"></a>Изменение параметров прокси-сервера для сервера обработки в локальной среде

Если локальный сервер обработки использует прокси-сервер для подключения к Azure, можно изменить параметры прокси-сервера следующим образом:

1. Войдите на компьютер сервера обработки. 
2. Откройте командную строку PowerShell с правами администратора и выполните следующую команду.
   ```powershell
   $pwd = ConvertTo-SecureString -String MyProxyUserPassword
   Set-OBMachineSetting -ProxyServer http://myproxyserver.domain.com -ProxyPort PortNumber –ProxyUserName domain\username -ProxyPassword $pwd
   net stop obengine
   net start obengine
   ```
2. Перейдите к папке **%PROGRAMDATA%\ASR\Agent**, и выполните следующую команду:
   ```
   cmd
   cdpcli.exe --registermt

   net stop obengine

   net start obengine

   exit
   ```

## <a name="remove-a-process-server"></a>Удаление сервера обработки

[!INCLUDE [site-recovery-vmware-unregister-process-server](../../includes/site-recovery-vmware-unregister-process-server.md)]

## <a name="exclude-folders-from-anti-virus-software"></a>Исключение папок из антивирусного программного обеспечения

Если антивирусная программа работает под управлением сервера обработки масштабирования (или главный целевой сервер), исключите следующие папки из антивирусной операций:


- C:\Program Files\Microsoft Azure Recovery Services Agent
- C:\ProgramData\ASR.
- C:\ProgramData\ASRLogs.
- C:\ProgramData\ASRSetupLogs.
- C:\ProgramData\LogUploadServiceLogs.
- C:\ProgramData\Microsoft Azure Site Recovery.
- Каталог установки сервера обработки. Пример: C:\Program Files (x86)\Microsoft Azure Site Recovery