---
title: Как установить HANA на сервере SAP HANA в Azure (крупные экземпляры) | Документация Майкрософт
description: Как установить HANA на сервере SAP HANA в Azure (крупные экземпляры).
services: virtual-machines-linux
documentationcenter: ''
author: hermanndms
manager: gwallace
editor: ''
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 09/10/2018
ms.author: rclaus
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: ce82a2972d9cc349e527811e32c974996327e70b
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/09/2019
ms.locfileid: "67709744"
---
# <a name="install-hana-on-sap-hana-on-azure-large-instances"></a>Установка HANA на сервере SAP HANA в Azure (крупные экземпляры)

Чтобы установить HANA на сервере SAP HANA в Azure (крупные экземпляры), для начала необходимо выполнить следующие действия.
- Предоставьте корпорации Майкрософт все данные для развертывания крупного экземпляра SAP HANA.
- Получите от корпорации крупный экземпляр SAP HANA.
- Создайте виртуальную сеть Azure, которая подключается к локальной сети.
- Подключите канал ExpressRotue для крупных экземпляров HANA к той же виртуальной сети Azure.
- Установите виртуальную машину Azure, которая будет использоваться в качестве места перехода для крупных экземпляров HANA.
- Проверьте возможность подключения из места перехода к единице крупного экземпляра HANA и наоборот.
- Проверьте, установлены ли все необходимые пакеты и исправления.
- Прочитайте заметки протокола SAP и документацию по установке HANA в операционной системе, которую вы используете. Убедитесь, что выпуск HANA поддерживается версией операционной системы.

Следующий раздел иллюстрирует пример загрузки установочных пакетов HANA на переходную виртуальную машину. В этом случае используется ОС Windows.

## <a name="download-the-sap-hana-installation-bits"></a>Загрузка установочных пакетов SAP HANA
Единицы крупных экземпляров HANA не подключены к Интернету напрямую. Вы не сможете загрузить установочные пакеты непосредственно с SAP на виртуальную машину крупного экземпляра HANA. Вместо этого пакеты следует загрузить на переходную виртуальную машину.

Необходим S-пользователь SAP или другой пользователь, который позволяет вам получить доступ к SAP Marketplace.

