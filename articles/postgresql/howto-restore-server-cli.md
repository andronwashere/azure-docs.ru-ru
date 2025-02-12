---
title: Резервное копирование и восстановление сервера в базе данных Azure для PostgreSQL — один сервер
description: Узнайте, как резервное копирование и восстановление сервера в базе данных Azure для PostgreSQL — один сервер с помощью Azure CLI.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.devlang: azurecli
ms.topic: conceptual
ms.date: 05/06/2019
ms.openlocfilehash: 85fb00ad221ae982e4d3ddc9d2d5d20dd4f2793d
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "65069096"
---
# <a name="how-to-back-up-and-restore-a-server-in-azure-database-for-postgresql---single-server-using-the-azure-cli"></a>Резервное копирование и восстановление сервера в базе данных Azure для PostgreSQL — отдельного сервера с помощью Azure CLI

## <a name="backup-happens-automatically"></a>Резервное копирование выполняется автоматически
Чтобы обеспечить возможность восстановления, для серверов службы "База данных Azure для PostgreSQL" периодически выполняется резервное копирование. С помощью этой функции можно восстановить сервер и все его базы данных до более ранней точки во времени на новом сервере.

## <a name="prerequisites"></a>Технические условия
Вот что вам нужно, чтобы выполнить инструкции, приведенные в этом руководстве:
- [сервер и база данных Azure для PostgreSQL](quickstart-create-server-database-azure-cli.md);

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

 

> [!IMPORTANT]
> Для работы с этим руководством вам понадобится Azure CLI 2.0 или более поздней версии. Чтобы проверить версию, в командной строке Azure CLI введите `az --version`. Чтобы выполнить установку или обновление, см. сведения в статье [Установка Azure CLI]( /cli/azure/install-azure-cli).

## <a name="set-backup-configuration"></a>Настройка конфигурации резервного копирования

При создании сервер можно настроить для локально избыточного или геоизбыточного резервного копирования. 

> [!NOTE]
> После создания сервера тип избыточности (локальная или географическая) изменить нельзя.
>

При создании сервера с помощью команды `az postgres server create` параметр `--geo-redundant-backup` определяет тип избыточности для резервного копирования. Если установлено значение `Enabled`, выполняется геоизбыточное резервное копирование. Если установлено значение `Disabled`, выполняется локальное избыточное резервное копирование. 

Период хранения резервных копий устанавливается с помощью параметра `--backup-retention-days`. 

Дополнительные сведения о настройке этих значений при создании см. в статье [Создание сервера базы данных Azure для MySQL с помощью Azure CLI](quickstart-create-server-database-azure-cli.md).

Вот как можно изменить срок хранения резервной копии сервера.

```azurecli-interactive
az postgres server update --name mydemoserver --resource-group myresourcegroup --backup-retention 10
```

В указанном выше примере период хранения резервных копий mydemoserver изменен на 10 дней.

Срок хранения резервных копий определяет, насколько раннюю точку во времени можно задать для восстановления, так как это зависит от доступных резервных копий. Восстановление до точки во времени описано в следующем разделе.

## <a name="server-point-in-time-restore"></a>Восстановление сервера до точки во времени
Вы можете восстановить сервер до предыдущей точки во времени. Восстановленные данные копируются на новый сервер, а имеющийся сервер остается неизменным. Например, если таблица была случайно удалена в полдень, ее можно восстановить до момента сразу перед полуднем. Затем вы можете извлечь отсутствующие таблицы и данные из восстановленной копии сервера. 

Чтобы выполнить восстановление сервера, используйте команду Azure CLI [az postgres server restore](/cli/azure/postgres/server).

### <a name="run-the-restore-command"></a>Выполнение команды restore

Для восстановления сервера в командной строке Azure CLI введите следующую команду:

```azurecli-interactive
az postgres server restore --resource-group myresourcegroup --name mydemoserver-restored --restore-point-in-time 2018-03-13T13:59:00Z --source-server mydemoserver
```

Для команды `az postgres server restore` обязательны указанные ниже параметры.

