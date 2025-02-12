---
title: Использование SAP Business One на виртуальных машинах Azure | Документация Майкрософт
description: Использование SAP Business One в Azure.
services: virtual-machines-linux,virtual-machines-windows
documentationcenter: ''
author: msjuergent
manager: patfilot
editor: ''
tags: azure-resource-manager
keywords: ''
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 07/15/2018
ms.author: juergent
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 101710b5a57faa37be77ff4b059fa0d494f4e617
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "60835657"
---
# <a name="sap-business-one-on-azure-virtual-machines"></a>Использование SAP Business One на виртуальных машинах Azure
Этот документ содержит руководство для развертывания SAP Business One на виртуальных машинах Azure. Эта документация не заменяет документацию по установке Business One для SAP. Документация должна содержать основные принципы планирования и развертывания инфраструктуры Azure для запуска приложений Business One.

Business One поддерживает две разных базы данных.
- SQL Server – см. статью [SAP Note #928839 - Release Planning for Microsoft SQL Server](https://launchpad.support.sap.com/#/notes/928839) (Примечание к SAP № 928839. Планирование выпуска для Microsoft SQL Server)
- SAP HANA: SAP Business One для SAP HANA для четкой поддержки и извлечения [матрицы доступности продуктов SAP](https://support.sap.com/pam)

Что касается SQL Server, применяются основные соображения развертывания, описанные в разделе [Развертывание СУБД виртуальных машин Azure для SAP NetWeaver ](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/dbms-guide). для SAP HANA соображения упоминаются в этом документе.

## <a name="prerequisites"></a>Технические условия
Чтобы воспользоваться приведенными в этом руководстве сведениями, необходимо иметь базовые знания об использовании следующих компонентов Azure:

- [Виртуальные машины Azure в Windows](https://docs.microsoft.com/azure/virtual-machines/windows/tutorial-manage-vm)
- [Виртуальные машины Azure в Linux](https://docs.microsoft.com/azure/virtual-machines/linux/tutorial-manage-vm)
- [Управление сетями Azure и виртуальными сетями с помощью PowerShell](https://docs.microsoft.com/azure/virtual-machines/windows/tutorial-virtual-network)
- [Сети Azure и виртуальных сетей с помощью интерфейса командной строки](https://docs.microsoft.com/azure/virtual-machines/linux/tutorial-virtual-network)
- [Управление дисками Azure с помощью интерфейса командной строки Azure](https://docs.microsoft.com/azure/virtual-machines/linux/tutorial-manage-disks)

Даже если вы интересуетесь только Business One, документ [SAP NetWeaver на виртуальных машинах Windows. Руководство по планированию и внедрению](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/planning-guide) может быть полезным источником информации.

Предполагается, что вы, как и экземпляр развертывания SAP Business One:

- знакомы с установкой SAP HANA на заданной инфраструктуре, такой как виртуальная машина;
- знакомы с установкой приложения SAP Business One на такую инфраструктуру, как виртуальные машины Azure;
- знакомы с работой SAP Business One и выбранной СУБД;
- знакомы с развертыванием инфраструктуры в Azure.

В этом документе не рассматриваются все эти области.

Помимо документации Azure нужно знать основные примечания к SAP, которые относятся к системе Business One или центральные примечания из SAP для системы Business One.

- [528296 - General Overview Note for SAP Business One Releases and Related Products](https://launchpad.support.sap.com/#/notes/528296) (528296: общий обзор примечания из выпусков SAP Business One и других продуктов)
- [2216195 - Release Updates Note for SAP Business One 9.2, version for SAP HANA](https://launchpad.support.sap.com/#/notes/2216195) (2216195: примечание обновлений выпуска из SAP Business One 9.2, версии для SAP HANA)
- [2483583 - Central Note for SAP Business One 9.3](https://launchpad.support.sap.com/#/notes/2483583) (2483583: центральное примечание к SAP Business One 9.3)
- [2483615 - Release Updates Note for SAP Business One 9.3](https://launchpad.support.sap.com/#/notes/2483615) (2483615: примечание обновлений выпуска к SAP Business One 9.3)
- [2483595 - Collective Note for SAP Business One 9.3 General Issues](https://launchpad.support.sap.com/#/notes/2483595) (2483595: коллективное примечание к SAP Business One 9.3 общие проблемы)
- [2027458 - Collective Consulting Note for SAP HANA-Related Topics of SAP Business One, version for SAP HANA](https://launchpad.support.sap.com/#/notes/2027458) (2027458: примечание коллективного консалтинга к разделам SAP HANA, связанным с SAP Business One, версия для SAP HANA)


## <a name="business-one-architecture"></a>Архитектура Business One
Business One — это двухуровневое приложение.

- Клиентский уровень с "толстым" клиентом
- Уровень базы данных, который содержит схему базы данных для клиента

Более подробный обзор компонентов, выполняемых в клиентской части и часто выполняемых в элементе сервера, см. в [Руководстве администратора SAP Business One](https://help.sap.com/http.svc/rc/879bd9289df34a47af838e67d74ea302/9.3/en-US/AdministratorGuide_SQL.pdf) 

Так как существует сильно зависящее от задержек критически важное взаимодействие между клиентским уровнем и уровнем СУБД, оба уровня при развертывании в Azure должны быть расположены в Azure. Обычно пользователи затем используют RDS в одной или нескольких виртуальных машинах, на которых запущена служба RDS для клиентских компонентов Business One.

### <a name="sizing-vms-for-sap-business-one"></a>Изменение размера виртуальных машин для SAP Business One

Что касается размера клиентских виртуальных машин, требования к ресурсам документируются SAP в документе [Руководство по требованиям к оборудованию SAP Business One](https://help.sap.com/http.svc/rc/011000358700000244612011e/9.3/en-US/B1_Hardware_Requirements_Guide.pdf). Для Azure необходимо сосредоточиться и вычислить требования, перечисленные в разделе документа 2.4.

Так как виртуальные машины Azure содержат клиентские компоненты в Business One и узел СУБД, допускаются только виртуальные машины, поддерживаемые SAP NetWeaver. Список поддерживаемых SAP NetWeaver виртуальных машин Azure см. в разделе [Примечание SAP №1928533](https://launchpad.support.sap.com/#/notes/1928533).

При запуске SAP HANA в качестве серверной части СУБД для Business One только виртуальные машины, которые перечислены в разделе для бизнеса в HANA в [списке сертифицированных платформ IaaS HANA](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html#categories=Microsoft%20Azure%23SAP%20Business%20One), поддерживаются HANA. Эти более сильные ограничения для SAP HANA как системы СУБД не влияют на клиентские компоненты Business One.

### <a name="operating-system-releases-to-use-for-sap-business-one"></a>Выпуски операционной системы для SAP Business One

Как правило, всегда лучше использовать последние версии операционной системы. В частности, в среде Linux появилась новая функциональность Azure, которая представлена вместе с другими более поздними промежуточными выпусками Suse и Red Hat. Настоятельно рекомендуем использовать Windows Server 2016 на Windows-серверах.


## <a name="deploying-infrastructure-in-azure-for-sap-business-one"></a>Развертывание инфраструктуры в Azure для SAP Business One
В следующих нескольких разделах рассматриваются фрагменты инфраструктуры, которые важны для развертывания SAP.

### <a name="azure-network-infrastructure"></a>Сетевая инфраструктура Azure
Сетевая инфраструктура, которую необходимо развернуть в Azure, зависит от того, развертываете ли вы одну систему Business One для себя. Или вы являетесь поставщиком услуг, который размещает десятки систем Business One для клиентов. Также могут возникнуть небольшие отличия в схеме подключения к Azure. Среди различных возможностей есть одна схема, в которой создается VPN-подключение в Azure, и вы расширяете Active Directory через [VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-plan-design) или [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) в Azure.

![Простая конфигурация сети с Business One](./media/business-one-azure/simple-network-with-VPN.PNG)

Представленная упрощенная конфигурация включает несколько экземпляров безопасности, которые позволяют контролировать и ограничивать маршрутизацию. Начинается с 

- Маршрутизатор или брандмауэр на локальных серверах клиента.
- Следующий экземпляр – [группа безопасности сети Azure](https://docs.microsoft.com/azure/virtual-network/security-overview), которую можно использовать для введения правил маршрутизации и безопасности для виртуальной сети Azure, в которой запускается конфигурация системы SAP Business One.
- Чтобы избежать того, что пользователи клиента Business One могут также видеть сервер, на котором запущен сервер Business One, запускающий базу данных, необходимо отделить виртуальную машину, на которой размещен клиент и сервер Business One, в двух разных подсетях виртуальной сети.
- Используйте группу безопасности сети Azure, назначенную для двух разных подсетей, чтобы ограничить доступ к серверу Business One.

Более сложные версии конфигурации сети Azure основаны на [,описанных в рекомендациях по звездообразной архитектуре](https://docs.microsoft.com/azure/architecture/reference-architectures/hybrid-networking/hub-spoke) Azure. Шаблон звездообразной архитектуры изменит первую упрощенную конфигурацию, показанную ниже.


![Настройка звездообразной архитектуры сети с помощью Business One](./media/business-one-azure/hub-spoke-network-with-VPN.PNG)

В случаях, когда пользователи подключаются через Интернет без какого-либо частного подключения к Azure, разработка сети в Azure должена соответствовать принципам, описанным в справочной архитектуре Azure для [сети периметра между Azure и Интернетом](https://docs.microsoft.com/azure/architecture/reference-architectures/dmz/secure-vnet-dmz).

### <a name="business-one-database-server"></a>Сервер базы данных Business One
Для типа базы данных доступны SQL Server и SAP HANA. Независимо от СУБД, необходимо ознакомиться с документом [Соображения для развертывания СУБД виртуальных машин Azure для рабочей нагрузки SAP](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/dbms_guide_general), чтобы получить общее представление о развертываниях СУБД в виртуальных машинах Azure и связанных с ними вопросах сетевого взаимодействия и хранения.

Несмотря на то, что конкретные и общие базы данных уже выделены в документах, необходимо самостоятельно ознакомиться со следующим.

- [Управление доступностью виртуальных машин Windows в Azure](https://docs.microsoft.com/azure/virtual-machines/windows/manage-availability) и [Управление доступностью виртуальных машин Linux в Azure](https://docs.microsoft.com/azure/virtual-machines/linux/manage-availability)
- [Соглашение об уровне обслуживания для виртуальных машин](https://azure.microsoft.com/support/legal/sla/virtual-machines/v1_8/)

Эти документы помогут подобрать типы хранения и конфигурацию высокого уровня доступности.

Как правило необходимо сделать следующее.

- Использовать SSD (цен. категория "Премиум") вместо стандартных жестких дисков. Дополнительные сведения о типах доступных дисков см. в статье о [типах дисков, доступных в Azure](../../windows/disks-types.md).
- Использование управляемых дисков Azure на неуправляемых дисках
- Убедитесь, что операции ввода-вывода в секунду и пропускная способность ввода-вывода настроены для вашей конфигурации дисков
- Объедините тома /hana/data и/hana/log, чтобы получить эффективную стоимость конфигурации хранения


#### <a name="sql-server-as-dbms"></a>SQL Server в качестве СУБД
Чтобы развернуть SQL Server в качестве СУБД для Business One, см. статью [Развертывание СУБД виртуальных машин в Azure SQL Server для SAP NetWeaver](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/dbms_guide_sqlserver). 

Приблизительные оценки размеров для стороны СУБД для SQL Server.

| Количество пользователей | Число виртуальных ЦП | Память | Пример типов виртуальных машин |
| --- | --- | --- | --- |
| до 20 | 4\. | 16 ГБ | D4s_v3, и для E4s_v3 |
| до 40 | 8 | 32 ГБ | D8s_v3, E8s_v3 |
| до 80 | 16 | 64 ГБ | D16s_v3, E16s_v3 |
| до 150 | 32 | 128 ГБ | D32s_v3, E32s_v3 |

Указанные варианты размеров должны дать представление о том, с чего начать. Возможно, для более легкой адаптации в Azure понадобится меньше или больше ресурсов. Переключение между типами виртуальных машин возможно с помощью перезапуска виртуальной машины.

#### <a name="sap-hana-as-dbms"></a>SAP HANA в качестве СУБД
Используя SAP HANA в качестве СУБД в следующих разделах, необходимо следовать указаниям в документе [Руководство по работе SAP HANA в Azure ](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-vm-operations).

Для настройки высокой доступности и аварийного восстановления вокруг SAP HANA в качестве базы данных для Business One в Azure необходимо прочитать документацию [Обеспечение высокого уровня доступности для SAP HANA на виртуальных машинах Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-availability-overview) и документацию, указанную в этом документе.

Сведения о резервном копировании и восстановлении стратегий SAP HANA см. в [Руководстве по резервному копированию SAP HANA на виртуальных машинах Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-backup-guide) и документации, указанной в этом документе.

 
### <a name="business-one-client-server"></a>Клиентский сервер Business One
Для этих компонентов рекомендации по хранению не являются основной проблемой. Тем не менее, необходимо иметь надежную платформу. Таким образом, следует использовать хранилище Azure класса Premium для этой виртуальной машины даже для базового виртуального жесткого диска. Изменение размера виртуальной машины на основе данных, указанных в [Руководстве по требованиям к оборудованию SAP Business One](https://help.sap.com/http.svc/rc/011000358700000244612011e/9.3/en-US/B1_Hardware_Requirements_Guide.pdf). Для Azure необходимо сосредоточиться и вычислить требования, перечисленные в разделе документа 2.4. При расчете требований, нужно сравнить их со следующими документами, чтобы найти идеальную виртуальную машину.

- [Размеры виртуальных машин Windows в Azure](https://docs.microsoft.com/azure/virtual-machines/windows/sizes)
- [Примечание к SAP №1928533](https://launchpad.support.sap.com/#/notes/1928533)

Сравните количество процессоров и памяти, необходимых для документирования Microsoft. Также при выборе виртуальной машины помните о пропускной способности сети.








