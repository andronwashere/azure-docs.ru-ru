---
title: Установка лицензионных компонентов для среды выполнения интеграции Azure–SSIS | Документация Майкрософт
description: Узнайте, как независимый поставщик программного обеспечения может разрабатывать и устанавливать платные или лицензионные пользовательские компоненты для среды выполнения интеграции Azure–SSIS
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 04/13/2018
author: swinarko
ms.author: sawinark
ms.reviewer: douglasl
manager: craigg
ms.openlocfilehash: 80d4fff03422beacccd3aff3cdd8cb1047d5f5af
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "61344658"
---
# <a name="install-paid-or-licensed-custom-components-for-the-azure-ssis-integration-runtime"></a>Установка платных или лицензионных пользовательских компонентов для среды выполнения интеграции Azure–SSIS

В этой статье описывается, как независимый поставщик программного обеспечения может разрабатывать и устанавливать платные или лицензионные пользовательские компоненты для пакетов MSSQL Integration Services, которые выполняются в среде выполнения интеграции Azure–SSIS.

## <a name="the-problem"></a>Задача

Характер среды выполнения Azure SSIS представляет собой несколько проблем, которые делают типичные методы лицензирования, используемые для локальной установки пользовательских компонентов, неподходящими. В результате среде выполнения интеграции Azure–SSIS требуется другой подход.

-   Узлы среды выполнения интеграции Azure SSIS являются неустойчивыми и могут быть выделены или выпущены в любое время. Например, можно запускать или останавливать узлы для управления затратами или выполнять масштабирование в разных размерах узлов. В результате привязка лицензии стороннего компонента к определенному узлу с помощью относящихся к компьютеру сведений, таких как MAC-адрес или идентификатор ЦП, больше недоступна.

-   Также вы можете масштабировать среду выполнения интеграции Azure SSIS. То есть количество узлов можно уменьшить и увеличить в любое время.

## <a name="the-solution"></a>Решение

В результате ограничений традиционных методов лицензирования, описанных в предыдущем разделе, среда выполнения интеграции Azure–SSIS предоставляет новое решение. Это решение использует переменные среды Windows и системные переменные MSSQL Integration Services для привязки и проверки лицензий сторонних компонентов. Независимые поставщики программного обеспечения могут использовать эти переменные, чтобы получить уникальную и постоянную информацию о среде выполнения интеграции Azure SSIS, такую как идентификатор кластера и число его узлов. Благодаря этой информации независимые поставщики программного обеспечения могут связать лицензии для своего компонента со средой выполнения интеграции Azure–SSIS *как кластером*. Эта привязка использует идентификатор, который остается неизменным при запуске или остановке, увеличении или уменьшении масштаба или перенастройке среды выполнения интеграции Azure–SSIS каким-либо образом.

На следующей схеме показаны обычные потоки установки, активации, привязки и проверки компонентов сторонних поставщиков, которые используют эти переменные:

![Установка лицензированных компонентов](media/how-to-configure-azure-ssis-ir-licensed-components/licensed-component-installation.png)

## <a name="instructions"></a>Указания
1. Независимые поставщики программного обеспечения могут предлагать свои лицензированные компоненты на различных номерах SKU или уровнях, например только один узел, до 5 узлов, до 10 узлов и т. д. Когда клиент приобретает продукт, независимый поставщик программного обеспечения предоставляет соответствующий ключ продукта. Он также может предоставить контейнер больших двоичных объектов службы хранилища Azure, который содержит скрипт установки и связанные с ним файлы. Клиенты могут скопировать эти файлы в собственный контейнер хранилища и изменять их, используя собственный ключ продукта (например, запустив `IsvSetup.exe -pid xxxx-xxxx-xxxx`). Клиенты могут подготовить или перенастроить среду выполнения интеграции Azure SSIS, используя URI SAS их контейнера в качестве параметра. Дополнительные сведения см. в статье о [пользовательской установке для среды выполнения интеграции Azure SSIS](how-to-configure-azure-ssis-ir-custom-setup.md).

2. После подготовки и перенастройки среды выполнения интеграции Azure SSIS пакет установки независимого поставщика программного обеспечения выполняется на каждом узле для запроса переменных среды `SSIS_CLUSTERID` и `SSIS_CLUSTERNODECOUNT`. Затем среда выполнения интеграции Azure SSIS отправляет свой идентификатор кластера и ключ лицензированного продукта на сектор активации независимого поставщика программного обеспечения для создания ключа активации.

3. После получения ключа активации пакет установки независимого поставщика программного обеспечения может хранить ключ локально в каждом узле (например, в реестре).

4. Когда клиенты запускают пакет, использующий лицензированный компонент независимого поставщика программного обеспечения, в узле среды выполнения интеграции Azure SSIS, он считывает локально хранимый ключ активации и проверяет его с помощью идентификатора кластера узла. Кроме того, пакет также может сообщать количество узлов кластера на сервер активации независимого поставщика программного обеспечения.

    Ниже приведен пример кода, который проверяет ключ активации и сообщает о количестве узлов кластера:

    ```csharp
    public override DTSExecResult Validate(Connections, VariableDispenser, IDTSComponentEvents componentEvents, IDTSLogging log) 
                                                                                                                               
    {                                                                                                                             
                                                                                                                               
    Variables vars = null;                                                                                                        
                                                                                                                               
    variableDispenser.LockForRead("System::ClusterID");                                                                           
                                                                                                                               
    variableDispenser.LockForRead("System::ClusterNodeCount");                                                                    
                                                                                                                               
    variableDispenser.GetVariables(ref vars);                                                                                     
                                                                                                                               
    // Validate Activation Key with ClusterID                                                                                     
                                                                                                                               
    // Report on ClusterNodeCount                                                                                                 
                                                                                                                               
    vars.Unlock();                                                                                                                
                                                                                                                               
    return base.Validate(connections, variableDispenser, componentEvents, log);                                                   
                                                                                                                               
    }
    ```

## <a name="isv-partners"></a>Независимые партнеры-поставщики программного обеспечения

Список независимых партнеров-поставщиков программного обеспечения, которые адаптировали свои компоненты и расширения для среды выполнения интеграции Azure–SSIS, можно найти в конце этого блога — [Enterprise Edition, пользовательская установка и сторонние расширения для служб SSIS в ADF](https://blogs.msdn.microsoft.com/ssis/2018/04/27/enterprise-edition-custom-setup-and-3rd-party-extensibility-for-ssis-in-adf/).

## <a name="next-steps"></a>Дальнейшие действия

-   [Пользовательская установка для среды выполнения интеграции Azure SSIS](how-to-configure-azure-ssis-ir-custom-setup.md)

-   [Выпуск Enterprise Edition среды выполнения интеграции Azure SSIS](how-to-configure-azure-ssis-ir-enterprise-edition.md)
