---
title: Шаблоны столбцов сопоставления Потока данных Фабрики данных Azure
description: Создание обобщенных шаблонов преобразования данных с помощью шаблонов столбцов фабрики данных Azure в сопоставлении потоков данных
author: kromerm
ms.author: makromer
ms.service: data-factory
ms.topic: conceptual
ms.date: 01/30/2019
ms.openlocfilehash: 076c3318a68a50e6bd1b4f9f2a4a4b9a034533c6
ms.sourcegitcommit: e72073911f7635cdae6b75066b0a88ce00b9053b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/19/2019
ms.locfileid: "68346572"
---
# <a name="azure-data-factory-mapping-data-flows-column-patterns"></a>Шаблоны столбцов потоков данных сопоставления фабрики данных Azure

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

Несколько преобразований Потока данных Фабрики данных Azure поддерживают идею "Шаблоны столбцов", так что вы можете создавать столбцы на основе шаблонов вместо жестко закодированных имен столбцов. Эту функцию можно использовать в построителе выражений для определения шаблонов, соответствующих столбцам для преобразования, а не для указания точных, конкретных имен полей. Шаблоны полезны, если входящие поля источника изменяются часто, особенно в случае изменения столбцов в текстовых файлах или базах данных NoSQL. Это условие иногда называют «смещением схемы».

![шаблоны столбцов](media/data-flow/columnpattern2.png "Шаблоны столбцов")

Шаблоны столбцов можно использовать для обработки как сценариев смещения схемы, так и общих сценариев. Этот вариант подходит для условий, когда вы не можете полностью знать имя каждого столбца. Вы можете сопоставить шаблон по имени столбца и типу данных столбца и выполнить сборку выражения для преобразования, которое будет выполнять эту операцию в любом поле потока данных, которое соответствует вашим шаблонам `name` & `type`.

При добавлении выражения в преобразование, которое принимают шаблоны, выберите "Add Column Pattern" (Добавить шаблон столбца). Шаблоны столбцов позволяют использовать шаблоны сопоставления столбца смещения схемы.

При создании шаблона шаблонов столбцов используйте `$$` в выражении для представления ссылки на каждое сопоставленное поле из потока входных данных.

Если вы решите использовать одну из функций регулярного выражения построителя выражений, вы можете впоследствии использовать $1, $2, $3... для ссылки на подшаблоны, соответствующие вашему выражению регулярного выражения.

Примером сценария шаблона столбца является использование SUM с серией входящих полей. Статистические вычисления SUM находятся в статистическом преобразовании. Затем можно использовать SUM при каждом совпадении типов полей, совпадающих с "integer", а затем использовать $ $ для ссылки на каждое совпадение в выражении.

## <a name="match-columns"></a>Сопоставление столбцов
![типы шаблонов столбцов](media/data-flow/pattern2.png "Типы шаблонов")

Для построения шаблонов, основанных на столбцах, можно сопоставить имя столбца, тип, поток или расположение и использовать любое сочетание функций выражений и регулярных выражений.

![Расположение столбца](media/data-flow/position.png "Расположение столбца")

## <a name="next-steps"></a>Следующие шаги
Дополнительные сведения о [языке выражений](https://aka.ms/dataflowexpressions) потока данных сопоставления ADF для преобразований данных
