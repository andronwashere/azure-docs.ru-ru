---
title: Создание правил брандмауэра Базы данных Azure для MariaDB и управление ими с помощью Azure CLI
description: В этой статье описывается, как создать правила брандмауэра Базы данных Azure для MariaDB и управлять ими с помощью интерфейса командной строки Azure.
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.devlang: azurecli
ms.topic: conceptual
ms.date: 04/09/2019
ms.openlocfilehash: 562987b953f0a8a20a917e208f43557bd768c0a0
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "61038626"
---
# <a name="create-and-manage-azure-database-for-mariadb-firewall-rules-by-using-the-azure-cli"></a>Создание правил брандмауэра Базы данных Azure для MariaDB и управление ими с помощью Azure CLI
Правила брандмауэра уровня сервера можно использовать для управления доступом к базе данных Azure для сервера MariaDB с определенного IP-адреса или диапазона IP-адресов. С помощью удобных команд Azure CLI можно создавать, обновлять, удалять, выводить список и отображать правила брандмауэра для управления сервером. Общие сведения о базе данных Azure для MariaDB брандмауэров, см. в разделе [база данных Azure для правил брандмауэра сервера MariaDB](./concepts-firewall-rules.md).

Правилами виртуальных сетей (VNet) также может использоваться для обеспечения безопасного доступа к серверу. Дополнительные сведения о [Создание и управление виртуальной сетью на базе службы конечных точек и правил, с помощью Azure CLI](howto-manage-vnet-cli.md).

## <a name="prerequisites"></a>Технические условия
* [Установка Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli).
* [Сервер Базы данных Azure для MariaDB и база данных](quickstart-create-mariadb-server-database-using-azure-cli.md).

## <a name="firewall-rule-commands"></a>Команды для правил брандмауэра:
Команда **az mariadb server firewall-rule** используется в Azure CLI для создания, удаления, вывода списка, отображения и обновления правил брандмауэра.

Команды:
- **create**: Создание правила брандмауэра сервера Azure MariaDB.
- **delete**: Удаление правила брандмауэра сервера Azure MariaDB.
- **list**: Вывод списка правил брандмауэра сервера Azure MariaDB.
- **show**: Отображение сведений о правиле брандмауэра сервера Azure MariaDB.
- **update**: Обновление правила брандмауэра сервера Azure MariaDB.

## <a name="sign-in-to-azure-and-list-your-azure-database-for-mariadb-servers"></a>Войдите в Azure и вывод списка базы данных Azure для MariaDB серверов
Безопасно подключитесь к Azure CLI с помощью своей учетной записи Azure, используя команду **az login**.

1. В командной строке выполните следующую команду.
   ```azurecli
   az login
   ```
   Эта команда выведет код, который понадобится на следующем шаге.

