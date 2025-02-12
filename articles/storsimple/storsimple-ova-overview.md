---
title: Обзор виртуального массива Microsoft Azure StorSimple | Документация Майкрософт
description: В статье описывается виртуальный массив Microsoft Azure StorSimple — интегрированное решение хранилища, которое управляет задачами хранилища в локальном виртуальном массиве и облачном хранилище Microsoft Azure.
services: storsimple
documentationcenter: NA
author: alkohli
manager: jeconnoc
editor: ''
ms.assetid: 169c639b-1124-46a5-ae69-ba9695525b77
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 03/14/2019
ms.author: alkohli
ms.openlocfilehash: e5713af737a6d9d190814b4155a8e772deea06bc
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "60630371"
---
# <a name="introduction-to-the-storsimple-virtual-array"></a>Общие сведения о виртуальном массиве StorSimple

## <a name="overview"></a>Обзор

Виртуальный массив Microsoft Azure StorSimple — это интегрированное решение хранилища. Оно управляет задачами хранилища в локальном виртуальном массиве (выполняется в низкоуровневой оболочке) и облачном хранилище Microsoft Azure. Виртуальный массив — это эффективное, экономичное и легко управляемое решение на базе файлового сервера или сервера iSCSI. Это решение устраняет многие проблемы и сокращает расходы, связанные с использованием корпоративного хранилища и защитой данных. Виртуальный массив хорошо подходит для хранения редко используемых архивных данных.

В этой статье приведен обзор виртуальных массивов. Вот еще несколько ресурсов по этой теме:

