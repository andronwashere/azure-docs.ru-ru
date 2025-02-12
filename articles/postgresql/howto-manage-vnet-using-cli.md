---
title: Создание и управление ими конечных точек службы виртуальной сети и правил для базы данных Azure для PostgreSQL - отдельного сервера с помощью интерфейса командной строки Azure
description: В этой статье описывается, как создавать и управлять ими конечных точек службы виртуальной сети и правил для базы данных Azure для PostgreSQL с помощью командной строки Azure.
author: bolzmj
ms.author: mbolz
ms.service: postgresql
ms.devlang: azurecli
ms.topic: conceptual
ms.date: 5/6/2019
ms.openlocfilehash: df2af0240852b23203e504d8233de4af48475438
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "65067568"
---
# <a name="create-and-manage-vnet-service-endpoints-for-azure-database-for-postgresql---single-server-using-azure-cli"></a>Создание и управление ими конечных точек службы виртуальной сети для базы данных Azure для PostgreSQL - отдельного сервера с помощью интерфейса командной строки Azure
Правила и конечные точки служб виртуальной сети расширяют частное адресное пространство виртуальной сети на сервер базы данных Azure для PostgreSQL. С помощью удобных команд интерфейса командной строки Azure (CLI) можно создавать, обновлять, удалять, выводить списки и просматривать правила и конечные точки службы виртуальной сети для управления сервером. Общие сведения о конечных точках службы виртуальной сети базы данных Azure для PostgreSQL, включая ограничения, см. в [этой статье](concepts-data-access-and-security-vnet.md). Конечные точки службы виртуальной сети доступны во всех поддерживаемых регионах базы данных Azure для PostgreSQL.

## <a name="prerequisites"></a>Технические условия
Прежде чем приступить к выполнению этого руководства, необходимы следующие компоненты:
- Установите [Azure CLI](/cli/azure/install-azure-cli) или используйте Azure Cloud Shell в браузере.
- [сервер и база данных Azure для PostgreSQL](quickstart-create-server-database-azure-cli.md);

> [!NOTE]
> Поддержка конечных точек службы виртуальной сети предназначена только для серверов общего назначения и серверов, оптимизированных для операций в памяти.
> В случае пиринга между виртуальными сетями, если трафик проходит через общий шлюз виртуальной сети с конечными точками и должен попадать в кэширующий узел, создайте правило ACL или виртуальной сети, чтобы разрешить виртуальным машинам Azure в шлюзе виртуальной сети доступ к Базе данных Azure для сервера PostgreSQL.

## <a name="configure-vnet-service-endpoints-for-azure-database-for-postgresql"></a>Настройка конечных точек службы виртуальной сети для базы данных Azure для PostgreSQL
Для настройки виртуальных сетей используется команда [az network vnet](https://docs.microsoft.com/cli/azure/network/vnet?view=azure-cli-latest).

Если у вас еще нет подписки Azure, создайте [бесплатную](https://azure.microsoft.com/free/) учетную запись Azure, прежде чем начинать работу.

[!INCLUDE [cloud-shell-try-it](../../includes/cloud-shell-try-it.md)]

Если вы решили установить и использовать интерфейс командной строки локально, для работы с этой статьей вам понадобится Azure CLI 2.0 или более поздней версии. Чтобы узнать, какая установлена версия, выполните команду `az --version`. Если вам необходимо выполнить установку или обновление, см. статью [Установка Azure CLI 2.0](/cli/azure/install-azure-cli). 

Если интерфейс командной строки выполняется локально, необходимо войти учетную запись, выполнив команду [az login](https://docs.microsoft.com/cli/azure/authenticate-azure-cli?view=azure-cli-latest). Запишите свойство **id** из выходных данных команды для соответствующего имени подписки.
```azurecli-interactive
az login
```

Если вы используете несколько подписок, выберите соответствующую, в которой за ресурс будет взиматься плата. Выберите конкретный идентификатор подписки вашей учетной записи, выполнив команду [az account set](https://docs.microsoft.com/cli/azure/account?view=azure-cli-latest#az-account-set). Подставьте свойство **id** из выходных данных **az login** для вашей подписки в заполнитель для идентификатора подписки.

- Учетная запись должна предоставлять необходимые разрешения для создания виртуальной сети и конечной точки службы.

Пользователь с правами на запись в виртуальной сети может настроить конечные точки служб в виртуальных сетях независимо друг от друга.

Для защиты ресурсов служб Azure в виртуальной сети пользователь должен иметь разрешение "Microsoft.Network/virtualNetworks/subnets/joinViaServiceEndpoint/" для добавляемых подсетей. Это разрешение по умолчанию включено во встроенные роли администраторов служб и может быть изменено при создании настраиваемых ролей.

Узнайте больше о [встроенных ролях](https://docs.microsoft.com/azure/active-directory/role-based-access-built-in-roles) и назначении разрешений, определенных для [настраиваемых ролей](https://docs.microsoft.com/azure/active-directory/role-based-access-control-custom-roles).

Виртуальные сети и ресурсы служб Azure могут находиться в одной или разных подписках. Если виртуальные сети и ресурсы служб Azure находятся в разных подписках, ресурсы должны быть размещены в одном клиенте Active Directory (AD).

> [!IMPORTANT]
> Прежде чем выполнять сценарий ниже и настраивать конечные точки службы, советуем прочитать эту статью о конфигурациях конечной точки службы и рекомендациях: **Конечная точка службы виртуальной сети.** [Конечная точка службы для виртуальной сети](../virtual-network/virtual-network-service-endpoints-overview.md) — это подсеть, значения свойств которой включают в себя одно формальное имя типа службы Azure или несколько. Конечные точки службы виртуальной сети используют имя типа службы **Microsoft.Sql**, которое относится к службе Azure, которая называется "База данных SQL". Этот тег службы также применяется к службам "База данных SQL Azure", "База данных Azure для PostgreSQL" и "База данных Azure для MySQL". Важно отметить, что при применении тега службы **Microsoft.Sql** к конечной точке службы виртуальной сети она настроит трафик конечной точки службы для всех служб базы данных Azure, в том числе служб "База данных SQL Azure", "База данных Azure для PostgreSQL" и "База данных Azure для MySQL" в подсети. 
> 

### <a name="sample-script-to-create-an-azure-database-for-postgresql-database-create-a-vnet-vnet-service-endpoint-and-secure-the-server-to-the-subnet-with-a-vnet-rule"></a>Пример сценария для создания базы данных службы "База данных Azure для PostgreSQL", виртуальной сети, конечной точки службы виртуальной сети и обеспечения безопасности передачи данных с сервера в подсеть с помощью правила виртуальной сети
В этом примере скрипта измените выделенные строки, чтобы настроить имя и пароль администратора. Замените идентификатор подписки, используемый в команде `az account set --subscription`, собственным.
[!code-azurecli-interactive[main](../../cli_scripts/postgresql/create-postgresql-server-vnet/create-postgresql-server.sh?highlight=5,20 "Create an Azure Database for PostgreSQL, VNet, VNet service endpoint, and VNet rule.")]

## <a name="clean-up-deployment"></a>Очистка развертывания
После выполнения примера сценария можно удалить группу ресурсов и все связанные с ней ресурсы, выполнив следующую команду.
[!code-azurecli-interactive[main](../../cli_scripts/postgresql/create-postgresql-server-vnet/delete-postgresql.sh "Delete the resource group.")]
