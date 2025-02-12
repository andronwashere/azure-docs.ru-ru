---
title: Обновление версии ОС в центре безопасности Azure | Документация Майкрософт
description: В этой статье объясняется, как выполнить рекомендацию центра безопасности Azure **Обновление версии ОС**.
services: security-center
documentationcenter: na
author: rkarlin
manager: barbkess
editor: ''
ms.assetid: aa372492-ecdb-4368-8fdd-d8ed31e216ee
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/28/2018
ms.author: rkarlin
ms.openlocfilehash: a17bc3cf8c8f1ffbb2a91e17944bfc1c203f439d
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "60905856"
---
# <a name="update-os-version-in-azure-security-center"></a>Обновление версии ОС в центре безопасности Azure
Для виртуальных машин (ВМ) в облачных службах центр безопасности Azure порекомендует обновить версию операционной системы (ОС) на более новую, если она есть.  Мониторинг выполняется только для облачных служб и рабочих ролей, запущенных в слотах рабочей среды.

> [!NOTE]
> В документе приводится обзор службы с помощью примера развертывания.  Он не является пошаговым руководством.
> 
> 

## <a name="implement-the-recommendation"></a>Выполнение рекомендаций
1. В колонке **Рекомендации** выберите **Обновление версии ОС**.
   ![Обновление версии ОС][1]
2. После этого откроется колонка **Update OS version**(Обновление версии ОС). Выполните указанные в этой колонке действия, чтобы обновить версию ОС.

## <a name="see-also"></a>См. также
В этой статье объясняется, как выполнить рекомендацию центра безопасности в отношении обновления версии ОС. Дополнительные сведения об облачных службах и обновлении версии ОС для облачной службы см. в следующих статьях:

* [Обзор облачных служб](../cloud-services/cloud-services-choose-me.md)
* [Обновление облачной службы](../cloud-services/cloud-services-update-azure-service.md)
* [Настройка облачных служб](../cloud-services/cloud-services-how-to-configure-portal.md)

Дополнительные сведения о Центре безопасности см. в следующих статьях:

* [Настройка политик безопасности в Центре безопасности Azure](tutorial-security-policy.md) — узнайте, как настроить политики безопасности для подписок и групп ресурсов Azure.
* [Управление рекомендациями по безопасности в Центре безопасности Azure](security-center-recommendations.md) — узнайте, как рекомендации могут помочь вам защитить ресурсы Azure.
* [Наблюдение за работоспособностью системы безопасности в Центре безопасности Azure](security-center-monitoring.md) — узнайте, как наблюдать за работоспособностью ресурсов Azure.
* [Управление оповещениями безопасности в Центре безопасности Azure и реагирование на них](security-center-managing-and-responding-alerts.md) — узнайте, как управлять оповещениями системы безопасности и реагировать на них.
* [Мониторинг решений партнеров с помощью центра безопасности Azure](security-center-partner-solutions.md) — узнайте, как отслеживать состояние работоспособности решений партнеров.
* [Центр безопасности Azure: часто задаваемые вопросы](security-center-faq.md) — часто задаваемые вопросы об использовании этой службы.
* [Блог по безопасности Azure](https://blogs.msdn.com/b/azuresecurity/) — последние новости и информация об обеспечении безопасности в Azure.

<!--Image references-->
[1]: ./media/security-center-update-os-version/update-os-version.png
