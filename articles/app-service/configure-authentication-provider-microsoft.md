---
title: Настройка проверки подлинности учетной записи Майкрософт в Службе приложений Azure
description: Настройка проверки подлинности учетной записи Майкрософт для приложения служб приложений.
author: mattchenderson
services: app-service
documentationcenter: ''
manager: syntaxc4
editor: ''
ms.assetid: ffbc6064-edf6-474d-971c-695598fd08bf
ms.service: app-service
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 04/19/2018
ms.author: mahender
ms.custom: seodec18
ms.openlocfilehash: e3da856efd7d44f15f9de27c9e38375d40dc211d
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "60850976"
---
# <a name="how-to-configure-your-app-service-application-to-use-microsoft-account-login"></a>Настройка приложения службы приложений для использования входа по учетной записи Майкрософт
[!INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]

В этом разделе показано, как настроить службу приложений Azure для использования учетной записи Майкрософт в качестве поставщика проверки подлинности. 

## <a name="register-microsoft-account"> </a>Регистрация приложения с использованием учетной записи Майкрософт
1. Перейдите на [портал Azure]и перейдите к своему приложению. Скопируйте **URL-адрес**, который позже понадобится для настройки приложения с помощью учетной записи Майкрософт.
2. Перейдите на страницу [Мои приложения] в центре разработки для учетной записи Майкрософт и войдите по учетной записи Майкрософт, если это требуется.
3. Щелкните **Добавить приложение**, введите имя приложения и выберите **Создать**.
4. Запишите **идентификатор приложения**, так как он понадобится позже. 
5. В разделе "Платформы" щелкните **Добавить платформу** и выберите "Интернет".
6. В разделе "URI перенаправления" укажите конечную точку для вашего приложения, а затем щелкните **Сохранить**. 
   
   > [!NOTE]
   > Ваш универсальный код ресурса (URI) перенаправления — это URL-адрес приложения, дополненный путем */.auth/login/microsoftaccount/callback*. Например, `https://contoso.azurewebsites.net/.auth/login/microsoftaccount/callback`.   
   > Убедитесь, что используете схему HTTPS.
   
7. В разделе "Секреты приложения" щелкните **Создать новый пароль**. Запишите отображенное значение. Если страница закроется, оно больше не отобразится.

    > [!IMPORTANT]
    > Пароль — это важный элемент обеспечения безопасности. Не сообщайте пароль никому и не раскрывайте его в клиентском приложении.
    
8. Нажмите кнопку **Сохранить**

## <a name="secrets"> </a>Добавление данных учетной записи Майкрософт в приложение службы приложений
1. На [портал Azure] перейдите к своему приложению, а затем щелкните **Параметры** > **Проверка подлинности и авторизация**.
2. Если функция проверки подлинности и авторизации не включена, установите переключатель в положение **Включено**.
3. Щелкните **учетную запись Майкрософт**. Вставьте значения идентификатора приложения и пароля, полученные ранее, и при необходимости включите любые области, которые требуются приложению. Нажмите кнопку **ОК**.
   
    ![][1]
   
    По умолчанию служба приложений обеспечивает проверку подлинности, но не ограничивает авторизованный доступ к содержимому сайта и API. Авторизация пользователей должна быть включена в код приложения.
4. (Необязательно.) Чтобы предоставить доступ к сайту только пользователям, прошедшим проверку подлинности по учетной записи Майкрософт, установите для параметра **Предпринимаемое действие, если проверка подлинности для запроса не выполнена** значение **Учетная запись Майкрософт**. В этом случае все запросы, не прошедшие проверку подлинности, направляются для проверки подлинности по учетной записи Майкрософт.
5. Выберите команду **Сохранить**.

Теперь вы можете использовать учетную запись Майкрософт для проверки подлинности в приложении.

## <a name="related-content"> </a>Связанная информация
[!INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]

<!-- Images. -->

[0]: ./media/app-service-mobile-how-to-configure-microsoft-authentication/app-service-microsoftaccount-redirect.png
[1]: ./media/app-service-mobile-how-to-configure-microsoft-authentication/mobile-app-microsoftaccount-settings.png

<!-- URLs. -->

[Мои приложения]: https://go.microsoft.com/fwlink/p/?LinkId=262039
[портал Azure]: https://portal.azure.com/