1. Войдите и перейдите к [службе SAP Marketplace](https://support.sap.com/en/index.html). Выберите **Загрузить программное обеспечение** > **Установки и обновления** > **По алфавитному указателю**. Затем в разделе H выберите **SAP HANA Platform Edition** > **SAP HANA Platform Edition 2.0** > **Установка**. Загрузите файлы, как показано на снимке экрана ниже:

   ![Снимок экрана файлов для загрузки](./media/hana-installation/image16_download_hana.PNG)

2. В этом примере мы загрузили установочные пакеты SAP HANA 2.0. На переходной виртуальной машине Azure разверните самораспаковывающиеся архивы в каталог, как показано ниже.

   ![Снимок экрана самораспаковывающегося архива](./media/hana-installation/image17_extract_hana.PNG)

3. Когда архивы будут извлечены, скопируйте каталог, созданный при извлечении (в данном случае это 51052030). Скопируйте каталог непосредственно из HANA Large Instance/hana/shared volume в созданный каталог.

   > [!Important]
   > Не копируйте установочные пакеты в корневой или загрузочный LUN, так как пространство ограничено и предназначено для использования и другими процессами.


## <a name="install-sap-hana-on-the-hana-large-instance-unit"></a>Установка SAP HANA на единице крупного экземпляра HANA
Для установки SAP HANA необходимо выполнить вход с правами привилегированного пользователя. Только такой пользователь имеет достаточно разрешений для установки SAP HANA. Задайте разрешения для каталога, скопированного в /hana/shared.

```
chmod –R 744 <Installation bits folder>
```

Если вы хотите установить SAP HANA с помощью установки графического пользовательского интерфейса, необходимо установить пакет gtk2 на крупных экземплярах HANA. Чтобы проверить его установку, выполните следующую команду:

```
rpm –qa | grep gtk2
```

(Далее иллюстрируется установка SAP HANA с помощью графического пользовательского интерфейса.)

Перейдите в каталог установки и откройте вложенный каталог HDB_LCM_LINUX_X86_64. 

Из этого каталога запустите:

```
./hdblcmgui 
```
На этом этапе вы просматриваете череду экранов, которые предоставляют данные для установки. В этом примере мы устанавливаем сервер базы данных SAP HANA и компоненты клиента SAP HANA. Поэтому мы выбираем пункт **База данных SAP HANA**.

![Снимок экрана управления жизненным циклом SAP HANA с выбранной базой данных SAP HANA](./media/hana-installation/image18_hana_selection.PNG)

На следующем экране выберите **Установить новую систему**.

![Снимок экрана управления жизненным циклом SAP HANA с выбранной установкой новой системы](./media/hana-installation/image19_select_new.PNG)

Затем выберите один из нескольких дополнительных компонентов, которые можно установить.

![Снимок экрана управления жизненным циклом SAP HANA с перечисленными дополнительными компонентами](./media/hana-installation/image20_select_components.PNG)

Здесь мы выбрали SAP HANA Client и SAP HANA Studio. Мы также устанавливаем вертикально масштабируемый экземпляр. Затем выберите **единую систему сервера**. 

![Снимок экрана управления жизненным циклом SAP HANA с выбранной единой системой сервера](./media/hana-installation/image21_single_host.PNG)

Далее следует предоставить некоторые данные.

![Снимок экрана управления жизненным циклом SAP HANA с определяемыми полями свойств системы](./media/hana-installation/image22_provide_sid.PNG)

> [!Important]
> В качестве системного идентификатора (SID) HANA необходимо указать тот же SID, который вы предоставили корпорации Майкрософт при заказе развертывания крупных экземпляров HANA. Выбор другого SID приведет к сбою установки из-за проблемы с разрешениями доступа на разных томах.

Для пути установки используется каталог /hana/shared. На следующем шаге укажите расположение для файлов данных HANA и файлов журнала HANA.


![Снимок экрана управления жизненным циклом SAP HANA с полями данных и журнала](./media/hana-installation/image23_provide_log.PNG)

> [!Note]
> ИД безопасности, который вы указали при определении свойств системы (два экрана назад), должен соответствовать точкам подключения ИД безопасности. Если есть несоответствие, вернитесь назад и настройте ИД безопасности на значение, которое у вас есть на точках подключения.

На следующем шаге проверьте имя узла и при необходимости измените его. 

![Снимок экрана управления жизненным циклом SAP HANA с именем сервера](./media/hana-installation/image24_review_host_name.PNG)

На следующем шаге необходимо получить данные, переданные в Майкрософт при заказе развертывания крупных экземпляров HANA. 

![Снимок экрана управления жизненным циклом SAP HANA с определяемыми полями системного администратора](./media/hana-installation/image25_provide_guid.PNG)

> [!Important]
> Укажите те же **ИД пользователя системного администратора** и **ИД группы**, которые были переданы в корпорацию Майкрософт при заказе развертывания. В противном случае установка SAP HANA на единице крупного экземпляра HANA завершится сбоем.

Следующие два шага здесь не отображаются. Они позволяют предоставить пароль для системного пользователя базы данных SAP HANA и пароль для пользователя sapadm. Последний используется для агента хоста SAP, который устанавливается как часть экземпляра базы данных SAP HANA.

После определения пароля отображается экран подтверждения. Проверьте все указанные данные и продолжайте установку. Появится экран с отображением хода выполнения, как показано ниже.

![Снимок экрана управления жизненным циклом SAP HANA с установкой индикаторов хода выполнения](./media/hana-installation/image27_show_progress.PNG)

После завершения установки должен отобразиться следующий экран:

![Снимок экрана управления жизненным циклом SAP HANA с завершением определения установки](./media/hana-installation/image28_install_finished.PNG)

Теперь экземпляр SAP HANA должен быть готов к использованию. Вы сможете подключиться к нему из SAP HANA Studio. Кроме того, убедитесь, что последние обновления проверены и установлены.


## <a name="next-steps"></a>Следующие шаги

- [Высокий уровень доступности и аварийное восстановление SAP HANA в Azure (крупные экземпляры)](hana-overview-high-availability-disaster-recovery.md)

