---
title: Управление резервными копиями с помощью управления доступом на основе ролей
description: Использование управления доступом на основе ролей для управления доступом к операциям управления резервными копиями в хранилище служб восстановления.
author: utraghuv
manager: vijayts
ms.service: backup
ms.topic: conceptual
ms.date: 06/24/2019
ms.author: utraghuv
ms.openlocfilehash: 928c08862fdb8a447b6b7afdd7fc12317a201224
ms.sourcegitcommit: c72ddb56b5657b2adeb3c4608c3d4c56e3421f2c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/24/2019
ms.locfileid: "68464959"
---
# <a name="use-role-based-access-control-to-manage-azure-backup-recovery-points"></a>Использование управления доступом на основе ролей для управления точками восстановления службы архивации Azure
Контроль доступа на основе ролей (RBAC) Azure обеспечивает точное управление доступом для Azure. С помощью RBAC вы можете распределить обязанности внутри команды и предоставить пользователям доступ на уровне, который им необходим для выполнения поставленных задач.

> [!IMPORTANT]
> Роли, предоставляемые Azure Backup, ограничены действиями, которые могут быть выполнены в портал Azure или с помощью REST API или командлетов PowerShell или хранилища служб восстановления. Эти роли не управляют действиями, выполняемыми в пользовательском интерфейсе клиента агента службы Azure Backup, System Center Data Protection Manager или Azure Backup Server.

Azure Backup предоставляет три встроенных роли для управления операциями управления резервным копированием. Дополнительные сведения см. в статье о [встроенных ролях RBAC в Azure](../role-based-access-control/built-in-roles.md).

