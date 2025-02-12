---
title: Рекомендации по топологии сети для Azure AD Application Proxy | Документация Майкрософт
description: В этой статье рассматриваются аспекты топологии сети при использовании прокси приложения Azure AD.
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
ms.date: 07/22/2019
ms.author: mimart
ms.reviewer: harshja
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: a7320df63885f562b4724285a3ca5c3cf6ea2a52
ms.sourcegitcommit: 04ec7b5fa7a92a4eb72fca6c6cb617be35d30d0c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/22/2019
ms.locfileid: "68381440"
---
# <a name="network-topology-considerations-when-using-azure-active-directory-application-proxy"></a>Аспекты топологии сети при использовании прокси приложения Azure Active Directory

В этой статье рассматриваются аспекты топологии сети при использовании прокси приложения Azure Active Directory для публикации и удаленного доступа к приложениям.

## <a name="traffic-flow"></a>Поток трафика

При публикации приложения через прокси приложения Azure AD весь трафик, передаваемый от пользователей к приложениям, проходит через три следующих подключения:

1. Пользователь подключается к общедоступной конечной точке службы прокси приложения Azure AD в Azure.
1. Служба прокси приложения подключается к соединителю прокси приложения.
1. Соединитель прокси приложения подключается к целевому приложению.

![Схема передачи трафика от пользователя к целевому приложению](./media/application-proxy-network-topology/application-proxy-three-hops.png)

## <a name="tenant-location-and-application-proxy-service"></a>Расположение клиента и службы прокси приложения

При регистрации в клиенте Azure AD регион клиента определяется указанной страной или регионом. Когда вы включаете прокси приложения, экземпляры службы прокси приложения для вашего клиента выбираются или создаются в том же регионе, где находится клиент Azure AD, или в ближайшем к нему регионе.

Например, если страна и регион клиента Azure AD – Великобритания, все соединители прокси приложения для него будут использовать экземпляры службы в центрах обработки данных на территории EС. Когда ваши пользователи обращаются к опубликованным приложениям, этот трафик проходит через экземпляры службы прокси приложения в этом расположении.

## <a name="considerations-for-reducing-latency"></a>Рекомендации по уменьшению задержки

Любые решения прокси увеличивают задержку сетевого подключения. Независимо от того, какое решение прокси или VPN используется для удаленного доступа, любое из них содержит набор серверов для подключения к корпоративной сети.

Организации обычно размещают серверные конечные точки в сети периметра. Однако при использовании прокси приложения Azure AD трафик проходит через прокси-службу в облаке, а соединители находятся в корпоративной сети. Сеть периметра не требуется.

В следующих разделах приведены дополнительные рекомендации по сокращению задержки. 

### <a name="connector-placement"></a>Расположение соединителя

Служба прокси приложения автоматически выбирает расположение экземпляров, исходя из расположения клиента. Тем не менее вам следует выбрать место установки соединителя, и от него будут зависеть характеристики задержки сетевого трафика.

Настраивая службу прокси приложения, задайте себе следующие вопросы:

- Где расположено приложение?
- Где находится больше всего пользователей, обращающихся к приложению?
- Где находится экземпляр прокси приложения?
- Есть ли у вас выделенное сетевое подключение к центрам обработки данных Azure (например, Azure ExpressRoute или аналогичное VPN-подключение)?

Соединитель должен взаимодействовать с Azure и приложениями (шаги 2 и 3 на схеме потока трафика), поэтому расположение соединителя влияет на задержку этих двух подключений. При выборе расположения для соединителя учитывайте следующее.

- Если вы хотите использовать ограниченное делегирование Kerberos (KCD) для единого входа, то соединителю нужен "канал прямой видимости" к центру обработки данных. Кроме того, сервер соединителя должен быть присоединен к домену.  
- Если вы сомневаетесь, установите соединитель ближе к приложению.

### <a name="general-approach-to-minimize-latency"></a>Общий подход к уменьшению задержки

Вы можете уменьшить задержку сквозного трафика, оптимизировав каждое из сетевых подключений. Каждое подключение можно оптимизировать следующим образом:

- уменьшить расстояние между двумя конечными точками прыжка;
- выбрать нужную сеть для прохода (например, проход частной сети, а не общедоступного Интернета, может выполняться быстрее из-за выделенных каналов связи).

Можно использовать выделенный канал связи VPN или ExpressRoute между Azure и корпоративной сетью, если он у вас есть.

