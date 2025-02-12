---
title: Мониторинг Синхронизации файлов Azure | Документация Майкрософт
description: Узнайте, как выполнять мониторинг Синхронизации файлов Azure.
services: storage
author: roygara
ms.service: storage
ms.topic: article
ms.date: 06/28/2019
ms.author: rogarana
ms.subservice: files
ms.openlocfilehash: 86c4bf328430bbc623d8e493eec5db520d50ef82
ms.sourcegitcommit: 9b80d1e560b02f74d2237489fa1c6eb7eca5ee10
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/01/2019
ms.locfileid: "67485979"
---
# <a name="monitor-azure-file-sync"></a>Мониторинг службы "Синхронизация файлов Azure"

Используйте службу "Синхронизация файлов Azure", чтобы централизованно хранить файловые ресурсы организации в службе файлов Azure, обеспечивая гибкость, производительность и совместимость локального файлового сервера. Это достигается путем преобразования Windows Server в быстрый кэш общего файлового ресурса Azure. Для локального доступа к данным вы можете использовать любой протокол, доступный в Windows Server, в том числе SMB, NFS и FTPS. Кроме того, вы можете создать любое количество кэшей в любом регионе.

В этой статье описывается, как выполнять мониторинг развертывания службы синхронизации файлов Azure с помощью Azure Monitor, служба синхронизации хранилища и Windows Server.

В настоящее время доступны следующие параметры наблюдения.

## <a name="azure-monitor"></a>Azure Monitor

Используйте [Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/overview) для просмотра метрик и настройки оповещений для синхронизации облачные распределение по уровням и подключения к серверу.  

### <a name="metrics"></a>metrics

Метрики для Синхронизации файлов Azure включены по умолчанию и отправляются в Azure Monitor каждые 15 минут.

Чтобы просмотреть метрики службы синхронизации файлов Azure в Azure Monitor, выберите **служб синхронизации хранилища** тип ресурса.

В Azure Monitor используются следующие метрики для Синхронизации файлов Azure:

| Название метрики | Описание |
|-|-|
| Синхронизировано байт | Размер переданных данных (отправка и скачивание).<br><br>Единица измерения: Байты<br>Тип агрегирования: Sum<br>Применимые измерения. Имя конечной точки сервера, направление синхронизации, имя группы синхронизации |
| Отзыв уровней в облаке | Размер отозванных данных.<br><br>**Примечание**. Эта метрика будет удалена в будущем. Используется размер метрика распределения по уровням отзыв облака для мониторинга объема данных отозвать.<br><br>Единица измерения: Байты<br>Тип агрегирования: Sum<br>Применяемые измерения: Имя сервера |
| Облако распределения по уровням размер отзыва | Размер отозванных данных.<br><br>Единица измерения: Байты<br>Тип агрегирования: Sum<br>Применяемые измерения: Имя сервера имя, группу синхронизации |
| Облако распределения по уровням размер отзыв приложением | Размер данных, инициированные приложением.<br><br>Единица измерения: Байты<br>Тип агрегирования: Sum<br>Применяемые измерения: Приложения имя, сервер имя, имя группы синхронизации |
| Пропускную способность отзыв распределения по уровням в облаке | Объем пропускной способности данных об отзыве.<br><br>Единица измерения: Байты<br>Тип агрегирования: Sum<br>Применяемые измерения: Имя сервера имя, группу синхронизации |
| Несинхронизирующиеся файлы | Число файлов, которые не удалось синхронизировать.<br><br>Единица измерения: Количество<br>Тип агрегирования: Sum<br>Применимые измерения. Имя конечной точки сервера, направление синхронизации, имя группы синхронизации |
| Синхронизировано файлов | Число переданных файлов (отправка и скачивание).<br><br>Единица измерения: Количество<br>Тип агрегирования: Sum<br>Применимые измерения. Имя конечной точки сервера, направление синхронизации, имя группы синхронизации |
| Сетевой статус сервера | Число пульсов, полученных от сервера.<br><br>Единица измерения: Количество<br>Тип агрегирования: Максимальная<br>Применяемые измерения: Имя сервера |
| Результат сеанса синхронизации | Результат сеанса синхронизации (1 — успешный сеанс синхронизации; 0 — сбой синхронизации).<br><br>Единица измерения: Количество<br>Типы агрегирования: Максимальная<br>Применимые измерения. Имя конечной точки сервера, направление синхронизации, имя группы синхронизации |

