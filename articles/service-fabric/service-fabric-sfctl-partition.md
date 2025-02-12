---
title: 'Интерфейс командной строки Azure Service Fabric : sfctl partition | Документация Майкрософт'
description: Описание команд sfctl partition интерфейса командной строки Azure Service Fabric.
services: service-fabric
documentationcenter: na
author: Christina-Kang
manager: chackdan
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: cli
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 12/06/2018
ms.author: bikang
ms.openlocfilehash: f7c9bcc51757100cb1fc957dee12213bc8bf2eec
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "60556613"
---
# <a name="sfctl-partition"></a>sfctl partition
Запрос секций для любой службы и управление ими.

## <a name="commands"></a>Команды

|Команда|Описание|
| --- | --- |
| data-loss | Этот API вызовет потерю данных в указанной секции. |
| data-loss-status | Возвращает ход выполнения операции потери данных, запущенной с помощью API StartDataLoss. |
| health | Возвращает состояние работоспособности указанной секции Service Fabric. |
| info | Возвращает сведения о секции Service Fabric. |
| list | Получает список секций службы Service Fabric. |
| load | Возвращает сведения о нагрузке указанной секции Service Fabric. |
| load-reset | Сбрасывает текущую нагрузку секции Service Fabric. |
| quorum-loss | Вызывает потерю кворума в указанной секции службы с отслеживанием состояния. |
| quorum-loss-status | Возвращает ход выполнения операции потери кворума, запущенной с помощью API StartQuorumLoss. |
| восстановить | Указывает кластеру Service Fabric, что следует попытаться восстановить определенную секцию, которая в настоящее время находится в состоянии потери кворума. |
| recover-all | Указывает кластеру Service Fabric, что следует попытаться восстановить все службы (в том числе системные), которые в настоящее время находятся в состоянии потери кворума. |
| report-health | Отправляет отчет о работоспособности секции Service Fabric. |
| restart | Этот API перезапустит некоторые или все реплики или экземпляры указанной секции. |
| restart-status | Возвращает ход выполнения операции PartitionRestart, запущенной с помощью StartPartitionRestart. |
| svc-name | Возвращает имя службы Service Fabric для секции. |

## <a name="sfctl-partition-data-loss"></a>sfctl partition data-loss
Этот API вызовет потерю данных в указанной секции.

Он активирует вызов API OnDataLossAsync секции.  Этот API вызовет потерю данных в указанной секции. Он активирует вызов API OnDataLoss секции. Потеря фактических данных будет зависеть от указанного DataLossMode.  <br> PartialDataLoss. Только кворум реплик будет удален и OnDataLoss запустится для секции, но фактическая потеря данных зависит от наличия репликации на лету.  <br> FullDataLoss. Все реплики удаляются, таким образом все данные будут потеряны и активируется OnDataLoss. Этот API следует вызывать только со службой с отслеживанием состояния в качестве целевого объекта. Вызов этого API с системной службой в качестве целевого объекта не рекомендуется.

> [!NOTE] 
> Вызов этого API нельзя отменить. Вызов CancelOperation только остановит выполнение и очистит внутреннее состояния системы. Данные не восстановятся, если команда выполнялась достаточное время, чтобы вызвать потерю данных. Вызовите API GetDataLossProgress с тем же OperationId, чтобы вернуть сведения об операции, запущенной с помощью этого API.

### <a name="arguments"></a>Аргументы

|Аргумент|Описание|
| --- | --- |
| --data-loss-mode [обязательный параметр] | Это перечисление передается в API StartDataLoss, чтобы указать, какой тип потери данных следует вызвать. |
| --operation-id [обязательный параметр] | Идентификатор GUID, определяющий вызов этого API.  Он передается в соответствующий API GetProgress. |
| --partition-id [обязательный параметр] | Идентификатор секции. |
| --service-id [обязательный параметр] | Идентификатор службы. Обычно он содержит полное имя службы без указания схемы универсального кода ресурса (URI) fabric\:. Начиная с версии 6.0 иерархические имена разделяются знаком \~. Например, если имя службы — fabric\:/myapp/app1/svc1, в версии 6.0 и более поздних версиях будет использоваться идентификатор службы myapp\~app1\~svc1, а в предыдущих версиях — myapp/app1/svc1. |
| --timeout -t | Время ожидания сервера в секундах.  Значение по умолчанию\: 60. |

