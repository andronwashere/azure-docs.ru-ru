---
title: Отключение входа пользователя в корпоративное приложение в Azure Active Directory | Документы Майкрософт
description: Узнайте, как в Azure Active Directory можно отключить корпоративное приложение, чтобы пользователи не могли войти в него.
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 04/12/2019
ms.author: mimart
ms.reviewer: asteen
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 7256791c0b6bfbc72a26f6093cdd3c39410f702f
ms.sourcegitcommit: 47ce9ac1eb1561810b8e4242c45127f7b4a4aa1a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/11/2019
ms.locfileid: "67807604"
---
# <a name="disable-user-sign-ins-for-an-enterprise-app-in-azure-active-directory"></a>Отключение входа пользователя в корпоративное приложение в Azure Active Directory

Его можно легко отключить корпоративное приложение, чтобы пользователи не могут войти в него в Azure Active Directory (Azure AD). Потребуются соответствующие разрешения для управления корпоративным приложением. И необходимо быть глобальным администратором для каталога.

## <a name="how-do-i-disable-user-sign-ins"></a>Как отключить вход пользователей?

1. Войдите на [портал Azure](https://portal.azure.com) с помощью учетной записи глобального администратора каталога.
1. Выберите **Все службы**, в текстовом поле введите **Azure Active Directory**, а затем нажмите клавишу **ВВОД**.
1. На **Azure Active Directory** -  ***имя_каталога*** (то есть области Azure AD для каталога, вы управляете), выбрать команду **корпоративные приложения**.
1. На **корпоративные приложения — все приложения** панели отобразится список приложений, вы можете управлять. Выберите приложение.
1. В области ***имя_приложения*** (то есть в области с именем выбранного приложения в заголовке) выберите **Свойства**.
1. В области ***имя_приложения*** - **Свойства** для параметра **Включен ли вход для пользователей?** выберите значение **Нет**.
1. Щелкните **Сохранить** .

## <a name="next-steps"></a>Следующие шаги

* [Просмотр всех моих групп](../fundamentals/active-directory-groups-view-azure-portal.md)
* [Назначение корпоративному приложению пользователя или группы](assign-user-or-group-access-portal.md)
* [Удаление назначения пользователя или группы из корпоративного приложения](remove-user-or-group-access-portal.md)
* [Изменение имени или логотипа корпоративного приложения](change-name-or-logo-portal.md)
