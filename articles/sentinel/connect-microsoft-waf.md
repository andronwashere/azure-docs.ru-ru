---
title: Подключение данных брандмауэра веб-приложения Microsoft предварительную версию Sentinel Azure | Документация Майкрософт
description: Узнайте, как соединиться с данных брандмауэра веб-приложения Microsoft Azure Sentinel.
services: sentinel
documentationcenter: na
author: rkarlin
manager: rkarlin
editor: ''
ms.assetid: bfa2eca4-abdc-49ce-b11a-0ee229770cdd
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/07/2019
ms.author: rkarlin
ms.openlocfilehash: 0a308394c3cfa77f80db1361b5a49a485ee5ca0e
ms.sourcegitcommit: 6a42dd4b746f3e6de69f7ad0107cc7ad654e39ae
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/07/2019
ms.locfileid: "67611347"
---
# <a name="connect-data-from-microsoft-web-application-firewall"></a>Подключайте данные из Microsoft брандмауэр веб-приложения

> [!IMPORTANT]
> Сейчас Azure Sentinel предоставляется в общедоступной предварительной версии.
> Эта предварительная версия предоставляется без соглашения об уровне обслуживания и не рекомендована для использования рабочей среде. Некоторые функции могут не поддерживаться или их возможности могут быть ограничены. Дополнительные сведения см. в статье [Дополнительные условия использования предварительных выпусков Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Можно осуществлять потоковую передачу журналов из брандмауэра шлюза приложений Azure Microsoft веб-приложения (WAF). Этот WAF защиту приложений от распространенных сетевых уязвимостей, например путем внедрения кода SQL и межсайтовых сценариев и позволяет настраивать правила, чтобы сократить число ложных срабатываний. Выполните эти инструкции для потоковой передачи журналов брандмауэра приложений Microsoft Web в Azure Sentinel.


## <a name="prerequisites"></a>предварительные требования

- Существующий ресурс шлюза приложений

## <a name="connect-to-microsoft-web-application-firewall"></a>Подключение к Microsoft брандмауэр веб-приложения

Если вас уже есть брандмауэр веб-приложения Microsoft, убедитесь, что у вас есть существующий ресурс шлюза.
После развертывания брандмауэра веб-приложения Майкрософт и получение данных, данные оповещения можно легко настроить потоковую передачу в Azure Sentinel.
    
1. На портале Azure Sentinel выберите **соединители данных**.
1. На странице соединителей данных, выберите **WAF** плитку.
1. Перейдите к [ресурсов шлюза приложений](https://ms.portal.azure.com/#blade/HubsExtension/BrowseAllResourcesBlade/resourceType/Microsoft.Network%2FapplicationGateways) и выберите WAF.
    1. Выберите **параметров диагностики**.
    1. Выберите **+ добавить параметр диагностики** под таблицей.
    1. В **параметров диагностики** введите **имя** и выберите **отправить в Log Analytics**.
    1. В разделе **рабочей области Log Analytics** выберите рабочую область Azure Sentinel.
    1. Выберите типы журналов, которые необходимо проанализировать. Мы рекомендуем: ApplicationGatewayAccessLog и ApplicationGatewayFirewallLog.
1. Для использования соответствующей схемы в Log Analytics для оповещений Microsoft web application брандмауэра, поиск **AzureDiagnostics**.

## <a name="next-steps"></a>Следующие шаги
В этом документе вы узнали, как соединиться с брандмауэром веб-приложения Microsoft Azure Sentinel. Ознакомьтесь с дополнительными сведениями об Azure Sentinel в соответствующих статьях.
- Узнайте, как [получить представление о данных и потенциальные угрозы](quickstart-get-visibility.md).
- Начало работы [обнаружение угроз с помощью Azure Sentinel](tutorial-detect-threats.md).
