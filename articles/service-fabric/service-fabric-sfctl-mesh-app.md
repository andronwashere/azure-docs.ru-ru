---
title: 'Интерфейс командной строки Azure Service Fabric : -sfctl mesh app | Документация Майкрософт'
description: Описание команд sfctl mesh app интерфейса командной строки Service Fabric.
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
ms.openlocfilehash: fb812412c7dd07800c1e2231c9472a122ab7d7d4
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "60837289"
---
# <a name="sfctl-mesh-app"></a>sfctl mesh app
Получение и удаление ресурсов приложения.

## <a name="commands"></a>Команды

|Команда|Описание|
| --- | --- |
| delete | Удаляет ресурс приложения. |
| list | Выводит список всех ресурсов приложения. |
| show | Получает ресурс приложения с заданным именем. |

## <a name="sfctl-mesh-app-delete"></a>sfctl mesh app delete
Удаляет ресурс приложения.

Удаляет ресурс приложения, идентифицированного по имени.

### <a name="arguments"></a>Аргументы

|Аргумент|Описание|
| --- | --- |
| --name -n [обязательный параметр] | Имя приложения. |

### <a name="global-arguments"></a>Глобальные аргументы

|Аргумент|Описание|
| --- | --- |
| --debug | Увеличение уровня детализации ведения журнала для отображения всех журналов отладки. |
| --help -h | Отображение этого справочного сообщения и выход. |
| --output -o | Формат выходных данных.  Допустимые значения\: json, jsonc, table, tsv.  Значение по умолчанию\: json. |
| --query | Строка запроса JMESPath. Дополнительные сведения и примеры см. на веб-сайте http\://jmespath.org/. |
| --verbose | Повышение уровня детализации ведения журнала. Используйте параметр --debug, чтобы получить полные журналы отладки. |

## <a name="sfctl-mesh-app-list"></a>sfctl mesh app list
Выводит список всех ресурсов приложения.

Возвращает сведения обо всех ресурсах приложения в указанной группе ресурсов. Сведения содержат описание и другие свойства приложения.

### <a name="global-arguments"></a>Глобальные аргументы

|Аргумент|Описание|
| --- | --- |
| --debug | Увеличение уровня детализации ведения журнала для отображения всех журналов отладки. |
| --help -h | Отображение этого справочного сообщения и выход. |
| --output -o | Формат выходных данных.  Допустимые значения\: json, jsonc, table, tsv.  Значение по умолчанию\: json. |
| --query | Строка запроса JMESPath. Дополнительные сведения и примеры см. на веб-сайте http\://jmespath.org/. |
| --verbose | Повышение уровня детализации ведения журнала. Используйте параметр --debug, чтобы получить полные журналы отладки. |

## <a name="sfctl-mesh-app-show"></a>sfctl mesh app show
Получает ресурс приложения с заданным именем.

Получение сведений о ресурсе приложения с заданным именем. Сведения содержат описание и другие свойства приложения.

### <a name="arguments"></a>Аргументы

|Аргумент|Описание|
| --- | --- |
| --name -n [обязательный параметр] | Имя приложения. |

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