---
title: включение файла
description: включение файла
services: bastion
author: cherylmc
ms.service: bastion
ms.topic: include
ms.date: 06/17/2019
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 29ab9b3c33aae6005510c34b207c7f87714149e5
ms.sourcegitcommit: ccb9a7b7da48473362266f20950af190ae88c09b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/05/2019
ms.locfileid: "67608335"
---
### <a name="preview"></a>Как получить общедоступную предварительную версию?

Вам необходимо зарегистрироваться, чтобы получить общедоступную предварительную версию. Следуйте инструкциям из [этой статьи](../articles/bastion/bastion-create-host-portal.md), чтобы создать ресурс Бастиона Azure. Пока эту службу можно использовать только с помощью [предварительной версии портала Azure](https://aka.ms/BastionHost). На обычном портале Azure она недоступна.

### <a name="regions"></a>В каких регионах доступна предварительная версия?

Вы можете развернуть и использовать ресурс Бастиона в любом из указанных ниже регионов. Для этого перейдите по [этой ссылке на предварительную версию портала Azure](https://aka.ms/BastionHost).

[!INCLUDE [region](bastion-regions-include.md)]

### <a name="portal"></a>Не удается найти ресурс Бастиона на портале Azure. Что делать?

Убедитесь, что вы используете [предварительную версию портала Azure](https://aka.ms/BastionHost), а не обычный портал Azure.

### <a name="publicip"></a>Нужен ли общедоступный IP-адрес моей виртуальной машине?

Общедоступный IP-адрес не нужен для виртуальной машины Azure, с помощью которой вы подключаетесь к Бастиону Azure. Бастион устанавливает подключение к вашей виртуальной машине по протоколу удаленного рабочего стола или SSH, используя ее частный IP-адрес в вашей виртуальной сети.

### <a name="rdpssh"></a>Нужен ли клиент RDP или SSH?

Для подключения по протоколу RDP или SSH к виртуальной машине Azure на портале Azure клиент RDP или SSH не нужен. Чтобы ознакомиться с предварительной версией службы на портале Azure, [перейдите по этой ссылке](https://aka.ms/BastionHost). Это позволит вам подключиться к своей виртуальной машине по протоколу RDP или SSH непосредственно в браузере.

### <a name="agent"></a>Нужно ли запускать агент на виртуальной машине Azure?

В браузере или на виртуальной машине Azure не нужно устанавливать агент или другое программное обеспечение. Бастион работает без агента и не требует дополнительного программного обеспечения для RDP и SSH.

### <a name="browsers"></a>Какие браузеры поддерживаются?

На этапе общедоступной предварительной версии используйте браузер Microsoft Edge или Google Chrome в ОС Windows. Для Apple Mac используйте браузер Google Chrome. ОС Windows и Mac также поддерживают Microsoft Edge Chromium.

### <a name="roles"></a>Нужны ли какие-то роли для доступа к виртуальной машине?

Для подключения нужны такие роли:

* роль читателя на виртуальной машине;
* роль читателя на сетевом адаптере с частным IP-адресом для виртуальной машины;
* роль читателя в ресурсе Бастиона Azure.

### <a name="previewbill"></a>Будет ли выставляться счет за использование этой предварительной версии?

За использование общедоступной предварительной версии счет будет выставляться в неполном объеме. Но для развертывания не действует соглашение об уровне обслуживания. Дополнительные сведения см. на [странице с расценками](https://aka.ms/BastionHostPricing).

### <a name="previewbill"></a>Почему я получаю сообщение об ошибке "ваш сеанс истек" до начала сеанса Бастиона?

Сеанс должен быть инициирован только на портале Azure. Войдите на портал Azure и заново начните сеанс. Если вы перешли по URL-адресу непосредственно из другого сеанса браузера или вкладки, эта ошибка закономерна. Она призвана обеспечивать безопасность сеанса и возможность его запуска только на портале Azure.
