---
title: Доменные службы Azure Active Directory. Руководство по устранению неполадок | Документация Майкрософт
description: Руководство по устранению неполадок для доменных служб Azure AD
services: active-directory-ds
documentationcenter: ''
author: iainfoulds
manager: daveba
editor: curtand
ms.assetid: 4bc8c604-f57c-4f28-9dac-8b9164a0cf0b
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/22/2019
ms.author: iainfou
ms.openlocfilehash: 2df1ac6325f692e2d433238ae0b92d8e3f8482b5
ms.sourcegitcommit: f811238c0d732deb1f0892fe7a20a26c993bc4fc
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/29/2019
ms.locfileid: "67472285"
---
# <a name="azure-ad-domain-services---troubleshooting-guide"></a>Доменные службы Azure AD: руководство по устранению неполадок
В этой статье приводятся советы по устранению проблем, с которыми вы можете столкнуться при настройке или администрировании доменных служб Azure Active Directory (AD).

## <a name="you-cannot-enable-azure-ad-domain-services-for-your-azure-ad-directory"></a>Не удается включить доменные службы Azure AD для каталога Azure AD
Предоставленные в этом разделе сведения помогут устранить ошибки при попытке включения доменных служб Azure AD для каталога.

Выберите действия по устранению неполадок, соответствующие возникшему сообщению об ошибке.

