---
title: Автор политики для свойства массива для ресурсов Azure
description: Сведения для создания параметров массива, создать правила для массива выражения языка, оценить псевдоним [*], а также для добавления элементов в существующий массив правилам определения политики Azure.
author: DCtheGeek
ms.author: dacoulte
ms.date: 03/06/2019
ms.topic: conceptual
ms.service: azure-policy
manager: carmonm
ms.openlocfilehash: 479f77791a0b035f2d1de6085dfb12f5196288ee
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "65979333"
---
# <a name="author-policies-for-array-properties-on-azure-resources"></a>Автор политики для свойства массива для ресурсов Azure

Свойства Azure Resource Manager обычно определяются как строки и логические значения. Если существует отношение один ко многим, сложных свойств вместо этого определяются как массивы. В политике Azure массивы используются несколькими различными способами:

- Тип [параметр определения](../concepts/definition-structure.md#parameters), которые обеспечивают несколько вариантов
- Часть [правило политики](../concepts/definition-structure.md#policy-rule) используя условия **в** или **notIn**
- Часть правила политики, результатом является [ \[ \* \] псевдоним](../concepts/definition-structure.md#understanding-the--alias) для вычисления конкретных сценариев, таких как **None**, **любой**, или  **Все**
- В [добавить эффект](../concepts/effects.md#append) заменить или добавить в существующий массив

Этой статье рассматривается каждый используется "Политика Azure" и предоставляет несколько примеры определений.

## <a name="parameter-arrays"></a>Массивы параметров

### <a name="define-a-parameter-array"></a>Определите массивом параметров

Определение параметра в виде массива обеспечивает гибкость политики, когда требуется более одного значения.
Это определение политики разрешает любой единое расположение для параметра **allowedLocations** и, по умолчанию _eastus2_:

```json
"parameters": {
    "allowedLocations": {
        "type": "string",
        "metadata": {
            "description": "The list of allowed locations for resources.",
            "displayName": "Allowed locations",
            "strongType": "location"
        },
        "defaultValue": "eastus2"
    }
}
```

Как **тип** был _строка_только одно значение можно задать при назначении политики. Если эта политика назначается, ресурсов в области, допустимы только в одном регионе Azure. Большинство определения политик должны разрешать список утвержденных параметров, например, позволяет _eastus2_, _eastus_, и _westus2_.

Чтобы создать определение политики, чтобы разрешить несколько параметров, используйте _массива_ **тип**. Ту же политику можно переписать следующим образом:

```json
"parameters": {
    "allowedLocations": {
        "type": "array",
        "metadata": {
            "description": "The list of allowed locations for resources.",
            "displayName": "Allowed locations",
            "strongType": "location"
        },
        "defaultValue": "eastus2",
        "allowedValues": [
            "eastus2",
            "eastus",
            "westus2"
        ]

    }
}
```

> [!NOTE]
> После сохранения определения политики **тип** свойство параметра нельзя изменить.

Это новое определение параметр принимает несколько значений во время назначения политики. Со свойством массива **allowedValues** определены, доступные во время назначения значения — дальнейшие ограничен предопределенный список вариантов. Использование **allowedValues** является необязательным.

### <a name="pass-values-to-a-parameter-array-during-assignment"></a>Передавать значения в массив параметров во время назначения

При назначении политики на портале Azure, параметр **тип** _массива_ отображается в виде единого текстового поля. Подсказка говорит: «использовать; для разделения значений. (например, Лондон; New York)». Для передачи значений недопустима _eastus2_, _eastus_, и _westus2_ в параметр, используйте следующую строку:

`eastus2;eastus;westus2`

Формат для значения параметра отличается в том случае, при использовании Azure CLI, Azure PowerShell или REST API. Значения передаются через строку JSON, который также включает в себя имя параметра.

```json
{
    "allowedLocations": {
        "value": [
            "eastus2",
            "eastus",
            "westus2"
        ]
    }
}
```

Чтобы использовать эту строку с каждый пакет SDK, используйте следующие команды:

- Azure CLI: Команда [Создание назначения политики az](/cli/azure/policy/assignment?view=azure-cli-latest#az-policy-assignment-create) с параметром **params**
- Azure PowerShell Командлет [New AzPolicyAssignment](/powershell/module/az.resources/New-Azpolicyassignment) с параметром **PolicyParameter**
- REST API: В _ПОМЕСТИТЬ_ [создать](/rest/api/resources/policyassignments/create) операцию как часть текста запроса, так как значение **properties.parameters** свойство

## <a name="policy-rules-and-arrays"></a>Правила политики и массивы

### <a name="array-conditions"></a>Массив условий

Правило политики [условия](../concepts/definition-structure.md#conditions) , _массива_
**тип** параметра может использоваться с ограничена `in` и `notIn`. Использовать следующее определение политики с условием `equals` в качестве примера:

```json
{
  "policyRule": {
    "if": {
      "not": {
        "field": "location",
        "equals": "[parameters('allowedLocations')]"
      }
    },
    "then": {
      "effect": "audit"
    }
  },
  "parameters": {
    "allowedLocations": {
      "type": "Array",
      "metadata": {
        "description": "The list of allowed locations for resources.",
        "displayName": "Allowed locations",
        "strongType": "location"
      }
    }
  }
}
```

При попытке создать это определение политики с помощью портала Azure интересов об ошибке, например, это сообщение об ошибке:

- «Политика «{GUID}» не удалось параметризовать из-за ошибки проверки. Проверьте, если параметры политики определены правильно. Внутреннее исключение «оценки, результат выражения языка «[parameters('allowedLocations')]» имеет тип «Массив», ожидалось что относится к типу «Строка»».»

Ожидаемый **тип** условия `equals` — _строка_. Так как **allowedLocations** определяется как **тип** _массива_, обработчик политик оценивает выражение языка и вызывает ошибку. С помощью `in` и `notIn` условие, ожидает, что модуль политики **тип** _массива_ в выражении языка. Чтобы устранить это сообщение об ошибке, измените `equals` либо `in` или `notIn`.

### <a name="evaluating-the--alias"></a>Оценка псевдоним [*]

Псевдонимы, которые имеют **[\*]** подключен к имени указать **тип** — _массива_. Не нужно оценивать значение весь массив, вместо **[\*]** дает возможность оценить каждый элемент массива. Существует три сценария, для вычисления элемента это полезно в: None, любой и все.

Триггеры ядра политики **эффект** в **затем** только тогда, когда **Если** правило оценивается как истинное.
Это важно для понимания в контексте способ **[\*]** оценивает каждый отдельный элемент массива.

Пример правила политики для таблицы сценарий ниже:

```json
"policyRule": {
    "if": {
        "allOf": [
            {
                "field": "Microsoft.Storage/storageAccounts/networkAcls.ipRules",
                "exists": "true"
            },
            <-- Condition (see table below) -->
        ]
    },
    "then": {
        "effect": "[parameters('effectType')]"
    }
}
```

**IpRules** массива для таблицы ниже сценария выглядит следующим образом:

```json
"ipRules": [
    {
        "value": "127.0.0.1",
        "action": "Allow"
    },
    {
        "value": "192.168.1.1",
        "action": "Allow"
    }
]
```

Для каждого условия примера ниже замените `<field>` с `"field": "Microsoft.Storage/storageAccounts/networkAcls.ipRules[*].value"`.

Следующие результаты являются результатом сочетания условие правила политики в примере и массива из существующих значений выше:

|Условие |Результат |Объяснение |
|-|-|-|
|`{<field>,"notEquals":"127.0.0.1"}` |Nothing |Один элемент массива результатом является значение false (127.0.0.1! = 127.0.0.1) и один — как true (127.0.0.1! = 192.168.1.1), поэтому **notEquals** условие _false_ и эффект не срабатывает. |
|`{<field>,"notEquals":"10.0.4.1"}` |Действия политики |Оба элемента массива, интерпретируются как true (10.0.4.1! = 127.0.0.1 и 10.0.4.1! = 192.168.1.1), поэтому **notEquals** условие _true_ и активируется эффект. |
|`"not":{<field>,"Equals":"127.0.0.1"}` |Действия политики |Один элемент массива оценивается как истинное (127.0.0.1 == 127.0.0.1) и один — как false (127.0.0.1 == 192.168.1.1), поэтому **равно** условие _false_. Логический оператор оценивается как истинное (**не** _false_), поэтому влияние активируется. |
|`"not":{<field>,"Equals":"10.0.4.1"}` |Действия политики |Оба элемента массива возвращаться false (10.0.4.1 == 127.0.0.1 и 10.0.4.1 == 192.168.1.1), поэтому **равно** условие _false_. Логический оператор оценивается как истинное (**не** _false_), поэтому влияние активируется. |
|`"not":{<field>,"notEquals":"127.0.0.1" }` |Действия политики |Один элемент массива результатом является значение false (127.0.0.1! = 127.0.0.1) и один — как true (127.0.0.1! = 192.168.1.1), поэтому **notEquals** условие _false_. Логический оператор оценивается как истинное (**не** _false_), поэтому влияние активируется. |
|`"not":{<field>,"notEquals":"10.0.4.1"}` |Nothing |Оба элемента массива, интерпретируются как true (10.0.4.1! = 127.0.0.1 и 10.0.4.1! = 192.168.1.1), поэтому **notEquals** условие _true_. Логический оператор, результатом является значение false (**не** _true_), поэтому результат не срабатывает. |
|`{<field>,"Equals":"127.0.0.1"}` |Nothing |Один элемент массива оценивается как истинное (127.0.0.1 == 127.0.0.1) и один — как false (127.0.0.1 == 192.168.1.1), поэтому **равно** условие _false_ и эффект не срабатывает. |
|`{<field>,"Equals":"10.0.4.1"}` |Nothing |Оба элемента массива возвращаться false (10.0.4.1 == 127.0.0.1 и 10.0.4.1 == 192.168.1.1), поэтому **равно** условие _false_ и эффект не срабатывает. |

## <a name="the-append-effect-and-arrays"></a>Добавление эффекта и массивы

[Добавить эффект](../concepts/effects.md#append) ведет себя по-разному в зависимости от того **details.field** — **[\*]** псевдоним или нет.

- Если не **[\*]** псевдонима добавьте заменяет весь массив с **значение** свойство
- Когда **[\*]** псевдонима добавьте добавляет **значение** к существующему полю массива или создает новый массив

Дополнительные сведения см. в разделе [добавить примеры](../concepts/effects.md#append-examples).

## <a name="next-steps"></a>Дальнейшие действия

- Просмотрите примеры доступны на [примеры политик Azure](../samples/index.md).
- Изучите статью о [структуре определения Политики Azure](../concepts/definition-structure.md).
- Изучите [сведения о действии политик](../concepts/effects.md).
- Понять, как [программное создание политик](programmatically-create.md).
- Узнайте, как [исправлять несоответствующие ресурсы](remediate-resources.md).
- Дополнительные сведения о группе управления см. в статье [Упорядочивание ресурсов с помощью групп управления Azure](../../management-groups/overview.md).