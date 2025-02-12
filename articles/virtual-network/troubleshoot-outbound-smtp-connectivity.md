---
title: Устранение проблем с исходящими подключениями SMTP в Azure | Документация Майкрософт
description: Узнайте, как устранять проблемы с исходящими подключениями SMTP в Azure.
services: virtual-network
author: genlin
manager: cshepard
editor: ''
ms.service: virtual-network
ms.devlang: na
ms.topic: troubleshooting
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/20/2018
ms.author: genli
ms.openlocfilehash: 13ed2dc2b304368e468c433b5abf5d056c33e406
ms.sourcegitcommit: c63e5031aed4992d5adf45639addcef07c166224
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/28/2019
ms.locfileid: "67466478"
---
# <a name="troubleshoot-outbound-smtp-connectivity-issues-in-azure"></a>Устранение проблем с исходящими подключениями SMTP в Azure

Начиная с 15 ноября 2017 г. исходящие электронные сообщения, которые отправлены непосредственно на внешние домены (например, outlook.com и gmail.com) с виртуальной машины, стали доступными только для определенных типов подписки в Microsoft Azure. Были заблокированы исходящие подключения SMTP, в которых используется TCP-порт 25. (Порт 25 в основном используется для доставки электронной почты, не прошедшей проверку подлинности.)

Это изменение в поведении применяется только к новым подпискам и новым развертываниям с 15 ноября 2017 г.

## <a name="recommended-method-of-sending-email"></a>Рекомендуемый метод отправки электронной почты
Рекомендуем использовать аутентифицированные службы ретрансляции SMTP (они обычно подключаются через TCP-порт 587 или 443, но поддерживают и другие порты) для отправки электронной почты с виртуальных машин Azure или из "Службы приложений Azure". Эти службы используются для поддержания репутации IP-адреса или домена, чтобы минимизировать вероятность того, что сторонние поставщики услуг электронной почты будут отклонять сообщения. Такие службы ретрансляции SMTP включают [SendGrid](https://sendgrid.com/partners/azure/). Вы можете также использовать безопасную службу ретрансляции SMTP, которая выполняется локально.

Использование этих служб доставки электронной почты не ограничено в Azure, каким бы ни был тип подписки.

## <a name="enterprise-agreement"></a>Соглашение Enterprise
Для пользователей Azure, заключивших Соглашение Enterprise, никак не изменяется техническая возможность отправлять электронные сообщения без использования аутентифицированной ретрансляции. Новые и существующие пользователи, заключившие Соглашение Enterprise, могут пытаться доставлять исходящие электронные сообщения с виртуальных машин Azure непосредственно внешним поставщикам услуг электронной почты без каких-либо ограничений со стороны платформы Azure. Хотя не гарантируется, что поставщики услуг электронной почты будут принимать входящие электронные сообщения от любого пользователя, попытки доставки не будут заблокированы платформой Azure для виртуальных машин в подписках с Соглашением Enterprise. Вам нужно будет решать непосредственно с поставщиками услуг электронной почты вопросы доставки сообщений или фильтрации нежелательной почты, затрагивающие конкретных поставщиков услуг.

## <a name="pay-as-you-go"></a>Оплата по мере использования
Если вы до 15 ноября 2017 г. зарегистрировались для оплаты по мере использования или оформили подписку Microsoft Partner Network, для вас не будет никаких изменений, касающихся технической возможности попробовать доставку исходящих электронных сообщений. Вы по-прежнему сможете пытаться доставлять исходящие электронные сообщения с виртуальных машин Azure в рамках этих подписок непосредственно внешним поставщикам услуг электронной почты без каких-либо ограничений со стороны платформы Azure. Не гарантируется, что поставщики услуг электронной почты будет принимать входящие электронные сообщения от любого пользователя. Пользователям нужно будет решать непосредственно с такими поставщиками вопросы доставки сообщений или фильтрации нежелательной почты, затрагивающие конкретных поставщиков услуг.

Для подписок с оплатой по мере использования или подписок Microsoft Partner Network, созданных после 15 ноября 2017 г., будут применяться технические ограничения. Электронные сообщения, отправленные непосредственно с виртуальных машин в рамках этих подписок, будут блокироваться. Если нужна возможность отправлять электронные сообщения с виртуальных машин Azure непосредственно внешним поставщикам услуг электронной почты (без использования аутентифицированной ретрансляции SMTP), можно сделать запрос на снятие ограничения. Запросы проверяет и одобряет только корпорация Майкрософт. Они будут утверждены только после дополнительных проверок защиты от мошенничества. Для создания запроса, откройте обращение в службу поддержки, используя следующий тип проблемы: **Управление подпиской** тип проблемы: **При запросе на включение потока электронной почты порт 25**. Убедитесь, что вы добавили сведения о том, почему при развертывании нужно отправлять почту непосредственно поставщикам услуг электронной почты, а не использовать аутентифицированную ретрансляцию.

После оплаты по мере использования или Microsoft Partner Network подписки будет создано и виртуальные машины были «Остановлено» & «Запущено» на портале Azure, все виртуальные машины в рамках этой подписки будет исключен в дальнейшем. Исключение применяется только к подписке, в запросе.

> [!NOTE]
> Корпорация Майкрософт оставляет за собой право отменить это исключение, если выяснится, что произошло нарушение условий использования службы.

## <a name="msdn-azure-pass-azure-in-open-education-bizspark-and-free-trial"></a>MSDN, Azure Pass, Azure с открытой лицензией, для образования, BizSpark и бесплатная пробная версия
Если вы создали любую подписку MSDN, Azure Pass, Azure с открытой лицензией, для образования, BizSpark, спонсорское предложение Azure, Azure для учащихся, бесплатную пробную версию или любую подписку Visual Studio после 15 ноября 2017 г., на вас будут распространяться технические ограничения, которые блокируют электронные сообщения, отправляемые с виртуальных машин в рамках этих подписок непосредственно поставщикам услуг электронной почты. Ограничения применяются во избежание злоупотребления. Запросы на удаление этого ограничения не будут выполнены.

При использовании этих типов подписки, настоятельно рекомендуется использовать службы ретрансляции SMTP, как описано ранее в этой статье или изменить тип подписки.

## <a name="cloud-service-provider-csp"></a>Поставщик облачных служб (CSP)

Если вы используете ресурсы Azure с помощью CSP, вы можете запросить CSP может создать запрос на исключение unblock с корпорацией Майкрософт от вашего имени безопасной ретрансляции SMTP не может быть использовано.

## <a name="need-help-contact-support"></a>Требуется помощь? Обращение в службу поддержки

Если вам еще нужна помощь, [обратитесь в службу поддержки](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) которая поможет быстро устранить с помощью следующих типа проблемы проблему: **Управление подпиской** тип проблемы: **При запросе на включение потока электронной почты порт 25**.
