---
title: Настройка ролей RBAC для административного доступа к службе "Поиск Azure" на портале
description: Сведения о настройке административного управления доступом на основе ролей на портале Azure для контроля и делегирования задач администрирования в службе "Поиск Azure".
author: HeidiSteen
manager: cgronlun
services: search
ms.service: search
ms.topic: conceptual
ms.date: 04/05/2019
ms.author: heidist
ms.custom: seodec2018
ms.openlocfilehash: 34def35eba1e5c1645e6e1f9a505704d153ac716
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "61282395"
---
# <a name="set-rbac-roles-for-administrative-access"></a>Настройка ролей RBAC с правами администратора

Azure реализует [глобальную модель авторизации на основе ролей](../role-based-access-control/role-assignments-portal.md) для всех служб, управляемых через портал или API Resource Manager. Роли владельца, участника или читателя определяют уровень *администрирования службы* для пользователей, групп и субъектов безопасности Active Directory, назначенных для каждой роли. 

> [!Note]
> Элементы управления доступом на основе ролей для защиты частей индекса или набора документов отсутствуют. Чтобы применить доступ к результатам поиска на основе удостоверений, вы можете создать фильтры безопасности по идентификаторам, удаляя из результатов те документы, к которым запрашивающая сторона не должна иметь доступ. Дополнительные сведения см. в статьях о [фильтрах безопасности](search-security-trimming-for-azure-search.md) и [безопасности в Active Directory](search-security-trimming-for-azure-search-with-aad.md).

## <a name="management-tasks-by-role"></a>Задачи управления с использованием разных ролей

В службе "Поиск Azure" роли связаны с уровнями разрешений, которые позволяют выполнять следующие задачи управления:

| Роль | Задача |
| --- | --- |
| Владелец. |Создание или удаление службы или любого объекта в службе, включая ключи API, индексы, индексаторы, источники данных индексаторов и расписания индексаторов.<p>Просмотр состояния службы, в том числе счетчиков и размера хранилища.<p>Добавление или удаление ролевой принадлежности (только владелец может управлять ролевой принадлежностью).<p>Администраторам подписки и владельцам службы автоматически присваивается роль владельца. |
| Участник |Тот же уровень доступа, что и владелец, но без возможности управлять ролями. Например, участник может создавать, удалять или просматривать и повторно создавать ключи [api-keys](search-security-api-keys.md), но не может изменять членство в ролях. |
| [Встроенная роль участника Службы поиска](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#search-service-contributor) | Аналог роли участника. |
| Читатель |Просмотр основных компонентов и метрик службы. Члены этой роли не могут просматривать сведения об индексе, индексаторе, источнике данных или ключе.  |

Роли не гарантируют права доступа к конечной точке службы. Операции службы поиска, такие как управление индексами, совокупностями индексов и поисковые запросы к данным, управляются посредством ключей API, а не через роли. Дополнительные сведения см. в разделе [Управление ключами API](search-security-api-keys.md).

## <a name="see-also"></a>См. также

+ [Управление с помощью PowerShell](search-manage-powershell.md) 
+ [Производительность и оптимизация в службе "Поиск Azure"](search-performance-optimization.md)
+ [Начало работы с управлением доступом на основе ролей на портале Azure](../role-based-access-control/overview.md).