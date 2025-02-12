---
title: Kickstart экспериментов из примеров
titleSuffix: Azure Machine Learning Studio
description: Узнайте, как использовать примеры экспериментов машинного обучения для создания экспериментов с использованием Коллекции решений ИИ Azure и Студии машинного обучения Azure.
services: machine-learning
ms.service: machine-learning
ms.subservice: studio
ms.topic: conceptual
author: xiaoharper
ms.author: amlstudiodocs
ms.custom: seodec18, previous-author=heatherbshapiro, previous-ms.author=hshapiro
ms.date: 01/05/2018
ms.openlocfilehash: f88323069ed23f4a038ffa4a030b1c4d4541ec42
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "61460385"
---
# <a name="create-azure-machine-learning-studio-experiments-from-working-examples-in-azure-ai-gallery"></a>Создание экспериментов Студии машинного обучения на основе рабочих примеров из Коллекции решений ИИ Azure

Узнайте, как использовать примеры экспериментов машинного обучения из [коллекции решений Azure AI](https://gallery.azure.ai/), чтобы не создавать собственные решения с нуля. Эти примеры помогут вам создать решение машинного обучения.

В коллекции содержатся примеры экспериментов, созданные командой Студии машинного обучения Azure, а также участниками сообщества машинного обучения. Также можно задавать вопросы и публиковать комментарии об экспериментах.

Чтобы узнать, как использовать коллекцию, просмотрите 3-минутное видео [Копирование работы других пользователей для обработки и анализа данных](data-science-for-beginners-copy-other-peoples-work-to-do-data-science.md) из серии [Обработка и анализ данных для начинающих](data-science-for-beginners-the-5-questions-data-science-answers.md).



## <a name="find-an-experiment-to-copy-in-azure-ai-gallery"></a>Поиск эксперимента для копирования в коллекцию решений Azure AI
Чтобы просмотреть доступные эксперименты, перейдите в раздел [Коллекция](https://gallery.azure.ai/) и щелкните **Эксперименты** в верхней части страницы.

### <a name="find-the-newest-or-most-popular-experiments"></a>Поиск последних и самых популярных экспериментов
На этой странице можно просмотреть **недавно добавленные** эксперименты, а также **популярные эксперименты** или последние **популярные эксперименты Майкрософт**.

### <a name="look-for-an-experiment-that-meets-specific-requirements"></a>Поиск эксперимента, соответствующего определенным требованиям
Для просмотра всех экспериментов выполните следующие действия.

1. В верхней части страницы щелкните **Просмотреть все** .
2. В левой части окна в разделе **Refine by** (Отфильтровать по) в разделе **Категории** выберите **Эксперимент**, чтобы просмотреть все эксперименты в коллекции.
3. Эксперименты, соответствующие определенным требованиям, можно найти несколькими различными способами.
   * **Выберите фильтры в левой части окна.** Например, чтобы просмотреть эксперименты, в которых используется алгоритм обнаружения аномалий на основе PCA, сделайте следующее. В разделе **Категории** выберите **Эксперимент**. Затем в разделе **Используемые алгоритмы** нажмите кнопку **Показать все** и в диалоговом окне выберите **Обнаружение аномалий на основе PCA**. Может потребоваться выполнить прокрутку.<br></br>
     ![Выбор фильтров](./media/sample-experiments/choose-an-algorithm.png)
   * **Используйте поле поиска.** Например, чтобы найти эксперименты, предоставленные корпорацией Майкрософт и относящиеся к распознаванию цифр с использованием алгоритма двухклассовой машины опорных векторов, введите "распознавание цифр" в поле поиска. Затем выберите фильтры **Experiment** (Эксперимент), **Microsoft content only** (Только содержимое Майкрософт) и **Two-Class Support Vector Machine** (Двухклассовая машина опорных векторов).<br></br>
     ![Использование поля поиска](./media/sample-experiments/search-for-experiments.png)
4. Щелкните эксперимент, чтобы узнать о нем подробнее.
5. Чтобы запустить или изменить эксперимент, щелкните **Open in Studio** (Открыть в Студии) на странице эксперимента. <br></br>

    ![Пример эксперимента](./media/sample-experiments/example-experiment.png)

    > [!NOTE]
    > При первом открытии эксперимента в Студии машинного обучения можно испытать его бесплатно или приобрести подписку Azure. Дополнительные сведения о бесплатной пробной и платной версии Студии машинного обучения см. на странице [цен на машинное обучение](https://azure.microsoft.com/pricing/details/machine-learning/).
    >
    >

## <a name="create-a-new-experiment-using-an-example-as-a-template"></a>Создание эксперимента, используя в качестве шаблона пример
Эксперимент в студии машинного обучения также можно создать, используя пример из коллекции в качестве шаблона.

1. Войдите в [Студию](https://studio.azureml.net) с использованием учетной записи Майкрософт, после чего щелкните **Создать**, чтобы создать эксперимент.
2. Просмотрите примеры содержимого и выберите то, что вам подходит.

В рабочей области студии машинного обучения будет создан эксперимент на основе выбранного примера в качестве шаблона.

## <a name="next-steps"></a>Дальнейшие действия
* [Импорт обучающих данных в Студию машинного обучения Azure из разных источников данных](import-data.md)
* [Краткое руководство по языку программирования R для службы машинного обучения Azure](r-quickstart.md)
* [Развертывание веб-службы машинного обучения Azure](publish-a-machine-learning-web-service.md)
