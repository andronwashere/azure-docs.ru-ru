---
title: Включить удаленный рабочий стол для Linux в службы лабораторий Azure | Документация Майкрософт
description: Сведения о включении удаленного рабочего стола для виртуальных машин Linux в лаборатории в лаборатории служб Azure.
services: lab-services
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/24/2019
ms.author: spelluru
ms.openlocfilehash: 389d467bd9672743d4a086e8a1c505fb0366dba7
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "66237130"
---
# <a name="enable-and-use-remote-desktop-for-linux-virtual-machines-in-a-lab-in-azure-lab-services"></a>Включение и использование удаленного рабочего стола для виртуальных машин Linux в лаборатории в лаборатории служб Azure
В этой статье показано, как выполнить следующие задачи:

- Включить удаленный рабочий стол для виртуальной Машины Linux
- Как преподаватель может подключиться к шаблону виртуальной Машины с помощью подключения удаленного рабочего стола (RDP).
- Как учащихся подключаться к учащегося Машине по протоколу RDP

## <a name="enable-remote-desktop-for-linux-vm"></a>Включить удаленный рабочий стол для виртуальной Машины Linux
Во время создания лаборатории, можно включить преподавателей **удаленный рабочий стол** для **Linux** изображения. **Включить удаленный рабочий стол** вариант отображается при выборе образа Linux для шаблона. Если этот параметр включен, преподавателей можно подключиться к шаблона виртуальной Машины и виртуальные машины учащегося с помощью протокола удаленного рабочего СТОЛА (удаленного рабочего стола). 

![Включить удаленный рабочий стол для образов Linux](../media/how-to-enable-remote-desktop-linux/enable-rdp-option.png)

На **Включение удаленного рабочего стола** окно сообщения, выберите **продолжить с удаленным рабочим столом**. 

![Включить удаленный рабочий стол для образов Linux](../media/how-to-enable-remote-desktop-linux/enabling-remote-desktop-connection-dialog.png)

> [!IMPORTANT] 
> Включение **удаленный рабочий стол** открывает только **RDP** порт на компьютерах Linux. Вы, как преподаватель, подключитесь к компьютеру Linux с помощью SSH в первый раз и установить пакеты RDP и графического пользовательского интерфейса, таким образом, можно подключиться к машине Linux по протоколу RDP позже. Затем вы **публикации** изображение, чтобы учащиеся могут RDP в для учащихся виртуальных машин Linux. 

## <a name="supported-operating-systems"></a>Поддерживаемые операционные системы
В настоящее время удаленного рабочего стола поддерживается для следующих операционных систем:

- openSUSE Leap 42.3
- Версия 7.5 на основе CentOS
- Debian 9 "Stretch"
- Ubuntu Server 16.04 LTS

## <a name="teachers-connecting-to-the-template-vm-using-rdp"></a>Преподавателей, подключении к шаблону виртуальной Машины с помощью протокола удаленного рабочего СТОЛА
Преподаватели необходимо подключиться к шаблону виртуальной Машины сначала с помощью SSH и установить пакеты RDP и графического пользовательского интерфейса на него. Затем преподавателей можно использовать следующие действия для подключения к виртуальным машинам Linux, с помощью протокола удаленного рабочего СТОЛА: 

Вы увидите **удаленного рабочего стола** возможность подключения к шаблону виртуальной Машины во время создания лаборатории. 

![Подключение к шаблона с помощью протокола удаленного рабочего СТОЛА во время создания](../media/how-to-enable-remote-desktop-linux/connect-at-creation.png)

Вы увидите **удаленного рабочего стола** запускается параметр на домашней странице лаборатории после создания лаборатории и шаблона виртуальной Машины. Запустите шаблон виртуальной Машины, если она еще не запущена. 

![Подключение к шаблону по протоколу RDP, после создания лаборатории](../media/how-to-enable-remote-desktop-linux/rdp-after-lab-creation.png) 

Дополнительные сведения о подключении к виртуальной Машине с помощью SSH или RDP см. в разделе [Connect с помощью SSH или RDP]((#connect-using-ssh-or-rdp). 

## <a name="teachers-connecting-to-a-student-vm-using-rdp"></a>Преподавателей, подключении к учащимся виртуальной Машины с помощью протокола удаленного рабочего СТОЛА
Преподаватель/профессором могут подключаться к учащимся виртуальной Машины путем переключения на **виртуальных машин** Просмотр и выбор **подключения** значок. До того, необходимо преподавателей **публикации** образ шаблона с помощью удаленного рабочего СТОЛА и графического пользовательского интерфейса пакеты, установленные на нем. 

![Подключение к учащегося виртуальной Машины преподавателей](../media/how-to-enable-remote-desktop-linux/teacher-connect-to-student-vm.png)

Дополнительные сведения о подключении к виртуальной Машине с помощью SSH или RDP см. в разделе [Connect с помощью SSH или RDP]((#connect-using-ssh-or-rdp). 

## <a name="students-connecting-to-the-student-vm"></a>Учащиеся, подключении к учащегося виртуальной Машины
Студент может RDP в для своих виртуальных машин Linux после владелец лаборатории (учитель/Профессор) **публикует** шаблона виртуальной Машины с помощью удаленного рабочего СТОЛА и графического пользовательского интерфейса пакеты установлены на компьютере. Для этого выполните следующие действия: 

1. Когда учащийся выполняет вход на портал Labs непосредственно (`https://labs.azure.com`) или с помощью ссылка для регистрации (`https://labs.azure.com/register/<registrationCode>`), для каждой лаборатории учащийся имеет доступ к отображается элемент. 
2. На плитке, выберите **запустить** Если виртуальная машина остановлена. 
3. Нажмите кнопку **Подключиться**. Вы увидите два варианта для подключения к виртуальной Машине: **SSH** и **удаленного рабочего стола**.

    ![Виртуальная машина — параметры соединения учащегося](../media/how-to-enable-remote-desktop-linux/student-vm-connect-options.png)

## <a name="connect-using-ssh-or-rdp"></a>Подключение с использованием SSH или RDP
При выборе **SSH** параметр, вы увидите следующее **подключение к виртуальной машине** диалоговое окно:  

![Строка подключения SSH](../media/how-to-enable-remote-desktop-linux/ssh-connection-string.png)

Выберите **копирования** кнопку рядом с текстовым полем, чтобы скопировать его в буфер обмена. Сохраните строку подключения SSH. Используйте эту строку подключения из терминала SSH (например [PuTTY](https://www.putty.org/)) для подключения к виртуальной машине.

При выборе **RDP** параметр, RDP-файл загружается на вашем компьютере. Сохраните его и откройте его, чтобы подключиться к компьютеру. 

## <a name="next-steps"></a>Дальнейшие действия
Ознакомьтесь со следующими статьями:

- [Создание и администрирование учетных записей лаборатории (для администраторов)](how-to-manage-lab-accounts.md)
- [Создание и администрирование учетных записей лаборатории (для владельцев лаборатории)](how-to-manage-classroom-labs.md)
- [Настройка и публикация шаблонов (для владельцев лаборатории)](how-to-create-manage-template.md)
- [Доступ к лабораториям для аудитории (для пользователей лаборатории)](how-to-use-classroom-lab.md)