## <a name="focus-your-optimization-strategy"></a>Направление стратегии оптимизации

Мало что можно сделать для управления подключением между пользователями и службой прокси приложения. Пользователи могут получить доступ к вашим приложениям из домашней сети, кафе или другой страны или региона. Вместо этого можно оптимизировать подключения между службой прокси приложения, соединителями прокси приложения и приложениями. Рекомендуется реализовать в вашей среде указанные ниже шаблоны.

### <a name="pattern-1-put-the-connector-close-to-the-application"></a>Шаблон 1. Размещение соединителя ближе к приложению

Расположите соединитель близко к целевому приложению в сети клиента. Эта конфигурация позволяет свести к минимуму влияние шага 3 на схеме топографии, так как соединитель и приложение расположены рядом.

Если соединитель должен быть в непосредственной видимости контроллера домена, то этот шаблон является предпочтительным. Большая часть наших клиентов использует этот шаблон, так как он хорошо подходит для большинства сценариев. Этот шаблон можно также совместить с шаблоном 2, чтобы оптимизировать трафик между службой и соединителем.

### <a name="pattern-2-take-advantage-of-expressroute-with-microsoft-peering"></a>Шаблон 2. Использование преимуществ ExpressRoute с пирингом Майкрософт

Если ExpressRoute настроен с пирингом Майкрософт, быстрое подключение ExpressRoute можно использовать для передачи трафика между прокси приложением и соединителем. Соединитель по-прежнему располагается в сети и вблизи от приложения.

### <a name="pattern-3-take-advantage-of-expressroute-with-private-peering"></a>Шаблон 3. Использование преимуществ ExpressRoute с частным пирингом

Если у вас есть выделенное подключение VPN или ExpressRoute с частным пирингом между Azure и корпоративной сетью, можно использовать другой вариант. В этой конфигурации виртуальная сеть в Azure обычно рассматривается как расширение корпоративной сети. Это означает, что вы можете установить соединитель в центре обработки данных Azure, сохраняя низкую задержку для подключения соединителя к приложению.

Задержка не увеличивается, так как трафик передается через выделенное подключение. Кроме того, улучшается задержка между соединителем и службой прокси приложения, так как соединитель устанавливается в центре обработки данных Azure, рядом с расположением клиента Azure AD.

![Схема, демонстрирующая установку соединителя в центре обработки данных Azure](./media/application-proxy-network-topology/application-proxy-expressroute-private.png)

### <a name="other-approaches"></a>Другие подходы

Эта статья посвящена выбору расположения для соединителя, но для улучшения характеристик задержки вы можете также переместить само приложение.

Все больше организаций перемещают сети в размещенные среды. В этой ситуации приложения также могут мигрировать в размещенную среду, оставаясь в пределах корпоративной сети и домена. В шаблонах, которые описаны в предыдущем разделе, можно учесть новое расположение приложения. Если вы рассматриваете этот вариант, перейдите на страницу [Доменные службы Azure Active Directory](../../active-directory-domain-services/overview.md).

Кроме того, рассмотрите возможность использования [групп соединителей](application-proxy-connector-groups.md) для целевых приложений, находящихся в разных расположениях и сетях.

## <a name="common-use-cases"></a>Распространенные варианты использования

В этом разделе рассматриваются несколько распространенных сценариев. Предполагается, что клиент Azure AD (а значит, и конечная точка службы прокси) находится в США. Все аспекты, рассмотренные в описанных вариантах, применимы и к другим регионам мира.

В этих сценариях мы называем соединения "прыжками" и присваиваем им номера:

- **Прыжок 1:** от пользователя к службе прокси приложения.
- **Прыжок 2:** от службы прокси приложения к соединителю прокси приложения.
- **Прыжок 3:** от соединителя прокси приложения к целевому приложению. 

### <a name="use-case-1"></a>Вариант использования 1

**Сценарий.** Приложение находится в корпоративной сети в США, там же расположены и пользователи. Отсутствуют подключения ExpressRoute или VPN между контроллером домена Azure и корпоративной сетью.

**Рекомендация.** Применяйте шаблон 1, описанный в предыдущем разделе. Если необходимо уменьшить задержку, рассмотрите возможность создать подключение ExpressRoute.

Это простой шаблон. Вы оптимизируете прыжок 3, располагая соединитель рядом с приложением. Это самый естественный вариант, поскольку соединителю требуется "прямая видимость" приложения и центра обработки данных, чтобы выполнять операции ограниченного делегирования Kerberos.

