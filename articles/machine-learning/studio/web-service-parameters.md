---
title: Параметры веб-службы Студии машинного обучения Azure | Документация Майкрософт
description: Как использовать параметры веб-службы машинного обучения Azure для изменения поведения модели при доступе к веб-службе.
services: machine-learning
documentationcenter: ''
author: xiaoharper
ms.custom: seodec18
ms.author: amlstudiodocs
editor: cgronlun
ms.assetid: c49187db-b976-4731-89d6-11a0bf653db1
ms.service: machine-learning
ms.subservice: studio
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 01/12/2017
ms.openlocfilehash: a236043d5622e5a2e1ffd572c887fb5ffac2174a
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "60345453"
---
# <a name="use-azure-machine-learning-studio-web-service-parameters"></a>Использование параметров веб-службы Студии машинного обучения Azure
Веб-служба машинного обучения Azure создается при публикации эксперимента, содержащего модули с настраиваемыми параметрами. В некоторых случаях может потребоваться изменение поведения модуля, пока веб-служба выполняется. *Параметры веб-службы* позволяют это сделать. 

Типичный пример — настройка модуля [Импорт данных][reader], благодаря которому пользователь опубликованной веб-службы может указать другой источник данных при обращении к веб-службе. Или настройка модуля [Export Data][writer] (Экспорт данных), позволяющего указать другое назначение. Некоторые другие примеры включают изменение количества битов модуля [Feature Hashing][feature-hashing] (Хэширование функций) или количества желаемых функций для модуля [Filter-Based Feature Selection][filter-based-feature-selection] (Выбор компонентов на основе фильтра). 

Можно определить параметры веб-службы и связать их с одним или несколькими параметрами модуля в эксперименте. Вы также можете указать, являются они обязательными или нет. Пользователь веб-службы затем может предоставить значения этих параметров при вызове веб-службы. 



## <a name="how-to-set-and-use-web-service-parameters"></a>Как установить и использовать параметры веб-службы
Параметр веб-службы задается путем щелчка по значку рядом с параметром конкретного модуля и выбора команды "Установить в качестве параметра веб-службы". При этом создается новый параметр веб-службы, который связывается с параметром модуля. Затем при доступе к веб-службе пользователь может указать значение параметра веб-службы, который будет применен к параметру модуля.

После определения параметр веб-службы будет доступен для любого другого параметра модуля в эксперименте. Если вы определите параметр веб-службы, связанный с параметром для одного модуля, можно использовать этот же параметр веб-службы для любого другого модуля в тех случаях, когда параметр использует значения одного типа. Например, если параметр веб-службы имеет числовое значение, это означает, что его можно использовать только для параметров модуля, которые принимают числовые значения. Когда пользователь устанавливает значение для параметра веб-службы, оно будет применено ко всем связанным параметрам модуля.

Вы можете принять решение о том, следует ли предоставлять значение по умолчанию для параметра веб-службы. Если принимается решение предоставлять это значение, то параметр становится необязательным для пользователя веб-службы. Если вы не предоставите значение по умолчанию, то от пользователя потребуется ввести значение при доступе к веб-службе.

Документация по API для веб-службы содержит сведения для пользователя веб-службы о том, как программно указать параметр веб-службы при доступе к ней.

> [!NOTE]
> Документация по API для классической веб-службы предоставляется по ссылке на **страницу справки API**, доступной на **панели мониторинга** этой веб-службы в Студии машинного обучения. Документация по API для новой веб-службы предоставляется на портале [веб-служб Машинного обучения Azure](https://services.azureml.net/Quickstart) на страницах **Consume** (Использование) и **Swagger API** этой веб-службы.
> 
> 

## <a name="example"></a>Пример
В качестве примера предположим, что у нас имеется эксперимент с модулем [Export Data][writer] (Экспорт данных), который отправляет сведения в хранилище BLOB-объектов Azure. Мы определим параметр веб-службы с именем "Путь к BLOB-объекту", который позволяет пользователю веб-службы изменять путь к хранилищу BLOB-объектов при доступе к этой службе.

1. В Студии машинного обучения щелкните модуль [Export Data][writer] (Экспорт данных), чтобы выбрать его. Его свойства отображаются на панели "Свойства" справа от холста эксперимента.
2. Укажите тип хранилища.
   
   * В разделе **Укажите целевое местоположение данных**выберите "Хранилище BLOB-объектов Azure".
   * В разделе **Укажите тип проверки подлинности**выберите "Учетная запись".
   * Введите данные учетной записи для хранилища BLOB-объектов Azure. 

3. Щелкните значок справа от элемента **Путь к BLOB-объекту, начиная с параметра контейнера**. Это выглядит следующим образом.
   
   ![Значок параметра веб-службы](./media/web-service-parameters/icon.png)
   
   Выберите "Установить как параметр веб-службы".
   
   В раздел **Параметры веб-службы** внизу на странице "Свойства" добавляется запись с именем "Путь к BLOB-объекту, начиная с контейнера". Этот параметр веб-службы теперь связан с параметром модуля [Export Data][writer] (Экспорт данных).
4. Чтобы переименовать параметр веб-службы, щелкните имя, введите "Путь к BLOB-объекту", после чего нажмите клавишу **ВВОД** . 
5. Чтобы предоставить значение по умолчанию для параметра веб-службы, щелкните значок справа от имени, выберите "Предоставить значение по умолчанию", введите значение (например, "контейнер1/выход1.csv"), после чего нажмите клавишу **ВВОД** .
   
   ![Параметр веб-службы](./media/web-service-parameters/parameter.png)
6. Нажмите кнопку **Запустить**. 
7. Щелкните **Deploy Web Service** (Развернуть веб-службу) и выберите **Deploy Web Service [Classic]** (Развернуть веб-службу [классическую]) или **Deploy Web Service [New]** (Развернуть веб-службу [новую]), чтобы развернуть ее.

> [!NOTE] 
> Для развертывания новой веб-службы у вас должен быть достаточный уровень разрешений в подписке, в которую выполняется развертывание веб-службы. Дополнительные сведения см. в статье [Управление веб-службой с помощью портала веб-служб машинного обучения Azure](manage-new-webservice.md). 

Теперь пользователь веб-службы может указать новое расположение для модуля [Export Data][writer] (Экспорт данных) при доступе к веб-службе.

## <a name="more-information"></a>Дополнительные сведения
Более подробный пример см. в публикации [AzureML Web Service Parameters](https://blogs.technet.com/b/machinelearning/archive/2014/11/25/azureml-web-service-parameters.aspx) (Параметры веб-службы AzureML) [блога, посвященного машинному обучению](https://blogs.technet.com/b/machinelearning/archive/2014/11/25/azureml-web-service-parameters.aspx).

Дополнительные сведения о доступе к веб-службе машинного обучения см. в статье [Как использовать веб-службу машинного обучения Azure](consume-web-services.md).

<!-- Module References -->
[feature-hashing]: https://msdn.microsoft.com/library/azure/c9a82660-2d9c-411d-8122-4d9e0b3ce92a/
[filter-based-feature-selection]: https://msdn.microsoft.com/library/azure/918b356b-045c-412b-aa12-94a1d2dad90f/
[reader]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
[writer]: https://msdn.microsoft.com/library/azure/7a391181-b6a7-4ad4-b82d-e419c0d6522c/

