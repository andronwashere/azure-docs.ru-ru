---
title: Вход пользователей и вызов API Microsoft Graph из классического приложения .NET (WPF) | Документация Майкрософт
description: Узнайте, как создать классическое приложение .NET Windows, которое интегрируется с Azure AD для входа в систему и вызывает программные интерфейсы приложения, защищенные Azure AD, по протоколу OAuth 2.0.
services: active-directory
documentationcenter: .net
author: rwike77
manager: CelesteDG
editor: ''
ms.assetid: ed33574f-6fa3-402c-b030-fae76fba84e1
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: quickstart
ms.date: 07/17/2019
ms.author: ryanwi
ms.reviewer: jmprieur
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 946d376ef8af7d08b7924bd5b126c8b04e555668
ms.sourcegitcommit: 4b431e86e47b6feb8ac6b61487f910c17a55d121
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/18/2019
ms.locfileid: "68324413"
---
# <a name="quickstart-sign-in-users-and-call-the-microsoft-graph-api-from-a-net-desktop-wpf-app"></a>Краткое руководство. Вход пользователей и вызов API Microsoft Graph из классического приложения .NET (WPF)

[Платформа удостоверений Майкрософт](v2-overview.md) — это решение на основе платформы для разработчиков Azure Active Directory (Azure AD). С ее помощью разработчики могут создавать приложения, которые обеспечивают вход с использованием любых удостоверений Майкрософт и получение маркеров для вызова таких программных API-интерфейсов Майкрософт, как Microsoft Graph или других программных интерфейсов, созданных разработчиками.

[Библиотека аутентификации Майкрософт (MSAL)](msal-overview.md) позволяет разработчикам получать маркеры из конечной точки платформы удостоверений Майкрософт, чтобы получить доступ к защищенным веб-API. Библиотека проверки подлинности Active Directory (ADAL) интегрируется с конечной точкой Azure AD для разработчиков (версия 1.0), а MSAL интегрируется с конечной точкой платформы удостоверений Майкрософт (версия 2.0).

С новыми классическими приложениями мы рекомендуем использовать платформу удостоверений Майкрософт (версия 2.0) и MSAL для получения маркеров и доступа к защищенным веб-API: [Краткое руководство Получение маркера безопасности и вызов API Microsoft Graph из классического приложения Windows](quickstart-v2-windows-desktop.md)
