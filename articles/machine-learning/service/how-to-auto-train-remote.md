---
title: Удаленные целевые объекты вычислений автоматического машинного обучения
titleSuffix: Azure Machine Learning service
description: Узнайте, как создавать модели с помощью автоматизированного машинного обучения на Машинное обучение Azure удаленном целевом объекте вычислений со службой Машинное обучение Azure
services: machine-learning
author: nacharya1
ms.author: nilesha
ms.reviewer: sgilley
ms.service: machine-learning
ms.subservice: core
ms.workload: data-services
ms.topic: conceptual
ms.date: 7/12/2019
ms.openlocfilehash: 852190f7b66c0d2c527d1784c72f963e11620064
ms.sourcegitcommit: c71306fb197b433f7b7d23662d013eaae269dc9c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/22/2019
ms.locfileid: "68371108"
---
# <a name="train-models-with-automated-machine-learning-in-the-cloud"></a>Обучение моделей с помощью автоматического машинного обучения в облаке

В службе "Машинное обучение Azure" можно обучить модель для различных типов доступных для управления вычислительных ресурсов. Целью вычислений может быть локальный компьютер или ресурс в облаке.

Вы можете легко масштабировать или масштабировать эксперимент машинного обучения, добавив дополнительные целевые объекты вычислений, например Машинное обучение Azure Compute (Амлкомпуте). Амлкомпуте — это управляемая инфраструктура вычислений, которая позволяет легко создавать один или несколько узлов.

Из этой статьи вы узнаете, как создать модель с помощью автоматизированного ML с Амлкомпуте.

## <a name="how-does-remote-differ-from-local"></a>Чем удаленный компьютер отличается от локального?

Руководство «[обучение модели классификации с автоматизированным машинным обучением](tutorial-auto-train-models.md)» рассказывает о том, как использовать локальный компьютер для обучения модели с помощью автоматизированного ml. Рабочий процесс, используемый при локальном обучении, применяется и для удаленных целевых объектов. Тем не менее при использовании удаленного объекта вычислений итерации экспериментов автоматического машинного обучения выполняются асинхронно. Эта функция позволяет отменить конкретную итерацию, просмотреть состояние выполнения или продолжить работать с другими ячейками в записной книжке Jupyter. Чтобы обучить удаленно, сначала необходимо создать удаленный целевой объект вычислений, например Амлкомпуте. Затем необходимо настроить удаленный ресурс и отправить на него код.

В этой статье приведены дополнительные шаги, необходимые для запуска автоматического эксперимента ML на удаленном целевом объекте Амлкомпуте. Объект рабочей области, `ws`, из учебника используется во всем представленном ниже коде.

```python
ws = Workspace.from_config()
```

## <a name="create-resource"></a>Создать ресурс

Создайте целевой объект амлкомпуте в рабочей области (`ws`), если он еще не создан.

**Оценка времени**. Создание целевого объекта Амлкомпуте занимает примерно 5 минут.

```python
from azureml.core.compute import AmlCompute
from azureml.core.compute import ComputeTarget

amlcompute_cluster_name = "automlcl"  # Name your cluster
provisioning_config = AmlCompute.provisioning_configuration(vm_size="STANDARD_D2_V2",
                                                            # for GPU, use "STANDARD_NC6"
                                                            # vm_priority = 'lowpriority', # optional
                                                            max_nodes=6)

compute_target = ComputeTarget.create(
    ws, amlcompute_cluster_name, provisioning_config)

# Can poll for a minimum number of nodes and for a specific timeout.
# If no min_node_count is provided, it will use the scale settings for the cluster.
compute_target.wait_for_completion(
    show_output=True, min_node_count=None, timeout_in_minutes=20)
```

Теперь можно использовать объект `compute_target` в качестве целевого объекта удаленных вычислений.

К ограничениям имен кластеров относятся:
+ Должно быть короче 64 символов.
+ Не может содержать ни один из следующих символов: `\` ~ ! @ # $ % ^ & * ( ) = + _ [ ] { } \\\\ | ; : \' \\" , < > / ?.`

## <a name="access-data-using-getdata-function"></a>Доступ к данным с помощью функции get_data ()

Предоставьте удаленному ресурсу доступ к обучающим данным. Для экспериментов автоматического машинного обучения на удаленном объекте вычислений необходимо извлечь данные с помощью функции `get_data()`.

Для предоставления доступа необходимо выполнить следующие действия:
+ Создайте файл get_data.py, содержащий функцию `get_data()`.
+ Поместите этот файл в каталог, доступный как абсолютный путь.

Код для чтения данных из хранилища BLOB-объектов или локальном диске можно инкапсулировать в файле get_data.py. В следующем примере кода данные поступают из пакета sklearn.

