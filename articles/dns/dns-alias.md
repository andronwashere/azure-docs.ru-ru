---
title: Обзор записей псевдонимов Azure DNS
description: Общие сведения о поддержке записей псевдонимов в Microsoft Azure DNS.
services: dns
author: vhorne
ms.service: dns
ms.topic: article
ms.date: 07/19/2019
ms.author: victorh
ms.openlocfilehash: 89b50cff2d46f8c92c09653aeaac49551c97e9c6
ms.sourcegitcommit: da0a8676b3c5283fddcd94cdd9044c3b99815046
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/18/2019
ms.locfileid: "68314469"
---
# <a name="azure-dns-alias-records-overview"></a>Обзор записей псевдонимов Azure DNS

Записи псевдонимов Azure DNS являются квалификациями на набор записей DNS. Они позволяют ссылаться на другие ресурсы Azure из вашей зоны DNS. Например, вы можете создать набор записей псевдонимов, который ссылается на общедоступный IP-адрес Azure вместо записи A. Псевдоним набора записей динамически указывает на экземпляр сервиса общедоступного IP-адреса Azure. В результате псевдоним набора записей просто обновляется во время разрешения DNS.

Набор записей псевдонимов поддерживается для следующих типов записей в зоне DNS Azure: 

- А
- AAAA
- CNAME

