---
title: Перенос виртуальных машин в хранилище Azure класса Premium | Документация Майкрософт
description: Перенесите существующие виртуальные машины в хранилище Azure класса Premium. Хранилище Premium обеспечивает поддержку дисков с высокой производительностью и малой задержкой для интенсивных рабочих нагрузок ввода-вывода на виртуальных машинах Azure.
services: storage
author: roygara
ms.service: storage
ms.topic: article
ms.date: 06/27/2017
ms.author: rogarana
ms.reviewer: yuemlu
ms.subservice: common
ms.openlocfilehash: 6b6e442ff3333a7fd085f8e452ae056e7daaba8c
ms.sourcegitcommit: d2785f020e134c3680ca1c8500aa2c0211aa1e24
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/04/2019
ms.locfileid: "67565515"
---
# <a name="migrating-to-azure-premium-storage-unmanaged-disks"></a>Перенос в хранилище Azure класса Premium (использующее неуправляемые диски)

> [!NOTE]
> В этой статье описывается, как перенести виртуальную машину, использующую неуправляемые диски уровня "Стандартный", в виртуальную машину, использующую неуправляемые диски уровня "Премиум". Для новых виртуальных машин мы рекомендуем использовать Управляемые диски Azure и преобразовать неуправляемые диски в управляемые. Вам не нужно обрабатывать базовые учетные записи хранения. Управляемые диски сделают это за вас. Дополнительные сведения см. в статье [Managed Disks Overview](../../virtual-machines/windows/managed-disks-overview.md) (Обзор Управляемых дисков).
>

Хранилище Azure Premium обеспечивает поддержку дисков с высокой производительностью и малой задержкой для виртуальных машин с интенсивной рабочей нагрузкой ввода-вывода. Диски виртуальной машины приложения можно перенести в хранилище Azure класса Premium, чтобы использовать их быстродействие и производительность.

Это руководство призвано помочь новым пользователям службы хранилища Azure класса Premium подготовиться к плавному переходу из своей текущей системы к хранилищу класса Premium. В руководстве рассматриваются три ключевых компонента процесса:

* [планирование переноса в хранилище класса Premium](#plan-the-migration-to-premium-storage);
* [подготовка и копирование виртуальных жестких дисков (VHD) в хранилище класса Premium](#prepare-and-copy-virtual-hard-disks-VHDs-to-premium-storage);
* [создание виртуальной машины Azure c использованием хранилища класса Premium](#create-azure-virtual-machine-using-premium-storage).

Виртуальные машины можно перенести в хранилище класса Premium с других платформ. Кроме того, имеющиеся виртуальные машины Azure можно перенести из хранилища Azure класса Standard в хранилище класса Premium. В этом руководстве рассматриваются оба этих сценария. В зависимости от используемого сценария выполняйте инструкции, описанные в соответствующем разделе.

> [!NOTE]
> Обзор возможностей и цен на диски SSD (цен. категория "Премиум") см. здесь: [What disk types are available in Azure?](../../virtual-machines/windows/disks-types.md#premium-ssd) (Какие типы дисков доступны в Azure?). Для оптимальной работы приложения рекомендуем переносить все диски виртуальных машин, требующие большого числа операций ввода-вывода в секунду, в хранилище Azure класса Premium. Если диск не требует большого числа операций ввода-вывода в секунду, выберите более доступное хранилище уровня Standard — в этом случае данные с диска виртуальной машины будут храниться не на твердотельных, а на жестких дисках.
>

Для полного завершения процесса переноса могут потребоваться дополнительные действия, предваряющие описанные в этом руководстве инструкции и следующие за ними. Например, может потребоваться настроить виртуальные сети или конечные точки, а также внести изменения в код самого приложения, для чего может потребоваться простой приложения. Эти действия будут отличаться для каждого приложения. Чтобы максимально плавно полностью перейти к хранилищу класса Premium, их следует выполнять в комплексе с инструкциями этого руководства.

## <a name="plan-the-migration-to-premium-storage"></a>Планирование переноса в хранилище класса Premium
Сведения в этом разделе позволят вам подготовить среду к выполнению инструкций по переносу, описанных в этой статье, а также определить оптимальные типы виртуальных машин и дисков.

### <a name="prerequisites"></a>Технические условия
* Вам понадобится подписка Azure. Если у вас нет подписки, можно оформить [бесплатную пробную](https://azure.microsoft.com/pricing/free-trial/) подписку на один месяц или посетить страницу [Цены Azure](https://azure.microsoft.com/pricing/), чтобы ознакомиться с дополнительными возможностями.
* Чтобы выполнять командлеты PowerShell, вам потребуется модуль Microsoft Azure PowerShell. Инструкции по установке см. в статье [Установка и настройка Azure PowerShell](/powershell/azure/overview).
* Если вы планируете использовать виртуальные машины Azure в хранилище класса Premium, эти машины должны поддерживать его. С этими виртуальными машинами можно использовать диски хранилища класса Premium и Standard. Диски хранилища Premium в будущем будут доступны с большим количеством типов виртуальных машин. Дополнительные сведения о всех доступных типах и размерах дисков виртуальной машины Azure см. в разделах [Размеры виртуальных машин](../../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) и [Размеры для облачных служб](../../cloud-services/cloud-services-sizes-specs.md).

### <a name="considerations"></a>Рекомендации
Виртуальные машины Azure поддерживают подключение нескольких дисков хранилища уровня "Премиум", за счет чего приложения могут использовать до 256 ТБ емкости на каждой виртуальной машине. С хранилищем Premium вы получите выполнение до 80 000 операций ввода-вывода в секунду на виртуальную машину и пропускную способность второго диска до 2000 МБ в секунду на виртуальную машину с очень низкой задержкой при операциях чтения. Вы можете выбирать виртуальные машины и диски среди множества различных вариантов. Сведения в этом разделе помогут выбрать оптимальный вариант для вашей рабочей нагрузки.

#### <a name="vm-sizes"></a>Размеры виртуальной машины
Спецификации размеров виртуальных машин Azure перечислены в статье [Размеры виртуальных машин](../../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Просмотрите характеристики виртуальных машин, которые работают с хранилищем класса Premium, и выберите размер виртуальной машины, наиболее подходящий для вашей рабочей нагрузки. Убедитесь, что виртуальная машина имеет достаточную пропускную способность для поддержки трафика диска.

#### <a name="disk-sizes"></a>Размеры диска
Существуют пять типов дисков, которые можно использовать с виртуальной машиной. Для каждого из них действуют определенные ограничения на количество операций ввода-вывода в секунду и пропускную способность. Учитывайте эти ограничения при выборе типа диска для вашей виртуальной машины в зависимости от потребностей приложения с точки зрения емкости, производительности, масштабируемости и пиковых нагрузок.

| Диски уровня "Премиум"  | P10   | P20   | P30            | P40            | P50            | 
|:-------------------:|:-----:|:-----:|:--------------:|:--------------:|:--------------:|
| Размер диска           | 128 ГБ| 512 ГБ| 1024 ГБ (1 ТБ) | 2048 ГБ (2 ТБ) | 4095 ГБ (4 ТБ) | 
| Количество операций ввода-вывода в секунду на диск       | 500   | 2300  | 5000           | 7500           | 7500           | 
| Пропускная способность на диск | 100 МБ в секунду | 150 МБ в секунду | 200 МБ в секунду | 250 МБ в секунду | 250 МБ в секунду |

В зависимости от рабочей нагрузки определите, требуются ли дополнительные диски данных для виртуальной машины. К виртуальной машине можно подключить несколько дисков постоянных данных. Если необходимо увеличить емкость и производительность тома, вы можете создать на дисках чередующийся том. (Дополнительные сведения о чередовании дисков см. [здесь](../../virtual-machines/windows/premium-storage-performance.md#disk-striping).) Если вы создаете чередующийся том на дисках данных хранилища класса "Премиум" с помощью [дисковых пространств][4], необходимо настроить по одному столбцу для каждого используемого диска. В противном случае из-за неравномерного распределения трафика между дисками общая производительность чередующегося тома может быть ниже ожидаемой. Выполнить эту же операцию на виртуальных машинах с Linux можно с помощью служебной программы *mdadm*. Дополнительные сведения см. в статье [Настройка программного RAID-массива в Linux](../../virtual-machines/linux/configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

#### <a name="storage-account-scalability-targets"></a>Целевые показатели масштабируемости для учетной записи хранения
У учетных записей хранения класса Premium есть следующие целевые показатели масштабируемости в дополнение к [целевым показателям масштабируемости и производительности службы хранилища Azure](storage-scalability-targets.md). Если потребности вашего приложения превышают целевые показатели по масштабируемости для отдельной учетной записи хранения, при создании настройте приложение на использование нескольких учетных записей хранения и распределите данные между этими учетными записями.

| Общая емкость учетной записи | Общая пропускная способность для учетной записи локально избыточного хранилища |
|:--- |:--- |
| Емкость диска: 35 ТБ<br />Емкость для моментальных снимков: 10 ТБ |До 50 гигабит в секунду для входящих и исходящих подключений |

Дополнительные сведения о характеристиках хранилища класса Premium см. в статье о [целевых показателях масштабируемости и производительности хранилища](storage-scalability-targets.md#premium-performance-storage-account-scale-limits).

#### <a name="disk-caching-policy"></a>Политика кэширования дисков
По умолчанию для политики кэширования дисков задано значение *только чтение* для всех дисков с данными "Премиум" и *чтение и запись* для дисков операционной системы "Премиум", подключенных к виртуальной машине. Этот параметр конфигурации рекомендуется использовать, чтобы достичь оптимальной производительности при операциях ввода-вывода приложения. Отключите кэширование дисков данных, которые отличаются высокой интенсивностью записи или используются только для записи (например, файлов журналов SQL Server), чтобы улучшить производительность приложения. Параметры кэша для существующих дисков данных можно обновить с помощью [портала Azure](https://portal.azure.com) или *- HostCaching* параметр *Set-AzureDataDisk* командлета.

#### <a name="location"></a>Location
Выберите расположение, где доступно хранилище Azure Premium. Актуальные сведения о доступных расположениях см. на странице [Доступность продуктов по регионам](https://azure.microsoft.com/regions/#services). Виртуальные машины, расположенные в одном регионе с учетной записью хранения, где хранятся диски для виртуальной машины, обеспечивают более высокую производительность, чем находящиеся в других регионах.

#### <a name="other-azure-vm-configuration-settings"></a>Прочие параметры конфигурации виртуальной машины Azure
При создании виртуальной машины Azure появится запрос на настройку некоторых ее параметров. Помните, что одни параметры фиксируются на все время существования виртуальной машины, а другие можно изменить или добавить позже. Просмотрите эти параметры конфигурации виртуальной машины Azure и убедитесь, что они настроены правильно в соответствии с требованиями вашей рабочей нагрузки.

### <a name="optimization"></a>Оптимизация
В статье [Хранилище Azure класса Premium: разработка, обеспечивающая повышенную производительность](../../virtual-machines/windows/premium-storage-performance.md) приведены рекомендации по разработке высокопроизводительных приложений с использованием хранилища Azure класса Premium. Эти рекомендации можно использовать вместе с рекомендациями по производительности, применимыми к технологиям, используемым приложением.

## <a name="prepare-and-copy-virtual-hard-disks-VHDs-to-premium-storage"></a>Подготовка VHD и их копирование в хранилище класса Premium
В следующем разделе приведены рекомендации по подготовке VHD на виртуальной машине и их копированию в службу хранилища Azure.

* [Сценарий 1. Перенос существующих виртуальных машин Azure в хранилище Azure класса Premium.](#scenario1)
* [Сценарий 2. Перенос виртуальных машин в хранилище Azure класса Premium с других платформ.](#scenario2)

### <a name="prerequisites"></a>Технические условия
Чтобы подготовить VHD к переносу, требуется следующее:

* Подписка Azure, учетная запись хранения и контейнер в этой учетной записи хранения, куда будет скопирован VHD. Обратите внимание, что целевой учетной записью хранения может быть учетная запись хранения Standard или Premium в зависимости от требований.
* Средство для обобщения VHD, если на его основе планируется создать несколько экземпляров виртуальной машины. Например, sysprep для Windows или virt-sysprep для Ubuntu.
* Средство загрузки VHD-файла в учетную запись хранения. См. раздел [Передача данных с помощью служебной программы командной строки AzCopy](storage-use-azcopy.md) или воспользуйтесь [обозревателем хранилищ Azure](https://blogs.msdn.com/b/windowsazurestorage/archive/2014/03/11/windows-azure-storage-explorers-2014.aspx). В этом руководстве описано копирование виртуального жесткого диска с помощью средства AzCopy.

> [!NOTE]
> Чтобы добиться оптимальной производительности при синхронном копировании с помощью AzCopy, скопируйте VHD, запустив одно из средств на виртуальной машине Azure, которая принадлежит к тому же региону, что и целевая учетная запись хранения. При копировании виртуального жесткого диска из виртуальной машины Azure в другом регионе производительность может быть ниже.
>
> Для копирования большого объема данных при ограниченной пропускной способности можно [использовать службу импорта и экспорта Azure для передачи данных в хранилище BLOB-объектов](../storage-import-export-service.md). Это позволит перенести данные, доставив жесткие диски в центр обработки данных Azure. С помощью службы импорта и экспорта данных Azure можно скопировать данные только в учетную запись хранения Standard. После переноса данных в учетную запись хранения категории "Стандартный" их можно передать в учетную запись хранилища класса "Премиум" с помощью [API-копирования BLOB-объектов](https://msdn.microsoft.com/library/azure/dd894037.aspx) или AzCopy.
>
> Обратите внимание, что Microsoft Azure поддерживает только файлы виртуальных жестких дисков фиксированного размера. VHDX-файлы или динамические виртуальные жесткие диски не поддерживаются. При наличии динамический виртуальный жесткий диск можно преобразовать в диск фиксированного размера с помощью командлета [Convert-VHD](https://technet.microsoft.com/library/hh848454.aspx) .
>
>

### <a name="scenario1"></a>Сценарий 1. Перенос существующих виртуальных машин Azure в хранилище Azure класса Premium.
При переносе имеющихся виртуальных машин Azure остановите работу виртуальной машины, подготовьте VHD в зависимости от выбранного типа, а затем скопируйте диск с помощью AzCopy или PowerShell.

Чтобы виртуальная машина перешла в исходное состояние, она должна быть полностью выключена. Простой будет длиться до завершения переноса.

#### <a name="step-1-prepare-vhds-for-migration"></a>Шаг 1. Подготовка VHD к переносу
При переносе имеющихся виртуальных машин Azure в хранилище класса Premium VHD может представлять собой:

* обобщенный образ операционной системы;
* уникальный диск операционной системы;
* диск данных.

Ниже рассмотрены три соответствующих сценария подготовки VHD.

##### <a name="use-a-generalized-operating-system-vhd-to-create-multiple-vm-instances"></a>Использование обобщенного VHD операционной системы для создания нескольких экземпляров виртуальной машины
При загрузке виртуального жесткого диска, который будет использоваться для создания нескольких экземпляров универсальной ВМ Azure, необходимо сначала произвести обобщение виртуального жесткого диска с помощью программы sysprep. Это применимо для локального или находящегося в облаке виртуального жесткого диска. Служебная программа sysprep удаляет все сведения о компьютере с VHD.

> [!IMPORTANT]
> Создайте моментальный снимок или резервную копию виртуальной машины перед ее обобщением. После запуска sysprep останавливается работа экземпляра виртуальной машины и отменяется его выделение. Выполните описанные ниже шаги, чтобы запустить sysprep для виртуального жесткого диска операционной системы Windows. Обратите внимание, что для запуска команды Sysprep потребуется завершить работу виртуальной машины. Дополнительные сведения о команде Sysprep см. в разделах [Обзор Sysprep](https://technet.microsoft.com/library/hh825209.aspx) или [Технический справочник по Sysprep](https://technet.microsoft.com/library/cc766049.aspx).
>
>

1. Откройте окно командной строки с правами администратора.
2. Введите следующую команду, чтобы открыть Sysrep:

    ```
    %windir%\system32\sysprep\sysprep.exe
    ```

3. В средстве подготовки системы выберите "Ввод системной автоматизации OOBE", установите флажок "Обобщение", выберите **Завершение работы** и затем нажмите **ОК**, как показано на рисунке ниже. Служебная программа sysprep обобщит операционную систему и завершит ее работу.

    ![][1]

Для виртуальной машины Ubuntu используйте virt-sysrep для достижения этой же цели. Дополнительные сведения см. в разделе [virt-sysprep](https://manpages.ubuntu.com/manpages/precise/man1/virt-sysprep.1.html). См. также прочее [программное обеспечение для подготовки серверов Linux](https://www.cyberciti.biz/tips/server-provisioning-software.html) с открытым кодом при использовании других операционных систем Linux.

##### <a name="use-a-unique-operating-system-vhd-to-create-a-single-vm-instance"></a>Использование уникального VHD операционной системы для создания одного экземпляра виртуальной машины
При наличии приложения, которое работает на виртуальной машине и для которого требуются данные конкретного компьютера, не обобщайте виртуальный жесткий диск. Необобщенный виртуальный жесткий диск может использоваться для создания уникального экземпляра виртуальной машины Azure. Например, при наличии контроллера домена на вашем виртуальном жестком диске выполнение команды sysprep сделает его неэффективным в качестве контроллера домена. Просмотрите приложения, выполняющиеся на виртуальной машине, а также влияние sysprep на них, прежде чем обобщить VHD.

##### <a name="register-data-disk-vhd"></a>Регистрация VHD в качестве диска данных
При наличии подлежащих переносу дисков с данными необходимо завершить работу виртуальных машин, использующих эти диски данных.

Выполните шаги ниже, чтобы скопировать VHD в хранилище Azure класса Premium и зарегистрировать диск в качестве подготовленного диска данных.

#### <a name="step-2-create-the-destination-for-your-vhd"></a>Шаг 2. Создание целевого объекта для виртуального жесткого диска
Создайте учетную запись хранения для обслуживания виртуальных жестких дисков. При планировании места хранения VHD учитывайте следующие аспекты.

* У целевой учетной записи хранения должен быть класс Premium.
* Расположение учетной записи хранения должно совпадать с расположением виртуальных машин Azure хранилища класса Premium, которые будут созданы на последнем этапе. Можно скопировать в новую учетную запись хранения или запланировать использование прежней учетной записи в зависимости от требований.
* Скопируйте и сохраните ключ целевой учетной записи хранения для следующего этапа.

В учетной записи хранения класса Standard можно хранить некоторые диски данных (например, диски, данные которых используются не так часто), но мы настоятельно рекомендуем перенести все данные рабочей нагрузки в хранилище класса Premium.

#### <a name="copy-vhd-with-azcopy-or-powershell"></a>Шаг 3. Копирование VHD с помощью AzCopy или PowerShell
Чтобы скопировать VHD с помощью одного из этих средств, вам понадобится путь контейнера и ключ учетной записи хранения (выберите **Портал Azure** > **Хранилище**, чтобы узнать их). URL-адрес будет иметь следующий вид контейнера «https:\//myaccount.blob.core.windows.net/mycontainer/».

##### <a name="option-1-copy-a-vhd-with-azcopy-asynchronous-copy"></a>Вариант 1: Копирование VHD с помощью AzCopy (асинхронное копирование)
Средство AzCopy позволяет легко передать VHD через Интернет. В зависимости от размера виртуальных жестких дисков это может занять определенное время. Не забывайте проверять ограничения для исходящих и входящих данных учетной записи хранения при использовании этого параметра. Дополнительные сведения см. в статье [Целевые показатели производительности и масштабируемости службы хранилища Azure](storage-scalability-targets.md).

1. Загрузите и установите AzCopy для Windows здесь: [последняя версия AzCopy](https://aka.ms/downloadazcopy).
2. Откройте Azure PowerShell и перейдите в папку, где установлена AzCopy.
3. Используйте следующую команду для копирования файла виртуального жесткого диска из "Источника" в "Место назначения".

    ```azcopy
    AzCopy /Source: <source> /SourceKey: <source-account-key> /Dest: <destination> /DestKey: <dest-account-key> /BlobType:page /Pattern: <file-name>
    ```

    Пример:

    ```azcopy
    AzCopy /Source:https://sourceaccount.blob.core.windows.net/mycontainer1 /SourceKey:key1 /Dest:https://destaccount.blob.core.windows.net/mycontainer2 /DestKey:key2 /Pattern:abc.vhd
        ```

    Here are descriptions of the parameters used in the AzCopy command:

   * **/Source: _&lt;source&gt;:_** Location of the folder or storage container URL that contains the VHD.
   * **/SourceKey: _&lt;source-account-key&gt;:_** Storage account key of the source storage account.
   * **/Dest: _&lt;destination&gt;:_** Storage container URL to copy the VHD to.
   * **/DestKey: _&lt;dest-account-key&gt;:_** Storage account key of the destination storage account.
   * **/Pattern: _&lt;file-name&gt;:_** Specify the file name of the VHD to copy.

For details on using AzCopy tool, see [Transfer data with the AzCopy Command-Line Utility](storage-use-azcopy.md).

##### Option 2: Copy a VHD with PowerShell (Synchronized copy)

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

You can also copy the VHD file using the PowerShell cmdlet Start-AzStorageBlobCopy. Use the following command on Azure PowerShell to copy VHD. Replace the values in <> with corresponding values from your source and destination storage account. To use this command, you must have a container called vhds in your destination storage account. If the container doesn't exist, create one before running the command.

```powershell
$sourceBlobUri = <source-vhd-uri>

$sourceContext = New-AzStorageContext  –StorageAccountName <source-account> -StorageAccountKey <source-account-key>

$destinationContext = New-AzStorageContext  –StorageAccountName <dest-account> -StorageAccountKey <dest-account-key>

Start-AzStorageBlobCopy -srcUri $sourceBlobUri -SrcContext $sourceContext -DestContainer <dest-container> -DestBlob <dest-disk-name> -DestContext $destinationContext
```

Пример:

```powershell
C:\PS> $sourceBlobUri = "https://sourceaccount.blob.core.windows.net/vhds/myvhd.vhd"

C:\PS> $sourceContext = New-AzStorageContext  –StorageAccountName "sourceaccount" -StorageAccountKey "J4zUI9T5b8gvHohkiRg"

C:\PS> $destinationContext = New-AzStorageContext  –StorageAccountName "destaccount" -StorageAccountKey "XZTmqSGKUYFSh7zB5"

C:\PS> Start-AzStorageBlobCopy -srcUri $sourceBlobUri -SrcContext $sourceContext -DestContainer "vhds" -DestBlob "myvhd.vhd" -DestContext $destinationContext
```

### <a name="scenario2"></a>Сценарий 2. Перенос виртуальных машин в хранилище Azure класса Premium с других платформ.
При переносе виртуального жесткого диска из облачного хранилища, не относящегося к Azure, в хранилище Azure необходимо сначала экспортировать виртуальный жесткий диск в локальный каталог. Вручную скопируйте полный исходный путь к локальному каталогу, где хранится VHD, а затем передайте этот диск в службу хранилища Azure с помощью AzCopy.

#### <a name="step-1-export-vhd-to-a-local-directory"></a>Шаг 1. Экспорт VHD в локальный каталог
##### <a name="copy-a-vhd-from-aws"></a>Копирование VHD из AWS
1. При использовании AWS экспортируйте экземпляр EC2 на виртуальный жесткий диск в сегмент Amazon S3. Выполните действия, описанные в документации по Amazon в разделе об экспорте экземпляров Amazon EC2, чтобы установить интерфейс командной строки Amazon EC2, а также выполнить команду create-instance-export-task и экспортировать экземпляр EC2 в файл VHD. При выполнении команды **create-instance-export-task** обязательно используйте **VHD** для переменной DISK&#95;IMAGE&#95;FORMAT. Экспортированный файл виртуального жесткого диска сохраняется в сегменте Amazon S3, назначенном во время этого процесса.

    ```
    aws ec2 create-instance-export-task --instance-id ID --target-environment TARGET_ENVIRONMENT \
      --export-to-s3-task DiskImageFormat=DISK_IMAGE_FORMAT,ContainerFormat=ova,S3Bucket=BUCKET,S3Prefix=PREFIX
    ```

2. Загрузите VHD-файл из контейнеров S3. Выберите "Файл виртуального жесткого диска", а затем команду **Действия** > **Загрузка**.

    ![][3]

##### <a name="copy-a-vhd-from-other-non-azure-cloud"></a>Копирование VHD из облака (вне Azure)
При переносе виртуального жесткого диска из облачного хранилища, не относящегося к Azure, в хранилище Azure необходимо сначала экспортировать виртуальный жесткий диск в локальный каталог. Скопируйте полный исходный путь к локальному каталогу, где хранится VHD.

##### <a name="copy-a-vhd-from-on-premises"></a>Копирование VHD из локальной среды
При переносе VHD из локальной среды необходимо ввести полный исходный путь его хранения. Это может быть адрес сервера или общей папки.

#### <a name="step-2-create-the-destination-for-your-vhd"></a>Шаг 2. Создание целевого объекта для виртуального жесткого диска
Создайте учетную запись хранения для обслуживания виртуальных жестких дисков. При планировании места хранения VHD учитывайте следующие аспекты.

* Целевой учетной записью хранения может быть хранилище Standard или Premium в зависимости от требований приложения.
* Регион учетной записи хранения должен совпадать с регионом виртуальных машин Azure хранилища класса Premium, которые будут созданы на последнем этапе. Можно скопировать в новую учетную запись хранения или запланировать использование прежней учетной записи в зависимости от требований.
* Скопируйте и сохраните ключ целевой учетной записи хранения для следующего этапа.

Мы настоятельно рекомендуем перенести все данные рабочей нагрузки в хранилище класса Premium.

#### <a name="step-3-upload-the-vhd-to-azure-storage"></a>Шаг 3. Загрузка VHD в службу хранилища Azure
Теперь, когда VHD расположен в локальном каталоге, с помощью AzCopy или Azure PowerShell VHD-файл можно передать в службу хранилища Azure. Это можно сделать двумя способами.

##### <a name="option-1-using-azure-powershell-add-azurevhd-to-upload-the-vhd-file"></a>Вариант 1: Передача VHD-файла с помощью командлета Azure PowerShell Add-AzureVhd

```powershell
Add-AzureVhd [-Destination] <Uri> [-LocalFilePath] <FileInfo>
```

Пример \<Uri > может быть  ** _"https://storagesample.blob.core.windows.net/mycontainer/blob1.vhd"_** . Пример \<FileInfo > может быть  ** _«C:\path\to\upload.vhd»_** .

##### <a name="option-2-using-azcopy-to-upload-the-vhd-file"></a>Вариант 2. Передача VHD-файла с помощью AzCopy
Средство AzCopy позволяет легко передать VHD через Интернет. В зависимости от размера виртуальных жестких дисков это может занять определенное время. Не забывайте проверять ограничения для исходящих и входящих данных учетной записи хранения при использовании этого параметра. Дополнительные сведения см. в статье [Целевые показатели производительности и масштабируемости службы хранилища Azure](storage-scalability-targets.md).

1. Загрузите и установите AzCopy для Windows здесь: [последняя версия AzCopy](https://aka.ms/downloadazcopy).
2. Откройте Azure PowerShell и перейдите в папку, где установлена AzCopy.
3. Используйте следующую команду для копирования файла виртуального жесткого диска из "Источника" в "Место назначения".

    ```azcopy
    AzCopy /Source: <source> /SourceKey: <source-account-key> /Dest: <destination> /DestKey: <dest-account-key> /BlobType:page /Pattern: <file-name>
    ```

    Пример:

    ```azcopy
    AzCopy /Source:https://sourceaccount.blob.core.windows.net/mycontainer1 /SourceKey:key1 /Dest:https://destaccount.blob.core.windows.net/mycontainer2 /DestKey:key2 /BlobType:page /Pattern:abc.vhd
        ```

    Here are descriptions of the parameters used in the AzCopy command:

   * **/Source: _&lt;source&gt;:_** Location of the folder or storage container URL that contains the VHD.
   * **/SourceKey: _&lt;source-account-key&gt;:_** Storage account key of the source storage account.
   * **/Dest: _&lt;destination&gt;:_** Storage container URL to copy the VHD to.
   * **/DestKey: _&lt;dest-account-key&gt;:_** Storage account key of the destination storage account.
   * **/BlobType: page:** Specifies that the destination is a page blob.
   * **/Pattern: _&lt;file-name&gt;:_** Specify the file name of the VHD to copy.

For details on using AzCopy tool, see [Transfer data with the AzCopy Command-Line Utility](storage-use-azcopy.md).

##### Other options for uploading a VHD
You can also upload a VHD to your storage account using one of the following means:

* [Azure Storage Copy Blob API](https://msdn.microsoft.com/library/azure/dd894037.aspx)
* [Azure Storage Explorer Uploading Blobs](https://azurestorageexplorer.codeplex.com/)
* [Storage Import/Export Service REST API Reference](https://msdn.microsoft.com/library/dn529096.aspx)

> [!NOTE]
> We recommend using Import/Export Service if estimated uploading time is longer than 7 days. You can use [DataTransferSpeedCalculator](https://github.com/Azure-Samples/storage-dotnet-import-export-job-management/blob/master/DataTransferSpeedCalculator.html) to estimate the time from data size and transfer unit.
>
> Import/Export can be used to copy to a standard storage account. You will need to copy from standard storage to premium storage account using a tool like AzCopy.
>
>

## <a name="create-azure-virtual-machine-using-premium-storage"></a>Create Azure VMs using Premium Storage
After the VHD is uploaded or copied to the desired storage account, follow the instructions in this section to register the VHD as an OS image, or OS disk depending on your scenario and then create a VM instance from it. The data disk VHD can be attached to the VM once it is created.
A sample migration script is provided at the end of this section. This simple script does not match all scenarios. You may need to update the script to match with your specific scenario. To see if this script applies to your scenario, see below [A Sample Migration Script](#a-sample-migration-script).

### Checklist
1. Wait until all the VHD disks copying is complete.
2. Make sure Premium Storage is available in the region you are migrating to.
3. Decide the new VM series you will be using. It should be a Premium Storage capable, and the size should be depending on the availability in the region and based on your needs.
4. Decide the exact VM size you will use. VM size needs to be large enough to support the number of data disks you have. E.g. if you have 4 data disks, the VM must have 2 or more cores. Also, consider processing power, memory and network bandwidth needs.
5. Create a Premium Storage account in the target region. This is the account you will use for the new VM.
6. Have the current VM details handy, including the list of disks and corresponding VHD blobs.

Prepare your application for downtime. To do a clean migration, you have to stop all the processing in the current system. Only then you can get it to consistent state which you can migrate to the new platform. Downtime duration will depend on the amount of data in the disks to migrate.

> [!NOTE]
> If you are creating an Azure Resource Manager VM from a specialized VHD Disk, please refer to [this template](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-specialized-vhd) for deploying Resource Manager VM using existing disk.
>
>

### Register your VHD
To create a VM from OS VHD or to attach a data disk to a new VM, you must first register them. Follow steps below depending on your VHD's scenario.

#### Generalized Operating System VHD to create multiple Azure VM instances
After generalized OS image VHD is uploaded to the storage account, register it as an **Azure VM Image** so that you can create one or more VM instances from it. Use the following PowerShell cmdlets to register your VHD as an Azure VM OS image. Provide the complete container URL where VHD was copied to.

```powershell
Add-AzureVMImage -ImageName "OSImageName" -MediaLocation "https://storageaccount.blob.core.windows.net/vhdcontainer/osimage.vhd" -OS Windows
```

Скопируйте и сохраните имя этого нового образа виртуальной машины Azure. В приведенном выше примере это *OSImageName*.

#### <a name="unique-operating-system-vhd-to-create-a-single-azure-vm-instance"></a>Уникальный виртуальный жесткий диск операционной системы для создания одного экземпляра виртуальной машины Azure
После загрузки уникального VHD операционной системы в учетную запись хранилища зарегистрируйте его как **диск ОС Azure**, чтобы на его основе можно было создать один или несколько экземпляров виртуальной машины. Используйте эти командлеты PowerShell для регистрации вашего виртуального жесткого диска в качестве диска операционной системы Azure. Укажите полный URL-адрес контейнера, в который был скопирован виртуальный жесткий диск.

```powershell
Add-AzureDisk -DiskName "OSDisk" -MediaLocation "https://storageaccount.blob.core.windows.net/vhdcontainer/osdisk.vhd" -Label "My OS Disk" -OS "Windows"
```

Скопируйте и сохраните имя этого нового образа операционной системы Azure. В приведенном выше примере это *OSDisk*.

#### <a name="data-disk-vhd-to-be-attached-to-new-azure-vm-instances"></a>Присоединение VHD к новым экземплярам виртуальной машины Azure
Загрузив виртуальный жесткий диск данных в учетную запись хранения, зарегистрируйте его как диск данных Azure. Это позволит вам присоединить его к новому экземпляру виртуальной машины Azure серии DS, DSv2 или GS.

Используйте эти командлеты PowerShell для регистрации вашего виртуального жесткого диска в качестве диска данных Azure. Укажите полный URL-адрес контейнера, в который был скопирован виртуальный жесткий диск.

```powershell
Add-AzureDisk -DiskName "DataDisk" -MediaLocation "https://storageaccount.blob.core.windows.net/vhdcontainer/datadisk.vhd" -Label "My Data Disk"
```

Скопируйте и сохраните имя этого нового диска данных Azure. В приведенном выше примере это *DataDisk*.

### <a name="create-a-premium-storage-capable-vm"></a>Создание виртуальной машины, поддерживающей хранилище класса Premium
После регистрации образа ОС или диска ОС можно создать виртуальную машину серии DS, DSv2 или GS. Будет использовано зарегистрированное имя образа или диска операционной системы. Выберите тип виртуальной машины из уровня хранилища Premium. В приведенном ниже примере мы используем размер виртуальной машины *Standard_DS2*.

> [!NOTE]
> Обновите размер диска, чтобы обеспечить соответствие требованиям к емкости и производительности, а также доступным размерам Azure.
>
>

Пошагово выполните нижеприведенные командлеты PowerShell для создания новой виртуальной машины. Сначала задайте общие параметры.

```powershell
$serviceName = "yourVM"
$location = "location-name" (e.g., West US)
$vmSize ="Standard_DS2"
$adminUser = "youradmin"
$adminPassword = "yourpassword"
$vmName ="yourVM"
$vmSize = "Standard_DS2"
```

Сначала создайте облачную службу, в которой будут размещаться новые виртуальные машины.

```powershell
New-AzureService -ServiceName $serviceName -Location $location
```

Затем в зависимости от сценария создайте экземпляр виртуальной машины Azure из зарегистрированного образа или диска ОС.

#### <a name="generalized-operating-system-vhd-to-create-multiple-azure-vm-instances"></a>Обобщенный виртуальный жесткий диск операционной системы для создания нескольких экземпляров виртуальной машины Azure
Создайте один или несколько экземпляров виртуальной машины Azure серии DS с помощью зарегистрированного **образа ОС Azure** . Укажите имя этого образа ОС в конфигурации виртуальной машины при создании новой виртуальной машины, как показано ниже.

```powershell
$OSImage = Get-AzureVMImage –ImageName "OSImageName"

$vm = New-AzureVMConfig -Name $vmName –InstanceSize $vmSize -ImageName $OSImage.ImageName

Add-AzureProvisioningConfig -Windows –AdminUserName $adminUser -Password $adminPassword –VM $vm

New-AzureVM -ServiceName $serviceName -VM $vm
```

#### <a name="unique-operating-system-vhd-to-create-a-single-azure-vm-instance"></a>Уникальный виртуальный жесткий диск операционной системы для создания одного экземпляра виртуальной машины Azure
Создайте новый экземпляр виртуальной машины Azure серии DS с помощью зарегистрированного **диска ОС Azure** . Укажите имя этого диска ОС в конфигурации виртуальной машины при создании новой виртуальной машины, как показано ниже.

```powershell
$OSDisk = Get-AzureDisk –DiskName "OSDisk"

$vm = New-AzureVMConfig -Name $vmName -InstanceSize $vmSize -DiskName $OSDisk.DiskName

New-AzureVM -ServiceName $serviceName –VM $vm
```

Укажите другие сведения о виртуальной машине Azure, например: облачную службу, регион, учетную запись хранения, группу доступности и политику кэширования. Обратите внимание, что экземпляр виртуальной машины должен быть связан с операционной системой или дисками данных. Следовательно, выбранная облачная служба, регион и учетная запись хранения должны находиться в том же расположении, что и базовые VHD этих дисков.

### <a name="attach-data-disk"></a>Присоединение диска данных
Наконец, зарегистрировав VHD в качестве дисков данных, присоедините их к новой виртуальной машине Azure, поддерживающей хранилище класса Premium.

Используйте следующий командлет PowerShell, чтобы присоединить диск данных к новой виртуальной машине и указать политику кэширования. В примере ниже для политики кэширования установлено значение *ReadOnly*.

```powershell
$vm = Get-AzureVM -ServiceName $serviceName -Name $vmName

Add-AzureDataDisk -ImportFrom -DiskName "DataDisk" -LUN 0 –HostCaching ReadOnly –VM $vm

Update-AzureVM  -VM $vm
```

> [!NOTE]
> Чтобы обеспечить поддержку приложений, может потребоваться выполнить дополнительные шаги, не описанные в этом руководстве.
>
>

### <a name="checking-and-plan-backup"></a>Проверка и планирование резервного копирования
После запуска новой виртуальной машины войдите на нее, используя идентификатор входа и пароль исходной виртуальной машины, и убедитесь, что все работает правильно. Все настройки, в том числе чередующиеся тома, будут присутствовать в новой виртуальной машине.

Последний шаг заключается в планировании резервного копирования и расписания обслуживания для новой виртуальной машины в соответствии с требованиями приложения.

### <a name="a-sample-migration-script"></a>Пример скрипта переноса
Автоматизировать перенос нескольких виртуальных машин можно с помощью сценариев PowerShell. Ниже приведен пример сценария, который автоматизирует процесс переноса виртуальной машины. Обратите внимание, что этот скрипт приведен в качестве примера, что предполагает некоторые допущения в отношении дисков текущей виртуальной машины. Вам может понадобиться обновить скрипт в соответствии с ситуацией.

Допускается следующее:

* Вы создаете классическую виртуальную машину Azure.
* Исходные диски операционной системы и диски данных находятся в одной учетной записи хранения и одном контейнере. Если они находятся в разных местах, используйте AzCopy или Azure PowerShell, чтобы скопировать VHD. См. предыдущий шаг: [Копирование VHD с помощью AzCopy или PowerShell](#copy-vhd-with-azcopy-or-powershell). Скрипт также можно изменить согласно своему сценарию, но мы рекомендуем использовать AzCopy или PowerShell, так как этот способ более удобный и быстрый.

Скрипт автоматизации приводится ниже. Замените текст своими сведениями, и обновите скрипт так, чтобы он соответствовал конкретному сценарию.

> [!NOTE]
> Имеющийся сценарий не сохраняет конфигурацию сети исходной виртуальной машины. После переноса виртуальных машин параметры сети необходимо переопределить.
>
>

```powershell
    <#
    .Synopsis
    This script is provided as an EXAMPLE to show how to migrate a VM from a standard storage account to a premium storage account. You can customize it according to your specific requirements.

    .Description
    The script will copy the vhds (page blobs) of the source VM to the new storage account.
    And then it will create a new VM from these copied vhds based on the inputs that you specified for the new VM.
    You can modify the script to satisfy your specific requirement, but please be aware of the items specified
    in the Terms of Use section.

    .Terms of Use
    Copyright © 2015 Microsoft Corporation.  All rights reserved.

    THIS CODE AND ANY ASSOCIATED INFORMATION ARE PROVIDED "AS IS" WITHOUT WARRANTY OF ANY KIND,
    EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE IMPLIED WARRANTIES OF MERCHANTABILITY
    AND/OR FITNESS FOR A PARTICULAR PURPOSE. THE ENTIRE RISK OF USE, INABILITY TO USE, OR
    RESULTS FROM THE USE OF THIS CODE REMAINS WITH THE USER.

    .Example (Save this script as Migrate-AzureVM.ps1)

    .\Migrate-AzureVM.ps1 -SourceServiceName CurrentServiceName -SourceVMName CurrentVMName –DestStorageAccount newpremiumstorageaccount -DestServiceName NewServiceName -DestVMName NewDSVMName -DestVMSize "Standard_DS2" –Location "Southeast Asia"

    .Link
    To find more information about how to set up Azure PowerShell, refer to the following links.
    https://azure.microsoft.com/documentation/articles/powershell-install-configure/
    https://azure.microsoft.com/documentation/articles/storage-powershell-guide-full/
    https://azure.microsoft.com/blog/2014/10/22/migrate-azure-virtual-machines-between-storage-accounts/

    #>

    Param(
    # the cloud service name of the VM.
    [Parameter(Mandatory = $true)]
    [string] $SourceServiceName,

    # The VM name to copy.
    [Parameter(Mandatory = $true)]
    [String] $SourceVMName,

    # The destination storage account name.
    [Parameter(Mandatory = $true)]
    [String] $DestStorageAccount,

    # The destination cloud service name
    [Parameter(Mandatory = $true)]
    [String] $DestServiceName,

    # the destination vm name
    [Parameter(Mandatory = $true)]
    [String] $DestVMName,

    # the destination vm size
    [Parameter(Mandatory = $true)]
    [String] $DestVMSize,

    # the location of destination VM.
    [Parameter(Mandatory = $true)]
    [string] $Location,

    # whether or not to copy the os disk, the default is only copy data disks
    [Parameter(Mandatory = $false)]
    [Bool] $DataDiskOnly = $true,

    # how frequently to report the copy status in seconds
    [Parameter(Mandatory = $false)]
    [Int32] $CopyStatusReportInterval = 15,

    # the name suffix to add to new created disks to avoid conflict with source disk names
    [Parameter(Mandatory = $false)]
    [String]$DiskNameSuffix = "-prem"

    ) #end param

    #######################################################################
    #  Verify Azure PowerShell module and version
    #######################################################################

    #import the Azure PowerShell module
    Write-Host "`n[WORKITEM] - Importing Azure PowerShell module" -ForegroundColor Yellow
    $azureModule = Import-Module Azure -PassThru

    if ($azureModule -ne $null)
    {
        Write-Host "`tSuccess" -ForegroundColor Green
    }
    else
    {
        #show module not found interaction and bail out
        Write-Host "[ERROR] - PowerShell module not found. Exiting." -ForegroundColor Red
        Exit
    }


    #Check the Azure PowerShell module version
    Write-Host "`n[WORKITEM] - Checking Azure PowerShell module version" -ForegroundColor Yellow
    If ($azureModule.Version -ge (New-Object System.Version -ArgumentList "0.8.14"))
    {
        Write-Host "`tSuccess" -ForegroundColor Green
    }
    Else
    {
        Write-Host "[ERROR] - Azure PowerShell module must be version 0.8.14 or higher. Exiting." -ForegroundColor Red
        Exit
    }

    #Check if there is an azure subscription set up in PowerShell
    Write-Host "`n[WORKITEM] - Checking Azure Subscription" -ForegroundColor Yellow
    $currentSubs = Get-AzureSubscription -Current
    if ($currentSubs -ne $null)
    {
        Write-Host "`tSuccess" -ForegroundColor Green
        Write-Host "`tYour current azure subscription in PowerShell is $($currentSubs.SubscriptionName)." -ForegroundColor Green
    }
    else
    {
        Write-Host "[ERROR] - There is no valid Azure subscription found in PowerShell. Please refer to this article https://azure.microsoft.com/documentation/articles/powershell-install-configure/ to connect an Azure subscription. Exiting." -ForegroundColor Red
        Exit
    }


    #######################################################################
    #  Check if the VM is shut down
    #  Stopping the VM is a required step so that the file system is consistent when you do the copy operation.
    #  Azure does not support live migration at this time..
    #######################################################################

    if (($sourceVM = Get-AzureVM –ServiceName $SourceServiceName –Name $SourceVMName) -eq $null)
    {
        Write-Host "[ERROR] - The source VM doesn't exist in the current subscription. Exiting." -ForegroundColor Red
        Exit
    }

    # check if VM is shut down
    if ( $sourceVM.Status -notmatch "Stopped" )
    {
        Write-Host "[Warning] - Stopping the VM is a required step so that the file system is consistent when you do the copy operation. Azure does not support live migration at this time. If you'd like to create a VM from a generalized image, sys-prep the Virtual Machine before stopping it." -ForegroundColor Yellow
        $ContinueAnswer = Read-Host "`n`tDo you wish to stop $SourceVMName now? Input 'N' if you want to shut down the VM manually and come back later.(Y/N)"
        If ($ContinueAnswer -ne "Y") { Write-Host "`n Exiting." -ForegroundColor Red;Exit }
        $sourceVM | Stop-AzureVM

        # wait until the VM is shut down
        $VMStatus = (Get-AzureVM –ServiceName $SourceServiceName –Name $vmName).Status
        while ($VMStatus -notmatch "Stopped")
        {
            Write-Host "`n[Status] - Waiting VM $vmName to shut down" -ForegroundColor Green
            Sleep -Seconds 5
            $VMStatus = (Get-AzureVM –ServiceName $SourceServiceName –Name $vmName).Status
        }
    }

    # exporting the source vm to a configuration file, you can restore the original VM by importing this config file
    # see more information for Import-AzureVM
    $workingDir = (Get-Location).Path
    $vmConfigurationPath = $env:HOMEPATH + "\VM-" + $SourceVMName + ".xml"
    Write-Host "`n[WORKITEM] - Exporting VM configuration to $vmConfigurationPath" -ForegroundColor Yellow
    $exportRe = $sourceVM | Export-AzureVM -Path $vmConfigurationPath


    #######################################################################
    #  Copy the vhds of the source vm
    #  You can choose to copy all disks including os and data disks by specifying the
    #  parameter -DataDiskOnly to be $false. The default is to copy only data disk vhds
    #  and the new VM will boot from the original os disk.
    #######################################################################

    $sourceOSDisk = $sourceVM.VM.OSVirtualHardDisk
    $sourceDataDisks = $sourceVM.VM.DataVirtualHardDisks

    # Get source storage account information, not considering the data disks and os disks are in different accounts
    $sourceStorageAccountName = $sourceOSDisk.MediaLink.Host -split "\." | select -First 1
    $sourceStorageKey = (Get-AzStorageKey -StorageAccountName $sourceStorageAccountName).Primary
    $sourceContext = New-AzStorageContext –StorageAccountName $sourceStorageAccountName -StorageAccountKey $sourceStorageKey

    # Create destination context
    $destStorageKey = (Get-AzStorageKey -StorageAccountName $DestStorageAccount).Primary
    $destContext = New-AzStorageContext –StorageAccountName $DestStorageAccount -StorageAccountKey $destStorageKey

    # Create a container of vhds if it doesn't exist
    if ((Get-AzStorageContainer -Context $destContext -Name vhds -ErrorAction SilentlyContinue) -eq $null)
    {
        Write-Host "`n[WORKITEM] - Creating a container vhds in the destination storage account." -ForegroundColor Yellow
        New-AzStorageContainer -Context $destContext -Name vhds
    }


    $allDisksToCopy = $sourceDataDisks
    # check if need to copy os disk
    $sourceOSVHD = $sourceOSDisk.MediaLink.Segments[2]
    if ($DataDiskOnly)
    {
        # copy data disks only, this option requires deleting the source VM so that dest VM can boot
        # from the same vhd blob.
        $ContinueAnswer = Read-Host "`n`t[Warning] You chose to copy data disks only. Moving VM requires removing the original VM (the disks and backing vhd files will NOT be deleted) so that the new VM can boot from the same vhd. This is an irreversible action. Do you wish to proceed right now? (Y/N)"
        If ($ContinueAnswer -ne "Y") { Write-Host "`n Exiting." -ForegroundColor Red;Exit }
        $destOSVHD = Get-AzStorageBlob -Blob $sourceOSVHD -Container vhds -Context $sourceContext
        Write-Host "`n[WORKITEM] - Removing the original VM (the vhd files are NOT deleted)." -ForegroundColor Yellow
        Remove-AzureVM -Name $SourceVMName -ServiceName $SourceServiceName

        Write-Host "`n[WORKITEM] - Waiting until the OS disk is released by source VM. This may take up to several minutes."
        $diskAttachedTo = (Get-AzureDisk -DiskName $sourceOSDisk.DiskName).AttachedTo
        while ($diskAttachedTo -ne $null)
        {
            Start-Sleep -Seconds 10
            $diskAttachedTo = (Get-AzureDisk -DiskName $sourceOSDisk.DiskName).AttachedTo
        }

    }
    else
    {
        # copy the os disk vhd
        Write-Host "`n[WORKITEM] - Starting copying os disk $($disk.DiskName) at $(get-date)." -ForegroundColor Yellow
        $allDisksToCopy += @($sourceOSDisk)
        $targetBlob = Start-AzStorageBlobCopy -SrcContainer vhds -SrcBlob $sourceOSVHD -DestContainer vhds -DestBlob $sourceOSVHD -Context $sourceContext -DestContext $destContext -Force
        $destOSVHD = $targetBlob
    }


    # Copy all data disk vhds
    # Start all async copy requests in parallel.
    foreach($disk in $sourceDataDisks)
    {
        $blobName = $disk.MediaLink.Segments[2]
        # copy all data disks
        Write-Host "`n[WORKITEM] - Starting copying data disk $($disk.DiskName) at $(get-date)." -ForegroundColor Yellow
        $targetBlob = Start-AzStorageBlobCopy -SrcContainer vhds -SrcBlob $blobName -DestContainer vhds -DestBlob $blobName -Context $sourceContext -DestContext $destContext -Force
        # update the media link to point to the target blob link
        $disk.MediaLink = $targetBlob.ICloudBlob.Uri.AbsoluteUri
    }

    # Wait until all vhd files are copied.
    $diskComplete = @()
    do
    {
        Write-Host "`n[WORKITEM] - Waiting for all disk copy to complete. Checking status every $CopyStatusReportInterval seconds." -ForegroundColor Yellow
        # check status every 30 seconds
        Sleep -Seconds $CopyStatusReportInterval
        foreach ( $disk in $allDisksToCopy)
        {
            if ($diskComplete -contains $disk)
            {
                Continue
            }
            $blobName = $disk.MediaLink.Segments[2]
            $copyState = Get-AzStorageBlobCopyState -Blob $blobName -Container vhds -Context $destContext
            if ($copyState.Status -eq "Success")
            {
                Write-Host "`n[Status] - Success for disk copy $($disk.DiskName) at $($copyState.CompletionTime)" -ForegroundColor Green
                $diskComplete += $disk
            }
            else
            {
                if ($copyState.TotalBytes -gt 0)
                {
                    $percent = ($copyState.BytesCopied / $copyState.TotalBytes) * 100
                    Write-Host "`n[Status] - $('{0:N2}' -f $percent)% Complete for disk copy $($disk.DiskName)" -ForegroundColor Green
                }
            }
        }
    }
    while($diskComplete.Count -lt $allDisksToCopy.Count)

    #######################################################################
    #  Create a new vm
    #  the new VM can be created from the copied disks or the original os disk.
    #  You can ddd your own logic here to satisfy your specific requirements of the vm.
    #######################################################################

    # Create a VM from the existing os disk
    if ($DataDiskOnly)
    {
        $vm = New-AzureVMConfig -Name $DestVMName -InstanceSize $DestVMSize -DiskName $sourceOSDisk.DiskName
    }
    else
    {
        $newOSDisk = Add-AzureDisk -OS $sourceOSDisk.OS -DiskName ($sourceOSDisk.DiskName + $DiskNameSuffix) -MediaLocation $destOSVHD.ICloudBlob.Uri.AbsoluteUri
        $vm = New-AzureVMConfig -Name $DestVMName -InstanceSize $DestVMSize -DiskName $newOSDisk.DiskName
    }
    # Attached the copied data disks to the new VM
    foreach ($dataDisk in $sourceDataDisks)
    {
        # add -DiskLabel $dataDisk.DiskLabel if there are labels for disks of the source vm
        $diskLabel = "drive" + $dataDisk.Lun
        $vm | Add-AzureDataDisk -ImportFrom -DiskLabel $diskLabel -LUN $dataDisk.Lun -MediaLocation $dataDisk.MediaLink
    }

    # Edit this if you want to add more customization to the new VM
    # $vm | Add-AzureEndpoint -Protocol tcp -LocalPort 443 -PublicPort 443 -Name 'HTTPs'
    # $vm | Set-AzureSubnet "PubSubnet","PrivSubnet"

    New-AzureVM -ServiceName $DestServiceName -VMs $vm -Location $Location
```

#### <a name="optimization"></a>Оптимизация
Конфигурацию текущей виртуальной машины можно настроить для правильной работы с дисками хранилища класса Standard. Например, можно повысить производительность, используя несколько дисков в чередующемся томе. Например, вместо 4 отдельных дисков в хранилище класса Premium можно использовать один диск, чтобы сократить затраты. Такие способы оптимизации необходимо применять в индивидуальном порядке. Кроме того, после переноса они требуют выполнения дополнительных действий. Обратите внимание, что такая оптимизация может быть полезной для баз данных и приложений, которые зависят от разметки диска, определенной во время установки.

##### <a name="preparation"></a>Подготовка
1. Выполните простой перенос, как описано в предыдущем разделе. Оптимизация будет выполняться на новой виртуальной машине после переноса.
2. Для создания оптимальной конфигурации необходимо определить размеры новых дисков.
3. Определите сопоставление текущих дисков и томов со спецификациям нового диска.

##### <a name="execution-steps"></a>Шаги выполнения
1. Создайте новые диски нужного размера на виртуальной машине хранилища класса Premium.
2. Войдите на виртуальную машину и скопируйте данные из текущего тома на новый диск, который сопоставлен с этим томом. Сделайте это для всех текущих томов, которые необходимо сопоставить с новым диском.
3. Затем измените параметры приложения, чтобы переключиться на новые диски, и отключите старые тома.

Дополнительные сведения об оптимизации приложений для повышения производительности дисков см. в статье [Хранилище Azure класса Premium. Проектирование для обеспечения высокой производительности](../../virtual-machines/windows/premium-storage-performance.md).

### <a name="application-migrations"></a>Перенос приложений
Перенос баз данных и других сложных приложений может потребовать выполнения специальных действий, которые определяет поставщик приложений. См. документацию по соответствующему приложению. (например, обычно базы данных можно переносить путем резервного копирования и восстановления.

## <a name="next-steps"></a>Дальнейшие действия
Ознакомьтесь со следующими ресурсами для конкретных сценариев переноса виртуальных машин.

* [Перенос виртуальных машин Azure между учетными записями хранения.](https://azure.microsoft.com/blog/2014/10/22/migrate-azure-virtual-machines-between-storage-accounts/)
* [Создание и загрузка виртуального жесткого диска Windows Server в Azure.](../../virtual-machines/windows/upload-generalized-managed.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Создание и отправка диска VHD Linux в Azure](../../virtual-machines/linux/create-upload-generic.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Перенос виртуальных машин из Amazon AWS в Microsoft Azure.](https://channel9.msdn.com/Series/Migrating-Virtual-Machines-from-Amazon-AWS-to-Microsoft-Azure)

Дополнительные сведения о службе хранилища Azure и виртуальных машинах Azure см. также в следующих источниках:

* [Хранилище Azure](https://azure.microsoft.com/documentation/services/storage/)
* [Виртуальные машины Azure](https://azure.microsoft.com/documentation/services/virtual-machines/)
* [Выбор типа диска для виртуальных машин IaaS](../../virtual-machines/windows/disks-types.md)

[1]:./media/storage-migration-to-premium-storage/migration-to-premium-storage-1.png
[2]:./media/storage-migration-to-premium-storage/migration-to-premium-storage-1.png
[3]:./media/storage-migration-to-premium-storage/migration-to-premium-storage-3.png
[4]: https://technet.microsoft.com/library/hh831739.aspx