### <a name="global-arguments"></a>Глобальные аргументы

|Аргумент|Описание|
| --- | --- |
| --debug | Увеличение уровня детализации ведения журнала для отображения всех журналов отладки. |
| --help -h | Отображение этого справочного сообщения и выход. |
| --output -o | Формат выходных данных.  Допустимые значения\: json, jsonc, table, tsv.  Значение по умолчанию\: json. |
| --query | Строка запроса JMESPath. Дополнительные сведения и примеры см. на веб-сайте http\://jmespath.org/. |
| --verbose | Повышение уровня детализации ведения журнала. Используйте параметр --debug, чтобы получить полные журналы отладки. |

## <a name="sfctl-partition-data-loss-status"></a>sfctl partition data-loss-status
Возвращает ход выполнения операции потери данных, запущенной с помощью API StartDataLoss.

Возвращает сведения о ходе выполнения операции с потерей данных, запущенной с помощью StartDataLoss, с использованием OperationId.

### <a name="arguments"></a>Аргументы

|Аргумент|Описание|
| --- | --- |
| --operation-id [обязательный параметр] | Идентификатор GUID, определяющий вызов этого API.  Он передается в соответствующий API GetProgress. |
| --partition-id [обязательный параметр] | Идентификатор секции. |
| --service-id [обязательный параметр] | Идентификатор службы. Обычно он содержит полное имя службы без указания схемы универсального кода ресурса (URI) fabric\:. Начиная с версии 6.0 иерархические имена разделяются знаком \~. Например, если имя службы — fabric\:/myapp/app1/svc1, в версии 6.0 и более поздних версиях будет использоваться идентификатор службы myapp\~app1\~svc1, а в предыдущих версиях — myapp/app1/svc1. |
| --timeout -t | Время ожидания сервера в секундах.  Значение по умолчанию\: 60. |

### <a name="global-arguments"></a>Глобальные аргументы

|Аргумент|Описание|
| --- | --- |
| --debug | Увеличение уровня детализации ведения журнала для отображения всех журналов отладки. |
| --help -h | Отображение этого справочного сообщения и выход. |
| --output -o | Формат выходных данных.  Допустимые значения\: json, jsonc, table, tsv.  Значение по умолчанию\: json. |
| --query | Строка запроса JMESPath. Дополнительные сведения и примеры см. на веб-сайте http\://jmespath.org/. |
| --verbose | Повышение уровня детализации ведения журнала. Используйте параметр --debug, чтобы получить полные журналы отладки. |

## <a name="sfctl-partition-health"></a>sfctl partition health
Возвращает состояние работоспособности указанной секции Service Fabric.

EventsHealthStateFilter используется для фильтрации получаемой коллекции событий работоспособности службы на основе состояния работоспособности. ReplicasHealthStateFilter используется для фильтрации коллекции объектов ReplicaHealthState для секции. Если указанная секция не существует в хранилище данных о работоспособности, то этот запрос возвращает ошибку.

### <a name="arguments"></a>Аргументы

