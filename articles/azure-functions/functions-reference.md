---
title: Руководство по разработке для Функций Azure | Документация Майкрософт
description: Узнайте о концепциях и методах Функций Azure, необходимых для разработки функции в Azure, для всех языков программирования и привязок.
services: functions
documentationcenter: na
author: ggailey777
manager: jeconnoc
keywords: руководство разработчика, Функции Azure, функции, обработка событий, объекты webhook, динамические вычисления, независимая архитектура
ms.assetid: d8efe41a-bef8-4167-ba97-f3e016fcd39e
ms.service: azure-functions
ms.devlang: multiple
ms.topic: reference
ms.date: 10/12/2017
ms.author: glenga
ms.openlocfilehash: 737da27fd8da292b6c1f8997d4bd300bbec8740e
ms.sourcegitcommit: d2785f020e134c3680ca1c8500aa2c0211aa1e24
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/04/2019
ms.locfileid: "67565535"
---
# <a name="azure-functions-developers-guide"></a>Руководство для разработчиков по Функциям Azure
Некоторые из Функций Azure имеют ряд общих технических особенностей и компонентов вне зависимости от используемого языка или привязки. Прежде чем приступать к детальному изучению особенностей, характерных для определенного языка или привязки, ознакомьтесь с этими общими сведениями.

В этой статье предполагается, что вы уже прочли статью [Общие сведения о Функциях Azure](functions-overview.md).

## <a name="function-code"></a>Код функции
Ключевым элементом решения "Функции Azure" является *функция* . Функция состоит из двух важных частей: ваш код, написан на разных языках, и файл конфигурации function.json. Для компилируемых языков этот файл создается автоматически из заметки к вашему коду. Для сценарных языков вы должны предоставить файл конфигурации самостоятельно.

Файл function.json определяет триггер, привязки и другие параметры конфигурации функции. Каждая функция имеет только один триггер. В среде выполнения этот файл используется для определения событий, которые необходимо отслеживать, и способа передачи данных в выполнение функции и возвращения данных из него. Ниже приведен пример файла function.json.

```json
{
    "disabled":false,
    "bindings":[
        // ... bindings here
        {
            "type": "bindingType",
            "direction": "in",
            "name": "myParamName",
            // ... more depending on binding
        }
    ]
}
```

В свойстве `bindings` указываются свойства триггеров и привязок. Каждая привязка имеет ряд общих параметров и некоторые параметры, характерные для данного типа привязки. Для каждой привязки требуются указанные ниже параметры.

| Свойство | Значения и типы | Комментарии |
| --- | --- | --- |
| `type` |строка |Тип привязки. Пример: `queueTrigger`. |
| `direction` |"in", "out" |Указывает, служит ли привязка для получения данных в функции или для отправки их из функции. |
| `name` |string |Имя, используемое для связанных данных в функции. Для C# это имя аргумента, а для JavaScript — ключ в списке ключей и значений. |

## <a name="function-app"></a>Приложение-функция
Приложение-функция предоставляет контекст выполнения в Azure, в котором выполняются функции. Приложение-функция состоит из одной или нескольких независимых функций, которые управляются, развертываются и масштабируются вместе. Для всех функций в приложении-функции действует один ценовой план, непрерывное развертывание и версия среды выполнения. Приложение-функцию можно рассматривать как способ упорядочения функций и совместного управления ими. 

> [!NOTE]
> Все функции в приложении-функции должны использовать один язык. Это не было обязательным в [предыдущих версиях](functions-versions.md) среды выполнения "Функции Azure".

## <a name="folder-structure"></a>Структура папок
[!INCLUDE [functions-folder-structure](../../includes/functions-folder-structure.md)]

Выше приведена структура папки по умолчанию (рекомендуемая) для приложения-функции. Если вы желаете изменить расположение файла кода функции, измените раздел `scriptFile` в файле _function.json_. Мы рекомендуем развертывать проект в приложение-функцию в Azure путем [развертывания пакета](deployment-zip-push.md). Вы также можете использовать имеющиеся средства, такие как [непрерывная интеграция и развертывание](functions-continuous-deployment.md) и Azure DevOps.

> [!NOTE]
> Если вы развертываете пакет вручную, убедитесь, что развертываете файл _host.json_ и папки функций непосредственно в папку `wwwroot`. Не включайте папку `wwwroot` в развертывания. В противном случае вы получите папки `wwwroot\wwwroot`.

