---
title: Мониторинг и ведение журналов в службе защиты паролей AD Azure — Azure Active Directory
description: Общие сведения о мониторинге и ведении журнала защиты паролем Azure AD
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 02/01/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: jsimmons
ms.collection: M365-identity-device-management
ms.openlocfilehash: a029135da79d1a0b24b2941873a0fe3187ac9f7c
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "60414806"
---
# <a name="azure-ad-password-protection-monitoring-and-logging"></a>Мониторинг и ведение журнала защиты паролем Azure AD

После развертывания защиты паролем Azure AD мониторинг и отчетность являются важными задачами. В этой статье мы подробно рассмотрим несколько способов мониторинга, включая те, в которых каждая служба регистрирует сведения и создает отчеты об использовании защиты паролем Azure AD.

Мониторинг и создание отчетов выполняются сообщения журнала событий или с помощью командлетов PowerShell. Журнал событий сообщения в журнал DC агент и прокси-сервера обе службы. Все командлеты PowerShell, описанных ниже доступны только на прокси-сервера (см. в разделе модуль AzureADPasswordProtection PowerShell). Программное обеспечение агента DC не устанавливает модуль PowerShell.

## <a name="dc-agent-event-logging"></a>Ведение журнала событий агента контроллера домена

На каждом контроллере домена программное обеспечение службы агента контроллера домена записывает каждый отдельный результат операции проверки паролей (и другие состояния) в локальный журнал событий.

`\Applications and Services Logs\Microsoft\AzureADPasswordProtection\DCAgent\Admin`

`\Applications and Services Logs\Microsoft\AzureADPasswordProtection\DCAgent\Operational`

`\Applications and Services Logs\Microsoft\AzureADPasswordProtection\DCAgent\Trace`

Журнал администратора агента контроллера домена является главным источником информации о поведении программного обеспечения.

Обратите внимание, что по умолчанию журнал трассировки отключен.

События регистрируются различными компонентами агента контроллера домена с использованием всех последующих диапазонов:

|Компонент |Диапазон идентификаторов событий|
| --- | --- |
|Библиотека DLL фильтрации паролей агента контроллера домена| 10000–19999|
|Процесс размещения службы агента контроллера домена| 20000–29999|
|Логика проверки политики службы агента контроллера домена| 30000–39999|

## <a name="dc-agent-admin-event-log"></a>Журнал событий администратора агента контроллера домена

### <a name="password-validation-outcome-events"></a>Результаты событий проверки паролей

На каждом контроллере домена программное обеспечение службы агента контроллера домена записывает каждый отдельный результат проверки паролей в журнал событий администратора агента контроллера домена.

Обычно для успешной операции проверки пароля из библиотеки DLL фильтрации паролей агента контроллера домена регистрируется одно событие, а в случае неудачной проверки пароля — два: одно из службы агента контроллера домена и одно из библиотеки DLL фильтрации паролей агента контроллера домена.

Дискретные события для фиксации этих ситуаций регистрируются на основе следующих факторов:

* Будет ли настроен или изменен заданный пароль.
* Была ли пройдена проверка данного пароля.
* Произошла ли ошибка проверки из-за глобальной политики корпорации Майкрософт, политики организации или совокупности обеих политик.
* Включен или выключен в данный момент режим "Только аудит" для текущей политики паролей.

Ключевыми событиями, связанными с проверкой паролей, являются следующие:

|   |Изменение пароля |Настройка пароля|
| --- | :---: | :---: |
|Pass; |10014 |10015|
|Ошибка (из-за политики паролей клиента)| 10016, 30002| 10017, 30003|
|Ошибка (из-за политики паролей Майкрософт)| 10016, 30004| 10017, 30005|
|Ошибка (из-за совокупности политики паролей клиента и политики паролей Майкрософт)| 10016, 30026| 10017, 30027|
|Прошел проверку "Только аудит" (могла возникнуть ошибка проверки политики паролей клиента)| 10024, 30008| 10025, 30007|
|Прошел проверку "Только аудит" (могла возникнуть ошибка проверки политики паролей Майкрософт)| 10024, 30010| 10025, 30009|
|Прошел проверку "Только аудит" (могла возникнуть комбинированная ошибка проверки политики паролей клиента и Майкрософт)| 10024, 30028| 10025, 30029|

