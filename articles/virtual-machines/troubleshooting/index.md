---
layout: LandingPage
description: Сведения об устранении неполадок с развертыванием виртуальных машин.
title: Документация по устранению неполадок с виртуальными машинами Azure | Документация Майкрософт
services: virtual-machines
author: genlin
manager: gwallace
ms.assetid: ''
ms.service: virtual-machines
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: landing-page
ms.date: 10/3/2018
ms.author: genli
ms.openlocfilehash: 47db174042f7dd2e9d1bd1503c8e8b8f635b4dbe
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/09/2019
ms.locfileid: "67705747"
---
# <a name="troubleshooting-azure-virtual-machines"></a>Устранение неполадок с виртуальными машинами Azure

- Ошибки распределения ресурсов
    - [Ошибки выделения ресурсов](allocation-failure.md)
    - [Ошибки распределения ресурсов для классических развертываний](allocation-failure-classic.md)
- [Диагностика загрузки](boot-diagnostics.md)
- RDP
    - [Сброс RDP](reset-rdp.md)
    - [Устранение неполадок с RDP](troubleshoot-rdp-connection.md)
    - [Подробные сведения об устранении неполадок с RDP](detailed-troubleshoot-rdp.md)
    - [Устранение определенных ошибок](troubleshoot-specific-rdp-errors.md)
- SSH 
    - [Устранение неполадок с SSH](troubleshoot-ssh-connection.md)
    - [Подробные сведения об устранении неполадок с SSH](detailed-troubleshoot-ssh-connection.md)
    - [Распространенные сообщения об ошибках](error-messages.md)
    - [Проблемы с производительностью виртуальных машин Windows](performance-diagnostics.md  )
    - [Как использовать PerfInsights](how-to-use-perfInsights.md)
    - [Расширение для диагностики производительности](performance-diagnostics-vm-extension.md)
- [Установка агента виртуальной машины Windows в автономном режиме](install-vm-agent-offline.md)
- Повторное развертывание виртуальной машины
    - [Linux](redeploy-to-new-node-linux.md)
    - [Windows](redeploy-to-new-node-windows.md)
- Сброс пароля к виртуальной машине
    - [Windows](reset-local-password-without-agent.md)
    - [Linux](reset-password.md)
- [Сброс сетевого адаптера](reset-network-interface.md)
- [Перезапуск или изменение размера виртуальной машины](restart-resize-error-troubleshooting.md)
- Использование последовательной консоли
    - [Виртуальные машины Linux](serial-console-linux.md)
        - [Загрузчик GRUB в последовательной консоли (однопользовательский режим)](serial-console-grub-single-user-mode.md)
        - [Немаскируемое прерывание в последовательной консоли (SysRq)](serial-console-nmi-sysrq.md)
    - [Виртуальные машины Windows](serial-console-windows.md)
        - [CMD и команды PowerShell](serial-console-cmd-ps-commands.md)
- [Ошибки при удалении ресурсов хранилища](storage-resource-deletion-errors.md      )
- [Непредвиденные перезагрузки виртуальных машин с подключенными VHD](unexpected-reboots-attached-vhds.md)
- [Проблемы с активацией Windows](troubleshoot-activation-problems.md)
- [Ошибки доступа к приложению](troubleshoot-app-connection.md)
- Устранение неполадок развертываний
    - [Linux](troubleshoot-deploy-vm-linux.md)
    - [Windows](troubleshoot-deploy-vm-windows.md)
- [Изменение имен устройств](troubleshoot-device-names-problems.md)
- Доступ к восстановлению виртуальной машины
    - Windows
        - [PowerShell](troubleshoot-recovery-disks-windows.md)
        - [Портал Azure](troubleshoot-recovery-disks-portal-windows.md)
    - Linux
        - [CLI](troubleshoot-recovery-disks-linux.md)
    - [Портал Azure](troubleshoot-recovery-disks-portal-linux.md)
- [Ошибки при загрузке](boot-error-troubleshoot.md)
- [Ошибки BitLocker](troubleshoot-bitlocker-boot-error.md)
- [Проверка ошибок файловой системы](troubleshoot-check-disk-boot-error.md)
- [Отображение ошибки "синий экран" при загрузке виртуальной машины Azure под управлением Windows](troubleshoot-common-blue-screen-error.md)
- [Ошибки регулирования](troubleshooting-throttling-errors.md)
- [Использование вложенной виртуализации](troubleshoot-vm-by-use-nested-virtualization.md)
- [Общие сведения о перезагрузке системы](understand-vm-reboot.md)

