---
title: Экземпляр отказоустойчивого кластера SQL Server на виртуальных машинах Azure | Документация Майкрософт
description: В этой статье объясняется, как создать экземпляр отказоустойчивого кластера SQL Server на виртуальных машинах Azure.
services: virtual-machines
documentationCenter: na
author: MikeRayMSFT
manager: craigg
editor: monicar
tags: azure-service-management
ms.assetid: 9fc761b1-21ad-4d79-bebc-a2f094ec214d
ms.service: virtual-machines-sql
ms.devlang: na
ms.custom: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 06/11/2018
ms.author: mikeray
ms.openlocfilehash: 39f38af601888f847cd1a82da9e2e03e6893c28e
ms.sourcegitcommit: f10ae7078e477531af5b61a7fe64ab0e389830e8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/05/2019
ms.locfileid: "67607281"
---
# <a name="configure-sql-server-failover-cluster-instance-on-azure-virtual-machines"></a>Настройка экземпляра отказоустойчивого кластера SQL Server на виртуальных машинах Azure

В этой статье объясняется, как создать экземпляр отказоустойчивого кластера SQL Server на виртуальных машинах Azure в модели Resource Manager. В этом решении [локальные дисковые пространства Windows Server 2016 Datacenter Edition \(S2D\) ](https://technet.microsoft.com/windows-server-docs/storage/storage-spaces/storage-spaces-direct-overview) используются в качестве программной виртуальной сети SAN, синхронизирующей хранилище (диски с данными) между узлами (виртуальные машины Azure) в кластере Windows. S2D — это новшество в Windows Server 2016.

На следующей схеме показано полное решение на виртуальных машинах Azure.

![Группа доступности](./media/virtual-machines-windows-portal-sql-create-failover-cluster/00-sql-fci-s2d-complete-solution.png)

На предыдущей схеме показано следующее:

- Две виртуальные машины Azure в отказоустойчивом кластере Windows. Когда виртуальная машина находится в отказоустойчивом кластере, она также называется *узлом кластера* или просто *узлом*.
- В каждой виртуальной машине есть два или больше дисков данных.
- S2D синхронизирует данные на диске данных и представляет синхронизированное хранилище в качестве пула носителей.
- Пул носителей представляет общий том кластера (CSV) для отказоустойчивого кластера.
- Роль экземпляра кластера SQL Server FCI использует CSV для дисков данных.
- Azure Load Balancer для хранения IP-адреса для экземпляра отказоустойчивого кластера SQL Server.
- В группе доступности Azure хранятся все ресурсы.

   >[!NOTE]
   >Все ресурсы Azure, представленные на схеме, находятся в одной и той же группе ресурсов.

[Дополнительные сведения о \(S2D\)](https://technet.microsoft.com/windows-server-docs/storage/storage-spaces/storage-spaces-direct-overview) см. в статье Локальные дисковые пространства в Windows Server 2016.

S2D поддерживает два типа архитектуры — конвергентную и гиперконвергентную. В этом документе используется гиперконвергентная архитектура. Гиперконвергентная инфраструктура размещает хранилище на тех же серверах, на которых размещено кластеризованное приложение. В этой архитектуре хранилище находится на каждом узле экземпляра отказоустойчивого кластера SQL Server.

## <a name="licensing-and-pricing"></a>Лицензирование и цены

На виртуальных машинах Azure можно лицензировать SQL Server, используя образы виртуальных машин с оплатой по мере использования (PAYG) или с использованием собственной лицензии (BYOL). Тип выбранного образа влияет на процесс выставления счетов.

При лицензировании PAYG для экземпляра отказоустойчивого кластера (FCI) SQL Server на виртуальных машинах Azure оплачиваются все узлы, в том числе пассивные. Дополнительные сведения см. на странице [Цены на виртуальные машины SQL Server Enterprise](https://azure.microsoft.com/pricing/details/virtual-machines/sql-server-enterprise/). 

Клиенты, заключившие Соглашение Enterprise с применением программы Software Assurance, имеют право использовать по одному бесплатному пассивному узлу отказоустойчивого кластера на каждый активный узел. Чтобы получить такую возможность в Azure, используйте образы виртуальных машин BYOL, а затем примените одну и ту же лицензию как к активным, так и к пассивным узлам FCI. Дополнительные сведения см. в статье о [Соглашении Enterprise](https://www.microsoft.com/Licensing/licensing-programs/enterprise.aspx).

Чтобы сравнить возможности лицензирования PAYG и BYOL для SQL Server на виртуальных машинах Azure, см. статью о [начале работы с виртуальными машинами SQL](virtual-machines-windows-sql-server-iaas-overview.md#get-started-with-sql-vms).

Полные сведения о лицензировании SQL Server см. [на странице с ценами](https://www.microsoft.com/sql-server/sql-server-2017-pricing).

### <a name="example-azure-template"></a>Пример шаблона Azure

На основе шаблона в Azure можно создать полное решение. Пример шаблона доступен в [шаблонах быстрого запуска Azure](https://github.com/MSBrett/azure-quickstart-templates/tree/master/sql-server-2016-fci-existing-vnet-and-ad) на GitHub. Этот пример не предназначен или протестирован для какой-либо конкретной рабочей нагрузки. Вы можете запустить шаблон, чтобы создать экземпляр отказоустойчивого кластера SQL Server с хранилищем S2D, подключенным к вашему домену. Шаблон можно оценить и изменить в соответствии со своими потребностями.

## <a name="before-you-begin"></a>Перед началом работы

Прежде чем продолжить, необходимо разобраться с некоторыми моментами и настроить несколько компонентов.

### <a name="what-to-know"></a>Необходимые знания
Вам необходимо иметь представление о следующих технологиях:

- [технологии кластера под управлением Windows](https://docs.microsoft.com/windows-server/failover-clustering/failover-clustering-overview);
- [экземпляры отказоустойчивого кластера SQL Server](https://docs.microsoft.com/sql/sql-server/failover-clusters/windows/always-on-failover-cluster-instances-sql-server).

Одно важное отличие рекомендуется на отказоустойчивый кластер гостевых виртуальных Машин Azure IaaS одним сетевым Адаптером на сервере (узел кластера) и одной подсети. Сеть Azure обладает физической избыточностью, что делает ненужными дополнительные сетевые адаптеры и подсети в кластере гостевых виртуальных машин IaaS Azure. Несмотря на то, что отчет о проверке кластера выдаст предупреждение о том, что узлы доступны только в одной сети, это предупреждение можно спокойно проигнорировать в отказоустойчивых кластерах гостевых виртуальных машин IaaS Azure. 

Дополнительно необходимо иметь общее представление о следующих технологиях:

- [гиперконвергентное решение, использующее локальные дисковые пространства в Windows Server 2016](https://technet.microsoft.com/windows-server-docs/storage/storage-spaces/hyper-converged-solution-using-storage-spaces-direct);
- [группы ресурсов Azure](../../../azure-resource-manager/manage-resource-groups-portal.md).

> [!IMPORTANT]
> В настоящее время [расширение агента IaaS SQL Server](virtual-machines-windows-sql-server-agent-extension.md) не поддерживается для экземпляра отказоустойчивого кластера SQL Server в Azure. Мы рекомендуем удалить расширение с виртуальных машин, которые участвуют в экземпляре отказоустойчивого кластера. Это расширение поддерживает такие компоненты, как автоматическое резервное копирование и установка исправлений, а также некоторые возможности портала для SQL. Эти компоненты не будут работать для виртуальных машин SQL после удаления агента.

### <a name="what-to-have"></a>Необходимые компоненты

Перед выполнением инструкций, приведенных в данной статье, вам необходимо иметь следующее:

- подписка Microsoft Azure;
- домен Windows на виртуальных машинах Azure;
- учетная запись с разрешением на создание объектов в виртуальной машине Azure;
- виртуальная сеть и подсеть Azure с достаточным пространством IP-адресов для следующих компонентов:
   - обе виртуальные машины;
   - IP-адрес отказоустойчивого кластера;
   - IP-адрес для каждого экземпляра отказоустойчивого кластера;
- служба DNS, настроенная в сети Azure, указывающей на контроллеры домена.

После выполнения этих предварительных требований можно продолжить создание отказоустойчивого кластера. Сначала нужно создать виртуальные машины.

## <a name="step-1-create-virtual-machines"></a>Шаг 1. Создание виртуальных машин

1. Войдите на [портал Azure](https://portal.azure.com) с использованием своей подписки.

1. [Создайте группу доступности Azure](../tutorial-availability-sets.md).

   Группа доступности группирует виртуальные машины в доменах сбоя и доменах обновления. Она позволяет гарантировать, что на ваше приложение не повлияют отдельные неисправные точки, такие как сетевой коммутатор или блок питания стойки серверов.

   Если вы не создали группу ресурсов для виртуальных машин, сделайте это во время создания группы доступности Azure. Если вы используете портал Azure для создания группы доступности, сделайте следующее:

   - На портале Azure щелкните **+** , чтобы открыть Azure Marketplace. Найдите **Группа доступности**.
   - Выберите **Группа доступности**.
   - Нажмите кнопку **Создать**.
   - В колонке **Создание группы доступности** установите следующие значения:
      - **Имя.** Имя группы доступности.
      - **Подписка**: Вашу подписку Azure.
      - **Группа ресурсов.** Если вы хотите использовать имеющуюся группу, выберите **Использовать существующий** и выберите группу из раскрывающегося списка. В противном случае выберите **Создать** и введите имя для группы.
      - **Расположение.** Задайте расположение, в котором вы планируете создать виртуальные машины.
      - **Домены сбоя**. Используйте значение по умолчанию (3).
      - **Домены обновления**. Используйте значение по умолчанию (5).
   - Щелкните **Создать**, чтобы создать группу доступности.

1. Создайте виртуальные машины в группе доступности.

   Подготовьте две виртуальные машины SQL Server в группе доступности Azure. Инструкции см. в статье [Подготовка виртуальной машины SQL Server на портале Azure](virtual-machines-windows-portal-sql-server-provision.md).

   Разместите обе виртуальные машины:

   - в той же группе ресурсов Azure, что и группа доступности;
   - в той же сети, что и контроллер домена;
   - в подсети с достаточным пространством IP-адресов для обеих виртуальных машин и всех экземпляров отказоустойчивого кластера, которые со временем могут использоваться в этом кластере;
   - в группе доступности Azure.   

      >[!IMPORTANT]
      >После создания виртуальной машины установить или изменить группу доступности невозможно.

   Выберите образ из Azure Marketplace. Вы можете использовать образ Marketplace с образом, который включает Windows Server и SQL Server или просто Windows Server. Дополнительные сведения см. в статье [Приступая к работе с SQL Server в виртуальных машинах Azure](virtual-machines-windows-sql-server-iaas-overview.md).

   Официальные образы SQL Server в коллекции Azure содержат установленный экземпляр SQL Server, а также программное обеспечение для установки SQL Server и необходимый ключ.

   Выберите необходимый образ в соответствии с желаемым способом оплаты за лицензию SQL Server:

   - **Лицензирование с оплатой по мере использования**. Посекундная оплата этих образов доступна в таких лицензиях SQL Server:
      - **SQL Server 2016 Enterprise на базе Windows Server Datacenter 2016**;
      - **SQL Server 2016 Standard на базе Windows Server Datacenter 2016**;
      - **SQL Server 2016 Developer на базе Windows Server Datacenter 2016**.

   - **BYOL (с использованием собственной лицензии)** :

      - **{BYOL} SQL Server 2016 Enterprise на базе Windows Server Datacenter 2016**;
      - **{BYOL} SQL Server 2016 Standard на базе Windows Server Datacenter 2016**.

   >[!IMPORTANT]
   >После создания виртуальной машины удалите предварительно установленный автономный экземпляр SQL Server. Вы будете использовать предварительно установленный носитель SQL Server для создания экземпляра отказоустойчивого кластера SQL Server после настройки отказоустойчивого кластера и S2D.

   Кроме того, можно использовать образы Azure Marketplace только с операционной системой. Выберите образ **Windows Server 2016 Datacenter** и установите экземпляр отказоустойчивого кластера SQL Server после настройки отказоустойчивого кластера и S2D. Этот образ не содержит установочный носитель SQL Server. Поместите установочный носитель в расположении, где будет выполняться установка SQL Server для каждого сервера.

1. После создания виртуальных машин в Azure подключитесь к каждой виртуальной машине с помощью протокола удаленного рабочего стола.

   При первом подключении к виртуальной машине с помощью протокола удаленного рабочего стола компьютер запрашивает, должен ли этот компьютер быть доступным в сети. Щелкните **Да**.

1. Если вы используете один из образов виртуальных машин на основе SQL Server, удалите экземпляр SQL Server.

   - В разделе **Программы и компоненты** правой кнопкой мыши щелкните **Microsoft SQL Server 2016 (64-разрядная версия)** и выберите **Удалить/Изменить**.
   - Нажмите кнопку **Удалить**.
   - Выберите экземпляр по умолчанию.
   - Удалите все компоненты в разделе **Службы ядра СУБД**. Не удаляйте компоненты в разделе **Общие компоненты**. См. следующий рисунок.

      ![Удаление компонентов](./media/virtual-machines-windows-portal-sql-create-failover-cluster/03-remove-features.png)

   - Нажмите кнопку **Далее**, а затем — кнопку **Удалить**.

1. <a name="ports"></a>Откройте порты брандмауэра.

   На каждой виртуальной машине откройте следующие порты в брандмауэре Windows.

   | Цель | TCP-порт | Примечания
   | ------ | ------ | ------
   | SQL Server | 1433 | Обычный порт для экземпляров SQL Server по умолчанию. Если используется образ из коллекции, этот порт будет автоматически открыт.
   | Проверка работоспособности | 59999 | Любой открытый TCP-порт. Позже настройте [проверку работоспособности](#probe) балансировщика нагрузки и кластер для использования этого порта.  

1. Добавьте хранилище в виртуальную машину. Дополнительные сведения см. в статье [Хранилище класса "Премиум": высокопроизводительная служба хранилища для рабочих нагрузок виртуальных машин Azure](../disks-types.md).

   Для обеих виртуальных машин необходимо по крайней мере два диска данных.

   Присоедините диски в формате RAW — диски, не отформатированные в NTFS.
      >[!NOTE]
      >Если присоединить диски, отформатированные в NTFS, S2D можно включить только без проверки допустимости диска.  

   Подключите как минимум два SSD (цен. категория "Премиум") к каждой виртуальной машине. Мы рекомендуем по крайней мере диски P30 (1 ТБ).

   Для кэширования узла задайте значение **Только для чтения**.

   Емкость хранилища, используемого в рабочих средах, зависит от рабочей нагрузки. Значения, описанные в этой статье, предназначены для демонстрации и тестирования.

1. [Добавьте виртуальные машины в имеющийся домен](virtual-machines-windows-portal-sql-availability-group-prereq.md#joinDomain).

После создания и настройки виртуальных машин можно настроить отказоустойчивый кластер.

## <a name="step-2-configure-the-windows-failover-cluster-with-s2d"></a>Шаг 2. Настройка отказоустойчивого кластера Windows с S2D

Следующим шагом является настройка отказоустойчивого кластера с S2D. На этом шаге вам предстоит выполнить дополнительные действия:

1. Добавление компонента отказоустойчивой кластеризации Windows.
1. Проверка кластера
1. Создание отказоустойчивого кластера.
1. Создание облака-свидетеля.
1. Добавление хранилища.

### <a name="add-windows-failover-clustering-feature"></a>Добавление компонента отказоустойчивой кластеризации Windows

1. Чтобы начать, подключитесь к первой виртуальной машине с помощью протокола удаленного рабочего стола, используя учетную запись домена, которая является участником локальной группы администраторов, а также имеет разрешения на создание объектов в Active Directory. Используйте эту учетную запись для остальной части конфигурации.

1. [Добавьте компонент отказоустойчивой кластеризации для каждой виртуальной машины](virtual-machines-windows-portal-sql-availability-group-prereq.md#add-failover-clustering-features-to-both-sql-server-vms).

   Чтобы установить компонент отказоустойчивой кластеризации из пользовательского интерфейса, сделайте следующее на обеих виртуальных машинах.
   - В **диспетчере серверов** выберите **Управление**, а затем — **Добавить роли и компоненты**.
   - В **мастере добавления ролей и компонентов** нажимайте кнопку **Далее**, пока не откроется раздел **Выбор компонентов**.
   - В разделе **Выбор компонентов** выберите **Отказоустойчивая кластеризация**. Включите все необходимые компоненты и средства управления. Щелкните **Добавить компоненты**.
   - Чтобы установить компоненты, нажмите кнопку **Далее**, а затем — **Готово**.

   Чтобы установить компонент отказоустойчивой кластеризации с помощью PowerShell, запустите следующий скрипт из сеанса PowerShell администратора на одной из виртуальных машин.

   ```powershell
   $nodes = ("<node1>","<node2>")
   Invoke-Command  $nodes {Install-WindowsFeature Failover-Clustering -IncludeAllSubFeature -IncludeManagementTools}
   ```

Для справки в следующих шагах используются инструкции, приведенные на шаге 3 в статье [Гиперконвергентное решение с использованием локальных дисковых пространств в Windows Server 2016](https://technet.microsoft.com/windows-server-docs/storage/storage-spaces/hyper-converged-solution-using-storage-spaces-direct#step-3-configure-storage-spaces-direct).

### <a name="validate-the-cluster"></a>Проверка кластера

В этом руководстве используются инструкции, приведенные в разделе о [проверке кластера](https://technet.microsoft.com/windows-server-docs/storage/storage-spaces/hyper-converged-solution-using-storage-spaces-direct#step-31-run-cluster-validation).

Проверьте кластер в пользовательском интерфейсе или с помощью PowerShell.

Чтобы проверить работу кластера с помощью пользовательского интерфейса, сделайте следующее на одной из виртуальных машин.

1. В **диспетчере серверов** выберите **Сервис**, а затем — **Диспетчер отказоустойчивости кластеров**.
1. В **диспетчере отказоустойчивости кластеров** выберите **Действие**, а затем щелкните **Проверить конфигурацию...** .
1. Нажмите кнопку **Далее**.
1. На вкладке **Выбор серверов или кластера** введите имена обеих виртуальных машин.
1. На вкладке **Параметры тестирования** выберите **Выполнять только выбранные тесты**. Нажмите кнопку **Далее**.
1. На вкладке **Выбор тестов** выберите все тесты, кроме **Хранилище**. См. следующий рисунок.

   ![Проверка тестов](./media/virtual-machines-windows-portal-sql-create-failover-cluster/10-validate-cluster-test.png)

1. Нажмите кнопку **Далее**.
1. На вкладке **Подтверждение** нажмите кнопку **Далее**.

**Мастер проверки конфигурации** выполняет проверочные тесты.

Чтобы проверить кластер с помощью PowerShell, запустите следующий скрипт из сеанса PowerShell администратора на одной из виртуальных машин.

   ```powershell
   Test-Cluster –Node ("<node1>","<node2>") –Include "Storage Spaces Direct", "Inventory", "Network", "System Configuration"
   ```

После проверки кластера создайте отказоустойчивый кластер.

### <a name="create-the-failover-cluster"></a>Создание отказоустойчивого кластера.

В этом руководстве используются инструкции, приведенные в разделе [Создание отказоустойчивого кластера](https://technet.microsoft.com/windows-server-docs/storage/storage-spaces/hyper-converged-solution-using-storage-spaces-direct#step-32-create-a-cluster).

Для создания отказоустойчивого кластера требуется следующее.
- Имена виртуальных машин, которые становятся узлами кластера.
- Имя отказоустойчивого кластера.
- IP-адрес отказоустойчивого кластера. Вы можете использовать IP-адрес, который не используется в той же виртуальной сети и подсети Azure, где расположены узлы кластера.

Приведенная ниже команда PowerShell создает отказоустойчивый кластер. Обновите скрипт, введя имена узлов (имена виртуальных машин) и доступный IP-адрес из виртуальной сети Azure:

```powershell
New-Cluster -Name <FailoverCluster-Name> -Node ("<node1>","<node2>") –StaticAddress <n.n.n.n> -NoStorage
```   

### <a name="create-a-cloud-witness"></a>Создание облака-свидетеля

Облако-свидетель является новым типом свидетеля кворума кластера, хранящегося в Azure Storage Blob. Это устраняет необходимость в отдельной виртуальной машине, используемой для размещения файлового ресурса-свидетеля.

1. [Создайте облако-свидетель для отказоустойчивого кластера](https://technet.microsoft.com/windows-server-docs/failover-clustering/deploy-cloud-witness).

1. Создайте контейнер больших двоичных объектов.

1. Сохраните ключи доступа и URL-адрес контейнера.

1. Настройте свидетель кворума отказоустойчивого кластера. См. раздел о [настройке свидетеля кворума в пользовательском интерфейсе](https://technet.microsoft.com/windows-server-docs/failover-clustering/deploy-cloud-witness#to-configure-cloud-witness-as-a-quorum-witness).

### <a name="add-storage"></a>Добавление хранилища

Диски для S2D должны быть пустыми и не содержать разделов или других данных. Чтобы очистить диски, выполните [действия, описанные в этом руководстве](https://technet.microsoft.com/windows-server-docs/storage/storage-spaces/hyper-converged-solution-using-storage-spaces-direct#step-34-clean-disks).

1. [Включите локальные дисковые пространства \(S2D\)](https://technet.microsoft.com/windows-server-docs/storage/storage-spaces/hyper-converged-solution-using-storage-spaces-direct#step-35-enable-storage-spaces-direct).

   Следующий скрипт PowerShell включает локальные дисковые пространства.  

   ```powershell
   Enable-ClusterS2D
   ```

   В **диспетчере отказоустойчивости кластеров** теперь отображается пул носителей.

1. [Создайте том](https://technet.microsoft.com/windows-server-docs/storage/storage-spaces/hyper-converged-solution-using-storage-spaces-direct#step-36-create-volumes).

   Одна из функций S2D — автоматическое создание пула носителей при включении. Теперь можно создать том. Командлет PowerShell `New-Volume` автоматизирует процесс создания тома, включая форматирование, добавление в кластер и создание общего тома кластера (CSV). В следующем примере создается CSV емкостью 800 ГБ.

   ```powershell
   New-Volume -StoragePoolFriendlyName S2D* -FriendlyName VDisk01 -FileSystem CSVFS_REFS -Size 800GB
   ```   

   После выполнения этой команды том емкостью 800 ГБ подключается как ресурс кластера. Том находится в папке `C:\ClusterStorage\Volume1\`.

   На следующем снимке экрана показан общий том кластера с S2D.

   ![ClusterSharedVolume](./media/virtual-machines-windows-portal-sql-create-failover-cluster/15-cluster-shared-volume.png)

## <a name="step-3-test-failover-cluster-failover"></a>Шаг 3. Тестирование отработки отказа отказоустойчивого кластера

В диспетчере отказоустойчивости кластеров убедитесь, что можете переместить ресурс хранилища в другой узел кластера. Если вы можете подключиться к отказоустойчивому кластеру с помощью **диспетчера отказоустойчивости кластеров** и перемещать хранилище с одного узла на другой, можно приступать к настройке экземпляра отказоустойчивого кластера.

## <a name="step-4-create-sql-server-fci"></a>Шаг 4. Создание экземпляра отказоустойчивого кластера SQL Server

После настройки отказоустойчивого кластера и всех компонентов кластера, включая хранилище, можно создать экземпляр отказоустойчивого кластера SQL Server.

1. Подключитесь к первой виртуальной машине Azure с помощью RDP.

1. В **диспетчере отказоустойчивости кластеров** убедитесь, что все основные ресурсы кластера находятся на первой виртуальной машине. При необходимости переместите все ресурсы на эту виртуальную машину.

1. Найдите установочный носитель. Если на виртуальной машине используется один из образов Azure Marketplace, носитель находится в папке `C:\SQLServer_<version number>_Full`. Щелкните **Настройка**.

1. В диалоговом окне **Центр установки SQL Server** выберите **Установка**.

1. Щелкните **Новая установка отказоустойчивого кластера SQL Server**. Следуйте указаниям мастера, чтобы установить экземпляр отказоустойчивого кластера SQL Server.

   Каталоги данных экземпляра отказоустойчивого кластера должны находиться в кластеризованном хранилище. В случае с S2D это не общий диск, а точка подключения к тому на каждом сервере. S2D синхронизирует том между обоими узлами. Том предоставляется в кластере в качестве общего тома кластера. Используйте точки подключения CSV для каталогов данных.

   ![DataDirectories](./media/virtual-machines-windows-portal-sql-create-failover-cluster/20-data-dicrectories.png)

1. После завершения работы мастера программа установки установит экземпляр отказоустойчивого кластера SQL Server на первом узле.

1. После успешной установки экземпляра отказоустойчивого кластера на первом узле подключитесь ко второму узлу с помощью протокола удаленного рабочего стола.

1. Откройте диалоговое окно **Центр установки SQL Server**. Щелкните **Установка**.

1. Выберите **Добавление узла в отказоустойчивый кластер SQL Server**. Следуйте указаниям мастера, чтобы установить сервер SQL и добавить его в экземпляр отказоустойчивого кластера.

   >[!NOTE]
   >При использовании образа коллекции Azure Marketplace с SQL Server средства SQL Server включаются в образ. Если вы не использовали этот образ, установите средства SQL Server отдельно. См. сведения в статье [Скачивание SQL Server Management Studio (SSMS)](https://msdn.microsoft.com/library/mt238290.aspx).

## <a name="step-5-create-azure-load-balancer"></a>Шаг 5. Создание Azure Load Balancer

На виртуальных машинах Azure кластеры используют балансировщик нагрузки для хранения IP-адреса, который должен находиться на одном узле кластера в определенный момент времени. В этом решении балансировщик нагрузки используется для хранения IP-адреса для экземпляра отказоустойчивого кластера SQL Server.

[Создайте и настройте балансировщик нагрузки Azure](virtual-machines-windows-portal-sql-availability-group-tutorial.md#configure-internal-load-balancer).

### <a name="create-the-load-balancer-in-the-azure-portal"></a>Создание балансировщика нагрузки на портале Azure

Создание балансировщика нагрузки

1. На портале Azure перейдите в группу ресурсов с виртуальными машинами.

1. Щелкните **+ Добавить**. В Marketplace найдите **Балансировщик нагрузки**. Щелкните **Балансировщик нагрузки**.

1. Нажмите кнопку **Создать**.

1. Настройте для балансировщика нагрузки следующие параметры.

   - **Имя.** Имя, определяющее подсистему балансировки нагрузки.
   - **Тип:** Подсистема балансировки нагрузки может быть открытой или закрытой. Доступ к закрытому балансировщику нагрузки может осуществляться в пределах одной и той же виртуальной сети. Большинство приложений Azure могут использовать закрытый балансировщик нагрузки. Если приложению требуется доступ к SQL Server непосредственно через Интернет, используйте открытый балансировщик нагрузки.
   - **Виртуальная сеть**. Виртуальная сеть, в которой находятся виртуальные машины.
   - **Подсеть.** Подсеть, в которой находятся виртуальные машины.
   - **Частный IP-адрес**. IP-адрес, назначенный сетевому ресурсу кластера SQL Server FCI.
   - **Подписка**. Вашу подписку Azure.
   - **Группа ресурсов**. Используйте ту же группу ресурсов, которая используется для виртуальных машин.
   - **Расположение.** Используйте то же расположение Azure, что используется для виртуальных машин.
   См. следующий рисунок.

   ![CreateLoadBalancer](./media/virtual-machines-windows-portal-sql-create-failover-cluster/30-load-balancer-create.png)

### <a name="configure-the-load-balancer-backend-pool"></a>Настройка серверного пула балансировщика нагрузки

1. Вернитесь в группу ресурсов Azure с виртуальными машинами и найдите новый балансировщик нагрузки. Может потребоваться обновить представление в группе ресурсов. Щелкните балансировщик нагрузки.

1. Выберите **Серверные пулы** и нажмите кнопку **+ Добавить**, чтобы добавить серверный пул.

1. Свяжите серверный пул с группой доступности, в которую входят виртуальные машины.

1. В разделе **Целевые IP-конфигурации сети** установите флажок **Виртуальная машина** и выберите виртуальные машины, которые будут узлами кластера. Не забудьте включить все виртуальные машины, на которых будут размещаться FCI. 

1. Нажмите кнопку **ОК**, чтобы создать внутренний пул.

### <a name="configure-a-load-balancer-health-probe"></a>Настройка пробы работоспособности балансировщика нагрузки

1. В колонке балансировщика нагрузки щелкните **Health probes** (Пробы работоспособности).

1. Щелкните **+ Добавить**.

1. В колонке **Add health probe** (Добавить проверку работоспособности) <a name="probe"></a>задайте параметры проверки работоспособности:

   - **Имя.** Имя для проверки работоспособности.
   - **Протокол**: Протокол TCP.
   - **Порт**: Порт, который вы создали в брандмауэре для проверки работоспособности в [этот шаг](#ports). В этой статье, в примере используется TCP-порт `59999`.
   - **Интервал.** 5 секунд.
   - **Пороговое значение сбоя**. 2 последовательных сбоя.

1. Нажмите кнопку ОК.

### <a name="set-load-balancing-rules"></a>Задание правил балансировки нагрузки

1. В колонке балансировщика нагрузки щелкните **Правила балансировки нагрузки**.

1. Щелкните **+ Добавить**.

1. Настройте следующие параметры балансировки нагрузки.

   - **Имя.** Имя для правил балансировки нагрузки.
   - **Интерфейсный IP-адрес**. Используйте IP-адрес для сетевого ресурса кластера SQL Server FCI.
   - **Порт**: TCP-порт экземпляра отказоустойчивого кластера SQL Server. Порт экземпляра по умолчанию — 1433.
   - **Серверный порт**. Для этого значения используется тот же порт, что и для значения **Порт** при включении параметра **Плавающий IP-адрес (прямой ответ от сервера)** .
   - **Серверный пул**. Используйте имя серверного пула, настроенного ранее.
   - **Зонд работоспособности**. Используйте проверку работоспособности, настроенную ранее.
   - **Сохранение сеанса**. Отсутствует.
   - **Время ожидания простоя (в минутах)** . 4.
   - **Плавающий IP-адрес (прямой ответ от сервера)** . Enabled

1. Нажмите кнопку **ОК**.

## <a name="step-6-configure-cluster-for-probe"></a>Шаг 6. Настройка кластера для проверки

Задайте параметр порта проверки кластера в PowerShell.

Чтобы задать параметр порта пробы кластера, измените переменные в следующем сценарии, указав значения для своей среды. Удалите угловые скобки `<>` из сценария. 

   ```powershell
   $ClusterNetworkName = "<Cluster Network Name>"
   $IPResourceName = "<SQL Server FCI IP Address Resource Name>" 
   $ILBIP = "<n.n.n.n>" 
   [int]$ProbePort = <nnnnn>

   Import-Module FailoverClusters

   Get-ClusterResource $IPResourceName | Set-ClusterParameter -Multiple @{"Address"="$ILBIP";"ProbePort"=$ProbePort;"SubnetMask"="255.255.255.255";"Network"="$ClusterNetworkName";"EnableDhcp"=0}
   ```

В предыдущем сценарии задайте значения для своей среды. Эти значения описаны в списке ниже.

   - `<Cluster Network Name>`. Имя отказоустойчивого кластера Windows Server для сети. Выберите **Диспетчер отказоустойчивости кластеров** > **Сети**, щелкните правой кнопкой мыши сеть и щелкните **Свойства**. Правильное значение указано в поле **Имя** на вкладке **Общие**. 

   - `<SQL Server FCI IP Address Resource Name>`. Имя ресурса IP-адреса экземпляра отказоустойчивого кластера SQL Server. Выберите **Диспетчер отказоустойчивости кластеров** > **Роли**. Для роли экземпляра отказоустойчивого кластера SQL Server в разделе **Имя сервера** щелкните правой кнопкой мыши ресурс IP-адреса и выберите **Свойства**. Правильное значение указано в поле **Имя** на вкладке **Общие**. 

   - `<ILBIP>`. IP-адрес внутренней подсистемы балансировки нагрузки. Этот адрес настраивается на портале Azure в качестве интерфейсного адреса внутренней подсистемы балансировки нагрузки. Это также IP-адрес экземпляра отказоустойчивого кластера SQL Server. Его можно найти в окне **Диспетчер отказоустойчивости кластеров** на той же странице свойств, где указано значение `<SQL Server FCI IP Address Resource Name>`.  

   - `<nnnnn>`. Порт пробы, настроенный в пробе работоспособности подсистемы балансировки нагрузки. Допускается любой неиспользуемый TCP-порт. 

>[!IMPORTANT]
>Маска подсети для параметра кластера должна быть широковещательным адресом TCP IP: `255.255.255.255`.

После настройки пробы кластера все параметры кластера можно просмотреть в PowerShell. Выполните следующий скрипт:

   ```powershell
   Get-ClusterResource $IPResourceName | Get-ClusterParameter 
  ```

## <a name="step-7-test-fci-failover"></a>Шаг 7. Проверка отработки отказа экземпляра отказоустойчивого кластера

Проверьте отработку отказа экземпляра отказоустойчивого кластера, чтобы проверить функциональные возможности кластера. Сделайте следующее:

1. Подключитесь к одному из узлов кластера SQL Server FCI с помощью протокола удаленного рабочего стола.

1. Откройте **диспетчер отказоустойчивости кластеров**. Щелкните **Роли**. Обратите внимание, какой узел является владельцем роли SQL Server FCI.

1. Щелкните правой кнопкой мыши роль SQL Server FCI.

1. Щелкните **Переместить** и выберите **Лучший из возможных узлов**.

В **диспетчере отказоустойчивости кластеров** отобразится роль, и ее ресурсы перейдут в автономный режим. Затем ресурсы переместятся и станут доступными на другом узле.

### <a name="test-connectivity"></a>Проверка подключения

Чтобы проверить подключение, войдите на другую виртуальную машину в той же виртуальной сети. Откройте **SQL Server Management Studio** и подключитесь к экземпляру отказоустойчивого кластера SQL Server.

>[!NOTE]
>При необходимости можно [скачать SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx).

## <a name="limitations"></a>Ограничения

Виртуальные машины Azure поддерживают координатор распределенных транзакций Microsoft (MSDTC) на Windows Server 2019 с хранилищем на общем томе кластера (CSV) и с [Load Balancer ценовой категории "Базовый"](../../../load-balancer/load-balancer-standard-overview.md).

MSDTC не поддерживается на виртуальных машинах Azure в Windows Server 2016 и более ранних версий, поскольку:

- Кластерный ресурс MSDTC нельзя настроить для использования общего хранилища. Windows Server 2016, если вы создаете ресурс MSDTC, не будет показывать доступное общее хранилище для использования, даже если оно существует. Эта проблема устранена в Windows Server 2019.
- Load Balancer ценовой категории "Базовый" не обрабатывает порты RPC.

## <a name="see-also"></a>См. также

[Deploy a two-node S2D SOFS for UPD storage in Azure](https://technet.microsoft.com/windows-server-docs/compute/remote-desktop-services/rds-storage-spaces-direct-deployment) (Развертывание S2D SOFS с двумя узлами для хранилища UPD в Azure).

[Гиперконвергентное решение с использованием локальных дисковых пространств в Windows Server 2016](https://technet.microsoft.com/windows-server-docs/storage/storage-spaces/hyper-converged-solution-using-storage-spaces-direct).

[Локальные дисковые пространства в Windows Server 2016](https://technet.microsoft.com/windows-server-docs/storage/storage-spaces/storage-spaces-direct-overview).

[SQL Server 2016 now supports Windows Server 2016 Storage Spaces Direct](https://blogs.technet.microsoft.com/dataplatforminsider/2016/09/27/sql-server-2016-now-supports-windows-server-2016-storage-spaces-direct/) (SQL Server 2016 теперь поддерживает локальные дисковые пространства Windows Server 2016).
