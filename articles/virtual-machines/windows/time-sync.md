---
title: Синхронизация времени для виртуальных машин Windows в Azure | Документы Майкрософт
description: Синхронизация времени для виртуальных машин Windows
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: gwallace
editor: tysonn
tags: azure-resource-manager
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 09/17/2018
ms.author: cynthn
ms.openlocfilehash: d413fe73735f526444aea76d68f44163065578a0
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/09/2019
ms.locfileid: "67710233"
---
# <a name="time-sync-for-windows-vms-in-azure"></a>Синхронизация времени для виртуальных машин Windows в Azure

Синхронизация времени важна для безопасности и корреляции событий. Иногда она применяется для реализации распределенных транзакций. Точность времени в нескольких компьютерных системах достигается за счет синхронизации. На синхронизацию может влиять ряд факторов, включая перезагрузки и сетевой трафик между источником времени и получающим сведения о времени компьютером. 

Платформа Azure основана на инфраструктуре, работающей под управлением Windows Server 2016. В Windows Server 2016 реализованы улучшенные алгоритмы коррекции времени и синхронизации локальных часов с временем в формате UTC.  Кроме того, в Windows Server 2016 улучшена работа службы VMICTimeSync, которая управляет синхронизацией виртуальных машин с узлом для получения точного времени. К числу улучшений относится более точное исходное время при запуске или восстановлении виртуальной машины, а также коррекция задержки при прерывании для образцов, предоставляемых в службу времени Windows (W32time). 