### <a name="alerts"></a>Оповещения

Настройка оповещений в Azure Monitor, выберите службу синхронизации хранилища, а затем выберите [метрики службы синхронизации файлов Azure](https://docs.microsoft.com/azure/storage/files/storage-sync-files-monitoring#metrics) для оповещения.  

В следующей таблице перечислены примеры сценариев для мониторинга и соответствующие метрика, используемая для оповещения:

| Сценарий | Метрику для использования для предупреждения |
|-|-|
| Работоспособность конечной точки сервера на портале = ошибка | Результат сеанса синхронизации |
| Файлы не удалось синхронизировать с сервера или облачная конечная точка | Несинхронизирующиеся файлы |
| Зарегистрированный сервер не смог связаться со службой синхронизации хранилища | Сетевой статус сервера |
| Облако распределения по уровням отзыв превысил 500GiB за день  | Облако распределения по уровням размер отзыва |

Дополнительные сведения о настройке оповещений в Azure Monitor, см. в разделе [Обзор оповещений в Microsoft Azure]( https://docs.microsoft.com/azure/azure-monitor/platform/alerts-overview).

## <a name="storage-sync-service"></a>Служба синхронизации хранилища

Чтобы просмотреть зарегистрированного сервера работоспособность, работоспособность конечной точки сервера и метрики, перейдите к службе синхронизации хранилища на портале Azure. Вы можете проверить работоспособность зарегистрированного сервера в **зарегистрированных серверов** колонке и конечной точки исправности сервера в **группы синхронизации** колонке.

### <a name="registered-server-health"></a>Работоспособность зарегистрированного сервера

- Если **Registered server** находится в состоянии **Online**, сервер успешно взаимодействует со службой.
- Если **Registered server** находится в состоянии **появляется в автономном режиме**, убедитесь, что выполняется процесс монитор синхронизации хранилища (AzureStorageSyncMonitor.exe) на сервере. Если сервер находится за брандмауэром или прокси-сервера, см. в разделе [в этой статье](https://docs.microsoft.com/azure/storage/files/storage-sync-files-firewall-and-proxy) для настройки брандмауэра и прокси-сервера.

### <a name="server-endpoint-health"></a>Работоспособность конечной точки сервера

- Состояние работоспособности конечной точки сервера на портале основано на событиях синхронизации, регистрируемых в журнале событий телеметрии на сервере (с идентификатором 9102 и 9302). Сеанс синхронизации из-за временной ошибки, такие как ошибка отменена в случае сбоя синхронизации может по-прежнему отображаться работоспособном состоянии на портале, до тех пор, пока текущий сеанс синхронизации ведет работу. Код события 9302 используется для определения, если файлы применяются. Дополнительные сведения см. в разделе [синхронизировать работоспособности](https://docs.microsoft.com/azure/storage/files/storage-sync-files-troubleshoot?tabs=server%2Cazure-portal#broken-sync) и [ход выполнения синхронизации](https://docs.microsoft.com/azure/storage/files/storage-sync-files-troubleshoot?tabs=server%2Cazure-portal#how-do-i-monitor-the-progress-of-a-current-sync-session).
- Если на портале отображается ошибка синхронизации, так как синхронизация не выполняется, см. в разделе [документацию по устранению неполадок](https://docs.microsoft.com/azure/storage/files/storage-sync-files-troubleshoot?tabs=portal1%2Cazure-portal#common-sync-errors) рекомендации.

### <a name="metric-charts"></a>Диаграммы метрик

- Следующие диаграммы метрик отображаются на портале службы синхронизации хранилища:

  | Название метрики | Описание | Имя колонки |
  |-|-|-|
  | Синхронизировано байт | Размер переданных данных (отправка и скачивание) | Группа синхронизации, конечная точка сервера |
  | Отзыв уровней в облаке | Размер отозванных данных | Зарегистрированные серверы |
  | Несинхронизирующиеся файлы | Число файлов, которые не удалось синхронизировать | Конечная точка сервера |
  | Синхронизировано файлов | Число переданных файлов (отправка и скачивание) | Группа синхронизации, конечная точка сервера |
  | Сетевой статус сервера | Число пульсов, полученных от сервера | Зарегистрированные серверы |

- Дополнительные сведения см. в разделе [Azure Monitor](https://docs.microsoft.com/azure/storage/files/storage-sync-files-monitoring#azure-monitor).

  > [!Note]  
  > Диапазон времени для диаграммы на портале службы синхронизации хранилища составляет 24 часа. Чтобы просмотреть различные периоды времени или измерения, используйте Azure Monitor.

## <a name="windows-server"></a>Windows Server

В Windows Server можно просматривать распределения по уровням, облака зарегистрированном сервере и синхронизировать работоспособности.

### <a name="event-logs"></a>Журналы событий

Используйте журнал событий телеметрии на сервере, чтобы отслеживать работоспособность зарегистрированного сервера, синхронизации и распределения по уровням в облаке. Журнал событий телеметрии находится в средстве просмотра событий в *приложений и Services\Microsoft\FileSync\Agent*.

Работоспособность синхронизации:

- После завершения сеанса синхронизации, регистрируется событие с Идентификатором 9102. Используйте это событие, чтобы определить, успешно ли сеансов синхронизации (**HResult = 0**), а также-item ошибка синхронизации. Дополнительные сведения см. в разделе [синхронизировать работоспособности](https://docs.microsoft.com/azure/storage/files/storage-sync-files-troubleshoot?tabs=server%2Cazure-portal#broken-sync) и [ошибки на элемент](https://docs.microsoft.com/azure/storage/files/storage-sync-files-troubleshoot?tabs=server%2Cazure-portal#how-do-i-see-if-there-are-specific-files-or-folders-that-are-not-syncing) документации.

  > [!Note]  
  > Иногда сеансов синхронизации в целом ошибкой или иметь PerItemErrorCount ненулевое значение. Тем не менее они по-прежнему продолжать работу и успешно синхронизировать некоторые файлы. Это видно в примененных полях, например AppliedFileCount, AppliedDirCount, AppliedTombstoneCount и AppliedSizeBytes. Эти поля сообщит о том, какая часть сеанса выполнена успешно. Если вы видите последовательных ошибок в несколько сеансов синхронизации, и они имеют все большее число примененных, предоставьте время синхронизации, чтобы повторить попытку, прежде чем открыть запрос в службу поддержки.

- Идентификатор события 9302 регистрируется каждые 5–10 минут при активном сеансе синхронизации. Это событие используется для определения того, если текущий сеанс синхронизации ведет работу (**AppliedItemCount > 0**). Если синхронизация не выполняется, со временем не выполняется, сеанс синхронизации и 9102 идентификатор события регистрируются с ошибкой. Дополнительные сведения см. в разделе [синхронизировать документации работам](https://docs.microsoft.com/azure/storage/files/storage-sync-files-troubleshoot?tabs=server%2Cazure-portal#how-do-i-monitor-the-progress-of-a-current-sync-session).

Работоспособность зарегистрированного сервера:

- Если сервер отправляет службе запросы, связанные с выполнением заданий, идентификатор события 9301 регистрируется каждые 30 секунд. Если GetNextJob по окончании **состояние = 0**, он может взаимодействовать со службой. Если GetNextJob завершается с ошибкой, проверьте [документацию по устранению неполадок](https://docs.microsoft.com/azure/storage/files/storage-sync-files-troubleshoot?tabs=portal1%2Cazure-portal#common-sync-errors) рекомендации.

Работоспособность распределения по уровням в облаке.

- Чтобы наблюдать за использованием распределения по уровням на сервере, используйте 9003 идентификатор события, события 9016 и 9029 в журнале событий телеметрии, который находится в средстве просмотра событий в *приложений и Services\Microsoft\FileSync\Agent*.

  - Идентификатор события 9003 обеспечивает распределение ошибок для конечной точки сервера. Пример: Общее число ошибок и код ошибки. Одно событие записывается в код ошибки.
  - Идентификатор события 9016 предоставляет результаты копий для тома. Пример: — Процент свободного места, количество файлов, синхронизирован. в сеансе, и количество файлов не смог фантомных записей.
  - Идентификатор события 9029 предоставляет сведения о сеансе копирования для конечной точки сервера. Пример: Число файлов в сеансе, число файлов многоуровневые в сеансе, и количество файлов уже многоуровневые.
  
- Чтобы отслеживать активность отзыва на сервере, используйте 9005 идентификатор события, 9006, 9009 и 9059 в журнале событий телеметрии, который находится в средстве просмотра событий в *приложений и Services\Microsoft\FileSync\Agent*.

  - Идентификатор события 9005 обеспечивает надежность отзыва для конечной точки сервера. Пример: Общее количество уникальных файлов доступны и общее уникальных файлов с неудачные попытки доступа.
  - Идентификатор события 9006 обеспечивает распределение ошибок отзыва для конечной точки сервера. Пример: Общее число невыполненных запросов и код ошибки. Одно событие записывается в код ошибки.
  - Идентификатор события 9009 предоставляет сведения о сеансе отзыва для конечной точки сервера. Пример: DurationSeconds CountFilesRecallSucceeded и CountFilesRecallFailed.
  - Идентификатор события 9059 предоставляет сведения об отзыве приложения для конечной точки сервера. Пример: ShareId, имя приложения и TotalEgressNetworkBytes.

### <a name="performance-counters"></a>Счетчики производительности

Воспользуйтесь счетчиками производительности Синхронизации файлов Azure на сервере для отслеживания действия синхронизации.

Чтобы просмотреть счетчики производительности службы синхронизации файлов Azure на сервере, откройте системный монитор (Perfmon.exe). Счетчики в категорию можно найти **число переданных байтов AFS** и **операций синхронизации AFS** объектов.

В системном мониторе можно просмотреть следующие счетчики производительности Синхронизации файлов Azure:

| Объект \ Имя счетчика производительности | Описание |
|-|-|
| AFS Bytes Transferred \ Downloaded Bytes/sec (Число переданных байт AFS \ Число байт, скачиваемых в секунду) | Число байт, скачиваемых в секунду. |
| AFS Bytes Transferred \ Uploaded Bytes/sec (Количество переданных байт AFS \ Число байт, отправляемых в секунду) | Число байт, отправляемых в секунду. |
| AFS Bytes Transferred \ Total Bytes/sec (Число переданных байт AFS \ Общее число байт в секунду) | Общее число байт в секунду (отправка и скачивание). |
| AFS Sync Operations \ Downloaded Sync Files/sec (Операции синхронизации AFS \ Число файлов, скачиваемых в секунду) | Число файлов, скачиваемых в секунду. |
| AFS Sync Operations \ Uploaded Sync Files/sec (Операции синхронизации AFS \ Число файлов, отправляемых в секунду) | Число файлов, отправляемых в секунду. |
| AFS Sync Operations \ Total Sync File Operations/sec (Операции синхронизации AFS \ Общее число операций синхронизации файлов в секунду) | Общее число синхронизируемых файлов (отправка и скачивание). |

## <a name="next-steps"></a>Дальнейшие действия
- [Планирование развертывания службы синхронизации файлов Azure (предварительная версия)](storage-sync-files-planning.md)
- [Параметры брандмауэра и прокси-сервера Синхронизации файлов Azure](storage-sync-files-firewall-and-proxy.md)
- [Как развернуть службу синхронизации файлов Azure (предварительная версия)](storage-sync-files-deployment-guide.md)
- [Устранение неполадок службы "Синхронизация файлов Azure"](storage-sync-files-troubleshoot.md)
- [Часто задаваемые вопросы о службе "Файлы Azure"](storage-files-faq.md)
