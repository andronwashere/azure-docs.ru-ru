---
title: Развертывание моделей в ППВМ
titleSuffix: Azure Machine Learning service
description: Узнайте, как развернуть веб-службу с моделью, работающей в FPGA со службой Машинное обучение Azure, для вывода невысокой задержки.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: larryfr
ms.author: tedway
author: tedway
ms.date: 07/25/2019
ms.custom: seodec18
ms.openlocfilehash: cec1a74938690a4f781ea7850fdd6d649550b3eb
ms.sourcegitcommit: 5604661655840c428045eb837fb8704dca811da0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/25/2019
ms.locfileid: "68494909"
---
# <a name="deploy-a-model-as-a-web-service-on-an-fpga-with-azure-machine-learning-service"></a>Развертывание модели как веб-службы в ППВМ с помощью Службы машинного обучения Azure

Вы можете развернуть модель как веб-службу в [поле с программируемыми массивами Gate (FPGA)](concept-accelerate-with-fpgas.md) с машинное обучение Azure модели с аппаратным ускорением. Использование FPGA обеспечивает немалое определение задержки, даже с одним размером пакета. Вывод или оценка модели — это этап, в котором развернутая модель используется для прогнозирования, чаще всего в рабочих данных.

Модели, доступные сегодня:
  - ResNet 50
  - ResNet 152
  - DenseNet-121
  - VGG-16
  - SSD-ВГГ

FPGA доступны в следующих регионах Azure:
  - East US
  - Юго-Восточная Азия
  - Западная Европа
  - Западная часть США 2

> [!IMPORTANT]
> Чтобы оптимизировать задержку и пропускную способность, клиент, отправляющий данные в модель FPGA, должен находиться в одном из регионов выше (то, в котором была развернута модель).

## <a name="prerequisites"></a>предварительные требования

