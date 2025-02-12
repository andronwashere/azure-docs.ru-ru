---
title: Подробный обзор расширенной аналитики в Azure HDInsight
description: Узнайте, как в расширенной аналитике используются алгоритмы обработки больших данных.
author: ashishthaps
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 11/14/2017
ms.author: ashishth
ms.openlocfilehash: ac0edf2de4337154b665b8f3898134a7c2fd1f4c
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "64712397"
---
# <a name="deep-dive---advanced-analytics"></a>Подробный обзор расширенной аналитики

## <a name="what-is-advanced-analytics-for-hdinsight"></a>Расширенная аналитика для HDInsight

HDInsight предоставляет возможность получать ценные сведения из большого количества структурированных, неструктурированных и быстро меняющихся данных. Под расширенной аналитикой подразумевается использование масштабируемых архитектур, статистических моделей и моделей машинного обучения, а также интеллектуальных панелей мониторинга для предоставления информативных данных. Машинное обучение (или *прогнозная аналитика*) использует алгоритмы, которые идентифицируют и изучают отношения в данных, чтобы делать прогнозы и помогать в принятии решений.

## <a name="advanced-analytics-process"></a>Процесс расширенной аналитики

![Process](./media/apache-hadoop-deep-dive-advanced-analytics/process.png)

После того как вы определили бизнес-проблему и начали собирать и обрабатывать данные, вам необходимо создать модель для предсказания определенного вопроса. Модель будет использовать один или несколько алгоритмов машинного обучения, чтобы сделать тип прогноза, который соответствует вашим бизнес-требованиям.  Большую часть данных следует использовать для обучения модели, а остальную — для ее тестирования и оценки. 

После создания, загрузки, тестирования и оценки модель следует развернуть, чтобы она начала отвечать на ваши вопросы. На последнем шаге вы наблюдаете за эффективностью своей модели и настраиваете ее нужным образом. 

## <a name="common-types-of-algorithms"></a>Общие типы алгоритмов

Решения для расширенной аналитики предоставляют набор алгоритмов машинного обучения. Ниже приведен краткий обзор категорий алгоритмов и связанных с ними общих вариантов использования для бизнеса.

![Варианты использования машинного обучения](./media/apache-hadoop-deep-dive-advanced-analytics/ml-use-cases.png)

При выборе наиболее подходящего алгоритма необходимо учитывать, нужно ли предоставлять данные для обучения. Алгоритмы машинного обучения классифицируются следующим образом:

* Контролируемый алгоритм — для предоставления результатов его необходимо обучить по набору помеченных данных.
* Частично-контролируемый алгоритм — может быть дополнен объектами, которые не были доступны на начальном этапе обучения, посредством интерактивного запроса средством обучения.
* Неконтролируемый алгоритм — не требует данных для обучения. 
* Алгоритм с подкреплением — использует программные агенты для определения идеального поведения в определенном контексте (часто используется в робототехнике).


| Категория алгоритмов| Использование | Тип обучения | Алгоритмы |
| --- | --- | --- | -- |
| классификация; | Классификация людей или предметов по группам | Контролируемое | Деревья принятия решений, логистическая регрессия, нейронные сети |
| Кластеризация | Разделение набора примеров на однородные группы | Неконтролируемое | Кластеризация методом K-средних |
| Определение шаблона | Определение частых взаимосвязей в данных | Неконтролируемое | Правила взаимосвязей |
| Регрессия | Прогнозирование числовых результатов | Контролируемое | Линейная регрессия, нейронные сети |
| Подкрепление | Определение оптимальной работы программы-робота | Подкрепление | Моделирования методом Монте-Карло, DeepMind |

## <a name="machine-learning-on-hdinsight"></a>Машинное обучение в HDInsight

HDInsight имеет несколько вариантов машинного обучения для рабочего процесса расширенной аналитики:

* Машинное обучение и Apache Spark.
* Службы машинного обучения и R
* Машинное обучение Azure и Apache Hive
* Apache Spark и глубокое обучение

### <a name="machine-learning-and-apache-spark"></a>Машинное обучение и Apache Spark.


