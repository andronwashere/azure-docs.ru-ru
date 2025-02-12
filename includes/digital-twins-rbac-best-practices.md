---
title: включение файла
description: включение файла
services: digital-twins
author: kingdomofends
ms.service: digital-twins
ms.topic: include
ms.date: 12/28/2018
ms.author: adgera
ms.custom: include file
ms.openlocfilehash: e81b8fb06240d566e46ca0b45a0910e014dee329
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/18/2019
ms.locfileid: "67185921"
---
Управление доступом на основе ролей — это стратегия обеспечения безопасности на основе наследования для управления доступом, разрешениями и ролями. Дочерние роли наследуют разрешения от родительских. Разрешения также можно назначить без наследования от родительской роли и для настройки роли по мере необходимости.

Например, администратору области, возможно, понадобится глобальный доступ, чтобы выполнять все операции для указанной области. При этом требуется доступ ко всем узлам, которые находятся на нижнем уровне или в пределах области. Установщику устройств могут потребоваться только разрешения на *чтение* и *обновление* для устройств и датчиков.

В каждом случае роли следует предоставлять *строго для требуемого уровня доступа* для выполнения задач в соответствии с принципом минимальных привилегий. В соответствии с этим принципом удостоверение предоставляется *только при соблюдении следующих условий*:

* Уровень доступа, необходимый для выполнения работы.
* Роль, соответствующая и ограничиваемая выполнением требуемого задания.

>[!IMPORTANT]
> Всегда следуйте принципу минимальных привилегий.

Есть еще две важные стратегии управления доступом на основе ролей:

> [!div class="checklist"]
> * Периодически выполняйте аудит назначения ролей, чтобы убедиться, что каждой роли назначены правильные привилегии.
> * Очищайте роли и назначения при их смене пользователем.