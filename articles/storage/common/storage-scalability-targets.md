---
title: Целевые показатели масштабируемости и производительности службы хранилища Azure — учетные записи хранения
description: Сведения о целевых показателях масштабируемости и производительности, включая емкость, частоту запросов, а также входящую и исходящую пропускную способность, для учетных записей хранения Azure.
services: storage
author: roygara
ms.service: storage
ms.topic: conceptual
ms.date: 07/18/2019
ms.author: rogarana
ms.subservice: common
ms.openlocfilehash: 046c2308d5cef2df7e12b6185fc24b8df4f821dc
ms.sourcegitcommit: 4b431e86e47b6feb8ac6b61487f910c17a55d121
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/18/2019
ms.locfileid: "68326946"
---
# <a name="azure-storage-scalability-and-performance-targets-for-storage-accounts"></a>Целевые показатели масштабируемости и производительности службы хранилища Azure для учетных записей хранения

В статье подробно рассматриваются целевые показатели масштабируемости и производительности для учетных записей хранения Azure. Приведенные целевые показатели производительности и масштабируемости предельно высоки, но достижимы. В любом случае частота запросов, с которой успешно справляется учетная запись хранения, и ее пропускная способность зависят от размера хранимых объектов, используемых схем доступа и типа рабочей нагрузки приложения.

Обязательно протестируйте службу для определения соответствия ее производительности необходимым требованиям. По возможности избегайте внезапных пиковых нагрузок по трафику. Убедитесь в том, что трафик соответствующим образом распределяется по разделам.

Когда при работе приложения достигается предельная рабочая нагрузка на раздел, служба хранилища Azure начинает выдавать код ошибки 503 (сервер занят) или 500 (время ожидания операции истекло). При возникновении ошибок 503 попробуйте изменить приложение, чтобы при повторных попытках оно использовало политику экспоненциальной задержки. Экспоненциальное откладывание позволяет уменьшить нагрузку на раздел и облегчить обработку им пикового трафика.

## <a name="storage-account-scale-limits"></a>Ограничения масштабирования учетной записи хранения

[!INCLUDE [azure-storage-limits](../../../includes/azure-storage-limits.md)]

## <a name="premium-performance-storage-account-scale-limits"></a>Ограничения масштабирования учетной записи хранилища производительности Premium

[!INCLUDE [azure-premium-limits](../../../includes/azure-storage-limits-premium.md)]

## <a name="storage-resource-provider-scale-limits"></a>Ограничения масштабируемости поставщика ресурсов хранилища

[!INCLUDE [azure-storage-limits-azure-resource-manager](../../../includes/azure-storage-limits-azure-resource-manager.md)]

## <a name="azure-blob-storage-scale-targets"></a>Целевые показатели масштабируемости хранилища BLOB-объектов Azure

[!INCLUDE [storage-blob-scale-targets](../../../includes/storage-blob-scale-targets.md)]

## <a name="azure-files-scale-targets"></a>Целевые показатели масштабируемости службы файлов Azure

Дополнительные сведения о целевых показателях масштабируемости и производительности для службы файлов Azure и службы синхронизации файлов Azure см. в [этой статье](../files/storage-files-scale-targets.md).

> [!IMPORTANT]
> Ограничения учетной записи хранения применяются ко всем общим ресурсам. Масштабирование до максимального размера для учетных записей хранения возможно, только если для каждой учетной записи хранения существует только одна общая папка.
>
> Стандартные общие файловые ресурсы, размер которых превышает 5 Тиб, доступны в предварительной версии и имеют определенные ограничения.
> Список ограничений и подключение к предварительной версии этих больших размеров общих файловых ресурсов см. в разделе [Стандартные файловые ресурсы](../files/storage-files-planning.md#standard-file-shares) раздела рекомендации по планированию файлов Azure.

[!INCLUDE [storage-files-scale-targets](../../../includes/storage-files-scale-targets.md)]

### <a name="premium-files-scale-targets"></a>Целевые объекты масштабирования файлов Premium

Существуют три категории ограничений, которые следует учитывать при работе с файлами уровня "Премиум": учетные записи хранения, общие папки и файлы.

Пример: Один общий ресурс может достигать 100 000 операций ввода-вывода в секунду, а один файл — до 5 000 операций ввода-вывода. Например, если в одной общей папке имеется три файла, максимальное число операций ввода-вывода в секунду, которое можно получить из этой общей папки, — 15 000.

#### <a name="premium-file-share-limits"></a>Ограничения общей папки уровня "Премиум"

[!INCLUDE [storage-files-premium-scale-targets](../../../includes/storage-files-premium-scale-targets.md)]

### <a name="azure-file-sync-scale-targets"></a>Целевые показатели масштабируемости службы "Синхронизация файлов Azure"

Служба синхронизации файлов Azure разрабатывалась для использования без ограничений, но такое использование не всегда возможно. В таблице указаны пределы тестирования, проведенного Майкрософт, и жесткие ограничения для некоторых целевых показателей.

[!INCLUDE [storage-sync-files-scale-targets](../../../includes/storage-sync-files-scale-targets.md)]

## <a name="azure-queue-storage-scale-targets"></a>Целевые показатели масштабируемости хранилища очередей Azure

[!INCLUDE [storage-queues-scale-targets](../../../includes/storage-queues-scale-targets.md)]

## <a name="azure-table-storage-scale-targets"></a>Целевые показатели масштабируемости табличного хранилища Azure

[!INCLUDE [storage-table-scale-targets](../../../includes/storage-tables-scale-targets.md)]

## <a name="see-also"></a>См. также

- [Сведения о ценах на хранилище](https://azure.microsoft.com/pricing/details/storage/)
- [Лимиты, квоты и ограничения подписки и обслуживания Azure](../../azure-subscription-service-limits.md)
- [Репликация хранилища Azure](../storage-redundancy.md)
- [Производительность хранилища Microsoft Azure и контрольный список масштабируемости](../storage-performance-checklist.md)
