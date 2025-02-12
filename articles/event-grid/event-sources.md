---
title: Источники событий службы "Сетка событий Azure"
description: Описание поддерживаемых источников события в службе "Сетка событий Azure".
services: event-grid
author: spelluru
ms.service: event-grid
ms.topic: conceptual
ms.date: 02/12/2019
ms.author: spelluru
ms.openlocfilehash: ad9bb5c135684c4573195298fb8e55a08208f6b6
ms.sourcegitcommit: 1572b615c8f863be4986c23ea2ff7642b02bc605
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/10/2019
ms.locfileid: "67785824"
---
# <a name="event-sources-in-azure-event-grid"></a>Источники событий в службе "Сетка событий Azure"

Источник события — это место, где оно произошло. Некоторые службы Azure автоматически настроены для отправки событий. Вы также можете создать пользовательские приложения, отправляющие события. Для распространения событий с помощью Сетки событий пользовательские приложения не обязательно должны размещаться в Azure.

В этой статье представлены ссылки на информацию о каждом источнике события.

## <a name="azure-subscriptions"></a>Подписки Azure

Подпишитесь на события подписок Azure, чтобы реагировать на изменения ресурсов в подписке Azure.

|Заголовок |Описание  |
|---------|---------|
| [Учебник. по службе автоматизации Azure со службой "Сетка событий Azure" и Microsoft Teams](ensure-tags-exists-on-new-virtual-machines.md) |Создайте виртуальную машину, которая отправляет событие. Это событие вызывает модуль runbook службы автоматизации, который присваивает виртуальной машине тег и отправляет сообщение в канал Microsoft Teams. |
| [Подписка на события через портал](subscribe-through-portal.md) | Использование портала для создания подписки на события, связанные с подпиской Azure. |
| [Создание подписки на события, связанные с подпиской Azure, с использованием Azure CLI](./scripts/event-grid-cli-azure-subscription.md) |Пример сценария, который создает подписку Сетки событий для подписки Azure и отправляет события в веб-перехватчик. |
| [Создание подписки на события, связанные с подпиской Azure, с помощью PowerShell](./scripts/event-grid-powershell-azure-subscription.md)| Пример сценария, который создает подписку Сетки событий для подписки Azure и отправляет события в веб-перехватчик. |
| [Схема событий службы "Сетка событий Azure" для подписок](event-schema-subscriptions.md) | Содержит сведения о полях в событиях подписки Azure. |

## <a name="container-registry"></a>Реестр контейнеров

Подписка на события Реестра контейнеров Azure для реагирования на изменения в образах.

|Заголовок |Описание  |
|---------|---------|
| [Быстрое руководство по отправке событий реестра контейнеров](../container-registry/container-registry-event-grid-quickstart.md?toc=%2fazure%2fevent-grid%2ftoc.json) | Содержит сведения об отправке событий Реестра контейнеров с помощью Azure CLI. |
| [Схема событий службы "Сетка событий Azure" для подписок](event-schema-container-registry.md) | Содержит сведения о полях событий Реестра контейнеров. |

## <a name="custom-topics"></a>Пользовательские разделы

Подпишитесь на пользовательские разделы, чтобы реагировать на события приложений.