Случаи, описанные в таблице выше, касаются объединенных политик, действовавших в ситуации, когда было обнаружено, что в пароле пользователя находится по крайней мере один маркер из списка заблокированных паролей Майкрософт и списка паролей, заблокированных пользователем.

Когда пара событий регистрируется вместе, каждое из них связанно явным образом благодаря одинаковым CorrelationId.

### <a name="password-validation-summary-reporting-via-powershell"></a>Сводка отчетов о проверке пароля с помощью PowerShell

С помощью командлета `Get-AzureADPasswordProtectionSummaryReport` можно создать сводное представление о действиях проверки пароля. Пример выходных данных этого командлета выглядит следующим образом:

```powershell
Get-AzureADPasswordProtectionSummaryReport -DomainController bplrootdc2
DomainController                : bplrootdc2
PasswordChangesValidated        : 6677
PasswordSetsValidated           : 9
PasswordChangesRejected         : 10868
PasswordSetsRejected            : 34
PasswordChangeAuditOnlyFailures : 213
PasswordSetAuditOnlyFailures    : 3
PasswordChangeErrors            : 0
PasswordSetErrors               : 1
```

Область, охватываемую отчетом этого командлета, можно указать с помощью параметра -Forest, -Domain или -DomainController. Если этот параметр не указан, используется параметр -Forest.

Командлет `Get-AzureADPasswordProtectionSummaryReport` запрашивает журнал событий администратора агента контроллера домена, а затем подсчитывает общее количество событий, соответствующих каждой отображаемой категории результатов. В следующей таблице приведены сопоставления между каждым из результатов и его идентификатором соответствующего события.

|Свойство "Get-AzureADPasswordProtectionSummaryReport" |Идентификатор соответствующего события|
| :---: | :---: |
|PasswordChangesValidated |10014|
|PasswordSetsValidated |10015|
|PasswordChangesRejected |10016|
|PasswordSetsRejected |10017|
|PasswordChangeAuditOnlyFailures |10024|
|PasswordSetAuditOnlyFailures |10025|
|PasswordChangeErrors |10012|
|PasswordSetErrors |10013|

Обратите внимание, что командлет `Get-AzureADPasswordProtectionSummaryReport` предоставляется в виде формата сценария PowerShell и по требованию может выступать в виде прямой ссылки на следующее расположение.

`%ProgramFiles%\WindowsPowerShell\Modules\AzureADPasswordProtection\Get-AzureADPasswordProtectionSummaryReport.ps1`

> [!NOTE]
> Для выполнения этого командлета на каждом контроллере домена запускается сеанс PowerShell. Для успешного выполнения на каждом контроллере домена необходимо включить поддержку удаленных сеансов PowerShell, а клиент должен иметь достаточные привилегии. Чтобы получить дополнительные сведения о требованиях к удаленным сеансам PowerShell, выполните командлет Get-Help about_Remote_Troubleshooting в окне PowerShell.

> [!NOTE]
> Этот командлет удаленно запрашивает журнал событий администратора для каждого службы агента контроллера домена. Если журналы событий содержат большое количество событий, выполнение командлета может занять много времени. Кроме того, пакетные сетевые запросы для передачи больших наборов данных могут повлиять на производительность контроллера домена. Поэтому данный командлет следует осмотрительно использовать в рабочих средах.

### <a name="sample-event-log-message-for-event-id-10014-successful-password-change"></a>Пример сообщения журнала событий для идентификатора события 10014 (пароль был успешно изменен)

```text
The changed password for the specified user was validated as compliant with the current Azure password policy.

 UserName: BPL_02885102771
 FullName:
```

### <a name="sample-event-log-message-for-event-id-10017-and-30003-failed-password-set"></a>Пример сообщения журнала событий для идентификатора события 10017 и 30003 (не удалось выполнить настройку пароля)

