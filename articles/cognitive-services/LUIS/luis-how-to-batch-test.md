---
title: Пакетный тест — LUIS
titleSuffix: Azure Cognitive Services
description: Использование наборов высказываний для пакетного тестирования Интеллектуальной службы распознавание речи (LUIS) для поиска фрагментов речи с неправильными намерениями и сущностями.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: article
ms.date: 03/29/2019
ms.author: diberry
ms.openlocfilehash: bd0910fedc9fd27af198f6a2b94b34daf57c050f
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/26/2019
ms.locfileid: "68563674"
---
# <a name="batch-testing-with-a-set-of-example-utterances"></a>Пакетное тестирование с помощью набора примеров высказываний

 Пакетное тестирование представляет собой комплексный тест текущей обученной модели для оценки ее производительности в LUIS. Наборы данных, используемые для пакетного тестирования, не должны включать примеры фраз в намерениях или фразах, полученных из конечной точки среды выполнения прогнозирования. 

<a name="batch-testing"></a>

## <a name="import-a-dataset-file-for-batch-testing"></a>Импорт файла набора данных для пакетного тестирования

1. В верхней строке нажмите кнопку **Test** (Тестировать), а затем щелкните ссылку **Batch testing panel** (Панель пакетного тестирования).

    ![Ссылка на пакетное тестирование](./media/luis-how-to-batch-test/batch-testing-link.png)

2. Нажмите кнопку **Import dataset** (Импортировать набор данных). Откроется диалоговое окно **Import new dataset** (Импорт нового набора данных). Нажмите кнопку **Выбрать файл** и расположите JSON-файл с правильным [форматом JSON](luis-concept-batch-test.md#batch-file-format) содержащий *не более 1000* высказываний для тестирования.

    Ошибки импорта выводятся в красной строке уведомлений в верхней части браузера. Если при импорте возникают ошибки, набор данных не создается. Дополнительные сведения см. в разделе об [общих ошибках](luis-concept-batch-test.md#common-errors-importing-a-batch).

3. В поле **Dataset Name** (Имя набора данных) введите имя для набора данных. Файл набора данных содержит **массив высказываний**, включая *помеченное намерение* и *сущности*. Просмотрите синтаксис [примера пакетного файла](luis-concept-batch-test.md#batch-file-format). 

4. Нажмите кнопку **Готово**. Будет добавлен файл набора данных.

## <a name="run-rename-export-or-delete-dataset"></a>Запуск, переименование, экспорт и удаление набора данных

Чтобы запустить, переименовать, экспортировать или удалить набор данных, используйте кнопку с многоточием (***...*** ) в конце строки набора данных.

![Действия с набором данных](./media/luis-how-to-batch-test/batch-testing-options.png)

## <a name="run-a-batch-test-on-your-trained-app"></a>Запуск пакетного теста в обученном приложении

Чтобы запустить тест, выберите имя набора данных. После завершения теста в этой строке отображается результат теста набора данных.

![Результат пакетного теста](./media/luis-how-to-batch-test/run-test.png)

Скачиваемый набор данных — это тот же файл, который был отправлен для пакетного тестирования.

|Область|Значение|
|--|--|
|![Зеленый круглый значок успешного выполнения теста](./media/luis-how-to-batch-test/batch-test-result-green.png)|Все высказывания успешно протестированы.|
|![Красный значок x неудачно выполненного теста](./media/luis-how-to-batch-test/batch-test-result-red.png)|По меньшей мере одно намерение высказывания не соответствует прогнозированию.|
|![Значок готовности к тестированию](./media/luis-how-to-batch-test/batch-test-result-blue.png)|Тест готов к запуску.|

<a name="access-batch-test-result-details-in-a-visualized-view"></a>

## <a name="view-batch-test-results"></a>Просмотр результатов пакетного тестирования 

Чтобы просмотреть результаты пакетного тестирования, щелкните **See results** (Просмотреть результаты).

![Результаты пакетного тестирования](./media/luis-how-to-batch-test/run-test-results.png)

<a name="filter-chart-results-by-intent-or-entity"></a>  

## <a name="filter-chart-results"></a>Фильтрация результатов на диаграмме

Чтобы отфильтровать диаграмму по определенному намерению или сущности, выберите намерение или сущность на панели фильтрации справа. Точки данных и их распределение на диаграмме обновляются в зависимости от выбора. 
 
![Визуализированный результат пакетного тестирования](./media/luis-how-to-batch-test/filter-by-entity.png) 

## <a name="view-single-point-utterance-data"></a>Просмотр данных высказывания для одной точки

На диаграмме наведите указатель мыши на точку данных, чтобы увидеть оценку достоверности ее прогнозирования. Выберите точку данных для извлечения ее соответствующего высказывания из списка высказываний в нижней части страницы. 

![Выбранное высказывание](./media/luis-how-to-batch-test/selected-utterance.png)


<a name="relabel-utterances-and-retrain"></a>
<a name="false-test-results"></a>

## <a name="view-section-data"></a>Просмотр данных раздела

В диаграмме из четырех частей выберите имя раздела, например **False Positive** (Ложноположительный результат), в правой верхней части диаграммы. Под диаграммой в списке отображаются все высказывания в указанном разделе. 

![Высказывания, выбранные по разделу](./media/luis-how-to-batch-test/selected-utterances-by-section.png)

На предыдущем рисунке высказывание `switch on` помечено с намерением TurnAllOn, но получило прогнозирование намерения None. Это указывает на то, что для выполнения ожидаемого прогнозирования намерению TurnAllOn необходимы дополнительные высказывания. 

Два красных раздела на диаграмме указывают на высказывания, которые не соответствуют ожидаемому прогнозированию. Это значит, что LUIS требуется дополнительное обучение. 

Два зеленых раздела на диаграмме означают соответствие ожидаемому прогнозированию.

[!INCLUDE [Entity roles in batch testing - currently not supported](../../../includes/cognitive-services-luis-roles-not-supported-in-batch-testing.md)]

## <a name="next-steps"></a>Следующие шаги

Если тестирование показывает, что приложение LUIS не распознает правильные намерения и сущности, вы можете повысить производительность приложения LUIS, пометив дополнительные высказывания или добавив компоненты. 

* я[Label suggested utterances with LUIS](luis-how-to-review-endpoint-utterances.md) (Пометка предлагаемых высказываний в LUIS) 
* [Use features to improve your LUIS app's performance](luis-how-to-add-features.md) (Использование функций для повышения производительности приложения LUIS) 
* [Understand batch testing with this tutorial](luis-tutorial-batch-testing.md) (Общие сведения о пакетном тестировании в этом руководстве)
* [Learn batch testing concepts](luis-concept-batch-test.md) (Общие сведения о принципах пакетного тестирования)
