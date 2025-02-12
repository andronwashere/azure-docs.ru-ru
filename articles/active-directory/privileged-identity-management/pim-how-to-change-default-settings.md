---
title: Настройка параметров Azure AD роли в PIM — Azure Active Directory | Документация Майкрософт
description: Узнайте, как настроить параметры роли Azure AD в Azure AD Privileged Identity Management (PIM).
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.subservice: pim
ms.date: 05/31/2019
ms.author: rolyon
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: 2bc7d3ffcb56251825bf5f6d760de647938f1ead
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "66417871"
---
# <a name="configure-azure-ad-role-settings-in-pim"></a>Настройка параметров Azure AD роли в PIM

Администратор привилегированных ролей можно настроить Azure Active Directory (Azure AD) Privileged Identity Management (PIM) в своей организации, включая изменение взаимодействие для пользователей, активирующих назначения временных ролей.

## <a name="open-role-settings"></a>Открытие параметров роли

Выполните следующие действия, чтобы открыть параметры для роли Azure AD.

1. Откройте страницу **Azure AD Privileged Identity Management**.

1. Выберите **Роли Azure AD**.

1. Щелкните **Параметры**.

    ![Роли Azure AD — параметры](./media/pim-how-to-change-default-settings/pim-directory-roles-settings.png)

1. Щелкните **Роли**.

1. Выберите роль, параметры которой нужно настроить.

    ![Azure AD роли - параметры](./media/pim-how-to-change-default-settings/pim-directory-roles-settings-role.png)

    На странице параметров для каждой роли есть ряд параметров, которые можно настроить. Эти параметры распространяются только на пользователей, которым доступны **соответствующие** **временные** назначения.

## <a name="activations"></a>Активации

С помощью ползунка **Активации** задайте максимальное время в часах, в течение которого роль остается активной. Возможное значение: от 1 до 72 часов.

## <a name="notifications"></a>Уведомления

Используйте переключатель режима **Уведомления**, чтобы указать, будут ли администраторы получать уведомления по электронной почте при активации ролей. Этот параметр полезен, если требуется обнаружить несанкционированные или недопустимые активации.

Если режим **включен**, уведомления будут отправляться таким администраторам:

- Администратор привилегированных ролей
- Администратор безопасности
- глобального администратора;

Дополнительные сведения см. в статье [Уведомления по электронной почте в PIM](pim-email-notifications.md).

## <a name="incidentrequest-ticket"></a>Обращение с запросом или инцидентом

С помощью переключателя **Обращение с запросом или инцидентом** позволяет выбрать, требовать ли от временных администраторов указывать номер обращения при активации своей роли. Этот параметр полезен при выполнении аудита доступа к ролям.

## <a name="multi-factor-authentication"></a>Многофакторная идентификация

С помощью переключателя **Многофакторная идентификация** можно выбрать, следует ли требовать от пользователей перед активацией ролей проходить аутентификацию с использованием Многофакторной идентификации. Такая проверка будет выполняться один раз за сеанс, а не при каждой активации роли. Есть две подсказки, которые рекомендуется учитывать при включении проверки MFA:

* Пользователи, использующие для электронной почты учетные записи Майкрософт (как правило, @outlook.com, но не обязательно), не могут зарегистрироваться в службе "Многофакторная идентификация Azure" (MFA) . Если вы хотите назначить роли пользователям с учетными записями Майкрософт, то следует сделать их постоянными администраторами или отключить проверку MFA для этих ролей.
* Нельзя отключить MFA для ролей с высоким уровнем привилегий в Azure AD и Office 365. С помощью этой функции безопасности должны быть надежно защищены следующие роли:  
  
  * Администратор Azure Information Protection
  * администратора выставления счетов;
  * Администратор облачных приложений
  * Администратор соответствия требованиям
  * Администратор условного доступа
  * Администратор службы CRM
  * Лицо, утверждающее доступ клиентов к LockBox
  * Создатели каталогов
  * Администратор Exchange
  * глобального администратора;
  * Администратор службы Intune
  * Администратор службы Power BI
  * Администратор привилегированных ролей
  * Администратор безопасности
  * Администратор службы SharePoint
  * Администратор Skype для бизнеса
  * администратора пользователей;

Дополнительные сведения см. в статье [Настройка требования MFA для ролей ресурсов Azure AD в компоненте управления привилегированными пользователями](pim-how-to-require-mfa.md).

## <a name="require-approval"></a>Требуется утверждение

Если необходимо требовать утверждения для активации роли, выполните следующие действия.

1. Установите для переключателя **Требуется утверждение** значение **Включено**. Область развернется, отобразив параметры для выбора утверждающих лиц.

    ![Роли Azure AD — параметры — требовать утверждения](./media/pim-how-to-change-default-settings/pim-directory-roles-settings-require-approval.png)

    Если **НЕ** указать ни одного утверждающего лица, то таковыми по умолчанию станут администраторы привилегированных ролей. Администраторы привилегированных ролей должны будут утверждать **ВСЕ** запросы на активацию этой роли.

1. Чтобы указать утверждающих лиц, щелкните **Выбор утверждающих лиц**.

    ![Роли Azure AD — параметры — требовать утверждения](./media/pim-how-to-change-default-settings/pim-directory-roles-settings-require-approval-select-approvers.png)

1. Выберите одного или нескольких утверждающих лиц, а затем нажмите кнопку **Выбрать**. Можно выбрать пользователей или группы. Рекомендуется выбрать не менее 2-х утверждающих лиц. Самостоятельное утверждение запрещено.

    Выбранные вами элементы отобразятся в списке выбранных утверждающих лиц.

1. После указания всех параметров роли нажмите кнопку **Сохранить**, чтобы сохранить изменения.


<!--PLACEHOLDER: Need an explanation of what the temporary Global Administrator setting is for.-->

## <a name="next-steps"></a>Дальнейшие действия

- [Назначение ролей Azure AD в PIM](pim-how-to-add-role-to-user.md)
- [Настройка оповещений безопасности для ролей Azure AD в PIM](pim-how-to-configure-security-alerts.md)
