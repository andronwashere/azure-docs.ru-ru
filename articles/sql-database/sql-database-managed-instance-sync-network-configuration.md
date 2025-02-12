---
title: Синхронизация конфигурации сети в Службе приложений Azure | Документация Майкрософт
description: В этой статье описано, как синхронизировать конфигурацию сети для плана размещения Службы приложений Azure.
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
ms.date: 12/13/2018
ms.openlocfilehash: 0d7920080fd61389741fbe785f5141003bef5251
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "61314726"
---
# <a name="sync-networking-configuration-for-azure-app-service-hosting-plan"></a>Синхронизация конфигурации сети для плана размещения Службы приложений Azure

Несмотря на [интеграцию приложения с виртуальной сетью Azure](../app-service/web-sites-integrate-with-vnet.md), бывает так, что установить подключение к Управляемому экземпляру не удается. В этом случае вам нужно обновить конфигурацию сети для своего плана обслуживания.

## <a name="sync-network-configuration-for-app-service-hosting-plan"></a>Синхронизация конфигурации сети для плана размещения Службы приложений

Для этого выполните следующие действия:  

1. Перейдите к плану службы веб-приложений.

   ![План службы приложений](./media/sql-database-managed-instance-sync-networking/app-service-plan.png)

2. Нажмите кнопку **Сети** и выберите **Щелкните здесь для управления**.

   ![Управление планом службы](./media/sql-database-managed-instance-sync-networking/manage-plan.png)

3. Выберите **виртуальную сеть** и щелкните **Синхронизировать сеть**.

   ![Синхронизация сети](./media/sql-database-managed-instance-sync-networking/sync.png)

4. Дождитесь завершения синхронизации.
  
   ![Синхронизация выполнена](./media/sql-database-managed-instance-sync-networking/sync-done.png)

Теперь можно попробовать заново установить подключение к Управляемому экземпляру.

## <a name="next-steps"></a>Дальнейшие действия

- Дополнительные сведения о настройке виртуальной сети Управляемого экземпляра см. в статье [Архитектура подключения к Управляемому экземпляру Базы данных SQL Azure](sql-database-managed-instance-connectivity-architecture.md) и [Configure an existing VNet for Azure SQL Database Managed Instance](sql-database-managed-instance-configure-vnet-subnet.md) (Настройка имеющейся виртуальной сети для Управляемого экземпляра Базы данных SQL Azure).
