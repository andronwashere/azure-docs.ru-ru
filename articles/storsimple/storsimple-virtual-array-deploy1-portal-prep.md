---
title: Подготовка портала к использованию виртуального массива StorSimple | Документация Майкрософт
description: Первое руководство по развертыванию виртуального массива StorSimple посвящено подготовке портала Azure
services: storsimple
documentationcenter: NA
author: alkohli
manager: jeconnoc
editor: ''
ms.assetid: 68a4cfd3-94c9-46cb-805c-46217290ce02
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 01/11/2019
ms.author: alkohli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 7c6f0a6371b38f0271237db0f7d80b831ecc145c
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "62127146"
---
# <a name="deploy-storsimple-virtual-array---prepare-the-azure-portal"></a>Развертывание виртуального массива StorSimple. Подготовка портала Azure

![](./media/storsimple-virtual-array-deploy1-portal-prep/getstarted4.png)
## <a name="overview"></a>Обзор

Это первая статья в серии руководств по развертыванию, необходимых для полного развертывания виртуального массива в качестве файлового сервера или сервера iSCSI с помощью модели Resource Manager. В этой статье описаны подготовительные работы для создания и настройки службы диспетчера устройств StorSimple, которая является необходимым компонентом для подготовки виртуального массива к работе. Кроме того, здесь представлены ссылки на контрольный список и предварительные требования для настройки развертывания.

Чтобы завершить процесс установки и настройки, вам потребуются права администратора. Перед началом настройки рекомендуется изучить контрольный список для настройки развертывания. Подготовка портала займет менее 10 минут.

Сведения, приведенные в этой статье, относятся к развертыванию виртуальных массивов StorSimple на портале Azure, а также в облаке Microsoft Azure для государственных организаций.

### <a name="get-started"></a>Начало работы
Рабочий процесс развертывания состоит из подготовки портала, подготовки виртуального массива к работе в виртуальной среде и настройки. Чтобы приступить к развертыванию виртуального массива StorSimple в качестве файлового сервера или сервера iSCSI, изучите ресурсы, приведенные в таблице ниже.

#### <a name="deployment-articles"></a>Статьи по развертыванию

Чтобы развернуть виртуальный массив StorSimple, ознакомьтесь с перечисленными ниже статьями в указанном порядке.

| **#** | **Шаг** | **Выполняемые действия** | **Справочная документация** |
| --- | --- | --- | --- |
| 1. |**Настройка портала Azure** |Перед подготовкой виртуального массива StorSimple к работе создайте и настройте службу диспетчера устройств StorSimple. |[Развертывание виртуального массива StorSimple — подготовка портала](storsimple-virtual-array-deploy1-portal-prep.md) |
| 2. |**Подготовка виртуального массива к работе** |Для Hyper-V: подготовьте виртуальный массив StorSimple в главной системе, в которой выполняется Hyper-V на платформе Windows Server 2012 R2, Windows Server 2012 или Windows Server 2008 R2, и подключитесь к нему. <br></br> <br></br> В случае с VMware подготовьте виртуальный массив StorSimple в главной системе под управлением VMware ESXi 5.0, 5.5, 6.0 или 6.5 и установите подключение к нему.<br></br> |[Развертывание виртуального массива StorSimple — подготовка виртуального массива в Hyper-V](storsimple-virtual-array-deploy2-provision-hyperv.md) <br></br> <br></br> [Развертывание виртуального массива StorSimple — подготовка виртуального массива в VMware](storsimple-virtual-array-deploy2-provision-vmware.md) |
| 3. |**Настройка виртуального массива** |Выполните начальную установку файлового сервера StorSimple, зарегистрируйте его и настройте устройство. Затем подготовьте к работе общие папки SMB. <br></br> <br></br> Выполните начальную установку сервера iSCSI, зарегистрируйте его и настройте устройство. Затем подготовьте к работе тома iSCSI. |[Развертывание виртуального массива StorSimple — установка в качестве файлового сервера](storsimple-virtual-array-deploy3-fs-setup.md)<br></br> <br></br>[Развертывание виртуального массива StorSimple — настройка виртуального устройства в качестве сервера iSCSI](storsimple-virtual-array-deploy3-iscsi-setup.md) |

Теперь можно приступать к настройке портала Azure.

## <a name="configuration-checklist"></a>Контрольный список для настройки

