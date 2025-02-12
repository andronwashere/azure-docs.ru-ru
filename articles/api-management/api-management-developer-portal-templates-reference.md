---
title: Шаблоны портала разработчика в службе управления API Azure | Документация Майкрософт
description: Сведения о настройке содержимого страниц портала разработчика с использованием набора шаблонов в службе управления API Azure.
services: api-management
documentationcenter: ''
author: vladvino
manager: erikre
editor: ''
ms.assetid: 5189f3d8-2a4c-4dc8-ab19-11c7df0114d4
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/19/2017
ms.author: apimpm
ms.openlocfilehash: 0f339984a9db1a337f97e6db24571004cc34f679
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "60657651"
---
# <a name="developer-portal-templates"></a>Шаблоны портала разработчика

Служба управления API Azure позволяет настраивать содержимое страниц портала разработчика с помощью набора шаблонов. С помощью этих шаблонов вы можете гибко настраивать содержимое страниц, используя синтаксис [DotLiquid](http://dotliquidmarkup.org/), любой удобный текстовый редактор, например [DotLiquid для разработчиков](https://github.com/dotliquid/dotliquid/wiki/DotLiquid-for-Designers), и предоставленный набор локализованных [строковых ресурсов](api-management-template-resources.md#strings), [ресурсов глифов](api-management-template-resources.md#glyphs), а также [элементов управления страницы](api-management-page-controls.md).  
  
Дополнительные сведения о работе с шаблонами см. в статье [Настройка портала разработчика в службе управления API Azure с помощью шаблонов](api-management-developer-portal-templates.md).  

[!INCLUDE [premium-dev-standard-basic.md](../../includes/api-management-availability-premium-dev-standard-basic.md)]

##  <a name="DeveloperPortalTemplates"></a>Шаблоны портала разработчика  
  
-   [Интерфейсы API](api-management-api-templates.md)  
    -   [Список API](api-management-api-templates.md#APIList)  
    -   [Операция](api-management-api-templates.md#Product)  
    -   [Примеры кода](api-management-api-templates.md#CodeSamples)  
        -   [Curl](api-management-api-templates.md#Curl)  
        -   [C#](api-management-api-templates.md#CSharp)  
        -   [Java](api-management-api-templates.md#Stub)  
        -   [JavaScript](api-management-api-templates.md#JavaScript)  
        -   [Objective C](api-management-api-templates.md#ObjectiveC)  
        -   [PHP](api-management-api-templates.md#PHP)  
        -   [Python](api-management-api-templates.md#Python)  
        -   [Ruby](api-management-api-templates.md#Ruby)  
-   [Продукты](api-management-product-templates.md)  
    -   [Список продуктов](api-management-product-templates.md#ProductList)  
    -   [Продукт](api-management-product-templates.md#Product)  
-   [Приложения](api-management-application-templates.md)  
    -   [Список приложений](api-management-application-templates.md#ProductList)  
    -   [Приложения](api-management-application-templates.md#Application)  
-   [Проблемы](api-management-issue-templates.md)  
    -   [Список проблем](api-management-issue-templates.md#IssueList)  
-   [Профиль пользователя](api-management-user-profile-templates.md)  
    -   [Профиль](api-management-user-profile-templates.md#Profile)  
    -   [Подписки](api-management-user-profile-templates.md#Subscriptions).  
    -   [Приложения](api-management-user-profile-templates.md#Applications).  
    -   [Обновить сведения об учетной записи](api-management-user-profile-templates.md#UpdateAccountInfo)  
-   [Страницы](api-management-page-templates.md)  
    -   [Вход](api-management-page-templates.md#SignIn)  
    -   [Регистрация](api-management-page-templates.md#SignUp)  
    -   [Страница не найдена](api-management-page-templates.md#PageNotFound)

## <a name="next-steps"></a>Дальнейшие действия  

+ [Справочник по шаблонам](api-management-developer-portal-templates-reference.md)  
+ [Справочник по моделям данных](api-management-template-data-model-reference.md)  
+ [Элементы управления страницы](api-management-page-controls.md)  
+ [Ресурсы шаблонов](api-management-template-resources.md)