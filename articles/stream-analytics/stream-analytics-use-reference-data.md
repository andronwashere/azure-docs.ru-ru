---
title: Использование эталонных данных для уточняющих запросов в Azure Stream Analytics
description: В этой статье описано использование эталонных данных для уточняющих запросов или корреляции в конструкторе запросов для заданий Azure Stream Analytics.
services: stream-analytics
author: jseb225
ms.author: jeanb
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 06/21/2019
ms.openlocfilehash: ed50dfd7e3c423c1c26a7dc19ae60dcb319f1850
ms.sourcegitcommit: 6a42dd4b746f3e6de69f7ad0107cc7ad654e39ae
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/07/2019
ms.locfileid: "67621615"
---
# <a name="using-reference-data-for-lookups-in-stream-analytics"></a>Использование эталонных данных для уточняющих запросов в Stream Analytics

Эталонные данные (также называются таблицами подстановки) — это ограниченный набор данных, который является статическим или медленно изменяющихся по своей природе, используется для выполнения уточняющего запроса или для дополнения потоков данных. Например, в сценарии Интернета вещей вы могли бы сохранить метаданные о датчиках (которые часто не изменяются) в ссылочных данных и соединить их с потоками данных Интернета вещей в режиме реального времени. Azure Stream Analytics загружает эталонные данные в память, чтобы сократить задержки при обработке потока. Для использования эталонных данных в задании Azure Stream Analytics обычно используется [соединение эталонных данных](https://docs.microsoft.com/stream-analytics-query/reference-data-join-azure-stream-analytics) в запросе. 

Stream Analytics поддерживает хранилище BLOB-объектов Azure и Базу данных SQL Azure как слой хранилища для эталонных данных. Вы также можете преобразовать или копировать эталонные данные в хранилище BLOB-объектов из Фабрики данных Azure, чтобы использовать [любое количество облачных и локальных хранилищ данных](../data-factory/copy-activity-overview.md).

## <a name="azure-blob-storage"></a>Хранилище BLOB-объектов Azure

Ссылочные данные моделируются как последовательность больших двоичных объектов (с учетом конфигурации входных данных) в порядке возрастания даты и времени, указанных в имени большого двоичного объекта. Они добавляются **только** в конец последовательности, для чего используется **более поздние** дата и время, чем в последнем большом двоичном объекте в последовательности.

### <a name="configure-blob-reference-data"></a>Настройка эталонных данных большого двоичного объекта

Чтобы настроить **ссылочные данные**, необходимо сначала создать входные данные соответствующего типа. В таблице ниже описано каждое свойство, которое необходимо указать во время создания входных ссылочных данных.

|**Имя свойства**  |**Описание**  |
|---------|---------|
|Псевдоним входных данных   | Понятное имя, с помощью которого запрос задания будет ссылаться на эти входные данные.   |
|Учетная запись хранения   | Имя учетной записи хранения, в которой находятся большие двоичные объекты. Если учетная запись расположена в одной подписке с заданием Stream Analytics, то ее имя можно выбрать из раскрывающегося списка.   |
|Ключ учетной записи хранения   | Секретный ключ, связанный с учетной записью хранения. Заполняется автоматически, если учетная запись хранения расположена в одной подписке с заданием Stream Analytics.   |
|Контейнер хранилища   | Контейнеры обеспечивают логическую группировку BLOB-объектов, хранящихся в службе BLOB-объектов Microsoft Azure. При передаче BLOB-объекта в службу BLOB-объектов для него необходимо указать контейнер.   |
|Шаблон пути   | Путь, используемый для поиска больших двоичных объектов в указанном контейнере. В пути можно указать один или несколько экземпляров следующих двух переменных:<BR>{date}, {time}<BR>Пример 1: products/{date}/{time}/product-list.csv<BR>Пример 2: products/{date}/product-list.csv<BR>Пример 3: product-list.csv<BR><br> Если большой двоичный объект не существует по указанному пути, задание Stream Analytics будет бесконечно ожидать, пока он не станет доступным.   |
|Формат даты (необязательное свойство)   | Если в указанном шаблоне пути использовалась переменная {date}, то из раскрывающегося списка поддерживаемых форматов можно выбрать формат даты для упорядочивания больших двоичных объектов.<BR>Пример: ГГГГ/ММ/ДД, ММ/ДД/ГГГГ и т. д.   |
|Формат времени (необязательное свойство)   | Если в указанном шаблоне пути использовалась переменная {time}, то из раскрывающегося списка поддерживаемых форматов можно выбрать формат времени для упорядочивания больших двоичных объектов.<BR>Пример: ЧЧ, ЧЧ/мм или ЧЧ-мм.  |
|Формат сериализации событий   | Чтобы запросы работали как следует, в задании Stream Analytics нужно указать, какой формат сериализации используется для потоков входящих данных. Поддерживаемые форматы для ссылочных данных: CSV и JSON.  |
|кодировка   | В настоящее время единственным поддерживаемым форматом кодировки является UTF-8.  |

### <a name="static-reference-data"></a>Статические ссылочные данные

Если ссылочные данные не будут изменяться, поддержку статических данных можно настроить, указав статический путь в конфигурации входных данных. Azure Stream Analytics будет обрабатывать BLOB-объекты по указанному пути. Токены подстановки {date} и {time} можно не использовать, Поскольку ссылочные данные — это неизменяемые в Stream Analytics, перезапись большого двоичного объекта данных статическую ссылку не рекомендуется.

### <a name="generate-reference-data-on-a-schedule"></a>Создание эталонных данных по расписанию

Если ссылочные данные являются медленно изменяющимся набором данных, вы можете включить поддержку обновления ссылочных данных. Для этого укажите соответствующий шаблон пути во входной конфигурации, используя токены подстановки {date} и {time}. На основе этого свойства path pattern служба Stream Analytics будет выбирать обновленные определения ссылочных данных. Например, шаблон `sample/{date}/{time}/products.csv` с форматом даты **ГГГГ-ММ-ДД** и форматом времени **ЧЧ:ММ** указывает службе Stream Analytics выбрать обновленный большой двоичный объект `sample/2015-04-16/17-30/products.csv` в 17:30 16 апреля 2015 г. в часовом поясе UTC.

Azure Stream Analytics каждую минуту автоматически проверяет BLOB-объекты со ссылочными данными на наличие изменений. Если с небольшой задержкой (например, 10:30:30) передается большой двоичный объект с отметки времени 10:30:00, можно заметить, что небольшая задержка в задании Stream Analytics, ссылающиеся на этот большой двоичный объект. Чтобы избежать таких сценариев, рекомендуется отправить более ранних, чем время начала целевого большого двоичного объекта (10: 30:00 в этом примере) чтобы предоставить достаточно времени и задание Stream Analytics, чтобы обнаружить и загрузить его в памяти и выполнения операций. 

> [!NOTE]
> В настоящее время задания Stream Analytics ищут обновления больших двоичных объектов, только если время, закодированное в имени большого двоичного объекта, отстает от времени компьютера. Например, задание будет искать `sample/2015-04-16/17-30/products.csv` как можно раньше, но не ранее 17:30 16 апреля 2015 г. в часовом поясе UTC. Оно *никогда* не будет искать большой двоичный объект, закодированное имя которого опережает время в имени последнего обнаруженного большого двоичного объекта.
> 
> Например, когда задание находит большой двоичный объект `sample/2015-04-16/17-30/products.csv` он игнорирует любые файлы с Дата предшествующая 17:30 16 апреля 2015 г таким образом Если закодирована `sample/2015-04-16/17-25/products.csv` большой двоичный объект создается в том же контейнере задание не будет его использовать.
> 
> Аналогичным образом, если `sample/2015-04-16/17-30/products.csv` создается в 22:03 16 апреля 2015 г., но в контейнере нет большого двоичного объекта с более ранней датой, задание будет использовать этот файл начиная с 22:03 16 апреля 2015 г., а до этого времени будет использовать ссылочные данные за прошедшее время.
> 
> Исключение составляют случаи, когда заданию необходимо повторно обработать данные за прошлые периоды или когда задание запускается впервые. В начале выполнения задание ищет самый новый большой двоичный объект, созданный до указанного времени запуска задания. Это позволяет обеспечить наличие **непустого** набора ссылочных данных на момент начала задания. Если его не удалось найти, задание будет показывать следующие данные диагностики: `Initializing input without a valid reference data blob for UTC time <start time>`.

[Фабрика данных Azure](https://azure.microsoft.com/documentation/services/data-factory/) может использоваться для управления заданием по созданию обновленных больших двоичных объектов, необходимых службе Stream Analytics для обновления определений ссылочных данных. Фабрика данных представляет собой облачную службу интеграции информации, которая организует и автоматизирует перемещение и преобразование данных. Фабрика данных позволяет [подключаться к большому количеству облачных и локальных хранилищ данных](../data-factory/copy-activity-overview.md), а также легко перемещать данные по заданному расписанию. Дополнительные сведения и пошаговые инструкции, с помощью которых можно настроить конвейер фабрики данных и создать ссылочные данные для службы Stream Analytics, обновляемые по заданному расписанию, см. в этом [примере на сайте GitHub](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/ReferenceDataRefreshForASAJobs).

### <a name="tips-on-refreshing-blob-reference-data"></a>Советы по обновлению эталонных данных большого двоичного объекта

1. Не перезаписывайте BLOB-объекты ссылочных данных, так как они не должны изменяться.
2. Обновлять ссылочные данные рекомендуется такими способами:
    * использовать {date} и {time} в шаблоне пути;
    * добавить новый BLOB-объект, используя тот же контейнер и шаблон пути, которые определены во входных данных задания;
    * использовать дату и время, **большие**, чем дата и время, указанные в последнем BLOB-объекте в последовательности.
3. Большие двоичные объекты ссылочных данных упорядочиваются **не** по времени их последнего изменения, а по времени и дате, указанным в имени большого двоичного объекта с помощью подстановок {date} и {time}.
3. Чтобы не отобразилось слишком большое количество больших двоичных объектов, попробуйте удалить очень старые большие двоичные объекты, которые больше не будут обрабатываться. Обратите внимание на то, что ASA может быть понадобиться повторно обработать несколько больших двоичных объектов в таких сценариях, как перезагрузка.

## <a name="azure-sql-database"></a>База данных SQL Azure

Эталонные данные базы данных SQL Azure извлекаются заданием Stream Analytics и хранятся в памяти в виде моментального снимка для обработки. Моментальный снимок эталонных данных также хранится в контейнере в учетной записи хранения, указанной в параметрах конфигурации. Контейнер создается автоматически при запуске задания. Если задание остановлено или переходит в состояние сбоя, при его перезапуске автоматически созданные контейнеры удаляются.  

Если ссылочные данные являются медленно изменяющимся набором данных, необходимо периодически обновлять моментальный снимок, который используется в задании. Stream Analytics позволяет задать частоту обновления при настройке входного подключения к базе данных SQL Azure. Среда выполнения Stream Analytics будет запрашивать базу данных SQL Azure в интервале, заданном частотой обновления. Наиболее быстрая частота обновления, которая поддерживается — один раз в минуту. Для каждого обновления Stream Analytics сохраняет новый моментальный снимок в указанной учетной записи хранения.

Stream Analytics предоставляет два варианта выполнения запросов к базе данных SQL Azure. Запрос моментального снимка является обязательным, и его необходимо включить в каждое задание. Stream Analytics выполняет запрос моментального снимка периодически, в зависимости от интервала обновления, и использует результат запроса (моментального снимка) в качестве эталонного набора данных. Запрос моментального снимка должен соответствовать большинству сценариев, но при возникновении проблем с производительностью с большими наборами данных и быстрой частотой обновления, можете использовать параметр разностного запроса. Запросы, которые принимают более 60 секунд для возврата эталонного набора данных приведет к времени ожидания.

С параметром разностных запросов Stream Analytics выполняет запрос моментального снимка изначально, чтобы получить базовый набор эталонных данных. После чего Stream Analytics выполняет разностный запрос, в зависимости от интервала обновления, чтобы получить добавочные изменения. Эти добавочные изменения постоянно применяются к эталонному набору данных, чтобы обновлять его. Благодаря разностному запросу сократите затраты на хранение и сетевые операции ввода-вывода.

### <a name="configure-sql-database-reference"></a>Настройка ссылки на Базу данных SQL

Чтобы настроить эталонные данные в Базе данных SQL, необходимо сначала создать входные **Эталонные данные**. В указанной ниже таблице описано каждое свойство, которое необходимо предоставить во время создания входных эталонных данных. Дополнительные сведения см. в статье [Use reference data from a SQL Database for an Azure Stream Analytics job (Preview)](sql-reference-data.md) (Использование эталонных данных из Базы данных SQL для задания Azure Stream Analytics (предварительная версия)).

|**Имя свойства**|**Описание**  |
|---------|---------|
|Псевдоним входных данных|Понятное имя, с помощью которого запрос задания будет ссылаться на эти входные данные.|
|Subscription|Выберите свою подписку|
|База данных|База данных SQL Azure с эталонными данными.|
|Имя пользователя|Имя пользователя, связанное с базой данных SQL Azure.|
|Пароль|Пароль, связанный с базой данных SQL Azure.|
|Периодическое обновление|Этот параметр позволяет выбрать частоту обновления. Выбор "Вкл." позволит вам указать частоту обновления в формате ДД:ЧЧ:MM.|
|Запрос моментального снимка|Это параметр запроса по умолчанию, который извлекает ссылочные данные из Базы данных SQL.|
|Разностный запрос|Для более сложных сценариев с большими наборами данных и краткой частотой обновления выберите это, чтобы добавить разностной запрос.|

## <a name="size-limitation"></a>Ограничение размера

Stream Analytics поддерживает эталонные данные **размером не более 300 МБ**. Ограничение в 300 МБ для эталонных данных можно соблюдать только при использовании простых запросов. Когда сложность запросов возрастает и они включают в себя обработку с отслеживанием состояния, например оконные агрегатные функции, темпоральные соединения и темпоральные аналитические функции, ожидается, что поддерживаемый объем эталонных данных снизится. Если Azure Stream Analytics не сможет загружать эталонные данные и выполнять сложные операции, заданию не хватит памяти и оно завершится сбоем. В таких случаях показатель использования единиц потоковой передачи в процентах достигнет 100 %.    

|**Количество единиц потоковой передачи**  |**Прибл. макс. допустимый размер (в МБ)**  |
|---------|---------|
|1   |50   |
|3   |150   |
|6 и более   |300   |

Если количество единиц потоковой передачи для задания превысит 6, максимальный поддерживаемый объем эталонных данных не увеличится.

Для ссылочных данных сжатие не поддерживается. 

## <a name="next-steps"></a>Следующие шаги
> [!div class="nextstepaction"]
> [Краткое руководство по созданию задания Stream Analytics с помощью портала Azure](stream-analytics-quick-create-portal.md)

<!--Link references-->
[stream.analytics.developer.guide]: ../stream-analytics-developer-guide.md
[stream.analytics.scale.jobs]: stream-analytics-scale-jobs.md
[stream.analytics.introduction]: stream-analytics-real-time-fraud-detection.md
[stream.analytics.get.started]: stream-analytics-get-started.md
[stream.analytics.query.language.reference]: https://go.microsoft.com/fwlink/?LinkID=513299
[stream.analytics.rest.api.reference]: https://go.microsoft.com/fwlink/?LinkId=517301
