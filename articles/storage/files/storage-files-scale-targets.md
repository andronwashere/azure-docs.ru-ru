---
title: Целевые показатели масштабируемости и производительности службы файлов Azure | Документация Майкрософт
description: 'Ознакомьтесь с такими целевыми показателями масштабируемости и производительности службы файлов Аzure: емкость, частота запросов и предельная входящая и исходящая пропускная способность.'
services: storage
author: roygara
ms.service: storage
ms.topic: article
ms.date: 5/5/2019
ms.author: rogarana
ms.subservice: files
ms.openlocfilehash: 91ec65e17b77ccb3864fce45e30729ff420a48b6
ms.sourcegitcommit: 5bdd50e769a4d50ccb89e135cfd38b788ade594d
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/03/2019
ms.locfileid: "67542652"
---
# <a name="azure-files-scalability-and-performance-targets"></a>Целевые показатели масштабируемости и производительности службы файлов Azure

[Служба файлов Azure](storage-files-introduction.md) предлагает полностью управляемые общие файловые ресурсы в облаке, доступ к которым можно получить с помощью стандартного отраслевого протокола SMB. В этой статье описываются целевые показатели масштабируемости и производительности службы файлов Azure и службы синхронизации файлов Azure.

Приведенные здесь целевые показатели масштабируемости и производительности предельно высоки, но на них могут оказывать негативное воздействие другие параметры вашего развертывания. Например, пропускная способность по файлу может быть ограничена не только серверами, на которых размещена служба файлов Azure, но и доступной пропускной способностью вашей сети. Мы настоятельно рекомендуем протестировать модель использования, чтобы определить, соответствуют ли показатели масштабируемости и производительности службы файлов Azure вашим требованиям. Мы также стремимся постепенно увеличивать предельные показатели. Мы ждем ваших отзывов о том, какие предельные показатели вы хотели бы увеличить. Отзывы можно оставить в комментариях ниже или на форуме [UserVoice службы файлов Azure](https://feedback.azure.com/forums/217298-storage/category/180670-files).

## <a name="azure-storage-account-scale-targets"></a>Целевые показатели масштабируемости для учетной записи хранения Azure

Учетная запись хранения Azure является родительским объектом общей папки Azure. Учетная запись хранения представляет собой пул носителей в Azure, которые можно использовать для хранения данных в разных службах хранения, включая службу файлов Azure. Другими службами для хранения данных в учетных записях хранения являются хранилище BLOB-объектов Azure, хранилище очередей Azure и табличное хранилище Azure. Следующие целевые показатели касаются всех служб хранения, которые сохраняют данные в учетных записях хранения:

[!INCLUDE [azure-storage-limits](../../../includes/azure-storage-limits.md)]

[!INCLUDE [azure-storage-limits-azure-resource-manager](../../../includes/azure-storage-limits-azure-resource-manager.md)]

> [!Important]  
> Использование учетной записи хранения общего назначения в других службах хранения влияет на файловые ресурсы Azure в вашей учетной записи хранения. Например, при достижении максимальной емкости учетной записи хранения в службе хранилища BLOB-объектов Azure вы не сможете создать файлы в общей папке Azure, даже если ее размер не превышает максимального.

## <a name="azure-files-scale-targets"></a>Целевые показатели масштабируемости службы файлов Azure

Существует три категории ограничения для службы файлов Azure: учетные записи хранения, общие папки и файлы.

Пример: С файловыми ресурсами уровня "премиум" имеет одну общую папку можно достичь 100 000 операций ввода-ВЫВОДА и один файл можно масштабировать до 5000 операций ввода-ВЫВОДА. Поэтому, если у вас есть три файла в одну общую папку, максимальное число IOPS, можно получить из этого общедоступного ресурса 15 000.

### <a name="standard-storage-account-limits"></a>Ограничения учетной записи хранилища класса Standard

См. в разделе [целевые показатели масштабируемости учетной записи службы хранилища Azure](#azure-storage-account-scale-targets) раздел для этих ограничений.

### <a name="premium-filestorage-account-limits"></a>Ограничения учетной записи хранилища уровня "премиум"

[!INCLUDE [azure-storage-limits-filestorage](../../../includes/azure-storage-limits-filestorage.md)]

> [!IMPORTANT]
> Ограничения учетной записи хранения распространяются на все общие папки. Масштабирование до max для учетных записей хранилища — это только достигаемое в том случае, если имеется только один общий ресурс на одну учетную запись хранилища.

### <a name="file-share-and-file-scale-targets"></a>Общая папка и файл целевые показатели масштабируемости

> [!NOTE]
> Стандартные общие папки, размер которых превышает 5 Тиб доступны в предварительной версии и имеют некоторые ограничения.
> Список ограничений и для подключения к предварительной версии этих больших размеров файлов общего ресурса, см. в разделе [стандартные файловые ресурсы](storage-files-planning.md#standard-file-shares) разделе руководства по планированию.

[!INCLUDE [storage-files-scale-targets](../../../includes/storage-files-scale-targets.md)]

[!INCLUDE [storage-files-premium-scale-targets](../../../includes/storage-files-premium-scale-targets.md)]

## <a name="azure-file-sync-scale-targets"></a>Целевые показатели масштабируемости службы "Синхронизация файлов Azure"

Служба синхронизации файлов Azure разрабатывалась для использования без ограничений, но такое использование не всегда возможно. В таблице указаны пределы тестирования, проведенного Майкрософт, и жесткие ограничения для некоторых целевых показателей.

[!INCLUDE [storage-sync-files-scale-targets](../../../includes/storage-sync-files-scale-targets.md)]

### <a name="azure-file-sync-performance-metrics"></a>Метрики производительности службы синхронизации файлов Azure

Так как агент службы "Синхронизация файлов Azure" выполняется на компьютере Windows Server, подключенном к файловым ресурсам Azure, эффективная производительность синхронизации зависит от ряда факторов в инфраструктуре: Windows Server и базовая конфигурация диска, пропускная способность сети между сервером и службой хранилища Azure, размер файла, общий размер набора данных и действие, выполняемое по отношению к набору данных. Так как служба синхронизации файлов Azure работает на уровне файлов, характеристики производительности решения на основе этой службы лучше измеряются в количестве обрабатываемых объектов (файлов и каталогов) в секунду.

Для службы синхронизации файлов Azure производительность имеет решающее значение на двух этапах:

1. **Одноразовая начальная подготовка.** Сведения об оптимальном развертывании для оптимизации производительности при начальной подготовке см. в разделе [Подключение с помощью службы "Синхронизация файлов Azure"](storage-sync-files-deployment-guide.md#onboarding-with-azure-file-sync).
2. **Непрерывная синхронизация.** После первоначального добавления данных в общие папки Azure служба "Синхронизация файлов Azure" синхронизирует несколько конечных точек.

Чтобы помочь вам спланировать развертывание для каждого из этапов, ниже приведены результаты, полученные во время внутреннего тестирования в системе со следующей конфигурацией.

| Конфигурация системы |  |
|-|-|
| ЦП | 64 виртуальных ядер с кэшем L3 на 64 МиБ |
| Память | 128 ГБ |
| Диск | Диски SAS с RAID 10 и кэшем, работающем от аккумулятора |
| Сеть | Сеть 1 Гбит/с |
| Рабочая нагрузка | Файловый сервер общего назначения|

| Одноразовая начальная подготовка  |  |
|-|-|
| Число объектов | 25 млн объектов |
| Размер набора данных| ~4.7 Тиб |
| Средний размер файлов | ~ 200 Киб (размер самого большого файла: 100 Гиб) |
| Пропускная способность отправки | 20 объектов в секунду |
| Пропускная способность скачивания пространств имен* | 400 объектов в секунду |

\* При создании новой конечной точки сервера агент синхронизации файлов Azure не скачивает содержимое файла. Сначала он синхронизирует полное пространство имен, а затем запускает фоновый вызов для скачивания файлов, либо полностью, либо, если включено распределение по уровням облака, в политику распределения по уровням облака, установленную на конечной точке сервера.

| Непрерывная синхронизация  |   |
|-|--|
| Количество синхронизированных объектов| 125 000 объектов (~1 % обновления) |
| Размер набора данных| 50 ГиБ |
| Средний размер файлов | ~500 КиБ |
| Пропускная способность отправки | 30 объектов в секунду |
| Полная пропускная способность скачивания* | 60 объектов в секунду |

\* Если включено распределение по уровням облака, вы, скорее всего, будете наблюдать лучшую производительность, так как скачиваются только некоторые данные файлов. Служба синхронизации файлов Azure скачивает данные кэшированных файлов только при их изменении в любой из конечных точек. Для любых многоуровневых или вновь созданных файлов агент не скачивает данные файла и вместо этого синхронизирует пространство имен со всеми конечными точками сервера. Агент также поддерживает частичное скачивание многоуровневых файлов по мере получения доступа к ним пользователем. 

> [!Note]  
> Цифры выше не являются показателем наблюдаемой производительности. Фактическая производительность будет зависеть от нескольких факторов, как указано в начале этого раздела.

В качестве общего руководства по развертыванию вы должны учитывать следующие моменты:

- Пропускная способность объекта приблизительно масштабируется пропорционально количеству групп синхронизации на сервере. Разделение данных на несколько групп синхронизации на сервере улучшает пропускную способность, которая также ограничена сервером и сетью.
- Пропускная способность объекта обратно пропорциональна пропускной способности в МиБ в секунду. Для файлов меньшего размера вы получите более высокую пропускную способность с точки зрения количества объектов, обработанных в секунду, но меньшую пропускную способность в МиБ в секунду. И наоборот, для файлов большего размера будет обработано меньше объектов в секунду, но с более высокой пропускной способностью в МиБ в секунду. Пропускная способность в МиБ в секунду ограничена целевыми объектами масштабирования файлов Azure.

## <a name="see-also"></a>См. также

- [Планирование развертывания службы файлов Azure](storage-files-planning.md)
- [Планирование развертывания службы синхронизации файлов Azure (предварительная версия)](storage-sync-files-planning.md)
- [Целевые показатели масштабируемости и производительности службы хранилища Azure](../common/storage-scalability-targets.md)
