---
title: Общие сведения о центре безопасности Azure для рекомендаций по обеспечению безопасности Интернета вещей предварительной версии | Документация Майкрософт
description: Дополнительные сведения о концепции рекомендации по обеспечению безопасности и их использовании в центре безопасности Azure для Интернета вещей.
services: asc-for-iot
ms.service: asc-for-iot
documentationcenter: na
author: mlottner
manager: rkarlin
editor: ''
ms.assetid: 02ced504-d3aa-4770-9d10-b79f80af366c
ms.subservice: asc-for-iot
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/24/2019
ms.author: mlottner
ms.openlocfilehash: 1ee71bbacdba7a14e94de41563a04be9c0f00d13
ms.sourcegitcommit: 6a42dd4b746f3e6de69f7ad0107cc7ad654e39ae
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/07/2019
ms.locfileid: "67618413"
---
# <a name="security-recommendations"></a>Рекомендации по обеспечению безопасности

> [!IMPORTANT]
> Центр безопасности Azure для Интернета вещей сейчас предоставляется в общедоступной предварительной версии.
> Эта предварительная версия предоставляется без соглашения об уровне обслуживания и не рекомендована для использования рабочей среде. Некоторые функции могут не поддерживаться или их возможности могут быть ограничены. Дополнительные сведения см. в статье [Дополнительные условия использования предварительных выпусков Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Центр безопасности Azure (ASC) для Интернета вещей проверяет ваших ресурсов Azure и устройств Интернета вещей и предоставляет рекомендации по обеспечению безопасности, чтобы уменьшить поверхность атаки. Рекомендации по обеспечению безопасности требуют действий и стремиться к тому, чтобы помочь клиентам в соответствие рекомендациям по обеспечению безопасности.

В этой статье вы найдете список рекомендаций, которые можно запустить на центр Интернета вещей и/или устройств Интернета вещей.

## <a name="recommendations-for-iot-devices"></a>Рекомендации для устройств Интернета вещей

Рекомендации устройства обеспечить полезные сведения и предложения по повышению уровня безопасности устройства. 

| severity | ИМЯ                                                      | источника данных | Описание                                                                                                                                                                                           |
|----------|-----------------------------------------------------------|-------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Средний   | Открыть порты на устройстве                                      | Агент       | Удалось найти конечную точку прослушивания на устройстве                                                                                                                                                          |
| Средний   | Политика брандмауэра широкие права, по одному из цепочек. | Агент       | Допускается политики брандмауэра, найти (ввода-ВЫВОДА). Политика брандмауэра следует запретить весь трафик по умолчанию, а также определить правила, чтобы разрешить необходимое взаимодействие с устройства.                               |
| Средний   | Правило брандмауэра широкие права в цепочке входной найдено     | Агент       | Правило в брандмауэре были обнаружены, содержащее широкие шаблон для широкого диапазона IP-адресов или портов.                                                                                    |
| Средний   | Удалось найти правило брандмауэра широкие права в цепочке выходных данных    | Агент       | Правило в брандмауэре были обнаружены, содержащее широкие шаблон для широкого диапазона IP-адресов или портов.                                                                                   |
| Средний   | Сбой проверки базовой операции системы           | Агент       | Устройство не соответствует [CIS Linux тесты производительности](https://www.cisecurity.org/cis-benchmarks/)                                                                                                         |

### <a name="operational-recommendations-for-iot-devices"></a>Рекомендации по эксплуатации для устройств Интернета вещей

Рекомендации по эксплуатации обеспечить полезные сведения и предложения по улучшению безопасности конфигурации агента.

| severity | ИМЯ                                    | источника данных | Описание                                                                       |
|----------|-----------------------------------------|-------------|-----------------------------------------------------------------------------------|
| Низкий      | Агент отправляет неиспользуемой сообщения          | Агент       | 10% или более сообщений системы безопасности были меньше 4 КБ за последние 24 часа.  |
| Низкий      | Не оптимальной конфигурации двойника безопасности | Агент       | Настройка безопасности двойника не является оптимальным.                                        |
| Низкий      | Конфликт конфигурации безопасности двойника    | Агент       | Конфликты были определены в конфигурации безопасности двойника.                           |


## <a name="recommendations-for-iot-hub"></a>Рекомендации для центра Интернета вещей

Рекомендация оповещения содержат информацию и предложения для действий повысить уровень защищенности вашей среды.  

| severity | ИМЯ                                                     | источника данных | Описание                                                                                                                                                                                                             |
|----------|----------------------------------------------------------|-------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Высокий     | Идентичные проверки подлинности учетные данные, используемые несколькими устройствами | Центр Интернета вещей     | Учетные данные проверки подлинности центра Интернета вещей потребленной несколькими устройствами. Это может означать устройство незаконные олицетворения законным устройства. Использование повторяющихся учетных данных повышает риск олицетворения устройства вредоносным субъектом. |
| Средний   | Политика фильтрации IP-адрес по умолчанию должен запретить                  | Центр Интернета вещей     | Фильтр IP-конфигурации должны иметь правила, определенные для разрешенный трафик и следует по умолчанию, запрет весь остальной трафик по умолчанию.                                                                                                     |
| Средний   | Правила фильтрации IP-адрес включает в себя большой диапазон IP-адресов                   | Центр Интернета вещей     | Разрешить IP-адрес фильтр правила исходный диапазон IP-адресов, слишком велик. Либеральная правила можно предоставить центру Интернета вещей для злоумышленников.                                                                                       |
| Низкий      | Включение журналов диагностики в центре Интернета вещей                       | Центр Интернета вещей     | Включите журналы и сохраняйте до года. Сохранение журналов позволяет повторно создать действие след в целях контроля при возникновении инцидента безопасности или несанкционированного доступа к сети.                                       |
|