```python
# Create a project_folder if it doesn't exist
if not os.path.exists(project_folder):
    os.makedirs(project_folder)

#Write the get_data file.
%%writefile $project_folder/get_data.py

from sklearn import datasets
from scipy import sparse
import numpy as np

def get_data():

    digits = datasets.load_digits()
    X_digits = digits.data[10:,:]
    y_digits = digits.target[10:]

    return { "X" : X_digits, "y" : y_digits }
```

## <a name="create-run-configuration"></a>Создать конфигурацию запуска

Чтобы сделать зависимости доступными для скрипта get_data. корректировки, определите `RunConfiguration` объект с определенным `CondaDependencies`. Используйте этот объект для `run_configuration` параметра в. `AutoMLConfig`

```python
from azureml.core.runconfig import RunConfiguration
from azureml.core.conda_dependencies import CondaDependencies

run_config = RunConfiguration(framework="python")
run_config.target = compute_target
run_config.environment.docker.enabled = True
run_config.environment.docker.base_image = azureml.core.runconfig.DEFAULT_CPU_IMAGE

dependencies = CondaDependencies.create(
    pip_packages=["scikit-learn", "scipy", "numpy"])
run_config.environment.python.conda_dependencies = dependencies
```

Дополнительные примеры этого конструктивного шаблона см. в этом [примере записной книжке](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/remote-amlcompute/auto-ml-remote-amlcompute.ipynb) .

## <a name="configure-experiment"></a>Настройка эксперимента

