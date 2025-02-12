---
title: Настройка частных IP-адресов для (классических) виртуальных машин с помощью портала Azure | Документация Майкрософт
description: Узнайте, как настроить частные IP-адреса для (классических) виртуальных машин с помощью портала Azure.
services: virtual-network
documentationcenter: na
author: genlin
manager: cshepard
editor: tysonn
tags: azure-service-management
ms.assetid: b8ef8367-58b2-42df-9f26-3269980950b8
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/04/2016
ms.author: genli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 72d1c4d2ea3adf7d8751adfbb013435f8f2530f0
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "62125752"
---
# <a name="configure-private-ip-addresses-for-a-virtual-machine-classic-using-the-azure-portal"></a>Настройка частных IP-адресов для (классической) виртуальной машины с помощью портала Azure

[!INCLUDE [virtual-networks-static-private-ip-selectors-classic-include](../../includes/virtual-networks-static-private-ip-selectors-classic-include.md)]

[!INCLUDE [virtual-networks-static-private-ip-intro-include](../../includes/virtual-networks-static-private-ip-intro-include.md)]

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]

В этой статье рассматривается классическая модель развертывания. Кроме того, вы можете [управлять статическим частным IP-адресом в модели развертывания для диспетчера ресурсов](virtual-networks-static-private-ip-arm-pportal.md).

[!INCLUDE [virtual-networks-static-ip-scenario-include](../../includes/virtual-networks-static-ip-scenario-include.md)]

Предполагается, что для выполнения приведенных ниже примеров была создана простая среда. Чтобы выполнять действия в соответствии с указаниями, представленными в этом документе, сначала постройте тестовую среду, как описано в статье [Создание виртуальной сети](virtual-networks-create-vnet-classic-pportal.md).

## <a name="how-to-specify-a-static-private-ip-address-when-creating-a-vm"></a>Указание статического частного IP-адреса при создании виртуальной машины
Чтобы создать виртуальную машину с именем *DNS01* в подсети *FrontEnd* виртуальной сети *TestVNet* со статическим частным IP-адресом *192.168.1.101*, сделайте следующее:

1. В браузере перейдите по адресу https://portal.azure.com и при необходимости выполните вход с помощью учетной записи Azure.
2. Выберите **Создать** > **Вычисление** > **Windows Server 2012 R2 Datacenter**. Обратите внимание на то, что в списке **Выберите модель развертывания** уже указан параметр **Классический**, и нажмите кнопку **Создать**.
   
    ![Создание виртуальной машины на портале Azure](./media/virtual-networks-static-ip-classic-pportal/figure01.png)
3. В колонке **Создать виртуальную машину** введите имя создаваемой виртуальной машины (в текущем сценарии это *DNS01*), учетную запись локального администратора и пароль.
   
    ![Создание виртуальной машины на портале Azure](./media/virtual-networks-static-ip-classic-pportal/figure02.png)
4. Выберите **Необязательная конфигурация** > **Сеть** > **Виртуальная сеть**, а затем — **TestVNet**. Если сеть **TestVNet** недоступна, убедитесь, что вы используете расположение *Центральная часть США* и создали тестовую среду, описанную в начале этой статьи.
   
    ![Создание виртуальной машины на портале Azure](./media/virtual-networks-static-ip-classic-pportal/figure03.png)
5. В колонке **Сеть** выберите подсеть *FrontEnd* и щелкните **IP-адреса**. В разделе **Назначение IP-адресов** выберите **Статический** и введите *192.168.1.101* в поле **IP-адрес**, как показано ниже.
   
    ![Создание виртуальной машины на портале Azure](./media/virtual-networks-static-ip-classic-pportal/figure04.png)    
6. Нажмите кнопку **ОК** в колонке **IP-адреса**, кнопку **ОК** в колонке **Сеть** и кнопку **ОК** в колонке **Необязательная конфигурация**.
7. В колонке **Создать виртуальную машину** нажмите кнопку **Создать**. Ниже обратите внимание на плитку, отображенную на панели мониторинга.
   
    ![Создание виртуальной машины на портале Azure](./media/virtual-networks-static-ip-classic-pportal/figure05.png)

## <a name="how-to-retrieve-static-private-ip-address-information-for-a-vm"></a>Как получить информацию о статическом частном IP-адресе виртуальной машины
Чтобы просмотреть информацию о статическом частном IP-адресе виртуальной машины, созданной с помощью описанные выше действий, выполните следующее.

1. На портале Azure выберите **Просмотреть все** > **Виртуальные машины (классические)**  > **DNS01** > **Все параметры** > **IP-адреса** и обратите внимание на назначение IP-адресов и сам IP-адрес.
   
    ![Создание виртуальной машины на портале Azure](./media/virtual-networks-static-ip-classic-pportal/figure06.png)

## <a name="how-to-remove-a-static-private-ip-address-from-a-vm"></a>Удаление статического частного IP-адреса виртуальной машины

В колонке **IP-адреса** выберите **Динамический** справа от элемента **Назначение IP-адресов** и нажмите кнопку **Сохранить** и **Да**, как показано на следующем рисунке.
   
    ![Create VM in Azure portal](./media/virtual-networks-static-ip-classic-pportal/figure07.png)

## <a name="how-to-add-a-static-private-ip-address-to-an-existing-vm"></a>Как добавить статический частный IP-адрес для существующей виртуальной машины

1. В предыдущей колонке **IP-адреса** справа от элемента **Назначение IP-адресов** выберите **Статический**.
2. Введите *192.168.1.101* в поле **IP-адрес**, нажмите кнопку **Сохранить**, а затем — **Да**.

## <a name="set-ip-addresses-within-the-operating-system"></a>Настройка IP-адресов в операционной системе

Не рекомендуется без необходимости статически назначать виртуальной машине Azure частный IP-адрес в ее операционной системе. Если вы будете вручную устанавливать частный IP-адрес в операционной системе, убедитесь, что он соответствует частному IP-адресу, назначенному виртуальной машине Azure, иначе соединение с виртуальной машиной может быть потеряно. Никогда не следует вручную назначать общедоступный IP-адрес для виртуальной машины Azure в ее операционной системе.

## <a name="next-steps"></a>Дальнейшие действия
* Ознакомьтесь с информацией о [зарезервированных общедоступных IP-адресах](virtual-networks-reserved-public-ip.md) .
* Узнайте об [общедоступных IP-адресах уровня экземпляра (ILPIP)](virtual-networks-instance-level-public-ip.md) .
* Ознакомьтесь с информацией о [REST API зарезервированных IP-адресов](https://msdn.microsoft.com/library/azure/dn722420.aspx).

