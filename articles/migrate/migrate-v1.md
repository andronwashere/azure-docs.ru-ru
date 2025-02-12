---
title: Работа с предыдущей версией службы "Миграция Azure" | Документация Майкрософт
description: Содержит сводку по работе со старой версией службы "Миграция Azure"
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: overview
ms.date: 07/11/2019
ms.author: raynew
ms.custom: mvc
ms.openlocfilehash: 7cf87c3a146f51666a2c24c7cd0d6e9425159225
ms.sourcegitcommit: 920ad23613a9504212aac2bfbd24a7c3de15d549
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/15/2019
ms.locfileid: "68228408"
---
# <a name="work-with-the-previous-version-of-azure-migrate"></a>Работа с предыдущей версией службы "Миграция Azure"

Данная статья содержит информацию о работе предыдущей версии службы "Миграция Azure".


Существует две версии службы "Миграция Azure".

- **Текущая версия**. С помощью этой версии службы можно создавать проекты службы "Миграция Azure", обнаруживать локальные компьютеры и выполнять оркестрацию оценок и миграций. [Дополнительные сведения](whats-new.md) о новых возможностях этой версии.
- **Предыдущая версия**. Если вы используете предыдущую версию службы "Миграция Azure" (поддерживалась только оценка локальных виртуальных машин VMware), теперь следует использовать текущую версию. Если вам по-прежнему нужно использовать проекты службы "Миграция Azure", которые созданы в предыдущей версии, далее указано то, что можно сделать, а что — нет.
    - Вы больше не можете создавать проекты миграции.
    - Мы не рекомендуем выполнять новые операции обнаружения.
    - Вы по-прежнему можете получить доступ к существующим проектам.
    - Вы по-прежнему можете выполнить оценки.
    

## <a name="upgrade-between-versions"></a>Обновление при смене версий

Нельзя обновить проекты или компоненты из предыдущей версии на новую. Вам необходимо [создать проект Миграции Azure](how-to-add-tool-first-time.md) и добавить в него средства оценки и миграции.

## <a name="find-projects-from-previous-version"></a>Поиск проектов из предыдущей версии

Найдите проекты из предыдущей версии следующим образом:

1. Перейдите на портал Azure и выберите **Все службы**, найдите службу **Миграция Azure** и выберите ее. 
2. На панели мониторинга службы "Миграция Azure" есть уведомление и ссылка для доступа к старым проектам службы.
3. Щелкните ссылку, чтобы открыть проекты версии 1.


## <a name="create-an-assessment"></a>Создание оценки

После обнаружения виртуальных машин на портале их можно сгруппировать и создать оценки.

- Можно быстро создавать, как и локальные оценки сразу после обнаружения виртуальных машин на портале.
- Советуем подождать по крайней мере один день, прежде чем создавать оценку производительности для получения надежных рекомендаций.

Создайте оценку следующим образом:

1. На странице **Обзор** проекта щелкните **+Создать оценку**.
2. Щелкните **Просмотреть все**, чтобы просмотреть свойства оценки.
3. Создайте группу и укажите ее имя.
4. Выберите компьютеры, которые необходимо добавить в группу.
5. Щелкните **Создать оценку**, чтобы создать группу и оценку.
6. После создания оценки просмотрите ее, выбрав **Обзор** > **Панель мониторинга**.
7. Щелкните **Экспорт оценки**, чтобы скачать оценку в виде файла Excel.

Если необходимо обновить существующую оценку с помощью последних данных о производительности, можно использовать команду для оценки **Пересчитать**, чтобы ее обновить.

## <a name="review-an-assessment"></a>Проверка оценки 

Оценка состоит из трех этапов:

- Оценка начинается с анализа пригодности, чтобы определить, совместимы ли компьютеры в Azure.
- Оценки размеров.
- Оценка ежемесячной стоимости.

Компьютер переходит на следующий этап, только если проходит предыдущий. Например, если компьютер не прошел проверку пригодности, то он помечается как непригодный для Azure, и размер и стоимость для него не вычисляются.


### <a name="review-azure-readiness"></a>Проверка готовности к работе в Azure