Задайте значения для `AutoMLConfig`.  (См. [полный список параметров](how-to-configure-auto-train.md#configure-experiment) и их возможные значения.)

```python
from azureml.train.automl import AutoMLConfig
import time
import logging

automl_settings = {
    "name": "AutoML_Demo_Experiment_{0}".format(time.time()),
    "iteration_timeout_minutes": 10,
    "iterations": 20,
    "n_cross_validations": 5,
    "primary_metric": 'AUC_weighted',
    "preprocess": False,
    "max_concurrent_iterations": 10,
    "verbosity": logging.INFO
}

automl_config = AutoMLConfig(task='classification',
                             debug_log='automl_errors.log',
                             path=project_folder,
                             compute_target=compute_target,
                             run_configuration=run_config,
                             data_script=project_folder + "/get_data.py",
                             **automl_settings,
                             )
```

### <a name="enable-model-explanations"></a>Включение описаний модели

Задайте дополнительный параметр `model_explainability` в конструкторе `AutoMLConfig`. Кроме того, объект проверки кадра данных нужно передать в качестве параметра `X_valid` для использования функции объясняемости модели.

```python
automl_config = AutoMLConfig(task='classification',
                             debug_log='automl_errors.log',
                             path=project_folder,
                             compute_target=compute_target,
                             run_configuration=run_config,
                             data_script=project_folder + "/get_data.py",
                             **automl_settings,
                             model_explainability=True,
                             X_valid=X_test
                             )
```

## <a name="submit-training-experiment"></a>Отправка обучающего эксперимента

Теперь отправьте конфигурацию для автоматического выбора алгоритма, гиперпараметров и обучения модели.

```python
from azureml.core.experiment import Experiment
experiment = Experiment(ws, 'automl_remote')
remote_run = experiment.submit(automl_config, show_output=True)
```

Результат будет выглядеть примерно так:

    Running on remote compute: mydsvmParent Run ID: AutoML_015ffe76-c331-406d-9bfd-0fd42d8ab7f6
    ***********************************************************************************************
    ITERATION: The iteration being evaluated.
    PIPELINE:  A summary description of the pipeline being evaluated.
    DURATION: Time taken for the current iteration.
    METRIC: The result of computing score on the fitted pipeline.
    BEST: The best observed score thus far.
    ***********************************************************************************************

     ITERATION     PIPELINE                               DURATION                METRIC      BEST
             2      Standardize SGD classifier            0:02:36                  0.954     0.954
             7      Normalizer DT                         0:02:22                  0.161     0.954
             0      Scale MaxAbs 1 extra trees            0:02:45                  0.936     0.954
             4      Robust Scaler SGD classifier          0:02:24                  0.867     0.954
             1      Normalizer kNN                        0:02:44                  0.984     0.984
             9      Normalizer extra trees                0:03:15                  0.834     0.984
             5      Robust Scaler DT                      0:02:18                  0.736     0.984
             8      Standardize kNN                       0:02:05                  0.981     0.984
             6      Standardize SVM                       0:02:18                  0.984     0.984
            10      Scale MaxAbs 1 DT                     0:02:18                  0.077     0.984
            11      Standardize SGD classifier            0:02:24                  0.863     0.984
             3      Standardize gradient boosting         0:03:03                  0.971     0.984
            12      Robust Scaler logistic regression     0:02:32                  0.955     0.984
            14      Scale MaxAbs 1 SVM                    0:02:15                  0.989     0.989
            13      Scale MaxAbs 1 gradient boosting      0:02:15                  0.971     0.989
            15      Robust Scaler kNN                     0:02:28                  0.904     0.989
            17      Standardize kNN                       0:02:22                  0.974     0.989
            16      Scale 0/1 gradient boosting           0:02:18                  0.968     0.989
            18      Scale 0/1 extra trees                 0:02:18                  0.828     0.989
            19      Robust Scaler kNN                     0:02:32                  0.983     0.989


## <a name="explore-results"></a>Изучите результаты.

Вы можете использовать тот же [мини](https://docs.microsoft.com/python/api/azureml-widgets/azureml.widgets?view=azure-ml-py) -приложение Jupyter, как показано в [учебном курсе](tutorial-auto-train-models.md#explore-the-results) , чтобы просмотреть диаграмму и таблицу результатов.

```python
from azureml.widgets import RunDetails
RunDetails(remote_run).show()
```

Вот как выглядит статическое изображение мини-приложения.  В записной книжке можно щелкнуть любую строку таблицы, чтобы просмотреть свойства прогона и файлы журналов для этого прогона.   Для просмотра графика каждой доступной метрики для каждой итерации можно также использовать раскрывающийся список над графиком.

![таблица мини-приложения](./media/how-to-auto-train-remote/table.png)
![график мини-приложения](./media/how-to-auto-train-remote/plot.png)

Мини-приложение показывает URL-адрес, который можно использовать для просмотра и изучения данных отдельного прогона.  

Если вы не в записной книжке Jupyter, можно отобразить URL-адрес из самого запуска:

```
remote_run.get_portal_url()
```

Та же информация доступна в вашей рабочей области.  Дополнительные сведения об этих результатах см. в разделе [понимание автоматических результатов машинного обучения](how-to-understand-automated-ml.md).

### <a name="view-logs"></a>Просмотреть журналы

Найдите журналы на DSVM в каталоге `/tmp/azureml_run/{iterationid}/azureml-logs`.

## <a name="explain"></a>Лучшее описание модели

Извлечение данных описания модели позволяет просмотреть подробные данные о моделях для повышения прозрачности компонентов, выполняемых на внутреннем сервере. В этом примере вы выполняете описания модели только для самой подходящей модели. При выполнении для всех моделей в конвейере это займет значительное время. Описание модели включает следующее:

* shap_values: Сведения о пояснениях, создаваемых библиотекой ШАП lib.
* expected_values: ожидаемое значение модели, применяемое к набору данных X_train.
* overall_summary: Значения важности функций уровня модели сортируются в порядке убывания.
* overall_imp: Имена функций сортируются в том же порядке, что и в overall_summary.
* per_class_summary: значения важности функций на уровне класса, отсортированные в порядке убывания. Доступно только для случая классификации.
* per_class_imp: имена компонентов, отсортированных в порядке, описанном в per_class_summary. Доступно только для случая классификации.

Используйте следующий код, чтобы выбрать лучший конвейер из итераций. Метод `get_output` возвращает лучшее выполнение и соответствующую модель для последнего соответствующего вызова.

```python
best_run, fitted_model = remote_run.get_output()
```

Импортируйте функцию `retrieve_model_explanation` и выполните оптимальную модель.

```python
from azureml.train.automl.automlexplainer import retrieve_model_explanation

shap_values, expected_values, overall_summary, overall_imp, per_class_summary, per_class_imp = \
    retrieve_model_explanation(best_run)
```

Выведите результаты для переменных описания `best_run`, которые вы хотите просмотреть.

```python
print(overall_summary)
print(overall_imp)
print(per_class_summary)
print(per_class_imp)
```

Вывод результатов переменных описаний сводки `best_run` в следующих выходных данных:

![Выходные данные консоли объясняемости модели](./media/how-to-auto-train-remote/expl-print.png)

Вы также можете визуализировать важность функции с помощью пользовательского интерфейса мини-приложения, а также пользовательского веб-интерфейса на портале Azure в рабочей области.

![Пользовательский интерфейс объясняемости модели](./media/how-to-auto-train-remote/model-exp.png)

## <a name="example"></a>Пример

В записной книжке [хов-то-усе-азуремл/аутоматед-Мачине-леарнинг/ремоте-амлкомпуте/АУТО-мл-ремоте-амлкомпуте. ipynb](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/remote-amlcompute/auto-ml-remote-amlcompute.ipynb) показаны понятия, описанные в этой статье.

[!INCLUDE [aml-clone-in-azure-notebook](../../../includes/aml-clone-for-examples.md)]

## <a name="next-steps"></a>Следующие шаги

Узнайте, [как настроить параметры автоматического обучения](how-to-configure-auto-train.md)
