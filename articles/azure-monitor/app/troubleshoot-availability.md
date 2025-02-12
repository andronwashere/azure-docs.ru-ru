---
title: Устранение неполадок тестов доступности Azure Application Insights | Документация Майкрософт
description: Устранение неполадок веб-тестов в Azure Application Insights. Получение оповещений, когда веб-сайт становится недоступным или медленно реагирует на запросы.
services: application-insights
documentationcenter: ''
author: lgayhardt
manager: carmonm
ms.assetid: 46dc13b4-eb2e-4142-a21c-94a156f760ee
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 06/19/2019
ms.reviewer: sdash
ms.author: lagayhar
ms.openlocfilehash: 87bc87d7d105d581f0143e87044fb0337c0fd7f6
ms.sourcegitcommit: 82efacfaffbb051ab6dc73d9fe78c74f96f549c2
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/20/2019
ms.locfileid: "67305106"
---
# <a name="troubleshooting"></a>Устранение неполадок

Эта статья поможет вам устранить распространенные проблемы, которые могут возникнуть при использовании отслеживания доступности.

## <a name="ssltls-errors"></a>Ошибки SSL/TLS

|Признак или сообщение об ошибке| Возможные причины|
|--------|------|
|Не удалось создать безопасный канал SSL/TLS  | Версия протокола SSL. Поддерживаются только TLS 1.0, 1.1 и 1.2. **SSLv3 не поддерживается.**
|Слой записей TLSv1.2. Оповещения (уровень: Критическая ошибка, описание: Неверную запись MAC)| См. в разделе StackExchange поток для [сведения](https://security.stackexchange.com/questions/39844/getting-ssl-alert-write-fatal-bad-record-mac-during-openssl-handshake).
|URL-адрес, происходит сбой — CDN (сеть доставки содержимого) | Это может быть вызвано неправильной настройкой в вашей сети доставки Содержимого |  

### <a name="possible-workaround"></a>Возможное решение

* Если URL-адрес, на которых наблюдаются проблемы всегда для зависимых ресурсов, рекомендуется отключить **разбирать зависимые запросы** веб-теста.

## <a name="test-fails-only-from-certain-locations"></a>Тест завершается с ошибкой только из определенных расположений

|Признак или сообщение об ошибке| Возможные причины|
|----|---------|
|Попытка подключения не удалось, так как подключенная сторона не ответила правильно после определенного периода времени  | Агенты тестирования в определенных местах блокируются брандмауэром.|
|    |Изменение маршрутизации определенных IP-адресов происходит через (подсистемы балансировки нагрузки, диспетчеры трафика Geo, Azure Express Route.) 
|    |Если с помощью Azure ExpressRoute, существуют сценарии, где пакеты можно удалить в случаях, где [асимметричная Маршрутизация происходит](https://docs.microsoft.com/azure/expressroute/expressroute-asymmetric-routing).|

## <a name="intermittent-test-failure-with-a-protocol-violation-error"></a>Периодические сбои тестирования с ошибкой о нарушении протокола

|Признак или сообщение об ошибке| Возможные причины|
|----|---------|
CR нарушение протокола должен следовать перевод строки | Это происходит при обнаружении неправильно сформированные заголовки. В частности некоторые заголовки могут не использовать CRLF для указания конец строки, что нарушает спецификацию протокола HTTP и поэтому не пройдет проверку на уровне .NET WebRequest.
 || Это может также быть вызвано подсистемами балансировки нагрузки или сети доставки содержимого.

> [!NOTE]
> При переходе по URL-адресу в браузерах, для которых установлена упрощенная проверка заголовков HTTP, может не произойти сбой. См. подробное описание проблемы в этой записи блога: http://mehdi.me/a-tale-of-debugging-the-linkedin-api-net-and-http-protocol-violations/  

## <a name="common-troubleshooting-questions"></a>Общие вопросы об устранении неисправностей

### <a name="site-looks-okay-but-i-see-test-failures-why-is-application-insights-alerting-me"></a>Сайт выглядит правильно, но я вижу сбои при тестировании? Почему Application Insights предупреждений мне?

   * У этого тест **разбирать зависимые запросы** включена? Он приводит к строгой проверке таких ресурсов, как скрипты, изображения и т. д. Эти типы сбоев могут быть незаметны в браузере. Проверьте все изображения, скрипты, таблицы стилей и любые другие файлы, загружаемые на страницу. Если любой из них в случае сбоя теста, отчет о, даже если основная HTML-страница загружается без проблем. Чтобы не зависело от таких сбоев ресурсов теста, просто снимите флажок разбирать зависимые запросы из конфигурации теста.

   * Чтобы уменьшить вероятность возникновения помех из нестабильной работе временные сети и т.д., убедитесь, включить повторные попытки для сбоев тестов проверяется в конфигурации. Вы также можете выполнять тестирование из любого расположения и соответствующим образом управлять пороговым значением правила генерации оповещений для предотвращения проблем с конкретными расположениями, вызывающих ненужные оповещения.

   * Щелкните любую из красных точек в интерфейсе доступности или любой сбой доступности в проводнике поиска для просмотра сведений о том, почему мы сообщили о сбое. Результат теста, а также коррелированные данные телеметрии на стороне сервера (если они включены) должны помочь понять, почему тест завершился ошибкой. Распространенные причины временных неполадок — проблемы сети или подключения.

   * Истекло ли время ожидания теста? Мы прерываем тесты через две минуты. Если ваша проверка связи или многошаговый тест занимает более двух минут, мы воспринимаем это как сбой. Разбейте тест на несколько сеансов, которые можно выполнить последовательно.

   * Все ли расположения сообщили о сбоях? Если только некоторые их них прислали отчет о сбое, возможно, причина в проблемах с сетью или CDN. Щелкая красные точки, вы можете понять, почему расположение сообщило о сбое.

### <a name="i-did-not-get-an-email-when-the-alert-triggered-or-resolved-or-both"></a>Я не получил сообщение электронной почты при активации оповещения и разрешенные или оба?

Проверьте конфигурацию классических оповещений, чтобы убедиться, что ваш адрес электронной почты явно указан, или список рассылки, в который вы входите, настроен для получения уведомлений. Если это так, проверьте конфигурацию списка рассылки, чтобы убедиться, что он может получать внешние сообщения электронной почты. Проверьте также, не настроил ли администратор электронной почты политики, которые могут вызвать эту проблему.

### <a name="i-did-not-receive-the-webhook-notification"></a>Я не получал уведомления веб-перехватчика?

Убедитесь, что приложение, получающее уведомления от веб-перехватчика, доступно и успешно обрабатывает запросы веб-перехватчика. Дополнительную информацию см. [здесь](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitor-alerts-unified-log-webhook).

### <a name="intermittent-test-failure-with-a-protocol-violation-error"></a>Периодические сбои тестирования с ошибкой о нарушении протокола?

Ошибка ("нарушение протокола...За возвратом каретки должен следовать перевод строки") свидетельствует о возможной неполадке с сервером (или зависимостями). Такое происходит, когда в ответе задаются неправильно сформированные заголовки. Это может быть вызвано подсистемами балансировки нагрузки или сетями доставки содержимого (CDN). В частности некоторые заголовки могут не использовать CRLF для указания конца строки, что нарушает спецификацию протокола HTTP и таким образом, не проходят проверку на уровне .NET WebRequest. Изучите ответ, заголовки, содержащие, которые могут быть неправильными.

> [!NOTE]
> При переходе по URL-адресу в браузерах, для которых установлена упрощенная проверка заголовков HTTP, может не произойти сбой. См. подробное описание проблемы в этой записи блога: http://mehdi.me/a-tale-of-debugging-the-linkedin-api-net-and-http-protocol-violations/  

### <a name="i-dont-see-any-related-server-side-telemetry-to-diagnose-test-failures"></a>Я не вижу, все связанные серверные данные телеметрии для диагностики сбоев тестов? *

Если служба Application Insights настроена для серверного приложения, возможно, это произошло из-за [выборки](../../azure-monitor/app/sampling.md) в операции. Выберите другой результат доступности.

### <a name="can-i-call-code-from-my-web-test"></a>Можно ли вызывать код из моего веб-теста?

№ Действия теста должны находиться в WEBTEST-файле. Кроме того, нельзя вызывать другие веб-тесты и использовать циклы. Но есть несколько подключаемых модулей, которые могут оказаться полезными.


### <a name="is-there-a-difference-between-web-tests-and-availability-tests"></a>Есть разница между «веб-тесты» и «тесты доступности»?

Эти два термина могут быть взаимозаменяемыми. Тест доступности является более универсальным термином, так как кроме многошаговых веб-тестов вам доступны проверка связи с отдельным URL-адресом.

### <a name="id-like-to-use-availability-tests-on-our-internal-server-that-runs-behind-a-firewall"></a>Я бы хотел использовать тесты доступности на нашем внутреннем сервере, который работает за брандмауэром.

   Есть два возможных решения:

   * Разрешите в брандмауэре входящие запросы с [IP-адресов агентов веб-тестирования](../../azure-monitor/app/ip-addresses.md).
   * Напишите собственный код для периодической проверки внутреннего сервера. Запустите код в виде фонового процесса на тестовом сервере за брандмауэром. Процесс тестирования может отправлять результаты в Application Insights с помощью API [TrackAvailability()](https://docs.microsoft.com/dotnet/api/microsoft.applicationinsights.telemetryclient.trackavailability) в основном пакете SDK. Для этого тестовый сервер должен иметь исходящий доступ к конечной точке приема в Application Insights, что представляет гораздо меньшую угрозу безопасности, чем разрешение входящих запросов. Результаты не будут отображаться в колонках веб-тестов доступности, но будут отображаться как результаты в обозревателе метрик, поиска и анализа.

### <a name="uploading-a-multi-step-web-test-fails"></a>Отправка Многошаговый веб-тест завершается сбоем

Некоторые причины, по которым он может произойти:
   * Максимальный размер — 300 000.
   * Циклы не поддерживаются.
   * Ссылки на другие веб-тесты не поддерживаются.
   * Источники данных не поддерживаются.

### <a name="my-multi-step-test-doesnt-complete"></a>Мой Многошаговый тест не выполняется.

Максимальное количество запросов в тесте — 100. Кроме того, тест будет остановлен, если он выполняется дольше двух минут.

### <a name="how-can-i-run-a-test-with-client-certificates"></a>Как запустить тест с клиентскими сертификатами?

В настоящее время это не поддерживается.

## <a name="who-receives-the-classic-alert-notifications"></a>Кто получает (классические) оповещения?

Этот раздел относится только к классическим оповещениям, он поможет вам оптимизировать уведомления так, чтобы они приходили только желаемым получателям. Чтобы узнать больше о различиях между [классическими оповещениями](../platform/alerts-classic.overview.md) и новыми возможностями оповещений, ознакомьтесь с [обзорной статьей об оповещениях](../platform/alerts-overview.md). Чтобы управлять оповещениями в новом интерфейсе оповещений, используйте [группы действий](../platform/action-groups.md).

* Мы рекомендуем использовать определенных получателей для классических оповещений.

* Если флажок **bulk/group** (массовый/групповой) включен, оповещения о сбоях из X из Y расположений будут оправляться пользователям с ролями администратора или соадминистратора.  По сути, _все_ администраторы _подписки_ будут получать уведомления.

* Для оповещений о метриках доступности **массового или группу** "флажок" параметр, если параметр включен, отправляет пользователям с ролями владельца, участника или читателя в подписке. Фактически, _все_ пользователи, имеющие доступ к подписке Application Insights, находятся в области действия и будут получать уведомления. 

> [!NOTE]
> При отключении используемого флажка **bulk/group** (массовый/груповой) вы не сможете отменить изменение.

Используйте новые возможности оповещений и оповещения почти в реальном времени, если вам нужно уведомлять пользователей в зависимости от их ролей. С помощью [групп действий](../platform/action-groups.md) вы можете настроить уведомления по электронной почте для пользователей с любой из ролей — участника, владельца или читателя (не объединяемых вместе как один параметр).

## <a name="next-steps"></a>Дальнейшие действия

* [Многошаговый веб-тестирование](availability-multistep.md)
* [URL-адрес проверок связи](monitor-web-app-availability.md)
