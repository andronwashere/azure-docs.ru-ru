---
title: Защита устройств и ПО с помощью диспетчера безопасности Azure IoT Edge | Документация Майкрософт
description: Управляет состоянием безопасности устройства IoT Edge и целостностью служб безопасности.
services: iot-edge
keywords: безопасность, безопасный элемент, анклава, TEE, IoT Edge
author: eustacea
manager: philmea
ms.author: eustacea
ms.date: 07/30/2018
ms.topic: article
ms.service: iot-edge
ms.custom: seodec18
ms.openlocfilehash: bc441e2bbd36c8d078eb67aff48e58684a026289
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "60444998"
---
# <a name="azure-iot-edge-security-manager"></a>Диспетчер безопасности Azure IoT Edge

Диспетчер безопасности Azure IoT Edge — это хорошо ограниченное ядро обеспечения ​​безопасности для защиты устройства IoT Edge и всех его компонентов путем абстрагирования защищенного кремниевого оборудования. Он является основным средством усиления безопасности и обеспечивает интеграцию технологий с оригинальными изготовителями устройств (поставщиками вычислительной техники).

![Диспетчер безопасности Azure IoT Edge](media/edge-security-manager/iot-edge-security-manager.png)

Диспетчер безопасности IoT Edge предназначен для защиты целостности устройства IoT Edge и всех встроенных программных операций.  Это достигается путем передачи базового корня доверия оборудования (если доступно), чтобы безопасно выполнить загрузку среды выполнения IoT Edge и продолжать следить за целостностью операций.  По сути, диспетчер безопасности IoT Edge — это программное обеспечение, работающее вместе с безопасным оборудованием с микросхемами (где это доступно) и позволяющее обеспечить максимально возможные гарантии безопасности.  

В задачи диспетчера безопасности IoT Edge входят (это не исчерпывающий список):

* Безопасная и измеренная начальная загрузка устройства Azure IoT Edge.
* Подготовка удостоверения устройства и передача доверия, где это применимо.
* Размещение и защита компонентов облачных служб для устройства, например службы подготовки устройств.
* Безопасная подготовка модулей IoT Edge с уникальными идентификаторами.
* Хранение корня доверия оборудования с помощью нотариальных служб.
* Мониторинг целостности операций IoT Edge во время выполнения.

Диспетчер безопасности IoT Edge состоит из трех компонентов:

* управляющая программа безопасности IoT Edge;
* уровень абстракции платформы аппаратного модуля безопасности (HSM);
* необязательный, но очень рекомендуемый корень доверия кремниевого оборудования или HSM.

## <a name="the-iot-edge-security-daemon"></a>Управляющая программа безопасности IoT Edge

Управляющая программа безопасности IoT Edge — это программное обеспечение, отвечающее за логические операции диспетчера безопасности IoT Edge. Эта программа — значительная часть доверенной вычислительной базы устройства IoT Edge. 

### <a name="design-principles"></a>Принципы проектирования

Управляющая программа безопасности IoT Edge придерживается двух основных принципов: максимальное увеличение операционной целостности, минимизация раздувания и обновления.

#### <a name="maximize-operational-integrity"></a>Максимальное увеличение операционной целостности

Управляющая программа безопасности IoT Edge обеспечивает самый высокий уровень целостности в рамках защиты любого корня доверия оборудования. При правильной интеграции корень доверия оборудования проверяет и отслеживает управляющую программу безопасности в статическом состоянии и во время выполнения, чтобы противостоять незаконному изменению.

Физический доступ всегда является угрозой для устройств Интернета вещей. Корень доверия оборудования играет важную роль в защите целостности управляющей программы безопасности IoT Edge.  Корень доверия оборудования представлен в двух вариантах:

* безопасные элементы для защиты конфиденциальной информации, такой как секреты и криптографические ключи;
* безопасные внутренние зоны для защиты секретов, например ключей, и уязвимых рабочих нагрузок, таких как выставление счетов и ценообразование.

Существуют два типа сред выполнения для использования корня доверия оборудования:

* стандартная или расширенная среда выполнения (REE), которая использует безопасные элементы для защиты конфиденциальной информации;
* доверенная среда выполнения (TEE), которая использует безопасные технологии "огораживания" для защиты конфиденциальной информации и обеспечения защиты выполнения программного обеспечения.

