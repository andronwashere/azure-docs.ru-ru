---
title: Руководство по резервному копированию SAP HANA на виртуальных машинах Azure | Документация Майкрософт
description: Рекомендации по использованию двух основных возможностей резервного копирования SAP HANA на виртуальных машинах Azure.
services: virtual-machines-linux
documentationcenter: ''
author: hermanndms
manager: gwallace
editor: ''
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ums.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 07/05/2018
ms.author: rclaus
ms.openlocfilehash: 91671b39e6ac33e16636cc924f5c0aa5e3fcbf3b
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/09/2019
ms.locfileid: "67709945"
---
# <a name="backup-guide-for-sap-hana-on-azure-virtual-machines"></a>Руководство по резервному копированию SAP HANA на виртуальных машинах Azure

## <a name="getting-started"></a>Приступая к работе

В этом руководстве по резервному копированию для платформы SAP HANA, запущенной на виртуальных машинах Azure, рассматриваются темы, относящиеся только к Azure. Общие сведения об элементах, связанных с резервным копированием на платформе SAP HANA, см. в _документации по SAP HANA_, ссылки на которую представлены далее в этой статье.

В этой статье рассматриваются две основные возможности резервного копирования SAP HANA на виртуальных машинах Azure:

- резервное копирование HANA в файловую систему на виртуальной машине Azure под управлением Linux (см. статью [Резервное копирование SAP HANA в Azure на уровне файлов](sap-hana-backup-file-level.md));
- резервное копирование HANA на основе моментальных снимков хранилища с помощью функции создания моментальных снимков Azure Storage Blob (вручную) или службы архивации Azure (см. статью [Резервное копирование SAP HANA на основе моментальных снимков хранилища](sap-hana-backup-storage-snapshots.md)).

SAP HANA предоставляет API-интерфейс архивации, который позволяет интегрировать средства резервного копирования сторонних поставщиков непосредственно с SAP HANA. (В этом руководстве эта возможность не рассматривается.) Сейчас этот API-интерфейс не поддерживает прямую интеграцию SAP HANA со службой архивации Azure.

