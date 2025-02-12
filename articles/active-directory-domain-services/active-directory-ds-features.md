---
title: Доменные службы Azure Active Directory. Функции | Документация Майкрософт
description: Функции доменных служб Azure Active Directory
services: active-directory-ds
documentationcenter: ''
author: iainfoulds
manager: daveba
editor: curtand
ms.assetid: 8d1c3eb3-1022-4add-a919-c98cc6584af1
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/10/2019
ms.author: iainfou
ms.openlocfilehash: e0f38ed8367f076c9f9c4da77440bb3e9603d514
ms.sourcegitcommit: b2db98f55785ff920140f117bfc01f1177c7f7e2
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/16/2019
ms.locfileid: "68234214"
---
# <a name="features-provided-by-azure-active-directory-domain-services"></a>Функции, предоставляемые доменными службами Azure Active Directory

В управляемых доменах доменных служб Azure AD доступны следующие функции.

* **Простое развертывание**. Доменные службы Azure AD для каталога Azure AD можно включить всего несколькими щелчками мыши. Управляемый домен включает в себя только облачные учетные записи пользователей и учетные записи пользователей, синхронизированные из локального каталога.
* **Поддержка присоединения к домену**. Вы можете легко присоединить к домену компьютеры в виртуальной сети Azure, в которой доступен ваш управляемый домен. Присоединение к домену для клиента Windows операционных систем сервера происходит незаметно для доменов, поддерживаемых доменными службами Azure AD. Также можно использовать средства автоматического присоединения к домену для таких доменов.
* **Один экземпляр домена для каждого каталога Azure AD**. Вы можете создать один домен Active Directory для каждого каталога Azure AD.
* **Создание доменов с пользовательскими именами**. С помощью доменных служб Azure AD вы можете создавать домены с пользовательскими именами (например, contoso100.com). Можно использовать проверенные и непроверенные доменные имена. Кроме того, можно создать домен с суффиксом домена (т. е. *.onmicrosoft.com), предоставляемым каталогом Azure AD.
* **Интеграция с Azure AD**. Для доменных служб Azure AD не нужно настраивать репликацию или управлять ею. Учетные записи пользователей, членство в группах и учетные данные пользователя (пароли) из вашего каталога Azure AD автоматически доступны в доменных службах Azure AD. Новые пользователи, группы или изменения атрибутов в клиенте Azure AD или в локальном каталоге автоматически синхронизируются с доменными службами Azure AD.
* **Аутентификация NTLM и Kerberos**. С поддержкой аутентификации NTLM и Kerberos можно развернуть приложения, использующие встроенную аутентификацию Windows.
* **Использование корпоративных учетных данных и паролей**. Пароли для пользователей клиента Azure AD подходят для доменных служб Azure AD. Пользователи могут использовать учетные данные организации для приподключения к домену компьютеров, входа в интерактивном или удаленном рабочем столе и проверки подлинности в управляемом домене.
* **Поддержка привязки LDAP и чтения LDAP**. Для аутентификации пользователей в доменах, поддерживаемых доменными службами Azure AD, можно использовать приложения, использующие привязки LDAP. Кроме того, приложения, использующие операции чтения LDAP для запроса атрибутов пользователя или компьютера из каталога, также смогут работать с доменными службами Azure AD.
* **Защищенный протокол LDAP (LDAPS)** . Вы можете включить доступ к каталогу через протокол LDAP (LDAPS). Защищенный доступ LDAP доступен в виртуальной сети по умолчанию. Тем не менее можно дополнительно включить безопасный LDAP для доступа через Интернет.
* **Групповая политика:** Вы можете использовать один встроенный объект групповой политики (GPO) для каждого контейнера пользователя или компьютера, чтобы обеспечить соответствие учетных записей пользователей, а также компьютеров, присоединенных к домену, обязательным политикам безопасности. Кроме того, можно создать пользовательский объект групповой политики и назначить его пользовательским подразделениям для [управления групповой политикой](manage-group-policy.md).
* **Управление DNS**. Участники группы администраторов контроллера домена AAD могут управлять DNS для управляемого домена с помощью таких привычных инструментов администрирования DNS, как оснастка MMC для администрирования DNS.
* **Создание пользовательских подразделений (OU)** . Участники группы "Администраторы контроллера домена AAD" могут создавать пользовательские подразделения в управляемом домене. Этим пользователям предоставляется полный набор административных привилегий для пользовательских подразделений, то есть они могут добавлять или удалять учетные записи служб, компьютеры, группы и т. д. в данных пользовательских подразделениях.
* **Доступность во многих глобальных регионах Azure**. На странице [служб Azure по регионам](https://azure.microsoft.com/regions/#services/) можно узнать, в каких регионах Azure доступны доменные службы Azure AD.
* **Высокий уровень доступности**. Доменные службы AD Azure обеспечивают высокий уровень доступности вашего домена. Эта функция обеспечивает большее время непрерывной работы службы и отказоустойчивость. Встроенный мониторинг предоставляет средства автоматического восстановления после сбоев путем развертывания новых экземпляров для замены неисправных экземпляров и для обеспечения непрерывной работы домена.
* **Защита блокировкой учетной записи AD**. Учетные записи пользователей блокируются на 30 минут, если в течение 2 минут используются пять недействительных паролей. Учетные записи автоматически разблокируются через 30 минут.
* **Использование привычных средств управления**. Вы можете использовать привычные средства управления Windows Server Active Directory, такие как центр администрирования Active Directory или Active Directory PowerShell, для администрирования управляемых доменов.
