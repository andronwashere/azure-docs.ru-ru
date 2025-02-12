---
title: Высокий уровень доступности и аварийное восстановление для SQL Server | Документация Майкрософт
description: Обсуждение различных типов стратегий HADR для SQL Server на виртуальных машинах Azure.
services: virtual-machines-windows
documentationcenter: na
author: MikeRayMSFT
manager: craigg
editor: ''
tags: azure-service-management
ms.assetid: 53981f7e-8370-4979-b26a-93a5988d905f
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 06/27/2017
ms.author: mikeray
ms.openlocfilehash: aac7a573b254c2e46d2ecf25d176df28e5e5d7da
ms.sourcegitcommit: e5dcf12763af358f24e73b9f89ff4088ac63c6cb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/14/2019
ms.locfileid: "67137697"
---
# <a name="high-availability-and-disaster-recovery-for-sql-server-in-azure-virtual-machines"></a>Высокий уровень доступности и аварийное восстановление для SQL Server на виртуальных машинах Azure

Виртуальные машины Microsoft Azure с SQL Server могут помочь удешевить базу данных, обеспечивающую высокий уровень доступности и аварийное восстановление (HADR). Виртуальные машины Azure поддерживают большинство решений HADR для SQL Server как для использования только в службе Azure, так и в качестве гибридных решений. Если решение предназначено только для использования в службе Azure, в ней выполняется вся система HADR. В гибридной конфигурации часть решения выполняется в Azure, а другая часть — в локальной сети организации. Гибкость среды Azure позволяет полностью или частично перемещать ресурсы в Azure в соответствии с бюджетом и требованиями систем баз данных SQL Server к HADR.

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-both-include.md)]

## <a name="understanding-the-need-for-an-hadr-solution"></a>Целесообразность использования решения HADR
Вам следует убедиться, что ваша система баз данных поддерживает возможности HADR, оговоренные в соглашении об уровне обслуживания (SLA). Тот факт, что Azure обеспечивает механизмы высокого уровня доступности, такие как восстановление облачных служб и обнаружение восстановления после сбоя для виртуальных машин, сам по себе не гарантирует, что вы выполните условия этого соглашения. Эти механизмы поддерживают высокий уровень доступности исключительно виртуальных машин, а не экземпляра SQL Server, который выполняется на этих виртуальных машинах. В работе экземпляра SQL Server может возникнуть ошибка, даже если виртуальная машина подключена и исправна. Кроме того, даже доступные в Azure механизмы обеспечения высокого уровня доступности допускают простои виртуальных машин, связанные с такими событиями, как восстановление после сбоев программного или аппаратного обеспечения либо обновление операционной системы.

