---
title: Параметры сортировки в хранилище данных SQL Azure | Документация Майкрософт
description: Типы параметров сортировки, поддерживаемые в хранилище данных SQL Azure.
services: sql-data-warehouse
author: antvgski
manager: igorstan
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.date: 07/10/2019
ms.author: anvang
ms.reviewer: jrasnick
ms.openlocfilehash: af88d2f0ce0e26fe67dda265b59684083c133dd5
ms.sourcegitcommit: a8b638322d494739f7463db4f0ea465496c689c6
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/17/2019
ms.locfileid: "68297587"
---
# <a name="database-collation-support-for-azure-sql-data-warehouse"></a>Поддержка параметров сортировки базы данных для хранилища данных SQL Azure

Параметры сортировки базы данных по умолчанию можно изменить из портал Azure при создании новой базы данных хранилища данных SQL Azure. Эта возможность упрощает создание новой базы данных с помощью одной из 3800 поддерживаемых параметров сортировки базы данных для хранилища данных SQL.
Параметры сортировки предоставляют языковой стандарт, кодовую страницу, порядок сортировки и правила учета символов для символьных типов данных. После выбора все столбцы и выражения, для которых требуются сведения о параметрах сортировки, наследуют выбранные параметры сортировки из параметра базы данных. Наследование по умолчанию может быть переопределено явным образом, указывая другие параметры сортировки для символьного типа данных.

## <a name="changing-collation"></a>Изменение параметров сортировки
Чтобы изменить параметры сортировки по умолчанию, вы просто обновляете поле Параметры сортировки в процессе подготовки.

Например, если вы хотите изменить параметры сортировки по умолчанию на с учетом регистра, просто переименуйте параметры сортировки с SQL_Latin1_General_CP1_CI_AS на SQL_Latin1_General_CP1_CS_AS. 

#<a name="list-of-unsupported-collation-types"></a>Список неподдерживаемых типов параметров сортировки
*   Japanese_Bushu_Kakusu_140_BIN
*   Japanese_Bushu_Kakusu_140_BIN2
*   Japanese_Bushu_Kakusu_140_CI_AI_VSS
*   Japanese_Bushu_Kakusu_140_CI_AI_WS_VSS
*   Japanese_Bushu_Kakusu_140_CI_AI_KS_VSS
*   Japanese_Bushu_Kakusu_140_CI_AI_KS_WS_VSS
*   Japanese_Bushu_Kakusu_140_CI_AS_VSS
*   Japanese_Bushu_Kakusu_140_CI_AS_WS_VSS
*   Japanese_Bushu_Kakusu_140_CI_AS_KS_VSS
*   Japanese_Bushu_Kakusu_140_CI_AS_KS_WS_VSS
*   Japanese_Bushu_Kakusu_140_CS_AI_VSS
*   Japanese_Bushu_Kakusu_140_CS_AI_WS_VSS
*   Japanese_Bushu_Kakusu_140_CS_AI_KS_VSS
*   Japanese_Bushu_Kakusu_140_CS_AI_KS_WS_VSS
*   Japanese_Bushu_Kakusu_140_CS_AS_VSS
*   Japanese_Bushu_Kakusu_140_CS_AS_WS_VSS
*   Japanese_Bushu_Kakusu_140_CS_AS_KS_VSS
*   Japanese_Bushu_Kakusu_140_CS_AS_KS_WS_VSS
*   Japanese_Bushu_Kakusu_140_CI_AI
*   Japanese_Bushu_Kakusu_140_CI_AI_WS
*   Japanese_Bushu_Kakusu_140_CI_AI_KS
*   Japanese_Bushu_Kakusu_140_CI_AI_KS_WS
*   Japanese_Bushu_Kakusu_140_CI_AS
*   Japanese_Bushu_Kakusu_140_CI_AS_WS
*   Japanese_Bushu_Kakusu_140_CI_AS_KS
*   Japanese_Bushu_Kakusu_140_CI_AS_KS_WS
*   Japanese_Bushu_Kakusu_140_CS_AI
*   Japanese_Bushu_Kakusu_140_CS_AI_WS
*   Japanese_Bushu_Kakusu_140_CS_AI_KS
*   Japanese_Bushu_Kakusu_140_CS_AI_KS_WS
*   Japanese_Bushu_Kakusu_140_CS_AS
*   Japanese_Bushu_Kakusu_140_CS_AS_WS
*   Japanese_Bushu_Kakusu_140_CS_AS_KS
*   Japanese_Bushu_Kakusu_140_CS_AS_KS_WS
*   Japanese_XJIS_140_BIN
*   Japanese_XJIS_140_BIN2
*   Japanese_XJIS_140_CI_AI_VSS
*   Japanese_XJIS_140_CI_AI_WS_VSS
*   Japanese_XJIS_140_CI_AI_KS_VSS
*   Japanese_XJIS_140_CI_AI_KS_WS_VSS
*   Japanese_XJIS_140_CI_AS_VSS
*   Japanese_XJIS_140_CI_AS_WS_VSS
*   Japanese_XJIS_140_CI_AS_KS_VSS
*   Japanese_XJIS_140_CI_AS_KS_WS_VSS
*   Japanese_XJIS_140_CS_AI_VSS
*   Japanese_XJIS_140_CS_AI_WS_VSS
*   Japanese_XJIS_140_CS_AI_KS_VSS
*   Japanese_XJIS_140_CS_AI_KS_WS_VSS
*   Japanese_XJIS_140_CS_AS_VSS
*   Japanese_XJIS_140_CS_AS_WS_VSS
*   Japanese_XJIS_140_CS_AS_KS_VSS
*   Japanese_XJIS_140_CS_AS_KS_WS_VSS
*   Japanese_XJIS_140_CI_AI
*   Japanese_XJIS_140_CI_AI_WS
*   Japanese_XJIS_140_CI_AI_KS
*   Japanese_XJIS_140_CI_AI_KS_WS
*   Japanese_XJIS_140_CI_AS
*   Japanese_XJIS_140_CI_AS_WS
*   Japanese_XJIS_140_CI_AS_KS
*   Japanese_XJIS_140_CI_AS_KS_WS
*   Japanese_XJIS_140_CS_AI
*   Japanese_XJIS_140_CS_AI_WS
*   Japanese_XJIS_140_CS_AI_KS
*   Japanese_XJIS_140_CS_AI_KS_WS
*   Japanese_XJIS_140_CS_AS
*   Japanese_XJIS_140_CS_AS_WS
*   Japanese_XJIS_140_CS_AS_KS
*   Japanese_XJIS_140_CS_AS_KS_WS
*   SQL_EBCDIC1141_CP1_CS_AS
*   SQL_EBCDIC277_2_CP1_CS_AS

## <a name="checking-the-current-collation"></a>Проверка текущих параметров сортировки
Чтобы проверить текущие параметры сортировки для базы данных, можно выполнить следующий фрагмент кода T-SQL: В качестве параметров сортировки выберите DATABASEPROPERTYEX (DB_NAME (), "collation"). Если в качестве параметра свойства передан параметр "collation", функция DatabasePropertyEx возвращает текущие параметры сортировки для указанной базы данных. Дополнительные сведения о функции DatabasePropertyEx см. на сайте MSDN.

