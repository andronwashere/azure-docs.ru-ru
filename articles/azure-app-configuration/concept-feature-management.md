---
title: Управление службой "Конфигурация приложений Azure" | Документация Майкрософт
description: Общие сведения о том, как Конфигурация приложений Azure позволяет включать и отключать функции приложения по требованию.
services: azure-app-configuration
documentationcenter: ''
author: yegu-ms
manager: maiye
editor: ''
ms.service: azure-app-configuration
ms.devlang: na
ms.topic: overview
ms.workload: tbd
ms.date: 04/19/2019
ms.author: yegu
ms.openlocfilehash: 46f39e87e4e4cf115cbc1fceeabf0dab38fade28
ms.sourcegitcommit: 51a7669c2d12609f54509dbd78a30eeb852009ae
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/30/2019
ms.locfileid: "66393363"
---
# <a name="feature-management-overview"></a>Общие сведения об управлении функциями

Традиционно внедрение новой функции приложения требует полного повторного развертывания приложения. Для тестирования функции вам, скорее всего, потребуется развернуть приложение много раз, чтобы иметь возможность управлять тем, когда эта функция становится видимой и кому.

Управление функциями — современная методика разработки программного обеспечения, которая отделяет выпуск функций от развертывания кода и позволяет быстро вносить изменения в доступность функций по требованию. Она использует методику, называемую *флагами функций* (также известную как *ротация функций*, *коммутатор функций* и т д.), чтобы динамически управлять жизненным циклом функций.

Управление функциями помогает разработчикам справиться со следующими проблемами.

* **Управление ветвью кода**. Флаги функций часто используются для создания оболочки новых функциональных возможностей приложения, которые находятся в стадии разработки. Такая функция по умолчанию скрыта. Вы можете безопасно внедрить функцию, хотя она и не завершена, и она будет находится в неактивном состоянии в рабочей среде. При таком подходе, который называется *темным развертыванием*, вы можете выпускать весь свой код в конце каждого цикла развертывания. Вам больше не нужно поддерживать какую-либо ветвь кода в нескольких циклах из-за того, что выполнение функции занимает более одного цикла.
* **Тестирование в рабочей среде**. Вы можете использовать флаги функций, чтобы предоставлять ранний доступ к новым функциональным возможностям в рабочей среде. Например, вы можете ограничить доступ, предоставив его только участникам команды или внутренним тестировщикам бета-версии. Эти пользователи получат полноценный опыт работы в рабочей среде, а не имитацию или опыт частичного использования в тестовой среде.
* **Фокус-тестирование**. Вы можете также использовать флаги функций для последовательного развертывания новых функциональных возможностей для пользователей. Сначала вы можете нацелиться на небольшой процент пользователей и, когда станете увереннее в реализации, постепенно увеличивать этот процент с течением времени.
* **Мгновенное отключение**. Флаги функций обеспечивают средство подстраховки при выпуске новых функций. С их помощью можно легко включать и отключать функции приложения. При необходимости можно быстро отключить функцию, не выполняя повторную сборку и развертывание приложения.
* **Выборочная активация**. Хотя большинство флагов функций существует только до того момента, когда их связанные функциональные возможности будут выпущены успешно, некоторые могут существовать в течение длительного времени. Вы можете использовать флаги функций для разделения пользователей и предоставления определенного набора функций каждой группе. Например, у вас может быть функция, работающая в определенном веб-браузере. Вы можете определить флаг функции таким образом, чтобы только пользователи этого браузера могли видеть и использовать данную функцию. С помощью этого подхода вы можете легко расширить список поддерживаемых браузеров позднее, не внося изменения в код.

## <a name="basic-concepts"></a>Основные понятия

Ниже приведено несколько новых терминов, связанных с управлением функциями.

* **Флаг функции**. Это переменная с двоичным состоянием *on* (Включен) или *off* (Выключен). С флагом функции также связан блок кода. Состояние флага функции активирует запуск блока кода.
* **Диспетчер функций**. Это пакет приложения, который обрабатывает жизненный цикл флагов всех функций в приложении. Обычно диспетчер функций предоставляет дополнительные возможности, такие как кэширование флагов функций и обновление их состояния.
* **Фильтр**. Это правило для оценки состояния флага функции. Группа пользователей, тип устройства или браузера, геолокация, временное окно — это все примеры того, что может представлять фильтр.

Эффективная реализация управления функциями характеризуется по крайней мере двумя компонентами, которые работают вместе.

* Приложение, которое использует флаги функций.
* Отдельный репозиторий, в котором хранятся флаги функций и их текущее состояние.

Взаимодействие этих компонентов показано в следующих примерах.

## <a name="feature-flag-usage-in-code"></a>Использование флага функции в коде

Базовый шаблон для реализации флагов функций в приложении прост. Вы можете представить флаг функции как логическую переменную состояния, используемую в коде с условным оператором `if`.

```csharp
if (featureFlag) {
    // Run the following code
}
```

В этом случае, если параметру `featureFlag` присвоено значение `True`, вложенный блок кода выполняется, в противном случае — пропускается. Можно задать статическое значение `featureFlag`, как показано в следующем примере кода.

```csharp
bool featureFlag = true;
```

Можно также оценить состояние флага на основе определенных правил.

```csharp
bool featureFlag = isBetaUser();
```

Чуть более сложный шаблон флага функции также включает в себя оператор `else`.

```csharp
if (featureFlag) {
    // This following code will run if the featureFlag value is true
} else {
    // This following code will run if the featureFlag value is false
}
```

Тем не менее можно перепрограммировать это поведение в базовом шаблоне. В руководстве по [использованию флагов функций в приложении ASP.NET Core](./use-feature-flags-dotnet-core.md) показано преимущество стандартизации простого шаблона кода. Например:

```csharp
if (featureFlag) {
    // This following code will run if the featureFlag value is true
}

if (!featureFlag) {
    // This following code will run if the featureFlag value is false
}
```

## <a name="feature-flag-repository"></a>Репозиторий флагов функций

Чтобы эффективно использовать их, необходимо переместить все флаги функций, используемые в приложении, во внешнее хранилище. Это позволит изменять состояние флагов функций без изменения и повторного развертывания самого приложения.

Служба "Конфигурация приложений Azure" должна быть централизованным репозиторием для флагов функций. Вы можете использовать ее, чтобы определить различные типы флагов функций и быстро и легко управлять их состоянием. Затем вы можете легко получить доступ к этим флагам функций из своего приложения с помощью библиотек Конфигурации приложений для различных платформ языков программирования.

В руководстве по [использованию флагов функций в приложении ASP.NET Core](./use-feature-flags-dotnet-core.md) показано, как поставщик Конфигурации приложений для .NET Core и библиотеки управления функциями используются для реализации флагов функций в веб-приложении ASP.NET.

## <a name="next-steps"></a>Дополнительная информация

> [!div class="nextstepaction"]
> [Краткое руководство по добавлению флагов функций в веб-приложение ASP.NET Core](./quickstart-feature-flag-aspnet-core.md)  
