---
title: Техническое руководство по публикации SaaS-приложений в Azure Marketplace
description: Пошаговое руководство и контрольный список по публикации SaaS-приложений в Azure Marketplace
services: Marketplace, Compute, Storage, Networking, Blockchain, Security, SaaS
author: keithcharlie
ms.service: marketplace
ms.topic: article
ms.date: 07/09/2018
ms.author: kevidal
ms.openlocfilehash: 92c3452e07dee126666e6ee1fe0c46b1f8f6dfa4
ms.sourcegitcommit: de47a27defce58b10ef998e8991a2294175d2098
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/15/2019
ms.locfileid: "67876539"
---
# <a name="saas-applications-offer-publishing-guide"></a>Руководство по публикации SaaS-приложений

Приложения SaaS можно опубликовать в Marketplace с тремя разными призывами к действию: "Свяжитесь со мной", "Попробовать сейчас" и "Получить". В этом руководстве рассказывается об этих трех вариантах, в том числе о требованиях для каждого. 

## <a name="offer-overview"></a>Обзор предложения  

Приложения SaaS доступны в обоих онлайн-магазинах Azure. В следующей таблице описаны текущие доступные варианты:

| Вариант онлайн-магазина | Вывод списка | Пробная версия или транзакция |  
| --- | --- | --- |  
| AppSource | Да (связаться со мной) | Да (PowerBI, Dynamics) |
| Azure Marketplace | Нет | Да (приложения SaaS) |   

**Список**.  Вариант публикации "Список" включает тип предложения "Свяжитесь со мной" и используется, когда реализовать пробную версию или транзакцию нецелесообразно. Преимущество такого подхода заключается в том, что он позволяет издателям, решения которых уже опубликованы, немедленно получать сведения о потенциальных клиентах, с которыми можно заключить сделки, стимулирующие рост бизнеса.  
**Пробная версия или транзакция.**  Заказчик может непосредственно купить или запросить пробную версию вашего решения. Предоставление пробной версии положительно влияет на взаимодействие с клиентами и позволяет им изучить решение до покупки. Благодаря этому у вас будет больше шансов на продвижение в онлайн-магазинах, что позволит получать больше сведений о потенциальных клиентах. Для бесплатных пробных версий должна предоставляться бесплатная поддержка, по крайней мере на протяжении пробного периода.  

| Предложение приложений SaaS | Бизнес-требования | Технические требования |  
| --- | --- | --- |  
| **Связаться с нами** | Да | Нет |  
| **PowerBI, Dynamics** | Да | Да (интеграция с Azure AD) |  
| **Приложения SaaS**| Да | Да (интеграция с Azure AD) |     

## <a name="saas-list"></a>Список SaaS

Призыв к действию для списка SaaS без функций пробной версии и выставления счетов — "Свяжитесь со мной". 

Нет необходимости настраивать Azure Active Directory, чтобы добавить приложение SaaS в список. 

|Требования  |Сведения  |
|---------|---------|
|Приложение является предложением SaaS  |   Ваше решение — предложение SaaS с мультитенантным продуктом SaaS.      |


## <a name="saas-trial"></a>Пробная версия SaaS

Вы предоставляете решение или приложение с бесплатной пробной версией на основе технологии "программное обеспечение как услуга" (SaaS). Бесплатную пробную версию можно представить в виде пробной учетной записи с ограниченными возможностями или сроком действия. 


|Требования  |Сведения  |
|---------|---------|
|Приложение является предложением SaaS  |   Ваше решение — предложение SaaS с мультитенантным продуктом SaaS.      |
|Приложение поддерживает AAD     |   Клиент будет перенаправляться в ваш домен, и вы будете взаимодействовать с клиентом напрямую.       |


## <a name="saas-trial-technical-requirements"></a>Технические требования пробной версии SaaS

Технические требования для приложений SaaS просты. Для публикации издателям требуется только интеграция с Azure Active Directory (Azure AD). Интеграция Azure AD с приложениями описана во многих документах, и корпорация Майкрософт предоставляет множество ресурсов и пакетов SDK для выполнения этой задачи.  