10017:

```text
The reset password for the specified user was rejected because it did not comply with the current Azure password policy. Please see the correlated event log message for more details.

 UserName: BPL_03283841185
 FullName:
```

30003:

```text
The reset password for the specified user was rejected because it matched at least one of the tokens present in the per-tenant banned password list of the current Azure password policy.

 UserName: BPL_03283841185
 FullName:
```

### <a name="sample-event-log-message-for-event-id-30001-password-accepted-due-to-no-policy-available"></a>Пример сообщения журнала событий для идентификатора события 30001 (пароль принят из-за отсутствия политики)

```text
The password for the specified user was accepted because an Azure password policy is not available yet

UserName: SomeUser
FullName: Some User

This condition may be caused by one or more of the following reasons:%n

1. The forest has not yet been registered with Azure.

   Resolution steps: an administrator must register the forest using the Register-AzureADPasswordProtectionForest cmdlet.

2. An Azure AD password protection Proxy is not yet available on at least one machine in the current forest.

   Resolution steps: an administrator must install and register a proxy using the Register-AzureADPasswordProtectionProxy cmdlet.

3. This DC does not have network connectivity to any Azure AD password protection Proxy instances.

   Resolution steps: ensure network connectivity exists to at least one Azure AD password protection Proxy instance.

4. This DC does not have connectivity to other domain controllers in the domain.

   Resolution steps: ensure network connectivity exists to the domain.
```

### <a name="sample-event-log-message-for-event-id-30006-new-policy-being-enforced"></a>Пример сообщения журнала событий для идентификатора события 30006 (применяется новая политика)

```text
The service is now enforcing the following Azure password policy.

 Enabled: 1
 AuditOnly: 1
 Global policy date: ‎2018‎-‎05‎-‎15T00:00:00.000000000Z
 Tenant policy date: ‎2018‎-‎06‎-‎10T20:15:24.432457600Z
 Enforce tenant policy: 1
```

### <a name="sample-event-log-message-for-event-id-30019-azure-ad-password-protection-is-disabled"></a>Пример сообщения журнала событий для идентификатора события 30019 (защита паролей Azure AD — отключена)

```text
The most recently obtained Azure password policy was configured to be disabled. All passwords submitted for validation from this point on will automatically be considered compliant with no processing performed.

No further events will be logged until the policy is changed.%n

```

## <a name="dc-agent-operational-log"></a>Операционный журнал агента контроллера домена

Служба агента контроллера домена также будет регистрировать события, связанные с операциями, в следующий журнал:

`\Applications and Services Logs\Microsoft\AzureADPasswordProtection\DCAgent\Operational`

## <a name="dc-agent-trace-log"></a>Журнал трассировки агента контроллера домена

Служба агента контроллера домена также может регистрировать подробные события трассировки на уровне отладки в следующий журнал:

`\Applications and Services Logs\Microsoft\AzureADPasswordProtection\DCAgent\Trace`

Ведение журнала трассировки отключено по умолчанию.

> [!WARNING]
> Когда этот параметр включен, журнал трассировки получает большой объем событий и может влиять на производительность контроллера домена. Поэтому этот расширенный журнал следует включать, только когда проблема требует более глубокого изучения и только на минимальное время.

## <a name="dc-agent-text-logging"></a>Ведение текстового журнала агента контроллера домена

Службу агента контроллера домена можно настроить для записи в текстовый журнал, задав следующее значение реестра:

```text
HKLM\System\CurrentControlSet\Services\AzureADPasswordProtectionDCAgent\Parameters!EnableTextLogging = 1 (REG_DWORD value)
```

Ведение текстового журнала отключено по умолчанию. Чтобы изменения этого значения вступили в силу, необходимо перезапустить службу агента контроллера домена. После перезапуска служба агента контроллера домена будет вести запись в файл журнала, расположенный в папке:

`%ProgramFiles%\Azure AD Password Protection DC Agent\Logs`

