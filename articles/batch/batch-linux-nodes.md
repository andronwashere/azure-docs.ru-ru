---
title: Выполнение вычислительных узлов виртуальных машин под управлением Linux в пакетной службе Azure | Документация Майкрософт
description: Узнайте, как обрабатывать параллельные вычислительные рабочие нагрузки в пулах виртуальных машин Linux в пакетной службе Azure.
services: batch
documentationcenter: python
author: laurenhughes
manager: jeconnoc
editor: ''
ms.assetid: dc6ba151-1718-468a-b455-2da549225ab2
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: na
ms.date: 06/01/2018
ms.author: lahugh
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 10a3c5a4f1c6eaceecb9dc5262d8694ee4265b48
ms.sourcegitcommit: a12b2c2599134e32a910921861d4805e21320159
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/24/2019
ms.locfileid: "67340178"
---
# <a name="provision-linux-compute-nodes-in-batch-pools"></a>Подготовка вычислительных узлов Linux в пулах пакетной службы

Пакетная служба Azure позволяет выполнять параллельные вычислительные рабочие нагрузки на виртуальных машинах Linux и Windows. В этой статье подробно описывается создание пулов вычислительных узлов Linux в пакетной службе с помощью [Python для пакетной службы][py_batch_package] and [Batch .NET][api_net] клиентских библиотек.

> [!NOTE]
> Пакеты приложений поддерживаются во всех пулах пакетной службы, созданных после 5 июля 2017 г. Если пул создан с помощью конфигурации облачной службы, пакеты приложений также поддерживаются в пулах пакетной службы, созданных между 10 марта 2016 г. и 5 июля 2017 г. Пулы пакетной службы, созданные до 10 марта 2016 г., не поддерживают пакеты приложений. Дополнительные сведения о развертывании приложений на узлах пакетной службы с помощью пакетов приложений см .в [этой статье](batch-application-packages.md).
>
>

## <a name="virtual-machine-configuration"></a>Конфигурация виртуальной машины
При создании пула вычислительных узлов в пакетной службе есть два варианта для выбора размера узла и операционной системы: Cloud Services Configuration (Конфигурация облачных служб) и "Конфигурация виртуальной машины".

**Cloud Services Configuration** (Конфигурация облачных служб) предоставляет *только*вычислительные узлы Windows. Доступные размеры вычислительных узлов перечислены в статье [Размеры для облачных служб](../cloud-services/cloud-services-sizes-specs.md), а доступные операционные системы — в статье [Таблица совместимости выпусков гостевых ОС Azure и пакетов SDK](../cloud-services/cloud-services-guestos-update-matrix.md). При создании пула, содержащего узлы облачных служб Azure, необходимо указать размер узла и "семейство ОС", которые описаны в упомянутых ранее статьях. Для пулов вычислительных узлов Windows чаще всего используются облачные службы.

**Virtual Machine Configuration** предоставляет образы Windows и Linux для вычислительных узлов. Доступные размеры вычислительных узлов перечислены в статьях [Размеры виртуальных машин в Azure](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) (Linux) и [Размеры виртуальных машин в Azure](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) (Windows). При создании пула, содержащего узлы конфигурации виртуальных машин, необходимо указать размер узлов, ссылку на образ виртуальной машины и номер SKU агента узла пакетной службы для установки на узлах.

### <a name="virtual-machine-image-reference"></a>Ссылка на образ виртуальной машины
Для предоставления вычислительных узлов в конфигурации виртуальной машины пакетная служба использует [масштабируемые наборы виртуальных машин](../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md). Можно указать образ из [Azure Marketplace][vm_marketplace], или укажите пользовательский образ, который вы подготовили. Дополнительные сведения о пользовательских образах см. в статье [Использование управляемого пользовательского образа для создания пула виртуальных машин](batch-custom-images.md).

При настройке ссылки на образ виртуальной машины задаются свойства образа виртуальной машины. Приведенные ниже свойства являются обязательными при создании ссылки на образ виртуальной машины.

| **Свойства ссылки на образ** | **Пример** |
| --- | --- |
| Издатель |Canonical |
| ПРЕДЛОЖЕНИЕ |UbuntuServer |
| SKU |14.04.4-LTS |
| Version |latest |

