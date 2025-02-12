---
title: Увеличение масштаба инфраструктуры Azure DevTest Labs
description: Эта статья содержит рекомендации по увеличению масштаба инфраструктуры Azure DevTest Labs.
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/11/2019
ms.author: spelluru
ms.reviewer: christianreddington,anthdela,juselph
ms.openlocfilehash: 25a088686c739c53feadd6354baf75f3147bdc33
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "60561495"
---
# <a name="scale-up-your-azure-devtest-labs-infrastructure"></a>Увеличение масштаба инфраструктуры Azure DevTest Labs
Прежде чем реализовать DevTest Labs на корпоративном уровне, нужно решить некоторые важные вопросы. Общее понимание этих вопросов поможет организации принимать решения относительно проектирования в будущем. Но эти вопросы не должны мешать организации приступить к подтверждению концепции. Ниже приведены три основных аспекта, с которых нужно начать планирование увеличения масштаба:

- сетевые подключения и безопасность;
- топология использования подписки;
- роли и обязанности.

## <a name="networking-and-security"></a>Сетевые подключения и безопасность
Сетевые подключения и безопасность — это ключевые вопросы для всех организаций. Если развертывание на корпоративном уровне требует более глубокого анализа, то для успешного подтверждения концепции существует меньше требований. Несколько ключевых моментов, которым следует уделить внимание:

- **Подписка Azure**. Чтобы развернуть DevTest Labs, вам потребуется доступ к подписке Azure с соответствующими правами для создания ресурсов. Существует несколько способов для получения доступа к подпискам Azure, включая подписки с соглашением Enterprise и оплатой по мере использования. Дополнительные сведения о получении доступа к подписке Azure см. на странице [Лицензирование Azure для корпоративных пользователей](https://azure.microsoft.com/pricing/enterprise-agreement/).
- **Доступ к локальным ресурсам**. В некоторых организациях требуется, чтобы ресурсы в DevTest Labs имели доступ к локальным ресурсам. Для этого нужно обеспечить безопасное подключение из локальной среды к Azure. Таким образом, перед началом работы важно установить и настроить VPN-подключение или подключение ExpressRoute. Дополнительные сведения см. в статье [о виртуальных сетях](../virtual-network/virtual-networks-overview.md).
- **Дополнительные требования к безопасности**. Прежде чем реализовать подтверждение концепции, возможно, также потребуется рассмотреть и другие требования к безопасности, например политики для компьютеров, доступ к общедоступным IP-адресам, подключение к Интернету и т. п. 

## <a name="subscription-topology"></a>Топология использования подписки
Топология использования подписки — важный аспект при планировании развертывания DevTest Labs в организации. Тем не менее, не нужно спешить утверждать все решения до завершения подтверждения концепции. При оценке количества подписок, необходимых для внедрения в организации, можно прийти к двум противоположным решениям: 

- использовать одну подписку для всей организации;
- использовать одну подписку на пользователя.

Ниже мы рассмотрим преимущества каждого подхода.

### <a name="one-subscription"></a>Одна подписка
Часто подход с использованием одной подписки не является удобным для больших организаций. Но ограничение количества подписок обеспечивает следующие преимущества.

- **Прогнозирование** затрат для предприятия.  При использовании одной подписки планировать расходы становится гораздо проще, так как все ресурсы находятся в одном пуле. Такой подход упрощает принятие решений по проведению контроля расходов в любой момент времени в цикле выставления счетов.
- **Управляемость** виртуальных машин, артефакты, формулы, конфигурации сети, разрешения, политики, и т.д. проще, так как все обновления нужно выполнить только в одной подписке, в отличие от внесение обновлений в нескольких подписках.
- При таком подходе значительно сокращаются трудозатраты на обслуживание **сети** для организаций, где требуется возможность подключения к локальной среде. Чтобы подключать виртуальные сети между разными подписками (звездообразная модель), требуются дополнительные подписки, а для них — дополнительные настройки, управление, диапазон IP-адресов и т. д.
- **Совместная работа в группе** становится проще, если все работают в одной подписке. Например, это упрощает переназначение виртуальной машины коллегам, совместное использование ресурсов группы и т. д.

### <a name="subscription-per-user"></a>Одна подписка на пользователя
Подход с отдельной подпиской для каждого пользователя обеспечивает свой набор преимуществ. Использование нескольких подписок обеспечивает такие преимущества:

- **Квоты масштабирования Azure** не будут препятствовать внедрению. К примеру, на момент написания этой статьи в Azure можно создать 200 учетных записей хранения на подписку. Для большинства служб Azure применяются квоты на число операций (многие из них можно настроить, но не все). В модели использования одной подписки на пользователя маловероятно, что большинство квот будет достигнуто. Дополнительные сведения о текущих квотах масштабирования в Azure см. в статье [Подписка Azure, границы, квоты и ограничения службы](../azure-subscription-service-limits.md).
- **Взимание средств за использование** для групп или отдельных разработчиков становится гораздо проще, что позволяет организациям учитывать затраты при использовании текущей модели.
- Упрощается **владение и назначение разрешений** в средах DevTest Labs. Вы предоставляете разработчикам доступ на уровне подписки, и они полностью отвечают за все, включая конфигурацию сети, политики лаборатории и управление виртуальными машинами.

Для компаний существует больше ограничений по максимальному и минимальному числу подписок. Поэтому необходимо настроить подписки таким образом, который будет представлять собой что-то среднее между этими подходами. Организации рекомендуется использовать минимальное количество подписок и не забывать о принудительных функциях, позволяющих повысить общее число подписок. Еще раз отметим, что топология использования подписок очень важна для развертывания DevTest Labs в организации, но не должна препятствовать подтверждению концепции. Сведения о выборе подписок и степени структурированности лабораторий в организации см. в статье об [управлении инфраструктурой](devtest-lab-guidance-governance-policy-compliance.md).

## <a name="roles-and-responsibilities"></a>Роли и обязанности
Подтверждение концепции DevTest Labs предусматривает три первичные роли с определенными обязанностями — владелец подписки, владелец DevTest Labs, пользователь DevTest Labs и (при необходимости) участник.

- **Владелец подписки** может управлять подпиской Azure, в том числе назначать пользователей, управлять политиками, создавать и администрировать топологии сети, запрашивать увеличения квоты и т. д. Дополнительные сведения см. в [этой статье](../role-based-access-control/rbac-and-directory-admin-roles.md).
- **Владелец DevTest Labs** имеет полный административный доступ к лаборатории. Этот пользователь отвечает за добавление и удаление пользователей, управление параметрами затрат, общие параметры лаборатории и другие задачи, связанные с артефактами или виртуальными машинами. Владелец лаборатории также имеет все права пользователя DevTest Labs.
- **Пользователь DevTest Labs** может создавать и использовать виртуальные машины в лаборатории. Эти пользователи имеют минимальные права администрирования (запуск, приостановление, удаление и настройка виртуальных машин) для виртуальных машин, которые они создают. Они не могут управлять виртуальными машинами других пользователей.

## <a name="next-steps"></a>Дальнейшие действия
См. следующую статью в этом цикле: [Оркестрация внедрения службы Azure DevTest Labs](devtest-lab-guidance-orchestrate-implementation.md)