---
title: включение файла
description: включение файла
ms.topic: include
ms.custom: include file
services: time-series-insights
ms.service: time-series-insights
author: kingdomofends
ms.author: adgera
ms.date: 07/02/2019
ms.openlocfilehash: a463e3cf475909c34054717460dc10dbba4ad8f0
ms.sourcegitcommit: 5bdd50e769a4d50ccb89e135cfd38b788ade594d
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/03/2019
ms.locfileid: "67543954"
---
> [!IMPORTANT]
> * С мая 2019 г. новая колонка **Azure Active Directory** > **Регистрация приложений** заменяет прежнюю колонку **Azure Active Directory** > **Регистрация приложений (прежняя версия)** .
> * Регистрация приложений, созданная или отображенная в прежней колонке, автоматически появится в новой.
> * Подробную информацию о переходе на новый интерфейс регистрации приложений Azure см. в статьях [Training guide: App registrations in the Azure portal](https://docs.microsoft.com/azure/active-directory/develop/app-registrations-training-guide) (Учебное руководство. Регистрация приложений на портале Azure) и [Краткое руководство. Регистрация приложения с помощью платформы удостоверений Майкрософт](https://docs.microsoft.com/azure/active-directory/develop/quickstart-register-app).

1. На [портале Azure](https://ms.portal.azure.com/) последовательно выберите **Azure Active Directory** > **Регистрация приложений** > **Новая регистрация**.

   [![Регистрация нового приложения в Azure Active Directory](media/time-series-insights-aad-registration/active-directory-new-application-registration.png)](media/time-series-insights-aad-registration/active-directory-new-application-registration.png#lightbox)

   > [!TIP]
   > С помощью новой панели регистрации приложения Azure Active Directory можно выполнить фильтрацию отображаемых приложений, выбрав параметр **Собственные приложения**.

    Приложение будет указано здесь после его регистрации.

1. Присвойте приложению имя и выберите пункт **Accounts in this organizational directory only** (Учетные записи только в этом каталоге организации),чтобы указать **поддерживаемые типы учетных записей**, с помощью которых можно получить доступ к API. Выберите допустимый URI для перенаправления пользователей после аутентификации, а затем нажмите кнопку **Зарегистрировать**.

   [![Создание приложения в Azure Active Directory](media/time-series-insights-aad-registration/active-directory-registration.png)](media/time-series-insights-aad-registration/active-directory-registration.png#lightbox)

1. Важная информация о приложении Azure Active Directory отображается в колонке **Обзор** указанного приложения. Выберите приложение в разделе **Собственные приложения** и щелкните **Обзор**.

   [![Копирование идентификатора приложения](media/time-series-insights-aad-registration/active-directory-copy-application-id.png)](media/time-series-insights-aad-registration/active-directory-copy-application-id.png#lightbox)

   Скопируйте значение параметра **Идентификатор приложения (клиента)** , чтобы использовать его в клиентском приложении.

1. В колонке **Аутентификация** указаны важные параметры конфигурации аутентификации. 

    1. **URI перенаправления** должны соответствовать адресу, указанному в запросе аутентификации.

        * Для приложений, размещенных в локальной среде разработки, выберите **Public client (mobile & desktop)** (Общедоступный клиент (мобильный и классический)). Убедитесь, что для параметра **Тип клиента по умолчанию** установлено значение "Да".
        * Для одностраничных приложений, размещенных в Службе приложений Azure, выберите **Веб**.

    1. Включите поток неявного предоставления разрешения, проверив **маркеры идентификации**.

   [![Создание секрета клиента](media/time-series-insights-aad-registration/active-directory-auth-blade.png)](media/time-series-insights-aad-registration/active-directory-auth-blade.png#lightbox)

   Выберите команду **Сохранить**.

1. Свяжите приложение Azure Active Directory с Аналитикой временных рядов Azure. Последовательно выберите **Разрешения API** > **Add a permission (Добавить разрешение)**  > **Интерфейсы API, используемые моей организацией**. 

    [![Связывание API с приложением Azure Active Directory](media/time-series-insights-aad-registration/active-directory-app-api-permission.png)](media/time-series-insights-aad-registration/active-directory-app-api-permission.png#lightbox)

   Введите `Azure Time Series Insights` в строке поиска, а затем выберите `Azure Time Series Insights`.

1. Затем укажите тип разрешения API, необходимый для приложения. По умолчанию будет выделен тип **Делегированные разрешения**. Выберите тип разрешения и щелкните **Добавить разрешения**.

    [![Выбор типа разрешения API, необходимого для приложения](media/time-series-insights-aad-registration/active-directory-app-permission-grant.png)](media/time-series-insights-aad-registration/active-directory-app-permission-grant.png#lightbox)