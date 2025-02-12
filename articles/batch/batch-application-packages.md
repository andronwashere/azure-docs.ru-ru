---
title: Установка пакетов приложений на вычислительных узлах (пакетная служба Azure) | Документация Майкрософт
description: Пакеты приложений пакетной службы Azure позволяют легко управлять несколькими приложениями и их версиями для установки на вычислительные узлы пакетной службы.
services: batch
documentationcenter: .net
author: laurenhughes
manager: gwallace
editor: ''
ms.assetid: 3b6044b7-5f65-4a27-9d43-71e1863d16cf
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: big-compute
ms.date: 04/26/2019
ms.author: lahugh
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 5967f2ac8c766005cee876b5b42109062abad6a1
ms.sourcegitcommit: 4b431e86e47b6feb8ac6b61487f910c17a55d121
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/18/2019
ms.locfileid: "68323847"
---
# <a name="deploy-applications-to-compute-nodes-with-batch-application-packages"></a>Развертывание приложений на вычислительных узлах с помощью пакетов приложений пакетной службы

Использование пакетов приложений пакетной службы Azure обеспечивает простое управление приложениями задач и их развертывание на вычислительных узлах в пуле. С помощью пакетов приложений вы можете передать несколько версий приложений, выполняемых вашими задачами (в том числе вспомогательные файлы), а также управлять ими, а затем автоматически развернуть одно или несколько таких приложений на вычислительных узлах в пуле.

В этой статье раскрываются такие темы, как передача пакетов приложений на портале Azure, а также управление ими, Затем вы узнаете, как установить их на расчетные узлы пула с помощью библиотеки [.NET пакетной][api_net] службы.

> [!NOTE]
> Пакеты приложений поддерживаются во всех пулах пакетной службы, созданных после 5 июля 2017 г. Если пул создан с помощью конфигурации облачной службы, пакеты приложений также поддерживаются в пулах пакетной службы, созданных между 10 марта 2016 г. и 5 июля 2017 г. Пулы пакетной службы, созданные до 10 марта 2016 г., не поддерживают пакеты приложений.
>
> API-интерфейсы для создания пакетов приложений и управления ими являются частью библиотеки [.NET][api_net_mgmt] library. The APIs for installing application packages on a compute node are part of the [Batch .NET][api_net] для управления пакетной службой. Схожие функции находятся в доступных API-интерфейсах пакетной службы для других языков. 
>
> Компонент "Пакеты приложений", описываемый в этой статье, заменяет компонент "Приложения пакетной службы", доступный в предыдущих версиях службы.

