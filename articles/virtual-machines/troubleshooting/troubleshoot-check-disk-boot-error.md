---
title: Проверка файловой системы при загрузке виртуальной машины Azure | Документация Майкрософт
description: Узнайте, как устранить проблему, когда отображается уведомление "Проверка файловой системы" при загрузке виртуальной машины
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
ms.date: 08/31/2018
ms.author: genli
ms.openlocfilehash: 51a97443f6b9ba2a37fa2db708b8520a9c450000
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "60594803"
---
# <a name="windows-shows-checking-file-system-when-booting-an-azure-vm"></a>Отображение в Windows уведомления "Проверка файловой системы" при загрузке виртуальной машины Azure

В этой статье описывается ошибка "Проверка файловой системы", которая может возникнуть при загрузке виртуальной машины Windows (VM) в Microsoft Azure.

> [!NOTE] 
> В Azure предлагаются две модели развертывания для создания ресурсов и работы с ними: [модель развертывания с помощью Resource Manager и классическая модель](../../azure-resource-manager/resource-manager-deployment-model.md). В этой статье описывается использование модели развертывания c помощью Resource Manager. Для новых развертываний рекомендуется использовать эту модель вместо классической.

## <a name="symptom"></a>Симптом 

Виртуальная машина Windows не запускается. Когда вы проверяете скриншоты загрузки в окне [Диагностика загрузки](boot-diagnostics.md), видно, что процесс "Проверка диска" (chkdsk.exe) работает с одним из следующих сообщений.

- Проверка и восстановление диска (C:)
- Проверка файловой системы на диске С.

## <a name="cause"></a>Причина:

Если в файловой системе обнаружена ошибка NTFS, Windows проверит и восстановит согласованность диска при следующем перезапуске. Обычно это происходит, если у виртуальной машины был неожиданный перезапуск, или если процесс ее завершения был прерван.

## <a name="solution"></a>Решение 

Windows будет загружаться нормально после завершения процесса проверки диска. Если виртуальная машина зависает в процессе проверки диска, попробуйте запустить проверку диска на виртуальной машине в автономном режиме.
1.  Сделайте снимок диска ОС затронутой виртуальной машины в качестве резервной копии. Дополнительные сведения см. в статье [Создание моментального снимка](../windows/snapshot-copy-managed-disk.md).
2.  [Устранение неполадок с виртуальной машиной Windows при подключении диска операционной системы к виртуальной машине восстановления с помощью портала Azure](troubleshoot-recovery-disks-portal-windows.md).  
3.  Для восстановления виртуальной машины запустите проверку диска на подключенном диске ОС. В следующем примере имя драйвера подключенного диска ОС является папка E. 
        
        chkdsk E: /f
4.  После завершения проверки диска отсоедините диск от виртуальной машины восстановления, а затем снова присоедините диск к затронутой виртуальной машине в качестве диска ОС. Дополнительные сведения см. в разделе [Устранение неполадок с виртуальной машиной Windows при подключении диска операционной системы к виртуальной машине восстановления с помощью Azure PowerShell](troubleshoot-recovery-disks-portal-windows.md).
