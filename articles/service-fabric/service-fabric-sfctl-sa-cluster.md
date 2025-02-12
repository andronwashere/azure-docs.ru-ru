---
title: Интерфейс командной строки Azure Service Fabric. sfctl sa-cluster | Документация Майкрософт
description: Описание команд автономного кластера sfctl интерфейса командной строки Azure Service Fabric.
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
ms.openlocfilehash: a652439729e538b3ce2545ab3b09284e6645ce9d
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "60556395"
---
# <a name="sfctl-sa-cluster"></a>Кластер sfctl sa-cluster
Управление автономными кластерами Service Fabric.

## <a name="commands"></a>Команды

|Команда|Описание|
| --- | --- |
| config | Получение конфигурации автономного кластера Service Fabric. |
| config-upgrade | Запуск обновления конфигурации автономного кластера Service Fabric. |
| upgrade-status | Получение состояния обновления конфигурации автономного кластера Service Fabric. |

## <a name="sfctl-sa-cluster-config"></a>sfctl sa-cluster config
Получение конфигурации автономного кластера Service Fabric.

Конфигурация кластера содержит свойства кластера, которые включают в себя различные типы узлов в кластере, конфигурацию безопасности, топологии доменов сбоя и доменов обновления и т. д.

### <a name="arguments"></a>Аргументы

|Аргумент|Описание|
| --- | --- |
| --configuration-api-version [обязательный параметр] | Версия API конфигурации json автономного кластера. |
| --timeout -t | Время ожидания сервера в секундах.  Значение по умолчанию\: 60. |

### <a name="global-arguments"></a>Глобальные аргументы

|Аргумент|Описание|
| --- | --- |
| --debug | Увеличение уровня детализации ведения журнала для отображения всех журналов отладки. |
| --help -h | Отображение этого справочного сообщения и выход. |
| --output -o | Формат выходных данных.  Допустимые значения\: json, jsonc, table, tsv.  Значение по умолчанию\: json. |
| --query | Строка запроса JMESPath. Дополнительные сведения и примеры см. на веб-сайте http\://jmespath.org/. |
| --verbose | Повышение уровня детализации ведения журнала. Используйте параметр --debug, чтобы получить полные журналы отладки. |

## <a name="sfctl-sa-cluster-config-upgrade"></a>sfctl sa-cluster config-upgrade
Запуск обновления конфигурации автономного кластера Service Fabric.

Проверка указанных параметров обновления поставляемой конфигурации и запуск обновления конфигурации кластера, если параметры действительны.

### <a name="arguments"></a>Аргументы

|Аргумент|Описание|
| --- | --- |
| --cluster-config            [обязательный параметр] | Конфигурация кластера. |
| --application-health-policies | Словарь в формате JSON, содержащий пары из имени типа приложения и максимального процента неработоспособности, по достижении которого порождается ошибка. |
| --delta-unhealthy-nodes | Максимальный процент разностной оценки неработоспособных узлов, допускаемый во время обновления кластера. Допустимые значения — это целые значения от нуля до 100. |
| --health-check-retry | Продолжительность времени между попытками выполнить проверку работоспособности, если приложение или кластер неработоспособны.  Значение по умолчанию\: PT0H0M0S. |
| --health-check-stable | Период времени, в течение которого приложение или кластер должен оставаться работоспособным, прежде чем процесс обновления перейдет к следующему домену обновления.  Значение по умолчанию\: PT0H0M0S. <br><br> Сначала он интерпретируется как строка, представляющая длительность ISO 8601. В случае неудачи он интерпретируется как число, представляющее общее количество миллисекунд. |
| --health-check-wait | Продолжительность ожидания после завершения обновления домена до начала процесса проверки работоспособности.  Значение по умолчанию\: PT0H0M0S. |
| --timeout -t | Время ожидания сервера в секундах.  Значение по умолчанию\: 60. |
| --unhealthy-applications | Максимально допустимый процент работоспособных приложений во время обновления. Допустимые значения — это целые значения от нуля до 100. |
| --unhealthy-nodes | Максимально допустимый процент работоспособных узлов во время обновления. Допустимые значения — это целые значения от нуля до 100. |
| --upgrade-domain-delta-unhealthy-nodes | Максимальный процент разностной оценки ухудшения производительности доменов обновления, допускаемый во время обновления. Допустимые значения — это целые значения от нуля до 100. |
| --upgrade-domain-timeout | Время выполнения каждого домена обновления, по истечении которого выполняется действие FailureAction.  Значение по умолчанию\: PT0H0M0S. <br><br> Сначала он интерпретируется как строка, представляющая длительность ISO 8601. В случае неудачи он интерпретируется как число, представляющее общее количество миллисекунд. |
| --upgrade-timeout | Общее время завершения обновления, по истечении которого выполняется действие FailureAction.  Значение по умолчанию\: PT0H0M0S. <br><br> Сначала он интерпретируется как строка, представляющая длительность ISO 8601. В случае неудачи он интерпретируется как число, представляющее общее количество миллисекунд. |

### <a name="global-arguments"></a>Глобальные аргументы

|Аргумент|Описание|
| --- | --- |
| --debug | Увеличение уровня детализации ведения журнала для отображения всех журналов отладки. |
| --help -h | Отображение этого справочного сообщения и выход. |
| --output -o | Формат выходных данных.  Допустимые значения\: json, jsonc, table, tsv.  Значение по умолчанию\: json. |
| --query | Строка запроса JMESPath. Дополнительные сведения и примеры см. на веб-сайте http\://jmespath.org/. |
| --verbose | Повышение уровня детализации ведения журнала. Используйте параметр --debug, чтобы получить полные журналы отладки. |

### <a name="examples"></a>Примеры

Запуск обновления конфигурации кластера

```
sfctl sa-cluster config-upgrade --cluster-config <YOUR CLUSTER CONFIG> --application-health-
policies "{"fabric:/System":{"ConsiderWarningAsError":true}}"
```

## <a name="sfctl-sa-cluster-upgrade-status"></a>sfctl sa-cluster upgrade-status
Получение состояния обновления конфигурации автономного кластера Service Fabric.

Получение подробностей состояния обновления конфигурации автономного кластера Service Fabric.

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


## <a name="next-steps"></a>Дальнейшие действия
- [Настройте](service-fabric-cli.md) интерфейс командной строки Service Fabric.
- Узнайте, как использовать интерфейс командной строки Service Fabric, с помощью [примеров сценариев](/azure/service-fabric/scripts/sfctl-upgrade-application).