| **Сообщение об ошибке** | **Способы устранения:** |
| --- |:--- |
| *Имя contoso100.com уже используется в этой сети. Введите неиспользуемое имя.* |[Конфликт доменных имен в виртуальной сети](troubleshoot.md#domain-name-conflict) |
| *Не удалось включить доменные службы в этом клиенте Azure AD. У службы нет необходимых разрешений на доступ к приложению с именем Azure AD Domain Services Sync. Удалите приложение с именем Azure AD Domain Services Sync, а затем попробуйте снова включить доменные службы для своего клиента Azure AD.* |[У доменных служб нет необходимых разрешений на доступ к приложению Azure AD Domain Services Sync](troubleshoot.md#inadequate-permissions) |
| *Не удалось включить доменные службы в этом клиенте Azure AD. У приложения доменных служб в клиенте Azure AD нет разрешений, необходимых для включения доменных служб. Удалите приложение с идентификатором d87dcbc6-a371-462e-88e3-28ad15ec4e64, а затем попробуйте снова включить доменные службы для своего клиента Azure AD.* |[Приложение доменных служб в клиенте настроено неправильно](troubleshoot.md#invalid-configuration) |
| *Не удалось включить доменные службы в этом клиенте Azure AD. Приложение Microsoft Azure AD отключено в клиенте Azure AD. Включите приложение с идентификатором 00000002-0000-0000-c000-000000000000, а затем попробуйте снова включить доменные службы для своего клиента Azure AD.* |[Приложение Microsoft Graph отключено в клиенте Azure AD](troubleshoot.md#microsoft-graph-disabled) |

### <a name="domain-name-conflict"></a>Конфликт доменных имен
**Сообщение об ошибке:**

*Имя contoso100.com уже используется в этой сети. Введите неиспользуемое имя.*

**Исправление:**

Убедитесь, что в этой виртуальной сети нет домена с таким же доменным именем. Например, предположим, что в выбранной виртуальной сети уже есть домен с именем contoso.com, Позже вы попытаетесь включить в ней управляемый домен доменных служб Azure AD с таким же доменным именем (contoso.com). В таком случае попытка включить доменные службы Azure AD приведет к ошибке.

Причина заключается в конфликте доменных имен в этой виртуальной сети. В такой ситуации для настройки управляемого домена доменных служб Azure AD необходимо использовать другое имя. Кроме того, можно отменить подготовку существующего домена и включить доменные службы Azure AD.

### <a name="inadequate-permissions"></a>Недостаточно разрешений
**Сообщение об ошибке:**

*Не удалось включить доменные службы в этом клиенте Azure AD. У службы нет необходимых разрешений на доступ к приложению с именем Azure AD Domain Services Sync. Удалите приложение с именем Azure AD Domain Services Sync, а затем попробуйте снова включить доменные службы для своего клиента Azure AD.*

**Исправление:**

Проверьте, есть ли в каталоге Azure AD приложение с именем Azure AD Domain Services Sync. Если такое приложение есть, удалите его, а затем повторно включите доменные службы Azure AD.

Выполните следующие действия, чтобы проверить наличие приложения и удалить его при необходимости.

1. На [портале Azure](https://portal.azure.com/#blade/Microsoft_AAD_IAM/StartboardApplicationsMenuBlade/AllApps/menuId/) перейдите в раздел **Приложения** в каталоге Azure AD.
2. Выберите **Все приложения** в раскрывающемся меню **Показать**. Выберите **Любое** в раскрывающемся меню **Состояние приложения**. Выберите **Любое** в раскрывающемся меню **Видимость приложений**.
3. В поле поиска введите **Синхронизация доменных служб Azure AD**. Если приложение есть, щелкните его, а затем нажмите кнопку **Удалить** на панели инструментов, чтобы удалить его.
4. Удалив приложение, попробуйте еще раз включить доменные службы Azure AD.

### <a name="invalid-configuration"></a>Недопустимая конфигурация
**Сообщение об ошибке:**

*Не удалось включить доменные службы в этом клиенте Azure AD. У приложения доменных служб в клиенте Azure AD нет разрешений, необходимых для включения доменных служб. Удалите приложение с идентификатором d87dcbc6-a371-462e-88e3-28ad15ec4e64, а затем попробуйте снова включить доменные службы для своего клиента Azure AD.*

**Исправление:**

Проверьте, есть ли в каталоге Azure AD приложение с именем AzureActiveDirectoryDomainControllerServices (и идентификатором d87dcbc6-a371-462e-88e3-28ad15ec4e64). Если такое приложение есть, его нужно удалить, а затем повторно включить доменные службы Azure AD.

Используйте следующий скрипт PowerShell, чтобы найти приложение и удалить его.

> [!NOTE]
> В этом скрипте используются командлеты **Azure AD PowerShell версии 2**. Полный список всех доступных командлетов и сведения по скачиванию модуля см. в [справочной документации по AzureAD PowerShell](https://msdn.microsoft.com/library/azure/mt757189.aspx).
>
>

```powershell
$InformationPreference = "Continue"
$WarningPreference = "Continue"

$aadDsSp = Get-AzureADServicePrincipal -Filter "AppId eq 'd87dcbc6-a371-462e-88e3-28ad15ec4e64'" -ErrorAction Ignore
if ($aadDsSp -ne $null)
{
    Write-Information "Found Azure AD Domain Services application. Deleting it ..."
    Remove-AzureADServicePrincipal -ObjectId $aadDsSp.ObjectId
    Write-Information "Deleted the Azure AD Domain Services application."
}

$identifierUri = "https://sync.aaddc.activedirectory.windowsazure.com"
$appFilter = "IdentifierUris eq '" + $identifierUri + "'"
$app = Get-AzureADApplication -Filter $appFilter
if ($app -ne $null)
{
    Write-Information "Found Azure AD Domain Services Sync application. Deleting it ..."
    Remove-AzureADApplication -ObjectId $app.ObjectId
    Write-Information "Deleted the Azure AD Domain Services Sync application."
}

$spFilter = "ServicePrincipalNames eq '" + $identifierUri + "'"
$sp = Get-AzureADServicePrincipal -Filter $spFilter
if ($sp -ne $null)
{
    Write-Information "Found Azure AD Domain Services Sync service principal. Deleting it ..."
    Remove-AzureADServicePrincipal -ObjectId $sp.ObjectId
    Write-Information "Deleted the Azure AD Domain Services Sync service principal."
}
```
<br>

### <a name="microsoft-graph-disabled"></a>Интерфейс Microsoft Graph отключен
**Сообщение об ошибке:**

Не удалось включить доменные службы в этом клиенте Azure AD. Приложение Microsoft Azure AD отключено в клиенте Azure AD. Включите приложение с идентификатором 00000002-0000-0000-c000-000000000000, а затем попробуйте снова включить доменные службы для своего клиента Azure AD.

**Исправление:**

Проверьте, отключено ли приложение с идентификатором 00000002-0000-0000-c000-000000000000. Это приложение Microsoft Azure AD, которое предоставляет доступ к API Graph вашему клиенту Azure AD. Для доменных служб Azure AD требуется, чтобы это приложение было включено. В противном случае они не смогут синхронизировать ваш клиент Azure AD с управляемым доменом.

Чтобы устранить эту ошибку, включите это приложение и попытайтесь включить доменные службы для своего клиента Azure AD.


## <a name="users-are-unable-to-sign-in-to-the-azure-ad-domain-services-managed-domain"></a>Пользователи не могут войти в домен, находящийся под управлением доменных служб Azure AD
Если один или несколько пользователей клиента Azure AD не могут войти в только что созданный управляемый домен, выполните следующие действия по устранению неполадок.

* **Вход с использованием формата имени участника-пользователя**. Попробуйте выполнить вход, используя имя участника-пользователя (например, joeuser@contoso.com) вместо имени для входа в формате SAMAccountName (CONTOSO\joeuser). Имя для входа в формате SAMAccountName может создаваться автоматически для пользователей, префикс имени участника-пользователя которых слишком длинный или совпадает с именем другого пользователя в управляемом домене. Формат имени участника-пользователя гарантирует уникальность пользователей в пределах клиента Azure AD.

> [!NOTE]
> Мы рекомендуем использовать этот формат для входа в управляемый домен доменных служб Azure AD.
>
>

* Убедитесь, что [включена синхронизация паролей](active-directory-ds-getting-started-password-sync.md) в соответствии с шагами, описанными на странице «Приступая к работе».
* **Внешние учетные записи**. Убедитесь, что затронутая учетная запись пользователя не является внешней учетной записью клиента Azure AD. К примерам внешних учетных записей относятся учетные записи Майкрософт (например, joe@live.com) или учетные записи из внешнего каталога Azure AD. Так как у доменных служб Azure AD нет учетных данных для этих учетных записей, эти пользователи не могут войти в управляемый домен.
* **Синхронизированные учетные записи**. Если учетные записи пользователей синхронизируются из локального каталога, убедитесь в следующем.

  * Вы развернули или установили [последнюю рекомендуемую версию Azure AD Connect](https://www.microsoft.com/download/details.aspx?id=47594).
  * Вы включили в Azure AD Connect [выполнение полной синхронизации](active-directory-ds-getting-started-password-sync.md).
  * В зависимости от размера каталога на доступность учетных записей пользователей и хэшей учетных данных в доменных службах Azure AD может потребоваться некоторое время. Подождите, прежде чем повторять проверку подлинности.
  * Если проблема повторится после выполнения описанных выше действий, попытайтесь перезапустить службу Microsoft Azure AD Sync. На компьютере синхронизации откройте командную строку и выполните следующие команды.

    1. net stop 'Microsoft Azure AD Sync'
    2. net start 'Microsoft Azure AD Sync'
* **Только облачные учетные записи**. Если учетная запись пользователя существует только в облаке, убедитесь, что пользователь изменил пароль после включения доменных служб Azure AD. Этот шаг вызывает создание сверток учетных данных, необходимых для доменных служб Azure AD.

## <a name="there-are-one-or-more-alerts-on-your-managed-domain"></a>Имеется одно или несколько оповещений для управляемого домена

Дополнительные сведения об устранении неполадок с оповещениями в управляемом домене см. в статье [Доменные службы Azure Active Directory. Устранение неполадок: предупреждения](troubleshoot-alerts.md).

## <a name="users-removed-from-your-azure-ad-tenant-are-not-removed-from-your-managed-domain"></a>Пользователи, удаленные из вашего клиента Azure AD, не удаляются из управляемого домена
Azure AD обеспечивает защиту от случайного удаления объектов-пользователей. При удалении учетной записи пользователя из вашего клиента Azure AD соответствующий объект-пользователь перемещается в корзину. При синхронизации этой операции удаления с управляемым доменом соответствующая учетная запись пользователя помечается как отключенная. Эта функция позволяет восстановить или отменить удаление учетной записи пользователя позже.

Учетная запись пользователя остается в управляемом домене в отключенном состоянии, даже если в каталоге Azure AD повторно создать учетную запись с тем же именем участника-пользователя. Чтобы удалить учетную запись из управляемого домена, принудительно удалите ее из своего клиента Azure AD.

Чтобы полностью удалить учетную запись пользователя из управляемого домена, навсегда удалите пользователя из своего клиента Azure AD. Используйте командлет PowerShell `Remove-MsolUser` с параметром `-RemoveFromRecycleBin`, как описано в этой [статье MSDN](/previous-versions/azure/dn194132(v=azure.100)).


## <a name="contact-us"></a>Свяжитесь с нами
Чтобы [оставить отзыв или обратиться за помощью](contact-us.md), свяжитесь с командой разработки продукта, отвечающей за доменные службы Azure Active Directory.
