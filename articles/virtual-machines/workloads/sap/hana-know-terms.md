---
title: Определение терминов SAP HANA в Azure (крупные экземпляры) | Документация Майкрософт
description: Определение терминов SAP HANA в Azure (крупные экземпляры).
services: virtual-machines-linux
documentationcenter: ''
author: RicksterCDN
manager: gwallace
editor: ''
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 11/20/2018
ms.author: juergent
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 2564e0d34383f7c1daff5b02a871778fb90546cc
ms.sourcegitcommit: 10251d2a134c37c00f0ec10e0da4a3dffa436fb3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/13/2019
ms.locfileid: "67868994"
---
# <a name="know-the-terms"></a>Определение терминов

В этом руководстве широко используются некоторые общие определения. Ознакомьтесь со следующими терминами и их описанием.

- **IaaS**: Инфраструктура как услуга.
- **PaaS**: Платформа как услуга.
- **SaaS**: Программное обеспечение как услуга.
- **Компонент SAP**: Отдельное приложение SAP, например, компонент ERP Central (ECC), Бизнес-хранилище (BW), диспетчер решений или Enterprise Portal (EP). Компоненты SAP могут основываться на традиционной технологии ABAP или Java или быть приложениями не на основе NetWeaver, например бизнес-объектами.
- **Среда SAP**: Один или несколько компонентов SAP, логически сгруппированных для выполнения бизнес-функций, таких как разработка, контроль качества, обучение, аварийное восстановление или рабочая среда.
- **Ландшафт SAP**: Относится ко всем ресурсам SAP в своей ландшафтной ИТ. Ландшафт SAP включает все рабочие и нерабочие среды.
- **Система SAP**: Сочетание уровня СУБД и уровня приложений, например системы разработки SAP ERP, тестовой системы SAP BW и рабочей системы SAP CRM. В развертываниях Azure не поддерживается разделение этих двух уровней между локальной средой и Azure. Система SAP должна быть развернута полностью в локальной среде или полностью в Azure. При этом вы можете развернуть различные системы ландшафта SAP как в Azure, так и локально. Например, системы разработки и тестирования SAP CRM можно развернуть в Azure, а рабочую систему SAP CRM — в локальной среде. Решение SAP HANA в Azure (крупные экземпляры) предназначено для размещения уровня приложений систем SAP на виртуальных машинах и соответствующего экземпляра SAP HANA в единице в стеке крупных экземпляров SAP HANA в Azure.
- **Метка крупных экземпляров**: Стек аппаратной инфраструктуры, который SAP HANA сертифицирован TDI и выделен для запуска SAP HANA экземпляров в Azure.
- **SAP HANA в Azure (крупные экземпляры):** Официальное название предложения в Azure для запуска экземпляров HANA на SAP HANA оборудовании, сертифицированном с помощью TDI, которое разворачивается в метках крупных экземпляров в разных регионах Azure. Связанный термин *крупные экземпляры HANA* — сокращенное название *SAP HANA в Azure (крупные экземпляры)* , которое широко используется в этом техническом руководстве по развертыванию.
- **Между организациями**: Описание сценария развертывания виртуальных машин в подписке Azure с подключением типа "сеть — сеть", несколькими сайтами или Azure ExpressRoute между локальными центрами обработки данных и Azure. В документации Azure развертывания такого рода также называются распределенными развертываниями. Такое подключение используется для расширения в Azure локальных доменов, локальной службы Active Directory, OpenLDAP и локальной службы DNS. Локальная среда распространяется на ресурсы Azure, входящие в подписки Azure. При таком расширении виртуальные машины могут быть частью локального домена. 

   Пользователи локального домена имеют доступ к серверам и могут запускать службы на этих виртуальных машинах (например, службы СУБД). Возможно взаимодействие и разрешение имен между виртуальными машинами, развернутыми в локальной сети и в Azure. Это типичный сценарий, при котором развертывается большинство ресурсов SAP. Дополнительные сведения см. в статье [VPN-шлюз Azure](../../../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) и [Создание виртуальной сети с подключением типа "сеть — сеть" с помощью портал Azure](../../../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
- **Tenant:** Клиент, развернутый в метке крупных экземпляров HANA, изолирован в *клиенте.* Клиент изолируется от других клиентов на уровне сети, хранилища и вычислений. Таким образом, единицы хранения и вычисления, назначенные разным клиентам, не могут видеть друг друга или взаимодействовать друг с другом на уровне стека крупных экземпляров HANA. Пользователь может выполнить развертывание в разные клиенты. Но и в таком случае клиенты не будут взаимодействовать на уровне стека крупных экземпляров HANA.
- **Категория SKU**: Для крупных экземпляров HANA предлагаются следующие две категории номеров SKU:
    - **Класс типа I**: S72, S72m, S96, S144, S144m, S192, S192m и S192xm
    - **Класс типа II**: S384, S384m, S384xm, S384xxm, S576m, S576xm, S768m, S768xm и S960m
- **Метка**: Определяет внутренний размер развертывания Майкрософт для крупных экземпляров HANA. Прежде чем можно будет развернуть единицы крупных экземпляров HANA, в расположении центра обработки данных необходимо развернуть метку крупных экземпляров HANA, состоящую из стоек для вычислений, сети и хранения. Такое развертывание называется отметкой крупных экземпляров HANA или версией 4 (см. ниже). Мы используем альтернативный вариант **строки крупных экземпляров** .
- **Редакция**: Для меток крупных экземпляров HANA существуют две различные версии штампа. Они отличаются по архитектуре и сходству с узлами виртуальных машин Azure.
    - "Редакция 3" (редакция 3): является исходным макетом, который был развернут из середины года 2016
    - "Редакция 4" (версия 4). — это новая схема, которая обеспечивает более близкое сходство с узлами виртуальных машин Azure и снижает задержку сети между виртуальными машинами Azure и единицами крупных экземпляров HANA. 

Существует множество дополнительных ресурсов, посвященных развертыванию рабочих нагрузок SAP в облаке. Если вы планируете развернуть SAP HANA в Azure, рекомендуется ознакомиться с особенностями сред Azure IaaS и подготовиться к развертыванию рабочих нагрузок SAP в такой среде. Прежде чем продолжить работу с руководством, ознакомьтесь со статьей [Размещение и выполнение сценариев рабочей нагрузки SAP с помощью Azure](get-started.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). 

**Дальнейшие действия**
- См. раздел [HLI Certification](hana-certification.md) (Сертификация HLI)