Кроме того, геоизбыточного хранилища (GRS) в Azure, которое реализуется с помощью такой функции, как георепликация, может оказаться недостаточно для аварийного восстановления баз данных. Так как при георепликации данные отправляются асинхронно, последние обновления могут быть потеряны в случае сбоя. Дополнительные сведения об ограничениях относительно георепликации приведены [в разделе со сведениями о том, что георепликация не поддерживается для файлов данных и журналов на отдельных дисках](#geo-replication-support) .

## <a name="hadr-deployment-architectures"></a>Архитектуры развертывания HADR
В Azure поддерживаются следующие технологии HADR для SQL Server.

* [Группы доступности Always On](https://technet.microsoft.com/library/hh510230.aspx)
* [Экземпляры отказоустойчивого кластера Always On](https://technet.microsoft.com/library/ms189134.aspx)
* [Доставка журналов](https://technet.microsoft.com/library/ms187103.aspx)
* [Резервное копирование и восстановление SQL Server с помощью службы хранилища BLOB-объектов Azure](https://msdn.microsoft.com/library/jj919148.aspx)
* [Зеркальное отображение базы данных (SQL Server)](https://technet.microsoft.com/library/ms189852.aspx) — устаревшее в SQL Server 2016

Эти технологии можно объединить, чтобы реализовать решение SQL Server, поддерживающее возможности высокого уровня доступности и аварийного восстановления. В зависимости от используемой технологии для гибридного развертывания может потребоваться VPN-туннель с виртуальной сетью Azure. В разделах ниже приведено несколько примеров архитектур развертывания.

## <a name="azure-only-high-availability-solutions"></a>Только в Azure: Решения высокого уровня доступности

Решение для обеспечения высокого уровня доступности SQL Server можно реализовать на уровне базы данных с помощью групп доступности AlwaysOn. Его можно также создать на уровне экземпляра с помощью экземпляров отказоустойчивого кластера AlwaysOn. Чтобы повысить избыточность на обоих уровнях, можно также создать группы доступности для экземпляров отказоустойчивого кластера. 

| Технология | Примеры архитектур |
| --- | --- |
| **Группы доступности** |Реплики доступности, выполняемые на виртуальных машинах Azure в одном регионе, обеспечивают высокий уровень доступности. Необходимо настроить виртуальную машину контроллера домена, так как для отказоустойчивой кластеризации Windows требуется домен Active Directory.<br/> ![Группы доступности](./media/virtual-machines-windows-sql-high-availability-dr/azure-only-ha-always-on.png)<br/>Дополнительные сведения см. в статье [Автоматическая настройка групп доступности AlwaysOn на виртуальных машинах Azure с использованием Resource Manager](virtual-machines-windows-portal-sql-alwayson-availability-groups.md). |
| **Экземпляры отказоустойчивого кластера** |Экземпляры отказоустойчивого кластера (FCI), которые требуют наличия общего хранилища, можно создавать тремя способами.<br/><br/>1. Отказоустойчивый кластер с двумя узлами, выполняющийся на виртуальных машинах Azure, с хранилищем, подключенным с использованием [локальных дисковых пространств Windows Server 2016 \(S2D\)](virtual-machines-windows-portal-sql-create-failover-cluster.md), для предоставления программной виртуальной сети SAN.<br/><br/>2. Отказоустойчивый кластер с двумя узлами, выполняющийся на виртуальных машинах Azure, с хранилищем, поддерживаемым сторонним решением кластеризации. Конкретный пример, в котором используется SIOS DataKeeper, приведен в публикации блога [High Availability for a file share using WSFC, ILB and 3rd-party Software SIOS Datakeeper](https://azure.microsoft.com/blog/high-availability-for-a-file-share-using-wsfc-ilb-and-3rd-party-software-sios-datakeeper/) (Обеспечение высокого уровня доступности файлового ресурса с помощью WSFC, внутренней подсистемы балансировки нагрузки и стороннего ПО SIOS Datakeeper).<br/><br/>3. Отказоустойчивый кластер с двумя узлами, выполняющийся на виртуальных машинах Azure, с удаленным целевым общим блочным хранилищем iSCSI, подключенным через ExpressRoute. Например, решение NetApp Private Storage (NPS) предоставляет цели iSCSI через ExpressRoute с Equinix для виртуальных машин Azure.<br/><br/>По любым вопросам, связанным с доступом к данным при отработке отказа, касающимся сторонних решений репликации данных и общих хранилищ, следует обратиться к их поставщикам.<br/><br/>Обратите внимание, что использование экземпляра FCI поверх [хранилища файлов Azure](https://azure.microsoft.com/services/storage/files/) пока не поддерживается, так как это решение не использует хранилище уровня "Премиум". Мы работаем над тем, чтобы обеспечить поддержку этого сценария в ближайшее время. |

## <a name="azure-only-disaster-recovery-solutions"></a>Только в Azure: Решения аварийного восстановления
У вас может быть решение аварийного восстановления для баз данных SQL Server в Azure, использующее группы доступности, зеркальное отображение базы данных или архивацию и восстановление с большими двоичными объектами хранилища.

| Технология | Примеры архитектур |
| --- | --- |
| **Группы доступности** |Реплики доступности, выполняемые в нескольких центрах обработки данных на виртуальных машинах Azure для аварийного восстановления. Это межрегиональное решение обеспечивает защиту от простоя всего сайта. <br/> ![Группы доступности](./media/virtual-machines-windows-sql-high-availability-dr/azure-only-dr-alwayson.png)<br/>В пределах региона все реплики должны находиться в одной облачной службе и одной виртуальной сети. Так как каждый регион будет включать отдельную виртуальную сеть, для этих решений требуется подключение между виртуальными сетями. Чтобы узнать больше, ознакомьтесь с разделом [Настройка подключения между виртуальными сетями на портале Azure](../../../vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal.md). Подробные инструкции см. в статье [Настройка группы доступности AlwaysOn на виртуальных машинах Azure в разных регионах](virtual-machines-windows-portal-sql-availability-group-dr.md).|
| **Зеркальное отображение базы данных** |Основной, зеркальный и другие серверы, выполняемые в разных центрах обработки данных для аварийного восстановления. Необходимо выполнить развертывание с использованием сертификатов сервера. Зеркальное отображение базы данных SQL Server не поддерживается для SQL Server 2008 и SQL Server 2008 R2 на виртуальной Машине Azure. <br/>![Зеркальное отображение базы данных](./media/virtual-machines-windows-sql-high-availability-dr/azure-only-dr-dbmirroring.png) |
| **Резервное копирование и восстановление с помощью службы хранилища больших двоичных объектов Azure** |Рабочие базы данных архивируются напрямую в хранилище BLOB-объектов в другом центре обработки данных для аварийного восстановления.<br/>![Резервное копирование и восстановление](./media/virtual-machines-windows-sql-high-availability-dr/azure-only-dr-backup-restore.png)<br/>Дополнительные сведения см. в статье [Резервное копирование и восстановление SQL Server в виртуальных машинах Azure](virtual-machines-windows-sql-backup-recovery.md). |
| **Репликация и отработка отказа SQL Server в Azure с помощью Azure Site Recovery** |Рабочая среда SQL Server одного из центров обработки данных Azure реплицируется непосредственно в службу хранилища Azure в другом центре обработки данных Azure в целях аварийного восстановления.<br/>![Репликация с помощью Azure Site Recovery](./media/virtual-machines-windows-sql-high-availability-dr/azure-only-dr-standalone-sqlserver-asr.png)<br/>Дополнительные сведения см. в разделе [Защита SQL Server с помощью аварийного восстановления SQL Server и Azure Site Recovery](../../../site-recovery/site-recovery-sql.md). |


## <a name="hybrid-it-disaster-recovery-solutions"></a>Гибридные ИТ: Решения аварийного восстановления
У вас может быть решение аварийного восстановления для баз данных SQL Server в гибридной ИТ-среде, использующее группы доступности, зеркальное отображение базы данных и доставку журналов, а также архивацию и восстановление с помощью хранилища больших двоичных объектов Azure.

| Технология | Примеры архитектур |
| --- | --- |
| **Группы доступности** |Некоторые реплики доступности, выполняемые на виртуальных машинах Azure, и другие реплики, которые выполняются локально для аварийного восстановления на нескольких сайтах. Рабочий сайт может размещаться локально или в центре обработки данных Azure.<br/>![Группы доступности](./media/virtual-machines-windows-sql-high-availability-dr/hybrid-dr-alwayson.png)<br/>Так как все реплики доступности должны находиться в одном отказоустойчивом кластере, он должен охватывать обе сети (отказоустойчивый кластер с несколькими подсетями). Для этой конфигурации требуется установить VPN-подключение между сетью Azure и локальной сетью.<br/><br/>Для успешного аварийного восстановления баз данных также следует установить реплику контроллера домена на сайте аварийного восстановления.<br/><br/>Для добавления реплики Azure в существующую группу доступности AlwaysOn можно воспользоваться мастером добавления реплики в SSMS. Дополнительные сведения см. в разделе руководства: Расширение вашей группы доступности AlwaysOn в Azure. |
| **Зеркальное отображение базы данных** |Один участник выполняется на виртуальной машине Azure, а другой — локально для межсайтового аварийного восстановления с использованием сертификатов сервера. Участники не обязательно должны находиться в одном домене Active Directory. Кроме того, не требуется VPN-подключение.<br/>![Зеркальное отображение базы данных](./media/virtual-machines-windows-sql-high-availability-dr/hybrid-dr-dbmirroring.png)<br/>В другом сценарии зеркального отображения базы данных один участник выполняется на виртуальной машине Azure, а другой — локально в том же домене Active Directory для межсайтового аварийного восстановления. Требуется [VPN-подключение между виртуальной сетью Azure и локальной сетью](../../../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md).<br/><br/>Для успешного аварийного восстановления баз данных также следует установить реплику контроллера домена на сайте аварийного восстановления. Зеркальное отображение базы данных SQL Server не поддерживается для SQL Server 2008 и SQL Server 2008 R2 на виртуальной Машине Azure. |
| **Доставка журналов** |Один сервер запущен на виртуальной машине Azure, а другой — в локальной сети для межсайтового аварийного восстановления. Доставка журналов зависит от общего доступа к файлам Windows, поэтому требуется VPN-подключение между виртуальной сетью Azure и локальной сетью.<br/>![Доставка журналов](./media/virtual-machines-windows-sql-high-availability-dr/hybrid-dr-log-shipping.png)<br/>Для успешного аварийного восстановления баз данных также следует установить реплику контроллера домена на сайте аварийного восстановления. |
| **Резервное копирование и восстановление с помощью службы хранилища больших двоичных объектов Azure** |Локальные рабочие базы данных архивируются напрямую в хранилище BLOB-объектов для аварийного восстановления.<br/>![Резервное копирование и восстановление](./media/virtual-machines-windows-sql-high-availability-dr/hybrid-dr-backup-restore.png)<br/>Дополнительные сведения см. в статье [Резервное копирование и восстановление SQL Server в виртуальных машинах Azure](virtual-machines-windows-sql-backup-recovery.md). |
| **Репликация и отработка отказа SQL Server в Azure с помощью Azure Site Recovery** |Локальная рабочая среда SQL Server реплицируется непосредственно в службу хранилища Azure в целях аварийного восстановления.<br/>![Репликация с помощью Azure Site Recovery](./media/virtual-machines-windows-sql-high-availability-dr/hybrid-dr-standalone-sqlserver-asr.png)<br/>Дополнительные сведения см. в разделе [Защита SQL Server с помощью аварийного восстановления SQL Server и Azure Site Recovery](../../../site-recovery/site-recovery-sql.md). |

## <a name="important-considerations-for-sql-server-hadr-in-azure"></a>Важные соображения в отношении HADR для SQL Server в Azure
Виртуальные машины, хранилища и сети Azure имеют рабочие характеристики, отличные от характеристик локальной, невиртуализированной ИТ-инфраструктуры. Чтобы успешно реализовать решение HADR для SQL Server в Azure, вы должны понимать эти различия и учитывать их при разработке решения.

### <a name="high-availability-nodes-in-an-availability-set"></a>Узлы с высоким уровнем доступности в группе доступности
Группы доступности в Azure позволяют размещать узлы с высоким уровнем доступности в отдельные домены сбоя и домены обновления. Для размещения виртуальных машин Azure в одной группе доступности их необходимо развернуть в одной облачной службе. В одной группе доступности можно разместить только узлы из одной облачной службы. Дополнительные сведения см. в статье [Управление доступностью виртуальных машин](../manage-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

### <a name="failover-cluster-behavior-in-azure-networking"></a>Поведение отказоустойчивого кластера в сети Azure
Несоответствие службы DHCP в Azure стандарту RFC может привести к сбою некоторых создаваемых конфигураций отказоустойчивого кластера, так как сетевому имени кластера может быть назначен повторяющийся IP-адрес, например IP-адрес одного из узлов кластера. Это актуально, если вам нужно реализовать группы доступности, зависящие от функции отказоустойчивого кластера Windows.

Рассмотрим сценарий, в котором вы создаете и подключаете к сети кластер с двумя узлами.

1. Кластер подключается к сети, после чего узел NODE1 запрашивает динамически назначаемый IP-адрес для сетевого имени кластера.
2. Служба DHCP не назначает других IP-адресов, кроме собственного IP-адреса узла NODE1, так как она распознает, что запрос поступает от самого узла NODE1.
3. Windows обнаруживает, что повторяющийся адрес назначен как узлу NODE1, так и сетевому имени отказоустойчивого кластера, поэтому группе кластера по умолчанию не удается подключиться к сети.
4. Группа кластера по умолчанию перемещается на узел NODE2, который рассматривает IP-адрес узла NODE1 как IP-адрес кластера и подключает группу кластера по умолчанию к сети.
5. Когда узел NODE2 пытается установить соединение с узлом NODE1, пакеты, направляемые на NODE1, не покидают узел NODE2, так как он разрешает IP-адрес узла NODE1 для самого себя. Узлу NODE2 не удается установить соединение с узлом NODE1, что приводит к потере кворума и завершению работы кластера.
6. Тем временем узел NODE1 может отправлять пакеты на узел NODE2, который не может отвечать на них. Узел NODE1 теряет кворум и завершает работу кластера.

Этого можно избежать путем назначения сетевому имени кластера неиспользуемого статического IP-адреса, например IP-адреса локальной связи (такого как 169.254.1.1), чтобы позволит подключить сетевое имя кластера к сети. Указания по упрощению этого процесса см. в статье [How to Configure Windows Failover Cluster in Azure for AlwaysOn Availability Groups](https://social.technet.microsoft.com/wiki/contents/articles/14776.configuring-windows-failover-cluster-in-windows-azure-for-alwayson-availability-groups.aspx) (Как настроить отказоустойчивый кластер Windows в Azure для групп доступности Always On).

Дополнительные сведения см. в статье [Автоматическая настройка групп доступности AlwaysOn на виртуальных машинах Azure с использованием Resource Manager](virtual-machines-windows-portal-sql-alwayson-availability-groups.md).

### <a name="availability-group-listener-support"></a>Поддержка прослушивателя группы доступности
Прослушиватели группы доступности поддерживаются на виртуальных машинах Azure, работающих под управлением Windows Server 2008 R2, Windows Server 2012, Windows Server 2012 R2 и Windows Server 2016. Это поддержка возможна благодаря использованию конечных точек балансировки нагрузки на виртуальных машинах Azure, представляющих собой узлы группы доступности. Необходимо выполнить специальные шаги по настройке, чтобы прослушиватели работали с клиентскими приложениями, запущенными в Azure и выполняемыми локально.

Существует два основных способа настройки: внешний (общедоступный) или внутренний прослушиватель. Внешний (общедоступный) прослушиватель использует балансировщик нагрузки для Интернета и связан с общедоступным виртуальным IP-адресом, доступным через Интернет. Внутренний прослушиватель использует внутренний балансировщик нагрузки и поддерживает только клиенты, находящиеся в той же виртуальной сети. Для обоих типов балансировщика нагрузки необходимо включить прямой ответ от сервера. 

Если группа доступности охватывает несколько подсетей Azure (например, развернутую систему, включающую несколько регионов Azure), то строка подключения клиента должна включать текст **MultisubnetFailover=True**. В результате будут выполняться попытки параллельного подключения к репликам в разных подсетях. Инструкции по настройке прослушивателя см. в разделах:

* [Настройка прослушивателя внутренней подсистемы балансировки нагрузки для групп доступности в Azure](virtual-machines-windows-portal-sql-ps-alwayson-int-listener.md).
* [Настройка внешнего прослушивателя для групп доступности в Azure](../sqlclassic/virtual-machines-windows-classic-ps-sql-ext-listener.md).

Вы по-прежнему можете подключаются к отдельным репликам доступности, напрямую подключаясь к экземпляру службы. Кроме того, так как группы доступности обратно совместимы с клиентами зеркального отображения базы данных, вы можете подключаться к репликам доступности как к участникам зеркального отображения, пока реплики настроены аналогично зеркальному отображению базы данных:

* имеется по одной первичной и вторичной реплике;
* вторичная реплика настроена как недоступная для чтения (для параметра **Вторичная реплика для чтения** установлено значение **Нет**).

Ниже приведен пример строки подключения клиента, соответствующий этой конфигурации зеркального отображения базы данных с использованием ADO.NET или SQL Server Native Client.

    Data Source=ReplicaServer1;Failover Partner=ReplicaServer2;Initial Catalog=AvailabilityDatabase;

Дополнительные сведения о подключении клиента см. в следующих статьях.

* [Использование ключевых слов строки подключения с собственным клиентом SQL Server](https://msdn.microsoft.com/library/ms130822.aspx)
* [Подключение клиентов к сеансу зеркального отображения базы данных (SQL Server)](https://technet.microsoft.com/library/ms175484.aspx)
* [Подключение к прослушивателю группы доступности в гибридной ИТ-среде](https://blogs.msdn.com/b/sqlalwayson/archive/2013/02/14/connecting-to-availability-group-listener-in-hybrid-it.aspx)
* [Прослушиватели групп доступности, возможность подключения клиентов и отработка отказа приложений (SQL Server)](https://technet.microsoft.com/library/hh213417.aspx)
* [Использование строк подключения к зеркально отображаемой базе данных с группами доступности](https://technet.microsoft.com/library/hh213417.aspx)

### <a name="network-latency-in-hybrid-it"></a>Задержки сети в гибридной ИТ-среде
Решение HADR следует развернуть, исходя из предположения, что между локальной сетью и Azure могут возникать значительные задержки сети. При развертывании реплик в Azure вместо синхронной следует использовать асинхронную фиксацию для режима синхронизации. Развертывая серверы зеркального отображения базы данных как локально, так и в Azure, используйте режим высокой производительности вместо режима высокой безопасности.

### <a name="geo-replication-support"></a>Поддержка георепликации
Георепликация на дисках Azure не поддерживает хранение файлов данных и журнала из одной базы данных на отдельных дисках. GRS реплицирует изменения на каждом диске независимо и асинхронно. Этот механизм обеспечивает единый порядок записи на одном диске в геореплицированной копии, но не в геореплицированных копиях нескольких дисков. Если настроить базу данных на хранение файлов данных и журнала на отдельных дисках, восстановленные после сбоя диски могут содержать копию файла данных, созданную позже файла журнала, что нарушит упреждающее протоколирование в SQL Server и свойствах ACID транзакций. Если вы не можете отключить георепликацию в учетной записи хранения, все файлы данных и журнала базы данных следует хранить на одном диске. Если необходимо использовать несколько дисков из-за размера базы данных, для обеспечения избыточности данных необходимо развернуто одно из перечисленных выше решений для аварийного восстановления.

## <a name="next-steps"></a>Дальнейшие действия
Если требуется создать виртуальную машину Azure с SQL Server, то см. статью [Подготовка виртуальной машины SQL Server на портале Azure](virtual-machines-windows-portal-sql-server-provision.md).

Чтобы обеспечить максимальную производительность SQL Server на виртуальной машине Azure, см. указания в статье [Рекомендации по оптимизации производительности SQL Server в виртуальных машинах Azure](virtual-machines-windows-sql-performance.md).

Другие темы, связанные с запуском SQL Server на виртуальных машинах Azure, рассматриваются в статье [SQL Server на виртуальных машинах Azure](virtual-machines-windows-sql-server-iaas-overview.md).

### <a name="other-resources"></a>Другие ресурсы
* [Установка нового леса Active Directory в виртуальной сети Azure](../../../active-directory/active-directory-new-forest-virtual-machine.md)
* [Создание отказоустойчивого кластера для групп доступности на виртуальной машине Azure](https://gallery.technet.microsoft.com/scriptcenter/Create-WSFC-Cluster-for-7c207d3a)

