---
title: Подключение данных системного журнала к предварительной версии Azure Sentinel | Документация Майкрософт
description: Узнайте, как подключить данные syslog к Azure Sentinel.
services: sentinel
documentationcenter: na
author: rkarlin
manager: rkarlin
editor: ''
ms.assetid: 5dd59729-c623-4cb4-b326-bb847c8f094b
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/10/2019
ms.author: rkarlin
ms.openlocfilehash: fef9fa128d2ebb84fb82579f254735fdb9aa7ee2
ms.sourcegitcommit: 1b7b0e1c915f586a906c33d7315a5dc7050a2f34
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/15/2019
ms.locfileid: "67881063"
---
# <a name="connect-your-external-solution-using-syslog"></a>Подключение внешнего решения с помощью syslog

> [!IMPORTANT]
> Сейчас Azure Sentinel предоставляется в общедоступной предварительной версии.
> Эта предварительная версия предоставляется без соглашения об уровне обслуживания и не рекомендована для использования рабочей среде. Некоторые функции могут не поддерживаться или их возможности могут быть ограничены. Дополнительные сведения см. в статье [Дополнительные условия использования предварительных выпусков Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Вы можете подключить любое локальное устройство, поддерживающее syslog, в метку Azure. Это делается с помощью агента, основанного на компьютере Linux между устройством и Sentinel. Если компьютер Linux находится в Azure, вы можете выполнить потоковую передачу журналов из устройства или приложения в выделенную рабочую область, созданную в Azure, и подключить ее. Если компьютер Linux не находится в Azure, можно выполнить потоковую передачу журналов с устройства на отдельную локальную ВИРТУАЛЬную машину или компьютер, на котором устанавливается агент для Linux. 

> [!NOTE]
> Если устройство поддерживает syslog CEF, подключение является более полным, и следует выбрать этот параметр и выполнить инструкции из раздела [подключение данных из CEF](connect-common-event-format.md).

## <a name="how-it-works"></a>Принцип работы

Системный журнал (Syslog) — это протокол ведения журнала событий, который обычно используется в Linux. Приложения отправляют сообщения, которые могут храниться на локальном компьютере или передаваться в сборщик системного журнала. При установке агента Log Analytics для Linux он настраивает локальную управляющую программу Syslog для пересылки сообщений в агент. Затем агент отправляет сообщение в Azure Monitor, где создается соответствующая запись.

Дополнительные сведения см. [в разделе источники данных syslog в Azure Monitor](../azure-monitor/platform/data-sources-syslog.md).

> [!NOTE]
> Агент может получать журналы из нескольких источников, но должен быть установлен на выделенном прокси-компьютере.

## <a name="connect-your-syslog-appliance"></a>Подключение устройства системного журнала

1. На портале Sentinel Azure выберите **соединители данных** , щелкните строку **системного журнала** в таблице и в области syslog справа щелкните **открыть страницу соединителя**.
2. Если компьютер Linux находится в Azure, выберите **скачать и установить агент на виртуальной машине Linux в Azure**. В окне виртуальные машины выберите компьютеры, на которых требуется установить агент, и щелкните **подключить** в верхней части экрана.
1. Если компьютер Linux не находится в Azure, выберите **скачать и установить агент на компьютере Linux без Azure**. В окне **Direct Agent** скопируйте команду в разделе **скачать и подключить агент для Linux** и запустите его на компьютере. 
1. В разделе **Настройка журналов для подключения** в окне настройки соединителя syslog следуйте инструкциям:
    1. Щелкните ссылку, чтобы **открыть конфигурацию дополнительных параметров рабочей области**. 
    1. Выберите **данные**, а затем **системный журнал**.
    1. Затем в наборе таблиц, в котором будет осуществляться получение системного журнала. Необходимо либо добавить, либо выбрать средства, включаемые устройством syslog в заголовки журнала. Эту конфигурацию можно просмотреть в системном модуле syslog-d в папке:/ЕТК/рсислог.д/секурити-конфиг-омсажент.конф и в r-syslog в разделе/ЕТК/сислог-нг/секурити-конфиг-омсажент.конф. 
       > [!NOTE]
       > Если вы установите флажок, чтобы **Применить указанную ниже конфигурацию к моим**компьютерам, эта конфигурация будет применена ко всем компьютерам Linux, подключенным к этой рабочей области. Эту конфигурацию можно увидеть на компьютере syslog в разделе 
1. Нажмите кнопку " **нажать", чтобы открыть колонку "Конфигурация"** .
1. Выберите **данные** , а затем **системный журнал**.
   - Убедитесь, что каждое средство, отправляемое syslog, находится в таблице. Для каждого отслеживаемого устройства задайте серьезность. Нажмите кнопку **Применить**.
1. На компьютере syslog убедитесь, что вы отправляете эти средства. 

1. Чтобы использовать соответствующую схему в Log Analytics журналов системного журнала, выполните поиск **системного**журнала.
1. Вы можете использовать функцию Kusto, описанную в разделе [Использование функций в Azure Monitor запросов журналов](../azure-monitor/log-query/functions.md) для анализа сообщений Syslog, а затем сохранить их как новую log Analytics функцию, а затем использовать функцию в качестве нового типа данных.




## <a name="next-steps"></a>Следующие шаги
В этом документе вы узнали, как подключить локальные устройства syslog к Azure Sentinel. Ознакомьтесь с дополнительными сведениями об Azure Sentinel в соответствующих статьях.
- Узнайте, как [получить представление о данных и потенциальных угрозах](quickstart-get-visibility.md).
- Приступая [к обнаружению угроз с помощью Azure Sentinel](tutorial-detect-threats.md).
