---
title: Фабрика данных Azure. Часто задаваемые вопросы | Документация Майкрософт
description: Получите ответы на часто задаваемые вопросы о фабрике данных Azure.
services: data-factory
documentationcenter: ''
author: sharonlo101
manager: craigg
ms.assetid: 532dec5a-7261-4770-8f54-bfe527918058
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 06/27/2018
ms.author: shlo
ms.openlocfilehash: d704c32ee7417c6460ad6cc880e451adddfa61de
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "61345760"
---
# <a name="azure-data-factory-faq"></a>Часто задаваемые вопросы о фабрике данных Azure
Эта статья содержит ответы на часто задаваемые вопросы о фабрике данных Azure.  

## <a name="what-is-azure-data-factory"></a>Что такое фабрика данных Azure? 
Фабрика данных представляет собой полностью управляемую облачную службу интеграции информации, которая автоматизирует перемещение и преобразование данных. Как на фабрике сырье превращается в готовую продукцию с помощью оборудования, так и в фабриках данных Azure необработанные данные собираются и преобразовываются в готовые к использованию сведения с помощью специальных служб. 

Фабрика данных Azure позволяет создавать управляемые данными рабочие процессы для перемещения данных между локальными и облачными хранилищами данных. Можно обрабатывать и преобразовывать данные с помощью служб вычислений, например Azure HDInsight, Azure Data Lake Analytics и среды выполнения интеграции SQL Server Integration Services (SSIS). 

С помощью фабрики данных вы можете выполнить обработку данных, используя облачную службу на основе Azure или собственную вычислительную среду с локальным размещением, например SSIS, SQL Server или Oracle. После создания конвейера, который выполняет необходимое действие, можно запланировать периодический запуск (почасовая, ежедневно или еженедельно, например), временное окно планирования, или запуск на основе произошедшего события. Дополнительную информацию см. в статье [Общие сведения о службе фабрики данных Azure, службе интеграции данных в облаке](introduction.md).

### <a name="control-flows-and-scale"></a>Потоки управления и масштабирование 
Для поддержки разнообразных последовательностей и шаблонов интеграции в современных хранилищах данных, фабрика данных включает гибкие данных конвейера моделирования. Это влечет за собой полный контроль потока парадигм, которые включают условное выполнение программирования, ветвления в конвейеры данных, а также возможность явно передавать параметры внутри потоков и между ними. Поток управления также включает в себя преобразование данных с помощью диспетчеризация действий для подсистемы выполнения внешних и возможности потока данных, включая перемещение данных в масштабе, с помощью действия копирования.

Фабрика данных позволяет создать любой поток для своего сценария интеграции данных и запускать его по запросу или постоянно по расписанию. Ниже приведены несколько общих потоков, которые эта модель поддерживает.   

- Потоки управления:
    - Действия можно связывать друг с другом в последовательности в конвейере.
    - Действия могут быть разветвлены в конвейере.
    - Параметры
        - Параметры могут быть определены на уровне конвейера и аргументы, которые могут быть переданы при вызове конвейера по запросу или из триггера.
        - Действия могут использовать аргументы, передаваемые в конвейер.
    - Передача пользовательского состояния:
        - Выходные данные действия, включая состояние, могут использоваться в следующем действии в конвейере.
    - Контейнеры зацикливания:
        - Действие foreach вызывает перебор заданной коллекции действий в цикле. 
- Потоки на основе триггеров:
    - Конвейеры можно вызывать по требованию или в определенное время.
- Разностные потоки:
    - Параметры могут использоваться для определения вашей наибольшей активности для разностного копирования при перемещении справочных таблиц или измерений из реляционного хранилища, либо локально или в облаке, для загрузки данных в озеро. 

Дополнительные сведения см. в статье [Руководство. Ветвления и создание цепочки действий в конвейере фабрики данных](tutorial-control-flow.md).

### <a name="data-transformed-at-scale-with-code-free-pipelines"></a>Данные преобразуются в нужном масштабе с конвейерами без кода
Новый браузерный инструментарий позволяет писать и развертывать конвейеры без кода с помощью современного интерактивного веб-интерфейса.

Для визуальных данных разработчики и инженеры данных фабрика данных веб-Интерфейс является среде разработки без кода, который будет использоваться для построения конвейеров. Он полностью интегрирован с Visual Studio Online Git и обеспечивает интеграцию для Непрерывной и последовательной разработки с помощью параметров отладки.

