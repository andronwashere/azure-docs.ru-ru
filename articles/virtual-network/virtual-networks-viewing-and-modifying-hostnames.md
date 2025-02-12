---
title: Просмотр и изменение имен узлов | Документация Майкрософт
description: Просмотр и изменение имен узлов для виртуальных машин Azure, веб- и рабочих ролей для разрешения имен
services: virtual-network
documentationcenter: na
author: genlin
manager: cshepard
editor: tysonn
ms.assetid: c668cd8e-4e43-4d05-acc3-db64fa78d828
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/30/2018
ms.author: genli
ms.openlocfilehash: 3fdb0f566789382a1606b19e4fac179f9ecf40cd
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "62122963"
---
# <a name="viewing-and-modifying-hostnames"></a>Просмотр и изменение имен узлов
Чтобы позволить ссылаться на экземпляры ролей по имени узла, необходимо задать значение имени узла в файле конфигурации службы для каждой роли. Для этого добавьте имя нужного узла в атрибут **vmName** элемента **Role**. Значение атрибута **vmName** используется в качестве основы для имени узла каждого экземпляра роли. Например, если **vmName** имеет значение *webrole* и этот экземпляр имеет три роли, имена узлов экземпляров будут следующими: *webrole0*, *webrole1* и *webrole2*. Вам не нужно указывать имя узла для виртуальных машин в файле конфигурации, так как имя узла виртуальной машины заполняется на основе ее имени. Дополнительные сведения о настройке службы Microsoft Azure см. в статье, посвященной [схеме конфигурации службы Azure (файл CSCFG)](https://msdn.microsoft.com/library/azure/ee758710.aspx).

## <a name="viewing-hostnames"></a>Просмотр имен узлов
Имена узлов виртуальных машин и экземпляров ролей в облачной службе можно просмотреть с помощью приведенных ниже средств.

### <a name="service-configuration-file"></a>Файл конфигурации службы
Файл конфигурации для развернутой службы можно скачать в колонке **настройки** службы на портале Azure. Затем найдите атрибут **vmName** для элемента **Role name**, чтобы увидеть имя узла. Обратите внимание, что имя узла используется в качестве основы для имени узла каждого экземпляра роли. Например, если **vmName** имеет значение *webrole* и этот экземпляр имеет три роли, имена узлов экземпляров будут следующими: *webrole0*, *webrole1* и *webrole2*.

### <a name="remote-desktop"></a>удаленный рабочий стол;
После включения подключений к удаленному рабочему столу (Windows), для удаленного взаимодействия с Windows PowerShell (Windows) или SSH (Linux и Windows) в виртуальных машинах или экземплярах роли можно просматривать имя узла из активного подключения к удаленному рабочему столу разными способами.

* Введите hostname в командной строке или терминале SSH.
* Введите ipconfig/all в командной строке (только Windows).
* Просмотрите имя компьютера в параметрах системы (только Windows).

### <a name="azure-service-management-rest-api"></a>Интерфейс API REST управления службой Azure
В клиенте REST выполните следующие действия:

1. Убедитесь, что у вас есть сертификат клиента для подключения к порталу Azure. Чтобы получить сертификат клиента, выполните действия, описанные в [как: Скачайте и импортируйте параметры публикации и сведений о подписке](https://msdn.microsoft.com/library/dn385850.aspx). 
2. Задайте запись заголовка с именем x-ms-version и значением 2013-11-01.
3. Отправьте запрос в следующем формате: https:\//management.core.windows.net/\<subscrition-id\>/services/hostedservices/\<имя_службы\>? внедрения сведений = true
4. Найдите элемент **HostName** для каждого элемента **RoleInstance**.

> [!WARNING]
> Вы можете также просмотреть суффикс внутреннего домена для облачной службы из ответа на вызов REST, просмотрев элемент **InternalDnsSuffix** или запустив команду ipconfig /all из командной строки в сеансе удаленного рабочего стола (Windows), а также выполнив команду cat /etc/resolv.conf из терминала SSH (Linux).
> 
> 

## <a name="modifying-a-hostname"></a>Изменение имени узла
Вы можете изменить имя узла для любой виртуальной машины или экземпляра роли, отправив измененный файл конфигурации службы или переименовав компьютер из сеанса удаленного рабочего стола.

## <a name="next-steps"></a>Дальнейшие действия
[Разрешение имен (DNS)](virtual-networks-name-resolution-for-vms-and-role-instances.md)

[Схема конфигурации службы Azure (CSCFG-файл)](https://msdn.microsoft.com/library/windowsazure/ee758710.aspx)

[Схема настройки виртуальной сети Azure](https://go.microsoft.com/fwlink/?LinkId=248093)

[Укажите параметры DNS с помощью файлов конфигурации сети](virtual-networks-specifying-a-dns-settings-in-a-virtual-network-configuration-file.md)