2. Откройте браузер, перейдите на страницу [https://aka.ms/devicelogin](https://aka.ms/devicelogin), а затем введите код.

3. В командной строке вход с использованием учетных данных Azure.

4. После авторизации имени для входа в консоли отобразится список подписок. Скопируйте идентификатор необходимой подписки, чтобы выбрать текущую подписку для использования. Введите команду [az account set](/cli/azure/account#az-account-set).
   ```azurecli-interactive
   az account set --subscription <your subscription id>
   ```

5. Выведите список серверов Базы данных Azure для MariaDB для своей подписки и группы ресурсов, если вы не уверены, что знаете нужные имена. Выполните команду [az mariadb server list](/cli/azure/mariadb/server#az-mariadb-server-list).

   ```azurecli-interactive
   az mariadb server list --resource-group myresourcegroup
   ```

   Обратите внимание на атрибут имени в списке, который потребуется для указания сервера MariaDB для дальнейшей работы. При необходимости подтвердите данные этого сервера с помощью атрибута name, чтобы гарантировать, что имя указано правильно. Выполните команду [az mariadb server show](/cli/azure/mariadb/server#az-mariadb-server-show).

   ```azurecli-interactive
   az mariadb server show --resource-group myresourcegroup --name mydemoserver
   ```

## <a name="list-firewall-rules-on-azure-database-for-mariadb-server"></a>Вывод списка правил брандмауэра для сервера Базы данных Azure для MariaDB 
Используя имя сервера и имя группы ресурсов, выведите список существующих правил брандмауэра на сервере. Выполните команду [az mariadb server firewall list](/cli/azure/mariadb/server/firewall-rule#az-mariadb-server-firewall-rule-list).  Обратите внимание, что атрибут имени сервера указан в параметре **--server**, а не в параметре **--name**. 
```azurecli-interactive
az mariadb server firewall-rule list --resource-group myresourcegroup --server-name mydemoserver
```
Выходные данные будут содержать правила, если они имеются, в формате JSON (по умолчанию). Параметр **--output table** позволяет вывести результаты в более удобном для чтения табличном формате.
```azurecli-interactive
az mariadb server firewall-rule list --resource-group myresourcegroup --server-name mydemoserver --output table
```
## <a name="create-a-firewall-rule-on-azure-database-for-mariadb-server"></a>Создание правила брандмауэра для сервера Базы данных Azure для MariaDB
Используя имя сервера Azure MariaDB и имя группы ресурсов, создайте правило брандмауэра на сервере. Выполните команду [az mariadb server firewall create](/cli/azure/mariadb/server/firewall-rule#az-mariadb-server-firewall-rule-create). Введите имя правила, а также начальный IP-адрес и конечный IP-адрес (чтобы предоставить доступ к диапазону IP-адресов) для правила.
```azurecli-interactive
az mariadb server firewall-rule create --resource-group myresourcegroup --server-name mydemoserver --name FirewallRule1 --start-ip-address 13.83.152.0 --end-ip-address 13.83.152.15
```

Чтобы разрешить доступ с отдельного IP-адреса, укажите один и тот же начальный и конечный IP-адрес, как показано в этом примере.
```azurecli-interactive
az mariadb server firewall-rule create --resource-group myresourcegroup --server-name mydemoserver --name FirewallRule1 --start-ip-address 1.1.1.1 --end-ip-address 1.1.1.1
```

Чтобы разрешить приложениям подключаться с IP-адресов Azure к серверу Базы данных Azure для MariaDB, укажите в качестве начального и конечного IP-адресов 0.0.0.0, как показано в примере.
```azurecli-interactive
az mariadb server firewall-rule create --resource-group myresourcegroup --server mariadb --name "AllowAllWindowsAzureIps" --start-ip-address 0.0.0.0 --end-ip-address 0.0.0.0
```

> [!IMPORTANT]
> Этот параметр позволяет настроить брандмауэр так, чтобы разрешить все подключения из Azure, включая подключения из подписок других клиентов. При выборе этого параметра убедитесь, что используемое имя для входа и разрешения пользователя предоставляют доступ только авторизованным пользователям.
> 

При успешном выполнении выходные данные каждой команды create будут содержать сведения о созданном правиле брандмауэра в формате JSON (по умолчанию). Если возникнет сбой, выходные данные будут содержать вместо этого текст сообщения об ошибке.

## <a name="update-a-firewall-rule-on-azure-database-for-mariadb-server"></a>Обновление правила брандмауэра для сервера Базы данных Azure для MariaDB 
Используя имя сервера Azure MariaDB и имя группы ресурсов, измените правило брандмауэра на сервере. Выполните команду [az mariadb server firewall update](/cli/azure/mariadb/server/firewall-rule#az-mariadb-server-firewall-rule-update). В качестве входных данных укажите имя существующего правила брандмауэра, а также атрибуты начального и конечного IP-адресов, которые нужно обновить.
```azurecli-interactive
az mariadb server firewall-rule update --resource-group myresourcegroup --server-name mydemoserver --name FirewallRule1 --start-ip-address 13.83.152.0 --end-ip-address 13.83.152.1
```
При успешном выполнении команды ее выходные данные будут содержать сведения об обновленном правиле брандмауэра в формате JSON (по умолчанию). Если возникнет сбой, выходные данные будут содержать вместо этого текст сообщения об ошибке.

> [!NOTE]
> Если правило брандмауэра не существует, оно будет создано командой update.

## <a name="show-firewall-rule-details-on-azure-database-for-mariadb-server"></a>Отображение сведений о правиле брандмауэра для сервера Базы данных Azure для MariaDB
Используя имя сервера Azure MariaDB и имя группы ресурсов, отобразите сведения о правиле брандмауэра на сервере. Выполните команду [az mariadb server firewall show](/cli/azure/mariadb/server/firewall-rule#az-mariadb-server-firewall-rule-show) Укажите имя существующего правила брандмауэра в качестве входных данных.
```azurecli-interactive
az mariadb server firewall-rule show --resource-group myresourcegroup --server-name mydemoserver --name FirewallRule1
```
При успешном выполнении команды ее выходные данные будут содержать сведения об указанном правиле брандмауэра в формате JSON (по умолчанию). Если возникнет сбой, выходные данные будут содержать вместо этого текст сообщения об ошибке.

## <a name="delete-a-firewall-rule-on-azure-database-for-mariadb-server"></a>Удаление правила брандмауэра для сервера Базы данных Azure для MariaDB
Используя имя сервера Azure MariaDB и имя группы ресурсов, удалите правило брандмауэра с сервера. Выполните команду [az mariadb server firewall delete](/cli/azure/mariadb/server/firewall-rule#az-mariadb-server-firewall-rule-delete). Укажите имя существующего правила брандмауэра.
```azurecli-interactive
az mariadb server firewall-rule delete --resource-group myresourcegroup --server-name mydemoserver --name FirewallRule1
```
При успешном выполнении выходные данные отсутствуют. В случае сбоя отображается текст сообщения об ошибке.

## <a name="next-steps"></a>Дальнейшие действия
- Дополнительные сведения о [правилах брандмауэра сервера Базы данных Azure для MariaDB](./concepts-firewall-rules.md).
- [Создание правил брандмауэра MariaDB в Базе данных Azure для MariaDB и управление ими на портале Azure](./howto-manage-firewall-portal.md)
- Дополнительно обеспечить безопасность доступа к серверу с [Создание и управление виртуальной сетью на базе службы конечных точек и правил, с помощью Azure CLI](howto-manage-vnet-cli.md).
