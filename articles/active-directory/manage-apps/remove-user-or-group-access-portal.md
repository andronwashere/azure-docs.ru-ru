---
title: Удаление назначений пользователей или групп из приложения в Azure Active Directory | Документация Майкрософт
description: Узнайте, как удалить назначение доступа к корпоративному приложению для пользователя или группы в Azure Active Directory.
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
ms.openlocfilehash: 380816283156969c47f45a9b47435688df91f4ca
ms.sourcegitcommit: 04ec7b5fa7a92a4eb72fca6c6cb617be35d30d0c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/22/2019
ms.locfileid: "68381047"
---
# <a name="remove-a-user-or-group-assignment-from-an-enterprise-app-in-azure-active-directory"></a>Удаление назначения доступа к корпоративному приложению для пользователя или группы в Azure Active Directory

Вы можете легко удалить пользователя или группу из назначенного доступа к одному из корпоративных приложений в Azure Active Directory (Azure AD). Необходимы соответствующие разрешения для управления корпоративным приложением. И вы должны быть глобальным администратором для каталога.

> [!NOTE]
> В приложениях Майкрософт (например, в приложениях Office 365) удалить пользователей в корпоративном приложении можно с помощью PowerShell.

## <a name="how-do-i-remove-a-user-or-group-assignment-to-an-enterprise-app-in-the-azure-portal"></a>Как удалить назначение доступа к корпоративному приложению для пользователя или группы на портале Azure?

1. Войдите на [портал Azure](https://portal.azure.com) с помощью учетной записи глобального администратора каталога.
1. Выберите **Все службы**, в текстовом поле введите **Azure Active Directory**, а затем нажмите клавишу **ВВОД**.
1. На странице **Azure Active Directory- *директоринаме***  (то есть на странице Azure AD для каталога, которым вы управляете) выберите **корпоративные приложения**.
1. На странице **корпоративные приложения — все приложения** вы увидите список приложений, которыми вы можете управлять. Выберите приложение.
1. На странице "Обзор" ***AppName*** (то есть на странице с именем выбранного приложения в заголовке) выберите **Пользователи & группы**.
1. На странице ***имя_приложения*** **— User & Group Assignment** (Назначение групп и пользователей) выберите одного из нескольких пользователей или групп, а затем щелкните **Удалить**. При появлении запроса подтвердите свое решение.

## <a name="how-do-i-remove-a-user-or-group-assignment-to-an-enterprise-app-using-powershell"></a>Как удалить назначение доступа к корпоративному приложению для пользователя или группы с помощью PowerShell?

1. Откройте окно Windows PowerShell с повышенными привилегиями.

   > [!NOTE]
   > Вам потребуется установить модуль Azure AD (с помощью команды `Install-Module -Name AzureAD`). Если будет предложено установить модуль NuGet или новый модуль PowerShell Azure для Active Directory версии 2, введите Y и нажмите клавишу ВВОД.

1. Выполните команду `Connect-AzureAD` и войдите в систему с помощью учетных данных глобального администратора.
1. Чтобы удалить пользователя и роль из приложения, используйте следующий скрипт:

    ```powershell
    # Store the proper parameters
    $user = get-azureaduser -ObjectId <objectId>
    $spo = Get-AzureADServicePrincipal -ObjectId <objectId>

    #Get the ID of role assignment 
    $assignments = Get-AzureADServiceAppRoleAssignment -ObjectId $spo.ObjectId | Where {$_.PrincipalDisplayName -eq $user.DisplayName}

    #if you run the following, it will show you what is assigned what
    $assignments | Select *

    #To remove the App role assignment run the following command.
    Remove-AzureADServiceAppRoleAssignment -ObjectId $spo.ObjectId -AppRoleAssignmentId $assignments[assignment #].ObjectId
    ```

## <a name="next-steps"></a>Следующие шаги

- [Просмотр всех моих групп](../fundamentals/active-directory-groups-view-azure-portal.md)
- [Назначение корпоративному приложению пользователя или группы](assign-user-or-group-access-portal.md)
- [Отключение входа пользователя в корпоративное приложение](disable-user-sign-in-portal.md)
- [Изменение имени или логотипа корпоративного приложения](change-name-or-logo-portal.md)
