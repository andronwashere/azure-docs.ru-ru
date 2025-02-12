---
title: Часто задаваемые вопросы о Службах мультимедиа Azure версии 3 | Документация Майкрософт
description: В этой статье предоставлены ответы на часто задаваемые вопросы о Службах мультимедиа Azure версии 3
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 06/21/2019
ms.author: juliako
ms.openlocfilehash: 28b9c8f343437c20e277d2f3ba53767afa45a5c2
ms.sourcegitcommit: a0b37e18b8823025e64427c26fae9fb7a3fe355a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/25/2019
ms.locfileid: "68501257"
---
# <a name="media-services-v3-frequently-asked-questions"></a>Часто задаваемые вопросы о службах мультимедиа v3

В этой статье предоставлены ответы на часто задаваемые вопросы о Службах мультимедиа Azure (AMS) версии 3.

## <a name="general"></a>Общее

### <a name="what-azure-roles-can-perform-actions-on-azure-media-services-resources"></a>Какие роли Azure могут выполнять действия с ресурсами служб мультимедиа Azure? 

См. раздел [Управление доступом на основе ролей (RBAC) для учетных записей служб мультимедиа](rbac-overview.md).

### <a name="how-do-i-configure-media-reserved-units"></a>Как настроить зарезервированные единицы мультимедиа?

