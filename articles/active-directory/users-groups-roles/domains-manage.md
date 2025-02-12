---
title: Добавление и проверка пользовательских доменных имен в Azure Active Directory | Документация Майкрософт
description: Основные принципы и указания по управлению доменным именем в Azure Active Directory.
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.subservice: users-groups-roles
ms.topic: article
ms.date: 01/31/2019
ms.author: curtand
ms.reviewer: elkuzmen
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 908ae768ae471ab6f49452c99323c31d34772d45
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "60472337"
---
# <a name="managing-custom-domain-names-in-your-azure-active-directory"></a>Управление личными доменными именами в Azure Active Directory

Доменное имя является важной частью идентификатора для различных ресурсов каталога: имени пользователя, адреса электронной почты пользователя и адреса группы. Оно также может входить в URI идентификатора приложения. Для использования в ресурсе Azure Active Directory (Azure AD) доменное имя должно принадлежать каталогу, содержащему ресурс. Только глобальный администратор может управлять доменами в Azure AD.

## <a name="set-the-primary-domain-name-for-your-azure-ad-directory"></a>Установка основного доменного имени для каталога Azure AD

При создании каталога исходное доменное имя, например contoso.onmicrosoft.com, также становится основным доменным именем Основной домен задает доменное имя по умолчанию, применяемое для каждого создаваемого пользователя. При установке основного доменного имени упрощается процесс создания пользователей на портале для администратора. Чтобы изменить основное доменное имя, выполните следующее.