![На схеме показаны пользователи, прокси-сервер, соединитель и приложение в США](./media/application-proxy-network-topology/application-proxy-pattern1.png)

### <a name="use-case-2"></a>Вариант использования 2

**Сценарий.** Приложение находится в корпоративной сети в США, а пользователи распределены по всему миру. Отсутствуют подключения ExpressRoute или VPN между контроллером домена Azure и корпоративной сетью.

**Рекомендация.** Применяйте шаблон 1, описанный в предыдущем разделе.

Здесь снова наиболее применима оптимизация прыжка 3 за счет размещения соединителя рядом с приложением. Обычно прыжок 3 не будет дорогим, если он полностью проходит в одном регионе. Но прыжок 1 может оказаться намного дороже в зависимости от расположения пользователя, так как пользователи со всего мира обращаются к одному экземпляру прокси приложения, размещенному в США. Следует отметить, что в случае глобального распределения пользователя любые решения прокси будет иметь сходные характеристики.

![Пользователи распределяются глобально, но все остальное находится в США.](./media/application-proxy-network-topology/application-proxy-pattern2.png)

### <a name="use-case-3"></a>Вариант использования 3

**Сценарий.** Приложение находится в корпоративной сети клиента на территории США. Между Azure и корпоративной сетью существует ExpressRoute и корпоративная сеть.

**Рекомендация.** Применяйте шаблоны 1 и 2, описанные в предыдущем разделе.

Сначала разместите соединитель как можно ближе к приложению. Тогда система автоматически использует ExpressRoute для прыжка 2.

Если ссылка ExpressRoute использует пиринг Майкрософт, трафик между прокси и соединителем будет проходить через эту ссылку. Задержка для прыжка 2 будет оптимизирована.

![Схема, показывающая ExpressRoute между прокси и соединителем](./media/application-proxy-network-topology/application-proxy-pattern3.png)

### <a name="use-case-4"></a>Вариант использования 4

**Сценарий.** Приложение находится в корпоративной сети клиента на территории США. Между Azure и корпоративной сетью установлено подключение ExpressRoute с частным пирингом.

**Рекомендация.** Применяйте шаблон 3, описанный в предыдущем разделе.

Разместите соединитель в центре обработки данных Azure, который подключен к корпоративной сети через частный пиринг ExpressRoute.

Соединитель можно разместить в центре обработки данных Azure. Так как соединитель по-прежнему имеет "прямую видимость" приложения и центра обработки данных через частную сеть, прыжок 3 остается оптимизированным. Кроме того, теперь оптимизирован и прыжок 2.

![Соединитель в центре обработки данных Azure, ExpressRoute между соединителем и приложением](./media/application-proxy-network-topology/application-proxy-pattern4.png)

### <a name="use-case-5"></a>Вариант использования 5

**Сценарий.** Приложение находится в сети организации в ЕС, а экземпляр прокси приложения и большинство пользователей – в США.

**Рекомендация.** Разместите соединитель рядом с приложением. Так как пользователи США обращаются к экземпляру прокси приложения, размещенному в одном с ними регионе, прыжок 1 не будет слишком дорогим. Прыжок 3 оптимизирован. Рекомендуется использовать ExpressRoute для оптимизации прыжка 2.

![На схеме показаны пользователи и прокси-сервер в США, соединителе и приложении в ЕС](./media/application-proxy-network-topology/application-proxy-pattern5b.png)

Также в этой ситуации можно применить еще один вариант. Если большинство пользователей организации находятся в США, возможно ваша сеть уже "расширена" в этот регион. Разместите соединитель в США и используйте выделенный канал внутренней корпоративной сети для подключения к приложению в ЕС. Этот позволит оптимизировать прыжки 2 и 3.

![На схеме показаны пользователи, прокси-сервер и соединитель в США, приложение в ЕС](./media/application-proxy-network-topology/application-proxy-pattern5c.png)

## <a name="next-steps"></a>Следующие шаги

- [Включение прокси приложения](application-proxy-add-on-premises-application.md)
- [Включение единого входа](application-proxy-configure-single-sign-on-with-kcd.md)
- [Включить условный доступ](application-proxy-integrate-with-sharepoint-server.md)
- [Устранение неполадок с прокси приложения](application-proxy-troubleshoot.md)
