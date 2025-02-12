---
title: Поддерживаемые языки в решении "Функции Azure"
description: Узнайте, какие языки поддерживаются (являются общедоступными), а какие используются в режиме экспериментальной или предварительной версии.
services: functions
documentationcenter: na
author: ggailey777
manager: jeconnoc
ms.service: azure-functions
ms.devlang: dotnet
ms.topic: reference
ms.date: 08/02/2018
ms.author: glenga
ms.openlocfilehash: 8839fc0fb9f19a1cfa95a4191213503dba7602c6
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "65148819"
---
# <a name="supported-languages-in-azure-functions"></a>Поддерживаемые языки в решении "Функции Azure"

В этой статье перечислены уровни поддержки языков, используемых с решением "Функции Azure".

## <a name="levels-of-support"></a>Уровни поддержки

Уровней поддержки три:

* **Общедоступные** — полностью поддерживаемые языки, утвержденные для использования в рабочей среде.
* **Предварительная версия** — языки, которые еще не поддерживаются, но в будущем станут общедоступными.
* **Экспериментальные** — языки, которые не поддерживаются и могут быть удалены в будущем. Нет гарантии, что они будут поддерживаться в качестве общедоступных.

## <a name="languages-in-runtime-1x-and-2x"></a>Языки в средах выполнения версий 1.x и 2.x

Доступны [две версии среды выполнения в решении "Функции Azure"](functions-versions.md). В следующей таблице показаны поддерживаемые языки для каждой версии среды выполнения.

[!INCLUDE [functions-supported-languages](../../includes/functions-supported-languages.md)]

### <a name="experimental-languages"></a>Языки, используемые в режиме экспериментальной версии

Языки, используемые как экспериментальные, в версии 1.x не масштабируются должным образом и поддерживают не все привязки.

Но не используйте экспериментальные функции для важных задач, так как эти функции не поддерживаются официально. Случаи поддержки не следует открывать для проблем, если применяются языки, используемые в качестве экспериментальных. 

В среде выполнения версии 2.x не предоставляется поддержка языков, используемых в качестве экспериментальных. Поддержка новых языков добавляется только в том случае, если язык может поддерживаться в рабочей среде. 

### <a name="language-extensibility"></a>Расширяемость языка

Среда выполнения версии 2.x разработана для обеспечения [расширяемости языка](https://github.com/Azure/azure-webjobs-sdk-script/wiki/Language-Extensibility). Языки JavaScript и Java в среде выполнения 2.x поддерживают эту расширяемость.

## <a name="next-steps"></a>Дальнейшие действия

Дополнительные сведений о том, как в решении "Функции Azure" использовать один из языков, предоставляемых в режиме общедоступной или экспериментальной версии, см. в следующих ресурсах:

> [!div class="nextstepaction"]
> [C#](functions-reference-csharp.md)

> [!div class="nextstepaction"]
> [F#](functions-reference-fsharp.md)

> [!div class="nextstepaction"]
> [JavaScript](functions-reference-node.md)

> [!div class="nextstepaction"]
> [Java](functions-reference-java.md)

> [!div class="nextstepaction"]
> [Python](functions-reference-python.md)