Предполагается, что для устройств, использующих защищенные закрытые зоны в качестве корня доверия оборудования, конфиденциальная логика в управляющей программе безопасности IoT Edge будет находиться внутри такой зоны.  Неконфиденциальные части управляющей программы безопасности могут находиться за пределами TEE.  В любом случае от производителей систем собственной разработки и поставщиков вычислительной техники ожидается увеличение доверия со стороны их HSM для измерения и защиты целостности управляющей программы безопасности IoT Edge при загрузке и во время выполнения.

#### <a name="minimize-bloat-and-churn"></a>Минимизация раздувания и обновления

Другой основной принцип управляющей программы безопасности IoT Edge заключается в минимизации обновлений.  Для высшего уровня доверия управляющая программа безопасности IoT Edge может тесно соединяться с корнем доверия оборудования устройства и работать как собственный код.  Обычно эти типы реализаций обновляют программное обеспечение управляющей программы с помощью защищенных путей обновления корня доверия (в отличие от механизмов, предоставляемых ОС), что может быть сложно в зависимости от конкретного оборудования и сценария развертывания.  Хотя обновление безопасности настоятельно рекомендуется для устройств IoT, разумно предположить, что чрезмерные требования к обновлению или объемные полезные данные обновления могут значительно расширить уровень угрозы.  К примерам относятся пропуски обновлений, чтобы увеличить операционную доступность, или корень доверия устройства, слишком ограниченный, чтобы обрабатывать объемные полезные данные обновления.  Таким образом, структура управляющей программы безопасности IoT Edge является компактной, чтобы поддерживать небольшой объем и, следовательно, небольшую доверенную вычислительную базу и минимизировать требования к обновлению.

### <a name="architecture-of-iot-edge-security-daemon"></a>Архитектура управляющей программы безопасности IoT Edge

![Управляющая программа безопасности Azure IoT Edge](media/edge-security-manager/iot-edge-security-daemon.png)

Управляющая программа безопасности IoT Edge создается таким образом, чтобы поддерживать любую доступную технологию корня доверия оборудования для повышения безопасности.  Она также поддерживает раздельные операции между стандартной (расширенной) средой выполнения и доверенной средой выполнения (TEE), если аппаратные технологии предлагают среды TEE. Специфические для конкретной роли интерфейсы гарантируют взаимодействие основных компонентов IoT Edge для обеспечения целостности устройства IoT Edge и его операций.

#### <a name="cloud-interface"></a>Облачный интерфейс

