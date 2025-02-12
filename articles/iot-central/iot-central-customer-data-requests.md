---
title: Возможности запроса данных клиента в Azure IoT Central | Документация Майкрософт
description: Возможности запроса данных клиента в Azure IoT Central
author: dominicbetts
ms.author: dobett
ms.date: 04/24/2019
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: timlt
ms.openlocfilehash: 4c63ac8d6b16b2a0476600f67c66b2ccc8dc2aab
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "64685558"
---
# <a name="summary-of-customer-data-request-features"></a>Сводка о возможностях запроса данных клиента

Azure IoT Central — это полностью управляемое решение Интернета вещей (IoT) (модели "программное обеспечение как услуга"), которое облегчает подключение и мониторинг ресурсов Интернета вещей, а также управление ими в любом масштабе. С помощью этого решения можно также получать аналитические сведения из данных Интернета вещей и предпринимать обоснованные действия.

[!INCLUDE [gdpr-intro-sentence](../../includes/gdpr-intro-sentence.md)]

## <a name="identifying-customer-data"></a>Идентификация данных клиента

Идентификаторы объектов Azure Active Directory используются для идентификации пользователей и назначения ролей. На портале Azure IoT Central отображаются адреса электронной почты пользователей для назначений ролей. Однако сохраняется только идентификатор объекта Azure Active Directory, адрес электронной почты динамически запрашивается из Azure Active Directory. Администраторы Azure IoT Central могут просматривать, экспортировать, а также удалять пользователей приложения в разделе администрирования пользователей в приложении.

В приложении можно настроить адреса электронной почты, на которые будут отправляться оповещения. В этом случае адреса электронной почты хранятся в IoT Central и управление ими должно осуществляться на странице администрирования учетных записей в приложении.

Что касается устройств, корпорация Майкрософт не сохраняет сведения и не имеет доступа к данным, обеспечивающим связь пользователя и устройства. Многие устройства, управляемые в Azure IoT Central, не являются персональными (например, торговый автомат или кофеварка). Однако клиенты могут считать некоторые устройства такими, которые содержат персональные данные, и по своему усмотрению использовать собственные средства или системы отслеживания инвентаризации, связывающие устройства с частными лицами. Azure IoT Central хранит все данные, связанные с устройствами, и управляет ими как персональными данными.

При использовании корпоративных служб корпорация Майкрософт создает некоторые данные, известные как создаваемые системой журналы. Эти журналы содержат сведения о фактических действиях, выполненных в службе, и диагностические данные, связанные с отдельными устройствами и не связанные с действиями пользователей. Создаваемые системой журналы Azure IoT Central недоступны и не подлежат экспорту администраторами приложения.

## <a name="deleting-customer-data"></a>Удаление данных клиента

Удалять данные пользователя можно только на странице администрирования IoT Central. Администраторы приложений можно выбрать пользователя, который необходимо удалить, а затем выберите **удалить** в правом верхнем углу приложения для удаления записи. Они также могут удалять отдельные учетные записи, которые больше не связаны с конкретным приложением.

После удаления пользователя ему не будут отправляться последующие оповещения по электронной почте. Тем не менее адрес электронной почты необходимо удалить по отдельности из каждого настроенного оповещения.

Дополнительные сведения см. в статье о [настройке правил и действий для устройства](tutorial-configure-rules.md).

## <a name="exporting-customer-data"></a>Экспорт данных клиента

Экспортировать данные можно только на странице администрирования IoT Central. Администратор приложения может выбрать, скопировать и вставить данные клиента, включая назначенные роли.

## <a name="links-to-additional-documentation"></a>Ссылки на дополнительную документацию

Дополнительные сведения об администрировании учетной записи, включая определения ролей, см. в статье [Администрирование приложения](howto-administer.md).