|Аргумент|Описание|
| --- | --- |
| --partition-id [обязательный параметр] | Идентификатор секции. |
| --events-health-state-filter | Позволяет отфильтровать коллекцию возвращаемых объектов HealthEvent по состоянию работоспособности. Возможные значения для этого параметра — целочисленное значение одного из приведенных ниже состояний работоспособности. Возвращаются только те события, которые соответствуют заданному фильтру. Для оценки общего состояния работоспособности используются все события. Если фильтр не указан, возвращаются все записи. Значения состояния — это перечисление на основе флага, поэтому значение может представлять собой сочетание этих значений, полученное с помощью битового оператора OR. Например, если указано значение 6, то возвращаются все события со значениями HealthState "ОК" (2) и "Warning" (4).  <br> -Default — значение по умолчанию. Соответствует любому значению HealthState. Значение равно нулю.  <br> -None — фильтр, который не соответствует какому-либо значению HealthState. Используется для возвращения нуля результатов для определенной коллекции состояний. Значение равно 1.  <br> -ОК — фильтр, извлекающий входные данные с HealthState со значением "Ок". Значение равно 2.  <br> -Warning — фильтр, извлекающий входные данные с HealthState со значением "Warning". Значение равно 4.  <br> -Error — фильтр, извлекающий входные данные с HealthState со значением "Error". Значение равно 8.  <br> -All — фильтр, извлекающий входные данные с любым значением HealthState. Значение равно 65 535. |
| --exclude-health-statistics | Указывает, должна ли возвращаться статистика работоспособности в составе результатов запроса. Значение по умолчанию: false. Статистика содержит число дочерних сущностей в состоянии работоспособности OK, Warning и Error. |
| --replicas-health-state-filter | Позволяет фильтровать коллекцию объектов ReplicaHealthState для секции. Это значение можно получить из элементов HealthStateFilter или с помощью битовых операций с элементами HealthStateFilter. Возвратятся только те реплики, которые соответствуют заданному фильтру. Для оценки общего состояния работоспособности будут использоваться все реплики. Если фильтр не указан, возвратятся все записи. Значения состояния — это перечисление на основе флага. Поэтому значение может представлять собой сочетание этих значений, полученное с помощью битового оператора OR. Например, если указано значение 6, то возвратятся все события со значениями HealthState "OK" (2) и "Warning" (4). Возможные значения для этого параметра — целочисленное значение одного из приведенных ниже состояний работоспособности.  <br> -Default — значение по умолчанию. Соответствует любому значению HealthState. Значение равно нулю.  <br> -None — фильтр, который не соответствует какому-либо значению HealthState. Используется для возвращения нуля результатов для определенной коллекции состояний. Значение равно 1.  <br> -ОК — фильтр, извлекающий входные данные с HealthState со значением "Ок". Значение равно 2.  <br> -Warning — фильтр, извлекающий входные данные с HealthState со значением "Warning". Значение равно 4.  <br> -Error — фильтр, извлекающий входные данные с HealthState со значением "Error". Значение равно 8.  <br> -All — фильтр, извлекающий входные данные с любым значением HealthState. Значение равно 65 535. |
| --timeout -t | Время ожидания сервера в секундах.  Значение по умолчанию\: 60. |

### <a name="global-arguments"></a>Глобальные аргументы

|Аргумент|Описание|
| --- | --- |
| --debug | Увеличение уровня детализации ведения журнала для отображения всех журналов отладки. |
| --help -h | Отображение этого справочного сообщения и выход. |
| --output -o | Формат выходных данных.  Допустимые значения\: json, jsonc, table, tsv.  Значение по умолчанию\: json. |
| --query | Строка запроса JMESPath. Дополнительные сведения и примеры см. на веб-сайте http\://jmespath.org/. |
| --verbose | Повышение уровня детализации ведения журнала. Используйте параметр --debug, чтобы получить полные журналы отладки. |

## <a name="sfctl-partition-info"></a>sfctl partition info
Возвращает сведения о секции Service Fabric.

Возвращает сведения об указанной секции. Ответ содержит идентификатор секции, сведения о схеме секционирования, ключи, поддерживаемые секцией, состояние, данные о работоспособности и другие сведения о секции.

### <a name="arguments"></a>Аргументы

|Аргумент|Описание|
| --- | --- |
| --partition-id [обязательный параметр] | Идентификатор секции. |
| --timeout -t | Время ожидания сервера в секундах.  Значение по умолчанию\: 60. |

### <a name="global-arguments"></a>Глобальные аргументы

|Аргумент|Описание|
| --- | --- |
| --debug | Увеличение уровня детализации ведения журнала для отображения всех журналов отладки. |
| --help -h | Отображение этого справочного сообщения и выход. |
| --output -o | Формат выходных данных.  Допустимые значения\: json, jsonc, table, tsv.  Значение по умолчанию\: json. |
| --query | Строка запроса JMESPath. Дополнительные сведения и примеры см. на веб-сайте http\://jmespath.org/. |
| --verbose | Повышение уровня детализации ведения журнала. Используйте параметр --debug, чтобы получить полные журналы отладки. |

## <a name="sfctl-partition-list"></a>sfctl partition list
Получает список секций службы Service Fabric.

Ответ содержит идентификатор секции, сведения о схеме секционирования, ключи, поддерживаемые секцией, состояние, данные о работоспособности и другие сведения о секции.

### <a name="arguments"></a>Аргументы