Облачный интерфейс позволяет управляющей программе безопасности IoT Edge получать доступ к облачным службам, таким как облачные дополнения к безопасности устройства, например обновление безопасности.  К примеру, управляющая программа безопасности IoT Edge в настоящее время использует такой интерфейс для доступа к [службе подготовки устройств к добавлению в Центр Интернета вещей](https://docs.microsoft.com/azure/iot-dps/) для управления жизненным циклом удостоверений устройств.  

#### <a name="management-api"></a>API управления

Управляющая программа безопасности IoT Edge предлагает API управления, который вызывается агентом IoT Edge при создании, запуске, остановке и удалении модуля Edge. Управляющая программа безопасности IoT Edge хранит "регистрации" всех активных модулей. Эти регистрации сопоставляют идентификатор модуля с некоторыми его свойствами. Примерами этих свойств являются идентификатор процесса (pid), выполняющегося в контейнере, или хэш содержимого контейнера Docker.

Эти свойства используются API рабочей нагрузки (описанным ниже), чтобы подтвердить, что вызывающий агент уполномочен выполнять действие.

API управления — это привилегированный API, вызываемый только из агента IoT Edge.  Так как управляющая программа безопасности IoT Edge обеспечивает начальную загрузку и запускает агент IoT Edge, она может создать неявную регистрацию для агента IoT Edge после того, как подтвердит, что агент IoT Edge не был незаконно изменен. Тот же процесс аттестации, который применяет API рабочей нагрузки, используется для предоставления доступа к API управления только для агента IoT Edge.

#### <a name="container-api"></a>API контейнера

Управляющая программа безопасности IoT Edge предоставляет интерфейс контейнера для взаимодействия с используемой системой контейнеров, например Moby и Docker, для создания экземпляра модуля.

#### <a name="workload-api"></a>API рабочей нагрузки

API рабочей нагрузки представляет собой API-интерфейс управляющей программы безопасности IoT Edge, доступный для всех модулей, включая агент IoT Edge. Он предоставляет подтверждение подлинности в форме подписанного токена с корнем HSM или в форме сертификата X509 и соответствующего пакета доверия модулю. Пакет доверия содержит сертификаты ЦС для всех других серверов, которым должны доверять модули.

Управляющая программа безопасности IoT Edge использует процесс аттестации для защиты этого API. Когда модуль вызывает этот API, управляющая программа безопасности IoT Edge пытается найти регистрацию для соответствующего идентификатора. В случае обнаружения она использует свойства регистрации для измерения модуля. Если результат процесса измерения совпадает с регистрацией, создается подписанный токен с корнем HSM или сертификат X509. Соответствующие сертификаты ЦС (пакет доверия) возвращаются в модуль.  Модуль использует этот сертификат для подключения к Центру Интернета вещей, другим модулям или запуска сервера. Когда срок действия подписанного токена или сертификата истекает, модуль должен запросить новый сертификат. 

### <a name="integration-and-maintenance"></a>Интеграция и обслуживание

Корпорация Майкрософт поддерживает основную базу кода для управляющей программы безопасности [IoT Edge на GitHub](https://github.com/Azure/iotedge/tree/master/edgelet).

#### <a name="installation-and-updates"></a>Установка и обновления

Установкой и обновлением управляющей программы безопасности IoT Edge управляет система управления пакетами операционной системы. Устройства IoT Edge с корнем доверия оборудования должны обеспечить дополнительное улучшение целостности управляющей программы, управляя жизненным циклом устройства с помощью систем управления загрузкой и обновлением.  Разработчики устройств могут исследовать эти возможности в соответствии с функциями их устройств.

#### <a name="versioning"></a>Управление версиями

Среда выполнения IoT Edge отслеживает и сообщает версию управляющей программы безопасности IoT Edge. Версия сообщается как атрибут *среда выполнения.платформа.версия* передаваемого свойства модуля агента IoT Edge.

### <a name="hardware-security-module-platform-abstraction-layer-hsm-pal"></a>Уровень абстракции платформы аппаратного модуля безопасности (HSM)

Уровень абстракции платформы HSM абстрагирует все корни доверия оборудования, чтобы избавить разработчика или пользователя IoT Edge от сложностей.  Он включает в себя комбинацию интерфейсов API и процедур взаимодействия между доменами, например связь между стандартной средой выполнения и защищенной внутренней зоной.  Фактическая реализация уровня абстракции платформы HSM зависит от конкретного используемого защищенного оборудования. Его существование позволяет использовать практически любое безопасное оборудование с микросхемами.

## <a name="secure-silicon-root-of-trust-hardware"></a>Корень доверия надежного кремниевого оборудования

Надежное кремниевое оборудование необходимо для закрепления доверия внутри аппаратного устройства IoT Edge.  Существует большое количество надежного кремниевого оборудования, включая доверенный платформенный модуль (TPM), встроенные защищенные элементы (eSM), ARM Trustzone, Intel SGX, а также настраиваемые технологии надежного кремниевого оборудования.  Использование корня доверия надежного кремниевого оборудования в устройствах настоятельно рекомендуется из-за угроз, связанных с физической доступностью устройств Интернета вещей.

## <a name="iot-edge-security-manager-integration-and-maintenance"></a>Интеграция и обслуживание диспетчера безопасности IoT Edge

Цель диспетчера безопасности IoT Edge — определить и изолировать компоненты, которые повышают безопасность и обеспечивают целостность платформы Azure IoT Edge. Третьи стороны, например изготовители, должны применять пользовательские функции безопасности, доступные для оборудования их устройств.  Изучите следующий раздел со ссылками, демонстрирующими усиление защиты диспетчера безопасности Интернета вещей Azure с помощью доверенного платформенного модуля (TPM) на платформах Linux и Windows. В этих примерах используется программное обеспечение или виртуальные модули TPM, но они непосредственно применяются к дискретным устройствам TPM.  

## <a name="next-steps"></a>Дальнейшие действия

Просмотрите блог, посвященный [обеспечению безопасности интеллектуальной границы](https://azure.microsoft.com/blog/securing-the-intelligent-edge/).

Перейдите к созданию и подготовке устройства IoT Edge с помощью [виртуального доверенного платформенного модуля на виртуальной машине Linux](how-to-auto-provision-simulated-device-linux.md).

Перейдите к созданию и подготовке устройства IoT Edge с помощью [имитированного доверенного платформенного модуля в Windows](how-to-auto-provision-simulated-device-windows.md).