> [!NOTE]
> Если вы планируете использовать запись псевдонима для записей типов A или AAAA, чтобы указать на [профиль диспетчера трафика Azure](../traffic-manager/quickstart-create-traffic-manager-profile.md), необходимо убедиться в том, что профиль диспетчера трафика имеет только [внешние конечные точки](../traffic-manager/traffic-manager-endpoint-types.md#external-endpoints). Укажите IPv4- или IPv6-адрес для внешних конечных точек в диспетчере трафика. В конечных точках нельзя использовать полные доменные имена (FQDN). Рекомендуется использовать статические IP-адреса.

## <a name="capabilities"></a>Возможности

- **Выберите ресурс общедоступного IP-адреса из набора DNS-записей A/AAAA**. Можно создать набор записей A/AAAA и сделать его набором записей псевдонимов, указывающим на ресурс общедоступного IP-адреса. Набор записей DNS изменяется автоматически при изменении или удалении общедоступного IP-адреса. Несвязанные DNS-записи, которые указывают на неправильные IP-адреса, не используются.

   Существует ограничение в 20 наборов записей псевдонимов на ресурс.

- **Выберите профиль диспетчера трафика из набора DNS-записей A/AAAA/CNAME**. Вы можете создать набор записей A/AAAA или CNAME и использовать записи псевдонимов, чтобы указать на профиль диспетчера трафика. Это особенно полезно, когда необходимо маршрутизировать трафик в зоне вершине, так как традиционные записи CNAME не поддерживаются для зоны вершине. Например, профиль диспетчера трафика — myprofile.trafficmanager.net, а зона DNS бизнеса — contoso.com. Вы можете создать набор записей типа A/AAAA (псевдоним) для contoso.com (вершина зоны) и указать на myprofile.trafficmanager.net.
- **Укажите конечную точку сети доставки содержимого (CDN) Azure**. Это полезно при создании статических веб-сайтов с помощью службы хранилища Azure и Azure CDN.
- **Выберите другой набор DNS-записей в пределах той же зоны**. Записи псевдонимов могут ссылаться на другие наборы записей одного типа. Например, набор записей DNS CNAME может быть псевдонимом для другого набора записей CNAME. Такой подход полезен в том случае, если вы хотите, чтобы некоторые наборы записей были псевдонимами, а некоторые ими не были.

## <a name="scenarios"></a>Сценарии

Существует несколько распространенных сценариев для записей псевдонимов.

### <a name="prevent-dangling-dns-records"></a>Предотвратить несвязанные DNS-записи

Одной из распространенных проблем с традиционными DNS-записями являются "несвязанные записи". Например, записи DNS, которые не были обновлены в соответствии с изменениями IP-адресов. Проблема возникает особенно часто с типами записей A/AAAA или CNAME.

В традиционной записи зоны DNS, если целевой IP-адрес или CNAME больше не существуют, запись DNS, связанную с ними, необходимо обновить вручную. В некоторых организациях ручное обновление может не происходить вовремя из-за проблем с процессами или разделения ролей и связанных уровней разрешений. Например, роль, имеющая права на удаление записи CNAME или IP-адрес приложения. Но она не имеет прав на обновление записи DNS, указывающей на эти целевые объекты. Задержка при обновлении записи DNS может стать причиной сбоя для пользователей.

Записи псевдонимов предотвращают несвязанные ссылки, тесно связывая жизненный цикл записи DNS с ресурсом Azure. Например, рассмотрим запись DNS, которая является псевдонимом и указывает на общедоступный IP-адрес или профиль диспетчера трафика. Если удалить эти базовые ресурсы, запись псевдонима DNS станет пустым набором записей. Он больше не ссылается на удаленный ресурс.

### <a name="update-dns-record-set-automatically-when-application-ip-addresses-change"></a>Автоматическое обновление набора записей DNS при изменении IP-адресов приложения

Этот сценарий похож на предыдущий. Возможно, приложение перемещается, или основная виртуальная машина перезапускается. При изменении IP-адреса для основного общедоступного IP-ресурса запись псевдонима обновляется. Это позволяет избежать потенциальных угроз безопасности, возникающих в случае направления пользователей к другому приложению, для которого назначен старый общедоступный IP-адрес.

### <a name="host-load-balanced-applications-at-the-zone-apex"></a>Размещайте приложения с балансировкой нагрузки в апексе зоны

Протокол DNS предотвращает назначение записей CNAME на вершине зоны. Например, если ваш домен — contoso.com; Вы можете создать записи CNAME для somelabel.contoso.com; но вы не можете создать запись CNAME для самого contoso.com.
Это ограничение создает проблемы владельцам приложений, которые размещают приложения с балансировкой нагрузки за [диспетчером трафика Azure](../traffic-manager/traffic-manager-overview.md). Поскольку для использования профиля диспетчера трафика требуется создать запись CNAME, невозможно указать профиль диспетчера трафика из зоны вершине.

Эта проблема решена с помощью записей псевдонимов. В отличие от записей CNAME, записи псевдонимов создаются в зоне вершине, и владельцы приложения могут использовать его для указания записи вершине зоны в профиль диспетчера трафика с внешними конечными точками. Владельцы приложений указывают на тот же профиль диспетчера трафика, который используется для любого другого домена в своей зоне DNS.

Например, contoso.com и www\.contoso.com могут указывать на один и тот же профиль диспетчера трафика. Дополнительные сведения об использовании записей псевдонимов с профилями диспетчера трафика Azure см. в разделе "Дополнительная информация".

### <a name="point-zone-apex-to-azure-cdn-endpoints"></a>Точка зоны вершине в конечные точки Azure CDN

Так же как и профиль диспетчера трафика, можно также использовать записи псевдонимов, чтобы указать вершине зоны DNS для Azure CDN конечных точек. Это полезно при создании статических веб-сайтов с помощью службы хранилища Azure и Azure CDN. После этого можно получить доступ к веб-сайту, не добавляя к DNS-имени префикс www.

Например, если статический веб-сайт называется www.contoso.com, пользователи могут получить доступ к сайту с помощью contoso.com, не требуя добавления www к DNS-имени.

Как было сказано ранее, записи CNAME не поддерживаются в зоне вершине. Поэтому нельзя использовать запись CNAME для указания contoso.com к конечной точке CDN. Вместо этого можно использовать запись псевдонима, чтобы напрямую указать зоне вершине для конечной точки CDN.

> [!NOTE]
> Указание зоны, вершине в конечные точки CDN для Azure CDN из Akamai, в настоящее время не поддерживается.

## <a name="next-steps"></a>Следующие шаги

Дополнительные сведения о записях псевдонима см. в следующих статьях:

- [Учебник. по настройке записи псевдонима для ссылки на общедоступный IP-адрес Azure](tutorial-alias-pip.md)
- [Учебник. Настройка записи псевдонима для поддержки вершинных доменных имен с помощью диспетчера трафика](tutorial-alias-tm.md)
- [DNS: часто задаваемые вопросы](https://docs.microsoft.com/azure/dns/dns-faq#alias-records)
