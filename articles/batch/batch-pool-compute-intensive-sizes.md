---
title: Использование ресурсоемких виртуальных машин Azure в пакетной службе | Документация Майкрософт
description: Узнайте, как воспользоваться преимуществами размеров виртуальных машин HPC (высокопроизводительные вычисления) или GPU (графический процессор) в пулах пакетной службы Azure.
documentationcenter: ''
author: laurenhughes
manager: gwallace
editor: ''
ms.assetid: ''
ms.service: batch
ms.workload: big-compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/17/2018
ms.author: lahugh
ms.openlocfilehash: 687783520b082cdfd1a6ffc91a8641ea35fafd68
ms.sourcegitcommit: 4b431e86e47b6feb8ac6b61487f910c17a55d121
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/18/2019
ms.locfileid: "68323351"
---
# <a name="use-rdma-or-gpu-instances-in-batch-pools"></a>Использование экземпляров RDMA или GPU в пулах пакетной службы

Для выполнения некоторых пакетных заданий можно воспользоваться размерами виртуальных машин Azure, предназначенными для крупномасштабных вычислений. Пример:

* для запуска [рабочих нагрузок MPI](batch-mpi.md) с несколькими экземплярами выберите размеры серии H или другие размеры, имеющие сетевой интерфейс для удаленного доступа к памяти (RDMA). Эти размеры подключаются к сети InfiniBand для обмена данными между узлами, что может ускорить приложения MPI. 

* Для приложений CUDA можно выбрать размеры серии N, которые включают карты графического процессора (GPU) NVIDIA Tesla.

Эта статья содержит рекомендации и примеры для использования некоторых специализированных размеров Azure в пулах пакетной службы. Спецификации и общие сведения см. по следующим ссылкам:

* размеры виртуальных машин, оптимизированных для высокопроизводительных вычислений ([Linux](../virtual-machines/linux/sizes-hpc.md), [Windows](../virtual-machines/windows/sizes-hpc.md)); 

* размеры виртуальных машин с поддержкой графического процессора (GPU) ([Linux](../virtual-machines/linux/sizes-gpu.md), [Windows](../virtual-machines/windows/sizes-gpu.md)). 

