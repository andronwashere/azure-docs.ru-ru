---
title: Обнаружение угроз для виртуальных машин & серверах в центре безопасности Azure | Документация Майкрософт
description: В этом разделе представлены оповещения о виртуальных машинах и серверах, доступных в центре безопасности Azure.
services: security-center
documentationcenter: na
author: monhaber
manager: rkarlin
editor: ''
ms.assetid: dd2eb069-4c76-4154-96bb-6e6ae553ef46
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 7/02/2019
ms.author: v-mohabe
ms.openlocfilehash: f23865fc0a1943a5157e4ff8eb8de10a71ef0883
ms.sourcegitcommit: a8b638322d494739f7463db4f0ea465496c689c6
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/17/2019
ms.locfileid: "68295792"
---
# <a name="threat-detection-for-vms--servers-in-azure-security-center"></a>Обнаружение угроз для виртуальных машин & серверах в центре безопасности Azure

В этом разделе представлены различные типы методов обнаружения и предупреждений, доступных для виртуальных машин и серверов в следующих операционных системах. Список поддерживаемых версий см. в разделе [платформы и функции, поддерживаемые центром безопасности Azure](https://docs.microsoft.com/azure/security-center/security-center-os-coverage).

* [Windows](#windows-machines)
* [Linux](#linux-machines)

## Windows<a name="windows-machines"></a>

Центр безопасности интегрируется со службами Azure для мониторинга и защиты компьютеров под управлением Windows.  Центр безопасности предоставляет оповещения и предложения по исправлению всех этих служб в удобном для использования формате.

### ATP в защитнике Microsoft Server<a nanme="windows-atp"></a>

Центр безопасности Azure расширяет свои платформы защиты облачных рабочих нагрузок, интегрируя его с помощью Advanced Threat Protection в Защитнике Windows (ATP). Это обеспечивает комплексные возможности обнаружения конечных точек и ответов (ЕДР).

> [!NOTE]
> Датчик ATP в Защитнике Windows Server автоматически включен на серверах Windows, подключенных к центру безопасности Azure.

Когда пакет ATP в Защитнике Windows Server обнаруживает угрозу, он запускает оповещение. Оповещение отображается на панели мониторинга центра безопасности. На панели мониторинга можно выполнить сведение к консоли ATP в Защитнике Windows для выполнения подробного изучения, чтобы выявить область атаки. Дополнительные сведения о ATP в Защитнике Windows Server см. в статье подключение [серверов к службе ATP в Защитнике Windows](https://docs.microsoft.com/windows/security/threat-protection/microsoft-defender-atp/configure-server-endpoints).

### Анализ аварийного дампа<a nanme="windows-dump"></a>

Когда программа дает сбой, аварийный дамп записывает часть памяти в момент сбоя.

Сбой может быть вызван вредоносными программами или содержать вредоносные программы. Чтобы избежать обнаружения с помощью продуктов безопасности, в различных формах вредоносных программ используется атака без файлов, что позволяет избежать записи на диск или шифрования программных компонентов, записанных на диск. Такой тип атаки трудно обнаружить с помощью традиционных подходов на основе дисков.

Однако такой тип атаки можно обнаружить с помощью анализа памяти. Анализируя память в аварийном дампе, центр безопасности может определить методы, используемые атакой, для использования уязвимых мест в программном обеспечении, доступа к конфиденциальным данным и тайно пользуется, сохраняемых на скомпрометированном компьютере. Это делается в серверной части центра безопасности с минимальным влиянием на производительность узлов.

> [!div class="mx-tableFixed"]

|Предупреждение|Описание|
|---|---|
|**Обнаружено внедрение кода**|Внедрение кода — это вставка исполняемых модулей в выполняемые процессы или потоки. Этот метод используется вредоносными программами для доступа к данным, в то время как он успешно скрывается, чтобы предотвратить их поиск и удаление. <br/>Это оповещение означает, что внедренный модуль присутствует в аварийном дампе. Для различения вредоносных и невредоносных модулей центр безопасности проверяет, соответствует ли внедренный модуль профилю подозрительного поведения.|
|**Обнаружен подозрительный сегмент кода**|Указывает, что сегмент кода был выделен с помощью нестандартных методов, таких как отражение путем внедрения и обработка. Предупреждение предоставляет дополнительные характеристики сегмента кода, которые были обработаны, чтобы предоставить контекст для возможностей и поведения для сегмента кода, от которого получен отчет.|
|**Обнаружен обнаружение кода оболочки**|Код оболочки — это атакующий код, который выполняется, когда вредоносная программа воспользовалась уязвимостью программного обеспечения.<br/>Это предупреждение означает, что при анализе аварийного дампа обнаружен исполняемый код, который отражает поведение, обычно выполняемое вредоносными полезными данными. Хотя это поведение может быть также и невредоносным программным обеспечением, оно не является типичным для обычных методов разработки программного обеспечения.|

### Обнаружение атак с нефайловыми файлами<a nanme="windows-fileless"></a>

В Azure мы регулярно видим атаки, предназначенные для конечных точек клиентов.

Чтобы избежать обнаружения, атаки без файлов вставляют вредоносные полезные данные в память. Полезные данные злоумышленника сохраняются в памяти скомпрометированных процессов и выполняют широкий спектр вредоносных действий.

При обнаружении атак без использования файлов автоматизированные приемы памяти обозначают наборы средств для атаки без файлов, методы и поведение. Это решение периодически сканирует компьютер во время выполнения и извлекает аналитические сведения непосредственно из памяти процессов, критических с точки зрения безопасности.

Он находит свидетельство об уязвимости, внедрение кода и выполнение вредоносных полезных данных. Обнаружение безфайловых атак создает подробные оповещения системы безопасности для ускорения рассмотрения предупреждений, корреляции и времени отклика. Этот подход дополняет ЕДР решения на основе событий, обеспечивая более высокий охват обнаружения.

> [!NOTE]
> Вы можете имитировать оповещения Windows, загрузив [Центр безопасности Azure сборник тренировочных заданий](https://gallery.technet.microsoft.com/Azure-Security-Center-0ac8a5ef): Оповещения системы безопасности и соблюдение предоставленных рекомендаций

> [!div class="mx-tableFixed"]

|Предупреждение|Описание|
|---|---|
|**Обнаружен прием безфайловых атак**|Память, указанная ниже, содержит набор средств для атаки, не имеющих файлов: Метерпретер. Наборы средств для атаки без файлов обычно не имеют присутствия в файловой системе, что делает обнаружение традиционным антивирусным процессом.|

### <a name="further-reading"></a>Дополнительные сведения

Примеры и дополнительные сведения об обнаружении центра безопасности:

* [Как центр безопасности Azure автоматизирует обнаружение кибератакной атаки](https://azure.microsoft.com/blog/leverage-azure-security-center-to-detect-when-compromised-linux-machines-attack/)
* [Как центр безопасности Azure обнаруживает уязвимости с помощью средств администрирования](https://azure.microsoft.com/blog/azure-security-center-can-detect-emerging-vulnerabilities-in-linux/)

## Linux<a name="linux-machines"></a>

Центр безопасности собирает записи аудита с компьютеров Linux с помощью **аудита**, одной из наиболее распространенных платформ аудита Linux. Аудит имеет преимущество в течение длительного времени и прижить к ядру ядра. 

### Интеграционные оповещения Linux и интеграция Microsoft Monitoring Agent (MMA)<a name="linux-auditd"></a>

Система аудита состоит из подсистемы уровня ядра, которая отвечает за наблюдение за системными вызовами, их фильтрацию по заданному набору правил и запись сообщений для них в сокет. Центр безопасности интегрирует функциональные возможности из пакета, прошедшие аудит, в Microsoft Monitoring Agent (MMA). Эта интеграция включает сбор событий аудита во всех поддерживаемых дистрибутивах Linux без каких бы то ни было предварительных требований.  

записи аудита собираются, расширяются и объединяются в события с помощью агента MMA для Linux. Центр безопасности постоянно работает над добавлением новой аналитики, которая использует сигналы Linux для обнаружения вредоносного поведения на облачных и локальных компьютерах Linux. Подобно возможностям Windows, эти аналитические данные охватывают все подозрительные процессы, сомнительна попытки входа в систему, загрузку модулей ядра и другие действия, которые указывают на то, что компьютер находится под угрозой или был нарушен.  

Ниже приведено несколько примеров аналитики, которые демонстрируют, как мы занимаемся на разных стадиях жизненного цикла атаки.

> [!div class="mx-tableFixed"]

|Предупреждение|Описание|
|---|---|
|**Процесс доступа к файлу санкционированных ключей SSH ненеобычен**|Доступ к файлу с санкционированными ключами SSH осуществлялся в методе, аналогичном известным кампаниям по вредоносным программам. Этот доступ может означать, что злоумышленник пытается получить постоянный доступ к компьютеру.|
|**Обнаружена неустойчивая попытки**|При анализе данных узла обнаружено, что был установлен сценарий запуска для однопользовательского режима. <br/>Поскольку в этом режиме необходимо выполнить любой законный процесс, это может означать, что злоумышленник добавил вредоносный процесс для каждого уровня выполнения, чтобы гарантировать сохранение.|
|**Обнаружено управление запланированными задачами**|При анализе данных узла обнаружена возможная обработка запланированных задач. Злоумышленники часто добавляют запланированные задачи на компьютеры, которые были скомпрометированы для получения сохранения.|
|**Подозрительные изменения отметки времени файла**|При анализе данных узла обнаружено подозрительное изменение метки времени. Злоумышленники часто копируют метки времени из существующих легальных файлов в новые средства, чтобы избежать обнаружения этих недавно удаленных файлов.|
|**В группу "Sudo" был добавлен новый пользователь**|При анализе данных узла обнаружено, что пользователь был добавлен в группу "Sudo", что позволяет его членам выполнять команды с высоким уровнем привилегий.|
|**Вероятность использования уязвимости Динорут в DHCP-клиенте**|При анализе данных узла обнаружено выполнение необычной команды с родительским процессом скрипта дхклиент.|
|**Обнаружен подозрительный модуль ядра**|При анализе данных узла обнаружен файл общего объекта, загружаемого в качестве модуля ядра. Это может быть подлинное действие, или указывает на то, что одна из ваших компьютеров была скомпрометирована.|
|**Обнаружен процесс, связанный со интеллектуальным анализом цифровых валют**|При анализе данных узла обнаружено выполнение процесса, который обычно связан с интеллектуальным анализом цифровых валют|
|**Потенциальная пересылка портов на внешний IP-адрес**|Анализ данных узла обнаружил инициацию перенаправления портов на внешний IP-адрес.|

> [!NOTE]
> Вы можете имитировать оповещения Windows, загрузив [центр безопасности Azure сборник тренировочных заданий: Оповещения](https://gallery.technet.microsoft.com/Azure-Security-Center-0ac8a5ef) системы безопасности и следуйте указанным рекомендациям.


Дополнительные сведения вы найдете в следующих статьях:  

* [Использование центра безопасности Azure для обнаружения скомпрометированных атак компьютеров Linux](https://azure.microsoft.com/blog/leverage-azure-security-center-to-detect-when-compromised-linux-machines-attack/)

* [Центр безопасности Azure может обнаруживать новые уязвимости в Linux](https://azure.microsoft.com/blog/azure-security-center-can-detect-emerging-vulnerabilities-in-linux/)

 