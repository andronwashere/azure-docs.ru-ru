---
title: Как сбросить локальный пароль Linux на виртуальных машинах Azure | Документация Майкрософт
description: Описывается, как сбросить локальный пароль Linux на виртуальной машине Azure.
services: virtual-machines-linux
documentationcenter: ''
author: Deland-Han
manager: cshepard
editor: ''
tags: ''
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.topic: troubleshooting
ms.date: 06/15/2018
ms.author: delhan
ms.openlocfilehash: d96d75f4f2623476f7af4e6eea930c1f2c503e3a
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "60306957"
---
# <a name="how-to-reset-local-linux-password-on-azure-vms"></a>Как сбросить локальный пароль Linux на виртуальных машинах Azure

В этой статье описано несколько способов сброса локальных паролей на виртуальных машинах Linux. Если истек срок действия учетной записи пользователя или требуется создать новую учетную запись, можно использовать приведенные ниже способы, чтобы создать новую учетную запись локального администратора и повторно получить доступ к виртуальной машине.

## <a name="symptoms"></a>Проблемы

Невозможно подключиться к виртуальной машине, и появляется сообщение о том, что указан неправильный пароль. Кроме того, с помощью VMAgent невозможно сбросить пароль на портале Azure.

## <a name="manual-password-reset-procedure"></a>Процедура сброса пароля вручную

1.  Удалите виртуальную машину и сохраните ее подключенные диски.

2.  Подключите диск ОС в качестве диска данных к другой временной виртуальной машине в том же расположении.

3.  Выполните приведенную ниже команду SSH на временной виртуальной машине, чтобы стать суперпользователем.

    ```bash
    sudo su
    ```

4.  Выполните **fdisk -l** или просмотрите системные журналы, чтобы найти только что подключенный диск. Найдите имя подключаемого диска. Затем на временной виртуальной машине просмотрите соответствующий файл журнала.

    ```bash
    grep SCSI /var/log/kern.log (ubuntu)
    grep SCSI /var/log/messages (centos, suse, oracle)
    ```

    Ниже приведен пример выходных данных команды grep.

    ```bash
    kernel: [ 9707.100572] sd 3:0:0:0: [sdc] Attached SCSI disk
    ```

5.  Создайте точку подключения **tempmount**.

    ```bash
    mkdir /tempmount
    ```

6.  Подключите диск ОС к точке подключения. Обычно требуется подключить *sdc1* или *sdc2*. Это зависит от раздела размещения в каталоге */etc* диска неисправного компьютера.

    ```bash
    mount /dev/sdc1 /tempmount
    ```

7.  Перед внесением любых изменений создайте копии основных файлов учетных данных:

    ```bash
    cp /etc/passwd /etc/passwd_orig    
    cp /etc/shadow /etc/shadow_orig    
    cp /tempmount/etc/passwd /etc/passwd
    cp /tempmount/etc/shadow /etc/shadow 
    cp /tempmount/etc/passwd /tempmount/etc/passwd_orig
    cp /tempmount/etc/shadow /tempmount/etc/shadow_orig
    ```

8.  Сбросьте соответствующий пароль пользователя.

    ```bash
    passwd <<USER>> 
    ```

9.  Переместите измененные файлы на правильное расположение на диске неисправного компьютера.

    ```bash
    cp /etc/passwd /tempmount/etc/passwd
    cp /etc/shadow /tempmount/etc/shadow
    cp /etc/passwd_orig /etc/passwd
    cp /etc/shadow_orig /etc/shadow
    ```

10. Вернитесь к корневому каталогу и отключите диск.

    ```bash
    cd /
    umount /tempmount
    ```

11. Отсоедините диск от портала управления.

12. Повторно создайте виртуальную машину.

## <a name="next-steps"></a>Дальнейшие действия

* [Troubleshoot Azure VM by attaching OS disk to another Azure VM](https://social.technet.microsoft.com/wiki/contents/articles/18710.troubleshoot-azure-vm-by-attaching-os-disk-to-another-azure-vm.aspx) (Устранение неполадок виртуальной машины Azure путем присоединения диска ОС к другой виртуальной машине Azure)

* [Azure CLI: Как удалить и повторно развернуть виртуальную Машину из виртуального жесткого диска](https://blogs.msdn.microsoft.com/linuxonazure/2016/07/21/azure-cli-how-to-delete-and-re-deploy-a-vm-from-vhd/)
