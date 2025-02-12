---
title: Работа с библиотекой обработчика для канала изменений в Azure Cosmos DB
description: Использование библиотеки обработчика для канала изменений Azure Cosmos DB.
author: rimman
ms.service: cosmos-db
ms.devlang: dotnet
ms.topic: conceptual
ms.date: 07/23/2019
ms.author: rimman
ms.reviewer: sngun
ms.openlocfilehash: e5142e9b4e7c2c79fd2b7e41123db4422334b730
ms.sourcegitcommit: c72ddb56b5657b2adeb3c4608c3d4c56e3421f2c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/24/2019
ms.locfileid: "68467794"
---
# <a name="change-feed-processor-in-azure-cosmos-db"></a>Обработчик канала изменений в Azure Cosmos DB 

Обработчик веб-канала изменений входит в состав [Azure Cosmos DB пакета SDK v3](https://github.com/Azure/azure-cosmos-dotnet-v3). Он упрощает процесс чтения веб-канала изменений и эффективно распределяет обработку событий между несколькими потребителями.

Основным преимуществом библиотеки обработчика веб-канала изменений является отказоустойчивое поведение, которое гарантирует доставку всех событий в веб-канале изменений "не менее одного раза".

## <a name="components-of-the-change-feed-processor"></a>Компоненты обработчика веб-канала изменений

Существует четыре основных компонента реализации обработчика веб-канала изменений: 

1. **Отслеживаемый контейнер**. Контейнер с данными, из которых формируется канал изменений. Все операции вставки и обновления отслеживаемого контейнера отражаются в веб-канале изменений контейнера.

1. **Контейнер аренд**. Контейнер аренды выступает в качестве хранилища состояний и координирует обработку веб-канала изменений для нескольких рабочих ролей. Контейнер аренды может храниться в той же учетной записи, что и отслеживаемый контейнер, или в отдельной учетной записи. 

1. **Узел:** Узел — это экземпляр приложения, который использует обработчик веб-канала изменений для прослушивания изменений. Несколько экземпляров с одной и той же конфигурацией аренды могут выполняться параллельно, но каждый экземпляр должен иметь другое **имя экземпляра**. 

1. **Делегат:** Делегат — это код, определяющий, что вы, разработчик должен выполнять с каждым пакетом изменений, считанных обработчиком веб-канала изменений. 

Чтобы лучше разобраться, как взаимодействуют эти четыре элемента обработчика для канала изменений, давайте рассмотрим пример, приведенный на схеме ниже. Отслеживаемый контейнер хранит документы и использует "City" в качестве ключа секции. Мы видим, что значения ключа секции распределяются по диапазонам, содержащим элементы. Существует два экземпляра узла, и обработчик веб-канала изменений назначает каждому экземпляру разные диапазоны значений ключа секции для максимального распределения вычислительных ресурсов. Каждый диапазон считывается параллельно, и его ход выполнения сохраняется отдельно от других диапазонов в контейнере аренды.

![Пример обработчика канала изменений](./media/change-feed-processor/changefeedprocessor.png)

## <a name="implementing-the-change-feed-processor"></a>Реализация обработчика веб-канала изменений

Точка входа всегда является отслеживаемым контейнером из `Container` экземпляра, который вы вызываете: `GetChangeFeedProcessorBuilder`

[!code-csharp[Main](~/samples-cosmosdb-dotnet-change-feed-processor/src/Program.cs?name=DefineProcessor)]

Где первый параметр представляет собой уникальное имя, описывающее цель этого процессора, а второе имя — это реализация делегата, которая будет выполнять обработку изменений. 

Пример делегата:

[!code-csharp[Main](~/samples-cosmosdb-dotnet-change-feed-processor/src/Program.cs?name=Delegate)]

Наконец, определите имя для этого экземпляра процессора с помощью `WithInstanceName` , а это контейнер для поддержания `WithLeaseContainer`состояния аренды.

Вызов `Build` предоставит экземпляр процессора, который можно запустить, вызвав `StartAsync`.

## <a name="processing-life-cycle"></a>Жизненный цикл обработки

Нормальный жизненный цикл экземпляра узла:

1. Чтение веб-канала изменений.
1. Если изменения отсутствуют, спящий режим для предопределенного времени (настраиваемый с помощью `WithPollInterval` в построителе) и переходит к #1.
1. При наличии изменений отправьте их в **делегат**.
1. Когда делегат завершит **обработку изменений,** обновите хранилище аренды с последней обработанной точкой во времени и перейдите к #1.

## <a name="error-handling"></a>Обработка ошибок

Обработчик веб-канала изменений устойчив к ошибкам пользовательского кода. Это означает, что если в реализации делегата имеется необработанное исключение (шаг #4), то обработка потока, в которой определен пакет изменений, будет остановлена и будет создан новый поток. Новый поток будет проверять, что был последним моментом в хранилище аренды для этого диапазона значений ключей секций, и перезапускаться из него, фактически отправляя тот же пакет изменений делегату. Это поведение будет продолжаться до тех пор, пока делегат не будет правильно обрабатывать изменения, и это является причиной того, что обработчик веб-канала изменений имеет гарантию "хотя бы один раз", так как при возникновении исключения код делегата будет пытаться повторить этот пакет.

## <a name="dynamic-scaling"></a>Динамическое масштабирование

Как упоминалось во время введения, обработчик веб-канала изменений может автоматически распределять вычисление между несколькими экземплярами. Вы можете развернуть несколько экземпляров приложения с помощью обработчика веб-канала изменений и воспользоваться его преимуществами. единственными основными требованиями являются:

1. Все экземпляры должны иметь одинаковую конфигурацию контейнеров аренды.
1. Все экземпляры должны иметь одно и то же имя рабочего процесса.
1. Каждый экземпляр должен иметь другое имя экземпляра (`WithInstanceName`).

Если эти три условия применяются, то обработчик веб-канала изменений будет использовать алгоритм равномерного распределения и распространит все аренды в контейнере аренды во всех работающих экземплярах и параллельных вычислений. Одна Аренда может принадлежать только одному экземпляру в определенный момент времени, поэтому максимальное число экземпляров равно числу аренд.

Экземпляры могут увеличиваться и уменьшаться, а обработчик веб-канала изменений динамически корректирует нагрузку, перегружая соответствующим образом.

## <a name="change-feed-and-provisioned-throughput"></a>Канал изменений и подготовленная пропускная способность

Вы оплачиваете единицы запросов, так как при перемещении данных в контейнеры Cosmos и из них всегда используются единицы запросов. Плата выставляется за число потребляемых ЕЗ контейнером аренды.

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Пакет SDK для Azure Cosmos DB](sql-api-sdk-dotnet.md)
* [Дополнительные примеры на GitHub](https://github.com/Azure-Samples/cosmos-dotnet-change-feed-processor)

## <a name="next-steps"></a>Следующие шаги

Вы можете продолжить знакомство с каналом изменений, перейдя к следующим статьям:

* [Работа с поддержкой веб-канала изменений в Azure Cosmos DB](change-feed.md)
* [Чтение канала изменений Azure Cosmos DB](read-change-feed.md)
* [Как использовать канал изменений Azure Cosmos DB с Функциями Azure](change-feed-functions.md)
