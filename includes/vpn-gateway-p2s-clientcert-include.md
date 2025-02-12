---
title: включение файла
description: включение файла
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 12/11/2018
ms.author: cherylmc
ms.openlocfilehash: 31ccf14c82f6248c74d6af932fe9e338d26d2747
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/18/2019
ms.locfileid: "67185149"
---
На каждом клиентском компьютере, который вы подключаете к виртуальной сети с помощью подключения типа "точка — сеть", должен быть установлен сертификат клиента. Его нужно создать из корневого сертификата и установить на каждый клиентский компьютер. Если вы не установите допустимый сертификат клиента, проверка подлинности завершится со сбоем, когда клиент попытается подключиться к виртуальной сети.

Можно создать уникальный сертификат для каждого клиента или использовать один сертификат для нескольких клиентов. Преимущество уникальных клиентских сертификатов заключается в том, что при необходимости можно отозвать один сертификат. В противном случае, если потребуется отозвать сертификат для проверки подлинности, который используют несколько клиентов, вам придется создать и установить новые сертификаты для всех клиентов, которые используют этот сертификат.

Сертификаты клиентов можно создать, используя следующие методы:

- **Корпоративный сертификат.**

  - При использовании корпоративного решения, создать сертификат клиента с общим именем в формате *имя\@yourdomain.com*. Используйте этот формат вместо формата *доменное_имя\имя_пользователя*.
  - Убедитесь, что сертификат клиента основан на шаблоне сертификата пользователя, в котором первым указан пункт *Проверка подлинности клиента*. Проверить сертификат можно, дважды щелкнув его и выбрав на вкладке **Сведения** **Улучшенный ключ**.

- **Самозаверяющийся корневой сертификат**. Выполните действия, описанные в следующих статьях о сертификате P2S, чтобы создаваемые сертификаты клиента были совместимы с подключениями P2S. Действия, описанные в следующих статьях, помогут создать совместимый сертификат клиента: 

  * [Инструкции по использованию PowerShell в Windows 10](../articles/vpn-gateway/vpn-gateway-certificates-point-to-site.md#clientcert). Для создания сертификатов с помощью этих инструкций требуется Windows 10 и PowerShell. Созданные сертификаты можно установить на любой поддерживаемый клиент P2S.
  * [Инструкции по MakeCert](../articles/vpn-gateway/vpn-gateway-certificates-point-to-site-makecert.md). Если у вас нет компьютера с Windows 10, создайте сертификаты с помощью MakeCert. Хотя это нерекомендуемое средство, его все же можно использовать для создания сертификатов. Созданные сертификаты можно установить на любом поддерживаемом клиенте P2S.
  * [Инструкции для Linux](../articles/vpn-gateway/vpn-gateway-certificates-point-to-site-linux.md)

  Если вы создаете сертификат клиента из самозаверяющего корневого сертификата, он автоматически устанавливается на компьютере, используемом для его создания. Если вы хотите установить сертификат клиента на другом клиентском компьютере, экспортируйте его как PFX-файл вместе со всей цепочкой сертификатов. После экспорта будет создан PFX-файл, содержащий сведения корневого сертификата, необходимые для проверки подлинности клиента. 

**Экспорт сертификата**

Сведения об экспорте сертификатов см. в [этом разделе](../articles/vpn-gateway/vpn-gateway-certificates-point-to-site.md#clientexport).