Для начала вам нужна подписка, предназначенная для публикации в Azure Marketplace, чтобы изолировать этот сценарий от других инициатив. После этого можно приступать к развертыванию приложения SaaS в этой подписке и начинать разработку.  

Полезную документацию по Azure Active Directory, примеры и руководства см. последующим ссылкам: 

* [Руководство разработчика по Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-developers-guide)

* [Интеграция с Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-how-to-integrate)

* [Интеграция приложений с Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications)

* [Безопасность и идентификация на странице стратегии развития Azure](https://azure.microsoft.com/roadmap/?category=security-identity)

Видеоруководства см. по следующим ссылкам:

* [Видео о проверке подлинности Azure Active Directory с участием Витторио Берточчи (Vittorio Bertocci)](https://channel9.msdn.com/Shows/XamarinShow/Episode-27-Azure-Active-Directory-Authentication-with-Vittorio-Bertocci?term=azure%20active%20directory%20integration)

* [Технический брифинг. Удостоверение Azure Active Directory. Часть 1 из 2](https://channel9.msdn.com/Blogs/MVP-Enterprise-Mobility/Azure-Active-Directory-Identity-Technical-Briefing-Part-1-of-2?term=azure%20active%20directory%20integration)

* [Технический брифинг. Удостоверение Azure Active Directory. Часть 2 из 2](https://channel9.msdn.com/Blogs/MVP-Azure/Azure-Active-Directory-Identity-Technical-Briefing-Part-2-of-2?term=azure%20active%20directory%20integration)

* [Создание приложений с использованием Microsoft Azure Active Directory](https://channel9.msdn.com/Blogs/Windows-Development-for-the-Enterprise/Building-Apps-with-Microsoft-Azure-Active-Directory?term=azure%20active%20directory%20integration)

* [Видеоролики Microsoft Azure, посвященные Azure Active Directory](https://azure.microsoft.com/resources/videos/index/?services=active-directory)

Бесплатный курс обучения по Azure Active Directory см. по следующей ссылке:  
* [Серия материалов "Microsoft Azure для ИТ-специалистов": Azure Active Directory](https://mva.microsoft.com/training-courses/microsoft-azure-for-it-pros-content-series-azure-active-directory-16754?l=N0e23wtxC_2106218965)

Проверить наличие обновлений Azure Active Directory можно по следующей ссылке:   
* [Обновления службы Azure AD](https://azure.microsoft.com/updates/?product=active-directory)

## <a name="using-azure-active-directory-to-enable-trials"></a>Активация пробных версий с помощью Azure Active Directory  

Корпорация Майкрософт выполняет проверку подлинности всех пользователей Marketplace с помощью Azure AD, поэтому когда прошедший проверку пользователь щелкает список пробных версий в Marketplace и перенаправляется в вашу пробную среду, вы можете предоставить его непосредственно в пробную версию без необходимости дополнительный шаг входа. Токен, получаемый вашим приложением от Azure AD во время аутентификации, содержит ценные сведения о пользователе, которые помогут вам создать его учетную запись в своем приложении, чтобы автоматизировать процесс подготовки и повысить вероятность перехода. Дополнительные сведения о токене см. в статье [Справочник по токенам в Azure AD](https://docs.microsoft.com/azure/active-directory/develop/active-directory-token-and-claims).

С помощью Azure AD для приложения или пробной версии можно включить аутентификацию одним щелчком. Она предоставляет следующие преимущества:  
* упрощается взаимодействие клиентов с пробной версией в Marketplace;  
* сохраняется ощущение использования продукта, даже когда пользователь перенаправляется с Marketplace на ваш домен или в пробную среду;  
* сокращается вероятность отказа от перенаправления, так как отсутствует дополнительный шаг выполнения входа.  
* Уменьшаются ограничения развертывания для большого числа пользователей Azure AD.  

## <a name="certifying-your-azure-ad-integration-for-marketplace"></a>сертификация интеграции Azure AD для Marketplace.  

Сертифицировать интеграцию Azure AD несколькими способами, в зависимости от того, является ли ваше приложение однопользовательским или многопользовательским и является ли вы впервые интегрированным единым входом в Azure AD (SSO) или уже поддерживает его.  

**Мультитенантные приложения**  

Если вы уже используете Azure AD, сделайте следующее:
1.  Зарегистрируйте приложение на портале Azure.
2.  Включите функцию поддержки мультитенантности в Azure AD, чтобы получить пробную версию одним щелчком. Дополнительные сведения можно найти [здесь](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications).  

Если вы впервые используете федеративный единый вход в Azure AD, сделайте следующее: 
1.  Зарегистрируйте приложение на портале Azure.
2.  Настройте единый вход в Azure AD с помощью [OpenID Connect](https://docs.microsoft.com/azure/active-directory/develop/active-directory-protocols-openid-connect-code) или [OAuth 2.0](https://docs.microsoft.com/azure/active-directory/develop/active-directory-protocols-oauth-code).
3.  Включите функцию поддержки мультитенантности в AAD, чтобы получить пробную версию одним щелчком. Дополнительные сведения можно найти [здесь](https://docs.microsoft.com/azure/active-directory/develop/active-directory-devhowto-appsource-certified).  

**Для однотенантного приложения используйте любой из следующих вариантов:**  
* Добавьте пользователей в свой каталог в качестве гостевых пользователей, используя [Azure B2B](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b)
* Вручную подготовьте пробную версию для клиентов с использованием функции "Связаться со мной".
* Разработайте тестовый выпуск для одного клиента.
* Создайте демонстрационное мультитенантное приложение с SSO.

## <a name="saas-subscriptions"></a>Подписки SaaS

Используйте тип приложения SaaS, чтобы предоставить пользователю возможность приобрести ваше техническое решение на основе SaaS в виде подписки. Для приложения SaaS должны соблюдаться приведенные ниже требования.
- Счета за службу должны выставляться по фиксированному ежемесячному тарифу.
- Предоставьте способ обновления или отмены услуги в любой момент.
Коммерческие транзакции хранятся в корпорации Майкрософт. Корпорация Майкрософт выставляет счета вашим клиентам от вашего имени. Чтобы использовать приложение SaaS в виде подписки, необходимо включить собственный API службы управления подписками. API службы управления подписками должен взаимодействовать непосредственно с интерфейсами API Azure Resource Manager. API службы управления подписками должен поддерживать подготовку, обновление и отмену услуги.

| Требование | Сведения |  
|:--- |:--- |  
|Выставление счетов и ценообразование | Цена вашего предложения определяется помесячной фиксированной ставкой. Оплата по мере использования и возможность повышения уровня на основе использования сейчас не поддерживаются. |  
|Отмена | Ваше предложение может быть отменено клиентом в любое время. |  
|Целевая страница транзакции | Вы размещаете целевую страницу транзакции под совместным брендом в Azure, где пользователи могут создать учетную запись службы SaaS и управлять ею. |   
| API подписки | Вы предоставляете службу, которая может взаимодействовать с подпиской SaaS для создания, обновления и удаления учетной записи пользователя и плана обслуживания. Критические изменения в API должны поддерживаться в течение 24 часов. Некритические изменения в API будут выпускаться периодически. |  

>[!Note]
>Вы можете получить доступ к партнерским каналам поставщика облачных решений (CSP).  Дополнительные сведения о маркетинге вашего предложения с помощью партнерских каналов Microsoft CSP см. в статье [поставщики облачных решений](./cloud-solution-providers.md) .

## <a name="next-steps"></a>Следующие шаги
Если это еще не сделано,

- [зарегистрируйтесь](https://azuremarketplace.microsoft.com/sell) в Marketplace.

Если вы зарегистрированы и создаете предложение или работаете над существующим,

- [войдите на Портал Cloud Partner](https://cloudpartner.azure.com), чтобы создать или завершить предложение.
- Дополнительные сведения см. в статье [Предложение приложения Azure SaaS](https://docs.microsoft.com/azure/marketplace/partner-center-portal/create-new-saas-offer).
