---
title: Общие сведения о включении Azure Monitor для виртуальных машин (Предварительная версия) | Документация Майкрософт
description: Узнайте, как развертывать и настраивать Azure Monitor для виртуальных машин. Ознакомьтесь с требованиями к системе.
services: azure-monitor
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: azure-monitor
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/22/2019
ms.author: magoedte
ms.openlocfilehash: f2c6c872abf7525a80856cd37c1bb9358cf91217
ms.sourcegitcommit: bafb70af41ad1326adf3b7f8db50493e20a64926
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/25/2019
ms.locfileid: "68489428"
---
# <a name="enable-azure-monitor-for-vms-preview-overview"></a>Общие сведения о включении Azure Monitor для виртуальных машин (Предварительная версия)

В этой статье представлен обзор параметров, доступных для настройки Azure Monitor для виртуальных машин. Для мониторинга работоспособности и производительности используйте Azure Monitor для виртуальных машин. Обнаружение зависимостей приложений, выполняемых на виртуальных машинах Azure и масштабируемых наборах виртуальных машин, локальных виртуальных машин или виртуальных машин, размещенных в другой облачной среде.  

Чтобы настроить Azure Monitor для виртуальных машин, выполните следующие действия.

* Включите одну ВИРТУАЛЬную машину Azure или масштабируемый набор виртуальных машин, выбрав **Insights (Предварительная версия)** непосредственно из виртуальной машины или масштабируемого набора виртуальных машин.
* Включите две или больше виртуальных машин Azure и масштабируемых наборов виртуальных машин с помощью политики Azure. Этот метод гарантирует, что на существующих и новых виртуальных машинах и масштабируемых наборах необходимые зависимости установлены и настроены правильно. Выводятся сведения о несоответствующих виртуальных машинах и масштабируемых наборах, поэтому можно решить, следует ли их включить и исправить.
* Включите две или несколько виртуальных машин или масштабируемых наборов виртуальных машин Azure в определенной подписке или группе ресурсов с помощью PowerShell.
* Включите Azure Monitor для виртуальных машин для мониторинга виртуальных машин или физических компьютеров, размещенных в корпоративной сети или в другой облачной среде.

## <a name="prerequisites"></a>предварительные требования

Прежде чем начинать работу, убедитесь, что вы понимаете сведения, содержащиеся в следующих разделах.

### <a name="log-analytics"></a>Log Analytics

Azure Monitor для виртуальных машин поддерживает рабочую область Log Analytics в следующих регионах:

- Центрально-западная часть США
- Западная часть США 2<sup>1</sup>
- East US
- Центральная Канада
- Южная часть Соединенного Королевства
- Западная Европа
- Юго-Восточная Азия
- Восточная Австралия
- Юго-восточная Австралия

<sup>1</sup> Этот регион сейчас не поддерживает функцию мониторинга работоспособности в Azure Monitor для виртуальных машин.

>[!NOTE]
>Виртуальные машины Azure можно развернуть из любого региона. Эти виртуальные машины не ограничиваются регионами, поддерживаемыми рабочей областью Log Analytics.
>

Если у вас нет рабочей области, можно создать ее с помощью одного из следующих ресурсов:
* [CLI Azure.](../../azure-monitor/learn/quick-create-workspace-cli.md)
* [PowerShell](../../azure-monitor/learn/quick-create-workspace-posh.md)
* [Портал Azure](../../azure-monitor/learn/quick-create-workspace.md)
* [Azure Resource Manager](../../azure-monitor/platform/template-workspace-configuration.md)

Вы также можете создать рабочую область, пока вы включаете мониторинг для отдельной ВИРТУАЛЬНОЙ машины Azure или масштабируемого набора виртуальных машин в портал Azure.

Чтобы настроить сценарий масштабирования, в котором используется политика Azure, Azure PowerShell или шаблоны Azure Resource Manager, в рабочей области Log Analytics:

* Установите решения ServiceMap и InfrastructureInsights. Эту установку можно выполнить с помощью предоставленного шаблона Azure Resource Manager. На вкладке Начало **работы** выберите **настроить рабочую область**.
* Настройте рабочую область Log Analytics для сбора данных счетчиков производительности.

Чтобы настроить рабочую область для сценария с масштабом, используйте один из следующих методов.