| Параметр | Предлагаемое значение | Описание  |
| --- | --- | --- |
| resource-group |  myresourcegroup |  Группа ресурсов, в которой находится исходный сервер.  |
| name | mydemoserver-restored | Имя нового сервера, созданного командой restore. |
| restore-point-in-time | 2018-03-13T13:59:00Z | Выберите точку во времени, до которой необходимо выполнить восстановление. Значения даты и времени должны находиться в пределах срока хранения резервной копии исходного сервера. Используйте формат даты и времени ISO8601. Можно использовать местный часовой пояс, например `2018-03-13T05:59:00-08:00`. Также можно использовать формат UTC Zulu, например `2018-03-13T13:59:00Z`. |
| source-server | mydemoserver | Имя или идентификатор исходного сервера, с которого необходимо выполнить восстановление. |

При восстановлении сервера до более ранней точки во времени будет создан новый сервер. Исходный сервер и его базы данных, начиная с указанного момента во времени, копируются на новый сервер.

Значения расположения и ценовой категории для восстановленного сервера совпадают со значениями исходного сервера. 

После завершения восстановления найдите новый сервер, чтобы убедиться, что данные были восстановлены, как и ожидалось.

На новом сервере, созданном во время восстановления, нет правил брандмауэра, которые были настроены на сервере-источнике. Для этого нового сервера правила брандмауэра нужно настроить отдельно.

## <a name="geo-restore"></a>Геовосстановление
Если вы настроили сервер для геоизбыточного резервного копирования, из резервной копии можно создать новый сервер. Его можно создать в любом регионе, где доступна службы "База данных Azure для PostgreSQL".  

Чтобы создать сервер с помощью геоизбыточного резервного копирования, выполните команду Azure CLI `az postgres server georestore`.

> [!NOTE]
> Сразу после создания сервер может быть еще не готов к географическому восстановлению. Заполнение метаданных может занять несколько часов.
>

Для геовосстановления сервера в командной строке Azure CLI введите следующую команду.

```azurecli-interactive
az postgres server georestore --resource-group myresourcegroup --name mydemoserver-georestored --source-server mydemoserver --location eastus --sku-name GP_Gen4_8 
```
С помощью этой команды можно создать сервер *mydemoserver-georestored* в восточной части США, который будет принадлежать группе ресурсов *myresourcegroup*. Это сервер общего назначения четвертого поколения с 8 виртуальными ядрами. Он создан на основе геоизбыточных резервных копий *mydemoserver*, которые также расположены в группе ресурсов *myresourcegroup*.

Если необходимо создать сервер в другой группе ресурсов с имеющегося сервера, в параметре `--source-server` необходимо определить имя сервера следующим образом.

```azurecli-interactive
az postgres server georestore --resource-group newresourcegroup --name mydemoserver-georestored --source-server "/subscriptions/$<subscription ID>/resourceGroups/$<resource group ID>/providers/Microsoft.DBforPostgreSQL/servers/mydemoserver" --location eastus --sku-name GP_Gen4_8

```

Для команды `az postgres server georestore` обязательны указанные ниже параметры.

| Параметр | Предлагаемое значение | Описание  |
| --- | --- | --- |
|resource-group| myresourcegroup | Имя группы ресурсов, к которой будет принадлежать новый сервер.|
|name | mydemoserver-georestored | Имя нового сервера. |
|source-server | mydemoserver | Имя имеющегося сервера, геоизбыточные резервные копии которого используются. |
|location | eastus | Расположение нового сервера. |
|sku-name| GP_Gen4_8 | Этот параметр определяет ценовую категорию, поколение вычислительных ресурсов и количество виртуальных ядер нового сервера. GP_Gen4_8 сопоставляется с сервером общего назначения четвертого поколения с 8 виртуальными ядрами.|


>[!Important]
>При создании сервера с помощью геовосстановления он наследует тот же размер хранилища и ценовую категорию, что и исходный сервер. Эти значения нельзя изменить во время создания. После создания сервера размер хранилища можно увеличить.

После завершения восстановления найдите новый сервер, чтобы убедиться, что данные были восстановлены, как и ожидалось.

На новом сервере, созданном во время восстановления, нет правил брандмауэра, которые были настроены на сервере-источнике. Для этого нового сервера правила брандмауэра нужно настроить отдельно.

## <a name="next-steps"></a>Дальнейшие действия
- Подробнее о [резервном копировании](concepts-backup.md) в службе.
- Подробнее о вариантах обеспечения [непрерывности бизнеса](concepts-business-continuity.md).
