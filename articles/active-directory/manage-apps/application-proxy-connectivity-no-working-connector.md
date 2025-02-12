---
title: Не удается найти рабочую группу соединителей для приложения прокси приложения | Документы Майкрософт
description: В этой статье описаны проблемы, которые могут возникнуть при отсутствии рабочего соединителя или группы соединителей для приложения прокси приложения.
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.assetid: ''
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/21/2018
ms.author: mimart
ms.reviewer: asteen
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4f829b8e8a4bc08b43d3c30a6333771ccd4e26e8
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "65783612"
---
# <a name="no-working-connector-group-found-for-an-application-proxy-application"></a>Не удается найти рабочую группу соединителей для приложения прокси приложения

Эта статья помогает устранить общие проблемы, которые появляются, когда не обнаружен соединитель для приложения Application Proxy, интегрированный с Azure Active Directory.

## <a name="overview-of-steps"></a>Обзор действий
Если для вашего приложения отсутствуют рабочий соединитель или группа соединителей, решить эту проблему можно несколькими способами:

-   При отсутствии соединителей в группе можно выполнить следующие действия:

    -   Скачайте новый соединитель справа на локальный сервер и назначьте его этой группе

    -   Переместите активный соединитель в группу

-   При отсутствии активных соединителей в группе можно выполнить следующие действия:

    -   Определите причину, по которой соединитель не активен, и устраните ее

    -   Переместите активный соединитель в группу

Чтобы узнать, какая из приведенных проблем возникла в вашем случае, откройте меню "Прокси приложения" в приложении и просмотрите предупреждение, связанное с группой соединителей. Если в группе нет соединителей, предупреждающее сообщение указывает на то, что данная группа должна иметь, по крайней мере, один соединитель. Если у вас нет активных соединителей, предупреждающее сообщение объясняет это. Зачастую используются неактивные соединители. 

   ![Выбор группы соединителей на портале Azure](./media/application-proxy-connectivity-no-working-connector/no-active-connector.png)

Сведения о каждом из этих вариантов см. в соответствующем разделе ниже. В инструкциях предполагается, что вы начинаете со страницы управления коннектором. Если у вас есть сообщение об ошибке, то можно щелкнуть по нему, чтобы перейти на эту страницу. Вы также можете перейти на страницу, выбрав **Azure Active Directory**, нажав **Enterprise Applications**, затем **Application Proxy.**

   ![Управление группой соединителей на портале Azure](./media/application-proxy-connectivity-no-working-connector/app-proxy.png)

## <a name="download-a-new-connector"></a>Скачайте новый соединитель

Чтобы скачать новый соединитель, нажмите кнопку "Скачать соединитель" в верхней части страницы.

Установите соединитель на компьютере, находящемся в пределах "прямой видимости" для серверного приложения. Обычно соединитель устанавливается на том же сервере, что и приложение. После загрузки соединитель должен появиться в этом меню. Щелкните соединитель и укажите соответствующую группу соединителя в раскрывающемся списке "Группа соединителя". Сохраните изменения.

   ![Скачайте соединитель с портала Azure](./media/application-proxy-connectivity-no-working-connector/download-connector.png)
   
## <a name="move-an-active-connector"></a>Переместите активный соединитель

Если у вас есть активный соединитель, который должен принадлежать к группе и находится в прямой видимости для серверного приложения, можно перенести соединитель в назначенную группу. Для этого щелкните соединитель. В раскрывающемся списке "Группа соединителя" выберите соответствующую группу и нажмите кнопку "Сохранить".

## <a name="resolve-an-inactive-connector"></a>Решите проблему с неактивным соединителем

Если только соединители группы неактивны, скорее всего на компьютере, на котором они находятся, открыты не все необходимые порты.

Дополнительные сведения о решении этой проблемы см. в документе "Решение проблем с портами".

## <a name="next-steps"></a>Дальнейшие действия
[Сведения о соединителях прокси приложения Azure AD](application-proxy-connectors.md)