Для заданий анализа аудио и видео, которые активируют Службы мультимедиа версии 3 или Индексатор видео, настоятельно рекомендуется подготовить к работе вашу учетную запись с помощью 10 единиц MRU S3. Если вам требуется более 10 единиц MRU S3, отправьте запрос в службу поддержки с помощью [портала Azure](https://portal.azure.com/).

Дополнительные сведения см. в разделе [Масштабирование при обработке мультимедиа](media-reserved-units-cli-how-to.md).

### <a name="what-is-the-recommended-method-to-process-videos"></a>Что такое рекомендуемый метод для обработки видео?

[Преобразования](https://docs.microsoft.com/rest/api/media/transforms) можно использовать для настройки общих задач кодирования или анализа видеоматериалов. Каждое **преобразование** описывает набор инструкций (или рабочий процесс задач) для обработки видео- и аудиофайлов. [Задание](https://docs.microsoft.com/rest/api/media/jobs) — это фактический запрос к службам мультимедиа для применения **преобразования** к заданному входному видео или звуковому содержимому. После создания преобразования можно отправлять задания с помощью API Служб мультимедиа или любого из опубликованных пакетов SDK. Дополнительные сведения см. в статье [Преобразования и задания](transforms-jobs-concept.md).

### <a name="how-does-pagination-work"></a>Как работает разбиение на страницы?

При использовании разбиения на страницы для перечисления коллекции всегда нужно использовать следующую ссылку, которая не зависит от конкретного размера страницы. Дополнительные сведения и примеры см. в [этой статье о фильтрации, упорядочении и разбиении на страницы](entities-overview.md).

### <a name="what-features-are-not-yet-available-in-azure-media-services-v3"></a>Какие функции пока недоступны в службах мультимедиа Azure v3?

Дополнительные сведения см. [в разделе зазоры функций в отношении API v2](migrate-from-v2-to-v3.md#feature-gaps-with-respect-to-v2-apis).

### <a name="what-is-the-process-of-moving-a-media-services-account-between-subscriptions"></a>В чем заключается процесс перемещения учетной записи служб мультимедиа между подписками?  

Дополнительные сведения см. [в статье Перемещение учетной записи служб мультимедиа между подписками](media-services-account-concept.md).

## <a name="live-streaming"></a>Потоковая трансляция 

###  <a name="how-to-insert-breaksvideos-and-image-slates-during-live-stream"></a>Как вставлять паузы, видео или баннеры во время передачи динамического потока?

Функция кодирования в реальном времени в Службах мультимедиа версии 3 не поддерживает вставку видео или баннеров во время передачи динамического потока. 

Можно использовать [локальный динамический кодировщик](recommended-on-premises-live-encoders.md), чтобы переключать исходное видео. Многие приложения дают возможность переключения источников, включая Telestream Wirecast, Switcher Studio (в iOS), OBS Studio (бесплатное приложение) и многие другие.

## <a name="content-protection"></a>Защита содержимого

### <a name="should-i-use-an-aes-128-clear-key-encryption-or-a-drm-system"></a>Следует ли использовать шифрование AES-128 с открытым ключом или систему DRM?

Клиенты часто интересуются, следует ли им использовать шифрование AES или систему DRM. Основное различие между двумя системами заключается в том, что при шифровании AES ключ содержимого передается клиенту по протоколу TLS, чтобы ключ был зашифрован при передаче, но без дополнительного шифрования ("в очистке"). В результате ключ, используемый для расшифровки содержимого, доступен клиентскому проигрывателю, и его можно просмотреть в сетевой трассировке на клиенте в виде обычного текста. Шифрование AES-128 с открытым ключом подходит для случаев, когда средство просмотра является доверенной стороной (например, шифрование корпоративных видеоматериалов, распространяемых в компании для просмотра сотрудниками).

Системы DRM, такие как PlayReady, Widevine и FairPlay, обеспечивают дополнительный уровень шифрования ключа, используемого для расшифровки содержимого по сравнению с ключом AES-128 Clear. Ключ содержимого шифруется в ключе, защищенном средой выполнения DRM, дополнительно к любому шифрованию транспортного уровня, обеспечиваемому протоколом TLS. Кроме того, расшифровка выполняется в защищенной среде на уровне операционной системы, где злоумышленнику труднее атаковать. Мы рекомендуем использовать DRM в случаях, когда зритель не может быть доверенным лицом, и вам нужен самый высокий уровень безопасности.

### <a name="how-and-where-to-get-jwt-token-before-using-it-to-request-license-or-key"></a>Где и как получить токен JWT, чтобы использовать его для запроса лицензии или ключа?

1. Для рабочей среды необходимо иметь службы маркеров безопасности (STS) (веб-служба), которая выдает маркер JWT при запросе HTTPS. Для тестирования можно использовать код, показанный в методе **GetTokenAsync**, который определен в файле [Program.cs](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/blob/master/AMSV3Tutorials/EncryptWithDRM/Program.cs).
2. После выполнения проверки подлинности пользователя проигрывателю необходимо будет выполнить запрос к службе безопасности токенов на получение такого токена и присвоить его как значение токена. Можно использовать [API решения "Проигрыватель мультимедиа Azure"](https://amp.azure.net/libs/amp/latest/docs/).

* Пример запуска службы безопасности токенов с симметричным и асимметричным ключом см. на странице [https://aka.ms/jwt](https://aka.ms/jwt). 
* Пример проигрывателя на базе Проигрывателя мультимедиа Azure, в котором используется такой токен JWT, см. на странице [https://aka.ms/amtest](https://aka.ms/amtest) (разверните ссылку player_settings, чтобы увидеть входные данные токена).

### <a name="how-do-you-authorize-requests-to-stream-videos-with-aes-encryption"></a>Как вы авторизуете запросы на потоковую передачу видео с помощью шифрования AES?

Правильный подход заключается в использовании STS (службы токенов безопасности).

В службе токенов безопасности, в зависимости от профиля пользователя, добавьте разные утверждения (например, "Пользователь (ценовая категория "Премиум")", "Пользователь (ценовая категория "Базовый")", "Пользователь бесплатной пробной версии"). С различными утверждениями в JWT пользователь может видеть разное содержимое. Конечно, для разного содержимого или ресурса параметр ContentKeyPolicyRestriction будет иметь соответствующие утверждения RequiredClaims.

Используйте API служб мультимедиа Azure для настройки доставки лицензий и ключей и шифрования ресурсов (как показано в [этом примере](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/blob/master/AMSV3Tutorials/EncryptWithAES/Program.cs)).

Дополнительные сведения можно найти в разделе

- [Обзор системы защиты содержимого](content-protection-overview.md)
- [Проектирование системы для защиты содержимого с несколькими подсистемами DRM и управлением доступом](design-multi-drm-system-with-access-control.md)

### <a name="http-or-https"></a>HTTP или HTTPS?
Приложение проигрывателя ASP.NET MVC, которое мы разработали, должно поддерживать следующее:

* Проверку подлинности пользователя через Azure AD, который должен находиться в разделе HTTPS.
* Обмен маркерами JWT между клиентом и Azure AD, который находится в разделе HTTPS.
* Приобретение лицензий DRM клиентом, который должен находиться в разделе HTTPS, если доставка лицензий осуществляется Службами мультимедиа. Набор продуктов PlayReady не требует HTTPS для доставки лицензий. Если ваш сервер лицензирования PlayReady находится за пределами Служб мультимедиа, можно использовать HTTP или HTTPS.

Приложение проигрывателя ASP.NET использует рекомендуемый протокол HTTPS, поэтому проигрыватель находится на странице в разделе HTTPS. Тем не менее, для потоковой передачи предпочтительно использовать HTTP, поэтому следует обратить внимание на проблему смешанного содержимого.

* Браузер запрещает смешанное содержимое. Однако подключаемые модули, например Silverlight и OSMF для бесперебойной работы и DASH, разрешают. Смешанное содержимое является угрозой безопасности — это происходит из-за возможности вставить вредоносные JavaScript, что может подвергнуть риску данные клиента. Браузеры по умолчанию блокируют эту возможность. На данный момент единственным способом обойти это на стороне сервера (источника) — разрешить все домены (и HTTPS, и HTTP). Это, скорее всего, тоже не самая лучшая идея.
* Не следует использовать смешанное содержимое. Приложение проигрывателя и Проигрыватель мультимедиа должны использовать HTTP или HTTPS. При воспроизведении смешанного содержимого технология silverlightSS требует удаления предупреждения о смешанном содержимом. Технология flashSS обрабатывает смешанное содержимое без предупреждения о смешанном содержимом.
* Если конечная точка потоковой передачи была создана до августа 2014 г., она не будет поддерживать протокол HTTPS. В этом случае создайте и используйте новую конечную точку потоковой передачи для использования протокола HTTPS.

### <a name="what-about-live-streaming"></a>Как насчет динамической потоковой передачи?

Вы можете использовать точно такую же структуру и реализацию для защиты динамической потоковой передачи в службах мультимедиа, рассматривая ресурс, связанный с программой, как ресурс VOD. Чтобы обеспечить защиту динамического содержимого с помощью нескольких DRM, примените ту же настройку и обработку к ресурсу, как если бы он был ресурсом VOD, прежде чем связать ресурс с выходом в реальном времени.

### <a name="what-about-license-servers-outside-media-services"></a>Как насчет серверов лицензирования за пределами служб мультимедиа?

Часто пользователи могли вложить средства в ферму серверов лицензирования в собственном центре обработки данных или у поставщика услуг DRM. Система защиты содержимого Служб мультимедиа дает возможность работы в гибридном режиме. Содержимое размещается и динамически защищается в Службах мультимедиа, в то же время лицензии DRM доставляются серверами за пределами Служб мультимедиа. В этом случае обратите внимание на следующие изменения:

* Служба маркеров безопасности должна выдавать маркеры, которые являются допустимыми и могут быть проверены на ферме серверов лицензий. Например, серверы лицензий Widevine, предоставляемые Axinom, требуют определенного маркера JWT, который содержит сообщение о правах. Таким образом, необходимо иметь STS для издания такого маркера JWT. 
* Настройка службы доставки лицензий в Службах мультимедиа больше не требуется. Вам нужно предоставить URL-адреса приобретения лицензии (для PlayReady, Widevine и FairPlay) при настройке ContentKeyPolicies.

## <a name="media-services-v2-vs-v3"></a>Сравнение версий 2 и 3 Служб мультимедиа 

### <a name="can-i-use-the-azure-portal-to-manage-v3-resources"></a>Могу ли я использовать портал Azure для управления ресурсами версии 3?

В настоящее время вы не можете использовать портал Azure для управления ресурсами версии 3. Используйте [REST API](https://aka.ms/ams-v3-rest-ref), [CLI](https://aka.ms/ams-v3-cli-ref) или один из поддерживаемых [пакетов SDK](media-services-apis-overview.md#sdks).

### <a name="is-there-an-assetfile-concept-in-v3"></a>Существует элемент AssetFile в версии 3?

Элементы AssetFiles были удалены из API AMS, чтобы отделить Службы мультимедиа от зависимости пакета SDK службы хранилища. Теперь служба хранилища, а не службы мультимедиа, сохраняет данные, которые относятся к службе хранилища. 

Дополнительные сведения см. в разделе [Руководство по миграции из версии 2 в версию 3 Служб мультимедиа](migrate-from-v2-to-v3.md).

### <a name="where-did-client-side-storage-encryption-go"></a>Где происходит шифрование хранилища на стороне клиента?

Сейчас рекомендуется использовать шифрование хранилища на стороне сервера (которое включено по умолчанию). Дополнительные сведения см. в статье [Шифрование службы хранилища Azure для неактивных данных (предварительная версия)](https://docs.microsoft.com/azure/storage/common/storage-service-encryption).

## <a name="next-steps"></a>Следующие шаги

[Что такое Службы мультимедиа Azure версии 3?](media-services-overview.md)
