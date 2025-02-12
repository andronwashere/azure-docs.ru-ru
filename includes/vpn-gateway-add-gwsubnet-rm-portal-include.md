---
title: включение файла
description: включение файла
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 11/30/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 42e983ead6f7562c6a31cf9ef4ad2d97d0ff9707
ms.sourcegitcommit: 2e4b99023ecaf2ea3d6d3604da068d04682a8c2d
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/09/2019
ms.locfileid: "67673500"
---
1. На [портале Azure](https://portal.azure.com) выберите виртуальную сеть Resource Manager, для которой необходимо создать шлюз.

2. На странице виртуальной сети в разделе **Параметры** щелкните **Подсети**, чтобы открыть страницу **Подсети**.

3. На странице **Подсети** щелкните **Подсеть шлюза**. Откроется страница **Добавление подсети**.

   ![Добавить подсеть шлюза](./media/vpn-gateway-add-gwsubnet-rm-portal-include/addgwsub.png "Add the gateway subnet")

4. В поле **Имя** автоматически добавляется значение *GatewaySubnet*. По этому имени Azure идентифицирует подсеть как подсеть шлюза. Настройте автоматическое заполнение значений **диапазона адресов** для соответствия требованиям конфигурации, а затем щелкните **OK**, чтобы создать подсеть.

   ![Добавление подсети](./media/vpn-gateway-add-gwsubnet-rm-portal-include/addsubnetgw.png "Adding the subnet")