SAP HANA официально поддерживается на различных типах виртуальных машин Azure, таких как Azure серии M. Для получения полного списка виртуальных машин Azure, сертифицированных для SAP HANA, ознакомьтесь со [списком сертифицированных платформ IaaS](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html#categories=Microsoft%20Azure). При добавлении новых предложений для SAP HANA в Azure мы обновим эту статью.

В Azure также доступно гибридное решение SAP HANA, где эта платформа выполняется на невиртуализированных физических серверах. Тем не менее, в этом руководстве по резервному копированию SAP HANA в Azure рассматривается чистая среда Azure, где SAP HANA выполняется на виртуальной машине Azure, а не в &quot;крупных экземплярах&quot;. Дополнительные сведения об этом решении для резервного копирования в &quot;крупных экземплярах&quot; на основе моментальных снимков хранилища см. в статье [Обзор и описание архитектуры SAP HANA в Azure (крупные экземпляры)](hana-overview-architecture.md).

Общие сведения о продуктах SAP, поддерживаемых в Azure, см. в [примечании SAP 1928533](https://launchpad.support.sap.com/#/notes/1928533).

На трех рисунках ниже представлены общие сведения о вариантах резервного копирования SAP HANA с использованием возможностей Azure, а также показаны три возможных будущих сценария резервного копирования. Эти варианты подробно описаны в связанных статьях [Резервное копирование SAP HANA в Azure на уровне файлов](sap-hana-backup-file-level.md) и [Резервное копирование SAP HANA на основе моментальных снимков хранилища](sap-hana-backup-storage-snapshots.md). Кроме того, в них представлены рекомендации по размеру и производительности резервных копий SAP HANA, размер которых может составлять несколько терабайт.

![Две возможности сохранения текущего состояния виртуальной машины](media/sap-hana-backup-guide/image001.png)

На рисунке выше показана возможность сохранения текущего состояния виртуальной машины в службе архивации Azure или вручную на основе моментального снимка дисков виртуальной машины. При таком подходе пользователи не должны управлять резервными копиями SAP HANA. При использовании сценария резервного копирования на основе моментального снимка диска клиенты сталкиваются с проблемами, связанными с согласованностью файловой системы и состоянием соответствующего приложению диска. Сведения о согласованности см. в разделе _Согласованность данных SAP HANA при создании моментальных снимков хранилища_ этой статьи. Далее в этой статье также рассматриваются возможности и ограничения службы архивации Azure в отношении резервных копий SAP HANA.

![Варианты создания резервной копии файлов SAP HANA на виртуальной машине](media/sap-hana-backup-guide/image002.png)

На рисунке выше показаны варианты создания резервной копии файлов SAP HANA на виртуальной машине, а также их сохранение в другое расположение с помощью разных средств. Для создания резервной копии HANA требуется больше времени, чем для выполнения этого процесса на основе моментальных снимков, но первый вариант более целесообразный в плане целостности и согласованности данных. Дополнительные сведения приведены далее в этой статье.

![Возможный будущий сценарий резервного копирования SAP HANA](media/sap-hana-backup-guide/image003.png)

На рисунке выше показан возможный будущий сценарий резервного копирования SAP HANA. Если SAP HANA поддерживает создание резервных копий во вторичном экземпляре репликации, это обеспечивает дополнительные возможности резервного копирования. В соответствии с записью на вики-сайте о SAP HANA сейчас эта возможность недоступна:

_&quot;Можно ли создавать резервные копии во вторичном экземпляре?_

_Нет. Сейчас резервные копии данных и журналов можно создавать только в первичном экземпляре. Если включена функция автоматического резервного копирования журналов, после перенаправления на вторичный экземпляр резервные копии журнала автоматически записываются туда&quot;_ .

## <a name="sap-resources-for-hana-backup"></a>Ресурсы SAP для резервного копирования HANA

### <a name="sap-hana-backup-documentation"></a>Документация по резервному копированию SAP HANA

- [Introduction to SAP HANA Administration](https://help.sap.com/viewer/6b94445c94ae495c83a19646e7c3fd56/2.0.00/en-US) (Общие сведения об администрировании SAP HANA)
- [Planning Your Backup and Recovery Strategy](https://help.sap.com/saphelp_hanaplatform/helpdata/en/ef/085cd5949c40b788bba8fd3c65743e/content.htm) (Планирование стратегии резервного копирования и восстановления)
- [Schedule HANA Backup using ABAP DBACOCKPIT](https://www.hanatutorials.com/p/schedule-hana-backup-using-abap.html) (Планирование резервного копирования HANA с помощью ABAP DBACOCKPIT)
- [Schedule Data Backups (SAP HANA Cockpit)](https://help.sap.com/saphelp_hanaplatform/helpdata/en/6d/385fa14ef64a6bab2c97a3d3e40292/frameset.htm) (Планирование резервного копирования данных (панель SAP HANA))
- Часто задаваемые вопросы о резервном копировании SAP HANA см. в [примечании к SAP 1642148](https://launchpad.support.sap.com/#/notes/1642148)
- Часто задаваемые вопросы о моментальных снимках базы данных и хранилища SAP HANA см. в [примечании к SAP 2039883](https://launchpad.support.sap.com/#/notes/2039883)
- Сведения о сетевых файловых системах (NFS), неподдерживаемых для резервного копирования и восстановления, см. в [примечании к SAP 1820529](https://launchpad.support.sap.com/#/notes/1820529)

### <a name="why-sap-hana-backup"></a>Преимущества резервного копирования SAP HANA

Служба хранилища Azure обеспечивает доступность и надежность по умолчанию. (Дополнительные сведения см. в статье [Введение в хранилище Microsoft Azure](../../../storage/common/storage-introduction.md).)

Минимальный размер &quot;резервной копии&quot; зависит от соглашений об уровне обслуживания для Azure, используемых для хранения файлов журналов и данных SAP HANA на виртуальных жестких дисках Azure, подключенных к виртуальной машине сервера SAP HANA. Этот подход охватывает сбои виртуальной машины, но не возможные повреждения файлов журналов и данных SAP HANA или логические ошибки, например случайное удаление данных или файлов. Резервные копии также необходимы для обеспечения соответствия требованиям или по юридическим причинам. Короче говоря, резервные копии SAP HANA нужны всегда.

### <a name="how-to-verify-correctness-of-sap-hana-backup"></a>Как проверить правильность резервной копии SAP HANA
При использовании моментальных снимков хранилища мы советуем выполнять тестовое восстановление в другой системе. Этот подход позволяет убедиться в правильности резервной копии, а также в том, что внутренние процессы резервного копирования и восстановления выполняются должным образом. Хотя в локальной среде этот процесс выполнить сложно, в облаке это можно сделать гораздо проще, временно выделив для этой цели необходимые ресурсы.

Обратите внимание, что недостаточно выполнить простое восстановление и проверку, когда платформа HANA настроена и запущена. В идеале необходимо проверить согласованность таблицы. Это позволит убедиться, что восстановленная база данных в порядке. SAP HANA предлагает несколько типов проверок согласованности (см. [примечание к SAP 1977584](https://launchpad.support.sap.com/#/notes/1977584)).

Сведения о проверке согласованности таблицы см. на веб-сайте SAP в [этой статье](https://help.sap.com/saphelp_hanaplatform/helpdata/en/25/84ec2e324d44529edc8221956359ea/content.htm#loio9357bf52c7324bee9567dca417ad9f8b).

Для стандартных резервных копий файлов тестовое восстановление выполнять не обязательно. Есть два средства SAP HANA, с помощью которых можно проверить, какую резервную копию можно использовать для восстановления: hdbbackupdiag и hdbbackupcheck. Дополнительные сведения об этих средствах см. в статье [Manually Checking Whether a Recovery is Possible](https://help.sap.com/saphelp_hanaplatform/helpdata/en/77/522ef1e3cb4d799bab33e0aeb9c93b/content.htm) (Проверка возможности восстановления вручную).

### <a name="pros-and-cons-of-hana-backup-versus-storage-snapshot"></a>Преимущества и недостатки резервной копии HANA и резервной копии на основе моментального снимка хранилища

В документации по SAP не указано, какая резервная копия более предпочтительна: резервная копия HANA или резервная копия на основе моментальных снимков хранилища. Там перечислены только преимущества и недостатки каждой из них. Таким образом каждый пользователь может выбрать предпочитаемый вариант в зависимости от ситуации и доступной технологии хранения (см. статью [Планирование стратегии резервного копирования и восстановления](https://help.sap.com/saphelp_hanaplatform/helpdata/en/ef/085cd5949c40b788bba8fd3c65743e/content.htm)).

В Azure необходимо учитывать, что функция создания моментальных снимков больших двоичных объектов Azure не гарантирует согласованность файловой системы (см. статью [Using blob snapshots with PowerShell](https://blogs.msdn.microsoft.com/cie/2016/05/17/using-blob-snapshots-with-powershell/) (Использование моментальных снимков больших двоичных объектов в PowerShell)). В разделе _Согласованность данных SAP HANA при создании моментальных снимков хранилища_ (см. ниже) рассматриваются некоторые вопросы, касающиеся этой функции.

Кроме того один должен понимать процесс выставления счетов, при работе часто с моментальными снимками BLOB-объектов, как описано в этой статье: [Основные сведения о том, как моментальные снимки увеличивают плату](/rest/api/storageservices/understanding-how-snapshots-accrue-charges)— он&#39;t очевидным, как с помощью виртуальные диски Azure.

### <a name="sap-hana-data-consistency-when-taking-storage-snapshots"></a>Согласованность данных SAP HANA при создании моментальных снимков хранилища

При создании моментальных снимков хранилища обеспечение согласованности файловой системы и приложения связанно с определенными проблемами. Самый простой способ избежать проблем — завершить работу SAP HANA или даже всей виртуальной машины. Это можно выполнить в прототипе, демонстрационной системе или даже в системе разработки. Но это не вариант для рабочей системы.

В Azure необходимо учитывать, что функция создания моментальных снимков больших двоичных объектов Azure не гарантирует согласованность файловой системы. Она хорошо работает, если задействован только один диск виртуальной машины. Но даже в таком случае необходимо проверять дополнительные компоненты. В [примечании к SAP 2039883](https://launchpad.support.sap.com/#/notes/2039883) приведены важные сведения о резервных копиях SAP HANA и моментальных снимках хранилища. Например, там указано, что при использовании файловой системы XFS перед созданием моментального снимка хранилища необходимо выполнить **xfs\_freeze**. Это позволит обеспечить согласованность данных. (Дополнительные сведения о **xfs\_freeze** см. на [странице руководства Linux по xfs\_freeze(8)](https://linux.die.net/man/8/xfs_freeze).)

Тема согласованности данных становится еще более сложной, если в отдельной файловой системе находится несколько дисков или томов. Примером может служить чередование данных с помощью mdadm или LVM. В упомянутом выше примечании к SAP указано:

_&quot;Помните, что система хранения должна гарантировать согласованность операций ввода-вывода при создании моментального снимка хранилища для каждого тома данных SAP HANA. Это значит, что создание моментальных снимков для тома данных определенной службы SAP HANA должно быть атомарной операцией&quot;_ .

Ниже приведены действия для создания согласованного моментального снимка, представляющего область данных HANA, в сценарии, когда файловая система XFS охватывает четыре виртуальных диска Azure.

- Подготовьте моментальный снимок HANA.
- Заморозьте файловую систему (например, выполните **xfs\_freeze**).
- Создайте все необходимые моментальные снимки больших двоичных объектов в Azure.
- Освободите файловую систему.
- Подтвердите моментальный снимок HANA.

Мы советуем использовать приведенный выше процесс во всех случаях вне зависимости от файловой системы. Так вы гарантируете безопасность данных. Кроме того, его можно использовать при наличии одного диска или при чередовании данных по нескольким дискам с помощью mdadm или LVM.

Очень важно подтвердить моментальный снимок HANA. В режиме подготовки моментального снимка системе SAP HANA может потребоваться дополнительное дисковое пространство, так как этот процесс сопровождается &quot;копированием при записи&quot;. Кроме того, вы также не сможете запустить новое резервное копирование до подтверждения моментального снимка SAP HANA.

Служба архивации Azure использует расширения виртуальной машины Azure, что позволяет обеспечить согласованность файловой системы. Эти расширения недоступны для автономного использования. Пользователи по-прежнему должны управлять согласованностью SAP HANA. Дополнительные сведения см. в статье [Резервное копирование SAP HANA в Azure на уровне файлов](sap-hana-backup-file-level.md).

### <a name="sap-hana-backup-scheduling-strategy"></a>Стратегия планирования резервного копирования SAP HANA

В статье SAP HANA [Planning Your Backup and Recovery Strategy](https://help.sap.com/saphelp_hanaplatform/helpdata/en/ef/085cd5949c40b788bba8fd3c65743e/content.htm) (Планирование стратегии резервного копирования и восстановления) приведен базовый план резервного копирования:

- создание моментального снимка хранилища (ежедневно);
- создание полной резервной копии данных с использованием файла или формата backint (раз в неделю);
- создание автоматических резервных копий журналов.

Моментальное снимки хранилища создавать необязательно. Вместо них можно использовать измененные резервные копии HANA, например добавочные или разностные резервные копии (см. статью об [измененных резервных копиях](https://help.sap.com/saphelp_hanaplatform/helpdata/en/c3/bb7e33bb571014a03eeabba4e37541/content.htm)).

В руководстве по администрированию HANA приведен пример списка, где предполагается, что пользователь восстанавливает SAP HANA до определенной точки во времени, используя следующую последовательность резервных копий:

1. полная резервная копия данных;
2. разностная резервная копия;
3. добавочная резервная копия 1;
4. добавочная резервная копия 2;
5. резервные копии журнала.

Рассматривая точное расписание в отношении того, когда и как часто должен выполняться определенный тип резервного копирования, мы не можем предоставить общие рекомендации. Этот процесс очень сильно зависит от клиента и от количества изменений данных в системе. Специалисты SAP советуют создавать одну полную копию HANA раз в неделю, и это можно рассматривать как общую рекомендацию.
Сведения о резервных копиях журнала см. в [документации по SAP HANA](https://help.sap.com/saphelp_hanaplatform/helpdata/en/c3/bb7e33bb571014a03eeabba4e37541/content.htm).

Кроме того, специалисты SAP советуют выполнять действия по обслуживанию каталога резервных копий, чтобы предотвратить его постоянный рост. (Дополнительные сведения см. в статье [Housekeeping for Backup Catalog and Backup Storage](https://help.sap.com/saphelp_hanaplatform/helpdata/en/ca/c903c28b0e4301b39814ef41dbf568/content.htm) (Действия по обслуживанию каталога резервных копий и хранилища службы архивации)).

### <a name="sap-hana-configuration-files"></a>Файлы конфигурации SAP HANA

Как указано в часто задаваемых вопросах в [примечании к SAP 1642148](https://launchpad.support.sap.com/#/notes/1642148), файлы конфигурации SAP HANA не являются частью стандартной резервной копии HANA. Они не важны для восстановления системы. После восстановления конфигурацию HANA можно изменить вручную. Если пользователь хочет получить ту же настраиваемую конфигурацию в процессе восстановления, файлы конфигурации HANA необходимо восстанавливать отдельно.

Если стандартные резервные копии HANA передаются в выделенную файловую систему резервного копирования HANA, пользователь может также копировать в нее файлы конфигурации, а затем скопировать все вместе в конечное хранилище, например в "холодное" хранилище BLOB-объектов.

### <a name="sap-hana-cockpit"></a>Панель SAP HANA

Панель SAP HANA позволяет выполнять мониторинг, а также управлять платформой SAP HANA через браузер. Здесь также можно обрабатывать резервные копии SAP HANA, и, следовательно, ее можно использовать в качестве альтернативы SAP HANA Studio и ABAP DBACOCKPIT. (Дополнительные сведения см. в [этой статье](https://help.sap.com/saphelp_hanaplatform/helpdata/en/73/c37822444344f3973e0e976b77958e/content.htm).)

![Экран управления базами данных на панели SAP HANA](media/sap-hana-backup-guide/image004.png)

На рисунке выше показан экран управления базами данных на панели SAP HANA и элемент резервного копирования слева. Чтобы просмотреть резервные копии, требуются определенные разрешения пользователя для учетной записи входа.

![Мониторинг текущих резервных копий на панели SAP HANA](media/sap-hana-backup-guide/image005.png)

Вы можете отслеживать состояние резервных копий на панели SAP HANA во время их создания. Как только резервное копирование будет завершено, станут доступны все сведения о резервных копиях.

![Пример использования Firefox на виртуальной машине Azure SLES 12 с GNOME Desktop](media/sap-hana-backup-guide/image006.png)

Предыдущие снимки экрана сделаны на виртуальной машине Azure под управлением Windows. В отличии от них снимок экрана выше сделан в браузере Firefox на виртуальной машине Azure SLES 12 с GNOME Desktop. На нем показаны параметры определения расписания резервного копирования на панели SAP HANA. Обратите внимание, что в качестве префикса в файлах резервных копий используются дата и время. В SAP HANA Studio при создании полной резервной копии файла по умолчанию используется префикс &quot;COMPLETE\_DATA\_BACKUP&quot;. Мы советуем использовать уникальный префикс.

### <a name="sap-hana-backup-encryption"></a>Шифрование резервной копии SAP HANA

SAP HANA обеспечивает шифрование данных и журнала. Если данные и журнал SAP HANA не зашифрованы, тогда резервные копии также не шифруются. Пользователи могут использовать для шифрования резервных копий SAP HANA сторонние решения. Дополнительные сведения о шифровании SAP HANA см. в статье [Data and Log Volume Encryption](https://help.sap.com/saphelp_hanaplatform/helpdata/en/dc/01f36fbb5710148b668201a6e95cf2/content.htm) (Шифрование журнального тома и тома данных).

В Microsoft Azure клиент может использовать для шифрования соответствующую функцию на виртуальной машине IaaS. Например, пользователи могут использовать присоединенные к виртуальной машине выделенные диски данных, на которых хранятся резервные копии SAP HANA, а затем сделать копии на эти диски.

Зашифрованные диски и виртуальные машины можно обрабатывать в службе архивации Azure (см. статью [Архивация и восстановление зашифрованных виртуальных машин с помощью службы архивации Azure](../../../backup/backup-azure-vms-encryption.md)).

Кроме того, вы можете отключить шифрование виртуальной машины SAP HANA и ее дисков, а затем сохранить файлы резервных копий SAP HANA в учетной записи хранения с поддержкой шифрования (см. статью [Шифрование службы хранилища Azure для неактивных данных (предварительная версия)](../../../storage/common/storage-service-encryption.md)).

## <a name="test-setup"></a>Тестирование настройки

### <a name="test-virtual-machine-on-azure"></a>Тестирование виртуальных машин в Azure

Для приведенных ниже тестов резервного копирования и восстановления мы использовали SAP HANA на виртуальной машине Azure типа GS5. Принципы выполнения тестов не отличаются от тестов для виртуальных машин серии M.

![Раздел "Обзор" на портале Azure для тестовой виртуальной машины HANA](media/sap-hana-backup-guide/image007.png)

На рисунке выше показан раздел "Обзор" на портале Azure для тестовой виртуальной машины HANA.

### <a name="test-backup-size"></a>Проверка размера резервной копии

![Снимок экрана консоли резервного копирования в HANA Studio с файлом резервной копии размером 229 ГБ для сервера индекса HANA](media/sap-hana-backup-guide/image008.png)

Пустая таблица заполнена данными, общий размер резервной копии которых составляет 200 ГБ. Это позволит получить точные данные о производительности. На снимке экрана выше показана консоль резервного копирования в HANA Studio с файлом резервной копии размером 229 ГБ для сервера индекса HANA. В тестах в SAP HANA Studio использовался префикс резервного копирования COMPLETE_DATA_BACKUP (по умолчанию). В реальных рабочих системах следует определить более практичный префикс. На панели SAP HANA это время и дата.

### <a name="test-tool-to-copy-files-directly-to-azure-storage"></a>Тестирование средства для копирования файлов непосредственно в службу хранилища Azure

Чтобы передать файлы резервных копий SAP HANA непосредственно в хранилище BLOB-объектов Azure или общие папки Azure, использовалось средство blobxfer. Оно поддерживает оба целевых объекта и легко интегрируется в скрипты автоматизации благодаря интерфейсу командной строки. Средство blobxfer доступно в [GitHub](https://github.com/Azure/blobxfer).

### <a name="test-backup-size-estimation"></a>Оценка размера тестовой резервной копии

Очень важно оценить размер резервной копии SAP HANA. Это позволяет улучшить производительность за счет определения максимального размера файла для нескольких файлов резервных копий в связи с параллелизмом во время копирования файлов. (Это объясняется далее в статье.) Необходимо также определить, какую резервную копию необходимо создавать: полную или измененную (добавочную или разностную).

К счастью, размер файлов резервных копий можно оценить с помощью простой инструкции SQL: **select \* from M\_BACKUP\_SIZE\_ESTIMATIONS** (см. статью [Estimate the Space Needed in the File System for a Data Backup](https://help.sap.com/saphelp_hanaplatform/helpdata/en/7d/46337b7a9c4c708d965b65bc0f343c/content.htm) (Оценка пространства, необходимого в файловой системе для резервной копии данных)).

![Выходные данные этой инструкции SQL практически соответствует реальному размеру полной резервной копии данных на диске](media/sap-hana-backup-guide/image009.png)

В тестовой системе выходные данные этой инструкции SQL практически соответствуют реальному размеру полной резервной копии данных на диске.

### <a name="test-hana-backup-file-size"></a>Проверка размера файла резервной копии HANA

![Ограничение максимального размера файлов резервных копий HANA в консоли резервного копирования HANA Studio](media/sap-hana-backup-guide/image010.png)

В консоли резервного копирования HANA Studio можно ограничить максимальный размер файлов резервных копий HANA. В примере среды эта функция позволяет разбить файл резервной копии емкостью 230 ГБ на несколько меньших файлов. Файлы меньшего размера значительно влияют на производительность (см. статью [Резервное копирование SAP HANA в Azure на уровне файлов](sap-hana-backup-file-level.md)).

## <a name="summary"></a>Сводка

В таблице ниже приведены определенные на основе результатов тестов преимущества и недостатки решений для резервного копирования базы данных SAP HANA, запущенной на виртуальных машинах Azure.

**Резервное копирование SAP HANA в файловую систему и последующее копирование файлов резервных копий в окончательное расположение**

|Решение                                           |Преимущества                                 |Недостатки                                  |
|---------------------------------------------------|-------------------------------------|--------------------------------------|
|Хранение резервных копий HANA на дисках виртуальной машины                      |Управление выполняется без дополнительных усилий     |Используется дисковое пространство локальной виртуальной машины           |
|Копирование файлов резервных копий в хранилище BLOB-объектов с помощью средства blobxfer |Параллелизм для копирования нескольких файлов, возможность использования "холодного" хранилища BLOB-объектов | Настройка дополнительного средства и написание пользовательских скриптов | 
|Копирование больших двоичных объектов через PowerShell или интерфейс командной строки                    |Не нужно использовать дополнительные средства. Процесс выполняется через PowerShell или интерфейс командной строки |Процесс выполняется вручную. Пользователь должен писать скрипты и управлять скопированными большими двоичными объектами для восстановления|
|Копирование в общую папку NFS                                  |Постобработка файлов резервных копий на другой виртуальной машине, не влияя на сервер HANA|Копирование выполняется медленно|
|Копирование в службу файлов Azure с помощью blobxfer                |Пространство на дисках локальной виртуальной машины не используется|Резервная копия HANA не поддерживает непосредственную запись. Сейчас ограничение размера файлового ресурса составляет 5 ТБ|
|Агент службы архивации Azure                                 | Используется как предпочтительное решение         | Сейчас недоступен в Linux    |



**Резервное копирование SAP HANA на основе моментальных снимков хранилища**

|Решение                                           |Преимущества                                 |Недостатки                                  |
|---------------------------------------------------|-------------------------------------|--------------------------------------|
|Служба архивации Azure                               | Поддерживает резервное копирование виртуальной машины на основе моментальных снимков хранилища | Если восстановление выполняется не на уровне файлов, необходимо создать виртуальную машину, что подразумевает получение нового лицензионного ключа SAP HANA|
|Создание моментальных снимков больших двоичных объектов вручную                              | Гибкость создания и восстановления конкретных дисков, не изменяя уникальный идентификатор виртуальной машины|Все процессы выполняются клиентами вручную|

## <a name="next-steps"></a>Следующие шаги
* В статье [Резервное копирование SAP HANA в Azure на уровне файлов](sap-hana-backup-file-level.md) описан вариант резервного копирования на основе файлов,
* а в статье [Резервное копирование SAP HANA на основе моментальных снимков хранилища](sap-hana-backup-storage-snapshots.md) — на основе моментальных снимков хранилища.
* Дополнительные сведения об обеспечении высокого уровня доступности и планировании аварийного восстановления SAP HANA в Azure (крупные экземпляры) см. в [этой статье](hana-overview-high-availability-disaster-recovery.md).