> [!NOTE]
> Некоторые размеры виртуальных машин могут быть недоступны в регионах, где вы создаете учетные записи пакетной службы. Чтобы убедиться, что размер доступен, перейдите на страницу [Доступность продуктов по регионам](https://azure.microsoft.com/regions/services/). Кроме того, см. статью, посвященную [выбору размера виртуальной машины для пула пакетной службы](batch-pool-vm-sizes.md).

## <a name="dependencies"></a>Зависимости

Возможности RDMA и GPU размеров для ресурсоемких вычислений в пакетной службе поддерживаются только в некоторых операционных системах. (Список поддерживаемых операционных систем является подмножеством ОС, поддерживаемых для виртуальных машин, созданных в этих размерах.) Вам может потребоваться установить или настроить дополнительный драйвер или другое программное обеспечение на узлах в зависимости от способа создания пула пакетной службы. В указанной ниже таблице представлены сведения об этих зависимостях. Перейдите по ссылке, чтобы просмотреть дополнительные сведения. Варианты настройки пулов пакетной службы см. далее в этой статье.

### <a name="linux-pools---virtual-machine-configuration"></a>Пулы Linux — конфигурация виртуальной машины

| Size | Функция | Операционные системы | Необходимое программное обеспечение | Параметры пула |
| -------- | -------- | ----- |  -------- | ----- |
| [H16r, H16mr, A8, A9](../virtual-machines/linux/sizes-hpc.md#rdma-capable-instances)<br/>[NC24r, NC24rs_v2, NC24rs_v3, ND24rs<sup>*</sup>](../virtual-machines/linux/n-series-driver-setup.md#rdma-network-connectivity) | RDMA | Ubuntu 16.04 LTS или<br/>Экземпляр HPC на платформе CentOS<br/>(Microsoft Azure Marketplace) | Intel MPI 5<br/><br/>Драйверы RDMA Linux | Включение связи между узлами, отключение параллельного выполнения задач |
| [Серия NC, NCv2, NCv3, NDv2](../virtual-machines/linux/n-series-driver-setup.md) | Графический процессор NVIDIA Tesla (зависит от серии) | Ubuntu 16.04 LTS или<br/>CentOS 7.3 или 7.4<br/>(Microsoft Azure Marketplace) | Драйверы NVIDIA CUDA или из набора средств CUDA Toolkit | Н/Д | 
| [Серия NV, NVv2](../virtual-machines/linux/n-series-driver-setup.md) | Графический процессор NVIDIA Tesla M60 | Ubuntu 16.04 LTS или<br/>CentOS 7.3<br/>(Microsoft Azure Marketplace) | Драйверы NVIDIA GRID | Н/Д |

<sup>*</sup>Размеры серии N с поддержкой RDMA также включают графические процессоры NVIDIA Tesla

### <a name="windows-pools---virtual-machine-configuration"></a>Пулы Windows — конфигурация виртуальной машины

| Size | Функция | Операционные системы | Необходимое программное обеспечение | Параметры пула |
| -------- | ------ | -------- | -------- | ----- |
| [H16r, H16mr, A8, A9](../virtual-machines/windows/sizes-hpc.md#rdma-capable-instances)<br/>[NC24r, NC24rs_v2, NC24rs_v3, ND24rs<sup>*</sup>](../virtual-machines/windows/n-series-driver-setup.md#rdma-network-connectivity) | RDMA | Windows Server 2016, 2012 R2 или<br/>2012 (Azure Marketplace) | Microsoft MPI 2012 R2 или более поздней версии либо<br/> Intel MPI 5<br/><br/>Драйверы RDMA Windows | Включение связи между узлами, отключение параллельного выполнения задач |
| [Серия NC, NCv2, NCv3, ND, NDv2](../virtual-machines/windows/n-series-driver-setup.md) | Графический процессор NVIDIA Tesla (зависит от серии) | Windows Server 2016 или <br/>2012 R2 (Azure Marketplace) | Драйверы NVIDIA CUDA или из набора средств CUDA Toolkit| Н/Д | 
| [Серия NV, NVv2](../virtual-machines/windows/n-series-driver-setup.md) | Графический процессор NVIDIA Tesla M60 | Windows Server 2016 или<br/>2012 R2 (Azure Marketplace) | Драйверы NVIDIA GRID | Н/Д |

<sup>*</sup>Размеры серии N с поддержкой RDMA также включают графические процессоры NVIDIA Tesla

### <a name="windows-pools---cloud-services-configuration"></a>Пулы Windows — конфигурация облачных служб

> [!NOTE]
> Размеры серии N не поддерживаются в пулах пакетной службы с использованием конфигурации облачных служб.
>

| Size | Функция | Операционные системы | Необходимое программное обеспечение | Параметры пула |
| -------- | ------- | -------- | -------- | ----- |
| [H16r, H16mr, A8, A9](../virtual-machines/windows/sizes-hpc.md#rdma-capable-instances) | RDMA | Windows Server 2016, 2012 R2, 2012 или<br/>2008 R2 (семейство версий гостевой ОС) | Microsoft MPI 2012 R2 или более поздней версии либо<br/>Intel MPI 5<br/><br/>Драйверы RDMA Windows | Включение связи между узлами,<br/> отключение параллельного выполнения задач |

## <a name="pool-configuration-options"></a>Варианты настройки пула

Для настройки специализированного размера виртуальной машины для пула пакетной службы есть несколько вариантов для установки необходимого программного обеспечения или драйверов.

* Для пулов в конфигурации виртуальной машины выберите стандартный образ виртуальной машины [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/) с предустановленными драйверами и программным обеспечением. Примеры: 

  * [HPC версии 7.4 на базе CentOS](https://azuremarketplace.microsoft.com/marketplace/apps/RogueWave.CentOSbased74HPC?tab=Overview) — включает драйверы RDMA и Intel MPI 5.1;

  * [виртуальная машина для обработки и анализа данных](../machine-learning/data-science-virtual-machine/overview.md) для Linux или Windows — включает драйверы NVIDIA CUDA.

  * Образы Linux для рабочих нагрузок контейнеров пакетной службы, которые также включают драйверы GPU и RDMA:

    * [CentOS (с драйверами GPU и RDMA) для контейнерных пулов пакетной службы Azure](https://azuremarketplace.microsoft.com/marketplace/apps/microsoft-azure-batch.centos-container-rdma?tab=Overview);

    * [Ubuntu Server (с драйверами GPU и RDMA) для контейнерных пулов пакетной службы Azure](https://azuremarketplace.microsoft.com/marketplace/apps/microsoft-azure-batch.ubuntu-server-container-rdma?tab=Overview).

* Создайте [пользовательский образ виртуальной машины Windows или Linux](batch-custom-images.md), содержащий драйверы, программное обеспечение и другие настройки, необходимые для размера виртуальной машины. 

* Создайте [пакет приложения](batch-application-packages.md) пакетной службы на основе драйвера или установщика в формате ZIP и настройте пакетную службу для развертывания пакета на узлах пула и установки после создания каждого узла. Например, если пакет приложения является установщиком, создайте командную строку [задачи запуска](batch-api-basics.md#start-task), чтобы автоматически установить приложение на всех узлах пула. Рекомендуется использовать пакет приложения и задачу запуска пула, если рабочая нагрузка зависит от конкретной версии драйвера.

  > [!NOTE] 
  > Задача запуска должна выполняться с повышенным уровнем разрешений (администратора) и должна дождаться успешного выполнения. Задачи с длительным выполнением приведут к увеличению времени подготовки пула пакетной службы.
  >

* [Batch Shipyard](https://github.com/Azure/batch-shipyard) автоматически настраивает драйверы графического процессора и RDMA для прозрачной работы с контейнерными рабочими нагрузками в пакетной службе Azure. Для выполнения действий Batch Shipyard полностью полагается на файлы конфигурации. Существует множество примеров шаблонов конфигураций, поддерживающих рабочие нагрузки GPU и RDMA, например [шаблон CNTK GPU](https://github.com/Azure/batch-shipyard/tree/master/recipes/CNTK-GPU-OpenMPI), с помощью которого можно предварительно настроить драйверы GPU на виртуальных машинах серии N и загрузить программное обеспечение набора средств Microsoft Cognitive Toolkit как образ Docker.


## <a name="example-nvidia-gpu-drivers-on-windows-nc-vm-pool"></a>Пример: драйверы GPU NVIDIA в пуле виртуальных машин NC Windows

Чтобы запустить приложения CUDA в пуле узлов NC под управлением Windows, необходимо установить драйверы GPU NVDIA. Ниже приведен пример процедуры с использованием пакета приложения для установки драйверов GPU NVIDIA. Вы можете выбрать этот вариант, если рабочая нагрузка зависит от конкретной версии драйвера GPU.

1. Скачайте пакет установки для драйверов GPU в Windows Server 2016 с [веб-сайта NVIDIA](https://www.nvidia.com/Download/index.aspx), например [версию 411.82](https://us.download.nvidia.com/Windows/Quadro_Certified/411.82/411.82-tesla-desktop-winserver2016-international.exe). Сохраните файл локально, используя короткое имя, например *GPUDriverSetup.exe*.
2. Создайте ZIP-файл пакета.
3. Отправьте пакет в учетную запись пакетной службы. Пошаговые инструкции см. в руководстве для [пакетов приложений](batch-application-packages.md). Укажите идентификатор приложения, например *GPUDriver*, и его версию, например *411.82*.
1. Используйте API пакетной службы или портал Azure для создания пула в конфигурации виртуальных машин с нужным количеством узлов и необходимым масштабом. В следующей таблице показаны примеры параметров для автоматической установки драйверов GPU NVIDIA с помощью задачи запуска.

| Параметр | Значение |
| ---- | ----- | 
| **Тип образа** | Marketplace (Linux/Windows) |
| **Издатель** | MicrosoftWindowsServer |
| **ПРЕДЛОЖЕНИЕ** | WindowsServer |
| **SKU** | 2016-Datacenter |
| **Размер узла** | NC6 уровня "Стандартный" |
| **Ссылки на пакет приложения** | GPUDriver, версия 411.82 |
| **Start task enabled** (Включена задача запуска) | True<br>**Командная строка** - `cmd /c "%AZ_BATCH_APP_PACKAGE_GPUDriver#411.82%\\GPUDriverSetup.exe /s"`<br/>**Удостоверение пользователя.** Автоматический пользователь пула, администратор<br/>**Ожидать успешное выполнение.** True

## <a name="example-nvidia-gpu-drivers-on-a-linux-nc-vm-pool"></a>Пример: драйверы GPU NVIDIA в пуле виртуальных машин NC Linux

Чтобы запустить приложения CUDA в пуле узлов NC под управлением Linux, необходимо установить драйверы GPU NVIDIA Tesla из набора средств CUDA Toolkit. Далее приведен пример процедуры по созданию и развертыванию пользовательского образа Ubuntu 16.04 LTS с драйверами графического процессора.

1. Разверните виртуальную машину Azure серии NC под управлением Ubuntu 16.04 LTS. Например, создайте виртуальную машину в центрально-южной части США. 
2. Добавьте [расширение драйверов GPU NVIDIA](../virtual-machines/extensions/hpccompute-gpu-linux.md
) в виртуальную машину с помощью портала Azure, клиентского компьютера, подключенного к подписке Azure, или Azure Cloud Shell. Кроме того, можно выполнить соответствующие инструкции, чтобы подключить виртуальную машину и [установить драйверы CUDA](../virtual-machines/linux/n-series-driver-setup.md) вручную.
3. Выполните инструкции по созданию [моментального снимка и пользовательского образа виртуальной машины Linux](batch-custom-images.md) для пакетной службы.
4. Создайте учетную запись пакетной службы в регионе, поддерживающем виртуальные машины NC.
5. Используйте API пакетной службы или портал Azure для создания пула [с помощью пользовательского образа](batch-custom-images.md) с нужным числом узлов и в нужном масштабе. В следующей таблице приведены примеры параметров пула для образа:

| Параметр | Значение |
| ---- | ---- |
| **Тип образа** | Пользовательский образ |
| **Пользовательский образ** | *Имя образа* |
| **Номер SKU агента узла** | batch.node.ubuntu 16.04 |
| **Размер узла** | NC6 уровня "Стандартный" |

## <a name="example-microsoft-mpi-on-a-windows-h16r-vm-pool"></a>Пример: Microsoft MPI в пуле виртуальных машин H16r Windows

Чтобы выполнять приложения Windows MPI в пуле узлов виртуальных машин H16r в Azure, необходимо настроить расширение HpcVmDrivers и установить [Microsoft MPI](https://docs.microsoft.com/message-passing-interface/microsoft-mpi). Ниже приведен пример процедуры по развертыванию пользовательского образа Windows Server 2016 с необходимыми драйверами и программным обеспечением.

1. Выполните развертывание виртуальной машины H16r Azure под управлением Windows Server 2016. Например, создайте виртуальную машину в западной части США. 
2. Добавьте расширение HpcVmDrivers в виртуальную машину, [выполнив команду Azure PowerShell](../virtual-machines/windows/sizes-hpc.md#rdma-capable-instances
) на клиентском компьютере, который подключен к вашей подписке Azure, или с помощью Azure Cloud Shell. 
1. Откройте подключение к удаленному рабочему столу своей виртуальной машины.
1. Скачайте [пакет установки](https://www.microsoft.com/download/details.aspx?id=57467) (MSMpiSetup.exe) с последней версией Microsoft MPI и установите Microsoft MPI.
1. Выполните инструкции по созданию [моментального снимка и пользовательского образа виртуальной машины Windows](batch-custom-images.md) для пакетной службы.
1. Используйте API пакетной службы или портал Azure для создания пула [с помощью пользовательского образа](batch-custom-images.md) с нужным числом узлов и в нужном масштабе. В следующей таблице приведены примеры параметров пула для образа:

| Параметр | Значение |
| ---- | ---- |
| **Тип образа** | Пользовательский образ |
| **Пользовательский образ** | *Имя образа* |
| **Номер SKU агента узла** | batch.node.windows amd64 |
| **Размер узла** | H16r Standard |
| **Включена связь между узлами** | True |
| **Максимальное число заданий на узел** | 1 |

## <a name="example-intel-mpi-on-a-linux-h16r-vm-pool"></a>Пример: Intel MPI в пуле виртуальных машин H16r Linux

Для выполнения приложений MPI в пуле узлов Linux серии H можно, например, использовать образ [HPC версии 7.4 на базе CentOS](https://azuremarketplace.microsoft.com/marketplace/apps/RogueWave.CentOSbased74HPC?tab=Overview) из Azure Marketplace. Драйверы RDMA Linux и Intel MPI предварительно установлены. Этот образ также поддерживает рабочие нагрузки контейнеров Docker.

Используйте API пакетной службы или портал Azure для создания пула с помощью этого образа с нужным количеством узлов и в нужном масштабе. В следующей таблице приведены примеры параметров пула.

| Параметр | Значение |
| ---- | ---- |
| **Тип образа** | Marketplace (Linux/Windows) |
| **Издатель** | OpenLogic |
| **ПРЕДЛОЖЕНИЕ** | CentOS-HPC |
| **SKU** | 7.4 |
| **Размер узла** | H16r Standard |
| **Включена связь между узлами** | True |
| **Максимальное число заданий на узел** | 1 |

## <a name="next-steps"></a>Следующие шаги

* Для выполнения заданий MPI в пуле пакетной службы Azure ознакомьтесь с примерами для [Windows](batch-mpi.md) или [Linux](https://blogs.technet.microsoft.com/windowshpc/2016/07/20/introducing-mpi-support-for-linux-on-azure-batch/).

* Примеры рабочих нагрузок графического процессора в пакетной службе см. в инструкциях для [Batch Shipyard](https://github.com/Azure/batch-shipyard/).