> [!TIP]
> Текстовый журнал получает те же записи на уровне отладки, которые могут быть зарегистрированы в журнале трассировки, но, как правило, в более удобном для просмотра и анализа формате.

> [!WARNING]
> Когда этот параметр включен, этот журнал получает большой объем событий и может влиять на производительность контроллера домена. Поэтому этот расширенный журнал следует включать, только когда проблема требует более глубокого изучения и только на минимальное время.

## <a name="dc-agent-performance-monitoring"></a>Мониторинг производительности агента контроллера домена

Программное обеспечение службы агента контроллера домена устанавливает объект счетчика производительности с именем **Azure AD Password Protection**. В настоящее время доступны следующие счетчики производительности:

|Имя счетчика производительности | Описание|
| --- | --- |
|Обработано паролей |Этот счетчик отображает общее количество обработанных паролей (принятых или отклоненных) с момента последнего перезапуска.|
|Принято паролей |Этот счетчик отображает общее количество принятых паролей с момента последнего перезапуска.|
|Отклонено паролей |Этот счетчик отображает общее количество отклоненных паролей с момента последнего перезапуска.|
|Выполняется запросов фильтрации паролей |Этот счетчик отображает количество запросов на фильтрацию паролей, которые в настоящее время выполняются.|
|Пиковое число запросов фильтрации паролей |Этот счетчик отображает максимальное количество одновременных запросов фильтрации паролей с момента последнего перезапуска.|
|Ошибки запросов фильтрации паролей |Этот счетчик отображает общее количество неудачных запросов фильтрации паролей из-за ошибки с момента последнего перезапуска. Ошибки могут возникнуть, если служба агента контроллера домена защиты паролем Azure AD не запущена.|
|Запросы фильтрации паролей/сек |Этот счетчик отображает скорость обработки паролей.|
|Время обработки запроса фильтрации паролей |Этот счетчик отображает среднее время, необходимое для обработки запроса фильтрации паролей.|
|Пиковое количество времени обработки запроса фильтрации паролей |Этот счетчик отображает пиковое количество времени обработки запросов фильтрации паролей с момента последнего перезапуска.|
|Пароли, принятые из-за режима аудита |Этот счетчик отображает общее количество паролей, которые обычно отклоняются, но были приняты, потому что политика паролей была настроена так, чтобы быть в режиме аудита (с момента последнего перезапуска).|

## <a name="dc-agent-discovery"></a>Обнаружение агента контроллера домена

Командлет `Get-AzureADPasswordProtectionDCAgent` может использоваться для отображения базовой информации о различных агентах контроллера домена, работающих в домене или лесу. Эта информация извлекается из объектов serviceConnectionPoint, зарегистрированных запущенными службами агента контроллера домена.

Пример выходных данных этого командлета выглядит следующим образом:

```powershell
Get-AzureADPasswordProtectionDCAgent
ServerFQDN            : bplChildDC2.bplchild.bplRootDomain.com
Domain                : bplchild.bplRootDomain.com
Forest                : bplRootDomain.com
PasswordPolicyDateUTC : 2/16/2018 8:35:01 AM
HeartbeatUTC          : 2/16/2018 8:35:02 AM
```

Различные свойства обновляются каждой службой агента контроллера домена приблизительно каждый час. Данные по-прежнему зависят от задержки репликации Active Directory.

На объем запроса командлета может влиять использование параметров -Forest или -Domain.

Если значение HeartbeatUTC становится устаревшим, это может быть симптомом того, что машина службы агента контроллера домена защиты паролем Azure AD не работает на контроллере домена, была удалена или была понижена уровнем и больше не является контроллером домена.

Если значение PasswordPolicyDateUTC становится устаревшим, это может быть симптомом того, что машина службы агента контроллера домена защиты паролем Azure AD не работает должным образом.

## <a name="proxy-service-event-logging"></a>Ведение журналов событий службы прокси-сервера

Служба прокси-сервера выводит минимальный набор событий в следующие журналы событий:

`\Applications and Services Logs\Microsoft\AzureADPasswordProtection\ProxyService\Admin`

