---
title: Оптимизация затрат на хранение в Azure Cosmos DB
description: В этой статье описывается управление затратами на хранение данных в Azure Cosmos DB
author: rimman
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/21/2019
ms.author: rimman
ms.openlocfilehash: 71f1f8896126728277ba6f0bf2c0ded1b2a608b7
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "65967260"
---
# <a name="optimize-storage-cost-in-azure-cosmos-db"></a>Оптимизация затрат на хранение в Azure Cosmos DB

В Azure Cosmos DB нет ограничений на объем хранилища и пропускную способность. В отличие от пропускной способности, которую нужно подготавливать и настраивать для каждого контейнера или базы данных Azure Cosmos, оплата за хранилище начисляется в зависимости от фактического потребления. Счета выставляются только за используемое логическое хранилище, и вам не нужно заранее резервировать объем. Хранилище автоматически масштабируется в зависимости от объема данных, которые вы добавляете в контейнер Azure Cosmos DB или удаляете из него.

## <a name="storage-cost"></a>Стоимость хранения

Плата за хранилище начисляется с точностью до гигабайта. Локальное хранилище на основе твердотельных накопителей используется для хранения данных и индексирования. Общий используемый объем хранилища равен объему всех данных и индексов по всем регионам, где вы используете Azure Cosmos DB. Если в учетной записи Azure Cosmos настроена глобальная репликация по трем регионам, вы будете оплачивать суммарный объем используемого хранилища во всех трех регионах. Чтобы оценить требования к хранилищу, воспользуйтесь [планировщиком ресурсов](https://www.documentdb.com/capacityplanner). За хранение данных в Azure Cosmos DB взимается плата 0,25 долл. США за ГБ в месяц. Актуальные данные о ценах см. [на этой странице](https://azure.microsoft.com/pricing/details/cosmos-db/). Вы можете настроить оповещения для оценки объема хранилища, используемого контейнером Azure Cosmos. Сведения о мониторинге хранилища см. в статье [Мониторинг Azure Cosmos DB](monitor-accounts.md).

## <a name="optimize-cost-with-item-size"></a>Оптимизация затрат с помощью изменения размера элементов

Для оптимальной производительности Azure Cosmos DB и снижения затрат желательно, чтобы размер элементов не превышал 2 МБ. Если вам требуется хранить элементы данных крупнее 2 МБ, попробуйте преобразовать схему этих элементов. В тех редких случаях, когда схему изменить невозможно, разделите такие элементы на элементы меньшего размера и создайте между ними логическую связь по общему идентификатору. Все функции Azure Cosmos DB будут стабильно работать благодаря привязке к такому логическому идентификатору.

## <a name="optimize-cost-with-indexing"></a>Оптимизация затрат с помощью индексирования

По умолчанию все данные автоматически индексируются, что может увеличить общий объем используемого хранилища. Но вы можете сократить эти расходы, применив пользовательскую политику индексирования. Автоматическое индексирование, не настроенное с помощью политики, потребляет около 10–20 % от размера элементов. Удаляя или настраивая политики индексирования, вы можете избежать дополнительных затрат на операции записи и дополнительную пропускную способность. В статье [Индексирование в Azure Cosmos DB](indexing-policies.md) есть информация о настройке пользовательских политик индексирования. Те, кто уже работал с реляционными базами данных, могут ожидать по меньшей мере удвоения требований к хранилищу при использовании политики "индексировать все". Но в среднем случае для Azure Cosmos DB это увеличение существенно ниже. Затраты на индексирование в Azure Cosmos DB обычно довольно низки (10–20 %) даже при полном автоматическом индексировании, так как база данных специально проектировалась для снижения объема данных в хранилище. Управляя политикой индексирования, вы можете еще более точно контролировать соотношение между объемом индекса и производительностью запросов.

## <a name="optimize-cost-with-time-to-live-and-change-feed"></a>Оптимизация затрат с помощью настройки времени жизни и канала изменений

Если данные больше не нужны, их можно корректно удалить из учетной записи Azure Cosmos, настроив [срок жизни](time-to-live.md) и [канал изменений](change-feed.md). Также можно перенести устаревшие данные в другое хранилище данных, например хранилище больших двоичных объектов Azure или хранилище данных Azure. В Azure Cosmos DB можно задать срок жизни, чтобы элементы автоматически удалялись из контейнера по прошествии определенного времени. По умолчанию можно задать срок жизни на уровне контейнера и переопределить значения по элементу. После установки срока жизни на уровне контейнера или элемента Azure Cosmos DB будет автоматически удалять соответствующие элементы по прошествии указанного времени с момента их последнего изменения. С помощью канала изменений можно перенести данные в любой другой контейнер в Azure Cosmos DB или во внешнее хранилище данных. При такой миграции не возникает простой, а после ее завершения вы можете удалить исходный контейнер Azure Cosmos или настроить срок жизни для его автоматического удаления.

## <a name="optimize-cost-with-rich-media-data-types"></a>Оптимизация затрат с помощью мультимедийных типов данных 

Если вы намерены хранить мультимедийные данные, например видео, изображения и т. п., Azure Cosmos DB предлагает для этого несколько вариантов. Один из них предполагает хранение мультимедийных типов данных в виде элементов Azure Cosmos. Для каждого такого элемента действует ограничение по размеру в 2 МБ, но это ограничение можно обойти разбиением элемента данных на цепочку вложенных элементов. Вы также можете сохранить эти данные в хранилище BLOB-объектов Azure и ссылаться на них из элементов Azure Cosmos с помощью метаданных. Такой подход имеет ряд преимуществ и недостатков. Первый подход обеспечивает оптимальную производительность с точки зрения задержки и пропускной способности в соответствии с соглашением об уровне обслуживания, а также полную готовность к глобальному распространению мультимедийных типов данных в дополнение к обычным элементам Azure Cosmos. Однако такая поддержка означает дополнительные расходы. Сохраняя мультимедиа в хранилище BLOB-объектов Azure, вы сможете снизить общие расходы. Если задержка для вас критически важна, используйте для мультимедийных файлов хранилище класса Premium и ссылайтесь на них из элементов Azure Cosmos. Такая схема естественным образом интегрируется с сетью доставки содержимого, позволяя распространять изображения с пограничного сервера по низкой цене и обойти географические ограничения. Единственный недостаток этого сценария заключается в том, что вам придется иметь дело с двумя службами (Azure Cosmos DB и хранилище BLOB-объектов Azure), что может повысить эксплуатационные расходы. 

## <a name="check-storage-consumed"></a>Проверка используемого объема хранилища

Чтобы проверить использование хранилища в контейнере Azure Cosmos, вы можете выполнить в нем запрос HEAD или GET и проверить заголовки `x-ms-request-quota` и `x-ms-request-usage` в ответе. Кроме того, при работе с пакетом SDK для .NET, можно использовать [DocumentSizeQuota](https://docs.microsoft.com/previous-versions/azure/dn850325(v%3Dazure.100)), и [DocumentSizeUsage](https://msdn.microsoft.com/library/azure/dn850324.aspx) свойства для получения использованного объема хранилища.

## <a name="using-sdk"></a>Использование пакета SDK

```csharp
// Measure the item size usage (which includes the index size)
ResourceResponse<DocumentCollection> collectionInfo = await client.ReadDocumentCollectionAsync(UriFactory.CreateDocumentCollectionUri("db", "coll"));   

Console.WriteLine("Item size quota: {0}, usage: {1}", collectionInfo.DocumentQuota, collectionInfo.DocumentUsage);
```

## <a name="next-steps"></a>Дальнейшие действия

Теперь вы можете перейти к подробным сведениям об оптимизации затрат в Azure Cosmos DB, которые представлены в следующих статьях.

* Дополнительные сведения [об оптимизации для разработки и тестирования](optimize-dev-test.md).
* Дополнительные сведения о [расшифровке счета Azure Cosmos DB](understand-your-bill.md)
* Дополнительные сведения об [оптимизации расходов на пропускную способность](optimize-cost-throughput.md).
* Дополнительные сведения об [оптимизации затрат на операции чтения и записи](optimize-cost-reads-writes.md).
* Дополнительные сведения об [оптимизации затрат на запросы](optimize-cost-queries.md).
* Дополнительные сведения об [оптимизации затрат на учетные записи Azure Cosmos с поддержкой нескольких регионов](optimize-cost-regions.md).

