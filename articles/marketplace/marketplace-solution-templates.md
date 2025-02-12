---
title: Шаблон решения приложения Azure предлагают руководство по публикации | Azure Marketplace
description: В этой статье описываются требования к публикации шаблона приложения в Azure Marketplace.
services: Azure, Marketplace, Compute, Storage, Networking, Blockchain, Security
author: ellacroi
manager: nunoc
ms.service: marketplace
ms.topic: article
ms.date: 11/15/2018
ms.author: ellacroi
ms.openlocfilehash: c2393b6ea9f1a2c2b35be63272743e081f4ae240
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "64937747"
---
# <a name="azure-applications-solution-template-offer-publishing-guide"></a>Приложения Azure. Руководство по публикации предложения "Шаблон решения"

Шаблоны решений — это один из основных способов публикации решения в Azure Marketplace. Сведения в этом руководстве помогут понять требования к этому предложению. 

Тип предложения "Приложение Azure: шаблон решения" используется, если в решении требуются дополнительные возможности автоматизации развертывания и настройки, недоступные в обычной виртуальной машине. Вы можете автоматизировать подготовку одной или нескольких виртуальных машин с помощью шаблонов решения для приложений Azure. Вы также можете подготавливать ресурсы сети и хранилища. Тип предложения "Приложения Azure: шаблоны решений" предоставляет преимущества автоматизации для отдельных виртуальных машин и целых решений на основе IaaS.

Эти шаблоны решений являются предложениями транзакций с развертыванием и выставлением счетов через Marketplace. Пользователь видит призыв к действию — "Получить сейчас".


## <a name="requirements-for-solution-templates"></a>Требования к шаблонам решений

| **Требования** | **Сведения**  |
| ---------------  | -----------  |
|Выставление счетов и ценообразование    |  Ресурсы будут подготовлены в подписке Azure клиента. Транзакции, связанные с виртуальными машинами с оплатой по мере использования (PAYGO), будут проводиться через корпорацию Майкрософт, а выставление счетов будет осуществляться по подписке Azure клиента (PAYGO).  <br/> Хотя в случае использования собственной лицензии (BYOL) корпорация Майкрософт будет выставлять счета на расходы на инфраструктуру по подписке клиента, вы будете напрямую выставлять клиенту счета на лицензирование ПО.   |
|Совместимый с Azure виртуальный жесткий диск (VHD)  |   Виртуальные машины должны быть созданы на платформе Windows или Linux.  См. дополнительные сведения о [создании совместимых с Azure виртуальных жестких дисков](./cloud-partner-portal/virtual-machine/cpp-create-vhd.md). |
| Определение потребления услуг клиентами | Определение потребления услуг клиентами требуется включить для всех шаблонов решений, публикуемых в Azure Marketplace. См. дополнительные сведения об [определении потребления услуг клиентами партнеров Azure](./azure-partner-customer-usage-attribution.md).  |
|  |  |

## <a name="next-steps"></a>Дальнейшие действия
Если вы еще не сделали это, [зарегистрируйтесь](https://azuremarketplace.microsoft.com/sell) в Marketplace.

Если вы зарегистрированы и создаете предложение или работаете над существующим, войдите на [Портал Cloud Partner](https://cloudpartner.azure.com), чтобы завершить процесс.