- Подписка Azure.  Если у вас ее нет, создайте бесплатную учетную запись перед началом работы. Опробуйте [бесплатную или платную версию Службы машинного обучения Azure](https://aka.ms/AMLFree).

- Квота FPGA. Используйте Azure CLI, чтобы проверить, есть ли у вас квота:

    ```shell
    az vm list-usage --location "eastus" -o table --query "[?localName=='Standard PBS Family vCPUs']"
    ```

    > [!TIP]
    > Другие возможные расположения: ``southeastasia``, ``westeurope``и ``westus2``.

    Команда возвращает текст, аналогичный приведенному ниже:

    ```text
    CurrentValue    Limit    LocalName
    --------------  -------  -------------------------
    0               6        Standard PBS Family vCPUs
    ```

    Убедитесь, что у вас есть по меньшей мере 6 виртуальных ЦП в __куррентвалуе__.

    Если у вас нет квоты, отправьте запрос по адресу [https://aka.ms/accelerateAI](https://aka.ms/accelerateAI).

- Должны быть установлены рабочая область службы "Машинное обучение Azure" и пакет SDK Машинного обучения Azure для Python. Дополнительные сведения см. [в разделе Создание рабочей области](setup-create-workspace.md).
 
- Пакет SDK для Python для моделей с аппаратным ускорением:

    ```shell
    pip install --upgrade azureml-accel-models
    ```

## <a name="sample-notebooks"></a>Примеры записных книжек

Для удобства [примеры записных книжек](https://aka.ms/aml-accel-models-notebooks) доступны для примера ниже и других примеров.

## <a name="create-and-containerize-your-model"></a>Создание и контейнеризовать модели

В этом документе описывается, как создать граф TensorFlow для предварительной обработки входного образа, сделать его характеризатора с помощью ResNet 50 для FPGA, а затем запустить функции с помощью классификатора, обученного в наборе данных ImageNet.

Следуйте инструкциям по выполнению следующих процедур:

* Определение модели TensorFlow
* Преобразование модели
* Развертывание модели
* Использовать развернутую модель.
* Удалить развернутые службы.

### <a name="load-azure-ml-workspace"></a>Загрузить рабочую область машинного обучения Azure

Загрузите рабочую область машинного обучения Azure.

```python
import os
import tensorflow as tf

from azureml.core import Workspace

ws = Workspace.from_config()
print(ws.name, ws.resource_group, ws.location, ws.subscription_id, sep='\n')
```

### <a name="preprocess-image"></a>Предварительная обработка изображения

Входными данными для веб-службы являются изображения JPEG.  Первым шагом является декодирование изображения JPEG и его предварительная обработка.  Изображения JPEG обрабатываются как строки, а результатом являются десятки, которые будут входными данными для модели ResNet 50.

```python
# Input images as a two-dimensional tensor containing an arbitrary number of images represented a strings
import azureml.accel.models.utils as utils
tf.reset_default_graph()

in_images = tf.placeholder(tf.string)
image_tensors = utils.preprocess_array(in_images)
print(image_tensors.shape)
```

### <a name="load-featurizer"></a>Загрузить характеризатора

Инициализируйте модель и загрузите контрольную точку TensorFlow квантованной версии ResNet50 для использования в качестве характеризатора.  Вы можете заменить "QuantizedResnet50" в приведенном ниже фрагменте кода, импортировав другие глубокие нейронные сети:

- QuantizedResnet152
- QuantizedVgg16
- Densenet121

```python
from azureml.accel.models import QuantizedResnet50
save_path = os.path.expanduser('~/models')
model_graph = QuantizedResnet50(save_path, is_frozen=True)
feature_tensor = model_graph.import_graph_def(image_tensors)
print(model_graph.version)
print(feature_tensor.name)
print(feature_tensor.shape)
```

### <a name="add-classifier"></a>Добавить классификатор

Этот классификатор обучен с помощью набора данных ImageNet.  Примеры для перемещения обучения и обучения с помощью настраиваемых весов доступны в наборе [примеров записных книжек](https://aka.ms/aml-notebooks).

```python
classifier_output = model_graph.get_default_classifier(feature_tensor)
print(classifier_output)
```

### <a name="save-the-model"></a>Сохранение модели

Теперь, когда препроцессор, ResNet 50 характеризатора и классификатор загружены, сохраните граф и связанные с ним переменные в качестве модели.

```python
model_name = "resnet50"
model_save_path = os.path.join(save_path, model_name)
print("Saving model in {}".format(model_save_path))

with tf.Session() as sess:
    model_graph.restore_weights(sess)
    tf.saved_model.simple_save(sess, model_save_path,
                               inputs={'images': in_images},
                               outputs={'output_alias': classifier_output})
```

### <a name="save-input-and-output-tensors"></a>Сохранение входных и выходных десятков
Входные и выходные десятки, созданные во время предварительной обработки и выполнения шагов классификатора, необходимы для преобразования и вывода модели.

```python
input_tensors = in_images.name
output_tensors = classifier_output.name

print(input_tensors)
print(output_tensors)
```

> [!IMPORTANT]
> Сохраните входные и выходные десятки, так как они понадобятся для преобразования модели и запросов вывода.

Доступны модели и соответствующие дополнительные десятки выходных данных классификатора по умолчанию, которые используются для вывода при использовании классификатора по умолчанию.

+ Resnet50, QuantizedResnet50
  ```python
  output_tensors = "classifier_1/resnet_v1_50/predictions/Softmax:0"
  ```
+ Resnet152, QuantizedResnet152
  ```python
  output_tensors = "classifier/resnet_v1_152/predictions/Softmax:0"
  ```
+ Densenet121, QuantizedDensenet121
  ```python
  output_tensors = "classifier/densenet121/predictions/Softmax:0"
  ```
+ Vgg16, QuantizedVgg16
  ```python
  output_tensors = "classifier/vgg_16/fc8/squeezed:0"
  ```
+ Ссдвгг, Куантизедссдвгг
  ```python
  output_tensors = ['ssd_300_vgg/block4_box/Reshape_1:0', 'ssd_300_vgg/block7_box/Reshape_1:0', 'ssd_300_vgg/block8_box/Reshape_1:0', 'ssd_300_vgg/block9_box/Reshape_1:0', 'ssd_300_vgg/block10_box/Reshape_1:0', 'ssd_300_vgg/block11_box/Reshape_1:0', 'ssd_300_vgg/block4_box/Reshape:0', 'ssd_300_vgg/block7_box/Reshape:0', 'ssd_300_vgg/block8_box/Reshape:0', 'ssd_300_vgg/block9_box/Reshape:0', 'ssd_300_vgg/block10_box/Reshape:0', 'ssd_300_vgg/block11_box/Reshape:0']
  ```

### <a name="register-model"></a>Регистрация модели

[Зарегистрируйте](./concept-model-management-and-deployment.md) созданную модель.  Добавление тегов и других метаданных о модели помогает контролировать обученные модели.

```python
from azureml.core.model import Model

registered_model = Model.register(workspace=ws,
                                  model_path=model_save_path,
                                  model_name=model_name)

print("Successfully registered: ", registered_model.name,
      registered_model.description, registered_model.version, sep='\t')
```

Если вы уже зарегистрировали модель и хотите загрузить ее, вы можете получить ее.

```python
from azureml.core.model import Model
model_name = "resnet50"
# By default, the latest version is retrieved. You can specify the version, i.e. version=1
registered_model = Model(ws, name="resnet50")
print(registered_model.name, registered_model.description,
      registered_model.version, sep='\t')
```

### <a name="convert-model"></a>Преобразование модели

Преобразуйте граф TensorFlow в формат открытого нейронного сетевого обмена ([ONNX](https://onnx.ai/)).  Вам потребуется указать имена десятков входных и выходных данных, и эти имена будут использоваться клиентом при использовании веб-службы.

```python
from azureml.accel import AccelOnnxConverter

convert_request = AccelOnnxConverter.convert_tf_model(
    ws, registered_model, input_tensors, output_tensors)

# If it fails, you can run wait_for_completion again with show_output=True.
convert_request.wait_for_completion(show_output=False)

# If the above call succeeded, get the converted model
converted_model = convert_request.result
print("\nSuccessfully converted: ", converted_model.name, converted_model.url, converted_model.version,
      converted_model.id, converted_model.created_time, '\n')
```

### <a name="create-docker-image"></a>Создание образа DOCKER

Преобразованная модель и все зависимости добавляются в образ DOCKER.  После этого образ DOCKER можно развернуть и создать экземпляр.  Поддерживаемые целевые объекты развертывания включают AKS в облаке или пограничном устройстве, например [Azure Data Box Edge](https://docs.microsoft.com/azure/databox-online/data-box-edge-overview).  Вы также можете добавить теги и описания для зарегистрированного образа DOCKER.

```python
from azureml.core.image import Image
from azureml.accel import AccelContainerImage

image_config = AccelContainerImage.image_configuration()
# Image name must be lowercase
image_name = "{}-image".format(model_name)

image = Image.create(name=image_name,
                     models=[converted_model],
                     image_config=image_config,
                     workspace=ws)
image.wait_for_creation(show_output=False)
```

Выведите список образов по тегу и получите подробные журналы для любой отладки.

```python
for i in Image.list(workspace=ws):
    print('{}(v.{} [{}]) stored at {} with build log {}'.format(
        i.name, i.version, i.creation_state, i.image_location, i.image_build_log_uri))
```

## <a name="model-deployment"></a>Развертывание модели

### <a name="deploy-to-the-cloud"></a>Развертывание в облаке

Для развертывания модели в качестве крупномасштабной рабочей веб-службы используется Служба Azure Kubernetes (AKS). Создать новую можно с помощью Машинное обучение Azure пакета SDK, интерфейса командной строки или портал Azure.

```python
from azureml.core.compute import AksCompute, ComputeTarget

# Specify the Standard_PB6s Azure VM
prov_config = AksCompute.provisioning_configuration(vm_size="Standard_PB6s",
                                                    agent_count=1)

aks_name = 'my-aks-cluster'
# Create the cluster
aks_target = ComputeTarget.create(workspace=ws,
                                  name=aks_name,
                                  provisioning_configuration=prov_config)
```

Развертывание AKS может занять около 15 минут.  Проверьте, прошло ли развертывание.

```python
aks_target.wait_for_completion(show_output=True)
print(aks_target.provisioning_state)
print(aks_target.provisioning_errors)
```

Разверните контейнер в кластере AKS.
```python
from azureml.core.webservice import Webservice, AksWebservice

# For this deployment, set the web service configuration without enabling auto-scaling or authentication for testing
aks_config = AksWebservice.deploy_configuration(autoscale_enabled=False,
                                                num_replicas=1,
                                                auth_enabled=False)

aks_service_name = 'my-aks-service'

aks_service = Webservice.deploy_from_image(workspace=ws,
                                           name=aks_service_name,
                                           image=image,
                                           deployment_config=aks_config,
                                           deployment_target=aks_target)
aks_service.wait_for_deployment(show_output=True)
```

#### <a name="test-the-cloud-service"></a>Тестирование облачной службы
Образ DOCKER поддерживает gRPC и TensorFlow, обслуживающий API-интерфейс "Predict".  Используйте пример клиента для вызова образа DOCKER, чтобы получить прогнозы из модели.  Пример кода клиента доступен:
- [Python](https://github.com/Azure/aml-real-time-ai/blob/master/pythonlib/amlrealtimeai/client.py)
- [C#](https://github.com/Azure/aml-real-time-ai/blob/master/sample-clients/csharp)

Если вы хотите использовать TensorFlow, можно [скачать пример клиента](https://www.tensorflow.org/serving/setup).

```python
# Using the grpc client in Azure ML Accelerated Models SDK package
from azureml.accel import PredictionClient

address = aks_service.scoring_uri
ssl_enabled = address.startswith("https")
address = address[address.find('/')+2:].strip('/')
port = 443 if ssl_enabled else 80

# Initialize AzureML Accelerated Models client
client = PredictionClient(address=address,
                          port=port,
                          use_ssl=ssl_enabled,
                          service_name=aks_service.name)
```

Поскольку этот классификатор был обучен в наборе данных [ImageNet](http://www.image-net.org/) , сопоставьте классы с легко читаемыми метками.

```python
import requests
classes_entries = requests.get(
    "https://raw.githubusercontent.com/Lasagne/Recipes/master/examples/resnet50/imagenet_classes.txt").text.splitlines()

# Score image with input and output tensor names
results = client.score_file(path="./snowleopardgaze.jpg",
                            input_name=input_tensors,
                            outputs=output_tensors)

# map results [class_id] => [confidence]
results = enumerate(results)
# sort results by confidence
sorted_results = sorted(results, key=lambda x: x[1], reverse=True)
# print top 5 results
for top in sorted_results[:5]:
    print(classes_entries[top[0]], 'confidence:', top[1])
```

### <a name="clean-up-the-service"></a>Очистка службы
Удалите веб-службу, образ и модель (необходимо выполнить в этом порядке, так как существуют зависимости).

```python
aks_service.delete()
aks_target.delete()
image.delete()
registered_model.delete()
converted_model.delete()
```

## <a name="deploy-to-a-local-edge-server"></a>Развертывание на локальном пограничном сервере

Все [Azure Data Box Edge устройства](https://docs.microsoft.com/azure/databox-online/data-box-edge-overview
) содержат FPGA для запуска модели.  Только одна модель может быть запущена в FPGA за один раз.  Чтобы запустить другую модель, просто разверните новый контейнер. Инструкции и примеры кода можно найти в [этом примере Azure](https://github.com/Azure-Samples/aml-hardware-accelerated-models).

## <a name="secure-fpga-web-services"></a>Защита веб-служб FPGA

Сведения о защите веб-служб FPGA см. в статье [Использование SSL для защиты веб-служб, развернутых с помощью Службы машинного обучения Azure](how-to-secure-web-service.md).

## <a name="pbs-family-vms"></a>Виртуальные машины семейства PBS

Семейство виртуальных машин Azure содержит Intel Арриа 10 FPGA.  При проверке выделения квоты Azure она будет отображаться как "Стандартная гарнитура виртуальных ЦП".  Виртуальная машина PB6 имеет шесть виртуальных ЦП и одно FPGA, и оно будет автоматически подготовлено Azure ML в процессе развертывания модели в FPGA.  Он используется только с МАШИНным обучением Azure и не может выполнять произвольный битстреамс.  Например, вы не сможете зафлэшировать FPGA с битстреамс для шифрования, кодирования и т. д. 
