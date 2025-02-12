---
title: Руководство по классификации изображений. Развертывание моделей
titleSuffix: Azure Machine Learning service
description: В этом руководстве показано, как использовать службу "Машинное обучение Azure" для развертывания модели классификации изображений с помощью scikit-learn в Jupyter Notebook для Python. Это руководство представляет собой вторую часть серии, состоящей из двух частей.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: tutorial
author: sdgilley
ms.author: sgilley
ms.date: 05/14/2019
ms.custom: seodec18
ms.openlocfilehash: c673fd43abe6808256eb74f435aad48ed8d41539
ms.sourcegitcommit: 4b647be06d677151eb9db7dccc2bd7a8379e5871
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/19/2019
ms.locfileid: "68359842"
---
# <a name="tutorial-deploy-an-image-classification-model-in-azure-container-instances"></a>Руководство по Развертывание модели классификации изображений в Экземплярах контейнеров Azure

Это руководство представляет собой **вторую часть серии, состоящей из двух частей**. В [предыдущем руководстве](tutorial-train-models-with-aml.md) вы обучили модели машинного обучения и затем зарегистрировали модель в рабочей области в облаке.  

Теперь вы готовы развернуть модель как веб-службу в [Экземплярах контейнеров Azure](https://docs.microsoft.com/azure/container-instances/). Веб-служба — это образ. В данном случае образ Docker. Он инкапсулирует логику оценки и саму модель. 

В этой части вы будете использовать Службу машинного обучения Azure, чтобы научиться выполнять следующие задачи:

> [!div class="checklist"]
> * настройка тестовой среды;
> * извлечение модели из рабочей области;
> * локальное тестирование модели;
> * развертывание модели в Экземплярах контейнеров;
> * тестирование развернутой модели.

Экземпляры контейнеров — это идеальное решение для тестирования и понимания рабочего процесса. Для масштабируемых рабочих развертываний рекомендуется использовать Службу Azure Kubernetes. Дополнительные сведения см. в статье [Развертывание моделей с помощью Службы машинного обучения Azure](how-to-deploy-and-where.md).

>[!NOTE]
> Код в этой статье протестирован с помощью пакета SDK для Машинного обучения Azure версии 1.0.41.

## <a name="prerequisites"></a>Предварительные требования
Перейдите к разделу о [настройке среды разработки](#start), чтобы ознакомиться с шагами записной книжки.  

Чтобы запустить записную книжку, выполните обучение модели, как описано в статье [Руководство (часть 1). Обучение модели классификации изображений с помощью Службы машинного обучения Azure](tutorial-train-models-with-aml.md).   Затем запустите записную книжку **tutorials/img-classification-part2-deploy.ipynb**, используя тот же сервер записных книжек.


## <a name="start"></a>Настройка среды

Начните с создания тестовой среды.

### <a name="import-packages"></a>Импорт пакетов

Импортируйте пакеты Python, необходимые для этого руководства.

```python
%matplotlib inline
import numpy as np
import matplotlib
import matplotlib.pyplot as plt
 
import azureml
from azureml.core import Workspace, Run

# display the core SDK version number
print("Azure ML SDK Version: ", azureml.core.VERSION)
```

### <a name="retrieve-the-model"></a>Извлечение модели

В предыдущем руководстве вы зарегистрировали модель в рабочей области. Теперь загрузите эту рабочую область и скачайте модель в локальный каталог.


```python
from azureml.core import Workspace
from azureml.core.model import Model
import os
ws = Workspace.from_config()
model = Model(ws, 'sklearn_mnist')

model.download(target_dir=os.getcwd(), exist_ok=True)

# verify the downloaded model file
file_path = os.path.join(os.getcwd(), "sklearn_mnist_model.pkl")

os.stat(file_path)
```

## <a name="test-the-model-locally"></a>Локальное тестирование модели

Перед развертыванием убедитесь, что модель работает локально, выполнив следующие действия:
* загрузку тестовых данных;
* прогнозирование тестовых данных;
* изучение матрицы неточностей.

### <a name="load-test-data"></a>Загрузка тестовых данных

Загрузите тестовые данные из каталога **./data/** , созданного в обучающем руководстве:

```python
from utils import load_data
import os

data_folder = os.path.join(os.getcwd(), 'data')
# note we also shrink the intensity values (X) from 0-255 to 0-1. This helps the neural network converge faster
X_test = load_data(os.path.join(data_folder, 'test-images.gz'), False) / 255.0
y_test = load_data(os.path.join(
    data_folder, 'test-labels.gz'), True).reshape(-1)
```

### <a name="predict-test-data"></a>Прогнозирование тестовых данных

Чтобы получить прогнозы, введите тестовый набор данных в модель:

```python
import pickle
from sklearn.externals import joblib

clf = joblib.load(os.path.join(os.getcwd(), 'sklearn_mnist_model.pkl'))
y_hat = clf.predict(X_test)
```

###  <a name="examine-the-confusion-matrix"></a>Изучение матрицы неточностей

Создайте матрицу неточностей, чтобы увидеть количество правильно классифицированных выборок из тестового набора. Обратите внимание на неправильно классифицированное значение неверных прогнозов: 

```python
from sklearn.metrics import confusion_matrix

conf_mx = confusion_matrix(y_test, y_hat)
print(conf_mx)
print('Overall accuracy:', np.average(y_hat == y_test))
```

Отображается следующая матрица неточностей:

    [[ 960    0    1    2    1    5    6    3    1    1]
     [   0 1112    3    1    0    1    5    1   12    0]
     [   9    8  920   20   10    4   10   11   37    3]
     [   4    0   17  921    2   21    4   12   20    9]
     [   1    2    5    3  915    0   10    2    6   38]
     [  10    2    0   41   10  770   17    7   28    7]
     [   9    3    7    2    6   20  907    1    3    0]
     [   2    7   22    5    8    1    1  950    5   27]
     [  10   15    5   21   15   27    7   11  851   12]
     [   7    8    2   13   32   13    0   24   12  898]]
    Overall accuracy: 0.9204
   

Используйте `matplotlib` для отображения матрицы неточностей в виде графа. В этом графе ось X показывает фактические значения, а ось Y — прогнозируемые значения. Цвет в каждой сетке показывает частоту ошибок. Чем светлее цвет, тем выше частота ошибок. Например, многие цифры 5 неправильно классифицированы как 3. Поэтому вы видите яркую сетку в расположении (5,3).

```python
# normalize the diagonal cells so that they don't overpower the rest of the cells when visualized
row_sums = conf_mx.sum(axis=1, keepdims=True)
norm_conf_mx = conf_mx / row_sums
np.fill_diagonal(norm_conf_mx, 0)

fig = plt.figure(figsize=(8, 5))
ax = fig.add_subplot(111)
cax = ax.matshow(norm_conf_mx, cmap=plt.cm.bone)
ticks = np.arange(0, 10, 1)
ax.set_xticks(ticks)
ax.set_yticks(ticks)
ax.set_xticklabels(ticks)
ax.set_yticklabels(ticks)
fig.colorbar(cax)
plt.ylabel('true labels', fontsize=14)
plt.xlabel('predicted values', fontsize=14)
plt.savefig('conf.png')
plt.show()
```

![Диаграмма, где отображается матрица неточностей](./media/tutorial-deploy-models-with-aml/confusion.png)

## <a name="deploy-as-a-web-service"></a>Развертывание в виде веб-службы

Если после тестирования модели вы получили устраивающие вас результаты, разверните модель в виде веб-службы, размещенной в Экземплярах контейнеров. 

Чтобы создать правильную среду для Экземпляров контейнеров, предоставьте следующие компоненты:
* сценарий оценки, чтобы показать, как использовать модель;
* файл среды, чтобы показать, какие пакеты должны быть установлены;
* файл конфигурации для создания экземпляра контейнера;
* обученная ранее модель.

<a name="make-script"></a>

### <a name="create-scoring-script"></a>Создание сценария оценки

Создайте сценарий оценки, называемый **score.py**. Он нужен в вызове веб-службы, чтобы показать, как использовать модель.

В сценарий оценки необходимо включить две обязательные функции:
* Функция `init()`, которая обычно загружает модель в глобальный объект. Эта функция выполняется только один раз при запуске контейнера Docker. 

* Функция `run(input_data)` использует модель для прогнозирования значения на основе входных данных. Входные и выходные данные для запуска обычно используют JSON для сериализации и десериализации, но поддерживаются и другие форматы.

```python
%%writefile score.py
import json
import numpy as np
import os
import pickle
from sklearn.externals import joblib
from sklearn.linear_model import LogisticRegression

from azureml.core.model import Model

def init():
    global model
    # retrieve the path to the model file using the model name
    model_path = Model.get_model_path('sklearn_mnist')
    model = joblib.load(model_path)

def run(raw_data):
    data = np.array(json.loads(raw_data)['data'])
    # make prediction
    y_hat = model.predict(data)
    # you can return any data type as long as it is JSON-serializable
    return y_hat.tolist()
```

<a name="make-myenv"></a>

### <a name="create-environment-file"></a>Создание файла среды

Создайте файл среды **myenv.yml**, который указывает все зависимости пакетов для этого сценария. Этот файл гарантирует, что все эти зависимости устанавливаются в образ Docker. Для этой модели требуется `scikit-learn` и `azureml-sdk`.

```python
from azureml.core.conda_dependencies import CondaDependencies

myenv = CondaDependencies()
myenv.add_conda_package("scikit-learn")

with open("myenv.yml", "w") as f:
    f.write(myenv.serialize_to_string())
```
Проверьте содержимое файла `myenv.yml`.

```python
with open("myenv.yml", "r") as f:
    print(f.read())
```

### <a name="create-a-configuration-file"></a>Создание файла конфигурации

Создайте файл конфигурации развертывания. Укажите необходимое для контейнера службы "Экземпляры контейнеров" количество ЦП и объем ОЗУ в гигабайтах. Хотя это зависит от модели, для многих моделей по умолчанию обычно бывает достаточно одного ядра и 1 ГБ ОЗУ. Если в дальнейшем вам потребуется больше ресурсов, создайте образ еще раз и повторно разверните службу.

```python
from azureml.core.webservice import AciWebservice

aciconfig = AciWebservice.deploy_configuration(cpu_cores=1,
                                               memory_gb=1,
                                               tags={"data": "MNIST",
                                                     "method": "sklearn"},
                                               description='Predict MNIST with sklearn')
```

### <a name="deploy-in-container-instances"></a>Развертывание в Экземплярах контейнеров
Примерное время завершения развертывания — **около 7–8 минут**.

Настройте изображение и разверните его. Следующий код выполняет указанные далее действия.

1. Создание образа с использованием таких файлов:
   * файл оценки (`score.py`);
   * файл среды (`myenv.yml`);
   * файл модели.
1. Регистрация образа в рабочей области. 
1. Отправка образа в контейнер службы "Экземпляры контейнеров".
1. Запуск контейнера в Экземплярах контейнеров с помощью образа.
1. Получение конечной точки HTTP веб-службы.


```python
%%time
from azureml.core.webservice import Webservice
from azureml.core.image import ContainerImage

# configure the image
image_config = ContainerImage.image_configuration(execution_script="score.py", 
                                                  runtime="python", 
                                                  conda_file="myenv.yml")

service = Webservice.deploy_from_model(workspace=ws,
                                       name='sklearn-mnist-svc',
                                       deployment_config=aciconfig,
                                       models=[model],
                                       image_config=image_config)

service.wait_for_deployment(show_output=True)
```

Получите конечную точку HTTP веб-службы оценки, которая принимает вызовы клиента REST. К этой конечной точке можно предоставить совместный доступ всем, кто хочет протестировать веб-службу или интегрировать ее в приложение. 

```python
print(service.scoring_uri)
```


## <a name="test-the-deployed-service"></a>Тестирование развернутой службы

Ранее вы оценили все тестовые данные с помощью локальной версии модели. Теперь можно проверить развернутую модель, используя случайную выборку из 30 изображений в тестовых данных.  

Следующий код выполняет указанные далее действия.
1. Отправка данных в виде массива JSON в веб-службу, размещенную в Экземплярах контейнеров. 

1. Использование API `run` пакета SDK для вызова службы. Вы также можете выполнять необработанные вызовы с помощью любого средства HTTP, например **curl**.

1. Вывод возвращенных прогнозов и отображение их вместе с входными изображениями. Красный шрифт и обратное изображение (белое на черном фоне) используются для выделения неправильно классифицированных изображений. 

Так как модель характеризуется высокой точностью, перед отображением неправильно классифицированной выборки может потребоваться несколько раз запустить следующий код.

```python
import json

# find 30 random samples from test set
n = 30
sample_indices = np.random.permutation(X_test.shape[0])[0:n]

test_samples = json.dumps({"data": X_test[sample_indices].tolist()})
test_samples = bytes(test_samples, encoding='utf8')

# predict using the deployed model
result = service.run(input_data=test_samples)

# compare actual value vs. the predicted values:
i = 0
plt.figure(figsize=(20, 1))

for s in sample_indices:
    plt.subplot(1, n, i + 1)
    plt.axhline('')
    plt.axvline('')

    # use different color for misclassified sample
    font_color = 'red' if y_test[s] != result[i] else 'black'
    clr_map = plt.cm.gray if y_test[s] != result[i] else plt.cm.Greys

    plt.text(x=10, y=-10, s=result[i], fontsize=18, color=font_color)
    plt.imshow(X_test[s].reshape(28, 28), cmap=clr_map)

    i = i + 1
plt.show()
```

Это результат одной случайной выборки тестовых изображений:

![Рисунок, показывающий результаты](./media/tutorial-deploy-models-with-aml/results.png)

Для проверки веб-службы можно также отправить необработанный HTTP-запрос.

```python
import requests

# send a random row from the test set to score
random_index = np.random.randint(0, len(X_test)-1)
input_data = "{\"data\": [" + str(list(X_test[random_index])) + "]}"

headers = {'Content-Type': 'application/json'}

# for AKS deployment you'd need to the service key in the header as well
# api_key = service.get_key()
# headers = {'Content-Type':'application/json',  'Authorization':('Bearer '+ api_key)}

resp = requests.post(service.scoring_uri, input_data, headers=headers)

print("POST to url", service.scoring_uri)
#print("input data:", input_data)
print("label:", y_test[random_index])
print("prediction:", resp.text)
```

## <a name="clean-up-resources"></a>Очистка ресурсов

Чтобы сохранить группу ресурсов и рабочую область для других руководств и исследований, удалите только развертывание службы "Экземпляры контейнеров" с помощью этого вызова API:

```python
service.delete()
```

[!INCLUDE [aml-delete-resource-group](../../../includes/aml-delete-resource-group.md)]


## <a name="next-steps"></a>Дополнительная информация

+ Сведения обо всех [вариантах развертывания для Службы машинного обучения Azure](how-to-deploy-and-where.md).
+ Сведения о [создании клиентов для веб-службы](how-to-consume-web-service.md).
+  Асинхронное [создание прогнозов на больших объемах данных](how-to-run-batch-predictions.md).
+ Мониторинг моделей машинного обучения в Azure с помощью [Application Insights](how-to-enable-app-insights.md).
+ Ознакомьтесь с руководством по [автоматическому выбору алгоритма](tutorial-auto-train-models.md). 
