---
title: Использование концентраторов уведомлений с PHP
description: Узнайте, как использовать Центры уведомлений Azure из серверной части PHP.
services: notification-hubs
documentationcenter: ''
author: jwargo
manager: patniko
editor: spelluru
ms.assetid: 0156f994-96d0-4878-b07b-49b7be4fd856
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: php
ms.devlang: php
ms.topic: article
ms.date: 01/04/2019
ms.author: jowargo
ms.openlocfilehash: 054edaf321d90015840fd84e1697fca742fd7e1e
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "60872189"
---
# <a name="how-to-use-notification-hubs-from-php"></a>Использование концентраторов уведомлений из PHP

[!INCLUDE [notification-hubs-backend-how-to-selector](../../includes/notification-hubs-backend-how-to-selector.md)]

Вы можете обращаться ко всем функциям концентраторов уведомлений из серверной части Java/PHP/Ruby, используя интерфейс REST Центра уведомлений в соответствии с описанием в статье MSDN [Интерфейсы API REST концентраторов уведомлений](https://msdn.microsoft.com/library/dn223264.aspx).

В этом разделе описывается:

* построение клиента REST для функций концентраторов уведомлений на PHP;
* использование статьи по началу работы [Руководство по отправке push-уведомлений в приложения iOS с помощью Центров уведомлений Azure](notification-hubs-ios-apple-push-notification-apns-get-started.md) для выбранной мобильной платформы, реализация серверной части на PHP.

## <a name="client-interface"></a>Интерфейс клиента

Основной интерфейс клиента может предоставлять те же методы, которые доступны в [пакете SDK службы "Центры уведомлений" для .NET](https://msdn.microsoft.com/library/jj933431.aspx). Это позволяет напрямую переводить все руководства и примеры, доступные на этом сайте в настоящий момент и пополняемые интернет-сообществом.

Весь доступный код находится в [Примере оболочки PHP REST].

Например, чтобы создать клиента, необходимо выполнить следующие действия.

    ```php
    $hub = new NotificationHub("connection string", "hubname");
    ```

Отправка собственного уведомления iOS.

    ```php
    $notification = new Notification("apple", '{"aps":{"alert": "Hello!"}}');
    $hub->sendNotification($notification, null);
    ```

## <a name="implementation"></a>Реализация

Если вы еще этого не делали, выполните шаги, описанные в статье [Учебника по началу работы], до последнего раздела, в котором вам нужно реализовать серверную часть.
Кроме того, при необходимости можно воспользоваться кодом из [Примере оболочки PHP REST] и перейти непосредственно к разделу [Завершение работы с руководством](#complete-tutorial).

Подробные сведения о реализации полноценной оболочки REST можно найти в [MSDN](https://msdn.microsoft.com/library/dn530746.aspx). В этом разделе описывается реализация основных действий на PHP, необходимых для доступа к конечным точкам REST службы "Центры уведомлений".

1. Проанализируйте строку подключения
2. Создайте маркер проверки подлинности
3. Выполните вызов HTTP

### <a name="parse-the-connection-string"></a>Проанализируйте строку подключения

Ниже показан основной класс, реализующий клиента, конструктор которого выполняет анализ строки подключения:

    ```php
    class NotificationHub {
        const API_VERSION = "?api-version=2013-10";

        private $endpoint;
        private $hubPath;
        private $sasKeyName;
        private $sasKeyValue;

        function __construct($connectionString, $hubPath) {
            $this->hubPath = $hubPath;

            $this->parseConnectionString($connectionString);
        }

        private function parseConnectionString($connectionString) {
            $parts = explode(";", $connectionString);
            if (sizeof($parts) != 3) {
                throw new Exception("Error parsing connection string: " . $connectionString);
            }

            foreach ($parts as $part) {
                if (strpos($part, "Endpoint") === 0) {
                    $this->endpoint = "https" . substr($part, 11);
                } else if (strpos($part, "SharedAccessKeyName") === 0) {
                    $this->sasKeyName = substr($part, 20);
                } else if (strpos($part, "SharedAccessKey") === 0) {
                    $this->sasKeyValue = substr($part, 16);
                }
            }
        }
    }
    ```

### <a name="create-a-security-token"></a>Создание маркера безопасности

В документации Azure содержатся сведения о том, как [создавать маркер безопасности SAS](https://docs.microsoft.com/previous-versions/azure/reference/dn495627(v=azure.100)#create-sas-security-token).

Для создания маркера на основе универсального кода ресурса (URI) текущего запроса и учетных данных, извлеченных из строки подключения, добавьте метод `generateSasToken` в класс `NotificationHub`.

    ```php
    private function generateSasToken($uri) {
        $targetUri = strtolower(rawurlencode(strtolower($uri)));

        $expires = time();
        $expiresInMins = 60;
        $expires = $expires + $expiresInMins * 60;
        $toSign = $targetUri . "\n" . $expires;

        $signature = rawurlencode(base64_encode(hash_hmac('sha256', $toSign, $this->sasKeyValue, TRUE)));

        $token = "SharedAccessSignature sr=" . $targetUri . "&sig="
                    . $signature . "&se=" . $expires . "&skn=" . $this->sasKeyName;

        return $token;
    }
    ```

### <a name="send-a-notification"></a>Отправка уведомления

Сначала следует определить класс, представляющий уведомление.

    ```php
    class Notification {
        public $format;
        public $payload;

        # array with keynames for headers
        # Note: Some headers are mandatory: Windows: X-WNS-Type, WindowsPhone: X-NotificationType
        # Note: For Apple you can set Expiry with header: ServiceBusNotification-ApnsExpiry in W3C DTF, YYYY-MM-DDThh:mmTZD (for example, 1997-07-16T19:20+01:00).
        public $headers;

        function __construct($format, $payload) {
            if (!in_array($format, ["template", "apple", "windows", "fcm", "windowsphone"])) {
                throw new Exception('Invalid format: ' . $format);
            }

            $this->format = $format;
            $this->payload = $payload;
        }
    }
    ```

Этот класс представляет собой контейнер для текста собственного уведомления либо набор свойств, в случае с шаблонным уведомлением, а также набор заголовков, содержащих свойства формата (собственная платформа или шаблон) и специальные свойства платформы (например, свойство срока действия Apple и заголовки WNS).

Обратитесь к [документации по REST API для службы "Центры уведомлений"](https://msdn.microsoft.com/library/dn495827.aspx) и изучите форматы специализированных платформ уведомлений, чтобы узнать обо всех доступных параметрах.

Имея этот класс, мы можем создавать методы отправки уведомлений внутри класса `NotificationHub`.

    ```php
    public function sendNotification($notification, $tagsOrTagExpression="") {
        if (is_array($tagsOrTagExpression)) {
            $tagExpression = implode(" || ", $tagsOrTagExpression);
        } else {
            $tagExpression = $tagsOrTagExpression;
        }

        # build uri
        $uri = $this->endpoint . $this->hubPath . "/messages" . NotificationHub::API_VERSION;
        $ch = curl_init($uri);

        if (in_array($notification->format, ["template", "apple", "fcm"])) {
            $contentType = "application/json";
        } else {
            $contentType = "application/xml";
        }

        $token = $this->generateSasToken($uri);

        $headers = [
            'Authorization: '.$token,
            'Content-Type: '.$contentType,
            'ServiceBusNotification-Format: '.$notification->format
        ];

        if ("" !== $tagExpression) {
            $headers[] = 'ServiceBusNotification-Tags: '.$tagExpression;
        }

        # add headers for other platforms
        if (is_array($notification->headers)) {
            $headers = array_merge($headers, $notification->headers);
        }

        curl_setopt_array($ch, array(
            CURLOPT_POST => TRUE,
            CURLOPT_RETURNTRANSFER => TRUE,
            CURLOPT_SSL_VERIFYPEER => FALSE,
            CURLOPT_HTTPHEADER => $headers,
            CURLOPT_POSTFIELDS => $notification->payload
        ));

        // Send the request
        $response = curl_exec($ch);

        // Check for errors
        if($response === FALSE){
            throw new Exception(curl_error($ch));
        }

        $info = curl_getinfo($ch);

        if ($info['http_code'] <> 201) {
            throw new Exception('Error sending notification: '. $info['http_code'] . ' msg: ' . $response);
        }
    } 
    ```

Указанные выше методы отправляют запрос HTTP POST в конечную точку `/messages` концентратора уведомлений с надлежащим текстом и заголовками для отправки уведомления.

## <a name="complete-tutorial"></a>Завершение работы с учебником

Теперь вы можете завершить работу с учебником по началу работы, отправив уведомление из серверной части PHP.

Инициализируйте клиент концентратора уведомлений (замените строку подключения и имя концентратора в соответствии с инструкциями в [Учебника по началу работы]):

    ```php
    $hub = new NotificationHub("connection string", "hubname");
    ```

Затем добавьте код отправки, определяемый целевой мобильной платформой.

### <a name="windows-store-and-windows-phone-81-non-silverlight"></a>Магазин Windows и Windows Phone 8.1 (без Silverlight)

    ```php
    $toast = '<toast><visual><binding template="ToastText01"><text id="1">Hello from PHP!</text></binding></visual></toast>';
    $notification = new Notification("windows", $toast);
    $notification->headers[] = 'X-WNS-Type: wns/toast';
    $hub->sendNotification($notification, null);
    ```

### <a name="ios"></a>iOS

    ```php
    $alert = '{"aps":{"alert":"Hello from PHP!"}}';
    $notification = new Notification("apple", $alert);
    $hub->sendNotification($notification, null);
    ```

### <a name="android"></a>Android

    ```php
    $message = '{"data":{"msg":"Hello from PHP!"}}';
    $notification = new Notification("fcm", $message);
    $hub->sendNotification($notification, null);
    ```

### <a name="windows-phone-80-and-81-silverlight"></a>Windows Phone 8.0 и 8.1 Silverlight

    ```php
    $toast = '<?xml version="1.0" encoding="utf-8"?>' .
                '<wp:Notification xmlns:wp="WPNotification">' .
                   '<wp:Toast>' .
                        '<wp:Text1>Hello from PHP!</wp:Text1>' .
                   '</wp:Toast> ' .
                '</wp:Notification>';
    $notification = new Notification("windowsphone", $toast);
    $notification->headers[] = 'X-WindowsPhone-Target : toast';
    $notification->headers[] = 'X-NotificationClass : 2';
    $hub->sendNotification($notification, null);
    ```

### <a name="kindle-fire"></a>Kindle Fire

    ```php
    $message = '{"data":{"msg":"Hello from PHP!"}}';
    $notification = new Notification("adm", $message);
    $hub->sendNotification($notification, null);
    ```

После выполнения кода PHP на целевом устройстве должно отобразиться уведомление.

## <a name="next-steps"></a>Следующие шаги

В этом разделе было показано, как создать простой клиент Java REST для службы "Центры уведомлений". На данном этапе можно сделать следующее.

* Загрузить полный [Примере оболочки PHP REST], содержащий весь указанный выше код.
* Продолжить изучение функции тегов в концентраторах уведомлений в [учебнике "Срочные новости"]
* Изучить отправку уведомлений отдельным пользователям в [учебнике "Уведомление пользователей"]

Дополнительную информацию можно найти также в [Центре разработчика PHP](https://azure.microsoft.com/develop/php/).

[Примере оболочки PHP REST]: https://github.com/Azure/azure-notificationhubs-samples/tree/master/notificationhubs-rest-php
[Учебника по началу работы]: https://azure.microsoft.com/documentation/articles/notification-hubs-ios-get-started/