### <a name="rich-cross-platform-sdks-for-advanced-users"></a>Пакеты SDK для кросс платформенных широкие возможности для опытных пользователей
Фабрика данных версии 2 предоставляет широкий набор пакетов SDK, которые могут использоваться для создания, администрирования и мониторинга конвейеров с помощью избранного интерфейса IDE, включая:
* Пакет SDK для Python
* Интерфейс командной строки PowerShell
* Пакет SDK для C#

Пользователи также могут использовать документированные интерфейсы REST API для взаимодействия с фабрикой данных версии 2.

### <a name="iterative-development-and-debugging-by-using-visual-tools"></a>Итеративные разработка и отладка с помощью визуальных инструментов
Azure визуальные инструменты для фабрики данных включите итеративные разработка и отладка. Вы можете создать конвейеров и тестовых запусков с помощью **Отладка** возможность на холсте конвейера, не написав ни строчки кода. Можно просмотреть результаты тестовых запусков в **вывода** окно холста конвейера. После успешного завершения тестового запуска, можно добавить дополнительные действия в конвейер и продолжайте отладку итеративным методом. После в ходе тестовых запусков можно отменить. 

Не требуется опубликовать изменения в службе фабрики данных, прежде чем выбирать **Отладка**. Это может оказаться полезным в сценариях, где вы хотите убедиться, что новые дополнения или изменения будет работать должным образом, перед обновлением рабочих процессов фабрики данных в средах разработки, тестирования или производства. 

### <a name="ability-to-deploy-ssis-packages-to-azure"></a>Возможность развертывания пакетов служб SSIS в Azure 
Если вы хотите перемещать рабочие нагрузки служб SSIS, то создайте фабрику данных и подготовьте среду выполнения интеграции Azure SSIS. Среду выполнения интеграции Azure SSIS — это полностью управляемый кластер виртуальных машин Azure (узлов), выделенный для выполнения пакетов служб SSIS в облаке. Пошаговые инструкции см. в руководстве [Развертывание пакетов служб интеграции SQL Server (SSIS) в Azure](tutorial-create-azure-ssis-runtime-portal.md). 
 
### <a name="sdks"></a>Пакеты SDK
Если вы являетесь опытным пользователем, и ищет программный интерфейс, фабрика данных предоставляет широкий набор пакетов SDK, которые можно использовать для создания, управления и мониторинга конвейеров с помощью вашей любимой интегрированной среде разработки. Поддерживаются такие языки, как .NET, Python, PowerShell и REST.

### <a name="monitoring"></a>Мониторинг
Фабрики данных можно отслеживать с помощью PowerShell, пакета SDK и визуальных средств наблюдения в браузерном пользовательском интерфейсе. Можно отслеживать и управлять по запросу, на основе триггеров: и часы управляемых пользовательских потоков эффективным способом. Существующие задачи "Отмена", см. в разделе сбои, вы можете быстро выполнить детализацию для получения подробных сообщений об ошибках и отладки проблем, все из одной панели без переключения контекста или перехода вперед и назад между экранами. 

### <a name="new-features-for-ssis-in-data-factory"></a>Новые возможности служб SSIS в фабрике данных
Так как начальная общедоступная Предварительная версия в 2017 г., фабрика данных добавлены следующие компоненты для служб SSIS:

