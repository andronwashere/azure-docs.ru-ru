---
title: Создание мобильного приложения для iOS в службе мобильных приложений Azure | Документация Майкрософт
description: Изучите этот учебник, чтобы начать работу с серверным частями мобильных приложений Azure для разработки приложений iOS на Objective-C или Swift
services: app-service\mobile
documentationcenter: ios
author: elamalani
manager: crdun
editor: ''
ms.assetid: 6461a899-9340-42dd-b118-ffc5ba00e846
ms.service: app-service-mobile
ms.workload: na
ms.tgt_pltfrm: mobile-ios
ms.devlang: objective-c
ms.topic: conceptual
ms.date: 06/25/2019
ms.author: emalani
ms.openlocfilehash: 9140de50e23447f53fd1fa204a2a67c324672397
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/28/2019
ms.locfileid: "67449140"
---
# <a name="create-an-ios-app"></a>Создание приложения iOS

[!INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

> [!NOTE]
> Центр приложений Visual Studio вкладывает средства в новые и интегрированной службы, необходимые для разработки мобильных приложений. Разработчики могут использовать **построения**, **теста** и **распределить** служб для настройки конвейера непрерывной интеграции и доставки. После развертывания приложения, разработчики могут отслеживать состояние и использования их приложений с помощью **Analytics** и **диагностики** служб и общайтесь с пользователями, с помощью **Push** Служба. Разработчики также могут использовать **Auth** подлинность пользователей и **данных** службы для сохранения и синхронизировать данные приложения в облаке. Ознакомьтесь с [App Center](https://appcenter.ms/?utm_source=zumo&utm_campaign=app-service-mobile-ios-get-started) уже сегодня.
>

## <a name="overview"></a>Обзор

В этом руководстве показано, как добавить облачную серверную службу [Мобильные приложения службы приложений](app-service-mobile-value-prop.md) в приложение iOS. Сначала в Azure создается мобильный внутренний сервер. Затем скачивается простой пример приложения iOS со *списком задач*, в котором хранятся данные в Azure.

Для работы с этим учебником требуется [учетная запись Azure](https://azure.microsoft.com/pricing/free-trial/)

## <a name="create-a-new-azure-mobile-app-backend"></a>Создание серверной части мобильного приложения Azure

[!INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]

## <a name="create-a-database-connection-and-configure-the-client-and-server-project"></a>Создание подключения к базе данных и настройка проекта клиента и сервера
[!INCLUDE [app-service-mobile-configure-new-backend.md](../../includes/app-service-mobile-configure-new-backend.md)]

## <a name="run-the-ios-app"></a>Запуск приложения iOS

[!INCLUDE [app-service-mobile-ios-run-app](../../includes/app-service-mobile-ios-run-app.md)]

<!-- URLs -->
[Azure portal]: https://portal.azure.com/
[Xcode]: https://go.microsoft.com/fwLink/p/?LinkID=266532
[Visual Studio Community 2013]: https://go.microsoft.com/fwLink/p/?LinkID=534203