#### <a name="use-local-tools-and-publishing"></a>Использование локальных инструментов и публикация
Приложения-функции можно разрабатывать и публиковать с помощью различных средств, включая [Visual Studio](./functions-develop-vs.md), [Visual Studio Code](functions-create-first-function-vs-code.md), [IntelliJ](./functions-create-maven-intellij.md), [Eclipse](./functions-create-maven-eclipse.md) и [Azure Functions Core Tools](./functions-develop-local.md). Дополнительные сведения см. в статье [Как программировать и тестировать Функции Azure в локальной среде](./functions-develop-local.md).

<!--NOTE: I've removed documentation on FTP, because it does not sync triggers on the consumption plan --glenga -->

## <a id="fileupdate"></a> Редактирование функций на портале Azure
Редактор функций на портале Azure позволяет обновлять файл *function.json* и файл кода для функции непосредственно на портале. Рекомендуется использовать его только для небольших изменений или для подтверждения концепции. Наиболее оптимальным способом является использование локального средства разработки, например, VS Code.

## <a name="parallel-execution"></a>Параллельное выполнение
Если однопотоковая среда выполнения функций не успевает обрабатывать активируемые события, она может вызывать функцию несколько раз в параллельном режиме.  Если для приложения-функции используется [план потребления](functions-scale.md#how-the-consumption-and-premium-plans-work), это приложение может автоматически развертываться.  Каждый экземпляр приложения-функции, как при использовании плана потребления, так и при использовании обычного [плана размещения службы приложений](../app-service/overview-hosting-plans.md), может обрабатывать несколько параллельных вызовов функции с помощью нескольких потоков.  Максимальное количество одновременных вызовов функции в каждом экземпляре приложения-функции зависит от типа триггера и ресурсов, используемых функциями в приложении-функции.

## <a name="functions-runtime-versioning"></a>Управление версиями среды выполнения Функций

Версию среды выполнения Функций можно настроить с помощью параметра приложения `FUNCTIONS_EXTENSION_VERSION`. Например, значение "~2" указывает на то, что приложение-функция будет использовать 2.x в качестве основного номера версии. При выпуске приложения-функции обновляются до версии с новым дополнительным номером. Дополнительные сведения, в том числе инструкции по просмотру точной версии приложения-функции см. в статье [Выбор целевых версий среды выполнения Функций Azure](set-runtime-version.md).

## <a name="repositories"></a>Репозитории
Код функций Azure имеет вид открытого исходного кода и хранится в репозиториях GitHub:

* [Функции Azure](https://github.com/Azure/Azure-Functions)
* [Узел функций Azure](https://github.com/Azure/azure-functions-host/)
* [портал функций Azure;](https://github.com/azure/azure-functions-ux)
* [шаблоны функций Azure;](https://github.com/azure/azure-functions-templates)
* [пакет Azure SDK для веб-заданий;](https://github.com/Azure/azure-webjobs-sdk/)
* [расширения пакета Azure SDK для веб-заданий.](https://github.com/Azure/azure-webjobs-sdk-extensions/)

## <a name="bindings"></a>Привязки
В таблице ниже приведены все поддерживаемые привязки.

[!INCLUDE [dynamic compute](../../includes/functions-bindings.md)]

Проблемы с ошибками, поступающими от привязок? См. документацию по [кодам ошибок Функций Azure](functions-bindings-error-pages.md).

## <a name="reporting-issues"></a>Создание отчетов о проблемах
[!INCLUDE [Reporting Issues](../../includes/functions-reporting-issues.md)]

## <a name="next-steps"></a>Дальнейшие действия
Для получения дополнительных сведений см. следующие ресурсы:

* [Azure Functions triggers and bindings (Триггеры и привязки в Функциях Azure)](functions-triggers-bindings.md)
* [Как программировать и тестировать функции Azure в локальной среде](./functions-develop-local.md)
* [Best Practices for Azure Functions](functions-best-practices.md) (Рекомендации по Функциям Azure)
* [Справочник разработчика C# по функциям Azure](functions-dotnet-class-library.md)
* [Справочник разработчика NodeJS по функциям Azure](functions-reference-node.md)