* [рекомендации по использованию виртуального массива StorSimple](storsimple-ova-best-practices.md);
* обзор устройств серии StorSimple 8000 см. в статье [Серия StorSimple 8000: решение гибридного облачного хранилища](storsimple-overview.md);
* дополнительные сведения об устройствах серий StorSimple 5000 и 7000 см. в [онлайн-справке по StorSimple](http://onlinehelp.storsimple.com/).

Виртуальный массив поддерживает протокол iSCSI или SMB. Он запускается в существующей инфраструктуре низкоуровневой оболочки и обеспечивает распределение по уровням в облаке, резервное копирование в облако, быстрое восстановление, восстановление на уровне элементов и аварийное восстановление.

В следующей таблице перечислены важные функции виртуального массива StorSimple.

| Компонент | Виртуальный массив StorSimple |
| --- | --- |
| Требования для установки |Использует инфраструктуру виртуализации (Hyper-V или VMware) |
| Доступность |Один узел |
| Общая емкость (включая облако) |До 64 ТБ доступной для использования емкости на каждый виртуальный массив. |
| Локальная емкость |От 390 ГБ до 6,4 ТБ доступной для использования емкости на каждый виртуальный массив (нужно подготовить от 500 ГБ до 8 ТБ места на диске). |
| Собственные протоколы |iSCSI или SMB |
| Целевое время восстановления (RTO) |iSCSI: менее 2 минут независимо от размера |
| Целевая точка восстановления (RPO) |Ежедневное резервное копирование и резервное копирование по требованию |
| Распределение по уровням хранилища |Использует тепловую карту, чтобы определить, какие данные следует распределить по уровням или перенести |
| Поддержка |Инфраструктура виртуализации, поддерживаемая поставщиком |
| Производительность |Зависит от базовой инфраструктуры |
| Мобильность данных |Позволяет выполнить восстановление на то же устройство или восстановление на уровне элементов (файловый сервер) |
| Уровни хранилища |Локальное хранилище низкоуровневой оболочки и облако |
| Размер общего ресурса |Многоуровневые — до 20 ТБ, локально закрепленные — до 2 ТБ |
| Размер тома |Многоуровневый: от 500 ГБ до 5 ТБ; локально закрепленный: от 50 ГБ до 200 ГБ <br> Максимальный локально зарезервированный объем для многоуровневых томов: 200 ГБ. |
| Моментальные снимки |Отказоустойчивые |
| Восстановление на уровне элементов |Пользователи могут выполнить восстановление из общих папок |

## <a name="why-use-storsimple"></a>Зачем использовать StorSimple?

StorSimple подключает пользователей и серверы к хранилищу Azure за считанные минуты, не изменяя приложение.

В приведенной ниже таблице описаны некоторые ключевые преимущества, связанные с использованием виртуального массива StorSimple.

| Компонент | Преимущество |
| --- | --- |
| Прозрачная интеграция |Виртуальный массив поддерживает протокол iSCSI или SMB. Перемещение данных между локальным и облачным уровнями осуществляется прозрачно и незаметно для пользователей. |
| Снижение затрат на хранилище |StorSimple позволяет подготовить локальное хранилище достаточного объема в соответствии с текущими требованиями наиболее используемых "горячих" данных. По мере роста потребностей хранения StorSimple распределяет "холодные" данные по уровням экономичного облачного хранилища. Перед отправкой в облако данные дедуплицируются и сжимаются, чтобы еще больше сократить требования к хранилищу и затраты. |
| Упрощенное управление хранилищем |StorSimple обеспечивает централизованное управление в облаке, позволяя управлять несколькими устройствами с помощью диспетчера устройств StorSimple. |
| Усовершенствованное аварийное восстановление и соответствие |StorSimple помогает ускорить аварийное восстановление, восстанавливая метаданные немедленно, а остальные данные — по мере необходимости. Это означает, что нормальная работа может быть продолжена с минимальными простоями. |
| Мобильность данных |Доступ к данным, распределенным по уровням в облаке, может быть организован и для других сайтов в целях восстановления и переноса данных. Обратите внимание, что данные можно восстанавливать только в исходный виртуальный массив. Но с помощью функции аварийного восстановления вы можете восстановить весь виртуальный массив в другой виртуальный массив. |

## <a name="storsimple-workload-summary"></a>Сводка по рабочим нагрузкам StorSimple

Ниже приведена сводка по поддерживаемым рабочим нагрузкам StorSimple.

|Сценарий     |Рабочая нагрузка     |Поддерживаются      |Ограничения               | Применимые версии|
|-------------|-------------|---------------|---------------------------|--------------------|
|Технология ROBO (Remote Office/Branch Office)  |Общий доступ к файлам     |Да      |См. раздел [Максимальные ограничения для файлового сервера](storsimple-ova-limits.md).<br></br>См. раздел [Системные требования для поддерживаемых версий SMB](storsimple-ova-system-requirements.md).| Все версии     |
|Облачная архивация  |Общий доступ к архивным файлам     |Да      |См. раздел [Максимальные ограничения для файлового сервера](storsimple-ova-limits.md).<br></br>См. раздел [Системные требования для поддерживаемых версий SMB](storsimple-ova-system-requirements.md).| Все версии     |

Виртуальный массив StorSimple лучше всего подходит для редко используемых данных. Так как у виртуального массива есть локальный кэш для повышения производительности, пользователи должны предполагать, что устройство хранит файлы на самом низком уровне хранилища (облака). Каждый виртуальный массив может выполнять операции записи и чтения в хранилище Azure со скоростью приблизительно 100 Мбит/с. Этот канал совместно используется всеми запросами, поступающими на устройство, и может стать узким местом, как показано на следующем рисунке.

![Облачная архивация](./media/storsimple-ova-overview/cloud-archiving.png)

Если несколько пользователей одновременно обращаются к виртуальному массиву, они все используют одно подключение к Azure, что приводит к снижению производительности. Гарантированного уровня производительности для каждого пользователя нет, и устройство обрабатывает отдельные запросы по мере их поступления.

Виртуальный массив StorSimple не подходит для рабочих нагрузок, требующих высокого уровня доступности. Виртуальный массив — это отдельный узел устройства, который простаивает при установке обновлений программного обеспечения. Администраторам следует планировать период обслуживания в 30 минут, 3–4 раза в год.

## <a name="workflows"></a>Рабочие процессы

Виртуальный массив StorSimple особенно хорошо подходит для следующих рабочих процессов:

* [облачное управление хранилищем;](#cloud-based-storage-management)
* [резервное копирование независимо от расположения;](#location-independent-backup)
* [защита данных и аварийное восстановление.](#data-protection-and-disaster-recovery)

### <a name="cloud-based-storage-management"></a>облачное управление хранилищем;
Службу диспетчера устройств StorSimple, запущенную на портале Azure, можно использовать для управления данными, хранящимися на нескольких устройствах и в нескольких расположениях. Это особенно полезно в сценариях распределенных филиалов. Обратите внимание, что для управления виртуальными массивами и физическими устройствами StorSimple необходимо создать отдельные экземпляры службы диспетчера устройств StorSimple. Также учитывайте, что виртуальный массив теперь использует новый портал Azure, а не классический портал Azure.

![облачное управление хранилищем;](./media/storsimple-ova-overview/cloud-based-storage-management.png)

### <a name="location-independent-backup"></a>резервное копирование независимо от расположения;
Если используется виртуальный массив, облачные моментальные снимки позволяют получить независимые от расположения копии томов или общих папок на определенный момент времени. Облачные моментальные снимки включены по умолчанию и их нельзя отключать. Всех томов и общих папок создаются в то же время, по одной политике ежедневного резервного копирования, и может иметь дополнительные нерегламентированного резервного копирования при необходимости.

### <a name="data-protection-and-disaster-recovery"></a>Защита данных и аварийное восстановление
Виртуальный массив поддерживает следующие сценарии защиты данных и аварийного восстановления.

* **Восстановление тома или общего ресурса**. Используйте новый рабочий процесс для восстановления тома или общего ресурса. Этот подход можно использовать для восстановления всего тома или общей папки.
* **Восстановление на уровне элемента**. Общие ресурсы упрощают доступ к последним архивам. Можно легко восстановить отдельный файл из специальной папки *.backup*, доступной в облаке. Этой возможностью восстановления управляет пользователь, поэтому вмешательство администратора не требуется.
* **Аварийное восстановление**. Возможность отработки отказа позволяет восстановить все общие папки или тома в новом виртуальном массиве. Для этого нужно создать виртуальный массив и зарегистрировать его в службе диспетчера устройств StorSimple, а затем выполнить отработку отказа для исходного виртуального массива. Все подготовленные ресурсы будут перемещены в новый виртуальный массив.

## <a name="storsimple-virtual-array-components"></a>Компоненты виртуального массива StorSimple

Виртуальный массив содержит следующие компоненты.

* [Виртуальный массив](#virtual-array). Это гибридное облачное хранилище на основе виртуальной машины, подготовленной в виртуализированной среде или низкоуровневой оболочке.
* [Служба диспетчера устройств StorSimple](#storsimple-device-manager-service). Это расширение портала Azure, которое позволяет управлять одним или несколькими виртуальными устройствами StorSimple в одном веб-интерфейсе, к которому можно получить доступ из разных географических расположений. Службу диспетчера устройств StorSimple можно использовать для создания служб и просмотра устройств и уведомлений, а также управления этими ресурсами, томами, общими папками и существующими моментальными снимками.
* [Локальный пользовательский веб-интерфейс](#local-web-user-interface). Он используется для настройки устройств при их подключении к локальной сети и регистрации в службе диспетчера устройств StorSimple. 
* [Интерфейс командной строки](#command-line-interface). Этот интерфейс Windows PowerShell позволяет запустить сеанс поддержки на виртуальном массиве.
  В следующих разделах каждый из этих компонентов описывается подробно и объясняется, каким образом решение упорядочивает данные и выделяет память, а также помогает управлять хранилищем и защищать данные.

### <a name="virtual-array"></a>Виртуальный массив

Виртуальный массив — это система хранения с одним узлом, которая выполняет функции основного хранилища, управляет взаимодействием с облачным хранилищем, а также обеспечивает безопасность и конфиденциальность всех данных, хранящихся на устройстве.

Для скачивания доступна одна модель виртуального массива. Максимальная емкость виртуального массива составляет до 6,4 ТБ на самом устройстве (для этого требуется 8 ТБ базового хранилища) и до 64 ТБ в сочетании с облачным хранилищем.

Виртуальный массив предоставляет следующие возможности.

* Оно экономично. Оно использует существующую инфраструктуру виртуализации и его можно развернуть в существующей низкоуровневой оболочке Hyper-V или VMware.
* Оно находится в центре обработки данных и его можно настроить в качестве файлового сервера или сервера iSCSI.
* Оно интегрировано с облаком.
* Резервные копии хранятся в облаке, что упрощает аварийное восстановление и восстановление на уровне элементов.
* Виртуальный массив можно обновлять так же, как физическое устройство.

> [!NOTE]
> Виртуальный массив нельзя расширить. Поэтому важно подготовить достаточно места для хранения еще при создании виртуального массива.

### <a name="storsimple-device-manager-service"></a>Служба диспетчера устройств StorSimple

Microsoft Azure StorSimple включает службу диспетчера устройств StorSimple, которая предоставляет пользовательский веб-интерфейс для централизованного управления хранилищем StorSimple. Вы можете использовать службу диспетчера устройств StorSimple для выполнения следующих задач.

* Управление несколькими виртуальными массивами StorSimple из одной службы.
* Настройка параметров безопасности для виртуальных массивов StorSimple и управление ими. (Шифрование в облаке зависит от интерфейсов API Microsoft Azure.)
* Настройка учетных данных и свойств учетной записи хранения.
* Настройка томов и общих папок и управление ими.
* Резервное копирование и восстановление данных в общих папках или томах.
* Мониторинг производительности.
* Просмотр системных параметров и идентификация возможных проблем.

Служба диспетчера устройств StorSimple используется для повседневных задач по администрированию виртуального массива.

Дополнительные сведения см. в статье [Администрирование виртуального массива StorSimple с помощью службы диспетчера устройств StorSimple](storsimple-virtual-array-manager-service-administration.md).

### <a name="local-web-user-interface"></a>Локальный пользовательский веб-интерфейс

Виртуальный массив содержит пользовательский веб-интерфейс, который однократно используется для настройки устройства и его регистрации в службе диспетчера устройств StorSimple. С его помощью можно выключать и перезапускать виртуальный массив, выполнять диагностические тесты, обновлять программное обеспечение, изменять пароль администратора устройства, просматривать системные журналы и направлять запрос на обслуживание в службу технической поддержки Майкрософт.

Сведения об использовании веб-интерфейса см. в статье [Использование веб-интерфейса для администрирования виртуального массива StorSimple](storsimple-ova-web-ui-admin.md).

### <a name="command-line-interface"></a>Интерфейс командной строки

Встроенный интерфейс Windows PowerShell позволяет запустить сеанс поддержки со службой технической поддержки Майкрософт, специалисты которой помогут устранить неполадки и решить проблемы, которые могут возникнуть на виртуальном устройстве.

## <a name="storage-management-technologies"></a>Технологии управления хранилищем

Помимо виртуальных массивов и других компонентов, решение StorSimple использует следующие технологии программного обеспечения для быстрого доступа к важным данным, сокращения потребления хранилища и защиты данных, хранящихся в виртуальном массиве:

* [Автоматическое распределение по уровням хранилища](#automatic-storage-tiering) 
* [локально закрепленные общие папки и тома;](#locally-pinned-shares-and-volumes)
* сжатие и дедупликация данных, распределенных по уровням в облаке или скопированных в облако; 
* [резервное копирование по расписанию и по требованию.](#scheduled-and-on-demand-backups)

### <a name="automatic-storage-tiering"></a>Автоматическое распределение по уровням хранилища
Виртуальный массив использует новый механизм распределения по уровням для управления данными, хранящимися в виртуальном массиве и в облаке. Существует всего два уровня: локальный виртуальный массив и облачное хранилище Azure. Виртуальный массив StorSimple автоматически распределяет данные по уровням согласно тепловой карте, которая отслеживает текущее использование, время существования и связи с другими данными. Наиболее активные данные (используемые чаще всего) хранятся локально, а менее активные и неактивные данные автоматически переносятся в облако. (Все резервные копии хранятся в облаке). StorSimple настраивает и переупорядочивает данные и назначения хранилища по мере изменения шаблонов использования. Например, некоторые сведения могут стать менее активными со временем. По мере уменьшения активности они распределяются по уровням в облаке. Если эти же данные снова становятся активными, они переносятся на уровень виртуального массива.

Данные для определенного многоуровневого общего файлового ресурса или тома имеют собственное пространство локального уровня (около 10 % от общего подготовленного пространства для этого общего файлового ресурса или тома). Таким образом, доступное в виртуальном массиве пространство для хранения этих общих папок или томов сокращается, но в то же время обеспечивается независимое распределение по уровням для разных общих папок и томов. Таким образом, чрезмерная загруженность рабочего процесса общей папки или тома не приведет к переносу других рабочих нагрузок в облако.

Многоуровневые тома, созданные для iSCSI, имеют максимальный локальный зарезервированный объем 200 ГБ, независимо от размера тома.

![Автоматическое распределение по уровням хранилища](./media/storsimple-ova-overview/automatic-storage-tiering.png)

> [!NOTE]
> Любой том можно настроить как локально закрепленный. В этом случае его данные остаются в виртуальном массиве и никогда не перемещаются в облако. Дополнительные сведения см. в разделе [Локально закрепленные общие папки и тома](#locally-pinned-shares-and-volumes).


### <a name="locally-pinned-shares-and-volumes"></a>локально закрепленные общие папки и тома;

Можно создать соответствующие общие папки и тома как локально закрепленные. Благодаря этому данные, используемые критически важными приложениями, остаются в виртуальном массиве и никогда не перемещаются в облако. Локально закрепленные общие папки и тома отличаются следующими особенностями.

* Они не подвержены характерным для облака задержкам или проблемам со связью.
* Они используют преимущества облачного резервного копирования и аварийного восстановления StorSimple.

Можно восстановить локально закрепленные общие папки или тома как многоуровневые, а многоуровневые общие папки или тома как локально закрепленные. 

Дополнительные сведения о локально закрепленных томах см. в статье [Управление томами в виртуальном массиве StorSimple с помощью службы диспетчера устройств StorSimple](storsimple-virtual-array-manage-volumes.md).

### <a name="deduplication-and-compression-for-data-tiered-or-backed-up-to-the-cloud"></a>сжатие и дедупликация данных, распределенных по уровням в облаке или скопированных в облако;

StorSimple использует дедупликацию и сжатие данных для дальнейшего уменьшения требований к хранилищу в облаке. Дедупликация уменьшает общий объем сохраненных данных, устраняя избыточность в наборе сохраненных данных. По мере изменения сведений StorSimple пропускает неизмененные данные, захватывая только изменения. Кроме того, StorSimple сокращает объем сохраненных данных, определяя и удаляя повторяющуюся информацию.

> [!NOTE]
> Данные, хранящиеся в виртуальном массиве, не подвергаются дедупликации или сжатию. Дедупликация и сжатие выполняются непосредственно перед отправкой данных в облако.

### <a name="scheduled-and-on-demand-backups"></a>резервное копирование по расписанию и по требованию.

Функции защиты данных StorSimple позволяют создавать резервные копии по требованию. Кроме того, расписание резервного копирования по умолчанию гарантирует ежедневное резервное копирование данных. Резервные копии создаются в виде добавочных моментальных снимков, которые хранятся в облаке. Моментальные снимки, которые записывают изменения только с момента последнего резервного копирования, можно создать и быстро восстановить. Они могут быть критически важными в сценариях аварийного восстановления, так как заменяют дополнительные системы хранения данных (например, систему резервного копирования на магнитную ленту) и позволяют восстанавливать данные в центр обработки данных или на альтернативные сайты, если это необходимо.

## <a name="managing-personal-information"></a>Управление персональными данными

Диспетчер устройств StorSimple для виртуальной серии собирает персональные данные в двух ключевых экземплярах:
 - Параметры оповещений пользователей, где настраиваются адреса электронной почты пользователей. Эти сведения может удалить администратор. 
 - Пользователи, которые могут обращаться к данным, размещенным на общих ресурсах. Отображается список пользователей, которым разрешен доступ к данным в общем ресурсе. Этот список может быть экспортирован. Этот список также удаляется при удалении общих ресурсов.

Дополнительные сведения см. в [политике конфиденциальности Майкрософт в центре управления безопасностью](https://www.microsoft.com/trustcenter).

## <a name="next-steps"></a>Дальнейшие действия

Узнайте, [как подготовить портал виртуального массива](storsimple-virtual-array-deploy1-portal-prep.md).
