---
title: Работа с большими наборами данных
description: Узнайте, как получать большие наборы данных и управлять ими в Azure Resource Graph.
author: DCtheGeek
ms.author: dacoulte
ms.date: 04/01/2019
ms.topic: conceptual
ms.service: resource-graph
manager: carmonm
ms.openlocfilehash: d04f46dbc60a7242e44d76915e15281cc6248d20
ms.sourcegitcommit: 1572b615c8f863be4986c23ea2ff7642b02bc605
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/10/2019
ms.locfileid: "67786531"
---
# <a name="working-with-large-azure-resource-data-sets"></a>Работа с большими наборами данных ресурса Azure

Azure Resource Graph предназначен для работы и получения сведений о ресурсах в вашей среде Azure. Resource Graph дает возможность получать эти данные быстро, даже при запросе тысяч записей. Resource Graph имеет несколько параметров для работы с этими большими наборами данных.

Рекомендации по работе с запросами с высокой частотой, см. в разделе [рекомендации для отрегулированных запросов](./guidance-for-throttled-requests.md).

## <a name="data-set-result-size"></a>Размер результата набора данных

По умолчанию Resource Graph ограничивает любой запрос возвращением до **100** записей. Этот элемент управления защищает как пользователя, так и службу от непреднамеренных запросов, которые могут привести к большим наборам данных. Это событие чаще всего происходит, когда клиент экспериментирует с запросами, чтобы найти и отфильтровать ресурсы в соответствии с их конкретными потребностями. Этот элемент управления отличается от использования операторов языка Azure Data Explorer [top](/azure/kusto/query/topoperator) или [limit](/azure/kusto/query/limitoperator) для ограничения результатов.

> [!NOTE]
> При использовании **первый**, рекомендуется упорядочить результаты по крайней мере один столбец с `asc` или `desc`. Без сортировки, результаты возвращаются, случайные и не воспроизводимым.

Ограничение по умолчанию может быть отменено всеми методами взаимодействия с Resource Graph. Следующие примеры демонстрируют изменение ограничения размера набора данных на _200_.

```azurecli-interactive
az graph query -q "project name | order by name asc" --first 200 --output table
```

```azurepowershell-interactive
Search-AzGraph -Query "project name | order by name asc" -First 200
```

В [REST API](/rest/api/azureresourcegraph/resources/resources) элемент управления является **$top** и частью **QueryRequestOptions**.

Элемент управления, который является _наиболее строгим_, победит. Например, если ваш запрос использует операторы **top** или **limit** и приводит к большему количеству записей, чем **First**, максимальное количество возвращаемых записей будет равно **First**. Аналогичным образом, если **top** или **limit** меньше, чем **First**, возвращенный набор записей будет меньшим значением, настроенным с помощью **top** или **limit**.

**First** сейчас имеет максимальное значение _5000_.

## <a name="skipping-records"></a>Пропуск записей

Следующий параметр для работы с большими наборами данных — элемент управления **Skip**. Этот элемент управления позволяет пропустить или перейти определенное число записей перед возвратом результатов. Параметр **Skip** полезен для запросов, которые сортируют результаты удобным образом, где намерением является получение записей в середине результирующего набора. Если необходимые результаты находятся в конце возвращенного набора данных, более эффективно использовать другую конфигурацию сортировки и извлекать результаты из верхней части набора данных.

> [!NOTE]
> При использовании **Skip**, рекомендуется упорядочить результаты по крайней мере один столбец с `asc` или `desc`. Без сортировки, результаты возвращаются, случайные и не воспроизводимым.

В следующих примерах показано, как пропустить первые _10_ записей, в результате которых будет получен запрос, вместо этого начиная возвращаемый набор результатов с 11-й записи.

```azurecli-interactive
az graph query -q "project name | order by name asc" --skip 10 --output table
```

```azurepowershell-interactive
Search-AzGraph -Query "project name | order by name asc" -Skip 10
```

В [REST API](/rest/api/azureresourcegraph/resources/resources) элемент управления является **$skip** и частью **QueryRequestOptions**.

## <a name="paging-results"></a>Разбиение результатов по страницам

Если это необходимо разбить результирующий набор на меньшие наборы записей для обработки или так как результирующий набор превышает максимально допустимое значение _1000_ вернул записей, использовать разбиение по страницам. [REST API](/rest/api/azureresourcegraph/resources/resources) **QueryResponse** предоставляет значения, указывающие, что набор результатов разбит на **resultTruncated** и **$skipToken**.
**resultTruncated** является логическим значением, которое оповещает потребителя, если существуют дополнительные записи, которые не возвращаются в ответе. Это условие также можно определить, когда свойство **count** меньше, чем свойство **totalRecords**. **totalRecords** определяет, сколько записей соответствует запросу.

Когда свойство **resultTruncated** равно **true**, свойство **$skipToken** задается в ответе. Это значение используется с теми же значениями запроса и подписки для получения следующего набора записей, который соответствует запросу.

В следующих примерах показывается как **пропустить** первой записи 3000 и возврат **первый** 1000 записей после фильтров, пропущенных с помощью интерфейса командной строки Azure и Azure PowerShell:

```azurecli-interactive
az graph query -q "project id, name | order by id asc" --first 1000 --skip 3000
```

```azurepowershell-interactive
Search-AzGraph -Query "project id, name | order by id asc" -First 1000 -Skip 3000
```

> [!IMPORTANT]
> Запрос должен **проецировать** поле **id**, чтобы сработала нумерация страниц. Если он отсутствует из запроса, ответ не будет содержать **$skipToken**.

Пример см. в разделе [Запрос следующей страницы](/rest/api/azureresourcegraph/resources/resources#next-page-query) в документации REST API.

## <a name="next-steps"></a>Следующие шаги

- См. в разделе язык используется в [запросы Starter](../samples/starter.md).
- См. Дополнительные использует в [расширенных запросов](../samples/advanced.md).
- Узнайте, как [ознакомьтесь с ресурсами](explore-resources.md).