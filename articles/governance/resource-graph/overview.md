---
title: Обзор Azure Resource Graph
description: Узнайте, как служба Azure Resource Graph позволяет выполнять сложные запросы ресурсов в нужном масштабе.
author: DCtheGeek
ms.author: dacoulte
ms.date: 05/06/2019
ms.topic: overview
ms.service: resource-graph
manager: carmonm
ms.openlocfilehash: d78c640f4269c799d3d371e6dd9db477faf96694
ms.sourcegitcommit: 47ce9ac1eb1561810b8e4242c45127f7b4a4aa1a
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/11/2019
ms.locfileid: "67807422"
---
# <a name="overview-of-the-azure-resource-graph-service"></a>Обзор службы Azure Resource Graph

Azure Resource Graph — это служба в Azure, призванная расширить управление ресурсами Azure за счет действенных средств для оценки ресурсов и возможности масштабных запросов к выбранному набору подписок, которые позволят вам эффективно контролировать вашу среду. Такие запросы предоставляют следующие возможности:

- Возможность запрашивать ресурсы, используя сложные фильтры, группирование и сортировку по свойствам ресурсов.
- Возможность итеративно изучать ресурсы в соответствии с требованиями системы управления.
- Возможность оценить эффект от применения политик в обширной облачной среде.
- Возможность [детализировать изменения, внесенные в свойства ресурса](./how-to/get-resource-changes.md) (предварительная версия).

В этой документации мы рассмотрим каждую из этих возможностей подробно.

> [!NOTE]
> Azure Resource Graph обеспечивает работу панели поиска портала Azure, новой возможности обзора "Все ресурсы", а также _отличия визуальных элементов_ [Изменение журнала](../policy/how-to/determine-non-compliance.md#change-history-preview) политики Azure
> . Этот компонент предназначен для клиентов, которым нужно управлять масштабными средами.

[!INCLUDE [service-provider-management-toolkit](../../../includes/azure-lighthouse-supported-service.md)]

## <a name="how-does-resource-graph-complement-azure-resource-manager"></a>Как Azure Resource Graph дополняет Azure Resource Manager

Azure Resource Manager в настоящее время поддерживает запросы основных полей ресурсов, а именно "Имя ресурса", "Идентификатор", "Тип", "Группа ресурсов", "Подписка" и "Расположение". Resource Manager также предоставляет средства вызова отдельных поставщиков ресурсов, позволяя получить подробные свойства для каждого из ресурсов.

Azure Resource Graph дает вам доступ к свойствам, возвращаемым поставщиками ресурсов, без необходимости вызывать каждый поставщик ресурсов отдельно. Чтобы получить список поддерживаемых типов ресурсов, выполните поиск по значению **Да** в таблице [Ресурсы для развертываний в полном режиме](../../azure-resource-manager/complete-mode-deletion.md).

Возможности Azure Resource Graph:

- Доступ к свойствам, возвращаемым поставщиками ресурсов, без необходимости вызывать каждый поставщик ресурсов отдельно.
- Просмотр журнала изменений, внесенных в ресурс за последние 14 дней, чтобы узнать, какие свойства были изменены и когда. (предварительная версия)

## <a name="how-resource-graph-is-kept-current"></a>Обновление Resource Graph

При обновлении ресурса Azure Resource Graph получает от Resource Manager уведомление об изменении.
Затем Resource Graph обновляет свою базу данных. Resource Graph также выполняет обычное _полное сканирование_. Такое сканирование обеспечивает обновление данных Resource Graph при наличии пропущенных уведомлений или при обновлении ресурса за пределами Resource Manager.

## <a name="the-query-language"></a>Язык запросов

Теперь, когда вы разобрались, что такое Azure Resource Graph, рассмотрим порядок создания запросов.

Важно понимать, что язык запросов Azure Resource Graph похож на [язык запросов Kusto](../../data-explorer/data-explorer-overview.md), используемый в Azure Data Explorer.

Сведения об операциях и функциях, которые можно использовать с Azure Resource Graph, см. в статье [Язык запросов Resource Graph](./concepts/query-language.md).
Информацию о ресурсах см. в статье о [просмотре ресурсов](./concepts/explore-resources.md).

## <a name="permissions-in-azure-resource-graph"></a>Разрешения в Azure Resource Graph

Чтобы использовать Resource Graph, необходимо иметь соответствующие права для [управления доступом на основе ролей](../../role-based-access-control/overview.md) (RBAC) как минимум с доступом на чтение ресурсов, которые вы планируете запрашивать. Без минимального разрешения `read` для объекта или группы объектов Azure результаты не будут возвращаться.

> [!NOTE]
> Resource Graph использует подписки, доступные субъекту на момент входа. Чтобы просмотреть ресурсы новой подписки, добавленные во время активного сеанса, субъекту необходимо обновить контекст. Это происходит автоматически при выходе и повторном входе.

## <a name="throttling"></a>Регулирование

Так как Resource Graph — бесплатная служба, запросы можно регулировать, чтобы обеспечить лучшие возможности и время отклика для всех клиентов. Если организации нужно использовать API Resource Graph для крупномасштабных и частых запросов, оставьте отзыв в соответствующем разделе на [странице портала Resource Graph](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyMenuBlade/ResourceGraph).
Предоставьте экономическое обоснование и установите флажок "Майкрософт может связаться с вами по электронной почте по поводу вашего отзыва", чтобы наша команда могла связаться с вами.

Resource Graph регулирует запросы на уровне пользователя. Ответ службы содержит следующие заголовки HTTP:

- `x-ms-user-quota-remaining` (целое число): оставшаяся квота ресурсов для пользователя. Это значение соответствует количеству запросов.
- `x-ms-user-quota-resets-after` (чч:мм:сс): время, оставшееся до сброса квоты потребления пользователя.

Дополнительные сведения см. в разделе [рекомендаций для регулируемых запросов](./concepts/guidance-for-throttled-requests.md).

## <a name="running-your-first-query"></a>Выполнение первого запроса

Resource Graph поддерживает Azure CLI, Azure PowerShell и пакет SDK Azure для .NET. Запрос имеет одинаковую структуру для всех языков. Узнайте, как включить Resource Graph в [Azure CLI](first-query-azurecli.md#add-the-resource-graph-extension) и [Azure PowerShell](first-query-powershell.md#add-the-resource-graph-module).

## <a name="next-steps"></a>Дополнительная информация

- Выполните первый запрос с помощью [Azure CLI](first-query-azurecli.md).
- Выполните первый запрос с помощью [Azure PowerShell](first-query-powershell.md).
- Начните с [начальных запросов](./samples/starter.md).
- Перейдите к [расширенным запросам](./samples/advanced.md).