В представлении готовности к работе в Azure в отчете об оценке отображается состояние готовности каждой виртуальной машины.

**Готовность** | **State** | **Дополнительные сведения**
--- | --- | ---
Готово к работе в Azure | Нет проблем совместимости. Компьютер может быть перенесен в Azure без каких-либо изменений, он загрузится в Azure и будет иметь полную поддержку Azure. | Для виртуальных машин, которые готовы к миграции, служба "Миграция Azure" рекомендует размер виртуальной машины в Azure.
Условно готово к работе в Azure. | Компьютер может загружаться в Azure, но не будет иметь полную поддержку Azure. Например, компьютеры с более старой версией Windows Server не поддерживаются в Azure. | Служба "Миграция Azure" также определяет причину проблем готовности к работе и предоставляет рекомендации по их устранению.
не готова к работе в Azure; |  Виртуальная машина не загрузится в Azure. Например, если размер диска виртуальной машины превышает 4 ТБ, то ее невозможно будет разместить на Azure. | Служба "Миграция Azure" также определяет причину проблем готовности к работе и предоставляет рекомендации по их устранению.
готовность неизвестна. | Обычно из-за недоступности данных служба "Миграция Azure" не может опознать готовность к работе в Azure.


#### <a name="azure-vm-properties"></a>Свойства виртуальной машины Azure
Готовность учитывает ряд свойств виртуальной машины, чтобы определить, может ли она работать в Azure.