>[!NOTE]
>Краткий обзор службы времени в Windows можно получить в [этом видео](https://aka.ms/WS2016TimeVideo).
>
> Дополнительные сведения см. в статье [Точное время в Windows Server 2016](https://docs.microsoft.com/windows-server/networking/windows-time-service/accurate-time). 

## <a name="overview"></a>Обзор

Точность часов компьютера оценивается по тому, насколько близки их показания к стандартному времени в формате UTC. Время UTC устанавливается по точным атомным часам, отклонение которых не превышает одной секунды за 300 лет. Однако для считывания времени UTC напрямую требуется специальное оборудование. Вместо этого со временем UTC синхронизируются серверы времени, к которым затем обращаются другие компьютеры. Таким образом достигается масштабируемость и надежность. На каждом компьютере выполняется служба синхронизации времени, которая знает, какие серверы времени следует использовать, и регулярно проверяет необходимость коррекции часов компьютера, при необходимости корректируя время. 

Узлы Azure синхронизированы с внутренними серверами времени Майкрософт, которые получают время от принадлежащих Майкрософт устройств Stratum 1 с антеннами GPS. Виртуальные машины Azure могут получать точное время от узла (*время узла*) непосредственно с сервера времени или использовать эти способы в сочетании. 

Взаимодействие виртуальной машины с узлом также может влиять на показания часов. Во время [обслуживания с сохранением памяти](maintenance-and-updates.md#maintenance-that-doesnt-require-a-reboot) виртуальные машины приостанавливаются на срок до 30 секунд. Например, до начала обслуживания часы виртуальной машины показывают 10:00:00, и приостановка длится 28 секунд. Когда виртуальная машина возобновляет работу, ее часы по-прежнему показывают 10:00:00, то есть отстают на 28 секунд. Чтобы скорректировать это отклонение, служба VMICTimeSync отслеживает происходящее в узле и запрашивает внесение изменений в виртуальные машины.

Служба VMICTimeSync работает в режиме образца или синхронизации и влияет только на перевод часов вперед. В режиме образца, предусматривающем функционирование службы W32time, служба VMICTimeSync опрашивает узел каждые 5 секунд и предоставляет W32time образцы времени. Примерно каждые 30 секунд служба W32time корректирует гостевые часы, используя для этого последний образец времени. Режим синхронизации активируется, если гостевая система возобновляет работу или при отставании гостевых часов от часов узла более чем на 5 секунд. Если служба W32time функционирует правильно, последнее никогда не происходит.

При отсутствии синхронизации времени на часах виртуальной машины накапливались бы ошибки. При наличии только одной виртуальной машины последствия могут быть незначительными, если только рабочая нагрузка не требует очень точного отсчета времени. Но в большинстве случаев имеется несколько взаимосвязанных виртуальных машин, которые используют время для отслеживания транзакций, поэтому оно должно быть согласовано в масштабе всего развертывания. Если время в виртуальных машинах различается, могут иметь место указанные ниже последствия.

- Проверка подлинности будет завершаться сбоем. Протоколы безопасности, такие как Kerberos, или технологии, зависящие от сертификатов, требуют согласования времени в системах. 
- Очень трудно понять, что случилось в системе, если журналы (или другие данные) рассогласованы по времени. Одно и то же событие будет представляться как произошедшее в разное время, что усложняет корреляцию.
- Если часы идут неверно, могут происходить ошибки при выставлении счетов.

Наилучшие результаты для развертываний Windows достигаются при использовании Windows Server 2016 в качестве операционной системы на виртуальной машине, что позволяет пользоваться последними улучшениями в плане синхронизации времени.

## <a name="configuration-options"></a>Варианты настройки

Есть три способа настроить синхронизацию времени для виртуальных машин Windows, размещенных в Azure:

- время узла и time.windows.com; это конфигурация по умолчанию для образов из Azure Marketplace;
- только от узла;
- использование другого внешнего сервера времени с применением или без применения времени узла.


### <a name="use-the-default"></a>Конфигурация по умолчанию

По умолчанию образы виртуальных машин с ОС Windows настроены для синхронизации w32time из двух источников: 

- поставщика NtpClient, который получает сведения с сервера time.windows.com;
- службы VMICTimeSync, которая служит для передачи времени узла виртуальным машинам и корректировки после приостановки виртуальных машин для обслуживания. Для поддержания точного времени узлы Azure используют принадлежащие Майкрософт устройства Stratum 1.

w32time отдает приоритет поставщикам времени в следующей очередности: уровень страты, задержка корня, дисперсия корня, смещение времени. В большинстве случаев служба w32time предпочитает сервер time.windows.com узлу, так как страта time.windows.com меньше. 

Для присоединенных к домену компьютеров домен сам устанавливает иерархию синхронизации времени, однако корень леса по-прежнему должен получать время откуда-нибудь и описанные ниже особенности соблюдаются.


### <a name="host-only"></a>Только от узла 

Так как time.windows.com — это общедоступный NTP-сервер, синхронизация времени с ним требует передачи трафика через Интернет. Задержки пакетов могут отрицательно сказываться на качестве синхронизации времени. Отказ от использования сервера time.windows.com и переход на синхронизацию только с узлом порой может улучшить результаты.

Переход на синхронизацию времени только с узлом имеет смысл, если возникают проблемы при использовании конфигурации синхронизации по умолчанию. Попробуйте использовать синхронизацию только с узлом и посмотрите, улучшится ли синхронизация времени в виртуальной машине. 

### <a name="external-time-server"></a>Внешний сервер времени

Если предъявляются особые требования к синхронизации времени, можно также воспользоваться внешними серверами времени. Внешние серверы времени могут предоставлять определенное время, что может быть полезно для тестирования, обеспечения одинаковости времени в машинах, размещенных в центрах обработки данных, не принадлежащих Майкрософт, или обработки корректировочных секунд особым образом.

Внешние серверы можно использовать в сочетании со службой VMICTimeSync и поставщиком VMICTimeProvider, что позволяет получить результаты, сходные с конфигурацией по умолчанию. 

## <a name="check-your-configuration"></a>Проверка конфигурации


Проверьте, настроен ли поставщик времени NtpClient для использования явно заданных NTP-серверов (NTP) или синхронизации времени в домене (NT5DS).

```
w32tm /dumpreg /subkey:Parameters | findstr /i "type"
```

Если виртуальная машина использует NTP, вы увидите такой результат:

```
Value Name                 Value Type          Value Data
Type                       REG_SZ              NTP
```

Чтобы узнать, какой сервер времени использует поставщик времени NtpClient, в командной строке с повышенными привилегиями введите следующее:

```
w32tm /dumpreg /subkey:Parameters | findstr /i "ntpserver"
```

Если виртуальная машина использует сервер по умолчанию, выходные данные будут выглядеть примерно так:

```
NtpServer                  REG_SZ              time.windows.com,0x8
```


Чтобы узнать, какой поставщик времени используется в настоящее время, введите следующее:

```
w32tm /query /source
```


Ниже приведены возможные результаты и их значение.
    
- **time.windows.com** — в конфигурации по умолчанию служба w32time получает время с сервера time.windows.com. Качество синхронизации времени зависит от подключения к нему через Интернет и задержек пакетов. Это обычный результат при установке с параметрами по умолчанию.
- **Поставщик синхронизации времени виртуальных машин IC** — время виртуальной машины синхронизируется с узлом. Обычно такой результат бывает, если вы включили синхронизацию времени только с узлом или если NtpServer в настоящее время недоступен. 
- *Сервер домена* — текущая машина находится в домене, и домен определяет иерархию синхронизации времени.
- *Другой сервер* — служба w32time была явным образом настроена для получения времени с другого сервера. Качество синхронизации зависит от качества этого сервера.
- **Локальные часы CMOS** — часы не синхронизированы. Этот результат может быть получен, если служба w32time не успела запуститься после перезагрузки или если не доступен ни один настроенный источник времени.


## <a name="opt-in-for-host-only-time-sync"></a>Включение синхронизации времени только с узлом

Команда Azure непрерывно работает над улучшением синхронизации времени на серверах и гарантирует, что вся инфраструктура синхронизации времени размещается в принадлежащих Майкрософт центрах обработки данных. Если возникают проблемы с синхронизацией времени в конфигурации по умолчанию, в которой в качестве основного источника времени используется сервер time.windows.com, вы можете включить синхронизацию времени только с узлом с помощью приведенных ниже команд.

Пометьте поставщик VMIC как включенный: 

```
reg add HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\w32time\TimeProviders\VMICTimeProvider /v Enabled /t REG_DWORD /d 1 /f
```

Пометьте поставщик NTPClient как выключенный:

```
reg add HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\w32time\TimeProviders\NtpClient /v Enabled /t REG_DWORD /d 0 /f
```

Перезапустите службу w32time:

```
net stop w32time && net start w32time
```


## <a name="windows-server-2012-and-r2-vms"></a>Виртуальные машины Windows Server 2012 и Windows Server 2012 R2 

В Windows Server 2012 и Windows Server 2012 R2 разные параметры по умолчанию для синхронизации времени. Служба w32time по умолчанию настроена так, чтобы обеспечивались низкие издержки за счет точности времени. 

Если вы хотите перевести развертывания Windows Server 2012 и Windows Server 2012 R2 на использование новых параметров по умолчанию, в соответствии с которыми предпочтение отдается точности времени, выполните указанные ниже действия.

Измените интервалы опроса и обновления w32time в соответствии с параметрами Windows Server 2016.

```
reg add HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\w32time\Config /v MinPollInterval /t REG_DWORD /d 6 /f
reg add HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\w32time\Config /v MaxPollInterval /t REG_DWORD /d 10 /f
reg add HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\w32time\Config /v UpdateInterval /t REG_DWORD /d 100 /f
w32tm /config /update
```

Чтобы служба w32time могла использовать новые интервалы опроса, необходимо пометить NTP-серверы как использующие их. Если серверы помечены битовой маской 0x1, данный механизм переопределяется и служба w32time использует значение SpecialPollInterval. Убедитесь в том, что указанные NTP-серверы используют флаг 0x8 или не используют флаг вообще:

Проверьте, какие флаги используются для NTP-серверов.

```
w32tm /dumpreg /subkey:Parameters | findstr /i "ntpserver"
```

## <a name="next-steps"></a>Следующие шаги

Ниже приведены ссылки на дополнительные материалы по синхронизации времени:

- [Средства и параметры службы времени Windows](https://docs.microsoft.com/windows-server/networking/windows-time-service/Windows-Time-Service-Tools-and-Settings)
- [Улучшения Windows Server 2016](https://docs.microsoft.com/windows-server/networking/windows-time-service/windows-server-2016-improvements)
- [Точное время в Windows Server 2016](https://docs.microsoft.com/windows-server/networking/windows-time-service/accurate-time)
- [Граница области поддержки для настройки службы времени Windows для сред высокой точности](https://docs.microsoft.com/windows-server/networking/windows-time-service/support-boundary)


