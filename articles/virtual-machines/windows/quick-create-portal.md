---
title: Краткое руководство. Создание виртуальной машины Windows с помощью портала Azure | Документация Майкрософт
description: Из этого краткого руководства вы узнаете, как с помощью портала Azure создать виртуальную машину Windows.
services: virtual-machines-windows
documentationcenter: virtual-machines
author: cynthn
manager: gwallace
editor: tysonn
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 07/02/2019
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: fed118ee78e6a11f3492060a55ac9847bb5b6a88
ms.sourcegitcommit: dad277fbcfe0ed532b555298c9d6bc01fcaa94e2
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/10/2019
ms.locfileid: "67720044"
---
# <a name="quickstart-create-a-windows-virtual-machine-in-the-azure-portal"></a>Краткое руководство. Создание виртуальной машины Windows на портале Azure

Виртуальные машины Azure можно создать на портале Azure. В этом случае для создания виртуальных машин и всех связанных ресурсов используется пользовательский интерфейс в браузере. В этом кратком руководстве показано, как с помощью портала Azure развернуть в Azure виртуальную машину Windows под управлением Windows Server 2016. Чтобы проверить работу виртуальной машины, вы подключитесь к ней по протоколу удаленного рабочего стола (RDP) и установите веб-сервер IIS.

Если у вас еще нет подписки Azure, [создайте бесплатную учетную запись Azure](https://azure.microsoft.com/free/?WT.mc_id=A261C142F), прежде чем начинать работу.

## <a name="sign-in-to-azure"></a>Вход в Azure

Войдите на портал Azure по адресу https://portal.azure.com.

## <a name="create-virtual-machine"></a>Создание виртуальной машины

1. Выберите **Создать ресурс** в верхнем левом углу окна портала Azure.

1. На странице **Создать** в разделе **Популярные**выберите **Windows Server 2016 Datacenter**.

1. На вкладке **Основные сведения** в разделе **Сведения о проекте** убедитесь, что выбрана правильная подписка, и при необходимости щелкните **Создать** для группы ресурсов. Введите *myResourceGroup* в качестве имени. 

    ![Создание группы ресурсов для виртуальной машины](./media/quick-create-portal/project-details.png)

1. В разделе **Подробности об экземпляре** введите *myVM* в поле **Имя виртуальной машины** и выберите *Восточная часть США* в поле **Расположение**. Оставьте другие значения по умолчанию.

    ![Раздел "Подробности об экземпляре"](./media/quick-create-portal/instance-details.png)

1. В разделе **Учетная запись администратора** укажите имя пользователя, например *azureuser*, и пароль. Пароль должен включать минимум 12 символов и соответствовать [определенным требованиям к сложности](faq.md#what-are-the-password-requirements-when-creating-a-vm).

    ![Ввод имени пользователя и пароля](./media/quick-create-portal/administrator-account.png)

1. В разделе **Правила входящего порта**, щелкните **Разрешить выбранные порты**, а затем выберите **RDP (3389)** и **HTTP** из раскрывающегося списка.

    ![Открытие портов для RDP и HTTP](./media/quick-create-portal/inbound-port-rules.png)

1. Оставьте остальные значения по умолчанию и нажмите кнопку **Просмотр и создание**, расположенную в нижней части страницы.

    ![Просмотр и создание](./media/quick-create-portal/review-create.png)


## <a name="connect-to-virtual-machine"></a>Подключение к виртуальной машине

Создайте подключение удаленного рабочего стола к виртуальной машине. Ниже представлены инструкции для подключения к виртуальной машине с компьютера Windows. На компьютере Mac вам понадобится клиент RDP, например [Remote Desktop Client](https://itunes.apple.com/us/app/microsoft-remote-desktop/id715768417?mt=12) из Mac App Store.

1. Нажмите кнопку **Подключиться** на странице свойств виртуальной машины. 

    ![Подключение к виртуальной машине Azure с портала](./media/quick-create-portal/portal-quick-start-9.png)
    
2. На странице **подключения к виртуальной машине** сохраните значения по умолчанию, чтобы использовать подключение по DNS-имени через порт 3389, и нажмите кнопку **Скачать RDP-файл**.

2. Откройте скачанный RDP-файл и при появлении запроса нажмите кнопку **Подключиться**. 

3. В окне **Безопасность Windows** выберите **Варианты выбора** и нажмите **Использовать другую учетную запись**. Введите имя пользователя в формате **localhost**\\*имя_пользователя*, а затем введите созданный для этой виртуальной машины пароль и нажмите кнопку **ОК**.

4. При входе в систему может появиться предупреждение о сертификате. Щелкните **Да** или **Продолжить**, чтобы создать подключение.

## <a name="install-web-server"></a>Установка веб-сервера

Чтобы проверить работу виртуальной машины, установите веб-сервер IIS. На виртуальной машине откройте командную строку PowerShell и выполните следующую команду:

```powershell
Install-WindowsFeature -name Web-Server -IncludeManagementTools
```

После этого закройте RDP-подключение к виртуальной машине.


## <a name="view-the-iis-welcome-page"></a>Просмотр страницы приветствия IIS

Выберите на портале виртуальную машину. В окне обзора скопируйте IP-адрес с помощью находящейся справа от него кнопки **Щелкните, чтобы скопировать**, а затем вставьте его на вкладку браузера. Откроется страница приветствия IIS по умолчанию, которая будет иметь следующий вид:

![Сайт IIS по умолчанию](./media/quick-create-powershell/default-iis-website.png)

## <a name="clean-up-resources"></a>Очистка ресурсов

Если группа ресурсов, виртуальная машина и все связанные с ними ресурсы вам больше не требуются, их можно удалить. Выберите группу ресурсов для виртуальной машины и щелкните **Удалить**. Подтвердите имя группы ресурсов, чтобы завершить удаление ресурсов.

## <a name="next-steps"></a>Дополнительная информация

При работе с этим кратким руководством вы развернули простую виртуальную машину, открыли сетевой порт для веб-трафика и установили базовый веб-сервер. Дополнительные сведения о виртуальных машинах Azure см. в руководстве по работе с виртуальными машинами Windows.

> [!div class="nextstepaction"]
> [Создание виртуальных машин Windows и управление ими с помощью модуля Azure PowerShell](./tutorial-manage-vm.md)