|Аргумент|Описание|
| --- | --- |
| --service-id [обязательный параметр] | Идентификатор службы. Обычно он содержит полное имя службы без указания схемы универсального кода ресурса (URI) fabric\:. Начиная с версии 6.0 иерархические имена разделяются знаком \~. Например, если имя службы — fabric\:/myapp/app1/svc1, в версии 6.0 и более поздних версиях будет использоваться идентификатор службы myapp\~app1\~svc1, а в предыдущих версиях — myapp/app1/svc1. |
| --continuation-token | Параметр маркера продолжения используется для получения следующего набора результатов. Маркер продолжения с непустым значением добавляется в ответ API, когда результаты из системы не помещаются в один ответ. Когда это значение передается в следующем вызове API, API возвращает следующий набор результатов. Если результаты отсутствуют, маркер продолжения не будет содержать значение. Значение этого параметра не должно быть указано в формате URL-адреса. |
| --timeout -t | Время ожидания сервера в секундах.  Значение по умолчанию\: 60. |

### <a name="global-arguments"></a>Глобальные аргументы

|Аргумент|Описание|
| --- | --- |
| --debug | Увеличение уровня детализации ведения журнала для отображения всех журналов отладки. |
| --help -h | Отображение этого справочного сообщения и выход. |
| --output -o | Формат выходных данных.  Допустимые значения\: json, jsonc, table, tsv.  Значение по умолчанию\: json. |
| --query | Строка запроса JMESPath. Дополнительные сведения и примеры см. на веб-сайте http\://jmespath.org/. |
| --verbose | Повышение уровня детализации ведения журнала. Используйте параметр --debug, чтобы получить полные журналы отладки. |

## <a name="sfctl-partition-load"></a>sfctl partition load
Возвращает сведения о нагрузке указанной секции Service Fabric.

Возвращает сведения о нагрузке указанной секции. Ответ включает список отчетов по загрузке для секции Service Fabric. Каждый отчет содержит имя метрики нагрузки, значение и время последнего сообщения в формате UTC.

### <a name="arguments"></a>Аргументы

|Аргумент|Описание|
| --- | --- |
| --partition-id [обязательный параметр] | Идентификатор секции. |
| --timeout -t | Время ожидания сервера в секундах.  Значение по умолчанию\: 60. |

### <a name="global-arguments"></a>Глобальные аргументы

|Аргумент|Описание|
| --- | --- |
| --debug | Увеличение уровня детализации ведения журнала для отображения всех журналов отладки. |
| --help -h | Отображение этого справочного сообщения и выход. |
| --output -o | Формат выходных данных.  Допустимые значения\: json, jsonc, table, tsv.  Значение по умолчанию\: json. |
| --query | Строка запроса JMESPath. Дополнительные сведения и примеры см. на веб-сайте http\://jmespath.org/. |
| --verbose | Повышение уровня детализации ведения журнала. Используйте параметр --debug, чтобы получить полные журналы отладки. |

## <a name="sfctl-partition-load-reset"></a>sfctl partition load-reset
Сбрасывает текущую нагрузку секции Service Fabric.

Сбрасывает текущую нагрузку секции Service Fabric к загрузке по умолчанию для службы.

### <a name="arguments"></a>Аргументы

|Аргумент|Описание|
| --- | --- |
| --partition-id [обязательный параметр] | Идентификатор секции. |
| --timeout -t | Время ожидания сервера в секундах.  Значение по умолчанию\: 60. |

### <a name="global-arguments"></a>Глобальные аргументы

|Аргумент|Описание|
| --- | --- |
| --debug | Увеличение уровня детализации ведения журнала для отображения всех журналов отладки. |
| --help -h | Отображение этого справочного сообщения и выход. |
| --output -o | Формат выходных данных.  Допустимые значения\: json, jsonc, table, tsv.  Значение по умолчанию\: json. |
| --query | Строка запроса JMESPath. Дополнительные сведения и примеры см. на веб-сайте http\://jmespath.org/. |
| --verbose | Повышение уровня детализации ведения журнала. Используйте параметр --debug, чтобы получить полные журналы отладки. |

## <a name="sfctl-partition-quorum-loss"></a>sfctl partition quorum-loss
Вызывает потерю кворума в указанной секции службы с отслеживанием состояния.

Этот API полезен в ситуации временной потери кворума в службе. Вызовите API GetQuorumLossProgress с тем же OperationId, чтобы вернуть сведения об операции, запущенной с помощью этого API. Его можно вызвать только в постоянных службах с отслеживанием состояния (HasPersistedState имеет значение true).  Не используйте этот API в службах без отслеживания состояния или в службах с отслеживанием состояния, применяемых только в памяти.

### <a name="arguments"></a>Аргументы

