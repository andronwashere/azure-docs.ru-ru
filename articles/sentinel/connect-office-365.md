---
title: Подключайте данные из Office 365 для предварительной версии Sentinel Azure | Документация Майкрософт
description: Узнайте, как подключиться к Azure Sentinel данных Office 365.
services: sentinel
documentationcenter: na
author: rkarlin
manager: rkarlin
editor: ''
ms.assetid: ff7c862e-2e23-4a28-bd18-f2924a30899d
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/07/2019
ms.author: rkarlin
ms.openlocfilehash: 1d6a467307e4816ffbb45f23bac55b8023267352
ms.sourcegitcommit: 6a42dd4b746f3e6de69f7ad0107cc7ad654e39ae
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/07/2019
ms.locfileid: "67611238"
---
# <a name="connect-data-from-office-365-logs"></a>Подключайте данные из Office 365 журналов

> [!IMPORTANT]
> Сейчас Azure Sentinel предоставляется в общедоступной предварительной версии.
> Эта предварительная версия предоставляется без соглашения об уровне обслуживания и не рекомендована для использования рабочей среде. Некоторые функции могут не поддерживаться или их возможности могут быть ограничены. Дополнительные сведения см. в статье [Дополнительные условия использования предварительных выпусков Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Можно осуществлять потоковую передачу журналов аудита из [Office 365](https://docs.microsoft.com/office365/admin/admin-home?view=o365-worldwide) в Azure Sentinel одним щелчком. Можно осуществлять потоковую передачу журналов аудита из нескольких клиентов в одной рабочей области в Azure Sentinel. Соединитель журнала действий Office 365 позволяет отслеживать действия текущих пользователей. Вы получите сведения о различных пользователей, admin, системы и действия политики и событий из Office 365. Подключив журналы Office 365 в Azure Sentinel эти данные можно использовать для просмотра панелей мониторинга, создать настраиваемые оповещения и улучшить процесс исследования.


## <a name="prerequisites"></a>предварительные требования

- На клиенте должен быть глобальный администратор или администратор безопасности
- На компьютере, с которого вы выполнили вход в Azure Sentinel, чтобы создать подключение, убедитесь, что открыт порт 4433 для веб-трафика.

## <a name="connect-to-office-365"></a>Подключение к Office 365

1. В Azure Sentinel, выберите **соединители данных** и нажмите кнопку **Office 365** плитку.

2. Если вы еще не включили, в разделе **подключения** использовать **включить** кнопку, чтобы включить решение Office 365. Если оно уже включено, оно будет идентифицировано в окне подключения как уже включена.
1. Office 365 позволяет потоковая передача данных из нескольких клиентов в Azure Sentinel. Для каждого клиента, который вы хотите подключиться, добавьте клиент в разделе **подключают клиентов к Azure Sentinel**. 
1. Откроется экран сообщения об Active Directory. Предлагается для проверки подлинности пользователя глобального администратора в каждом клиенте, который вы хотите подключиться к Azure Sentinel и предоставление разрешений для Azure Sentinel для чтения копия журналов. 
5. В разделе журналов действий Stream Office 365, щелкните **выберите** для выбора типов журнала для потоковой передачи для Azure Sentinel. В настоящее время Azure Sentinel поддерживает Exchange и SharePoint.

4. Нажмите кнопку **применить изменения**.

3. Для соответствующей схемы в Log Analytics можно использовать для журналов Office 365, поиск **OfficeActivity**.


## <a name="next-steps"></a>Следующие шаги
В этом документе вы узнали, как подключиться к Azure Sentinel Office 365. Ознакомьтесь с дополнительными сведениями об Azure Sentinel в соответствующих статьях.
- Узнайте, как [получить представление о данных и потенциальные угрозы](quickstart-get-visibility.md).
- Начало работы [обнаружение угроз с помощью Azure Sentinel](tutorial-detect-threats.md).