1. Войдите на [портал Azure](https://portal.azure.com) с помощью учетной записи глобального администратора каталога.
2. Выберите **Azure Active Directory**.
3. Выберите **Имена пользовательских доменов**.
  
   ![Открыв страницу управления пользователями](./media/domains-manage/add-custom-domain.png)
4. Выберите имя домена, который будет основным.
5. Выберите команду **Назначить основным**. При появлении запроса подтвердите свой выбор.
  
   ![Сделать имя основного домена](./media/domains-manage/make-primary-domain.png)

В качестве основного домена для каталога можно указать любой проверенный личный домен, который не является федеративным. При изменении основного домена для каталога имена существующих пользователей не изменятся.

## <a name="add-custom-domain-names-to-your-azure-ad-tenant"></a>Добавление личных доменных имен в клиент Azure AD

Можно добавить до 900 имен управляемых доменов. Если вы настраиваете все домены для федерации с локальной службой Active Directory, в каждый каталог можно добавить до 450 доменных имен.

## <a name="add-subdomains-of-a-custom-domain"></a>Добавление поддоменов для личного домена

Если вы хотите добавить в ваш каталог доменное имя третьего уровня, например "europe.contoso.com", необходимо сначала добавить и проверить домен второго уровня, например "contoso.com". Azure AD автоматически проверяет поддомен. Чтобы убедиться, что добавленный поддомен проверен, обновите список доменов в браузере.

## <a name="what-to-do-if-you-change-the-dns-registrar-for-your-custom-domain-name"></a>Что делать, если вы изменили регистратор DNS для пользовательского доменного имени

Если вы измените регистраторы DNS, в Azure AD нет дополнительных задач настройки. Вы можете продолжать использовать доменное имя с Azure AD без прерывания. Если личное доменное имя используется в Office 365, Intune или других службах, которым необходимы личные домены в Azure AD, обратитесь к документации для этих служб.

## <a name="delete-a-custom-domain-name"></a>Удаление имени личного домена

Имя личного домена можно удалить из Azure AD, если ваша организация не использует его или если это доменное имя необходимо использовать с другой службой Azure AD.

Перед удалением личного доменного имени необходимо убедиться, что от этого имени не зависят никакие ресурсы в каталоге. Доменное имя невозможно удалить из каталога в следующих случаях:

* Какой-либо пользователь имеет имя пользователя, электронный адрес или адрес прокси-сервера, которые включают это доменное имя.
* Какая-либо группа имеет адрес электронной почты или адрес прокси-сервера, которые включают это доменное имя.
* Какое-либо приложение в Azure AD имеет URI идентификатора приложения, который включает это доменное имя.

Перед удалением личного доменного имени необходимо изменить или удалить каждый такой ресурс в каталоге Azure AD.

### <a name="forcedelete-option"></a>Параметр ForceDelete

Вы можете применить операцию принудительного удаления (**ForceDelete**) к доменному имени в [Центре администрирования Azure AD](https://aad.portal.azure.com) или с помощью [API Microsoft Graph](https://docs.microsoft.com/graph/api/domain-forcedelete?view=graph-rest-beta). В обоих случаях используется асинхронная операция и все ссылки обновляются с имени личного домена, например user@contoso.com, до начального имени домена по умолчанию, например user@contoso.onmicrosoft.com. 

Чтобы вызвать **ForceDelete** на портале Azure, вы должны убедиться, что существует менее 1000 ссылок на доменное имя, а любые ссылки, в которых Exchange является службой подготовки, должны быть обновлены или удалены в [Центре администрирования Exchange](https://outlook.office365.com/ecp/). Сюда входят группы безопасности с поддержкой почты и распределенные списки Exchange. Дополнительные сведения см. в статье [Manage mail-enabled security groups in Exchange Server](https://technet.microsoft.com/library/bb123521(v=exchg.160).aspx#Remove%20mail-enabled%20security%20groups) (Управление группами безопасности с поддержкой почты в Exchange Server). Кроме того, операция **ForceDelete** не будет выполнена, если выполняется любое из следующих условий:

* Вы приобрели домен через службы подписки домена Office 365.
* Вы являетесь партнером, управляющим от имени другого клиента пользователя.

Следующие действия выполняются как часть операции **ForceDelete**:

* Имя участника-пользователя (UPN), электронный адрес (EmailAddress) и адрес прокси-сервера (ProxyAddress) пользователей со ссылками на имя личного домена переименовываются на основе начального имени домена по умолчанию.
* Электронный адрес (EmailAddress) групп со ссылками на имя личного домена переименовывается на основе начального имени домена по умолчанию.
* Идентификаторы URI (identifierUris) приложений со ссылками на имя личного домена переименовываются на основе начального имени домена по умолчанию.

Возвращается сообщение об ошибке, если:

* Количество объектов, подлежащих переименованию, превышает 1000.
* Одно из приложений, которое нужно переименовать, является мультитенантным.

### <a name="frequently-asked-questions"></a>Часто задаваемые вопросы

**Вопрос. Удаление домена завершается ошибкой, указывающей, что у меня есть группы Exchange, которые были зарегистрированы в этом доменном имени. Почему?** <br>
**Ответ.** Сейчас определенные группы, такие как группы безопасности с поддержкой почты и распределенные списки, предоставляются Exchange, и их необходимо вручную удалить в [Центре администрирования Exchange](https://outlook.office365.com/ecp/). Могут существовать устаревшие адреса прокси-серверов, которые ссылаются на имя личного домена. Их необходимо обновить вручную для использования другого доменного имени. 

**Вопрос. Я вошел качестве администратора\@contoso.com, но мне не удается удалить имя домена «contoso.com»?**<br>
**Ответ.** Вы не можете ссылаться на имя личного домена, которое вы пытаетесь удалить, в своем имени учетной записи пользователя. Убедитесь, что учетная запись глобального администратора использует имя домена по умолчанию (.onmicrosoft.com), например admin@contoso.onmicrosoft.com. Войдите в систему с другой учетной записью глобального администратора, например admin@contoso.onmicrosoft.com, или другим именем личного домена, например fabrikam.com, где учетная запись — admin@fabrikam.com.

**Вопрос. Я нажимаю кнопку "Удалить домен" и вижу состояние `In Progress` для операции удаления. Сколько времени это занимает? Что произойдет, если операцию не удастся выполнить?**<br>
**Ответ.** Операция удаления домена — это асинхронная фоновая задача, которая переименовывает все ссылки на доменное имя. Эта операция должна завершиться через пару минут. Если удаление домена не выполняется, убедитесь, что у вас нет:

* приложений с настроенным доменным именем в appIdentifierURI;
* любой группы с поддержкой почты, ссылающейся на имя личного домена;
* более 1000 ссылок на доменное имя.

Если вы обнаружите, что ни одно из условий не выполнено, вручную очистите ссылки и попробуйте снова удалить домен.

## <a name="use-powershell-or-graph-api-to-manage-domain-names"></a>Использование PowerShell или API Graph для управления доменными именами

Большинство задач по управлению доменными именами в Azure Active Directory также можно выполнить с помощью Microsoft PowerShell или программными средствами с помощью API Graph Azure AD.

* [Управление доменными именами в Azure AD с помощью PowerShell](https://msdn.microsoft.com/library/azure/e1ef403f-3347-4409-8f46-d72dafa116e0#BKMK_ManageDomains)
* [Управление именами доменов в Azure AD с помощью API Graph](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/domains-operations)

## <a name="next-steps"></a>Дальнейшие действия

* [Добавление имен личных доменов](/azure/active-directory/fundamentals/add-custom-domain?context=azure/active-directory/users-groups-roles/context/ugr-context)
* [Сведения об удалении групп безопасности с поддержкой почты Exchange в Центре администрирования Exchange для имени личного домена в Azure AD](https://technet.microsoft.com/library/bb123521(v=exchg.160).aspx#Remove%20mail-enabled%20security%20groups)
* [Раздел об удалении имени личного домена с помощью операции ForceDelete в API Microsoft Graph](https://docs.microsoft.com/graph/api/domain-forcedelete?view=graph-rest-beta)