> [!TIP]
> Дополнительные сведения об этих свойствах и о том, как получить список образов из Marketplace, см. в статье [Выбор образов виртуальных машин Linux с помощью интерфейса командной строки Azure (Azure CLI)](../virtual-machines/linux/cli-ps-findimage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Обратите внимание, что не все образы из Marketplace в настоящее время совместимы с пакетной службой. Дополнительные сведения см. в разделе [Номер SKU агента узла](#node-agent-sku).
>
>

### <a name="node-agent-sku"></a>Номер SKU агента узла
Агент узла пакетной службы — это программа, которая выполняется на каждом узле в пуле и предоставляет интерфейс контроля и управления между узлом и пакетной службой. Существуют различные реализации агента узла, известные как номера SKU, для различных операционных систем. По существу, при создании конфигурации виртуальной машины сначала следует указать ссылку на образ виртуальной машины, а затем указать агент узла, который будет установлен на этом образе. Как правило, каждый номер SKU агента узла совместим с несколькими образами виртуальных машин. Вот несколько примеров номеров SKU агентов узлов:

* batch.node.ubuntu 14.04;
* batch.node.centos 7
* batch.node.windows amd64

> [!IMPORTANT]
> Не все образы виртуальных машин, доступные в Marketplace, совместимы с доступными на данный момент агентами узлов пакетной службы. Для получения списка доступных номеров SKU агентов узлов и образов виртуальных машин, с которыми они совместимы, используйте пакеты SDK пакетной службы. Дополнительные сведения и примеры получения списка допустимых образов в среде выполнения см. далее в этой статье в разделе [Список образов виртуальных машин](#list-of-virtual-machine-images).
>
>

## <a name="create-a-linux-pool-batch-python"></a>Создание пула Linux. Python для пакетной службы
В следующем фрагменте кода показан пример использования [клиентской библиотеки пакетной службы Microsoft Azure для Python][py_batch_package] to create a pool of Ubuntu Server compute nodes. Reference documentation for the Batch Python module can be found at [azure.batch package][py_batch_docs] на Read the Docs.

Этот фрагмент создает [ImageReference][py_imagereference] explicitly and specifies each of its properties (publisher, offer, SKU, version). In production code, however, we recommend that you use the [list_node_agent_skus][py_list_skus] метод для определения и выбора доступных сочетаний образа и узла агента SKU во время выполнения.

```python
# Import the required modules from the
# Azure Batch Client Library for Python
import azure.batch.batch_service_client as batch
import azure.batch.batch_auth as batchauth
import azure.batch.models as batchmodels

# Specify Batch account credentials
account = "<batch-account-name>"
key = "<batch-account-key>"
batch_url = "<batch-account-url>"

# Pool settings
pool_id = "LinuxNodesSamplePoolPython"
vm_size = "STANDARD_A1"
node_count = 1

# Initialize the Batch client
creds = batchauth.SharedKeyCredentials(account, key)
config = batch.BatchServiceClientConfiguration(creds, batch_url)
client = batch.BatchServiceClient(creds, batch_url)

# Create the unbound pool
new_pool = batchmodels.PoolAddParameter(id=pool_id, vm_size=vm_size)
new_pool.target_dedicated = node_count

# Configure the start task for the pool
start_task = batchmodels.StartTask()
start_task.run_elevated = True
start_task.command_line = "printenv AZ_BATCH_NODE_STARTUP_DIR"
new_pool.start_task = start_task

# Create an ImageReference which specifies the Marketplace
# virtual machine image to install on the nodes.
ir = batchmodels.ImageReference(
    publisher="Canonical",
    offer="UbuntuServer",
    sku="14.04.2-LTS",
    version="latest")

# Create the VirtualMachineConfiguration, specifying
# the VM image reference and the Batch node agent to
# be installed on the node.
vmc = batchmodels.VirtualMachineConfiguration(
    image_reference=ir,
    node_agent_sku_id="batch.node.ubuntu 14.04")

# Assign the virtual machine configuration to the pool
new_pool.virtual_machine_configuration = vmc

# Create pool in the Batch service
client.pool.add(new_pool)
```

Как упоминалось ранее, рекомендуется вместо создания [ImageReference][py_imagereference] explicitly, you use the [list_node_agent_skus][py_list_skus] метод для динамического выбора из поддерживаемых в настоящее время сочетаний агента узла/Marketplace образ. Следующий фрагмент кода Python демонстрирует, как использовать этот метод.

```python
# Get the list of node agents from the Batch service
nodeagents = client.account.list_node_agent_skus()

# Obtain the desired node agent
ubuntu1404agent = next(
    agent for agent in nodeagents if "ubuntu 14.04" in agent.id)

# Pick the first image reference from the list of verified references
ir = ubuntu1404agent.verified_image_references[0]

# Create the VirtualMachineConfiguration, specifying the VM image
# reference and the Batch node agent to be installed on the node.
vmc = batchmodels.VirtualMachineConfiguration(
    image_reference=ir,
    node_agent_sku_id=ubuntu1404agent.id)
```

## <a name="create-a-linux-pool-batch-net"></a>Создание пула Linux. .NET для пакетной службы
В следующем фрагменте кода показан пример использования [.NET для пакетной службы][nuget_batch_net] client library to create a pool of Ubuntu Server compute nodes. You can find the [Batch .NET reference documentation][api_net] на сайте docs.microsoft.com.

В следующем фрагменте кода используется [PoolOperations][net_pool_ops] .[ListNodeAgentSkus][net_list_skus] метод для выбора из списка, в настоящее время поддерживается Marketplace сочетаний образа и узла агента SKU. Этот способ является предпочтительным, так как список поддерживаемых сочетаний может меняться время от времени. Чаще всего добавляются поддерживаемые сочетания.

```csharp
// Pool settings
const string poolId = "LinuxNodesSamplePoolDotNet";
const string vmSize = "STANDARD_A1";
const int nodeCount = 1;

// Obtain a collection of all available node agent SKUs.
// This allows us to select from a list of supported
// VM image/node agent combinations.
List<NodeAgentSku> nodeAgentSkus =
    batchClient.PoolOperations.ListNodeAgentSkus().ToList();

// Define a delegate specifying properties of the VM image
// that we wish to use.
Func<ImageReference, bool> isUbuntu1404 = imageRef =>
    imageRef.Publisher == "Canonical" &&
    imageRef.Offer == "UbuntuServer" &&
    imageRef.Sku.Contains("14.04");

// Obtain the first node agent SKU in the collection that matches
// Ubuntu Server 14.04. Note that there are one or more image
// references associated with this node agent SKU.
NodeAgentSku ubuntuAgentSku = nodeAgentSkus.First(sku =>
    sku.VerifiedImageReferences.Any(isUbuntu1404));

// Select an ImageReference from those available for node agent.
ImageReference imageReference =
    ubuntuAgentSku.VerifiedImageReferences.First(isUbuntu1404);

// Create the VirtualMachineConfiguration for use when actually
// creating the pool
VirtualMachineConfiguration virtualMachineConfiguration =
    new VirtualMachineConfiguration(imageReference, ubuntuAgentSku.Id);

// Create the unbound pool object using the VirtualMachineConfiguration
// created above
CloudPool pool = batchClient.PoolOperations.CreatePool(
    poolId: poolId,
    virtualMachineSize: vmSize,
    virtualMachineConfiguration: virtualMachineConfiguration,
    targetDedicatedComputeNodes: nodeCount);

// Commit the pool to the Batch service
await pool.CommitAsync();
```

Несмотря на то, что в приведенном ранее фрагменте используется [PoolOperations][net_pool_ops] .[ListNodeAgentSkus][net_list_skus] метод для динамического перечисления и выбора поддерживаемых сочетаний образа и узла агента SKU (рекомендуется), можно также настроить [ Объект ImageReference][net_imagereference] явным образом:

```csharp
ImageReference imageReference = new ImageReference(
    publisher: "Canonical",
    offer: "UbuntuServer",
    sku: "14.04.2-LTS",
    version: "latest");
```

## <a name="list-of-virtual-machine-images"></a>Список образов виртуальных машин
В следующей таблице указаны образы виртуальных машин из Marketplace, которые совместимы с доступными агентами узлов пакетной службы на момент написания этой статьи. Важно отметить, что этот список не является окончательным, так как в любое время образы и агенты узлов могут добавляться или удаляться. Мы рекомендуем приложений пакетной службы и службы всегда использовать [list_node_agent_skus][py_list_skus] (Python) or [ListNodeAgentSkus][net_list_skus] (.NET пакетной службы) для определения и выбора в настоящее время доступных номеров SKU.

> [!WARNING]
> Следующий список может меняться в любое время. При выполнении заданий пакетной службы следует всегда использовать методы **получения списка номеров SKU агентов узлов**, доступные в интерфейсах API пакетной службы, для перечисления совместимых виртуальных машин и номеров SKU агентов узлов.
>
>

| **Издатель** | **ПРЕДЛОЖЕНИЕ** | **Номер SKU образа** | **Версия** | **Идентификатор SKU агента узла** |
| ------------- | --------- | ------------- | ----------- | --------------------- |
| пакет (1) | rendering-centos73 | rendering | latest | batch.node.centos 7 |
| пакет (1) | rendering-windows2016 | rendering | latest | batch.node.windows amd64 |
| Canonical | UbuntuServer | 16.04-LTS | latest | batch.node.ubuntu 16.04 |
| Canonical | UbuntuServer | 14.04.5-LTS | latest | batch.node.ubuntu 14.04 |
| Credativ | Debian | 9 | latest | batch.node.debian 9 |
| Credativ | Debian | 8 | latest | batch.node.debian 8 |
| microsoft-ads | linux-data-science-vm | linuxdsvm | latest | batch.node.centos 7 |
| microsoft-ads | standard-data-science-vm | standard-data-science-vm | latest | batch.node.windows amd64 |
| microsoft-azure-batch | centos-container | 7-4 | latest | batch.node.centos 7 |
| microsoft-azure-batch | centos-container-rdma | 7-4 | latest | batch.node.centos 7 |
| microsoft-azure-batch | ubuntu-server-container | 16-04-lts | latest | batch.node.ubuntu 16.04 |
| microsoft-azure-batch | ubuntu-server-container-rdma | 16-04-lts | latest | batch.node.ubuntu 16.04 |
| MicrosoftWindowsServer | WindowsServer | 2016-Datacenter | latest | batch.node.windows amd64 |
| MicrosoftWindowsServer | WindowsServer | 2016-Datacenter-smalldisk | latest | batch.node.windows amd64 |
| MicrosoftWindowsServer | WindowsServer | 2016-Datacenter-with-Containers | latest | batch.node.windows amd64 |
| MicrosoftWindowsServer | WindowsServer | 2012-R2-Datacenter | latest | batch.node.windows amd64 |
| MicrosoftWindowsServer | WindowsServer | 2012-R2-Datacenter-smalldisk | latest | batch.node.windows amd64 |
| MicrosoftWindowsServer | WindowsServer | 2012-Datacenter | latest | batch.node.windows amd64 |
| MicrosoftWindowsServer | WindowsServer | 2012-Datacenter-smalldisk | latest | batch.node.windows amd64 |
| MicrosoftWindowsServer | WindowsServer | 2008-R2-SP1 | latest | batch.node.windows amd64 |
| MicrosoftWindowsServer | WindowsServer | 2008-R2-SP1-smalldisk | latest | batch.node.windows amd64 |
| OpenLogic | CentOS | 7.4 | latest | batch.node.centos 7 |
| OpenLogic | CentOS-HPC | 7.4 | latest | batch.node.centos 7 |
| OpenLogic | CentOS-HPC | 7.3 | latest | batch.node.centos 7 |
| OpenLogic | CentOS-HPC | 7.1 | latest | batch.node.centos 7 |
| Oracle | Oracle-Linux | 7.4 | latest | batch.node.centos 7 |
| SUSE | SLES-HPC | 12-SP2 | latest | batch.node.opensuse 42.1 |

## <a name="connect-to-linux-nodes-using-ssh"></a>Подключение к узлам Linux с помощью SSH
Во время разработки или устранения неполадок может потребоваться войти на узлы в пуле. В отличие от вычислительных узлов Windows, для подключения к узлам Linux нельзя использовать протокол удаленного рабочего стола (RDP). Вместо этого пакетная служба включает доступ по протоколу SSH на каждом узле для удаленного подключения.

В следующем фрагменте кода Python создается пользователь на каждом узле пула, который необходим для удаленного подключения. Затем выводятся сведения о подключении SSH для каждого узла.

```python
import datetime
import getpass
import azure.batch.batch_service_client as batch
import azure.batch.batch_auth as batchauth
import azure.batch.models as batchmodels

# Specify your own account credentials
batch_account_name = ''
batch_account_key = ''
batch_account_url = ''

# Specify the ID of an existing pool containing Linux nodes
# currently in the 'idle' state
pool_id = ''

# Specify the username and prompt for a password
username = 'linuxuser'
password = getpass.getpass()

# Create a BatchClient
credentials = batchauth.SharedKeyCredentials(
    batch_account_name,
    batch_account_key
)
batch_client = batch.BatchServiceClient(
    credentials,
    base_url=batch_account_url
)

# Create the user that will be added to each node in the pool
user = batchmodels.ComputeNodeUser(username)
user.password = password
user.is_admin = True
user.expiry_time = \
    (datetime.datetime.today() + datetime.timedelta(days=30)).isoformat()

# Get the list of nodes in the pool
nodes = batch_client.compute_node.list(pool_id)

# Add the user to each node in the pool and print
# the connection information for the node
for node in nodes:
    # Add the user to the node
    batch_client.compute_node.add_user(pool_id, node.id, user)

    # Obtain SSH login information for the node
    login = batch_client.compute_node.get_remote_login_settings(pool_id,
                                                                node.id)

    # Print the connection info for the node
    print("{0} | {1} | {2} | {3}".format(node.id,
                                         node.state,
                                         login.remote_login_ip_address,
                                         login.remote_login_port))
```

Ниже приведен пример выходных данных вышеупомянутого кода для пула, содержащего четыре узла Linux.

```
Password:
tvm-1219235766_1-20160414t192511z | ComputeNodeState.idle | 13.91.7.57 | 50000
tvm-1219235766_2-20160414t192511z | ComputeNodeState.idle | 13.91.7.57 | 50003
tvm-1219235766_3-20160414t192511z | ComputeNodeState.idle | 13.91.7.57 | 50002
tvm-1219235766_4-20160414t192511z | ComputeNodeState.idle | 13.91.7.57 | 50001
```

При создании пользователя на узле вместо пароля можно указать открытый ключ SSH. В пакет SDK для Python используйте **ssh_public_key** параметр на [ComputeNodeUser][py_computenodeuser]. In .NET, use the [ComputeNodeUser][net_computenodeuser].[ SshPublicKey][net_ssh_key] свойство.

## <a name="pricing"></a>Цены
Пакетная служба Azure основана на технологии виртуальных машин Azure и облачных служб Azure. Сама пакетная служба предоставляется бесплатно. Это означает, что плата взимается только за вычислительные ресурсы, используемые решениями пакетной службы. При выборе **конфигурация облачных служб**, плата взимается на основе [цен на облачные службы][cloud_services_pricing] структуры. При выборе **конфигурации виртуальной машины**, плата взимается на основе [цены на виртуальные машины][vm_pricing] структуры. 

При развертывании приложений на узлах пакетной службы с помощью [пакетов приложений](batch-application-packages.md) также взимается плата за ресурсы службы хранилища Azure, используемые пакетами приложений. Как правило, эти затраты на службу хранилища Azure минимальны. 

## <a name="next-steps"></a>Дальнейшие действия

[Примеры кода Python][github_samples_py] in the [azure-batch-samples][github_samples] репозитории на сайте GitHub содержат сценарии, в которых показано, как выполнять распространенные пакетные операции, такие как создание пула, задания и задачи. [Файл сведений][github_py_readme], прилагаемый к примерам кода Python, содержит подробные сведения об установке необходимых пакетов.

[api_net]: https://msdn.microsoft.com/library/azure/mt348682.aspx
[api_net_mgmt]: https://msdn.microsoft.com/library/azure/mt463120.aspx
[api_rest]: https://msdn.microsoft.com/library/azure/dn820158.aspx
[cloud_services_pricing]: https://azure.microsoft.com/pricing/details/cloud-services/
[github_py_readme]: https://github.com/Azure/azure-batch-samples/blob/master/Python/Batch/README.md
[github_samples]: https://github.com/Azure/azure-batch-samples
[github_samples_py]: https://github.com/Azure/azure-batch-samples/tree/master/Python/Batch
[github_samples_pyclient]: https://github.com/Azure/azure-batch-samples/blob/master/Python/Batch/article_samples/python_tutorial_client.py
[portal]: https://portal.azure.com
[net_cloudpool]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.aspx
[net_computenodeuser]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenodeuser.aspx
[net_imagereference]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.imagereference.aspx
[net_list_skus]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.listnodeagentskus.aspx
[net_pool_ops]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.aspx
[net_ssh_key]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenodeuser.sshpublickey.aspx
[nuget_batch_net]: https://www.nuget.org/packages/Microsoft.Azure.Batch/
[rest_add_pool]: https://msdn.microsoft.com/library/azure/dn820174.aspx
[py_account_ops]: http://azure-sdk-for-python.readthedocs.org/en/dev/ref/azure.batch.operations.html#azure.batch.operations.AccountOperations
[py_azure_sdk]: https://pypi.python.org/pypi/azure
[py_batch_docs]: https://azure-sdk-for-python.readthedocs.io/batch.html
[py_batch_package]: https://pypi.python.org/pypi/azure-batch
[py_computenodeuser]: https://docs.microsoft.com/python/api/azure.batch.models.computenodeuser
[py_imagereference]: https://docs.microsoft.com/python/api/azure.mgmt.batch.models.imagereference
[py_list_skus]: https://docs.microsoft.com/python/api/azure-batch/azure.batch.operations.AccountOperations?view=azure-python
[vm_marketplace]: https://azure.microsoft.com/marketplace/virtual-machines/
[vm_pricing]: https://azure.microsoft.com/pricing/details/virtual-machines/