* Используйте [Azure PowerShell](vminsights-enable-at-scale-powershell.md#set-up-a-log-analytics-workspace).
* На странице " [**покрытие политики**](vminsights-enable-at-scale-policy.md#manage-policy-coverage-feature-overview) Azure Monitor для виртуальных машин" выберите **настроить рабочую область**. 

### <a name="supported-operating-systems"></a>Поддерживаемые операционные системы

В следующей таблице перечислены операционные системы Windows и Linux, поддерживаемые Azure Monitor для виртуальных машин. Далее в этом разделе вы найдете полный список, в котором подробно описывается основной и дополнительный выпуск ОС Linux и поддерживаемые версии ядра.

|Версия ОС |Производительность |Карты |Здоровье |
|-----------|------------|-----|-------|
|Windows Server 2019 | X | X | X |
|Windows Server 2016 1803 | X | X | X |
|Windows Server 2016 | X | X | X |
|Windows Server 2012 R2 | X | X | X |
|Windows Server 2012 | X | X | |
|Windows Server 2008 R2 | X | X| |
|Red Hat Enterprise Linux (RHEL) 6, 7| X | X| X |
|Ubuntu 14,04, 16,04, 18,04 | X | X | X |
|CentOS Linux 6, 7 | X | X | X |
|SUSE Linux Enterprise Server (SLES) 12 | X | X | X |
|Debian 8, 9,4 | X<sup>1</sup> | | X |

<sup>1</sup> Функция "Производительность" Azure Monitor для виртуальных машин доступна только в Azure Monitor. Она недоступна непосредственно из левой панели виртуальной машины Azure.

>[!NOTE]
>Функция работоспособности Azure Monitor для виртуальных машин не поддерживает вложенную [виртуализацию](../../virtual-machines/windows/nested-virtualization.md) на виртуальной машине Azure.
>

>[!NOTE]
>В операционной системе Linux:
> - Поддерживаются только версии ядра по умолчанию и SMP для Linux.
> - Нестандартные версии ядра, такие как расширение физических адресов (PAE) и Xen, не поддерживаются ни в одном дистрибутиве Linux. Например, система со строкой версии *2.6.16.21-0.8-xen* не поддерживается.
> - Пользовательские ядра, включая перекомпиляции стандартных ядер, не поддерживаются.
> - Поддерживается ядро Центосплус.

#### <a name="red-hat-linux-7"></a>Red Hat Linux 7

| Версия ОС | Версия ядра |
|:--|:--|
| 7.4 | 3.10.0-693 |
| 7.5 | 3.10.0-862 |
| 7.6 | 3.10.0-957 |

#### <a name="red-hat-linux-6"></a>Red Hat Linux 6

| Версия ОС | Версия ядра |
|:--|:--|
| 6.9 | 2.6.32-696 |
| 6.10 | 2.6.32-754 |

#### <a name="centosplus"></a>центосплус
| Версия ОС | Версия ядра |
|:--|:--|
| 6.9 | 2.6.32 — 696.18.7<br>2.6.32 — 696.30.1 |
| 6.10 | 2.6.32 — 696.30.1<br>2.6.32 — 754.3.5 |

#### <a name="ubuntu-server"></a>Сервер Ubuntu

| Версия ОС | Версия ядра |
|:--|:--|
| Ubuntu 18.04 | ядро 4,15.\*<br>4,18 * |
| Ubuntu 16.04.3 | ядро 4.15.* |
| 16.04 | 4.4.\*<br>4.8.\*<br>4.10.\*<br>4.11.\*<br>4.13.\* |
| 14.04 | 3.13.\*<br>4.4.\* |

#### <a name="suse-linux-12-enterprise-server"></a>SUSE Linux 12 Enterprise Server

| Версия ОС | Версия ядра
|:--|:--|
|12 с пакетом обновления 2 | 4.4.* |
|12 с пакетом обновления 3 | 4.4.* |
|12 SP4 | 4.4.* |
|12 SP4 | Ядро, настраиваемое Azure |

### <a name="the-microsoft-dependency-agent"></a>Microsoft Dependency Agent

Функция Map в Azure Monitor для виртуальных машин получает данные от агента зависимостей Майкрософт. Dependency Agent является зависимым от агента Log Analytics для создания подключений к Log Analytics. Поэтому в системе должен быть установлен и настроен агент Log Analytics с агентом зависимостей.

Если вы включаете Azure Monitor для виртуальных машин для одной виртуальной машины Azure или используете метод масштабного развертывания, используйте расширение агента зависимостей виртуальной машины Azure для установки агента в ходе работы.

В гибридной среде можно скачать и установить агент зависимостей вручную. Если виртуальные машины размещены за пределами Azure, используйте метод автоматического развертывания.

В приведенной ниже таблице описаны подключенные источники, которые функция схемы поддерживает в гибридной среде.

| Подключенный источник | Поддерживается | Описание |
|:--|:--|:--|
| Агенты Windows | Да | Вместе с [агентом log Analytics для Windows](../../azure-monitor/platform/log-analytics-agent.md)агентам Windows требуется агент зависимостей. Дополнительные сведения см. в разделе [Поддерживаемые операционные системы](#supported-operating-systems). |
| Агенты Linux | Да | Вместе с [агентом log Analytics для Linux](../../azure-monitor/platform/log-analytics-agent.md)агентам Linux требуется агент зависимостей. Дополнительные сведения см. в разделе [Поддерживаемые операционные системы](#supported-operating-systems). |
| Группа управления System Center Operations Manager | Нет | |

Вы можете скачать агент зависимостей из следующих расположений:

| Файл | OS | Version | SHA-256 |
|:--|:--|:--|:--|
| [InstallDependencyAgent-Windows.exe](https://aka.ms/dependencyagentwindows) | Windows | 9.8.1 | 622C99924385CBF539988D759BCFDC9146BB157E7D577C997CDD2674E27E08DD |
| [InstallDependencyAgent-Linux64.bin](https://aka.ms/dependencyagentlinux) | Linux | 9.8.1 | 3037934A5D3FB7911D5840A9744AE9F980F87F620A7F7B407F05E276FE7AE4A8 |

## <a name="role-based-access-control"></a>Управление доступом на основе ролей

Чтобы включить функции и получить доступ к ним в Azure Monitor для виртуальных машин, необходимо иметь роль *участника log Analytics* . Для просмотра данных о производительности, работоспособности и сопоставлении необходимо иметь роль " *читатель мониторинга* " для виртуальной машины Azure. Необходимо настроить рабочую область Log Analytics для Azure Monitor для виртуальных машин.

Дополнительные сведения о том, как управлять доступом к рабочей области Log Analytics, см. в статье [Управление рабочими областями](../../azure-monitor/platform/manage-access.md).

## <a name="how-to-enable-azure-monitor-for-vms-preview"></a>Включение Azure Monitor для виртуальных машин (Предварительная версия)

Включите Azure Monitor для виртуальных машин с помощью одного из методов, описанных в этой таблице.

| Состояние развертывания | Метод | Описание |
|------------------|--------|-------------|
| Одна ВИРТУАЛЬная машина Azure или масштабируемый набор виртуальных машин | [Включение из виртуальной машины](vminsights-enable-single-vm.md) | Вы можете включить одну виртуальную машину Azure, выбрав **Insights (Предварительная версия)** непосредственно из виртуальной машины или масштабируемого набора виртуальных машин. |
| Несколько виртуальных машин Azure или масштабируемых наборов виртуальных машин | [Включить с помощью политики Azure](vminsights-enable-at-scale-policy.md) | Вы можете включить несколько виртуальных машин Azure, используя политику Azure и доступные определения политик. |
| Несколько виртуальных машин Azure или масштабируемых наборов виртуальных машин | [Включение с помощью Azure PowerShell или шаблонов Azure Resource Manager](vminsights-enable-at-scale-powershell.md) | Вы можете включить несколько виртуальных машин Azure или масштабируемых наборов виртуальных машин в указанной подписке или группе ресурсов с помощью шаблонов Azure PowerShell или Azure Resource Manager. |
| Гибридное облачное решение | [Включить для гибридной среды](vminsights-enable-hybrid-cloud.md) | Можно выполнить развертывание на виртуальных машинах или физических компьютерах, размещенных в вашем центре обработки данных или в других облачных средах. |

## <a name="performance-counters-enabled"></a>Включение счетчиков производительности 

Azure Monitor для виртуальных машин настраивает Log Analytics рабочую область для получения сведений об используемых счетчиках производительности. В следующих таблицах перечислены объекты и счетчики, собираемые каждые 60 секунд.

### <a name="windows-performance-counters"></a>Счетчики производительности Windows

|Имя объекта |Имя счетчика |
|------------|-------------|
|Логический диск |Процент свободного места |
|Логический диск |Среднее время чтения с диска (с) |
|Логический диск |Среднее время обращения к диску (с) |
|Логический диск |Среднее время записи на диск (с) |
|Логический диск |Байт в секунду для диска |
|Логический диск |Скорость чтения с диска (байт/с) |
|Логический диск |Операций чтения с диска в секунду |
|Логический диск |Обращений к диску в секунду |
|Логический диск |Скорость записи (байт/с) |
|Логический диск |Операций записи на диск в секунду |
|Логический диск |Свободно мегабайт |
|Память |Доступная память в МБ |
|Сетевой адаптер |Получено байт/с |
|Сетевой адаптер |Отправлено байт/с |
|Процессор |% загруженности процессора |

### <a name="linux-performance-counters"></a>Счетчики производительности Linux

|Имя объекта |Имя счетчика |
|------------|-------------|
|Логический диск |Процент используемого места |
|Логический диск |Скорость чтения с диска (байт/с) |
|Логический диск |Операций чтения с диска в секунду |
|Логический диск |Обращений к диску в секунду |
|Логический диск |Скорость записи (байт/с) |
|Логический диск |Операций записи на диск в секунду |
|Логический диск |Свободно мегабайт |
|Логический диск |Скорость обмена с логическим диском (байт/с) |
|Память |Доступный объем памяти в МБ |
|Сеть |Всего получено байт |
|Сеть |Всего передано байт |
|Процессор |% загруженности процессора |

## <a name="management-packs"></a>Пакеты управления

Если Azure Monitor для виртуальных машин включена и настроена с Log Analytics рабочей областью, пакет управления перенаправляется на все компьютеры Windows, передающие данные в эту рабочую область. Если [Группа управления System Center Operations Manager интегрирована](../../azure-monitor/platform/om-agents.md) с рабочей областью log Analytics, пакет управления сопоставление служб развертывается из группы управления на компьютерах Windows, отправляющих отчеты в группу управления.  

Пакет управления называется *Microsoft. IntelligencePacks. ApplicationDependencyMonitor*. Он записывается в папку%Programfiles%\Microsoft Monitoring Agent\Agent\Health Service State\management Packs Packs \. Источник данных, используемый пакетом управления, — %Program files%\Microsoft Monitoring Agent\Agent\Health Service State\Resources\<Автоматически_созданный_идентификатор>\Microsoft.EnterpriseManagement.Advisor.ApplicationDependencyMonitorDataSource.dll.

## <a name="diagnostic-and-usage-data"></a>Данные диагностики и использования

Корпорация Майкрософт автоматически собирает данные об использовании и производительности при работе службы Azure Monitor. Корпорация Майкрософт использует эти данные для повышения качества, безопасности и целостности службы. 

Чтобы обеспечить точную и эффективную работу по устранению неполадок, функция Map включает данные о конфигурации программного обеспечения. Данные содержат такие сведения, как операционная система, версия, IP-адрес, DNS-имя и имя рабочей станции. Корпорация Майкрософт не собирает сведения об именах, адресах и другую контактную информацию.

Дополнительные сведения о сборе и использовании данных см. в [заявлении о конфиденциальности служб Microsoft Online Services](https://go.microsoft.com/fwlink/?LinkId=512132).

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-dsr-and-stp-note.md)]

Теперь, когда вы включили мониторинг для виртуальной машины, данные мониторинга доступны для анализа в Azure Monitor для виртуальных машин.

## <a name="next-steps"></a>Следующие шаги

Чтобы узнать, как использовать функцию работоспособности, см. статью о [просмотре сведений о работоспособности Azure Monitor для виртуальных машин](vminsights-health.md). Для просмотра обнаруженных зависимостей приложений см. статью о [просмотре схемы Azure Monitor для виртуальных машин](vminsights-maps.md).
