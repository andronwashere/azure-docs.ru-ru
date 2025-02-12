---
title: Облачное устройство StorSimple (обновление 3) | Документация Майкрософт
description: Описание способов создания, развертывания и администрирования облачного устройства StorSimple в виртуальной сети Microsoft Azure. (Применимо к StorSimple с обновлением 3 и более поздних версий.)
services: storsimple
documentationcenter: ''
author: alkohli
manager: timlt
editor: ''
ms.assetid: ''
ms.service: storsimple
ms.devlang: NA
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/08/2017
ms.author: alkohli
ms.openlocfilehash: df7866d4f87f55523e8139232e48d81cb17c80e4
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "62117339"
---
# <a name="deploy-and-manage-a-storsimple-cloud-appliance-in-azure-update-3-and-later"></a>Развертывание и администрирование облачного устройства StorSimple в Azure (обновление 3 и более поздние версии)

## <a name="overview"></a>Обзор

Облачное устройство StorSimple серии 8000 — это дополнительная функция, поставляемая вместе с решением Microsoft Azure StorSimple. Облачное устройство StorSimple работает в виртуальной машине в виртуальной сети Microsoft Azure, и вы можете использовать его для резервного копирования и клонирования данных с узлов.

В этой статье описывается поэтапный процесс развертывания облачного устройства StorSimple в Azure, а также управление им. После прочтения этой статьи вы узнаете:

* Чем облачное устройство отличается от физического.
* Как создавать и настраивать облачные устройства.
* Как подключаться к облачному устройству.
* Как работать с облачным устройством.

Сведения в этом руководстве относятся ко всем облачным устройствам StorSimple с обновлением версии 3 или более поздней.

#### <a name="cloud-appliance-model-comparison"></a>Сравнение моделей облачного устройства

Облачное устройство StorSimple доступно в двух моделях — 8010 (ценовая категория "Стандартный"; ранее представлена как 1100) и 8020 (ценовая категория "Премиум"; впервые представлена в обновлении версии 2). В следующей таблице представлено сравнение двух моделей.

