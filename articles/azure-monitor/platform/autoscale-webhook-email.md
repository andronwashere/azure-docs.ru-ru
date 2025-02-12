---
title: Использование автомасштабирования для отправки электронной почты и уведомлений об оповещениях веб-перехватчика
description: 'Узнайте, как использовать действия автомасштабирования для вызова URL-адресов веб-сайтов или отправки уведомлений по электронной почте в Azure Monitor. '
author: anirudhcavale
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 04/03/2017
ms.author: ancav
ms.subservice: autoscale
ms.openlocfilehash: c1386f4058f9490bad0161b680005db6031bace1
ms.sourcegitcommit: ac1cfe497341429cf62eb934e87f3b5f3c79948e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/01/2019
ms.locfileid: "67491530"
---
# <a name="use-autoscale-actions-to-send-email-and-webhook-alert-notifications-in-azure-monitor"></a>Использование действий автомасштабирования для отправки электронной почты и уведомлений об оповещениях веб-перехватчика в Azure Monitor
В этой статье показано, как настраиваются триггеры, позволяющие вам обращаться к определенным URL-адресам или отправлять сообщения электронной почты на основе действий автоматического масштабирования в Azure.  

## <a name="webhooks"></a>Веб-перехватчики
Объекты Webhook (веб-перехватчики) позволяют направлять уведомления об оповещениях Azure в другие системы для постобработки или отображения настраиваемых уведомлений. Например, оповещения Azure могут направляться в службы, которые способны обрабатывать входящие веб-запросы для отправки SMS, ведения журналов, уведомления членов команды в чате или через службы обмена мгновенными сообщениями и т. д. Универсальный код ресурса (URI) веб-перехватчика должен быть допустимой конечной точкой HTTP или HTTPS.

## <a name="email"></a>Email
Электронная почта может отправляться на любой допустимый адрес электронной почты. Также будут уведомляться администраторы и соадминистраторы подписки, где работает это правило.

## <a name="cloud-services-and-web-apps"></a>Облачные службы и веб-приложения
Вы можете явно согласиться на облачные службы и фермы серверов (веб-приложения) на портале Azure.

* Выберите метрику **scale by (масштабировать по)** .

![scale by (масштабировать по)](./media/autoscale-webhook-email/insights-autoscale-notify.png)

## <a name="virtual-machine-scale-sets"></a>Наборы для масштабирования виртуальных машин
Для новых виртуальных машин, созданных с помощью Resource Manager, наборы для масштабирования можно настроить с помощью REST API, шаблонов Resource Manager, PowerShell и интерфейса командной строки. Интерфейс портала еще недоступен.
Если вы используете REST API или шаблон Resource Manager, включите элемент уведомлений с использованием следующих параметров:

```
"notifications": [
      {
        "operation": "Scale",
        "email": {
          "sendToSubscriptionAdministrator": false,
          "sendToSubscriptionCoAdministrators": false,
          "customEmails": [
              "user1@mycompany.com",
              "user2@mycompany.com"
              ]
        },
        "webhooks": [
          {
            "serviceUri": "https://foo.webhook.example.com?token=abcd1234",
            "properties": {
              "optional_key1": "optional_value1",
              "optional_key2": "optional_value2"
            }
          }
        ]
      }
    ]
```

| Поле | Обязательное? | Описание |
| --- | --- | --- |
| operation |Да |Значение должно быть Scale. |
| sendToSubscriptionAdministrator |Да |Значение должно быть true или false. |
| sendToSubscriptionCoAdministrators |Да |Значение должно быть true или false. |
| customEmails |Да |Значение может быть null или массивом строк с адресами электронной почты. |
| webhooks |Да |Значение может быть null или допустимым универсальным кодом ресурса (URI). |
| serviceUri |Да |Допустимый универсальный код ресурса (URI) HTTPS. |
| properties |Да |Значение должно быть пустым, {}, или может содержать пары "ключ — значение". |