`\Applications and Services Logs\Microsoft\AzureADPasswordProtection\ProxyService\Operational`

`\Applications and Services Logs\Microsoft\AzureADPasswordProtection\ProxyService\Trace`

Обратите внимание, что по умолчанию журнал трассировки отключен.

> [!WARNING]
> Когда этот параметр включен, журнал трассировки получает большой объем событий, и это может повлиять на производительность прокси-узла. Поэтому этот журнал следует включать, только когда проблема требует более глубокого изучения и только на минимальное время.

События регистрируются различными компонентами прокси-сервера с использованием следующих диапазонов.

|Компонент |Диапазон идентификаторов событий|
| --- | --- |
|Процесс размещения службы прокси-сервера| 10000–19999|
|Основная бизнес-логика службы прокси-сервера| 20000–29999|
|Командлеты PowerShell| 30000–39999|

## <a name="proxy-service-text-logging"></a>Ведение текстового журнала службы прокси-сервера

Службу прокси-сервера можно настроить для ведения текстового журнала, задав следующее значение реестра:

HKLM\System\CurrentControlSet\Services\AzureADPasswordProtectionProxy\Parameters!EnableTextLogging = 1 (REG_DWORD value)

Ведение текстового журнала отключено по умолчанию. Чтобы изменения этого значения вступили в силу, необходимо перезапустить службу прокси-сервера. После перезапуска служба прокси-сервера будет вести записи в файл журнала, который расположен в папке:

`%ProgramFiles%\Azure AD Password Protection Proxy\Logs`

> [!TIP]
> Текстовый журнал получает те же записи на уровне отладки, которые могут быть зарегистрированы в журнале трассировки, но, как правило, в более удобном для просмотра и анализа формате.

> [!WARNING]
> Когда этот параметр включен, этот журнал получает большой объем событий и может влиять на производительность компьютера. Поэтому этот расширенный журнал следует включать, только когда проблема требует более глубокого изучения и только на минимальное время.

## <a name="powershell-cmdlet-logging"></a>Ведение журнала командлетов PowerShell

Результаты выполнения командлетов PowerShell, которые приводят к изменению состояния (например, Register-AzureADPasswordProtectionProxy), обычно записываются в операционный журнал как событие результата.

Кроме того большая часть командлетов PowerShell для защиты паролей Azure AD будет записывать в журнал текст, расположенный в разделе:

`%ProgramFiles%\Azure AD Password Protection Proxy\Logs`

Если возникает ошибка в командлете и ее причина и решение не очевидны, можно также обратиться к этим текстовым журналам.

## <a name="proxy-discovery"></a>Обнаружение прокси-сервера

Командлет `Get-AzureADPasswordProtectionProxy` может использоваться для отображения базовой информации о различных службах прокси-сервера защиты паролем Azure AD, работающих в домене или лесу. Эта информация извлекается из объектов serviceConnectionPoint, зарегистрированных запущенными службами прокси-сервера.

Пример выходных данных этого командлета выглядит следующим образом:

```powershell
Get-AzureADPasswordProtectionProxy
ServerFQDN            : bplProxy.bplchild2.bplRootDomain.com
Domain                : bplchild2.bplRootDomain.com
Forest                : bplRootDomain.com
HeartbeatUTC          : 12/25/2018 6:35:02 AM
```

Различные свойства обновляются каждой службой прокси-сервера приблизительно каждый час. Данные по-прежнему зависят от задержки репликации Active Directory.

На объем запроса командлета может влиять использование параметров -Forest или -Domain.

Если значение HeartbeatUTC становится устаревшим, это может быть симптомом того, что на том компьютере служба прокси-сервера защиты паролем Azure AD не работает или была удалена.

## <a name="next-steps"></a>Дальнейшие действия

[Предварительный просмотр: мониторинг, отчетность и устранение неполадок защиты паролем Azure AD](howto-password-ban-bad-on-premises-troubleshoot.md)

Для получения дополнительной информации о глобальном и пользовательском запрещенных списках паролей см. статью [Запрет неверных паролей в организации](concept-password-ban-bad.md).
