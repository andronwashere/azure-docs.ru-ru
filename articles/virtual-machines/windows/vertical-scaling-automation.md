---
title: Вертикальное масштабирование виртуальных машин Windows c помощью службы автоматизации Azure | Документация Майкрософт
description: Вертикальное масштабирование виртуальной машины Windows в ответ на оповещения мониторинга c помощью службы автоматизации Azure
services: virtual-machines-windows
documentationcenter: ''
author: singhkays
manager: gwallace
editor: ''
tags: azure-resource-manager
ms.assetid: 4f964713-fb67-4bcc-8246-3431452ddf7d
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 04/18/2019
ms.author: kasing
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: a7cccd36c619e58b8dedb9a52e70c478dc7b857c
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/09/2019
ms.locfileid: "67707922"
---
# <a name="vertically-scale-windows-vms-with-azure-automation"></a>Вертикальное масштабирование виртуальных машин Windows с помощью службы автоматизации Azure

Вертикальное масштабирование — это процесс увеличения или уменьшения объема ресурсов виртуальной машины в зависимости от рабочей нагрузки. В Azure это можно сделать, изменив размер виртуальной машины. Вертикальное масштабирование можно использовать в следующих сценариях:

* если виртуальная машина используется редко, вы можете уменьшить ее размер, чтобы сократить ежемесячные затраты;
* если виртуальная машина испытывает пиковые нагрузки, ее размер можно увеличить, чтобы расширить емкость.

Ниже представлены шаги для реализации вертикального масштабирования.

1. Настройка службы автоматизации Azure для доступа к виртуальным машинам.
2. Импорт модулей Runbook службы автоматизации Azure для вертикального масштабирования в подписку
3. Добавление веб-перехватчика в модуль Runbook.
4. Добавление правила оповещения для виртуальной машины

## <a name="scale-limitations"></a>Ограничения масштабирования

Размеры, до которых можно масштабировать виртуальную машину, могут быть ограничены из-за размера первой виртуальной машины ввиду поддерживаемых размеров кластера, в котором развернута текущая виртуальная машина. В этой статье мы учли эту особенность и в опубликованных модулях Runbook службы автоматизации использовали для масштабирования только приведенные ниже примеры размеров виртуальных машин. Это означает, что нельзя внезапно увеличить виртуальную машину размером Standard_D1v2 до размера Standard_G5 или уменьшить до Basic_A0. Также не поддерживается ограниченное виртуальной машины, размеры масштабирование вверх или вниз. 

Вы можете выбрать для масштабирования следующие пары размеров:

