---
title: Управление псевдонимом DNS для базы данных SQL Azure с помощью PowerShell | Документация Майкрософт
description: Командлеты PowerShell, таких как New-AzSqlServerDNSAlias позволяют перенаправлять новые клиентские подключения на другой сервер базы данных SQL Azure без необходимости конфигурацию клиента.
keywords: dns sql database
services: sql-database
ms.service: sql-database
ms.subservice: operations
ms.devlang: PowerShell
ms.topic: conceptual
author: oslake
ms.author: moslake
ms.reviewer: genemi,amagarwa,maboja, jrasnick
manager: craigg
ms.date: 05/14/2019
ms.openlocfilehash: 4318e6557dc72dff7200beb8783575131659b77f
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "65797706"
---
# <a name="powershell-for-dns-alias-to-azure-sql-database"></a>Управление псевдонимом DNS для Базы данных SQL Azure с помощью PowerShell

В этой статье представлен скрипт PowerShell, на примере которого показано, как управлять псевдонимом DNS для Базы данных SQL Azure. Скрипт выполняет приведенные ниже командлеты, которые реализуют следующие действия.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

В примере кода используются следующие командлеты:

- [New-AzSqlServerDNSAlias](https://docs.microsoft.com/powershell/module/az.Sql/New-azSqlServerDnsAlias): Создает псевдоним DNS в системе службы "База данных SQL Azure". Псевдоним ссылается на сервер 1 Базы данных SQL Azure.
- [Get-AzSqlServerDNSAlias](https://docs.microsoft.com/powershell/module/az.Sql/Get-azSqlServerDnsAlias): Получает и выводит все псевдонимы DNS, назначенные серверу 1 базы данных SQL.
- [Set-AzSqlServerDNSAlias](https://docs.microsoft.com/powershell/module/az.Sql/Set-azSqlServerDnsAlias): Изменяет имя сервера базы данных SQL, на который ссылается псевдоним, с "Сервер 1" на "Сервер 2".
- [Remove-AzSqlServerDNSAlias](https://docs.microsoft.com/powershell/module/az.Sql/Remove-azSqlServerDnsAlias): С помощью имени псевдонима удаляет псевдоним DNS с сервера 2 базы данных SQL.

## <a name="dns-alias-in-connection-string"></a>Использование псевдонима DNS в строке подключения

Чтобы подключить определенный сервер Базы данных SQL Azure, клиент (например, SQL Server Management Studio) может указать псевдоним DNS вместо действительного имени сервера. В следующем примере псевдоним *any-unique-alias-name* заменяет имя первого узла с разделителями-точками в строке сервера с четырьмя узлами.

- Пример строки сервера: `any-unique-alias-name.database.windows.net`.

## <a name="prerequisites"></a>Технические условия

Чтобы выполнить пример скрипта PowerShell, приведенный в этой статье, потребуются следующие компоненты:

- Подписка и учетная запись Azure. Для получения бесплатной пробной версии, щелкните ссылку [https://azure.microsoft.com/free/][https://azure.microsoft.com/free/].
- Модуль Azure PowerShell, с помощью командлета **New AzSqlServerDNSAlias**.
  - Чтобы выполнить установку или обновление, см. статью [Установка и настройка Azure PowerShell][install-Az-ps-84p].
  - Выполните `Get-Module -ListAvailable Az;` в файле powershell\_ise.exe, чтобы узнать версию.
- Два сервера Базы данных SQL Azure.

## <a name="code-example"></a>Примеры кода

В начале следующего примера кода PowerShell литеральные значения присваиваются нескольким переменным. Чтобы выполнить код, сначала необходимо изменить все значения заполнителей на реальные значения в системе. Или же можно просто изучить код. В примере также указаны выходные данные кода.

```powershell
################################################################
###    Assign prerequisites.                                 ###
################################################################
cls;

$SubscriptionName             = '<EDIT-your-subscription-name>';
[string]$SubscriptionGuid_Get = '?'; # The script assigns this value, not you.

$SqlServerDnsAliasName = '<EDIT-any-unique-alias-name>';

$1ResourceGroupName = '<EDIT-rg-1>';  # Can be same or different than $2ResourceGroupName.
$1SqlServerName     = '<EDIT-sql-1>'; # Must differ from $2SqlServerName.

$2ResourceGroupName = '<EDIT-rg-2>';
$2SqlServerName     = '<EDIT-sql-2>';

# Login to your Azure subscription, first time per session.
Write-Host "You must log into Azure once per powershell_ise.exe session,";
Write-Host "  thus type 'yes' only the first time.";
Write-Host " ";
$yesno = Read-Host '[yes/no]  Do you need to log into Azure now?';
if ('yes' -eq $yesno)
{
    Connect-AzAccount -SubscriptionName $SubscriptionName;
}

$SubscriptionGuid_Get = Get-AzSubscription `
    -SubscriptionName $SubscriptionName;

################################################################
###    Working with DNS aliasing for Azure SQL DB server.    ###
################################################################

Write-Host '[1] Assign a DNS alias to SQL DB server 1.';
New-AzSqlServerDNSAlias `
    –ResourceGroupName  $1ResourceGroupName `
    -ServerName         $1SqlServerName `
    -ServerDNSAliasName $SqlServerDnsAliasName;

Write-Host '[2] Get and list all the DNS aliases that are assigned to SQL DB server 1.';
Get-AzSqlServerDNSAlias `
    –ResourceGroupName $1ResourceGroupName `
    -ServerName        $1SqlServerName;

Write-Host '[3] Move the DNS alias from 1 to SQL DB server 2.';
Set-AzSqlServerDNSAlias `
    –ResourceGroupName  $2ResourceGroupName `
    -NewServerName      $2SqlServerName `
    -ServerDNSAliasName $SqlServerDnsAliasName `
    -OldServerResourceGroup  $1ResourceGroupName `
    -OldServerName           $1SqlServerName `
    -OldServerSubscriptionId $SubscriptionGuid_Get;

# Here your client, such as SSMS, can connect to your "$2SqlServerName"
# by using "$SqlServerDnsAliasName" in the server name.
# For example, server:  "any-unique-alias-name.database.windows.net".

# Remove-AzSqlServerDNSAlias  - would fail here for SQL DB server 1.

Write-Host '[4] Remove the DNS alias from SQL DB server 2.';
Remove-AzSqlServerDNSAlias `
    –ResourceGroupName  $2ResourceGroupName `
    -ServerName         $2SqlServerName `
    -ServerDNSAliasName $SqlServerDnsAliasName;
```

### <a name="actual-console-output-from-the-powershell-example"></a>Фактические выходные данные консоли из примера PowerShell

Следующие выходные данные консоли скопированы и вставлены из фактического выполнения.

```powershell
You must log into Azure once per powershell_ise.exe session,
  thus type 'yes' only the first time.

[yes/no]  Do you need to log into Azure now?: yes


Environment           : AzureCloud
Account               : gm@acorporation.com
TenantId              : 72f988bf-1111-1111-1111-111111111111
SubscriptionId        : 45651c69-2222-2222-2222-222222222222
SubscriptionName      : mysubscriptionname
CurrentStorageAccount :

[1] Assign a DNS alias to SQL DB server 1.
[2] Get the DNS alias that is assigned to SQL DB server 1.
[3] Move the DNS alias from 1 to SQL DB server 2.
[4] Remove the DNS alias from SQL DB server 2.
ResourceGroupName ServerName         ServerDNSAliasName
----------------- ----------         ------------------
gm-rg-dns-1       gm-sqldb-dns-1     unique-alias-name-food
gm-rg-dns-1       gm-sqldb-dns-1     unique-alias-name-food
gm-rg-dns-2       gm-sqldb-dns-2     unique-alias-name-food


[C:\windows\system32\]
>>
```

## <a name="next-steps"></a>Дальнейшие действия

Полное описание функции псевдонима DNS для Базы данных SQL см. в статье [Псевдоним DNS для базы данных SQL Azure][dns-alias-overview-37v].

<!-- Article links. -->

[https://azure.microsoft.com/free/]: https://azure.microsoft.com/free/

[install-Az-ps-84p]: https://docs.microsoft.com/powershell/azure/install-az-ps

[dns-alias-overview-37v]: dns-alias-overview.md