## <a name="application-package-requirements"></a>Требования к пакетам приложений
Чтобы использовать пакеты приложений, необходимо [связать учетную запись хранения Azure](#link-a-storage-account) с учетной записью пакетной службы.

## <a name="about-applications-and-application-packages"></a>О приложениях и пакетах приложений
В пакетной службе Azure *приложение* ссылается на набор двоичных файлов с возможностью управления версиями, которые могут автоматически скачиваться на вычислительные узлы в пуле. *Пакет приложения* ссылается на *определенный набор* таких двоичных файлов, представляя конкретную *версию* приложения.

![Обзорная схема: приложения и пакеты приложений][1]

### <a name="applications"></a>ПРИЛОЖЕНИЯ
Приложение в пакетной службе содержит один или несколько пакетов приложений и задает параметры конфигурации для приложения, Например, приложение может указать версию пакета приложения по умолчанию для установки на вычислительных узлах, а также определяет необходимость обновления или удаления пакетов.

### <a name="application-packages"></a>Пакеты приложений
Пакет приложения — это ZIP-файл, содержащий двоичные файлы приложения и вспомогательные файлы, которые необходимы задачам для выполнения приложения. Каждый пакет приложения представляет определенную версию приложения.

Пакеты приложений можно указывать на уровне пула и задачи. При создании пула или задачи можно указать один или несколько таких пакетов и (при необходимости) версию.

* **Пакеты приложений уровня пула** развертываются на *каждом* узле в пуле. Развертывание приложений осуществляется во время присоединения узла к пулу, а также когда узел перезагружается или для него пересоздается образ.
  
    Если все узлы в пуле выполняют задачи задания, пакеты приложения уровня пула считаются пригодными. При создании пула можно указать один или несколько пакетов приложений, а также добавить или обновить имеющиеся в пуле пакеты. Чтобы обновить имеющиеся пакеты приложений, следует перезапустить узлы пула. Это позволит установить новый пакет.
* **Пакеты приложений уровня задач** развертываются только на вычислительных узлах, на которых запланировано выполнение задач. Развертывание осуществляется перед выполнением командной строки задачи. Если в узле уже развернут указанный пакет приложений и настроена версия, повторное развертывание не инициируется и используется имеющийся пакет.
  
    Пакеты приложений уровня задач полезны в средах с общим пулом, где различные задания выполняются в одном пуле, который не удаляется по завершении задания. Если задание содержит меньше задач, чем число узлов в пуле, пакеты приложений задач помогут минимизировать объем передаваемых данных, так как приложение развертывается только на узлах, на которых выполняются задачи.
  
    Мы также рекомендуем использовать пакеты приложений уровня задач для выполнения небольшого количества задач в сценариях с большими приложениями. Например, для выполнения задачи предварительной обработки или объединения с помощью соответствующих ресурсоемких приложений.

> [!IMPORTANT]
> Существуют ограничения на количество приложений и пакетов приложений в учетной записи пакетной службы, а также на максимальный размер пакета. Дополнительные сведения см. в статье [Квоты и ограничения пакетной службы Azure](batch-quota-limit.md).
> 
> 

### <a name="benefits-of-application-packages"></a>Преимущества пакетов приложений
Пакеты приложений позволяют упростить код в решении пакетной службы, а также сократить расходы на управление приложениями, выполняемыми задачами.

При использовании пакетов приложений задача запуска пула не должна указывать длинный список отдельных файлов ресурсов для установки на узлах. Кроме того, вам не нужно вручную управлять несколькими версиями этих файлов приложения в службе хранилища Azure или на узлах, а также создавать [URL-адреса SAS](../storage/common/storage-dotnet-shared-access-signature-part-1.md), чтобы предоставить доступ к файлам в своей учетной записи хранения. Пакетная служба работает со службой хранилища Azure в фоновом режиме, обеспечивая хранение пакетов приложений и их развертывание на вычислительных узлах.

> [!NOTE] 
> Общий размер задачи запуска не должен превышать 32 768 символов, включая файлы ресурсов и переменные среды. Если задача запуска превышает это ограничение, то можно воспользоваться пакетами приложений. Также можно создать ZIP-архив, содержащий файлы ресурсов, передать его в виде большого двоичного объекта в службу хранилища Azure, а затем распаковать его из командной строки задачи запуска. 
>
>

## <a name="upload-and-manage-applications"></a>Передача приложений и управление ими
Вы можете использовать [портал Azure][portal] или API-интерфейсы управления пакетной службой для управления пакетами приложений в учетной записи пакетной службы. В следующих разделах мы сначала покажем, как связать учетную запись хранения, а затем ознакомимся с добавлением приложений и пакетов, а также с управлением ими на портале.

### <a name="link-a-storage-account"></a>Связывание учетной записи хранения
Чтобы использовать пакеты приложений, вам сначала нужно связать [учетную запись хранения Azure](batch-api-basics.md#azure-storage-account) со своей учетной записью пакетной службы. Если вы еще не настроили учетную запись хранения, то на портале Azure отобразится предупреждение, когда вы в первый раз щелкнете элемент **Приложения** в вашей учетной записи пакетной службы.



![Предупреждение "Нет настроенной учетной записи хранения" на портале Azure][9]

Пакетная служба использует связанную учетную запись хранения для хранения пакетов приложений. Когда вы свяжете две учетные записи, пакетная служба сможет автоматически развертывать на вычислительных узлах пакеты, хранимые в связанной учетной записи хранения. Щелкните **Учетная запись хранения** в окне **Предупреждение**, а затем снова выберите **Учетная запись хранения**, чтобы связать учетную запись хранения с учетной записью пакетной службы.

![Выбор колонки учетной записи хранения на портале Azure][10]

Мы рекомендуем создать учетную запись хранения *специально* для учетной записи пакетной службы и выбрать ее здесь. Созданную учетную запись хранения затем можно связать с учетной записью пакетной службы в окне **Учетная запись хранения**.

> [!NOTE] 
> В настоящее время невозможно использовать пакеты приложений с учетной записью хранения Azure, для которой настроены [правила брандмауэра](../storage/common/storage-network-security.md).
> 

Пакетная служба использует службу хранилища Azure для хранения пакетов приложений в виде блочных BLOB-объектов. Вы платите [как обычная][storage_pricing] для данных блочного BLOB-объекта, и размер каждого пакета не может превышать [максимальный размер блочного BLOB-объекта](../storage/common/storage-scalability-targets.md#azure-blob-storage-scale-targets). Вам обязательно следует учитывать размер и количество пакетов приложений, периодически удаляя устаревшие пакеты для минимизации расходов.
> 
> 

### <a name="view-current-applications"></a>Просмотр текущих приложений
Чтобы просмотреть приложения в учетной записи пакетной службы, щелкните пункт меню слева **Приложения** в вашей **учетной записи пакетной службы**.

![Элемент "Приложения"][2]

Если выбрать этот пункт меню, то откроется окно **Приложения**:

![Список приложений][3]

В этом окне выводится идентификатор каждого приложения в вашей учетной записи, а также следующие свойства:

* **Пакеты**. Количество версий, связанных с этим приложением.
* **Версия по умолчанию**. Версия приложения, которая будет установлена, если вы не обозначите версию, указывая приложение для пула. Это необязательный параметр.
* **Разрешить обновления**. Значение, которое определяет разрешение на обновления, удаления и дополнения пакетов. Если для этого параметра задано значение **Нет**, возможность обновления и удаления пакетов будет отключена для приложения. Вы сможете только добавлять новые версии пакета приложения. По умолчанию используется значение **Да**.

Если вы хотите просмотреть структуру файлов пакета приложения на вашем кластерном узле, перейдите к учетной записи пакетной службы на портале. В учетной записи пакетной службы перейдите к разделу **Пулы**. Выберите пул, содержащий нужные вам кластерные узлы.

![Узлы в пуле][13]

Выбрав пул, перейдите на узел вычислений, на котором установлен пакет приложения. После этого сведения о пакете приложения находятся в папке **приложения** . Дополнительные папки на кластерном узле содержат другие файлы, такие как задачи запуска, выходные файлы, вывод ошибок и т. д.

![Файлы на узле][14]

### <a name="view-application-details"></a>Просмотр сведений о приложении
Чтобы просмотреть подробные сведения о приложении, выберите его в окне **Приложения**.

![Сведения о приложении][4]

В разделе сведений о приложении можно настроить следующие параметры для приложения.

* **Разрешить обновления**. Указывает на возможность обновления и удаления пакетов приложений. См. раздел "Обновление или удаление пакета приложения" ниже.
* **Версия по умолчанию**. Указывает пакет приложения по умолчанию для развертывания на вычислительных узлах.
* **Отображаемое имя**. Указывает понятное имя, которое решение пакетной службы может использовать при отображении сведений о приложении, например в пользовательском интерфейсе службы, предоставляемой пользователям с помощью пакетной службы.

### <a name="add-a-new-application"></a>Добавление нового приложения
Чтобы создать приложение, добавьте пакет приложения и укажите новый уникальный идентификатор приложения. Первый пакет приложения, добавленный с помощью идентификатора нового приложения, в свою очередь создает приложение.

Выберите **Приложения** > **Добавить**.

![Колонка "Новое приложение" на портале Azure][5]

Окно **Новое приложение** содержит следующие поля для настройки параметров нового приложения и пакета приложения.

**Идентификатор приложения**

Это поле указывает идентификатор нового приложения, на который распространяются стандартные правила проверки идентификатора пакетной службы Azure. Ниже приводятся правила предоставления идентификатора приложения.

* На узлах Windows идентификатор может содержать любое сочетание буквенно-цифровых знаков, дефисов и символов подчеркивания. На узлах Linux допускаются только буквенно-цифровые знаки и символы подчеркивания.
* Не может содержать более 64 символов.
* Должен быть уникальным в рамках учетной записи пакетной службы.
* Не меняет и не учитывает регистр.

**Версия**

Это поле указывает версию пакета приложения, который вы передаете. На строки версии распространяются следующие правила проверки.

* На узлах Windows строка версии может содержать любое сочетание буквенно-цифровых знаков, дефисов, символов подчеркивания и точек. На узлах Linux строка версии может содержать только буквенно-цифровые знаки и символы подчеркивания.
* Не может содержать более 64 символов.
* Должны быть уникальными в рамках приложения.
* Не меняет и не учитывает регистр.

**Пакет приложения**

Это поле указывает ZIP-файл, содержащий двоичные файлы приложения и вспомогательные файлы, необходимые для выполнения приложения. Щелкните текстовое поле **Выбор файла** или значок папки, чтобы выбрать ZIP-файл, содержащий файлы приложения.

Выбрав файл, нажмите кнопку **ОК** , чтобы передать файл в службу хранилища Azure. По завершении операции передачи портал отображает уведомление. Скорость выполнения этой операции зависит от размера передаваемого файла и возможностей сетевого подключения.

> [!WARNING]
> Не закрывайте окно **Новое приложение** до завершения операции. Такое действие останавливает процесс передачи.
> 
> 

### <a name="add-a-new-application-package"></a>Добавление нового пакета приложения
Чтобы добавить версию пакета приложения для имеющегося приложения, выберите это приложение в окне **Приложения** и щелкните **Пакеты** > **Добавить**.

![Колонка "Добавление пакета приложения" на портале Azure][8]

Как видите, поля в **новом окне приложения** совпадают, но поле **Application ID (идентификатор приложения** ) отключено. Как и для нового приложения, укажите **версию** нового пакета, выберите ZIP-файл **пакета приложения**, а затем нажмите кнопку **OК**, чтобы передать пакет.

### <a name="update-or-delete-an-application-package"></a>Обновление или удаление пакета приложения
Чтобы обновить или удалить имеющийся пакет приложения, откройте раздел сведений о приложении, выберите **Пакеты**, а затем щелкните **многоточие** в строке пакета приложения, который вы хотите изменить, и выберите нужное действие.

![Обновление или удаление пакета на портале Azure][7]

**Update**

Если щелкнуть **Обновить**, отобразится окно **Обновление пакета**. Это окно аналогично окну **Новый пакет приложения**, но в нем активно только поле выбора пакета, с помощью которого вы можете указать новый ZIP-файл для передачи.

![Колонка "Обновление пакета" на портале Azure][11]

**Удаление**

Если щелкнуть **Удалить**, вам будет предложено подтвердить удаление версии пакета. Затем пакетная служба удалит пакет из службы хранилища Azure. Удалив версию приложения по умолчанию, вы удалите настройку **версии по умолчанию** для этого приложения.

![Удаление приложения][12]

## <a name="install-applications-on-compute-nodes"></a>Установка приложений на вычислительных узлах
Мы узнали об управлении пакетами приложений с помощью портала Azure. Теперь можно перейти к их развертыванию на вычислительных узлах, а также их выполнению с помощью задач пакетной службы.

### <a name="install-pool-application-packages"></a>Установка пакетов приложений уровня пула
Чтобы установить пакет приложения на все вычислительные узлы в пуле, укажите *ссылки* на один или несколько пакетов приложений для пула. Пакеты приложения, которые указываются для пула, устанавливаются на каждый вычислительный узел, когда он присоединяется к пулу, а также когда он перезагружается или для него пересоздается образ.

В пакетной среде .NET укажите один или несколько [CloudPool][net_cloudpool] .[ApplicationPackageReferences][net_cloudpool_pkgref] при создании нового пула или для существующего пула. Класс [ApplicationPackageReference][net_pkgref] определяет идентификатор приложения и версию для установки на вычислительные узлы пула.

```csharp
// Create the unbound CloudPool
CloudPool myCloudPool =
    batchClient.PoolOperations.CreatePool(
        poolId: "myPool",
        targetDedicatedComputeNodes: 1,
        virtualMachineSize: "standard_d1_v2",
        cloudServiceConfiguration: new CloudServiceConfiguration(osFamily: "5"));

// Specify the application and version to install on the compute nodes
myCloudPool.ApplicationPackageReferences = new List<ApplicationPackageReference>
{
    new ApplicationPackageReference {
        ApplicationId = "litware",
        Version = "1.1001.2b" }
};

// Commit the pool so that it's created in the Batch service. As the nodes join
// the pool, the specified application package is installed on each.
await myCloudPool.CommitAsync();
```

> [!IMPORTANT]
> Если развертывание пакета приложения по какой либо причине завершается сбоем, пакетная служба помечает узел как непригодный для [использования][net_nodestate]и не планирует выполнение задач на этом узле. В этом случае следует **перезапустить** этот узел, чтобы повторно инициировать развертывание пакета. Перезапуск узла также возобновляет планирование задач на узле.
> 
> 

### <a name="install-task-application-packages"></a>Установка пакетов приложений уровня задач
Как и для пакетов уровня пула, укажите *ссылки* на один или несколько пакетов приложений для задачи. Если на узле запланировано выполнение задачи, скачивание и извлечение пакета осуществляется непосредственно перед выполнением командной строки задачи. Если в пуле уже установлен указанный пакет и настроена версия, скачивание не инициируется и используется имеющийся пакет.

Чтобы установить пакет приложения задачи, настройте свойство [CloudTask][net_cloudtask] .[ApplicationPackageReferences][net_cloudtask_pkgref] задачи:

```csharp
CloudTask task =
    new CloudTask(
        "litwaretask001",
        "cmd /c %AZ_BATCH_APP_PACKAGE_LITWARE%\\litware.exe -args -here");

task.ApplicationPackageReferences = new List<ApplicationPackageReference>
{
    new ApplicationPackageReference
    {
        ApplicationId = "litware",
        Version = "1.1001.2b"
    }
};
```

## <a name="execute-the-installed-applications"></a>Выполнение установленных приложений
Пакеты, указанные для уровня пула или задач, скачиваются и извлекаются в указанный каталог `AZ_BATCH_ROOT_DIR` на узле. Пакетная служба также создает переменную среды, содержащую путь к указанному каталогу. Командные строки задачи будут использовать эту переменную среды, ссылаясь на приложение в узле. 

На узлах Windows переменная имеет следующий формат:

```
Windows:
AZ_BATCH_APP_PACKAGE_APPLICATIONID#version
```

На узлах Linux формат немного отличается. Точки (.), дефисы (-) и символы решетки (#) в переменной среды преобразовываются в символы подчеркивания. Кроме того, обратите внимание, что регистр идентификатора приложения сохраняется. Пример:

```
Linux:
AZ_BATCH_APP_PACKAGE_applicationid_version
```

Свойства `APPLICATIONID` и `version` — это значения, соответствующие версии пакета и приложения, указанных при развертывании. Например, если вы выбираете установку приложения *blender* версии 2.7 на узлах Windows, то командные строки задачи будут использовать переменную среды для получения доступа к его файлам:

```
Windows:
AZ_BATCH_APP_PACKAGE_BLENDER#2.7
```

На узлах Linux укажите переменную среды в таком формате. Преобразуйте точки (.), дефисы (-) и знаки номера (#) в нижние подчеркивания и соблюдайте регистр идентификатора приложения:

```
Linux:
AZ_BATCH_APP_PACKAGE_blender_2_7
``` 

При передаче пакета приложения можно указать версию по умолчанию для развертывания на вычислительных узлах. Если для приложения указана версия по умолчанию, то на это приложение можно ссылаться без суффикса версии. Версию приложения по умолчанию можно указать на портале Azure в окне **Приложения**, как показано в разделе [Передача приложений и управление ими](#upload-and-manage-applications).

Например, если для приложения *blender* в качестве версии по умолчанию указана версия 2.7 и задачи ссылаются на следующую переменную среды, то узлы Windows будут выполнять версию 2.7:

`AZ_BATCH_APP_PACKAGE_BLENDER`

В следующем фрагменте кода показан образец командной строки задачи, используемый для запуска версии приложения *blender* по умолчанию.

```csharp
string taskId = "blendertask01";
string commandLine =
    @"cmd /c %AZ_BATCH_APP_PACKAGE_BLENDER%\blender.exe -args -here";
CloudTask blenderTask = new CloudTask(taskId, commandLine);
```

> [!TIP]
> Дополнительные сведения о параметрах среды вычислительных узлов см. в разделе [Параметры среды для задач](batch-api-basics.md#environment-settings-for-tasks) статьи [Обзор функций пакетной службы для разработчиков](batch-api-basics.md).
> 
> 

## <a name="update-a-pools-application-packages"></a>Обновление пакетов приложений пула
Если для существующего пула уже выполнена настройка пакета приложения, вы можете указать новый пакет для пула. Если указывается ссылка на новый пакет для пула, действует следующее.

* Пакетная служба устанавливает новый указанный пакет на все новые узлы, присоединенные к пулу, и на все имеющиеся узлы, которые были перезагружены или для которых был повторно создан образ.
* Новый пакет приложения не будет автоматически установлен на вычислительные узлы, которые во время обновления ссылки на пакет уже существовали в пуле. Для получения нового пакета их необходимо перезагрузить либо заново создать для них образ.
* Переменные среды, создаваемые при развертывании нового пакета, отражают новые ссылки на пакеты приложения.

В этом примере существующий пул имеет версию 2,7 приложения для наложения  , настроенную как одно из его [CloudPool][net_cloudpool].[ApplicationPackageReferences][net_cloudpool_pkgref]. Чтобы обновить узлы в пуле, используя версию 2.76b, укажите новое свойство [ApplicationPackageReference][net_pkgref] с новой версией, а затем подтвердите изменения.

```csharp
string newVersion = "2.76b";
CloudPool boundPool = await batchClient.PoolOperations.GetPoolAsync("myPool");
boundPool.ApplicationPackageReferences = new List<ApplicationPackageReference>
{
    new ApplicationPackageReference {
        ApplicationId = "blender",
        Version = newVersion }
};
await boundPool.CommitAsync();
```

После настройки новой версии пакетная служба устанавливает версию 2.76b на всех *новых* узлах, присоединенных к пулу. Чтобы установить версию 2.76b на все узлы, *уже* имеющиеся в пуле, перезагрузите их или создайте для них новый образ. Обратите внимание: после перезагрузки на узлах остаются файлы предыдущих развертываний пакета.

## <a name="list-the-applications-in-a-batch-account"></a>Список приложений в учетной записи пакетной службы
Вы можете перечислить приложения и их пакеты в учетной записи пакетной службы с помощью метода [метода applicationoperations][net_appops] .[ListApplicationSummaries][net_appops_listappsummaries] .

```csharp
// List the applications and their application packages in the Batch account.
List<ApplicationSummary> applications = await batchClient.ApplicationOperations.ListApplicationSummaries().ToListAsync();
foreach (ApplicationSummary app in applications)
{
    Console.WriteLine("ID: {0} | Display Name: {1}", app.Id, app.DisplayName);

    foreach (string version in app.Versions)
    {
        Console.WriteLine("  {0}", version);
    }
}
```

## <a name="wrap-up"></a>Заключение
С помощью пакетов приложений вы можете предоставлять пользователям возможность выбирать приложения для своих заданий и указывать точную версию для использования при обработке заданий в пакетной службе. Вы также можете предоставить пользователям возможность передавать и отслеживать свои приложения в вашей службе.

## <a name="next-steps"></a>Следующие шаги
* Элемент [пакетной REST API][api_rest] also provides support to work with application packages. For example, see the [applicationPackageReferences][rest_add_pool_with_packages] в разделе [Добавление пула в учетную запись][rest_add_pool] для получения сведений о том, как указать пакеты для установки с помощью REST API. Дополнительные сведения о получении сведений о приложении с помощью пакетного REST API см. в разделе [приложения][rest_applications] .
* Узнайте больше о программном [управлении квотами и учетными записями пакетной службы Azure с помощью библиотеки .NET для управления пакетной службой](batch-management-dotnet.md). Библиотека [.NET для управления пакетной][api_net_mgmt] службой может включать функции создания и удаления учетной записи для приложения или службы.

[api_net]: https://docs.microsoft.com/dotnet/api/overview/azure/batch/client?view=azure-dotnet
[api_net_mgmt]: https://docs.microsoft.com/dotnet/api/overview/azure/batch/management?view=azure-dotnet
[api_rest]: https://docs.microsoft.com/rest/api/batchservice/
[batch_mgmt_nuget]: https://www.nuget.org/packages/Microsoft.Azure.Management.Batch/
[github_samples]: https://github.com/Azure/azure-batch-samples
[storage_pricing]: https://azure.microsoft.com/pricing/details/storage/
[net_appops]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.applicationoperations.aspx
[net_appops_listappsummaries]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.applicationoperations.listapplicationsummaries.aspx
[net_cloudpool]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.aspx
[net_cloudpool_pkgref]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.applicationpackagereferences.aspx
[net_cloudtask]: https://msdn.microsoft.com/library/microsoft.azure.batch.cloudtask.aspx
[net_cloudtask_pkgref]: https://msdn.microsoft.com/library/microsoft.azure.batch.cloudtask.applicationpackagereferences.aspx
[net_nodestate]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenode.state.aspx
[net_pkgref]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.applicationpackagereference.aspx
[portal]: https://portal.azure.com
[rest_applications]: https://msdn.microsoft.com/library/azure/mt643945.aspx
[rest_add_pool]: https://msdn.microsoft.com/library/azure/dn820174.aspx
[rest_add_pool_with_packages]: https://msdn.microsoft.com/library/azure/dn820174.aspx#bk_apkgreference

[1]: ./media/batch-application-packages/app_pkg_01.png "Общая схема: пакеты приложений"
[2]: ./media/batch-application-packages/app_pkg_02.png "Элемент "Приложения" на портале Azure"
[3]: ./media/batch-application-packages/app_pkg_03.png "Колонка "Приложения" на портале Azure"
[4]: ./media/batch-application-packages/app_pkg_04.png "Параметры в колонке "Сведения о приложении" на портале Azure"
[5]: ./media/batch-application-packages/app_pkg_05.png "Колонка "Новое приложение" на портале Azure"
[7]: ./media/batch-application-packages/app_pkg_07.png "Пункты "Обновить" и "Удалить" для пакетов на портале Azure"
[8]: ./media/batch-application-packages/app_pkg_08.png "Колонка "Новый пакет приложения" на портале Azure"
[9]: ./media/batch-application-packages/app_pkg_09.png "Предупреждение об отсутствии связанной учетной записи хранения"
[10]: ./media/batch-application-packages/app_pkg_10.png "Выбор колонки учетной записи хранения на портале Azure"
[11]: ./media/batch-application-packages/app_pkg_11.png "Колонка "Обновление пакета" на портале Azure"
[12]: ./media/batch-application-packages/app_pkg_12.png "Диалоговое окно для подтверждения удаления пакета на портале Azure"
[13]: ./media/batch-application-packages/package-file-structure.png "Сведения об узле вычислений в портал Azure"
[14]: ./media/batch-application-packages/package-file-structure-node.png "Файлы на кластерном узле, отображаемом в портал Azure"
