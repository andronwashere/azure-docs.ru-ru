---
title: включение файла
description: включение файла
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 04/04/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 19ad4e39ca4e402c37b2cfa69c7c306b6e5a2766
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/18/2019
ms.locfileid: "67185140"
---
1. Найдите и откройте страницу шлюза виртуальной сети. Это можно сделать несколькими способами. Вы можете перейти к шлюзу VNet1GW, последовательно открыв **TestVNet1 -> Обзор -> Подключенные устройства -> VNet1GW**.
2. На странице VNet1GW щелкните **Подключения**. В верхней части страницы "Подключения" нажмите кнопку **Добавить**, чтобы открыть страницу **Добавление подключения**.

   ![Создание подключения типа "сеть — сеть"](./media/vpn-gateway-add-site-to-site-connection-portal-include/configure-site-to-site-connection.png)
3. На странице **Добавление подключения** задайте значения для подключения.

   - **Имя**. Имя вашего подключения.
   - **Тип подключения**. Выберите **Сеть — сеть (IPSec)** .
   - **Шлюз виртуальной сети.** Это значение является фиксированным, так как вы подключаетесь из этого шлюза.
   - **Шлюз локальной сети.** Щелкните **Выбрать шлюз локальной сети** и укажите нужный шлюз.
   - **Общий ключ** — это значение должно соответствовать ключу локального VPN-устройства. В этом примере используется abc123, но вы можете (это рекомендуется) создать и использовать что-нибудь посложнее. Важно, чтобы заданное здесь значение совпадало со значением, указываемым при настройке вашего VPN-устройства.
   - Остальные значения для **подписки**, **группы ресурсов**, и **расположения** являются фиксированными.

4. Щелкните **ОК** для создания подключения. На экране появится надпись *Идет создание подключения* .
5. Данные созданного подключения появятся на странице **Подключения** шлюза виртуальной сети. Состояние изменится с *Неизвестно* на *Подключение*, а затем — *Успешно*.
