---
title: Предварительный просмотр использования дисков для задания экспорта инструмента импорта и экспорта Azure версии 1 | Документация Майкрософт
description: Узнайте, как использовать предварительный просмотр списка больших двоичных объектов, выбранных для задания экспорта в службе импорта и экспорта Azure.
author: muralikk
services: storage
ms.service: storage
ms.topic: article
ms.date: 01/15/2017
ms.author: muralikk
ms.subservice: common
ms.openlocfilehash: 53ab1e28c5864b403d52bf5e73f0c5c41b8f18a8
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "61478459"
---
# <a name="previewing-drive-usage-for-an-export-job"></a>Предварительный просмотр использования дисков для задания экспорта
Прежде чем создать задание импорта, необходимо выбрать набор больших двоичных объектов для экспорта. Служба импорта и экспорта Microsoft Azure позволяет использовать список путей к большим двоичным объектам или их префиксов для выбора больших двоичных объектов.  
  
Затем необходимо определить, какое количество дисков нужно отправить. Инструмент импорта и экспорта предоставляет команду `PreviewExport`, позволяющую предварительно просмотреть использование дисков для выбранных больших двоичных объектов в зависимости от объема дисков, которые вы собираетесь использовать.

## <a name="command-line-parameters"></a>Параметры командной строки

При использовании команды `PreviewExport` инструмента импорта и экспорта можно использовать следующие параметры.

|Параметр командной строки|Описание|  
|--------------------------|-----------------|  
|**/logdir:** <каталог_журнала\>|Необязательный элемент. Каталог журнала. В этот каталог записываются файлы подробного журнала. Если каталог журнала не указан, вместо него будет использоваться текущий каталог.|  
|**/sn:** <имя_учетной_записи_хранения\>|Обязательный элемент. Имя учетной записи хранения для задания экспорта.|  
|**/sk:** ключ_учетной_записи_хранения\>|Требуется, только если не указан SAS контейнера. Ключ учетной записи хранения для задания экспорта.|  
|**/csas:** <SAS_контейнера\>|Требуется, только если не указан ключ учетной записи хранения. SAS контейнера для получения списка экспортируемых в рамках задания экспорта больших двоичных объектов.|  
|**/ExportBlobListFile:** <файл_списка_больших_двоичных_объектов_экспорта\>|Обязательный элемент. Путь к XML-файлу, содержащему список путей к большим двоичным объектам или префиксов путей экспортируемых больших двоичных объектов. Формат файла, используемый в элементе `BlobListBlobPath` в операции [Put Job](/rest/api/storageimportexport/jobs) интерфейса REST API службы импорта и экспорта.|  
|**/DriveSize:** <размер_диска\>|Обязательный элемент. Размер дисков, используемых для задания экспорта, *например* 500 ГБ, 1,5 ТБ.|  

## <a name="command-line-example"></a>Пример для командной строки

В следующем примере показано использование команды `PreviewExport`:  
  
```  
WAImportExport.exe PreviewExport /sn:bobmediaaccount /sk:VkGbrUqBWLYJ6zg1m29VOTrxpBgdNOlp+kp0C9MEdx3GELxmBw4hK94f7KysbbeKLDksg7VoN1W/a5UuM2zNgQ== /ExportBlobListFile:C:\WAImportExport\mybloblist.xml /DriveSize:500GB    
```  
  
Файл списка больших двоичных объектов экспорта может содержать имена и префиксы больших двоичных объектов, как показано ниже:  
  
```xml 
<?xml version="1.0" encoding="utf-8"?>  
<BlobList>  
<BlobPath>pictures/animals/koala.jpg</BlobPath>  
<BlobPathPrefix>/vhds/</BlobPathPrefix>  
<BlobPathPrefix>/movies/</BlobPathPrefix>  
</BlobList>  
```

Инструмент импорта и экспорта Azure получает список всех экспортируемых больших двоичных объектов и вычисляет способ их размещения на дисках определенного размера с учетом любой необходимой нагрузки, а затем определяет количество дисков, необходимое для хранения больших двоичных объектов и сведений об использовании диска.  
  
Ниже приведен пример выходных данных без журналов сведений:  
  
```  
Number of unique blob paths/prefixes:   3  
Number of duplicate blob paths/prefixes:        0  
Number of nonexistent blob paths/prefixes:      1  
  
Drive size:     500.00 GB  
Number of blobs that can be exported:   6  
Number of blobs that cannot be exported:        2  
Number of drives needed:        3  
        Drive #1:       blobs = 1, occupied space = 454.74 GB  
        Drive #2:       blobs = 3, occupied space = 441.37 GB  
        Drive #3:       blobs = 2, occupied space = 131.28 GB    
```  
  
## <a name="next-steps"></a>Дальнейшие действия

* [Руководство по использованию инструмента импорта и экспорта](../storage-import-export-tool-how-to-v1.md)
