---
title: Как использовать очереди служебной шины с помощью PHP | Документация Майкрософт
description: Узнайте, как использовать очереди служебной шины в Azure. Примеры кода написаны на PHP.
services: service-bus-messaging
documentationcenter: php
author: axisc
manager: timlt
editor: spelluru
ms.assetid: e29c829b-44c5-4350-8f2e-39e0c380a9f2
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: PHP
ms.topic: article
ms.date: 04/10/2019
ms.author: aschhab
ms.openlocfilehash: 92ea3c71dda011c5f7b19682d9bdea6c226ae5d2
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "65992083"
---
# <a name="how-to-use-service-bus-queues-with-php"></a>Как использовать очереди служебной шины с PHP
[!INCLUDE [service-bus-selector-queues](../../includes/service-bus-selector-queues.md)]

В этом руководстве вы узнаете, как создавать приложения PHP для отправки и получения сообщений из очереди служебной шины. 

## <a name="prerequisites"></a>Технические условия
1. Подписка Azure. Для работы с этим учебником требуется учетная запись Azure. Вы можете активировать ваши [преимущества для подписчиков MSDN](https://azure.microsoft.com/pricing/member-offers/credit-for-visual-studio-subscribers/?WT.mc_id=A85619ABF) или зарегистрироваться для [бесплатную учетную запись](https://azure.microsoft.com/free/?WT.mc_id=A85619ABF).
2. Если у вас нет очереди для работы с, выполните шаги [с помощью портала Azure создать очередь служебной шины](service-bus-quickstart-portal.md) статью, чтобы создать очередь.
    1. Чтение быстрого **Обзор** служебной шины **очереди**. 
    2. Чтобы создать служебную шину **пространства имен**. 
    3. Получить **строку подключения**. 

        > [!NOTE]
        > Вы создадите **очереди** в пространстве имен служебной шины с помощью приложения PHP в этом руководстве. 
3. [Пакет Azure SDK для PHP](../php-download-sdk.md)

## <a name="create-a-php-application"></a>Создание приложения PHP
Для создания приложения PHP, которое получает доступ к службе BLOB-объектов Azure, достаточно сослаться на классы в [пакете SDK Azure для PHP](../php-download-sdk.md) непосредственно из кода. Для создания приложения можно использовать любые средства разработки или Блокнот.

> [!NOTE]
> В установленном пакете PHP должно быть установлено и включено [расширение OpenSSL](https://php.net/openssl).

В этом руководстве будет использовать функции службы, которые могут быть вызваны из приложения PHP локально или в коде, работающем в Azure веб-роли, рабочей роли или веб-сайта.

## <a name="get-the-azure-client-libraries"></a>Получение клиентских библиотек Azure
[!INCLUDE [get-client-libraries](../../includes/get-client-libraries.md)]

## <a name="configure-your-application-to-use-service-bus"></a>Настройка приложения для использования служебной шины
Чтобы использовать интерфейсы API очередей служебной шины, необходимо следующее:

1. Ссылка на файл автозагрузчика с использованием оператора [require_once][require_once].
2. Ссылка на любые классы, которые могут использоваться.

В следующем примере показано, как включить файл автозагрузчика и добавить ссылку на класс `ServicesBuilder`.

> [!NOTE]
> В этом примере (и других примерах в этой статье) предполагается, что установлены клиентские библиотеки PHP для Azure с помощью Composer. При установке библиотек вручную или в качестве пакета PEAR необходимо использовать ссылку на файл автозагрузчика **WindowsAzure.php**.
> 
> 

```php
require_once 'vendor/autoload.php';
use WindowsAzure\Common\ServicesBuilder;
```

В приведенных ниже примерах всегда будет отображаться оператор `require_once`, однако ссылки будут приводиться только на классы, которые необходимы для выполнения этого примера.

## <a name="set-up-a-service-bus-connection"></a>Настройка подключения к Service Bus
Для создания клиента служебной шины необходимо сначала сформировать правильную строку подключения в следующем формате:

```
Endpoint=[yourEndpoint];SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=[Primary Key]
```

где `Endpoint` обычно имеет формат `[yourNamespace].servicebus.windows.net`.

Для создания клиента службы Azure необходимо использовать класс `ServicesBuilder`. Вы можете:

* Передать строку подключения напрямую или
* использовать **CloudConfigurationManager (CCM)** для проверки нескольких внешних источников на наличие строки подключения:
  * По умолчанию предоставляется поддержка одного внешнего источника — переменных среды.
  * Можно добавить новые источники, расширив класс `ConnectionStringSource`.

В приведенных здесь примерах строка подключения передается напрямую.

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;

$connectionString = "Endpoint=[yourEndpoint];SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=[Primary Key]";

$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);
```

## <a name="create-a-queue"></a>Создание очереди
Операции управления очередями служебной шины можно выполнять с помощью класса `ServiceBusRestProxy`. Объект `ServiceBusRestProxy` создается посредством фабричного метода `ServicesBuilder::createServiceBusService` с соответствующей строкой подключения, инкапсулирующей в себе разрешения маркера на управление им.

В приведенном ниже примере показано, как создать экземпляр `ServiceBusRestProxy` и вызвать метод `ServiceBusRestProxy->createQueue` для создания очереди `myqueue` в пространстве имен службы `MySBNamespace`.

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use WindowsAzure\Common\ServiceException;
use WindowsAzure\ServiceBus\Models\QueueInfo;

// Create Service Bus REST proxy.
$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);

try    {
    $queueInfo = new QueueInfo("myqueue");

    // Create queue.
    $serviceBusRestProxy->createQueue($queueInfo);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here: 
    // https://docs.microsoft.com/rest/api/storageservices/Common-REST-API-Error-Codes
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

> [!NOTE]
> Можно использовать метод `listQueues` для объектов `ServiceBusRestProxy`, чтобы проверить, существует ли уже очередь с указанным именем в пространстве имен.
> 
> 

## <a name="send-messages-to-a-queue"></a>Отправка сообщений в очередь
Чтобы отправить сообщение в очередь служебной шины, в приложении вызывается метод `ServiceBusRestProxy->sendQueueMessage`. В следующем примере кода показано, как отправить сообщение в очередь `myqueue`, созданную ранее в пространстве имен службы `MySBNamespace`.

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use WindowsAzure\Common\ServiceException;
use WindowsAzure\ServiceBus\Models\BrokeredMessage;

// Create Service Bus REST proxy.
$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);

try    {
    // Create message.
    $message = new BrokeredMessage();
    $message->setBody("my message");

    // Send message.
    $serviceBusRestProxy->sendQueueMessage("myqueue", $message);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here: 
    // https://docs.microsoft.com/rest/api/storageservices/Common-REST-API-Error-Codes
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

Сообщения, отправленные (и полученные из) служебной шины, очереди являются экземплярами [BrokeredMessage][BrokeredMessage] класса. У объектов [BrokeredMessage][BrokeredMessage] есть набор стандартных методов и свойств, используемый для хранения настраиваемых свойств приложения, и текст из произвольных данных приложения.

Очереди служебной шины поддерживают максимальный размер сообщения 256 КБ для [уровня "Стандартный"](service-bus-premium-messaging.md) и 1 МБ для [уровня Premium](service-bus-premium-messaging.md). Максимальный размер заголовка, который содержит стандартные и настраиваемые свойства приложения, — 64 КБ. Ограничения на количество сообщений в очереди нет, но есть максимальный общий размер сообщений, содержащихся в очереди. Максимальный размер очереди ограничен 5 ГБ.

## <a name="receive-messages-from-a-queue"></a>Получение сообщений из очереди

Наилучшим способом получать сообщения из очереди является использование метода `ServiceBusRestProxy->receiveQueueMessage`. Сообщения можно получить в двух различных режимах: [*ReceiveAndDelete*](/dotnet/api/microsoft.servicebus.messaging.receivemode) и [*PeekLock*](/dotnet/api/microsoft.servicebus.messaging.receivemode#Microsoft_ServiceBus_Messaging_ReceiveMode_PeekLock). Значение по умолчанию — **PeekLock**.

В режиме [ReceiveAndDelete](/dotnet/api/microsoft.servicebus.messaging.receivemode) получение является одиночной операцией. Это значит, что когда служебная шина получает запрос на чтение для сообщения в очереди, это сообщение помечается как использованное и возвращается в приложение. Режим [ReceiveAndDelete](/dotnet/api/microsoft.servicebus.messaging.receivemode) представляет собой самую простую модель, которая лучше всего работает в ситуациях, когда приложение может не обрабатывать сообщение при сбое. Чтобы это понять, рассмотрим сценарий, в котором объект-получатель выдает запрос на получение и выходит из строя до его обработки. Поскольку служебная шина помечает сообщение как использованное, то когда после своего перезапуска приложение снова начнет обрабатывать сообщения, оно пропустит сообщение, использованное до сбоя.

В используемом по умолчанию режиме [PeekLock](/dotnet/api/microsoft.servicebus.messaging.receivemode#Microsoft_ServiceBus_Messaging_ReceiveMode_PeekLock) процесс получения становится двухэтапной операцией, что позволяет поддерживать приложения, не устойчивые к пропуску сообщений. Получив запрос, Service Bus находит следующее сообщение, блокирует его, чтобы другие получатели не могли его принять, а затем возвращает его приложению. Когда приложение завершает обработку сообщения (или сохраняет его для будущей обработки), оно завершает второй этап процесса получения, вызывая метод `ServiceBusRestProxy->deleteMessage` для полученного сообщения. Когда служебная шина обнаруживает вызов метода `deleteMessage`, она помечает сообщение как использованное и удаляет его из очереди.

В следующем примере показывается, как получать и обрабатывать сообщения с помощью режима [PeekLock](/dotnet/api/microsoft.servicebus.messaging.receivemode#Microsoft_ServiceBus_Messaging_ReceiveMode_PeekLock) (используется по умолчанию).

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use WindowsAzure\Common\ServiceException;
use WindowsAzure\ServiceBus\Models\ReceiveMessageOptions;

// Create Service Bus REST proxy.
$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);

try    {
    // Set the receive mode to PeekLock (default is ReceiveAndDelete).
    $options = new ReceiveMessageOptions();
    $options->setPeekLock();

    // Receive message.
    $message = $serviceBusRestProxy->receiveQueueMessage("myqueue", $options);
    echo "Body: ".$message->getBody()."<br />";
    echo "MessageID: ".$message->getMessageId()."<br />";

    /*---------------------------
        Process message here.
    ----------------------------*/

    // Delete message. Not necessary if peek lock is not set.
    echo "Message deleted.<br />";
    $serviceBusRestProxy->deleteMessage($message);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // https://docs.microsoft.com/rest/api/storageservices/Common-REST-API-Error-Codes
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

## <a name="how-to-handle-application-crashes-and-unreadable-messages"></a>Как обрабатывать сбои приложения и нечитаемые сообщения

служебная шина предоставляет функции, помогающие корректно выполнить восстановление после ошибок в приложении или трудностей, возникших при обработке сообщения. Если приложение-получатель по каким-либо причинам не может обработать сообщение, то оно может вызвать метод `unlockMessage` для полученного сообщения (вместо метода `deleteMessage`). После этого служебная шина разблокирует сообщение в очереди и сделает его доступным для приема тем же приложением или другим приложением.

Кроме того, с сообщением, заблокированным в очереди, связано время ожидания. Если приложение не сможет обработать сообщение в течение времени ожидания (например, при сбое приложения), служебная шина разблокирует сообщение автоматически и сделает его доступным для приема.

Если сбой приложения происходит после обработки сообщения, но перед отправкой запроса `deleteMessage`, то это сообщение повторно доставляется в приложение после его перезапуска. Часто такой подход называют *Обработать хотя бы один раз*, т. е. каждое сообщение будет обрабатываться по крайней мере один раз, но в некоторых случаях это же сообщение может быть доставлено повторно. Если повторная обработка недопустима, рекомендуется добавить в приложение дополнительную логику для обработки повторной доставки сообщений. Часто это достигается с помощью метода `getMessageId` сообщения, который остается постоянным для различных попыток доставки.

> [!NOTE]
> Вы можете управлять ресурсами служебной шины с помощью [обозревателя служебной шины](https://github.com/paolosalvatori/ServiceBusExplorer/). Обозреватель служебной шины позволяет без труда подключаться к пространству имен служебной шины и управлять сущностями обмена сообщениями. Средство предоставляет дополнительные возможности, например функции импорта и экспорта или возможность проверять разделы, очереди, подписки, службы ретрансляции, центры уведомлений и концентраторы событий. 

## <a name="next-steps"></a>Дальнейшие действия
Вы ознакомились с основами использования очередей служебной шины. Дополнительные сведения см. в статье [Очереди, разделы и подписки служебной шины][Queues, topics, and subscriptions].

Дополнительные сведения также доступны в [Центре разработчика PHP](https://azure.microsoft.com/develop/php/).

[BrokeredMessage]: /dotnet/api/microsoft.servicebus.messaging.brokeredmessage
[Queues, topics, and subscriptions]: service-bus-queues-topics-subscriptions.md
[require_once]: https://php.net/require_once


