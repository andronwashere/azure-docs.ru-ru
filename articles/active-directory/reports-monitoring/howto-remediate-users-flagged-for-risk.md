---
title: Пользователи, помеченные для риска на Azure Active Directory портале | Документация Майкрософт
description: Описание отчета системы безопасности о пользователях под угрозой на портале Azure Active Directory
services: active-directory
author: MarkusVi
manager: daveba
ms.assetid: addd60fe-d5ac-4b8b-983c-0736c80ace02
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: report-monitor
ms.date: 11/13/2018
ms.author: markvi
ms.reviewer: dhanyahk
ms.collection: M365-identity-device-management
ms.openlocfilehash: 30d02c5484ea4cce2953eac6b1b7b26a17c142bc
ms.sourcegitcommit: 9a699d7408023d3736961745c753ca3cec708f23
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/16/2019
ms.locfileid: "68276547"
---
# <a name="remediate-users-flagged-for-risk-in-the-azure-active-directory-portal"></a>Применение защитных мер к пользователям, находящимся в группе риска на портале Azure Active Directory

С помощью отчетов о безопасности в Azure Active Directory (Azure AD) можно оценить вероятность скомпрометированных учетных записей пользователей в своей среде. "Пользователь, находящийся в группе риска" означает, что безопасность учетной записи пользователя, возможно, была нарушена.

Корпорация Майкрософт стремится поддерживать безопасность вашей среды. В рамках этого обязательства мы постоянно отслеживаем действия, которые расцениваются как необычные или соответствуют известным шаблонам атак. 

При обнаружении необычных действий, которые могут указывать на несанкционированный доступ к некоторым учетным записям пользователей, вы получите уведомления, позволяющие принять соответствующие меры. Это не означает, что собственные системы Майкрософт были скомпрометированы.

## <a name="access-the-users-flagged-for-risk-report"></a>Доступ к отчету о пользователях, находящихся в группе риска

Вы можете просмотреть пользователей, находящихся в группе риска, в [соответствующем отчете](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/RiskyUsers) на портале Azure. Если у вас нет Azure AD, вы можете бесплатно зарегистрироваться на сайте [https://aka.ms/AccessAAD](https://aka.ms/AccessAAD). 

Из отчета о пользователях, находящихся в группе риска, можно выполнить следующие действия для каждого пользователя:

- Создавать временный пароль.
- Требовать от пользователя безопасного сброса пароля при следующем входе в систему.
- Отклонить риск пользователя без выполнения действий исправления.

Дополнительные сведения см. в статье [Описание отчета о пользователях под угрозой на портале Azure](concept-user-at-risk.md).

### <a name="azure-ad-subscription-for-office-365-customers"></a>Подписка Azure AD для клиентов Office 365

Можно использовать учетные данные Office 365 для доступа к **центру администрирования Azure**. После активации доступа в Azure AD вы будете перенаправлены на портал Azure AD. На уровне подписки "Базовый" ограничен объем предоставляемых в отчетах сведений. Дополнительные данные и аналитика доступны для подписчиков Azure уровня "Премиум".

Для доступа к отчетам **пользователей, помеченных для** отчетов о рисках, в Microsoft 365 центре администрирования:

1.  В меню навигации слева выберите **Центры администрирования**. 
2.  Выберите **Azure AD**.
3.  Войдите в **центр администрирования Azure Active Directory**.
4.  Если в верхней части страницы отображается баннер с запросом **ознакомиться с новым порталом**, щелкните эту ссылку.
4.  В меню навигации слева щелкните **Azure Active Directory**. 
5.  В области навигации выберите **Пользователи, находящиеся в группе риска** в разделе **Безопасность**.

## <a name="remediation-actions"></a>Действия исправления

Выполните следующие действия, чтобы устранить проблемы, связанные с затронутыми учетными записями, и защитить среду.

1.  [Проверьте правильность сведений](https://aka.ms/MFAValid) для многофакторной проверки подлинности и самостоятельного сброса пароля. 
2.  [Включите многофакторную проверку подлинности](https://aka.ms/MFAuth) для всех пользователей. 
3.  Используйте этот [сценарий исправления](https://aka.ms/remediate) для каждой затронутой учетной записи, чтобы автоматически выполнить действия ниже: 

    1\. Сбросить пароль, чтобы защитить учетную запись и прервать активные сеансы.

    2\. Удалить делегаты почтовых ящиков.

    В. Отключить правила переадресации сообщений электронной почты на внешние домены.

    Г. Удалить глобальное свойство переадресации сообщений электронной почты в почтовых ящиках.

    Д. Включить многофакторную проверку подлинности в учетной записи пользователя.

    f. Установить высокую сложность пароля в учетной записи.

    ж. Включить аудит почтовых ящиков.

    h. Создать журнал аудита, который сможет проверить администратор.

4. Проанализируйте клиент Office 365 и другую ИТ-инфраструктуру, включая проверку всех параметров клиента, учетных записей пользователей и параметров конфигурации для каждого пользователя на наличие возможных изменений. Проверьте на наличие признаков использования методов сохранения, а также признаков того, что злоумышленник мог использовать закрепление в среде, чтобы получить учетные данные VPN или доступ к другим корпоративным ресурсам. 

5.  В рамках исследования подумайте, есть ли причины уведомить государственные органы, в том числе правоохранительные.

Кроме этого, вам следует выполнить следующее.

- Ознакомьтесь с этим [руководством по мерам, предпринимаемым в случае обнаружения необычной активности](https://aka.ms/fixaccount), и выполните описанные в нем шаги. 
- [Включите конвейер аудита](https://aka.ms/improvesecurity) для анализа действий в своем клиенте. После завершения анализа журналы действий в хранилище аудита начнут заполняться соответствующими данными. На этом этапе вы также можете использовать [ресурс поиска и исследования в центре безопасности и соответствия требованиям](https://aka.ms/sccsearch). 
- Используйте этот [сценарий для включения аудита почтовых ящиков](https://aka.ms/mailboxaudit1) для всех учетных записей. 
- Просмотрите делегированные разрешения и правила переадресации сообщений для всех почтовых ящиков. Чтобы выполнить эту задачу, можно использовать этот [сценарий PowerShell](https://aka.ms/delegateforwardrules). 

## <a name="next-steps"></a>Следующие шаги

* [Защита идентификации Azure Active Directory.](../active-directory-identityprotection.md)
* [Пользователи, находящиеся в группе риска](concept-user-at-risk.md)