|Заголовок  |Описание  |
|---------|---------|
| [Создание и перенаправление пользовательских событий с помощью Azure CLI и службы "Сетка событий"](custom-event-quickstart.md) | Содержит сведения об отправке пользовательских событий с помощью Azure CLI. |
| [Создание и перенаправление пользовательских событий с помощью службы Azure PowerShell и "Сетка событий"](custom-event-quickstart-powershell.md) | Содержит сведения об отправке пользовательских событий с помощью Azure PowerShell. |
| [Создание и перенаправление пользовательских событий с помощью портала Azure и службы "Сетка событий"](custom-event-quickstart-portal.md) | Содержит сведения об отправке пользовательских событий с помощью портала. |
| [Перенаправление пользовательских событий в хранилище очередей Azure с помощью Azure CLI и службы "Сетка событий"](custom-event-to-queue-storage.md) | В этой статье объясняется, как отправлять пользовательские события в хранилище очередей. |
| [Публикация в пользовательском разделе для службы "Сетка событий Azure"](post-to-custom-topic.md) | Содержит сведения о публикации события в пользовательском разделе. |
| [Создание пользовательского раздела службы "Сетка событий" с помощью Azure CLI](./scripts/event-grid-cli-create-custom-topic.md)|Пример сценария, который создает пользовательский раздел. Этот сценарий извлекает конечную точку и ключ.|
| [Создание подписки на события, связанные с пользовательским разделом, с использованием Azure CLI](./scripts/event-grid-cli-subscribe-custom-topic.md)|Пример сценария, который создает подписку на пользовательский раздел. Он отправляет события в веб-перехватчик.|
| [Создание пользовательского раздела службы "Сетка событий" с помощью PowerShell](./scripts/event-grid-powershell-create-custom-topic.md)|Пример сценария, который создает пользовательский раздел. Этот сценарий извлекает конечную точку и ключ.|
| [Создание подписки на события, связанные с пользовательским разделом, с использованием PowerShell](./scripts/event-grid-powershell-subscribe-custom-topic.md)|Пример сценария, который создает подписку на пользовательский раздел. Он отправляет события в веб-перехватчик.|
| [Шаблон Resource Manager для создания пользовательского раздела и конечной точки веб-перехватчика](https://github.com/Azure/azure-quickstart-templates/tree/master/101-event-grid) | Шаблон Resource Manager, который создает пользовательский раздел и подписку для этого раздела. Он отправляет события в веб-перехватчик. |
|
| [Шаблон Resource Manager для создания пользовательского раздела и конечной точки Центров событий](https://github.com/Azure/azure-quickstart-templates/tree/master/101-event-grid-event-hubs-handler)| Шаблон Resource Manager, который создает подписку для пользовательского раздела. Он отправляет события в Центры событий Azure. |
| [Схема событий службы "Сетка событий Azure" для подписок](event-schema.md) | Содержит сведения о полях в пользовательских событиях. |

## <a name="event-hubs"></a>Центры событий

Подпишитесь на события Центров событий, чтобы реагировать на события создания файла сбора данных. Центры событий могут действовать либо как источник события, либо как обработчик события. В следующих статьях показано, как использовать Центры событий в качестве источника.

|Заголовок  |Описание  |
|---------|---------|
| [Потоковая передача больших данных в хранилище данных](event-grid-event-hubs-integration.md) | Когда Центры событий создают файл сбора, служба "Сетка событий" отправляет событие приложению-функции. Это приложение извлекает файл записи и перемещает данные в хранилище данных. |
| [Схема событий службы "Сетка событий Azure" для концентраторов](event-schema-event-hubs.md) | Содержит сведения о полях в событиях в Центрах событий. |

Примеры Центров событий в качестве обработчика см. в разделе [Концентраторы событий](event-handlers.md#event-hubs).

## <a name="iot-hub"></a>Центр Интернета вещей

Подписаться на события центра Интернета вещей реагировать на устройство, созданное, удаленные, подключенными, отключенных и события телеметрии.

|Заголовок  |Описание  |
|---------|---------|
| [Отправка уведомлений электронной почты о событиях в Центре Интернета вещей Azure с помощью Logic Apps](publish-iot-hub-events-to-logic-apps.md) | Приложение логики отправляет уведомление по электронной почте каждый раз, когда добавляется устройство в Центр Интернета вещей. |
| [Реагирование на события в Центре Интернета вещей, используя службу "Сетка событий" для запуска действий (предварительная версия)](../iot-hub/iot-hub-event-grid.md) | Общие сведения об интеграции Центра Интернета вещей со службой "Сетка событий". |
| [Схема событий службы "Сетка событий Azure" для подписок](event-schema-iot-hub.md) | Содержит сведения о полях событий в Центре Интернета вещей. |
| [Order device connection events from Azure IoT Hub using Azure Cosmos DB](../iot-hub/iot-hub-how-to-order-connection-state-events.md) (Упорядочение событий подключения устройств из Центра Интернета вещей с помощью Azure Cosmos DB) | Сведения об упорядочении событий изменения состояния подключения устройства. |

## <a name="media-services"></a>Службы мультимедиа

Подпишитесь на события Служб мультимедиа, чтобы реагировать на события состояния задания.

|Заголовок  |Описание  |
|---------|---------|
| [Реагирование на события Служб мультимедиа Azure](../media-services/latest/reacting-to-media-services-events.md) | Общие сведения об интеграции Служб мультимедиа со службой "Сетка событий". |
| [Маршрутизация событий Служб мультимедиа Azure в пользовательскую конечную веб-точку с помощью CLI](../media-services/latest/job-state-events-cli-how-to.md?toc=%2fazure%2fevent-grid%2ftoc.json) | Содержит сведения об отправке событий из Служб мультимедиа. |
| [Azure Event Grid schemas for Media Services events](../media-services/latest/media-services-event-schemas.md?toc=%2fazure%2fevent-grid%2ftoc.json) (Схемы событий службы "Сетка событий Azure" для событий Служб мультимедиа) | Содержит сведения о полях в событиях Служб мультимедиа. |

## <a name="resource-groups"></a>Группы ресурсов

Подпишитесь на события группы ресурсов, чтобы реагировать на изменения ресурсов в группе.

|Заголовок  |Описание  |
|---------|---------|
| [Отслеживание изменений виртуальной машины с помощью Azure Logic Apps и службы "Сетка событий Azure"](monitor-virtual-machine-changes-event-grid-logic-app.md) | Приложение логики отслеживает изменения в виртуальной машине и отправляет сообщения электронной почты об этих изменениях. |
| [Создание подписки на события, связанные с группой ресурсов, с использованием Azure CLI](./scripts/event-grid-cli-resource-group.md)| Пример сценария, позволяющий подписаться на события, связанные с группой ресурсов. Он отправляет события в веб-перехватчик. |
| [Подписка на события группы ресурсов и фильтрация событий для ресурса с использованием Azure CLI](./scripts/event-grid-cli-resource-group-filter.md) | Пример сценария, позволяющий подписаться на события группы ресурсов и отфильтровать события для ресурса. |
| [Создание подписки на события, связанные с группой ресурсов, с использованием PowerShell](./scripts/event-grid-powershell-resource-group.md) | Пример сценария, позволяющий подписаться на события, связанные с группой ресурсов. Он отправляет события в веб-перехватчик. |
| [Подписка на события группы ресурсов и фильтрация по ресурсу с использованием PowerShell](./scripts/event-grid-powershell-resource-group-filter.md) | Пример сценария, позволяющий подписаться на события группы ресурсов и отфильтровать события для ресурса. |
| [Шаблон Resource Manager: подписка на ресурсы](https://github.com/Azure/azure-quickstart-templates/tree/master/101-event-grid-resource-events-to-webhook) | Подписывается на события для группы ресурсов или подписки Azure. Он отправляет события в веб-перехватчик. |
| [Схема событий](event-schema-resource-groups.md) | Содержит сведения о полях в событиях группы ресурсов. |

## <a name="service-bus"></a>Служебная шина

Подпишитесь на события служебной шины, чтобы отвечать на сообщения без активного прослушивателя.

|Заголовок  |Описание  |
|---------|---------|
| [Учебник. с примерами интеграции служебной шины Azure со службой "Сетка событий Azure"](../service-bus-messaging/service-bus-to-event-grid-integration-example.md?toc=%2fazure%2fevent-grid%2ftoc.json) | Служба "Сетка событий" отправляет сообщения из раздела служебной шины в приложение-функцию и приложение логики. |
| [Apache Интеграция служебной шины Azure со службой "Сетка событий"](../service-bus-messaging/service-bus-to-event-grid-integration-concept.md) | Общие сведения об интеграции служебной шины со службой "Сетка событий". |
| [Схема событий службы "Сетка событий Azure" для служебной шины](event-schema-service-bus.md) | Содержит сведения о полях в событиях служебной шины. |

## <a name="storage"></a>Хранилище

Подпишитесь на события в хранилище BLOB-объектов, чтобы реагировать на события создания и удаления большого двоичного объекта.

|Заголовок  |Описание  |
|---------|---------|
| [Перенаправление событий хранилища BLOB-объектов в пользовательскую конечную веб-точку с помощью Azure CLI](../storage/blobs/storage-blob-event-quickstart.md?toc=%2fazure%2fevent-grid%2ftoc.json) | Содержит сведения об отправке событий хранилища BLOB-объектов в веб-перехватчик с помощью Azure CLI. |
| [Перенаправление событий хранилища BLOB-объектов в пользовательскую конечную веб-точку с помощью PowerShell](../storage/blobs/storage-blob-event-quickstart-powershell.md?toc=%2fazure%2fevent-grid%2ftoc.json) | Содержит сведения об отправке событий хранилища BLOB-объектов в веб-перехватчик с помощью Azure PowerShell. |
| [Создание и перенаправление событий хранилища BLOB-объектов с помощью службы "Сетка событий Azure" и портала Azure](blob-event-quickstart-portal.md) | Содержит сведения об отправке событий хранилища BLOB-объектов в веб-перехватчик с помощью портала. |
| [Создание подписки на события, связанные с учетной записью хранения больших двоичных объектов, с использованием Azure CLI](./scripts/event-grid-cli-blob.md) | Пример сценария, позволяющий подписаться на события, связанные с учетной записью хранения больших двоичных объектов. Он отправляет событие в веб-перехватчик. |
| [Создание подписки на события, связанные с учетной записью хранения больших двоичных объектов, с помощью PowerShell](./scripts/event-grid-powershell-blob.md) | Пример сценария, позволяющий подписаться на события, связанные с учетной записью хранения больших двоичных объектов. Он отправляет событие в веб-перехватчик. |
| [Шаблон Resource Manager. Создание учетной записи хранения BLOB-объектов и подписки](https://github.com/Azure/azure-quickstart-templates/tree/master/101-event-grid-subscription-and-storage) | Развертывает учетную запись службы хранилища больших двоичных объектов Azure и создает подписку на события для этой учетной записи хранения. Он отправляет события в веб-перехватчик. |
| [Реагирование на события хранилища BLOB-объектов](../storage/blobs/storage-blob-event-overview.md) | Общие сведения об интеграции хранилища BLOB-объектов со службой "Сетка событий". |
| [Схема событий службы "Сетка событий Azure" для хранилища BLOB-объектов](event-schema-blob-storage.md) | Содержит сведения о полях в событиях хранилища BLOB-объектов. |

## <a name="maps"></a>Maps
Подпишитесь на события Azure Maps, чтобы реагировать на события геозоны. Например, приложение может делать уведомление на электронную почту каждый раз, когда устройство входит в геозону или выходит из нее.

|Заголовок  |Описание  |
|---------|---------|
| [Реагирование на события Azure Maps c помощью Сетки событий](../azure-maps/azure-maps-event-grid-integration.md?toc=%2fazure%2fevent-grid%2ftoc.json) | Общие сведения об интеграции Azure Maps со службой "Сетка событий". |
| [Учебник. Настройка геозоны с использованием Azure Maps](../azure-maps/tutorial-geofence.md?toc=%2fazure%2fevent-grid%2ftoc.json) | Это руководство поможет выполнить основные действия по настройке геозоны с помощью Azure Maps. Используйте Сетку событий Azure для выполнения потоковой передачи результатов геозоны и на их основе создавайте уведомления. |
| [Схема событий службы "Сетка событий Azure" для подписок](event-schema-azure-maps.md) | Отображает поля в событиях службы Azure Maps. |

## <a name="app-configuration"></a>Конфигурация приложений
Подписаться на события конфигурации приложения Azure для реагирования на события изменения значений ключа.

|Заголовок | Описание |
|---------|---------|
| [Реагирование на события конфигурации приложения Azure с помощью сетки событий](../azure-app-configuration/concept-app-configuration-event.md?toc=%2fazure%2fevent-grid%2ftoc.json) | Общие сведения об интеграции Конфигурация приложения Azure с помощью сетки событий. |
| [Краткое руководство: перенаправление событий Конфигурация приложения Azure пользовательские сетевую конечную точку с помощью Azure CLI](../azure-app-configuration/howto-app-configuration-event.md?toc=%2fazure%2fevent-grid%2ftoc.json) | Показано, как с помощью Azure CLI для отправки событий Конфигурация приложения Azure в веб-перехватчика. |
| [Схема событий службы "Сетка событий Azure" для подписок](event-schema-app-configuration.md) | Отображает поля в событиях Конфигурация приложения Azure. |

## <a name="azure-signalr"></a>Azure SignalR
Подписаться на события служба Azure SignalR для реагирования на события подключения клиента.

|Заголовок | Описание |
|---------|---------|
| [Реагирование на события служба Azure SignalR с помощью сетки событий](../azure-signalr/signalr-concept-event-grid-integration.md) | Общие сведения об интеграции служба Azure SignalR с помощью сетки событий. |
| [Как отправлять события служба Azure SignalR "Сетка событий"](../azure-signalr/signalr-howto-event-grid-integration.md) | Демонстрируется способ отправки событий служба Azure SignalR для приложения с помощью сетки событий. |
| [Схема событий службы "Сетка событий Azure" для подписок](event-schema-azure-signalr.md) | Отображает поля событий служба Azure SignalR. |



## <a name="next-steps"></a>Следующие шаги

* Общие сведения о службе "Сетка событий" см. в разделе [Общие сведения о службе "Сетка событий Azure"](overview.md).
* Сведения о том, как быстро приступить к использованию службы "Сетка событий", см. в разделе [Создание и перенаправление пользовательского события со службой "Сетка событий Azure"](custom-event-quickstart.md).
