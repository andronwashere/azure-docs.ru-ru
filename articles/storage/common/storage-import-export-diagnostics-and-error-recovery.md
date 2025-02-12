---
title: Диагностика и восстановление после ошибок для заданий импорта и экспорта Azure | Документация Майкрософт
description: Узнайте, как включить подробное ведение журнала для заданий службы импорта и экспорта Microsoft Azure.
author: muralikk
services: storage
ms.service: storage
ms.topic: article
ms.date: 01/23/2017
ms.author: muralikk
ms.subservice: common
ms.openlocfilehash: 306b94fbe23e0ae92dcd59f7a87b7bb58ef7c3b6
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "61478816"
---
# <a name="diagnostics-and-error-recovery-for-azure-importexport-jobs"></a>Диагностика и восстановление после ошибок для заданий импорта и экспорта Azure
Для каждого обрабатываемого диска служба импорта и экспорта Azure создает журнал ошибок в соответствующей учетной записи хранения. Можно также включить ведение подробного журнала, задав для свойства `LogLevel` значение `Verbose` при вызове операции [Put Job](/rest/api/storageimportexport/jobs) или [Update Job Properties](/rest/api/storageimportexport/jobs).

 По умолчанию журналы записываются в контейнер `waimportexport`. При вызове операции `Put Job` или `Update Job Properties` в свойстве `DiagnosticsPath` можно указать другое имя. Журналы хранятся в виде блочных BLOB-объектов, для которых действует следующее соглашение об именовании: `waies/jobname_driveid_timestamp_logtype.xml`.

 Универсальный код ресурса (URI) журналов для задания можно получить, вызвав операцию [Get Job](/rest/api/storageimportexport/jobs). Универсальный код ресурса (URI) подробного журнала возвращается в свойство `VerboseLogUri` для каждого диска, а универсальный код ресурса (URI) журнала ошибок — в свойство `ErrorLogUri`.

Данные журнала можно использовать для выявления следующих проблем.

## <a name="drive-errors"></a>Ошибки диска

К ошибкам диска относятся следующие ошибки:

-   Ошибки при доступе к файлу манифеста или его чтении.

-   Неправильные ключи BitLocker.

-   Ошибки операций чтения или записи с диском.

## <a name="blob-errors"></a>Ошибки больших двоичных объектов

К ошибкам больших двоичных объектов относятся следующие ошибки:

-   Неправильные либо конфликтующие большие двоичные объекты или их имена.

-   Отсутствуют файлы.

-   Не удалось найти большой двоичный объект.

-   Усеченные файлы (файлы на диске меньше, чем указано в манифесте).

-   Повреждено содержимое файла (для заданий импорта, выявляется по несовпадению контрольной суммы MD5).

-   Повреждены файлы метаданных и свойств большого двоичного объекта (обнаружено несовпадение контрольной суммы MD5).

-   Неправильная схема для файлов свойств и (или) метаданных большого двоичного объекта.

Возможны ситуации, когда некоторые части задания импорта или экспорта не завершаются успешно, хотя само задание считается выполненным. В этом случае можно передать или скачать отсутствующие части данных по сети, можно также создать новое задание для передачи данных. Ознакомьтесь со [справочником по инструменту импорта и экспорта Azure](storage-import-export-tool-how-to-v1.md), чтобы узнать, как восстанавливать данные по сети.

## <a name="next-steps"></a>Дальнейшие действия

* [Использование REST API службы импорта и экспорта Azure](storage-import-export-using-the-rest-api.md)