[HDInsight Spark](../spark/apache-spark-overview.md) — это размещаемая в Azure унифицированная платформа параллельной обработки данных [Apache Spark](https://spark.apache.org/) с открытым кодом, которая использует обработку в памяти для оптимизации анализа больших данных. Подсистема обработки Spark призвана ускорить разработку, повысить удобство использования и реализовать сложную аналитику. Возможности распределенного вычисления в памяти Spark отлично подходят для итеративных алгоритмов, используемых в машинном обучении и графовых вычислениях. 


Существуют три масштабируемых библиотеки машинного обучения Spark, которые предоставляют возможности алгоритмического моделирования для этой распределенной среды.

* [**MLlib**](https://spark.apache.org/docs/latest/ml-guide.html) — содержит исходный API, созданный на основе Spark RDDs.
* [**SparkML**](https://spark.apache.org/docs/1.2.2/ml-guide.html) — это новый пакет, который предоставляет API более высокого уровня, созданный поверх кадров данных Spark, для построения конвейеров машинного обучения.
* [**MMLSpark**](https://github.com/Azure/mmlspark) — библиотека машинного обучения Майкрософт для Apache Spark (MMLSpark), предназначенная оптимизировать работу специалистов по обработке и анализу данных в Spark, увеличить скорость реализации экспериментов и использовать самые современные методы машинного обучения, включая глубокое обучение, на очень больших наборах данных. Библиотека MMLSpark упрощает общие задачи моделирования для создания моделей в PySpark. 

### <a name="r-and-ml-services"></a>Службы машинного обучения и R

В рамках HDInsight можно создать кластер HDInsight со [Службами машинного обучения](../r-server/r-server-overview.md), который можно использовать с большими наборами данных и моделями. Эта новая возможность предоставляет специалистам по обработке и анализу данных, а также статистикам знакомый интерфейс R, который может масштабироваться по требованию через HDInsight без дополнительных затрат на настройку и обслуживание кластера.

### <a name="azure-machine-learning-and-apache-hive"></a>Машинное обучение Azure и Apache Hive

[Студия машинного обучения Azure](https://studio.azureml.net/) предоставляет средства для создания моделей прогнозной аналитики, а также полностью управляемую службу, которую можно использовать для развертывания прогнозных моделей как готовых к использованию веб-служб. Служба машинного обучения Azure дает разработчикам инструменты для создания полноценных решений прогнозирующей аналитики в облаке. Вы сможете быстро создавать, тестировать, развертывать и контролировать модели прогнозирования. Выбирайте из большой библиотеки алгоритмов, используйте веб-студию для создания моделей и с легкостью развертывайте свою модель в виде веб-службы.

### <a name="apache-spark-and-deep-learning"></a>Apache Spark и глубокое обучение

[Глубокое обучение](https://www.microsoft.com/research/group/dltc/) является типом машинного обучения, использующим *глубокие нейронные сети* (DNN), построенные по принципу биологических процессов в человеческом мозгу. Многие исследователи рассматривают глубокое обучение как перспективный подход к искусственному интеллекту. Некоторыми примерами глубокого обучения являются программы-переводчики разговорного языка, системы распознавания образов и механизмы машинного рассуждения. Чтобы оптимизировать свою работу в области глубокого обучения, корпорация Майкрософт разработала бесплатный простой в использовании набор средств с открытым исходным кодом — [Microsoft Cognitive Toolkit](https://www.microsoft.com/en-us/cognitive-toolkit/). Он широко используется множественными продуктами Microsoft, компаниями во всем мире, которым необходимо развертывать глубокое обучение в масштабе, а также учащимися, заинтересованными в современных алгоритмах и методах. 

## <a name="scenario---score-images-to-identify-patterns-in-urban-development"></a>Сценарий оценки изображений для определения шаблонов в градостроительстве

Рассмотрим пример конвейера машинного обучения расширенной аналитики с использованием HDInsight.

В этом сценарии вы увидите, как DNN, созданные на платформе глубокого обучения, и Microsoft Cognitive Toolkit (CNTK) могут быть использованы для оценки больших коллекций изображений, хранящихся в учетной записи хранилища BLOB-объектов Azure, с использованием PySpark в кластере HDInsight Spark. Этот подход применяется к общему варианту использования DNN, классификации аэрофотоснимков и может использоваться для определения последних шаблонов развития городов.  Вы будете использовать предварительно обученную модель классификации изображений. Модель предварительно обучена по [набору данных CIFAR 10](https://www.cs.toronto.edu/~kriz/cifar.html) и применена к 10 000 изображениям.

Этот сценарий расширенной аналитики состоит из трех основных задач.

1. Создание кластера Azure HDInsight Hadoop с дистрибутивом Apache Spark 2.1.0. 
2. Запуск пользовательского сценария для установки Microsoft Cognitive Toolkit на всех узлах в кластере Azure HDInsight Spark. 
3. Передача предварительно созданной записной книжки Jupyter в кластер HDInsight Spark для применения обученной модели углубленного обучения Microsoft Cognitive Toolkit к файлам в учетной записи хранилища BLOB-объектов Azure с помощью API Python для Spark (PySpark). 

В этом примере используется набор изображений CIFAR-10, скомпилированный и распространенный Алексом Крижевским (Alex Krizhevsky), Винодом Наиром (Vinod Nair) и Джеффри Хинтоном (Geoffrey Hinton). Набор данных CIFAR-10 содержит 60 000 цветных изображений размером 32 × 32, принадлежащих к 10 взаимоисключающим классам:

![Изображений](./media/apache-hadoop-deep-dive-advanced-analytics/ml-images.png)

Дополнительные сведения о наборе данных см. в работе Алекса Крижевского [Learning Multiple Layers of Features from Tiny Images](https://www.cs.toronto.edu/~kriz/learning-features-2009-TR.pdf) (Обучение нескольким слоям признаков на основе небольших изображений).

Набор данных был разделен на набор для обучения из 50 000 изображений и тестовый набор из 10 000 изображений. Первый набор был использован для обучения двадцатислойной глубокой модели на базе сверточной нейронной сети (ResNet) с использованием Microsoft Cognitive Toolkit, следуя [этому руководству](https://github.com/Microsoft/CNTK/tree/master/Examples/Image/Classification/ResNet) из репозитория Cognitive Toolkit GitHub. Оставшиеся 10 000 изображений были использованы для проверки точности модели. Именно здесь вступают в действие распределенные вычисления: задача предварительной обработки и оценки изображений имеет высокий уровень параллелизма. С сохраненной обученной моделью мы использовали:

* PySpark для распределения изображений и обученной модели на рабочих узлах кластера.
* Python для предварительной обработки изображений на каждом узле кластера HDInsight Spark.
* Cognitive Toolkit для загрузки модели и оценки предварительно обработанных изображений на каждом узле.
* Записные книжки Jupyter для запуска скрипта PySpark, агрегирования результатов и использования [Matplotlib](https://matplotlib.org/) для визуализации эффективности модели.

Вся предварительная обработка и оценка 10 000 изображений занимает менее одной минуты в кластере с 4 рабочими узлами. Модель точно прогнозирует метки примерно 9100 изображений (91 %). Матрица несоответствий демонстрирует наиболее распространенные ошибки классификации. Например, в матрице показано, что неправильная маркировка собак как кошек и наоборот происходит чаще, чем для других пар меток.

![Результаты](./media/apache-hadoop-deep-dive-advanced-analytics/ml-results.png)

### <a name="try-it-out"></a>Просто попробуйте!

Следуйте [этому руководству](../spark/apache-spark-microsoft-cognitive-toolkit.md), чтобы полностью реализовать это решение: настройте кластер HDInsight Spark, установите Cognitive Toolkit и запустите записную книжку Jupyter для оценки 10 000 изображений CIFAR.

## <a name="next-steps"></a>Дальнейшие действия

Apache Hive и служба "Машинное обучение Azure"

* [Процесс обработки и анализа данных группы на практике: использование кластеров Azure HDInsight Hadoop](../../machine-learning/team-data-science-process/hive-walkthrough.md)
* [Процесс обработки и анализа данных группы на практике: использование кластера Azure HDInsight Hadoop с набором данных объемом 1 ТБ](../../machine-learning/team-data-science-process/hive-criteo-walkthrough.md)

Apache Spark и MLLib

* [Общие сведения об обработке и анализе данных с помощью платформы Spark в Azure HDInsight](../../machine-learning/team-data-science-process/spark-overview.md)
* [Руководство. Создание приложения машинного обучения Apache Spark в HDInsight](../spark/apache-spark-ipython-notebook-machine-learning.md)
* [Использование Apache Spark MLlib для создания приложения машинного обучения и анализа набора данных](../spark/apache-spark-machine-learning-mllib-ipython.md)

Глубокое обучение, Cognitive Toolkit и другие

* [Общие сведения о виртуальных машинах Linux и Windows для обработки и анализа данных](../../machine-learning/data-science-virtual-machine/overview.md)
* [Introducing H2O.ai on Azure HDInsight](https://azure.microsoft.com/blog/introducing-h2o-ai-with-on-azure-hdinsight-to-bring-the-most-robust-ai-platform-for-enterprises/) (Знакомство с приложением H2O.ai в Azure HDInsight)
