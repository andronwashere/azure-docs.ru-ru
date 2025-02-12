---
title: Подключение данных событий безопасности Windows для предварительной версии Sentinel Azure | Документация Майкрософт
description: Узнайте, как подключиться к Azure Sentinel данных событий безопасности Windows.
services: sentinel
documentationcenter: na
author: rkarlin
manager: rkarlin
editor: ''
ms.assetid: d51d2e09-a073-41c8-b396-91d60b057e6a
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/17/2019
ms.author: rkarlin
ms.openlocfilehash: 188febf090ddb3f685f9d3c3b94d822f15bbcfcb
ms.sourcegitcommit: 80aaf27e3ad2cc4a6599a3b6af0196c6239e6918
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/09/2019
ms.locfileid: "67673765"
---
# <a name="connect-windows-security-events"></a>Подключение событий безопасности Windows 

> [!IMPORTANT]
> Сейчас Azure Sentinel предоставляется в общедоступной предварительной версии.
> Эта предварительная версия предоставляется без соглашения об уровне обслуживания и не рекомендована для использования рабочей среде. Некоторые функции могут не поддерживаться или их возможности могут быть ограничены. Дополнительные сведения см. в статье [Дополнительные условия использования предварительных выпусков Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Вы можете передавать все события безопасности с серверов Windows, подключенных к рабочей области Azure Sentinel. Это подключение позволяет просматривать панели мониторинга, создать настраиваемые оповещения и улучшит исследование. Это дает больше возможностей регулировать сети вашей организации и улучшает возможности безопасности операции.  Вы можете выбрать, какие события для потоковой передачи:

- **Все события** -безопасность всех Windows и события AppLocker.
- **Распространенные** -стандартный набор событий для аудита. Полный журнал аудита пользователя включается в этот набор. Например этот набор содержит вход пользователя и пользователя на выход события (идентификатор события 4634). Мы добавили действия аудита, такие как изменение группы безопасности, операции основного контроллера домена Kerberos и другие события, рекомендованные отраслевыми организациями.

События небольших объемов были включены в набор "Общий". Основной причиной для его выбора среди всех событий является возможность уменьшить объем и оставить определенные события нефильтрованными.
- **Минимальный** -небольшой набор событий, которые могут указывать на потенциальные угрозы. Включив этот параметр, нельзя будет иметь полный журнал аудита.  Этот набор содержит только те события, которые могут указывать на брешь и важные события небольших объемов. Например этот набор содержит успешных и неудачных входа пользователя (идентификаторы события 4624 и 4625), но она не содержит выхода важно для аудита, но не может применяться для обнаружения и относительно большой объем информации. Основная часть данных этого набора является знак в события и события создания процесса (событие ID 4688).
- **Нет** -нет безопасности и события AppLocker.

> [!NOTE]
> 
> - Данные будут храниться в географическое расположение рабочей области, на котором выполняется Azure Sentinel.

Ниже приведены полный обзор безопасности и App Locker идентификаторы событий для каждого набора:

| Уровень данных | Собранные индикаторы событий |
| --- | --- |
| Небольшие | 1102,4624,4625,4657,4663,4688,4700,4702,4719,4720,4722,4723,4724,4727,4728,4732,4735,4737,4739,4740,4754,4755, |
| | 4756,4767,4799,4825,4946,4948,4956,5024,5033,8001,8002,8003,8004,8005,8006,8007,8222 |
| Common | 1,299,300,324,340,403,404,410,411,412,413,431,500,501,1100,1102,1107,1108,4608,4610,4611,4614,4622, |
| |  4624,4625,4634,4647,4648,4649,4657,4661,4662,4663,4665,4666,4667,4688,4670,4672,4673,4674,4675,4689,4697, |
| | 4700,4702,4704,4705,4716,4717,4718,4719,4720,4722,4723,4724,4725,4726,4727,4728,4729,4733,4732,4735,4737, |
| | 4738,4739,4740,4742,4744,4745,4746,4750,4751,4752,4754,4755,4756,4757,4760,4761,4762,4764,4767,4768,4771, |
| | 4774,4778,4779,4781,4793,4797,4798,4799,4800,4801,4802,4803,4825,4826,4870,4886,4887,4888,4893,4898,4902, |
| | 4904,4905,4907,4931,4932,4933,4946,4948,4956,4985,5024,5033,5059,5136,5137,5140,5145,5632,6144,6145,6272, |
| | 6273,6278,6416,6423,6424,8001,8002,8003,8004,8005,8006,8007,8222,26401,30004 |

## <a name="set-up-the-windows-security-events-connector"></a>Настройка соединителя событий безопасности Windows

Чтобы полностью интегрировать события системы безопасности Windows в Azure Sentinel:

1. На портале Azure Sentinel выберите **соединители данных** и выберите команду **событий безопасности Windows** плитку. 
1. Выберите типы данных для потоковой передачи.
1. Нажмите кнопку **Обновить**.
6. Для соответствующей схемы в Log Analytics можно использовать для событий безопасности Windows, найдите **SecurityEvent**.

## <a name="validate-connectivity"></a>Проверка подключения

Может занять около 20 минут, пока не журналов в Log Analytics. 



## <a name="next-steps"></a>Следующие шаги
В этом документе вы узнали, как подключиться к Azure Sentinel событий безопасности Windows. Ознакомьтесь с дополнительными сведениями об Azure Sentinel в соответствующих статьях.
- Узнайте, как [получить представление о данных и потенциальные угрозы](quickstart-get-visibility.md).
- Начало работы [обнаружение угроз с помощью Azure Sentinel](tutorial-detect-threats.md).

