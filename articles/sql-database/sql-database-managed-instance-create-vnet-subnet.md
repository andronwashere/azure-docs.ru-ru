---
title: Создание виртуальной сети для Управляемого экземпляра Базы данных SQL Azure | Документация Майкрософт
description: В этой статье описывается создание виртуальной сети, в которой можно развернуть Управляемый экземпляр Базы данных SQL Azure.
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: srdan-bozovic-msft
ms.author: srbozovi
ms.reviewer: sstein, bonova, carlrab
manager: craigg
ms.date: 01/15/2019
ms.openlocfilehash: d6e205c23545eb4a01ce58a8bc2b63c58200e32a
ms.sourcegitcommit: 920ad23613a9504212aac2bfbd24a7c3de15d549
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/15/2019
ms.locfileid: "68228289"
---
# <a name="create-a-virtual-network-for-azure-sql-database-managed-instance"></a>Создание виртуальной сети для Управляемого экземпляра Базы данных SQL Azure

В этой статье описывается создание допустимой виртуальной сети, в которой можно развернуть Управляемый экземпляр Базы данных SQL Azure.

Управляемый экземпляр Базы данных SQL Azure необходимо развернуть в [виртуальной сети](../virtual-network/virtual-networks-overview.md) Azure. Это развертывание поддерживает следующие сценарии:

- Защищенный частный IP-адрес.
- Подключение к управляемому экземпляру непосредственно из локальной сети
- Подключение управляемого экземпляра к связанному серверу или другому локальному хранилищу данных
- Подключение управляемого экземпляра к ресурсам Azure  

> [!Note]
> Вы должны [определить размер подсети для Управляемого экземпляра](sql-database-managed-instance-determine-size-vnet-subnet.md) перед развертыванием первого экземпляра. Вы не можете изменить размер подсети после того, как поместите в нее ресурсы.
>
> Если вы планируете использовать имеющуюся виртуальную сеть, необходимо изменить ее конфигурацию, чтобы она соответствовала управляемому экземпляру. Дополнительные сведения см. в статье [Настройка имеющейся виртуальной сети для Управляемого экземпляра Базы данных SQL Azure](sql-database-managed-instance-configure-vnet-subnet.md).
>
> После создания управляемого экземпляра перемещение управляемого экземпляра или виртуальной сети в другую группу ресурсов или подписку не поддерживается.


## <a name="create-a-virtual-network"></a>Создание виртуальной сети

Самый простой способ создания и настройки виртуальной сети — использовать шаблон развертывания Azure Resource Manager.

1. Войдите на портал Azure.

2. Выберите **Развертывание в Azure**:

   <a target="_blank" href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-sql-managed-instance-azure-environment%2Fazuredeploy.json" rel="noopener" data-linktype="external"> <img src="https://azuredeploy.net/deploybutton.png" data-linktype="external"> </a>

   Эта кнопка откроет форму, которую можно использовать для настройки сетевой среды, в которой можно развернуть Управляемый экземпляр.

   > [!Note]
   > Этот шаблон Azure Resource Manager развертывает виртуальную сеть с двумя подсетями. Одна подсеть с именем **ManagedInstances** зарезервирована для Управляемого экземпляра и имеет таблицу предварительно настроенных маршрутов. Другая называется **Default** и используется другими ресурсами, которые должны получать доступ к Управляемому экземпляру (например, виртуальные машины Azure).

3. Настройте сетевую среду. В приведенной ниже форме можно настроить параметры сетевой среды:

   ![Шаблон Resource Manager для настройки сети Azure](./media/sql-database-managed-instance-vnet-configuration/create-mi-network-arm.png)

   Вы можете изменить имена виртуальной сети и подсетей и настроить диапазоны IP-адресов, связанные с сетевыми ресурсами. После нажатия кнопки **Купить** эта форма создаст и настроит вашу среду. Если вам не нужны две подсети, можно удалить подсеть, заданную по умолчанию.

## <a name="next-steps"></a>Следующие шаги

- Дополнительные сведения см. в статье [Использование Управляемого экземпляра Базы данных SQL с виртуальными сетями и почти полной совместимостью](sql-database-managed-instance.md).
- [Архитектура подключения к Управляемому экземпляру Базы данных SQL Azure](sql-database-managed-instance-connectivity-architecture.md).
- Узнайте, как [изменять имеющиеся виртуальные сети для Управляемого экземпляра](sql-database-managed-instance-configure-vnet-subnet.md).
- Чтобы узнать, как создать виртуальную сеть и Управляемый экземпляр, а также восстановить базу данных из резервной копии базы данных, ознакомьтесь со статьей [Краткое руководство. Создание Управляемого экземпляра Базы данных SQL Azure](sql-database-managed-instance-get-started.md).
- Во избежание проблем при настройке DNS ознакомьтесь со сведениями в статье [Настройка пользовательской службы DNS для Управляемого экземпляра Базы данных SQL Azure](sql-database-managed-instance-custom-dns.md).
