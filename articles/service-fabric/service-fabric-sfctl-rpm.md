---
title: 'Интерфейс командной строки Azure Service Fabric : sfctl rpm | Документация Майкрософт'
description: Описание команд sfctl rpm интерфейса командной строки Azure Service Fabric.
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
ms.openlocfilehash: 04080d75042bfa8a07533336936165e0abef051b
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "60556361"
---
# <a name="sfctl-rpm"></a>sfctl rpm
Запрос службы диспетчера восстановления и отправка команд для нее.

## <a name="commands"></a>Команды

|Команда|Описание|
| --- | --- |
| approve-force | Вызывает принудительное утверждение заданной задачи исправления. |
| delete | Удаляет завершенную задачу исправления. |
| list | Возвращает список задач исправления, соответствующих заданным фильтрам. |

## <a name="sfctl-rpm-approve-force"></a>sfctl rpm approve-force
Вызывает принудительное утверждение заданной задачи исправления.

Этот API поддерживает платформу Service Fabric. Он не предназначен для использования непосредственно в коде.

### <a name="arguments"></a>Аргументы

|Аргумент|Описание|
| --- | --- |
| --task-id [обязательный параметр] | Идентификатор задачи восстановления. |
| --version | Текущий номер версии задачи исправления. Если это значение не равно нулю, то запрос будет успешно выполнен только в том случае, если это значение соответствует фактической текущей версии задачи исправления. Если значение равно нулю, то проверка версии не выполняется. |

### <a name="global-arguments"></a>Глобальные аргументы

|Аргумент|Описание|
| --- | --- |
| --debug | Увеличение уровня детализации ведения журнала для отображения всех журналов отладки. |
| --help -h | Отображение этого справочного сообщения и выход. |
| --output -o | Формат выходных данных.  Допустимые значения\: json, jsonc, table, tsv.  Значение по умолчанию\: json. |
| --query | Строка запроса JMESPath. Дополнительные сведения и примеры см. на веб-сайте http\://jmespath.org/. |
| --verbose | Повышение уровня детализации ведения журнала. Используйте параметр --debug, чтобы получить полные журналы отладки. |

## <a name="sfctl-rpm-delete"></a>sfctl rpm delete
Удаляет завершенную задачу исправления.

Этот API поддерживает платформу Service Fabric. Он не предназначен для использования непосредственно в коде.

### <a name="arguments"></a>Аргументы

|Аргумент|Описание|
| --- | --- |
| --task-id [обязательный параметр] | Идентификатор завершенной задачи исправления, которая удаляется. |
| --version | Текущий номер версии задачи исправления. Если это значение не равно нулю, то запрос будет успешно выполнен только в том случае, если это значение соответствует фактической текущей версии задачи исправления. Если значение равно нулю, то проверка версии не выполняется. |

### <a name="global-arguments"></a>Глобальные аргументы

|Аргумент|Описание|
| --- | --- |
| --debug | Увеличение уровня детализации ведения журнала для отображения всех журналов отладки. |
| --help -h | Отображение этого справочного сообщения и выход. |
| --output -o | Формат выходных данных.  Допустимые значения\: json, jsonc, table, tsv.  Значение по умолчанию\: json. |
| --query | Строка запроса JMESPath. Дополнительные сведения и примеры см. на веб-сайте http\://jmespath.org/. |
| --verbose | Повышение уровня детализации ведения журнала. Используйте параметр --debug, чтобы получить полные журналы отладки. |

## <a name="sfctl-rpm-list"></a>sfctl rpm list
Возвращает список задач исправления, соответствующих заданным фильтрам.

Этот API поддерживает платформу Service Fabric. Он не предназначен для использования непосредственно в коде.

### <a name="arguments"></a>Аргументы

|Аргумент|Описание|
| --- | --- |
| --executor-filter | Имя исполнителя исправления, запрошенные задачи которого должны быть включены в список. |
| --state-filter | Битовая операция "ИЛИ" с приведенными ниже значениями, результат которой указывает, какие задачи должны быть включены в список результатов. <br> 1 — Created <br>2 — Claimed  <br>4 — Preparing  <br>8 — Approved  <br>16 — Executing  <br>32 — Restoring  <br>64 — Completed |
| --task-id-filter | Префикс идентификатора задачи исправления для сравнения. |

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