* [Участник резервного копирования](../role-based-access-control/built-in-roles.md#backup-contributor) . Эта роль имеет все разрешения на создание резервной копии и управление ею, за исключением удаления хранилища служб восстановления и предоставления доступа другим пользователям. Эта роль представляет собой администратора управления архивацией, который может выполнять любую операцию управления архивацией.
* [Оператор архивации.](../role-based-access-control/built-in-roles.md#backup-operator) Эта роль имеет те же разрешения, что и участник, но не может удалять резервные копии и управлять политикой архивации. Эта роль эквивалентна роли участника, за исключением того, что пользователь с этой ролью не может выполнять операции удаления, такие как прекращение архивации с удалением данных или удаление регистрации локальных ресурсов.
* [Читатель резервных копий](../role-based-access-control/built-in-roles.md#backup-reader). Эта роль имеет разрешения на просмотр всех операций управления архивацией. Используйте эту роль, чтобы осуществлять мониторинг.

Если вы хотите определить собственные роли для дополнительного управления, см. статью [Пользовательские роли в Azure RBAC](../role-based-access-control/custom-roles.md).



## <a name="mapping-backup-built-in-roles-to-backup-management-actions"></a>Сопоставление встроенных ролей архивации с операциями управления архивацией
В следующей таблице указаны операции управления архивацией и соответствующая минимальная роль RBAC, необходимая для их выполнения.

| Операция управления | Минимальная требуемая роль RBAC | Требуемая область |
| --- | --- | --- |
| Создание хранилища Служб восстановления | Участник архивации | Группа ресурсов, содержащая хранилище |
| Включение архивации виртуальных машин Azure | Оператор архивации | Группа ресурсов, содержащая хранилище |
| | Участник виртуальных машин | Ресурс виртуальной машины |
| Архивация виртуальной машины по запросу | Оператор архивации | Ресурс хранилища восстановления |
| Восстановление виртуальной машины | Оператор архивации | Хранилище служб восстановления |
| | Участник | Группа ресурсов, в которой будет развернута виртуальная машина |
| | Участник виртуальных машин | Исходная виртуальная машина, для которой выполнено резервное копирование |
| Восстановление резервной копии виртуальной машины с неуправляемыми дисками | Оператор архивации | Ресурс хранилища восстановления |
| | Участник виртуальных машин | Исходная виртуальная машина, для которой выполнено резервное копирование |
| | Участник учетных записей хранения | Ресурс учетной записи хранения, где будет восстанавливаться диски |
| Восстановление управляемых дисков из резервной копии виртуальной машины | Оператор архивации | Ресурс хранилища восстановления |
| | Участник виртуальных машин | Исходная виртуальная машина, для которой выполнено резервное копирование |
| | Участник учетных записей хранения | Временная учетная запись хранения, выбранная в процессе восстановления, для хранения данных из хранилища перед их преобразованием в управляемые диски |
| | Участник | Группа ресурсов, в которую будут восстановлены управляемые диски |
| Восстановление отдельных файлов из резервной копии виртуальной машины | Оператор архивации | Ресурс хранилища восстановления |
| | Участник виртуальных машин | Исходная виртуальная машина, для которой выполнено резервное копирование |
| Создание политики архивации виртуальных машин Azure | Участник архивации | Ресурс хранилища восстановления |
| Изменение политики архивации виртуальных машин Azure | Участник архивации | Ресурс хранилища восстановления |
| Удаление политики архивации виртуальных машин Azure | Участник архивации | Ресурс хранилища восстановления |
| Прекращение архивации (с сохранением или удалением данных) виртуальной машины | Участник архивации | Ресурс хранилища восстановления |
| Регистрация локального сервера Windows Server, клиента, SCDPM или Azure Backup Server | Оператор архивации | Ресурс хранилища восстановления |
| Удаление зарегистрированного локального сервера Windows Server, клиента, SCDPM или Azure Backup Server | Участник архивации | Ресурс хранилища восстановления |

> [!IMPORTANT]
> Если вы зададите роль участника виртуальной машины в пределах ресурса и выберете резервное копирование в ее параметрах, откроется экран "Включить резервное копирование", несмотря на то что оно уже выполнено. Причина в том, что вызов для проверки состояния резервного копирования работает только на уровне подписки. Чтобы избежать этой проблемы, перейдите к хранилищу и откройте представление элемента резервного копирования для виртуальной машины или назначьте роль участника виртуальной машины на уровне подписки.

## <a name="minimum-role-requirements-for-the-azure-file-share-backup"></a>Минимальные требования к ролям для резервного копирования файлового ресурса Azure
В следующей таблице записаны действия по управлению архивацией и соответствующая роль, необходимая для выполнения операции с общей папкой Azure.

| Операция управления | Требуется роль | Ресурсы |
| --- | --- | --- |
| Включение резервного копирования файловых ресурсов Azure | Участник архивации | Хранилище служб восстановления |
| | Учетная запись хранения | Ресурс учетной записи хранения участника |
| Архивация виртуальной машины по запросу | Оператор архивации | Хранилище служб восстановления |
| Восстановить общую папку | Оператор архивации | Хранилище служб восстановления |
| | Участник учетных записей хранения | Ресурсы учетной записи хранения, в которой имеются исходные и целевые общие папки восстановления |
| Восстановление отдельных файлов | Оператор архивации | Хранилище служб восстановления |
| | Участник учетных записей хранения |   Ресурсы учетной записи хранения, в которой имеются исходные и целевые общие папки восстановления |
| Остановить защиту | Участник архивации | Хранилище служб восстановления |      
| Отмена регистрации учетной записи хранения в хранилище |   Участник архивации | Хранилище служб восстановления |
| | Участник учетных записей хранения | Ресурс учетной записи хранения|


## <a name="next-steps"></a>Следующие шаги
* [Управление доступом на основе ролей](../role-based-access-control/role-assignments-portal.md): Начало работы с RBAC на портале Azure.
* Сведения об управлении доступом с помощью следующих средств:
  * [PowerShell](../role-based-access-control/role-assignments-powershell.md)
  * [Интерфейс командной строки Azure](../role-based-access-control/role-assignments-cli.md)
  * [REST API](../role-based-access-control/role-assignments-rest.md)
* [Устранение неполадок при управлении доступом на основе ролей](../role-based-access-control/troubleshooting.md). Рекомендации по устранению распространенных проблем.