-   Поддерживает три дополнительные конфигурации и вариантов базы данных SQL Azure для размещения базы данных служб SSIS (SSISDB) проектов и пакетов.
-   База данных SQL с конечными точками службы виртуальной сети
-   Управляемый экземпляр
-   Эластичный пул
-   Поддержка виртуальной сетью Azure Resource Manager на основе классической виртуальной сети будет признан устаревшим в будущем, который позволяет внедрить и соединений среды выполнения интеграции Azure SSIS к виртуальной сети, настроенных для базы данных SQL с помощью службы виртуальной сети доступ конечных точек и MI/локальных данных. Дополнительные сведения см. также [присоединить среду выполнения интеграции Azure SSIS к виртуальной сети](join-azure-ssis-integration-runtime-virtual-network.md).
-   Поддержка аутентификации Azure Active Directory (Azure AD) и проверку подлинности SQL для подключения к базе данных SSISDB, позволяя проверки подлинности Azure AD с помощью фабрики данных управляемого удостоверения для ресурсов Azure
-   Поддержка собственной лицензии SQL Server в локальной и получить экономию существенную часть затрат из элемента "Преимущество гибридного использования Azure"
-   Поддержка в выпуске Enterprise Edition среды выполнения интеграции Azure SSIS, которая позволяет использовать возможности advanced и "премиум", интерфейс пользовательской установки для установки дополнительных компонентов или расширения и экосистема партнеров. Дополнительные сведения см. также [Enterprise Edition, Выборочная установка и сторонних производителей расширяемости для служб SSIS в ADF](https://blogs.msdn.microsoft.com/ssis/2018/04/27/enterprise-edition-custom-setup-and-3rd-party-extensibility-for-ssis-in-adf/). 
-   Более глубокую интеграцию служб SSIS в фабрике данных, который позволяет вызывать или триггера первого класса действия выполнение пакета служб SSIS в конвейерах фабрики данных и планировать их с помощью SSMS. Дополнительные сведения см. также [Modernize и распространить рабочие процессы ETL/ELT с помощью действия SSIS в конвейерах фабрики данных AZURE](https://blogs.msdn.microsoft.com/ssis/2018/05/23/modernize-and-extend-your-etlelt-workflows-with-ssis-activities-in-adf-pipelines/).


## <a name="what-is-the-integration-runtime"></a>Что такое среда выполнения интеграции?
Среда IR — это вычислительная инфраструктура, используемые фабрикой данных Azure для обеспечивает следующие возможности интеграции данных в различных сетевых средах:

- **Перемещение данных**. Для перемещения данных среда IR перемещает данные между исходной и целевой хранилищами данных, а также поддержку встроенных соединителей, преобразования формата, сопоставление столбцов и высокой производительностью и масштабируемая передача данных.
- **Диспетчеризация действий**. Для преобразования среда IR предоставляет изначальную возможность выполнять пакеты служб SSIS.
- **Выполнение пакетов служб SSIS**. Среда выполнения интеграции в собственном коде выполняет пакеты служб SSIS в управляемой среде вычислений Azure. Среда IR также поддерживает отправки и мониторинга действий по преобразованию в разных вычислительных служб, таких как Azure HDInsight, машинное обучение Azure, база данных SQL и SQL Server.

Можно развернуть один или несколько экземпляров среды выполнения интеграции, когда возникает необходимость перемещения и преобразования данных. Среда IR может работать в общедоступной сети Azure или в частной сети (локальной, виртуальной сети Azure или виртуальном частном облаке Amazon Web Services [VPC]). 

Дополнительные сведения см. в статье [Среда выполнения интеграции в фабрике данных Azure](concepts-integration-runtime.md).

## <a name="what-is-the-limit-on-the-number-of-integration-runtimes"></a>Что такое ограничение количества сред выполнения интеграции?
В фабрике данных нет жестких ограничений на количество экземпляров среды выполнения интеграции. Однако есть ограничение на число ядер виртуальной машины, которые среда выполнения интеграции может использовать для каждой подписки при выполнении пакетов служб SSIS. Дополнительные сведения см. в разделе [Ограничения фабрики данных](../azure-subscription-service-limits.md#data-factory-limits).

## <a name="what-are-the-top-level-concepts-of-azure-data-factory"></a>Какие основные концепции в фабрике данных Azure?
В подписке Azure может быть один или несколько экземпляров фабрики данных Azure. Фабрика данных Azure содержит четыре ключевых компонента. Они образуют платформу, на которой можно создавать управляемые данными рабочие процессы, предусматривающие перемещение и преобразование данных.

### <a name="pipelines"></a>Конвейеры
Фабрика данных может иметь один или несколько конвейеров. Конвейер — это логическая группа действий, которые выполняют определенный блок задач. Действия в конвейере совместно выполняют задачу. Например, конвейер может включать группу действий, которые принимают данные из большого двоичного объекта Azure и выполняют запрос Hive в кластере HDInsight для секционирования данных. Преимуществом является то, что конвейер позволяет управлять группами действий, а не каждым отдельным действием. Вы можете связать вместе действия в конвейере, чтобы выполнять их последовательно, или выполнять их параллельно и независимо друг от друга.

### <a name="activities"></a>Действия
Действия представляют отдельные этапы обработки в конвейере. Например можно использовать действие копирования для копирования данных из одного хранилища данных в другое хранилище данных. Точно так же можно использовать действие Hive, которое выполняет запрос Hive к кластеру Azure HDInsight для преобразования или анализа данных. Фабрика данных поддерживает три типа действий: действия перемещения данных, действия преобразования данных и действия управления.

### <a name="datasets"></a>Наборы данных
Наборы данных представляют структуры данных в хранилищах. Эти структуры указывают данные, необходимые для использования в действиях, разделяя их на входные и выходные. 

### <a name="linked-services"></a>Связанные службы
Связанные службы напоминают строки подключения, определяющие сведения о подключении, необходимые для подключения фабрики данных к внешним ресурсам. Подумайте об использовании этого таким образом: Связанная служба определяет подключение к источнику данных и набор данных представляет структуру данных. Например, связанная служба хранилища Azure определяет строку подключения для подключения к учетной записи хранения Azure. И набор данных BLOB-объектов Azure указывает контейнер больших двоичных объектов и папку, которая содержит данные.

Связанные службы используются в фабрике данных для двух целей:

- Для представления *хранилища данных*, включая, помимо прочего, локальный экземпляр SQL Server, экземпляр базы данных Oracle, общую папку и учетную запись хранилища BLOB-объектов Azure. Список поддерживаемых хранилищ данных см. в статье [Действие копирования в фабрике данных Azure](copy-activity-overview.md).
- Для представления *вычислительного ресурса*, в котором можно выполнить действие. Например, действие HDInsightHive выполняется в кластере Hadoop в HDInsight. Список поддерживаемых действий преобразования и вычислительных сред см. в статье [Преобразование данных в фабрике данных Azure](transform-data.md).

### <a name="triggers"></a>Триггеры
Триггеры обозначают единицу обработки, которая определяет время запуска для выполнения конвейера. Существует несколько типов триггеров для разных событий. 

### <a name="pipeline-runs"></a>Запуски конвейера
Запуск конвейера — это экземпляр выполнения конвейера. Запуск конвейера обычно выполняется путем передачи аргументов для параметров, определенных в конвейере. Вы можете передать аргументы вручную или в определении триггера.

### <a name="parameters"></a>Параметры
Параметры представляют собой пары "ключ — значение" в конфигурации только для чтения. Вы определяете параметры в конвейере и передаете для них аргументы во время выполнения из контекста запуска. Контекст запуска создается триггером или из конвейера, который выполняется вручную. Действия в конвейере используют значения параметров.

Набор данных — это строго типизированный параметр и сущность, которая может ссылаться и использовать повторно. Действие может ссылаться на наборы данных, и он может использовать параметры, которые определены в определении набора данных.

Связанная служба также является строго типизированным параметром, который содержит сведения о подключении к хранилищу данных или среде вычислений. Это также сущность, которая может ссылаться и использовать повторно.

### <a name="control-flows"></a>Потоки управления
Потоки управления выполняют оркестрацию действий в конвейере, которая включает цепочки действий в последовательности, ветвление и параметры, определяемые на уровне конвейера, а также аргументы, которые передаются во время вызова конвейера по запросу или из триггера. Сюда также входит передача пользовательского состояния и контейнеров зацикливания (то есть итераторы foreach).


Дополнительные сведения о понятиях фабрики данных см. в следующих статьях:

- [Datasets and linked services in Azure Data Factory](concepts-datasets-linked-services.md) (Наборы данных и связанные службы в фабрике данных Azure)
- [Конвейеры и действия](concepts-pipelines-activities.md)
- [Среда выполнения интеграции в фабрике данных Azure](concepts-integration-runtime.md)

## <a name="what-is-the-pricing-model-for-data-factory"></a>Какая модель ценообразования применяется для фабрики данных?
Подробные сведения о ценах на фабрику данных Azure см. на [этой странице](https://azure.microsoft.com/pricing/details/data-factory/).

## <a name="how-can-i-stay-up-to-date-with-information-about-data-factory"></a>Как оставаться в курсе последних новостей о фабрике данных?
Чтобы узнавать о последних новостях о фабрике данных Azure, используйте следующие сайты:

- [Блог](https://azure.microsoft.com/blog/tag/azure-data-factory/)
- [Документация по фабрике данных Azure V2](/azure/data-factory)
- [Домашняя страница продукта](https://azure.microsoft.com/services/data-factory/)

## <a name="technical-deep-dive"></a>Подробное техническое руководство 

### <a name="how-can-i-schedule-a-pipeline"></a>Как запланировать конвейер? 
Для планирования конвейера можно использовать триггер планировщика или триггер по временному окну. Триггер использует определенное время по расписанию календарь, который будет планировать работу конвейеров периодически или на основе календаря повторяющимся шаблонам на (понедельникам в 18:00:00) и четвергам в 21:00:00. Дополнительные сведения см. в статье [Выполнение конвейера и триггеры в фабрике данных Azure](concepts-pipeline-execution-triggers.md).

### <a name="can-i-pass-parameters-to-a-pipeline-run"></a>Можно ли передать параметры в выполнение конвейера?
Да, параметры являются полноправной основной концепцией в фабрике данных. Вы можете определить параметры на уровне конвейера и передать аргументы при выполнении конвейера, запускаемого по требованию или с помощью триггера.  

### <a name="can-i-define-default-values-for-the-pipeline-parameters"></a>Можно ли определить значения по умолчанию для параметров конвейера? 
Да. Вы можете определить значения по умолчанию для параметров в конвейерах. 

### <a name="can-an-activity-in-a-pipeline-consume-arguments-that-are-passed-to-a-pipeline-run"></a>Может ли действие в конвейере использовать аргументы, передаваемые в конвейер? 
Да. Каждое действие в рамках конвейера может использовать значение параметра, переданное в конвейер и запущенное с помощью конструкции `@parameter`. 

### <a name="can-an-activity-output-property-be-consumed-in-another-activity"></a>Может ли свойство из выходных данных действия использоваться в другом действии? 
Да. Выходные данные действия могут использоваться в последующем действии. Для этого применяется конструкция `@activity`.
 
### <a name="how-do-i-gracefully-handle-null-values-in-an-activity-output"></a>Как корректно обрабатывать значения null в выходных данных действия? 
Для корректной обработки значений null в выражениях можно использовать конструкцию `@coalesce`. 

## <a name="mapping-data-flows"></a>Сопоставления потоков данных

### <a name="which-data-factory-version-do-i-use-to-create-data-flows"></a>Какую версию фабрики данных следует использовать для создания потоков данных
Используйте версию фабрики данных версии 2 для создания потоков данных.
  
### <a name="i-was-a-previous-private-preview-customer-who-used-data-flows-and-i-used-the-data-factory-v2-preview-version-for-data-flows"></a>Я был предыдущих клиентам закрытой предварительной версии, кто использовал потоки данных, и я использовал в предварительной версии фабрики данных версии 2 для потоков данных.
Эта версия больше не используется. Использование фабрики данных версии 2 для потоков данных.
  
### <a name="what-has-changed-from-private-preview-to-limited-public-preview-in-regard-to-data-flows"></a>Что изменилось с закрытой предварительной версии на ограниченной общедоступной предварительной версии в отношении потоков данных?
Больше не придется вывести собственные кластеры Azure Databricks. Создание кластера и демонтажа для управления фабрики данных. Наборы данных больших двоичных объектов и наборов данных Gen2 хранилища Озера данных Azure разделены текстовый файл с разделителями и наборы данных Apache Parquet. Gen2 хранилища Озера данных и хранилище BLOB-объектов по-прежнему можно использовать для хранения этих файлов. Используйте соответствующую связанную службу для этих подсистемы хранилища.

### <a name="can-i-migrate-my-private-preview-factories-to-data-factory-v2"></a>Можно ли перенести мой закрытая Предварительная версия фабрики в фабрика данных версии 2?

Да. [Следуйте инструкциям](https://www.slideshare.net/kromerm/adf-mapping-data-flow-private-preview-migration).

### <a name="i-need-help-troubleshooting-my-data-flow-logic-what-info-do-i-need-to-provide-to-get-help"></a>Мне нужна помощь, устранение неполадок моей логику потока данных. Какие сведения нужно предоставить помощь?

Когда корпорация Майкрософт предоставляет помощь или Устранение неполадок с помощью потоков данных, укажите план кода DSL. Для этого выполните следующие действия.

1. Конструктор потоков данных, выберите **кода** в правом верхнем углу. При этом отобразится редактируемый код JSON для потока данных.
2. В представлении кода выберите **планирование** в правом верхнем углу. С помощью этого переключателя переключается из JSON только для чтения формате DSL сценарий плана.
3. Скопируйте и вставьте этот сценарий или сохраните его в текстовый файл.

### <a name="how-do-i-access-data-by-using-the-other-80-dataset-types-in-data-factory"></a>Как открыть данные с помощью 80 других типов наборов данных в фабрике данных?

Сопоставление потока данных в настоящее время позволяет базы данных SQL Azure, хранилище данных SQL Azure, с разделителями, текстовые файлы из хранилища BLOB-объектов Azure или Gen2 хранилища Озера данных Azure и Parquet-файлы из хранилища BLOB-объектов или Gen2 хранилища Озера данных в собственном коде для источника и приемника. 

Использование действия копирования для промежуточного хранения данных из любого из других соединителей, а затем выполните действием потока данных для преобразования данных, после его размещается. Например конвейер сначала скопирует в хранилище BLOB-объектов и затем действием потока данных будет использовать набор данных в источнике для преобразования этих данных.

## <a name="next-steps"></a>Дальнейшие действия
Пошаговые инструкции по созданию фабрики данных см. в следующих руководствах:

- [Краткое руководство Создание фабрики данных и конвейера с помощью пакета SDK .NET](quickstart-create-data-factory-dot-net.md)
- [Учебник. Копирование данных из хранилища BLOB-объектов Azure в базу данных SQL Azure с помощью фабрики данных Azure](tutorial-copy-data-dot-net.md)