**Свойство** | **Дополнительные сведения** | **Готовность**
--- | --- | ---
**Тип загрузки** | BIOS поддерживается. UEFI не поддерживается. | Если тип загрузки — UEFI, виртуальная машина считается условно готовой.
**Ядра** | Компьютерное ядро <= максимальному количеству ядер (128), поддерживаемых для виртуальной машины Azure.<br/><br/> Если доступен журнал производительности, служба "Миграция Azure" рассматривает используемые ядра.<br/>Если <br/>в параметрах оценки указан фактор комфорта, то количество используемых ядер умножается на него.<br/><br/> Если журнал производительности отсутствует, служба "Миграция Azure" использует выделенные ядра, не применяя фактор комфорта. | Если меньше заданного ограничения или равно ему, виртуальная машина готова.
**Память** | Объем памяти компьютера <= максимальному объему памяти (3892 ГБ на компьютере Azure серии Standard_M128m&nbsp;<sup>2</sup>) для виртуальной машины Azure. [Узнайте больше](https://docs.microsoft.com/azure/virtual-machines/windows/sizes).<br/><br/> Если доступен журнал производительности, служба "Миграция Azure" рассматривает используемую память.<br/><br/>Если задан фактор комфорта, объем используемой памяти умножается на него.<br/><br/> Если журнал отсутствует, используется выделенная память без применения фактора комфорта.<br/><br/> | Если не превышает ограничение, виртуальная машина готова.
**Диск хранилища** | Выделенный размер диска не должен превышать 4 ТБ (4096 ГБ).<br/><br/> Количество дисков, подключенных к компьютеру, не должно превышать 65, включая диск операционной системы. | Если не превышает ограничение, виртуальная машина готова.
**Сеть** | К компьютеру должно быть подключено не более 32 сетевых адаптеров. | Если не превышает ограничение, виртуальная машина готова.

#### <a name="guest-operating-system"></a>Операционная система на виртуальной машине

Помимо свойств виртуальных машин служба "Миграция Azure" также просматривает гостевую ОС локальной виртуальной машины, чтобы определить, может ли виртуальная машина работать в Azure.

- Служба "Миграция Azure" рассматривает ОС, указанную в vCenter Server.
- Так как обнаружение, выполняемое службой "Миграция Azure", основано на использовании устройства, не предусмотрен способ проверить, является ли ОС виртуальной машины той же, что и в vCenter Server.

Используется следующая логика.

**Операционная система** | **Дополнительные сведения** | **Готовность**
--- | --- | ---
Windows Server 2016 и все пакеты обновления | Azure обеспечивает полную поддержку. | Готово к работе в Azure
Windows Server 2012 R2 и все пакеты обновления | Azure обеспечивает полную поддержку. | Готово к работе в Azure
Windows Server 2012 и все пакеты обновления | Azure обеспечивает полную поддержку. | Готово к работе в Azure
Windows Server 2008 R2 и все пакеты обновления | Azure обеспечивает полную поддержку.| Готово к работе в Azure
Windows Server 2008 (32-разрядная и 64-разрядная) | Azure обеспечивает полную поддержку. | Готово к работе в Azure
Windows Server 2003, 2003 R2 | Больше не поддерживаются. Для их поддержки в Azure требуется [соглашение Custom Support Agreement (CSA)](https://aka.ms/WSosstatement). | Условно готово к работе в Azure. Следует обновить ОС перед миграцией в Azure.
Windows 2000, 98, 95, NT, 3.1, MS-DOS | Не поддерживаются. Компьютер может быть загружен в Azure, но Azure не будет поддерживать ОС. | Условно готово к работе в Azure. Следует обновить ОС перед миграцией в Azure.
Windows Client 7, 8 и 10 | Azure обеспечивает поддержку [только для подписки Visual Studio](https://docs.microsoft.com/azure/virtual-machines/windows/client-images). | Условно готово к работе в Azure.
Windows 10 Pro Desktop | Azure обеспечивает поддержку с [правами на мультитенантное размещение.](https://docs.microsoft.com/azure/virtual-machines/windows/windows-desktop-multitenant-hosting-deployment) | Условно готово к работе в Azure.
Windows Vista, XP Professional | Не поддерживаются. Компьютер может быть загружен в Azure, но Azure не будет поддерживать ОС. | Условно готово к работе в Azure. Следует обновить ОС перед миграцией в Azure.
Linux | Azure рекомендует эти [операционные систем Linux](../virtual-machines/linux/endorsed-distros.md). Другие операционные системы Linux могут быть загружены в Azure, но перед переносом мы советуем обновить ОС до рекомендуемой версии. | Устройство готово к работе в Azure, если использует рекомендуемую версию.<br/><br/>Условно готово, если версия не рекомендуется.
Другие операционные системы<br/><br/> Например, Oracle Solaris, Apple Mac OS, FreeBSD и т. д. | Azure не рекомендует эти операционные системы. Компьютер может быть загружен в Azure, но Azure не будет поддерживать ОС. | Условно готово к работе в Azure. Следует установить поддерживаемую ОС перед миграцией в Azure.  
ОС, указанные как **Другое** в vCenter Server | Служба "Миграция Azure" не сможет идентифицировать ОС. | Готовность неизвестна. Убедитесь, что ОС виртуальной машины поддерживается в Azure.
32-разрядные операционные системы | Компьютер может быть загружен в Azure, но Azure не предоставит полную поддержку. | Условно готово к работе в Azure. Измените операционную систему компьютера с 32-разрядной на 64-разрядную перед миграцией в Azure


### <a name="review-sizing"></a>Проверка размера

 Рекомендуемый размер службы "Миграция Azure" зависит от условий определения размера, указанных в свойствах оценки.

- Если указано определение размера на основе производительности, рекомендации для размера учитываются на основе журнала производительности виртуальных машин (ЦП и памяти) и дисков (операций ввода-вывода в секунду и пропускной способности).
- Если указано условие определения размера как для локальной виртуальной машины, при формировании рекомендации размера в Azure учитывается размер локальной виртуальной машины. Размер диска будет производиться на основе типа хранилища, указанного в свойствах оценки (по умолчанию — диски класса "Премиум"). Миграция Azure не учитывает данные о производительности для виртуальной машины и дисков.

### <a name="review-cost-estimates"></a>Проверка оценки стоимости

Оценка стоимости отображает общую стоимость вычислений и хранения для запуска виртуальных машин в Azure, а также сведения для каждой машины.

- Оценка стоимости вычисляется с учетом рекомендации по размеру для виртуальной машины и ее дисков, а также свойств оценки.
- Примерная ежемесячная стоимость вычислений и хранилища суммируется для всех виртуальных машин в группе.
- Оценка стоимости приводится для запуска локальных виртуальных машин как виртуальных машин Azure IaaS (инфраструктура как услуга). При использовании службы "Миграция Azure" не учитывается стоимость платформы как услуги (PaaS) или программного обеспечения как услуги (SaaS).

### <a name="review-confidence-rating-performance-based-assessment"></a>Проверка оценки достоверности (оценка на основе производительности)

Каждая оценка на основе производительности связана с оценкой достоверности.

- Оценка достоверности находится в диапазоне от 1 до 5 звезд (1 звезда — самая низкая, а 5 звезд — самая высокая).
- Оценка достоверности назначается оцениванию на основе доступности точек данных, необходимых для вычисления оценки.
- Оценка достоверности помогает определить надежность рекомендаций по выбору размера, предоставленных службой "Миграция Azure".
- Оценка достоверности недоступна для локальных оценок "как есть".

Для определения размера на основе производительности службе "Миграция Azure" требуются следующие сведения.
- Данные об использовании ЦП.
- Данные памяти виртуальной машины.
- Для каждого диска, присоединенного к виртуальной машине, необходимы сведения об операциях ввода-вывода в секунду и пропускной способности диска.
- Для каждого сетевого адаптера, подключенного к виртуальной машине, службе "Миграция Azure" необходимы сведения о входящем и исходящем сетевом трафике.
- Если что-либо из перечисленного выше недоступно, рекомендации по размеру (и по соответствующим оценкам достоверности) могут быть ненадежными.


В зависимости от процента доступных точек данных возможные оценки достоверности представлены в таблице:

**Уровень доступности точек данных** | **Оценка достоверности**
--- | ---
0–20 % | 1 звезда
21–40 % | 2 звезды
41–60 % | 3 звезды
61–80 % | 4 звезды
81–100 % | 5 звезд


#### <a name="assessment-issues-affecting-confidence-ratings"></a>Проблемы оценки, затрагивающие оценки достоверности

В процессе оценки могут быть доступны не все точки данных по следующим причинам.

- Среда не была профилирована на период времени оценки. Например, если создавать оценки с продолжительностью производительности набора в один день, нужно подождать как минимум день после запуска обнаружения или сбора всех точек данных.
- В течение периода, для которого рассчитывалась оценка, некоторые виртуальные машины были отключены. Если какие-либо виртуальные машины были отключены в течение некоторого времени, служба "Миграция Azure" не сможет собрать данные о производительности за этот период.
- За период расчета оценки было создано несколько виртуальных машин. Например, вы создаете оценку, используя журнал производительности за последний месяц, при этом несколько виртуальных машин были созданы неделю назад, журнал производительности новых виртуальных машин не будет доступен за весь период.

> [!NOTE]
> Если оценка достоверности меньше чем пять звезд, подождите по крайней мере один день, чтобы устройство могло профилировать среду, а затем пересчитайте оценку. Если вы этого не сделаете, размер на основе производительности может быть ненадежным. Если вы не хотите делать пересчет, рекомендуется переключиться на режим определения размера как для локальной виртуальной машины, изменив свойства оценки.



## <a name="create-groups-using-dependency-visualization"></a>Создание групп с помощью визуализации зависимостей

Кроме создания групп вручную вы можете создавать группы с помощью визуализации зависимостей.
- Этот метод обычно используется, если вы хотите оценить группы с более высоким уровнем достоверности, выполнив перекрестную проверку зависимостей компьютеров, прежде чем выполнять оценку.
- Визуализация зависимостей поможет эффективно спланировать миграцию в Azure, гарантируя полный перенос без простоев.
- Вы сможете обнаружить все взаимозависимые системы, которые нужно переносить вместе, и определить, используют ли пользователи запущенную систему или ее уже нужно вывести из эксплуатации.
- Служба "Миграция Azure" использует решение "Сопоставление служб" в Azure Monitor для визуализации зависимостей.

> [!NOTE]
> Визуализация зависимостей недоступна в Azure для государственных организаций.

Чтобы настроить визуализацию зависимостей, свяжите рабочую область Log Analytics с проектом службы "Миграция Azure", установите агенты на компьютерах, для которых необходимо визуализировать зависимости, а затем создайте группы с помощью сведений о зависимостях. 



### <a name="associate-a-log-analytics-workspace"></a>Связывание рабочей области Log Analytics

Чтобы использовать визуализацию зависимостей, свяжите рабочую область Log Analytics с проектом миграции. Вы можете создать или присоединить рабочую область только в той же подписке, в которой создан проект миграции.

1. Чтобы присоединить рабочую область Log Analytics к проекту в области **Обзор** > **Основное** щелкните **Требуется настройка**.
2. Вы можете создать рабочую область или присоединить имеющуюся.
  - Чтобы создать рабочую область, укажите имя. Рабочая область создается в том же [географическом регионе Azure](https://azure.microsoft.com/global-infrastructure/geographies/), что и проект миграции.
  - При подключении имеющейся рабочей области вы можете выбрать нужную из всех доступных рабочих областей в той же подписке, что и проект миграции. В списке указаны только рабочие области, созданные в регионе, в котором [поддерживается решение "Сопоставление служб"](https://docs.microsoft.com/azure/azure-monitor/insights/service-map-configure#supported-azure-regions). Чтобы подключить рабочую область, необходим доступ к ней с ролью "Читатель".

> [!NOTE]
> Изменить рабочую область, связанную с проектом миграции, невозможно.

### <a name="download-and-install-vm-agents"></a>Скачивание и установка агентов виртуальной машины

Настроив рабочую область, скачайте и установите агенты на каждом локальном компьютере, который нужно оценить. Кроме того, если у вас есть компьютеры без подключения к Интернету, на них необходимо скачать и установить [шлюз Log Analytics](../azure-monitor/platform/gateway.md).

1. На странице **Обзор** выберите **Управление** > **Компьютеры** и выберите необходимый компьютер.
2. В столбце **Зависимости** щелкните **Установить агенты**.
3. На странице **Зависимости** скачайте и установите Microsoft Monitoring Agent (MMA) и агент зависимостей на каждую виртуальную машину, которую нужно оценить.
4. Скопируйте идентификатор и ключ рабочей области. Они требуются для установки MMA на локальном компьютере.

> [!NOTE]
> Чтобы автоматизировать установку агентов, можно воспользоваться средством развертывания, например System Center Configuration Manager, или средством нашего партнера, таким как [Intigua](https://www.intigua.com/getting-started-intigua-for-azure-migration), который предоставляет решение развертывания агентов для службы "Миграция Azure".


#### <a name="install-the-mma-agent-on-a-windows-machine"></a>Установка агента MMA на компьютере Windows

Вот как можно установить агент на компьютере Windows.

1. Дважды щелкните скачанный файл агента.
2. На странице **приветствия** нажмите кнопку **Далее**. На странице **Условия лицензии** нажмите кнопку **Принимаю**, чтобы принять условия лицензии.
3. В поле **Конечная папка** оставьте или измените папку установки по умолчанию. Нажмите кнопку **Далее**.
4. В разделе **Параметры установки агента** последовательно выберите **Azure Log Analytics** > **Далее**.
5. Щелкните **Добавить**, чтобы добавить новую рабочую область Log Analytics. Вставьте идентификатор и ключ рабочей области, скопированные на портале. Щелкните **Далее**.

Вы можете установить агент из командной строки или автоматически, например с помощью System Center Configuration Manager. Дополнительные сведения об использовании этих методов для установки агента MMA см. в разделе [Установка и настройка агента](https://docs.microsoft.com/azure/azure-monitor/platform/log-analytics-agent#install-and-configure-agent).

#### <a name="install-the-mma-agent-on-a-linux-machine"></a>Установка агента MMA на компьютере Linux

Вот как можно установить агент на компьютере Linux.

1. Перенесите соответствующий пакет (x86 или x64) на компьютер Linux с помощью scp или sftp.
2. Установите пакет, используя аргумент --install.

    ```sudo sh ./omsagent-<version>.universal.x64.sh --install -w <workspace id> -s <workspace key>```

[Дополнительные сведения](https://docs.microsoft.com/azure/log-analytics/log-analytics-concept-hybrid#supported-linux-operating-systems) о списке операционных систем Linux, поддерживаемых MMA.

### <a name="install-the-mma-agent-on-a-machine-monitored-by-operations-manager"></a>Установка агента MMA на компьютере под наблюдением Operations Manager

Для компьютеров, отслеживаемых System Center Operations Manager 2012 R2 или более поздней версии, нет необходимости устанавливать агент MMA. Сопоставление служб интегрируется с Operations Manager MMA для сбора необходимых данных о зависимостях. [Узнайте больше](https://docs.microsoft.com/azure/azure-monitor/insights/service-map-scom#prerequisites). Агент зависимостей должен быть установлен.


### <a name="install-the-dependency-agent"></a>Установка агента зависимостей

1. Чтобы установить агент зависимостей на компьютере Windows, дважды щелкните файл установки и следуйте инструкциям мастера.
2. Чтобы установить агент зависимостей на компьютере Linux, сделайте это с правами привилегированного пользователя, используя следующую команду.

    ```sh InstallDependencyAgent-Linux64.bin```

- Узнайте больше о поддержке агента зависимостей для ОС [Windows](../azure-monitor/insights/service-map-configure.md#supported-windows-operating-systems) и [Linux](../azure-monitor/insights/service-map-configure.md#supported-linux-operating-systems).
- [Узнайте больше](https://docs.microsoft.com/azure/monitoring/monitoring-service-map-configure#installation-script-examples) о том, как установить агент зависимостей с помощью скриптов.


### <a name="create-a-group-with-dependency-mapping"></a>Создание группы с помощью сопоставления зависимостей

1. После установки агентов перейдите на портал и выберите **Управление** > **Компьютеры**.
2. Найдите компьютер, на который вы установили агенты.
3. Столбец **зависимостей** для компьютера должен теперь отображаться как **Просмотреть зависимости**. Щелкните столбец для просмотра зависимостей компьютера.
4. На карте зависимостей компьютера появятся следующие сведения:
    - Входящие (клиенты) и исходящие (серверы) TCP-подключения к компьютеру и от него:
        - Зависимые компьютеры, на которых не установлен агент MMA и агент зависимостей, будут сгруппированы по номерам портов.
        - Зависимые компьютеры, на которых установлен агент MMA и агент зависимостей, будут отображаться в отдельных полях.
    - Чтобы просмотреть процессы, запущенные на компьютере, разверните поле каждого компьютера.
    - Свойства компьютера, включая отображение FQDN, операционной системы и MAC-адреса. Вы можете щелкнуть поле каждого компьютера, чтобы просмотреть сведения.

4. Вы можете просмотреть зависимости за разные периоды. Для этого нужно щелкнуть длительность периода в метке диапазона времени. По умолчанию диапазон равен одному часу. Можно изменить диапазон времени или указать даты начала и окончания и длительность.

   > [!NOTE]
   >    Поддерживается диапазон времени до часа. Используйте журналы Azure Monitor для [запроса данных зависимостей](https://docs.microsoft.com/azure/migrate/how-to-create-group-machine-dependencies) на более длительный срок.

5. После определения зависимых компьютеров, которые необходимо сгруппировать, выберите несколько компьютеров на карте, удерживая клавишу CTRL, а затем щелкните **Группировать компьютеры**.
6. Укажите имя группы. Проверьте, нашла ли служба миграции Azure зависимые компьютеры.

    > [!NOTE]
    > Если нет, компьютеры нельзя добавить в группу. Чтобы добавить такие компьютеры в группу, необходимо снова запустить процесс обнаружения в подходящей области в vCenter Server, и убедиться, что служба миграции Azure нашла зависимые компьютеры.  

7. Если вы хотите создать оценку для этой группы, установите флажок.
8. Нажмите кнопку **ОК**, чтобы сохранить группу.

После создания группы рекомендуется установить агенты на всех входящих в нее компьютерах и уточнить ее путем визуализации зависимости всей группы.

## <a name="query-dependency-data-from-azure-monitor-logs"></a>Запрос данных зависимостей из журналов Azure Monitor

Данные о зависимостях, собранные Сопоставлением служб, доступны для запросов в рабочей области Log Analytics, связанной с вашим проектом Миграции Azure. [Дополнительные сведения](https://docs.microsoft.com/azure/azure-monitor/insights/service-map#log-analytics-records) о таблицах данных Сопоставления служб для запроса в журналах Azure Monitor. 

Для выполнения запросов Kusto сделайте следующее.

1. После установки агентов перейдите на портал и выберите **Обзор**.
2. В разделе **Обзор** перейдите в раздел проекта **Основное** и щелкните по имени рабочей области, указанному рядом с разделом **Рабочая область OMS**.
3. На странице "Рабочая область Log Analytics" щелкните **Общие** > **Журналы**.
4. Составьте запрос для сбора данных о зависимости с помощью журналов Azure Monitor. Примеры запросов приведены в следующем разделе.
5. Выполните запрос, нажав кнопку "Запуск". 

[Дополнительные сведения](https://docs.microsoft.com/azure/azure-monitor/log-query/get-started-portal) о написании запросов Kusto. 

### <a name="sample-azure-monitor-logs-queries"></a>Примеры запросов с журналами Azure Monitor

Ниже приведены примеры запросов, которые можно использовать для извлечения данных зависимостей. Вы можете изменить запросы, чтобы извлечь предпочтительные точки данных. Исчерпывающий список полей в записях данных зависимостей доступен [здесь](https://docs.microsoft.com/azure/azure-monitor/insights/service-map#log-analytics-records). Дополнительные примеры можно найти [здесь](https://docs.microsoft.com/azure/azure-monitor/insights/service-map#sample-log-searches).

#### <a name="summarize-inbound-connections-on-a-set-of-machines"></a>Создание сводки входящих подключений для набора компьютеров.

Записи в таблицах для метрик подключений, VMConnection не представляют отдельные физические сетевые подключения. Система группирует несколько физических сетевых подключений в логическое подключение. [Дополнительные сведения](https://docs.microsoft.com/azure/azure-monitor/insights/service-map#connections) о том, как данные физических сетевых подключений агрегированы в одну логическую запись в таблице VMConnection. 

```
// the machines of interest
let ips=materialize(ServiceMapComputer_CL
| summarize ips=makeset(todynamic(Ipv4Addresses_s)) by MonitoredMachine=ResourceName_s
| mvexpand ips to typeof(string));
let StartDateTime = datetime(2019-03-25T00:00:00Z);
let EndDateTime = datetime(2019-03-30T01:00:00Z); 
VMConnection
| where Direction == 'inbound' 
| where TimeGenerated > StartDateTime and TimeGenerated  < EndDateTime
| join kind=inner (ips) on $left.DestinationIp == $right.ips
| summarize sum(LinksEstablished) by Computer, Direction, SourceIp, DestinationIp, DestinationPort
```

#### <a name="summarize-volume-of-data-sent-and-received-on-inbound-connections-between-a-set-of-machines"></a>Создание сводки объема отправленных и полученных данных по входящим подключениям между наборами компьютеров.

```
// the machines of interest
let ips=materialize(ServiceMapComputer_CL
| summarize ips=makeset(todynamic(Ipv4Addresses_s)) by MonitoredMachine=ResourceName_s
| mvexpand ips to typeof(string));
let StartDateTime = datetime(2019-03-25T00:00:00Z);
let EndDateTime = datetime(2019-03-30T01:00:00Z); 
VMConnection
| where Direction == 'inbound' 
| where TimeGenerated > StartDateTime and TimeGenerated  < EndDateTime
| join kind=inner (ips) on $left.DestinationIp == $right.ips
| summarize sum(BytesSent), sum(BytesReceived) by Computer, Direction, SourceIp, DestinationIp, DestinationPort
```


## <a name="next-steps"></a>Дополнительная информация
[Дополнительные сведения](migrate-services-overview.md) о последней версии службы "Миграция Azure".