В контрольном списке указаны сведения, которые необходимо собрать перед настройкой программного обеспечения в виртуальном массиве StorSimple. Заблаговременная подготовка этих сведений поможет оптимизировать процесс развертывания устройства StorSimple в вашей среде. В зависимости от того, в качестве какого сервера будет развертываться виртуальный массив StorSimple (файловый сервер или сервер iSCSI), вам потребуется один из двух контрольных списков.

* Скачайте [контрольный список для настройки виртуального массива StorSimple в качестве файлового сервера](https://download.microsoft.com/download/E/E/6/EE690BB0-B442-4B84-8165-4731EE727ACF/MicrosoftAzureStorSimpleVirtualArrayFileServerConfigurationChecklist.pdf).
* Скачайте [контрольный список для настройки виртуального массива StorSimple в качестве сервера iSCSI](https://download.microsoft.com/download/E/E/6/EE690BB0-B442-4B84-8165-4731EE727ACF/MicrosoftAzureStorSimpleVirtualArrayiSCSIServerConfigurationChecklist.pdf).

## <a name="prerequisites"></a>Технические условия

Ниже приведены предварительные требования для настройки службы диспетчера устройств StorSimple, виртуального массива StorSimple и сети центра обработки данных.

### <a name="for-the-storsimple-device-manager-service"></a>Предварительные требования для службы диспетчера устройств StorSimple

Перед тем как начать, убедитесь в следующем.

* Имеется учетная запись Майкрософт и данные для доступа к ней.
* Имеется учетная запись хранения Microsoft Azure и данные для доступа к ней.
* В вашей подписке Microsoft Azure настроена служба диспетчера устройств StorSimple.

### <a name="for-the-storsimple-virtual-array"></a>Предварительные требования для виртуального массива StorSimple

Перед развертыванием виртуального массива выполните указанные ниже условия.

* У вас должен быть доступ к главной системе, в которой выполняется Hyper-V на платформе Windows Server 2008 R2 или более поздней версии либо VMware (ESXi 5.0, 5.5, 6.0 или 6.5), которую можно использовать для подготовки устройства.
* Главная система должна быть в состоянии выделить указанный ниже объем ресурсов для подготовки виртуального массива.
  
  * Не менее 4 ядер.
  * Не менее 8 ГБ ОЗУ. Если планируется настроить виртуальный массив как файловый сервер, то 8 ГБ ОЗУ обеспечат поддержку 2 млн файлов. Для поддержки 2–4 млн файлов потребуется 16 ГБ ОЗУ.
  * Один сетевой интерфейс.
  * Виртуальный диск размером 500 ГБ для системных данных.

### <a name="for-the-datacenter-network"></a>Для сети центра обработки данных

Перед тем как начать, убедитесь в следующем.

* Сеть в центре обработки данных настроена в соответствии с требованиями к сетевым характеристикам устройства StorSimple. Дополнительные сведения см. в статье [Системные требования для виртуального массива StorSimplе](storsimple-ova-system-requirements.md).
* Виртуальный массив StorSimple всегда подключен к выделенному интернет-каналу с пропускной способностью не менее 5 Мбит/с. Эту пропускную способность не следует использовать совместно с другими приложениями.

## <a name="step-by-step-preparation"></a>Пошаговая подготовка

Ниже приведены пошаговые инструкции для подготовки портала к использованию службы диспетчера устройств StorSimple.

## <a name="step-1-create-a-new-service"></a>Шаг 1. Создание службы

Один экземпляр службы диспетчера устройств StorSimple может управлять несколькими виртуальными массивами StorSimple. Чтобы создать экземпляр службы диспетчера устройств StorSimple, выполните следующие действия. Если у вас уже есть служба диспетчера устройств, управляющая виртуальными массивами StorSimple, пропустите этот шаг и перейдите к разделу [Шаг 2. Получение ключа регистрации службы](#step-2-get-the-service-registration-key).

[!INCLUDE [storsimple-virtual-array-create-new-service](../../includes/storsimple-virtual-array-create-new-service.md)]

> [!IMPORTANT]
> Если вы не использовали автоматическое создание учетной записи хранения вашей службой, то после успешного создания службы вам нужно будет создать хотя бы одну учетную запись хранения.
> 
> * Если вы не создали учетную запись хранения автоматически, перейдите к разделу [Настройка новой учетной записи хранения для службы](#optional-step-configure-a-new-storage-account-for-the-service) , чтобы ознакомиться с подробными инструкциями.
> * Если вы включили автоматическое создание учетной записи хранения, перейдите к разделу [Шаг 2. Получение ключа регистрации службы](#step-2-get-the-service-registration-key).
> 
> 

## <a name="step-2-get-the-service-registration-key"></a>Шаг 2. Получение регистрационного ключа службы

После запуска и настройки службы диспетчера устройств StorSimple вам необходимо будет получить ключ регистрации службы. Этот ключ используется для регистрации вашего устройства StorSimple в службе и подключения к ней.

На [портале Azure](https://portal.azure.com/) выполните указанные ниже действия.

[!INCLUDE [storsimple-virtual-array-get-service-registration-key](../../includes/storsimple-virtual-array-get-service-registration-key.md)]

> [!NOTE]
> Ключ регистрации службы используется для регистрации всех устройств диспетчера устройств StorSimple, которые необходимо зарегистрировать в службе диспетчера устройств StorSimple.
> 
> 

## <a name="step-3-download-the-virtual-array-image"></a>Шаг 3. Скачивание образа виртуального массива

После создания ключа регистрации службы вам потребуется скачать образ соответствующего виртуального массива, чтобы подготовить виртуальный массив к работе в главной системе. Образы виртуального массива зависят от операционной системы. Их можно скачать на портале Azure на странице быстрого запуска.

> [!IMPORTANT]
> Программное обеспечение, выполняемое на виртуальном массиве StorSimple, можно использовать только со службой диспетчера устройств StorSimple.
> 
> 

На [портале Azure](https://portal.azure.com/) выполните указанные ниже действия.

#### <a name="to-get-the-virtual-array-image"></a>Получение образа виртуального массива

1. Войдите на [портал Azure](https://portal.azure.com/). 
2. На портале Azure щелкните **Больше служб > Диспетчеры устройств StorSimple**.
3. Выберите имеющуюся службу диспетчера устройств StorSimple. В колонке **Диспетчеры устройств StorSimple** щелкните **Быстрый запуск**. 
4. Щелкните ссылку на образ, который вы хотите скачать из Центра загрузки Майкрософт. Размер файла образа составляет около 4,8 ГБ.
   
   * VHDX для Hyper-V в Windows Server 2012 и более поздней версии.
   * VHDX для Hyper-V в Windows Server 2008 R2 и более поздней версии.
   * VMDK для VMWare ESXi 5.0, 5.5, 6.0 и 6.5.
5. Скачайте файл и распакуйте его на локальный диск, запомнив, где находится распакованный файл.

## <a name="optional-step-configure-a-new-storage-account-for-the-service"></a>Необязательный шаг. Настройка новой учетной записи хранения для службы

Это необязательный шаг. Выполните его, только если вы автоматически не создавали учетную запись хранения для службы.

Если требуется создать учетную запись хранения Azure в другом регионе, то см. пошаговые инструкции в разделе [Создайте учетную запись хранения](../storage/common/storage-quickstart-create-account.md).

Чтобы добавить имеющуюся учетную запись хранения Microsoft Azure, на [портале Azure](https://ms.portal.azure.com/) на странице службы диспетчера устройств StorSimple выполните следующие действия.

#### <a name="to-add-a-storage-account-credential-that-has-the-same-azure-subscription-as-the-device-manager-service"></a>Добавление учетных данных учетной записи хранения, которая связана с той же подпиской Azure, что и служба диспетчера устройств

1. Найдите службу диспетчера устройств и щелкните ее дважды. Откроется колонка **Обзор**.
2. В разделе **Конфигурация** выберите **Учетные данные для учетной записи хранения**.
3. Щелкните **Добавить**.
4. В колонке **добавления учетной записи хранения** выполните следующие действия.
   
   1. Для параметра **Подписка** выберите значение **Текущая**.
   
   2. Укажите имя вашей учетной записи хранения.
   
   3. Выберите **Включить**, чтобы создать безопасный канал для обмена данными между вашим устройством StorSimple и облаком. Если используется частное облако, выберите параметр **Отключить**.
   
   4. Щелкните **Добавить**. Когда учетная запись хранения будет создана, вы получите уведомление.<br></br>
   
      ![Добавление учетных данных имеющейся учетной записи хранения](./media/storsimple-virtual-array-manage-storage-accounts/ova-add-storageacct.png)

## <a name="next-step"></a>Дальнейшие действия

Далее необходимо подготовить виртуальную машину для виртуального массива StorSimple. Инструкции зависят от используемой ОС сервера виртуальных машин.

* [Развертывание виртуального массива StorSimple — подготовка виртуального массива в Hyper-V](storsimple-virtual-array-deploy2-provision-hyperv.md)
* [Развертывание виртуального массива StorSimple — подготовка виртуального массива в VMware](storsimple-virtual-array-deploy2-provision-vmware.md)

