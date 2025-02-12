---
title: Подключайте данные из Azure AD для предварительной версии Sentinel Azure | Документация Майкрософт
description: Узнайте, как подключиться к Azure Sentinel данные Azure Active Directory.
services: sentinel
documentationcenter: na
author: rkarlin
manager: rkarlin
editor: ''
ms.assetid: 0a8f4a58-e96a-4883-adf3-6b8b49208e6a
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/17/2019
ms.author: rkarlin
ms.openlocfilehash: 5784101c9f2e0dc238ac48c5d0f6fbe4c0dc596f
ms.sourcegitcommit: 6a42dd4b746f3e6de69f7ad0107cc7ad654e39ae
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/07/2019
ms.locfileid: "67620675"
---
# <a name="connect-data-from-azure-active-directory"></a>Подключайте данные из Azure Active Directory

> [!IMPORTANT]
> Сейчас Azure Sentinel предоставляется в общедоступной предварительной версии.
> Эта предварительная версия предоставляется без соглашения об уровне обслуживания и не рекомендована для использования рабочей среде. Некоторые функции могут не поддерживаться или их возможности могут быть ограничены. Дополнительные сведения см. в статье [Дополнительные условия использования предварительных выпусков Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Azure Sentinel позволяет собирать данные из [Azure Active Directory](../active-directory/fundamentals/active-directory-whatis.md) и передать их в Azure Sentinel. Вы можете выбрать поток [журналы входа](../active-directory/reports-monitoring/concept-sign-ins.md) и [журналы аудита](../active-directory/reports-monitoring/concept-audit-logs.md) .

## <a name="prerequisites"></a>Предварительные требования

- Если вы хотите экспортировать данные входа из Active Directory, необходимо иметь лицензию Azure AD P1 или P2.

- Пользователь с глобального администратора или безопасности разрешений администратора для клиента, необходимо выполнять потоковую передачу журналов из.

- Чтобы иметь возможность увидеть состояние подключения, необходимо иметь разрешение на доступ к журналам диагностики Azure AD. 


## <a name="connect-to-azure-ad"></a>Подключение к Azure AD

1. В Azure Sentinel, выберите **соединители данных** и нажмите кнопку **Azure Active Directory** плитку.

2. Рядом с журналы для потоковой передачи в Azure Sentinel, щелкните **Connect**.

6. Чтобы использовать соответствующей схемы в Log Analytics для оповещений Azure AD, поиск **SigninLogs** и **AuditLogs**.




## <a name="next-steps"></a>Следующие шаги
В этом документе вы узнали, как соединиться с Azure AD Azure Sentinel. Ознакомьтесь с дополнительными сведениями об Azure Sentinel в соответствующих статьях.
- Узнайте, как [получить представление о данных и потенциальные угрозы](quickstart-get-visibility.md).
- Начало работы [обнаружение угроз с помощью Azure Sentinel](tutorial-detect-threats.md).