|Аргумент|Описание|
| --- | --- |
| --operation-id [обязательный параметр] | Идентификатор GUID, определяющий вызов этого API.  Он передается в соответствующий API GetProgress. |
| --partition-id [обязательный параметр] | Идентификатор секции. |
| --quorum-loss-duration [обязательный параметр] | Количество времени, в течение которого секция будет находиться в состоянии потери кворума.  Значение должно указываться в секундах. |
| --quorum-loss-mode [обязательный параметр] | Это перечисление передается в API StartQuorumLoss, чтобы указать, какой тип потери кворума следует вызвать. |
| --service-id [обязательный параметр] | Идентификатор службы. Обычно он содержит полное имя службы без указания схемы универсального кода ресурса (URI) fabric\:. Начиная с версии 6.0 иерархические имена разделяются знаком \~. Например, если имя службы — fabric\:/myapp/app1/svc1, в версии 6.0 и более поздних версиях будет использоваться идентификатор службы myapp\~app1\~svc1, а в предыдущих версиях — myapp/app1/svc1. |
| --timeout -t | Время ожидания сервера в секундах.  Значение по умолчанию\: 60. |

### <a name="global-arguments"></a>Глобальные аргументы

|Аргумент|Описание|
| --- | --- |
| --debug | Увеличение уровня детализации ведения журнала для отображения всех журналов отладки. |
| --help -h | Отображение этого справочного сообщения и выход. |
| --output -o | Формат выходных данных.  Допустимые значения\: json, jsonc, table, tsv.  Значение по умолчанию\: json. |
| --query | Строка запроса JMESPath. Дополнительные сведения и примеры см. на веб-сайте http\://jmespath.org/. |
| --verbose | Повышение уровня детализации ведения журнала. Используйте параметр --debug, чтобы получить полные журналы отладки. |

## <a name="sfctl-partition-quorum-loss-status"></a>sfctl partition quorum-loss-status
Возвращает ход выполнения операции потери кворума, запущенной с помощью API StartQuorumLoss.

Возвращает сведения о ходе выполнения операции потери кворума, запущенной с помощью StartQuorumLoss, с использованием предоставленного OperationId.

### <a name="arguments"></a>Аргументы

|Аргумент|Описание|
| --- | --- |
| --operation-id [обязательный параметр] | Идентификатор GUID, определяющий вызов этого API.  Он передается в соответствующий API GetProgress. |
| --partition-id [обязательный параметр] | Идентификатор секции. |
| --service-id [обязательный параметр] | Идентификатор службы. Обычно он содержит полное имя службы без указания схемы универсального кода ресурса (URI) fabric\:. Начиная с версии 6.0 иерархические имена разделяются знаком \~. Например, если имя службы — fabric\:/myapp/app1/svc1, в версии 6.0 и более поздних версиях будет использоваться идентификатор службы myapp\~app1\~svc1, а в предыдущих версиях — myapp/app1/svc1. |
| --timeout -t | Время ожидания сервера в секундах.  Значение по умолчанию\: 60. |

### <a name="global-arguments"></a>Глобальные аргументы

|Аргумент|Описание|
| --- | --- |
| --debug | Увеличение уровня детализации ведения журнала для отображения всех журналов отладки. |
| --help -h | Отображение этого справочного сообщения и выход. |
| --output -o | Формат выходных данных.  Допустимые значения\: json, jsonc, table, tsv.  Значение по умолчанию\: json. |
| --query | Строка запроса JMESPath. Дополнительные сведения и примеры см. на веб-сайте http\://jmespath.org/. |
| --verbose | Повышение уровня детализации ведения журнала. Используйте параметр --debug, чтобы получить полные журналы отладки. |

## <a name="sfctl-partition-recover"></a>sfctl partition recover
Указывает кластеру Service Fabric, что следует попытаться восстановить определенную секцию, которая в настоящее время находится в состоянии потери кворума.

Эту операцию следует выполнять только в том случае, если известно, что неработающие реплики невозможно восстановить. Неправильное использование этого API может привести к потере данных.

### <a name="arguments"></a>Аргументы

|Аргумент|Описание|
| --- | --- |
| --partition-id [обязательный параметр] | Идентификатор секции. |
| --timeout -t | Время ожидания сервера в секундах.  Значение по умолчанию\: 60. |

### <a name="global-arguments"></a>Глобальные аргументы