* [Серии A](#a-series)
* [Серия B](#b-series)
* [Серии D](#d-series)
* [E-Series](#e-series)
* [Серия F](#f-series)
* [Серия G](#g-series)
* [Серия H](#h-series)
* [Серия L](#l-series)
* [Серия M](#m-series)
* [Серии N](#n-series)

### <a name="a-series"></a>Серия A

| Начальный размер | Увеличить масштаб размера | 
| --- | --- |
| Basic_A0 | Basic_A1 |
| Basic_A1 | Basic_A2 |
| Basic_A2 | Basic_A3 |
| Basic_A3 | Basic_A4 |
| Standard_A0 | Standard_A1 |
| Standard_A1 | Standard_A2 |
| Standard_A2 | Standard_A3 |
| Standard_A3 | Standard_A4 |
| Standard_A5 | Standard_A6 |
| Standard_A6 | Standard_A7 |
| Standard_A8 | Standard_A9 |
| Standard_A10 | Standard_A11 |
| Standard_A1_v2 | Standard_A2_v2 |
| Standard_A2_v2 | Standard_A4_v2 |
| Standard_A4_v2 | Standard_A8_v2 |
| Standard_A2m_v2 | Standard_A4m_v2 |
| Standard_A4m_v2 | Standard_A8m_v2 |

### <a name="b-series"></a>Серия B

| Начальный размер | Увеличить масштаб размера | 
| --- | --- |
| Standard_B1s | Standard_B2s |
| Standard_B1ms | Standard_B2ms |
| Standard_B2ms | Standard_B4ms |
| Standard_B4ms | Standard_B8ms |

### <a name="d-series"></a>Серия D

| Начальный размер | Увеличить масштаб размера | 
| --- | --- |
| Standard_D1 | Standard_D2 |
| Standard_D2 | Standard_D3 |
| Standard_D3 | Standard_D4 |
| Standard_D11 | Standard_D12 |
| Standard_D12 | Standard_D13 |
| Standard_D13 | Standard_D14 |
| Standard_DS1 | Standard_DS2 |
| Standard_DS2 | Standard_DS3 |
| Standard_DS3 | Standard_DS4 |
| Standard_DS11 | Standard_DS12 |
| Standard_DS12 | Standard_DS13 |
| Standard_DS13 | Standard_DS14 |
| Standard_D1_v2 | Standard_D2_v2 |
| Standard_D2_v2 | Standard_D3_v2 |
| Standard_D3_v2 | Standard_D4_v2 |
| Standard_D4_v2 | Standard_D5_v2 |
| Standard_D11_v2 | Standard_D12_v2 |
| Standard_D12_v2 | Standard_D13_v2 |
| Standard_D13_v2 | Standard_D14_v2 |
| Standard_DS1_v2 | Standard_DS2_v2 |
| Standard_DS2_v2 | Standard_DS3_v2 |
| Standard_DS3_v2 | Standard_DS4_v2 |
| Standard_DS4_v2 | Standard_DS5_v2 |
| Standard_DS11_v2 | Standard_DS12_v2 |
| Standard_DS12_v2 | Standard_DS13_v2 |
| Standard_DS13_v2 | Standard_DS14_v2 |
| Standard_D2_v3 | Standard_D4_v3 |
| Standard_D4_v3 | Standard_D8_v3 |
| Standard_D8_v3 | Standard_D16_v3 |
| Standard_D16_v3 | Standard_D32_v3 |
| Standard_D32_v3 | Standard_D64_v3 |
| Standard_D2s_v3 | Standard_D4s_v3 |
| Standard_D4s_v3 | Standard_D8s_v3 |
| Standard_D8s_v3 | Standard_D16s_v3 |
| Standard_D16s_v3 | Standard_D32s_v3 |
| Standard_D32s_v3 | Standard_D64s_v3 |
| Standard_DC2s | Standard_DC4s |

### <a name="e-series"></a>Серия E

| Начальный размер | Увеличить масштаб размера | 
| --- | --- |
| Standard_E2_v3 | Standard_E4_v3 |
| Standard_E4_v3 | Standard_E8_v3 |
| Standard_E8_v3 | Standard_E16_v3 |
| Standard_E16_v3 | Standard_E20_v3 |
| Standard_E20_v3 | Standard_E32_v3 |
| Standard_E32_v3 | Standard_E64_v3 |
| Standard_E2s_v3 | Standard_E4s_v3 |
| Standard_E4s_v3 | Standard_E8s_v3 |
| Standard_E8s_v3 | Standard_E16s_v3 |
| Standard_E16s_v3 | Standard_E20s_v3 |
| Standard_E20s_v3 | Standard_E32s_v3 |
| Standard_E32s_v3 | Standard_E64s_v3 |

### <a name="f-series"></a>Серия F

| Начальный размер | Увеличить масштаб размера | 
| --- | --- |
| Standard_F1 | Standard_F2 |
| Standard_F2 | Standard_F4 |
| Standard_F4 | Standard_F8 |
| Standard_F8 | Standard_F16 |
| Standard_F1s | Standard_F2s |
| Standard_F2s | Standard_F4s |
| Standard_F4s | Standard_F8s |
| Standard_F8s | Standard_F16s |
| Standard_F2s_v2 | Standard_F4s_v2 |
| Standard_F4s_v2 | Standard_F8s_v2 |
| Standard_F8s_v2 | Standard_F16s_v2 |
| Standard_F16s_v2 | Standard_F32s_v2 |
| Standard_F32s_v2 | Standard_F64s_v2 |
| Standard_F64s_v2 | Standard_F7s_v2 |

### <a name="g-series"></a>Серия G

| Начальный размер | Увеличить масштаб размера | 
| --- | --- |
| Standard_G1 | Standard_G2 |
| Standard_G2 | Standard_G3 |
| Standard_G3 | Standard_G4 |
| Standard_G4 | Standard_G5 |
| Standard_GS1 | Standard_GS2 |
| Standard_GS2 | Standard_GS3 |
| Standard_GS3 | Standard_GS4 |
| Standard_GS4 | Standard_GS5 |

### <a name="h-series"></a>Серия H

| Начальный размер | Увеличить масштаб размера | 
| --- | --- |
| Standard_H8 | Standard_H16 |
| Standard_H8m | Standard_H16m |

### <a name="l-series"></a>Серия L

| Начальный размер | Увеличить масштаб размера | 
| --- | --- |
| Standard_L4s | Standard_L8s |
| Standard_L8s | Standard_L16s |
| Standard_L16s | Standard_L32s |
| Standard_L8s_v2 | Standard_L16s_v2 |
| Standard_L16s_v2 | Standard_L32s_v2 |
| Standard_L32s_v2 | Standard_L64s_v2 |
| Standard_L64s_v2 | Standard_L80s_v2 |

### <a name="m-series"></a>Серия M

| Начальный размер | Увеличить масштаб размера | 
| --- | --- |
| Standard_M8ms | Standard_M16ms |
| Standard_M16ms | Standard_M32ms |
| Standard_M32ms | Standard_M64ms |
| Standard_M64ms | Standard_M128ms |
| Standard_M32ls | Standard_M64ls |
| Standard_M64s | Standard_M128s |
| Standard_M64 | Standard_M128 |
| Standard_M64m | Standard_M128m |

### <a name="n-series"></a>Серия N

| Начальный размер | Увеличить масштаб размера | 
| --- | --- |
| Standard_NC6 | Standard_NC12 |
| Standard_NC12 | Standard_NC24 |
| Standard_NC6s_v2 | Standard_NC12s_v2 |
| Standard_NC12s_v2 | Standard_NC24s_v2 |
| Standard_NC6s_v3 | Standard_NC12s_v3 |
| Standard_NC12s_v3 | Standard_NC24s_v3 |
| Виртуальная машина Standard_ND6 | Виртуальная машина Standard_ND12 |
| Виртуальная машина Standard_ND12 | Виртуальная машина Standard_ND24 |
| Standard_NV6 | Standard_NV12 |
| Standard_NV12 | Standard_NV24 |
| Standard_NV6s_v2 | Standard_NV12s_v2 |
| Standard_NV12s_v2 | Standard_NV24s_v2 |

## <a name="setup-azure-automation-to-access-your-virtual-machines"></a>Настройка службы автоматизации Azure для доступа к виртуальным машинам.
Первое, что вам нужно сделать, — создать учетную запись службы автоматизации Azure, где будут размещаться модули Runbook, используемые для масштабирования виртуальной машины. Недавно в службе автоматизации появилась функция "Запуск от имени...", которая упрощает настройку субъекта-службы для автоматического запуска модулей Runbook от имени пользователя. Дополнительные сведения об этом см. в следующей статье:

* [Проверка подлинности модулей Runbook в Azure с помощью учетной записи запуска от имени](../../automation/automation-sec-configure-azure-runas-account.md)

## <a name="import-the-azure-automation-vertical-scale-runbooks-into-your-subscription"></a>Импорт модулей Runbook службы автоматизации Azure для вертикального масштабирования в подписку
Модули Runbook, необходимые для вертикального масштабирования виртуальной машины, уже опубликованы в коллекции Runbook службы автоматизации Azure. Вам потребуется импортировать их в подписку. Дополнительные сведения об импорте модулей Runbook см. в статье:

* [Runbook и коллекции модулей для службы автоматизации Azure](../../automation/automation-runbook-gallery.md)

На приведенном ниже рисунке показаны модули Runbook, которые необходимо импортировать.

![Импорт модулей Runbook](./media/vertical-scaling-automation/scale-runbooks.png)

## <a name="add-a-webhook-to-your-runbook"></a>Добавление веб-перехватчика в модуль Runbook.
После импорта модулей Runbook в них необходимо добавить веб-перехватчик. Этого может потребовать система оповещений виртуальной машины. Дополнительные сведения о создании веб-перехватчика для вашего модуля Runbook см. в статье:

* [Объекты Webhook в службе автоматизации Azure](../../automation/automation-webhooks.md)

Прежде чем закрывать диалоговое окно, убедитесь, что вы скопировали веб-перехватчик. Он понадобится в следующем разделе.

## <a name="add-an-alert-to-your-virtual-machine"></a>Добавление правила оповещения для виртуальной машины
1. Перейдите в раздел параметров виртуальной машины.
2. Щелкните "Правила оповещения".
3. Щелкните "Добавить оповещение".
4. Выберите метрику, на основе значения которой будут срабатывать оповещения.
5. Выберите условие, при выполнении которого будет срабатывать оповещение.
6. Установите пороговое значение для условия, упомянутого выше.
7. Выберите период для проверки условий и порогового значения, упомянутых выше.
8. Вставьте скопированный веб-перехватчик, описанный в предыдущем разделе.

![Добавить правило оповещения для виртуальной машины 1](./media/vertical-scaling-automation/add-alert-webhook-1.png)

![Добавить правило оповещения для виртуальной машины 2](./media/vertical-scaling-automation/add-alert-webhook-2.png)

