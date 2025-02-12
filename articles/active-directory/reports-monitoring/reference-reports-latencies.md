---
title: Задержки в отчетах Azure Active Directory | Документация Майкрософт
description: Узнайте, сколько времени необходимо для отображения событий отчетов на портале Azure.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: ''
ms.assetid: 9b88958d-94a2-4f4b-a18c-616f0617a24e
ms.service: active-directory
ms.devlang: na
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: report-monitor
ms.date: 05/13/2019
ms.author: markvi
ms.reviewer: dhanyahk
ms.collection: M365-identity-device-management
ms.openlocfilehash: 8d4d02f9e99acb68db126ccde465f73cc2294e06
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "65594014"
---
# <a name="azure-active-directory-reporting-latencies"></a>Задержки в отчетах Azure Active Directory

Задержка — это время, затрачиваемое Azure Active Directory (Azure AD) на отображение данных отчетов на [портале Azure](https://portal.azure.com). В этой статье содержатся данные о предполагаемых задержках для различных типов отчетов. 

## <a name="activity-reports"></a>Отчеты об активности

Существуют два типа отчетов об активности:

- [Входы](concept-sign-ins.md) — информация об использовании управляемых приложений и входах пользователей.
- [Журналы аудита](concept-audit-logs.md) — системная информация о пользователях и группах, управляемых приложениях и действиях с каталогами.

В следующей таблице приведены сведения о задержках для отчетов об активности. 

> [!NOTE]
> **Задержка 95-го процентиля** — это время, в течение которого будут представлены 95 % журналов; **задержка 99-го процентиля** — это время, в течение которого будут представлены 99 % журналов. 
>

| Отчет | Задержка 95-го процентиля |Задержка 99-го процентиля|
| :-- | --- | --- |
| Журналы аудита | 2 мин  | 5 мин  |
| Вход в систему | 2 мин  | 5 мин |

### <a name="how-soon-can-i-see-activities-data-after-getting-a-premium-license"></a>Как скоро после получения лицензии уровня "Премиум" я увижу данные о действиях?

Если у вас уже есть данные о действиях с бесплатной лицензией, то вы увидите их сразу же после обновления. Если у вас нет данных, то потребуется один или два дня, чтобы эти данные отобразились в отчетах после обновления лицензии до уровня "Премиум".

## <a name="security-reports"></a>Отчеты о безопасности

Есть два типа отчетов системы безопасности:

- [Входы, представляющие риск](concept-risky-sign-ins.md). Вход, представляющий риск, означает, что в систему пытался войти пользователь, который не является законным владельцем учетной записи. 
- [Пользователи, находящиеся в группе риска](concept-user-at-risk.md). Такая пометка означает, что конфиденциальность учетной записи пользователя, возможно, нарушена. 

В следующей таблице приведены сведения о задержках для отчетов о безопасности.

| Отчет | Минимальная | Средние | Максимальная |
| :-- | --- | --- | --- |
| пользователи под угрозой;          | 5 мин   | 15 минут  | 2 ч  |
| Вход, представляющий риск         | 5 мин   | 15 минут  | 2 ч  |

## <a name="risk-events"></a>События риска

Azure AD использует адаптивные алгоритмы машинного обучения и эвристические методы, чтобы обнаруживать подозрительные действия, связанные с вашими учетными записями. Каждое обнаруженное подозрительное действие сохраняется в записи, называемой **событие риска**.

В следующей таблице приведены сведения о задержках для событий риска.

| Отчет | Минимальная | Средние | Максимальная |
| :-- | --- | --- | --- |
| Попытки входа с анонимных IP-адресов |5 мин |15 минут |2 ч |
| Попытки входа из неизвестных расположений |5 мин |15 минут |2 ч |
| Пользователи с утерянными учетными данными |2 ч |4 ч |8 ч |
| Невозможно переместиться в нетипичные расположения |5 мин |1 час |8 ч  |
| Попытки входа с инфицированных устройств |2 ч |4 ч |8 ч  |
| Попытки входа с IP-адресов с подозрительными действиями |2 ч |4 ч |8 ч  |


## <a name="next-steps"></a>Дальнейшие действия

* [Что такое отчеты в Azure Active Directory](overview-reports.md).
* [Программный доступ к отчетам Azure Active Directory](concept-reporting-api.md).
* [События риска Azure Active Directory](concept-risk-events.md)