| Модель устройства | 8010<sup>1</sup> | 8020 |
| --- | --- | --- |
| **Максимальная емкость** |30 TБ |64 ТБ |
| **Azure** |Standard_A3 (4 ядра, 7 ГБ памяти)| Standard_DS3 (4 ядра, 14 ГБ памяти)|
| **Регионы доступности** |Все регионы Azure |Регионы Azure, которые поддерживают хранилища класса Premium и виртуальные машины DS3 Azure<br></br>Используйте [этот список](https://azure.microsoft.com/regions/services/), чтобы узнать, доступны ли в вашем регионе **виртуальные машины серии DS** и соответствующие **хранилища дисков**. |
| **Тип хранилища** |Использует хранилище Azure класса Standard для локальных дисков<br></br> Узнайте, как [создать учетную запись хранения класса Standard](../storage/common/storage-create-storage-account.md) |Использует хранилище Azure класса Premium для локальных дисков<sup>2</sup> <br></br> |
| **Руководство по рабочим нагрузкам** |Извлечение файлов уровня элемента из резервных копий |Сценарии разработки и тестирования в облаке <br></br>Низкая задержка и рабочие нагрузки с более высокой производительностью<br></br>Дополнительное устройство для аварийного восстановления |

<sup>1</sup> *Ранее известное как 1100*.

<sup>2</sup> *Как 8010, так и 8020 используют для облачного уровня хранилище Azure класса Standard. Разница существует только на локальном уровне в пределах устройства*.

## <a name="how-the-cloud-appliance-differs-from-the-physical-device"></a>Чем облачное устройство отличается от физического.

Облачное устройство StorSimple — это программная версия StorSimple, которая выполняется на одном узле в виртуальной машине Microsoft Azure. Облачное устройство поддерживает сценарии аварийного восстановления, в которых физическое устройство недоступно. Облачное устройство подходит для извлечения элементов из резервных копий, локального аварийного восстановления и использования в сценариях облачной разработки и тестирования.

#### <a name="differences-from-the-physical-device"></a>Отличия от физического устройства

В следующей таблице приведены основные различия между облачными и физическими устройствами StorSimple.

|  | Физическое устройство | Облачное устройство |
| --- | --- | --- |
| **Location** |Размещается в центре обработки данных. |Выполняется в Azure. |
| **Сетевые интерфейсы** |Имеет шесть сетевых интерфейсов: DATA 0 — DATA 5. |Имеет только один сетевой интерфейс: DATA 0. |
| **Регистрация** |Регистрируется во время начальной настройки. |Регистрация представляет собой отдельную задачу. |
| **Ключ шифрования данных службы** |Повторно создается на физическом устройстве, а затем облачное устройство обновляется с помощью нового ключа. |Повторное создание из облачного устройства невозможно. |
| **Поддерживаемые типы томов** |Поддерживает локально закрепленные и многоуровневые тома. |Поддерживает только многоуровневые тома. |

## <a name="prerequisites-for-the-cloud-appliance"></a>Предварительные требования для облачного устройства

В следующих разделах разъясняются предварительные условия для настройки облачного устройства StorSimple. Перед развертыванием облачного устройства ознакомьтесь с вопросами безопасности использования облачных устройств.

[!INCLUDE [StorSimple Cloud Appliance security](../../includes/storsimple-8000-cloud-appliance-security.md)]

#### <a name="azure-requirements"></a>Требования Azure

Перед подготовкой облачного устройства вам необходимо будет сделать следующее в среде Azure.

* У вас должно быть физическое устройство StorSimple серии 8000 (модель 8100 или 8600), развернутое и выполняющееся в центре обработки данных. Зарегистрируйте это устройство с помощью той же службы диспетчера устройств StorSimple, для которой вы хотите создать облачное приложение StorSimple.
* [Настройте виртуальную сеть в Azure](../virtual-network/manage-virtual-network.md#create-a-virtual-network) для облачного устройства. При использовании хранилища класса Premium необходимо создать виртуальную сеть в регионе Azure, поддерживающем хранилище класса Premium. Регионы хранилища класса Premium — это регионы, соответствующие строке "Дисковый накопитель" в списке [служб Azure по регионам](https://azure.microsoft.com/regions/services/).
* Мы советуем использовать DNS-сервер по умолчанию, предоставленный Azure, вместо того чтобы указывать имя собственного DNS-сервера. Если DNS-имя сервера не является допустимым или если DNS-сервер не может правильно разрешать IP-адреса, создание облачного устройства завершится с ошибкой.
* Подключения типа "точка-сеть" и "сеть-сеть" являются необязательными. При необходимости можно настроить эти параметры для более сложных сценариев.
* Вы можете создать [виртуальные машины Azure](../virtual-machines/virtual-machines-windows-quick-create-portal.md) (серверы узла) в виртуальной сети, которая может использовать тома, предоставляемые облачным устройством. Эти серверы должны соответствовать следующим требованиям.

  * Они должны являться виртуальными машинами Windows или Linux с установленным программным обеспечением инициатора iSCSI.
  * Они должны работать в той же виртуальной сети, что и облачное устройство.
  * Они должны иметь возможность подключения к платформе iSCSI облачного устройства по внутреннему IP-адресу облачного устройства.
  * Убедитесь, что вы настроили поддержку iSCSI и облачный трафик в одной виртуальной сети.

#### <a name="storsimple-requirements"></a>Требования StorSimple

Внесите следующие изменения в службу Azure StorSimple перед созданием облачного устройства.

* Добавьте [записи контроля доступа](storsimple-8000-manage-acrs.md) для виртуальных машин, которые будут серверами узла для облачного устройства.
* Используйте [учетную запись хранения](storsimple-8000-manage-storage-accounts.md#add-a-storage-account), которая находится в том же регионе, что и облачное устройство. Если учетные записи хранения находятся в разных регионах, это может привести к сильному понижению производительности. С облачным устройством можно использовать учетную запись хранения класса Standard или Premium. Дополнительные сведения о том, как [создать учетную запись хранения класса Standard](../storage/common/storage-create-storage-account.md).
* Для создания облачного устройства используйте учетную запись хранения, отличную от той, которая используется для ваших данных. Использование той же учетной записи хранения может привести к сильному понижению производительности.

Перед началом работы убедитесь, что у вас есть следующие сведения.

* Учетная запись на портале Azure и учетные данные для доступа.
* Копия ключа шифрования данных службы, полученная с физического устройства, зарегистрированного в службе диспетчера устройств StorSimple.

## <a name="create-and-configure-the-cloud-appliance"></a>Создание и настройка облачного устройства

Перед выполнением этих процедур убедитесь, что соблюдены [предварительные условия для облачного устройства](#prerequisites-for-the-cloud-appliance).

Для создания облачного устройства StorSimple выполните следующие действия.

### <a name="step-1-create-a-cloud-appliance"></a>Шаг 1. Создание облачного устройства

Для создания облачного устройства StorSimple выполните следующие действия.

[!INCLUDE [Create a cloud appliance](../../includes/storsimple-8000-create-cloud-appliance-u2.md)]

Если на этом этапе создания облачного устройства происходит сбой, подключение к Интернету может отсутствовать. Дополнительные сведения см. в разделе [Поиск и устранение ошибок подключения к Интернету](#troubleshoot-internet-connectivity-errors) при создании облачного устройства.

### <a name="step-2-configure-and-register-the-cloud-appliance"></a>Шаг 2. Настройка и регистрация облачного устройства

Перед тем как начать эту процедуру, убедитесь, что у вас есть копия ключа шифрования данных службы. Ключ шифрования данных службы создается при регистрации первого физического устройства StorSimple в службе диспетчера устройств StorSimple. Его рекомендовалось сохранить в защищенном месте. Если у вас нет копии ключа шифрования данных службы, следует обратиться в службу поддержки Майкрософт за помощью.

Выполните следующие действия, чтобы настроить и зарегистрировать облачное устройство StorSimple.

[!INCLUDE [Configure and register a cloud appliance](../../includes/storsimple-8000-configure-register-cloud-appliance.md)]

### <a name="step-3-optional-modify-the-device-configuration-settings"></a>Шаг 3. (необязательный). Изменение параметров конфигурации устройства

В следующем разделе описаны параметры конфигурации устройства, которые необходимо настроить для облачного устройства StorSimple, если вы хотите использовать CHAP, диспетчер моментальных снимков StorSimple или изменить пароль администратора устройства.

#### <a name="configure-the-chap-initiator"></a>Настройка инициатора CHAP

Этот параметр содержит учетные данные, которые облачное устройство (целевое устройство) ожидает от инициаторов (серверов), пытающихся получить доступ к томам. Инициаторы предоставят имя пользователя и пароль CHAP для собственной идентификации на устройстве во время проверки подлинности. Подробные инструкции см. в статье о [настройке CHAP для устройства](storsimple-8000-configure-chap.md#unidirectional-or-one-way-authentication).

#### <a name="configure-the-chap-target"></a>Настройка целевого устройства CHAP

Этот параметр содержит учетные данные, которые облачное устройство использует при запросе взаимной или двунаправленной проверки подлинности инициатором с поддержкой CHAP. Ваше облачное устройство будет использовать обратные имя пользователя и пароль CHAP для собственной идентификации в инициаторе во время проверки подлинности.

> [!NOTE]
> Параметры целевого устройства CHAP являются глобальными. При их применении все тома, подключенные к облачному устройству, будут использовать проверку подлинности CHAP.

Подробные инструкции см. в статье о [настройке CHAP для устройства](storsimple-8000-configure-chap.md#bidirectional-or-mutual-authentication).

#### <a name="configure-the-storsimple-snapshot-manager-password"></a>Настройка пароля диспетчера моментальных снимков StorSimple

Программное обеспечение диспетчера моментальных снимков StorSimple находится на узле Windows и позволяет администраторам управлять созданием резервных копий для устройства StorSimple в виде локальных и облачных моментальных снимков.

> [!NOTE]
> Для облачного устройства узел Windows — это виртуальная машина Azure.

При настройке устройства в диспетчере моментальных снимков StorSimple вас попросят указать IP-адрес и пароль устройства StorSimple для проверки подлинности устройства для хранения данных. Подробные инструкции см. в статье [Изменение паролей StorSimple с помощью службы диспетчера StorSimple](storsimple-8000-change-passwords.md#set-the-storsimple-snapshot-manager-password).

#### <a name="change-the-device-administrator-password"></a>Изменение пароля администратора устройства

При использовании интерфейса Windows PowerShell для доступа к облачному устройству вам потребуется ввести пароль администратора устройства. Для обеспечения безопасности ваших данных необходимо изменить этот пароль перед тем, как использовать облачное устройство. Подробные инструкции см. в статье [Изменение паролей StorSimple с помощью службы диспетчера StorSimple](../storsimple/storsimple-8000-change-passwords.md#change-the-device-administrator-password).

## <a name="connect-remotely-to-the-cloud-appliance"></a>Удаленное подключение к облачному устройству

Удаленный доступ к облачному устройству в интерфейсе Windows PowerShell по умолчанию отключен. Необходимо сначала настроить удаленное управление на облачном устройстве, а затем включить его в клиенте, который будет использоваться для доступа к устройству.

Ниже описана двухэтапная процедура установки удаленного подключения к облачному устройству.

### <a name="step-1-configure-remote-management"></a>Шаг 1. Настройка удаленного управления

Выполните следующие действия, чтобы настроить удаленное управление для облачного устройства StorSimple.

[!INCLUDE [Configure remote management via HTTP for cloud appliance](../../includes/storsimple-8000-configure-remote-management-http-device.md)]

### <a name="step-2-remotely-access-the-cloud-appliance"></a>Шаг 2. Удаленный доступ к облачному устройству

После включения удаленного управления на облачном устройстве используйте удаленное взаимодействие Windows PowerShell для подключения к устройству с другой виртуальной машины в рамках одной виртуальной сети. Например, вы можете подключиться с виртуальной машины узла, которую вы настроили и использовали для подключения iSCSI. В большинстве развертываний у вас уже есть открытая общедоступная конечная точка для доступа к виртуальной машине-хосту, которую можно использовать для доступа к облачному устройству.

> [!WARNING]
> **Для повышения уровня безопасности настоятельно рекомендуется использовать протокол HTTPS при подключении к конечным точкам и удалении конечных точек после завершения удаленного сеанса PowerShell.**

Процедуры настройки удаленного взаимодействия на облачном устройстве описаны в статье [Удаленное подключение к устройству StorSimple серии 8000](storsimple-8000-remote-connect.md).

## <a name="connect-directly-to-the-cloud-appliance"></a>Прямое подключение к облачному устройству

Также можно подключиться непосредственно к облачному устройству. Чтобы подключиться напрямую к облачному устройству с другого компьютера, находящегося за пределами виртуальной сети или за пределами среды Microsoft Azure, необходимо создать дополнительные конечные точки.

Выполните следующие действия, чтобы создать общедоступную конечную точку на облачном устройстве.

[!INCLUDE [Create public endpoints on a cloud appliance](../../includes/storsimple-8000-create-public-endpoints-cloud-appliance.md)]

Рекомендуется подключиться с другой виртуальной машины внутри той же виртуальной сети, так как это минимизирует число общедоступных конечных точек в виртуальной сети. В этом случае подключитесь к виртуальной машине с помощью сеанса удаленного рабочего стола, а затем настройте виртуальную машину так же, как любой другой клиент Windows в локальной сети. Вам не нужно добавлять номер общедоступного порта, так как порт будет уже известен.

## <a name="get-private-ip-for-the-cloud-appliance"></a>Получение частного IP-адреса для облачного устройства

Для подключения облачного устройства к серверу узла на той же виртуальной машине вам потребуется внутренний (частный) IP-адрес облачного устройства. Чтобы получить частный IP-адрес облачного устройства, выполните следующие действия.

1. Перейдите к базовой виртуальной машине для облачного устройства. Имя виртуальной машины такое же, как и у облачного устройства. Перейдите к разделу **Все ресурсы**, укажите имя облачного устройства и имя подписки, а затем выберите тип "Виртуальные машины". В представленном списке виртуальных машин выберите и щелкните виртуальную машину, которая соответствует облачному устройству.

     ![Выбор виртуальной машины для облачного устройства](./media/storsimple-8000-cloud-appliance-u2/sca-vm.png)

2. Последовательно выберите **Параметры > Сеть**. В области справа появится частный IP-адрес облачного устройства. Запишите его.

    ![Получение частного IP-адреса для виртуальной машины](./media/storsimple-8000-cloud-appliance-u2/sca-private-ip-vm-networking.png)

## <a name="work-with-the-storsimple-cloud-appliance"></a>Работа с облачным устройством StorSimple

После создания и настройки облачного устройства StorSimple вы можете начать работу с ним. Работа с контейнерами томов, томами и политиками резервного копирования на облачном устройстве аналогична работе с ними на физическом устройстве StorSimple. Единственное отличие состоит в том, что из списка устройств нужно выбрать облачное устройство. Ознакомьтесь со сведениями в статье [Администрирование виртуального массива StorSimple с помощью службы диспетчера устройств StorSimple](storsimple-8000-manager-service-administration.md), чтобы получить пошаговые процедуры для различных задач управления облачным устройством.

В следующих разделах рассматриваются некоторые отличия, с которыми вы столкнетесь при работе с облачным устройством.

### <a name="maintain-a-storsimple-cloud-appliance"></a>Обслуживание облачного устройства StorSimple

Так как это программное устройство, обслуживание облачного устройства сведено к минимуму по сравнению с обслуживанием физического устройства.

Вы не можете обновить облачное устройство. Чтобы создать облачное устройство, используйте последнюю версию программного обеспечения.


### <a name="storage-accounts-for-a-cloud-appliance"></a>Учетные записи хранения для облачного устройства

Учетные записи хранения используются в службе диспетчера устройств StorSimple, на облачных и физических устройствах. При создании учетных записей хранения в понятном имени рекомендуется использовать идентификатор региона. Это упростит обеспечение использования одного и того же региона во всех компонентах системы. Для облачного устройства важно, чтобы все компоненты находились в одном регионе, чтобы предотвратить понижение производительности.

Описание пошаговой процедуры см. в разделе [Добавление учетной записи хранения](storsimple-8000-manage-storage-accounts.md#add-a-storage-account).

### <a name="deactivate-a-storsimple-cloud-appliance"></a>Деактивация облачного устройства StorSimple

При деактивации облачного устройства будет удалена виртуальная машина и ресурсы, созданные при ее подготовке. После деактивации облачного устройства его невозможно вернуть в предыдущее состояние. Перед деактивацией облачного устройства убедитесь, что остановили или удалили клиенты и узлы, зависящие от него.

Деактивация облачного устройства приведет к следующим действиям:

* Облачное устройство будет удалено.
* Диск ОС и диски данных, созданные для облачного устройства, будут удалены.
* Размещенная служба и виртуальная сеть, созданные во время подготовки, будут сохранены. Если вы не используете их, удалите их вручную.
* Облачные моментальные снимки, созданные для облачного устройства, будут сохранены.

Описание пошаговой процедуры см. в статье [Отключение и удаление устройства StorSimple](storsimple-8000-deactivate-and-delete-device.md).

После того как облачное устройство будет показано как деактивированное в колонке службы диспетчера StorSimple, вы сможете удалить облачное устройство из списка устройств в колонке **Устройства**.

### <a name="start-stop-and-restart-a-cloud-appliance"></a>Запуск, остановка и перезапуск облачного устройства
В отличие от физического устройства StorSimple на облачном устройстве StorSimple нет кнопки включения и выключения питания. Однако иногда возникают ситуации, когда необходимо остановить и перезапустить облачное устройство.

Самый простой способ запуска, остановки и перезапуска облачного устройства — использовать колонку службы "Виртуальные машины". Перейдите к службе виртуальных машин. В списке виртуальных машин определите виртуальную машину, соответствующую облачному устройству (аналогичное имя), и щелкните ее имя. Состояние облачного устройства в колонке виртуальной машины — **Работает**, так как оно запускается по умолчанию после создания. Вы можете запустить, остановить и перезапустить виртуальную машину в любое время.

[!INCLUDE [Stop and restart cloud appliance](../../includes/storsimple-8000-stop-restart-cloud-appliance.md)]

### <a name="reset-to-factory-defaults"></a>Сброс к параметрам по умолчанию
Если вы хотите заново настроить облачное устройство, просто деактивируйте и удалите его, а затем создайте новое.

## <a name="fail-over-to-the-cloud-appliance"></a>Отработка отказа на облачное устройство
Аварийное восстановление — это один из основных сценариев, для которых разработано облачное устройство StorSimple. В этом сценарии физическое устройство StorSimple или весь центр обработки данных могут быть недоступны. Однако вы можете использовать облачное устройство для восстановления операций в другом местоположении. Во время аварийного восстановления контейнеры томов из исходного устройства сменят владельца и будут перемещены на облачное устройство.

Предварительные требования для аварийного восстановления:

* Облачное устройство создано и настроено.
* Все тома в контейнере томов находятся в автономном режиме.
* Контейнер томов, для которого необходимо выполнить отработку отказа, должен иметь связанный с ним облачный моментальный снимок.

> [!NOTE]
> * При использовании облачного устройства в качестве вторичного устройства для аварийного восстановления следует помнить, что устройство 8010 имеет 30 ТБ в хранилище класса Standard, а устройство 8020 — 64 ТБ в хранилище класса Premium. Облачное устройство большей емкости 8020 может лучше подходить для сценария аварийного восстановления.

Описание пошаговой процедуры см. в статье [Отработка отказа и аварийное восстановление для устройства StorSimple](storsimple-8000-device-failover-cloud-appliance.md).

## <a name="delete-the-cloud-appliance"></a>Удаление облачного устройства
Если вы ранее настроили и использовали облачное устройство StorSimple, но сейчас хотите остановить выставление счетов за вычисления, связанные с его использованием, необходимо завершить работу облачного устройства. Завершение работы облачного устройства освобождает виртуальную машину. Это остановит выставление счетов для вашей подписки. Однако плата за диск операционной системы и диск с данными продолжит взиматься.

Чтобы остановить выставление всех счетов, необходимо удалить облачное устройство. Чтобы удалить резервные копии, созданные облачным устройством, необходимо отключить или удалить устройство. Дополнительные сведения см. в статье [Отключение и удаление устройства StorSimple](storsimple-8000-deactivate-and-delete-device.md).

[!INCLUDE [Delete a cloud appliance](../../includes/storsimple-8000-delete-cloud-appliance.md)]

## <a name="troubleshoot-internet-connectivity-errors"></a>Поиск и устранение ошибок подключения к Интернету
Если в ходе создания облачного устройства прерывается подключение к Интернету, этот этап завершается ошибкой. Чтобы устранить сбой подключения к Интернету, выполните следующие действия на портале Azure:

1. [Создайте виртуальную машину под управлением Windows на портале Azure.](https://docs.microsoft.com/azure/virtual-machines/windows/quick-create-portal) Важно, чтобы для этой виртуальной машины использовалась та же учетная запись хранения, виртуальная сеть и подсеть, что и для нового облачного устройства. Если в Azure уже имеется узел Windows Server, использующий ту же учетную запись хранения, виртуальную сеть и подсеть, вы можете использовать его для поиска и устранения ошибок подключения к Интернету.
2. Войдите удаленно на виртуальную машину, созданную на предыдущем этапе.
3. Откройте окно командной строки на виртуальной машине (нажмите Win + R, затем введите `cmd`).
4. Выполните в командной строке следующий командлет.

    `nslookup windows.net`
5. Если командлет `nslookup` завершается ошибкой, это значит, что облачное устройство нельзя зарегистрировать в службе диспетчера устройств StorSimple из-за ошибки подключения к Интернету.
6. Внесите необходимые изменения в свою виртуальную сеть, чтобы обеспечить доступ облачного устройства к сайтам Azure, таким как _windows.net_.

## <a name="next-steps"></a>Дальнейшие действия
* Узнайте, как [управлять облачным устройством с помощью службы диспетчера устройств StorSimple](storsimple-8000-manager-service-administration.md).
* Узнайте, как [восстановить том StorSimple из резервного набора данных](storsimple-8000-restore-from-backup-set-u2.md).