|Аргумент|Описание|
| --- | --- |
| --debug | Увеличение уровня детализации ведения журнала для отображения всех журналов отладки. |
| --help -h | Отображение этого справочного сообщения и выход. |
| --output -o | Формат выходных данных.  Допустимые значения\: json, jsonc, table, tsv.  Значение по умолчанию\: json. |
| --query | Строка запроса JMESPath. Дополнительные сведения и примеры см. на веб-сайте http\://jmespath.org/. |
| --verbose | Повышение уровня детализации ведения журнала. Используйте параметр --debug, чтобы получить полные журналы отладки. |

## <a name="sfctl-partition-recover-all"></a>sfctl partition recover-all
Указывает кластеру Service Fabric, что следует попытаться восстановить все службы (в том числе системные), которые в настоящее время находятся в состоянии потери кворума.

Эту операцию следует выполнять только в том случае, если известно, что неработающие реплики невозможно восстановить. Неправильное использование этого API может привести к потере данных.

### <a name="arguments"></a>Аргументы

|Аргумент|Описание|
| --- | --- |
| --timeout -t | Время ожидания сервера в секундах.  Значение по умолчанию\: 60. |

### <a name="global-arguments"></a>Глобальные аргументы

|Аргумент|Описание|
| --- | --- |
| --debug | Увеличение уровня детализации ведения журнала для отображения всех журналов отладки. |
| --help -h | Отображение этого справочного сообщения и выход. |
| --output -o | Формат выходных данных.  Допустимые значения\: json, jsonc, table, tsv.  Значение по умолчанию\: json. |
| --query | Строка запроса JMESPath. Дополнительные сведения и примеры см. на веб-сайте http\://jmespath.org/. |
| --verbose | Повышение уровня детализации ведения журнала. Используйте параметр --debug, чтобы получить полные журналы отладки. |

## <a name="sfctl-partition-report-health"></a>sfctl partition report-health
Отправляет отчет о работоспособности секции Service Fabric.

Возвращает состояние работоспособности для указанной секции Service Fabric. Отчет должен содержать информацию об источнике отчета о работоспособности и свойство, в котором об этом сообщается. Отчет отправляется в секцию шлюза Service Fabric, которая передает его в хранилище данных о работоспособности. Отчет может быть принят шлюзом, но затем отклонен хранилищем данных о работоспособности после дополнительной проверки. Например, хранилище данных о работоспособности может отклонить отчет из-за недопустимого параметра, например устаревшего порядкового номера. Чтобы проверить, применен ли отчет в хранилище данных о работоспособности, найдите этот отчет в разделе событий.

### <a name="arguments"></a>Аргументы