## <a name="authentication-in-webhooks"></a>Проверка подлинности в веб-перехватчиках
Для webhook может использоваться аутентификация на основе маркеров, заключающаяся в сохранении универсального кода ресурса (URI) webhook с идентификатором маркера в качестве параметра запроса. Например, https: \/ /mysamplealert/webcallback? tokenid = sometokenid & someparameter = somevalue

## <a name="autoscale-notification-webhook-payload-schema"></a>Схема полезных данных веб-перехватчика уведомлений автомасштабирования
При создании уведомлений автомасштабирования в полезные данные веб-перехватчика включаются следующие метаданные:

```
{
        "version": "1.0",
        "status": "Activated",
        "operation": "Scale In",
        "context": {
                "timestamp": "2016-03-11T07:31:04.5834118Z",
                "id": "/subscriptions/s1/resourceGroups/rg1/providers/microsoft.insights/autoscalesettings/myautoscaleSetting",
                "name": "myautoscaleSetting",
                "details": "Autoscale successfully started scale operation for resource 'MyCSRole' from capacity '3' to capacity '2'",
                "subscriptionId": "s1",
                "resourceGroupName": "rg1",
                "resourceName": "MyCSRole",
                "resourceType": "microsoft.classiccompute/domainnames/slots/roles",
                "resourceId": "/subscriptions/s1/resourceGroups/rg1/providers/microsoft.classicCompute/domainNames/myCloudService/slots/Production/roles/MyCSRole",
                "portalLink": "https://portal.azure.com/#resource/subscriptions/s1/resourceGroups/rg1/providers/microsoft.classicCompute/domainNames/myCloudService",
                "oldCapacity": "3",
                "newCapacity": "2"
        },
        "properties": {
                "key1": "value1",
                "key2": "value2"
        }
}
```


| Поле | Обязательное? | Описание |
| --- | --- | --- |
| status |Да |Состояние, которое указывает, что создано действие автоматического масштабирования |
| operation |Да |Для увеличения экземпляров это будет "Развернуть", а для уменьшения экземпляров — "Свернуть" |
| context |Да |Контекст действия автоматического масштабирования |
| timestamp |Да |Отметка времени, когда было запущено действие автоматического масштабирования |
| id |Да |Идентификатор Resource Manager для параметра автоматического масштабирования |
| name |Да |Имя параметра автоматического масштабирования |
| сведения |Да |Описание действия, предпринятого службой автоматического масштабирования, и изменение числа экземпляров |
| subscriptionId |Да |Идентификатор подписки для масштабируемого целевого ресурса |
| resourceGroupName |Да |Имя группы ресурсов для масштабируемого целевого ресурса |
| resourceName |Да |Имя масштабируемого целевого ресурса |
| resourceType |Да |Поддерживается три значения: "microsoft.classiccompute/domainnames/slots/roles" — роли облачной службы, "microsoft.compute/virtualmachinescalesets" — наборы для масштабирования виртуальных машин и "Microsoft.Web/serverfarms" — веб-приложение |
| resourceId |Да |Идентификатор Resource Manager для масштабируемого целевого ресурса |
| portalLink |Да |Ссылка на страницу сводки целевого ресурса на портале Azure |
| oldCapacity |Да |Текущее (старое) число экземпляров, когда автомасштабирование предпринимает действие масштабирования |
| newCapacity |Да |Новое число экземпляров, до которого автомасштабирование масштабирует ресурс |
| properties |Нет |Необязательный элемент. Набор пар <ключ, значение> (например, Dictionary <String, String>). Поле свойства не является обязательным. В настраиваемом пользовательском интерфейсе или рабочем процессе на основе приложения логики вы можете вводить ключи и значения для передачи в виде полезных данных. Еще один способ передачи пользовательских свойств обратно в исходящий вызов веб-перехватчика — использование самого универсального кода ресурса (URI) веб-перехватчика (в виде параметров запроса). |