|Аргумент|Описание|
| --- | --- |
| --health-property [обязательный параметр] | Свойство информации о работоспособности. <br><br> Один объект может возвращать отчеты о работоспособности для разных свойств. Это свойство имеет строковый формат, а не фиксированное перечисление, чтобы создатель отчетов мог гибко классифицировать состояние, активировавшее отчет. Например, создатель отчета с идентификатором источника LocalWatchdog может отслеживать состояние доступного дискового пространства на узле и передавать для этого узла свойство AvailableDisk. Этот же создатель отчета может отслеживать подключения узла и передавать свойство Connectivity для того же узла. В хранилище данных о работоспособности эти отчеты обрабатываются как независимые события работоспособности для указанного узла. Кроме идентификатора источника, свойство однозначно идентифицирует данные о работоспособности. |
| --health-state [обязательный параметр] | Допускаются следующие значения\: Invalid, OK, Warning, Error или Unknown. |
| --partition-id [обязательный параметр] | Идентификатор секции. |
| --source-id [обязательный параметр] | Имя источника, которое определяет клиента, службу наблюдения или компонент системы, создавший информацию о работоспособности. |
| --description | Описание информации о работоспособности. <br><br> Это свойство предоставляет информацию об отчете в произвольном текстовом формате, доступном для чтения. Максимальная длина строки для описания составляет 4096 символов. Более длинные строки автоматически усекаются. При усечении в последние символы описания помещается маркер [Truncated], длина которого входит в строку длиной 4096 символов. Наличие маркера указывает на усечение. Обратите внимание, что усеченное описание включает менее 4096 символов исходной строки. |
| --immediate | Этот флаг обозначает, что отчет нужно отправить немедленно. <br><br> Отчет о работоспособности отправляется в приложение шлюза Service Fabric, которое передает его в хранилище данных о работоспособности. Если флагу Immediate задано значение true, отчет немедленно отправляется через шлюз HTTP в хранилище данных о работоспособности, независимо от параметров клиента структуры, которые использует приложение шлюза HTTP. Это полезно для критически важных отчетов, которые необходимо передать как можно быстрее. В зависимости от времени и других условий отправка отчета может завершиться сбоем, например, если шлюз HTTP закрыт или сообщение не достигает этого шлюза. Если флагу Immediate задано значение false, отчет отправляется с учетом параметров клиента работоспособности через шлюз HTTP. Это значит, что он будет включен в пакет отчетов в соответствии со значением параметра HealthReportSendInterval. Рекомендуется применять именно такой вариант, так как он позволяет клиенту работоспособности оптимизировать отправку отчетов в хранилище данных о работоспособности, а также обработку отчетов о работоспособности. По умолчанию отчеты не отправляются немедленно. |
| --remove-when-expired | Это значение указывает, удаляется ли отчет из хранилища работоспособности после истечения срока его действия. <br><br> Если задано значение true, отчет будет удален из хранилища работоспособности после истечения срока его действия. Если задано значение false, отчет с истекшим сроком действия рассматривается как указывающий на ошибку. Для этого свойства по умолчанию используется значение false. Если клиенты создают периодические отчеты, для параметра RemoveWhenExpired следует использовать значение false (по умолчанию). Если возникнет проблема с создателем отчетов (например, взаимоблокировка), не позволяющая ему отправить следующий отчет, это позволит зарегистрировать ошибку для сущности с истекшим отчетом о работоспособности. Такая сущность будет находиться в состоянии "Ошибка работоспособности". |
| --sequence-number | Порядковый номер отчета о работоспособности в формате числовой строки. <br><br> Порядковый номер отчета используется хранилищем данных о работоспособности для отслеживания устаревших отчетов. Если значение не указано, порядковый номер автоматически создается клиентом работоспособности при добавлении отчета. |
| --timeout -t | Время ожидания сервера в секундах.  Значение по умолчанию\: 60. |
| --ttl | Срок, в течение которого отчет о работоспособности считается действительным. В этом поле указывается длительность в формате ISO8601. <br><br> Если клиенты создают периодические отчеты, периодичность отправки отчетов должна быть выше, чем срок жизни отчетов. Если клиенты передают отчеты об изменении состояния, для них можно установить бесконечный срок действия. По истечении срока жизни событие работоспособности, которое содержит данные о работоспособности, удаляется из хранилища данных о работоспособности (если RemoveWhenExpired имеет значение true) или считается ошибкой (если RemoveWhenExpired имеет значение false). Если значение не указано, по умолчанию устанавливается бесконечный срок жизни. |

### <a name="global-arguments"></a>Глобальные аргументы

|Аргумент|Описание|
| --- | --- |
| --debug | Увеличение уровня детализации ведения журнала для отображения всех журналов отладки. |
| --help -h | Отображение этого справочного сообщения и выход. |
| --output -o | Формат выходных данных.  Допустимые значения\: json, jsonc, table, tsv.  Значение по умолчанию\: json. |
| --query | Строка запроса JMESPath. Дополнительные сведения и примеры см. на веб-сайте http\://jmespath.org/. |
| --verbose | Повышение уровня детализации ведения журнала. Используйте параметр --debug, чтобы получить полные журналы отладки. |

## <a name="sfctl-partition-restart"></a>sfctl partition restart
Этот API перезапустит некоторые или все реплики или экземпляры указанной секции.

Этот API удобен для тестирования отработки отказа. Если он используется для секции службы без отслеживания состояния, то параметр RestartPartitionMode должен иметь значение AllReplicasOrInstances. Вызовите API GetPartitionRestartProgress, указав то же значение OperationId, чтобы узнать ход выполнения.

### <a name="arguments"></a>Аргументы

|Аргумент|Описание|
| --- | --- |
| --operation-id [обязательный параметр] | Идентификатор GUID, определяющий вызов этого API.  Он передается в соответствующий API GetProgress. |
| --partition-id [обязательный параметр] | Идентификатор секции. |
| --restart-partition-mode [обязательный параметр] | Указывает, какие секции нужно перезапустить. |
| --service-id [обязательный параметр] | Идентификатор службы. Обычно он содержит полное имя службы без указания схемы универсального кода ресурса (URI) fabric\:. Начиная с версии 6.0 иерархические имена разделяются знаком \~. Например, если имя службы — fabric\:/myapp/app1/svc1, в версии 6.0 и более поздних версиях будет использоваться идентификатор службы myapp\~app1\~svc1, а в предыдущих версиях — myapp/app1/svc1. |
| --timeout -t | Время ожидания сервера в секундах.  Значение по умолчанию\: 60. |

### <a name="global-arguments"></a>Глобальные аргументы

|Аргумент|Описание|
| --- | --- |
| --debug | Увеличение уровня детализации ведения журнала для отображения всех журналов отладки. |
| --help -h | Отображение этого справочного сообщения и выход. |
| --output -o | Формат выходных данных.  Допустимые значения\: json, jsonc, table, tsv.  Значение по умолчанию\: json. |
| --query | Строка запроса JMESPath. Дополнительные сведения и примеры см. на веб-сайте http\://jmespath.org/. |
| --verbose | Повышение уровня детализации ведения журнала. Используйте параметр --debug, чтобы получить полные журналы отладки. |

## <a name="sfctl-partition-restart-status"></a>sfctl partition restart-status
Возвращает ход выполнения операции PartitionRestart, запущенной с помощью StartPartitionRestart.

Возвращает сведения о ходе выполнения PartitionRestart, запущенного с помощью StartPartitionRestart, с использованием предоставленного OperationId.

### <a name="arguments"></a>Аргументы

|Аргумент|Описание|
| --- | --- |
| --operation-id [обязательный параметр] | Идентификатор GUID, определяющий вызов этого API.  Он передается в соответствующий API GetProgress. |
| --partition-id [обязательный параметр] | Идентификатор секции. |
| --service-id [обязательный параметр] | Идентификатор службы. Обычно он содержит полное имя службы без указания схемы универсального кода ресурса (URI) fabric\:. Начиная с версии 6.0 иерархические имена разделяются знаком \~. Например, если имя службы — fabric\:/myapp/app1/svc1, в версии 6.0 и более поздних версиях будет использоваться идентификатор службы myapp\~app1\~svc1, а в предыдущих версиях — myapp/app1/svc1. |
| --timeout -t | Время ожидания сервера в секундах.  Значение по умолчанию\: 60. |

### <a name="global-arguments"></a>Глобальные аргументы

|Аргумент|Описание|
| --- | --- |
| --debug | Увеличение уровня детализации ведения журнала для отображения всех журналов отладки. |
| --help -h | Отображение этого справочного сообщения и выход. |
| --output -o | Формат выходных данных.  Допустимые значения\: json, jsonc, table, tsv.  Значение по умолчанию\: json. |
| --query | Строка запроса JMESPath. Дополнительные сведения и примеры см. на веб-сайте http\://jmespath.org/. |
| --verbose | Повышение уровня детализации ведения журнала. Используйте параметр --debug, чтобы получить полные журналы отладки. |

## <a name="sfctl-partition-svc-name"></a>sfctl partition svc-name
Возвращает имя службы Service Fabric для секции.

Возвращает имя службы для заданной секции. Возвращается сообщение об ошибке 404, если идентификатор секции не существует в кластере.

### <a name="arguments"></a>Аргументы

|Аргумент|Описание|
| --- | --- |
| --partition-id [обязательный параметр] | Идентификатор секции. |
| --timeout -t | Время ожидания сервера в секундах.  Значение по умолчанию\: 60. |

### <a name="global-arguments"></a>Глобальные аргументы

|Аргумент|Описание|
| --- | --- |
| --debug | Увеличение уровня детализации ведения журнала для отображения всех журналов отладки. |
| --help -h | Отображение этого справочного сообщения и выход. |
| --output -o | Формат выходных данных.  Допустимые значения\: json, jsonc, table, tsv.  Значение по умолчанию\: json. |
| --query | Строка запроса JMESPath. Дополнительные сведения и примеры см. на веб-сайте http\://jmespath.org/. |
| --verbose | Повышение уровня детализации ведения журнала. Используйте параметр --debug, чтобы получить полные журналы отладки. |


## <a name="next-steps"></a>Дальнейшие действия
- [Настройте](service-fabric-cli.md) интерфейс командной строки Service Fabric.
- Узнайте, как использовать интерфейс командной строки Service Fabric, с помощью [примеров сценариев](/azure/service-fabric/scripts/sfctl-upgrade-application).
